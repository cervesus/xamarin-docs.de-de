---
title: Binden von IOS-Bibliotheken
description: In diesem Dokument wird beschrieben, wie c#-Bindungen für den Ziel-C-Code erstellt werden, sodass Native Bibliotheken und cocoapods in einer xamarin. IOS-Anwendung verwendet werden können.
ms.prod: xamarin
ms.assetid: EBDC50DC-B44B-4003-AB2B-1EEB868A5E01
ms.technology: xamarin-ios
ms.custom: xamu-video
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 1e0342a41587b7479381ad763790227aba2ef414
ms.sourcegitcommit: a3f13a216fab4fc20a9adf343895b9d6a54634a5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85853084"
---
# <a name="binding-ios-libraries"></a>Binden von IOS-Bibliotheken

> [!IMPORTANT]
> Wir untersuchen derzeit die benutzerdefinierte Bindungs Verwendung auf der xamarin-Plattform. Nehmen Sie an [**dieser Umfrage**](https://www.surveymonkey.com/r/KKBHNLT) Teil, um zukünftige Entwicklungsbemühungen zu informieren.

Befolgen Sie diese Links, um mehr über das Binden von Ziel-C-Bibliotheken und cocoapods für xamarin. IOS und xamarin. Mac zu erfahren:

- [**Übersicht**](~/cross-platform/macios/binding/overview.md) -
   Beschreibt, wie die Bindung funktioniert.
- [**Binden von Ziel-C-Bibliotheken**](~/cross-platform/macios/binding/objective-c-libraries.md) -
   Anweisungen zum Binden von Ziel-C-Bibliotheken für die Verwendung in xamarin-Projekten.
- [**Referenzhandbuch**](~/cross-platform/macios/binding/binding-types-reference.md) -
   zur Typdefinition Beschreibt alle verfügbaren Attribute für das Binden von Autoren, um den Bindungs Generierungsprozess zu steuern.

## <a name="objective-sharpie"></a>[Objektive Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Ziel-Sharpie ist ein Befehlszeilen Tool, mit dem der erste Durchlauf einer Bindung Bootstrap wird.
Dies funktioniert durch das Überprüfen der Header Dateien einer nativen Bibliothek, um die öffentliche API der [Bindungs Definition](~/cross-platform/macios/binding/objective-c-libraries.md) zuzuordnen (ein Prozess, der andernfalls manuell ausgeführt wird). Der Ziel-Sharpie erstellt nicht allein eine Bindung, aber er kann Ihnen beim Einstieg helfen!

Mit dem Ziel-Sharpie 3,0 wurde die Möglichkeit eingeführt, cocoapods direkt zu binden.

## <a name="walkthrough---binding-an-ios-objective-c-library"></a>[Exemplarische Vorgehensweise: Binden einer IOS-Ziel-C-Bibliothek](walkthrough.md)

Auf dieser Seite finden Sie eine schrittweise exemplarische Vorgehensweise zum Erstellen eines IOS-Bindungs Projekts mithilfe des Open Source-Projekts " [**infcolorpicker**](https://github.com/InfinitApps/InfColorPicker) " als Beispiel. Die **infcolorpicker** -Bibliothek bietet einen wiederverwendbaren Ansichts Controller, mit dem der Benutzer eine Farbe basierend auf der HSB-Darstellung auswählen kann, sodass die Farbauswahl benutzerfreundlicher wird.
Ziel-Sharpie wird verwendet, um den Bindungsprozess zu unterstützen.

## <a name="video"></a>Video

> [!VIDEO https://youtube.com/embed/ZUoPLcmnf1o]

**IOS-Bindungen in C/C++-Video**

## <a name="related-links"></a>Ähnliche Themen

- [Binden von Objective-C](~/cross-platform/macios/binding/index.md)
- [Mac-Bindung](~/mac/platform/binding.md)
