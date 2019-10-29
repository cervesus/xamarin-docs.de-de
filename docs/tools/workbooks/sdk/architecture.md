---
title: Architekturübersicht
description: In diesem Dokument wird die Architektur von Xamarin Workbooks beschrieben und erläutert, wie der interaktive Agent und der interaktive Client zusammenarbeiten.
ms.prod: xamarin
ms.assetid: 6C0226BE-A0C4-4108-B482-0A903696AB04
author: davidortinau
ms.author: daortin
ms.date: 03/30/2017
ms.openlocfilehash: 7b3f2613e315bc05fedfb5b2fa70d11c2874ba65
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029617"
---
# <a name="architecture-overview"></a>Architekturübersicht

Xamarin Workbooks bietet zwei Hauptkomponenten, die zusammenarbeiten müssen:- _Agent_ und- _Client_.

## <a name="interactive-agent"></a>Interaktiver Agent

Bei der Agent-Komponente handelt es sich um eine kleine plattformspezifische Assembly, die im Kontext einer .NET-Anwendung ausgeführt wird.

Xamarin Workbooks bietet vorgefertigte "leere" Anwendungen für eine Reihe von Plattformen, wie z. b. IOS, Android, Mac und WPF. Diese Anwendungen hosten den Agent explizit.

Während der Live Untersuchung (Xamarin Inspector) wird der Agent über den IDE-Debugger in eine vorhandene Anwendung als Teil des regulären Entwicklung& debuggingworkflows eingefügt.

## <a name="interactive-client"></a>Interaktiver Client

Der Client ist eine native Shell (Cocoa on Mac, WPF unter Windows), die eine Webbrowser Oberfläche zum Darstellen der Arbeitsmappen/repl-Schnittstelle hostet. Aus der SDK-Perspektive werden alle Client Integrationen in JavaScript und CSS implementiert.

Der Client ist verantwortlich (über Roslyn), um Quellcode in kleine Assemblys zu kompilieren und zur Ausführung an den verbundenen Agent zu senden. Die Ergebnisse der Ausführung werden zum Rendern zurück an den Client gesendet. Jede Zelle in einer Arbeitsmappe ergibt eine Assembly, die auf die Assembly der vorherigen Zelle verweist.

Da ein Agent auf jeder Art von .NET-Plattform ausgeführt werden kann und Zugriff auf alle Elemente in der laufenden Anwendung hat, muss darauf geachtet werden, dass die Ergebnisse Platt Form agnostisch serialisiert werden.
