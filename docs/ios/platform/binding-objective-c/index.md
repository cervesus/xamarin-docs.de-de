---
title: Binden von iOS-Bibliotheken
description: "Gewusst wie systemeigene Bibliotheken für iOS (und CocoaPods), in die Xamarin-apps zugegriffen werden kann."
ms.topic: article
ms.prod: xamarin
ms.assetid: DBBAA086-BB0F-8161-DF44-632F4F5DFE5D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 3afe1a03299e600502d49b1db039af4c6642e131
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="binding-ios-libraries"></a>Binden von iOS-Bibliotheken

_Gewusst wie systemeigene Bibliotheken für iOS (und CocoaPods), in die Xamarin-apps zugegriffen werden kann._

Führen Sie diese Links erfahren Sie mehr über das Binden von Objective-C-Bibliotheken und CocoaPods für Xamarin.iOS und Xamarin.Mac aus:

- [**Übersicht über** ](~/cross-platform/macios/binding/overview.md) -
  beschreibt die Funktionsweise der Bindung.
- [**Binden von Objective-C-Bibliotheken** ](~/cross-platform/macios/binding/objective-c-libraries.md) -
  Anweisungen wie Objective-C-Bibliotheken für die Verwendung in Projekten Xamarin gebunden.
- [**Geben Sie die Definition Referenzhandbuch** ](~/cross-platform/macios/binding/binding-types-reference.md) -
  beschreibt alle Attribute für Autoren von Bindungen in der Bindung Generierungsprozess Laufwerk verfügbar.

## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objektive Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Objektive Sharpie ist ein Befehlszeilentool helfen beim Bootstrapping für des ersten Durchlaufs einer Bindung.
Es funktioniert, indem die Analyse der Headerdateien einer systemeigenen Bibliothek zuordnen die öffentliche API in der [Bindungsdefinition](~/cross-platform/macios/binding/objective-c-libraries.md) (ein Prozess, der andernfalls manuell durchgeführt wird). Objektive Sharpie Erstellen einer Bindung nicht allein, aber es kann Ihnen beim Einstieg helfen.

Objektive Sharpie 3.0 eingeführt, die Fähigkeit zur Cocoapods direkte Bindung!

## <a name="walkthrough---binding-an-ios-objective-c-librarywalkthroughmd"></a>[Exemplarische Vorgehensweise – binden ein iOS Objective-C-Bibliothek](walkthrough.md)

Diese Seite enthält eine schrittweise Anleitung zum Erstellen einer iOS-Bindung-Projekts mithilfe der open Source [ **InfColorPicker** ](https://github.com/InfinitApps/InfColorPicker) Objective-C-Projekt als Beispiel. Die **InfColorPicker** Bibliothek stellt eine wiederverwendbare modellansichtcontroller, mit denen den Benutzer eine Farbe anhand der HSB-Darstellung Farbauswahl Benutzerfreundlicher machen auswählen können.
Objektive Sharpie wird verwendet um des Bindungsvorgangs zu unterstützen.



## <a name="related-links"></a>Verwandte Links

- [Binden von Objective-C](~/cross-platform/macios/binding/index.md)
- [Mac-Bindung](~/mac/platform/binding.md)
