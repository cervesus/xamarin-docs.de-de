---
title: Xamarin.Forms-Bezeichnung
description: In diesem Artikel wird erläutert, wie Xamarin.Forms Label-Klasse zu verwenden, um einzelne und mehrzeiligen Text in Anwendungen anzuzeigen.
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/28/2019
ms.openlocfilehash: 64fa15a15468a84ada3a377a9ac85bbf6310099c
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75490037"
---
# <a name="xamarinforms-label"></a>Xamarin.Forms-Bezeichnung

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Anzeigen von Text in Xamarin.Forms_

Die [ `Label` ](xref:Xamarin.Forms.Label) Ansicht dient zum Anzeigen von Text, sowohl einzelne als auch mit mehreren Zeilen. Bezeichnungen können haben Textdekorationen, farbige Text ein, und verwenden benutzerdefinierte Schriftarten (Familien, Größen und Optionen).

## <a name="text-decorations"></a>Textverzierungen

Auf die Unterstreichung und Durchstreichung Textdekorationen angewendet werden können [ `Label` ](xref:Xamarin.Forms.Label) Instanzen durch Festlegen der `Label.TextDecorations` Eigenschaft, um eine oder mehrere `TextDecorations` Enumerationsmember:

- `None`
- `Underline`
- `Strikethrough`

Der folgende XAML-Beispiel veranschaulicht das Festlegen der `Label.TextDecorations` Eigenschaft:

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

Die folgenden Screenshots zeigen die `TextDecorations` Enumerationsmember angewendet [ `Label` ](xref:Xamarin.Forms.Label) Instanzen:

![Bezeichnungen mit Text Dekorationen](label-images/label-textdecorations.png)

> [!NOTE]
> Textdekorationen können auch angewendet werden, um [ `Span` ](xref:Xamarin.Forms.Span) Instanzen. Weitere Informationen zu den `Span` Klasse, finden Sie unter [formatiertem Text](#Formatted_Text).

## <a name="character-spacing"></a>Zeichenabstand

Der Zeichenabstand kann auf [`Label`](xref:Xamarin.Forms.Label) Instanzen angewendet werden, indem die `Label.CharacterSpacing`-Eigenschaft auf einen `double` Wert festgelegt wird:

```xaml
<Label Text="Character spaced text"
       CharacterSpacing="10" />
```

Der entsprechende C#-Code lautet:

```csharp
Label label = new Label { Text = "Character spaced text", CharacterSpacing = 10 };
```

Das Ergebnis ist, dass Zeichen in dem Text, der vom [`Label`](xref:Xamarin.Forms.Label) angezeigt wird, `CharacterSpacing` geräteunabhängigen Einheiten voneinander entfernt sind.

## <a name="colors"></a>Farben

Bezeichnungen können festgelegt werden, verwenden Sie eine benutzerdefinierte Farbe, die über die bindbare [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor) Eigenschaft.

Besondere Sorgfalt ist erforderlich, um sicherzustellen, dass Farben auf jeder Plattform verwendet werden können. Da jede Plattform unterschiedliche Standardwerte für Text- und Hintergrundfarben verfügt, müssen Sie darauf achten, einen Standardwert auswählen, der auf den einzelnen funktioniert.

Im folgenden XAML-Beispiel wird die Textfarbe einer `Label`:

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

Die folgenden Screenshots zeigen das Ergebnis der Einstellung der `TextColor` Eigenschaft:

![TextColor-Beispiel für Bezeichnung](label-images/textcolor.png)

Weitere Informationen zu Farben, finden Sie unter [Farben](~/xamarin-forms/user-interface/colors.md).

## <a name="fonts"></a>Schriftarten

Weitere Informationen zum Angeben von Schriftarten auf einem `Label`, finden Sie unter [Schriftarten](~/xamarin-forms/user-interface/text/fonts.md).

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>Abschneiden und-Umbruch

Bezeichnungen können festgelegt werden, um Text zu behandeln, die nicht in einer Zeile in eine von mehreren Möglichkeiten, die verfügbar gemacht werden, indem Sie passen die `LineBreakMode` Eigenschaft. [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode) ist eine Enumeration mit den folgenden Werten:

- **HeadTruncation** &ndash; schneidet den Anfang der Text am Ende angezeigt.
- **CharacterWrap** &ndash; Text auf einer neuen Zeile an einer Zeichengrenze umschließt.
- **MiddleTruncation** &ndash; zeigt an, die Anfang und Ende des Texts, mit der mittleren ersetzen Sie dies durch ein Auslassungszeichen.
- **NoWrap** &ndash; wird die Anzeige von nur-Text nicht umbrochen so viel Text kann in einer Zeile passen.
- **TailTruncation** &ndash; zeigt den Anfang des Texts, Ende abgeschnitten.
- **WordWrap** &ndash; Zeilenumbruch an der Wortgrenze.

## <a name="display-a-specific-number-of-lines"></a>Anzeigen einer bestimmten Anzahl von Zeilen

Die Anzahl der Zeilen, die angezeigt wird eine [ `Label` ](xref:Xamarin.Forms.Label) kann angegeben werden, durch Festlegen der `Label.MaxLines` Eigenschaft, um eine `int` Wert:

- Wenn `MaxLines`-1 ist, d. h. der Standardwert, berücksichtigt die `Label` den Wert der [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) -Eigenschaft, um entweder nur eine Zeile, möglicherweise abgeschnitten oder alle Zeilen mit dem gesamten Text anzuzeigen.
- Wenn `MaxLines` 0 ist, wird der `Label` nicht angezeigt.
- Wenn `MaxLines` 1 ist, das Ergebnis ist identisch mit der Einstellung der [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode) Eigenschaft [ `NoWrap` ](xref:Xamarin.Forms.LineBreakMode), [ `HeadTruncation` ](xref:Xamarin.Forms.LineBreakMode), [ `MiddleTruncation` ](xref:Xamarin.Forms.LineBreakMode), oder [ `TailTruncation` ](xref:Xamarin.Forms.LineBreakMode). Allerdings die `Label` wird den Wert des berücksichtigt die [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode) Eigenschaft im Hinblick auf Platzierung von Auslassungszeichen, falls zutreffend.
- Bei der `MaxLines` ist größer als 1 ist, die `Label` unternehmensworkloads unter Berücksichtigung den Wert, der die angegebene Anzahl von Zeilen, zeigt der [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode) Eigenschaft im Hinblick auf Platzierung von Auslassungszeichen, falls zutreffend. Festlegen der `MaxLines` Eigenschaft mit einem Wert größer als 1 keine Auswirkungen hat, wenn die [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode) -Eigenschaftensatz auf [ `NoWrap` ](xref:Xamarin.Forms.LineBreakMode).

Der folgende XAML-Beispiel veranschaulicht das Festlegen der `MaxLines` Eigenschaft für eine [ `Label` ](xref:Xamarin.Forms.Label):

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

Die folgenden Screenshots zeigen das Ergebnis der Einstellung der `MaxLines` -Eigenschaft auf 2, wenn der Text lang genug, um mehr als 2 Zeilen belegt ist:

![Beispiel für eine Bezeichnung MaxLines](label-images/label-maxlines.png)

## <a name="display-html"></a>HTML anzeigen

Die [`Label`](xref:Xamarin.Forms.Label) -Klasse verfügt über eine `TextType`-Eigenschaft, die bestimmt, ob die `Label` Instanz nur-Text oder HTML-Text anzeigen soll. Diese Eigenschaft sollte auf einen der Member der `TextType`-Enumeration festgelegt werden:

- `Text` gibt an, dass der `Label` nur-Text anzeigt, und ist der Standardwert der `Label.TextType`-Eigenschaft.
- `Html` gibt an, dass die `Label` HTML-Text anzeigen wird.

Daher können [`Label`](xref:Xamarin.Forms.Label) Instanzen HTML-Code anzeigen, indem Sie die `Label.TextType`-Eigenschaft auf `Html`und die `Label.Text`-Eigenschaft auf eine HTML-Zeichenfolge festlegen:

```csharp
Label label = new Label
{
    Text = "This is <strong style=\"color:red\">HTML</strong> text.",
    TextType = TextType.Html
};
```

Im obigen Beispiel müssen die doppelten Anführungszeichen in HTML mithilfe des `\` Symbols mit Escapezeichen versehen werden.

In XAML können HTML-Zeichen folgen aufgrund der zusätzlichen Escapezeichen für die `<`-und `>` Symbole nicht lesbar werden:

```xaml
<Label Text="This is &lt;strong style=&quot;color:red&quot;&gt;HTML&lt;/strong&gt; text."
       TextType="Html"  />
```

Zur besseren Lesbarkeit kann der HTML-Code in einem `CDATA` Abschnitt Inline geschaltet werden:

```xaml
<Label TextType="Html">
    <![CDATA[
    This is <strong style="color:red">HTML</strong> text.
    ]]>
</Label>
```

In diesem Beispiel wird die `Label.Text`-Eigenschaft auf die HTML-Zeichenfolge festgelegt, die im `CDATA`-Abschnitt Inline ist. Dies funktioniert, da die `Text`-Eigenschaft der `ContentProperty` für die `Label`-Klasse ist.

Die folgenden Screenshots zeigen eine [`Label`](xref:Xamarin.Forms.Label) , die HTML anzeigt:

![Screenshots einer Bezeichnung, die HTML unter IOS und Android anzeigt](label-images/label-html.png)

> [!IMPORTANT]
> Das Anzeigen von HTML in einer [`Label`](xref:Xamarin.Forms.Label) ist auf die HTML-Tags beschränkt, die von der zugrunde liegenden Plattform unterstützt werden.

<a name="Formatted_Text" />

## <a name="formatted-text"></a>Formatierter Text

Bezeichnungen machen eine [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) Eigenschaft verfügbar, die die Darstellung von Text mit mehreren Schriftarten und Farben in derselben Ansicht ermöglicht.

Die `FormattedText` Eigenschaft ist vom Typ [ `FormattedString` ](xref:Xamarin.Forms.FormattedString), die umfasst eine oder mehrere [ `Span` ](xref:Xamarin.Forms.Span) Instanzen festlegen, über die [ `Spans` ](xref:Xamarin.Forms.FormattedString.Spans) Eigenschaft . Die folgenden `Span` Eigenschaften können verwendet werden, um die visuelle Darstellung festlegen:

- [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor) – die Hintergrundfarbe des Span-Element.
- `CharacterSpacing` vom Typ `double`: Abstand zwischen den Zeichen des `Span`-Texts.
- [`Font`](xref:Xamarin.Forms.Span.Font) – die Schriftart für den Text in der Spanne.
- [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes) – die Schriftartattribute für den Text in der Spanne.
- [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily) – die Schriftfamilie, zu der die Schriftart für den Text in der Spanne gehört.
- [`FontSize`](xref:Xamarin.Forms.Span.FontSize) – die Größe der Schriftart für den Text in der Spanne.
- [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor) – die Farbe für den Text in der Spanne. Diese Eigenschaft ist veraltet und wurde ersetzt durch die `TextColor` Eigenschaft.
- [`LineHeight`](xref:Xamarin.Forms.Span.LineHeight) -Der Multiplikator angewendet, die Standardhöhe der Zeile der Spanne. Weitere Informationen finden Sie unter [Zeilenhöhe](#line-height).
- [`Style`](xref:Xamarin.Forms.Span.Style) – der Stil, der auf die Spanne angewendet werden soll.
- [`Text`](xref:Xamarin.Forms.Span.Text) – der Text der Spanne.
- [`TextColor`](xref:Xamarin.Forms.Span.TextColor) – die Farbe für den Text in der Spanne.
- `TextDecorations` -der Ergänzungen, die den Text in der Spanne zuweisen. Weitere Informationen finden Sie unter [Textdekorationen](#text-decorations).

Die Eigenschaften [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor), [`Text`](xref:Xamarin.Forms.Span.Text)und [`Text`](xref:Xamarin.Forms.Span.Text) bindbare verfügen über den Standard Bindungs Modus [`OneWay`](xref:Xamarin.Forms.BindingMode). Weitere Informationen zu diesem Bindungs Modus finden Sie im [Bindungs Modus](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md) im Leitfaden zur [Standard Bindung](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md#the-default-binding-mode) .

Darüber hinaus die [ `GestureRecognizers` ](xref:Xamarin.Forms.GestureElement.GestureRecognizers) Eigenschaft kann verwendet werden, um eine Auflistung von Bewegung Erkennungen definieren, die auf Gesten reagiert, auf die [ `Span` ](xref:Xamarin.Forms.Span).

> [!NOTE]
> Es ist nicht möglich, HTML in einem [`Span`](xref:Xamarin.Forms.Span)anzuzeigen.

Das folgende XAML-Beispiel zeigt eine `FormattedText` -Eigenschaft, die besteht aus drei [ `Span` ](xref:Xamarin.Forms.Span) Instanzen:

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
> Die [ `Text` ](xref:Xamarin.Forms.Span.Text) Eigenschaft eine `Span` kann über die Datenbindung festgelegt werden. Weitere Informationen finden Sie unter [Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md).

Beachten Sie, dass eine [ `Span` ](xref:Xamarin.Forms.Span) können auch auf alle Aktionen, die das Span Elements hinzugefügt werden, reagieren [ `GestureRecognizers` ](xref:Xamarin.Forms.GestureElement.GestureRecognizers) Auflistung. Z. B. eine [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) wurde mit dem zweiten `Span` in den obigen Codebeispielen. Aus diesem Grund, wenn dies `Span` getippt wird die `TapGestureRecognizer` antwortet durch Ausführen der `ICommand` von definiert die [ `Command` ](xref:Xamarin.Forms.TapGestureRecognizer.Command) Eigenschaft. Weitere Informationen zu Geste Erkennungen, finden Sie unter [Xamarin.Forms Gesten](~/xamarin-forms/app-fundamentals/gestures/index.md).

Die folgenden Screenshots zeigen das Ergebnis der Einstellung der `FormattedString` Eigenschaft, um drei `Span` Instanzen:

![Bezeichnung FormattedText-Beispiel](label-images/formattedtext.png)

## <a name="line-height"></a>Zeilenhöhe

Die vertikale Gesamthöhe des eine [ `Label` ](xref:Xamarin.Forms.Label) und [ `Span` ](xref:Xamarin.Forms.Span) kann angepasst werden, durch Festlegen der [ `Label.LineHeight` ](xref:Xamarin.Forms.Label.LineHeight) Eigenschaft oder [ `Span.LineHeight` ](xref:Xamarin.Forms.Span.LineHeight) zu einem `double` Wert. Unter iOS und Android werden diese Werte Multiplikatoren, der die ursprüngliche Zeilenhöhe, und auf die universelle Windows-Plattform (UWP) die `Label.LineHeight` -Eigenschaftswert ist ein Faktor der Schriftgrad.

> [!NOTE]
>
> - Unter iOS die [ `Label.LineHeight` ](xref:Xamarin.Forms.Label.LineHeight) und [ `Span.LineHeight` ](xref:Xamarin.Forms.Span.LineHeight) Eigenschaften Ändern der Zeilenhöhe von Text, der in einer einzelnen Zeile passt und Text, der auf mehrere Zeilen umbrochen wird.
> - Unter Android die [ `Label.LineHeight` ](xref:Xamarin.Forms.Label.LineHeight) und [ `Span.LineHeight` ](xref:Xamarin.Forms.Span.LineHeight) Eigenschaften nur ändern, die Zeilenhöhe des Texts, die auf mehrere Zeilen umbrochen wird.
> - Auf UWP die [ `Label.LineHeight` ](xref:Xamarin.Forms.Label.LineHeight) eigenschaftenänderungen die Zeilenhöhe des Texts, die auf mehrere Zeilen umbrochen wird und die [ `Span.LineHeight` ](xref:Xamarin.Forms.Span.LineHeight) Eigenschaft hat keine Auswirkungen.

Der folgende XAML-Beispiel veranschaulicht das Festlegen der [ `LineHeight` ](xref:Xamarin.Forms.Label.LineHeight) Eigenschaft für eine [ `Label` ](xref:Xamarin.Forms.Label):

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

Die folgenden Screenshots zeigen das Ergebnis der Einstellung der [ `Label.LineHeight` ](xref:Xamarin.Forms.Label.LineHeight) Eigenschaft 1.8:

![Bezeichnung "LineHeight example"](label-images/label-lineheight.png)

Der folgende XAML-Beispiel veranschaulicht das Festlegen der [ `LineHeight` ](xref:Xamarin.Forms.Span.LineHeight) Eigenschaft für eine [ `Span` ](xref:Xamarin.Forms.Span):

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

Die folgenden Screenshots zeigen das Ergebnis der Einstellung der [ `Span.LineHeight` ](xref:Xamarin.Forms.Span.LineHeight) Eigenschaft 1.8:

![Span LineHeight-Beispiel](label-images/span-lineheight.png)

## <a name="padding"></a>Abstand

Padding stellt den Leerraum zwischen einem Element und seinen untergeordneten Elementen dar und wird verwendet, um das Element vom eigenen Inhalt zu trennen. Auffüll Zeichen können auf [`Label`](xref:Xamarin.Forms.Label) Instanzen angewendet werden, indem die `Label.Padding`-Eigenschaft auf einen [`Thickness`](xref:Xamarin.Forms.Thickness) Wert festgelegt wird:

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
> Wenn unter IOS eine [`Label`](xref:Xamarin.Forms.Label) erstellt wird, mit der die `Padding`-Eigenschaft festgelegt wird, wird Auffüllung angewendet, und der Auffüll Wert kann später aktualisiert werden. Wenn jedoch eine `Label` erstellt wird, die die `Padding`-Eigenschaft nicht festgelegt, hat der Versuch, Sie zu einem späteren Zeitpunkt festzulegen, keine Auswirkung.
>
> Unter Android und der universelle Windows-Plattform kann der `Padding`-Eigenschafts Wert beim Erstellen des `Label` oder später angegeben werden.

Weitere Informationen zum Auffüllen finden Sie unter [Ränder und Padding](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).

## <a name="hyperlinks"></a>Links

Der von [`Label`](xref:Xamarin.Forms.Label) -und [`Span`](xref:Xamarin.Forms.Span) Instanzen angezeigte Text kann mit folgendem Ansatz in Hyperlinks umgewandelt werden:

1. Legen Sie die Eigenschaften `TextColor` und `TextDecoration` der [`Label`](xref:Xamarin.Forms.Label) oder [`Span`](xref:Xamarin.Forms.Span)fest.
1. Fügen Sie der [`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers) Auflistung der [`Label`](xref:Xamarin.Forms.Label) oder [`Span`](xref:Xamarin.Forms.Span)ein [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) hinzu, dessen [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command) -Eigenschaft an einen `ICommand`gebunden ist und dessen [`CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) -Eigenschaft die zu öffnende URL enthält.
1. Definieren Sie die `ICommand`, die vom [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)ausgeführt wird.
1. Schreiben Sie den Code, der vom `ICommand`ausgeführt wird.

Das folgende Codebeispiel stammt aus dem Beispiel für [Hyperlink-Demos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks/) und zeigt eine [`Label`](xref:Xamarin.Forms.Label) , deren Inhalt aus mehreren [`Span`](xref:Xamarin.Forms.Span) Instanzen festgelegt ist:

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

In diesem Beispiel bilden der erste und der dritte [`Span`](xref:Xamarin.Forms.Span) -Instanzen Text, während der zweite `Span` einen anfügbaren Hyperlink darstellt. Die Textfarbe ist auf Blau festgelegt, und Sie verfügt über eine Unterstreichung der Text Dekoration. Dadurch wird ein Hyperlink erstellt, wie in den folgenden Screenshots gezeigt:

[![Links](label-images/hyperlinks-small.png "Links")](label-images/hyperlinks-large.png#lightbox)

Wenn der Hyperlink abgetippt wird, antwortet der [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) , indem er die von seiner [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command) -Eigenschaft definierte `ICommand` ausführt. Außerdem wird die von der [`CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) -Eigenschaft angegebene URL an die `ICommand` als Parameter übergeben.

Der Code-Behind für die XAML-Seite enthält die `TapCommand` Implementierung:

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

Der `TapCommand` führt die `Launcher.OpenAsync`-Methode aus und übergibt dabei den [`TapGestureRecognizer.CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) -Eigenschafts Wert als Parameter. Die `Launcher.OpenAsync`-Methode wird von xamarin. Essentials bereitgestellt und öffnet die URL in einem Webbrowser. Der Gesamteffekt besteht daher darin, dass beim Tippen auf der Seite ein Webbrowser angezeigt wird und die URL, zu der der Hyperlink verknüpft ist, zu navigieren.

### <a name="creating-a-reusable-hyperlink-class"></a>Erstellen einer wiederverwendbaren Hyperlink-Klasse

Die vorherige Methode zum Erstellen eines Links erfordert das Schreiben von wiederholtem Code, wenn Sie in Ihrer Anwendung einen Hyperlink benötigen. Allerdings können die Klassen [`Label`](xref:Xamarin.Forms.Label) und [`Span`](xref:Xamarin.Forms.Span) untergeordnet werden, um `HyperlinkLabel`-und `HyperlinkSpan` Klassen zu erstellen, wobei die Gestenerkennung und der Text Formatierungs Code dort hinzugefügt werden.

Das folgende Codebeispiel stammt aus dem Beispiel für [Hyperlink-Demos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks/) und zeigt eine `HyperlinkSpan`-Klasse:

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

Die `HyperlinkSpan`-Klasse definiert eine `Url`-Eigenschaft und zugeordnete [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), und der-Konstruktor legt die Hyperlink-Darstellung und die [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) fest, die beim Tippen des Links reagieren werden. Wenn ein `HyperlinkSpan` abgetippt wird, antwortet der `TapGestureRecognizer`, indem er die `Launcher.OpenAsync`-Methode ausführt, um die von der `Url`-Eigenschaft angegebene URL in einem Webbrowser zu öffnen.

Die `HyperlinkSpan`-Klasse kann durch Hinzufügen einer Instanz der-Klasse zum XAML-Code verwendet werden:

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

## <a name="styling-labels"></a>Formatierung von Bezeichnungen

In den vorherigen Abschnitten behandelt Einstellung [ `Label` ](xref:Xamarin.Forms.Label) und [ `Span` ](xref:Xamarin.Forms.Span) Eigenschaften auf einer Basis pro Instanz. Allerdings können Sätze von Eigenschaften in einem Stil gruppiert werden, die mit einem oder mehreren Ansichten, konsistent angewendet wird. Dies kann die Lesbarkeit des Codes zu erhöhen und stellen Änderungen des Entwurfs einfacher zu implementieren. Weitere Informationen finden Sie unter [Stile](~/xamarin-forms/user-interface/text/styles.md).

## <a name="related-links"></a>Verwandte Links

- [Text (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [Hyperlinks (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks)
- [Erstellen von mobilen Apps mit Xamarin.Forms Kapitel 3](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [Label-API](xref:Xamarin.Forms.Label)
- [Span-API](xref:Xamarin.Forms.Span)
