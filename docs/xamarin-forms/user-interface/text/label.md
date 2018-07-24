---
title: Xamarin.Forms-Bezeichnung
description: In diesem Artikel wird erläutert, wie Xamarin.Forms Label-Klasse zu verwenden, um einzelne und mehrzeiligen Text in Anwendungen anzuzeigen.
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/16/2018
ms.openlocfilehash: b5069381126db0859508480df5596ed5ec85686f
ms.sourcegitcommit: 4c0093ee5d4aeb16c0e6f0c740c4796736971651
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2018
ms.locfileid: "39203035"
---
# <a name="xamarinforms-label"></a>Xamarin.Forms-Bezeichnung

_Anzeigen von Text in Xamarin.Forms_

Die [ `Label` ](xref:Xamarin.Forms.Label) Ansicht dient zum Anzeigen von Text, sowohl einzelne als auch mit mehreren Zeilen. Bezeichnungen können benutzerdefinierte Schriftarten (Familien, Größen und Optionen) und farbige Text sein.

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>Abschneiden und-Umbruch

Bezeichnungen können festgelegt werden, um Text zu behandeln, die nicht in einer Zeile in eine von mehreren Möglichkeiten, die verfügbar gemacht werden, indem Sie passen die `LineBreakMode` Eigenschaft. [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode) ist eine Enumeration mit den folgenden Werten:

- **HeadTruncation** &ndash; schneidet den Anfang der Text am Ende angezeigt.
- **CharacterWrap** &ndash; Text auf einer neuen Zeile an einer Zeichengrenze umschließt.
- **MiddleTruncation** &ndash; zeigt an, die Anfang und Ende des Texts, mit der mittleren ersetzen Sie dies durch ein Auslassungszeichen.
- **NoWrap** &ndash; wird die Anzeige von nur-Text nicht umbrochen so viel Text kann in einer Zeile passen.
- **TailTruncation** &ndash; zeigt den Anfang des Texts, Ende abgeschnitten.
- **WordWrap** &ndash; Zeilenumbruch an der Wortgrenze.

## <a name="fonts"></a>Schriftarten

Weitere Informationen zum Angeben von Schriftarten auf einem `Label`, finden Sie unter [Schriftarten](~/xamarin-forms/user-interface/text/fonts.md).

## <a name="colors"></a>Farben

`Label`s kann festgelegt werden, verwenden Sie eine benutzerdefinierte Farbe, die über die bindbare [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor) Eigenschaft.

Besondere Sorgfalt ist erforderlich, um sicherzustellen, dass Farben auf jeder Plattform verwendet werden können. Da jede Plattform unterschiedliche Standardwerte für Text- und Hintergrundfarben verfügt, müssen Sie darauf achten, einen Standardwert auswählen, der auf den einzelnen funktioniert.

Verwenden Sie den folgenden XAML, um die Textfarbe einer Beschriftung festzulegen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="TextSample.LabelPage"
             Title="Label Demo">
    <StackLayout Padding="5,10">
      <Label TextColor="#77d065" FontSize = "20" Text="This is a label." />
    </StackLayout>
</ContentPage>
```

Der entsprechende C#-Code ist:

```csharp
public partial class LabelPage : ContentPage
{
    public LabelPage ()
    {
        InitializeComponent ();

        var layout = new StackLayout { Padding = new Thickness(5,10) };
        var label = new Label { Text="This is a label.", TextColor = Color.FromHex("#77d065"), FontSize = 20 };
        layout.Children.Add(label);
        this.Content = layout;
    }
}
```

Die folgenden Screenshots zeigen das Ergebnis der Einstellung der `TextColor` Eigenschaft:

![](label-images/textcolor.png "Bezeichnung TextColor-Beispiel")

Weitere Informationen zu Farben, finden Sie unter [Farben](~/xamarin-forms/user-interface/colors.md).

<a name="Formatted_Text" />

## <a name="formatted-text"></a>Formatierter Text

Bezeichnungen verfügbar machen eine [ `FormattedText` ](xref:Xamarin.Forms.Label.FormattedText) Eigenschaft ermöglicht die Darstellung von Text mit mehreren Schriftarten und Farben in derselben Ansicht.

Die `FormattedText` Eigenschaft ist vom Typ [ `FormattedString` ](xref:Xamarin.Forms.FormattedString), die umfasst eine oder mehrere [ `Span` ](xref:Xamarin.Forms.Span) Instanzen festlegen, über die [ `Spans` ](xref:Xamarin.Forms.FormattedString.Spans) Eigenschaft . Jede `Span` besitzt die folgenden Eigenschaften:

- [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor) – die Hintergrundfarbe des Span-Element.
- [`Font`](xref:Xamarin.Forms.Span.Font) – die Schriftart für den Text in der Spanne.
- [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes) – die Schriftartattribute für den Text in der Spanne.
- [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily) – die Schriftfamilie, zu der die Schriftart für den Text in der Spanne gehört.
- [`FontSize`](xref:Xamarin.Forms.Span.FontSize) – die Größe der Schriftart für den Text in der Spanne.
- [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor) – die Farbe für den Text in der Spanne. Diese Eigenschaft ist veraltet und wurde ersetzt durch die `TextColor` Eigenschaft.
- [`Style`](xref:Xamarin.Forms.Span.Style) – die Formatvorlage, die für die Spanne gelten.
- [`Text`](xref:Xamarin.Forms.Span.Text) – der Text der Spanne.
- [`TextColor`](xref:Xamarin.Forms.Span.TextColor) – die Farbe für den Text in der Spanne.

Das folgende XAML-Beispiel zeigt eine `FormattedText` -Eigenschaft, die besteht aus drei `Span` Instanzen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="TextSample.LabelPage"
             Title="Label Demo - XAML">
    <StackLayout Padding="5,10">
        ...
        <Label>
            <Label.FormattedText>
                <FormattedString>
                    <Span Text="Red Bold, " TextColor="Red" FontAttributes="Bold" />
                    <Span Text="default, " Style="{DynamicResource BodyStyle}" />
                    <Span Text="italic small." FontAttributes="Italic" FontSize="Small" />
                </FormattedString>
            </Label.FormattedText>
        </Label>
    </StackLayout>
</ContentPage>
```

Der entsprechende C#-Code ist:

```csharp
public class LabelPageCode : ContentPage
{
    public LabelPageCode ()
    {
        var layout = new StackLayout{ Padding = new Thickness (5, 10) };
        ...
        var formattedString = new FormattedString ();
        formattedString.Spans.Add (new Span{ Text = "Red bold, ", ForegroundColor = Color.Red, FontAttributes = FontAttributes.Bold });
        formattedString.Spans.Add (new Span { Text = "default, ", Style = Device.Styles.BodyStyle });
        formattedString.Spans.Add (new Span { Text = "italic small.", FontAttributes = FontAttributes.Italic, FontSize =  Device.GetNamedSize(NamedSize.Small, typeof(Label)) });

        layout.Children.Add (new Label { FormattedText = formattedString });
        this.Content = layout;
    }
}
```

> [!NOTE]
> Die [ `Text` ](xref:Xamarin.Forms.Span.Text) Eigenschaft eine `Span` kann über die Datenbindung festgelegt werden. Weitere Informationen finden Sie unter [Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md).

Die folgenden Screenshots zeigen das Ergebnis der Einstellung der `FormattedString` Eigenschaft, um drei `Span` Instanzen:

![](label-images/formattedtext.png "Bezeichnung FormattedText-Beispiel")

## <a name="styling-a-label"></a>Formatieren einer Bezeichnung

In den vorherigen Abschnitten behandelt Einstellung [ `Label` ](xref:Xamarin.Forms.Label) Eigenschaften auf einer Basis pro Instanz. Allerdings können Sätze von Eigenschaften in einem Stil gruppiert werden, die mit einem oder mehreren Ansichten, konsistent angewendet wird. Dies kann die Lesbarkeit des Codes zu erhöhen und stellen Änderungen des Entwurfs einfacher zu implementieren. Weitere Informationen finden Sie unter [Stile](~/xamarin-forms/user-interface/text/styles.md).

## <a name="related-links"></a>Verwandte Links

- [Erstellen von mobilen Apps mit Xamarin.Forms Kapitel 3](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [Text (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Label-API](xref:Xamarin.Forms.Label)
