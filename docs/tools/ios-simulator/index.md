---
title: iOS-Remotesimulator für Windows
description: Mit dem iOS-Remotesimulator für Windows können Sie Ihre Apps in einem iOS-Simulator testen, der unter Windows zusammen mit Visual Studio 2019 angezeigt wird.
ms.prod: xamarin
ms.assetid: 63c50190-7e54-4140-a30d-1a0e577c47d7
author: davidortinau
ms.author: daortin
ms.date: 04/26/2019
ms.openlocfilehash: 84c5a72f887f38c5efcee09cfc19986093db226e
ms.sourcegitcommit: d86b7a18cf8b1ef28cd0fe1d311f1c58a65101a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2020
ms.locfileid: "85101348"
---
# <a name="remoted-ios-simulator-for-windows"></a>iOS-Remotesimulator für Windows

Mit dem iOS-Remotesimulator für Windows können Sie Ihre Apps in einem iOS-Simulator testen, der unter Windows zusammen mit Visual Studio 2019 und Visual Studio 2017 angezeigt wird.

[![iOS-Simulator unter Windows](images/hero-sml.png "iOS-Simulator unter Windows")](images/hero.png#lightbox)

## <a name="getting-started"></a>Erste Schritte

Der iOS-Remotesimulator für Windows wird automatisch als Teil von Xamarin in Visual Studio 2019 und Visual Studio 2017 installiert. Führen Sie folgende Schritte aus, um den Simulator zu verwenden:

1. [Koppeln Sie Visual Studio 2019 mit einem Mac-Buildhost](~/ios/get-started/installation/windows/connecting-to-mac/index.md).
2. Starten Sie in Visual Studio das Debuggen eines iOS- oder tvOS-Projekts. Der iOS-Remotesimulator für Windows wird auf Ihrem Windows-Computer geöffnet.

Eine ausführliche Anleitung finden Sie in [diesem Video](deploy.md).

## <a name="simulator-window"></a>Simulatorfenster

Die Symbolleiste im oberen Bereich des Simulatorfensters enthält eine Reihe nützlicher Schaltflächen:

- **Home** – simuliert die Home-Schaltfläche auf einem iOS-Gerät.
- **Sperren** – Sperrt den Simulator (zum Entsperren wischen).
- **Screenshot** – speichert einen Screenshot des Simulators (unter **Pictures\Xamarin\iOS Simulator\\** ).
- [**Einstellungen**](#settings) – öffnet Tastatur-, Standort- und andere Einstellungen.
- [**Weitere Optionen**](#other-options) – öffnet verschiedene Simulatoroptionen (z.B. Drehen, Schüttelbewegungen und Touch ID).

    [![Beispiel einer Karte im iOS-Simulator](images/maps-app-sml.png "Beispiel einer Karte im iOS-Simulator")](images/maps-app.png#lightbox)

## <a name="settings"></a>Einstellungen

Wenn Sie in der Symbolleiste auf das Zahnradsymbol klicken, wird das Fenster **Einstellungen** geöffnet:

[![Einstellungen des iOS-Simulators](images/settings-sml.png "Einstellungen des iOS-Simulators")](images/settings.png#lightbox)

Über diese Einstellungen können Sie die Hardwaretastatur aktivieren und einen Standort auswählen, der vom Gerät übermittelt wird (es kann sowohl ein fester Standort als auch die kontinuierliche Aktualisierung des Standorts ausgewählt werden). Außerdem können Sie Touch ID aktivieren und den Inhalt und die Einstellungen für den Simulator zurücksetzen.

## <a name="other-options"></a>Weitere Optionen

Über die Schaltfläche mit Auslassungspunkten auf der Symbolleiste können Sie auf weitere Optionen (z.B. zum Drehen, zu Schüttelbewegungen und für den Neustart) zugreifen. Diese Optionen können auch in einer Liste angezeigt werden, indem Sie mit der rechten Maustaste auf eine beliebige Stelle im Fenster des Simulators klicken:

[![Zusätzliche Einstellungen des iOS-Simulators](images/more-sml.png "Zusätzliche Einstellungen des iOS-Simulators")](images/more.png#lightbox)

## <a name="touchscreen-support"></a>Touchscreen-Unterstützung

Die meisten modernen Windows-Computer verfügen über Touchscreens. Da der iOS-Remotesimulator für Windows Touchinteraktionen unterstützt, können Sie Ihre App mit denselben Fingerbewegungen (Finger zusammenführen, wischen und Mehrfingerbewegungen) testen, die Sie bei physischen iOS-Geräten verwenden.

Gleichermaßen behandelt der iOS-Remotesimulator für Windows Schrifteingaben wie Apple Pencil-Eingaben.

## <a name="sound-handling"></a>Soundwiedergabe

Die vom Simulator wiedergegebenen Sounds stammen von den Lautsprechern des Host-Macs.
iOS-Sounds sind auf dem Windows-Computer nicht hörbar.

## <a name="disabling-the-remoted-ios-simulator-for-windows"></a>Deaktivieren des iOS-Remotesimulators für Windows

Zum Deaktivieren des iOS-Remotesimulators für Windows wechseln Sie zu **Tools > Optionen > Xamarin > iOS-Einstellungen** und deaktivieren das Kontrollkästchen **Simulator remote auf Windows**.

[![Kontrollkästchen zur Verwendung des Simulators](images/options-sml.png "Kontrollkästchen zur Verwendung des Simulators")](images/options.png#lightbox)

Wenn diese Option deaktiviert ist, wird beim Debuggen der iOS-Simulator auf dem verbundenen Mac-Buildhost geöffnet.

## <a name="troubleshooting"></a>Problembehandlung

Wenn bei Verwendung des iOS-Remotesimulators Probleme auftreten, können Sie in diesen Verzeichnissen auf Protokolle zugreifen:

- **Mac:** `~/Library/Logs/Xamarin/Simulator.Server`
- **Windows:** `%LOCALAPPDATA%\Xamarin\Logs\Xamarin.Simulator`

Wenn Sie [ein Problem in Visual Studio melden](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio), kann es hilfreich sein, diese Protokolle anzufügen (es sind Optionen verfügbar, um Uploads als privat festzulegen).
