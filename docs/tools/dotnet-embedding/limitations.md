---
title: .NET Einbetten von Einschränkungen
description: Dieses Dokument beschreibt die Einschränkungen von .NET einbetten, das Tool, das Sie .NET Code in anderen Programmiersprachen nutzen kann.
ms.prod: xamarin
ms.assetid: EBBBB886-1CEF-4DF4-AFDD-CA96049F878E
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: fdd3ac4cd57ac7f79f9071d62e758625b30f05dd
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34794123"
---
# <a name="net-embedding-limitations"></a>.NET Einbetten von Einschränkungen

Dieses Dokument wird erläutert, die Einschränkungen der Einbettung .NET und nach Möglichkeit werden mögliche problemumgehungen für sie bereitgestellt.

## <a name="general"></a>Allgemein

### <a name="use-more-than-one-embedded-library-in-a-project"></a>Verwenden von mehr als eine eingebettete Bibliothek in einem Projekt

Es ist nicht möglich, zwei Mono / Laufzeiten koexistieren innerhalb derselben Anwendung haben. Dies bedeutet, dass Sie zwei verschiedene .NET einbetten generierte Bibliotheken innerhalb derselben Anwendung verwenden können.

**Problemumgehung:** können Sie den Generator eine Bibliothek erstellen, die mehrere Assemblys (aus anderen Projekten) enthält.

### <a name="subclassing"></a>Unterklasse erstellen

Einbetten von .NET vereinfacht die Integration der Mono / Runtime innerhalb von Anwendungen, verfügbar machen, einen Satz von Ready to Use-APIs für die Zielsprache und Plattform.

Obwohl dies nicht über eine bidirektionale Integration ist, z. B. nicht die Unterklasse einen verwalteten Typ und erwarten, dass verwalteten Code wieder in Ihrem systemeigenen Code aufgerufen werden, da der verwaltete Code diese Co Existenz nicht bewusst ist.

Je nach Ihren Anforderungen möglicherweise Teile dieses Problem zu umgehen dieser Einschränkung können z. B. werden können

* verwaltete Code in Ihrem nativen Code können p/Invoke aufrufen. Dies erfordert zum ermöglichen der Anpassung von systemeigenem Code verwalteten Code anpassen.

* Verwenden Sie Produkte wie Xamarin.iOS, und machen Sie eine verwaltete Bibliothek, die Möglichkeit bietet, dass Objective-C (in diesem Fall) Unterklasse einige NSObject Unterklassen verwaltet.

## <a name="objective-c-generated-code"></a>Objective-C generierter code

### <a name="nullability"></a>NULL-Zulässigkeit

Es gibt keine Metadaten in .NET, die uns informieren, wenn ein null-Verweis akzeptabel ist oder nicht für eine API. Die meisten APIs lösen `ArgumentNullException` Wenn bewältigen kann kein `null` Argument. Dies ist problematisch, wie Objective-C-Behandlung von Ausnahmen, die etwas bessere vermieden wird.

Da wir nicht genau NULL-Zulässigkeit Anmerkungen in den Headerdateien zu generieren und verwaltete Ausnahmen minimieren möchten wir den Standardwert nicht-Null-Argumente (`NS_ASSUME_NONNULL_BEGIN`) und fügen Sie einige spezifisch ist, wenn die Genauigkeit möglich ist, die NULL-Zulässigkeit Anmerkungen hinzu.

### <a name="bitcode-ios"></a>Bitcode (iOS)

Einbetten von .NET unterstützt zurzeit nicht Bitcode iOS, die für einige Projektvorlagen Xcode aktiviert ist. Dies hat erfolgreich generiert Link Frameworks deaktiviert werden.

![Bitcode-Option](images/ios-bitcode-option.png)
