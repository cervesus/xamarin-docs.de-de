---
title: Lebenszyklus der Xamarin.Forms-App
description: In diesem Artikel wird erläutert, wie Sie auf den Lebenszyklus der App reagieren, einschließlich Lebenszyklusmethoden, Seitennavigationsereignisse und modaler Navigationsereignisse.
ms.prod: xamarin
ms.assetid: 69B416CF-B243-4790-AB29-F030B32465BE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: 5bdce8d1752b3d7273ffec233b120266909999c4
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675119"
---
# <a name="xamarinforms-app-lifecycle"></a>Lebenszyklus der Xamarin.Forms-App

Die [`Application`](xref:Xamarin.Forms.Application)-Basisklasse bietet folgende Features:

* [Lebenszyklusmethoden:](#Lifecycle_Methods) `OnStart`, `OnSleep` und `OnResume`
* [Seitennavigationsereignisse:](#page) [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing), [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing)
* [Modale Navigationsereignisse:](#modal) `ModalPushing`, `ModalPushed`, `ModalPopping` und `ModalPopped`

<a name="Lifecycle_Methods" />

## <a name="lifecycle-methods"></a>Lebenszyklusmethoden

Die [`Application`](xref:Xamarin.Forms.Application)-Klasse enthält drei virtuelle Methoden, die zum Bearbeiten von Lebenszyklusmethoden überschrieben werden können:

* **OnStart:** Wird aufgerufen, wenn die App startet.

* **OnSleep:** Wird jedes Mal aufgerufen, wenn die Ausführung der App im Hintergrund fortgesetzt wird.

* **OnResume:** Wird aufgerufen, wenn die App aus dem Hintergrund aufgerufen und fortgesetzt wird.

Beachten Sie, dass es *keine* Methode zum Beenden der App gibt.
Unter normalen Umständen (d.h. kein Absturz) wird die App im Status *OnSleep* beendet, ohne dass Ihr Code darüber benachrichtigt wird.

Implementieren Sie wie nachfolgend veranschaulicht einen `WriteLine`-Aufruf in jeder Methode, und testen Sie diesen auf jeder Plattform. So können Sie beobachten, wann die Methoden aufgerufen werden.

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

Stellen Sie beim Aktualisieren *älterer* Xamarin.Forms-Apps (z.B. mit Xamarin.Forms 1.3 oder älter) sicher, dass die Hauptaktivität von Android `ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation` im Attribut `[Activity()]` enthält. Wenn dies nicht vorhanden ist, werden Sie beobachten, dass die `OnStart`-Methode rotierend sowie beim ersten Ausführen der App aufgerufen wird. Dieses Attribut ist in der aktuellen App-Vorlage von Xamarin.Forms automatisch enthalten.

<a name="page" />

## <a name="page-navigation-events"></a>Seitennavigationsereignisse

Die [`Application`](xref:Xamarin.Forms.Application)-Klasse verfügt über zwei Ereignisse, die Benachrichtigungen zu angezeigten und ausgeblendeten Seiten bereitstellt:

- [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing): Wird ausgelöst, wenn eine Seite auf dem Bildschirm angezeigt wird.
- [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing): Wird ausgelöst, wenn eine Seite auf dem Bildschirm ausgeblendet wird.

Diese Ereignisse können in Szenarios verwendet werden, bei denen Sie Seiten beim Anzeigen auf dem Bildschirm nachverfolgen möchten.

> [!NOTE]
> Die Ereignisse [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing) und [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing) werden unmittelbar nach den Ereignissen [`Page.Appearing`](xref:Xamarin.Forms.Page.Appearing) und [`Page.Disappearing`](xref:Xamarin.Forms.Page.Disappearing) in der [`Page`](xref:Xamarin.Forms.Page)-Basisklasse ausgelöst.

<a name="modal" />

## <a name="modal-navigation-events"></a>Modale Navigationsereignisse

Es gibt vier Ereignisse in der [`Application`](xref:Xamarin.Forms.Application)-Klasse (jedes enthält eigen Ereignisargumente), mit deren Hilfe Sie auf modale Seiten reagieren können, die angezeigt und geschlossen werden:

* **ModalPushing** - `ModalPushingEventArgs`
* **ModalPushed** - `ModalPushedEventArgs`
* **ModalPopping:** Die Klasse `ModalPoppingEventArgs` enthält eine Eigenschaft `Cancel`. Wenn `Cancel` auf `true` festgelegt ist, wird der modale Pop geschlossen.
* **ModalPopped** - `ModalPoppedEventArgs`

> [!NOTE]
> Zur Implementierung der Lebenszyklusmethoden und modalen Navigationsereignisse der App wurden alle Methoden vor `Application` zum Erstellen einer Xamarin.Forms-App (d.h. Apps mit der Version 1.2 oder älter, die eine statische `GetMainPage`-Methode verwenden) aktualisiert, um eine `Application`-Standardklasse zu erstellen, die `MainPage` übergeordnet ist.
>
> Xamarin.Forms-Apps, die dieses Legacyverhalten verwenden, müssen auf eine `Application`-Unterklasse aktualisiert werden. Dies wird auf der Seite [Application class (App-Klasse)](~/xamarin-forms/app-fundamentals/application-class.md) beschrieben.
