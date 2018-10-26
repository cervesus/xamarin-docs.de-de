---
title: Behandeln von Toucheingaben in Xamarin.iOS-Apps
description: Dieses Dokument enthält Links zu Anleitungen, die beschreiben, wie Sie mit Touch, Multitouch, Gesten und 3D Touch in einer Xamarin.iOS-app arbeiten.
ms.prod: xamarin
ms.assetid: E3904713-6018-4755-A315-EB045DFB3500
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 01/23/2017
ms.openlocfilehash: 5aabc3a3c2ffbcffc0e12379989f7eb43b03a902
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116627"
---
# <a name="handling-touch-in-xamarinios-apps"></a>Behandeln von Toucheingaben in Xamarin.iOS-Apps

Wie andere mobilen Plattformen hat iOS eine Reihe von Möglichkeiten, Fingereingabe zu behandeln. Es kann Multitouch unterstützen – viele Datenpunkte wenden Sie sich an, auf dem Bildschirm – und komplexe Gesten. Dieser Leitfaden beschreibt einige der Konzepte, sowie den Besonderheiten der Implementierung von Berührung und Gesten unter iOS.

iOS kapselt Touch-Daten in die `UITouch` -Klasse, die eine Reihe von Anwendungen zur Verfügung gestellt wird `UIResponder` Methoden. Anwendungen können außer Kraft setzen dieser Methoden in Unterklassen von `UIView` und `UIViewController`, beide erben von `UIResponder`.

Neben dem Erfassen von Touch-Daten, bietet iOS bedeutet, dass für das Interpretieren der Muster der Workflows in die Eingabeaktionen. Diese Geste Erkennungen können wiederum verwendet werden, um anwendungsspezifische-Befehle, wie eine Drehung um ein Bild oder eine Reihe von einer Seite zu interpretieren. iOS bietet eine umfangreiche Sammlung von Klassen, allgemeine Gesten minimalen zusätzlichen Code zu behandeln.

Die Wahl zwischen Workflows und Bewegung Erkennungen kann eine verwirrend sein. Es wird empfohlen, im Allgemeinen Präferenz zu Geste Erkennungen gewährt werden soll. Geste Erkennungen werden als diskrete Klassen implementiert, die stärkere Trennung von Belangen und eine bessere Kapselung bereitstellen. Dies können sie ganz leicht die Logik zwischen verschiedenen Ansichten, minimieren die Menge an geschriebenen Code gemeinsam nutzen.

Es gibt jedoch müssen Sie manchmal Low-Level-Touch-Verarbeitung und sogar mehrere Finger, z. B. zum Erstellen eines Programms Finger-paint nachverfolgen.

## <a name="sections"></a>Abschnitte

-  [Toucheingabe in iOS](touch-in-ios.md)
-  [Exemplarische Vorgehensweise: Verwenden von Toucheingaben in iOS](ios-touch-walkthrough.md)
-  [Nachverfolgen von Multitoucheingaben](touch-tracking.md)

Dieses Handbuch dient als Einführung in die Toucheingabe in iOS. Weitere Informationen zum Verwenden von 3D Touch und Übermitteln von Haptischem Feedback in iOS wurden die in iOS 9 und 10 bzw. finden Sie in der unten angegebenen speziellen Anleitungen eingeführt:

* [3D Touch](~/ios/platform/3d-touch.md)
* [Übermitteln von haptischem Feedback](~/ios/user-interface/ios-ui/haptic-feedback.md)

## <a name="related-links"></a>Verwandte Links

- [iOS-Touch (Beispiel) starten](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start)
- [iOS letzte Touch (Beispiel)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final)
- [FingerPaint (Beispiel)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint)
