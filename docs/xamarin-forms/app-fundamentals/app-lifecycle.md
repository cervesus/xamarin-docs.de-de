---
title: Xamarin.Forms-App-Lebenszyklus
description: In diesem Artikel wird erläutert, wie auf den Lebenszyklus der Anwendung, wie z.B. Lebenszyklusmethoden Seite Navigationsereignisse und modalen Navigationsereignisse reagieren wird.
ms.prod: xamarin
ms.assetid: 69B416CF-B243-4790-AB29-F030B32465BE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: 5bdce8d1752b3d7273ffec233b120266909999c4
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675119"
---
# <a name="xamarinforms-app-lifecycle"></a>Xamarin.Forms-App-Lebenszyklus

Die [ `Application` ](xref:Xamarin.Forms.Application) Basisklasse bietet die folgenden Features:

* [Lebenszyklusmethoden](#Lifecycle_Methods) `OnStart`, `OnSleep`, und `OnResume`.
* [Seite Navigationsereignisse](#page) [ `PageAppearing` ](xref:Xamarin.Forms.Application.PageAppearing), [ `PageDisappearing` ](xref:Xamarin.Forms.Application.PageDisappearing).
* [Modale Navigationsereignisse](#modal) `ModalPushing`, `ModalPushed`, `ModalPopping`, und `ModalPopped`.

<a name="Lifecycle_Methods" />

## <a name="lifecycle-methods"></a>Lebenszyklusmethoden

Die [ `Application` ](xref:Xamarin.Forms.Application) -Klasse enthält drei virtuelle Methoden, die überschrieben werden können, um Methoden zu verarbeiten:

* **OnStart** -wird aufgerufen, wenn die Anwendung gestartet wird.

* **OnSleep** -jedes Mal aufgerufen, die Anwendung, die in den Hintergrund wechselt.

* **OnResume** -wird aufgerufen, wenn die Anwendung fortgesetzt wird, nach dem in den Hintergrund gesendet werden,.

Beachten Sie, dass *keine* Methode zum Beenden der Anwendung.
Unter normalen Umständen (z. B. keinen Absturz) erfolgt die Beendigung der Anwendung aus der *OnSleep* Zustands, ohne zusätzlichen Benachrichtigungen an Ihrem Code.

Um zu beobachten, wenn diese Methoden aufgerufen werden, implementieren Sie eine `WriteLine` Aufrufen in den einzelnen (wie unten gezeigt) und auf jeder Plattform zu testen.

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

Bei der Aktualisierung *ältere* Xamarin.Forms-Anwendungen (z. b. Erstellen Sie mit Xamarin.Forms 1.3 oder älter), stellen Sie sicher, dass die Android Hauptaktivität enthält `ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation` in die `[Activity()]` Attribut. Wenn dies nicht vorhanden ist werden Sie beobachten das `OnStart` Methode wird aufgerufen, für die Rotation, sowie beim ersten der Anwendung Start. Dieses Attribut ist automatisch in die aktuelle Xamarin.Forms-app-Vorlagen enthalten.

<a name="page" />

## <a name="page-navigation-events"></a>Seitennavigation-Ereignisse

Es gibt zwei Ereignisse auf die [ `Application` ](xref:Xamarin.Forms.Application) Klasse, die Benachrichtigung über die Seiten angezeigt werden, und wieder verschwindet bereitstellen:

- [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing) : wird ausgelöst, wenn eine Seite ist auf dem Bildschirm angezeigt werden soll.
- [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing) : wird ausgelöst, wenn eine Seite nicht mehr auf dem Bildschirm angezeigt wird.

Diese Ereignisse können in Szenarien verwendet, in dem Sie Seiten nachverfolgen, wie sie im Bildschirm angezeigt werden soll.

> [!NOTE]
> Die [ `PageAppearing` ](xref:Xamarin.Forms.Application.PageAppearing) und [ `PageDisappearing` ](xref:Xamarin.Forms.Application.PageDisappearing) Ereignisse ausgelöst werden, aus der [ `Page` ](xref:Xamarin.Forms.Page) Basisklasse unmittelbar nach der [ `Page.Appearing` ](xref:Xamarin.Forms.Page.Appearing) und [ `Page.Disappearing` ](xref:Xamarin.Forms.Page.Disappearing) Ereignisse bzw.

<a name="modal" />

## <a name="modal-navigation-events"></a>Modale Navigation-Ereignisse

Es gibt vier Ereignisse auf die [ `Application` ](xref:Xamarin.Forms.Application) -Klasse, jeweils mit eigenen Ereignisargumente, mit denen Sie Antworten auf modale Seiten angezeigt und geschlossen werden:

* **ModalPushing** - `ModalPushingEventArgs`
* **ModalPushed** - `ModalPushedEventArgs`
* **ModalPopping** : die `ModalPoppingEventArgs` -Klasse enthält eine `Cancel` Eigenschaft. Wenn `Cancel` nastaven NA hodnotu `true` modale Pop wird abgebrochen.
* **ModalPopped** - `ModalPoppedEventArgs`

> [!NOTE]
> Zur Implementierung der Anwendung Lebenszyklusmethoden und modalen Navigation-Ereignisse, die alle vor`Application` Methoden zum Erstellen einer Xamarin.Forms-app (d. h. Anwendungen in Version 1.2 oder früher geschrieben, die eine statische `GetMainPage` Methode) aktualisiert, um das Erstellen einer standardmäßige `Application` festgelegt wird, als das übergeordnete Element des `MainPage`.
>
> Xamarin.Forms-Anwendungen, die dieses Legacyverhalten verwenden müssen aktualisiert werden, um eine `Application` Unterklasse, die unter der [Anwendungsklasse](~/xamarin-forms/app-fundamentals/application-class.md) Seite.
