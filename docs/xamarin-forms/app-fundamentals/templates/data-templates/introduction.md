---
title: "Einführung"
description: "Xamarin.Forms Datenvorlagen bieten die Möglichkeit zum Definieren der Darstellung von Daten auf unterstützte Steuerelemente. Dieser Artikel bietet eine Einführung in Data-Vorlagen, untersuchen, warum diese erforderlich sind."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4ED4ACF4-BE4A-44ED-8EAF-C03947B8663B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: e7c1a8b233538b7ad40a18e44747bef672210516
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="introduction"></a>Einführung

_Xamarin.Forms Datenvorlagen bieten die Möglichkeit zum Definieren der Darstellung von Daten auf unterstützte Steuerelemente. Dieser Artikel bietet eine Einführung in Data-Vorlagen, untersuchen, warum diese erforderlich sind._

Betrachten Sie eine [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) , die eine Auflistung von anzeigt `Person` Objekte. Das folgende Codebeispiel zeigt die Definition der `Person` Klasse:

```csharp
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
    public string Location { get; set; }
}
```

Die `Person` Klasse definiert `Name`, `Age`, und `Location` Eigenschaften, die festgelegt werden können ein `Person` Objekt erstellt wird. Die [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) dient zum Anzeigen der Auflistung der `Person` Objekte, wie im folgenden Beispiel der Verwendung von XAML-Code dargestellt:

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

Elemente werden hinzugefügt, um die [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) in XAML, indem Sie initialisieren die [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) Eigenschaft aus einem Array von `Person` Instanzen.

> [!NOTE]
> Beachten Sie, dass die `x:Array` Element erfordert eine `Type` Attribut, der angibt, der Typ der Elemente im Array.

Die entsprechende C#-Seite wird angezeigt, im folgenden Codebeispiel wird initialisiert die [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) Eigenschaft, um eine `List` von `Person` Instanzen:

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

Die [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Aufrufe `ToString` beim Anzeigen der Objekte in der Auflistung. Da es ist keine `Person.ToString` zu überschreiben, `ToString` gibt den Typnamen, der jedes Objekt zurück, wie in den folgenden Screenshots dargestellt:

![](introduction-images/no-data-template.png "ListView ohne eine Datenvorlage")

Die `Person` Objekt überschreiben, kann die `ToString` Methode, um aussagekräftige Daten anzuzeigen, wie im folgenden Codebeispiel gezeigt:

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

Dadurch wird die [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Anzeigen der `Person.Name` Eigenschaftswert für jedes Objekt in der Auflistung, wie in den folgenden Screenshots dargestellt:

![](introduction-images/override-tostring.png "ListView mit einer Datenvorlage")

Die `Person.ToString` Außerkraftsetzung zurückgeben eine formatierte Zeichenfolge besteht der `Name`, `Age`, und `Location` Eigenschaften. Dieser Ansatz bietet jedoch nur eine eingeschränkte Kontrolle über die Darstellung der einzelnen Elemente der Daten. Zur Erhöhung der Flexibilität eine [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) können erstellt werden, die die Darstellung der Daten definiert.

## <a name="creating-a-datatemplate"></a>Erstellen einer DataTemplate

Ein [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) verwendet, um die Darstellung der Daten anzugeben, und in der Regel die Datenbindung zum Anzeigen von Daten verwendet. Die allgemeinen Verwendungsszenario ist beim Anzeigen von Daten aus einer Auflistung von Objekten in einer [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Beispielsweise, wenn eine `ListView` gebunden ist, um eine Auflistung von `Person` Objekte, die `ListView.ItemTemplate` -Eigenschaftensatz auf eine `DataTemplate` , definiert die Darstellung der einzelnen `Person` -Objekt in der `ListView`. Die `DataTemplate` enthält Elemente, die Eigenschaftswerte jedes binden `Person` Objekt. Weitere Informationen zur Datenbindung finden Sie unter [Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) (Datenbindungsgrundlagen).

Ein [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) können als Wert für die folgenden Eigenschaften verwendet werden:

- [`ListView.HeaderTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.HeaderTemplate/)
- [`ListView.FooterTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.FooterTemplate/)
- [`ListView.GroupHeaderTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupHeaderTemplate/)
- [`ItemsView.ItemTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/), die von geerbt wird [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/).
- [`MultiPage.ItemTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiPage%3CT%3E/), die von geerbt wird [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/), [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/), und [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/).

> [!NOTE]
> Beachten Sie, dass, obwohl die [ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) macht Verwendungen von [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) Objekte aufweist, werden nicht verwendet eine [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/). Da datenbindungen immer direkt auf festgelegt werden `Cell` Objekte.

Ein [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) dar, das als direkt untergeordnetes Element von den oben aufgeführten Eigenschaften sogenannten platziert ist ein *Inlinevorlage*. Alternativ können Sie eine `DataTemplate` kann als eine Ressource Steuerungsebene auf Seitenebene oder Anwendungsebene definiert werden. Auswählen des Installationsorts für das Definieren einer [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) wirkt sich darauf aus, wo sie verwendet werden kann:

- Ein [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) definiert das Steuerelement kann Ebene nur auf das Steuerelement angewendet werden.
- Ein [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) definiert auf der Seite Ebene auf mehrere gültige Steuerelemente auf der Seite angewendet werden kann.
- Ein [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) auf Anwendungsebene definierten gültigen angewendeten Steuerelemente in der gesamten Anwendung werden können.

Weiter unten in der Hierarchie anzeigen Datenvorlagen haben Vorrang vor den definierten höher einrichten, bei der Freigabe `x:Key` Attribute. Z. B. eine Datenvorlage auf Anwendungsebene wird durch eine Datenvorlage auf Seitenebene überschrieben werden, und eine Datenvorlage auf Seitenebene wird durch eine Datenvorlage Steuerungsebene oder die Vorlage für eine Inline-Daten überschrieben werden.


## <a name="related-links"></a>Verwandte Links

- [Darstellung der Zelle](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [Datenvorlagen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
- [DataTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)
