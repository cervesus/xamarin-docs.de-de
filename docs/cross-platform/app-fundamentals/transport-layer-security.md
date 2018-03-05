---
title: Transport Layer Security (TLS)
description: "Aktivieren von TLS 1.2 für Xamarin-Projekten für Android, iOS und Mac"
ms.topic: article
ms.prod: xamarin
ms.assetid: 399F71C6-16A4-4ABC-B30D-AF17D066A5FA
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/10/2017
ms.openlocfilehash: 5237ed35116e5f8983df579d0ab68363996fb06f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="transport-layer-security-tls"></a>Transport Layer Security (TLS)

_Aktivieren von TLS 1.2 für Xamarin-Projekten für Android, iOS und Mac_

Verwenden die neueste Version des [ _Transport Layer Security_ (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security) ist wichtig, sicherzustellen, dass Netzwerkkommunikation Anwendung sind sicherer.

> [!NOTE]
> Xamarin frei seit [Februar 2017](https://releases.xamarin.com/stable-release-cycle-9/) TLS 1.2 wird standardmäßig in neuen Projekten verwendet.

TLS 1.2-Unterstützung ist jetzt verfügbar:

* Mono 4.8 (enthält [TLS 1.2-Unterstützung](http://www.mono-project.com/docs/about-mono/releases/4.8.0/#tls-12-support))
* Xamarin.iOS
* Xamarin.Mac
* Xamarin.Android (Android 5.0 oder höher erforderlich)

Projekte verweisen müssen die **System.Net.Http** Assembly. 

## <a name="updating-to-tls-12"></a>Aktualisieren von TLS 1.2

Dieser Abschnitt erläutert einige Konfigurationsoptionen für Netzwerke in Xamarin-Projekten, die damit aktualisiert werden können Ihre _vorhandenen_ apps sichereres Protokoll nutzen.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Diese Einstellungen finden Sie in **Projektoptionen > Android Options** klicken und dann auf die **erweitert** Schaltfläche: 

[![Konfigurieren von "HttpClient" und TLS in Visual Studio](transport-layer-security-images/properties-vs-sml.png)](transport-layer-security-images/properties-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)
Diese Einstellungen finden Sie in **Projekteigenschaften > Buildoptionen > erweitert** Registerkarte:

[![Konfigurieren von "HttpClient" und TLS in Xamarin Studio und Visual Studio für Mac](transport-layer-security-images/properties-xs-sml.png)](transport-layer-security-images/properties-xs.png)

-----


### <a name="httpclient-implementation"></a>HttpClient-Implementierung

Xamarin-Entwickler wurden immer die systemeigenen Netzwerkklassen in ihrem Code verwenden, jedoch gibt es auch eine Option aus, der bestimmt, welche Netzwerkstapel verwendet wird, indem die `HttpClient` Klassen. Dies bietet eine vertraute .NET-API, die die Geschwindigkeit und Sicherheit bietet für die systemeigene Plattform Vorteile.

Folgende Optionen stehen zur Verfügung:

- **Verwalteter Stapel** – die Netzwerkfunktionalität bereitgestellter Mono-oder
- **Systemeigenen Stapel** – verschiedene Netzwerk-APIs, die von der zugrunde liegenden Plattformen (Android, iOS oder Mac OS) bereitgestellt.

Verwaltete Stapel bietet das höchste Maß an Kompatibilität mit vorhandenen .NET Code, aber er kann langsamer und größere ausführbare dazu.

Die systemeigenen Optionen können schneller verarbeitet werden und haben eine höhere Sicherheit (einschließlich TLS 1.2), aber möglicherweise keine bereit, die Funktionen und Optionen von der `HttpClient` Klasse.


### <a name="ssltls-implementation"></a>SSL/TLS-Implementierung

Projektoptionen können auch die Auswahl der SSL/TLS-Implementierung zu unterstützen:

- **Mono/verwaltete** – TLS 1.1 auf Android-Geräten, TLS 1.0 auf IOS- und Mac OS.
- **Systemeigene** – TLS 1.2 auf Android-, IOS- und Mac OS.

Neue Xamarin-Projekte standardmäßig die native Implementierung, die TLS 1.2 unterstützt (die für alle Projekte empfohlen wird), muss Sie jedoch wieder auf den verwalteten Code Wenn wechseln, können aus Gründen der Kompatibilität.

> [!IMPORTANT]
> Die **Mono/verwaltete** Option wird in entfernt eine [der nächsten Releases](https://developer.xamarin.com/releases/ios/xamarin.ios_10/xamarin.ios_10.8/).
>
> Die Native-Option wird empfohlen.

# <a name="platform-specific-details"></a>Clientplattform-spezifische Details

Die oben genannten Zusammenfassung wird erläutert, die Einstellungen auf Projektebene für "HttpClient" und SSL/TLS-Implementierung in Xamarin-Projekten. Die Implementierung für "HttpClient" kann auch dynamisch in Code festgelegt werden, und auf iOS zwei systemeigene Optionen zur Auswahl stehen.

- [**Android**](~/android/app-fundamentals/http-stack.md)
- [**iOS und Mac**](~/cross-platform/macios/http-stack.md)


# <a name="summary"></a>Zusammenfassung

Anwendungen sollten Transport Layer Security (TLS) 1.2 möglichst verwenden.
Neue apps standardmäßig jetzt im vorliegenden Fall jedoch möglicherweise Sie die Einstellungen in vorhandenen Anwendungen gemäß der Anleitung in diesem Artikel zu aktualisieren müssen.

## <a name="related-links"></a>Verwandte Links

- [App-Transportsicherheit](~/ios/app-fundamentals/ats.md)
- [Xamarin.Android Environment](~/android/deploy-test/environment.md)
- [Xamarin-Zyklus 9 (Februar 2017)](https://releases.xamarin.com/stable-release-cycle-9/)
- [TLS (Wikipedia)](https://en.wikipedia.org/wiki/Transport_Layer_Security)
- [Mono 4.8 Release Notes - TLS 1.2-Unterstützung](http://www.mono-project.com/docs/about-monohttps://developer.xamarin.com/releases/4.8.0/#tls-12-support)
- [BoringSSL](https://boringssl.googlesource.com/boringssl/)
- [HttpClientHandler, "HttpClient" und "webrequesthandler" erläutert](https://blogs.msdn.microsoft.com/henrikn/2012/08/07/httpclient-httpclienthandler-and-webrequesthandler-explained/)
- [System.Net.HttpClient](https://msdn.microsoft.com/en-us/library/system.net.http.httpclient(v=vs.118).aspx)
- [System.Net.HttpClientHandler](https://msdn.microsoft.com/en-us/library/system.net.http.httpclienthandler(v=vs.118).aspx)
- [System.Net.HttpMessageHandler](https://msdn.microsoft.com/en-us/library/system.net.http.httpmessagehandler(v=vs.118).aspx)
- [System.Net.HttpWebRequest](https://msdn.microsoft.com/en-us/library/system.net.httpwebrequest(v=vs.110).aspx)
- [System.Net.WebClient](https://msdn.microsoft.com/en-us/library/system.net.webclient(v=vs.110).aspx)
- [System.Net.WebRequest](https://msdn.microsoft.com/en-us/library/system.net.webrequest(v=vs.110).aspx)
- [java.net.URLConnection](http://developer.android.com/reference/java/net/URLConnection.html)
- [Foundation.CFNetwork](https://developer.xamarin.com/api/type/CoreFoundation.CFNetwork/)
- [Foundation.NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/)
- [System.Net.WebRequest](https://msdn.microsoft.com/en-us/library/system.net.webrequest(v=vs.110).aspx)
- [HTTP-Client (Beispiel)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
