---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: Vergleich allgemeiner Aufgaben
description: In diesem Dokument wird erläutert, wie verschiedene häufige Aufgaben in WPF und xamarin. Forms ausgeführt werden. Es werden Schaltflächen, Timer, Schriftgrößen, das Öffnen eines URIs und das Anzeigen eines Aktions Blatts untersucht.
author: conceptdev
ms.author: crdun
ms.date: 04/26/2017
ms.openlocfilehash: fecd8ed774adbacf69e3b2db514a4698e71711d8
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290328"
---
# <a name="common-tasks-comparison"></a>Vergleich allgemeiner Aufgaben

| Aufgabe | WPF | Xamarin.Forms |
|--- |--- |--- |
|Anzeigen einer Meldung auf dem Bildschirm mit Schaltflächen|`MessageBox`|`Page.DisplayAlert`|
|Erstellen eines Timers|`DispatcherTimer`-Klasse|`Device.StartTimer`statische Methode|
|Standardschrift Größe erhalten|`SystemFonts`statische Klasse|`Device.GetNamedSize`statische Methode|
|Öffnen eines URI/einer URL|`Process.Start`|`Device.OpenUri`|
|Aktions Blatt anzeigen (Liste der Schaltflächen)|n/v|`Page.DisplayActionSheet`|
