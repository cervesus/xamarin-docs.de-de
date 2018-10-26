---
title: Architekturübersicht
description: Dieses Dokument beschreibt die Architektur von Xamarin Workbooks, überprüfen, wie die interaktive Agent und die interaktiven Client zusammenarbeiten.
ms.prod: xamarin
ms.assetid: 6C0226BE-A0C4-4108-B482-0A903696AB04
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: 352e8d0191512184ffd7d82fa0dfa0bc79fa24ca
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107260"
---
# <a name="architecture-overview"></a>Architekturübersicht

Xamarin Workbooks verfügt über zwei Hauptkomponenten, die miteinander zusammenarbeiten müssen: _Agent_ und _Client_.

## <a name="interactive-agent"></a>Interaktive Agent

Die Agent-Komponente ist eine kleine plattformspezifische-Assembly, die im Kontext einer Anwendung .NET ausgeführt wird.

Xamarin Workbooks enthält vorgefertigte "empty" Anwendungen für eine Reihe von Plattformen wie iOS, Android, Mac und WPF. Diese Anwendungen hosten explizit den Agent.

Während der Untersuchung (Xamarin Inspector) wird der Agent über den IDE-Debugger in eine vorhandene Anwendung als Teil der regulären Entwicklung & debuggingworkflow eingefügt.

## <a name="interactive-client"></a>Interaktiven Client

Der Client ist eine native Shell (Cocoa unter Mac, WPF für Windows), die eine Web-Browser-Oberfläche für die Darstellung der Arbeitsmappe/REPL-Oberfläche hostet. Aus Sicht der SDK werden alle Client-Integrationen in JavaScript und CSS-implementiert.

Der Client ist (über Roslyn) zuständig für das Kompilieren von Quellcode in kleinen Assemblys und diese über mit dem verbundenen Agent zur Ausführung gesendet. Ergebnisse der Ausführung werden für das Rendering an den Client gesendet. Jede Zelle in einer Arbeitsmappe führt zu einer Assembly, die auf die Assembly der vorherigen Zelle verweist.

Da ein Agent auf jede Art von .NET Plattform ausgeführt werden kann und Zugriff auf alle Elemente in der ausgeführten Anwendung hat, muss darauf geachtet werden Ergebnisse auf plattformunabhängige Weise serialisiert.
