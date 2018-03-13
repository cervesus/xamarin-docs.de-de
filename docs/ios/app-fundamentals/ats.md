---
title: App-Transportsicherheit
description: App-Transportsicherheit (ATS) erzwingt, sichere Verbindungen zwischen Internet-Ressourcen (z. B. die app-Back-End-Server) und Ihre app.
ms.topic: article
ms.prod: xamarin
ms.assetid: F8C5E444-2D05-4D9B-A2EF-EB052CD6F007
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/13/2017
ms.openlocfilehash: a4491f550369bbb8515635ecbb7c1c2b74de48cf
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="app-transport-security"></a>App-Transportsicherheit

_App-Transportsicherheit (ATS) erzwingt, sichere Verbindungen zwischen Internet-Ressourcen (z. B. die app-Back-End-Server) und Ihre app._

In diesem Artikel werden die Änderungen der Sicherheit, die Transportsicherheit für die App für eine app für iOS 9 erzwingt vorgestellt und [Bedeutung dies für Ihre Projekte Xamarin.iOS](#xamarinsupport), berücksichtigt der [ATS Konfigurationsoptionen](#config) und Es behandelt wird wie [opt-Out-of ATS](#optout) ATS, falls erforderlich. Da ATS standardmäßig aktiviert ist, lösen alle unsichere internetverbindungen eine Ausnahme in iOS 9-apps, (es sei denn, Sie sie explizit zugelassen haben).


## <a name="about-app-transport-security"></a>Informationen zu App-Transportsicherheit

Wie bereits erwähnt, wird sichergestellt, dass ATS, dass die gesamte Internetkommunikation in iOS 9 und OS X El Capitan entsprechen, um die Verbindung secure bewährte Methoden um versehentlichen Offenlegung vertraulicher Informationen entweder direkt über Ihre app oder eine Bibliothek, die es zu verhindern belegt.

Für vorhandene apps implementieren die `HTTPS` Protokoll, wann immer möglich. Sie sollten für neue Xamarin.iOS apps verwenden `HTTPS` ausschließlich bei der Kommunikation mit Ressourcen im Internet. Darüber hinaus muss forward Secrecy TLS Version 1.2 mit allgemeinen API-Kommunikation verschlüsselt werden.

Jede Verbindung mit [NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/), [CFUrl](https://developer.xamarin.com/api/type/CoreFoundation.CFUrl/) oder [NSUrlSession](https://developer.xamarin.com/api/type/Foundation.NSUrlSession/) verwendet ATS standardmäßig in apps für iOS 9 und OS X 10.11 (El Capitan) erstellt wurden.

## <a name="default-ats-behavior"></a>ATS Standardverhalten

Da ATS in apps für iOS 9 und OS X 10.11 (El Capitan), alle Verbindungen mit erstellt standardmäßig aktiviert ist. [NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/), [CFUrl](https://developer.xamarin.com/api/type/CoreFoundation.CFUrl/) oder [NSUrlSession](https://developer.xamarin.com/api/type/Foundation.NSUrlSession/) unterliegen werden ATS sicherheitsanforderungen. Wenn Ihre Verbindungen diese Anforderung nicht erfüllen, werden sie mit einer Ausnahme fehl.

### <a name="ats-connection-requirements"></a>Verbindungsanforderungen ATS

ATS erzwingt die folgenden Anforderungen für alle Internet-Verbindungen:

- Alle Verbindungs-Verschlüsselungen müssen forward Secrecy verwenden. Die Liste der zulässigen Chiffren unten anzuzeigen.
- Das Transport Layer Security (TLS)-Protokoll muss Version 1.2 oder höher sein.
- Mindestens ein SHA256-Fingerabdruck mit 2048-Bit größer RSA-Schlüssel oder ein 256-Bit- oder größer Verschlüsselung mit elliptischen Kurven (ECC) Schlüssel muss für alle Zertifikate verwendet werden.

Vorgang, da ATS in iOS 9 standardmäßig aktiviert ist, jeder Versuch, eine Verbindung herzustellen, die diese Anforderungen nicht erfüllt eine Ausnahme ausgelöst wird, führt. 

<a name="ATS-Compatible-Ciphers" />

### <a name="ats-compatible-ciphers"></a>Kompatible ATS-Verschlüsselungen

Die folgenden forward Secrecy Cipher Typ akzeptiert werden, indem ATS Internetkommunikation gesichert:

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

Weitere Informationen zum Arbeiten mit iOS interne Kommunikationsklassen finden Sie unter der Apple- [NSURLConnection-Klassenreferenz](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSURLConnection_Class/index.html#//apple_ref/doc/uid/TP40003755), [CFURL Verweis](https://developer.apple.com/library/prerelease/ios/documentation/CoreFoundation/Reference/CFURLRef/index.html#//apple_ref/doc/uid/20001206) oder [NSURLSession-Klassenreferenz ](https://developer.apple.com/library/prerelease/ios/documentation/Foundation/Reference/NSURLSession_class/index.html#//apple_ref/doc/uid/TP40013435).

<a name="xamarinsupport" />

## <a name="supporting-ats-in-xamarinios"></a>Unterstützen von ATS in Xamarin.iOS

Da ATS in iOS 9 und OS X El Capitan standardmäßig aktiviert ist, wenn Ihre app Xamarin.iOS oder Bibliothek oder Dienst verwendeten Verbindung mit dem Internet herstellt, Maßnahmen ergreifen müssen, oder Ihre Verbindungen führt dazu, eine Ausnahme ausgelöst.

Für eine vorhandene app Apple schlägt vor, Sie unterstützen die `HTTPS` Protokoll so bald wie möglich. Wenn Sie entweder nicht möglich, da Sie eine Verbindung herstellen a 3rd Webdienst Partei, die keine unterstützt `HTTPS` oder wenn unterstützen `HTTPS` wäre unpraktisch, Sie können opt-Out-of ATS. Finden Sie unter der [Opting-Out-of ATS](#optout) weiter unten für weitere Details.

Sie sollten für eine neue app Xamarin.iOS verwenden `HTTPS` ausschließlich bei der Kommunikation mit Ressourcen im Internet. Erneut Umständen (z. B. mit einem 3rd Party-Webdienst), in dem dies ist nicht möglich, und der ATS teilnehmen müssen.

Darüber hinaus erzwingt ATS allgemeine API-Kommunikation mit TLS Version 1.2 mit forward Secrecy verschlüsselt werden. Finden Sie unter der [ATS Verbindungsanforderungen](#ATS-Connection-Requirements) und [ATS kompatibel Chiffren](#ATS-Compatible-Ciphers) Abschnitten oben Weitere Details.

Obwohl Sie möglicherweise nicht vertraut sind mit TLS ([Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security)) ist der Nachfolger von SSL ([Secure Socket Layer](https://en.wikipedia.org/wiki/Transport_Layer_Security)) und stellt eine Auflistung von kryptografischen Protokolle über Sicherheit erzwingen Netzwerkverbindungen.

Die TLS-Ebene wird gesteuert durch den Webdienst, den Sie nutzen, und es ist daher außerhalb der app-Steuerelements. Sowohl die `HttpClient` und die `ModernHttpClient` automatisch das höchste Maß TLS-Verschlüsselung unterstützt, die vom Server verwendet werden sollen.

Je nach dem Server, Sie sind kommuniziert (insbesondere, wenn es sich um einen 3rd Party-Dienst ist), müssen Sie möglicherweise forward Secrecy deaktivieren, oder wählen Sie eine niedrigere TLS-Ebene. Finden Sie unter der [ATS-Optionen konfigurieren](#Configuring-ATS-Options) weiter unten für weitere Details.

> [!IMPORTANT]
> **Hinweis:** App Transportsicherheit gilt nicht für Xamarin-apps mit **verwaltet HTTPClient-Implementierungen**. Dies gilt für Verbindungen mit dem CFNetwork **HTTPClient-Implementierungen** oder **NSURLSession HTTPClient-Implementierungen** nur.

### <a name="setting-the-httpclient-implementation"></a>Die Implementierung für "HttpClient" festlegen

Doppelklicken Sie zum Festlegen von der Implementierung "HttpClient" von einer iOS-app auf die **Projekt** in der **Projektmappen-Explorer** So öffnen die **Projektoptionen**. Navigieren Sie zu **iOS-Build** und wählen Sie den gewünschten Client unter den **"HttpClient" Implementierung** Dropdownliste:

![](ats-images/client01.png "Festlegen der iOS-Build-Optionen")


#### <a name="managed-handler"></a>Verwaltete Handler

Der verwaltete Handler ist der vollständig verwalteter HttpClient-Handler, der ist mit früheren Versionen von Xamarin.iOS geliefert wurde, und der Standardhandler ist.

IT-Spezialisten:

- Es ist die am häufigsten mit Microsoft .NET und ältere Version der Xamarin kompatibel.

Nachteile:

- Es ist nicht vollständig in integriert iOS (z. B. es um TLS 1.0 beschränkt ist).
- Es ist in der Regel sehr viel langsamer als die systemeigene APIs.
- Erfordert mehr verwalteten Code, und größeren apps erstellt.

#### <a name="cfnetwork-handler"></a>CFNetwork Handler

Der Handler CFNetwork basierend basiert auf dem systemeigenen `CFNetwork` Framework.

IT-Spezialisten:

- Verwendet systemeigene API für eine bessere Leistung und kleinere ausführbare Größen.
- Bietet Unterstützung für neuere Standards wie TLS 1.2.

Nachteile:

- Erfordert iOS 6 oder höher.
- Der WatchOS nicht verfügbar.
- Einige Funktionen für "HttpClient" und die Optionen sind nicht verfügbar.

#### <a name="nsurlsession-handler"></a>NSUrlSession Handler

Der Handler NSUrlSession basierend basiert auf dem systemeigenen `NSUrlSession` API.

IT-Spezialisten:

- Verwendet systemeigene API für eine bessere Leistung und kleinere ausführbare Größen.
- Bietet Unterstützung für neuere Standards wie TLS 1.2.

Nachteile:

- Erfordert iOS 7 oder höher.
- Einige Funktionen für "HttpClient" und die Optionen sind nicht verfügbar. 

## <a name="diagnosing-ats-issues"></a>Diagnostizieren von Problemen ATS

Bei einem Verbindungsversuch mit dem Internet, entweder direkt oder über eine iOS 9-Webansicht, erhalten Sie möglicherweise einen Fehler in der Form:

> App-Transportsicherheit hat ressourcenauslastung einem Klartext-HTTP (http://www.-the-blocked-domain.com) blockiert, da sie unsicher ist. Über die Datei "Info.plist"-Datei Ihrer app können temporäre Ausnahmen konfiguriert werden.

In iOS9 erzwingt App Transport Sicherheit (ATS) sichere Verbindungen zwischen Internet-Ressourcen (z. B. die app-Back-End-Server) und Ihre app. Darüber hinaus ATS erfordert Kommunikation unter Verwendung der `HTTPS` Protokoll und allgemeine API-Kommunikation mit TLS Version 1.2 mit forward Secrecy verschlüsselt werden.

Da ATS in apps für iOS 9 und OS X 10.11 (El Capitan), alle Verbindungen mit erstellt standardmäßig aktiviert ist. `NSURLConnection`, `CFURL` oder `NSURLSession` unterliegen ATS sicherheitsanforderungen sein wird. Wenn Ihre Verbindungen diese Anforderung nicht erfüllen, werden sie mit einer Ausnahme fehl.

Apple bietet außerdem die [TLSTool Beispiel-App](https://developer.apple.com/library/mac/samplecode/sc1236/Introduction/Intro.html#//apple_ref/doc/uid/DTS40014927-Intro-DontLinkElementID_2) kompiliert werden kann (oder optional transcodiert Xamarin und c#) und ATS/TLS-Diagnoseproblemen verwendet. Finden Sie unter der [Opting-Out-of ATS](#optout) im folgenden Abschnitt, Informationen zur Behebung dieses Problems.


<a name="config" />

## <a name="configuring-ats-options"></a>Konfigurieren von ATS-Optionen

Sie können einige der Funktionen von ATS konfigurieren, indem Sie Werte für bestimmte Schlüssel in Ihrer app **"Info.plist"** Datei. Die folgenden Schlüssel sind verfügbar für die Steuerung ATS (_anzeigen, wie sie geschachtelt sind_):

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

Jeder Schlüssel weist die folgenden Typ und die Bedeutung:

- **NSAppTransportSecurity** (`Dictionary`) – enthält alle von der Einstellungsschlüssel und Werte für ATS.
- **NSAllowsArbitraryLoads** (`Boolean`) – Wenn `YES` ATS für eine beliebige Domäne deaktiviert **nicht** abgelesen `NSExceptionDomains`. Für aufgelisteten Domänen werden die angegebene Sicherheitseinstellungen verwendet werden.
- **NSAllowsArbitraryLoadsInWebContent** (`Boolean`) - If `YES` will allow web pages to load correctly while Apple Transport Security (ATS) protection is still enabled for the rest of the app.
- **NSExceptionDomains** (`Dictionary`) – eine Auflistung von Domänen, und die Sicherheitseinstellungen, die für eine bestimmte Domäne ATS verwendet werden soll.
- **< Domain-name-for-exception-as-string >** (`Dictionary`) – eine Auflistung von Ausnahmen für eine bestimmte Domäne (z. b. `www.xamarin.com`) angezeigt wird.
- **NSExceptionMinimumTLSVersion** (`String`) – die TLS-Mindestversion entweder als `TLSv1.0`, `TLSv1.1` oder `TLSv1.2` (Dies ist die Standardeinstellung).
- **NSExceptionRequiresForwardSecrecy** (`Boolean`) - If `NO` the domain does not have to use a cipher with forward security. Der Standardwert ist `YES`.
- **NSExceptionAllowsInsecureHTTPLoads** (`Boolean`) - If `NO` (the default) all communications with this domain must be in the `HTTPS` protocol.
- **NSRequiresCertificateTransparency** (`Boolean`) - If `YES` the domain’s Secure Sockets Layer (SSL) must include valid transparency data. Der Standardwert ist `NO`.
- **NSIncludesSubdomains** (`Boolean`) – Wenn `YES` diese Einstellungen außer Kraft setzen alle Unterdomänen dieser Domäne. Der Standardwert ist `NO`.
- **NSThirdPartyExceptionMinimumTLSVersion** (`String`) – die TLS-Protokollversion, die verwendet werden, wenn die Domäne einen 3rd Party Dienst außerhalb der Kontrolle des Entwicklers ist.
- **NSThirdPartyExceptionRequiresForwardSecrecy** (`Boolean`) - If `YES` a 3rd party domain requires forward secrecy.
- **NSThirdPartyExceptionAllowsInsecureHTTPLoads** (`Boolean`) - If `YES` the ATS will allow non-secure communication with 3rd party domains.

<a name="optout" />

### <a name="opting-out-of-ats"></a>Die Entscheidung Out-of ATS

Beim Apple hoch schlägt mit der `HTTPS` Protokoll und sichere Kommunikation zu Internet basierten Informationen, kann es Fälle, in denen dies nicht immer möglich. Angenommen, Sie sind bei der Kommunikation mit einem 3rd Party-Webdienst oder Internet übermittelt von werbeeinblendungen in Ihrer app.

Wenn Ihre app Xamarin.iOS eine Anforderung mit einer unsicheren Domäne vornehmen muss, folgende Änderungen auf Ihre app **"Info.plist"** Datei wird die Sicherheitsstandardwerte, die für eine bestimmte Domäne ATS erzwingt deaktivieren:

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

Innerhalb von Visual Studio für Mac, doppelklicken Sie auf die `Info.plist` in der Datei die **Projektmappen-Explorer**, wechseln Sie zu der **Quelle** anzeigen und die oben aufgeführten Schlüssel hinzufügen:

[![](ats-images/ats01.png "Die Datenquellensicht an, der die Datei "Info.plist"")](ats-images/ats01.png#lightbox)


Wenn Ihre app zum Laden und Anzeigen von Webinhalten von Standorten nicht sicheren benötigt, fügen Sie die folgenden für Ihre app **"Info.plist"** Datei zum Zulassen von Webseiten ordnungsgemäß geladen werden, während der Apple-Transport-Sicherheit (ATS)-Schutz für die restlichen weiterhin aktiviert ist der app:

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key> NSAllowsArbitraryLoadsInWebContent</key>
    <true/>
</dict>
```

Optional können Sie die folgenden Änderungen vornehmen, auf Ihre app **"Info.plist"** Datei ATS für alle Domänen und Internetkommunikation vollständig zu deaktivieren:

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

Innerhalb von Visual Studio für Mac, doppelklicken Sie auf die `Info.plist` in der Datei die **Projektmappen-Explorer**, wechseln Sie zu der **Quelle** anzeigen und die oben aufgeführten Schlüssel hinzufügen:

[![](ats-images/ats02.png "Die Datenquellensicht an, der die Datei "Info.plist"")](ats-images/ats02.png#lightbox)

> [!IMPORTANT]
> **Hinweis:** Wenn Ihre Anwendung eine Verbindung mit der eine unsichere Website erfordert, sollten Sie **immer** Geben Sie die Domäne als eine Ausnahme mit `NSExceptionDomains` anstatt durch das ATS deaktivieren vollständig mit `NSAllowsArbitraryLoads`. `NSAllowsArbitraryLoads` sollte nur im Notfall Extremsituationen verwendet werden.




ATS wieder deaktivieren sollten _nur_ als letzte Möglichkeit verwendet werden, wenn der Wechsel auf sichere Verbindungen nicht praktikabel oder nicht verfügbar ist.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde eingeführt App Transport Sicherheit (ATS) und die Möglichkeit, die es erzwingt eine sichere Kommunikation mit dem Internet beschrieben. Zuerst behandelt wir die Änderungen, die für eine Xamarin.iOS-app unter iOS 9 ATS erfordert. Klicken Sie dann behandelt wir steuernde ATS-Funktionen und Optionen. Schließlich behandelt wir ATS in Ihrer app Xamarin.iOS abwählen.



## <a name="related-links"></a>Verwandte Links

- [iOS 9-Beispiele](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
