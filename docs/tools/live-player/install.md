---
title: Xamarin Live Player-Visual-Studio-Konfiguration
description: Dieses Dokument beschreibt, wie Sie mit dem Xamarin Live Player live Änderungen an einer ausgeführten Anwendung vornehmen.
ms.prod: xamarin
ms.assetid: 5DDF9203-8826-4B04-93F5-B8D07EDE3873
author: lobrien
ms.author: laobri
ms.date: 06/13/2019
ms.openlocfilehash: a29a637526c2829b44ae89d505dac37a648dee77
ms.sourcegitcommit: 93b1e2255d59c8ca6674485938f26bd425740dd1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2019
ms.locfileid: "67157742"
---
# <a name="xamarin-live-player-visual-studio-configuration"></a>Xamarin Live Player-Visual-Studio-Konfiguration

![Feature der Vorschauversion](~/media/shared/preview.png)

> [!WARNING]
> Die Xamarin Live Player-Vorschauversion wurde beendet. Die app ist nicht mehr verfügbar. Die folgenden Anweisungen dienen für Kunden, die Sie weiterhin die Vorschau mit Visual Studio 2017 verwenden.

> [!TIP]
> Sie können die [XAML-Vorschau](~/xamarin-forms/xaml/xaml-previewer/index.md) in 2019 für Visual Studio oder Visual Studio für Mac Ihre Entwürfe Bildschirm anzuzeigen, wie Sie sie bearbeiten.

# <a name="visual-studio-2017tabwindows"></a>[Visual Studio 2017](#tab/windows)

## <a name="using-xamarin-live-player"></a>Mithilfe von Xamarin Live Player

Sie müssen die Xamarin Live Player-app bereits auf Ihrem Gerät verfügen. Es ist nicht mehr zum download zur Verfügung.

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

## <a name="using-xamarin-live-player"></a>Mithilfe von Xamarin Live Player

Sie müssen die Xamarin Live Player-app bereits auf Ihrem Gerät verfügen. Es ist nicht mehr zum download zur Verfügung.

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

- [Problembehandlung](~/tools/live-player/troubleshooting.md)
