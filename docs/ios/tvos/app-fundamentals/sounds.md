---
title: Wiedergabe von Sound in tvos. außerdem wurden mit AVAudioPlayer in Xamarin
description: Dieser Artikel zeigt, wie eine Hilfsklasse, mit der die Wiedergabe von sound mithilfe einer AVAudioPlayer in einer Anwendung Xamarin.iOS steuern.
ms.prod: xamarin
ms.assetid: E0305572-DC64-48BB-BD97-0A5096E6CA04
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 7d95a8ea6c22c0d897d8ccfe0c2ca401f6523783
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788629"
---
# <a name="playing-sound-in-tvos-with-avaudioplayer-in-xamarin"></a>Wiedergabe von Sound in tvos. außerdem wurden mit AVAudioPlayer in Xamarin

## <a name="about-the-avaudioplayer"></a>Informationen zu den AVAudioPlayer

Die `AVAudioPlayer` wird verwendet, um die Wiedergabe Audiodaten aus dem Arbeitsspeicher oder eine Datei. Apple empfiehlt die Verwendung dieser Klasse in Ihrer app audio wiedergegeben, es sei denn, Sie nehmen das streaming-Netzwerk oder erfordern mit geringer Latenz-audio-e/a.

Sie können die `AVAudioPlayer` die folgenden Schritte ausführen:

- Wiedergeben von Sounds beliebiger Dauer mit optionalen Schleifen.
- Wiedergeben Sie mehrere Sound, zur gleichen Zeit mit optionalen Synchronisierung.
- Steuern Sie Volumes, Wiedergaberate und Stereo Positionierung für jede Wiedergeben von Sounds.
- Unterstützung von Funktionen wie schneller Vorwärtscursor oder Rewind.
- Wiedergabe auf Daten zu erhalten.

`AVAudioPlayer` unterstützt von Sounds in einem beliebigen audio Format wie z. B. von tvos. außerdem wurden, iOS und OS X bereitgestellt `.aif`, `.wav` oder `.mp3`.

## <a name="playing-sounds-in-tvos"></a>Wiedergabe von Tönen in tvos. außerdem wurden

Da tvos. außerdem wurden die gleichen Audio Toolbox Klassen als iOS unterstützt, finden Sie unter unserer iOS [Playing Sound mit AVAudioPlayer](http://developer.xamarin.com/recipes/ios/media/sound/avaudioplayer/) Dokumentation für die vollständigen Details der Audiowiedergabe in einer app Xamarin.tvOS.



## <a name="related-links"></a>Verwandte Links

- [AVAudioPlayer-Referenz](https://developer.apple.com/library/ios/documentation/AVFoundation/Reference/AVAudioPlayerClassReference/)
