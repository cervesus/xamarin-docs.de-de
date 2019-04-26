---
title: 'Fehler beim Senden von App-Store: "Ungültiges Paket: Optionen, die nicht in Bitcode eingebettet werden können, werden in der Übermittlung erkannt"'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 137313FB-3D29-428B-93C1-5A05DC8F7C03
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/03/2018
ms.openlocfilehash: 867ad29abfa6a38971b60ac9ebf181905949dafd
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61421728"
---
# <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submission"></a>Fehler beim Senden von App-Store: "Ungültiges Paket: Optionen, die nicht in Bitcode eingebettet werden können, werden in der Übermittlung erkannt"

WatchOS und TvOS-apps _erfordern_ Bitcode, wenn sie an den App Store übermittelt werden. Beim Erstellen und Übermitteln von WatchOS und TvOS-apps mithilfe von Xcode 8.3 oder früher, kann der folgende Fehler auftreten, (über e-Mail-Benachrichtigung) beim Versuch, die in den App Store hochladen:

>Ungültiges Paket: die app kann nicht verarbeitet werden, da die Optionen, die nicht in Bitcode eingebettet werden können, die in der Übermittlung erkannt werden. Es ist wahrscheinlich, dass Sie die app nicht in die toolkette, die in Xcode bereitgestellten erstellen.

Die Lösung des Problems ist die Erstellung von Anwendungen mit Xcode 9 und die neueste Version von Xamarin.iOS.
