---
title: Problembehandlung
description: "Dieser Artikel enthält einige Tipps zur Problembehandlung für die Arbeit mit tvos. außerdem wurden 10 in Xamarin.tvOS-apps."
ms.topic: article
ms.prod: xamarin
ms.assetid: EA5564BB-C415-49A2-B70C-3DBF5E0F3FAB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: d45b9ed04d3ae4815d408d82068e588a2cbcc6f8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="troubleshooting"></a>Problembehandlung

_Dieser Artikel enthält einige Tipps zur Problembehandlung für die Arbeit mit tvos. außerdem wurden 10 in Xamarin.tvOS-apps._

Den folgenden Abschnitten werden einige bekannte Probleme, die auftreten können, wenn tvos. außerdem wurden 10 mit Xamarin.tvOS und die Lösung dieser Probleme verwenden:

- [App Store](#App-Store)
- [Binäre Kompatibilität](#Binary-Compatibility)
- [CFNetwork HTTP Protocol](#CFNetwork-HTTP-Protocol)
- [CloudKit](#CloudKit)
- [CoreImage](#CoreImage)
- [NSUserActivity](#NSUserActivity)
- [UIKit](#UIKit)

<a name="App-Store" />

## <a name="app-store"></a>App Store

Bekannte Probleme:

 - Wenn In App-Käufen in der Sandbox-Umgebung zu testen, kann das Authentifizierungsdialogfeld zweimal angezeigt.
 - In App-Einkäufe gehosteten Inhalt in der Sandbox-Umgebung testen mit erscheint das Dialogfeld das Kennwort jedes Mal, wenn die app in den Vordergrund gebracht wird, bis der Download des Inhalte abgeschlossen ist.

<a name="Binary-Compatibility" />

## <a name="binary-compatibility"></a>Binäre Kompatibilität

Bekannte Probleme:

 - Aufrufen von `NSObject.ValueForKey` wird eine `null` Schlüssel wird eine Ausnahme ausgelöst.
 - Verweisen auf eine Schriftart anhand des Namens, beim Aufrufen von `UIFont.WithName` führt dazu, dass ein Absturz (Crash).
 - Beide `NSURLSession` und NSURLConnection` no longer RC4 cipher suites during the TLS handshake for `http://' URLs.
 - Apps können hängen, wenn sie eine Überblicksansicht Geometry entweder zum Ändern der `ViewWillLayoutSubviews` oder `LayoutSubviews` Methoden.
 - Für alle SSL/TLS-Verbindungen wird der symmetrische RC4-Verschlüsselungsverfahren jetzt standardmäßig deaktiviert. Darüber hinaus wird der sichere Transport-API unterstützt nicht mehr SSLv3, und es wird empfohlen, dass die Verwendung von SHA-1 und 3DES Kryptografie so bald wie möglich mit der app zu beenden.

<a name="CFNetwork-HTTP-Protocol" />

## <a name="cfnetwork-http-protocol"></a>CFNetwork HTTP Protocol

Die `HTTPBodyStream` Eigenschaft von der `NSMutableURLRequest` "Class" muss festgelegt werden, in eine geöffnete Stream seit `NSURLConnection` und `NSURLSession` nun ausschließlich diese Anforderung erzwingen.

<a name="CloudKit" />

## <a name="cloudkit"></a>CloudKit

Vorgänge mit langer zurückgegeben wird ein _"Sie haben keine Berechtigung zum Speichern der Datei."_ Fehler.

<a name="CoreImage" />

## <a name="coreimage"></a>CoreImage

Die `CIImageProcessor` -API unterstützt nun eine beliebige Eingabe Abbildanzahl. `CIImageProcessor` API, die in der tvos. außerdem wurden 10 Beta 1 enthalten war, werden entfernt.

<a name="NSUserActivity" />

## <a name="nsuseractivity"></a>NSUserActivity

Nach einer Übergabe der `UserInfo` Eigenschaft ein `NSUserActivity` Objekt ist möglicherweise leer. Explizit aufrufen `BecomeCurrent` NSUserActivity "-Objekt als aktuellen dieses Problem zu umgehen.

<a name="UIKit" />

## <a name="uikit"></a>UIKit

Bekannte Probleme:

 - Änderungen an der Darstellung des Hintergrunds `UINavigationBar`, `UITabBar` oder `UIToolBar` möglicherweise eine Layoutdurchlauf, um die neue Darstellung zu beheben. Versucht, diese eindeutigkeitsmetrik innerhalb von einer `LayoutSubviews`, `UpdateConstraints`, `WillLayoutSubviews` oder `DidUpdateSubviews` Ereignis kann es zu einer unendlichen Layout-Schleife.
 - In tvos. außerdem wurden 10 Aufrufen der `RemoveGestureRecognizer` Methode eine `UIView` Objekt explizit bricht alle in Bearbeitung Gestenhandler-Erkennung.
 - Dargestellten View-Controller können jetzt die Darstellung der Statusleiste beeinflussen.
 - tvos. außerdem wurden 10 ist erforderlich, den Entwickler Aufrufen `base.AwakeFromNib` beim Erstellen von Unterklassen für `UIViewController` und überschreiben die `AwakeFromNib` Methode.
 - Apps mit benutzerdefinierten `UIView` Unterklassen, die außer Kraft setzen `LayoutSubviews` und modifizierte Seiten das Layout vor dem Aufruf `base.LayoutSubviews` möglicherweise eine unendliche Layout-Schleife in tvos. außerdem wurden 10 auslösen.
 - Richtung-spezifische oder flippable Bilder Assets sind keine kippen beim Zuweisen zu `UIButton` Objekte.





## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [Was ist neu in tvos. außerdem wurden 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
