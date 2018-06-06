---
title: Wiedergabe von sound mit AVAudioPlayer in Xamarin.Mac
description: Dieses Dokument beschreibt, wie mit AVAudioPlayer in einer app Xamarin.Mac sound wiedergegeben wird. Es wird erläutert, AVAudioPlayer zu einem hohen und Links zu anderen Dokumentation, die es umfassender wird untersucht.
ms.prod: xamarin
ms.assetid: 4A683A94-F75D-4EAF-8497-E9443653250B
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/19/2016
ms.openlocfilehash: 9e5b9ec43189999f8a0aee29eb50221b494e2133
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791854"
---
# <a name="playing-sound-with-avaudioplayer-in-xamarinmac"></a>Wiedergabe von sound mit AVAudioPlayer in Xamarin.Mac

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
