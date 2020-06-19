---
title: Verwenden von urhusharp inXamarin.Forms
description: In diesem Artikel wird erläutert, wie urhusharp verwendet werden kann, um einer Anwendung 3D-Grafiken Xamarin.Forms für die erweiterte Visualisierung hinzuzufügen.
ms.prod: xamarin
ms.assetid: 0646B98E-CC04-4537-9715-9F82338FD7FF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/11/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: fbf717092da7f77e265803fae87efb5bf0e9876f
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84574365"
---
# <a name="using-urhosharp-in-xamarinforms"></a>Verwenden von urhusharp inXamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/urho-samples/tree/master/FormsSample)

## <a name="what-is-urhosharp"></a>Was ist urhusharp?

[Urhusharp](~/graphics-games/urhosharp/index.md) ist eine leistungsstarke 3D-Engine für xamarin-und .NET-Entwickler. In der [Einführung](~/graphics-games/urhosharp/introduction.md) werden weitere Informationen zur urhosharp-Bibliothek erläutert. in [diesen Anmerkungen](~/graphics-games/urhosharp/using.md) wird beschrieben, wie Szenen und Aktionen programmiert werden.

Urhusharp kann zum rendergrafiegrafik in Anwendungen verwendet werden Xamarin.Forms .
In diesem [Beispiel](https://github.com/xamarin/urho-samples/tree/master/FormsSample) wird veranschaulicht, wie urhusharp verwendet werden kann, um ein interaktives 3D-Diagramm zu erstellen:

![](urhosharp-images/ios-animation.gif "UrhoSharp 3D Interactive Chart on iOS")
![](urhosharp-images/android-animation.gif "UrhoSharp 3D Interactive Chart on Android")

## <a name="adding-the-urhosharp-nuget-packages"></a>Hinzufügen der urhusharp-nuget-Pakete

Vor der Verwendung von urhusharp müssen Entwickler der Projekt Mappe das urhusharp-nuget-Paket hinzufügen. In diesem Handbuch wird davon ausgegangen, dass ein Xamarin.Forms Projekt mit einem IOS-, Android-und .NET Standard-Bibliotheksprojekt. Der gesamte Code wird im .NET Standard Bibliotheksprojekt geschrieben. Allerdings muss das nuget-Tool urhusharp auch den IOS-und Android-Projekten hinzugefügt werden.

Das nuget-Paket urhusharp. Forms enthält alle Objekte, die zum Erstellen von urhusharp-Objekten benötigt werden. Das nuget-Paket urhosharp. Forms enthält die- `UrhoSurface` Klasse, die zum Hosten von urhosharp in verwendet wird Xamarin.Forms .
Klicken Sie zunächst mit der rechten Maustaste auf den Ordner **Pakete** im Projekt .NET Standard Bibliothek, und wählen Sie **Pakete hinzufügen...** aus. Geben Sie den Suchbegriff **urhusharp. Forms**ein, wählen Sie **urhusharp für aus Xamarin.Forms **, und klicken Sie dann auf **Paket hinzufügen**.

[![](urhosharp-images/add-package-sml.png "Add Packages Dialog")](urhosharp-images/add-package.png#lightbox "Add Packages Dialog")

Das nuget-Paket "urhusharp. Forms" wird dem Projekt hinzugefügt:

![](urhosharp-images/packages.png "Packages Folder")

Wiederholen Sie die obigen Schritte für plattformspezifische Projekte (z. b. IOS und Android).

## <a name="walkthrough-adding-urhosharp-to-a-xamarinforms-app"></a>Exemplarische Vorgehensweise: Hinzufügen von urhusharp zu einer Xamarin.Forms App

Diese Schritte beschreiben den Code im Xamarin.Forms urhusharp-Beispiel:

1. [Erstellen einer xamarin Forms-Seite](#1-create-a-xamarin-forms-page)
2. [Hinzufügen der urhusurface](#2-add-the-urhosurface)
3. [Erstellen einer Urho-Anwendung](#3-build-an-urho-application)
4. [Hinzufügen der Charts-Klasse zur urhusurface-Klasse](#4-add-the-charts-class-to-the-urhosurface)
5. [Interagieren mit urhusharp](#5-interacting-with-urhosharp)

Beachten Sie, dass das Beispiel c# 6-Funktionen verwendet und nicht in älteren Versionen von Visual Studio kompiliert werden kann.

### <a name="1-create-a-xamarin-forms-page"></a>1. Erstellen einer xamarin Forms-Seite

Der folgende Code zeigt eine Xamarin.Forms Seite `UrhoPage` vor dem Hinzufügen eines Urho-bezogenen Codes:

```csharp
public class UrhoPage : ContentPage
{
  Slider selectedBarSlider, rotationSlider;

  public UrhoPage()
  {
    // we'll add Urho later

    rotationSlider = new Slider(0, 500, 250);

    selectedBarSlider = new Slider(0, 5, 2.5);

    Title = " UrhoSharp + Xamarin.Forms";
    Content = new StackLayout {
      Padding = new Thickness (12, 12, 12, 40),
      VerticalOptions = LayoutOptions.FillAndExpand,
      Children = {
        rotationSlider,
        new Label { Text = "SELECTED VALUE:" },
        selectedBarSlider,
      }
    };
  }
```

### <a name="2-add-the-urhosurface"></a>2. Hinzufügen der urhusurface

Urhosharp kann in einem `ContentPage` wie andere Steuerelemente gehostet werden Xamarin.Forms .
Der folgende Code Ausschnitt zeigt eine `UrhoSurface` , die der Seite hinzugefügt wurde Xamarin.Forms :

```csharp
using Urho;
using Urho.Forms;
...
public class UrhoPage : ContentPage
{
  UrhoSurface urhoSurface;

  public UrhoPage()
  {
    urhoSurface = new UrhoSurface();
    urhoSurface.VerticalOptions = LayoutOptions.FillAndExpand;
...
    Content = new StackLayout {
    Padding = new Thickness (12, 12, 12, 40),
    VerticalOptions = LayoutOptions.FillAndExpand,
    Children = {
      urhoSurface,  // added
      new Label { Text = "ROTATION:" },
      rotationSlider,
      new Label { Text = "SELECTED VALUE:" },
      selectedBarSlider,
    }
  };
```

### <a name="3-build-an-urho-application"></a>3. Erstellen einer Urho-Anwendung

Informationen zur `Charts` Implementierung der in diesem Beispiel verwendeten Urho 3D-Grafiken finden Sie in der-Klasse. Die grundlegende Code Gliederung wird unten gezeigt: Beachten Sie, dass die-Klasse implementiert, `Urho.Application` die sich von der-Klasse unterscheidet `Xamarin.Forms.Application` , die in **app.cs**implementiert ist.

```csharp
using Urho;
using Urho.Actions;
using Urho.Gui;
using Urho.Shapes;

namespace FormsSample
{
    public class Charts : Urho.Application
    {
    public Charts (ApplicationOptions options = null) : base(options) { }
    protected override void Start ()
    {
      ...
    }
    protected override void OnUpdate(float timeStep)
    {
      ...
    }
```

Die [urhosharp-Dokumentation](~/graphics-games/urhosharp/index.md) enthält weitere Informationen zum Erstellen von 3D-Szenen und-Aktionen.

### <a name="4-add-the-charts-class-to-the-urhosurface"></a>4. Hinzufügen der Charts-Klasse zum urhusurface

Verwenden `UrhoSurface.Show<T>` Sie die generische-Methode, um der Seite die Urho-Anwendung hinzuzufügen Xamarin.Forms . Der folgende Code Ausschnitt zeigt den zusätzlichen Code, der zum Erstellen der-Klasse erforderlich ist `Charts` :

```csharp
public class UrhoPage : ContentPage
{
  Charts urhoApp;
  ...
  protected override async void OnAppearing ()
  {
    urhoApp = await urhoSurface.Show<Charts> (new ApplicationOptions(assetsFolder: null)
      { Orientation = ApplicationOptions.OrientationType.Portrait });
  }
```

Hinweis: die `Show<T>` Methode ist asynchron und sollte mit dem- `await` Schlüsselwort aufgerufen werden.

### <a name="5-interacting-with-urhosharp"></a>5. Interaktion mit urhusharp

Im Beispiel können die Diagramm leisten ausgewählt und geändert werden. Die `Charts` -Klasse macht `Bars` und verfügbar `SelectedBar` , um diese Interaktion zu aktivieren.

Jede "Leiste" weist einen Auswahl Ereignishandler `Charts` auf, nachdem die Klasse gerendert wurde, indem Sie die verfügbar gemachte Auflistung durchläuft `Bars` :

```csharp
protected override async void OnAppearing ()
{
  urhoApp = await urhoSurface.Show<Charts>(new ApplicationOptions(assetsFolder: null) { Orientation = ApplicationOptions.OrientationType.Portrait });
  foreach (var bar in urhoApp.Bars)
  {
    bar.Selected += OnBarSelection;
  }
}
```

Der-Ereignishandler verwendet den Wert des- Xamarin.Forms `Slider` Steuer Elements, um die Höhe des angegebenen Balkens anzupassen:

```csharp
private void OnBarSelection(Bar bar)
{
  //reset value
  selectedBarSlider.ValueChanged -= OnValuesSliderValueChanged;
  selectedBarSlider.Value = bar.Value;
  selectedBarSlider.ValueChanged += OnValuesSliderValueChanged;
}

void OnValuesSliderValueChanged(object sender, ValueChangedEventArgs e)
{  // C# 6
  if (urhoApp?.SelectedBar != null)
  {
    urhoApp.SelectedBar.Value = (float)e.NewValue;
  }
}
```

Verknüpfen Sie schließlich die beiden Steuer `Slider` Elemente so, dass sich der urhusharp-Zeichenbereich ändert, wenn der Wert geändert wird. Der erste Schieberegler dreht das 3D-Diagramm Bild, und der zweite Schieberegler passt die Höhe der ausgewählten Leiste an.

```csharp
rotationSlider = new Slider(0, 500, 250);
rotationSlider.ValueChanged += (s, e) => urhoApp?.Rotate((float)(e.NewValue - e.OldValue));

selectedBarSlider = new Slider(0, 5, 2.5);
selectedBarSlider.ValueChanged += OnValuesSliderValueChanged;
```

Die Animationen am [oberen Rand der Seite](#what-is-urhosharp) zeigen das Beispiel, das ausgeführt wird.

## <a name="summary"></a>Zusammenfassung

Diese Seite zeigt, wie urhusharp verwendet werden kann, um 3D-Datenvisualisierung hinzuzufügen Xamarin.Forms . Weitere Informationen zum Erstellen von Urho-Szenen, die mithilfe der oben gezeigten Methode in Apps integriert werden können, finden Sie in der [urhusharp-Dokumentation](~/graphics-games/urhosharp/index.md) Xamarin.Forms .

## <a name="related-links"></a>Verwandte Links

- [Beispiel für Diagramme (c# 6)](https://github.com/xamarin/urho-samples/tree/master/FormsSample)
- [UrhoSharp](~/graphics-games/urhosharp/index.md)
