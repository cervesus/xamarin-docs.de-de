---
title: Menu-Steuerelement (Force Touch)
ms.prod: xamarin
ms.assetid: 5A7F83FB-9BC4-4812-92C5-CEC8DAE8211E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 4895f140acbc8a622cfb91c2fe9db6c5e9d9d1d7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="menu-control-force-touch"></a>Menu-Steuerelement (Force Touch)

Überwachen-Kit bietet eine Force Touch-Geste, die löst ein Menü, wenn auf einem Watch-app-Bildschirm implementiert.

![](menu-images/menu.png "Apple Watch Anzeigen eines Menüs")
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

## <a name="responding-to-force-touch"></a>Reagieren auf Force Touch

Wenn ein `Menu` wurde für einen Controller Schnittstelle implementiert, wenn ein Benutzer eine Force berühren ausführt, klicken Sie im Menü angezeigt wird. Wenn kein Menü implementiert wurde, wird der Bildschirm kurz animiert eine keine andere Aktion ausgeführt.

Force Fingereingaben sind nicht mit einem bestimmten Element auf dem Bildschirm verknüpft. kann nur ein Menü mit einer Schnittstelle-Controller angefügt werden, und es wird angezeigt, unabhängig davon, wo Force berühren drücken Sie dann auf dem Bildschirm auftritt.

Optionen können zwischen einem und vier Menü angezeigt.


## <a name="adding-a-menu"></a>Hinzufügen eines Menüs

Ein `Menu` muss hinzugefügt werden, um eine `InterfaceController` auf das Storyboard zur Entwurfszeit. Wenn ein Menüsteuerelement auf einen Controller Schnittstelle gezogen wird es gibt keinen Hinweis auf das Storyboard-Vorschau jedoch **Menü** wird angezeigt, der **Dokumentgliederung** Pad:

![](menu-images/menu-action.png "Bearbeiten eines Menüs zur Entwurfszeit")

Elemente können bis zu vier Menü Menu-Steuerelement hinzugefügt werden. Sie können konfiguriert werden, der **Eigenschaften** aufgefüllt. Die folgenden Attribute können festgelegt werden:

- Titel und
- Benutzerdefiniertes Bild oder
- Ein Systemabbild: akzeptieren "," hinzufügen "," blockieren "," Ablehnen "," Info, vielleicht mehr, Stummschalten, anhalten, spielen, wiederholen, Resume, Freigabe, zufällige, sprechender Benutzer, Papierkorb.

Erstellen einer `Action` durch Auswahl der **Ereignisse** Teil der **Eigenschaften** Pad und geben Sie den Namen für die Aktionsmethode. Eine partielle Methode wird im Code erstellt werden, die in die Controllerklasse Schnittstelle wie folgt implementiert werden können:

```csharp
partial void MenuItemTapped ()
{
    Console.WriteLine ("A menu item was tapped.");
}
```

### <a name="custom-images"></a>Benutzerdefinierte Bilder

Ähnlich wie bei Registerkarte Bilder in iOS menüelementbilder erfordern ein nicht transparenter Muster mit einen alpha-Kanal, der den Hintergrund durch durchscheinen kann.

Sie sollten die Symbole für das Menü Watch-app-Projekt (nicht das Überwachungsfenster app Erweiterungsprojekt) für eine optimale Leistung hinzufügen.


## <a name="changing-the-menu-items"></a>Ändern die Menüelemente

<!--
### Design Time Items

Menu items added the the storyboard can be shown and hidden programmatically.
-->

### <a name="adding-at-runtime"></a>Hinzufügen zur Laufzeit

Sie können nicht dazu führen, dass eine `Menu` zu an einen Controller Schnittstelle zur Laufzeit hinzugefügt werden, obwohl die Auflistung der `MenuItem`s *können* programmgesteuert geändert werden.
Verwenden der `AddMenuItem` Methode wie gezeigt:

```csharp
AddMenuItem (WKMenuItemIcon.Accept, "Yes", new ObjCRuntime.Selector ("tapped"));
```

Die Xamarin.iOS Watch-Kit-API derzeit erfordert eine `selector` für die `AdMenuItem` -Methode, die wie folgt deklariert werden:

```csharp
[Export("tapped")]
void MenuItemTapped ()
{
    Console.WriteLine ("The dynamically added 'Yes' menu item was tapped.");
}
```

### <a name="removing-at-runtime"></a>Zur Laufzeit entfernen

Die `ClearAllMenuItems` Methode kann aufgerufen werden, um alle entfernen *programmgesteuert hinzugefügten* Menüelemente.

Menüelemente, die in das Storyboard konfiguriert sind, können nicht gelöscht werden.



## <a name="related-links"></a>Verwandte Links

- [WatchKitCatalog (Beispiel)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Apple Menü doc](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Menus.html)
