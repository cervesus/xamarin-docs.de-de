---
title: Xamarin.FormsMENUITEM
description: Die MenuItem-Klasse wird zum Erstellen von Menü Elementen für Menüs wie ListView-Element Kontextmenüs und shellanwendungsflyout-Menüs verwendet.
ms.prod: xamarin
ms.assetId: 62655C21-6053-466D-A7F4-DE2BE36538F5
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 08/01/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: de8c6bff2c9dc72821692708f5852cd874c31ede
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84139203"
---
# <a name="xamarinforms-menuitem"></a>Xamarin.FormsMENUITEM

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-menuitemdemos/)

Die- Xamarin.Forms [`MenuItem`](xref:Xamarin.Forms.MenuItem) Klasse definiert Menü Elemente für Menüs wie `ListView` Element Kontextmenüs und shellanwendungsflyout-Menüs.

Die folgenden Screenshots zeigen `MenuItem` Objekte in einem `ListView` Kontextmenü unter IOS und Android:

[!["MenuItems unter IOS und Android"](menuitem-images/menuitem-demo-cropped.png "MenuItems unter IOS und Android")](menuitem-images/menuitem-demo-full.png#lightbox "MenuItems unter IOS und Android vollständiges Image")

Die- `MenuItem` Klasse definiert die folgenden Eigenschaften:

* [`Command`](xref:Xamarin.Forms.MenuItem.Command)ist eine `ICommand` , die das Binden von Benutzeraktionen (z. b. Finger Tippen oder Klicks) auf Befehle ermöglicht, die für ein ViewModel definiert sind.
* [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter)ist eine `object` , die den Parameter angibt, der an den übergeben werden soll `Command` .
* [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource)ein- `ImageSource` Wert, der das Anzeige Symbol definiert.
* [`IsDestructive`](xref:Xamarin.Forms.MenuItem.IsDestructive)ein `bool` Wert, der angibt, ob das `MenuItem` zugehörige Benutzeroberflächen Element aus der Liste entfernt.
* [`IsEnabled`](xref:Xamarin.Forms.MenuItem.IsEnabled)ein `bool` Wert, der angibt, ob dieses-Objekt auf Benutzereingaben antwortet.
* [`Text`](xref:Xamarin.Forms.MenuItem.Text)ein- `string` Wert, der den Anzeige Text angibt.

Diese Eigenschaften werden von Objekten unterstützt, [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) sodass die `MenuItem` Instanz das Ziel von Daten Bindungen sein kann.

## <a name="create-a-menuitem"></a>Erstellen eines MENUITEM

`MenuItem`-Objekte können in einem Kontextmenü für `ListView` die Elemente eines-Objekts verwendet werden. Das gängigste Muster besteht darin, `MenuItem` Objekte innerhalb einer-Instanz zu erstellen `ViewCell` , die als- `DataTemplate` Objekt für die s verwendet wird `ListView` `ItemTemplate` . Wenn das `ListView` Objekt aufgefüllt wird, wird jedes Element mithilfe von erstellt `DataTemplate` , wobei die Optionen verfügbar gemacht werden, `MenuItem` Wenn das Kontextmenü für ein Element aktiviert wird.

Im folgenden Beispiel wird die `MenuItem` Instanziierung innerhalb des Kontexts eines- `ListView` Objekts veranschaulicht:

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

Ein `MenuItem` kann auch im Code erstellt werden:

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

Die `MenuItem`-Klasse macht ein `Clicked`-Ereignis verfügbar. Ein Ereignishandler kann an dieses Ereignis angefügt werden, um auf die- `MenuItem` Instanz in XAML auf tippen oder Klicks zu reagieren:

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

Die `MenuItem` -Klasse unterstützt das Model-View-ViewModel (MVVM)-Muster durch [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekte und die- `ICommand` Schnittstelle. Der folgende XAML-Code zeigt `MenuItem` Instanzen an, die an in einem ViewModel definierten Befehlen gebunden sind:

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

Im vorherigen Beispiel `MenuItem` werden zwei-Objekte mit Ihren `Command` -und- `CommandParameter` Eigenschaften definiert, die an Befehle für das ViewModel gebunden sind. Das ViewModel enthält die Befehle, auf die in XAML verwiesen wird:

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

Die Beispielanwendung enthält eine- `DataService` Klasse, die verwendet wird, um eine Liste von Elementen zum Auffüllen der-Objekte zu erhalten `ListView` . Ein "ViewModel" wird mit Elementen aus der-Klasse instanziiert `DataService` und als `BindingContext` im Code Behind festgelegt:

```csharp
public MenuItemXamlMvvmPage()
{
    InitializeComponent();
    BindingContext = new ListPageViewModel(DataService.GetListItems());
}
```

## <a name="menuitem-icons"></a>MenuItem-Symbole

> [!WARNING]
> `MenuItem`in Objekten werden nur Symbole unter Android angezeigt. Auf anderen Plattformen wird nur der von der- `Text` Eigenschaft angegebene Text angezeigt.

 Symbole werden mithilfe der- `IconImageSource` Eigenschaft angegeben. Wenn ein Symbol angegeben wird, wird der von der- `Text` Eigenschaft angegebene Text nicht angezeigt. Der folgende Screenshot zeigt ein- `MenuItem` Symbol mit einem Symbol unter Android:

!["Screenshot des MenuItem-Symbols unter Android"](menuitem-images/menuitem-android-icon.png "Screenshot des MenuItem-Symbols unter Android")

Weitere Informationen zur Verwendung von Bildern in Xamarin.Forms finden Sie unter [Bilder Xamarin.Forms in ](~/xamarin-forms/user-interface/images.md).

## <a name="enable-or-disable-a-menuitem-at-runtime"></a>Aktivieren oder Deaktivieren eines MenuItem zur Laufzeit

Um das Deaktivieren von zur `MenuItem` Laufzeit zu aktivieren, binden Sie seine- `Command` Eigenschaft an eine `ICommand` -Implementierung, und stellen Sie sicher, dass ein Delegat den ggf `canExecute` . aktiviert und deaktiviert `ICommand` .

> [!IMPORTANT]
> Binden Sie die- `IsEnabled` Eigenschaft nicht an eine andere Eigenschaft, wenn Sie die- `Command` Eigenschaft zum Aktivieren oder Deaktivieren von verwenden `MenuItem` .

Das folgende Beispiel zeigt eine `MenuItem` , deren- `Command` Eigenschaft an einen mit dem Namen gebunden ist `ICommand` `MyCommand` :

```xaml
<MenuItem Text="My menu item"
          Command="{Binding MyCommand}" />
```

Die- `ICommand` Implementierung erfordert einen Delegaten `canExecute` , der den Wert einer Eigenschaft zurückgibt `bool` , um die zu aktivieren und zu deaktivieren `MenuItem` :

```csharp
public class MyViewModel : INotifyPropertyChanged
{
    bool isMenuItemEnabled = false;
    public bool IsMenuItemEnabled
    {
        get { return isMenuItemEnabled; }
        set
        {
            isMenuItemEnabled = value;
            MyCommand.ChangeCanExecute();
        }
    }

    public Command MyCommand { get; private set; }

    public ToolbarItemViewModel()
    {
        MyCommand = new Command(() =>
        {
            // Execute logic here
        },
        () => IsToolbarItemEnabled);
    }
}
```

In diesem Beispiel wird der `MenuItem` deaktiviert, bis die- `IsMenuItemEnabled` Eigenschaft festgelegt ist. Wenn dies auftritt, wird die- `Command.ChangeCanExecute` Methode aufgerufen, die bewirkt, dass der Delegat `canExecute` `MyCommand` erneut ausgewertet wird.

## <a name="cross-platform-context-menu-behavior"></a>Plattformübergreifendes Kontextmenü Verhalten

Kontextmenüs werden auf jeder Plattform auf unterschiedliche Weise aufgerufen und angezeigt.

Unter Android wird das Kontextmenü durch Long-Press für ein Listenelement aktiviert. Das Kontextmenü ersetzt den Bereich für Titel und Navigationsleiste, und die `MenuItem` Optionen werden als horizontale Schaltflächen angezeigt.

!["Screenshot des Kontextmenüs unter Android"](menuitem-images/menuitem-android-icon.png "Screenshot des Kontextmenüs unter Android")

Unter IOS wird das Kontextmenü durch Schwenken auf ein Listenelement aktiviert. Das Kontextmenü wird auf dem Listenelement angezeigt und `MenuItems` als horizontale Schaltflächen angezeigt.

!["Screenshot des Kontextmenüs unter IOS"](menuitem-images/menuitem-ios-contextmenu.png "Screenshot des Kontextmenüs unter IOS")

Bei UWP wird das Kontextmenü aktiviert, indem mit der rechten Maustaste auf ein Listenelement geklickt wird. Das Kontextmenü wird in der Nähe des Cursors als vertikale Liste angezeigt.

!["Screenshot des Kontextmenüs auf der UWP"](menuitem-images/menuitem-uwp.png "Screenshot des Kontextmenüs auf der UWP")

## <a name="related-links"></a>Verwandte Links

* [MenuItem-Demos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-menuitemdemos/)
* [Bilder inXamarin.Forms](~/xamarin-forms/user-interface/images.md)
