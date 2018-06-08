---
title: App-Lebenszyklus
description: Reagieren auf Anwendungslebenszyklus
ms.prod: xamarin
ms.assetid: 69B416CF-B243-4790-AB29-F030B32465BE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: a22ad8f3f272212f5c7f088ba2112f2771ff4a7f
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34846343"
---
# <a name="app-lifecycle"></a>App-Lebenszyklus

Die [ `Application` ](xref:Xamarin.Forms.Application) Basisklasse bietet die folgenden Funktionen:

* [Lebenszyklusmethoden](#Lifecycle_Methods) `OnStart`, `OnSleep`, und `OnResume`.
* [Seite Navigationsereignisse](#page) [ `PageAppearing` ](xref:Xamarin.Forms.Application.PageAppearing), [ `PageDisappearing` ](xref:Xamarin.Forms.Application.PageDisappearing).
* [Modale Navigationsereignisse](#modal) `ModalPushing`, `ModalPushed`, `ModalPopping`, und `ModalPopped`.

<a name="Lifecycle_Methods" />

## <a name="lifecycle-methods"></a>Lebenszyklusmethoden

Die [ `Application` ](xref:Xamarin.Forms.Application) Klasse enthält drei virtuelle Methoden, die überschrieben werden können, um die Lebenszyklusmethoden behandeln:

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

<a name="page" />

## <a name="page-navigation-events"></a>Seitennavigation-Ereignisse

Es gibt zwei Ereignisse auf die [ `Application` ](xref:Xamarin.Forms.Application) Klasse, die Benachrichtigung von Seiten und bereitstellen:

- [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing) -wird ausgelöst, wenn eine Seite auf dem Bildschirm angezeigt werden soll.
- [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing) -ausgelöst, wenn eine Seite nicht mehr aus dem Bildschirm angezeigt wird.

Diese Ereignisse können in Szenarien verwendet, an die Seiten nachverfolgen, wie sie auf dem Bildschirm angezeigt werden soll.

> [!NOTE]
> Die [ `PageAppearing` ](xref:Xamarin.Forms.Application.PageAppearing) und [ `PageDisappearing` ](xref:Xamarin.Forms.Application.PageDisappearing) Ereignisse werden ausgelöst, aus der [ `Page` ](xref:Xamarin.Forms.Page) Basisklasse unmittelbar nach der [ `Page.Appearing` ](xref:Xamarin.Forms.Page.Appearing) und [ `Page.Disappearing` ](xref:Xamarin.Forms.Page.Disappearing) Ereignisse bzw.

<a name="modal" />

## <a name="modal-navigation-events"></a>Modale Navigationsereignisse

Es gibt vier Ereignisse auf die [ `Application` ](xref:Xamarin.Forms.Application) -Klasse, jeweils über eigene Ereignisargumente, mit denen Sie Antworten auf modale Seiten angezeigt und ausgeblendet wird:

* **ModalPushing** - `ModalPushingEventArgs`
* **ModalPushed** - `ModalPushedEventArgs`
* **ModalPopping** – der `ModalPoppingEventArgs` Klasse enthält eine `Cancel` Eigenschaft. Wenn `Cancel` festgelegt ist, um `true` modale Pop wird abgebrochen.
* **ModalPopped** - `ModalPoppedEventArgs`

> [!NOTE]
> Zur Implementierung von der Anwendung Lebenszyklusmethoden und modale Navigationsereignisse, alle vor`Application` Methoden zum Erstellen einer app Xamarin.Forms (ie. Anwendungen, die in Version 1.2 oder ältere geschrieben, die eine statische `GetMainPage` Methode) wurden aktualisiert, um das Erstellen einer standardmäßige `Application` die festgelegt wird, als das übergeordnete Element des `MainPage`.
>
> Xamarin.Forms-Anwendungen, die dieses Legacyverhalten verwendet müssen aktualisiert werden, um eine `Application` Unterklasse auf gemäß der [Anwendungsklasse](~/xamarin-forms/app-fundamentals/application-class.md) Seite.
