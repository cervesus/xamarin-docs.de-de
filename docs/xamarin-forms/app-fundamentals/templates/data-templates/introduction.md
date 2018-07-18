---
title: Einführung in Xamarin.Forms-Datenvorlagen
description: Xamarin.Forms-Datenvorlagen bieten die Möglichkeit, die Darstellung von Daten für unterstützte Steuerelemente zu definieren. Dieser Artikel enthält eine Einführung in die Data-Vorlagen, untersuchen, warum sie erforderlich sind.
ms.prod: xamarin
ms.assetid: 4ED4ACF4-BE4A-44ED-8EAF-C03947B8663B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 129ce7a04b93bfb3cb1b9a1639aee61cd56d09d5
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998913"
---
# <a name="introduction-to-xamarinforms-data-templates"></a>Einführung in Xamarin.Forms-Datenvorlagen

_Xamarin.Forms-Datenvorlagen bieten die Möglichkeit, die Darstellung von Daten für unterstützte Steuerelemente zu definieren. Dieser Artikel enthält eine Einführung in die Data-Vorlagen, untersuchen, warum sie erforderlich sind._

Erwägen Sie eine [ `ListView` ](xref:Xamarin.Forms.ListView) , die zeigt, dass eine Auflistung von `Person` Objekte. Das folgende Codebeispiel zeigt die Definition der `Person` Klasse:

```csharp
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
    public string Location { get; set; }
}
```

Die `Person` -Klasse definiert `Name`, `Age`, und `Location` Eigenschaften, die dann festgelegt werden, können eine `Person` Objekt erstellt wird. Die [ `ListView` ](xref:Xamarin.Forms.ListView) wird verwendet, um die Auflistung der Anzeige `Person` Objekte, wie in den folgenden XAML-Codebeispiel gezeigt:

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
                    <local:Person Name="John" Age="37" Location="USA" />
                    <local:Person Name="Tom" Age="42" Location="UK" />
                    <local:Person Name="Lucas" Age="29" Location="Germany" />
                    <local:Person Name="Tariq" Age="39" Location="UK" />
                    <local:Person Name="Jane" Age="30" Location="USA" />
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </StackLayout>
</ContentPage>
```

Elemente werden hinzugefügt, um die [ `ListView` ](xref:Xamarin.Forms.ListView) in XAML durch die Initialisierung der [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) Eigenschaft aus einem Array von `Person` Instanzen.

> [!NOTE]
> Beachten Sie, dass die `x:Array` -Element ist ein `Type` Attribut, der angibt, der des Typs der Elemente im Array.

Die entsprechende C#-Seite wird angezeigt, in dem folgenden Codebeispiel wird die initialisiert die [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) Eigenschaft, um eine `List` von `Person` Instanzen:

```csharp
public WithoutDataTemplatePageCS()
{
    ...
    var people = new List<Person>
    {
        new Person { Name = "Steve", Age = 21, Location = "USA" },
        new Person { Name = "John", Age = 37, Location = "USA" },
        new Person { Name = "Tom", Age = 42, Location = "UK" },
        new Person { Name = "Lucas", Age = 29, Location = "Germany" },
        new Person { Name = "Tariq", Age = 39, Location = "UK" },
        new Person { Name = "Jane", Age = 30, Location = "USA" }
    };

    Content = new StackLayout
    {
        Margin = new Thickness(20),
        Children = {
            ...
            new ListView { ItemsSource = people, Margin = new Thickness(0, 20, 0, 0) }
        }
    };
}
```

Die [ `ListView` ](xref:Xamarin.Forms.ListView) Aufrufe `ToString` beim Anzeigen der Objekte in der Auflistung. Da gibt es keine `Person.ToString` außer Kraft setzen, `ToString` gibt den Typnamen, der jedes Objekt zurück, wie in den folgenden Screenshots gezeigt:

![](introduction-images/no-data-template.png "ListView ohne eine Datenvorlage")

Die `Person` Objekt kann außer Kraft setzen der `ToString` Methode, um aussagekräftige Daten anzuzeigen, wie im folgenden Codebeispiel gezeigt:

```csharp
public class Person
{
  ...
  public override string ToString ()
  {
    return Name;
  }
}
```

Dadurch wird die [ `ListView` ](xref:Xamarin.Forms.ListView) Anzeigen der `Person.Name` Wert der Eigenschaft für jedes Objekt in der Auflistung, wie in den folgenden Screenshots gezeigt:

![](introduction-images/override-tostring.png "ListView mit einer Datenvorlage")

Die `Person.ToString` außer Kraft setzen kann eine formatierte Zeichenfolge, die mit Zurückgeben der `Name`, `Age`, und `Location` Eigenschaften. Dieser Ansatz bietet jedoch nur eine eingeschränkte Steuerung der Darstellung der einzelnen Elemente der Daten. Zur Erhöhung der Flexibilität eine [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) können erstellt werden, die die Darstellung der Daten definiert.

## <a name="creating-a-datatemplate"></a>Erstellen ein DataTemplate-Element

Ein [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) verwendet, um die Darstellung der Daten anzugeben, und in der Regel die Datenbindung zum Anzeigen von Daten verwendet. Die allgemeinen Verwendungsszenario ist beim Anzeigen von Daten aus einer Auflistung von Objekten in einem [ `ListView` ](xref:Xamarin.Forms.ListView). Z. B., wenn eine `ListView` gebunden ist, auf eine Auflistung von `Person` Objekte, die `ListView.ItemTemplate` Eigenschaft auf festgelegt ein `DataTemplate` , definiert die Darstellung der einzelnen `Person` -Objekt in der `ListView`. Die `DataTemplate` enthält Elemente, die Bindung an Eigenschaftswerte der einzelnen `Person` Objekt. Weitere Informationen zur Datenbindung finden Sie unter [Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) (Datenbindungsgrundlagen).

Ein [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) können als Wert verwendet werden, für die folgenden Eigenschaften:

- [`ListView.HeaderTemplate`](xref:Xamarin.Forms.ListView.HeaderTemplate)
- [`ListView.FooterTemplate`](xref:Xamarin.Forms.ListView.FooterTemplate)
- [`ListView.GroupHeaderTemplate`](xref:Xamarin.Forms.ListView.GroupHeaderTemplate)
- [`ItemsView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1), die von geerbt wird [ `ListView` ](xref:Xamarin.Forms.ListView).
- [`MultiPage.ItemTemplate`](xref:Xamarin.Forms.MultiPage`1), die von geerbt wird [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage), [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage), und [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage).

> [!NOTE]
> Beachten Sie, dass, obwohl die [ `TableView` ](xref:Xamarin.Forms.TableView) verwendet [ `Cell` ](xref:Xamarin.Forms.Cell) Objekte aufweist, wird nicht verwendet eine [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate). Dies ist, da der datenbindungen, die direkt auf always festgelegt sind `Cell` Objekte.

Ein [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) befindet, die als direkt untergeordnetes Element der oben aufgeführten Eigenschaften als bekannt ist, eine *Inlinevorlage*. Sie können auch eine `DataTemplate` kann als eine Ressource Steuerungsebene, Seiten- oder Anwendungsebene definiert werden. Auswählen des Installationsorts für das Definieren einer [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) wirkt sich auf, wo sie verwendet werden kann:

- Ein [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) definiert an das Steuerelement kann auf Ebene nur für das Steuerelement angewendet werden.
- Ein [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) definiert auf der Seite Ebene auf mehrere gültige Steuerelemente auf der Seite angewendet werden kann.
- Ein [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) auf Anwendungsebene definierten gültigen angewendeten Steuerelemente in der gesamten Anwendung werden können.

Weiter unten in der Hierarchie von Inhaltsansichten Datenvorlagen haben Vorrang vor den definierten höher einrichten, bei der Freigabe `x:Key` Attribute. Z. B. eine Datenvorlage auf Anwendungsebene werden in der Vorlage auf Seitenebene Daten überschrieben, und eine Datenvorlage auf Seitenebene wird durch eine Steuerungsebene Datenvorlage oder eine Inlinevorlage für die Daten überschrieben werden.


## <a name="related-links"></a>Verwandte Links

- [Darstellung von Zellen](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [Datenvorlagen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
- [DataTemplate-Element](xref:Xamarin.Forms.DataTemplate)
