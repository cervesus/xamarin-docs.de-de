---
title: Hinweise zur Fehlerbehebung
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 56137ACA-4811-B312-6860-E16D0FA123F7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/15/2018
ms.openlocfilehash: b750dd4eebb4e181e3a1d3a33c6505bb58b3848b
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757084"
---
# <a name="troubleshooting-tips"></a>Hinweise zur Fehlerbehebung

## <a name="getting-diagnostic-information"></a>Diagnoseinformationen werden erhalten.

Xamarin. Android bietet einige Punkte, die beim Nachverfolgen verschiedener Fehler zu sehen sind.
Dazu gehören:

1. Diagnose MSBuild-Ausgabe.
2. Geräte Bereitstellungs Protokolle.
3. Android-Debug-Protokoll Ausgabe.

<a name="Diagnostic_MSBuild_Output" />

## <a name="diagnostic-msbuild-output"></a>Diagnose MSBuild-Ausgabe

Die Diagnose MSBuild kann zusätzliche Informationen im Zusammenhang mit der Paket Erstellung enthalten und kann einige Informationen zur Paket Bereitstellung enthalten.

So aktivieren Sie die Diagnoseausgabe von MSBuild in Visual Studio

1. Klicken Sie auf Extras **> Optionen...**
2. Wählen Sie in der linken Struktur **Projekte und Projektmappen > Erstellen und ausführen** aus.
3. Legen Sie im rechten Bereich das Dropdown Menü Ausführlichkeit der MSBuild-Buildausgabe auf Diagnose fest.
4. Klicken Sie auf **OK**.
5. Bereinigen Sie Ihr Paket, und erstellen Sie es erneut.
6. Die Diagnoseausgabe ist im Ausgabebereich sichtbar.

So aktivieren Sie die MSBuild-Diagnoseausgabe in Visual Studio für Mac/OS X:

1. Klicken Sie auf **Visual Studio für Mac > Einstellungen...**
2. Wählen Sie in der linken Struktur die Option **Projekte > Build** aus.
3. Legen Sie im rechten Bereich die Dropdown-Option Protokoll Ausführlichkeit auf Diagnose fest.
4. Klicken Sie auf **OK**.
5. Starten Sie Visual Studio für Mac neu.
6. Bereinigen Sie Ihr Paket, und erstellen Sie es erneut.
7. Die Diagnoseausgabe wird im Fehler-Pad angezeigt (**Anzeigen von > Pads > Fehlern** ), indem Sie auf die Schaltfläche Ausgabe erstellen klicken.

## <a name="device-deployment-logs"></a>Geräte Bereitstellungs Protokolle

So aktivieren Sie die Protokollierung der Geräte Bereitstellung in Visual Studio:

1. **Tools > Optionen...** >
2. Wählen Sie in der linken Strukturansicht **xamarin > Android-Einstellungen** aus.
3. Aktivieren Sie im rechten Bereich das Kontrollkästchen [X] **Erweiterung Debugprotokollierung (schreibt "monodroid. log" in den Desktop)** .
4. Protokollmeldungen werden in die Datei "monodroid. log" auf dem Desktop geschrieben.

Von Visual Studio für Mac werden immer Geräte Bereitstellungs Protokolle geschrieben. Die Suche ist etwas schwieriger. eine *androidutils* -Protokolldatei wird für jeden Tag und die Uhrzeit der Bereitstellung erstellt, z. b.: **Androidtools-2012-10 -24 _12-35 -45. log**.

- Unter Windows werden Protokolldateien in `%LOCALAPPDATA%\XamarinStudio-{VERSION}\Logs`geschrieben.
- Unter OS X werden Protokolldateien in `$HOME/Library/Logs/XamarinStudio-{VERSION}`geschrieben.

## <a name="android-debug-log-output"></a>Android-Debug-Protokoll Ausgabe

Android schreibt viele Nachrichten in das [Android-Debugprotokoll](~/android/deploy-test/debugging/android-debug-log.md).
Xamarin. Android verwendet Android-Systemeigenschaften, um die Generierung zusätzlicher Nachrichten im Android-Debugprotokoll zu steuern. Android-Systemeigenschaften können über den Befehl *setprop* innerhalb des [Android Debug Bridge (ADB)](https://developer.android.com/guide/developing/tools/adb.html)festgelegt werden:

```shell
adb shell setprop PROPERTY_NAME PROPERTY_VALUE
```

System Eigenschaften werden während des Prozess Starts gelesen und müssen daher entweder vor dem Starten der Anwendung festgelegt werden, oder die Anwendung muss neu gestartet werden, nachdem die Systemeigenschaften geändert wurden.

### <a name="xamarinandroid-system-properties"></a>Xamarin.Android-Systemeigenschaften

Xamarin. Android unterstützt die folgenden Systemeigenschaften:

- *debug.mono.debug*: Wenn eine nicht leere Zeichenfolge ist, entspricht `*mono-debug*`dies.

- *debug.mono.env*: Eine Pipe-getrennte *|* Liste von Umgebungsvariablen, die beim Starten der Anwendung exportiert werden sollen, *bevor* Mono initialisiert wurde. Dies ermöglicht das Festlegen von Umgebungsvariablen, die Mono-Protokollierung steuern.

  - *Hinweis:* Da der Wert ' *|* ' getrennt ist, muss der Wert über eine zusätzliche Ebene von Anführungszeichen verfügen, da der \` \`ADB-Shellbefehl eine Reihe von Anführungszeichen entfernt.

  - *Hinweis:* Android-System Eigenschaftswerte dürfen nicht länger als 92 Zeichen sein.

  - Beispiel:

    ```
    adb shell setprop debug.mono.env "'MONO_LOG_LEVEL=info|MONO_LOG_MASK=asm'"
    ```

- *debug.mono.log*: Eine durch Trennzeichen getrennte (" *,* ") Liste von Komponenten, die zusätzliche Nachrichten in das Android-Debugprotokoll Drucken sollten. Standardmäßig ist nichts festgelegt. Zu den Komponenten gehören:

  - *alle*: Alle Nachrichten drucken
  - *gc*: Drucken von GC-bezogenen Nachrichten.
  - *gref*: Drucken (schwache, globale) Verweis Zuordnungs-und Zuordnungs Zuordnungs Nachrichten.
  - *lref*: Drucken von lokalen Verweis Zuordnungs-und-Zuordnungs Nachrichten.

  *Hinweis*: Diese sind *sehr* ausführlich. Aktivieren Sie diese Option nur, wenn dies wirklich erforderlich ist.

- *debug.mono.trace*: Ermöglicht das Festlegen der [Mono-Trace](http://docs.go-mono.com/?link=man%3amono(1)) `=PROPERTY_VALUE` -Einstellung.

## <a name="deleting-bin-and-obj"></a>Löschen `bin` von und`obj`

Xamarin. Android hat in der Vergangenheit von einer Situation wie z. b.:

- Es tritt ein merkwürdiger Build-oder Laufzeitfehler auf.
- Sie `Clean`können `Rebuild`Ihre Verzeichnisse`bin` und`obj` manuell löschen.
- Das Problem wird entfernt.

Wir haben stark investiert, um Probleme zu beheben, z. b. aufgrund ihrer Auswirkung auf die Produktivität von Entwicklern.

Wenn ein Problem wie das folgende auftritt:

1. Notieren Sie sich den Hinweis. Was war die letzte Aktion, die Ihr Projekt in diesen Zustand versetzt hat?
1. Speichern Sie das aktuelle Buildprotokoll. Versuchen Sie es erneut, und zeichnen Sie ein [Diagnosebuildprotokoll](#diagnostic-msbuild-output) auf.
1. Senden Sie einen [Fehlerbericht][bug].

Bevor Sie Ihre `bin` Verzeichnisse `obj` und löschen, zippen Sie Sie, und speichern Sie Sie zur späteren Diagnose, wenn dies erforderlich ist. Sie können das xamarin. Android-Anwendungsprojekt wahrscheinlich nur `Clean` in Ihrem xamarin. Android-Anwendungsprojekt ausführen.

[bug]: https://github.com/xamarin/xamarin-android/wiki/Submitting-Bugs,-Feature-Requests,-and-Pull-Requests

## <a name="xamarinandroid-cannot-resolve-systemvaluetuple"></a>Xamarin. Android kann System. valuetuple nicht auflösen.

Dieser Fehler tritt aufgrund einer Inkompatibilität mit Visual Studio auf.

- **Visual Studio 2017 Update 1** (Version 15,1 oder älter) ist nur mit dem **System. valuetuple-nuget 4.3.0** (oder älter) kompatibel.

- **Visual Studio 2017 Update 2** (Version 15,2 oder höher) ist nur mit dem **System. valuetuple-nuget-4.3.1** (oder höher) kompatibel.

Wählen Sie das richtige System. valuetuple-nuget aus, das Ihrer Visual Studio 2017-Installation entspricht.

## <a name="gc-messages"></a>GC-Nachrichten

Die Nachrichten von GC-Komponenten können angezeigt werden, indem die Debug. Mono. Log-System Eigenschaft auf einen Wert festgelegt wird, der GC enthält.

GC-Nachrichten werden immer dann generiert, wenn der GC ausgeführt wird und Informationen dazu bereitstellt, wie viel Arbeit die GC hat:

```shell
I/monodroid-gc(12331): GC cleanup summary: 81 objects tested - resurrecting 21.
```

Zusätzliche GC-Informationen, wie z. b. Zeit Steuerungsinformationen `MONO_LOG_LEVEL` , können generiert `debug`werden, indem die-Umgebungsvariable auf

```shell
adb shell setprop debug.mono.env MONO_LOG_LEVEL=debug
```

Dies führt zu (vielen) zusätzlichen Mono-Nachrichten, einschließlich der folgenden drei Konsequenzen:

```shell
D/Mono (15723): GC_BRIDGE num-objects 1 num_hash_entries 81226 sccs size 81223 init 0.00ms df1 285.36ms sort 38.56ms dfs2 50.04ms setup-cb 9.95ms free-data 106.54ms user-cb 20.12ms clenanup 0.05ms links 5523436/5523436/5523096/1 dfs passes 1104 6883/11046605
D/Mono (15723): GC_MINOR: (Nursery full) pause 2.01ms, total 287.45ms, bridge 225.60 promoted 0K major 325184K los 1816K
D/Mono ( 2073): GC_MAJOR: (user request) pause 2.17ms, total 2.47ms, bridge 28.77 major 576K/576K los 0K/16K
```

In der `GC_BRIDGE` Meldung ist `num-objects` die Anzahl von Bridge Objekten, die dieser Durchlauf berücksichtigt, und `num_hash_entries` die Anzahl der Objekte, die während dieses auflaufs des Bridge Codes verarbeitet werden.

In der `GC_MINOR` - `GC_MAJOR` und der `total` -Nachricht ist die Zeitspanne, in der die Welt angehalten wird (es werden keine Threads `bridge` ausgeführt), während die Zeitspanne ist, die der Verarbeitungs Code der Bridge benötigt (der die Java-VM verarbeitet). Die Welt wird *nicht* angehalten, während die Bridge Verarbeitung stattfindet.

 *Im allgemeinen*gilt: je größer der Wert `num_hash_entries`von ist, desto mehr Zeit `bridge` werden die Auflistungen benötigt, und `total` desto größer ist die Zeit, die für das Sammeln aufgewendet wird.

## <a name="global-reference-messages"></a>Globale Verweis Nachrichten

Um die Protokoll Protokollierung (Global Reference loggigs) zu aktivieren, muss die *Debug. Mono. log* -System Eigenschaft *Gref*enthalten, z. b.:

```shell
adb shell setprop debug.mono.log gref
```

Xamarin. Android verwendet globale Android-Verweise, um Zuordnungen zwischen Java-Instanzen und den zugeordneten verwalteten Instanzen bereitzustellen, wie beim Aufrufen einer Java-Methode, die Java-Instanzen bereitgestellt werden muss.

Leider erlauben Android-Emulatoren nur, dass gleichzeitig 2000 globale Verweise vorhanden sind. Hardware hat eine viel höhere Grenze von 52000 globalen verweisen. Der untere Grenzwert kann problematisch sein, wenn Sie Anwendungen im Emulator ausführen, sodass Sie wissen, *woher* die Instanz stammt, sehr nützlich sein kann.

 *Hinweis*: der globale Verweis Zähler ist intern für xamarin. Android und kann keine globalen Verweise enthalten, die von anderen in den Prozess geladenen nativen Bibliotheken übernommen werden. Verwenden Sie den globalen Verweis Zähler als Schätzung.

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

Es gibt vier Nachrichten, die Folgen:

- Globale Verweis Erstellung: Hierbei handelt es sich um die Zeilen, die mit *+ g +* beginnen und eine Stapel Überwachung für den erstellten Codepfad bereitstellen.
- Globale Verweis Zerstörung: Hierbei handelt es sich um die Zeilen, die mit " *-g* " beginnen, und es kann eine Stapel Überwachung für den Codepfad zur Freigabe des globalen Verweises bereitgestellt werden. Wenn der GC die Gref freigibt, wird keine Stapel Überwachung bereitgestellt.
- Schwache globale Verweis Erstellung: Hierbei handelt es sich um die Zeilen, die mit *+ w +* beginnen.
- Schwache globale verweiszerstörung: Hierbei handelt es sich um Zeilen, die mit *-w-* beginnen.

In allen Nachrichten ist der *grefc* -Wert die Anzahl der globalen Verweise, die xamarin. Android erstellt hat, während der *grefwc* -Wert die Anzahl der schwachen globalen Verweise ist, die xamarin. Android erstellt hat. Der *handle* *-oder obj-handle-* Wert ist der Wert des jni-Handles, und das */* Zeichen nach ' ' ist der Typ des Handle-Werts: */L* for local Reference, */G* for Global References und */W* for Weak Global References.

Im Rahmen des GC-Prozesses werden globale Verweise (+ g +) in schwache globale Verweise konvertiert (was zu "+ w +" und "-g" führt), eine Java-seitige GC wird gestartet, und anschließend wird die schwache globale Referenz geprüft, um festzustellen, ob Sie gesammelt wurde. Wenn es immer noch aktiv ist, wird eine neue Gref um den schwachen Ref (+ g +,-w-) erstellt. andernfalls wird der schwache Verweis zerstört (-w).

## <a name="java-instance-is-created-and-wrapped-by-a-mcw"></a>Die Java-Instanz wird von einem MCW erstellt und umschließt.

```shell
I/monodroid-gref(27679): +g+ grefc 2211 gwrefc 0 obj-handle 0x4066df10/L -> new-handle 0x4066df10/L from ...
I/monodroid-gref(27679): handle 0x4066df10; key_handle 0x4066df10: Java Type: `android/graphics/drawable/TransitionDrawable`; MCW type: `Android.Graphics.Drawables.TransitionDrawable`
```

## <a name="a-gc-is-being-performed"></a>Eine GC wird ausgeführt...

```shell
I/monodroid-gref(27679): +w+ grefc 1953 gwrefc 259 obj-handle 0x4066df10/G -> new-handle 0xde68f95f/W from take_weak_global_ref_jni
I/monodroid-gref(27679): -g- grefc 1952 gwrefc 259 handle 0x4066df10/G from take_weak_global_ref_jni
```

## <a name="object-is-still-alive-as-handle--null"></a>Das Objekt ist noch aktiv, als handle! = NULL.
## <a name="wref-turned-back-into-a-gref"></a>Wref in eine Gref zurückgekehrt

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x4066df10
I/monodroid-gref(27679): +g+ grefc 1930 gwrefc 39 obj-handle 0xde68f95f/W -> new-handle 0x4066df10/G from take_global_ref_jni
I/monodroid-gref(27679): -w- grefc 1930 gwrefc 38 handle 0xde68f95f/W from take_global_ref_jni
```

## <a name="object-is-dead-as-handle--null"></a>Objekt ist tot, als handle = = NULL
## <a name="wref-is-freed-no-new-gref-created"></a>Wref wurde freigegeben, es wurde keine neue Gref erstellt.

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x0
I/monodroid-gref(27679): -w- grefc 1914 gwrefc 296 handle 0xde68f95f/W from take_global_ref_jni
```

Es gibt hier ein "interessantes" Falten: auf Zielen, auf denen Android vor 4,0 ausgeführt wird, ist der Gref-Wert mit der Adresse des Java-Objekts im Arbeitsspeicher der Android-Laufzeit identisch. (Das heißt, die GC ist ein nicht beweglicher, konservativer, Collector und übergibt direkte Verweise auf diese Objekte.) Folglich hat die resultierende Gref nach einer + g +, + w +,-g-, + g +,-w-Sequenz denselben Wert wie der ursprüngliche Gref-Wert. Dies erleichtert das Durchlaufen von Protokollen.

Android 4,0 verfügt jedoch über einen verschiebenden Collector und übergibt direkte Verweise auf Android-Runtime-VM-Objekte nicht mehr. Folglich ist der Gref-Wert nach einer + g +, + w +,-g-, + g +,-w-Sequence *anders*. Wenn das Objekt mehrere GCS überdauert, werden mehrere Gref-Werte verwendet, um zu ermitteln, wo eine Instanz tatsächlich von zugewiesen wurde.

### <a name="querying-programmatically"></a>Programm gesteuertes Abfragen

Sie können sowohl die Gref-als auch die Wref-Anzahl Abfragen `JniRuntime` , indem Sie das-Objekt Abfragen.

`Java.Interop.JniRuntime.CurrentRuntime.GlobalReferenceCount`-Globaler Verweis Zähler

`Java.Interop.JniRuntime.CurrentRuntime.WeakGlobalReferenceCount`-Schwacher Verweis Zähler

## <a name="android-debug-logs"></a>Android-Debugprotokolle

Die [Android-Debugprotokolle](~/android/deploy-test/debugging/android-debug-log.md) können zusätzlichen Kontext in Bezug auf Laufzeitfehler enthalten, die angezeigt werden.

## <a name="floating-point-performance-is-terrible"></a>Die Gleit Komma Leistung ist schrecklich!

Alternativ: "meine APP wird mit dem Debugbuild zehnmal schneller ausgeführt als mit dem Releasebuild!"

Xamarin. Android unterstützt mehrere Geräte ABIS: *ARMEABI*, *ARMEABI-v7a*und *x86*. Device ABIS kann in den **Projekteigenschaften > Registerkarte Anwendung > Unterstützte Architekturen**angegeben werden.

Bei Debugbuilds wird ein Android-Paket verwendet, das alle ABIS bereitstellt und somit die schnellste ABI für das Zielgerät verwendet.

Releasebuilds enthalten nur die Option ABIS, die auf der Registerkarte Projekteigenschaften ausgewählt ist. Es können mehrere ausgewählt werden.

*ARMEABI* ist die Standard-ABI und bietet die breiteste Geräte Unterstützung. *Allerdings*unterstützt ARMEABI nicht MultiCPU-Geräte und Hardware-Gleit Komma, andere Dinge. Folglich werden apps, die die ARMEABI Release Runtime verwenden, an einen einzelnen Kern gebunden und verwenden eine Soft-float-Implementierung. Beide können zu einer deutlich langsameren Leistung für Ihre APP beitragen.

Wenn Ihre APP eine angemessene Gleit Komma Leistung (z. b. Spiele) erfordert, sollten Sie die *ARMEABI-v7a* ABI aktivieren. Möglicherweise möchten Sie nur die " *ARMEABI-v7a* Runtime" unterstützen. Dies bedeutet jedoch, dass ältere Geräte, die nur *ARMEABI* unterstützen, Ihre APP nicht ausführen können.

## <a name="could-not-locate-android-sdk"></a>Android SDK wurde nicht gefunden.

In Google sind 2 Downloads für die Android SDK für Windows verfügbar.
Wenn Sie das exe-Installationsprogramm auswählen, werden die Registrierungsschlüssel geschrieben, die xamarin. Android angeben, wo es installiert wurde. Wenn Sie die ZIP-Datei auswählen und Sie selbst entpacken, weiß xamarin. Android nicht, wo nach dem SDK gesucht werden soll. Sie können xamarin. Android angeben, wo sich das SDK in Visual Studio befindet, indem Sie zu Extras **> Optionen > xamarin > Android-Einstellungen**wechseln:

[![Android SDK Speicherort in den xamarin Android-Einstellungen](troubleshooting-images/01.png)](troubleshooting-images/01.png#lightbox)

## <a name="ide-does-not-display-target-device"></a>IDE zeigt das Zielgerät nicht an

Manchmal versuchen Sie, die Anwendung auf einem Gerät bereitzustellen, aber das Gerät, das Sie bereitstellen möchten, wird im Dialogfeld "Gerät auswählen" nicht angezeigt. Dies kann der Fall sein, wenn die Android Debug Bridge entscheidet, in den Urlaub zu gehen.

Zum Diagnostizieren dieses Problems suchen Sie das [ADB-Programm](~/android/deploy-test/debugging/android-debug-log.md), und führen Sie dann Folgendes aus:

```shell
adb devices
```

Wenn Ihr Gerät nicht vorhanden ist, müssen Sie den Android Debug Bridge Server neu starten, damit Ihr Gerät gefunden werden kann:

```shell
adb kill-server
adb start-server
```

Die Version von " **ADB Start-Server** " kann durch die Ausführung von HTC synchronisiert werden. Wenn der **ADB-Befehl Start-Server** nicht ausdruckt, auf welchem Port er gestartet wird, beenden Sie die HTC-Synchronisierungs Software, und versuchen Sie, den ADB-Server neu zu starten.

## <a name="the-specified-task-executable-keytool-could-not-be-run"></a>Die angegebene ausführbare Task Datei "keytool" konnte nicht ausgeführt werden.

Dies bedeutet, dass ihr Pfad nicht das Verzeichnis enthält, in dem sich das Verzeichnis "bin" des Java SDK befindet. Überprüfen Sie, ob Sie diese Schritte im [Installations](~/android/get-started/installation/index.md) Handbuch befolgt haben.

## <a name="monodroidexe-or-aresgenexe-exited-with-code-1"></a>"monodroid. exe" oder "aresgen. exe" wurde mit dem Code 1 beendet.

Um dieses Problem zu beheben, navigieren Sie zu Visual Studio, und ändern Sie den ausführlichkeits Grad von MSBuild. Wählen Sie hierzu Folgendes aus: **Tools > Optionen > Projekt und Projekt** Mappen **> Erstellen** und **Ausführen > MSBuild-Projektbuildausgabe Ausführlichkeit** , und legen Sie diesen Wert auf **Normal**fest.

Erstellen Sie neu, und überprüfen Sie den Ausgabebereich von Visual Studio, der den vollständigen Fehler enthalten sollte.

## <a name="there-is-not-enough-storage-space-on-the-device-to-deploy-the-package"></a>Auf dem Gerät ist nicht genügend Speicherplatz vorhanden, um das Paket bereitzustellen.

Dies tritt auf, wenn Sie den Emulator nicht innerhalb von Visual Studio starten. Wenn Sie den Emulator außerhalb von Visual Studio starten, müssen Sie die `-partition-size 512` Optionen übergeben, z. b.

```shell
emulator -partition-size 512 -avd MonoDroid
```

Stellen Sie sicher, dass Sie den richtigen simulatornamen verwenden, also [den Namen, den Sie beim Konfigurieren des Simulators verwendet](~/android/get-started/installation/windows.md#device)haben.

## <a name="install_failed_invalid_apk-when-installing-a-package"></a>Bei der Installation\_eines Pakets ist ein\_UngültigesAPK installiert.\_

Android-Paketnamen *müssen* einen Zeitraum (" *.* ") enthalten. Bearbeiten Sie den Paketnamen so, dass er einen bestimmten Zeitraum enthält.

- In Visual Studio:
  - Klicken Sie mit der rechten Maustaste auf > Eigenschaften
  - Klicken Sie Links auf die Registerkarte Android-Manifest.
  - Aktualisieren Sie das Feld Paketname.
    - Wenn die Meldung &ldquo;"androidmanifest. xml" wurde nicht gefunden. Klicken Sie, um eine hinzuzufügen. &rdquo;, klicken Sie auf den Link, und aktualisieren Sie dann das Feld Paketname.
- In Visual Studio für Mac:
  - Klicken Sie mit der rechten Maustaste auf das Projekt > Optionen.
  - Navigieren Sie zum Abschnitt Build/Android-Anwendung.
  - Ändern Sie das Feld Paketname so, dass es ein "." enthält.

## <a name="install_failed_missing_shared_library-when-installing-a-package"></a>Fehler beim Installieren\_von\_\_fehlenden freigegebenen Bibliotheken beim Installieren eines Pakets\_

Eine "freigegebene Bibliothek" in diesem Kontext ist *keine* Native freigegebene Bibliotheksdatei (*libfoo.so*). Es handelt sich vielmehr um eine Bibliothek, die separat auf dem Zielgerät installiert werden muss, z. b. Google Maps.

Das Android-Paket gibt an, welche freigegebenen Bibliotheken `<uses-library/>` mit dem-Element benötigt werden. Wenn eine *erforderliche* Bibliothek auf dem Zielgerät nicht vorhanden ist (z. `//uses-library/@android:required` b. " *true*", dies ist die Standardeinstellung), schlägt die Paketinstallation fehl, weil die *\_Installation fehl\_geschlagen\_ist.\_ Bibliothek*.

Um zu ermitteln, welche freigegebenen Bibliotheken erforderlich sind, zeigen Sie die *generierte*
Datei "**androidmanifest. XML** " an (z. **b.\\obj Debuggen\\Android\\androidmanifest. XML**) und suchen nach dem `<uses-library/>` -Elemente. `<uses-library/>`Elemente können manuell in den **Eigenschaften\\der Datei "androidmanifest. XML** " des Projekts und über das [benutzerdefinierte Attribut "sereslibraryattribute](xref:Android.App.UsesLibraryAttribute)" hinzugefügt werden.

Wenn Sie z. b. einen Assemblyverweis auf *Mono. Android. GoogleMaps. dll* hinzufügen, wird implizit ein für die frei `<uses-library/>` gegebene Google Maps-Bibliothek hinzugefügt.

## <a name="install_failed_update_incompatible-when-installing-a-package"></a>Nicht kompatiblesUpdate\_beim Installieren eines Pakets\_\_

Für Android-Pakete gelten drei Anforderungen:

- Sie müssen ein "." enthalten. (siehe vorheriger Eintrag)
- Sie müssen einen eindeutigen Zeichen folgen-Paketnamen aufweisen (daher die Reverse-TLD-Konvention in Android-App-Namen, z. b. com. Android. Chrome für die Chrome-APP).
- Beim Aktualisieren von Paketen muss das Paket denselben Signatur Schlüssel aufweisen.

Stellen Sie sich daher Folgendes Szenario vor:

1. Sie erstellen & Ihre APP als debuggingapp bereitzustellen.
2. Sie ändern den Signatur Schlüssel, z. b. so, dass er als releaseapp verwendet wird (oder weil Ihnen der standardmäßige debugsignaturschlüssel nicht gefällt).
3. Sie installieren ihre APP, ohne Sie zuerst zu entfernen, z. b. Debuggen > Starten ohne Debuggen in Visual Studio

Wenn dies geschieht, schlägt die Paketinstallation mit dem Fehler\_"\_Fehler\_bei der Installation fehlgeschlagen" fehl, da sich der Paketname nicht geändert hat, während der Signatur Schlüssel nicht geändert wurde Das [Android-Debugprotokoll](~/android/deploy-test/debugging/android-debug-log.md) enthält auch eine Meldung ähnlich der folgenden:

```shell
E/PackageManager(  146): Package [PackageName] signatures do not match the previously installed version; ignoring!
```

Entfernen Sie die Anwendung vor der erneuten Installation vollständig von Ihrem Gerät, um diesen Fehler zu beheben.

## <a name="install_failed_uid_changed-when-installing-a-package"></a>Fehler beim Installieren\_der\_UID bei der Installation eines Pakets.\_

Wenn ein Android-Paket installiert ist, wird ihm eine *Benutzer-ID* (UID) zugewiesen.
Gelegentlich tritt `INSTALL_FAILED_UID_CHANGED`bei der Installation aus unbekannten Gründen bei der Installation über eine bereits installierte App ein Fehler auf:

```shell
ERROR [2015-03-23 11:19:01Z]: ANDROID: Deployment failed
Mono.AndroidTools.InstallFailedException: Failure [INSTALL_FAILED_UID_CHANGED]
   at Mono.AndroidTools.Internal.AdbOutputParsing.CheckInstallSuccess(String output, String packageName)
   at Mono.AndroidTools.AndroidDevice.<>c__DisplayClass2c.<InstallPackage>b__2b(Task`1 t)
   at System.Threading.Tasks.ContinuationTaskFromResultTask`1.InnerInvoke()
   at System.Threading.Tasks.Task.Execute()
```

Um dieses Problem zu umgehen, müssen Sie das Android-Paket *vollständig deinstallieren* , indem Sie die APP entweder über die Benutzeroberfläche `adb`des Android-Ziels installieren oder Folgendes verwenden:

```shell
$ adb uninstall @PACKAGE_NAME@
```

**nicht verwenden** , da dadurch Anwendungsdaten erhalten bleiben und somit die in Konflikt stehende UID auf dem Zielgerät beibehalten wird. `adb uninstall -k`

## <a name="release-apps-fail-to-launch-on-device"></a>Release-Apps können auf dem Gerät nicht gestartet werden.

Enthält die Android-Debug-Protokoll Ausgabe eine Meldung ähnlich der folgenden:

```shell
D/AndroidRuntime( 1710): Shutting down VM
W/dalvikvm( 1710): threadid=1: thread exiting with uncaught exception (group=0xb412f180)
E/AndroidRuntime( 1710): FATAL EXCEPTION: main
E/AndroidRuntime( 1710): java.lang.UnsatisfiedLinkError: Couldn't load monodroid: findLibrary returned null
E/AndroidRuntime( 1710):        at java.lang.Runtime.loadLibrary(Runtime.java:365)
```

Wenn dies der Fall ist, gibt es zwei mögliche Ursachen:

1. Die APK-Datei bietet keine ABI, die das Zielgerät unterstützt.
    Die APK-Datei enthält z. b. nur ARMEABI-v7a-Binärdateien, und das Zielgerät unterstützt nur ARMEABI.

2. Ein [Android-Fehler](http://code.google.com/p/android/issues/detail?id=21670). Wenn dies der Fall ist, deinstallieren Sie die APP, überwinden Sie die Finger, und installieren Sie die APP neu.

Um (1) zu korrigieren, bearbeiten Sie die Projektoptionen/-Eigenschaften, und [fügen Sie die Unterstützung für die erforderliche ABI der Liste der unterstützten ABIS hinzu](~/android/app-fundamentals/cpu-architectures.md). Um zu ermitteln, welche ABI Sie hinzufügen müssen, führen Sie den folgenden ADB-Befehl für das Zielgerät aus:

```shell
adb shell getprop ro.product.cpu.abi
adb shell getprop ro.product.cpu.abi2
```

Die Ausgabe enthält den primären (und optionalen sekundären) ABIS.

```shell
$ adb shell getprop | grep ro.product.cpu
[ro.product.cpu.abi2]: [armeabi]
[ro.product.cpu.abi]: [armeabi-v7a]
```

## <a name="the-outpath-property-is-not-set-for-project-ldquomyappcsprojrdquo"></a>Die outPath-Eigenschaft ist für das Projekt &ldquo;"MyApp. csproj" nicht festgelegt.&rdquo;

Dies bedeutet im Allgemeinen, dass Sie über einen HP-Computer &ldquo;verfügen&rdquo; und die Umgebungsvariablen Plattform auf etwa MCD oder HPD festgelegt wurde. Dies steht in Konflikt mit der MSBuild Platform-Eigenschaft, die &ldquo;im allgemeinen&rdquo; auf eine&rdquo;beliebige CPU oder &ldquo;x86 festgelegt ist. Sie müssen diese Umgebungsvariable von Ihrem Computer entfernen, bevor MSBuild funktionsfähig ist:

- System Steuerung > System > Advanced > Umgebungsvariablen

Starten Sie Visual Studio neu, oder führen Sie Visual Studio für Mac erneut aus. Die Dinge sollten jetzt erwartungsgemäß funktionieren.

## <a name="javalangclasscastexception-monoandroidruntimejavaobject-cannot-be-cast-to"></a>Java. lang. classcastexception: Mono. Android. Runtime. JavaObject kann nicht in...

Xamarin. Android 4. x lässt die ordnungsgemäße Ausführung von generischen generischen Typen nicht ordnungsgemäß aus. Verwenden Sie beispielsweise den folgenden C\# -Code mit [simpleexpandablelistadapter](xref:Android.Widget.SimpleExpandableListAdapter):

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

Das Problem besteht darin, dass xamarin. Android fälschlicherweise die generischen generischen Typen marsheitet. Das `List<IDictionary<string, object>>` wird an eine [java. lang. arrraylist](xref:Java.Util.ArrayList)gemarshallt, aber das `ArrayList` -Objekt `mono.android.runtime.JavaObject` enthält-Instanzen (die `Dictionary<string, object>` auf die-Instanzen verweisen) anstelle von etwas, das " [java. util. map](xref:Java.Util.IMap)" implementiert. Dies führt zu folgender Ausnahme:

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

Die Problem Umgehung besteht darin, die bereitgestellten Java-Auflistungs [Typen](~/android/internals/api-design.md) anstelle &ldquo; `System.Collections.Generic` der&rdquo; Typen für die inneren Typen zu verwenden. Dies führt beim Marshalling der Instanzen zu entsprechenden Java-Typen. (Der folgende Code ist komplizierter als notwendig, um die Gref-Lebensdauer zu verringern. Sie kann vereinfacht werden, um den ursprünglichen Code über `s/List/JavaList/g` zu `s/Dictionary/JavaDictionary/g` ändern, und, wenn die Gref-Lebensdauer keine Sorge ist.)

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

[Dies wird in einer zukünftigen Version korrigiert](https://bugzilla.xamarin.com/show_bug.cgi?id=5401).

## <a name="unexpected-nullreferenceexceptions"></a>Unerwartete NullReferenceExceptions

Gelegentlich gibt das [Android-Debugprotokoll](~/android/deploy-test/debugging/android-debug-log.md) NullReferenceExceptions &ldquo;an, das&rdquo; nicht vorkommen kann, oder aus Mono für Android-Lauf Zeit Code in Kürze vor dem Ausführen der APP:

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

Dies kann der Fall sein, wenn die Android-Laufzeit den Prozess abbricht, was aus einer beliebigen Anzahl von Gründen auftreten kann. dazu gehört das Erreichen des Gref- &ldquo;Limits&rdquo; für das Ziel oder das Auftreten eines Fehlers mit jni.

Um zu ermitteln, ob dies der Fall ist, überprüfen Sie das Android-Debugprotokoll auf eine Meldung von Ihrem Prozess, die folgendem ähnelt:

```shell
E/dalvikvm(  123): VM aborting
```

## <a name="abort-due-to-global-reference-exhaustion"></a>Abbrechen aufgrund der globalen Verweis Erschöpfung

Die jni-Ebene der Android-Laufzeit unterstützt nur eine begrenzte Anzahl von jni-Objekt verweisen, die zu einem bestimmten Zeitpunkt gültig sind. Wenn dieser Grenzwert überschritten wird, wird der Wert unterschritten.

Der Grenzwert für Gref (*Global Reference*) beträgt 2000 Verweise im Emulator und ~ 52000 Verweise auf Hardware.

Sie werden feststellen, dass zu viele Grefs erstellt werden, wenn Nachrichten wie diese im Android-Debugprotokoll angezeigt werden:

```shell
D/dalvikvm(  602): GREF has increased to 1801
```

Wenn Sie das Gref-Limit erreichen, wird eine Meldung wie die folgende ausgegeben:

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

Im obigen Beispiel (das übrigens von [Fehler 685215](https://bugzilla.novell.com/show_bug.cgi?id=685215)stammt) liegt das Problem darin, dass zu viele Android. Graphics. Point-Instanzen erstellt werden. eine Liste der Fehlerbehebungen für diesen speziellen Fehler finden Sie in [Kommentar \#2](https://bugzilla.novell.com/show_bug.cgi?id=685215#c2) .

In der Regel besteht eine sinnvolle Lösung darin, herauszufinden, welcher Typ zu viele &ndash; Instanzen hat Android. Graphics zugeordnet. zeigen Sie &ndash; in der obigen Dumpdatei nach, wo Sie im Quellcode erstellt werden, und löschen Sie Sie entsprechend (damit Ihre Die Java-Objekt Lebensdauer wurde verkürzt.) Dies ist nicht immer angebracht (\#685215 ist multithreaded, sodass die triviale Lösung den Löschvorgang vermeidet), aber es ist das erste zu berücksichtigende.

Sie können die [Gref-Protokollierung](~/android/troubleshooting/index.md) aktivieren, um festzustellen, wann Grefs erstellt werden und wie viele vorhanden sind.

## <a name="abort-due-to-jni-type-mismatch"></a>Abbruch aufgrund von jni-Typen Konflikt

Wenn Sie den jni-Code per Hand führen, ist es möglich, dass die Typen nicht korrekt abgeglichen werden, z. `java.lang.Runnable.run` b. Wenn Sie versuchen, `java.lang.Runnable`für einen Typ aufzurufen, der nicht implementiert. In diesem Fall wird im Android-Debugprotokoll eine Meldung ähnlich der folgenden angezeigt:

```shell
W/dalvikvm( 123): JNI WARNING: can't call Ljava/Type;;.method on instance of Lanother/java/Type;
W/dalvikvm( 123):              in Lmono/java/lang/RunnableImplementor;.n_run:()V (CallVoidMethodA)
...
E/dalvikvm( 123): VM aborting
```

## <a name="dynamic-code-support"></a>Dynamische Code Unterstützung

### <a name="dynamic-code-does-not-compile"></a>Dynamischer Code wird nicht kompiliert.

Um C\# Dynamic in Ihrer Anwendung oder Bibliothek zu verwenden, müssen Sie dem Projekt die Datei "System. Core. dll", "Microsoft. CSharp. dll" und "Mono. CSharp. dll" hinzufügen.

### <a name="in-release-build-missingmethodexception-occurs-for-dynamic-code-at-run-time"></a>Im Releasebuild tritt MissingMethodException bei dynamischem Code zur Laufzeit auf.

- Es ist wahrscheinlich, dass das Anwendungsprojekt keine Verweise auf "System. Core. dll", "Microsoft. CSharp. dll" oder "Mono. CSharp. dll" enthält. Stellen Sie sicher, dass auf diese Assembly verwiesen wird.

  - Denken Sie daran, dass dynamischer Code immer Kosten hat. Wenn Sie effizienten Code benötigen, sollten Sie die Verwendung von dynamischem Code in Erwägung gezogen.

- In der ersten Vorschau wurden diese Assemblys ausgeschlossen, es sei denn, die Typen in den einzelnen Assemblys werden explizit vom Anwendungscode verwendet. Im folgenden finden Sie eine Problem Umgehung:[http://lists.ximian.com/pipermail/mo...il/009798.html](http://lists.ximian.com/pipermail/monodroid/2012-April/009798.html)

## <a name="projects-built-with-aotllvm-crash-on-x86-devices"></a>Mit AOT + llvm-abstürzen auf x86-Geräten erstellter Projekte

Beim Bereitstellen einer mit [AOT + llvm](~/android/deploy-test/release-prep/index.md) auf x86-basierten Geräten erstellten APP wird möglicherweise eine Ausnahme Fehlermeldung ähnlich der folgenden angezeigt:

```shell
Assertion: should not be reached at /Users/.../external/mono/mono/mini/tramp-x86.c:124
Fatal signal 6 (SIGABRT), code -6 in tid 4051 (amarin.bug56111)
```

Dies ist ein bekanntes Problem, das in [56111](https://bugzilla.xamarin.com/show_bug.cgi?id=56111)gemeldet wird. Die Problem Umgehung besteht darin, llvm zu deaktivieren.
