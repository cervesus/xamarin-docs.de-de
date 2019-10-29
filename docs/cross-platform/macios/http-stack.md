---
title: HttpClient-und SSL/TLS-Implementierungs Auswahl für IOS/macOS
description: Der httpclient-Stapel und der SSL/TLS-Implementierungs Selektor bestimmen die HttpClient-und SSL/TLS-Implementierung, die von ihrer xamarin IOS-, tvos-oder macOS-App verwendet wird.
ms.prod: xamarin
ms.assetid: 12101297-BB04-4410-85F0-A0D41B7E6591
author: davidortinau
ms.author: daortin
ms.date: 04/20/2018
ms.openlocfilehash: 3b87eb6ae9e97be4ee34b092efce72c0d97c88b3
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73015847"
---
# <a name="httpclient-and-ssltls-implementation-selector-for-iosmacos"></a>HttpClient-und SSL/TLS-Implementierungs Auswahl für IOS/macOS

Der **HttpClient-Implementierungs Selektor** für xamarin. IOS, xamarin. tvos und xamarin. Mac steuert, welche `HttpClient`-Implementierung verwendet werden soll. Sie können zu einer-Implementierung wechseln, die systemeigene IOS-, tvos-oder macOS-Transporte verwendet (`NSUrlSession` oder `CFNetwork`, abhängig vom Betriebssystem). Der Vorteil ist TLS 1,2-Support, kleinere Binärdateien und schnellere Downloads. der Nachteil ist, dass es erforderlich ist, dass die-Ereignisschleife ausgeführt wird, damit asynchrone Vorgänge ausgeführt werden.

Projekte müssen auf die **System .net. http** -Assembly verweisen.

> [!WARNING]
> **April, 2018** – aufgrund erhöhter Sicherheitsanforderungen (einschließlich der PCI-Konformität) erwarten große cloudanbieter und Webserver die Unterstützung von TLS-Versionen, die älter sind als 1,2. Xamarin-Projekte, die in früheren Versionen von Visual Studio erstellt wurden, verwenden standardmäßig ältere Versionen von TLS.
>
> Um sicherzustellen, dass Ihre apps weiterhin mit diesen Servern und Diensten funktionieren, **sollten Sie Ihre xamarin-Projekte mit der unten gezeigten `NSUrlSession`-Einstellung aktualisieren und dann die apps für Ihre Benutzer neu erstellen und erneut** bereitstellen.

## <a name="selecting-an-httpclient-stack"></a>Auswählen eines httpclient-Stapels

So passen Sie die `HttpClient` an, die von Ihrer APP verwendet wird:

1. Doppelklicken Sie auf den **Projektnamen** im **Projektmappen-Explorer** , um die Projektoptionen zu öffnen.
2. Wechseln Sie zu den Buildeinstellungen für das Projekt (z. b. **IOS-Build** für eine xamarin. IOS-APP).
3. Wählen Sie in der Dropdown Liste **HttpClient-Implementierung** den `HttpClient` Typ als einen der folgenden aus: **nsurlsession** (empfohlen), **CFNetwork**oder **Managed**.

[![wählen Sie die HttpClient-Implementierung aus Managed, CFNetwork oder nsurlsession aus.](http-stack-images/http-xs-sml.png)](http-stack-images/http-xs.png#lightbox)

> [!TIP]
> Für TLS 1,2-Unterstützung wird die `NSUrlSession` Option empfohlen.

### <a name="nsurlsession"></a>NSUrlSession

Der `NSURLSession`basierte Handler basiert auf dem systemeigenen `NSURLSession` Framework, das in ios 7 und höher verfügbar ist. 
**Dies ist die empfohlene Einstellung.**

#### <a name="pros"></a>Vorteile

- Native APIs werden verwendet, um eine bessere Leistung und eine geringere ausführbare Größe zu erzielen.
- Unterstützung für die neuesten Standards, z. b. TLS 1,2.

#### <a name="cons"></a>Nachteile

- Erfordert IOS 7 oder höher.
- Einige `HttpClient` Features/Optionen sind nicht verfügbar.

### <a name="cfnetwork"></a>CFNetwork

Der `CFNetwork`basierte Handler basiert auf dem systemeigenen `CFNetwork` Framework, das in ios 6 und höher verfügbar ist.

#### <a name="pros"></a>Vorteile

- Native APIs werden verwendet, um eine bessere Leistung und eine geringere ausführbare Größe zu erzielen.
- Unterstützung für neuere Standards, z. b. TLS 1,2.

#### <a name="cons"></a>Nachteile

- Erfordert IOS 6 oder höher.
- Nicht verfügbar für watchos.
- Einige Features/Optionen für httpclient sind nicht verfügbar.

### <a name="managed"></a>Verwaltet

Der verwaltete Handler ist der vollständig verwaltete httpclient-Handler, der mit der vorherigen Version von xamarin ausgeliefert wurde.

#### <a name="pros"></a>Vorteile

- Sie verfügt über die kompatibelste Funktion mit Microsoft .net und älteren xamarin-Versionen.

#### <a name="cons"></a>Nachteile

- Sie ist nicht vollständig in die Apple-Betriebssysteme integriert und ist auf TLS 1,0 beschränkt. Möglicherweise ist es möglicherweise nicht möglich, in Zukunft eine Verbindung mit sicheren Webservern oder Clouddiensten herzustellen.
- Dies ist in der Regel viel langsamer als bei der Verschlüsselung als bei den nativen APIs.
- Hierfür ist mehr verwalteter Code erforderlich, wodurch eine größere verteilbare App erstellt werden kann.

### <a name="programmatically-setting-the-httpmessagehandler"></a>Programm gesteuertes Festlegen von httpmessagehandler

Neben der oben gezeigten projektweiten Konfiguration können Sie auch eine `HttpClient` instanziieren und die gewünschte `HttpMessageHandler` über den Konstruktor einfügen, wie in den folgenden Code Ausschnitten veranschaulicht:

```csharp
// This will use the default message handler for the application; as
// set in the Project Options for the project.
HttpClient client = new HttpClient();

// This will create an HttpClient that explicitly uses the CFNetworkHandler
HttpClient client = new HttpClient(new CFNetworkHandler());

// This will create an HttpClient that explicitly uses NSUrlSessionHandler
HttpClient client = new HttpClient(new NSUrlSessionHandler());
```

Dadurch ist es möglich, eine andere `HttpMessageHandler` von dem, was im Dialogfeld " **Projektoptionen** " deklariert ist, zu verwenden.

## <a name="ssltls-implementation"></a>SSL/TLS-Implementierung

SSL (Secure Socket Layer) und dessen Nachfolger, TLS (Transport Layer Security), bieten Unterstützung für http und andere Netzwerkverbindungen über `System.Net.Security.SslStream`. Xamarin. IOS, xamarin. tvos oder die `System.Net.Security.SslStream` Implementierung von xamarin. Mac Ruft die native SSL/TLS-Implementierung von Apple auf, anstatt die verwaltete Implementierung zu verwenden, die von Mono bereitgestellt wird. Die native Implementierung von Apple unterstützt TLS 1,2.

> [!WARNING]
> Die anstehende Version Xamarin.Mac 4.8 unterstützt nur macOS 10.9 oder höher.
> Ältere Versionen von Xamarin.Mac unterstützen macOS 10.7 oder höher, aber diese älteren Versionen von MacOS verfügen nicht über ausreichende TLS-Infrastruktur zur Unterstützung von TLS 1.2. Für macOS 10.7 oder macOS 10.8 sollten Sie Xamarin.Mac 4.6 oder niedriger verwenden.

## <a name="app-transport-security"></a>App-Transportsicherheit

Die _App-Transport Sicherheit (app Transport Security_ , ATS) von Apple erzwingt sichere Verbindungen zwischen Internetressourcen (z. b. dem Back-End-Server der APP) und ihrer app. Mithilfe von ATS wird sichergestellt, dass die gesamte Internetkommunikation den bewährten Methoden der sicheren Verbindung entspricht. Dadurch wird die versehentliche Offenlegung vertraulicher Informationen entweder direkt über Ihre APP oder eine von ihr verwendete Bibliothek verhindert.

Da ATS in apps, die für IOS 9, tvos 9 und OS X 10,11 (El Capitan) und höher erstellt werden, standardmäßig aktiviert ist, gelten für alle Verbindungen mit `NSUrlConnection`, `CFUrl` oder `NSUrlSession` die Sicherheitsanforderungen. Wenn Ihre Verbindungen diese Anforderungen nicht erfüllen, können Sie mit einer Ausnahme fehlschlagen.

Basierend auf Ihrer Auswahl von HttpClient-Stapel und SSL/TLS-Implementierung müssen Sie möglicherweise Änderungen an Ihrer APP vornehmen, damit Sie ordnungsgemäß mit ATS funktioniert.

Weitere Informationen zu ATS finden Sie in unserem Handbuch zur [App-Transport Sicherheit](~/ios/app-fundamentals/ats.md).

## <a name="known-issues"></a>Bekannte Probleme

In diesem Abschnitt werden bekannte Probleme mit der TLS-Unterstützung in xamarin. IOS behandelt.

### <a name="project-failed-to-load-with-error-requested-value-appletls-wasnt-found"></a>Fehler beim Laden des Projekts. Fehler "angeforderter Wert wurde nicht gefunden".

Xamarin. IOS 9,8 hat einige neue Einstellungen eingeführt, die die **csproj** -Datei für eine xamarin. IOS-Anwendung enthielt. Diese Änderungen können Probleme verursachen, wenn das Projekt mit älteren Versionen von xamarin. IOS geöffnet wird. Der folgende Screenshot zeigt ein Beispiel für die Fehlermeldung, die in diesem Szenario angezeigt werden kann:

![Screenshot des Fehlers beim Laden des Projekts. der angeforderte Wert "Legacy" wurde nicht gefunden.](http-stack-images/tlserror-xs.png)

Dieser Fehler wird durch die Einführung der `MtouchTlsProvider` Einstellung in die Projektdatei in xamarin. IOS 9,8 verursacht. Wenn es nicht möglich ist, auf xamarin. IOS 9,8 (oder höher) zu aktualisieren, können Sie umgehen, indem Sie die **csproj** -Datei Anwendung manuell bearbeiten, das `MtouchTlsprovider` Element entfernen und dann die geänderte Projektdatei speichern.

Der folgende Code Ausschnitt ist ein Beispiel dafür, wie die `MtouchTlsProvider` Einstellung in einer **csproj** -Datei aussehen kann:

```xml
<MtouchTlsProvider>Default</MtouchTlsProvider>
```

## <a name="related-links"></a>Verwandte Links

- [Transport Layer Security (TLS)](~/cross-platform/app-fundamentals/transport-layer-security.md)
- [App-Transportsicherheit](~/ios/app-fundamentals/ats.md)
