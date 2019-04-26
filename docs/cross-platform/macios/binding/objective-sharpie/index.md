---
title: Erstellen von Bindungen mit objektive Sharpie
description: Dieser Abschnitt enthält eine Einführung in die Ziel-Sharpie, Xamarin Befehlszeilentool verwendet, um den Prozess zum Erstellen einer Bindung an ein Objective-C-Bibliothek automatisieren
ms.prod: xamarin
ms.assetid: 9C0A932C-7601-4357-B3F7-62ABAC835019
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: 53fcbbc408ae147405a3285d9391457051d6e16e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61261195"
---
# <a name="creating-bindings-with-objective-sharpie"></a>Erstellen von Bindungen mit objektive Sharpie

_Dieser Abschnitt enthält eine Einführung in die Ziel-Sharpie, Xamarin Befehlszeilentool verwendet, um den Prozess zum Erstellen einer Bindung an ein Objective-C-Bibliothek automatisieren_

- [Übersicht über die](#overview) & [Verlauf](#history)
- [Erste Schritte](get-started.md)
- [Tools und Befehle](tools.md)
- [Features](platform/index.md)
- [Beispiele](examples/index.md)
- [Vollständige Exemplarische Vorgehensweise](~/ios/platform/binding-objective-c/walkthrough.md)
- [Releaseverlauf](releases.md)

## <a name="overview"></a>Übersicht

Objektive Sharpie ist ein Befehlszeilentool können Sie das erste Element des eine Bindung zu starten.
Es funktioniert, indem Sie die Analyse der Headerdateien zuordnen die öffentliche API in einer nativen Bibliothek die [Bindungsdefinition](~/cross-platform/macios/binding/objective-c-libraries.md#The_API_definition_file) (ein Prozess, die zuvor manuell durchgeführt wurde).

Objektive Sharpie verwendet Clang-Analyse-Headerdateien, damit die Bindung so genau und umfassend wie möglich ist. Dies kann erheblich reduzieren, Zeit und Mühe, die es dauert, um eine Bindung für die Qualität zu erzeugen.

> [!IMPORTANT]
> Objektive Sharpie ist ein Tool für erfahrene Xamarin-Entwickler mit Kenntnissen von Objective-C (und durch Erweiterung, C). Bevor Sie versuchen, binden Sie eine Objective-C-Bibliothek sollten Sie wissen, wie Sie die native Bibliothek erstellen, auf der Befehlszeile aus (und ein gutes Verständnis der Funktionsweise der nativen Bibliothek) verfügen.

## <a name="history"></a>Versionsgeschichte

Wir haben wurden weiterentwickelt und die Ziel-Sharpie intern bei Xamarin für die letzten drei Jahre. Als Beweis der Leistungsfähigkeit von Ziel Sharpie APIs in Xamarin.iOS- und Xamarin.Mac eingeführt wurden, da iOS 8, Mac OS X 10.10 unterstützt, und WatchOS 2.0 wurden komplett mit Ziel Sharpie Bootstrapping durchgeführt. Xamarin basiert stark auf Ziel Sharpie intern für die Erstellung eigener Produkte.

Ziel-Sharpie ist jedoch ein sehr fortschrittlichen Tool, das erfordert erweiterte Kenntnisse der Objective-C und C, wie Sie mit dem Clang-Compiler in der Befehlszeile und in der Regel wie systemeigene Bibliotheken zusammengestellt werden. Aufgrund dieser selten, wir sind der Ansicht, dass durch eine grafische Benutzeroberfläche Assistent legt fest, die falschen Erwartungen und daher Ziel Sharpie ist derzeit nur als ein Befehlszeilentool zur Verfügung.

## <a name="related-links"></a>Verwandte Links

- [Objektive Sharpie-download](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie.pkg)
- [Exemplarische Vorgehensweise: Binden eine Objective-C-Bibliothek](~/ios/platform/binding-objective-c/walkthrough.md)
- [Binden von Objective-C-Bibliotheken](~/cross-platform/macios/binding/objective-c-libraries.md)
- [Informationen zu](~/cross-platform/macios/binding/overview.md)
- [Bindung Typen – Referenzhandbuch](~/cross-platform/macios/binding/binding-types-reference.md)
- [Xamarin für Objective-C-Entwickler](~/ios/get-started/objective-c-developers/index.md)
- [Xamarin University-Kurs: Erstellen eine Bibliothek für Objective-C-Bindungen](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University-Kurs: Erstellen Sie eine Bibliothek Objective-C-Bindungen mit objektive Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
