---
title: .NET Einbetten von Einschränkungen
ms.prod: xamarin
ms.assetid: EBBBB886-1CEF-4DF4-AFDD-CA96049F878E
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 80d2402329020002a43e1f9dd7b518e2ce74747c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="net-embedding-limitations"></a>.NET Einbetten von Einschränkungen


Dieses Dokument wird erläutert, die Einschränkungen der .NET Einbettung (Embeddinator 4000) und nach Möglichkeit werden mögliche problemumgehungen für sie bereitgestellt.

## <a name="general"></a>Allgemein

### <a name="use-more-than-one-embedded-library-in-a-project"></a>Verwenden von mehr als eine eingebettete Bibliothek in einem Projekt

Es ist nicht möglich, zwei Mono / Laufzeiten koexistieren innerhalb derselben Anwendung haben. Dies bedeutet, dass Sie zwei unterschiedliche Embeddinator 4000 generierte Bibliotheken innerhalb derselben Anwendung verwenden können.

**Problemumgehung:** können Sie den Generator eine Bibliothek erstellen, die mehrere Assemblys (aus anderen Projekten) enthält.

### <a name="subclassing"></a>Unterklasse erstellen

Die Embeddinator vereinfacht die Integration der Mono / Runtime innerhalb von Anwendungen, verfügbar machen, einen Satz von Ready to Use-APIs für die Zielsprache und Plattform.

Obwohl dies keine zwei-Wege-Integration ist, z. B. nicht die Unterklasse einen verwalteten Typ und erwarten, dass verwalteten Code wieder in Ihrem systemeigenen Code aufgerufen werden, da der verwaltete Code diese Co Existenz nicht bewusst ist.

Je nach Ihren Anforderungen möglicherweise Teile dieses Problem zu umgehen dieser Einschränkung können z. B. werden können

* verwaltete Code in Ihrem nativen Code können p/Invoke aufrufen. Dies erfordert zum ermöglichen der Anpassung von systemeigenem Code verwalteten Code anpassen.

* Verwenden Sie Produkte wie Xamarin.iOS, und machen Sie eine verwaltete Bibliothek, die ObjC (in diesem Fall) einige verwalteten NSObject Unterklassen Unterklasse zulassen würde.


## <a name="objc-generated-code"></a>ObjC generierten code

### <a name="nullability"></a>NULL-Zulässigkeit

Keine Metadaten im .NET vorhanden ist, teilen, die uns, wenn ein null-Verweis akzeptabel ist oder nicht für eine API. Die meisten APIs lösen `ArgumentNullException` Wenn bewältigen kann kein `null` Argument. Dies ist problematisch, da ObjC Behandlung von Ausnahmen etwas bessere vermieden ist.

Da wir nicht genau NULL-Zulässigkeit Anmerkungen in den Headerdateien zu generieren und verwaltete Ausnahmen minimieren möchten wir den Standardwert nicht-Null-Argumente (`NS_ASSUME_NONNULL_BEGIN`) und fügen Sie einige spezifisch ist, wenn die Genauigkeit möglich ist, die NULL-Zulässigkeit Anmerkungen hinzu.
