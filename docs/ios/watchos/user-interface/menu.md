---
title: watchos-Menü Steuerelement (Force Touch) in xamarin
description: In diesem Dokument wird beschrieben, wie Sie die watchos Force-touchbewegung in xamarin verwenden. Es wird erläutert, wie auf eine Force-Fingereingabe reagiert wird, wie ein Menü hinzugefügt und die Menü Elemente geändert werden.
ms.prod: xamarin
ms.assetid: 5A7F83FB-9BC4-4812-92C5-CEC8DAE8211E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 1a9013c8f3adad0f52c264ea1216cab784d6b58f
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86939658"
---
# <a name="watchos-menu-control-force-touch-in-xamarin"></a>watchos-Menü Steuerelement (Force Touch) in xamarin

Das Watch-Kit bietet eine Force Touch Geste, mit der ein Menü ausgelöst wird, wenn es auf einem Bildschirm für die Überwachung

![Apple Watch, das ein Menü anzeigt](menu-images/menu.png)
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

## <a name="responding-to-force-touch"></a>Reagieren auf Force Touch

Wenn ein `Menu` für einen Schnittstellen Controller implementiert wurde, wird das Menü angezeigt, wenn ein Benutzer einen Force Touch. Wenn kein Menü implementiert wurde, wird der Bildschirm kurz animiert, sodass keine weitere Aktion durchgeführt wird.

Force-Berührungen werden keinem bestimmten Element auf dem Bildschirm zugeordnet. an einen Schnittstellen Controller kann nur ein Menü angefügt werden, und es wird unabhängig davon angezeigt, wo der Force Touch auf dem Bildschirm angezeigt wird.

Zwischen einer und vier Menü Optionen können angezeigt werden.

## <a name="adding-a-menu"></a>Hinzufügen eines Menüs

Ein `Menu` muss einem `InterfaceController` auf dem Storyboard zur Entwurfszeit hinzugefügt werden. Wenn ein Menü Steuerelement auf einen Schnittstellen Controller gezogen wird, gibt es keinen visuellen Hinweis auf die Storyboard-Vorschau. das **Menü** wird jedoch im **Dokument** Gliederungs Fenster angezeigt:

![Bearbeiten eines Menüs zur Entwurfszeit](menu-images/menu-action.png)

Dem Menü Steuerelement können bis zu vier Menü Elemente hinzugefügt werden. Sie können im **eigenschaftenpad** konfiguriert werden. Die folgenden Attribute können festgelegt werden:

- Titel und
- Benutzerdefiniertes Image oder
- Ein System Abbild: Accept, Add, Block, ablehnen, Info, Maybe, more, stumm, Pause, Play, Repeat, Resume, Share, shuffle, Speaker, Papierkorb.

Erstellen Sie einen, `Action` indem Sie im **eigenschaftenpad** den Abschnitt **Ereignisse** auswählen und den Namen für die Aktionsmethode eingeben. Im Code wird eine partielle Methode erstellt, die wie folgt in der Schnittstellen Controller Klasse implementiert werden kann:

```csharp
partial void MenuItemTapped ()
{
    Console.WriteLine ("A menu item was tapped.");
}
```

### <a name="custom-images"></a>Custom Images

Ähnlich wie Registerkarten Bilder in ios erfordern Menü Element Bilder ein undurchsichtiges Muster mit einem Alphakanal, mit dem der Hintergrund durch angezeigt werden kann.

Für eine optimale Leistung sollten Sie die für das Menü verwendeten Images dem App-Projekt überwachen (nicht das App-Erweiterungsprojekt) hinzufügen.

## <a name="changing-the-menu-items"></a>Ändern der Menü Elemente

<!--
### Design Time Items

Menu items added the storyboard can be shown and hidden programmatically.
-->

### <a name="adding-at-runtime"></a>Hinzufügen zur Laufzeit

Sie können nicht bewirken `Menu` , dass ein einem Schnittstellen Controller zur Laufzeit hinzugefügt wird, obwohl die Auflistung von `MenuItem` s Programm gesteuert geändert werden *kann* .
Verwenden Sie die- `AddMenuItem` Methode wie folgt:

```csharp
AddMenuItem (WKMenuItemIcon.Accept, "Yes", new ObjCRuntime.Selector ("tapped"));
```

Die xamarin. IOS Watch-Kit-API erfordert derzeit eine `selector` für die- `AdMenuItem` Methode, die wie folgt deklariert werden sollte:

```csharp
[Export("tapped")]
void MenuItemTapped ()
{
    Console.WriteLine ("The dynamically added 'Yes' menu item was tapped.");
}
```

### <a name="removing-at-runtime"></a>Entfernen zur Laufzeit

Die- `ClearAllMenuItems` Methode kann aufgerufen werden, um alle *Programm gesteuert hinzugefügten* Menü Elemente zu entfernen.

Im Storyboard konfigurierte Menü Elemente können nicht gelöscht werden.

## <a name="related-links"></a>Ähnliche Themen

- [Watchkitcatalog (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Menü Dokumentation von Apple](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Menus.html)
