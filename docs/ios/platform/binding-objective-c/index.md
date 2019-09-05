---
title: Binden von IOS-Bibliotheken
description: In diesem Dokument wird beschrieben, C# wie Sie Bindungen mit dem Ziel-C-Code erstellen, sodass Native Bibliotheken und cocoapods in einer xamarin. IOS-Anwendung verwendet werden können.
ms.prod: xamarin
ms.assetid: EBDC50DC-B44B-4003-AB2B-1EEB868A5E01
ms.technology: xamarin-ios
ms.custom: xamu-video
author: conceptdev
ms.author: crdun
ms.date: 03/18/2017
ms.openlocfilehash: 24203d8a3a4356fb4de08d132c164d9f2d19a0c9
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291800"
---
# <a name="binding-ios-libraries"></a>Binden von IOS-Bibliotheken

Befolgen Sie diese Links, um mehr über das Binden von Ziel-C-Bibliotheken und cocoapods für xamarin. IOS und xamarin. Mac zu erfahren:

- [**Übersicht**](~/cross-platform/macios/binding/overview.md) -
  Beschreibt, wie die Bindung funktioniert.
- [**Binden von Ziel-C-Bibliotheken**](~/cross-platform/macios/binding/objective-c-libraries.md) -
  Anweisungen zum Binden von Ziel-C-Bibliotheken für die Verwendung in xamarin-Projekten.
- [**Referenzhandbuch zur Typdefinition**](~/cross-platform/macios/binding/binding-types-reference.md) -
  Beschreibt alle verfügbaren Attribute für das Binden von Autoren, um den Bindungs Generierungsprozess zu steuern.

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
