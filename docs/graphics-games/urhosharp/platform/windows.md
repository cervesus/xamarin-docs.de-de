---
title: Von UrhoSharp Windows-Unterstützung
description: Dieses Dokument erläutert die Windows-Unterstützung für von UrhoSharp. Es wird beschrieben, wie Sie ein Projekt erstellen, konfigurieren und starten Urho, Integration in WPF und UWP integriert.
ms.prod: xamarin
ms.assetid: A4F36014-AE4E-4F07-A1AC-F264AAA68ACF
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 8aca028ec1015616a9884cd09b7ffa5e04f2e43d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50119604"
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

## <a name="integrated-with-windowsforms"></a>Windows.Forms integriert

### <a name="creating-a-project"></a>Erstellen eines Projekts

Erstellen Sie ein Projekt Windows.Forms, verweisen Sie Urho NuGet, und stellen Sie sicher, dass Sie die Ressourcen (die Verzeichnisse zugreifen können, die das Datenverzeichnis enthält).

### <a name="configuring-and-launching-urho-from-windowsforms"></a>Konfigurieren und starten Urho von Windows.Forms

Starten Sie das Formular Urho, finden Sie unter [vollständiges Beispiel](https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/WinForms/SamplesForm.cs)
