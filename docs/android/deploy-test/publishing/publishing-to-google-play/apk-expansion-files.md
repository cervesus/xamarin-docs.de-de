---
title: APK-Erweiterungsdateien
ms.prod: xamarin
ms.assetid: DB7E38E8-3C4E-5191-27EA-22DE63044FE2
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 712322435614966348fc5c10cabf724870c307e4
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021292"
---
# <a name="apk-expansion-files"></a>APK-Erweiterungsdateien

Einige Anwendungen (z.B einige Spiele) benötigen weitere Ressourcen und Objekte als in der maximalen Größenbegrenzung der Android-App von Google Play bereitgestellt werden können. Dieser Grenzwert hängt von der Version von Android ab, für die Ihr APK ausgerichtet ist:

- 100 MB für APKs, die auf Android 4.0 oder höher (API-Ebene 14 oder höher) abzielen.
- 50 MB für APKs, die auf Android 3.2 oder früher (API-Ebene 13 oder höher) abzielen.

Um diese Einschränkung zu umgehen, wird Google Play zwei *Erweiterungsdateien* hosten und zusammen mit einem APK verteilen. Somit kann eine Anwendung dieses Limit indirekt überschreiten. 

Auf die meisten Geräte werden bei der Installation einer Anwendung Erweiterungsdateien und das APK heruntergeladen und auf den freigegebenen Speicherort (SD-Karte oder USB-fähige Partition) auf dem Gerät gespeichert werden. Auf einigen älteren Geräten können die Erweiterungsdateien nicht automatisch mit dem APK installiert werden. In diesen Situationen muss die Anwendung Code enthalten, der die Erweiterungsdateien herunterlädt, wenn der Benutzer die Anwendungen zuerst ausführt.

Erweiterungsdateien werden als *transparente binäre Blobs (Obb)* behandelt und können bis zu 2 GB groß sein. Android führt keine spezielle Verarbeitung dieser Dateien aus, nachdem sie heruntergeladen werden &ndash; diese Dateien können in einem beliebigen Format vorliegen, das für die Anwendung geeignet ist. Begrifflich ist die empfohlene Vorgehensweise für Erweiterungsdateien wie folgt:

- **Main-Erweiterung:** Diese Datei ist die primäre Erweiterungsdatei für Ressourcen und Objekte, die über dem Grenzwert der APK-Größe liegen. Die Main-Erweiterungsdatei sollte die wesentlichen Objekte enthalten, die eine Anwendung benötigt und selten aktualisiert werden.
- **Patch-Erweiterung:** Dies ist für kleine Updates in der Main-Erweiterungsdatei vorgesehen. Diese Datei kann aktualisiert werden. Die Anwendung ist verantwortlich für die Ausführung der erforderlichen Patches oder Updates aus dieser Datei.

Die Erweiterungsdateien müssen gleichzeitig mit dem APK hochgeladen werden.
Google Play verhindert, dass eine Erweiterungsdatei zu einem vorhandenen APK hochgeladen oder vorhandene APKs aktualisiert werden. Wenn es notwendig ist, eine Erweiterungsdatei zu aktualisieren, muss ein neues APK mit aktualisiertem `versionCode` hochgeladen werden.

## <a name="expansion-file-storage"></a>Dateispeicher für die Erweiterungsdateien

Wenn die Dateien auf ein Gerät heruntergeladen werden, werden sie unter **_freigegebener-speicher_/Android/obb/_paket-name_** gespeichert:

- **_freigegebener-speicher_** : Das durch `Android.OS.Environment.ExternalStorageDirectory` angegebene Verzeichnis.
- **_paket-name_** : Dies ist der Paketname der Anwendung im Java-Stil.

Nach dem Herunterladen sollten die Erweiterungsdateien nicht verschoben, geändert, umbenannt oder von ihrem Speicherort auf dem Gerät gelöscht werden. Ansonsten werden die Erweiterungsdateien erneut heruntergeladen, und die alte(n) Datei(en) gelöscht. Darüber hinaus sollte das Dateiverzeichnis für die Erweiterung nur das Paket der Erweiterungsdateien enthalten.

Erweiterungsdateien bieten keine Sicherheit bzw. keinen Schutz für Ihre Inhalte &ndash; andere Anwendungen oder Benutzer können auf Dateien zugreifen, die auf dem freigegebenen Speicher gespeichert sind.

Wenn es notwendig ist, eine Erweiterungsdatei zu entpacken, sollten die entpackten Dateien in ein separates Verzeichnis, z.B. in `Android.OS.Environment.ExternalStorageDirectory`, gespeichert werden.

Eine Alternative zum Extrahieren von Dateien aus einer Erweiterungsdatei ist das Lesen der Objekte oder Ressourcen direkt aus der Erweiterungsdatei. Die Erweiterungsdatei ist nicht mehr als eine ZIP-Datei, die mit einem entsprechenden `ContentProvider` verwendet werden kann. Die [Android.Play.ExpansionLibrary](https://github.com/mattleibow/Android.Play.ExpansionLibrary) enthält eine Assembly, [System.IO.Compression.Zip](https://github.com/mattleibow/Android.Play.ExpansionLibrary/tree/master/System.IO.Compression.Zip), die eine `ContentProvider` für direkten Zugriff auf einige Mediendateien ermöglicht. Wenn Mediendateien in eine ZIP-Datei gepackt werden, können Medienwiedergabeaufrufe Dateien im ZIP-Ordner direkt nutzen, ohne dass die ZIP-Datei entpackt werden muss. Die Mediendateien sollten nicht komprimiert werden, wenn sie der ZIP-Datei hinzugefügt werden. 

### <a name="filename-format"></a>FileName-Format

Wenn die Erweiterungsdateien heruntergeladen werden, wird Google Play das folgende Schema zur Benennung der Erweiterung verwenden:

```
[main|patch].<expansion-version>.<package-name>.obb
```

Die drei Komponenten dieses Schemas sind:

- `main` oder `patch`: Dies gibt an, ob es sich um die Main- oder die Patch-Erweiterungsdatei handelt . Es kann nur jeweils eine geben.
- `<expansion-version>`: Dies ist eine ganze Zahl, die dem `versionCode` des APK entspricht, dem die Datei zuerst zugeordnet wurde.
- `<package-name>`: Dies ist der Paketname der Anwendung im Java-Stil.

Wenn die APK-Version beispielsweise 21 und der Paketname `mono.samples.helloworld` lautet, wird die Haupterweiterungsdatei folgendermaßen benannt: **main.21.mono.samples.helloworld**.

## <a name="download-process"></a>Downloadvorgang

Wenn eine Anwendung über Google Play installiert wird, sollten die Erweiterungsdateien heruntergeladen und zusammen mit dem APK gespeichert werden. In bestimmten Situationen wird dies möglicherweise nicht der Fall sein, oder Erweiterungsdateien werden gelöscht. Um diese Bedingung behandeln zu können, muss eine Anwendung überprüfen, ob die Erweiterungsdateien vorhanden sind, und sie dann herunterladen, falls erforderlich. Das folgende Flussdiagramm zeigt den empfohlenen Workflow dieses Prozesses:

[![APK-Erweiterungsflussdiagramm](apk-expansion-files-images/apkexpansion.png)](apk-expansion-files-images/apkexpansion.png#lightbox)

Wenn eine Anwendung gestartet wird, sollte überprüft werden, ob die entsprechenden Erweiterungsdateien auf dem aktuellen Gerät vorhanden sind. Wenn dies nicht der Fall ist, muss die Anwendung eine Anforderung der [Anwendungslizenzierung](https://developer.android.com/google/play/licensing/index.html) von Google Play stellen. Diese Überprüfung erfolgt mithilfe der *License Verification Library (LVL)* , und sie muss für kostenlose und lizenzierte Anwendungen erfolgen. Die LVL wird hauptsächlich von kostenpflichtigen Anwendungen verwendet, um Lizenzbeschränkungen zu erzwingen. Google hat die LVL jedoch erweitert, sodass sie ebenfalls mit den Erweiterungsbibliotheken verwendet werden kann. Kostenlose Anwendungen müssen die LVL-Überprüfung ausführen, können aber die Lizenzeinschränkungen ignorieren. Die Anforderung der LVL ist verantwortlich für die Bereitstellung der folgenden Informationen zu den Erweiterungsdateien, die die Anwendung erfordert: 

- **Dateigröße**: Die Größe der Erweiterungsdateien dienen als Teil der Überprüfung, die bestimmt, ob die richtigen Erweiterungsdateien bereits heruntergeladen wurden oder nicht.
- **Dateinamen**: Dies ist der Dateiname (auf dem aktuellen Gerät), in dem die Erweiterungspakete gespeichert werden müssen.
- **URL für den Download**: Die URL, die zum Herunterladen der Erweiterungspakete verwendet werden soll. Diese ist für jeden Download eindeutig und läuft kurz nach der Bereitstellung ab.

Nachdem die Überprüfung der LVL durchgeführt wurde, sollte die Anwendung die Erweiterungsdateien herunterladen, und die folgenden Punkte im Rahmen des Downloads berücksichtigen:

- Das Gerät weist möglicherweise nicht genügend Speicherplatz zum Speichern der Erweiterungsdateien auf.
- Wenn kein WLAN verfügbar ist, sollte der Benutzer den Download anhalten oder beenden dürfen, um zu verhindern, dass unerwünschte Datengebühren anfallen.
- Die Erweiterungsdateien werden im Hintergrund heruntergeladen, um zu vermeiden, dass Benutzerinteraktionen blockiert werden.
- Während der Download im Hintergrund ausgeführt wird, soll eine Statusanzeige angezeigt werden.
- Fehler, die während des Downloads auftreten, werden ordnungsgemäß behandelt und rückgängig gemacht werden.

## <a name="architectural-overview"></a>Architekturübersicht

Wenn die Hauptaktivität gestartet wird, wird geprüft, ob die Erweiterungsdateien heruntergeladen werden. Wenn die Dateien heruntergeladen werden, müssen sie auf Gültigkeit überprüft werden.

Wenn die Erweiterungsdateien nicht heruntergeladen wurden oder die aktuellen Dateien ungültig sind, müssen neue Erweiterungsdateien heruntergeladen werden. Ein gebundener Dienst wird als Teil der Anwendung erstellt. Wenn die Hauptaktivität der Anwendung gestartet wird, verwendet sie den gebundenen Dienst, um eine Überprüfung für die Google-Lizenzierungsdienste durchzuführen, um die Namen der Erweiterungsdatei und die URL der Dateien zum Herunterladen zu ermitteln. Der gebundene Dienst lädt dann die Dateien in einem Hintergrundthread herunter.

Um den erforderlichen Aufwand für die Integration der Erweiterungsdateien in einer Anwendung zu erleichtern, erstellte Google mehrere Bibliotheken in Java. Die fraglichen Bibliotheken sind:

- **Downloader-Bibliothek**: Dies ist eine Bibliothek, die den erforderlichen Aufwand für die Integration der Erweiterungsdateien in einer Anwendung reduziert. Die Bibliothek lädt die Erweiterungsdateien in einem Hintergrunddienst herunter, zeigt Benutzerbenachrichtigungen an, behandelt Probleme mit der Netzwerkkonnektivität, setzt Downloads fort, usw.
- **License Verification Library (LVL)** : Eine Bibliothek zum Erstellen und Verarbeiten von Aufrufen an die Anwendungslizenzierungsdienste. Sie kann auch zur Ausführung von Lizenzierungsüberprüfungen verwendet werden, um festzustellen, ob die Anwendung für die Verwendung auf dem Gerät autorisiert ist.
- **APK-Erweiterungs-ZIP-Bibliothek (optional)** : Wenn sich die Erweiterungsdateien in einer ZIP-Datei befinden, fungiert diese Bibliothek als Inhaltsanbieter und lässt zu, dass eine Anwendung Ressourcen und Objekte direkt aus der ZIP-Datei liest, ohne die ZIP-Datei zu erweitern.

Diese Bibliotheken wurden in C# portiert und sind unter der Apache 2.0-Lizenz verfügbar. Um Erweiterungsdateien schnell in eine vorhandene Anwendung zu integrieren, können diese Bibliotheken zu einer vorhandenen Xamarin.Android-Anwendung hinzugefügt werden. Den Code finden Sie in der [Android.Play.ExpansionLibrary](https://github.com/mattleibow/Android.Play.ExpansionLibrary) auf GitHub.
