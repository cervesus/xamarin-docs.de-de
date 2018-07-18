---
title: Erste Schritte mit iOS 12, 12-TvOS und WatchOS 5
description: Dieses Dokument beschreibt, wie Sie bis zu iOS-Build 12 und 12-TvOS-apps mit Xamarin zu erhalten. Es wird erläutert, wie Xcode 10 herunterladen und Aktualisieren von Visual Studio für Mac und Visual Studio 2017.
ms.prod: xamarin
ms.assetid: 6C0F0133-1A5F-408B-8BCA-BDCA313A55C2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: 70f67f934c2503e6f6fa0d3bae1f37bcc1f6f0a4
ms.sourcegitcommit: cfb72be633e335147d156af3ef9527151b9e31d9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2018
ms.locfileid: "39030651"
---
# <a name="getting-started-with-ios-12-tvos-12-and-watchos-5"></a>Erste Schritte mit iOS 12, 12-TvOS und WatchOS 5

![Vorschau](~/media/shared/preview.png)

> [!WARNING]
> Xamarin iOS-12, TvOS 12 und WatchOS-5-Unterstützung ist gegenwärtig im vorschaustadium, d. h., er möglicherweise Fehler enthalten ist nicht die Features, und kann sich ändern. Verwenden sie nur für Experimente.

> [!NOTE]
> Weitere Informationen finden Sie in der Xamarin-Vorschauversion [release Blogbeitrag](https://releases.xamarin.com/preview-release-xcode-10-beta-3/).

In diesem Dokument wird beschrieben, wie abzurufenden Build iOS 12, 12-TvOS und WatchOS-5-apps mit Xamarin eingerichtet wird. Es wird erläutert, wie Xcode 10 herunterladen und Aktualisieren von Visual Studio für Mac und Visual Studio 2017.

## <a name="download-and-install"></a>Herunterladen und installieren

1. **Installieren Sie die neueste Betaversion des Xcode-10** – registrierten Apple-Entwickler können herunterladen und installieren Sie die neueste Version von Xcode 10 aus dem [Apple Developer Portal](https://developer.apple.com/download/).

   > [!NOTE]
   > Die iOS-12, TvOS-12 und WatchOS-5-SDKs werden mit Xcode 10 verteilt.

2. **Führen Sie die Xcode-10** – Ausführen von Xcode 10 vor dem Aktualisieren und Ausführen von Visual Studio für Mac oder Visual Studio 2017; einige Tools, die Xamarin erfordert installiert.

3. **Aktualisieren von Visual Studio für Mac und Visual Studio 2017** – befolgen Sie die Anweisungen auf der [release Blog](https://releases.xamarin.com/preview-release-xcode-10-beta-3/) zum Installieren der Xamarin-Vorschauversion.

4. _(optional)_  **Installieren Sie die neueste Betaversion für iOS auf Ihrem iOS-Geräten** – für den Test des Geräts mit den apps, die verwenden der neu eingeführten iOS 12, TvOS 12 oder WatchOS 5 registrierten Apple-Entwickler-APIs können [herunterladen](https://developer.apple.com/download) und Installieren Sie die aktuelle iOS 12, TvOS 12 oder Betaversionen der WatchOS-5-Entwickler auf ihren Geräten.

   > [!TIP]
   > Auch wenn Ihre app keine neuen iOS-12, TvOS 12 oder WatchOS 5 verwendet APIs, achten Sie darauf, dass Sie für die Erstellung mit dem 12-iOS, TvOS 12 oder WatchOS 5 SDK (installiert mit Xcode 10) und der Test sicherstellen, dass alles, wie funktioniert erwartet. Wenn eine app keine neuen APIs nicht aufruft, Sie neu kompilieren können sie mit der 12-iOS, TvOS 12 oder WatchOS 5 SDK und Testen Sie es auf Geräten, die noch nicht auf die neue Betriebssysteme aktualisiert wurden.

   > [!IMPORTANT]
   > Vor dem Upgrade von Ihren Geräts auf 12-iOS, TvOS 12 oder WatchOS 5 zum Testen von Xamarin-Anwendungen, die neuen iOS-12, 12-TvOS oder WatchOS 5 aufrufen APIs:
   > - Lesen [Apple – Anmerkungen zu dieser](https://developer.apple.com/download/) für das Update des Betriebssystems.
   > - Lesen die Xamarin-Vorschauversion [release Blogbeitrag](https://releases.xamarin.com/preview-release-xcode-10-beta-3/).

## <a name="related-links"></a>Verwandte Links

- [Xcode 10 herunterladen](https://developer.apple.com/download/)
- Xamarin-Vorschauversion [release Blogbeitrag](https://releases.xamarin.com/preview-release-xcode-10-beta-3/)
