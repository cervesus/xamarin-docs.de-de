---
title: Wiedergabe von Sound mit AVAudioPlayer in xamarin. Mac
description: In diesem Dokument wird beschrieben, wie Sie mit AVAudioPlayer in einer xamarin. Mac-app Sound abspielen. Er erläutert AVAudioPlayer auf hoher Ebene und enthält Links zu anderen Dokumentationen, in denen er vollständig untersucht wird.
ms.prod: xamarin
ms.assetid: 4A683A94-F75D-4EAF-8497-E9443653250B
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 10/19/2016
ms.openlocfilehash: 18043a88a129d48a1cad3b9ee15b6989d50ad126
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030045"
---
# <a name="playing-sound-with-avaudioplayer-in-xamarinmac"></a>Wiedergabe von Sound mit AVAudioPlayer in xamarin. Mac

## <a name="about-the-avaudioplayer"></a>Informationen zu AVAudioPlayer

Die `AVAudioPlayer`-Klasse wird verwendet, um Audiodaten entweder aus dem Arbeitsspeicher oder aus einer Datei wiederzugeben. Apple empfiehlt die Verwendung dieser Klasse, um Audiodaten in der APP wiederzugeben, es sei denn, Sie verwenden das Netzwerk Streaming oder benötigen audioe/a mit niedriger Latenz.

Mit der `AVAudioPlayer`-Klasse können Sie folgende Aufgaben ausführen:

- Wiedergabe von Sounds beliebiger Dauer mit optionalen Schleifen.
- Gleich Zeitangabe mehrerer Sounds mit optionaler Synchronisierung.
- Steuern Sie das Volumen, die Wiedergabe Rate und die Stereo Positionierung der einzelnen Sounds.
- Unterstützung von Features wie "schneller vorwärts" oder "Zurückspulen".
- Abrufen von Messungs Daten auf Wiedergabe Ebene.

`AVAudioPlayer` unterstützt Sounds in beliebigen Audioformaten, die von IOS, tvos und macOS bereitgestellt werden, wie z. b. AIF,. wav oder. MP3.

## <a name="playing-sounds-in-macos"></a>Abspielen von Sounds in macOS

Da macOS die gleichen audiotoolbox-Klassen wie IOS unterstützt, finden Sie in unserer Dokumentation zu den IOS- [spielen mit der AVAudioPlayer](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/sound/avaudioplayer) -Dokumentation ausführliche Informationen zur Wiedergabe von Audio in einer xamarin. Mac-app.

## <a name="related-links"></a>Verwandte Links

- [AVAudioPlayer-Referenz](https://developer.apple.com/documentation/avfoundation/avaudioplayer)
