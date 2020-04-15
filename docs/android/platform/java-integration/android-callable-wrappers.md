---
title: Android Callable Wrapper für Xamarin.Android
ms.prod: xamarin
ms.assetid: C33E15FA-1E2B-819A-C656-CA588D611492
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/15/2018
ms.openlocfilehash: 7278fd624bb3147c2e1a1a1a79adde68813a9888
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "73020151"
---
# <a name="android-callable-wrappers-for-xamarinandroid"></a>Android Callable Wrapper für Xamarin.Android

Android Callable Wrapper (ACWs) sind immer dann erforderlich, wenn die Android-Runtime verwalteten Code aufruft. Diese Wrapper sind erforderlich, da es keine Möglichkeit gibt, Klassen mit ART (Android-Runtime) zur Laufzeit zu registrieren. (Insbesondere wird die [JNI-Funktion „DefineClass()“](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp15986) von der Android-Runtime nicht unterstützt.} Android Callable Wrapper kompensieren daher die fehlende Unterstützung für die Typregistrierung zur Laufzeit. 

*Jedes Mal, wenn* Android-Code eine `virtual`- oder Schnittstellenmethode ausführen muss, die in verwaltetem Code überschrieben (`overridden`) oder implementiert wird, muss Xamarin.Android einen Java-Proxy bereitstellen, damit diese Methode an den entsprechenden verwalteten Typ weitergeleitet wird. Diese Java-Proxytypen sind Java-Code, der über die „gleiche“ Basisklasse und Java-Schnittstellenliste wie der verwaltete Typ verfügt, dieselben Konstruktoren implementiert und alle überschriebenen Basisklassen- und Schnittstellenmethoden deklariert. 

Android Callable Wrapper werden vom **monodroid.exe**-Programm im Rahmen des [Buildprozesses](~/android/deploy-test/building-apps/build-process.md) generiert: sie werden für alle Typen generiert, die (direkt oder indirekt) [Java.Lang.Object](xref:Java.Lang.Object) erben. 

## <a name="android-callable-wrapper-naming"></a>Android Callable Wrapper-Namen

Paketnamen für Android Callable Wrapper basieren auf MD5SUM des Namens mit Assemblyqualifikation des exportierten Typs. Dieses Benennungsverfahren ermöglicht es, dass derselbe voll qualifizierte Typname von unterschiedlichen Assemblys zur Verfügung gestellt werden kann, ohne dass ein Fehler bei der Paketerstellung auftritt. 

Aufgrund dieses MD5SUM-Benennungsschemas ist kein direkter Zugriff auf Typen über den Namen möglich. Der folgende `adb`-Befehl funktioniert beispielsweise nicht, da der Typname `my.ActivityType` standardmäßig nicht generiert wird: 

```shell
adb shell am start -n My.Package.Name/my.ActivityType
```

Außerdem werden möglicherweise Fehler wie die folgenden angezeigt, wenn Sie versuchen, über den Namen auf einen Typ zu verweisen:

```shell
java.lang.ClassNotFoundException: Didn't find class "com.company.app.MainActivity"
on path: DexPathList[[zip file "/data/app/com.company.App-1.apk"] ...
```

Wenn ein Zugriff auf Typen über den Namen *notwendig* ist, können Sie einen Namen für den betreffenden Typ in einer Attributdeklaration deklarieren. Der folgende Code deklariert beispielsweise eine Aktivität mit dem voll qualifizierten Namen `My.ActivityType`:

```csharp
namespace My {
    [Activity]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

Um den Namen dieser Aktivität explizit zu deklarieren, kann die `ActivityAttribute.Name`-Eigenschaft festgelegt werden: 

```csharp
namespace My {
    [Activity(Name="my.ActivityType")]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

Nachdem diese Eigenschaftseinstellung hinzugefügt wurde, ist von externem Code und `adb`-Skripts aus ein Zugriff auf `my.ActivityType` anhand des Namens möglich. Das `Name`-Attribut kann für viele verschiedene Typen festgelegt werden, z. B. `Activity`, `Application`, `Service`, `BroadcastReceiver` und `ContentProvider`: 

- [ActivityAttribute.Name](xref:Android.App.ActivityAttribute.Name)
- [ApplicationAttribute.Name](xref:Android.App.ApplicationAttribute.Name)
- [ServiceAttribute.Name](xref:Android.App.ServiceAttribute.Name)
- [BroadcastReceiverAttribute.Name](xref:Android.Content.BroadcastReceiverAttribute.Name)
- [ContentProviderAttribute.Name](xref:Android.Content.ContentProviderAttribute.Name)

MD5SUM-basierte ACW-Namen wurden in Xamarin.Android 5.0 eingeführt. Weitere Informationen zu Attributnamen finden Sie unter [RegisterAttribute](xref:Android.Runtime.RegisterAttribute). 

## <a name="implementing-interfaces"></a>Implementieren von Schnittstellen

Bisweilen müssen Sie möglicherweise eine Android-Schnittstelle implementieren, z. B. [Android.Content.IComponentCallbacks](xref:Android.Content.IComponentCallbacks). Da alle Android-Klassen und -Schnittstellen die [Android.Runtime.IJavaObject](xref:Android.Runtime.IJavaObject)-Schnittstelle erweitern, stellt sich die Frage, wie `IJavaObject` implementiert wird. 

Die Frage wurde oben beantwortet: der Grund dafür, dass alle Android-Typen `IJavaObject` implementieren müssen, ist, dass Xamarin.Android einen Android Callable Wrapper für Android bereitstellt, d. h. einen Java-Proxy für den angegebenen Typ. Da **monodroid.exe** nur nach `Java.Lang.Object`-Unterklassen sucht und `IJavaObject,` von `Java.Lang.Object` implementiert wird, ist die Antwort offensichtlich: die `Java.Lang.Object`-Unterklasse: 

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

*Im restlichen Teil dieser Seite finden Sie Implementierungsdetails, die sich ohne vorherige Ankündigung ändern können* (und hier nur vorhanden sind, da Entwickler neugierig sind, was passiert). 

Betrachten Sie beispielsweise den folgenden C#-Code:

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

Beachten Sie, dass die Basisklasse beibehalten wird und für alle Methoden, die innerhalb von verwaltetem Code überschrieben werden, `native`-Methodendeklarationen bereitgestellt werden. 
