---
title: Wie automatisiere ich ein Android NUnit-Testprojekt?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: EA3CFCC4-2D2E-49D6-A26C-8C0706ACA045
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/29/2018
ms.openlocfilehash: 1246eeac63a0ae232396d4c2fd69d8bf516f5e3e
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027000"
---
# <a name="how-do-i-automate-an-android-nunit-test-project"></a>Wie automatisiere ich ein Android NUnit-Testprojekt?

> [!NOTE]
> In dieser Führungslinie wird das Automatisieren eines Android NUnit-Testprojekts erläutert, nicht die Schritte für ein Xamarin.UITest-Projekt. Die Führungslinien zu Xamarin.UITest finden Sie [hier](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/xamarin-android-uitest).

Wenn Sie ein Projekt für eine **Komponententest-App (Android)** in Visual Studio (oder ein Projekt für einen **Android-Komponententest** in Visual Studio für Mac) erstellen, führt dieses Projekt Ihre Tests nicht standardmäßig automatisch aus.
Zum Ausführen von NUnit-Tests auf einem Zielgerät können Sie eine [Android.App.Instrumentation](xref:Android.App.Instrumentation)-Unterklasse erstellen, die mithilfe des folgenden Befehls gestartet wird: 

```shell
adb shell am instrument 
```

Dieser Prozess wird in den folgenden Schritten erläutert:

1. Erstellen Sie eine neue Datei namens **TestInstrumentation.cs**: 

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

    In dieser Datei wird `Xamarin.Android.NUnitLite.TestSuiteInstrumentation` (von **Xamarin.Android.NUnitLite.dll**) als Unterklasse definiert, um `TestInstrumentation` zu erstellen.

2. Implementieren Sie den `TestInstrumentation`-Konstruktor und die `AddTests`-Methode. Die `AddTests`-Methode steuert, welche Tests tatsächlich ausgeführt werden.

3. Ändern Sie die `.csproj`-Datei, um **TestInstrumentation.cs** hinzuzufügen. Zum Beispiel:

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

4. Verwenden Sie den folgenden Befehl, um die Komponententests auszuführen. Ersetzen Sie `PACKAGE_NAME` durch den Paketnamen der App (der Paketname befindet sich im `/manifest/@package`-Attribut der App, das sich in der Datei **AndroidManifest.xml** befindet):

    ```shell
    adb shell am instrument -w PACKAGE_NAME/app.tests.TestInstrumentation
    ```

5. Optional können Sie die `.csproj`-Datei bearbeiten, um das MSBuild-Ziel für `RunTests` hinzuzufügen. Dies ermöglicht das Aufrufen der Komponententests mithilfe eines Befehls wie dem folgenden:

    ```shell
    msbuild /t:RunTests Project.csproj
    ```

    (Beachten Sie, dass die Verwendung dieses neuen Ziels nicht erforderlich ist. Der vorherige `adb`-Befehl kann anstelle von `msbuild` verwendet werden.)

Weitere Informationen zur Verwendung des `adb shell am instrument`-Befehls zum Ausführen von Komponententests, finden Sie im Android Developer-Artikel [Ausführen von Tests mit ADB](https://developer.android.com/studio/test/command-line.html#RunTestsDevice).

> [!NOTE]
> Seit dem Release von [Xamarin.Android 5.0](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_5/xamarin.android_5.1/index.md#Android_Callable_Wrapper_Naming) basieren die Standardpaketnamen für Android Callable Wrapper auf der MD5SUM-Hashfunktion des Namens mit Assemblyqualifikation des exportierten Typs. Dadurch kann derselbe voll qualifizierte Name von zwei verschiedenen Assemblys bereitgestellt werden, ohne dass ein Paketfehler auftritt. Stellen Sie also sicher, dass Sie die `Name`-Eigenschaft für das `Instrumentation`-Attribut verwenden, um einen lesbaren ACW- oder Klassennamen zu generieren.

_Der ACW-Name muss im obigen `adb`-Befehl verwendet werden_.
Das Umbenennen/Umgestalten der C#-Klasse erfordert daher die Bearbeitung des `RunTests`-Befehls, um den richtigen ACW-Namen zu verwenden.
