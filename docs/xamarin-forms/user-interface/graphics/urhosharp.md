---
title: Verwenden von UrhoSharp in Xamarin.Forms
description: In diesem Artikel wird erläutert, wie von UrhoSharp verwendet werden kann, 3D-Grafiken einer Xamarin.Forms-Anwendung für die erweiterte Visualisierung hinzu.
ms.prod: xamarin
ms.assetid: 0646B98E-CC04-4537-9715-9F82338FD7FF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/11/2016
ms.openlocfilehash: 7045bd4d3343d0c11c6cd52fa02cdc005175b8a7
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70772932"
---
# <a name="using-urhosharp-in-xamarinforms"></a>Verwenden von UrhoSharp in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://github.com/xamarin/urho-samples/tree/master/FormsSample)

## <a name="what-is-urhosharp"></a>Was ist von UrhoSharp?

[Von UrhoSharp](~/graphics-games/urhosharp/index.md) ist eine leistungsstarke 3D-Engine für Entwickler, Xamarin und .NET. Die [Einführung](~/graphics-games/urhosharp/introduction.md) näher erläutert, die Bibliothek von UrhoSharp und [die Anmerkungen zu dieser](~/graphics-games/urhosharp/using.md) beschrieben, wie Sie Szenen und Aktionen zu programmieren.

Von UrhoSharp kann zum Rendern von Grafiken in Xamarin.Forms-Anwendungen verwendet werden.
Dies [Beispiel](https://github.com/xamarin/urho-samples/tree/master/FormsSample) wird veranschaulicht, wie von UrhoSharp verwendet werden kann, um eine interaktive 3D-Diagramm zu erstellen:

![](urhosharp-images/ios-animation.gif "Interaktives Diagramm 3D von UrhoSharp unter iOS")
![](urhosharp-images/android-animation.gif "von UrhoSharp Interaktive 3D-Diagramm unter Android")

## <a name="adding-the-urhosharp-nuget-packages"></a>Die UrhoSharp Nuget-Pakete hinzufügen

Vor dem Verwenden von UrhoSharp, müssen Entwickler das UrhoSharp Nuget-Paket zu ihrer Projektmappe hinzufügen. Dieses Handbuch setzt voraus, ein Xamarin.Forms-Projekt mit einem iOS, Android und .NET Standard Library-Projekt. Der gesamte Code wird in der .NET Standard-Bibliotheksprojekts geschrieben werden. aber die UrhoSharp Nuget muss auch für die IOS- und Android-Projekte hinzugefügt werden.

Das UrhoSharp.Forms Nuget-Paket enthält alle Objekte, die zum Erstellen von UrhoSharp Objekte erforderlich sind. Das UrhoSharp.Forms Nuget-Paket enthält die `UrhoSurface` -Klasse, die zum Hosten von UrhoSharp in Xamarin.Forms verwendet wird.
Um zu beginnen, mit der Maustaste auf die **Pakete** Ordner im .NET Standard Library-Projekt, und wählen **Pakete hinzufügen...** . Geben Sie den Suchbegriff **UrhoSharp.Forms**Option **von UrhoSharp für Xamarin.Forms**, klicken Sie dann auf **Paket hinzufügen**.

[![](urhosharp-images/add-package-sml.png "Pakete-Dialogfeld \"hinzufügen\"")](urhosharp-images/add-package.png#lightbox "Pakete-Dialogfeld \"hinzufügen\"")

Das UrhoSharp.Forms NuGet-Paket wird dem Projekt hinzugefügt werden:

![](urhosharp-images/packages.png "Ordner \"Pakete\"")

Wiederholen Sie die oben genannten Schritte für plattformspezifische Projekte (z. B. iOS und Android) ein.

## <a name="walkthrough-adding-urhosharp-to-a-xamarinforms-app"></a>Exemplarische Vorgehensweise: Hinzufügen von urhusharp zu einer xamarin. Forms-App

Diesen Schritten wird den Code in der von UrhoSharp die Xamarin.Forms-Beispiel beschrieben:

1. [Erstellen einer Xamarin-Forms-Seite](#1)
2. [Fügen Sie der UrhoSurface hinzu.](#2)
3. [Erstellen einer Anwendung Urho](#3)
4. [Fügen Sie die Diagramme-Klasse, um die UrhoSurface](#4)
5. [Interaktion mit von UrhoSharp](#5)

Beachten Sie die im Beispiel verwendeten C# 6-Funktionen und kann nicht auf älteren Versionen von Visual Studio kompilieren.

<a name="1"/>

### <a name="1-create-a-xamarin-forms-page"></a>1. Erstellen einer Xamarin-Forms-Seite

Der folgende Code zeigt eine Xamarin.Forms-Startseite `UrhoPage` vor jedem Urho-bezogenem Code hinzugefügt wurde:

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

<a name="2"/>

### <a name="2-add-the-urhosurface"></a>2. Fügen Sie der UrhoSurface hinzu.

Von UrhoSharp in aufgenommen werden kann. eine `ContentPage` wie andere Xamarin.Forms-Steuerelemente.
Der Codeausschnitt unten zeigt eine `UrhoSurface` der Xamarin.Forms-Seite hinzugefügt:

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

<a name="3"/>

### <a name="3-build-a-urho-application"></a>3. Erstellen einer Anwendung Urho

Finden Sie in der `Charts` -Klasse für die Implementierung der Urho 3D-Grafiken, die in diesem Beispiel verwendet. Die Kontur der basic-Code wird angezeigt. im folgenden – Beachten Sie, dass die Klasse implementiert `Urho.Application` unterscheidet, die `Xamarin.Forms.Application` -Klasse, die in implementiert wird **"App.cs"** .

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

Die [von UrhoSharp Dokumentation](~/graphics-games/urhosharp/index.md) enthält weitere Informationen zum Erstellen von 3D-Szenen und Aktionen.

<a name="4"/>

### <a name="4-add-the-charts-class-to-the-urhosurface"></a>4. Fügen Sie die Diagramme-Klasse, um die UrhoSurface

Verwenden der `UrhoSurface.Show<T>` generische Methode, um die Anwendung Urho auf der Seite "Xamarin.Forms" hinzuzufügen. Der folgende Codeausschnitt zeigt den zusätzlichen Code zum Erstellen der `Charts` Klasse:

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

Hinweis: die `Show<T>` Methode ist asynchron und aufgerufen werden soll, mit der `await` Schlüsselwort.

<a name="5"/>

### <a name="5-interacting-with-urhosharp"></a>5. Interaktion mit von UrhoSharp

Im Beispiel kann die Diagrammbalken ausgewählt und geändert werden. Die `Charts` -Klasse macht die `Bars` und `SelectedBar` diese Interaktion zu aktivieren.

Jede "Bar" verfügt über eine Auswahl-Ereignishandler hinzugefügt, nachdem die `Charts` Klasse gerendert wurde, durch Iteration über den verfügbar gemachten `Bars` Auflistung:

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

Der Ereignishandler verwendet den Wert der Xamarin.Forms `Slider` Steuerelement, um die Höhe der angegebenen Leiste anzupassen:

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

Schließlich werden die beiden socialsharing.Share `Slider` steuert, damit bei deren Wert geändert wird im Zeichenbereich von UrhoSharp betroffen ist. Der erste Schieberegler wird das 3D-Diagramm Bild gedreht und der zweite Schieberegler passt die Höhe des ausgewählten Balkens an.

```csharp
rotationSlider = new Slider(0, 500, 250);
rotationSlider.ValueChanged += (s, e) => urhoApp?.Rotate((float)(e.NewValue - e.OldValue));

selectedBarSlider = new Slider(0, 5, 2.5);
selectedBarSlider.ValueChanged += OnValuesSliderValueChanged;
```

Die Animationen an die [Seitenanfang](#what-is-urhosharp) zeigen Sie das Beispiel ausführen.

## <a name="summary"></a>Zusammenfassung

Diese Seite zeigt, wie von UrhoSharp verwendet werden kann, um Xamarin.Forms 3D datenvisualisierung hinzuzufügen. Lesen der [von UrhoSharp Dokumentation](~/graphics-games/urhosharp/index.md) für Weitere Informationen dazu, wie Sie Urho Szenen erstellen, die in Xamarin.Forms-apps mithilfe der oben gezeigten Methode aufgenommen werden kann.

## <a name="related-links"></a>Verwandte Links

- [Beispiel Diagramme (C# 6)](https://github.com/xamarin/urho-samples/tree/master/FormsSample)
- [UrhoSharp](~/graphics-games/urhosharp/index.md)
