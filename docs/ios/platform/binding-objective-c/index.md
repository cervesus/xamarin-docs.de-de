---
title: Binden von IOS-Bibliotheken
description: In diesem Dokument wird beschrieben, C# wie Sie Bindungen mit dem Ziel-C-Code erstellen, sodass Native Bibliotheken und cocoapods in einer xamarin. IOS-Anwendung verwendet werden können.
ms.prod: xamarin
ms.assetid: EBDC50DC-B44B-4003-AB2B-1EEB868A5E01
ms.technology: xamarin-ios
ms.custom: xamu-video
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 61d1adfc997b34302f1f89f56653af906ca90135
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022221"
---
# <a name="binding-ios-libraries"></a>Binden von IOS-Bibliotheken

Befolgen Sie diese Links, um mehr über das Binden von Ziel-C-Bibliotheken und cocoapods für xamarin. IOS und xamarin. Mac zu erfahren:

- In der [**Übersicht**](~/cross-platform/macios/binding/overview.md) -
  wird beschrieben, wie die Bindung funktioniert.
- Binden von [**Ziel-c-Bibliotheken**](~/cross-platform/macios/binding/objective-c-libraries.md) -
  Anweisungen zum Binden von Ziel-c-Bibliotheken zur Verwendung in xamarin-Projekten.
- [**Typdefinition-Referenzhandbuch**](~/cross-platform/macios/binding/binding-types-reference.md) -
  beschreibt alle Attribute, die für die Bindung von Autoren zum Steuern des Bindungs Generierungs Prozesses verfügbar sind.

## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objektive Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Ziel-Sharpie ist ein Befehlszeilen Tool, mit dem der erste Durchlauf einer Bindung Bootstrap wird.
Dies funktioniert durch das Überprüfen der Header Dateien einer nativen Bibliothek, um die öffentliche API der [Bindungs Definition](~/cross-platform/macios/binding/objective-c-libraries.md) zuzuordnen (ein Prozess, der andernfalls manuell ausgeführt wird). Der Ziel-Sharpie erstellt nicht allein eine Bindung, aber er kann Ihnen beim Einstieg helfen!

Mit dem Ziel-Sharpie 3,0 wurde die Möglichkeit eingeführt, cocoapods direkt zu binden.

## <a name="walkthrough---binding-an-ios-objective-c-librarywalkthroughmd"></a>[Exemplarische Vorgehensweise: Binden einer IOS-Ziel-C-Bibliothek](walkthrough.md)

Auf dieser Seite finden Sie eine schrittweise exemplarische Vorgehensweise zum Erstellen eines IOS-Bindungs Projekts mithilfe des Open Source-Projekts " [**infcolorpicker**](https://github.com/InfinitApps/InfColorPicker) " als Beispiel. Die **infcolorpicker** -Bibliothek bietet einen wiederverwendbaren Ansichts Controller, mit dem der Benutzer eine Farbe basierend auf der HSB-Darstellung auswählen kann, sodass die Farbauswahl benutzerfreundlicher wird.
Ziel-Sharpie wird verwendet, um den Bindungsprozess zu unterstützen.

## <a name="video"></a>Video

> [!VIDEO https://youtube.com/embed/ZUoPLcmnf1o]

**IOS-Bindungen in CC++ /Video**

## <a name="related-links"></a>Verwandte Links

- [Binden von Objective-C](~/cross-platform/macios/binding/index.md)
- [Mac-Bindung](~/mac/platform/binding.md)
