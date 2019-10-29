---
title: watchos 3-Problembehandlung
description: Dieses Dokument enthält einige Tipps zur Problembehandlung bei der Arbeit mit watchos 3 in xamarin. Tipps beziehen sich auf Aktivitäten, Apple Pay, Hintergrund Aktualisierungen, NSURLConnection, Datenschutz und vieles mehr.
ms.prod: xamarin
ms.assetid: 5911D898-0E23-40CC-9F3C-5F61B4D50ADC
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 093ac4a3242866413042de0b650433d4369ad35f
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028258"
---
# <a name="watchos-3-troubleshooting"></a>watchos 3-Problembehandlung

_Dieser Artikel enthält einige Tipps zur Problembehandlung bei der Arbeit mit watchos 3 in xamarin Apple Watch-apps._

Auf dieser Seite werden einige bekannte Probleme aufgelistet, die bei der Verwendung von watchos 3 mit xamarin und der Lösung für diese Probleme auftreten können.

## <a name="activities"></a>Aktivitäten

Damit die gemeinsame Nutzung von Aktivitäten ordnungsgemäß funktioniert, muss für alle gekoppelten Apple Watch watchos 3 ausgeführt werden.

Bekannte Probleme:

- Das Antworten auf eine Benachrichtigung über die Aktivitäts Freigabe schlägt manchmal fehl.
- Beim Antworten auf eine Aktivitäts Freigabe Benachrichtigung mit einer Nachricht tritt möglicherweise ein Fehler auf.
- Der Kontext Text oberhalb einer Benachrichtigungs Benachrichtigung für die Aktivitäts Freigabe ist falsch.

## <a name="apple-pay"></a>Apple Pay

Bekannte Probleme:

- Wenn ein falsches Ablaufdatum oder CW-Code für eine neue Zahlungsweise in Apple Pay eingegeben wird, stürzt der laufende Prozess ab, wenn er auf den **nächsten** Punkt trifft.
- In-App-Apple Pay Käufe, die eine PIN-Nummer erfordern, können abstürzen.

## <a name="auto-mac-unlock"></a>Automatische Mac-Sperre

Wenn Sie watchos 3 Beta 2 (oder höher) und macOS Sierra Beta 2 (oder höher) verwenden und die zweistufige Authentifizierung für das icloud-Konto des Benutzers aktiviert ist, können Sie Ihre Apple Watch zum automatischen Entsperren Ihres Mac verwenden.

## <a name="background-refresh"></a>Hintergrund Aktualisierung

Das verletzen von Systemressourcen führt zum Absturz einer watchos 3-APP mit den folgenden Ausnahme Codes:

- **0xc51bad01** : die APP hat zu viel CPU-Zeit beansprucht.
- **0xc51bad02** : die APP hat zu viel Zeit beansprucht.
- **0xc51bad03** : die APP verfügte nicht über genügend Laufzeit, um die aktuelle Aufgabe abzuschließen.

## <a name="clock"></a>Uhr

Komplikationen von neu installierten Apple Watch-Apps können als leer angezeigt werden. Neustart Apple Watch, um dieses Problem zu beheben.

## <a name="connectivity"></a>Konnektivität

Bekannte Probleme:

- watchos fordert den Benutzer nicht zur Eingabe von Zugriffsberechtigungen für geschützte Benutzerdaten auf der Apple Watch auf. Gewähren Sie Zugriff auf die iPhone-App, bevor Sie Daten in der Watch-App verwenden.
- Der Apple Watch kann einen Status eingeben, in dem alle watchconnectivity-Übertragungen fehlschlagen. Starten Sie den Apple Watch neu

## <a name="notifications"></a>Benachrichtigungen

Wenn eine Medien Anlage zu groß ist, wird Sie auf dem iPhone des Benutzers angezeigt, aber nicht auf Ihrem Apple Watch.

## <a name="nsurlconnection"></a>NSURLConnection

Alle `NSURLConnection` Verbindungen, die ältere TLS-Protokolle verwenden, schlagen fehl. Für alle SSL/TLS-Verbindungen ist das symmetrische RC4-Chiffre jetzt standardmäßig deaktiviert. Außerdem unterstützt die Secure-Transport-API SSLv3 nicht mehr. es wird empfohlen, dass die APP so bald wie möglich die SHA-1-und 3DES-Kryptografie nicht mehr verwendet.

Ab watchos 3 wird die Sicherheit von SSL/TLS-Verbindungen streng von Apple erzwungen. Betroffene Dienste und apps sollten Webserver aktualisiert haben, damit Sie die neuesten TLS-Protokoll Versionen verwenden.

## <a name="nsurlsession"></a>NSURLSession

Ab watchos 3 muss die `HTTPBodyStream`-Eigenschaft der `NSMutableURLRequest`-Klasse auf einen nicht geöffneten Stream festgelegt werden, da `NSURLConnection` und `NSURLSession` diese Anforderung jetzt streng erzwingen.

## <a name="privacy"></a>Datenschutz

Bekannte Probleme:

Beim Arbeiten mit `https://`-URLs unterstützen sowohl `NSURLSession` als auch `NSURLConnection` keine RC4-Verschlüsselungs Sammlungen mehr während des TLS-Handshakes. Einer der folgenden Fehlercodes kann generiert werden:

- **-1200 oder-98** -für `NSURLErrorSecurityConnectionFailed`-und securetransport-Fehler.
- **-1200 [3:-9824]** -Fehler beim Laden von http.
- **-1200** - `NSURLConnection` mit einem Fehler beendet.

Ab watchos 3 wird die Sicherheit von SSL/TLS-Verbindungen streng von Apple erzwungen. Betroffene Dienste und apps sollten Webserver aktualisiert haben, damit Sie die neuesten TLS-Protokoll Versionen verwenden. Weitere Informationen finden Sie unter [NSURLConnection](#nsurlconnection) weiter oben.

## <a name="snapshots"></a>Momentaufnahmen

Watchkit-apps, die die neue `HandelBackgroundTask`-API nicht übernommen haben, erhalten in watchos 3 keine regelmäßigen Updates mehr. 

## <a name="watchkit"></a>WatchKit

Spritekit-und scenekit-Szenen werden angehalten, wenn eine app in den Hintergrund des watchos-Dock gelangt.

## <a name="related-links"></a>Verwandte Links

- [watchOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+watchOS)
- [Neues in watchos 3](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
