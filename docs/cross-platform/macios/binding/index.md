---
title: Objective-C-Bindung
ms.topic: article
ms.prod: xamarin
ms.assetid: DBBAA086-BB0F-8161-DF44-632F4F5DFE5D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 01/25/2016
ms.openlocfilehash: e836081d79d904e2d0952386e536eefdabe361e1
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="binding-objective-c"></a>Objective-C-Bindung

Dieser Abschnitt enthält eine Vielzahl von Dokumenten, die Erstellen von Bindungen für Objective-C-Bibliotheken abdecken, damit sie von C#-Anwendungen mit einem Xamarin.iOS oder Xamarin.Mac erstellt aufgerufen werden können.

##  <a name="overviewcross-platformmaciosbindingoverviewmd"></a>[Übersicht](~/cross-platform/macios/binding/overview.md)

Dieses Dokument enthält einige der im Rahmen der wie eine Bindung stattfindet. Es ist eine erweiterte Dokument mit einige technische Informationen benötigen.

##  <a name="binding-objective-c-librariescross-platformmaciosbindingobjective-c-librariesmd"></a>[Binden von Objective-C-Bibliotheken](~/cross-platform/macios/binding/objective-c-libraries.md)

Dieses Dokument beschreibt den Prozess zum Erstellen der C#-Bindungen Objective-C-APIs und wie die Ausdrücke in .NET verwendet die Idiome in Objective-C zugeordnet sind.
Wenn Sie C-APIs binden, sollten Sie den Standardmechanismus .NET dafür das P/Invoke-Framework verwenden.

##  <a name="binding-definition-reference-guidecross-platformmaciosbindingbinding-types-referencemd"></a>[Bindung Definition Referenzhandbuch](~/cross-platform/macios/binding/binding-types-reference.md)

Dies ist im Referenzhandbuch zu, das alle Attribute für Autoren von Bindungen in der Bindung Generierungsprozess Laufwerk verfügbar beschreibt.


## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objektive Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Objektive Sharpie ist ein Befehlszeilentool helfen beim Bootstrapping für des ersten Durchlaufs einer Bindung. Es funktioniert, indem die Analyse der Headerdateien einer systemeigenen Bibliothek zuordnen die öffentliche API in der [Bindungsdefinition](~/cross-platform/macios/binding/objective-c-libraries.md) (ein Prozess, der auch manuell durchgeführt werden kann).

## <a name="ios"></a>iOS

Die [iOS-Seite "Bindung"](~/ios/platform/binding-objective-c/index.md) Links zurück in diese gemeinsamen Ressourcen für die Bindung zusätzlich zu den Beispielen unten.

### <a name="walkthrough-binding-an-objective-c-libraryiosplatformbinding-objective-cwalkthroughmd"></a>[Exemplarische Vorgehensweise: Binden einer Objective-C-Bibliothek](~/ios/platform/binding-objective-c/walkthrough.md)

Dieser Artikel enthält eine schrittweise Anleitung zum Erstellen einer Bindung-Projekts mithilfe der open Source [InfColorPicker](https://github.com/InfinitApps/InfColorPicker) Objective-C-Projekt als Beispiel. Die InfColorPicker-Bibliothek stellt eine wiederverwendbare modellansichtcontroller, mit denen den Benutzer eine Farbe anhand der HSB-Darstellung Farbauswahl Benutzerfreundlicher machen auswählen können. Objektive Sharpie wird verwendet um des Bindungsvorgangs zu unterstützen.

### <a name="binding-sampleshttpsgithubcommonomonotouch-bindings"></a>[Bindung-Beispiele](https://github.com/mono/monotouch-bindings)

Eine Auflistung von Drittanbieter-Bindungen, die möglich verwendet einen Verweis für die Erstellung neuer Projekte binden.

## <a name="mac"></a>Mac

In der Vergangenheit [Mac Bindung](~/mac/platform/binding.md) wurde ein sehr manueller Prozess. Zurzeit eine [herunterladbare Preview](https://forums.xamarin.com/discussion/59760/xamarin-mac-binding-project-preview) des Mac binden projektunterstützung für eine zukünftige Version von Visual Studio für Mac.



## <a name="related-links"></a>Verwandte Links

- [iOS binden](~/ios/platform/binding-objective-c/index.md)
- [Mac-Bindung](~/mac/platform/binding.md)
