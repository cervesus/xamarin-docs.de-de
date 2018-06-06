---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: Allgemeine Aufgaben Vergleich
description: Dieses Dokument vergleicht, wie verschiedene häufige Aufgaben für WPF und Xamarin.Forms ausführen. Es geht um Schaltflächen, Timer und Schriftgrade Ihren Bedürfnissen, öffnen einen URI und zum Anzeigen einer Aktion-Tabelle.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 2991e74ec59e3c43c665941706c2d9ac94bfb9ad
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780475"
---
# <a name="common-tasks-comparison"></a>Allgemeine Aufgaben Vergleich

| Aufgabe | WPF | Xamarin.Forms |
|--- |--- |--- |
|Eine Meldung angezeigt, auf dem Bildschirm mit den Schaltflächen|`MessageBox`|`Page.DisplayAlert`|
|Erstellen eines Zeitgebers|`DispatcherTimer`-Klasse|`Device.StartTimer` statische Methode|
|Abrufen einer Standardschriftgröße|`SystemFonts` statische Klasse|`Device.GetNamedSize` statische Methode|
|Öffnen Sie eine URI-URL|`Process.Start`|`Device.OpenUri`|
|Anzeigen der Aktion Blatt (Liste der Schaltflächen)|n/v|`Page.DisplayActionSheet`|
