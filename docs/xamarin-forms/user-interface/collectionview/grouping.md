---
title: Xamarin. Forms CollectionView-Gruppierung
description: CollectionView kann ordnungsgemäß gruppierte Daten anzeigen, indem die isgruppierte Eigenschaft auf true festgelegt wird.
ms.prod: xamarin
ms.assetid: 7E494245-FDBD-49D6-B7FA-CEF976EB59BB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/24/2019
ms.openlocfilehash: 8fd37999428c2813bbf96de3bcbd6ebd1fe0879d
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69894027"
---
# <a name="xamarinforms-collectionview-grouping"></a>Xamarin. Forms CollectionView-Gruppierung

![](~/media/shared/preview.png "Diese API ist derzeit als Vorabversion erhältlich")

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CollectionViewDemos)

Große Datasets können häufig zu unhandlich werden, wenn Sie in einer ständig scrollliste angezeigt werden. In diesem Szenario kann die Organisation der Daten in Gruppen die Benutzerfreundlichkeit verbessern, indem die Navigation in den Daten vereinfacht wird.

[`CollectionView`](xref:Xamarin.Forms.CollectionView)unterstützt das Anzeigen von gruppierten Daten und definiert die folgenden Eigenschaften, die Steuern, wie diese angezeigt werden:

- `IsGrouped`gibt mit dem `bool`Typ an, ob die zugrunde liegenden Daten in Gruppen angezeigt werden sollen. Der Standardwert dieser Eigenschaft ist `false`.
- `GroupHeaderTemplate`, vom Typ [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), die Vorlage, die für den Header jeder Gruppe verwendet werden soll.
- `GroupFooterTemplate`, vom Typ [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), die Vorlage, die für die Fußzeile der einzelnen Gruppen verwendet werden soll.

Diese Eigenschaften werden von [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekten unterstützt. Dies bedeutet, dass die Eigenschaften Ziele von Daten Bindungen sein können.

Weitere Informationen zu Datenvorlagen finden Sie unter [Xamarin.Forms-Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

> [!IMPORTANT]
> Das Gruppieren von [`CollectionView`](xref:Xamarin.Forms.CollectionView) Daten mit wird derzeit nur unter IOS unterstützt.

## <a name="group-data"></a>Gruppieren von Daten

Die Daten müssen gruppiert werden, bevor Sie angezeigt werden können. Dies kann erreicht werden, indem eine Liste von Gruppen erstellt wird, wobei jede Gruppe eine Liste von Elementen ist. Die Liste der Gruppen sollte eine `IEnumerable<T>` -Sammlung sein, in `T` der zwei Datenelemente definiert:

- Ein Gruppenname.
- Eine `IEnumerable` -Auflistung, die die Elemente definiert, die zur Gruppe gehören.

Der Prozess zum Gruppieren von Daten ist daher:

- Erstellen Sie einen Typ, der ein einzelnes Element modelliert.
- Erstellen Sie einen Typ, der eine einzelne Gruppe von Elementen modelliert.
- Erstellen Sie `IEnumerable<T>` eine-Auflistung `T` , wobei der Typ ist, der eine einzelne Gruppe von Elementen modelliert. Diese Sammlung ist daher eine Auflistung von Gruppen, in der die gruppierten Daten gespeichert werden.
- Fügen Sie der `IEnumerable<T>` Auflistung Daten hinzu.

### <a name="example"></a>Beispiel

Beim Gruppieren von Daten besteht der erste Schritt darin, einen Typ zu erstellen, der ein einzelnes Element modelliert. Das folgende Beispiel zeigt die `Animal` -Klasse aus der Beispielanwendung:

```csharp
public class Animal
{
    public string Name { get; set; }
    public string Location { get; set; }
    public string Details { get; set; }
    public string ImageUrl { get; set; }
}
```

Die `Animal` -Klasse modelliert ein einzelnes Element. Ein Typ, der eine Gruppe von Elementen modelliert, kann dann erstellt werden. Das folgende Beispiel zeigt die `AnimalGroup` -Klasse aus der Beispielanwendung:

```csharp
public class AnimalGroup : List<Animal>
{
    public string Name { get; private set; }

    public AnimalGroup(string name, List<Animal> animals) : base(animals)
    {
        Name = name;
    }
}
```

Die `AnimalGroup` -Klasse erbt von `List<T>` der-Klasse und `Name` fügt eine Eigenschaft hinzu, die den Gruppennamen darstellt.

Eine `IEnumerable<T>` Auflistung von Gruppen kann dann erstellt werden:

```csharp
public List<AnimalGroup> Animals { get; private set; } = new List<AnimalGroup>();
```

Dieser Code definiert eine Auflistung mit `Animals`dem Namen, wobei jedes Element in der Auflistung `AnimalGroup` ein Objekt ist. Jedes `AnimalGroup` -Objekt enthält einen Namen und eine `List<Animal>` Auflistung, die die `Animal` Objekte in der Gruppe definiert.

Gruppierte Daten können dann der `Animals` Sammlung hinzugefügt werden:

```csharp
Animals.Add(new AnimalGroup("Bears", new List<Animal>
{
    new Animal
    {
        Name = "American Black Bear",
        Location = "North America",
        Details = "Details about the bear go here.",
        ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/0/08/01_Schwarzbär.jpg"
    },
    new Animal
    {
        Name = "Asian Black Bear",
        Location = "Asia",
        Details = "Details about the bear go here.",
        ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/b/b7/Ursus_thibetanus_3_%28Wroclaw_zoo%29.JPG/180px-Ursus_thibetanus_3_%28Wroclaw_zoo%29.JPG"
    },
    // ...
}));

Animals.Add(new AnimalGroup("Monkeys", new List<Animal>
{
    new Animal
    {
        Name = "Baboon",
        Location = "Africa & Asia",
        Details = "Details about the monkey go here.",
        ImageUrl = "http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
    },
    new Animal
    {
        Name = "Capuchin Monkey",
        Location = "Central & South America",
        Details = "Details about the monkey go here.",
        ImageUrl = "http://upload.wikimedia.org/wikipedia/commons/thumb/4/40/Capuchin_Costa_Rica.jpg/200px-Capuchin_Costa_Rica.jpg"
    },
    new Animal
    {
        Name = "Blue Monkey",
        Location = "Central and East Africa",
        Details = "Details about the monkey go here.",
        ImageUrl = "http://upload.wikimedia.org/wikipedia/commons/thumb/8/83/BlueMonkey.jpg/220px-BlueMonkey.jpg"
    },
    // ...
}));
```

Mit diesem Code werden zwei Gruppen in `Animals` der-Auflistung erstellt. Der erste `AnimalGroup` hat den `Bears`Namen und enthält eine `List<Animal>` Auflistung von "Bear Details". Die zweite `AnimalGroup` hat den `Monkeys`Namen und enthält eine `List<Animal>` Auflistung von affendetails.

## <a name="display-grouped-data"></a>Anzeigen von gruppierten Daten

[`CollectionView`](xref:Xamarin.Forms.CollectionView)zeigt die gruppierten Daten an, sofern die Daten ordnungsgemäß gruppiert wurden, indem die `IsGrouped` -Eigenschaft `true`auf festgelegt wird:

```xaml
<CollectionView ItemsSource="{Binding Animals}"
                IsGrouped="true">
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                ...
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Der entsprechende C#-Code ist:

```csharp
CollectionView collectionView = new CollectionView
{
    IsGrouped = true
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
// ...
```

Die Darstellung der einzelnen Elemente in der [`CollectionView`](xref:Xamarin.Forms.CollectionView) wird definiert, indem die [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) -Eigenschaft auf [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)festgelegt wird. Weitere Informationen finden Sie unter [Definieren der Element](~/xamarin-forms/user-interface/collectionview/populate-data.md#define-item-appearance)Darstellung.

> [!NOTE]
> Standardmäßig [`CollectionView`](xref:Xamarin.Forms.CollectionView) zeigt den Gruppennamen in der Gruppen Kopfzeile und-Fußzeile an. Dieses Verhalten kann geändert werden, indem Sie die Gruppen Kopfzeile und die Fußzeile anpassen.

## <a name="customize-the-group-header"></a>Anpassen der Gruppen Kopfzeile

Die Darstellung der einzelnen Gruppen Kopfzeilen kann angepasst werden `CollectionView.GroupHeaderTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), indem die-Eigenschaft auf festgelegt wird:

```xaml
<CollectionView ItemsSource="{Binding Animals}"
                IsGrouped="true">
    ...
    <CollectionView.GroupHeaderTemplate>
        <DataTemplate>
            <Label Text="{Binding Name}"
                   BackgroundColor="LightGray"
                   FontSize="Large"
                   FontAttributes="Bold" />
        </DataTemplate>
    </CollectionView.GroupHeaderTemplate>
</CollectionView>
```

In diesem Beispiel wird jeder Gruppen Header auf einen [`Label`](xref:Xamarin.Forms.Label) festgelegt, der den Gruppennamen anzeigt und andere Darstellungs Eigenschaften haben.

## <a name="customize-the-group-footer"></a>Anpassen der Gruppen Fußzeile

Die Darstellung der einzelnen Gruppen Fußzeilen kann angepasst werden `CollectionView.GroupFooterTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), indem die-Eigenschaft auf festgelegt wird:

```xaml
<CollectionView ItemsSource="{Binding Animals}"
                IsGrouped="true">
    ...
    <CollectionView.GroupFooterTemplate>
        <DataTemplate>
            <Label Text="{Binding Count, StringFormat='Total animals: {0:D}'}"
                   Margin="0,0,0,10" />
        </DataTemplate>
    </CollectionView.GroupFooterTemplate>
</CollectionView>
```

In diesem Beispiel wird jeder Gruppenfuß auf einen [`Label`](xref:Xamarin.Forms.Label) festgelegt, der die Anzahl der Elemente in der Gruppe anzeigt.

## <a name="empty-groups"></a>Leere Gruppen

Wenn eine [`CollectionView`](xref:Xamarin.Forms.CollectionView) gruppierte Daten anzeigt, werden alle leeren Gruppen angezeigt. Diese Gruppen werden mit einer Gruppen Kopfzeile und-Fußzeile angezeigt, die angibt, dass die Gruppe leer ist.

> [!NOTE]
> Unter IOS 10 und niedriger können alle Gruppen Kopfzeilen und-Fußzeilen für leere Gruppen am oberen Rand des `CollectionView`angezeigt werden.

## <a name="group-without-templates"></a>Ohne Vorlagen gruppieren

[`CollectionView`](xref:Xamarin.Forms.CollectionView)kann ordnungsgemäß gruppierte Daten anzeigen [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), ohne dass die-Eigenschaft auf festgelegt wird:

```xaml
<CollectionView ItemsSource="{Binding Animals}"
                IsGrouped="true" />
```

In diesem Szenario können sinnvolle Daten angezeigt werden, indem die `ToString` -Methode in dem Typ, der ein einzelnes Element modelliert, überschrieben wird, und der Typ, der eine einzelne Gruppe von Elementen modelliert.

## <a name="related-links"></a>Verwandte Links

- [CollectionView (Beispiel)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CollectionViewDemos/)
- [Xamarin. Forms-Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
