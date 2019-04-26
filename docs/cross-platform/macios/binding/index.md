---
title: Binden von Objective-C
description: Dieses Dokument enthält Links zu verschiedenen Anleitungen, die beschreiben, wie Sie erstellen C# -Bindungen mit Objective-C-Code ermöglicht es Entwicklern, die sofort einsetzbare Bibliotheken in Xamarin-Anwendungen zu nutzen.
ms.prod: xamarin
ms.assetid: DBBAA086-BB0F-8161-DF44-632F4F5DFE5D
author: asb3993
ms.author: amburns
ms.date: 01/25/2016
ms.openlocfilehash: 3f1e1ce324e849c0c939d936eb6ee1470cf24a3b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61266607"
---
# <a name="binding-objective-c"></a>Binden von Objective-C

Dieser Abschnitt enthält eine Reihe von Dokumenten Erstellen von Bindungen für Objective-C-Bibliotheken, damit sie von aufgerufen werden können C# Anwendungen, die mit Xamarin.iOS oder Xamarin.Mac erstellt wurden.

##  <a name="overviewcross-platformmaciosbindingoverviewmd"></a>[Übersicht](~/cross-platform/macios/binding/overview.md)

Dieses Dokument enthält einige der die Interna der wie eine Bindung durchgeführt wird. Es ist eine erweiterte Dokument mit technischen Informationen.

##  <a name="binding-objective-c-librariescross-platformmaciosbindingobjective-c-librariesmd"></a>[Binden von Objective-C-Bibliotheken](~/cross-platform/macios/binding/objective-c-libraries.md)

Dieses Dokument beschreibt den Prozess zum Erstellen C# Bindungen von Objective-C-APIs und wie die Ausdrücke in .NET verwendet, die Ausdrücke in Objective-C zugeordnet werden.
Wenn Sie nur die C-APIs binden, sollten Sie den Standardmechanismus für .NET dafür das P/Invoke-Framework verwenden.

##  <a name="binding-definition-reference-guidecross-platformmaciosbindingbinding-types-referencemd"></a>[Bindung Definition – Referenzhandbuch](~/cross-platform/macios/binding/binding-types-reference.md)

Dies ist die Referenz-Anleitung, die alle Attribute für Autoren von Bindungen, die den Generierungsprozess der Bindung Laufwerk beschreibt.


## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objektive Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Objektive Sharpie ist ein Befehlszeilentool können Sie das erste Element des eine Bindung zu starten. Es funktioniert, indem Sie die Analyse der Headerdateien zuordnen die öffentliche API in einer nativen Bibliothek die [Bindungsdefinition](~/cross-platform/macios/binding/objective-c-libraries.md) (ein Prozess, die auch manuell ausgeführt werden können).

## <a name="ios"></a>iOS

Die [iOS Bindung Seite](~/ios/platform/binding-objective-c/index.md) Links zurück in diese gemeinsamen Ressourcen für die Bindung zusätzlich zu den Beispielen weiter unten.

### <a name="walkthrough-binding-an-objective-c-libraryiosplatformbinding-objective-cwalkthroughmd"></a>[Exemplarische Vorgehensweise: Binden eine Objective-C-Bibliothek](~/ios/platform/binding-objective-c/walkthrough.md)

Dieser Artikel enthält eine schrittweise Anleitung zum Erstellen einer Bindung-Projekts mithilfe des open Source [InfColorPicker](https://github.com/InfinitApps/InfColorPicker) Objective-C-Projekt als Beispiel. Die InfColorPicker-Bibliothek bietet es sich um einen wieder verwendbaren View-Controller, mit denen den Benutzer eine Farbe basierend auf seiner HSB-Darstellung, sodass Farbauswahl Benutzerfreundlicher auswählen können. Objektive Sharpie dienen zur Unterstützung des Bindungsvorgangs.

### <a name="binding-sampleshttpsgithubcommonomonotouch-bindings"></a>[Beispiele für Bindungen](https://github.com/mono/monotouch-bindings)

Eine Auflistung von Drittanbieter-Bindungen, die sein können einen Verweis zurück, der beim Binden von Projekten neu erstellen verwendet wurde.

## <a name="mac"></a>Mac

In der Vergangenheit [Mac Bindung](~/mac/platform/binding.md) wurde ein sehr manueller Prozess. Es gibt derzeit ein [herunterladbare Vorschau](https://forums.xamarin.com/discussion/59760/xamarin-mac-binding-project-preview) Binden von Mac-Projekt Unterstützung für eine zukünftige Version von Visual Studio für Mac.



## <a name="related-links"></a>Verwandte Links

- [iOS binden](~/ios/platform/binding-objective-c/index.md)
- [Mac-Bindung](~/mac/platform/binding.md)
- [Xamarin University-Kurs: Erstellen eine Bibliothek für Objective-C-Bindungen](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University-Kurs: Erstellen Sie eine Bibliothek Objective-C-Bindungen mit objektive Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
