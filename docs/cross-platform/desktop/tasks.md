---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: Allgemeine Aufgaben Vergleich
ms.technology: xamarin-crossplatform
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 84b3edc1a8cbc9ad642d94b457321786fae5fe70
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
---
# <a name="common-tasks-comparison"></a>Allgemeine Aufgaben Vergleich

| Aufgabe | WPF | Xamarin.Forms |
|--- |--- |--- |
|Eine Meldung angezeigt, auf dem Bildschirm mit den Schaltflächen|`MessageBox`|`Page.DisplayAlert`|
|Erstellen eines Zeitgebers|`DispatcherTimer`-Klasse|`Device.StartTimer` statische Methode|
|Abrufen einer Standardschriftgröße|`SystemFonts` statische Klasse|`Device.GetNamedSize` statische Methode|
|Öffnen Sie eine URI-URL|`Process.Start`|`Device.OpenUri`|
|Anzeigen der Aktion Blatt (Liste der Schaltflächen)|n/v|`Page.DisplayActionSheet`|
