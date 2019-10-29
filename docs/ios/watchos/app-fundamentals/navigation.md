---
title: Arbeiten mit der watchos-Navigation in xamarin
description: In diesem Dokument wird beschrieben, wie Sie in einer watchos-Anwendung mit der Navigation arbeiten. Es werden modale Schnittstellen, hierarchische Navigation und Seiten basierte Schnittstellen erläutert.
ms.prod: xamarin
ms.assetid: 71A64C10-75C8-4159-A547-6A704F3B5C2E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: d3565e359ccbad9f7b779969f4273a8cbae4d438
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021750"
---
# <a name="working-with-watchos-navigation-in-xamarin"></a>Arbeiten mit der watchos-Navigation in xamarin

Die einfachste Navigations Option, die auf der Überwachung verfügbar ist, ist ein einfaches [modales Popup](#modal) , das oberhalb der aktuellen Szene angezeigt wird.

Für Multi-Scene Watch-apps sind zwei Navigations Paradigmen verfügbar:

- [Hierarchische Navigation](#Hierarchical_Navigation)
- [Seiten basierte Schnittstellen](#Page-Based_Interfaces)

<a name="modal"/>

## <a name="modal-interfaces"></a>Modale Schnittstellen

Verwenden Sie die `PresentController`-Methode, um einen Schnittstellen Controller modale zu öffnen. Der Schnittstellen Controller muss bereits in " **Interface. Storyboard**" definiert sein.

```csharp
PresentController ("pageController","some context info");
```

Modale vorgestellte Controller verwenden den gesamten Bildschirm (der die vorherige Szene abdeckt). Standardmäßig ist der Titel auf **Abbrechen** festgelegt, und beim Tippen wird der Controller geschlossen.

Um den modale dargestellten Controller Programm gesteuert zu schließen, wenden Sie `DismissController`an.

```csharp
DismissController();
```

Modale Bildschirme können entweder eine einzelne Szene sein oder ein Seiten basiertes Layout verwenden.

<a name="Hierarchical_Navigation"/>

## <a name="hierarchical-navigation"></a>Hierarchische Navigation

Zeigt Szenen wie einen Stapel an, der wieder durch navigiert werden kann, ähnlich wie `UINavigationController` unter IOS funktioniert. Szenen können auf den Navigations Stapel verschoben und ausgeschaltet werden (entweder Programm gesteuert oder durch Benutzer Auswahl).

![](navigation-images/hierarchy-1.png "Szenen können auf den Navigations Stapel verschoben werden.") ![](navigation-images/hierarchy-2.png "Szenen können aus dem Navigations Stapel entfernt werden.")

Wie bei IOS navigiert ein Left Edge-Swipe zurück zum übergeordneten Controller in einem hierarchischen Navigations Stapel.

Die Beispiele [watchkitcatalog](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog) und [watchtables](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchtables) enthalten die hierarchische Navigation.

### <a name="pushing-and-popping-in-code"></a>Pushübertragung und popping in Code

Das Watch Kit erfordert keinen übergreifenden "Navigations Controller", wie es bei IOS der Fall ist. über Push wird ein Controller einfach mithilfe der `PushController`-Methode per Push erstellt, und es wird automatisch ein Navigations Stapel erstellt.

```csharp
PushController("secondPageController","some context info");
```

Der Bildschirm der Überwachung enthält oben links eine Schaltfläche " **zurück** ", aber Sie können eine Szene auch Programm gesteuert aus dem Navigations Stapel entfernen, indem Sie `PopController`verwenden.

```csharp
PopController();
```

Wie bei IOS ist es auch möglich, mithilfe von `PopToRootController`zum Stammverzeichnis des Navigations Stapels zurückzukehren.

```csharp
PopToRootController();
```

### <a name="using-segues"></a>Verwenden von "".

Sie können im Storyboard zwischen Szenen erstellt werden, um die hierarchische Navigation zu definieren. Um den Kontext für die zielszene zu erhalten, ruft das Betriebssystem `GetContextForSegue` auf, um den neuen Schnittstellen Controller zu initialisieren.

```csharp
public override NSObject GetContextForSegue (string segueIdentifier)
{
  if (segueIdentifier == "mySegue") {
    return new NSString("some context info");
  }
  return base.GetContextForSegue (segueIdentifier);
}
```

<a name="Page-Based_Interfaces"/>

## <a name="page-based-interfaces"></a>Seiten basierte Schnittstellen

Seiten basierte Schnittstellen Schwenken von links nach rechts, ähnlich wie `UIPageViewController` unter IOS funktioniert. Indikator Punkte werden am unteren Bildschirmrand angezeigt, um anzuzeigen, welche Seite derzeit angezeigt wird.

![](navigation-images/paged-1.png "Erste Beispielseite")![](navigation-images/paged-2.png "Beispiel für eine zweite Seite")![](navigation-images/paged-5.png "Fünfte Beispielseite")

Verwenden Sie `ReloadRootControllers` mit einem Array von Schnittstellen Controllern und Kontexten, um eine Seiten basierte Schnittstelle als Hauptbenutzer Oberfläche für Ihre Watch-APP zu erstellen:

```csharp
var controllerNames = new [] { "pageController", "pageController", "pageController", "pageController", "pageController" };
var contexts = new [] { "First", "Second", "Third", "Fourth", "Fifth" };
ReloadRootControllers (controllerNames, contexts);
```

Sie können auch einen seitenbasierten Controller, der nicht das Stammverzeichnis ist, mithilfe `PresentController` aus einem der anderen Szenen in einer APP darstellen.

```csharp
var controllerNames = new [] { "pageController", "pageController", "pageController", "pageController", "pageController" };
var contexts = new [] { "First", "Second", "Third", "Fourth", "Fifth" };
PresentController (controllerNames, contexts);
```

## <a name="related-links"></a>Verwandte Links

- [Watchkitcatalog (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Watchtables (Beispiel)](https://developer.xamarin.com//samples/monotouch/watchOS/WatchTables/)
