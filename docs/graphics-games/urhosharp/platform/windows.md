---
title: Von UrhoSharp Windows-Unterstützung
description: Dieses Dokument erläutert die Windows-Unterstützung für von UrhoSharp. Es beschreibt, wie ein Projekt erstellen, konfigurieren und Urho starten, integrieren in WPF und UWP integriert.
ms.prod: xamarin
ms.assetid: A4F36014-AE4E-4F07-A1AC-F264AAA68ACF
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 46a028da577a4c49e18cccb681351d7614bb196b
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832645"
---
# <a name="urhosharp-windows-support"></a>Von UrhoSharp Windows-Unterstützung

Obwohl Urho eine portable Klassenbibliothek ist und ermöglicht die gleiche API für die verschiedenen Plattformen verwendet werden, für die Spiele-Logik, Sie dennoch Urho in Ihren jeweiligen Treiber, Plattform und in einigen Fällen zu initialisieren müssen, sollten Sie bestimmte Features der Plattform nutzen .

In den nachfolgenden Seiten wird angenommen, dass `MyGame` ist eine Unterklasse von der `Application` Klasse.

**Unterstützte Architekturen:** nur 64-Bit-Windows.

Vollständige Beispiele, die zeigt, wie mit diesem finden Sie unserem [Beispiele](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples)

## <a name="standalone-project"></a>Eigenständige-Projekt

### <a name="creating-a-project"></a>Erstellen eines Projekts

Erstellen Sie ein Konsolenprojekt, verweisen Sie Urho NuGet, und stellen Sie sicher, dass Sie die Ressourcen (die Verzeichnisse zugreifen können, die das Datenverzeichnis enthält).

### <a name="configuring-and-launching-urho"></a>Konfigurieren und starten Urho

Tun Sie dies an, um die Anwendung zu starten:

```csharp
DesktopUrhoInitializer.AssetsDirectory = "../Assets";
new MyGame().Run();
```

### <a name="example"></a>Beispiel

[Vollständiges Beispiel](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/Desktop)

## <a name="integrated-with-wpf"></a>In WPF integriert

### <a name="creating-a-project"></a>Erstellen eines Projekts

Erstellen Sie ein WPF-Projekt, verweisen Sie Urho NuGet, und stellen Sie sicher, dass Sie die Ressourcen (die Verzeichnisse zugreifen können, die das Datenverzeichnis enthält).

### <a name="configuring-and-launching-urho-from-wpf"></a>Konfigurieren und starten Urho von WPF

Erstellen Sie eine Unterklasse von `Window` und Ihre Ressourcen wie folgt konfigurieren:

```csharp
    public partial class MainWindow : Window
    {
        Application currentApplication;

        public MainWindow()
        {
            InitializeComponent();
            DesktopUrhoInitializer.AssetsDirectory = @"../../Assets";
            Loaded += (s,e) => RunGame (new MyGame ());
        }

        async void RunGame(MyGame game)
        {
            var urhoSurface = new Panel { Dock = DockStyle.Fill };
            WindowsFormsHost.Child = urhoSurface;
            WindowsFormsHost.Focus();
            urhoSurface.Focus();
            await Task.Yield();
            var appOptions = new ApplicationOptions(assetsFolder: "Data")
                {
                    ExternalWindow = RunInSdlWindow.IsChecked.Value ? IntPtr.Zero : urhoSurface.Handle,
                    LimitFps = false, //true means "limit to 200fps"
                };
            currentApplication = Urho.Application.CreateInstance(value.Type, appOptions);
            currentApplication.Run();
        }
    }
```

### <a name="example"></a>Beispiel

[Vollständiges Beispiel](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/WPF)

## <a name="integrated-with-uwp"></a>Mit der UWP integriert

### <a name="creating-a-project"></a>Erstellen eines Projekts

Erstellen Sie einer UWP-Projekt, verweisen Sie Urho NuGet, und stellen Sie sicher, dass Sie die Ressourcen (die Verzeichnisse zugreifen können, die das Datenverzeichnis enthält).

### <a name="configuring-and-launching-urho-from-uwp"></a>Konfiguriert und startet Urho aus der UWP

Erstellen Sie eine Unterklasse von `Window` und Ihre Ressourcen wie folgt konfigurieren:

```csharp
    {
        InitializeComponent();
        GameTypes = typeof(Sample).GetTypeInfo().Assembly.GetTypes()
            .Where(t => t.GetTypeInfo().IsSubclassOf(typeof(Application)) && t != typeof(Sample))
            .Select((t, i) => new TypeInfo(t, $"{i + 1}. {t.Name}", ""))
            .ToArray();
        DataContext = this;
        Loaded += (s, e) => RunGame (new MyGame ());
    }

    public void RunGame(TypeInfo value)
    {
        //at this moment, UWP supports assets only in pak files (see PackageTool)
        currentApplication = UrhoSurface.Run(value.Type, "Data.pak");
    }
}
```

### <a name="example"></a>Beispiel

[Vollständiges Beispiel](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/UWP)

## <a name="integrated-with-windows-forms"></a>In Windows Forms integriert

### <a name="creating-a-project"></a>Erstellen eines Projekts

Erstellen Sie ein Windows Forms-Projekt, verweisen Sie Urho NuGet, und stellen Sie sicher, dass Sie die Ressourcen (die Verzeichnisse zugreifen können, die das Datenverzeichnis enthält).

### <a name="configuring-and-launching-urho-from-windowsforms"></a>Konfigurieren und starten Urho von Windows.Forms

Starten Sie das Formular Urho, finden Sie unter [vollständiges Beispiel](https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/WinForms/SamplesForm.cs)
