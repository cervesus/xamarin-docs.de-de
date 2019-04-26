---
title: Binden von iOS-Bibliotheken
description: Dieses Dokument enthält Informationen zum Erstellen C# -Bindungen mit Objective-C-Code, wodurch sie systemeigene Bibliotheken und CocoaPods in einer Xamarin.iOS-Anwendung zu nutzen.
ms.prod: xamarin
ms.assetid: EBDC50DC-B44B-4003-AB2B-1EEB868A5E01
ms.technology: xamarin-ios
ms.custom: xamu-video
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: e599d4f99877e24e06de2c26ed2cafe48526f6f5
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61346930"
---
# <a name="binding-ios-libraries"></a>Binden von iOS-Bibliotheken

Führen Sie diesen Links, um zu erfahren Sie mehr über das Binden von Objective-C-Bibliotheken und CocoaPods für Xamarin.iOS- und Xamarin.Mac aus:

- [**Übersicht über die** ](~/cross-platform/macios/binding/overview.md) -
  beschreibt, wie die Bindung funktioniert.
- [**Binden von Objective-C-Bibliotheken** ](~/cross-platform/macios/binding/objective-c-libraries.md) -
  Anleitungen zum Binden von Objective-C-Bibliotheken für die Verwendung in Xamarin-Projekte.
- [**Geben Sie die Definition Referenzhandbuch** ](~/cross-platform/macios/binding/binding-types-reference.md) -
  beschreibt alle Attribute für Autoren von Bindungen, die den Generierungsprozess der Bindung Laufwerk verfügbar.

## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objektive Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Objektive Sharpie ist ein Befehlszeilentool können Sie das erste Element des eine Bindung zu starten.
Es funktioniert, indem Sie die Analyse der Headerdateien zuordnen die öffentliche API in einer nativen Bibliothek die [Bindungsdefinition](~/cross-platform/macios/binding/objective-c-libraries.md) (ein Prozess, die andernfalls manuell durchgeführt werden). Objektive Sharpie Erstellen einer Bindung nicht allein, aber sie können Ihnen beim Einstieg helfen!

Objektive Sharpie 3.0 wurde die Möglichkeit, direkte Binden von Cocoapods eingeführt.

## <a name="walkthrough---binding-an-ios-objective-c-librarywalkthroughmd"></a>[Exemplarische Vorgehensweise: Binden einer iOS Objective-C-Bibliothek](walkthrough.md)

Diese Seite enthält eine schrittweise Anleitung zum Erstellen einer iOS-Bindung-Projekts mithilfe des open Source [ **InfColorPicker** ](https://github.com/InfinitApps/InfColorPicker) Objective-C-Projekt als Beispiel. Die **InfColorPicker** -Bibliothek bietet einen wieder verwendbaren View-Controller, mit denen den Benutzer eine Farbe basierend auf seiner HSB-Darstellung, sodass Farbauswahl Benutzerfreundlicher auswählen können.
Objektive Sharpie dienen zur Unterstützung des Bindungsvorgangs.

## <a name="xamarin-university-lightning-lecture"></a>Xamarin University Lightning Lecture

> [!VIDEO https://youtube.com/embed/ZUoPLcmnf1o]

**iOS-Bindungen in C/C++ durch [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>Verwandte Links

- [Binden von Objective-C](~/cross-platform/macios/binding/index.md)
- [Mac-Bindung](~/mac/platform/binding.md)
- [Xamarin University-Kurs: Erstellen eine Bibliothek für Objective-C-Bindungen](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University-Kurs: Erstellen Sie eine Bibliothek Objective-C-Bindungen mit objektive Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)