---
title: Architekturübersicht
ms.prod: xamarin
ms.assetid: 6C0226BE-A0C4-4108-B482-0A903696AB04
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: 5a836ec6e2e46316c46e4b6d6986d08601f84db1
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
---
# <a name="architecture-overview"></a>Architekturübersicht

Xamarin-Arbeitsmappen verfügt über zwei Hauptkomponenten, die in Verbindung miteinander arbeiten zu müssen: _Agent_ und _Client_.

## <a name="interactive-agent"></a>Interaktive Agent

Die Agent-Komponente ist eine kleine plattformspezifischen-Assembly, die im Kontext einer Anwendung .NET ausgeführt wird.

Xamarin-Arbeitsmappen enthält vorgefertigte "empty" Anwendungen für eine Reihe von Plattformen wie iOS, Android, Mac und WPF. Diese Anwendungen hosten explizit den Agent.

Während der live-Prüfung (Xamarin-Inspektor) wird der Agent über die IDE-Debugger in eine vorhandene Anwendung als Teil der regulären Entwicklung und Debuggen von Workflows eingefügt.

## <a name="interactive-client"></a>Interaktiven Client

Der Client ist eine systemeigene Shell (Kakao auf einem Mac, WPF unter Windows), die eine Web-Browser-Oberfläche zum Darstellen der Arbeitsmappe/REPL-Schnittstelle hostet. Aus Sicht SDK werden alle Client-Integrationen in JavaScript und CSS-implementiert.

Der Client ist dafür verantwortlich (über Roslyn) zum Kompilieren von Quellcode in kleinen Assemblys und sie über die verbundenen Agent für die Ausführung zu senden. Ergebnisse der Ausführung werden für das Rendering zurück an den Client gesendet. Jede Zelle in einer Arbeitsmappe ergibt eine Assembly, die die Assembly der vorherigen Zelle verweist.

Daran, dass ein Agent auf eine beliebige Art von .NET Plattform ausgeführt werden kann und Zugriff auf alle Elemente in der laufenden Anwendung hat, muss darauf geachtet werden, Ergebnisse in einer Weise plattformunabhängigem serialisiert werden sollen.