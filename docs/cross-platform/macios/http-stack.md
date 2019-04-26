---
title: "\"HttpClient\" und SSL/TLS-Implementierungsauswahl für iOS/macOS"
description: Die HttpClient-Stapel und SSL/TLS-Implementierungsauswahl bestimmt die "HttpClient" und SSL/TLS-Implementierung, die von Ihrer Xamarin iOS, TvOS und MacOS-app verwendet wird.
ms.prod: xamarin
ms.assetid: 12101297-BB04-4410-85F0-A0D41B7E6591
author: asb3993
ms.author: amburns
ms.date: 04/20/2018
ms.openlocfilehash: fd48c7148aadd8d156544113e2d719295294bf40
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61261272"
---
# <a name="httpclient-and-ssltls-implementation-selector-for-iosmacos"></a>Für iOS/MacOS "HttpClient" und SSL/TLS-Implementierungsauswahl

Die **HttpClient-Implementierungsauswahl** für Xamarin.iOS, Xamarin.tvOS und Xamarin.Mac steuert, mit welchen `HttpClient` zu verwendende Implementierung. Sie können auf eine Implementierung, die native iOS, TvOS und MacOS-Transporte verwendet wechseln (`NSUrlSession` oder `CFNetwork`, je nachdem, auf dem Betriebssystem). Der Vorteil ist TLS 1.2-Unterstützung, kleinere Binärdateien und schnellere downloads. Der Nachteil ist, dass es erfordert, dass die Ereignisschleife ausgeführt werden, damit asynchrone Vorgänge ausgeführt werden.

Projekte verweisen müssen die **System.Net.Http** Assembly.

> [!WARNING]
> **April 2018** – erhöhte Sicherheit aufgrund der Anforderungen, einschließlich der PCI-Compliance Haupt-Cloudanbieter und Webserver werden erwartet, dass die Unterstützung von TLS-Versionen älter als 1.2 eingestellt.  Xamarin-Projekte erstellt, die in früheren Versionen von Visual Studio standardmäßig die ältere Versionen von TLS zu verwenden.
>
> Um sicherzustellen, dass Ihre apps mit diesen Servern und Diensten, funktionieren weiterhin, **sollten Sie Ihre Xamarin-Projekte mit aktualisieren die `NSUrlSession` festlegen wie unten gezeigt, klicken Sie dann erneut erstellen und erneut bereitstellen Ihrer apps** für Ihre Benutzer.

### <a name="selecting-an-httpclient-stack"></a>Auswählen eines HttpClient-Stapels

Anpassen der `HttpClient` , die von Ihrer app verwendet wird:

1. Doppelklicken Sie auf die **Projektname** in die **Projektmappen-Explorer** um Optionen für das Projekt zu öffnen.
2. Wechseln Sie zu der **erstellen** Einstellungen für Ihr Projekt (z. B. **iOS-Build** für eine Xamarin.iOS-app).
3. Von der **HttpClient-Implementierung** Dropdownliste, wählen die `HttpClient` Geben Sie als eine der folgenden: **Von NSUrlSession** (empfohlen), **CFNetwork**, oder **verwaltete**.

[![Wählen Sie die HttpClient-Implementierung aus verwaltet, CFNetwork und von NSUrlSession](http-stack-images/http-xs-sml.png)](http-stack-images/http-xs.png#lightbox)

> [!TIP]
> Für TLS 1.2-Unterstützung der `NSUrlSession` Option wird empfohlen.

### <a name="nsurlsession"></a>NSUrlSession

Die `NSURLSession`-Basis Handler basiert darauf, dass die systemeigene `NSURLSession` Framework in iOS 7 und höher verfügbar. 
**Dies ist die empfohlene Einstellung.**

#### <a name="pros"></a>Pros

- Systemeigene APIs verwendet für eine bessere Leistung als auch kleinere Größe der ausführbaren Datei.
- Unterstützung für die aktuelle Standards wie z. B. TLS 1.2.

#### <a name="cons"></a>Nachteile

- Erfordert iOS 7 oder höher.
- Einige `HttpClient` Features/stehen nicht zur Verfügung.

### <a name="cfnetwork"></a>CFNetwork

Die `CFNetwork`-Basis Handler basiert darauf, dass die systemeigene `CFNetwork` Framework in iOS 6 und höher verfügbar.

#### <a name="pros"></a>Pros

- Systemeigene APIs verwendet für eine bessere Leistung als auch kleinere Größe der ausführbaren Datei.
- Unterstützung für neuere Standards wie z. B. TLS 1.2.

#### <a name="cons"></a>Nachteile

- Erfordert iOS 6 oder höher.
- Auf WatchOS nicht verfügbar.
- Einige "HttpClient" Features/Optionen sind nicht verfügbar.

### <a name="managed"></a>Verwaltet

Der verwaltete Handler ist der vollständig verwaltete Handler für "HttpClient", der mit früheren Version von Xamarin geliefert wurde.

#### <a name="pros"></a>Pros

- Sie verfügt über die kompatibelste Featuregruppe mit Microsoft .NET und ältere Versionen von Xamarin.

#### <a name="cons"></a>Nachteile

- Es ist nicht vollständig in die Apple-Betriebssysteme integriert und ist TLS 1.0 auf. Sie können möglicherweise nicht mit Webserver sichern oder cloud-Dienste in der Zukunft verbinden.
- Es ist in der Regel sehr viel langsamer Dinge wie Verschlüsselung als die nativen APIs.
- Sie erfordert mehr verwalteten Code, wodurch eine größere app verteilbare.

### <a name="programmatically-setting-the-httpmessagehandler"></a>Programmgesteuertes Festlegen der httpmessagehandler, der für

Sie können auch instanziieren, zusätzlich zu der oben gezeigten Konfiguration projektweit ein `HttpClient` , und fügen Sie die gewünschte `HttpMessageHandler` über den Konstruktor, wie in diesen Codeausschnitten veranschaulicht:

```csharp
// This will use the default message handler for the application; as
// set in the Project Options for the project.
HttpClient client = new HttpClient();

// This will create an HttpClient that explicitly uses the CFNetworkHandler
HttpClient client = new HttpClient(new CFNetworkHandler());

// This will create an HttpClient that explicitly uses NSUrlSessionHandler
HttpClient client = new HttpClient(new NSUrlSessionHandler());
```

Dadurch können Sie einen anderen `HttpMessageHandler` aus, was in deklariert wird die **Projektoptionen** Dialogfeld.

## <a name="ssltls-implementation"></a>SSL/TLS-Implementierung

SSL (Secure Sockets Layer) und sein Nachfolger TLS (Transport Layer Security), bieten Unterstützung für HTTP und andere Netzwerkverbindungen über `System.Net.Security.SslStream`. Xamarin.iOS, Xamarin.tvOS oder des Xamarin.Mac `System.Net.Security.SslStream` Implementierung wird von Apple native SSL/TLS-Implementierung anstatt die verwaltete Implementierung von Mono aufrufen. Native Implementierung von Apple unterstützt TLS 1.2.

> [!WARNING]
> Die anstehende Version Xamarin.Mac 4.8 unterstützt nur macOS 10.9 oder höher.
> Ältere Versionen von Xamarin.Mac unterstützen macOS 10.7 oder höher, aber diese älteren Versionen von MacOS verfügen nicht über ausreichende TLS-Infrastruktur zur Unterstützung von TLS 1.2. Für macOS 10.7 oder macOS 10.8 sollten Sie Xamarin.Mac 4.6 oder niedriger verwenden.

## <a name="app-transport-security"></a>App-Transportsicherheit

Apple _App Transport Security_ (ATS) erzwungen, sichere Verbindungen zwischen Ressourcen für Internet (z. B. die app Back-End-Server) und Ihrer app. ATS wird sichergestellt, dass die gesamte Internetkommunikation entsprechen, um die Verbindung secure best Practices und verhindert versehentliche Offenlegung vertraulicher Informationen, die entweder direkt über Ihre app oder eine Bibliothek, die es verwendet.

Da ATS in apps für iOS 9, TvOS 9 und OS X 10.11 (El Capitan), die standardmäßig aktiviert ist und höher, alle Verbindungen mit `NSUrlConnection`, `CFUrl` oder `NSUrlSession` ATS-sicherheitsanforderungen unterliegen. Wenn Ihre Verbindungen diese Anforderungen nicht erfüllen, werden sie mit einer Ausnahme fehlschlagen.

Basierend auf Ihrer Auswahl HttpClient-Stapel und SSL/TLS-Implementierung, müssen Sie Änderungen an Ihrer app mit ATS ordnungsgemäß funktioniert.

Um mehr über ATS erfahren möchten, informieren Sie sich unsere [App Transport Security Guide](~/ios/app-fundamentals/ats.md).

## <a name="known-issues"></a>Bekannte Probleme

In diesem Abschnitt werden bekannte Probleme bei der TLS-Unterstützung in Xamarin.iOS behandelt.

### <a name="project-failed-to-load-with-error-requested-value-appletls-wasnt-found"></a>Projekt konnte nicht geladen werden, mit der Fehlermeldung "Angeforderter-Wert, auf die AppleTLS wurde nicht gefunden"

Xamarin.iOS 9.8 führt einige neuen Einstellungen enthalten die **csproj** -Datei für eine Xamarin.iOS-Anwendung. Diese Änderungen können Probleme verursachen, wenn das Projekt mit älteren Versionen von Xamarin.iOS geöffnet wird. Im folgende Screenshot ist ein Beispiel für die Fehlermeldung, die in diesem Szenario angezeigt werden kann:

![Screenshot: Fehler beim Laden des Projekts, angeforderte Wert Legacy wurde nicht gefunden.](http-stack-images/tlserror-xs.png)

Dieser Fehler wird verursacht durch die Einführung der `MtouchTlsProvider` auf die Projektdatei im Xamarin.iOS-9.8 festlegen. Ist dies nicht möglich ist, auf die Xamarin.iOS-9.8 aktualisieren (oder höher), die wurde manuell bearbeiten, die **csproj** Datei der Anwendung, entfernen Sie die `MtouchTlsprovider` -Element, und speichern Sie die geänderte Projektdatei.

Der folgende Codeausschnitt zeigt ein Beispiel der `MtouchTlsProvider` Einstellung sieht möglicherweise wie in einem **csproj** Datei:

```xml
<MtouchTlsProvider>Default</MtouchTlsProvider>
```

## <a name="related-links"></a>Verwandte Links

- [Transport Layer Security (TLS)](~/cross-platform/app-fundamentals/transport-layer-security.md)
- [App-Transportsicherheit](~/ios/app-fundamentals/ats.md)
