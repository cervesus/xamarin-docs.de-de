---
title: Android Callable Wrapper
ms.prod: xamarin
ms.assetid: C33E15FA-1E2B-819A-C656-CA588D611492
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/15/2018
ms.openlocfilehash: 7edbdaa5a690a641523cb5baad7909ed01992aa5
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61090867"
---
# <a name="android-callable-wrappers"></a>Android Callable Wrapper

Android Callable Wrapper (ACWs) sind erforderlich, wenn die Android-Runtime, verwalteter Code aufgerufen wird. Diese Wrapper sind erforderlich, da es keine Möglichkeit gibt, um Klassen mit (der Android-Laufzeit) zur Laufzeit zu registrieren. (Insbesondere die [JNI DefineClass() Funktion](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp15986) wird von der Android-Runtime nicht unterstützt.} Android Callable Wrapper machen daher für den Mangel an Unterstützung für die Registrierung von Common Language Runtime Typen. 

*Jedes Mal, wenn* Android-Code ausführen muss eine `virtual` oder die Methode, die `overridden` oder in verwaltetem Code implementiert wird, müssen Xamarin.Android einen Java-Proxy angeben, damit, dass diese Methode in den entsprechenden verwalteten Typ gesendet wird. Diese Java-Proxy-Typen sind Java-Code, der die "dieselbe" Basisklasse und die Liste der Java-Schnittstelle als der verwaltete Typ, implementieren die gleichen Konstruktoren und deklarieren eine beliebige außer Kraft gesetzte Basisklasse und die Schnittstellenmethoden hat. 

Android callable Wrapper werden generiert, indem die **monodroid.exe** beim Programmieren der [Buildprozess](~/android/deploy-test/building-apps/build-process.md): sie werden für alle Typen, die (direkt oder indirekt) erben generiert [ Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/). 



## <a name="android-callable-wrapper-naming"></a>Benennen von Android Callable Wrapper

Die Paketnamen für Android Callable Wrapper basieren auf der MD5SUM, der die Assembly qualifizierten Namen des Typs exportiert wird. Diese Benennungskonvention Technik ermöglicht es dem gleichen den vollqualifizierten Typnamen von anderen Assemblys verfügbar werden ohne einen Fehler für die paketerstellung. 

Aufgrund dieses Benennungsschema MD5SUM können nicht Sie die Typen direkt anhand des Namens zugreifen. Beispielsweise die folgenden `adb` Befehl funktioniert nicht, da der Typname `my.ActivityType` wird standardmäßig nicht generiert: 

```shell
adb shell am start -n My.Package.Name/my.ActivityType
```

Darüber hinaus können Fehler wie folgt angezeigt, wenn Sie versuchen, einen Typ anhand des Namens verweisen:

```shell
java.lang.ClassNotFoundException: Didn't find class "com.company.app.MainActivity"
on path: DexPathList[[zip file "/data/app/com.company.App-1.apk"] ...
```

Wenn Sie *führen* erfordern Zugriff auf Typen anhand des Namens können Sie einen Namen für diesen Typ in einer Attributdeklaration deklarieren. Hier ist z. B. Code, der eine Aktivität mit dem vollqualifizierten Namen deklariert `My.ActivityType`:

```csharp
namespace My {
    [Activity]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

Die `ActivityAttribute.Name` Eigenschaft kann festgelegt werden, den Namen dieser Aktivität explizit deklarieren: 

```csharp
namespace My {
    [Activity(Name="my.ActivityType")]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

Nach dem Hinzufügen dieser eigenschaftseinstellung `my.ActivityType` kann zugegriffen werden, anhand des Namens von externem Code und `adb` Skripts. Die `Name` Attribut kann festgelegt werden, für viele verschiedene Arten, einschließlich `Activity`, `Application`, `Service`, `BroadcastReceiver`, und `ContentProvider`: 

-   [ActivityAttribute.Name](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Name/)
-   [ApplicationAttribute.Name](https://developer.xamarin.com/api/property/Android.App.ApplicationAttribute.Name/)
-   [ServiceAttribute.Name](https://developer.xamarin.com/api/property/Android.App.ServiceAttribute.Name/)
-   [BroadcastReceiverAttribute.Name](https://developer.xamarin.com/api/property/Android.Content.BroadcastReceiverAttribute.Name/)
-   [ContentProviderAttribute.Name](https://developer.xamarin.com/api/property/Android.Content.ContentProviderAttribute.Name/)

Benennen der Inhaltsfehler MD5SUM basierende wurde in Xamarin.Android 5.0 eingeführt. Weitere Informationen zum Attribut zu benennen, finden Sie unter [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/). 



## <a name="implementing-interfaces"></a>Implementieren von Schnittstellen

Es gibt Situationen, wenn Sie eine Android-Schnittstelle implementieren, z.B. möchten [Android.Content.IComponentCallbacks](https://developer.xamarin.com/api/type/Android.Content.IComponentCallbacks/). Da alle Android-Klassen und -Schnittstelle erweitert die [Android.Runtime.IJavaObject](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) -Schnittstelle, die Frage: wie wir implementieren `IJavaObject`? 

Die Frage beantwortet wurde, oben: der Grund implementieren alle Android-Typen müssen `IJavaObject` ist, damit Xamarin.Android einen Android callable Wrapper für Android, d. h. einen Java-Proxy für den angegebenen Typ bereitstellen kann. Da **monodroid.exe** sucht nur nach `Java.Lang.Object` Unterklassen, und `Java.Lang.Object` implementiert `IJavaObject,` die Antwort ist offensichtlich: Unterklasse `Java.Lang.Object`: 

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


## <a name="implementation-details"></a>Implementierungsdetails

*Im weiteren Verlauf dieser Seite finden Sie Details zur Implementierung können ohne Vorankündigung geändert* (und wird nur verwendet werden, da Entwickler neugierig werden, was passiert hier angezeigt). 

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

Beachten Sie, dass die Basisklasse beibehalten wird, und `native` Methodendeklarationen stehen für jede Methode, die in verwaltetem Code überschrieben wird. 
