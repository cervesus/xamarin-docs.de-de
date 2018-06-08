---
title: Bezeichnung
description: Anzeigen von Text in Xamarin.Forms
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: e0caa12136feb84d22ec4b90b84f3f92a601e0c0
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847510"
---
# <a name="label"></a>Bezeichnung

_Anzeigen von Text in Xamarin.Forms_

Die `Label` Ansicht dient zur Anzeige von Text, einzelne und mehrere Zeilen. Bezeichnungen können benutzerdefinierte Schriftarten (Familien, Größen und Optionen) und der farbige Text sein. In diesem Artikel werden die folgenden Themen behandelt:

- **[Abschneiden und Wrapping](#Truncation_and_Wrapping)**  &ndash; Abschneiden sowie das Umbrechen von Optionen für die Behandlung von Situationen, in denen Text nicht in einer Zeile passt.
- **[Schriftart](#Font)**  &ndash; Schriftartoptionen.
- **[Farbe](#Color)**  &ndash; Bezeichnungstext Farboptionen.
- **[Formatierter Text](#Formatted_Text)**  &ndash; Optionen für die Anzeige von Text mit mehreren Formaten/Stile Inline.

## <a name="styling-label"></a>Formatieren Bezeichnung

Die folgenden Abschnitte enthalten, wie die Eigenschaften von `Label` auf eine instanzweise manuell. Beachten Sie, die Sätze von Eigenschaften kann in einen Stilspezifikation gruppiert werden, die mit einem oder mehreren Ansichten konsistent angewendet wird. Dadurch kann die Lesbarkeit des Codes zu erhöhen und Designänderungen einfacher zu implementieren. Finden Sie unter [Stile](~/xamarin-forms/user-interface/text/styles.md) für Weitere Informationen.

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>Abschneiden und Wrapping

Bezeichnungen können festgelegt werden, um Text zu behandeln, die nicht in einer Zeile in eine von mehreren Möglichkeiten, die verfügbar gemacht werden, indem Sie passen die `LineBreakMode` Eigenschaft. [`LineBreakMode`](https://developer.xamarin.com/api/type/Xamarin.Forms.LineBreakMode/) ist eine Enumeration der folgenden Optionen:

- **HeadTruncation** &ndash; schneidet den Anfang der Text am Ende angezeigt.
- **CharacterWrap** &ndash; Text auf einer neuen Zeile an einer Zeichengrenze umschließt.
- **MiddleTruncation** &ndash; Anfang und Ende des Texts, mit der mittleren ersetzen durch Ellipsen angezeigt.
- **NoWrap** &ndash; Anzeigen von nur-Text wird nicht umbrochen so viel Text als kann in einer Zeile passen.
- **TailTruncation** &ndash; zeigt den Anfang des Texts, abschneiden Ende.
- **WordWrap** &ndash; Text an der Wortgrenze umschließt.

## <a name="font"></a>Schriftart

Finden Sie unter [arbeiten mit Schriftarten](~/xamarin-forms/user-interface/text/fonts.md) für Weitere Informationen.

## <a name="color"></a>Farbe

`Label`s festgelegt werden kann, um eine benutzerdefinierte Textfarbe über die bindungsfähigen verwenden `TextColor` Eigenschaft.

Besondere Sorgfalt ist erforderlich, um sicherzustellen, dass Farben auf jeder Plattform verwendet werden kann können. Da jede Plattform unterschiedliche Standardwerte für Text- und Hintergrundfarben aufweist, müssen Sie achten Sie darauf, dass Sie den Standardwert auswählen, der auf jedem funktioniert.

Verwenden Sie den folgenden Code, um die Textfarbe einer Bezeichnung festzulegen:

In Code:

```csharp
public partial class LabelPage : ContentPage
{
    public LabelPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        var label = new Label { Text="This is a label.", TextColor = Color.FromHex("#77d065"), FontSize = 20 };
        layout.Children.Add(label);
    }
}
```

In XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.LabelPage"
Title="Label Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
      <Label TextColor="#77d065" FontSize = "20" Text="This is a label." />
    </StackLayout>
  </ContentPage.Content>
</ContentPage>
```

![](label-images/textcolor.png "Bezeichnung TextColor-Beispiel")

<a name="Formatted_Text" />

## <a name="formatted-text"></a>Formatierter Text

Bezeichnungen verfügbar machen eine `FormattedText` Eigenschaft die bietet die Möglichkeit zum Darstellen von Text mit mehreren Schriftarten und Farben in derselben Ansicht.

Die `FormattedText` -Eigenschaft ist vom Typ [ `FormattedString` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FormattedString/). Formatierte Zeichenfolgen bestehen aus einem oder mehreren `Span`s, die jeweils mit den folgenden Eigenschaften:

- **BackgroundColor** &ndash; kann verwendet werden, um eine Hintergrundfarbe, z. B. um eine Hervorhebung Effekt zu erzielen festzulegen.
- **FontAttributes** &ndash; Satz fett, kursiv oder keines von beiden werden können.
- **FontFamily** &ndash; wird die Schriftart, die verwendet werden.
- **FontSize** &ndash; legt die Größe des Texts fest.
- **Foreground Color** &ndash; legt die Farbe des Texts fest.
- **Text** &ndash; den Text dargestellt werden soll.

Der folgende C#-Code zeigt eine Bezeichnung, in dem das erste Wort fett formatiert ist und das letzte Wort ist rot:

```csharp
public partial class LabelPage : ContentPage
{
    public LabelPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
    var label = new Label { FontSize = 20 };
    var s = new FormattedString ();
    s.Spans.Add (new Span{ Text = "Red Bold", FontAttributes = FontAttributes.Bold });
    s.Spans.Add (new Span{ Text = "Default" });
    s.Spans.Add (new Span{ Text = "italic small", FontSize =  Device.GetNamedSize(NamedSize.Small, typeof(Label)), FontAttributes = FontAttributes.Italic});
    label.FormattedText = s;
        layout.Children.Add(label);
    }
}
```

In XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.LabelPage"
Title="Label Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
      <Label FontSize=20>
        <Label.FormattedText>
          <FormattedString>
            <Span Text="Red Bold" ForegroundColor="Red" FontAttributes="Bold" />
            <Span Text="Default" />
            <Span Text="italic small" FontAttributes="Italic" FontSize="Small" />
          </FormattedString>
        </Label.FormattedText>
      </Label>
    </StackLayout>
  </ContentPage.Content>
</ContentPage>
```

![](label-images/formattedtext.png "Bezeichnung FormattedText-Beispiel")


## <a name="related-links"></a>Verwandte Links

- [Beim Erstellen mobiler Apps mit Xamarin.Forms, Kapitel 3](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [Text (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Label-API](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)
