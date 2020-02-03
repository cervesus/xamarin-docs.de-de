---
title: Behandeln von Berührungen in xamarin. IOS-apps
description: Dieses Dokument ist mit Anleitungen verknüpft, die beschreiben, wie Touch, Multitouch, Gesten und 3D-Touch in einer xamarin. IOS-App verwendet werden.
ms.prod: xamarin
ms.assetid: E3904713-6018-4755-A315-EB045DFB3500
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 01/23/2017
ms.openlocfilehash: edf5f1c82857930a2647f306195a4c82f4338db6
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724629"
---
# <a name="handling-touch-in-xamarinios-apps"></a>Behandeln von Berührungen in xamarin. IOS-apps

Wie andere mobile Plattformen bietet IOS eine Reihe von Möglichkeiten, die Berührungs Möglichkeiten zu behandeln. Es kann Multitouch – viele Kontaktpunkte auf dem Bildschirm – und komplexe Gesten unterstützen. In diesem Handbuch werden einige der Konzepte sowie die Besonderheiten der Implementierung von toucheingaben und Gesten unter IOS vorgestellt.

IOS kapselt Berührungs Daten in der `UITouch`-Klasse, die Anwendungen über eine Reihe von `UIResponder` Methoden zur Verfügung gestellt wird. Anwendungen können diese Methoden in Unterklassen von `UIView` und `UIViewController`überschreiben, die beide von `UIResponder`erben.

Zusätzlich zur Erfassung von Touch-Daten bietet IOS die Möglichkeit, die Muster von Berührungen in Gesten zu interpretieren. Diese Gesten Erkennungs Tools können wiederum verwendet werden, um anwendungsspezifische Befehle zu interpretieren, z. b. eine Drehung eines Bilds oder eine Seite. IOS stellt eine umfangreiche Sammlung von Klassen bereit, um häufige Gesten mit dem minimal hinzugefügten Code zu behandeln.

Die Wahl zwischen den Berührungen und Gesten erkenungen kann verwirrend sein. In dieser Anleitung wird empfohlen, dass Gesten Erkennungs Tools im allgemeinen bevorzugt werden. Gesten Erkennungs Tools werden als diskrete Klassen implementiert, die eine stärkere Trennung von Anliegen und eine bessere Kapselung bieten. Dies vereinfacht die gemeinsame Nutzung der Logik zwischen unterschiedlichen Ansichten und minimiert so die Menge an geschriebenem Code.

Es gibt jedoch Zeiten, in denen Sie die Touch-Verarbeitung auf niedriger Ebene verwenden und sogar mehrere Finger nachverfolgen müssen, um z. b. ein Finger Paint-Programm zu erstellen.

## <a name="sections"></a>Abschnitte

- [Toucheingabe in iOS](touch-in-ios.md)
- [Exemplarische Vorgehensweise: Verwenden von Toucheingaben in iOS](ios-touch-walkthrough.md)
- [Nachverfolgen von Multitoucheingaben](touch-tracking.md)

Dieses Handbuch bietet eine Einführung in die Kontaktaufnahme in ios. Weitere Informationen zur Verwendung von 3D Toucheingabe und haptisches Feedback in Ios, die in ios 9 und 10 eingeführt wurden, finden Sie in den folgenden Anleitungen:

- [3D Touch](~/ios/platform/3d-touch.md)
- [Übermitteln von haptischem Feedback](~/ios/user-interface/ios-ui/haptic-feedback.md)

## <a name="related-links"></a>Verwandte Links

- [IOS-Berührungs Finale (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-touch-final)
- [Fingerpaint (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-fingerpaint)
