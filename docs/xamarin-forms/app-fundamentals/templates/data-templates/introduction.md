---
title: Einführung in Xamarin.Forms-Datenvorlagen
description: Mit Xamarin.Forms-Datenvorlagen können Sie die Darstellung von Daten in unterstützten Steuerelementen definieren. In diesem Artikel werden Datenvorlagen grundlegend vorgestellt, und es wird erläutert, warum sie nötig sind.
ms.prod: xamarin
ms.assetid: 4ED4ACF4-BE4A-44ED-8EAF-C03947B8663B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 10bba38de1dc8908ad853d5e4ca2bb845b4ac8c6
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "70771283"
---
# <a name="introduction-to-xamarinforms-data-templates"></a>Einführung in Xamarin.Forms-Datenvorlagen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-datatemplates)

_Mit Xamarin.Forms-Datenvorlagen können Sie die Darstellung von Daten in unterstützten Steuerelementen definieren. In diesem Artikel werden Datenvorlagen grundlegend vorgestellt, und es wird erläutert, warum sie nötig sind._

Nehmen wir an, Sie haben eine [`ListView`](xref:Xamarin.Forms.ListView)-Klasse, die eine Sammlung von `Person`-Objekten anzeigt. Das folgende Codebeispiel veranschaulicht die Definition der `Person`-Klasse:

```csharp
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
    public string Location { get; set; }
}
```

Die `Person`-Klasse definiert die Eigenschaften `Name`, `Age` und `Location`, die Sie beim Erstellen eines `Person`-Objekts festlegen können. Die [`ListView`](xref:Xamarin.Forms.ListView)-Klasse zeigt die Sammlung der `Person`-Objekte wie in diesem XAML-Codebeispiel veranschaulicht:

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

Sie können der [`ListView`](xref:Xamarin.Forms.ListView)-Klasse in XAML Elemente hinzufügen, indem Sie die Eigenschaft [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) aus einem Array von `Person`-Instanzen heraus initialisieren.

> [!NOTE]
> Bedenken Sie, dass Sie für das Element `x:Array` ein `Type`-Attribute benötigen, das die Elementtypen im Array angibt.

Die entsprechende C#-Seite ist im folgenden Codebeispiel enthalten, das die Eigenschaft [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) für eine Liste (`List`) von`Person`-Instanzen initialisiert:

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

Die [`ListView`](xref:Xamarin.Forms.ListView)-Klasse ruft beim Anzeigen der Sammlungsobjekte `ToString` auf. Da `Person.ToString` nicht außer Kraft gesetzt wird, wird für `ToString` der Typname jedes einzelnen Objekts zurückgegeben, wie in den folgenden Screenshots gezeigt:

![](introduction-images/no-data-template.png "ListView ohne Datenvorlage")

Wie im folgenden Codebeispiel gezeigt, kann das Objekt `Person` die `ToString`-Methode überschreiben, damit aussagekräftige Daten angezeigt werden:

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

Wie im nachfolgenden Screenshot zu sehen, zeigt dadurch das Element [`ListView`](xref:Xamarin.Forms.ListView) für jedes Objekt in der Sammlung den Wert der Eigenschaft `Person.Name` an:

![](introduction-images/override-tostring.png "ListView mit einer Datenvorlage")

Durch die Außerkraftsetzung von `Person.ToString` kann eine formatierte Zeichenfolge zurückgegeben werden, die aus den Eigenschaften `Name`, `Age` und `Location` besteht. So haben Sie jedoch nur wenig Kontrolle über die Darstellung der einzelnen Daten. Für mehr Flexibilität können Sie eine [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Klasse erstellen, die die Darstellung der Daten definiert.

## <a name="creating-a-datatemplate"></a>Erstellen einer Datenvorlage

Mit einer [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Klasse legen Sie die Darstellung der Daten fest. Zum Anzeigen der Daten wird für gewöhnlich eine Datenbindung verwendet. Diese Klasse findet häufig Anwendung, wenn Daten aus einer Objektsammlung einer [`ListView`](xref:Xamarin.Forms.ListView)-Klasse angezeigt werden sollen. Ein mögliches Szenario wäre eine `ListView`-Klasse, die an eine Sammlung von `Person`-Objekten gebunden ist. Die `ListView.ItemTemplate`-Eigenschaft wird auf eine `DataTemplate`-Klasse festgelegt, die die Darstellung der einzelnen `Person`-Objekte in der `ListView`-Klasse definiert. Die `DataTemplate`-Klasse enthält Elemente, die an die Eigenschaftswerte der einzelnen `Person` Objekte gebunden sind. Weitere Informationen zur Datenbindung finden Sie unter [Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) (Datenbindungsgrundlagen).

Eine [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Klasse kann als Wert für die folgenden Eigenschaften verwendet werden:

- [`ListView.HeaderTemplate`](xref:Xamarin.Forms.ListView.HeaderTemplate)
- [`ListView.FooterTemplate`](xref:Xamarin.Forms.ListView.FooterTemplate)
- [`ListView.GroupHeaderTemplate`](xref:Xamarin.Forms.ListView.GroupHeaderTemplate)
- [`ItemsView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1), von der [`ListView`](xref:Xamarin.Forms.ListView) erbt
- [`MultiPage.ItemTemplate`](xref:Xamarin.Forms.MultiPage`1), von der [`CarouselPage`](xref:Xamarin.Forms.CarouselPage), [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) und [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) erben

> [!NOTE]
> Bedenken Sie Folgendes: Obwohl das [`TableView`](xref:Xamarin.Forms.TableView)-Element [`Cell`](xref:Xamarin.Forms.Cell)-Objekte verwendet, wird kein [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Element verwendet Das liegt daran, dass Datenbindungen immer direkt bei `Cell`-Objekten festgelegt werden.

Eine [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Klasse, die den oben aufgelisteten Eigenschaften direkt untergeordnet ist, wird als *Inlinevorlage* bezeichnet. Sie können eine `DataTemplate`-Klasse aber auch als Ressource auf Steuerelement-, Seiten- oder Anwendungsebene definieren. Die Entscheidung, wo Sie eine [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Klasse definieren, hat Einfluss darauf, wo Sie sie verwenden können:

- Eine auf Steuerelementebene definierte [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Klasse kann nur auf ein bestimmtes Steuerelement angewendet werden.
- Eine auf Seitenebene definierte [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Klasse kann auf mehrere gültige Steuerelemente auf einer Seite angewendet werden.
- Eine auf Anwendungsebene definierte [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Klasse können Sie für alle gültigen Steuerelemente einer Anwendung nutzen.

Datenvorlagen, die weiter unten in der Hierarchie der Ansichten angeordnet sind, haben Vorrang vor denjenigen, die sich weiter oben befinden. Voraussetzung ist, dass gemeinsame `x:Key`-Attribute vorliegen. So gilt z. B. Folgendes: Eine Datenvorlage auf Anwendungsebene wird von einer Datenvorlage auf Seitenebene außer Kraft gesetzt. Die Datenvorlage auf Seitenebene wiederum kann durch eine Datenvorlage auf Steuerelementebene oder eine Inlinedatenvorlage außer Kraft gesetzt werden.

## <a name="related-links"></a>Verwandte Links

- [Darstellung von Zellen](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [Data Templates (Datenvorlagen (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-datatemplates)
- [DataTemplate](xref:Xamarin.Forms.DataTemplate)
