---
title: Problembehandlung für TvOS 10 Apps mit Xamarin erstellt wurde
description: Dieser Artikel enthält mehrere Problembehandlungstipps für die Arbeit mit TvOS 10 in Xamarin-apps. Probleme im Zusammenhang mit dem App Store, binäre Kompatibilität, CFNetwork HttpProtocol, CloudKit, Core-Image, NSUserActivity und UIKit beschrieben.
ms.prod: xamarin
ms.assetid: EA5564BB-C415-49A2-B70C-3DBF5E0F3FAB
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 3815790cfb73f93f399c14d3da44aa3210725388
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "60932437"
---
# <a name="troubleshooting-tvos-10-apps-built-with-xamarin"></a>Problembehandlung für TvOS 10 Apps mit Xamarin erstellt wurde

Den folgenden Abschnitten werden einige bekannte Probleme, die Verwendung von TvOS 10 mit Xamarin und die Lösung dieser Probleme auftreten können:

- [App Store](#App-Store)
- [Binäre Kompatibilität](#Binary-Compatibility)
- [CFNetwork HTTP-Protokoll](#CFNetwork-HTTP-Protocol)
- [CloudKit](#CloudKit)
- [Core-Image](#CoreImage)
- [NSUserActivity](#NSUserActivity)
- [UIKit](#UIKit)

<a name="App-Store" />

## <a name="app-store"></a>App Store

Bekannte Probleme:

 - Wenn In-App-Käufe in der sandboxumgebung zu testen, kann das Dialogfeld "Authentifizierung" zweimal angezeigt.
 - Wenn Sie In-App-Einkäufe mit gehosteten Inhalt in der sandboxumgebung zu testen, wird das Dialogfeld "Kennwort" angezeigt, jedes Mal, wenn die app in den Vordergrund gesetzt wird, bis der Download des Inhalte abgeschlossen ist.

<a name="Binary-Compatibility" />

## <a name="binary-compatibility"></a>Binäre Kompatibilität

Bekannte Probleme:

 - Aufrufen von `NSObject.ValueForKey` wird eine `null` Schlüssel führt zu einer Ausnahme.
 - Verweisen auf eine Schriftart anhand des Namens, beim Aufrufen von `UIFont.WithName` wird einen Absturz verursachen.
 - Beide `NSURLSession` und NSURLConnection` no longer RC4 cipher suites during the TLS handshake for `http://' URLs.
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

Die `CIImageProcessor` API unterstützt nun eine Anzahl von beliebigen eingabebildern speichereffiziente. `CIImageProcessor` -API, die in TvOS 10 Beta 1 enthalten war, werden entfernt.

<a name="NSUserActivity" />

## <a name="nsuseractivity"></a>NSUserActivity

Nach einer Übergabe die `UserInfo` Eigenschaft eine `NSUserActivity` Objekt kann leer sein. Explizit aufrufen `BecomeCurrent` NSUserActivity "-Objekt als aktuellen dieses Problem zu umgehen.

<a name="UIKit" />

## <a name="uikit"></a>UIKit

Bekannte Probleme:

 - Änderungen an der Darstellung des Hintergrunds `UINavigationBar`, `UITabBar` oder `UIToolBar` möglicherweise an einem Layoutdurchlauf, um das neue Erscheinungsbild zu beheben. Versucht, diese Darstellungen in der ein `LayoutSubviews`, `UpdateConstraints`, `WillLayoutSubviews` oder `DidUpdateSubviews` Ereignis kann dazu führen, eine unbegrenzte Layout-Schleife.
 - In TvOS 10 Aufrufen der `RemoveGestureRecognizer` Methode eine `UIView` Objekt explizit bricht alle in Bearbeitung befindlichen Stiftbewegungs-Erkennung.
 - Angegebene View-Controller kann nun die Darstellung der Statusleiste beeinträchtigen.
 - TvOS 10 verlangt vom Entwickler rufen `base.AwakeFromNib` beim Erstellen von Unterklassen für `UIViewController` und überschreiben die `AwakeFromNib` Methode.
 - Apps mit benutzerdefinierten `UIView` Unterklassen, die außer Kraft setzen `LayoutSubviews` und das Layout vor dem Aufruf modifizierte `base.LayoutSubviews` kann eine unbegrenzte Layout-Schleife in TvOS 10 ausgelöst werden.
 - Richtung-spezifische oder flippable Images-Ressourcen sind kein Kippen verwendet, wenn zugewiesen `UIButton` Objekte.

## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [Neuerungen in TvOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
