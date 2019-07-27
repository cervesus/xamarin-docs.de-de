---
title: Xamarin. Android im Vergleich zu Desktop-Unterschiede in der Mono-Laufzeit
ms.prod: xamarin
ms.assetid: F953F9B4-3596-8B3A-A8E4-8219B5B9F7CA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: 18e6e82011460a51a96df4694f15b36c5ec94ab5
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510711"
---
# <a name="limitations"></a>Einschränkungen

Da Anwendungen auf Android während des Buildprozesses Java-Proxy Typen generieren müssen, ist es nicht möglich, den gesamten Code zur Laufzeit zu generieren.

Dies sind die xamarin. Android-Einschränkungen im Vergleich zu Desktop Mono:

## <a name="limited-dynamic-language-support"></a>Eingeschränkte Unterstützung dynamischer Sprachen

 [Android Callable Wrapper](~/android/platform/java-integration/android-callable-wrappers.md) werden immer dann benötigt, wenn die Android-Laufzeit verwalteten Code aufrufen muss. Android Callable Wrapper werden zur Kompilierzeit basierend auf der statischen Analyse von Il generiert. Das Ergebnis: Sie *können* keine dynamischen Sprachen (IronPython, IronRuby usw.) in jedem Szenario verwenden, in dem die Unterklassen von Java-Typen erforderlich sind (einschließlich indirekter Unterklassen), da es keine Möglichkeit gibt, diese dynamischen Typen zur Kompilierzeit in zu extrahieren. Generieren Sie die erforderlichen Android Callable Wrapper.

## <a name="limited-java-generation-support"></a>Eingeschränkte Unterstützung für Java-Generierung

[Android Callable Wrapper](~/android/platform/java-integration/android-callable-wrappers.md) müssen generiert werden, damit Java-Code verwalteten Code aufrufen kann. *Standardmäßig*enthalten Android Callable Wrapper nur (bestimmte) deklarierte Konstruktoren und Methoden, die eine virtuelle Java-Methode über [`RegisterAttribute`](xref:Android.Runtime.RegisterAttribute)schreiben (d. h.) oder eine Java-Schnittstellen Methode implementieren (die Schnittstelle weist `Attribute`gleichermaßen).
  
Vor Version 4,1 konnten keine zusätzlichen Methoden deklariert werden. Mit der Version 4,1 [können die `Export` Benutzer `ExportField` definierten Attribute und verwendet werden, um Java-Methoden und-Felder innerhalb des Android Callable Wrapper zu deklarieren](~/android/platform/java-integration/working-with-jni.md).

### <a name="missing-constructors"></a>Fehlende Konstruktoren

Konstruktoren bleiben knifflig, [`ExportAttribute`](xref:Java.Interop.ExportAttribute) es sei denn, wird verwendet. Der Algorithmus zum Erstellen von Android Callable Wrapper Konstruktoren ist, dass ein Java-Konstruktor ausgegeben wird, wenn Folgendes gilt:

1. Für alle Parametertypen ist eine Java-Zuordnung vorhanden.
2. Die Basisklasse deklariert denselben Konstruktor &ndash; . Dies ist erforderlich, da der von Android Callable Wrapper den entsprechenden Basisklassenkonstruktor aufrufen *muss* . es können keine Standardargumente verwendet werden (da es keine einfache Möglichkeit gibt, um zu ermitteln, welche Werte sollte in Java verwendet werden.)

Betrachten Sie beispielsweise die folgende Klasse:

```csharp
[Service]
class MyIntentService : IntentService {
    public MyIntentService (): base ("value")
    {
    }
}
```

Obwohl dies perfekt logisch aussieht, enthält der resultierende Android Callable Wrapper *in Releasebuilds* keinen Standardkonstruktor. Wenn Sie also versuchen, diesen Dienst zu starten (z. [`Context.StartService`](xref:Android.Content.Context.StartService*)b. tritt ein Fehler auf:

```shell
E/AndroidRuntime(31766): FATAL EXCEPTION: main
E/AndroidRuntime(31766): java.lang.RuntimeException: Unable to instantiate service example.MyIntentService: java.lang.InstantiationException: can't instantiate class example.MyIntentService; no empty constructor
E/AndroidRuntime(31766):        at android.app.ActivityThread.handleCreateService(ActivityThread.java:2347)
E/AndroidRuntime(31766):        at android.app.ActivityThread.access$1600(ActivityThread.java:130)
E/AndroidRuntime(31766):        at android.app.ActivityThread$H.handleMessage(ActivityThread.java:1277)
E/AndroidRuntime(31766):        at android.os.Handler.dispatchMessage(Handler.java:99)
E/AndroidRuntime(31766):        at android.os.Looper.loop(Looper.java:137)
E/AndroidRuntime(31766):        at android.app.ActivityThread.main(ActivityThread.java:4745)
E/AndroidRuntime(31766):        at java.lang.reflect.Method.invokeNative(Native Method)
E/AndroidRuntime(31766):        at java.lang.reflect.Method.invoke(Method.java:511)
E/AndroidRuntime(31766):        at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:786)
E/AndroidRuntime(31766):        at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:553)
E/AndroidRuntime(31766):        at dalvik.system.NativeStart.main(Native Method)
E/AndroidRuntime(31766): Caused by: java.lang.InstantiationException: can't instantiate class example.MyIntentService; no empty constructor
E/AndroidRuntime(31766):        at java.lang.Class.newInstanceImpl(Native Method)
E/AndroidRuntime(31766):        at java.lang.Class.newInstance(Class.java:1319)
E/AndroidRuntime(31766):        at android.app.ActivityThread.handleCreateService(ActivityThread.java:2344)
E/AndroidRuntime(31766):        ... 10 more
```

Die Problem Umgehung besteht darin, einen Standardkonstruktor zu deklarieren `ExportAttribute`, ihn mit zu [`ExportAttribute.SuperStringArgument`](xref:Java.Interop.ExportAttribute.SuperArgumentsString)schmücken und Folgendes festzulegen: 

```csharp
[Service]
class MyIntentService : IntentService {
    [Export (SuperArgumentsString = "\"value\"")]
    public MyIntentService (): base("value")
    {
    }

    // ...
}
```


### <a name="generic-c-classes"></a>Generische C# Klassen

Generische C# Klassen werden nur teilweise unterstützt. Die folgenden Einschränkungen sind vorhanden:


-   Generische Typen dürfen nicht oder `[Export]` `[ExportField`verwenden. Wenn Sie versuchen, dies zu tun `XA4207` , wird ein Fehler generiert.

    ```csharp
    public abstract class Parcelable<T> : Java.Lang.Object, IParcelable
    {
        // Invalid; generates XA4207
        [ExportField ("CREATOR")]
        public static IParcelableCreator CreateCreator ()
        {
            ...
    }
    ```

-   Generische Methoden dürfen nicht oder `[Export]` `[ExportField]`verwenden:

    ```csharp
    public class Example : Java.Lang.Object
    {
        
        // Invalid; generates XA4207
        [Export]
        public static void Method<T>(T value)
        {
            ...
        }
    }
    ```

-   `[ExportField]`darf nicht für Methoden verwendet werden, die `void`Folgendes zurückgeben:

    ```csharp
    public class Example : Java.Lang.Object
    {
        // Invalid; generates XA4208
        [ExportField ("CREATOR")]
        public static void CreateSomething ()
        {
        }
    }
    ```

-   Instanzen generischer Typen _dürfen nicht_ aus Java-Code erstellt werden.
    Sie können nur sicher aus verwaltetem Code erstellt werden:

    ```csharp
    [Activity (Label="Die!", MainLauncher=true)]
    public class BadGenericActivity<T> : Activity
    {
        protected override void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);
        }
    }
    ```

## <a name="partial-java-generics-support"></a>Partielle Unterstützung für Java-Generika

Die Bindungs Unterstützung für Java Generika ist begrenzt. Insbesondere werden Elemente in einer generischen Instanzklasse, die von einer anderen generischen (nicht instanziierten) Klasse abgeleitet ist, als Java. lang. Object offengelegt. Beispielsweise gibt die [Android. Content. Intent. getsamcelableextra](xref:Android.Content.Intent.GetParcelableExtra*) -Methode "java. lang. Object" zurück. Dies ist auf gelöschte Java-Generika zurückzuführen.
Wir haben einige Klassen, die diese Einschränkung nicht anwenden, aber Sie werden manuell angepasst.

## <a name="related-links"></a>Verwandte Links

- [Android Callable Wrapper](~/android/platform/java-integration/android-callable-wrappers.md)
- [Arbeiten mit jni](~/android/platform/java-integration/working-with-jni.md)
- [Export Attribute](xref:Java.Interop.ExportAttribute)
- [SuperString](xref:Java.Interop.ExportAttribute.SuperArgumentsString)
- [RegisterAttribute](xref:Android.Runtime.RegisterAttribute)
