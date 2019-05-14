---
title: Xamarin.Forms-Bezeichnung
description: In diesem Artikel wird erläutert, wie Xamarin.Forms Label-Klasse zu verwenden, um einzelne und mehrzeiligen Text in Anwendungen anzuzeigen.
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/26/2019
ms.openlocfilehash: 31303114ddd829b596569981b5812b91c4e95b30
ms.sourcegitcommit: 0cb62b02a7efb5426f2356d7dbdfd9afd85f2f4a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/13/2019
ms.locfileid: "65557443"
---
# <a name="xamarinforms-label"></a>Xamarin.Forms-Bezeichnung

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)

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

Der entsprechende C#-Code ist:

```csharp
var underlineLabel = new Label { Text = "This is underlined text.", TextDecorations = TextDecorations.Underline };
var strikethroughLabel = new Label { Text = "This is text with strikethrough.", TextDecorations = TextDecorations.Strikethrough };
var bothLabel = new Label { Text = "This is underlined text with strikethrough.", TextDecorations = TextDecorations.Underline | TextDecorations.Strikethrough };
```

Die folgenden Screenshots zeigen die `TextDecorations` Enumerationsmember angewendet [ `Label` ](xref:Xamarin.Forms.Label) Instanzen:

![](label-images/label-textdecorations.png "Bezeichnungen mit Textergänzungen")

> [!NOTE]
> Textdekorationen können auch angewendet werden, um [ `Span` ](xref:Xamarin.Forms.Span) Instanzen. Weitere Informationen zu den `Span` Klasse, finden Sie unter [formatiertem Text](#Formatted_Text).

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

Der entsprechende C#-Code ist:

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

![](label-images/textcolor.png "Bezeichnung TextColor-Beispiel")

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

## <a name="displaying-a-specific-number-of-lines"></a>Anzeigen einer bestimmten Anzahl von Zeilen

Die Anzahl der Zeilen, die angezeigt wird eine [ `Label` ](xref:Xamarin.Forms.Label) kann angegeben werden, durch Festlegen der `Label.MaxLines` Eigenschaft, um eine `int` Wert:

- Wenn `MaxLines` ist 0 (null) der `Label` berücksichtigt den Wert des der [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode) Eigenschaft, um entweder nur eine Zeile, die möglicherweise abgeschnitten, anzeigen oder alle Zeilen mit den gesamten Text.
- Wenn `MaxLines` 1 ist, das Ergebnis ist identisch mit der Einstellung der [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode) Eigenschaft [ `NoWrap` ](xref:Xamarin.Forms.LineBreakMode), [ `HeadTruncation` ](xref:Xamarin.Forms.LineBreakMode), [ `MiddleTruncation` ](xref:Xamarin.Forms.LineBreakMode), oder [ `TailTruncation` ](xref:Xamarin.Forms.LineBreakMode). Allerdings die `Label` wird den Wert des berücksichtigt die [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode) Eigenschaft im Hinblick auf Platzierung von Auslassungszeichen, falls zutreffend.
- Bei der `MaxLines` ist größer als 1 ist, die `Label` unternehmensworkloads unter Berücksichtigung den Wert, der die angegebene Anzahl von Zeilen, zeigt der [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode) Eigenschaft im Hinblick auf Platzierung von Auslassungszeichen, falls zutreffend. Festlegen der `MaxLines` Eigenschaft mit einem Wert größer als 1 keine Auswirkungen hat, wenn die [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode) -Eigenschaftensatz auf [ `NoWrap` ](xref:Xamarin.Forms.LineBreakMode).

Der folgende XAML-Beispiel veranschaulicht das Festlegen der `MaxLines` Eigenschaft für eine [ `Label` ](xref:Xamarin.Forms.Label):

```xaml
<Label Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus."
       LineBreakMode="WordWrap"
       MaxLines="2" />
```

Der entsprechende C#-Code ist:

```csharp
var label =
{
  Text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus.", LineBreakMode = LineBreakMode.WordWrap,
  MaxLines = 2
};
```

Die folgenden Screenshots zeigen das Ergebnis der Einstellung der `MaxLines` -Eigenschaft auf 2, wenn der Text lang genug, um mehr als 2 Zeilen belegt ist:

![](label-images/label-maxlines.png "Bezeichnung MaxLines-Beispiel")

<a name="Formatted_Text" />

## <a name="formatted-text"></a>Formatierter Text

Bezeichnungen verfügbar machen eine [ `FormattedText` ](xref:Xamarin.Forms.Label.FormattedText) -Eigenschaft, können die Darstellung von Text mit mehreren Schriftarten und Farben in derselben Ansicht.

Die `FormattedText` Eigenschaft ist vom Typ [ `FormattedString` ](xref:Xamarin.Forms.FormattedString), die umfasst eine oder mehrere [ `Span` ](xref:Xamarin.Forms.Span) Instanzen festlegen, über die [ `Spans` ](xref:Xamarin.Forms.FormattedString.Spans) Eigenschaft . Die folgenden `Span` Eigenschaften können verwendet werden, um die visuelle Darstellung festlegen:

- [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor) – die Hintergrundfarbe des Span-Element.
- [`Font`](xref:Xamarin.Forms.Span.Font) – die Schriftart für den Text in der Spanne.
- [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes) – die Schriftartattribute für den Text in der Spanne.
- [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily) – die Schriftfamilie, zu der die Schriftart für den Text in der Spanne gehört.
- [`FontSize`](xref:Xamarin.Forms.Span.FontSize) – die Größe der Schriftart für den Text in der Spanne.
- [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor) – die Farbe für den Text in der Spanne. Diese Eigenschaft ist veraltet und wurde ersetzt durch die `TextColor` Eigenschaft.
- [`LineHeight`](xref:Xamarin.Forms.Span.LineHeight) -Der Multiplikator angewendet, die Standardhöhe der Zeile der Spanne. Weitere Informationen finden Sie unter [Zeilenhöhe](#line-height).
- [`Style`](xref:Xamarin.Forms.Span.Style)  – die Formatvorlage, die für die Spanne gelten.
- [`Text`](xref:Xamarin.Forms.Span.Text) – der Text der Spanne.
- [`TextColor`](xref:Xamarin.Forms.Span.TextColor) – die Farbe für den Text in der Spanne.
- `TextDecorations` -der Ergänzungen, die den Text in der Spanne zuweisen. Weitere Informationen finden Sie unter [Textdekorationen](#text-decorations).

> [!NOTE]
> Die [ `BackgroundColor` ](xref:Xamarin.Forms.Span.BackgroundColor), [ `Text` ](xref:Xamarin.Forms.Span.Text), und [ `Text` ](xref:Xamarin.Forms.Span.Text) bindbare Eigenschaften haben einen Standardmodus für die Bindung der [ `OneWay` ](xref:Xamarin.Forms.BindingMode). Weitere Informationen zu dieser Bindungsmodus, finden Sie unter [der Standard-Bindungsmodus](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md#the-default-binding-mode) in die [Bindungsmodus](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md) Guide.

Darüber hinaus die [ `GestureRecognizers` ](xref:Xamarin.Forms.GestureElement.GestureRecognizers) Eigenschaft kann verwendet werden, um eine Auflistung von Bewegung Erkennungen definieren, die auf Gesten reagiert, auf die [ `Span` ](xref:Xamarin.Forms.Span).

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

![](label-images/formattedtext.png "Bezeichnung FormattedText-Beispiel")

## <a name="line-height"></a>Zeilenhöhe

Die vertikale Gesamthöhe des eine [ `Label` ](xref:Xamarin.Forms.Label) und [ `Span` ](xref:Xamarin.Forms.Span) kann angepasst werden, durch Festlegen der [ `Label.LineHeight` ](xref:Xamarin.Forms.Label.LineHeight) Eigenschaft oder [ `Span.LineHeight` ](xref:Xamarin.Forms.Span.LineHeight) zu einem `double` Wert. Unter iOS und Android werden diese Werte Multiplikatoren, der die ursprüngliche Zeilenhöhe, und auf die universelle Windows-Plattform (UWP) die `Label.LineHeight` -Eigenschaftswert ist ein Faktor der Schriftgrad.

> [!NOTE]
> - Unter iOS die [ `Label.LineHeight` ](xref:Xamarin.Forms.Label.LineHeight) und [ `Span.LineHeight` ](xref:Xamarin.Forms.Span.LineHeight) Eigenschaften Ändern der Zeilenhöhe von Text, der in einer einzelnen Zeile passt und Text, der auf mehrere Zeilen umbrochen wird.
> - Unter Android die [ `Label.LineHeight` ](xref:Xamarin.Forms.Label.LineHeight) und [ `Span.LineHeight` ](xref:Xamarin.Forms.Span.LineHeight) Eigenschaften nur ändern, die Zeilenhöhe des Texts, die auf mehrere Zeilen umbrochen wird.
> - Auf UWP die [ `Label.LineHeight` ](xref:Xamarin.Forms.Label.LineHeight) eigenschaftenänderungen die Zeilenhöhe des Texts, die auf mehrere Zeilen umbrochen wird und die [ `Span.LineHeight` ](xref:Xamarin.Forms.Span.LineHeight) Eigenschaft hat keine Auswirkungen.

Der folgende XAML-Beispiel veranschaulicht das Festlegen der [ `LineHeight` ](xref:Xamarin.Forms.Label.LineHeight) Eigenschaft für eine [ `Label` ](xref:Xamarin.Forms.Label):

```xaml
<Label Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus."
       LineBreakMode="WordWrap"
       LineHeight="1.8" />
```

Der entsprechende C#-Code ist:

```csharp
var label =
{
  Text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus.", LineBreakMode = LineBreakMode.WordWrap,
  LineHeight = 1.8
};
```

Die folgenden Screenshots zeigen das Ergebnis der Einstellung der [ `Label.LineHeight` ](xref:Xamarin.Forms.Label.LineHeight) Eigenschaft 1.8:

![](label-images/label-lineheight.png "LineHeight Beispielbezeichnung")

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

Der entsprechende C#-Code ist:

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

![](label-images/span-lineheight.png "LineHeight-Span-Beispiel")

## <a name="hyperlinks"></a>Hyperlinks

Der angezeigte Text [ `Label` ](xref:Xamarin.Forms.Label) und [ `Span` ](xref:Xamarin.Forms.Span) Instanzen in Hyperlinks mit dem folgenden Ansatz aktiviert werden können:

1. Legen Sie die `TextColor` und `TextDecoration` Eigenschaften der [ `Label` ](xref:Xamarin.Forms.Label) oder [ `Span` ](xref:Xamarin.Forms.Span).
1. Hinzufügen einer [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) auf die [ `GestureRecognizers` ](xref:Xamarin.Forms.GestureElement.GestureRecognizers) Auflistung von der [ `Label` ](xref:Xamarin.Forms.Label) oder [ `Span` ](xref:Xamarin.Forms.Span), dessen [ `Command` ](xref:Xamarin.Forms.TapGestureRecognizer.Command) Eigenschaft bindet an eine `ICommand`, und dessen [ `CommandParameter` ](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) Eigenschaft enthält die URL zu öffnen.
1. Definieren der `ICommand` , die ausgeführt werden wird, durch die [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer).
1. Schreiben Sie den Code, die von ausgeführt werden, die `ICommand`.

Im folgenden Codebeispiel wird, stammt aus der [Hyperlink-Demos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/HyperlinkDemos) Beispiel zeigt eine [ `Label` ](xref:Xamarin.Forms.Label) , dessen Inhalt festgelegt ist, aus mehreren [ `Span` ](xref:Xamarin.Forms.Span) Instanzen:

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

In diesem Beispiel, das erste und dritte [ `Span` ](xref:Xamarin.Forms.Span) Instanzen umfassen Text, während die zweite `Span` tappable Link darstellt. Es hat die Textfarbe, die auf Blau festgelegt und eine Unterstreichung Textdekoration. Dadurch wird die Darstellung eines Hyperlinks, erstellt, wie in den folgenden Screenshots gezeigt:

[![Hyperlinks](label-images/hyperlinks-small.png "links")](label-images/hyperlinks-large.png#lightbox)

Wenn der Link angetippt wird, die [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) antwortet durch Ausführen der `ICommand` von definiert die [ `Command` ](xref:Xamarin.Forms.TapGestureRecognizer.Command) Eigenschaft. Darüber hinaus die URL gemäß der [ `CommandParameter` ](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) Eigenschaft wird zum Übergeben der `ICommand` als Parameter.

Der Code-Behind für die XAML-Seite enthält die `TapCommand` Implementierung:

```csharp
public partial class MainPage : ContentPage
{
    public ICommand TapCommand => new Command<string>(OpenBrowser);

    public MainPage()
    {
        InitializeComponent();
        BindingContext = this;
    }

    void OpenBrowser(string url)
    {
        Device.OpenUri(new Uri(url));
    }
}
```

Die `TapCommand` führt die `OpenBrowser` Methode auf und übergibt die [ `TapGestureRecognizer.CommandParameter` ](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) Eigenschaftswert als Parameter. Diese Methode wiederum ruft die [ `Device.OpenUri` ](xref:Xamarin.Forms.Device.OpenUri*) Methode, um die URL in einem Webbrowser zu öffnen. Der Gesamteffekt aus diesem Grund ist, die der Link auf der Seite getippt wird, wird ein Webbrowser geöffnet und der URL, die dem Link zugeordnet ist navigiert.

### <a name="creating-a-reusable-hyperlink-class"></a>Erstellen einer wiederverwendbaren Hyperlink-Klasse

Der vorherige Ansatz zum Erstellen eines Links erfordert das Schreiben von sich wiederholenden Code jedes Mal, wenn Sie einen Link in Ihre Anwendung benötigen. Jedoch sowohl die [ `Label` ](xref:Xamarin.Forms.Label) und [ `Span` ](xref:Xamarin.Forms.Span) können Klassen zum Erstellen als Unterklasse definiert werden `HyperlinkLabel` und `HyperlinkSpan` Klassen, mit dem stiftbewegungs-Erkennung und der Text, die Formatierung von Code es hinzugefügt wird.

Im folgenden Codebeispiel wird, stammt aus der [Hyperlink Demos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/HyperlinkDemos) Beispiel zeigt eine `HyperlinkSpan` Klasse:

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
            Command = new Command(() => Device.OpenUri(new Uri(Url)))
        });
    }
}
```

Die `HyperlinkSpan` -Klasse definiert eine `Url` -Eigenschaft, und die zugehörigen [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty), und der Konstruktor legt die Link-Darstellung und das [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) antwortet, die Wenn der Link getippt wird. Wenn eine `HyperlinkSpan` getippt wird, die `TapGestureRecognizer` antwortet durch Ausführen der [ `Device.OpenUri` ](xref:Xamarin.Forms.Device.OpenUri*) Methode zum Öffnen der vom angegebenen URL, die `Url` Eigenschaft in einem Webbrowser.

Die `HyperlinkSpan` Klasse kann genutzt werden, indem Sie eine Instanz der Klasse die XAML hinzufügen:

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

- [Text (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Links (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Hyperlinks)
- [Erstellen von mobilen Apps mit Xamarin.Forms Kapitel 3](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [Label-API](xref:Xamarin.Forms.Label)
- [Span-API](xref:Xamarin.Forms.Span)
