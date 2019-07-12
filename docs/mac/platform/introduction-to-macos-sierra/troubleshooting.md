---
title: Xamarin.Mac - MacOS Sierra-Problembehandlung
description: Dieses Dokument enthält mehrere Problembehandlungstipps für das Arbeiten mit MacOS Sierra in Xamarin.Mac-apps. Tipps beziehen sich auf den Mac App Store, Apple Pay, binäre Kompatibilität, CFNetwork, CloudKit und vieles mehr.
ms.prod: xamarin
ms.assetid: 323DD5EE-87CE-48E4-B234-1CF61B45A019
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 09/22/2016
ms.openlocfilehash: 322acff3279d0513266c7d9883726cac726334f7
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67830558"
---
# <a name="xamarinmac---macos-sierra-troubleshooting"></a>Xamarin.Mac - MacOS Sierra-Problembehandlung

_Dieser Artikel enthält mehrere Problembehandlungstipps für das Arbeiten mit MacOS Sierra in Xamarin.Mac-apps._

Den folgenden Abschnitten werden einige bekannte Probleme, die Verwendung von MacOS Sierra mit Xamarin.mac und die Lösung dieser Probleme auftreten können:

- [App Store](#App-Store)
- [Apple Pay](#Apple-Pay)
- [Binäre Kompatibilität](#Binary-Compatibility)
- [CFNetwork HTTP-Protokoll](#CFNetwork-HTTP-Protocol)
- [CloudKit](#CloudKit)
- [Core-Image](#CoreImage)
- [Benachrichtigungen](#Notifications)
- [NSUserActivity](#NSUserActivity)
- [Safari](#Safari)

<a name="App-Store" />

## <a name="app-store"></a>App Store

Bekannte Probleme:

- Wenn In-App-Käufe in der sandboxumgebung zu testen, kann das Dialogfeld "Authentifizierung" zweimal angezeigt.
- Wenn Sie In-App-Einkäufe mit gehosteten Inhalt in der sandboxumgebung zu testen, wird das Dialogfeld "Kennwort" angezeigt, jedes Mal, wenn die app in den Vordergrund gesetzt wird, bis der Download des Inhalte abgeschlossen ist.

<a name="Apple-Pay" />

## <a name="apple-pay"></a>Apple Pay

Ein falsche Ablauf Datums- oder Sicherheit-Code (CW) eingegeben, wird beim Hinzufügen einer neuen Zahlungskarte aufgedruckt ist Apple Pay wird die Karte der Bereitstellungsprozess beendet.

<a name="Binary-Compatibility" />

## <a name="binary-compatibility"></a>Binäre Kompatibilität

Bekannte Probleme:

- Aufrufen von `NSObject.ValueForKey` wird eine `null` Schlüssel führt zu einer Ausnahme.
- Beide `NSURLSession` und `NSURLConnection` mehr RC4-Verschlüsselungssammlungen während des TLS-Handshakes für `http://` URLs.
- Apps können hängen bleiben, wenn sie eine Benachrichtigungen für die eine Geometrie in entweder zum Ändern der `ViewWillLayoutSubviews` oder `LayoutSubviews` Methoden.
- Für alle SSL/TLS-Verbindungen ist das symmetrische RC4-Verschlüsselungsverfahren jetzt standardmäßig deaktiviert. Darüber hinaus den sicheren Transport-API unterstützt nicht mehr SSLv3, und es wird empfohlen, dass die app mithilfe von SHA-1 und 3DES Kryptografie so bald wie möglich zu beenden.

<a name="CFNetwork-HTTP-Protocol" />

## <a name="cfnetwork-http-protocol"></a>CFNetwork HTTP-Protokoll

Die `HTTPBodyStream` Eigenschaft der `NSMutableURLRequest` Klasse muss festgelegt werden, in einer nicht geöffneten Stream seit `NSURLConnection` und `NSURLSession` jetzt genau diese Anforderung zu erzwingen.

<a name="CloudKit" />

## <a name="cloudkit"></a>CloudKit

Lang andauernde Vorgänge zurückgegeben wird ein _"Sie haben keine Berechtigung zum Speichern der Datei."_ Fehler.

<a name="CoreImage" />

## <a name="core-image"></a>Core-Image

Die `CIImageProcessor` API unterstützt nun eine Anzahl von beliebigen eingabebildern speichereffiziente. `CIImageProcessor` -API, die in MacOS Sierra Beta 1 enthalten war, werden entfernt.

<a name="Notifications" />

## <a name="notifications"></a>Benachrichtigungen

Bei der Arbeit mit Notification-Content-Erweiterungen Ansichtscontroller werden nicht ordnungsgemäß freigegeben wird, und kann bei einem Absturz führen, wenn die Erweiterung Memory Limits erreicht sind.

<a name="NSUserActivity" />

## <a name="nsuseractivity"></a>NSUserActivity

Nach einer Übergabe die `UserInfo` Eigenschaft eine `NSUserActivity` Objekt kann leer sein. Explizit aufrufen `BecomeCurrent` `NSUserActivity` Objekt aktuelle dieses Problem zu umgehen.

<a name="Safari" />

## <a name="safari"></a>Safari

WebGeolocation erfordert eine sichere (`https://`) URL funktioniert sowohl für iOS 10 als auch für MacOS Sierra einen Missbrauch von Daten zu verhindern.







## <a name="related-links"></a>Verwandte Links

- [Mac-Beispiele](https://developer.xamarin.com/samples/mac/)
- [Neuerungen in OS X 10.12](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
