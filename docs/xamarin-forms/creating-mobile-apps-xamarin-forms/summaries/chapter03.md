---
title: Zusammenfassung zu Kapitel 3. Tiefer in Text
description: 'Erstellen von Mobile Apps mit xamarin. Forms: Zusammenfassung zu Kapitel 3. Tiefer in Text'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2E5581A6-4D3E-4BD5-9FDB-ACBA0F0FC734
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: 69415b59bbd376330454302981e3216c236a16bb
ms.sourcegitcommit: 93697a20e6fc7da547a8714ac109d7953b61d63f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2019
ms.locfileid: "72980921"
---
# <a name="summary-of-chapter-3-deeper-into-text"></a>Zusammenfassung zu Kapitel 3. Tiefer in Text

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)

In diesem Kapitel wird die [`Label`](xref:Xamarin.Forms.Label) Sicht ausführlicher behandelt, einschließlich Farbe, Schriftarten und Formatierung.

## <a name="wrapping-paragraphs"></a>Umrunden von Absätzen

Wenn die [`Text`](xref:Xamarin.Forms.Label.Text) -Eigenschaft von `Label` langen Text enthält, wird Sie von `Label` automatisch in mehrere Zeilen umschlossen, wie im [**Baskervilles**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/Baskervilles) -Beispiel veranschaulicht. Sie können Unicode-Codes wie "\u2014" für den EM-Bindestrich oder C# Zeichen wie "\r" einbetten, um eine neue Zeile zu unterbrechen.

Wenn die Eigenschaften [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) und [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) eines `Label` auf `LayoutOptions.Fill`festgelegt sind, wird die Gesamtgröße des `Label` durch den Speicherplatz geregelt, den der Container verfügbar macht. Der `Label` wird als *eingeschränkt*bezeichnet. Die Größe des `Label` ist die Größe des zugehörigen Containers.

Wenn die Eigenschaften `HorizontalOptions` und `VerticalOptions` auf andere Werte als `LayoutOptions.Fill`festgelegt sind, wird die Größe der `Label` durch den zum Darstellen des Texts erforderlichen Platz bis zu der Größe bestimmt, die der Container für den `Label`verfügbar macht. Der `Label` wird als *nicht eingeschränkt* bezeichnet und legt seine eigene Größe fest.

(Hinweis: Die Begriffe " *eingeschränkt* " und " *uneingeschränkt* " sind möglicherweise kontraintuitiv, da eine nicht eingeschränkte Ansicht in der Regel kleiner als eine eingeschränkte Ansicht ist. Außerdem werden diese Begriffe nicht konsistent in den frühen Kapiteln des Buchs verwendet.)

Eine Sicht, wie z. b. eine `Label`, kann in einer Dimension eingeschränkt und in der anderen nicht eingeschränkt werden. Ein `Label` umschließt nur Text in mehreren Zeilen, wenn er horizontal eingeschränkt ist.

Wenn eine `Label` eingeschränkt ist, kann Sie erheblich mehr Speicherplatz beanspruchen als für den Text erforderlich. Der Text kann im gesamten Bereich der `Label`positioniert werden. Legen Sie die [`HorizontalTextAlignment`](xref:Xamarin.Forms.Label.HorizontalTextAlignment) -Eigenschaft auf einen Member der [`TextAlignment`](xref:Xamarin.Forms.TextAlignment) Enumeration ([`Start`](xref:Xamarin.Forms.TextAlignment.Start), [`Center`](xref:Xamarin.Forms.TextAlignment.Center)oder [`End`](xref:Xamarin.Forms.TextAlignment.Center)) fest, um die Ausrichtung aller Zeilen des Absatzes zu steuern. Der Standardwert ist `Start` und linksbündig ausgerichtet.

Legen Sie die [`VerticalTextAlignment`](xref:Xamarin.Forms.Label.VerticalTextAlignment) -Eigenschaft auf einen Member der `TextAlignment`-Enumeration fest, um den Text am oberen, mittleren oder unteren Rand des Bereichs zu positionieren, der vom `Label`besetzt ist.

Legen Sie die [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) -Eigenschaft auf einen Member der [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode) Enumeration ([`WordWrap`](xref:Xamarin.Forms.LineBreakMode.WordWrap), [`CharacterWrap`](xref:Xamarin.Forms.LineBreakMode.CharacterWrap), [`NoWrap`](xref:Xamarin.Forms.LineBreakMode.NoWrap), [`HeadTruncation`](xref:Xamarin.Forms.LineBreakMode.HeadTruncation), [`MiddleTruncation`](xref:Xamarin.Forms.LineBreakMode.MiddleTruncation)oder [`TailTruncation`](xref:Xamarin.Forms.LineBreakMode.TailTruncation)) fest, um zu steuern, wie das Vielfache Zeilen in einem Absatz Umbruch oder werden abgeschnitten.

## <a name="text-and-background-colors"></a>Text-und Hintergrundfarben

Legen Sie die Eigenschaften [`TextColor`](xref:Xamarin.Forms.Label.TextColor) und [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) von `Label` auf [`Color`](xref:Xamarin.Forms.Color) Werte fest, um die Farbe von Text und Hintergrund zu steuern.

Der `BackgroundColor` gilt für den Hintergrund des gesamten Bereichs, der durch die `Label`belegt ist. Abhängig von den Eigenschaften `HorizontalOptions` und `VerticalOptions` kann diese Größe erheblich größer sein als der Bereich, der zum Anzeigen des Texts erforderlich ist. Mit Color können Sie verschiedene Werte von `HorizontalOptions`, `VerticalOptions`, `HorizontalExeAlignment`und `VerticalTextAlignment` experimentieren, um zu sehen, wie sich diese auf die Größe und Position des `Label`auswirken, sowie die Größe und Position des Texts innerhalb der `Label`.

## <a name="the-color-structure"></a>Die Farbstruktur

Mit der [`Color`](xref:Xamarin.Forms.Color) -Struktur können Sie Farben als Werte für rot-grün-blau (RGB) oder Hue-Sättigung-Luminosity (HSL) oder mit einem Farbnamen angeben. Ein Alpha Kanal ist auch verfügbar, um Transparenz anzuzeigen.

Verwenden Sie einen `Color`-Konstruktor, um Folgendes anzugeben:

- [grauer Farbton](xref:Xamarin.Forms.Color.%23ctor(System.Double))
- ein [RGB-Wert](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double))
- ein [RGB-Wert mit Transparenz](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double,System.Double))

Argumente sind `double` Werte zwischen 0 und 1.

Sie können auch mehrere statische Methoden verwenden, um `Color` Werte zu erstellen:

- [`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Double,System.Double,System.Double)) für `double` RGB-Werte von 0 bis 1
- [`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Int32,System.Int32,System.Int32)) für ganzzahlige RGB-Werte von 0 bis 255
- [`Color.FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Double,System.Double,System.Double,System.Double)) für `double` RGB-Werte mit Transparenz
- [`Color.FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Int32,System.Int32,System.Int32,System.Int32)) für ganzzahlige RGB-Werte mit Transparenz
- [`Color.FromHsla`](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double)) für `double` HSL-Werte mit Transparenz
- [`Color.FromUint`](xref:Xamarin.Forms.Color.FromUint(System.UInt32)) für einen `uint` Wert berechnet als (B + 256 \* (G + 256 \* (R + 256 \* a)))
- [`Color.FromHex`](xref:Xamarin.Forms.Color.FromHex(System.String)) für ein `string` Format von hexadezimalen Ziffern in der Form "#AARRGGBB" oder "#RRGGBB" oder "#ARGB" oder "#RGB", wobei jeder Buchstabe einer hexadezimalen Ziffer für die Alpha-, rot-, grün-und blauen Kanäle entspricht. Diese Methode wird für XAML-Farb Konvertierungen als primär verwendet [, wie in Kapitel 7, XAML und Code](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md)erläutert.

Nach der Erstellung kann ein `Color` Wert unveränderlich sein. Die Merkmale der Farbe können aus den folgenden Eigenschaften abgerufen werden:

- [`R`](xref:Xamarin.Forms.Color.R)
- [`G`](xref:Xamarin.Forms.Color.G)
- [`B`](xref:Xamarin.Forms.Color.B)
- [`A`](xref:Xamarin.Forms.Color.A)
- [`Hue`](xref:Xamarin.Forms.Color.Hue)
- [`Saturation`](xref:Xamarin.Forms.Color.Saturation)
- [`Luminosity`](xref:Xamarin.Forms.Color.Luminosity)

Dabei handelt es sich um alle `double` Werte zwischen 0 und 1.

`Color` definiert auch 240 öffentliche statische schreibgeschützte Felder für allgemeine Farben. Zum Zeitpunkt des Schreibens des Buchs waren nur 17 gängige Farben verfügbar.

Ein anderes öffentliches statisches Schreib geschütztes Feld definiert eine Farbe, bei der alle Farbkanäle auf NULL festgelegt sind:

- [`Color.Transparent`](xref:Xamarin.Forms.Color.Transparent)

Mehrere Instanzmethoden ermöglichen das Ändern einer vorhandenen Farbe, um eine neue Farbe zu erstellen:

- [`AddLuminosity`](xref:Xamarin.Forms.Color.AddLuminosity(System.Double))
- [`MultiplyAlpha`](xref:Xamarin.Forms.Color.MultiplyAlpha(System.Double))
- [`WithHue`](xref:Xamarin.Forms.Color.WithHue(System.Double))
- [`WithLuminosity`](xref:Xamarin.Forms.Color.WithLuminosity(System.Double))
- [`WithSaturation`](xref:Xamarin.Forms.Color.WithSaturation(System.Double))

Zum Schluss definieren zwei statische, schreibgeschützte Eigenschaften einen besonderen Farbwert:

- [`Color.Default`](xref:Xamarin.Forms.Color.Default)werden alle Kanäle auf &ndash;1 festgelegt.
- [`Color.Accent`](xref:Xamarin.Forms.Color.Accent)

`Color.Default` soll das Farbschema der Plattform erzwingen und folglich eine andere Bedeutung in verschiedenen Kontexten auf verschiedenen Plattformen haben. Standardmäßig lauten die Platt Form Farbschemas wie folgt:

- iOS: Dunkler Text auf einem hellen Hintergrund
- Android: Heller Text in einem dunklen Hintergrund (im Buch) oder dunkler Text auf einem hellen Hintergrund (für Material Design über AppCompat im **masterb** Ranch des beispielcoderepositorys)
- UWP Dunkler Text auf einem hellen Hintergrund

Der `Color.Accent` Wert ergibt eine plattformspezifische (und manchmal Benutzer auswählbare) Farbe, die entweder in einem dunklen oder hellen Hintergrund sichtbar ist.

## <a name="changing-the-application-color-scheme"></a>Ändern des Anwendungs Farbschemas

Die verschiedenen Plattformen verfügen über ein Standardfarbschema, wie in der obigen Liste gezeigt.

Bei Verwendung von Android ist es möglich, zu einem Dark-on-Light-Schema zu wechseln, indem ein helles Design in der Datei "Android. manifest. xml" angegeben wird, oder durch [Hinzufügen von AppCompat und Material Design](~/xamarin-forms/platform/android/appcompat-material-design.md).

Für die Windows-Plattformen wird das Farbschema normalerweise vom Benutzer ausgewählt, aber Sie können ein `RequestedTheme` Attribut, das entweder `Light` oder `Dark` in der app. XAML-Datei der Plattform festgelegt ist, hinzufügen. Standardmäßig enthält die Datei app. XAML im UWP-Projekt ein `RequestedTheme` Attribut, das auf `Light`festgelegt ist.

## <a name="font-sizes-and-attributes"></a>Schriftgrößen und Attribute

Legen Sie die [`FontFamily`](xref:Xamarin.Forms.Label.FontFamily) -Eigenschaft von `Label` auf eine Zeichenfolge wie z. b. "Times Roman" fest, um eine Schriftfamilie auszuwählen. Sie müssen jedoch eine Schriftfamilie angeben, die auf der jeweiligen Plattform unterstützt wird, und die Plattformen sind in dieser Hinsicht inkonsistent.

Legen Sie die [`FontSize`](xref:Xamarin.Forms.Label.FontSize) -Eigenschaft von `Label` auf eine `double` zum Angeben der ungefähren Höhe der Schriftart fest. Weitere Informationen zum intelligenten auswählen von Schriftgrößen finden Sie in [Kapitel 5, Umgang mit Größen](chapter05.md).

Alternativ können Sie eine von mehreren vordefinierten Platt Form abhängigen Schriftgrößen abrufen. Die statischen [`Device.GetNamedSize`](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,System.Type))-Methode und die [Überladung](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,Xamarin.Forms.Element)) geben beide einen `double` Schriftart Größen Wert zurück, der für die Plattform auf der Grundlage von Membern der [`NamedSize`](xref:Xamarin.Forms.NamedSize)-Enumeration ([`Default`](xref:Xamarin.Forms.NamedSize.Default), [`Micro`](xref:Xamarin.Forms.NamedSize.Micro), [`Small`](xref:Xamarin.Forms.NamedSize.Small), [`Medium`](xref:Xamarin.Forms.NamedSize.Medium), und [`Large`](xref:Xamarin.Forms.NamedSize.Large)). Der Wert, der vom `Medium`-Member zurückgegeben wird, ist nicht notwendigerweise identisch mit `Default`. Das Beispiel " [**namedfontsizes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) " zeigt Text mit diesen benannten Größen an.

Legen Sie die [`FontAttributes`](xref:Xamarin.Forms.Label.FontAttributes) -Eigenschaft `Label` auf einen Member dieser [`FontAttributes`](xref:Xamarin.Forms.FontAttributes) Enumeration, [`Bold`](xref:Xamarin.Forms.FontAttributes.Bold), [`Italic`](xref:Xamarin.Forms.FontAttributes.Italic)oder [`None`](xref:Xamarin.Forms.FontAttributes.None)fest. Sie können die `Bold`-und `Italic`-Elemente mit C# dem bitweisen OR-Operator kombinieren.

## <a name="formatted-text"></a>Formatierter Text

In allen bisherigen Beispielen wurde der gesamte Text, der vom `Label` angezeigt wird, einheitlich formatiert. Um die Formatierung innerhalb einer Text Zeichenfolge zu verändern, legen Sie die `Text`-Eigenschaft von `Label`nicht fest. Legen Sie stattdessen die [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) -Eigenschaft auf ein Objekt vom Typ [`FormattedString`](xref:Xamarin.Forms.FormattedString)fest.

`FormattedString` verfügt über eine [`Spans`](xref:Xamarin.Forms.FormattedString.Spans) -Eigenschaft, die eine Auflistung von [`Span`](xref:Xamarin.Forms.Span) -Objekten ist. Jedes `Span` Objekt verfügt über eigene Eigenschaften für [`Text`](xref:Xamarin.Forms.Span.Text), [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily), [`FontSize`](xref:Xamarin.Forms.Span.FontSize), [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes), [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor)und [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor) .

Das [**variableformattedtext**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormText) -Beispiel veranschaulicht die Verwendung der `FormattedText`-Eigenschaft für eine einzelne Textzeile, und [**variableformattedparagraph**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormPara) veranschaulicht die Technik für einen gesamten Absatz, wie hier gezeigt:

[![Dreifacher Screenshot des Variablen formatierten Absatzes](images/ch03fg06-small.png "Text der Variablen formatierten Bezeichnung")](images/ch03fg06-large.png#lightbox "Text der Variablen formatierten Bezeichnung")

Das [**namedfontsizes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) -Programm verwendet eine einzelne `Label` und ein `FormattedString`-Objekt, um alle benannten Schriftgrößen für jede Plattform anzuzeigen.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 3 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch03-Apr2016.pdf)
- [Kapitel 3-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)
- [Kapitel 3 F# -Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/FS)
- [Bezeichnung](~/xamarin-forms/user-interface/text/label.md)
- [Arbeiten mit Farben](~/xamarin-forms/user-interface/colors.md)
