---
title: 'title: "Xamarin.Forms – Shelllebenszyklus" description: "Shellanwendungen respektieren den Lebenszyklus von Xamarin.Forms, und ein „Appearing“-Ereignis wird ausgelöst, wenn eine Seite auf dem Bildschirm angezeigt werden soll. Ein „Disappearing“-Ereignis wird ausgelöst, wenn eine Seite in Kürze nicht mehr auf dem Bildschirm angezeigt werden soll."'
description: 'ms.prod: xamarin ms.assetid: 4E4EE50E-3BB4-441D-8355-CD9CD26ED1D0 ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 07/25/2019 no-loc: [Xamarin.Forms, Xamarin.Essentials]'
ms.prod: xamarin
ms.assetid: 4E4EE50E-3BB4-441D-8355-CD9CD26ED1D0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/25/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3a7a46187d861098b61f638a3fb460d890b081dd
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2020
ms.locfileid: "84138722"
---
# <a name="xamarinforms-shell-lifecycle"></a>Lebenszyklus der Xamarin.Forms-Shell

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Für Shell-Anwendungen gilt der Xamarin.Forms-Lebenszyklus. Deswegen wird beim Aufrufen einer Seite für die bevorstehende Bildschirmanzeige ein `Appearing`-Ereignis und beim bevorstehenden Entfernen der Seite vom Bildschirm ein `Disappearing`-Ereignis ausgelöst. Diese Ereignisse werden an Seiten weitergegeben und können durch Überschreiben der [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing)- oder [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing)-Methoden auf der Seite verarbeitet werden.

> [!NOTE]
> In einer Shellanwendung werden die `Appearing`- und `Disappearing`-Ereignisse durch plattformübergreifenden Code ausgelöst, bevor der Plattformcode eine Seite sichtbar macht oder eine Seite vom Bildschirm entfernt.

Weitere Informationen zum Lebenszyklus von Xamarin.Forms-Apps finden Sie unter [Lebenszyklus der Xamarin.Forms-App](~/xamarin-forms/app-fundamentals/app-lifecycle.md).

## <a name="hierarchical-navigation"></a>Hierarchische Navigation

In einer Shellanwendung führt das Pushen einer Seite auf den Navigationsstapel dazu, dass das aktuell sichtbare `ShellContent`-Objekt und der zugehörige Seiteninhalt das `Disappearing`-Ereignis auslösen. Analog dazu führt das Entfernen der letzten Seite vom Navigationsstapel dazu, dass das neu sichtbare `ShellContent`-Objekt und der zugehörige Seiteninhalt das Ereignis `Appearing` auslösen.

Weitere Informationen zur hierarchischen Navigation finden Sie unter [Xamarin.Forms: Hierarchische Navigation](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

## <a name="modal-navigation"></a>Modale Navigation

In einer Shellanwendung führt das Pushen einer modalen Seite auf den modalen Navigationsstapel dazu, dass alle sichtbaren Shellobjekte das `Disappearing`-Ereignis auslösen. Analog dazu führt das Entfernen der letzten modalen Seite vom modalen Navigationsstapel dazu, dass alle sichtbaren Shellobjekte das `Appearing`-Ereignis auslösen.

Weitere Informationen zur modalen Navigation finden Sie unter [Modale Xamarin.Forms-Seiten](~/xamarin-forms/app-fundamentals/navigation/modal.md).

## <a name="related-links"></a>Verwandte Links

- [Xaminals (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
- [Lebenszyklus der Xamarin.Forms-App](~/xamarin-forms/app-fundamentals/app-lifecycle.md)
- [Modale Xamarin.Forms-Seiten](~/xamarin-forms/app-fundamentals/navigation/modal.md)
