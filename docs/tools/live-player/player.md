---
title: Xamarin Player Live-App
description: "Bearbeiten und Testen von apps in Echtzeit auf Ihrem IOS- oder Android-Gerät"
ms.topic: article
ms.prod: xamarin
ms.assetid: A7EB73C1-38D7-46C5-9AF6-4C571C168BE7
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 05/10/2017
ms.openlocfilehash: 6ae0439566387e22d939ede83e6b4cb5c1a5a8fe
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="xamarin-live-player-app"></a>Xamarin Player Live-App

![Vorschaufunktion](~/media/shared/preview.png)

## <a name="get-the-app"></a>Abrufen der App

Xamarin Live Player steht im IOS App Store und Google Play zur Verfügung:

[ ![IOS App Store herunterladen](player-images/app-store-badge.svg)](https://itunes.apple.com/us/app/xamarin-live-player/id1228841832?ls=1&mt=8) [ ![verfügbar, die auf Google Play](player-images/google-play-badge.png)](https://play.google.com/store/apps/details?id=com.xamarin.live)



## <a name="using-the-app"></a>Mithilfe der App

Nachdem Sie die app auf Ihrem Telefon installiert haben, führen Sie die [setupanweisungen](~/tools/live-player/install.md) zur Verbindung mit Ihrem Computers. Führen Sie eine der [Beispiel-apps](~/tools/live-player/samples.md) Bezugsquelle arbeiten.

Beim Starten die Live-Player Xamarin-app sieht wie folgt (unter iOS und Android bzw.):

![Live-Player iOS-app-screenshot](player-images/app-iphone-sml.png) ![Live-Player Android-app-screenshot](player-images/app-android-sml.png)

Wenn Sie drücken **-Paar, das Visual Studio**, verwenden Sie die Kamera, um den Barcode auf dem Computer mit scannen:

![Screenshot der Barcodescanner iOS](player-images/scan-iphone-sml.png) ![Screenshot des Android Barcodescanner](player-images/scan-android-sml.png)

Wenn die Verbindung erfolgreich ist, sollte der Code auf dem Gerät fast sofort (z. B.-Rechnerbeispiel) ausgeführt werden:

![Rechner-Beispiel-app auf dem Gerät ausgeführt wird](player-images/basic-calculator-iphone-sml.png)

## <a name="options"></a>Optionen

Drücken Sie die Schaltfläche "Information" **(i)** am unteren Rand der app, zeigen die **Optionen** Menü:

![Screenshot der im Menü "Optionen"](player-images/options.png)

### <a name="logs"></a>Protokolle

Zeigen Sie die Protokolle, um Probleme zu diagnostizieren.

### <a name="settings"></a>Einstellungen

* Schaltet die Anzeige der kompilierungs-und Laufzeitfehler.
* Versionsinformationen.
* Senden von Feedback.

![Screenshot der Einstellungen](player-images/settings.png)

## <a name="managing-devices"></a>Verwalten von Geräten

Folgen Sie den Anweisungen, um ein Gerät zum ersten Mal eine Verbindung herstellen, [Anforderungen & Setup](~/tools/live-player/install.md). Sie können Koppeln von mehreren Geräten (z. B. eine iOS- und eine Android) und über die IDE zu verwalten.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Wählen Sie in Visual Studio **Tools > Xamarin Live Player > Geräte verwalten...**

![Verwalten von Gerätefenster](player-images/manage-tools-menu-vs.png)

Dieses Fenster können Sie die folgenden Aufgaben ausführen:

- Koppeln Sie ein neues Gerät durch das Scannen des Codes
- Alternativ kombinieren Sie ein Gerät, indem Sie den Code, der auf dem Bildschirm angezeigten eingeben
- Entfernen Sie ein vorhandenes Gerät aus der Liste

Sie können dieses Fenster auch aus der Liste der Geräte zugreifen.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

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
