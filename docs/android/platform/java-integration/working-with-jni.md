---
title: Arbeiten mit der JNI und Xamarin.Android
description: Xamarin.Android ermöglicht das Schreiben von Android-Apps mit C# anstatt Java. Mit Xamarin.Android werden verschiedene Assemblys bereitgestellt, die Bindungen für Java-Bibliotheken wie „Mono.Android.dll“ und „Mono.Android.GoogleMaps.dll“ bieten. Es stehen jedoch nicht für alle möglicherweise vorhandenen Java-Bibliotheken Bindungen zur Verfügung, und die bereitgestellten Bindungen können möglicherweise nicht für jeden Java-Typ und jedes Java-Member verwendet werden. Für nicht gebundene Java-Typen und -Member kann die native Java-Schnittstelle (Java Native Interface, JNI) verwendet werden. Dieser Artikel veranschaulicht die Verwendung der JNI zum Interagieren mit Java-Typen und -Membern aus Xamarin.Android-Anwendungen.
ms.prod: xamarin
ms.assetid: A417DEE9-7B7B-4E35-A79C-284739E3838E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 0fa717a775ff2f1ace9e248a8afde8d373e8a1f8
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "76724344"
---
# <a name="working-with-jni-and-xamarinandroid"></a>Arbeiten mit der JNI und Xamarin.Android

_Xamarin.Android ermöglicht das Schreiben von Android-Apps mit C# anstatt Java. Mit Xamarin.Android werden verschiedene Assemblys bereitgestellt, die Bindungen für Java-Bibliotheken wie „Mono.Android.dll“ und „Mono.Android.GoogleMaps.dll“ bieten. Es stehen jedoch nicht für alle möglicherweise vorhandenen Java-Bibliotheken Bindungen zur Verfügung, und die bereitgestellten Bindungen können möglicherweise nicht für jeden Java-Typ und jedes Java-Member verwendet werden. Für nicht gebundene Java-Typen und -Member kann die native Java-Schnittstelle (Java Native Interface, JNI) verwendet werden. Dieser Artikel veranschaulicht die Verwendung der JNI zum Interagieren mit Java-Typen und -Membern aus Xamarin.Android-Anwendungen._

## <a name="overview"></a>Übersicht

Es ist nicht immer notwendig oder möglich, einen Managed Callable Wrapper (MCW) zu erstellen, um Java-Code aufzurufen. In vielen Fällen ist die „Inline“-JNI für die einmalige Verwendung von nicht gebundenen Java-Membern sehr gut geeignet und nützlich. Häufig ist es einfacher, die JNI zu verwenden, um eine einzelne Methode in einer Java-Klasse aufzurufen, als eine komplette .jar-Bindung zu generieren.

Xamarin.Android stellt die Assembly `Mono.Android.dll` bereit, die eine Bindung für die `android.jar`-Bibliothek von Android bietet. In `Mono.Android.dll` nicht vorhandene Typen und Member und in `android.jar` nicht vorhandene Typen können für eine manuelle Bindung verwendet werden. Zum Binden von Java-Typen und -Membern verwenden Sie die **Java Native Interface** (**JNI**), um Typen zu suchen, Felder zu lesen und zu schreiben und Methoden aufzurufen.

Die JNI-API in Xamarin.Android ähnelt im Konzept sehr stark der `System.Reflection`-API in .NET: Sie ermöglicht Ihnen das Suchen nach Typen und Membern anhand des Namens, das Lesen und Schreiben von Feldwerten, das Aufrufen von Methoden und vieles mehr. Sie können die JNI und das benutzerdefinierte `Android.Runtime.RegisterAttribute`-Attribut verwenden, um virtuelle Methoden zu deklarieren, die gebunden werden können, um das Überschreiben zu unterstützen. Sie können Schnittstellen binden, sodass diese in C# implementiert werden können.

Im vorliegenden Dokument wird Folgendes erläutert:

- Verweisen auf Typen in der JNI
- Suchen, Lesen und Schreiben von Feldern
- Suchen und Aufrufen von Methoden
- Verfügbarmachen von virtuellen Methoden, um das Überschreiben aus verwaltetem Code zuzulassen
- Verfügbarmachen von Schnittstellen

## <a name="requirements"></a>Anforderungen

Die JNI, so wie sie durch den [Android.Runtime.JNIEnv-Namespace](xref:Android.Runtime.JNIEnv) verfügbar gemacht wird, steht in jeder Version von Xamarin.Android zur Verfügung.
Um Java-Typen und -Schnittstellen zu binden, müssen Sie Xamarin.Android 4.0 oder höher verwenden.

## <a name="managed-callable-wrappers"></a>Managed Callable Wrappers

Ein **Managed Callable Wrapper** (**MCW**) ist eine *Bindung* für eine Java-Klasse oder -Schnittstelle, die sämtliche JNI-Vorgänge umschließt, sodass der C#-Clientcode sich nicht um die zugrunde liegende Komplexität der JNI kümmern muss. Der größte Teil der `Mono.Android.dll` besteht aus Managed Callable Wrappers.

Managed Callable Wrappers werden für zwei Zwecke eingesetzt:

1. Sie kapseln die JNI-Verwendung, sodass die zugrunde liegende Komplexität für den Clientcode keine Rolle spielt.
1. Sie ermöglicht das Erstellen von Unterklassen für Java-Typen und das Implementieren von Java-Schnittstellen.

Der erste Punkt dient der Benutzerfreundlichkeit und der Kapselung der Komplexität, sodass Consumer eine einfache, verwaltete Gruppe von Klassen verwenden können. Hierfür müssen die verschiedenen [JNIEnv](xref:Android.Runtime.JNIEnv)-Member verwendet werden, wie weiter unten in diesem Artikel beschrieben. Denken Sie daran, dass Managed Callable Wrappers nicht unbedingt notwendig sind &ndash; die „Inline“-JNI ist für die einmalige Verwendung von nicht gebundenen Java-Membern sehr gut geeignet und nützlich. Für die Erstellung von Unterklassen und die Implementierung der Schnittstelle sind Managed Callable Wrappers erforderlich.

## <a name="android-callable-wrappers"></a>Android Callable Wrapper

Android Callable Wrappers (ACWs) sind immer dann erforderlich, wenn die Android-Runtime (ART) verwalteten Code aufrufen muss, weil es keine Möglichkeit gibt, Klassen zur Laufzeit bei der ART zu registrieren.
(Insbesondere wird die JNI-Funktion [DefineClass](https://docs.oracle.com/javase/6/docs/technotes/guides/jni/spec/functions.html#wp15986) von der Android-Runtime nicht unterstützt. Android Callable Wrappers kompensieren daher die fehlende Unterstützung für die Typregistrierung zur Laufzeit.)

Jedes Mal, wenn Android-Code eine virtuelle oder Schnittstellenmethode ausführen muss, die in verwaltetem Code überschrieben oder implementiert wird, muss Xamarin.Android einen Java-Proxy bereitstellen, damit diese Methode an den entsprechenden verwalteten Typ verteilt wird (dispatch). Diese Java-Proxytypen sind Java-Code, verfügen über die „gleiche“ Basisklasse und Java-Schnittstellenliste wie der verwaltete Typ, implementieren dieselben Konstruktoren und deklarieren alle überschriebenen Basisklassen- und Schnittstellenmethoden.

Android Callable Wrappers werden vom **monodroid.exe**-Programm während des [Buildprozesses](~/android/deploy-test/building-apps/build-process.md) für alle Typen generiert, die (direkt oder indirekt) [Java.Lang.Object](xref:Java.Lang.Object) erben.

### <a name="implementing-interfaces"></a>Implementieren von Schnittstellen

Bisweilen müssen Sie möglicherweise eine Android-Schnittstelle implementieren, (z. B. [Android.Content.IComponentCallbacks](xref:Android.Content.IComponentCallbacks)).

Alle Android-Klassen und -Schnittstellen erweitern die [Android.Runtime.IJavaObject](xref:Android.Runtime.IJavaObject)-Schnittstelle; daher müssen alle Android-Typen `IJavaObject` implementieren.
Diese Tatsache macht Xamarin.Android sich zunutze &ndash; die Erweiterung verwendet `IJavaObject`, um Android einen Java-Proxy (einen Android Callable Wrapper) für den angegebenen verwalteten Typ bereitzustellen. Da **monodroid.exe** nur nach `Java.Lang.Object`-Unterklassen (die `IJavaObject` implementieren müssen) sucht, bietet die Erstellung von Unterklassen für `Java.Lang.Object` eine Möglichkeit, Schnittstellen in verwaltetem Code zu implementieren. Zum Beispiel:

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

*Im restlichen Teil dieses Artikels finden Sie Implementierungsdetails, die sich ohne vorherige Ankündigung ändern können* (und hier nur vorhanden sind, weil Entwickler sicherlich wissen möchten, was im Hintergrund passiert).

Betrachten Sie beispielsweise die folgende C#-Quelle:

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

Das **mandroid.exe**-Programm generiert den folgenden Android Callable Wrapper:

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

Beachten Sie, dass die Basisklasse beibehalten wird und für alle Methoden, die in verwaltetem Code überschrieben werden, native Methodendeklarationen bereitgestellt werden.

### <a name="exportattribute-and-exportfieldattribute"></a>ExportAttribute und ExportFieldAttribute

In der Regel generiert Xamarin.Android den Java-Code automatisch, aus dem der ACW besteht. Diese Generierung basiert auf den Klassen- und Methodennamen, wenn eine Klasse aus einer Java-Klasse abgeleitet wird und vorhandene Java-Methoden überschreibt. In einigen Szenarien erfolgt die Codegenerierung aber nicht auf angemessene Weise, wie im Folgenden skizziert:

- Android unterstützt Aktionsnamen in XML-Attributen für das Layout, beispielsweise im XML-Attribut [android:onClick](xref:Android.Views.View.IOnClickListener.OnClick*). Wenn dieses angegeben ist, versucht die erzeugte View-Instanz, nach der Java-Methode zu suchen.

- Die [java.io.Serializable](https://developer.android.com/reference/java/io/Serializable.html)-Schnittstelle erfordert die Methoden `readObject` und `writeObject`. Da keine Member dieser Schnittstelle vorhanden sind, macht unsere entsprechende verwaltete Implementierung diese Methoden nicht für Java-Code verfügbar.

- Die [android.os.Parcelable](xref:Android.OS.Parcelable)-Schnittstelle erwartet, dass eine Implementierungsklasse ein statisches `CREATOR`-Feld vom Typ `Parcelable.Creator` aufweist. Der generierte Java-Code erfordert ein explizites Feld. In unserem Standardszenario gibt es keine Möglichkeit, ein Feld aus verwaltetem Code in Java-Code auszugeben.

Da die Codegenerierung keine Möglichkeit zum Generieren von beliebigen Java-Methoden mit beliebigen Namen bereitstellt, wurden in Xamarin.Android 4.2 das [ExportAttribute](xref:Java.Interop.ExportAttribute) und das [ExportFieldAttribute](xref:Java.Interop.ExportFieldAttribute) eingeführt, um für die oben genannten Szenarien eine Lösung zu bieten. Beide Attribute befinden sich im Namespace `Java.Interop`:

- `ExportAttribute` &ndash; gibt einen Methodennamen und die zugehörigen erwarteten Ausnahmetypen an (für eine explizite Auslösung in Java). Wenn dieses Attribut in einer Methode verwendet wird, „exportiert“ die Methode eine Java-Methode, die einen Dispatchcode in den entsprechenden JNI-Aufruf der verwalteten Methode. Es kann mit `android:onClick` und `java.io.Serializable` verwendet werden.

- `ExportFieldAttribute` &ndash; gibt einen Feldnamen an. Das Attribut befindet sich in einer Methode, die als Feldinitialisierer fungiert. Es kann mit `android.os.Parcelable` verwendet werden.

#### <a name="troubleshooting-exportattribute-and-exportfieldattribute"></a>Problembehandlung bei ExportAttribute und ExportFieldAttribute

- Bei der Paketerstellung tritt aufgrund einer fehlenden **Mono.Android.Export.dll** ein Fehler auf &ndash; wenn Sie `ExportAttribute` oder `ExportFieldAttribute` in einigen Methoden in Ihrem Code oder abhängigen Bibliotheken verwendet haben, müssen Sie die **Mono.Android.Export.dll** hinzufügen. Diese Assembly ist isoliert, um Rückrufcode aus Java zu unterstützen. Sie ist von der **Mono.Android.dll** getrennt, da sie die Anwendung vergrößert.

- Im Releasebuild tritt `MissingMethodException` bei Exportmethoden auf. (Dieses Problem wurde in der neuesten Version von Xamarin.Android behoben.)

### <a name="exportparameterattribute"></a>ExportParameterAttribute

`ExportAttribute` und `ExportFieldAttribute` stellen Funktionalität bereit, die vom Java-Runtimecode verwendet werden kann. Dieser Runtimecode greift über die generierten JNI-Methoden, die durch diese Attribute gesteuert werden, auf verwalteten Code zu. Aus diesem Grund gibt es keine Java-Methode, die durch die verwaltete Methode gebunden wird, und daher wird die Java-Methode über eine verwaltete Methodensignatur generiert.

Dieser Fall gilt jedoch nicht grundsätzlich. Insbesondere gilt er in einigen erweiterten Zuordnungen zwischen verwalteten Typen und Java-Typen wie diesen:

- InputStream
- OutputStream
- XmlPullParser
- XmlResourceParser

Wenn Typen wie diese für exportierte Methoden erforderlich sind, muss das `ExportParameterAttribute` verwendet werden, um dem entsprechenden Parameter oder Rückgabewert explizit einen Typ zuzuweisen.

### <a name="annotation-attribute"></a>Anmerkungsattribut

In Xamarin.Android 4.2 wurden `IAnnotation`-Implementierungstypen in Attribute („System.Attribute“) konvertiert, und es wurde Unterstützung für die Generierung von Anmerkungen in Java-Wrappern hinzugefügt.

Dies zieht folgende direktionale Änderungen nach sich:

- Der Bindungsgenerator generiert `Java.Lang.DeprecatedAttribute` aus `java.Lang.Deprecated` (sollte in verwaltetem Code `[Obsolete]` sein).

- Das bedeutet nicht, dass die vorhandene `Java.Lang.Deprecated`-Klasse verschwindet. Diese Java-basierten Objekte können weiterhin als normale Java-Objekte verwendet werden (wenn eine solche Verwendung existiert). Es gibt die Klassen `Deprecated` und `DeprecatedAttribute`.

- Die Klasse `Java.Lang.DeprecatedAttribute` ist als `[Annotation]` markiert. Wenn ein benutzerdefiniertes Attribut vorhanden ist, das von diesem `[Annotation]`-Attribut geerbt wird, erstellt der msbuild-Task eine Java-Anmerkung für dieses benutzerdefinierte Attribut (@Deprecated) im Android Callable Wrapper (ACW).

- Anmerkungen können in Klassen, Methoden und exportierten Feldern (bei denen es sich um eine Methode in verwaltetem Code handelt) generiert werden.

Wenn die enthaltende Klasse (die Klasse mit den Anmerkungen selbst oder die Klasse, die die Member mit den Anmerkungen enthält) nicht registriert ist, wird die gesamte Java-Klassenquelle nicht generiert, einschließlich Anmerkungen. Bei Methoden können Sie das `ExportAttribute` angeben, um die Methode explizit zu generieren und mit Anmerkungen zu versehen. Außerdem ist es nicht möglich, eine Definition für eine Java-Anmerkungsklasse zu „generieren“. Anders gesagt: Wenn Sie ein benutzerdefiniertes verwaltetes Attribut für eine bestimmte Anmerkung definieren, müssen Sie eine weitere JAR-Bibliothek hinzufügen, die die entsprechende Java-Anmerkungsklasse enthält. Es reicht nicht aus, eine Java-Quelldatei hinzuzufügen, die den Anmerkungstyp definiert. Der Java-Compiler funktioniert nicht auf die gleiche Weise wie **apt**.

Darüber hinaus gelten die folgenden Einschränkungen:

- Dieser Konvertierungsprozess berücksichtigt bisher keine `@Target`-Anmerkungen im Anmerkungstyp.

- Attribute in einer Eigenschaft funktionieren nicht. Verwenden Sie stattdessen Attribute für den Getter oder Setter für eine Eigenschaft.

## <a name="class-binding"></a>Klassenbindung

Das Binden einer Klasse bedeutet, dass ein Managed Callable Wrapper geschrieben wird, um den Aufruf des zugrunde liegenden Java-Typs zu vereinfachen.

Zum Binden virtueller und abstrakter Methoden, um das Überschreiben aus C# zuzulassen, ist Xamarin.Android 4.0 erforderlich. Allerdings kann jede Version von Xamarin.Android nicht virtuelle Methoden, statische Methoden oder virtuelle Methoden ohne unterstützende Überschreibungen binden.

Eine Bindung enthält in der Regel die folgenden Elemente:

- Ein [JNI-Handle für den Java-Typ, der gebunden werden soll](#_Looking_up_Java_Types).

- [JNI-Feld-IDs und Eigenschaften für jedes gebundene Feld](#_Instance_Fields).

- [JNI-Methoden-IDs und Methoden für jede gebundene Methode](#_Instance_Methods).

- Wenn die Erstellung von Unterklassen erforderlich ist, muss der Typ ein benutzerdefiniertes Attribut [RegisterAttribute](xref:Android.Runtime.RegisterAttribute) in der Typdeklaration aufweisen, bei dem [RegisterAttribute.DoNotGenerateAcw](xref:Android.Runtime.RegisterAttribute.DoNotGenerateAcw) auf `true` festgelegt ist.

### <a name="declaring-type-handle"></a>Handle für den deklarierenden Typ

Die Methoden zum Suchen von Feldern und Methoden erfordern einen Objektverweis auf ihren deklarierenden Typ. Per Konvention ist dieser in einem `class_ref`-Feld enthalten:

```csharp
static IntPtr class_ref = JNIEnv.FindClass(CLASS);
```

Informationen zum `CLASS`-Token finden Sie im Abschnitt [JNI-Typverweise](#_JNI_Type_References).

### <a name="binding-fields"></a>Binden von Feldern

Java-Felder werden als C#-Eigenschaften verfügbar gemacht: Beispielweise wird das Java-Feld [java.lang.System.in](https://developer.android.com/reference/java/lang/System.html#in) als C#-Eigenschaft [Java.Lang.JavaSystem.In](xref:Java.Lang.JavaSystem.In) gebunden.
Da die JNI zwischen statischen Feldern und Instanzfeldern unterscheidet, werden darüber hinaus beim Implementieren der Eigenschaften verschiedene Methoden verwendet.

An der Feldbindung sind drei Methodengruppen beteiligt:

1. Die *get field id*-Methode. Die *get field id*-Methode ist dafür zuständig, ein Feldhandle zurückzugeben, das von den Methoden *get field value* und *set field value* verwendet wird. Zum Abrufen der Feld-ID müssen der deklarierende Typ, der Name des Felds und die [JNI-Typsignatur](#JNI_Type_Signatures) des Felds bekannt sein.

1. Die *get field value*-Methoden. Diese Methoden erfordern das Feldhandle und sind für das Auslesen der Feldwerte aus Java zuständig.
    Welche Methode verwendet wird, hängt vom Typ des Felds ab.

1. Die *set field value*-Methoden. Diese Methoden erfordern das Feldhandle und sind für das Schreiben der Feldwerte in Java zuständig. Welche Methode verwendet wird, hängt vom Typ des Felds ab.

[Statische Felder](#_Static_Fields) verwenden die Methoden [JNIEnv.GetStaticFieldID](xref:Android.Runtime.JNIEnv.GetStaticMethodID*), `JNIEnv.GetStatic*Field` und [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*).

 [Instanzfelder](#_Instance_Fields) verwenden die Methoden [JNIEnv.GetFieldID](xref:Android.Runtime.JNIEnv.GetFieldID*), `JNIEnv.Get*Field` und [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*).

Die statische Eigenschaft `JavaSystem.In` kann z. B. folgendermaßen implementiert werden:

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

Hinweis: Wir verwenden [InputStreamInvoker.FromJniHandle](xref:Android.Runtime.InputStreamInvoker.FromJniHandle*), um den JNI-Verweis in eine `System.IO.Stream`-Instanz zu konvertieren, und wir verwenden `JniHandleOwnership.TransferLocalRef`, weil [JNIEnv.GetStaticObjectField](xref:Android.Runtime.JNIEnv.GetStaticObjectField*) einen lokalen Verweis zurückgibt.

Viele der [Android.Runtime](xref:Android.Runtime)-Typen weisen `FromJniHandle`-Methoden auf, die einen JNI-Verweis in den gewünschten Typ konvertieren.

### <a name="method-binding"></a>Methodenbindung

Java-Methoden werden als C#-Methoden und C#-Eigenschaften verfügbar gemacht. Die Java-Methode [java.lang.Runtime.runFinalizersOnExit](https://developer.android.com/reference/java/lang/Runtime.html#runFinalizersOnExit(boolean)) beispielsweise ist als [Java.Lang.Runtime.RunFinalizersOnExit](xref:Java.Lang.Runtime.RunFinalizersOnExit*)-Methode gebunden, und die [java.lang.Object.getClass](https://developer.android.com/reference/java/lang/Object.html#getClass)-Methode ist als [Java.Lang.Object.Class](xref:Java.Lang.Object.Class)-Eigenschaft gebunden.

Der Methodenaufruf erfolgt in zwei Schritten:

1. Rufen Sie die *get method id*-Methode für die aufzurufende Methode ab. Die *get method id*-Methode gibt ein Methodenhandle zurück, das von den Methoden für den Methodenaufruf verwendet wird. Zum Abrufen der Methoden-ID müssen der deklarierende Typ, der Name der Methode und die [JNI-Typsignatur](#JNI_Type_Signatures) der Methode bekannt sein.

1. Rufen Sie die Methode auf.

Genauso wie bei Feldern wird bei den Methoden zum Abrufen der Methoden-ID und zum Aufrufen der Methode zwischen statischen Methoden und Instanzmethoden unterschieden.

[Statische Methoden](#_Static_Methods_1) verwenden [JNIEnv.GetStaticMethodID()](xref:Android.Runtime.JNIEnv.GetStaticMethodID*) zum Suchen nach der Methoden-ID und die Methodenfamilie `JNIEnv.CallStatic*Method` für den Aufruf.

[Instanzmethoden](#_Instance_Methods) verwenden [JNIEnv.GetMethodID](xref:Android.Runtime.JNIEnv.GetMethodID*) zum Suchen nach der Methoden-ID und die Methodenfamilien `JNIEnv.Call*Method` und `JNIEnv.CallNonvirtual*Method` für den Aufruf.

Die Methodenbindung kann mehr bieten als nur Methodenaufrufe. Zur Methodenbindung gehört auch das Zulassen der Überschreibung (bei abstrakten und nicht abschließenden Methoden) oder Implementierung (bei Schnittstellenmethoden) von Methoden. Im Abschnitt [Unterstützen der Vererbung, Schnittstellen](#_Supporting_Inheritance,_Interfaces_1) wird die Komplexität der Unterstützung von virtuellen Methoden und Schnittstellenmethoden behandelt.

<a name="_Static_Methods_1" />

#### <a name="static-methods"></a>Statische Methoden

Das Binden einer statischen Methode umfasst die Verwendung von `JNIEnv.GetStaticMethodID` zum Abrufen eines Methodenhandles und die anschließende Verwendung der geeigneten `JNIEnv.CallStatic*Method`-Methode, je nach Rückgabetyp der Methode. Im Folgenden finden Sie ein Beispiel einer Bindung für die [Runtime.getRuntime](https://developer.android.com/reference/java/lang/Runtime.html#getRuntime())-Methode:

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

Beachten Sie, dass das Methodenhandle in einem statischen Feld gespeichert wird: `id_getRuntime`. Dies ist eine Leistungsoptimierung, da das Methodenhandle nicht bei jedem Aufruf gesucht werden muss. Es ist nicht notwendig, das Methodenhandle auf diese Weise zwischenzuspeichern. Sobald das Methodenhandle abgerufen wurde, wird [JNIEnv.CallStaticObjectMethod](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*) verwendet, um die Methode aufzurufen. `JNIEnv.CallStaticObjectMethod` gibt ein `IntPtr`-Element zurück, das das Handle der zurückgegebenen Java-Instanz enthält.
[Java.Lang.Object.GetObject&lt;T&gt;(IntPtr, JniHandleOwnership)](xref:Java.Lang.Object.GetObject*) wird verwendet, um das Java-Handle in eine stark typisierte Objektinstanz zu konvertieren.

#### <a name="non-virtual-instance-method-binding"></a>Binden nicht virtueller Instanzmethoden

Das Binden einer `final`-Instanzmethode oder einer Instanzmethode, die kein Überschreiben erfordert, umfasst die Verwendung von `JNIEnv.GetMethodID` zum Abrufen eines Methodenhandles und die anschließende Verwendung der geeigneten `JNIEnv.Call*Method`-Methode, je nach Rückgabetyp der Methode. Im Folgenden finden Sie ein Beispiel einer Bindung für die `Object.Class`-Eigenschaft:

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

Beachten Sie, dass das Methodenhandle in einem statischen Feld gespeichert wird: `id_getClass`.
Dies ist eine Leistungsoptimierung, da das Methodenhandle nicht bei jedem Aufruf gesucht werden muss. Es ist nicht notwendig, das Methodenhandle auf diese Weise zwischenzuspeichern. Sobald das Methodenhandle abgerufen wurde, wird [JNIEnv.CallStaticObjectMethod](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*) verwendet, um die Methode aufzurufen. `JNIEnv.CallStaticObjectMethod` gibt ein `IntPtr`-Element zurück, das das Handle der zurückgegebenen Java-Instanz enthält.
[Java.Lang.Object.GetObject&lt;T&gt;(IntPtr, JniHandleOwnership)](xref:Java.Lang.Object.GetObject*) wird verwendet, um das Java-Handle in eine stark typisierte Objektinstanz zu konvertieren.

### <a name="binding-constructors"></a>Bindungskonstruktoren

Konstruktoren sind Java-Methoden mit dem Namen `"<init>"`. Genau wie bei Java-Instanzmethoden wird `JNIEnv.GetMethodID` verwendet, um das Konstruktorhandle zu suchen. Im Gegensatz zu Java-Methoden werden die [JNIEnv.NewObject](xref:Android.Runtime.JNIEnv.NewObject*)-Methoden verwendet, um das Konstruktormethodenhandle aufzurufen. Der Rückgabewert von `JNIEnv.NewObject` ist ein lokaler JNI-Verweis:

```csharp
int value = 42;
IntPtr class_ref    = JNIEnv.FindClass ("java/lang/Integer");
IntPtr id_ctor_I    = JNIEnv.GetMethodID (class_ref, "<init>", "(I)V");
IntPtr lrefInstance = JNIEnv.NewObject (class_ref, id_ctor_I, new JValue (value));
// Dispose of lrefInstance, class_ref…
```

Normalerweise erstellt eine Klassenbindung eine Unterklasse von [Java.Lang.Object](xref:Java.Lang.Object).
Beim Erstellen von Unterklassen von `Java.Lang.Object` kommt zusätzliche Semantik ins Spiel: Eine `Java.Lang.Object`-Instanz verwaltet einen globalen Verweis auf eine Java-Instanz über die `Java.Lang.Object.Handle`-Eigenschaft.

1. Der `Java.Lang.Object`-Standardkonstruktor ordnet eine Java-Instanz zu.

1. Wenn der Typ ein `RegisterAttribute` aufweist und `RegisterAttribute.DoNotGenerateAcw` den Wert `true` besitzt, wird eine Instanz des `RegisterAttribute.Name`-Typs durch den zugehörigen Standardkonstruktor erstellt.

1. Andernfalls wird der [Android Callable Wrapper](~/android/platform/java-integration/android-callable-wrappers.md) (ACW), der `this.GetType` entspricht, durch den zugehörigen Standardkonstruktor instanziiert. Android Callable Wrappers werden während der Paketerstellung für jede `Java.Lang.Object`-Unterklasse generiert, für die `RegisterAttribute.DoNotGenerateAcw` nicht auf `true` festgelegt ist.

Bei Typen, bei denen es sich nicht um Klassenbindungen handelt, ist dies die erwartete Semantik: Durch Instanziieren einer C#-Instanz von `Mono.Samples.HelloWorld.HelloAndroid` sollte eine Java-Instanz von `mono.samples.helloworld.HelloAndroid` konstruiert werden, die ein generierter Android Callable Wrapper ist.

Bei Klassenbindungen kann dies das richtige Verhalten sein, wenn der Java-Typ einen Standardkonstruktor enthält und/oder kein weiterer Konstruktor aufgerufen werden muss. Andernfalls muss ein Konstruktor angegeben werden, der die folgenden Aktionen ausführt:

1. Aufrufen von [Java.Lang.Object(IntPtr, JniHandleOwnership)](xref:Java.Lang.Object#ctor*) anstelle des `Java.Lang.Object`-Standardkonstruktors. Dies ist erforderlich, um die Erstellung einer neuen Java-Instanz zu vermeiden.

1. Überprüfen des Werts von [Java.Lang.Object.Handle](xref:Java.Lang.Object.Handle) vor dem Erstellen von Java-Instanzen. Die `Object.Handle`-Eigenschaft weist einen anderen Wert als `IntPtr.Zero` auf, wenn im Java-Code ein Android Callable Wrapper konstruiert wurde, und die Klassenbindung wird so konstruiert, dass sie die erstellte Android Callable Wrapper-Instanz enthält. Ein Beispiel: Wenn Android eine `mono.samples.helloworld.HelloAndroid`-Instanz erstellt, wird zuerst der Android Callable Wrapper erstellt, und der Java-Konstruktor `HelloAndroid` erstellt eine Instanz des entsprechenden `Mono.Samples.HelloWorld.HelloAndroid`-Typs, wobei die `Object.Handle`-Eigenschaft auf die Java-Instanz vor Ausführung des Konstruktors festgelegt wird.

1. Wenn der aktuelle Laufzeittyp nicht derselbe ist wie der deklarierende Typ, muss eine Instanz des entsprechenden Android Callable Wrapper erstellt werden. [Object.SetHandle](xref:Java.Lang.Object.SetHandle*) wird verwendet, um das von [JNIEnv.CreateInstance](xref:Android.Runtime.JNIEnv.CreateInstance*) zurückgegebene Handle zu speichern.

1. Wenn der aktuelle Laufzeittyp derselbe ist wie der deklarierende Typ, wird der Java-Konstruktor aufgerufen und [Object.SetHandle](xref:Java.Lang.Object.SetHandle*) verwendet, um das von `JNIEnv.NewInstance` zurückgegebene Handle zu speichern.

Sehen Sie sich beispielsweise den Konstruktor [java.lang.Integer(int)](https://developer.android.com/reference/java/lang/Integer.html#Integer(int)) an. Dieser wird folgendermaßen gebunden:

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

Die [JNIEnv.CreateInstance](xref:Android.Runtime.JNIEnv.CreateInstance*)-Methoden sind Hilfsmethoden zum Ausführen von `JNIEnv.FindClass`, `JNIEnv.GetMethodID`, `JNIEnv.NewObject` und `JNIEnv.DeleteGlobalReference` für den von `JNIEnv.FindClass` zurückgegebenen Wert. Nähere Informationen finden Sie im nächsten Abschnitt.

<a name="_Supporting_Inheritance,_Interfaces_1" />

### <a name="supporting-inheritance-interfaces"></a>Unterstützen der Vererbung, Schnittstellen

Zum Erstellen von Unterklassen eines Java-Typs oder Implementieren einer Java-Schnittstelle müssen [Android Callable Wrappers](~/android/platform/java-integration/android-callable-wrappers.md) (ACWs) generiert werden. Die Generierung erfolgt für jede `Java.Lang.Object`-Unterklasse während des Paketerstellungsprozesses. Die ACW-Generierung wird durch das benutzerdefinierte Attribut [Android.Runtime.RegisterAttribute](xref:Android.Runtime.RegisterAttribute) gesteuert.

Bei C#-Typen erfordert der Konstruktor für das benutzerdefinierte `[Register]`-Attribut genau ein Argument: den [vereinfachten JNI-Typverweis](#_Simplified_Type_References_1) für den entsprechenden Java-Typ. Auf diese Weise können in Java und C# unterschiedliche Namen angegeben werden.

Vor Xamarin.Android 4.0 war das benutzerdefinierte `[Register]`-Attribut für vor Java-„Aliastypen“ nicht verfügbar. Dies liegt daran, dass der ACW-Generierungsprozess ACWs für jede vorhandene `Java.Lang.Object`-Unterklasse generieren würde.

In Xamarin.Android 4.0 wurde die Eigenschaft [RegisterAttribute.DoNotGenerateAcw](xref:Android.Runtime.RegisterAttribute.DoNotGenerateAcw) eingeführt. Diese Eigenschaft weist den ACW-Generierungsprozess an, den mit Anmerkungen versehenen Typ zu *überspringen*. Dadurch können neue Managed Callable Wrappers deklariert werden, die nicht dazu führen, dass zum Zeitpunkt der Paketerstellung ACWs generiert werden. Dies ermöglicht das Binden vorhandener Java-Typen. Sehen Sie sich als Beispiel die folgende einfache Java-Klasse an: `Adder`. Diese enthält eine Methode – `add` –, die ganze Zahlen addiert und das Ergebnis zurückgibt:

```java
package mono.android.test;
public class Adder {
    public int add (int a, int b) {
        return a + b;
    }
}
```

Der `Adder`-Typ könnte folgendermaßen gebunden werden:

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

Hier ist der C#-Typ `Adder` ein *Alias* für den Java-Typ `Adder`. Mit dem `[Register]`-Attribut wird der JNI-Name des Java-Typs `mono.android.test.Adder` angegeben, und mit der `DoNotGenerateAcw`-Eigenschaft wird die ACW-Generierung unterbunden. Dies führt zur Generierung eines ACW für den `ManagedAdder`-Typ, der ordnungsgemäß Unterklassen für den `mono.android.test.Adder`-Typ erstellt. Ohne Verwendung der `RegisterAttribute.DoNotGenerateAcw`-Eigenschaft hätte der Xamarin.Android-Buildprozess einen neuen `mono.android.test.Adder`-Java-Typ generiert. Dies hätte zu Kompilierungsfehlern geführt, weil der `mono.android.test.Adder`-Typ zweimal – in zwei separaten Dateien – vorhanden gewesen wäre.

### <a name="binding-virtual-methods"></a>Binden von virtuellen Methoden

`ManagedAdder` erstellt Unterklassen für den Java-Typ `Adder`, dies ist aber weniger interessant: Der C#-Typ `Adder` definiert keine virtuellen Methoden, daher kann `ManagedAdder` nichts überschreiben.

Zum Binden von `virtual`-Methoden, um ein Überschreiben durch Unterklassen zuzulassen, müssen verschiedene Vorgänge ausgeführt werden, die sich in die folgenden beiden Kategorien unterteilen lassen:

1. **Methodenbindung**

1. **Methodenregistrierung**

#### <a name="method-binding"></a>Methodenbindung

Für eine Methodenbindung müssen der C#-Definition `Adder` zwei unterstützende Member hinzugefügt werden: `ThresholdType` und `ThresholdClass`.

##### <a name="thresholdtype"></a>ThresholdType

Die Eigenschaft `ThresholdType` gibt den aktuellen Typ der Bindung zurück:

```csharp
partial class Adder {
    protected override System.Type ThresholdType {
        get {
            return typeof (Adder);
        }
    }
}
```

Mit dem `ThresholdType` in der Methodenbindung wird bestimmt, wann eine virtuelle und wann eine nicht virtuelle Methodenverteilung (dispatch) durchgeführt wird. Diese Eigenschaft sollte immer eine `System.Type`-Instanz zurückgeben, die dem deklarierenden C#-Typ entspricht.

##### <a name="thresholdclass"></a>ThresholdClass

Die Eigenschaft `ThresholdClass` gibt den JNI-Klassenverweis für den gebundenen Typ zurück:

```csharp
partial class Adder {
    protected override IntPtr ThresholdClass {
        get {
            return class_ref;
        }
    }
}
```

`ThresholdClass` wird in der Methodenbindung verwendet, wenn nicht virtuelle Methoden aufgerufen werden.

#### <a name="binding-implementation"></a>Implementierung der Bindung

Die Implementierung der Methodenbindung ist für den Aufruf der Java-Methode zur Laufzeit zuständig. Sie enthält auch eine Deklaration für das benutzerdefinierte `[Register]`-Attribut, die zur Methodenregistrierung gehört und im Abschnitt zur Methodenregistrierung erläutert wird:

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

Das `id_add`-Feld enthält die Methoden-ID für die Java-Methode, die aufgerufen werden soll. Der `id_add`-Wert wird aus `JNIEnv.GetMethodID` abgerufen. Dafür ist die deklarierende Klasse (`class_ref`), der Name der Java-Methode (`"add"`) und die JNI-Signatur der Methode (`"(II)I"`) erforderlich.

Sobald die Methoden-ID abgerufen wurde, wird `GetType` mit `ThresholdType` verglichen, um zu bestimmen, ob eine virtuelle oder eine nicht virtuelle Verteilung erforderlich ist. Eine virtuelle Verteilung ist erforderlich, wenn `GetType` mit `ThresholdType` übereinstimmt, da `Handle` auf eine in Java zugeordnete Unterklasse verweisen kann, die die Methode überschreibt.

Wenn `GetType` nicht mit `ThresholdType` übereinstimmt, wurde eine Unterklasse von `Adder` erstellt (z. B. durch `ManagedAdder`), und die Implementierung von `Adder.Add` wird nur aufgerufen, wenn die Unterklasse `base.Add` aufgerufen hat. Dies ist der Fall der nicht virtuellen Verteilung, und hierbei kommt `ThresholdClass` ins Spiel. `ThresholdClass` gibt an, welche Java-Klasse die Implementierung der aufzurufenden Methode bereitstellt.

#### <a name="method-registration"></a>Methodenregistrierung

Nehmen Sie einmal an, Sie hätten eine aktualisierte `ManagedAdder`-Definition, die die `Adder.Add`-Methode überschreibt:

```csharp
partial class ManagedAdder : Adder {
    public override int Add (int a, int b) {
        return (a*2) + (b*2);
    }
}
```

Wie Sie sich erinnern, verfügte `Adder.Add` über ein benutzerdefiniertes `[Register]`-Attribut:

```csharp
[Register ("add", "(II)I", "GetAddHandler")]
```

Der Konstruktor für das benutzerdefinierte `[Register]`-Attribut akzeptiert drei Werte:

1. Den Namen der Java-Methode – in diesem Fall `"add"`.

1. Die JNI-Typsignatur der Methode – in diesem Fall `"(II)I"`.

1. Die *Connectormethode* – in diesem Fall `GetAddHandler`.
    Connectormethoden werden weiter unten erläutert.

Die ersten beiden Parameter ermöglichen es dem Prozess zur ACW-Generierung, eine Methodendeklaration zu generieren, um die Methode zu überschreiben. Der resultierende ACW enthält einen Teil des folgenden Codes:

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

Beachten Sie, dass eine `@Override`-Methode deklariert wird, die an eine gleichnamige Methode mit dem Präfix `n_` delegiert wird. So ist sichergestellt, dass beim Aufrufen von `ManagedAdder.add` durch den Java-Code `ManagedAdder.n_add` aufgerufen wird, wodurch die überschreibende C#-Methode `ManagedAdder.Add` ausgeführt werden darf.

Daher lautet die wichtigste Frage: Wie ist `ManagedAdder.n_add` an `ManagedAdder.Add` gekoppelt?

`native` Java-Methoden sind bei der Java-Runtime (der Android-Runtime) durch die [JNI-Funktion „RegisterNatives“](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp17734) registriert.
`RegisterNatives` akzeptiert ein Array aus Strukturen, das den Java-Methodennamen, die JNI-Typsignatur und einen Funktionszeiger für den Aufruf enthält, der der [JNI-Aufrufkonvention](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/design.html#wp715) folgt.
Der Funktionszeiger muss eine Funktion sein, die zwei Zeigerargumente gefolgt von den Methodenparametern akzeptiert. Die Java-Methode `ManagedAdder.n_add` muss durch eine Funktion implementiert werden, die den folgenden C-Prototyp aufweist:

```csharp
int FunctionName(JNIEnv *env, jobject this, int a, int b)
```

Xamarin.Android macht keine `RegisterNatives`-Methode verfügbar. Stattdessen stellen der ACW und der MCW gemeinsam die Informationen bereit, die zum Aufrufen von `RegisterNatives` erforderlich sind: der ACW enthält den Methodennamen und die JNI-Typsignatur. Das einzige, was fehlt, ist ein Funktionszeiger zur Kopplung.

Hier kommt die *Connectormethode* ins Spiel. Der dritte Parameter des benutzerdefinierten `[Register]`-Attributs ist der Name einer im registrierten Typ definierten Methode oder einer Basisklasse des registrierten Typs, die keine Parameter akzeptiert und einen `System.Delegate` zurückgibt. Der zurückgegebene `System.Delegate` wiederum verweist auf eine Methode, die über die korrekte JNI-Funktionssignatur verfügt. Nicht zuletzt *muss* der von der Connectormethode zurückgegebene Delegat gerootet sein, sodass er von der Garbage Collection nicht bereinigt wird, da er für Java bereitgestellt wird.

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
int>`-Delegaten, der auf die `n_Add`-Methode verweist, und ruft dann [JNINativeWrapper.CreateDelegate](xref:Android.Runtime.JNINativeWrapper.CreateDelegate*) auf.
`JNINativeWrapper.CreateDelegate` umschließt die bereitgestellte Methode in einem try/catch-Block, sodass alle nicht Ausnahmefehler behandelt werden, und führt zum Auslösen des [AndroidEvent.UnhandledExceptionRaiser](xref:Android.Runtime.AndroidEnvironment.UnhandledExceptionRaiser)-Ereignisses. Der resultierende Delegat wird in der statischen Variable `cb_add` gespeichert, sodass die Garbage Collection den Delegaten nicht freigibt.

Und schließlich ist die `n_Add`-Methode für das Marshalling der JNI-Parameter in die entsprechenden verwalteten Typen sowie für das Delegieren des Methodenaufrufs zuständig.

Hinweis: Verwenden Sie immer `JniHandleOwnership.DoNotTransfer`, wenn Sie einen MCW über eine Java-Instanz abrufen. Eine Behandlung der Wrapper als lokaler Verweis (und damit einhergehend der Aufruf von `JNIEnv.DeleteLocalRef`) unterbricht die Übergänge verwalteter Stapel -&gt; Java-Stapel -&gt; verwalteter Stapel.

### <a name="complete-adder-binding"></a>Abschließen der Adder-Bindung

Hier finden Sie die vollständige verwaltete Bindung für den `mono.android.tests.Adder`-Typ:

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

Beim Schreiben eines Typs, der den folgenden Kriterien entspricht:

1. Unterklassen von `Java.Lang.Object` werden erstellt.

1. Verfügt über ein benutzerdefiniertes `[Register]`-Attribut.

1. `RegisterAttribute.DoNotGenerateAcw` ist gleich `true`.

Dann gilt für die Garbage Collection-Interaktion: Der Typ *darf keine* Felder enthalten, die zur Laufzeit auf `Java.Lang.Object` oder eine Unterklasse von `Java.Lang.Object` verweisen. Felder vom Typ `System.Object` und Schnittstellentypen sind beispielsweise nicht zulässig. Typen, die nicht auf `Java.Lang.Object`-Instanzen verweisen können, sind zulässig, z. B. `System.String` und `List<int>`. Diese Beschränkung wurde eingeführt, um eine vorzeitige Objektbereinigung durch die Garbage Collection zu verhindern.

Wenn der Typ ein Instanzfeld enthalten muss, das auf eine `Java.Lang.Object`-Instanz verweisen kann, muss der Feldtyp `System.WeakReference` oder `GCHandle` lauten.

## <a name="binding-abstract-methods"></a>Binden abstrakter Methoden

Das Binden von `abstract`-Methoden ist in weiten Teilen identisch mit dem Binden von virtuellen Methoden. Es gibt nur zwei Unterschiede:

1. Die abstract-Methode ist abstrakt. Sie behält das `[Register]`-Attribut und die zugehörige Methodenregistrierung weiterhin bei, und die Methodenbindung wird einfach nur in den `Invoker`-Typ verschoben.

1. Ein nicht abstrakter `Invoker`-Typ wird erstellt, der Unterklassen des abstrakten Typs erstellt. Der `Invoker`-Typ muss alle abstrakten Methoden überschreiben, die in der Basisklasse deklariert sind, und die überschriebene Implementierung ist die Implementierung der Methodenbindung, obwohl der Fall einer nicht virtuellen Verteilung ignoriert werden kann.

Nehmen Sie beispielsweise an, die oben genannte `mono.android.test.Adder.add`-Methode wäre `abstract`. Die C#-Bindung würde geändert, sodass `Adder.Add` abstrakt wäre, und es würde ein neuer `AdderInvoker`-Typ definiert, der `Adder.Add` implementieren würde:

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

Der `Invoker`-Typ ist nur erforderlich, wenn JNI-Verweise auf in Java erstellte Instanzen abgerufen werden.

## <a name="binding-interfaces"></a>Binden von Schnittstellen

Das Binden von Schnittstellen folgt den gleichen Konzepten wie das Binden von Klassen, die virtuelle Methoden enthalten. Viele Details weisen allerdings feine (und nicht so feine) Unterschiede auf. Sehen Sie sich die folgende [Java-Schnittstellendeklaration](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/Adder.java#L14) an:

```csharp
public interface Progress {
    void onAdd(int[] values, int currentIndex, int currentSum);
}
```

Schnittstellenbindungen bestehen aus zwei Teilen: der C#-Schnittstellendefinition und einer Invokerdefinition für die Schnittstelle.

### <a name="interface-definition"></a>Schnittstellendefinition

Die C#-Schnittstellendefinition muss die folgenden Anforderungen erfüllen:

- Die Schnittstellendefinition muss über ein benutzerdefiniertes `[Register]`-Attribut verfügen.

- Die Schnittstellendefinition muss die `IJavaObject interface` erweitern.
    Wenn diese Anforderung nicht erfüllt ist, können ACWs nicht von der Java-Schnittstelle erben.

- Jede Schnittstellenmethode muss ein `[Register]`-Attribut zur Angabe des Namens der entsprechenden Java-Methode, die JNI-Signatur und die Connectormethode enthalten.

- Die Connectormethode muss auch den Typ angeben, in dem die Connectormethode zu finden ist.

Beim Binden der Methoden `abstract` und `virtual` wird in der Vererbungshierarchie des Typs, der registriert wird, nach der Connectormethode gesucht. Schnittstellen dürfen keine Methoden mit Text enthalten, daher funktioniert diese Vorgehensweise nicht. Aus diesem Grund muss angegeben werden, in welchem Typ sich die Connectormethode befindet. Der Typ wird in der Zeichenfolge der Connectormethode nach einem Doppelpunkt (`':'`) angegeben, und es muss sich um den in der Assembly qualifizierten Typnamen des Typs handeln, der den Invoker enthält.

Methodendeklarationen für Schnittstellen sind eine Übersetzung der entsprechenden Java-Methode unter Verwendung von *kompatiblen* Typen. Bei integrierten Java-Typen sind die kompatiblen Typen die entsprechenden C#-Typen, `int` in Java entspricht also `int` in C#. Bei Verweistypen ist der kompatible Typ ein Typ, der ein JNI-Handle des geeigneten Java-Typs bereitstellen kann.

Die Schnittstellenmember werden nicht direkt durch Java aufgerufen &ndash; der Aufruf erfolgt über den Invokertyp &ndash;, daher ist ein gewisses Maß an Flexibilität zulässig.

Die Java-Fortschrittsschnittstelle kann [in C# wie folgt deklariert werden](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L83):

```csharp
[Register ("mono/android/test/Adder$Progress", DoNotGenerateAcw=true)]
public interface IAdderProgress : IJavaObject {
    [Register ("onAdd", "([III)V",
            "GetOnAddHandler:Mono.Samples.SanityTests.IAdderProgressInvoker, SanityTests, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")]
    void OnAdd (JavaArray<int> values, int currentIndex, int currentSum);
}
```

Beachten Sie im Beispiel oben, dass der Java-Parameter `int[]` einem [JavaArray&lt;int&gt;](xref:Android.Runtime.JavaArray`1)-Typ zugeordnet wird.
Dies ist nicht notwendig, die Bindung hätte auch an einen `int[]`-Typ in C#, einen `IList<int>`-Typ oder etwas völlig anderes erfolgen können. Welcher Typ auch ausgewählt wird, der `Invoker` muss in der Lage sein, diesen Typ zum Aufruf in einen `int[]`-Java-Typ zu übersetzen.

### <a name="invoker-definition"></a>Invokerdefinition

Die `Invoker`-Typdefinition muss `Java.Lang.Object` erben, die entsprechende Schnittstelle implementieren und alle Verbindungsmethoden bereitstellen, auf die in der Schnittstellendefinition verwiesen wird. Es gibt noch eine weitere Empfehlung, die sich von der Klassenbindung unterscheidet: das `class_ref`-Feld und die Methoden-IDs sollten Instanzmember sein, keine statischen Member.

Der Grund, aus dem Instanzmember zu bevorzugen sind, hat mit dem `JNIEnv.GetMethodID`-Verhalten in der Android-Runtime zu tun. (Dieses Verhalten tritt möglicherweise auch in Java auf, dies wurde noch nicht getestet.) Beim Suchen nach einer Methode, die nicht aus der deklarierten, sondern aus einer implementierten Schnittstelle stammt, gibt `JNIEnv.GetMethodID` NULL zurück. Die Java-Schnittstelle [java.util.SortedMap&lt;K, V&gt;](https://developer.android.com/reference/java/util/SortedMap.html) implementiert die Schnittstelle [java.util.Map&lt;K, V&gt;](https://developer.android.com/reference/java/util/Map.html). Map stellt eine [clear](https://developer.android.com/reference/java/util/Map.html#clear())-Methode bereit, daher ist folgende `Invoker`-Definition für SortedMap scheinbar vernünftig:

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

Beim obigen Beispiel tritt ein Fehler auf, weil `JNIEnv.GetMethodID` beim Suchen nach der `Map.clear`-Methode in der `SortedMap`-Klasseninstanz `null` zurückgibt.

Hierfür gibt es zwei Lösungen: Sie können nachverfolgen, aus welcher Schnittstelle jede Methode stammt, und ein `class_ref`-Element für jede Schnittstelle bereitstellen. Alternativ dazu können Sie alle Elemente als Instanzmember beibehalten und die Methodensuche im Klassentyp mit den meisten Ableitungen ausführen, nicht im Schnittstellentyp. Letzteres erfolgt in **Mono.Android.dll**.

Die Invokerdefinition besteht aus sechs Abschnitten: dem Konstruktor, der `Dispose`-Methode, den Membern `ThresholdType` und `ThresholdClass`, der `GetObject`-Methode, der Implementierung der Schnittstellenmethode und der Implementierung der Connectormethode.

#### <a name="constructor"></a>Konstruktor

Der Konstruktor muss die Runtimeklasse der aufgerufenen Instanz suchen und diese Klasse im Instanzfeld `class_ref` speichern:

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

Hinweis: Im Konstruktortext muss die `Handle`-Eigenschaft verwendet werden, nicht der `handle`-Parameter, da unter Android v4.0 der `handle`-Parameter möglicherweise ungültig ist, nachdem die Ausführung des Basiskonstruktors abgeschlossen ist.

#### <a name="dispose-method"></a>Dispose-Methode

Die `Dispose`-Methode muss den globalen Verweis freigeben, der im Konstruktor zugeordnet ist:

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

#### <a name="thresholdtype-and-thresholdclass"></a>ThresholdType und ThresholdClass

Die Member `ThresholdType` und `ThresholdClass` sind hinsichtlich der Klassenbindung identisch:

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

Zur Unterstützung von [Extensions.JavaCast&lt;T&gt;()](xref:Android.Runtime.Extensions.JavaCast*) ist eine statische `GetObject`-Methode erforderlich:

```csharp
partial class IAdderProgressInvoker {
    public static IAdderProgress GetObject (IntPtr handle, JniHandleOwnership transfer)
    {
        return new IAdderProgressInvoker (handle, transfer);
    }
}
```

#### <a name="interface-methods"></a>Schnittstellenmethoden

Jede Methode der Schnittstelle muss über eine Implementierung verfügen, die die entsprechende Java-Methode über die JNI aufruft:

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

#### <a name="connector-methods"></a>Connectormethoden

Die Connectormethoden und die unterstützende Infrastruktur sind für das Marshallen der JNI-Parameter in geeignete C#-Typen zuständig. Der Java-Parameter `int[]` wird als `jintArray` der JNI übergeben – dies entspricht dem `IntPtr`-Element in C#. Das `IntPtr`-Element muss in ein `JavaArray<int>` gemarshallt werden, um das Aufrufen der C#-Schnittstelle zu unterstützen:

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

Wenn `int[]` der Vorzug vor `JavaList<int>` gegeben würde, könnte stattdessen [JNIEnv.GetArray()](xref:Android.Runtime.JNIEnv.GetArray*) verwendet werden:

```csharp
int[] _values = (int[]) JNIEnv.GetArray(values, JniHandleOwnership.DoNotTransfer, typeof (int));
```

Beachten Sie jedoch, dass `JNIEnv.GetArray` das gesamte Array zwischen den VMs kopiert. Bei großen Arrays könnte dies also zu einer hohen Auslastung der Garbage Collection führen.

### <a name="complete-invoker-definition"></a>Vollständige Invokerdefinition

Hier finden Sie die [vollständige Definition von IAdderProgressInvoker](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L88):

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

## <a name="jni-object-references"></a>JNI-Objektverweise

Viele JNIEnv-Methoden geben *JNI*-*Objektverweise* zurück, die `GCHandle`-Elementen ähneln. JNI bietet drei verschiedene Typen von Objektverweisen: lokale Verweise, globale Verweise und schwache globale Verweise. Alle drei werden als `System.IntPtr` dargestellt, *aber* (gemäß Abschnitt zu JNI-Funktionstypen) nicht alle `IntPtr`-Elemente, die aus `JNIEnv`-Methoden zurückgegeben werden, sind Verweise. [JNIEnv.GetMethodID](xref:Android.Runtime.JNIEnv.GetMethodID*) beispielsweise gibt ein `IntPtr`-Element zurück, aber nicht als Objektverweis, sondern als `jmethodID`. Weitere Informationen dazu finden Sie in der [JNI-Funktionsdokumentation](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html).

Lokale Verweise werden von den *meisten* Methoden erstellt, die Verweise erstellen.
Android lässt nur eine begrenzte Anzahl von lokalen Verweisen zu, in der Regel 512. Lokale Verweise können über [JNIEnv.DeleteLocalRef](xref:Android.Runtime.JNIEnv.DeleteLocalRef*) gelöscht werden.
Anders als bei der JNI geben nicht alle JNIEnv-Verweismethoden, die Objektverweise zurückgeben, lokale Verweise zurück. [JNIEnv.FindClass](xref:Android.Runtime.JNIEnv.FindClass*) z. B. gibt einen *globalen* Verweis zurück. Es wird dringend empfohlen, lokale Verweise so schnell wie möglich zu löschen. Sie können möglicherweise ein [Java.Lang.Object](xref:Java.Lang.Object) um das Objekt erstellen und `JniHandleOwnership.TransferLocalRef` für den Konstruktor [Java.Lang.Object(IntPtr handle, JniHandleOwnership transfer)](xref:Java.Lang.Object#ctor*) angeben.

Globale Verweise werden auch von [JNIEnv.NewGlobalRef](xref:Android.Runtime.JNIEnv.NewGlobalRef*) und [JNIEnv.FindClass](xref:Android.Runtime.JNIEnv.FindClass*) erstellt.
Diese können mit [JNIEnv.DeleteGlobalRef](xref:Android.Runtime.JNIEnv.DeleteGlobalRef*) zerstört werden.
Für Emulatoren gilt eine Obergrenze von 2.000 ausstehenden globalen Verweisen, während für Hardwaregeräte bis zu etwa 52.000 globale Verweise existieren können.

Schwache globale Verweise sind nur unter Android v2.2 (Froyo) und höher verfügbar. Schwache globale Verweise können mit [JNIEnv.DeleteWeakGlobalRef](xref:Android.Runtime.JNIEnv.DeleteWeakGlobalRef*) gelöscht werden.

### <a name="dealing-with-jni-local-references"></a>Verarbeiten von lokalen JNI-Verweisen

Die Methoden [JNIEnv.GetObjectField](xref:Android.Runtime.JNIEnv.GetObjectField*), [JNIEnv.GetStaticObjectField](xref:Android.Runtime.JNIEnv.GetStaticObjectField*), [JNIEnv.CallObjectMethod](xref:Android.Runtime.JNIEnv.CallObjectMethod*), [JNIEnv.CallNonvirtualObjectMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualObjectMethod*) und [JNIEnv.CallStaticObjectMethod](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*) geben ein `IntPtr`-Element mit einem lokalen JNI-Verweis auf ein Java-Objekt oder `IntPtr.Zero` zurück, wenn Java `null` zurückgegeben hat. Aufgrund der begrenzten Anzahl lokaler Verweise, die gleichzeitig ausstehend sein können (512 Einträge), ist es wünschenswert, sicherstellen zu können, dass die Verweise schnell gelöscht werden können. Es gibt drei Möglichkeiten zur Behandlung von lokalen Verweisen: Sie können explizit gelöscht werden, es kann eine `Java.Lang.Object`-Instanz erstellt werden, um sie zu speichern, und es kann `Java.Lang.Object.GetObject<T>()` verwendet werden, um einen Managed Callable Wrapper um sie zu erstellen.

### <a name="explicitly-deleting-local-references"></a>Explizites Löschen von lokalen Verweisen

Zum Löschen von lokalen Verweisen wird [JNIEnv.DeleteLocalRef](xref:Android.Runtime.JNIEnv.DeleteLocalRef*) verwendet. Sobald der lokale Verweis gelöscht wurde, kann er nicht mehr verwendet werden. Lassen Sie daher Vorsicht walten, und stellen Sie sicher, dass `JNIEnv.DeleteLocalRef` der letzte Vorgang mit dem lokalen Verweis ist.

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
try {
    // Do something with `lref`
}
finally {
    JNIEnv.DeleteLocalRef (lref);
}
```

### <a name="wrapping-with-javalangobject"></a>Umschließen mit Java.Lang.Object

`Java.Lang.Object` stellt einen [Java.Lang.Object(IntPtr handle, JniHandleOwnership transfer)](xref:Java.Lang.Object#ctor*)-Konstruktor bereit, der zum Umschließen eines vorhandenen JNI-Verweises verwendet werden kann. Der Parameter [JniHandleOwnership](xref:Android.Runtime.JniHandleOwnership) bestimmt, wie der Parameter `IntPtr` behandelt werden soll:

- [JniHandleOwnership.DoNotTransfer](xref:Android.Runtime.JniHandleOwnership.DoNotTransfer) &ndash; die erstellte `Java.Lang.Object`-Instanz erstellt einen neuen globalen Verweis aus dem `handle`-Parameter, und `handle` bleibt unverändert.
    Das aufrufende Element ist für die Freigabe von `handle` zuständig, sofern erforderlich.

- [JniHandleOwnership.TransferLocalRef](xref:Android.Runtime.JniHandleOwnership.TransferLocalRef) &ndash; die erstellte `Java.Lang.Object`-Instanz erstellt einen neuen globalen Verweis aus dem `handle`-Parameter, und `handle` wird mit [JNIEnv.DeleteLocalRef](xref:Android.Runtime.JNIEnv.DeleteLocalRef*) gelöscht. Das aufrufende Element darf `handle` nicht freigeben. Außerdem darf `handle` nicht mehr verwendet werden, nachdem die Ausführung des Konstruktors beendet wurde.

- [JniHandleOwnership.TransferGlobalRef](xref:Android.Runtime.JniHandleOwnership.TransferLocalRef) &ndash; die erstellte `Java.Lang.Object`-Instanz übernimmt den Besitz des `handle`-Parameters. Das aufrufende Element darf `handle` nicht freigeben.

Da die JNI-Methoden für den Methodenaufruf lokale Verweise zurückgeben, wird im Regelfall `JniHandleOwnership.TransferLocalRef` verwendet:

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef);
```

Der erstellte globale Verweis wird erst dann freigegeben, wenn die `Java.Lang.Object`-Instanz durch die Garbage Collection bereinigt wurde. Sofern dies möglich ist: Durch Löschen der Instanz wird der globale Verweis freigegeben, sodass Garbage Collection-Vorgänge beschleunigt werden:

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
using (var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef)) {
    // use value ...
}
```

### <a name="using-javalangobjectgetobjectlttgt"></a>Verwenden von Java.Lang.Object.GetObject&lt;T&gt;()

`Java.Lang.Object` stellt eine [Java.Lang.Object.GetObject&lt;T&gt;(IntPtr handle, JniHandleOwnership transfer)](xref:Java.Lang.Object.GetObject*)-Methode bereit, mit der ein Managed Callable Wrapper des angegebenen Typs erstellt werden kann.

Der Typ `T` muss folgende Anforderungen erfüllen:

1. `T` muss ein Verweistyp sein.

1. `T` muss die `IJavaObject`-Schnittstelle implementieren.

1. Wenn `T` keine abstrakte Klasse oder Schnittstelle ist, muss `T` einen Konstruktor mit den Parametertypen `(IntPtr,
    JniHandleOwnership)` bereitstellen.

1. Wenn `T` eine abstrakte Klasse oder Schnittstelle ist, *muss* ein *Invoker* für `T` verfügbar sein. Ein Invoker ist ein nicht abstrakter Typ, der `T` erbt oder `T` implementiert, und weist denselben Namen wie `T` mit Invokersuffix auf. Ein Beispiel: Wenn „T“ die `Java.Lang.IRunnable`-Schnittstelle ist, muss der `Java.Lang.IRunnableInvoker`-Typ vorhanden sein und den erforderlichen `(IntPtr,
    JniHandleOwnership)`-Konstruktor enthalten.

Da die JNI-Methoden für den Methodenaufruf lokale Verweise zurückgeben, wird im Regelfall `JniHandleOwnership.TransferLocalRef` verwendet:

```csharp
IntPtr lrefString = JNIEnv.CallObjectMethod(instance, methodID);
Java.Lang.String value = Java.Lang.Object.GetObject<Java.Lang.String>( lrefString, JniHandleOwnership.TransferLocalRef);
```

<a name="_Looking_up_Java_Types" />

## <a name="looking-up-java-types"></a>Suchen nach Java-Typen

Um ein Feld oder eine Methode in der JNI zu suchen, muss zuerst der deklarierende Typ für das Feld oder die Methode gesucht werden. Die Methode [Android.Runtime.JNIEnv.FindClass(string)](xref:Android.Runtime.JNIEnv.FindClass*)) wird zum Suchen nach Java-Typen verwendet. Der Zeichenfolgenparameter ist der *vereinfachte Typverweis* oder der *vollständige Typverweis* für den Java-Typ. Informationen zu vereinfachten und vollständigen Typverweisen finden Sie im Abschnitt [JNI-Typverweise](#_JNI_Type_References).

Hinweis: Im Gegensatz zu allen anderen `JNIEnv`-Methoden, die Objektinstanzen zurückgeben, gibt `FindClass` einen globalen Verweis zurück, keinen lokalen.

<a name="_Instance_Fields" />

## <a name="instance-fields"></a>Instanzfelder

Felder werden durch *Feld-IDs* geändert. Feld-IDs werden mit [JNIEnv.GetFieldID](xref:Android.Runtime.JNIEnv.GetFieldID*) abgerufen. Dafür sind die Klasse, in der das Feld definiert ist, der Name des Felds und die [JNI-Typsignatur](#JNI_Type_Signatures) des Felds erforderlich.

Feld-IDs müssen nicht freigegeben werden und sind gültig, solange der entsprechende Java-Typ geladen ist. (Android unterstützt derzeit das Entladen von Klassen nicht.)

Es gibt zwei Gruppen von Methoden zum Ändern von Instanzfeldern: eine zum Lesen von Instanzfeldern und eine zum Schreiben von Instanzfeldern. Beide Methodengruppen erfordern eine Feld-ID zum Lesen oder Schreiben des Feldwerts.

### <a name="reading-instance-field-values"></a>Lesen von Instanzfeldwerten

Die Gruppe von Methoden zum Lesen von Instanzfeldwerten weist folgendes Namensmuster auf:

```csharp
* JNIEnv.Get*Field(IntPtr instance, IntPtr fieldID);
```

Dabei ist `*` der Typ des Felds:

- [JNIEnv.GetObjectField](xref:Android.Runtime.JNIEnv.GetObjectField*) &ndash; zum Lesen des Werts eines Instanzfelds, das keinen integrierten Typ besitzt, z. B. `java.lang.Object`, Arrays und Schnittstellentypen. Der zurückgegebene Wert ist ein lokaler JNI-Verweis.

- [JNIEnv.GetBooleanField](xref:Android.Runtime.JNIEnv.GetBooleanField*) &ndash; zum Lesen des Werts von `bool`-Instanzfeldern.

- [JNIEnv.GetByteField](xref:Android.Runtime.JNIEnv.GetByteField*) &ndash; zum Lesen des Werts von `sbyte`-Instanzfeldern.

- [JNIEnv.GetCharField](xref:Android.Runtime.JNIEnv.GetCharField*) &ndash; zum Lesen des Werts von `char`-Instanzfeldern.

- [JNIEnv.GetShortField](xref:Android.Runtime.JNIEnv.GetShortField*) &ndash; zum Lesen des Werts von `short`-Instanzfeldern.

- [JNIEnv.GetIntField](xref:Android.Runtime.JNIEnv.GetIntField*) &ndash; zum Lesen des Werts von `int`-Instanzfeldern.

- [JNIEnv.GetLongField](xref:Android.Runtime.JNIEnv.GetLongField*) &ndash; zum Lesen des Werts von `long`-Instanzfeldern.

- [JNIEnv.GetFloatField](xref:Android.Runtime.JNIEnv.GetFloatField*) &ndash; zum Lesen des Werts von `float`-Instanzfeldern.

- [JNIEnv.GetDoubleField](xref:Android.Runtime.JNIEnv.GetDoubleField*) &ndash; zum Lesen des Werts von `double`-Instanzfeldern.

### <a name="writing-instance-field-values"></a>Schreiben von Instanzfeldwerten

Die Gruppe von Methoden zum Schreiben von Instanzfeldwerten weist folgendes Namensmuster auf:

```csharp
JNIEnv.SetField(IntPtr instance, IntPtr fieldID, Type value);
```

Dabei ist *Type* der Typ des Felds:

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*) &ndash; zum Schreiben des Werts eines Felds, das keinen integrierten Typ besitzt, z. B. `java.lang.Object`, Arrays und Schnittstellentypen. Der `IntPtr`-Wert kann ein lokaler JNI-Verweis, ein globaler JNI-Verweis, ein schwacher globaler JNI-Verweis oder (für `null`) `IntPtr.Zero` sein.

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*) &ndash; zum Schreiben des Werts von `bool`-Instanzfeldern.

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*) &ndash; zum Schreiben des Werts von `sbyte`-Instanzfeldern.

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*) &ndash; zum Schreiben des Werts von `char`-Instanzfeldern.

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*) &ndash; zum Schreiben des Werts von `short`-Instanzfeldern.

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*) &ndash; zum Schreiben des Werts von `int`-Instanzfeldern.

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*) &ndash; zum Schreiben des Werts von `long`-Instanzfeldern.

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*) &ndash; zum Schreiben des Werts von `float`-Instanzfeldern.

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*) &ndash; zum Schreiben des Werts von `double`-Instanzfeldern.

<a name="_Static_Fields" />

## <a name="static-fields"></a>Statische Felder

Statische Felder werden durch *Feld-IDs* geändert. Feld-IDs werden mit [JNIEnv.GetStaticFieldID](xref:Android.Runtime.JNIEnv.GetStaticFieldID*) abgerufen. Dafür sind die Klasse, in der das Feld definiert ist, der Name des Felds und die [JNI-Typsignatur](#JNI_Type_Signatures) des Felds erforderlich.

Feld-IDs müssen nicht freigegeben werden und sind gültig, solange der entsprechende Java-Typ geladen ist. (Android unterstützt derzeit das Entladen von Klassen nicht.)

Es gibt zwei Gruppen von Methoden zum Ändern von statischen Feldern: eine zum Lesen von statischen Feldern und eine zum Schreiben von statischen Feldern. Beide Methodengruppen erfordern eine Feld-ID zum Lesen oder Schreiben des Feldwerts.

### <a name="reading-static-field-values"></a>Lesen von statischen Feldwerten

Die Gruppe von Methoden zum Lesen von statischen Feldwerten weist folgendes Namensmuster auf:

```csharp
* JNIEnv.GetStatic*Field(IntPtr class, IntPtr fieldID);
```

Dabei ist `*` der Typ des Felds:

- [JNIEnv.GetStaticObjectField](xref:Android.Runtime.JNIEnv.GetStaticObjectField*) &ndash; zum Lesen des Werts eines statischen Felds, das keinen integrierten Typ besitzt, z. B. `java.lang.Object`, Arrays und Schnittstellentypen. Der zurückgegebene Wert ist ein lokaler JNI-Verweis.

- [JNIEnv.GetStaticBooleanField](xref:Android.Runtime.JNIEnv.GetStaticBooleanField*) &ndash; zum Lesen des Werts von statischen `bool`-Feldern.

- [JNIEnv.GetStaticByteField](xref:Android.Runtime.JNIEnv.GetStaticByteField*) &ndash; zum Lesen des Werts von statischen `sbyte`-Feldern.

- [JNIEnv.GetStaticCharField](xref:Android.Runtime.JNIEnv.GetStaticCharField*) &ndash; zum Lesen des Werts von statischen `char`-Feldern.

- [JNIEnv.GetStaticShortField](xref:Android.Runtime.JNIEnv.GetStaticShortField*) &ndash; zum Lesen des Werts von statischen `short`-Feldern.

- [JNIEnv.GetStaticLongField](xref:Android.Runtime.JNIEnv.GetStaticLongField*) &ndash; zum Lesen des Werts von statischen `long`-Feldern.

- [JNIEnv.GetStaticFloatField](xref:Android.Runtime.JNIEnv.GetStaticFloatField*) &ndash; zum Lesen des Werts von statischen `float`-Feldern.

- [JNIEnv.GetStaticDoubleField](xref:Android.Runtime.JNIEnv.GetStaticDoubleField*) &ndash; zum Lesen des Werts von statischen `double`-Feldern.

### <a name="writing-static-field-values"></a>Schreiben von statischen Feldwerten

Die Gruppe von Methoden zum Schreiben von statischen Feldwerten weist folgendes Namensmuster auf:

```csharp
JNIEnv.SetStaticField(IntPtr class, IntPtr fieldID, Type value);
```

Dabei ist *Type* der Typ des Felds:

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*) &ndash; zum Schreiben des Werts eines statischen Felds, das keinen integrierten Typ besitzt, z. B. `java.lang.Object`, Arrays und Schnittstellentypen. Der `IntPtr`-Wert kann ein lokaler JNI-Verweis, ein globaler JNI-Verweis, ein schwacher globaler JNI-Verweis oder (für `null`) `IntPtr.Zero` sein.

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*) &ndash; zum Schreiben des Werts von statischen `bool`-Feldern.

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*) &ndash; zum Schreiben des Werts von statischen `sbyte`-Feldern.

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*) &ndash; zum Schreiben des Werts von statischen `char`-Feldern.

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*) &ndash; zum Schreiben des Werts von statischen `short`-Feldern.

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*) &ndash; zum Schreiben des Werts von statischen `int`-Feldern.

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*) &ndash; zum Schreiben des Werts von statischen `long`-Feldern.

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*) &ndash; zum Schreiben des Werts von statischen `float`-Feldern.

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*) &ndash; zum Schreiben des Werts von statischen `double`-Feldern.

<a name="_Instance_Methods" />

## <a name="instance-methods"></a>Instanzmethoden

Instanzmethoden werden durch *Methoden-IDs* aufgerufen. Methoden-IDs werden mit [JNIEnv.GetMethodID](xref:Android.Runtime.JNIEnv.GetMethodID*) abgerufen. Dafür sind der Typ, in dem die Methode Feld definiert ist, der Name der Methode und die [JNI-Typsignatur](#JNI_Type_Signatures) der Methode erforderlich.

Methoden-IDs müssen nicht freigegeben werden und sind gültig, solange der entsprechende Java-Typ geladen ist. (Android unterstützt derzeit das Entladen von Klassen nicht.)

Es gibt zwei Gruppen von Methoden zum Aufrufen von Methoden: eine zum virtuellen Aufrufen von Methoden und eine zum nicht virtuellen Aufrufen von Methoden. Beide Methodengruppen erfordern eine Methoden-ID zum Aufrufen der Methode, bei nicht virtuellen Aufrufen müssen Sie auch angeben, welche Klassenimplementierung aufgerufen werden soll.

Schnittstellenmethoden können nur innerhalb des deklarierenden Typs gesucht werden; Methoden aus erweiterten oder geerbten Schnittstellen können nicht gesucht werden. Weitere Informationen finden Sie im Abschnitt zum Binden von Schnittstellen und zur Implementierung eines Invokers.

Jede in der Klasse, einer beliebigen Basisklasse oder der implementierten Schnittstelle deklarierte Methode kann gesucht werden.

### <a name="virtual-method-invocation"></a>Virtueller Methodenaufruf

Die Gruppe von Methoden zum virtuellen Aufrufen von Methoden weist folgendes Namensmuster auf:

```csharp
* JNIEnv.Call*Method( IntPtr instance, IntPtr methodID, params JValue[] args );
```

Dabei ist `*` der Rückgabetyp der Methode.

- [JNIEnv.CallObjectMethod](xref:Android.Runtime.JNIEnv.CallObjectMethod*) &ndash; zum Aufrufen einer Methode, die einen nicht integrierten Typ zurückgibt, z. B. `java.lang.Object`, Arrays und Schnittstellen. Der zurückgegebene Wert ist ein lokaler JNI-Verweis.

- [JNIEnv.CallBooleanMethod](xref:Android.Runtime.JNIEnv.CallBooleanMethod*) &ndash; zum Aufrufen einer Methode, die einen `bool`-Wert zurückgibt.

- [JNIEnv.CallByteMethod](xref:Android.Runtime.JNIEnv.CallByteMethod*) &ndash; zum Aufrufen einer Methode, die einen `sbyte`-Wert zurückgibt.

- [JNIEnv.CallCharMethod](xref:Android.Runtime.JNIEnv.CallCharMethod*) &ndash; zum Aufrufen einer Methode, die einen `char`-Wert zurückgibt.

- [JNIEnv.CallShortMethod](xref:Android.Runtime.JNIEnv.CallShortMethod*) &ndash; zum Aufrufen einer Methode, die einen `short`-Wert zurückgibt.

- [JNIEnv.CallLongMethod](xref:Android.Runtime.JNIEnv.CallLongMethod*) &ndash; zum Aufrufen einer Methode, die einen `long`-Wert zurückgibt.

- [JNIEnv.CallFloatMethod](xref:Android.Runtime.JNIEnv.CallFloatMethod*) &ndash; zum Aufrufen einer Methode, die einen `float`-Wert zurückgibt.

- [JNIEnv.CallDoubleMethod](xref:Android.Runtime.JNIEnv.CallDoubleMethod*) &ndash; zum Aufrufen einer Methode, die einen `double`-Wert zurückgibt.

### <a name="non-virtual-method-invocation"></a>Nicht virtueller Methodenaufruf

Die Gruppe von Methoden zum nicht virtuellen Aufrufen von Methoden weist folgendes Namensmuster auf:

```csharp
* JNIEnv.CallNonvirtual*Method( IntPtr instance, IntPtr class, IntPtr methodID, params JValue[] args );
```

Dabei ist `*` der Rückgabetyp der Methode. Ein nicht virtueller Methodenaufruf wird in der Regel verwendet, um die Basismethode einer virtuellen Methode aufzurufen.

- [JNIEnv.CallNonvirtualObjectMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualObjectMethod*) &ndash; zum nicht virtuellen Aufrufen einer Methode, die einen nicht integrierten Typ zurückgibt, z. B. `java.lang.Object`, Arrays und Schnittstellen. Der zurückgegebene Wert ist ein lokaler JNI-Verweis.

- [JNIEnv.CallNonvirtualBooleanMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualBooleanMethod*) &ndash; zum nicht virtuellen Aufrufen einer Methode, die einen `bool`-Wert zurückgibt.

- [JNIEnv.CallNonvirtualByteMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualByteMethod*) &ndash; zum nicht virtuellen Aufrufen einer Methode, die einen `sbyte`-Wert zurückgibt.

- [JNIEnv.CallNonvirtualCharMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualCharMethod*) &ndash; zum nicht virtuellen Aufrufen einer Methode, die einen `char`-Wert zurückgibt.

- [JNIEnv.CallNonvirtualShortMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualShortMethod*) &ndash; zum nicht virtuellen Aufrufen einer Methode, die einen `short`-Wert zurückgibt.

- [JNIEnv.CallNonvirtualLongMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualLongMethod*) &ndash; zum nicht virtuellen Aufrufen einer Methode, die einen `long`-Wert zurückgibt.

- [JNIEnv.CallNonvirtualFloatMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualFloatMethod*) &ndash; zum nicht virtuellen Aufrufen einer Methode, die einen `float`-Wert zurückgibt.

- [JNIEnv.CallNonvirtualDoubleMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualDoubleMethod*) &ndash; zum nicht virtuellen Aufrufen einer Methode, die einen `double`-Wert zurückgibt.

<a name="_Static_Methods" />

## <a name="static-methods"></a>Statische Methoden

Statische Methoden werden durch *Methoden-IDs* aufgerufen. Methoden-IDs werden mit [JNIEnv.GetStaticMethodID](xref:Android.Runtime.JNIEnv.GetStaticMethodID*) abgerufen. Dafür sind der Typ, in dem die Methode Feld definiert ist, der Name der Methode und die [JNI-Typsignatur](#JNI_Type_Signatures) der Methode erforderlich.

Methoden-IDs müssen nicht freigegeben werden und sind gültig, solange der entsprechende Java-Typ geladen ist. (Android unterstützt derzeit das Entladen von Klassen nicht.)

### <a name="static-method-invocation"></a>Statischer Methodenaufruf

Die Gruppe von Methoden zum virtuellen Aufrufen von Methoden weist folgendes Namensmuster auf:

```csharp
* JNIEnv.CallStatic*Method( IntPtr class, IntPtr methodID, params JValue[] args );
```

Dabei ist `*` der Rückgabetyp der Methode.

- [JNIEnv.CallStaticObjectMethod](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*) &ndash; zum Aufrufen einer statischen Methode, die einen nicht integrierten Typ zurückgibt, z. B. `java.lang.Object`, Arrays und Schnittstellen. Der zurückgegebene Wert ist ein lokaler JNI-Verweis.

- [JNIEnv.CallStaticBooleanMethod](xref:Android.Runtime.JNIEnv.CallStaticBooleanMethod*) &ndash; zum Aufrufen einer statischen Methode, die einen `bool`-Wert zurückgibt.

- [JNIEnv.CallStaticByteMethod](xref:Android.Runtime.JNIEnv.CallStaticByteMethod*) &ndash; zum Aufrufen einer statischen Methode, die einen `sbyte`-Wert zurückgibt.

- [JNIEnv.CallStaticCharMethod](xref:Android.Runtime.JNIEnv.CallStaticCharMethod*) &ndash; zum Aufrufen einer statischen Methode, die einen `char`-Wert zurückgibt.

- [JNIEnv.CallStaticShortMethod](xref:Android.Runtime.JNIEnv.CallStaticShortMethod*) &ndash; zum Aufrufen einer statischen Methode, die einen `short`-Wert zurückgibt.

- [JNIEnv.CallStaticLongMethod](xref:Android.Runtime.JNIEnv.CallLongMethod*) &ndash; zum Aufrufen einer statischen Methode, die einen `long`-Wert zurückgibt.

- [JNIEnv.CallStaticFloatMethod](xref:Android.Runtime.JNIEnv.CallStaticFloatMethod*) &ndash; zum Aufrufen einer statischen Methode, die einen `float`-Wert zurückgibt.

- [JNIEnv.CallStaticDoubleMethod](xref:Android.Runtime.JNIEnv.CallStaticDoubleMethod*) &ndash; zum Aufrufen einer statischen Methode, die einen `double`-Wert zurückgibt.

<a name="JNI_Type_Signatures" />

## <a name="jni-type-signatures"></a>JNI-Typsignaturen

[JNI-Typsignaturen](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/types.html#wp16432) sind [JNI-Typverweise](#_JNI_Type_References) (allerdings keine vereinfachten Typverweise), außer für Methoden. Bei Methoden sieht die JNI-Typsignatur so aus: eine öffnende Klammer `'('`, gefolgt von den Typverweisen für alle Parametertypen, direkt hintereinander geschrieben (ohne Kommas oder etwas anderes dazwischen), gefolgt von einer schließenden Klammer `')'`, gefolgt vom JNI-Typverweis des Rückgabetyps der Methode.

Betrachten Sie beispielsweise diese Java-Methode:

```java
long f(int n, String s, int[] array);
```

Die JNI-Typsignatur sieht wie folgt aus:

```csharp
(ILjava/lang/String;[I)J
```

Im Allgemeinen wird *dringend* empfohlen, zum Bestimmen von JNI-Signaturen den Befehl `javap` zu verwenden. Ein Beispiel: Die JNI-Typsignatur der Methode [java.lang.Thread.State.valueOf(String)](https://developer.android.com/reference/java/lang/Thread.State.html#valueOf(java.lang.String)) lautet „(Ljava/lang/String;)Ljava/lang/Thread$State;“, während die JNI-Typsignatur der [java.lang.Thread.State.values](https://developer.android.com/reference/java/lang/Thread.State.html#values)-Methode „()[Ljava/lang/Thread$State;“ lautet. Beachten Sie die abschließenden Semikolons, sie *sind Teil* der JNI-Typsignatur.

<a name="_JNI_Type_References" />

## <a name="jni-type-references"></a>JNI-Typverweise

JNI-Typverweise unterscheiden sich von Java-Typverweisen. Sie können mit der JNI keine vollqualifizierten Java-Typnamen wie `java.lang.String` verwenden. Stattdessen müssen Sie je nach Kontext die JNI-Varianten `"java/lang/String"` oder `"Ljava/lang/String;"` verwenden. Weitere Informationen finden Sie im Folgenden.
Es gibt vier Arten von JNI-Typverweisen:

- **Integriert**
- **Vereinfacht**
- **Typ**
- **array**

### <a name="built-in-type-references"></a>Integrierte Typverweise

Integrierte Typverweise bestehen aus einem einzigen Zeichen und werden zum Verweisen auf integrierte Werttypen verwendet. Die Zuordnung lautet wie folgt:

- `"B"` für `sbyte`.
- `"S"` für `short`.
- `"I"` für `int`.
- `"J"` für `long`.
- `"F"` für `float`.
- `"D"` für `double`.
- `"C"` für `char`.
- `"Z"` für `bool`.
- `"V"` für `void`-Rückgabetypen von Methoden.

<a name="_Simplified_Type_References_1" />

### <a name="simplified-type-references"></a>Vereinfachte Typverweise

Vereinfachte Typverweise können nur in [JNIEnv.FindClass(string)](xref:Android.Runtime.JNIEnv.FindClass*)) verwendet werden.
Es gibt zwei Möglichkeiten, einen vereinfachten Typverweis abzuleiten:

1. Aus einem vollqualifizierten Java-Namen. Ersetzen Sie jeden `'.'` im Paketnamen und vor dem Typnamen durch `'/'` und jeden `'.'` in einem Typnamen durch `'$'`.

1. Lesen der Ausgabe von `'unzip -l android.jar | grep JavaName'`.

Jede der beiden Möglichkeiten führt dazu, dass der Java-Typ [java.lang.Thread.State](https://developer.android.com/reference/java/lang/Thread.State.html) dem vereinfachten Typverweis `java/lang/Thread$State` zugeordnet wird.

### <a name="type-references"></a>Typverweise

Ein Typverweis ist ein integrierter Typverweis oder ein vereinfachter Typverweis mit dem Präfix `'L'` und dem Suffix `';'`. Beim Java-Typ [java.lang.String](https://developer.android.com/reference/java/lang/String.html) lautet der vereinfachte Typverweis `"java/lang/String"`, während der Typverweis `"Ljava/lang/String;"` lautet.

Typverweise werden mit Arraytypverweisen und JNI-Signaturen verwendet.

Eine weitere Möglichkeit zum Abrufen eines Typverweises ist das Lesen der Ausgabe von `'javap -s -classpath android.jar fully.qualified.Java.Name'`.
Je nachdem, welcher Typ beteiligt ist, können Sie eine Konstruktordeklaration oder einen Methodenrückgabetyp verwenden, um den JNI-Namen zu bestimmen. Zum Beispiel:

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

`Thread.State` ist ein Java-Enumerationstyp, daher können wir die Signatur der `valueOf`-Methode verwenden, um zu bestimmen, dass der Typverweis „Ljava/lang/Thread$State;“ lautet.

### <a name="array-type-references"></a>Arraytypverweise

Arraytypverweise sind durch das Zeichen `'['` gekennzeichnet, das einem JNI-Typverweis vorangestellt ist.
Vereinfachte Typverweise können beim Angeben von Arrays nicht verwendet werden.

`int[]` ist beispielsweise `"[I"`, `int[][]` ist `"[[I"` und `java.lang.Object[]` ist `"[Ljava/lang/Object;"`.

## <a name="java-generics-and-type-erasure"></a>Java-Generics und Typlöschung

In den *meisten* Fällen sind Java-Generics aus Sicht der JNI *nicht vorhanden*.
Es gibt zwar einige „Tricks“, aber diese beziehen sich darauf, wie Java mit Generics interagiert, nicht darauf, wie die JNI generische Member sucht und aufruft.

Bei der Interaktion über die JNI gibt es keinen Unterschied zwischen einem generischen Typ oder Member und einem nicht generischen Typ oder Member. Der generische Typ [java.lang.Class&lt;T&gt;](https://developer.android.com/reference/java/lang/Class.html) beispielsweise ist auch der „unformatierte“ generische Typ `java.lang.Class`. Beide Typen weisen denselben vereinfachten Typverweis `"java/lang/Class"` auf.

## <a name="java-native-interface-support"></a>Unterstützung für die Java Native Interface

[Android.Runtime.JNIEnv](xref:Android.Runtime.JNIEnv) ist ein Managed Wrapper für die Java Native Interface (JNI). JNI-Funktionen sind in der [Java Native Interface Specification](https://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html) deklariert, obwohl die Methoden geändert wurden: Der explizite `JNIEnv*`-Parameter wurde entfernt, und `IntPtr` wird statt `jobject`, `jclass`, `jmethodID` usw. verwendet. Sehen Sie sich z. B. die JNI-Funktion [NewObject](https://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp4517) an:

```csharp
jobject NewObjectA(JNIEnv *env, jclass clazz, jmethodID methodID, jvalue *args);
```

Diese wird als [JNIEnv.NewObject](xref:Android.Runtime.JNIEnv.NewObject*)-Methode verfügbar gemacht:

```csharp
public static IntPtr NewObject(IntPtr clazz, IntPtr jmethod, params JValue[] parms);
```

Die Übersetzung zwischen den beiden Aufrufen ist einigermaßen unkompliziert. In C sieht das so aus:

```c
jobject CreateMapActivity(JNIEnv *env)
{
    jclass    Map_Class   = (*env)->FindClass(env, "mono/samples/googlemaps/MyMapActivity");
    jmethodID Map_defCtor = (*env)->GetMethodID (env, Map_Class, "<init>", "()V");
    jobject   instance    = (*env)->NewObject (env, Map_Class, Map_defCtor);

    return instance;
}
```

Die Entsprechung in C# sieht so aus:

```csharp
IntPtr CreateMapActivity()
{
    IntPtr Map_Class   = JNIEnv.FindClass ("mono/samples/googlemaps/MyMapActivity");
    IntPtr Map_defCtor = JNIEnv.GetMethodID (Map_Class, "<init>", "()V");
    IntPtr instance    = JNIEnv.NewObject (Map_Class, Map_defCtor);

    return instance;
}
```

Sobald Sie über eine Java-Objektinstanz in einem IntPtr-Element verfügen, möchten Sie diese vermutlich nutzen. Sie können JNIEnv-Methoden wie [JNIEnv.CallVoidMethod()](xref:Android.Runtime.JNIEnv.CallVoidMethod*) verwenden, aber wenn es bereits einen entsprechenden C#-Wrapper gibt, sollten Sie einen Wrapper um den JNI-Verweis erstellen. Dafür können Sie die Erweiterungsmethode [Extensions.JavaCast\<T>](xref:Android.Runtime.Extensions.JavaCast*) verwenden:

```csharp
IntPtr lrefActivity = CreateMapActivity();

// imagine that Activity were instead an interface or abstract type...
Activity mapActivity = new Java.Lang.Object(lrefActivity, JniHandleOwnership.TransferLocalRef)
    .JavaCast<Activity>();
```

Sie können auch die Methode [Java.Lang.Object.GetObject\<T>](xref:Java.Lang.Object.GetObject*) verwenden:

```csharp
IntPtr lrefActivity = CreateMapActivity();

// imagine that Activity were instead an interface or abstract type...
Activity mapActivity = Java.Lang.Object.GetObject<Activity>(lrefActivity, JniHandleOwnership.TransferLocalRef);
```

Darüber hinaus wurden alle JNI-Funktionen geändert, indem der `JNIEnv*`-Parameter entfernt wurde, der in jeder JNI-Funktion vorhanden ist.

## <a name="summary"></a>Zusammenfassung

Die direkte Arbeit mit der JNI ist ausgesprochen kompliziert und sollte um jeden Preis vermieden werden. Leider ist dies nicht immer möglich – daher hoffen wir, dass der vorliegende Leitfaden hilfreich ist, wenn Sie bei Mono für Android auf nicht gebundene Java-Elemente stoßen.

## <a name="related-links"></a>Verwandte Links

- [Java Native Interface Specification](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/jniTOC.html)
- [Java Native Interface Functions](https://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html)
