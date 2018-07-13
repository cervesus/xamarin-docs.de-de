---
title: Formatieren von Xamarin.Forms-Zeichenfolgen
description: In diesem Artikel wird erläutert, wie Xamarin.FOrms datenbindungen, die zum Formatieren und Anzeigen von Objekten als Zeichenfolgen verwendet wird. Dies wird erreicht, indem der StringFormat der Bindung auf eine standardmäßige .NET Formatierungszeichenfolge mit Platzhalter.
ms.prod: xamarin
ms.assetid: 978C85B7-CB58-4483-A131-21B381A865E0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: a876e81c67b6ec61a2cb29143cb001a7d6160032
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998146"
---
# <a name="xamarinforms-string-formatting"></a>Formatieren von Xamarin.Forms-Zeichenfolgen

Manchmal ist es hilfreich, um datenbindungen zu verwenden, um die Zeichenfolgendarstellung eines Objekts oder den Wert anzuzeigen. Beispielsweise möchten Sie verwenden eine `Label` den aktuellen Wert der anzuzeigenden eine `Slider`. In dieser Datenbindung, die `Slider` ist die Quelle und das Ziel ist die `Text` Eigenschaft der `Label`.

Zum Anzeigen von Zeichenfolgen im Code ist die leistungsfähigste Tool die statische [ `String.Format` ](xref:System.String.Format(System.String,System.Object)) Methode. Die Formatierungszeichenfolge enthält Formatierungscodes für verschiedene Objekttypen spezifisch können, und Sie andere Text zusammen mit den Werten, der formatiert wird. Finden Sie unter den [Formatieren von Typen in .NET](/dotnet/standard/base-types/formatting-types/) Weitere Informationen zur zeichenfolgenformatierung.

## <a name="the-stringformat-property"></a>Der StringFormat-Eigenschaft

In datenbindungen, die diese Funktion übernommen wird: Festlegen der [ `StringFormat` ](xref:Xamarin.Forms.BindingBase.StringFormat) Eigenschaft `Binding` (oder die [ `StringFormat` ](xref:Xamarin.Forms.Xaml.BindingExtension.StringFormat) Eigenschaft der `Binding` Markuperweiterung) zu einer Standard .NET Formatierungszeichenfolge mit Platzhalter für ein:

```xaml
<Slider x:Name="slider" />
<Label Text="{Binding Source={x:Reference slider},
                      Path=Value,
                      StringFormat='The slider value is {0:F2}'}" />
```

Beachten Sie, dass die Formatierungszeichenfolge durch einfache Anführungszeichen (Apostroph) Zeichen den XAML-Parser vermeiden die geschweiften Klammern als ein anderes XAML-Markuperweiterung behandeln können getrennt wird. Andernfalls Zeichenfolge ohne das einfache Anführungszeichen die gleiche Zeichenfolge ist, Sie zum Anzeigen eines Gleitkommawerts in einem Aufruf von verwenden `String.Format`. Eine Formatierung Spezifikation von `F2` bewirkt, dass den Wert, der mit zwei Dezimalstellen angezeigt werden.

Die `StringFormat` Eigenschaft ist nur sinnvoll, wenn die Eigenschaft vom Typ `string`, und der Bindungsmodus `OneWay` oder `TwoWay`. Für bidirektionale Bindungen die `StringFormat` gilt nur für Werte, die aus der Quelle zum Ziel übergeben.

Wie Sie im nächsten Artikel sehen werden, auf die [Bindungspfad](binding-path.md), datenbindungen können sehr komplex und convoluted werden. Bei diesen datenbindungen zu debuggen, können Sie Hinzufügen einer `Label` in die XAML-Datei mit einer `StringFormat` einige fortgeschrittene Ergebnisse angezeigt werden sollen. Auch wenn Sie es nur zum Anzeigen des Objekttyps verwenden, kann die hilfreich sein.

Die **Zeichenfolgenformatierung** Seite veranschaulicht verschiedene Verwendungen von der `StringFormat` Eigenschaft:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
             x:Class="DataBindingDemos.StringFormattingPage"
             Title="String Formatting">

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>

            <Style TargetType="BoxView">
                <Setter Property="Color" Value="Blue" />
                <Setter Property="HeightRequest" Value="2" />
                <Setter Property="Margin" Value="0, 5" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10">
        <Slider x:Name="slider" />
        <Label Text="{Binding Source={x:Reference slider},
                              Path=Value,
                              StringFormat='The slider value is {0:F2}'}" />

        <BoxView />

        <TimePicker x:Name="timePicker" />
        <Label Text="{Binding Source={x:Reference timePicker},
                              Path=Time,
                              StringFormat='The TimeSpan is {0:c}'}" />

        <BoxView />

        <Entry x:Name="entry" />
        <Label Text="{Binding Source={x:Reference entry},
                              Path=Text,
                              StringFormat='The Entry text is &quot;{0}&quot;'}" />

        <BoxView />

        <StackLayout BindingContext="{x:Static sys:DateTime.Now}">
            <Label Text="{Binding}" />
            <Label Text="{Binding Path=Ticks,
                                  StringFormat='{0:N0} ticks since 1/1/1'}" />
            <Label Text="{Binding StringFormat='The {{0:MMMM}} specifier produces {0:MMMM}'}" />
            <Label Text="{Binding StringFormat='The long date is {0:D}'}" />
        </StackLayout>

        <BoxView />

        <StackLayout BindingContext="{x:Static sys:Math.PI}">
            <Label Text="{Binding}" />
            <Label Text="{Binding StringFormat='PI to 4 decimal points = {0:F4}'}" />
            <Label Text="{Binding StringFormat='PI in scientific notation = {0:E7}'}" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

Die Bindungen für die `Slider` und `TimePicker` veranschaulichen Sie die Verwendung der Formatangaben speziell für `double` und `TimeSpan` -Datentypen. Die `StringFormat` , anzeigt, dass den Text aus der `Entry` Ansicht veranschaulicht, wie in der Formatierungszeichenfolge, mit der Verwendung von doppelten Anführungszeichen an das `&quot;` HTML-Entität.

Im nächsten Abschnitt in der XAML-Datei werden eine `StackLayout` mit einer `BindingContext` legen Sie auf eine `x:Static` Markuperweiterung, die die statische Verweise `DateTime.Now` Eigenschaft. Die erste Bindung verfügt über keine Eigenschaften:

```xaml
<Label Text="{Binding}" />
```

Dies einfach zeigt die `DateTime` Wert der `BindingContext` mit standardformatierung. Die zweite Bindung zeigt die `Ticks` Eigenschaft `DateTime`, während die anderen beiden Bindungen anzeigen der `DateTime` selbst mit einem bestimmten Format. Beachten Sie, dass dies `StringFormat`:

```xaml
<Label Text="{Binding StringFormat='The {{0:MMMM}} specifier produces {0:MMMM}'}" />
```

Wenn Sie die linke oder rechte geschweifte Klammern in Ihrer Formatierungszeichenfolge anzeigen möchten, verwenden Sie einfach ein paar davon.

Im letzten Abschnitt wird die `BindingContext` auf den Wert der `Math.PI` und mit standardformatierung und zwei verschiedene Arten von Formatieren von numerischen Daten angezeigt.

Hier wird das Programm auf allen drei Plattformen ausgeführt wird:

[![Formatierung von Zeichenfolgen](string-formatting-images/stringformatting-small.png "Formatierung von Zeichenfolgen")](string-formatting-images/stringformatting-large.png#lightbox "Formatierung von Zeichenfolgen")

## <a name="viewmodels-and-string-formatting"></a>ViewModels und Formatieren von Zeichenfolgen

Bei der Verwendung von `Label` und `StringFormat` um den Wert einer Sicht anzuzeigen, die auch das Ziel eines "ViewModels" ist, können Sie entweder die Bindung aus der Ansicht definieren die `Label` oder über das "ViewModel", um die `Label`. Im Allgemeinen empfiehlt sich beim zweite Ansatz da überprüft wird, dass die Bindungen zwischen Ansicht und ViewModel arbeiten.

Dieser Ansatz wird angezeigt, der **besser Farbauswahl** das gleiche "ViewModel" als verwendet Beispiel die **einfache Farbauswahl** Programm in der [ **Binding Mode** ](binding-mode.md) Artikel:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.BetterColorSelectorPage"
             Title="Better Color Selector">

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Slider">
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>

            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout>
        <StackLayout.BindingContext>
            <local:HslColorViewModel Color="Sienna" />
        </StackLayout.BindingContext>

        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

        <StackLayout Margin="10, 0">
            <Label Text="{Binding Name}" />

            <Slider Value="{Binding Hue}" />
            <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}" />

            <Slider Value="{Binding Saturation}" />
            <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}" />

            <Slider Value="{Binding Luminosity}" />
            <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}" />
        </StackLayout>
    </StackLayout>
</ContentPage>    
```

Stehen nun drei Paare mit `Slider` und `Label` Elemente, die auf die gleiche gebunden sind source-Eigenschaft in der `HslColorViewModel` Objekt. Der einzige Unterschied besteht darin, die `Label` verfügt über eine `StringFormat` Eigenschaft, um jedes `Slider` Wert.

[![Besser Farbe Selektor](string-formatting-images/bettercolorselector-small.png "besser Farbe Selektor")](string-formatting-images/bettercolorselector-large.png#lightbox "besser Color-Auswahl")

Sie Fragen sich vielleicht wie RGB (Rot, Grün, Blau)-Werte im Hexadezimalformat für herkömmliche zweistellige angezeigt werden kann. Diese ganzzahligen Werte nicht direkt zur Verfügung, aus der `Color` Struktur. Eine Lösung wäre zum Berechnen von ganzzahligen Werten Farbkomponenten in "ViewModel", und sie als Eigenschaften verfügbar zu machen. Sie können anschließend formatieren Sie sie mithilfe der `X2` Spezifikation Formatierung.

Ein anderer Ansatz ist allgemeiner: können Sie schreiben eine *Bindung Wertkonverter* wie im Artikel weiter unten erläutert [ **Binden von Wertkonvertern**](converters.md).

Im nächste Artikel, jedoch untersucht die [ **Bindungspfad** ](binding-path.md) Weitere ausführliche Informationen, und zeigen, wie Sie es zum Verweisen auf untergeordnete Eigenschaften und Elemente in Auflistungen verwenden können.


## <a name="related-links"></a>Verwandte Links

- [Binden von Daten-Demos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Im Kapitel Daten-Bindung von Xamarin.Forms-Buch](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
