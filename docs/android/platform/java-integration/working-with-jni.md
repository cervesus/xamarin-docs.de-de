---
title: Arbeiten mit JNI und xamarin. Android
description: Xamarin. Android ermöglicht das Schreiben von Android C# -apps mit anstelle von Java. Mit xamarin. Android werden mehrere Assemblys bereitgestellt, die Bindungen für Java-Bibliotheken bereitstellen, einschließlich Mono. Android. dll und Mono. Android. GoogleMaps. dll. Allerdings werden Bindungen nicht für jede mögliche Java-Bibliothek bereitgestellt, und die bereitgestellten Bindungen binden möglicherweise nicht alle Java-Typen und-Member. Um ungebundene Java-Typen und-Member zu verwenden, kann die Java Native Interface (JNI) verwendet werden. In diesem Artikel wird veranschaulicht, wie jni verwendet wird, um mit Java-Typen und-Membern von xamarin. Android-Anwendungen zu interagieren.
ms.prod: xamarin
ms.assetid: A417DEE9-7B7B-4E35-A79C-284739E3838E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 0fa717a775ff2f1ace9e248a8afde8d373e8a1f8
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724344"
---
# <a name="working-with-jni-and-xamarinandroid"></a>Arbeiten mit JNI und xamarin. Android

_Xamarin. Android ermöglicht das Schreiben von Android C# -apps mit anstelle von Java. Mit xamarin. Android werden mehrere Assemblys bereitgestellt, die Bindungen für Java-Bibliotheken bereitstellen, einschließlich Mono. Android. dll und Mono. Android. GoogleMaps. dll. Allerdings werden Bindungen nicht für jede mögliche Java-Bibliothek bereitgestellt, und die bereitgestellten Bindungen binden möglicherweise nicht alle Java-Typen und-Member. Um ungebundene Java-Typen und-Member zu verwenden, kann die Java Native Interface (JNI) verwendet werden. In diesem Artikel wird veranschaulicht, wie jni verwendet wird, um mit Java-Typen und-Membern von xamarin. Android-Anwendungen zu interagieren._

## <a name="overview"></a>Übersicht

Es ist nicht immer erforderlich, einen verwalteten Callable Wrapper (MCW) zum Aufrufen von Java-Code zu erstellen. In vielen Fällen ist "Inline"-jni durchaus akzeptabel und nützlich für die einmalige Verwendung ungebundener Java-Member. Es ist häufig einfacher, JNI zu verwenden, um eine einzelne Methode in einer Java-Klasse aufzurufen, als eine gesamte jar-Bindung zu generieren.

Xamarin. Android stellt die `Mono.Android.dll`-Assembly bereit, die eine Bindung für die `android.jar`-Bibliothek von Android bereitstellt. Typen und Member, die nicht in `Mono.Android.dll` vorhanden sind, und Typen, die nicht in `android.jar` vorhanden sind, können verwendet werden, wenn Sie manuell gebunden Zum Binden von Java-Typen und-Membern verwenden Sie die **Java Native Interface** (**jni**), um Typen zu suchen, Felder zu lesen und zu schreiben und Methoden aufzurufen.

Die jni-API in xamarin. Android ist konzeptionell sehr ähnlich wie die `System.Reflection`-API in .net: Sie ermöglicht es Ihnen, Typen und Member nach Name, Lese-und Schreib Feldwerten, Methoden aufrufen und mehr zu suchen. Mit JNI und dem benutzerdefinierten `Android.Runtime.RegisterAttribute`-Attribut können Sie virtuelle Methoden deklarieren, die an das Überschreiben von Unterstützung gebunden werden können. Sie können Schnittstellen binden, sodass Sie in C#implementiert werden können.

In diesem Dokument wird Folgendes erläutert:

- Gibt an, wie jni auf Typen verweist.
- Vorgehensweise beim Suchen, lesen und Schreiben von Feldern
- Gewusst wie: Suchen und Aufrufen von Methoden
- So machen Sie virtuelle Methoden verfügbar, um das Überschreiben von verwaltetem Code zuzulassen.
- Verfügbar machen von Schnittstellen.

## <a name="requirements"></a>Requirements (Anforderungen)

Jni ist in jeder Version von xamarin. Android verfügbar, die durch den [Android. Runtime. jnienv-Namespace](xref:Android.Runtime.JNIEnv)verfügbar gemacht wird.
Zum Binden von Java-Typen und-Schnittstellen müssen Sie xamarin. Android 4,0 oder höher verwenden.

## <a name="managed-callable-wrappers"></a>Verwaltete Callable Wrapper

Ein **verwalteter Callable Wrapper** (**MCW**) ist eine *Bindung* für eine Java-Klasse oder-Schnittstelle, die alle jni-Maschinen umschließt, sodass sich der Client C# Code nicht um die zugrunde liegende Komplexität von jni kümmern muss. Die meisten `Mono.Android.dll` bestehen aus verwalteten Callable Wrappern.

Verwaltete Callable Wrapper dienen zwei Zwecken:

1. Kapseln Sie die jni-Verwendung, damit der Client Code die zugrunde liegende Komplexität nicht kennen muss.
1. Ermöglicht die Unterklasse von Java-Typen und die Implementierung von Java-Schnittstellen.

Der erste Zweck besteht lediglich aus Gründen der leichteren und Kapselung der Komplexität, damit Consumer über einen einfachen, verwalteten Satz von Klassen verfügen, die verwendet werden können. Hierfür müssen die verschiedenen [jnienv](xref:Android.Runtime.JNIEnv) -Member verwendet werden, wie weiter unten in diesem Artikel beschrieben. Beachten Sie, dass verwaltete Callable Wrapper nicht unbedingt erforderlich sind &ndash; "Inline"-jni-Verwendung ist durchaus akzeptabel und eignet sich für die einmalige Verwendung ungebundener Java-Member. Die Unterklassen-und Schnittstellen Implementierung erfordert die Verwendung verwalteter Aufruf barer Wrapper.

## <a name="android-callable-wrappers"></a>Android Callable Wrapper

Android Callable Wrapper (ACW) sind immer dann erforderlich, wenn die Android-Laufzeit (Art) verwalteten Code aufrufen muss. Diese Wrapper sind erforderlich, da es keine Möglichkeit gibt, Klassen mit Kunst zur Laufzeit zu registrieren.
(Insbesondere die Funktion [defineClass](https://docs.oracle.com/javase/6/docs/technotes/guides/jni/spec/functions.html#wp15986) jni wird von der Android-Laufzeit nicht unterstützt. Android Callable Wrapper bilden daher die fehlende Unterstützung für die Laufzeit-Typregistrierung.)

Wenn Android-Code eine virtuelle oder Schnittstellen Methode ausführen muss, die überschrieben oder in verwaltetem Code implementiert wird, muss xamarin. Android einen Java-Proxy bereitstellen, damit diese Methode an den entsprechenden verwalteten Typ weitergeleitet wird. Diese Java-Proxy Typen sind Java-Code, der die "gleiche" Basisklasse und Java-Schnittstellen Liste wie der verwaltete Typ hat, wobei dieselben Konstruktoren implementiert werden und alle überschriebenen Basisklassen-und Schnittstellen Methoden deklariert werden.

Android Callable Wrapper werden während des [Buildprozesses](~/android/deploy-test/building-apps/build-process.md)durch das **monodroid. exe** -Programm generiert und für alle Typen generiert, die (direkt oder indirekt) [java. lang. Object](xref:Java.Lang.Object)erben.

### <a name="implementing-interfaces"></a>Implementieren von Schnittstellen

Es gibt Zeiten, in denen Sie möglicherweise eine Android-Schnittstelle implementieren müssen (z. b [. Android. Content. icomponentcallbacks](xref:Android.Content.IComponentCallbacks)).

Alle Android-Klassen und-Schnittstellen erweitern die [Android. Runtime. ijavaobject](xref:Android.Runtime.IJavaObject) -Schnittstelle. Daher müssen alle Android-Typen `IJavaObject`implementieren.
Xamarin. Android nutzt diese Tatsache &ndash; `IJavaObject` zum Bereitstellen von Android mit einem Java-Proxy (einem Android Callable Wrapper) für den angegebenen verwalteten Typ verwendet wird. Da **monodroid. exe** nur nach `Java.Lang.Object` Unterklassen sucht (die `IJavaObject`implementieren müssen), bietet die Unterklassen `Java.Lang.Object` uns eine Möglichkeit, Schnittstellen in verwaltetem Code zu implementieren. Beispiel:

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

### <a name="implementation-details"></a>Details zur Implementierung

*Der Rest dieses Artikels enthält Implementierungsdetails, die sich ohne vorherige Ankündigung ändern* können (und wird hier nur hier vorgestellt, da Entwickler möglicherweise neugierig sind, was im Hintergrund passiert).

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

- Die [java. IO. serialisierbare](https://developer.android.com/reference/java/io/Serializable.html) Schnittstelle erfordert `readObject`-und `writeObject`-Methoden. Da es sich nicht um Member dieser Schnittstelle handelt, werden diese Methoden von der entsprechenden verwalteten Implementierung nicht für Java-Code verfügbar gemacht.

- Die [Android. OS. Parser](xref:Android.OS.Parcelable) -Schnittstelle erwartet, dass eine Implementierungs Klasse ein statisches Feld `CREATOR` vom Typ `Parcelable.Creator`haben muss. Der generierte Java-Code erfordert ein explizites Feld. In unserem Standardszenario gibt es keine Möglichkeit, ein Feld in Java-Code aus verwaltetem Code auszugeben.

Da die Codegenerierung keine Projekt Mappe bereitstellt, um beliebige Java-Methoden mit beliebigen Namen zu generieren, beginnend mit xamarin. Android 4,2, wurden [Export Attribute](xref:Java.Interop.ExportAttribute) und [exportfieldattribute](xref:Java.Interop.ExportFieldAttribute) eingeführt, um eine Lösung für die oben genannten Szenarien zu bieten. Beide Attribute befinden sich im `Java.Interop`-Namespace:

- `ExportAttribute` &ndash; gibt einen Methodennamen und seine erwarteten Ausnahme Typen an (um explizites "auslösen" in Java zu geben). Wenn Sie für eine Methode verwendet wird, wird von der Methode eine Java-Methode exportiert, die einen dispatchcode für den entsprechenden jni-Aufruf der verwalteten Methode generiert. Dies kann mit `android:onClick` und `java.io.Serializable`verwendet werden.

- `ExportFieldAttribute` &ndash; einen Feldnamen angibt. Sie befindet sich auf einer Methode, die als Feldinitialisierer fungiert. Dies kann mit `android.os.Parcelable`verwendet werden.

#### <a name="troubleshooting-exportattribute-and-exportfieldattribute"></a>Problembehandlung bei Export Attribute und exportfieldattribute

- Die Paket Erstellung schlägt fehl, da **Mono. Android. Export. dll** fehlt &ndash; Wenn Sie `ExportAttribute` oder `ExportFieldAttribute` für einige Methoden in Ihrem Code oder abhängigen Bibliotheken verwendet haben, müssen Sie **Mono. Android. Export. dll**hinzufügen. Diese Assembly ist für die Unterstützung von Rückruf Code von Java isoliert. Es ist von **Mono. Android. dll** getrennt, da es der Anwendung zusätzliche Größe hinzufügt.

- Im Releasebuild findet `MissingMethodException` für Export Methoden &ndash; im Releasebuild `MissingMethodException` für Export Methoden statt. (Dieses Problem ist in der neuesten Version von xamarin. Android behoben.)

### <a name="exportparameterattribute"></a>ExportParameterAttribute

`ExportAttribute` und `ExportFieldAttribute` stellen Funktionen bereit, die von Java-Lauf Zeit Code verwendet werden können. Dieser Lauf Zeit Code greift über die generierten jni-Methoden, die von diesen Attributen gesteuert werden, auf verwalteten Code zu. Folglich gibt es keine vorhandene Java-Methode, die von der verwalteten Methode gebunden wird. Daher wird die Java-Methode aus der Signatur einer verwalteten Methode generiert.

Dieser Fall ist jedoch nicht vollständig Determinant. Dies trifft insbesondere auf einige erweiterte Zuordnungen zwischen verwalteten Typen und Java-Typen zu, wie z. b.:

- InputStream
- OutputStream
- XmlPullParser
- XmlResourceParser

Wenn Typen wie diese für exportierte Methoden erforderlich sind, muss der `ExportParameterAttribute` verwendet werden, um den entsprechenden Parameter oder Rückgabewert explizit einem Typ zuzuweisen.

### <a name="annotation-attribute"></a>Annotation-Attribut

In xamarin. Android 4,2 haben wir `IAnnotation` Implementierungs Typen in Attribute (System. Attribute) konvertiert und Unterstützung für die Anmerkung-Generierung in Java-Wrapper hinzugefügt.

Dies bedeutet, dass die folgenden Richtungsänderungen vorgenommen werden:

- Der Bindungs Generator generiert `Java.Lang.DeprecatedAttribute` aus `java.Lang.Deprecated` (während er in verwaltetem Code `[Obsolete]` werden sollte).

- Dies bedeutet nicht, dass vorhandene `Java.Lang.Deprecated` Klasse verschwindet. Diese Java-basierten Objekte können weiterhin als normale Java-Objekte verwendet werden (wenn eine solche Verwendung vorhanden ist). Es gibt `Deprecated`-und `DeprecatedAttribute`-Klassen.

- Die `Java.Lang.DeprecatedAttribute`-Klasse ist als `[Annotation]` markiert. Wenn ein benutzerdefiniertes Attribut vorhanden ist, das von diesem `[Annotation]` Attribut geerbt wird, generiert die MSBuild-Aufgabe eine Java-Anmerkung für dieses benutzerdefinierte Attribut (@Deprecated) im Android Callable Wrapper (ACW).

- Anmerkungen können auf Klassen, Methoden und exportierten Feldern (eine Methode in verwaltetem Code) generiert werden.

Wenn die enthaltende Klasse (die mit Anmerkungen versehene Klasse) oder die Klasse, die die mit Anmerkungen versehene Member enthält, nicht registriert ist, wird die gesamte Java-Klassen Quelle überhaupt nicht generiert, einschließlich der Anmerkungen. Für Methoden können Sie die `ExportAttribute` angeben, um die Methode explizit zu generieren und mit Anmerkungen versehen. Außerdem handelt es sich nicht um eine Funktion zum "generieren" einer Java-Anmerkung-Klassendefinition. Anders ausgedrückt: Wenn Sie ein benutzerdefiniertes verwaltetes Attribut für eine bestimmte Anmerkung definieren, müssen Sie eine andere jar-Bibliothek hinzufügen, die die entsprechende Java-Anmerkung-Klasse enthält. Das Hinzufügen einer Java-Quelldatei, die den Anmerkung-Typ definiert, ist nicht ausreichend. Der Java-Compiler funktioniert nicht auf die gleiche Weise wie **apt**.

Außerdem gelten die folgenden Einschränkungen:

- Dieser Konvertierungsprozess berücksichtigt bisher keine `@Target` Anmerkung für den Anmerkung-Typ.

- Attribute auf eine Eigenschaft können nicht verwendet werden. Verwenden Sie stattdessen Attribute für den Eigenschaften Getter oder Setter.

## <a name="class-binding"></a>Klassen Bindung

Das Binden einer Klasse bedeutet, dass ein verwalteter Aufruf barer Wrapper geschrieben wird, um den Aufruf des zugrunde liegenden Java-Typs zu vereinfachen.

Das Binden von virtuellen und abstrakten Methoden, um C# das Überschreiben von zuzulassen, erfordert xamarin. Android 4,0. Allerdings kann jede Version von xamarin. Android nicht virtuelle Methoden, statische Methoden oder virtuelle Methoden binden, ohne außer Kraft setzungen zu unterstützen.

Eine Bindung enthält in der Regel die folgenden Elemente:

- Ein [jni-Handle für den Java-Typ, der gebunden wird](#_Looking_up_Java_Types).

- [Jni-Feld-IDs und-Eigenschaften für jedes gebundene Feld](#_Instance_Fields).

- [Jni-Methoden-IDs und-Methoden für jede gebundene Methode](#_Instance_Methods).

- Wenn Unterklassen erforderlich sind, muss der Typ über ein benutzerdefiniertes [RegisterAttribute](xref:Android.Runtime.RegisterAttribute) -Attribut für die Typdeklaration verfügen, bei dem " [RegisterAttribute. donotgenerateacw](xref:Android.Runtime.RegisterAttribute.DoNotGenerateAcw) " auf "`true`" festgelegt ist.

### <a name="declaring-type-handle"></a>Deklarieren des typhandles

Für die Methoden Suchmethoden für Felder und Methoden ist ein Objekt Verweis erforderlich, der auf Ihren deklarierenden Typ verweist. Gemäß der Konvention wird dies in einem `class_ref` Feld gespeichert:

```csharp
static IntPtr class_ref = JNIEnv.FindClass(CLASS);
```

Ausführliche Informationen zum `CLASS` Token finden Sie im Abschnitt [jni-Typverweise](#_JNI_Type_References) .

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

Hinweis: Wir verwenden [inputstreaminvoker. fromjnihandle](xref:Android.Runtime.InputStreamInvoker.FromJniHandle*) , um den jni-Verweis in eine `System.IO.Stream` Instanz zu konvertieren, und wir verwenden `JniHandleOwnership.TransferLocalRef`, da [jnienv. getstaticobjectfield](xref:Android.Runtime.JNIEnv.GetStaticObjectField*) einen lokalen Verweis zurückgibt.

Viele der [Android. Runtime](xref:Android.Runtime) -Typen verfügen über `FromJniHandle`-Methoden, mit denen ein jni-Verweis in den gewünschten Typ konvertiert wird.

### <a name="method-binding"></a>Methoden Bindung

Java-Methoden werden als C# Methoden und als C# Eigenschaften verfügbar gemacht. Die Java-Methode java [. lang. Runtime. runFinalizersOnExit](https://developer.android.com/reference/java/lang/Runtime.html#runFinalizersOnExit(boolean)) -Methode ist beispielsweise als [java. lang. Runtime. runFinalizersOnExit](xref:Java.Lang.Runtime.RunFinalizersOnExit*) -Methode gebunden, und die [java. lang. Object. GetClass](https://developer.android.com/reference/java/lang/Object.html#getClass) -Methode ist als die [java. lang. Object. Class](xref:Java.Lang.Object.Class) -Eigenschaft gebunden.

Der Methodenaufruf ist ein zweistufiger Prozess:

1. Die *Get-Methoden-ID* für die aufzurufende Methode. Die Methode " *get Method ID* " ist für das Zurückgeben eines Methoden Handles zuständig, das von den Methodenaufruf Methoden verwendet wird. Zum Abrufen der Methoden-ID müssen Sie den deklarierenden Typ, den Namen der Methode und die [jni-Typsignatur](#JNI_Type_Signatures) der Methode kennen.

1. Rufen Sie die Methode auf.

Ebenso wie bei Feldern unterscheiden sich die Methoden, die zum Aufrufen der Methoden-ID und Aufrufen der Methode verwendet werden, von statischen Methoden und Instanzmethoden.

[Statische Methoden](#_Static_Methods_1) verwenden [jnienv. getstaticmethodid ()](xref:Android.Runtime.JNIEnv.GetStaticMethodID*) , um die Methoden-ID zu suchen, und verwenden die `JNIEnv.CallStatic*Method` Familie von Methoden für den Aufruf.

[Instanzmethoden](#_Instance_Methods) verwenden [jnienv. getmethodid](xref:Android.Runtime.JNIEnv.GetMethodID*) , um die Methoden-ID zu suchen, und verwenden die `JNIEnv.Call*Method`-und `JNIEnv.CallNonvirtual*Method` Familien von Methoden für den Aufruf.

Die Methoden Bindung ist möglicherweise mehr als nur der Methodenaufruf. Die Methoden Bindung umfasst auch das zulassen, dass eine Methode überschrieben wird (für abstrakte und nicht abschließende Methoden) oder implementiert (für Schnittstellen Methoden). Der Abschnitt [unterstützende Vererbung, Schnittstellen](#_Supporting_Inheritance,_Interfaces_1) behandelt die Komplexität der Unterstützung virtueller Methoden und Schnittstellen Methoden.

<a name="_Static_Methods_1" />

#### <a name="static-methods"></a>Statische Methoden

Das Binden einer statischen Methode beinhaltet die Verwendung von `JNIEnv.GetStaticMethodID`, um ein Methoden Handle abzurufen, und anschließend die entsprechende `JNIEnv.CallStatic*Method` Methode abhängig vom Rückgabetyp der Methode zu verwenden. Im folgenden finden Sie ein Beispiel für eine Bindung für die [Runtime. GetRuntime](https://developer.android.com/reference/java/lang/Runtime.html#getRuntime()) -Methode:

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

Beachten Sie, dass das Methoden Handle in einem statischen Feld `id_getRuntime`gespeichert wird. Dies ist eine Leistungsoptimierung, sodass der Methoden Handle nicht bei jedem Aufruf gesucht werden muss. Es ist nicht erforderlich, das Methoden Handle auf diese Weise zwischenzuspeichern. Nachdem das Methoden Handle abgerufen wurde, wird [jnienv. callstaticobjectmethod](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*) zum Aufrufen der-Methode verwendet. `JNIEnv.CallStaticObjectMethod` gibt eine `IntPtr` zurück, die das Handle der zurückgegebenen Java-Instanz enthält.
[Java. lang. Object. GetObject&lt;t&gt;(IntPtr, jnishandownership)](xref:Java.Lang.Object.GetObject*) wird verwendet, um das Java-Handle in eine stark typisierte Objektinstanz zu konvertieren.

#### <a name="non-virtual-instance-method-binding"></a>Methoden Bindung für nicht virtuelle Instanzen

Das Binden einer `final` Instanzmethode oder einer Instanzmethode, die keine Überschreibung erfordert, umfasst die Verwendung von `JNIEnv.GetMethodID`, um ein Methoden Handle abzurufen. Anschließend wird die entsprechende `JNIEnv.Call*Method`-Methode verwendet, je nach Rückgabetyp der Methode. Im folgenden finden Sie ein Beispiel für eine Bindung für die `Object.Class`-Eigenschaft:

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

Beachten Sie, dass das Methoden Handle in einem statischen Feld `id_getClass`gespeichert wird.
Dies ist eine Leistungsoptimierung, sodass der Methoden Handle nicht bei jedem Aufruf gesucht werden muss. Es ist nicht erforderlich, das Methoden Handle auf diese Weise zwischenzuspeichern. Nachdem das Methoden Handle abgerufen wurde, wird [jnienv. callstaticobjectmethod](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*) zum Aufrufen der-Methode verwendet. `JNIEnv.CallStaticObjectMethod` gibt eine `IntPtr` zurück, die das Handle der zurückgegebenen Java-Instanz enthält.
[Java. lang. Object. GetObject&lt;t&gt;(IntPtr, jnishandownership)](xref:Java.Lang.Object.GetObject*) wird verwendet, um das Java-Handle in eine stark typisierte Objektinstanz zu konvertieren.

### <a name="binding-constructors"></a>Bindungskonstruktoren

Konstruktoren sind Java-Methoden mit dem Namen `"<init>"`. Ebenso wie bei Java-Instanzmethoden wird `JNIEnv.GetMethodID` verwendet, um nach dem konstruktorhandle zu suchen. Im Gegensatz zu Java-Methoden werden die [jnienv. NewObject](xref:Android.Runtime.JNIEnv.NewObject*) -Methoden verwendet, um das konstruktormethodenhandle aufzurufen. Der Rückgabewert von `JNIEnv.NewObject` ist ein lokaler jni-Verweis:

```csharp
int value = 42;
IntPtr class_ref    = JNIEnv.FindClass ("java/lang/Integer");
IntPtr id_ctor_I    = JNIEnv.GetMethodID (class_ref, "<init>", "(I)V");
IntPtr lrefInstance = JNIEnv.NewObject (class_ref, id_ctor_I, new JValue (value));
// Dispose of lrefInstance, class_ref…
```

Normalerweise wird eine Klassen Bindung " [java. lang. Object](xref:Java.Lang.Object)" Unterklassen.
Beim Unterklassen `Java.Lang.Object`wird eine zusätzliche Semantik ins Spiel genommen: eine `Java.Lang.Object` Instanz verwaltet einen globalen Verweis auf eine Java-Instanz über die `Java.Lang.Object.Handle`-Eigenschaft.

1. Der `Java.Lang.Object` Standardkonstruktor weist eine Java-Instanz zu.

1. Wenn der Typ eine `RegisterAttribute` hat und `RegisterAttribute.DoNotGenerateAcw` `true` ist, wird eine Instanz des `RegisterAttribute.Name` Typs über den Standardkonstruktor erstellt.

1. Andernfalls wird der [Android Callable Wrapper](~/android/platform/java-integration/android-callable-wrappers.md) (ACW), der `this.GetType` entspricht, über seinen Standardkonstruktor instanziiert. Android Callable Wrapper werden während der Paket Erstellung für jede `Java.Lang.Object` Unterklasse generiert, für die `RegisterAttribute.DoNotGenerateAcw` nicht auf `true`festgelegt ist.

Bei Typen, bei denen es sich nicht um Klassen Bindungen handelt, ist dies die erwartete Semantik C# : das Instanziieren einer `Mono.Samples.HelloWorld.HelloAndroid` Instanz sollte eine Java-`mono.samples.helloworld.HelloAndroid` Instanz erstellen, bei der es sich um einen generierten Android Callable Wrapper

Bei Klassen Bindungen kann dies das richtige Verhalten sein, wenn der Java-Typ einen Standardkonstruktor enthält und/oder kein anderer Konstruktor aufgerufen werden muss. Andernfalls muss ein Konstruktor bereitgestellt werden, der die folgenden Aktionen ausführt:

1. Aufrufen von " [java. lang. Object" (IntPtr, jnilenker Ownership)](xref:Java.Lang.Object#ctor*) anstelle des Standardkonstruktors `Java.Lang.Object`. Dies ist erforderlich, um das Erstellen einer neuen Java-Instanz zu vermeiden.

1. Überprüfen Sie den Wert von [java. lang. Object. handle](xref:Java.Lang.Object.Handle) , bevor Sie Java-Instanzen erstellen. Die `Object.Handle`-Eigenschaft hat einen anderen Wert als `IntPtr.Zero`, wenn ein von Android Callable Wrapper in Java-Code erstellt wurde, und die Klassen Bindung wird erstellt, um die erstellte Android Callable Wrapper-Instanz zu enthalten. Wenn Android z. b. eine `mono.samples.helloworld.HelloAndroid` Instanz erstellt, wird der Android Callable Wrapper zuerst erstellt, und der Java-`HelloAndroid`-Konstruktor erstellt eine Instanz des entsprechenden `Mono.Samples.HelloWorld.HelloAndroid` Typs, wobei die `Object.Handle`-Eigenschaft vor der Konstruktorausführung auf die Java-Instanz festgelegt wird.

1. Wenn der aktuelle Lauf Zeittyp nicht mit dem deklarierenden Typ identisch ist, muss eine Instanz des entsprechenden Android Callable-Wrappers erstellt werden. verwenden Sie [Object. SetHandle](xref:Java.Lang.Object.SetHandle*) , um das von [jnienv. kreateinstance](xref:Android.Runtime.JNIEnv.CreateInstance*)zurückgegebene Handle zu speichern.

1. Wenn der aktuelle Lauf Zeittyp mit dem deklarierenden Typ identisch ist, rufen Sie den Java-Konstruktor auf, und verwenden Sie [Object. SetHandle](xref:Java.Lang.Object.SetHandle*) , um das von `JNIEnv.NewInstance` zurückgegebene Handle zu speichern.

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

Die [jnienv. kreatabstance](xref:Android.Runtime.JNIEnv.CreateInstance*) -Methoden sind Hilfsprogramme zum Durchführen einer `JNIEnv.FindClass`, `JNIEnv.GetMethodID`, `JNIEnv.NewObject`und `JNIEnv.DeleteGlobalReference` für den Wert, der von `JNIEnv.FindClass`zurückgegeben wird. Weitere Details finden Sie im nächsten Abschnitt.

<a name="_Supporting_Inheritance,_Interfaces_1" />

### <a name="supporting-inheritance-interfaces"></a>Unterstützende Vererbung, Schnittstellen

Zum Unterklassen eines Java-Typs oder zum Implementieren einer Java-Schnittstelle ist die Generierung von [Android Callable Wrappern](~/android/platform/java-integration/android-callable-wrappers.md) (acws) erforderlich, die während des Verpackungsprozesses für jede `Java.Lang.Object` Unterklasse generiert werden. Die ACW-Generierung wird durch das benutzerdefinierte Attribut " [Android. Runtime. RegisterAttribute](xref:Android.Runtime.RegisterAttribute) " gesteuert.

Für C# -Typen erfordert der `[Register]` benutzerdefinierte Attributkonstruktor ein Argument: den [jni-vereinfachten Typverweis](#_Simplified_Type_References_1) für den entsprechenden Java-Typ. Dadurch können unterschiedliche Namen zwischen Java und C#bereitgestellt werden.

Vor xamarin. Android 4,0 war das benutzerdefinierte Attribut `[Register]` nicht für "Alias" für vorhandene Java-Typen verfügbar. Dies liegt daran, dass der ACW-Generierungsprozess acws für jede gefundene `Java.Lang.Object` Unterklasse generieren würde.

Xamarin. Android 4,0 führte die [RegisterAttribute. donotgenerateacw](xref:Android.Runtime.RegisterAttribute.DoNotGenerateAcw) -Eigenschaft ein. Diese Eigenschaft weist den ACW-Generierungsprozess an, den mit Anmerkungen versehenen Typ zu über *springen* . Dies ermöglicht die Deklaration neuer verwalteter Aufruf barer Wrapper, die zum Zeitpunkt der Paket Erstellung nicht zum Generieren von acws führen. Dadurch können vorhandene Java-Typen gebunden werden. Sehen Sie sich zum Beispiel die folgende einfache Java-Klasse an, `Adder`, die eine Methode enthält, `add`, die zu ganzen Zahlen hinzufügt und das Ergebnis zurückgibt:

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

Hier gibt der `Adder` C# Typ den `Adder` Java-Typ *Aliase* an. Das `[Register]`-Attribut wird verwendet, um den jni-Namen des `mono.android.test.Adder` Java-Typs anzugeben, und die `DoNotGenerateAcw`-Eigenschaft wird verwendet, um die ACW-Generierung zu verhindern. Dies führt zur Generierung eines ACW für den `ManagedAdder`-Typ, der den `mono.android.test.Adder` Typ ordnungsgemäß Unterklassen unterteilt. Wenn die `RegisterAttribute.DoNotGenerateAcw`-Eigenschaft nicht verwendet wurde, hätte der xamarin. Android-Buildprozess einen neuen `mono.android.test.Adder` Java-Typ generiert. Dies würde zu Kompilierungs Fehlern führen, da der `mono.android.test.Adder` Typ zweimal in zwei separaten Dateien vorhanden wäre.

### <a name="binding-virtual-methods"></a>Binden von virtuellen Methoden

`ManagedAdder` Unterklassen den Java-`Adder` Typ, aber es ist nicht besonders interessant C# : der `Adder` Typ definiert keine virtuellen Methoden, sodass `ManagedAdder` nichts überschreiben können.

Die Bindung `virtual` Methoden, die das Überschreiben durch Unterklassen zulassen, erfordert einige Dinge, die in die folgenden beiden Kategorien fallen:

1. **Methoden Bindung**

1. **Methoden Registrierung**

#### <a name="method-binding"></a>Methoden Bindung

Eine Methoden Bindung erfordert das Hinzufügen von zwei Unterstützungs Membern C# zur `Adder` Definition: `ThresholdType`und `ThresholdClass`.

##### <a name="thresholdtype"></a>ThresholdType

Die `ThresholdType`-Eigenschaft gibt den aktuellen Typ der Bindung zurück:

```csharp
partial class Adder {
    protected override System.Type ThresholdType {
        get {
            return typeof (Adder);
        }
    }
}
```

`ThresholdType` wird in der Methoden Bindung verwendet, um zu bestimmen, wann eine virtuelle und nicht virtuelle Methoden Verteilung durchgeführt werden soll. Er sollte immer eine `System.Type`-Instanz zurückgeben, die dem C# deklarierenden Typ entspricht.

##### <a name="thresholdclass"></a>ThresholdClass

Die `ThresholdClass`-Eigenschaft gibt den jni-Klassen Verweis für den gebundenen Typ zurück:

```csharp
partial class Adder {
    protected override IntPtr ThresholdClass {
        get {
            return class_ref;
        }
    }
}
```

`ThresholdClass` wird in der Methoden Bindung verwendet, wenn nicht virtuelle Methoden aufgerufen werden.

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

Das `id_add`-Feld enthält die Methoden-ID für die aufzurufende Java-Methode. Der `id_add` Wert wird von `JNIEnv.GetMethodID`abgerufen, der die deklarierende Klasse (`class_ref`), den Namen der Java-Methode (`"add"`) und die jni-Signatur der Methode (`"(II)I"`) erfordert.

Nachdem die Methoden-ID abgerufen wurde, wird `GetType` mit `ThresholdType` verglichen, um zu bestimmen, ob eine virtuelle oder eine nicht virtuelle Verteilung erforderlich ist. Die virtuelle Verteilung ist erforderlich, wenn `GetType` mit `ThresholdType`übereinstimmt, da `Handle` möglicherweise auf eine Java-zugeordnete Unterklasse verweist, die die-Methode überschreibt.

Wenn `GetType` nicht `ThresholdType`entspricht, wurde `Adder` untergeordnet (z. b. durch `ManagedAdder`), und die `Adder.Add`-Implementierung wird nur aufgerufen, wenn die Unterklasse `base.Add`aufgerufen hat. Dabei handelt es sich um den nicht virtuellen dispatchfall, in den `ThresholdClass` kommt. `ThresholdClass` gibt an, welche Java-Klasse die Implementierung der aufzurufenden Methode bereitstellt.

#### <a name="method-registration"></a>Methoden Registrierung

Angenommen, wir haben eine aktualisierte `ManagedAdder` Definition, die die `Adder.Add`-Methode überschreibt:

```csharp
partial class ManagedAdder : Adder {
    public override int Add (int a, int b) {
        return (a*2) + (b*2);
    }
}
```

Beachten Sie, dass `Adder.Add` über ein `[Register]` benutzerdefiniertes Attribut verfügte:

```csharp
[Register ("add", "(II)I", "GetAddHandler")]
```

Der `[Register]` benutzerdefinierte Attributkonstruktor akzeptiert drei Werte:

1. Der Name der Java-Methode, `"add"` in diesem Fall.

1. Die jni-Typsignatur der Methode, `"(II)I"` in diesem Fall.

1. Die *Connector-Methode* , `GetAddHandler` in diesem Fall.
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

Beachten Sie, dass eine `@Override`-Methode deklariert wird, die an eine `n_`Präfix-Methode mit demselben Namen delegiert. Dadurch wird sichergestellt, dass beim Aufrufen von `ManagedAdder.add`von Java-Code `ManagedAdder.n_add` aufgerufen wird, sodass C# die über schreibende `ManagedAdder.Add` Methode ausgeführt werden kann.

Daher ist die wichtigste Frage: wie wird `ManagedAdder.n_add` mit `ManagedAdder.Add`verknüpft?

Java-`native` Methoden werden über die [Funktion "jni registernatives](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp17734)" bei der Java-Laufzeit (Android Runtime) registriert.
`RegisterNatives` nimmt ein Array von Strukturen an, das den Namen der Java-Methode, die jni-Typsignatur und einen Funktionszeiger enthält, der die [jni-Aufruf Konvention](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/design.html#wp715)befolgt.
Der Funktionszeiger muss eine Funktion sein, die zwei Zeigerargumente annimmt, gefolgt von den Methoden Parametern. Die Java-`ManagedAdder.n_add`-Methode muss über eine Funktion implementiert werden, die über den folgenden C-Prototyp verfügt:

```csharp
int FunctionName(JNIEnv *env, jobject this, int a, int b)
```

Xamarin. Android macht keine `RegisterNatives` Methode verfügbar. Stattdessen stellen der ACW und der MCW die erforderlichen Informationen zum Aufrufen `RegisterNatives`bereit: der ACW enthält den Methodennamen und die jni-Typsignatur. das einzige fehlende Ergebnis ist ein Funktionszeiger, der verknüpft werden kann.

An dieser Stelle kommt die *Connector-Methode* ins Spiel. Der dritte `[Register]` Parameter des benutzerdefinierten Attributs ist der Name einer Methode, die im registrierten Typ definiert ist, oder eine Basisklasse des registrierten Typs, die keine Parameter annimmt und eine `System.Delegate`zurückgibt. Der zurückgegebene `System.Delegate` wiederum verweist auf eine Methode, die über die korrekte jni-Funktions Signatur verfügt. Schließlich *muss* der Delegat, der von der Connector-Methode zurückgegeben wird, den Stamm aufweisen, damit der GC ihn nicht sammelt, da der Delegat Java bereitgestellt wird.

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

Die `GetAddHandler`-Methode erstellt einen `Func<IntPtr, IntPtr, int, int,
int>` Delegaten, der auf die `n_Add`-Methode verweist, und ruft dann [jninativewrapper. kreatedelegat](xref:Android.Runtime.JNINativeWrapper.CreateDelegate*)auf.
`JNINativeWrapper.CreateDelegate` umschließt die angegebene Methode in einem try/catch-Block, sodass alle nicht behandelten Ausnahmen behandelt werden, und führt dazu, dass das Ereignis " [androidevent. unlenker dexceptionraiser](xref:Android.Runtime.AndroidEnvironment.UnhandledExceptionRaiser) " ausgelöst wird. Der resultierende Delegat wird in der statischen `cb_add` Variablen gespeichert, sodass der Garbage Collector von der GC nicht freigegeben wird.

Zum Schluss ist die `n_Add`-Methode für das Mars Hallen der jni-Parameter an die entsprechenden verwalteten Typen und die anschließende Delegierung des Methoden Aufrufes verantwortlich.

Hinweis: Verwenden Sie immer `JniHandleOwnership.DoNotTransfer`, wenn Sie eine MCW über eine Java-Instanz erhalten. Wenn Sie Sie als lokaler Verweis behandeln (und damit `JNIEnv.DeleteLocalRef`), werden verwaltete Stapel Übergänge mit verwaltetem&gt;&gt; unterbunden.

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

### <a name="restrictions"></a>Beschränkungen

Beim Schreiben eines Typs, der mit den folgenden Kriterien übereinstimmt:

1. Unterklassen `Java.Lang.Object`

1. Hat ein benutzerdefiniertes `[Register]`-Attribut

1. `RegisterAttribute.DoNotGenerateAcw` ist `true`.

Bei der GC-Interaktion darf der Typ *nicht* über Felder verfügen, die zur Laufzeit auf eine `Java.Lang.Object` oder `Java.Lang.Object` Unterklasse verweisen können. Beispielsweise sind Felder vom Typ `System.Object` und alle Schnittstellentypen nicht zulässig. Typen, die nicht auf `Java.Lang.Object` Instanzen verweisen können, sind zulässig, z. b. `System.String` und `List<int>`. Diese Einschränkung besteht darin, eine vorzeitige Objekt Auflistung durch die GC zu verhindern.

Wenn der Typ ein Instanzfeld enthalten muss, das auf eine `Java.Lang.Object` Instanz verweisen kann, muss der Feldtyp `System.WeakReference` oder `GCHandle`sein.

## <a name="binding-abstract-methods"></a>Binden abstrakter Methoden

Bindungs `abstract` Methoden sind größtenteils identisch mit der Bindung virtueller Methoden. Es gibt nur zwei Unterschiede:

1. Die abstrakte Methode ist abstrakt. Es behält weiterhin das `[Register]`-Attribut und die zugeordnete Methoden Registrierung bei, die Methoden Bindung wird lediglich in den `Invoker`-Typ verschoben.

1. Es wird ein nicht `abstract` `Invoker` Typ erstellt, in dem der abstrakte Typ Unterklassen erstellt wird. Der `Invoker` Typ muss alle abstrakten Methoden überschreiben, die in der Basisklasse deklariert sind, und die überschriebene Implementierung ist die Implementierung der Methoden Bindung, obwohl der nicht virtuelle dispatchfall ignoriert werden kann.

Nehmen Sie beispielsweise an, dass die oben genannte `mono.android.test.Adder.add`-Methode `abstract`wurde. Die C# Bindung würde sich ändern, sodass `Adder.Add` abstrakt sind, und es wird ein neuer `AdderInvoker` Typ definiert, der `Adder.Add`implementiert hat:

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

Der `Invoker` Typ ist nur erforderlich, wenn jni-Verweise auf von Java erstellte Instanzen erhalten werden.

## <a name="binding-interfaces"></a>Bindungsschnittstellen

Bindungsschnittstellen sind konzeptionell vergleichbar mit Bindungs Klassen, die virtuelle Methoden enthalten, aber viele der Besonderheiten unterscheiden sich in der subtilen (und nicht so geringfügigen) Art. Beachten Sie die folgende [Java-Schnittstellen Deklaration](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/Adder.java#L14):

```csharp
public interface Progress {
    void onAdd(int[] values, int currentIndex, int currentSum);
}
```

Schnittstellen Bindungen bestehen aus zwei Teilen: C# der Schnittstellen Definition und einer invokerdefinition für die-Schnittstelle.

### <a name="interface-definition"></a>Schnittstellendefinition

Die C# Schnittstellen Definition muss die folgenden Anforderungen erfüllen:

- Die Schnittstellen Definition muss über ein `[Register]` benutzerdefiniertes Attribut verfügen.

- Die Schnittstellen Definition muss die `IJavaObject interface`erweitern.
    Wenn dies nicht der Fall ist, wird verhindert, dass acws von der Java-Schnittstelle erbt.

- Jede Schnittstellen Methode muss ein `[Register]` Attribut enthalten, das den Namen der entsprechenden Java-Methode, die jni-Signatur und die Connector-Methode angibt.

- Die Connector-Methode muss auch den Typ angeben, auf dem sich die Connector-Methode befinden kann.

Beim Binden von `abstract`-und `virtual` Methoden wird die Connector-Methode innerhalb der Vererbungs Hierarchie des registrierten Typs durchsucht. Schnittstellen können keine Methoden enthalten, die Text enthalten, sodass dies nicht funktioniert. Daher muss ein Typ angegeben werden, der angibt, wo sich die Connector-Methode befindet. Der Typ wird in der Zeichenfolge der Verbindungs Zeichenfolge nach einem Doppelpunkt `':'`angegeben und muss der durch die Assembly qualifizierte Typname des Typs sein, der den Invoker enthält.

Schnittstellen Methoden Deklarationen sind eine Übersetzung der entsprechenden Java-Methode unter Verwendung *kompatibler* Typen. Bei Java-Builtin-Typen sind die kompatiblen Typen die C# entsprechenden Typen, z. b. C# Java-`int` `int`. Bei Verweis Typen ist der kompatible Typ ein Typ, der ein jni-Handle des entsprechenden Java-Typs bereitstellen kann.

Die Schnittstellenmember werden nicht direkt von Java aufgerufen, &ndash; Aufrufe durch den Invoker-Typ vermittelt werden &ndash; damit ein gewisses Maß an Flexibilität zulässig ist.

Die Java-Status Schnittstelle kann [ C# in wie folgt deklariert](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L83)werden:

```csharp
[Register ("mono/android/test/Adder$Progress", DoNotGenerateAcw=true)]
public interface IAdderProgress : IJavaObject {
    [Register ("onAdd", "([III)V",
            "GetOnAddHandler:Mono.Samples.SanityTests.IAdderProgressInvoker, SanityTests, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")]
    void OnAdd (JavaArray<int> values, int currentIndex, int currentSum);
}
```

Beachten Sie oben, dass der Java-`int[]`-Parameter einem [javaarray&lt;int-&gt;](xref:Android.Runtime.JavaArray`1)zugeordnet wird.
Dies ist nicht erforderlich: Wir könnten es an eine C# `int[]`, eine `IList<int>`oder etwas anderes gebunden haben. Je nachdem, welcher Typ ausgewählt wird, muss der `Invoker` in der Lage sein, ihn in einen Java-`int[]` Typ für den Aufruf zu übersetzen.

### <a name="invoker-definition"></a>Invoker-Definition

Die `Invoker` Typdefinition muss `Java.Lang.Object`erben, die entsprechende Schnittstelle implementieren und alle Verbindungsmethoden bereitstellen, auf die in der Schnittstellen Definition verwiesen wird. Es gibt einen weiteren Vorschlag, der sich von einer Klassen Bindung unterscheidet: die `class_ref` Feld-und Methoden-IDs sollten Instanzmember und keine statischen Member sein.

Der Grund für das bevorzugen von Instanzmembern besteht darin, `JNIEnv.GetMethodID` Verhalten in der Android-Laufzeit zu verwenden. (Dies kann auch Java-Verhalten sein, es wurde noch nicht getestet.) `JNIEnv.GetMethodID` gibt bei der Suche nach einer Methode, die von einer implementierten Schnittstelle stammt, und nicht der deklarierten Schnittstelle NULL zurück. Sehen Sie sich die Java [. util. SortedMap-&lt;k, v&gt;](https://developer.android.com/reference/java/util/SortedMap.html) Java-Schnittstelle an, die die Schnittstelle " [java. util. map&lt;K, v&gt;](https://developer.android.com/reference/java/util/Map.html) " implementiert. Map bietet eine [klare](https://developer.android.com/reference/java/util/Map.html#clear()) Methode, daher wäre eine scheinbar sinnvolle `Invoker` Definition für SortedMap Folgendes:

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

Der obige Fehler schlägt fehl, da `JNIEnv.GetMethodID` `null` zurückgibt, wenn die `Map.clear`-Methode über die `SortedMap` Klasseninstanz nachgeschlagen wird.

Hierfür gibt es zwei Lösungen: nachverfolgen, von welcher Schnittstelle jede Methode stammt, und über eine `class_ref` für jede Schnittstelle, oder Sie behalten alles als Instanzmember bei und führen die Methoden Suche für den am weitesten abgeleiteten Klassentyp und nicht für den Schnittstellentyp aus. Letzteres erfolgt in **Mono. Android. dll**.

Die Invoker-Definition hat sechs Abschnitte: den Konstruktor, die `Dispose`-Methode, die `ThresholdType` und `ThresholdClass` Member, die `GetObject`-Methode, die Implementierung der Schnittstellen Methode und die Implementierung der Connector-Methode.

#### <a name="constructor"></a>Konstruktor

Der Konstruktor muss die Lauf Zeit Klasse der aufgerufenen Instanz nachschlagen und die Lauf Zeit Klasse im Feld Instanz `class_ref` speichern:

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

Hinweis: die `Handle`-Eigenschaft muss innerhalb des konstruktortexts verwendet werden, und nicht der `handle` Parameter, wie bei Android v 4.0 ist der `handle`-Parameter möglicherweise ungültig, nachdem die Ausführung des basiskonstruktors abgeschlossen wurde.

#### <a name="dispose-method"></a>Dispose-Methode

Die `Dispose`-Methode muss den globalen Verweis freigeben, der im-Konstruktor zugeordnet ist:

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

Die `ThresholdType`-und `ThresholdClass` Member sind identisch mit den in einer Klassen Bindung gefundenen Elementen:

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

Eine statische `GetObject` Methode ist erforderlich, um [Extensions. javacast&lt;t&gt;()](xref:Android.Runtime.Extensions.JavaCast*)zu unterstützen:

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

Die Connector-Methoden und die unterstützende Infrastruktur sind dafür verantwortlich, die jni-Parameter C# an geeignete Typen zu Mars Hallen. Der Java-`int[]` Parameter wird als jni-`jintArray`übergeben, bei dem es sich um C#einen `IntPtr` in handelt. Der `IntPtr` muss an einen `JavaArray<int>` gemarshallt werden, um das Aufrufen der C# -Schnittstelle zu unterstützen:

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

Wenn `int[]` als `JavaList<int>`bevorzugt wird, könnte stattdessen [jnienv. GetArray ()](xref:Android.Runtime.JNIEnv.GetArray*) verwendet werden:

```csharp
int[] _values = (int[]) JNIEnv.GetArray(values, JniHandleOwnership.DoNotTransfer, typeof (int));
```

Beachten Sie jedoch, dass `JNIEnv.GetArray` das gesamte Array zwischen VMS kopiert. bei großen Arrays könnte dies also zu vielen zusätzlichen GC-Auslastung führen.

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

Viele jnienv-Methoden geben *jni* - *Objekt Verweise*zurück, die `GCHandle`s ähneln. Jni bietet drei verschiedene Typen von Objekt verweisen: lokale Verweise, globale Verweise und schwache globale Verweise. Alle drei werden als `System.IntPtr`dargestellt, jedoch nicht alle `IntPtr`s, die von `JNIEnv` Methoden zurück *gegeben werden,* sind Verweise. [Jnienv. getmethodid](xref:Android.Runtime.JNIEnv.GetMethodID*) gibt z. b. einen `IntPtr`zurück, aber es wird kein Objekt Verweis zurückgegeben. es wird ein `jmethodID`zurückgegeben. Weitere Informationen finden Sie in der [Dokumentation zur jni-Funktion](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html) .

Lokale Verweise werden von *den meisten* Verweis Erstellungs Methoden erstellt.
Android ermöglicht nur eine begrenzte Anzahl lokaler Verweise, die zu jedem beliebigen Zeitpunkt vorhanden sind, normalerweise 512. Lokale Verweise können mithilfe von " [jnienv. Delta](xref:Android.Runtime.JNIEnv.DeleteLocalRef*)Name" gelöscht werden.
Anders als bei jni werden nicht alle Reference-jnienv-Methoden, die Objekt Verweise zurückgeben, lokale Verweise zurückgeben. [Jnienv. FindClass](xref:Android.Runtime.JNIEnv.FindClass*) gibt einen *globalen* Verweis zurück. Es wird dringend empfohlen, lokale Verweise so schnell wie möglich zu löschen, indem Sie möglicherweise ein [java. lang. Object](xref:Java.Lang.Object) um das Objekt herum erstellen und `JniHandleOwnership.TransferLocalRef` für den [java. lang. Object (IntPtr Handle, jnishandownership Transfer)](xref:Java.Lang.Object#ctor*) -Konstruktor angeben.

Globale Verweise werden von " [jnienv. newglobalref](xref:Android.Runtime.JNIEnv.NewGlobalRef*) " und " [jnienv. FindClass](xref:Android.Runtime.JNIEnv.FindClass*)" erstellt.
Sie können mit [jnienv. deleteglobalref](xref:Android.Runtime.JNIEnv.DeleteGlobalRef*)zerstört werden.
Emulatoren haben eine Beschränkung von 2.000 ausstehenden globalen verweisen, während Hardware Geräte eine Beschränkung von ungefähr 52.000 globalen verweisen haben.

Schwache globale Verweise sind nur unter Android v 2.2 (Froyo) und höher verfügbar. Schwache globale Verweise können mit [jnienv. deleteweakglobalref](xref:Android.Runtime.JNIEnv.DeleteWeakGlobalRef*)gelöscht werden.

### <a name="dealing-with-jni-local-references"></a>Umgang mit lokalen jni-verweisen

Die Methoden [jnienv. getobjectfield](xref:Android.Runtime.JNIEnv.GetObjectField*), [jnienv. getstaticobjectfield](xref:Android.Runtime.JNIEnv.GetStaticObjectField*), jnienv. [cdiskbjectmethod](xref:Android.Runtime.JNIEnv.CallObjectMethod*), [jnienv. callnonvirtualobjectmethod](xref:Android.Runtime.JNIEnv.CallNonvirtualObjectMethod*) und [jnienv. callstaticobjectmethod](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*) geben einen `IntPtr` zurück `null``IntPtr.Zero`, der einen lokalen jni-Verweis auf ein Java-Objekt enthält. Aufgrund der begrenzten Anzahl lokaler Verweise, die gleichzeitig (512 Einträge) ausstehen können, ist es wünschenswert, sicherzustellen, dass die Verweise rechtzeitig gelöscht werden. Es gibt drei Möglichkeiten, wie lokale Verweise behandelt werden können: Explizites löschen, Erstellen einer `Java.Lang.Object`-Instanz, um Sie zu speichern, und Verwenden von `Java.Lang.Object.GetObject<T>()`, um einen verwalteten Aufruf baren Wrapper um diese zu erstellen.

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

`Java.Lang.Object` stellt einen Konstruktor vom Typ [java. lang. Object (IntPtr Handle, jnishandownership Transfer)](xref:Java.Lang.Object#ctor*) bereit, der zum Umschließen eines beendenden jni-Verweises verwendet werden kann. Der [jnilenker Ownership](xref:Android.Runtime.JniHandleOwnership) -Parameter bestimmt, wie der `IntPtr`-Parameter behandelt werden soll:

- [Jnilenker Ownership. donottransfer](xref:Android.Runtime.JniHandleOwnership.DoNotTransfer) &ndash; die erstellte `Java.Lang.Object` Instanz erstellt einen neuen globalen Verweis aus dem `handle`-Parameter, und `handle` ist unverändert.
    Der Aufrufer ist dafür verantwortlich, `handle` bei Bedarf freizugeben.

- [Jnilenker Ownership. transferlocalref](xref:Android.Runtime.JniHandleOwnership.TransferLocalRef) &ndash; die erstellte `Java.Lang.Object` Instanz erstellt einen neuen globalen Verweis aus dem `handle`-Parameter, und `handle` mit [jnienv. Delta](xref:Android.Runtime.JNIEnv.DeleteLocalRef*) Always on gelöscht wird. Der Aufrufer darf `handle` nicht freigeben und darf `handle` nicht verwenden, nachdem die Ausführung des Konstruktors abgeschlossen wurde.

- [Jnilenker Ownership. transferglobalref](xref:Android.Runtime.JniHandleOwnership.TransferLocalRef) &ndash; die erstellte `Java.Lang.Object` Instanz übernimmt den Besitz des `handle`-Parameters. Der Aufrufer darf `handle` nicht freigeben.

Da die Methodenaufruf Methoden von jni lokale Verweise zurückgeben, werden `JniHandleOwnership.TransferLocalRef` normalerweise verwendet:

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef);
```

Der erstellte globale Verweis wird erst freigegeben, wenn für die `Java.Lang.Object` Instanz eine Garbage Collection durchgeführt wurde. Wenn Sie in der Lage sind, die Instanz zu löschen, wird der globale Verweis freigegeben, sodass Garbage Collections beschleunigt werden:

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
using (var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef)) {
    // use value ...
}
```

### <a name="using-javalangobjectgetobjectlttgt"></a>Verwenden von Java. lang. Object. GetObject&lt;t&gt;()

`Java.Lang.Object` stellt eine [java. lang. Object. GetObject&lt;t&gt;-Methode (IntPtr Handle, jnishandownership Transfer)](xref:Java.Lang.Object.GetObject*) bereit, mit der ein verwalteter Aufruf barer Wrapper des angegebenen Typs erstellt werden kann.

Der Typ `T` muss die folgenden Anforderungen erfüllen:

1. `T` muss ein Verweistyp sein.

1. `T` muss die `IJavaObject`-Schnittstelle implementieren.

1. Wenn `T` keine abstrakte Klasse oder Schnittstelle ist, müssen `T` einen Konstruktor mit den Parametertypen `(IntPtr,
    JniHandleOwnership)` bereitstellen.

1. Wenn `T` eine abstrakte Klasse oder eine Schnittstelle ist, *muss* ein *aufrufende Instanz* für `T` verfügbar sein. Ein aufrufende Instanz ist ein nicht abstrakter Typ, der `T` erbt oder `T` implementiert und denselben Namen wie `T` mit einem aufrufende Instanz-Suffix hat. Wenn z. b. T die Schnittstelle `Java.Lang.IRunnable` ist, muss der Typ `Java.Lang.IRunnableInvoker` vorhanden sein und muss den erforderlichen `(IntPtr,
    JniHandleOwnership)`-Konstruktor enthalten.

Da die Methodenaufruf Methoden von jni lokale Verweise zurückgeben, werden `JniHandleOwnership.TransferLocalRef` normalerweise verwendet:

```csharp
IntPtr lrefString = JNIEnv.CallObjectMethod(instance, methodID);
Java.Lang.String value = Java.Lang.Object.GetObject<Java.Lang.String>( lrefString, JniHandleOwnership.TransferLocalRef);
```

<a name="_Looking_up_Java_Types" />

## <a name="looking-up-java-types"></a>Suchen von Java-Typen

Zum Suchen eines Felds oder einer Methode in jni muss zuerst der deklarierende Typ für das Feld oder die Methode gesucht werden. Die [Android. Runtime. jnienv. FindClass (String)](xref:Android.Runtime.JNIEnv.FindClass*))-Methode wird verwendet, um Java-Typen zu suchen. Der String-Parameter ist der *vereinfachte Typverweis* oder der *vollständige Typverweis* für den Java-Typ. Weitere Informationen zu vereinfachten und vollständigen Typverweisen finden Sie im [Abschnitt jni-Typverweise](#_JNI_Type_References) .

Hinweis: im Gegensatz zu jeder anderen `JNIEnv` Methode, die Objektinstanzen zurückgibt, gibt `FindClass` einen globalen Verweis und keinen lokalen Verweis zurück.

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

Dabei ist `*` der Typ des Felds:

- [Jnienv. getobjectfield](xref:Android.Runtime.JNIEnv.GetObjectField*) &ndash; den Wert eines beliebigen Instanzfelds lesen, bei dem es sich nicht um einen Buildtyp handelt, z. b. `java.lang.Object`, Arrays und Schnittstellentypen. Der zurückgegebene Wert ist ein lokaler jni-Verweis.

- [Jnienv. getbooleanfield](xref:Android.Runtime.JNIEnv.GetBooleanField*) &ndash; den Wert `bool` Instanzfelder lesen.

- [Jnienv. GetByteField](xref:Android.Runtime.JNIEnv.GetByteField*) &ndash; den Wert `sbyte` Instanzfelder lesen.

- [Jnienv. getcharfield](xref:Android.Runtime.JNIEnv.GetCharField*) &ndash; den Wert `char` Instanzfelder lesen.

- [Jnienv. getshortfield](xref:Android.Runtime.JNIEnv.GetShortField*) &ndash; den Wert `short` Instanzfelder lesen.

- [Jnienv. getintfield](xref:Android.Runtime.JNIEnv.GetIntField*) &ndash; den Wert `int` Instanzfelder lesen.

- [Jnienv. getlongfield](xref:Android.Runtime.JNIEnv.GetLongField*) &ndash; den Wert `long` Instanzfelder lesen.

- [Jnienv. getfloatfield](xref:Android.Runtime.JNIEnv.GetFloatField*) &ndash; den Wert `float` Instanzfelder lesen.

- [Jnienv. getdoublefield](xref:Android.Runtime.JNIEnv.GetDoubleField*) &ndash; den Wert `double` Instanzfelder lesen.

### <a name="writing-instance-field-values"></a>Schreiben von instanzfeldwerten

Der Satz von Methoden zum Schreiben von instanzfeldwerten folgt dem Benennungs Muster:

```csharp
JNIEnv.SetField(IntPtr instance, IntPtr fieldID, Type value);
```

Where *Type* ist der Typ des Felds:

- [Jnienv. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; den Wert eines beliebigen Felds schreiben, bei dem es sich nicht um einen Buildtyp handelt, z. b. `java.lang.Object`, Arrays und Schnittstellentypen. Der `IntPtr` Wert kann eine lokale jni-Referenz, eine globale jni-Referenz, eine globale jni-Referenz oder eine `IntPtr.Zero` (für `null`) sein.

- [Jnienv. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; den Wert `bool` Instanzfelder schreiben.

- [Jnienv. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; den Wert `sbyte` Instanzfelder schreiben.

- [Jnienv. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; den Wert `char` Instanzfelder schreiben.

- [Jnienv. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; den Wert `short` Instanzfelder schreiben.

- [Jnienv. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; den Wert `int` Instanzfelder schreiben.

- [Jnienv. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; den Wert `long` Instanzfelder schreiben.

- [Jnienv. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; den Wert `float` Instanzfelder schreiben.

- [Jnienv. SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; den Wert `double` Instanzfelder schreiben.

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

Dabei ist `*` der Typ des Felds:

- [Jnienv. getstaticobjectfield](xref:Android.Runtime.JNIEnv.GetStaticObjectField*) &ndash; den Wert eines beliebigen statischen Felds lesen, das kein Buildtyp ist, z. b. `java.lang.Object`, Arrays und Schnittstellentypen. Der zurückgegebene Wert ist ein lokaler jni-Verweis.

- [Jnienv. getstaticbooleanfield](xref:Android.Runtime.JNIEnv.GetStaticBooleanField*) &ndash; den Wert `bool` statischen Felder lesen.

- [Jnienv. getstaticbytefield](xref:Android.Runtime.JNIEnv.GetStaticByteField*) &ndash; den Wert `sbyte` statischen Felder lesen.

- [Jnienv. getstaticcharfield](xref:Android.Runtime.JNIEnv.GetStaticCharField*) &ndash; den Wert `char` statischen Felder lesen.

- [Jnienv. getstaticshortfield](xref:Android.Runtime.JNIEnv.GetStaticShortField*) &ndash; den Wert `short` statischer Felder lesen.

- [Jnienv. getstaticlongfield](xref:Android.Runtime.JNIEnv.GetStaticLongField*) &ndash; den Wert `long` statischer Felder lesen.

- [Jnienv. getstaticfloatfield](xref:Android.Runtime.JNIEnv.GetStaticFloatField*) &ndash; den Wert `float` statischen Felder lesen.

- [Jnienv. getstaticdoublefield](xref:Android.Runtime.JNIEnv.GetStaticDoubleField*) &ndash; den Wert `double` statischer Felder lesen.

### <a name="writing-static-field-values"></a>Schreiben statischer Feldwerte

Der Satz von Methoden zum Schreiben statischer Feldwerte folgt dem Benennungs Muster:

```csharp
JNIEnv.SetStaticField(IntPtr class, IntPtr fieldID, Type value);
```

Where *Type* ist der Typ des Felds:

- [Jnienv. SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; den Wert eines beliebigen statischen Felds, das kein Buildtyp ist, wie z. b. `java.lang.Object`, Arrays und Schnittstellentypen, schreiben. Der `IntPtr` Wert kann eine lokale jni-Referenz, eine globale jni-Referenz, eine globale jni-Referenz oder eine `IntPtr.Zero` (für `null`) sein.

- [Jnienv. SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; den Wert `bool` statischen Felder zu schreiben.

- [Jnienv. SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; den Wert `sbyte` statischen Felder zu schreiben.

- [Jnienv. SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; den Wert `char` statischen Felder zu schreiben.

- [Jnienv. SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; den Wert `short` statischen Felder zu schreiben.

- [Jnienv. SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; den Wert `int` statischen Felder zu schreiben.

- [Jnienv. SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; den Wert `long` statischen Felder zu schreiben.

- [Jnienv. SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; den Wert `float` statischen Felder zu schreiben.

- [Jnienv. SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; den Wert `double` statischen Felder zu schreiben.

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

Dabei ist `*` der Rückgabetyp der Methode.

- [Jnienv. cverteilbjectmethod](xref:Android.Runtime.JNIEnv.CallObjectMethod*) &ndash; eine Methode aufrufen, die einen nicht-Builtin-Typ zurückgibt, wie z. b. `java.lang.Object`, Arrays und Schnittstellen. Der zurückgegebene Wert ist ein lokaler jni-Verweis.

- [Jnienv. callbooleanmethod](xref:Android.Runtime.JNIEnv.CallBooleanMethod*) &ndash; eine Methode aufrufen, die einen `bool` Wert zurückgibt.

- [Jnienv. callbytemethod](xref:Android.Runtime.JNIEnv.CallByteMethod*) &ndash; eine Methode aufrufen, die einen `sbyte` Wert zurückgibt.

- [Jnienv. callcharmethod](xref:Android.Runtime.JNIEnv.CallCharMethod*) &ndash; eine Methode aufrufen, die einen `char` Wert zurückgibt.

- [Jnienv. callshortmethod](xref:Android.Runtime.JNIEnv.CallShortMethod*) &ndash; eine Methode aufrufen, die einen `short` Wert zurückgibt.

- [Jnienv. calllongmethod](xref:Android.Runtime.JNIEnv.CallLongMethod*) &ndash; eine Methode aufrufen, die einen `long` Wert zurückgibt.

- [Jnienv. callfloatmethod](xref:Android.Runtime.JNIEnv.CallFloatMethod*) &ndash; eine Methode aufrufen, die einen `float` Wert zurückgibt.

- [Jnienv. calldoublemethod](xref:Android.Runtime.JNIEnv.CallDoubleMethod*) &ndash; eine Methode aufrufen, die einen `double` Wert zurückgibt.

### <a name="non-virtual-method-invocation"></a>Aufruf der nicht virtuellen Methode

Der Satz von Methoden zum Aufrufen von Methoden, die nicht virtuell sind, folgt dem Benennungs Muster:

```csharp
* JNIEnv.CallNonvirtual*Method( IntPtr instance, IntPtr class, IntPtr methodID, params JValue[] args );
```

Dabei ist `*` der Rückgabetyp der Methode. Der Aufruf der nicht virtuellen Methode wird normalerweise verwendet, um die Basis Methode einer virtuellen Methode aufzurufen.

- [Jnienv. callnonvirtualobjectmethod](xref:Android.Runtime.JNIEnv.CallNonvirtualObjectMethod*) &ndash; nicht virtuell eine Methode aufrufen, die einen nicht-Builtin-Typ zurückgibt, wie z. b. `java.lang.Object`, Arrays und Schnittstellen. Der zurückgegebene Wert ist ein lokaler jni-Verweis.

- [Jnienv. callnonvirtualbooleanmethod](xref:Android.Runtime.JNIEnv.CallNonvirtualBooleanMethod*) &ndash; nicht virtuell eine Methode aufrufen, die einen `bool` Wert zurückgibt.

- [Jnienv. callnonvirtualbytemethod](xref:Android.Runtime.JNIEnv.CallNonvirtualByteMethod*) &ndash; nicht virtuell eine Methode aufrufen, die einen `sbyte` Wert zurückgibt.

- [Jnienv. callnonvirtualcharmethod](xref:Android.Runtime.JNIEnv.CallNonvirtualCharMethod*) &ndash; nicht virtuell eine Methode aufrufen, die einen `char` Wert zurückgibt.

- [Jnienv. callnonvirtualshortmethod](xref:Android.Runtime.JNIEnv.CallNonvirtualShortMethod*) &ndash; nicht virtuell eine Methode aufrufen, die einen `short` Wert zurückgibt.

- [Jnienv. callnonvirtuallongmethod](xref:Android.Runtime.JNIEnv.CallNonvirtualLongMethod*) &ndash; nicht virtuell eine Methode aufrufen, die einen `long` Wert zurückgibt.

- [Jnienv. callnonvirtualfloatmethod](xref:Android.Runtime.JNIEnv.CallNonvirtualFloatMethod*) &ndash; nicht virtuell eine Methode aufrufen, die einen `float` Wert zurückgibt.

- [Jnienv. callnonvirtualdoublemethod](xref:Android.Runtime.JNIEnv.CallNonvirtualDoubleMethod*) &ndash; nicht virtuell eine Methode aufrufen, die einen `double` Wert zurückgibt.

<a name="_Static_Methods" />

## <a name="static-methods"></a>Statische Methoden

Statische Methoden werden über Methoden- *IDs*aufgerufen. Methoden-IDs werden über [jnienv. getstaticmethodid abgerufen.](xref:Android.Runtime.JNIEnv.GetStaticMethodID*)dies erfordert den Typ, in dem die Methode definiert ist, den Namen der Methode und die [jni-Typsignatur](#JNI_Type_Signatures) der Methode.

Methoden-IDs müssen nicht freigegeben werden und sind gültig, solange der entsprechende Java-Typ geladen wurde. (Android unterstützt derzeit keine Entladen der Klasse.)

### <a name="static-method-invocation"></a>Statischer Methodenaufruf

Der Satz von Methoden zum Aufrufen von Methoden, die virtuell auf das Benennungs Muster folgen:

```csharp
* JNIEnv.CallStatic*Method( IntPtr class, IntPtr methodID, params JValue[] args );
```

Dabei ist `*` der Rückgabetyp der Methode.

- [Jnienv. callstaticobjectmethod](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*) &ndash; eine statische Methode aufrufen, die einen nicht-Builtin-Typ zurückgibt, z. b. `java.lang.Object`, Arrays und Schnittstellen. Der zurückgegebene Wert ist ein lokaler jni-Verweis.

- [Jnienv. callstaticbooleanmethod](xref:Android.Runtime.JNIEnv.CallStaticBooleanMethod*) &ndash; eine statische Methode aufrufen, die einen `bool` Wert zurückgibt.

- [Jnienv. callstaticbytemethod](xref:Android.Runtime.JNIEnv.CallStaticByteMethod*) &ndash; eine statische Methode aufrufen, die einen `sbyte` Wert zurückgibt.

- [Jnienv. callstaticcharmethod](xref:Android.Runtime.JNIEnv.CallStaticCharMethod*) &ndash; eine statische Methode aufrufen, die einen `char` Wert zurückgibt.

- [Jnienv. callstaticshortmethod](xref:Android.Runtime.JNIEnv.CallStaticShortMethod*) &ndash; eine statische Methode aufrufen, die einen `short` Wert zurückgibt.

- [Jnienv. callstaticlongmethod](xref:Android.Runtime.JNIEnv.CallLongMethod*) &ndash; eine statische Methode aufrufen, die einen `long` Wert zurückgibt.

- [Jnienv. callstaticfloatmethod](xref:Android.Runtime.JNIEnv.CallStaticFloatMethod*) &ndash; eine statische Methode aufrufen, die einen `float` Wert zurückgibt.

- [Jnienv. callstaticdoublemethod](xref:Android.Runtime.JNIEnv.CallStaticDoubleMethod*) &ndash; eine statische Methode aufrufen, die einen `double` Wert zurückgibt.

<a name="JNI_Type_Signatures" />

## <a name="jni-type-signatures"></a>Jni-Typsignaturen

[Jni-Typsignaturen](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/types.html#wp16432) sind [jni-Typverweise](#_JNI_Type_References) (obwohl nicht vereinfachte Typverweise), mit Ausnahme der Methoden. Bei-Methoden ist die jni-Typsignatur eine öffnende Klammer `'('`, gefolgt von den Typverweisen für alle Parametertypen, die zusammen verkettet werden (ohne Trennzeichen oder sonstige), gefolgt von einer schließenden Klammer `')'`, gefolgt von der jni-Typreferenz des Methoden Rückgabe Typs.

Beispielsweise mit der Java-Methode:

```java
long f(int n, String s, int[] array);
```

Die jni-Typsignatur lautet wie folgt:

```csharp
(ILjava/lang/String;[I)J
```

Im Allgemeinen wird *dringend* empfohlen, den `javap`-Befehl zu verwenden, um jni-Signaturen zu ermitteln. Beispielsweise die jni-Typsignatur von [java. die lang. Thread. State. valueOf (String)](https://developer.android.com/reference/java/lang/Thread.State.html#valueOf(java.lang.String)) -Methode ist "(Ljava/lang/String;) Ljava/lang/Thread $ State;", während die jni-Typsignatur der [java. lang. Thread. State. Values](https://developer.android.com/reference/java/lang/Thread.State.html#values) -Methode "() [Ljava/lang/Thread $ State;" lautet. Achten Sie auf die nachfolgenden Semikolons. Diese *sind* Teil der jni-Typsignatur.

<a name="_JNI_Type_References" />

## <a name="jni-type-references"></a>Jni-Typverweise

Jni-Typverweise unterscheiden sich von Java-Typverweisen. Sie können keine voll qualifizierten Java-Typnamen (z. b. `java.lang.String` mit JNI) verwenden. stattdessen müssen Sie je nach Kontext die jni-Variationen `"java/lang/String"` oder `"Ljava/lang/String;"`verwenden. Weitere Informationen finden Sie weiter unten.
Es gibt vier Typen von jni-Typverweisen:

- **integriert**
- **einfach**
- **type**
- **array**

### <a name="built-in-type-references"></a>Integrierte Typverweise

Integrierte Typverweise sind ein einzelnes Zeichen, das verwendet wird, um auf integrierte Werttypen zu verweisen. Die Zuordnung lautet wie folgt:

- `"B"` für `sbyte`.
- `"S"` für `short`.
- `"I"` für `int`.
- `"J"` für `long`.
- `"F"` für `float`.
- `"D"` für `double`.
- `"C"` für `char`.
- `"Z"` für `bool`.
- `"V"` für `void` Methoden Rückgabe Typen.

<a name="_Simplified_Type_References_1" />

### <a name="simplified-type-references"></a>Vereinfachte Typverweise

Vereinfachte Typverweise können nur in [jnienv. FindClass (String)](xref:Android.Runtime.JNIEnv.FindClass*)) verwendet werden.
Es gibt zwei Möglichkeiten, einen vereinfachten Typverweis abzuleiten:

1. Ersetzen Sie unter einem voll qualifizierten Java-Namen alle `'.'` innerhalb des Paket namens und vor dem Typnamen durch `'/'` und alle `'.'` innerhalb eines Typnamens mit `'$'`.

1. Lesen Sie die Ausgabe `'unzip -l android.jar | grep JavaName'`.

Beide Ergebnisse führen dazu, dass der Java-Typ [java. lang. Thread. State](https://developer.android.com/reference/java/lang/Thread.State.html) dem vereinfachten Typverweis `java/lang/Thread$State`zugeordnet wird.

### <a name="type-references"></a>Typverweise

Ein Typverweis ist ein integrierter Typverweis oder ein vereinfachter Typverweis mit einem `'L'` Präfix und einem `';'` Suffix. Für den Java-Typ [java. lang. String](https://developer.android.com/reference/java/lang/String.html)wird der vereinfachte Typverweis `"java/lang/String"`, während der Typverweis `"Ljava/lang/String;"`ist.

Typverweise werden mit Array-Typverweisen und mit jni-Signaturen verwendet.

Eine zusätzliche Möglichkeit zum Abrufen eines Typverweises besteht darin, die Ausgabe `'javap -s -classpath android.jar fully.qualified.Java.Name'`zu lesen.
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

`Thread.State` ist ein Java-Enumeration-Typ. Daher können wir die Signatur der `valueOf`-Methode verwenden, um zu bestimmen, ob der Typverweis Ljava/lang/Thread $ State; ist.

### <a name="array-type-references"></a>Array-Typverweise

Array-Typverweise werden `'['` einem jni-Typverweis vorangestellt.
Vereinfachte Typverweise können beim Angeben von Arrays nicht verwendet werden.

`int[]` z. b. `"[I"`ist, `int[][]` `"[[I"`und `java.lang.Object[]` `"[Ljava/lang/Object;"`ist.

## <a name="java-generics-and-type-erasure"></a>Java-Generika und typlöschung

In *den meisten* Fällen sind Java-Generika *nicht vorhanden*, wie Sie über jni sichtbar sind.
Es gibt einige "Falten", aber diese Falten bestehen darin, wie Java mit Generika interagiert, nicht mit der Art und Weise, wie jni sucht und generische Member aufruft.

Bei der Interaktion mit jni gibt es keinen Unterschied zwischen einem generischen Typ oder Member und einem nicht generischen Typ oder Member. Der generische Typ " [java. lang. Class&lt;t&gt;](https://developer.android.com/reference/java/lang/Class.html) " ist beispielsweise auch der generische Typ `java.lang.Class`, der beide den gleichen vereinfachten Typverweis `"java/lang/Class"`hat.

## <a name="java-native-interface-support"></a>Unterstützung von Java Native Interface

[Android. Runtime. jnienv](xref:Android.Runtime.JNIEnv) ist ein verwalteter Wrapper für die Jave Native Interface (JNI). Jni-Funktionen werden innerhalb der [Java Native Interface-Spezifikation](https://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html)deklariert, obwohl die Methoden so geändert wurden, dass die expliziten `JNIEnv*` Parameter und `IntPtr` anstelle von `jobject`, `jclass`, `jmethodID`usw. verwendet werden. Sehen Sie sich beispielsweise die [jni NewObject-Funktion](https://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp4517)an:

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

Wenn Sie über eine Java-Objektinstanz verfügen, die in einem IntPtr enthalten ist, möchten Sie wahrscheinlich etwas damit tun. Hierfür können Sie jnienv-Methoden wie [jnienv. callvoidmethod ()](xref:Android.Runtime.JNIEnv.CallVoidMethod*) verwenden. Wenn jedoch bereits ein analoger C# Wrapper vorhanden ist, sollten Sie einen Wrapper über die jni-Referenz erstellen. Dies können Sie über die Erweiterungsmethode [Extensions. javacast\<t >](xref:Android.Runtime.Extensions.JavaCast*) :

```csharp
IntPtr lrefActivity = CreateMapActivity();

// imagine that Activity were instead an interface or abstract type...
Activity mapActivity = new Java.Lang.Object(lrefActivity, JniHandleOwnership.TransferLocalRef)
    .JavaCast<Activity>();
```

Sie können auch die Methode [java. lang. Object. GetObject\<t >](xref:Java.Lang.Object.GetObject*) verwenden:

```csharp
IntPtr lrefActivity = CreateMapActivity();

// imagine that Activity were instead an interface or abstract type...
Activity mapActivity = Java.Lang.Object.GetObject<Activity>(lrefActivity, JniHandleOwnership.TransferLocalRef);
```

Außerdem wurden alle jni-Funktionen geändert, indem der `JNIEnv*`-Parameter entfernt wurde, der in jeder jni-Funktion vorhanden ist.

## <a name="summary"></a>Zusammenfassung

Der direkte Umgang mit jni ist ein schreckliches Verfahren, das bei allen Kosten vermieden werden sollte. Leider ist dies nicht immer vermeidbar. hoffentlich erhalten Sie in diesem Handbuch einige Unterstützung, wenn Sie die ungebundenen Java-Fälle mit Mono für Android erreichen.

## <a name="related-links"></a>Verwandte Links

- [Java Native Interface-Spezifikation](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/jniTOC.html)
- [Java Native Interface-Funktionen](https://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html)
