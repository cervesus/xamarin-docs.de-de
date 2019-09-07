---
title: Wie automatisiere ich ein Android NUnit-Testprojekt?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: EA3CFCC4-2D2E-49D6-A26C-8C0706ACA045
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/29/2018
ms.openlocfilehash: e96f9a0ce4d1eec9bf853faceeb85a2acb4840af
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70761025"
---
# <a name="how-do-i-automate-an-android-nunit-test-project"></a>Wie automatisiere ich ein Android NUnit-Testprojekt?

> [!NOTE]
> In diesem Handbuch wird erläutert, wie ein Android nunit-Testprojekt und kein xamarin. UITest-Projekt automatisiert werden. Xamarin. UITest-Anleitungen finden Sie [hier](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest).

Wenn Sie ein Komponenten **Test-App-Projekt (Android)** in Visual Studio (oder einem Android-Komponenten **Test** Projekt in Visual Studio für Mac) erstellen, werden die Tests in diesem Projekt nicht automatisch standardmäßig ausgeführt.
Zum Ausführen von nunit-Tests auf einem Zielgerät können Sie eine Unterklasse " [Android. app. Instrumentation](xref:Android.App.Instrumentation) " erstellen, die mithilfe des folgenden Befehls gestartet wird: 

```shell
adb shell am instrument 
```

Dieser Vorgang wird in den folgenden Schritten erläutert:

1. Erstellen Sie eine neue Datei mit dem Namen **TestInstrumentation.cs**: 

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

    In dieser Datei `Xamarin.Android.NUnitLite.TestSuiteInstrumentation` wird (aus **xamarin. Android. nunitlite. dll**) zur Erstellung `TestInstrumentation`von subklassifiziert.

2. Implementieren Sie `TestInstrumentation` den-Konstruktor `AddTests` und die-Methode. Die `AddTests` -Methode steuert, welche Tests tatsächlich ausgeführt werden.

3. Ändern Sie `.csproj` die Datei, um **TestInstrumentation.cs**hinzuzufügen. Beispiel:

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

4. Verwenden Sie den folgenden Befehl, um die Komponententests auszuführen. Ersetzen `PACKAGE_NAME` Sie durch den Paketnamen der APP (der Paketname befindet sich im APP- `/manifest/@package` Attribut, das sich in " **androidmanifest. XML**" befindet):

    ```shell
    adb shell am instrument -w PACKAGE_NAME/app.tests.TestInstrumentation
    ```

5. Optional können Sie die `.csproj` Datei ändern, um das `RunTests` MSBuild-Ziel hinzuzufügen. Dadurch können die Komponententests mit einem Befehl wie dem folgenden aufgerufen werden:

    ```shell
    msbuild /t:RunTests Project.csproj
    ```

    (Beachten Sie, dass die Verwendung dieses neuen Ziels nicht erforderlich ist `adb` . der frühere Befehl kann anstelle `msbuild`von verwendet werden.)

Weitere Informationen zur Verwendung des `adb shell am instrument` -Befehls zum Ausführen von-Komponententests finden Sie im Thema Android-Entwickler, die [Tests mit ADB](https://developer.android.com/studio/test/command-line.html#RunTestsDevice) ausführen.

> [!NOTE]
> Bei der [xamarin. Android 5,0](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_5/xamarin.android_5.1/index.md#Android_Callable_Wrapper_Naming) -Version basieren die standardmäßigen Paketnamen für Android Callable Wrapper auf der md5sum des durch die Assembly qualifizierten Namens des exportierten Typs. Dadurch kann derselbe voll qualifizierte Name von zwei verschiedenen Assemblys bereitgestellt werden, und es wird kein Paketfehler ausgegeben. Stellen Sie daher sicher, dass Sie `Name` die-Eigenschaft `Instrumentation` für das-Attribut verwenden, um einen lesbaren ACW/Class-Namen zu generieren.

_Der ACW-Name muss im `adb` obigen Befehl verwendet werden_.
Wenn Sie die C# Klasse umbenennen/umgestalten, muss `RunTests` der Befehl so geändert werden, dass Sie den korrekten ACW-Namen verwendet.
