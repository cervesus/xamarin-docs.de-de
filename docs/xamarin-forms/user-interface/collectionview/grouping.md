---
Title: " Xamarin.Forms CollectionView-Gruppierung" Beschreibung: "CollectionView kann ordnungsgemäß gruppierte Daten anzeigen, indem die isgroupeigenschaft auf true festgelegt wird."
ms. Prod: xamarin ms. assetid: 7e494245-bdbd-49d6-b7fa-cef976eb59bb ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 09/17/2019 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-collectionview-grouping"></a>Xamarin.FormsCollectionView-Gruppierung

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

Große Datasets können häufig zu unhandlich werden, wenn Sie in einer ständig scrollliste angezeigt werden. In diesem Szenario kann die Organisation der Daten in Gruppen die Benutzerfreundlichkeit verbessern, indem die Navigation in den Daten vereinfacht wird.

[`CollectionView`](xref:Xamarin.Forms.CollectionView)unterstützt das Anzeigen von gruppierten Daten und definiert die folgenden Eigenschaften, die Steuern, wie diese angezeigt werden:

- `IsGrouped`gibt mit dem Typ an `bool` , ob die zugrunde liegenden Daten in Gruppen angezeigt werden sollen. Der Standardwert dieser Eigenschaft ist `false`.
- `GroupHeaderTemplate`, vom Typ [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , die Vorlage, die für den Header jeder Gruppe verwendet werden soll.
- `GroupFooterTemplate`, vom Typ [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , die Vorlage, die für die Fußzeile der einzelnen Gruppen verwendet werden soll.

Diese Eigenschaften werden von- [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Objekten unterstützt. Dies bedeutet, dass die Eigenschaften Ziele von Daten Bindungen sein können.

Die folgenden Screenshots zeigen, wie [`CollectionView`](xref:Xamarin.Forms.CollectionView) gruppierte Daten angezeigt werden:

[![Screenshot von gruppierten Daten in einer CollectionView unter IOS und Android](grouping-images/grouped-data.png "CollectionView mit gruppierten Daten")](grouping-images/grouped-data-large.png#lightbox "CollectionView mit gruppierten Daten")

Weitere Informationen zu Datenvorlagen finden Sie unter [Xamarin.Forms-Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="group-data"></a>Gruppieren von Daten

Die Daten müssen gruppiert werden, bevor Sie angezeigt werden können. Dies kann erreicht werden, indem eine Liste von Gruppen erstellt wird, wobei jede Gruppe eine Liste von Elementen ist. Die Liste der Gruppen sollte eine- `IEnumerable<T>` Sammlung sein, in der `T` zwei Datenelemente definiert:

- Ein Gruppenname.
- Eine-Auflistung `IEnumerable` , die die Elemente definiert, die zur Gruppe gehören.

Der Prozess zum Gruppieren von Daten ist daher:

- Erstellen Sie einen Typ, der ein einzelnes Element modelliert.
- Erstellen Sie einen Typ, der eine einzelne Gruppe von Elementen modelliert.
- Erstellen Sie eine-Auflistung `IEnumerable<T>` , wobei `T` der Typ ist, der eine einzelne Gruppe von Elementen modelliert. Diese Sammlung ist daher eine Auflistung von Gruppen, in der die gruppierten Daten gespeichert werden.
- Fügen Sie der Auflistung Daten hinzu `IEnumerable<T>` .

### <a name="example"></a>Beispiel

Beim Gruppieren von Daten besteht der erste Schritt darin, einen Typ zu erstellen, der ein einzelnes Element modelliert. Das folgende Beispiel zeigt die- `Animal` Klasse aus der Beispielanwendung:

```csharp
public class Animal
{
    public string Name { get; set; }
    public string Location { get; set; }
    public string Details { get; set; }
    public string ImageUrl { get; set; }
}
```

Die- `Animal` Klasse modelliert ein einzelnes Element. Ein Typ, der eine Gruppe von Elementen modelliert, kann dann erstellt werden. Das folgende Beispiel zeigt die- `AnimalGroup` Klasse aus der Beispielanwendung:

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

Die `AnimalGroup` -Klasse erbt von der `List<T>` -Klasse und fügt eine `Name` Eigenschaft hinzu, die den Gruppennamen darstellt.

Eine Auflistung `IEnumerable<T>` von Gruppen kann dann erstellt werden:

```csharp
public List<AnimalGroup> Animals { get; private set; } = new List<AnimalGroup>();
```

Dieser Code definiert eine Auflistung `Animals` mit dem Namen, wobei jedes Element in der Auflistung ein `AnimalGroup` Objekt ist. Jedes `AnimalGroup` -Objekt enthält einen Namen und eine Auflistung `List<Animal>` , die die `Animal` Objekte in der Gruppe definiert.

Gruppierte Daten können dann der Sammlung hinzugefügt werden `Animals` :

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
        ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
    },
    new Animal
    {
        Name = "Capuchin Monkey",
        Location = "Central & South America",
        Details = "Details about the monkey go here.",
        ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/4/40/Capuchin_Costa_Rica.jpg/200px-Capuchin_Costa_Rica.jpg"
    },
    new Animal
    {
        Name = "Blue Monkey",
        Location = "Central and East Africa",
        Details = "Details about the monkey go here.",
        ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/8/83/BlueMonkey.jpg/220px-BlueMonkey.jpg"
    },
    // ...
}));
```

Mit diesem Code werden zwei Gruppen in der-Auflistung erstellt `Animals` . Der erste `AnimalGroup` hat den Namen `Bears` und enthält eine Auflistung `List<Animal>` von "Bear Details". Die zweite `AnimalGroup` hat den Namen `Monkeys` und enthält eine Auflistung `List<Animal>` von affendetails.

## <a name="display-grouped-data"></a>Anzeigen von gruppierten Daten

[`CollectionView`](xref:Xamarin.Forms.CollectionView)zeigt die gruppierten Daten an, sofern die Daten ordnungsgemäß gruppiert wurden, indem die-Eigenschaft auf festgelegt wird `IsGrouped` `true` :

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

Der entsprechende C#-Code lautet:

```csharp
CollectionView collectionView = new CollectionView
{
    IsGrouped = true
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
// ...
```

Die Darstellung der einzelnen Elemente in der [`CollectionView`](xref:Xamarin.Forms.CollectionView) wird definiert, indem die-Eigenschaft auf festgelegt wird [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) . Weitere Informationen finden Sie unter [Definieren der Element](~/xamarin-forms/user-interface/collectionview/populate-data.md#define-item-appearance)Darstellung.

> [!NOTE]
> Standardmäßig [`CollectionView`](xref:Xamarin.Forms.CollectionView) zeigt den Gruppennamen in der Gruppen Kopfzeile und-Fußzeile an. Dieses Verhalten kann geändert werden, indem Sie die Gruppen Kopfzeile und die Fußzeile anpassen.

## <a name="customize-the-group-header"></a>Anpassen der Gruppen Kopfzeile

Die Darstellung der einzelnen Gruppen Kopfzeilen kann angepasst werden, indem die-Eigenschaft auf festgelegt wird `CollectionView.GroupHeaderTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) :

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

In diesem Beispiel wird jeder Gruppen Header auf einen festgelegt, [`Label`](xref:Xamarin.Forms.Label) der den Gruppennamen anzeigt und andere Darstellungs Eigenschaften haben. Die folgenden Screenshots zeigen den angepassten Gruppen Header:

[![Screenshot eines angepassten Gruppen Headers in einer CollectionView unter IOS und Android](grouping-images/customized-header.png "CollectionView mit angepassten Gruppen Kopfzeile")](grouping-images/customized-header-large.png#lightbox "CollectionView mit angepassten Gruppen Kopfzeile")

## <a name="customize-the-group-footer"></a>Anpassen der Gruppen Fußzeile

Die Darstellung der einzelnen Gruppen Fußzeilen kann angepasst werden, indem die-Eigenschaft auf festgelegt wird `CollectionView.GroupFooterTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) :

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

In diesem Beispiel wird jeder Gruppenfuß auf einen festgelegt, [`Label`](xref:Xamarin.Forms.Label) der die Anzahl der Elemente in der Gruppe anzeigt. Die folgenden Screenshots zeigen die angepasste Gruppen Fußzeile:

[![Screenshot einer angepassten Gruppen Fußzeile in einer CollectionView unter IOS und Android](grouping-images/customized-footer.png "CollectionView mit angepasster Gruppen Fußzeile")](grouping-images/customized-footer-large.png#lightbox "CollectionView mit angepasster Gruppen Fußzeile")

## <a name="empty-groups"></a>Leere Gruppen

Wenn eine [`CollectionView`](xref:Xamarin.Forms.CollectionView) gruppierte Daten anzeigt, werden alle leeren Gruppen angezeigt. Diese Gruppen werden mit einer Gruppen Kopfzeile und-Fußzeile angezeigt, die angibt, dass die Gruppe leer ist. Die folgenden Screenshots zeigen eine leere Gruppe an:

[![Screenshot einer leeren Gruppe in einer CollectionView unter IOS und Android](grouping-images/empty-group.png "CollectionView mit einer leeren Gruppe")](grouping-images/empty-group-large.png#lightbox "CollectionView mit einer leeren Gruppe")

> [!NOTE]
> Unter IOS 10 und niedriger können alle Gruppen Kopfzeilen und-Fußzeilen für leere Gruppen am oberen Rand des angezeigt werden `CollectionView` .

## <a name="group-without-templates"></a>Ohne Vorlagen gruppieren

[`CollectionView`](xref:Xamarin.Forms.CollectionView)kann ordnungsgemäß gruppierte Daten anzeigen, ohne dass die-Eigenschaft auf festgelegt wird [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) :

```xaml
<CollectionView ItemsSource="{Binding Animals}"
                IsGrouped="true" />
```

In diesem Szenario können sinnvolle Daten angezeigt werden, indem die `ToString` -Methode in dem Typ, der ein einzelnes Element modelliert, überschrieben wird, und der Typ, der eine einzelne Gruppe von Elementen modelliert.

## <a name="related-links"></a>Verwandte Links

- [CollectionView (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Xamarin.FormsDatenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
