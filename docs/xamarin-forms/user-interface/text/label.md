---
title: Xamarin.Forms Label
description: In diesem Artikel wird erläutert, wie Sie die Xamarin.Forms Label-Klasse verwenden, um ein- und mehrzeiligen Text in Anwendungen anzuzeigen.
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/09/2020
ms.openlocfilehash: bed6b8a774ecb352427f16b78d10e50821f92701
ms.sourcegitcommit: 765b69ed451a0f48625ea597c3f39de95f3ae693
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2020
ms.locfileid: "80987594"
---
# <a name="xamarinforms-label"></a>Xamarin.Forms Label

[![Beispiel](~/media/shared/download.png) herunterladen Download des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Anzeigen von Text in Xamarin.Forms_

Die [`Label`](xref:Xamarin.Forms.Label) Ansicht wird zum Anzeigen von Text verwendet, sowohl ein- als auch mehrseitig. Beschriftungen können Textdekorationen, farbigen Text enthalten und benutzerdefinierte Schriftarten (Familien, Größen und Optionen) verwenden.

## <a name="text-decorations"></a>Textdekorationen

Unterstreichen und Durchstreichen von Textdekorationen [`Label`](xref:Xamarin.Forms.Label) können auf `Label.TextDecorations` Instanzen angewendet `TextDecorations` werden, indem die Eigenschaft auf ein oder mehrere Enumerationsmember gesetzt wird:

- `None`
- `Underline`
- `Strikethrough`

Im folgenden XAML-Beispiel `Label.TextDecorations` wird das Festlegen der Eigenschaft veranschaulicht:

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

Die folgenden Screenshots `TextDecorations` zeigen die Enumerationsmember, die auf [`Label`](xref:Xamarin.Forms.Label) Instanzen angewendet werden:

![Etiketten mit Textdekorationen](label-images/label-textdecorations.png)

> [!NOTE]
> Textdekorationen können auch [`Span`](xref:Xamarin.Forms.Span) auf Instanzen angewendet werden. Weitere Informationen zur `Span` Klasse finden Sie unter [Formatierter Text](#Formatted_Text).

## <a name="character-spacing"></a>Zeichenabstand

Der Zeichenabstand kann [`Label`](xref:Xamarin.Forms.Label) auf Instanzen angewendet werden, indem die `Label.CharacterSpacing` Eigenschaft auf einen `double` Wert gesetzt wird:

```xaml
<Label Text="Character spaced text"
       CharacterSpacing="10" />
```

Der entsprechende C#-Code lautet:

```csharp
Label label = new Label { Text = "Character spaced text", CharacterSpacing = 10 };
```

Das Ergebnis ist, dass Zeichen [`Label`](xref:Xamarin.Forms.Label) im Text, der von den angezeigt wird, `CharacterSpacing` geräteunabhängige Einheiten voneinander entfernt sind.

## <a name="new-lines"></a>Zeilenumbrüche

Es gibt zwei Haupttechniken zum [`Label`](xref:Xamarin.Forms.Label) Erzwingen von Text in einer neuen Zeile aus XAML:

1. Verwenden Sie das Unicode-Zeilenvorschubzeichen , das "&#10;" ist.
1. Geben Sie Ihren Text mithilfe der *Eigenschaftenelementsyntax* an.

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

Text kann in c- in eine neue Zeile mit dem Zeichen "n" gezwungen werden:

```csharp
Label label = new Label { Text = "First line\nSecond line" };
```

## <a name="colors"></a>Farben

Beschriftungen können so eingestellt werden, dass [`TextColor`](xref:Xamarin.Forms.Label.TextColor) eine benutzerdefinierte Textfarbe über die bindbare Eigenschaft verwendet wird.

Besondere Sorgfalt ist notwendig, um sicherzustellen, dass Farben auf jeder Plattform verwendet werden. Da jede Plattform unterschiedliche Standardwerte für Text- und Hintergrundfarben hat, müssen Sie darauf achten, eine Standardeinstellung auszuwählen, die auf jeder Plattform funktioniert.

Im folgenden XAML-Beispiel wird `Label`die Textfarbe eines festgelegt:

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

Die folgenden Screenshots zeigen das `TextColor` Ergebnis der Einstellung der Eigenschaft:

![Label TextColor-Beispiel](label-images/textcolor.png)

Weitere Informationen zu Farben finden Sie unter [Farben](~/xamarin-forms/user-interface/colors.md).

## <a name="fonts"></a>Schriftarten

Weitere Informationen zum Angeben von `Label`Schriftarten auf einem finden Sie unter [Schriftarten](~/xamarin-forms/user-interface/text/fonts.md).

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>Abschneiden und Umwickeln

Beschriftungen können so eingestellt werden, dass sie Text verarbeiten, der nicht `LineBreakMode` auf eine Zeile auf eine von mehreren Arten passt, die von der Eigenschaft verfügbar gemacht werden. [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode)ist eine Enumeration mit den folgenden Werten:

- **HeadTruncation** &ndash; kürt den Kopf des Textes und zeigt das Ende an.
- **CharacterWrap** &ndash; umschließt Text in eine neue Zeile an einer Zeichengrenze.
- **MiddleTruncation** &ndash; zeigt den Anfang und das Ende des Textes an, wobei die Mitte durch eine Auslassung ersetzt wird.
- **NoWrap** &ndash; umschließt keinen Text und zeigt nur so viel Text an, wie in eine Zeile passen kann.
- **TailTruncation** &ndash; zeigt den Anfang des Textes an und wird am Ende abgeschnitten.
- **WordWrap** &ndash; umschließt Text an der Wortgrenze.

## <a name="display-a-specific-number-of-lines"></a>Anzeigen einer bestimmten Anzahl von Zeilen

Die Anzahl der von [`Label`](xref:Xamarin.Forms.Label) a angezeigten `Label.MaxLines` Zeilen kann `int` angegeben werden, indem die Eigenschaft auf einen Wert festgelegt wird:

- Wenn `MaxLines` -1 ist, was der `Label` Standardwert ist, [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) respektiert der den Wert der Eigenschaft, um entweder nur eine Zeile anzuzeigen, möglicherweise abgeschnitten, oder alle Zeilen mit dem gesamten Text.
- Wenn `MaxLines` es sich `Label` um 0 handelt, wird der nicht angezeigt.
- Wenn `MaxLines` 1 ist, ist das [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) Ergebnis [`NoWrap`](xref:Xamarin.Forms.LineBreakMode)identisch [`HeadTruncation`](xref:Xamarin.Forms.LineBreakMode) [`MiddleTruncation`](xref:Xamarin.Forms.LineBreakMode)mit [`TailTruncation`](xref:Xamarin.Forms.LineBreakMode)dem Festlegen der Eigenschaft auf , , , oder . Der `Label` wird jedoch den Wert [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) der Immobilie in Bezug auf die Platzierung einer Ellipse respektieren, falls zutreffend.
- Wenn `MaxLines` größer als 1 `Label` ist, wird der bis zur angegebenen Anzahl von [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) Zeilen angezeigt, wobei der Wert der Eigenschaft in Bezug auf die Platzierung einer Auslassungspunkte berücksichtigt wird, falls zutreffend. Das Festlegen `MaxLines` der Eigenschaft auf einen Wert größer [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) als 1 [`NoWrap`](xref:Xamarin.Forms.LineBreakMode)hat jedoch keine Auswirkungen, wenn die Eigenschaft auf festgelegt ist.

Im folgenden XAML-Beispiel `MaxLines` wird [`Label`](xref:Xamarin.Forms.Label)das Festlegen der Eigenschaft auf einem veranschaulicht:

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

Die folgenden Screenshots zeigen das `MaxLines` Ergebnis der Einstellung der Eigenschaft auf 2, wenn der Text lang genug ist, um mehr als 2 Zeilen zu belegen:

![Label MaxLines Beispiel](label-images/label-maxlines.png)

## <a name="display-html"></a>Anzeigen von HTML

Die [`Label`](xref:Xamarin.Forms.Label) Klasse `TextType` verfügt über eine `Label` Eigenschaft, die bestimmt, ob die Instanz Nur-Text oder HTML-Text anzeigen soll. Diese Eigenschaft sollte auf eines der `TextType` Member der Enumeration festgelegt werden:

- `Text`gibt an, dass der `Label` Nur-Text angezeigt `Label.TextType` wird und der Standardwert der Eigenschaft ist.
- `Html`gibt an, dass der `Label` HTML-Text anzeigt.

Daher [`Label`](xref:Xamarin.Forms.Label) können Instanzen HTML `Label.TextType` anzeigen, `Html`indem `Label.Text` sie die Eigenschaft auf , und die Eigenschaft auf eine HTML-Zeichenfolge festlegen:

```csharp
Label label = new Label
{
    Text = "This is <strong style=\"color:red\">HTML</strong> text.",
    TextType = TextType.Html
};
```

Im obigen Beispiel müssen die doppelten Anführungszeichen im `\` HTML-Code mithilfe des Symbols escapeieren.

In XAML können HTML-Zeichenfolgen unlesbar werden, `<` da `>` die und Die Symbole zusätzlich entweichen:

```xaml
<Label Text="This is &lt;strong style=&quot;color:red&quot;&gt;HTML&lt;/strong&gt; text."
       TextType="Html"  />
```

Alternativ kann der HTML-Code für eine bessere `CDATA` Lesbarkeit in einem Abschnitt einstrichen:

```xaml
<Label TextType="Html">
    <![CDATA[
    This is <strong style="color:red">HTML</strong> text.
    ]]>
</Label>
```

In diesem Beispiel `Label.Text` wird die Eigenschaft auf die HTML-Zeichenfolge `CDATA` festgelegt, die im Abschnitt einstrichen wird. Dies funktioniert, da die `Text` Eigenschaft die `ContentProperty` für die `Label` Klasse ist.

Die folgenden Screenshots [`Label`](xref:Xamarin.Forms.Label) zeigen ein html anzeigendes:

![Screenshots eines Labels mit HTML, iOS und Android](label-images/label-html.png)

> [!IMPORTANT]
> Das Anzeigen [`Label`](xref:Xamarin.Forms.Label) von HTML in a ist auf die HTML-Tags beschränkt, die von der zugrunde liegenden Plattform unterstützt werden.

<a name="Formatted_Text" />

## <a name="formatted-text"></a>Formatierter Text

Beschriftungen [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) machen eine Eigenschaft verfügbar, die die Darstellung von Text mit mehreren Schriftarten und Farben in derselben Ansicht ermöglicht.

Die `FormattedText` Eigenschaft ist [`FormattedString`](xref:Xamarin.Forms.FormattedString)vom Typ , [`Span`](xref:Xamarin.Forms.Span) der eine [`Spans`](xref:Xamarin.Forms.FormattedString.Spans) oder mehrere Instanzen umfasst, die über die Eigenschaft festgelegt werden. Die `Span` folgenden Eigenschaften können verwendet werden, um die visuelle Darstellung festzulegen:

- [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor)– die Farbe des Spannweite hintergrund.
- `CharacterSpacing` vom Typ `double`: Abstand zwischen den Zeichen des `Span`-Texts.
- [`Font`](xref:Xamarin.Forms.Span.Font)– die Schriftart für den Text in der Spanne.
- [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes)– die Schriftartattribute für den Text in der Spanne.
- [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily)– die Schriftfamilie, zu der die Schriftart für den Text in der Spanne gehört.
- [`FontSize`](xref:Xamarin.Forms.Span.FontSize)– die Größe der Schriftart für den Text in der Spanne.
- [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor)– die Farbe für den Text in der Spanne. Diese Eigenschaft ist veraltet und `TextColor` wurde durch die Eigenschaft ersetzt.
- [`LineHeight`](xref:Xamarin.Forms.Span.LineHeight)- der Multiplikator, der auf die Standardlinienhöhe der Spanne angewendet werden soll. Weitere Informationen finden Sie unter [Linienhöhe](#line-height).
- [`Style`](xref:Xamarin.Forms.Span.Style)– der Stil, der auf die Spanne angewendet werden soll.
- [`Text`](xref:Xamarin.Forms.Span.Text)– den Text der Spanne.
- [`TextColor`](xref:Xamarin.Forms.Span.TextColor)– die Farbe für den Text in der Spanne.
- `TextDecorations`- die Dekorationen, die auf den Text in der Spanne angewendet werden sollen. Weitere Informationen finden Sie unter [Textdekorationen](#text-decorations).

Die [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor) [`Text`](xref:Xamarin.Forms.Span.Text)Eigenschaften [`Text`](xref:Xamarin.Forms.Span.Text) , und bindbar haben [`OneWay`](xref:Xamarin.Forms.BindingMode)den Standardbindungsmodus von . Weitere Informationen zu diesem Bindungsmodus finden Sie im [Standardbindungsmodus](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md#the-default-binding-mode) im [Handbuch Bindungsmodus.](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md)

Darüber hinaus [`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers) kann die Eigenschaft verwendet werden, um eine Auflistung von Gestenerkennungsern zu definieren, die auf Gesten auf der [`Span`](xref:Xamarin.Forms.Span)reagieren.

> [!NOTE]
> Es ist nicht möglich, HTML [`Span`](xref:Xamarin.Forms.Span)in einem anzuzeigen.

Im folgenden XAML-Beispiel wird `FormattedText` eine [`Span`](xref:Xamarin.Forms.Span) Eigenschaft veranschaulicht, die aus drei Instanzen besteht:

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
> Die [`Text`](xref:Xamarin.Forms.Span.Text) Eigenschaft `Span` von a kann durch Datenbindung festgelegt werden. Weitere Informationen finden Sie unter [Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md).

Beachten Sie, dass a [`Span`](xref:Xamarin.Forms.Span) auch auf alle Gesten reagieren [`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers) kann, die der Sammlung der Spanne hinzugefügt werden. Beispielsweise wurde [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) dem zweiten `Span` in den obigen Codebeispielen ein hinzugefügt. Daher, wenn `Span` dies `TapGestureRecognizer` angetippt wird, `ICommand` wird [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command) die Antwort durch Ausführen der durch die Eigenschaft definierten ausführen. Weitere Informationen zu Gestenerkennungfinden finden Sie unter [Xamarin.Forms Gestures](~/xamarin-forms/app-fundamentals/gestures/index.md).

Die folgenden Screenshots zeigen das `FormattedString` Ergebnis `Span` der Einstellung der Eigenschaft auf drei Instanzen:

![Label FormattedText-Beispiel](label-images/formattedtext.png)

## <a name="line-height"></a>Zeilenhöhe

Die vertikale Höhe [`Label`](xref:Xamarin.Forms.Label) von [`Span`](xref:Xamarin.Forms.Span) a und a [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) kann [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) angepasst `double` werden, indem die Eigenschaft oder ein Wert gesetzt wird. Unter iOS und Android sind diese Werte Multiplikatoren der ursprünglichen Zeilenhöhe, und `Label.LineHeight` auf der universellen Windows-Plattform (UWP) ist der Eigenschaftswert ein Multiplikator der Schriftgröße der Bezeichnung.

> [!NOTE]
>
> - Unter iOS [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) ändern [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) die und Eigenschaften die Zeilenhöhe von Text, der in eine einzelne Zeile passt, und Text, der in mehrere Zeilen umbrochen wird.
> - Unter Android [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) ändern [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) die und-Eigenschaften nur die Zeilenhöhe von Text, der in mehrere Zeilen umbrochen wird.
> - In UWP [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) ändert die Eigenschaft die Zeilenhöhe von Text, der [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) in mehrere Zeilen umbrochen wird, und die Eigenschaft hat keine Auswirkungen.

Im folgenden XAML-Beispiel [`LineHeight`](xref:Xamarin.Forms.Label.LineHeight) wird [`Label`](xref:Xamarin.Forms.Label)das Festlegen der Eigenschaft auf einem veranschaulicht:

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

Die folgenden Screenshots zeigen das [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) Ergebnis der Einstellung der Eigenschaft auf 1.8:

![Label LineHeight-Beispiel](label-images/label-lineheight.png)

Im folgenden XAML-Beispiel [`LineHeight`](xref:Xamarin.Forms.Span.LineHeight) wird [`Span`](xref:Xamarin.Forms.Span)das Festlegen der Eigenschaft auf einem veranschaulicht:

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

Die folgenden Screenshots zeigen das [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) Ergebnis der Einstellung der Eigenschaft auf 1.8:

![Span LineHeight Beispiel](label-images/span-lineheight.png)

## <a name="padding"></a>Auffüllen

Padding stellt den Abstand zwischen einem Element und seinen untergeordneten Elementen dar und wird verwendet, um das Element von seinem eigenen Inhalt zu trennen. Padding kann auf [`Label`](xref:Xamarin.Forms.Label) Instanzen `Label.Padding` angewendet werden, indem die Eigenschaft auf einen [`Thickness`](xref:Xamarin.Forms.Thickness) Wert gesetzt wird:

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
> Wenn unter iOS [`Label`](xref:Xamarin.Forms.Label) ein erstellt `Padding` wird, das die Eigenschaft festlegt, wird die Auffüllung angewendet, und der Auffüllungswert kann später aktualisiert werden. Wenn jedoch `Label` eine erstellt wird, die `Padding` die Eigenschaft nicht festgelegt, hat der Versuch, sie später festzulegen, keine Auswirkungen.
>
> Auf Android und der universellen Windows-Plattform kann der `Padding` Eigenschaftswert beim Erstellen oder später `Label` angegeben werden.

Weitere Informationen zum Auffüllen finden Sie unter [Ränder und Auffüllung](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).

## <a name="hyperlinks"></a>Links

Der von [`Label`](xref:Xamarin.Forms.Label) und [`Span`](xref:Xamarin.Forms.Span) von Instanzen angezeigte Text kann mit folgendem Ansatz in Hyperlinks umgewandelt werden:

1. Legen `TextColor` Sie `TextDecoration` die [`Label`](xref:Xamarin.Forms.Label) und [`Span`](xref:Xamarin.Forms.Span)Eigenschaften der oder fest.
1. Fügen [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) Sie [`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers) der Auflistung [`Label`](xref:Xamarin.Forms.Label) [`Span`](xref:Xamarin.Forms.Span)der [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command) or eine hinzu, deren Eigenschaft an eine `ICommand`gebunden ist und deren [`CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) Eigenschaft die zu öffnende URL enthält.
1. Definieren `ICommand` Sie die, die [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)von der ausgeführt wird.
1. Schreiben Sie den Code, der `ICommand`von der ausgeführt wird.

Das folgende Codebeispiel aus dem [Beispiel Hyperlink-Demos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks/) zeigt eine, [`Label`](xref:Xamarin.Forms.Label) deren Inhalt aus mehreren [`Span`](xref:Xamarin.Forms.Span) Instanzen festgelegt ist:

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

In diesem Beispiel bestehen [`Span`](xref:Xamarin.Forms.Span) die erste und dritte `Span` Instanz aus Text, während die zweite einen tappbaren Hyperlink darstellt. Es hat seine Textfarbe auf blau gesetzt, und hat eine Unterstreichen Textdekoration. Dadurch wird die Darstellung eines Hyperlinks erstellt, wie in den folgenden Screenshots gezeigt:

[![Links](label-images/hyperlinks-small.png "Links")](label-images/hyperlinks-large.png#lightbox)

Wenn der Hyperlink angetippt wird, antwortet der, [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) indem er die `ICommand` durch seine [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command) Eigenschaft definierte ausführt. Darüber hinaus wird die [`CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) von der Eigenschaft `ICommand` angegebene URL als Parameter an den übergeben.

Der CodeBehind für die XAML-Seite enthält die `TapCommand` Implementierung:

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

Der `TapCommand` führt `Launcher.OpenAsync` die Methode [`TapGestureRecognizer.CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) aus und übergibt den Eigenschaftswert als Parameter. Die `Launcher.OpenAsync` Methode wird von Xamarin.Essentials bereitgestellt und öffnet die URL in einem Webbrowser. Daher besteht der Gesamteffekt darin, dass beim Abhören des Hyperlinks auf der Seite ein Webbrowser angezeigt wird und die dem Hyperlink zugeordnete URL navigiert wird.

### <a name="creating-a-reusable-hyperlink-class"></a>Erstellen einer wiederverwendbaren Hyperlinkklasse

Der vorherige Ansatz zum Erstellen eines Hyperlinks erfordert das Schreiben von wiederholtem Code jedes Mal, wenn Sie einen Hyperlink in Ihrer Anwendung benötigen. Sowohl die [`Label`](xref:Xamarin.Forms.Label) als [`Span`](xref:Xamarin.Forms.Span) auch die Klassen können `HyperlinkLabel` `HyperlinkSpan` jedoch zum Erstellen und Klassen unterklassifiziert werden, wobei der Gestenerkennungs- und Textformatierungscode dort hinzugefügt wird.

Das folgende Codebeispiel aus dem [Beispiel Hyperlink-Demos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks/) zeigt eine `HyperlinkSpan` Klasse:

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

Die `HyperlinkSpan` Klasse definiert `Url` eine Eigenschaft [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)und zugeordnet , und der [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) Konstruktor legt die Hyperlinkdarstellung und die, die beim Abhören des Hyperlinks reagiert. Wenn `HyperlinkSpan` ein getippt `TapGestureRecognizer` wird, reagiert `Launcher.OpenAsync` der, indem die Methode `Url` zum Öffnen der von der Eigenschaft angegebenen URL in einem Webbrowser ausgeführt wird.

Die `HyperlinkSpan` Klasse kann durch Hinzufügen einer Instanz der Klasse zum XAML verbraucht werden:

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

## <a name="styling-labels"></a>Styling-Etiketten

In den vorherigen [`Label`](xref:Xamarin.Forms.Label) Abschnitten wurden Einstellungen und [`Span`](xref:Xamarin.Forms.Span) Eigenschaften pro Instanz behandelt. Eigenschaftensätze können jedoch in einem Stil gruppiert werden, der konsistent auf eine oder mehrere Ansichten angewendet wird. Dies kann die Lesbarkeit von Code erhöhen und die Implementierung von Entwurfsänderungen vereinfachen. Weitere Informationen finden Sie unter [Stile](~/xamarin-forms/user-interface/text/styles.md).

## <a name="related-links"></a>Verwandte Links

- [Text (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [Hyperlinks (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks)
- [Erstellen mobiler Apps mit Xamarin.Forms, Kapitel 3](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [Label-API](xref:Xamarin.Forms.Label)
- [Span-API](xref:Xamarin.Forms.Span)
