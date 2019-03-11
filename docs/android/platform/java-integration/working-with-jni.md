---
title: Arbeiten mit JNI
description: Xamarin.Android ermöglicht das Schreiben von Android-apps in C# anstelle von Java. Mehrere Assemblys sind, die mit Xamarin.Android bereitgestellt, die Bindungen für Java-Clientbibliotheken, einschließlich Mono.Android.dll und Mono.Android.GoogleMaps.dll. Allerdings Bindungen nicht für alle möglichen Java-Bibliothek bereitgestellt werden, und die Bindungen, die bereitgestellt werden können nicht gebunden werden, jedem Java-Typen und Member. Um ungebundenen Java-Typen und Member verwenden, kann der Java Native Interface (JNI) verwendet werden. In diesem Artikel wird veranschaulicht, wie JNI für die Interaktion mit Java-Typen und Member von Xamarin.Android-Anwendungen verwendet wird.
ms.prod: xamarin
ms.assetid: A417DEE9-7B7B-4E35-A79C-284739E3838E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 8ad2dde701814c0977e25e6e58272c0aa01ca4ca
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57672845"
---
# <a name="working-with-jni"></a>Arbeiten mit JNI

_Xamarin.Android ermöglicht das Schreiben von Android-apps in C# anstelle von Java. Mehrere Assemblys sind, die mit Xamarin.Android bereitgestellt, die Bindungen für Java-Clientbibliotheken, einschließlich Mono.Android.dll und Mono.Android.GoogleMaps.dll. Allerdings Bindungen nicht für alle möglichen Java-Bibliothek bereitgestellt werden, und die Bindungen, die bereitgestellt werden können nicht gebunden werden, jedem Java-Typen und Member. Um ungebundenen Java-Typen und Member verwenden, kann der Java Native Interface (JNI) verwendet werden. In diesem Artikel wird veranschaulicht, wie JNI für die Interaktion mit Java-Typen und Member von Xamarin.Android-Anwendungen verwendet wird._


## <a name="overview"></a>Übersicht

Es ist nicht immer notwendig oder möglich, eine verwaltete Callable Wrapper (MCW) zum Aufrufen von Java-Code zu erstellen. In vielen Fällen "Inline" JNI durchaus akzeptabel ist und für die einmalige Verwendung von ungebundenen Java-Elementen hilfreich. Es ist häufig einfacher, JNI zu verwenden, um eine einzelne Methode für eine Javaklasse als generieren, mit eine gesamten JAR-Bindung aufzurufen.

Xamarin.Android bietet die `Mono.Android.dll` -Assembly, die eine Bindung für Android bietet `android.jar` Bibliothek. Typen und Member nicht vorhanden in `Mono.Android.dll` und nicht in vorhandene `android.jar` kann durch manuelles binden sie verwendet werden. Um Java-Typen und Member zu binden, verwenden Sie die **Java Native Interface** (**JNI**) auf Typen zu suchen, lesen und schreiben die Felder und Methoden aufrufen.

Die JNI-API in Xamarin.Android ähnelt vom Konzept her der `System.Reflection` -API in .NET: erleichtert es möglich, dass Sie für die Suche Typen und Member nach Namen, lesen und Schreiben von Feldwerten haben, rufen Sie Methoden und vieles mehr. JNI können und die `Android.Runtime.RegisterAttribute` benutzerdefiniertes Attribut, um virtuelle Methoden zu deklarieren, die gebunden werden kann, um überschreiben zu unterstützen. Sie können Schnittstellen binden, sodass sie in implementiert werden können C#.

Dieses Dokument erläutert:

-  Wie JNI Typen verweist.
-  So suchen, lesen und Schreiben von Feldern.
-  Informationen zum Suchen und Aufrufen von Methoden.
-  Informationen zum Verfügbarmachen von virtueller Methoden zum Zulassen von verwaltetem Code überschreiben.
-  Wie Schnittstellen verfügbar gemacht.



## <a name="requirements"></a>Anforderungen

JNI, über verfügbar gemacht der [Android.Runtime.JNIEnv Namespace](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/), finden Sie in jeder Version von Xamarin.Android.
Um Java-Typen und Schnittstellen zu binden, müssen Sie Xamarin.Android 4.0 oder höher verwenden.


## <a name="managed-callable-wrappers"></a>Verwaltete Callable Wrapper

Ein **Callable Wrapper verwaltet** (**MCW**) ist eine *Bindung* für eine Java-Klasse oder Schnittstelle, die sich die alle die JNI Maschinen also diesen Client umschließt C# Code muss nicht auf der zugrunde liegenden Komplexität des JNI zu kümmern. Die meisten `Mono.Android.dll` besteht aus der verwalteten callable Wrapper.

Verwaltete callable Wrapper dienen zwei Zwecken:

1.  Kapseln Sie JNI verwenden, sodass Clientcode nicht wissen, zu der zugrunde liegenden Komplexität muss.
1.  Stellen sie untergeordnete Klasse Java-Typen und Java-Schnittstellen implementieren können.

Die erste Aufgabe ist ausschließlich für die benutzerfreundlichkeit und Kapselung von Komplexität so, dass Consumer eine einfache verwaltete Gruppe von Klassen zu verwenden. Dies erfordert die Verwendung der verschiedenen [JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/) Elemente weiter unten in diesem Artikel beschrieben. Denken Sie daran, die verwaltet callable Wrapper sind nicht unbedingt erforderlich &ndash; "Inline" JNI verwenden, ist durchaus akzeptabel und eignet sich für die einmalige Verwendung von ungebundenen Java-Elementen. Unterklasse und Schnittstelle-Implementierung erfordert die Verwendung von verwalteten callable Wrapper.



## <a name="android-callable-wrappers"></a>Android Callable Wrapper

Android callable Wrapper (Inhaltsfehler) sind erforderlich, wenn die Android-Laufzeit (ART) verwalteter Code aufgerufen muss. Diese Wrapper sind erforderlich, da es keine Möglichkeit gibt, um Klassen mit zur Laufzeit zu registrieren.
(Insbesondere die [DefineClass](http://docs.oracle.com/javase/6/docs/technotes/guides/jni/spec/functions.html#wp15986) JNI-Funktion wird von der Android-Runtime nicht unterstützt. Android callable Wrapper machen daher für den Mangel an Unterstützung für die Registrierung von Common Language Runtime Typen.)

Sobald Android-Code muss zum Ausführen von einem virtuellen oder Schnittstelle Methode, die überschrieben oder in verwaltetem Code implementiert wird, müssen Xamarin.Android, einen Java-Proxy, damit diese Methode in den entsprechenden verwalteten Typ verteilt ruft angeben. Diese Java-Proxy-Typen sind Java-Code, der die "dieselbe" Basisklasse und die Liste der Java-Schnittstelle als den verwalteten Typ so implementieren die gleichen Konstruktoren und alle außer Kraft gesetzte Basisklasse und die Schnittstellenmethoden deklariert haben.

Android callable Wrapper werden generiert, indem die **monodroid.exe** Programm während der [Buildprozess](~/android/deploy-test/building-apps/build-process.md), und werden für alle Typen, die (direkt oder indirekt) erben generiert [ Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/).



### <a name="implementing-interfaces"></a>Implementieren von Schnittstellen

Es gibt Situationen, wenn Sie möglicherweise eine Android-Schnittstelle implementieren müssen (z. B. [Android.Content.IComponentCallbacks](https://developer.xamarin.com/api/type/Android.Content.IComponentCallbacks/)).

Alle Android-Klassen und Schnittstellen erweitern die [Android.Runtime.IJavaObject](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) Schnittstelle; aus diesem Grund müssen alle Android Typen implementieren `IJavaObject`.
Xamarin.Android nutzt diese Tatsache &ndash; verwendet `IJavaObject` für einen Java-Proxy (ein Android callable Wrapper) für den angegebenen verwalteten Typ Android bereit. Da **monodroid.exe** sucht nur nach `Java.Lang.Object` Unterklassen (die implementieren müssen `IJavaObject`), Erstellen von Unterklassen `Java.Lang.Object` bietet uns eine Möglichkeit zum Implementieren von Schnittstellen in verwaltetem Code. Zum Beispiel:

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

*Im weiteren Verlauf dieses Artikels bietet Details zur Implementierung können ohne Vorankündigung geändert* (und wird nur verwendet werden, da Entwickler möglicherweise neugierig, was hinter den Kulissen passiert hier angezeigt).

Betrachten Sie die folgenden C# Quelle:

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

Die **mandroid.exe** Programm werden die folgenden Android Callable Wrapper zu generieren:

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

Beachten Sie, dass die Basisklasse wird beibehalten, und systemeigene Methodendeklarationen stehen für jede Methode, die in verwaltetem Code überschrieben wird.



### <a name="exportattribute-and-exportfieldattribute"></a>ExportAttribute und ExportFieldAttribute

Xamarin.Android generiert in der Regel automatisch den Java-Code, der der Inhaltsfehler umfasst; Diese Generation, basiert auf die Namen der Klasse und Methode, wenn eine Klasse eine Java-Klasse abgeleitet und vorhandene Java-Methoden überschreibt. Allerdings ist die codegenerierung in einigen Szenarien nicht ausreichend, wie im folgenden erläutert:

-   Android unterstützt Aktionsnamen im Layout-XML-Attribute, z. B. die [Android: OnClick](https://developer.xamarin.com/api/member/Android.Views.View+IOnClickListener.OnClick/p/Android.Views.View/) XML-Attribut. Wenn es angegeben wird, versucht die vergrößerte Ansicht-Instanz, um die Java-Methode zu suchen.

-   Die [java.io.Serializable](https://developer.android.com/reference/java/io/Serializable.html) Schnittstelle erfordert `readObject` und `writeObject` Methoden. Da es keine Member dieser Schnittstelle, macht unsere entsprechende verwaltete Implementierung dieser Methoden zu Java-Code.

-   Die [android.os.Parcelable](https://developer.xamarin.com/api/type/Android.Os.Parcelable/) Schnittstelle erwartet, dass eine Implementierungsklasse ein statisches Feld haben muss `CREATOR` des Typs `Parcelable.Creator`. Der generierte Java-Code erfordert einige explicit-Feld. Mit diesem standard Szenario besteht keine Möglichkeit, Ausgabefeld in Java-Code aus verwaltetem Code zur Verfügung.


Da Code Generation eine Lösung zum Generieren von beliebiger Java-Methoden mit willkürlichen Namen nicht bereitstellt, beginnend mit Xamarin.Android 4.2, die [ExportAttribute](https://developer.xamarin.com/api/type/Java.Interop.ExportAttribute/) und [ExportFieldAttribute](https://developer.xamarin.com/api/type/Java.Interop.ExportFieldAttribute/) wurden eingeführt, um eine Lösung für die oben genannten Szenarien zu bieten. Beide Attribute befinden sich in der `Java.Interop` Namespace:

-   `ExportAttribute` &ndash; Gibt einen Methodennamen und die Typen der erwarteten Ausnahme (zu bieten, explicit "löst" im Java). Wenn sie für eine Methode verwendet wird, wird die Methode "eine Java-Methode, die Dispatch-Code für den entsprechenden JNI-Aufruf an die verwaltete Methode generiert exportieren". Dies kann verwendet werden, mit `android:onClick` und `java.io.Serializable`.

-   `ExportFieldAttribute` &ndash; Gibt einen Feldnamen an. Es befindet sich auf eine Methode, die als ein Feldinitialisierer funktioniert. Dies kann verwendet werden, mit `android.os.Parcelable`.

Die [ExportAttribute](https://developer.xamarin.com/samples/monodroid/ExportAttribute/) Beispielprojekt veranschaulicht, wie diese Attribute verwenden.


#### <a name="troubleshooting-exportattribute-and-exportfieldattribute"></a>Problembehandlung bei ExportAttribute und ExportFieldAttribute

-   Paketerstellung tritt aufgrund eines fehlenden **Mono.Android.Export.dll** &ndash; bei Verwendung `ExportAttribute` oder `ExportFieldAttribute` auf einige Methoden in Ihrem Code oder abhängigen Bibliotheken angezeigt werden, müssen Sie  **Mono.Android.Export.dll**. Diese Assembly wird isoliert auf Rückrufcode von Java zu unterstützen. Es unterscheidet sich von **Mono.Android.dll** während zusätzlichen Speicherplatz für die Anwendung hinzugefügt.

-   In Releasebuild `MissingMethodException` tritt für Exportmethoden &ndash; im Releasebuild `MissingMethodException` für Exportmethoden auftritt. (Dieses Problem wurde in der neuesten Version von Xamarin.Android behoben.)



### <a name="exportparameterattribute"></a>ExportParameterAttribute

`ExportAttribute` und `ExportFieldAttribute` bieten Funktionen, Java-Laufzeitcode verwenden kann. Dieser Code zur Laufzeit zugreift verwalteten Code über die generierten JNI-Methoden, die von dieser Attribute gesteuert. Es gibt daher keine vorhandenen Java-Methode, die die verwaltete Methode bindet. Daher wird die Java-Methode über eine verwaltete Methodensignatur generiert.

Hier ist jedoch nicht vollständig Determinante. Am wichtigsten ist, ist dies in einigen erweiterten Zuordnungen zwischen verwalteten Typen und Java-Datentypen, z. B. "true":

-  InputStream
-  OutputStream
-  XmlPullParser
-  XmlResourceParser

Wenn für die exportierten Methoden, Typen, wie diese benötigt werden die `ExportParameterAttribute` muss explizit Geben Sie den entsprechenden Parameter oder Rückgabewert einen Typ verwendet werden.



### <a name="annotation-attribute"></a>Anmerkungsattribut

In Xamarin.Android 4.2, konvertiert es `IAnnotation` Implementierungstypen in Attribute (System.Attribute abgeleitet wurden), und added Support für die Generierung der Anmerkung im Java-Wrapper.

Dies bedeutet, dass die folgenden direktionalen Änderungen:

-   Der Generator für die Bindung generiert `Java.Lang.DeprecatedAttribute` aus `java.Lang.Deprecated` (während es sich handeln soll `[Obsolete]` in verwaltetem Code).

-   Dies bedeutet nicht, dass vorhandene `Java.Lang.Deprecated` Klasse wird verschwinden. Diese Java-basierte Objekte können immer noch als üblichen Java-Objekten verwendet werden, (sofern es sich um eine solche Nutzung vorhanden ist). Es werden `Deprecated` und `DeprecatedAttribute` Klassen.

-   Die `Java.Lang.DeprecatedAttribute` Klasse ist als markiert `[Annotation]` . Wenn es ist ein benutzerdefiniertes Attribut, die aus diesem geerbt wurde `[Annotation]` -Attribut, Msbuild-Aufgabe generiert eine Java-Anmerkung für das benutzerdefinierte Attribut (@Deprecated) in der Android Callable Wrapper (Inhaltsfehler).

-   Anmerkungen konnte generiert werden, auf Klassen, Methoden und Felder (die eine Methode in verwaltetem Code) exportiert.

Wenn die enthaltende Klasse (Klasse, die mit Anmerkungen versehene Member enthält, oder mit Anmerkungen versehenen Klasse selbst) nicht registriert ist, wird die gesamte Klassenquelle für die Java nicht überhaupt generiert auch mit den Anmerkungen. Für Methoden, Sie können angeben, die `ExportAttribute` zum Abrufen der Methode explizit generiert und mit Anmerkungen versehen. Darüber hinaus ist es kein Feature, das "die Definition einer Java-Anmerkung-Klasse generieren". Das heißt, wenn Sie ein benutzerdefiniertes verwaltetes Attribut für eine bestimmte Anmerkung definieren, müssen Sie eine andere JAR-Bibliothek hinzufügen, die die entsprechende Java Annotation-Klasse enthält. Hinzufügen einer Java-Quelldatei, die den Anmerkungstyp definiert, ist nicht ausreichend. Der Java-Compiler funktioniert nicht auf die gleiche Weise wie **apt**.

Außerdem gelten die folgenden Einschränkungen:

-   Dieser Konvertierungsprozess berücksichtigt keine `@Target` bisher Anmerkung für den Anmerkungstyp.

-   Attribute auf eine Eigenschaft ist nicht möglich. Verwenden Sie stattdessen die Attribute für Eigenschaften-Getter oder Setter.



## <a name="class-binding"></a>Klasse-Bindung

Binden einer Klasse bedeutet einen aufrufbaren Wrapper zur Vereinfachung der Aufruf von den zugrunde liegenden Java-Typ.

Binden die virtuelle und abstrakte Methoden, um zuzulassen, von überschreiben C# erfordert Xamarin.Android 4.0. Jedoch kann eine beliebige Version von Xamarin.Android nicht virtuelle Methoden, statische Methoden oder virtuelle Methoden binden, ohne Unterstützung von Außerkraftsetzungen.

Eine Bindung enthält in der Regel die folgenden Elemente:

-  Ein [JNI zu behandeln, in den Java-Typ gebunden wird](#_Looking_up_Java_Types).

-  [JNI-IDs und die Eigenschaften für jedes gebundene Feld zu Feld](#_Instance_Fields).

-  [IDs für JNI-Methode und Methoden für jede gebundene Methode](#_Instance_Methods).

-  Wenn Unterklasse erforderlich ist, wird der Typ muss, damit eine [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/) benutzerdefiniertes Attribut für die Deklaration des Typs mit [RegisterAttribute.DoNotGenerateAcw](https://developer.xamarin.com/api/property/Android.Runtime.RegisterAttribute.DoNotGenerateAcw/) festgelegt `true`.



### <a name="declaring-type-handle"></a>Deklarieren von Typhandle

Die Methoden des Felds "und"-Methode erfordert einen Objektverweis auf ihren deklarierenden Typ verweist. Gemäß der Konvention wird dies, verbleibt in einem `class_ref` Feld:

```csharp
static IntPtr class_ref = JNIEnv.FindClass(CLASS);
```

Finden Sie unter den [JNI-Typverweise](#_JNI_Type_References) Abschnitt ausführliche Informationen zu den `CLASS` token.


### <a name="binding-fields"></a>Binden von Feldern

Java-Felder werden verfügbar gemacht, als C# Eigenschaften, z. B. das Java-Feld [java.lang.System.in](https://developer.android.com/reference/java/lang/System.html#in) gebunden ist, als die C# Eigenschaft [Java.Lang.JavaSystem.In](https://developer.xamarin.com/api/property/Java.Lang.JavaSystem.In/).
Darüber hinaus da JNI zwischen Feldern in statische und Instanzfelder unterscheidet, verschiedene Methoden verwendet werden, wenn Sie die Eigenschaften zu implementieren.

Feld Bindung umfasst drei Sätze von Methoden an:

1.  Die *Feld-Id abrufen* Methode. Die *Feld-Id abrufen* Methode ist verantwortlich für die Rückgabe ein Felds zu behandeln, die die *erhalten Feldwert* und *Feldwert festgelegt* Methoden verwenden. Die Feld-Id abrufen muss bekannt sein, die deklarieren, geben Sie den Namen des Felds, und die [JNI-Typsignatur](#JNI_Type_Signatures) des Felds.

1.  Die *erhalten Feldwert* Methoden. Diese Methoden erfordern Feldhandle und sind zuständig für das Lesen der Wert des Felds aus Java.
    Die zu verwendende Methode hängt von der Typ des Felds ab.

1.  Die *Feldwert festgelegt* Methoden. Diese Methoden erfordern Feldhandle und sind zuständig für das Schreiben der Wert des Felds innerhalb von Java. Die zu verwendende Methode hängt von der Typ des Felds ab.


 [Statische Felder](#_Static_Fields) verwenden die [JNIEnv.GetStaticFieldID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticMethodID/), `JNIEnv.GetStatic*Field`, und [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/) Methoden.

 [Instanzfelder](#_Instance_Fields) verwenden die [JNIEnv.GetFieldID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetFieldID/), `JNIEnv.Get*Field`, und [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/) Methoden.

Z. B. die statische Eigenschaft `JavaSystem.In` als implementiert werden:

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

Hinweis: Verwenden wir [InputStreamInvoker.FromJniHandle](https://developer.xamarin.com/api/member/Android.Runtime.InputStreamInvoker.FromJniHandle/(System.IntPtr%2cAndroid.Runtime.JniHandleOwnership)) konvertieren die JNI-verweisen in einem `System.IO.Stream` -Instanz, und wir verwenden `JniHandleOwnership.TransferLocalRef` da [JNIEnv.GetStaticObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticObjectField/) gibt ein lokaler Verweis.

Viele der [Android.Runtime](https://developer.xamarin.com/api/namespace/Android.Runtime/) Typen `FromJniHandle` Methoden, die einer JNI konvertiert in den gewünschten Typ zu verweisen.



### <a name="method-binding"></a>Methode-Bindung

Java-Methoden werden verfügbar gemacht, als C# Methoden und als C# Eigenschaften. Zum Beispiel die Java-Methode ["java.lang.Runtime.runFinalizersOnExit"](https://developer.android.com/reference/java/lang/Runtime.html#runFinalizersOnExit(boolean)) Methode gebunden ist, als die ["java.lang.Runtime.runFinalizersOnExit"](https://developer.xamarin.com/api/member/Java.Lang.Runtime.RunFinalizersOnExit/) -Methode, und die [java.lang.Object.getClass ](https://developer.android.com/reference/java/lang/Object.html#getClass) Methode gebunden ist, als die ["java.lang.Object.Class"](https://developer.xamarin.com/api/property/Java.Lang.Object.Class/) Eigenschaft.

Methodenaufruf ist ein zweistufiger Prozess:

1.  Die *Methoden-Id abrufen* für die aufzurufende Methode. Die *Methoden-Id abrufen* Methode ist verantwortlich für die Rückgabe einer Methodenhandle, die Methoden der Methode verwendet werden. Die Methoden-Id abrufen muss bekannt sein, die deklarieren, geben Sie den Namen der Methode, und die [JNI-Typsignatur](#JNI_Type_Signatures) der Methode.

1.  Rufen Sie die Methode auf.

Unterscheiden sich genau wie bei Feldern, die Methoden verwenden, um die Methoden-Id zu erhalten, und rufen die Methode zwischen statischen Methoden und Instanzmethoden.

[Statische Methoden](#_Static_Methods_1) verwenden [JNIEnv.GetStaticMethodID()](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticMethodID/) die Methoden-Id zu suchen und Verwenden der `JNIEnv.CallStatic*Method` -Methodenfamilie für Aufruf.

[Instanzmethoden](#_Instance_Methods) verwenden [JNIEnv.GetMethodID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetMethodID/) die Methoden-Id zu suchen und Verwenden der `JNIEnv.Call*Method` und `JNIEnv.CallNonvirtual*Method` -Serie von Methoden für den Aufruf.

Bindung der Methode ist möglicherweise mehr als nur-Methodenaufruf. Methode Bindung außerdem umfasst es erlaubt, eine Methode, um (für abstrakte und nicht endgültige-Methoden) überschrieben werden, oder (für die Schnittstellenmethoden) implementiert. Die [Unterstützung von Vererbung, Schnittstellen](#_Supporting_Inheritance,_Interfaces_1) Abschnitt wird beschrieben, die Komplexität der Unterstützung von virtuellen Methoden und die Schnittstellenmethoden.

<a name="_Static_Methods_1" />

#### <a name="static-methods"></a>Statische Methoden

Binden eine statische Methode umfasst die Verwendung `JNIEnv.GetStaticMethodID` zum Abrufen eines Methode-Handles, und klicken Sie dann mit der entsprechenden `JNIEnv.CallStatic*Method` -Methode, abhängig vom Rückgabetyp der Methode. Folgendes ist ein Beispiel für eine Bindung für die [Runtime.getRuntime](https://developer.android.com/reference/java/lang/Runtime.html#getRuntime()) Methode:

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

Beachten Sie, dass wir in einem statischen Feld, das Methodenhandle speichern `id_getRuntime`. Dies ist eine leistungsoptimierung, damit die Methodenhandle muss nicht bei jedem Aufruf gesucht werden soll. Es ist nicht erforderlich, um das Methodenhandle auf diese Weise zwischenzuspeichern. Sobald das Methodenhandle abgerufen wurde, [JNIEnv.CallStaticObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticObjectMethod/) wird verwendet, um die Methode aufrufen. `JNIEnv.CallStaticObjectMethod` Gibt eine `IntPtr` das Handle des zurückgegebenen Java-Instanz enthält.
[Java.Lang.Object.GetObject&lt;T&gt;(IntPtr, JniHandleOwnership)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) wird verwendet, um das Java-Handle in eine Instanz der stark typisiertes Objekt zu konvertieren.



#### <a name="non-virtual-instance-method-binding"></a>Bindung für nicht virtuelle Instanz-Methode

Binden einer `final` umfasst die Instanzmethode oder Instanzmethode die außer Kraft gesetzt, erfordern keine Verwendung `JNIEnv.GetMethodID` zum Abrufen eines Methode-Handles, und klicken Sie dann mit der entsprechenden `JNIEnv.Call*Method` -Methode, abhängig vom Rückgabetyp der Methode. Folgendes ist ein Beispiel für eine Bindung für die `Object.Class` Eigenschaft:

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

Beachten Sie, dass wir in einem statischen Feld, das Methodenhandle speichern `id_getClass`.
Dies ist eine leistungsoptimierung, damit die Methodenhandle muss nicht bei jedem Aufruf gesucht werden soll. Es ist nicht erforderlich, um das Methodenhandle auf diese Weise zwischenzuspeichern. Sobald das Methodenhandle abgerufen wurde, [JNIEnv.CallStaticObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticObjectMethod/) wird verwendet, um die Methode aufrufen. `JNIEnv.CallStaticObjectMethod` Gibt eine `IntPtr` das Handle des zurückgegebenen Java-Instanz enthält.
[Java.Lang.Object.GetObject&lt;T&gt;(IntPtr, JniHandleOwnership)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) wird verwendet, um das Java-Handle in eine Instanz der stark typisiertes Objekt zu konvertieren.


### <a name="binding-constructors"></a>Binden von Konstruktoren

Konstruktoren sind Java-Methoden, mit dem Namen `"<init>"`. Genau wie bei Java-Instanzenmethoden, `JNIEnv.GetMethodID` wird verwendet, um das Handle Konstruktor zu suchen. Im Gegensatz zu Java-Methoden die [JNIEnv.NewObject](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.NewObject/) Methoden werden verwendet, um das Handle der Konstruktor-Methode aufrufen. Der Rückgabewert von `JNIEnv.NewObject` wird von einem lokalen JNI-verweisen:


```csharp
int value = 42;
IntPtr class_ref    = JNIEnv.FindClass ("java/lang/Integer");
IntPtr id_ctor_I    = JNIEnv.GetMethodID (class_ref, "<init>", "(I)V");
IntPtr lrefInstance = JNIEnv.NewObject (class_ref, id_ctor_I, new JValue (value));
// Dispose of lrefInstance, class_ref…
```

Normalerweise wird eine Bindung für die Klasse Unterklasse [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/).
Beim Erstellen von Unterklassen für `Java.Lang.Object`, wird eine zusätzliche semantische ins Spiel: eine `Java.Lang.Object` -Instanz verwaltet einen globalen Verweis auf eine Java-Instanz über die `Java.Lang.Object.Handle` Eigenschaft.

1.  Die `Java.Lang.Object` Standardkonstruktor wird ordnen Sie eine Java-Instanz.

1.  Wenn der Typ hat einen `RegisterAttribute` , und `RegisterAttribute.DoNotGenerateAcw` ist `true` , klicken Sie dann eine Instanz von der `RegisterAttribute.Name` Typ mit seinem Standardkonstruktor erstellt wurde.

1.  Andernfalls die [Android Callable Wrapper](~/android/platform/java-integration/android-callable-wrappers.md) (Inhaltsfehler), entspricht `this.GetType` über den Standardkonstruktor instanziiert wird. Android Callable Wrapper generiert werden, während der paketerstellung für jede `Java.Lang.Object` -Unterklasse für die `RegisterAttribute.DoNotGenerateAcw` ist nicht festgelegt, um `true`.

Für Typen sind keine Bindungen Klasse, dies ist der erwartete semantische: Instanziieren einer `Mono.Samples.HelloWorld.HelloAndroid` C# erstellen-Instanz sollte eine Java `mono.samples.helloworld.HelloAndroid` -Instanz, die einen generierten Android Callable Wrapper ist.

Für Klasse Bindungen kann dies das richtige Verhalten sein, wenn es sich bei den Java-Typ enthält einen standardmäßigen Konstruktor bzw. keine anderen Konstruktor muss aufgerufen werden. Andernfalls muss ein Konstruktor angegeben werden, das die folgenden Aktionen ausführt:

1.  Aufrufen der [Java.Lang.Object (IntPtr, JniHandleOwnership)](https://developer.xamarin.com/api/constructor/Java.Lang.Object.Object/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) anstelle des standardmäßigen `Java.Lang.Object` Konstruktor. Dies ist erforderlich, um zu vermeiden, Erstellen einer neuen Java-Instanz.

1.  Überprüfen Sie den Wert der [Java.Lang.Object.Handle](https://developer.xamarin.com/api/property/Java.Lang.Object.Handle/) vor dem Erstellen von jeder Java-Instanzen. Die `Object.Handle` Eigenschaft hat einen Wert als `IntPtr.Zero` Wenn ein Android Callable Wrapper in Java-Code erstellt wurde, und die Bindung für die Klasse enthält die erstellte Android Callable Wrapper-Instanz erstellt wird. Erstellt z. B. wenn Android eine `mono.samples.helloworld.HelloAndroid` Instanz, die Android Callable Wrapper erstellt werden zuerst und anschließend die Java `HelloAndroid` Konstruktor erstellt eine Instanz des entsprechenden `Mono.Samples.HelloWorld.HelloAndroid` Typ, mit der `Object.Handle` -Eigenschaft Legen Sie auf die Java-Instanz vor der Ausführung der Konstruktor.

1.  Ist der aktuelle Laufzeittyp nicht identisch mit den deklarierenden Typ, wird eine Instanz des zugehörigen Android Callable Wrapper muss erstellt werden, und verwenden Sie [Object.SetHandle](https://developer.xamarin.com/api/member/Java.Lang.Object.SetHandle/(System.IntPtr%2cAndroid.Runtime.JniHandleOwnership)) zum Speichern des von zurückgegebenen Handles [ JNIEnv.CreateInstance](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CreateInstance/).

1.  Wenn der aktuelle Laufzeittyp identisch mit dem deklarierenden Typ ist, klicken Sie dann den Java-Konstruktor aufrufen und verwenden Sie [Object.SetHandle](https://developer.xamarin.com/api/member/Java.Lang.Object.SetHandle/(System.IntPtr%2cAndroid.Runtime.JniHandleOwnership)) zum Speichern des von zurückgegebenen Handles `JNIEnv.NewInstance` .


Betrachten Sie beispielsweise die [java.lang.Integer(int)](https://developer.android.com/reference/java/lang/Integer.html#Integer(int)) Konstruktor. Dies wird als gebunden:

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

Erstellen von Unterklassen für einen Java-Typ oder eine Java-Schnittstelle zu implementieren, erfordert die Generierung [Android Callable Wrapper](~/android/platform/java-integration/android-callable-wrappers.md) (ACWs), die für generiert werden, sind alle `Java.Lang.Object` Unterklasse während des Paketerstellungsprozesses. Inhaltsfehler-Generierung wird gesteuert, durch die [Android.Runtime.RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/) benutzerdefinierten Attributs.

Für C# Typen, die `[Register]` benutzerdefinierten Attribut-Konstruktor erfordert ein Argument: der [JNI vereinfacht Typverweis](#_Simplified_Type_References_1) für die entsprechende Java geben. Dadurch geben Sie unterschiedliche Namen zwischen Java und C#.

Vor dem Xamarin.Android 4.0 die `[Register]` benutzerdefiniertes Attribut wurde für "alias" vorhandenen Java-Typen nicht verfügbar sind. Dies ist, da Sie der Generierungsprozess der Inhaltsfehler ACWs für würde jeder `Java.Lang.Object` Unterklasse gefunden.

Xamarin.Android 4.0 eingeführt, die [RegisterAttribute.DoNotGenerateAcw](https://developer.xamarin.com/api/property/Android.Runtime.RegisterAttribute.DoNotGenerateAcw/) Eigenschaft. Diese Eigenschaft weist den Generierungsprozess Inhaltsfehler *überspringen* dem mit Anmerkungen versehene Typ ermöglicht die Deklaration von neuen verwalteten Callable Wrapper, die nicht ACWs generiert werden, bei der Erstellung des Pakets führt. Dadurch wird die Bindung des vorhandene Java-Typen. Betrachten Sie beispielsweise die folgende einfache Java-Klasse, `Adder`, die eine Methode enthält `add`, fügt zu einer ganzen Zahl und gibt das Ergebnis zurück:

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

Hier ist die `Adder` C# Typ *Aliase* der `Adder` Java-Typ. Die `[Register]` Attribut wird verwendet, um den JNI-Namen des angeben der `mono.android.test.Adder` Java-Datentyp, und die `DoNotGenerateAcw` Eigenschaft wird verwendet, um Inhaltsfehler Generation behindern. Dies führt zu einer Inhaltsfehler für die Generierung der `ManagedAdder` eingeben, die ordnungsgemäß Unterklassen der `mono.android.test.Adder` Typ. Wenn die `RegisterAttribute.DoNotGenerateAcw` Eigenschaft noch nicht verwendet wurde, und klicken Sie dann die Xamarin.Android-Buildprozess wäre, generiert einen neuen `mono.android.test.Adder` Java-Typ. Führt dies zu Kompilierungsfehlern, als die `mono.android.test.Adder` Typ wird zweimal in zwei separaten Dateien vorhanden sein.



### <a name="binding-virtual-methods"></a>Binden virtuelle Methoden

`ManagedAdder` Unterklassen der Java `Adder` Typ, aber es ist nicht besonders interessant: die C# `Adder` Typ nicht keine virtuellen Methoden definieren, also `ManagedAdder` alles kann nicht überschrieben werden.

Binden von `virtual` Methoden zum Zulassen von Unterklassen überschreiben erfordert einige Dinge, die ausgeführt werden, die in den folgenden beiden Kategorien fallen:

1.  **Methode-Bindung**

1.  **Registrieren**


#### <a name="method-binding"></a>Methode-Bindung

Eine Methode-Bindung erfordert das Hinzufügen von zwei Elementen, Unterstützung auf die C# `Adder` Definition: `ThresholdType`, und `ThresholdClass`.

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

`ThresholdType` wird in der Methode binden verwendet, um zu bestimmen, wann der virtuelle oder Dispatch von nicht virtuellen Methoden ausgeführt werden muss. Sie sollten immer zurück. eine `System.Type` -Instanz, die den deklarierenden entspricht C# Typ.

##### <a name="thresholdclass"></a>ThresholdClass

Die `ThresholdClass` Eigenschaft gibt die JNI-Klassenreferenz, für den gebundenen Typ zurück:

```csharp
partial class Adder {
    protected override IntPtr ThresholdClass {
        get {
            return class_ref;
        }
    }
}
```

`ThresholdClass` wird beim Aufrufen von nicht virtuellen Methoden in der Bindung der Methode verwendet.

#### <a name="binding-implementation"></a>Implementierung der Bindung

Die methodenimplementierung der Bindung ist verantwortlich für die Common Language Runtime-Aufruf, der die Java-Methode. Es enthält auch eine `[Register]` Deklaration des benutzerdefinierten Attributs, die Teil der Registrierung für die Methode und wird im Abschnitt registrieren erläutert werden:

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

Die `id_add` Feld enthält die Methoden-ID für die Java-Methode aufrufen. Die `id_add` Wert aus einer `JNIEnv.GetMethodID`, erfordert die deklarierende Klasse (`class_ref`), den Namen der Java-Methode (`"add"`), und die JNI-Signatur der Methode (`"(II)I"`).

Sobald die Methoden-ID abgerufen wurde, `GetType` im Vergleich zu `ThresholdType` zu bestimmen, ob virtuelle oder nicht virtueller Dispatch erforderlich ist. Virtueller Dispatch ist erforderlich, wenn `GetType` entspricht `ThresholdType`, als `Handle` bezieht sich möglicherweise auf eine Java-zugeordneten-Unterklasse, die die Methode überschrieben wird.

Wenn `GetType` stimmt nicht überein `ThresholdType`, `Adder` hat in Unterklassen unterteilt wurden (z. B. durch `ManagedAdder`), und die `Adder.Add` Implementierung wird nur aufgerufen werden, wenn die Unterklasse aufgerufen `base.Add`. Dies ist der Fall nicht virtueller Dispatch, wofür `ThresholdClass` ins Spiel. `ThresholdClass` Gibt an, die Java-Klasse die Implementierung der aufzurufenden Methode bereitstellt.



#### <a name="method-registration"></a>Registrieren

Angenommen, wir haben ein aktualisiertes `ManagedAdder` Definition die außer Kraft setzt die `Adder.Add` Methode:

```csharp
partial class ManagedAdder : Adder {
    public override int Add (int a, int b) {
        return (a*2) + (b*2);
    }
}
```

Zur Erinnerung: `Adder.Add` hatte eine `[Register]` benutzerdefiniertes Attribut:

```csharp
[Register ("add", "(II)I", "GetAddHandler")]
```

Die `[Register]` Konstruktor des benutzerdefinierten Attributs lässt drei Werte:

1.  Der Name der Methode Java `"add"` in diesem Fall.

1.  Die JNI-Typ-Signatur der Methode `"(II)I"` in diesem Fall.

1.  Die *Connector Methode* , `GetAddHandler` in diesem Fall.
    Connector-Methoden werden weiter unten erläutert.


Die ersten beiden Parameter können den Generierungsprozess Inhaltsfehler, um die Deklaration einer Methode zum Überschreiben der Methode zu generieren. Die resultierende Inhaltsfehler enthält einen Teil des folgenden Codes:

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

Beachten Sie, dass ein `@Override` Methode deklariert ist, wird der delegiert an eine `n_`-Methode mit dem gleichen Namen vorangestellt. Diese stellen sicher, dass bei der Java-Code ruft `ManagedAdder.add`, `ManagedAdder.n_add` wird aufgerufen, sodass das Überschreiben C# `ManagedAdder.Add` auszuführende Methode.

Daher die wichtigste Frage: wie ist `ManagedAdder.n_add` bis zu verknüpft `ManagedAdder.Add`?

Java `native` Methoden werden registriert, mit der Runtime Java (der Android-Laufzeit) über die [JNI RegisterNatives Funktion](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp17734).
`RegisterNatives` akzeptiert ein Array von Strukturen, die Namen der Java-Methode, die Signatur der JNI-Typ und einen Funktionszeiger aufrufen, das folgt [JNI-Aufrufkonvention](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/design.html#wp715).
Der Funktionszeiger muss es sich um eine Funktion sein, die zwei Zeigerargumente gefolgt von der Methodenparameter akzeptiert. Die Java `ManagedAdder.n_add` Methode muss über eine Funktion mit dem folgenden C-Prototyp implementiert werden:

```csharp
int FunctionName(JNIEnv *env, jobject this, int a, int b)
```

Xamarin.Android macht eine `RegisterNatives` Methode. Stattdessen der Inhaltsfehler und die MCW zusammen bieten die notwendigen Informationen zum Aufrufen `RegisterNatives`: der Inhaltsfehler enthält den Methodennamen und die Signatur der JNI-Typ, der fehlt nur noch einen Funktionszeiger zu verknüpfen.

Hier kommt die *Connector Methode* ins Spiel. Die dritte `[Register]` benutzerdefinierten Attribut-Parameter ist der Name einer Methode, die definiert, die in den registrierten Typ oder eine Basisklasse des registrierten Typs, der keine Parameter akzeptiert und gibt eine `System.Delegate`. Das zurückgegebene `System.Delegate` wiederum verweist auf eine Methode mit der richtigen Signatur der JNI-Funktion. Schließlich der Delegat, der der Connector Methodenrückgabe *müssen* werden wurden die nutzungsbeschränkungen entfernt, damit nicht der Garbage Collector erfasst, wie der Delegat auf Java bereitgestellt wird.

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

Die `GetAddHandler` -Methode erstellt eine `Func<IntPtr, IntPtr, int, int,
int>` Delegaten bezieht sich auf die `n_Add` -Methode ruft dann [JNINativeWrapper.CreateDelegate](https://developer.xamarin.com/api/member/Android.Runtime.JNINativeWrapper.CreateDelegate/).
`JNINativeWrapper.CreateDelegate` Dient als Wrapper für die angegebene Methode in einem Try/Catch-Block, damit nicht behandelten Ausnahmen behandelt werden, und bei, dass führt die [AndroidEvent.UnhandledExceptionRaiser](https://developer.xamarin.com/api/event/Android.Runtime.AndroidEnvironment.UnhandledExceptionRaiser/) Ereignis. Der resultierende Delegat befindet sich in der statischen `cb_add` Variablen, damit der Garbage Collector den Delegaten nicht freigegeben werden.

Zum Schluss die `n_Add` Methode ist verantwortlich für das Marshalling der JNI-Parameter für den entsprechenden verwalteten Typen, delegieren die Methode aufrufen.

Hinweis: Verwenden Sie immer `JniHandleOwnership.DoNotTransfer` beim Abrufen einer MCW über eine Java-Instanz. Behandeln sie als lokaler Verweis (und somit Aufrufen `JNIEnv.DeleteLocalRef`) unterbricht verwaltet:&gt; Java –&gt; Stack Übergänge verwaltet.



### <a name="complete-adder-binding"></a>Führen Sie die Adder-Bindung

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

Wenn Sie einen Typ zu schreiben, die die folgenden Kriterien entspricht:

1.  Unterklassen `Java.Lang.Object`

1.  Verfügt über eine `[Register]` benutzerdefiniertes Attribut

1.  `RegisterAttribute.DoNotGenerateAcw` ist gleich `true`.


Klicken Sie dann für die GC-Interaktion der Typ *darf nicht* haben alle Felder, die auf verweisen möglicherweise eine `Java.Lang.Object` oder `Java.Lang.Object` Unterklasse zur Laufzeit. Z. B. die Felder des Typs `System.Object` und einen beliebigen anderen Schnittstellentyp sind nicht zulässig. Typen, die nicht auf verweisen können `Java.Lang.Object` Instanzen zulässig sind, z. B. `System.String` und `List<int>`. Diese Einschränkung ist um vorzeitige objektauflistung, die von der GC zu verhindern.

Wenn der Typ ein Instanzenfeld enthalten muss, die auf verweisen kann eine `Java.Lang.Object` -Instanz, und klicken Sie dann der Feldtyp sein muss `System.WeakReference` oder `GCHandle`.



## <a name="binding-abstract-methods"></a>Binden die abstrakte Methoden

Binden von `abstract` Methoden entspricht weitgehend dem virtuelle Methoden zu binden. Es gibt nur zwei Unterschiede:

1.  Die abstrakte Methode ist abstrakt. Weiterhin wurden die `[Register]` -Attribut und den zugehörigen-Methode, die Bindung der Methode wird nur in verschoben der `Invoker` Typ.

1.  Nicht `abstract` `Invoker` Typ erstellt, die den abstrakten Typ. Die `Invoker` Typ muss alle abstrakte Methoden, die in der Basisklasse deklariert überschreiben und die außer Kraft gesetzte Implementierung ist die Methode binden-Implementierung, die nicht virtuelle Dispatch Groß-/Kleinschreibung ignoriert werden kann.


Nehmen wir beispielsweise an, die den oben genannten `mono.android.test.Adder.add` Methode `abstract`. Die C# Bindung ändern, damit `Adder.Add` wurden abstrakte verwendet werden soll, und ein neues `AdderInvoker` Typ würde definiert werden, die implementiert `Adder.Add`:

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

Die `Invoker` Typ ist nur erforderlich, wenn JNI-verweisen auf Java erstellte Instanzen zu erhalten.


## <a name="binding-interfaces"></a>Binden von Schnittstellen

Binden von Schnittstellen ist konzeptionell identisch mit der Bindung von Klassen, die virtuelle Methoden enthalten, die viele Einzelheiten in kleinen (und nicht so kleine) Methoden unterscheiden sich jedoch. Beachten Sie Folgendes [Java Schnittstellendeklaration](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/Adder.java#L14):

```csharp
public interface Progress {
    void onAdd(int[] values, int currentIndex, int currentSum);
}
```

Schnittstelle Bindungen bestehen aus zwei Teilen: dem C# Schnittstellendefinition und eine Definition aufrufende Instanz für die Schnittstelle.



### <a name="interface-definition"></a>Schnittstellendefinition

Die C# Schnittstellendefinition muss die folgenden Anforderungen erfüllen:

-   Die Schnittstellendefinition benötigen eine `[Register]` benutzerdefinierten Attributs.

-   Die Schnittstellendefinition muss erweitern die `IJavaObject interface`.
    Bei unterlassen wird verhindert, dass ACWs erben von der Java-Schnittstelle.

-   Jede Schnittstellenmethode darf eine `[Register]` Attribut, in dem entsprechenden Namen der Java-Methode, die JNI-Signatur und die Connector-Methode.

-   Die Connector-Methode muss auch den Typ angeben, dem für die Connector-Methode gefunden werden kann.

Beim Binden von `abstract` und `virtual` Methoden, die Connector-Methode würde durchsucht werden, in der Vererbungshierarchie des Typs, die registriert wird. Schnittstellen können keine Methoden,-bodied-Elemente enthält, damit dies funktioniert nicht, daher die Anforderung haben, die ein Typ angegeben werden, der angibt, auf dem sich die Connector-Methode befindet. Der Typ wird in der Zeichenfolge für den Connector-Methode, nach einem Doppelpunkt angegeben `':'`, muss die Assembly qualifizierte Typname des Typs mit der aufrufenden Instanz.

Schnittstellendeklarationen-Methode gibt eine Verschiebung um die entsprechende Java-Methode, mit *kompatibel* Typen. Für "Vordefiniert" Java-Datentypen, die kompatiblen Typen sind die entsprechenden C# Typen, z. B. Java `int` ist C# `int`. Für Verweistypen ist die kompatible Typ ein Typ, der ein JNI-Handle des entsprechenden Typs Java bereitstellen kann.

Die Schnittstellen-Member werden nicht direkt aufgerufen werden durch Java &ndash; Aufruf wird durch den Typ der aufrufenden Instanz vermittelt werden &ndash; damit ein gewisses Maß an Flexibilität zulässig ist.

Die Java-Statusschnittstelle möglich [in deklarierten C# als](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L83):

```csharp
[Register ("mono/android/test/Adder$Progress", DoNotGenerateAcw=true)]
public interface IAdderProgress : IJavaObject {
    [Register ("onAdd", "([III)V",
            "GetOnAddHandler:Mono.Samples.SanityTests.IAdderProgressInvoker, SanityTests, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")]
    void OnAdd (JavaArray<int> values, int currentIndex, int currentSum);
}
```

Beachten Sie, dass die oben genannten, dass wir die Java zuordnen `int[]` Parameter, um eine [JavaArray&lt;Int&gt;](https://developer.xamarin.com/api/type/Android.Runtime.JavaArray%601/).
Ist dies nicht erforderlich: Wir können gebunden wurde, eine C# `int[]`, oder ein `IList<int>`, oder etwas anderes vollständig. Unabhängig Typ ausgewählt wird, die `Invoker` muss es in einer Java übersetzen können `int[]` Typ für den Aufruf.


### <a name="invoker-definition"></a>Aufrufer-Definition

Die `Invoker` Typdefinition muss erben `Java.Lang.Object`, die entsprechende Schnittstelle implementieren und geben Sie alle Verbindungsmethoden, die auf die in der Schnittstellendefinition. Es gibt weitere empfohlen, die von einer Klasse Bindung abweicht: die `class_ref` Feld und Methode IDs muss ein Instanzmember, nicht statische Member.

Der Grund für die Bevorzugung von Instanzmembern besteht in `JNIEnv.GetMethodID` Verhalten in der Android-Laufzeit. (Dies ist möglicherweise auch die Java-Verhalten, es noch nicht getestet wurde.) `JNIEnv.GetMethodID` gibt null zurück, wenn eine Methode suchen, die aus einer implementierten Schnittstelle und nicht der deklarierte Schnittstelle stammen. Betrachten Sie die ["java.util.SortedMap"&lt;K, V&gt; ](https://developer.android.com/reference/java/util/SortedMap.html) Java-Schnittstelle, die implementiert die [java.util.Map&lt;K, V&gt; ](https://developer.android.com/reference/java/util/Map.html) Schnittstelle. Map enthält eine [löschen](https://developer.android.com/reference/java/util/Map.html#clear()) -Methode, also eine scheinbar berechtigte `Invoker` Definition für SortedMap wäre:

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

Die oben genannten schlägt fehl, da `JNIEnv.GetMethodID` zurück `null` bei der Suche nach der `Map.clear` Methode über die `SortedMap` Klasseninstanz.

Es gibt zwei Lösungen dafür: Nachverfolgen der Schnittstelle, die jede Methode stammt, und eine `class_ref` für jede Schnittstelle, oder behalten Sie alles, was als Instanzmember, und führen Sie die Methodensuche auf den am stärksten abgeleiteten Klassentyp, nicht den Schnittstellentyp. Letztere erfolgt im **Mono.Android.dll**.

Die Definition der aufrufenden Instanz enthält sechs Abschnitte: den Konstruktor der `Dispose` -Methode, die `ThresholdType` und `ThresholdClass` Member, die `GetObject` Methode schnittstellenimplementierung-Methode und die Connector-methodenimplementierung.



#### <a name="constructor"></a>Konstruktor

Der Konstruktor muss die Laufzeitklasse der aufgerufenen Instanz zu suchen, und speichern die Common Language Runtime-Klasse in der Instanz `class_ref` Feld:

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

Hinweis: Die `Handle` -Eigenschaft muss in den Text des Konstruktors verwendet werden und nicht die `handle` -Parameter, beispielsweise auf Android v4. 0 die `handle` Parameter ist möglicherweise ungültig, nach dem Basiskonstruktor ausgeführt worden ist.


#### <a name="dispose-method"></a>Dispose-Methode

Die `Dispose` -Methode muss den globalen Verweis zugeordnet, die im Konstruktor freizugeben:

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

Die `ThresholdType` und `ThresholdClass` Elemente sind identisch mit was in einer Klasse Bindung gefunden wurde:

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

Jede Methode der Schnittstelle benötigt eine Implementierung, die die entsprechende Java-Methode über JNI aufruft:

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

Die Connector-Methoden und die unterstützende Infrastruktur sind verantwortlich für das Marshalling der JNI-Parameter an entsprechende C# Typen. Der Java `int[]` Parameter werden als eine JNI übergeben `jintArray`, d.h. eine `IntPtr` in C#. Die `IntPtr` gemarshallt werden muss, um eine `JavaArray<int>` zur Unterstützung von Aufrufen der C# Schnittstelle:

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

Wenn `int[]` wäre vorzuziehen `JavaList<int>`, klicken Sie dann [JNIEnv.GetArray()](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetArray/(System.IntPtr%2cAndroid.Runtime.JniHandleOwnership%2cSystem.Type)) kann stattdessen verwendet werden:

```csharp
int[] _values = (int[]) JNIEnv.GetArray(values, JniHandleOwnership.DoNotTransfer, typeof (int));
```

Beachten Sie jedoch, `JNIEnv.GetArray` kopiert das gesamte Array zwischen virtuellen Computern, sodass bei großen Arrays dies viele zusätzliche GC-Auslastung führen konnte.


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

Viele JNIEnv-Methoden zurückgeben *JNI* *Objektverweise*, die ähneln `GCHandle`s. JNI bietet drei verschiedene Typen von Objektverweisen: lokale Verweise, globalen Verweise und globale weak-Verweise. Alle drei werden als dargestellt `System.IntPtr`, *aber* (gemäß Abschnitt JNI-Funktionstypen) nicht alle `IntPtr`aus zurückgegebenen `JNIEnv` Methoden sind Verweise. Z. B. [JNIEnv.GetMethodID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetMethodID/) gibt ein `IntPtr`, er wird nicht zurückgegeben, einen Objektverweis, gibt jedoch eine `jmethodID`. Wenden Sie sich an den [JNI-Funktion Dokumentation](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html) Details.

Lokale Verweise werden erstellt, indem *die meisten* Methoden zum Erstellen von Verweis.
Android lässt nur eine begrenzte Anzahl von lokalen Verweisen zu jedem Zeitpunkt in der Regel 512 vorhanden sein. Lokale Verweise gelöscht werden können, über [JNIEnv.DeleteLocalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteLocalRef/).
Im Gegensatz zu JNI Verweis nicht alle JNIEnv-Methoden, die das Rückgabeobjekt auf lokale Verweise zurückgeben; [JNIEnv.FindClass](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.FindClass/) gibt eine *globale* Verweis. Es wird dringend empfohlen, dass Sie lokale Verweise, so schnell löschen wie, möglicherweise durch das Erstellen einer [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) um das Objekt und geben Sie `JniHandleOwnership.TransferLocalRef` auf die [Java.Lang.Object (IntPtr verarbeiten, JniHandleOwnership Übertragung)](https://developer.xamarin.com/api/constructor/Java.Lang.Object.Object/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) Konstruktor.

Globale Verweise werden erstellt, indem [JNIEnv.NewGlobalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.NewGlobalRef/) und [JNIEnv.FindClass](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.FindClass/).
Sie können mit zerstört werden [JNIEnv.DeleteGlobalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteGlobalRef/).
Emulatoren verfügen über ein Limit von 2.000 ausstehenden globale Verweise Hardwaregeräte eine Beschränkung von ca. 52.000 globalen Verweise.

Globale Weak-Verweise sind nur verfügbar, unter Android Version 2.2 (Froyo) und höher. Globale Weak-Verweise können gelöscht werden, mit [JNIEnv.DeleteWeakGlobalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteWeakGlobalRef/(System.IntPtr)).


### <a name="dealing-with-jni-local-references"></a>Umgang mit lokalen JNI-verweisen

Die [JNIEnv.GetObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetObjectField/), [JNIEnv.GetStaticObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticObjectField/), [JNIEnv.CallObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallObjectMethod/), [JNIEnv.CallNonvirtualObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualObjectMethod/)und [JNIEnv.CallStaticObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticObjectMethod/) Methoden zurückgeben einer `IntPtr` enthält einen lokalen JNI-verweisen auf ein Java-Objekt, oder `IntPtr.Zero` Java zurückgegeben `null`. Aufgrund der begrenzten Anzahl von lokalen verweisen, die auf ausstehende sein können, nachdem (512 Einträge), um sicherzustellen, dass die Verweise werden rechtzeitig gelöscht. Es gibt drei Möglichkeiten, die mit lokale verweisen verwendet werden können: explizit gelöscht werden, erstellen eine `Java.Lang.Object` Instanz zum Speichern und einen mit `Java.Lang.Object.GetObject<T>()` um einen verwalteten callable Wrapper um diese zu erstellen.



### <a name="explicitly-deleting-local-references"></a>Explizit löschen von lokalen verweisen

[JNIEnv.DeleteLocalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteLocalRef/) dient zum Löschen von lokaler verweisen. Nach lokale Verweis gelöscht wurde, nicht verwendet werden nicht mehr so sorgfältig vorgegangen werden, um sicherzustellen, `JNIEnv.DeleteLocalRef` wird als letztes ausgeführt werden, mit dem lokalen Verweis.

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
try {
    // Do something with `lref`
}
finally {
    JNIEnv.DeleteLocalRef (lref);
}
```



### <a name="wrapping-with-javalangobject"></a>Umbrechen mit Java.Lang.Object

`Java.Lang.Object` Stellt eine [Java.Lang.Object (IntPtr-Handles, JniHandleOwnership Übertragung)](https://developer.xamarin.com/api/constructor/Java.Lang.Object.Object/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) Konstruktor, der zum Umschließen einer vorhandenen JNI-verweisen verwendet werden kann. Die [JniHandleOwnership](https://developer.xamarin.com/api/type/Android.Runtime.JniHandleOwnership/) Parameter bestimmt, wie die `IntPtr` Parameter behandelt werden soll:

-   [JniHandleOwnership.DoNotTransfer](https://developer.xamarin.com/api/field/Android.Runtime.JniHandleOwnership.DoNotTransfer/) &ndash; das erstellte `Java.Lang.Object` Instanz erstellt einen neuen globalen Verweis aus der `handle` -Parameter und `handle` bleibt unverändert.
    Der Aufrufer ist dafür verantwortlich, Freigeben von `handle` , falls erforderlich.

-   [JniHandleOwnership.TransferLocalRef](https://developer.xamarin.com/api/field/Android.Runtime.JniHandleOwnership.TransferLocalRef/) &ndash; das erstellte `Java.Lang.Object` Instanz erstellt einen neuen globalen Verweis aus der `handle` -Parameter und `handle` gelöscht wird, mit [JNIEnv.DeleteLocalRef ](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteLocalRef/) . Der Aufrufer muss nicht freigeben `handle` , und darf kein verwenden `handle` nach der Konstruktor ausgeführt worden ist.

-   [JniHandleOwnership.TransferGlobalRef](https://developer.xamarin.com/api/field/Android.Runtime.JniHandleOwnership.TransferLocalRef/) &ndash; das erstellte `Java.Lang.Object` Instanz übernimmt den Besitz der `handle` Parameter. Der Aufrufer muss nicht freigeben `handle` .


Da die JNI-Methode aufrufen Methoden lokalen Refs zurückgeben `JniHandleOwnership.TransferLocalRef` wird normalerweise verwendet werden:

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef);
```

Der erstellte globale Verweis wird nicht reserviert, bis die `Java.Lang.Object` Instanz ist die Garbage collection. Wenn Sie der Lage sind, wird Freigeben von der Instanz der globalen Verweis, Garbage collection beschleunigt freizugeben:

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
using (var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef)) {
    // use value ...
}
```



### <a name="using-javalangobjectgetobjectlttgt"></a>Mithilfe von Java.Lang.Object.GetObject&lt;T&gt;)

`Java.Lang.Object` Stellt eine [Java.Lang.Object.GetObject&lt;T&gt;(IntPtr-Handles, JniHandleOwnership Übertragung)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) -Methode, die zum Erstellen eines verwalteten callable Wrappers des angegebenen Typs verwendet werden kann.

Der Typ `T` müssen die folgenden Anforderungen erfüllen:

1.  `T` ein Verweistyp muss sein.

1.  `T` Implementieren Sie müssen die `IJavaObject` Schnittstelle.

1.  Wenn `T` ist keiner abstrakten Klasse oder Schnittstelle, klicken Sie dann `T` muss einen Konstruktor bereitstellen, mit den Parametertypen `(IntPtr,
    JniHandleOwnership)` .

1.  Wenn `T` ist eine abstrakte Klasse oder eine Schnittstelle, gibt es *müssen* werden ein *Invoker* zur `T` . Eine aufrufende Instanz ist ein nicht abstrakter Typ, der erbt `T` oder implementiert `T` , und hat den gleichen Namen wie `T` mit dem Suffix aufrufende Instanz. Wenn T die Schnittstelle ist z. B. `Java.Lang.IRunnable` , klicken Sie dann den Typ `Java.Lang.IRunnableInvoker` muss vorhanden sein und darf die erforderlichen `(IntPtr,
    JniHandleOwnership)` Konstruktor.


Da die JNI-Methode aufrufen Methoden lokalen Refs zurückgeben `JniHandleOwnership.TransferLocalRef` wird normalerweise verwendet werden:

```csharp
IntPtr lrefString = JNIEnv.CallObjectMethod(instance, methodID);
Java.Lang.String value = Java.Lang.Object.GetObject<Java.Lang.String>( lrefString, JniHandleOwnership.TransferLocalRef);
```

<a name="_Looking_up_Java_Types" />

## <a name="looking-up-java-types"></a>Nachschlagen von Java-Typen

Um ein Feld oder eine Methode in der JNI suchen zu können, muss der deklarierende Typ für das Feld oder eine Methode zuerst angesehen werden. Die [Android.Runtime.JNIEnv.FindClass(string)](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.FindClass/(System.String)) Methode wird verwendet, um Java-Typen zu suchen. Der Parameter ist der *vereinfacht Typverweis* oder *vollständige Typverweis* für den Java-Typ. Finden Sie unter den [Typverweise JNI-Abschnitt](#_JNI_Type_References) ausführliche Informationen zum vereinfachten und vollständige Typverweise.

Hinweis: Im Gegensatz zu allen anderen `JNIEnv` -Methode die Objektinstanz zurückgibt `FindClass` gibt einen globalen Verweis, nicht auf einen lokalen Verweis zurück.

<a name="_Instance_Fields" />

## <a name="instance-fields"></a>Instanzfelder

Über die Felder bearbeitet werden *Feld-IDs*. Feld-IDs erhalten Sie über [JNIEnv.GetFieldID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetFieldID/), erfordert, dass die Klasse, die das Feld definiert ist, den Namen des Felds, und die [JNI-Typsignatur](#JNI_Type_Signatures) des Felds.

Feld-IDs müssen nicht freigegeben werden, und gültig sind, sofern der entsprechende Java-Typ geladen wird. (Android unterstützt derzeit Entladen der Klasse nicht.)

Es gibt zwei Sätze von Methoden zum Bearbeiten von Instanzfelder: eine für das Lesen von Instanzfeldern und eine für das Schreiben von Instanzfeldern. Alle Reihen von Methoden erfordern eine Feld-ID zum Lesen oder schreiben den Wert des Felds.


### <a name="reading-instance-field-values"></a>Instanz-Feldwerte lesen

Der Satz von Methoden für das Lesen der Werte von Gruppeninstanzen Feld folgt dem Benennungsmuster:

```csharp
* JNIEnv.Get*Field(IntPtr instance, IntPtr fieldID);
```
wo `*` ist der Typ des Felds:

-   [JNIEnv.GetObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetObjectField/) &ndash; liest den Wert eines Instanzenfelds, die einen integrierten Typ, z. B. nicht `java.lang.Object` , Arrays und Schnittstellentypen. Der zurückgegebene Wert ist eine lokalen JNI-verweisen.

-   [JNIEnv.GetBooleanField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetBooleanField/) &ndash; liest den Wert der `bool` Instanzfelder.

-   [JNIEnv.GetByteField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetByteField/) &ndash; liest den Wert der `sbyte` Instanzfelder.

-   [JNIEnv.GetCharField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetCharField/) &ndash; liest den Wert der `char` Instanzfelder.

-   [JNIEnv.GetShortField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetShortField/) &ndash; liest den Wert der `short` Instanzfelder.

-   [JNIEnv.GetIntField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetIntField/) &ndash; liest den Wert der `int` Instanzfelder.

-   [JNIEnv.GetLongField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetLongField/) &ndash; liest den Wert der `long` Instanzfelder.

-   [JNIEnv.GetFloatField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetFloatField/) &ndash; liest den Wert der `float` Instanzfelder.

-   [JNIEnv.GetDoubleField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetDoubleField/) &ndash; liest den Wert der `double` Instanzfelder.





### <a name="writing-instance-field-values"></a>Schreiben von Instanz-Feldwerte

Der Satz von Methoden zum Schreiben von Feldwerten Instanz folgt dem Benennungsmuster:

```csharp
JNIEnv.SetField(IntPtr instance, IntPtr fieldID, Type value);
```

wo *Typ* ist der Typ des Felds:

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.IntPtr)) &ndash; schreibt den Wert eines Felds, die einen integrierten Typ, z. B. nicht `java.lang.Object` , Arrays und Schnittstellentypen. Die `IntPtr` Wert möglicherweise einen lokalen JNI-verweisen, die globalen JNI-verweisen, die unsichere globalen JNI-verweisen, oder `IntPtr.Zero` (für `null` ).

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Boolean)) &ndash; schreiben Sie den Wert der `bool` Instanzfelder.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.SByte)) &ndash; schreiben Sie den Wert der `sbyte` Instanzfelder.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Char)) &ndash; schreiben Sie den Wert der `char` Instanzfelder.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Int16)) &ndash; schreiben Sie den Wert der `short` Instanzfelder.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Int32)) &ndash; schreiben Sie den Wert der `int` Instanzfelder.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Int64)) &ndash; schreiben Sie den Wert der `long` Instanzfelder.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Single)) &ndash; schreiben Sie den Wert der `float` Instanzfelder.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Double)) &ndash; schreiben Sie den Wert der `double` Instanzfelder.


<a name="_Static_Fields" />

## <a name="static-fields"></a>Statische Felder

Statische Felder bearbeitet werden, über *Feld-IDs*. Feld-IDs erhalten Sie über [JNIEnv.GetStaticFieldID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticFieldID/), erfordert, dass die Klasse, die das Feld definiert ist, den Namen des Felds, und die [JNI-Typsignatur](#JNI_Type_Signatures) des Felds.

Feld-IDs müssen nicht freigegeben werden, und gültig sind, sofern der entsprechende Java-Typ geladen wird. (Android unterstützt derzeit Entladen der Klasse nicht.)

Es gibt zwei Sätze von Methoden zum Bearbeiten von statischer Feldern: eine für das Lesen von Instanzfeldern und eine für das Schreiben von Instanzfeldern. Alle Reihen von Methoden erfordern eine Feld-ID zum Lesen oder schreiben den Wert des Felds.


### <a name="reading-static-field-values"></a>Lesen Sie die Werte für statische Felder

Der Satz von Methoden zum Lesen von statischen Feldwerte folgt dem Benennungsmuster:

```csharp
* JNIEnv.GetStatic*Field(IntPtr class, IntPtr fieldID);
```

wo `*` ist der Typ des Felds:

-   [JNIEnv.GetStaticObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticObjectField/) &ndash; liest den Wert eines statischen Felds, die einen integrierten Typ, z. B. nicht `java.lang.Object` , Arrays und Schnittstellentypen. Der zurückgegebene Wert ist eine lokalen JNI-verweisen.

-   [JNIEnv.GetStaticBooleanField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticBooleanField/) &ndash; liest den Wert der `bool` statische Felder.

-   [JNIEnv.GetStaticByteField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticByteField/) &ndash; liest den Wert der `sbyte` statische Felder.

-   [JNIEnv.GetStaticCharField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticCharField/) &ndash; liest den Wert der `char` statische Felder.

-   [JNIEnv.GetStaticShortField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticShortField/) &ndash; liest den Wert der `short` statische Felder.

-   [JNIEnv.GetStaticLongField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticLongField/) &ndash; liest den Wert der `long` statische Felder.

-   [JNIEnv.GetStaticFloatField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticFloatField/) &ndash; liest den Wert der `float` statische Felder.

-   [JNIEnv.GetStaticDoubleField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticDoubleField/) &ndash; liest den Wert der `double` statische Felder.



### <a name="writing-static-field-values"></a>Schreiben Sie die Werte für statische Felder

Der Satz von Methoden zum Schreiben von statischen Feldwerte folgt dem Benennungsmuster:

```csharp
JNIEnv.SetStaticField(IntPtr class, IntPtr fieldID, Type value);
```

wo *Typ* ist der Typ des Felds:

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.IntPtr)) &ndash; schreibt den Wert eines statischen Felds, die einen integrierten Typ, z. B. nicht `java.lang.Object` , Arrays und Schnittstellentypen. Die `IntPtr` Wert möglicherweise einen lokalen JNI-verweisen, die globalen JNI-verweisen, die unsichere globalen JNI-verweisen, oder `IntPtr.Zero` (für `null` ).

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

Instanzmethoden werden aufgerufen, durch *Methode IDs*. Methode-IDs erhalten Sie über [JNIEnv.GetMethodID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetMethodID/), erfordert, dass den Typ, der die Methode definiert ist, den Namen der Methode, und die [JNI-Typsignatur](#JNI_Type_Signatures) der Methode.

Methode-IDs müssen nicht freigegeben werden, und gültig sind, sofern der entsprechende Java-Typ geladen wird. (Android unterstützt derzeit Entladen der Klasse nicht.)

Es gibt zwei Sätze von Methoden zum Aufrufen von Methoden: eine für das Aufrufen von Methoden praktisch und eine für das Aufrufen von Methoden nicht virtuell. Beide Sätze von Methoden erfordern eine Methoden-ID zum Aufrufen der Methode und nicht virtuelle Aufruf erfordert auch, dass Sie angeben, welche klassenimplementierung aufgerufen werden soll.

Schnittstellenmethoden können nur innerhalb des deklarierenden Typs gesucht werden; Methoden, die Schnittstellen erweiterte/vererbte stammen können nicht gesucht werden. Finden Sie unter den neueren Schnittstellen Bindung / Invoker-Implementierung, die Informationen im Abschnitt.

Jede Methode in der Klasse deklariert, oder Basisklasse oder die implementierte Schnittstelle kann gesucht werden.


### <a name="virtual-method-invocation"></a>Virtuelle Methodenaufruf

Der Satz von Methoden zum Aufrufen von Methoden folgt praktisch das Muster:

```csharp
* JNIEnv.Call*Method( IntPtr instance, IntPtr methodID, params JValue[] args );
```

wo `*` ist der Rückgabetyp der Methode.

-   [JNIEnv.CallObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallObjectMethod/) &ndash; Aufrufen einer Methode, die einen Typ nicht "Vordefiniert", z. B. zurückgibt `java.lang.Object` , arrays und Schnittstellen. Der zurückgegebene Wert ist eine lokalen JNI-verweisen.

-   [JNIEnv.CallBooleanMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallBooleanMethod/) &ndash; Aufrufen einer Methode, wodurch eine `bool` Wert.

-   [JNIEnv.CallByteMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallByteMethod/) &ndash; Aufrufen einer Methode, wodurch eine `sbyte` Wert.

-   [JNIEnv.CallCharMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallCharMethod/) &ndash; Aufrufen einer Methode, wodurch eine `char` Wert.

-   [JNIEnv.CallShortMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallShortMethod/) &ndash; Aufrufen einer Methode, wodurch eine `short` Wert.

-   [JNIEnv.CallLongMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallLongMethod/) &ndash; Aufrufen einer Methode, wodurch eine `long` Wert.

-   [JNIEnv.CallFloatMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallFloatMethod/) &ndash; Aufrufen einer Methode, wodurch eine `float` Wert.

-   [JNIEnv.CallDoubleMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallDoubleMethod/) &ndash; Aufrufen einer Methode, wodurch eine `double` Wert.



### <a name="non-virtual-method-invocation"></a>Nicht virtuelle Methodenaufruf.

Der Satz von Methoden zum Aufrufen von Methoden folgt nicht virtuell dem Benennungsmuster:

```csharp
* JNIEnv.CallNonvirtual*Method( IntPtr instance, IntPtr class, IntPtr methodID, params JValue[] args );
```

wo `*` ist der Rückgabetyp der Methode. Nicht virtuelle Methodenaufruf wird normalerweise verwendet, um die Basismethode einer virtuellen Methode aufzurufen.

-   [JNIEnv.CallNonvirtualObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualObjectMethod/) &ndash; nicht virtuell Aufrufen einer Methode die einen Typ nicht "Vordefiniert", z. B. zurückgibt `java.lang.Object` , arrays und Schnittstellen. Der zurückgegebene Wert ist eine lokalen JNI-verweisen.

-   [JNIEnv.CallNonvirtualBooleanMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualBooleanMethod/) &ndash; nicht virtuell Aufrufen eine Methode, wodurch eine `bool` Wert.

-   [JNIEnv.CallNonvirtualByteMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualByteMethod/) &ndash; nicht virtuell Aufrufen eine Methode, wodurch eine `sbyte` Wert.

-   [JNIEnv.CallNonvirtualCharMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualCharMethod/) &ndash; nicht virtuell Aufrufen eine Methode, wodurch eine `char` Wert.

-   [JNIEnv.CallNonvirtualShortMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualShortMethod/) &ndash; nicht virtuell Aufrufen eine Methode, wodurch eine `short` Wert.

-   [JNIEnv.CallNonvirtualLongMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualLongMethod/) &ndash; nicht virtuell Aufrufen eine Methode, wodurch eine `long` Wert.

-   [JNIEnv.CallNonvirtualFloatMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualFloatMethod/) &ndash; nicht virtuell Aufrufen eine Methode, wodurch eine `float` Wert.

-   [JNIEnv.CallNonvirtualDoubleMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualDoubleMethod/) &ndash; nicht virtuell Aufrufen eine Methode, wodurch eine `double` Wert.


<a name="_Static_Methods" />

## <a name="static-methods"></a>Statische Methoden

Statische Methoden werden aufgerufen, durch *Methode IDs*. Methode-IDs erhalten Sie über [JNIEnv.GetStaticMethodID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticMethodID/), erfordert, dass den Typ, der die Methode definiert ist, den Namen der Methode, und die [JNI-Typsignatur](#JNI_Type_Signatures) der Methode.

Methode-IDs müssen nicht freigegeben werden, und gültig sind, sofern der entsprechende Java-Typ geladen wird. (Android unterstützt derzeit Entladen der Klasse nicht.)



### <a name="static-method-invocation"></a>Statische Methode aufrufen

Der Satz von Methoden zum Aufrufen von Methoden folgt praktisch das Muster:

```csharp
* JNIEnv.CallStatic*Method( IntPtr class, IntPtr methodID, params JValue[] args );
```

wo `*` ist der Rückgabetyp der Methode.

-   [JNIEnv.CallStaticObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticObjectMethod/) &ndash; Aufrufen einer statischen Methode, die einen Typ nicht "Vordefiniert", z. B. zurückgibt `java.lang.Object` , arrays und Schnittstellen. Der zurückgegebene Wert ist eine lokalen JNI-verweisen.

-   [JNIEnv.CallStaticBooleanMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticBooleanMethod/) &ndash; Aufrufen einer statischen Methode, wodurch eine `bool` Wert.

-   [JNIEnv.CallStaticByteMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticByteMethod/) &ndash; Aufrufen einer statischen Methode, wodurch eine `sbyte` Wert.

-   [JNIEnv.CallStaticCharMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticCharMethod/) &ndash; Aufrufen einer statischen Methode, wodurch eine `char` Wert.

-   [JNIEnv.CallStaticShortMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticShortMethod/) &ndash; Aufrufen einer statischen Methode, wodurch eine `short` Wert.

-   [JNIEnv.CallStaticLongMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallLongMethod/) &ndash; Aufrufen einer statischen Methode, wodurch eine `long` Wert.

-   [JNIEnv.CallStaticFloatMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticFloatMethod/) &ndash; Aufrufen einer statischen Methode, wodurch eine `float` Wert.

-   [JNIEnv.CallStaticDoubleMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticDoubleMethod/) &ndash; Aufrufen einer statischen Methode, wodurch eine `double` Wert.


<a name="JNI_Type_Signatures" />

## <a name="jni-type-signatures"></a>Signaturen für JNI-Typ

[JNI-Typsignaturen](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/types.html#wp16432) sind [JNI-Typverweise](#_JNI_Type_References) (jedoch nicht vereinfachte Typverweise), außer bei Methoden. Methoden, die Signatur der JNI-Typ ist eine öffnende Klammer `'('`, gefolgt von der Verweise auf Typen für alle Typen, die verkettet (mit ohne Trennzeichen Komma o. ä.), gefolgt von einer schließenden Klammer parametru `')'`, gefolgt von den JNI-Typverweis, der Rückgabetyp der Methode.

Angenommen, die Java-Methode:

```java
long f(int n, String s, int[] array);
```

Die Signatur der JNI-Typ wäre:

```csharp
(ILjava/lang/String;[I)J
```

Im Allgemeinen ist es *stark* empfohlen, verwenden die `javap` Befehl aus, um die JNI-Signaturen zu bestimmen. Z. B. die JNI Typsignatur der [java.lang.Thread.State.valueOf(String)](https://developer.android.com/reference/java/lang/Thread.State.html#valueOf(java.lang.String)) Methode ist "(Ljava/Lang/String); Ljava/Lang / $Threadzustand;", während die JNI geben die Signatur des der [ java.lang.Thread.State.values](https://developer.android.com/reference/java/lang/Thread.State.html#values) Methode ist "() [Ljava/Lang / $Threadzustand;". Achten Sie für die nachfolgende Semikolons; Diese *sind* Teil der Signatur der JNI-Typ.

<a name="_JNI_Type_References" />

## <a name="jni-type-references"></a>Verweise auf die JNI-Typen

Verweise auf die JNI-Typen unterscheiden sich von der Verweise auf die Java-Typen. Sie können keine vollqualifizierte Namen der Java-Typ wie z. B. `java.lang.String` mit JNI, verwenden Sie stattdessen die JNI-Varianten `"java/lang/String"` oder `"Ljava/lang/String;"`, je nach Kontext; Weitere Informationen siehe unten.
Es gibt vier Arten von Typverweisen JNI:

-  **built-in**
-  **simplified**
-  **Typ**
-  **array**


### <a name="built-in-type-references"></a>Verweise auf integrierte Typen

Verweise auf integrierte Typen sind ein einzelnes Zeichen, die mit der integrierten Werttypen verweisen. Die Zuordnung ist wie folgt aus:

-  `"B"` für `sbyte` .
-  `"S"` für `short` .
-  `"I"` für `int` .
-  `"J"` für `long` .
-  `"F"` für `float` .
-  `"D"` für `double` .
-  `"C"` für `char` .
-  `"Z"` für `bool` .
-  `"V"` für `void` Methode zurückgeben.


<a name="_Simplified_Type_References_1" />

### <a name="simplified-type-references"></a>Vereinfachte Typverweise

Vereinfachte Typverweise können nur verwendet werden, [JNIEnv.FindClass(string)](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.FindClass/(System.String)).
Es gibt zwei Möglichkeiten, um ein vereinfachtes Typverweis abgeleitet werden:

1.  Ersetzen Sie einen Namen für den vollqualifizierten Java jeder `'.'` innerhalb der Paketname und vor der Typname mit `'/'` , und jede `'.'` in einen Typnamen mit `'$'` .

1.  Lesen die Ausgabe des `'unzip -l android.jar | grep JavaName'` .


Eine der beiden in der Java-Typ führt [java.lang.Thread.State](https://developer.android.com/reference/java/lang/Thread.State.html) zugeordnet wird, um die vereinfachte Typverweis `java/lang/Thread$State`.


### <a name="type-references"></a>Verweise auf Typen

Ein Typverweis ist ein integrierter Typ-Verweis oder einen vereinfachten Typverweis mit einer `'L'` Präfix und einem `';'` Suffix. Für den Java-Typ [java.lang.String](https://developer.android.com/reference/java/lang/String.html), ist der vereinfachte Typverweis `"java/lang/String"`, während der Typverweis `"Ljava/lang/String;"`.

Verweise auf Typen werden mit Array-Typverweisen und mit JNI-Signaturen verwendet werden.

Eine weitere Möglichkeit zum Abrufen von eines Typverweis ist, lesen Sie die Ausgabe des `'javap -s -classpath android.jar fully.qualified.Java.Name'`.
Je nach Typ erfolgt, können Sie eine Konstruktordeklaration verwendet oder eine Methode geben die JNI ermitteln. Zum Beispiel:

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

`Thread.State` ein Java-Enum-Typ ist, daher wir die Signatur verwenden der `valueOf` Methode ermitteln, ob der Typverweis Ljava/Lang/Thread$ Zustand ist.



### <a name="array-type-references"></a>Verweise auf Typen von Arrays

Array-Typ-Verweise sind `'['` mit dem Präfix an einen JNI-Typverweis.
Vereinfachte Typverweise können nicht verwendet werden, wenn Sie Arrays angeben.

Z. B. `int[]` ist `"[I"`, `int[][]` ist `"[[I"`, und `java.lang.Object[]` ist `"[Ljava/lang/Object;"`.



## <a name="java-generics-and-type-erasure"></a>Java-Generika und Typlöschung

*Die meisten* der Zeit, wie durch JNI, Java-Generika *nicht vorhanden sind*.
Es gibt einige "Falten", aber diese Falten befinden sich in der Interaktion von Java mit Generika, nicht mit wie JNI sucht und generische Member aufruft.

Bei der Interaktion mit JNI besteht kein Unterschied zwischen einen generischen Typ oder Member und einen nicht generischen Typ oder Member. Z. B. den generischen Typ [java.lang.Class&lt;T&gt; ](https://developer.android.com/reference/java/lang/Class.html) ist auch der "rohes" generische Typ `java.lang.Class`, die jeweils des gleichen vereinfachte Typverweis `"java/lang/Class"`.


## <a name="java-native-interface-support"></a>Java Native Interface-Unterstützung

[Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/) wird der verwalteter Wrapper für die Jave Native Interface (JNI). JNI-Funktionen werden deklariert, innerhalb der [Java Native Interface Specification](http://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html), obwohl die Methoden geändert haben, um den expliziten entfernen `JNIEnv*` Parameter und `IntPtr` anstelle `jobject`, `jclass`, `jmethodID`usw. Betrachten Sie beispielsweise die [JNI NewObject-Funktion](http://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp4517):

```csharp
jobject NewObjectA(JNIEnv *env, jclass clazz, jmethodID methodID, jvalue *args);
```

Dies wird als verfügbar gemacht, die [JNIEnv.NewObject](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.NewObject/p/System.IntPtr/System.IntPtr/Android.Runtime.JValue[]/) Methode:

```csharp
public static IntPtr NewObject(IntPtr clazz, IntPtr jmethod, params JValue[] parms);
```

Die Übersetzung zwischen den beiden aufrufen ist vernünftigerweise unkompliziert. In C# müssten Sie:

```c
jobject CreateMapActivity(JNIEnv *env)
{
    jclass    Map_Class   = (*env)->FindClass(env, "mono/samples/googlemaps/MyMapActivity");
    jmethodID Map_defCtor = (*env)->GetMethodID (env, Map_Class, "<init>", "()V");
    jobject   instance    = (*env)->NewObject (env, Map_Class, Map_defCtor);

    return instance;
}
```

Die C# entsprechende wäre:

```csharp
IntPtr CreateMapActivity()
{
    IntPtr Map_Class   = JNIEnv.FindClass ("mono/samples/googlemaps/MyMapActivity");
    IntPtr Map_defCtor = JNIEnv.GetMethodID (Map_Class, "<init>", "()V");
    IntPtr instance    = JNIEnv.NewObject (Map_Class, Map_defCtor);

    return instance;
}
```

Nachdem Sie eine Java-Objektinstanz, die in einen IntPtr gespeichert haben, sollten Sie wahrscheinlich etwas damit anfangen. Sie können z. B. JNIEnv Methoden verwenden [ <span class="external">JNIEnv.CallVoidMethod()</span> ](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallVoidMethod/p/System.IntPtr/System.IntPtr/Android.Runtime.JValue[]/) zu tun, aber wenn bereits eine analoge C# Wrapper und Sie sollten einen Wrapper über die JNI-verweisen zu konstruieren. Sie erreichen dies über die [Extensions.JavaCast <t>()</t> ](https://developer.xamarin.com/api/member/Android.Runtime.Extensions.JavaCast%7BTResult%7D/p/Android.Runtime.IJavaObject/) Erweiterungsmethode:

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

Darüber hinaus alle JNI-Funktionen geändert wurden durch das Entfernen der `JNIEnv*` Parameter, die in jeder JNI-Funktion vorhanden.


## <a name="summary"></a>Zusammenfassung

Direkt mit JNI ist eine terrible Erfahrung, die unter allen Umständen vermieden werden sollten. Leider ist es nicht immer vermeidbar. Dieses Handbuch stellt hoffentlich Hier erhalten Sie Unterstützung bereit, wenn Sie die ungebundenen Java Fälle mit Mono für Android erreichen.


## <a name="related-links"></a>Verwandte Links

- [SanityTests (Beispiel)](https://developer.xamarin.com/samples/SanityTests/)
- [Java Native Interface-Spezifikation](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/jniTOC.html)
- [Java Native Interface--Funktionen](http://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html)
