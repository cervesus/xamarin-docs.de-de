---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: Allgemeine Aufgaben Vergleich
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: b4d45ae61c5d7a7093d51c4440d11dd883c157a4
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="common-tasks-comparison"></a>Allgemeine Aufgaben Vergleich

| Aufgabe | WPF | Xamarin.Forms |
|--- |--- |--- |
|Eine Meldung angezeigt, auf dem Bildschirm mit den Schaltflächen|`MessageBox`|`Page.DisplayAlert`|
|Erstellen eines Zeitgebers|`DispatcherTimer`-Klasse|`Device.StartTimer` statische Methode|
|Abrufen einer Standardschriftgröße|`SystemFonts` statische Klasse|`Device.GetNamedSize` statische Methode|
|Öffnen Sie eine URI-URL|`Process.Start`|`Device.OpenUri`|
|Anzeigen der Aktion Blatt (Liste der Schaltflächen)|n/v|`Page.DisplayActionSheet`|
