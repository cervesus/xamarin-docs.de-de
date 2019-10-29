---
title: Problembehandlung bei tvos 10 mit xamarin erstellten apps
description: Dieser Artikel enthält einige Tipps zur Problembehandlung für die Arbeit mit tvos 10 in xamarin-apps. Es werden Probleme im Zusammenhang mit dem App-Store, die binäre Kompatibilität, das CFNetwork-httpProtocol, das cloudkit, das Core-Image, nsuseractivity und UIKit beschrieben.
ms.prod: xamarin
ms.assetid: EA5564BB-C415-49A2-B70C-3DBF5E0F3FAB
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: a6588dee675aee3e2580b70dfdea2920c6235775
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030604"
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

<a name="App-Store" />

## <a name="app-store"></a>App Store

Bekannte Probleme:

- Wenn Sie in-App-Käufe in der Sandbox Umgebung testen, wird das Authentifizierungs Dialogfeld möglicherweise zweimal angezeigt.
- Wenn Sie in-App-Käufe mit gehosteten Inhalten in der Sandbox Umgebung testen, wird das Dialogfeld Kennwort jedes Mal angezeigt, wenn die app in den Vordergrund gestellt wird, bis der Download des Inhalts abgeschlossen ist.

<a name="Binary-Compatibility" />

## <a name="binary-compatibility"></a>Binäre Kompatibilität

Bekannte Probleme:

- Wenn Sie `NSObject.ValueForKey` `null` Schlüssel aufrufen, wird eine Ausnahme ausgelöst.
- Wenn Sie beim Aufrufen von `UIFont.WithName` auf eine Schriftart nach Namen verweisen, wird ein Absturz verursacht.
- Beide `NSURLSession` und `NSURLConnection` während des TLS-Handshakes für `http://`-URLs nicht mehr RC4-Verschlüsselungs Sammlungen.
- Apps können hängen bleiben, wenn Sie die Geometrie einer Super Ansicht in den `ViewWillLayoutSubviews`-oder `LayoutSubviews`-Methoden ändern.
- Für alle SSL/TLS-Verbindungen ist das symmetrische RC4-Chiffre jetzt standardmäßig deaktiviert. Außerdem unterstützt die Secure-Transport-API SSLv3 nicht mehr. es wird empfohlen, dass die APP so bald wie möglich die SHA-1-und 3DES-Kryptografie nicht mehr verwendet.

<a name="CFNetwork-HTTP-Protocol" />

## <a name="cfnetwork-http-protocol"></a>CFNetwork-http-Protokoll

Die `HTTPBodyStream`-Eigenschaft der `NSMutableURLRequest`-Klasse muss auf einen nicht geöffneten Stream festgelegt werden, da `NSURLConnection` und `NSURLSession` diese Anforderung jetzt strikt erzwingen.

<a name="CloudKit" />

## <a name="cloudkit"></a>CloudKit

Vorgänge mit langer Ausführungszeit geben eine _"Sie besitzen keine Berechtigung zum Speichern der Datei" zurück._ Zeit.

<a name="CoreImage" />

## <a name="core-image"></a>Core-Image

Die `CIImageProcessor`-API unterstützt jetzt eine beliebige Eingabe Image Anzahl. `CIImageProcessor` API, die in tvos 10 Beta 1 enthalten war, wird entfernt.

<a name="NSUserActivity" />

## <a name="nsuseractivity"></a>NSUserActivity

Nach einem Handoff-Vorgang ist die `UserInfo`-Eigenschaft eines `NSUserActivity` Objekts möglicherweise leer. Nennen Sie `BecomeCurrent` nsuseractivity-Objekt explizit als aktuelle Problem Umgehung.

<a name="UIKit" />

## <a name="uikit"></a>UIKit

Bekannte Probleme:

- Änderungen an der Hintergrund Darstellung von `UINavigationBar`, `UITabBar` oder `UIToolBar` können zu einem Layoutdurchlauf führen, um die neue Darstellung aufzulösen. Wenn Sie versuchen, diese Vorkommen innerhalb eines `LayoutSubviews`-, `UpdateConstraints`-, `WillLayoutSubviews`-oder `DidUpdateSubviews`-Ereignisses zu ändern, kann dies zu einer endlos layoutschleife führen.
- In tvos 10 wird durch den Aufruf der `RemoveGestureRecognizer`-Methode eines `UIView` Objekts jede in Bearbeitung befindliche Gestenerkennung explizit abgebrochen.
- Vorgestellte Ansichts Controller können sich nun auf die Darstellung der Statusleiste auswirken.
- tvos 10 erfordert, dass der Entwickler `base.AwakeFromNib` bei der Unterklassen `UIViewController` aufruft und die `AwakeFromNib`-Methode überschreibt.
- Apps mit benutzerdefinierten `UIView` Unterklassen, die `LayoutSubviews` und das Layout vor dem Aufrufen von `base.LayoutSubviews` überschreiben, können eine endlos layoutschleife in tvos 10 auslöst.
- Zuweisungs spezifische oder flippbare Bilder sind bei Zuweisung zu `UIButton` Objekten nicht gekippt.

## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [Neues in tvos 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
