---
title: Arbeiten mit WatchOS zur Navigation in Xamarin
description: Dieses Dokument beschreibt das Arbeiten mit Navigation in einer WatchOS-Anwendung. Es wird erläutert, modale Schnittstellen, hierarchische Navigation und seitenbasierte Schnittstellen.
ms.prod: xamarin
ms.assetid: 71A64C10-75C8-4159-A547-6A704F3B5C2E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 71978bebcdf6033a766ae2bcb75ae061ed215a8b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50102372"
---
# <a name="working-with-watchos-navigation-in-xamarin"></a>Arbeiten mit WatchOS zur Navigation in Xamarin

Die einfachste verfügbare Navigationsoption für die Überwachung ist eine einfache [modales Fenster](#modal) , angezeigt wird, auf die aktuelle Szene.

Für die Multi-Szene-Watch-apps gibt es zwei Navigation Paradigmen, die verfügbar sind:

- [Hierarchische Navigation](#Hierarchical_Navigation)
- [Ein seitenbasiertes Schnittstellen](#Page-Based_Interfaces)

<a name="modal"/>

## <a name="modal-interfaces"></a>Modale Schnittstellen

Verwenden der `PresentController` Methode, um eines schnittstellencontrollers, modal aufzurufen. Die schnittstellencontrollers muss bereits definiert werden, der **Interface.storyboard**.

```csharp
PresentController ("pageController","some context info");
```

Modal angezeigt Controller verwenden Sie den gesamten Bildschirm (die vorherige Szene erläutert). Der Titel ist standardmäßig auf festgelegt **Abbrechen** , und tippen, wird den Controller schließen.

Rufen Sie zum programmgesteuerten schließen den Controller modal angezeigt `DismissController`.

```csharp
DismissController();
```

Modale Bildschirmen können einer einzigen Szene auf oder verwenden Sie ein Layout seitenbasierte sein.

<a name="Hierarchical_Navigation"/>

## <a name="hierarchical-navigation"></a>Hierarchische Navigation

Präsentiert von Szenen wie einen Stapel, der über zurück navigiert, ähnlich wie möglich `UINavigationController` unter iOS arbeitet. Im Hintergrund können mithilfe von Push auf den Navigationsstapel übertragen und geholt (entweder programmgesteuert oder durch Auswahl des Benutzers).

![](navigation-images/hierarchy-1.png "Szenen geschoben werden können, auf dem Navigationsstapel") ![](navigation-images/hierarchy-2.png "können im Hintergrund beim Übersetzen der Navigationsstapel geholt werden")

Navigieren wie bei iOS, eine links-Edge-wischgeste wieder an den übergeordneten Controller in einem Stapel für die hierarchische Navigation.

Sowohl die [WatchKitCatalog](https://developer.xamarin.com/samples/WatchKitCatalog) und [WatchTables](https://developer.xamarin.com/samples/WatchTables) Beispiele umfassen die hierarchische Navigation.

### <a name="pushing-and-popping-in-code"></a>Mithilfe von Push übertragen und entfernt wurde, im Code

Kit erfordert kein bestärken "navigationscontroller" sehen Sie sich einfach erstellt werden, wie iOS - push einen Controller mithilfe der `PushController` -Methode und einem Navigationsstapel automatisch erstellt werden.

```csharp
PushController("secondPageController","some context info");
```

Die Überwachung des Bildschirms enthält einen **wieder** Schaltfläche in der oberen linken Ecke, aber Sie können eine Szene auch programmgesteuert entfernen, aus dem Stapel mit Navigation `PopController`.

```csharp
PopController();
```

Wie iOS, es auch möglich, in das Stammverzeichnis des Stapel für die Navigation mit zurückzugeben `PopToRootController`.

```csharp
PopToRootController();
```

### <a name="using-segues"></a>Mit Segues

Übergänge zwischen Szenen in das Storyboard so definieren Sie die hierarchische Navigation erstellt werden können. Beim Abrufen von Kontext für die Ziel-Szene, die Aufrufe des Betriebssystems `GetContextForSegue` zum Initialisieren der neuen schnittstellencontrollers.

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

## <a name="page-based-interfaces"></a>Ein seitenbasiertes Schnittstellen

Seitenbasierte Schnittstellen Wischen Sie nach links, rechts, ähnlich wie die `UIPageViewController` unter iOS arbeitet. Indikator-Punkte werden am unteren Rand des Bildschirms anzeigen, welche Seite derzeit angezeigt wird angezeigt.

![](navigation-images/paged-1.png "Erste Beispielseite") ![](navigation-images/paged-2.png "zweite Beispielseite") ![](navigation-images/paged-5.png "fünfte Beispielseite")


Verwenden, um einer Seite-basierte Schnittstelle für die Watch-app die Hauptbenutzeroberfläche machen `ReloadRootControllers` mit einem Array von Schnittstellencontroller und Kontexte:

```csharp
var controllerNames = new [] { "pageController", "pageController", "pageController", "pageController", "pageController" };
var contexts = new [] { "First", "Second", "Third", "Fourth", "Fifth" };
ReloadRootControllers (controllerNames, contexts);
```

Sie können auch einen seitenbasierte-Controller, der nicht der Stamm ist darstellen mit `PresentController` aus einem der anderen Szenen in einer app.

```csharp
var controllerNames = new [] { "pageController", "pageController", "pageController", "pageController", "pageController" };
var contexts = new [] { "First", "Second", "Third", "Fourth", "Fifth" };
PresentController (controllerNames, contexts);
```



## <a name="related-links"></a>Verwandte Links

- [WatchKitCatalog (Beispiel)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [WatchTables (Beispiel)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchTables/)
