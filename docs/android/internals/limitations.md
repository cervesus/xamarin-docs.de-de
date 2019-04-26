---
title: Xamarin.Android vs. Desktop - Unterschiede in die Mono-Laufzeit
ms.prod: xamarin
ms.assetid: F953F9B4-3596-8B3A-A8E4-8219B5B9F7CA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: 115d715214d7af3174c41d9d82e894ce429dab42
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "60953343"
---
# <a name="limitations"></a>Einschränkungen

Da Anwendungen unter Android erforderlich ist, Generieren von Java-Proxy-Typen während des Buildprozesses, ist es nicht möglich, alle Code zur Laufzeit zu generieren.

Dies sind die Xamarin.Android-Einschränkungen, die im Vergleich zum Desktop Mono:


## <a name="limited-dynamic-language-support"></a>Eingeschränkte dynamische sprachunterstützung

 [Android callable Wrapper](~/android/platform/java-integration/android-callable-wrappers.md) sind erforderlich, einem beliebigen Zeitpunkt die Android-Laufzeit verwalteter Code aufgerufen muss. Android callable Wrapper werden zum Zeitpunkt der Kompilierung generiert basierend auf der statischen Analyse von IL. Das Ergebnis dieser: Sie *kann nicht* verwenden dynamische Sprachen (IronPython, IronRuby, usw.) in jedem Szenario, für die Unterklassen von Java-Typen (einschließlich indirekte Unterklassen), erforderlich ist, wie es keine Möglichkeit gibt, extrahieren Sie diese dynamischen Typen zum Zeitpunkt der Kompilierung zum Generieren der erforderlichen Android callable Wrapper.


## <a name="limited-java-generation-support"></a>Unterstützung für die Codegenerierung beschränkt Java

[Android Callable Wrapper](~/android/platform/java-integration/android-callable-wrappers.md) müssen in der Reihenfolge für die Java-Code zum Aufrufen von verwalteten Codes generiert werden soll. *Standardmäßig*, Android callable Wrapper sind nur enthalten, (bestimmte) deklarierten Konstruktoren und Methoden, die eine virtuelle Java-Methode außer Kraft setzen (d. h. sie hat [ `RegisterAttribute` ](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)) oder eine Java-Methode ()-Schnittstelle implementieren Schnittstelle verfügt ebenso über `Attribute`).
  
Vor der Version 4.1 konnten keine zusätzlichen Methoden deklariert werden. Mit der Version 4.1 [der `Export` und `ExportField` benutzerdefinierte Attribute können verwendet werden, um Java-Methoden und Felder in den Android Callable Wrapper deklarieren](~/android/platform/java-integration/working-with-jni.md).

### <a name="missing-constructors"></a>Fehlende Konstruktoren

Konstruktoren bleiben schwierig, es sei denn, [ `ExportAttribute` ](https://developer.xamarin.com/api/type/Java.Interop.ExportAttribute) verwendet wird. Der Algorithmus zum Generieren von Android aufrufbare wrapperkonstruktoren ist, dass ein Java-Konstruktor Wenn ausgegeben werden:

1. Es ist eine Java-Zuordnung für alle Parametertypen
2. Die Basisklasse deklariert, die gleichen Konstruktor &ndash; Dies ist erforderlich, da der Android callable Wrapper *muss* den entsprechenden Konstruktor der Basisklasse aufrufen, können keine Standardargumente verwendet werden, (wie es keine einfache Möglichkeit ist, zu bestimmen, welche Werte sollten innerhalb von Java verwendet).

Betrachten Sie beispielsweise die folgende Klasse:

```csharp
[Service]
class MyIntentService : IntentService {
    public MyIntentService (): base ("value")
    {
    }
}
```

Während dieser durchaus logisch, sieht die resultierende Android callable Wrapper *in Releasebuilds* enthält einen standardmäßigen Konstruktor nicht. Daher, wenn Sie versuchen, diesen Dienst zu starten (z. B. [ `Context.StartService` ](https://developer.xamarin.com/api/member/Android.Content.Context.StartService/p/Android.Content.Intent/)), wird fehlerhaft sein:

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

Die problemumgehung besteht darin, einen standardmäßigen Konstruktor zu deklarieren, die mit Verzieren der `ExportAttribute`, und legen Sie die [ `ExportAttribute.SuperStringArgument` ](https://developer.xamarin.com/api/property/Java.Interop.ExportAttribute.SuperArgumentsString/): 

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

Generische C# Klassen werden nur teilweise unterstützt. Die folgenden Einschränkungen gelten:


-   Generische Typen können nicht `[Export]` oder `[ExportField`]. Dies dennoch versuchen, generiert eine `XA4207` Fehler.

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

-   `[ExportField]` kann nicht verwendet werden, auf Methoden, die zurückgeben `void`:

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

-   Instanzen von generischen Typen _darf nicht_ aus Java-Code erstellt werden.
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


## <a name="partial-java-generics-support"></a>Partielle Java Generika-Unterstützung

Die Java, die generische Typen, die Datenbindung unterstützen, ist begrenzt. Besonders, verbleiben Elemente in eine generische Instanz-Klasse, die von einem anderen generischen (nicht instanziiert)-Klasse abgeleitet ist als Java.Lang.Object verfügbar gemacht werden. Z. B. [Android.Content.Intent.GetParcelableExtra](https://developer.xamarin.com/api/member/Android.Content.Intent.GetParcelableExtra/p/System.String/) Methodenrückgabe Java.Lang.Object. Dies ist aufgrund gelöschter Java Generika.
Wir haben einige Klassen, die diese Einschränkung nicht gelten, aber sie werden manuell angepasst.


## <a name="related-links"></a>Verwandte Links

- [Android Callable Wrapper](~/android/platform/java-integration/android-callable-wrappers.md)
- [Arbeiten mit JNI](~/android/platform/java-integration/working-with-jni.md)
- [ExportAttribute](https://developer.xamarin.com/api/type/Java.Interop.ExportAttribute/)
- [SuperString](https://developer.xamarin.com/api/property/Java.Interop.ExportAttribute.SuperArgumentsString/)
- [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)
