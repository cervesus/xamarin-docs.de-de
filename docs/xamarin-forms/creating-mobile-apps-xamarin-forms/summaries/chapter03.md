---
title: Zusammenfassung der Kapitel 3. Eine umfassendere in text
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 9d283a4136a7cdfe39ea0b2da65273332fd47b00
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-3-deeper-into-text"></a>Zusammenfassung der Kapitel 3. Eine umfassendere in text

In diesem Kapitel werden die [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Anzeigen ausführlicher, z. B. Farben, Schriftarten und Formatierung.

## <a name="wrapping-paragraphs"></a>Umschließen von Absätzen

Wenn die [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) Eigenschaft `Label` langen Text enthält `Label` automatisch auf mehrere Zeilen umschließt, wie die [ **Baskervilles** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/Baskervilles) Beispiel. Sie können Unicode-Codes wie z. B. "\u2014" für die Em-Dash oder C#-Zeichen wie "\r" in eine neue Zeile unterbrechen einbetten.

Wenn die [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) und [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) Eigenschaften des eine `Label` festgelegt `LayoutOptions.Fill`, die Gesamtgröße des der `Label` unterliegt den Speicherplatz, Containers Stellt zur Verfügung. Die `Label` gilt als *eingeschränkte*. Die Größe der `Label` ist die Größe des Containers.

Wenn die `HorizontalOptions` und `VerticalOptions` Eigenschaften außer auf Werte festgelegt sind `LayoutOptions.Fill`, die Größe der `Label` unterliegt den Speicherplatz erforderlich, um den Text ein, bis die Größe zu rendern, die für der Container zur Verfügung der `Label`. Die `Label` gilt als *uneingeschränkte* und seine eigene Größe bestimmt.

(Hinweis: die Begriffe *eingeschränkte* und *uneingeschränkte* kann daran liegen, Counter-intuitiven, eine nicht eingeschränkte Ansicht in der Regel kleiner als eine eingeschränkte Ansicht ist. Darüber hinaus sind diese Begriffe nicht einheitlich in den frühen Kapiteln des Buchs verwendet.)

Eine Sicht, z. B. eine `Label` eingeschränkt, die in einer Dimension und in den anderen uneingeschränkten werden können. Ein `Label` wird nur in mehreren Zeilen Text umbrochen, wenn er horizontal eingeschränkt ist.

Wenn eine `Label` ist eingeschränkt, können sie erheblich mehr Speicherplatz als erforderlich für den Text beansprucht. Der Text innerhalb der gesamten Bereich der positioniert werden kann die `Label`. Legen Sie die [ `HorizontalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.HorizontalTextAlignment/) Eigenschaft an ein Mitglied der [ `TextAlignment` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextAlignment/) Enumeration ([`Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Start/), [ `Center` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Center/), oder [ `End` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Center/)) die Ausrichtung aller Zeilen aus dem Absatz. Die Standardeinstellung ist `Start` und den Text linksbündig.

Festlegen der [ `VerticalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.VerticalTextAlignment/) Eigenschaft an ein Mitglied der `TextAlignment` Enumeration zum Positionieren von der Text oben, Mitte oder unten auf der der Bereich der `Label`.

Legen Sie die [ `LineBreakMode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.LineBreakMode/) Eigenschaft an ein Mitglied der [ `LineBreakMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LineBreakMode/) Enumeration ([`WordWrap`](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.WordWrap/), [ `CharacterWrap` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.CharacterWrap/), [ `NoWrap` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.NoWrap/), [ `HeadTruncation` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.HeadTruncation/), [ `MiddleTruncation` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.MiddleTruncation/), oder [ `TailTruncation` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.TailTruncation/)), Steuerung, wie das Vielfache werden Zeilen in eine Absatzmarke oder abgeschnitten werden.

## <a name="text-and-background-colors"></a>Text- und Hintergrundfarben

Legen Sie die [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) und [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/) Eigenschaften des `Label` zu [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) Werte zu steuern, die Farbe von Text und Hintergrund.

Die `BackgroundColor` gilt für den Hintergrund des den gesamten Bereich der `Label`. Je nach den `HorizontalOptions` und `VerticalOptions` Eigenschaften, Größe möglicherweise erheblich größer als der Bereich erforderlich, um den Text anzuzeigen. Verwenden Sie Farbe, zum Experimentieren mit verschiedenen Werten von `HorizontalOptions`, `VerticalOptions`, `HorizontalExeAlignment`, und `VerticalTextAlignment` um anzuzeigen, wie sie die Größe und Position der Auswirkungen auf die `Label`, und die Größe und Position des Texts in der `Label`.

## <a name="the-color-structure"></a>Die Color-Struktur

Die [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) Struktur können Sie Farben als Rot-Grün-Blau (RGB), oder Werte für Farbton-Sättigung-Helligkeit (HSL) oder mit einem Color-Namen angeben. Ein Alphakanal ist auch verfügbar, um Transparenz anzugeben.

Verwenden einer `Color` Konstruktor angeben:

- eine [grauer Schattierung](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/)
- ein [RGB-Wertes](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/)
- ein [RGB-Wertes mit Transparenz](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/System.Double/)

Argumente sind `double` Werte im Bereich von 0 bis 1.

Sie können auch mehrere statische Methoden zum Erstellen `Color` Werte:

- [`Color.FromRgb`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgb/p/System.Double/System.Double/System.Double/) für `double` RGB-Werten von 0 bis 1
- [`Color.FromRgb`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgb/p/System.Int32/System.Int32/System.Int32/) für ganzzahlige RGB-Werte von 0 bis 255
- [`Color.FromRgba`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgba/p/System.Double/System.Double/System.Double/System.Double/) für `double` RGB-Werte mit Transparenz
- [`Color.FromRgba`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgba/p/System.Int32/System.Int32/System.Int32/System.Int32/) für die RGB-Werte für ganze Zahl mit Transparenz
- [`Color.FromHsla`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHsla/p/System.Double/System.Double/System.Double/System.Double/) für `double` HSL-Werte mit Transparenz
- [`Color.FromUint`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromUint/p/System.UInt32/) für eine `uint` Wert berechnet als (B + 256 * (G + 256 * (R + 256 * A)))
- [`Color.FromHex`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHex/p/System.String/) für eine `string` Format von hexadezimalen Ziffern im Format "#AARRGGBB" oder "#RRGGBB" oder "#ARGB" oder "#RGB", wobei jeder Buchstabe eine hexadezimale Ziffer für den Alpha-, Rot-, Grün- und blaue-Kanälen entspricht. Diese Methode ist für die Verwendung von XAML-Farbe Konvertierungen verwendet werden, wie beschrieben in primären [Kapitel 7, XAML und Code](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md).

Nach dem Erstellen einer `Color` Wert ist unveränderlich. Die Eigenschaften der ausgewählten Farbe können unter den folgenden Eigenschaften abgerufen werden:

- [`R`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.R/)
- [`G`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.G/)
- [`B`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.B/)
- [`A`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.A/)
- [`Hue`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Hue/)
- [`Saturation`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Saturation/)
- [`Luminosity`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Luminosity/)

Dies sind alle `double` Werte im Bereich von 0 bis 1.

`Color` definiert auch 240 öffentliche statische schreibgeschützte Felder für allgemeine Farben. Zum Zeitpunkt der Buch geschrieben wurde, waren nur 17 allgemein verwendete Farben verfügbar.

Eine andere öffentliche statisches schreibgeschützte Feld definiert eine Farbe mit allen Farbkanälen auf 0 (null) festgelegt:

- [`Color.Transparent`](https://developer.xamarin.com/api/field/Xamarin.Forms.Color.Transparent/)

Mehrere Instanzmethoden zulassen, ändern eine vorhandene Farbe aus, um eine neue Farbe zu erstellen:

- [`AddLuminosity`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.AddLuminosity/p/System.Double/)
- [`MultiplyAlpha`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.MultiplyAlpha/p/System.Double/)
- [`WithHue`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.WithHue/p/System.Double/)
- [`WithLuminosity`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.WithLuminosity/p/System.Double/)
- [`WithSaturation`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.WithSaturation/p/System.Double/)

Zwei statische schreibgeschützte Eigenschaften definieren Sie schließlich die spezielle Farbwert:

- [`Color.Default`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/), alle Kanäle zu legen & #x 2013; 1
- [`Color.Accent`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Accent/)

`Color.Default` Dient zum Erzwingen der Plattform Farbschema und daher hat eine andere Bedeutung in anderen Kontexten auf verschiedenen Plattformen. Standardmäßig sind die Farbschemas Plattform:

- iOS: Dunkel Text auf einer heller Hintergrund
- Android: Hell Text auf einem dunklen Hintergrund (im Buch) oder dunkel auf Hintergrund (für den Entwurf von Material über AppCompat in der **master** Verzweigung des Repositorys Code Beispiel)
- Universelle Windows-Plattform: Dunkel Text auf einer heller Hintergrund
- Windows 8.1: Hell Text auf einem dunklen Hintergrund
- Windows Phone 8.1: Hell Text auf einem dunklen Hintergrund

Die `Color.Accent` Wert der Ergebnisse in einer plattformspezifischen (und in einigen Fällen wählbar)-Farbe, die in einem Hintergrundthread dunkle oder helle sichtbar ist.

## <a name="changing-the-application-color-scheme"></a>Ändern des Farbschemas für die Anwendung

Die verschiedenen Plattformen haben ein Standard-Farbschema wie in der Liste oben gezeigt.

Wenn Android verwenden möchten, es ist möglich, einen dunklen-auf-Light-Schema zu wechseln, indem Sie ein Design "hell" angeben, in der Datei Android.Manifest.xml oder [AppCompat hinzufügen und Material Entwurf](~/xamarin-forms/platform/android/appcompat.md).

Für die Windows-Plattformen ist normalerweise das Farbschema vom Benutzer ausgewählt, Sie können jedoch Hinzufügen einer `RequestedTheme` Attributsatz entweder `Light` oder `Dark` in der Datei "App.xaml" der Plattform. Standardmäßig enthält die Datei "App.xaml" im uwp-Projekt eine `RequestedTheme` -Attributsatz zur `Light`.

## <a name="font-sizes-and-attributes"></a>Schriftgrade Ihren Bedürfnissen und Attribute

Legen Sie die [ `FontFamily` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FontFamily/) Eigenschaft `Label` in eine Zeichenfolge wie z. B. "Times Roman", wählen Sie eine Schriftfamilie. Allerdings müssen Sie eine Schriftfamilie angeben, die auf bestimmten Plattform unterstützt wird, und die Plattformen sind in dieser Hinsicht inkonsistent.

Festlegen der [ `FontSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FontSize/) Eigenschaft `Label` auf eine `double` für die ungefähre Höhe der Schriftart angeben. Finden Sie unter [Kapitel 5, arbeiten mit der Größe](chapter05.md), Weitere Informationen zum auswählen und Schriftgrade Ihren Bedürfnissen.

Sie können alternativ eine mehrere vordefinierte plattformabhängige Schriftgraden abrufen. Die statische [ `Device.GetNamedSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.GetNamedSize/p/Xamarin.Forms.NamedSize/System.Type/) Methode und [überladen](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.GetNamedSize/p/Xamarin.Forms.NamedSize/Xamarin.Forms.Element/) Zurückgeben einer `double` Wert des Schriftgrads für die Plattform entsprechende basierend auf Mitglieder der der [ `NamedSize` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NamedSize/)Enumeration ([`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Default/), [ `Micro` ](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Micro/), [ `Small` ](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Small/), [ `Medium` ](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Medium/),  und [ `Large` ](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Large/)). Der Rückgabewert der `Medium` Element ist nicht unbedingt identisch `Default`. Die [ **NamedFontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) Beispiel zeigt Text mit diese benannten Größe.

Legen Sie die [ `FontAttributes` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FontAttributes/) Eigenschaft `Label` auf einen Member dieser [ `FontAttributes` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FontAttributes/) -Enumeration, [ `Bold` ](https://developer.xamarin.com/api/field/Xamarin.Forms.FontAttributes.Bold/), [ `Italic` ](https://developer.xamarin.com/api/field/Xamarin.Forms.FontAttributes.Italic/), oder [ `None` ](https://developer.xamarin.com/api/field/Xamarin.Forms.FontAttributes.None/). Sie können Kombinieren der `Bold` und `Italic` Member mit dem C#-bitweise OR-Operator.

## <a name="formatted-text"></a>Formatierter Text

In allen Beispielen bisher, der gesamte Text angezeigt werden, indem die `Label` einheitlich formatiert wurde. Um die Formatierung innerhalb einer Textzeichenfolge zu variieren, legen Sie nicht die `Text` Eigenschaft `Label`. Legen Sie stattdessen die [ `FormattedText` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FormattedText/) Eigenschaft, um ein Objekt des Typs [ `FormattedString` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FormattedString/).

`FormattedString` verfügt über eine [ `Spans` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FormattedString.Spans/) -Eigenschaft, die eine Sammlung von [ `Span` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Span/) Objekte. Jede `Span` Objekt verfügt über eine eigene [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.Text/), [ `FontFamily` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.FontFamily/), [ `FontSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.FontSize/), [ `FontAttributes` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.FontAttributes/), [ `ForegroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.ForegroundColor/), und [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.BackgroundColor/) Eigenschaften.

Die [ **VariableFormattedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormText) Beispiel veranschaulicht die Verwendung der `FormattedText` -Eigenschaft für eine einzelne Textzeile und [ **VariableFormattedParagraph** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormPara) der Vorgehensweise für eine gesamte Absatz wird veranschaulicht, wie hier gezeigt:

[![Dreifacher Screenshot Variablen formatierten Absatz](images/ch03fg06-small.png "Variable formatierten Text der Strukturknotenbezeichnung")](images/ch03fg06-large.png "Variable formatiert Bezeichnungstext")

Die [ **NamedFontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) Anwendung verwendet ein einzelnes `Label` und ein `FormattedString` Objekt zum Anzeigen aller der benannten Schriftgrade Ihren Bedürfnissen für jede Plattform.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 3 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch03-Apr2016.pdf)
- [Kapitel 3-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)
- [Kapitel 3 f#-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/FS)
- [Bezeichnung](~/xamarin-forms/user-interface/text/label.md)
- [Arbeiten mit Farben](~/xamarin-forms/user-interface/colors.md)
