---
title: Zusammenfassung der Kapitel 3. Details zu text
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 3. Details zu text'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2E5581A6-4D3E-4BD5-9FDB-ACBA0F0FC734
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 3ef8f14bd60cf612408bb9e3885ef319d3efc8c5
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998334"
---
# <a name="summary-of-chapter-3-deeper-into-text"></a>Zusammenfassung der Kapitel 3. Details zu text

In diesem Kapitel werden die [ `Label` ](xref:Xamarin.Forms.Label) Anzeigen ausführlicher behandelt, einschließlich der Farben, Schriftarten und Formatierungen.

## <a name="wrapping-paragraphs"></a>Umschließen von Absätzen

Wenn die [ `Text` ](xref:Xamarin.Forms.Label.Text) Eigenschaft `Label` langen Text enthält, `Label` umschließt ihn automatisch in mehrere Zeilen, wie die [ **Baskervilles** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/Baskervilles) Beispiel. Sie können Unicode-Fehlercodes, wie z. B. "\u2014' für den Em-Dash- oder C#-Zeichen wie '\r' in eine neue Zeile unterbrochen einbetten.

Bei der [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) und [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) Eigenschaften eine `Label` festgelegt `LayoutOptions.Fill`, die Gesamtgröße der der `Label` unterliegt den Speicherplatz, die seinem Container Stellt zur Verfügung. Die `Label` gilt als *eingeschränkte*. Die Größe der `Label` ist die Größe des Containers.

Wenn die `HorizontalOptions` und `VerticalOptions` Eigenschaften nicht auf Werte festgelegt sind `LayoutOptions.Fill`, wird die Größe des der `Label` unterliegt den speicherplatzanforderungen, die zum Rendern von Text, bis die Größe, die für der Container zur Verfügung der `Label`. Die `Label` gilt als *uneingeschränkte* und legt seine eigene Größe fest.

(Hinweis: die Begriffe *eingeschränkte* und *uneingeschränkte* möglicherweise nicht intuitiv, eine uneingeschränkte Sicht im Allgemeinen kleiner als eine eingeschränkte Ansicht ist. Darüber hinaus werden diese Begriffe nicht durchgängig in der ersten Kapitel des Buchs verwendet.)

Eine Ansicht z. B. eine `Label` in einer Dimension beschränkt und im anderen uneingeschränkt werden kann. Ein `Label` wird nur auf mehrere Zeilen Text umbrochen, wenn es horizontal beschränkt ist.

Wenn eine `Label` ist eingeschränkt, können sie deutlich mehr Speicherplatz als erforderlich für den Text belegen. Der Text innerhalb des gesamten Bereichs der positioniert werden kann die `Label`. Legen Sie die [ `HorizontalTextAlignment` ](xref:Xamarin.Forms.Label.HorizontalTextAlignment) Eigenschaft auf einen Member der [ `TextAlignment` ](xref:Xamarin.Forms.TextAlignment) Enumeration ([`Start`](xref:Xamarin.Forms.TextAlignment.Start), [ `Center` ](xref:Xamarin.Forms.TextAlignment.Center), oder [ `End` ](xref:Xamarin.Forms.TextAlignment.Center)) zur Steuerung der Ausrichtung aller Zeilen aus dem Absatz. Der Standardwert ist `Start` und richtet den Text.

Legen Sie die [ `VerticalTextAlignment` ](xref:Xamarin.Forms.Label.VerticalTextAlignment) Eigenschaft auf einen Member der `TextAlignment` Enumeration, positionieren Sie den Text an den Anfang, zentriert oder unteren Rand des Bereichs, der durch die `Label`.

Legen Sie die [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode) Eigenschaft auf einen Member der [ `LineBreakMode` ](xref:Xamarin.Forms.LineBreakMode) Enumeration ([`WordWrap`](xref:Xamarin.Forms.LineBreakMode.WordWrap), [ `CharacterWrap` ](xref:Xamarin.Forms.LineBreakMode.CharacterWrap), [ `NoWrap` ](xref:Xamarin.Forms.LineBreakMode.NoWrap), [ `HeadTruncation` ](xref:Xamarin.Forms.LineBreakMode.HeadTruncation), [ `MiddleTruncation` ](xref:Xamarin.Forms.LineBreakMode.MiddleTruncation), oder [ `TailTruncation` ](xref:Xamarin.Forms.LineBreakMode.TailTruncation)), Steuerelement, wie die mehrere Zeilen in ein Absatzwechsel oder abgeschnitten werden.

## <a name="text-and-background-colors"></a>Text- und Hintergrundfarben

Legen Sie die [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor) und [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor) Eigenschaften `Label` zu [ `Color` ](xref:Xamarin.Forms.Color) Werten zum Steuern, der Farbe von Text und Hintergrund.

Die `BackgroundColor` gilt für den Hintergrund der gesamten Bereichs, der von der `Label`. Je nachdem auf die `HorizontalOptions` und `VerticalOptions` Eigenschaften, die, dass die Größe möglicherweise erheblich größer als der Bereich erforderlich, um den Text anzuzeigen. Können Sie zum Experimentieren mit verschiedenen Werten von Farbe `HorizontalOptions`, `VerticalOptions`, `HorizontalExeAlignment`, und `VerticalTextAlignment` zu sehen, wie sie die Größe und Position der Auswirkungen auf die `Label`, und die Größe und Position des Texts in der `Label`.

## <a name="the-color-structure"></a>Die Color-Struktur

Die [ `Color` ](xref:Xamarin.Forms.Color) Struktur können Sie die Farben als Rot-Grün-Blau (RGB), oder Werte für Farbton-Sättigung-Helligkeit (HSL) oder mit einem Namen der Farbe anzugeben. Ein Alphakanal ist auch verfügbar, um Transparenz anzugeben.

Verwenden einer `Color` Konstruktor an:

- eine [graue Schattierung](xref:Xamarin.Forms.Color.%23ctor(System.Double))
- ein [RGB-Wert](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double))
- ein [RGB-Wert, mit Transparenz](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double,System.Double))

Argumente sind `double` Werte zwischen 0 und 1.

Sie können auch mehrere statische Methoden zum Erstellen verwenden `Color` Werte:

- [`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Double,System.Double,System.Double)) für `double` RGB-Werte zwischen 0 und 1
- [`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Int32,System.Int32,System.Int32)) für ganzzahlige RGB-Werte von 0 bis 255
- [`Color.FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Double,System.Double,System.Double,System.Double)) für `double` RGB-Werte, mit Transparenz
- [`Color.FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Int32,System.Int32,System.Int32,System.Int32)) für ganzzahlige RGB-Werte mit Transparenz
- [`Color.FromHsla`](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double)) für `double` HSL-Werte, mit Transparenz
- [`Color.FromUint`](xref:Xamarin.Forms.Color.FromUint(System.UInt32)) für eine `uint` Wert berechnet als (B + 256 * (G + 256 * (R + 256 * ein)))
- [`Color.FromHex`](xref:Xamarin.Forms.Color.FromHex(System.String)) für eine `string` Format von hexadezimalen Ziffern in der Form "#AARRGGBB" oder "#RRGGBB" oder "#ARGB" oder "#RGB", wobei jeder Buchstabe eine hexadezimale Ziffer für den Alpha-, Rot-, Grün- und blaue-Kanälen entspricht. Diese Methode ist primär für die Konvertierung von XAML-Farbe verwendet werden, wie unter [Kapitel 7, XAML und Code](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md).

Nach der Erstellung einer `Color` Wert ist unveränderlich. Die Merkmale der Farbe erhalten Sie unter den folgenden Eigenschaften:

- [`R`](xref:Xamarin.Forms.Color.R)
- [`G`](xref:Xamarin.Forms.Color.G)
- [`B`](xref:Xamarin.Forms.Color.B)
- [`A`](xref:Xamarin.Forms.Color.A)
- [`Hue`](xref:Xamarin.Forms.Color.Hue)
- [`Saturation`](xref:Xamarin.Forms.Color.Saturation)
- [`Luminosity`](xref:Xamarin.Forms.Color.Luminosity)

Hierbei handelt es sich um alle `double` Werte zwischen 0 und 1.

`Color` definiert außerdem 240 öffentliche statische schreibgeschützte Felder für allgemeine Farben. Zum Zeitpunkt der Buch geschrieben wurde, waren nur 17 allgemein verwendete Farben verfügbar.

Eine andere öffentlichen, statischen schreibgeschützten Felds definiert eine Farbe, mit der alle Farbkanäle auf 0 (null) festgelegt:

- [`Color.Transparent`](xref:Xamarin.Forms.Color.Transparent)

Mehrere Instanzmethoden zur Verfügung ermöglichen das Ändern einer vorhandenen Farbe, um eine neue Farbe zu erstellen:

- [`AddLuminosity`](xref:Xamarin.Forms.Color.AddLuminosity(System.Double))
- [`MultiplyAlpha`](xref:Xamarin.Forms.Color.MultiplyAlpha(System.Double))
- [`WithHue`](xref:Xamarin.Forms.Color.WithHue(System.Double))
- [`WithLuminosity`](xref:Xamarin.Forms.Color.WithLuminosity(System.Double))
- [`WithSaturation`](xref:Xamarin.Forms.Color.WithSaturation(System.Double))

Schließlich definieren Sie zwei statische schreibgeschützte Eigenschaften besondere Farbwert:

- [`Color.Default`](xref:Xamarin.Forms.Color.Default), legen Sie auf allen Kanälen &ndash;1
- [`Color.Accent`](xref:Xamarin.Forms.Color.Accent)

`Color.Default` Dient zum Erzwingen der Plattform Farbschema aus, und hat daher eine andere Bedeutung in unterschiedlichen Kontexten auf verschiedenen Plattformen. Sind standardmäßig die Farbschemas für die Plattform aus:

- iOS: Dunkel Text auf einem hellen Hintergrund
- Android: Hell Text auf dunklem Hintergrund (im Buch) "oder" dunkel Text auf einem hellen Hintergrund (für Material Design über AppCompat in die **master** Branch von beispielcoderepository)
- UWP: Dunkel Text auf einem hellen Hintergrund
- Windows 8.1: Heller Text auf dunklem Hintergrund
- Windows Phone 8.1: Heller Text auf dunklem Hintergrund

Die `Color.Accent` führt eine plattformspezifische (und manchmal Benutzer ausgewählt werden können) Farbe, die auf einem dunkle oder helle Hintergrund angezeigt wird.

## <a name="changing-the-application-color-scheme"></a>Ändern des Farbschemas der Anwendung

Die verschiedenen Plattformen haben ein Standardfarbschema, wie in der Liste oben gezeigt.

Wenn Sie Android nutzen zu können, kann zu einem Dark-auf-Light-Schema wechseln, indem Sie ein Design "hell" angeben, in der Datei Android.Manifest.xml oder [hinzufügen AppCompat und Material Design](~/xamarin-forms/platform/android/appcompat.md).

Für die Windows-Plattformen ist normalerweise das Farbschema "vom Benutzer ausgewählt, aber Sie können Hinzufügen einer `RequestedTheme` -Attributsatz auf `Light` oder `Dark` " App.xaml "-Datei von der Plattform. Standardmäßig enthält die Datei "App.xaml" im UWP-Projekt eine `RequestedTheme` -Attributsatz auf `Light`.

## <a name="font-sizes-and-attributes"></a>Schriftgrade Ihren Bedürfnissen entsprechend und Attribute

Legen Sie die [ `FontFamily` ](xref:Xamarin.Forms.Label.FontFamily) Eigenschaft `Label` in eine Zeichenfolge, z. B. "Times Roman", wählen Sie eine Schriftfamilie. Allerdings müssen Sie eine Schriftfamilie angeben, die auf der speziellen Plattform unterstützt wird, und die Plattformen in dieser Hinsicht inkonsistent sind.

Legen Sie die [ `FontSize` ](xref:Xamarin.Forms.Label.FontSize) Eigenschaft `Label` auf eine `double` für die ungefähre Höhe der Schriftart angibt. Finden Sie unter [Kapitel 5, Umgang mit Größen](chapter05.md), Weitere Informationen zum Auswählen auf intelligente Weise Schriftgrade Ihren Bedürfnissen entsprechend.

Alternativ können Sie eine von mehreren vordefinierten plattformabhängige Schriftgraden abrufen. Die statische [ `Device.GetNamedSize` ](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,System.Type)) Methode und [überladen](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,Xamarin.Forms.Element)) Zurückgeben einer `double` Schriftgradwert für die Plattform basierend auf Mitgliedern der der [ `NamedSize` ](xref:Xamarin.Forms.NamedSize)Enumeration ([`Default`](xref:Xamarin.Forms.NamedSize.Default), [ `Micro` ](xref:Xamarin.Forms.NamedSize.Micro), [ `Small` ](xref:Xamarin.Forms.NamedSize.Small), [ `Medium` ](xref:Xamarin.Forms.NamedSize.Medium),  und [ `Large` ](xref:Xamarin.Forms.NamedSize.Large)). Der Rückgabewert der `Medium` Member ist nicht unbedingt identisch mit `Default`. Die [ **NamedFontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) Beispiel zeigt Text mit diesen mit dem Namen Größen.

Legen Sie die [ `FontAttributes` ](xref:Xamarin.Forms.Label.FontAttributes) Eigenschaft `Label` auf einen Member dieser [ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes) Enumeration [ `Bold` ](xref:Xamarin.Forms.FontAttributes.Bold), [ `Italic` ](xref:Xamarin.Forms.FontAttributes.Italic), oder [ `None` ](xref:Xamarin.Forms.FontAttributes.None). Sie kombinieren, können die `Bold` und `Italic` Elemente mit den C#-bitweise OR-Operator.

## <a name="formatted-text"></a>Formatierter Text

In all den bisherigen Beispielen sind der gesamte Text angezeigt werden, indem die `Label` gleichmäßig formatiert wurde. Um die Formatierung innerhalb einer Textzeichenfolge zu variieren, legen Sie nicht die `Text` Eigenschaft `Label`. Legen Sie stattdessen die [ `FormattedText` ](xref:Xamarin.Forms.Label.FormattedText) Eigenschaft, um ein Objekt des Typs [ `FormattedString` ](xref:Xamarin.Forms.FormattedString).

`FormattedString` verfügt über eine [ `Spans` ](xref:Xamarin.Forms.FormattedString.Spans) -Eigenschaft, die eine Auflistung von [ `Span` ](xref:Xamarin.Forms.Span) Objekte. Jede `Span` Objekt verfügt über eine eigene [ `Text` ](xref:Xamarin.Forms.Span.Text), [ `FontFamily` ](xref:Xamarin.Forms.Span.FontFamily), [ `FontSize` ](xref:Xamarin.Forms.Span.FontSize), [ `FontAttributes` ](xref:Xamarin.Forms.Span.FontAttributes), [ `ForegroundColor` ](xref:Xamarin.Forms.Span.ForegroundColor), und [ `BackgroundColor` ](xref:Xamarin.Forms.Span.BackgroundColor) Eigenschaften.

Die [ **VariableFormattedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormText) Beispiel zeigt die Verwendung der `FormattedText` -Eigenschaft für eine einzelne Textzeile und [ **VariableFormattedParagraph** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormPara) wird das Verfahren für einen gesamten Absatz, veranschaulicht, wie hier gezeigt:

[![Dreifacher Screenshot der Variable formatierten Absatz](images/ch03fg06-small.png "Variable formatierten Text der Strukturknotenbezeichnung")](images/ch03fg06-large.png#lightbox "Variable formatierten Text der Strukturknotenbezeichnung")

Die [ **NamedFontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) Programm verwendet eine einzelne `Label` und `FormattedString` Objekt zum Anzeigen aller dem benannten Schriftgrade für jede Plattform.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 3 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch03-Apr2016.pdf)
- [Kapitel 3-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)
- [Kapitel 3 F#-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/FS)
- [Bezeichnung](~/xamarin-forms/user-interface/text/label.md)
- [Arbeiten mit Farben](~/xamarin-forms/user-interface/colors.md)
