---
title: Änderungen an der Architektur in iOS 11
description: Dieses Dokument beschreibt die Veraltung von 32-Bit-apps in iOS 11. Es wird erläutert, wie Anwendungen auf 64-Bit-Zielarchitekturen zu aktualisieren.
ms.prod: xamarin
ms.assetid: 55F62F3F-8570-402B-B7D9-2875F76CB946
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/13/2016
ms.openlocfilehash: b7d1df6ed2a8bd480025681ebcbe48acd7592564
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61169349"
---
# <a name="architecture-changes-in-ios-11"></a>Änderungen an der Architektur in iOS 11

Eine der wichtigsten Änderungen, die Sie mit iOS 11 bewusst sein sollten ist die Einstellung 32-Bit-Unterstützung für apps, wie im [Apple](https://developer.apple.com/news/?id=06282017b) Pressemitteilung. Alle neuen apps und Updates für vorhandene apps müssen 64-Bit-unterstützen. 32-Bit-Anwendungen **nicht gestartet** in iOS 11.

Um Ihre app in Visual Studio für Mac zu aktualisieren, verwenden Sie die folgenden Schritte aus:

1. Mit der rechten Maustaste auf den Projektnamen geöffnet **Projektoptionen**.
2. Navigieren Sie zu der **iOS-Build** Registerkarte.
3. Legen Sie die **unterstützte Architekturen** Dropdown-Liste für **x86_64** für die **Debuggen | iPhoneSimulator** und **Release | iPhoneSimulator**:

    ![Ändern Sie die simulatorarchitekturen Dropdown-Liste](architecture-changes-images/image1.png)

4. Ändern Sie die Konfiguration für iOS-Geräte **Debuggen | iPhone** und legen Sie die **unterstützte Architekturen** Dropdown-Liste für **ARM64**:

    ![Ändern der Gerätearchitekturen Dropdown-Liste](architecture-changes-images/image2.png)

5. Ändern Sie die Konfiguration in **Release | iPhone** und legen Sie die **unterstützte Architekturen** Dropdown-Liste für **ARM64**.

Weitere Informationen zu den 32-Bit und 64-Bit-Architekturen, finden Sie unter den [32/64 Bit Platform Considerations](~/cross-platform/macios/32-and-64/index.md#ios) Guide.

## <a name="related-links"></a>Verwandte Links

- [Was ist neu in iOS 11 (Apple)](https://developer.apple.com/ios/)
- [Aktualisierte App-Store-Produktseite (Apple)](https://developer.apple.com/app-store/product-page/)
- [Aktualisieren Ihrer App für iOS 11 (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/204/)
