---
title: Erstellen von Bindungen mit dem Ziel-Sharpie
description: Dieser Abschnitt enthält eine Einführung in das-Befehlszeilen Tool von Target Sharpie, xamarin, das verwendet wird, um den Prozess der Erstellung einer Bindung an eine Ziel-C-Bibliothek zu automatisieren.
ms.prod: xamarin
ms.assetid: 9C0A932C-7601-4357-B3F7-62ABAC835019
author: davidortinau
ms.author: daortin
ms.date: 10/11/2017
ms.openlocfilehash: 4d6ab6cf48c5c365a4d8d05ef108a4d3a5d16134
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016193"
---
# <a name="creating-bindings-with-objective-sharpie"></a>Erstellen von Bindungen mit dem Ziel-Sharpie

_Dieser Abschnitt enthält eine Einführung in das-Befehlszeilen Tool von Target Sharpie, xamarin, das verwendet wird, um den Prozess der Erstellung einer Bindung an eine Ziel-C-Bibliothek zu automatisieren._

- [Übersicht](#overview) & [Verlauf](#history)
- [Erste Schritte](get-started.md)
- [Tools und Befehle](tools.md)
- [Features](platform/index.md)
- [Beispiele](examples/index.md)
- [Exemplarische Vorgehensweise vervollständigen](~/ios/platform/binding-objective-c/walkthrough.md)
- [Releaseverlauf](releases.md)

## <a name="overview"></a>Übersicht

Ziel-Sharpie ist ein Befehlszeilen Tool, mit dem der erste Durchlauf einer Bindung Bootstrap wird.
Dies funktioniert durch das Überprüfen der Header Dateien einer nativen Bibliothek, um die öffentliche API der [Bindungs Definition](~/cross-platform/macios/binding/objective-c-libraries.md#The_API_definition_file) zuzuordnen (ein Prozess, der zuvor manuell abgeschlossen wurde).

Der Ziel-Sharpie verwendet clang-Analyse Header Dateien, sodass die Bindung so genau und gründlich wie möglich ist. Dies kann die Zeit und den Aufwand für die Erstellung einer Qualitäts Bindung erheblich verkürzen.

> [!IMPORTANT]
> Ziel-Sharpie ist ein Tool für erfahrene xamarin-Entwickler mit erweiterten Kenntnissen von Target-C (und der Erweiterung C). Bevor Sie versuchen, eine Ziel-C-Bibliothek zu binden, sollten Sie fundierte Kenntnisse darüber haben, wie Sie die native Bibliothek in der Befehlszeile erstellen (und ein gutes Verständnis der Funktionsweise der systemeigenen Bibliothek).

## <a name="history"></a>Versionsgeschichte

Wir haben den Ziel-sharkreis in den letzten drei Jahren intern in xamarin entwickelt und verwendet. Als Beweis für die Leistungsfähigkeit von Target Sharpie wurden APIs, die in xamarin. IOS und xamarin. Mac seit IOS 8, Mac OS X 10,10 und watchos 2,0 eingeführt wurden, mit dem Ziel-Sharpie vollständig Bootstrap. Xamarin basiert intern auf dem Ziel Sharpie, um seine eigenen Produkte zu entwickeln.

Bei Ziel-Sharpie handelt es sich jedoch um ein sehr erweitertes Tool, das erweiterte Kenntnisse von "target-c" und "c" erfordert, die Verwendung des clang-Compilers in der Befehlszeile und im Allgemeinen, wie systemeigene Bibliotheken gemeinsam eingesetzt werden. Aufgrund dieser hohen Obergrenze haben wir festgestellt, dass ein GUI-Assistent die falschen Erwartungen festlegt, und daher ist das Ziel "Sharpie" derzeit nur als Befehlszeilen Tool verfügbar.

## <a name="related-links"></a>Verwandte Links

- [Ziel-Sharpie-Download](https://aka.ms/objective-sharpie)
- [Exemplarische Vorgehensweise: Binden einer Ziel-C-Bibliothek](~/ios/platform/binding-objective-c/walkthrough.md)
- [Binden von Objective-C-Bibliotheken](~/cross-platform/macios/binding/objective-c-libraries.md)
- [Bindungs Details](~/cross-platform/macios/binding/overview.md)
- [Bindungs Typen-Referenzhandbuch](~/cross-platform/macios/binding/binding-types-reference.md)
- [Xamarin für Objective-C-Entwickler](~/ios/get-started/objective-c-developers/index.md)
