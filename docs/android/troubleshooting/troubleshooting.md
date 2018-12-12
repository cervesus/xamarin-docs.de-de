---
title: Hinweise zur Fehlerbehebung
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 56137ACA-4811-B312-6860-E16D0FA123F7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/15/2018
ms.openlocfilehash: ccf5d97ff553fd304c4a3af158085d490bb665b7
ms.sourcegitcommit: 2868c968f418cd7cc110f9664f3c3ffb6df1f9af
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2018
ms.locfileid: "53267598"
---
# <a name="troubleshooting-tips"></a>Hinweise zur Fehlerbehebung


## <a name="getting-diagnostic-information"></a>Abrufen von Diagnoseinformationen

Xamarin.Android hat einige Stellen, bei der nachverfolgung nach verschiedenen Fehlern zu suchen.
Dazu gehören:

1.  Diagnoseausgabe von MSBuild.
2.  Bereitstellungsprotokolle des Geräts.
3.  Protokollausgabe für Android-Debugprotokoll.


<a name="Diagnostic_MSBuild_Output" />

## <a name="diagnostic-msbuild-output"></a>Diagnoseausgabe von MSBuild

Diagnose MSBuild kann zusätzliche Informationen, die im Zusammenhang mit der Erstellung des Pakets enthalten und kann einige Bereitstellungsinformationen Paket enthalten.

So aktivieren Sie die Diagnoseausgabe von MSBuild in Visual Studio

1.  Klicken Sie auf **Tools > Optionen...**
2.  Wählen Sie in der linken Strukturansicht **Projekte und Projektmappen > Erstellen und ausführen**
3.  Legen Sie im rechten Bereich der Dropdown-Liste für MSBuild Build Ausgabe Ausführlichkeit auf Diagnose
4.  Klicken Sie auf **OK**.
5.  Bereinigen Sie Ihr Paket, und erstellen Sie es erneut.
6.  Diagnoseausgabe wird im Ausgabebereich angezeigt.


So aktivieren Sie die Diagnoseausgabe von MSBuild in Visual Studio für Mac/OS X:

1.  Klicken Sie auf **Visual Studio für Mac > Einstellungen...**
2.  Wählen Sie in der linken Strukturansicht **Projekte > Build**
3.  Klicken Sie im rechten Bereich legen Sie Dropdown-Diagnoseprotokolle
4.  Klicken Sie auf **OK**.
5.  Starten Sie Visual Studio für Mac neu.
6.  Bereinigen Sie Ihr Paket, und erstellen Sie es erneut.
7.  Die Diagnoseausgabe im Bereich "Fehler" angezeigt wird (**Ansicht > Pads > Fehler** ), indem Sie auf die Schaltfläche "Buildausgabe".




## <a name="device-deployment-logs"></a>Gerät-Bereitstellungsprotokolle

So aktivieren Sie die Bereitstellung geräteprotokollierung in Visual Studio:

1.  **Extras > Optionen...**>
2.  Wählen Sie in der linken Strukturansicht **Xamarin > Android-Einstellungen**
3.  Aktivieren Sie im rechten Bereich die [X] **Erweiterung Debugprotokollierung (schreibt "monodroid.log" auf Ihrem Desktop)** Kontrollkästchen.
4.  Protokollmeldungen werden in der Datei "monodroid.log" auf Ihrem Desktop geschrieben.


Visual Studio für Mac schreibt immer Bereitstellungsprotokolle des Geräts. Es ist etwas schwieriger, gefunden wurden; eine *AndroidUtils* Protokolldatei wird erstellt für jedes Tag und die Zeit, die eine Bereitstellung, z. B. erfolgt: **AndroidTools-2012-10-24_12-35-45.log**.

-  Auf Windows, werden in Protokolldateien geschrieben `%LOCALAPPDATA%\XamarinStudio-{VERSION}\Logs`.
-  Auf OS X, werden in Protokolldateien geschrieben `$HOME/Library/Logs/XamarinStudio-{VERSION}`.




## <a name="android-debug-log-output"></a>Protokollausgabe für Android-Debugprotokoll

Android schreiben viele Nachrichten an die [Android-Debugprotokoll](~/android/deploy-test/debugging/android-debug-log.md).
Xamarin.Android verwendet Android-Systemeigenschaften, um die Generierung von zusätzliche Nachrichten in das Android-Debug-Protokoll zu steuern. Android-Systemeigenschaften können festgelegt werden, über die *Setprop* Befehl innerhalb der [Android Debug Bridge (Adb)](http://developer.android.com/guide/developing/tools/adb.html):

```shell
adb shell setprop PROPERTY_NAME PROPERTY_VALUE
```

Systemeigenschaften werden während des Prozessstarts gelesen, und daher müssen entweder festgelegt werden, bevor die Anwendung wird gestartet, oder die Anwendung muss neu gestartet werden, nachdem die Systemeigenschaften geändert wurden.



### <a name="xamarinandroid-system-properties"></a>Xamarin.Android-Systemeigenschaften

Xamarin.Android unterstützt die folgenden Systemeigenschaften:

-   *Debug.Mono.Debug*: Wenn eine nicht leere Zeichenfolge ist, dies entspricht dem `*mono-debug*`.

-   *Debug.Mono.env*: Eine durch senkrechte Striche getrennte ("*|*") Liste von Umgebungsvariablen, die während des Anwendungsstarts exportieren *vor* Mono initialisiert wurde. Dies ermöglicht das Festlegen von Umgebungsvariablen, mono Protokollierung.

    - *Beachten Sie*: Da der Wert ist "*|*'-getrennt wird und der Wert muss enthalten eine zusätzliche Stufe der Anführungszeichen, als die \` *Adb-Shell* \` Befehl entfernt einen Satz von Anführungszeichen.

    - *Beachten Sie*: Android-System-Eigenschaftswerte können nicht länger als 92 Zeichen lang sein.

    - Beispiel:

            adb shell setprop debug.mono.env "'MONO_LOG_LEVEL=info|MONO_LOG_MASK=asm'"

-   *Debug.Mono.log*: Eine durch Trennzeichen getrennte ("*,*") Liste von Komponenten, die zusätzliche Nachrichten an das Android Debugprotokoll gedruckt werden sollen. In der Standardeinstellung ist "nothing" festgelegt. Komponenten umfassen Folgendes:

    -   *alle*: Alle Nachrichten zu drucken
    -   *GC*: Drucken Sie Nachrichten GC in Verbindung stehen.
    -   *Gref*: Drucken Sie die Belegung und Freigabe-Meldungen ("schwache", "global") zu verweisen.
    -   *Lref*: Drucken Sie belegen und freigeben Nachrichten von lokalen Verweis.

    *Beachten Sie*: Hierbei handelt es sich *extrem* ausführlich. Aktivieren Sie, wenn unbedingt erforderlich nicht.

-   *Debug.Mono.Trace*: Ermöglicht das Festlegen der [Mono--Trace](http://docs.go-mono.com/?link=man%3amono(1)) `=PROPERTY_VALUE` festlegen.



## <a name="xamarinandroid-cannot-resolve-systemvaluetuple"></a>Xamarin.Android kann nicht System.ValueTuple aufgelöst werden.

Dieser Fehler tritt aufgrund einer Inkompatibilität mit Visual Studio.

- **Visual Studio 2017 Update 1** (Version 15.1 oder früher) ist nur kompatibel mit der **System.ValueTuple NuGet 4.3.0** (und früher).

- **Visual Studio 2017 Update 2** (Version 15.2 oder höher) ist nur kompatibel mit der **System.ValueTuple NuGet 4.3.1** (oder höher).

Wählen Sie die richtige System.ValueTuple NuGet, der mit Ihrer Visual Studio 2017-Installation entspricht.


## <a name="gc-messages"></a>GC-Nachrichten

Komponentenmeldungen der GC können angezeigt werden, durch Festlegen der Systemeigenschaft debug.mono.log auf einen Wert, der gc enthält.

GC-Nachrichten werden generiert, wenn der Garbage Collector ausgeführt wird, und bietet Informationen, die dazu, wie lange funktioniert der Garbage Collector hat:

```shell
I/monodroid-gc(12331): GC cleanup summary: 81 objects tested - resurrecting 21.
```

Weitere GC-Informationen, z. B. Informationen zur zeitlichen Steuerung kann generiert werden, durch Festlegen der `MONO_LOG_LEVEL` Umgebungsvariable `debug`:

```shell
adb shell setprop debug.mono.env MONO_LOG_LEVEL=debug
```

Dies führt (viele) zusätzliche Mono-Meldungen, einschließlich dieser drei Auswirkungen:

```shell
D/Mono (15723): GC_BRIDGE num-objects 1 num_hash_entries 81226 sccs size 81223 init 0.00ms df1 285.36ms sort 38.56ms dfs2 50.04ms setup-cb 9.95ms free-data 106.54ms user-cb 20.12ms clenanup 0.05ms links 5523436/5523436/5523096/1 dfs passes 1104 6883/11046605
D/Mono (15723): GC_MINOR: (Nursery full) pause 2.01ms, total 287.45ms, bridge 225.60 promoted 0K major 325184K los 1816K
D/Mono ( 2073): GC_MAJOR: (user request) pause 2.17ms, total 2.47ms, bridge 28.77 major 576K/576K los 0K/16K
```

In der `GC_BRIDGE` Nachricht `num-objects` ist die Anzahl der Standortverknüpfungsbrücken-Objekte, die in dieser Phase in Betracht ziehen wird, und `num_hash_entries` ist die Anzahl von Objekten, die während dieses Aufrufs des Codes Bridge verarbeitet.

In der `GC_MINOR` und `GC_MAJOR` Nachrichten `total` ist die Zeitspanne, während die ganzen Welt angehalten ist (es sind keine Threads ausführen), während `bridge` ist die Menge an Zeit, die in der Bridge, die Verarbeitung von Code (der Java VM behandelt). Die Welt ist *nicht* angehalten, während bridgeverarbeitung ausgeführt wird.

 *Im allgemeinen*, je größer der Wert der `num_hash_entries`, die mehr Zeit, die die `bridge` Sammlungen werden in Anspruch nehmen, und je größer die `total` Zeit erfasst werden.



## <a name="global-reference-messages"></a>Globalen Verweis Nachrichten

So aktivieren Sie die globalen Verweis Loggig (GREF) anmelden, die *debug.mono.log* Systemeigenschaft darf *Gref*, z. B.:

```shell
adb shell setprop debug.mono.log gref
```

Xamarin.Android verwendet globale Android-Referenzen für die Zuordnungen zwischen Java-Instanzen und die zugehörigen verwalteten Instanzen als Geben Sie eine Java-Methode aufrufen, die eine Java-Instanz benötigt Java zur Verfügung gestellt werden.

Android-Emulatoren können leider nur global 2000-Verweise zu einem Zeitpunkt vorhanden sein. Hardware hat einen viel höheren Grenzwert von 52000 globale verweisen. Die untere Grenze kann problematisch sein, bei der Ausführung von Anwendungen auf dem Emulator zu wissen, *, in denen* der Instanz stammt, kann sehr nützlich sein.

 *Beachten Sie*: die Anzahl der globalen Verweis ist intern in Xamarin.Android und nicht (und nicht möglich) enthalten globale Verweise, die von anderen nativen Clientbibliotheken, die in den Prozess geladen entfernt. Verwenden Sie die globalen Verweis als Schätzung.

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

Es gibt vier Nachrichten Auswirkungen:

-  Erstellen des globalen: Hierbei handelt es sich um die Zeilen, die mit beginnen *+ g +* , und stellt eine stapelüberwachung für den Pfad für den erstellen.
-  Zerstörung von globalen Verweis: Hierbei handelt es sich um die Zeilen, die mit beginnen *- g-* , und möglicherweise eine stapelüberwachung des Codepfads, verwirft des globalen Verweis. Wenn der Garbage Collector die Gref freigegeben werden, wird keine stapelüberwachung bereitgestellt.
-  Globale schwachen Verweisen Erstellung: Hierbei handelt es sich um die Zeilen, die mit beginnen *+ w +* .
-  Globale schwachen Verweisen auf die Zerstörung: Hierbei handelt es sich um Linien, die mit beginnen *-w-* .


In der alle Nachrichten die *Grefc* Wert ist die Anzahl der globalen Verweise, die Xamarin.Android erstellt wurden, während die *Grefwc* Wert ist die Anzahl der schwachen globale verweisen, die Xamarin.Android erstellt wurde. Die *behandeln* oder *Obj-Handle* Wert ist der Wert des Handles JNI, und das Zeichen nach der " */*" ist der Typ des Handlewerts:   */l* für den lokalen Verweis */g* für globale-Verweise und */w* für globale weak-Verweise.

Im Rahmen der GC, globalen Verweise vorhanden sind (+ g und höher) werden in konvertiert globale weak-Verweise (verursachen, a + w + "und" - g-), eine GC Java-Komponente wird gestartet, und klicken Sie dann der globale schwachen aktiviert ist, um festzustellen, ob sie gesammelt wurden. Wenn es aktiv ist, wird eine neue Gref rund um die schwachen Verweis erstellt (+ g +, -w-), andernfalls wird der schwache Verweis zerstört (-w).

## <a name="java-instance-is-created-and-wrapped-by-a-mcw"></a>Java-Instanz erstellt und von einem MCW umschlossen

```shell
I/monodroid-gref(27679): +g+ grefc 2211 gwrefc 0 obj-handle 0x4066df10/L -> new-handle 0x4066df10/L from ...
I/monodroid-gref(27679): handle 0x4066df10; key_handle 0x4066df10: Java Type: `android/graphics/drawable/TransitionDrawable`; MCW type: `Android.Graphics.Drawables.TransitionDrawable`
```

## <a name="a-gc-is-being-performed"></a>Ein globaler Katalog wird ausgeführt...

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

Es gibt hier ein "interessanten" Nachteil: für Ziele, die unter Android vor 4.0 ausgeführt werden, der Gref-Wert entspricht der Adresse des Java-Objekts in der Android-Runtime-Arbeitsspeicher. (D. h. der Garbage Collector ist eine nicht verschieben, konservative, -Sammlung, und sie direkte Verweise auf diese Objekte übergeben wird.) Daher nach einer + g + + w +, - g-, + g +, - w-Sequenz, die sich ergebende Gref müssen den gleichen Wert wie der ursprüngliche Gref-Wert. Auf diese Weise Grepping-Protokolle durchsuchen, ziemlich einfach.

Android 4.0, allerdings verfügt über einen gleitenden Collector und nicht mehr übergibt direkte Verweise auf Android-Runtime-Objekte. Daher nach einer + g + + w +, - g-, + g +, - w-Sequenz, die Gref-Wert *unterscheiden sich*. Wenn das Objekt mehrere globale Kataloge überdauert, geht es mehrere Gref-Werten, wodurch erschwert, um zu bestimmen, in dem eine Instanz von tatsächlich zugeordnet wurde.

### <a name="querying-programmatically"></a>Programmgesteuertes Abfragen

Sie können Abfragen GREF sowohl WREF Anzahl Abfragen die `JniRuntime` Objekt.

`Java.Interop.JniRuntime.CurrentRuntime.GlobalReferenceCount` -Globalen Verweiszähler

`Java.Interop.JniRuntime.CurrentRuntime.WeakGlobalReferenceCount` -Die Anzahl von schwache Verweise



## <a name="android-debug-logs"></a>Android-Debugprotokoll-Protokolle

Die [Android Debugprotokolle](~/android/deploy-test/debugging/android-debug-log.md) vorsehen zusätzlichen Kontext in Bezug auf alle Laufzeitfehler, die Sie sehen.



## <a name="floating-point-performance-is-terrible"></a>Gleitkomma-Leistung ist sehr schlecht!

Sie können auch "ausgeführt wird meine app 10-Mal schnellere mit dem Debug-Build als mit dem Releasebuild!"

Xamarin.Android unterstützt mehrere Geräte ABIs: *Armeabi*, *Armeabi-v7a*, und *X86*. Gerät ABIs können angegeben werden, in **Projekteigenschaften > Registerkarte "Anwendung" > unterstützte Architekturen**.

Debug-Builds verwenden Sie ein Android-Paket, die alle ABIs enthält, und daher verwendet die schnellste ABI für das Zielgerät.

Releasebuilds enthalten nur die ABIs in der Registerkarte "Projekteigenschaften" ausgewählt. Mehr als eine kann ausgewählt werden.

*Armeabi* ist die Standardeinstellung ABI und verfügt über die eine sehr umfassende geräteunterstützung. *Jedoch*, Armeabi unterstützt keine Multi-CPU-Geräte und Hardware Gleitkomma-Amont andere Dinge. Daher-apps mit der Armeabi-Runtime-Version werden auf einen einzelnen Kern gebunden werden und eine Soft-Float-Implementierung verwenden. Beide können zur erheblich langsameren Leistung für Ihre app beitragen.

Wenn Ihre app ordentliche Gleitkomma-Leistung (z. B. Spiele) erfordert, müssen Sie aktivieren die *Armeabi-v7a* ABI. Empfiehlt es sich nur zum unterstützen der *Armeabi-v7a* Common Language Runtime, obwohl dies bedeutet, dass ältere Geräte, die unterstützen nur *Armeabi* nicht zum Ausführen der app.



## <a name="could-not-locate-android-sdk"></a>Android SDK wurde nicht gefunden werden.

Stehen 2 Downloads von Google für Android-SDK für Windows zur Verfügung.
Wenn Sie die .exe-Installer auswählen, wird es Registrierungsschlüssel schreiben, die Xamarin.Android mitteilen, wo sie installiert wurde. Wenn Sie die ZIP-Datei und Entzippen Sie sie selbst, weiß Xamarin.Android nicht, wo Sie für das SDK zu suchen. Sie können Xamarin.Android anweisen ist, in dem das SDK in Visual Studio, indem Sie zu **Tools > Optionen > Xamarin > Android-Einstellungen**:

[![Android SDK-Speicherort in den Xamarin Android-Einstellungen](troubleshooting-images/01.png)](troubleshooting-images/01.png#lightbox)



## <a name="ide-does-not-display-target-device"></a>IDE wird Zielgerät nicht angezeigt werden.

Mitunter versuchen Sie zum Bereitstellen der Anwendung auf einem Gerät, aber das Gerät, die, das Sie für wird nicht angezeigt, in das Gerät auswählen-Dialogfeld bereitstellen möchten. Dies kann passieren, wenn möchte, dass Android Debug Bridge in Urlaub fahren.

Um dieses Problem zu diagnostizieren, suchen die [Adb Programm](~/android/deploy-test/debugging/android-debug-log.md), führen Sie dann:

```shell
adb devices
```

Wenn Ihr Gerät nicht vorhanden ist, müssen Sie den Android Debug Bridge-Server neu starten, damit Ihr Gerät gefunden werden kann:

```shell
adb kill-server
adb start-server
```

HTC-Sync-Software kann verhindern, dass **starten Sie Adb-Server** ordnungsgemäß funktioniert. Wenn die **starten Sie Adb-Server** Befehl nicht drucken, welchen Port wird es auf gestartet beenden Sie die Synchronisierung der HTC-Software und starten Sie Adb-Server neu.


## <a name="the-specified-task-executable-keytool-could-not-be-run"></a>Die angegebene Aufgabe ausführbare Datei "Keytool" konnte nicht ausgeführt werden

Dies bedeutet, dass es sich bei Ihrem Pfad nicht das Verzeichnis enthält, auf dem sich das Java-SDK-Bin-Verzeichnis befindet. Überprüfen Sie, dass Sie diese Schritte aus der [Installation](~/android/get-started/installation/index.md) Guide.


## <a name="monodroidexe-or-aresgenexe-exited-with-code-1"></a>monodroid.exe oder aresgen.exe wurde mit Fehlercode 1 beendet.

Um dieses Problem zu debuggen, wechseln Sie in Visual Studio, und ändern den Ausführlichkeitsgrad von MSBuild zu diesem Zweck wählen aus: **Extras > Optionen > Projekt** und **Lösungen > Erstellen** und **ausführen > MSBuild-Projekt erstellen Ausgabeausführlichkeit** und legen Sie diesen Wert auf **Normal**.

Erstellen Sie neu, und überprüfen Sie Visual Studio-Ausgabebereich, der die vollständige Fehlermeldung enthalten sollte.

## <a name="there-is-not-enough-storage-space-on-the-device-to-deploy-the-package"></a>Es ist nicht genügend Speicherplatz auf dem Gerät zur Bereitstellung des Pakets

Dies tritt auf, wenn Sie den Emulator in Visual Studio nicht gestartet. Wenn Sie den außerhalb von Visual Studio-Emulator zu starten, müssen Sie übergeben die `-partition-size 512` Optionen, z. B.

```shell
emulator -partition-size 512 -avd MonoDroid
```

Stellen Sie sicher, Sie verwenden Sie den Namen des richtigen Simulator, d. h. [den Namen, die Sie beim Konfigurieren des Simulators verwendet](~/android/get-started/installation/windows.md#device).


## <a name="installfailedinvalidapk-when-installing-a-package"></a>Installieren Sie\_Fehler\_ungültige\_APK beim Installieren eines Pakets

Android-Paketnamen *müssen* einen Punkt enthalten ("*.*"). Bearbeiten Sie Ihren Paketnamen an, sodass er einen Punkt enthält.

-   In Visual Studio:
    -   Klicken Sie mit der rechten Maustaste auf Ihr Projekt > Eigenschaften
    -   Klicken Sie auf der Registerkarte "Android-Manifest" auf der linken Seite.
    -   Aktualisieren Sie das Feld des Paket-Name.
        -   Wenn die Meldung &ldquo;keine "androidmanifest.xml" gefunden. Klicken Sie auf diese Option, um eine hinzufügen. &rdquo;, klicken Sie auf den Link, und aktualisieren Sie dann auf das Feld des Paket-Name.
-   In Visual Studio für Mac:
    -   Klicken Sie mit der rechten Maustaste auf Ihr Projekt > Optionen.
    -   Navigieren Sie zu der Build / Android-Anwendung-Abschnitt.
    -   Ändern Sie das Paket-Name-Feld enthält eine '.'.




## <a name="installfailedmissingsharedlibrary-when-installing-a-package"></a>Installieren Sie\_Fehler\_MISSING\_SHARED\_Bibliothek beim Installieren eines Pakets

"Freigegebene Bibliothek" in diesem Kontext ist *nicht* einer nativen freigegebenen Bibliothek (*libfoo.so*) Datei; es wird stattdessen eine Bibliothek, die auf dem Zielgerät, z. B. Google Maps separat installiert werden muss.

Das Android-Paket gibt an, welche freigegebenen Bibliotheken mit erforderlich sind die `<uses-library/>` Element. Wenn eine *erforderlichen* Bibliothek ist nicht auf dem Zielgerät vorhanden (z. B. `//uses-library/@android:required` ist *"true"*, dies ist die Standardeinstellung), Paketinstallation nicht ausgeführt werden, mit *installieren\_ Fehler bei\_MISSING\_SHARED\_Bibliothek*.

Um zu bestimmen, welche freigegebenen Bibliotheken erforderlich sind, zeigen die *generiert*
 **"androidmanifest.xml"** Datei (z. B. **Obj\\Debuggen\\android \\"Androidmanifest.xml"**) und suchen Sie nach der `<uses-library/>` Elemente. `<uses-library/>` Elemente können manuell hinzugefügt werden, in Ihrem Projekts die **Eigenschaften\\"androidmanifest.xml"** Datei und über die [UsesLibraryAttribute benutzerdefiniertes Attribut](https://developer.xamarin.com/api/type/Android.App.UsesLibraryAttribute/).

Beispielsweise durch Hinzufügen eines Assemblyverweises zu *Mono.Android.GoogleMaps.dll* implizit fügen eine `<uses-library/>` für die freigegebene Bibliothek von Google Maps.



## <a name="installfailedupdateincompatible-when-installing-a-package"></a>Installieren Sie\_Fehler\_UPDATE\_INKOMPATIBEL, die beim Installieren eines Pakets

Android-Paketen haben drei Anforderungen:

-   Sie darf ein '. " (siehe vorherigen Eintrag)
-   Sie müssen eine eindeutige Zeichenfolge-Paketname (daher der Reverse-Tld-Konvention finden Sie in der Android-app-Namen, z. B. com.android.chrome für Chrome-app)
-   Wenn Sie Pakete zu aktualisieren, müssen das Paket den gleichen Anmeldeschlüssel.

Stellen Sie daher dieses Szenario vor:

1.  Sie erstellen und Bereitstellen Ihrer app als eine Debug-app
2.  Sie ändern den Signaturschlüssel, z. B. in als Freigabe-app verwenden (oder weil es sich bei Ihnen nicht gefallen die standardmäßig bereitgestellten Debuggen Signaturschlüssel)
3.  Sie Ihrer app zu installieren, ohne ihn zuerst zu entfernen, z. B. Debuggen > Starten ohne Debugging in Visual Studio


In diesem Fall fehl Paketinstallation mit einer Installation\_Fehler\_UPDATE\_INKOMPATIBLE Fehler, da der Paketname und die Signatur nicht geändert haben Schlüssel hat. Die [Android-Debugprotokoll](~/android/deploy-test/debugging/android-debug-log.md) enthält auch eine ähnliche Meldung:

```shell
E/PackageManager(  146): Package [PackageName] signatures do not match the previously installed version; ignoring!
```

Um diesen Fehler zu beheben, vollständig Entfernen der Anwendungs auf Ihrem Gerät vor der erneuten Installation.


## <a name="installfaileduidchanged-when-installing-a-package"></a>Installieren Sie\_Fehler\_UID\_geändert, wenn ein Paket installieren

Wenn ein Android-Paket installiert ist, erhält er eine *Benutzer-Id* (UID).
*Manchmal*, die Installation fehl Gründen derzeit unbekannt, wenn Sie über eine bereits installierte app installieren, mit `INSTALL_FAILED_UID_CHANGED`:

```shell
ERROR [2015-03-23 11:19:01Z]: ANDROID: Deployment failed
Mono.AndroidTools.InstallFailedException: Failure [INSTALL_FAILED_UID_CHANGED]
   at Mono.AndroidTools.Internal.AdbOutputParsing.CheckInstallSuccess(String output, String packageName)
   at Mono.AndroidTools.AndroidDevice.<>c__DisplayClass2c.<InstallPackage>b__2b(Task`1 t)
   at System.Threading.Tasks.ContinuationTaskFromResultTask`1.InnerInvoke()
   at System.Threading.Tasks.Task.Execute()
```

Um dieses Problem zu umgehen *vollständig deinstallieren* Android-Paket, indem Sie die app aus der Android-Ziel-GUI installieren oder `adb`:

```shell
$ adb uninstall @PACKAGE_NAME@
```

**Verwenden Sie keine** `adb uninstall -k`, da hierdurch *beibehalten* Anwendungsdaten, und behalten Sie daher die in Konflikt stehende UID auf dem Zielgerät.



## <a name="release-apps-fail-to-launch-on-device"></a>Release-apps nicht auf Gerät gestartet werden soll.

Die Ausgabe des Android-Debugprotokoll enthält eine Meldung ähnlich:

```shell
D/AndroidRuntime( 1710): Shutting down VM
W/dalvikvm( 1710): threadid=1: thread exiting with uncaught exception (group=0xb412f180)
E/AndroidRuntime( 1710): FATAL EXCEPTION: main
E/AndroidRuntime( 1710): java.lang.UnsatisfiedLinkError: Couldn't load monodroid: findLibrary returned null
E/AndroidRuntime( 1710):        at java.lang.Runtime.loadLibrary(Runtime.java:365)
```

Wenn dies der Fall ist, gibt es zwei mögliche Gründe:

1.  Die apk-Datei keine ABI bereitgestellt wird, das Zielgerät unterstützt.
    Klicken Sie z. B. die apk-Datei enthält nur Armeabi-v7a-Binärdateien, und das Zielgerät unterstützt nur Armeabi.

2.  Ein [Android Fehler](http://code.google.com/p/android/issues/detail?id=21670). Wenn dies der Fall ist, deinstallieren Sie die app, die Finger plattformübergreifende und die Anwendung neu installieren.

Um (1) zu beheben, bearbeiten Sie die Projekteigenschaften/Optionen und [Hinzufügen von Unterstützung für die erforderlichen ABI zu der Liste der unterstützten ABIs](~/android/app-fundamentals/cpu-architectures.md). Um zu bestimmen, welche ABI, die zum Hinzufügen, müssen Sie führen die folgenden Adb-Befehl für das Zielgerät:

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

## <a name="the-outpath-property-is-not-set-for-project-ldquomyappcsprojrdquo"></a>Die OutPath-Eigenschaft ist nicht festgelegt, für Projekt &ldquo;MyApp.csproj-Datei&rdquo;

Dies im Allgemeinen bedeutet, dass Sie eine HP-Computer und die Umgebungsvariable &ldquo;Plattform&rdquo; etwas wie MCD oder HPD festgelegt wurde. Dies steht in Konflikt mit der MSBuild-Plattform-Eigenschaft, die in der Regel, um festgelegt ist &ldquo;Any CPU&rdquo; oder &ldquo;X86&rdquo;. Sie benötigen, um diese Umgebungsvariable auf Ihrem Computer zu entfernen, bevor MSBuild verwendet werden kann:

-   Systemsteuerung > System > Erweitert > Umgebungsvariablen

Starten Sie Visual Studio oder Visual Studio für Mac neu, und versuchen Sie neu zu erstellen. Dinge sollte nun wie erwartet funktionieren.

## <a name="javalangclasscastexception-monoandroidruntimejavaobject-cannot-be-cast-to"></a>java.lang.ClassCastException: mono.android.runtime.JavaObject nicht konvertiert werden kann...

Xamarin.Android 4.x nicht ordnungsgemäß Marshallen von geschachtelten generischen Typen ordnungsgemäß. Betrachten Sie beispielsweise die folgende C\# code mit [SimpleExpandableListAdapter](https://developer.xamarin.com/api/type/Android.Widget.SimpleExpandableListAdapter/):


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


Das Problem ist, dass Xamarin.Android nicht ordnungsgemäß geschachtelte generische Typen gemarshallt. Die `List<IDictionary<string, object>>` gemarshallt wird eine [java.lang.ArrrayList](https://developer.xamarin.com/api/type/Java.Util.ArrayList/), aber die `ArrayList` ist enthält `mono.android.runtime.JavaObject` Instanzen (der Verweis der `Dictionary<string, object>` Instanzen) anstelle von etwas, das implementiert[java.util.Map](https://developer.xamarin.com/api/type/Java.Util.IMap/), sodass in der folgenden Ausnahme:

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

Die problemumgehung besteht darin, die bereitgestellten [Java Auflistungstypen](~/android/internals/api-design.md) statt der `System.Collections.Generic` -Typen für die &ldquo;innere&rdquo; Typen. Dies führt zu entsprechenden Java-Datentypen beim Marshalling von Instanzen. (Der folgende Code ist komplizierter als nötig, um Gref-Lebensdauer zu reduzieren. Es vereinfacht werden, ändern den ursprünglichen Code über `s/List/JavaList/g` und `s/Dictionary/JavaDictionary/g` Wenn Gref-Lebensdauer befürchten sind.)

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


## <a name="unexpected-nullreferenceexceptions"></a>Unerwartete NullReferenceExceptions

Gelegentlich die [Android-Debugprotokoll](~/android/deploy-test/debugging/android-debug-log.md) wird ganz zu schweigen NullReferenceExceptions, &ldquo;kann nicht ausgeführt,&rdquo; oder Mono für Android-Runtime-Code, kurz vor der app-anruft stammen:

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

Dies kann auftreten, wenn die Android-Runtime entscheidet, den Prozess abzubrechen, dies für eine beliebige Anzahl von Gründen, einschließlich des Ziels GREF Obergrenze erreichen oder Aktionen ausgeführt werden kann passieren, &ldquo;falsche&rdquo; mit JNI.

Um festzustellen, ob dies der Fall ist, überprüfen Sie das Android Debugprotokoll für eine Nachricht von einem Prozess ähnelt:

```shell
E/dalvikvm(  123): VM aborting
```


## <a name="abort-due-to-global-reference-exhaustion"></a>Aufgrund von Ausschöpfung des globalen Verweis Abbrechen

Der Android-Runtime-JNI-Ebene unterstützt nur eine begrenzte Anzahl von JNI-Objektverweise, rechtzeitig zu einem bestimmten Zeitpunkt gültig sein. Wenn dieses Limit überschritten wird, werden bestimmte Elemente beschädigt.

Die GREF (*globalen Verweis*) Grenzwert ist 2000 Verweise im Emulator und ~ 52000 verweist auf Hardware.

Sie wissen, dass Sie beginnen, um zu viele GREFs zu erstellen, wenn Sie Fehlermeldungen wie diese im Android Debuggen finden Sie unter:

```shell
D/dalvikvm(  602): GREF has increased to 1801
```

Wenn Sie die GREF-Grenze erreicht haben, wird eine Meldung wie die folgende ausgegeben:

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


Im obigen Beispiel (was übrigens stammt [Fehler 685215](https://bugzilla.novell.com/show_bug.cgi?id=685215)) das Problem ist, zu viele Android.Graphics.Point Instanzen erstellt werden, finden Sie unter [Kommentar \#2](https://bugzilla.novell.com/show_bug.cgi?id=685215#c2) für eine Liste der Updates für Dieses bestimmte Problem.

In der Regel eine nützliche Lösung ist zu ermitteln, welche Art zu viele Instanzen hat zugeordnet &ndash; Android.Graphics.Point im oben genannten Dump &ndash; suchen, in denen sie in Ihrem Quellcode und verwerfen Sie sie entsprechend erstellt werden (so, dass ihre Java-Object Lebensdauer wird verkürzt). Dies ist nicht immer geeignet (\#685215 ist eine Multithreadanwendung, damit die Lösung einfache den Dispose-Aufruf vermieden), aber es ist die erste, was zu berücksichtigen.

Sie können [GREF Protokollierung](~/android/troubleshooting/index.md) angezeigt, wenn GREFs erstellt werden, und wie viele vorhanden sind.


## <a name="abort-due-to-jni-type-mismatch"></a>Abbruch aufgrund einer JNI-Typenkonflikt

Wenn Sie manuell-JNI-Code ein, es ist möglich, dass die Typen ordnungsgemäß, z. B. abweicht, wenn Sie versuchen, aufzurufen `java.lang.Runnable.run` auf einen Typ, der nicht implementiert `java.lang.Runnable`. In diesem Fall kann eine Meldung wie folgt in der Android-Debug-Protokolls:

```shell
W/dalvikvm( 123): JNI WARNING: can't call Ljava/Type;;.method on instance of Lanother/java/Type;
W/dalvikvm( 123):              in Lmono/java/lang/RunnableImplementor;.n_run:()V (CallVoidMethodA)
...
E/dalvikvm( 123): VM aborting
```

## <a name="dynamic-code-support"></a>Unterstützung für dynamische Code

### <a name="dynamic-code-does-not-compile"></a>Dynamische Code wird nicht kompiliert.

Verwenden von C\# dynamisch in Ihrer Anwendung oder Bibliothek, müssen Sie "System.Core.dll" "," Microsoft.CSharp.dll "und" Mono.CSharp.dll zu Ihrem Projekt hinzufügen.

### <a name="in-release-build-missingmethodexception-occurs-for-dynamic-code-at-run-time"></a>Im Releasebuild tritt ein, MissingMethodException für dynamische Code zur Laufzeit.

-   Es ist wahrscheinlich, dass Ihr Anwendungsprojekt keine Verweise auf "System.Core.dll" "," Microsoft.CSharp.dll "oder" Mono.CSharp.dll. Stellen Sie sicher, dass diese Assemblys verwiesen werden.

    -   Bedenken Sie diese dynamischen Code immer Kosten. Wenn Sie die effizienten Code benötigen, sollten Sie nicht mithilfe von dynamischem Code.

-   In der ersten Vorschauversion wurden diese Assemblys ausgeschlossen, es sei denn, die Typen in jeder Assembly explizit von der Anwendung verwendet werden. Finden Sie in der folgenden für dieses Problem zu umgehen: [http://lists.ximian.com/pipermail/mo...il/009798.html](http://lists.ximian.com/pipermail/monodroid/2012-April/009798.html)


## <a name="projects-built-with-aotllvm-crash-on-x86-devices"></a>Projekte mit AOT + LLVM-Absturz Grundlage X86 Geräte

Beim Bereitstellen einer Apps mit erstellten [AOT + LLVM](~/android/deploy-test/release-prep/index.md) auf X86-basierten Geräten, möglicherweise eine Ausnahmefehlermeldung ähnlich der folgenden angezeigt:

```shell
Assertion: should not be reached at /Users/.../external/mono/mono/mini/tramp-x86.c:124
Fatal signal 6 (SIGABRT), code -6 in tid 4051 (amarin.bug56111)
```

Dies ist ein bekanntes Problem, wie Sie in [56111](https://bugzilla.xamarin.com/show_bug.cgi?id=56111). Die problemumgehung besteht darin, LLVM deaktivieren.
