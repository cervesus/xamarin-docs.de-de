---
title: Beginnen Sie mit IOS 13, ipados 13, tvos 13 und watchos 6
description: In diesem Dokument wird beschrieben, wie Sie das Einrichten von IOS 13-, ipados 13-, tvos 13-und watchos 6-apps mit xamarin einrichten. Darin wird erläutert, wie Sie Xcode 11 herunterladen und Visual Studio für Mac aktualisieren.
ms.prod: xamarin
ms.assetid: 97414545-85D2-433C-9246-63B6763F488A
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 07/01/2019
ms.openlocfilehash: 1f190ef2f99e71c8cf21680f9902caccf3454450
ms.sourcegitcommit: 09bc69d7119a04684c9e804c5cb113b8b1bb7dfc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2019
ms.locfileid: "71206324"
---
# <a name="get-started-with-ios-13"></a>Einstieg in ios 13

In diesem Dokument wird beschrieben, wie Sie mit dem Entwickeln von xamarin-apps beginnen, die mit Xcode 11 (IOS 13) veröffentlichte APIs aufzurufen. Die Verwendung der Vorschau erfordert macOS-10.14.4 (mujave) oder eine neuere Version.

## <a name="download-and-install"></a>Herunterladen und installieren

1. **Installieren von Xcode 11** – registrierte Apple-Entwickler können die neueste Version von Xcode 11 aus dem Apple- [Entwickler Portal](https://developer.apple.com/download/) oder dem **App Store**herunterladen und installieren.

2. **Ausführen von Xcode 11** – Ausführen von Xcode 11 vor dem Aktualisieren und Ausführen von Visual Studio für Mac, da einige von xamarin erforderliche Tools installiert werden.

3. Wählen Sie in Visual Studio für Mac **Visual Studio > nach Updates suchen... aus**, wählen Sie den **stabilen** Kanal aus, und installieren Sie die verfügbaren Updates.

4. Optionale **Installieren von IOS 13 auf Ihren IOS-Geräten** – für Gerätetests von apps, die mit Xcode 11 eingeführte APIs verwenden, können registrierte Apple-Entwickler das Betriebssystem auf Ihren Geräten [herunterladen](https://developer.apple.com/download) und installieren. 

   > [!TIP]
   > Auch wenn Ihre APP keine neuen APIs verwendet, stellen Sie sicher, dass Sie mit den neuesten Xcode 11-sdys erstellt werden, und testen Sie Sie, um sicherzustellen, dass alles wie erwartet funktioniert. Wenn eine APP keine neuen APIs aufruft, können Sie Sie mit diesen neuen sdchen neu kompilieren und auf Geräten testen, für die noch kein Upgrade auf das neue Betriebssystem durchgeführt wurde.
   >
   > Bevor Sie Ihre Geräte auf die neuesten Betriebssystemversionen von Apple aktualisieren, um Ihre xamarin-apps zu testen, stellen Sie Folgendes sicher:
   >
   > - Lesen Sie die Anmerkungen zu dieser [Version](https://developer.apple.com/download/) für die Betriebssystemupdates.

## <a name="related-links"></a>Verwandte Links

- [XCode herunterladen](https://developer.apple.com/download/)
- [Xamarin.iOS-Versionshinweise](/xamarin/ios/release-notes/13/13.0)
