---
title: Formatierung von Zeichenfolgen
description: Verwenden Sie zum Formatieren und Anzeigen von Objekten als Zeichenfolgen datenbindungen
ms.prod: xamarin
ms.assetid: 978C85B7-CB58-4483-A131-21B381A865E0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 4e143f650c3cde7577def1a95e53b207608a088a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="string-formatting"></a>Formatierung von Zeichenfolgen

Manchmal ist es praktisch, datenbindungen zu verwenden, um die Zeichenfolgendarstellung eines Objekts oder Wert anzuzeigen. Beispielsweise möchten Sie verwenden eine `Label` den aktuellen Wert der anzuzeigenden eine `Slider`. In dieser Datenbindung der `Slider` ist die Quelle und das Ziel ist die `Text` Eigenschaft von der `Label`.

Zum Anzeigen von Zeichenfolgen im Code ist sehr leistungsstarke Tool die statische [ `String.Format` ](https://developer.xamarin.com/api/member/System.String.Format/p/System.String/System.Object/) Methode. Die Formatierungszeichenfolge enthält Formatierungscodes speziell für verschiedene Typen von Objekten, und Sie können Werte zusammen mit den zu formatierenden Text enthalten. Finden Sie unter der [Formatierung von Typen in .NET](/dotnet/standard/base-types/formatting-types/) Artikel finden Sie weitere Informationen zum Formatieren der Zeichenfolge.

## <a name="the-stringformat-property"></a>StringFormat-Eigenschaft

Diese Funktion ist in datenbindungen übernommen: Festlegen der [ `StringFormat` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.StringFormat/) Eigenschaft `Binding` (oder die [ `StringFormat` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.StringFormat/) Eigenschaft von der `Binding` Markuperweiterung) auf eine Standard .NET Formatierungszeichenfolge mit Platzhalter für ein:

```xaml
<Slider x:Name="slider" />
<Label Text="{Binding Source={x:Reference slider},
                      Path=Value,
                      StringFormat='The slider value is {0:F2}'}" />
```

Beachten Sie, dass die Formatierungszeichenfolge durch einfache Anführungszeichen (Apostroph)-Zeichen zu vermeiden, behandeln die geschweiften Klammern als eine andere Verwendung von XAML-Markuperweiterung XAML-Parser getrennt wird. Andernfalls Zeichenfolge ohne die einfache Anführungszeichen die gleiche Zeichenfolge ist, Sie zum Anzeigen eines Gleitkommawerts in einem Aufruf von verwenden `String.Format`. Eine Formatierung Spezifikation der `F2` bewirkt, dass der Wert mit zwei Dezimalstellen angezeigt werden soll.

Die `StringFormat` Eigenschaft ist nur sinnvoll, wenn die Zieleigenschaft des Typs ist `string`, und der Bindungsmodus `OneWay` oder `TwoWay`. Für bidirektionale Bindungen die `StringFormat` gilt nur für Werte, die aus der Quelle an das Ziel übergeben.

Wie Sie in der nächsten Artikel auf sehen die [Bindungspfad](binding-path.md), datenbindungen können ziemlich komplex und kompliziert werden. Wenn diese datenbindungen zu debuggen, können Sie hinzufügen eine `Label` in der XAML-Datei mit einer `StringFormat` einige Zwischenergebnisse angezeigt. Auch wenn Sie es mit der nur ein Objekttyp angezeigt, die hilfreich sein können.

Die **Zeichenfolgenformatierung** Seite veranschaulicht verschiedene Verwendungsmöglichkeiten für die `StringFormat` Eigenschaft:

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

Die Bindungen auf den `Slider` und `TimePicker` Anzeigen der speziell für die Verwendung von Formatangaben `double` und `TimeSpan` -Datentypen. Die `StringFormat` , anzeigt, dass den Text aus der `Entry` Ansicht zeigt, wie doppelte Anführungszeichen in die Formatierungszeichenfolge mit der Verwendung von der `&quot;` HTML-Entität.

Im nächste Abschnitt in der XAML-Datei ist eine `StackLayout` mit einer `BindingContext` legen Sie auf eine `x:Static` Markuperweiterung, die die statische Verweise `DateTime.Now` Eigenschaft. Die erste Bindung verfügt über keine Eigenschaften:

```xaml
<Label Text="{Binding}" />
```

Dies einfach zeigt die `DateTime` Wert, der die `BindingContext` mit standardformatierung. Die zweite Bindung zeigt die `Ticks` Eigenschaft `DateTime`, während die anderen beiden Bindungen anzeigen der `DateTime` selbst mit einem bestimmten Format. Beachten Sie dies `StringFormat`:

```xaml
<Label Text="{Binding StringFormat='The {{0:MMMM}} specifier produces {0:MMMM}'}" />
```

Wenn Sie nach links oder rechts geschweifte Klammern in der Formatzeichenfolge angezeigt, verwenden Sie einfach ein Paar von ihnen.

Im letzten Abschnitt wird die `BindingContext` auf den Wert des `Math.PI` und mit standardformatierung und zwei verschiedene Arten von Formatieren von numerischen Daten angezeigt.

Hier wird das Programm auf allen drei Plattformen ausgeführt:

[![Formatieren von Zeichenfolgen](string-formatting-images/stringformatting-small.png "Formatierung eine Zeichenfolge")](string-formatting-images/stringformatting-large.png#lightbox "Formatierung eine Zeichenfolge")

## <a name="viewmodels-and-string-formatting"></a>ViewModels und Formatierung von Zeichenfolgen

Bei Verwendung `Label` und `StringFormat` um den Wert einer Sicht anzuzeigen, die auch das Ziel ein ViewModel ist, können Sie entweder die Bindung aus der Ansicht definieren die `Label` oder über das ViewModel auf die `Label`. Der zweite Ansatz wird generell empfohlen, da überprüft wird, dass die Bindungen zwischen der Ansicht und ViewModel funktionieren.

Dieser Ansatz wird angezeigt, der **besser Farbauswahl** Beispiel, das im gleichen ViewModel als verwendet die **einfache Farbauswahl** -Programm in der [ **binden Modus** ](binding-mode.md) Artikel:

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

Stehen nun drei Paare von `Slider` und `Label` Elemente, die auf den gleichen gebunden sind source-Eigenschaft in der `HslColorViewModel` Objekt. Der einzige Unterschied besteht darin, die `Label` verfügt über eine `StringFormat` anzuzeigende jeder Eigenschaft `Slider` Wert.

[![Farbe besser Selektor](string-formatting-images/bettercolorselector-small.png "besser Farbe Selektor")](string-formatting-images/bettercolorselector-large.png#lightbox "besser Farbe Selektor")

Sie Fragen sich vielleicht wie RGB (Rot, Grün, Blau)-Werte im herkömmlichen zweistellige Hexadezimalformat angezeigt werden konnte. Diese ganzzahligen Werte stehen nicht zur Verfügung direkt aus der `Color` Struktur. Eine Lösung bestünde darin ganzzahlige Werte der Farbe Komponenten innerhalb der ViewModel berechnen und diese als Eigenschaften verfügbar gemacht. Kann anschließend formatieren Sie sie mithilfe der `X2` Spezifikation Formatierung.

Ein anderer Ansatz arbeitet universeller: können Sie schreiben eine *Bindung Wertkonverter* wie in diesem Artikel weiter unten erläutert [ **Bindung Wertkonverter**](converters.md).

Das nächste Dokument, jedoch untersucht die [ **Bindungspfad** ](binding-path.md) in mehr Details, und zeigen, wie Sie verweisen auf untergeordnete Eigenschaften und Elemente in Auflistungen verwenden.


## <a name="related-links"></a>Verwandte Links

- [Binden von Daten-Demos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Daten Bindung Kapitel Xamarin.Forms Buchs](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
