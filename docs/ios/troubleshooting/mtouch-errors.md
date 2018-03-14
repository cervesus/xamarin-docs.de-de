---
title: Xamarin.iOS errors
ms.topic: article
ms.prod: xamarin
ms.assetid: 9F76162B-D622-45DA-996B-2FBF8017E208
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/06/2018
ms.openlocfilehash: 32a73232667e54eef7536f0bb0d1baa190269d8e
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="xamarinios-errors"></a>Xamarin.iOS errors

## <a name="mt0xxx-mtouch-error-messages"></a>MT0xxx: Mtouch-Fehlermeldungen

Beispiel: Parameter, Umgebung, Tools fehlt.

<!--
 MT0xxx mtouch itself, e.g. parameters, environment (e.g. missing tools)
 https://github.com/xamarin/xamarin-macios/blob/master/tools/mtouch/error.cs
    -->

### <a name="a-namemt0000mt0000-unexpected-error---please-fill-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT0000"/>MT0000: Unerwarteter Fehler: Bitte geben Sie einen Fehlerbericht an http://bugzilla.xamarin.com

Ein unerwarteter Fehler aufgetreten ist. Bitte [Datei einen Fehlerbericht](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) durch so viele Informationen wie möglich, einschließlich:

* Vollständige Buildprotokolle, mit maximale Ausführlichkeit (z. B. `-v -v -v -v` in der **zusätzliche Mtouch Argumente**);
* Eine minimale Testfall, der den Fehler reproduzieren; und
* Alle Informationen der version

Die einfachste Möglichkeit zum Abrufen von Informationen zur exakten Version ist die Verwendung der **Visual Studio für Mac** Menü **zu Visual Studio für Mac** Element **Details anzeigen** Schaltfläche und Kopieren/Einfügen der Version-Informationen (Sie können die **Informationen kopieren** Schaltfläche).

### <a name="a-namemt0001mt0001--devname-was-provided-without-any-device-specific-action"></a><a name="MT0001"/>MT0001: "-Devname" wurde ohne Eingreifen gerätespezifischen bereitgestellt

Dies ist eine Warnung, die ausgegeben wird, wenn - Devname an Mtouch, wenn keine gerätespezifischen Aktion übergeben wird (-Logdev/Installdev/Killdev/Launchdev /-Listapps) wurde angefordert.

### <a name="a-namemt0002mt0002-could-not-parse-the-environment-variable-"></a><a name="MT0002"/>MT0002: Konnte nicht analysiert werden. die Umgebungsvariable *.

Dieser Fehler tritt auf, wenn Sie versuchen, eine ungültige umgebungsschlüssel festzulegen = Variable-Wert-Paar. Das richtige Format ist: `mtouch --setenv=VARIABLE=VALUE`

### <a name="a-namemt0003mt0003-application-name-exe-conflicts-with-an-sdk-or-product-assembly-dll-name"></a><a name="MT0003"/>MT0003: Anwendungsname ' * .exe "steht in Konflikt mit einem Namen der Assembly (.dll) SDK oder Produkt.

Namen der ausführbaren Assembly und den Namen der Anwendung darf nicht den Namen der DLLs in der app übereinstimmen. Ändern Sie den Namen der ausführbaren Datei.

### <a name="a-namemt0004mt0004-new-refcounting-logic-requires-sgen-to-be-enabled-too"></a><a name="MT0004"/>MT0004: Die Logik der neuen Refcounting erfordert SGen zu aktiviert werden.

Wenn Sie die Erweiterung Refcounting aktivieren, müssen Sie auch der SGen-Garbagecollector in iOS-Build-Optionen (Registerkarte "Erweitert") für das Projekt aktivieren.

Beginnend mit Xamarin.iOS 7.2.1 diese Anforderung wurde aufgehoben, die Logik der neuen Refcounting kann mit Boehm und SGen Garbage Collectors aktiviert werden.

### <a name="a-namemt0005mt0005-the-output-directory--does-not-exist"></a><a name="MT0005"/>MT0005: Das Ausgabeverzeichnis * ist nicht vorhanden.

Erstellen Sie das Verzeichnis ein.

Dieser Fehler wird nicht mehr generiert, Mtouch erstellt automatisch das Verzeichnis aus, wenn er nicht vorhanden ist.

### <a name="a-namemt0006mt0006-there-is-no-devel-platform-at--use---platformplat-to-specify-the-sdk"></a><a name="MT0006"/>MT0006: Es ist keine Plattform für entwickl *, verwenden – Plattform = Plattform, um das SDK anzugeben.

Xamarin.iOS wurde nicht an der Position in der Fehlermeldung genannten SDK-Verzeichnis gefunden. Stellen Sie sicher, dass der Pfad korrekt ist.

### <a name="a-namemt0007mt0007-the-root-assembly--does-not-exist"></a><a name="MT0007"/>MT0007: Die stammassembly * ist nicht vorhanden.

Die Assembly an der Position in der Fehlermeldung genannten kann nicht Xamarin.iOS gefunden werden. Stellen Sie sicher, dass der Pfad korrekt ist.

### <a name="a-namemt0008mt0008-you-should-provide-one-root-assembly-only-found--assemblies-"></a><a name="MT0008"/>MT0008: Sollten Sie angeben, eine Assembly, die sich nur #-Stammassemblys: *.

Mehrere stammassembly wurde zwar nur eine stammassembly Mtouch, übergeben.

### <a name="a-namemt0009mt0009-error-while-loading-assemblies-"></a><a name="MT0009"/>MT0009: Fehler beim Laden von Assemblys: *.

Fehler beim Laden der Assemblys die Stamm-Assemblyverweise. Weitere Informationen kann in der Buildausgabe bereitgestellt werden.

### <a name="a-namemt0010mt0010-could-not-parse-the-command-line-arguments-"></a><a name="MT0010"/>MT0010: Konnte nicht analysiert werden die Befehlszeilenargumente: *.

Fehler beim Analysieren der Befehlszeilenargumente. Stellen Sie sicher, dass sie korrekt sind.

### <a name="a-namemt0011mt0011--was-built-against-a-more-recent-runtime--than-monotouch-supports"></a><a name="MT0011"/>MT0011: * für eine neuere Common Language Runtime (*) erstellt wurde, als MonoTouch unterstützt.

Diese Warnung wird i. d. r. gemeldet, weil das Projekt einen Verweis auf eine Klassenbibliothek enthält, die nicht mit der Xamarin.iOS BCL erstellt wurde.

Genauso wie eine app mit dem SDK für .NET 4.0 auf einem System, nur .NET 2.0, unterstützen möglicherweise nicht funktioniert eine Bibliothek mit .NET 4.0 erstellt wurden, funktionieren nicht auf Xamarin.iOS, Xamarin.iOS möglicherweise API "nicht vorhanden" verwenden.

Die allgemeine Lösung besteht darin, die Bibliothek als eine Xamarin.iOS-Klassenbibliothek zu erstellen. Dies geschieht durch Erstellen eines neuen Xamarin.iOS Class Library-Projekts, und fügen Sie die Quelldateien hinzu. Wenn Sie nicht über den Quellcode für die Bibliothek verfügen, sollten Sie wenden Sie sich an den Hersteller und anfordern, dass sie eine Xamarin.iOS-kompatible Version der ihre Bibliothek bieten.

### <a name="a-namemt0012mt0012-incomplete-data-is-provided-to-complete-"></a><a name="MT0012"/>MT0012: Unvollständige Daten werden bereitgestellt, um Sie abzuschließen *.

Dieser Fehler wird nicht mehr in der aktuellen Version von Xamarin.iOS gemeldet.

### <a name="a-namemt0013mt0013-profiling-support-requires-sgen-to-be-enabled-too"></a><a name="MT0013"/>MT0013: Profilerstellungsunterstützung Sgen zu aktiviert werden muss.

SGen (--Sgen) muss aktiviert sein, wenn die profilerstellung (--profilerstellung) aktiviert ist.

### <a name="a-namemt0014mt0014-the-ios--sdk-does-not-support-building-applications-targeting-"></a><a name="MT0014"/>MT0014: IOS * SDK unterstützt keine Erstellen von Anwendungen für *.

Dies kann in folgenden Situationen auftreten:

*  ARMv6 aktiviert ist, und Xcode 4.5 oder höher installiert ist.
*  ARMv7s aktiviert ist, und Xcode 4.4 oder eine frühere Version installiert ist.

Stellen Sie sicher, dass die installierte Version von Xcode die ausgewählten Architekturen unterstützt.

### <a name="a-namemt0015mt0015-invalid-abi--supported-abis-are-i386-x8664--armv7-armv7llvm-armv7llvmthumb2-armv7s-armv7sllvm-armv7sllvmthumb2-arm64-and-arm64llvm"></a><a name="MT0015"/>MT0015: Ungültige ABI: *. Sind Sie unterstützten ABIs: i386, x86_64, armv7 armv7 + Llvm, armv7 Llvm + thumb2, armv7s, armv7s + Llvm, armv7s + Llvm + thumb2, arm64 und arm64 + Llvm.

Mtouch wurde ein ungültiger ABI übergeben. Geben Sie einen gültigen ABI an.

### <a name="a-namemt0016mt0016-the-option--has-been-deprecated"></a><a name="MT0016"/>MT0016: Die Option * wurde als veraltet markiert.

Die erwähnten Mtouch-Option wurde als veraltet markiert und wird ignoriert.

### <a name="a-namemt0017mt0017-you-should-provide-a-root-assembly"></a><a name="MT0017"/>MT0017: Sie sollten eine stammassembly bereitstellen.

Es ist erforderlich, um eine stammassembly (in der Regel der Hauptausführungsdatei) angeben, wenn eine app zu erstellen.

### <a name="a-namemt0018mt0018-unknown-command-line-argument-"></a><a name="MT0018"/>MT0018: Unbekanntes Befehlszeilenargument: *.

Das Befehlszeilenargument in der Fehlermeldung genannten erkannt Mtouch nicht.

### <a name="a-namemt0019mt0019-only-one---loginstallkilllaunchdev-or---launchdebugsim-option-can-be-used"></a><a name="MT0019"/>MT0019: Nur eine--[Log | installieren | kill | starten] Dev oder--[starten | debug] Sim-Option kann verwendet werden.

Es gibt mehrere Optionen zum Mtouch, die gleichzeitig verwendet werden können:

-  --logdev
-  --installdev
-  --killdev
-  --launchdev
-  --launchdebug
-  --launchsim

### <a name="a-namemt0020mt0020-the-valid-options-for--are-"></a><a name="MT0020"/>MT0020 Die gültigen Optionen für "\*"sind"\*".

### <a name="a-namemt0021mt0021-cannot-compile-using-gccg---use-gcc-when-using-the-static-registrar-this-is-the-default-when-compiling-for-device-either-remove-the---use-gcc-flag-or-use-the-dynamic-registrar---registrardynamic"></a><a name="MT0021"/>MT0021 kann Kompilieren mit Gcc / g ++ (--verwenden Gcc) nach der Verwendung von statischen Registrierungsstelle (Dies ist die Standardeinstellung beim Kompilieren für Gerät). Entfernen Sie entweder die--Verwendung-Gcc kennzeichnen, oder verwenden Sie die dynamische Registrierungsstelle (--Registrierungsstelle: dynamische).

### <a name="a-namemt0022mt0022-the-options---unsupported--enable-generics-in-registrar-and---registrar-are-not-compatible"></a><a name="MT0022"/>MT0022 Die Optionen '– nicht unterstützte – Enable-Generika-im-Registrierungsstelle"und"--Registrierungsstelle "sind nicht kompatibel.

Entfernen Sie beide Optionen `--unsupported--enable-generics-in-registrar` und `--registrar`. Beginnen mit Xamarin.iOS 7.2.1 die Standard-Registrierungsstelle unterstützt Generika.

Dieser Fehler wird nicht mehr angezeigt (das Befehlszeilenargument `--unsupported--enable-generics-in-registrar` Mtouch entzogen wurde).

### <a name="a-namemt0023mt0023-application-name-exe-conflicts-with-another-user-assembly"></a><a name="MT0023"/>MT0023 Anwendungsname "* .exe" steht in Konflikt mit einem anderen Benutzerassembly.

Namen der ausführbaren Assembly und den Namen der Anwendung darf nicht den Namen der DLLs in der app übereinstimmen. Ändern Sie den Namen der ausführbaren Datei.

### <a name="a-namemt0024mt0024-could-not-find-required-file-"></a><a name="MT0024"/>Erforderliche Datei MT0024 wurde nicht gefunden "*".

### <a name="a-namemt0025mt0025-no-sdk-version-was-provided-please-add---sdkxy-to-specify-which-ios-sdk-should-be-used-to-build-your-application"></a><a name="MT0025"/>MT0025 keine SDK-Version wurde angegeben. Fügen Sie `--sdk=X.Y` an, welche iOS SDK zum Erstellen der Anwendung verwendet werden sollte.

### <a name="a-namemt0026mt0026-could-not-parse-the-command-line-argument--"></a><a name="MT0026"/>MT0026 konnte nicht analysiert werden. das Befehlszeilenargument ' *': *

### <a name="a-namemt0027mt0027-the-options--and--are-not-compatible"></a><a name="MT0027"/>MT0027 Der Optionen\*'und'\*' sind nicht kompatibel.

### <a name="a-namemt0028mt0028-cannot-enable-pie--pie-when-targeting-ios-41-or-earlier-please-disable-pie--piefalse-or-set-the-deployment-target-to-at-least-ios-42"></a><a name="MT0028"/>Kreis MT0028 kann nicht aktiviert werden kann (-Kreis) Geschäftsgruppen iOS 4.1 oder früher. Deaktivieren Sie den Kreis (-Kreisdiagramm: "false"), oder legen Sie das Bereitstellungsziel auf mindestens iOS 4.2

### <a name="a-namemt0029mt0029-repl---enable-repl-is-only-supported-in-the-simulator---sim"></a><a name="MT0029"/>MT0029: REPL (--aktivieren Repl) wird nur in der Simulator unterstützt (--SIM-Karte).

Textgröße für Replikation wird nur unterstützt, wenn Sie für den Simulator erstellen. Dies bedeutet, dass, wenn Sie übergeben `--enable-repl` zu Mtouch, müssen Sie außerdem `--sim`.

### <a name="a-namemt0030mt0030-the-executable-name--and-the-app-name--are-different-this-may-prevent-crash-logs-from-getting-symbolicated-properly"></a><a name="MT0030"/>MT0030: Namen der ausführbaren Datei (\*) und der app-Name (\*) unterschiedlich ist, wird dadurch möglicherweise verhindert, dass Absturz Protokolle erste ordnungsgemäß symbolicated sind.

Wenn Xcode symbolicates (Speicheradressen-Funktionsnamen und die Datei/Line Zahlen übersetzt) des Prozesses kann fehlschlagen, wenn die ausführbare Datei und die app unterschiedliche Namen (ohne Erweiterung haben).

Zum Beheben dieses entweder ändern "Anwendungsname" des Projekts Build/iOS Anwendungsoptionen oder ändern Sie "Assemblyname" in Build/Ausgabeoptionen für das Projekt.

### <a name="a-namemt0031mt0031-the-command-line-arguments---enable-background-fetch-and---launch-for-background-fetch-require---launchsim-too"></a><a name="MT0031"/>MT0031: Die Befehlszeilenargumente "--aktivieren als abrufen im Hintergrund" und "--Launch-für-Hintergrund-Fetch" erforderlich "--Launchsim" zu.

### <a name="a-namemt0032mt0032-the-option---debugtrack-is-ignored-unless---debug-is-also-specified"></a><a name="MT0032"/>MT0032: Die Option "--Debugtrack" wird ignoriert, es sei denn, "– debug" ist ebenfalls angegeben.

### <a name="a-namemt0033mt0033--a-xamarinios-project-must-reference-either-monotouchdll-or-xamariniosdll"></a><a name="MT0033"/>MT0033 ein Xamarin.iOS Projekt muss monotouch.dll oder Xamarin.iOS.dll verweisen.

### <a name="a-namemt0034mt0034--cannot-include-both-monotouchdll-and-xamariniosdll-in-the-same-xamarinios-project----is-referenced-explicitly-while--is-referenced-by-"></a><a name="MT0034"/>MT0034 kann nicht im selben Xamarin.iOS Projekt - "monotouch.dll" und "Xamarin.iOS.dll" einschließen "\*" wird explizit verwiesen wird, während er sich "\*" verweist auf "*".

<!-- MT0035 unused -->

### <a name="a-namemt0036mt0036--cannot-launch-a--simulator-for-a--app-please-enable-the-correct-architectures-in-your-projects-ios-build-options-advanced-page"></a><a name="MT0036"/>Launch MT0036 kann eine *-Simulator für eine * app. Aktivieren Sie die richtigen Architecture(s) in Ihrem Projekt iOS-Build-Optionen (Seite "Erweitert").

### <a name="a-namemt0037mt0037--monotouchdll-is-not-64-bit-compatible-either-reference-xamariniosdll-or-do-not-build-for-a-64-bit-architecture-arm64-andor-x8664"></a><a name="MT0037"/>MT0037 monotouch.dll ist nicht 64-Bit-kompatibel. Verweisen auf Xamarin.iOS.dll oder nicht für eine 64-Bit-Architektur (ARM64 und/oder x86_64) erstellen.

### <a name="a-namemt0038mt0038--the-old-registrars---registraroldstaticolddynamic-are-not-supported-when-referencing-xamariniosdll"></a><a name="MT0038"/>MT0038 Der alten Registrierungsstellen (--Registrierungsstelle: Oldstatic | Olddynamic) werden nicht unterstützt, wenn Sie Xamarin.iOS.dll verweisen.

### <a name="a-namemt0039mt0039--applications-targeting-armv6-cannot-reference-xamariniosdll"></a><a name="MT0039"/>MT0039-Anwendungen, die für ARMv6 Xamarin.iOS.dll nicht verwiesen werden.

### <a name="a-namemt0040mt0040--could-not-find-the-assembly--referenced-by-"></a><a name="MT0040"/>Die Assembly MT0040 wurde nicht gefunden "\*", auf die verwiesen wird durch"\*".

### <a name="a-namemt0041mt0041--cannot-reference-both-monotouchdll-and-xamariniosdll"></a><a name="MT0041"/>MT0041 kann nicht sowohl "monotouch.dll" und "Xamarin.iOS.dll" verweisen.

### <a name="a-namemt0042mt0042--no-reference-to-either-monotouchdll-or-xamariniosdll-was-found-a-reference-to-monotouchdll-will-be-added"></a><a name="MT0042"/>MT0042 kein Verweis auf monotouch.dll oder Xamarin.iOS.dll wurde gefunden. Ein Verweis auf monotouch.dll wird hinzugefügt.

### <a name="a-namemt0043mt0043-the-boehm-garbage-collector-is-currently-not-supported-when-referencing-xamariniosdll-the-sgen-garbage-collector-has-been-selected-instead"></a><a name="MT0043"/>MT0043: Boehm Garbagecollector wird derzeit nicht unterstützt, wenn "Xamarin.iOS.dll" verweisen auf. SGen-Garbagecollector hat stattdessen ausgewählt wurde.

Nur der SGen-Garbagecollector wird mit Unified-Projekten unterstützt. Stellen Sie sicher, es sind keine zusätzlichen Mtouch Flags Boehm als der Garbage Collector angeben.

### <a name="a-namemt0044mt0044---listsim-is-only-supported-with-xcode-60-or-later"></a><a name="MT0044"/>MT0044:--Listsim wird nur mit Xcode 6.0 oder höher unterstützt.

Eine neuere Version von Xcode zu installieren.

### <a name="a-namemt0045mt0045---extension-is-only-supported-when-using-the-ios-80-or-later-sdk"></a><a name="MT0045"/>MT0045:--Erweiterung wird nur unterstützt, wenn das iOS 8.0 (oder höher) SDK.

<!-- MT0046 is not reported anymore -->

### <a name="a-namemt0047mt0047-the-minimum-deployment-target-for-unified-applications-is-511-the-current-deployment-target-is--please-select-a-newer-deployment-target-in-your-projects-ios-application-options"></a><a name="MT0047"/>MT0047: Das minimale Bereitstellungsziel für Unified Anwendungen 5.1.1, ist das aktuelle Bereitstellungsziel "*". Wählen Sie eine neuere Bereitstellungsziel entweder in Ihrem Projekt iOS Anwendungsoptionen.

<!-- MT0048 is not reported anymore -->

### <a name="a-namemt0049mt0049-framework-is-supported-only-if-deployment-target-is-80-or-later--features-might-not-work-correctly"></a><a name="MT0049"/>MT0049: *.framework wird nur unterstützt, wenn Bereitstellungsziel 8.0 oder höher ausgeführt wird. * die Features funktionieren möglicherweise nicht ordnungsgemäß.

Das angegebene Framework wird nicht in der iOS-Version unterstützt das Bereitstellungsziel entweder bezieht sich auf. Aktualisieren Sie das Bereitstellungsziel entweder auf eine neuere Version von iOS oder entfernen Sie die Verwendung der angegebenen Framework aus der app.

<!-- MT0050 is not reported anymore -->

### <a name="a-namemt0051mt0051-xamarinios--requires-xcode-50-or-later-the-current-xcode-version-found-in--is-"></a><a name="MT0051"/>MT0051: Xamarin.iOS * Xcode 5.0 oder höher erfordert. Die aktuelle Version von Xcode (gefunden *) ist *.

Installieren Sie eine neuere Xcode.

### <a name="a-namemt0052mt0052-no-command-specified"></a><a name="MT0052"/>MT0052: Es wurde kein Befehl angegeben.

Für Mtouch wurde keine Aktion angegeben.

<!-- 0053 is used by mmp -->

### <a name="a-namemt0054mt0054-unable-to-canonicalize-the-path--"></a><a name="MT0054"/>MT0054: Zum Umwandeln des Pfads kann nicht ' *': *

Dies ist ein interner Fehler. Wenn dieser Fehler angezeigt wird, melden Sie den Fehler [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt0055mt0055-the-xcode-path--does-not-exist"></a><a name="MT0055"/>MT0055: Der Pfad Xcode ' *' ist nicht vorhanden.

Mithilfe des Pfads Xcode übergeben `--sdkroot` ist nicht vorhanden. Geben Sie einen gültigen Pfad an.

### <a name="a-namemt0056mt0056-cannot-find-xcode-in-the-default-location-applicationsxcodeapp-please-install-xcode-or-pass-a-custom-path-using---sdkroot-path"></a><a name="MT0056"/>MT0056: Xcode kann nicht gefunden werden, am Standardspeicherort (/ Applications/Xcode.app). Installieren Sie Xcode, oder übergeben Sie einen benutzerdefinierten Pfad--Sdkroot mit <path>.
### <a name="a-namemt0057mt0057-cannot-determine-the-path-to-xcodeapp-from-the-sdk-root--please-specify-the-full-path-to-the-xcodeapp-bundle"></a><a name="MT0057"/>MT0057: Kann nicht den Pfad zum Xcode.app zu ermitteln, aus dem Sdk-Stammverzeichnis "*". Geben Sie den vollständigen Pfad zum Paket Xcode.app an.

Der Pfad zu übergeben, mit `--sdkroot` gibt keine gültige Xcode-app. Geben Sie einen Pfad zu einer Xcode-app an.

### <a name="a-namemt0058mt0058-the-xcodeapp--is-invalid-the-file--does-not-exist"></a><a name="MT0058"/>MT0058: Xcode.app "\*" ist ungültig (die Datei "\*" ist nicht vorhanden).

Der Pfad zu übergeben, mit `--sdkroot` gibt keine gültige Xcode-app. Geben Sie einen Pfad zu einer Xcode-app an.

### <a name="a-namemt0059mt0059-could-not-find-the-currently-selected-xcode-on-the-system-"></a><a name="MT0059"/>MT0059: Der aktuell ausgewählten Xcode auf dem System wurde nicht gefunden werden: *

### <a name="a-namemt0060mt0060-could-not-find-the-currently-selected-xcode-on-the-system-xcode-select---print-path-returned--but-that-directory-does-not-exist"></a><a name="MT0060"/>MT0060: Der aktuell ausgewählten Xcode auf dem System wurde nicht gefunden werden. "Xcode auswählen – Print-Path" zurückgegeben "*", aber dieser Verzeichnis ist nicht vorhanden.

### <a name="a-namemt0061mt0061-no-xcodeapp-specified-using---sdkroot-using-the-system-xcode-as-reported-by-xcode-select---print-path-"></a><a name="MT0061"/>MT0061: Es wurden keine Xcode.app angegeben (mit--Sdkroot), das System Xcode verwenden, wie von "Xcode-SELECT-Anweisung – Print-Path" gemeldet: *

Dies ist eine informative Warnung, erläutern die Xcode werden verwendet, da keine angegeben wurde.

### <a name="a-namemt0062mt0062-no-xcodeapp-specified-using---sdkroot-or-xcode-select---print-path-using-the-default-xcode-instead-"></a><a name="MT0062"/>MT0062: Es wurden keine Xcode.app angegeben (mit Sdkroot – oder "Xcode-auswählen – Print-Path"), verwenden Sie stattdessen die Standardeinstellung Xcode: *

Dies ist eine informative Warnung, erläutern die Xcode werden verwendet, da keine angegeben wurde.

### <a name="a-namemt0063mt0063-cannot-find-the-executable-in-the-extension--no-cfbundleexecutable-entry-in-its-infoplist"></a><a name="MT0063"/>MT0063: Die ausführbare Datei kann nicht gefunden werden, in der Erweiterung * (kein CFBundleExecutable-Eintrag in der Datei "Info.plist")

Jede Datei "Info.plist" muss eine ausführbare Datei (mit der CFBundleExecutable-Eintrag), verfügen, jedoch ein Eintrag während der Erstellung automatisch generiert werden soll.

Dies weist in der Regel ein Fehler im Xamarin.iOS hin; Bitte der Datei eines Fehlerberichts an [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) zu einem Testfall.

### <a name="a-namemt0064mt0064-xamarinios-only-supports-embedded-frameworks-with-unified-projects"></a><a name="MT0064"/>MT0064: Xamarin.iOS unterstützt nur eingebettete Frameworks mit Unified-Projekten.

Xamarin.iOS unterstützt nur eingebettete Frameworks beim Verwenden der einheitliche API; Aktualisieren Sie das Projekt, um die einheitliche API verwenden.

### <a name="a-namemt0065mt0065-xamarinios-only-supports-embedded-frameworks-when-deployment-target-is-at-least-80-current-deployment-target--embedded-frameworks-"></a><a name="MT0065"/>MT0065: Xamarin.iOS unterstützt nur eingebettete Frameworks beim Bereitstellungsziel mindestens 8.0 ist (aktuelle Bereitstellungsziel: * eingebettet Frameworks: *)

Xamarin.iOS unterstützt nur eingebettete Frameworks aus, wenn das Bereitstellungsziel mindestens 8.0 ist (da frühere Versionen von iOS unterstützt keine eingebetteten Frameworks).

Aktualisieren Sie das Bereitstellungsziel entweder in das Projekt "Info.plist" 8.0 oder höher durchführen.

### <a name="a-namemt0066mt0066-invalid-build-registrar-assembly-"></a><a name="MT0066"/>MT0066: Ungültige erstellen Registrierungsstelle-Assembly: *

Dies weist in der Regel ein Fehler im Xamarin.iOS hin; Bitte der Datei eines Fehlerberichts an [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) zu einem Testfall.

### <a name="a-namemt0067mt0067-invalid-registrar-"></a><a name="MT0067"/>MT0067: Ungültige Registrierungsstelle: *

Dies weist in der Regel ein Fehler im Xamarin.iOS hin; Bitte der Datei eines Fehlerberichts an [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) zu einem Testfall.

### <a name="a-namemt0068mt0068-invalid-value-for-target-framework-"></a><a name="MT0068"/>MT0068: Ungültiger Wert für Zielframework: *.

Es wurde ein ungültiges Ziel-Framework übergeben mithilfe der--Zielframework Argument. Geben Sie ein gültiges Ziel-Framework.

<!--### <a name="MT0069"/>MT0069: currently unused -->

### <a name="a-namemt0070mt0070-invalid-target-framework--valid-target-frameworks-are-"></a><a name="MT0070"/>MT0070: Ungültiges Ziel-Framework: *. Gültige Zielframeworks sind: *.

Es wurde ein ungültiges Ziel-Framework übergeben mithilfe der--Zielframework Argument. Geben Sie ein gültiges Ziel-Framework.

### <a name="a-namemt0071mt0071-unknown-platform--this-usually-indicates-a-bug-in-xamarinios-please-file-a-bug-report-at-httpbugzillaxamarincom-with-a-test-case"></a><a name="MT0071"/>MT0071: Unbekannte Plattform: *. Dies weist in der Regel ein Fehler im Xamarin.iOS hin; Bitte Datei einen Fehlerbericht an http://bugzilla.xamarin.com zu einem Testfall.

Dies weist in der Regel ein Fehler im Xamarin.iOS hin; Bitte der Datei eines Fehlerberichts an [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) zu einem Testfall.

### <a name="a-namemt0072mt0072-extensions-are-not-supported-for-the-platform-"></a><a name="MT0072"/>MT0072: Erweiterungen werden für die Plattform nicht unterstützt "*".

Dies weist in der Regel ein Fehler im Xamarin.iOS hin; Bitte der Datei eines Fehlerberichts an [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) zu einem Testfall.

### <a name="a-namemt0073mt0073-xamarinios--does-not-support-a-deployment-target-of--the-minimum-is--please-select-a-newer-deployment-target-in-your-projects-infoplist"></a><a name="MT0073"/>MT0073: Xamarin.iOS * unterstützt kein Bereitstellungsziel aus der * (das Minimum ist *). Wählen Sie eine neuere Bereitstellungsziel entweder in Ihrem Projekt "Info.plist" ein.

Das minimale Bereitstellungsziel ist in der Fehlermeldung angegeben. Wählen Sie eine neuere Bereitstellungsziel entweder in das Projekt "Info.plist" ein.

Wenn Sie das Bereitstellungsziel entweder aktualisieren nicht möglich ist, verwenden Sie eine ältere Version von Xamarin.iOS.

### <a name="a-namemt0074mt0074-xamarinios--does-not-support-a-minimum-deployment-target-of--the-maximum-is--please-select-an-older-deployment-target-in-your-projects-infoplist-or-upgrade-to-a-newer-version-of-xamarinios"></a><a name="MT0074"/>MT0074: Xamarin.iOS * unterstützt keine minimale Bereitstellungsziel * (maximal: *). Wählen Sie eine ältere Bereitstellungsziel entweder auf das Projekt "Info.plist" oder ein upgrade auf eine neuere Version von Xamarin.iOS.

Xamarin.iOS unterstützt nicht das Festlegen der minimalen Bereitstellungsziel entweder auf eine höhere Version als die Version, der für diese Version von Xamarin.iOS erstellt wurde.

Wählen Sie eine ältere minimale Bereitstellungsziel entweder auf das Projekt "Info.plist", oder ein upgrade auf eine neuere Version von Xamarin.iOS.

### <a name="a-namemt0075mt0075-invalid-architecture--for--projects-valid-architectures-are-"></a><a name="MT0075"/>MT0075: Ungültige Architektur ' *' für * Projekte. Gültige Architekturen sind: *

Es wurde eine ungültige Architektur angegeben. Stellen Sie sicher, dass Architektur gültig ist.

### <a name="a-namemt0076mt0075-no-architecture-specified-using-the---abi-argument-an-architecture-is-required-for--projects"></a><a name="MT0076"/>MT0075: Keine Architektur angegeben (mit dem--Abi-Argument). Eine Architektur ist erforderlich, damit * Projekte.

Dies weist in der Regel ein Fehler im Xamarin.iOS hin; Bitte der Datei eines Fehlerberichts an [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) zu einem Testfall.

### <a name="a-namemt0077mt0076-watchos-projects-must-be-extensions"></a><a name="MT0077"/>MT0076: WatchOS Projekte müssen Erweiterungen verwendet werden.

Dies weist in der Regel ein Fehler im Xamarin.iOS hin; Bitte der Datei eines Fehlerberichts an [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) zu einem Testfall.

### <a name="a-namemt0078mt0077-incremental-builds-are-enabled-with-a-deployment-target--80-currently--this-is-not-supported-the-resulting-application-will-not-launch-on-ios-9-so-the-deployment-target-will-be-set-to-80"></a><a name="MT0078"/>MT0077: Inkrementelle Builds sind mit einem Ziel < 8.0-Bereitstellung aktiviert (derzeit *). Dies wird nicht unterstützt (die sich ergebende Anwendung nicht gestartet auf iOS 9), damit das Bereitstellungsziel auf 8.0 festgelegt wird.

Dies ist eine Warnung darüber informiert, dass das Bereitstellungsziel auf 8.0 für diesen Build festgelegt wurde, damit inkrementelle builds ordnungsgemäß arbeiten.

Inkrementelle Builds werden nur unterstützt, wenn das Bereitstellungsziel mindestens 8.0 ist (da die resultierende Anwendung auf iOS 9 andernfalls starten).

### <a name="a-namemt0079mt0078-the-recommended-xcode-version-for-xamarinios--is-xcode--or-later-the-current-xcode-version-found-in--is-"></a><a name="MT0079"/>MT0078: Die empfohlene Xcode Version für Xamarin.iOS * Xcode ist * oder höher. Die aktuelle Version von Xcode (gefunden *) ist *.

Dies ist eine Warnung darüber informiert, dass die aktuelle Version von Xcode nicht die empfohlene Version von Xcode für diese Version von Xamarin.iOS ist.

Aktualisieren Sie Xcode, um eine optimale Verhalten sicherzustellen.

### <a name="a-namemt0080mt0080-disabling-newrefcount---new-refcountfalse-is-deprecated"></a><a name="MT0080"/>MT0080: Deaktivieren von NewRefCount,--new-Refcount:false, ist veraltet.

Dies ist eine Warnung darüber informiert, die der Anforderung zum Deaktivieren der neuen Refcount-Wert (--neu – Refcount:false) wurde ignoriert.

Die neue Refcount-Funktion ist nun für alle Projekte obligatorisch, und es ist daher nicht möglich, mehr zu deaktivieren.

### <a name="a-namemt0081mt0081-the-command-line-argument---download-crash-report-also-requires---download-crash-report-to"></a><a name="MT0081"/>MT0081: Das Befehlszeilenargument – Download Absturzbericht erfordert auch – Download-Crash-Meldung an.
### <a name="a-namemt0082mt0082-repl---enable-repl-is-only-supported-when-linking-is-not-used---nolink"></a><a name="MT0082"/>MT0082: REPL (--aktivieren Repl) wird nur unterstützt, wenn die Verknüpfung nicht verwendet wird (--Nolink).
### <a name="a-namemt0083mt0083-asm-only-bitcode-is-not-supported-on-watchos-use-either---bitcodemarker-or---bitcodefull"></a><a name="MT0083"/>MT0083: Reine Asm Bitcode wird auf WatchOS nicht unterstützt. Verwenden Sie entweder--Bitcode: Marker ' oder '--Bitcode: vollständige.
### <a name="a-namemt0084mt0084-bitcode-is-not-supported-in-the-simulator-do-not-pass---bitcode-when-building-for-the-simulator"></a><a name="MT0084"/>MT0084: Bitcode wird in der Simulator nicht unterstützt. Übergeben Sie – Bitcode nicht auf, wenn für den Simulator zu erstellen.
### <a name="a-namemt0085mt0085-no-reference-to--was-found-it-will-be-added-automatically"></a><a name="MT0085"/>MT0085: Kein Verweis auf "*" wurde gefunden. Er wird automatisch hinzugefügt.
### <a name="a-namemt0086mt0086-a-target-framework---target-framework-must-be-specified-when-building-for-tvos-or-watchos"></a><a name="MT0086"/>MT0086: Eines Zielframeworks (--Zielframework) muss beim Erstellen für tvos. außerdem wurden oder WatchOS angegeben werden.

Dies weist in der Regel ein Fehler im Xamarin.iOS hin; Bitte der Datei eines Fehlerberichts an [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) zu einem Testfall.

### <a name="a-namemt0087mt0087-incremental-builds---fastdev-is-not-supported-with-the-boehm-gc-incremental-builds-will-be-disabled"></a><a name="MT0087"/>MT0087: Inkrementelle Builds (--Fastdev) wird mit dem Boehm globalen Katalogserver nicht unterstützt. Inkrementelle Builds werden deaktiviert.

### <a name="a-namemt0088mt0088-the-gc-must-be-in-cooperative-mode-for-watchos-apps-please-remove-the---coopfalse-argument-to-mtouch"></a><a name="MT0088"/>MT0088: Der globale Katalogserver muss im Modus für WatchOS-apps. Entfernen Sie die--Mtouch Argument Coop: "false".

### <a name="a-namemt0089mt0089-the-option--cannot-take-the-value--when-cooperative-mode-is-enabled-for-the-gc"></a><a name="MT0089"/>MT0089: Die Option "\*"kann nicht erhalten den Wert"\*" Wenn kooperativen Modus für den globalen Katalog aktiviert ist.

### <a name="a-namemt0091mt0091-this-version-of-xamarinios-requires-the--sdk-shipped-with-xcode--either-upgrade-xcode-to-get-the-required-header-files-or-set-the-managed-linker-behaviour-to-link-framework-sdks-only-to-try-to-avoid-the-new-apis"></a><a name="MT0091"/>MT0091: Diese Version von Xamarin.iOS erfordert die * SDK (Xcode Lieferumfang *). Entweder Xcode erhalten die erforderlichen Header-Dateien oder das Verhalten des verwalteten Linker auf Link Framework SDKs nur (für den Versuch, vermeiden Sie die neuen APIs) zu aktualisieren.

Xamarin.iOS erfordert die Headerdateien, von der SDK-Version in der Fehlermeldung zur Erstellung der Anwendung angegeben. Die empfohlene Methode zum Beheben dieses Fehlers Xcode zum Abrufen der erforderlichen SDK zu aktualisieren, dazu gehören die erforderlichen Headerdateien. Wenn Sie mehrere Versionen von Xcode installiert haben oder ein Xcode in einer nicht standardmäßigen Speicherort verwenden möchten, stellen Sie sicher, dass die richtigen Xcode-Speicherort in der IDE Einstellungen.

Ein potenzieller ist alternative Lösung zum Aktivieren des verwalteten Linkers. Hiermit entfernen Sie nicht verwendete API einschließen, in den meisten Fällen, die neue API, in dem die Headerdateien fehlt (oder ist unvollständig) sind. Jedoch stellt dies nicht funktioniert, wenn das Projekt-API verwendet, die in einer neueren SDK als an denjenigen Ihrer Xcode eingeführt wurde.

Eine letzte Stroh-Lösung mit einer älteren Version von Xamarin.iOS wäre, erfordert eine, die das SDK das Projekt unterstützt.

<!-- MT0092 used by mlaunch -->

### <a name="a-namemt0093mt0093-could-not-find-mlaunch"></a><a name="MT0093"/>MT0093: "Mlaunch" konnte nicht gefunden werden.

<!-- MT0094 is not reported anymore -->

### <a name="a-namemt0095mt0095-aot-files-could-not-be-copied-to-the-destination-directory-dest-error"></a><a name="MT0095"/>MT0095: Aot Dateien konnten nicht in das Zielverzeichnis {Dest} kopiert werden: {Error}

### <a name="a-namemt0096mt0096-no-reference-to-xamariniosdll-was-found"></a><a name="MT0096"/>MT0096: Es wurde kein Verweis auf Xamarin.iOS.dll gefunden.

<!-- MT0097: used by mmp -->
<!-- MT0098: used by mmp -->

### <a name="a-namemt0099mt0099-internal-error--please-file-a-bug-report-with-a-test-case-httpbugzillaxamarincom"></a><a name="MT0099"/>MT0099: Interner Fehler *. Bitte der Datei eines Fehlerberichts zu einem Testfall (http://bugzilla.xamarin.com).

Diese Fehlermeldung wird gemeldet, wenn es sich bei eine internen konsistenzüberprüfung in Xamarin.iOS ein Fehler auftritt.

Hiermit wird einen Fehler im Xamarin.iOS; Bitte der Datei eines Fehlerberichts an [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) zu einem Testfall.

### <a name="a-namemt0100mt0100-invalid-assembly-build-target--please-file-a-bug-report-with-a-test-case-httpbugzillaxamarincom"></a><a name="MT0100"/>MT0100: Ungültige Assembly Buildziel: "*". Bitte der Datei eines Fehlerberichts zu einem Testfall (http://bugzilla.xamarin.com).

Diese Fehlermeldung wird gemeldet, wenn es sich bei eine internen konsistenzüberprüfung in Xamarin.iOS ein Fehler auftritt.

Dies ist immer ein Fehler im Xamarin.iOS; Bitte der Datei eines Fehlerberichts an [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) zu einem Testfall.

### <a name="a-namemt0101mt0101-the-assembly--is-specified-multiple-times-in---assembly-build-target-arguments"></a><a name="MT0101"/>MT0101: Die Assembly ' *' wurde mehrmals in – Assembly-Build-Ziel Argumente angegeben.

Die Assembly, die in der Fehlermeldung genannten ist mehrmals in – Assembly-Build-Ziel Argumente angegeben. Stellen Sie sicher, dass jede Assembly nur einmal angegeben wird.

### <a name="a-namemt0102mt0102-the-assemblies--and--have-the-same-target-name--but-different-targets--and-"></a><a name="MT0102"/>MT0102: Die Assemblys\*'und'\*"haben den gleichen Zielnamen ("\*"), aber unterschiedliche Ziele ("\*"und" * ").

Die Assemblys, die in der Fehlermeldung genannten wurden in Konflikt stehende Build-Ziele.

Zum Beispiel:

    --assembly-build-target:Assembly1.dll=framework=MyBinary --assembly-build-target:Assembly2.dll=dynamiclibrary=MyBinary

In diesem Beispiel wird versucht, eine dynamische Bibliothek sowie ein Framework, mit dem gleichen erstellen (`MyBinary`).

### <a name="a-namemt0103mt0103-the-static-object--contains-more-than-one-assembly--but-each-static-object-must-correspond-with-exactly-one-assembly"></a><a name="MT0103"/>MT0103: Die statische Objekt "\*' enthält mehr als eine Assembly ("\*"), aber jedes statisches Objekt muss mit genau einer Assembly entsprechen.

Die Assemblys, die in der Fehlermeldung genannten werden alle in eine einzelne statische Objekt kompiliert. Dies ist nicht zulässig, die an ein anderes statisches Objekt muss jede Assembly kompiliert werden.

Zum Beispiel:

    --assembly-build-target:Assembly1.dll=staticobject=MyBinary --assembly-build-target:Assembly2.dll=staticobject=MyBinary

In diesem Beispiel wird versucht, ein statisches Objekt erstellt (`MyBinary`) bestehend aus zwei Assemblys (`Assembly1.dll` und `Assembly2.dll`), dies ist nicht zulässig.

### <a name="a-namemt0105mt0105-no-assembly-build-target-was-specified-for-"></a><a name="MT0105"/>MT0105: Keine Assembly Build-Ziel wurde angegeben, für "*".

Beim Angeben der Assemblys Versionsattribut Ziel `--assembly-build-target`, jede Assembly in der app muss ein Buildziel zugewiesen haben.

Dieser Fehler wird gemeldet, wenn die Assembly, die in der Fehlermeldung genannten keine Assembly mit dem Ziel zugeordnet zu erstellen.

Finden Sie in der Dokumentation zu `--assembly-build-target` für Weitere Informationen zu erhalten.

### <a name="a-namemt0106mt0106-the-assembly-build-target-name--is-invalid-the-character--is-not-allowed"></a><a name="MT0106"/>MT0106: Erstellen der Assembly Zielname "\*" ist ungültig: das Zeichen '\*' ist nicht zulässig.

Der Name der Assembly Build-Ziel muss ein gültiger Dateiname sein.

Diese Werte werden z. B. diesen Fehler auslösen:

    --assembly-build-target:Assembly1.dll=staticobject=my/path.o

Da `my/path.o` ist kein gültiger Dateiname aufgrund der Verzeichnistrennzeichen.

### <a name="a-namemt0107mt0107-the-assemblies--have-different-custom-llvm-optimizations--which-is-not-allowed-when-they-are-all-compiled-to-a-single-binary"></a><a name="MT0107"/>MT0107: Die Assemblys\*' haben unterschiedliche benutzerdefinierte LLVM Optimierungen (\*), dies ist nicht zulässig, wenn sie alle in einem einzelnen Binärwert kompiliert werden.

### <a name="a-namemt0108mt0108-the-assembly-build-target--did-not-match-any-assemblies"></a><a name="MT0108"/>MT0108: Die Assembly Buildziel ' *' alle Assemblys stimmte nicht überein.

### <a name="a-namemt0109mt0109-the-assembly-0-was-loaded-from-a-different-path-than-the-provided-path-provided-path-1-actual-path-2"></a><a name="MT0109"/>MT0109: Die Assembly "{0}" wurde geladen von einem anderen Pfad als der angegebene Pfad (Pfad bereitgestellt: \ {1\}, der tatsächliche Pfad: \ {2\}).

Dies ist eine Warnung gibt an, dass eine Assembly verwiesen wird, von der Anwendung geladen von einem anderen Speicherort als angefordert wurde.

Dies kann bedeuten, dass die app mehrere Assemblys mit demselben Namen, jedoch von verschiedenen Standorten, was zu unerwarteten Ergebnissen führen können (nur die erste Assembly wird verwendet) verweist.

### <a name="a-namemt0110mt0110-incremental-builds-have-been-disabled-because-this-version-of-xamarinios-does-not-support-incremental-builds-in-projects-that-include-third-party-binding-libraries-and-that-compiles-to-bitcode"></a><a name="MT0110"/>MT0110: Inkrementelle Builds wurden deaktiviert, da diese Version von Xamarin.iOS nicht inkrementelle Builds in Projekten unterstützt, die Bibliotheken der Drittanbieter-Bindung enthalten, die in Bitcode kompiliert.

Inkrementelle Builds wurden deaktiviert, da diese Version von Xamarin.iOS nicht inkrementelle Builds in Projekten unterstützt, die Bibliotheken der Drittanbieter-Bindung enthalten, die in Bitcode (tvos. außerdem wurden und WatchOS Projekte) kompiliert.

Es ist keine Aktion Ihrerseits erforderlich, diese Meldung dient nur Informationszwecken.

Weitere Informationen finden Sie unter Fehler #[51710](https://bugzilla.xamarin.com/show_bug.cgi?id=51710).

Diese Warnung wird nicht mehr gemeldet.

### <a name="a-namemt0111mt0111-bitcode-has-been-enabled-because-this-version-of-xamarinios-does-not-support-building-watchos-projects-using-llvm-without-enabling-bitcode"></a><a name="MT0111"/>MT0111: Bitcode wurde aktiviert, weil diese Version von Xamarin.iOS Gebäude WatchOS nicht unterstützt Projekten mit LLVM ohne Bitcode aktivieren.

Bitcode wurde automatisch aktiviert, weil diese Version von Xamarin.iOS Gebäude WatchOS nicht unterstützt Projekten mit LLVM ohne Bitcode aktivieren.

Es ist keine Aktion Ihrerseits erforderlich, diese Meldung dient nur Informationszwecken.

Weitere Informationen finden Sie unter Fehler #[51634](https://bugzilla.xamarin.com/show_bug.cgi?id=51634).

### <a name="a-namemt0112mt0112-native-code-sharing-has-been-disabled-because-"></a><a name="MT0112"/>MT0112: Freigeben von systemeigenem Code wurde deaktiviert, da *

Es gibt mehrere Gründe dafür, dass Codefreigabe deaktiviert werden kann:

* Da die Container-app Bereitstellungsziel älter als iOS 8.0 oder höher ist (es hat *)).

Freigeben von systemeigenem Code erfordert iOS 8.0 oder höher da native Codefreigabe mit Benutzer-Frameworks implementiert wird eingeführt mit iOS 8.0 oder höher wurde.

* Da die Container-app I18N-Assemblys (*) enthält.

Freigeben von systemeigenem Code wird derzeit nicht unterstützt, wenn die Container app I18N-Assemblys enthält.

* Da die Container-app benutzerdefinierte XML-Definitionen für den verwalteten Linker (*) enthält.

Freigeben von systemeigenem Code erfordert, wird für Projekte, die benutzerdefinierte XML-Definitionen für den verwalteten Linker verwenden nicht unterstützt.

### <a name="a-namemt0113mt0113-native-code-sharing-has-been-disabled-for-the-extension--because-"></a><a name="MT0113"/>MT0113: Freigeben von systemeigenem Code wurde deaktiviert, für die Erweiterung "*" da *.

* Da die Container-app die Bitcode Optionen unterscheiden (\*) und die Erweiterung (\*).

  Freigeben von systemeigenem Code erfordert, dass die Optionen Bitcode zwischen den Projekten entsprechen, deren Code verwendet.

* Da die – Assembly-Build-Ziel Optionen unterscheiden sich zwischen dem Container-app (\*) und die Erweiterung (\*).

  Freigeben von systemeigenem Code erfordert, dass die – Assembly-Build-Ziel Optionen unterscheiden sich zwischen den Projekten, deren Code verwendet.

  Diese Bedingung kann auftreten, wenn inkrementelle builds nicht entweder aktiviert oder deaktiviert werden in allen Projekten.

* Da die I18N-Assemblys die Container-app unterschiedlich sind (\*) und die Erweiterung (\*).

  Freigeben von systemeigenem Code wird derzeit nicht für Erweiterungen unterstützt, die I18N-Assemblys enthalten.

* Da die Argumente für den Compiler AOT unterschieden zwischen der app Container sind (\*) und die Erweiterung (\*).

  Freigeben von systemeigenem Code erfordert, dass die Argumente für den Compiler AOT zwischen Projekten unterscheiden sich nicht, deren Code verwendet.

* Da sich die anderen Argumente an den Compiler AOT zwischen der app Container unterscheiden (\*) und die Erweiterung (\*).

  Freigeben von systemeigenem Code erfordert, dass die Argumente für den Compiler AOT zwischen Projekten unterscheiden sich nicht, deren Code verwendet.

  Diese Bedingung tritt auf, wenn die "32-Bit-Gleitkommawert dort sämtliche Vorgänge durchführen als 64-Bit-Gleitkommawert" zwischen den Projekten unterscheiden.

* Da LLVM nicht aktiviert oder die Container-app deaktiviert (\*) und die Erweiterung (\*).

  Freigeben von systemeigenem Code erfordert, dass LLVM entweder aktiviert oder deaktiviert für alle Projekte, deren Code verwendet.

* Da die verwalteten Linker-Einstellungen zwischen dem Container app unterscheiden (\*) und die Erweiterung (\*).

  Freigeben von systemeigenem Code erfordert, dass die verwalteten Linker-Einstellungen für alle Projekte identisch sind, deren Code verwendet.

* Da der übersprungenen Assemblys für den verwalteten Linker unterschieden zwischen der app Container sind (\*) und die Erweiterung (\*).

  Freigeben von systemeigenem Code erfordert, dass die verwalteten Linker-Einstellungen für alle Projekte identisch sind, deren Code verwendet.

* Da die Erweiterung eine benutzerdefinierte XML-Definitionen für den verwalteten Linker (*) aufweist.

  Freigeben von systemeigenem Code erfordert, wird für Projekte, die benutzerdefinierte XML-Definitionen für den verwalteten Linker verwenden nicht unterstützt.

* Da die Container-app für die ABI nicht erstellt werden * (während die Erweiterung für diese ABI aufbaut).

  Freigeben von systemeigenem Code erfordert, dass die Container-app für alle Architekturen erstellt für jede app-Erweiterung erstellt.

  Z. B.: Diese Bedingung tritt auf, wenn eine Erweiterung für ARM64 + ARMv7 erstellt, aber die Container-app ist nur für ARM64 erstellt.

* Da die Container-app für die ABI aufbaut \*, dies ist nicht kompatibel mit der Erweiterung ABI (\*).

  Freigeben von systemeigenem Code erfordert, dass alle Projekte für die genaue gleiche API zu erstellen.

  Z. B.: Diese Bedingung tritt auf, wenn eine Erweiterung für ARMv7 + Llvm + thumb2 erstellt, aber die Container-app ist nur für ARMv7 + Llvm erstellt.

* Da die Container-app auf die Assembly verweist "\*"from"\*", während die Erweiterung einer anderen Version von verweist auf "*".

  Freigeben von systemeigenem Code erfordert, dass alle Projekte, deren Code verwendet die gleichen Versionen für alle Assemblys verwenden.

<!-- MT0114: used by mmp -->

### <a name="a-namemt0115mt0115-it-is-recommended-to-reference-dynamic-symbols-using-code---dynamic-symbol-modecode-when-bitcode-is-enabled"></a><a name="MT0115"/>MT0115: Es wird empfohlen, dynamische Symbole, die mithilfe von Code zu verweisen (--Dynamic Symbolmodus = Code) Wenn Bitcode aktiviert ist.

Xamarin.iOS Projekte werden häufig systemeigene Symbole dynamisch verweisen was bedeutet, dass der systemeigene Linker diese systemeigene Symbole während der systemeigenen Verknüpfungsvorgang entfernen kann, da der systemeigene Linker nicht angezeigt wird, dass diese Symbole verwendet werden.

In der Regel fragt Xamarin.iOS den systemeigenen Linker diese Symbole beibehalten (mithilfe der `-u symbol` Linker-Flags), aber beim Kompilieren für Bitcode des systemeigenen Linkers akzeptiert keine der `-u` Flag.

Xamarin.iOS hat eine alternative Lösung implementiert: wir generiert zusätzliche systemeigenen Code, der diese Symbole verweist und somit der systemeigene Linker sehen, dass diese Symbole verwendet werden. Dies erfolgt automatisch beim Kompilieren in Bitcode.

Wenn `--dynamic-symbol-mode=linker` Mtouch diesem alternative Lösung wird deaktiviert und Xamarin.iOS versucht, übergeben Sie übergeben `-u` an den systemeigenen Linker. Dies führt wahrscheinlich zu systemeigenen Linkerfehler behandelt.

Die Lösung besteht darin, entfernen Sie die `--dynamic-symbol-mode=linker` Argument aus den Argumenten zusätzliche Mtouch in Buildoptionen für das Projekt.

<!-- 0116 - 0124: free to use -->

### <a name="a-namemt0116mt0116-invalid-architecture-arch-32-bit-architectures-are-not-supported-when-deployment-target-is-11-or-later-make-sure-the-project-does-not-build-for-a-32-bit-architecture"></a><a name="MT0116"/>MT0116: Ungültige Architektur: {Arch}. 32-Bit-Architekturen werden nicht unterstützt, wenn Bereitstellungsziel 11 oder höher ausgeführt wird. Stellen Sie sicher, dass das Projekt für eine 32-Bit-Architektur nicht erstellt werden.

iOS 11 enthält keine Unterstützung für 32-Bit-Anwendungen, sodass es nicht unterstützt wird, um für eine 32-Bit-Anwendung zu erstellen, wenn das Bereitstellungsziel entweder iOS 11 oder höher ist.

Ändern Sie die Zielarchitektur in das Projekt iOS Buildoptionen in arm64 oder ändern Sie das Bereitstellungsziel entweder in das Projekt "Info.plist" in einer früheren Version von iOS.

### <a name="a-namemt0117mt0117-cant-launch-a-32-bit-app-on-a-simulator-that-only-supports-64-bit"></a><a name="MT0117"/>MT0117: Kann keine 32-Bit-app auf einem Simulator starten, die nur 64 Bit unterstützt.

### <a name="a-namemt0118mt0118-aot-files-could-not-be-found-at-the-expected-directory-msymdir"></a><a name="MT0118"/>MT0118: Aot Dateien konnte nicht im erwarteten Verzeichnis "{Msymdir}" gefunden werden.

<!-- 0119 - 0123: free to use -->

### <a name="a-namemt0123mt0123-the-executable-assembly--does-not-reference-"></a><a name="MT0123"/>MT0123: Die ausführbare Assembly * verweist nicht auf *.

Kein Verweis auf die Plattformassembly gefunden (Xamarin.iOS.dll / Xamarin.TVOS.dll / Xamarin.WatchOS.dll) in der ausführbaren Assembly.

Dies tritt meistens dann in der ausführbaren Projekt, das nichts aus der Plattformassembly verwendet kein Code vorhanden ist eine leere Main-Methode (und kein anderer Code) würde z. B. diesen Fehler anzeigen:

```csharp
class Program {
    void Main (string[] args)
    {
    }
}
```

Mithilfe einer API aus dem Plattformassembly, wird der Fehler beheben:

```csharp
class Program {
    void Main (string[] args)
    {
        System.Console.WriteLine (typeof (UIKit.UIWindow));
    }
}
```

### <a name="a-namemt0124mt0124-could-not-set-the-current-language-to-lang-according-to-langlang-exception"></a><a name="MT0124"/>MT0124: Die aktuelle Sprache auf "{Lang}" konnte nicht festgelegt werden (gemäß LANG = {LANG}): {Ausnahme}

Dies ist eine Warnung, die angibt, dass die aktuelle Sprache der Sprache in der Fehlermeldung angegebene festgelegt werden konnte.

Die aktuelle Sprache standardmäßig Systemsprache.

### <a name="a-namemt0125mt0125-the---assembly-build-target-command-line-argument-is-ignored-in-the-simulator"></a><a name="MT0125"/>MT0125: Das – Assembly-Build-Ziel Befehlszeilenargument wird im Simulator ignoriert.

Es ist keine Aktion erforderlich, diese Meldung dient nur Informationszwecken.

### <a name="a-namemt0126mt0126-incremental-builds-have-been-disabled-because-incremental-builds-are-not-supported-in-the-simulator"></a><a name="MT0126"/>MT0126: Inkrementelle Builds wurden deaktiviert, da inkrementelle Builds nicht in der Simulator unterstützt werden.

Es ist keine Aktion erforderlich, diese Meldung dient nur Informationszwecken.

### <a name="a-namemt0127mt0127-incremental-builds-have-been-disabled-because-this-version-of-xamarinios-does-not-support-incremental-builds-in-projects-that-include-more-than-one-third-party-binding-libraries"></a><a name="MT0127"/>MT0127: Inkrementelle Builds wurden deaktiviert, da diese Version von Xamarin.iOS inkrementelle Builds in-Projekten nicht unterstützt, die mehr als eine Drittanbieter-Bindung Bibliotheken enthalten.

Inkrementelle Builds wurden automatisch deaktiviert, da diese Version von Xamarin.iOS nicht immer Projekte mit mehreren Bibliotheken der Drittanbieter-Bindung ordnungsgemäß erstellt wird.

Es ist keine Aktion erforderlich, diese Meldung dient nur Informationszwecken.

Weitere Informationen finden Sie unter Fehler #[52727](https://bugzilla.xamarin.com/show_bug.cgi?id=52727).

### <a name="a-namemt0128mt0128-could-not-touch-the-file--"></a><a name="MT0128"/>MT0128: Berühren die Datei konnte nicht ' *': *

Wenn eine Datei berühren (die ausgeführt wird, um sicherzustellen, dass teilweise Builds ordnungsgemäß ausgeführt werden), ist ein Fehler aufgetreten.

Diese Warnung kann wahrscheinlich ignoriert werden; Klicken Sie im Falle eines Probleme melden den Fehler (https://bugzilla.xamarin.com] (https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)), und es wird untersucht werden.

## <a name="mt1xxx-project-related-error-messages"></a>MT1xxx: Projektbezogene Fehlermeldungen

### <a name="mt10xx-installer--mtouch"></a>MT10xx: Installer / Mtouch

<!--
 MT1xxx file copy / symlinks (project related)
  MT10xx installer.cs / mtouch.cs
  -->

### <a name="a-namemt1001mt1001-could-not-find-an-application-at-the-specified-directory"></a><a name="MT1001"/>Eine Anwendung im angegebenen Verzeichnis MT1001 konnte nicht gefunden

### <a name="a-namemt1002mt1002-could-not-create-symlinks-files-were-copied"></a><a name="MT1002"/>Symbolische Verknüpfungen MT1002 konnte nicht erstellt werden, werden Dateien kopiert wurden

### <a name="a-namemt1003mt1003-could-not-kill-the-application--you-may-have-to-kill-the-application-manually"></a><a name="MT1003"/>MT1003 konnte die Anwendung nicht beenden "*". Möglicherweise wird die Anwendung manuell beenden.

### <a name="a-namemt1004mt1004-could-not-get-the-list-of-installed-applications"></a><a name="MT1004"/>Die Liste der installierten Anwendungen MT1004 konnte nicht abgerufen werden.

### <a name="a-namemt1005mt1005-could-not-kill-the-application--on-the-device----you-may-have-to-kill-the-application-manually"></a><a name="MT1005"/>MT1005 konnte die Anwendung nicht beenden "\*"auf dem Gerät"\*": *-Möglicherweise müssen Sie die Anwendung manuell beenden.

### <a name="a-namemt1006mt1006-could-not-install-the-application--on-the-device--"></a><a name="MT1006"/>Die Anwendung MT1006 konnte nicht installiert "\*"auf dem Gerät"\*": *.

### <a name="a-namemt1007mt1007-failed-to-launch-the-application--on-the-device---you-can-still-launch-the-application-manually-by-tapping-on-it"></a><a name="MT1007"/>MT1007 Fehler beim Starten die Anwendung "\*"auf dem Gerät"\*": *. Sie können die Anwendung immer noch durch Tippen auf darauf manuell starten.

### <a name="a-namemt1008mt1008-failed-to-launch-the-simulator"></a><a name="MT1008"/>MT1008: Fehler beim Starten des Simulators

Dieser Fehler wird gemeldet, wenn Mtouch fehlgeschlagen ist, um den Simulator zu starten.   Dies kann manchmal auftreten, da bereits ein veralteter oder dead Simulator-Prozess ausgeführt wird.

Der folgende Befehl in der Befehlszeile Unix ausgestellt kann verwendet werden, zum Beenden von Prozessen Simulator stattfindet:

```bash
$ launchctl list|grep UIKitApplication|awk '{print $3}'|xargs launchctl remove
```

### <a name="a-namemt1009mt1009-could-not-copy-the-assembly--to--"></a><a name="MT1009"/>Kopieren Sie MT1009 konnte nicht an die Assembly "\*'to'\*': *

Dies ist ein bekanntes Problem in bestimmten Versionen von Xamarin.iOS.

Versuchen Sie um Sie in diesem Fall die folgende problemumgehung:

```bash
sudo chmod 0644 /Library/Frameworks/Xamarin.iOS.framework/Versions/Current/lib/mono/*/*.mdb
```

Jedoch, da in der neuesten Version von Xamarin.iOS dieses Problem gelöst wurde, bitte Datei einen neuen Fehler am [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) mit der vollständigen Versionsinformationen und Buildausgabe-Protokoll.

### <a name="a-namemt1010mt1010-could-not-load-the-assembly--"></a><a name="MT1010"/>Lädt die Assembly MT1010 konnte nicht ' *': *

### <a name="a-namemt1011mt1011-could-not-add-missing-resource-file-"></a><a name="MT1011"/>Fehlende Ressourcendatei MT1011 konnte nicht hinzugefügt werden: ' *'

### <a name="a-namemt1012mt1012-failed-to-list-the-apps-on-the-device--"></a><a name="MT1012"/>MT1012 Liste der apps auf dem Gerät konnte nicht ' *': *

### <a name="a-namemt1013mt1013-dependency-tracking-error-no-files-to-compare-please-file-a-bug-report-at-httpbugzillaxamarincom-with-a-test-case"></a><a name="MT1013"/>MT1013 Abhängigkeitsnachverfolgung-Fehler: keine Dateien, verglichen werden soll. Bitte Datei einen Fehlerbericht an http://bugzilla.xamarin.com zu einem Testfall.

Hiermit wird einen Fehler im Xamarin.iOS. Melden Sie den Fehler auf [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) mit einem Test Caes.

### <a name="a-namemt1014mt1014-failed-to-re-use-cached-version-of--"></a><a name="MT1014"/>Zwischengespeicherte Version der erneuten Verwendung MT1014 konnte nicht ' *': *.

### <a name="a-namemt1015mt1015--failed-to-create-the-executable--"></a><a name="MT1015"/>MT1015 Fehler beim Erstellen der ausführbaren Datei "*": *

### <a name="a-namemt1015mt1015--failed-to-create-the-executable--"></a><a name="MT1015"/>MT1015 Fehler beim Erstellen der ausführbaren Datei "*": *

### <a name="a-namemt1016mt1016-failed-to-create-the-notice-file-because-a-directory-already-exists-with-the-same-name"></a><a name="MT1016"/>MT1016: Fehler beim NOTICE-Datei erstellt werden, da bereits ein Verzeichnis mit dem gleichen Namen vorhanden ist.

Entfernen Sie das Verzeichnis `NOTICE` aus dem Projekt.

### <a name="a-namemt1017mt1017-failed-to-create-the-notice-file-"></a><a name="MT1017"/>MT1017: Fehler beim Erstellen der SCHWARZEN-Datei: *.

### <a name="a-namemt1018mt1018-your-application-failed-code-signing-checks-and-could-not-be-installed-on-the-device--check-your-certificates-provisioning-profiles-and-bundle-ids-probably-your-device-is-not-part-of-the-selected-provisioning-profile-error-0xe8008015"></a><a name="MT1018"/>MT1018: Die Anwendung bei der Überprüfung Signieren von Code und konnte nicht installiert werden, auf dem Gerät "*". Überprüfen Sie die Zertifikate, Profile, Bereitstellung und Bündeln von Ids. Ihr Gerät ist wahrscheinlich nicht Teil der ausgewählten provisioning-Profil (Fehler: 0xe8008015).
### <a name="a-namemt1019mt1019-your-application-has-entitlements-not-supported-by-your-current-provisioning-profile-and-could-not-be-installed-on-the-device--please-check-the-ios-device-log-for-more-detailed-information-error-0xe8008016"></a><a name="MT1019"/>MT1019: Die Anwendung verfügt über Berechtigungen, die von Ihrem aktuellen Bereitstellungsprofil nicht unterstützt und konnte nicht installiert werden, auf dem Gerät "*". Überprüfen Sie die iOS-Gerät-Protokoll, detailliertere Informationen (Fehler: 0xe8008016).

Wenn dies:

* Die Anwendung verfügt über Berechtigungen, die das aktuelle Profil für die Bereitstellung nicht unterstützt.
  Folgende Lösungen sind möglich:
  - Geben Sie einen anderen Bereitstellungsprofil, das die Ansprüche unterstützt den Anforderungen der Anwendung.
  - Entfernen Sie die Berechtigungen, die in der aktuellen provisioning-Profil nicht unterstützt.
* Das Gerät, das Sie bereitstellen, ist nicht enthalten, in die provisioning-Profil, das Sie verwenden möchten.
  Folgende Lösungen sind möglich:
  - Erstellen Sie eine neue app aus einer Vorlage in Xcode, wählen Sie demselben provisioning-Profil aus und auf demselben Gerät bereitstellen. In einigen Fällen kann Xcode automatisch aktualisieren den bereitstellungsprofilen mit neuen Geräten (in anderen Fällen, die Xcode auszuführende Aktion aufgefordert werden).
  -Wechseln Sie zu der iOS Developer Center, und klicken Sie dann das Bereitstellungsprofil mit dem neuen Gerät aktualisieren dann das aktualisierte Bereitstellungsprofil auf Ihren Computer herunterladen.

In den meisten Fällen, die Weitere Informationen über den Fehler für den iOS-Geräteprotokoll gedruckt werden sollen, können die Diagnose des Problems.

### <a name="a-namemt1020mt1020-failed-to-launch-the-application--on-the-device--"></a><a name="MT1020"/>MT1020: Fehler beim Starten die Anwendung "\*"auf dem Gerät"\*": *

### <a name="a-namemt1021mt1021-could-not-copy-the-file--to--2"></a><a name="MT1021"/>MT1021: Konnte nicht kopieren Sie die Datei "\*'to'\*": \ {2\}

Eine Datei konnte nicht kopiert werden. Die Fehlermeldung aus der Kopiervorgang wurde Weitere Informationen zum Fehler.

### <a name="a-namemt1022mt1022-could-not-copy-the-directory--to--2"></a><a name="MT1022"/>MT1022: Konnte nicht kopiert werden das Verzeichnis "\*'to'\*": \ {2\}

Ein Verzeichnis konnte nicht kopiert werden. Die Fehlermeldung aus der Kopiervorgang wurde Weitere Informationen zum Fehler.

### <a name="a-namemt1023mt1023-could-not-communicate-with-the-device-to-find-the-application---"></a><a name="MT1023"/>MT1023: Konnte nicht mit dem Gerät zum Suchen nach der Anwendung kommunizieren ' *': *

Fehler beim Versuch, eine Anwendung auf dem Gerät zu suchen.

Um dieses Problem zu lösen versuchen Folgendes:

* Löschen Sie die Anwendung, von dem Gerät, und wiederholen Sie den Vorgang.
* Trennen Sie das Gerät, und verbinden Sie es erneut.
* Starten Sie das Gerät neu.
* Starten Sie den Mac neu.

### <a name="a-namemt1024mt1024-the-application-signature-could-not-be-verified-on-device--please-make-sure-that-the-provisioning-profile-is-installed-and-not-expired-error-0xe8008017"></a><a name="MT1024"/>MT1024: Die Anwendungssignatur konnte nicht auf Gerät überprüft werden "*". Stellen Sie sicher, dass das Bereitstellungsprofil installiert und nicht abgelaufen ist (Fehler: 0xe8008017).

Das Gerät abgewiesen. die Installation der Anwendung die Signatur nicht überprüft werden konnte.

Stellen Sie sicher, dass das Bereitstellungsprofil installiert und nicht abgelaufen ist.

### <a name="a-namemt1025mt1025-could-not-list-the-crash-reports-on-the-device-"></a><a name="MT1025"/>MT1025: Konnten nicht aufgelistet werden auf dem Gerät die Absturzberichte *.

Fehler beim Versuch, den Absturz (Crash) unterstellt sind, auf dem Gerät aufgelistet.

Um dieses Problem zu lösen versuchen Folgendes:

* Löschen Sie die Anwendung, von dem Gerät, und wiederholen Sie den Vorgang.
* Trennen Sie das Gerät, und verbinden Sie es erneut.
* Starten Sie das Gerät neu.
* Starten Sie den Mac neu.
* Synchronisieren Sie das Gerät mit iTunes (Dadurch werden alle Absturzberichte vom Gerät entfernt).

### <a name="a-namemt1026mt1026-could-not-download-the-crash-report--from-the-device-"></a><a name="MT1026"/>MT1026: Konnte nicht heruntergeladen werden der Bericht zum Systemabsturz erstellt * vom Gerät *.

Fehler beim Versuch, den Absturzberichte vom Gerät herunterladen.

Um dieses Problem zu lösen versuchen Folgendes:

* Löschen Sie die Anwendung, von dem Gerät, und wiederholen Sie den Vorgang.
* Trennen Sie das Gerät, und verbinden Sie es erneut.
* Starten Sie das Gerät neu.
* Starten Sie den Mac neu.
* Synchronisieren Sie das Gerät mit iTunes (Dadurch werden alle Absturzberichte vom Gerät entfernt).

### <a name="a-namemt1027mt1027-cant-use-xcode-7-to-launch-applications-on-devices-with-ios--xcode-7-only-supports-ios-6"></a><a name="MT1027"/>MT1027: Können Xcode 7 und höher verwenden, um die Anwendungen auf Geräten mit iOS starten * (Xcode 7 unterstützt nur für iOS 6 und höher).

Es ist nicht möglich, Xcode 7 und höher verwenden, um Anwendungen auf Geräten mit iOS-Version unter 6.0 zu starten.

Verwenden Sie eine ältere Version von Xcode, oder tippen Sie auf die app manuell, ihn zu starten.

### <a name="a-namemt1028mt1028-invalid-device-specification--expected-ios-watchos-or-all"></a><a name="MT1028"/>MT1028: Ungültiges Gerätespezifikation: "*". Erwartete "Ios", "Watchos" oder "all".

Die Device Specification übergeben verwenden – Gerät ist ungültig. Gültige Werte sind: "Ios", 'Watchos' oder 'all'.

### <a name="a-namemt1029mt1029-could-not-find-an-application-at-the-specified-directory-"></a><a name="MT1029"/>MT1029: Eine Anwendung im angegebenen Verzeichnis konnte nicht gefunden werden: *

Der Pfad der Anwendung an--Launchdev übergeben, ist nicht vorhanden. Geben Sie einen gültigen app-Bündel.

### <a name="a-namemt1030mt1030-launching-applications-on-device-using-a-bundle-identifier-is-deprecated-please-pass-the-full-path-to-the-bundle-to-launch"></a><a name="MT1030"/>MT1030: Starten von Anwendungen auf Gerät mit einem Paket-ID ist veraltet. Übergeben Sie den vollständigen Pfad, um das Paket zu starten.

Es wird empfohlen, den Pfad zu der app, starten Sie auf dem Gerät anstelle der Paket-Id übergeben.

### <a name="a-namemt1031mt1031-could-not-launch-the-app--on-the-device--because-the-device-is-locked-please-unlock-the-device-and-try-again"></a><a name="MT1031"/>MT1031: Konnte nicht starten Sie die app "\*"auf dem Gerät"\*", da das Gerät gesperrt ist. Entsperren Sie das Gerät, und wiederholen Sie den Vorgang.

Entsperren Sie das Gerät, und wiederholen Sie den Vorgang.

### <a name="a-namemt1032mt1032-this-application-executable-might-be-too-large--mb-to-execute-on-device-if-bitcode-was-enabled-you-might-want-to-disable-it-for-development-it-is-only-required-to-submit-applications-to-apple"></a><a name="MT1032"/>MT1032: Diese ausführbare Datei der Anwendung möglicherweise zu groß (* MB) auf Gerät auszuführen. Wenn Bitcode aktiviert war möglicherweise möchten ihn für die Entwicklung zu deaktivieren, wird nur benötigt, um Anwendungen an Apple senden.

### <a name="a-namemt1033mt1033-could-not-uninstall-the-application--from-the-device--"></a><a name="MT1033"/>MT1033: Konnte nicht deinstalliert werden die Anwendung "\*"vom Gerät"\*": *

<!-- 1034 used by mmp -->

### <a name="a-namemt1035mt1035-cannot-include-different-versions-of-the-framework-name"></a><a name="MT1035"/>MT1035: Kann nicht unterschiedliche Versionen des Frameworks "{Name}" enthalten.

Es ist nicht möglich, eine Verknüpfung mit verschiedenen Versionen des gleichen Frameworks.

Dies geschieht in der Regel auf, wenn eine Erweiterung eine andere Version von einem Framework als die Container-app (möglicherweise durch einen Drittanbieter-Assemblybindung) verweist.

Nach diesem Fehler vorhanden ist, mehrere [MT1036](#MT1036) Fehler, die die Pfade für jede andere Framework auflisten.

### <a name="a-namemt1036mt1036-framework-name-included-from-path-related-to-previous-error"></a><a name="MT1036"/>MT1036: Framework "{Name}" vom enthalten: {Pfad} (verknüpft mit vorherigen Fehler)

Dieser Fehler wird gemeldet, nur zusammen mit [MT1036](#MT1036). Finden Sie unter [MT1036](#MT1036) für Weitere Informationen.

### <a name="mt11xx-debug-service"></a>MT11xx: Debuggen-Diensts

<!--
  MT11xx DebugService.cs
  -->

### <a name="a-namemt1101mt1101-could-not-start-app"></a><a name="MT1101"/>App MT1101 konnte nicht gestartet werden

### <a name="a-namemt1102mt1102-could-not-attach-to-the-app-to-kill-it-"></a><a name="MT1102"/>MT1102 konnte nicht angefügt werden, zu der app (für ihn zu beenden): *

### <a name="a-namemt1103mt1103-could-not-detach"></a><a name="MT1103"/>MT1103 konnte nicht getrennt

### <a name="a-namemt1104mt1104-failed-to-send-packet-"></a><a name="MT1104"/>MT1104 Paket konnte nicht gesendet: *

### <a name="a-namemt1105mt1105-unexpected-response-type"></a><a name="MT1105"/>MT1105 unerwarteter Antworttyp

### <a name="a-namemt1106mt1106-could-not-get-list-of-applications-on-the-device-request-timed-out"></a><a name="MT1106"/>Liste der Anwendungen auf dem Gerät MT1106 konnte nicht abgerufen: Timeout.

### <a name="a-namemt1107mt1107-application-failed-to-launch-"></a><a name="MT1107"/>MT1107: Anwendung konnte nicht gestartet werden: *

Überprüfen Sie, ob das Gerät gesperrt wird.

Wenn Sie eine Unternehmens-app bereitstellen, oder verwenden ein kostenlose provisioning-Profil an, Sie müssen möglicherweise die Entwickler vertrauen (Dies wird erläutert <a href="http://stackoverflow.com/a/30726375/183422">hier</a>).

### <a name="a-namemt1108mt1108-could-not-find-developer-tools-for-this-xx-yy-device"></a><a name="MT1108"/>MT1108: Entwicklertools für dieses Gerät XX (JJ) wurde nicht gefunden werden.

Einige Vorgänge aus Mtouch erfordern die <tt>DeveloperDiskImage.dmg</tt> Datei vorhanden sein.   Diese Datei ist Teil der Xcode und befindet sich normalerweise relativ zum SDKS an, das Sie verwenden, um die berücksichtigt werden sollen, erstellen in der <tt>Xcode.app/Contents/Developer/iPhoneOS.platform/DeviceSupport/VERSION/DeveloperDiskImage.dmg</tt>.

Entweder kann dieser Fehler auftreten, da Sie nicht über eine DeveloperDiskImage.dmg verfügen, die das Gerät entspricht, das Sie eine Verbindung hergestellt haben.

### <a name="a-namemt1109mt1109-application-failed-to-launch-because-the-device-is-locked-please-unlock-the-device-and-try-again"></a><a name="MT1109"/>MT1109: Anwendung konnte nicht gestartet werden, da das Gerät gesperrt ist. Entsperren Sie das Gerät, und wiederholen Sie den Vorgang.

Überprüfen Sie, ob das Gerät gesperrt wird.

### <a name="a-namemt1110mt1110-application-failed-to-launch-because-of-ios-security-restrictions-please-ensure-the-developer-is-trusted"></a><a name="MT1110"/>MT1110: Anwendung konnte aufgrund von sicherheitseinschränkungen iOS gestartet. Stellen Sie sicher, dass der Entwickler als vertrauenswürdig eingestuft wird.

Wenn Sie eine Unternehmens-app bereitstellen, oder verwenden ein kostenlose provisioning-Profil an, Sie müssen möglicherweise die Entwickler vertrauen (Dies wird erläutert <a href="http://stackoverflow.com/a/30726375/183422">hier</a>).

### <a name="a-namemt1111mt1111-application-launched-successfully-but-its-not-possible-to-wait-for-the-app-to-exit-as-requested-because-its-not-possible-to-detect-app-termination-when-launching-using-gdbserver"></a><a name="MT1111"/>MT1111: Anwendung erfolgreich gestartet wurde, aber es ist nicht möglich, warten Sie, bis die app zu beenden, wie angefordert, da es nicht Beenden der app zu erkennen, wenn starten mit Gdbserver möglich ist.

### <a name="mt12xx-simulator"></a>MT12xx: Simulator

<!--
  MT12xx simcontroller.cs
  -->

### <a name="a-namemt1201mt1201-could-not-load-the-simulator-"></a><a name="MT1201"/>MT1201: Konnten nicht geladen werden den Simulator: *
### <a name="a-namemt1202mt1202-invalid-simulator-configuration-"></a><a name="MT1202"/>MT1202: Ungültige Simulator Konfiguration: *
### <a name="a-namemt1203mt1203-invalid-simulator-specification-"></a><a name="MT1203"/>MT1203: Ungültige Simulator Spezifikation: *
### <a name="a-namemt1204mt1204-invalid-simulator-specification--runtime-not-specified"></a><a name="MT1204"/>MT1204: Ungültige Simulator Spezifikation ' *': Common Language Runtime nicht angegeben.
### <a name="a-namemt1205mt1205-invalid-simulator-specification--device-type-not-specified"></a><a name="MT1205"/>MT1205: Ungültige Simulator Spezifikation ' *': nicht angegebenen Gerätetyp.
### <a name="a-namemt1206mt1206-could-not-find-the-simulator-runtime-"></a><a name="MT1206"/>MT1206: Die Simulator-Laufzeit nicht gefunden "*".
### <a name="a-namemt1207mt1207-could-not-find-the-simulator-device-type-"></a><a name="MT1207"/>MT1207: Gerätetyp, für den Simulator nicht gefunden "*".
### <a name="a-namemt1208mt1208-could-not-find-the-simulator-runtime-"></a><a name="MT1208"/>MT1208: Die Simulator-Laufzeit nicht gefunden "*".
### <a name="a-namemt1209mt1209-could-not-find-the-simulator-device-type-"></a><a name="MT1209"/>MT1209: Gerätetyp, für den Simulator nicht gefunden "*".
### <a name="a-namemt1210mt1210-invalid-simulator-specification--unknown-key-"></a><a name="MT1210"/>MT1210: Ungültige Simulator Spezifikation: \*, Unbekannter Schlüssel "\*"
### <a name="a-namemt1211mt1211-the-simulator-version--does-not-support-the-simulator-type-"></a><a name="MT1211"/>MT1211: Die Simulator-Version "\*"unterstützt nicht den Simulator-Typ"\*"
### <a name="a-namemt1212mt1212-failed-to-create-a-simulator-version-where-type---and-runtime--"></a><a name="MT1212"/>MT1212: Fehler beim Erstellen der Simulator-Version, in dem eingeben = * und = *.
### <a name="a-namemt1213mt1213-invalid-simulator-specification-for-xcode-4-"></a><a name="MT1213"/>MT1213: Ungültige Simulator Spezifikation für Xcode 4: *
### <a name="a-namemt1214mt1214-invalid-simulator-specification-for-xcode-5-"></a><a name="MT1214"/>MT1214: Ungültige Simulator Spezifikation für Xcode 5: *
### <a name="a-namemt1215mt1215-invalid-sdk-specified-"></a><a name="MT1215"/>MT1215: Ungültige SDK angegeben: *
### <a name="a-namemt1216mt1216-could-not-find-the-simulator-udid-"></a><a name="MT1216"/>MT1216: Simulator UDID nicht gefunden "*".
### <a name="a-namemt1217mt1217-could-not-load-the-app-bundle-at-"></a><a name="MT1217"/>MT1217: Konnten nicht geladen werden app-Bündel auf "*".
### <a name="a-namemt1218mt1218-no-bundle-identifier-found-in-the-app-at-"></a><a name="MT1218"/>MT1218: Kein Paket-ID gefunden wird, in der app auf "*".
### <a name="a-namemt1219mt1219-could-not-find-the-simulator-for-"></a><a name="MT1219"/>MT1219: Mit dem Simulator für nicht gefunden "*".
### <a name="a-namemt1220mt1220-could-not-find-the-latest-simulator-runtime-for-device-"></a><a name="MT1220"/>MT1220: Die neueste Simulator Common Language Runtime für das Gerät konnte nicht gefunden werden "*".

Dies gibt in der Regel ein Problem mit Xcode an.

Versuchen zur Behebung dieses Problems Folgendes:

* Verwenden Sie den Simulator einmal in Xcode.
* Übergeben Sie eine explizite SDK-Version, die mit--Sdk <version>.
* Installieren Sie Xcode neu.

### <a name="a-namemt1221mt1221-could-not-find-the-paired-iphone-simulator-for-the-watchos-simulator-"></a><a name="MT1221"/>MT1221: Den gepaarten iPhone-Simulator für den Simulator WatchOS wurde nicht gefunden werden "*".

Eine WatchOS-app in einem Simulator WatchOS starten zu können, muss eine gepaarte iOS-Simulator als auch vorhanden sein.

Sehen Sie sich Simulatoren mit iOS-Simulatoren, die über Xcodes Geräte Benutzeroberfläche gekoppelt werden können (Menü Fenster-Geräte >).

### <a name="mt13xx-linkwith"></a>MT13xx: [LinkWith]

<!--
  MT13xx [LinkWith]
  -->

### <a name="a-namemt1301mt1301-native-library---was-ignored-since-it-does-not-match-the-current-build-architectures-"></a><a name="MT1301"/>MT1301 systemeigene Bibliothek `*` (\*) wurde ignoriert, da sie nicht die aktuellen Build Architecture(s) übereinstimmt (\*)

### <a name="a-namemt1302mt1302-could-not-extract-the-native-library--from--please-ensure-the-native-library-was-properly-embedded-in-the-managed-assembly-if-the-assembly-was-built-using-a-binding-project-the-native-library-must-be-included-in-the-project-and-its-build-action-must-be-objcbindingnativelibrary"></a><a name="MT1302"/>Die systemeigene Bibliothek MT1302 konnte nicht extrahiert ' *' von '+'. Stellen Sie sicher, dass die systemeigene Bibliothek ordnungsgemäß in der verwalteten Assembly eingebettet waren (wenn die Assembly wurde mit einer bindungsprojekt erstellt, die systemeigene Bibliothek im Projekt enthalten sein muss, und seine Buildaktion muss "ObjcBindingNativeLibrary").

### <a name="a-namemt1303mt1303-could-not-decompress-the-native-framework--from--please-review-the-build-log-for-more-information-from-the-native-zip-command"></a><a name="MT1303"/>MT1303: Konnte nicht dekomprimiert werden die systemeigene Framework "\*"from"\*". Überprüfen Sie das Buildprotokoll Informationen aus dem systemeigenen "Zip"-Befehl.

Das angegebene systemeigene Framework aus der Bibliothek für die Bindung konnte nicht dekomprimiert werden.

Überprüfen Sie das Protokoll Bulid für Weitere Informationen zu diesem Fehler aus dem systemeigenen "Zip"-Befehl.

### <a name="a-namemt1304mt1304-the-embedded-framework--in--is-invalid-it-does-not-contain-an-infoplist"></a><a name="MT1304"/>MT1304: Das eingebettete Framework "*" in * ist ungültig: er enthält keine Datei "Info.plist".

Das angegebene eingebettete Framework enthält eine Datei "Info.plist" nicht und ist daher keinem gültigen Framework.

Stellen Sie sicher, dass das Framework gültig ist.

### <a name="a-namemt1305mt1305-the-binding-library--contains-a-user-framework--but-embedded-user-frameworks-require-ios-80-the-current-deployment-target-is--please-set-the-deployment-target-in-the-infoplist-file-to-at-least-80"></a><a name="MT1305"/>MT1305: Die Bindung-Bibliothek "\*" enthält ein Framework für Benutzer (\*), aber eingebettete Benutzer Frameworks erfordern iOS 8.0 oder höher (ist das aktuelle Bereitstellungsziel *). Legen Sie das Bereitstellungsziel entweder in der Datei "Info.plist" auf mindestens 8.0.

Die angegebene Bindung-Bibliothek enthält ein eingebettetes Framework, Xamarin.iOS unterstützt jedoch nur eingebettete Frameworks auf iOS 8.0 oder höher.

Legen Sie das Bereitstellungsziel entweder in der Datei "Info.plist" auf mindestens 8.0, um diesen Fehler zu beheben (oder verwenden Sie keine eingebettete Frameworks).

### <a name="mt14xx-crash-reports"></a>MT14xx: Absturzberichte

<!--
  MT14xx    CrashReports.cs
  -->

### <a name="a-namemt1400mt1400-could-not-open-crash-report-service-afcconnectionopen-returned-"></a><a name="MT1400"/>MT1400: Konnte nicht geöffnet werden Absturz Berichtsdienst: AFCConnectionOpen zurückgegeben *

Fehler beim Versuch, auf dem Gerät Absturzberichte zuzugreifen.

Um dieses Problem zu lösen versuchen Folgendes:

* Löschen Sie die Anwendung, von dem Gerät, und wiederholen Sie den Vorgang.
* Trennen Sie das Gerät, und verbinden Sie es erneut.
* Starten Sie das Gerät neu.
* Starten Sie den Mac neu.
* Synchronisieren Sie das Gerät mit iTunes (Dadurch werden alle Absturzberichte vom Gerät entfernt).

### <a name="a-namemt1401mt1401-could-not-close-crash-report-service-afcconnectionclose-returned-"></a><a name="MT1401"/>MT1401: Konnte nicht geschlossen Absturz Berichtsdienst: AFCConnectionClose zurückgegeben *

Fehler beim Versuch, auf dem Gerät Absturzberichte zuzugreifen.

Um dieses Problem zu lösen versuchen Folgendes:

* Löschen Sie die Anwendung, von dem Gerät, und wiederholen Sie den Vorgang.
* Trennen Sie das Gerät, und verbinden Sie es erneut.
* Starten Sie das Gerät neu.
* Starten Sie den Mac neu.
* Synchronisieren Sie das Gerät mit iTunes (Dadurch werden alle Absturzberichte vom Gerät entfernt).

### <a name="a-namemt1402mt1402-could-not-read-file-info-for--afcfileinfoopen-returned-"></a><a name="MT1402"/>MT1402: Konnte nicht gelesen werden Dateiinformationen für *: AFCFileInfoOpen zurückgegeben *

Fehler beim Versuch, auf dem Gerät Absturzberichte zuzugreifen.

Um dieses Problem zu lösen versuchen Folgendes:

* Löschen Sie die Anwendung, von dem Gerät, und wiederholen Sie den Vorgang.
* Trennen Sie das Gerät, und verbinden Sie es erneut.
* Starten Sie das Gerät neu.
* Starten Sie den Mac neu.
* Synchronisieren Sie das Gerät mit iTunes (Dadurch werden alle Absturzberichte vom Gerät entfernt).

### <a name="a-namemt1403mt1403-could-not-read-crash-report-afcdirectoryopen--returned-"></a><a name="MT1403"/>MT1403: Konnte nicht gelesen werden Bericht zum Systemabsturz erstellt: AFCDirectoryOpen (*) zurückgegeben: *

Fehler beim Versuch, auf dem Gerät Absturzberichte zuzugreifen.

Um dieses Problem zu lösen versuchen Folgendes:

* Löschen Sie die Anwendung, von dem Gerät, und wiederholen Sie den Vorgang.
* Trennen Sie das Gerät, und verbinden Sie es erneut.
* Starten Sie das Gerät neu.
* Starten Sie den Mac neu.
* Synchronisieren Sie das Gerät mit iTunes (Dadurch werden alle Absturzberichte vom Gerät entfernt).

### <a name="a-namemt1404mt1404-could-not-read-crash-report-afcfilerefopen--returned-"></a><a name="MT1404"/>MT1404: Konnte nicht gelesen werden Bericht zum Systemabsturz erstellt: AFCFileRefOpen (*) zurückgegeben: *

Fehler beim Versuch, auf dem Gerät Absturzberichte zuzugreifen.

Um dieses Problem zu lösen versuchen Folgendes:

* Löschen Sie die Anwendung, von dem Gerät, und wiederholen Sie den Vorgang.
* Trennen Sie das Gerät, und verbinden Sie es erneut.
* Starten Sie das Gerät neu.
* Starten Sie den Mac neu.
* Synchronisieren Sie das Gerät mit iTunes (Dadurch werden alle Absturzberichte vom Gerät entfernt).

### <a name="a-namemt1405mt1405-could-not-read-crash-report-afcfilerefread--returned-"></a><a name="MT1405"/>MT1405: Konnte nicht gelesen werden Bericht zum Systemabsturz erstellt: AFCFileRefRead (*) zurückgegeben: *

Fehler beim Versuch, auf dem Gerät Absturzberichte zuzugreifen.

Um dieses Problem zu lösen versuchen Folgendes:

* Löschen Sie die Anwendung, von dem Gerät, und wiederholen Sie den Vorgang.
* Trennen Sie das Gerät, und verbinden Sie es erneut.
* Starten Sie das Gerät neu.
* Starten Sie den Mac neu.
* Synchronisieren Sie das Gerät mit iTunes (Dadurch werden alle Absturzberichte vom Gerät entfernt).

### <a name="a-namemt1406mt1406-could-not-list-crash-reports-afcdirectoryopen--returned-"></a><a name="MT1406"/>MT1406: Konnten nicht aufgelistet werden Absturzberichte: AFCDirectoryOpen (*) zurückgegeben: *

Fehler beim Versuch, auf dem Gerät Absturzberichte zuzugreifen.

Um dieses Problem zu lösen versuchen Folgendes:

* Löschen Sie die Anwendung, von dem Gerät, und wiederholen Sie den Vorgang.
* Trennen Sie das Gerät, und verbinden Sie es erneut.
* Starten Sie das Gerät neu.
* Starten Sie den Mac neu.
* Synchronisieren Sie das Gerät mit iTunes (Dadurch werden alle Absturzberichte vom Gerät entfernt).

<!--- 1407 used by mmp -->

### <a name="mt16xx-macho"></a>MT16xx: MachO

<!--
  MT16xx    MachO.cs
  -->

### <a name="a-namemt1600mt1600-not-a-mach-o-dynamic-library-unknown-header-0x-"></a><a name="MT1600"/>MT1600: Nicht abstimmen-o dynamische Bibliothek (unbekannte Header "0 X *"): *.

Bei der Verarbeitung der betreffenden dynamische Bibliothek ist ein Fehler aufgetreten.

Stellen Sie sicher, dass die dynamische Bibliothek gültige Mach-o dynamische Bibliothek ist.

Das Format einer Bibliothek kann überprüft werden, mithilfe der `file` von Endstatus Befehl:

    file -arch all -l /path/to/library.dylib

### <a name="a-namemt1601mt1601-not-a-static-library-unknown-header--"></a><a name="MT1601"/>MT1601: Keine statische Bibliothek (unbekannte Header ' *'): *.

Fehler beim Verarbeiten der statischen Bibliothek fraglichen.

Stellen Sie sicher, dass die statische Bibliothek eine gültige statische Mach-O-Bibliothek ist.

Das Format einer Bibliothek kann überprüft werden, mithilfe der `file` von Endstatus Befehl:

    file -arch all -l /path/to/library.a

### <a name="a-namemt1602mt1602-not-a-mach-o-dynamic-library-unknown-header-0x-"></a><a name="MT1602"/>MT1602: Nicht abstimmen-o dynamische Bibliothek (unbekannte Header "0 X *"): *.

Bei der Verarbeitung der betreffenden dynamische Bibliothek ist ein Fehler aufgetreten.

Stellen Sie sicher, dass die dynamische Bibliothek gültige Mach-o dynamische Bibliothek ist.

Das Format einer Bibliothek kann überprüft werden, mithilfe der `file` von Endstatus Befehl:

    file -arch all -l /path/to/library.dylib

### <a name="a-namemt1603mt1603-unknown-format-for-fat-entry-at-position--in-"></a><a name="MT1603"/>MT1603: Unbekanntes Format für fat-Eintrags an Position * in *.

Fehler beim Verarbeiten der fraglichen fat-Archivs.

Stellen Sie sicher, dass das fat-Archiv gültig ist.

Das Format des ein fat-Archiv kann überprüft werden, mithilfe der `file` von Endstatus Befehl:

    file -arch all -l /path/to/file

### <a name="a-namemt1604mt1604-file-of-type--is-not-a-macho-file-"></a><a name="MT1604"/>MT1604: Datei vom Typ * ist keine MachO-Datei (*).

Fehler beim Verarbeiten der fraglichen MachO Datei.

Stellen Sie sicher, dass die Datei eine gültige Mach-o dynamische Bibliothek ist.

Das Format einer Datei kann überprüft werden, mithilfe der `file` von Endstatus Befehl:

    file -arch all -l /path/to/file

## <a name="mt2xxx-linker-error-messages"></a>MT2xxx: Linker-Fehlermeldungen

<!--
 MT2xxx Linker
  MT20xx Linker (general) errors
  -->

### <a name="a-namemt2001mt2001-could-not-link-assemblies"></a><a name="MT2001"/>Assemblys verknüpft MT2001 konnte nicht

Dieser Fehler konnte bedeutet, dass der verwaltete Linker einen unerwarteten Fehler, z. B. eine Ausnahme aufgetreten und nicht abgeschlossen oder speichern Sie die Assembly, die verarbeitet werden. Weitere Informationen zu den genauen Fehler wird Teil des Buildprotokolls z. B. sein.

```
error MT2001: Could not link assemblies.
    Method: `System.Void Todo.TodoListPageCS/<<-ctor>b__1_0>d::SetStateMachine(System.Runtime.CompilerServices.IAsyncStateMachine)`
    Assembly: `QuickTodo, Version=1.0.6297.28241, Culture=neutral, PublicKeyToken=null`
Reason: Value cannot be null.
Parameter name: instruction
```

Es ist wichtig, einen Fehlerbericht für solche Probleme Datei. In den meisten Fällen kann dieses Problem zu umgehen bereitgestellt werden, bis eine richtige Korrektur veröffentlicht wird. Die oben angegebenen Informationen unbedingt (zusammen mit einem Testfall und/oder die Assembly Binairy) zur Behebung des Problems zur Verfügung.

### <a name="a-namemt2002mt2002-can-not-resolve-reference-"></a><a name="MT2002"/>Verweis MT2002 kann nicht aufgelöst werden: *

### <a name="a-namemt2003mt2003-option--will-be-ignored-since-linking-is-disabled"></a><a name="MT2003"/>MT2003-Option ' *' wird ignoriert, da verknüpfen deaktiviert ist

### <a name="a-namemt2004mt2004-extra-linker-definitions-file--could-not-be-located"></a><a name="MT2004"/>Zusätzliche MT2004 Linker-Definitionsdatei ' *' konnte nicht gefunden werden.

### <a name="a-namemt2005mt2005-definitions-from--could-not-be-parsed"></a><a name="MT2005"/>MT2005 Definitionen von ' *' konnte nicht analysiert werden.

### <a name="a-namemt2006mt2006-can-not-load-mscorlibdll-from--please-reinstall-xamarinios"></a><a name="MT2006"/>MT2006: Kann nicht geladen werden "mscorlib.dll" aus: *. Installieren Sie den Xamarin.iOS.

Dies gibt im Allgemeinen, dass ein Problem mit Ihrer Installation Xamarin.iOS vorliegt. Bitte versuchen Sie es installieren von Xamarin.iOS.

<!--- 2007 used by mmp -->
<!--- 2009 used by mmp -->

### <a name="a-namemt2010mt2010-unknown-httpmessagehandler--valid-values-are-httpclienthandler-default-cfnetworkhandler-or-nsurlsessionhandler"></a><a name="MT2010"/>MT2010: Unbekannte HttpMessageHandler `*`. Gültige Werte sind HttpClientHandler (Standard), CFNetworkHandler oder NSUrlSessionHandler

### <a name="a-namemt2011mt2011-unknown-tlsprovider---valid-values-are-default-or-appletls"></a><a name="MT2011"/>MT2011: Unbekannte TlsProvider `*`.  Gültige Werte sind Default oder Appletls.

Der angegebene Wert auf `tls-provider=` wird kein gültiger TLS (Transport Layer Security)-Anbieter.

Die `default` und `appletls` sind die einzigen gültigen Werte und beide die gleiche Option, um die SSL/TLS unterstützt die systemeigene Apple-TLS-API verwenden darstellen.

<!--- 2012 used by mmp -->
<!--- 2013 used by mmp -->
<!--- 2014 used by mmp -->

### <a name="a-namemt2015mt2015-invalid-httpmessagehandler--for-watchos-the-only-valid-value-is-nsurlsessionhandler"></a><a name="MT2015"/>MT2015: Ungültige HttpMessageHandler `*` für WatchOS. Der einzige gültige Wert ist NSUrlSessionHandler.

Dies ist eine Warnung, die auftritt, da die Projektdatei ein ungültiger HttpMessageHandler angibt.

Frühere Versionen von unseren Tools Preview wird standardmäßig generiert einen ungültigen Wert in der Projektdatei.

Um diese Warnung zu beheben, öffnen Sie die Projektdatei in einem Text-Editor, und entfernen Sie alle HttpMessageHandler Knoten aus der XML.

### <a name="a-namemt2016mt2016-invalid-tlsprovider-legacy-option-the-only-valid-value-appletls-will-be-used"></a><a name="MT2016"/>MT2016: Ungültige TlsProvider `legacy` Option. Der einzige gültige Wert `appletls` verwendet werden.

Die `legacy` -Anbieter, der ein vollständig verwalteter SSLv3 wurde / einzigen TLSv1-Anbieter wird nicht mehr mit Xamarin.iOS ausgeliefert. Projekte, die diesem alten Anbieter verwenden, und erstellen Sie jetzt mit der neueren `appletls` eine.

Um diese Warnung zu beheben, öffnen Sie die Projektdatei in einem Text-Editor, und entfernen Sie alle "MtouchTlsProvider" aus der XML-Knoten.

### <a name="a-namemt2017mt2017-could-not-process-xml-description"></a><a name="MT2017"/>MT2017: XML-Beschreibung konnte nicht verarbeitet werden.

Dies bedeutet, dass ein Fehler aufgetreten ist, auf die [Linker für die benutzerdefinierte XML-Konfigurationsdatei](https://developer.xamarin.com/guides/cross-platform/advanced/custom_linking/) Sie angegeben haben, überprüfen Sie die Datei.

### <a name="a-namemt2018mt2018-the-assembly--is-referenced-from-two-different-locations--and-"></a><a name="MT2018"/>MT2018: Die Assembly "\*" wird von zwei unterschiedlichen Orten verwiesen: "\*" und "*".

Die Assembly, die in der Fehlermeldung genannten wird von mehreren Speicherorten geladen. Stellen Sie sicher, dass immer die gleiche Version einer Assembly verwenden.

### <a name="a-namemt2019mt2019-can-not-load-the-root-assembly-"></a><a name="MT2019"/>MT2019: Lädt die stammassembly kann nicht ' *'

Die stammassembly konnte nicht geladen werden. Stellen Sie sicher, dass der Pfad in der Fehlermeldung an eine vorhandene Datei verweist, und dass es eine gültige .NET-Assembly ist.

### <a name="a-namemt202xmt202x-binding-optimizer-failed-processing-"></a><a name="MT202x"/>MT202x: Bindung Optimierer Fehler bei Verarbeitung `...`.

Ein unerwartetes Ereignis aufgetreten beim Optimieren der generierte Code binden. Das Element, die Ursache des Problems heißt in der Fehlermeldung. Um dieses Problem zu beheben, die Assembly mit dem Namen (oder mit dem Typ oder Methode mit dem Namen) müssen bereitgestellt werden, in einer [Bericht](http://bugzilla.xamarin.com) zusammen mit einer vollständigen Buildprotokoll mit Ausführlichkeit aktiviert (d. h. `-v -v -v -v` in der **zusätzliche Mtouch Argumente**).

Die letzte Ziffer `x` werden:
* `0` für den Namen einer Assembly;
* `1` für einen Typnamen;
* `3` für einen Methodennamen;

### <a name="a-namemt2030mt2030-remove-user-resources-failed-processing-"></a><a name="MT2030"/>MT2030: Remove-Benutzerressourcen Fehler bei Verarbeitung `...`.

Unerwartete aufgetreten beim Versuch, Benutzerressourcen zu entfernen. Die Assembly, die Ursache des Problems heißt in der Fehlermeldung. Zur Behebung dieses Problems der Assemblys in zur Verfügung gestellt werden müssen eine [Bericht](http://bugzilla.xamarin.com) zusammen mit einer vollständigen Buildprotokoll mit Ausführlichkeit aktiviert (d. h. `-v -v -v -v` in der **zusätzliche Mtouch Argumente**).

Benutzerressourcen handelt es sich um Dateien, die in Assemblys (als Ressourcen), die extrahiert werden, zur Buildzeit Anwendungspaket Erstellen eingefügt. Dies umfasst Folgendes:

* `__monotouch_content_*` und `__monotouch_pages_*` Ressourcen und
* Systemeigene Bibliotheken, die in einer bindungsassembly eingebettet;

### <a name="a-namemt2040mt2040-default-httpmessagehandler-setter-failed-processing-"></a><a name="MT2040"/>MT2040: Standardeinstellung HttpMessageHandler Setter Fehler verarbeiten `...`.

Unerwartete trat beim Festlegen des standardmäßigen `HttpMessageHandler` für die Anwendung. Starten Sie bitte eine [Bericht](http://bugzilla.xamarin.com) zusammen mit einer vollständigen Buildprotokoll mit Ausführlichkeit aktiviert (d. h. `-v -v -v -v` in der **zusätzliche Mtouch Argumente**).

### <a name="a-namemt2050mt2050-code-remover-failed-processing-"></a><a name="MT2050"/>MT2050: Code Entfernungsprogramm Fehler bei Verarbeitung `...`.

Ein unerwartetes Ereignis aufgetreten beim Versuch, Code aus BCL Protokollversands mit der Anwendung entfernen. Starten Sie bitte eine [Bericht](http://bugzilla.xamarin.com) zusammen mit einer vollständigen Buildprotokoll mit Ausführlichkeit aktiviert (d. h. `-v -v -v -v` in der **zusätzliche Mtouch Argumente**).

### <a name="a-namemt2060mt2060-sealer-failed-processing-"></a><a name="MT2060"/>MT2060: Sealer Fehler bei Verarbeitung `...`.

Unerwartete beim Versuch, Typen oder Methoden (letzte) versiegeln, oder wenn einige Methoden devirtualizing aufgetreten ist. Die Assembly, die Ursache des Problems heißt in der Fehlermeldung. Zur Behebung dieses Problems der Assemblys in zur Verfügung gestellt werden müssen eine [Bericht](http://bugzilla.xamarin.com) zusammen mit einer vollständigen Buildprotokoll mit Ausführlichkeit aktiviert (d. h. `-v -v -v -v` in der **zusätzliche Mtouch Argumente**).

### <a name="a-namemt2070mt2070-metadata-reducer-failed-processing-"></a><a name="MT2070"/>MT2070: Metadaten reducerdatei an Fehler bei Verarbeitung `...`.

Ein unerwartetes Ereignis aufgetreten beim Versuch, die Metadaten aus der Anwendung zu reduzieren. Die Assembly, die Ursache des Problems heißt in der Fehlermeldung. Zur Behebung dieses Problems der Assemblys in zur Verfügung gestellt werden müssen eine [Bericht](http://bugzilla.xamarin.com) zusammen mit einer vollständigen Buildprotokoll mit Ausführlichkeit aktiviert (d. h. `-v -v -v -v` in der **zusätzliche Mtouch Argumente**).

### <a name="a-namemt2080mt2080-marknsobjects-failed-processing-"></a><a name="MT2080"/>MT2080: MarkNSObjects Fehler bei Verarbeitung `...`.

Ein unerwartetes Ereignis aufgetreten beim Versuch, markieren Sie `NSObject` Unterklassen aus der Anwendung. Die Assembly, die Ursache des Problems heißt in der Fehlermeldung. Zur Behebung dieses Problems der Assemblys in zur Verfügung gestellt werden müssen eine [Bericht](http://bugzilla.xamarin.com) zusammen mit einer vollständigen Buildprotokoll mit Ausführlichkeit aktiviert (d. h. `-v -v -v -v` in der **zusätzliche Mtouch Argumente**).

### <a name="a-namemt2090mt2090-inliner-failed-processing-"></a><a name="MT2090"/>MT2090: Inliners: Fehler bei Verarbeitung `...`.

Unerwartete aufgetreten beim Versuch, Inlinecode aus der Anwendung. Die Assembly, die Ursache des Problems heißt in der Fehlermeldung. Zur Behebung dieses Problems der Assemblys müssen in bereitgestellt werden, eine [Bericht](https://bugzilla.xamarin.com) zusammen mit einer vollständigen Buildprotokoll mit Ausführlichkeit aktiviert (d. h. `-v -v -v -v` in der **zusätzliche Mtouch Argumente**).

<!-- MT21xx: more linker errors -->

<!--- 2100 used by mmp -->

### <a name="a-namemt2100mt2100-smart-enum-conversion-preserver-failed-processing-"></a><a name="MT2100"/>MT2100: Intelligenten Enum Konvertierung damit Fehler bei Verarbeitung `...`.

Ein unerwartetes Ereignis aufgetreten beim Versuch, markieren Sie die Konvertierungsmethoden für intelligente Enumerationen aus der Anwendung. Die Assembly, die Ursache des Problems heißt in der Fehlermeldung. Zur Behebung dieses Problems der Assemblys müssen in bereitgestellt werden, eine [Bericht](https://bugzilla.xamarin.com) zusammen mit einer vollständigen Buildprotokoll mit Ausführlichkeit aktiviert (d. h. `-v -v -v -v` in der **zusätzliche Mtouch Argumente**).

### <a name="a-namemt2101mt2101-cant-resolve-the-reference--referenced-from-the-method--in-"></a><a name="MT2101"/>MT2101: Den Verweis kann nicht aufgelöst werden "\*", auf die verwiesen wird von der Methode"\*" in "*".

Beim Verarbeiten der Methode, die in der Fehlermeldung genannten ist ein ungültiger Assemblyverweis aufgetreten.

Die Assembly, die Ursache des Problems heißt in der Fehlermeldung. Zur Behebung dieses Problems der Assemblys in zur Verfügung gestellt werden müssen eine [Bericht](https://bugzilla.xamarin.com) zusammen mit einer vollständigen Buildprotokoll mit Ausführlichkeit aktiviert (d. h. `-v -v -v -v` in der **zusätzliche Mtouch Argumente**).

### <a name="a-namemt2102mt2102-error-processing-the-method--in-the-assembly--"></a><a name="MT2102"/>MT2102: Fehler bei der Verarbeitung der Methodennamens "\*"in der Assembly"\*": *

Ein unerwartetes Ereignis aufgetreten beim Versuch, markieren Sie die Methode, die in der Fehlermeldung genannten.

Die Assembly, die Ursache des Problems heißt in der Fehlermeldung. Zur Behebung dieses Problems der Assemblys in zur Verfügung gestellt werden müssen eine [Bericht](https://bugzilla.xamarin.com) zusammen mit einer vollständigen Buildprotokoll mit Ausführlichkeit aktiviert (d. h. `-v -v -v -v` in der **zusätzliche Mtouch Argumente**).

### <a name="a-namemt2103mt2103-error-processing-assembly--"></a><a name="MT2103"/>MT2103: Fehler beim Verarbeiten der Assembly "\*": *

Unerwarteter Fehler beim Verarbeiten einer Assemblys.

Die Assembly, die Ursache des Problems heißt in der Fehlermeldung. Zur Behebung dieses Problems der Assemblys müssen in bereitgestellt werden, eine [Bericht](https://bugzilla.xamarin.com) zusammen mit einer vollständigen Buildprotokoll mit Ausführlichkeit aktiviert (d. h. `-v -v -v -v` in der **zusätzliche Mtouch Argumente**).

### <a name="a-namemt2104mm2104-unable-to-link-assembly-0-as-it-is-mixed-mode"></a><a name="MT2104"/>MM2104: Kann nicht auf die Assembly "{0}" zu verknüpfen, wenn er im gemischten Modus ist.

Gemischte Assemblys können nicht vom Linker verarbeitet werden.

Https://msdn.microsoft.com/en-us/library/x0w2664k.aspx für Weitere Informationen finden Sie im gemischten Modus Assemblys.

## <a name="mt3xxx-aot-error-messages"></a>MT3xxx: AOT-Fehlermeldungen

<!--
 MT3xxx AOT
  MT30xx AOT (general) errors
  -->

### <a name="a-namemt3001mt3001-could-not-aot-the-assembly-"></a><a name="MT3001"/>MT3001 konnte nicht AOT der Assembly "*"

Dies gibt im Allgemeinen einen Fehler im Compiler AOT. Um einen Fehlerbericht [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) mit einem Projekt, das zum Reproduzieren des Fehlers verwendet werden kann.

Manchmal ist es möglich, dieses Problem umgehen durch inkrementelle Builds in das Projekt iOS-Build-Option deaktivieren (wird jedoch weiterhin einen Fehler so melden Sie es trotzdem).

### <a name="a-namemt3002mt3002-aot-restriction-method--must-be-static-since-it-is-decorated-with-monopinvokecallback-see-httpsdeveloperxamarincomguidesiosadvancedtopicslimitationsreversecallbackshttpsdeveloperxamarincomguidesiosadvancedtopicslimitationsreversecallbacks"></a><a name="MT3002"/>MT3002 AOT Einschränkung: Methode ' *' muss statisch sein, da er mit [MonoPInvokeCallback] ergänzt wird. Finden Sie unter [https://developer.xamarin.com/guides/ios/advanced_topics/limitations/#Reverse_Callbacks](https://developer.xamarin.com/guides/ios/advanced_topics/limitations/#Reverse_Callbacks)

Diese Fehlermeldung stammen aus der AOT-Compiler.

### <a name="a-namemt3003mt3003-conflicting---debug-and---llvm-options-soft-debugging-is-disabled"></a><a name="MT3003"/>MT3003 In Konflikt stehenden----Llvm Optionen und Debuggen. Soft-Debuggen ist deaktiviert.

Debuggen wird nicht unterstützt, wenn die LLVM aktiviert ist. Wenn Sie die app zu debuggen möchten, deaktivieren Sie zuerst LLVM.

### <a name="a-namemt3004mt3004-could-not-aot-the-assembly--because-it-doesnt-exist"></a><a name="MT3004"/>MT3004 konnte nicht AOT der Assembly ' *', da er nicht vorhanden ist.

### <a name="a-namemt3005mt3005-the-dependency--of-the-assembly--was-not-found-please-review-the-projects-references"></a><a name="MT3005"/>MT3005 Die Abhängigkeit "\*"der Assembly"\*" wurde nicht gefunden. Überprüfen Sie den Verweisen des Projekts an.

Dies tritt normalerweise auf, wenn eine andere Version einer Plattform-Assembly (in der Regel die .NET 4-Version von "mscorlib.dll") auf eine Assembly verweist.

Dies wird nicht unterstützt und möglicherweise nicht erstellen oder die Ausführung ordnungsgemäß (die Assembly möglicherweise API aus dem .NET 4-Version von "mscorlib.dll", die die Xamarin.iOS-Version nicht verwenden).

### <a name="a-namemt3006mt3006-could-not-compute-a-complete-dependency-map-for-the-project-this-will-result-in-slower-build-times-because-xamarinios-cant-properly-detect-what-needs-to-be-rebuilt-and-what-does-not-need-to-be-rebuilt-please-review-previous-warnings-for-more-details"></a><a name="MT3006"/>Eine vollständige Zuordnung für das Projekt MT3006 konnte nicht berechnet werden. Dies führt zu langsamer Builderstellung beschleunigen, da Xamarin.iOS korrekt erkennen kann, was neu erstellt werden muss (und was nicht neu erstellt werden muss). Überprüfen Sie vorherige Warnungen, die weitere Details an.

 Erstellen oder ordnungsgemäß ausführen (die Assembly möglicherweise API aus dem .NET 4-Version von "mscorlib.dll", die die Xamarin.iOS-Version nicht verwenden).

### <a name="a-namemt3007mt3007-debug-info-files-mdb-will-not-be-loaded-when-llvm-is-enabled"></a><a name="MT3007"/>MT3007: Debug-Info-Dateien (*.mdb) werden nicht geladen werden, wenn Llvm aktiviert ist.

### <a name="a-namemt3008mt3008-bitcode-support-requires-the-use-of-the-llvm-aot-backend---llvm"></a><a name="MT3008"/>MT3008: Bitcode erfordert die Verwendung von LLVM AOT Back-End (--Llvm)

Bitcode erfordert die Verwendung von LLVM AOT Back-End (--Llvm).

Deaktivieren Sie Bitcode Unterstützung oder aktivieren Sie LLVM.

<!--- 3009 used by mmp -->
<!--- 3010 used by mmp -->

## <a name="mt4xxx-code-generation-error-messages"></a>MT4xxx: Code Generation-Fehlermeldungen

### <a name="mt40xx-main"></a>MT40xx: Main

<!--
 MT4xxx code generation
  MT40xx main.m
  -->

### <a name="a-namemt4001mt4001-the-main-template-could-not-be-expanded-to-"></a><a name="MT4001"/>MT4001 Die Haupt-Vorlage konnte nicht auf erweitert werden `*`.

Fehler beim Generieren der main.m. Melden Sie den Fehler auf [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt4002mt4002-failed-to-compile-the-generated-code-for-pinvoke-methods-please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT4002"/>MT4002 Fehler beim Kompilieren des generierten Codes für P/Invoke-Methoden. Bitte der Datei eines Fehlerberichts an http://bugzilla.xamarin.com

Fehler beim Kompilieren des generierten Codes für P/Invoke-Methoden. Bitte der Datei eines Fehlerberichts an [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="mt41xx-registrar"></a>MT41xx: Registrierungsstelle

<!--
  MT41xx registrar.m
  -->

### <a name="a-namemt4101mt4101-the-registrar-cannot-build-a-signature-for-type-"></a><a name="MT4101"/>MT4101 Kann nicht die Registrierungsstelle eine Signatur für den Typ erstellen `*`.

Ein Typ in exportierten API gefunden, die nicht weiß wie in Objective-c gemarshallt werden sollen, die Common Language runtime

Wenn Sie glauben, Xamarin.iOS sollte den betreffende Typ unterstützen, bitte Datei eine Verbesserung-Anforderung auf dem [http://bugzilla.xamarin.com](http://bugzilla.xamarin.com).

### <a name="a-namemt4102mt4102-the-registrar-found-an-invalid-type--in-signature-for-method--use--instead"></a><a name="MT4102"/>MT4102 Die Registrierungsstelle wurde einen ungültigen Typ gefunden `*` in Signatur für die Methode `*`. Verwenden Sie stattdessen `*`.

Dies derzeit nur erfolgt über einen Typ: System.DateTime. Verwenden Sie stattdessen die entsprechende Objective-C (NSDate).

### <a name="a-namemt4103mt4103-the-registrar-found-an-invalid-type--in-signature-for-method--the-type-implements-inativeobject-but-does-not-have-a-constructor-that-takes-two-intptr-bool-arguments"></a><a name="MT4103"/>MT4103 Die Registrierungsstelle wurde einen ungültigen Typ gefunden. `*` in Signatur für die Methode `*`: der Typ implementiert INativeObject, jedoch verfügt nicht über einen Konstruktor, der zwei akzeptiert (IntPtr, Bool) Argumente

Dies tritt auf, wenn die Registrierungsstelle treffen, einen Typ in einer Signatur mit den genannten Eigenschaften. Die Registrierungsstelle müssen eventuell neue Instanzen des Typs erstellen und in diesem Fall erfordert einen Konstruktor mit (IntPtr, Bool) Signatur - gibt das erste Argument (IntPtr) während der zweite, wenn der Aufrufer über den Besitz der systemeigenen übergibt das verwaltete Handle Behandeln Sie (Wenn dieser Wert "false" ist, "beibehalten" für das Objekt aufgerufen werden).

### <a name="a-namemt4104mt4104-the-registrar-cannot-marshal-the-return-value-for-type--in-signature-for-method-"></a><a name="MT4104"/>MT4104 Der Registrierungsstelle nicht für den Typ den Rückgabewert Marshallen `*` in Signatur für die Methode `*`.

Ein Typ in exportierten API gefunden, die nicht weiß wie in Objective-c gemarshallt werden sollen, die Common Language runtime

Wenn Sie glauben, Xamarin.iOS sollte den betreffende Typ unterstützen, bitte Datei eine Verbesserung-Anforderung auf dem [http://bugzilla.xamarin.com](http://bugzilla.xamarin.com).

### <a name="a-namemt4105mt4105-the-registrar-cannot-marshal-the-parameter-of-type--in-signature-for-method-"></a><a name="MT4105"/>MT4105 Der Registrierungsstelle nicht marshallen den Parameter vom Typ `*` in Signatur für die Methode `*`.

Wenn Sie glauben, Xamarin.iOS sollte den betreffende Typ unterstützen, bitte Datei eine Verbesserung-Anforderung auf dem [http://bugzilla.xamarin.com](http://bugzilla.xamarin.com).

### <a name="a-namemt4106mt4106-the-registrar-cannot-marshal-the-return-value-for-structure--in-signature-for-method-"></a><a name="MT4106"/>MT4106 Der Registrierungsstelle nicht marshallen den Rückgabewert für Struktur `*` in Signatur für die Methode `*`.

Ein Typ in exportierten API gefunden, die nicht weiß wie in Objective-c gemarshallt werden sollen, die Common Language runtime

Wenn Sie glauben, Xamarin.iOS sollte den betreffende Typ unterstützen, bitte Datei eine Verbesserung-Anforderung auf dem [http://bugzilla.xamarin.com](http://bugzilla.xamarin.com).

### <a name="a-namemt4107mt4107-the-registrar-cannot-marshal-the-parameter-of-type--in-signature-for-method-"></a><a name="MT4107"/>MT4107 Der Registrierungsstelle nicht marshallen den Parameter vom Typ `*` in Signatur für die Methode `+`.

Ein Typ in exportierten API gefunden, die nicht weiß wie in Objective-c gemarshallt werden sollen, die Common Language runtime

Wenn Sie glauben, Xamarin.iOS sollte den betreffende Typ unterstützen, bitte Datei eine Verbesserung-Anforderung auf dem [http://bugzilla.xamarin.com](http://bugzilla.xamarin.com).

### <a name="a-namemt4108mt4108-the-registrar-cannot-get-the-objectivec-type-for-managed-type-"></a><a name="MT4108"/>MT4108 Die Registrierungsstelle kann nicht den Typ der ObjectiveC für verwalteten Typ abgerufen `*`.

Ein Typ in exportierten API gefunden, die nicht weiß wie in Objective-c gemarshallt werden sollen, die Common Language runtime

Wenn Sie glauben, Xamarin.iOS sollte den betreffende Typ unterstützen, bitte Datei eine Verbesserung-Anforderung auf dem [http://bugzilla.xamarin.com](http://bugzilla.xamarin.com).

### <a name="a-namemt4109mt4109-failed-to-compile-the-generated-registrar-code-please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT4109"/>MT4109 Fehler beim Kompilieren des Codes generierte Registrierungsstelle. Bitte der Datei eines Fehlerberichts an http://bugzilla.xamarin.com

Fehler beim Kompilieren des generierten Codes für die Registrierungsstelle. Das Buildprotokoll wird die Ausgabe aus dem systemeigenen Compiler, die erläutern, warum Kompilieren des Codes ist nicht enthalten.

Dies ist immer ein Fehler im Xamarin.iOS; Bitte der Datei eines Fehlerberichts an [http://bugzilla.xamarin.com](http://bugzilla.xamarin.com) mit Ihrem Projekt oder einen Testfall.

### <a name="a-namemt4110mt4110-the-registrar-cannot-marshal-the-out-parameter-of-type--in-signature-for-method-"></a><a name="MT4110"/>MT4110 Der Registrierungsstelle nicht Marshallen der Out-Parameter des Typs `*` in Signatur für die Methode `*`.

### <a name="a-namemt4111mt4111-the-registrar-cannot-build-a-signature-for-type--in-method-"></a><a name="MT4111"/>MT4111 Kann nicht die Registrierungsstelle eine Signatur für den Typ erstellen `*` in Methode `*`.

### <a name="a-namemt4112mt4112-the-registrar-found-an-invalid-type--registering-generic-types-with-objective-c-is-not-supported-and-may-lead-to-random-behavior-andor-crashes-for-backwards-compatibility-with-older-versions-of-xamarinios-it-is-possible-to-ignore-this-error-by-passing---unsupported--enable-generics-in-registrar-as-an-additional-mtouch-argument-in-the-projects-ios-build-options-page-see-developerxamarincomguidesiosadvancedtopicsregistrarhttpsdeveloperxamarincomguidesiosadvancedtopicsregistrar-for-more-information"></a><a name="MT4112"/>MT4112 Die Registrierungsstelle wurde einen ungültigen Typ gefunden `*`. Registrieren von generischen Typen mit Objective-C wird nicht unterstützt und führt möglicherweise zu zufälliges Verhalten und/oder Abstürze (für Abwärtskompatibilität Kompatibilität mit älteren Versionen von Xamarin.iOS ist es möglich, diesen Fehler zu ignorieren, indem Sie übergeben `--unsupported--enable-generics-in-registrar` als eine zusätzliche Mtouch Argument in das Projekt iOS-Build-Optionsseite. Finden Sie unter [developer.xamarin.com/guides/ios/advanced_topics/registrar](https://developer.xamarin.com/guides/ios/advanced_topics/registrar) für Weitere Informationen).

### <a name="a-namemt4113mt4113-the-registrar-found-a-generic-method--exporting-generic-methods-is-not-supported-and-will-lead-to-random-behavior-andor-crashes"></a><a name="MT4113"/>MT4113-Die Registrierungsstelle eine generische Methode gefunden: "\*.\*". Exportieren von generischen Methoden wird nicht unterstützt und führt zum zufälliges Verhalten und/oder abstürzen.

### <a name="a-namemt4114mt4114-unexpected-error-in-the-registrar-for-the-method----please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT4114"/>MT4114 unerwarteter Fehler bei der Registrierung für die Methode "\*.\*"-Datei geben einen Fehlerbericht an http://bugzilla.xamarin.com

### <a name="a-namemt4116mt4116-could-not-register-the-assembly--"></a><a name="MT4116"/>MT4116 konnte nicht registriert werden die Assembly ' *': *

### <a name="a-namemt4117mt4117-the-registrar-found-a-signature-mismatch-in-the-method----the-selector-indicates-the-method-takes--parameters-while-the-managed-method-has--parameters"></a><a name="MT4117"/>MT4117 Gefunden die Registrierungsstelle eine Nichtübereinstimmung der Datenträgersignatur des in der Methode "*.*' – gibt die Auswahl an die Methode nimmt * Parameter, während die verwaltete Methode hat * Parameter.

### <a name="a-namemt4118mt4118-cannot-register-two-managed-types--and--with-the-same-native-name-"></a><a name="MT4118"/>Zwei verwaltete Typen MT4118 kann nicht registriert werden kann ("\*"und"\*") mit dem gleichen Namen für die systemeigene ("*").

### <a name="a-namemt4119mt4119-could-not-register-the-selector--of-the-member--because-the-selector-is-already-registered-on-a-different-member"></a><a name="MT4119"/>Den Selektor MT4119 konnte nicht registriert "\*"des Elements"\*. *", da die Auswahl auf ein anderes Element bereits registriert ist.

### <a name="a-namemt4120mt4120-the-registrar-found-an-unknown-field-type--in-field--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT4120"/>MT4120 Die Registrierungsstelle gefunden einen Unbekannter Feldtyp '\*"im Feld"\*. * ". Bitte der Datei eines Fehlerberichts an http://bugzilla.xamarin.com

Dieser Fehler zeigt einen Fehler im Xamarin.iOS an. Bitte der Datei eines Fehlerberichts an [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt4121mt4121-cannot-use-gccg-to-compile-the-generated-code-from-the-static-registrar-when-using-the-accounts-framework-the-header-files-provided-by-apple-used-during-the-compilation-require-clang-either-use-clang---compilerclang-or-the-dynamic-registrar---registrardynamic"></a><a name="MT4121"/>MT4121 können keine GCC / G ++, um den generierten Code von der statischen Registrierungsstelle zu kompilieren, nach der Verwendung von Konten Framework (Header-Dateien, die von Apple, die während der Kompilierung verwendeten bereitgestellten erfordern Clang). Verwenden Sie entweder Clang (--Compiler: Clang) oder die dynamische Registrierungsstelle (--Registrierungsstelle: dynamische).

### <a name="a-namemt4122mt4122-cannot-use-the-clang-compiler-provided-in-the--sdk-to-compile-the-generated-code-from-the-static-registrar-when-non-ascii-type-names--are-present-in-the-application-either-use-gccg---compilergccg-the-dynamic-registrar---registrardynamic-or-a-newer-sdk"></a><a name="MT4122"/>MT4122 können keine Clang Compiler gemäß der *.* SDK zum Kompilieren des generierten Codes von der statischen Registrierungsstelle bei nicht-ASCII-Typnamen ("*") in der Anwendung vorhanden sind. Verwenden Sie entweder GCC / G ++ (--Compiler: Gcc | g ++), die dynamische Registrierungsstelle (--Registrierungsstelle: dynamische) oder eine neuere SDK.

### <a name="a-namemt4123mt4123-the-type-of-the-variadic-parameter-in-the-variadic-function--must-be-systemintptr"></a><a name="MT4123"/>MT4123 Den Typ des Parameters Variadic in Variadic-Funktion "*" System.IntPtr werden muss.

### <a name="a-namemt4124mt4124-invalid--found-on--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT4124"/>Ungültiger MT4124 * finden Sie auf "*". Bitte der Datei eines Fehlerberichts an http://bugzilla.xamarin.com

Dieser Fehler zeigt einen Fehler im Xamarin.iOS an. Bitte der Datei eines Fehlerberichts an [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt4125mt4125-the-registrar-found-an-invalid-type--in-signature-for-method--the-interface-must-have-a-protocol-attribute-specifying-its-wrapper-type"></a><a name="MT4125"/>MT4125 Die Registrierungsstelle wurde einen ungültigen Typ gefunden. "\*"in der Signatur für die Methode"\*": die Schnittstelle benötigen eine Protocol-Attributs, dessen Wrappertyp angibt.

### <a name="a-namemt4126mt4126-cannot-register-two-managed-protocols--and--with-the-same-native-name-"></a><a name="MT4126"/>Zwei verwalteten Protokolle MT4126 kann nicht registriert werden kann ("\*"und"\*") mit dem gleichen Namen für die systemeigene ("*").

### <a name="a-namemt4127mt4127-cannot-register-more-than-one-interface-method-for-the-method--which-is-implementing-"></a><a name="MT4127"/>MT4127 kann mehr als eine Methode der Schnittstelle für die Methode nicht registrieren "\*" (die Implementierung ist "\*").

### <a name="a-namemt4128mt4128--the-registrar-found-an-invalid-generic-parameter-type--in-the-method--the-generic-parameter-must-have-an-nsobject-constraint"></a><a name="MT4128"/>MT4128 Die Registrierungsstelle wurde einen Ungültiger generischen Parametertyp gefunden "\*"in der Methode"\*". Der generische Parameter benötigen eine 'NSObject'-Einschränkung.

### <a name="a-namemt4129mt4129--the-registrar-found-an-invalid-generic-return-type--in-the-method--the-generic-return-type-must-have-an-nsobject-constraint"></a><a name="MT4129"/>MT4129 Die Registrierungsstelle gefunden ein ungültiges Rückgabetyps generische "\*"in der Methode"\*". Der generische Rückgabetyp muss eine 'NSObject'-Einschränkung aufweisen.

### <a name="a-namemt4130mt4130--the-registrar-cannot-export-static-methods-in-generic-classes-"></a><a name="MT4130"/>MT4130 Die Registrierungsstelle kann nicht statische Methoden in generischen Klassen exportieren ("*").

### <a name="a-namemt4131mt4131--the-registrar-cannot-export-static-properties-in-generic-classes-"></a><a name="MT4131"/>MT4131 Kann nicht die Registrierungsstelle statische Eigenschaften von generischen Klassen exportieren ("\*.\*").

### <a name="a-namemt4132mt4132--the-registrar-found-an-invalid-generic-return-type--in-the-property--the-return-type-must-have-an-nsobject-constraint"></a><a name="MT4132"/>MT4132 Die Registrierungsstelle gefunden ein ungültiges Rückgabetyps generische "\*"in der Eigenschaft"\*". Der Rückgabetyp muss eine 'NSObject'-Einschränkung aufweisen.

### <a name="a-namemt4133mt4133--cannot-construct-an-instance-of-the-type--from-objective-c-because-the-type-is-generic-runtime-exception"></a><a name="MT4133"/>MT4133 kann nicht erstellt eine Instanz des Typs "*" aus Objective-C, da der Typ generisch ist. [Laufzeitausnahme]

### <a name="a-namemt4134mt4134--your-application-is-using-the--framework-which-isnt-included-in-the-ios-sdk-youre-using-to-build-your-app-this-framework-was-introduced-in-ios--while-youre-building-with-the-ios--sdk-please-select-a-newer-sdk-in-your-apps-ios-build-options"></a><a name="MT4134"/>MT4134 Der Anwendung verwendet wird der "*" Framework, das in der iOS-SDK enthalten ist, Sie beim Erstellen Ihrer app verwenden (iOS dieses Framework eingeführt wurde *, beim Erstellen mit iOS * SDK.) Wählen Sie einen neueren SDK in Ihre app iOS Buildoptionen an.

### <a name="a-namemt4135mt4135--the-member--has-an-export-attribute-that-doesnt-specify-a-selector-a-selector-is-required"></a><a name="MT4135"/>MT4135 Das Element "\*.\*" verfügt über ein Export-Attribut, das angibt, einen Selektor. Ein Selektor ist erforderlich.

### <a name="a-namemt4136mt4136--the-registrar-cannot-marshal-the-parameter-type--of-the-parameter--in-the-method-"></a><a name="MT4136"/>MT4136 Der Registrierungsstelle nicht marshallen den Parametertyp "\*"des Parameters"\*"in der Methode"\*. *"

<!-- MT4137 is unused -->

### <a name="a-namemt4138mt4138--the-registrar-cannot-marshal-the-property-type--of-the-property-"></a><a name="MT4138"/>MT4138 Der Registrierungsstelle nicht marshallen den Eigenschaftentyp "\*'der Eigenschaft"\*. * ".

### <a name="a-namemt4139mt4139--the-registrar-cannot-marshal-the-property-type--of-the-property--properties-with-the-connect-attribute-must-have-a-property-type-of-nsobject-or-a-subclass-of-nsobject"></a><a name="MT4139"/>MT4139 Der Registrierungsstelle nicht marshallen den Eigenschaftentyp "\*'der Eigenschaft"\*. * ". Eigenschaften mit dem Attribut [verbinden] müssen den Eigenschaftentyp NSObject (oder eine Unterklasse von NSObject) haben.

### <a name="a-namemt4140mt4140--the-registrar-found-a-signature-mismatch-in-the-method----the-selector-indicates-the-variadic-method-takes--parameters-while-the-managed-method-has--parameters"></a><a name="MT4140"/>MT4140 Gefunden die Registrierungsstelle eine Nichtübereinstimmung der Datenträgersignatur des in der Methode "*.*" – gibt die Auswahl an die Variadic-Methode nimmt * Parameter, während die verwaltete Methode hat * Parameter.

### <a name="a-namemt4141mt4141--cannot-register-the-selector--on-the-member--because-xamarinios-implicitly-registers-this-selector"></a><a name="MT4141"/>Den Selektor MT4141 kann nicht registriert werden kann "\*"für das Element"\*" da Xamarin.iOS diese Auswahl implizit registriert.

Dieser Fehler tritt beim Erstellen von Unterklassen für ein Framework, und versucht wird, implementieren Sie eine "beibehalten", "release" oder 'Dealloc'-Methode:

```csharp
class MyNSObject : NSObject
{
    [Export ("retain")]
    new void Retain () {}

    [Export ("release")]
    new void Release () {}

    [Export ("dealloc")]
    new void Dealloc () {}
}
```

Es ist jedoch möglich, diese Methoden überschreiben, wenn die Klasse ist die erste Unterklasse von Framework nicht eingeben:

```csharp
class MyNSObject : NSObject
{
}

class MyCustomNSObject : MyNSObject
{
    [Export ("retain")]
    new void Retain () {}

    [Export ("release")]
    new void Release () {}

    [Export ("dealloc")]
    new void Dealloc () {}
}
```

In diesem Fall überschreibt Xamarin.iOS `retain`, `release` und `dealloc` auf die `MyNSObject` -Klasse, und es wird kein Konflikt auftritt.

### <a name="a-namemt4142mt4142-failed-to-register-the-type-"></a><a name="MT4142"/>MT4142: Fehler beim Registrieren des Typs "*".
### <a name="a-namemt4143mt4143-the-objectivec-class--could-not-be-registered-it-does-not-seem-to-derive-from-any-known-objectivec-class-including-nsobject"></a><a name="MT4143"/>MT4143: Die ObjectiveC-Klasse ' *' konnte nicht registriert, es scheint nicht von einer bekannten ObjectiveC-Klasse (einschließlich NSObject) abgeleitet werden.
### <a name="a-namemt4144mt4144-cannot-register-the-method--since-it-does-not-have-an-associated-trampoline-please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT4144"/>MT4144: Die Methode kann nicht registriert werden "*", da sie nicht über eine zugeordnete Trampoline verfügt. Bitte Datei einen Fehlerbericht an http://bugzilla.xamarin.com.

Hiermit wird einen Fehler im Xamarin.iOS. Melden Sie den Fehler auf [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt4145mt4145-invalid-enum--enums-with-the-native-attribute-must-have-a-underlying-enum-type-of-either-long-or-ulong"></a><a name="MT4145"/>MT4145: Ungültiger Enum ' *': Enumerationen mit dem [systemeigene]-Attribut müssen einen zugrunde liegende Enumerationstyp 'long' oder 'Ulong' aufweisen.
### <a name="a-namemt4146mt4146-the-name-parameter-of-the-registrar-attribute-on-the-class---contains-an-invalid-character--"></a><a name="MT4146"/>MT4146: Der Name-Parameter von der Registrierungsstelle-Attribut für der Klasse\*"("\*") enthält ein ungültiges Zeichen:"\*"(\*).

Der Name einer Klasse Objectice-C Leerzeichen ist nicht möglich, dies bedeutet, dass die `Register` -Attribut in der entsprechenden verwalteten Klasse sind keine `Name` Parameter darf keine Leerzeichen enthalten, entweder.

Bitte überprüfen Sie, ob die `Register` Attribut für die verwaltete Klasse in der Fehlermeldung genannten enthält keine Leerzeichen.

### <a name="a-namemt4147mt4147-detected-a-protocol-inheriting-from-the-jsexport-protocol-while-using-the-dynamic-registrar-it-is-not-possible-to-export-protocols-to-javascriptcore-dynamically-the-static-registrar-must-be-used-add---registrarstatic-to-the-additional-mtouch-arguments-in-the-projects-ios-build-options-to-select-the-static-registrar"></a><a name="MT4147"/>MT4147: Erkannt, ein Protokoll, das von dem Protokoll JSExport bei der Verwendung der dynamischen Registrierungsstelle erben. Es ist nicht möglich, die Protokolle dynamisch in JavaScriptCore exportieren; die statische Registrierungsstelle muss verwendet werden (hinzufügen "--Registrierungsstelle: statische, auf die zusätzliche Mtouch Argumente in das Projekt iOS-Build-Optionen zur Auswahl von statischen Registrierungsstelle).
### <a name="a-namemt4148mt4148-the-registrar-found-a-generic-protocol--exporting-generic-protocols-is-not-supported"></a><a name="MT4148"/>MT4148:-Die Registrierungsstelle eine generische Protokoll gefunden: ' *'. Exportieren von generischen Protokolle wird nicht unterstützt.
### <a name="a-namemt4149mt4149-cannot-register-the-method--because-the-type-of-the-first-parameter--does-not-match-the-category-type-"></a><a name="MT4149"/>MT4149: Die Methode kann nicht registriert werden "\*.\*' da der Typ des ersten Parameters ("\*") entspricht nicht den Kategorietyp ("\*").
### <a name="a-namemt4150mt4150-cannot-register-the-type--because-the-type-property--in-its-category-attribute-does-not-inherit-from-nsobject"></a><a name="MT4150"/>MT4150: Den Typ kann nicht registriert werden "\*' da der Type-Eigenschaft ("\*") in der Kategorie Attribut erbt nicht von NSObject.
### <a name="a-namemt4151mt4151-cannot-register-the-type--because-the-type-property-in-its-category-attribute-isnt-set"></a><a name="MT4151"/>MT4151: Den Typ kann nicht registriert werden "*", da die Type-Eigenschaft in der Category-Attribut nicht festgelegt ist.
### <a name="a-namemt4152mt4152-cannot-register-the-type--as-a-category-because-it-implements-inativeobject-or-subclasses-nsobject"></a><a name="MT4152"/>MT4152: Den Typ kann nicht registriert werden "*" als Kategorie da INativeObject oder Unterklassen NSObject implementiert.
### <a name="a-namemt4153mt4153-cannot-register-the-type--as-a-category-because-its-generic"></a><a name="MT4153"/>MT4153: Den Typ kann nicht registriert werden "\*" als Kategorie, da diese generisch ist.
### <a name="a-namemt4154mt4154-cannot-register-the-method--as-a-category-method-because-its-generic"></a><a name="MT4154"/>MT4154: Die Methode kann nicht registriert werden "\*" als Kategorie-Methode, da diese generisch ist.
### <a name="a-namemt4155mt4155-cannot-register-the-method--with-the-selector--as-a-category-method-on--because-the-objective-c-already-has-an-implementation-for-this-selector"></a><a name="MT4155"/>MT4155: Die Methode kann nicht registriert werden "\*"mit dem Selektor"\*" als Kategorie-Methode auf "*" den Objective-C ist eine Implementierung für diese Auswahl bereits vorhanden.
### <a name="a-namemt4156mt4156-cannot-register-two-categories--and--with-the-same-native-name-"></a><a name="MT4156"/>MT4156: Zwei Kategorien kann nicht registriert werden können ("\*"und"\*") mit dem gleichen Namen für die systemeigene ("*").
### <a name="a-namemt4157mt4157-cannot-register-the-category-method--because-at-least-one-parameter-is-required-and-its-type-must-match-the-category-type-"></a><a name="MT4157"/>MT4157: Die Kategorie-Methode kann nicht registriert werden "\*", da mindestens ein Parameter erforderlich ist (und dessen Typ muss den Typ der Kategorie entsprechen "\*")
### <a name="a-namemt4158mt4158-cannot-register-the-constructor--in-the-category--because-constructors-in-categories-are-not-supported"></a><a name="MT4158"/>MT4158: Den Konstruktor kann nicht registriert werden * in der Kategorie * da Konstruktoren in Kategorien nicht unterstützt werden.
### <a name="a-namemt4159mt4159-cannot-register-the-method--as-a-category-method-because-category-methods-must-be-static"></a><a name="MT4159"/>MT4159: Die Methode kann nicht registriert werden "*" als Methode Kategorie da Kategorie Methoden statisch sein müssen.

### <a name="a-namemt4160mt4160-invalid-character---found-in-selector--for-"></a><a name="MT4160"/>MT4160: Ungültiges Zeichen "\*" (\*) im Selektor gefunden "\*"for"\*".

### <a name="a-namemt4161mt4161-the-registrar-found-an-unsupported-structure--all-fields-in-a-structure-must-also-be-structures-field--with-type-2-is-not-a-structure"></a><a name="MT4161"/>MT4161:-Die Registrierungsstelle eine nicht unterstützte Struktur gefunden "\*": alle Felder in einer Struktur muss auch Strukturen (Feld "\*' mit Typ"\ {2\}"ist eine Struktur).

Die Registrierungsstelle gefunden eine Struktur mit nicht unterstützte Felder.

Alle Felder in einer Struktur, die für Objective-C verfügbar gemacht wird, muss auch Strukturen (nicht-Klassen).

### <a name="a-namemt4162mt4162-the-type--used-as--2-is-not-available-in---it-was-introduced-in---please-build-with-a-newer--sdk-usually-done-by-using-the-most-recent-version-of-xcode"></a><a name="MT4162"/>MT4162: Der Typ "\*" (als verwendet * \ {2\}) ist nicht verfügbar in ** (es seit * *)\* erstellen Sie mit einer neueren * SDK (normalerweise mit der neuesten Version von Xcode vorgenommen.

Die Registrierungsstelle gefunden, einen Typ, der nicht im aktuellen SDK enthalten ist.

Aktualisieren Sie Xcode.

### <a name="a-namemt4163mt4163-internal-error-in-the-registrar--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT4163"/>MT4163: Interner Fehler in die Registrierungsstelle (*). Bitte der Datei eines Fehlerberichts an http://bugzilla.xamarin.com

Dieser Fehler zeigt einen Fehler im Xamarin.iOS an. Bitte der Datei eines Fehlerberichts an [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt4164mt4164-cannot-export-the-property--because-its-selector--is-an-objective-c-keyword-please-use-a-different-name"></a><a name="MT4164"/>MT4164: Die Eigenschaft kann nicht exportiert werden "\*" da dem Selektor "\*" ist ein Objective-C-Schlüsselwort. Verwenden Sie einen anderen Namen ein.

Der Selektor für die betreffende Eigenschaft ist keinem gültigen Objective-C-Bezeichner.

Verwenden Sie einen gültigen Objective-C-Bezeichner als Selektoren an.

### <a name="a-namemt4165mt4165-the-registrar-couldnt-find-the-type-systemvoid-in-any-of-the-referenced-assemblies"></a><a name="MT4165"/>MT4165: Die Registrierungsstelle konnte nicht in jedem der referenzierten Assemblys den Typ 'System.Void' gefunden.

Dieser Fehler wahrscheinlich gibt einen Fehler im Xamarin.iOS an. Bitte der Datei eines Fehlerberichts an [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt4166mt4166-cannot-register-the-method--because-the-signature-contains-a-type--that-isnt-a-reference-type"></a><a name="MT4166"/>MT4166: Die Methode kann nicht registriert werden "\*", da die Signatur ein Typs enthält (\*), die keinen Referenztyp darstellt.

Dies weist in der Regel ein Fehler im Xamarin.iOS hin; Melden Sie den Fehler auf [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt4167mt4167-cannot-register-the-method--because-the-signature-contains-a-generic-type--with-a-generic-argument-type-that-isnt-an-nsobject-subclass-"></a><a name="MT4167"/>MT4167: Die Methode kann nicht registriert werden "\*", da die Signatur ein generischen Typs enthält (\*) mit einem generischen Argument, die eine Unterklasse NSObject (*) nicht.

Dies weist in der Regel ein Fehler im Xamarin.iOS hin; Melden Sie den Fehler auf [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt4168mt4168-cannot-register-the-type-managedname-because-its-objective-c-name-exportedname-is-an-objective-c-keyword-please-use-a-different-name"></a><a name="MT4168"/>MT4168: Den Typ kann nicht registriert werden "{verwaltet\_Name}" da seinen Namen Objective-C ' {exportiert\_Name} "ist ein Objective-C-Schlüsselwort. Verwenden Sie einen anderen Namen ein.

Der Objective-C-Name für den betreffenden Typ ist kein gültiger Objective-C-Bezeichner.

Verwenden Sie einen gültigen Objective-C-Bezeichner.

### <a name="a-namemt4169mt4169-failed-to-generate-a-pinvoke-wrapper-for-method-message"></a><a name="MT4169"/>MT4169: Fehler beim Generieren der Wrapper P/Invoke für {Methode}: {Message}

Xamarin.iOS Fehler beim Generieren einer P/Invoke-Wrapperfunktion für die genannten.
Überprüfen Sie die gemeldeten Fehler Nachricht für die zugrunde liegende Ursache.

### <a name="a-namemt4170mt4170-the-registrar-cant-convert-from-managed-type-to-native-type-for-the-return-value-in-the-method-method"></a><a name="MT4170"/>MT4170: Die Registrierungsstelle kann nicht aus "{verwalteten Type}" in "{native Type}" für den Rückgabewert in {Methode} konvertieren.

Siehe die Beschreibung des Fehlers <a href="#MT4172">MT4172</a>.

### <a name="a-namemt4171mt4171-the-bindas-attribute-on-the-member-member-is-invalid-the-bindas-type-type-is-different-from-the-property-type-type"></a><a name="MT4171"/>MT4171: Das BindAs-Attribut für das Element {Member} ist ungültig: der Typ BindAs {Type} unterscheidet sich von den Eigenschaftentyp {Type}.

Stellen Sie sicher, dass der Typ im BindAs-Attribut den Typ des Elements übereinstimmt, die es angefügt ist.

### <a name="a-namemt4172mt4172-the-registrar-cant-convert-from-native-type-to-managed-type-for-the-parameter-parameter-name-in-the-method-method"></a><a name="MT4172"/>MT4172: Die Registrierungsstelle kann nicht aus "{native Type}" in "{verwalteten Type}" für den Parameter "{Parametername}" in der Methode {Methode} konvertieren.

Konvertieren zwischen den erwähnten Typen von unterstützt der Registrierungsstelle nicht.

Dies ist ein Fehler in Xamarin.iOS, wenn die betreffende-API von Xamarin.iOS bereitgestellt wird; Melden Sie den Fehler auf [http://bugzilla.xamarin.com][1].

Wenn Sie während der Entwicklung eines Projekts Bindung für eine systemeigene Bibliothek in diesen ausführen, können wir zum Hinzufügen der Unterstützung für neue typkombinationen öffnen. Wenn dies der Fall ist, bitte der Datei einer Anforderung zur Erweiterung ([http://bugzilla.xamarin.com][2]) in einem Test Groß-/Kleinschreibung, und wir werden auszuwerten.

[1]: https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS
[2]: https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS&component=General&bug_severity=enhancement

## <a name="mt5xxx-gcc-and-toolchain-error-messages"></a>MT5xxx: GCC und toolkette Fehlermeldungen

### <a name="mt51xx-compilation"></a>MT51xx: Compilation

<!--
 MT5xxx GCC and toolchain
  MT51xx compilation
  -->

### <a name="a-namemt5101mt5101-missing--compiler-please-install-xcode-command-line-tools-component"></a><a name="MT5101"/>Fehlende MT5101 ' *' Compiler. Installieren Sie Xcode "Befehlszeilentools"-Komponente

### <a name="a-namemt5102mt5102-failed-to-assemble-the-file--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT5102"/>MT5102 konnte die Datei "*". Bitte der Datei eines Fehlerberichts an http://bugzilla.xamarin.com

### <a name="a-namemt5103mt5103-failed-to-compile-the-file--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT5103"/>MT5103 Fehler beim Kompilieren der das "*". Bitte der Datei eines Fehlerberichts an http://bugzilla.xamarin.com

### <a name="a-namemt5104mt5104-could-not-find-neither-the--nor-the--compiler-please-install-xcode-command-line-tools-component"></a><a name="MT5104"/>MT5104 konnte nicht gefunden werden weder die "\*"noch die"\*" Compiler. Installieren Sie Xcode "Befehlszeilentools"-Komponente

<!-- 5105 is used by mmp -->

### <a name="a-namemt5106mt5106-could-not-compile-the-files--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT5106"/>MT5106: Konnte nicht kompiliert werden die Dateien "*". Bitte der Datei eines Fehlerberichts an http://bugzilla.xamarin.com

Dies weist in der Regel ein Fehler im Xamarin.iOS hin; Melden Sie den Fehler auf [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="mt52xx-linking"></a>MT52xx: Linking

<!--
  MT52xx linking
  -->

### <a name="a-namemt5201mt5201-native-linking-failed-please-review-the-build-log-and-the-user-flags-provided-to-gcc-"></a><a name="MT5201"/>Fehler bei der systemeigenen MT5201 verknüpfen. Überprüfen Sie das Buildprotokoll und die Benutzerflags bereitgestellt, um den Gcc: *

### <a name="a-namemt5202mt5202-native-linking-failed-please-review-the-build-log"></a><a name="MT5202"/>Fehler bei der systemeigenen MT5202 verknüpfen. Überprüfen Sie das Buildprotokoll an.

### <a name="a-namemt5203mt5203-native-linking-warning-"></a><a name="MT5203"/>Systemeigene MT5203 verknüpfen Warnung: *

<!--- 5204-5208 are not used -->

### <a name="a-namemt5209mt5209-native-linking-error-"></a><a name="MT5209"/>MT5209 systemeigene Fehler verknüpfen: *

### <a name="a-namemt5210mt5210-native-linking-failed-undefined-symbol--please-verify-that-all-the-necessary-frameworks-have-been-referenced-and-native-libraries-are-properly-linked-in"></a><a name="MT5210"/>MT5210: Verknüpfen von systemeigenen fehlgeschlagen ist, nicht definiertes Symbol: *. Stellen Sie sicher, dass alle erforderlichen Frameworks auf die verwiesen wurde, und systemeigene Bibliotheken ordnungsgemäß verknüpft sind.

Dies geschieht, wenn der systemeigene Linker ein Symbol finden kann, die an einer beliebigen Stelle verwiesen wird. Es gibt mehrere Gründe, warum dieser Fehler kann auftreten:

* Eine Drittanbieter-Bindung erfordert, dass ein Framework, aber die Bindung gibt keinen hierzu finden Sie unter seiner `[LinkWith]` Attribut. Lösungen:
  - Wenn Sie den Autor der Bindung eines Drittanbieters, oder haben Zugriff auf die Updatequelle, ändern Sie der Bindung `[LinkWith]` Attribut, um das Framework umfassen notwendig:

            [LinkWith ("mylib.a", Frameworks = "SystemConfiguration")]

  - Wenn Sie die Bindung eines Drittanbieters nicht ändern, Sie können manuell Verknüpfen mit den erforderlichen Framework durch übergeben `-gcc_flags '-framework SystemFramework'` auf `mtouch` (Dies erfolgt durch die zusätzliche Mtouch Argumente in das Projekt iOS-Build-Optionsseite ändern. Denken Sie daran, dass dies für jede Projektkonfiguration erfolgen muss).
* In einigen Fällen eine verwaltete Bindung besteht aus mehreren systemeigene Bibliotheken, und alle in den Bindungen enthalten sein müssen. Es ist möglich, mehr als eine systemeigene Bibliothek in jeder bindungsprojekt, damit die Lösung besteht darin, alle erforderlichen systemeigenen Bibliotheken nur die bindungsprojekt hinzufügen.</li>
* Eine verwaltete Bindung bezieht sich auf systemeigene Symbole, die in die systemeigene Bibliothek nicht vorhanden waren.
    Dies geschieht in der Regel auf, wenn eine Bindung für einige Zeit vorhanden ist und der systemeigene Code, damit eine bestimmte systemeigene Klasse entweder entfernt oder umbenannt wurde, während die Bindung nicht aktualisiert wurde während dieser Zeit geändert wurde.
* P/Invoke bezieht sich auf ein systemeigenes Symbol, das nicht existiert. Beginnend mit Xamarin.iOS 7.4 ein <a href="#MT5214">MT5214</a> für diesen Fall Fehler gemeldet werden (Weitere Informationen finden Sie unter "MT5214").
* Eine Drittanbieter-Bindung / Bibliothek mit C++ erstellt wurde, aber die Bindung gibt keinen hierzu finden Sie unter seiner `[LinkWith]` Attribut. Dies ist in der Regel recht einfach zu erkennen, Schreibberechtigung die Symbole sind ergänzte C++-Symbolen (ist ein allgemeines Beispiel `__ZNKSt9exception4whatEv`).
  - Wenn Sie den Autor der Bindung eines Drittanbieters, oder haben Zugriff auf die Updatequelle, ändern Sie der Bindung `[LinkWith]` Attribut festzulegende der `IsCxx` Kennzeichen:

            [LinkWith ("mylib.a", IsCxx = true)]

  - Wenn die Drittanbieter-Bindung kann nicht geändert werden, oder Sie manuell mit einer Bibliothek eines Drittanbieters verknüpfen möchten, können Sie das entsprechende Flag festlegen, indem übergeben <code>-cxx</code> zu Mtouch (Dies erfolgt durch Bearbeiten der Mtouch zusätzliche Argumente in das Projekt iOS-Build-Optionsseite . Denken Sie daran, dass dies für jede Projektkonfiguration erfolgen muss).

### <a name="a-namemt5211mt5211-native-linking-failed-undefined-objective-c-class--the-symbol--could-not-be-found-in-any-of-the-libraries-or-frameworks-linked-with-your-application"></a><a name="MT5211"/>MT5211: Verknüpfen von systemeigenen fehlgeschlagen ist, nicht definierte Objective-C-Klasse: \*. Das Symbol "\*" konnte nicht gefunden werden, in jede der Bibliotheken oder Frameworks, die mit der Anwendung verknüpft.

Dies geschieht, wenn der systemeigene Linker eine Objective-C-Klasse finden kann, die an einer beliebigen Stelle verwiesen wird. Es gibt verschiedene Gründe, die dieser Fehler kann auftreten: die gleichen wie für [MT5210](#MT5210) und darüber hinaus:

* Eine Bindung eines Drittanbieters ein Objective-C-Protokoll gebunden, aber nicht kommentieren Sie ihn mit der <code>[Protocol]</code> Attribut in der api-Definition. Lösungen:
  - Fügen Sie die fehlende `[Protocol]` Attribut:

              [BaseType (typeof (NSObject))]
              [Protocol] // Add this
              public interface MyProtocol
              {
              }

### <a name="a-namemt5212mt5212-native-linking-failed-duplicate-symbol-"></a><a name="MT5212"/>MT5212: Verknüpfen von systemeigenen Fehler Doppelte Symbol: *.

Dies geschieht, wenn der systemeigene Linker doppelte Symbole zwischen den systemeigenen Bibliotheken auftritt. Nach dieser Fehler möglicherweise eine oder mehrere [MT5213](#MT5213) Fehlern mit Assistenten für den Speicherort für jedes Vorkommen des Symbols. Mögliche Ursachen für diesen Fehler:

* Die gleiche systemeigene Bibliothek ist zweimal enthalten.
* Zwei unterschiedliche systemeigene Bibliotheken geschehen, um die gleichen Symbole zu definieren.
* Eine systemeigene Bibliothek ist nicht korrekt erstellt und das gleiche Symbol mehr als einmal enthält.
  Sie können dies überprüfen, indem Sie mithilfe der folgenden Befehle aus einer Terminal (x86_64/armv7/armv7s/arm64 Sie entsprechend der Architektur für erstellen i386 ersetzt):

        # Native libraries are usually fat libraries, containing binary code for
        # several architectures in the same file. First we extract the binary
        # code for the architecture we're interested in.
        lipo libNative.a -thin i386 -output libNative.i386.a

        # Now query the native library for the duplicated symbol.
        nm libNative.i386.a | fgrep 'SYMBOL'

        # You can also list the object files inside the native library.
        # In most cases this will reveal duplicated object files.
        ar -t libNative.i386.a

  Es gibt einige Möglichkeiten, um dieses Problem zu beheben:

  - Fordern Sie an, dass der Anbieter, der die systemeigene Bibliothek Behebung des Problems, und geben die aktualisierte Version.
  - Reparieren Sie sie selbst durch das Entfernen der zusätzlichen Objektdateien (dies funktioniert nur, wenn das Problem tatsächlich doppelte Objektdateien ist)

            # Find out if the library is a fat library, and which
            # architectures it contains.
            lipo -info libNative.a

            # Extract each architecture (i386/x86_64/armv7/armv7s/arm64) to a separate file
            lipo libNative.a -thin ARCH -output libNative.ARCH.a

            # Extract the object files for the offending architecture
            # This will remove the duplicates by overwriting them
            # (since they have the same filename)
            mkdir -p ARCH
            cd ARCH
            ar -x ../libNative.ARCH.a

            # Reassemble the object files in an .a
            ar -r ../libNative.ARCH.a *.o
            cd ..

            # Reassemble the fat library
            lipo *.a -create -output libNative.a

  - Bitten Sie den Linker an, nicht verwendeten Code entfernt. Xamarin.iOS dies automatisch geschieht, wenn alle folgenden Bedingungen erfüllt sind:
    - Alle Drittanbieter-Bindungen `[LinkWith]` Attribute SmartLink aktiviert haben:

            [assembly: LinkWith ("libNative.a", SmartLink = true)]

    - Nicht `-gcc_flags` Mtouch (im Feld Argumente zusätzliche Mtouch des Projekts e/as Buildoptionen) übergeben wird.
    - Es ist auch möglich, bitten Sie den Linker an, direkt durch Hinzufügen von nicht verwendeten Code entfernt `-gcc_flags -dead_strip` Buildoptionen für zusätzliche Mtouch Argumente in iOS-Projekts.

### <a name="a-namemt5213mt5213-duplicate-symbol-in--location-related-to-previous-error"></a><a name="MT5213"/>MT5213: Doppelte Symbols in: * (Speicherort für den vorherigen Fehler)

Dieser Fehler wird gemeldet, nur zusammen mit [MT5212](#MT5212). Finden Sie unter [MT5212](#MT5212) für Weitere Informationen.

### <a name="a-namemt5214mt5214--native-linking-failed-undefined-symbol--this-symbol-was-referenced-the-managed-member--please-verify-that-all-the-necessary-frameworks-have-been-referenced-and-native-libraries-linked"></a><a name="MT5214"/>MT5214 systemeigene Fehler und verknüpfen undefined Symbol: *. Dieses Symbol wurde verwiesen, wenn das verwaltete Element *. Stellen Sie sicher, dass alle erforderlichen Frameworks verwiesen wird und systemeigene Bibliotheken verknüpft wurden.

Dieser Fehler wird gemeldet, wenn der verwaltete Code P/Invoke auf einer nativen Methode enthält, die nicht vorhanden ist. Zum Beispiel:

```csharp
using System.Runtime.InteropServices;
class MyImports {
   [DllImport ("__Internal")]
   public static extern void MyInexistentFunc ();
}
```

Es gibt einige mögliche Lösungen:

  -  Entfernen Sie die betreffende P/Invokes aus dem Quellcode.
  -  Aktivieren des verwalteten Linkers für alle Assemblys (dies in iOS-Build-Optionen für das Projekt erfolgt durch Festlegung "Linker" auf "Alle Assemblys"). Dadurch werden alle dem P/Invokes verwenden Sie nicht effektiv aus der app entfernt (automatisch, anstelle von manuell wie im vorangehenden Punkt). Der Nachteil besteht darin, dass dies Builds Simulator etwas langsamer veranlasst, und er Ihre app unterbrochen kann, wenn Reflektion verwendeten – Weitere Informationen zu den Linker verwendbaren [hier](~/ios/deploy-test/linker.md) )
  -  Erstellen Sie eine zweite systemeigene Bibliothek, die die fehlende systemeigene Symbole enthält. Beachten Sie, dass dies lediglich eine problemumgehung ist (Wenn Sie versuchen versehentlich, diese Funktionen aufrufen, die app abstürzt).

### <a name="a-namemt5215mt5215-references-to--might-require-additional--frameworkxxx-or--lxxx-instructions-to-the-native-linker"></a><a name="MT5215"/>MT5215: Verweise auf "*" möglicherweise zusätzliche - Framework = XXX oder lXXX - Anweisungen, um die systemeigenen Linker

Dies ist eine Warnung, die angibt, dass P/Invoke auf die betreffende Bibliothek verweisenden erkannt wurde, aber die app ist nicht mit verknüpfen.

### <a name="a-namemt5216mt5216-native-linking-failed-for--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT5216"/>MT5216: Verknüpfen von systemeigenen Fehler bei der für *. Bitte der Datei eines Fehlerberichts an http://bugzilla.xamarin.com

Dieser Fehler wird gemeldet, wenn die Ausgabe aus dem Compiler AOT verknüpfen.

Dieser Fehler wahrscheinlich gibt einen Fehler im Xamarin.iOS an. Bitte der Datei eines Fehlerberichts an [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt5217mt5217-native-linking-possibly-failed-because-the-linker-command-line-was-too-long--characters"></a><a name="MT5217"/>MT5217: Verknüpfen von systemeigenen möglicherweise fehlgeschlagen, da der Linker-Befehlszeile zu lang war (* Zeichen).

Fehler beim Verknüpfen von systemeigenen, und es ist möglich, dafür ist, dass der Linker-Befehl zu lang war.

Xamarin.iOS Projekte werden häufig systemeigene Symbole dynamisch verweisen was bedeutet, dass der systemeigene Linker diese systemeigene Symbole während der systemeigenen Verknüpfungsvorgang entfernen kann, da der systemeigene Linker nicht angezeigt wird, dass diese Symbole verwendet werden.

In der Regel fragt Xamarin.iOS den systemeigenen Linker zu solchen Symbole mithilfe der `-u symbol` Linker-Flags, wenn jedoch gibt es viele solcher Symbole, die gesamte Befehlszeilen möglicherweise länger als maximal Befehlszeilen wie vom Betriebssystem angegeben.

Es gibt einige mögliche Quellen für diese dynamische Symbole:

* P/Invokes an Methoden in statisch verknüpfte Bibliotheken (wobei der Name der Dll ist `__Internal` in das DllImport-Attribut `[DllImport ("__Internal")]`).
* Verweise auf Speicheradressen in statisch verknüpfte Bibliotheken aus Projekten binden Feld (`[Field]` Attribute).
* Objective-C-Klassen, die in das statisch verknüpfte Bibliotheken aus der Bindung von Projekten (wenn inkrementelle Builds zu verwenden oder wenn die statische Registrierungsstelle nicht verwenden) verwiesen wird.

Folgende Lösungen sind möglich:

* Aktivieren Sie den verwalteten Linker (sofern möglich, für alle Assemblys aufheben, statt den einzigen SDK-Assemblys). Dies kann genügend Quellen entfernen, für die dynamische Symbole, damit die der Linker Befehlszeile mit der nicht größer als der maximal.
* Reduzieren Sie die Anzahl der P/Invokes, Feldverweisen und/oder Objective-C-Klassen.
* Schreiben Sie die dynamische Symbole, um kürzere Namen haben.
* Übergeben Sie `-dlsym:false` als zusätzliche Mtouch Argument in das Projekt iOS Buildoptionen. Mit dieser Option Xamarin.iOS einen systemeigenen Verweis in der AOT kompilierte Code generiert, und bitten Sie den Linker an, dieses Symbol behalten müssen keine. Allerdings dieser funktioniert nur für Geräte erstellt, und es verursachen Linkerfehler treten P/Invokes auf Funktionen, die in der statischen Bibliothek nicht vorhanden waren.
* Übergeben Sie `--dynamic-symbol-mode=code` als eine zusätzliche Mtouch Argumente in das Projekt iOS Buildoptionen. Mit dieser Option generiert Xamarin.iOS zusätzlichen systemeigenen Code, der diese Symbole anstelle den systemeigenen Linker diese Symbole verwenden Sie Befehlszeilenargumente zu bitten verweist. Der Nachteil dieses Ansatzes ist, dass sie die ausführbare Datei etwas vergrößert wird.
* Aktivieren Sie die statische Registrierungsstelle durch Übergabe `--registrar:static` als zusätzliche Mtouch Argument in das Projekt iOS-Build-Optionen (für Simulator erstellt, da die statische Registrierungsstelle bereits die Standardeinstellung ist für Geräte-builds). Die statische Registrierungsstelle generiert Code, der Objective-C-Klassen statisch verweist, daher keine Notwendigkeit besteht, bitten Sie den systemeigenen Linker solche Klassen zu.
* Deaktivieren Sie inkrementelle Builds (für Geräte-Builds). Wenn inkrementelle Builds aktiviert sind, wird nicht vom statischen Registrierungsstelle generierte Code durch die systemeigenen Linker berücksichtigt werden, d. h., die Xamarin.iOS weiterhin den Linker an, behalten anfordern muss Objective-C-Klassen verwiesen. Daher deaktivieren inkrementelle Builds wird verhindert, dass diese Anforderung.

### <a name="a-namemt5218mt5218-cant-ignore-the-dynamic-symbol-symbol---ignore-dynamic-symbolsymbol-because-it-was-not-detected-as-a-dynamic-symbol"></a><a name="MT5218"/>MT5218: Kann nicht das dynamische Symbol {Symbol} ignorieren (--ignorieren-Dynamic-Symbol = {Symbol}), da er nicht als ein dynamisches Symbol erkannt wurde.

Das Befehlszeilenargument `--ignore-dynamic-symbol=symbol` übergeben wurde, aber dieses Symbol befindet es sich nicht um ein Symbol, das als Symbol für dynamische erkannt wurde, die manuell beibehalten werden sollen.

Es gibt zwei wichtige Gründe:

* Der Symbolname ist falsch.
    * Keinen Unterstrich den Symbolnamen voran.
    * Das Symbol für Objective-C-Klassen ist `OBJC_CLASS_$_<classname>`.
* Das Symbol ist richtig, aber es ist ein Symbol, das bereits normales beibehalten wird (einige build Optionen Ursachen die genaue Liste der dynamischen Symbole variieren).

### <a name="mt53xx-other-tools"></a>MT53xx: Andere tools

<!--
  MT53xx other tools
  -->

### <a name="a-namemt5301mt5301-missing-strip-tool-please-install-xcode-command-line-tools-component"></a><a name="MT5301"/>MT5301: Fehlender Tool "entfernen". Installieren Sie Xcode "Befehlszeilentools"-Komponente

### <a name="a-namemt5302mt5302-missing-dsymutil-tool-please-install-xcode-command-line-tools-component"></a><a name="MT5302"/>MT5302: Fehlender "Dsymutil"-Tool. Installieren Sie Xcode "Befehlszeilentools"-Komponente

### <a name="a-namemt5303mt5303-failed-to-generate-the-debug-symbols-dsym-directory-please-review-the-build-log"></a><a name="MT5303"/>MT5303: Fehler beim Generieren von der Debugsymbolen (dSYM Directory). Überprüfen Sie das Buildprotokoll an.

Fehler beim Ausführen von Dsymutil für die endgültige .app-Verzeichnis, um den Debugsymbolen erstellen. Überprüfen Sie das Buildprotokoll um des Dsymutil Ausgabe anzuzeigen und wie es behoben werden kann.

### <a name="a-namemt5304mt5304-failed-to-strip-the-final-binary-please-review-the-build-log"></a><a name="MT5304"/>MT5304: Fehler, um die endgültige Binärdatei zu entfernen. Überprüfen Sie das Buildprotokoll an.

Fehler beim Ausführen des Tools "Streifen", um Debuginformationen aus der Anwendung zu entfernen.

### <a name="a-namemt5305mt5305-missing-lipo-tool-please-install-xcode-command-line-tools-component"></a><a name="MT5305"/>MT5305: Fehlender "Lipo"-Tool. Installieren Sie Xcode "Befehlszeilentools"-Komponente

### <a name="a-namemt5306mt5306-failed-to-create-the-a-fat-library-please-review-the-build-log"></a><a name="MT5306"/>MT5306: Fehler beim Erstellen der fat-Bibliothek. Überprüfen Sie das Buildprotokoll an.

Fehler beim Ausführen des Tools "Lipo". Überprüfen Sie das Buildprotokoll zum Anzeigen des Fehlers, der von "Lipo" gemeldet.

### <a name="a-namemt5307mt5307-failed-to-sign-the-executable-please-review-the-build-log"></a><a name="MT5307"/>MT5307: Fehler bei der ausführbaren Datei. Überprüfen Sie das Buildprotokoll an.

Fehler beim Signieren der Anwendung. Überprüfen Sie das Buildprotokoll zum Anzeigen des Fehlers, der von "Codesign" gemeldet.

<!-- 5308 is used by mmp -->
<!-- 5309 is used by mmp -->
<!-- 5310 is used by mmp -->

## <a name="mt6xxx-mtouch-internal-tools-error-messages"></a>MT6xxx: Mtouch interne tools Fehlermeldungen

### <a name="mt600x-stripper"></a>MT600x: Stripper

<!--
 MT6xxx mtouch internal tools
  MT600x Stripper
  -->

### <a name="a-namemt6001mt6001-running-version-of-cecil-doesnt-support-assembly-stripping"></a><a name="MT6001"/>MT6001: Ausgeführten Version von Cecil unterstützt keine Assembly entfernen

### <a name="a-namemt6002mt6002-could-not-strip-assembly-"></a><a name="MT6002"/>MT6002: Konnte keine Assembly entfernen `*`.

Fehler beim Entfernen von verwalteten Codes (nach Entfernen der IL-Code) aus den Assemblys in der Anwendung.

### <a name="a-namemt6003mt6003-unauthorizedaccessexception-message"></a><a name="MT6003"/>MT6003: [UnauthorizedAccessException Message]

Sicherheit Fehler beim Entfernen von Debugsymbole von der Anwendung.

## <a name="mt7xxx-msbuild-error-messages"></a>MT7xxx: MSBuild-Fehlermeldungen

<!--
 MT7xxx msbuild errors
  -->

### <a name="a-namemt7001mt7001-could-not-resolve-host-ips-for-wifi-debugger-settings"></a><a name="MT7001"/>MT7001: Host-IP-Adressen für WLAN-Debuggereinstellungen konnte nicht aufgelöst werden.

*MSBuild-Aufgabe: DetectDebugNetworkConfigurationTaskBase*

Schritte zur Fehlerbehebung:

- Führen Sie aus `csharp -e 'System.Net.Dns.GetHostEntry (System.Net.Dns.GetHostName ()).AddressList'` (, die Informationen erhalten Sie eine IP-Adresse und nicht um einen Fehler offensichtlich).
- Führen Sie aus "Ping \`Hostname\`" dem möglicherweise erhalten Sie weitere Informationen wie z. B.: `cannot resolve MyHost.local: Unknown host`

In einigen Fällen ist es ein "Lokales Netzwerk"-Problem, und durch Hinzufügen des Hosts unbekannten Fehler behandelt werden `127.0.0.1   MyHost.local` in `/etc/hosts`.

### <a name="a-namemt7002mt7002-this-machine-does-not-have-any-network-adapters-this-is-required-when-debugging-or-profiling-on-device-over-wifi"></a><a name="MT7002"/>MT7002: Netzwerkadapter werden von diesem Computer nicht vorhanden. Dies ist erforderlich, beim Debuggen oder profilerstellung für Gerät über WiFi.

*MSBuild-Aufgabe: DetectDebugNetworkConfigurationTaskBase*

### <a name="a-namemt7003mt7003-the-app-extension--does-not-contain-an-infoplist"></a><a name="MT7003"/>MT7003: Die App-Erweiterung "*" enthält eine Datei "Info.plist" nicht.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7004mt7004-the-app-extension--does-not-specify-a-cfbundleidentifier"></a><a name="MT7004"/>MT7004: Die App-Erweiterung "*" gibt keine CFBundleIdentifier.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7005mt7005-the-app-extension--does-not-specify-a-cfbundleexecutable"></a><a name="MT7005"/>MT7005: Die App-Erweiterung "*" gibt keine CFBundleExecutable.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7006mt7006-the-app-extension--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a><a name="MT7006"/>MT7006: Die App-Erweiterung "\*" hat eine ungültige CFBundleIdentifier (\*), beginnt nicht mit der Haupt-app-Bündel CFBundleIdentifier (*).

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7007mt7007-the-app-extension--has-a-cfbundleidentifier--that-ends-with-the-illegal-suffix-key"></a><a name="MT7007"/>MT7007: Die App-Erweiterung "\*" hat eine CFBundleIdentifier (\*) mit dem unzulässige Suffix ".key" endet.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7008mt7008-the-app-extension--does-not-specify-a-cfbundleshortversionstring"></a><a name="MT7008"/>MT7008: Die App-Erweiterung "*" gibt keine CFBundleShortVersionString.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7009mt7009-the-app-extension--has-an-invalid-infoplist-it-does-not-contain-an-nsextension-dictionary"></a><a name="MT7009"/>MT7009: Die App-Erweiterung "*" hat eine ungültige Datei "Info.plist": er enthält kein NSExtension Wörterbuch.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7010mt7010-the-app-extension--has-an-invalid-infoplist-the-nsextension-dictionary-does-not-contain-an-nsextensionpointidentifier-value"></a><a name="MT7010"/>MT7010: Die App-Erweiterung "*" hat eine ungültige Datei "Info.plist": das NSExtension-Wörterbuch enthält keinen NSExtensionPointIdentifier-Wert.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7011mt7011-the-watchkit-extension--has-an-invalid-infoplist-the-nsextension-dictionary-does-not-contain-an-nsextensionattributes-dictionary"></a><a name="MT7011"/>MT7011: Die WatchKit-Erweiterung "*" hat eine ungültige Datei "Info.plist": das NSExtension-Wörterbuch enthält kein NSExtensionAttributes Wörterbuch.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7012mt7012-the-watchkit-extension--does-not-have-exactly-one-watch-app"></a><a name="MT7012"/>MT7012: Die WatchKit-Erweiterung "*" verfügt nicht über genau eine Watch-app.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7013mt7013-the-watchkit-extension--has-an-invalid-infoplist-uirequireddevicecapabilities-must-contain-the-watch-companion-capability"></a><a name="MT7013"/>MT7013: Die WatchKit-Erweiterung "*" hat eine ungültige Datei "Info.plist": UIRequiredDeviceCapabilities muss die Funktion "Watch-Companion" enthalten.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7014mt7014-the-watch-app--does-not-contain-an-infoplist"></a><a name="MT7014"/>MT7014: Watch-App "*" enthält eine Datei "Info.plist" nicht.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7015mt7015-the-watch-app--does-not-specify-a-cfbundleidentifier"></a><a name="MT7015"/>MT7015: Watch-App "*" gibt keine CFBundleIdentifier.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7016mt7016-the-watch-app--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a><a name="MT7016"/>MT7016: Watch-App "\*" hat eine ungültige CFBundleIdentifier (\*), beginnt nicht mit der Haupt-app-Bündel CFBundleIdentifier (*).

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7017mt7017-the-watch-app--does-not-have-a-valid-uidevicefamily-value-expected-watch-4-but-found--"></a><a name="MT7017"/>MT7017: Watch-App "\*" verfügt nicht über einen gültigen UIDeviceFamily-Wert. Erwartete "überwachen (4)" wurde erwartet, aber "\* (*)".

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7018mt7018-the-watch-app--does-not-specify-a-cfbundleexecutable"></a><a name="MT7018"/>MT7018: Watch-App "*" gibt keine CFBundleExecutable

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7019mt7019-the-watch-app--has-an-invalid-wkcompanionappbundleidentifier-value--it-does-not-match-the-main-app-bundles-cfbundleidentifier-"></a><a name="MT7019"/>MT7019: Watch-App "\*" weist einen ungültigen WKCompanionAppBundleIdentifier-Wert ("\*"), entspricht nicht der Haupt-app-Bündel CFBundleIdentifier ("*").

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7020mt7020-the-watch-app--has-an-invalid-infoplist-the-wkwatchkitapp-key-must-be-present-and-have-a-value-of-true"></a><a name="MT7020"/>MT7020: Watch-App "*" hat eine ungültige Datei "Info.plist": der WKWatchKitApp-Schlüssel muss vorhanden sein und den Wert "True" aufweisen.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7021mt7021-the-watch-app--has-an-invalid-infoplist-the-lsrequiresiphoneos-key-must-not-be-present"></a><a name="MT7021"/>MT7021: Watch-App "*" hat eine ungültige Datei "Info.plist": der LSRequiresIPhoneOS-Schlüssel muss nicht vorhanden sein.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7022mt7022-the-watch-app--does-not-contain-a-watch-extension"></a><a name="MT7022"/>MT7022: Watch-App "*" enthält keine Watch-Erweiterung.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7023mt7023-the-watch-extension--does-not-contain-an-infoplist"></a><a name="MT7023"/>MT7023: Der Watch-Erweiterung "*" enthält eine Datei "Info.plist" nicht.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7024mt7024-the-watch-extension--does-not-specify-a-cfbundleidentifier"></a><a name="MT7024"/>MT7024: Der Watch-Erweiterung "*" gibt keine CFBundleIdentifier.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7025mt7025-the-watch-extension--does-not-specify-a-cfbundleexecutable"></a><a name="MT7025"/>MT7025: Der Watch-Erweiterung "*" gibt keine CFBundleExecutable.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7026mt7026-the-watch-extension--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a><a name="MT7026"/>MT7026: Der Watch-Erweiterung "\*" hat eine ungültige CFBundleIdentifier (\*), beginnt nicht mit der Haupt-app-Bündel CFBundleIdentifier (*).

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7027mt7027-the-watch-extension--has-a-cfbundleidentifier--that-ends-with-the-illegal-suffix-key"></a><a name="MT7027"/>MT7027: Der Watch-Erweiterung "\*" hat eine CFBundleIdentifier (\*) mit dem unzulässige Suffix ".key" endet.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7028mt7028-the-watch-extension--has-an-invalid-infoplist-it-does-not-contain-an-nsextension-dictionary"></a><a name="MT7028"/>MT7028: Der Watch-Erweiterung "*" hat eine ungültige Datei "Info.plist": er enthält kein NSExtension Wörterbuch.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7029mt7029-the-watch-extension--has-an-invalid-infoplist-the-nsextensionpointidentifier-must-be-comapplewatchkit"></a><a name="MT7029"/>MT7029: Der Watch-Erweiterung "*" hat eine ungültige Datei "Info.plist": die NSExtensionPointIdentifier muss "com.apple.watchkit" sein.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7030mt7030-the-watch-extension--has-an-invalid-infoplist-the-nsextension-dictionary-must-contain-nsextensionattributes"></a><a name="MT7030"/>MT7030: Der Watch-Erweiterung "*" hat eine ungültige Datei "Info.plist": das Wörterbuch NSExtension NSExtensionAttributes enthalten muss.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7031mt7031-the-watch-extension--has-an-invalid-infoplist-the-nsextensionattributes-dictionary-must-contain-a-wkappbundleidentifier"></a><a name="MT7031"/>MT7031: Der Watch-Erweiterung "*" hat eine ungültige Datei "Info.plist": das Wörterbuch NSExtensionAttributes muss eine WKAppBundleIdentifier enthalten.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7032mt7032-the-watchkit-extension--has-an-invalid-infoplist-uirequireddevicecapabilities-should-not-contain-the-watch-companion-capability"></a><a name="MT7032"/>MT7032: Die WatchKit-Erweiterung "*" hat eine ungültige Datei "Info.plist": UIRequiredDeviceCapabilities dürfen nicht für die Funktion "Watch-Companion".

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7033mt7033-the-watch-app--does-not-contain-an-infoplist"></a><a name="MT7033"/>MT7033: Watch-App "*" enthält eine Datei "Info.plist" nicht.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7034mt7034-the-watch-app--does-not-specify-a-cfbundleidentifier"></a><a name="MT7034"/>MT7034: Watch-App "*" gibt keine CFBundleIdentifier.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7035mt7035-the-watch-app--does-not-have-a-valid-uidevicefamily-value-expected--but-found--"></a><a name="MT7035"/>MT7035: Watch-App "\*" verfügt nicht über einen gültigen UIDeviceFamily-Wert. Erwartet '\*"gefunden wurde jedoch"\* (\*) ".

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7036mt7036-the-watch-app--does-not-specify-a-cfbundleexecutable"></a><a name="MT7036"/>MT7036: Watch-App "*" gibt keine CFBundleExecutable.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7037mt7037-the-watchkit-extension-extensionname-has-an-invalid-wkappbundleidentifier-value--it-does-not-match-the-watch-apps-cfbundleidentifier-"></a><a name="MT7037"/>MT7037: Die WatchKit-Erweiterung "{.-Kennung Erweiterungsname}" weist einen ungültigen WKAppBundleIdentifier-Wert ("\*"), entspricht nicht der Watch-App CFBundleIdentifier ("\*").

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7038mt7038-the-watch-app--has-an-invalid-infoplist-the-wkcompanionappbundleidentifier-must-exist-and-must-match-the-main-app-bundles-cfbundleidentifier"></a><a name="MT7038"/>MT7038: Watch-App "*" hat eine ungültige Datei "Info.plist": die WKCompanionAppBundleIdentifier muss vorhanden sein und muss mit der Haupt-app-Bündel CFBundleIdentifier übereinstimmen.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7039mt7039-the-watch-app--has-an-invalid-infoplist-the-lsrequiresiphoneos-key-must-not-be-present"></a><a name="MT7039"/>MT7039: Watch-App "*" hat eine ungültige Datei "Info.plist": der LSRequiresIPhoneOS-Schlüssel muss nicht vorhanden sein.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7040mt7040-the-app-bundle-appbundlepath-does-not-contain-an-infoplist"></a><a name="MT7040"/>MT7040: Die app-Bündel {AppBundlePath} enthält eine Datei "Info.plist" nicht.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7041mt7041-main-infoplist-path-does-not-specify-a-cfbundleidentifier"></a><a name="MT7041"/>MT7041: Main "Info.plist" Pfad Gibt eine CFBundleIdentifier keinen.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7042mt7042-main-infoplist-path-does-not-specify-a-cfbundleexecutable"></a><a name="MT7042"/>MT7042: Main "Info.plist" Pfad Gibt eine CFBundleExecutable keinen.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7043mt7043-main-infoplist-path-does-not-specify-a-cfbundlesupportedplatforms"></a><a name="MT7043"/>MT7043: Main "Info.plist" Pfad Gibt eine CFBundleSupportedPlatforms keinen.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7044mt7044-main-infoplist-path-does-not-specify-a-uidevicefamily"></a><a name="MT7044"/>MT7044: Main "Info.plist" Pfad Gibt eine UIDeviceFamily keinen.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

### <a name="a-namemt7045mt7045-unrecognized-format-"></a><a name="MT7045"/>MT7045: Unbekanntes Format: *.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

Wobei * sind möglich:

- Zeichenfolge
- array
- dict
- bool
- Reelle
- Ganze Zahl
- date
- Daten

### <a name="a-namemt7046mt7046-add-entry--incorrectly-specified"></a><a name="MT7046"/>MT7046: Hinzufügen: Eintrag, *, falsch angegeben.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

### <a name="a-namemt7047mt7047-add-entry--contains-invalid-array-index"></a><a name="MT7047"/>MT7047: Hinzufügen: Eintrag, *, enthält ungültigen Arrayindex.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

### <a name="a-namemt7048mt7048-add--entry-already-exists"></a><a name="MT7048"/>MT7048: Hinzufügen: * Eintrag bereits vorhanden ist.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

### <a name="a-namemt7049mt7049-add-cant-add-entry--to-parent"></a><a name="MT7049"/>MT7049: Hinzufügen: Eintrag kann nicht hinzugefügt *, mit dem übergeordneten.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

### <a name="a-namemt7050mt7050-delete-cant-delete-entry--from-parent"></a><a name="MT7050"/>MT7050: Löschen: Eintrag kann nicht gelöscht werden *, vom übergeordneten Element.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

### <a name="a-namemt7051mt7051-delete-entry--contains-invalid-array-index"></a><a name="MT7051"/>MT7051: Löschen: Eintrag, *, enthält ungültigen Arrayindex.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

### <a name="a-namemt7052mt7052-delete-entry--does-not-exist"></a><a name="MT7052"/>MT7052: Löschen: Eintrag, *, ist nicht vorhanden.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

### <a name="a-namemt7053mt7053-import-entry--incorrectly-specified"></a><a name="MT7053"/>MT7053: Importieren: Eintrag, *, falsch angegeben.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

### <a name="a-namemt7054mt7054-import-entry--contains-invalid-array-index"></a><a name="MT7054"/>MT7054: Importieren: Eintrag, *, enthält ungültigen Arrayindex.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

### <a name="a-namemt7055mt7055-import-error-reading-file-"></a><a name="MT7055"/>MT7055: Importieren: Fehler beim Lesen der Datei: *.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

### <a name="a-namemt7056mt7056-import-cant-add-entry--to-parent"></a><a name="MT7056"/>MT7056: Importieren: kann nicht hinzugefügt Eintrag, *, mit dem übergeordneten.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

### <a name="a-namemt7057mt7057-merge-cant-add-array-entries-to-dict"></a><a name="MT7057"/>MT7057: Zusammenführen: Arrayeinträge kann nicht hinzugefügt werden, um dict.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

### <a name="a-namemt7058mt7058-merge-specified-entry-must-be-a-container"></a><a name="MT7058"/>MT7058: Zusammenführen: angegebene Eintrag muss einem Container.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

### <a name="a-namemt7059mt7059-merge-entry--contains-invalid-array-index"></a><a name="MT7059"/>MT7059: Zusammenführen: Eintrag, *, enthält ungültigen Arrayindex.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

### <a name="a-namemt7060mt7060-merge-entry--does-not-exist"></a><a name="MT7060"/>MT7060: Zusammenführen: Eintrag, *, ist nicht vorhanden.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

### <a name="a-namemt7061mt7061-merge-error-reading-file-"></a><a name="MT7061"/>MT7061: Zusammenführen: Fehler beim Lesen der Datei: *.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

### <a name="a-namemt7062mt7062-set-entry--incorrectly-specified"></a><a name="MT7062"/>MT7062: Legen Sie: Eintrag, *, falsch angegeben.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

### <a name="a-namemt7063mt7063-set-entry--contains-invalid-array-index"></a><a name="MT7063"/>MT7063: Legen Sie: Eintrag, *, enthält ungültigen Arrayindex.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

### <a name="a-namemt7064mt7064-set-entry--does-not-exist"></a><a name="MT7064"/>MT7064: Legen Sie: Eintrag, *, ist nicht vorhanden.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

### <a name="a-namemt7065mt7065-unknown-propertylist-editor-action-"></a><a name="MT7065"/>MT7065: Unbekannte PropertyList Editor-Aktion: *.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

### <a name="a-namemt7066mt7066-error-loading--"></a><a name="MT7066"/>MT7066: Fehler beim Laden ' *': *.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

### <a name="a-namemt7067mt7067-error-saving--"></a><a name="MT7067"/>MT7067: Fehler beim Speichern einer ' *': *.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

## <a name="mt8xxx-runtime-error-messages"></a>MT8xxx: Common Language Runtime-Fehlermeldungen

<!--
 MT8xxx runtime
  MT800x misc
  -->

### <a name="a-namemt8001mt8001--version-mismatch-between-the-native-xamarinios-runtime-and-monotouchdll-please-reinstall-xamarinios"></a><a name="MT8001"/>MT8001 Versionskonflikt zwischen dem native Xamarin.iOS Common Language Runtime und monotouch.dll. Installieren Sie den Xamarin.iOS.

### <a name="a-namemt8002mt8002--could-not-find-the-method--in-the-type-"></a><a name="MT8002"/>Die Methode MT8002 wurde nicht gefunden "\*"im Typ"\*".

### <a name="a-namemt8003mt8003--failed-to-find-the-closed-generic-method--on-the-type-"></a><a name="MT8003"/>MT8003 Fehler beim Suchen des geschlossenen generischen Methode "\*"für den Typ"\*".

### <a name="a-namemt8004mt8004-cannot-create-an-instance-of--for-the-native-object-0x-of-type--because-another-instance-already-exists-for-this-native-object-of-type-"></a><a name="MT8004"/>MT8004: Eine Instanz kann nicht erstellt werden * für das systemeigene Objekt 0 X * (vom Typ "*"), da bereits eine andere Instanz für diese systemeigenes Objekt vorhanden ist (vom Typ *).

### <a name="a-namemt8005mt8005-wrapper-type--is-missing-its-native-objectivec-class-"></a><a name="MT8005"/>MT8005: Wrappertyp "\*"fehlt der systemeigenen ObjectiveC-Klasse"\*".

### <a name="a-namemt8006mt8006-failed-to-find-the-selector--on-the-type-"></a><a name="MT8006"/>MT8006: Fehler beim Suchen des des Selektors "\*"für den Typ"\*"

### <a name="a-namemt8007mt8007-cannot-get-the-method-descriptor-for-the-selector--on-the-type--because-the-selector-does-not-correspond-to-a-method"></a><a name="MT8007"/>MT8007: Der Methodendeskriptor kann nicht abgerufen werden, für die Auswahl "\*"für den Typ"\*", da die Auswahl einer Methode nicht entsprechen

### <a name="a-namemt8008mt8008-the-loaded-version-of-xamariniosdll-was-compiled-for--bits-while-the-process-is--bits-please-file-a-bug-at-httpbugzillaxamarincom"></a><a name="MT8008"/>MT8008: Die geladene Version von Xamarin.iOS.dll kompiliert wurde * bits, während der Prozess ist * Bits. Bitte melden Sie Fehler am http://bugzilla.xamarin.com.

Dies gibt an, dass etwas im Buildprozess nicht stimmt. Melden Sie den Fehler auf [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8009mt8009-unable-to-locate-the-block-to-delegate-conversion-method-for-the-method-s-parameter--please-file-a-bug-at-httpbugzillaxamarincom"></a><a name="MT8009"/>MT8009: Suchen Sie den Block zum Delegieren von Konvertierungsmethode für die Methode kann nicht *.*" s Parameter #*. Bitte melden Sie Fehler am http://bugzilla.xamarin.com.

Dies gibt an, dass eine API ordnungsgemäß gebunden war nicht. Ist dies eine API verfügbar gemacht werden, indem Sie Xamarin, senden Sie einen Fehlerbericht in unserer Bugzilla ([http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)), wenn es sich um eine Bindung eines Drittanbieters ist wenden Sie sich an den Hersteller.

### <a name="a-namemt8010mt8010-native-type-size-mismatch-between-xamariniosmacdll-and-the-executing-architecture-xamariniosmacdll-was-built-for--bit-while-the-current-process-is--bit"></a><a name="MT8010"/>MT8010: Systemeigene Größe Typenkonflikt zwischen Xamarin. [iOS | Mac-DLL und der ausgeführten-Architektur. Xamarin. [iOS | Mac-DLL erstellt wurde, für die *-bit, während der aktuelle Prozess ist *-Bit.

Dies gibt an, dass etwas im Buildprozess nicht stimmt. Melden Sie den Fehler auf [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8011mt8011-unable-to-locate-the-delegate-to-block-conversion-attribute-delegateproxy-for-the-return-value-for-the-method--please-file-a-bug-at-httpbugzillaxamarincom"></a><a name="MT8011"/>MT8011: Nicht der Delegat, der Block Konvertierung-Attribut ([DelegateProxy]) für den Rückgabewert der Methode gefunden *.*. Bitte melden Sie Fehler am http://bugzilla.xamarin.com.

Xamarin.iOS konnte eine erforderliche Methode zur Laufzeit (um einen Delegaten in einen Speicherblock zu konvertieren) zu suchen.

Dies weist in der Regel ein Fehler im Xamarin.iOS hin; Melden Sie den Fehler auf [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8012mt8012-invalid-delegateproxyattribute-for-the-return-value-for-the-method--delegatetype-is-null-please-file-a-bug-at-httpbugzillaxamarincom"></a><a name="MT8012"/>MT8012: Ungültige DelegateProxyAttribute für den Rückgabewert der Methode *.*: Delegattyp ist null. Bitte melden Sie Fehler am http://bugzilla.xamarin.com.

Das DelegateProxy-Attribut für die betreffende Methode ist ungültig.

Dies weist in der Regel ein Fehler im Xamarin.iOS hin; Melden Sie den Fehler auf [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8013mt8013-invalid-delegateproxyattribute-for-the-return-value-for-the-method--delegatetype-2-specifies-a-type-without-a-handler-field-please-file-a-bug-at-httpbugzillaxamarincom"></a><a name="MT8013"/>MT8013: Ungültige DelegateProxyAttribute für den Rückgabewert der Methode *.*: Delegattyp (\ {2\}) gibt einen Typ, ein Feld "Ereignishandler". Bitte melden Sie Fehler am http://bugzilla.xamarin.com.

Das DelegateProxy-Attribut für die betreffende Methode ist ungültig.

Dies weist in der Regel ein Fehler im Xamarin.iOS hin; Melden Sie den Fehler auf [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8014mt8014-invalid-delegateproxyattribute-for-the-return-value-for-the-method--the-delegatetypes-2-handler-field-is-null-please-file-a-bug-at-httpbugzillaxamarincom"></a><a name="MT8014"/>MT8014: Ungültige DelegateProxyAttribute für den Rückgabewert der Methode *.*: der Delegattyps (\ {2\,}) 'Handler'-Feld ist null. Bitte melden Sie Fehler am http://bugzilla.xamarin.com.

Das DelegateProxy-Attribut für die betreffende Methode ist ungültig.

Dies weist in der Regel ein Fehler im Xamarin.iOS hin; Melden Sie den Fehler auf [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8015mt8015-invalid-delegateproxyattribute-for-the-return-value-for-the-method--the-delegatetypes-2-handler-field-is-not-a-delegate-its-a--please-file-a-bug-at-httpbugzillaxamarincom"></a><a name="MT8015"/>MT8015: Ungültige DelegateProxyAttribute für den Rückgabewert der Methode *.*: der Delegattyps (\ {2\,}) 'Handler'-Feld keinen Delegaten, ist es eine *. Bitte melden Sie Fehler am http://bugzilla.xamarin.com.

Das DelegateProxy-Attribut für die betreffende Methode ist ungültig.

Dies weist in der Regel ein Fehler im Xamarin.iOS hin; Melden Sie den Fehler auf [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8016mt8016-unable-to-convert-delegate-to-block-for-the-return-value-for-the-method--because-the-input-isnt-a-delegate-its-a--please-file-a-bug-at-httpbugzillaxamarincom"></a><a name="MT8016"/>MT8016: Kann nicht konvertiert werden Delegaten für den Rückgabewert der Methode blockiert *.*, da die Eingabe eines Delegaten ist es eine *. Bitte melden Sie Fehler am http://bugzilla.xamarin.com.

Das DelegateProxy-Attribut für die betreffende Methode ist ungültig.

Dies weist in der Regel ein Fehler im Xamarin.iOS hin; Melden Sie den Fehler auf [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<!-- 8017 is used by mmp -->

### <a name="a-namemt8018mt8018-internal-consistency-error-please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT8018"/>MT8018: Interner Konsistenzfehler. Bitte Datei einen Fehlerbericht an http://bugzilla.xamarin.com.

Hiermit wird einen Fehler im Xamarin.iOS. Melden Sie den Fehler auf [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8019mt8019-could-not-find-the-assembly--in-the-loaded-assemblies"></a><a name="MT8019"/>MT8019: Die Assembly wurde nicht gefunden * in den geladenen Assemblys.

Hiermit wird einen Fehler im Xamarin.iOS. Melden Sie den Fehler auf [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8020mt8020-could-not-find-the-module-with-metadatatoken--in-the-assembly-"></a><a name="MT8020"/>MT8020: Das Modul mit MetadataToken nicht gefunden * in der Assembly *.

Hiermit wird einen Fehler im Xamarin.iOS. Melden Sie den Fehler auf [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8021mt8021-unknown-implicit-token-type-"></a><a name="MT8021"/>MT8021: Unbekannte implizite Tokentyp: *.

Hiermit wird einen Fehler im Xamarin.iOS. Melden Sie den Fehler auf [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8022mt8022-expected-the-token-reference--to-be-a--but-its-a--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT8022"/>MT8022: Erwartet der Tokenverweis * werden eine *, aber es ist eine *. Bitte Datei einen Fehlerbericht an http://bugzilla.xamarin.com.

Hiermit wird einen Fehler im Xamarin.iOS. Melden Sie den Fehler auf [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8023mt8023-an-instance-object-is-required-to-construct-a-closed-generic-method-for-the-open-generic-method--token-reference--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT8023"/>MT8023: Ein Instanzobjekt ist erforderlich, um eine geschlossene generische Methode für die offene generische Methode erstellen: * (token-Referenz: *). Bitte Datei einen Fehlerbericht an http://bugzilla.xamarin.com.

Hiermit wird einen Fehler im Xamarin.iOS. Melden Sie den Fehler auf [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8024mt8024-could-not-find-a-valid-extension-type-for-the-smart-enum-smarttype-please-file-a-bug-at-httpsbugzillaxamarincom"></a><a name="MT8024"/>MT8024: Gültiger Erweiterungstyp für intelligente Enum "{Smart_type}" wurde nicht gefunden werden. Bitte melden Sie Fehler am https://bugzilla.xamarin.com.

Hiermit wird einen Fehler im Xamarin.iOS. Melden Sie den Fehler auf [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).
