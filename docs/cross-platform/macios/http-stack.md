---
title: "\"HttpClient\" und SSL/TLS-Implementierung Selektor für iOS/macOS"
description: Der Stapel für "HttpClient" und SSL/TLS-Implementierung Auswahl bestimmt die "HttpClient" und SSL/TLS-Implementierung, die von der Xamarin-app für iOS, tvos. außerdem wurden oder MacOS verwendet wird.
ms.prod: xamarin
ms.assetid: 12101297-BB04-4410-85F0-A0D41B7E6591
author: asb3993
ms.author: amburns
ms.date: 04/20/2018
ms.openlocfilehash: 9de2c97933bd33111a751be51e06dffe09794f15
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782267"
---
# <a name="httpclient-and-ssltls-implementation-selector-for-iosmacos"></a>"HttpClient" und SSL/TLS-Implementierung Selektor für iOS/macOS

Die **"HttpClient" Implementierung Selektor** Xamarin.iOS, Xamarin.tvOS und Xamarin.Mac steuert die `HttpClient` Implementierung zu verwenden. Wechseln können Sie eine Implementierung, iOS, tvos. außerdem wurden oder MacOS native Transporte verwendet (`NSUrlSession` oder `CFNetwork`, je nachdem, auf dem Betriebssystem). Der Vorteil ist TLS 1.2-Unterstützung, kleinere Binärdateien und schnellere downloads. Der Nachteil ist, dass dies erfordert, dass die Ereignisschleife ausgeführt werden, damit asynchrone Vorgänge ausgeführt werden.

Projekte verweisen müssen die **System.Net.Http** Assembly.

> [!WARNING]
> **April, 2018** – aufgrund der erhöhten Sicherheit Anforderungen, einschließlich der PCI-Konformität Hauptversion Cloud-Anbieter und der Webserver unterstützen TLS-Versionen, die älter sind als 1.2 zu beenden.  Xamarin-Projekte erstellt, die in früheren Versionen von Visual Studio standardmäßig die frühere Versionen von TLS zu verwenden.
>
> Um sicherzustellen, dass Ihre apps mit diesen Servern und -Dienste, funktionieren weiterhin **sollten Sie Ihre Projekte Xamarin mit Aktualisieren der `NSUrlSession` festlegen wie unten gezeigt, klicken Sie dann erneut erstellen und erneut bereitstellen Ihrer apps** für Ihre Benutzer.

<a name="Selecting-a-HttpClient-Stack" />

### <a name="selecting-a-httpclient-stack"></a>Auswählen eines Stapels "HttpClient"

Anpassen der `HttpClient` von Ihrer Anwendung verwendet wird:

1. Doppelklicken Sie auf die **Projektname** in der **Projektmappen-Explorer** um Optionen für das Projekt zu öffnen.
2. Wechseln Sie zu der **erstellen** Einstellungen für das Projekt (z. B. **iOS-Build** für eine app Xamarin.iOS).
3. Aus der **"HttpClient" Implementierung** Dropdownliste, wählen Sie die `HttpClient` Geben Sie als eines der folgenden: **NSUrlSession** (empfohlen), **CFNetwork**, oder  **Verwaltet**.

[![Wählen Sie "HttpClient" Implementierung von verwalteten CFNetwork oder NSUrlSession](http-stack-images/http-xs-sml.png)](http-stack-images/http-xs.png#lightbox)

> [!TIP]
> Für TLS 1.2-Unterstützung der `NSUrlSession` Option wird empfohlen.

<a name="NSUrlSession" />

### <a name="nsurlsession"></a>NSUrlSession

Die `NSURLSession`-Basis Handler basiert auf dem systemeigenen `NSURLSession` Framework in iOS 7 und höher verfügbar. 
**Dies ist die empfohlene Einstellung.**

#### <a name="pros"></a>IT-Spezialisten

- Systemeigene APIs verwendet für eine bessere Leistung und geringer Größe ausführbarer.
- Unterstützung für die aktuellsten Standards, z. B. TLS 1.2.

#### <a name="cons"></a>Nachteile

- Erfordert iOS 7 oder höher.
- Einige `HttpClient` Funktionen-Optionen sind nicht verfügbar.

<a name="CFNetwork" />

### <a name="cfnetwork"></a>CFNetwork

Die `CFNetwork`-Basis Handler basiert auf dem systemeigenen `CFNetwork` Framework in iOS 6 und höher verfügbar.

#### <a name="pros"></a>IT-Spezialisten

- Systemeigene APIs verwendet für eine bessere Leistung und geringer Größe ausführbarer.
- Unterstützung für neuere Standards wie TLS 1.2.

#### <a name="cons"></a>Nachteile

- Erfordert iOS 6 oder höher.
- Für WatchOS nicht verfügbar.
- Einige "HttpClient" Features/Optionen sind nicht verfügbar.

<a name="Managed" />

### <a name="managed"></a>Verwaltet

Der verwaltete Handler ist der vollständig verwalteter HttpClient-Handler, der mit früheren Version von Xamarin geliefert wurde.

#### <a name="pros"></a>IT-Spezialisten

- Er verfügt über die meisten kompatibel Featuresatz mit Microsoft .NET und ältere Versionen von Xamarin.

#### <a name="cons"></a>Nachteile

- Es ist nicht vollständig in der Apple-Betriebssysteme integriert und auf TLS 1.0 beschränkt ist. Sie können möglicherweise nicht mit sicheren Webserver oder cloud-Dienste in der Zukunft verbinden.
- Es ist in der Regel sehr viel langsamer Dinge, wie die Verschlüsselung als systemeigene APIs.
- Sie erfordert mehr verwalteten Code, wodurch eine größere app verteilbarer.

### <a name="programmatically-setting-the-httpmessagehandler"></a>Programmgesteuertes Festlegen der HttpMessageHandler

Neben der oben gezeigten Projektweite-Konfiguration, können Sie auch Instanziieren einer `HttpClient` und fügen die gewünschte `HttpMessageHandler` durch den Konstruktor, wie im folgenden Codeausschnitte veranschaulicht:

```csharp
// This will use the default message handler for the application; as
// set in the Project Options for the project.
HttpClient client = new HttpClient();

// This will create an HttpClient that explicitly uses the CFNetworkHandler
HttpClient client = new HttpClient(new CFNetworkHandler());

// This will create an HttpClient that explicitly uses NSUrlSessionHandler
HttpClient client = new HttpClient(new NSUrlSessionHandler());
```

Dies ermöglicht es zu einem anderen `HttpMessageHandler` von Was deklariert wird die **Projektoptionen** Dialogfeld.

<a name="New-SSL-TLS-implementation-build-option" />
<a name="Selecting-a-SSL-TLS-implementation" />
<a name="Apple-TLS" />

## <a name="ssltls-implementation"></a>SSL/TLS-Implementierung

SSL (Secure Sockets Layer) und dessen Nachfolger – TLS (Transport Layer Security) bieten Unterstützung für HTTP und andere Netzwerkverbindungen über `System.Net.Security.SslStream`. Xamarin.iOS, Xamarin.tvOS oder des Xamarin.Mac `System.Net.Security.SslStream` Implementierung rufen Sie eine Apple systemeigene SSL/TLS-Implementierung anstatt die verwaltete Implementierung von Mono bereitgestellt. Systemeigene Apple-Implementierung unterstützt TLS 1.2.

<a name="App-Transport-Security" />

## <a name="app-transport-security"></a>App-Transportsicherheit

Apple _App Transportsicherheit_ (ATS) erzwingt, sichere Verbindungen zwischen Internet-Ressourcen (z. B. die app-Back-End-Server) und Ihre app. ATS wird sichergestellt, dass die gesamte Internetkommunikation entsprechen, um die Verbindung secure bewährte Methoden um versehentlichen Offenlegung vertraulicher Informationen entweder direkt über Ihre app oder eine Bibliothek, die es benötigt, ist das verhindern.

Da ATS in apps für iOS 9, tvos. außerdem wurden 9 und OS X 10.11 (El Capitan) erstellten standardmäßig aktiviert ist und höher, alle Verbindungen mit `NSUrlConnection`, `CFUrl` oder `NSUrlSession` werden unterliegen ATS sicherheitsanforderungen. Wenn Ihre Verbindungen diese Anforderungen nicht erfüllen, werden sie mit einer Ausnahme fehl.

Basierend auf Ihrer Auswahl für "HttpClient" Stack "und" SSL/TLS-Implementierung, müssen Sie Änderungen an Ihrer app mit ATS ordnungsgemäß funktioniert.

Um weitere Informationen zu ATS zu suchen, finden Sie unter unsere [App Transport Sicherheitshandbuch](~/ios/app-fundamentals/ats.md).

## <a name="known-issues"></a>Bekannte Probleme

In diesem Abschnitt werden bekannte Probleme bei der TLS-Unterstützung in Xamarin.iOS behandelt werden.

### <a name="project-failed-to-load-with-error-requested-value-appletls-wasnt-found"></a>Projekt konnte nicht geladen werden, mit dem Fehler "Angeforderte-Wert, der AppleTLS nicht gefunden wurde"

Xamarin.iOS 9.8 eingeführt wurden einige neue Einstellungen enthalten die **csproj** Datei für eine Anwendung Xamarin.iOS. Diese Änderung können Probleme verursachen, wenn das Projekt mit älteren Versionen von Xamarin.iOS geöffnet wird. Der folgende Screenshot ist ein Beispiel für die Fehlermeldung, die in diesem Szenario angezeigt werden kann:

![Screenshot der Fehler beim Laden des Projekts, angeforderte Wert Legacy wurde nicht gefunden.](http-stack-images/tlserror-xs.png)

Dieser Fehler wird verursacht, durch die Einführung der `MtouchTlsProvider` auf die Projektdatei in Xamarin.iOS 9.8 festlegen. Wenn es nicht möglich ist, um Xamarin.iOS 9.8 aktualisieren (oder höher) ist, wird die Arbeit ca. manuell bearbeiten, die **csproj** Datei der Anwendung, entfernen Sie die `MtouchTlsprovider` -Element, und speichern Sie die geänderte Datei.

Der folgende Codeausschnitt zeigt ein Beispiel der `MtouchTlsProvider` Einstellung sähe wie z. B. innerhalb einer **csproj** Datei:

```xml
<MtouchTlsProvider>Default</MtouchTlsProvider>
```

## <a name="related-links"></a>Verwandte Links

- [Transport Layer Security (TLS)](~/cross-platform/app-fundamentals/transport-layer-security.md)
- [App-Transportsicherheit](~/ios/app-fundamentals/ats.md)
