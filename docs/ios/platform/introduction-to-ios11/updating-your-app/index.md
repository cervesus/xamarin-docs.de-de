---
title: Aktualisieren Ihre App für iOS-11
description: Dieses enthält Dokumentenlinks zu verschiedenen Handbüchern, die neuen Features für Entwickler von Xamarin.iOS mit der Version 11 e/as zu beschreiben. Klicken Sie beispielsweise Updates der visuellen Entwurf, App Store ändert, und Symbol "App" aktualisiert.
ms.prod: xamarin
ms.assetid: EC809504-9CF6-4949-B6EE-36384297E744
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: ea57acbd6165f7b1abd8b9bd69873670c179f411
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787571"
---
# <a name="updating-your-app-to-ios-11"></a>Aktualisieren Ihre App für iOS-11

In iOS 11 wurde Apple Architektur Updates, neue visuelle Änderungen und eine aktualisierte iTunes Connect Prozess eingeführt. Dieses Handbuch untersucht jede dieser Änderungen hilft Ihnen, Ihre Xamarin.iOS app für iOS 11 aktualisiert.

## <a name="architecture-changesarchitecture-changesmd"></a>[Architekturänderungen](architecture-changes.md)

Eine der größten Änderungen, die Sie mit iOS 11 bewusst sein sollten ist veralten von 32-Bit-Unterstützung für apps, wie im [Apple](https://developer.apple.com/news/?id=06282017b) Pressemitteilung.

Dieses Handbuch führt Sie durch Ihre app für 64-Bit-Version aktualisieren.

## <a name="visual-design-updatesvisual-designmd"></a>[Visuelle Designupdates](visual-design.md)

Apple iOS 11 eingeführt neue visuelle Änderungen, einschließlich Updates auf der Navigationsleiste in der Suchleiste und Tabellensichten. Darüber hinaus wurden Verbesserungen vorgenommen, um mehr Flexibilität über Ränder und Vollbild-Inhalt zu ermöglichen. Diese Änderungen werden in diesem Handbuch behandelt.

## <a name="app-store-changesapp-store-changesmd"></a>[App Store-Änderungen](app-store-changes.md)

IOS App Store wurde völlig neu entworfen, die nicht nur Benutzer den Store effizient zu navigieren können, sondern auch, die Sie als Entwickler, um die app Benutzern höher zu stufen. Diese Erweiterungen umfassen mit app-Käufen und Updates auf der Seite "Product". iOS 11 fügt auch Updates hinsichtlich kommunizieren mit Benutzern, das Symbol für Ihre app hinzufügen und Freigeben von Ihrer app für die Öffentlichkeit.

## <a name="app-icon-updates"></a>Symbol "App" wird aktualisiert

> [!NOTE]
> App-Symbole sollte jetzt übermittelt werden, indem ein _Asset-Katalog_. 

Informationen zur Verwendung von Asset Kataloge finden Sie unter der [Symbol "App-Store"](~/ios/app-fundamentals/images-icons/app-store-icon.md) Handbuch. Hilfestellung bei der Migration aus einer Datei "Info.plist" Symbole, um eine Asset-Katalog finden Sie unter der [Migrieren von Datei "Info.plist" Asset-Katalog](~/ios/app-fundamentals/images-icons/app-icons.md) Handbuch.

Symbol "erforderliche" in den Asset-Katalog heißt **App Store** und 1024 x 1024 Größe sein. Laut Apple kann das App Store-Symbol im Ressourcenkatalog nicht transparent sein oder einen Alphakanal enthalten.

![App-Speicherort Symbol im Asset-Katalog.](images/image1.png)

## <a name="related-links"></a>Verwandte Links

- [Was ist neu in iOS 11 (Apple)](https://developer.apple.com/ios/)
- [Aktualisierte App Store-Produktseite (Apple)](https://developer.apple.com/app-store/product-page/)
- [Aktualisieren Ihre App für iOS 11 (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/204/)
