---
title: Xamarin.Forms Bindungspfad
description: In diesem Artikel wird das Xamarin.Forms-datenbindungen zu verwenden, um den Zugriff auf untergeordnete Eigenschaften und Elemente der Auflistung mit dem Path-Eigenschaft der Bindungsklasse erläutert.
ms.prod: xamarin
ms.assetid: 3CF721A5-E157-468B-AD3A-DA0A45E58E8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: d7c3b1ba991380451b4a82c389c4d46e950bc914
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240472"
---
# <a name="xamarinforms-binding-path"></a>Xamarin.Forms Bindungspfad

In allen vorherigen Datenbindungsfunktionen Beispielen die [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) Eigenschaft von der `Binding` Klasse (oder die [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.Path/) Eigenschaft von der `Binding` Markuperweiterung) festgelegt wurde Um eine einzelne Eigenschaft. Es ist möglich, legen Sie `Path` auf eine *untergeordnete Eigenschaft* (eine Eigenschaft einer Eigenschaft), oder auf einen Member einer Auflistung.

Nehmen wir beispielsweise an Ihre Seite enthält eine `TimePicker`:

```xaml
<TimePicker x:Name="timePicker">
```

Die `Time` Eigenschaft `TimePicker` ist vom Typ `TimeSpan`, aber vielleicht möchten eine Datenbindung erstellen, verweist der `TotalSeconds` -Eigenschaft dieses `TimeSpan` Wert. So sieht die Datenbindung aus:

```xaml
{Binding Source={x:Reference timePicker},
         Path=Time.TotalSeconds}
```

Die `Time` -Eigenschaft ist vom Typ `TimeSpan`, verfügt über eine `TotalSeconds` Eigenschaft. Die `Time` und `TotalSeconds` Eigenschaften sind einfach verbundenen mit einem Punkt. Die Elemente in der `Path` Zeichenfolge verweist immer auf Eigenschaften und nicht auf die Typen dieser Eigenschaften.

Beispiel und einigen weiteren Bildschirmen, in angezeigt werden der **Pfad Variationen** Seite:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:globe="clr-namespace:System.Globalization;assembly=mscorlib"
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
                              StringFormat='The first Label has {0} characters'}" />
    </StackLayout>
</ContentPage>
```

In der zweiten `Label`, Bindungsquelle wird die Seite selbst. Die `Content` -Eigenschaft ist vom Typ `StackLayout`, verfügt über eine `Children` Eigenschaft vom Typ `IList<View>`, verfügt über eine `Count` , der angibt, der Anzahl der untergeordneten Eigenschaft.

## <a name="paths-with-indexers"></a>Pfade mit Indexer

Die Bindung im dritten `Label` in der **Pfad Variationen** Seiten Verweise der [ `CultureInfo` ](https://developer.xamarin.com/api/type/System.Globalization.CultureInfo/) -Klasse in der `System.Globalization` Namespace:

```xaml
<Label Text="{Binding Source={x:Static globe:CultureInfo.CurrentCulture},
                      Path=DateTimeFormat.DayNames[3],
                      StringFormat='The middle day of the week is {0}'}" />
```

Die Quelle wird festgelegt, der statischen `CultureInfo.CurrentCulture` Eigenschaft, die ein Objekt des Typs `CultureInfo`. Klasse eine Eigenschaft mit dem Namen definiert `DateTimeFormat` des Typs [ `DateTimeFormatInfo` ](https://developer.xamarin.com/api/type/System.Globalization.DateTimeFormatInfo/) , enthält eine `DayNames` Auflistung. Der Index wird das vierte Element ausgewählt.

Das vierte `Label` etwas Ähnliches aber für die Kultur Frankreich zugeordnet ist. Die `Source` -Eigenschaft der Bindung festgelegt ist, um `CultureInfo` Objekt mit einem Konstruktor:

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

Finden Sie unter [Konstruktorargumente übergeben](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments) detaillierte Informationen zum Angeben von Konstruktorargumente in XAML.

Schließlich im letzte Beispiel ähnelt dem zweiten mit dem Unterschied, dass eines der untergeordneten Elemente des verweist auf die `StackLayout`:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content.Children[1].Text.Length,
                      StringFormat='The first Label has {0} characters'}" />
```

Dieses untergeordnete Element ist ein `Label`, verfügt über eine `Text` Eigenschaft vom Typ `String`, verfügt über eine `Length` Eigenschaft. Die erste `Label` Berichte der `TimeSpan` legen Sie in der `TimePicker`, sodass, wenn dieser Text ändert, die endgültige `Label` sowie Änderungen.

Hier wird das Programm auf allen drei Plattformen ausgeführt:

[![Pfad Variationen](binding-path-images/pathvariations-small.png "Pfad Variationen")](binding-path-images/pathvariations-large.png#lightbox "Pfad Varianten")

## <a name="debugging-complex-paths"></a>Debuggen von komplexen Pfaden

Komplexe Pfad-Definitionen können schwer zu erstellen sein: müssen Sie wissen, der jede untergeordnete Eigenschaft oder den Typ der Elemente in der Auflistung, die nächste untergeordnete Eigenschaft ordnungsgemäß hinzugefügt werden, aber die Typen selbst werden nicht im Pfad angezeigt. Eine gute Möglichkeit besteht darin inkrementeller build den Pfad, und sehen Sie sich die Zwischenergebnisse. Für dieses Beispiel letzten konnte Sie ohne Starten `Path` Definition überhaupt:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      StringFormat='{0}'}" />
```

Die den Typ der Bindungsquelle, anzeigt oder `DataBindingDemos.PathVariationsPage`. Sie wissen `PathVariationsPage` leitet sich von `ContentPage`, sodass er verfügt eine `Content` Eigenschaft:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content,
                      StringFormat='{0}'}" />
```

Der Typ des der `Content` Eigenschaft wird jetzt offen gelegt werden `Xamarin.Forms.StackLayout`. Hinzufügen der `Children` Eigenschaft, um die `Path` und der Typ ist `Xamarin.Forms.ElementCollection'1[Xamarin.Forms.View]`, also in einer Klasse, die interne Xamarin.Forms, jedoch offensichtlich einen Sammlungstyp. Fügen Sie einen Index, und der Typ ist `Xamarin.Forms.Label`. Auf diese Weise fortgesetzt.

Da Xamarin.Forms den Bindungspfad verarbeitet, installiert eine `PropertyChanged` Handler für jedes Objekt im Pfad, der implementiert die `INotifyPropertyChanged` Schnittstelle. Angenommen, die letzte Bindung antwortet auf eine Änderung in der ersten `Label` da die `Text` -Eigenschaft ändert.

Wenn eine Eigenschaft in den Bindungspfad keine implementiert `INotifyPropertyChanged`, Änderungen an dieser Eigenschaft werden ignoriert. Einige Änderungen konnte den Bindungspfad vollständig ungültig, weshalb Sie diese Technik verwenden sollten, nur, wenn die Zeichenfolge von Eigenschaften und untergeordnete Eigenschaften nie ungültig.



## <a name="related-links"></a>Verwandte Links

- [Binden von Daten-Demos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Daten Bindung Kapitel Xamarin.Forms Buchs](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
