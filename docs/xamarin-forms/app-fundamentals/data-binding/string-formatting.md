---
title: Zeichenfolgenformatierung in Xamarin.Forms
description: In diesem Artikel wird erläutert, wie Sie Objekte mithilfe von Xamarin.Forms-Datenbindungen als Zeichenfolgen formatieren und anzeigen. Legen Sie das StringFormat der Bindung dazu auf eine .NET-Standard-Formatierungszeichenfolge mit einem Platzhalter fest.
ms.prod: xamarin
ms.assetid: 978C85B7-CB58-4483-A131-21B381A865E0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: ba7148ecabf7f534a953fda3c3d3021abeaa034c
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70771566"
---
# <a name="xamarinforms-string-formatting"></a>Zeichenfolgenformatierung in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)

Manchmal bietet es sich an, Objekte oder Werte mithilfe von Datenbindungen als Zeichenfolgen darzustellen. Sie können den aktuellen Wert eines `Slider` z. B. mithilfe eines `Label` anzeigen lassen. In dieser Datenbindung ist `Slider` die Quelle, und die `Text`-Eigenschaft von `Label` ist das Ziel.

Wenn es darum geht, Zeichenfolgen im Code anzuzeigen, ist die statische [`String.Format`](xref:System.String.Format(System.String,System.Object))-Methode das leistungsstärkste Tool. Die Formatierungszeichenfolge enthält Formatierungscodes, die nur von bestimmten Objekttypen verwendet werden, und Sie können abgesehen von den Werten, die formatiert werden, weiteren Text hineinschreiben. Weitere Informationen zum Formatieren von Zeichenfolgen finden Sie unter [Formatieren von Typen in .NET](/dotnet/standard/base-types/formatting-types/).

## <a name="the-stringformat-property"></a>Die StringFormat-Eigenschaft

Die Funktion wird in Datenbindungen übernommen: Sie legen die [`StringFormat`](xref:Xamarin.Forms.BindingBase.StringFormat)-Eigenschaft von `Binding` (oder die [`StringFormat`](xref:Xamarin.Forms.Xaml.BindingExtension.StringFormat)-Eigenschaft der `Binding`-Markuperweiterung) auf eine .NET-Standard-Formatierungszeichenfolge mit einem Platzhalter fest:

```xaml
<Slider x:Name="slider" />
<Label Text="{Binding Source={x:Reference slider},
                      Path=Value,
                      StringFormat='The slider value is {0:F2}'}" />
```

Beachten Sie, dass die Formatierungszeichenfolge in einfache Anführungszeichen (Apostroph) eingeschlossen ist. So wird vermieden, dass der XAML-Parser die geschweiften Klammern als eine weitere XAML-Markuperweiterung behandelt. Die Zeichenfolge ohne die einfachen Anführungszeichen ist dieselbe, mit der Sie einen Gleitkommawert in einem Aufruf von `String.Format` anzeigen lassen. Eine Formatierungsangabe von `F2` bewirkt, dass der Wert mit zwei Nachkommastellen dargestellt wird.

Die `StringFormat`-Eigenschaft ist nur sinnvoll, wenn die Zieleigenschaft vom Typ `string` ist und der Bindungsmodus `OneWay` oder `TwoWay` lautet. Für bidirektionale Bindungen ist `StringFormat` nur bei Werten anwendbar, die von der Quelle an das Ziel übergeben werden.

Wie Sie im nächsten Artikel zum [Bindungspfad](binding-path.md) erfahren werden, können Datenbindungen sehr komplex sein. Beim Debuggen dieser Datenbindungen können Sie in der XAML-Datei mit einem `StringFormat` ein `Label` hinzufügen, damit einige Zwischenergebnisse angezeigt werden. Selbst wenn Sie damit nur einen Objekttyp anzeigen lassen, kann es hilfreich sein.

Auf der Seite **String Formatting** (Zeichenfolgenformatierung) werden verschiedene Verwendungen der `StringFormat`-Eigenschaft veranschaulicht:

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

Die Bindungen für `Slider` und `TimePicker` zeigen, wie die für `double`- und `TimeSpan`-Datentypen spezifischen Formatangaben verwendet werden. Das `StringFormat`, das den Text aus der `Entry`-Ansicht anzeigt, veranschaulicht, wie doppelte Anführungszeichen in der Formatierungszeichenfolge mithilfe der HTML-Entität `&quot;` angegeben werden.

Im nächsten Abschnitt der XAML-Datei wird ein `StackLayout` mit einem `BindingContext` auf eine `x:Static`-Markuperweiterung festgelegt, die auf die statische `DateTime.Now`-Eigenschaft verweist. Die erste Bindung verfügt über keine Eigenschaften:

```xaml
<Label Text="{Binding}" />
```

Es wird einfach der `DateTime`-Wert von `BindingContext` mit der Standardformatierung angezeigt. Die zweite Bindung zeigt die `Ticks`-Eigenschaft von `DateTime` an, während die anderen beiden Bindungen `DateTime` selbst in einem bestimmten Format anzeigen. Sehen Sie sich dieses `StringFormat` an:

```xaml
<Label Text="{Binding StringFormat='The {{0:MMMM}} specifier produces {0:MMMM}'}" />
```

Wenn in Ihrer Formatierungszeichenfolge linke oder rechte geschweifte Klammern angezeigt werden sollen, verwenden Sie sie einfach zweimal.

Im letzten Abschnitt wird `BindingContext` auf den Wert von `Math.PI` festgelegt. Dieser wird dann in der Standardformatierung und zwei verschiedenen numerischen Formatierungstypen angezeigt.

Dies ist das Programm, das ausgeführt wird:

[![Zeichenfolgenformatierung](string-formatting-images/stringformatting-small.png "String Formatting")](string-formatting-images/stringformatting-large.png#lightbox "String Formatting")

## <a name="viewmodels-and-string-formatting"></a>ViewModels und die Zeichenfolgenformatierung

Wenn Sie die Werte einer Ansicht, die auch das Ziel eines ViewModels ist, mithilfe von `Label` und `StringFormat` anzeigen, können Sie entweder die Bindung zwischen Ansicht und `Label` oder zwischen ViewModel und `Label` definieren. Im Allgemeinen empfiehlt sich der zweite Ansatz, da hierbei überprüft wird, ob die Bindungen zwischen View und ViewModel funktionieren.

Dieser Ansatz wird im Beispiel **Better Color Selector** (Bessere Farbauswahl) vorgestellt. Darin wird dasselbe ViewModel wie im Programm **Simple Color Selector** (Einfache Farbauswahl) verwendet, das im Artikel [**Bindungsmodus in Xamarin.Forms**](binding-mode.md) erläutert wird:

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

Es gibt nun drei Paare von `Slider`- und `Label`-Elementen, die an dieselbe Quelleigenschaft im `HslColorViewModel`-Objekt gebunden sind. Der einzige Unterschied besteht darin, dass `Label` über eine `StringFormat`-Eigenschaft verfügt, mit der jeder `Slider`-Wert angezeigt werden kann.

[![Bessere Farbauswahl](string-formatting-images/bettercolorselector-small.png "Bessere Farbauswahl")](string-formatting-images/bettercolorselector-large.png#lightbox "Bessere Farbauswahl")

Sie fragen sich vielleicht, wie Sie RGB-Werte (Rot, Grün, Blau) im herkömmlichen zweistelligen Hexadezimalformat darstellen können. Diese Integerwerte sind in der `Color`-Struktur nicht direkt verfügbar. Eine Lösung wäre eine Berechnung der Integerwerte der Farbkomponenten im ViewModel. Diese lassen sich anschließend als Eigenschaften verfügbar machen. Anschließend können Sie sie mithilfe der Formatierungsangabe `X2` formatieren.

Es gibt einen weiteren, allgemeineren Ansatz: Sie können *Wertkonverter für Bindungen* schreiben. Diese lernen Sie später im Artikel [**Binden von Wertkonvertern**](converters.md) kennen.

Im nächste Artikel erfahren Sie jedoch zunächst mehr über den [**Bindungspfad**](binding-path.md) und darüber, wie Sie damit auf untergeordnete Eigenschaften und Elemente in Sammlungen verweisen.

## <a name="related-links"></a>Verwandte Links

- [Data Binding Demos (Demos zur Datenbindung (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)
- [Kapitel zu Datenbindung aus dem Xamarin.Forms-Buch](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
