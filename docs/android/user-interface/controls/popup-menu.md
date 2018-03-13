---
title: "PopUp-Menü"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1C58E12B-4634-4691-BF59-D5A3F6B0E6F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/18/2017
ms.openlocfilehash: f976d798ae1b1279fc8f82d3cf1d738bb2c93911
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="popup-menu"></a>PopUp-Menü

Die `PopupMenu` Klasse fügt Unterstützung für die Anzeige von Popupmenüs, die eine bestimmte Sicht zugeordnet sind. Die folgende Abbildung zeigt ein Popupmenü angezeigt auf eine Schaltfläche und das zweite Element hervorgehoben, wie diese Option ausgewählt ist:

 [![Beispiel für eine PopopMenu mit drei Elementen drei](popup-menu-images/20-popupmenu.png)](popup-menu-images/20-popupmenu.png#lightbox)

Android 4 hinzugefügt, verschiedene neue Funktionen zur `PopupMenu` , erleichtern ein wenig, nämlich arbeiten:

-   **Inflate** &ndash; der vergrößert werden soll-Methode steht jetzt direkt auf die PopupMenu-Klasse.
-   **DismissEvent** &ndash; der PopupMenu-Klasse verfügt jetzt über eine DismissEvent.

Werfen wir einen Blick auf diese Verbesserungen. In diesem Beispiel haben wir eine einzelne Aktivität, die eine Schaltfläche enthält. Wenn der Benutzer die Schaltfläche klickt, wird ein Popup-Menü angezeigt, wie unten dargestellt:

 [![Beispiel für die app, die in einen Emulator mit den Schaltflächen und Popupmenü 3-Element](popup-menu-images/06-popupmenu.png)](popup-menu-images/06-popupmenu.png#lightbox)


## <a name="creating-a-popup-menu"></a>Erstellen ein Popup-Menü

Wenn wir erstellen eine Instanz von der `PopupMenu`, müssen wir ihren Konstruktor übergeben Sie einen Verweis auf die `Context`, sowie die Sicht, die Sie im Menü zugeordnet ist. In diesem Fall erstellen wir die `PopupMenu` im Click-Ereignishandler für unsere Schaltfläche die Bezeichnung `showPopupMenu`.
Diese Schaltfläche ist auch die Sicht, müssen wir fügen, die `PopupMenu`, wie im folgenden Code gezeigt:

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
}
```

Android 3 erforderlich der Code, um das Menü aus einer XML-Ressource vergrößert werden soll, erhalten Sie zunächst einen Verweis auf eine `MenuInflator`, und rufen Sie anschließend die `Inflate` Methode mit den Ressourcen-ID der XML-Code, die enthalten das Menü und die Instanz im Menü, um in vergrößert werden soll. Ein derartiger Ansatz funktioniert weiterhin in Android 4 und höher als der folgende Code zeigt:

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.MenuInflater.Inflate (Resource.Menu.popup_menu, menu.Menu);
};
```

Seit Android 4, Sie können jedoch jetzt Aufrufen `Inflate` direkt auf die Instanz von der `PopupMenu`. Dadurch wird den Code präziser, wie hier gezeigt:

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.Inflate (Resource.Menu.popup_menu);
    menu.Show ();
};
```

Im obigen Code nach dem Menü überhöhte Wir rufen Sie einfach `menu.Show` auf dem Bildschirm angezeigt.


## <a name="handling-menu-events"></a>Behandlung von Menüereignissen im

Wenn der Benutzer ein Menüelement auswählt der `MenuItemClick` Ereignis wird ausgelöst, und klicken Sie im Menü wird abgeschaltet werden. Tippen Sie auf eine beliebige Stelle außerhalb des Menüs, wird es einfach schließen. In beiden Fällen ab Android 4, wenn das Menü geschlossen wird dessen `DismissEvent` ausgelöst werden soll. Der folgende Code fügt der Ereignishandler für beide die `MenuItemClick` und `DismissEvent` Ereignisse:

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
- [Einführung in Eis Rustikal Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 Platform](http://developer.android.com/sdk/android-4.0.html)
