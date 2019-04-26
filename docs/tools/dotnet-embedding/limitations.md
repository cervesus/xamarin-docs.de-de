---
title: Einbetten von .NET-Einschränkungen
description: Dieses Dokument beschreibt die Einschränkungen für das Einbetten von .NET, das Tool, das Ihnen ermöglicht, die .NET Code in anderen Programmiersprachen nutzen.
ms.prod: xamarin
ms.assetid: EBBBB886-1CEF-4DF4-AFDD-CA96049F878E
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: 7a162d632c98b4e412fa1b7b0c0c40ac945ff09f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "60945783"
---
# <a name="net-embedding-limitations"></a>Einbetten von .NET-Einschränkungen

Dieses Dokument beschreibt die Einschränkungen für das Einbetten von .NET und nach Möglichkeit werden mögliche problemumgehungen für sie bereitgestellt.

## <a name="general"></a>Allgemein

### <a name="use-more-than-one-embedded-library-in-a-project"></a>Verwenden von mehr als eine eingebettete Bibliothek in einem Projekt

Es ist nicht möglich, dass zwei Mono Laufzeiten, die in der gleichen Anwendung koexistieren. Dies bedeutet, dass Sie nicht zwei verschiedene Einbetten von .NET generierter Bibliotheken in der gleichen Anwendung verwenden können.

**Problemumgehung:** Sie können den Generator verwenden, eine einzelne Bibliothek erstellen, die mehrere Assemblys (von verschiedenen Projekten) enthält.

### <a name="subclassing"></a>Erstellen von Unterklassen für

Einbetten von .NET erleichtert die Integration von die Mono-Laufzeit in Anwendungen durch die Bereitstellung einer Reihe von sofort zu verwendende APIs für die Ziel-Sprache und Plattform.

Obwohl dies nicht über eine bidirektionale Integration ist, z. B. nicht die Unterklasse einen verwalteten Typ und erwarten, dass verwalteten Code in nativen Code ab, Rückruf, da der verwaltete Code nicht über diese objektkoexistenz ist.

Je nach Ihren Anforderungen entspricht kann es möglich, dieses Problem zu umgehen, Teile dieser Einschränkung können z. B. sein

* der verwaltete Code in Ihrem nativen Code können p/invoke. Dies erfordert das Anpassen von Ihren verwalteten Code, um Anpassungen von nativem Code zu ermöglichen;

* Verwenden Sie Produkte wie Xamarin.iOS, und machen Sie eine verwaltete Bibliothek, die ermöglichen, dass die Objective-C (in diesem Fall) um eine Unterklasse einige Unterklassen von NSObject verwaltet.

## <a name="objective-c-generated-code"></a>Objective-C-generierten code

### <a name="nullability"></a>NULL-Zulässigkeit

Es gibt keine Metadaten, die in .NET, der an, ob ein null-Verweis akzeptabel ist oder nicht für eine API aus. Die meisten APIs lösen `ArgumentNullException` , wenn sie bewältigen können keinem `null` Argument. Dies ist problematisch, da Objective-C-Behandlung von Ausnahmen, die etwas besser vermieden wird.

Da wir genau NULL-Zulässigkeit Anmerkungen in den Headerdateien zu generieren und verwaltete Ausnahmen minimieren möchten kann nicht in Zukunft Standard werden für nicht-Null-Argumente (`NS_ASSUME_NONNULL_BEGIN`), und fügen Sie einige spezifisch ist, wenn Genauigkeit möglich ist, die NULL-Zulässigkeit Anmerkungen hinzu.

### <a name="bitcode-ios"></a>Bitcode (iOS)

Einbetten von .NET unterstützt derzeit keine Bitcode unter iOS, die für einige Vorlagen der Xcode-Projekt aktiviert ist. Dies müssen erfolgreich generiert Link Frameworks deaktiviert werden.

![Bitcode-Option](images/ios-bitcode-option.png)
