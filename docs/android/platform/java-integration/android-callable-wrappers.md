---
title: Android Callable Wrapper für xamarin. Android
ms.prod: xamarin
ms.assetid: C33E15FA-1E2B-819A-C656-CA588D611492
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/15/2018
ms.openlocfilehash: b55cffc19eec5ae95a0a0aba8053bdaaa49e7747
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70761479"
---
# <a name="android-callable-wrappers-for-xamarinandroid"></a>Android Callable Wrapper für xamarin. Android

Android Callable Wrapper (acws) sind immer dann erforderlich, wenn die Android-Laufzeit verwalteten Code aufruft. Diese Wrapper sind erforderlich, da es keine Möglichkeit gibt, Klassen bei der Kunst (Android-Laufzeit) zur Laufzeit zu registrieren. (Insbesondere die [jni defineClass ()-Funktion](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp15986) wird von der Android-Laufzeit nicht unterstützt.} Android Callable Wrapper bilden daher die fehlende Unterstützung für die Laufzeit-Typregistrierung. 

*Jedes Mal* Android-Code muss eine `virtual` -oder-Schnittstellen Methode ausführen, `overridden` die in verwaltetem Code implementiert oder implementiert wird. xamarin. Android muss einen Java-Proxy bereitstellen, damit diese Methode an den entsprechenden verwalteten Typ weitergeleitet wird. Diese Java-Proxy Typen sind Java-Code, der über die gleiche Basisklasse und Java-Schnittstellen Liste wie der verwaltete Typ verfügt. dabei werden dieselben Konstruktoren implementiert, und es werden alle überschriebenen Basisklassen-und Schnittstellen Methoden deklariert. 

Aufrufbare Android-Wrapper werden während des [Buildprozesses](~/android/deploy-test/building-apps/build-process.md) vom Programm **monodroid.exe** generiert: Sie werden für alle Typen generiert, die (direkt oder indirekt) [Java.Lang.Object](xref:Java.Lang.Object) erben. 

## <a name="android-callable-wrapper-naming"></a>Benennung von Android Callable Wrapper

Paketnamen für von Android Callable Wrapper basieren auf der md5sum des durch die Assembly qualifizierten Namens des exportierten Typs. Diese Benennungs Technik ermöglicht es, dass derselbe voll qualifizierte Typname von unterschiedlichen Assemblys zur Verfügung gestellt werden kann, ohne dass ein Verpackungsfehler eingeführt wird. 

Aufgrund dieses "md5sum Naming"-Schemas können Sie nicht direkt über den Namen auf Ihre Typen zugreifen. Beispielsweise kann der folgende `adb` Befehl nicht ausgeführt werden, da der Typname `my.ActivityType` nicht standardmäßig generiert wird: 

```shell
adb shell am start -n My.Package.Name/my.ActivityType
```

Außerdem werden möglicherweise Fehler wie die folgenden angezeigt, wenn Sie versuchen, mit dem Namen auf einen Typ zu verweisen:

```shell
java.lang.ClassNotFoundException: Didn't find class "com.company.app.MainActivity"
on path: DexPathList[[zip file "/data/app/com.company.App-1.apk"] ...
```

Wenn Sie den Zugriff auf Typen nach *Namen benötigen, können Sie einen* Namen für diesen Typ in einer Attribut Deklaration deklarieren. Hier ist beispielsweise der Code, der eine Aktivität mit dem voll qualifizierten Namen `My.ActivityType`deklariert:

```csharp
namespace My {
    [Activity]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

Die `ActivityAttribute.Name` -Eigenschaft kann festgelegt werden, um den Namen dieser Aktivität explizit zu deklarieren: 

```csharp
namespace My {
    [Activity(Name="my.ActivityType")]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

Nachdem diese Eigenschafts Einstellung hinzugefügt `my.ActivityType` wurde, können Sie anhand des Namens aus dem externen `adb` Code und aus Skripts darauf zugreifen. Das `Name` -Attribut kann für viele verschiedene Typen wie `Activity`, `Application`, `Service`, `BroadcastReceiver`und `ContentProvider`festgelegt werden: 

- [ActivityAttribute.Name](xref:Android.App.ActivityAttribute.Name)
- [ApplicationAttribute.Name](xref:Android.App.ApplicationAttribute.Name)
- [ServiceAttribute.Name](xref:Android.App.ServiceAttribute.Name)
- [BroadcastReceiverAttribute.Name](xref:Android.Content.BroadcastReceiverAttribute.Name)
- [ContentProviderAttribute.Name](xref:Android.Content.ContentProviderAttribute.Name)

Die md5sum-basierte ACW-Benennung wurde in xamarin. Android 5,0 eingeführt. Weitere Informationen zur Attribut Benennung finden Sie unter [RegisterAttribute](xref:Android.Runtime.RegisterAttribute). 

## <a name="implementing-interfaces"></a>Implementieren von Schnittstellen

Es gibt Zeiten, in denen Sie möglicherweise eine Android-Schnittstelle implementieren müssen, z. b. [Android. Content. icomponentcallbacks](xref:Android.Content.IComponentCallbacks). Da alle Android-Klassen und-Schnittstellen die [Android. Runtime. ijavaobject](xref:Android.Runtime.IJavaObject) -Schnittstelle erweitern, wird die Frage `IJavaObject`gestellt: wie implementieren wir? 

Die Frage wurde oben beantwortet: der Grund, warum alle Android-Typen `IJavaObject` implementiert werden müssen, ist, dass xamarin. Android einen Android Callable Wrapper für Android bereitstellt, d. h. einen Java-Proxy für den angegebenen Typ. Da **monodroid. exe** nur nach `Java.Lang.Object` Unterklassen sucht und `Java.Lang.Object` die Antwort `IJavaObject,` implementiert, ist offensichtlich: unter `Java.Lang.Object`Klasse: 

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

*Der Rest dieser Seite enthält Implementierungsdetails, die sich ohne vorherige Ankündigung ändern können* . (und wird hier nur vorgestellt, weil Entwickler neugierig sind, was passiert). 

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

Beachten Sie, dass die Basisklasse beibehalten wird `native` und Methoden Deklarationen für jede Methode bereitgestellt werden, die innerhalb von verwaltetem Code überschrieben wird. 
