---
title: Einschränkungen
ms.prod: xamarin
ms.assetid: F953F9B4-3596-8B3A-A8E4-8219B5B9F7CA
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: 07353cfa3ce9bc20688b9a59c09a6e602f16dd60
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="limitations"></a>Einschränkungen

Da Anwendungen auf Android-Geräten erforderlich sind, Generieren von Java-Proxytypen während des Buildprozesses nimmt, ist es nicht möglich, alle Code zur Laufzeit generieren.

Dies sind die im Vergleich zum Desktop Mono Xamarin.Android-Einschränkungen:


## <a name="limited-dynamic-language-support"></a>Begrenzte dynamische sprachunterstützung

 [Android Aufrufwrappern](~/android/platform/java-integration/android-callable-wrappers.md) jederzeit die Android-Laufzeit verwalteter Code aufgerufen muss werden benötigt. Android callable Wrapper werden zum Zeitpunkt der Kompilierung generiert basierend auf statische Analyse von IL. Das Ergebnis dieses: Sie *kann nicht* orientieren dynamische Sprachen (IronPython, IronRuby, usw.) in jedem Szenario, für die Erstellung von Unterklassen von Java-Typen (einschließlich indirekte Unterklassen), erforderlich ist es gibt keine Möglichkeit zum Extrahieren von diesen dynamischen Typen zum Zeitpunkt der Kompilierung den erforderlichen Android callable Wrapper generiert.


## <a name="limited-java-generation-support"></a>Unterstützung der eingeschränkten Java-Generierung

[Android Aufrufwrappern](~/android/platform/java-integration/android-callable-wrappers.md) nacheinander für Java-Code zum Aufrufen von verwalteten Codes generiert werden müssen. *Standardmäßig*, Android callable Wrapper (bestimmte) deklarierten Konstruktoren und Methoden, die überschreiben eine virtuelle Java-Methode nur enthalten (d. h. sie hat [ `RegisterAttribute` ](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)) oder eine Java-Methode ()-Schnittstelle implementieren Ebenso Schnittstelle verfügt über `Attribute`).
  
Vor der Version 4.1 konnte keine zusätzlichen Methoden deklariert werden. Mit der Version 4.1 [der `Export` und `ExportField` benutzerdefinierte Attribute können verwendet werden, um Java-Methoden und Felder innerhalb der Android Callable Wrapper deklarieren](~/android/platform/java-integration/working-with-jni.md).

### <a name="missing-constructors"></a>Fehlende Konstruktoren

Konstruktoren bleiben einfach, es sei denn, [ `ExportAttribute` ](https://developer.xamarin.com/api/type/Java.Interop.ExportAttribute) verwendet wird. Der Algorithmus zum Generieren von Android Aufrufwrapper Konstruktoren ist, dass ein Java-Konstruktor Wenn ausgegeben werden:

1. Es ist eine Java-Zuordnung für alle Parametertypen
2. Die Basisklasse deklariert den gleichen Konstruktor &ndash; Dies ist erforderlich, da Android callable Wrappers *müssen* entsprechenden Basisklassenkonstruktor aufrufen; können keine Standardargumente verwendet werden, (wie es keine einfache Methode zum ist bestimmen, welche Werte annehmen müssen innerhalb von Java verwendet).

Betrachten Sie beispielsweise die folgende Klasse:

```csharp
[Service]
class MyIntentService : IntentService {
    public MyIntentService (): base ("value")
    {
    }
}
```

Während dieser perfekt logischen sieht den resultierenden Android callable Wrapper *in Releasebuilds* enthält einen standardmäßigen Konstruktor nicht. Daher, wenn Sie versuchen, diesen Dienst zu starten (z. B. [ `Context.StartService` ](https://developer.xamarin.com/api/member/Android.Content.Context.StartService/p/Android.Content.Intent/)), schlägt fehl:

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

Die problemumgehung besteht darin, einen standardmäßigen Konstruktor zu deklarieren, die hinzugefügt wurden, mit der `ExportAttribute`, und legen Sie die [ `ExportAttribute.SuperStringArgument` ](https://developer.xamarin.com/api/property/Java.Interop.ExportAttribute.SuperArgumentsString/): 

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


### <a name="generic-c-classes"></a>Generische Klassen in c#

Generische Klassen in c# werden nur teilweise unterstützt. Die folgenden Einschränkungen vorhanden sind:


-   Generische Typen können nicht `[Export]` oder `[ExportField`]. Versuch generiert eine `XA4207` Fehler.

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

-   Generische Methoden können nicht `[Export]` oder `[ExportField]`:

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

-   `[ExportField]` dürfen nicht verwendet werden, auf Methoden, die zurückgeben `void`:

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

-   Instanzen generischer Typen _muss nicht_ aus Java-Code erstellt werden.
    Sie können nur sichere Weise von verwaltetem Code erstellt werden:

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


## <a name="partial-java-generics-support"></a>Partielle Java Generika-Unterstützung

Java, Binden von Generika-Unterstützung, ist begrenzt. Besonders, Elemente in einer generischen Instanzenklasse, die von einer anderen generischen (nicht instanziiert)-Klasse abgeleitet wird links sind als Java.Lang.Object verfügbar gemacht. Beispielsweise [Android.Content.Intent.GetParcelableExtra](https://developer.xamarin.com/api/member/Android.Content.Intent.GetParcelableExtra/p/System.String/) Methodenrückgabe Java.Lang.Object. Dies liegt an den gelöschten Java Generika.
Wir haben einige Klassen, die diese Einschränkung nicht gelten, aber sie manuell angepasst werden.


## <a name="related-links"></a>Verwandte Links

- [Android Callable Wrapper](~/android/platform/java-integration/android-callable-wrappers.md)
- [Arbeiten mit JNI](~/android/platform/java-integration/working-with-jni.md)
- [ExportAttribute](https://developer.xamarin.com/api/type/Java.Interop.ExportAttribute/)
- [SuperString](https://developer.xamarin.com/api/property/Java.Interop.ExportAttribute.SuperArgumentsString/)
- [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)
