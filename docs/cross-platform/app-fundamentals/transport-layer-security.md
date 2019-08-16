---
title: Transport Layer Security (TLS) 1,2
description: In diesem Dokument wird beschrieben, wie Sie TLS 1,2 für xamarin. IOS-, xamarin. Android-und xamarin. Mac-Projekte aktivieren. Es zeigt, wie dies in Visual Studio 2019 und Visual Studio für Mac durchzuführen ist.
ms.prod: xamarin
ms.assetid: 399F71C6-16A4-4ABC-B30D-AF17D066A5FA
author: asb3993
ms.author: amburns
ms.date: 04/20/2018
ms.openlocfilehash: 31069199e8ebc89da76b63e58651adea82db6882
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69526768"
---
# <a name="transport-layer-security-tls-12"></a>Transport Layer Security (TLS) 1,2

Die Verwendung der neuesten Version von [ _Transport Layer Security_ (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security) ist wichtig, um sicherzustellen, dass die Kommunikation mit dem Anwendungs Netzwerk sicher ist.

> [!WARNING]
> **April, 2018** – aufgrund erhöhter Sicherheitsanforderungen (einschließlich der PCI-Konformität) erwarten große cloudanbieter und Webserver die Unterstützung von TLS-Versionen, die älter sind als 1,2. Xamarin-Projekte, die in früheren Versionen von Visual Studio erstellt wurden, verwenden standardmäßig ältere Versionen von TLS.
>
> Um sicherzustellen, dass Ihre apps weiterhin mit diesen Servern und Diensten funktionieren, **sollten Sie Ihre xamarin-Projekte aktualisieren, um die unten aufgeführten Einstellungen zu verwenden. Anschließend können Sie Ihre apps für Ihre Benutzer neu erstellen und erneut** bereitstellen.

Projekte müssen auf die **System .net. http** -Assembly verweisen und wie unten dargestellt konfiguriert werden.

## <a name="update-xamarinandroid-to-tls-12"></a>Aktualisieren von xamarin. Android auf TLS 1,2

Aktualisieren Sie die **HttpClient-Implementierung** und die **SSL/TLS-Implementierungs** Optionen, um TLS 1,2-Sicherheit zu aktivieren.

> [!NOTE]
> Erfordert Android 5,0 oder höher.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Diese Einstellungen finden Sie in den **Projekteigenschaften > Android-Optionen** , und klicken Sie dann auf die Schaltfläche **erweitert** :

[![Konfigurieren von HttpClient und TLS in Visual Studio](transport-layer-security-images/android-win-sml.png)](transport-layer-security-images/android-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Diese Einstellungen finden Sie auf der Registerkarte **Projektoptionen > Build > Android-Build** :

[![Konfigurieren von HttpClient und TLS in Visual Studio für Mac](transport-layer-security-images/android-mac-sml.png)](transport-layer-security-images/android-mac.png#lightbox)

-----

## <a name="update-xamarinios-to-tls-12"></a>Aktualisieren von xamarin. IOS auf TLS 1,2

Aktualisieren Sie die **HttpClient-Implementierungs** Option, um die Sicherheit von TL 1,2 zu aktivieren.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Diese Einstellung befindet sich in den **Projekteigenschaften > IOS-Build**:

[![Konfigurieren von HttpClient und TLS in Visual Studio](transport-layer-security-images/ios-win-sml.png)](transport-layer-security-images/ios-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Diese Einstellung befindet sich auf der Registerkarte **Projektoptionen > Build > IOS-Build** :

[![HttpClient in Visual Studio für Mac konfigurieren](transport-layer-security-images/ios-mac-sml.png)](transport-layer-security-images/ios-mac.png#lightbox)

-----

## <a name="update-xamarinmac-to-tls-12"></a>Aktualisieren von xamarin. Mac auf TLS 1,2

Um in Visual Studio für Mac TLS 1,2 in einer xamarin. Mac-app zu aktivieren, aktualisieren Sie die **HttpClient-Implementierungs** Option in den **Projektoptionen > Build > Mac-Build**:

[![HttpClient in Visual Studio für Mac konfigurieren](transport-layer-security-images/macos-mac-sml.png)](transport-layer-security-images/macos-mac.png#lightbox)

> [!WARNING]
> Die anstehende Version Xamarin.Mac 4.8 unterstützt nur macOS 10.9 oder höher.
> Ältere Versionen von Xamarin.Mac unterstützen macOS 10.7 oder höher, aber diese älteren Versionen von MacOS verfügen nicht über ausreichende TLS-Infrastruktur zur Unterstützung von TLS 1.2. Für macOS 10.7 oder macOS 10.8 sollten Sie Xamarin.Mac 4.6 oder niedriger verwenden.

## <a name="alternative-configuration-options"></a>Alternative Konfigurationsoptionen

In diesem Abschnitt werden Alternativen zu den oben gezeigten TLS 1,2-unterstützten Konfigurationen erläutert.
Anwendungsentwickler sollten diese Alternativen nur berücksichtigen, wenn Sie die Risiken der Verwendung unterschiedlicher TLS-Unterstützung verstehen.

### <a name="httpclient-implementation"></a>HttpClient-Implementierung

Xamarin-Entwickler haben immer in der Lage, die nativen Netzwerk Klassen im Code zu verwenden. es gibt jedoch auch eine Option, die bestimmt, welcher Netzwerk `HttpClient` Stapel von den Klassen verwendet wird. Dadurch wird eine vertraute .NET-API bereitstellt, die die Vorteile der systemeigenen Plattform in der Geschwindigkeit und Sicherheit bietet.

Folgende Optionen stehen zur Verfügung:

- **Verwalteter Stapel** – die von Mono bereitgestellte Netzwerkfunktionalität oder
- **Nativer Stapel** – verschiedene Netzwerk-APIs, die von den zugrunde liegenden Plattformen (Android, IOS oder macOS) bereitgestellt werden.

Der verwaltete Stapel bietet das höchste Maß an Kompatibilität mit vorhandenem .NET-Code, kann jedoch langsamer sein und zu einer größeren ausführbaren Größe führen.

Die systemeigenen Optionen können schneller sein und eine bessere Sicherheit (einschließlich TLS 1,2) bieten. Sie bieten jedoch möglicherweise nicht alle Funktionen und `HttpClient` Optionen der-Klasse.

### <a name="ssltls-implementation-android"></a>SSL/TLS-Implementierung (Android)

Mit den Optionen für Android-Projekte können Sie auch auswählen, welche SSL/TLS-Implementierung unterstützt wird:

- **Mono/Managed** – TLS 1,1 unter Android
- **Native** – TLS 1,2 unter Android.

Neue xamarin-Projekte haben standardmäßig die systemeigene Implementierung, die TLS 1,2 unterstützt (Dies wird für alle Projekte empfohlen), Sie können jedoch aus Kompatibilitätsgründen zurück zum verwalteten Code wechseln.

> [!IMPORTANT]
> Die **Mono/Managed-** Option wurde aus den Optionen für [IOS-und Mac-Projekte entfernt](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/xamarin.ios_10/xamarin.ios_10.8.md) .
>
> Die native Option wird immer auf IOS-und Mac-Plattformen verwendet.

## <a name="platform-specific-details"></a>Plattformspezifische Details

Die obige Zusammenfassung erläutert die Einstellungen auf Projektebene für die HttpClient-und SSL/TLS-Implementierung in xamarin-Projekten. Die HttpClient-Implementierung kann auch dynamisch im Code festgelegt werden. Weitere Informationen finden Sie in den folgenden plattformspezifischen Leitfäden:

- [**Android**](~/android/app-fundamentals/http-stack.md)
- [**IOS und Mac**](~/cross-platform/macios/http-stack.md)

## <a name="summary"></a>Zusammenfassung

Anwendungen sollten Transport Layer Security (TLS) 1,2 verwenden, sofern dies möglich ist.
Aktualisieren Sie die Einstellungen in vorhandenen Anwendungen entsprechend den Anweisungen in diesem Artikel, und erstellen Sie Sie neu, und stellen Sie Sie für Ihre Kunden erneut bereit.

## <a name="related-links"></a>Verwandte Links

- [App-Transportsicherheit](~/ios/app-fundamentals/ats.md)
- [Xamarin.Android Environment](~/android/deploy-test/environment.md)
- [Xamarin Cycle 9 (Februar 2017)](https://releases.xamarin.com/stable-release-cycle-9/)
- [TLS (Wikipedia)](https://en.wikipedia.org/wiki/Transport_Layer_Security)
- [Versions Hinweise zu Mono 4,8-TLS 1,2-Unterstützung](https://www.mono-project.com/docs/about-mono/releases/4.8.0/#tls-12-support)
- [BoringSSL](https://boringssl.googlesource.com/boringssl/)
- ["Httpclient", "httpclienthandler" und "webrequesthandler" wurden erläutert.](https://blogs.msdn.microsoft.com/henrikn/2012/08/07/httpclient-httpclienthandler-and-webrequesthandler-explained/)
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
- [HTTP-Client (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/httpclient/)
