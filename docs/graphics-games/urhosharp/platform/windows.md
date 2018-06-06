---
title: UrhoSharp Windows-Unterstützung
description: Dieses Dokument beschreibt die Windows-Unterstützung für UrhoSharp. Es wird beschrieben, wie Sie ein Projekt erstellen, konfigurieren und starten Urho, WPF integriert und universelle Windows-Plattform integriert.
ms.prod: xamarin
ms.assetid: A4F36014-AE4E-4F07-A1AC-F264AAA68ACF
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: 094eaf0ebe84ce8c1771bd6481ee897463349856
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783232"
---
# <a name="urhosharp-windows-support"></a>UrhoSharp Windows-Unterstützung

Während Urho eine portable Klassenbibliothek ist, und ermöglicht die gleiche API über die verschiedenen Plattform verwendet werden, für die Spiellogik Sie weiterhin Urho in Ihren jeweiligen Treiber, Plattform und in einigen Fällen zu initialisieren müssen, sollten Sie bestimmte Funktionen der Plattform nutzen .

In den nachfolgenden Seiten wird angenommen, dass `MyGame` ist eine Unterklasse von der `Application` Klasse.

**Unterstützte Architekturen:** nur 64-Bit-Windows.

Sehen Sie vollständige Beispiele wie dies in unserem [Beispiele](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples)

## <a name="standalone-project"></a>Eigenständiges Projekt

### <a name="creating-a-project"></a>Erstellen eines Projekts

Erstellen eines Konsolenprojekts, verweisen die Urho NuGet, und stellen Sie sicher, dass Sie die Ressourcen (die Verzeichnisse, die das Datenverzeichnis enthält) zugreifen können.

### <a name="configuring-and-launching-urho"></a>Konfigurieren und starten Urho

Um die Anwendung zu starten, tun Sie Folgendes aus:

```csharp
DesktopUrhoInitializer.AssetsDirectory = "../Assets";
new MyGame().Run();
```

### <a name="example"></a>Beispiel

[Vollständiges Beispiel](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/Desktop)

## <a name="integrated-with-wpf"></a>In WPF integriert

### <a name="creating-a-project"></a>Erstellen eines Projekts

Erstellen Sie ein WPF-Projekt, verweisen Sie die Urho NuGet, und stellen Sie sicher, dass Sie die Ressourcen (die Verzeichnisse zugreifen können, die das Datenverzeichnis enthält).

### <a name="configuring-and-launching-urho-from-wpf"></a>Konfigurieren und starten Urho von WPF

Erstellen Sie eine Unterklasse von `Window` und Ihre Objekte wie folgt konfigurieren:

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

## <a name="integrated-with-uwp"></a>Universelle Windows-Plattform integriert

### <a name="creating-a-project"></a>Erstellen eines Projekts

Erstellen Sie uwp-Projekt, verweisen Sie die Urho NuGet, und stellen Sie sicher, dass Sie die Ressourcen (die Verzeichnisse zugreifen können, die das Datenverzeichnis enthält).

### <a name="configuring-and-launching-urho-from-uwp"></a>Konfigurieren und starten Urho von UWP

Erstellen Sie eine Unterklasse von `Window` und Ihre Objekte wie folgt konfigurieren:

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

Erstellen Sie ein Projekt Windows.Forms, verweisen Sie die Urho NuGet, und stellen Sie sicher, dass Sie die Ressourcen (die Verzeichnisse, die das Datenverzeichnis enthält) zugreifen können.

### <a name="configuring-and-launching-urho-from-windowsforms"></a>Konfigurieren und Starten von Windows.Forms Urho

Starten Sie das Formular Urho, finden Sie unter [vollständiges Beispiel](https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/WinForms/SamplesForm.cs)
