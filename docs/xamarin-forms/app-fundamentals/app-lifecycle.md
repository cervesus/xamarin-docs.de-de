---
title: Lebenszyklus der Xamarin.Forms-App
description: In diesem Artikel wird erläutert, wie Sie auf den Lebenszyklus der App reagieren, einschließlich Lebenszyklusmethoden, Seitenbenachrichtigungsereignisse und modaler Navigationsereignisse.
ms.prod: xamarin
ms.assetid: 69B416CF-B243-4790-AB29-F030B32465BE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: 41e8d073982bf7963b3a77a939bf28e52e86feaa
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "67675177"
---
# <a name="xamarinforms-app-lifecycle"></a>Lebenszyklus der Xamarin.Forms-App

Die [`Application`](xref:Xamarin.Forms.Application)-Basisklasse bietet folgende Features:

- [Lebenszyklusmethoden:](#Lifecycle_Methods) `OnStart`, `OnSleep` und `OnResume`
- [Seitennavigationsereignisse:](#page) [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing), [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing)
- [Modale Navigationsereignisse:](#modal) `ModalPushing`, `ModalPushed`, `ModalPopping` und `ModalPopped`

<a name="Lifecycle_Methods" />

## <a name="lifecycle-methods"></a>Lebenszyklusmethoden

Die [`Application`](xref:Xamarin.Forms.Application)-Klasse enthält drei virtuelle Methoden, die überarbeitet werden können, um auf Lebenszyklusänderungen zu reagieren:

- `OnStart`: Wird aufgerufen, wenn die App startet.
- `OnSleep`: Wird jedes Mal aufgerufen, wenn die Ausführung der App im Hintergrund fortgesetzt wird.
- `OnResume`: Wird aufgerufen, wenn die App aus dem Hintergrund aufgerufen und fortgesetzt wird.

> [!NOTE]
> Es gibt *keine* Methode zum Beenden der App. Unter normalen Umständen (d.h. kein Absturz) wird die App im Status *OnSleep* beendet, ohne dass Ihr Code darüber benachrichtigt wird.

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

> [!IMPORTANT]
> Unter Android wird die `OnStart`-Methode rotierend und beim ersten Ausführen der App aufgerufen, wenn `ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation` im `[Activity()]`-Attribut in der Hauptaktivität nicht vorhanden ist.

<a name="page" />

## <a name="page-notification-events"></a>Benachrichtigungsereignisse für Seiten

Die [`Application`](xref:Xamarin.Forms.Application)-Klasse verfügt über zwei Ereignisse, die Benachrichtigungen zu angezeigten und ausgeblendeten Seiten bereitstellt:

- [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing): Wird ausgelöst, wenn eine Seite auf dem Bildschirm angezeigt wird.
- [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing): Wird ausgelöst, wenn eine Seite auf dem Bildschirm ausgeblendet wird.

Diese Ereignisse können in Szenarios verwendet werden, bei denen Sie Seiten beim Anzeigen auf dem Bildschirm nachverfolgen möchten.

> [!NOTE]
> Die Ereignisse [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing) und [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing) werden unmittelbar nach den Ereignissen [`Page.Appearing`](xref:Xamarin.Forms.Page.Appearing) und [`Page.Disappearing`](xref:Xamarin.Forms.Page.Disappearing) in der [`Page`](xref:Xamarin.Forms.Page)-Basisklasse ausgelöst.

<a name="modal" />

## <a name="modal-navigation-events"></a>Modale Navigationsereignisse

Es gibt vier Ereignisse in der [`Application`](xref:Xamarin.Forms.Application)-Klasse (jedes enthält eigen Ereignisargumente), mit deren Hilfe Sie auf modale Seiten reagieren können, die angezeigt und geschlossen werden:

- `ModalPushing`: Wird ausgelöst, wenn eine Seite modal mithilfe von Push übertragen wird.
- `ModalPushed`: Wird ausgelöst, nachdem eine Seite modal mithilfe von Push übertragen wurde.
- `ModalPopping`: Wird ausgelöst, wenn eine Seite modal angehalten wird.
- `ModalPopped`: Wird ausgelöst, nachdem eine Seite modal angehalten wurde.

> [!NOTE]
> Die `ModalPopping`-Ereignisargumente vom Typ `ModalPoppingEventArgs` enthalten eine `Cancel`-Eigenschaft. Wenn `Cancel` auf `true` festgelegt ist, wird der modale Pop geschlossen.
