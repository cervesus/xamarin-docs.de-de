---
Title: "Bezeichnung Xamarin.Forms " Description: "in diesem Artikel wird erläutert, wie Sie die Xamarin.Forms Label-Klasse verwenden, um einen Text in einer einzelnen und mehreren Zeilen in Anwendungen anzuzeigen."
ms. Prod: xamarin ms. assetid: 02e6c553-5670-49a0-8ee9-5153ed21ea91 ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 04/09/2020 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-label"></a>Xamarin.FormsETI

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Anzeigen von Text in xamarin. Forms_

Die [`Label`](xref:Xamarin.Forms.Label) Sicht wird zum Anzeigen von Text verwendet, sowohl für einzelne als auch für mehrere Zeilen. Bezeichnungen können Text Dekorationen und farbige Text enthalten und benutzerdefinierte Schriftarten (Familien, Größen und Optionen) verwenden.

## <a name="text-decorations"></a>Text Dekorationen

Text Dekorationen unterstrichen und durchgestrichen können auf-Instanzen angewendet werden, [`Label`](xref:Xamarin.Forms.Label) indem die- `Label.TextDecorations` Eigenschaft auf ein oder mehrere Enumerationsmember festgelegt wird `TextDecorations` :

- `None`
- `Underline`
- `Strikethrough`

Im folgenden XAML-Beispiel wird das Festlegen der- `Label.TextDecorations` Eigenschaft veranschaulicht:

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

Die folgenden Screenshots zeigen die `TextDecorations` auf-Instanzen angewendeten Enumerationsmember [`Label`](xref:Xamarin.Forms.Label) :

![Bezeichnungen mit Text Dekorationen](label-images/label-textdecorations.png)

> [!NOTE]
> Text Dekorationen können auch auf-Instanzen angewendet werden [`Span`](xref:Xamarin.Forms.Span) . Weitere Informationen zur- `Span` Klasse finden Sie unter [formatierten Text](#formatted-text).

## <a name="character-spacing"></a>Zeichenabstand

Der Zeichenabstand kann auf-Instanzen angewendet werden, [`Label`](xref:Xamarin.Forms.Label) indem die- `Label.CharacterSpacing` Eigenschaft auf einen Wert festgelegt wird `double` :

```xaml
<Label Text="Character spaced text"
       CharacterSpacing="10" />
```

Der entsprechende C#-Code lautet:

```csharp
Label label = new Label { Text = "Character spaced text", CharacterSpacing = 10 };
```

Das Ergebnis ist, dass Zeichen im Text, die von angezeigt [`Label`](xref:Xamarin.Forms.Label) werden, `CharacterSpacing` geräteunabhängige Einheiten voneinander getrennt sind.

## <a name="new-lines"></a>Zeilenumbrüche

Es gibt zwei Haupttechniken zum Erzwingen von Text in einem in [`Label`](xref:Xamarin.Forms.Label) einer neuen Zeile, von XAML:

1. Verwenden Sie das Unicode-Zeilenvorschub Zeichen " &amp; #10;".
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

Bezeichnungen können so festgelegt werden, dass eine benutzerdefinierte Textfarbe über die bindbare Eigenschaft verwendet wird [`TextColor`](xref:Xamarin.Forms.Label.TextColor) .

Eine besondere Sorgfalt ist erforderlich, um sicherzustellen, dass Farben auf jeder Plattform verwendet werden können. Da jede Plattform über andere Standardwerte für Text-und Hintergrundfarben verfügt, müssen Sie darauf achten, einen Standardwert auszuwählen, der für jede Plattform geeignet ist.

Im folgenden XAML-Beispiel wird die Textfarbe eines festgelegt `Label` :

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

Die folgenden Screenshots zeigen das Ergebnis der Einstellung der- `TextColor` Eigenschaft:

![TextColor-Beispiel für Bezeichnung](label-images/textcolor.png)

Weitere Informationen zu Farben finden Sie unter [Farben](~/xamarin-forms/user-interface/colors.md).

## <a name="fonts"></a>Fonts

Weitere Informationen zum Angeben von Schriftarten auf einem `Label` finden Sie unter [Schriftarten](~/xamarin-forms/user-interface/text/fonts.md).

## <a name="truncation-and-wrapping"></a>Abschneiden und umwickeln

Bezeichnungen können so festgelegt werden, dass Sie Text verarbeiten können, der auf eine der folgenden Arten nicht in einer Zeile passen kann `LineBreakMode` . [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode)ist eine Enumeration mit den folgenden Werten:

- **Headtrunzierung** &ndash; verkürzt den Anfang des Texts und zeigt das Ende an.
- **Merkmal Umbruch** &ndash; umschließt Text in eine neue Zeile an einer Zeichengrenze.
- **Middletrunzierung** &ndash; zeigt den Anfang und das Ende des Texts an, wobei die mittlere durch ein Auslassungs Zeichen ersetzt wird.
- **Nowrap** &ndash; schließt keinen Text ein, sodass nur so viel Text angezeigt wird, wie er in eine Zeile passen kann.
- **Tailtrunation** &ndash; zeigt den Anfang des Texts an und verkürzt das Ende.
- **WordWrap** &ndash; umschließt Text an der Wort Grenze.

## <a name="display-a-specific-number-of-lines"></a>Anzeigen einer bestimmten Anzahl von Zeilen

Die Anzahl der Zeilen, die von einer angezeigt [`Label`](xref:Xamarin.Forms.Label) werden, kann durch Festlegen der- `Label.MaxLines` Eigenschaft auf einen Wert angegeben werden `int` :

- Wenn-1 ist, d. h. der `MaxLines` Standardwert, `Label` respektiert den Wert der- [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) Eigenschaft, um entweder nur eine Zeile, möglicherweise abgeschnitten oder alle Zeilen mit dem gesamten Text anzuzeigen.
- Wenn `MaxLines` den Wert 0 hat, wird der `Label` nicht angezeigt.
- Wenn den Wert `MaxLines` 1 hat, ist das Ergebnis mit dem Festlegen der- [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) Eigenschaft auf [`NoWrap`](xref:Xamarin.Forms.LineBreakMode) , [`HeadTruncation`](xref:Xamarin.Forms.LineBreakMode) , [`MiddleTruncation`](xref:Xamarin.Forms.LineBreakMode) oder identisch [`TailTruncation`](xref:Xamarin.Forms.LineBreakMode) . Allerdings berücksichtigt der den Wert der-Eigenschaft hinsichtlich der Platzierung eines Auslassungs Zeichen `Label` [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) , falls zutreffend.
- Wenn `MaxLines` größer als 1 ist, `Label` wird das bis zur angegebenen Anzahl von Zeilen angezeigt. dabei wird der Wert der Eigenschaft in [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) Bezug auf die Platzierung der Auslassungs Punkte berücksichtigt, falls zutreffend. Das Festlegen der- `MaxLines` Eigenschaft auf einen Wert größer als 1 hat jedoch keine Auswirkung, wenn die- [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) Eigenschaft auf festgelegt ist [`NoWrap`](xref:Xamarin.Forms.LineBreakMode) .

Das folgende XAML-Beispiel veranschaulicht das Festlegen der- `MaxLines` Eigenschaft für einen [`Label`](xref:Xamarin.Forms.Label) :

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

Die folgenden Screenshots zeigen das Ergebnis der Einstellung der- `MaxLines` Eigenschaft auf 2, wenn der Text lang genug ist, um mehr als 2 Zeilen zu belegen:

![Beispiel für eine Bezeichnung MaxLines](label-images/label-maxlines.png)

## <a name="display-html"></a>Anzeigen von HTML

Die- [`Label`](xref:Xamarin.Forms.Label) Klasse verfügt über eine- `TextType` Eigenschaft, die bestimmt, ob die-Instanz nur- `Label` Text oder HTML-Text anzeigen soll. Diese Eigenschaft sollte auf einen der Member der-Enumeration festgelegt werden `TextType` :

- `Text`Gibt an, dass der `Label` nur-Text anzeigt und der Standardwert der `Label.TextType` Eigenschaft ist.
- `Html`Gibt an, dass der `Label` HTML-Text anzeigt.

Daher [`Label`](xref:Xamarin.Forms.Label) können-Instanzen HTML anzeigen, indem die `Label.TextType` -Eigenschaft auf `Html` und die- `Label.Text` Eigenschaft auf eine HTML-Zeichenfolge festgelegt wird:

```csharp
Label label = new Label
{
    Text = "This is <strong style=\"color:red\">HTML</strong> text.",
    TextType = TextType.Html
};
```

Im obigen Beispiel müssen die doppelten Anführungszeichen in HTML mithilfe des Symbols mit Escapezeichen versehen werden `\` .

In XAML können HTML-Zeichen folgen aufgrund der zusätzlichen Escapezeichen der `<` Symbole und nicht lesbar werden `>` :

```xaml
<Label Text="This is &lt;strong style=&quot;color:red&quot;&gt;HTML&lt;/strong&gt; text."
       TextType="Html"  />
```

Zur besseren Lesbarkeit kann der HTML-Code in einem Abschnitt Inline sein `CDATA` :

```xaml
<Label TextType="Html">
    <![CDATA[
    This is <strong style="color:red">HTML</strong> text.
    ]]>
</Label>
```

In diesem Beispiel wird die `Label.Text` -Eigenschaft auf die HTML-Zeichenfolge festgelegt, die im-Abschnitt Inline ist `CDATA` . Dies funktioniert, da die- `Text` Eigenschaft `ContentProperty` für die-Klasse ist `Label` .

Die folgenden Screenshots zeigen, wie [`Label`](xref:Xamarin.Forms.Label) HTML angezeigt wird:

![Screenshots einer Bezeichnung, die HTML unter IOS und Android anzeigt](label-images/label-html.png)

> [!IMPORTANT]
> Das Anzeigen von HTML in einer [`Label`](xref:Xamarin.Forms.Label) ist auf die HTML-Tags beschränkt, die von der zugrunde liegenden Plattform unterstützt werden.

## <a name="formatted-text"></a>Formatierter Text

Bezeichnungen machen eine [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) Eigenschaft verfügbar, die die Darstellung von Text mit mehreren Schriftarten und Farben in derselben Ansicht ermöglicht.

Die- `FormattedText` Eigenschaft ist vom Typ [`FormattedString`](xref:Xamarin.Forms.FormattedString) , der eine oder mehrere-Instanzen enthält, die [`Span`](xref:Xamarin.Forms.Span) über die-Eigenschaft festgelegt werden [`Spans`](xref:Xamarin.Forms.FormattedString.Spans) . Die folgenden `Span` Eigenschaften können verwendet werden, um die visuelle Darstellung festzulegen:

- [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor)– die Farbe des spannen Hintergrunds.
- `CharacterSpacing` vom Typ `double`: Abstand zwischen den Zeichen des `Span`-Texts.
- [`Font`](xref:Xamarin.Forms.Span.Font)– die Schriftart für den Text in der Spanne.
- [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes)– die Schriftart Attribute für den Text in der Spanne.
- [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily)– die Schriftfamilie, zu der die Schriftart für den Text in der Spanne gehört.
- [`FontSize`](xref:Xamarin.Forms.Span.FontSize)– die Größe der Schriftart für den Text in der Spanne.
- [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor)– die Farbe für den Text in der Spanne. Diese Eigenschaft ist veraltet und wurde durch die- `TextColor` Eigenschaft ersetzt.
- [`LineHeight`](xref:Xamarin.Forms.Span.LineHeight)-der Multiplikator, der auf die Standard Zeilenhöhe der Spanne angewendet werden soll. Weitere Informationen finden Sie unter [Zeilenhöhe](#line-height).
- [`Style`](xref:Xamarin.Forms.Span.Style)– der Stil, der auf die Spanne angewendet werden soll.
- [`Text`](xref:Xamarin.Forms.Span.Text)– der Text der Spanne.
- [`TextColor`](xref:Xamarin.Forms.Span.TextColor)– die Farbe für den Text in der Spanne.
- `TextDecorations`-die auf den Text in der Spanne anzuwendenden Dekorationen. Weitere Informationen finden Sie unter [Text Dekorationen](#text-decorations).

Die [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor) [`Text`](xref:Xamarin.Forms.Span.Text) [`Text`](xref:Xamarin.Forms.Span.Text) bindbaren Eigenschaften, und verfügen über einen Standard Bindungs Modus von [`OneWay`](xref:Xamarin.Forms.BindingMode) . Weitere Informationen zu diesem Bindungs Modus finden Sie im [Bindungs Modus](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md) im Leitfaden zur [Standard Bindung](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md#the-default-binding-mode) .

Außerdem kann die- [`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers) Eigenschaft verwendet werden, um eine Auflistung von Gesten Erkennungs Programmen zu definieren, die auf Gesten auf dem reagieren [`Span`](xref:Xamarin.Forms.Span) .

> [!NOTE]
> Es ist nicht möglich, HTML in einem anzuzeigen [`Span`](xref:Xamarin.Forms.Span) .

Das folgende XAML-Beispiel veranschaulicht eine `FormattedText` Eigenschaft, die aus drei [`Span`](xref:Xamarin.Forms.Span) Instanzen besteht:

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
> Die- [`Text`](xref:Xamarin.Forms.Span.Text) Eigenschaft eines `Span` kann durch Datenbindung festgelegt werden. Weitere Informationen finden Sie unter [Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md).

Beachten Sie, dass ein [`Span`](xref:Xamarin.Forms.Span) auch auf alle Gesten reagieren kann, die der Span-Auflistung hinzugefügt werden [`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers) . Beispielsweise wurde eine [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) der zweiten `Span` in den obigen Codebeispielen hinzugefügt. Wenn dieses- `Span` Objekt abgetippt wird, antwortet daher, `TapGestureRecognizer` indem das `ICommand` von der-Eigenschaft definierte ausgeführt wird [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command) . Weitere Informationen zu Gesten erkentungen finden Sie unter [ Xamarin.Forms Gesten](~/xamarin-forms/app-fundamentals/gestures/index.md).

Die folgenden Screenshots zeigen das Ergebnis der Einstellung der- `FormattedString` Eigenschaft auf drei `Span` Instanzen:

![Bezeichnung FormattedText-Beispiel](label-images/formattedtext.png)

## <a name="line-height"></a>Zeilenhöhe

Die vertikale Höhe eines [`Label`](xref:Xamarin.Forms.Label) und eines [`Span`](xref:Xamarin.Forms.Span) kann angepasst werden, indem die- [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) Eigenschaft oder [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) auf einen-Wert festgelegt wird `double` . Unter IOS und Android sind diese Werte multipliziert mit der ursprünglichen Zeilenhöhe, und auf der universelle Windows-Plattform (UWP) ist der `Label.LineHeight` Eigenschafts Wert ein Multiplikator für die Schriftgröße der Bezeichnung.

> [!NOTE]
>
> - Unter IOS ändern die [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) -Eigenschaft und die-Eigenschaft [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) die Zeilenhöhe von Text, der in eine einzelne Zeile passt, sowie Text, der in mehrere Zeilen umbrochen wird.
> - Unter Android ändern die [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) -Eigenschaft und die-Eigenschaft [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) nur die Zeilenhöhe von Text, der in mehrere Zeilen umbrochen wird.
> - Bei UWP ändert die- [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) Eigenschaft die Zeilenhöhe von Text, der in mehrere Zeilen umbrochen wird, und die- [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) Eigenschaft hat keine Auswirkungen.

Das folgende XAML-Beispiel veranschaulicht das Festlegen der- [`LineHeight`](xref:Xamarin.Forms.Label.LineHeight) Eigenschaft für einen [`Label`](xref:Xamarin.Forms.Label) :

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

Die folgenden Screenshots zeigen das Ergebnis der Einstellung der- [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) Eigenschaft auf 1,8:

![Bezeichnung "LineHeight example"](label-images/label-lineheight.png)

Das folgende XAML-Beispiel veranschaulicht das Festlegen der- [`LineHeight`](xref:Xamarin.Forms.Span.LineHeight) Eigenschaft für einen [`Span`](xref:Xamarin.Forms.Span) :

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

Die folgenden Screenshots zeigen das Ergebnis der Einstellung der- [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) Eigenschaft auf 1,8:

![Span LineHeight-Beispiel](label-images/span-lineheight.png)

## <a name="padding"></a>Abstand

Padding stellt den Leerraum zwischen einem Element und seinen untergeordneten Elementen dar und wird verwendet, um das Element vom eigenen Inhalt zu trennen. Auffüll Zeichen können auf-Instanzen angewendet werden, [`Label`](xref:Xamarin.Forms.Label) indem die- `Label.Padding` Eigenschaft auf einen Wert festgelegt wird [`Thickness`](xref:Xamarin.Forms.Thickness) :

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
> Wenn unter IOS ein [`Label`](xref:Xamarin.Forms.Label) erstellt wird, mit dem die-Eigenschaft festgelegt wird `Padding` , wird Auffüllung angewendet, und der Auffüll Wert kann später aktualisiert werden. Wenn jedoch ein `Label` erstellt wird, von dem die-Eigenschaft nicht festgelegt wird `Padding` , hat der Versuch, ihn später festzulegen, keine Auswirkung.
>
> Unter Android und der universelle Windows-Plattform kann der- `Padding` Eigenschafts Wert beim Erstellen von `Label` oder später angegeben werden.

Weitere Informationen zum Auffüllen finden Sie unter [Ränder und Padding](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).

## <a name="hyperlinks"></a>Hyperlinks

Der von der [`Label`](xref:Xamarin.Forms.Label) -Instanz und der-Instanz angezeigte Text [`Span`](xref:Xamarin.Forms.Span) kann mit dem folgenden Ansatz in Hyperlinks umgewandelt werden:

1. Legen Sie `TextColor` die `TextDecoration` Eigenschaften und von [`Label`](xref:Xamarin.Forms.Label) oder fest [`Span`](xref:Xamarin.Forms.Span) .
1. Fügen Sie der [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) [`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers) -Auflistung des [`Label`](xref:Xamarin.Forms.Label) [`Span`](xref:Xamarin.Forms.Span) [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command) -Objekts oder-Objekts, dessen-Eigenschaft an einen gebunden `ICommand` ist und dessen- [`CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) Eigenschaft die zu öffnende URL enthält, ein hinzu.
1. Definieren Sie das `ICommand` , das von ausgeführt wird [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) .
1. Schreiben Sie den Code, der von der ausgeführt wird `ICommand` .

Das folgende Codebeispiel stammt aus dem Beispiel für [Hyperlink-Demos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks/) und zeigt eine, [`Label`](xref:Xamarin.Forms.Label) deren Inhalt von mehreren-Instanzen festgelegt wird [`Span`](xref:Xamarin.Forms.Span) :

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

In diesem Beispiel bilden die erste und die dritte [`Span`](xref:Xamarin.Forms.Span) Instanz Text, während der zweite `Span` einen anfügbaren Hyperlink darstellt. Die Textfarbe ist auf Blau festgelegt, und Sie verfügt über eine Unterstreichung der Text Dekoration. Dadurch wird ein Hyperlink erstellt, wie in den folgenden Screenshots gezeigt:

[![Hyperlinks](label-images/hyperlinks-small.png "Hyperlinks")](label-images/hyperlinks-large.png#lightbox)

Wenn der Hyperlink abgetippt wird, [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) antwortet der durch Ausführen der, die durch die- `ICommand` Eigenschaft definiert wird [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command) . Außerdem wird die von der-Eigenschaft angegebene URL [`CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) als Parameter an das-Objekt übergeben `ICommand` .

Die Code-Behind-Datei für die XAML-Seite enthält die- `TapCommand` Implementierung:

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

`TapCommand`Führt die- `Launcher.OpenAsync` Methode aus und übergibt den- [`TapGestureRecognizer.CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) Eigenschafts Wert als Parameter. Die `Launcher.OpenAsync` -Methode wird von bereitgestellt Xamarin.Essentials und öffnet die URL in einem Webbrowser. Der Gesamteffekt besteht daher darin, dass beim Tippen auf der Seite ein Webbrowser angezeigt wird und die URL, zu der der Hyperlink verknüpft ist, zu navigieren.

### <a name="creating-a-reusable-hyperlink-class"></a>Erstellen einer wiederverwendbaren Hyperlink-Klasse

Die vorherige Methode zum Erstellen eines Links erfordert das Schreiben von wiederholtem Code, wenn Sie in Ihrer Anwendung einen Hyperlink benötigen. Allerdings können die [`Label`](xref:Xamarin.Forms.Label) -Klasse und die- [`Span`](xref:Xamarin.Forms.Span) Klasse zum Erstellen von `HyperlinkLabel` -und-Klassen unter klassifiziert werden `HyperlinkSpan` , wobei die Gestenerkennung und der Text Formatierungs Code dort hinzugefügt werden.

Das folgende Codebeispiel stammt aus dem Beispiel für [Hyperlink-Demos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks/) und zeigt eine- `HyperlinkSpan` Klasse:

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

Die `HyperlinkSpan` -Klasse definiert eine-Eigenschaft und eine zugeordnete-Klasse `Url` [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) , und der-Konstruktor legt die Hyperlink-Darstellung und die fest, die [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) beim Tippen des Links reagieren wird. Wenn ein- `HyperlinkSpan` Objekt getippt wird, antwortet das, `TapGestureRecognizer` indem die-Methode ausgeführt wird, `Launcher.OpenAsync` um die von der-Eigenschaft angegebene URL `Url` in einem Webbrowser zu öffnen.

Die- `HyperlinkSpan` Klasse kann verwendet werden, indem Sie der XAML eine Instanz der-Klasse hinzufügen:

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

In den vorherigen Abschnitten wurden die Einstellungen [`Label`](xref:Xamarin.Forms.Label) und [`Span`](xref:Xamarin.Forms.Span) Eigenschaften pro Instanz behandelt. Allerdings können Sätze von Eigenschaften in einem Stil gruppiert werden, der konsistent auf eine oder mehrere Ansichten angewendet wird. Dies kann die Lesbarkeit von Code erhöhen und die Implementierung von Entwurfs Änderungen vereinfachen. Weitere Informationen finden Sie unter [Stile](~/xamarin-forms/user-interface/text/styles.md).

## <a name="related-links"></a>Verwandte Links

- [Text (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [Hyperlinks (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks)
- [Erstellen von Mobile Apps mit Xamarin.Forms , Kapitel 3](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [Label-API](xref:Xamarin.Forms.Label)
- [Span-API](xref:Xamarin.Forms.Span)
