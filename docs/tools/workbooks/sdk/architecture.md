---
title: Architekturübersicht
description: In diesem Dokument wird die Architektur von Xamarin Workbooks beschrieben und erläutert, wie der interaktive Agent und der interaktive Client zusammenarbeiten.
ms.prod: xamarin
ms.assetid: 6C0226BE-A0C4-4108-B482-0A903696AB04
author: conceptdev
ms.author: crdun
ms.date: 03/30/2017
ms.openlocfilehash: 7129d0bedddb272ef87e3d209cb05c2ca0c0acf4
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70285279"
---
# <a name="architecture-overview"></a>Architekturübersicht

Xamarin Workbooks verfügt über zwei Hauptkomponenten, die zusammenarbeiten müssen: _Agent_ und _Client_.

## <a name="interactive-agent"></a>Interaktiver Agent

Bei der Agent-Komponente handelt es sich um eine kleine plattformspezifische Assembly, die im Kontext einer .NET-Anwendung ausgeführt wird.

Xamarin Workbooks bietet vorgefertigte "leere" Anwendungen für eine Reihe von Plattformen, wie z. b. IOS, Android, Mac und WPF. Diese Anwendungen hosten den Agent explizit.

Während der Live Untersuchung (Xamarin Inspector) wird der Agent über den IDE-Debugger in eine vorhandene Anwendung als Teil des regulären Entwicklung& debuggingworkflows eingefügt.

## <a name="interactive-client"></a>Interaktiver Client

Der Client ist eine native Shell (Cocoa on Mac, WPF unter Windows), die eine Webbrowser Oberfläche zum Darstellen der Arbeitsmappen/repl-Schnittstelle hostet. Aus der SDK-Perspektive werden alle Client Integrationen in JavaScript und CSS implementiert.

Der Client ist verantwortlich (über Roslyn), um Quellcode in kleine Assemblys zu kompilieren und zur Ausführung an den verbundenen Agent zu senden. Die Ergebnisse der Ausführung werden zum Rendern zurück an den Client gesendet. Jede Zelle in einer Arbeitsmappe ergibt eine Assembly, die auf die Assembly der vorherigen Zelle verweist.

Da ein Agent auf jeder Art von .NET-Plattform ausgeführt werden kann und Zugriff auf alle Elemente in der laufenden Anwendung hat, muss darauf geachtet werden, dass die Ergebnisse Platt Form agnostisch serialisiert werden.
