---
title: Xamarin Player Live-App
description: Bearbeiten und Testen von apps in Echtzeit auf Ihrem IOS- oder Android-Gerät
ms.topic: article
ms.prod: xamarin
ms.assetid: A7EB73C1-38D7-46C5-9AF6-4C571C168BE7
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 05/10/2017
ms.openlocfilehash: d666bb9a7749845265d50b86e46ef78e2036cb69
ms.sourcegitcommit: 17a9cf246a4d33cfa232016992b308df540c8e4f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2018
---
# <a name="xamarin-live-player-app"></a>Xamarin Player Live-App

![Vorschaufunktion](~/media/shared/preview.png)

## <a name="get-the-app"></a>Abrufen der App

# <a name="androidtabandroid"></a>[Android](#tab/android)

Xamarin Live Player steht für Android aus Google Play zur Verfügung:

[ ![Auf Google Play verfügbar](images/google-play-badge.png)](https://play.google.com/store/apps/details?id=com.xamarin.live)

Ist Live Xamarin Player für Android-Geräten ohne Google Play erhältlich [HockeyApp](https://aka.ms/xlp-hockeyapp) Verteilung. Darüber hinaus frühe Vorschau erstellt, für Android aus Google Play direkt installiert werden kann, durch die Teilnahme an der [öffnen-Betaprogramm](https://play.google.com/apps/testing/com.xamarin.live)

# <a name="iostabios"></a>[iOS](#tab/ios)

Es wird empfohlen, Benutzer können die [Xamarin Live Player app _iOS Vorschau_ ](https://aka.ms/liveplayeralpha) genießen Sie schnellen Zugriff auf die neuesten Verbesserungen über TestFlight.

-----

## <a name="using-the-app"></a>Mithilfe der App

Nachdem Sie die app auf Ihrem Telefon installiert haben, führen Sie die [setupanweisungen](~/tools/live-player/install.md) zur Verbindung mit Ihrem Computers. Führen Sie eine der [Beispiel-apps](~/tools/live-player/samples.md) Bezugsquelle arbeiten.

Beim Starten die Live-Player Xamarin-app sieht wie folgt (unter iOS und Android bzw.):

![Live-Player iOS-app-screenshot](player-images/app-iphone-sml.png) ![Live-Player Android-app-screenshot](player-images/app-android-sml.png)

Wenn Sie drücken **-Paar, das Visual Studio**, verwenden Sie die Kamera, um den Barcode auf dem Computer mit scannen:

![Screenshot der Barcodescanner iOS](player-images/scan-iphone-sml.png) ![Screenshot des Android Barcodescanner](player-images/scan-android-sml.png)

Wenn die Verbindung erfolgreich ist, der Code sollte auf dem Gerät fast sofort ausführen (z. B. die [-Rechnerbeispiel](https://developer.xamarin.com/samples/mobile/LivePlayer/BasicCalculator)):

![Rechner-Beispiel-app auf dem Gerät ausgeführt wird](player-images/basic-calculator-iphone-sml.png)

## <a name="options"></a>Optionen

Drücken Sie die Schaltfläche "Information" **(i)** am unteren Rand der app, zeigen die **Optionen** Menü:

[ ![Screenshot der im Menü "Optionen"](player-images/options-sml.png)](player-images/options.png#lightbox)

### <a name="logs"></a>Protokolle

Zeigen Sie die Protokolle, um Probleme zu diagnostizieren.

### <a name="settings"></a>Einstellungen

* Schaltet die Anzeige der kompilierungs-und Laufzeitfehler.
* Versionsinformationen.
* Senden von Feedback.

[ ![Screenshot der Einstellungen](player-images/settings-sml.png)](player-images/settings.png#lightbox)

## <a name="managing-devices"></a>Verwalten von Geräten

Folgen Sie den Anweisungen, um ein Gerät zum ersten Mal eine Verbindung herstellen, [Anforderungen & Setup](~/tools/live-player/install.md). Sie können Koppeln von mehreren Geräten (z. B. eine iOS- und eine Android) und über die IDE zu verwalten.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Wählen Sie in Visual Studio **Tools > Xamarin Live Player > Geräte verwalten...**

![Verwalten von Gerätefenster](player-images/manage-tools-menu-vs.png)

Dieses Fenster können Sie die folgenden Aufgaben ausführen:

- Koppeln Sie ein neues Gerät durch das Scannen des Codes
- Alternativ kombinieren Sie ein Gerät, indem Sie den Code, der auf dem Bildschirm angezeigten eingeben
- Entfernen Sie ein vorhandenes Gerät aus der Liste

Sie können dieses Fenster auch aus der Liste der Geräte zugreifen.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Wählen Sie in Visual Studio für Mac **Tools > Verwalten von Geräten (Xamarin Live-Player)...**

![Verwalten von Gerätefenster](player-images/manage-tools-menu.png)

Dieses Fenster können Sie die folgenden Aufgaben ausführen:

- Koppeln Sie ein neues Gerät durch das Scannen des Codes
- Alternativ kombinieren Sie ein Gerät, indem Sie den Code, der auf dem Bildschirm angezeigten eingeben
- Entfernen Sie ein vorhandenes Gerät aus der Liste

![Verwalten von Gerätefenster](player-images/manage.png)

Sie können dieses Fenster auch aus der Liste der Geräte zugreifen:

![Wählen Sie aus der Geräteliste Live Xamarin Player-Geräte](player-images/manage-device-menu.png)

-----

Wenn Sie alle Probleme finden Sie unter auftreten [Einschränkungen und Problembehandlung](~/tools/live-player/troubleshooting.md).


## <a name="related-links"></a>Verwandte Links

- [Einschränkungen](~/tools/live-player/limitations.md)
- [Problembehandlung](~/tools/live-player/troubleshooting.md)
- [Xamarin Player Live-Beispiele](~/tools/livehttps://developer.xamarin.com/samples.md)
