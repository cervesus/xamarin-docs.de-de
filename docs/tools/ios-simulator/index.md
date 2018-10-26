---
title: iOS-Remotesimulator für Windows
description: Den remoten iOS-Simulator für Windows können Sie Ihre apps auf einem iOS-Simulator angezeigt, die in Windows mit Visual Studio 2017 testen.
ms.prod: xamarin
ms.assetid: 63c50190-7e54-4140-a30d-1a0e577c47d7
author: lobrien
ms.author: laobri
ms.date: 05/11/2018
ms.openlocfilehash: c923a62916959c74b8cd753d25361afee68e75fe
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50131535"
---
# <a name="remoted-ios-simulator-for-windows"></a>iOS-Remotesimulator für Windows

Den remoten iOS-Simulator für Windows können Sie Ihre apps auf einem iOS-Simulator angezeigt, die in Windows mit Visual Studio 2017 testen.

[![](images/hero-sml.png "iOS-Simulator unter Windows")](images/hero.png#lightbox)

## <a name="getting-started"></a>Erste Schritte

Der Remote-iOS-Simulator für Windows wird automatisch als Teil von Xamarin in Visual Studio 2017 installiert werden. Gehen Sie folgendermaßen vor, um es zu verwenden:

1. [Koppeln Sie Visual Studio 2017 auf einem Mac Build Host](~/ios/get-started/installation/windows/connecting-to-mac/index.md).
2. Debuggen Sie in Visual Studio 2017 ein IOS- oder TvOS-Projekt. Der Remote-iOS-Simulator für Windows wird auf Ihrem Windows-Computer angezeigt.

Sehen Sie sich [in diesem Video](deploy.md) eine schrittweise Anleitung.

## <a name="simulator-window"></a>Simulatorfenster

Auf die Symbolleiste am oberen Rand des Simulators überein Fensters enthält eine Reihe von Schaltflächen der hilfreich:

- **Home** – simuliert die Schaltfläche "Start" auf einem iOS-Gerät
- **Sperre** – Sperren Sie den Simulator (Wischen zum Entsperren erforderlich)
- **Bildschirmabbildung von** – speichert einen Screenshot des Simulators
- [**Einstellungen** ](#settings) – zeigt an, Tastatur, Speicherort und andere Einstellungen
- [**Andere Optionen** ](#other-options) – bringt verschiedene Simulator-Optionen wie z. B. Rotation "und" Shake-Gesten

    [![](images/maps-app-sml.png "iOS-Simulator zuordnet, Beispiel")](images/maps-app.png#lightbox)

## <a name="settings"></a>Einstellungen

Klicken Sie auf der Symbolleiste auf die Zahnradsymbol öffnet die **Einstellungen** Fenster:

[![](images/settings-sml.png "Einstellungen für iOS-simulator")](images/settings.png#lightbox)

Diese Einstellungen können Sie die Hardwaretastatur aktivieren, wählen Sie einen Speicherort an, die das Gerät sollte Bericht (statische und dynamische Speicherorte werden beide unterstützt), aktivieren Sie Touch ID und Zurücksetzen von Inhalt und alle Einstellungen für den Simulator.

## <a name="other-options"></a>Weitere Optionen

Der Symbolleiste auf die Schaltfläche zeigen, andere Optionen wie Drehung, Schütteln Gesten und neu gestartet wird. Diese dieselben Optionen können als Liste angezeigt werden, mit der rechten Maustaste an einer beliebigen Stelle im Fenster "des Simulators überein":

[![](images/more-sml.png "zusätzliche Einstellungen für iOS-simulator")](images/more.png#lightbox)

## <a name="touchscreen-support"></a>Unterstützung für Touchscreen

Die meisten modernen Windows-Computer über Touchscreens verfügen. Da den remoten iOS-Simulator für Windows Touch-Interaktionen unterstützt, können Sie testen, Ihre app mit der gleichen verkleinern, wischbewegungen und Finger von Multi Touchgesten, die mit physischen iOS-Geräten verwendet werden.

Auf ähnliche Weise behandelt den remoten iOS-Simulator für Windows Windows nastala Chyba vstupu als Eingabe der Apple-Stift.

## <a name="disabling-the-remoted-ios-simulator-for-windows"></a>Deaktivieren den remoten iOS-Simulator für Windows

Um den remoten iOS-Simulator für Windows zu deaktivieren, navigieren Sie zu **Tools > Optionen > Xamarin > iOS-Einstellungen** , und deaktivieren Sie **Simulator Remote auf Windows**.

[![](images/options-sml.png "Kontrollkästchen, um den Simulator verwenden.")](images/options.png#lightbox)

Mit dieser Option deaktiviert ist, öffnet Debuggen buildhost im iOS-Simulator auf dem verbundenen Mac.
