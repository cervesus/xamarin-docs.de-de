---
title: Transport Layer Security (TLS) 1.2
description: In diesem Dokument wird beschrieben, wie zum Aktivieren von TLS 1.2 für Xamarin.iOS, Xamarin.Android und Xamarin.Mac-Projekte. Es wird veranschaulicht, wie das geht in Visual Studio 2017 und Visual Studio für Mac.
ms.prod: xamarin
ms.assetid: 399F71C6-16A4-4ABC-B30D-AF17D066A5FA
author: asb3993
ms.author: amburns
ms.date: 04/20/2018
ms.openlocfilehash: 889eaf2b2f87b22010315f5e92dcfd1cd4c7c446
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668269"
---
# <a name="transport-layer-security-tls-12"></a>Transport Layer Security (TLS) 1.2

Verwenden die neueste Version des [ _Transport Layer Security_ (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security) ist wichtig, sicherzustellen, dass Netzwerkkommunikation für die Anwendung sicherer sind.

> [!WARNING]
> **April 2018** – erhöhte Sicherheit aufgrund der Anforderungen, einschließlich der PCI-Compliance Haupt-Cloudanbieter und Webserver werden erwartet, dass die Unterstützung von TLS-Versionen älter als 1.2 eingestellt.  Xamarin-Projekte erstellt, die in früheren Versionen von Visual Studio standardmäßig die ältere Versionen von TLS zu verwenden.
>
> Um sicherzustellen, dass Ihre apps mit diesen Servern und Diensten, funktionieren weiterhin, **sollten Sie Ihren Xamarin-Projekten, zum Verwenden Sie die folgenden Einstellungen, und klicken Sie dann erneut erstellen, und erneut bereitstellen Ihrer apps aktualisieren** für Ihre Benutzer.

Projekte verweisen müssen die **System.Net.Http** Assembly und konfiguriert werden, wie unten dargestellt.

## <a name="update-xamarinandroid-to-tls-12"></a>Aktualisieren von Xamarin.Android auf TLS 1.2

Update der **HttpClient-Implementierung** und **SSL/TLS-Implementierung** Optionen zum Aktivieren von TLS 1.2-Sicherheit.

> [!NOTE]
> Ist Android 5.0 oder höher erforderlich.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Diese Einstellungen befinden sich **Projekteigenschaften > Android-Optionen** und dann auf die **erweitert** Schaltfläche:

[![Konfigurieren von "HttpClient" und TLS in Visual Studio](transport-layer-security-images/android-win-sml.png)](transport-layer-security-images/android-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Diese Einstellungen befinden sich **Projektoptionen > Erstellen > Android-Build** Registerkarte:

[![Konfigurieren von "HttpClient" und TLS in Visual Studio für Mac](transport-layer-security-images/android-mac-sml.png)](transport-layer-security-images/android-mac.png#lightbox)

-----

## <a name="update-xamarinios-to-tls-12"></a>Aktualisieren von Xamarin.iOS auf TLS 1.2

Update der **HttpClient-Implementierung** Option aus, um die Sicherheit von TSL 1.2 zu aktivieren.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Mit dieser Einstellung finden Sie in **Projekteigenschaften > iOS-Build**:

[![Konfigurieren von "HttpClient" und TLS in Visual Studio](transport-layer-security-images/ios-win-sml.png)](transport-layer-security-images/ios-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Mit dieser Einstellung finden Sie in **Projektoptionen > Erstellen > iOS-Build** Registerkarte:

[![Konfigurieren von "HttpClient" in Visual Studio für Mac](transport-layer-security-images/ios-mac-sml.png)](transport-layer-security-images/ios-mac.png#lightbox)

-----

## <a name="update-xamarinmac-to-tls-12"></a>Aktualisieren von Xamarin.Mac TLS 1.2

Aktualisieren Sie in Visual Studio für Mac, um TLS 1.2 in einer Xamarin.Mac-app zu aktivieren, die **HttpClient-Implementierung** option **Projektoptionen > Erstellen > Mac Build**:

[![Konfigurieren von "HttpClient" in Visual Studio für Mac](transport-layer-security-images/macos-mac-sml.png)](transport-layer-security-images/macos-mac.png#lightbox)

> [!WARNING]
> Die anstehende Version Xamarin.Mac 4.8 unterstützt nur macOS 10.9 oder höher.
> Ältere Versionen von Xamarin.Mac unterstützen macOS 10.7 oder höher, aber diese älteren Versionen von MacOS verfügen nicht über ausreichende TLS-Infrastruktur zur Unterstützung von TLS 1.2. Für macOS 10.7 oder macOS 10.8 sollten Sie Xamarin.Mac 4.6 oder niedriger verwenden.

## <a name="alternative-configuration-options"></a>Alternative Konfiguration-Optionen

Dieser Abschnitt beschreibt die alternativen für die TLS 1.2 unterstützt Konfigurationen, die oben gezeigte.
Anwendungsentwickler sollten diese alternativen nur, wenn sie verstehen, dass die Risiken der Verwendung der verschiedenen Ebenen der TLS-Unterstützung.

### <a name="httpclient-implementation"></a>HttpClient-Implementierung

Xamarin-Entwickler wurden immer in der Lage, die native Netzwerkklassen in ihrem Code verwenden, jedoch gibt es eine Option aus, der bestimmt, welche Netzwerkstapel wird auch verwendet, durch die `HttpClient` Klassen. Dies bietet eine vertraute .NET-API, die die Geschwindigkeit und Sicherheit Vorteile der nativen Plattform bietet.

Folgende Optionen stehen zur Verfügung:

- **Verwaltete Stapel** – die Mono bereitgestellte Netzwerkfunktionalität, oder
- **Systemeigenen Stapel** – verschiedene Netzwerk-APIs, die von den zugrunde liegenden Plattformen (Android, iOS und MacOS) bereitgestellt.

Der verwaltete Stapel bietet die höchste Ebene der Kompatibilität mit vorhandenem Code für .NET –, aber er kann langsamer und Größe der größeren ausführbaren Datei dazu.

Die nativen Optionen können schneller sein und eine bessere Sicherheit (einschließlich TLS 1.2), aber möglicherweise nicht alle Funktionen und Optionen des bereit die `HttpClient` Klasse.

### <a name="ssltls-implementation-android"></a>SSL/TLS-Implementierung (Android)

Android-Projekt-Optionen können Sie auch wählen Sie die SSL/TLS-Implementierung, unterstützt:

- **Mono/verwalteten** – TLS 1.1 unter Android
- **Native** – TLS 1.2 unter Android.

Neue Xamarin-Projekte standardmäßig für die native Implementierung, die TLS 1.2 unterstützt (was für alle Projekte zu empfehlen ist), aber Sie wieder auf den verwalteten Code wechseln können Wenn Gründen der Kompatibilität erforderlich.

> [!IMPORTANT]
> Die **Mono bzw. einer verwalteten** Option wurde [von IOS- und Mac entfernt](https://developer.xamarin.com/releases/ios/xamarin.ios_10/xamarin.ios_10.8/) Projektoptionen.
>
> Die Native-Option wird immer unter iOS und Mac-Plattformen verwendet.

## <a name="platform-specific-details"></a>Plattformspezifischen details

Die oben genannten Zusammenfassung wird erläutert, die Einstellungen auf Projektebene für "HttpClient" und SSL/TLS-Implementierung in Xamarin-Projekte. Die HttpClient-Implementierung kann auch dynamisch im Code festgelegt werden. Diese plattformspezifischen Leitfäden Weitere Informationen finden Sie unter:

- [**Android**](~/android/app-fundamentals/http-stack.md)
- [**iOS und Mac**](~/cross-platform/macios/http-stack.md)

## <a name="summary"></a>Zusammenfassung

Anwendungen sollten Transport Layer Security (TLS) 1.2 verwenden, wo immer dies möglich.
Sie sollten aktualisieren Sie die Einstellungen in vorhandenen Anwendungen gemäß den Anweisungen in diesem Artikel und dann erneut erstellen und erneut für Ihre Kunden bereitstellen.

## <a name="related-links"></a>Verwandte Links

- [App-Transportsicherheit](~/ios/app-fundamentals/ats.md)
- [Xamarin.Android Environment](~/android/deploy-test/environment.md)
- [Xamarin Cycle 9 (Februar 2017)](https://releases.xamarin.com/stable-release-cycle-9/)
- [TLS (Wikipedia)](https://en.wikipedia.org/wiki/Transport_Layer_Security)
- [Mono 4.8-Versionsanmerkungen - TLS 1.2-Unterstützung](https://www.mono-project.com/docs/about-mono/releases/4.8.0/#tls-12-support)
- [BoringSSL](https://boringssl.googlesource.com/boringssl/)
- ["HttpClient" HttpClientHandler und WebRequestHandler erläutert](https://blogs.msdn.microsoft.com/henrikn/2012/08/07/httpclient-httpclienthandler-and-webrequesthandler-explained/)
- [System.Net.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)
- [System.Net.HttpClientHandler](https://msdn.microsoft.com/library/system.net.http.httpclienthandler(v=vs.118).aspx)
- [System.Net.HttpMessageHandler](https://msdn.microsoft.com/library/system.net.http.httpmessagehandler(v=vs.118).aspx)
- [System.Net.HttpWebRequest](https://msdn.microsoft.com/library/system.net.httpwebrequest(v=vs.110).aspx)
- [System.Net.WebClient](https://msdn.microsoft.com/library/system.net.webclient(v=vs.110).aspx)
- [System.Net.WebRequest](https://msdn.microsoft.com/library/system.net.webrequest(v=vs.110).aspx)
- [java.net.URLConnection](https://developer.android.com/reference/java/net/URLConnection.html)
- [Foundation.CFNetwork](xref:CoreFoundation.CFNetwork)
- [Foundation.NSUrlConnection](xref:Foundation.NSUrlConnection)
- [System.Net.WebRequest](https://msdn.microsoft.com/library/system.net.webrequest(v=vs.110).aspx)
- [HTTP-Client (Beispiel)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
