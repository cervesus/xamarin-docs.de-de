---
title: App-Transport Sicherheit in xamarin. IOS
description: App-Transport Sicherheit (app Transport Security, ATS) erzwingt sichere Verbindungen zwischen Internetressourcen (z. b. dem Back-End-Server der APP) und ihrer app.
ms.prod: xamarin
ms.assetid: F8C5E444-2D05-4D9B-A2EF-EB052CD6F007
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/13/2017
ms.openlocfilehash: 62ccaea83a3648c5d9b0a029b3a22d136c4f2cee
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68649413"
---
# <a name="app-transport-security-in-xamarinios"></a>App-Transport Sicherheit in xamarin. IOS

_App-Transport Sicherheit (app Transport Security, ATS) erzwingt sichere Verbindungen zwischen Internetressourcen (z. b. dem Back-End-Server der APP) und ihrer app._

In diesem Artikel werden die Sicherheitsänderungen vorgestellt, die von der APP-Transport Sicherheit für eine IOS 9-APP erzwungen werden, und das bedeutet, dass [für Ihre xamarin. IOS-Projekte](#xamarinsupport)die [Optionen der ATS](#config) [-](#optout) Konfiguration abgedeckt werden. Falls erforderlich. Da ATS standardmäßig aktiviert ist, wird bei allen nicht sicheren Internetverbindungen eine Ausnahme in ios 9-apps ausgelöst (es sei denn, Sie haben Sie explizit zugelassen).


## <a name="about-app-transport-security"></a>Informationen zur APP-Transport Sicherheit

Wie bereits erwähnt, stellt ATS sicher, dass die gesamte Internetkommunikation in ios 9 und OS X El Capitan den bewährten Methoden der sicheren Verbindung entspricht. Dadurch wird die versehentliche Offenlegung vertraulicher Informationen entweder direkt über Ihre APP oder eine Bibliothek verhindert, Gste.

Implementieren Sie für vorhandene apps das `HTTPS` Protokoll, wann immer dies möglich ist. Bei neuen xamarin. IOS-apps sollten Sie exklusiv `HTTPS` verwenden, wenn Sie mit Internetressourcen kommunizieren. Darüber hinaus muss die API-Kommunikation auf hoher Ebene mit der TLS-Version 1,2 mit vorwärts Geheimnis verschlüsselt werden.

Alle Verbindungen, die mit " [NSURLConnection](xref:Foundation.NSUrlConnection)", " [cfurl](xref:CoreFoundation.CFUrl) " oder " [nsurlsession](xref:Foundation.NSUrlSession) " hergestellt wurden, verwenden standardmäßig in apps, die für IOS 9 und OS X 10,11 (El Capitan) erstellt wurden.

## <a name="default-ats-behavior"></a>Standardverhalten bei ATS

Da ATS in apps, die für IOS 9 und OS X 10,11 (El Capitan) erstellt werden, standardmäßig aktiviert ist, gelten für alle Verbindungen, die [NSURLConnection](xref:Foundation.NSUrlConnection), [cfurl](xref:CoreFoundation.CFUrl) oder [nsurlsession](xref:Foundation.NSUrlSession) verwenden, Sicherheitsanforderungen. Wenn Ihre Verbindungen diese Anforderung nicht erfüllen, schlagen Sie mit einer Ausnahme fehl.

### <a name="ats-connection-requirements"></a>Anforderungen an Verbindungsanforderungen

ATS erzwingt die folgenden Anforderungen für alle Internetverbindungen:

- Alle Verbindungs Chiffren müssen ein vorwärts Geheimnis verwenden. Weitere Informationen finden Sie unten in der Liste der akzeptierten Chiffren.
- Das TLS-Protokoll (Transport Layer Security) muss mindestens eine Version 1,2 aufweisen.
- Für alle Zertifikate muss mindestens ein SHA256-Fingerabdruck mit einem 2048-Bit-oder einem höheren RSA-Schlüssel oder einem 256-Bit-oder einem höheren Elliptic Curve-Schlüssel (ECC) verwendet werden.

Da ATS standardmäßig in ios 9 aktiviert ist, wird bei jedem Versuch, eine Verbindung herzustellen, die diese Anforderungen nicht erfüllt, eine Ausnahme ausgelöst. 

<a name="ATS-Compatible-Ciphers" />

### <a name="ats-compatible-ciphers"></a>Mit ATS kompatible Chiffren

Der folgende Forward-Geheimnis Chiffre wird von der von ATS gesicherten Internetkommunikation akzeptiert:

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

Weitere Informationen zum Arbeiten mit IOS-Internet Kommunikations Klassen finden Sie in der Referenz zur [NSURLConnection-Klasse](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSURLConnection_Class/index.html#//apple_ref/doc/uid/TP40003755)von Apple, in der [cfurl-Referenz](https://developer.apple.com/library/prerelease/ios/documentation/CoreFoundation/Reference/CFURLRef/index.html#//apple_ref/doc/uid/20001206) oder in der [nsurlsession-Klassenreferenz](https://developer.apple.com/library/prerelease/ios/documentation/Foundation/Reference/NSURLSession_class/index.html#//apple_ref/doc/uid/TP40013435).

<a name="xamarinsupport" />

## <a name="supporting-ats-in-xamarinios"></a>Unterstützung von ATS in xamarin. IOS

Da ATS in ios 9 und OS X El Capitan standardmäßig aktiviert ist, müssen Sie, wenn Ihre xamarin. IOS-APP oder eine beliebige Bibliothek oder einen beliebigen Dienst eine Verbindung mit dem Internet herstellt, eine bestimmte Aktion durchführen. andernfalls wird eine Ausnahme ausgelöst.

Für eine vorhandene App schlägt Apple vor, das `HTTPS` Protokoll so bald wie möglich zu unterstützen. Wenn Sie dies nicht tun können, weil Sie eine Verbindung mit einem Drittanbieter-Webdienst `HTTPS` herstellen, der `HTTPS` nicht unterstützt, oder wenn die Unterstützung nicht praktikabel wäre, können Sie sich gegen Sie entscheiden. Weitere Informationen finden Sie weiter unten im Abschnitt " [Opt-out of ATS](#optout) ".

Für eine neue xamarin. IOS-App sollten Sie exklusiv `HTTPS` verwenden, wenn Sie mit Internetressourcen kommunizieren. Auch hier gibt es möglicherweise Situationen (z. b. die Verwendung eines Drittanbieter-Webdiensts), in denen dies nicht möglich ist, und Sie müssen sich gegen Sie entscheiden.

Darüber hinaus erzwingt ATS eine allgemeine API-Kommunikation, die mit der TLS-Version 1,2 mit vorwärts Geheimnis verschlüsselt werden soll. Weitere Informationen finden Sie in den Abschnitten zu den in den [Anforderungen zu Verbindungsanforderungen](#ats-connection-requirements) und mit [ATS kompatiblen Chiffren](#ats-compatible-ciphers) .

Obwohl Sie ggf. nicht mit TLS ([Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security)) vertraut sind, handelt es sich um den Nachfolger von SSL ([Secure Socket Layer](https://en.wikipedia.org/wiki/Transport_Layer_Security)) und stellt eine Sammlung kryptografischer Protokolle bereit, um die Sicherheit von Netzwerkverbindungen zu erzwingen.

Die TLS-Ebene wird vom verwendeten Webdienst gesteuert und ist daher außerhalb der APP-Kontrolle. `HttpClient` Sowohl`ModernHttpClient` als auch sollten automatisch die höchste Ebene der TLS-Verschlüsselung verwenden, die vom Server unterstützt wird.

Abhängig von dem Server, mit dem Sie kommunizieren (insbesondere wenn es sich um einen Drittanbieter Dienst handelt), müssen Sie möglicherweise das vorwärts Geheimnis deaktivieren oder eine niedrigere TLS-Ebene auswählen. Weitere Informationen finden Sie unten im Abschnitt [Konfigurieren von ATS-Optionen](#configuring-ats-options) .

> [!IMPORTANT]
> Die APP-Transport Sicherheit gilt nicht für xamarin-apps, die **verwaltete httpclient-Implementierungen**verwenden. Es gilt nur für Verbindungen mit CFNetwork- **HttpClient-Implementierungen** oder **nsurlsession httpclient-Implementierungen** .

### <a name="setting-the-httpclient-implementation"></a>Festlegen der httpclient-Implementierung

Zum Festlegen der httpclient-Implementierung, die von einer IOS-App verwendet wird, doppelklicken Sie auf das **Projekt** im **Projektmappen-Explorer** , um die **Projektoptionen**zu öffnen. Navigieren Sie zu **IOS Build** , und wählen Sie in der Dropdown Liste **HttpClient-Implementierung** den gewünschten Clienttyp aus:

![](ats-images/client01.png "Festlegen der IOS-Buildoptionen")


#### <a name="managed-handler"></a>Verwalteter Handler

Der verwaltete Handler ist der vollständig verwaltete httpclient-Handler, der in früheren Versionen von xamarin. IOS enthalten war und der Standard Handler ist.

Vorteile

- Sie ist mit Microsoft .net und einer älteren Version von xamarin am meisten kompatibel.

Nachteile

- Sie ist nicht vollständig in ios integriert (z. b. auf TLS 1,0 beschränkt).
- Es ist in der Regel viel langsamer als die nativen APIs.
- Hierfür ist mehr verwalteter Code erforderlich, und es werden größere Apps erstellt.

#### <a name="cfnetwork-handler"></a>CFNetwork-Handler

Der CFNetwork-basierte Handler basiert auf dem `CFNetwork` systemeigenen Framework.

Vorteile

- Verwendet Native API, um eine bessere Leistung und kleinere ausführbare Größen zu erzielen.
- Bietet Unterstützung für neuere Standards, z. b. TLS 1,2.

Nachteile

- Erfordert IOS 6 oder höher.
- Nicht verfügbar für watchos.
- Einige Features und Optionen für httpclient sind nicht verfügbar.

#### <a name="nsurlsession-handler"></a>Nsurlsession-Handler

Der nsurlsession-basierte Handler basiert auf der `NSUrlSession` systemeigenen API.

Vorteile

- Verwendet Native API, um eine bessere Leistung und kleinere ausführbare Größen zu erzielen.
- Bietet Unterstützung für neuere Standards, z. b. TLS 1,2.

Nachteile

- Erfordert IOS 7 oder höher.
- Einige Features und Optionen für httpclient sind nicht verfügbar. 

## <a name="diagnosing-ats-issues"></a>Diagnostizieren von ATS-Problemen

Wenn Sie versuchen, eine Verbindung mit dem Internet (entweder direkt oder über eine Webansicht in ios 9) herzustellen, erhalten Sie möglicherweise eine Fehlermeldung in der Form:

> Die APP-Transport Sicherheit hat eine Klartext-http http://www.-the-blocked-domain.com) (Ressourcen Last, da Sie unsicher ist) blockiert. Temporäre Ausnahmen können über die Datei "Info. plist" ihrer App konfiguriert werden.

In iOS9 erzwingt App-Transport Sicherheit (app Transport Security, ATS) sichere Verbindungen zwischen Internetressourcen (z. b. dem Back-End-Server der APP) und ihrer app. Darüber hinaus erfordert die Kommunikation mit dem `HTTPS` Protokoll und der übergeordneten API-Kommunikation, um mit der TLS-Version 1,2 mit vorwärts Geheimnisse verschlüsselt zu werden.

Da ATS in apps, die für IOS 9 und OS X 10,11 (El Capitan) erstellt werden, standardmäßig aktiviert ist `NSURLConnection`, `CFURL` gelten `NSURLSession` für alle Verbindungen, die verwenden, oder die Sicherheitsanforderungen. Wenn Ihre Verbindungen diese Anforderung nicht erfüllen, schlagen Sie mit einer Ausnahme fehl.

Apple stellt außerdem die [tlstool-Beispiel-App](https://developer.apple.com/library/mac/samplecode/sc1236/Introduction/Intro.html#//apple_ref/doc/uid/DTS40014927-Intro-DontLinkElementID_2) bereit, die kompiliert (oder optional in xamarin und C#transcodiert) und zur Diagnose von ATS/TLS-Problemen verwendet werden kann. Informationen zum Beheben dieses Problems finden Sie unten im Abschnitt " [Opt-out of ATS](#optout) ".


<a name="config" />

## <a name="configuring-ats-options"></a>Konfigurieren von ATS-Optionen

Sie können mehrere Funktionen von ATS konfigurieren, indem Sie Werte für bestimmte Schlüssel in der **Info. plist** -Datei Ihrer APP festlegen. Die folgenden Schlüssel sind zum Steuern von ATS verfügbar (_eingerückt, um zu veranschaulichen, wie Sie eingefügt werden_):

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

Jeder Schlüssel hat den folgenden Typ und Bedeutung:

- **Nsapptransportsecurity** (`Dictionary`): Enthält alle Einstellungs Schlüssel und-Werte für ATS.
- **Nsallowsarbitraryloads** (`Boolean`): Wenn `YES` Ate für eine Domäne deaktiviert werden, die **nicht** in `NSExceptionDomains`aufgeführt ist. Bei aufgelisteten Domänen werden die angegebenen Sicherheitseinstellungen verwendet.
- **Nsallowsarbitraryloadsinwebcontent** (`Boolean`): Gibt `YES` an, ob Webseiten ordnungsgemäß geladen werden können, während der Schutz für die Apple-Transport Sicherheit weiterhin für den Rest der App aktiviert ist.
- **Nsexceptiondomains** (`Dictionary`): Eine Auflistung von Domänen und die Sicherheitseinstellungen, die von ATS für eine bestimmte Domäne verwendet werden sollen.
- **< Domain-Name-for-Exception-As-String >** (`Dictionary`): Eine Auflistung von Ausnahmen für eine bestimmte Domäne (z. b. `www.xamarin.com`) angezeigt wird.
- **Nsexceptionminimumtlsversion** (`String`): Die minimale TLS-Version `TLSv1.0`als `TLSv1.1` oder `TLSv1.2` (Dies ist die Standardeinstellung).
- **Nsexceptionrequirements sforwardgeheimnisses** (`Boolean`): Wenn `NO` die Domäne keine Chiffre mit vorwärts Sicherheit verwenden muss. Der Standardwert ist `YES`.
- **NSExceptionAllowsInsecureHTTPLoads** (`Boolean`) - If `NO` (the default) all communications with this domain must be in the `HTTPS` protocol.
- **Nsrequirements scertificatetransparenz** (`Boolean`): Wenn `YES` die Secure Sockets Layer der Domäne gültige Transparenz Daten enthalten muss. Der Standardwert ist `NO`.
- **Nsincludessubdomains** (`Boolean`): Wenn `YES` diese Einstellungen alle Unterdomänen dieser Domäne überschreiben. Der Standardwert ist `NO`.
- **Nsthirdpartyexceptionminimumtlsversion** (`String`): Die TLS-Version, die verwendet wird, wenn es sich bei der Domäne um einen Drittanbieter Dienst außerhalb der Kontrolle des Entwicklers handelt.
- **Nsthirdpartyexceptionrequirements sforwardgeheimnisses** (`Boolean`): Wenn `YES` für eine Drittanbieter Domäne ein vorwärts Geheimnis erforderlich ist.
- **NSThirdPartyExceptionAllowsInsecureHTTPLoads** (`Boolean`) - If `YES` the ATS will allow non-secure communication with 3rd party domains.

<a name="optout" />

### <a name="opting-out-of-ats"></a>Deaktivieren von ATS

Apple empfiehlt zwar dringend die Verwendung `HTTPS` des Protokolls und die sichere Kommunikation mit internetbasierten Informationen, es kann jedoch vorkommen, dass dies nicht immer möglich ist. Beispielsweise bei der Kommunikation mit einem Drittanbieter-Webdienst oder bei Verwendung von über das Internet übermittelten anzeigen in Ihrer APP.

Wenn Ihre xamarin. IOS-App eine Anforderung an eine unsichere Domäne richten muss, deaktivieren die folgenden Änderungen an der Datei " **Info. plist** " der APP die Sicherheitsstandards, die von ATS für eine bestimmte Domäne erzwungen werden:

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

Doppelklicken Sie in Visual Studio für Mac auf die `Info.plist` Datei im **Projektmappen-Explorer**, wechseln Sie zur **Quell** Ansicht, und fügen Sie die obigen Schlüssel hinzu:

[![](ats-images/ats01.png "Die Quell Ansicht der Datei \"Info. plist\"")](ats-images/ats01.png#lightbox)


Wenn Ihre APP Webinhalte von nicht sicheren Websites laden und anzeigen muss, fügen Sie der Datei " **Info. plist** " Ihrer APP Folgendes hinzu, damit Webseiten ordnungsgemäß geladen werden können, während der Apple Transport Security (ATS)-Schutz weiterhin für den Rest der App aktiviert ist:

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key> NSAllowsArbitraryLoadsInWebContent</key>
    <true/>
</dict>
```

Optional können Sie die folgenden Änderungen an der Datei " **Info. plist** " Ihrer APP vornehmen, um "ATS" für alle Domänen und die Internetkommunikation vollständig zu deaktivieren:

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

Doppelklicken Sie in Visual Studio für Mac auf die `Info.plist` Datei im **Projektmappen-Explorer**, wechseln Sie zur **Quell** Ansicht, und fügen Sie die obigen Schlüssel hinzu:

[![](ats-images/ats02.png "Die Quell Ansicht der Datei \"Info. plist\"")](ats-images/ats02.png#lightbox)

> [!IMPORTANT]
> Wenn Ihre Anwendung eine Verbindung mit einer unsicheren Website erfordert, sollten Sie die Domäne **immer** als Ausnahme mit `NSExceptionDomains` eingeben, anstatt sie vollständig mithilfe `NSAllowsArbitraryLoads`von zu deaktivieren. `NSAllowsArbitraryLoads` sollte nur im Notfall extrem verwendet werden.




Die Deaktivierung von ATS sollte _nur_ als letztes Mittel verwendet werden, wenn der Wechsel zu sicheren Verbindungen entweder nicht verfügbar oder nicht praktikabel ist.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden App-Transport Sicherheit (app Transport Security, ATS) vorgestellt und beschrieben, wie die sichere Kommunikation mit dem Internet erzwungen wird. Zunächst haben wir die Änderungen erläutert, die für eine xamarin. IOS-app unter IOS 9 erforderlich sind. Anschließend haben wir die Kontrolle über die Features und Optionen von ATS behandelt. Schließlich haben wir uns mit der Entscheidung in ihrer xamarin. IOS-App abgemeldet.



## <a name="related-links"></a>Verwandte Links

- [IOS 9-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [IOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
