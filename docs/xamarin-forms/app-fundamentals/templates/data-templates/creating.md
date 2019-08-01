---
title: Erstellen eines Xamarin.Forms-DataTemplate-Objekts
description: Datenvorlagen können inline erstellt werden, z.B. in einem ResourceDictionary-Objekt oder über einen benutzerdefinierten Typ oder geeigneten Xamarin.Forms-Zellentyp. In diesem Artikel werden alle Methoden erläutert.
ms.prod: xamarin
ms.assetid: CFF4AB5E-9069-461C-84D8-F9F6C38510AB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 1163f264fc54a461d8d95854524439589cdc81f5
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68646986"
---
# <a name="creating-a-xamarinforms-datatemplate"></a>Erstellen eines Xamarin.Forms-DataTemplate-Objekts

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-datatemplates)

_Datenvorlagen können inline erstellt werden, z.B. in einem ResourceDictionary-Objekt oder über einen benutzerdefinierten Typ oder geeigneten Xamarin.Forms-Zellentyp. In diesem Artikel werden alle Methoden erläutert._

Ein gängiges Verwendungsszenario für eine [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) ist das Anzeigen von Daten aus einer Objektsammlung in einer [`ListView`](xref:Xamarin.Forms.ListView). Das Aussehen der Daten in jeder Zelle in der [`ListView`](xref:Xamarin.Forms.ListView) kann durch das Festlegen der [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1)-Eigenschaft auf [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) definiert werden. Dafür gibt es mehrere Möglichkeiten:

- [Erstellen eines Inline-DataTemplate-Objekts](#inline)
- [Erstellen eines DataTemplate-Objekts mit einem Typ](#type)
- [Erstellen eines DataTemplate-Objekts als Ressource](#resource)

Unabhängig von der verwendeten Methode führt dies dazu, dass das Aussehen jeder Zelle in der [`ListView`](xref:Xamarin.Forms.ListView) von einer [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) definiert wird. Dies wird im folgenden Screenshots veranschaulicht:

![](creating-images/data-template-appearance.png "ListView mit einem DataTemplate-Objekt")

<a name="inline" />

## <a name="creating-an-inline-datatemplate"></a>Erstellen eines Inline-DataTemplate-Objekts

Die [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1)-Eigenschaft kann auf eine Inline-[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) festgelegt werden. Eine Inlinevorlage wird als direktes untergeordnetes Element einer entsprechenden Steuerelementeigenschaft eingefügt. Sie sollte verwendet werden, wenn die Datenvorlage nicht an einer anderen Stelle wiederverwendet werden muss. Die in der `DataTemplate` angegebenen Elemente definieren das Aussehen jeder Zelle. Dies wird in folgendem XAML-Codebeispiel veranschaulicht:

```xaml
<ListView Margin="0,20,0,0">
    <ListView.ItemsSource>
        <x:Array Type="{x:Type local:Person}">
            <local:Person Name="Steve" Age="21" Location="USA" />
            <local:Person Name="John" Age="37" Location="USA" />
            <local:Person Name="Tom" Age="42" Location="UK" />
            <local:Person Name="Lucas" Age="29" Location="Germany" />
            <local:Person Name="Tariq" Age="39" Location="UK" />
            <local:Person Name="Jane" Age="30" Location="USA" />
        </x:Array>
    </ListView.ItemsSource>
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
                <Grid>
                    ...
                    <Label Text="{Binding Name}" FontAttributes="Bold" />
                    <Label Grid.Column="1" Text="{Binding Age}" />
                    <Label Grid.Column="2" Text="{Binding Location}" HorizontalTextAlignment="End" />
                </Grid>
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Das untergeordnete Element einer Inline-[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) muss den Typ [`Cell`](xref:Xamarin.Forms.Cell) aufweisen oder von diesem abgeleitet sein. In diesem Beispiel wird [`ViewCell`](xref:Xamarin.Forms.ViewCell) verwendet, das sich aus `Cell` ableitet. Das Layout in der `ViewCell` wird hier von einem [`Grid`](xref:Xamarin.Forms.Grid)-Objekt bestimmt. Das `Grid`-Objekt enthält drei [`Label`](xref:Xamarin.Forms.Label)-Instanzen, die ihre [`Text`](xref:Xamarin.Forms.Label.Text)-Eigenschaften an die entsprechenden Eigenschaften jedes `Person`-Objekts in der Sammlung binden.

Der äquivalente C#-Code ist im folgenden Codebeispiel zu sehen:

```csharp
public class WithDataTemplatePageCS : ContentPage
{
    public WithDataTemplatePageCS()
    {
        ...
        var people = new List<Person>
        {
            new Person { Name = "Steve", Age = 21, Location = "USA" },
            ...
        };

        var personDataTemplate = new DataTemplate(() =>
        {
            var grid = new Grid();
            ...
            var nameLabel = new Label { FontAttributes = FontAttributes.Bold };
            var ageLabel = new Label();
            var locationLabel = new Label { HorizontalTextAlignment = TextAlignment.End };

            nameLabel.SetBinding(Label.TextProperty, "Name");
            ageLabel.SetBinding(Label.TextProperty, "Age");
            locationLabel.SetBinding(Label.TextProperty, "Location");

            grid.Children.Add(nameLabel);
            grid.Children.Add(ageLabel, 1, 0);
            grid.Children.Add(locationLabel, 2, 0);

            return new ViewCell { View = grid };
        });

        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children = {
                ...
                new ListView { ItemsSource = people, ItemTemplate = personDataTemplate, Margin = new Thickness(0, 20, 0, 0) }
            }
        };
    }
}
```

In C# wird die Inline-[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) mit einer Konstruktorüberladung erstellt, die ein `Func`-Argument angibt.

<a name="type" />

## <a name="creating-a-datatemplate-with-a-type"></a>Erstellen eines DataTemplate-Objekts mit einem Typ

Die [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1)-Eigenschaft kann auch als [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) festgelegt werden, die über einen Zellentyp erstellt wird. Ein Vorteil dieser Vorgehensweise besteht darin, dass das vom Zellentyp definierte Aussehen von weiteren Datenvorlagen in der Anwendung wiederverwendet werden kann. Der folgende XAML Code ist ein Beispiel für ein solches Vorgehen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataTemplates"
             ...>
    <StackLayout Margin="20">
        ...
        <ListView Margin="0,20,0,0">
           <ListView.ItemsSource>
                <x:Array Type="{x:Type local:Person}">
                    <local:Person Name="Steve" Age="21" Location="USA" />
                    ...
                </x:Array>
            </ListView.ItemsSource>
            <ListView.ItemTemplate>
                <DataTemplate>
                    <local:PersonCell />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

Hier wird die [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1)-Eigenschaft auf [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) festgelegt, die über einen benutzerdefinierten Typ erstellt wird, der das Aussehen der Zelle definiert. Der benutzerdefinierte Typ muss von einem [`ViewCell`](xref:Xamarin.Forms.ViewCell)-Typ abgeleitet werden. Dies wird im folgenden Codebeispiel veranschaulicht:

```xaml
<ViewCell xmlns="http://xamarin.com/schemas/2014/forms"
          xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
          x:Class="DataTemplates.PersonCell">
     <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.2*" />
            <ColumnDefinition Width="0.3*" />
        </Grid.ColumnDefinitions>
        <Label Text="{Binding Name}" FontAttributes="Bold" />
        <Label Grid.Column="1" Text="{Binding Age}" />
        <Label Grid.Column="2" Text="{Binding Location}" HorizontalTextAlignment="End" />
    </Grid>
</ViewCell>
```

In der [`ViewCell`](xref:Xamarin.Forms.ViewCell) wird hier das Layout von einem [`Grid`](xref:Xamarin.Forms.Grid)-Objekt bestimmt. Das `Grid`-Objekt enthält drei [`Label`](xref:Xamarin.Forms.Label)-Instanzen, die ihre [`Text`](xref:Xamarin.Forms.Label.Text)-Eigenschaften an die entsprechenden Eigenschaften jedes `Person`-Objekts in der Sammlung binden.

Der entsprechende C#-Code ist im folgenden Codebeispiel zu sehen:

```csharp
public class WithDataTemplatePageFromTypeCS : ContentPage
{
    public WithDataTemplatePageFromTypeCS()
    {
        ...
        var people = new List<Person>
        {
            new Person { Name = "Steve", Age = 21, Location = "USA" },
            ...
        };

        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children = {
                ...
                new ListView { ItemTemplate = new DataTemplate(typeof(PersonCellCS)), ItemsSource = people, Margin = new Thickness(0, 20, 0, 0) }
            }
        };
    }
}
```

In C# wird die Inline-[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) mit einer Konstruktorüberladung erstellt, die den Zellentyp als Argument angibt. Der Zellentyp muss vom [`ViewCell`](xref:Xamarin.Forms.ViewCell)-Typ abgeleitet werden. Dies wird im folgenden Codebeispiel veranschaulicht:

```csharp
public class PersonCellCS : ViewCell
{
    public PersonCellCS()
    {
        var grid = new Grid();
        ...
        var nameLabel = new Label { FontAttributes = FontAttributes.Bold };
        var ageLabel = new Label();
        var locationLabel = new Label { HorizontalTextAlignment = TextAlignment.End };

        nameLabel.SetBinding(Label.TextProperty, "Name");
        ageLabel.SetBinding(Label.TextProperty, "Age");
        locationLabel.SetBinding(Label.TextProperty, "Location");

        grid.Children.Add(nameLabel);
        grid.Children.Add(ageLabel, 1, 0);
        grid.Children.Add(locationLabel, 2, 0);

        View = grid;
    }
}
```

> [!NOTE]
> Beachten Sie, dass Xamarin.Forms außerdem Zellentypen beinhaltet, die verwendet werden können, um einfache Daten in [`ListView`](xref:Xamarin.Forms.ListView)-Zellen anzuzeigen. Weitere Informationen finden Sie im Artikel zur [Darstellung von Zellen](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

<a name="resource" />

## <a name="creating-a-datatemplate-as-a-resource"></a>Erstellen eines DataTemplate-Objekts als Ressource

Datenvorlage können auch als wiederverwendbare Objekte in einem [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) erstellt werden. Geben Sie dazu jeder Deklaration ein eindeutiges `x:Key`-Attribut, das sie mit einem aussagekräftigen Schlüssel im `ResourceDictionary` versieht. Dies wird in folgendem XAML-Codebeispiel veranschaulicht:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             ...>
    <ContentPage.Resources>
        <ResourceDictionary>
            <DataTemplate x:Key="personTemplate">
                 <ViewCell>
                    <Grid>
                        ...
                    </Grid>
                </ViewCell>
            </DataTemplate>
        </ResourceDictionary>
    </ContentPage.Resources>
    <StackLayout Margin="20">
        ...
        <ListView ItemTemplate="{StaticResource personTemplate}" Margin="0,20,0,0">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type local:Person}">
                    <local:Person Name="Steve" Age="21" Location="USA" />
                    ...
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </StackLayout>
</ContentPage>
```

Das [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Objekt wird der [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1)-Eigenschaft mithilfe der Markuperweiterung `StaticResource` zugewiesen. Beachten Sie, dass das `DataTemplate`-Objekt zwar im [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)-Objekt der Seite definiert wird, es aber dennoch auch auf Steuerelementebene oder Anwendungsebene definiert werden kann.

Im folgenden Codebeispiel wird die entsprechende Seite in C# dargestellt:

```csharp
public class WithDataTemplatePageCS : ContentPage
{
  public WithDataTemplatePageCS ()
  {
    ...
    var personDataTemplate = new DataTemplate (() => {
      var grid = new Grid ();
      ...
      return new ViewCell { View = grid };
    });

    Resources = new ResourceDictionary ();
    Resources.Add ("personTemplate", personDataTemplate);

    Content = new StackLayout {
      Margin = new Thickness(20),
      Children = {
        ...
        new ListView { ItemTemplate = (DataTemplate)Resources ["personTemplate"], ItemsSource = people };
      }
    };
  }
}
```

Das [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Objekt wird dem [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)-Objekt mithilfe der [`Add`](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object))-Methode hinzugefügt, die eine `Key`-Zeichenfolge angibt, mit der auf die `DataTemplate` verwiesen wird, wenn diese abgerufen wird.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde beschrieben, wie Sie Datenvorlagen auf unterschiedliche Weisen erstellen können: inline, über einen benutzerdefinierten Typ oder in einem [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)-Objekt. Eine Inlinevorlage sollte verwendet werden, wenn keine Notwendigkeit besteht, die Datenvorlage woanders erneut zu verwenden. Alternativ kann eine Datenvorlage erneut verwendet werden, indem sie als benutzerdefinierter Typ oder als Ressource auf Steuerelementebene, Seitenebene oder Anwendungsebene definiert wird.


## <a name="related-links"></a>Verwandte Links

- [Darstellung von Zellen](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [Data Templates (Datenvorlagen (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-datatemplates)
- [DataTemplate](xref:Xamarin.Forms.DataTemplate)
