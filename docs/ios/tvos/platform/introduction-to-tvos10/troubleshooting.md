---
title: Problembehandlung bei tvos 10 mit xamarin erstellten apps
description: Dieser Artikel enthält einige Tipps zur Problembehandlung für die Arbeit mit tvos 10 in xamarin-apps. Es werden Probleme im Zusammenhang mit dem App-Store, die binäre Kompatibilität, das CFNetwork-httpProtocol, das cloudkit, das Core-Image, nsuseractivity und UIKit beschrieben.
ms.prod: xamarin
ms.assetid: EA5564BB-C415-49A2-B70C-3DBF5E0F3FAB
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: cd04450d1429092453e6d8b65278d87b5d52e45e
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571622"
---
# <a name="troubleshooting-tvos-10-apps-built-with-xamarin"></a>Problembehandlung bei tvos 10 mit xamarin erstellten apps

In den folgenden Abschnitten werden einige bekannte Probleme aufgelistet, die bei der Verwendung von tvos 10 mit xamarin und der Lösung für diese Probleme auftreten können:

- [App Store](#App-Store)
- [Binäre Kompatibilität](#Binary-Compatibility)
- [CFNetwork-http-Protokoll](#CFNetwork-HTTP-Protocol)
- [CloudKit](#CloudKit)
- [Core-Image](#CoreImage)
- [NSUserActivity](#NSUserActivity)
- [UIKit](#UIKit)

<a name="App-Store"></a>

## <a name="app-store"></a>App Store

Bekannte Probleme:

- Wenn Sie in-App-Käufe in der Sandbox Umgebung testen, wird das Authentifizierungs Dialogfeld möglicherweise zweimal angezeigt.
- Wenn Sie in-App-Käufe mit gehosteten Inhalten in der Sandbox Umgebung testen, wird das Dialogfeld Kennwort jedes Mal angezeigt, wenn die app in den Vordergrund gestellt wird, bis der Download des Inhalts abgeschlossen ist.

<a name="Binary-Compatibility"></a>

## <a name="binary-compatibility"></a>Binäre Kompatibilität

Bekannte Probleme:

- `NSObject.ValueForKey`Wenn Sie einen Schlüssel aufrufen, wird eine Ausnahme ausgelöst `null` .
- Das verweisen auf eine Schriftart nach Namen beim Aufrufen von `UIFont.WithName` führt zu einem Absturz.
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

Die `CIImageProcessor` API unterstützt jetzt eine beliebige Eingabe Image Anzahl. `CIImageProcessor`Die API, die in tvos 10 Beta 1 enthalten war, wird entfernt.

<a name="NSUserActivity"></a>

## <a name="nsuseractivity"></a>NSUserActivity

Nach einem Handoff-Vorgang ist die- `UserInfo` Eigenschaft eines- `NSUserActivity` Objekts möglicherweise leer. Ruft das `BecomeCurrent` nsuseractivity-Objekt explizit als aktuelle Problem Umgehung auf.

<a name="UIKit"></a>

## <a name="uikit"></a>UIKit

Bekannte Probleme:

- Ändert die Hintergrund `UINavigationBar` `UITabBar` Darstellung von oder `UIToolBar` kann zu einem Layoutdurchlauf führen, um die neue Darstellung aufzulösen. Wenn Sie versuchen, diese Vorkommen innerhalb eines-,-oder-Ereignisses zu ändern `LayoutSubviews` , kann dies zu `UpdateConstraints` `WillLayoutSubviews` `DidUpdateSubviews` einer endlos layoutschleife führen.
- In tvos 10 werden durch das Aufrufen der- `RemoveGestureRecognizer` Methode eines- `UIView` Objekts explizit alle in Bearbeitung befindlichen Gesten Erkennungsmethoden abgebrochen.
- Vorgestellte Ansichts Controller können sich nun auf die Darstellung der Statusleiste auswirken.
- tvos 10 erfordert, dass der Entwickler `base.AwakeFromNib` beim Unterklassen `UIViewController` -und Überschreiben der- `AwakeFromNib` Methode aufruft.
- Apps mit benutzerdefinierten `UIView` Unterklassen, `LayoutSubviews` die das Layout überschreiben und vor dem Aufrufen von geändert haben, `base.LayoutSubviews` können eine endlos layoutschleife in tvos 10 auslöst.
- Zuweisungs spezifische oder flippbare Bilder sind bei der Zuweisung zu Objekten nicht gekippt `UIButton` .

## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [Neues in tvos 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
