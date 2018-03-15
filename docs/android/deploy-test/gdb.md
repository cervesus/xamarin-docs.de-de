---
title: GDB
ms.topic: article
ms.prod: xamarin
ms.assetid: CD0BE462-FA38-4881-B481-82AD05B3B8FE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: 55d72a49f90095a33577279d018e1696dda8fc42
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="gdb"></a>GDB

## <a name="overview"></a>Übersicht

Xamarin.Android 4.10 hat Teilunterstützung für das Verwenden von `gdb` durch das Verwenden des `_Gdb`-MSBuild-Ziels eingeführt. 

> [!NOTE]
> `gdb`-Unterstützung erfordert die Installation des Android NDK.

Es gibt drei Möglichkeiten zum Verwenden von `gdb`:

1.  [Debugbuilds mit aktiviertem Fast Deployment](#Debug_Builds_with_Fast_Deployment).
1.  [Debugbuilds mit deaktiviertem Fast Deployment](#Debug_Builds_without_Fast_Deployment).
1.  [Releasebuilds](#Release_Builds).


Sollten Probleme auftreten, sehen Sie sich den Abschnitt zur [Problembehandlung](#Troubleshooting) an.

<a name="Debug_Builds_with_Fast_Deployment" />

### <a name="debug-builds-with-fast-deployment"></a>Debugbuilds mit Fast Deployment

Wenn Sie einen Debugbuild mit aktiviertem Fast Deployment bereitstellen, kann `gdb` mithilfe des `_Gdb`-MSBuild-Ziels angehängt werden.

Installieren Sie zunächst die App. Dies können Sie über die IDE oder die Befehlszeile durchführen:

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:Install *.csproj
```

Führen Sie dann das `_Gdb`-Ziel aus. Am Ende der Ausführung wird eine `gdb`-Befehlszeile ausgegeben:

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:_Gdb *.csproj
...
    Target _Gdb:
        "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
...
```

Das `_Gdb`-Ziel startet eine beliebige Startprogrammaktivität, die in Ihrer `AndroidManifest.xml`-Datei deklariert wird. Um explizit anzugeben, welche Aktivität ausgeführt werden soll, verwenden Sie die `RunActivity`-MSBuild-Eigenschaft. Das Starten von Diensten und anderen Android-Konstrukten wird momentan nicht unterstützt.

Das `_Gdb`-Ziel erstellt ein `gdb-symbols`-Verzeichnis und kopiert die Inhalte der Verzeichnisse `/system/lib` und `$APPDIR/lib` Ihres Ziels dorthin.


> [!NOTE]
> Die Inhalte des `gdb-symbols`-Verzeichnisses sind mit dem Android-Ziel verknüpft, für das Sie die Bereitstellung durchgeführt haben, und werden nicht automatisch ersetzt, wenn Sie das Ziel ändern. (Dies ist ein Fehler.) Wenn Sie Android-Zielgeräte ändern, müssen Sie dieses Verzeichnis manuell löschen.

Kopieren Sie zum Schluss den generierten `gdb`-Befehl, und führen Sie ihn in Ihrer Shell aus:

```bash
$ "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
...
(gdb) bt
#0  0x40082e84 in nanosleep () from /Users/jon/Development/Projects/Scratch.HelloXamarin20/gdb-symbols/libc.so
#1  0x4008ffe6 in sleep () from /Users/jon/Development/Projects/Scratch.HelloXamarin20/gdb-symbols/libc.so
#2  0x74e46240 in ?? ()
#3  0x74e46240 in ?? ()
(gdb) c
```

<a name="Debug_Builds_without_Fast_Deployment" />

## <a name="debug-builds-without-fast-deployment"></a>Debugbuilds ohne Fast Deployment

Debugbuilds *mit* Fast Deployment funktionieren durch das Kopieren des `gdbserver`-Programms des Android NDK in das `.__override__`-Verzeichnis von Fast Deployment. Wenn Fast Deployment deaktiviert ist, darf dieses Verzeichnis nicht vorhanden sein.

Es gibt zwei Wege, das Problem zu umgehen:

-   Legen Sie die Systemeigenschaft `debug.mono.log` so fest, dass das `.__override__`-Verzeichnis erstellt wird.
-   Beziehen Sie `gdbserver` in `.apk` ein.

### <a name="setting-the-debugmonolog-system-property"></a>Festlegen der Systemeigenschaft `debug.mono.log`

Verwenden Sie den `adb`-Befehl, um die Systemeigenschaft `debug.mono.log` festzulegen:

```bash
$ adb shell setprop debug.mono.log gc
```

Sobald die Systemeigenschaft festgelegt wurde, führen Sie das `_Gdb`-Ziel und den ausgegebenen `gdb`-Befehl aus, genauso wie mit den Debugbuilds mit der Fast Deployment-Konfiguration:

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:_Gdb *.csproj
  ...
    Target _Gdb:
        "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
  ...
$ "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
...
(gdb) c
```


### <a name="including-gdbserver-in-your-app"></a>Beziehen Sie `gdbserver` in Ihre App ein

So beziehen Sie `gdbserver` in Ihre App ein:

1. Suchen Sie `gdbserver` im Android NDK (es sollte sich unter **$ANDROID\_NDK\_PATH/prebuilt/android-arm/gdbserver/gdbserver** befinden), und kopieren Sie es in Ihr Projektverzeichnis.

2. Benennen Sie `gdbserver` in **ibs/armeabi-v7a/libgdbserver.so** um.

3. Fügen Sie Ihrem Projekt **libs/armeabi-v7a/libgdbserver.so** mit einer **Buildaktion** von `AndroidNativeLibrary` hinzu.

4. Erstellen Sie die Anwendung erneut, und installieren Sie sie erneut.

Sobald die App erneut installiert wurde, führen Sie das `_Gdb`-Ziel und den ausgegebenen `gdb`-Befehl aus, genauso wie mit den Debugbuilds mit der Fast Deployment-Konfiguration:

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:_Gdb *.csproj
  ...
    Target _Gdb:
        "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
  ...
$ "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
...
(gdb) c
```

<a name="Release_Builds" />

## <a name="release-builds"></a>Releasebuilds

`gdb`-Unterstützung erfordert dreierlei:

1.  Die Berechtigung `INTERNET`.
2.  Aktiviertes App-Debugging.
3.  `gdbserver` muss zugänglich sein.

Die Berechtigung `INTERNET` ist standardmäßig in Debug-Apps aktiviert. Wenn Sie noch nicht in Ihrer App vorhanden ist, können Sie sie entweder hinzufügen, indem Sie **Properties/AndroidManifest.xml** oder die [Projekteigenschaften](https://developer.xamarin.com/recipes/android/general/projects/add_permissions_to_android_manifest/) bearbeiten.

Das App-Debuggen kann durch das Festlegen der benutzerdefinierten Attributeigenschaft [ApplicationAttribute.Debugging](https://developer.xamarin.com/api/property/Android.App.ApplicationAttribute.Debuggable/) auf `true` oder durch das Bearbeiten von **Properties/AndroidManifest.xml** und Festlegen des Attributs `//application/@android:debuggable` auf `true` aktiviert werden:

```xml
<application android:label="Example.Name.Here" android:debuggable="true">
```

Wie Sie ein zugängliches `gdbserver` gewährleisten, wird im Abschnitt [Debugbuilds ohne Fast Deployment](#Debug_Builds_without_Fast_Deployment) beschrieben.

Ein Nachteil: Das `_Gdb`-MSBuild-Ziel bricht alle vorher ausgeführten App-Instanzen ab. Dies funktioniert nicht auf Zielen vor Android V4.0.

<a name="Troubleshooting" />

## <a name="troubleshooting"></a>Problembehandlung

### <a name="monopmip-doesnt-work"></a>`mono_pmip` funktioniert nicht

Die Funktion `mono_pmip` (nützlich zum [Abrufen verwalteter Stackframes](http://www.mono-project.com/Debugging#Debugging_with_GDB)) wird von `libmonosgen-2.0.so` exportiert, was das `_Gdb`-Ziel momentan noch nicht lädt. (Dies wird in einem kommenden Release behoben.)

Um das Abrufen von Funktionen in `libmonosgen-2.0.so` zu ermöglichen, kopieren Sie es vom Zielgerät in das `gdb-symbols`-Verzeichnis:

```bash
$ adb pull /data/data/Mono.Android.DebugRuntime/lib/libmonosgen-2.0.so Project/gdb-symbols
```

Starten Sie dann die Debugsitzung neu.

### <a name="bus-error-10-when-running-the-gdb-command"></a>Busfehler: 10 beim Ausführen des Befehls `gdb`

Wenn beim Befehl `gdb` der Fehler `"Bus error: 10"` auftritt, starten Sie das Android-Gerät neu.

```bash
$ "/path/to/arm-linux-androideabi-gdb" -x "Project/gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
Copyright (C) 2011 Free Software Foundation, Inc.
...
Bus error: 10
$
```

### <a name="no-stack-trace-after-attach"></a>Keine Stapelüberwachung nach dem Anfügen

```bash
$ "/path/to/arm-linux-androideabi-gdb" -x "Project/gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
Copyright (C) 2011 Free Software Foundation, Inc.
...
(gdb) bt
No stack.
```

Dies ist häufig ein Zeichen dafür, dass die Inhalte des `gdb-symbols`-Verzeichnisses nicht mit Ihrem Android-Ziel synchronisiert sind. (Haben Sie das Android-Ziel verändert?)

Löschen Sie das `gdb-symbols`-Verzeichnis, und versuchen Sie es erneut.
