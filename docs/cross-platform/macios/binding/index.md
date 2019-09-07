---
title: Binden von Objective-C
description: Dieses Dokument enthält Links zu verschiedenen Leitfäden, in denen beschrieben C# wird, wie Bindungen mit dem Ziel-C-Code erstellt werden, sodass Entwickler in xamarin-Anwendungen offline-Bibliotheken verwenden können.
ms.prod: xamarin
ms.assetid: DBBAA086-BB0F-8161-DF44-632F4F5DFE5D
author: conceptdev
ms.author: crdun
ms.date: 01/25/2016
ms.openlocfilehash: d48245ac6939a7b1a1528a7b42ec4a701f062a95
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70765760"
---
# <a name="binding-objective-c"></a>Binden von Objective-C

Dieser Abschnitt enthält eine Vielzahl von Dokumenten, die das Erstellen von Bindungen zu Ziel-C-Bibliotheken abdecken, sodass Sie C# von Anwendungen aufgerufen werden können, die mit xamarin. IOS oder xamarin. Mac erstellt wurden.

## <a name="overviewcross-platformmaciosbindingoverviewmd"></a>[Übersicht](~/cross-platform/macios/binding/overview.md)

Dieses Dokument enthält einige der internale für die Art und Weise, wie eine Bindung erfolgt. Es handelt sich um ein erweitertes Dokument mit einigen technischen Informationen.

## <a name="binding-objective-c-librariescross-platformmaciosbindingobjective-c-librariesmd"></a>[Binden von Objective-C-Bibliotheken](~/cross-platform/macios/binding/objective-c-libraries.md)

In diesem Dokument wird der Prozess zum Erstellen C# von Bindungen von Ziel-c-APIs und die Zuordnung der Ausdrücke in Ziel-c zu den in .NET verwendeten Ausdrücke beschrieben.
Wenn Sie nur C-APIs binden, sollten Sie hierfür den standardmäßigen .net-Mechanismus verwenden.

## <a name="binding-definition-reference-guidecross-platformmaciosbindingbinding-types-referencemd"></a>[Referenzhandbuch zur Bindungs Definition](~/cross-platform/macios/binding/binding-types-reference.md)

Dies ist die Referenzanleitung, in der alle Attribute beschrieben werden, die für die Bindung von Autoren zum Steuern der Bindungs Generierung verfügbar sind.

## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objektive Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Ziel-Sharpie ist ein Befehlszeilen Tool, mit dem der erste Durchlauf einer Bindung Bootstrap wird. Dies funktioniert durch das Überprüfen der Header Dateien einer nativen Bibliothek, um die öffentliche API der [Bindungs Definition](~/cross-platform/macios/binding/objective-c-libraries.md) zuzuordnen (ein Prozess, der auch manuell ausgeführt werden kann).

## <a name="ios"></a>iOS

Die [Seite IOS-Bindung](~/ios/platform/binding-objective-c/index.md) wird zusätzlich zu den folgenden Beispielen mit diesen gemeinsamen Bindungs Ressourcen verknüpft.

### <a name="walkthrough-binding-an-objective-c-libraryiosplatformbinding-objective-cwalkthroughmd"></a>[Exemplarische Vorgehensweise: Binden einer Ziel-C-Bibliothek](~/ios/platform/binding-objective-c/walkthrough.md)

Dieser Artikel enthält eine schrittweise exemplarische Vorgehensweise zum Erstellen eines Bindungs Projekts mithilfe des Open Source-Projekts " [infcolorpicker](https://github.com/InfinitApps/InfColorPicker) " als Beispiel. Die infcolorpicker-Bibliothek bietet einen wiederverwendbaren Ansichts Controller, mit dem der Benutzer eine Farbe basierend auf der HSB-Darstellung auswählen kann, sodass die Farbauswahl benutzerfreundlicher wird. Ziel-Sharpie wird verwendet, um den Bindungsprozess zu unterstützen.

### <a name="binding-sampleshttpsgithubcommonomonotouch-bindings"></a>[Bindungs Beispiele](https://github.com/mono/monotouch-bindings)

Eine Auflistung von Bindungen von Drittanbietern, die als Verweis verwendet werden kann, wenn neue Bindungs Projekte erstellt werden.

## <a name="mac"></a>Mac

In der Vergangenheit war die [Mac-Bindung](~/mac/platform/binding.md) ein sehr manueller Prozess. Zurzeit ist eine [herunterladbare Vorschau](https://forums.xamarin.com/discussion/59760/xamarin-mac-binding-project-preview) der Mac-Bindungs Projektunterstützung für eine zukünftige Version von Visual Studio für Mac verfügbar.

## <a name="related-links"></a>Verwandte Links

- [IOS-Bindung](~/ios/platform/binding-objective-c/index.md)
- [Mac-Bindung](~/mac/platform/binding.md)
