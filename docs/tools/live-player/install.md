---
title: Xamarin Live Player der Visual Studio-Konfiguration
description: In diesem Dokument wird beschrieben, wie Sie die Xamarin Live Player verwenden, um Live-Änderungen an einer laufenden Anwendung vorzunehmen.
ms.prod: xamarin
ms.assetid: 5DDF9203-8826-4B04-93F5-B8D07EDE3873
author: conceptdev
ms.author: crdun
ms.date: 06/13/2019
ms.openlocfilehash: 94f1d36bf97aab7eabb57e6f2712c9850b390ab1
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290489"
---
# <a name="xamarin-live-player-visual-studio-configuration"></a>Xamarin Live Player der Visual Studio-Konfiguration

![Feature der Vorschauversion](~/media/shared/preview.png)

> [!WARNING]
> Die Xamarin Live Player Vorschau wurde beendet. Die APP ist nicht mehr verfügbar. Die folgenden Anweisungen werden für Kunden bereitgestellt, die die Vorschauversion mit Visual Studio 2017 weiterhin verwenden.

> [!TIP]
> Sie können den [XAML-Previewer](~/xamarin-forms/xaml/xaml-previewer/index.md) in Visual Studio 2019 oder Visual Studio für Mac verwenden, um die Bildschirm Entwürfe während der Bearbeitung anzuzeigen.

# <a name="visual-studio-2017tabwindows"></a>[Visual Studio 2017](#tab/windows)

## <a name="using-xamarin-live-player"></a>Verwenden von Xamarin Live Player

Sie müssen bereits über die Xamarin Live Player-App auf Ihrem Gerät verfügen. Der Download ist nicht mehr möglich.

1. Öffnen Sie **Visual Studio 2017**.
2. Wechseln Sie zu Extras **> Optionen...** , und wählen Sie die Registerkarte **xamarin > andere** aus.
3. **Xamarin Live Player Tick aktivieren**:

    ![Aktivieren Sie das Kontrollkästchen Xamarin Live Player aktivieren im Fenster "Optionen".](install-images/vs2017-options.png)

4. Erstellen oder öffnen Sie ein xamarin-Projekt (oder ein [Beispiel](~/tools/live-player/samples.md)).
5. Wählen Sie **Live Player** in der Geräteliste aus:

    ![Die Geräteliste enthält eine Xamarin Live Player-Option.](install-images/devices-empty-windows.png)

    - Wenn Sie bereits ein Gerät gekoppelt haben, ist es als Option verfügbar.
    - Andernfalls werden Sie aufgefordert, bei Bedarf ein Gerät zu koppeln.

6. Klicken Sie auf die Schaltfläche **Ausführen** , oder wählen Sie im Menü **Ausführen** oder Rechtsklick eine der folgenden Optionen aus:

    - **Starten ohne Debugging** – Sie können die APP bearbeiten und sehen, dass die Änderungen auf dem Gerät auftreten (die APP wird neu gestartet, wenn Änderungen vorgenommen und die Datei gespeichert wird).
    - **Debuggen starten** – Sie können Breakpoints festlegen und Variablen überprüfen, aber der Code kann nicht bearbeitet werden.

    Alternativ können Sie Extras **> Xamarin Live Player > Aktuelle Ansicht Live ausführen**auswählen, um die APP zu bearbeiten und die Änderungen auf dem Gerät anzuzeigen. Die aktuelle Ansicht wird angezeigt (anstelle des Hauptbildschirms der Anwendung).

7. Wenn ein Gerät bereits gekoppelt ist und die Xamarin Live Player-App auf dem Gerät ausgeführt wird, wird der Code direkt ausgeführt.

    Wenn kein Gerät gekoppelt ist, wird ein QR-Code mit Anweisungen zum Koppeln eines Geräts angezeigt:

    ![Koppeln eines Geräte Fensters](install-images/manage-empty-windows.png)

    Wenn für das Gerät keine Verbindung hergestellt werden kann, wird möglicherweise ein Fehler angezeigt.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

## <a name="using-xamarin-live-player"></a>Verwenden von Xamarin Live Player

Sie müssen bereits über die Xamarin Live Player-App auf Ihrem Gerät verfügen. Der Download ist nicht mehr möglich.

1. Öffnen Sie **Visual Studio für Mac**.
2. Wechseln Sie zu **Visual Studio > Einstellungen...** , und wählen Sie die Registerkarte **Projekte > Xamarin Live Player (Vorschau)** aus.
3. **Xamarin Live Player Tick aktivieren**:

    [![Aktivieren Sie das Kontrollkästchen Xamarin Live Player aktivieren im Fenster "Optionen".](install-images/vsmac-options-sml.png)](install-images/vsmac-options.png#lightbox)

4. Erstellen oder öffnen Sie ein xamarin-Projekt (oder ein [Beispiel](~/tools/live-player/samples.md)).
5. Wählen Sie **Live Player** in der Geräteliste aus.

    ![Die Geräteliste enthält eine Xamarin Live Player-Option.](install-images/devices.png)

    - Wenn Sie bereits ein Gerät gekoppelt haben, ist es als Option verfügbar.
    - Andernfalls werden Sie aufgefordert, bei Bedarf ein Gerät zu koppeln.
    - Wählen Sie **Xamarin Live Player Geräte...** aus, um die Geräte zu verwalten, die Sie mit Xamarin Live Player verwenden möchten.

6. Klicken Sie auf die Schaltfläche **Ausführen** , oder wählen Sie im Menü **Ausführen** oder Rechtsklick eine der folgenden Optionen aus:

    ![Menü Optionen ausführen](install-images/run-menu.png)

    - **Starten ohne Debugging** – Sie können die APP bearbeiten und sehen, dass die Änderungen auf dem Gerät auftreten (die APP wird neu gestartet, wenn Änderungen vorgenommen und die Datei gespeichert wird).
    - **Debuggen starten** – Sie können Breakpoints festlegen und Variablen überprüfen, aber der Code kann nicht bearbeitet werden.
    - **Aktuelle Ansicht Live ausführen** – Sie können die APP bearbeiten und sehen, dass die Änderungen auf dem Gerät erfolgen. Die aktuelle Ansicht wird angezeigt (anstelle des Hauptbildschirms der Anwendung).

7. Wenn ein Gerät bereits gekoppelt ist und die Xamarin Live Player-App auf dem Gerät ausgeführt wird, wird der Code direkt ausgeführt.

    Wenn kein Gerät gekoppelt ist, wird ein QR-Code mit Anweisungen zum Koppeln eines Geräts angezeigt:

    ![Koppeln eines Geräte Fensters](install-images/manage-empty.png)

    Wenn für das Gerät keine Verbindung hergestellt werden kann, wird ein Fehler angezeigt:

    ![Verbindung mit Geräte Fehlermeldung nicht möglich](install-images/error-cannot-connect.png)

-----

Wenn Probleme auftreten oder keine Verbindung hergestellt werden kann, finden Sie weitere Informationen unter [Einschränkungen und Problem](~/tools/live-player/troubleshooting.md)Behandlung.

## <a name="related-links"></a>Verwandte Links

- [Problembehandlung](~/tools/live-player/troubleshooting.md)
