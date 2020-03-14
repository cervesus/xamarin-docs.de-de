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
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305756"
---
# <a name="colors-in-xamarinforms"></a>Farben in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithcolors)

_Xamarin. Forms stellt eine flexible, plattformübergreifende farbklasse bereit._

In diesem Artikel werden die verschiedenen Möglichkeiten vorgestellt, mit denen die [`Color`](xref:Xamarin.Forms.Color) -Klasse in xamarin. Forms verwendet werden kann.

Die [`Color`](xref:Xamarin.Forms.Color) -Klasse stellt eine Reihe von Methoden bereit, um eine farbinstanz zu erstellen:

- **Benannte Farben** : eine Auflistung allgemeiner benannter Farben, einschließlich `Red`, `Green`und `Blue`.
- **FromHex** -Zeichen folgen Wert, der der in HTML verwendeten Syntax ähnelt, z. b. "00FF00". Alpha kann optional als erstes Zeichenpaar ("CC00FF00") angegeben werden.
- **Fromhsla** -Hue, Sättigung und Helligkeit `double` Werte mit optionalem Alpha-Wert (0,0-1.0).
- **FromRgb** -rot, grün und blau `int` Werte (0-255).
- **Fromrgba** -rot, grün, blau und Alpha `int` Werte (0-255).
- **Fromuint** : legt einen einzelnen `double` Wert fest, der **ARGB**repräsentiert.

Hier sind einige Beispiel Farben, die dem `BackgroundColor` einiger Bezeichnungen mit unterschiedlichen Variationen der zulässigen Syntax zugewiesen sind:

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

Diese Farben werden auf jeder Plattform unten angezeigt. Beachten Sie, dass das endgültige Farb `Accent` eine blaue Farbe für IOS und Android ist. Dieser Wert wird von xamarin. Forms definiert.

 [![Farb Demo](colors-images/colors-sml.png "Farb Demo")](colors-images/colors.png#lightbox "Farb Demo")

## <a name="colordefault"></a>Color.Default

Verwenden Sie die `Default`, um einen Farbwert auf den Platt Form Standardwert zurückzusetzen (oder neu festzulegen) (das Verständnis dafür, dass dies eine andere zugrunde liegende Farbe auf jeder Plattform für jede Eigenschaft darstellt).

Entwickler können diesen Wert verwenden, um eine `Color`-Eigenschaft festzulegen. diese Instanz sollte jedoch **nicht** für die RGB-Komponente der Komponente abgefragt werden (Sie sind alle auf-1 festgelegt).

## <a name="colortransparent"></a>Color.Transparent

Legen Sie die Farbe zu deaktivieren.

## <a name="coloraccent"></a>Color.Accent

Unter iOS und Android wird diese Instanz in eine kontrastreicher Farbe festgelegt, die auf den Standardhintergrund angezeigt wird, aber ist nicht die Standardtextfarbe identisch.

## <a name="additional-methods"></a>Zusätzliche Methoden

[`Color`](xref:Xamarin.Forms.Color) Instanzen enthalten die folgenden zusätzlichen Methoden:

- **Addluminosity** : gibt einen `Color` zurück, indem die Helligkeit um das angegebene Delta geändert wird.
- **Multiplyalpha** : gibt einen `Color` zurück, indem das Alpha geändert und mit dem angegebenen Alpha Wert multipliziert wird.
- $ **Hex** -gibt eine hexadezimale `string` Darstellung eines `Color`zurück.
- **Withhue** : gibt einen `Color`zurück, der den Farbton durch den angegebenen Wert ersetzt.
- **Withluminosity** : gibt einen `Color`zurück, der die Helligkeit durch den angegebenen Wert ersetzt.
- **Withsättigung** : gibt einen `Color`zurück, der die Sättigung durch den angegebenen Wert ersetzt.

## <a name="implicit-conversions"></a>Implizite Konvertierungen

Die implizite Konvertierung zwischen den `Xamarin.Forms.Color`-und `System.Drawing.Color` Typen kann ausgeführt werden:

```csharp
Xamarin.Forms.Color xfColor = Xamarin.Forms.Color.FromRgb(0, 72, 255);
System.Drawing.Color sdColor = System.Drawing.Color.FromArgb(38, 127, 0);

// Implicity convert from a Xamarin.Forms.Color to a System.Drawing.Color
System.Drawing.Color sdColor2 = xfColor;

// Implicitly convert from a System.Drawing.Color to a Xamarin.Forms.Color
Xamarin.Forms.Color xfColor2 = sdColor;
```

## <a name="deviceruntimeplatform"></a>Device.RuntimePlatform

In diesem Code Ausschnitt wird die `Device.RuntimePlatform`-Eigenschaft verwendet, um die Farbe eines `ActivityIndicator`selektiv festzulegen:

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

- [Colorssample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithcolors)
- [Bindbare Auswahl (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablepicker)
