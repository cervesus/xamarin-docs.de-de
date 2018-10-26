---
title: Wie automatisiere ich ein Android NUnit-Testprojekt?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: EA3CFCC4-2D2E-49D6-A26C-8C0706ACA045
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/29/2018
ms.openlocfilehash: b785ef171d2cb00d4f8f5a17f37d49de17fd3da9
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106857"
---
# <a name="how-do-i-automate-an-android-nunit-test-project"></a>Wie automatisiere ich ein Android NUnit-Testprojekt?

> [!NOTE]
> Dieses Handbuch wird erläutert, wie ein Android NUnit-Testprojekt, keines Xamarin.UITest-Projekts zu automatisieren. Xamarin.UITest-Anleitungen finden Sie [hier](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest).

Bei der Erstellung einer **Komponententest-App (Android)** Projekt in Visual Studio (oder **Android Komponententest** Projekt in Visual Studio für Mac), dieses Projekt werden die Tests werden standardmäßig nicht automatisch ausgeführt.
Um die NUnit-Tests auf einem Zielgerät ausgeführt werden, können Sie erstellen eine [Android.App.Instrumentation](https://developer.xamarin.com/api/type/Android.App.Instrumentation/) Unterklasse, die gestartet wird, mithilfe des folgenden Befehls: 

```shell
adb shell am instrument 
```

Die folgenden Schritte erläutern diesen Prozess:

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
    In dieser Datei [Xamarin.Android.NUnitLite.TestSuiteInstrumentation](https://developer.xamarin.com/api/type/Xamarin.Android.NUnitLite.TestSuiteInstrumentation/) (von **Xamarin.Android.NUnitLite.dll**) ist eine Unterklasse erstellen `TestInstrumentation`.

2.  Implementieren der [TestInstrumentation](https://developer.xamarin.com/api/constructor/Xamarin.Android.NUnitLite.TestSuiteInstrumentation.TestSuiteInstrumentation/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) Konstruktor und die [AddTests](https://developer.xamarin.com/api/member/Xamarin.Android.NUnitLite.TestSuiteInstrumentation.AddTests%28%29) Methode. Die `AddTests` Methode-Steuerelemente, die tatsächlich welche Tests ausgeführt werden.

3.  Ändern der `.csproj` hinzuzufügende Datei **TestInstrumentation.cs**. Zum Beispiel:

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

3.  Verwenden Sie den folgenden Befehl aus, um die Komponententests auszuführen. Ersetzen Sie dies `PACKAGE_NAME` mit den Namen der app-Paket (der Paketnamen finden Sie in der app `/manifest/@package` Attribut befindet sich in **"androidmanifest.xml"**):

    ```shell
    adb shell am instrument -w PACKAGE_NAME/app.tests.TestInstrumentation
    ```

4.  Optional können Sie ändern die `.csproj` hinzuzufügenden Datei zu den `RunTests` MSBuild-Ziel. Dadurch kann die Komponententests mit einem Befehl wie folgt aufrufen:

    ```shell
    msbuild /t:RunTests Project.csproj
    ```
    (Beachten Sie, dass dieses neue Ziel nicht erforderlich ist, die frühere `adb` Befehl kann verwendet werden, anstelle von `msbuild`.)

Weitere Informationen zur Verwendung der `adb shell am instrument` Befehl zum Ausführen von Komponententests finden Sie unter der Android-Entwickler [Ausführen von Tests mit ADB](https://developer.android.com/studio/test/command-line.html#RunTestsDevice) Thema.


> [!NOTE]
> Mit der [Xamarin.Android 5.0](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Android_Callable_Wrapper_Naming) freigeben, die Standardnamen für das Paket für Android Callable Wrapper die MD5SUM, der die Assembly qualifizierten Namen des exportierten Typs basiert. Dies ermöglicht den gleichen den vollqualifizierten Namen, die aus zwei verschiedenen Assemblys bereitgestellt werden und keinen Fehler für die paketerstellung. So stellen Sie sicher, dass Sie verwenden die `Name` Eigenschaft für die `Instrumentation` Attribut um einen lesbaren Namen der Inhaltsfehler "Class" zu generieren.

_Der Name der Inhaltsfehler muss verwendet werden, der `adb` obenstehende_.
Refactoring/Umbenennen der C# Klasse benötigen daher ändern die `RunTests` Befehl aus, um den richtigen Namen der Inhaltsfehler verwenden.

