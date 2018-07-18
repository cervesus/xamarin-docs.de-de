---
title: Erstellen einer Xamarin.Forms-DataTemplate
description: Datenvorlagen können Inline in einem ResourceDictionary oder aus einem benutzerdefinierten Typ oder die entsprechende Xamarin.Forms-Zellentyp erstellt werden. In diesem Artikel untersucht jede Technik.
ms.prod: xamarin
ms.assetid: CFF4AB5E-9069-461C-84D8-F9F6C38510AB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 63f9bf82bc8e637aced1afa5d5699ac1e8dc3f8c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994614"
---
# <a name="creating-a-xamarinforms-datatemplate"></a>Erstellen einer Xamarin.Forms-DataTemplate

_Datenvorlagen können Inline in einem ResourceDictionary oder aus einem benutzerdefinierten Typ oder die entsprechende Xamarin.Forms-Zellentyp erstellt werden. In diesem Artikel untersucht jede Technik._

Ein allgemeines Verwendungsszenario für ein [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) zeigt Daten aus einer Auflistung von Objekten in einem [ `ListView` ](xref:Xamarin.Forms.ListView). Die Darstellung der Daten für jede Zelle in der [ `ListView` ](xref:Xamarin.Forms.ListView) verwaltet werden können, durch Festlegen der [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) Eigenschaft, um eine [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate). Es gibt eine Reihe von Techniken, die verwendet werden können, um dies zu erreichen:

- [Erstellen eine Inline-DataTemplate](#inline).
- [Erstellen ein DataTemplate-Element mit einem Typ](#type).
- [Erstellen eine Datenvorlage als Ressource](#resource).

Unabhängig von der Methode verwendet wird, das Ergebnis ist, die die Darstellung der einzelnen Zellen in der [ `ListView` ](xref:Xamarin.Forms.ListView) wird definiert, indem eine [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), wie in den folgenden Screenshots gezeigt:

![](creating-images/data-template-appearance.png "ListView mit einem DataTemplate")

<a name="inline" />

## <a name="creating-an-inline-datatemplate"></a>Erstellen eine Inline-DataTemplate

Die [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) Eigenschaft kann festgelegt werden, auf eine Inline- [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate). Eine Inlinevorlage, von die als direkt untergeordnetes Element von einem entsprechenden Steuerelementeigenschaft platziert wird, sollte verwendet werden, wenn keine Notwendigkeit besteht, die Datenvorlage an anderer Stelle wiederverwenden. Der im angegebenen Elemente den `DataTemplate` definieren Sie die Darstellung der einzelnen Zellen, wie in den folgenden XAML-Codebeispiel gezeigt:

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

Das untergeordnete Element des Inlinetyp [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) müssen, werden von abgeleitet werden, oder [ `ViewCell` ](xref:Xamarin.Forms.ViewCell). Layout in der `ViewCell` wird hier von verwaltet eine [ `Grid` ](xref:Xamarin.Forms.Grid). Die `Grid` enthält drei [ `Label` ](xref:Xamarin.Forms.Label) Bindung Instanzen ihrer [ `Text` ](xref:Xamarin.Forms.Label.Text) Eigenschaften, die die entsprechenden Eigenschaften der einzelnen `Person` Objekt in der Auflistung.

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

In C# geschrieben, die Inline [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) wurde mit einer Überladung des Konstruktors, der angibt, ein `Func` Argument.

<a name="type" />

## <a name="creating-a-datatemplate-with-a-type"></a>Erstellen ein DataTemplate-Element mit einem Typ

Die [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) Eigenschaft kann auch festgelegt werden, um eine [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) , die von einem Zellentyp erstellt wird. Der Vorteil dieses Ansatzes ist, die Darstellung, die durch den Zellentyp definiert, die durch mehrere Data-Vorlagen in der gesamten Anwendung wiederverwendet werden kann. Der folgende XAML-Code zeigt ein Beispiel dieses Ansatzes:

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

Hier die [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) -Eigenschaftensatz auf eine [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) , die einen benutzerdefinierten Typ, der definiert, die Darstellung der Zelle erstellt wird. Der benutzerdefinierte Typ muss vom Typ abgeleitet werden [ `ViewCell` ](xref:Xamarin.Forms.ViewCell), wie im folgenden Codebeispiel gezeigt:

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

In der [ `ViewCell` ](xref:Xamarin.Forms.ViewCell), Layout wird hier verwaltet, indem eine [ `Grid` ](xref:Xamarin.Forms.Grid). Die `Grid` enthält drei [ `Label` ](xref:Xamarin.Forms.Label) Bindung Instanzen ihrer [ `Text` ](xref:Xamarin.Forms.Label.Text) Eigenschaften, die die entsprechenden Eigenschaften der einzelnen `Person` Objekt in der Auflistung.

Der entsprechende C#-Code ist im folgenden Beispiel dargestellt:

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

In c# die [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) wurde mit einer Überladung des Konstruktors, der den Zellentyp als Argument angibt. Der Zellentyp muss vom Typ abgeleitet werden [ `ViewCell` ](xref:Xamarin.Forms.ViewCell), wie im folgenden Codebeispiel gezeigt:

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
> Beachten Sie, dass Xamarin.Forms auch Zellentypen, die verwendet werden können enthält, um einfache Daten angezeigt werden sollen [ `ListView` ](xref:Xamarin.Forms.ListView) Zellen. Weitere Informationen finden Sie unter [Zellendarstellung](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

<a name="resource" />

## <a name="creating-a-datatemplate-as-a-resource"></a>Erstellen eine Datenvorlage als Ressource

Datenvorlagen können auch erstellt werden, als wieder verwendbare Objekte in einem [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). Dies wird erreicht, indem jeder Deklaration einen eindeutigen `x:Key` -Attribut, das darüber mit einem beschreibenden Schlüssel in der `ResourceDictionary`, wie in den folgenden XAML-Codebeispiel gezeigt:

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

Die [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) zugewiesen wird die [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) -Eigenschaft mithilfe der `StaticResource` Markuperweiterung. Beachten Sie, dass die `DataTemplate` ist in der Seite definiert [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), sie können auch auf der Steuerungsebene oder Anwendungsebene definiert werden.

Das folgende Codebeispiel zeigt die entsprechende Seite in c#:

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

Die [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) hinzugefügt wird die [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) mithilfe der [ `Add` ](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object)) -Methode, die gibt an, eine `Key` Zeichenfolge, die zum Referenz der `DataTemplate` beim Abrufen.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie Datenvorlagen, Inline, erstellen Sie einen benutzerdefinierten Typ oder in einem [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). Eine Inlinevorlage sollte verwendet werden, wenn keine Notwendigkeit besteht, die Datenvorlage an anderer Stelle wiederverwenden. Alternativ kann eine Datenvorlage wiederverwendet werden, definieren sie als einen benutzerdefinierten Typ oder als Steuerungsebene, Seiten- oder Anwendungsebene Ressource.


## <a name="related-links"></a>Verwandte Links

- [Darstellung von Zellen](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [Datenvorlagen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
- [DataTemplate-Element](xref:Xamarin.Forms.DataTemplate)
