---
title: Xamarin Player Live-Setup
description: Bearbeiten und Testen von apps in Echtzeit auf Ihrem IOS- oder Android-Gerät
ms.prod: xamarin
ms.assetid: 5DDF9203-8826-4B04-93F5-B8D07EDE3873
author: topgenorth
ms.author: toopge
ms.date: 05/14/2018
ms.openlocfilehash: bbc935c2770fab1a853bb12fba2b7eb0283bb258
ms.sourcegitcommit: b706fbe05344f7201fbe8f82d9dbbceb66060637
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/15/2018
---
# <a name="xamarin-live-player-setup"></a>Xamarin Player Live-Setup

Xamarin-Live-Player können Sie die live Änderungen an Ihrer Anwendung vornehmen und diese Änderungen wiederzugeben Livemigration auf dem Gerät. Der Code ausgeführt wird, in der Live-Player Xamarin-app – keine Notwendigkeit zum Einrichten von Emulatoren oder Kabel verwenden zur Bereitstellung von! Dieser Artikel beschreibt, wie Xamarin Live Player eingerichtet wird.

![Vorschaufunktion](~/media/shared/preview.png)

## <a name="1-get-the-app"></a>1. Abrufen der App

# <a name="androidtabandroid"></a>[Android](#tab/android)

Xamarin Live Player steht für Android aus Google Play zur Verfügung:

[ ![Auf Google Play verfügbar](install-images/google-play-badge.png)](https://play.google.com/store/apps/details?id=com.xamarin.live)

Ist Live Xamarin Player für Android-Geräten ohne Google Play erhältlich [HockeyApp](https://aka.ms/xlp-hockeyapp) Verteilung. Darüber hinaus frühe Vorschau erstellt, für Android aus Google Play direkt installiert werden kann, durch die Teilnahme an der [öffnen-Betaprogramm](https://play.google.com/apps/testing/com.xamarin.live)

# <a name="iostabios"></a>[iOS](#tab/ios)

Es wird empfohlen, Benutzer können iOS-app Xamarin Live Player Vorschau, um schnellen Zugriff auf die neuesten Verbesserungen über TestFlight genießen. Durch den Zugriff auf die Xamarin-Live-Player, sind Sie auf dem Microsoft zustimmen, [Nutzungsbestimmungen](https://www.microsoft.com/en-us/legal/intellectualproperty/copyright/default.aspx) & [Privacy Statement](https://privacy.microsoft.com/en-us/privacystatement). Microsoft kann Ihre Kontaktinformationen verwendet, um Updates und Sonderangeboten zu Xamarin und andere Microsoft-Produkte und Dienste bereitzustellen. Sie können jederzeit kündigen.

Führen Sie die Xamarin-Live-Player iOS Vorschau für den Zugriff auf die [TestFlight Registrierungsinformationen](https://fastring.xamarinliveplayer.com/), wonach Sie erhalten eine e-Mail von TestFlight zum Live-Player Xamarin iOS Preview installieren.

-----

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="2-get-visual-studio-2017"></a>2. Visual Studio-2017 abrufen

Live-Player Xamarin benötigen Sie Folgendes:

- Visual Studio 2017 15.4 oder höher.
- Visual Studio-Computer und einem Gerät in einem WiFi-Netzwerk.

## <a name="3-using-xamarin-live-player-for-the-first-time"></a>3. Xamarin-Live-Player zum ersten Mal zu verwenden.

1. Open **Visual Studio 2017**.
2. Wechseln Sie zu **Extras > Optionen...**  , und wählen Sie die **Xamarin > andere** Registerkarte.
3. Teilstriche **aktivieren Xamarin Live Player**:

  ![Aktivieren Sie das Kontrollkästchen aktivieren Xamarin Live-Player im Fenster "Optionen"](install-images/vs2017-options.png)

2. Erstellen oder öffnen Sie eine Xamarin-Projekt (oder eine [Beispiel](~/tools/live-player/samples.md)).
3. Wählen Sie **Player Live** in der Liste des Geräts:

  ![Geräteliste enthält eine Option für die Xamarin-Live-Player](install-images/devices-empty-windows.png)

  * Wenn Sie bereits ein Gerät verbunden haben, wird es als eine Option verfügbar sein.
  * Andernfalls werden Sie aufgefordert, ein Gerät bei der ersten Verwendung zu koppeln.
4. Drücken Sie die **ausführen** Schaltfläche, oder wählen Sie eine der folgenden Optionen aus der **ausführen** oder Kontextmenü:

  - **Starten ohne Debugging** – Sie können die app und ausführliche Informationen finden Sie die Änderungen, auf dem Gerät auftreten bearbeiten (app wird neu gestartet werden Änderungen und die Datei gespeichert wurde).
  - **Debuggen starten** – Sie können Haltepunkte festlegen und Variablen überprüfen, aber Code kann nicht bearbeitet werden.

  Wählen Sie alternativ **Tools > Xamarin Live Player > Führen Sie aktuellen Ansicht Live**, und Sie können Sie die app und finden Sie die Änderungen, auf dem Gerät auftreten bearbeiten. Die aktuelle Ansicht wird (anstatt die Anwendung Hauptbildschirm) angezeigt.

5. Wenn ein Gerät bereits gekoppelt wurde und die Live-Player Xamarin-app auf dem Gerät ausgeführt wird, wird der Code gerade ausgeführt!

  Ist kein Gerät zugeordnet ist ein QR-Code wird mit Anweisungen angezeigt. ein Gerät gekoppelt werden:

  ![Koppeln Sie einen Gerätefenster](install-images/manage-empty-windows.png)

  Wenn das Gerät nicht erreichbar ist, für die Paarung, ggf. ein Fehler angezeigt.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

## <a name="2-get-visual-studio-for-mac"></a>2. Erwerben von Visual Studio für Mac

Live-Player Xamarin benötigen Sie Folgendes:

- OS X 10.11, MacOS 10.12 oder höher
- Visual Studio für Mac
- Einem Mac, und ein Gerät sich im selben WiFi-Netzwerk

## <a name="3-using-xamarin-live-player-for-the-first-time"></a>3. Xamarin-Live-Player zum ersten Mal zu verwenden.

1. Open **Visual Studio für Mac**.
2. Wechseln Sie zu **Visual Studio > Einstellungen...**  , und wählen Sie die **Projekte > Xamarin Player Live (Vorschau)** Registerkarte.
3. Teilstriche **aktivieren Xamarin Live Player**:

  [![Aktivieren Sie das Kontrollkästchen aktivieren Xamarin Live-Player im Fenster "Optionen"](install-images/vsmac-options-sml.png)](install-images/vsmac-options.png#lightbox)

2. Erstellen oder öffnen Sie eine Xamarin-Projekt (oder eine [Beispiel](~/tools/live-player/samples.md)).
3. Wählen Sie **Player Live** in der Geräteliste.

  ![Geräteliste enthält eine Option für die Xamarin-Live-Player](install-images/devices.png)

  * Wenn Sie bereits ein Gerät verbunden haben, wird es als eine Option verfügbar sein.
  * Andernfalls werden Sie aufgefordert, ein Gerät bei der ersten Verwendung zu koppeln.
  * Wählen Sie **Xamarin Player Live Geräte...**  zum Verwalten der Geräte mit Xamarin-Live-Player verwenden möchten.

4. Drücken Sie die **ausführen** Schaltfläche, oder wählen Sie eine der folgenden Optionen aus der **ausführen** oder Kontextmenü:

  ![Führen Sie die Menüoptionen](install-images/run-menu.png)

  - **Starten ohne Debugging** – Sie können die app und ausführliche Informationen finden Sie die Änderungen, auf dem Gerät auftreten bearbeiten (app wird neu gestartet werden Änderungen und die Datei gespeichert wurde).
  - **Debuggen starten** – Sie können Haltepunkte festlegen und Variablen überprüfen, aber Code kann nicht bearbeitet werden.
  - **Aktuelle Ansicht ausführen Live** – Sie können die app und ausführliche Informationen finden Sie die Änderungen, auf dem Gerät auftreten bearbeiten. Die aktuelle Ansicht wird (anstatt die Anwendung Hauptbildschirm) angezeigt.

5. Wenn ein Gerät bereits gekoppelt wurde und die Live-Player Xamarin-app auf dem Gerät ausgeführt wird, wird der Code gerade ausgeführt!

  Wenn kein Gerät verbunden ist, wird mit Anweisungen zum Koppeln Sie ein Gerät ein QR-Code angezeigt:

  ![Koppeln Sie einen Gerätefenster](install-images/manage-empty.png)

  Wenn das Gerät nicht erreichbar ist, für die Paarung, wird ein Fehler angezeigt:

  ![Keine Verbindung mit Gerät-Fehlermeldung](install-images/error-cannot-connect.png)


-----

Wenn Sie alle Probleme auftreten, oder können keine Verbindung hergestellt haben, finden Sie unter [Einschränkungen und Problembehandlung](~/tools/live-player/troubleshooting.md).


## <a name="related-links"></a>Verwandte Links

- [Einschränkungen](~/tools/live-player/limitations.md)
- [Problembehandlung](~/tools/live-player/troubleshooting.md)
- [Xamarin Player Live-Beispiele](~/tools/live-player/samples.md)
