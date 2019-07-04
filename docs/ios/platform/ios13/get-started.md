---
title: Erste Schritte mit iOS 13, iPadOS 13, 13-TvOS und WatchOS 6
description: Dieses Dokument beschreibt, wie Sie bis zu iOS-Build 13, iPadOS 13, TvOS 13 und WatchOS 6-apps mit Xamarin zu erhalten. Es wird erläutert, wie Xcode 11 herunterladen und Aktualisieren von Visual Studio für Mac.
ms.prod: xamarin
ms.assetid: 97414545-85D2-433C-9246-63B6763F488A
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 07/01/2019
ms.openlocfilehash: 554cddf5717401f912cab38c78a6af17659a0cf7
ms.sourcegitcommit: 8ecfa339d0f3e7687977bfe4fc96448942690183
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2019
ms.locfileid: "67558685"
---
# <a name="get-started-with-ios-13"></a>Erste Schritte mit iOS 13

![Feature der Vorschauversion](~/media/shared/preview.png)

In diesem Dokument wird beschrieben, wie mit dem Erstellen von Xamarin-apps, die mit Xcode 11, veröffentlichten APIs für iOS 13 aufgerufen wird. Mithilfe der Vorschau ist MacOS 10.14.4 (Mojave) oder höher.

## <a name="download-and-install"></a>Herunterladen und installieren

1. **Installieren Sie Xcode 11 Beta** – registrierten Apple-Entwickler können herunterladen und Installieren der neuesten Version von Xcode 11 Beta aus der [Apple Developer Portal](https://developer.apple.com/download/) oder **App Store**.

2. **Führen Sie die Xcode-11-Betaversion** – führen Sie Xcode 11 vor dem Aktualisieren und Ausführen von Visual Studio für Mac, während sie einige installiert wird, Xamarin-tools erfordert.

3. Wählen Sie in Visual Studio für Mac **Visual Studio > nach Updates suchen...** , wählen die **Xcode 11 Previews** channel, und installieren Sie die verfügbaren Updates.

4. Wählen Sie in Visual Studio für Mac **Visual Studio > Einstellungen > Projekte > SDK-Speicherorte > Apple** , und wählen Sie **Xcode-beta.app**.

5. (Optional) Wenn diese Vorschauversion mithilfe bewerten _Xcode 11 Beta 3_, müssen Sie aktivieren, verknüpfen. Mit der rechten Maustaste in des Projekts, navigieren Sie zu **Optionen > iOS-Build > linkerverhalten** und legen Sie den Wert der Linker Verhalten **Link nur Framework-SDKs**. Diese problemumgehung kann nicht in einer kommenden Vorschau erforderlich werden.

6. (Optional) **IOS 13 auf iOS-Geräten installieren** – für den Test des Geräts von apps, die APIs verwendet, die mit Xcode 11, registrierten Apple verwenden, Entwickler können [herunterladen](https://developer.apple.com/download) und das Betriebssystem auf ihren Geräten installieren. Da iOS in der Betaversion ist, achten Sie auf dem primären Gerät installieren.

   > [!TIP]
   > Auch wenn Ihre app keine neuen APIs nicht verwendet, achten Sie darauf, dass Sie mit den neuesten Xcode-11-SDKs zu erstellen und testen, um sicherzustellen, dass alles wie erwartet funktioniert. Wenn eine app keine neuen APIs nicht aufruft, können Sie mit diesen neuen SDKs neu kompilieren und Testen auf Geräten, die noch nicht auf das neue Betriebssystem aktualisiert wurden.
   >
   > Bevor Sie von Apple, um Ihre Xamarin-apps zu testen. aktualisieren Ihre Geräte, auf das neueste Betriebssystem veröffentlicht werden, achten Sie darauf, dass Sie zu:
   >
   > - Lesen [Apple – Anmerkungen zu dieser](https://developer.apple.com/download/) für das Aktualisieren des Betriebssystems.

## <a name="related-links"></a>Verwandte Links

- [Xcode herunterladen](https://developer.apple.com/download/)
- [Anmerkungen zur Version von Xamarin.iOS-Vorschau](/xamarin/ios/release-notes/12/12.99)
