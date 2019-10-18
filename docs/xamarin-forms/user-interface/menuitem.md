---
title: Xamarin. Forms (MenuItem)
description: Die MenuItem-Klasse wird zum Erstellen von Menü Elementen für Menüs wie ListView-Element Kontextmenüs und shellanwendungsflyout-Menüs verwendet.
ms.prod: xamarin
ms.assetId: 62655C21-6053-466D-A7F4-DE2BE36538F5
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 08/01/2019
ms.openlocfilehash: 5bc36f03eac4ced7c19a0053dfea93dbe2ca4497
ms.sourcegitcommit: 850dd7a3ed10eb3f66692e765d3e31438cff0288
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72531009"
---
# <a name="xamarinforms-menuitem"></a>Xamarin. Forms (MenuItem)

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-menuitemdemos/)

Die [`MenuItem`](xref:Xamarin.Forms.MenuItem) Klasse xamarin. Forms definiert Menü Elemente für Menüs wie `ListView` Element Kontextmenüs und shellanwendungsflyout-Menüs.

Die folgenden Screenshots zeigen `MenuItem` Objekte in einem `ListView`-Kontextmenü unter IOS und Android:

[!["MenuItems unter IOS und Android"](menuitem-images/menuitem-demo-cropped.png "MenuItems unter IOS und Android")](menuitem-images/menuitem-demo-full.png#lightbox "MenuItems unter IOS und Android full image")

Die `MenuItem`-Klasse definiert die folgenden Eigenschaften:

* [`Command`](xref:Xamarin.Forms.MenuItem.Command) ist ein `ICommand`, der die Bindung von Benutzeraktionen, wie z. b. Finger Tippen oder Klicks, auf Befehle ermöglicht, die für ein ViewModel definiert sind.
* [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter) ist ein `object`, der den Parameter angibt, der an die `Command` übergeben werden soll.
* [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource) ist ein `ImageSource` Wert, der das Anzeige Symbol definiert.
* [`IsDestructive`](xref:Xamarin.Forms.MenuItem.IsDestructive) ist ein `bool` Wert, der angibt, ob der `MenuItem` das zugehörige UI-Element aus der Liste entfernt.
* [`IsEnabled`](xref:Xamarin.Forms.MenuItem.IsEnabled) ist ein `bool` Wert, der bestimmt, ob dieses Objekt auf Benutzereingaben antwortet.
* [`Text`](xref:Xamarin.Forms.MenuItem.Text) ist ein `string` Wert, der den Anzeige Text angibt.

Diese Eigenschaften werden von [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Objekten gestützt, sodass die `MenuItem` Instanz das Ziel von Daten Bindungen sein kann.

## <a name="create-a-menuitem"></a>Erstellen eines MENUITEM

`MenuItem` Objekte können in einem Kontextmenü für die Elemente eines `ListView` Objekts verwendet werden. Das gängigste Muster besteht darin, `MenuItem` Objekte innerhalb einer `ViewCell` Instanz zu erstellen, die als `DataTemplate` Objekt für die `ListView`s `ItemTemplate` verwendet wird. Wenn das `ListView` Objekt aufgefüllt wird, wird jedes Element mithilfe der `DataTemplate` erstellt, wobei die `MenuItem` Optionen verfügbar gemacht werden, wenn das Kontextmenü für ein Element aktiviert wird.

Das folgende Beispiel zeigt `MenuItem` Instanziierung innerhalb des Kontexts eines `ListView` Objekts:

```xaml
<ListView>
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
                <ViewCell.ContextActions>
                    <MenuItem Text="Context Menu Option" />
                </ViewCell.ContextActions>
                <Label Text="{Binding .}" />
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Eine `MenuItem` kann auch im Code erstellt werden:

```csharp
// A function returns a ViewCell instance that
// is used as the template for each list item
DataTemplate dataTemplate = new DataTemplate(() =>
{
    // A Label displays the list item text
    Label label = new Label();
    label.SetBinding(Label.TextProperty, ".");

    // A ViewCell serves as the DataTemplate
    ViewCell viewCell = new ViewCell
    {
        View = label
    };

    // Add a MenuItem instance to the ContextActions
    MenuItem menuItem = new MenuItem
    {
        Text = "Context Menu Option"
    };
    viewCell.ContextActions.Add(menuItem);

    // The function returns the custom ViewCell
    // to the DataTemplate constructor
    return viewCell;
});

// Finally, the dataTemplate is provided to
// the ListView object
ListView listView = new ListView
{
    ...
    ItemTemplate = dataTemplate
};
```

## <a name="define-menuitem-behavior-with-events"></a>Definieren von MenuItem-Verhalten mit Ereignissen

Die `MenuItem`-Klasse macht ein `Clicked`-Ereignis verfügbar. Ein Ereignishandler kann an dieses Ereignis angefügt werden, um auf die `MenuItem` Instanz in XAML zu reagieren:

```xaml
<MenuItem ...
          Clicked="OnItemClicked" />
```

Ein Ereignishandler kann auch im Code angefügt werden:

```csharp
MenuItem item = new MenuItem { ... }
item.Clicked += OnItemClicked;
```

In den vorherigen Beispielen wurde auf einen `OnItemClicked` Ereignishandler verwiesen. Der folgende Code zeigt eine Beispiel Implementierung:

```csharp
void OnItemClicked(object sender, EventArgs e)
{
    // The sender is the menuItem
    MenuItem menuItem = sender as MenuItem;

    // Access the list item through the BindingContext
    var contextItem = menuItem.BindingContext;

    // Do something with the contextItem here
}
```

## <a name="define-menuitem-behavior-with-mvvm"></a>Definieren von MenuItem-Verhalten mit MVVM

Die `MenuItem`-Klasse unterstützt das Model-View-ViewModel (MVVM)-Muster über [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekte und die `ICommand`-Schnittstelle. Der folgende XAML-Code zeigt `MenuItem` Instanzen, die an die für ein ViewModel definierten Befehle gebunden sind:

```xaml
<ContentPage.BindingContext>
    <viewmodels:ListPageViewModel />
</ContentPage.BindingContext>

<StackLayout>
    <Label Text="{Binding Message}" ... />
    <ListView ItemsSource="{Binding Items}">
        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <ViewCell.ContextActions>
                        <MenuItem Text="Edit"
                                    IconImageSource="icon.png"
                                    Command="{Binding Source={x:Reference contentPage}, Path=BindingContext.EditCommand}"
                                    CommandParameter="{Binding .}"/>
                        <MenuItem Text="Delete"
                                    Command="{Binding Source={x:Reference contentPage}, Path=BindingContext.DeleteCommand}"
                                    CommandParameter="{Binding .}"/>
                    </ViewCell.ContextActions>
                    <Label Text="{Binding .}" />
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</StackLayout>
```

Im vorherigen Beispiel werden zwei `MenuItem` Objekte mit ihrer `Command` und `CommandParameter` Eigenschaften definiert, die an Befehle für das ViewModel gebunden sind. Das ViewModel enthält die Befehle, auf die in XAML verwiesen wird:

```csharp
public class ListPageViewModel : INotifyPropertyChanged
{
    ...

    public ICommand EditCommand => new Command<string>((string item) =>
    {
        Message = $"Edit command was called on: {item}";
    });

    public ICommand DeleteCommand => new Command<string>((string item) =>
    {
        Message = $"Delete command was called on: {item}";
    });
}
```

Die Beispielanwendung enthält eine `DataService` Klasse, die verwendet wird, um eine Liste von Elementen zum Auffüllen der `ListView` Objekte zu erhalten. Ein ViewModel wird mit Elementen aus der `DataService`-Klasse instanziiert und als `BindingContext` im Code Behind festgelegt:

```csharp
public MenuItemXamlMvvmPage()
{
    InitializeComponent();
    BindingContext = new ListPageViewModel(DataService.GetListItems());
}
```

## <a name="menuitem-icons"></a>MenuItem-Symbole

> [!WARNING]
> `MenuItem` Objekten werden nur Symbole unter Android angezeigt. Auf anderen Plattformen wird nur der von der `Text`-Eigenschaft angegebene Text angezeigt.

 Symbole werden mithilfe der `IconImageSource`-Eigenschaft angegeben. Wenn ein Symbol angegeben wird, wird der durch die `Text`-Eigenschaft angegebene Text nicht angezeigt. Der folgende Screenshot zeigt eine `MenuItem` mit einem Symbol unter Android:

!["Screenshot des MenuItem-Symbols unter Android"](menuitem-images/menuitem-android-icon.png "Screenshot des MenuItem-Symbols unter Android")

Weitere Informationen zur Verwendung von Bildern in xamarin. Forms finden Sie unter [Bilder in xamarin. Forms](~/xamarin-forms/user-interface/images.md).

## <a name="cross-platform-context-menu-behavior"></a>Plattformübergreifendes Kontextmenü Verhalten

Kontextmenüs werden auf jeder Plattform auf unterschiedliche Weise aufgerufen und angezeigt.

Unter Android wird das Kontextmenü durch Long-Press für ein Listenelement aktiviert. Das Kontextmenü ersetzt den Bereich Titel und Navigationsleiste, und `MenuItem` Optionen werden als horizontale Schaltflächen angezeigt.

!["Screenshot des Kontextmenüs unter Android"](menuitem-images/menuitem-android-icon.png "Screenshot des Kontextmenüs unter Android")

Unter IOS wird das Kontextmenü durch Schwenken auf ein Listenelement aktiviert. Das Kontextmenü wird auf dem Listenelement angezeigt, und `MenuItems` als horizontale Schaltflächen angezeigt.

!["Screenshot des Kontextmenüs unter IOS"](menuitem-images/menuitem-ios-contextmenu.png "Screenshot des Kontextmenüs unter IOS")

Bei UWP wird das Kontextmenü aktiviert, indem mit der rechten Maustaste auf ein Listenelement geklickt wird. Das Kontextmenü wird in der Nähe des Cursors als vertikale Liste angezeigt.

!["Screenshot des Kontextmenüs auf der UWP"](menuitem-images/menuitem-uwp.png "Screenshot des Kontextmenüs auf der UWP")

## <a name="related-links"></a>Verwandte Links

* [MenuItem-Demos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-menuitemdemos/)
* [Bilder in xamarin. Forms](~/xamarin-forms/user-interface/images.md)
