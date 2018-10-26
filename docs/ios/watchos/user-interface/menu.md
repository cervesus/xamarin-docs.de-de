---
title: WatchOS-Menu-Steuerelement (Force Touch) in Xamarin
description: Dieses Dokument beschreibt, wie Sie die WatchOS-Force Touch-Geste in Xamarin zu verwenden. Es wird erläutert, wie auf eine Fingereingabe-Force-Antworten wie ein Menü, und Ändern der Menüelemente hinzugefügt.
ms.prod: xamarin
ms.assetid: 5A7F83FB-9BC4-4812-92C5-CEC8DAE8211E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 7696c820ab6fdf19bdef46db31061fb5914e6cf4
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109756"
---
# <a name="watchos-menu-control-force-touch-in-xamarin"></a>WatchOS-Menu-Steuerelement (Force Touch) in Xamarin

Watch-Kit enthält eine Force Touch-Geste, die ein Menü, bei der Implementierung auf einem Bildschirm der Watch-app auslöst.

![](menu-images/menu.png "Apple Watch Anzeigen eines Menüs")
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

## <a name="responding-to-force-touch"></a>Reagieren auf Force Touch

Wenn eine `Menu` wurde für einen Controller Schnittstelle implementiert, wenn ein Benutzer eine Force Touch ausführt, das Menü angezeigt werden wird. Wenn kein Menü implementiert wurde, wird der Bildschirm wird kurz animiert eine keine andere Aktion ausgeführt wird.

-Force-Workflows sind kein bestimmtes Element auf dem Bildschirm zugeordnet. nur ein Menü eines schnittstellencontrollers angefügt werden kann, und es wird angezeigt, unabhängig davon, wo die-Force Touch drücken Sie auf dem Bildschirm auftritt.

Zwischen einer und vier Menü können die Optionen angezeigt werden.


## <a name="adding-a-menu"></a>Hinzufügen eines Menüs

Ein `Menu` muss hinzugefügt werden, um eine `InterfaceController` auf dem Storyboard zur Entwurfszeit. Wenn ein Steuerelement auf einer schnittstellencontrollers gezogen wird kein visueller Indikator für die Storyboard-Vorschau vorhanden ist, aber die **Menü** wird in der **Dokumentgliederung** Pad:

![](menu-images/menu-action.png "Bearbeiten ein Menü zur Entwurfszeit")

Bis zu vier Menü können das Menüsteuerelement Elemente hinzugefügt werden. Sie können konfiguriert werden, der **Eigenschaften** Pad. Die folgenden Attribute können festgelegt werden:

- Titel, und
- Benutzerdefiniertes Image oder
- Ein Systemabbild: akzeptieren "," Add "," Block "," Ablehnen "," Info, vielleicht mehr Stummschaltung, anhalten, spielen, wiederholen "," fortsetzen "," Freigabe "," Shuffle "," Speaker "," Papierkorb.

Erstellen einer `Action` durch Auswählen der **Ereignisse** Teil der **Eigenschaften** Auffüllzeichen, und geben Sie den Namen für die Aktionsmethode. Eine partielle Methode wird im Code erstellt werden, die in der Schnittstelle Controller-Klasse wie folgt implementiert werden können:

```csharp
partial void MenuItemTapped ()
{
    Console.WriteLine ("A menu item was tapped.");
}
```

### <a name="custom-images"></a>Benutzerdefinierte Images

Ähnlich wie bei Registerkarte Bilder unter iOS werden menüelementbilder erfordern ein nicht transparenter Muster mit einem Alphakanal, der den Hintergrund durchscheinen kann.

Sie sollten die für das Menü das Watch-app-Projekt (nicht das Watch-app-Erweiterungsprojekt) für eine optimale Leistung verwendeten Images hinzufügen.


## <a name="changing-the-menu-items"></a>Ändern der Menüelemente

<!--
### Design Time Items

Menu items added the storyboard can be shown and hidden programmatically.
-->

### <a name="adding-at-runtime"></a>Zur Laufzeit hinzufügen

Sie können nicht dazu führen, dass eine `Menu` hinzuzufügende eines schnittstellencontrollers zur Laufzeit, obwohl die Auflistung der `MenuItem`s *können* programmgesteuert geändert werden.
Verwenden der `AddMenuItem` Methode wie gezeigt:

```csharp
AddMenuItem (WKMenuItemIcon.Accept, "Yes", new ObjCRuntime.Selector ("tapped"));
```

Kit Watch Xamarin.iOS-API erfordert derzeit ein `selector` für die `AdMenuItem` -Methode, die wie folgt deklariert werden soll:

```csharp
[Export("tapped")]
void MenuItemTapped ()
{
    Console.WriteLine ("The dynamically added 'Yes' menu item was tapped.");
}
```

### <a name="removing-at-runtime"></a>Zur Laufzeit entfernen

Die `ClearAllMenuItems` Methode aufgerufen werden, um alle entfernen *programmgesteuert hinzugefügten* Menüelemente.

Menüelemente, die konfiguriert werden, in dem Storyboard können nicht gelöscht werden.



## <a name="related-links"></a>Verwandte Links

- [WatchKitCatalog (Beispiel)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Apple Menü-doc](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Menus.html)
