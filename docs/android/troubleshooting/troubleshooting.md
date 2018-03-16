---
title: Hinweise zur Fehlerbehebung
ms.topic: article
ms.prod: xamarin
ms.assetid: 56137ACA-4811-B312-6860-E16D0FA123F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/15/2018
ms.openlocfilehash: 015fff63c612c3acf29681b90c1e945c5e460034
ms.sourcegitcommit: 028936cd2fe547963c1cf82343c3ee16f658089a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="troubleshooting-tips"></a>Hinweise zur Fehlerbehebung


## <a name="getting-diagnostic-information"></a>Abrufen von Diagnoseinformationen

Xamarin.Android wurde einige stellen müssen, um verschiedene Fehlern nachzuverfolgen und zu suchen.
Dazu gehören:

1.  MSBuild-Diagnoseausgabe.
2.  Bereitstellungsprotokolle des Geräts.
3.  Android-Debugausgabe Protokoll.


<a name="Diagnostic_MSBuild_Output" />

## <a name="diagnostic-msbuild-output"></a>MSBuild-Diagnoseausgabe

Diagnose MSBuild enthält möglicherweise weitere Informationen im Zusammenhang mit der Erstellung des Pakets und möglicherweise einige Bereitstellungsinformationen Paket enthalten.

So aktivieren Sie die Diagnoseausgabe von MSBuild in Visual Studio

1.  Klicken Sie auf **Extras > Optionen...**
2.  Wählen Sie in der linken Struktur **Projekte und Projektmappen > Erstellen und ausführen**
3.  Legen Sie im rechten Bereich der MSBuild Build Ausgabe Ausführlichkeit-Dropdownliste auf Diagnose
4.  Klicken Sie auf **OK**.
5.  Bereinigen Sie Ihr Paket, und erstellen Sie es erneut.
6.  Diagnoseausgabe wird in den Ausgabebereich angezeigt.


So aktivieren Sie die Diagnoseausgabe von MSBuild in Visual Studio für Mac OS/X:

1.  Klicken Sie auf **Visual Studio für Mac > Einstellungen...**
2.  Wählen Sie in der linken Struktur **Projekte > Erstellen**
3.  Legen Sie im rechten Bereich die Ausführlichkeit des Protokolls Dropdown-Menü auf Diagnose
4.  Klicken Sie auf **OK**.
5.  Starten Sie Visual Studio für Mac neu.
6.  Bereinigen Sie Ihr Paket, und erstellen Sie es erneut.
7.  Diagnoseausgabe ist innerhalb der Fehler Pad sichtbar (**Ansicht > füllt > Fehler** ), durch Klicken auf die Schaltfläche Ausgabe erstellen.




## <a name="device-deployment-logs"></a>Bereitstellungsprotokolle für Gerät

Gerät Bereitstellung in Visual Studio-Protokollierung:

1.  **Extras > Optionen...**>
2.  Wählen Sie in der linken Struktur **Xamarin > Android-Einstellungen**
3.  Aktivieren Sie im rechten Bereich die [X] **Erweiterung Debug-Protokollierung (schreibt monodroid.log auf Ihrem Desktop)** Kontrollkästchen.
4.  Protokollmeldungen werden in der Datei monodroid.log auf Ihrem Desktop geschrieben.


Visual Studio für Mac schreibt immer Bereitstellungsprotokolle Gerät. Suchen sie nach ist etwas schwieriger. eine *AndroidUtils* Protokolldatei wird erstellt für jede Tag + Zeit, die eine Bereitstellung, z. B. erfolgt: **AndroidTools-2012-10-24_12-35-45.log**.

-  Unter Windows, Protokolldateien geschrieben werden `%LOCALAPPDATA%\XamarinStudio-{VERSION}\Logs`.
-  Unter OS X-Protokolldateien geschrieben werden `$HOME/Library/Logs/XamarinStudio-{VERSION}`.




## <a name="android-debug-log-output"></a>Die Ausgabe der Android-Debug-Protokolls

Android schreibt viele Nachrichten an die [Android Debugprotokoll](~/android/deploy-test/debugging/android-debug-log.md).
Xamarin.Android verwendet Android Systemeigenschaften, um die Generierung von Android Debugprotokoll zusätzliche Nachrichten steuern. Android-Systemeigenschaften können festgelegt werden, über die *Setprop* Befehl innerhalb der [Android Debug Bridge (Adb)](http://developer.android.com/guide/developing/tools/adb.html):

```shell
adb shell setprop PROPERTY_NAME PROPERTY_VALUE
```

Systemeigenschaften werden während des Prozessstarts gelesen, und somit müssen entweder festgelegt werden, bevor die Anwendung gestartet wird oder die Anwendung muss neu gestartet werden, nachdem die Systemeigenschaften geändert werden.



### <a name="xamarinandroid-system-properties"></a>Xamarin.Android-Systemeigenschaften

Xamarin.Android unterstützt die folgenden Systemeigenschaften:

-   *Debug.Mono.Debug*: Wenn eine nicht leere Zeichenfolge ist, entspricht dies dem `*mono-debug*`.

-   *Debug.Mono.env*: eine Pipe getrennte ("*|*") Liste von Umgebungsvariablen so exportieren Sie während des Anwendungsstarts, *vor* Mono initialisiert wurde. Dies ermöglicht das Festlegen von Umgebungsvariablen Steuerelement Mono / Protokollierung.

    - *Hinweis*: weil der Wert ist "*|*"-getrennt, muss der Wert ein höheres Niveau zitieren haben als die \` *Adb Shell* \` Befehl entfernt einen der Satz von Anführungszeichen.

    - *Hinweis*: Android systemeigenschaftswerte können nicht länger als 92 Zeichen lang sein.

    - Beispiel:

            adb shell setprop debug.mono.env "'MONO_LOG_LEVEL=info|MONO_LOG_MASK=asm'"

-   *Debug.Mono.log*: eine durch Trennzeichen getrennte ("*,*") Liste der Komponenten, die zusätzliche Nachrichten an das Android Debugprotokoll gedruckt werden sollen. Standardmäßig ist ' Nothing ' festgelegt werden. Komponenten gehören:

    -   *alle*: alle Nachrichten zu drucken
    -   *GC*: Fehlermeldungen im Zusammenhang mit dem Druckserver GC.
    -   *Gref*: Drucken (schwache, globale) Verweis belegen und freigeben Nachrichten.
    -   *Lref*: Verweis auf lokale belegen und freigeben Nachrichten zu drucken.

    *Hinweis*: Hierbei handelt es sich *extrem* ausführlich. Aktivieren Sie, sofern Sie sich wirklich um müssen nicht.

-   *Debug.Mono.Trace*: ermöglicht das Festlegen der [Mono - Ablaufverfolgung](http://docs.go-mono.com/?link=man%3amono(1)) `=PROPERTY_VALUE` Einstellung.



## <a name="xamarinandroid-cannot-resolve-systemvaluetuple"></a>Xamarin.Android kann nicht System.ValueTuple aufgelöst werden.

Dieser Fehler tritt aufgrund einer Inkompatibilität mit Visual Studio.

- **Visual Studio 2017 Update 1** (15.1 oder eine ältere Version) ist nur kompatibel mit der **System.ValueTuple NuGet 4.3.0** (oder älter).

- **Visual Studio 2017 Update 2** (Version 15.2 oder höher) ist nur kompatibel mit der **System.ValueTuple NuGet 4.3.1** (oder höher).

Wählen Sie die richtige System.ValueTuple NuGet-Visual Studio-2017 Installationsumfang entspricht.


## <a name="gc-messages"></a>GC-Nachrichten

GC-komponentenmeldungen können durch Festlegen der debug.mono.log-System-Eigenschaft auf einen Wert, der gc verfügt angezeigt werden.

GC-Nachrichten werden generiert, wenn der globale Katalogserver ausgeführt wird und stellt Informationen bereit, über wie viele arbeiten das GC hat:

```shell
I/monodroid-gc(12331): GC cleanup summary: 81 objects tested - resurrecting 21.
```

Weitere GC-Informationen, z. B. Informationen zur zeitlichen Steuerung kann generiert werden, durch Festlegen der `MONO_LOG_LEVEL` Umgebungsvariablen `debug`:

```shell
adb shell setprop debug.mono.env MONO_LOG_LEVEL=debug
```

Dies führt zu (viele) zusätzliche Mono / Nachrichten, einschließlich dieser drei folgen:

```shell
D/Mono (15723): GC_BRIDGE num-objects 1 num_hash_entries 81226 sccs size 81223 init 0.00ms df1 285.36ms sort 38.56ms dfs2 50.04ms setup-cb 9.95ms free-data 106.54ms user-cb 20.12ms clenanup 0.05ms links 5523436/5523436/5523096/1 dfs passes 1104 6883/11046605
D/Mono (15723): GC_MINOR: (Nursery full) pause 2.01ms, total 287.45ms, bridge 225.60 promoted 0K major 325184K los 1816K
D/Mono ( 2073): GC_MAJOR: (user request) pause 2.17ms, total 2.47ms, bridge 28.77 major 576K/576K los 0K/16K
```

In der `GC_BRIDGE` Nachricht `num-objects` ist die Anzahl der Standortverknüpfungsbrücken-Objekte, die in dieser Phase in Betracht ziehen wird, und `num_hash_entries` ist die Anzahl der Objekte, die während dieses Aufrufs des Codes Bridge verarbeitet.

In der `GC_MINOR` und `GC_MAJOR` Nachrichten `total` ist die Zeitspanne, während die ganzen Welt angehalten wird (keine Threads ausgeführt werden), während `bridge` ist die Zeitdauer, die in der Bridge verarbeitet Code (der Java VM behandelt). Wird von die ganzen Welt *nicht* angehalten, während die Brücke Verarbeitung ausgeführt wird.

 *Im allgemeinen*, desto größer der Wert der `num_hash_entries`, die mehr Zeit, die die `bridge` Sammlungen dauert, und die größere der `total` aufgewendete Zeit gesammelt werden.



## <a name="global-reference-messages"></a>Globale Verweis Nachrichten

So aktivieren Sie die globalen Verweis Loggig (GREF) anmelden, die *debug.mono.log* Systemeigenschaft darf *Gref*, z. B.:

```shell
adb shell setprop debug.mono.log gref
```

Xamarin.Android sorgt Android globalen Verweise für Zuordnungen zwischen Java-Instanzen und die zugeordneten verwalteten Instanzen als wenn eine Java-Methode aufrufen, die eine Java-Instanz muss Java zur Verfügung gestellt wird.

Leider können Sie Android-Emulatoren nur 2000 globale Verweise zu einem Zeitpunkt vorhanden sein. Hardware verfügt über eine viel höhere Grenze von 52000 globale verweisen. Die untere Grenze kann problematisch sein, beim Ausführen von Anwendungen auf dem Emulator, zu wissen, *, in denen* der Instanz stammt, kann sehr nützlich sein.

 *Hinweis*: der globale Verweiszähler nur intern Xamarin.Android, und nicht (und kann nicht) globale Verweise aufgenommen durch andere systemeigenen Bibliotheken, die in den Prozess geladen wurden. Verwenden Sie den globalen Verweiszähler als Schätzung.

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

Es gibt vier Nachrichten folgen:

-  Globale Verweis erstellen: Dies sind die Zeilen, die mit beginnt *+ g +* , und stellt eine stapelüberwachung für den Codepfad der erstellende bereit.
-  Globale Verweis Zerstörung: Dies sind die Zeilen, die mit beginnt *- g-* , kann eine stapelüberwachung für das Freigeben von globalen Verweis Codepfad liefern. Wenn der globale Katalogserver die Gref freigegeben ist, wird keine stapelüberwachung bereitgestellt werden.
-  Schwache globale verweisen Erstellung: Dies sind die Zeilen, die mit beginnt *+ w +* .
-  Globale schwache verweisen Zerstörung: Dies sind Zeilen, die mit beginnt *-w-* .


In allen Nachrichten die *Grefc* Wert ist die Anzahl der globalen Verweise, die Xamarin.Android erstellt, während die *Grefwc* Wert ist die Anzahl der schwachen globalen Verweise, die Xamarin.Android erstellt hat. Die *behandeln* oder *Obj-Handle* Wert ist der Handlewert JNI und die Zeichen nach der "  */* " ist der Typ der Handlewert:   */l* für den lokalen Verweis */g* für globale Verweise und */w* für schwache globalen Verweise.

Im Rahmen des Prozesses GC globalen Verweise vorhanden sind (+ g +) werden in konvertiert schwache globalen Verweise (verursacht eine + w + und -g-), eine Java-Seite-GC wird ausgeschlossen, und der schwache Verweis für die globale wird dann überprüft, um festzustellen, ob diese gesammelt wurden. Wenn sie noch aktiv ist, wird eine neue Gref erstellt, um schwache Ref (+ g +, -w-), andernfalls der schwache Ref zerstört wird (-w).

## <a name="java-instance-is-created-and-wrapped-by-a-mcw"></a>Java-Instanz erstellt und von einem MCW umschlossen

```shell
I/monodroid-gref(27679): +g+ grefc 2211 gwrefc 0 obj-handle 0x4066df10/L -> new-handle 0x4066df10/L from ...
I/monodroid-gref(27679): handle 0x4066df10; key_handle 0x4066df10: Java Type: `android/graphics/drawable/TransitionDrawable`; MCW type: `Android.Graphics.Drawables.TransitionDrawable`
```

## <a name="a-gc-is-being-performed"></a>Eine GC wird ausgeführt...

```shell
I/monodroid-gref(27679): +w+ grefc 1953 gwrefc 259 obj-handle 0x4066df10/G -> new-handle 0xde68f95f/W from take_weak_global_ref_jni
I/monodroid-gref(27679): -g- grefc 1952 gwrefc 259 handle 0x4066df10/G from take_weak_global_ref_jni
```

## <a name="object-is-still-alive-as-handle--null"></a>Objekt ist noch aktiv ist, als Handle! = Null
## <a name="wref-turned-back-into-a-gref"></a>Wref wieder in einen Gref umgewandelt.

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x4066df10
I/monodroid-gref(27679): +g+ grefc 1930 gwrefc 39 obj-handle 0xde68f95f/W -> new-handle 0x4066df10/G from take_global_ref_jni
I/monodroid-gref(27679): -w- grefc 1930 gwrefc 38 handle 0xde68f95f/W from take_global_ref_jni
```

## <a name="object-is-dead-as-handle--null"></a>Objekt ist deaktiviert, als Handle == Null
## <a name="wref-is-freed-no-new-gref-created"></a>Wref wird freigegeben, werden keine neuen Gref erstellt

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x0
I/monodroid-gref(27679): -w- grefc 1914 gwrefc 296 handle 0xde68f95f/W from take_global_ref_jni
```

Es wird eine "interessanten" Falten: für Ziele, die unter Android vor 4.0 ausgeführt werden, die Gref-Wert ist gleich die Adresse des Java-Objekts im Arbeitsspeicher für die Android-Laufzeit. (D. h. der globale Katalogserver ist ein nicht verschieben, konservative, Sammlung, und sie direkte Verweise auf diese Objekte übergeben wird.) Daher nach einem + g + + w +, - g-, + g +, - w-Sequenz, die resultierenden Gref müssen den gleichen Wert wie der ursprüngliche Gref-Wert. Auf diese Weise Grepping in den Protokollen recht einfach.

Android 4.0 ist jedoch einen gleitenden Sammler hat und nicht mehr übergibt direkte Verweise auf Android-Laufzeit VM-Objekte. Folglich nach einer + g + + w +, - g-, + g +, - w-Sequenz, die Gref Wert *weicht*. Wenn das Objekt mehrere globale Kataloge überdauert, wechseln sie durch mehrere Gref-Werte, wodurch die erschwert, um zu bestimmen, in dem eine Instanz von tatsächlich zugeordnet wurde.

### <a name="querying-programmatically"></a>Programmgesteuert Abfragen

Sie können die GREF und die WREF Abfragen, durch Abfragen der `JniRuntime` Objekt.

`Java.Interop.JniRuntime.CurrentRuntime.GlobalReferenceCount` -Globalen Verweiszähler dieser Planergruppe

`Java.Interop.JniRuntime.CurrentRuntime.WeakGlobalReferenceCount` -Die Anzahl von schwache Verweise



## <a name="offline-activation"></a>Offline-Aktivierung

Wenn Sie nicht Xamarin.Android unter Windows zu aktivieren oder die Vollversion von Xamarin.Android auf Mac OS X installiert sind, Sie bitte die [Offline Aktivierung](~/android/get-started/installation/index.md) Seite.



## <a name="cant-upgrade-to-indiebusiness-from-trial-account"></a>Kann nicht auf Indie/Business-Testversion Konto aktualisieren

Wenn Sie kürzlich Xamarin.Android erworben und zuvor eine Testversion Xamarin.Android begonnen hat, müssen Sie möglicherweise die folgenden Schritte zum Abrufen dieser Lizenz Änderung von Visual Studio für Mac oder Visual Studio übernommen.

-  Schließen Sie Visual Studio für Mac/Visual Studio
-  Entfernen Sie alle Dateien aus ~/Library/MonoAndroid auf Mac oder %PROGRAMDATA%\Mono Android\License\ für Windows
-  Öffnen Sie Visual Studio erneut für Mac/Visual Studio, und beim Erstellen eines Projekts Xamarin.Android


Dies sollten Sie einsatzbereit abrufen. Wenn weiterhin Probleme bestehen, sollten Sie versuchen zu einer [Offline Aktivierung](~/android/get-started/installation/index.md) zum Abschließen der Aktivierung von Ihrer Arbeitsstation.



## <a name="receiving-activation-incomplete-error-message"></a>Empfangen von "Aktivierung unvollständige Fehlermeldung

Dieses Problem kann auftreten, wenn Xamarin.Android für Visual Studio verwenden. Um dieses Problem zu beheben, senden Sie die Protokolle aus den folgenden Ort hinzu  *contact@xamarin.com* .

-  Protokollspeicherort: **%LocalAppData%\\Xamarin\\Protokolle**




## <a name="receiving-error-retrieving-update-information-error-message"></a>Fehlermeldung "Fehler beim Abrufen von Informationen zum Update"

Von Zeit zu Zeit wird ein Update mit diesen folgenden Fehler fehl, dies ist häufig bei der Suche nach Updates:

Ein Großteil der Zeit, dieser Fehler kann durch Protokollierung aus Ihrem Konto Xamarin aufgelöst werden, und klicken Sie dann wieder Protokollierung.

Um dies zu erreichen, suchen Sie der Plattform Ihrer Wahl unter und führen Sie die Schritte:

**Unter Mac:**
1. Öffnen Sie Visual Studio für Mac
2. Wählen Sie Visual Studio für Mac > Konto...
3. Klicken Sie auf Abmelden
4. Klicken Sie auf Anmelden
5. Geben Sie Ihre Anmeldeinformationen
6. Nach Updates suchen

**Auf PCs, die mit Visual Studio:**
1. Öffnen Sie Visual Studio.
2. Wählen Sie Extras > Xamarin-Konto
3. Klicken Sie auf Abmelden
4. Klicken Sie auf Anmelden
5. Geben Sie Ihre Anmeldeinformationen
6. Nach Updates suchen

Wenn diese Fehlermeldung angezeigt weiterhin, e-Mail-  **contact@xamarin.com** .




## <a name="android-debug-logs"></a>Android Debugprotokolle

Die [Android Debugprotokolle](~/android/deploy-test/debugging/android-debug-log.md) vorsehen zusätzlichen Kontext zur Laufzeitfehler gemeldet wird.



## <a name="floating-point-performance-is-terrible"></a>Gleitkomma-Leistung ist sehr schlecht!

Sie können auch "runs Meine app 10-Mal schnellere mit Debugbuild als mit dem Releasebuild!"

Xamarin.Android unterstützt mehrere Geräte ABIs: *Armeabi*, *Armeabi v7a*, und *X86*. Gerät ABIs können angegeben werden, in **Projekteigenschaften > Registerkarte "Anwendung" > unterstützt Architekturen**.

Debug-Builds verwenden Sie ein Android-Paket enthält alle ABIs, und verwenden daher wird die schnellste ABI für das Zielgerät.

Releasebuilds schließt nur die ABIs auf der Registerkarte "Projekteigenschaften" ausgewählt. Mehrere kann ausgewählt werden.

*Armeabi* ist die Standardeinstellung ABI und die umfangreichste Gerät unterstützt. *Jedoch*, Armeabi unterstützt keine Multi-CPU-Geräte und Hardware Gleitkommazahlen Amont anderem. Daher verwenden Runtime Version Armeabi apps werden auf einem einzigen Kern gebunden werden und eine Soft-Float-Implementierung verwenden. Beide Fälle können zu einer erheblich langsameren Leistung für Ihre app beitragen.

Wenn Ihre app ordentliche Gleitkomma Leistung (z. B. Spiele) erfordert, sollten Sie aktivieren die *Armeabi v7a* ABI. Sie möchten nur unterstützen die *Armeabi v7a* Common Language Runtime, obwohl dies bedeutet, dass ältere Geräte, die nur unterstützen *Armeabi* wird in der Lage, Ihre app auszuführen.



## <a name="could-not-locate-android-sdk"></a>Android SDK konnte nicht gefunden werden.

Es sind 2 Downloads von Google Android-SDK für Windows verfügbar.
Wenn Sie das Installationsprogramm .exe auswählen, wird es Registrierungsschlüssel geschrieben, die Xamarin.Android anweisen, in dem er installiert wurde. Wenn Sie die ZIP-Datei und Entpacken Sie es selbst, kennt Xamarin.Android nicht, wo das SDK gesucht. Ersichtlich Xamarin.Android ist, in dem das SDK in Visual Studio, navigieren Sie zu **Extras > Optionen > Xamarin > Android-Einstellungen**:

[![Android SDK-Verzeichnis in den Xamarin Android-Einstellungen](troubleshooting-images/01.png)](troubleshooting-images/01.png#lightbox)



## <a name="ide-does-not-display-target-device"></a>Zielgerät IDE nicht angezeigt.

Manchmal versuchen Sie zum Bereitstellen der anwendungskennworts auf einem Gerät, aber das Gerät, das bereitgestellt wird, ist nicht im Dialogfeld "Gerät auswählen" angezeigt werden soll. Dies kann geschehen, wenn die Android Debug Bridge entscheidet Urlaub gehen an.

Um dieses Problem zu diagnostizieren, suchen die [Adb Programm](~/android/deploy-test/debugging/android-debug-log.md), führen Sie dann:

```shell
adb devices
```

Wenn Ihr Gerät nicht vorhanden ist, müssen Sie den Android Debug Bridge-Server neu starten, damit, dass Ihr Gerät gefunden werden kann:

```shell
adb kill-server
adb start-server
```

HTC-Sync-Software kann verhindern, dass **Adb Start-Server** nicht mehr ordnungsgemäß funktionieren. Wenn die **Adb Start-Server** Befehl nicht drucken Sie den Port, er wird gestartet, auf, beenden Sie die Synchronisierung HTC-Software und wiederholen Sie den Neustart des Servers Adb.


## <a name="the-specified-task-executable-keytool-could-not-be-run"></a>Die angegebene Aufgabe ausführbare Datei "Keytool" konnte nicht ausgeführt werden

Dies bedeutet, dass der Pfad nicht das Verzeichnis enthält, auf dem sich das Java SDK-Bin-Verzeichnis befindet. Überprüfen Sie, dass Sie diese Schritte aus der [Installation](~/android/get-started/installation/index.md) Handbuch.


## <a name="monodroidexe-or-aresgenexe-exited-with-code-1"></a>monodroid.exe oder aresgen.exe beendet werden, mit dem Code 1

Wählen Sie zum Debuggen dieses Problems, wechseln Sie in Visual Studio und ändern den Ausführlichkeitsgrad MSBuild dazu: **Tools > Optionen > Projekt** und **Lösungen > Erstellen** und **ausführen > Ausführlichkeit der MSBuild-Projekt erstellen Ausgabe** und legen Sie diesen Wert auf **Normal**.

Neu erstellen Sie, und überprüfen Sie die Visual Studio-Ausgabebereich, der den vollständigen Fehler enthalten sollte.

## <a name="there-is-not-enough-storage-space-on-the-device-to-deploy-the-package"></a>Es ist nicht genügend Speicherplatz auf dem Gerät zur Bereitstellung des Pakets

Dies tritt auf, wenn Sie den Emulator über innerhalb von Visual Studio starten nicht. Wenn den Emulator außerhalb von Visual Studio gestartet wird, müssen Sie übergeben die `-partition-size 512` Optionen, z. B.

```shell
emulator -partition-size 512 -avd MonoDroid
```

Stellen Sie sicher, verwenden Sie z. B. den Namen richtig Simulator [den Namen, die Sie beim Konfigurieren des Simulators verwendet](~/android/get-started/installation/windows.md#device).


## <a name="installfailedinvalidapk-when-installing-a-package"></a>Installieren Sie\_Fehler\_ungültige\_APK beim Installieren eines Pakets

Android Paketnamen *müssen* enthalten einen Punkt ("*.*"). Bearbeiten Sie Ihre Paketname, sodass es einen Punkt enthält.

-   Within Visual Studio:
    -   Klicken Sie mit der mit der rechten Maustaste auf Ihr Projekt > Eigenschaften
    -   Klicken Sie auf der Registerkarte "Android-Manifest", auf der linken Seite.
    -   Aktualisieren Sie das Feld für den Paketpfad ein.
        -   Wenn die Meldung &ldquo;keine AndroidManifest.xml gefunden. Klicken Sie auf diese Option, um eine Anforderung hinzuzufügen. &rdquo;, klicken Sie auf den Link, und aktualisieren Sie dann das Feld für den Paketpfad.
-   In Visual Studio für Mac:
    -   Klicken Sie mit der mit der rechten Maustaste auf Ihr Projekt > Optionen.
    -   Navigieren Sie zu dem Build / Android Anwendungsabschnitt.
    -   Ändern Sie das Paket Name-Feld enthält eine '.'.




## <a name="installfailedmissingsharedlibrary-when-installing-a-package"></a>Installieren Sie\_Fehler\_MISSING\_SHARED\_Bibliothek beim Installieren eines Pakets

Eine "freigegebene Bibliothek" in diesem Kontext ist *nicht* eine systemeigene freigegebene Bibliothek (*libfoo.so*) Datei; es wird stattdessen eine Bibliothek, die auf dem Zielgerät, z. B. Google Maps separat installiert werden muss.

Die Android-Paket gibt an, welche freigegebenen Bibliotheken mit erforderlich sind die `<uses-library/>` Element. Wenn eine *erforderlichen* Bibliothek ist nicht auf dem Zielgerät vorhanden (z. B. `//uses-library/@android:required` ist *"true"*, dies ist die Standardeinstellung), Paketinstallation nicht ausgeführt werden, mit *installieren\_ Fehler bei\_MISSING\_SHARED\_Bibliothek*.

Um zu bestimmen, welche freigegebenen Bibliotheken erforderlich sind, zeigen die *generiert*
**AndroidManifest.xml** Datei (z. B. **Obj\\Debuggen\\android \\AndroidManifest.xml**) und suchen Sie nach der `<uses-library/>` Elemente. `<uses-library/>` Elemente können manuell hinzugefügt werden, in Ihrem Projekts **Eigenschaften\\AndroidManifest.xml** Datei und über die [UsesLibraryAttribute benutzerdefiniertes Attribut](https://developer.xamarin.com/api/type/Android.App.UsesLibraryAttribute/).

Z. B. Hinzufügen eines Assemblyverweises zu *Mono.Android.GoogleMaps.dll* implizit fügen eine `<uses-library/>` für die Google Maps freigegebene Bibliothek.



## <a name="installfailedupdateincompatible-when-installing-a-package"></a>Installieren Sie\_Fehler\_UPDATE\_INKOMPATIBEL beim Installieren eines Pakets

Android Pakete haben drei Anforderungen:

-   Sie müssen enthalten einen "." (Siehe vorherige Eintrag)
-   Sie benötigen eine eindeutige Zeichenfolge Paketname (daher die Reverse-Tld Konvention wird im Android-app-Namen, z. B. com.android.chrome für die app Chrome angezeigt)
-   Wenn Sie Pakete zu aktualisieren, muss das Paket den gleichen Anmeldeschlüssel haben.

Stellen Sie daher vor diesem Szenario:

1.  Sie erstellen und Bereitstellen Ihrer app als eine Debug-app
2.  Sie ändern den Signaturschlüssel, z. B. in als Freigabe-app verwenden (oder weil Sie nicht mögen, die standardmäßig bereitgestellten Debuggen Signaturschlüssel)
3.  Ihre app zu installieren, ohne ihn zuerst entfernen, z. B. Debuggen > Starten ohne Debugging in Visual Studio


In diesem Fall fehl Paketinstallation mit einer Installation\_Fehler\_UPDATE\_INKOMPATIBLE Fehler Schlüssel, da Sie der Paketnamen nicht geändert haben, während der Anmeldung haben. Die [Android Debugprotokoll](~/android/deploy-test/debugging/android-debug-log.md) enthält auch eine Meldung ähnlich:

```shell
E/PackageManager(  146): Package [PackageName] signatures do not match the previously installed version; ignoring!
```

Um diesen Fehler zu beheben, entfernen Sie die Anwendung von Ihrem Gerät vor der erneuten Installation.


## <a name="installfaileduidchanged-when-installing-a-package"></a>Installieren Sie\_Fehler\_UID\_geändert, wenn ein Paket installieren

Wenn ein Android-Paket installiert ist, wird ihm zugewiesenen eine *Benutzer-Id* (UID).
*In einigen Fällen*, die Installation fehl Gründen derzeit unbekannt, bei der Installation über eine bereits installierte app, mit `INSTALL_FAILED_UID_CHANGED`:

```shell
ERROR [2015-03-23 11:19:01Z]: ANDROID: Deployment failed
Mono.AndroidTools.InstallFailedException: Failure [INSTALL_FAILED_UID_CHANGED]
   at Mono.AndroidTools.Internal.AdbOutputParsing.CheckInstallSuccess(String output, String packageName)
   at Mono.AndroidTools.AndroidDevice.<>c__DisplayClass2c.<InstallPackage>b__2b(Task`1 t)
   at System.Threading.Tasks.ContinuationTaskFromResultTask`1.InnerInvoke()
   at System.Threading.Tasks.Task.Execute()
```

Zum Umgehen dieses Problems *vollständig deinstallieren* Android-Paket, durch die Installation der app aus der Android-Ziel-GUI oder über `adb`:

```shell
$ adb uninstall @PACKAGE_NAME@
```

**Verwenden Sie keine** `adb uninstall -k`, wie dies wird *beibehalten* Anwendungsdaten verwendet werden, und daher nur die in Konflikt stehenden UID auf dem Zielgerät beibehalten.



## <a name="release-apps-fail-to-launch-on-device"></a>Release-apps nicht auf Gerät starten

Die Ausgabe Android Debugprotokoll enthält eine Meldung ähnlich:

```shell
D/AndroidRuntime( 1710): Shutting down VM
W/dalvikvm( 1710): threadid=1: thread exiting with uncaught exception (group=0xb412f180)
E/AndroidRuntime( 1710): FATAL EXCEPTION: main
E/AndroidRuntime( 1710): java.lang.UnsatisfiedLinkError: Couldn't load monodroid: findLibrary returned null
E/AndroidRuntime( 1710):        at java.lang.Runtime.loadLibrary(Runtime.java:365)
```

Wenn dies der Fall ist, gibt es zwei mögliche Gründe:

1.  Die .apk stellt kein ABI bereit, die das Zielgerät unterstützt.
    Beispielsweise enthält die .apk nur Armeabi v7a Binärdateien, und das Zielgerät unterstützt nur Armeabi.

2.  Ein [Android Fehler](http://code.google.com/p/android/issues/detail?id=21670). Wenn dies der Fall ist, deinstallieren Sie die app, cross Finger und installieren Sie die app.

(1) zu beheben, bearbeiten Sie die Projekteigenschaften/Optionen und [Hinzufügen von Unterstützung für die erforderlichen ABI zur Liste der unterstützten ABIs](~/android/app-fundamentals/cpu-architectures.md). Um zu bestimmen, welche ABI, die zum Hinzufügen, müssen Sie den folgenden Befehl aus Adb für Zielgerät ausgeführt:

```shell
adb shell getprop ro.product.cpu.abi
adb shell getprop ro.product.cpu.abi2
```

Die Ausgabe enthält die primären (und optional sekundäres) ABIs.

```shell
$ adb shell getprop | grep ro.product.cpu
[ro.product.cpu.abi2]: [armeabi]
[ro.product.cpu.abi]: [armeabi-v7a]
```

## <a name="the-outpath-property-is-not-set-for-project-ldquomyappcsprojrdquo"></a>Die OutPath-Eigenschaft nicht festgelegt ist, für das Projekt &ldquo;MyApp.csproj&rdquo;

Dies im Allgemeinen bedeutet, dass Sie eine HP-Computer und die Umgebungsvariable &ldquo;Plattform&rdquo; etwas wie MCD oder HPD festgelegt wurde. Dies steht in Konflikt mit der MSBuild-Plattform-Eigenschaft, die in der Regel auf &ldquo;Any CPU&rdquo; oder &ldquo;X86&rdquo;. Sie müssen diese Umgebungsvariable vom Computer entfernen, damit MSBuild funktionsfähig:

-   In der Systemsteuerung > System > Erweitert > Umgebungsvariablen

Starten Sie Visual Studio oder Visual Studio für Mac, und versuchen Sie neu erstellen. Dinge sollte jetzt wie erwartet funktionieren.

## <a name="javalangclasscastexception-monoandroidruntimejavaobject-cannot-be-cast-to"></a>java.lang.ClassCastException: mono.android.runtime.JavaObject cannot be cast to...

Xamarin.Android 4.x nicht ordnungsgemäß Marshallen von geschachtelten generischen Typen ordnungsgemäß. Betrachten Sie beispielsweise den folgenden C\# code mit [SimpleExpandableListAdapter](https://developer.xamarin.com/api/type/Android.Widget.SimpleExpandableListAdapter/):


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


Das Problem ist, dass Xamarin.Android falsch geschachtelte generische Typen marshallt. Die `List<IDictionary<string, object>>` gemarshallten ist ein [java.lang.ArrrayList](https://developer.xamarin.com/api/type/Java.Util.ArrayList/), aber die `ArrayList` ist enthält `mono.android.runtime.JavaObject` Instanzen (die Verweisdaten der `Dictionary<string, object>` Instanzen) anstelle von etwas, das implementiert[java.util.Map](https://developer.xamarin.com/api/type/Java.Util.IMap/), wodurch in der folgenden Ausnahme:

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

Die problemumgehung ist die Verwendung der bereitgestellten [Java Auflistungstypen](~/android/internals/api-design.md) anstelle von der `System.Collections.Generic` -Typen für die &ldquo;innere&rdquo; Typen. Dies führt zu entsprechenden Java-Typen, wenn die Instanzen zu marshallen. (Der folgende Code ist komplizierter als erforderlich, um Gref Lebensdauer zu reduzieren. Zum Ändern des ursprünglichen Codes über vereinfacht werden `s/List/JavaList/g` und `s/Dictionary/JavaDictionary/g` Wenn Gref Lebensdauer befürchten sind.)

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

[Dies wird in einer zukünftigen Version behoben](https://bugzilla.xamarin.com/show_bug.cgi?id=5401).


## <a name="unexpected-nullreferenceexceptions"></a>Unerwarteter NullReferenceExceptions

Gelegentlich die [Android Debugprotokoll](~/android/deploy-test/debugging/android-debug-log.md) wird NullReferenceExceptions erwähnen, &ldquo;nicht auftreten,&rdquo; oder stammen aus Mono für Android Laufzeitcode kurz vor der app Dies:

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

Dies kann auftreten, wenn die Android-Laufzeit entscheidet sich den Prozess abgebrochen werden; dies kann eine beliebige Anzahl von Gründen, einschließlich des Ziels GREF Limit erreichen oder etwas geschehen &ldquo;falsche&rdquo; mit JNI.

Um festzustellen, ob dies der Fall ist, überprüfen Sie das Android Debugprotokoll für eine Nachricht von Ihrem Prozess ähnelt:

```shell
E/dalvikvm(  123): VM aborting
```


## <a name="abort-due-to-global-reference-exhaustion"></a>Abbruch aufgrund einer globalen Verweis Ausschöpfung des Speichers

Android-Runtime-JNI-Ebene unterstützt nur eine begrenzte Anzahl von JNI-Objektverweise zeitlich zu einem bestimmten Zeitpunkt gültig sein. Wenn dieses Limit überschritten wird, unterbrechen Dinge.

Die GREF (*globalen Verweis*) "Limit" ist 2000 Verweise im Emulator und ~ 52000 verweist auf Hardware.

Sie wissen, dass Sie zu viele GREFs zu erstellen, wenn Sie, wie diese Nachrichten, in der Android Debugprotokoll sehen starten:

```shell
D/dalvikvm(  602): GREF has increased to 1801
```

Wenn das GREF-Limit erreicht ist, wird eine Meldung wie die folgende ausgegeben:

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


Im obigen Beispiel (stammt, die übrigens [bug 685215](https://bugzilla.novell.com/show_bug.cgi?id=685215)) das Problem ist, die zu viele Android.Graphics.Point Instanzen erstellt werden, finden Sie unter [Kommentar \#2](https://bugzilla.novell.com/show_bug.cgi?id=685215#c2) für eine Liste der Updates für diesen bestimmten Fehler.

In der Regel eine nützliche Lösung ist gefunden, welcher Typ zu viele Instanzen verfügt zugeordnet &ndash; Android.Graphics.Point im obigen Dump &ndash; später feststellen, wo sie in Ihren Quellcode und verwerfen Sie sie entsprechend erstellt werden (so, dass ihre Java-Object Lebensdauer verkürzt wird). Dies ist nicht immer geeignete (\#685215 ist Multithread, wodurch die triviale Lösung den Dispose-Aufruf vermieden werden), aber Erstes sollten Sie berücksichtigen.

Sie können [GREF Protokollierung](~/android/troubleshooting/index.md) angezeigt, wenn GREFs erstellt werden und wie viele vorhanden sind.


## <a name="abort-due-to-jni-type-mismatch"></a>Abbruch aufgrund einer JNI-Typenkonflikt

Wenn Sie Hand-JNI-Code ein, ist es möglich, dass ordnungsgemäß, z. B. mit den Typen übereinstimmen wird nicht, wenn Sie versuchen, aufzurufen `java.lang.Runnable.run` für einen Typ, der nicht implementiert `java.lang.Runnable`. In diesem Fall wird eine Meldung in das Android Debugprotokoll etwa folgendermaßen:

```shell
W/dalvikvm( 123): JNI WARNING: can't call Ljava/Type;;.method on instance of Lanother/java/Type;
W/dalvikvm( 123):              in Lmono/java/lang/RunnableImplementor;.n_run:()V (CallVoidMethodA)
...
E/dalvikvm( 123): VM aborting
```

## <a name="dynamic-code-support"></a>Unterstützung für Dynamic-Code

### <a name="dynamic-code-does-not-compile"></a>Dynamische Code kompiliert nicht

Verwenden von C\# dynamisch in Ihrer Anwendung oder eine Bibliothek, müssen Sie "System.Core.dll", Microsoft.CSharp.dll und Mono.CSharp.dll zu Ihrem Projekt hinzufügen.

### <a name="in-release-build-missingmethodexception-occurs-for-dynamic-code-at-run-time"></a>Tritt auf in Releasebuild MissingMethodException für dynamische Code zur Laufzeit.

-   Es ist wahrscheinlich, dass das Anwendungsprojekt keine Verweise auf "System.Core.dll", Microsoft.CSharp.dll oder Mono.CSharp.dll verfügt. Stellen Sie sicher, dass diese Assemblys verwiesen werden.

    -   Bedenken Sie dynamischen Code immer Kosten. Wenn Sie effizientem Code benötigen, sollten Sie nicht mit dynamischem Code.

-   Diese Assemblys wurden in die erste Vorschau ausgeschlossen, es sei denn, die Typen in den jeweiligen Assemblys explizit von der Anwendung verwendet werden. Finden Sie hier für dieses Problem zu umgehen: [http://lists.ximian.com/pipermail/mo...il/009798.html](http://lists.ximian.com/pipermail/monodroid/2012-April/009798.html)


## <a name="projects-built-with-aotllvm-crash-on-x86-devices"></a>Mit AOT + LLVM Absturz auf X86 erstellte Projekte Geräte

Bei der Bereitstellung von Apps mit [AOT + LLVM](~/android/deploy-test/release-prep/index.md) auf X86-basierten Geräten, möglicherweise eine Ausnahmefehlermeldung ähnlich der folgenden angezeigt:

```shell
Assertion: should not be reached at /Users/.../external/mono/mono/mini/tramp-x86.c:124
Fatal signal 6 (SIGABRT), code -6 in tid 4051 (amarin.bug56111)
```

Dies ist ein bekanntes Problem, wie Sie in [56111](https://bugzilla.xamarin.com/show_bug.cgi?id=56111). Die problemumgehung besteht darin LLVM deaktivieren.
