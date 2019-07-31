---
title: Popupmenü
description: Vorgehensweise beim Hinzufügen eines Popup Menüs, das für eine bestimmte Ansicht verankert ist.
ms.prod: xamarin
ms.assetid: 1C58E12B-4634-4691-BF59-D5A3F6B0E6F7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/31/2018
ms.openlocfilehash: 9b3e4177d6be5854e80952d091aa78787d9645bb
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68644927"
---
# <a name="xamarinandroid-popup-menu"></a>Xamarin. Android-Popup Menü

Der [PopupMenu](xref:Android.Widget.PopupMenu) (auch als Kontext _Menü_bezeichnet) ist ein Menü, das mit einer bestimmten Ansicht verankert ist. Im folgenden Beispiel enthält eine einzelne-Aktivität eine Schaltfläche. Wenn der Benutzer auf die Schaltfläche tippt, wird ein Popupmenü mit drei Elementen angezeigt:

[![Beispiel für eine APP mit einer Schaltfläche und einem Popupmenü mit drei Elementen](popup-menu-images/01-app-example-sml.png)](popup-menu-images/01-app-example.png#lightbox)


## <a name="creating-a-popup-menu"></a>Erstellen eines Popup Menüs

Der erste Schritt besteht darin, eine Menü Ressourcen Datei für das Menü zu erstellen und in **Ressourcen/Menü**zu platzieren. Der folgende XML-Code ist beispielsweise der Code für das drei-Element-Menü, das im vorherigen Screenshot, **Resources/Menu/popup_menu. XML**, angezeigt wird:

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

Erstellen Sie als nächstes eine Instanz `PopupMenu` von, und Verankern Sie Sie an der Ansicht. Wenn Sie eine Instanz von `PopupMenu`erstellen, übergeben Sie den Konstruktor einen Verweis `Context` auf und die Ansicht, an die das Menü angefügt wird. Folglich ist das Popup Menü während der Erstellung an dieser Ansicht verankert.

Im folgenden Beispiel wird der `PopupMenu` im Click-Ereignishandler für die Schaltfläche (mit dem Namen `showPopupMenu`) erstellt. Diese Schaltfläche ist auch die Ansicht, in `PopupMenu` der die verankert ist, wie im folgenden Codebeispiel gezeigt:

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
};
```

Zum Schluss muss das Popup Menü mit der zuvor erstellten Menü Ressource *aufgeblasen* werden. Im folgenden Beispiel wird der Aufruf der [Inflate](xref:Android.Views.LayoutInflater.Inflate*) -Methode des Menüs hinzugefügt, und die [Show](xref:Android.Widget.PopupMenu.Show) -Methode wird aufgerufen, um Sie anzuzeigen:

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.Inflate (Resource.Menu.popup_menu);
    menu.Show ();
};
```


## <a name="handling-menu-events"></a>Behandeln von Menü Ereignissen

Wenn der Benutzer ein Menü Element auswählt, wird das [MenuItemClick](xref:Android.Widget.PopupMenu.MenuItemClick) -Click-Ereignis ausgelöst, und das Menü wird verworfen. Wenn Sie auf eine beliebige Stelle außerhalb des Menüs tippen, wird Sie einfach verworfen In beiden Fällen wird das [dismissvent](xref:Android.Widget.PopupMenu.Dismiss) -Ereignis ausgelöst, wenn das Menü verworfen wird. Der folgende Code fügt Ereignishandler für das-Ereignis `MenuItemClick` und `DismissEvent` das-Ereignis hinzu:

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

- [Popupmenudemo (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/popupmenudemo)
