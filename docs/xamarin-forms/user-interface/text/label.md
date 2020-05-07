---
title: Xamarin. Forms-Bezeichnung
description: In diesem Artikel wird erläutert, wie Sie mithilfe der Bezeichnung "xamarin. Forms" einen einzelnen und mehrzeiligen Text in Anwendungen anzeigen.
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/09/2020
ms.openlocfilehash: 6ce4e39986afb7444e7d126e9789760d9311ca45
ms.sourcegitcommit: 737c7fbbe8aed1f33110a5217d7e6d6ef3e5b785
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "82793221"
---
# <a name="xamarinforms-label"></a>Xamarin. Forms-Bezeichnung

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Anzeigen von Text in xamarin. Forms_

Die [`Label`](xref:Xamarin.Forms.Label) Sicht wird zum Anzeigen von Text verwendet, sowohl für einzelne als auch für mehrere Zeilen. Bezeichnungen können Text Dekorationen und farbige Text enthalten und benutzerdefinierte Schriftarten (Familien, Größen und Optionen) verwenden.

## <a name="text-decorations"></a>Text Dekorationen

Text Dekorationen unterstrichen und durchgestrichen können auf [`Label`](xref:Xamarin.Forms.Label) -Instanzen angewendet werden, indem `Label.TextDecorations` die-Eigenschaft auf ein `TextDecorations` oder mehrere Enumerationsmember festgelegt wird:

- `None`
- `Underline`
- `Strikethrough`

Im folgenden XAML-Beispiel wird das `Label.TextDecorations` Festlegen der-Eigenschaft veranschaulicht:

```xaml
<Label Text="This is underlined text." TextDecorations="Underline"  />
<Label Text="This is text with strikethrough." TextDecorations="Strikethrough" />
<Label Text="This is underlined text with strikethrough." TextDecorations="Underline, Strikethrough" />
```

Der entsprechende C#-Code lautet:

```csharp
var underlineLabel = new Label { Text = "This is underlined text.", TextDecorations = TextDecorations.Underline };
var strikethroughLabel = new Label { Text = "This is text with strikethrough.", TextDecorations = TextDecorations.Strikethrough };
var bothLabel = new Label { Text = "This is underlined text with strikethrough.", TextDecorations = TextDecorations.Underline | TextDecorations.Strikethrough };
```

Die folgenden Screenshots zeigen die `TextDecorations` auf [`Label`](xref:Xamarin.Forms.Label) -Instanzen angewendeten Enumerationsmember:

![Bezeichnungen mit Text Dekorationen](label-images/label-textdecorations.png)

> [!NOTE]
> Text Dekorationen können auch auf [`Span`](xref:Xamarin.Forms.Span) -Instanzen angewendet werden. Weitere Informationen zur- `Span` Klasse finden Sie unter [formatierten Text](#Formatted_Text).

## <a name="character-spacing"></a>Zeichenabstand

Der Zeichenabstand kann auf [`Label`](xref:Xamarin.Forms.Label) -Instanzen angewendet werden, `Label.CharacterSpacing` indem die- `double` Eigenschaft auf einen Wert festgelegt wird:

```xaml
<Label Text="Character spaced text"
       CharacterSpacing="10" />
```

Der entsprechende C#-Code lautet:

```csharp
Label label = new Label { Text = "Character spaced text", CharacterSpacing = 10 };
```

Das Ergebnis ist, dass Zeichen im Text, die von [`Label`](xref:Xamarin.Forms.Label) angezeigt werden, `CharacterSpacing` geräteunabhängige Einheiten voneinander getrennt sind.

## <a name="new-lines"></a>Zeilenumbrüche

Es gibt zwei Haupttechniken zum Erzwingen von Text in [`Label`](xref:Xamarin.Forms.Label) einem in einer neuen Zeile, von XAML:

1. Verwenden Sie das Unicode-Zeilenvorschub Zeichen "&amp;#10;".
1. Geben Sie den Text mithilfe der *Eigenschafts Element* Syntax an.

Der folgende Code zeigt ein Beispiel für beide Techniken:

```xaml
<!-- Unicode line feed character -->
<Label Text="First line &#10; Second line" />

<!-- Property element syntax -->
<Label>
    <Label.Text>
        First line
        Second line
    </Label.Text>
</Label>
```

In c# kann Text in eine neue Zeile mit dem Zeichen "\n" erzwungen werden:

```csharp
Label label = new Label { Text = "First line\nSecond line" };
```

## <a name="colors"></a>Farben

Bezeichnungen können so festgelegt werden, dass eine benutzerdefinierte Textfarbe über [`TextColor`](xref:Xamarin.Forms.Label.TextColor) die bindbare Eigenschaft verwendet wird.

Eine besondere Sorgfalt ist erforderlich, um sicherzustellen, dass Farben auf jeder Plattform verwendet werden können. Da jede Plattform über andere Standardwerte für Text-und Hintergrundfarben verfügt, müssen Sie darauf achten, einen Standardwert auszuwählen, der für jede Plattform geeignet ist.

Im folgenden XAML-Beispiel wird die Textfarbe eines `Label`festgelegt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="TextSample.LabelPage"
             Title="Label Demo">
    <StackLayout Padding="5,10">
      <Label TextColor="#77d065" FontSize = "20" Text="This is a green label." />
    </StackLayout>
</ContentPage>
```

Der entsprechende C#-Code lautet:

```csharp
public partial class LabelPage : ContentPage
{
    public LabelPage ()
    {
        InitializeComponent ();

        var layout = new StackLayout { Padding = new Thickness(5,10) };
        var label = new Label { Text="This is a green label.", TextColor = Color.FromHex("#77d065"), FontSize = 20 };
        layout.Children.Add(label);
        this.Content = layout;
    }
}
```

Die folgenden Screenshots zeigen das Ergebnis der Einstellung der `TextColor` -Eigenschaft:

![TextColor-Beispiel für Bezeichnung](label-images/textcolor.png)

Weitere Informationen zu Farben finden Sie unter [Farben](~/xamarin-forms/user-interface/colors.md).

## <a name="fonts"></a>Schriftarten

Weitere Informationen zum Angeben von Schriftarten auf `Label`einem finden Sie unter [Schriftarten](~/xamarin-forms/user-interface/text/fonts.md).

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>Abschneiden und umwickeln

Bezeichnungen können so festgelegt werden, dass Sie Text verarbeiten können, der auf eine der folgenden Arten `LineBreakMode` nicht in einer Zeile passen kann. [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode)ist eine Enumeration mit den folgenden Werten:

- Die **kopftrunzierung** &ndash; verkürzt den Anfang des Texts und zeigt das Ende an.
- Der Zeichen Umbruch umschließt Text in eine neue Zeile an einer Zeichengrenze. **CharacterWrap** &ndash;
- Die **middletrunzierung** &ndash; zeigt den Anfang und das Ende des Texts an, wobei die mittlere durch ein Auslassungs Zeichen ersetzt wird.
- **Nowrap** &ndash; packt Text nicht, sodass nur so viel Text angezeigt wird, wie er in eine Zeile passen kann.
- **Tailtrunationzeigt** &ndash; den Anfang des Texts, wobei das Ende abgeschnitten wird.
- **WordWrap** &ndash; umschließt Text an der Wort Grenze.

## <a name="display-a-specific-number-of-lines"></a>Anzeigen einer bestimmten Anzahl von Zeilen

Die Anzahl der Zeilen, die von [`Label`](xref:Xamarin.Forms.Label) einer angezeigt werden, kann durch `Label.MaxLines` Festlegen der- `int` Eigenschaft auf einen Wert angegeben werden:

- Wenn `MaxLines` -1 ist, d. h. der Standardwert `Label` , respektiert den Wert der [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) -Eigenschaft, um entweder nur eine Zeile, möglicherweise abgeschnitten oder alle Zeilen mit dem gesamten Text anzuzeigen.
- Wenn `MaxLines` den Wert 0 hat `Label` , wird der nicht angezeigt.
- Wenn `MaxLines` den Wert 1 hat, ist das Ergebnis mit dem [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) Festlegen der [`NoWrap`](xref:Xamarin.Forms.LineBreakMode)- [`HeadTruncation`](xref:Xamarin.Forms.LineBreakMode)Eigenschaft [`MiddleTruncation`](xref:Xamarin.Forms.LineBreakMode)auf, [`TailTruncation`](xref:Xamarin.Forms.LineBreakMode), oder identisch. Allerdings berücksichtigt `Label` der den Wert der- [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) Eigenschaft hinsichtlich der Platzierung eines Auslassungs Zeichen, falls zutreffend.
- Wenn `MaxLines` größer als 1 ist, wird `Label` das bis zur angegebenen Anzahl von Zeilen angezeigt. dabei wird der Wert der [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) Eigenschaft in Bezug auf die Platzierung der Auslassungs Punkte berücksichtigt, falls zutreffend. Das Festlegen der `MaxLines` -Eigenschaft auf einen Wert größer als 1 hat jedoch keine Auswirkung, [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) wenn die-Eigenschaft [`NoWrap`](xref:Xamarin.Forms.LineBreakMode)auf festgelegt ist.

Das folgende XAML-Beispiel veranschaulicht das `MaxLines` Festlegen der- [`Label`](xref:Xamarin.Forms.Label)Eigenschaft für einen:

```xaml
<Label Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus."
       LineBreakMode="WordWrap"
       MaxLines="2" />
```

Der entsprechende C#-Code lautet:

```csharp
var label =
{
  Text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus.", LineBreakMode = LineBreakMode.WordWrap,
  MaxLines = 2
};
```

Die folgenden Screenshots zeigen das Ergebnis der Einstellung der `MaxLines` -Eigenschaft auf 2, wenn der Text lang genug ist, um mehr als 2 Zeilen zu belegen:

![Beispiel für eine Bezeichnung MaxLines](label-images/label-maxlines.png)

## <a name="display-html"></a>Anzeigen von HTML

Die [`Label`](xref:Xamarin.Forms.Label) -Klasse verfügt `TextType` über eine-Eigenschaft, die `Label` bestimmt, ob die-Instanz nur-Text oder HTML-Text anzeigen soll. Diese Eigenschaft sollte auf einen der Member der- `TextType` Enumeration festgelegt werden:

- `Text`Gibt an, `Label` dass der nur-Text anzeigt und der Standardwert der `Label.TextType` Eigenschaft ist.
- `Html`Gibt an, `Label` dass der HTML-Text anzeigt.

Daher können [`Label`](xref:Xamarin.Forms.Label) -Instanzen HTML anzeigen, indem die `Label.TextType` -Eigenschaft `Html`auf und die `Label.Text` -Eigenschaft auf eine HTML-Zeichenfolge festgelegt wird:

```csharp
Label label = new Label
{
    Text = "This is <strong style=\"color:red\">HTML</strong> text.",
    TextType = TextType.Html
};
```

Im obigen Beispiel müssen die doppelten Anführungszeichen in HTML mithilfe des `\` Symbols mit Escapezeichen versehen werden.

In XAML können HTML-Zeichen folgen aufgrund der zusätzlichen Escapezeichen der `<` Symbole `>` und nicht lesbar werden:

```xaml
<Label Text="This is &lt;strong style=&quot;color:red&quot;&gt;HTML&lt;/strong&gt; text."
       TextType="Html"  />
```

Zur besseren Lesbarkeit kann der HTML-Code in einem `CDATA` Abschnitt Inline sein:

```xaml
<Label TextType="Html">
    <![CDATA[
    This is <strong style="color:red">HTML</strong> text.
    ]]>
</Label>
```

In diesem Beispiel wird die `Label.Text` -Eigenschaft auf die HTML-Zeichenfolge festgelegt, die im `CDATA` -Abschnitt Inline ist. Dies funktioniert, da `Text` die-Eigenschaft `ContentProperty` für die `Label` -Klasse ist.

Die folgenden Screenshots zeigen, [`Label`](xref:Xamarin.Forms.Label) wie HTML angezeigt wird:

![Screenshots einer Bezeichnung, die HTML unter IOS und Android anzeigt](label-images/label-html.png)

> [!IMPORTANT]
> Das Anzeigen von HTML [`Label`](xref:Xamarin.Forms.Label) in einer ist auf die HTML-Tags beschränkt, die von der zugrunde liegenden Plattform unterstützt werden.

<a name="Formatted_Text" />

## <a name="formatted-text"></a>Formatierter Text

Bezeichnungen machen eine [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) Eigenschaft verfügbar, die die Darstellung von Text mit mehreren Schriftarten und Farben in derselben Ansicht ermöglicht.

Die `FormattedText` -Eigenschaft ist vom [`FormattedString`](xref:Xamarin.Forms.FormattedString)Typ, der eine oder mehrere [`Span`](xref:Xamarin.Forms.Span) -Instanzen enthält, die [`Spans`](xref:Xamarin.Forms.FormattedString.Spans) über die-Eigenschaft festgelegt werden. Die folgenden `Span` Eigenschaften können verwendet werden, um die visuelle Darstellung festzulegen:

- [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor)– die Farbe des spannen Hintergrunds.
- `CharacterSpacing` vom Typ `double`: Abstand zwischen den Zeichen des `Span`-Texts.
- [`Font`](xref:Xamarin.Forms.Span.Font)– die Schriftart für den Text in der Spanne.
- [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes)– die Schriftart Attribute für den Text in der Spanne.
- [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily)– die Schriftfamilie, zu der die Schriftart für den Text in der Spanne gehört.
- [`FontSize`](xref:Xamarin.Forms.Span.FontSize)– die Größe der Schriftart für den Text in der Spanne.
- [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor)– die Farbe für den Text in der Spanne. Diese Eigenschaft ist veraltet und wurde durch die `TextColor` -Eigenschaft ersetzt.
- [`LineHeight`](xref:Xamarin.Forms.Span.LineHeight)-der Multiplikator, der auf die Standard Zeilenhöhe der Spanne angewendet werden soll. Weitere Informationen finden Sie unter [Zeilenhöhe](#line-height).
- [`Style`](xref:Xamarin.Forms.Span.Style)– der Stil, der auf die Spanne angewendet werden soll.
- [`Text`](xref:Xamarin.Forms.Span.Text)– der Text der Spanne.
- [`TextColor`](xref:Xamarin.Forms.Span.TextColor)– die Farbe für den Text in der Spanne.
- `TextDecorations`-die auf den Text in der Spanne anzuwendenden Dekorationen. Weitere Informationen finden Sie unter [Text Dekorationen](#text-decorations).

Die [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor)bindbaren [`Text`](xref:Xamarin.Forms.Span.Text)Eigenschaften [`Text`](xref:Xamarin.Forms.Span.Text) , und verfügen über einen Standard Bindungs Modus von [`OneWay`](xref:Xamarin.Forms.BindingMode). Weitere Informationen zu diesem Bindungs Modus finden Sie im [Bindungs Modus](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md) im Leitfaden zur [Standard Bindung](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md#the-default-binding-mode) .

Außerdem kann die [`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers) -Eigenschaft verwendet werden, um eine Auflistung von Gesten Erkennungs Programmen zu definieren, die auf Gesten auf dem [`Span`](xref:Xamarin.Forms.Span)reagieren.

> [!NOTE]
> Es ist nicht möglich, HTML in einem [`Span`](xref:Xamarin.Forms.Span)anzuzeigen.

Das folgende XAML-Beispiel veranschaulicht `FormattedText` eine Eigenschaft, die aus [`Span`](xref:Xamarin.Forms.Span) drei Instanzen besteht:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="TextSample.LabelPage"
             Title="Label Demo - XAML">
    <StackLayout Padding="5,10">
        ...
        <Label LineBreakMode="WordWrap">
            <Label.FormattedText>
                <FormattedString>
                    <Span Text="Red Bold, " TextColor="Red" FontAttributes="Bold" />
                    <Span Text="default, " Style="{DynamicResource BodyStyle}">
                        <Span.GestureRecognizers>
                            <TapGestureRecognizer Command="{Binding TapCommand}" />
                        </Span.GestureRecognizers>
                    </Span>
                    <Span Text="italic small." FontAttributes="Italic" FontSize="Small" />
                </FormattedString>
            </Label.FormattedText>
        </Label>
    </StackLayout>
</ContentPage>
```

Der entsprechende C#-Code lautet:

```csharp
public class LabelPageCode : ContentPage
{
    public LabelPageCode ()
    {
        var layout = new StackLayout{ Padding = new Thickness (5, 10) };
        ...
        var formattedString = new FormattedString ();
        formattedString.Spans.Add (new Span{ Text = "Red bold, ", ForegroundColor = Color.Red, FontAttributes = FontAttributes.Bold });

        var span = new Span { Text = "default, " };
        span.GestureRecognizers.Add(new TapGestureRecognizer { Command = new Command(async () => await DisplayAlert("Tapped", "This is a tapped Span.", "OK")) });
        formattedString.Spans.Add(span);
        formattedString.Spans.Add (new Span { Text = "italic small.", FontAttributes = FontAttributes.Italic, FontSize =  Device.GetNamedSize(NamedSize.Small, typeof(Label)) });

        layout.Children.Add (new Label { FormattedText = formattedString });
        this.Content = layout;
    }
}
```

> [!IMPORTANT]
> Die [`Text`](xref:Xamarin.Forms.Span.Text) -Eigenschaft eines `Span` kann durch Datenbindung festgelegt werden. Weitere Informationen finden Sie unter [Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md).

Beachten Sie, [`Span`](xref:Xamarin.Forms.Span) dass ein auch auf alle Gesten reagieren kann, die der Span- [`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers) Auflistung hinzugefügt werden. Beispielsweise wurde eine [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) der zweiten `Span` in den obigen Codebeispielen hinzugefügt. `Span` Wenn dieses-Objekt abgetippt wird `TapGestureRecognizer` , antwortet daher, indem `ICommand` das von der [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command) -Eigenschaft definierte ausgeführt wird. Weitere Informationen zu Gesten erkentungen finden Sie unter [xamarin. Forms-Gesten](~/xamarin-forms/app-fundamentals/gestures/index.md).

Die folgenden Screenshots zeigen das Ergebnis der Einstellung der `FormattedString` -Eigenschaft auf `Span` drei Instanzen:

![Bezeichnung FormattedText-Beispiel](label-images/formattedtext.png)

## <a name="line-height"></a>Zeilenhöhe

Die vertikale [`Label`](xref:Xamarin.Forms.Label) Höhe eines und eines [`Span`](xref:Xamarin.Forms.Span) kann angepasst werden, indem die [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) -Eigenschaft oder [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) auf einen `double` -Wert festgelegt wird. Unter IOS und Android sind diese Werte multipliziert mit der ursprünglichen Zeilenhöhe, und auf der universelle Windows-Plattform (UWP) ist der `Label.LineHeight` Eigenschafts Wert ein Multiplikator für die Schriftgröße der Bezeichnung.

> [!NOTE]
>
> - Unter IOS ändern die [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) - [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) Eigenschaft und die-Eigenschaft die Zeilenhöhe von Text, der in eine einzelne Zeile passt, sowie Text, der in mehrere Zeilen umbrochen wird.
> - Unter Android ändern die [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) - [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) Eigenschaft und die-Eigenschaft nur die Zeilenhöhe von Text, der in mehrere Zeilen umbrochen wird.
> - Bei UWP ändert die [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) -Eigenschaft die Zeilenhöhe von Text, der in mehrere Zeilen umbrochen wird [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) , und die-Eigenschaft hat keine Auswirkungen.

Das folgende XAML-Beispiel veranschaulicht das [`LineHeight`](xref:Xamarin.Forms.Label.LineHeight) Festlegen der- [`Label`](xref:Xamarin.Forms.Label)Eigenschaft für einen:

```xaml
<Label Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus."
       LineBreakMode="WordWrap"
       LineHeight="1.8" />
```

Der entsprechende C#-Code lautet:

```csharp
var label =
{
  Text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus.", LineBreakMode = LineBreakMode.WordWrap,
  LineHeight = 1.8
};
```

Die folgenden Screenshots zeigen das Ergebnis der Einstellung der [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) -Eigenschaft auf 1,8:

![Bezeichnung "LineHeight example"](label-images/label-lineheight.png)

Das folgende XAML-Beispiel veranschaulicht das [`LineHeight`](xref:Xamarin.Forms.Span.LineHeight) Festlegen der- [`Span`](xref:Xamarin.Forms.Span)Eigenschaft für einen:

```xaml
<Label LineBreakMode="WordWrap">
    <Label.FormattedText>
        <FormattedString>
            <Span Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit. In a tincidunt sem. Phasellus mollis sit amet turpis in rutrum. Sed aliquam ac urna id scelerisque. "
                  LineHeight="1.8"/>
            <Span Text="Nullam feugiat sodales elit, et maximus nibh vulputate id."
                  LineHeight="1.8" />
        </FormattedString>
    </Label.FormattedText>
</Label>
```

Der entsprechende C#-Code lautet:

```csharp
var formattedString = new FormattedString();
formattedString.Spans.Add(new Span
{
  Text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. In a tincidunt sem. Phasellus mollis sit amet turpis in rutrum. Sed aliquam ac urna id scelerisque. ",
  LineHeight = 1.8
});
formattedString.Spans.Add(new Span
{
  Text = "Nullam feugiat sodales elit, et maximus nibh vulputate id.",
  LineHeight = 1.8
});
var label = new Label
{
  FormattedText = formattedString,
  LineBreakMode = LineBreakMode.WordWrap
};
```

Die folgenden Screenshots zeigen das Ergebnis der Einstellung der [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) -Eigenschaft auf 1,8:

![Span LineHeight-Beispiel](label-images/span-lineheight.png)

## <a name="padding"></a>Auffüllen

Padding stellt den Leerraum zwischen einem Element und seinen untergeordneten Elementen dar und wird verwendet, um das Element vom eigenen Inhalt zu trennen. Auffüll Zeichen können auf [`Label`](xref:Xamarin.Forms.Label) -Instanzen angewendet werden, `Label.Padding` indem die- [`Thickness`](xref:Xamarin.Forms.Thickness) Eigenschaft auf einen Wert festgelegt wird:

```xaml
<Label Padding="10">
    <Label.FormattedText>
        <FormattedString>
            <Span Text="Lorem ipsum" />
            <Span Text="dolor sit amet." />
        </FormattedString>
    </Label.FormattedText>
</Label>
```

Der entsprechende C#-Code lautet:

```csharp
FormattedString formattedString = new FormattedString();
formattedString.Spans.Add(new Span
{
  Text = "Lorem ipsum"
});
formattedString.Spans.Add(new Span
{
  Text = "dolor sit amet."
});
Label label = new Label
{
    FormattedText = formattedString,
    Padding = new Thickness(20)
};
```

> [!IMPORTANT]
> Wenn unter IOS ein [`Label`](xref:Xamarin.Forms.Label) erstellt wird, mit dem die `Padding` -Eigenschaft festgelegt wird, wird Auffüllung angewendet, und der Auffüll Wert kann später aktualisiert werden. Wenn jedoch ein `Label` erstellt wird, von dem die- `Padding` Eigenschaft nicht festgelegt wird, hat der Versuch, ihn später festzulegen, keine Auswirkung.
>
> Unter Android und der universelle Windows-Plattform kann der `Padding` -Eigenschafts Wert beim `Label` Erstellen von oder später angegeben werden.

Weitere Informationen zum Auffüllen finden Sie unter [Ränder und Padding](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).

## <a name="hyperlinks"></a>Links

Der von [`Label`](xref:Xamarin.Forms.Label) der-Instanz [`Span`](xref:Xamarin.Forms.Span) und der-Instanz angezeigte Text kann mit dem folgenden Ansatz in Hyperlinks umgewandelt werden:

1. Legen Sie `TextColor` die `TextDecoration` Eigenschaften und von [`Label`](xref:Xamarin.Forms.Label) oder [`Span`](xref:Xamarin.Forms.Span)fest.
1. Fügen Sie [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) der- [`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers) Auflistung des- [`Label`](xref:Xamarin.Forms.Label) Objekts [`Span`](xref:Xamarin.Forms.Span)oder- [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command) Objekts, dessen- `ICommand`Eigenschaft an einen [`CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) gebunden ist und dessen-Eigenschaft die zu öffnende URL enthält, ein hinzu.
1. Definieren Sie `ICommand` das, das von ausgeführt wird [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer).
1. Schreiben Sie den Code, der von der `ICommand`ausgeführt wird.

Das folgende Codebeispiel stammt aus dem Beispiel für [Hyperlink-Demos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks/) und zeigt [`Label`](xref:Xamarin.Forms.Label) eine, deren Inhalt von mehreren [`Span`](xref:Xamarin.Forms.Span) -Instanzen festgelegt wird:

```xaml
<Label>
    <Label.FormattedText>
        <FormattedString>
            <Span Text="Alternatively, click " />
            <Span Text="here"
                  TextColor="Blue"
                  TextDecorations="Underline">
                <Span.GestureRecognizers>
                    <TapGestureRecognizer Command="{Binding TapCommand}"
                                          CommandParameter="https://docs.microsoft.com/xamarin/" />
                </Span.GestureRecognizers>
            </Span>
            <Span Text=" to view Xamarin documentation." />
        </FormattedString>
    </Label.FormattedText>
</Label>
```

In diesem Beispiel bilden die erste und die [`Span`](xref:Xamarin.Forms.Span) dritte Instanz Text, während der zweite `Span` einen anfügbaren Hyperlink darstellt. Die Textfarbe ist auf Blau festgelegt, und Sie verfügt über eine Unterstreichung der Text Dekoration. Dadurch wird ein Hyperlink erstellt, wie in den folgenden Screenshots gezeigt:

[![Links](label-images/hyperlinks-small.png "Links")](label-images/hyperlinks-large.png#lightbox)

Wenn der Hyperlink abgetippt wird, [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) antwortet der durch Ausführen der `ICommand` , die durch [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command) die-Eigenschaft definiert wird. Außerdem wird die von der [`CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) -Eigenschaft angegebene URL als Parameter an das `ICommand` -Objekt übergeben.

Die Code-Behind-Datei für die XAML- `TapCommand` Seite enthält die-Implementierung:

```csharp
public partial class MainPage : ContentPage
{
    // Launcher.OpenAsync is provided by Xamarin.Essentials.
    public ICommand TapCommand => new Command<string>(async (url) => await Launcher.OpenAsync(url));

    public MainPage()
    {
        InitializeComponent();
        BindingContext = this;
    }
}
```

`TapCommand` Führt die- `Launcher.OpenAsync` Methode aus und übergibt [`TapGestureRecognizer.CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) den-Eigenschafts Wert als Parameter. Die `Launcher.OpenAsync` -Methode wird von xamarin. Essentials bereitgestellt und öffnet die URL in einem Webbrowser. Der Gesamteffekt besteht daher darin, dass beim Tippen auf der Seite ein Webbrowser angezeigt wird und die URL, zu der der Hyperlink verknüpft ist, zu navigieren.

### <a name="creating-a-reusable-hyperlink-class"></a>Erstellen einer wiederverwendbaren Hyperlink-Klasse

Die vorherige Methode zum Erstellen eines Links erfordert das Schreiben von wiederholtem Code, wenn Sie in Ihrer Anwendung einen Hyperlink benötigen. Allerdings können die [`Label`](xref:Xamarin.Forms.Label) - [`Span`](xref:Xamarin.Forms.Span) Klasse und die-Klasse zum Erstellen `HyperlinkLabel` von `HyperlinkSpan` -und-Klassen unter klassifiziert werden, wobei die Gestenerkennung und der Text Formatierungs Code dort hinzugefügt werden.

Das folgende Codebeispiel stammt aus dem Beispiel für [Hyperlink-Demos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks/) und zeigt `HyperlinkSpan` eine-Klasse:

```csharp
public class HyperlinkSpan : Span
{
    public static readonly BindableProperty UrlProperty =
        BindableProperty.Create(nameof(Url), typeof(string), typeof(HyperlinkSpan), null);

    public string Url
    {
        get { return (string)GetValue(UrlProperty); }
        set { SetValue(UrlProperty, value); }
    }

    public HyperlinkSpan()
    {
        TextDecorations = TextDecorations.Underline;
        TextColor = Color.Blue;
        GestureRecognizers.Add(new TapGestureRecognizer
        {
            // Launcher.OpenAsync is provided by Xamarin.Essentials.
            Command = new Command(async () => await Launcher.OpenAsync(Url))
        });
    }
}
```

Die `HyperlinkSpan` -Klasse definiert `Url` eine-Eigenschaft und [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)eine zugeordnete-Klasse, und der-Konstruktor [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) legt die Hyperlink-Darstellung und die fest, die beim Tippen des Links reagieren wird. Wenn ein `HyperlinkSpan` -Objekt getippt `TapGestureRecognizer` wird, antwortet das, `Launcher.OpenAsync` indem die-Methode ausgeführt wird, um die `Url` von der-Eigenschaft angegebene URL in einem Webbrowser zu öffnen.

Die `HyperlinkSpan` -Klasse kann verwendet werden, indem Sie der XAML eine Instanz der-Klasse hinzufügen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:HyperlinkDemo"
             x:Class="HyperlinkDemo.MainPage">
    <StackLayout>
        ...
        <Label>
            <Label.FormattedText>
                <FormattedString>
                    <Span Text="Alternatively, click " />
                    <local:HyperlinkSpan Text="here"
                                         Url="https://docs.microsoft.com/appcenter/" />
                    <Span Text=" to view AppCenter documentation." />
                </FormattedString>
            </Label.FormattedText>
        </Label>
    </StackLayout>
</ContentPage>
```

## <a name="styling-labels"></a>Formatieren von Bezeichnungen

In den vorherigen Abschnitten wurden [`Label`](xref:Xamarin.Forms.Label) die [`Span`](xref:Xamarin.Forms.Span) Einstellungen und Eigenschaften pro Instanz behandelt. Allerdings können Sätze von Eigenschaften in einem Stil gruppiert werden, der konsistent auf eine oder mehrere Ansichten angewendet wird. Dies kann die Lesbarkeit von Code erhöhen und die Implementierung von Entwurfs Änderungen vereinfachen. Weitere Informationen finden Sie unter [Stile](~/xamarin-forms/user-interface/text/styles.md).

## <a name="related-links"></a>Verwandte Links

- [Text (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [Hyperlinks (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks)
- [Erstellen von Mobile Apps mit xamarin. Forms, Kapitel 3](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [Label-API](xref:Xamarin.Forms.Label)
- [Span-API](xref:Xamarin.Forms.Span)
