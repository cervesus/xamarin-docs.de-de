---
title: Arbeiten mit JNI
description: "Xamarin.Android ermöglicht das Schreiben von Android-apps in c# anstelle von Java. Mehrere Assemblys werden mit Xamarin.Android bereitgestellt, die Bindungen für Java-Clientbibliotheken, einschließlich Mono.Android.dll und Mono.Android.GoogleMaps.dll. Allerdings Bindungen werden nicht für alle möglichen Java-Bibliothek bereitgestellt und die Bindungen, die bereitgestellt werden können nicht gebunden werden, jedem Java-Typen und Member. Um ungebundenen Java-Typen und Member verwenden, kann die Java Native Interface (JNI) verwendet werden. Dieser Artikel zeigt, wie JNI mit Java-Typen und Member von Xamarin.Android Anwendungen interagieren."
ms.topic: article
ms.prod: xamarin
ms.assetid: A417DEE9-7B7B-4E35-A79C-284739E3838E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: f14d456cba66142c51e0755cdfd3c6795bd1cf73
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2018
---
# <a name="working-with-jni"></a>Arbeiten mit JNI

_Xamarin.Android ermöglicht das Schreiben von Android-apps in c# anstelle von Java. Mehrere Assemblys werden mit Xamarin.Android bereitgestellt, die Bindungen für Java-Clientbibliotheken, einschließlich Mono.Android.dll und Mono.Android.GoogleMaps.dll. Allerdings Bindungen werden nicht für alle möglichen Java-Bibliothek bereitgestellt und die Bindungen, die bereitgestellt werden können nicht gebunden werden, jedem Java-Typen und Member. Um ungebundenen Java-Typen und Member verwenden, kann die Java Native Interface (JNI) verwendet werden. Dieser Artikel zeigt, wie JNI mit Java-Typen und Member von Xamarin.Android Anwendungen interagieren._


## <a name="overview"></a>Übersicht

Es ist nicht immer möglich, eine verwaltete Callable Wrapper (MCW) zum Aufrufen von Java-Code zu erstellen oder erforderlich sind. In vielen Fällen ist "Inline" JNI vollkommen akzeptabel und nützlich für die einmalige Verwendung von ungebundenen Java-Elementen. Es ist häufig einfacher, JNI zu verwenden, um eine einzelne Methode für eine Javaklasse als zum Generieren einer gesamten JAR-Bindung aufzurufen.

Xamarin.Android bietet die `Mono.Android.dll` -Assembly, die eine Bindung für Android bietet `android.jar` Bibliothek. Typen und Member nicht vorhanden in `Mono.Android.dll` und Typen in `android.jar` können durch manuelles Bindung verwendet werden. Um Java-Typen und Member zu binden, verwenden Sie die **Java Native Interface** (**JNI**) Typen zu suchen, lesen und schreiben die Felder und Methoden aufrufen.

Die JNI-API in Xamarin.Android konzeptionell sehr ähnlich ist die `System.Reflection` -API in .NET: Er ermöglicht es Ihnen, suchen Sie die Typen und Member anhand des Namens, Lese- / Feldwerte, rufen Methoden und vieles mehr. JNI können und die `Android.Runtime.RegisterAttribute` benutzerdefinierten Attributs zur Verwendung virtuelle Methoden zu deklarieren, die zur Unterstützung überschreiben gebunden werden kann. Sie können Schnittstellen binden, damit sie in c# implementiert werden können.

Dieses Dokument erläutert:

-  Typen wie JNI bezieht.
-  Zum Suchen, lesen und schreiben.
-  Informationen zum Suchen und Aufrufen von Methoden.
-  Vorgehensweise zum Überschreiben von verwaltetem Code ermöglichen virtuelle Methoden verfügbar zu machen.
-  Wie Schnittstellen verfügbar gemacht wird.



## <a name="requirements"></a>Anforderungen

JNI, bis zur Verfügung gestellt der [Android.Runtime.JNIEnv Namespace](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/), ist in jeder Version von Xamarin.Android verfügbar.
Um Java-Typen und Schnittstellen zu binden, müssen Sie Xamarin.Android 4.0 oder höher verwenden.


## <a name="managed-callable-wrappers"></a>Verwaltete Callable Wrapper

Ein **Callable Wrapper verwaltet** (**MCW**) ist eine *Bindung* für eine Java-Klasse oder Schnittstelle, die von der alle JNI Maschinen umschließt, damit C#-Clientcode nicht kümmern muss die zugrunde liegende Komplexität der JNI. Die meisten `Mono.Android.dll` verwalteten Aufrufwrappern besteht.

Verwaltete Aufrufwrappern dienen zwei Zwecken:

1.  Kapseln Sie JNI verwenden, sodass Clientcode nicht über die zugrunde liegenden Komplexität wissen muss.
1.  Ermöglichen sie untergeordnete Klasse Java-Typen und Java-Schnittstellen implementieren.

Erste dient ausschließlich zur Vereinfachung und Kapselung Komplexität so, dass Consumer einen einfachen, verwalteten Satz von Klassen zu verwenden. Dies erfordert die Verwendung verschiedener [JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/) Elemente wie weiter unten in diesem Artikel beschrieben. Denken Sie daran, die Aufrufwrappern verwaltet werden, nicht unbedingt erforderlich &ndash; "Inline" JNI verwenden vollkommen akzeptabel ist und eignet sich für die einmalige Verwendung von ungebundenen Java-Elementen. Implementierung von untergeordneten classing und Schnittstelle erfordert die Verwendung der verwalteten callable Wrapper.



## <a name="android-callable-wrappers"></a>Android Callable Wrapper

Android callable Wrapper (Inhaltsfehler) sind erforderlich, wenn die Android-Laufzeit (Grafik) benötigt, um verwalteten Code aufrufen. Diese Wrapper sind erforderlich, da es keine Möglichkeit gibt, um Klassen mit Grafiken zur Laufzeit zu registrieren.
(Insbesondere die [DefineClass](http://docs.oracle.com/javase/6/docs/technotes/guides/jni/spec/functions.html#wp15986) JNI-Funktion wird von der Android-Laufzeit nicht unterstützt. Android Aufrufwrappern bilden daher für den Mangel an Unterstützung für die Registrierung von Common Language Runtime Typen.)

Wenn Android Code muss, führen Sie einen virtuellen oder Methode, überschreiben oder in verwaltetem Code implementiert ist, müssen Xamarin.Android, einen Java-Proxy, damit diese Methode in den entsprechenden verwalteten Typ verteilt ruft bereitstellen. Diese Java-Proxy-Typen sind Java-Code, der die "dieselbe" Basisklasse und Java-Schnittstellenliste aufweisen wie der verwaltete Typ, implementieren die gleichen Konstruktoren und jede außer Kraft gesetzte Basisklasse und Methoden der Schnittstelle deklariert.

Android callable Wrapper werden generiert, indem die **monodroid.exe** programmieren, während die [Buildprozess](~/android/deploy-test/building-apps/build-process.md), und werden für alle Typen, die (direkt oder indirekt) erben generiert [ Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/).



### <a name="implementing-interfaces"></a>Implementieren von Schnittsellen

Vorkommen, dass Sie eine Android Schnittstelle implementieren müssen möglicherweise, vorhanden sind (z. B. [Android.Content.IComponentCallbacks](https://developer.xamarin.com/api/type/Android.Content.IComponentCallbacks/)).

Alle Android Klassen und Schnittstellen erweitern die [Android.Runtime.IJavaObject](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) Schnittstelle; aus diesem Grund müssen alle Android Typen implementieren `IJavaObject`.
Xamarin.Android nutzt diese Tatsache &ndash; verwendet `IJavaObject` Android mit einem Java-Proxy (ein Android callable Wrapper) für den angegebenen verwalteten Typ angeben. Da **monodroid.exe** sucht nur nach `Java.Lang.Object` Unterklassen (diese müssen implementieren `IJavaObject`), Erstellen von Unterklassen `Java.Lang.Object` bietet uns eine Möglichkeit zum Implementieren von Schnittstellen in verwaltetem Code. Zum Beispiel:

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

*Der übrige Teil dieser Artikel enthält Details zur Implementierung können ohne Vorankündigung geändert* (und wird nur verwendet werden, da Entwickler möglicherweise interessieren, hinter den Kulissen was hier vorgestellten).

Betrachten Sie z. B. die folgende C#-Quellcode:

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

Die **mandroid.exe** Programm den folgenden Android Callable Wrapper generiert:

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

Beachten Sie, dass die Basisklasse wird beibehalten, und systemeigene Methodendeklarationen für jede Methode, die in verwaltetem Code überschrieben wird bereitgestellt.



### <a name="exportattribute-and-exportfieldattribute"></a>ExportAttribute und ExportFieldAttribute

In der Regel generiert Xamarin.Android automatisch den Java-Code, der die Inhaltsfehler ausmacht. Diese Generierung basiert auf den Klassen- und Namen, wenn eine Klasse aus einer Javaklasse abgeleitet und vorhandene Java-Methoden überschreibt. Allerdings ist die codegenerierung in einigen Szenarien nicht ausreichend, wie im folgenden erläutert:

-   Android unterstützt Aktionsnamen im Layout-XML-Attribute, z. B. die [Android: OnClick](https://developer.xamarin.com/api/member/Android.Views.View+IOnClickListener.OnClick/p/Android.Views.View/) XML-Attribut. Wenn es angegeben wird, versucht die vergrößerte Ansicht Instanz zum Nachschlagen der Java-Methode.

-   Die [java.io.Serializable](http://developer.android.com/reference/java/io/Serializable.html) Schnittstelle erfordert `readObject` und `writeObject` Methoden. Da es keine Member dieser Schnittstelle sind, macht unsere entsprechende verwaltete Implementierung dieser Methoden zum Java-Code nicht verfügbar.

-   Die [android.os.Parcelable](https://developer.xamarin.com/api/type/Android.Os.Parcelable/) Schnittstelle erwartet, dass eine Implementierungsklasse ein statisches Felds benötigen `CREATOR` vom Typ `Parcelable.Creator`. Der generierte Java-Code erfordert einige expliziten Felds. Mit unseren standard Szenario besteht keine Möglichkeit zum Ausgabefeld in Java-Code von verwaltetem Code zur Verfügung.


Da codegenerierung eine Projektmappe, um beliebige Java-Methoden mit beliebigen Namen generieren keine bietet, beginnend mit Xamarin.Android 4.2, die [ExportAttribute](https://developer.xamarin.com/api/type/Java.Interop.ExportAttribute/) und [ExportFieldAttribute](https://developer.xamarin.com/api/type/Java.Interop.ExportFieldAttribute/) wurden eingeführt, um eine Lösung für die oben genannten Szenarien zu bieten. Beide Attribute befinden sich in der `Java.Interop` Namespace:

-   `ExportAttribute` &ndash; Gibt einen Methodennamen und die erwartete Ausnahmetypen (Geben Sie explizite "löst" im Java). Wenn sie für eine Methode verwendet wird, wird die Methode "eine Java-Methode, die eine Verteilung von Code, den entsprechenden JNI-Aufruf an die verwaltete Methode generiert exportieren". Dies kann verwendet werden, mit `android:onClick` und `java.io.Serializable`.

-   `ExportFieldAttribute` &ndash; Gibt einen Feldnamen. Es befindet sich auf eine Methode, die als ein Feldinitialisierer funktioniert. Dies kann verwendet werden, mit `android.os.Parcelable`.

Die [ExportAttribute](https://developer.xamarin.com/samples/monodroid/ExportAttribute/) -Beispielprojekt wird veranschaulicht, wie diese Attribute nicht verwenden.


#### <a name="troubleshooting-exportattribute-and-exportfieldattribute"></a>Problembehandlung bei ExportAttribute und ExportFieldAttribute

-   Verpacken ein Fehler auftritt, konnte aufgrund eines fehlenden **Mono.Android.Export.dll** &ndash; bei Verwendung `ExportAttribute` oder `ExportFieldAttribute` auf einige Methoden im Code oder der abhängigen Bibliotheken an, Sie müssen hinzufügen  **Mono.Android.Export.dll**. Diese Assembly ist nur im Zusammenhang mit Rückrufcode aus Java zu unterstützen. Es wird getrennt von **Mono.Android.dll** während zusätzliche Größe an die Anwendung hinzugefügt.

-   In Releasebuild `MissingMethodException` tritt für Exportmethoden &ndash; im Releasebuild `MissingMethodException` für Exportmethoden auftritt. (Dieses Problem wird in der neuesten Version des Xamarin.Android behoben.)



### <a name="exportparameterattribute"></a>ExportParameterAttribute

`ExportAttribute` und `ExportFieldAttribute` bieten Funktionen, Java-Laufzeit-Code verwenden kann. Dieser Code zur Laufzeit zugreift verwalteten Code über die generierten JNI-Methoden, die von dieser Attribute gesteuert. Es gibt daher keine vorhandenen Java-Methode, die die verwaltete Methode bindet; Daher ist die Java-Methode aus einer verwalteten Methodensignatur generiert.

Dieser Fall ist jedoch nicht vollständig Determinante. Dies ist vor allem Kontrollvorgänge in einigen erweiterten Zuordnungen zwischen verwalteten Typen und Java-Datentypen, z. B. "true":

-  InputStream
-  OutputStream
-  XmlPullParser
-  XmlResourceParser

Wenn solche Typen für exportierte Methoden erforderlich sind die `ExportParameterAttribute` muss explizit erteilen den entsprechenden Parameter oder Rückgabewert einen Typ verwendet werden.



### <a name="annotation-attribute"></a>Anmerkung-Attribut

4.2 Xamarin.Android, Konvertierung `IAnnotation` der Implementierungstypen in Attribute (System.Attribute) sowie zusätzliche Unterstützung für die Generierung der Anmerkung in Java-Wrapper.

Dies bedeutet, dass die folgenden direktionalen Änderungen:

-   Der Generator Bindung generiert `Java.Lang.DeprecatedAttribute` aus `java.Lang.Deprecated` (während der Fall sein sollte `[Obsolete]` in verwaltetem Code).

-   Dies bedeutet nicht, dass für vorhandene `Java.Lang.Deprecated` Klasse verschwinden zu lassen. Diese Java-basierte Objekte konnte weiterhin als üblichen Java-Objekte verwendet werden (sofern eine solche Vorgehensweise vorhanden ist). Es werden `Deprecated` und `DeprecatedAttribute` Klassen.

-   Die `Java.Lang.DeprecatedAttribute` Klasse ist als markiert `[Annotation]` . Wenn es ist ein benutzerdefiniertes Attribut, die von diesem geerbt wird `[Annotation]` -Attribut, Msbuild-Aufgabe generiert eine Java-Anmerkung für dieses benutzerdefinierte Attribut (@Deprecated) in der Android Callable Wrapper (Inhaltsfehler).

-   Anmerkungen konnte generiert werden, auf Klassen, Methoden und Felder (also eine Methode in verwaltetem Code) exportiert.

Wenn die enthaltende Klasse (die kommentierte Klasse selbst oder die Klasse, die mit Anmerkungen versehenen Elemente enthält) nicht registriert ist, wird die gesamte Klassenquelle für die Java nicht überhaupt generiert z. B. Anmerkungen. Für Methoden können Sie angeben der `ExportAttribute` zum Abrufen der Methode explizit generiert und mit Anmerkungen versehen. Darüber hinaus ist es kein Feature, mit eine Klassendefinition für Java-Anmerkung "generieren". Wenn Sie ein benutzerdefiniertes verwaltetes Attribut für eine bestimmte Anmerkung definieren, müssen Sie also eine andere JAR-Bibliothek hinzufügen, die die entsprechende Klasse der Java-Anmerkung enthält. Hinzufügen einer Java-Quelldatei, die die Anmerkungstyp definiert ist nicht ausreichend. Der Java-Compiler funktioniert nicht auf die gleiche Weise wie **apt**.

Außerdem gelten die folgenden Einschränkungen:

-   Im Rahmen dieses Konvertierungsprozesses berücksichtigt nicht `@Target` bisher Anmerkung für die Anmerkungstyp.

-   Attribute auf eine Eigenschaft ist nicht möglich. Verwenden Sie stattdessen die Attribute für das Eigenschaften-Getter oder Setter-Methode.



## <a name="class-binding"></a>Klasse Bindung

Binden einer Klasse bedeutet einen aufrufbaren Wrapper zur Vereinfachung der Aufruf des zugrunde liegenden Java-Typs.

Virtuelle und abstrakte Methoden gestatten in c# überschreiben Bindung erfordert Xamarin.Android 4.0. Jedoch kann eine beliebige Version von Xamarin.Android nicht virtuelle Methoden, statische Methoden oder virtuellen Methoden binden, ohne Unterstützung der überschreibt.

Eine Bindung enthält in der Regel die folgenden Elemente:

-  Ein [JNI handle, für die Java-Datentyp gebunden wird](#_Looking_up_Java_Types).

-  [JNI Feld-IDs und die Eigenschaften für jedes gebundenen Feld](#_Instance_Fields).

-  [JNI-Methode-IDs und Methoden für jede gebundene Methode](#_Instance_Methods).

-  Wenn untergeordnete classing erforderlich ist, muss der Typ haben eine [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/) benutzerdefiniertes Attribut in der Typdeklaration mit [RegisterAttribute.DoNotGenerateAcw](https://developer.xamarin.com/api/property/Android.Runtime.RegisterAttribute.DoNotGenerateAcw/) festgelegt `true`.



### <a name="declaring-type-handle"></a>Deklarieren von Typhandle

Die Feld- und Lookup-Methoden erfordern einen Objektverweis auf ihren deklarierenden Typ verweisen. Gemäß der Konvention wird dies aufrechterhalten einer `class_ref` Feld:

```csharp
static IntPtr class_ref = JNIEnv.FindClass(CLASS);
```

Finden Sie unter der [JNI Typverweise](#_JNI_Type_References) Abschnitt Weitere Informationen zu den `CLASS` token.


### <a name="binding-fields"></a>Binden von Feldern

Java-Felder werden verfügbar gemacht, als C#-Eigenschaften, z. B. das Java-Feld [java.lang.System.in](http://developer.android.com/reference/java/lang/System.html#in) als c#-Eigenschaft gebunden ist [Java.Lang.JavaSystem.In](https://developer.xamarin.com/api/property/Java.Lang.JavaSystem.In/).
Darüber hinaus seit JNI zwischen statische Felder und Instanzfelder unterscheidet, unterschiedliche Methoden verwendet werden, wenn die Eigenschaften implementiert.

Feld Bindung umfasst drei Gruppen von Methoden an:

1.  Die *Feld-Id abrufen* Methode. Die *Feld-Id abrufen* Methode ist verantwortlich für die Rückgabe ein Felds zu behandeln, die die *abrufen Feldwert* und *Feldwert festgelegt* Methoden verwendet. Das Abrufen von Feld-Id ist erforderlich, zu wissen, das deklarieren, geben Sie den Namen des Felds, und die [JNI Typsignatur](#_JNI_Type_Signatures) des Felds.

1.  Die *abrufen Feldwert* Methoden. Diese Methoden erfordern das Handle des Felds, und es sind verantwortlich für das Lesen der Wert des Felds aus Java.
    Die zu verwendende Methode hängt vom Typ des Felds ab.

1.  Die *festgelegt Feldwert* Methoden. Diese Methoden erfordern das Feldhandle und für das Schreiben der Wert des Felds innerhalb von Java zuständig sind. Die zu verwendende Methode hängt vom Typ des Felds ab.


 [Statische Felder](#_Static_Fields) verwenden die [JNIEnv.GetStaticFieldID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticMethodID/), `JNIEnv.GetStatic*Field`, und [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/) Methoden.

 [Instanzenfelder](#_Instance_Fields) verwenden die [JNIEnv.GetFieldID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetFieldID/), `JNIEnv.Get*Field`, und [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/) Methoden.

Angenommen, die statische Eigenschaft `JavaSystem.In` als implementiert werden können:

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

Hinweis: Wir verwenden [InputStreamInvoker.FromJniHandle](https://developer.xamarin.com/api/member/Android.Runtime.InputStreamInvoker.FromJniHandle/(System.IntPtr%2cAndroid.Runtime.JniHandleOwnership)) konvertieren den JNI-Verweis in einem `System.IO.Stream` -Instanz, und wir verwenden `JniHandleOwnership.TransferLocalRef` da [JNIEnv.GetStaticObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticObjectField/) Gibt einen lokalen Verweis.

Anzahl der [Android.Runtime](https://developer.xamarin.com/api/namespace/Android.Runtime/) aufweisen `FromJniHandle` Methoden, die einer JNI konvertieren, werden in den gewünschten Typ verweisen.



### <a name="method-binding"></a>Methode-Bindung

Java-Methoden werden als C#-Methoden und C#-Eigenschaften verfügbar gemacht. Z. B. die Java-Methode ["java.lang.Runtime.runFinalizersOnExit"](http://developer.android.com/reference/java/lang/Runtime.html#runFinalizersOnExit(boolean)) Methode gebunden ist, als die ["java.lang.Runtime.runFinalizersOnExit"](https://developer.xamarin.com/api/member/Java.Lang.Runtime.RunFinalizersOnExit/) -Methode, und die [java.lang.Object.getClass ](http://developer.android.com/reference/java/lang/Object.html#getClass) Methode gebunden ist, als die ["java.lang.Object.Class"](https://developer.xamarin.com/api/property/Java.Lang.Object.Class/) Eigenschaft.

Der Methodenaufruf ist ein zweistufiger Prozess:

1.  Die *Methode-Id abrufen* für die aufzurufende Methode. Die *Methode-Id abrufen* Methode ist verantwortlich für die Rückgabe einer Methodenhandle, der die Methoden, die Methode verwendet wird. Abrufen der Id Methode muss bekannt sein, die deklarieren, geben Sie den Namen der Methode und die [JNI Typsignatur](#_JNI_Type_Signatures) der Methode.

1.  Rufen Sie die Methode auf.

Unterscheiden sich nur mit Feldern, die Methoden verwenden, rufen Sie die Methode-Id und das Aufrufen der Methodennamens zwischen statischen Methoden und Instanzenmethoden.

[Statische Methoden](#_Static_Methods_1) verwenden [JNIEnv.GetStaticMethodID()](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticMethodID/) Methode-Id zu suchen und Verwenden der `JNIEnv.CallStatic*Method` -Methodenfamilie für Aufruf.

[Instanzmethoden](#_Instance_Methods) verwenden [JNIEnv.GetMethodID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetMethodID/) Methode-Id zu suchen und Verwenden der `JNIEnv.Call*Method` und `JNIEnv.CallNonvirtual*Method` Familien Methoden zum Aufrufen.

Methode-Bindung ist möglicherweise mehr als nur-Methodenaufruf. Methode Bindung enthält eine aufzurufende Methode (für abstrakte und nicht-Final-Methoden) überschrieben werden, sodass auch oder (für Methoden der Schnittstelle) implementiert. Die [Unterstützung von Vererbung, Schnittstellen](#_Supporting_Inheritance,_Interfaces_1) Abschnitt behandelt die Komplexitäten virtuelle Methoden und die Schnittstellenmethoden zu unterstützen.

<a name="_Static_Methods_1" />

#### <a name="static-methods"></a>Statische Methoden

Binden eine statische Methode umfasst die Verwendung des `JNIEnv.GetStaticMethodID` zum Abrufen einer Methode Handles, klicken Sie dann mit dem entsprechenden `JNIEnv.CallStatic*Method` -Methode, abhängig vom Rückgabetyp der Methode. Im folgenden ist ein Beispiel für eine Bindung für die [Runtime.getRuntime](http://developer.android.com/reference/java/lang/Runtime.html#getRuntime()) Methode:

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

Beachten Sie, dass wir das Methodenhandle in einem statischen Feld Speichern `id_getRuntime`. Dies ist zur Optimierung der Leistung, sodass das Methodenhandle nicht benötigt, bei jedem Aufruf gesucht werden soll. Es ist nicht erforderlich, das Methodenhandle auf diese Weise zwischengespeichert. Nachdem die Methodenhandle abgerufen wird, [JNIEnv.CallStaticObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticObjectMethod/) wird verwendet, um die Methode aufzurufen. `JNIEnv.CallStaticObjectMethod` Gibt eine `IntPtr` enthält das Handle für die zurückgegebene Java-Instanz.
[Java.Lang.Object.GetObject&lt;T&gt;(IntPtr, JniHandleOwnership)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) wird verwendet, um das Handle Java in eine stark typisierte Objektinstanz zu konvertieren.



#### <a name="non-virtual-instance-method-binding"></a>Bindung für nicht virtuelle Instanz-Methode

Binden einer `final` Instanzmethode, eine Methode oder Instanzmethode die erfordern nicht überschreiben, müssen Sie mit dem `JNIEnv.GetMethodID` zum Abrufen einer Methode Handles, klicken Sie dann mit dem entsprechenden `JNIEnv.Call*Method` -Methode, abhängig vom Rückgabetyp der Methode. Im folgenden ist ein Beispiel für eine Bindung für die `Object.Class` Eigenschaft:

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

Beachten Sie, dass wir das Methodenhandle in einem statischen Feld Speichern `id_getClass`.
Dies ist zur Optimierung der Leistung, sodass das Methodenhandle nicht benötigt, bei jedem Aufruf gesucht werden soll. Es ist nicht erforderlich, das Methodenhandle auf diese Weise zwischengespeichert. Nachdem die Methodenhandle abgerufen wird, [JNIEnv.CallStaticObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticObjectMethod/) wird verwendet, um die Methode aufzurufen. `JNIEnv.CallStaticObjectMethod` Gibt eine `IntPtr` enthält das Handle für die zurückgegebene Java-Instanz.
[Java.Lang.Object.GetObject&lt;T&gt;(IntPtr, JniHandleOwnership)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) wird verwendet, um das Handle Java in eine stark typisierte Objektinstanz zu konvertieren.


### <a name="binding-constructors"></a>Binden von Konstruktoren

Konstruktoren sind von Java-Methoden, mit dem Namen `"<init>"`. Wie bei Instanzmethoden Java, `JNIEnv.GetMethodID` wird verwendet, um das Handle Konstruktor zu suchen. Im Gegensatz zu Java-Methoden die [JNIEnv.NewObject](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.NewObject/) Methoden werden verwendet, um das Handle der Konstruktor-Methode aufrufen. Der Rückgabewert der `JNIEnv.NewObject` ein lokaler JNI-Verweis ist:


```csharp
int value = 42;
IntPtr class_ref    = JNIEnv.FindClass ("java/lang/Integer");
IntPtr id_ctor_I    = JNIEnv.GetMethodID (class_ref, "<init>", "(I)V");
IntPtr lrefInstance = JNIEnv.NewObject (class_ref, id_ctor_I, new JValue (value));
// Dispose of lrefInstance, class_ref…
```

Normalerweise wird eine Klasse Bindung Unterklasse [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/).
Beim Erstellen von Unterklassen für `Java.Lang.Object`, wird eine zusätzliche semantische kommt es zu einem: eine `Java.Lang.Object` Instanz behält einen globalen Verweis auf eine Java-Instanz über die `Java.Lang.Object.Handle` Eigenschaft.

1.  Die `Java.Lang.Object` Standardkonstruktor wird eine Java-Instanz zuweisen.

1.  Wenn der Typ verfügt über eine `RegisterAttribute` , und `RegisterAttribute.DoNotGenerateAcw` ist `true` , klicken Sie dann eine Instanz von der `RegisterAttribute.Name` Typ wird durch seinen Standardkonstruktor erstellt.

1.  Andernfalls die [Android Callable Wrapper](~/android/platform/java-integration/android-callable-wrappers.md) (Inhaltsfehler), entspricht `this.GetType` über seinen Standardkonstruktor instanziiert wird. Android Callable Wrapper werden generiert, bei der paketerstellung für jede `Java.Lang.Object` Unterklasse für den `RegisterAttribute.DoNotGenerateAcw` nicht festgelegt ist, um `true`.

Für Typen sind keine Bindungen Klasse, ist dies der erwarteten semantische: Instanziieren einer `Mono.Samples.HelloWorld.HelloAndroid` erstellen Sie C#-Instanz sollte eine Java `mono.samples.helloworld.HelloAndroid` -Instanz, die einen generierten Android Callable Wrapper ist.

Für Klasse Bindungen möglicherweise dies das richtige Verhalten, wenn der Java-Typ einen standardmäßigen Konstruktor enthält und/oder keine anderen Konstruktor muss aufgerufen werden. Andernfalls muss die folgenden Aktionen durchführt ein Konstruktor bereitgestellt werden:

1.  Aufrufen der [Java.Lang.Object (IntPtr, JniHandleOwnership)](https://developer.xamarin.com/api/constructor/Java.Lang.Object.Object/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) anstelle des standardmäßigen `Java.Lang.Object` Konstruktor. Dies ist erforderlich, um zu vermeiden, Erstellen einer neuen Java-Instanz.

1.  Überprüfen Sie den Wert der [Java.Lang.Object.Handle](https://developer.xamarin.com/api/property/Java.Lang.Object.Handle/) vor dem Erstellen von jeder Java-Instanzen. Die `Object.Handle` Eigenschaft weist einen Wert außer `IntPtr.Zero` ein Android Callable Wrapper, die in Java-Code erstellt wurde, und die Bindung für die Klasse enthält die erstellte Android Callable Wrapper-Instanz erstellt wird. Erstellt z. B. wenn Android ein `mono.samples.helloworld.HelloAndroid` Instanz, die den Android Callable Wrapper erstellt werden, erste und der Programmiersprache Java `HelloAndroid` Konstruktor erstellt eine Instanz des entsprechenden `Mono.Samples.HelloWorld.HelloAndroid` Typ, mit der `Object.Handle` Eigenschaft mit der Java-Instanz vor der Ausführung der Konstruktor festgelegt wird.

1.  Ist der aktuelle Laufzeittyp nicht identisch mit den deklarierenden Typ, wird eine Instanz des entsprechenden Android Callable Wrappers muss erstellt werden, und verwenden [Object.SetHandle](https://developer.xamarin.com/api/member/Java.Lang.Object.SetHandle/(System.IntPtr%2cAndroid.Runtime.JniHandleOwnership)) zum Speichern von zurückgegebenen Handles [ JNIEnv.CreateInstance](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CreateInstance/).

1.  Wenn der aktuelle Laufzeittyp der deklarierende Typ identisch ist, klicken Sie dann den Java-Konstruktor aufrufen und verwenden [Object.SetHandle](https://developer.xamarin.com/api/member/Java.Lang.Object.SetHandle/(System.IntPtr%2cAndroid.Runtime.JniHandleOwnership)) zum Speichern von zurückgegebenen Handles `JNIEnv.NewInstance` .


Betrachten Sie beispielsweise die [java.lang.Integer(int)](http://developer.android.com/reference/java/lang/Integer.html#Integer(int)) Konstruktor. Dies wird als gebunden:

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

Die [JNIEnv.CreateInstance](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CreateInstance/) Methoden sind Hilfsprogramme zum Ausführen einer `JNIEnv.FindClass`, `JNIEnv.GetMethodID`, `JNIEnv.NewObject`, und `JNIEnv.DeleteGlobalReference` auf den Rückgabewert aus `JNIEnv.FindClass`. Nähere Informationen finden Sie im nächsten Abschnitt.

<a name="_Supporting_Inheritance,_Interfaces_1" />

### <a name="supporting-inheritance-interfaces"></a>Unterstützung von Vererbung, Schnittstellen

Erstellen von Unterklassen für eine Java-Typ oder eine Java-Schnittstelle implementiert, erfordert die Generierung [Android Aufrufwrappern](~/android/platform/java-integration/android-callable-wrappers.md) (ACWs) erzeugte für jede `Java.Lang.Object` Unterklasse während des Verpackungsprozesses. Inhaltsfehler Generation wird gesteuert durch die [Android.Runtime.RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/) benutzerdefinierten Attributs.

Für C#-Typen die `[Register]` benutzerdefinierte Attributkonstruktor erfordert ein Argument: der [JNI vereinfacht Typverweis](#_Simplified_Type_References_1) Geben Sie für die entsprechende Java. Dies ermöglicht unterschiedliche Namen zwischen Java und c# bereitstellt.

Vor 4.0 Xamarin.Android die `[Register]` benutzerdefinierte Attribut "alias" vorhandenen Java-Typen nicht verfügbar war. Dies ist, da der Inhaltsfehler Generierungsprozess, ACWs für generieren würden jede `Java.Lang.Object` Unterklasse gefunden.

Xamarin.Android 4.0 eingeführt der [RegisterAttribute.DoNotGenerateAcw](https://developer.xamarin.com/api/property/Android.Runtime.RegisterAttribute.DoNotGenerateAcw/) Eigenschaft. Diese Eigenschaft weist den Inhaltsfehler Generierungsprozess *überspringen* die kommentierte-Typ angegeben, wodurch die Deklaration von neuen verwalteten Callable Wrapper, die nicht ACWs generiert wird, zum Zeitpunkt der Erstellung Paket führt. Dadurch können vorhandene Bindung Java-Typen. Betrachten Sie beispielsweise die folgende einfache Java-Klasse `Adder`, die eine Methode enthält `add`, fügt zu einer ganzen Zahl und gibt das Ergebnis zurück:

```java
package mono.android.test;
public class Adder {
    public int add (int a, int b) {
        return a + b;
    }
}
```

Die `Adder` Typ als gebunden werden kann:

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

Hier wird die `Adder` C#-Typ *Aliase* der `Adder` Java-Typen. Die `[Register]` Attribut wird verwendet, um den JNI-Namen des angeben der `mono.android.test.Adder` Java-Datentyp und die `DoNotGenerateAcw` Eigenschaft wird verwendet, um dadurch die Inhaltsfehler Generation. Dies führt zu einer Inhaltsfehler für die Generierung der `ManagedAdder` eingeben, die ordnungsgemäß Unterklassen der `mono.android.test.Adder` Typ. Wenn die `RegisterAttribute.DoNotGenerateAcw` hadn't Eigenschaft verwendet wurde, und klicken Sie dann des Buildprozesses Xamarin.Android generiert einen neuen `mono.android.test.Adder` Java-Typen. Dies würde Kompilierungsfehler enthält, als die `mono.android.test.Adder` Typ würde zweimal in zwei separaten Dateien vorhanden sein.



### <a name="binding-virtual-methods"></a>Binden von virtuellen Methoden

`ManagedAdder` Unterklassen der Java `Adder` aufweisen, aber es ist nicht besonders interessant: C#- `Adder` Typ keine virtuellen Methoden definieren, also `ManagedAdder` nichts kann nicht überschrieben werden.

Binden von `virtual` Methoden zum Zulassen von Unterklassen überschreiben erfordert einige Punkte, die ausgeführt werden, die in den folgenden beiden Kategorien fallen:

1.  **Methode-Bindung**

1.  **Methode-Registrierung**


#### <a name="method-binding"></a>Methode-Bindung

Eine Methode-Bindung erfordert das Hinzufügen von zwei Support-Elementen, auf die C#- `Adder` Definition: `ThresholdType`, und `ThresholdClass`.

##### <a name="thresholdtype"></a>ThresholdType

Die `ThresholdType` Eigenschaft gibt den aktuellen Typ der Bindung zurück:

```csharp
partial class Adder {
    protected override System.Type ThresholdType {
        get {
            return typeof (Adder);
        }
    }
}
```

`ThresholdType` wird in der Methode binden verwendet, um zu ermitteln, wann es im Vergleich zu nicht virtuelle Methoden zu verteilen virtueller durchführen soll. Es sollten stets eine `System.Type` -Instanz, die die deklarierende C#-Typ entspricht.

##### <a name="thresholdclass"></a>ThresholdClass

Die `ThresholdClass` Eigenschaft gibt die JNI-Klassenreferenz für den Typ gebunden:

```csharp
partial class Adder {
    protected override IntPtr ThresholdClass {
        get {
            return class_ref;
        }
    }
}
```

`ThresholdClass` wird in der Methode Bindung verwendet werden, nicht virtuelle Methoden aufrufen.

#### <a name="binding-implementation"></a>Implementierung der Bindung

Die Bindung methodenimplementierung ist verantwortlich für Common Language Runtime-Aufruf der Java-Methode. Es enthält auch eine `[Register]` benutzerdefinierte Attributdeklaration, die Teil der Methode Registrierung und wird im Abschnitt Methode Registrierung erläutert werden:

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

Die `id_add` Feld enthält die Methode-ID für die Java-Methode aufrufen. Die `id_add` Wert stammt von `JNIEnv.GetMethodID`, wofür deklarierende Klasse (`class_ref`), den Namen der Java-Methode (`"add"`), und die JNI-Signatur der Methode (`"(II)I"`).

Nachdem die Methode-ID abgerufen wird, `GetType` im Vergleich zu `ThresholdType` zu bestimmen, ob virtuell oder nicht virtuell Verteilung erforderlich ist. Virtueller Dispatch ist erforderlich, wenn `GetType` entspricht `ThresholdType`, als `Handle` bezieht sich möglicherweise auf eine Java-belegt Unterklasse der die Methode außer Kraft gesetzt.

Wenn `GetType` passt nicht `ThresholdType`, `Adder` wurde als Unterklasse wurde (z. B. durch `ManagedAdder`), und die `Adder.Add` Implementierung wird nur aufgerufen werden, wenn die Unterklasse aufgerufen `base.Add`. Dies ist der Fall nicht virtuell Dispatch, also Where `ThresholdClass` eingeht. `ThresholdClass` Gibt an, welche Java-Klasse die Implementierung der aufzurufenden Methode bereitstellt.



#### <a name="method-registration"></a>Methode-Registrierung

Angenommen, wir haben ein aktualisiertes `ManagedAdder` Definition die überschreibt die `Adder.Add` Methode:

```csharp
partial class ManagedAdder : Adder {
    public override int Add (int a, int b) {
        return (a*2) + (b*2);
    }
}
```

Bedenken Sie, dass `Adder.Add` hatte ein `[Register]` benutzerdefiniertes Attribut:

```csharp
[Register ("add", "(II)I", "GetAddHandler")]
```

Die `[Register]` benutzerdefinierte Attributkonstruktor lässt drei Werte:

1.  Der Name der Methode Java `"add"` in diesem Fall.

1.  Die JNI-Typ-Signatur der Methode, `"(II)I"` in diesem Fall.

1.  Die *Connector Methode* , `GetAddHandler` in diesem Fall.
    Connector-Methoden werden weiter unten erläutert.


Die ersten beiden Parameter ermöglichen den Inhaltsfehler Generierungsprozess generieren eine Methodendeklaration, um die Methode zu überschreiben. Der resultierende Inhaltsfehler würde Teil des folgenden Codes enthalten:

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

Beachten Sie, dass ein `@Override` Methode deklariert, die delegiert an eine `n_`-Methode mit dem gleichen Namen vorangestellt. Dies sicher, dass bei der Java-Code ruft `ManagedAdder.add`, `ManagedAdder.n_add` aufgerufen wird, wird der überschreibenden c# ermöglichen `ManagedAdder.Add` ausgeführt wird.

Daher die wichtigste Frage: wie ist `ManagedAdder.n_add` bis zu verknüpft `ManagedAdder.Add`?

Java `native` Methoden sind bei der Java (Android Runtime)-Laufzeit über registriert die [RegisterNatives JNI-Funktion](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp17734).
`RegisterNatives` wird ein Array von Strukturen, die Namen der Java-Methode, der Typsignatur JNI und einen Funktionszeiger aufrufen, das folgt [JNI Aufrufkonvention](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/design.html#wp715).
Der Funktionszeiger muss eine Funktion sein, die zwei Zeigerargumente gefolgt von der Methodenparameter akzeptiert. Die Java `ManagedAdder.n_add` Methode muss über eine Funktion, die den folgenden C-Prototyp besitzt implementiert werden:

```csharp
int FunctionName(JNIEnv *env, jobject this, int a, int b)
```

Xamarin.Android macht eine `RegisterNatives` Methode. Stattdessen der Inhaltsfehler und die MCW zusammen bieten die notwendigen Informationen aufzurufenden `RegisterNatives`: der Inhaltsfehler enthält den Methodennamen und die Signatur der JNI-Typ, nur Bedeutung fehlt ein Funktionszeiger zu verknüpfen.

Dies ist, wenn die *Connector Methode* eingeht. Das dritte `[Register]` benutzerdefinierten Attributparameter ist der Name einer Methode definiert, die in den registrierten Typ oder eine Basisklasse des registrierten Typs, der keine Parameter akzeptiert und gibt eine `System.Delegate`. Das zurückgegebene `System.Delegate` verweist wiederum auf eine Methode, die richtige JNI-Funktionssignatur hat. Schließlich der Delegat, der der Connector Methodenrückgabe *müssen* werden Rooting manipuliert worden sein, damit nicht der globale Katalogserver gesammelt, wie der Delegat Java bereitgestellt wird.

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

Die `GetAddHandler` Methode erstellt eine `Func<IntPtr, IntPtr, int, int,
int>` Delegaten bezieht sich auf die `n_Add` -Methode ruft dann [JNINativeWrapper.CreateDelegate](https://developer.xamarin.com/api/member/Android.Runtime.JNINativeWrapper.CreateDelegate/).
`JNINativeWrapper.CreateDelegate` Dient als Wrapper für die angegebene Methode in einem Try/Catch-Block, damit nicht behandelten Ausnahmen behandelt werden und, durch das Auslösen dazu führt der [AndroidEvent.UnhandledExceptionRaiser](https://developer.xamarin.com/api/event/Android.Runtime.AndroidEnvironment.UnhandledExceptionRaiser/) Ereignis. Der resultierende Delegat befindet sich in der statischen `cb_add` Variable, damit das GC der Delegat nicht freigegeben wird.

Schließlich die `n_Add` Methode ist verantwortlich für das Marshalling von die JNI-Parameter für den entsprechenden verwalteten Typen, delegieren die Methode aufrufen.

Hinweis: Verwenden Sie immer `JniHandleOwnership.DoNotTransfer` beim Abrufen einer MCW über eine Java-Instanz. Behandeln sie als einen lokalen Verweis (und somit Aufrufen `JNIEnv.DeleteLocalRef`) unterbrochen verwaltet:&gt; Java -&gt; Stapel Übergänge verwaltet.



### <a name="complete-adder-binding"></a>Führen Sie Adder-Bindung

Die vollständige verwaltet die Bindung für die `mono.android.tests.Adder` Typ ist:

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

Beim Schreiben eines Typs übereinstimmt, die die folgenden Kriterien:

1.  Unterklassen `Java.Lang.Object`

1.  Verfügt über eine `[Register]` benutzerdefiniertes Attribut

1.  `RegisterAttribute.DoNotGenerateAcw` ist gleich `true`.


Klicken Sie dann für die GC-Interaktion der Typ *muss nicht* haben alle Felder, die auf verweisen möglicherweise eine `Java.Lang.Object` oder `Java.Lang.Object` Unterklasse zur Laufzeit. Z. B. die Felder des Typs `System.Object` und einen beliebigen anderen Schnittstellentyp sind nicht zulässig. Typen, die auf verweisen können nicht `Java.Lang.Object` Instanzen zulässig sind, z. B. `System.String` und `List<int>`. Diese Einschränkung wird verhindert, dass vorzeitige objektauflistung von dem globalen Katalogserver.

Wenn der Typ ein Instanzenfelds enthalten muss, die auf verweisen kann eine `Java.Lang.Object` -Instanz erfolgt, der Typ des Felds Transportservers `System.WeakReference` oder `GCHandle`.



## <a name="binding-abstract-methods"></a>Binden von abstrakten Methoden

Binden von `abstract` Methoden ist größtenteils identisch mit der Bindung von virtueller Methoden. Es gibt nur zwei Unterschiede:

1.  Die abstrakte Methode ist abstrakt. Dennoch wurden die `[Register]` Attribut und die zugeordnete Methode Registrierung Binding-Methode wird nur in verschoben der `Invoker` Typ.

1.  Nicht `abstract` `Invoker` Typ erstellt, die die abstrakten Typ. Die `Invoker` Typ überschreiben, muss alle abstrakte Methoden, die in der Basisklasse deklariert, und die überschriebene Implementierung ist die Implementierung der Methode zu binden, obwohl die nicht virtuelle Dispatch Groß-/Kleinschreibung ignoriert werden kann.


Nehmen wir beispielsweise an, die oben genannten `mono.android.test.Adder.add` Methode wäre `abstract`. Würde die C#-Bindung ändern, damit `Adder.Add` wurden abstrakte und eine neue `AdderInvoker` Typ würde definiert werden, die implementiert `Adder.Add`:

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

Die `Invoker` Typ ist nur erforderlich, wenn JNI-Verweise auf Java-erstellte Instanzen zu erhalten.


## <a name="binding-interfaces"></a>Binden von Schnittstellen

Binden von Schnittstellen konzeptionell vergleichbar mit der Bindung von Klassen mit virtuellen Methoden ist allerdings viele Besonderheiten unterscheiden sich in feine (und nur schwer erkennbare) Punkten. Beachten Sie Folgendes [Java-Schnittstellendeklaration](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/Adder.java#L14):

```csharp
public interface Progress {
    void onAdd(int[] values, int currentIndex, int currentSum);
}
```

Schnittstelle Bindungen bestehen aus zwei Teilen: der C#-Schnittstellendefinition und die Definition einer aufrufende Instanz für die Schnittstelle.



### <a name="interface-definition"></a>Schnittstellendefinition

Die Schnittstellendefinition c# muss die folgenden Anforderungen erfüllen:

-   Die Schnittstellendefinition benötigen eine `[Register]` benutzerdefinierten Attributs.

-   Die Schnittstellendefinition erweitern muss die `IJavaObject interface`.
    Bei unterlassen wird verhindert, dass ACWs erben von der Java-Schnittstelle.

-   Jede Schnittstellenmethode darf eine `[Register]` -Attribut gibt die Namen der entsprechenden Java-Methode, die JNI-Signatur und die Connector-Methode.

-   Die Connector-Methode muss auch den Typ angeben, dem auf die Connector-Methode gespeichert werden kann.

Beim Binden von `abstract` und `virtual` Methoden, die Connector-Methode würde durchsucht werden, in der Vererbungshierarchie des Typs, die registriert wird. Schnittstellen können keine Methoden, die Stellen, damit dies funktioniert nicht, daher die Anforderung enthält, die ein Typ angegeben werden, der angibt, auf dem sich die Connector-Methode befindet. Der Typ wird in die Zeichenfolge der Connector-Methode angegeben, nach einem Doppelpunkt `':'`, muss die Assembly qualifizierten Typnamen des Typs mit der aufrufenden Instanz.

Schnittstelle Methodendeklarationen sind eine Übersetzung der entsprechenden Java Methode mit *kompatibel* Typen. Für "Vordefiniert" Java-Datentypen, kompatiblen Typen sind die entsprechenden C#-Typen, z. B. Java `int` ist c# `int`. Für Verweistypen ist die kompatible Typ ein Typ, der ein JNI-Handle des entsprechenden Java-Typs bereitstellen kann.

Die Schnittstellenmember nicht direkt von Java aufgerufen &ndash; Aufruf wird über den Typ der aufrufenden Instanz hergestellt werden &ndash; damit gewisse Flexibilität zulässig ist.

Die Java-Statusschnittstelle kann [als in c# deklariert](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L83):

```csharp
[Register ("mono/android/test/Adder$Progress", DoNotGenerateAcw=true)]
public interface IAdderProgress : IJavaObject {
    [Register ("onAdd", "([III)V",
            "GetOnAddHandler:Mono.Samples.SanityTests.IAdderProgressInvoker, SanityTests, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")]
    void OnAdd (JavaArray<int> values, int currentIndex, int currentSum);
}
```

Beachten Sie, dass die oben genannten, dass wir die Java zuordnen `int[]` Parameter an eine [JavaArray&lt;Int&gt;](https://developer.xamarin.com/api/type/Android.Runtime.JavaArray%601/).
Dies ist nicht erforderlich: wir konnte er gebunden an eine c# `int[]`, oder ein `IList<int>`, oder etwas anderes vollständig. Der Typ ausgewählt wird, die `Invoker` muss es in einer Java übersetzen können `int[]` Typ für Aufruf.


### <a name="invoker-definition"></a>Aufrufer-Definition

Die `Invoker` Typdefinition erben muss `Java.Lang.Object`, die entsprechende Schnittstelle implementieren und geben Sie alle Verbindungsmethoden, die in der Schnittstellendefinition verwiesen wird. Weitere empfohlen, die eine Klasse Bindung nicht vorhanden ist: die `class_ref` Feld- und IDs Instanzmember, der nicht statische Member werden sollte.

Der Grund für Instanzmember ausgehandelt wurde mit `JNIEnv.GetMethodID` Verhalten in der Android-Runtime. (Dies ist möglicherweise auch die Java-Verhalten, noch nicht getestet.) `JNIEnv.GetMethodID` gibt null zurück, bei der Suche nach einer Methode, die aus einer implementierten Schnittstelle und nicht die deklarierte Schnittstelle stammen. Betrachten Sie die ["java.util.SortedMap"&lt;K, V&gt; ](http://developer.android.com/reference/java/util/SortedMap.html) Java-Schnittstelle, die implementiert die [java.util.Map&lt;K, V&gt; ](http://developer.android.com/reference/java/util/Map.html) Schnittstelle. Karte enthält eine [deaktivieren](http://developer.android.com/reference/java/util/Map.html#clear()) -Methode, daher eine scheinbar angemessenen `Invoker` Definition für SortedMap wäre:

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

Die oben genannten schlägt fehl, da `JNIEnv.GetMethodID` zurück `null` beim Nachschlagen der `Map.clear` Methode über die `SortedMap` Klasseninstanz.

Es gibt zwei Lösungen dafür: jede Methode stammen aus stets nachzuverfolgen und haben eine `class_ref` für jede Schnittstelle, oder als Instanzmember remotelesen und die Methode-Lookup ausführen, auf dem am meisten abgeleiteten Klassentyp nicht den Schnittstellentyp. Letztere erfolgt im **Mono.Android.dll**.

Die Definition der aufrufenden Instanz enthält sechs Abschnitte: den Konstruktor der `Dispose` -Methode, die `ThresholdType` und `ThresholdClass` Elemente, die `GetObject` -Methode, Schnittstelle methodenimplementierung und die Implementierung der Connector-Methode.



#### <a name="constructor"></a>Konstruktor

Der Konstruktor muss die Laufzeitklasse der aufgerufenen Instanz zu suchen und speichern die Laufzeitklasse in der Instanz `class_ref` Feld:

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

Hinweis: Die `Handle` -Eigenschaft muss innerhalb des Texts Konstruktor verwendet werden und nicht die `handle` -Parameter, beispielsweise auf Android v4. 0 die `handle` Parameter ist möglicherweise ungültig, nachdem der Basiskonstruktor Ausführung abgeschlossen ist.


#### <a name="dispose-method"></a>Dispose-Methode

Die `Dispose` Methode benötigt, um den globalen Verweis zugeordnet, die im Konstruktor freizugeben:

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

Die `ThresholdType` und `ThresholdClass` Mitglieder sind identisch mit was in einer Klasse Bindung gefunden wird:

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

Eine statische `GetObject` Methode ist erforderlich, um die Unterstützung von [Extensions.JavaCast&lt;T&gt;()](https://developer.xamarin.com/api/member/Android.Runtime.Extensions.JavaCast%7BTResult%7D/p/Android.Runtime.IJavaObject/):

```csharp
partial class IAdderProgressInvoker {
    public static IAdderProgress GetObject (IntPtr handle, JniHandleOwnership transfer)
    {
        return new IAdderProgressInvoker (handle, transfer);
    }
}
```


#### <a name="interface-methods"></a>Schnittstellenmethoden

EVERY-Methode der Schnittstelle muss eine Implementierung aufweisen, die JNI den entsprechenden Java-Methode aufruft:

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

Die Connector-Methoden und die unterstützende Infrastruktur sind verantwortlich für das Marshalling der JNI-Parameter für die entsprechende C#-Typen. Die Java `int[]` Parameter wird als eine JNI übergeben `jintArray`, also eine `IntPtr` in c#. Die `IntPtr` gemarshallt werden müssen, um eine `JavaArray<int>` um Unterstützung für das Aufrufen der C#-Schnittstelle:

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

Wenn `int[]` wäre besser geeignet als `JavaList<int>`, klicken Sie dann [JNIEnv.GetArray()](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetArray/(System.IntPtr%2cAndroid.Runtime.JniHandleOwnership%2cSystem.Type)) kann stattdessen verwendet werden:

```csharp
int[] _values = (int[]) JNIEnv.GetArray(values, JniHandleOwnership.DoNotTransfer, typeof (int));
```

Beachten Sie jedoch, `JNIEnv.GetArray` kopiert das gesamte Array zwischen virtuellen Computern, damit für große Arrays dies in vielen hinzugefügten GC-Druck führen kann.


### <a name="complete-invoker-definition"></a>Vollständige Invoker-Definition

Die [IAdderProgressInvoker Definition abgeschlossen](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L88):

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

Viele JNIEnv-Methoden zurückgeben *JNI* *Objektverweise*, dem ähneln `GCHandle`s. JNI bietet drei verschiedene Typen von Objektverweisen: lokalen verweisen, globale Verweise und schwache globalen Verweise. Alle drei werden als dargestellt `System.IntPtr`, *jedoch* (gemäß Abschnitt JNI-Funktionstypen) nicht alle `IntPtr`s Merry `JNIEnv` Methoden sind Verweise. Beispielsweise [JNIEnv.GetMethodID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetMethodID/) gibt eine `IntPtr`, es gibt keinen Objektverweis zurück, es gibt jedoch eine `jmethodID`. Wenden Sie sich an den [Dokumentation JNI-Funktion](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html) Details.

Lokale Verweise werden erstellt, indem *die meisten* Methoden Verweis erstellen.
Android gestattet nur eine begrenzte Anzahl von lokalen Verweisen zu jedem Zeitpunkt in der Regel 512 vorhanden sein. Lokale Verweise gelöscht werden können, über [JNIEnv.DeleteLocalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteLocalRef/).
Im Gegensatz zu JNI verweisen nicht alle JNIEnv-Methoden, die dem Rückgabeobjekt Verweise lokale Systemverweise zurückgeben. [JNIEnv.FindClass](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.FindClass/) gibt eine *globale* Verweis. Es wird dringend empfohlen, dass Sie lokale Verweise, so schnell löschen wie möglich, möglicherweise durch das Erstellen einer [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) um das Objekt und Angeben von `JniHandleOwnership.TransferLocalRef` auf die [Java.Lang.Object (IntPtr behandeln, JniHandleOwnership Übertragung)](https://developer.xamarin.com/api/constructor/Java.Lang.Object.Object/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) Konstruktor.

Globale Verweise werden erstellt, indem [JNIEnv.NewGlobalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.NewGlobalRef/) und [JNIEnv.FindClass](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.FindClass/).
Sie können mit zerstört [JNIEnv.DeleteGlobalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteGlobalRef/).
Emulatoren sind ein Limit von 2.000 ausstehenden globalen Verweise Hardwaregeräte ein Limit von ca. 52.000 globalen Verweise vorhanden.

Schwache globalen Verweise sind nur verfügbar, auf Android v2. 2 (Froyo) und höher. Schwache globalen Verweise gelöscht werden können, mit [JNIEnv.DeleteWeakGlobalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteWeakGlobalRef/(System.IntPtr)).


### <a name="dealing-with-jni-local-references"></a>Umgang mit lokalen JNI-Verweise

Die [JNIEnv.GetObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetObjectField/), [JNIEnv.GetStaticObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticObjectField/), [JNIEnv.CallObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallObjectMethod/), [JNIEnv.CallNonvirtualObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualObjectMethod/)und [JNIEnv.CallStaticObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticObjectMethod/) -Methoden zurückgeben einer `IntPtr` enthält einen lokalen JNI-Verweis auf ein Java-Objekt oder `IntPtr.Zero` Java zurückgegeben `null`. Aufgrund der begrenzten Anzahl von lokalen verweisen, die bei ausstehenden sein können, sobald die (512 Einträge), um sicherzustellen, dass die Verweise wünschenswert ist rechtzeitig gelöscht. Es gibt drei Möglichkeiten, die lokale verweisen behandelt werden können: explizit gelöscht werden, erstellen eine `Java.Lang.Object` Instanz zum aufrechterhalten werden und mit `Java.Lang.Object.GetObject<T>()` einen verwalteten callable Wrapper darum zu erstellen.



### <a name="explicitly-deleting-local-references"></a>Explizit löschen von lokalen verweisen

[JNIEnv.DeleteLocalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteLocalRef/) wird zum Löschen von lokaler verweisen. Nachdem Sie der lokale Verweis gelöscht wurde, nicht verwendet werden nicht mehr, damit darauf geachtet werden muss, um sicherzustellen, `JNIEnv.DeleteLocalRef` ist das letzte Ereignis, das mit der lokalen Objektverweis durchgeführt.

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

`Java.Lang.Object` Stellt eine [Java.Lang.Object (IntPtr-Handles, JniHandleOwnership Übertragung)](https://developer.xamarin.com/api/constructor/Java.Lang.Object.Object/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) Konstruktor, der verwendet werden kann, um ein bestehendes JNI-Referenz zu umschließen. Die [JniHandleOwnership](https://developer.xamarin.com/api/type/Android.Runtime.JniHandleOwnership/) Parameter bestimmt, wie die `IntPtr` Parameter behandelt werden soll:

-   [JniHandleOwnership.DoNotTransfer](https://developer.xamarin.com/api/field/Android.Runtime.JniHandleOwnership.DoNotTransfer/) &ndash; erstellten `Java.Lang.Object` Instanz erstellt einen neuen globalen Verweis aus der `handle` -Parameter und `handle` bleibt unverändert.
    Der Aufrufer ist dafür verantwortlich, die Freigabe `handle` , falls erforderlich.

-   [JniHandleOwnership.TransferLocalRef](https://developer.xamarin.com/api/field/Android.Runtime.JniHandleOwnership.TransferLocalRef/) &ndash; erstellten `Java.Lang.Object` Instanz erstellt einen neuen globalen Verweis aus der `handle` -Parameter und `handle` wird gelöscht, mit [JNIEnv.DeleteLocalRef ](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteLocalRef/) . Der Aufrufer muss nicht freigeben `handle` , und darf kein verwenden `handle` nach der Konstruktor Ausführung abgeschlossen ist.

-   [JniHandleOwnership.TransferGlobalRef](https://developer.xamarin.com/api/field/Android.Runtime.JniHandleOwnership.TransferLocalRef/) &ndash; The created `Java.Lang.Object` instance will take over ownership of the `handle` parameter. Der Aufrufer muss nicht freigeben `handle` .


Da die Methoden, die JNI-Methode lokalen Refs zurückgeben `JniHandleOwnership.TransferLocalRef` wird normalerweise verwendet werden:

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef);
```

Der erstellte globale Verweis wird nicht reserviert werden, bis die `Java.Lang.Object` Instanz ist die Garbage collection. Wenn Sie Lage sind, wird löschen, mit der die Instanz der globalen Verweis beschleunigen von Garbage Collections der freizugeben:

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
using (var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef)) {
    // use value ...
}
```



### <a name="using-javalangobjectgetobjectlttgt"></a>Verwenden Java.Lang.Object.GetObject&lt;T&gt;)

`Java.Lang.Object` Stellt eine [Java.Lang.Object.GetObject&lt;T&gt;(IntPtr-Handles, JniHandleOwnership Übertragung)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) -Methode, die zum Erstellen eines verwalteten callable Wrappers des angegebenen Typs verwendet werden kann.

Der Typ `T` die folgenden Anforderungen müssen erfüllt sein:

1.  `T` ein Referenztyp muss sein.

1.  `T` Implementieren Sie müssen die `IJavaObject` Schnittstelle.

1.  Wenn `T` ist keine abstrakte Klasse oder Schnittstelle, klicken Sie dann `T` muss einen Konstruktor mit den Parametertypen bereitstellen `(IntPtr,
    JniHandleOwnership)` .

1.  Wenn `T` ist eine abstrakte Klasse oder eine Schnittstelle *müssen* werden ein *Invoker* zur `T` . Eine aufrufende Instanz ist ein abstrakter Typ, der erbt `T` oder implementiert `T` , und hat den gleichen Namen wie `T` mit einem Suffix für die aufrufende Instanz. Wenn die Schnittstelle "T" ist beispielsweise `Java.Lang.IRunnable` , klicken Sie dann den Typ `Java.Lang.IRunnableInvoker` muss vorhanden sein und darf die erforderlichen `(IntPtr,
    JniHandleOwnership)` Konstruktor.


Da die Methoden, die JNI-Methode lokalen Refs zurückgeben `JniHandleOwnership.TransferLocalRef` wird normalerweise verwendet werden:

```csharp
IntPtr lrefString = JNIEnv.CallObjectMethod(instance, methodID);
Java.Lang.String value = Java.Lang.Object.GetObject<Java.Lang.String>( lrefString, JniHandleOwnership.TransferLocalRef);
```

<a name="_Looking_up_Java_Types" />

## <a name="looking-up-java-types"></a>Nachschlagen von Java-Typen

Um ein Feld oder eine Methode in JNI zu suchen, muss zuerst der deklarierende Typ für die Feld- oder Methodenparameter nachgeschlagen werden. Die [Android.Runtime.JNIEnv.FindClass(string)](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.FindClass/(System.String)) Methode wird verwendet, um Java-Typen zu suchen. Der Zeichenfolgenparameter ist die *vereinfacht Typverweis* oder *vollständige Typverweis* für den Java-Typ. Finden Sie unter der [Typverweise JNI-Abschnitt](#_JNI_Type_References) ausführliche Informationen über den vereinfachten und vollständige Typverweise.

Hinweis: im Gegensatz zu allen anderen `JNIEnv` Methode die Objekt-Instanzen zurückgibt `FindClass` gibt einen globalen Verweis, nicht auf einen lokalen Verweis zurück.

<a name="_Instance_Fields" />

## <a name="instance-fields"></a>Instanzfelder

Felder werden durch bearbeitet *Feld IDs*. Feld-IDs über abgerufen werden [JNIEnv.GetFieldID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetFieldID/), der erfordert, dass der Klasse, die das Feld definiert ist, den Namen des Felds, und die [JNI Typsignatur](#JNI_Type_Signatures) des Felds.

Feld-IDs müssen nicht freigegeben werden, und sind gültig, solange der entsprechende Java-Typ geladen wird. (Android unterstützt Klasse entladen augenblicklich nicht.)

Es gibt zwei Sätze von Methoden zum Bearbeiten von Instanzfelder: eine zum Lesen der Instanzfelder und eine für das Schreiben von Instanzfelder. Alle Sätze von Methoden erfordern eine Feld-ID zum Lesen oder schreiben den Wert des Felds.


### <a name="reading-instance-field-values"></a>Lesen Instanz Feldwerte

Der Satz von Methoden für das Lesen der Instanz Feldwerte folgt dem Benennungsmuster:

```csharp
* JNIEnv.Get*Field(IntPtr instance, IntPtr fieldID);
```
wobei `*` ist der Typ des Felds:

-   [JNIEnv.GetObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetObjectField/) &ndash; lesen den Wert eines Instanzenfelds, die ein Typ ist "Vordefiniert", wie z. B. nicht `java.lang.Object` , Arrays und Schnittstellentypen. Der zurückgegebene Wert ist ein Verweis auf lokale JNI.

-   [JNIEnv.GetBooleanField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetBooleanField/) &ndash; lesen den Wert der `bool` Instanzenfelder.

-   [JNIEnv.GetByteField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetByteField/) &ndash; lesen den Wert der `sbyte` Instanzenfelder.

-   [JNIEnv.GetCharField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetCharField/) &ndash; lesen den Wert der `char` Instanzenfelder.

-   [JNIEnv.GetShortField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetShortField/) &ndash; lesen den Wert der `short` Instanzenfelder.

-   [JNIEnv.GetIntField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetIntField/) &ndash; lesen den Wert der `int` Instanzenfelder.

-   [JNIEnv.GetLongField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetLongField/) &ndash; lesen den Wert der `long` Instanzenfelder.

-   [JNIEnv.GetFloatField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetFloatField/) &ndash; lesen den Wert der `float` Instanzenfelder.

-   [JNIEnv.GetDoubleField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetDoubleField/) &ndash; lesen den Wert der `double` Instanzenfelder.





### <a name="writing-instance-field-values"></a>Schreiben von Instanz-Feldwerte

Der Satz von Methoden zum Schreiben der Instanz Feldwerte folgt dem Benennungsmuster:

```csharp
JNIEnv.SetField(IntPtr instance, IntPtr fieldID, Type value);
```

wobei *Typ* ist der Typ des Felds:

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.IntPtr)) &ndash; schreiben Sie den Wert eines Felds, die ein Typ ist "Vordefiniert", wie z. B. nicht `java.lang.Object` , Arrays und Schnittstellentypen. Die `IntPtr` Wert ist möglicherweise eine JNI lokalen, Verweis JNI globalen, JNI schwachen globalen Verweis oder `IntPtr.Zero` (für `null` ).

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Boolean)) &ndash; schreiben Sie den Wert der `bool` Instanzenfelder.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.SByte)) &ndash; schreiben Sie den Wert der `sbyte` Instanzenfelder.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Char)) &ndash; schreiben Sie den Wert der `char` Instanzenfelder.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Int16)) &ndash; schreiben Sie den Wert der `short` Instanzenfelder.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Int32)) &ndash; schreiben Sie den Wert der `int` Instanzenfelder.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Int64)) &ndash; schreiben Sie den Wert der `long` Instanzenfelder.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Single)) &ndash; schreiben Sie den Wert der `float` Instanzenfelder.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Double)) &ndash; schreiben Sie den Wert der `double` Instanzenfelder.


<a name="_Static_Fields" />

## <a name="static-fields"></a>Statische Felder

Statische Felder sind durch bearbeitet *Feld IDs*. Feld-IDs über abgerufen werden [JNIEnv.GetStaticFieldID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticFieldID/), der erfordert, dass der Klasse, die das Feld definiert ist, den Namen des Felds, und die [JNI Typsignatur](#JNI_Type_Signatures) des Felds.

Feld-IDs müssen nicht freigegeben werden, und sind gültig, solange der entsprechende Java-Typ geladen wird. (Android unterstützt Klasse entladen augenblicklich nicht.)

Es gibt zwei Sätze von Methoden zum Bearbeiten von statischen Feldern: eine zum Lesen der Instanzfelder und eine für das Schreiben von Instanzfelder. Alle Sätze von Methoden erfordern eine Feld-ID zum Lesen oder schreiben den Wert des Felds.


### <a name="reading-static-field-values"></a>Lesen Sie die Werte für statische Felder

Der Satz von Methoden zum Lesen von statischen Feldwerte folgt dem Benennungsmuster:

```csharp
* JNIEnv.GetStatic*Field(IntPtr class, IntPtr fieldID);
```

wobei `*` ist der Typ des Felds:

-   [JNIEnv.GetStaticObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticObjectField/) &ndash; lesen den Wert der statische Felder, die einen Typ "Vordefiniert" wird z. B. nicht `java.lang.Object` , Arrays und Schnittstellentypen. Der zurückgegebene Wert ist ein Verweis auf lokale JNI.

-   [JNIEnv.GetStaticBooleanField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticBooleanField/) &ndash; lesen den Wert der `bool` statische Felder.

-   [JNIEnv.GetStaticByteField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticByteField/) &ndash; lesen den Wert der `sbyte` statische Felder.

-   [JNIEnv.GetStaticCharField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticCharField/) &ndash; lesen den Wert der `char` statische Felder.

-   [JNIEnv.GetStaticShortField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticShortField/) &ndash; lesen den Wert der `short` statische Felder.

-   [JNIEnv.GetStaticLongField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticLongField/) &ndash; lesen den Wert der `long` statische Felder.

-   [JNIEnv.GetStaticFloatField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticFloatField/) &ndash; lesen den Wert der `float` statische Felder.

-   [JNIEnv.GetStaticDoubleField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticDoubleField/) &ndash; lesen den Wert der `double` statische Felder.



### <a name="writing-static-field-values"></a>Schreiben von statischen Feldwerte

Der Satz von Methoden zum Schreiben von statischen Feldwerte folgt dem Benennungsmuster:

```csharp
JNIEnv.SetStaticField(IntPtr class, IntPtr fieldID, Type value);
```

wobei *Typ* ist der Typ des Felds:

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.IntPtr)) &ndash; schreiben Sie den Wert der statische Felder, die einen Typ "Vordefiniert" wird z. B. nicht `java.lang.Object` , Arrays und Schnittstellentypen. Die `IntPtr` Wert ist möglicherweise eine JNI lokalen, Verweis JNI globalen, JNI schwachen globalen Verweis oder `IntPtr.Zero` (für `null` ).

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Boolean)) &ndash; schreiben Sie den Wert der `bool` statische Felder.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.SByte)) &ndash; schreiben Sie den Wert der `sbyte` statische Felder.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Char)) &ndash; schreiben Sie den Wert der `char` statische Felder.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Int16)) &ndash; schreiben Sie den Wert der `short` statische Felder.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Int32)) &ndash; schreiben Sie den Wert der `int` statische Felder.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Int64)) &ndash; schreiben Sie den Wert der `long` statische Felder.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Single)) &ndash; schreiben Sie den Wert der `float` statische Felder.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Double)) &ndash; schreiben Sie den Wert der `double` statische Felder.


<a name="_Instance_Methods" />

## <a name="instance-methods"></a>Instanzmethoden

Instanzmethoden werden aufgerufen, durch *Methode IDs*. IDs-Methode erhalten Sie über [JNIEnv.GetMethodID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetMethodID/), die erfordert, dass der Typ, der die Methode definiert ist, den Namen der Methode, und die [JNI Typsignatur](#JNI_Type_Signatures) der Methode.

Methode-IDs müssen nicht freigegeben werden, und sind gültig, solange der entsprechende Java-Typ geladen wird. (Android unterstützt Klasse entladen augenblicklich nicht.)

Es gibt zwei Sätze von Methoden zum Aufrufen von Methoden: eine für das Aufrufen von Methoden praktisch und eine zum Aufrufen von Methoden nicht virtuell. Beide Gruppen von Methoden erfordern eine ID Methode zum Aufrufen der Methode, und nicht virtuellen Aufruf erfordert auch, dass Sie angeben, welche klassenimplementierung aufgerufen werden soll.

Schnittstellenmethoden können nur innerhalb der deklarierende Typ gesucht werden; Methoden, die Schnittstellen erweiterte/vererbt stammen können nicht gesucht werden. Finden Sie unter den neueren Schnittstellen Bindung / Invoker Implementierung section Weitere Details.

Jede Methode in der Klasse deklariert, oder eine Basisklasse oder die implementierte Schnittstelle kann nachgeschlagen werden.


### <a name="virtual-method-invocation"></a>Virtuelle Methodenaufruf

Der Satz von Methoden zum Aufrufen von Methoden folgt virtuell das Namensmuster:

```csharp
* JNIEnv.Call*Method( IntPtr instance, IntPtr methodID, params JValue[] args );
```

wobei `*` ist der Rückgabetyp der Methode.

-   [JNIEnv.CallObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallObjectMethod/) &ndash; Aufrufen einer Methode, die einen Typ nicht "Vordefiniert", wie z. B. zurückgibt `java.lang.Object` , arrays und Schnittstellen. Der zurückgegebene Wert ist ein Verweis auf lokale JNI.

-   [JNIEnv.CallBooleanMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallBooleanMethod/) &ndash; Aufrufen einer Methode die zurückgibt, eine `bool` Wert.

-   [JNIEnv.CallByteMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallByteMethod/) &ndash; Aufrufen einer Methode die zurückgibt, eine `sbyte` Wert.

-   [JNIEnv.CallCharMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallCharMethod/) &ndash; Aufrufen einer Methode die zurückgibt, eine `char` Wert.

-   [JNIEnv.CallShortMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallShortMethod/) &ndash; Aufrufen einer Methode die zurückgibt, eine `short` Wert.

-   [JNIEnv.CallLongMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallLongMethod/) &ndash; Aufrufen einer Methode die zurückgibt, eine `long` Wert.

-   [JNIEnv.CallFloatMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallFloatMethod/) &ndash; Aufrufen einer Methode die zurückgibt, eine `float` Wert.

-   [JNIEnv.CallDoubleMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallDoubleMethod/) &ndash; Aufrufen einer Methode die zurückgibt, eine `double` Wert.



### <a name="non-virtual-method-invocation"></a>Nicht virtuelle Methodenaufruf

Der Satz von Methoden zum Aufrufen von Methoden befolgt nicht virtuell das Namensmuster:

```csharp
* JNIEnv.CallNonvirtual*Method( IntPtr instance, IntPtr class, IntPtr methodID, params JValue[] args );
```

wobei `*` ist der Rückgabetyp der Methode. Nicht virtuelle Methodenaufruf wird normalerweise verwendet, um die Basismethode einer virtuellen Methode aufzurufen.

-   [JNIEnv.CallNonvirtualObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualObjectMethod/) &ndash; nicht virtuell Aufrufen einer Methode die zurückgibt, wie z. B. einen Typ nicht "Vordefiniert" `java.lang.Object` , arrays und Schnittstellen. Der zurückgegebene Wert ist ein Verweis auf lokale JNI.

-   [JNIEnv.CallNonvirtualBooleanMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualBooleanMethod/) &ndash; nicht virtuell Aufrufen einer Methode die zurückgibt, eine `bool` Wert.

-   [JNIEnv.CallNonvirtualByteMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualByteMethod/) &ndash; nicht virtuell Aufrufen einer Methode die zurückgibt, eine `sbyte` Wert.

-   [JNIEnv.CallNonvirtualCharMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualCharMethod/) &ndash; nicht virtuell Aufrufen einer Methode die zurückgibt, eine `char` Wert.

-   [JNIEnv.CallNonvirtualShortMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualShortMethod/) &ndash; nicht virtuell Aufrufen einer Methode die zurückgibt, eine `short` Wert.

-   [JNIEnv.CallNonvirtualLongMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualLongMethod/) &ndash; nicht virtuell Aufrufen einer Methode die zurückgibt, eine `long` Wert.

-   [JNIEnv.CallNonvirtualFloatMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualFloatMethod/) &ndash; nicht virtuell Aufrufen einer Methode die zurückgibt, eine `float` Wert.

-   [JNIEnv.CallNonvirtualDoubleMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualDoubleMethod/) &ndash; nicht virtuell Aufrufen einer Methode die zurückgibt, eine `double` Wert.


<a name="_Static_Methods" />

## <a name="static-methods"></a>Statische Methoden

Statische Methoden werden aufgerufen, durch *Methode IDs*. IDs-Methode erhalten Sie über [JNIEnv.GetStaticMethodID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticMethodID/), die erfordert, dass der Typ, der die Methode definiert ist, den Namen der Methode, und die [JNI Typsignatur](#JNI_Type_Signatures) der Methode.

Methode-IDs müssen nicht freigegeben werden, und sind gültig, solange der entsprechende Java-Typ geladen wird. (Android unterstützt Klasse entladen augenblicklich nicht.)



### <a name="static-method-invocation"></a>Statische Methode aufrufen

Der Satz von Methoden zum Aufrufen von Methoden folgt virtuell das Namensmuster:

```csharp
* JNIEnv.CallStatic*Method( IntPtr class, IntPtr methodID, params JValue[] args );
```

wobei `*` ist der Rückgabetyp der Methode.

-   [JNIEnv.CallStaticObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticObjectMethod/) &ndash; Aufrufen einer statischen Methode, die einen Typ nicht "Vordefiniert", wie z. B. zurückgibt `java.lang.Object` , arrays und Schnittstellen. Der zurückgegebene Wert ist ein Verweis auf lokale JNI.

-   [JNIEnv.CallStaticBooleanMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticBooleanMethod/) &ndash; Aufrufen einer statischen Methode die zurückgibt, eine `bool` Wert.

-   [JNIEnv.CallStaticByteMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticByteMethod/) &ndash; Aufrufen einer statischen Methode die zurückgibt, eine `sbyte` Wert.

-   [JNIEnv.CallStaticCharMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticCharMethod/) &ndash; Aufrufen einer statischen Methode die zurückgibt, eine `char` Wert.

-   [JNIEnv.CallStaticShortMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticShortMethod/) &ndash; Aufrufen einer statischen Methode die zurückgibt, eine `short` Wert.

-   [JNIEnv.CallStaticLongMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallLongMethod/) &ndash; Aufrufen einer statischen Methode die zurückgibt, eine `long` Wert.

-   [JNIEnv.CallStaticFloatMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticFloatMethod/) &ndash; Aufrufen einer statischen Methode die zurückgibt, eine `float` Wert.

-   [JNIEnv.CallStaticDoubleMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticDoubleMethod/) &ndash; Aufrufen einer statischen Methode die zurückgibt, eine `double` Wert.


<a name="JNI_Type_Signatures" />

## <a name="jni-type-signatures"></a>JNI Typsignaturen

[JNI Typsignaturen](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/types.html#wp16432) sind [JNI Typverweise](#_JNI_Type_References) (jedoch nicht vereinfachte Typverweise), außer bei Methoden. Methoden, die JNI-Typsignatur ist eine öffnende Klammer `'('`, gefolgt von der Typverweise für alle Typen, die verkettet (mit keine Kommas trennen oder Sonstiges), gefolgt von einer schließenden Klammer Parameters `')'`, gefolgt von den JNI-Typverweis, der dem Rückgabetyp der Methode.

Angenommen, die Java-Methode:

```java
long f(int n, String s, int[] array);
```

Die Typsignatur JNI würde folgendermaßen lauten:

```csharp
(ILjava/lang/String;[I)J
```

Im Allgemeinen ist es *stark* empfohlen, verwenden die `javap` Befehl JNI-Signaturen zu bestimmen. Z. B. die JNI Typsignatur der [java.lang.Thread.State.valueOf(String)](http://developer.android.com/reference/java/lang/Thread.State.html#valueOf(java.lang.String)) Methode ist "(Ljava/Lang/String); Ljava/Lang / $Threadzustand;", während die JNI geben die Signatur des der [ java.lang.Thread.State.values](http://developer.android.com/reference/java/lang/Thread.State.html#values) Methode ist "() [Ljava/Lang / $Threadzustand;". Achten Sie nachfolgende Semikolons; Diese *sind* JNI-Typsignatur Teil.

<a name="_JNI_Type_References" />

## <a name="jni-type-references"></a>JNI Typverweisen

JNI Typverweise unterscheiden sich von Java-Typverweisen. Können keine vollqualifizierte Namen der Java-Typ wie z. B. `java.lang.String` mit JNI, müssen Sie stattdessen die JNI-Varianten verwenden `"java/lang/String"` oder `"Ljava/lang/String;"`, je nach Kontext; Weitere Details siehe unten.
Es gibt vier Arten von Typverweisen JNI:

-  **built-in**
-  **simplified**
-  **Typ**
-  **array**


### <a name="built-in-type-references"></a>Integrierte Typverweisen

Integrierte Typverweise sind ein einzelnes Zeichen verwendet, um die integrierten Werttypen verweisen. Die Zuordnung ist wie folgt aus:

-  `"B"` für `sbyte` .
-  `"S"` für `short` .
-  `"I"` für `int` .
-  `"J"` für `long` .
-  `"F"` für `float` .
-  `"D"` für `double` .
-  `"C"` für `char` .
-  `"Z"` für `bool` .
-  `"V"` für `void` Methode Rückgabetypen.


<a name="_Simplified_Type_References_1" />

### <a name="simplified-type-references"></a>Vereinfachte Typverweisen

Vereinfachte Typverweise können nur verwendet werden, [JNIEnv.FindClass(string)](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.FindClass/(System.String)).
Es gibt zwei Möglichkeiten, um eine vereinfachte Typverweis abgeleitet werden:

1.  Ersetzen Sie einen Namen für vollständig qualifizierte Java jeder `'.'` innerhalb der Paketname und vor dem Typnamen mit `'/'` , und jeder `'.'` in einen Typnamen mit `'$'` .

1.  Lesen die Ausgabe des `'unzip -l android.jar | grep JavaName'` .


Eine der beiden führt zu den Java-Typ [java.lang.Thread.State](http://developer.android.com/reference/java/lang/Thread.State.html) zugeordnet wird, um die vereinfachte Typverweis `java/lang/Thread$State`.


### <a name="type-references"></a>Typverweisen

Ein Typverweis ist ein integrierter Typverweis oder eine vereinfachte Typverweis mit einer `'L'` Präfix und einem `';'` Suffix. Für die Java-Typ [java.lang.String](http://developer.android.com/reference/java/lang/String.html), ist die vereinfachte Typverweis `"java/lang/String"`, während die Datentyp-Methodenverweis `"Ljava/lang/String;"`.

Geben Sie Verweise werden mit Array Typverweise und JNI-Signaturen verwendet.

Eine weitere Möglichkeit zum Abrufen von eines Typverweis wird durch Lesen der Ausgabe der `'javap -s -classpath android.jar fully.qualified.Java.Name'`.
Je nach Typ beteiligt sind, können Sie eine Deklaration für einen Konstruktor oder eine Methode geben Sie den JNI-Namen ermitteln. Zum Beispiel:

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

`Thread.State` ein Java-Enum-Typ ist, daher können wir die Signatur der verwenden die `valueOf` Methode, um festzulegen, dass der Datentyp-Methodenverweis Ljava/Lang / $Threadzustand; ist.



### <a name="array-type-references"></a>Array von Typverweisen

Array-Typverweise sind `'['` an einen Typverweis JNI vorangestellt.
Vereinfachte Typverweise können nicht verwendet werden, wenn Sie Arrays angeben.

Beispielsweise `int[]` ist `"[I"`, `int[][]` ist `"[[I"`, und `java.lang.Object[]` ist `"[Ljava/lang/Object;"`.



## <a name="java-generics-and-type-erasure"></a>Java-Generika und Typlöschung

*Die meisten* Fällen gesehen über JNI Java Generika *nicht vorhanden sind*.
Es gibt einige "Falten", jedoch diese Falten in Interaktion Java bei Generika können nicht mit wie JNI sucht und generische Member aufruft.

Bei der Interaktion über JNI besteht kein Unterschied zwischen einen generischen Typ oder Member und einen nichtgenerischen Typ oder Member. Z. B. der generische Typ [java.lang.Class&lt;T&gt; ](http://developer.android.com/reference/java/lang/Class.html) ist auch die "unformatierte" generischer Typ `java.lang.Class`, beide haben den gleichen vereinfachte Typverweis `"java/lang/Class"`.


## <a name="java-native-interface-support"></a>Java Native Interface-Unterstützung

[Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/) wird von verwalteten Wrappern für Jave systemeigene Schnittstelle (JNI). JNI-Funktionen werden deklariert, innerhalb der [Java Native Interface Specification](http://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html), obwohl die Methoden geändert wurden, um die explizite entfernen `JNIEnv*` Parameter und `IntPtr` anstelle von `jobject`, `jclass`, `jmethodID`usw. Betrachten Sie beispielsweise die [JNI NewObject-Funktion](http://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp4517):

```csharp
jobject NewObjectA(JNIEnv *env, jclass clazz, jmethodID methodID, jvalue *args);
```

Dies wird verfügbar gemacht, als die [JNIEnv.NewObject](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.NewObject/p/System.IntPtr/System.IntPtr/Android.Runtime.JValue[]/) Methode:

```csharp
public static IntPtr NewObject(IntPtr clazz, IntPtr jmethod, params JValue[] parms);
```

Die Übersetzung zwischen den beiden aufrufen ist ziemlich einfach. In C# müssten Sie:

```c
jobject CreateMapActivity(JNIEnv *env)
{
    jclass    Map_Class   = (*env)->FindClass(env, "mono/samples/googlemaps/MyMapActivity");
    jmethodID Map_defCtor = (*env)->GetMethodID (env, Map_Class, "<init>", "()V");
    jobject   instance    = (*env)->NewObject (env, Map_Class, Map_defCtor);

    return instance;
}
```

Das C#-Äquivalent würde folgendermaßen lauten:

```csharp
IntPtr CreateMapActivity()
{
    IntPtr Map_Class   = JNIEnv.FindClass ("mono/samples/googlemaps/MyMapActivity");
    IntPtr Map_defCtor = JNIEnv.GetMethodID (Map_Class, "<init>", "()V");
    IntPtr instance    = JNIEnv.NewObject (Map_Class, Map_defCtor);

    return instance;
}
```

Nachdem Sie eine Java-Objektinstanz in einen IntPtr gehalten haben, möchten Sie wahrscheinlich etwas damit tun. Sie verwenden die JNIEnv Methoden wie [ <span class="external">JNIEnv.CallVoidMethod()</span> ](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallVoidMethod/p/System.IntPtr/System.IntPtr/Android.Runtime.JValue[]/) tun, aber wenn es befindet sich bereits ein analoger C#-Wrapper, sollten Sie einen Wrapper über die JNI-Verweis zu erstellen. Sie erreichen dies über die [Extensions.JavaCast <t>()</t> ](https://developer.xamarin.com/api/member/Android.Runtime.Extensions.JavaCast%7BTResult%7D/p/Android.Runtime.IJavaObject/) Erweiterungsmethode:

```csharp
IntPtr lrefActivity = CreateMapActivity();

// imagine that Activity were instead an interface or abstract type...
Activity mapActivity = new Java.Lang.Object(lrefActivity, JniHandleOwnership.TransferLocalRef)
    .JavaCast<Activity>();
```

Sie können auch die [Java.Lang.Object.GetObject <t>()</t> ](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) Methode:

```csharp
IntPtr lrefActivity = CreateMapActivity();

// imagine that Activity were instead an interface or abstract type...
Activity mapActivity = Java.Lang.Object.GetObject<Activity>(lrefActivity, JniHandleOwnership.TransferLocalRef);
```

Darüber hinaus alle Funktionen JNI geändert wurden durch das Entfernen der `JNIEnv*` Parameter, die in jeder JNI-Funktion vorhanden.


## <a name="summary"></a>Zusammenfassung

Umgang mit JNI direkt umfasst eine schrecklichen, die unter allen Umständen sollten vermieden werden. Leider ist es nicht immer vermeidbaren; Dieses Handbuch wird hoffentlich einige Unterstützung bieten, wenn Sie für Android ungebundenen Java Fälle mit Mono erreicht.


## <a name="related-links"></a>Verwandte Links

- [SanityTests (Beispiel)](https://developer.xamarin.com/samples/SanityTests/)
- [Java Native Interface-Spezifikation](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/jniTOC.html)
- [Java Native Interface-Funktionen](http://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html)
