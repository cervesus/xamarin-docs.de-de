---
title: Xamarin.Android-Umgebung
ms.prod: xamarin
ms.assetid: 67BFD4E1-276C-4B9F-9BD8-A5218D2BD529
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/15/2018
ms.openlocfilehash: 54fc52c2f2460726fe1c22149d4e7cc0e8a92609
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "73028069"
---
# <a name="xamarinandroid-environment"></a>Xamarin.Android-Umgebung

## <a name="execution-environment"></a>Ausführungsumgebung

Die *Ausführungsumgebung* ist die Menge an Umgebungsvariablen und Android-Systemeigenschaften, die die Programmausführung beeinflussen. Android-Systemeigenschaften können mit dem Befehl `adb shell setprop` festgelegt werden, während Umgebungsvariablen mit der Systemeigenschaft `debug.mono.env` festgelegt werden können:

```shell
## Enable GREF logging
adb shell setprop debug.mono.log gref

## Set the MONO_LOG_LEVEL and MONO_LOG_MASK environment variables
## so that additional Mono messages will be written to `adb logcat`.
adb shell setprop debug.mono.env "'MONO_LOG_LEVEL=info|MONO_LOG_MASK=asm'"
```

Android-Systemeigenschaften werden für alle Vorgänge auf dem Zielgerät festgelegt.

Ab Xamarin.Android 4.6 können sowohl Systemeigenschaften als auch Umgebungsvariablen anwendungsbasiert festgelegt und überschrieben werden, indem Sie dem Projekt eine *Umgebungsdatei* hinzufügen. Eine Umgebungsdatei ist eine mit UNIX formatierte Textdatei mit einem [**Buildvorgang** von `AndroidEnvironment`](~/android/deploy-test/building-apps/build-process.md).
Die Umgebungsdatei enthält Zeilen mit dem Format *key=value*.
Kommentare sind Zeilen, die mit `#` beginnen. Leere Zeilen werden ignoriert.

Wenn *key* mit einem Großbuchstaben beginnt, wird *key* als Umgebungsvariable behandelt und **setenv**(3) wird verwendet, um Umgebungsvariablen beim Prozessstart auf den angegebenen *Wert* festzulegen.

Wenn *key* mit einem Kleinbuchstaben beginnt, wird *key* als Android-Systemeigenschaft behandelt, und *value* ist der *Standardwert*: Android-Systemeigenschaften, die das Ausführungsverhalten von Xamarin.Android steuern, werden zuerst im Speicher für Android-Systemeigenschaften gesucht. Wenn kein Wert vorhanden ist, wird der Wert verwendet, der in der Umgebungsdatei angegeben ist. Dadurch ist es möglich, `adb shell setprop` zum Überschreiben von Werten zu Diagnosezwecken zu verwenden, die aus der Umgebungsdatei stammen.

## <a name="xamarinandroid-environment-variables"></a>Xamarin.Android-Umgebungsvariablen

Xamarin.Android unterstützt die `XA_HTTP_CLIENT_HANDLER_TYPE`-Variable, die entweder über `adb shell setprop debug.mono.env` oder den Buildvorgang `$(AndroidEnvironment)` festgelegt werden kann.

### `XA_HTTP_CLIENT_HANDLER_TYPE`

Der Typ mit Assemblyqualifikation, der vom [HttpMessageHandler](https://docs.microsoft.com/dotnet/api/system.net.http.httpmessagehandler?view=xamarinandroid-7.1) erben muss und mit dem Standardkonstruktor [`HttpClient()` erstellt wurde](https://docs.microsoft.com/dotnet/api/system.net.http.httpclient.-ctor?view=xamarinandroid-7.1#System_Net_Http_HttpClient__ctor).

In Xamarin.Android 6.1 ist diese Umgebungsvariable standardmäßig nicht festgelegt. [HttpClientHandler](https://docs.microsoft.com/dotnet/api/system.net.http.httpclienthandler?view=xamarinandroid-7.1) wird verwendet.

Alternativ kann der Wert `Xamarin.Android.Net.AndroidClientHandler` angegeben werden, um [`java.net.URLConnection`](xref:Java.Net.URLConnection)
für den Netzwerkzugriff zu verwenden, der *möglicherweise* die Verwendung von TLS 1.2 zulässt, wenn dies von Android unterstützt wird.

In Xamarin.Android 6.1 hinzugefügt.

## <a name="xamarinandroid-system-properties"></a>Xamarin.Android-Systemeigenschaften

Xamarin.Android unterstützt die folgenden Systemeigenschaften, die entweder über `adb shell setprop` oder den Buildvorgang `$(AndroidEnvironment)` festgelegt werden können.

- `debug.mono.debug`
- `debug.mono.env`
- `debug.mono.gc`
- `debug.mono.log`
- `debug.mono.max_grefc`
- `debug.mono.profile`
- `debug.mono.runtime_args`
- `debug.mono.trace`
- `debug.mono.wref`
- `XA_HTTP_CLIENT_HANDLER_TYPE`

### `debug.mono.debug`

Der Wert der Systemeigenschaft `debug.mono.debug` ist eine ganze Zahl. Wenn er `1` ist, verhalten Sie sich als wäre der Prozess mit `mono --debug` gestartet worden.
Dadurch werden für gewöhnlich Datei- und Zeileninformationen in Stapelüberwachungen usw. angezeigt, ohne dass die App von einem Debugger gestartet werden muss.

### `debug.mono.env`

Enthält eine durch `|` getrennte Liste von Umgebungsvariablen.

### `debug.mono.gc`

Der Wert der Systemeigenschaft `debug.mono.debug` ist eine ganze Zahl.
Wenn der Wert `1` ist, sollten GC-Informationen erfasst werden.

Die ist genauso, als würde die Systemeigenschaft `debug.mono.log``gc` enthalten.

### `debug.mono.log`

Steuert, welche Zusatzinformationen Xamarin.Android in `adb logcat` erfasst.
Dabei handelt es sich um eine Zeichenfolge mit Trennzeichen (`,`), die einen der folgenden Werte enthält:

- `all`: *Alle* Meldungen werden ausgegeben. Das ist nur selten sinnvoll, da es auch `lref`-Meldungen einschließt.
- `assembly`: `.apk` und Assemblyanalysemeldungen werden ausgegeben.
- `gc`: Meldungen, die mit GC in Verbindung stehen, werden ausgegeben.
- `gref`: Meldungen zu globalen JNI-Verweisen werden ausgegeben.
- `lref`: Meldungen zu lokalen JNI-Verweisen werden ausgegeben.
  > [!NOTE]
  > Dadurch wird eine *große Anzahl von Einträgen* in `adb logcat` erstellt.
  > In Xamarin.Android 5.1 wird dadurch auch eine `.__override__/lrefs.txt`-Datei erstellt, die *sehr groß* werden kann.
  > Vermeiden Sie dies.
- `timing`: Informationen zur zeitlichen Steuerung von Methoden werden ausgegeben. Dadurch werden auch die Dateien `.__override__/methods.txt` und `.__override__/counters.txt` erstellt.

### `debug.mono.max_grefc`

Der Wert der Systemeigenschaft `debug.mono.max_grefc` ist eine ganze Zahl.
Er *überschreibt* den erkannten maximalen Standard-GREF-Wert für das Zielgerät.

*Hinweis:* Diese Eigenschaft kann nur mit `adb shell setprop
debug.mono.max_grefc` verwendet werden, da der Wert nicht gleichzeitig mit der Datei **environment.txt** verfügbar ist.

### `debug.mono.profile`

Die Systemeigenschaft `debug.mono.profile` aktiviert den Profiler.
Sie entspricht der Option `mono --profile` und verwendet die gleichen Werte. (Weitere Informationen finden Sie auf der [**mono**(1)](http://docs.go-mono.com/?link=man%3amono(1))-Manpage.)

### `debug.mono.runtime_args`

Die Systemeigenschaft `debug.mono.runtime_args` enthält zusätzliche Optionen, die von **mono** analysiert werden müssen.

### `debug.mono.trace`

Die Systemeigenschaft `debug.mono.trace` aktiviert die Ablaufverfolgung.
Sie entspricht der Option `mono --trace` und verwendet die gleichen Werte. (Weitere Informationen finden Sie auf der [**mono**(1)](http://docs.go-mono.com/?link=man%3amono(1))-Manpage.)

*Verwenden Sie dies unter normalen Umständen nicht.* Die Ablaufverfolgung füllt die `adb logcat`-Ausgabe sehr schnell auf, sodass das Programm deutlich verlangsamt und das Programmverhalten verändert wird (bis hin zu zusätzlichen Fehlerbedingungen).

*Manchmal* können dadurch jedoch zusätzliche Untersuchungen durchgeführt werden...

### `debug.mono.wref`

Die Systemeigenschaft `debug.mono.wref` ermöglicht das Überschreiben des erkannten schwachen JIN-Verweismechanismus (Weak Reference). Es gibt zwei unterstützte Werte:

- `jni`: Schwache JNI-Verweise werden verwendet. Diese werden von `JNIEnv::NewWeakGlobalRef()` erstellt und von `JNIEnv::DeleteWeakGlobalREf()` gelöscht.
- `java`: Globale JNI-Verweise werden verwendet. Diese verweisen auf `java.lang.WeakReference`-Instanzen.

`java` wird standardmäßig von API-7 bis API-19 (Kit Kat) mit aktiviertem ART verwendet. (API-8 hat `jni`-Verweise hinzugefügt, und ART hat `jni`-Verweise *unterbrochen*.)

Diese Systemeigenschaft ist zum Testen und Untersuchen nützlich.
*Unter normalen Umständen* sollte sie nicht geändert werden.

### <a name="xa_http_client_handler_type"></a>XA\_HTTP\_CLIENT\_HANDLER\_TYPE

Diese Umgebungsvariable, die zuerst in Xamarin.Android 6.1 eingeführt wurde, deklariert die `HttpMessageHandler`-Standardimplementierung, die vom `HttpClient` verwendet wird. Standardmäßig ist diese Variable nicht festgelegt, und Xamarin.Android verwendet den `HttpClientHandler`.

```shell
XA_HTTP_CLIENT_HANDLER_TYPE=Xamarin.Android.Net.AndroidClientHandler
```

> [!NOTE]
> Das zugrunde liegende Android-Gerät muss TLS 1.2 unterstützen.
Android 5.0 und höher unterstützt TLS 1.2.

## <a name="example"></a>Beispiel

```shell
## Comments are lines which start with '#'
## Blank lines are ignored.

## Enable GREF messages to `adb logcat`
debug.mono.log=gref

## Clear out a Mono environment variable to decrease logging
MONO_LOG_LEVEL=
```

## <a name="related-links"></a>Verwandte Links

- [Transport Layer Security](~/cross-platform/app-fundamentals/transport-layer-security.md)
