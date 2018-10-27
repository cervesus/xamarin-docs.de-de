---
title: Bindungspfad für Xamarin.Forms
description: In diesem Artikel erläutert das Xamarin.Forms-datenbindungen zu verwenden, um den Zugriff auf untergeordnete Eigenschaften und Member der Auflistung mit der Bindungsklasse die Path-Eigenschaft.
ms.prod: xamarin
ms.assetid: 3CF721A5-E157-468B-AD3A-DA0A45E58E8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 8340e952d30e21a5249edd0fa3319462bbd5ff8b
ms.sourcegitcommit: 913763498b5d23fa4a92e877760c51164bf1aa41
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50150235"
---
# <a name="xamarinforms-binding-path"></a>Bindungspfad für Xamarin.Forms

In allen vorherigen Datenbindung Beispielen die [ `Path` ](xref:Xamarin.Forms.Binding.Path) Eigenschaft der `Binding` Klasse (oder die [ `Path` ](xref:Xamarin.Forms.Xaml.BindingExtension.Path) Eigenschaft der `Binding` Markuperweiterung) festgelegt wurde auf eine einzelne Eigenschaft. Es ist tatsächlich möglich, legen Sie `Path` zu einem *Untereigenschaft* (eine Eigenschaft einer Eigenschaft), oder auf einen Member einer Auflistung.

Nehmen wir beispielsweise an, die Ihre Seite enthält eine `TimePicker`:

```xaml
<TimePicker x:Name="timePicker">
```

Die `Time` Eigenschaft `TimePicker` ist vom Typ `TimeSpan`, aber vielleicht möchten Sie eine Datenbindung erstellen, verweist der `TotalSeconds` -Eigenschaft dieses `TimeSpan` Wert. So sieht die Datenbindung aus:

```xaml
{Binding Source={x:Reference timePicker},
         Path=Time.TotalSeconds}
```

Die `Time` Eigenschaft ist vom Typ `TimeSpan`, die eine `TotalSeconds` Eigenschaft. Die `Time` und `TotalSeconds` Eigenschaften mit einem Punkt verbunden sind. Die Elemente in der `Path` Zeichenfolge verweist immer auf Eigenschaften und nicht auf die Typen dieser Eigenschaften.

Beispiel und verschiedene andere, in angezeigt werden der **Pfad Variationen** Seite:

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
                              StringFormat='The second Label has {0} characters'}" />
    </StackLayout>
</ContentPage>
```

In der zweiten `Label`, die Bindungsquelle wird die Seite selbst. Die `Content` Eigenschaft ist vom Typ `StackLayout`, die eine `Children` Eigenschaft vom Typ `IList<View>`, die eine `Count` , der angibt, der Anzahl der untergeordneten Eigenschaft.

## <a name="paths-with-indexers"></a>Pfade mit Indexer

Die Bindung im dritten `Label` in die **Pfad Variationen** Verweise von Seiten der [ `CultureInfo` ](xref:System.Globalization.CultureInfo) -Klasse in der `System.Globalization` Namespace:

```xaml
<Label Text="{Binding Source={x:Static globe:CultureInfo.CurrentCulture},
                      Path=DateTimeFormat.DayNames[3],
                      StringFormat='The middle day of the week is {0}'}" />
```

Die Quelle wird festgelegt, an die statische `CultureInfo.CurrentCulture` Eigenschaft, die ein Objekt des Typs `CultureInfo`. Dass die Klasse eine Eigenschaft mit dem Namen definiert `DateTimeFormat` des Typs [ `DateTimeFormatInfo` ](xref:System.Globalization.DateTimeFormatInfo) , enthält eine `DayNames` Auflistung. Der Index wird das vierte Element ausgewählt.

Der vierte `Label` etwas Ähnliches, aber für die Kultur Frankreich zugeordnet ist. Die `Source` -Eigenschaft der Bindung nastaven NA hodnotu `CultureInfo` Objekt mit einem Konstruktor:

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

Finden Sie unter [Konstruktorargument übergeben](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments) für Weitere Informationen zum Angeben von Konstruktorargumente in XAML.

Schließlich wird im letzte Beispiel ähnlich wie der zweite, mit dem Unterschied, dass sie eine der untergeordneten Elemente verweist die `StackLayout`:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content.Children[1].Text.Length,
                      StringFormat='The first Label has {0} characters'}" />
```

Dieses untergeordnete Element ist ein `Label`, die eine `Text` Eigenschaft vom Typ `String`, die eine `Length` Eigenschaft. Die erste `Label` Berichte die `TimeSpan` legen Sie in der `TimePicker`, wenn Sie diesen Text ändern, also die endgültige `Label` auch Änderungen.

Hier wird das Programm auf allen drei Plattformen ausgeführt wird:

[![Pfad-Varianten](binding-path-images/pathvariations-small.png "Pfad Variationen")](binding-path-images/pathvariations-large.png#lightbox "Pfad-Varianten")

## <a name="debugging-complex-paths"></a>Debuggen von komplexen Pfaden

Definitionen von komplexen Pfad können schwierig sein, zu erstellen: Sie wissen den Typ der einzelnen untergeordneten Eigenschaften oder der Typ der Elemente in der Auflistung, die nächste untergeordnete Eigenschaft ordnungsgemäß hinzugefügt werden müssen, aber die Typen selbst erscheinen nicht im Pfad. Ein gutes Verfahren ist inkrementell, den Pfad zu erstellen, und sehen Sie sich die Zwischenergebnisse. Für dieses letzte Beispiel können Sie sich zunächst keine `Path` Definition gar:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      StringFormat='{0}'}" />
```

Die den Typ der Bindungsquelle, anzeigt oder `DataBindingDemos.PathVariationsPage`. Sie wissen `PathVariationsPage` leitet sich von `ContentPage`, sodass sie verfügt über eine `Content` Eigenschaft:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content,
                      StringFormat='{0}'}" />
```

Der Typ des der `Content` Eigenschaft jetzt angezeigt wird, sein `Xamarin.Forms.StackLayout`. Hinzufügen der `Children` Eigenschaft, um die `Path` und der Typ ist `Xamarin.Forms.ElementCollection'1[Xamarin.Forms.View]`, der eine Klasse, die für Xamarin.Forms, aber natürlich einen Auflistungstyp intern ist. Fügen Sie einen Index, und der Typ ist `Xamarin.Forms.Label`. Auf diese Weise fortgesetzt.

Xamarin.Forms beim Verarbeiten des Bindungspfads installiert eine `PropertyChanged` Handler für jedes Objekt im Pfad, der implementiert die `INotifyPropertyChanged` Schnittstelle. Z. B. die letzte Bindung reagiert auf eine Änderung in der ersten `Label` da die `Text` eigenschaftenänderungen.

Wenn eine Eigenschaft in den Bindungspfad nicht implementiert `INotifyPropertyChanged`, Änderungen an dieser Eigenschaft werden ignoriert. Einige Änderungen konnte den Bindungspfad vollständig ungültig, daher Sie dieses Verfahren verwenden sollten, nur, wenn die Zeichenfolge der Eigenschaften und die untergeordneten Eigenschaften nie ungültig.



## <a name="related-links"></a>Verwandte Links

- [Binden von Daten-Demos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Im Kapitel Daten-Bindung von Xamarin.Forms-Buch](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
