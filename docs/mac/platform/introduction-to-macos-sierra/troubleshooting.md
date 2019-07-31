---
title: Xamarin. Mac-macOS Sierra Problembehandlung
description: Dieses Dokument enthält einige Tipps zur Problembehandlung beim Arbeiten mit macOS Sierra in xamarin. Mac-apps. Tipps beziehen sich auf den Mac App Store, Apple Pay, binäre Kompatibilität, CFNetwork, cloudkit usw.
ms.prod: xamarin
ms.assetid: 323DD5EE-87CE-48E4-B234-1CF61B45A019
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 09/22/2016
ms.openlocfilehash: 8959527f0be7b4d5a3e0a2de5f0db4a4a25fa305
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68647897"
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

<a name="App-Store" />

## <a name="app-store"></a>App Store

Bekannte Probleme:

- Wenn Sie in-App-Käufe in der Sandbox Umgebung testen, wird das Authentifizierungs Dialogfeld möglicherweise zweimal angezeigt.
- Wenn Sie in-App-Käufe mit gehosteten Inhalten in der Sandbox Umgebung testen, wird das Dialogfeld Kennwort jedes Mal angezeigt, wenn die app in den Vordergrund gestellt wird, bis der Download des Inhalts abgeschlossen ist.

<a name="Apple-Pay" />

## <a name="apple-pay"></a>Apple Pay

Wenn beim Hinzufügen einer neuen Zahlungskarte zu Apple Pay ein falsches Ablaufdatum oder Sicherheitscode eingegeben wird, wird der Karten Bereitstellungs Prozess beendet.

<a name="Binary-Compatibility" />

## <a name="binary-compatibility"></a>Binäre Kompatibilität

Bekannte Probleme:

- Wenn `NSObject.ValueForKey` Sie einen `null` Schlüssel aufrufen, wird eine Ausnahme ausgelöst.
- Und nicht `NSURLConnection` mehr RC4-Verschlüsselungs Sammlungen während des TLS-Handshake für `http://` URLs. `NSURLSession`
- Apps können hängen bleiben, wenn Sie die Geometrie einer SuperView in der `ViewWillLayoutSubviews` - `LayoutSubviews` Methode oder der-Methode ändern.
- Für alle SSL/TLS-Verbindungen ist das symmetrische RC4-Chiffre jetzt standardmäßig deaktiviert. Außerdem unterstützt die Secure-Transport-API SSLv3 nicht mehr. es wird empfohlen, dass die APP so bald wie möglich die SHA-1-und 3DES-Kryptografie nicht mehr verwendet.

<a name="CFNetwork-HTTP-Protocol" />

## <a name="cfnetwork-http-protocol"></a>CFNetwork-http-Protokoll

Die `HTTPBodyStream` -Eigenschaft `NSMutableURLRequest` der `NSURLConnection` -`NSURLSession` Klasse muss auf einen nicht geöffneten Stream festgelegt werden, da Sie diese Anforderung jetzt strikt erzwingt.

<a name="CloudKit" />

## <a name="cloudkit"></a>CloudKit

Vorgänge mit langer Ausführungszeit geben eine _"Sie besitzen keine Berechtigung zum Speichern der Datei" zurück._ Zeit.

<a name="CoreImage" />

## <a name="core-image"></a>Core-Image

Die `CIImageProcessor` API unterstützt jetzt eine beliebige Eingabe Image Anzahl. `CIImageProcessor`Die API, die in macOS Sierra Beta 1 enthalten war, wird entfernt.

<a name="Notifications" />

## <a name="notifications"></a>Benachrichtigungen

Beim Arbeiten mit Benachrichtigungs Inhalts Erweiterungen werden die Ansichts Controller nicht ordnungsgemäß freigegeben und können zu einem Absturz führen, wenn die Grenzwerte für den Erweiterungs Arbeitsspeicher erreicht werden.

<a name="NSUserActivity" />

## <a name="nsuseractivity"></a>NSUserActivity

Nach einem Handoff-Vorgang ist `UserInfo` die-Eigenschaft `NSUserActivity` eines-Objekts möglicherweise leer. Explizit Objekt `BecomeCurrent` als aktuelle Problem Umgehung aufzurufen `NSUserActivity` .

<a name="Safari" />

## <a name="safari"></a>Safari

Webgeolokation erfordert eine sichere (`https://`)-URL, um sowohl an IOS 10 als auch macOS Sierra arbeiten zu können, um die böswillige Verwendung von Standortdaten zu verhindern.

## <a name="related-links"></a>Verwandte Links

- [Mac-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Mac)
- [Neues in macOS 10,12](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
