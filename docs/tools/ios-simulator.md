---
title: iOS-Remotesimulator für Windows
description: Remote-iOS-Simulator für Windows können Sie zum Testen Ihrer apps auf einem iOS-Simulator unter Windows zusammen mit Visual Studio 2017 angezeigt.
ms.prod: xamarin
ms.assetid: 63c50190-7e54-4140-a30d-1a0e577c47d7
author: topgenorth
ms.author: toopge
ms.date: 05/11/2018
ms.openlocfilehash: b07cc24e63f4aa3ce4451e3bdb5819f1df1058c6
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2018
---
# <a name="remoted-ios-simulator-for-windows"></a>iOS-Remotesimulator für Windows

Remote-iOS-Simulator für Windows können Sie zum Testen Ihrer apps auf einem iOS-Simulator unter Windows zusammen mit Visual Studio 2017 angezeigt.

[![](ios-simulator-images/hero-sml.png "iOS-Simulator unter Windows")](ios-simulator-images/hero.png#lightbox)

## <a name="getting-started"></a>Erste Schritte

Die Remote-iOS-Simulator für Windows wird automatisch als Teil der Xamarin in Visual Studio 2017 installiert. Gehen Sie folgendermaßen vor, um es verwenden zu um können:

1. [Visual 2017 mit einem Mac-buildhost koppeln](~/ios/get-started/installation/windows/connecting-to-mac/index.md).
2. Starten Sie in Visual Studio 2017 Debuggen eines Projekts auf iOS oder tvos. außerdem wurden ein. Die Remote-iOS-Simulator für Windows wird auf dem Windows-Computer angezeigt.

## <a name="simulator-window"></a>Simulatorfenster

Auf die Symbolleiste am oberen Rand des Simulators Fensters enthält eine Reihe von Schaltflächen nützlich:

- **Home** – simuliert die Schaltfläche "Start" auf einem iOS-Gerät
- **Sperre** – Sperren Sie den Simulator (Wischen zum Entsperren erforderlich)
- **Bildschirmabbildung von** – speichert einen Screenshot des Simulators
- [**Einstellungen** ](#settings) – zeigt an, Tastatur, Speicherort und andere Einstellungen
- [**Andere Optionen** ](#other-options) – bringt verschiedene Simulator-Optionen, wie Drehung und Shake Gesten

    [![](ios-simulator-images/maps-app-sml.png "iOS-Simulator zuordnet, Beispiel")](ios-simulator-images/maps-app.png#lightbox)

## <a name="settings"></a>Einstellungen

Klicken Sie auf der Symbolleiste Zahnradsymbol das **Einstellungen** Fenster:

[![](ios-simulator-images/settings-sml.png "Einstellungen für iOS-simulator")](ios-simulator-images/settings.png#lightbox)

Diese Einstellungen können Sie die Hardwaretastatur aktivieren, wählen einen Speicherort, der das Gerät sollte Bericht (statische und dynamische Standorten werden beide unterstützt), Touch ID zu aktivieren und den Inhalt und die Einstellungen für den Simulator zurücksetzen.

## <a name="other-options"></a>Weitere Optionen

Der Symbolleiste auf die Schaltfläche wird die anderen Optionen wie Drehung, Shake Gesten und neu zu starten. Diese gleichen Optionen können als Liste mit der rechten Maustaste an einer beliebigen Stelle in der Simulator-Fenster angezeigt werden:

[![](ios-simulator-images/more-sml.png "zusätzliche Einstellungen für iOS-simulator")](ios-simulator-images/more.png#lightbox)

## <a name="touchscreen-support"></a>Touchscreen-Unterstützung

Die meisten modernen Windows-Computer haben Touchscreens. Da die Remoteausführung iOS-Simulator für Windows Touch-Interaktionen unterstützt, können Sie Testen Ihrer app mit der gleichen verkleinern, Streifen und mit mehreren Finger Fingereingabe, die mit physischen iOS-Geräte verwendet.

Auf ähnliche Weise behandelt prozessübergreifendes iOS-Simulator für Windows Windows Tablettstift Eingabe als Eingabe der Apple-Zeichenstift.

## <a name="disabling-the-remoted-ios-simulator-for-windows"></a>Deaktivieren von Remote-iOS-Simulator für Windows

Um die Remote-iOS-Simulator für Windows zu deaktivieren, navigieren Sie zu **Tools > Optionen > Xamarin > für den iOS** und deaktivieren Sie **Remote-Simulator für Windows**.

[![](ios-simulator-images/options-sml.png "Kontrollkästchen, um den Simulator verwenden.")](ios-simulator-images/options.png#lightbox)

Mit dieser Option öffnet Debuggen deaktiviert, erstellen Sie den iOS-Simulator auf dem Mac verbundenen Host.
