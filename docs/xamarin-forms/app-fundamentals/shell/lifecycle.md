---
title: Lebenszyklus der Xamarin.Forms-Shell
description: Shellanwendungen respektieren den Lebenszyklus von Xamarin.Forms, und ein Appearing-Ereignis wird ausgelöst, wenn eine Seite auf dem Bildschirm angezeigt werden soll. Ein Disappearing-Ereignis wird ausgelöst, wenn eine Seite im Begriff ist, nicht mehr auf dem Bildschirm angezeigt zu werden.
ms.prod: xamarin
ms.assetid: 4E4EE50E-3BB4-441D-8355-CD9CD26ED1D0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/25/2019
ms.openlocfilehash: 2ed51763b5866c15e91d88a6a1a58c7285fb5973
ms.sourcegitcommit: e71474f91639bb43159b22f5d534325c3270ba93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72749769"
---
# <a name="xamarinforms-shell-lifecycle"></a>Lebenszyklus der Xamarin.Forms-Shell

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Shellanwendungen respektieren den Lebenszyklus von Xamarin.Forms, und ein `Appearing`-Ereignis wird ausgelöst, wenn eine Seite auf dem Bildschirm angezeigt werden soll. Ein `Disappearing`-Ereignis wird ausgelöst, wenn eine Seite im Begriff ist, nicht mehr auf dem Bildschirm angezeigt zu werden. Diese Ereignisse werden an Seiten weitergegeben und können durch Überschreiben der [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing)- oder [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing)-Methoden auf der Seite verarbeitet werden.

> [!NOTE]
> In einer Shellanwendung werden die `Appearing`- und `Disappearing`-Ereignisse durch plattformübergreifenden Code ausgelöst, bevor der Plattformcode eine Seite sichtbar macht oder eine Seite vom Bildschirm entfernt.

Weitere Informationen zum Lebenszyklus der Xamarin.Forms-App finden Sie unter [Lebenszyklus der Xamarin.Forms-App](~/xamarin-forms/app-fundamentals/app-lifecycle.md).

## <a name="hierarchical-navigation"></a>Hierarchische Navigation

In einer Shellanwendung führt das Pushen einer Seite auf den Navigationsstapel dazu, dass das aktuell sichtbare `ShellContent`-Objekt und der zugehörige Seiteninhalt das `Disappearing`-Ereignis auslösen. Analog dazu führt das Entfernen der letzten Seite vom Navigationsstapel dazu, dass das neu sichtbare `ShellContent`-Objekt und der zugehörige Seiteninhalt das Ereignis `Appearing` auslösen.

Weitere Informationen zur hierarchischen Navigation finden Sie unter [Hierarchische Xamarin.Forms-Navigation](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

## <a name="modal-navigation"></a>Modale Navigation

In einer Shellanwendung führt das Pushen einer modalen Seite auf den modalen Navigationsstapel dazu, dass alle sichtbaren Shellobjekte das `Disappearing`-Ereignis auslösen. Analog dazu führt das Entfernen der letzten modalen Seite vom modalen Navigationsstapel dazu, dass alle sichtbaren Shellobjekte das `Appearing`-Ereignis auslösen.

Weitere Informationen zur modalen Navigation finden Sie unter [Modale Xamarin.Forms-Seiten](~/xamarin-forms/app-fundamentals/navigation/modal.md).

## <a name="related-links"></a>Verwandte Links

- [Xaminals (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
- [Lebenszyklus der Xamarin.Forms-App](~/xamarin-forms/app-fundamentals/app-lifecycle.md)
- [Modale Xamarin.Forms-Seiten](~/xamarin-forms/app-fundamentals/navigation/modal.md)
