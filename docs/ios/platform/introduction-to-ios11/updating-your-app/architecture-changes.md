---
title: Architekturänderungen in iOS 11
description: Dieses Dokument beschreibt die Veraltung von 32-Bit-apps in iOS-11. Es wird erläutert, wie Anwendungen, die Ziel-64-Bit-Architekturen aktualisieren.
ms.prod: xamarin
ms.assetid: 55F62F3F-8570-402B-B7D9-2875F76CB946
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 09d528ceb0654debd7c0ac8818f19c622775eac2
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787434"
---
# <a name="architecture-changes-in-ios-11"></a>Architekturänderungen in iOS 11

Eine der größten Änderungen, die Sie mit iOS 11 bewusst sein sollten ist veralten von 32-Bit-Unterstützung für apps, wie im [Apple](https://developer.apple.com/news/?id=06282017b) Pressemitteilung. Alle neuen apps und Updates auf vorhandene apps müssen 64-Bit-unterstützen. 32-Bit-apps **nicht gestartet** in iOS-11.

Um Ihre app in Visual Studio für Mac aktualisieren, verwenden Sie die folgenden Schritte aus:

1. Mit der rechten Maustaste auf den Projektnamen geöffnet **Projektoptionen**.
2. Navigieren Sie zu der **iOS-Build** Registerkarte.
3. Legen Sie die **unterstützt Architekturen** Dropdownliste zur **x86_64** für die **Debuggen | iPhoneSimulator** und **Version | iPhoneSimulator**:

    ![Simulator-Architekturen Dropdownelement ändern](architecture-changes-images/image1.png)

4. Für iOS-Geräte, ändern Sie die Konfiguration in **Debuggen | iPhone** und legen Sie die **unterstützt Architekturen** Dropdownelement **ARM64**:

    ![Gerätearchitekturen Dropdownelement ändern](architecture-changes-images/image2.png)

5. Ändern Sie die Konfiguration in **Version | iPhone** und legen Sie die **unterstützt Architekturen** Dropdownelement **ARM64**.

Weitere Informationen zu den 32-Bit und 64-Bit-Architekturen, finden Sie unter der [32/64-Bit-Plattform Überlegungen](~/cross-platform/macios/32-and-64/index.md#ios) Handbuch.

## <a name="related-links"></a>Verwandte Links

- [Was ist neu in iOS 11 (Apple)](https://developer.apple.com/ios/)
- [Aktualisierte App Store-Produktseite (Apple)](https://developer.apple.com/app-store/product-page/)
- [Aktualisieren Ihre App für iOS 11 (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/204/)
