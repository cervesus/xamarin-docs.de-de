---
title: Android Callable Wrapper
ms.prod: xamarin
ms.assetid: C33E15FA-1E2B-819A-C656-CA588D611492
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: bb15c7f3a36cc7f1ed235d92e343bbae67a718b8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="android-callable-wrappers"></a>Android Callable Wrapper

Android Callable Wrapper (ACWs) sind erforderlich, wenn die Laufzeit Android verwalteter Code aufgerufen wird. Diese Wrapper sind erforderlich, da es keine Möglichkeit gibt, um Klassen zur Laufzeit mit Grafiken (Android Runtime) zu registrieren. (Insbesondere die [DefineClass() JNI-Funktion](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp15986) wird von der Android-Laufzeit nicht unterstützt.} Android Aufrufwrappern bilden daher für den Mangel an Unterstützung für die Registrierung von Common Language Runtime Typen. 

*Jedes Mal, wenn* Android Code ausgeführt werden muss eine `virtual` oder die Methode, die Schnittstelle `overridden` oder in verwaltetem Code implementiert, Xamarin.Android muss einen Java-Proxy, damit diese Methode in den entsprechenden verwalteten Typ weitergeleitet wird. Diese Java-Proxy-Typen sind Java-Code mit dem "dieselbe" Basisklasse und Java-Schnittstellenliste als verwalteten Typ, implementieren die gleichen Konstruktoren und jede außer Kraft gesetzte Basisklasse und Methoden der Schnittstelle deklariert. 

Android callable Wrapper werden generiert, indem die **monodroid.exe** programmieren, während die [Buildprozess](~/android/deploy-test/building-apps/build-process.md): sie werden generiert für alle Typen, die (direkt oder indirekt) erben [ Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/). 



## <a name="android-callable-wrapper-naming"></a>Benennen von Android Callable Wrapper

Paketnamen für Android Aufrufwrappern basieren auf der von der Assembly qualifizierte Name des Typs, der zu exportierenden MD5SUM. Diese Benennungskonvention Technik ermöglicht es, für den gleichen vollqualifizierten Typnamen verfügbar zu machen, indem verschiedene Assemblys ohne Einführung in ein Packaging-Fehler. 

Wegen dieses Benennungsschema MD5SUM kann nicht direkt eine Typen nach Namen zugegriffen werden. Beispielsweise die folgenden `adb` Befehl funktioniert nicht, da der Typname `my.ActivityType` wird standardmäßig nicht generiert: 

```shell
adb shell am start -n My.Package.Name/my.ActivityType
```

Darüber hinaus möglicherweise Fehler wie der folgende angezeigt, wenn Sie versuchen, einen Typ anhand des Namens verweisen:

```shell
java.lang.ClassNotFoundException: Didn't find class "com.company.app.MainActivity"
on path: DexPathList[[zip file "/data/app/com.company.App-1.apk"] ...
```

Wenn Sie *führen* erfordern Zugriff auf Typen anhand des Namens, können Sie einen Namen für diesen Typ in einer Attributdeklaration deklarieren. Hier ist z. B. Code, der eine Aktivität mit dem vollqualifizierten Namen deklariert `My.ActivityType`:

```csharp
namespace My {
    [Activity]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

Die `ActivityAttribute.Name` Eigenschaft kann festgelegt werden, um den Namen dieser Aktivität explizit zu deklarieren: 

```csharp
namespace My {
    [Activity(Name="my.ActivityType")]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

Nachdem die Einstellung dieser Eigenschaft hinzugefügt wird, `my.ActivityType` möglich, die anhand des Namens von externem Code und von `adb` Skripts. Die `Name` Attribut kann festgelegt werden, für viele verschiedene Arten, einschließlich `Activity`, `Application`, `Service`, `BroadcastReceiver`, und `ContentProvider`: 

-   [ActivityAttribute.Name](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Name/)
-   [ApplicationAttribute.Name](https://developer.xamarin.com/api/property/Android.App.ApplicationAttribute.Name/)
-   [ServiceAttribute.Name](https://developer.xamarin.com/api/property/Android.App.ServiceAttribute.Name/)
-   [BroadcastReceiverAttribute.Name](https://developer.xamarin.com/api/property/Android.Content.BroadcastReceiverAttribute.Name/)
-   [ContentProviderAttribute.Name](https://developer.xamarin.com/api/property/Android.Content.ContentProviderAttribute.Name/)

Benennen der Inhaltsfehler MD5SUM basierende wurde in Xamarin.Android 5.0 eingeführt. Weitere Informationen zum Benennen von Attribut finden Sie unter [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/). 



## <a name="implementing-interfaces"></a>Implementieren von Schnittsellen

Stehen vorkommen, dass Sie eine Android-Schnittstelle, wie z. B. implementieren müssen möglicherweise [Android.Content.IComponentCallbacks](https://developer.xamarin.com/api/type/Android.Content.IComponentCallbacks/). Da alle Android Klassen und Schnittstelle erweitern die [Android.Runtime.IJavaObject](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) -Schnittstelle, stellt sich die Frage: wie implementieren wir `IJavaObject`? 

Wurde die Frage beantwortet, oben: der Grund implementieren alle Android Typen müssen `IJavaObject` ist, damit Xamarin.Android einen Android callable Wrapper für Android, d. h. einen Java-Proxy für den angegebenen Typ bereitstellen kann. Da **monodroid.exe** sucht nur nach `Java.Lang.Object` Unterklassen und `Java.Lang.Object` implementiert `IJavaObject,` die Antwort ist offensichtlich: Unterklasse `Java.Lang.Object`: 

```csharp
class MyComponentCallbacks : Java.Lang.Object, Android.Content.IComponentCallbacks {

    public void OnConfigurationChanged (Android.Content.Res.Configuration newConfig)
    {
        // implementation goes here...
    } 

    public void OnLowMemory ()
    {
        // implementation goes here...
    }
}
```


## <a name="implementation-details"></a>Details zur Implementierung

*Der übrige Teil dieser Seite enthält Details zur Implementierung können ohne Vorankündigung geändert* (und ist nur möglich, weil Entwickler wissen, was passiert werden hier vorgestellten). 

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

public class HelloAndroid
    extends android.app.Activity
{
    static final String __md_methods;
    static {
        __md_methods = "n_onCreate:(Landroid/os/Bundle;)V:GetOnCreate_Landroid_os_Bundle_Handler\n" + "";
        mono.android.Runtime.register (
            "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, 
            Culture=neutral, PublicKeyToken=null", HelloAndroid.class, __md_methods);
    }

    public HelloAndroid ()
    {
        super ();
        if (getClass () == HelloAndroid.class)
            mono.android.TypeManager.Activate (
                "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, 
                Culture=neutral, PublicKeyToken=null", "", this, new java.lang.Object[] {  });
    }

    @Override
    public void onCreate (android.os.Bundle p0)
    {
        n_onCreate (p0);
    }

    private native void n_onCreate (android.os.Bundle p0);
}
```

Beachten Sie, dass die Basisklasse beibehalten wird, und `native` Methodendeklarationen werden bereitgestellt, für jede Methode, die in verwaltetem Code überschrieben wird. 
