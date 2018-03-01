---
title: Arbeiten mit Navigation
ms.topic: article
ms.prod: xamarin
ms.assetid: 71A64C10-75C8-4159-A547-6A704F3B5C2E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 79277d412b87e6ac44557122fa06d4d5d873fd38
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-navigation"></a>Arbeiten mit Navigation

Die einfachste verfügbare Navigationsoption für die Überwachung ist eine einfache [modales Popupdialogfeld](#modal) , zusätzlich zu der aktuellen Szene angezeigt wird.

Für mit mehreren Szene Watch-apps gibt es zwei Navigation Paradigmen verfügbar sind:

- [Hierarchische Navigation](#Hierarchical_Navigation)
- [Seitenbasierte Schnittstellen](#Page-Based_Interfaces)

## <a name="modal-interfaces"></a>Modale Schnittstellen

Verwenden der `PresentController` Methode, um eine Schnittstelle Controller modal zu öffnen. Der Controller Schnittstelle muss bereits definiert werden, der **Interface.storyboard**.

```csharp
PresentController ("pageController","some context info");
```

Controller modal angezeigt verwenden Sie den gesamten Bildschirm (abdecken der vorherigen Szene haben). Der Titel ist standardmäßig auf festgelegt **"Abbrechen"** und tippen sie auf den Controller verworfen wird.

Rufen Sie zum programmgesteuerten schließen den Controller modal angezeigt `DismissController`.

```csharp
DismissController();
```

Modale Bildschirme können es sich um eine einzelne Szene oder die Verwendung eines Layouts seitenbasierte sein.


## <a name="hierarchical-navigation"></a>Hierarchische Navigation

Zeigt im Hintergrund beim Übersetzen, wie ein Stapel, die Navigation durch zurückgehen, ähnlich wie möglich `UINavigationController` funktioniert für iOS. Szenen können auf Navigationsstapel abgelegt und (entweder programmgesteuert oder durch Auswahl des Benutzers) geholt werden.

![](navigation-images/hierarchy-1.png "Szenen geschoben werden können, auf dem Navigationsstapel") ![ ] (navigation-images/hierarchy-2.png "können im Hintergrund beim Übersetzen der Navigationsstapel geholt werden")

Wie bei iOS, wechselt einen linken Rand Wischen zurück zum übergeordneten Controller in einem Stapel hierarchische Navigation.

Sowohl die [WatchKitCatalog](https://developer.xamarin.com/samples/WatchKitCatalog) und [WatchTables](https://developer.xamarin.com/samples/WatchTables) Beispiele umfassen die hierarchische Navigation.

### <a name="pushing-and-popping-in-code"></a>Per Push übertragen und im Code das herunternehmen

Kit erfordert kein Recherchen "Navigation Controller" sehen Sie sich einfach erstellt werden, wie iOS - push einen Controller mit dem `PushController` -Methode und eine Navigationsstapel werden automatisch erstellt werden.

```csharp
PushController("secondPageController","some context info");
```

Bildschirm für die Überwachung umfasst eine **wieder** Schaltfläche in der oberen linken Ecke, aber Sie können auch programmgesteuert eine Szene aus dem Navigationsbereich Stapel mit entfernen `PopController`.

```csharp
PopController();
```

Wie bei iOS, es auch möglich, in das Stammverzeichnis des Stapel für die Navigation mit zurückgeben ist `PopToRootController`.

```csharp
PopToRootController();
```

### <a name="using-segues"></a>Segues verwenden

Segues zwischen Szenen in das Storyboard, um die hierarchische Navigation definieren erstellt werden können. Beim Abrufen der Kontext für die Ziel-Szene die Betriebssystem-Aufrufe `GetContextForSegue` zum Initialisieren des neuen Schnittstelle Controllers.

```csharp
public override NSObject GetContextForSegue (string segueIdentifier)
{
  if (segueIdentifier == "mySegue") {
    return new NSString("some context info");
  }
  return base.GetContextForSegue (segueIdentifier);
}
```

## <a name="page-based-interfaces"></a>Seitenbasierte Schnittstellen

Wischen Sie seitenbasierte Schnittstellen nach links-nach-rechts, ähnlich wie bei `UIPageViewController` funktioniert für iOS. Indikator-Punkte werden am unteren Rand des Bildschirms anzeigen, welche Seite derzeit angezeigt wird angezeigt.

![](navigation-images/paged-1.png "Erste Beispielseite") ![ ] (navigation-images/paged-2.png "zweite Beispielseite") ![ ] (navigation-images/paged-5.png "fünfte Beispielseite")


Verwenden, um einer Seite-basierte Schnittstelle für die app überwachen die Hauptbenutzeroberfläche vorzunehmen `ReloadRootControllers` mit einem Array der Schnittstelle Controller und Kontexte:

```csharp
var controllerNames = new [] { "pageController", "pageController", "pageController", "pageController", "pageController" };
var contexts = new [] { "First", "Second", "Third", "Fourth", "Fifth" };
ReloadRootControllers (controllerNames, contexts);
```

Sie können auch einen seitenbasierte Controller, der nicht der Stamm ist darstellen mit `PresentController` von einem anderen im Hintergrund in einer app.

```csharp
var controllerNames = new [] { "pageController", "pageController", "pageController", "pageController", "pageController" };
var contexts = new [] { "First", "Second", "Third", "Fourth", "Fifth" };
PresentController (controllerNames, contexts);
```



## <a name="related-links"></a>Verwandte Links

- [WatchKitCatalog (Beispiel)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [WatchTables (Beispiel)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchTables/)
