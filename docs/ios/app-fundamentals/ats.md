---
title: App-Transportsicherheit in Xamarin.iOS
description: App-Transport-Security (ATS) setzt sichere Verbindungen zwischen Ressourcen für Internet (z. B. die app Back-End-Server) und Ihrer app.
ms.prod: xamarin
ms.assetid: F8C5E444-2D05-4D9B-A2EF-EB052CD6F007
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/13/2017
ms.openlocfilehash: 0645b326576a68c97479bc5b59aabaa104f87ae2
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114261"
---
# <a name="app-transport-security-in-xamarinios"></a>App-Transportsicherheit in Xamarin.iOS

_App-Transport-Security (ATS) setzt sichere Verbindungen zwischen Ressourcen für Internet (z. B. die app Back-End-Server) und Ihrer app._

In diesem Artikel werden die Änderungen der Sicherheit, die App-Transportsicherheit für eine app für iOS 9 erzwingt vorgestellt und [was dies für Ihre Xamarin.iOS-Projekte bedeutet](#xamarinsupport), berücksichtigt die [ATS-Konfigurationsoptionen](#config) und Es behandelt wie [opt-Out-of ATS](#optout) ATS, falls erforderlich. Da ATS standardmäßig aktiviert ist, lösen alle unsicheren internetverbindungen (es sei denn, Sie es explizit zugelassen haben) eine Ausnahme in iOS 9-apps.


## <a name="about-app-transport-security"></a>Informationen zu App-Transportsicherheit

Wie bereits erwähnt, stellt sicher ATS, dass die gesamte Internetkommunikation in iOS 9 und OS X El Capitan entsprechen, um die Verbindung secure best Practices und verhindert versehentliche Offenlegung vertraulicher Informationen, die entweder direkt über Ihre app oder eine Bibliothek, die es verbraucht.

Für vorhandene apps geben, implementieren die `HTTPS` Protokoll, wann immer möglich. Verwenden Sie für den neuen Xamarin.iOS-apps, `HTTPS` ausschließlich bei der Kommunikation mit Ressourcen im Internet. Darüber hinaus muss die allgemeine Kommunikation zwischen forward Secrecy mit Version 1.2 von TLS verschlüsselt werden.

Verbindung mit [NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/), [CFUrl](https://developer.xamarin.com/api/type/CoreFoundation.CFUrl/) oder [NSUrlSession](https://developer.xamarin.com/api/type/Foundation.NSUrlSession/) ATS standardmäßig verwendet wird in apps für iOS 9 und OS X 10.11 (El Capitan) erstellt wurden.

## <a name="default-ats-behavior"></a>ATS-Standardverhalten

Da ATS, wird standardmäßig in apps aktiviert ist, die für iOS 9 und OS X 10.11 (El Capitan), alle Verbindungen mit [NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/), [CFUrl](https://developer.xamarin.com/api/type/CoreFoundation.CFUrl/) oder [NSUrlSession](https://developer.xamarin.com/api/type/Foundation.NSUrlSession/) wird vorgenommen werden. ATS-sicherheitsanforderungen. Wenn Ihre Verbindungen diese Anforderung nicht erfüllen, werden sie mit einer Ausnahme fehlschlagen.

### <a name="ats-connection-requirements"></a>Verbindung ATS-Anforderungen

ATS erzwingt die folgenden Anforderungen für alle Internet-Verbindungen:

- Forward Secrecy müssen alle Verschlüsselungen für Verbindung verwenden. Finden Sie in der Liste der zulässigen Verschlüsselungen unten.
- Das Protokoll Transport Layer Security (TLS) muss die Version 1.2 oder höher sein.
- Mit einem 2048-Bit oder größer RSA-Schlüssel oder einen 256-Bit oder größer Verschlüsselung mit elliptischen Kurven (ECC) Schlüssel mit mindestens SHA256-Fingerabdruck muss für alle Zertifikate verwendet werden.

In diesem Fall wird ATS in iOS 9 standardmäßig aktiviert ist, jeder Versuch, eine Verbindung herzustellen, die diese Anforderungen nicht erfüllt führen eine Ausnahme ausgelöst wird. 

<a name="ATS-Compatible-Ciphers" />

### <a name="ats-compatible-ciphers"></a>Kompatible ATS-Verschlüsselungen

Der folgende forward Secrecy Cipher Typ akzeptiert werden, indem ATS Internetkommunikation gesichert:

- `TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384`
- `TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256`
- `TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384`
- `TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA`
- `TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256`
- `TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA`
- `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`
- `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`
- `TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384`
- `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256`
- `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA`

Weitere Informationen zum Arbeiten mit iOS-internetklassen Kommunikation finden Sie unter Apple [NSURLConnection-Klassenreferenz](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSURLConnection_Class/index.html#//apple_ref/doc/uid/TP40003755), [CFURL Verweis](https://developer.apple.com/library/prerelease/ios/documentation/CoreFoundation/Reference/CFURLRef/index.html#//apple_ref/doc/uid/20001206) oder [NSURLSession-Klassenreferenz ](https://developer.apple.com/library/prerelease/ios/documentation/Foundation/Reference/NSURLSession_class/index.html#//apple_ref/doc/uid/TP40013435).

<a name="xamarinsupport" />

## <a name="supporting-ats-in-xamarinios"></a>Unterstützung von ATS in Xamarin.iOS

Da ATS in iOS 9 und OS X El Capitan, standardmäßig aktiviert ist, wenn Ihre Xamarin.iOS-app oder Bibliothek oder Dienst verwendeten Verbindung mit dem Internet herstellt, müssen Sie eine Aktion auszuführen, oder Ihre Verbindungen führt eine Ausnahme ausgelöst wird.

Für eine vorhandene app, Apple empfiehlt Sie unterstützen die `HTTPS` Protokoll so bald wie möglich. Wenn Sie entweder nicht möglich, da Sie eine Verbindung herstellen a 3rd Webdienst Parteien, die nicht unterstützt `HTTPS` oder beim supporten `HTTPS` wäre unpraktisch, Sie können opt-Out-of ATS. Finden Sie unter den [Opting skalierten ATS](#optout) Informationen weiter unten im Abschnitt.

Verwenden Sie für eine neue Xamarin.iOS-app `HTTPS` ausschließlich bei der Kommunikation mit Ressourcen im Internet. In diesem Fall Umständen (z. B. mit einem 3rd Party-Webdienst), in denen dies nicht möglich ist, und müssen Sie ATS deaktivieren.

Darüber hinaus erzwingt ATS allgemeine Kommunikation zwischen forward Secrecy mit Version 1.2 von TLS verschlüsselt werden. Finden Sie unter den [ATS-Verbindungsanforderungen](#ATS-Connection-Requirements) und [kompatibel ATS-Verschlüsselungen](#ATS-Compatible-Ciphers) Abschnitte über weitere Details.

Obwohl Sie möglicherweise nicht vertraut sind, mit TLS ([Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security)) ist der Nachfolger von SSL ([Secure Socket Layer](https://en.wikipedia.org/wiki/Transport_Layer_Security)) und stellt eine Auflistung von kryptografischen Protokollen zum Erzwingen der Sicherheit über Netzwerkverbindungen.

Die TLS-Ebene wird gesteuert, durch den Webdienst, den Sie nutzen, und es ist daher außerhalb der app-Steuerelemente. Sowohl die `HttpClient` und `ModernHttpClient` automatisch die höchste Ebene des TLS-Verschlüsselung unterstützt, die vom Server verwendet werden sollen.

Je nachdem, welche auf dem Server Sie sprechen auf (insbesondere, wenn es sich um einen 3rd Party-Dienst ist), müssen Sie möglicherweise forward Secrecy deaktivieren oder eine niedrigere TLS-Ebene auswählen. Finden Sie unter den [ATS-Optionen konfigurieren](#Configuring-ATS-Options) Informationen weiter unten im Abschnitt.

> [!IMPORTANT]
> App-Transportsicherheit gelten nicht für Xamarin-apps mithilfe von **verwaltet HTTPClient-Implementierungen**. Dies gilt für Verbindungen mit CFNetwork **HTTPClient-Implementierungen** oder **NSURLSession HTTPClient-Implementierungen** nur.

### <a name="setting-the-httpclient-implementation"></a>Festlegen der HTTPClient-Implementierung

Zum Festlegen der HTTPClient-Implementierung, die von einer iOS-app verwendet, doppelklicken Sie auf die **Projekt** in die **Projektmappen-Explorer** zum Öffnen der **Projektoptionen**. Navigieren Sie zu **iOS-Build** , und wählen Sie den gewünschten Typ unter der **HttpClient-Implementierung** Dropdownliste:

![](ats-images/client01.png "Festlegen der iOS-Buildoptionen")


#### <a name="managed-handler"></a>Verwalteten Handler

Der verwaltete Handler ist der vollständig verwaltete "HttpClient"-Handler, der hat mit früheren Versionen von Xamarin.iOS ausgeliefert wurde, und ist der Standard-Handler.

-Experten:

- Es ist die am häufigsten kompatibel mit Microsoft .NET und der älteren Version von Xamarin.

Nachteile:

- Es ist vollständig nicht integriert mit iOS (z. B. auf TLS 1.0 beschränkt werden kann).
- Es ist in der Regel sehr viel langsamer ist als die nativen APIs.
- Mehr von verwalteten Code erfordert und größeren apps erstellt.

#### <a name="cfnetwork-handler"></a>CFNetwork-Handler

Der Handler für CFNetwork basierend basiert darauf, dass die systemeigene `CFNetwork` Framework.

-Experten:

- Verwendet systemeigenen API für eine bessere Leistung und kleinere ausführbare Datei an.
- Bietet Unterstützung für neuere Standards wie z. B. TLS 1.2.

Nachteile:

- Erfordert iOS 6 oder höher.
- Der WatchOS nicht verfügbar.
- Einige Features von "HttpClient" und die Optionen sind nicht verfügbar.

#### <a name="nsurlsession-handler"></a>Von NSUrlSession-Handler

Der Handler von NSUrlSession basierend basiert darauf, dass die systemeigene `NSUrlSession` API.

-Experten:

- Verwendet systemeigenen API für eine bessere Leistung und kleinere ausführbare Datei an.
- Bietet Unterstützung für neuere Standards wie z. B. TLS 1.2.

Nachteile:

- Erfordert iOS 7 oder höher.
- Einige Features von "HttpClient" und die Optionen sind nicht verfügbar. 

## <a name="diagnosing-ats-issues"></a>Diagnose von ATS-Problemen

Wenn Sie versuchen, eine Verbindung mit dem Internet herstellen, entweder direkt oder über eine Webansicht in iOS 9, erhalten Sie möglicherweise einen Fehler in der Form:

> Transportsicherheit für die App wurde eine HTTP-Klartext blockiert (http://www.-the-blocked-domain.com) ressourcenauslastung, da dies unsicher ist. Vorübergehende Ausnahmen können über "Info.plist"-Datei Ihrer app konfiguriert werden.

In iOS9 erzwingt App Transport Security (ATS) über sichere Verbindungen zwischen Ressourcen für Internet (z. B. die app Back-End-Server) und Ihrer app. Darüber hinaus ATS erfordert Kommunikation unter Verwendung der `HTTPS` -Protokoll und allgemeine Kommunikation zwischen forward Secrecy mit Version 1.2 von TLS verschlüsselt werden.

Da ATS, wird standardmäßig in apps aktiviert ist, die für iOS 9 und OS X 10.11 (El Capitan), alle Verbindungen mit `NSURLConnection`, `CFURL` oder `NSURLSession` ATS-sicherheitsanforderungen unterliegen. Wenn Ihre Verbindungen diese Anforderung nicht erfüllen, werden sie mit einer Ausnahme fehlschlagen.

Apple bietet außerdem die [TLSTool-Beispiel-App](https://developer.apple.com/library/mac/samplecode/sc1236/Introduction/Intro.html#//apple_ref/doc/uid/DTS40014927-Intro-DontLinkElementID_2) kompiliert werden kann (oder optional transcodiert mit Xamarin und C#) und verwendet, um die ATS/TLS-Probleme zu diagnostizieren. Informieren Sie sich die [Opting skalierten ATS](#optout) Informationen zur Lösung dieses Problems weiter unten im Abschnitt.


<a name="config" />

## <a name="configuring-ats-options"></a>Konfigurieren von ATS-Optionen

Sie können einige der Features von ATS konfigurieren, durch Festlegen von Werten für bestimmte Schlüssel in Ihrer app **"Info.plist"** Datei. Die folgenden Schlüssel sind für die Steuerung von ATS verfügbar (_eingezogen angezeigt, wie sie geschachtelt sind_):

```csharp
NSAppTransportSecurity
    NSAllowsArbitraryLoads
    NSAllowsArbitraryLoadsInWebContent
    NSExceptionDomains
    <domain-name-for-exception-as-string>
        NSExceptionMinimumTLSVersion
        NSExceptionRequiresForwardSecrecy
        NSExceptionAllowsInsecureHTTPLoads
        NSRequiresCertificateTransparency
        NSIncludesSubdomains
        NSThirdPartyExceptionMinimumTLSVersion
        NSThirdPartyExceptionRequiresForwardSecrecy
        NSThirdPartyExceptionAllowsInsecureHTTPLoads
```

Jeder Schlüssel hat den folgenden Typ und die Bedeutung:

- **NSAppTransportSecurity** (`Dictionary`) – enthält alle Einstellungsschlüssel und Werte für ATS.
- **NSAllowsArbitraryLoads** (`Boolean`) – Wenn `YES` ATS für eine beliebige Domäne deaktiviert **nicht** in aufgeführten `NSExceptionDomains`. Für die aufgelisteten Domänen werden die angegebenen Sicherheitseinstellungen verwendet werden.
- **NSAllowsArbitraryLoadsInWebContent** (`Boolean`) – Wenn `YES` können Webseiten ordnungsgemäß geladen werden, während die Apple Transport Security (ATS) Schutz für den Rest der app immer noch aktiviert ist.
- **NSExceptionDomains** (`Dictionary`) – eine Auflistung von Domänen, und die Sicherheitseinstellungen, die ATS für eine bestimmte Domäne verwendet werden soll.
- **< Domain-name-for-exception-as-string >** (`Dictionary`) – eine Auflistung von Ausnahmen für eine bestimmte Domäne (z. b. `www.xamarin.com`).
- **NSExceptionMinimumTLSVersion** (`String`) – die TLS-Mindestversion entweder als `TLSv1.0`, `TLSv1.1` oder `TLSv1.2` (Dies ist die Standardeinstellung).
- **NSExceptionRequiresForwardSecrecy** (`Boolean`) – Wenn `NO` die Domäne muss kein Verschlüsselungsverfahren mit forward-Sicherheit verwenden. Der Standardwert ist `YES`.
- **NSExceptionAllowsInsecureHTTPLoads** (`Boolean`) - If `NO` (the default) all communications with this domain must be in the `HTTPS` protocol.
- **NSRequiresCertificateTransparency** (`Boolean`) – Wenn `YES` der Domäne Secure Sockets Layer (SSL) muss gültige Transparenzdaten enthalten. Der Standardwert ist `NO`.
- **NSIncludesSubdomains** (`Boolean`) – Wenn `YES` diese Einstellungen überschreiben alle Unterdomänen von dieser Domäne. Der Standardwert ist `NO`.
- **NSThirdPartyExceptionMinimumTLSVersion** (`String`) – die TLS-Version, die verwendet werden, wenn die Domäne eines 3rd Party-Diensts außerhalb der Kontrolle des Entwicklers ist.
- **NSThirdPartyExceptionRequiresForwardSecrecy** (`Boolean`) – Wenn `YES` eine 3rd Party-Domäne erfordert forward Secrecy.
- **NSThirdPartyExceptionAllowsInsecureHTTPLoads** (`Boolean`) - If `YES` the ATS will allow non-secure communication with 3rd party domains.

<a name="optout" />

### <a name="opting-out-of-ats"></a>Deaktivieren der Out-of ATS

Beim Apple hoch mit empfiehlt die `HTTPS` -Protokoll und eine sichere Kommunikation zu Internet basierten Informationen, kann es Fälle, in denen dies nicht immer möglich ist. Angenommen, Sie bei der Kommunikation mit einem 3rd Party-Webdienst oder Internet werbeeinblendungen in Ihre app bereitgestellt.

Wenn Ihre Xamarin.iOS-app eine Anforderung einer unsicheren Domäne vornehmen muss, die folgenden Änderungen an Ihrer app **"Info.plist"** Datei deaktiviert die Sicherheitsstandards, die ATS für eine bestimmte Domäne erzwingt:

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSExceptionDomains</key>
    <dict>
        <key>www.the-domain-name.com</key>
        <dict>
            <key>NSExceptionMinimumTLSVersion</key>
            <string>TLSv1.0</string>
            <key>NSExceptionRequiresForwardSecrecy</key>
            <false/>
            <key>NSExceptionAllowsInsecureHTTPLoads</key>
            <true/>
            <key>NSIncludesSubdomains</key>
            <true/>
        </dict>
    </dict>
</dict>
```

In Visual Studio für Mac, doppelklicken Sie auf die `Info.plist` Datei die **Projektmappen-Explorer**, wechseln Sie zu der **Quelle** anzuzeigen, und fügen Sie die oben aufgeführten Schlüssel:

[![](ats-images/ats01.png "Die Datenquellensicht an der Datei \"Info.plist\"")](ats-images/ats01.png#lightbox)


Wenn Ihre app muss zum Laden und Anzeigen von Webinhalten von nicht sicheren Websites, fügen Sie Folgendes zu Ihrer app **"Info.plist"** Datei können Sie Webseiten ordnungsgemäß geladen werden, während die Apple Transport Security (ATS) Schutz für den Rest weiterhin aktiviert ist der app:

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key> NSAllowsArbitraryLoadsInWebContent</key>
    <true/>
</dict>
```

Optional können Sie die folgenden Änderungen vornehmen, um Ihrer app **"Info.plist"** Datei, die ATS für alle Domänen und Internetkommunikation vollständig zu deaktivieren:

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

In Visual Studio für Mac, doppelklicken Sie auf die `Info.plist` Datei die **Projektmappen-Explorer**, wechseln Sie zu der **Quelle** anzuzeigen, und fügen Sie die oben aufgeführten Schlüssel:

[![](ats-images/ats02.png "Die Datenquellensicht an der Datei \"Info.plist\"")](ats-images/ats02.png#lightbox)

> [!IMPORTANT]
> Wenn Ihre Anwendung eine Verbindung mit einer nicht sicheren Website erfordert, sollten Sie **immer** Geben Sie die Domäne als eine Ausnahme mit `NSExceptionDomains` deaktivieren, dass ATS vollständig mit `NSAllowsArbitraryLoads`. `NSAllowsArbitraryLoads` sollte nur im Notfall extrem verwendet werden.




In diesem Fall deaktivieren ATS sollten _nur_ als letzten Ausweg verwendet werden, wenn der Wechsel zu sicheren Verbindungen nicht verfügbar oder nicht praktikabel ist.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat App Transport Security (ATS) eingeführt und beschrieben die Möglichkeit, die es erzwingt eine sichere Kommunikation mit dem Internet. Zuerst behandelt es die Änderungen, die ATS für eine Xamarin.iOS-app unter iOS 9 erforderlich sind. Klicken Sie dann behandelt wir steuern ATS-Funktionen und Optionen. Abschließend werden wir ATS in Ihrer Xamarin.iOS-app deaktivieren.



## <a name="related-links"></a>Verwandte Links

- [iOS 9-Beispiele](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
