---
title: 'Fehler beim Senden von App-Store: "Ungültiges Paket: Optionen, die nicht in Bitcode eingebettet werden können, werden in der Übermittlung erkannt"'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 137313FB-3D29-428B-93C1-5A05DC8F7C03
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 393c1ed81c68d21b610781dfe09de97969e031d1
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350923"
---
# <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submission"></a>Fehler beim Senden von App-Store: "Ungültiges Paket: Optionen, die nicht in Bitcode eingebettet werden können, werden in der Übermittlung erkannt"

WatchOS und TvOS-apps _erfordern_ Bitcode, wenn sie an den App Store übermittelt werden. Beim Erstellen und Übermitteln von WatchOS und TvOS-apps mithilfe von Xcode 8.3 oder früher, kann der folgende Fehler auftreten, (über e-Mail-Benachrichtigung) beim Versuch, die in den App Store hochladen:

>Ungültiges Paket: die app kann nicht verarbeitet werden, da die Optionen, die nicht in Bitcode eingebettet werden können, die in der Übermittlung erkannt werden. Es ist wahrscheinlich, dass Sie die app nicht in die toolkette, die in Xcode bereitgestellten erstellen.

Die Lösung des Problems ist die Erstellung von Anwendungen mit Xcode 9 und die neueste Version von Xamarin.iOS.
