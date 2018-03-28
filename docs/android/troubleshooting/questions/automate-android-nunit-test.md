---
title: Wie wird automatisiert ein Testprojekt für Android NUnit?
ms.topic: article
ms.prod: xamarin
ms.assetid: EA3CFCC4-2D2E-49D6-A26C-8C0706ACA045
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: acb213e8c73013bc9b2482afb45296c4e1f61ab5
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="how-do-i-automate-an-android-nunit-test-project"></a>Wie wird automatisiert ein Testprojekt für Android NUnit?

> [!NOTE]
> Diese Anleitung enthält die Schritte zum Einrichten einer Android NUnit Testprojekt befindet, nicht in einem Projekt Xamarin.UITest. Xamarin.UITest Handbücher verwendbaren [hier](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest).

Wenn Sie ein Komponententestprojekt für Android in Visual Studio für Mac oder eine Einheit Test-App (Android) in Visual Studio erstellen, wird standardmäßig er nicht automatisch die Tests ausgeführt.
Zum Ausführen von NUnit-Tests auf einem Zielgerät verwenden wir ein `Android.App.Instrumentation` -Unterklasse, die erstellt und mit ausgeführt werden kann die `adb shell am instrument` Befehl.

Wir erstellen Sie zunächst die **TestInstrumentation.cs** -Datei, die eine Unterklasse von erstellt `Xamarin.Android.NUnitLite.TestSuiteInstrumentation` (deklariert `Xamarin.Android.NUnitLite.dll`). Die `TestInstrumentation(IntPtr, JniHandleOwnership)` Konstruktor _müssen_ bereitgestellt werden, und die virtuellen `AddTests()` Methode muss überschrieben werden.
`AddTests()` Steuert, welche Tests tatsächlich ausgeführt werden. Diese Datei wird zum größten Teil Textbaustein.

Als Nächstes wird die `.csproj` geändert werden muss, um hinzufügen `TestInstrumentation.cs`.

Optional, die `.csproj` kann geändert werden, um das Hinzufügen der `RunTests` MSBuild-Ziels, das Möglichkeit bietet, die Komponententests als aufrufen:

```shell
msbuild /t:RunTests Project.csproj
```

Es ist nicht erforderlich, verwenden ein neues Ziel; der entsprechende Adb-Befehl kann stattdessen verwendet werden:

```shell
adb shell am instrument -w @PACKAGE_NAME@/app.tests.TestInstrumentation
```

Ersetzen Sie `@PACKAGE\_NAME@` je nach Bedarf; es ist der Wert vorhanden ist, in der **AndroidManifest.xml** `/manifest/@package` Attribut.


> [!NOTE]
> *Wichtige*: mit der [Xamarin.Android 5.0](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Android_Callable_Wrapper_Naming) freigeben, die Standardnamen für das Paket für Android Callable Wrapper auf die von der Assembly qualifizierte Name des Typs, der zu exportierenden MD5SUM basieren soll. Dies ermöglicht den gleichen vollqualifizierten Namen aus zwei verschiedenen Assemblys bereitgestellt werden und nicht erhalte die Fehlermeldung verpacken. So stellen Sie sicher, dass Sie verwenden die \`Namen\` Eigenschaft auf die \`Instrumentation\` Standardtitel zum Generieren eines lesbaren Inhaltsfehler "oder" Class-Namens.

_Der Name der Inhaltsfehler muss verwendet werden, der `adb` Befehl_. Umbenennen von/Umgestaltung der C#-Klasse wird daher erfordern ändern die `RunTests` Befehl aus, um den richtigen Inhaltsfehler Namen verwenden.

Ergänzungen zur CSPROJ-Datei:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Project DefaultTargets="Build" ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <ItemGroup>
        <Compile Include="TestInstrumentation.cs" />
    </ItemGroup>
    <Target Name="RunTests" DependsOnTargets="_ValidateAndroidPackageProperties">
        <Exec Command="&quot;$(_AndroidPlatformToolsDirectory)adb&quot; $(AdbTarget) $(AdbOptions) shell am instrument -w $(_AndroidPackage)/app.tests.TestInstrumentation" />
    </Target>
</Project>
```

**TestInstruments.cs**:

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

