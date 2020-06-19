---
title: 'title: "Xamarin.Forms – Bindungspfad" description: "In diesem Artikel wird erläutert, wie über Xamarin.Forms-Datenbindungen mit der „Path“-Eigenschaft der Klasse „Binding“ auf untergeordnete Eigenschaften und Sammlungsmember zugegriffen werden kann."'
description: 'ms.prod: xamarin ms.assetid: 3CF721A5-E157-468B-AD3A-DA0A45E58E8D ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 01/05/2018 no-loc: [Xamarin.Forms, Xamarin.Essentials]'
ms.prod: xamarin
ms.assetid: 3CF721A5-E157-468B-AD3A-DA0A45E58E8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a0ac8a568c3e8c46fa7e53112461aa0bff5684ae
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84570790"
---
# <a name="xamarinforms-binding-path"></a>Xamarin.Forms-Bindungspfad

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)

In allen vorherigen Datenbindungsbeispielen wurde die [`Path`](xref:Xamarin.Forms.Binding.Path)-Eigenschaft der `Binding`-Klasse (bzw. die [`Path`](xref:Xamarin.Forms.Xaml.BindingExtension.Path)-Eigenschaft der `Binding`-Markuperweiterung) auf eine einzelne Eigenschaft festgelegt. Es ist tatsächlich möglich, `Path` auf eine *untergeordnete Eigenschaft* (die Eigenschaft einer Eigenschaft) oder auf einen Collectionmember festzulegen.

Angenommen beispielsweise, Ihre Seite enthält einen `TimePicker`-Wert:

```xaml
<TimePicker x:Name="timePicker">
```

Die `Time`-Eigenschaft von `TimePicker` ist vom Typ `TimeSpan`. Vielleicht möchten Sie aber eine Datenbindung erstellen, die auf die `TotalSeconds`-Eigenschaft dieses `TimeSpan`-Werts verweist. So sieht die Datenbindung aus:

```xaml
{Binding Source={x:Reference timePicker},
         Path=Time.TotalSeconds}
```

Die Eigenschaft `Time` ist vom Typ `TimeSpan`, der eine Eigenschaft vom Typ `TotalSeconds` aufweist. Die Eigenschaften `Time` und `TotalSeconds` sind einfach durch einen Punkt miteinander verbunden. Die Elemente in der `Path`-Zeichenfolge verweisen immer auf Eigenschaften und nicht auf die Typen dieser Eigenschaften.

Dieses Beispiel wird zusammen mit einigen anderen Beispielen auf der Seite **Path Variations** (Pfadvariationen) angezeigt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:globe="clr-namespace:System.Globalization;assembly=netstandard"
             x:Class="DataBindingDemos.PathVariationsPage"
             Title="Path Variations"
             x:Name="page">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="HorizontalTextAlignment" Value="Center" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10, 0">
        <TimePicker x:Name="timePicker" />

        <Label Text="{Binding Source={x:Reference timePicker},
                              Path=Time.TotalSeconds,
                              StringFormat='{0} total seconds'}" />

        <Label Text="{Binding Source={x:Reference page},
                              Path=Content.Children.Count,
                              StringFormat='There are {0} children in this StackLayout'}" />

        <Label Text="{Binding Source={x:Static globe:CultureInfo.CurrentCulture},
                              Path=DateTimeFormat.DayNames[3],
                              StringFormat='The middle day of the week is {0}'}" />

        <Label>
            <Label.Text>
                <Binding Path="DateTimeFormat.DayNames[3]"
                         StringFormat="The middle day of the week in France is {0}">
                    <Binding.Source>
                        <globe:CultureInfo>
                            <x:Arguments>
                                <x:String>fr-FR</x:String>
                            </x:Arguments>
                        </globe:CultureInfo>
                    </Binding.Source>
                </Binding>
            </Label.Text>
        </Label>

        <Label Text="{Binding Source={x:Reference page},
                              Path=Content.Children[1].Text.Length,
                              StringFormat='The second Label has {0} characters'}" />
    </StackLayout>
</ContentPage>
```

Im zweiten `Label`-Element stellt die Seite selbst die Bindungsquelle dar. Die Eigenschaft `Content` ist vom Typ `StackLayout`. Ihr ist eine `Children` Eigenschaft vom Typ `IList<View>` zugeordnet, die eine `Count`-Eigenschaft aufweist. Diese gibt die Anzahl der untergeordneten Eigenschaften an.

## <a name="paths-with-indexers"></a>Pfade mit Indexer

Die Bindung im dritten `Label`-Element auf der Seite **Path Variations** (Pfadvariationen) verweist auf die [`CultureInfo`](xref:System.Globalization.CultureInfo)-Klasse im Namespace `System.Globalization`:

```xaml
<Label Text="{Binding Source={x:Static globe:CultureInfo.CurrentCulture},
                      Path=DateTimeFormat.DayNames[3],
                      StringFormat='The middle day of the week is {0}'}" />
```

Die Quelle ist auf die statische `CultureInfo.CurrentCulture`-Eigenschaft festgelegt, bei der es sich um ein Objekt vom Typ `CultureInfo` handelt. Diese Klasse definiert eine Eigenschaft mit dem Namen `DateTimeFormat` vom Typ [`DateTimeFormatInfo`](xref:System.Globalization.DateTimeFormatInfo), die eine `DayNames`-Collection enthält. Der Index wählt das vierte Element aus.

Das vierte `Label`-Element geht ähnlich vor, jedoch für die mit Frankreich assoziierte Kultur. Die Eigenschaft `Source` der Bindung wird mit einem Konstruktor auf das Objekt `CultureInfo` festgelegt:

```xaml
<Label>
    <Label.Text>
        <Binding Path="DateTimeFormat.DayNames[3]"
                 StringFormat="The middle day of the week in France is {0}">
            <Binding.Source>
                <globe:CultureInfo>
                    <x:Arguments>
                        <x:String>fr-FR</x:String>
                    </x:Arguments>
                </globe:CultureInfo>
            </Binding.Source>
        </Binding>
    </Label.Text>
</Label>
```

Weitere Einzelheiten zur Angabe von Konstruktorargumenten in XAML finden Sie unter [Übergeben von Konstruktorargumenten](~/xamarin-forms/xaml/passing-arguments.md#passing-constructor-arguments).

Das letzte Beispiel ist schließlich mit dem zweiten vergleichbar. Der einzige Unterschied ist, dass auf eines der untergeordneten Elemente von `StackLayout` verwiesen wird:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content.Children[1].Text.Length,
                      StringFormat='The first Label has {0} characters'}" />
```

Bei diesem untergeordneten Element handelt es sich um ein `Label`-Element, das die Eigenschaft `Text` vom Typ `String` aufweist, der die Eigenschaft `Length` zugeordnet ist. Das erste `Label`-Element meldet die in `TimePicker` festgelegte `TimeSpan`. Wenn dieser Text geändert wird, ändert sich folglich auch das endgültige `Label`-Element.

Dies ist das Programm, das ausgeführt wird:

[![Pfadabweichungen](binding-path-images/pathvariations-small.png "Pfadabweichungen")](binding-path-images/pathvariations-large.png#lightbox "Pfadabweichungen")

## <a name="debugging-complex-paths"></a>Debuggen komplexer Pfade

Die Erstellung komplexer Pfaddefinitionen kann sich als schwierig erweisen: Sie müssen den Typ der einzelnen untergeordneten Eigenschaften oder den Typ der Elemente in der Collection kennen, um die nächste untergeordnete Eigenschaft ordnungsgemäß hinzufügen zu können. Allerdings werden die Typen selbst nicht im Pfad angezeigt. Eine gute Methode besteht darin, den Pfad inkrementell zu erstellen und sich die Zwischenergebnisse anzusehen. Im letzten Beispiel konnten Sie starten, ohne dass für `Path` eine Definition vorhanden war:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      StringFormat='{0}'}" />
```

Hier wird der Typ der Bindungsquelle oder `DataBindingDemos.PathVariationsPage` angezeigt. Sie wissen, dass das `PathVariationsPage`-Element von `ContentPage` abgeleitet wird. Folglich verfügt es über eine `Content`-Eigenschaft:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content,
                      StringFormat='{0}'}" />
```

Als Typ der `Content`-Eigenschaft wird jetzt `Xamarin.Forms.StackLayout` angezeigt. Wenn Sie die Eigenschaft `Children` zur Eigenschaft `Path` hinzufügen, lautet der Typ `Xamarin.Forms.ElementCollection'1[Xamarin.Forms.View]`. Dies ist eine interne Klasse von Xamarin.Forms, aber natürlich auch ein Sammlungstyp. Wenn Sie einen Index hinzufügen, lautet der Typ `Xamarin.Forms.Label`. Fahren Sie auf diese Weise fort.

Während Xamarin.Forms den Bindungspfad verarbeitet, wird für jedes Objekt im Pfad ein `PropertyChanged`-Handler installiert, der die Schnittstelle `INotifyPropertyChanged` implementiert. Beispiel: Die endgültige Bindung reagiert auf eine Änderung im ersten `Label`-Element, da sich die Eigenschaft `Text` ändert.

Wenn eine Eigenschaft im Bindungspfad `INotifyPropertyChanged` nicht implementiert, werden sämtliche an dieser Eigenschaft vorgenommenen Änderungen ignoriert. Durch einige Änderungen könnte der Bindungspfad vollständig ungültig gemacht werden. Daher sollten Sie dieses Verfahren nur dann anwenden, wenn die Zeichenfolge der Eigenschaften und untergeordneten Eigenschaften niemals ungültig wird.

## <a name="related-links"></a>Verwandte Links

- [Data Binding Demos (Demos zur Datenbindung (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)
- [Kapitel zu Datenbindung aus dem Xamarin.Forms-Buch](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
