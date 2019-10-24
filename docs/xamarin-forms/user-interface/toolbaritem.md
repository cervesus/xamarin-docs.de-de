---
title: Xamarin. Forms-ToolBarItem
description: Die ToolBarItem-Klasse ist eine besondere Art von Schaltfläche, die in der Navigationsleiste einer Anwendung verwendet wird.
ms.prod: xamarin
ms.assetId: CC737D54-0280-46BD-A2BC-A0FB67DDD6A1
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/29/2019
ms.openlocfilehash: 0812347e85b0ccb6aa0bbb16649a89bb4d961c9b
ms.sourcegitcommit: a14edebf00f3e0f8944e59042ca7aa5c42173e30
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72780352"
---
# <a name="xamarinforms-toolbaritem"></a>Xamarin. Forms-ToolBarItem

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-toolbaritem/)

Die xamarin. Forms [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) -Klasse ist eine spezielle Schaltfläche, die der `ToolbarItems` Auflistung eines `Page` Objekts hinzugefügt werden kann. Jedes `ToolbarItem` Objekt wird in der Navigationsleiste der Anwendung als Schaltfläche angezeigt. Eine `ToolbarItem` Instanz kann ein Symbol aufweisen, das als primäres oder sekundäres Menü Element angezeigt wird. Die `ToolbarItem`-Klasse erbt von [`MenuItem`](xref:Xamarin.Forms.MenuItem).

Die folgenden Screenshots zeigen `ToolbarItem` Objekte in der Navigationsleiste unter IOS und Android:

!["Bildschirm Abbildung der ToolBarItem-Demo unter Android und IOS"](toolbaritem-images/toolbaritem-device-screenshot.png "Bildschirm Abbildung von ToolBarItem-Demo unter Android und IOS")

Die `ToolbarItem`-Klasse definiert die folgenden Eigenschaften:

* [`Order`](xref:Xamarin.Forms.ToolbarItem.Order) ist ein `ToolbarItemOrder` Enumerationswert, der bestimmt, ob die `ToolbarItem` Instanz im primären oder sekundären Menü angezeigt wird.
* [`Priority`](xref:Xamarin.Forms.ToolbarItem.Priority) ist ein `integer` Wert, der die Anzeigereihenfolge von Elementen in der `ToolbarItems` Auflistung eines `Page` Objekts bestimmt.

Die `ToolbarItem`-Klasse erbt die folgenden normalerweise verwendeten Eigenschaften von der `MenuItem`-Klasse:

* [`Command`](xref:Xamarin.Forms.MenuItem.Command) ist ein `ICommand`, der die Bindung von Benutzeraktionen, wie z. b. Finger Tippen oder Klicks, auf Befehle ermöglicht, die für ein ViewModel definiert sind.
* [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter) ist ein `object`, der den Parameter angibt, der an die `Command` übergeben werden soll.
* [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource) ist ein `ImageSource` Wert, der das Anzeige Symbol auf einem `ToolbarItem`-Objekt bestimmt.
* [`Text`](xref:Xamarin.Forms.MenuItem.Text) ist ein `string`, der den Anzeige Text in einem `ToolbarItem`-Objekt bestimmt.

Diese Eigenschaften werden von [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Objekten gestützt, sodass eine `ToolbarItem` Instanz das Ziel von Daten Bindungen sein kann.

> [!NOTE]
> Eine Alternative zum Erstellen einer Symbolleiste aus [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) Objekten besteht darin, die [`NavigationPage.TitleView`](xref:Xamarin.Forms.NavigationPage.TitleViewProperty) angefügte Eigenschaft auf eine Layoutklasse festzulegen, die mehrere Ansichten enthält. Weitere Informationen finden Sie unter [Anzeigen von Ansichten in der Navigationsleiste](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md#displaying-views-in-the-navigation-bar).

## <a name="create-a-toolbaritem"></a>Erstellen eines ToolBarItem

Ein `ToolbarItem`-Objekt kann in XAML instanziiert werden. Die Eigenschaften `Text` und `IconImageSource` können festgelegt werden, um zu bestimmen, wie die Schaltfläche auf der Navigationsleiste angezeigt wird. Im folgenden Beispiel wird gezeigt, wie eine `ToolbarItem` mit einigen allgemeinen Eigenschaften instanziiert und der `ToolbarItems` Auflistung einer `ContentPage` hinzugefügt wird:

```xaml
<ContentPage.ToolbarItems>
    <ToolbarItem Text="Example Item"
                 IconImageSource="example_icon.png"
                 Order="Primary"
                 Priority="0" />
</ContentPage.ToolbarItems>
```

Dieses Beispiel führt zu einem `ToolbarItem` Objekt, das Text enthält, ein Symbol, das zuerst im primären Navigationsleisten Bereich angezeigt wird. Eine `ToolbarItem` kann auch im Code erstellt und der `ToolbarItems` Auflistung hinzugefügt werden:

```csharp
ToolbarItem item = new ToolbarItem
{
    Text = "Example Item",
    IconImageSource = ImageSource.FromFile("example_icon.png"),
    Order = ToolbarItemOrder.Primary,
    Priority = 0
};

// "this" refers to a Page object
this.ToolbarItems.Add(item);
```

Die Datei, die durch den `string` dargestellt wird, der als `IconImageSource`-Eigenschaft bereitgestellt wird, muss in jedem Platt Form Projekt vorhanden sein.

> [!NOTE]
> Bild Ressourcen werden auf jeder Plattform unterschiedlich behandelt. Eine `ImageSource` kann aus Quellen stammen, einschließlich einer lokalen Datei oder eingebetteten Ressource, eines URIs oder eines Streams. Weitere Informationen zum Festlegen der `IconImageSource` Eigenschaft und der Bilder in xamarin. Forms finden Sie unter [Bilder in xamarin. Forms](~/xamarin-forms/user-interface/images.md).

## <a name="define-button-behavior"></a>Definieren des Schaltflächen Verhaltens

Die `ToolbarItem`-Klasse erbt das `Clicked`-Ereignis von der `MenuItem`-Klasse. Ein Ereignishandler kann an das `Clicked` Ereignis angefügt werden, um auf `ToolbarItem`-Instanzen in XAML auf tippen oder Klicks zu reagieren:

```xaml
<ToolbarItem ...
             Clicked="OnItemClicked" />
```

Ein Ereignishandler kann auch im Code angefügt werden:

```csharp
ToolbarItem item = new ToolbarItem { ... }
item.Clicked += OnItemClicked;
```

In den vorherigen Beispielen wurde auf einen `OnItemClicked` Ereignishandler verwiesen. Der folgende Code zeigt eine Beispiel Implementierung:

```csharp
void OnItemClicked(object sender, EventArgs e)
{
    ToolbarItem item = (ToolbarItem)sender;
    messageLabel.Text = $"You clicked the \"{item.Text}\" toolbar item.";
}
```

`ToolbarItem` Objekte können auch die Eigenschaften `Command` und `CommandParameter` verwenden, um auf Benutzereingaben ohne Ereignishandler zu reagieren. Weitere Informationen zur `ICommand`-Schnittstelle und zur MVVM-Datenbindung finden Sie unter [xamarin. Forms MenuItem MVVM-Verhalten](~/xamarin-forms/user-interface/menuitem.md#define-menuitem-behavior-with-mvvm).

## <a name="primary-and-secondary-menus"></a>Primäre und sekundäre Menüs

Die `ToolbarItemOrder`-Enumeration verfügt über `Default`-, `Primary`-und `Secondary`-Werte.

Wenn die `Order`-Eigenschaft auf `Primary` festgelegt ist, wird das `ToolbarItem`-Objekt auf der Hauptnavigationsleiste auf allen Plattformen angezeigt. `ToolbarItem` Objekte werden über den Seitentitel priorisiert, die abgeschnitten werden, um Platz für die Elemente zu schaffen. Die folgenden Screenshots zeigen `ToolbarItem` Objekte im primären Menü unter IOS und Android:

!["ToolBarItem-Hauptmenü: Screenshot Android und IOS"](toolbaritem-images/toolbaritem-primary-menu.png "Bildschirm Abbildung des primären ToolBarItem-Menüs unter Android und IOS")

Wenn die `Order`-Eigenschaft auf `Secondary` festgelegt ist, variiert das Verhalten plattformübergreifend. Bei UWP und Android wird das Menü `Secondary` Elemente als drei Punkte angezeigt, auf die Sie tippen oder klicken können, um Elemente in einer vertikalen Liste anzuzeigen. Unter IOS wird das Menü `Secondary` Elemente unterhalb der Navigationsleiste als horizontale Liste angezeigt. Die folgenden Screenshots zeigen ein sekundäres Menü unter IOS und Android:

!["ToolBarItem-Menü auf dem sekundären Menü" Android und IOS "](toolbaritem-images/toolbaritem-secondary-menu.png "Bildschirm Abbildung des sekundären ToolBarItem-Menüs unter Android und IOS")

> [!WARNING]
> Das Symbol Verhalten in `ToolbarItem` Objekten, deren `Order`-Eigenschaft auf `Secondary` festgelegt ist, ist plattformübergreifend nicht konsistent. Legen Sie die `IconImageSource`-Eigenschaft nicht für Elemente fest, die im sekundären Menü angezeigt werden.

## <a name="related-links"></a>Verwandte Links

* [ToolBarItem-Demos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-toolbaritem/)
* [Bilder in xamarin. Forms](~/xamarin-forms/user-interface/images.md)
* [Xamarin. Forms (MenuItem)](~/xamarin-forms/user-interface/menuitem.md)
