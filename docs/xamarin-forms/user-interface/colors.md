---
title: Farben in Xamarin.Forms
description: Xamarin.Forms stellt eine flexible Plattform-Color-Klasse bereit. Dieser Artikel beschreibt die Funktionalität von Color-Klasse und Ihre Verwendung bereitgestellt.
ms.prod: xamarin
ms.assetid: 22288ABF-57BE-47A9-ACC3-AC604D787C46
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/15/2017
ms.openlocfilehash: 1f29f283207ed8c1382424b3680177886ad2c806
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68653160"
---
# <a name="colors-in-xamarinforms"></a>Farben in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithcolors)

_Xamarin.Forms stellt eine flexible Plattform-Color-Klasse bereit._

In diesem Artikel werden die verschiedenen Möglichkeiten der `Color` Klasse kann in Xamarin.Forms verwendet werden.

Die `Color` Klasse bietet eine Reihe von Methoden, um eine Farbe-Instanz erstellen

-  **Mit dem Namen Farben** -eine Auflistung von allgemeinen benannte Farben, einschließlich `Red`, `Green`, und `Blue`.
-  **FromHex** -Zeichen folgen Wert, der der in HTML verwendeten Syntax ähnelt, z. b. "00FF00". Alpha kann optional als erstes Zeichenpaar ("CC00FF00") angegeben werden.
-  **FromHsla** -Farbton, Sättigung und Helligkeit `double` Werte, mit einem optionalen alpha-Wert (0,0-1,0).
-  **FromRgb** – Rot, Grün und Blau `int` Werte (0-255).
-  **FromRgba** – Rot, Grün, Blau und Alpha `int` Werte (0-255).
-  **FromUint** -legen Sie eine einzelne `double` Wert, der **über den Argb**.

Hier einige Beispiel-Farben, zugewiesen ist die `BackgroundColor` einige Bezeichnungen, die verschiedene Varianten der zulässigen Syntax verwenden:

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

Diese Farben werden auf jeder Plattform unten angezeigt. Beachten Sie, dass die endgültige Farbe - `Accent` -eine blue-ish Farbe für Android- und iOS ist dieser Wert wird von Xamarin.Forms definiert.

 [![Farbe Demo](colors-images/colors-sml.png "Farbe Demo")](colors-images/colors.png#lightbox "Farbe-Demo")

## <a name="colordefault"></a>Color.Default

Verwenden der `Default` können (oder neu festlegen) einen Farbwert wieder auf den Platform-Standardwert (Grundlegendes zu, dass dies eine andere zugrunde liegende Farbe auf jeder Plattform für jede Eigenschaft darstellt).

Entwickler können diesen Wert festlegen einer `Color` Eigenschaft jedoch sollte **nicht** Abfragen dieser Instanz für die Komponente RGB-Werte (diese sind alle auf-1 festgelegt).

## <a name="colortransparent"></a>Color.Transparent

Legen Sie die Farbe zu deaktivieren.

## <a name="coloraccent"></a>Color.Accent

Unter iOS und Android wird diese Instanz in eine kontrastreicher Farbe festgelegt, die auf den Standardhintergrund angezeigt wird, aber ist nicht die Standardtextfarbe identisch.

## <a name="additional-methods"></a>Zusätzliche Methoden

`Color` Instanzen umfassen zusätzliche Methoden, die zum Erstellen von neuer Farben verwendet werden können:

-  **AddLuminosity** -gibt eine neue Farbe durch die Helligkeit durch das angegebene Delta ändern.
-  **WithHue** -gibt eine neue Farbe, und Ersetzen Sie dabei den Farbton mit dem angegebenen Wert zurück.
-  **WithLuminosity** -gibt eine neue Farbe, und Ersetzen Sie dabei die Helligkeit mit dem angegebenen Wert zurück.
-  **WithSaturation** -gibt eine neue Farbe, und Ersetzen Sie dabei die Sättigung mit dem angegebenen Wert zurück.
-  **MultiplyAlpha** -gibt eine neue Farbe durch Ändern der Alpha, die er mit dem angegebenen Alphawert multipliziert.

## <a name="implicit-conversions"></a>Implizite Konvertierungen

Implizite Konvertierung zwischen den `Xamarin.Forms.Color` und `System.Drawing.Color` Typen ausgeführt werden können:

```csharp
Xamarin.Forms.Color xfColor = Xamarin.Forms.Color.FromRgb(0, 72, 255);
System.Drawing.Color sdColor = System.Drawing.Color.FromArgb(38, 127, 0);

// Implicity convert from a Xamarin.Forms.Color to a System.Drawing.Color
System.Drawing.Color sdColor2 = xfColor;

// Implicitly convert from a System.Drawing.Color to a Xamarin.Forms.Color
Xamarin.Forms.Color xfColor2 = sdColor;
```

## <a name="deviceruntimeplatform"></a>Device.RuntimePlatform

Dieser Codeausschnitt verwendet die `Device.RuntimePlatform` selektiv die Farbe der festzulegenden Eigenschaft eine `ActivityIndicator`:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator
{
    Color = Device.RuntimePlatform == Device.iOS ? Color.Black : Color.Default,
    IsRunning = true
};
```

## <a name="using-from-xaml"></a>Verwenden von XAML

Farben können auch einfach in XAML mithilfe von definierten Farbnamen oder die Hex-Darstellungen, die hier gezeigten verwiesen werden:

```xaml
<Label Text="Sea color" BackgroundColor="Aqua" />
<Label Text="RGB" BackgroundColor="#00FF00" />
<Label Text="Alpha plus RGB" BackgroundColor="#CC00FF00" />
<Label Text="Tiny RGB" BackgroundColor="#0F0" />
<Label Text="Tiny Alpha plus RGB" BackgroundColor="#C0F0" />
```

> [!NOTE]
> Verwendung von XAML-Kompilierung Farbnamen werden Groß-/Kleinschreibung, und Sie werden daher in Kleinbuchstaben geschrieben werden können. Weitere Informationen zu XAML-Kompilierung, finden Sie unter [XAML-Kompilierung](~/xamarin-forms/xaml/xamlc.md).

## <a name="summary"></a>Zusammenfassung

Die Xamarin.Forms `Color` Klasse wird verwendet, um die Plattform-fähigen Farbe Verweise zu erstellen. Es kann im freigegebenen Code und XAML verwendet werden.


## <a name="related-links"></a>Verwandte Links

- [ColorsSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithcolors)
- [Bindbare Auswahl (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablepicker)
