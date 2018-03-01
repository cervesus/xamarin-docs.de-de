---
title: Wiedergabe von sound mit AVAudioPlayer
description: Dieser Artikel zeigt, wie eine Hilfsklasse, mit der die Wiedergabe von sound mithilfe einer AVAudioPlayer steuern.
ms.topic: article
ms.prod: xamarin
ms.assetid: 4A683A94-F75D-4EAF-8497-E9443653250B
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/19/2016
ms.openlocfilehash: b3eb3f16f358becb22029cee295ef6064caad85a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="playing-sound-with-avaudioplayer"></a>Wiedergabe von sound mit AVAudioPlayer

_Dieser Artikel zeigt, wie eine Hilfsklasse, mit der die Wiedergabe von sound mithilfe einer AVAudioPlayer steuern._

## <a name="about-the-avaudioplayer"></a>Informationen zu den AVAudioPlayer

Die `AVAudioPlayer` Klasse dient zur Wiedergabe von Audiodaten aus dem Arbeitsspeicher oder eine Datei. Apple empfiehlt die Verwendung dieser Klasse in Ihrer app audio wiedergegeben, es sei denn, Sie nehmen das streaming-Netzwerk oder erfordern mit geringer Latenz-audio-e/a.

Sie können die `AVAudioPlayer` Klasse, um die folgenden Aktionen ausführen:

- Wiedergeben von Sounds beliebiger Dauer mit optionalen Schleifen.
- Wiedergeben Sie mehrere Sound, zur gleichen Zeit mit optionalen Synchronisierung.
- Steuern Sie Volumes, Wiedergaberate und Stereo Positionierung für jede Wiedergeben von Sounds.
- Unterstützung von Funktionen wie schneller Vorwärtscursor oder Rewind.
- Wiedergabe auf Daten zu erhalten.

`AVAudioPlayer` Sounds unterstützt in alle gebotenen iOS-, tvos. außerdem wurden und MacOS z. B. .aif, im WAV- oder MP3-audio-Format.

## <a name="playing-sounds-in-macos"></a>Wiedergabe von Tönen in macOS

Da MacOS dieselben Audio Toolbox Klassen als iOS unterstützt, finden Sie unter unserer iOS [Playing Sound mit AVAudioPlayer](https://developer.xamarin.com/recipes/ios/media/sound/avaudioplayer/) Dokumentation für die vollständigen Details der Audiowiedergabe in einer app Xamarin.Mac.



## <a name="related-links"></a>Verwandte Links

- [AVAudioPlayer-Referenz](https://developer.apple.com/documentation/avfoundation/avaudioplayer)
