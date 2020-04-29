---
title: Farben in xamarin. Forms
description: Xamarin. Forms stellt eine flexible, plattformübergreifende farbklasse bereit. In diesem Artikel werden die Funktionen erläutert, die von der Color-Klasse bereitgestellt werden, und wie Sie verwendet wird.
ms.prod: xamarin
ms.assetid: 22288ABF-57BE-47A9-ACC3-AC604D787C46
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/02/2020
ms.openlocfilehash: 42b532b8565d2d8e0289b8fd446e1dd7762a09ac
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517030"
---
# <a name="colors-in-xamarinforms"></a>Farben in xamarin. Forms

[![Beispiel](~/media/shared/download.png) herunterladen herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithcolors)

_Xamarin. Forms stellt eine flexible, plattformübergreifende farbklasse bereit._

In diesem Artikel werden die verschiedenen Möglichkeiten [`Color`](xref:Xamarin.Forms.Color) vorgestellt, mit denen die-Klasse in xamarin. Forms verwendet werden kann.

Die [`Color`](xref:Xamarin.Forms.Color) -Klasse stellt eine Reihe von Methoden zum Erstellen `Color` einer-Instanz bereit:

- **Benannte Farben** : eine Auflistung allgemeiner benannter Farben, einschließlich `Red`, `Green`und `Blue`.
- `FromHex`-Zeichen folgen Wert, der der in HTML verwendeten Syntax ähnelt, z. b. "00FF00". Alpha kann optional als erstes Zeichenpaar ("CC00FF00") angegeben werden.
- `FromHsla`-Hue, Sättigung und Helligkeits `double` Werte mit optionalem Alpha-Wert (0,0-1.0).
- `FromHsv`-Hue, Sättigung und Wert `int` oder `double` Werte.
- `FromHsva`-Hue, Sättigung und Wert `int` oder `double` Werte.
- `FromRgb`-Rot, grün und blau `int` -Werte (0-255).
- `FromRgba`-Rot, grün, blau und Alpha `int` Werte (0-255).
- `FromUint`-Legen Sie einen `double` einzelnen Wert fest, der **ARGB**darstellt.

Hier sind einige Beispiel Farben, die den `BackgroundColor` einiger Bezeichnungen zugewiesen sind, die unterschiedliche Variationen der zulässigen Syntax verwenden:

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

Diese Farben werden auf jeder Plattform unten angezeigt. Beachten Sie die endgültige Farbe `Accent` , die eine blaue Farbe für IOS und Android ist. Dieser Wert wird von xamarin. Forms definiert.

 [![Farb Demo](colors-images/colors-sml.png "Farb Demo")](colors-images/colors.png#lightbox "Farb Demo")

## <a name="colordefault"></a>Color. Default

Verwenden `Default` Sie, um einen Farbwert auf den Platt Form Standardwert zurückzusetzen (oder neu festzulegen) (das Verständnis dafür, dass dies eine andere zugrunde liegende Farbe auf jeder Plattform für jede Eigenschaft darstellt).

Entwickler können diesen Wert verwenden, um eine `Color` Eigenschaft festzulegen, sollten diese Instanz jedoch **nicht** nach den RGB-Werten der Komponente Abfragen (Sie sind alle auf "-1" festgelegt).

## <a name="colortransparent"></a>Color. transparent

Legen Sie die Farbe auf Clear fest.

## <a name="coloraccent"></a>Color. Akzent

Unter IOS und Android wird diese Instanz auf eine Kontrastfarbe festgelegt, die im Standard Hintergrund sichtbar ist, aber nicht mit der Standard Textfarbe identisch ist.

## <a name="additional-methods"></a>Zusätzliche Methoden

[`Color`](xref:Xamarin.Forms.Color)-Instanzen enthalten die folgenden zusätzlichen Methoden:

- `AddLuminosity`-Gibt einen `Color` zurück, indem die Helligkeit um das angegebene Delta geändert wird.
- `MultiplyAlpha`-Gibt einen `Color` zurück, indem das Alpha geändert und mit dem angegebenen Alpha-Wert multipliziert wird.
- `ToHex`-gibt eine hexadezimale `string` Darstellung eines `Color`zurück.
- `WithHue`-Gibt einen `Color`zurück, der den Farbton durch den angegebenen Wert ersetzt.
- `WithLuminosity`-Gibt einen `Color`zurück, der die Leuchtkraft durch den angegebenen Wert ersetzt.
- `WithSaturation`-Gibt einen `Color`zurück, der die Sättigung durch den angegebenen Wert ersetzt.

## <a name="implicit-conversions"></a>Implizite Konvertierungen

Die implizite Konvertierung zwischen `Xamarin.Forms.Color` dem `System.Drawing.Color` -Typ und dem-Typ kann ausgeführt werden:

```csharp
Xamarin.Forms.Color xfColor = Xamarin.Forms.Color.FromRgb(0, 72, 255);
System.Drawing.Color sdColor = System.Drawing.Color.FromArgb(38, 127, 0);

// Implicity convert from a Xamarin.Forms.Color to a System.Drawing.Color
System.Drawing.Color sdColor2 = xfColor;

// Implicitly convert from a System.Drawing.Color to a Xamarin.Forms.Color
Xamarin.Forms.Color xfColor2 = sdColor;
```

## <a name="deviceruntimeplatform"></a>Device. runtimeplatform

In diesem Code Ausschnitt wird die `Device.RuntimePlatform` -Eigenschaft verwendet, um die Farbe eines `ActivityIndicator`selektiv festzulegen:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator
{
    Color = Device.RuntimePlatform == Device.iOS ? Color.Black : Color.Default,
    IsRunning = true
};
```

## <a name="use-from-xaml"></a>Verwendung aus XAML

Auf Farben kann in XAML auch mit den definierten Farbnamen oder den hier gezeigten Hex-Darstellungen verwiesen werden:

```xaml
<Label Text="Sea color" BackgroundColor="Aqua" />
<Label Text="RGB" BackgroundColor="#00FF00" />
<Label Text="Alpha plus RGB" BackgroundColor="#CC00FF00" />
<Label Text="Tiny RGB" BackgroundColor="#0F0" />
<Label Text="Tiny Alpha plus RGB" BackgroundColor="#C0F0" />
```

> [!NOTE]
> Bei Verwendung der XAML-Kompilierung wird die Groß-/Kleinschreibung nicht beachtet, sodass Sie in Kleinbuchstaben geschrieben werden können. Weitere Informationen zur XAML-Kompilierung finden Sie unter [XAML Compilation (XAML-Kompilierung)](~/xamarin-forms/xaml/xamlc.md).

## <a name="related-links"></a>Verwandte Links

- [Colorssample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithcolors)
- [Bindbare Auswahl (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablepicker)
