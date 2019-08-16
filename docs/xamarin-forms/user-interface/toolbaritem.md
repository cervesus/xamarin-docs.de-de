---
title: Xamarin. Forms-ToolBarItem
description: Die ToolBarItem-Klasse ist eine besondere Art von Schaltfläche, die in der Navigationsleiste einer Anwendung verwendet wird.
ms.prod: xamarin
ms.assetId: CC737D54-0280-46BD-A2BC-A0FB67DDD6A1
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/29/2019
ms.openlocfilehash: b42a300d9d76a18322891856486720116eb6a8d4
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69522028"
---
# <a name="xamarinforms-toolbaritem"></a>Xamarin. Forms-ToolBarItem

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/en-us/samples/xamarin/xamarin-forms-samples/userinterface-toolbaritem/)

Die xamarin. Forms [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) -Klasse ist ein spezieller Typ von Schaltfläche, der der Auflistung `Page` eines- `ToolbarItems` Objekts hinzugefügt werden kann. Jedes `ToolbarItem` Objekt wird in der Navigationsleiste der Anwendung als Schaltfläche angezeigt. Eine `ToolbarItem` -Instanz kann ein Symbol aufweisen und als primäres oder sekundäres Menü Element angezeigt werden. Die `ToolbarItem` Klasse erbt von [`MenuItem`](xref:Xamarin.Forms.MenuItem).

Der folgende Screenshot zeigt `ToolbarItem` Objekte in der Navigationsleiste unter IOS und Android:

!["Bildschirm Abbildung der ToolBarItem-Demo unter Android und IOS"](toolbaritem-images/toolbaritem-device-screenshot.png "Bildschirm Abbildung von ToolBarItem-Demo unter Android und IOS")

Das `ToolbarItem` -Steuerelement definiert die folgenden Eigenschaften:

* [`Order`](xref:Xamarin.Forms.ToolbarItem.Order)ein `ToolbarItemOrder` Enumerationswert, der bestimmt, `ToolbarItem` ob die-Instanz im primären oder sekundären Menü angezeigt wird.
* [`Priority`](xref:Xamarin.Forms.ToolbarItem.Priority)ein `integer` -Wert, der die Anzeigereihenfolge von Elementen `Page` in der `ToolbarItems` Auflistung eines-Objekts bestimmt.

Die `ToolbarItem` -Klasse erbt die folgenden normalerweise verwendeten Eigenschaften von `MenuItem` der-Klasse:

* [`Text`](xref:Xamarin.Forms.MenuItem.Text)ist ein `string` , der den Anzeige Text für ein `ToolbarItem` -Objekt bestimmt.
* [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource)ein `ImageSource` -Wert, der das Anzeige Symbol für ein `ToolbarItem` -Objekt bestimmt.
* [`Command`](xref:Xamarin.Forms.MenuItem.Command)ist eine `ICommand` , die das Binden von Benutzeraktionen (z. b. Finger Tippen oder Klicks) auf Befehle ermöglicht, die für ein ViewModel definiert sind.
* [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter)ist eine `object` , die den Parameter angibt, der an den `SearchCommand`übergeben werden soll.

Diese Eigenschaften werden durch [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekte gestützt, sodass eine `ToolbarItem` -Instanz das Ziel von Daten Bindungen sein kann.

> [!NOTE]
> Eine Alternative zum Erstellen einer Symbolleiste [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) aus Objekten besteht darin, [`NavigationPage.TitleView`](xref:Xamarin.Forms.NavigationPage.TitleViewProperty) die angefügte-Eigenschaft auf eine Layoutklasse festzulegen, die mehrere Ansichten enthält. Weitere Informationen finden Sie unter [Anzeigen von Ansichten in der Navigationsleiste](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md#displaying-views-in-the-navigation-bar).

## <a name="create-a-toolbaritem"></a>Erstellen eines ToolBarItem

Ein `ToolbarItem` -Objekt kann in XAML instanziiert werden. Die `Text` Eigenschaften `IconImageSource` und können festgelegt werden, um zu bestimmen, wie die Schaltfläche auf der Navigationsleiste angezeigt wird. Im folgenden Beispiel wird gezeigt, wie ein `ToolbarItem` mit einigen allgemeinen Eigenschaften instanziiert wird:

```xaml
<ToolbarItem Text="Example Item"
             IconImageSource="example_icon.png"
             Order="Primary"
             Priority="0" />
```

In diesem Beispiel wird ein `ToolbarItem` -Objekt mit Text angezeigt, ein Symbol, das im Bereich der primären Navigationsleiste zuerst angezeigt wird. Ein `ToolbarItem` kann auch im Code erstellt und der `ToolbarItems` Auflistung hinzugefügt werden:

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

Die durch das `string`dargestellte Datei, die `IconImageSource` als-Eigenschaft bereitgestellt wird, muss in jedem Platt Form Projekt vorhanden sein.

> [!NOTE]
> Bild Ressourcen werden auf jeder Plattform unterschiedlich behandelt. Ein `ImageSource` kann aus Quellen stammen, einschließlich einer lokalen Datei oder eingebetteten Ressource, eines URIs oder eines Streams. Weitere Informationen zum Festlegen der- `IconImageSource` Eigenschaft und der Bilder in xamarin. Forms finden Sie unter [Bilder in xamarin. Forms](~/xamarin-forms/user-interface/images.md).

## <a name="define-button-behavior"></a>Definieren des Schaltflächen Verhaltens

Die `ToolbarItem` -Klasse erbt `Clicked` das-Ereignis `MenuItem` von der-Klasse. Ein Ereignishandler kann an das `Clicked` -Ereignis angefügt werden, um auf- `ToolbarItem` Instanzen in XAML auf tippen oder Klicks zu reagieren:

```xaml
<ToolbarItem ...
             Clicked="OnItemClicked" />
```

Ein Ereignishandler kann auch im Code angefügt werden:

```csharp
ToolbarItem item = new ToolbarItem { ... }
item.Clicked += OnItemClicked;
```

In den vorherigen Beispielen `OnItemClicked` wurde auf einen Ereignishandler verwiesen. Der folgende Code zeigt eine Beispiel Implementierung:

```csharp
void OnItemClicked(object sender, EventArgs e)
{
    ToolbarItem item = (ToolbarItem)sender;
    messageLabel.Text = $"You clicked the \"{item.Text}\" toolbar item.";
}
```

`ToolbarItem`-Objekte können auch die `Command` - `CommandParameter` Eigenschaft und die-Eigenschaft verwenden, um auf Benutzereingaben ohne Ereignishandler zu reagieren. Weitere Informationen `ICommand` zur-Schnittstelle und zur MVVM-Datenbindung finden Sie unter [xamarin. Forms MenuItem MVVM-Verhalten](~/xamarin-forms/user-interface/menuitem.md#define-menuitem-behavior-with-mvvm).

## <a name="primary-and-secondary-menus"></a>Primäre und sekundäre Menüs

Die `ToolbarItemOrder` Enumeration hat `Default`die `Primary`Werte, `Secondary` und.

Wenn die `Order` -Eigenschaft auf `Primary`festgelegt ist `ToolbarItem` , wird das-Objekt auf der Hauptnavigationsleiste auf allen Plattformen angezeigt. `ToolbarItem`Objekte werden über den Seitentitel priorisiert, der abgeschnitten wird, um Platz für die Elemente zu schaffen. Die folgenden Screenshots zeigen `ToolbarItem` Objekte im primären Menü unter IOS und Android:

!["ToolBarItem-Hauptmenü: Screenshot Android und IOS"](toolbaritem-images/toolbaritem-primary-menu.png "Bildschirm Abbildung des primären ToolBarItem-Menüs unter Android und IOS")

Wenn die `Order` -Eigenschaft auf `Secondary`festgelegt ist, variiert das Verhalten plattformübergreifend. Bei UWP und Android wird das `Secondary` Menü Elemente als drei Punkte angezeigt, auf die Sie tippen oder klicken können, um Elemente in einer vertikalen Liste anzuzeigen. Unter IOS wird das `Secondary` Menü Elemente unterhalb der Navigationsleiste als horizontale Liste angezeigt. Die folgenden Screenshots zeigen ein sekundäres Menü unter IOS und Android:

!["ToolBarItem-Menü auf dem sekundären Menü" Android und IOS "](toolbaritem-images/toolbaritem-secondary-menu.png "Bildschirm Abbildung des sekundären ToolBarItem-Menüs unter Android und IOS")

> [!WARNING]
> Das Symbol Verhalten `ToolbarItem` in Objekten, deren `Order` -Eigenschaft auf `Secondary` festgelegt ist, ist plattformübergreifend nicht konsistent. Legen Sie die `IconImageSource` -Eigenschaft für Elemente, die im sekundären Menü angezeigt werden, nicht fest.

## <a name="related-links"></a>Verwandte Links

* [ToolBarItem-Demos](https://docs.microsoft.com/en-us/samples/xamarin/xamarin-forms-samples/userinterface-toolbaritem/)
* [Bilder in xamarin. Forms](~/xamarin-forms/user-interface/images.md)
