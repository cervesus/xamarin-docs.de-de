---
title: Wiedergabe von Sound in tvos mit AVAudioPlayer in xamarin
description: In diesem Artikel wird gezeigt, wie Sie eine Hilfsklasse verwenden, um die Wiedergabe von Sound mithilfe von AVAudioPlayer in einer xamarin. IOS-Anwendung zu steuern.
ms.prod: xamarin
ms.assetid: E0305572-DC64-48BB-BD97-0A5096E6CA04
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/16/2017
ms.openlocfilehash: 57892689eeb5eef9747e19fa167b8598569f3cd1
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769207"
---
# <a name="playing-sound-in-tvos-with-avaudioplayer-in-xamarin"></a>Wiedergabe von Sound in tvos mit AVAudioPlayer in xamarin

## <a name="about-the-avaudioplayer"></a>Informationen zu AVAudioPlayer

`AVAudioPlayer` Wird verwendet, um Audiodaten entweder aus dem Arbeitsspeicher oder aus einer Datei wiederzugeben. Apple empfiehlt die Verwendung dieser Klasse, um Audiodaten in der APP wiederzugeben, es sei denn, Sie verwenden das Netzwerk Streaming oder benötigen audioe/a mit niedriger Latenz.

Mit dem `AVAudioPlayer` können Sie folgende Aufgaben ausführen:

- Wiedergabe von Sounds beliebiger Dauer mit optionalen Schleifen.
- Gleich Zeitangabe mehrerer Sounds mit optionaler Synchronisierung.
- Steuern Sie das Volumen, die Wiedergabe Rate und die Stereo Positionierung der einzelnen Sounds.
- Unterstützung von Features wie "schneller vorwärts" oder "Zurückspulen".
- Abrufen von Messungs Daten auf Wiedergabe Ebene.

`AVAudioPlayer`unterstützt Sounds in beliebigen Audioformaten, die von IOS, tvos und OS X `.aif`bereit `.wav` gestellt `.mp3`werden, z. b. oder.

## <a name="playing-sounds-in-tvos"></a>Abspielen von Sounds in tvos

Da tvos die gleichen Audio-Toolbox-Klassen wie IOS unterstützt, finden Sie in unserer Dokumentation zu den IOS- [spielen mit der AVAudioPlayer](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/sound/avaudioplayer) -Dokumentation ausführliche Informationen zur Wiedergabe von Audio in einer xamarin. tvos-app.

## <a name="related-links"></a>Verwandte Links

- [AVAudioPlayer-Referenz](https://developer.apple.com/library/ios/documentation/AVFoundation/Reference/AVAudioPlayerClassReference/)
