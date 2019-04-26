---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: Allgemeine Aufgaben-Vergleich
description: In diesem Dokument vergleicht, wie Sie zum Ausführen verschiedener allgemeiner Aufgaben für WPF und Xamarin.Forms. Er untersucht die Schaltflächen, Timer, Schriftgrößen, öffnen einen URI und ein aktionsblatt angezeigt.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 2991e74ec59e3c43c665941706c2d9ac94bfb9ad
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61269594"
---
# <a name="common-tasks-comparison"></a>Allgemeine Aufgaben-Vergleich

| Aufgabe | WPF | Xamarin.Forms |
|--- |--- |--- |
|Eine Meldung angezeigt, auf dem Bildschirm mit den Schaltflächen|`MessageBox`|`Page.DisplayAlert`|
|Erstellen Sie einen timer|`DispatcherTimer`-Klasse|`Device.StartTimer` statische Methode|
|Abrufen einer Standardschriftgröße|`SystemFonts` statische Klasse|`Device.GetNamedSize` statische Methode|
|Öffnen Sie eine URI-URL|`Process.Start`|`Device.OpenUri`|
|Anzeigen von aktionsblatt (Liste der Schaltflächen)|n/v|`Page.DisplayActionSheet`|
