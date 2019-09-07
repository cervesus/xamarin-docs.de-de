---
title: HttpClient-Stapel und SSL/TLS-Implementierungs Auswahl für Android
description: Der httpclient-Stapel und die SSL/TLS-Implementierungs Auswahl bestimmen die HttpClient-und SSL/TLS-Implementierung, die von xamarin. Android-Apps verwendet wird.
ms.prod: xamarin
ms.assetid: D7ABAFAB-5CA2-443D-B902-2C7F3AD69CE2
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/20/2018
ms.openlocfilehash: 3b74acee34c367814fbd2a948fe490f4225aee00
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70755391"
---
# <a name="httpclient-stack-and-ssltls-implementation-selector-for-android"></a>HttpClient-Stapel und SSL/TLS-Implementierungs Auswahl für Android

Der httpclient-Stapel und die SSL/TLS-Implementierungs Auswahl bestimmen die HttpClient-und SSL/TLS-Implementierung, die von xamarin. Android-Apps verwendet wird.

Projekte müssen auf die **System .net. http** -Assembly verweisen.

> [!WARNING]
> **April, 2018** – aufgrund erhöhter Sicherheitsanforderungen (einschließlich der PCI-Konformität) erwarten große cloudanbieter und Webserver die Unterstützung von TLS-Versionen, die älter sind als 1,2. Xamarin-Projekte, die in früheren Versionen von Visual Studio erstellt wurden, verwenden standardmäßig ältere Versionen von TLS.
>
> Um sicherzustellen, dass Ihre apps weiterhin mit diesen Servern und Diensten funktionieren, **sollten Sie Ihre xamarin-Projekte mit den `Android HttpClient` unten `Native TLS 1.2` gezeigten-und-Einstellungen aktualisieren und dann die apps für Ihre Benutzer neu erstellen und erneut** bereitstellen.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Die xamarin. Android-httpclient-Konfiguration befindet sich in den **Projektoptionen > Android-Optionen**, und klicken Sie dann auf die Schaltfläche **Erweiterte Optionen** .

Dies sind die empfohlenen Einstellungen für die TLS 1,2-Unterstützung:

[![Visual Studio Android-Optionen](http-stack-images/android-win-sml.png)](http-stack-images/android-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Die HttpClient-Konfiguration von xamarin. Android befindet sich in den **Projektoptionen > Build > Android** -Buildeinstellungen, und klicken Sie auf die Registerkarte **Allgemein** .

Dies sind die empfohlenen Einstellungen für die TLS 1,2-Unterstützung:

[![Android-Optionen Visual Studio für Mac](http-stack-images/android-mac-sml.png)](http-stack-images/android-mac.png#lightbox)

-----

## <a name="alternative-configuration-options"></a>Alternative Konfigurationsoptionen

### <a name="androidclienthandler"></a>AndroidClientHandler

"Androidclienthandler" ist der neue Handler, der an nativen Java/OS-Code delegiert, anstatt alles in verwaltetem Code zu implementieren.
**Dies ist die empfohlene Option.**

#### <a name="pros"></a>Vorteile

- Verwenden Sie die Native API, um eine bessere Leistung und eine kleinere ausführbare Datei
- Unterstützung für die neuesten Standards, z. b. TLS 1,2.

#### <a name="cons"></a>Nachteile

- Erfordert Android 4,1 oder höher.
- Einige Features/Optionen für httpclient sind nicht verfügbar.

### <a name="managed-httpclienthandler"></a>Verwaltet (httpclienthandler)

Der verwaltete Handler ist der vollständig verwaltete httpclient-Handler, der mit früheren xamarin. Android-Versionen ausgeliefert wurde.

#### <a name="pros"></a>Vorteile

- Es ist die am meisten kompatible (Features) mit MS .net und älteren xamarin-Versionen.

#### <a name="cons"></a>Nachteile

- Es ist nicht vollständig in das Betriebssystem integriert (z. b. beschränkt auf TLS 1,0).
- Dies ist in der Regel viel langsamer (z. b. Verschlüsselung) als Native API.
- Er erfordert mehr verwalteten Code, wodurch größere Anwendungen erstellt werden.

### <a name="choosing-a-handler"></a>Auswählen eines Handlers

Die Wahl zwischen `AndroidClientHandler` und `HttpClientHandler` hängt von den Anforderungen Ihrer Anwendung ab. `AndroidClientHandler`wird für die aktuellste Sicherheitsunterstützung empfohlen, z. b.

- Sie benötigen Unterstützung für TLS 1.2 und höher.
- Ihre APP zielt auf Android 4,1 (API 16) oder höher ab.
- Sie benötigen die TLS 1.2 +- `HttpClient`Unterstützung für.
- Sie benötigen keine TLS 1.2 +-unter `WebClient`Stützung für.

`HttpClientHandler`ist eine gute Wahl, wenn Sie TLS 1.2 + Support benötigen, aber Android-Versionen vor Android 4,1 unterstützen müssen. Es ist auch eine gute Wahl, wenn Sie TLS 1.2 +-unter `WebClient`Stützung für benötigen.

Ab xamarin. Android 8,3 wird `HttpClientHandler` standardmäßig das SSL (`btls`) als zugrunde liegender TLS-Anbieter verwendet. Der langweilige SSL-TLS-Anbieter bietet die folgenden Vorteile:

- Es unterstützt TLS 1.2 und höher.
- Alle Android-Versionen werden unterstützt.
- Er bietet Unterstützung für TLS 1.2 und `HttpClient` sowohl `WebClient`für als auch für.

Der Nachteil der Verwendung von langweiligen SSL als zugrunde liegenden TLS-Anbieter besteht darin, dass die Größe des resultierenden APK vergrößert werden kann (es werden ca. 1 MB zusätzlicher APK-Größe pro unterstützter ABI addiert).

Ab xamarin. Android 8,3 ist SSL (`btls`) der standardmäßige TLS-Anbieter. Wenn Sie kein langweiliges SSL verwenden möchten, können Sie die Verlaufs verwaltete SSL-Implementierung wiederherstellen, indem `$(AndroidTlsProvider)` Sie die `legacy` -Eigenschaft auf festlegen (Weitere Informationen zum Festlegen von Buildeigenschaften finden Sie unter [Buildprozess](~/android/deploy-test/building-apps/build-process.md)).

### <a name="programatically-using-androidclienthandler"></a>Programm gesteuert mithilfe von`AndroidClientHandler`

`Xamarin.Android.Net.AndroidClientHandler` Ist eine`HttpMessageHandler` -Implementierung speziell für xamarin. Android.
Instanzen dieser Klasse verwenden die native `java.net.URLConnection` Implementierung für alle HTTP-Verbindungen. Dadurch wird theoretisch eine Erhöhung der http-Leistung und kleinere APK-Größen bereitgestellt.

Dieser Code Ausschnitt ist ein Beispiel für eine explizite Vorgehensweise bei einer einzelnen Instanz der `HttpClient` -Klasse:

```csharp
// Android 4.1 or higher, Xamarin.Android 6.1 or higher
HttpClient client = new HttpClient(new Xamarin.Android.Net.AndroidClientHandler ());
```

> [!NOTE]
> Das zugrunde liegende Android-Gerät muss TLS 1,2 unterstützen (d.h. Android 4,1 und höher). Beachten Sie, dass die offizielle Unterstützung für TLS 1,2 in Android 5.0 und höher ist. Einige Geräte unterstützen jedoch TLS 1,2 in Android 4.1 und höher.

## <a name="ssltls-implementation-build-option"></a>Build-Option für SSL/TLS-Implementierung

Mit dieser Projekt Option wird gesteuert, `HttpClient` welche zugrunde liegende TLS- `WebRequest`Bibliothek von allen Webanforderungen verwendet wird. Standardmäßig ist TLS 1,2 ausgewählt:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Kombinations Feld "TLS/SSL-Implementierung" in Visual Studio](http-stack-images/tls06-vs.png)](http-stack-images/tls05-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![Kombinations Feld "TLS/SSL-Implementierung" in Visual Studio für Mac](http-stack-images/tls06-xs.png)](http-stack-images/tls05-xs.png#lightbox)

-----

Beispiel:

```csharp
var client = new HttpClient();
```

Wenn die HttpClient-Implementierung auf **verwaltet** festgelegt wurde und die TLS-Implementierung auf **native TLS 1.2 +** festgelegt `client` wurde, verwendet das Objekt automatisch `HttpClientHandler` den verwalteten und TLS 1,2 (bereitgestellt von der boringssl-Bibliothek) für seine HTTP-Anforderungen.

Wenn die **HttpClient-Implementierung** `AndroidHttpClient`jedoch auf festgelegt ist, verwenden `HttpClient` alle Objekte die zugrunde liegende Java- `java.net.URLConnection` Klasse und sind vom **TLS/SSL-Implementierungs** Wert nicht betroffen. `WebRequest`-Objekte verwenden die boringssl-Bibliothek.

## <a name="other-ways-to-control-ssltls-configuration"></a>Weitere Möglichkeiten zum Steuern der SSL/TLS-Konfiguration

Es gibt drei Möglichkeiten, wie eine xamarin. Android-Anwendung die TLS-Einstellungen steuern kann:

1. Wählen Sie die HttpClient-Implementierung und die TLS-Standardbibliothek in den Projektoptionen aus.
2. Programm gesteuert mithilfe `Xamarin.Android.Net.AndroidClientHandler`von.
3. Umgebungsvariablen deklarieren (optional).

Der empfohlene Ansatz ist die Verwendung der xamarin. Android-Projektoptionen, um den Standard `HttpMessageHandler` -und TLS-Code für die gesamte APP zu deklarieren. Anschließend können Sie bei Bedarf Objekte Programm gesteuert instanziieren `Xamarin.Android.Net.AndroidClientHandler` . Diese Optionen werden oben beschrieben.

Die dritte Option &ndash; zum Verwenden von &ndash; Umgebungsvariablen wird im folgenden erläutert.

### <a name="declare-environment-variables"></a>Umgebungsvariablen deklarieren

Es gibt zwei Umgebungsvariablen, die mit der Verwendung von TLS in xamarin. Android verknüpft sind:

- `XA_HTTP_CLIENT_HANDLER_TYPE`Diese Umgebungsvariable deklariert den Standard `HttpMessageHandler` Wert, der von der Anwendung verwendet wird. &ndash; Beispiel:

    ```csharp
    XA_HTTP_CLIENT_HANDLER_TYPE=Xamarin.Android.Net.AndroidClientHandler
    ```

- `XA_TLS_PROVIDER`Diese Umgebungsvariable deklariert, welche TLS-Bibliothek verwendet wird, entweder `btls`, `legacy`oder `default` (Dies entspricht dem weglassen dieser Variablen): &ndash;

    ```csharp
    XA_TLS_PROVIDER=btls
    ```

Diese Umgebungsvariable wird festgelegt, indem eine _umgebungsdatei_ zum Projekt hinzugefügt wird. Eine umgebungsdatei ist eine UNIX-formatierte nur-Text-Datei mit einer Buildaktion von **androidenvironment**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Screenshot der Aktion "androidenvironment Build" in Visual Studio](http-stack-images/tls03-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Screenshot der Aktion "androidenvironment Build" in Visual Studio für Mac.](http-stack-images/tls03-xs.png)

-----

Weitere Informationen zu Umgebungsvariablen und xamarin. Android finden Sie im [xamarin. Android-Umgebungs](~/android/deploy-test/environment.md) Handbuch.

## <a name="related-links"></a>Verwandte Links

- [Transport Layer Security (TLS)](~/cross-platform/app-fundamentals/transport-layer-security.md)
