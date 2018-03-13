---
title: "HttpClient-Stapel und SSL/TLS-Implementierung Selektor für iOS/macOS"
description: "Der Stapel für \"HttpClient\" und der SSL/TLS-Implementierung Auswahlzeiger bestimmt die \"HttpClient\" und SSL/TLS-Implementierung, die von der Xamarin-app für iOS, tvos. außerdem wurden oder MacOS verwendet wird."
ms.topic: article
ms.prod: xamarin
ms.assetid: 12101297-BB04-4410-85F0-A0D41B7E6591
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 06/12/2017
ms.openlocfilehash: eff096b1dca15b9b11038a599987f632bca2352f
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="httpclient-stack-and-ssltls-implementation-selector-for-iosmacos"></a>HttpClient-Stapel und SSL/TLS-Implementierung Selektor für iOS/macOS

## <a name="httpclient-stack-selector"></a>HttpClient-Stack-Selektor

Für Xamarin.iOS Xamarin.tvOS und Xamarin.Mac verfügbar: steuert die `HttpClient` Implementierung zu verwenden. Ein "HttpClient" sein, indem ist die Standardeinstellung weiterhin `HttpWebRequest`, während Sie jetzt optional zu einer Implementierung wechseln können, die native iOS, tvos. außerdem wurden oder MacOS-Transporte verwendet (`NSUrlSession` oder `CFNetwork` je nach Betriebssystem). Der Vorteil ist, kleinere Binärdateien und schnellere Downloads, der Nachteil besteht darin, dass die Ereignisschleife ausgeführt werden, damit asynchrone Vorgänge ausgeführt werden muss.

Projekte verweisen müssen die **System.Net.Http** Assembly.

<a name="Selecting-a-HttpClient-Stack" />

### <a name="selecting-a-httpclient-stack"></a>Auswählen eines Stapels "HttpClient"

Die von Ihrer Anwendung verwendeten "HttpClient" anpassen:

1. Doppelklicken Sie auf die **Projektname** in der **Projektmappen-Explorer** um Optionen für das Projekt zu öffnen.
2. Wechseln Sie zu der **erstellen** Einstellungen für das Projekt (z. B. **iOS-Build** für eine app Xamarin.iOS).
3. Aus der **"HttpClient" Implementierung** Dropdownliste, wählen Sie die "HttpClient" Geben Sie als eines der folgenden: **verwaltet**, **CFNetwork** oder **NSUrlSession**.

[![Wählen Sie "HttpClient" Implementierung von verwalteten CFNetwork oder NSUrlSession](http-stack-images/http-xs-sml.png)](http-stack-images/http-xs.png#lightbox)

<a name="Managed" />

### <a name="managed-default"></a>Verwaltete (Standard)

Der verwaltete Handler ist der vollständig verwalteter HttpClient-Handler, der mit früheren Version von Xamarin geliefert wurde.

#### <a name="pros"></a>IT-Spezialisten:

 - Er verfügt über die meisten kompatibel Featuresatz mit Microsoft .NET und ältere Versionen von Xamarin.

#### <a name="cons"></a>Nachteile:

 - Es ist nicht vollständig in der Apple-Betriebssysteme integriert und auf TLS 1.0 beschränkt ist.
 - Es ist in der Regel sehr viel langsamer Dinge, wie die Verschlüsselung als systemeigene APIs.
 - Sie erfordert mehr verwalteten Code, wodurch eine größere app verteilbarer.

<a name="CFNetwork" />

### <a name="cfnetwork"></a>CFNetwork

Der Handler CFNetwork basierende basiert auf dem systemeigenen `CFNetwork` Framework in iOS 6 und höher verfügbar.

#### <a name="pros"></a>IT-Spezialisten:

 - Systemeigene APIs verwendet für eine bessere Leistung und geringer Größe ausführbarer.
 - Unterstützung für neuere Standards wie TLS 1.2.

#### <a name="cons"></a>Nachteile:

 - Erfordert iOS 6 oder höher.
 - Für WatchOS nicht verfügbar.
 - Einige "HttpClient" Features/Optionen sind nicht verfügbar.

<a name="NSUrlSession" />

### <a name="nsurlsession"></a>NSUrlSession

Der Handler NSURLSession basierende basiert auf dem systemeigenen `NSURLSession` Framework in iOS 7 und höher verfügbar.

#### <a name="pros"></a>IT-Spezialisten:

 - Systemeigene APIs verwendet für eine bessere Leistung und geringer Größe ausführbarer.
 - Für die aktuellsten Standards, z. B. TLS 1.2 unterstützt.

#### <a name="cons"></a>Nachteile:

 - Erfordert iOS 7 oder höher.
 - Einige "HttpClient" Features/Optionen sind nicht verfügbar.

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

## <a name="ssltls-implementation-build"></a>Implementierung von SSL/TLS-build

SSL (Secure Sockets Layer) und dessen Nachfolger – TLS (Transport Layer Security) bieten Unterstützung für HTTP und andere Netzwerkverbindungen über `System.Net.Security.SslStream`. Xamarin.iOS, Xamarin.tvOS oder des Xamarin.Mac `System.Net.Security.SslStream` Implementierung rufen Sie eine Apple systemeigene SSL/TLS-Implementierung anstatt die verwaltete Implementierung von Mono bereitgestellt. Systemeigene Apple-Implementierung unterstützt TLS 1.2.

<a name="Mono" />
> [!WARNING]
> Die **Mono/verwaltete** TLS-Anbieter ist beschränkt, SSL v3 und TLS v1. Dieser TLS-Anbieter wurde als veraltet markiert und ist nicht mehr für Xamarin.iOS Clientanwendungen verfügbar sind. 

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
