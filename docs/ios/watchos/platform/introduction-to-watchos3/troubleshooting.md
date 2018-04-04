---
title: WatchOS 3 Problembehandlung
description: Dieser Artikel enthält einige Tipps zur Problembehandlung für die Arbeit mit WatchOS 3 in Xamarin Apple Watch-apps.
ms.prod: xamarin
ms.assetid: 5911D898-0E23-40CC-9F3C-5F61B4D50ADC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 159c6a6dadcaa325abc7fd747abc9b2ba2f26a9c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="watchos-3-troubleshooting"></a>WatchOS 3 Problembehandlung

_Dieser Artikel enthält einige Tipps zur Problembehandlung für die Arbeit mit WatchOS 3 in Xamarin Apple Watch-apps._

Diese Seite enthält einige bekannte Probleme, die bei Verwendung von WatchOS 3 mit Xamarin und die Lösung für diese Probleme auftreten können.

## <a name="activities"></a>Aktivitäten

Für die Aktivität freigeben, damit er ordnungsgemäß funktioniert müssen alle gepaarten Apple Überwachungen WatchOS 3 ausgeführt werden.

Bekannte Probleme:

- Antworten auf eine Aktivität Freigabe Benachrichtigung manchmal ein Fehler auftritt.
- Antworten auf eine Aktivität Freigabe Benachrichtigung mit einer Meldung schlägt möglicherweise fehl.
- Kontextabhängige Text über eine Aktivität Freigabe benachrichtigungsmeldung ist fehlerhaft.


## <a name="apple-pay"></a>Apple Pay

Bekannte Probleme:

- Wenn Sie ein Ablaufdatum falsch oder CW Code für eine neue Zahlung Sorgfalt Apple Pay,, beim eingegeben wird **Weiter** der ausgeführte Prozess abstürzt.
- In-App Apple Pay Käufe, erfordern eine PIN-Nummer stürzt möglicherweise ab.



## <a name="auto-mac-unlock"></a>Automatische Mac entsperren

Mithilfe von WatchOS 3 Beta 2 (oder höher) und MacOS Sierra Beta 2 (oder höher), wenn zweistufige Authentifizierung für das Konto des Benutzers iCloud aktiviert ist, können sie ihre Apple Watch-automatisch entsperren Sie ihren Mac.



## <a name="background-refresh"></a>Hintergrund aktualisieren

Verletzung der Systemressourcen führt zu einem Absturz des WatchOS 3-app mit den folgenden Ausnahmecodes:

- **0xc51bad01** – die app verwendet zu viel CPU-Zeit.
- **0xc51bad02** – die app verwendet zu viel Zeit Wand.
- **0xc51bad03** – die app verfügte nicht über genügend Common Language Runtime zum Durchführen der aktuellen Aufgabe.



## <a name="clock"></a>Uhr

Komplikationen von neu installierten Apple Watch-apps möglicherweise als leer angezeigt. Starten Sie neu Apple Watch, um dieses Problem zu beheben.


## <a name="connectivity"></a>Konnektivität

Bekannte Probleme:

- WatchOS fordert den Benutzer zur Zugriffsberechtigung für geschützte Benutzerdaten auf der Apple Watch nicht. Erteilen des Zugriffs auf die iPhone-app, bevor Sie Daten in die Watch-app verwenden.
- Der Apple Watch können einen versetzt alle WatchConnectivity Übertragungen, in dem ein Fehler auftritt, Neustart der Apple Watch diesen Fehler beheben.


## <a name="notifications"></a>Benachrichtigungen

Wenn eine Anlage Medien zu groß ist, wird er auf iPhone des Benutzers, aber nicht auf ihrer Apple Watch angezeigt.


## <a name="nsurlconnection"></a>NSURLConnection

Alle `NSURLConnection` Verbindungen mit älteren TLS-Protokolle schlägt fehl. Für alle SSL/TLS-Verbindungen wird der symmetrische RC4-Verschlüsselungsverfahren jetzt standardmäßig deaktiviert. Darüber hinaus wird der sichere Transport-API unterstützt nicht mehr SSLv3, und es wird empfohlen, dass die Verwendung von SHA-1 und 3DES Kryptografie so bald wie möglich mit der app zu beenden.

Zum Zeitpunkt der WatchOS 3 ist SSL/TLS-Verbindungen Sicherheit von Apple erzwungen wird. Betroffene Dienste und Anwendungen sollten aktualisiert Webserver in den neuesten Versionen der TLS-Protokoll verwenden.


## <a name="nsurlsession"></a>NSURLSession

Ab WatchOS 3 die `HTTPBodyStream` Eigenschaft von der `NSMutableURLRequest` "Class" muss festgelegt werden, in eine geöffnete Stream seit `NSURLConnection` und `NSURLSession` nun ausschließlich diese Anforderung erzwingen.


## <a name="privacy"></a>Datenschutz

Bekannte Probleme:

Bei der Arbeit mit `https://` URLs beide `NSURLSession` und `NSURLConnection` unterstützt nicht mehr während des TLS-Handshake RC4-Verschlüsselungssammlungen. Einer der folgenden Fehlercodes kann folgendermaßen generiert werden:

- **-1200 oder-98** - `NSURLErrorSecurityConnectionFailed` und SecureTransport-Fehler.
- **-1200 [3:-9824]** -HTTP-loadfehler.
- **-1200**  -  `NSURLConnection` wurde mit Fehlern abgeschlossen.

Zum Zeitpunkt der WatchOS 3 ist SSL/TLS-Verbindungen Sicherheit von Apple erzwungen wird. Betroffene Dienste und Anwendungen sollten aktualisiert Webserver in den neuesten Versionen der TLS-Protokoll verwenden. Finden Sie unter [NSURLConnection](#NSURLConnection) oben Weitere Informationen.


## <a name="snapshots"></a>Momentaufnahmen

WatchKit-apps, die nicht die neue übernommen haben `HandelBackgroundTask` API regelmäßige Updates nicht mehr in WatchOS 3 empfangen. 


## <a name="watchkit"></a>WatchKit

SpriteKit und SceneKit Szenen werden angehalten, wenn eine app Hintergrund in die WatchOS Andocken eingibt.


## <a name="related-links"></a>Verwandte Links

- [watchOS-Beispiele](https://developer.xamarin.com/samples/watchos/all/)
- [Was ist neu in WatchOS 3](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
