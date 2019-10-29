---
title: Aktualisieren Ihrer App auf iOS 11
description: Dieses Dokument enthält Links zu verschiedenen Leitfäden, in denen neue Features beschrieben werden, die für xamarin. IOS-Entwickler mit der Veröffentlichung von IOS 11 verfügbar sind. Zum Beispiel visuelle Design Updates, App Store-Änderungen und App-Symbol Updates.
ms.prod: xamarin
ms.assetid: EC809504-9CF6-4949-B6EE-36384297E744
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/13/2016
ms.openlocfilehash: 81d863b12aa7748acc6876929fb5d6361a20b507
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032115"
---
# <a name="updating-your-app-to-ios-11"></a>Aktualisieren Ihrer App auf iOS 11

In ios 11 hat Apple Architektur Updates, neue visuelle Änderungen und einen aktualisierten iTunes Connect-Prozess eingeführt. In dieser Anleitung werden diese Änderungen erläutert, sodass Sie Ihre xamarin. IOS-App für IOS 11 aktualisieren können.

## <a name="architecture-changesarchitecture-changesmd"></a>[Architekturänderungen](architecture-changes.md)

Eine der größten Änderungen, die Sie mit IOS 11 beachten sollten, ist die Veraltung der 32-Bit-Unterstützung für apps, wie in [der Apple-](https://developer.apple.com/news/?id=06282017b) Presse Version ausführlich beschrieben.

Dieser Leitfaden führt Sie durch die Aktualisierung Ihrer APP für 64-Bit.

## <a name="visual-design-updatesvisual-designmd"></a>[Visuelle Designupdates](visual-design.md)

In ios 11 hat Apple neue visuelle Änderungen eingeführt, einschließlich Aktualisierungen der Navigationsleiste, der Suchleiste und der Tabellen Ansichten. Außerdem wurden Verbesserungen vorgenommen, um mehr Flexibilität über Ränder und voll Bildinhalte zu ermöglichen. Diese Änderungen werden in diesem Handbuch behandelt.

## <a name="app-store-changesapp-store-changesmd"></a>[App Store-Änderungen](app-store-changes.md)

Der IOS App Store hat eine komplette Umgestaltung vorgenommen, die Benutzern nicht nur die effiziente Navigation im Store ermöglicht, sondern auch als Entwickler die herauf Stufung Ihrer APP zu den Benutzern ermöglicht. Diese Aktionen umfassen Updates für in-App-Käufe und Aktualisierungen der Produktseite. IOS 11 fügt außerdem Updates zum kommunizieren mit Benutzern, zum Hinzufügen des App-Symbols und zum Freigeben der APP auf der Öffentlichkeit hinzu.

## <a name="app-icon-updates"></a>App-Symbol Updates

> [!NOTE]
> App-Symbole sollten nun von einem _Asset-Katalog_geliefert werden. 

Weitere Informationen zur Verwendung von Asset-Katalogen finden Sie im [App Store-Symbol](~/ios/app-fundamentals/images-icons/app-store-icon.md) Handbuch. Hilfe zum Migrieren von Symbolen aus einer Info. plist zu einem Asset-Katalog finden Sie im Handbuch [Migrieren von Info. plist zu Asset-Katalogen](~/ios/app-fundamentals/images-icons/app-icons.md) .

Das erforderliche Symbol im Asset-Katalog heißt **App Store** und sollte 1024 x 1024 betragen. Laut Apple kann das App Store-Symbol im Ressourcenkatalog nicht transparent sein oder einen Alphakanal enthalten.

![App Store-Symbol Speicherort in Asset catalog.](images/image1.png)

## <a name="related-links"></a>Verwandte Links

- [Neuerungen in ios 11 (Apple)](https://developer.apple.com/ios/)
- [Aktualisierte App Store-Produktseite (Apple)](https://developer.apple.com/app-store/product-page/)
- [Aktualisieren Ihrer APP für IOS 11 (WWDC) (Video)](https://developer.apple.com/videos/play/wwdc2017/204/)
