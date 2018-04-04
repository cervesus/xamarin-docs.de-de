---
title: Erstellen einer DataTemplate
description: 'Datenvorlagen können Inline in einem: "ResourceDictionary" oder über einen benutzerdefinierten Typ oder eine entsprechende Xamarin.Forms Zellentyp erstellt werden. In diesem Artikel untersucht die einzelnen Verfahren.'
ms.prod: xamarin
ms.assetid: CFF4AB5E-9069-461C-84D8-F9F6C38510AB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: cd4266fb8e7d68a9f93bd169ca70c6a0f516a357
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="creating-a-datatemplate"></a>Erstellen einer DataTemplate

_Datenvorlagen können Inline in einem: "ResourceDictionary" oder über einen benutzerdefinierten Typ oder eine entsprechende Xamarin.Forms Zellentyp erstellt werden. In diesem Artikel untersucht die einzelnen Verfahren._

Ein häufiges Verwendungsszenario für eine [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) ist Anzeigen von Daten aus einer Auflistung von Objekten in einer [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Die Darstellung der Daten für jede Zelle in der [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) verwaltet werden können, durch Festlegen der [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) Eigenschaft, um eine [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/). Es gibt eine Reihe von Techniken, die verwendet werden kann, um dies zu erreichen:

- [Erstellen eine Inline-DataTemplate](#inline).
- [Erstellen eine DataTemplate mit einem Typ](#type).
- [Erstellen einer DataTemplate als Ressource](#resource).

Unabhängig von den Verfahren, die verwendet wird, das Ergebnis ist, die die Darstellung der einzelnen Zellen in der [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) wird definiert, indem eine [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), wie in den folgenden Screenshots veranschaulicht:

![](creating-images/data-template-appearance.png "ListView mit DataTemplate")

<a name="inline" />

## <a name="creating-an-inline-datatemplate"></a>Erstellen eine Inline-DataTemplate

Die [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) Eigenschaft kann festgelegt werden, auf eine Inline- [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/). Eine Inlinevorlage, d., die h. eine, die als direkt untergeordnetes Element von einem entsprechenden Steuerelementeigenschaft platziert wird, sollte verwendet werden, wenn keine Notwendigkeit besteht, die Datenvorlage an anderer Stelle wiederverwenden. Die Elemente im angegebenen die `DataTemplate` definieren die Darstellung der einzelnen Zellen, wie im folgenden Beispiel der Verwendung von XAML-Code dargestellt:

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

Das untergeordnete Element des ein Inlineschema [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) muss sein, oder abgeleitet, geben Sie [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/). Layout in der `ViewCell` wird hier von verwaltet eine [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/). Die `Grid` enthält drei [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Instanzen dieser Bindung ihre [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) Eigenschaften auf die entsprechenden Eigenschaften der einzelnen `Person` Objekt in der Auflistung.

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

In C# geschrieben, die Inline [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) wird unter Verwendung einer Überladung des Konstruktors, der angibt, erstellt eine `Func` Argument.

<a name="type" />

## <a name="creating-a-datatemplate-with-a-type"></a>Erstellen eine DataTemplate mit einem Typ

Die [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) Eigenschaft kann auch festgelegt werden, um eine [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) , die von einem Zellentyp erstellt wird. Der Vorteil dieses Ansatzes ist, dass die Darstellung, die durch den Zellentyp definierten vom mehrerer Datenvorlagen in der gesamten Anwendung wiederverwendet werden kann. Der folgende XAML-Code zeigt ein Beispiel dieses Ansatzes:

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

Hier die [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) -Eigenschaftensatz auf eine [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) , die von benutzerdefinierten Typen, die definiert die Darstellung der Zelle erstellt wird. Der benutzerdefinierte Typ muss vom Typ ableiten [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), wie im folgenden Codebeispiel gezeigt:

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

Innerhalb der [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), Layout wird hier von verwaltet eine [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/). Die `Grid` enthält drei [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Instanzen dieser Bindung ihre [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) Eigenschaften auf die entsprechenden Eigenschaften der einzelnen `Person` Objekt in der Auflistung.

Die entsprechende C#-Code wird im folgenden Beispiel gezeigt:

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

In c# ist die [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) wird unter Verwendung einer Überladung des Konstruktors, der angibt, welche Zelle als Argument erstellt. Der Zellentyp muss vom Typ ableiten [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), wie im folgenden Codebeispiel gezeigt:

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
> Beachten Sie, dass Xamarin.Forms auch Zellentypen, die verwendet werden kann enthält, um einfache Daten angezeigt werden sollen [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Zellen. Weitere Informationen finden Sie unter [Darstellung Zelle](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

<a name="resource" />

## <a name="creating-a-datatemplate-as-a-resource"></a>Erstellen eine DataTemplate als Ressource

Datenvorlagen können auch erstellt werden, als wieder verwendbare Objekte in einem [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). Dies erfolgt durch die Vergabe jede Deklaration eine eindeutige `x:Key` -Attribut, das mit einem beschreibenden Schlüssel in bietet die `ResourceDictionary`, wie im folgenden Beispiel der Verwendung von XAML-Code dargestellt:

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

Die [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) zugewiesen ist die [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) Eigenschaft mithilfe der `StaticResource` Markuperweiterung. Beachten Sie, dass die `DataTemplate` wird definiert, auf der Seite [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), kann auch auf das Steuerelement oder Anwendungsebene definiert werden.

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

Die [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) hinzugefügt wird die [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) mithilfe der [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ResourceDictionary.Add/p/System.String/System.Object/) Methode, der angibt, eine `Key` Zeichenfolge, die zur Referenz der `DataTemplate` beim Abrufen.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie Datenvorlagen, Inline aus einem benutzerdefinierten Typ oder erstellt eine [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). Eine Inlinevorlage sollte verwendet werden, wenn keine Notwendigkeit besteht, die Datenvorlage an anderer Stelle wiederverwenden. Alternativ kann eine Datenvorlage definieren ihn als einen benutzerdefinierten Typ oder als Steuerungsebene,-Ressource auf Seitenebene oder auf Anwendungsebene wiederverwendet werden.


## <a name="related-links"></a>Verwandte Links

- [Darstellung von Zellen](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [Datenvorlagen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
- [DataTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)
