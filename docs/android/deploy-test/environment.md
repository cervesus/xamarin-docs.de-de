---
title: Xamarin.Android-Umgebung
ms.topic: article
ms.prod: xamarin
ms.assetid: 67BFD4E1-276C-4B9F-9BD8-A5218D2BD529
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: ee612d4a8982a6ae505b4d329b9abbc84624a1e0
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
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

Wenn *key* mit einem Kleinbuchstaben beginnt, wird *key* als Android-Systemeigenschaft behandelt, und der *Wert* ist der *Standardwert*: Android-Systemeigenschaften, die das Ausführungsverhalten von Xamarin.Android steuern, werden zuerst vom Android-Systemeigenschaftenserver gesucht, und wenn kein Wert vorhanden ist, wird der Wert verwendet, der in der Umgebungsdatei angegeben wurde. Dadurch ist es möglich, `adb shell setprop` zum Überschreiben von Werten zu Diagnosezwecken zu verwenden, die aus der Umgebungsdatei stammen.

## <a name="xamarinandroid-environment-variables"></a>Xamarin.Android-Umgebungsvariablen

Xamarin.Android unterstützt die `XA_HTTP_CLIENT_HANDLER_TYPE`-Variable, die entweder über `adb shell setprop debug.mono.env` oder den Buildvorgang `$(AndroidEnvironment)` festgelegt werden kann.


### `XA_HTTP_CLIENT_HANDLER_TYPE`

Der Typ mit Assemblyqualifikation, der vom [HttpMessageHandler](https://msdn.microsoft.com/en-us/library/system.net.http.httpmessagehandler(v=vs.118).aspx) erben muss und mit dem Standardkonstruktor [`HttpClient()` erstellt wurde](https://msdn.microsoft.com/en-us/library/hh138077(v=vs.118).aspx).

In Xamarin.Android 6.1 ist diese Umgebungsvariable standardmäßig nicht festgelegt. [HttpClientHandler](https://msdn.microsoft.com/en-us/library/system.net.http.httpclienthandler(v=vs.118).aspx) wird verwendet.

Alternativ kann der Wert `Xamarin.Android.Net.AndroidClientHandler` festgelegt werden, um [`java.net.URLConnection`](https://developer.xamarin.com/api/type/Java.Net.URLConnection/) für den Netzwerkzugriff zu verwenden, wodurch *möglicherweise* TLS 1.2 verwendet werden kann, wenn dies von Android unterstützt wird.

In Xamarin.Android 6.1 hinzugefügt

## <a name="xamarinandroid-system-properties"></a>Xamarin.Android-Systemeigenschaften

Xamarin.Android unterstützt die folgenden Systemeigenschaften, die entweder über `adb shell setprop` oder den Buildvorgang `$(AndroidEnvironment)` festgelegt werden können.

* `debug.mono.debug`
* `debug.mono.env`
* `debug.mono.gc`
* `debug.mono.log`
* `debug.mono.max_grefc`
* `debug.mono.profile`
* `debug.mono.runtime_args`
* `debug.mono.trace`
* `debug.mono.wref`
* `XA_HTTP_CLIENT_HANDLER_TYPE`

### `debug.mono.debug`

Der Wert der Systemeigenschaft `debug.mono.debug` ist eine ganze Zahl. Wenn er `1` ist, verhalten Sie sich als wäre der Prozess mit `mono --debug` gestartet worden.
Dadurch werden für gewöhnlich Datei- und Zeileninformationen in Stapelüberwachungen usw. angezeigt, ohne dass die App von einem Debugger gestartet werden muss.

### `debug.mono.env`

Enthält eine durch `|` getrennte Liste von Umgebungsvariablen.

### `debug.mono.gc`

Der Wert der Systemeigenschaft `debug.mono.debug` ist eine ganze Zahl.
Wenn der Wert `1` ist, sollten GC-Informationen erfasst werden.

Die ist genauso, als würde die Systemeigenschaft `debug.mono.log` `gc` enthalten.

### `debug.mono.log`

Steuert, welche Zusatzinformationen Xamarin.Android in `adb logcat` erfasst.
Dabei handelt es sich um eine Zeichenfolge mit Trennzeichen (`,`), die einen der folgenden Werte enthält:

* `all`: Ausgeben *aller* Meldungen. Das ist nur selten sinnvoll, da es auch `lref`-Meldungen einschließt.
* `assembly`: Ausgeben von `.apk` und Assemblyanalysemeldungen.
* `gc`: Ausgeben von Meldungen, die mit GC in Verbindung stehen.
* `gref`: Ausgeben von Meldungen zu globalen JNI-Verweisen.
* `lref`: Ausgeben von Meldungen zu lokalen JNI-Verweisen.  
    *Hinweis:* Dadurch wird *sehr viel* an `adb logcat` gesendet.  
    In Xamarin.Android 5.1 wird dadurch auch eine `.__override__/lrefs.txt`-Datei erstellt, die *sehr groß* werden kann.  
    Vermeiden Sie dies.
* `timing`: Ausgeben von Informationen zur zeitlichen Steuerung von Methoden. Dadurch werden auch die Dateien `.__override__/methods.txt` und `.__override__/counters.txt` erstellt.


### `debug.mono.max_grefc`

Der Wert der Systemeigenschaft `debug.mono.max_grefc` ist eine ganze Zahl.
Er *überschreibt* den erkannten maximalen Standard-GREF-Wert für das Zielgerät.

*Hinweis:* Dies kann nur mit `adb shell setprop
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

* `jni`: Verwenden von schwachen JIN-Verweisen, die von `JNIEnv::NewWeakGlobalRef()` erstellt und von `JNIEnv::DeleteWeakGlobalREf()` gelöscht werden.
* `java`: Verwenden von globalen JIN-Verweisen, die auf `java.lang.WeakReference`-Instanzen verweisen.

`java` wird standardmäßig von API-7 bis API-19 (Kit Kat) mit aktiviertem ART verwendet. (API-8 hat `jni`-Verweise hinzugefügt, und ART hat `jni`-Verweise *kaputt gemacht*.)

Diese Systemeigenschaft ist zum Testen und Untersuchen nützlich.
*Unter normalen Umständen* sollte sie nicht geändert werden.

### <a name="xahttpclienthandlertype"></a>XA\_HTTP\_CLIENT\_HANDLER\_TYPE

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
