---
title: Verwenden von UrhoSharp in Xamarin.Forms
description: UrhoSharp kann verwendet werden, eine Anwendung für die erweiterte Visualisierung 3D-Grafiken hinzu
ms.prod: xamarin
ms.assetid: 0646B98E-CC04-4537-9715-9F82338FD7FF
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/11/2016
ms.openlocfilehash: 1a982078f7a3fb2ba462cd7d6f1420b1d27618f7
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="using-urhosharp-in-xamarinforms"></a>Verwenden von UrhoSharp in Xamarin.Forms

## <a name="what-is-urhosharp"></a>Was ist UrhoSharp?

[UrhoSharp](~/graphics-games/urhosharp/index.md) ist ein leistungsfähiges 3D-Modul für Entwickler von Xamarin und .NET. Die [Einführung](~/graphics-games/urhosharp/introduction.md) näher erläutert, die Bibliothek UrhoSharp und [Anmerkungen zu dieser](~/graphics-games/urhosharp/using.md) beschrieben, wie im Hintergrund beim Übersetzen und Aktionen zu programmieren.

UrhoSharp kann zum Rendern von Grafiken in Xamarin.Forms-Anwendungen verwendet werden.
Dies [Beispiel](https://github.com/xamarin/urho-samples/tree/master/FormsSample) veranschaulicht, wie UrhoSharp verwendet werden kann, um eine interaktive 3D-Diagramm zu erstellen:

![](urhosharp-images/ios-animation.gif "UrhoSharp Interaktive 3D-Diagramm auf iOS")
![](urhosharp-images/android-animation.gif "UrhoSharp Interaktive 3D-Diagramm unter Android")

## <a name="adding-the-urhosharp-nuget-packages"></a>Hinzufügen von UrhoSharp Nuget-Paketen

Vor der Verwendung UrhoSharp, müssen Entwickler ihre Projektmappe das UrhoSharp Nuget-Paket hinzugefügt. Dieses Handbuch setzt voraus, eine Xamarin.Forms-Projekt mit einem iOS, Android und .NET Standard-Steuerelementbibliothek-Projekt. Der gesamte Code wird in der .NET Standard-Bibliotheksprojekt geschrieben werden. aber die UrhoSharp Nuget muss für den IOS- und Android-Projekte zu hinzugefügt werden.

Das UrhoSharp.Forms NuGet-Paket enthält alle Objekte, die zum Erstellen von Objekten UrhoSharp erforderlich. Das UrhoSharp.Forms NuGet-Paket enthält die `UrhoSurface` -Klasse, die zum Host UrhoSharp in Xamarin.Forms verwendet wird.
Um zu beginnen, mit der Maustaste auf die **Pakete** Ordner im .NET Standard-Bibliotheksprojekt, und wählen **Pakete hinzufügen...** . Geben Sie den Suchbegriff **UrhoSharp.Forms**Option **UrhoSharp für Xamarin.Forms**, klicken Sie dann auf **Paket hinzufügen**.

[![](urhosharp-images/add-package-sml.png "Pakete-Dialogfeld "hinzufügen"")](urhosharp-images/add-package.png#lightbox "Pakete-Dialogfeld "hinzufügen"")

Das UrhoSharp.Forms NuGet-Paket wird dem Projekt hinzugefügt werden:

![](urhosharp-images/packages.png "Pakete (Ordner)")

Wiederholen Sie die oben genannten Schritte für plattformspezifischen Projekte (z. B. iOS und Android) aus.

## <a name="walkthrough-adding-urhosharp-to-a-xamarinforms-app"></a>Exemplarische Vorgehensweise: Hinzufügen von UrhoSharp mit einer Xamarin.Forms-app

Diese Schritte beschreiben den Code im Beispiel Xamarin.Forms UrhoSharp:

1. [Erstellen Sie eine Xamarin-Forms-Seite](#1)
2. [Fügen Sie der UrhoSurface hinzu.](#2)
3. [Erstellen einer Anwendung Urho](#3)
4. [Fügen Sie die Diagramme-Klasse, um die UrhoSurface](#4)
5. [Interaktion mit UrhoSharp](#5)

Beachten Sie, dass im Beispiel C#-6-Funktionen verwendet und möglicherweise nicht auf älteren Versionen von Visual Studio kompiliert werden.

<a name="1"/>

### <a name="1-create-a-xamarin-forms-page"></a>1. Erstellen Sie eine Xamarin-Forms-Seite

Der folgende Code zeigt eine Seite Xamarin.Forms `UrhoPage` vor Urho bezogenen Code hinzugefügt wurde:

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

UrhoSharp aufgenommen werden kann, einem `ContentPage` wie andere Steuerelemente Xamarin.Forms.
Der Codeausschnitt unten zeigt eine `UrhoSurface` auf der Seite "Xamarin.Forms" hinzugefügt:

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

### <a name="3-build-a-urho-application"></a>3. Erstellen Sie eine Anwendung Urho

Finden Sie in der `Charts` Klasse für die Implementierung der Urho-3D-Grafiken, die in diesem Beispiel verwendet. Die Gliederung basic-Code wird gezeigt unten – Beachten Sie, dass die Klasse implementiert `Urho.Application` unterscheidet sich von der `Xamarin.Forms.Application` -Klasse, die im implementiert ist **App.cs**.

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

Die [UrhoSharp Dokumentation](~/graphics-games/urhosharp/index.md) enthält weitere Informationen zum Erstellen von 3D-Szenen und Aktionen.

<a name="4"/>

### <a name="4-add-the-charts-class-to-the-urhosurface"></a>4. Fügen Sie die Diagramme-Klasse, um die UrhoSurface

Verwenden der `UrhoSurface.Show<T>` generische Methode, um die Anwendung Urho auf der Seite "Xamarin.Forms" hinzuzufügen. Der Codeausschnitt unten zeigt die zusätzlichen Code erforderlich, um das Erstellen der `Charts` Klasse:

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

Hinweis: die `Show<T>` Methode ist asynchron und aufgerufen werden, mit dem `await` Schlüsselwort.

<a name="5"/>

### <a name="5-interacting-with-urhosharp"></a>5. Interaktion mit UrhoSharp

Im Beispiel ermöglicht Diagrammbalken ausgewählt und geändert werden. Die `Charts` -Klasse verfügbar macht die `Bars` und `SelectedBar` diese Interaktion zu aktivieren.

"Balken" hat einen Auswahl-Ereignishandler hinzugefügt, nachdem die `Charts` Klasse dargestellt wurde, durch den verfügbar gemachten durchlaufen `Bars` Auflistung:

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

Schließlich Verknüpfen der beiden `Slider` Steuerelemente, sodass, wenn deren Wert ändert der Zeichenbereich UrhoSharp betroffen ist. Der erste Schieberegler dreht das Bild 3D-Diagramm und der zweite Schieberegler passt die Höhe des ausgewählten Balkens an.

```csharp
rotationSlider = new Slider(0, 500, 250);
rotationSlider.ValueChanged += (s, e) => urhoApp?.Rotate((float)(e.NewValue - e.OldValue));

selectedBarSlider = new Slider(0, 5, 2.5);
selectedBarSlider.ValueChanged += OnValuesSliderValueChanged;
```

Die Animationen an die [oben auf der Seite](#) zeigen Sie das Beispiel ausführen.

## <a name="summary"></a>Zusammenfassung

Auf dieser Seite werden wie UrhoSharp hinzuzufügende 3D datenvisualisierung in Xamarin.Forms verwendet werden kann. Lesen der [UrhoSharp Dokumentation](~/graphics-games/urhosharp/index.md) für Weitere Informationen zum Erstellen von Urho Szenen, die in Xamarin.Forms-apps, die mit der oben gezeigten Methode aufgenommen werden kann.


## <a name="related-links"></a>Verwandte Links

- [Diagramme-Beispiel (C#-6)](https://github.com/xamarin/urho-samples/tree/master/FormsSample)
- [UrhoSharp](~/graphics-games/urhosharp/index.md)
