---
title: Popupmenü
description: Wie ein Popupmenü, das verankert ist eine bestimmte Ansicht hinzugefügt.
ms.prod: xamarin
ms.assetid: 1C58E12B-4634-4691-BF59-D5A3F6B0E6F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/31/2018
ms.openlocfilehash: d7cadde88e9ae7ee30815ee9323785038dbb1a39
ms.sourcegitcommit: ecdc031e9e26bbbf9572885531ee1f2e623203f5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2018
ms.locfileid: "39393658"
---
# <a name="popup-menu"></a>Popupmenü

Die [PopupMenu](https://developer.xamarin.com/api/type/Android.Widget.PopupMenu/) (so genannte eine _Kontextmenü_) ist ein Menü, das eine bestimmte Ansicht verbunden ist. Im folgenden Beispiel enthält eine einzelne Aktivität eine Schaltfläche. Wenn der Benutzer die Schaltfläche tippt, wird ein Popupmenü mit drei Elementen angezeigt:

[![Beispiel für eine app mit einer Schaltfläche und drei Elementen Popup-Menü](popup-menu-images/01-app-example-sml.png)](popup-menu-images/01-app-example.png#lightbox)


## <a name="creating-a-popup-menu"></a>Erstellen eines Popup-Menüs

Der erste Schritt ist, erstellen eine Ressourcendatei "im Menü" für das Menü, und versetzen Sie ihn in **ressourcenmenü/**. Das folgende XML ist beispielsweise der Code für die drei Elementen im Menü angezeigt, die im vorherigen Screenshot **Resources/menu/popup_menu.xml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/item1"
          android:title="item 1" />
    <item android:id="@+id/item1"
          android:title="item 2" />
    <item android:id="@+id/item1"
          android:title="item 3" />
</menu>
```

Als Nächstes erstellen Sie eine Instanz des `PopupMenu` und Verankern sie die Anmerkung an seinem Ansichtszustand. Beim Erstellen einer Instanz von `PopupMenu`, übergeben Sie ihren Konstruktor einen Verweis auf die `Context` sowie die Ansicht, klicken Sie im Menü angefügt werden. Daher ist das Popupmenü während der Erstellung dieser Ansicht verankert.

Im folgenden Beispiel die `PopupMenu` wird in der Click-Ereignishandler für die Schaltfläche erstellt (mit dem Namen `showPopupMenu`). Diese Schaltfläche ist auch die Sicht, die `PopupMenu` verankert ist, wie im folgenden Codebeispiel gezeigt:

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
};
```

Abschließend muss das Popupmenü *vergrößerte* mit der Menüressource, die zuvor erstellt wurde. Im folgenden Beispiel der Aufruf des Menüs [Inflate](https://developer.xamarin.com/api/member/Android.Views.LayoutInflater.Inflate/p/System.Int32/Android.Views.ViewGroup/) Methode hinzugefügt wird und die zugehörige [anzeigen](https://developer.xamarin.com/api/member/Android.Widget.PopupMenu.Show%28%29/) Methode wird aufgerufen, um es anzuzeigen:

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.Inflate (Resource.Menu.popup_menu);
    menu.Show ();
};
```


## <a name="handling-menu-events"></a>Behandeln von Menüereignissen

Wenn der Benutzer ein Menüelement auswählt der [MenuItemClick](https://developer.xamarin.com/api/event/Android.Widget.PopupMenu.MenuItemClick/) klicken Sie auf Ereignis wird ausgelöst, und klicken Sie im Menü wird geschlossen. Durch Tippen auf eine beliebige Stelle außerhalb des Menüs wird es einfach geschlossen. In beiden Fällen, wenn Sie im Menü geschlossen wird dessen [DismissEvent](https://developer.xamarin.com/api/member/Android.Widget.PopupMenu.Dismiss%28%29/) ausgelöst. Der folgende Code fügt der Ereignishandler für beide die `MenuItemClick` und `DismissEvent` Ereignisse:

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.Inflate (Resource.Menu.popup_menu);

    menu.MenuItemClick += (s1, arg1) => {
        Console.WriteLine ("{0} selected", arg1.Item.TitleFormatted);
    };

    menu.DismissEvent += (s2, arg2) => {
        Console.WriteLine ("menu dismissed");
    };
    menu.Show ();
};
```



## <a name="related-links"></a>Verwandte Links

- [PopupMenuDemo (Beispiel)](https://developer.xamarin.com/samples/monodroid/PopupMenuDemo/)
