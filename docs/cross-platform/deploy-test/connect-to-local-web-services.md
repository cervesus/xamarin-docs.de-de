---
title: Herstellen einer Verbindung mit lokalen Webdiensten aus iOS-Simulatoren und Android-Emulatoren
description: In diesem Artikel wird erläutert, wie eine mobile Xamarin-Anwendung, die im iOS-Simulator oder Android-Emulator ausgeführt wird, einen lokal ausgeführten ASP.NET Core-Webdienst nutzen kann.
ms.prod: xamarin
ms.assetid: FD8FE199-898B-4841-8041-CC9CA1A00917
author: davidbritch
ms.author: dabritch
ms.date: 10/16/2019
ms.openlocfilehash: a29cc650d9aa3976b6fd7aaaa82e233317684335
ms.sourcegitcommit: 20c645f41620d5124da75943de1b690261d00660
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72426562"
---
# <a name="connect-to-local-web-services-from-ios-simulators-and-android-emulators"></a>Herstellen einer Verbindung mit lokalen Webdiensten aus iOS-Simulatoren und Android-Emulatoren

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todorest/)

Webdienste werden von vielen mobilen Anwendungen genutzt. Während der Entwicklungsphase ist es üblich, einen Webdienst lokal bereitzustellen und aus einer mobilen Anwendung zu nutzen, die im iOS-Simulator oder Android-Emulator ausgeführt wird. Auf diese Weise wird die Notwendigkeit vermieden, den Webdienst an einem gehosteten Endpunkt bereitzustellen, und es wird eine unkomplizierte Debugerfahrung ermöglicht, weil sowohl die mobile Anwendung als auch der Webdienst lokal ausgeführt werden.

Mobile Anwendungen, die im iOS-Simulator oder Android-Emulator ausgeführt werden, können wie folgt ASP.NET Core-Webdienste nutzen, die lokal ausgeführt und über HTTP verfügbar gemacht werden:

- Im iOS-Simulator ausgeführte Anwendungen können eine Verbindung mit den lokalen HTTP-Webdiensten über die IP-Adresse Ihres Computers oder über den `localhost`-Hostnamen herstellen. Beispielsweise kann eine Anwendung, die im iOS-Simulator ausgeführt wird, vorausgesetzt, dass ein lokaler HTTP-Webdienst einen GET-Vorgang über den relativen URI `/api/todoitems/` verfügbar macht, den Vorgang nutzen, indem sie eine GET-Anforderung an `http://localhost:<port>/api/todoitems/` sendet.
- Anwendungen, die im Android-Emulator ausgeführt werden, können eine Verbindung mit lokalen HTTP-Webdiensten über die Adresse `10.0.2.2` herstellen, bei der es sich um einen Alias für Ihre Host-Loopbackschnittstelle handelt (`127.0.0.1` auf Ihrem Entwicklungscomputer). Beispielsweise kann eine Anwendung, die im Android-Emulator ausgeführt wird, vorausgesetzt, dass ein lokaler HTTP-Webdienst einen GET-Vorgang über den relativen URI `/api/todoitems/` verfügbar macht, den Vorgang nutzen, indem sie eine GET-Anforderung an `http://10.0.2.2:<port>/api/todoitems/` sendet.

Bei einer Anwendung, die im iOS-Simulator oder Android-Emulator ausgeführt wird sind jedoch zusätzliche Arbeiten notwendig, damit diese lokale Webdienste nutzen kann, die über HTTPS verfügbar gemacht werden. Für dieses Szenario gehen Sie wie folgt vor:

1. Erstellen Sie ein selbstsigniertes Entwicklungszertifikat auf Ihrem Computer. Weitere Informationen finden Sie unter [Erstellen eines Entwicklungszertifikats](#create-a-development-certificate).
1. Konfigurieren Sie Ihr Projekt so, dass es den verwalteten `HttpClient`-Netzwerkstapel für Ihren Debugbuild verwendet. Weitere Informationen finden Sie unter [Konfigurieren Ihres Projekts](#configure-your-project).
1. Geben Sie die Adresse Ihres lokalen Computers an. Weitere Informationen finden Sie unter [Angeben der Adresse des lokalen Computers](#specify-the-local-machine-address).
1. Umgehen Sie die Sicherheitsüberprüfung des lokalen Entwicklungszertifikats. Weitere Informationen finden Sie unter [Umgehen der Zertifikatsicherheitsüberprüfung](#bypass-the-certificate-security-check).

Jeder Aspekt wird nacheinander erläutert.

## <a name="create-a-development-certificate"></a>Erstellen eines Entwicklungszertifikats

Durch das Installieren des ASP.NET Core SDK wird das ASP.NET Core-HTTPS-Entwicklungszertifikat im lokalen Benutzerzertifikatspeicher installiert. Allerdings ist das Zertifikat, auch wenn es installiert wurde, nicht vertrauenswürdig. Damit das Zertifikat vertrauenswürdig wird, führen Sie den folgenden, einmaligen Schritt durch, um das dotnet-Tool `dev-certs` auszuführen:

```console
dotnet dev-certs https --trust
```

Der folgende Befehl stellt Hilfe zum `dev-certs`-Tool bereit:

```console
dotnet dev-certs https --help
```

Alternativ erkennt Visual Studio, wenn Sie ein ASP.NET Core 2.1-Projekt (oder höher) ausführen, das HTTPS verwendet, ob das Entwicklungszertifikat fehlt, und bietet an, es zu installieren und ihm zu vertrauen.

> [!NOTE]
> Das ASP.NET Core-HTTPS-Entwicklungszertifikat ist selbstsigniert.

Weitere Informationen zum Aktivieren von lokalem HTTPS auf Ihrem Computer finden Sie unter [Aktivieren von lokalem HTTPS](/aspnet/core/getting-started#enable-local-https).

## <a name="configure-your-project"></a>Konfigurieren des Projekts

Xamarin-Anwendungen, die unter iOS und Android ausgeführt werden, können angeben, welcher Netzwerkstapel von der `HttpClient`-Klasse verwendet wird, wobei ein veralteter Netzwerkstapel und native Netzwerkstapel zur Auswahl stehen. Der verwaltete Stapel kann bietet ein hohes Maß an Kompatibilität mit vorhandenem .NET-Code, ist aber auf TLS 1.0 beschränkt und kann langsamer sein sowie zu einer größeren ausführbaren Datei führen. Die nativen Stapel können schneller sein und höhere Sicherheit gewährleisten, stellen aber eventuell nicht alle Funktionen der `HttpClient`-Klasse bereit.

### <a name="ios"></a>iOS

Xamarin-Anwendungen, die unter iOS ausgeführt werden, können den verwalteten Netzwerkstapel verwenden oder die nativen Netzwerkstapel `CFNetwork` oder `NSUrlSession`. Standardmäßig verwenden neue iOS-Plattformprojekte den Netzwerkstapel `NSUrlSession`, um TLS 1.2 zu unterstützen, und verwenden native APIs für eine bessere Leistung und eine kleinere ausführbare Datei.

Wenn jedoch eine Anwendung für Entwicklertests eine Verbindung mit einem sicheren Webdienst herstellen muss, der lokal ausgeführt wird, ist es einfacher, den verwalteten Netzwerkstapel zu verwenden. Aus diesem Grund wird empfohlen, Simulatorbuildprofile zum Debuggen so festzulegen,dass sie den verwalteten Netzwerkstapel verwenden, und Releasebuildprofile so, dass sie den Netzwerkstapel `NSUrlSession` verwenden. Jeder Netzwerkstapel kann programmgesteuert oder durch einen Selektor in den Projektoptionen festgelegt werden. Weitere Informationen finden Sie unter [HttpClient- und SSL/TLS-Implementierungsselektor für iOS/macOS](~/cross-platform/macios/http-stack.md).

### <a name="android"></a>Android

Xamarin-Anwendungen, die unter Android ausgeführt werden, können den verwalteten Netzwerkstapel `HttpClientHandler` verwenden oder den nativen Netzwerkstapel `AndroidClientHandler`. Standardmäßig verwenden neue Android-Plattformprojekte den Netzwerkstapel `AndroidClientHandler`, um TLS 1.2 zu unterstützen, und verwenden native APIs für eine bessere Leistung und eine kleinere ausführbare Datei. Weitere Informationen zu Android-Netzwerkstapeln finden Sie unter [HttpClient-Stapel und SSL/TLS-Implementierungsselektor für Android](~/android/app-fundamentals/http-stack.md).

## <a name="specify-the-local-machine-address"></a>Angeben der Adresse des lokalen Computers

iOS-Simulator und Android-Emulator gewähren beide Zugriff auf sichere Webdienste, die auf Ihrem lokalen Computer ausgeführt werden. Die Adresse des lokalen Computers ist jedoch für jeden anders.

### <a name="ios"></a>iOS

Der iOS-Simulator verwendet das Netzwerk des Hostcomputers. Daher können im Simulator ausgeführte Anwendungen eine Verbindung mit Webdiensten, die auf Ihrem lokalen Computer ausgeführt werden, über die IP-Adresse Ihres Computers oder über den `localhost`-Hostnamen herstellen. Beispielsweise kann eine Anwendung, die im iOS-Simulator ausgeführt wird, vorausgesetzt, dass ein lokaler, sicherer Webdienst einen GET-Vorgang über den relativen URI `/api/todoitems/` verfügbar macht, den Vorgang nutzen, indem sie eine GET-Anforderung an `https://localhost:<port>/api/todoitems/` sendet.

> [!NOTE]
> Wenn eine mobile Anwendung im iOS-Simulator aus Windows heraus ausgeführt wird, wird die Anwendung im [iOS-Remotesimulator für Windows](~/tools/ios-simulator/index.md) angezeigt. Die Anwendung wird allerdings auf dem gekoppelten Mac ausgeführt. Es gibt also für eine iOS-Anwendung, die auf einem Mac ausgeführt wird, keinen localhost-Zugriff auf einen Webdienst, der unter Windows ausgeführt wird.

### <a name="android"></a>Android

Jede Instanz des Android-Emulators ist von den Netzwerkschnittstellen Ihres Entwicklungscomputers isoliert und wird hinter einem virtuellen Router ausgeführt. Aus diesem Grund kann ein emuliertes Gerät Ihren Entwicklungscomputer oder andere Emulatorinstanzen im Netzwerk nicht sehen.

Der virtuelle Router für jeden Emulator verwaltet jedoch einen speziellen Netzwerkadressraum, der vorab zugeordnete Adressen enthält,wobei die `10.0.2.2`-Adressen ein Alias für Ihre Host-Loopback-Schnittstelle sind („127.0.0.1“ auf Ihrem Entwicklungscomputer). Deshalb kann eine Anwendung, die im Android-Emulator ausgeführt wird, vorausgesetzt, dass ein lokaler, sicherer Webdienst einen GET-Vorgang über den relativen URI `/api/todoitems/` verfügbar macht, den Vorgang nutzen, indem sie eine GET-Anforderung an `https://10.0.2.2:<port>/api/todoitems/` sendet.

### <a name="xamarinforms-example"></a>Xamarin.Forms-Beispiel

In einer Xamarin.Forms-Anwendung kann die [`Device`](xref:Xamarin.Forms.Device)-Klasse verwendet werden, um die Plattform zu erkennen, auf der die Anwendung ausgeführt wird. Der entsprechende Hostname, der Zugriff auf lokale, sichere Webdienste ermöglicht, kann dann wie folgt festgelegt werden:

```csharp
public static string BaseAddress =
    Device.RuntimePlatform == Device.Android ? "https://10.0.2.2:5001" : "https://localhost:5001";
public static string TodoItemsUrl = $"{BaseAddress}/api/todoitems/";
```

## <a name="bypass-the-certificate-security-check"></a>Umgehen der Zertifikatsicherheitsüberprüfung

Der Versuch, einen lokalen, sicheren Webdienst aus einer Anwendung aufzurufen, die im iOS-Simulator oder Android-Emulator ausgeführt wird, führt dazu,dass eine `HttpRequestException`-Ausnahme ausgelöst wird, selbst wenn auf jeder Plattform der verwaltete Netzwerkstapel verwendet wird. Der Grund hierfür ist, dass das lokale HTTPS-Entwicklungszertifikat selbstsigniert ist und selbstsignierte Zertifikate in iOS und Android nicht vertrauenswürdig sind.

Aus diesem Grund ist es erforderlich, SSL-Fehler zu ignorieren, wenn eine Anwendung einen lokalen, sicheren Webdienst nutzt. Der erforderliche Mechanismus, um dies zu erreichen, ist zurzeit unter iOS und Android unterschiedlich.

### <a name="ios"></a>iOS

SSL-Fehler können Sie bei Verwendung des verwalteten Netzwerkstapels unter iOS für lokale sichere Webdienste ignorieren, indem Sie die Eigenschaft `ServicePointManager.ServerCertificateValidationCallback` auf einen Rückruf festlegen, der das Ergebnis der Zertifikatsicherheitsüberprüfung für das lokale HTTPS-Entwicklungszertifikat ignoriert:

```csharp
#if DEBUG
    System.Net.ServicePointManager.ServerCertificateValidationCallback += (sender, certificate, chain, sslPolicyErrors) =>
    {
        if (certificate.Issuer.Equals("CN=localhost"))
            return true;
        return sslPolicyErrors == System.Net.Security.SslPolicyErrors.None;
    };
#endif
```

In diesem Codebeispiel wird das Ergebnis der Serverzertifikatvalidierung zurückgegeben, wenn das Zertifikat, das validiert wurde, kein `localhost`-Zertifikat ist. Für dieses Zertifikat wird das Ergebnis der Validierung ignoriert, und `true` wird zurückgegeben, was anzeigt, dass das Zertifikat gültig ist. Dieser Code sollte der `AppDelegate.FinishedLaunching`-Methode unter iOS vor dem Aufruf der `LoadApplication(new App())`-Methode hinzugefügt werden.

> [!NOTE]
> Die nativen Netzwerkstapel unter iOS klinken sich nicht in den `ServerCertificateValidationCallback` ein.

### <a name="android"></a>Android

SSL-Fehler können Sie bei Verwendung sowohl des verwalteten als auch nativen `AndroidClientHandler`-Netzwerkstapels unter Android für lokale sichere Webdienste ignorieren, indem Sie die Eigenschaft `ServerCertificateCustomValidationCallback` auf einem `HttpClientHandler`-Objekt auf einen Rückruf festlegen, der das Ergebnis der Zertifikatsicherheitsüberprüfung für das lokale HTTPS-Entwicklungszertifikat ignoriert:

```csharp
public HttpClientHandler GetInsecureHandler()
{
    var handler = new HttpClientHandler();
    handler.ServerCertificateCustomValidationCallback = (message, cert, chain, errors) =>
    {
        if (cert.Issuer.Equals("CN=localhost"))
            return true;
        return errors == System.Net.Security.SslPolicyErrors.None;
    };
    return handler;
}
```

In diesem Codebeispiel wird das Ergebnis der Serverzertifikatvalidierung zurückgegeben, wenn das Zertifikat, das validiert wurde, kein `localhost`-Zertifikat ist. Für dieses Zertifikat wird das Ergebnis der Validierung ignoriert, und `true` wird zurückgegeben, was anzeigt, dass das Zertifikat gültig ist. Das resultierende `HttpClientHandler`-Objekt sollte als Argument an den `HttpClient`-Konstruktor übergeben werden.

## <a name="related-links"></a>Verwandte Links

- [TodoREST (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todorest/)
- [Aktivieren von lokalem HTTPS](/aspnet/core/getting-started#enable-local-https)
- [HttpClient- und SSL/TLS-Implementierungsselektor für iOS/macOS](~/cross-platform/macios/http-stack.md)
- [HttpClient-Stapel und SSL/TLS-Implementierungsselektor für Android](~/android/app-fundamentals/http-stack.md)
