---
title: Zusammenfassung von Kapitel 3. Details zum Text
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung von Kapitel 3. Details zum Text'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2E5581A6-4D3E-4BD5-9FDB-ACBA0F0FC734
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5423a9f716f384eca107003bdeca69615f8b459f
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136902"
---
# <a name="summary-of-chapter-3-deeper-into-text"></a>Zusammenfassung von Kapitel 3. Details zum Text

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)

In diesem Kapitel wird die [`Label`](xref:Xamarin.Forms.Label)-Ansicht ausführlicher erläutert, einschließlich Farbe, Schriftarten und Formatierung.

## <a name="wrapping-paragraphs"></a>Umbrechen von Absätzen

Wenn die [`Text`](xref:Xamarin.Forms.Label.Text)-Eigenschaft von `Label` langen Text enthält, wird dieser von `Label` automatisch in mehrere Zeilen umbrochen, wie im [**Baskervilles**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/Baskervilles)-Beispiel veranschaulicht wird. Sie können Unicode-Codes wie „\u2014“ für einen Geviertstrich (EM-Dash) oder C#-Zeichen wie „\r“ für einen Zeilenumbruch einbetten.

Wenn die Eigenschaften [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) und [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) eines `Label` auf `LayoutOptions.Fill` festgelegt sind, wird die Gesamtgröße des `Label` durch den Platz bestimmt, den der Container zur Verfügung stellt. Man spricht auch davon, dass das `Label` *eingeschränkt* ist. Die Größe des `Label` entspricht der Größe seines Containers.

Wenn die Eigenschaften `HorizontalOptions` und `VerticalOptions` auf andere Werte als `LayoutOptions.Fill` festgelegt sind, wird die Größe des `Label` durch den Platz bestimmt, der zum Rendern des Texts erforderlich ist, bis maximal zu der Größe, die der Container für das `Label` zur Verfügung stellt. Man spricht davon, dass das `Label` *uneingeschränkt* ist und seine eigene Größe festlegt.

(Hinweis: Die Begriffe *eingeschränkt* und *uneingeschränkt* widersprechen eventuell der Intuition, da eine uneingeschränkte Ansicht in der Regel kleiner als eine eingeschränkte Ansicht ist. Außerdem werden diese Begriffe in den vorangehenden Kapiteln des Buchs nicht konsistent verwendet.)

Eine Ansicht wie `Label` kann in einer Dimension eingeschränkt und in einer anderen uneingeschränkt sein. Ein `Label` umbricht Text nur dann auf mehrere Zeilen, wenn er horizontal eingeschränkt ist.

Wenn ein `Label` eingeschränkt ist, kann es erheblich mehr Platz beanspruchen, als für den Text erforderlich ist. Der Text kann im gesamten Bereich des `Label` positioniert werden. Legen Sie die [`HorizontalTextAlignment`](xref:Xamarin.Forms.Label.HorizontalTextAlignment)-Eigenschaft auf einen Member der [`TextAlignment`](xref:Xamarin.Forms.TextAlignment)-Enumeration fest ([`Start`](xref:Xamarin.Forms.TextAlignment.Start), [`Center`](xref:Xamarin.Forms.TextAlignment.Center) oder [`End`](xref:Xamarin.Forms.TextAlignment.Center)), um die Ausrichtung aller Zeilen des Absatzes zu kontrollieren. Der Standardwert ist `Start`, wodurch der Text linksbündig ausgerichtet wird.

Legen Sie die [`VerticalTextAlignment`](xref:Xamarin.Forms.Label.VerticalTextAlignment)-Eigenschaft auf einen Member der `TextAlignment`-Enumeration fest, um den Text oben, zentriert oder unten in dem Bereich zu positionieren, der von dem `Label` belegt ist.

Legen Sie die [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode)-Eigenschaft auf einen Member der [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode)-Enumeration fest ([`WordWrap`](xref:Xamarin.Forms.LineBreakMode.WordWrap), [`CharacterWrap`](xref:Xamarin.Forms.LineBreakMode.CharacterWrap), [`NoWrap`](xref:Xamarin.Forms.LineBreakMode.NoWrap), [`HeadTruncation`](xref:Xamarin.Forms.LineBreakMode.HeadTruncation), [`MiddleTruncation`](xref:Xamarin.Forms.LineBreakMode.MiddleTruncation) oder [`TailTruncation`](xref:Xamarin.Forms.LineBreakMode.TailTruncation)), um zu kontrollieren, wie mehrere Zeilen ein einem Absatz umbrochen oder abgeschnitten werden.

## <a name="text-and-background-colors"></a>Text- und Hintergrundfarben

Legen Sie die Eigenschaften [`TextColor`](xref:Xamarin.Forms.Label.TextColor) und [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) von `Label` auf [`Color`](xref:Xamarin.Forms.Color)-Werte fest, um die Farbe von Text und Hintergrund zu kontrollieren.

`BackgroundColor` gilt für den Hintergrund des gesamten Bereichs, der von dem `Label` belegt ist. Abhängig von den Eigenschaften `HorizontalOptions` und `VerticalOptions` kann diese Größe erheblich größer sein als der Bereich, der zum Anzeigen des Texts erforderlich ist. Sie können „Color“ (Farbe) verwenden, um mit verschiedenen Werten von `HorizontalOptions`, `VerticalOptions`, `HorizontalExeAlignment` und `VerticalTextAlignment` zu experimentieren, um zu sehen, wie sich diese auf Größe und Position des `Label` auswirken sowie auf Größe und Position des Texts innerhalb des `Label`.

## <a name="the-color-structure"></a>Die Color-Struktur

Mit der [`Color`](xref:Xamarin.Forms.Color)-Struktur können Sie Farben als RGB-Werte (Rot-Grün-Blau), als HSL-Werte (Hue-Saturation-Luminosity; Farbton-Sättigung-Leuchtdichte) oder mit einem Farbnamen angeben. Ein Alphakanal ist außerdem verfügbar, um die Transparenz anzugeben.

Verwenden Sie einen `Color`-Konstruktor, um Folgendes anzugeben:

- eine [Grauschattierung](xref:Xamarin.Forms.Color.%23ctor(System.Double))
- einen [RGB-Wert](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double))
- einen [RGB-Wert mit Transparenz](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double,System.Double))

Argumente sind `double`-Werte von 0 bis 1.

Sie können auch mehrere statische Methoden verwenden, um `Color`-Werte zu erstellen:

- [`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Double,System.Double,System.Double)) für RGB-Werte vom Typ `double` von 0 bis 1
- [`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Int32,System.Int32,System.Int32)) für RGB-Werte von 0 bis 255
- [`Color.FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Double,System.Double,System.Double,System.Double)) für RGB-Werte vom Typ `double` mit Transparenz
- [`Color.FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Int32,System.Int32,System.Int32,System.Int32)) für ganzzahlige RGB-Werte (integer) mit Transparenz
- [`Color.FromHsla`](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double)) für HSL-Werte vom Typ `double` mit Transparenz
- [`Color.FromUint`](xref:Xamarin.Forms.Color.FromUint(System.UInt32)) für einen `uint`-Wert, berechnet als (B + 256 \* (G + 256 \* (R + 256 \* A)))
- [`Color.FromHex`](xref:Xamarin.Forms.Color.FromHex(System.String)) für ein `string`-Format aus hexadezimalen Ziffern in der Form „#AARRGGBB“ oder „#RRGGBB“ oder „#ARGB“ oder „#RGB“, wobei jeder Buchstabe einer hexadezimalen Ziffer für den Alpha-, Rot-, Grün- und Blaukanal entspricht. Diese Methode wird primär für XAML-Farbkonvertierungen verwendet, wie in [Kapitel 7, „XAML oder Code“](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md), erläutert.

Nachdem ein `Color`-Wert einmal erstellt ist, ist er unveränderlich. Die Eigenschaften der Farbe können aus den folgenden Eigenschaften abgerufen werden:

- [`R`](xref:Xamarin.Forms.Color.R)
- [`G`](xref:Xamarin.Forms.Color.G)
- [`B`](xref:Xamarin.Forms.Color.B)
- [`A`](xref:Xamarin.Forms.Color.A)
- [`Hue`](xref:Xamarin.Forms.Color.Hue)
- [`Saturation`](xref:Xamarin.Forms.Color.Saturation)
- [`Luminosity`](xref:Xamarin.Forms.Color.Luminosity)

Hierbei handelt es sich bei allen um `double`-Werte von 0 bis 1.

`Color` definiert außerdem 240 öffentliche, statische, schreibgeschützte Felder für gängige Farben. Zum Zeitpunkt des Verfassens des Buchs waren nur 17 gängige Farben verfügbar.

Ein anderes öffentliches, statisches, schreibgeschütztes Feld definiert eine Farbe, bei der alle Farbkanäle auf Null festgelegt sind:

- [`Color.Transparent`](xref:Xamarin.Forms.Color.Transparent)

Mehrere Instanzmethoden erlauben das Ändern einer vorhandenen Farbe, um eine neue Farbe zu erstellen:

- [`AddLuminosity`](xref:Xamarin.Forms.Color.AddLuminosity(System.Double))
- [`MultiplyAlpha`](xref:Xamarin.Forms.Color.MultiplyAlpha(System.Double))
- [`WithHue`](xref:Xamarin.Forms.Color.WithHue(System.Double))
- [`WithLuminosity`](xref:Xamarin.Forms.Color.WithLuminosity(System.Double))
- [`WithSaturation`](xref:Xamarin.Forms.Color.WithSaturation(System.Double))

Schließlich definieren zwei statische, schreibgeschützte Eigenschaften einen besonderen Farbwert:

- [`Color.Default`](xref:Xamarin.Forms.Color.Default), bei dem alle Kanäle auf &ndash;1 festgelegt sind.
- [`Color.Accent`](xref:Xamarin.Forms.Color.Accent)

`Color.Default` dient zum Erzwingen des Farbschemas der Plattform und hat folglich in verschiedenen Kontexten auf unterschiedlichen Plattformen eine andere Bedeutung. Standardmäßig lauten die Plattformfarbschemas wie folgt:

- iOS: Dunkler Text auf hellem Hintergrund.
- Android: Heller Text auf einem dunklen Hintergrund (im Buch) oder dunkler Text auf einem hellen Hintergrund (für Material Design über AppCompat im **Master**-Branch des Beispielcoderepositorys).
- UWP: Dunkler Text auf einem hellem Hintergrund.

Der `Color.Accent`-Wert ergibt eine plattformspezifische (und manchmal vom Benutzer auswählbare) Farbe, die entweder auf einem dunklen oder hellen Hintergrund sichtbar ist.

## <a name="changing-the-application-color-scheme"></a>Ändern des Anwendungsfarbschemas

Die verschiedenen Plattformen verfügen über ein Standardfarbschema, wie in der obigen Liste gezeigt.

Wenn Sie auf Android abzielen, ist es möglich, zu einem Dunkel-auf-Hell-Schema zu wechseln, indem Sie ein helles Design in der Datei „Android.manifest.xml“ angeben, oder indem Sie [AppCompat und Material Design hinzufügen](~/xamarin-forms/platform/android/appcompat-material-design.md).

Bei den Windows-Plattformen wird das Farbschema normalerweise vom Benutzer ausgewählt, aber Sie können ein `RequestedTheme`-Attribut hinzufügen, das in der Datei „App.xaml“ der Plattform entweder auf `Light` oder auf `Dark` festgelegt ist. Standardmäßig enthält die Datei „App.xaml“ im UWP-Projekt ein `RequestedTheme`-Attribut, das auf `Light` festgelegt ist.

## <a name="font-sizes-and-attributes"></a>Schriftgrade und Attribute

Legen Sie die [`FontFamily`](xref:Xamarin.Forms.Label.FontFamily)-Eigenschaft von `Label` auf eine Zeichenfolge wie „Times Roman“ fest, um eine Schriftfamilie auszuwählen. Sie müssen jedoch eine Schriftfamilie angeben, die auf der jeweiligen Plattform unterstützt wird, wobei die Plattformen in dieser Hinsicht inkonsistent sind.

Legen Sie die [`FontSize`](xref:Xamarin.Forms.Label.FontSize)-Eigenschaft von `Label` auf einen `double`-Wert fest, um die ungefähre Höhe der Schriftart anzugeben. Weitere Informationen zur intelligenten Auswahl von Schriftgraden finden Sie in [Kapitel 5, „Umgang mit Größen“](chapter05.md).

Alternativ können Sie einen von mehreren voreingestellten plattformabhängigen Schriftgraden abrufen. Für die statische [`Device.GetNamedSize`](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,System.Type))-Methode und die [Überladung](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,Xamarin.Forms.Element)) wird jeweils ein für die Plattform geeigneter `double`-Wert für den Schriftgrad zurückgegeben. Dies erfolgt basierend auf den Membern der Enumeration [`NamedSize`](xref:Xamarin.Forms.NamedSize) ([`Default`](xref:Xamarin.Forms.NamedSize.Default), [`Micro`](xref:Xamarin.Forms.NamedSize.Micro), [`Small`](xref:Xamarin.Forms.NamedSize.Small), [`Medium`](xref:Xamarin.Forms.NamedSize.Medium) und [`Large`](xref:Xamarin.Forms.NamedSize.Large)). Der vom `Medium`-Member zurückgegebene Wert ist nicht notwendigerweise identisch mit `Default`. Im [**NamedFontSizes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes)-Beispiel wird Text mit diesen benannten Größen angezeigt.

Legen Sie die [`FontAttributes`](xref:Xamarin.Forms.Label.FontAttributes)-Eigenschaft von `Label` auf einen Member dieser [`FontAttributes`](xref:Xamarin.Forms.FontAttributes)-Enumeration fest, [`Bold`](xref:Xamarin.Forms.FontAttributes.Bold), [`Italic`](xref:Xamarin.Forms.FontAttributes.Italic) oder [`None`](xref:Xamarin.Forms.FontAttributes.None). Sie können die Member `Bold` und `Italic` mit dem bitweisen OR-Operator von C# kombinieren.

## <a name="formatted-text"></a>Formatierter Text

In allen bisherigen Beispielen wurde der gesamte Text, der von `Label` angezeigt wird, einheitlich formatiert. Um die Formatierung innerhalb einer Textzeichenfolge zu variieren, legen Sie die `Text`-Eigenschaft von `Label` nicht fest. Legen Sie stattdessen die [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText)-Eigenschaft auf ein Objekt des Typs [`FormattedString`](xref:Xamarin.Forms.FormattedString) fest.

`FormattedString` besitzt eine [`Spans`](xref:Xamarin.Forms.FormattedString.Spans)-Eigenschaft, die eine Sammlung von [`Span`](xref:Xamarin.Forms.Span)-Objekten ist. Jedes `Span`-Objekt verfügt über eigene Eigenschaften [`Text`](xref:Xamarin.Forms.Span.Text), [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily), [`FontSize`](xref:Xamarin.Forms.Span.FontSize), [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes), [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor) und [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor).

Das [**VariableFormattedText-** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormText)-Beispiel veranschaulicht die Verwendung der `FormattedText`-Eigenschaft für eine einzelne Textzeile, und [**VariableFormattedParagraph**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormPara) demonstriert die Methode für einen ganzen Absatz, wie hier gezeigt:

[![Dreifacher Screenshot eines unterschiedlich formatierten Absatzes](images/ch03fg06-small.png "Unterschiedlich formatierter Bezeichnungstext")](images/ch03fg06-large.png#lightbox "Unterschiedlich formatierter Bezeichnungstext")

Das [**NamedFontSizes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes)-Programm verwendet ein einzelnes `Label` und ein `FormattedString`-Objekt, um alle benannten Schriftgrößen für jede Plattform anzuzeigen.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 3 – vollständiger Text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch03-Apr2016.pdf)
- [Kapitel 3 – Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)
- [Kapitel 3 – F#-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/FS)
- [Bezeichnung](~/xamarin-forms/user-interface/text/label.md)
- [Arbeiten mit Farben](~/xamarin-forms/user-interface/colors.md)
