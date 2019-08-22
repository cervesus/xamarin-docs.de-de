---
title: Farben in Xamarin.Forms
description: Xamarin.Forms stellt eine flexible Plattform-Color-Klasse bereit. Dieser Artikel beschreibt die Funktionalität von Color-Klasse und Ihre Verwendung bereitgestellt.
ms.prod: xamarin
ms.assetid: 22288ABF-57BE-47A9-ACC3-AC604D787C46
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2019
ms.openlocfilehash: 2a17b037803d1ca6e54000ea7ba3f05c8ce6034f
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69888471"
---
# <a name="colors-in-xamarinforms"></a>Farben in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithcolors)

_Xamarin.Forms stellt eine flexible Plattform-Color-Klasse bereit._

In diesem Artikel werden die verschiedenen Möglichkeiten [`Color`](xref:Xamarin.Forms.Color) vorgestellt, mit denen die-Klasse in xamarin. Forms verwendet werden kann.

Die [`Color`](xref:Xamarin.Forms.Color) -Klasse stellt eine Reihe von Methoden bereit, um eine farbinstanz zu erstellen:

- **Mit dem Namen Farben** -eine Auflistung von allgemeinen benannte Farben, einschließlich `Red`, `Green`, und `Blue`.
- **FromHex** -Zeichen folgen Wert, der der in HTML verwendeten Syntax ähnelt, z. b. "00FF00". Alpha kann optional als erstes Zeichenpaar ("CC00FF00") angegeben werden.
- **FromHsla** -Farbton, Sättigung und Helligkeit `double` Werte, mit einem optionalen alpha-Wert (0,0-1,0).
- **FromRgb** – Rot, Grün und Blau `int` Werte (0-255).
- **FromRgba** – Rot, Grün, Blau und Alpha `int` Werte (0-255).
- **FromUint** -legen Sie eine einzelne `double` Wert, der **über den Argb**.

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

[`Color`](xref:Xamarin.Forms.Color)-Instanzen enthalten die folgenden zusätzlichen Methoden:

- **Addluminosity** : gibt einen `Color` zurück, indem die Helligkeit durch das angegebene Delta geändert wird.
- **Multiplyalpha** : gibt einen `Color` zurück, indem das Alpha geändert und mit dem angegebenen Alpha-Wert multipliziert wird.
- $ **Hex** -gibt eine hexadezimale `string` Darstellung `Color`eines zurück.
- **Withhue** : gibt einen `Color`zurück, der den Farbton durch den angegebenen Wert ersetzt.
- **Withluminosity** : gibt einen `Color`zurück, der die Helligkeit durch den angegebenen Wert ersetzt.
- **Withsättigung** : gibt einen `Color`zurück, der die Sättigung durch den angegebenen Wert ersetzt.

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

Auf Farben kann in XAML auch mit den definierten Farbnamen oder den hier gezeigten Hex-Darstellungen verwiesen werden:

```xaml
<Label Text="Sea color" BackgroundColor="Aqua" />
<Label Text="RGB" BackgroundColor="#00FF00" />
<Label Text="Alpha plus RGB" BackgroundColor="#CC00FF00" />
<Label Text="Tiny RGB" BackgroundColor="#0F0" />
<Label Text="Tiny Alpha plus RGB" BackgroundColor="#C0F0" />
```

> [!NOTE]
> Verwendung von XAML-Kompilierung Farbnamen werden Groß-/Kleinschreibung, und Sie werden daher in Kleinbuchstaben geschrieben werden können. Weitere Informationen zur XAML-Kompilierung finden Sie unter [XAML Compilation (XAML-Kompilierung)](~/xamarin-forms/xaml/xamlc.md).

## <a name="related-links"></a>Verwandte Links

- [ColorsSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithcolors)
- [Bindbare Auswahl (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablepicker)
