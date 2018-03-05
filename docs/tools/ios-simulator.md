---
title: "Remote-iOS-Simulator (für Windows)"
description: "IOS-apps testen und Debuggen vollständig in Visual Studio unter Windows"
ms.topic: article
ms.prod: xamarin
ms.assetid: 63c50190-7e54-4140-a30d-1a0e577c47d7
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 04/07/2017
ms.openlocfilehash: 707ba5874c939fbd25f4e25a7cefd3dc5fc75131
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="remoted-ios-simulator-for-windows"></a>Remote-iOS-Simulator (für Windows)

_IOS-apps testen und Debuggen vollständig in Visual Studio unter Windows_

[ ![](ios-simulator-images/hero-sml.png "iOS-Simulator unter Windows")](ios-simulator-images/hero.png)

## <a name="download-and-install"></a>Herunterladen und installieren

Herunterladen der [Installer](https://dl.xamarin.com/xamarin-simulator/Xamarin.Simulator.Installer.msi) und auf Ihrem Windows-Computer installieren. Visual Studio-Tools für Xamarin sollte bereits installiert sein.

> [!NOTE]
> Mit einem remote-iOS-Simulator wird Visual Studio einem vernetzten Mac mit Xamarin installiert erfordert.

## <a name="getting-started"></a>Erste Schritte

So verwenden Sie den remote-iOS-simulator

1. Stellen Sie sicher, dass Visual Studio auf Ihrem Mac verbunden ist, mindestens einmal, bevor Sie den remote-iOS-Simulator starten.
2. Sicherzustellen, dass eine app für iOS oder tvos. außerdem wurden die **Startprojekt** und mit dem Debuggen beginnen.

Können Sie den remote-iOS-Simulator aus deaktivieren **Tools > Optionen > Xamarin > für den iOS** durch das Kontrollkästchen für **Remote-Simulator für Windows** hier dargestellt:

[ ![](ios-simulator-images/options-sml.png "Kontrollkästchen, um den Simulator verwenden.")](ios-simulator-images/options.png)

IOS-Simulator wird auf dem verbundenen Mac-Computer geöffnet. Überprüfen Sie diese Option, um den remote-iOS-Simulator wieder zu aktivieren.

## <a name="features"></a>Features

Der remote-iOS-Simulator bietet Ihnen eine Möglichkeit zum Testen und Debuggen von iOS-apps im Simulator vollständig von Visual Studio unter Windows.

### <a name="simulator-window"></a>Simulatorfenster

Die Symbolleiste des Fensters enthält eine Reihe von Schaltflächen für die Interaktion mit den Simulator:

- **Home** – Schaltfläche "Start" auf dem Gerät simuliert.
- **Sperre** – Sperren Sie den Simulator (Sie können zum Entsperren streichen).
- **Bildschirmabbildung von** – einen Screenshot der Simulator auf dem Datenträger gespeichert.
- [**Einstellungen** ](#settings) – konfigurieren Sie die Tastatur und Speicherort.
 - Andere [ **Optionen** ](#options) – eine Vielzahl von Simulator Optionen stehen zur Verfügung, z. B. drehen, Shake, oder rufen Sie andere Zustände im Simulator. Wenn einige Optionen verdeckt werden, können sie aus dem Symbol "Ellipse" zugegriffen werden, die in der Symbolleiste oder mit der rechten Maustaste auf das Fenster angezeigt wird.

    [ ![](ios-simulator-images/maps-app-sml.png "iOS-Simulator zuordnet, Beispiel")](ios-simulator-images/maps-app.png)


### <a name="settings"></a>Einstellungen

Das Zahnradsymbol "" öffnet die **Einstellungen** Fenster:

[ ![](ios-simulator-images/settings-sml.png "Einstellungen für iOS-simulator")](ios-simulator-images/settings.png)

Dadurch können Sie im Simulator die Hardwaretastatur aktivieren, und wählen der Speicherort auf dem Gerät (z. B. einen statischen Speicherort oder andere gleitenden Standortoptionen) gemeldet wird.



### <a name="other-options"></a>Weitere Optionen

Mit der rechten Sie Maustaste im simulatorfenster, um die im Simulator, wie Drehung, eine Geste Shake auslösen und Neustarten des Simulators verfügbaren Optionen anzuzeigen:

[ ![](ios-simulator-images/more-sml.png "zusätzliche Einstellungen für iOS-simulator")](ios-simulator-images/more.png)

### <a name="touchscreen-support"></a>Touchscreen-Unterstützung

Die meisten modernen Windows-Computer haben Touchscreens, und der remote-iOS-Simulator können Sie die simulatorfenster zum Testen von Benutzerinteraktionen in Ihre iOS-app zu berühren.

Dies schließt Pinch Streifen und mehrere Finger Fingereingabe - Dinge, die zuvor nur problemlos auf physischen Geräten getestet werden können.

Stift-Unterstützung in Windows wird auch in Apple Zeichenstift Eingabe im Simulator übersetzt.

<!--
<a name="knownissues" />

# Known Issues

 - Apple Watch devices may show in the Visual Studio device list, but are not yet supported.
 - Launching in **Release** mode may also start Apple’s simulator on the networked Mac.
 - Closing the remote iOS Simulator on Windows will not immediately stop debugging in Visual Studio. Stop debugging manually from the menu or the red button.
 - Opening too many different simulators simultaneously will produce unexpected results.
 - Exception of type `Foundation.NSErrorException` may be thrown while launching Simulators. Workaround is to kill csproxy (server process) on the Mac host and re-deploy to the simulator.
 - Performance may be slower when using Xcode 8
-->
