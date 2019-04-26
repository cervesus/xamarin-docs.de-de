---
title: Aktualisieren Ihrer App auf iOS 11
description: Dieses Dokument enthält Links zu verschiedenen Leitfäden, die neuen Features, die für Xamarin.iOS-Entwickler, mit der Veröffentlichung von iOS 11 beschrieben. Klicken Sie z. B. visuelle designupdates, App-Store Änderungen, und App-Symbol aktualisiert.
ms.prod: xamarin
ms.assetid: EC809504-9CF6-4949-B6EE-36384297E744
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/13/2016
ms.openlocfilehash: 3193f27c30080a4335abfe969acb3c8b33516469
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61386343"
---
# <a name="updating-your-app-to-ios-11"></a>Aktualisieren Ihrer App auf iOS 11

In iOS 11 hat Apple Architektur Updates, neuen visuellen Änderungen und eine aktualisierte iTunes Connect-Prozess eingeführt. Dieses Handbuch beschreibt jede dieser Änderungen, so können Sie zum Abrufen Ihrer Xamarin.iOS-app für iOS 11 aktualisiert.

## <a name="architecture-changesarchitecture-changesmd"></a>[Architekturänderungen](architecture-changes.md)

Eine der wichtigsten Änderungen, die Sie mit iOS 11 bewusst sein sollten ist die Einstellung 32-Bit-Unterstützung für apps, wie im [Apple](https://developer.apple.com/news/?id=06282017b) Pressemitteilung.

Dieses Handbuch führt Sie durch Aktualisieren Ihrer app für 64-Bit-Version.

## <a name="visual-design-updatesvisual-designmd"></a>[Visuelle Designupdates](visual-design.md)

IOS 11 hat Apple neue visuelle Änderungen, einschließlich Updates für die Navigationsleiste, eine Suchleiste und eine Tabellenansichten eingeführt. Darüber hinaus wurden Verbesserungen vorgenommen, um für mehr Flexibilität über Ränder und Vollbild-Inhalte zu ermöglichen. Diese Änderungen werden in diesem Handbuch behandelt.

## <a name="app-store-changesapp-store-changesmd"></a>[App Store-Änderungen](app-store-changes.md)

Der iOS App Store hatte völlig neu entworfen, die nicht nur Benutzer den Store effizient zu navigieren können, jedoch Sie auch als Entwickler, um die app Benutzern höher zu stufen können. Diese heraufstufungen umfassen Updates für in-app-Käufe und Updates für die Seite "Product". iOS 11 bietet außerdem die laufenden mit Benutzern kommunizieren, wie Sie Ihr app-Symbol hinzufügen und wie Sie Ihre app öffentlich freigeben.

## <a name="app-icon-updates"></a>Symbol der App-Updates

> [!NOTE]
> App-Symbole sollten nun übermittelt werden, durch eine _Asset-Katalog_. 

Informationen zur Verwendung von Asset-Katalogs finden Sie in der [App Store-Symbol](~/ios/app-fundamentals/images-icons/app-store-icon.md) Guide. Um Hilfe bei der Migration Symbole aus einer Datei "Info.plist" aus, um einen Ressourcenkatalog, finden Sie unter den [Migrieren von Datei "Info.plist" zu Ressourcenkataloge](~/ios/app-fundamentals/images-icons/app-icons.md) Guide.

Das erforderliche Symbole im Ressourcenkatalog heißt **App Store** und 1024 x 1024 Größe. Laut Apple kann das App Store-Symbol im Ressourcenkatalog nicht transparent sein oder einen Alphakanal enthalten.

![App speichern Symbols in den Asset-Katalog.](images/image1.png)

## <a name="related-links"></a>Verwandte Links

- [Was ist neu in iOS 11 (Apple)](https://developer.apple.com/ios/)
- [Aktualisierte App-Store-Produktseite (Apple)](https://developer.apple.com/app-store/product-page/)
- [Aktualisieren Ihrer App für iOS 11 (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/204/)
