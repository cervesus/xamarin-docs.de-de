---
title: iOS-Remotesimulator für Windows
description: Mit dem remoten IOS-Simulator für Windows können Sie Ihre apps in einem IOS-Simulator testen, der in Windows zusammen mit Visual Studio 2019 angezeigt wird.
ms.prod: xamarin
ms.assetid: 63c50190-7e54-4140-a30d-1a0e577c47d7
author: davidortinau
ms.author: daortin
ms.date: 04/26/2019
ms.openlocfilehash: d5898f9c6ee30eb1f12bf6480b93a609e762e6ea
ms.sourcegitcommit: ec62e2624295aa502ec35ac782031d61d61c3aaa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2020
ms.locfileid: "75886592"
---
# <a name="remoted-ios-simulator-for-windows"></a>iOS-Remotesimulator für Windows

Mit dem remoten IOS-Simulator für Windows können Sie Ihre apps in einem IOS-Simulator testen, der in Windows zusammen mit Visual Studio 2019 und Visual Studio 2017 angezeigt wird.

[![IOS-Simulator unter Windows](images/hero-sml.png "IOS-Simulator unter Windows")](images/hero.png#lightbox)

## <a name="getting-started"></a>Erste Schritte

Der Remote IOS-Simulator für Windows wird automatisch als Teil von xamarin in Visual Studio 2019 und Visual Studio 2017 installiert. Führen Sie die folgenden Schritte aus, um Sie zu verwenden:

1. Koppeln [Sie Visual 2019 mit einem Mac-buildhost](~/ios/get-started/installation/windows/connecting-to-mac/index.md).
2. Starten Sie in Visual Studio das Debuggen eines IOS-oder tvos-Projekts. Der Remote IOS-Simulator für Windows wird auf Ihrem Windows-Computer angezeigt.

Sehen Sie sich [dieses Video](deploy.md) mit einer Schritt-für-Schritt-Anleitung an.

## <a name="simulator-window"></a>Simulatorfenster

Die Symbolleiste am oberen Rand des simulatorfensters enthält eine Reihe nützlicher Schaltflächen:

- **Startseite** – simuliert die Start Schaltfläche auf einem IOS-Gerät.
- **Lock** – sperrt den Simulator (Schwenk zum Entsperren).
- **Screenshot** – speichert einen Screenshot des Simulators (gespeichert in **pictures\xamarin\ios Simulator\\** ).
- [**Einstellungen**](#settings) – zeigt Tastatur, Speicherort und andere Einstellungen an.
- [**Weitere Optionen**](#other-options) – führt verschiedene simulatoroptionen aus, z. b. Drehung, Shake Gesten und touchid.

    [![Beispiel für IOS-simulatormaps](images/maps-app-sml.png "Beispiel für IOS-simulatormaps")](images/maps-app.png#lightbox)

## <a name="settings"></a>Einstellungen

Durch Klicken auf das Zahnrad Symbol der Symbolleiste wird das Fenster **Einstellungen** geöffnet:

[![IOS-simulatoreinstellungen](images/settings-sml.png "IOS-simulatoreinstellungen")](images/settings.png#lightbox)

Mit diesen Einstellungen können Sie die Hardware Tastatur aktivieren, einen Speicherort auswählen, den das Gerät melden soll (statische und verschiebenden Speicherorte werden unterstützt), Fingereingabe-ID aktivieren und den Inhalt und die Einstellungen für den Simulator zurücksetzen.

## <a name="other-options"></a>Weitere Optionen

Die Schaltfläche mit den Auslassungs Punkten der Symbolleiste zeigt andere Optionen an, z. b. Drehung, Shake Gesten und Neustarts. Diese Optionen können als Liste angezeigt werden, indem Sie mit der rechten Maustaste auf eine beliebige Stelle im Fenster des Simulators klicken:

[![zusätzliche Einstellungen für den IOS-Simulator](images/more-sml.png "zusätzliche Einstellungen für den IOS-Simulator")](images/more.png#lightbox)

## <a name="touchscreen-support"></a>Touchscreen-Unterstützung

Die meisten modernen Windows-Computer verfügen über Touchscreens. Da der Remote IOS-Simulator für Windows touchinteraktionen unterstützt, können Sie Ihre APP mit den gleichen Tastenkombinationen, flpe und multifingertouch-Gesten testen, die Sie für physische IOS-Geräte verwenden.

Entsprechend behandelt der Remote IOS-Simulator für Windows die Windows-Tablettstifteingabe als Apple-Stift Eingabe.

## <a name="sound-handling"></a>Sound Behandlung

Die vom Simulator gespielten Sounds stammen von den Referenten des Host-Macs.
IOS-Sounds werden auf dem Windows-Computer nicht gehört.

## <a name="disabling-the-remoted-ios-simulator-for-windows"></a>Deaktivieren des remoten IOS-Simulators für Windows

Um den remoten IOS-Simulator für Windows zu deaktivieren, navigieren Sie zu Extras **> Optionen > xamarin > IOS-Einstellungen** , und deaktivieren Sie **Remote Simulator für Windows**.

[![Kontrollkästchen zur Verwendung des Simulators](images/options-sml.png "Kontrollkästchen zur Verwendung des Simulators")](images/options.png#lightbox)

Wenn diese Option deaktiviert ist, öffnet das Debuggen den IOS-Simulator auf dem verbundenen Mac-buildhost.

## <a name="troubleshooting"></a>Problembehandlung

Wenn Probleme mit dem remoten IOS-Simulator auftreten, können Sie die Protokolle an den folgenden Speicherorten anzeigen:

- **Mac** – `~/Library/Logs/Xamarin/Simulator.Server`
- **Windows** : `%LOCALAPPDATA%\Xamarin\Logs\Xamarin.Simulator`

Wenn Sie [ein Problem in Visual Studio melden](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio), ist es möglicherweise hilfreich, diese Protokolle anzufügen (es gibt Optionen, um Uploads privat zu halten).
