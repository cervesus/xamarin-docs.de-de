---
title: Transport Layer Security (TLS) 1.2
description: Aktivieren von TLS 1.2 für Xamarin-Projekten für Android, iOS und Mac
ms.prod: xamarin
ms.assetid: 399F71C6-16A4-4ABC-B30D-AF17D066A5FA
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/20/2018
ms.openlocfilehash: 6205e8633ccdd2c1e568e7de8103c38eb9edbc2f
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/26/2018
---
# <a name="transport-layer-security-tls-12"></a>Transport Layer Security (TLS) 1.2

Verwenden die neueste Version des [ _Transport Layer Security_ (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security) ist wichtig, sicherzustellen, dass Netzwerkkommunikation Anwendung sind sicherer.

> [!WARNING]
> **April, 2018** – aufgrund der erhöhten Sicherheit Anforderungen, einschließlich der PCI-Konformität Hauptversion Cloud-Anbieter und der Webserver unterstützen TLS-Versionen, die älter sind als 1.2 zu beenden.  Xamarin-Projekte erstellt, die in früheren Versionen von Visual Studio standardmäßig die frühere Versionen von TLS zu verwenden.
>
> Um sicherzustellen, dass Ihre apps mit diesen Servern und -Dienste, funktionieren weiterhin **sollten Sie Ihre Projekte Xamarin verwenden Sie die Einstellung, und klicken Sie dann erneut erstellen und erneut bereitstellen Ihrer apps aktualisieren** für Ihre Benutzer.

Projekte verweisen müssen die **System.Net.Http** Assembly und konfiguriert werden, wie unten dargestellt.

## <a name="update-android-to-tls-12"></a>Android TLS 1.2 zu aktualisieren.

Update der **"HttpClient" Implementierung** und **SSL/TLS-Implementierung** Optionen aus, um die Sicherheit des TLS 1.2 zu aktivieren.

> [!NOTE]
> Erfordert Android 5.0 oder höher.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Diese Einstellungen finden Sie in **Projekteigenschaften > Android Options** klicken und dann auf die **erweitert** Schaltfläche:

[![Konfigurieren von "HttpClient" und TLS in Visual Studio](transport-layer-security-images/android-win-sml.png)](transport-layer-security-images/android-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Diese Einstellungen finden Sie in **Projektoptionen > Erstellen > Android erstellen** Registerkarte:

[![Konfigurieren von "HttpClient" und TLS in Visual Studio für Mac](transport-layer-security-images/android-mac-sml.png)](transport-layer-security-images/android-mac.png#lightbox)

-----

## <a name="update-ios-to-tls-12"></a>Update iOS verwenden, um TLS 1.2

Update der **"HttpClient" Implementierung** Option aus, um TSL 1.2 Sicherheit zu aktivieren.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Mit dieser Einstellung finden Sie in **Projekteigenschaften > iOS-Build**:

[![Konfigurieren von "HttpClient" und TLS in Visual Studio](transport-layer-security-images/ios-win-sml.png)](transport-layer-security-images/ios-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Mit dieser Einstellung finden Sie in **Projektoptionen > Erstellen > iOS-Build** Registerkarte:

[![Konfigurieren von "HttpClient" in Visual Studio für Mac](transport-layer-security-images/ios-mac-sml.png)](transport-layer-security-images/ios-mac.png#lightbox)

-----

## <a name="update-macos-to-tls-12-in-visual-studio-for-mac"></a>Aktualisieren von MacOS TLS 1.2 in Visual Studio für Mac

Update der **"HttpClient" Implementierung** option **Projektoptionen > Erstellen > Mac erstellen** Tab, um TSL 1.2 Sicherheit zu aktivieren:

[![Konfigurieren von "HttpClient" in Visual Studio für Mac](transport-layer-security-images/macos-mac-sml.png)](transport-layer-security-images/macos-mac.png#lightbox)

## <a name="alternative-configuration-options"></a>Alternative Konfigurationsoptionen

Dieser Abschnitt beschreibt die alternativen zu den oben gezeigten Konfigurationen TLS 1.2 unterstützt.
Anwendungsentwickler sollten nur diese alternativen in Betracht ziehen, wenn sie verstehen, dass die Risiken einer verwenden verschiedene Ebenen der TLS-Unterstützung.

### <a name="httpclient-implementation"></a>HttpClient-Implementierung

Xamarin-Entwickler wurden immer die systemeigenen Netzwerkklassen in ihrem Code verwenden, jedoch gibt es auch eine Option aus, der bestimmt, welche Netzwerkstapel verwendet wird, indem die `HttpClient` Klassen. Dies bietet eine vertraute .NET-API, die die Geschwindigkeit und Sicherheit bietet für die systemeigene Plattform Vorteile.

Folgende Optionen stehen zur Verfügung:

- **Verwalteter Stapel** – die Netzwerkfunktionalität bereitgestellter Mono-oder
- **Systemeigenen Stapel** – verschiedene Netzwerk-APIs, die von der zugrunde liegenden Plattformen (Android, iOS oder Mac OS) bereitgestellt.

Verwaltete Stapel bietet das höchste Maß an Kompatibilität mit vorhandenen .NET Code, aber er kann langsamer und größere ausführbare dazu.

Die systemeigenen Optionen können schneller verarbeitet werden und haben eine höhere Sicherheit (einschließlich TLS 1.2), aber möglicherweise keine bereit, die Funktionen und Optionen von der `HttpClient` Klasse.

### <a name="ssltls-implementation-android"></a>SSL/TLS-Implementierung (Android)

Android-Projekt-Optionen können auch mit Ihnen bei der Auswahl der SSL/TLS-Implementierung zu unterstützen:

- **Mono/verwalteten** – TLS 1.1 auf Android-Geräten
- **Systemeigene** – TLS 1.2 auf Android-Geräten.

Neue Xamarin-Projekte standardmäßig die native Implementierung, die TLS 1.2 unterstützt (die für alle Projekte empfohlen wird), muss Sie jedoch wieder auf den verwalteten Code Wenn wechseln, können aus Gründen der Kompatibilität.

> [!IMPORTANT]
> Die **Mono/verwaltete** Option wurde [von IOS- und Mac entfernt](https://developer.xamarin.com/releases/ios/xamarin.ios_10/xamarin.ios_10.8/) Projekt Optionen.
>
> Die systemeigene Option ist immer auf IOS- und Mac-Plattformen verwendet.

## <a name="platform-specific-details"></a>Clientplattform-spezifische Details

Die oben genannten Zusammenfassung wird erläutert, die Einstellungen auf Projektebene für "HttpClient" und SSL/TLS-Implementierung in Xamarin-Projekten. Die Implementierung für "HttpClient" kann im Code auch dynamisch festgelegt werden. Diese plattformspezifischen Leitfäden für Weitere Informationen finden:

- [**Android**](~/android/app-fundamentals/http-stack.md)
- [**iOS und Mac**](~/cross-platform/macios/http-stack.md)


## <a name="summary"></a>Zusammenfassung

Anwendungen sollten Transport Layer Security (TLS) 1.2 möglichst verwenden.
Sie sollten aktualisieren Sie die Einstellungen in vorhandenen Anwendungen gemäß der Anleitung in diesem Artikel dann erneut erstellen und an Ihre Kunden erneut bereitstellen.

## <a name="related-links"></a>Verwandte Links

- [App-Transportsicherheit](~/ios/app-fundamentals/ats.md)
- [Xamarin.Android Environment](~/android/deploy-test/environment.md)
- [Xamarin-Zyklus 9 (Februar 2017)](https://releases.xamarin.com/stable-release-cycle-9/)
- [TLS (Wikipedia)](https://en.wikipedia.org/wiki/Transport_Layer_Security)
- [Mono 4.8 Release Notes - TLS 1.2-Unterstützung](http://www.mono-project.com/docs/about-mono/releases/4.8.0/#tls-12-support)
- [BoringSSL](https://boringssl.googlesource.com/boringssl/)
- [HttpClientHandler, "HttpClient" und "webrequesthandler" erläutert](https://blogs.msdn.microsoft.com/henrikn/2012/08/07/httpclient-httpclienthandler-and-webrequesthandler-explained/)
- [System.Net.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)
- [System.Net.HttpClientHandler](https://msdn.microsoft.com/library/system.net.http.httpclienthandler(v=vs.118).aspx)
- [System.Net.HttpMessageHandler](https://msdn.microsoft.com/library/system.net.http.httpmessagehandler(v=vs.118).aspx)
- [System.Net.HttpWebRequest](https://msdn.microsoft.com/library/system.net.httpwebrequest(v=vs.110).aspx)
- [System.Net.WebClient](https://msdn.microsoft.com/library/system.net.webclient(v=vs.110).aspx)
- [System.Net.WebRequest](https://msdn.microsoft.com/library/system.net.webrequest(v=vs.110).aspx)
- [java.net.URLConnection](http://developer.android.com/reference/java/net/URLConnection.html)
- [Foundation.CFNetwork](https://developer.xamarin.com/api/type/CoreFoundation.CFNetwork/)
- [Foundation.NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/)
- [System.Net.WebRequest](https://msdn.microsoft.com/library/system.net.webrequest(v=vs.110).aspx)
- [HTTP-Client (Beispiel)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
