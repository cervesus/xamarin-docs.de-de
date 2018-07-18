---
title: Wie wird automatisiert ein Testprojekt für Android NUnit?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: EA3CFCC4-2D2E-49D6-A26C-8C0706ACA045
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/29/2018
ms.openlocfilehash: f63a25f36682038b7fcd85d711d980b9e3ec869d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30763770"
---
# <a name="how-do-i-automate-an-android-nunit-test-project"></a>Wie wird automatisiert ein Testprojekt für Android NUnit?

> [!NOTE]
> Dieses Handbuch erläutert, wie Sie ein Testprojekt Android NUnit, nicht in einem Projekt Xamarin.UITest automatisieren. Xamarin.UITest Handbücher verwendbaren [hier](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest).

Beim Erstellen einer **Unit Test-App (Android)** in Visual Studio-Projekt (oder **Android Komponententest** Projekt in Visual Studio für Mac), dieses Projekt wird standardmäßig die Tests nicht automatisch ausgeführt.
Zum Ausführen von NUnit-Tests auf einem Zielgerät, erstellen Sie eine [Android.App.Instrumentation](https://developer.xamarin.com/api/type/Android.App.Instrumentation/) Unterklasse, die gestartet wird, mithilfe des folgenden Befehls: 

```shell
adb shell am instrument 
```

Die folgenden Schritte werden dieser Prozess beschrieben:

1.  Erstellen Sie eine neue Datei namens **TestInstrumentation.cs**: 

    ```cs 
    using System;
    using System.Reflection;
    using Android.App;
    using Android.Content;
    using Android.Runtime;
    using Xamarin.Android.NUnitLite;
     
    namespace App.Tests {
     
        [Instrumentation(Name="app.tests.TestInstrumentation")]
        public class TestInstrumentation : TestSuiteInstrumentation {
     
            public TestInstrumentation (IntPtr handle, JniHandleOwnership transfer) : base (handle, transfer)
            {
            }
     
            protected override void AddTests ()
            {
                AddTest (Assembly.GetExecutingAssembly ());
            }
        }
    }
    ```
    In dieser Datei [Xamarin.Android.NUnitLite.TestSuiteInstrumentation](https://developer.xamarin.com/api/type/Xamarin.Android.NUnitLite.TestSuiteInstrumentation/) (aus **Xamarin.Android.NUnitLite.dll**) ist eine Unterklasse erstellen `TestInstrumentation`.

2.  Implementieren der [TestInstrumentation](https://developer.xamarin.com/api/constructor/Xamarin.Android.NUnitLite.TestSuiteInstrumentation.TestSuiteInstrumentation/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) Konstruktor und die [AddTests](https://developer.xamarin.com/api/member/Xamarin.Android.NUnitLite.TestSuiteInstrumentation.AddTests%28%29) Methode. Die `AddTests` Methode steuert, welche Tests tatsächlich ausgeführt werden.

3.  Ändern der `.csproj` hinzuzufügenden Datei **TestInstrumentation.cs**. Zum Beispiel:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project DefaultTargets="Build" ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
        ...
        <ItemGroup>
            <Compile Include="TestInstrumentation.cs" />
        </ItemGroup>
        <Target Name="RunTests" DependsOnTargets="_ValidateAndroidPackageProperties">
            <Exec Command="&quot;$(_AndroidPlatformToolsDirectory)adb&quot; $(AdbTarget) $(AdbOptions) shell am instrument -w $(_AndroidPackage)/app.tests.TestInstrumentation" />
        </Target>
        ...
    </Project>
    ```

3.  Verwenden Sie den folgenden Befehl aus, um die Komponententests auszuführen. Ersetzen Sie `PACKAGE_NAME` mit Namen für die app-Pakets (der Paketnamen finden Sie in der app `/manifest/@package` Attribut befindet sich im **AndroidManifest.xml**):

    ```shell
    adb shell am instrument -w PACKAGE_NAME/app.tests.TestInstrumentation
    ```

4.  Optional können Sie ändern die `.csproj` hinzuzufügenden Datei die `RunTests` MSBuild-Ziel. Dadurch können die Komponententests mit einem Befehl wie folgt aufrufen:

    ```shell
    msbuild /t:RunTests Project.csproj
    ```
    (Beachten Sie, dass dieses neue Ziel nicht erforderlich ist, die frühere `adb` Befehl kann verwendet werden, anstelle von `msbuild`.)

Weitere Informationen zum Verwenden der `adb shell am instrument` Befehl zum Ausführen von Komponententests finden Sie unter der Android-Entwickler [Ausführen von Tests mit ADB](https://developer.android.com/studio/test/command-line.html#RunTestsDevice) Thema.


> [!NOTE]
> Mit der [Xamarin.Android 5.0](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Android_Callable_Wrapper_Naming) freigeben, die Standardnamen für das Paket für Android Callable Wrapper auf die von der Assembly qualifizierte Name des Typs, der zu exportierenden MD5SUM basieren soll. Dies ermöglicht den gleichen vollqualifizierten Namen aus zwei verschiedenen Assemblys bereitgestellt werden und nicht erhalte die Fehlermeldung verpacken. So stellen Sie sicher, dass Sie verwenden die `Name` Eigenschaft auf die `Instrumentation` Attribut um einen lesbaren Inhaltsfehler "oder" Class Namen generieren.

_Der Name der Inhaltsfehler muss verwendet werden, der `adb` oben aufgeführten Befehl_.
Umbenennen von/Umgestaltung der C#-Klasse wird daher erfordern ändern die `RunTests` Befehl aus, um den richtigen Inhaltsfehler Namen verwenden.

