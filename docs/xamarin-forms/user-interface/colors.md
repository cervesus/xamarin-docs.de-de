---
title: Farben
description: "Xamarin.Forms stellt eine flexible plattformübergreifende Farbe-Klasse bereit."
ms.topic: article
ms.prod: xamarin
ms.assetid: 22288ABF-57BE-47A9-ACC3-AC604D787C46
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/15/2017
ms.openlocfilehash: 69ab69efd60c486255164987795db1937b36b97d
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="colors"></a>Farben

_Xamarin.Forms stellt eine flexible plattformübergreifende Farbe-Klasse bereit._

Dieser Artikel führt die verschiedenen Möglichkeiten der `Color` -Klasse kann in Xamarin.Forms verwendet werden.

Die `Color` -Klasse bietet eine Reihe von Methoden zum Erstellen einer Farbe-Instanz

-  **Mit dem Namen Farben** -eine Auflistung von gemeinsam mit dem Namen-Farben, einschließlich `Red`, `Green`, und `Blue`.
-  **FromHex** -Zeichenfolge in HTML, z. B. "00FF00" verwendete Syntax ähnelt. Ist Alpha kann optional als das erste Paar von Zeichen ("CC00FF00") angegeben werden.
-  **FromHsla** -Farbton, Sättigung und Helligkeit `double` Werte, mit optionalen Alphawert (0,0 bis 1,0).
-  **FromRgb** -Rot, Grün und Blau `int` Werte (0-255).
-  **FromRgba** -Rot, Grün, Blau und Alpha `int` Werte (0-255).
-  **FromUint** -legen Sie ein einzelnes `double` Wert, der **Argb**.

Hier einige Beispiel-Farben, zugewiesen ist die `BackgroundColor` von einige Bezeichnungen, die verschiedene Variationen der zulässigen Syntax verwenden:

```csharp
var red    = new Label { Text = "Red",   BackgroundColor = Color.Red };
var orange = new Label { Text = "Orange",BackgroundColor = Color.FromHex("FF6A00") };
var yellow = new Label { Text = "Yellow",BackgroundColor = Color.FromHsla(0.167, 1.0, 0.5, 1.0) };
var green  = new Label { Text = "Green", BackgroundColor = Color.FromRgb (38, 127, 0) };
var blue   = new Label { Text = "Blue",  BackgroundColor = Color.FromRgba(0, 38, 255, 255) };
var indigo = new Label { Text = "Indigo",BackgroundColor = Color.FromRgb (0, 72, 255) };
var violet = new Label { Text = "Violet",BackgroundColor = Color.FromHsla(0.82, 1, 0.25, 1) };

var transparent = new Label { Text = "Transparent",BackgroundColor = Color.Transparent };
var @default = new Label    { Text = "Default",    BackgroundColor = Color.Default };
var accent = new Label      { Text = "Accent",     BackgroundColor = Color.Accent };
```

Diese Farben werden auf jeder Plattform unten angezeigt. Beachten Sie die endgültige Farbe - `Accent` -ist eine blue-ish Farbe für IOS- und Android; dieser Wert wird definiert, indem Sie Xamarin.Forms. Auf Windows Phone der `Accent` zeigt Rot *, die vom Benutzer ausgewählte Akzentfarbe für dieses Gerät ist*, dieser Wert ändert sich abhängig von Einstellungen des Benutzers.

 [ ![Farbe Demo](colors-images/colors-sml.png "Farbe Demo")](colors-images/colors.png "Farbe-Demo")

## <a name="colordefault"></a>Color.Default

Verwenden der `Default` können (oder erneut festlegen) einen Farbwert zurück zum Standardwert für die Plattform (Voraussetzung ist, dass dies eine unterschiedliche zugrunde liegenden Farbe auf jeder Plattform für jede Eigenschaft darstellt).

Entwickler können diesen Wert festlegen einer `Color` Eigenschaft jedoch sollte **nicht** Abfragen dieser Instanz der Komponente RGB-Werte (sie können alle auf-1 festgelegt).

## <a name="colortransparent"></a>Color.Transparent

Legen Sie die Farbe zu deaktivieren.

## <a name="coloraccent"></a>Color.Accent

Auf Windows Phone ist dies die Komplementärfarbe, die vom Benutzer ausgewählt wurde. Gute Windows Phone-Anwendungen verwenden diese als Teil ihrer formatieren, um ein homogenes Aussehen und Verhalten bereitzustellen.

Auf IOS- und Android wird diese Instanz eine kontrastreiche Farbe festlegen, die auf den Standardhintergrund angezeigt wird, jedoch ist nicht die Standard-Textfarbe identisch.

## <a name="additional-methods"></a>Zusätzliche Methoden

`Color` Instanzen enthalten zusätzliche Methoden, die verwendet werden können, um neue Farben zu erstellen:

-  **AddLuminosity** -ändern die Helligkeit durch das angegebene Delta gibt eine neue Farbe.
-  **WithHue** -gibt eine neue Farbe aus, und Ersetzen Sie den Farbton durch den angegebenen Wert zurück.
-  **WithLuminosity** -gibt eine neue Farbe aus, und Ersetzen Sie dabei die Brillanz mit dem angegebenen Wert zurück.
-  **WithSaturation** -gibt eine neue Farbe aus, und Ersetzen Sie dabei die Sättigung mit dem angegebenen Wert zurück.
-  **MultiplyAlpha** -gibt eine neue Farbe durch Ändern der Alpha, die der angegebene Alphawert multipliziert.

## <a name="deviceruntimeplatform"></a>Device.RuntimePlatform

Dieser Codeausschnitt verwendet die `Device.RuntimePlatform` selektiv die Farbe der festzulegenden Eigenschaft ein `ActivityIndicator`:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator
{
    Color = Device.RuntimePlatform == Device.iOS ? Color.Black : Color.Default,
    IsRunning = true
};
```

## <a name="using-from-xaml"></a>Verwendung von XAML

Farben können auch problemlos in XAML, die mit definierten Farbnamen oder die Hex-Darstellungen, die hier gezeigten verwiesen werden:

```xaml
<Label Text="Sea color" BackgroundColor="Aqua" />
<Label Text="RGB" BackgroundColor="#00FF00" />
<Label Text="Alpha plus RGB" BackgroundColor="#CC00FF00" />
<Label Text="Tiny RGB" BackgroundColor="#0F0" />
<Label Text="Tiny Alpha plus RGB" BackgroundColor="#C0F0" />
```

## <a name="summary"></a>Zusammenfassung

Der Xamarin.Forms `Color` Klasse wird verwendet, um die Plattform-fähigen Farbe Verweise zu erstellen. Es kann im freigegebenen Code und XAML verwendet werden.


## <a name="related-links"></a>Verwandte Links

- [ColorsSample](https://developer.xamarin.com/samples/WorkingWithColors)
- [Bindbare Zeitauswahl (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindablePicker/)
