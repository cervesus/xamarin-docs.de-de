---
title: 'Fehler beim Senden von App-Speicher: "Ungültige Paket - Optionen, die nicht zulässig, die in Bitcode eingebettet werden in die Übermittlung erkannt werden"'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 137313FB-3D29-428B-93C1-5A05DC8F7C03
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: cacb9040ddc8582490c68bcfd24e80c4c4679eb4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submission"></a>Fehler beim Senden von App-Speicher: "Ungültige Paket - Optionen, die nicht zulässig, die in Bitcode eingebettet werden in die Übermittlung erkannt werden"

WatchOS apps und apps für tvos. außerdem wurden _erfordern_ Bitcode, wenn sie auf den App Store übermittelt werden. Beim Erstellen und übermitteln WatchOS und tvos. außerdem wurden apps mithilfe von Xcode 8.3 oder früher, kann der folgende Fehler auftreten, (per e-Mail-Benachrichtigung) beim Versuch, auf den App Store hochladen:

>Ungültige Paket - app kann nicht verarbeitet werden, weil Optionen, die nicht zulässig, die in Bitcode eingebettet werden bei der Übermittlung erkannt werden. Es ist wahrscheinlich, dass Sie die app nicht mit der toolkette in Xcode bereitgestellten erstellen.

Die Lösung für dieses Problem ist, die Anwendungen mit Xcode-9 und die neueste Version von Xamarin.iOS zu erstellen.
