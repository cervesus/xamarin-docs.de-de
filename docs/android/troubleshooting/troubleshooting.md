---
title: Hinweise zur Fehlerbehebung
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 56137ACA-4811-B312-6860-E16D0FA123F7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/15/2018
ms.openlocfilehash: 6d83afa47c459633506736b2497a82c444352c90
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "79303516"
---
# <a name="troubleshooting-tips"></a>Hinweise zur Fehlerbehebung

## <a name="getting-diagnostic-information"></a>Abrufen von Diagnoseinformationen

Xamarin.Android gibt es mehrere Optionen zum Nachverfolgen verschiedener Fehler.
Dazu gehören:

1. Diagnoseausgabe von MSBuild
2. Gerätebereitstellungsprotokolle
3. Android-Debugprotokollausgabe

<a name="Diagnostic_MSBuild_Output" />

## <a name="diagnostic-msbuild-output"></a>Diagnoseausgabe von MSBuild

Die MSBuild-Diagnose kann zusätzliche Informationen im Zusammenhang mit der Erstellung und Bereitstellung von Paketen enthalten.

So aktivieren Sie die Diagnoseausgabe von MSBuild in Visual Studio

1. Klicken Sie auf **Extras > Optionen...** .
2. Wählen Sie in der linken Strukturansicht **Projekte und Projektmappen > Erstellen und Ausführen** aus.
3. Legen Sie im rechten Bereich das Dropdownelement „Ausführlichkeit der MSBuild-Buildausgabe“ auf „Diagnose“ fest.
4. Klicken Sie auf **OK**.
5. Bereinigen Sie Ihr Paket, und erstellen Sie es erneut.
6. Die Diagnoseausgabe wird im Ausgabebereich angezeigt.

So aktivieren Sie die Diagnoseausgabe von MSBuild in Visual Studio für Mac/OS X

1. Klicken Sie auf **Visual Studio für Mac > Einstellungen...**
2. Wählen Sie in der linken Strukturansicht **Projekte > Erstellen** aus.
3. Legen Sie im rechten Bereich das Dropdownelement „Protokollausführlichkeit“ auf „Diagnose“ fest.
4. Klicken Sie auf **OK**.
5. Starten Sie Visual Studio für Mac neu.
6. Bereinigen Sie Ihr Paket, und erstellen Sie es erneut.
7. Die Diagnoseausgabe wird im Bereich „Fehler“ angezeigt (**Ansicht > Bereiche > Fehler**), wenn Sie auf die Schaltfläche „Buildausgabe“ klicken.

## <a name="device-deployment-logs"></a>Gerätebereitstellungsprotokolle

So aktivieren Sie die Protokollierung der Gerätebereitstellung in Visual Studio:

1. **Extras > Optionen...** >
2. Wählen Sie in der linken Strukturansicht **Xamarin > Android-Einstellungen** aus.
3. Aktivieren Sie im rechten Bereich über das Kontrollkästchen [X] die **Erweiterung für die Debugprotokollierung (speichert „monodroid.log“ auf dem Desktop)** .
4. Protokollmeldungen werden in die Datei „monodroid.log“ auf dem Desktop geschrieben.

Von Visual Studio für Mac werden immer Gerätebereitstellungsprotokolle geschrieben. Die Suche ist etwas schwieriger. Eine *AndroidUtils*-Protokolldatei wird für jeden Tag und jede Uhrzeit erstellt, an dem eine Bereitstellung erfolgt: **AndroidTools-2012-10-24_12-35-45.log**.

- Unter Windows werden Protokolldateien in `%LOCALAPPDATA%\XamarinStudio-{VERSION}\Logs` geschrieben.
- Unter OS X werden Protokolldateien in `$HOME/Library/Logs/XamarinStudio-{VERSION}` geschrieben.

## <a name="android-debug-log-output"></a>Android-Debugprotokollausgabe

Android schreibt viele Nachrichten in das [Android-Debugprotokoll](~/android/deploy-test/debugging/android-debug-log.md).
Xamarin.Android verwendet Android-Systemeigenschaften, um die Generierung zusätzlicher Nachrichten im Android-Debugprotokoll zu steuern. Android-Systemeigenschaften können über den *setprop*-Befehl innerhalb der [Android Debug Bridge (ADB)](https://developer.android.com/guide/developing/tools/adb.html) festgelegt werden:

```shell
adb shell setprop PROPERTY_NAME PROPERTY_VALUE
```

Systemeigenschaften werden während des Prozessstarts gelesen und müssen daher entweder vor dem Starten der Anwendung festgelegt werden, oder die Anwendung muss neu gestartet werden, nachdem die Systemeigenschaften geändert wurden.

### <a name="xamarinandroid-system-properties"></a>Xamarin.Android-Systemeigenschaften

Xamarin.Android unterstützt die folgenden Systemeigenschaften:

- *debug.mono.debug*: Bei einer nicht leeren Zeichenfolge entspricht dies `*mono-debug*`.

- *debug.mono.env*: Eine durch Pipes ( *|* ) getrennte Liste von Umgebungsvariablen, die beim Starten der Anwendung exportiert werden sollen, *bevor* Mono initialisiert wurde. Dies ermöglicht das Festlegen von Umgebungsvariablen, die die Mono-Protokollierung steuern.

  > [!NOTE]
  > Da der Wert durch *|* getrennt ist, muss der Wert eine zusätzliche Ebene von Anführungszeichen aufweisen, da der \`*ADB-Shell*\`-Befehl einen Anführungszeichensatz entfernt.

  > [!NOTE]
  > Android-Systemeigenschaftswerte dürfen nicht länger als 92 Zeichen sein.

  Beispiel:

  ```
  adb shell setprop debug.mono.env "'MONO_LOG_LEVEL=info|MONO_LOG_MASK=asm'"
  ```

- *debug.mono.log*: Eine durch Trennzeichen getrennte ( *,* ) Liste von Komponenten, die zusätzliche Nachrichten in das Android-Debugprotokoll ausgeben sollen. Standardmäßig ist nichts festgelegt. Die Komponenten umfassen:

  - *all*: Alle Nachrichten werden ausgegeben.
  - *gc*: Meldungen, die mit GC (Garbage Collection) in Verbindung stehen, werden ausgegeben.
  - *gref*: Nachrichten zu Zuordnungen und Zuordnungsaufhebungen von (schwachen, globalen) Verweisen werden ausgegeben.
  - *lref*: Nachrichten zu Zuordnungen und Zuordnungsaufhebungen von Verweisen werden ausgegeben.

  > [!NOTE]
  > Diese sind *äußerst* ausführlich. Aktivieren Sie diese Option nur, wenn es wirklich erforderlich ist.

- *debug.mono.trace*: Ermöglicht das Festlegen der Einstellung [mono --trace](http://docs.go-mono.com/?link=man%3amono(1))`=PROPERTY_VALUE`.

## <a name="deleting-bin-and-obj"></a>Löschen von `bin` und `obj`

Bei Xamarin.Android traten in der Vergangenheit u. a. folgende Probleme auf:

- Es tritt ein ungewöhnlicher Build- oder Laufzeitfehler auf.
- Sie führen `Clean`-, `Rebuild`- oder manuelle Löschvorgänge für `bin`- und `obj`-Verzeichnisse durch.
- Das Problem wird behoben.

Wir haben uns intensiv mit der Lösung solcher Probleme beschäftigt, da sich diese negativ auf die Produktivität von Entwicklern auswirken.

Gehen Sie wie folgt vor, wenn ein solches Problem auftritt:

1. Überlegen Sie: Was war die letzte Aktion, bevor Ihr Projekt in diesen Zustand gebracht wurde?
1. Speichern Sie das aktuelle Buildprotokoll. Versuchen Sie erneut, einen Build zu initiieren, und zeichnen Sie ein [Diagnosebuildprotokoll](#diagnostic-msbuild-output) auf.
1. Senden Sie einen [Fehlerbericht][bug].

Bevor Sie Ihre `bin`- und `obj`-Verzeichnisse löschen, zippen Sie sie, und speichern Sie sie zur späteren Diagnose, falls diese erforderlich ist. Sie müssen für Ihr Xamarin.Android-Anwendungsprojekt wahrscheinlich einen `Clean`-Vorgang durchführen, um mit der Arbeit fortfahren zu können.

[bug]: https://github.com/xamarin/xamarin-android/wiki/Submitting-Bugs,-Feature-Requests,-and-Pull-Requests

## <a name="xamarinandroid-cannot-resolve-systemvaluetuple"></a>Xamarin.Android cannot resolve System.ValueTuple (Auflösen von Systemwerttupel in Xamarin.Android nicht möglich.)

Dieser Fehler tritt bei einer Inkompatibilität mit Visual Studio auf.

- **Visual Studio 2017 Update 1** (Version 15.1 oder früher) ist nur mit **System.ValueTuple NuGet 4.3.0** (oder früher) kompatibel.

- **Visual Studio 2017 Update 2** (Version 15.2 oder früher) ist nur mit **System.ValueTuple NuGet 4.3.1** (oder früher) kompatibel.

Wählen Sie die Version von System.ValueTuple NuGet aus, die Ihrer Visual Studio 2017-Installation entspricht.

## <a name="gc-messages"></a>GC-Nachrichten

Die Nachrichten von GC-Komponenten können angezeigt werden, indem die debug.mono.log-Systemeigenschaft auf einen Wert festgelegt wird, der GC enthält.

GC-Nachrichten werden immer dann generiert, wenn die GC ausgeführt wird und Informationen dazu bereitgestellt werden, welche Workload die GC verarbeitet hat:

```shell
I/monodroid-gc(12331): GC cleanup summary: 81 objects tested - resurrecting 21.
```

Weitere GC-Informationen wie Zeitangaben können generiert werden, indem Sie die `MONO_LOG_LEVEL`-Umgebungsvariable auf `debug` festlegen.

```shell
adb shell setprop debug.mono.env MONO_LOG_LEVEL=debug
```

Dies führt zu (vielen) zusätzlichen Mono-Nachrichten, einschließlich der drei folgenden, die von Bedeutung sind:

```shell
D/Mono (15723): GC_BRIDGE num-objects 1 num_hash_entries 81226 sccs size 81223 init 0.00ms df1 285.36ms sort 38.56ms dfs2 50.04ms setup-cb 9.95ms free-data 106.54ms user-cb 20.12ms clenanup 0.05ms links 5523436/5523436/5523096/1 dfs passes 1104 6883/11046605
D/Mono (15723): GC_MINOR: (Nursery full) pause 2.01ms, total 287.45ms, bridge 225.60 promoted 0K major 325184K los 1816K
D/Mono ( 2073): GC_MAJOR: (user request) pause 2.17ms, total 2.47ms, bridge 28.77 major 576K/576K los 0K/16K
```

In der `GC_BRIDGE`-Nachricht gibt `num-objects` die Anzahl der Bridgeobjekte an, die für diesen Durchlauf berücksichtigt werden, und `num_hash_entries` gibt die Anzahl der Objekte an, die während dieses Aufrufs des Bridgecodes verarbeitet werden.

In den Nachrichten `GC_MINOR` und `GC_MAJOR` gibt `total` die Zeitspanne an, in der das Programm angehalten wird (es werden keine Threads ausgeführt), und `bridge` gibt die Zeitspanne an, die der Code für die Bridgeverarbeitung benötigt (bei der die Java-VM verarbeitet wird). Während der Bridgeverarbeitung wird das Programm *nicht* angehalten.

 *Im Allgemeinen gilt*: Je höher der Wert von `num_hash_entries` ist, desto länger dauert die `bridge`-Sammlung und desto größer ist der Zeitaufwand für die Sammlung insgesamt (`total`).

## <a name="global-reference-messages"></a>Nachrichten zu globalen Verweisen

Zum Aktivieren der GREF-Protokollierung (Global Reference) muss die *debug.mono.log*-Systemeigenschaft *gref* enthalten, z. B.:

```shell
adb shell setprop debug.mono.log gref
```

Xamarin.Android verwendet globale Android-Verweise, um Zuordnungen zwischen Java-Instanzen und den zugeordneten verwalteten Instanzen bereitzustellen, da beim Aufrufen einer Java-Methode eine Java-Instanz für Java bereitgestellt werden muss.

Android-Emulatoren lassen nur 2.000 globale Verweise gleichzeitig zu. Die Grenze für Hardware liegt mit 52.000 globalen Verweisen deutlich höher. Der niedrige Grenzwert kann sich bei der Ausführung von Anwendungen auf dem Emulator als problematisch erweisen. Zu wissen, *woher* die Instanz stammt, kann deshalb sehr nützlich sein.

> [!NOTE]
> Die Anzahl globaler Verweise bezieht sich nur auf die internen in Xamarin.Android und berücksichtigt keine globalen Verweise, die von anderen in den Prozess geladenen nativen Bibliotheken stammen. Verwenden Sie Anzahl der globalen Verweise als Richtwert.

```shell
I/monodroid-gref(12405): +g+ grefc 108 gwrefc 0 obj-handle 0x40517468/L -> new-handle 0x40517468/L from    at Java.Lang.Object.RegisterInstance(IJavaObject instance, IntPtr value, JniHandleOwnership transfer)
I/monodroid-gref(12405):    at Java.Lang.Object.SetHandle(IntPtr value, JniHandleOwnership transfer)
I/monodroid-gref(12405):    at Java.Lang.Object..ctor(IntPtr handle, JniHandleOwnership transfer)
I/monodroid-gref(12405):    at Java.Lang.Thread+RunnableImplementor..ctor(System.Action handler, Boolean removable)
I/monodroid-gref(12405):    at Java.Lang.Thread+RunnableImplementor..ctor(System.Action handler)
I/monodroid-gref(12405):    at Android.App.Activity.RunOnUiThread(System.Action action)
I/monodroid-gref(12405):    at Mono.Samples.Hello.HelloActivity.UseLotsOfMemory(Android.Widget.TextView textview)
I/monodroid-gref(12405):    at Mono.Samples.Hello.HelloActivity.<OnCreate>m__3(System.Object o)
I/monodroid-gref(12405): handle 0x40517468; key_handle 0x40517468: Java Type: `mono/java/lang/RunnableImplementor`; MCW type: `Java.Lang.Thread+RunnableImplementor`
I/monodroid-gref(12405): Disposing handle 0x40517468
I/monodroid-gref(12405): -g- grefc 107 gwrefc 0 handle 0x40517468/L from    at Java.Lang.Object.Dispose(System.Object instance, IntPtr handle, IntPtr key_handle, JObjectRefType handle_type)
I/monodroid-gref(12405):    at Java.Lang.Object.Dispose()
I/monodroid-gref(12405):    at Java.Lang.Thread+RunnableImplementor.Run()
I/monodroid-gref(12405):    at Java.Lang.IRunnableInvoker.n_Run(IntPtr jnienv, IntPtr native__this)
I/monodroid-gref(12405):    at System.Object.c200fe6f-ac33-441b-a3a0-47659e3f6750(IntPtr , IntPtr )
I/monodroid-gref(27679): +w+ grefc 1916 gwrefc 296 obj-handle 0x406b2b98/G -> new-handle 0xde68f4bf/W from take_weak_global_ref_jni
I/monodroid-gref(27679): -w- grefc 1915 gwrefc 294 handle 0xde691aaf/W from take_global_ref_jni
```

Es gibt vier Nachrichten, die von Bedeutung sind:

- Erstellte globale Verweise: Die Zeilen beginnen mit *+g+* und stellen eine Stapelüberwachung für den erstellten Codepfad bereit.
- Gelöschte globale Verweise: Die Zeilen beginnen mit *-g-* und stellen möglicherweise eine Stapelüberwachung für den erstellten Codepfad zur Freigabe des globalen Verweises bereit. Wenn bei der GC ein globaler Verweis gelöscht wird, wird keine Stapelüberwachung bereitgestellt.
- Erstellte schwache globale Verweise: Die Zeilen beginnen mit *+w+* .
- Gelöschte schwache globale Verweise: Die Zeilen beginnen mit *-w-* .

In allen Nachrichten gibt der *grefc*-Wert die Anzahl der von Xamarin.Android erstellten globalen Verweise an, und der *grefwc*-Wert steht für die Anzahl der schwachen globalen Verweise, die Xamarin.Android erstellt hat. Der *handle*- oder *obj-handle*-Wert ist der Wert des JNI-Handles, und das Zeichen nach „ */* “ weist auf den Typ des Handlewerts hin: */L* für lokale Verweise, */G* für globale Verweise und */W* für schwache globale Verweise.

Im Rahmen des GC-Prozesses werden globale Verweise (+g+) in schwache globale Verweise konvertiert (was zu „+w+“- and „-g-“-Verweisen führt), und eine Java-seitige GC wird gestartet. Anschließend wird der schwache globale Verweis überprüft, um festzustellen, ob er in der Sammlung miteinbezogen wurde. Wenn dieser immer noch aktiv ist, wird ein neuer globaler Verweis für den schwachen Verweis (+ g +,-w-) erstellt. Andernfalls wird der schwache Verweis gelöscht (-w).

## <a name="java-instance-is-created-and-wrapped-by-a-mcw"></a>Java instance is created and wrapped by a MCW (Java-Instanz wird von einem MCW erstellt und umschlossen.)

```shell
I/monodroid-gref(27679): +g+ grefc 2211 gwrefc 0 obj-handle 0x4066df10/L -> new-handle 0x4066df10/L from ...
I/monodroid-gref(27679): handle 0x4066df10; key_handle 0x4066df10: Java Type: `android/graphics/drawable/TransitionDrawable`; MCW type: `Android.Graphics.Drawables.TransitionDrawable`
```

## <a name="a-gc-is-being-performed"></a>A GC is being performed... (Eine GC wird ausgeführt...)

```shell
I/monodroid-gref(27679): +w+ grefc 1953 gwrefc 259 obj-handle 0x4066df10/G -> new-handle 0xde68f95f/W from take_weak_global_ref_jni
I/monodroid-gref(27679): -g- grefc 1952 gwrefc 259 handle 0x4066df10/G from take_weak_global_ref_jni
```

## <a name="object-is-still-alive-as-handle--null"></a>Object is still alive, as handle != null (Objekt ist noch aktiv, da Ziehpunkt = NULL.)
## <a name="wref-turned-back-into-a-gref"></a>wref turned back into a gref (Schwacher Verweis wieder in globalen Verweis umgewandelt.)

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x4066df10
I/monodroid-gref(27679): +g+ grefc 1930 gwrefc 39 obj-handle 0xde68f95f/W -> new-handle 0x4066df10/G from take_global_ref_jni
I/monodroid-gref(27679): -w- grefc 1930 gwrefc 38 handle 0xde68f95f/W from take_global_ref_jni
```

## <a name="object-is-dead-as-handle--null"></a>Object is dead, as handle == null (Objekt ist inaktiv, da Ziehpunkt = NULL.)
## <a name="wref-is-freed-no-new-gref-created"></a>wref is freed, no new gref created (Schwacher Verweis wurde freigegeben, es wurde kein neuer globaler Verweis erstellt.)

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x0
I/monodroid-gref(27679): -w- grefc 1914 gwrefc 296 handle 0xde68f95f/W from take_global_ref_jni
```

Hier gibt es einen problematischen Aspekt zu berücksichtigen: Auf Zielen, auf denen eine ältere Android-Version als 4.0 ausgeführt wird, ist der Wert des globalen Verweises mit der Adresse des Java-Objekts im Arbeitsspeicher der Android-Runtime identisch. (Das heißt, die GC ist ein nicht beweglicher, konservativer Collector und übergibt direkte Verweise an diese Objekte.) Folglich stimmt der Wert des resultierenden globalen Verweises nach einer „+g+“-, „+w+“-, „-g“-, „+g+“-, „-w-“-Sequenz mit dem ursprünglichen Wert überein. Dies erleichtert das Durchsuchen von Protokollen.

Android 4.0 verfügt jedoch über einen beweglichen Collector und übergibt keine direkten Verweise auf Android Runtimeobjekte für die VM mehr. Folglich ist der Wert des globalen Verweises nach einer„+g+“-, „+w+“-, „-g“-, „+g+“-, „-w-“-Sequenz *anders*. Wenn das Objekt mehrere GCs überdauert, gibt es mehrere Werte des globalen Verweises, wodurch schwieriger festzustellen ist, woher die Zuweisung der Instanz tatsächlich stammt.

### <a name="querying-programmatically"></a>Programmgesteuerte Abfragen

Durch Abfragen des `JniRuntime`-Objekts können Sie sowohl die GREF- als auch die WREF-Anzahl abfragen.

`Java.Interop.JniRuntime.CurrentRuntime.GlobalReferenceCount` –Anzahl der globalen Verweise

`Java.Interop.JniRuntime.CurrentRuntime.WeakGlobalReferenceCount` – Anzahl der schwachen globalen Verweise

## <a name="android-debug-logs"></a>Android-Debugprotokolle

Die [Android-Debugprotokolle](~/android/deploy-test/debugging/android-debug-log.md) können zusätzlichen Kontext in Bezug auf auftretende Laufzeitfehler bereitstellen.

## <a name="floating-point-performance-is-terrible"></a>Floating-Point performance is terrible! (Gleitkommaleistung ist mangelhaft.)

Alternativ: „My app runs 10x faster with the Debug build than with the Release build!“ (App wird mit dem Debugbuild zehnmal schneller ausgeführt als mit dem Releasebuild!)

Xamarin.Android unterstützt mehrere Geräte-ABIs: *armeabi*, *armeabi-v7a* und *x86*. Geräte-ABIs können unter **Projekteigenschaften > Anwendung > Unterstützte Architekturen** angegeben werden.

Bei Debugbuilds wird ein Android-Paket verwendet, das alle ABIs bereitstellt, wodurch die schnellste ABI für das Zielgerät verwendet wird.

Releasebuilds enthalten nur die ABIs, die auf der Registerkarte „Projekteigenschaften“ ausgewählt sind. Es können mehrere ABIs ausgewählt werden.

*armeabi* ist die Standard-ABI und bietet die umfassendste Geräteunterstützung. *armeabi* bietet jedoch keine Unterstützung für u. a. mehrere CPU-Geräte und Gleitkommahardware. Folglich sind Apps, die die armeabi-Releaseruntime verwenden, an einen einzelnen Kern gebunden und verwenden eine soft-float-Implementierung. Beides kann zu einer deutlich langsameren Leistung Ihrer App beitragen.

Wenn Ihre App eine angemessene Gleitkommaleistung (z. B. für Spiele) erfordert, sollten Sie die ABI *armeabi-v7a* aktivieren. Sie sollten in diesem Fall nur die *armeabi-v7a*-Runtime unterstützen. Das bedeutet jedoch, dass Ihre App auf älteren Geräten, die nur *armeabi* unterstützen, nicht ausgeführt werden kann.

## <a name="could-not-locate-android-sdk"></a>Android SDK wurde nicht gefunden

In Google sind zwei Downloads für das Android SDK für Windows verfügbar.
Wenn Sie das Installationsprogramm (EXE-Datei) auswählen, werden Registrierungsschlüssel geschrieben, die Xamarin.Android angeben, wo es installiert wurde. Wenn Sie die ZIP-Datei auswählen und sie selbst entpacken, weiß Xamarin.Android nicht, wo nach dem SDK gesucht werden soll. Sie können Xamarin.Android mitteilen, wo sich das SDK in Visual Studio befindet, indem Sie zu **Extras > Optionen > Xamarin > Android-Einstellungen** navigieren:

[![Speicherort des Android SDK unter Xamarin > Android-Einstellungen](troubleshooting-images/01.png)](troubleshooting-images/01.png#lightbox)

## <a name="ide-does-not-display-target-device"></a>IDE zeigt Zielgerät nicht an

Es kann vorkommen, dass das Gerät, auf dem Sie Ihre Anwendung bereitstellen möchten, im Dialogfeld „Gerät auswählen“ nicht aufgeführt wird. Das kann der Fall sein, wenn die Android-Debugbrücke nicht ausgeführt wird.

Navigieren Sie zum [ADB-Programm](~/android/deploy-test/debugging/android-debug-log.md), und gehen Sie dann wie folgt vor, um dieses Problem zu diagnostizieren:

```shell
adb devices
```

Wenn Ihr Gerät nicht vorhanden ist, starten Sie den Server für die Android-Debugbrücke neu, um nach dem Gerät zu suchen:

```shell
adb kill-server
adb start-server
```

Die HTC-Synchronisierungssoftware verhindert ggf. das ordnungsgemäße Ausführen von **adb start-server**. Wenn der Befehl **adb start-server** nicht ausgibt, von welchem Port aus der Server gestartet wird, beenden Sie die HTC-Synchronisierungssoftware, und versuchen Sie, den ADB-Server neu zu starten.

## <a name="the-specified-task-executable-keytool-could-not-be-run"></a>The specified task executable "keytool" could not be run (Die angegebene ausführbare Datei „keytool“ der Aufgabe konnte nicht ausgeführt werden.).

Das bedeutet, dass der angegebene Pfad nicht das Verzeichnis enthält, in dem sich das Java SDK-Verzeichnis „bin“ befindet. Überprüfen Sie, ob Sie diese Schritte im Handbuch [Installation](~/android/get-started/installation/index.md) befolgt haben.

## <a name="monodroidexe-or-aresgenexe-exited-with-code-1"></a>monodroid.exe or aresgen.exe exited with code 1 („monodroid.exe“ oder „aresgen.exe“ wurde mit dem Code 1 beendet.)

Navigieren Sie zu Visual Studio, und ändern Sie den Ausführlichkeitsgrad von MSBuild, um dieses Problem zu beheben. Gehen Sie hierzu wie folgt vor: Wählen Sie **Extras > Optionen > Projekt**, **Projektmappen > Erstellen** und **Ausführen > Ausführlichkeit der MSBuild-Projektbuildausgabe** aus, und legen Sie diesen Wert auf **Normal** fest.

Initiieren Sie den Build erneut, und überprüfen Sie den Ausgabebereich von Visual Studio, in dem der vollständige Fehler angezeigt wird.

## <a name="there-is-not-enough-storage-space-on-the-device-to-deploy-the-package"></a>There is not enough storage space on the device to deploy the package (Auf dem Gerät ist nicht genügend Speicherplatz für die Bereitstellung des Pakets vorhanden.)

Dieser Fehler tritt auf, wenn Sie den Emulator nicht über Visual Studio starten. Wenn Sie den Emulator außerhalb von Visual Studio starten, müssen Sie z. B. die `-partition-size 512`-Optionen übergeben.

```shell
emulator -partition-size 512 -avd MonoDroid
```

Stellen Sie sicher, dass Sie den richtigen Simulatornamen, d. h. [den Namen, den Sie beim Konfigurieren des Simulators](~/android/get-started/installation/windows.md#device) angegeben haben, verwenden.

## <a name="install_failed_invalid_apk-when-installing-a-package"></a>INSTALL\_FAILED\_INVALID\_APK when installing a package (Paket konnte wegen ungültigem Anwendungspaket nicht installiert werden.)

Android-Paketnamen *müssen* einen Punkt („ *.* “) enthalten. Fügen Sie dem Paketnamen einen Punkt hinzu.

- In Visual Studio:
  - Klicken Sie mit der rechten Maustaste auf das Projekt, und klicken Sie dann auf „Eigenschaften“.
  - Klicken Sie links auf die Registerkarte „Android-Manifest“.
  - Aktualisieren Sie das Feld mit dem Paketnamen.
    - Wenn die Meldung &ldquo;No AndroidManifest.xml found. &rdquo; („Keine AndroidManifest.xml-Datei gefunden. Klicken Sie, um eine hinzuzufügen.“) angezeigt wird, klicken Sie auf den Link, und aktualisieren Sie dann das Feld mit dem Paketnamen.
- In Visual Studio für Mac:
  - Klicken Sie mit der rechten Maustaste auf das Projekt, und klicken Sie dann auf „Optionen“.
  - Navigieren Sie zum Abschnitt „Build > Android Application“ (Erstellen > Android-Anwendung).
  - Fügen Sie dem Paketnamen einen Punkt („.“) hinzu.

## <a name="install_failed_missing_shared_library-when-installing-a-package"></a>INSTALL\_FAILED\_MISSING\_SHARED\_LIBRARY when installing a package (Paket konnte wegen fehlender freigegebener Bibliothek nicht installiert werden.)

„Freigegebene Bibliothek“ bezieht sich in diesem Kontext *nicht* auf eine native freigegebene Bibliotheksdatei (*libfoo.so*), sondern bezeichnet eine Bibliothek, die separat auf dem Zielgerät installiert werden muss, z. B. Google Maps.

Das Android-Paket gibt an, welche freigegebenen Bibliotheken für das `<uses-library/>`-Element erforderlich sind. Wenn eine *erforderliche* Bibliothek auf dem Zielgerät nicht vorhanden ist (`//uses-library/@android:required` ist z. B. auf *true* – den Standardwert – festgelegt), tritt beim Installieren des Pakets ein Fehler auf (*INSTALL\_FAILED\_MISSING\_SHARED\_LIBRARY*).

Sie können ermitteln, welche freigegebenen Bibliotheken erforderlich sind, indem Sie sich die *generierte*
**AndroidManifest.xml**-Datei (z. B. **obj\\Debug\\android\\AndroidManifest.xml**) ansehen und nach den`<uses-library/>`-Elementen suchen. Sie können Ihrem Projekt `<uses-library/>`-Elemente manuell in der **Eigenschaften\\AndroidManifest.xml**-Datei und über das [benutzerdefinierte UsesLibraryAttribute-Attribut](xref:Android.App.UsesLibraryAttribute) hinzufügen.

Wenn Sie z. B. einen Assemblyverweis auf *Mono.Android.GoogleMaps.dll* hinzufügen, wird der freigegebenen Google Maps-Bibliothek implizit eine `<uses-library/>` hinzugefügt.

## <a name="install_failed_update_incompatible-when-installing-a-package"></a>INSTALL\_FAILED\_UPDATE\_INCOMPATIBLE when installing a package (Fehler bei der Installation wegen inkompatiblem Update.)

Für Android-Pakete gelten drei Anforderungen:

- Sie müssen einen Punkt „.“ enthalten (siehe vorheriger Eintrag).
- Der Paketname muss eine eindeutige Zeichenfolge aufweisen (daher die Reverse-Benennungskonvention mit TLD für Android-App-Namen, z. B. com.android.chrome für die Chrome-App).
- Beim Aktualisieren von Paketen muss das Paket denselben Signaturschlüssel aufweisen.

Stellen Sie sich dazu das folgende Szenario vor:

1. Sie erstellen eine App und stellen diese als Debugging-App bereit.
2. Sie ändern den Signaturschlüssel, um ihn als Release-App zu verwenden (oder weil Sie den standardmäßig verwendeten Signaturschlüssel zum Debuggen nicht verwenden möchten).
3. Sie installieren die App, ohne sie im ersten Schritt zu entfernen, z. B. „Debuggen > Starten ohne Debuggen“ in Visual Studio.

In diesem Fall schlägt das Installieren des Pakets fehl, und der Fehler INSTALL\_FAILED\_UPDATE\_INCOMPATIBLE wird angezeigt, da der Paketname nicht geändert wurde, der Signaturschlüssel hingegen schon. Das [Android-Debugprotokoll](~/android/deploy-test/debugging/android-debug-log.md) enthält auch eine Meldung ähnlich der folgenden:

```shell
E/PackageManager(  146): Package [PackageName] signatures do not match the previously installed version; ignoring!
```

Entfernen Sie die Anwendung vollständig von Ihrem Gerät, bevor Sie sie erneut installieren, um diesen Fehler zu beheben.

## <a name="install_failed_uid_changed-when-installing-a-package"></a>INSTALL\_FAILED\_UID\_CHANGED when installing a package (Paket konnte wegen geänderter UID nicht installiert werden.)

Wenn ein Android-Paket installiert wird, wird ihm eine *Benutzer-ID* (UID) zugewiesen.
*In manchen Fällen* schlägt die Installation über eine bereits vorhandene Anwendung aus derzeit unbekannten Gründen fehl (`INSTALL_FAILED_UID_CHANGED`):

```shell
ERROR [2015-03-23 11:19:01Z]: ANDROID: Deployment failed
Mono.AndroidTools.InstallFailedException: Failure [INSTALL_FAILED_UID_CHANGED]
   at Mono.AndroidTools.Internal.AdbOutputParsing.CheckInstallSuccess(String output, String packageName)
   at Mono.AndroidTools.AndroidDevice.<>c__DisplayClass2c.<InstallPackage>b__2b(Task`1 t)
   at System.Threading.Tasks.ContinuationTaskFromResultTask`1.InnerInvoke()
   at System.Threading.Tasks.Task.Execute()
```

Sie können dieses Problem umgehen, indem Sie das Android-Paket vollständig *deinstallieren* und die App entweder über die GUI des Android-Ziels oder mithilfe der `adb` installieren:

```shell
$ adb uninstall @PACKAGE_NAME@
```

**Verwenden sie nicht** `adb uninstall -k`, da dadurch die Anwendungsdaten und damit die in Konflikt stehende UID auf dem Zielgerät *beibehalten* werden.

## <a name="release-apps-fail-to-launch-on-device"></a>Release apps fail to launch on device (Auf dem Gerät können keine Release-Apps gestartet werden.)

Wenn die Ausgabe des Android-Debugprotokolls eine Meldung ähnlich der folgenden enthält,

```shell
D/AndroidRuntime( 1710): Shutting down VM
W/dalvikvm( 1710): threadid=1: thread exiting with uncaught exception (group=0xb412f180)
E/AndroidRuntime( 1710): FATAL EXCEPTION: main
E/AndroidRuntime( 1710): java.lang.UnsatisfiedLinkError: Couldn't load monodroid: findLibrary returned null
E/AndroidRuntime( 1710):        at java.lang.Runtime.loadLibrary(Runtime.java:365)
```

gibt es dafür zwei mögliche Ursachen:

1. Die APK-Datei bietet keine ABI, die das Zielgerät unterstützt.
    Die APK-Datei enthält z. B. nur armeabi-v7a-Binärdateien, und das Zielgerät unterstützt nur armeabi.

2. Ein [Android-Fehler](https://code.google.com/p/android/issues/detail?id=21670). Deinstallieren Sie in diesem Fall die App, und installieren Sie sie erneut.

Bearbeiten Sie im ersten Fall die Projektoptionen/-eigenschaften, und [fügen Sie der ABI-Liste Unterstützung für die erforderliche ABI hinzu](~/android/app-fundamentals/cpu-architectures.md). Sie können ermitteln, welche ABI Sie hinzufügen müssen, indem Sie den folgenden adb-Befehl für das Zielgerät ausführen:

```shell
adb shell getprop ro.product.cpu.abi
adb shell getprop ro.product.cpu.abi2
```

Die Ausgabe enthält die primären (und optional die sekundären) ABIs.

```shell
$ adb shell getprop | grep ro.product.cpu
[ro.product.cpu.abi2]: [armeabi]
[ro.product.cpu.abi]: [armeabi-v7a]
```

## <a name="the-outpath-property-is-not-set-for-project-ldquomyappcsprojrdquo"></a>Die OutPath-Eigenschaft ist für Projekt &ldquo;MyApp.csproj&rdquo; nicht festgelegt.

Das bedeutet in der Regel, dass Sie einen HP-Computer verwenden und die Umgebungsvariable &ldquo;Plattform&rdquo; auf beispielsweise MCD oder HPD festgelegt wurde. Dies steht in Konflikt mit der MSBuild-Plattformeigenschaft, die im Allgemeinen auf &ldquo;Any CPU&rdquo; (Beliebige CPU) oder auf &ldquo;x86&rdquo; festgelegt ist. Entfernen Sie diese Umgebungsvariable von Ihrem Computer, um MSBuild ausführen zu können. Wählen Sie hierzu folgende Optionen aus:

- Systemsteuerung > System und Sicherheit > System > Erweiterte Systemeinstellungen > Umgebungsvariablen

Starten Sie Visual Studio neu, oder führen Sie Visual Studio für Mac erneut aus. Sie müssten jetzt mit Ihrer Arbeit fortfahren können.

## <a name="javalangclasscastexception-monoandroidruntimejavaobject-cannot-be-cast-to"></a>java.lang.ClassCastException: mono.android.runtime.JavaObject cannot be cast to... (java.lang.ClassCastException: mono.android.runtime.JavaObject kann nicht umgewandelt werden in...)

In Xamarin.Android 4.x werden geschachtelte, generische Typen nicht richtig gemarshallt. Beachten Sie z. B. den folgenden C\#-Code, für den [SimpleExpandableListAdapter](xref:Android.Widget.SimpleExpandableListAdapter) verwendet wird:

```csharp
// BAD CODE; DO NOT USE
var groupData = new List<IDictionary<string, object>> () {
        new Dictionary<string, object> {
                { "NAME", "Group 1" },
                { "IS_EVEN", "This group is odd" },
        },
};
var childData = new List<IList<IDictionary<string, object>>> () {
        new List<IDictionary<string, object>> {
                new Dictionary<string, object> {
                        { "NAME", "Child 1" },
                        { "IS_EVEN", "This group is odd" },
                },
        },
};
mAdapter = new SimpleExpandableListAdapter (
        this,
        groupData,
        Android.Resource.Layout.SimpleExpandableListItem1,
        new string[] { "NAME", "IS_EVEN" },
        new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 },
        childData,
        Android.Resource.Layout.SimpleExpandableListItem2,
        new string[] { "NAME", "IS_EVEN" },
        new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 }
);
```

Das Problem besteht darin, dass in Xamarin.Android geschachtelte, generische Typen nicht korrekt gemarshallt werden. `List<IDictionary<string, object>>` wird an eine [java.lang.ArrayList](xref:Java.Util.ArrayList) gemarshallt, die `ArrayList` enthält jedoch `mono.android.runtime.JavaObject`-Instanzen (die auf die `Dictionary<string, object>`-Instanzen verweisen), weshalb [java.util.Map](xref:Java.Util.IMap) nicht implementiert wird. Dies führt zu folgender Ausnahme:

```shell
E/AndroidRuntime( 2991): FATAL EXCEPTION: main
E/AndroidRuntime( 2991): java.lang.ClassCastException: mono.android.runtime.JavaObject cannot be cast to java.util.Map
E/AndroidRuntime( 2991):        at android.widget.SimpleExpandableListAdapter.getGroupView(SimpleExpandableListAdapter.java:278)
E/AndroidRuntime( 2991):        at android.widget.ExpandableListConnector.getView(ExpandableListConnector.java:446)
E/AndroidRuntime( 2991):        at android.widget.AbsListView.obtainView(AbsListView.java:2271)
E/AndroidRuntime( 2991):        at android.widget.ListView.makeAndAddView(ListView.java:1769)
E/AndroidRuntime( 2991):        at android.widget.ListView.fillDown(ListView.java:672)
E/AndroidRuntime( 2991):        at android.widget.ListView.fillFromTop(ListView.java:733)
E/AndroidRuntime( 2991):        at android.widget.ListView.layoutChildren(ListView.java:1622)
```

Die Problemumgehung besteht darin, die bereitgestellten [Java-Sammlungstypen](~/android/internals/api-design.md) anstelle von `System.Collections.Generic`-Typen für die &ldquo;inneren&rdquo; Typen zu verwenden. Dies führt beim Marshalling der Instanzen zu entsprechenden Java-Typen. (Der folgende Code ist komplizierter als notwendig, um die Lebensdauer der globalen Verweise zu verringern. Er kann vereinfacht werden, um den ursprünglichen Code über `s/List/JavaList/g` und `s/Dictionary/JavaDictionary/g` zu bearbeiten, wenn die Lebensdauer der globalen Verweise kein Problem darstellen.)

```csharp
// insert good code here
using (var groupData = new JavaList<IDictionary<string, object>> ()) {
    using (var groupEntry = new JavaDictionary<string, object> ()) {
        groupEntry.Add ("NAME", "Group 1");
        groupEntry.Add ("IS_EVEN", "This group is odd");
        groupData.Add (groupEntry);
    }
    using (var childData = new JavaList<IList<IDictionary<string, object>>> ()) {
        using (var childEntry = new JavaList<IDictionary<string, object>> ())
        using (var childEntryDict = new JavaDictionary<string, object> ()) {
            childEntryDict.Add ("NAME", "Child 1");
            childEntryDict.Add ("IS_EVEN", "This child is odd.");
            childEntry.Add (childEntryDict);
            childData.Add (childEntry);
        }
        mAdapter = new SimpleExpandableListAdapter (
            this,
            groupData,
            Android.Resource.Layout.SimpleExpandableListItem1,
            new string[] { "NAME", "IS_EVEN" },
            new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 },
            childData,
            Android.Resource.Layout.SimpleExpandableListItem2,
            new string[] { "NAME", "IS_EVEN" },
            new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 }
        );
    }
}
```

[Dieses Problem wird in einem kommenden Release behoben](https://bugzilla.xamarin.com/show_bug.cgi?id=5401).

## <a name="unexpected-nullreferenceexceptions"></a>Unerwartete NullReferenceExceptions

Gelegentlich werden im [Android-Debugprotokoll](~/android/deploy-test/debugging/android-debug-log.md) NullReferenceExceptions erwähnt, die &ldquo;nicht vorkommen dürfen&rdquo; oder aus Mono für Android-Runtimecode abgerufen werden, kurz bevor die App nicht mehr ausgeführt wird:

```shell
E/mono(15202): Unhandled Exception: System.NullReferenceException: Object reference not set to an instance of an object
E/mono(15202):   at Java.Lang.Object.GetObject (IntPtr handle, System.Type type, Boolean owned)
E/mono(15202):   at Java.Lang.Object._GetObject[IOnTouchListener] (IntPtr handle, Boolean owned)
E/mono(15202):   at Java.Lang.Object.GetObject[IOnTouchListener] (IntPtr handle, Boolean owned)
E/mono(15202):   at Android.Views.View+IOnTouchListenerAdapter.n_OnTouch_Landroid_view_View_Landroid_view_MotionEvent_(IntPtr jnienv, IntPtr native__this, IntPtr native_v, IntPtr native_e)
E/mono(15202):   at (wrapper dynamic-method) object:b039cbb0-15e9-4f47-87ce-442060701362 (intptr,intptr,intptr,intptr)
```

oder

```shell
E/mono    ( 4176): Unhandled Exception:
E/mono    ( 4176): System.NullReferenceException: Object reference not set to an instance of an object
E/mono    ( 4176): at Android.Runtime.JNIEnv.NewString (string)
E/mono    ( 4176): at Android.Util.Log.Info (string,string)
```

Dieser Fehler kann auftreten, wenn die Android Runtime den Prozess abbricht, was aus zahlreichen Gründen geschehen kann, z. B. wenn die maximale Anzahl an globalen Verweisen des Ziels erreicht wird oder wenn etwas mit der JNI &ldquo;schiefgegangen&rdquo; ist.

Sie können ermitteln, ob dies der Fall ist, indem Sie überprüfen, ob das Android-Debugprotokoll eine Meldung für Ihren Prozess enthält, die folgender ähnelt:

```shell
E/dalvikvm(  123): VM aborting
```

## <a name="abort-due-to-global-reference-exhaustion"></a>Abbruch aufgrund der Auslastung von globalen Verweisen

Die JNI-Ebene der Android Runtime unterstützt nur eine begrenzte Anzahl von JNI-Objektverweisen, die zu einem bestimmten Zeitpunkt gültig sind. Wenn dieser Grenzwert überschritten wird, tritt ein Fehler auf.

Der Grenzwert für GREF (*globale Verweise*) liegt bei 2.000 Verweisen im Emulator und ca. 52.000 Verweisen für Hardware.

Wenn im Android-Debugprotokoll Meldungen wie die folgende angezeigt werden, ist das ein Hinweis darauf, dass Sie gerade dabei sind, zu viele globale Verweise zu erstellen:

```shell
D/dalvikvm(  602): GREF has increased to 1801
```

Wenn Sie den Grenzwert für globale Verweise erreichen, wird eine Meldung wie die folgende ausgegeben:

```shell
D/dalvikvm(  602): GREF has increased to 2001
W/dalvikvm(  602): Last 10 entries in JNI global reference table:
W/dalvikvm(  602):  1991: 0x4057eff8 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1992: 0x4057f010 cls=Landroid/graphics/Point; (28 bytes)
W/dalvikvm(  602):  1993: 0x40698e70 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1994: 0x40698e88 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1995: 0x40698ea0 cls=Landroid/graphics/Point; (28 bytes)
W/dalvikvm(  602):  1996: 0x406981f0 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1997: 0x40698208 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1998: 0x40698220 cls=Landroid/graphics/Point; (28 bytes)
W/dalvikvm(  602):  1999: 0x406956a8 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  2000: 0x406956c0 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602): JNI global reference table summary (2001 entries):
W/dalvikvm(  602):    51 of Ljava/lang/Class; 164B (41 unique)
W/dalvikvm(  602):    46 of Ljava/lang/Class; 188B (17 unique)
W/dalvikvm(  602):     6 of Ljava/lang/Class; 212B (6 unique)
W/dalvikvm(  602):    11 of Ljava/lang/Class; 236B (7 unique)
W/dalvikvm(  602):     3 of Ljava/lang/Class; 260B (3 unique)
W/dalvikvm(  602):     4 of Ljava/lang/Class; 284B (2 unique)
W/dalvikvm(  602):     8 of Ljava/lang/Class; 308B (6 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 316B
W/dalvikvm(  602):     4 of Ljava/lang/Class; 332B (3 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 356B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 380B (1 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 428B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 452B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 476B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 500B (1 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 548B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 572B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 596B (2 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 692B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 956B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 1004B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 1148B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 1172B (1 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 1316B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 3428B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 3452B
W/dalvikvm(  602):     1 of Ljava/lang/String; 28B
W/dalvikvm(  602):     2 of Ldalvik/system/VMRuntime; 12B (1 unique)
W/dalvikvm(  602):    10 of Ljava/lang/ref/WeakReference; 28B (10 unique)
W/dalvikvm(  602):     1 of Ldalvik/system/PathClassLoader; 44B
W/dalvikvm(  602):  1553 of Landroid/graphics/Point; 20B (1553 unique)
W/dalvikvm(  602):   261 of Landroid/graphics/Point; 28B (261 unique)
W/dalvikvm(  602):     1 of Landroid/view/MotionEvent; 100B
W/dalvikvm(  602):     1 of Landroid/app/ActivityThread$ApplicationThread; 28B
W/dalvikvm(  602):     1 of Landroid/content/ContentProvider$Transport; 28B
W/dalvikvm(  602):     1 of Landroid/view/Surface$CompatibleCanvas; 44B
W/dalvikvm(  602):     1 of Landroid/view/inputmethod/InputMethodManager$ControlledInputConnectionWrapper; 36B
W/dalvikvm(  602):     1 of Landroid/view/ViewRoot$1; 12B
W/dalvikvm(  602):     1 of Landroid/view/ViewRoot$W; 28B
W/dalvikvm(  602):     1 of Landroid/view/inputmethod/InputMethodManager$1; 28B
W/dalvikvm(  602):     1 of Landroid/view/accessibility/AccessibilityManager$1; 28B
W/dalvikvm(  602):     1 of Landroid/widget/LinearLayout$LayoutParams; 44B
W/dalvikvm(  602):     1 of Landroid/widget/LinearLayout; 332B
W/dalvikvm(  602):     2 of Lorg/apache/harmony/xnet/provider/jsse/TrustManagerImpl; 28B (1 unique)
W/dalvikvm(  602):     1 of Landroid/view/SurfaceView$MyWindow; 36B
W/dalvikvm(  602):     1 of Ltouchtest/RenderThread; 92B
W/dalvikvm(  602):     1 of Landroid/view/SurfaceView$3; 12B
W/dalvikvm(  602):     1 of Ltouchtest/DrawingView; 412B
W/dalvikvm(  602):     1 of Ltouchtest/Activity1; 180B
W/dalvikvm(  602): Memory held directly by tracked refs is 75624 bytes
E/dalvikvm(  602): Excessive JNI global references (2001)
E/dalvikvm(  602): VM aborting
```

Im obigen Beispiel (das sich auf den [Fehler 685215](https://bugzilla.novell.com/show_bug.cgi?id=685215) bezieht) besteht das Problem darin, dass zu viele Android.Graphics.Point-Instanzen erstellt werden. Eine Liste der Fehlerbehebungen für diesen speziellen Fehler finden Sie unter [comment \#2](https://bugzilla.novell.com/show_bug.cgi?id=685215#c2).

Es ist grundsätzlich sinnvoll, zunächst zu ermitteln, welchem Typ zu viele Instanzen zugeordnet sind. Im obigen Beispiel sind das Android.Graphics.Point-Instanzen. Dann müssen Sie herausfinden, wo sie im Quellcode erstellt wurden, und sie dann ggf. löschen (damit die Lebensdauer der Java-Objekte verkürzt wird). Dies hilft zwar nicht in jedem Fall weiter (\#685215 besteht aus einem Multithread, eine einfache Lösung vermeidet den Dispose-Aufruf), dieser Ansatz sollte allerdings trotzdem zunächst als erstes angewendet werden.

Sie können die [Protokollierung von globalen Verweisen](~/android/troubleshooting/index.md) aktivieren, um zu verfolgen, wann globale Verweise erstellt werden und wie viele vorhanden sind.

## <a name="abort-due-to-jni-type-mismatch"></a>Abbruch aufgrund von nicht übereinstimmenden JNI-Typen

Wenn Sie den JNI-Code manuell erstellen, stimmen die Typen möglicherweise nicht miteinander überein, z. B. wenn Sie versuchen, `java.lang.Runnable.run` für einen Typ aufzurufen, der `java.lang.Runnable` nicht implementiert hat. In diesem Fall wird im Android-Debugprotokoll eine Meldung ähnlich der folgenden angezeigt:

```shell
W/dalvikvm( 123): JNI WARNING: can't call Ljava/Type;;.method on instance of Lanother/java/Type;
W/dalvikvm( 123):              in Lmono/java/lang/RunnableImplementor;.n_run:()V (CallVoidMethodA)
...
E/dalvikvm( 123): VM aborting
```

## <a name="dynamic-code-support"></a>Unterstützung für dynamischen Code

### <a name="dynamic-code-does-not-compile"></a>Dynamischer Code wird nicht kompiliert

Wenn Sie in Ihrer Anwendung oder Bibliothek C\# Dynamic verwenden möchten, müssen Sie dem Projekt die Dateien „System.Core.dll, Microsoft.CSharp.dll and Mono.CSharp.dll“ und „Mono.CSharp.dll“ hinzufügen.

### <a name="in-release-build-missingmethodexception-occurs-for-dynamic-code-at-run-time"></a>Im Releasebuild tritt der Fehler „MissingMethodException“ bei dynamischem Code zur Laufzeit auf.

- Es ist wahrscheinlich, dass das Anwendungsprojekt keine Verweise auf „System.Core.dll“, „Microsoft.CSharp.dll“ oder „Mono.CSharp.dll“ enthält. Stellen Sie sicher, dass auf diese Assemblys verwiesen wird.

  - Denken Sie daran, dass dynamischer Code immer mit Kosten verbunden ist. Wenn Sie effizienten Code benötigen, sollten Sie keinen dynamischen Code verwenden.

- In der ersten Vorschau wurden diese Assemblys ausgeschlossen, es sei denn, die Typen in den einzelnen Assemblys werden explizit vom Anwendungscode verwendet. Unter dem folgenden Link finden Sie eine Problemumgehung: [http://lists.ximian.com/pipermail/mo...il/009798.html](http://lists.ximian.com/pipermail/monodroid/2012-April/009798.html)

## <a name="projects-built-with-aotllvm-crash-on-x86-devices"></a>Mit AOT+LLVM erstellte Projekte stürzen auf x86-Geräten ab

Beim Bereitstellen einer App, die mit [AOT+LLVM](~/android/deploy-test/release-prep/index.md) auf x86-basierten Geräten erstellt wurde, wird möglicherweise eine Ausnahmefehlermeldung ähnlich der folgenden angezeigt:

```shell
Assertion: should not be reached at /Users/.../external/mono/mono/mini/tramp-x86.c:124
Fatal signal 6 (SIGABRT), code -6 in tid 4051 (Xamarin.bug56111)
```

Dabei handelt es sich um ein bekanntes Problem. Die Problemumgehung besteht darin, LLVM zu deaktivieren.
