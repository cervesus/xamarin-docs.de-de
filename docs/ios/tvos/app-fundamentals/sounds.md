---
title: Wiedergabe von Sound in tvos mit AVAudioPlayer in xamarin
description: In diesem Artikel wird gezeigt, wie Sie eine Hilfsklasse verwenden, um die Wiedergabe von Sound mithilfe von AVAudioPlayer in einer xamarin. IOS-Anwendung zu steuern.
ms.prod: xamarin
ms.assetid: E0305572-DC64-48BB-BD97-0A5096E6CA04
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: 4dddde8d4408df6a9b9d73c0a3efff62f563591a
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030780"
---
# <a name="playing-sound-in-tvos-with-avaudioplayer-in-xamarin"></a>Wiedergabe von Sound in tvos mit AVAudioPlayer in xamarin

## <a name="about-the-avaudioplayer"></a>Informationen zu AVAudioPlayer

Der `AVAudioPlayer` wird verwendet, um Audiodaten aus dem Arbeitsspeicher oder aus einer Datei wiederzugeben. Apple empfiehlt die Verwendung dieser Klasse, um Audiodaten in der APP wiederzugeben, es sei denn, Sie verwenden das Netzwerk Streaming oder benötigen audioe/a mit niedriger Latenz.

Mit dem `AVAudioPlayer` können Sie folgende Aufgaben ausführen:

- Wiedergabe von Sounds beliebiger Dauer mit optionalen Schleifen.
- Gleich Zeitangabe mehrerer Sounds mit optionaler Synchronisierung.
- Steuern Sie das Volumen, die Wiedergabe Rate und die Stereo Positionierung der einzelnen Sounds.
- Unterstützung von Features wie "schneller vorwärts" oder "Zurückspulen".
- Abrufen von Messungs Daten auf Wiedergabe Ebene.

`AVAudioPlayer` unterstützt Sounds in beliebigen Audioformaten, die von IOS, tvos und OS X bereitgestellt werden, z. b. `.aif`, `.wav` oder `.mp3`.

## <a name="playing-sounds-in-tvos"></a>Abspielen von Sounds in tvos

Da tvos die gleichen Audio-Toolbox-Klassen wie IOS unterstützt, finden Sie in unserer Dokumentation zu den IOS- [spielen mit der AVAudioPlayer](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/sound/avaudioplayer) -Dokumentation ausführliche Informationen zur Wiedergabe von Audio in einer xamarin. tvos-app.

## <a name="related-links"></a>Verwandte Links

- [AVAudioPlayer-Referenz](https://developer.apple.com/library/ios/documentation/AVFoundation/Reference/AVAudioPlayerClassReference/)
