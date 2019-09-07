---
title: Arbeiten mit JNI und xamarin. Android
description: Xamarin. Android ermöglicht das Schreiben von Android C# -apps mit anstelle von Java. Mit xamarin. Android werden mehrere Assemblys bereitgestellt, die Bindungen für Java-Bibliotheken bereitstellen, einschließlich Mono. Android. dll und Mono. Android. GoogleMaps. dll. Allerdings werden Bindungen nicht für jede mögliche Java-Bibliothek bereitgestellt, und die bereitgestellten Bindungen binden möglicherweise nicht alle Java-Typen und-Member. Um ungebundene Java-Typen und-Member zu verwenden, kann die Java Native Interface (JNI) verwendet werden. In diesem Artikel wird veranschaulicht, wie jni verwendet wird, um mit Java-Typen und-Membern von xamarin. Android-Anwendungen zu interagieren.
ms.prod: xamarin
ms.assetid: A417DEE9-7B7B-4E35-A79C-284739E3838E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 9b4ecae0ce37aeb9893a4cb5e55da951789be182
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70761437"
---
# <a name="working-with-jni-and-xamarinandroid"></a>Arbeiten mit JNI und xamarin. Android

_Xamarin. Android ermöglicht das Schreiben von Android C# -apps mit anstelle von Java. Mit xamarin. Android werden mehrere Assemblys bereitgestellt, die Bindungen für Java-Bibliotheken bereitstellen, einschließlich Mono. Android. dll und Mono. Android. GoogleMaps. dll. Allerdings werden Bindungen nicht für jede mögliche Java-Bibliothek bereitgestellt, und die bereitgestellten Bindungen binden möglicherweise nicht alle Java-Typen und-Member. Um ungebundene Java-Typen und-Member zu verwenden, kann die Java Native Interface (JNI) verwendet werden. In diesem Artikel wird veranschaulicht, wie jni verwendet wird, um mit Java-Typen und-Membern von xamarin. Android-Anwendungen zu interagieren._

## <a name="overview"></a>Übersicht

Es ist nicht immer erforderlich, einen verwalteten Callable Wrapper (MCW) zum Aufrufen von Java-Code zu erstellen. In vielen Fällen ist "Inline"-jni durchaus akzeptabel und nützlich für die einmalige Verwendung ungebundener Java-Member. Es ist häufig einfacher, JNI zu verwenden, um eine einzelne Methode in einer Java-Klasse aufzurufen, als eine gesamte jar-Bindung zu generieren.

Xamarin. Android stellt die `Mono.Android.dll` -Assembly bereit, die eine Bindung für die `android.jar` Android-Bibliothek bereitstellt. Typen und Member, die nicht `Mono.Android.dll` in vorhanden sind und nicht `android.jar` in vorhanden sind, können verwendet werden, wenn Sie manuell gebunden werden. Zum Binden von Java-Typen und-Membern verwenden Sie die **Java Native Interface** (**jni**), um Typen zu suchen, Felder zu lesen und zu schreiben und Methoden aufzurufen.

Die jni-API in xamarin. Android ist konzeptionell sehr ähnlich wie `System.Reflection` die API in .net: Sie ermöglicht es Ihnen, Typen und Member nach Name, Lese-und Schreib Feldwerten, Methoden aufrufen und mehr zu suchen. Mit JNI und dem `Android.Runtime.RegisterAttribute` benutzerdefinierten-Attribut können Sie virtuelle Methoden deklarieren, die an das Überschreiben von Unterstützung gebunden werden können. Sie können Schnittstellen binden, sodass Sie in C#implementiert werden können.

In diesem Dokument wird Folgendes erläutert:

- Gibt an, wie jni auf Typen verweist.
- Vorgehensweise beim Suchen, lesen und Schreiben von Feldern
- Gewusst wie: Suchen und Aufrufen von Methoden
- So machen Sie virtuelle Methoden verfügbar, um das Überschreiben von verwaltetem Code zuzulassen.
- Verfügbar machen von Schnittstellen.

## <a name="requirements"></a>Anforderungen

Jni ist in jeder Version von xamarin. Android verfügbar, die durch den [Android. Runtime. jnienv-Namespace](xref:Android.Runtime.JNIEnv)verfügbar gemacht wird.
Zum Binden von Java-Typen und-Schnittstellen müssen Sie xamarin. Android 4,0 oder höher verwenden.

## <a name="managed-callable-wrappers"></a>Verwaltete Callable Wrapper

Ein **verwalteter Callable Wrapper** (**MCW**) ist eine *Bindung* für eine Java-Klasse oder-Schnittstelle, die alle jni-Maschinen umschließt, sodass sich der Client C# Code nicht um die zugrunde liegende Komplexität von jni kümmern muss. Die meisten `Mono.Android.dll` von bestehen aus verwalteten Callable Wrappern.

Verwaltete Callable Wrapper dienen zwei Zwecken:

1. Kapseln Sie die jni-Verwendung, damit der Client Code die zugrunde liegende Komplexität nicht kennen muss.
1. Ermöglicht die Unterklasse von Java-Typen und die Implementierung von Java-Schnittstellen.

Der erste Zweck besteht lediglich aus Gründen der leichteren und Kapselung der Komplexität, damit Consumer über einen einfachen, verwalteten Satz von Klassen verfügen, die verwendet werden können. Hierfür müssen die verschiedenen [jnienv](xref:Android.Runtime.JNIEnv) -Member verwendet werden, wie weiter unten in diesem Artikel beschrieben. Beachten Sie, dass verwaltete Callable Wrapper nicht unbedingt erforderlich &ndash; sind. die Verwendung von "Inline"-jni ist durchaus akzeptabel und eignet sich für die einmalige Verwendung ungebundener Java-Member. Die Unterklassen-und Schnittstellen Implementierung erfordert die Verwendung verwalteter Aufruf barer Wrapper.

## <a name="android-callable-wrappers"></a>Android Callable Wrapper

Android Callable Wrapper (ACW) sind immer dann erforderlich, wenn die Android-Laufzeit (Art) verwalteten Code aufrufen muss. Diese Wrapper sind erforderlich, da es keine Möglichkeit gibt, Klassen mit Kunst zur Laufzeit zu registrieren.
(Insbesondere die Funktion [defineClass](http://docs.oracle.com/javase/6/docs/technotes/guides/jni/spec/functions.html#wp15986) jni wird von der Android-Laufzeit nicht unterstützt. Android Callable Wrapper bilden daher die fehlende Unterstützung für die Laufzeit-Typregistrierung.)

Wenn Android-Code eine virtuelle oder Schnittstellen Methode ausführen muss, die überschrieben oder in verwaltetem Code implementiert wird, muss xamarin. Android einen Java-Proxy bereitstellen, damit diese Methode an den entsprechenden verwalteten Typ weitergeleitet wird. Diese Java-Proxy Typen sind Java-Code, der die "gleiche" Basisklasse und Java-Schnittstellen Liste wie der verwaltete Typ hat, wobei dieselben Konstruktoren implementiert werden und alle überschriebenen Basisklassen-und Schnittstellen Methoden deklariert werden.

Android Callable Wrapper werden während des [Buildprozesses](~/android/deploy-test/building-apps/build-process.md)durch das **monodroid. exe** -Programm generiert und für alle Typen generiert, die (direkt oder indirekt) [java. lang. Object](xref:Java.Lang.Object)erben.

### <a name="implementing-interfaces"></a>Implementieren von Schnittstellen

Es gibt Zeiten, in denen Sie möglicherweise eine Android-Schnittstelle implementieren müssen (z. b [. Android. Content. icomponentcallbacks](xref:Android.Content.IComponentCallbacks)).

Alle Android-Klassen und-Schnittstellen erweitern die [Android. Runtime. ijavaobject](xref:Android.Runtime.IJavaObject) -Schnittstelle. Daher müssen alle Android-Typen implementieren `IJavaObject`.
Xamarin. Android nutzt diese Tatsache &ndash; `IJavaObject` , um Android mit einem Java-Proxy (einem Android Callable Wrapper) für den angegebenen verwalteten Typ bereitzustellen. Da **monodroid. exe** nur nach `Java.Lang.Object` Unterklassen sucht (die implementieren `IJavaObject`müssen), bietet die subclassingmethode `Java.Lang.Object` eine Möglichkeit, Schnittstellen in verwaltetem Code zu implementieren. Beispiel:

```csharp
class MyComponentCallbacks : Java.Lang.Object, Android.Content.IComponentCallbacks {
    public void OnConfigurationChanged (Android.Content.Res.Configuration newConfig) {
        // implementation goes here...
    }
    public void OnLowMemory () {
        // implementation goes here...
    }
}
```

### <a name="implementation-details"></a>Implementierungsdetails

*Der Rest dieses Artikels enthält Implementierungsdetails, die sich ohne vorherige Ankündigung ändern können* . (und wird hier nur hier vorgestellt, weil Entwickler möglicherweise neugierig sind, was im Hintergrund passiert).

Angenommen, Sie haben folgende C# Quelle:

```csharp
using System;
using Android.App;
using Android.OS;

namespace Mono.Samples.HelloWorld
{
    public class HelloAndroid : Activity
    {
        protected override void OnCreate (Bundle savedInstanceState)
        {
            base.OnCreate (savedInstanceState);
            SetContentView (R.layout.main);
        }
    }
}
```

Mit dem Programm " **mandroid. exe** " wird der folgende Android Callable-Wrapper generiert:

```java
package mono.samples.helloWorld;

public class HelloAndroid extends android.app.Activity {
    static final String __md_methods;
    static {
        __md_methods =
            "n_onCreate:(Landroid/os/Bundle;)V:GetOnCreate_Landroid_os_Bundle_Handler\n" +
            "";
        mono.android.Runtime.register (
                "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null",
                HelloAndroid.class,
                __md_methods);
    }

    public HelloAndroid ()
    {
        super ();
        if (getClass () == HelloAndroid.class)
            mono.android.TypeManager.Activate (
                "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null",
                "", this, new java.lang.Object[] { });
    }

    @Override
    public void onCreate (android.os.Bundle p0)
    {
        n_onCreate (p0);
    }

    private native void n_onCreate (android.os.Bundle p0);
}
```

Beachten Sie, dass die Basisklasse beibehalten wird und dass für jede Methode, die innerhalb von verwaltetem Code überschrieben wird, systemeigene Methoden Deklarationen bereitgestellt werden.

### <a name="exportattribute-and-exportfieldattribute"></a>Export Attribute und exportfieldattribute

In der Regel generiert xamarin. Android automatisch den Java-Code, der den ACW umfasst. Diese Generierung basiert auf den Klassen-und Methodennamen, wenn eine Klasse von einer Java-Klasse abgeleitet ist und vorhandene Java-Methoden überschreibt. In einigen Szenarien ist die Codegenerierung jedoch nicht ausreichend, wie im folgenden beschrieben:

- Android unterstützt Aktions Namen in LayoutXml-Attributen, z. b. das [Android: OnClick](xref:Android.Views.View.IOnClickListener.OnClick*) -XML-Attribut. Wenn er angegeben wird, versucht die Instanz der aufgeblähten Ansicht, die Java-Methode zu suchen.

- Die [java. IO. serialisierbare](https://developer.android.com/reference/java/io/Serializable.html) Schnitt `readObject` Stelle `writeObject` erfordert-und-Methoden. Da es sich nicht um Member dieser Schnittstelle handelt, werden diese Methoden von der entsprechenden verwalteten Implementierung nicht für Java-Code verfügbar gemacht.

- Die [Android. OS. parameterable](xref:Android.OS.Parcelable) -Schnittstelle erwartet, dass eine Implementierungs Klasse ein statisches `CREATOR` Feld `Parcelable.Creator`vom Typ aufweisen muss. Der generierte Java-Code erfordert ein explizites Feld. In unserem Standardszenario gibt es keine Möglichkeit, ein Feld in Java-Code aus verwaltetem Code auszugeben.

Da die Codegenerierung keine Projekt Mappe bereitstellt, um beliebige Java-Methoden mit beliebigen Namen zu generieren, beginnend mit xamarin. Android 4,2, wurden [Export Attribute](xref:Java.Interop.ExportAttribute) und [exportfieldattribute](xref:Java.Interop.ExportFieldAttribute) eingeführt, um die oben beschriebene Lösung anzubieten. SS. Beide Attribute befinden sich im `Java.Interop` -Namespace:

- `ExportAttribute`&ndash; gibt einen Methodennamen und die erwarteten Ausnahme Typen an (um explizites "auslösen" in Java zu geben). Wenn Sie für eine Methode verwendet wird, wird von der Methode eine Java-Methode exportiert, die einen dispatchcode für den entsprechenden jni-Aufruf der verwalteten Methode generiert. Dies kann mit `android:onClick` und `java.io.Serializable`verwendet werden.

- `ExportFieldAttribute`&ndash; gibt einen Feldnamen an. Sie befindet sich auf einer Methode, die als Feldinitialisierer fungiert. Dies kann mit `android.os.Parcelable`verwendet werden.

Das Beispiel Projekt " [ExportAttribute](https://docs.microsoft.com/samples/xamarin/monodroid-samples/exportattribute) " veranschaulicht, wie diese Attribute verwendet werden.

#### <a name="troubleshooting-exportattribute-and-exportfieldattribute"></a>Problembehandlung bei Export Attribute und exportfieldattribute

- Die Paket Erstellung schlägt fehl, weil **Mono. Android. Export. dll** &ndash; fehlt `ExportAttribute` , `ExportFieldAttribute` Wenn Sie oder für einige Methoden in Ihrem Code oder abhängigen Bibliotheken verwendet haben. Sie müssen **Mono. Android. Export. dll**hinzufügen. Diese Assembly ist für die Unterstützung von Rückruf Code von Java isoliert. Es ist von **Mono. Android. dll** getrennt, da es der Anwendung zusätzliche Größe hinzufügt.

- Im Releasebuild `MissingMethodException` tritt für &ndash; Export Methoden in Releasebuild `MissingMethodException` auf, bei Export Methoden. (Dieses Problem ist in der neuesten Version von xamarin. Android behoben.)

### <a name="exportparameterattribute"></a>ExportParameterAttribute

`ExportAttribute`und `ExportFieldAttribute` stellen Funktionen bereit, die von Java-Lauf Zeit Code verwendet werden können. Dieser Lauf Zeit Code greift über die generierten jni-Methoden, die von diesen Attributen gesteuert werden, auf verwalteten Code zu. Folglich gibt es keine vorhandene Java-Methode, die von der verwalteten Methode gebunden wird. Daher wird die Java-Methode aus der Signatur einer verwalteten Methode generiert.

Dieser Fall ist jedoch nicht vollständig Determinant. Dies trifft insbesondere auf einige erweiterte Zuordnungen zwischen verwalteten Typen und Java-Typen zu, wie z. b.:

- InputStream
- OutputStream
- XmlPullParser
- XmlResourceParser

Wenn Typen wie diese für exportierte Methoden erforderlich sind, `ExportParameterAttribute` muss verwendet werden, um den entsprechenden Parameter oder Rückgabewert explizit einem Typ zuzuweisen.

### <a name="annotation-attribute"></a>Annotation-Attribut

In xamarin. Android 4,2 haben wir Implementierungs `IAnnotation` Typen in Attribute (System. Attribute) konvertiert und Unterstützung für die Generierung von Anmerkung in Java-Wrapper hinzugefügt.

Dies bedeutet, dass die folgenden Richtungsänderungen vorgenommen werden:

- Der Bindungs Generator generiert `Java.Lang.DeprecatedAttribute` aus `java.Lang.Deprecated` `[Obsolete]` (er sollte sich in verwaltetem Code befinden).

- Dies bedeutet nicht, dass die `Java.Lang.Deprecated` vorhandene Klasse verschwindet. Diese Java-basierten Objekte können weiterhin als normale Java-Objekte verwendet werden (wenn eine solche Verwendung vorhanden ist). Es gibt Klassen `DeprecatedAttribute`und. `Deprecated`

- Die `Java.Lang.DeprecatedAttribute` -Klasse ist als `[Annotation]` markiert. Wenn ein benutzerdefiniertes Attribut von diesem `[Annotation]` Attribut geerbt wird, generiert die MSBuild-Aufgabe eine Java-Anmerkung für dieses benutzerdefinierte Attribut (@Deprecated) im Android Callable Wrapper (ACW).

- Anmerkungen können auf Klassen, Methoden und exportierten Feldern (eine Methode in verwaltetem Code) generiert werden.

Wenn die enthaltende Klasse (die mit Anmerkungen versehene Klasse) oder die Klasse, die die mit Anmerkungen versehene Member enthält, nicht registriert ist, wird die gesamte Java-Klassen Quelle überhaupt nicht generiert, einschließlich der Anmerkungen. Für-Methoden können Sie angeben `ExportAttribute` , dass die Methode explizit generiert und mit Anmerkungen versehen werden soll. Außerdem handelt es sich nicht um eine Funktion zum "generieren" einer Java-Anmerkung-Klassendefinition. Anders ausgedrückt: Wenn Sie ein benutzerdefiniertes verwaltetes Attribut für eine bestimmte Anmerkung definieren, müssen Sie eine andere jar-Bibliothek hinzufügen, die die entsprechende Java-Anmerkung-Klasse enthält. Das Hinzufügen einer Java-Quelldatei, die den Anmerkung-Typ definiert, ist nicht ausreichend. Der Java-Compiler funktioniert nicht auf die gleiche Weise wie **apt**.

Außerdem gelten die folgenden Einschränkungen:

- Dieser Konvertierungsprozess berücksichtigt `@Target` bisher keine Anmerkung für den Anmerkung-Typ.

- Attribute auf eine Eigenschaft können nicht verwendet werden. Verwenden Sie stattdessen Attribute für den Eigenschaften Getter oder Setter.

## <a name="class-binding"></a>Klassen Bindung

Das Binden einer Klasse bedeutet, dass ein verwalteter Aufruf barer Wrapper geschrieben wird, um den Aufruf des zugrunde liegenden Java-Typs zu vereinfachen.

Das Binden von virtuellen und abstrakten Methoden, um C# das Überschreiben von zuzulassen, erfordert xamarin. Android 4,0. Allerdings kann jede Version von xamarin. Android nicht virtuelle Methoden, statische Methoden oder virtuelle Methoden binden, ohne außer Kraft setzungen zu unterstützen.

Eine Bindung enthält in der Regel die folgenden Elemente:

- Ein [jni-Handle für den Java-Typ, der gebunden wird](#_Looking_up_Java_Types).

- [Jni-Feld-IDs und-Eigenschaften für jedes gebundene Feld](#_Instance_Fields).

- [Jni-Methoden-IDs und-Methoden für jede gebundene Methode](#_Instance_Methods).

- Wenn Unterklassen erforderlich sind, muss der Typ über ein benutzerdefiniertes [RegisterAttribute](xref:Android.Runtime.RegisterAttribute) -Attribut für die Typdeklaration verfügen, bei dem " [RegisterAttribute. donotgenerateacw](xref:Android.Runtime.RegisterAttribute.DoNotGenerateAcw) " auf `true`festgelegt ist.

### <a name="declaring-type-handle"></a>Deklarieren des typhandles

Für die Methoden Suchmethoden für Felder und Methoden ist ein Objekt Verweis erforderlich, der auf Ihren deklarierenden Typ verweist. Gemäß der Konvention wird dies in einem `class_ref` Feld gespeichert:

```csharp
static IntPtr class_ref = JNIEnv.FindClass(CLASS);
```

Ausführliche Informationen `CLASS` zum Token finden Sie im Abschnitt [jni-Typverweise](#_JNI_Type_References) .

### <a name="binding-fields"></a>Bindungs Felder

Java-Felder werden als C# Eigenschaften verfügbar gemacht, z. b. ist das Java- C# Feld [java.lang.System.in](https://developer.android.com/reference/java/lang/System.html#in) als Eigenschaft [java.lang.JavaSystem.in](xref:Java.Lang.JavaSystem.In)gebunden.
Da jni zwischen statischen Feldern und Instanzfeldern unterscheidet, werden außerdem beim Implementieren der Eigenschaften verschiedene Methoden verwendet.

Die Feld Bindung umfasst drei Sätze von Methoden:

1. Die *Get Field ID* -Methode. Die *Get Field ID* -Methode ist verantwortlich für das Zurückgeben eines Feld Handles, das von den *Get Field Value* -und *Set-Feldwert* -Methoden verwendet wird. Zum Abrufen der Feld-ID müssen Sie den deklarierenden Typ, den Namen des Felds und die [jni-Typsignatur](#JNI_Type_Signatures) des Felds kennen.

1. Die *Get Field Value* -Methoden. Diese Methoden erfordern das Feld Handle und sind dafür verantwortlich, den Wert des Felds aus Java zu lesen.
    Die zu verwendende Methode hängt vom Typ des Felds ab.

1. Die Methoden des *Set-Feldwerts* . Diese Methoden erfordern das Feld Handle und sind dafür verantwortlich, den Wert des Felds in Java zu schreiben. Die zu verwendende Methode hängt vom Typ des Felds ab.

[Statische Felder](#_Static_Fields) verwenden die Methoden [jnienv. getstaticfieldid](xref:Android.Runtime.JNIEnv.GetStaticMethodID*), `JNIEnv.GetStatic*Field`und [jnienv. SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*) .

 [Instanzfelder](#_Instance_Fields) verwenden die Methoden [jnienv. getfieldid](xref:Android.Runtime.JNIEnv.GetFieldID*), `JNIEnv.Get*Field`und [jnienv. SetField](xref:Android.Runtime.JNIEnv.SetField*) .

Beispielsweise kann die statische Eigenschaft `JavaSystem.In` wie folgt implementiert werden:

```csharp
static IntPtr in_jfieldID;
public static System.IO.Stream In
{
    get {
        if (in_jfieldId == IntPtr.Zero)
            in_jfieldId = JNIEnv.GetStaticFieldID (class_ref, "in", "Ljava/io/InputStream;");
        IntPtr __ret = JNIEnv.GetStaticObjectField (class_ref, in_jfieldId);
        return InputStreamInvoker.FromJniHandle (__ret, JniHandleOwnership.TransferLocalRef);
    }
}
```

Hinweis: Wir verwenden [inputstreaminvoker. fromjnihandle](xref:Android.Runtime.InputStreamInvoker.FromJniHandle*) , um den jni-Verweis in eine `System.IO.Stream` -Instanz zu konvertieren, und `JniHandleOwnership.TransferLocalRef` wir verwenden, da [jnienv. getstaticobjectfield](xref:Android.Runtime.JNIEnv.GetStaticObjectField*) einen lokalen Verweis zurückgibt.

Viele der [Android. Runtime](xref:Android.Runtime) -Typen verfügen `FromJniHandle` über Methoden, die einen jni-Verweis in den gewünschten Typ konvertieren.

### <a name="method-binding"></a>Methoden Bindung

Java-Methoden werden als C# Methoden und als C# Eigenschaften verfügbar gemacht. Beispielsweise ist die Java-Methode java [. lang. Runtime. runFinalizersOnExit](https://developer.android.com/reference/java/lang/Runtime.html#runFinalizersOnExit(boolean)) als [java. lang. Runtime. runFinalizersOnExit](xref:Java.Lang.Runtime.RunFinalizersOnExit*) -Methode gebunden, und die [java. lang. Object. GetClass](https://developer.android.com/reference/java/lang/Object.html#getClass) -Methode ist als [java. lang. Object. Class gebunden. ](xref:Java.Lang.Object.Class)-Eigenschaft.

Der Methodenaufruf ist ein zweistufiger Prozess:

1. Die *Get-Methoden-ID* für die aufzurufende Methode. Die Methode " *get Method ID* " ist für das Zurückgeben eines Methoden Handles zuständig, das von den Methodenaufruf Methoden verwendet wird. Zum Abrufen der Methoden-ID müssen Sie den deklarierenden Typ, den Namen der Methode und die [jni-Typsignatur](#JNI_Type_Signatures) der Methode kennen.

1. Rufen Sie die Methode auf.

Ebenso wie bei Feldern unterscheiden sich die Methoden, die zum Aufrufen der Methoden-ID und Aufrufen der Methode verwendet werden, von statischen Methoden und Instanzmethoden.

[Statische Methoden](#_Static_Methods_1) verwenden [jnienv. getstaticmethodid ()](xref:Android.Runtime.JNIEnv.GetStaticMethodID*) , um die Methoden-ID zu suchen, und `JNIEnv.CallStatic*Method` verwenden die Familie der Methoden für den Aufruf.

[Instanzmethoden](#_Instance_Methods) verwenden [jnienv. getmethodid](xref:Android.Runtime.JNIEnv.GetMethodID*) , um die Methoden-ID zu suchen, `JNIEnv.Call*Method` und `JNIEnv.CallNonvirtual*Method` verwenden die-und-Familien von Methoden für den Aufruf.

Die Methoden Bindung ist möglicherweise mehr als nur der Methodenaufruf. Die Methoden Bindung umfasst auch das zulassen, dass eine Methode überschrieben wird (für abstrakte und nicht abschließende Methoden) oder implementiert (für Schnittstellen Methoden). Der Abschnitt [unterstützende Vererbung, Schnittstellen](#_Supporting_Inheritance,_Interfaces_1) behandelt die Komplexität der Unterstützung virtueller Methoden und Schnittstellen Methoden.

<a name="_Static_Methods_1" />

#### <a name="static-methods"></a>Statische Methoden

Das Binden einer statischen Methode Bein `JNIEnv.GetStaticMethodID` haltet die Verwendung von zum Abrufen eines Methoden Handles und `JNIEnv.CallStatic*Method` das anschließende Verwenden der entsprechenden Methode, je nach Rückgabetyp der Methode. Im folgenden finden Sie ein Beispiel für eine Bindung für die [Runtime. GetRuntime](https://developer.android.com/reference/java/lang/Runtime.html#getRuntime()) -Methode:

```csharp
static IntPtr id_getRuntime;

[Register ("getRuntime", "()Ljava/lang/Runtime;", "")]
public static Java.Lang.Runtime GetRuntime ()
{
    if (id_getRuntime == IntPtr.Zero)
        id_getRuntime = JNIEnv.GetStaticMethodID (class_ref,
                "getRuntime", "()Ljava/lang/Runtime;");

    return Java.Lang.Object.GetObject<Java.Lang.Runtime> (
            JNIEnv.CallStaticObjectMethod  (class_ref, id_getRuntime),
            JniHandleOwnership.TransferLocalRef);
}
```

Beachten Sie, dass das Methoden Handle in einem statischen Feld gespeichert `id_getRuntime`wird. Dies ist eine Leistungsoptimierung, sodass der Methoden Handle nicht bei jedem Aufruf gesucht werden muss. Es ist nicht erforderlich, das Methoden Handle auf diese Weise zwischenzuspeichern. Nachdem das Methoden Handle abgerufen wurde, wird [jnienv. callstaticobjectmethod](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*) zum Aufrufen der-Methode verwendet. `JNIEnv.CallStaticObjectMethod`Gibt einen `IntPtr` zurück, der das Handle der zurückgegebenen Java-Instanz enthält.
[Java. lang. Object. GetObject&lt;T&gt;(IntPtr, jnishandownership)](xref:Java.Lang.Object.GetObject*) wird verwendet, um das Java-Handle in eine stark typisierte Objektinstanz zu konvertieren.

#### <a name="non-virtual-instance-method-binding"></a>Methoden Bindung für nicht virtuelle Instanzen

Das Binden `final` einer Instanzmethode oder einer Instanzmethode, die keine Überschreibung `JNIEnv.GetMethodID` erfordert, umfasst die Verwendung von zum Abrufen eines Methoden `JNIEnv.Call*Method` Handles und die anschließende Verwendung der entsprechenden Methode, je nach Rückgabetyp der Methode. Im folgenden finden Sie ein Beispiel für eine Bindung für `Object.Class` die-Eigenschaft:

```csharp
static IntPtr id_getClass;
public Java.Lang.Class Class {
    get {
        if (id_getClass == IntPtr.Zero)
            id_getClass = JNIEnv.GetMethodID (class_ref, "getClass", "()Ljava/lang/Class;");
        return Java.Lang.Object.GetObject<Java.Lang.Class> (
                JNIEnv.CallObjectMethod (Handle, id_getClass),
                JniHandleOwnership.TransferLocalRef);
    }
}
```

Beachten Sie, dass das Methoden Handle in einem statischen Feld gespeichert `id_getClass`wird.
Dies ist eine Leistungsoptimierung, sodass der Methoden Handle nicht bei jedem Aufruf gesucht werden muss. Es ist nicht erforderlich, das Methoden Handle auf diese Weise zwischenzuspeichern. Nachdem das Methoden Handle abgerufen wurde, wird [jnienv. callstaticobjectmethod](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*) zum Aufrufen der-Methode verwendet. `JNIEnv.CallStaticObjectMethod`Gibt einen `IntPtr` zurück, der das Handle der zurückgegebenen Java-Instanz enthält.
[Java. lang. Object. GetObject&lt;T&gt;(IntPtr, jnishandownership)](xref:Java.Lang.Object.GetObject*) wird verwendet, um das Java-Handle in eine stark typisierte Objektinstanz zu konvertieren.

### <a name="binding-constructors"></a>Bindungskonstruktoren

Konstruktoren sind Java-Methoden mit dem `"<init>"`Namen. Ebenso wie bei den Java-Instanzmethoden wird verwendet, `JNIEnv.GetMethodID` um das konstruktorhandle zu suchen. Im Gegensatz zu Java-Methoden werden die [jnienv. NewObject](xref:Android.Runtime.JNIEnv.NewObject*) -Methoden verwendet, um das konstruktormethodenhandle aufzurufen. Der Rückgabewert von `JNIEnv.NewObject` ist ein lokaler jni-Verweis:

```csharp
int value = 42;
IntPtr class_ref    = JNIEnv.FindClass ("java/lang/Integer");
IntPtr id_ctor_I    = JNIEnv.GetMethodID (class_ref, "<init>", "(I)V");
IntPtr lrefInstance = JNIEnv.NewObject (class_ref, id_ctor_I, new JValue (value));
// Dispose of lrefInstance, class_ref…
```

Normalerweise wird eine Klassen Bindung " [java. lang. Object](xref:Java.Lang.Object)" Unterklassen.
Bei der Unterklasse `Java.Lang.Object`wird eine zusätzliche Semantik ins Spiel genommen: `Java.Lang.Object` eine-Instanz verwaltet einen globalen Verweis auf eine Java- `Java.Lang.Object.Handle` Instanz über die-Eigenschaft.

1. Der `Java.Lang.Object` Standardkonstruktor wird eine Java-Instanz zuordnen.

1. Wenn `RegisterAttribute` der Typ einen hat und `RegisterAttribute.DoNotGenerateAcw` ist `true` , wird eine Instanz des `RegisterAttribute.Name` Typs über den Standardkonstruktor erstellt.

1. Andernfalls wird der [Android Callable Wrapper](~/android/platform/java-integration/android-callable-wrappers.md) (ACW), der `this.GetType` entspricht, über seinen Standardkonstruktor instanziiert. Android Callable Wrapper werden während der Paket Erstellung für jede `Java.Lang.Object` Unterklasse generiert, für die `RegisterAttribute.DoNotGenerateAcw` nicht auf `true`festgelegt ist.

Bei Typen, die keine Klassen Bindungen sind, ist dies die erwartete Semantik: das Instanziieren einer `Mono.Samples.HelloWorld.HelloAndroid` C# -Instanz `mono.samples.helloworld.HelloAndroid` sollte eine Java-Instanz erstellen, bei der es sich um einen generierten, von Android Callable Wrapper handelt

Bei Klassen Bindungen kann dies das richtige Verhalten sein, wenn der Java-Typ einen Standardkonstruktor enthält und/oder kein anderer Konstruktor aufgerufen werden muss. Andernfalls muss ein Konstruktor bereitgestellt werden, der die folgenden Aktionen ausführt:

1. Aufrufen von " [java. lang. Object" (IntPtr, jnilenker Ownership)](xref:Java.Lang.Object#ctor*) anstelle des `Java.Lang.Object` Standardkonstruktors. Dies ist erforderlich, um das Erstellen einer neuen Java-Instanz zu vermeiden.

1. Überprüfen Sie den Wert von [java. lang. Object. handle](xref:Java.Lang.Object.Handle) , bevor Sie Java-Instanzen erstellen. Die `Object.Handle` -Eigenschaft weist einen anderen Wert als `IntPtr.Zero` auf, wenn ein von Android Callable Wrapper in Java-Code erstellt wurde, und die Klassen Bindung wird erstellt, um die erstellte Android Callable Wrapper-Instanz zu enthalten. Wenn z. b. Android eine `mono.samples.helloworld.HelloAndroid` -Instanz erstellt, wird der Android Callable Wrapper zuerst erstellt, und der `HelloAndroid` Java-Konstruktor erstellt eine Instanz des entsprechenden `Mono.Samples.HelloWorld.HelloAndroid` Typs, wobei die `Object.Handle` Eigenschaft Legen Sie vor der Konstruktorausführung auf die Java-Instanz fest.

1. Wenn der aktuelle Lauf Zeittyp nicht mit dem deklarierenden Typ identisch ist, muss eine Instanz des entsprechenden Android Callable-Wrappers erstellt werden. verwenden Sie [Object. SetHandle](xref:Java.Lang.Object.SetHandle*) , um das von [jnienv. kreateinstance](xref:Android.Runtime.JNIEnv.CreateInstance*)zurückgegebene Handle zu speichern.

1. Wenn der aktuelle Lauf Zeittyp mit dem deklarierenden Typ identisch ist, rufen Sie den Java-Konstruktor auf, und verwenden Sie [Object. SetHandle](xref:Java.Lang.Object.SetHandle*) zum Speichern des Handles, das von `JNIEnv.NewInstance` zurückgegeben wird.

Sehen Sie sich beispielsweise den [java. lang. Integer (int)](https://developer.android.com/reference/java/lang/Integer.html#Integer(int)) -Konstruktor an. Diese ist gebunden als:

```csharp
// Cache the constructor's method handle for later use
static IntPtr id_ctor_I;

// Need [Register] for subclassing
// RegisterAttribute.Name is always ".ctor"
// RegisterAttribute.Signature is tye JNI type signature of constructor
// RegisterAttribute.Connector is ignored; use ""
[Register (".ctor", "(I)V", "")]
public Integer (int value)
    // 1. Prevent Object default constructor execution
    : base (IntPtr.Zero, JniHandleOwnership.DoNotTransfer)
{
    // 2. Don't allocate Java instance if already allocated
    if (Handle != IntPtr.Zero)
        return;

    // 3. Derived type? Create Android Callable Wrapper
    if (GetType () != typeof (Integer)) {
        SetHandle (
                Android.Runtime.JNIEnv.CreateInstance (GetType (), "(I)V", new JValue (value)),
                JniHandleOwnership.TransferLocalRef);
        return;
    }

    // 4. Declaring type: lookup &amp; cache method id...
    if (id_ctor_I == IntPtr.Zero)
        id_ctor_I = JNIEnv.GetMethodID (class_ref, "<init>", "(I)V");
    // ...then create the Java instance and store
    SetHandle (
            JNIEnv.NewObject (class_ref, id_ctor_I, new JValue (value)),
            JniHandleOwnership.TransferLocalRef);
}
```

Die [jnienv. kreatabstance](xref:Android.Runtime.JNIEnv.CreateInstance*) -Methoden sind Hilfsprogramme, um `JNIEnv.FindClass`, `JNIEnv.GetMethodID`, `JNIEnv.NewObject`und `JNIEnv.DeleteGlobalReference` auf dem Wert auszuführen, der `JNIEnv.FindClass`von zurückgegeben wird. Nähere Informationen finden Sie im nächsten Abschnitt.

<a name="_Supporting_Inheritance,_Interfaces_1" />

### <a name="supporting-inheritance-interfaces"></a>Unterstützende Vererbung, Schnittstellen

Zum Unterklassen eines Java-Typs oder zum Implementieren einer Java-Schnittstelle ist die Generierung von [Android Callable Wrappern](~/android/platform/java-integration/android-callable-wrappers.md) (acws) `Java.Lang.Object` erforderlich, die während des Verpackungsprozesses für jede Unterklasse generiert werden. Die ACW-Generierung wird durch das benutzerdefinierte Attribut " [Android. Runtime. RegisterAttribute](xref:Android.Runtime.RegisterAttribute) " gesteuert.

Für C# -Typen erfordert `[Register]` der Konstruktor des benutzerdefinierten Attributs ein Argument: den [vereinfachten jni-Typverweis](#_Simplified_Type_References_1) für den entsprechenden Java-Typ. Dadurch können unterschiedliche Namen zwischen Java und C#bereitgestellt werden.

Vor xamarin. Android 4,0 war das `[Register]` benutzerdefinierte Attribut nicht als Alias für vorhandene Java-Typen verfügbar. Dies liegt daran, dass der ACW-Generierungsprozess acws `Java.Lang.Object` für jede aufgetretene Unterklasse generieren würde.

Xamarin. Android 4,0 führte die [RegisterAttribute. donotgenerateacw](xref:Android.Runtime.RegisterAttribute.DoNotGenerateAcw) -Eigenschaft ein. Diese Eigenschaft weist den ACW-Generierungsprozess an, den mit Anmerkungen versehenen Typ zu über *springen* . Dies ermöglicht die Deklaration neuer verwalteter Aufruf barer Wrapper, die zum Zeitpunkt der Paket Erstellung nicht zum Generieren von acws führen. Dadurch können vorhandene Java-Typen gebunden werden. Nehmen Sie zum Beispiel die folgende einfache Java-Klasse `Adder`, die eine `add`Methode enthält,, die zu ganzen Zahlen hinzufügt und das Ergebnis zurückgibt:

```java
package mono.android.test;
public class Adder {
    public int add (int a, int b) {
        return a + b;
    }
}
```

Der `Adder` Typ könnte wie folgt gebunden werden:

```csharp
[Register ("mono/android/test/Adder", DoNotGenerateAcw=true)]
public partial class Adder : Java.Lang.Object {
    static IntPtr class_ref = JNIEnv.FindClass ( "mono/android/test/Adder");

    public Adder ()
    {
    }

    public Adder (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
    }
}
partial class ManagedAdder : Adder {
}
```

Hier wird der `Adder` C# Java-TypdurchdenTypAliasAliase`Adder` . Das `[Register]` -Attribut wird verwendet, um den jni-Namen `mono.android.test.Adder` des Java-Typs anzugeben `DoNotGenerateAcw` , und die-Eigenschaft wird verwendet, um die ACW-Generierung zu verhindern. Dies führt zur Generierung einer Zugriffs Steuerungs Liste für den `ManagedAdder` Typ, der den `mono.android.test.Adder` Typ ordnungsgemäß Unterklassen verwendet. Wenn die `RegisterAttribute.DoNotGenerateAcw` Eigenschaft nicht verwendet wurde, hätte der xamarin. Android-Buildprozess einen neuen `mono.android.test.Adder` Java-Typ generiert. Dies würde zu Kompilierungs Fehlern führen, da `mono.android.test.Adder` der Typ zweimal in zwei separaten Dateien vorhanden wäre.

### <a name="binding-virtual-methods"></a>Binden von virtuellen Methoden

`ManagedAdder`Unterklassen der Java `Adder` -Typ, aber es ist nicht besonders interessant C# `Adder` : der Typ definiert keine virtuellen Methoden, `ManagedAdder` sodass nichts überschrieben werden kann.

Bindungs `virtual` Methoden, um die Überschreibung durch Unterklassen zuzulassen, müssen mehrere Aufgaben durchgeführt werden, die in die folgenden beiden Kategorien fallen:

1. **Methoden Bindung**

1. **Methoden Registrierung**

#### <a name="method-binding"></a>Methoden Bindung

Eine Methoden Bindung erfordert, dass zwei Unterstützungs C# `Adder` Elemente zur Definition hinzugefügt werden `ThresholdType`:, `ThresholdClass`und.

##### <a name="thresholdtype"></a>ThresholdType

Die `ThresholdType` -Eigenschaft gibt den aktuellen Typ der Bindung zurück:

```csharp
partial class Adder {
    protected override System.Type ThresholdType {
        get {
            return typeof (Adder);
        }
    }
}
```

`ThresholdType`wird in der Methoden Bindung verwendet, um zu bestimmen, wann eine virtuelle und nicht virtuelle Methoden Verteilung durchgeführt werden soll. Er sollte immer eine `System.Type` -Instanz zurückgeben, die dem deklarierenden C# Typ entspricht.

##### <a name="thresholdclass"></a>ThresholdClass

Die `ThresholdClass` -Eigenschaft gibt den jni-Klassen Verweis für den gebundenen Typ zurück:

```csharp
partial class Adder {
    protected override IntPtr ThresholdClass {
        get {
            return class_ref;
        }
    }
}
```

`ThresholdClass`wird in der Methoden Bindung verwendet, wenn nicht virtuelle Methoden aufgerufen werden.

#### <a name="binding-implementation"></a>Bindungs Implementierung

Die Implementierung der Methoden Bindung ist für den Lauf Zeit Aufruf der Java-Methode verantwortlich. Sie enthält auch eine `[Register]` benutzerdefinierte Attribut Deklaration, die Teil der Methoden Registrierung ist, und wird im Abschnitt zur Methoden Registrierung erläutert:

```csharp
[Register ("add", "(II)I", "GetAddHandler")]
    public virtual int Add (int a, int b)
    {
        if (id_add == IntPtr.Zero)
            id_add = JNIEnv.GetMethodID (class_ref, "add", "(II)I");
        if (GetType () == ThresholdType)
            return JNIEnv.CallIntMethod (Handle, id_add, new JValue (a), new JValue (b));
        return JNIEnv.CallNonvirtualIntMethod (Handle, ThresholdClass, id_add, new JValue (a), new JValue (b));
    }
}
```

Das `id_add` -Feld enthält die Methoden-ID für die aufzurufende Java-Methode. Der `id_add` Wert wird aus `JNIEnv.GetMethodID`abgerufen. hierfür sind die deklarierende Klasse`class_ref`(), der Java-Methoden`"add"`Name () und die jni-Signatur der Methode`"(II)I"`() erforderlich.

Nachdem die Methoden-ID abgerufen wurde `GetType` , wird mit `ThresholdType` verglichen, um zu bestimmen, ob eine virtuelle oder eine nicht virtuelle Verteilung erforderlich ist. Wenn `GetType` Übereinstimmungen `ThresholdType`gefunden werden, ist eine `Handle` virtuelle Verteilung erforderlich, die auf eine Java-zugeordnete Unterklasse verweist, die die-Methode überschreibt.

Wenn `GetType` nicht entspricht `ThresholdType` `ManagedAdder`, `Adder` wurde untergeordnet (z. b. von), und `Adder.Add` die Implementierung wird nur aufgerufen, wenn die Unterklasse `base.Add`aufgerufen wurde. Dabei handelt es sich um den nicht virtuellen dispatchfall, `ThresholdClass` in dem sich befindet. `ThresholdClass`Gibt an, welche Java-Klasse die Implementierung der aufzurufenden Methode bereitstellt.

#### <a name="method-registration"></a>Methoden Registrierung

Angenommen, wir haben eine `ManagedAdder` aktualisierte Definition, die die `Adder.Add` -Methode überschreibt:

```csharp
partial class ManagedAdder : Adder {
    public override int Add (int a, int b) {
        return (a*2) + (b*2);
    }
}
```

Rückruf, `Adder.Add` der über `[Register]` ein benutzerdefiniertes Attribut verfügte:

```csharp
[Register ("add", "(II)I", "GetAddHandler")]
```

Der `[Register]` Konstruktor des benutzerdefinierten Attributs akzeptiert drei Werte:

1. Der Name der Java-Methode, `"add"` in diesem Fall.

1. Die jni-Typsignatur der Methode, `"(II)I"` in diesem Fall.

1. Die *Connector* -Methode `GetAddHandler` , in diesem Fall.
    Connector-Methoden werden später erläutert.

Die ersten beiden Parameter ermöglichen es dem ACW-Generierungsprozess, eine Methoden Deklaration zu generieren, um die Methode zu überschreiben. Der resultierende ACW würde einen Teil des folgenden Codes enthalten:

```csharp
public class ManagedAdder extends mono.android.test.Adder {
    static final String __md_methods;
    static {
        __md_methods = "n_add:(II)I:GetAddHandler\n" +
            "";
        mono.android.Runtime.register (...);
    }
    @Override
    public int add (int p0, int p1) {
        return n_add (p0, p1);
    }
    private native int n_add (int p0, int p1);
    // ...
}
```

Beachten Sie, `@Override` dass eine-Methode deklariert wird, die `n_`an eine-Präfix-Methode mit demselben Namen delegiert. Dadurch `ManagedAdder.n_add` wird sichergestellt, dass beim `ManagedAdder.add`Aufrufen von Java-Code aufgerufen wird, sodass die C# `ManagedAdder.Add` über schreibende Methode ausgeführt werden kann.

Daher ist die wichtigste Frage: wie wird `ManagedAdder.n_add` `ManagedAdder.Add`verknüpft?

Java `native` -Methoden werden über die [Funktion "jni registernatives](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp17734)" bei der Java-Laufzeit (Android Runtime) registriert.
`RegisterNatives`nimmt ein Array von-Strukturen, das den Java-Methodennamen, die jni-Typsignatur und einen Funktionszeiger zum Aufrufen von der [jni-Aufruf Konvention](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/design.html#wp715)enthält.
Der Funktionszeiger muss eine Funktion sein, die zwei Zeigerargumente annimmt, gefolgt von den Methoden Parametern. Die Java `ManagedAdder.n_add` -Methode muss über eine Funktion implementiert werden, die über den folgenden C-Prototyp verfügt:

```csharp
int FunctionName(JNIEnv *env, jobject this, int a, int b)
```

Xamarin. Android macht keine `RegisterNatives` -Methode verfügbar. Stattdessen stellen der ACW und der MCW die erforderlichen Informationen zum Aufrufen `RegisterNatives`von bereit: der ACW enthält den Methodennamen und die jni-Typsignatur. das einzige fehlende Ergebnis ist ein Funktionszeiger, der verknüpft werden kann.

An dieser Stelle kommt die *Connector-Methode* ins Spiel. Der dritte `[Register]` benutzerdefinierte Attribut Parameter ist der Name einer Methode, die im registrierten Typ definiert ist, oder eine Basisklasse des registrierten Typs, die keine Parameter akzeptiert `System.Delegate`und einen zurückgibt. Der zurück `System.Delegate` gegebene bezieht sich wiederum auf eine Methode, die über die korrekte jni-Funktions Signatur verfügt. Schließlich *muss* der Delegat, der von der Connector-Methode zurückgegeben wird, den Stamm aufweisen, damit der GC ihn nicht sammelt, da der Delegat Java bereitgestellt wird.

```csharp
#pragma warning disable 0169
static Delegate cb_add;
// This method must match the third parameter of the [Register]
// custom attribute, must be static, must return System.Delegate,
// and must accept no parameters.
static Delegate GetAddHandler ()
{
    if (cb_add == null)
        cb_add = JNINativeWrapper.CreateDelegate ((Func<IntPtr, IntPtr, int, int, int>) n_Add);
    return cb_add;
}
// This method is registered with JNI.
static int n_Add (IntPtr jnienv, IntPtr lrefThis, int a, int b)
{
    Adder __this = Java.Lang.Object.GetObject<Adder>(lrefThis, JniHandleOwnership.DoNotTransfer);
    return __this.Add (a, b);
}
#pragma warning restore 0169
```

Die `GetAddHandler` -Methode erstellt `Func<IntPtr, IntPtr, int, int,
int>` einen Delegaten, der `n_Add` auf die-Methode verweist, und ruft dann [jninativewrapper. kreatedelegat](xref:Android.Runtime.JNINativeWrapper.CreateDelegate*)auf.
`JNINativeWrapper.CreateDelegate`umschließt die bereitgestellte Methode in einem try/catch-Block, sodass alle nicht behandelten Ausnahmen behandelt werden, und führt dazu, dass das Ereignis " [androidevent. unlenker dexceptionraiser](xref:Android.Runtime.AndroidEnvironment.UnhandledExceptionRaiser) " ausgelöst wird. Der resultierende Delegat wird in der statischen `cb_add` Variablen gespeichert, sodass die GC den Delegaten nicht freigibt.

Schließlich ist die `n_Add` -Methode für das Mars Hallen der jni-Parameter an die entsprechenden verwalteten Typen und die anschließende Delegierung des Methoden Aufrufes verantwortlich.

Hinweis: Verwenden `JniHandleOwnership.DoNotTransfer` Sie immer, wenn Sie eine MCW über eine Java-Instanz erhalten. Wenn Sie Sie als lokaler Verweis behandeln (und somit `JNIEnv.DeleteLocalRef`aufrufen), werden&gt; verwaltete, Java&gt; -verwaltete Stapel Übergänge nicht mehr unterbunden.

### <a name="complete-adder-binding"></a>Umfassende Adder-Bindung

Die komplette verwaltete Bindung für den `mono.android.tests.Adder` Typ lautet wie folgt:

```csharp
[Register ("mono/android/test/Adder", DoNotGenerateAcw=true)]
public class Adder : Java.Lang.Object {

    static IntPtr class_ref = JNIEnv.FindClass ("mono/android/test/Adder");

    public Adder ()
    {
    }

    public Adder (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
    }

    protected override Type ThresholdType {
        get {return typeof (Adder);}
    }

    protected override IntPtr ThresholdClass {
        get {return class_ref;}
    }

#region Add
    static IntPtr id_add;

    [Register ("add", "(II)I", "GetAddHandler")]
    public virtual int Add (int a, int b)
    {
        if (id_add == IntPtr.Zero)
            id_add = JNIEnv.GetMethodID (class_ref, "add", "(II)I");
        if (GetType () == ThresholdType)
            return JNIEnv.CallIntMethod (Handle, id_add, new JValue (a), new JValue (b));
        return JNIEnv.CallNonvirtualIntMethod (Handle, ThresholdClass, id_add, new JValue (a), new JValue (b));
    }

#pragma warning disable 0169
    static Delegate cb_add;
    static Delegate GetAddHandler ()
    {
        if (cb_add == null)
            cb_add = JNINativeWrapper.CreateDelegate ((Func<IntPtr, IntPtr, int, int, int>) n_Add);
        return cb_add;
    }

    static int n_Add (IntPtr jnienv, IntPtr lrefThis, int a, int b)
    {
        Adder __this = Java.Lang.Object.GetObject<Adder>(lrefThis, JniHandleOwnership.DoNotTransfer);
        return __this.Add (a, b);
    }
#pragma warning restore 0169
#endregion
}
```

### <a name="restrictions"></a>Restrictions

Beim Schreiben eines Typs, der mit den folgenden Kriterien übereinstimmt:

1. Unterklassen`Java.Lang.Object`

1. Verfügt über `[Register]` ein benutzerdefiniertes Attribut

1. `RegisterAttribute.DoNotGenerateAcw` ist gleich `true`.

Bei der GC-Interaktion *darf* der Typ keine Felder aufweisen, die zur Laufzeit auf `Java.Lang.Object` eine `Java.Lang.Object` -oder-Unterklasse verweisen können. Beispielsweise sind Felder vom Typ `System.Object` und alle Schnittstellentypen nicht zulässig. Typen, die nicht auf `Java.Lang.Object` -Instanzen verweisen können, sind `System.String` zulässig `List<int>`, z. b. und. Diese Einschränkung besteht darin, eine vorzeitige Objekt Auflistung durch die GC zu verhindern.

Wenn der Typ ein Instanzfeld enthalten muss, das auf eine `Java.Lang.Object` -Instanz verweisen kann, muss der Feldtyp `GCHandle`oder sein `System.WeakReference` .

## <a name="binding-abstract-methods"></a>Binden abstrakter Methoden

Bindungs `abstract` Methoden sind größtenteils identisch mit der Bindung virtueller Methoden. Es gibt nur zwei Unterschiede:

1. Die abstrakte Methode ist abstrakt. Es behält weiterhin das `[Register]` -Attribut und die zugeordnete Methoden Registrierung bei, aber die Methoden Bindung wird `Invoker` nur in den-Typ verschoben.

1. Ein Non- `abstract` `Invoker` Type wird erstellt, der den abstrakten Typ Unterklassen Unterklassen hat. Der `Invoker` Typ muss alle abstrakten Methoden überschreiben, die in der Basisklasse deklariert sind, und die überschriebene Implementierung ist die Implementierung der Methoden Bindung, obwohl der nicht virtuelle dispatchfall ignoriert werden kann.

Nehmen Sie beispielsweise an, dass `mono.android.test.Adder.add` die oben `abstract`genannte Methode. Die C# Bindung würde so geändert, `Adder.Add` dass Sie abstrakt sind, und `AdderInvoker` es wird ein neuer Typ definiert `Adder.Add`, der implementiert hat:

```csharp
partial class Adder {
    [Register ("add", "(II)I", "GetAddHandler")]
    public abstract int Add (int a, int b);

    // The Method Registration machinery is identical to the
    // virtual method case...
}

partial class AdderInvoker : Adder {
    public AdderInvoker (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
    }

    static IntPtr id_add;
    public override int Add (int a, int b)
    {
        if (id_add == IntPtr.Zero)
            id_add = JNIEnv.GetMethodID (class_ref, "add", "(II)I");
        return JNIEnv.CallIntMethod (Handle, id_add, new JValue (a), new JValue (b));
    }
}
```

Der `Invoker` -Typ ist nur erforderlich, wenn jni-Verweise auf von Java erstellte Instanzen erhalten werden.

## <a name="binding-interfaces"></a>Bindungsschnittstellen

Bindungsschnittstellen sind konzeptionell vergleichbar mit Bindungs Klassen, die virtuelle Methoden enthalten, aber viele der Besonderheiten unterscheiden sich in der subtilen (und nicht so geringfügigen) Art. Beachten Sie die folgende [Java-Schnittstellen Deklaration](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/Adder.java#L14):

```csharp
public interface Progress {
    void onAdd(int[] values, int currentIndex, int currentSum);
}
```

Schnittstellen Bindungen bestehen aus zwei Teilen: C# der Schnittstellen Definition und einer invokerdefinition für die-Schnittstelle.

### <a name="interface-definition"></a>Schnittstellen Definition

Die C# Schnittstellen Definition muss die folgenden Anforderungen erfüllen:

- Die Schnittstellen Definition muss über ein `[Register]` benutzerdefiniertes Attribut verfügen.

- Die Schnittstellen Definition muss die `IJavaObject interface`erweitern.
    Wenn dies nicht der Fall ist, wird verhindert, dass acws von der Java-Schnittstelle erbt.

- Jede Schnittstellen Methode muss ein `[Register]` -Attribut enthalten, das den Namen der entsprechenden Java-Methode, die jni-Signatur und die-Connector-Methode angibt.

- Die Connector-Methode muss auch den Typ angeben, auf dem sich die Connector-Methode befinden kann.

Beim Binden `abstract` von `virtual` -und-Methoden wird die-Connector-Methode innerhalb der Vererbungs Hierarchie des Typs gesucht, der registriert wird. Schnittstellen können keine Methoden enthalten, die Text enthalten, sodass dies nicht funktioniert. Daher muss ein Typ angegeben werden, der angibt, wo sich die Connector-Methode befindet. Der Typ wird in der Zeichenfolge der Verbindungs Zeichenfolge nach `':'`einem Doppelpunkt angegeben und muss der durch die Assembly qualifizierte Typname des Typs sein, der den Invoker enthält.

Schnittstellen Methoden Deklarationen sind eine Übersetzung der entsprechenden Java-Methode unter Verwendung *kompatibler* Typen. Bei Java-Builtin-Typen sind die kompatiblen Typen die C# entsprechenden Typen, z. `int` b C# `int`. Java ist. Bei Verweis Typen ist der kompatible Typ ein Typ, der ein jni-Handle des entsprechenden Java-Typs bereitstellen kann.

Die Schnittstellenmember werden nicht direkt durch den Aufruf &ndash; &ndash; von Java aufgerufen, sodass eine gewisse Flexibilität zulässig ist.

Die Java-Status Schnittstelle kann [ C# in wie folgt deklariert](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L83)werden:

```csharp
[Register ("mono/android/test/Adder$Progress", DoNotGenerateAcw=true)]
public interface IAdderProgress : IJavaObject {
    [Register ("onAdd", "([III)V",
            "GetOnAddHandler:Mono.Samples.SanityTests.IAdderProgressInvoker, SanityTests, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")]
    void OnAdd (JavaArray<int> values, int currentIndex, int currentSum);
}
```

Beachten Sie im obigen Abschnitt, dass der Java `int[]` -Parameter einem [javaarray&lt;-&gt;int](xref:Android.Runtime.JavaArray`1)-Wert zugeordnet wird.
Dies ist nicht erforderlich: Wir hätten es möglicherweise an C# `int[]`ein, oder `IList<int>`an oder etwas anderes gebunden haben. Je nachdem, welcher Typ ausgewählt `Invoker` wird, muss er in der Lage sein, ihn `int[]` für den Aufruf in einen Java-Typ zu übersetzen.

### <a name="invoker-definition"></a>Invoker-Definition

Die `Invoker` Typdefinition muss erben `Java.Lang.Object`, die entsprechende Schnittstelle implementieren und alle Verbindungsmethoden bereitstellen, auf die in der Schnittstellen Definition verwiesen wird. Es gibt einen weiteren Vorschlag, der sich von einer Klassen Bindung unter `class_ref` scheidet: die Feld-und Methoden-IDs sollten Instanzmember und keine statischen Member sein.

Der Grund für das bevorzugen von Instanzmembern `JNIEnv.GetMethodID` besteht darin, das Verhalten in der Android-Laufzeit zu verwenden. (Dies kann auch Java-Verhalten sein, es wurde noch nicht getestet.) `JNIEnv.GetMethodID` gibt NULL zurück, wenn eine Methode gesucht wird, die von einer implementierten Schnittstelle stammt, und nicht von der deklarierten Schnittstelle Sehen Sie sich die Java [. util. SortedMap&lt;k&gt; , v](https://developer.android.com/reference/java/util/SortedMap.html) -Java-Schnittstelle an, die die Schnittstelle " [Java&gt; . util. map&lt;k, v](https://developer.android.com/reference/java/util/Map.html) " implementiert. Map bietet eine [klare](https://developer.android.com/reference/java/util/Map.html#clear()) Methode, daher würde eine scheinbar `Invoker` sinnvolle Definition für SortedMap lauten:

```csharp
// Fails at runtime. DO NOT FOLLOW
partial class ISortedMapInvoker : Java.Lang.Object, ISortedMap {
    static IntPtr class_ref = JNIEnv.FindClass ("java/util/SortedMap");
    static IntPtr id_clear;
    public void Clear()
    {
        if (id_clear == IntPtr.Zero)
            id_clear = JNIEnv.GetMethodID(class_ref, "clear", "()V");
        JNIEnv.CallVoidMethod(Handle, id_clear);
    }
     // ...
}
```

Der obige Fehler tritt auf `JNIEnv.GetMethodID` , weil `null` zurückgibt, wenn `Map.clear` die-Methode `SortedMap` über die-Klasseninstanz nachgeschlagen wird.

Hierfür gibt es zwei Lösungen: verfolgen Sie, von welcher Schnittstelle jede Methode stammt, und `class_ref` verfügen Sie über eine für jede Schnittstelle, oder behalten Sie alles als Instanzmember bei, und führen Sie die Methoden Suche für den am weitesten abgeleiteten Klassentyp aus, nicht den Schnittstellentyp. Letzteres erfolgt in **Mono. Android. dll**.

Die Invoker-Definition hat sechs Abschnitte: den Konstruktor, `Dispose` die-Methode `ThresholdType` , `ThresholdClass` die-und `GetObject` -Member, die-Methode, die-Schnittstellen Methoden Implementierung und die-Implementierung der-Connector-Methode.

#### <a name="constructor"></a>Konstruktor

Der Konstruktor muss die Lauf Zeit Klasse der aufgerufenen Instanz nachschlagen und die Lauf Zeit Klasse im Instanzfeld `class_ref` speichern:

```csharp
partial class IAdderProgressInvoker {
    IntPtr class_ref;
    public IAdderProgressInvoker (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass (Handle);
        class_ref   = JNIEnv.NewGlobalRef (lref);
        JNIEnv.DeleteLocalRef (lref);
    }
}
```

Hinweis: Die `Handle` -Eigenschaft muss innerhalb des konstruktortexts und nicht des `handle` -Parameters verwendet werden, da der `handle` Parameter möglicherweise ungültig ist, nachdem die Ausführung des basiskonstruktors abgeschlossen wurde.

#### <a name="dispose-method"></a>Dispose-Methode

Die `Dispose` -Methode muss den globalen Verweis freigeben, der im-Konstruktor zugeordnet ist:

```csharp
partial class IAdderProgressInvoker {
    protected override void Dispose (bool disposing)
    {
        if (this.class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef (this.class_ref);
        this.class_ref = IntPtr.Zero;
        base.Dispose (disposing);
    }
}
```

#### <a name="thresholdtype-and-thresholdclass"></a>Stammoldtype und stammoldclass

Das `ThresholdType` - `ThresholdClass` Element und das-Element sind identisch mit dem, was in einer Klassen Bindung gefunden wird:

```csharp
partial class IAdderProgressInvoker {
    protected override Type ThresholdType {
        get {
            return typeof (IAdderProgressInvoker);
        }
    }
    protected override IntPtr ThresholdClass {
        get {
            return class_ref;
        }
    }
}
```

#### <a name="getobject-method"></a>GetObject-Methode

Eine statische `GetObject` Methode ist erforderlich, um [Extensions. javacast&lt;T&gt;()](xref:Android.Runtime.Extensions.JavaCast*)zu unterstützen:

```csharp
partial class IAdderProgressInvoker {
    public static IAdderProgress GetObject (IntPtr handle, JniHandleOwnership transfer)
    {
        return new IAdderProgressInvoker (handle, transfer);
    }
}
```

#### <a name="interface-methods"></a>Schnittstellenmethoden

Jede Methode der Schnittstelle muss über eine-Implementierung verfügen, die die entsprechende Java-Methode über jni aufruft:

```csharp
partial class IAdderProgressInvoker {
    IntPtr id_onAdd;
    public void OnAdd (JavaArray<int> values, int currentIndex, int currentSum)
    {
        if (id_onAdd == IntPtr.Zero)
            id_onAdd = JNIEnv.GetMethodID (class_ref, "onAdd", "([III)V");
        JNIEnv.CallVoidMethod (Handle, id_onAdd, new JValue (JNIEnv.ToJniHandle (values)), new JValue (currentIndex), new JValue (currentSum));
    }
}
```

#### <a name="connector-methods"></a>Connector-Methoden

Die Connector-Methoden und die unterstützende Infrastruktur sind dafür verantwortlich, die jni-Parameter C# an geeignete Typen zu Mars Hallen. Der Java `int[]` -Parameter wird als jni `jintArray`übergeben, bei dem es sich `IntPtr` um C#eine innerhalb von handelt. Muss an einen `JavaArray<int>` gemarshallt werden, um das Aufrufen der C# -Schnittstelle zu unterstützen: `IntPtr`

```csharp
partial class IAdderProgressInvoker {
    static Delegate cb_onAdd;
    static Delegate GetOnAddHandler ()
    {
        if (cb_onAdd == null)
            cb_onAdd = JNINativeWrapper.CreateDelegate ((Action<IntPtr, IntPtr, IntPtr, int, int>) n_OnAdd);
        return cb_onAdd;
    }

    static void n_OnAdd (IntPtr jnienv, IntPtr lrefThis, IntPtr values, int currentIndex, int currentSum)
    {
        IAdderProgress __this = Java.Lang.Object.GetObject<IAdderProgress>(lrefThis, JniHandleOwnership.DoNotTransfer);
        using (var _values = new JavaArray<int>(values, JniHandleOwnership.DoNotTransfer)) {
            __this.OnAdd (_values, currentIndex, currentSum);
        }
    }
}
```

Wenn `int[]` von`JavaList<int>`bevorzugt wird, könnte stattdessen [jnienv. GetArray ()](xref:Android.Runtime.JNIEnv.GetArray*) verwendet werden:

```csharp
int[] _values = (int[]) JNIEnv.GetArray(values, JniHandleOwnership.DoNotTransfer, typeof (int));
```

Beachten Sie jedoch, dass `JNIEnv.GetArray` das gesamte Array zwischen virtuellen Computern kopiert, sodass für große Arrays dies zu einer viel zusätzlichen GC-Auslastung führen kann.

### <a name="complete-invoker-definition"></a>Vervollständigen der Invoker-Definition

Die [gesamte iadderprogressinvoker-Definition](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L88):

```csharp
class IAdderProgressInvoker : Java.Lang.Object, IAdderProgress {

    IntPtr class_ref;

    public IAdderProgressInvoker (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass (Handle);
        class_ref = JNIEnv.NewGlobalRef (lref);
        JNIEnv.DeleteLocalRef (lref);
    }

    protected override void Dispose (bool disposing)
    {
        if (this.class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef (this.class_ref);
        this.class_ref = IntPtr.Zero;
        base.Dispose (disposing);
    }

    protected override Type ThresholdType {
        get {return typeof (IAdderProgressInvoker);}
    }

    protected override IntPtr ThresholdClass {
        get {return class_ref;}
    }

    public static IAdderProgress GetObject (IntPtr handle, JniHandleOwnership transfer)
    {
        return new IAdderProgressInvoker (handle, transfer);
    }

#region OnAdd
    IntPtr id_onAdd;
    public void OnAdd (JavaArray<int> values, int currentIndex, int currentSum)
    {
        if (id_onAdd == IntPtr.Zero)
            id_onAdd = JNIEnv.GetMethodID (class_ref, "onAdd",
                    "([III)V");
        JNIEnv.CallVoidMethod (Handle, id_onAdd,
                new JValue (JNIEnv.ToJniHandle (values)),
                new JValue (currentIndex),
new JValue (currentSum));
    }

#pragma warning disable 0169
    static Delegate cb_onAdd;
    static Delegate GetOnAddHandler ()
    {
        if (cb_onAdd == null)
            cb_onAdd = JNINativeWrapper.CreateDelegate ((Action<IntPtr, IntPtr, IntPtr, int, int>) n_OnAdd);
        return cb_onAdd;
    }

    static void n_OnAdd (IntPtr jnienv, IntPtr lrefThis, IntPtr values, int currentIndex, int currentSum)
    {
        IAdderProgress __this = Java.Lang.Object.GetObject<IAdderProgress>(lrefThis, JniHandleOwnership.DoNotTransfer);
        using (var _values = new JavaArray<int>(values, JniHandleOwnership.DoNotTransfer)) {
            __this.OnAdd (_values, currentIndex, currentSum);
        }
    }
#pragma warning restore 0169
#endregion
}
```

## <a name="jni-object-references"></a>Jni-Objekt Verweise

Viele jnienv-Methoden geben *jni* - *Objekt Verweise*zurück, die `GCHandle`mit s vergleichbar sind. Jni bietet drei verschiedene Typen von Objekt verweisen: lokale Verweise, globale Verweise und schwache globale Verweise. Alle `System.IntPtr`drei werden als dargestellt, *aber* (gemäß dem Abschnitt jni-Funktionstypen) sind nicht `IntPtr`alle von `JNIEnv` -Methoden zurückgegebenen s Verweise. [Jnienv. getmethodid](xref:Android.Runtime.JNIEnv.GetMethodID*) gibt z. b. `IntPtr`zurück, aber es wird kein Objekt Verweis zurückgegeben. es `jmethodID`wird ein zurückgegeben. Weitere Informationen finden Sie in der [Dokumentation zur jni-Funktion](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html) .

Lokale Verweise werden von *den meisten* Verweis Erstellungs Methoden erstellt.
Android ermöglicht nur eine begrenzte Anzahl lokaler Verweise, die zu jedem beliebigen Zeitpunkt vorhanden sind, normalerweise 512. Lokale Verweise können mithilfe von " [jnienv. Delta](xref:Android.Runtime.JNIEnv.DeleteLocalRef*)Name" gelöscht werden.
Anders als bei jni werden nicht alle Reference-jnienv-Methoden, die Objekt Verweise zurückgeben, lokale Verweise zurückgeben. [Jnienv. FindClass](xref:Android.Runtime.JNIEnv.FindClass*) gibt einen *globalen* Verweis zurück. Es wird dringend empfohlen, lokale Verweise so schnell wie möglich zu löschen, indem Sie möglicherweise ein [java. lang. Object](xref:Java.Lang.Object) um das Objekt herum erstellen `JniHandleOwnership.TransferLocalRef` und für [java. lang. Object (IntPtr Handle, jnishandownership Transfer) angeben. ](xref:Java.Lang.Object#ctor*)-Konstruktor.

Globale Verweise werden von " [jnienv. newglobalref](xref:Android.Runtime.JNIEnv.NewGlobalRef*) " und " [jnienv. FindClass](xref:Android.Runtime.JNIEnv.FindClass*)" erstellt.
Sie können mit [jnienv. deleteglobalref](xref:Android.Runtime.JNIEnv.DeleteGlobalRef*)zerstört werden.
Emulatoren haben eine Beschränkung von 2.000 ausstehenden globalen verweisen, während Hardware Geräte eine Beschränkung von ungefähr 52.000 globalen verweisen haben.

Schwache globale Verweise sind nur unter Android v 2.2 (Froyo) und höher verfügbar. Schwache globale Verweise können mit [jnienv. deleteweakglobalref](xref:Android.Runtime.JNIEnv.DeleteWeakGlobalRef*)gelöscht werden.

### <a name="dealing-with-jni-local-references"></a>Umgang mit lokalen jni-verweisen

Die Methoden [jnienv. getobjectfield](xref:Android.Runtime.JNIEnv.GetObjectField*), [jnienv. getstaticobjectfield](xref:Android.Runtime.JNIEnv.GetStaticObjectField*), jnienv. [cdiskbjectmethod](xref:Android.Runtime.JNIEnv.CallObjectMethod*), [jnienv. callnonvirtualobjectmethod](xref:Android.Runtime.JNIEnv.CallNonvirtualObjectMethod*) und [jnienv. callstaticobjectmethod](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*) geben ein `IntPtr` -Objekt zurück, das enthält einen lokalen jni-Verweis auf ein Java-Objekt `IntPtr.Zero` oder, wenn `null`Java zurückgegeben wurde. Aufgrund der begrenzten Anzahl lokaler Verweise, die gleichzeitig (512 Einträge) ausstehen können, ist es wünschenswert, sicherzustellen, dass die Verweise rechtzeitig gelöscht werden. Es gibt drei Möglichkeiten, wie lokale Verweise behandelt werden können: Explizites löschen, erstellen `Java.Lang.Object` einer-Instanz, um Sie zu `Java.Lang.Object.GetObject<T>()` speichern, und Verwenden von, um einen verwalteten Aufruf baren Wrapper um diese zu erstellen.

### <a name="explicitly-deleting-local-references"></a>Explizites Löschen lokaler Verweise

[Jnienv. Delta](xref:Android.Runtime.JNIEnv.DeleteLocalRef*) Always on dient zum Löschen lokaler Verweise. Nachdem der lokale Verweis gelöscht wurde, kann er nicht mehr verwendet werden. Daher muss sichergestellt werden, dass `JNIEnv.DeleteLocalRef` der letzte Schritt mit dem lokalen Verweis erfolgt.

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
try {
    // Do something with `lref`
}
finally {
    JNIEnv.DeleteLocalRef (lref);
}
```

### <a name="wrapping-with-javalangobject"></a>Umwickeln mit Java. lang. Object

`Java.Lang.Object`stellt einen " [java. lang. Object"-Konstruktor (IntPtr Handle, jnishandownership Transfer)](xref:Java.Lang.Object#ctor*) bereit, der zum Umschließen eines beendenden jni-Verweises verwendet werden kann. Der [jnilenker Ownership](xref:Android.Runtime.JniHandleOwnership) -Parameter bestimmt, `IntPtr` wie der Parameter behandelt werden soll:

- [Jnilenker Ownership. donottransfer](xref:Android.Runtime.JniHandleOwnership.DoNotTransfer) &ndash; die erstellte `Java.Lang.Object` Instanz erstellt einen neuen globalen Verweis aus dem `handle` -Parameter und `handle` ist unverändert.
    Der Aufrufer ist für `handle` die Freigabe bei Bedarf verantwortlich.

- [Jnilenker Ownership. transferlocalref](xref:Android.Runtime.JniHandleOwnership.TransferLocalRef) &ndash; die erstellte `Java.Lang.Object` Instanz erstellt einen neuen globalen Verweis aus dem `handle` -Parameter und `handle` wird mit [jnienv. Delta-Ref](xref:Android.Runtime.JNIEnv.DeleteLocalRef*) gelöscht. Der Aufrufer darf `handle` nicht freigegeben werden und `handle` darf nicht verwendet werden, nachdem die Ausführung des Konstruktors abgeschlossen wurde.

- [Jnilenker Ownership. transferglobalref](xref:Android.Runtime.JniHandleOwnership.TransferLocalRef) &ndash; die erstellte `Java.Lang.Object` `handle` Instanz übernimmt den Besitz des Parameters. Der Aufrufer darf `handle` nicht freigeben.

Da die Methodenaufruf Methoden von jni lokale Verweise zurückgeben, `JniHandleOwnership.TransferLocalRef` würde normalerweise verwendet werden:

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef);
```

Der erstellte globale Verweis wird erst freigegeben, wenn `Java.Lang.Object` für die Instanz eine Garbage Collection durchgeführt wird. Wenn Sie in der Lage sind, die Instanz zu löschen, wird der globale Verweis freigegeben, sodass Garbage Collections beschleunigt werden:

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
using (var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef)) {
    // use value ...
}
```

### <a name="using-javalangobjectgetobjectlttgt"></a>Verwenden von Java. lang. Object.&lt;GetObject&gt;T ()

`Java.Lang.Object`stellt eine [java. lang. Object. GetObject&lt;T&gt;(IntPtr Handle, jnishandownership Transfer)](xref:Java.Lang.Object.GetObject*) -Methode bereit, die zum Erstellen eines verwalteten Aufruf baren Wrappers vom angegebenen Typ verwendet werden kann.

Der Typ `T` muss die folgenden Anforderungen erfüllen:

1. `T`muss ein Verweistyp sein.

1. `T`muss die `IJavaObject` -Schnittstelle implementieren.

1. Wenn `T` keine abstrakte Klasse oder Schnittstelle ist `T` , muss einen Konstruktor mit den Parametertypen `(IntPtr,
    JniHandleOwnership)` bereitstellen.

1. Wenn `T` eine abstrakte Klasse oder eine Schnittstelle ist, *muss* ein *aufrufende Instanz* für `T` verfügbar sein. Ein aufrufende Instanz ist ein nicht abstrakter Typ, der erbt `T` oder implementiert `T` und denselben Namen wie `T` ein aufrufende Instanz-Suffix hat. Wenn z. b. T die Schnitt `Java.Lang.IRunnable` Stelle ist, muss `Java.Lang.IRunnableInvoker` der Typ vorhanden sein und muss den `(IntPtr,
    JniHandleOwnership)` erforderlichen Konstruktor enthalten.

Da die Methodenaufruf Methoden von jni lokale Verweise zurückgeben, `JniHandleOwnership.TransferLocalRef` würde normalerweise verwendet werden:

```csharp
IntPtr lrefString = JNIEnv.CallObjectMethod(instance, methodID);
Java.Lang.String value = Java.Lang.Object.GetObject<Java.Lang.String>( lrefString, JniHandleOwnership.TransferLocalRef);
```

<a name="_Looking_up_Java_Types" />

## <a name="looking-up-java-types"></a>Suchen von Java-Typen

Zum Suchen eines Felds oder einer Methode in jni muss zuerst der deklarierende Typ für das Feld oder die Methode gesucht werden. Die [Android. Runtime. jnienv. FindClass (String)](xref:Android.Runtime.JNIEnv.FindClass*))-Methode wird verwendet, um Java-Typen zu suchen. Der String-Parameter ist der *vereinfachte Typverweis* oder der *vollständige Typverweis* für den Java-Typ. Weitere Informationen zu vereinfachten und vollständigen Typverweisen finden Sie im [Abschnitt jni-Typverweise](#_JNI_Type_References) .

Hinweis: Im Gegensatz zu `JNIEnv` jeder anderen Methode, die Objekt `FindClass` Instanzen zurückgibt, gibt einen globalen Verweis und keinen lokalen Verweis zurück.

<a name="_Instance_Fields" />

## <a name="instance-fields"></a>Instanzfelder

Felder werden durch *Feld-IDs*manipuliert. Feld-IDs werden über [jnienv. getfieldid abgerufen.](xref:Android.Runtime.JNIEnv.GetFieldID*)dies erfordert die Klasse, in der das Feld definiert ist, den Namen des Felds und die [jni-Typsignatur](#JNI_Type_Signatures) des Felds.

Feld-IDs müssen nicht freigegeben werden und sind gültig, solange der entsprechende Java-Typ geladen wurde. (Android unterstützt derzeit keine Entladen der Klasse.)

Es gibt zwei Sätze von Methoden zum Bearbeiten von Instanzfeldern: eine zum Lesen von Instanzfeldern und eine zum Schreiben von Instanzfeldern. Alle Sätze von Methoden erfordern eine Feld-ID, um den Feldwert zu lesen oder zu schreiben.

### <a name="reading-instance-field-values"></a>Lesen von instanzfeldwerten

Der Satz von Methoden zum Lesen von instanzfeldwerten folgt dem Benennungs Muster:

```csharp
* JNIEnv.Get*Field(IntPtr instance, IntPtr fieldID);
```

dabei `*` ist der Typ des Felds:

- [Jnienv. getobjectfield](xref:Android.Runtime.JNIEnv.GetObjectField*) &ndash; liest den Wert eines beliebigen Instanzfelds, das kein `java.lang.Object` Buildtyp ist, z. b. Arrays und Schnittstellentypen. Der zurückgegebene Wert ist ein lokaler jni-Verweis.

- [Jnienv. getbooleanfield](xref:Android.Runtime.JNIEnv.GetBooleanField*) &ndash; liest den Wert von `bool` Instanzfeldern.

- [Jnienv. GetByteField](xref:Android.Runtime.JNIEnv.GetByteField*) &ndash; liest den Wert von `sbyte` Instanzfeldern.

- [Jnienv. getcharfield](xref:Android.Runtime.JNIEnv.GetCharField*) &ndash; liest den Wert von `char` Instanzfeldern.

- [Jnienv. getshortfield](xref:Android.Runtime.JNIEnv.GetShortField*) &ndash; liest den Wert von `short` Instanzfeldern.

- [Jnienv. getintfield](xref:Android.Runtime.JNIEnv.GetIntField*) &ndash; liest den Wert von `int` Instanzfeldern.

- [Jnienv. getlongfield](xref:Android.Runtime.JNIEnv.GetLongField*) &ndash; liest den Wert von `long` Instanzfeldern.

- [Jnienv. getfloatfield](xref:Android.Runtime.JNIEnv.GetFloatField*) &ndash; liest den Wert von `float` Instanzfeldern.

- [Jnienv. getdoublefield](xref:Android.Runtime.JNIEnv.GetDoubleField*) &ndash; liest den Wert von `double` Instanzfeldern.

### <a name="writing-instance-field-values"></a>Schreiben von instanzfeldwerten

Der Satz von Methoden zum Schreiben von instanzfeldwerten folgt dem Benennungs Muster:

```csharp
JNIEnv.SetField(IntPtr instance, IntPtr fieldID, Type value);
```

Where *Type* ist der Typ des Felds:

- [Jnienv. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; schreiben Sie den Wert eines beliebigen Felds, bei dem es sich nicht um einen `java.lang.Object` Buildtyp handelt, z. b. Arrays und Schnittstellentypen. Bei `IntPtr` dem Wert kann es sich um einen lokalen jni-Verweis, einen globalen jni-Verweis, einen `IntPtr.Zero` globalen jni-Verweis oder um (for `null` ) handeln.

- [Jnienv. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; schreiben Sie den Wert `bool` von Instanzfeldern.

- [Jnienv. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; schreiben Sie den Wert `sbyte` von Instanzfeldern.

- [Jnienv. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; schreiben Sie den Wert `char` von Instanzfeldern.

- [Jnienv. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; schreiben Sie den Wert `short` von Instanzfeldern.

- [Jnienv. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; schreiben Sie den Wert `int` von Instanzfeldern.

- [Jnienv. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; schreiben Sie den Wert `long` von Instanzfeldern.

- [Jnienv. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; schreiben Sie den Wert `float` von Instanzfeldern.

- [Jnienv. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; schreiben Sie den Wert `double` von Instanzfeldern.

<a name="_Static_Fields" />

## <a name="static-fields"></a>Statische Felder

Statische Felder werden durch *Feld-IDs*manipuliert. Feld-IDs werden über [jnienv. getstaticfieldid abgerufen.](xref:Android.Runtime.JNIEnv.GetStaticFieldID*)dies erfordert die Klasse, in der das Feld definiert ist, den Namen des Felds und die [jni-Typsignatur](#JNI_Type_Signatures) des Felds.

Feld-IDs müssen nicht freigegeben werden und sind gültig, solange der entsprechende Java-Typ geladen wurde. (Android unterstützt derzeit keine Entladen der Klasse.)

Es gibt zwei Sätze von Methoden zum Bearbeiten statischer Felder: eine zum Lesen von Instanzfeldern und eine zum Schreiben von Instanzfeldern. Alle Sätze von Methoden erfordern eine Feld-ID, um den Feldwert zu lesen oder zu schreiben.

### <a name="reading-static-field-values"></a>Lesen von statischen Feldwerten

Der Satz von Methoden zum Lesen statischer Feldwerte folgt dem Benennungs Muster:

```csharp
* JNIEnv.GetStatic*Field(IntPtr class, IntPtr fieldID);
```

dabei `*` ist der Typ des Felds:

- [Jnienv. getstaticobjectfield](xref:Android.Runtime.JNIEnv.GetStaticObjectField*) &ndash; liest den Wert eines beliebigen statischen Felds, das kein `java.lang.Object` Buildtyp ist, z. b. Arrays und Schnittstellentypen. Der zurückgegebene Wert ist ein lokaler jni-Verweis.

- [Jnienv. getstaticbooleanfield](xref:Android.Runtime.JNIEnv.GetStaticBooleanField*) &ndash; `bool` liest den Wert statischer Felder.

- [Jnienv. getstaticbytefield](xref:Android.Runtime.JNIEnv.GetStaticByteField*) &ndash; `sbyte` liest den Wert statischer Felder.

- [Jnienv. getstaticcharfield](xref:Android.Runtime.JNIEnv.GetStaticCharField*) &ndash; `char` liest den Wert statischer Felder.

- [Jnienv. getstaticshortfield](xref:Android.Runtime.JNIEnv.GetStaticShortField*) &ndash; `short` liest den Wert statischer Felder.

- [Jnienv. getstaticlongfield](xref:Android.Runtime.JNIEnv.GetStaticLongField*) &ndash; `long` liest den Wert statischer Felder.

- [Jnienv. getstaticfloatfield](xref:Android.Runtime.JNIEnv.GetStaticFloatField*) &ndash; `float` liest den Wert statischer Felder.

- [Jnienv. getstaticdoublefield](xref:Android.Runtime.JNIEnv.GetStaticDoubleField*) &ndash; `double` liest den Wert statischer Felder.

### <a name="writing-static-field-values"></a>Schreiben statischer Feldwerte

Der Satz von Methoden zum Schreiben statischer Feldwerte folgt dem Benennungs Muster:

```csharp
JNIEnv.SetStaticField(IntPtr class, IntPtr fieldID, Type value);
```

Where *Type* ist der Typ des Felds:

- [Jnienv. SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; schreiben Sie den Wert eines beliebigen statischen Felds, das kein `java.lang.Object` Buildtyp ist, z. b. Arrays und Schnittstellentypen. Bei `IntPtr` dem Wert kann es sich um einen lokalen jni-Verweis, einen globalen jni-Verweis, einen `IntPtr.Zero` globalen jni-Verweis oder um (for `null` ) handeln.

- [Jnienv. SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; `bool` schreiben Sie den Wert statischer Felder.

- [Jnienv. SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; `sbyte` schreiben Sie den Wert statischer Felder.

- [Jnienv. SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; `char` schreiben Sie den Wert statischer Felder.

- [Jnienv. SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; `short` schreiben Sie den Wert statischer Felder.

- [Jnienv. SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; `int` schreiben Sie den Wert statischer Felder.

- [Jnienv. SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; `long` schreiben Sie den Wert statischer Felder.

- [Jnienv. SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; `float` schreiben Sie den Wert statischer Felder.

- [Jnienv. SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; `double` schreiben Sie den Wert statischer Felder.

<a name="_Instance_Methods" />

## <a name="instance-methods"></a>Instanzmethoden

Instanzmethoden werden über *Methoden-IDs*aufgerufen. Methoden-IDs werden über [jnienv. getmethodid abgerufen.](xref:Android.Runtime.JNIEnv.GetMethodID*)dies erfordert den Typ, in dem die Methode definiert ist, den Namen der Methode und die [jni-Typsignatur](#JNI_Type_Signatures) der Methode.

Methoden-IDs müssen nicht freigegeben werden und sind gültig, solange der entsprechende Java-Typ geladen wurde. (Android unterstützt derzeit keine Entladen der Klasse.)

Es gibt zwei Sätze von Methoden zum Aufrufen von Methoden: eine zum Aufrufen von Methoden und eine für das Aufrufen von Methoden, die nicht virtuell sind. Beide Sätze von Methoden erfordern eine Methoden-ID zum Aufrufen der-Methode, und für einen nicht virtuellen Aufruf ist es auch erforderlich, dass Sie angeben, welche Klassen Implementierung aufgerufen werden soll.

Schnittstellen Methoden können nur innerhalb des deklarierenden Typs gesucht werden. Methoden, die von erweiterten/geerbten Schnittstellen stammen, können nicht gesucht werden. Weitere Informationen finden Sie im Abschnitt spätere bindungsschnittstellen/Invoker-Implementierung.

Jede Methode, die in der-Klasse oder einer beliebigen Basisklasse oder implementierten Schnittstelle deklariert ist, kann gesucht werden.

### <a name="virtual-method-invocation"></a>Virtueller Methodenaufruf

Der Satz von Methoden zum Aufrufen von Methoden, die virtuell auf das Benennungs Muster folgen:

```csharp
* JNIEnv.Call*Method( IntPtr instance, IntPtr methodID, params JValue[] args );
```

wobei `*` der Rückgabetyp der Methode ist.

- [Jnienv. cverteilbjectmethod](xref:Android.Runtime.JNIEnv.CallObjectMethod*) &ndash; rufen Sie eine Methode auf, die einen nicht-Builtin- `java.lang.Object` Typ zurückgibt, z. b. Arrays und Schnittstellen. Der zurückgegebene Wert ist ein lokaler jni-Verweis.

- [Jnienv. callbooleanmethod](xref:Android.Runtime.JNIEnv.CallBooleanMethod*) &ndash; rufen Sie eine Methode auf, `bool` die einen Wert zurückgibt.

- [Jnienv. callbytemethod](xref:Android.Runtime.JNIEnv.CallByteMethod*) &ndash; rufen Sie eine Methode auf, `sbyte` die einen Wert zurückgibt.

- [Jnienv. callcharmethod](xref:Android.Runtime.JNIEnv.CallCharMethod*) &ndash; rufen Sie eine Methode auf, `char` die einen Wert zurückgibt.

- [Jnienv. callshortmethod](xref:Android.Runtime.JNIEnv.CallShortMethod*) &ndash; Ruft eine Methode auf, die `short` einen Wert zurückgibt.

- [Jnienv. calllongmethod](xref:Android.Runtime.JNIEnv.CallLongMethod*) &ndash; Ruft eine Methode auf, die `long` einen Wert zurückgibt.

- [Jnienv. callfloatmethod](xref:Android.Runtime.JNIEnv.CallFloatMethod*) &ndash; rufen Sie eine Methode auf, `float` die einen Wert zurückgibt.

- [Jnienv. calldoublemethod](xref:Android.Runtime.JNIEnv.CallDoubleMethod*) &ndash; Ruft eine Methode auf, die `double` einen Wert zurückgibt.

### <a name="non-virtual-method-invocation"></a>Aufruf der nicht virtuellen Methode

Der Satz von Methoden zum Aufrufen von Methoden, die nicht virtuell sind, folgt dem Benennungs Muster:

```csharp
* JNIEnv.CallNonvirtual*Method( IntPtr instance, IntPtr class, IntPtr methodID, params JValue[] args );
```

wobei `*` der Rückgabetyp der Methode ist. Der Aufruf der nicht virtuellen Methode wird normalerweise verwendet, um die Basis Methode einer virtuellen Methode aufzurufen.

- [Jnienv. callnonvirtualobjectmethod](xref:Android.Runtime.JNIEnv.CallNonvirtualObjectMethod*) &ndash; verwendet nicht virtuell eine Methode, die einen nicht-Builtin- `java.lang.Object` Typ zurückgibt, wie z. b. Arrays und Schnittstellen. Der zurückgegebene Wert ist ein lokaler jni-Verweis.

- [Jnienv. callnonvirtualbooleanmethod](xref:Android.Runtime.JNIEnv.CallNonvirtualBooleanMethod*) &ndash; nicht virtuell rufen Sie eine Methode auf, die `bool` einen Wert zurückgibt.

- [Jnienv. callnonvirtualbytemethod](xref:Android.Runtime.JNIEnv.CallNonvirtualByteMethod*) &ndash; nicht virtuell rufen Sie eine Methode auf, die `sbyte` einen Wert zurückgibt.

- [Jnienv. callnonvirtualcharmethod](xref:Android.Runtime.JNIEnv.CallNonvirtualCharMethod*) &ndash; nicht virtuell rufen Sie eine Methode auf, die `char` einen Wert zurückgibt.

- [Jnienv. callnonvirtualshortmethod](xref:Android.Runtime.JNIEnv.CallNonvirtualShortMethod*) &ndash; nicht virtuell rufen Sie eine Methode auf, die `short` einen Wert zurückgibt.

- [Jnienv. callnonvirtuallongmethod](xref:Android.Runtime.JNIEnv.CallNonvirtualLongMethod*) &ndash; nicht virtuell rufen Sie eine Methode auf, die `long` einen Wert zurückgibt.

- [Jnienv. callnonvirtualfloatmethod](xref:Android.Runtime.JNIEnv.CallNonvirtualFloatMethod*) &ndash; nicht virtuell rufen Sie eine Methode auf, die `float` einen Wert zurückgibt.

- [Jnienv. callnonvirtualdoublemethod](xref:Android.Runtime.JNIEnv.CallNonvirtualDoubleMethod*) &ndash; nicht virtuell rufen Sie eine Methode auf, die `double` einen Wert zurückgibt.

<a name="_Static_Methods" />

## <a name="static-methods"></a>Statische Methoden

Statische Methoden werden über Methoden- *IDs*aufgerufen. Methoden-IDs werden über [jnienv. getstaticmethodid abgerufen.](xref:Android.Runtime.JNIEnv.GetStaticMethodID*)dies erfordert den Typ, in dem die Methode definiert ist, den Namen der Methode und die [jni-Typsignatur](#JNI_Type_Signatures) der Methode.

Methoden-IDs müssen nicht freigegeben werden und sind gültig, solange der entsprechende Java-Typ geladen wurde. (Android unterstützt derzeit keine Entladen der Klasse.)

### <a name="static-method-invocation"></a>Statischer Methodenaufruf

Der Satz von Methoden zum Aufrufen von Methoden, die virtuell auf das Benennungs Muster folgen:

```csharp
* JNIEnv.CallStatic*Method( IntPtr class, IntPtr methodID, params JValue[] args );
```

wobei `*` der Rückgabetyp der Methode ist.

- [Jnienv. callstaticobjectmethod](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*) &ndash; rufen Sie eine statische Methode auf, die einen nicht-Builtin- `java.lang.Object` Typ zurückgibt, z. b. Arrays und Schnittstellen. Der zurückgegebene Wert ist ein lokaler jni-Verweis.

- [Jnienv. callstaticbooleanmethod](xref:Android.Runtime.JNIEnv.CallStaticBooleanMethod*) &ndash; rufen Sie eine statische Methode auf, `bool` die einen Wert zurückgibt.

- [Jnienv. callstaticbytemethod](xref:Android.Runtime.JNIEnv.CallStaticByteMethod*) &ndash; rufen Sie eine statische Methode auf, `sbyte` die einen Wert zurückgibt.

- [Jnienv. callstaticcharmethod](xref:Android.Runtime.JNIEnv.CallStaticCharMethod*) &ndash; rufen Sie eine statische Methode auf, `char` die einen Wert zurückgibt.

- [Jnienv. callstaticshortmethod](xref:Android.Runtime.JNIEnv.CallStaticShortMethod*) &ndash; Ruft eine statische Methode auf, die `short` einen Wert zurückgibt.

- [Jnienv. callstaticlongmethod](xref:Android.Runtime.JNIEnv.CallLongMethod*) &ndash; Ruft eine statische Methode auf, die `long` einen Wert zurückgibt.

- [Jnienv. callstaticfloatmethod](xref:Android.Runtime.JNIEnv.CallStaticFloatMethod*) &ndash; rufen Sie eine statische Methode auf, `float` die einen Wert zurückgibt.

- [Jnienv. callstaticdoublemethod](xref:Android.Runtime.JNIEnv.CallStaticDoubleMethod*) &ndash; rufen Sie eine statische Methode auf, `double` die einen Wert zurückgibt.

<a name="JNI_Type_Signatures" />

## <a name="jni-type-signatures"></a>Jni-Typsignaturen

[Jni-Typsignaturen](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/types.html#wp16432) sind [jni-Typverweise](#_JNI_Type_References) (obwohl nicht vereinfachte Typverweise), mit Ausnahme der Methoden. Bei Methoden ist die jni-Typsignatur eine öffnende Klammer `'('`, gefolgt von den Typverweisen für alle Parametertypen, die zusammen verkettet werden (ohne Trennzeichen oder sonstige), gefolgt von einer schließenden `')'`Klammer, gefolgt von der jni-Typreferenz des Methoden Rückgabe Typs.

Beispielsweise mit der Java-Methode:

```java
long f(int n, String s, int[] array);
```

Die jni-Typsignatur lautet wie folgt:

```csharp
(ILjava/lang/String;[I)J
```

Im Allgemeinen wird *dringend* empfohlen, den `javap` Befehl zu verwenden, um jni-Signaturen zu bestimmen. Beispielsweise ist die jni-Typsignatur der [java. lang. Thread. State. valueOf (String)](https://developer.android.com/reference/java/lang/Thread.State.html#valueOf(java.lang.String)) -Methode "(Ljava/lang/String;) Ljava/lang/Thread $ State;", während die jni-Typsignatur der [java. lang. Thread. State. Values](https://developer.android.com/reference/java/lang/Thread.State.html#values) -Methode "() [Ljava/ lang/Thread $ State; ". Achten Sie auf die nachfolgenden Semikolons. Diese *sind* Teil der jni-Typsignatur.

<a name="_JNI_Type_References" />

## <a name="jni-type-references"></a>Jni-Typverweise

Jni-Typverweise unterscheiden sich von Java-Typverweisen. Sie können keine voll qualifizierten Java-Typnamen wie `java.lang.String` z. b. mit jni verwenden. stattdessen müssen Sie `"java/lang/String"` die `"Ljava/lang/String;"`jni-Variationen oder abhängig vom Kontext verwenden. Weitere Informationen finden Sie weiter unten.
Es gibt vier Typen von jni-Typverweisen:

- **built-in**
- **simplified**
- **Typ**
- **array**

### <a name="built-in-type-references"></a>Integrierte Typverweise

Integrierte Typverweise sind ein einzelnes Zeichen, das verwendet wird, um auf integrierte Werttypen zu verweisen. Die Zuordnung lautet wie folgt:

- `"B"`für `sbyte` .
- `"S"`für `short` .
- `"I"`für `int` .
- `"J"`für `long` .
- `"F"`für `float` .
- `"D"`für `double` .
- `"C"`für `char` .
- `"Z"`für `bool` .
- `"V"`für `void` Methoden Rückgabe Typen.

<a name="_Simplified_Type_References_1" />

### <a name="simplified-type-references"></a>Vereinfachte Typverweise

Vereinfachte Typverweise können nur in [jnienv. FindClass (String)](xref:Android.Runtime.JNIEnv.FindClass*)) verwendet werden.
Es gibt zwei Möglichkeiten, einen vereinfachten Typverweis abzuleiten:

1. Ersetzen Sie unter einem voll qualifizierten Java-Namen alle `'.'` innerhalb des Paket namens und vor dem Typnamen durch `'/'` und alle `'.'` innerhalb eines Typnamens mit `'$'` .

1. Lesen Sie die Ausgabe `'unzip -l android.jar | grep JavaName'` von.

Beide Ergebnisse führen dazu, dass der Java-Typ [java. lang. Thread. State](https://developer.android.com/reference/java/lang/Thread.State.html) dem vereinfachten Typverweis `java/lang/Thread$State`zugeordnet wird.

### <a name="type-references"></a>Typverweise

Ein Typverweis ist ein integrierter Typverweis oder ein vereinfachter Typverweis mit einem `'L'` Präfix und einem `';'` Suffix. Für den Java-Typ [java. lang. String](https://developer.android.com/reference/java/lang/String.html)ist `"java/lang/String"`der vereinfachte Typverweis, während der Typverweis ist `"Ljava/lang/String;"`.

Typverweise werden mit Array-Typverweisen und mit jni-Signaturen verwendet.

Eine zusätzliche Möglichkeit zum Abrufen eines Typverweises ist das Lesen der Ausgabe `'javap -s -classpath android.jar fully.qualified.Java.Name'`von.
Abhängig vom Typ können Sie eine Konstruktordeklaration oder einen Methoden Rückgabetyp verwenden, um den jni-Namen zu bestimmen. Beispiel:

```shell
$ javap -classpath android.jar -s java.lang.Thread.State
Compiled from "Thread.java"
```

```java
public final class java.lang.Thread$State extends java.lang.Enum{
public static final java.lang.Thread$State NEW;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State RUNNABLE;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State BLOCKED;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State WAITING;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State TIMED_WAITING;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State TERMINATED;
  Signature: Ljava/lang/Thread$State;
public static java.lang.Thread$State[] values();
  Signature: ()[Ljava/lang/Thread$State;
public static java.lang.Thread$State valueOf(java.lang.String);
  Signature: (Ljava/lang/String;)Ljava/lang/Thread$State;
static {};
  Signature: ()V
}
```

`Thread.State`ist ein Java-Enumeration-Typ. Daher können wir die Signatur der `valueOf` -Methode verwenden, um zu bestimmen, ob der Typverweis Ljava/lang/Thread $ State; ist.

### <a name="array-type-references"></a>Array-Typverweise

Array-Typverweise `'['` werden einem jni-Typverweis vorangestellt.
Vereinfachte Typverweise können beim Angeben von Arrays nicht verwendet werden.

Beispiels `int[]` Weise ist `"[I"`, `int[][]` ist ,und`java.lang.Object[]` ist .`"[Ljava/lang/Object;"` `"[[I"`

## <a name="java-generics-and-type-erasure"></a>Java-Generika und typlöschung

In *den meisten* Fällen sind Java-Generika *nicht vorhanden*, wie Sie über jni sichtbar sind.
Es gibt einige "Falten", aber diese Falten bestehen darin, wie Java mit Generika interagiert, nicht mit der Art und Weise, wie jni sucht und generische Member aufruft.

Bei der Interaktion mit jni gibt es keinen Unterschied zwischen einem generischen Typ oder Member und einem nicht generischen Typ oder Member. Der generische Typ " [java. lang.&lt;Class T&gt; ](https://developer.android.com/reference/java/lang/Class.html) " ist z. b. auch der generische Typ `java.lang.Class`"RAW", der `"java/lang/Class"`beide den gleichen vereinfachten Typverweis hat.

## <a name="java-native-interface-support"></a>Unterstützung von Java Native Interface

[Android. Runtime. jnienv](xref:Android.Runtime.JNIEnv) ist ein verwalteter Wrapper für die Jave Native Interface (JNI). Jni-Funktionen werden innerhalb der [Java Native Interface-Spezifikation](http://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html)deklariert, obwohl die Methoden so geändert wurden, dass `JNIEnv*` Sie den `IntPtr` expliziten Parameter entfernen, und `jmethodID`anstelle von `jobject`, `jclass`,, etc. Sehen Sie sich beispielsweise die [jni NewObject-Funktion](http://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp4517)an:

```csharp
jobject NewObjectA(JNIEnv *env, jclass clazz, jmethodID methodID, jvalue *args);
```

Dies wird als [jnienv. NewObject](xref:Android.Runtime.JNIEnv.NewObject*) -Methode verfügbar gemacht:

```csharp
public static IntPtr NewObject(IntPtr clazz, IntPtr jmethod, params JValue[] parms);
```

Die Übersetzung zwischen den beiden Aufrufen ist recht unkompliziert. In C haben Sie Folgendes:

```c
jobject CreateMapActivity(JNIEnv *env)
{
    jclass    Map_Class   = (*env)->FindClass(env, "mono/samples/googlemaps/MyMapActivity");
    jmethodID Map_defCtor = (*env)->GetMethodID (env, Map_Class, "<init>", "()V");
    jobject   instance    = (*env)->NewObject (env, Map_Class, Map_defCtor);

    return instance;
}
```

Die C# Entsprechung lautet wie folgt:

```csharp
IntPtr CreateMapActivity()
{
    IntPtr Map_Class   = JNIEnv.FindClass ("mono/samples/googlemaps/MyMapActivity");
    IntPtr Map_defCtor = JNIEnv.GetMethodID (Map_Class, "<init>", "()V");
    IntPtr instance    = JNIEnv.NewObject (Map_Class, Map_defCtor);

    return instance;
}
```

Wenn Sie über eine Java-Objektinstanz verfügen, die in einem IntPtr enthalten ist, möchten Sie wahrscheinlich etwas damit tun. Hierfür können Sie jnienv-Methoden wie [jnienv. callvoidmethod ()](xref:Android.Runtime.JNIEnv.CallVoidMethod*) verwenden. Wenn jedoch bereits ein analoger C# Wrapper vorhanden ist, sollten Sie einen Wrapper über die jni-Referenz erstellen. Dies können Sie über die Erweiterungsmethode [Extensions.\<javacast T >](xref:Android.Runtime.Extensions.JavaCast*) :

```csharp
IntPtr lrefActivity = CreateMapActivity();

// imagine that Activity were instead an interface or abstract type...
Activity mapActivity = new Java.Lang.Object(lrefActivity, JniHandleOwnership.TransferLocalRef)
    .JavaCast<Activity>();
```

Sie können auch die [java. lang. Object.\<GetObject T >](xref:Java.Lang.Object.GetObject*) -Methode verwenden:

```csharp
IntPtr lrefActivity = CreateMapActivity();

// imagine that Activity were instead an interface or abstract type...
Activity mapActivity = Java.Lang.Object.GetObject<Activity>(lrefActivity, JniHandleOwnership.TransferLocalRef);
```

Außerdem wurden alle jni-Funktionen geändert, indem der `JNIEnv*` in jeder jni-Funktion vorhandene Parameter entfernt wurde.

## <a name="summary"></a>Zusammenfassung

Der direkte Umgang mit jni ist ein schreckliches Verfahren, das bei allen Kosten vermieden werden sollte. Leider ist dies nicht immer vermeidbar. hoffentlich erhalten Sie in diesem Handbuch einige Unterstützung, wenn Sie die ungebundenen Java-Fälle mit Mono für Android erreichen.

## <a name="related-links"></a>Verwandte Links

- [Java Native Interface-Spezifikation](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/jniTOC.html)
- [Java Native Interface-Funktionen](http://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html)
