---
title: Die Installation von Xamarin Live Player
description: Dieses Dokument beschreibt, wie Sie Xamarin Live Player einrichten und verwenden, um live-Änderungen an einer ausgeführten Anwendung vornehmen.
ms.prod: xamarin
ms.assetid: 5DDF9203-8826-4B04-93F5-B8D07EDE3873
author: lobrien
ms.author: laobri
ms.date: 08/08/2018
ms.openlocfilehash: dd987b6d1a6db8e27544ddd95cdc219bb5f783b5
ms.sourcegitcommit: 7eed80186e23e6aff3ddbbf7ce5cd1fa20af1365
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2018
ms.locfileid: "51526649"
---
# <a name="xamarin-live-player-setup"></a>Die Installation von Xamarin Live Player

Xamarin Live Player können Sie die live-Änderungen an Ihrer app vornehmen und diese Änderungen wiederzugeben live auf dem Gerät. Ihr Code ausgeführt wird, in der Xamarin Live Player-app – müssen Sie keine Emulatoren einrichten oder mit Kabel bereitstellen! Dieser Artikel beschreibt, wie Sie Xamarin Live Player einrichten.

![Feature (Vorschau)](~/media/shared/preview.png)

## <a name="1-get-the-android-app"></a>1. Abrufen der Android-Apps

Xamarin Live Player steht für Android aus Google Play zur Verfügung:

[ ![Auf Google Play verfügbar](install-images/google-play-badge.png)](https://play.google.com/store/apps/details?id=com.xamarin.live)

Für Android ohne Google Play-Geräte mit dem Xamarin Live Player steht über [HockeyApp](https://aka.ms/xlp-hockeyapp) Verteilung. Darüber hinaus frühe Preview-builds für Android direkt aus dem Google Play installiert werden kann, indem Sie die Entscheidung für die [open Beta-Programm](https://play.google.com/apps/testing/com.xamarin.live)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="2-get-visual-studio-2017"></a>2. Abrufen von Visual Studio 2017

Xamarin Live Player sind erforderlich:

- Visual Studio 2017, Version 15.4 oder höher.
- Visual Studio-Computer und ein Gerät sich im gleichen WLAN-Netzwerk.

## <a name="3-using-xamarin-live-player-for-the-first-time"></a>3. Verwenden zum ersten Mal Xamarin Live Player

1. Open **Visual Studio 2017**.
2. Wechseln Sie zu **Tools > Optionen...**  , und wählen Sie die **Xamarin > andere** Registerkarte.
3. Teilstriche **Xamarin Live Player aktivieren**:

    ![Aktivieren Sie das Kontrollkästchen "Xamarin Live Player aktivieren" in das Fenster "Optionen"](install-images/vs2017-options.png)

4. Erstellen oder öffnen Sie ein Xamarin-Projekt (oder ein [Beispiel](~/tools/live-player/samples.md)).
5. Wählen Sie **Live Player** in der Geräteliste:

    ![Geräteliste enthält eine Option für die Xamarin Live Player](install-images/devices-empty-windows.png)

    - Wenn Sie bereits ein Gerät verbunden haben, werden sie als Option verfügbar.
    - Andernfalls werden Sie aufgefordert, ein Gerät im Bedarfsfall zu koppeln.

6. Drücken Sie die **ausführen** Schaltfläche, oder wählen Sie eine der folgenden Optionen aus der **ausführen** oder mit der rechten Maustaste im Menü:

    - **Starten ohne Debugging** – Sie können die app und die Änderungen, auf dem Gerät auftreten bearbeiten (app wird gestartet, wie Änderungen vorgenommen werden, und die Datei gespeichert).
    - **Debuggen starten** – Sie können Haltepunkte festlegen und untersuchen Sie Variablen, aber Code kann nicht bearbeitet werden.

    Wählen Sie alternativ **Tools > Xamarin Live Player > aktuelle Ansicht der Liveausführung**, mit dem Sie die app und die Änderungen, auf dem Gerät auftreten bearbeiten. Die aktuelle Ansicht wird (anstelle von Hauptbildschirm von der Anwendung) angezeigt.

7. Wenn ein Gerät wurde bereits gekoppelt, und die Xamarin Live Player-app auf dem Gerät ausgeführt wird, wird der Code sofort ausgeführt.

    Wenn kein Gerät verbunden ist, wird ein QR-Code mit Anweisungen dazu, ein Gerät koppeln angezeigt:

    ![Ein Gerätefenster koppeln](install-images/manage-empty-windows.png)

    Wenn das Gerät nicht erreichbar ist, für die Paarung, möglicherweise ein Fehler angezeigt.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

## <a name="2-get-visual-studio-for-mac"></a>2. Visual Studio für Mac herunterladen

Xamarin Live Player sind erforderlich:

- OS X 10.11, MacOS Sierra 10.12 oder höher
- Visual Studio für Mac
- Einen Mac und ein Gerät sich im gleichen WLAN-Netzwerk

## <a name="3-using-xamarin-live-player-for-the-first-time"></a>3. Verwenden zum ersten Mal Xamarin Live Player

1. Open **Visual Studio für Mac**.
2. Wechseln Sie zu **Visual Studio > Einstellungen...**  , und wählen Sie die **Projekte > Xamarin Live Player (Vorschauversion)** Registerkarte.
3. Teilstriche **Xamarin Live Player aktivieren**:

    [![Aktivieren Sie das Kontrollkästchen "Xamarin Live Player aktivieren" in das Fenster "Optionen"](install-images/vsmac-options-sml.png)](install-images/vsmac-options.png#lightbox)

4. Erstellen oder öffnen Sie ein Xamarin-Projekt (oder ein [Beispiel](~/tools/live-player/samples.md)).
5. Wählen Sie **Live Player** in der Geräteliste.

    ![Geräteliste enthält eine Option für die Xamarin Live Player](install-images/devices.png)

    - Wenn Sie bereits ein Gerät verbunden haben, werden sie als Option verfügbar.
    - Andernfalls werden Sie aufgefordert, ein Gerät im Bedarfsfall zu koppeln.
    - Wählen Sie **Xamarin Live Player-Geräte...**  zum Verwalten der Geräte mit Xamarin Live Player verwenden möchten.

6. Drücken Sie die **ausführen** Schaltfläche, oder wählen Sie eine der folgenden Optionen aus der **ausführen** oder mit der rechten Maustaste im Menü:

    ![Führen Sie die Optionen der Menüs](install-images/run-menu.png)

    - **Starten ohne Debugging** – Sie können die app und die Änderungen, auf dem Gerät auftreten bearbeiten (app wird gestartet, wie Änderungen vorgenommen werden, und die Datei gespeichert).
    - **Debuggen starten** – Sie können Haltepunkte festlegen und untersuchen Sie Variablen, aber Code kann nicht bearbeitet werden.
    - **Führen Sie aktuellen Ansicht der liveausführung** – Sie können die app und die Änderungen, auf dem Gerät auftreten bearbeiten. Die aktuelle Ansicht wird (anstelle von Hauptbildschirm von der Anwendung) angezeigt.

7. Wenn ein Gerät wurde bereits gekoppelt, und die Xamarin Live Player-app auf dem Gerät ausgeführt wird, wird der Code sofort ausgeführt.

    Wenn kein Gerät verbunden ist, wird ein QR-Code mit Anweisungen dazu, ein Gerät koppeln angezeigt:

    ![Ein Gerätefenster koppeln](install-images/manage-empty.png)

    Wenn das Gerät nicht erreichbar ist, für die Paarung, wird ein Fehler angezeigt:

    ![Keine Verbindung mit Gerät Fehlermeldung](install-images/error-cannot-connect.png)

-----

Wenn Sie Probleme auftreten oder keine Verbindung herstellen können, finden Sie unter [Einschränkungen und Problembehandlung](~/tools/live-player/troubleshooting.md).

## <a name="related-links"></a>Verwandte Links

- [Einschränkungen](~/tools/live-player/limitations.md)
- [Problembehandlung](~/tools/live-player/troubleshooting.md)
- [Xamarin Live Player-Beispiele](~/tools/live-player/samples.md)
