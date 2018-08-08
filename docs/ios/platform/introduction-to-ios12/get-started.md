---
title: Erste Schritte mit iOS 12, 12-TvOS und WatchOS 5
description: In diesem Dokument wird beschrieben, wie abzurufenden Build iOS 12, 12-TvOS und WatchOS-5-apps mit Xamarin eingerichtet wird. Es wird erläutert, wie Xcode 10 herunterladen und Aktualisieren von Visual Studio für Mac und Visual Studio 2017.
ms.prod: xamarin
ms.assetid: 6C0F0133-1A5F-408B-8BCA-BDCA313A55C2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/07/2018
ms.openlocfilehash: cb84ddc646933d253ca72fe8f9f581364aab698b
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615173"
---
# <a name="getting-started-with-ios-12-tvos-12-and-watchos-5"></a>Erste Schritte mit iOS 12, 12-TvOS und WatchOS 5

![Vorschau](~/media/shared/preview.png)

> [!WARNING]
> Der Xamarin-Unterstützung für iOS-, 12, TvOS-12 und WatchOS-5-SDKs mit Xcode 10 verteilt befindet sich derzeit in der Vorschauversion befindet, was bedeutet, die dass die, dass sie Fehler, enthält möglicherweise nicht werden, abgeschlossen, und können sich ändern. Verwenden sie nur für Experimente.

Dieses Dokument beschreibt, wie Sie festlegen, dass das Xamarin-apps zu erstellen, die mit Xcode 10 veröffentlichten APIs aufrufen. Es wird erläutert, wie Xcode 10 herunterladen und Aktualisieren von Visual Studio für Mac und Visual Studio 2017.

## <a name="download-and-install"></a>Herunterladen und installieren

1. **Installieren Sie die neueste Betaversion des Xcode-10** – registrierten Apple-Entwickler können herunterladen und installieren Sie die neueste Version von Xcode 10 aus dem [Apple Developer Portal](https://developer.apple.com/download/).

2. **Führen Sie die Xcode-10** – führen Sie Xcode 10 vor dem Aktualisieren und Ausführen von Visual Studio für Mac oder Visual Studio 2017, während sie einige installiert wird, Xamarin-tools erfordert.

3. **Aktualisieren von Visual Studio für Mac und Visual Studio 2017** – befolgen Sie die Anweisungen auf der [release Blog](https://releases.xamarin.com/preview-release-xcode-10-beta-5/) zum Installieren der Xamarin-Vorschauversion.

4. _(optional)_  **Installieren Sie die neueste Betaversion für iOS auf Ihrem iOS-Geräten** – für Apps, die die Xcode-10, registrierten Apple-Entwickler können APIs eingeführt, mit dem Test des Geräts [herunterladen](https://developer.apple.com/download) herunter, und installieren Sie die neueste Version Entwickler-Betaversionen auf ihren Geräten.

   > [!TIP]
   > Auch wenn Ihre app keine neuen APIs nicht verwendet, achten Sie darauf, dass Sie mit den neuesten Xcode-10-SDKs zu erstellen und testen, um sicherzustellen, dass alles wie erwartet funktioniert. Wenn eine app keine neuen APIs nicht aufruft, können Sie mit diesen neuen SDKs neu kompilieren und Testen auf Geräten, die noch nicht auf das neue Betriebssystem aktualisiert wurden.
   >
   > Bevor Sie von Apple, um Ihre Xamarin-apps zu testen. aktualisieren Ihre Geräte, auf das neueste Betriebssystem veröffentlicht werden, achten Sie darauf, dass Sie zu:
   >
   > - Lesen [Apple – Anmerkungen zu dieser](https://developer.apple.com/download/) für das Aktualisieren des Betriebssystems.
   > - Lesen die Xamarin-Vorschauversion [release Blogbeitrag](https://releases.xamarin.com/preview-release-xcode-10-beta-5/).

## <a name="related-links"></a>Verwandte Links

- [Xcode 10 herunterladen](https://developer.apple.com/download/)
- Xamarin-Vorschauversion [release Blogbeitrag](https://releases.xamarin.com/preview-release-xcode-10-beta-5/)
