---
title: 'Fehler beim Senden an den App Store: "Ungültige Bundle-Optionen, die nicht in Bitcode eingebettet werden dürfen, werden in der Übermittlung erkannt"'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 137313FB-3D29-428B-93C1-5A05DC8F7C03
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
ms.openlocfilehash: c6a03fa3327e81f577f85b05a033d9c97495eb2e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031075"
---
# <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submission"></a>Fehler beim Senden an den App Store: "Ungültige Bundle-Optionen, die nicht in Bitcode eingebettet werden dürfen, werden in der Übermittlung erkannt"

watchos-apps und tvos-apps _erfordern_ Bitcode, wenn Sie an den App Store übermittelt werden. Beim entwickeln und Übermitteln von watchos-und tvos-apps mit Xcode 8,3 oder früher tritt möglicherweise der folgende Fehler auf (per e-Mail-Benachrichtigung), wenn versucht wird, in den App Store hochzuladen:

>Ungültiges Bündel: die APP kann nicht verarbeitet werden, weil Optionen, die nicht in Bitcode eingebettet werden dürfen, bei der Übermittlung erkannt werden. Es ist wahrscheinlich, dass Sie die APP nicht mit der in Xcode bereitgestellten Toolkette entwickeln.

Die Lösung für dieses Problem besteht darin, die Anwendungen mit Xcode 9 und der neuesten Version von xamarin. IOS zu erstellen.
