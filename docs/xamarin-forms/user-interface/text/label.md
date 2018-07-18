---
title: Xamarin.Forms-Bezeichnung
description: In diesem Artikel wird erläutert, wie Xamarin.Forms Label-Klasse zu verwenden, um einzelne und mehrzeiligen Text in Anwendungen anzuzeigen.
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: ce602a84ea1024dc22298a3ec1567a9a34ad4a82
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995965"
---
# <a name="xamarinforms-label"></a>Xamarin.Forms-Bezeichnung

_Anzeigen von Text in Xamarin.Forms_

Die `Label` Ansicht dient zum Anzeigen von Text, sowohl einzelne als auch mit mehreren Zeilen. Bezeichnungen können benutzerdefinierte Schriftarten (Familien, Größen und Optionen) und farbige Text sein. In diesem Artikel werden die folgenden Themen behandelt:

- **[Abschneiden und-Umbruch](#Truncation_and_Wrapping)**  &ndash; Abschneiden sowie das Umbrechen von Optionen für die Behandlung von Situationen, in denen Text nicht in einer Zeile passt.
- **[Schriftart](#Font)**  &ndash; Schriftartoptionen.
- **[Farbe](#Color)**  &ndash; Farboptionen für Bezeichnungstext.
- **[Von formatiertem Text](#Formatted_Text)**  &ndash; Optionen zum Anzeigen von Text mit mehreren Formaten/Stile Inline.

## <a name="styling-label"></a>Stil-Bezeichnung

Die folgenden Abschnitte enthalten wie die Eigenschaften von `Label` manuell auf einer Basis pro Instanz. Beachten Sie, dass der Eigenschaften kann in einem Stil gruppiert werden, die mit einem oder mehreren Ansichten, konsistent angewendet wird. Dies kann die Lesbarkeit des Codes zu erhöhen und stellen Änderungen des Entwurfs einfacher zu implementieren. Finden Sie unter [Stile](~/xamarin-forms/user-interface/text/styles.md) für Weitere Informationen.

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>Abschneiden und-Umbruch

Bezeichnungen können festgelegt werden, um Text zu behandeln, die nicht in einer Zeile in eine von mehreren Möglichkeiten, die verfügbar gemacht werden, indem Sie passen die `LineBreakMode` Eigenschaft. [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode) ist eine Enumeration der folgenden Optionen:

- **HeadTruncation** &ndash; schneidet den Anfang der Text am Ende angezeigt.
- **CharacterWrap** &ndash; Text auf einer neuen Zeile an einer Zeichengrenze umschließt.
- **MiddleTruncation** &ndash; zeigt an, die Anfang und Ende des Texts, mit der mittleren ersetzen Sie dies durch ein Auslassungszeichen.
- **NoWrap** &ndash; wird die Anzeige von nur-Text nicht umbrochen so viel Text kann in einer Zeile passen.
- **TailTruncation** &ndash; zeigt den Anfang des Texts, Ende abgeschnitten.
- **WordWrap** &ndash; Zeilenumbruch an der Wortgrenze.

## <a name="font"></a>Schriftart

Finden Sie unter [arbeiten mit Schriftarten](~/xamarin-forms/user-interface/text/fonts.md) für Weitere Informationen.

## <a name="color"></a>Farbe

`Label`s kann festgelegt werden, verwenden Sie eine benutzerdefinierte Farbe, die über die bindbare `TextColor` Eigenschaft.

Besondere Sorgfalt ist erforderlich, um sicherzustellen, dass Farben auf jeder Plattform verwendet werden können. Da jede Plattform unterschiedliche Standardwerte für Text- und Hintergrundfarben verfügt, müssen Sie darauf achten, einen Standardwert auswählen, der auf den einzelnen funktioniert.

Verwenden Sie den folgenden Code, um die Textfarbe einer Beschriftung festzulegen:

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

Bezeichnungen verfügbar machen eine `FormattedText` Eigenschaft, die Ihnen die Möglichkeit zur Darstellung von Text mit mehreren Schriftarten und Farben in der gleichen Ansicht.

Die `FormattedText` Eigenschaft ist vom Typ [ `FormattedString` ](xref:Xamarin.Forms.FormattedString). Formatierte Zeichenfolgen bestehen aus einem oder mehreren `Span`s, die jeweils mit den folgenden Eigenschaften:

- **BackgroundColor** &ndash; dienen kann, legen Sie eine Hintergrundfarbe, z. B. um einen Textmarker-Effekt zu erzielen.
- **FontAttributes** &ndash; auf Fett, kursiv oder keines von beiden werden können.
- **FontFamily** &ndash; wird die Schriftart verwendet werden.
- **FontSize** &ndash; legt die Größe des Texts fest.
- **ForegroundColor** &ndash; legt die Farbe des Texts fest.
- **Text** &ndash; der Text, der angezeigt werden.

Der folgende C#-Code wird eine Bezeichnung, in dem das erste Wort fett formatiert ist und das letzte Wort Rot, veranschaulicht:

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

- [Erstellen von mobilen Apps mit Xamarin.Forms Kapitel 3](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [Text (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Label-API](xref:Xamarin.Forms.Label)
