---
title: WatchOS 3 zur Problembehandlung
description: Dieses Dokument enthält einige nützliche Problembehandlungstipps, bei der Arbeit mit WatchOS 3 in Xamarin. Tipps beziehen sich auf die Aktivitäten, Apple Pay, Aktualisierung im Hintergrund, NSURLConnection, Datenschutz und vieles mehr.
ms.prod: xamarin
ms.assetid: 5911D898-0E23-40CC-9F3C-5F61B4D50ADC
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 6d2aaf12bd6c45f6268cf87a77d2ee03a9d7a888
ms.sourcegitcommit: 946ce514fd6575aa6b93ff24181e02a60b24b106
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2019
ms.locfileid: "58677728"
---
# <a name="watchos-3-troubleshooting"></a>WatchOS 3 zur Problembehandlung

_Dieser Artikel enthält mehrere Problembehandlungstipps für die Arbeit mit WatchOS 3 in Xamarin Apple Watch-apps._

Diese Seite listet einige bekannte Probleme, die Verwendung von WatchOS 3 mit Xamarin und die Lösung dieser Probleme auftreten können.

## <a name="activities"></a>Aktivitäten

Für die Aktivität, die Freigabe der ordnungsgemäß funktioniert muss alle gekoppelte Apple Watch WatchOS 3 ausgeführt werden.

Bekannte Probleme:

- Beim Antworten auf eine Aktivität Freigabe Benachrichtigung manchmal ein Fehler auftritt.
- Antworten auf eine Aktivität Sharing-Benachrichtigung mit der Meldung schlägt möglicherweise fehl.
- Kontextbezogene Text über eine Aktivität Freigabe benachrichtigungsmeldung wird falsch sein.

## <a name="apple-pay"></a>Apple Pay

Bekannte Probleme:

- Wenn eine falsche Ablaufdatum oder CW Code für eine neue Zahlungsmethode Sorgfalt Apple Pay, eingegeben wird, wenn erreicht **Weiter** der ausgeführte Prozess stürzt ab.
- Erfordert eine PIN-Nummer innerhalb der App Apple Pay-Käufe stürzt möglicherweise ab.

## <a name="auto-mac-unlock"></a>Automatische Mac entsperren

Mithilfe von WatchOS 3 Beta 2 (oder höher) und MacOS Sierra Beta 2 (oder höher), wenn zweistufige Authentifizierung für die iCloud-Konto des Benutzers aktiviert ist, können sie ihre Apple Watch-automatisch entsperren Sie ihren Mac.

## <a name="background-refresh"></a>Aktualisierung im Hintergrund

Verletzung der Systemressourcen führt zu einem Absturz des WatchOS 3-app mit den folgenden Ausnahmecodes:

- **0xc51bad01** -die app verbraucht zu viel CPU-Zeit.
- **0xc51bad02** -die app verbraucht zu viel Zeit für die Wand.
- **0xc51bad03** – die app keine genügend Common Language Runtime, um die aktuelle Aufgabe abzuschließen.

## <a name="clock"></a>Uhr

Komplikationen von neu installierten Apple Watch-apps möglicherweise leer angezeigt. Starten Sie die Apple Watch zum Beheben dieses Problems neu.

## <a name="connectivity"></a>Konnektivität

Bekannte Probleme:

- WatchOS fordert den Benutzer für die Zugriffsberechtigung für die Daten der geschützten Benutzer auf der Apple Watch nicht. Gewähren Sie Zugriff auf die iPhone-app, bevor Sie Daten in der Watch-app verwenden.
- Der Apple Watch können einen versetzt alle WatchConnectivity Übertragungen, in denen ein Fehler auftritt, neu starten der Apple Watch-Fehler beheben.

## <a name="notifications"></a>Benachrichtigungen

Wenn eine Anlage mit Medien zu groß ist, wird es auf des Benutzers iPhone, aber nicht ihre Apple Watch angezeigt.

## <a name="nsurlconnection"></a>NSURLConnection

Alle `NSURLConnection` unter Verwendung älterer TLS-Protokolle Verbindungen schlagen fehl. Für alle SSL/TLS-Verbindungen ist das symmetrische RC4-Verschlüsselungsverfahren jetzt standardmäßig deaktiviert. Darüber hinaus den sicheren Transport-API unterstützt nicht mehr SSLv3, und es wird empfohlen, dass die app mithilfe von SHA-1 und 3DES Kryptografie so bald wie möglich zu beenden.

Seit WatchOS 3 wird SSL/TLS-verbindungssicherheit streng von Apple erzwungen wird. Betroffene Dienste und apps sollten Webserver verwenden Sie die neuesten Versionen der TLS-Protokoll aktualisiert.

## <a name="nsurlsession"></a>Von NSURLSession

Ab WatchOS 3 die `HTTPBodyStream` Eigenschaft der `NSMutableURLRequest` Klasse muss festgelegt werden, in einer nicht geöffneten Stream seit `NSURLConnection` und `NSURLSession` jetzt genau diese Anforderung zu erzwingen.

## <a name="privacy"></a>Datenschutz

Bekannte Probleme:

Bei der Arbeit mit `https://` URLs, die beide `NSURLSession` und `NSURLConnection` RC4-Verschlüsselungssammlungen nicht mehr unterstützt, während des TLS-Handshake. Eine der folgenden Fehlercodes kann generiert:

- **– 1200 oder-98** : `NSURLErrorSecurityConnectionFailed` und SecureTransport-Fehler.
- **-1200 [3:-9824]** -HTTP-Fehler beim Laden des.
- **-1200**  -  `NSURLConnection` wurde mit Fehlern abgeschlossen.

Seit WatchOS 3 wird SSL/TLS-verbindungssicherheit streng von Apple erzwungen wird. Betroffene Dienste und apps sollten Webserver verwenden Sie die neuesten Versionen der TLS-Protokoll aktualisiert. Finden Sie unter [NSURLConnection](#nsurlconnection) über weitere Informationen.

## <a name="snapshots"></a>Momentaufnahmen

WatchKit-apps, die nicht die neue übernommen haben `HandelBackgroundTask` API durch regelmäßige Updates nicht mehr in WatchOS 3 erhalten. 

## <a name="watchkit"></a>WatchKit

SpriteKit und SceneKit-Szenen werden angehalten, wenn eine Anwendung in WatchOS Dock in den Hintergrund wechselt.

## <a name="related-links"></a>Verwandte Links

- [watchOS-Beispiele](https://developer.xamarin.com/samples/watchos/all/)
- [Neuerungen in WatchOS 3](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
