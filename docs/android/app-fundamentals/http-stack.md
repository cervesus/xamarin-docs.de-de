---
title: HttpClient-Stapel und SSL/TLS-Implementierungsauswahl für Android
description: Die HttpClient-Stapel und SSL/TLS-Implementierung Selektoren bestimmen die "HttpClient" und SSL/TLS-Implementierung, die von den Xamarin.Android-apps verwendet werden.
ms.prod: xamarin
ms.assetid: D7ABAFAB-5CA2-443D-B902-2C7F3AD69CE2
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/20/2018
ms.openlocfilehash: a3704552c8fc147588919ecdde2813e831237d89
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "59239900"
---
# <a name="httpclient-stack-and-ssltls-implementation-selector-for-android"></a>HttpClient-Stapel und SSL/TLS-Implementierungsauswahl für Android

Die HttpClient-Stapel und SSL/TLS-Implementierung Selektoren bestimmen die "HttpClient" und SSL/TLS-Implementierung, die von den Xamarin.Android-apps verwendet werden.

Projekte verweisen müssen die **System.Net.Http** Assembly.

> [!WARNING]
> **April 2018** – erhöhte Sicherheit aufgrund der Anforderungen, einschließlich der PCI-Compliance Haupt-Cloudanbieter und Webserver werden erwartet, dass die Unterstützung von TLS-Versionen älter als 1.2 eingestellt.  Xamarin-Projekte erstellt, die in früheren Versionen von Visual Studio standardmäßig die ältere Versionen von TLS zu verwenden.
>
> Um sicherzustellen, dass Ihre apps mit diesen Servern und Diensten, funktionieren weiterhin, **sollten Sie Ihre Xamarin-Projekte mit aktualisieren die `Android HttpClient` und `Native TLS 1.2` unten angezeigten Einstellungen klicken Sie dann erneut erstellen und erneut bereitstellen Ihrer apps** auf Ihre Benutzer sind.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Die Xamarin.Android-HttpClient-Konfiguration befindet sich in **Projektoptionen > Android-Optionen**, klicken Sie dann auf die **erweiterte Optionen** Schaltfläche.

Dies sind die empfohlenen Einstellungen für TLS 1.2-Unterstützung:

[![Visual Studio Android Options](http-stack-images/android-win-sml.png)](http-stack-images/android-win.png#lightbox)


# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Die Xamarin.Android-HttpClient-Konfiguration befindet sich in **Projektoptionen > Erstellen > Android-Build** Einstellungen, und klicken Sie auf die **allgemeine** Registerkarte.

Dies sind die empfohlenen Einstellungen für TLS 1.2-Unterstützung:

[![Visual Studio für Mac-Android-Optionen](http-stack-images/android-mac-sml.png)](http-stack-images/android-mac.png#lightbox)

-----

## <a name="alternative-configuration-options"></a>Alternative Konfiguration-Optionen

### <a name="androidclienthandler"></a>AndroidClientHandler

AndroidClientHandler ist der neue Handler, der Delegaten in systemeigenen Java/OS-Code statt alles, was in verwaltetem Code zu implementieren.
**Dies ist die empfohlene Option.**

#### <a name="pros"></a>Pros

- Verwenden Sie systemeigene API für eine bessere Leistung und geringere Größe der ausführbaren Datei ein.
- Die Unterstützung für die neuesten Standards, z. B. TLS 1.2.

#### <a name="cons"></a>Nachteile

- Ist Android 4.1 oder höher erforderlich.
- Einige "HttpClient" Features/Optionen sind nicht verfügbar.

### <a name="managed-httpclienthandler"></a>Managed (HttpClientHandler)

Verwalteten Handler ist der vollständig verwaltete Handler für "HttpClient", der mit früheren Versionen von Xamarin.Android geliefert wurde.

#### <a name="pros"></a>Pros

- Es ist am kompatibel (Features) mit MS .NET und ältere Versionen von Xamarin.

#### <a name="cons"></a>Nachteile

- Es ist nicht vollständig mit dem Betriebssystem (z. b. integriert beschränkt auf TLS 1.0).
- Es ist in der Regel viel langsamer (z. b. Verschlüsselung) als systemeigene API.
- Sie erfordert mehr verwalteten Code, größere Anwendungen zu erstellen.



### <a name="choosing-a-handler"></a>Auswählen eines Handlers

Die Wahl zwischen `AndroidClientHandler` und `HttpClientHandler` richtet sich nach den Anforderungen Ihrer Anwendung. `AndroidClientHandler` wird für die Unterstützung der neuesten, z. B. empfohlen.

-   Sie benötigen, dass TLS 1.2 +-Unterstützung.
-   Ihre app auf die Android 4.1 (API 16) oder höher.
-   Sie benötigen die TLS 1.2 +-Unterstützung für `HttpClient`.
-   Sie müssen nicht TLS 1.2 +-Unterstützung für `WebClient`.

`HttpClientHandler` ist eine gute Wahl, wenn Sie TLS 1.2 + benötigen jedoch muss Unterstützung für Android-Versionen vor Android 4.1. Es ist auch eine gute Wahl, wenn Sie TLS 1.2 + benötigen Unterstützung für `WebClient`.

Ab Xamarin.Android 8.3 `HttpClientHandler` standardmäßig Boringssl (`btls`) als den zugrunde liegenden TLS-Anbieter. Der SSL-TLS langweilige-Anbieter bietet die folgenden Vorteile:

-   TLS 1.2 und höher unterstützt.
-   Alle Android-Versionen unterstützt.
-   Es bietet TLS 1.2 +-Unterstützung für beide `HttpClient` und `WebClient`.

Der Nachteil der Verwendung von Boringssl als die zugrunde liegende TLS-Anbieter ist, dass sie die Größe des resultierenden APK erhöhen kann (sie fügt zusätzliche APK-Größe pro unterstützte ABI von etwa 1MB).

Ab Xamarin.Android 8.3 ist die TLS-Standardanbieters Boringssl (`btls`). Wenn Sie nicht Boringssl verwenden möchten, können Sie wiederherstellen die historisch verwaltete SSL-Implementierung durch Festlegen der `$(AndroidTlsProvider)` Eigenschaft `legacy` (Weitere Informationen zum Festlegen von Build-Eigenschaften finden Sie unter [Build-Prozesses](~/android/deploy-test/building-apps/build-process.md)).


### <a name="programatically-using-androidclienthandler"></a>Programmgesteuert mithilfe von `AndroidClientHandler`

Die `Xamarin.Android.Net.AndroidClientHandler` ist ein `HttpMessageHandler` Implementierung, die speziell für Xamarin.Android.
Instanzen dieser Klasse verwendet die systemeigene `java.net.URLConnection` Implementierung für alle HTTP-Verbindungen. Dadurch wird eine Zunahme der HTTP-Leistung und kleinere APK theoretisch bereitgestellt.

Dieser Codeausschnitt ist ein Beispiel für eine einzelne Instanz explizit wie die `HttpClient` Klasse:

```csharp
// Android 4.1 or higher, Xamarin.Android 6.1 or higher
HttpClient client = new HttpClient(new Xamarin.Android.Net.AndroidClientHandler ());
```

> [!NOTE]
> Das zugrunde liegende Android-Gerät muss (d. h. TLS 1.2 unterstützen. Android 4.1 und höher)


## <a name="ssltls-implementation-build-option"></a>Buildoption SSL/TLS-Implementierung

Dieses Projektoption wird gesteuert, welche zugrunde liegenden TLS-Bibliothek von allen webanforderung verwendet werden, beide `HttpClient` und `WebRequest`. Standardmäßig wird die TLS 1.2 ausgewählt:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![TLS/SSL-Implementierung des Kombinationsfelds in Visual Studio](http-stack-images/tls06-vs.png)](http-stack-images/tls05-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![TLS/SSL-Implementierung des Kombinationsfelds in Visual Studio für Mac](http-stack-images/tls06-xs.png)](http-stack-images/tls05-xs.png#lightbox)

-----

Zum Beispiel:

```csharp
var client = new HttpClient();
```

Wenn die Implementierung von "HttpClient", um festgelegt wurde **verwaltet** und die TLS-Implementierung wurde festgelegt, um **natives TLS 1.2 +**, und klicken Sie dann die `client` Objekt würde automatisch verwenden Sie das verwaltete `HttpClientHandler` und TLS 1.2 (bereitgestellt von der Bibliothek BoringSSL) für die HTTP-Anforderungen.

Jedoch wenn die **HttpClient-Implementierung** nastaven NA hodnotu `AndroidHttpClient`, klicken Sie dann alle `HttpClient` verwenden Objekte die zugrunde liegenden Java-Klasse `java.net.URLConnection` und nicht betroffen von der **TLS/SSL-Implementierung** Wert. `WebRequest` Objekte, würde die BoringSSL-Bibliothek verwenden.

## <a name="other-ways-to-control-ssltls-configuration"></a>Weitere Informationen zum Steuern der SSL/TLS-Konfiguration

Es gibt drei Möglichkeiten, eine Xamarin.Android-Anwendung, die TLS-Einstellungen steuern kann:

1. Wählen Sie in den Projektoptionen HttpClient-Implementierung und standardmäßig TLS-Bibliothek.
2. Programmgesteuert mithilfe von `Xamarin.Android.Net.AndroidClientHandler`.
3. Deklarieren Sie Umgebungsvariablen (optional).

Die drei Optionen, wird empfohlen, um die Optionen für das Xamarin.Android-Projekt zu verwenden, um die Standardeinstellung deklarieren `HttpMessageHandler` und TLS für die gesamte app. Klicken Sie dann bei Bedarf programmgesteuert instanziieren `Xamarin.Android.Net.AndroidClientHandler` Objekte. Diese Optionen werden weiter oben beschrieben.

Die dritte Option &ndash; Verwenden von Umgebungsvariablen &ndash; nachfolgend beschrieben.

### <a name="declare-environment-variables"></a>Deklarieren von Umgebungsvariablen

Es gibt zwei Umgebungsvariablen, die für die Verwendung von TLS in Xamarin.Android verknüpft sind:

- `XA_HTTP_CLIENT_HANDLER_TYPE` &ndash; Diese Umgebungsvariable, deklariert die `HttpMessageHandler` , die die Anwendung verwendet wird. Zum Beispiel:

    ```csharp
    XA_HTTP_CLIENT_HANDLER_TYPE=Xamarin.Android.Net.AndroidClientHandler
    ```

- `XA_TLS_PROVIDER` &ndash; Diese Umgebungsvariable wird deklarieren, welcher TLS-Bibliothek verwendet werden soll, entweder `btls`, `legacy`, oder `default` (Dies ist identisch mit dieser Variablen auslassen):

    ```csharp
    XA_TLS_PROVIDER=btls
    ```

Diese Umgebungsvariable wird festgelegt, durch das Hinzufügen einer _Umgebungsdatei_ zum Projekt. Eine Umgebungsdatei ist eine Unix-Format nur-Text-Datei mit einer Buildaktion von **AndroidEnvironment**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Screenshot des Buildvorgangs AndroidEnvironment in Visual Studio.](http-stack-images/tls03-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Screenshot der AndroidEnvironment Buildvorgang in Visual Studio für Mac.](http-stack-images/tls03-xs.png)

-----

Informieren Sie sich die [Xamarin.Android-Umgebung](~/android/deploy-test/environment.md) Weitere Details zu den Umgebungsvariablen und Xamarin.Android-Handbuch.


## <a name="related-links"></a>Verwandte Links

- [Transport Layer Security (TLS)](~/cross-platform/app-fundamentals/transport-layer-security.md)
