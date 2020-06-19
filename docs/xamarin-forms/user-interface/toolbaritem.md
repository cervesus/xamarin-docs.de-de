---
title: Xamarin.FormsToolBarItem
description: Die ToolBarItem-Klasse ist eine besondere Art von Schaltfläche, die in der Navigationsleiste einer Anwendung verwendet wird.
ms.prod: xamarin
ms.assetId: CC737D54-0280-46BD-A2BC-A0FB67DDD6A1
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/29/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 46aba32ebbae1646b9af00877bba530b619210cd
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138215"
---
# <a name="xamarinforms-toolbaritem"></a>Xamarin.FormsToolBarItem

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-toolbaritem/)

Die Xamarin.Forms [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) -Klasse ist ein spezieller Typ von Schaltfläche, der der Auflistung eines-Objekts hinzugefügt werden kann `Page` `ToolbarItems` . Jedes `ToolbarItem` Objekt wird in der Navigationsleiste der Anwendung als Schaltfläche angezeigt. Eine `ToolbarItem` -Instanz kann ein Symbol aufweisen und als primäres oder sekundäres Menü Element angezeigt werden. Die `ToolbarItem` Klasse erbt von [`MenuItem`](xref:Xamarin.Forms.MenuItem) .

Die folgenden Screenshots zeigen `ToolbarItem` Objekte in der Navigationsleiste unter IOS und Android:

!["Bildschirm Abbildung der ToolBarItem-Demo unter Android und IOS"](toolbaritem-images/toolbaritem-device-screenshot.png "Bildschirm Abbildung von ToolBarItem-Demo unter Android und IOS")

Die- `ToolbarItem` Klasse definiert die folgenden Eigenschaften:

* [`Order`](xref:Xamarin.Forms.ToolbarItem.Order)ein `ToolbarItemOrder` Enumerationswert, der bestimmt, ob die- `ToolbarItem` Instanz im primären oder sekundären Menü angezeigt wird.
* [`Priority`](xref:Xamarin.Forms.ToolbarItem.Priority)ein- `integer` Wert, der die Anzeigereihenfolge von Elementen in der Auflistung eines `Page` -Objekts bestimmt `ToolbarItems` .

Die `ToolbarItem` -Klasse erbt die folgenden normalerweise verwendeten Eigenschaften von der- `MenuItem` Klasse:

* [`Command`](xref:Xamarin.Forms.MenuItem.Command)ist eine `ICommand` , die das Binden von Benutzeraktionen (z. b. Finger Tippen oder Klicks) auf Befehle ermöglicht, die für ein ViewModel definiert sind.
* [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter)ist eine `object` , die den Parameter angibt, der an den übergeben werden soll `Command` .
* [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource)ein- `ImageSource` Wert, der das Anzeige Symbol für ein- `ToolbarItem` Objekt bestimmt.
* [`Text`](xref:Xamarin.Forms.MenuItem.Text)ist ein `string` , der den Anzeige Text für ein- `ToolbarItem` Objekt bestimmt.

Diese Eigenschaften werden durch- [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Objekte gestützt, sodass eine- `ToolbarItem` Instanz das Ziel von Daten Bindungen sein kann.

> [!NOTE]
> Eine Alternative zum Erstellen einer Symbolleiste aus [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) Objekten besteht darin, die [`NavigationPage.TitleView`](xref:Xamarin.Forms.NavigationPage.TitleViewProperty) angefügte-Eigenschaft auf eine Layoutklasse festzulegen, die mehrere Ansichten enthält. Weitere Informationen finden Sie unter [Anzeigen von Ansichten in der Navigationsleiste](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md#displaying-views-in-the-navigation-bar).

## <a name="create-a-toolbaritem"></a>Erstellen eines ToolBarItem

Ein- `ToolbarItem` Objekt kann in XAML instanziiert werden. Die `Text` `IconImageSource` Eigenschaften und können festgelegt werden, um zu bestimmen, wie die Schaltfläche auf der Navigationsleiste angezeigt wird. Im folgenden Beispiel wird gezeigt, wie ein `ToolbarItem` mit einigen allgemeinen Eigenschaften instanziiert und der Auflistung hinzugefügt wird `ContentPage` `ToolbarItems` :

```xaml
<ContentPage.ToolbarItems>
    <ToolbarItem Text="Example Item"
                 IconImageSource="example_icon.png"
                 Order="Primary"
                 Priority="0" />
</ContentPage.ToolbarItems>
```

In diesem Beispiel wird ein `ToolbarItem` -Objekt mit Text angezeigt, ein Symbol, das im Bereich der primären Navigationsleiste zuerst angezeigt wird. Ein `ToolbarItem` kann auch im Code erstellt und der Auflistung hinzugefügt werden `ToolbarItems` :

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

Die durch das dargestellte Datei `string` , die als-Eigenschaft bereitgestellt `IconImageSource` wird, muss in jedem Platt Form Projekt vorhanden sein.

> [!NOTE]
> Bild Ressourcen werden auf jeder Plattform unterschiedlich behandelt. Ein `ImageSource` kann aus Quellen stammen, einschließlich einer lokalen Datei oder eingebetteten Ressource, eines URIs oder eines Streams. Weitere Informationen zum Festlegen der `IconImageSource` -Eigenschaft und der Bilder in finden Sie unter Xamarin.Forms [Bilder in Xamarin.Forms ](~/xamarin-forms/user-interface/images.md).

## <a name="define-button-behavior"></a>Definieren des Schaltflächen Verhaltens

Die- `ToolbarItem` Klasse erbt das- `Clicked` Ereignis von der- `MenuItem` Klasse. Ein Ereignishandler kann an das-Ereignis angefügt werden, um auf- `Clicked` `ToolbarItem` Instanzen in XAML auf tippen oder Klicks zu reagieren:

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

`ToolbarItem`-Objekte können auch die `Command` -Eigenschaft und die-Eigenschaft verwenden `CommandParameter` , um auf Benutzereingaben ohne Ereignishandler zu reagieren. Weitere Informationen zur `ICommand` -Schnittstelle und zur MVVM-Datenbindung finden Sie unter [ Xamarin.Forms MenuItem MVVM-Verhalten](~/xamarin-forms/user-interface/menuitem.md#define-menuitem-behavior-with-mvvm).

## <a name="enable-or-disable-a-toolbaritem-at-runtime"></a>Aktivieren oder Deaktivieren eines ToolBarItem zur Laufzeit

Um das Deaktivieren von zur `ToolbarItem` Laufzeit zu aktivieren, binden Sie seine- `Command` Eigenschaft an eine `ICommand` -Implementierung, und stellen Sie sicher, dass ein Delegat den ggf `canExecute` . aktiviert und deaktiviert `ICommand` .

Weitere Informationen finden Sie [unter Aktivieren oder Deaktivieren eines MenuItem zur Laufzeit](menuitem.md#enable-or-disable-a-menuitem-at-runtime).

## <a name="primary-and-secondary-menus"></a>Primäre und sekundäre Menüs

Die Enumeration `ToolbarItemOrder` hat `Default` die `Primary` Werte, und `Secondary` .

Wenn die- `Order` Eigenschaft auf festgelegt ist `Primary` , wird das- `ToolbarItem` Objekt auf der Hauptnavigationsleiste auf allen Plattformen angezeigt. `ToolbarItem`Objekte werden über den Seitentitel priorisiert, der abgeschnitten wird, um Platz für die Elemente zu schaffen. Die folgenden Screenshots zeigen `ToolbarItem` Objekte im primären Menü unter IOS und Android:

!["ToolBarItem-Hauptmenü: Screenshot Android und IOS"](toolbaritem-images/toolbaritem-primary-menu.png "Bildschirm Abbildung des primären ToolBarItem-Menüs unter Android und IOS")

Wenn die- `Order` Eigenschaft auf festgelegt ist `Secondary` , variiert das Verhalten plattformübergreifend. Bei UWP und Android wird das `Secondary` Menü Elemente als drei Punkte angezeigt, auf die Sie tippen oder klicken können, um Elemente in einer vertikalen Liste anzuzeigen. Unter IOS wird das `Secondary` Menü Elemente unterhalb der Navigationsleiste als horizontale Liste angezeigt. Die folgenden Screenshots zeigen ein sekundäres Menü unter IOS und Android:

!["ToolBarItem-Menü auf dem sekundären Menü" Android und IOS "](toolbaritem-images/toolbaritem-secondary-menu.png "Bildschirm Abbildung des sekundären ToolBarItem-Menüs unter Android und IOS")

> [!WARNING]
> Das Symbol Verhalten in `ToolbarItem` Objekten, deren- `Order` Eigenschaft auf festgelegt `Secondary` ist, ist plattformübergreifend nicht konsistent. Legen Sie die- `IconImageSource` Eigenschaft für Elemente, die im sekundären Menü angezeigt werden, nicht fest.

## <a name="related-links"></a>Verwandte Links

* [ToolBarItem-Demos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-toolbaritem/)
* [Bilder inXamarin.Forms](~/xamarin-forms/user-interface/images.md)
* [Xamarin.FormsMENUITEM](~/xamarin-forms/user-interface/menuitem.md)
