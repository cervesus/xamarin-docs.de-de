---
title: Xamarin. Mac-macOS Sierra Problembehandlung
description: Dieses Dokument enthält einige Tipps zur Problembehandlung beim Arbeiten mit macOS Sierra in xamarin. Mac-apps. Tipps beziehen sich auf den Mac App Store, Apple Pay, binäre Kompatibilität, CFNetwork, cloudkit usw.
ms.prod: xamarin
ms.assetid: 323DD5EE-87CE-48E4-B234-1CF61B45A019
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 09/22/2016
ms.openlocfilehash: e7bc6fa12ab6720842ab264678cbf8124353fc40
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84574417"
---
# <a name="xamarinmac---macos-sierra-troubleshooting"></a>Xamarin. Mac-macOS Sierra Problembehandlung

_Dieser Artikel enthält einige Tipps zur Problembehandlung beim Arbeiten mit macOS Sierra in xamarin. Mac-apps._

In den folgenden Abschnitten werden einige bekannte Probleme aufgeführt, die auftreten können, wenn macOS Sierra mit xamarin. Mac verwendet und die Lösung für diese Probleme verwendet wird:

- [App Store](#App-Store)
- [Apple Pay](#Apple-Pay)
- [Binäre Kompatibilität](#Binary-Compatibility)
- [CFNetwork-http-Protokoll](#CFNetwork-HTTP-Protocol)
- [CloudKit](#CloudKit)
- [Core-Image](#CoreImage)
- [Benachrichtigungen](#Notifications)
- [NSUserActivity](#NSUserActivity)
- [Safari](#Safari)

<a name="App-Store"></a>

## <a name="app-store"></a>App Store

Bekannte Probleme:

- Wenn Sie in-App-Käufe in der Sandbox Umgebung testen, wird das Authentifizierungs Dialogfeld möglicherweise zweimal angezeigt.
- Wenn Sie in-App-Käufe mit gehosteten Inhalten in der Sandbox Umgebung testen, wird das Dialogfeld Kennwort jedes Mal angezeigt, wenn die app in den Vordergrund gestellt wird, bis der Download des Inhalts abgeschlossen ist.

<a name="Apple-Pay"></a>

## <a name="apple-pay"></a>Apple Pay

Wenn beim Hinzufügen einer neuen Zahlungskarte zu Apple Pay ein falsches Ablaufdatum oder Sicherheitscode eingegeben wird, wird der Karten Bereitstellungs Prozess beendet.

<a name="Binary-Compatibility"></a>

## <a name="binary-compatibility"></a>Binäre Kompatibilität

Bekannte Probleme:

- `NSObject.ValueForKey`Wenn Sie einen Schlüssel aufrufen, wird eine Ausnahme ausgelöst `null` .
- `NSURLSession`Und `NSURLConnection` nicht mehr RC4-Verschlüsselungs Sammlungen während des TLS-Handshake für `http://` URLs.
- Apps können hängen bleiben, wenn Sie die Geometrie einer SuperView in der- `ViewWillLayoutSubviews` Methode oder der- `LayoutSubviews` Methode ändern.
- Für alle SSL/TLS-Verbindungen ist das symmetrische RC4-Chiffre jetzt standardmäßig deaktiviert. Außerdem unterstützt die Secure-Transport-API SSLv3 nicht mehr. es wird empfohlen, dass die APP so bald wie möglich die SHA-1-und 3DES-Kryptografie nicht mehr verwendet.

<a name="CFNetwork-HTTP-Protocol"></a>

## <a name="cfnetwork-http-protocol"></a>CFNetwork-http-Protokoll

Die `HTTPBodyStream` -Eigenschaft der `NSMutableURLRequest` -Klasse muss auf einen nicht geöffneten Stream festgelegt werden, da Sie `NSURLConnection` `NSURLSession` diese Anforderung jetzt strikt erzwingt.

<a name="CloudKit"></a>

## <a name="cloudkit"></a>CloudKit

Vorgänge mit langer Ausführungszeit geben eine _"Sie besitzen keine Berechtigung zum Speichern der Datei" zurück._ angezeigt.

<a name="CoreImage"></a>

## <a name="core-image"></a>Core-Image

Die `CIImageProcessor` API unterstützt jetzt eine beliebige Eingabe Image Anzahl. `CIImageProcessor`Die API, die in macOS Sierra Beta 1 enthalten war, wird entfernt.

<a name="Notifications"></a>

## <a name="notifications"></a>Benachrichtigungen

Beim Arbeiten mit Benachrichtigungs Inhalts Erweiterungen werden die Ansichts Controller nicht ordnungsgemäß freigegeben und können zu einem Absturz führen, wenn die Grenzwerte für den Erweiterungs Arbeitsspeicher erreicht werden.

<a name="NSUserActivity"></a>

## <a name="nsuseractivity"></a>NSUserActivity

Nach einem Handoff-Vorgang ist die- `UserInfo` Eigenschaft eines- `NSUserActivity` Objekts möglicherweise leer. Explizit `BecomeCurrent` `NSUserActivity` Objekt als aktuelle Problem Umgehung aufzurufen.

<a name="Safari"></a>

## <a name="safari"></a>Safari

Webgeolokation erfordert eine sichere ( `https://` )-URL, um sowohl an IOS 10 als auch macOS Sierra arbeiten zu können, um die böswillige Verwendung von Standortdaten zu verhindern.

## <a name="related-links"></a>Verwandte Links

- [Mac-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Mac)
- [Neues in macOS 10,12](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
