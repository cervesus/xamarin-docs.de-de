---
title: App-Lebenszyklus
description: Reagieren auf Anwendungslebenszyklus
ms.prod: xamarin
ms.assetid: 69B416CF-B243-4790-AB29-F030B32465BE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/19/2016
ms.openlocfilehash: 511591482a0e7512be34f6a210c6f44a1826be24
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="app-lifecycle"></a>App-Lebenszyklus

Die `Application` Basisklasse bietet die folgenden Funktionen:

* [Lebenszyklusmethoden](#Lifecycle_Methods) `OnStart`, `OnSleep`, und `OnResume`.
* [Modale Navigationsereignisse](#modal) `ModalPushing`, `ModalPushed`, `ModalPopping`, und `ModalPopped`.

<a name="Lifecycle_Methods" />

## <a name="lifecycle-methods"></a>Lebenszyklusmethoden

Die `Application` Klasse enthält drei virtuelle Methoden, die überschrieben werden können, um die Lebenszyklusmethoden behandeln:

* **OnStart** -wird aufgerufen, wenn die Anwendung gestartet wird.

* **OnSleep** -jedes Mal aufgerufen, die Anwendung in den Hintergrund wechselt.

* **OnResume** -wird aufgerufen, wenn die Anwendung wieder aufgenommen wird, nach dem Hintergrund gesendet werden.

Beachten Sie, dass es *keine* Methode zum Beenden der Anwendung.
Unter normalen Umständen (ie. keinem Absturz) Beenden der Anwendung erfolgt über die *OnSleep* -Zustand aufweist, ohne zusätzlichen Benachrichtigungen für Ihren Code.

Um zu betrachten, wenn diese Methoden aufgerufen werden, implementieren ein `WriteLine` Aufrufen in jedem (wie unten gezeigt) und auf jeder Plattform zu testen.

```csharp
protected override void OnStart()
{
    Debug.WriteLine ("OnStart");
}
protected override void OnSleep()
{
    Debug.WriteLine ("OnSleep");
}
protected override void OnResume()
{
    Debug.WriteLine ("OnResume");
}
```

Beim Aktualisieren von *ältere* Xamarin.Forms-Anwendungen (z. b. Erstellen Sie mit Xamarin.Forms 1.3 oder älter), stellen Sie sicher, dass das Android Hauptaktivität enthält `ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation` in die `[Activity()]` Attribut. Wenn dies nicht vorhanden ist bemerken die `OnStart` Methode wird aufgerufen, für Drehung sowie beim ersten der Anwendung starten. Dieses Attribut ist automatisch in den aktuellen Xamarin.Forms-app-Vorlagen enthalten.

<a name="modal" />

## <a name="modal-navigation-events"></a>Modale Navigationsereignisse

Es gibt vier neue Ereignisse auf die `Application` Klasse in Xamarin.Forms 1.4, jeweils über eigene Ereignisargumente:

* **ModalPushing** - `ModalPushingEventArgs`
* **ModalPushed** - `ModalPushedEventArgs`
* **ModalPopping** – der `ModalPoppingEventArgs` Klasse enthält eine `Cancel` Eigenschaft. Wenn `Cancel` festgelegt ist, um `true` modale Pop wird abgebrochen.
* **ModalPopped** - `ModalPoppedEventArgs`

Diese Ereignisse hilft Ihnen bei den Anwendungslebenszyklus besser verwalten, indem ermöglicht Reaktion auf einen modalen Seiten angezeigt und verworfen wird.

> [!NOTE]
> Zur Implementierung von der Anwendung Lebenszyklusmethoden und modale Navigationsereignisse, alle vor`Application` Methoden zum Erstellen einer app Xamarin.Forms (ie. Anwendungen, die in Version 1.2 oder ältere geschrieben, die eine statische `GetMainPage` Methode) wurden aktualisiert, um das Erstellen einer standardmäßige `Application` die festgelegt wird, als das übergeordnete Element des `MainPage`.
>
> Xamarin.Forms-Anwendungen, die dieses Legacyverhalten verwendet müssen aktualisiert werden, um eine `Application` Unterklasse auf gemäß der [Anwendungsklasse](~/xamarin-forms/app-fundamentals/application-class.md) Seite.
