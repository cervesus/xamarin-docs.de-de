---
title: Architektur Änderungen in ios 11
description: In diesem Dokument wird beschrieben, wie Sie 32-Bit-apps in ios 11 als veraltet kennzeichnen. Es wird erläutert, wie Anwendungen auf 64-Bit-Ziel Architekturen aktualisiert werden.
ms.prod: xamarin
ms.assetid: 55F62F3F-8570-402B-B7D9-2875F76CB946
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/13/2016
ms.openlocfilehash: 421e228a0021b4e4cdaf5da4c5776f9477477c28
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032093"
---
# <a name="architecture-changes-in-ios-11"></a>Architektur Änderungen in ios 11

Eine der größten Änderungen, die Sie mit IOS 11 beachten sollten, ist die Veraltung der 32-Bit-Unterstützung für apps, wie in [der Apple-](https://developer.apple.com/news/?id=06282017b) Presse Version ausführlich beschrieben. Alle neuen apps und Updates vorhandener apps müssen 64 Bit unterstützen. 32-Bit-Apps können nicht in ios 11 **gestartet werden** .

Führen Sie die folgenden Schritte aus, um Ihre APP in Visual Studio für Mac zu aktualisieren:

1. Klicken Sie mit der rechten Maustaste auf den Projektnamen, um die **Projektoptionen**zu öffnen.
2. Navigieren Sie zur Registerkarte **IOS-Build** .
3. Legen Sie die Dropdown Liste **unterstützte Architekturen** auf **x86_64** für das **Debuggen | iphonesimulator** und **Release | iphonesimulator**:

    ![Dropdown zum Ändern der simulatorarchitekturen](architecture-changes-images/image1.png)

4. Ändern Sie für IOS-Geräte die Konfiguration in **Debuggen | iPhone** , und legen Sie die Dropdown-Datei **unterstützte Architekturen** auf **ARM64**fest:

    ![Dropdown "Geräte Architekturen ändern"](architecture-changes-images/image2.png)

5. Ändern Sie die Konfiguration in **Release | iPhone** , und legen Sie die Dropdown-Datei für **unterstützte Architekturen** auf **ARM64**fest.

Weitere Informationen zu 32-Bit-und 64-Bit-Architekturen finden Sie im Handbuch zu den [Überlegungen zur 32/64-Bit-Plattform](~/cross-platform/macios/32-and-64/index.md#ios) .

## <a name="related-links"></a>Verwandte Links

- [Neuerungen in ios 11 (Apple)](https://developer.apple.com/ios/)
- [Aktualisierte App Store-Produktseite (Apple)](https://developer.apple.com/app-store/product-page/)
- [Aktualisieren Ihrer APP für IOS 11 (WWDC) (Video)](https://developer.apple.com/videos/play/wwdc2017/204/)
