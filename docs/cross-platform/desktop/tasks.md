---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: Vergleich allgemeiner Aufgaben
description: In diesem Dokument wird erläutert, wie verschiedene häufige Aufgaben in WPF und xamarin. Forms ausgeführt werden. Es werden Schaltflächen, Timer, Schriftgrößen, das Öffnen eines URIs und das Anzeigen eines Aktions Blatts untersucht.
author: davidortinau
ms.author: daortin
ms.date: 04/26/2017
ms.openlocfilehash: d1f1430d8044f94c17a19d747334b5ed8a1441cf
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016420"
---
# <a name="common-tasks-comparison"></a>Vergleich allgemeiner Aufgaben

| Aufgabe | WPF | Xamarin.Forms |
|--- |--- |--- |
|Anzeigen einer Meldung auf dem Bildschirm mit Schaltflächen|`MessageBox`|`Page.DisplayAlert`|
|Erstellen eines Timers|`DispatcherTimer`-Klasse|`Device.StartTimer` statische Methode|
|Standardschrift Größe erhalten|statische `SystemFonts`-Klasse|`Device.GetNamedSize` statische Methode|
|Öffnen eines URI/einer URL|`Process.Start`|`Device.OpenUri`|
|Aktions Blatt anzeigen (Liste der Schaltflächen)|n/v|`Page.DisplayActionSheet`|
