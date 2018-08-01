---
title: Xamarin.Mac - MacOS Sierra Problembehandlung
description: Dieses Dokument enthält einige Tipps zur Problembehandlung für die Arbeit mit MacOS Sierra in Xamarin.Mac-apps. Tipps beziehen sich auf dem Mac App Store, Apple Pay Binärkompatibilität, CFNetwork, CloudKit und mehr.
ms.prod: xamarin
ms.assetid: 323DD5EE-87CE-48E4-B234-1CF61B45A019
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/22/2016
ms.openlocfilehash: 5b2571d9562fd137257e2dd0ea2ada8f071bab92
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792319"
---
# <a name="xamarinmac---macos-sierra-troubleshooting"></a>Xamarin.Mac - MacOS Sierra Problembehandlung

_Dieser Artikel enthält einige Tipps zur Problembehandlung für die Arbeit mit MacOS Sierra in Xamarin.Mac-apps._

Den folgenden Abschnitten werden einige bekannte Probleme, die auftreten können, wenn MacOS Sierra mit Xamarin.mac und die Lösung dieser Probleme verwenden:

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

- Wenn In App-Käufen in der Sandbox-Umgebung zu testen, kann das Authentifizierungsdialogfeld zweimal angezeigt.
- In App-Einkäufe gehosteten Inhalt in der Sandbox-Umgebung testen mit erscheint das Dialogfeld das Kennwort jedes Mal, wenn die app in den Vordergrund gebracht wird, bis der Download des Inhalte abgeschlossen ist.

<a name="Apple-Pay" />

## <a name="apple-pay"></a>Apple Pay

Ein falsche Ablauf Datums- oder Sicherheit-Code (WINS, CW) eingegeben, wird beim Hinzufügen einer neuen Kreditkarte bezahlen von Apple wird die Karte Bereitstellungsprozess beendet.

<a name="Binary-Compatibility" />

## <a name="binary-compatibility"></a>Binäre Kompatibilität

Bekannte Probleme:

- Aufrufen von `NSObject.ValueForKey` wird eine `null` Schlüssel wird eine Ausnahme ausgelöst.
- Beide `NSURLSession` und NSURLConnection` no longer RC4 cipher suites during the TLS handshake for `http://' URLs.
- Apps können hängen, wenn sie eine Überblicksansicht Geometry entweder zum Ändern der `ViewWillLayoutSubviews` oder `LayoutSubviews` Methoden.
- Für alle SSL/TLS-Verbindungen wird der symmetrische RC4-Verschlüsselungsverfahren jetzt standardmäßig deaktiviert. Darüber hinaus wird der sichere Transport-API unterstützt nicht mehr SSLv3, und es wird empfohlen, dass die Verwendung von SHA-1 und 3DES Kryptografie so bald wie möglich mit der app zu beenden.

<a name="CFNetwork-HTTP-Protocol" />

## <a name="cfnetwork-http-protocol"></a>CFNetwork HTTP-Protokoll

Die `HTTPBodyStream` Eigenschaft von der `NSMutableURLRequest` "Class" muss festgelegt werden, in eine geöffnete Stream seit `NSURLConnection` und `NSURLSession` nun ausschließlich diese Anforderung erzwingen.

<a name="CloudKit" />

## <a name="cloudkit"></a>CloudKit

Vorgänge mit langer zurückgegeben wird ein _"Sie haben keine Berechtigung zum Speichern der Datei."_ Fehler.

<a name="CoreImage" />

## <a name="core-image"></a>Core-Image

Die `CIImageProcessor` -API unterstützt nun eine beliebige Eingabe Abbildanzahl. `CIImageProcessor` API, die in MacOS Sierra Beta 1 enthalten war, werden entfernt.

<a name="Notifications" />

## <a name="notifications"></a>Benachrichtigungen

Bei der Arbeit mit Notification Content Erweiterungen View Controller werden nicht ordnungsgemäß freigegeben wird, und wird unter Umständen in einem Absturz Wenn Erweiterung arbeitsspeichergrenzwerte erreicht werden.

<a name="NSUserActivity" />

## <a name="nsuseractivity"></a>NSUserActivity

Nach einer Übergabe der `UserInfo` Eigenschaft ein `NSUserActivity` Objekt ist möglicherweise leer. Explizit aufrufen `BecomeCurrent` `NSUserActivity` -Objekt als aktuellen dieses Problem zu umgehen.

<a name="Safari" />

## <a name="safari"></a>Safari

WebGeolocation erfordert eine sichere (`https://`) URL, die unter iOS 10 und MacOS Sierra, um zu verhindern, dass bei der böswilligen Verwendung von Standortdaten funktionieren.







## <a name="related-links"></a>Verwandte Links

- [Mac-Beispiele](https://developer.xamarin.com/samples/mac/)
- [Was ist neu in OS X 10.12](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
