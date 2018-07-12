---
title: Erste Schritte mit MacOS Mojave
description: In diesem Dokument wird beschrieben, wie abzurufenden Build MacOS Mojave-apps mit Xamarin.Mac eingerichtet wird. Es wird erläutert, wie Xcode 10 herunterladen und Aktualisieren von Visual Studio für Mac.
ms.prod: xamarin
ms.assetid: E9A7B68A-E164-4C5C-86AC-B2A3E7A30DA1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: ee5269bf314401328d0184631e817b37ce091479
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831344"
---
# <a name="getting-started-with-macos-mojave"></a>Erste Schritte mit MacOS Mojave

![Vorschau](~/media/shared/preview.png)

> [!WARNING]
> Xamarin Mac OS Mojave Unterstützung befindet sich derzeit in der Vorschau, was bedeutet, dass möglicherweise Fehler enthalten, ist es sich nicht um Features und kann sich ändern.
> Verwenden sie nur für Experimente.

> [!NOTE]
> Weitere Informationen finden Sie in der [Anmerkungen zu dieser Version](https://releases.xamarin.com/preview-release-xcode-10-beta/) für die Xamarin-Vorschauversion.

In diesem Dokument wird beschrieben, wie abzurufenden Build MacOS Mojave-apps mit Xamarin.Mac eingerichtet wird. Es wird erläutert, wie Xcode 10 herunterladen und Aktualisieren von Visual Studio für Mac.

## <a name="download-and-install"></a>Herunterladen und installieren

1. **Installieren Sie die neueste Betaversion des Xcode-10** – registrierten Apple-Entwickler können herunterladen und installieren Sie die neueste Version von Xcode 10 aus dem [Apple Developer Portal](https://developer.apple.com/download/).

   > [!NOTE]
   > MacOS Mojave SDK wird mit Xcode 10 verteilt.

2. **Führen Sie die Xcode-10** – Ausführen von Xcode 10 vor dem Aktualisieren und Ausführen von Visual Studio für Mac installiert einige Tools, die Xamarin ist erforderlich.

3. **Aktualisieren von Visual Studio für Mac** – befolgen Sie die Anweisungen in der [Anmerkungen zu dieser Version](https://releases.xamarin.com/preview-release-xcode-10-beta/) zum Installieren der Xamarin-Vorschauversion.

4. _(optional)_  **Installieren Sie die neueste Betaversion des MacOS-Mojave auf Ihrem Mac** – Xamarin.Mac-apps zu testen, die neu eingeführte MacOS Mojave registrierten Apple-Entwickler können APIs verwenden [herunterladen](https://developer.apple.com/download/) und installieren Sie die neueste MacOS Mojave Developer-Betaversion.

   > [!TIP]
   > Auch wenn Ihre app keine neuen MacOS Mojave-APIs verwendet, achten Sie darauf, dass Sie sich im MacOS-Mojave SDK (installiert mit Xcode 10) zu erstellen und testen, um sicherzustellen, dass alles wie erwartet funktioniert. Wenn eine app keine neuen APIs nicht aufruft, können Sie sich im MacOS-Mojave SDK neu kompilieren und testen, ohne das Betriebssystem Ihres MACS aktualisiert.

   > [!IMPORTANT]
   > Vor dem aktualisieren Ihren Mac, MacOS Mojave zum Erstellen und Testen von Xamarin.Mac-Anwendungen, die neue MacOS Mojave-APIs aufrufen:
   > - Lesen [Apple – Anmerkungen zu dieser](https://developer.apple.com/download/) für das Update des Betriebssystems.
   > - Lesen der [Anmerkungen zu dieser Version](https://releases.xamarin.com/preview-release-xcode-10-beta/) für die Xamarin-Vorschauversion. Beachten Sie, dass dieser erste Vorschauversion keine Bindungen für neue MacOS Mojave AppKit-APIs (z. B. dunkel Modus) enthält.

## <a name="related-links"></a>Verwandte Links

- [Xcode 10 herunterladen](https://developer.apple.com/download/)
- Xamarin-Vorschauversion [Anmerkungen zu dieser Version](https://releases.xamarin.com/preview-release-xcode-10-beta/)
