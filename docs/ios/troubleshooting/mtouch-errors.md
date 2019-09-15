---
title: Xamarin. IOS-Fehler
description: In diesem Dokument werden verschiedene Fehler beschrieben, die von mfinger generiert werden, das Tool, mit dem xamarin. IOS-Anwendungen gebündelt werden. Fehler werden nach Code aufgeführt und erhalten eine vollständige Beschreibung.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9F76162B-D622-45DA-996B-2FBF8017E208
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/06/2018
ms.openlocfilehash: 870765a7a32874dfa17a9b0cf7176e4a721abf47
ms.sourcegitcommit: cf56d2bae34dc0f8e94c2d3d28d5f460d59807bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2019
ms.locfileid: "70985960"
---
# <a name="xamarinios-errors"></a>Xamarin. IOS-Fehler

## <a name="mt0xxx-mtouch-error-messages"></a>MT0xxx: mfinger-Fehlermeldungen

Beispiel: Parameter, Umgebung, fehlende Tools.

<!--
 MT0xxx mtouch itself, e.g. parameters, environment (e.g. missing tools)
 https://github.com/xamarin/xamarin-macios/blob/master/tools/mtouch/error.cs
  -->

<a name="MT0000" />

### <a name="mt0000-unexpected-error---please-fill-a-bug-report-at-httpsgithubcomxamarinxamarin-maciosissuesnew"></a>MT0000: Unerwarteter Fehler: füllen Sie einen Fehlerbericht unter https://github.com/xamarin/xamarin-macios/issues/new

Es ist ein unerwarteter Fehler aufgetreten. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) mit so vielen Informationen wie möglich, einschließlich:

- Vollständige Buildprotokolle mit maximaler Ausführlichkeit (z `-v -v -v -v` . b. in den **zusätzlichen mberührungs-Argumenten**);
- Ein minimaler Testfall, der den Fehler reproduziert. immer
- Alle Versionsinformationen

Die einfachste Möglichkeit, um genaue Versionsinformationen zu erhalten, ist die Verwendung des Menüs " **Visual Studio für Mac** ", Informationen zu **Visual Studio für Mac** Element, Schaltfläche " **Details anzeigen** " und Kopieren/Einfügen der Versionsinformationen (die Schaltfläche " **Informationen kopieren** " können Sie verwenden) .

<a name="MT0001" />

### <a name="mt0001--devname-was-provided-without-any-device-specific-action"></a>MT0001: "-devname" wurde ohne gerätespezifische Aktion bereitgestellt.

Dies ist eine Warnung, die ausgegeben wird, wenn "-devname" an mberühren übermittelt wird, wenn keine gerätespezifische Aktion (-logdev/-installdev/-killdev/-launchdev/-listapps) angefordert wurde.

<a name="MT0002" />

### <a name="mt0002-could-not-parse-the-environment-variable-"></a>MT0002: Die Umgebungsvariable "*" konnte nicht analysiert werden.

Dieser Fehler tritt auf, wenn Sie versuchen, ein ungültiges Umgebungs Schlüssel-/Wert-Variablenpaar festzulegen. Das richtige Format lautet wie folgt:`mtouch --setenv=VARIABLE=VALUE`

<a name="MT0003" />

### <a name="mt0003-application-name-exe-conflicts-with-an-sdk-or-product-assembly-dll-name"></a>MT0003: Der Anwendungsname "*. exe" steht in Konflikt mit einem SDK-oder produktassemblynamen (. dll).

Der Name der ausführbaren Assembly und der Name der Anwendung dürfen nicht mit dem Namen einer beliebigen DLL in der APP identisch sein. Ändern Sie den Namen der ausführbaren Datei.

<a name="MT0004" />

### <a name="mt0004-new-refcounting-logic-requires-sgen-to-be-enabled-too"></a>MT0004: Die neue refcounting-Logik erfordert auch, dass Sgen aktiviert ist.

Wenn Sie die Erweiterung "ref" aktivieren, müssen Sie auch die SGen-Garbage Collector in den IOS-Buildoptionen des Projekts aktivieren (Registerkarte "Erweitert").

Ab xamarin. IOS 7.2.1 diese Anforderung wurde angehoben, die neue refcounting-Logik kann sowohl mit Boehm als auch mit Sgen-Garbage Collectors aktiviert werden.

<a name="MT0005" />

### <a name="mt0005-the-output-directory--does-not-exist"></a>MT0005: Das Ausgabeverzeichnis * ist nicht vorhanden.

Erstellen Sie das Verzeichnis.

Dieser Fehler wird nicht mehr generiert, da mfinger das Verzeichnis automatisch erstellt, wenn es nicht vorhanden ist.

<a name="MT0006" />

### <a name="mt0006-there-is-no-devel-platform-at--use---platformplat-to-specify-the-sdk"></a>MT0006: Es gibt keine Debug-Plattform bei *, verwenden Sie--Platform = Plat, um das SDK anzugeben.

Xamarin. IOS kann das SDK-Verzeichnis nicht an dem Speicherort finden, der in der Fehlermeldung angegeben ist. Überprüfen Sie, ob der Pfad richtig ist.

<a name="MT0007" />

### <a name="mt0007-the-root-assembly--does-not-exist"></a>MT0007: Die Stammassembly * ist nicht vorhanden.

Xamarin. IOS kann die Assembly nicht an dem Speicherort finden, der in der Fehlermeldung angegeben ist. Überprüfen Sie, ob der Pfad richtig ist.

<a name="MT0008" />

### <a name="mt0008-you-should-provide-one-root-assembly-only-found--assemblies-"></a>MT0008: Sie sollten nur eine Stammassembly bereitstellen, gefunden wurden # Assemblys: *.

Es wurden mehrere Stammassemblys an mberühren übermittelt, es kann jedoch nur eine Stammassembly vorhanden sein.

<a name="MT0009" />

### <a name="mt0009-error-while-loading-assemblies-"></a>MT0009: Fehler beim Laden von Assemblys: *.

Fehler beim Laden der Assemblys aus den Stamm-Assemblyverweisen. Weitere Informationen finden Sie möglicherweise in der Buildausgabe.

<a name="MT0010" />

### <a name="mt0010-could-not-parse-the-command-line-arguments-"></a>MT0010: Die Befehlszeilenargumente konnten nicht analysiert werden: *.

Fehler beim Parsen der Befehlszeilenargumente. Vergewissern Sie sich, dass alle korrekt sind.

<a name="MT0011" />

### <a name="mt0011--was-built-against-a-more-recent-runtime--than-monotouch-supports"></a>MT0011: * wurde mit einer neueren Laufzeit (*) erstellt, als MonoTouch unterstützt.

Diese Warnung wird in der Regel gemeldet, weil das Projekt über einen Verweis auf eine Klassenbibliothek verfügt, die nicht mit xamarin. IOS BCL erstellt wurde.

Dieselbe Art und Weise, wie eine APP, die das .NET 4,0 SDK verwendet, funktioniert nicht auf einem System, das nur .NET 2,0 unterstützt, eine mit .NET 4,0 entwickelte Bibliothek funktioniert möglicherweise nicht in xamarin. IOS. Sie verwendet möglicherweise die API, die nicht auf xamarin. IOS vorhanden ist.

Die allgemeine Lösung besteht darin, die Bibliothek als xamarin. IOS-Klassenbibliothek zu erstellen. Dies kann erreicht werden, indem ein neues xamarin. IOS-Klassen Bibliotheksprojekt erstellt und alle Quelldateien hinzugefügt werden. Wenn Sie nicht über den Quellcode für die Bibliothek verfügen, wenden Sie sich an den Hersteller, und bitten Sie ihn, eine xamarin. IOS-kompatible Version der Bibliothek bereitzustellen.

<a name="MT0012" />

### <a name="mt0012-incomplete-data-is-provided-to-complete-"></a>MT0012: Unvollständige Daten werden für den Abschluss von * bereitgestellt.

Dieser Fehler wird in der aktuellen Version von xamarin. IOS nicht mehr gemeldet.

<a name="MT0013" />

### <a name="mt0013-profiling-support-requires-sgen-to-be-enabled-too"></a>MT0013: Die Profil Erstellungs Unterstützung erfordert auch, dass Sgen aktiviert ist.

Sgen (--Sgen) muss aktiviert sein, wenn die Profilerstellung (--Profilerstellung) aktiviert ist.

<a name="MT0014" />

### <a name="mt0014-the-ios--sdk-does-not-support-building-applications-targeting-"></a>MT0014: Das IOS * SDK unterstützt das Entwickeln von Anwendungen, die auf * abzielen, nicht.

Dies kann in den folgenden Situationen vorkommen:

- ARMv6 ist aktiviert, und Xcode 4,5 oder höher ist installiert.
- ARMv7s ist aktiviert, und Xcode 4,4 oder früher ist installiert.

Überprüfen Sie, ob die installierte Version von Xcode die ausgewählten Architekturen unterstützt.

<a name="MT0015" />

### <a name="mt0015-invalid-abi--supported-abis-are-i386-x86_64--armv7-armv7llvm-armv7llvmthumb2-armv7s-armv7sllvm-armv7sllvmthumb2-arm64-and-arm64llvm"></a>MT0015: Ungültige ABI: *. Unterstützte ABIS: i386, x86_64, armv7, armv7 + llvm, armv7 + llvm + thumb2, armv7s, armv7s + llvm, armv7s + llvm + thumb2, arm64 und arm64 + llvm.

Es wurde eine ungültige ABI an mberühren übermittelt. Geben Sie eine gültige ABI an.

<a name="MT0016" />

### <a name="mt0016-the-option--has-been-deprecated"></a>MT0016: Die Option * ist veraltet.

Die angegebene mtouchscreen-Option ist veraltet und wird ignoriert.

<a name="MT0017" />

### <a name="mt0017-you-should-provide-a-root-assembly"></a>MT0017: Sie sollten eine Stammassembly bereitstellen.

Beim Entwickeln einer APP muss eine Stammassembly (in der Regel die ausführbare Datei) angegeben werden.

<a name="MT0018" />

### <a name="mt0018-unknown-command-line-argument-"></a>MT0018: Unbekanntes Befehlszeilenargument: *.

Das in der Fehlermeldung erwähnte Befehlszeilenargument wird nicht von mfinger erkannt.

<a name="MT0019" />

### <a name="mt0019-only-one---loginstallkilllaunchdev-or---launchdebugsim-option-can-be-used"></a>MT0019: Es kann nur eine--[Log | install | Kill | Launch] dev oder--[Launch | Debug] SIM-Option verwendet werden.

Es gibt mehrere Optionen für mtouchscreen, die nicht gleichzeitig verwendet werden können:

- --logdev
- --installdev
- --killdev
- --launchdev
- --launchdebug
- --launchsim

<a name="MT0020" />

### <a name="mt0020-the-valid-options-for--are-"></a>MT0020: Die gültigen Optionen für "\*" lauten "\*".

<a name="MT0021" />

### <a name="mt0021-cannot-compile-using-gccg---use-gcc-when-using-the-static-registrar-this-is-the-default-when-compiling-for-device-either-remove-the---use-gcc-flag-or-use-the-dynamic-registrar---registrardynamic"></a>MT0021: Kann nicht mithilfe von gcc/g + + (--Use-gcc) kompiliert werden, wenn die statische Registrierungsstelle verwendet wird (Dies ist die Standardeinstellung bei der Kompilierung für das Gerät). Entfernen Sie entweder das Flag "--Use-gcc", oder verwenden Sie die dynamische Registrierungsstelle (--Registrierungsstelle: dynamisch).

<a name="MT0022" />

### <a name="mt0022-the-options---unsupported--enable-generics-in-registrar-and---registrar-are-not-compatible"></a>MT0022: Die Optionen "--nicht unterstützt--enable-Generika-in-Registrierungs" und "--Registrierungs" sind nicht kompatibel.

Entfernen Sie die Optionen `--unsupported--enable-generics-in-registrar` und `--registrar`. Beginnend mit xamarin. IOS 7.2.1 die Standard Registrierungsstelle unterstützt Generika.

Dieser Fehler wird nicht mehr angezeigt (das Befehlszeilenargument `--unsupported--enable-generics-in-registrar` wurde aus mberühren entfernt).

<a name="MT0023" />

### <a name="mt0023-application-name-exe-conflicts-with-another-user-assembly"></a>MT0023: Der Anwendungsname "*. exe" steht in Konflikt mit einer anderen Benutzerassembly.

Der Name der ausführbaren Assembly und der Name der Anwendung dürfen nicht mit dem Namen einer beliebigen DLL in der APP identisch sein. Ändern Sie den Namen der ausführbaren Datei.

<a name="MT0024" />

### <a name="mt0024-could-not-find-required-file-"></a>MT0024: Die erforderliche Datei "*" konnte nicht gefunden werden.

<a name="MT0025" />

### <a name="mt0025-no-sdk-version-was-provided-please-add---sdkxy-to-specify-which-ios-sdk-should-be-used-to-build-your-application"></a>MT0025: Es wurde keine SDK-Version bereitgestellt. Fügen Sie `--sdk=X.Y` hinzu, um anzugeben, welches IOS SDK zum Erstellen der Anwendung verwendet werden soll.

<a name="MT0026" />

### <a name="mt0026-could-not-parse-the-command-line-argument--"></a>MT0026: Das Befehlszeilenargument "*" konnte nicht analysiert werden: *

<a name="MT0027" />

### <a name="mt0027-the-options--and--are-not-compatible"></a>MT0027: Die Optionen "\*" und "\*" sind nicht kompatibel.

<a name="MT0028" />

### <a name="mt0028-cannot-enable-pie--pie-when-targeting-ios-41-or-earlier-please-disable-pie--piefalse-or-set-the-deployment-target-to-at-least-ios-42"></a>MT0028: Kreis (-Kreis) kann nicht aktiviert werden, wenn für IOS 4,1 oder früher verwendet wird. Deaktivieren Sie den Kreis (-Pie: false), oder legen Sie das Bereitstellungs Ziel auf mindestens IOS 4,2 fest.

<a name="MT0029" />

### <a name="mt0029-repl---enable-repl-is-only-supported-in-the-simulator---sim"></a>MT0029: REPL (--Enable-repl) wird nur im Simulator (--SIM) unterstützt.

REPL wird nur unterstützt, wenn Sie für den Simulator entwickeln. Dies bedeutet, dass Sie bei `--enable-repl` der Übergabe an mberühren auch übergeben `--sim`müssen.

<a name="MT0030" />

### <a name="mt0030-the-executable-name--and-the-app-name--are-different-this-may-prevent-crash-logs-from-getting-symbolicated-properly"></a>MT0030: Der Name der Ausführ\*baren Datei () und der\*App-Name () sind unterschiedlich. Dies verhindert möglicherweise, dass Absturz Protokolle ordnungsgemäß symbolisiert werden.

Wenn Xcode ein Symbol für die Funktionsnamen und Datei-/Zeilennummern von Xcode bildet (übersetzt), schlägt der Prozess möglicherweise fehl, wenn die ausführbare Datei und die APP andere Namen aufweisen (ohne Erweiterung).

Um dieses Problem zu beheben, ändern Sie "Anwendungsname" in den Build-/IOS-Anwendungs Optionen des Projekts, oder ändern Sie "Assemblyname" in den Build-/Ausgabeoptionen des Projekts.

<a name="MT0031" />

### <a name="mt0031-the-command-line-arguments---enable-background-fetch-and---launch-for-background-fetch-require---launchsim-too"></a>MT0031: Die Befehlszeilenargumente '--Enable-background-Fetch ' und '--Launch-for-background-Fetch ' erfordern ebenfalls '--launchsim '.

<a name="MT0032" />

### <a name="mt0032-the-option---debugtrack-is-ignored-unless---debug-is-also-specified"></a>MT0032: Die Option "--debugtrack" wird ignoriert, es sei denn, "--Debug" wird ebenfalls angegeben.

<a name="MT0033" />

### <a name="mt0033-a-xamarinios-project-must-reference-either-monotouchdll-or-xamariniosdll"></a>MT0033: Ein xamarin. IOS-Projekt muss entweder MonoTouch. dll oder xamarin. IOS. dll referenzieren.

<a name="MT0034" />

### <a name="mt0034-cannot-include-both-monotouchdll-and-xamariniosdll-in-the-same-xamarinios-project----is-referenced-explicitly-while--is-referenced-by-"></a>MT0034: "MonoTouch. dll" und "xamarin. IOS. dll" können nicht im gleichen xamarin. IOS-\*Projekt enthalten sein. es wird explizit auf "" verwiesen, während auf "\*" von "*" verwiesen wird.

<!-- MT0035 unused -->

<a name="MT0036" />

### <a name="mt0036-cannot-launch-a--simulator-for-a--app-please-enable-the-correct-architectures-in-your-projects-ios-build-options-advanced-page"></a>MT0036: Ein *-Simulator für eine *-APP kann nicht gestartet werden. Aktivieren Sie die richtige Architektur (en) in den IOS-Buildoptionen Ihres Projekts (Seite "Erweitert").

<a name="MT0037" />

### <a name="mt0037-monotouchdll-is-not-64-bit-compatible-either-reference-xamariniosdll-or-do-not-build-for-a-64-bit-architecture-arm64-andor-x86_64"></a>MT0037: MonoTouch. dll ist nicht 64-Bit-kompatibel. Verweisen Sie entweder auf xamarin. IOS. dll, oder erstellen Sie nicht für eine 64-Bit-Architektur (ARM64 und/oder x86_64).

<a name="MT0038" />

### <a name="mt0038-the-old-registrars---registraroldstaticolddynamic-are-not-supported-when-referencing-xamariniosdll"></a>MT0038: Die alten Registrierungen (--Registrierungsstelle: oldstatic | olddynamic) werden nicht unterstützt, wenn auf xamarin. IOS. dll verwiesen wird.

<a name="MT0039" />

### <a name="mt0039-applications-targeting-armv6-cannot-reference-xamariniosdll"></a>MT0039: Anwendungen, die auf ARMv6 abzielen, können nicht auf xamarin. IOS. dll verweisen.

<a name="MT0040" />

### <a name="mt0040-could-not-find-the-assembly--referenced-by-"></a>MT0040: Die Assembly "\*", auf die von "\*" verwiesen wird, wurde nicht gefunden.

<a name="MT0041" />

### <a name="mt0041-cannot-reference-both-monotouchdll-and-xamariniosdll"></a>MT0041: Auf ' MonoTouch. dll ' und ' xamarin. IOS. dll ' kann nicht verwiesen werden.

<a name="MT0042" />

### <a name="mt0042-no-reference-to-either-monotouchdll-or-xamariniosdll-was-found-a-reference-to-monotouchdll-will-be-added"></a>MT0042: Es wurde kein Verweis auf ' MonoTouch. dll ' oder ' xamarin. IOS. dll ' gefunden. Ein Verweis auf "MonoTouch. dll" wird hinzugefügt.

<a name="MT0043" />

### <a name="mt0043-the-boehm-garbage-collector-is-currently-not-supported-when-referencing-xamariniosdll-the-sgen-garbage-collector-has-been-selected-instead"></a>MT0043: Der Boehm-Garbage Collector wird derzeit nicht unterstützt, wenn auf "xamarin. IOS. dll" verwiesen wird. Stattdessen wird die SGen-Garbage Collector ausgewählt.

Nur die SGen-Garbage Collector wird mit Unified projects unterstützt. Stellen Sie sicher, dass keine zusätzlichen mtouchscreen-Flags vorhanden sind, die Boehm als Garbage Collector angeben.

<a name="MT0044" />

### <a name="mt0044---listsim-is-only-supported-with-xcode-60-or-later"></a>MT0044:--listsim wird nur mit Xcode 6,0 oder höher unterstützt.

Installieren Sie eine neuere Xcode-Version.

<a name="MT0045" />

### <a name="mt0045---extension-is-only-supported-when-using-the-ios-80-or-later-sdk"></a>MT0045:--Extension wird nur unterstützt, wenn das IOS 8,0 (oder höher) SDK verwendet wird.

<!-- MT0046 is not reported anymore -->

<a name="MT0047" />

### <a name="mt0047-the-minimum-deployment-target-for-unified-applications-is-511-the-current-deployment-target-is--please-select-a-newer-deployment-target-in-your-projects-ios-application-options"></a>MT0047: Das minimale Bereitstellungs Ziel für vereinheitlichte Anwendungen ist 5.1.1, das aktuelle Bereitstellungs Ziel ist "*". Wählen Sie ein neueres Bereitstellungs Ziel in den IOS-Anwendungs Optionen Ihres Projekts aus.

<!-- MT0048 is not reported anymore -->

<a name="MT0049" />

### <a name="mt0049-framework-is-supported-only-if-deployment-target-is-80-or-later--features-might-not-work-correctly"></a>MT0049: *. Framework wird nur unterstützt, wenn das Bereitstellungs Ziel 8,0 oder höher ist. * Features funktionieren möglicherweise nicht ordnungsgemäß.

Das angegebene Framework wird in der IOS-Version, auf die sich das Bereitstellungs Ziel bezieht, nicht unterstützt. Aktualisieren Sie entweder das Bereitstellungs Ziel auf eine neuere IOS-Version, oder entfernen Sie die Verwendung des angegebenen Frameworks aus der app.

<!-- MT0050 is not reported anymore -->

<a name="MT0051" />

### <a name="mt0051-xamarinios--requires-xcode-50-or-later-the-current-xcode-version-found-in--is-"></a>MT0051: Xamarin. IOS * erfordert Xcode 5,0 oder höher. Die aktuelle Xcode-Version (in * gefunden) ist *.

Installieren Sie einen neueren Xcode.

<a name="MT0052" />

### <a name="mt0052-no-command-specified"></a>MT0052: Es wurde kein Befehl angegeben.

Es wurde keine Aktion für mberühren angegeben.

<!-- 0053 is used by mmp -->

<a name="MT0054" />

### <a name="mt0054-unable-to-canonicalize-the-path--"></a>MT0054: Der Pfad ' * ' kann nicht kanonisiert werden: *

Dies ist ein interner Fehler. Wenn dieser Fehler angezeigt wird, melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT0055" />

### <a name="mt0055-the-xcode-path--does-not-exist"></a>MT0055: Der Xcode-Pfad ' * ' ist nicht vorhanden.

Der mit über `--sdkroot` gegebene Xcode-Pfad ist nicht vorhanden. Geben Sie einen gültigen Pfad an.

<a name="MT0056" />

### <a name="mt0056-cannot-find-xcode-in-the-default-location-applicationsxcodeapp-please-install-xcode-or-pass-a-custom-path-using---sdkroot-path"></a>MT0056: Xcode kann nicht im Standard Speicherort (/Applications/Xcode.app) gefunden werden. Installieren Sie Xcode, oder übergeben Sie einen benutzerdefinierten Pfad mithilfe von "- \<-SDKRoot Path >".

<a name="MT0057" />

### <a name="mt0057-cannot-determine-the-path-to-xcodeapp-from-the-sdk-root--please-specify-the-full-path-to-the-xcodeapp-bundle"></a>MT0057: Der Pfad zu "Xcode. app" kann nicht aus dem SDK-Stamm "*" ermittelt werden. Geben Sie den vollständigen Pfad zum Xcode. app Bundle an.

Der mit `--sdkroot` angegebene Pfad gibt keine gültige Xcode-APP an. Geben Sie einen Pfad zu einer Xcode-APP an.

<a name="MT0058" />

### <a name="mt0058-the-xcodeapp--is-invalid-the-file--does-not-exist"></a>MT0058: Die Xcode. app '\*' ist ungültig (die Datei '\*' ist nicht vorhanden).

Der mit `--sdkroot` angegebene Pfad gibt keine gültige Xcode-APP an. Geben Sie einen Pfad zu einer Xcode-APP an.

<a name="MT0059" />

### <a name="mt0059-could-not-find-the-currently-selected-xcode-on-the-system-"></a>MT0059: Der aktuell ausgewählte Xcode wurde im System nicht gefunden: *

<a name="MT0060" />

### <a name="mt0060-could-not-find-the-currently-selected-xcode-on-the-system-xcode-select---print-path-returned--but-that-directory-does-not-exist"></a>MT0060: Der aktuell ausgewählte Xcode wurde im System nicht gefunden. "Xcode-Select--Print-Path" hat "*" zurückgegeben, aber dieses Verzeichnis ist nicht vorhanden.

<a name="MT0061" />

### <a name="mt0061-no-xcodeapp-specified-using---sdkroot-using-the-system-xcode-as-reported-by-xcode-select---print-path-"></a>MT0061: Es wurde keine Xcode. app angegeben (mithilfe von "--SDKRoot"), wobei der von "Xcode-Select--Print-Path" gemeldete System-Xcode verwendet wird: *

Diese Meldung dient nur zu Informationszwecken und erläutert, welcher Xcode verwendet werden soll, da kein Wert angegeben wurde.

<a name="MT0062" />

### <a name="mt0062-no-xcodeapp-specified-using---sdkroot-or-xcode-select---print-path-using-the-default-xcode-instead-"></a>MT0062: Es wurde keine Xcode. app angegeben (mit "--SDKRoot" oder "Xcode-Select--Print-Path"). stattdessen wird der standardmäßige Xcode verwendet: *

Diese Meldung dient nur zu Informationszwecken und erläutert, welcher Xcode verwendet werden soll, da kein Wert angegeben wurde.

<a name="MT0063" />

### <a name="mt0063-cannot-find-the-executable-in-the-extension--no-cfbundleexecutable-entry-in-its-infoplist"></a>MT0063: Die ausführbare Datei wurde in der Extension * (kein cfbundledeployment-Eintrag in der Datei "Info. plist") gefunden.

Jede "Info. plist" muss über eine ausführbare Datei (mit dem Eintrag "cfbundledeployment") verfügen. ein Eintrag sollte jedoch während des Builds automatisch generiert werden.

Dies weist normalerweise auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) mit einem Testfall.

<a name="MT0064" />

### <a name="mt0064-xamarinios-only-supports-embedded-frameworks-with-unified-projects"></a>MT0064: Xamarin. IOS unterstützt nur eingebettete Frameworks mit vereinheitlichten Projekten.

Xamarin. IOS unterstützt nur eingebettete Frameworks, wenn die Unified API verwendet werden. Aktualisieren Sie Ihr Projekt, um die Unified API zu verwenden.

<a name="MT0065" />

### <a name="mt0065-xamarinios-only-supports-embedded-frameworks-when-deployment-target-is-at-least-80-current-deployment-target--embedded-frameworks-"></a>MT0065: Xamarin. IOS unterstützt nur eingebettete Frameworks, wenn das Bereitstellungs Ziel mindestens 8,0 (Aktuelles Bereitstellungs Ziel: * eingebettete Frameworks: *) ist.

Xamarin. IOS unterstützt nur eingebettete Frameworks, wenn das Bereitstellungs Ziel mindestens 8,0 ist (da frühere Versionen von IOS keine eingebetteten Frameworks unterstützen).

Aktualisieren Sie das Bereitstellungs Ziel in der Info. plist des Projekts auf 8,0 oder höher.

<a name="MT0066" />

### <a name="mt0066-invalid-build-registrar-assembly-"></a>MT0066: Ungültige assemblyregistrierungs-Assembly: *

Dies weist normalerweise auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) mit einem Testfall.

<a name="MT0067" />

### <a name="mt0067-invalid-registrar-"></a>MT0067: Ungültige Registrierungsstelle: *

Dies weist normalerweise auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) mit einem Testfall.

<a name="MT0068" />

### <a name="mt0068-invalid-value-for-target-framework-"></a>MT0068: Ungültiger Wert für Ziel Framework: *.

Ein ungültiges Ziel Framework wurde mit dem Argument "--Target-Framework" übermittelt. Geben Sie ein gültiges Ziel Framework an.

<a name="MT0069" />

<!--### MT0069: currently unused -->

<a name="MT0070" />

### <a name="mt0070-invalid-target-framework--valid-target-frameworks-are-"></a>MT0070: Ungültiges Ziel Framework: *. Gültige Ziel Frameworks sind: *.

Ein ungültiges Ziel Framework wurde mit dem Argument "--Target-Framework" übermittelt. Geben Sie ein gültiges Ziel Framework an.

<a name="MT0071" />

### <a name="mt0071-unknown-platform--this-usually-indicates-a-bug-in-xamarinios-please-file-a-bug-report-at-httpbugzillaxamarincom-with-a-test-case"></a>MT0071: Unbekannte Plattform: *. Dies weist normalerweise auf einen Fehler in xamarin. IOS hin. Melden Sie einen Fehlerbericht bei http://bugzilla.xamarin.com mit einem Testfall.

Dies weist normalerweise auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) mit einem Testfall.

<a name="MT0072" />

### <a name="mt0072-extensions-are-not-supported-for-the-platform-"></a>MT0072: Erweiterungen werden für die Plattform "*" nicht unterstützt.

Dies weist normalerweise auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) mit einem Testfall.

<a name="MT0073" />

### <a name="mt0073-xamarinios--does-not-support-a-deployment-target-of--the-minimum-is--please-select-a-newer-deployment-target-in-your-projects-infoplist"></a>MT0073: Xamarin. IOS * unterstützt kein Bereitstellungs Ziel von * (das Minimalwert ist *). Wählen Sie in der Datei "Info. plist" des Projekts ein neueres Bereitstellungs Ziel aus.

Das minimale Bereitstellungs Ziel ist das in der Fehlermeldung angegebene. Wählen Sie in der Datei "Info. plist" des Projekts ein neueres Bereitstellungs Ziel aus.

Wenn das Aktualisieren des Bereitstellungs Ziels nicht möglich ist, verwenden Sie eine ältere Version von xamarin. IOS.

<a name="MT0074" />

### <a name="mt0074-xamarinios--does-not-support-a-minimum-deployment-target-of--the-maximum-is--please-select-an-older-deployment-target-in-your-projects-infoplist-or-upgrade-to-a-newer-version-of-xamarinios"></a>MT0074: Xamarin. IOS * unterstützt kein minimal Bereitstellungs Ziel von * (das Maximum ist *). Wählen Sie ein älteres Bereitstellungs Ziel in der Info. plist-Datei Ihres Projekts aus, oder führen Sie ein Upgrade auf eine neuere Version von xamarin. IOS aus.

Xamarin. IOS unterstützt nicht das Festlegen des minimalen Bereitstellungs Ziels auf eine höhere Version als die Version, für die diese bestimmte Version von xamarin. IOS erstellt wurde.

Wählen Sie ein älteres Minimum Bereitstellungs Ziel in der Info. plist-Datei des Projekts aus, oder führen Sie ein Upgrade auf eine neuere Version von xamarin. IOS aus.

<a name="MT0075" />

### <a name="mt0075-invalid-architecture--for--projects-valid-architectures-are-"></a>MT0075: Ungültige Architektur ' * ' für *-Projekte. Gültige Architekturen sind: *

Es wurde eine ungültige Architektur angegeben. Überprüfen Sie, ob die Architektur gültig ist.

<a name="MT0076" />

### <a name="mt0076-no-architecture-specified-using-the---abi-argument-an-architecture-is-required-for--projects"></a>MT0076: Es wurde keine Architektur angegeben (mit dem Argument "--abi"). Für *-Projekte ist eine Architektur erforderlich.

Dies weist normalerweise auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) mit einem Testfall.

<a name="MT0077" />

### <a name="mt0077-watchos-projects-must-be-extensions"></a>MT0077: Watchos-Projekte müssen als Erweiterungen fungieren.

Dies weist normalerweise auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) mit einem Testfall.

<a name="MT0078" />

### <a name="mt0078-incremental-builds-are-enabled-with-a-deployment-target--80-currently--this-is-not-supported-the-resulting-application-will-not-launch-on-ios-9-so-the-deployment-target-will-be-set-to-80"></a>MT0078: Inkrementelle Builds werden mit einem Bereitstellungs Ziel < 8,0 (derzeit *) aktiviert. Dies wird nicht unterstützt (die resultierende Anwendung wird nicht auf IOS 9 gestartet). Daher wird das Bereitstellungs Ziel auf 8,0 festgelegt.

Diese Warnung informiert darüber, dass das Bereitstellungs Ziel für diesen Build auf 8,0 festgelegt wurde, sodass inkrementelle Builds ordnungsgemäß funktionieren.

Inkrementelle Builds werden nur unterstützt, wenn das Bereitstellungs Ziel mindestens 8,0 ist (da die resultierende Anwendung auf IOS 9 andernfalls nicht gestartet wird).

<a name="MT0079" />

### <a name="mt0079-the-recommended-xcode-version-for-xamarinios--is-xcode--or-later-the-current-xcode-version-found-in--is-"></a>MT0079: Die empfohlene Xcode-Version für xamarin. IOS * ist Xcode * oder höher. Die aktuelle Xcode-Version (in * gefunden) ist *.

Diese Warnung informiert darüber, dass die aktuelle Version von Xcode nicht die empfohlene Version von Xcode für diese Version von xamarin. IOS ist.

Aktualisieren Sie Xcode, um ein optimales Verhalten sicherzustellen.

<a name="MT0080" />

### <a name="mt0080-disabling-newrefcount---new-refcountfalse-is-deprecated"></a>MT0080: Die Deaktivierung von newrefcount,--New-refcount: false, ist veraltet.

Diese Warnung gibt an, dass die Anforderung zum Deaktivieren des neuen refcount (--New-refcount: false) ignoriert wurde.

Die neue Funktion refcount ist nun für alle Projekte obligatorisch und kann daher nicht mehr deaktiviert werden.

<a name="MT0081" />

### <a name="mt0081-the-command-line-argument---download-crash-report-also-requires---download-crash-report-to"></a>MT0081: Das Befehlszeilenargument--Download-Crash-Report erfordert auch--Download-Crash-Report-to.

<a name="MT0082" />

### <a name="mt0082-repl---enable-repl-is-only-supported-when-linking-is-not-used---nolink"></a>MT0082: REPL (--Enable-repl) wird nur unterstützt, wenn Verknüpfungen nicht verwendet werden (--nolink).

<a name="MT0083" />

### <a name="mt0083-asm-only-bitcode-is-not-supported-on-watchos-use-either---bitcodemarker-or---bitcodefull"></a>MT0083: Nur ASM-Bitcode wird für watchos nicht unterstützt. Verwenden Sie entweder "--Bitcode: Marker" oder "--Bitcode: Full".

<a name="MT0084" />

### <a name="mt0084-bitcode-is-not-supported-in-the-simulator-do-not-pass---bitcode-when-building-for-the-simulator"></a>MT0084: Bitcode wird im Simulator nicht unterstützt. Übergeben Sie--Bitcode nicht, wenn Sie für den Simulator entwickeln.

<a name="MT0085" />

### <a name="mt0085-no-reference-to--was-found-it-will-be-added-automatically"></a>MT0085: Es wurde kein Verweis auf ' * ' gefunden. Es wird automatisch hinzugefügt.

<a name="MT0086" />

### <a name="mt0086-a-target-framework---target-framework-must-be-specified-when-building-for-tvos-or-watchos"></a>MT0086: Beim Aufbau für tvos oder watchos muss ein Ziel Framework (--Target-Framework) angegeben werden.

Dies weist auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT0087" />

### <a name="mt0087-incremental-builds---fastdev-is-not-supported-with-the-boehm-gc-incremental-builds-will-be-disabled"></a>MT0087: Inkrementelle Builds (--fastdev) werden für den Boehm-GC nicht unterstützt. Inkrementelle Builds werden deaktiviert.

<a name="MT0088" />

### <a name="mt0088-the-gc-must-be-in-cooperative-mode-for-watchos-apps-please-remove-the---coopfalse-argument-to-mtouch"></a>MT0088: Der GC muss sich im kooperativen Modus für watchos-apps befinden. Entfernen Sie das Argument "--Coop: false" in mberühren.

<a name="MT0089" />

### <a name="mt0089-the-option--cannot-take-the-value--when-cooperative-mode-is-enabled-for-the-gc"></a>MT0089: Die Option\*' ' kann den Wert '\*' nicht annehmen, wenn der Kooperative Modus für die GC aktiviert ist.

<a name="MT0091" />

### <a name="mt0091-this-version-of-xamarinios-requires-the--sdk-shipped-with-xcode--either-upgrade-xcode-to-get-the-required-header-files-or-set-the-managed-linker-behaviour-to-link-framework-sdks-only-to-try-to-avoid-the-new-apis"></a>MT0091: Für diese Version von xamarin. IOS ist das * SDK (geliefert mit Xcode *) erforderlich. Aktualisieren Sie Xcode, um die erforderlichen Header Dateien zu erhalten, oder legen Sie das verwaltete Linker-Verhalten so fest, dass frameworksdker nur verknüpft werden (um die neuen APIs zu vermeiden).

Xamarin. IOS erfordert, dass die Header Dateien aus der in der Fehlermeldung angegebenen SDK-Version erstellt werden, um Ihre Anwendung zu erstellen. Die empfohlene Vorgehensweise zum Beheben dieses Fehlers besteht darin, Xcode zu aktualisieren, um das erforderliche SDK zu erhalten. Dies schließt alle erforderlichen Header Dateien ein. Wenn Sie mehrere Versionen von Xcode installiert haben oder einen Xcode an einem nicht standardmäßigen Speicherort verwenden möchten, stellen Sie sicher, dass Sie den korrekten Xcode-Speicherort in den Einstellungen Ihrer IDE festlegen.

Eine mögliche alternative Lösung besteht darin, den verwalteten Linker zu aktivieren. Dadurch wird die nicht verwendete API entfernt. Dies gilt in den meisten Fällen für die neue API, bei der die Header Dateien fehlen (oder unvollständig). Dies funktioniert jedoch nicht, wenn Ihr Projekt API verwendet, die in einem neueren SDK eingeführt wurde, als das, das Ihr Xcode bereitstellt.

Eine letzte Lösung besteht darin, eine ältere Version von xamarin. IOS zu verwenden, eine, die das für Ihr Projekt erforderliche SDK unterstützt.

<!-- MT0092 used by mlaunch -->

<a name="MT0093" />

### <a name="mt0093-could-not-find-mlaunch"></a>MT0093: "Mlaunch" konnte nicht gefunden werden.

<!-- MT0094 is not reported anymore -->

<a name="MT0095" />

### <a name="mt0095-aot-files-could-not-be-copied-to-the-destination-directory-dest-error"></a>MT0095: AOT-Dateien konnten nicht in das Zielverzeichnis "{dest}: {Error}" kopiert werden.

<a name="MT0096" />

### <a name="mt0096-no-reference-to-xamariniosdll-was-found"></a>MT0096: Es wurde kein Verweis auf xamarin. IOS. dll gefunden.

<!-- MT0097: used by mmp -->
<!-- MT0098: used by mmp -->

<a name="MT0099" />

### <a name="mt0099-internal-error--please-file-a-bug-report-with-a-test-case-httpbugzillaxamarincom"></a>MT0099: Interner Fehler *. Melden Sie einen Fehlerbericht mit einem Testfall (http://bugzilla.xamarin.com) ).

Diese Fehlermeldung wird gemeldet, wenn eine interne Konsistenzprüfung in xamarin. IOS fehlschlägt.

Dies weist normalerweise auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) mit einem Testfall.

<a name="MT0100" />

### <a name="mt0100-invalid-assembly-build-target--please-file-a-bug-report-with-a-test-case-httpbugzillaxamarincom"></a>MT0100: Ungültiges Buildziel für Assembly: "*". Melden Sie einen Fehlerbericht mit einem Testfall (http://bugzilla.xamarin.com) ).

Diese Fehlermeldung wird gemeldet, wenn eine interne Konsistenzprüfung in xamarin. IOS fehlschlägt.

Dies weist normalerweise auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) mit einem Testfall.

<a name="MT0101" />

### <a name="mt0101-the-assembly--is-specified-multiple-times-in---assembly-build-target-arguments"></a>MT0101: Die Assembly "*" ist mehrmals in--Assembly-Build-Target-Argumenten angegeben.

Die in der Fehlermeldung erwähnte Assembly wird in--Assembly-Build-Target-Argumenten mehrmals angegeben. Stellen Sie sicher, dass jede Assembly nur einmal erwähnt wird.

<a name="MT0102" />

### <a name="mt0102-the-assemblies--and--have-the-same-target-name--but-different-targets--and-"></a>MT0102: Die Assemblys\*""\*und "" haben denselben Zielnamen (\*""), aber unterschiedliche Ziele\*("" und "*").

Die in der Fehlermeldung erwähnten Assemblys weisen in Konflikt stehende Buildziele auf.

Beispiel:

```
  --assembly-build-target:Assembly1.dll=framework=MyBinary --assembly-build-target:Assembly2.dll=dynamiclibrary=MyBinary
```

In diesem Beispiel wird versucht, sowohl eine dynamische Bibliothek als auch ein Framework mit dem gleichen Make`MyBinary`() zu erstellen.

<a name="MT0103" />

### <a name="mt0103-the-static-object--contains-more-than-one-assembly--but-each-static-object-must-correspond-with-exactly-one-assembly"></a>MT0103: Das statische-Objekt\*' ' enthält mehr als eine Assembly (\*' '), aber jedes statische Objekt muss genau einer Assembly entsprechen.

Die in der Fehlermeldung erwähnten Assemblys werden alle in ein einzelnes statisches Objekt kompiliert. Dies ist nicht zulässig, jede Assembly muss in ein anderes statisches Objekt kompiliert werden.

Beispiel:

```
--assembly-build-target:Assembly1.dll=staticobject=MyBinary --assembly-build-target:Assembly2.dll=staticobject=MyBinary
```

In diesem Beispiel wird versucht, ein statisches`MyBinary`-Objekt () zu erstellen`Assembly1.dll` , `Assembly2.dll`bestehend aus zwei Assemblys (und), die nicht zulässig sind.

<a name="MT0105" />

### <a name="mt0105-no-assembly-build-target-was-specified-for-"></a>MT0105: Für ' * ' wurde kein assemblybuildziel angegeben.

Wenn das assemblybuildziel mithilfe `--assembly-build-target`von angegeben wird, muss für jede Assembly in der APP ein Buildziel zugewiesen sein.

Dieser Fehler wird gemeldet, wenn der Assembly, die in der Fehlermeldung angegeben ist, kein assemblybuildziel zugewiesen wurde.

Weitere Informationen finden Sie `--assembly-build-target` in der Dokumentation zu.

<a name="MT0106" />

### <a name="mt0106-the-assembly-build-target-name--is-invalid-the-character--is-not-allowed"></a>MT0106: Der assemblybuildzielname "\*" ist ungültig:\*das Zeichen "" ist nicht zulässig.

Der assemblybuildzielname muss ein gültiger Dateiname sein.

Beispielsweise wird dieser Fehler durch diese Werte löst:

```
--assembly-build-target:Assembly1.dll=staticobject=my/path.o
```

Da `my/path.o` kein gültiger Dateiname aufgrund des Verzeichnis Trennzeichens ist.

<a name="MT0107" />

### <a name="mt0107-the-assemblies--have-different-custom-llvm-optimizations--which-is-not-allowed-when-they-are-all-compiled-to-a-single-binary"></a>MT0107: Die Assemblys '\*' haben unterschiedliche benutzerdefinierte llvm-Optimierungen (\*). Dies ist nicht zulässig, wenn alle in eine einzelne Binärdatei kompiliert werden.

<a name="MT0108" />

### <a name="mt0108-the-assembly-build-target--did-not-match-any-assemblies"></a>MT0108: Das assemblybuildziel "*" entsprach keiner Assembly.

<a name="MT0109" />

### <a name="mt0109-the-assembly-0-was-loaded-from-a-different-path-than-the-provided-path-provided-path-1-actual-path-2"></a>MT0109: Die Assembly "{0}" wurde von einem anderen als dem bereitgestellten Pfad geladen (bereitgestellter {1}Pfad:, tatsächlicher Pfad: {2}).

Diese Warnung weist darauf hin, dass eine Assembly, auf die von der Anwendung verwiesen wird, von einem anderen Speicherort als angefordert geladen wurde.

Dies kann bedeuten, dass die APP auf mehrere Assemblys mit demselben Namen verweist, aber von unterschiedlichen Speicherorten, die zu unerwarteten Ergebnissen führen können (nur die erste Assembly wird verwendet).

<a name="MT0110" />

### <a name="mt0110-incremental-builds-have-been-disabled-because-this-version-of-xamarinios-does-not-support-incremental-builds-in-projects-that-include-third-party-binding-libraries-and-that-compiles-to-bitcode"></a>MT0110: Inkrementelle Builds wurden deaktiviert, da diese Version von xamarin. IOS keine inkrementellen Builds in Projekten unterstützt, die Bindungs Bibliotheken von Drittanbietern enthalten und in Bitcode kompiliert werden.

Inkrementelle Builds wurden deaktiviert, da diese Version von xamarin. IOS keine inkrementellen Builds in Projekten unterstützt, die Bindungs Bibliotheken von Drittanbietern enthalten und in Bitcode (tvos-und watchos-Projekte) kompiliert werden.

Es ist keine Aktion erforderlich. diese Meldung dient nur zu Informationszwecken.

Weitere Informationen finden Sie unter Bug #[51710](https://bugzilla.xamarin.com/show_bug.cgi?id=51710).

Diese Warnung wird nicht mehr gemeldet.

<a name="MT0111" />

### <a name="mt0111-bitcode-has-been-enabled-because-this-version-of-xamarinios-does-not-support-building-watchos-projects-using-llvm-without-enabling-bitcode"></a>MT0111: Bitcode wurde aktiviert, da diese Version von xamarin. IOS keine Unterstützung für das Entwickeln von watchos-Projekten mit llvm bietet, ohne Bitcode zu aktivieren.

Bitcode wurde automatisch aktiviert, da diese Version von xamarin. IOS keine Unterstützung für das Entwickeln von watchos-Projekten mit llvm bietet, ohne Bitcode zu aktivieren.

Es ist keine Aktion erforderlich. diese Meldung dient nur zu Informationszwecken.

Weitere Informationen finden Sie unter Bug #[51634](https://bugzilla.xamarin.com/show_bug.cgi?id=51634).

<a name="MT0112" />

### <a name="mt0112-native-code-sharing-has-been-disabled-because-"></a>MT0112: Die Freigabe von nativem Code wurde deaktiviert, weil *

Es gibt mehrere Gründe, warum die Code Freigabe deaktiviert werden kann:

- Da das Bereitstellungs Ziel der Container-App älter als IOS 8,0 ist (*)).

Die Freigabe von nativem Code erfordert IOS 8,0, da die Freigabe von nativem Code mithilfe von Benutzer-Frameworks implementiert wird, die mit IOS 8,0 eingeführt wurden

- Da die Container-App i18n-Assemblys (*) enthält.

Die Freigabe von nativem Code wird derzeit nicht unterstützt, wenn die Container-App i18n-Assemblys

- die Container-App verfügt über benutzerdefinierte XML-Definitionen für den verwalteten Linker (*).

Die Freigabe von nativem Code erfordert für Projekte, die benutzerdefinierte XML-Definitionen für den verwalteten Linker verwenden, nicht unterstützt.

<a name="MT0113" />

### <a name="mt0113-native-code-sharing-has-been-disabled-for-the-extension--because-"></a>MT0113: Die Freigabe von nativem Code wurde für die Erweiterung "*" deaktiviert, da *.

- Da sich die Bitcode Optionen zwischen der Container-APP\*() und der Erweiterung\*() unterscheiden.

  Die Freigabe von nativem Code erfordert, dass die Bitcode Optionen zwischen den Projekten, die Code freigeben, entsprechen.

- die Optionen "--Assembly-Build-target" unterscheiden sich zwischen der Container\*-app () und\*der Erweiterung ().

  Die Freigabe von nativem Code erfordert, dass die Optionen "--Assembly-Build-target" zwischen den Projekten identisch sind, die Code gemeinsam verwenden.

  Diese Bedingung kann auftreten, wenn inkrementelle Builds in allen Projekten weder aktiviert noch deaktiviert sind.

- die i18n-Assemblys unterscheiden sich zwischen der\*Container-app ()\*und der Erweiterung ().

  Die Freigabe von nativem Code wird derzeit nicht für Erweiterungen unterstützt, die i18n-Assemblys

- die Argumente für den AOT-Compiler unterscheiden sich zwischen der Container-\*app () und der\*Erweiterung ().

  Die Freigabe von nativem Code erfordert, dass sich die Argumente für den AOT-Compiler nicht zwischen Projekten unterscheiden, die Code gemeinsam verwenden.

- Da sich die anderen Argumente des AOT-Compilers zwischen der Container-APP\*() und der Erweiterung\*() unterscheiden.

  Die Freigabe von nativem Code erfordert, dass sich die Argumente für den AOT-Compiler nicht zwischen Projekten unterscheiden, die Code gemeinsam verwenden.

  Diese Bedingung tritt auf, wenn sich die "alle 32-Bit-Gleit Komma Operationen als 64-Bit Float" unterscheiden.

- Da llvm nicht in der Container-app (\*) und der Erweiterung (\*) aktiviert oder deaktiviert ist.

  Die Freigabe von nativem Code erfordert, dass llvm für alle Projekte, die Code gemeinsam nutzen, entweder aktiviert oder deaktiviert ist.

- Da die verwalteten Linker-Einstellungen zwischen der Container-app (\*) und der Erweiterung (\*) unterschiedlich sind.

  Die Freigabe von nativem Code erfordert, dass die verwalteten Linker-Einstellungen für alle Projekte identisch sind, die Code gemeinsam verwenden.

- die übersprungenen Assemblys für den verwalteten Linker unterscheiden sich zwischen\*der Container-app (\*) und der Erweiterung ().

  Die Freigabe von nativem Code erfordert, dass die verwalteten Linker-Einstellungen für alle Projekte identisch sind, die Code gemeinsam verwenden.

- Da die Erweiterung über benutzerdefinierte XML-Definitionen für den verwalteten Linker (*) verfügt.

  Die Freigabe von nativem Code erfordert für Projekte, die benutzerdefinierte XML-Definitionen für den verwalteten Linker verwenden, nicht unterstützt.

- Da die Container-APP nicht für die ABI * erstellt wird (während die Erweiterung für diese ABI erstellt wird).

  Die Freigabe von nativem Code erfordert, dass die Container-App für alle Architekturen erstellt wird, für die APP-Erweiterungen erstellt werden.

  Beispiel: Diese Bedingung tritt auf, wenn eine Erweiterung für ARM64 + ARMv7 erstellt wird, aber die Container-app nur für ARM64 erstellt wird.

- Da die Container-App für die ABI \*aufbaut, die nicht mit der ABI (\*) der Erweiterung kompatibel ist.

  Die Freigabe von nativem Code erfordert, dass alle Projekte für genau dieselbe API erstellt werden.

  Beispiel: Diese Bedingung tritt auf, wenn eine Erweiterung für ARMv7 + llvm + thumb2 erstellt wird, aber die Container-app nur für ARMv7 + llvm erstellt wird.

- Da die Container-App auf die Assembly '\*' von '\*' verweist, während die Erweiterung auf eine andere Version als ' * ' verweist.

  Die Freigabe von nativem Code erfordert, dass alle Projekte, die Code gemeinsam verwenden, für alle Assemblys die gleichen Versionen verwenden.

<!-- MT0114: used by mmp -->

<a name="MT0115" />

### <a name="mt0115-it-is-recommended-to-reference-dynamic-symbols-using-code---dynamic-symbol-modecode-when-bitcode-is-enabled"></a>MT0115: Es wird empfohlen, mit Code (--Dynamic-Symbol-Mode = Code) auf dynamische Symbole zu verweisen, wenn Bitcode aktiviert ist.

Xamarin. IOS-Projekte verweisen häufig dynamisch auf native Symbole, was bedeutet, dass der Native Linker solche systemeigenen Symbole während des systemeigenen Verknüpfungs Vorgangs entfernen kann, da der Native Linker nicht feststellt, dass diese Symbole verwendet werden.

Normalerweise wird der Native Linker von xamarin. IOS aufgefordert, solche Symbole beizubehalten ( `-u symbol` mit dem Linker-Flag), aber beim Kompilieren für Bitcode akzeptiert der Native Linker `-u` das Flag nicht.

Xamarin. IOS hat eine alternative Lösung implementiert: Wir generieren zusätzlichen systemeigenen Code, der auf diese Symbole verweist. Daher wird der Native Linker sehen, dass diese Symbole verwendet werden. Dies erfolgt automatisch bei der Kompilierung in Bitcode.

Wenn `--dynamic-symbol-mode=linker` an mtouchscreen übergeben wird, wird diese alternative Lösung deaktiviert, und xamarin. IOS versucht, an den nativen Linker zu übergeben `-u` . Dies führt wahrscheinlich zu nativen Linker-Fehlern.

Die Lösung besteht darin, das `--dynamic-symbol-mode=linker` -Argument aus den zusätzlichen mberührungs-Argumenten in den Buildoptionen des Projekts zu entfernen.

<!-- 0116 - 0124: free to use -->

<a name="MT0116" />

### <a name="mt0116-invalid-architecture-arch-32-bit-architectures-are-not-supported-when-deployment-target-is-11-or-later-make-sure-the-project-does-not-build-for-a-32-bit-architecture"></a>MT0116: Ungültige Architektur: {Arch}. 32-Bit-Architekturen werden nicht unterstützt, wenn das Bereitstellungs Ziel 11 oder höher ist. Stellen Sie sicher, dass das Projekt nicht für eine 32-Bit-Architektur erstellt wird.

IOS 11 bietet keine Unterstützung für 32-Bit-Anwendungen. Daher wird es nicht unterstützt, für eine 32-Bit-Anwendung zu erstellen, wenn das Bereitstellungs Ziel IOS 11 oder höher ist.

Ändern Sie die Zielarchitektur in den IOS-Buildoptionen des Projekts in arm64, oder ändern Sie das Bereitstellungs Ziel in der Datei Info. plist des Projekts in eine frühere Version von IOS.

<a name="MT0117" />

### <a name="mt0117-cant-launch-a-32-bit-app-on-a-simulator-that-only-supports-64-bit"></a>MT0117: Eine 32-Bit-App kann nicht in einem Simulator gestartet werden, der nur 64 Bit unterstützt.

<a name="MT0118" />

### <a name="mt0118-aot-files-could-not-be-found-at-the-expected-directory-msymdir"></a>MT0118: Die AOT-Dateien wurden im erwarteten Verzeichnis "{msymdir}" nicht gefunden.

<!-- 0119 - 0123: free to use -->

<a name="MT0123" />

### <a name="mt0123-the-executable-assembly--does-not-reference-"></a>MT0123: Die ausführbare Assembly * verweist nicht auf *.

Es konnte kein Verweis auf die Plattformassembly (xamarin. IOS. dll/xamarin. tvos. dll/xamarin. watchos. dll) in der ausführbaren Assembly gefunden werden.

Dies geschieht in der Regel, wenn im ausführbaren Projekt kein Code vorhanden ist, der Elemente aus der Plattformassembly verwendet. Beispielsweise würde eine leere Main-Methode (und kein anderer Code) diesen Fehler anzeigen:

```csharp
class Program {
    void Main (string[] args)
    {
    }
}
```

Durch die Verwendung einer API aus der Plattformassembly wird der folgende Fehler behoben:

```csharp
class Program {
    void Main (string[] args)
    {
        System.Console.WriteLine (typeof (UIKit.UIWindow));
    }
}
```

<a name="MT0124" />

### <a name="mt0124-could-not-set-the-current-language-to-lang-according-to-langlang-exception"></a>MT0124: Die aktuelle Sprache konnte nicht auf "{lang}" festgelegt werden (gemäß lang = {lang}): {Exception}

Dies ist eine Warnung, die angibt, dass die aktuelle Sprache in der Fehlermeldung nicht auf die Sprache festgelegt werden konnte.

Die aktuelle Sprache wird standardmäßig auf die Systemsprache eingestellt.

<a name="MT0125" />

### <a name="mt0125-the---assembly-build-target-command-line-argument-is-ignored-in-the-simulator"></a>MT0125: Das Befehlszeilenargument "--Assembly-Build-target" wird im Simulator ignoriert.

Es ist keine Aktion erforderlich. diese Meldung dient nur zu Informationszwecken.

<a name="MT0126" />

### <a name="mt0126-incremental-builds-have-been-disabled-because-incremental-builds-are-not-supported-in-the-simulator"></a>MT0126: Inkrementelle Builds wurden deaktiviert, da inkrementelle Builds im Simulator nicht unterstützt werden.

Es ist keine Aktion erforderlich. diese Meldung dient nur zu Informationszwecken.

<a name="MT0127" />

### <a name="mt0127-incremental-builds-have-been-disabled-because-this-version-of-xamarinios-does-not-support-incremental-builds-in-projects-that-include-more-than-one-third-party-binding-libraries"></a>MT0127: Inkrementelle Builds wurden deaktiviert, da diese Version von xamarin. IOS keine inkrementellen Builds in Projekten unterstützt, die mehr als eine Bindungs Bibliothek von Drittanbietern enthalten.

Inkrementelle Builds wurden automatisch deaktiviert, da diese Version von xamarin. IOS nicht immer Projekte mit mehreren Bindungs Bibliotheken von Drittanbietern ordnungsgemäß erstellt.

Es ist keine Aktion erforderlich. diese Meldung dient nur zu Informationszwecken.

Weitere Informationen finden Sie unter Bug #[52727](https://bugzilla.xamarin.com/show_bug.cgi?id=52727).

<a name="MT0128" />

### <a name="mt0128-could-not-touch-the-file--"></a>MT0128: Die Datei "*" konnte nicht berührt werden: *

Beim berühren einer Datei ist ein Fehler aufgetreten (um sicherzustellen, dass teilweise Builds ordnungsgemäß durchgeführt werden).

Diese Warnung kann wahrscheinlich ignoriert werden. bei Problemen wird ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) angezeigt, und es wird untersucht.

<a name="MT0135" />

### <a name="mt0135-did-not-link-system-framework-0-referenced-by-assembly-1-because-it-was-introduced-in-2-3-and-were-using-the-2-4-sdk"></a>MT0135: Das System{0}Framework ' ' (referenziert von Assembly '{1}') wurde nicht verknüpft, weil es {2} in {3}eingeführt wurde und das {2} {4} SDK verwendet wird.

Zum Erstellen der Anwendung muss xamarin. IOS mit Systembibliotheken verknüpft werden, von denen einige von der in der Fehlermeldung angegebenen SDK-Version abhängen. Da Sie eine ältere Version des SDK verwenden, können Aufrufe dieser APIs zur Laufzeit fehlschlagen.

Die empfohlene Vorgehensweise zum Beheben dieses Fehlers besteht darin, Xcode zu aktualisieren, um das erforderliche SDK zu erhalten. Wenn Sie mehrere Versionen von Xcode installiert haben oder einen Xcode an einem nicht standardmäßigen Speicherort verwenden möchten, stellen Sie sicher, dass Sie den korrekten Xcode-Speicherort in den Einstellungen Ihrer IDE festlegen.

Sie können auch den verwalteten [Linker](https://docs.microsoft.com/xamarin/ios/deploy-test/linker) zum Entfernen nicht verwendeter APIs aktivieren, einschließlich (in den meisten Fällen) der neuen, die die angegebene Bibliothek erfordern. Dies funktioniert jedoch nicht, wenn Ihr Projekt APIs erfordert, die in einem neueren SDK eingeführt werden, als das, das Ihr Xcode bereitstellt.

Verwenden Sie als letzte Lösung eine ältere Version von xamarin. IOS, bei der diese neuen SDOs während des Buildprozesses nicht vorhanden sein müssen.

## <a name="mt1xxx-project-related-error-messages"></a>MT1xxx: Projektbezogene Fehlermeldungen

### <a name="mt10xx-installer--mtouch"></a>MT10xx: Installer/mtouchscreen

<!--
 MT1xxx file copy / symlinks (project related)
  MT10xx installer.cs / mtouch.cs
  -->

<a name="MT1001" />

### <a name="mt1001-could-not-find-an-application-at-the-specified-directory"></a>MT1001: Die Anwendung wurde im angegebenen Verzeichnis nicht gefunden.

<a name="MT1002" />

### <a name="mt1002-could-not-create-symlinks-files-were-copied"></a>MT1002: Es konnten keine symlinks erstellt werden, Dateien wurden kopiert.

<a name="MT1003" />

### <a name="mt1003-could-not-kill-the-application--you-may-have-to-kill-the-application-manually"></a>MT1003: Die Anwendung "*" konnte nicht beendet werden. Möglicherweise müssen Sie die Anwendung manuell beenden.

<a name="MT1004" />

### <a name="mt1004-could-not-get-the-list-of-installed-applications"></a>MT1004: Die Liste der installierten Anwendungen konnte nicht gefunden werden.

## <a name="mt1xxx-project-related-error-messages"></a>MT1xxx: Projektbezogene Fehlermeldungen

<a name="MT1005" />

### <a name="mt1005-could-not-kill-the-application--on-the-device----you-may-have-to-kill-the-application-manually"></a>MT1005: Die Anwendung '\*' auf dem Gerät '\*' konnte nicht beendet werden: *-Sie müssen die Anwendung möglicherweise manuell beenden.

<a name="MT1006" />

### <a name="mt1006-could-not-install-the-application--on-the-device--"></a>MT1006: Die Anwendung "\*" konnte nicht auf dem Gerät "\*" installiert werden: *.

<a name="MT1007" />

### <a name="mt1007-failed-to-launch-the-application--on-the-device---you-can-still-launch-the-application-manually-by-tapping-on-it"></a>MT1007: Fehler beim Starten der Anwendung '\*' auf dem Gerät '\*': *. Sie können die Anwendung trotzdem manuell starten, indem Sie darauf tippen.

<a name="MT1008" />

### <a name="mt1008-failed-to-launch-the-simulator"></a>MT1008: Fehler beim Starten des Simulators.

Dieser Fehler wird gemeldet, wenn der Simulator von mfinger nicht gestartet werden konnte.   Dies kann in einigen Fällen vorkommen, weil bereits ein veralteter oder unzustellter Simulatorprozess ausgeführt wird.

Der folgende Befehl, der in der UNIX-Befehlszeile ausgegeben wird, kann verwendet werden, um bearbeitene simulatorprozesse

```bash
$ launchctl list|grep UIKitApplication|awk '{print $3}'|xargs launchctl remove
```

<a name="MT1009" />

### <a name="mt1009-could-not-copy-the-assembly--to--"></a>MT1009: Die Assembly "\*" konnte nicht nach "\*" kopiert werden: *

Dies ist ein bekanntes Problem in bestimmten Versionen von xamarin. IOS.

Wenn dies der Fall ist, versuchen Sie es mit der folgenden Problem Umgehung:

```bash
sudo chmod 0644 /Library/Frameworks/Xamarin.iOS.framework/Versions/Current/lib/mono/*/*.mdb
```

Da dieses Problem jedoch in der neuesten Version von xamarin. IOS behoben wurde, sollten Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) mit den vollständigen Versionsinformationen und der buildprotokolleausgabe erstellen.

<a name="MT1010" />

### <a name="mt1010-could-not-load-the-assembly--"></a>MT1010: Die Assembly "*" konnte nicht geladen werden: *

<a name="MT1011" />

### <a name="mt1011-could-not-add-missing-resource-file-"></a>MT1011: Fehlende Ressourcen Datei konnte nicht hinzugefügt werden: "*"

<a name="MT1012" />

### <a name="mt1012-failed-to-list-the-apps-on-the-device--"></a>MT1012: Fehler beim Auflisten der apps auf dem Gerät "*": *

<a name="MT1013" />

### <a name="mt1013-dependency-tracking-error-no-files-to-compare-please-file-a-bug-report-at-httpbugzillaxamarincom-with-a-test-case"></a>MT1013: Fehler bei der Abhängigkeits Nachverfolgung: keine zu vergleichenden Dateien. Melden Sie einen Fehlerbericht bei http://bugzilla.xamarin.com mit einem Testfall.

Dies weist auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) mit einem Testfall.

<a name="MT1014" />

### <a name="mt1014-failed-to-re-use-cached-version-of--"></a>MT1014: Die zwischengespeicherte Version von "*" konnte nicht wieder verwendet werden: *.

<a name="MT1015" />

### <a name="mt1015-failed-to-create-the-executable--"></a>MT1015: Fehler beim Erstellen der ausführbaren Datei "*": *

<a name="MT1015" />

### <a name="mt1015-failed-to-create-the-executable--"></a>MT1015: Fehler beim Erstellen der ausführbaren Datei "*": *

<a name="MT1016" />

### <a name="mt1016-failed-to-create-the-notice-file-because-a-directory-already-exists-with-the-same-name"></a>MT1016: Die Benachrichtigungs Datei konnte nicht erstellt werden, da bereits ein Verzeichnis mit demselben Namen vorhanden ist.

Entfernen Sie das `NOTICE` Verzeichnis aus dem Projekt.

<a name="MT1017" />

### <a name="mt1017-failed-to-create-the-notice-file-"></a>MT1017: Fehler beim Erstellen der Hinweis Datei: *.

<a name="MT1018" />

### <a name="mt1018-your-application-failed-code-signing-checks-and-could-not-be-installed-on-the-device--check-your-certificates-provisioning-profiles-and-bundle-ids-probably-your-device-is-not-part-of-the-selected-provisioning-profile-error-0xe8008015"></a>MT1018: Ihre Anwendung hat Code Signatur Überprüfungen nicht bestanden und konnte auf dem Gerät "*" nicht installiert werden. Überprüfen Sie Ihre Zertifikate, Bereitstellungs Profile und Bündel-IDs. Wahrscheinlich ist Ihr Gerät nicht Teil des ausgewählten Bereitstellungs Profils (Fehler: 0xe8008015).

<a name="MT1019" />

### <a name="mt1019-your-application-has-entitlements-not-supported-by-your-current-provisioning-profile-and-could-not-be-installed-on-the-device--please-check-the-ios-device-log-for-more-detailed-information-error-0xe8008016"></a>MT1019: Ihre Anwendung verfügt über Berechtigungen, die von Ihrem aktuellen Bereitstellungs Profil nicht unterstützt werden und auf dem Gerät "*" nicht installiert werden konnten. Überprüfen Sie das IOS-Geräte Protokoll auf ausführlichere Informationen (Fehler: 0xe8008016).

Dies kann bei folgenden Aktionen vorkommen:

- Ihre Anwendung verfügt über Berechtigungen, die das aktuelle Bereitstellungs Profil nicht unterstützt.
  Folgende Lösungen sind möglich:
  - Geben Sie ein anderes Bereitstellungs Profil an, das die von Ihrer Anwendung benötigten Berechtigungen unterstützt.
  - Entfernen Sie die Berechtigungen, die im aktuellen Bereitstellungs Profil nicht unterstützt werden.
- Das Gerät, das Sie bereitstellen möchten, ist nicht im Bereitstellungs Profil enthalten, das Sie verwenden.
  Folgende Lösungen sind möglich:
  - Erstellen Sie eine neue APP aus einer Vorlage in Xcode, wählen Sie das gleiche Bereitstellungs Profil aus, und stellen Sie es auf demselben Gerät bereit. Manchmal kann Xcode Bereitstellungs Profile automatisch mit neuen Geräten aktualisieren (in anderen Fällen werden Sie von Xcode gefragt, was zu tun ist).
  : Wechseln Sie zum IOS dev Center, aktualisieren Sie das Bereitstellungs Profil mit dem neuen Gerät, und laden Sie dann das aktualisierte Bereitstellungs Profil auf Ihren Computer herunter.

In den meisten Fällen werden weitere Informationen über den Fehler in das IOS-Geräte Protokoll ausgegeben, das bei der Diagnose des Problems helfen kann.

<a name="MT1020" />

### <a name="mt1020-failed-to-launch-the-application--on-the-device--"></a>MT1020: Fehler beim Starten der Anwendung '\*' auf dem Gerät '\*': *

<a name="MT1021" />

### <a name="mt1021-could-not-copy-the-file--to--2"></a>MT1021: Die Datei "\*" konnte nicht nach "\*" kopiert werden:{2}

Eine Datei konnte nicht kopiert werden. In der Fehlermeldung des Kopiervorgangs finden Sie weitere Informationen zum Fehler.

<a name="MT1022" />

### <a name="mt1022-could-not-copy-the-directory--to--2"></a>MT1022: Das Verzeichnis "\*" konnte nicht nach "\*" kopiert werden:{2}

Ein Verzeichnis konnte nicht kopiert werden. In der Fehlermeldung des Kopiervorgangs finden Sie weitere Informationen zum Fehler.

<a name="MT1023" />

### <a name="mt1023-could-not-communicate-with-the-device-to-find-the-application---"></a>MT1023: Die Kommunikation mit dem Gerät konnte nicht durchsucht werden, um die Anwendung "*" zu finden: *

Fehler beim Versuch, eine Anwendung auf dem Gerät zu suchen.

Dinge, die Sie zu lösen versuchen:

- Löschen Sie die Anwendung vom Gerät, und wiederholen Sie den Vorgang.
- Trennen Sie das Gerät, und verbinden Sie es erneut.
- Starten Sie das Gerät neu.
- Starten Sie den Mac neu.

<a name="MT1024" />

### <a name="mt1024-the-application-signature-could-not-be-verified-on-device--please-make-sure-that-the-provisioning-profile-is-installed-and-not-expired-error-0xe8008017"></a>MT1024: Die Anwendungs Signatur konnte auf Gerät ' * ' nicht überprüft werden. Stellen Sie sicher, dass das Bereitstellungs Profil installiert und nicht abgelaufen ist (Fehler: 0xe8008017).

Das Gerät hat die Installation der Anwendung abgelehnt, da die Signatur nicht überprüft werden konnte.

Stellen Sie sicher, dass das Bereitstellungs Profil installiert und nicht abgelaufen ist.

<a name="MT1025" />

### <a name="mt1025-could-not-list-the-crash-reports-on-the-device-"></a>MT1025: Die Absturzberichte auf dem Gerät konnten nicht aufgelistet werden *.

Fehler beim Auflisten der Absturzberichte auf dem Gerät.

Dinge, die Sie zu lösen versuchen:

- Löschen Sie die Anwendung vom Gerät, und wiederholen Sie den Vorgang.
- Trennen Sie das Gerät, und verbinden Sie es erneut.
- Starten Sie das Gerät neu.
- Starten Sie den Mac neu.
- Synchronisieren Sie das Gerät mit iTunes (Dadurch werden alle Absturzberichte vom Gerät entfernt).

<a name="MT1026" />

### <a name="mt1026-could-not-download-the-crash-report--from-the-device-"></a>MT1026: Der Absturz Bericht * konnte nicht vom Gerät heruntergeladen werden. *

Fehler beim Versuch, die Absturzberichte vom Gerät herunterzuladen.

Dinge, die Sie zu lösen versuchen:

- Löschen Sie die Anwendung vom Gerät, und wiederholen Sie den Vorgang.
- Trennen Sie das Gerät, und verbinden Sie es erneut.
- Starten Sie das Gerät neu.
- Starten Sie den Mac neu.
- Synchronisieren Sie das Gerät mit iTunes (Dadurch werden alle Absturzberichte vom Gerät entfernt).

<a name="MT1027" />

### <a name="mt1027-cant-use-xcode-7-to-launch-applications-on-devices-with-ios--xcode-7-only-supports-ios-6"></a>MT1027: Xcode 7 + kann nicht verwendet werden, um Anwendungen auf Geräten mit IOS * zu starten (Xcode 7 unterstützt nur IOS 6 und höher).

Es ist nicht möglich, Xcode 7 + zum Starten von Anwendungen auf Geräten mit IOS-Version unter 6,0 zu verwenden.

Verwenden Sie eine ältere Version von Xcode, oder tippen Sie manuell auf die APP, um Sie zu starten.

<a name="MT1028" />

### <a name="mt1028-invalid-device-specification--expected-ios-watchos-or-all"></a>MT1028: Ungültige Geräte Spezifikation: "*". Es wurde ' IOS ', ' watchos ' oder ' all ' erwartet.

Die mithilfe von "--Device" bestandene Geräte Spezifikation ist ungültig. Gültige Werte sind: ' IOS ', ' watchos ' oder ' all '.

<a name="MT1029" />

### <a name="mt1029-could-not-find-an-application-at-the-specified-directory-"></a>MT1029: Die Anwendung wurde im angegebenen Verzeichnis nicht gefunden: *

Der an--launchdev über gegebene Anwendungspfad ist nicht vorhanden. Geben Sie einen gültigen App Bundle an.

<a name="MT1030" />

### <a name="mt1030-launching-applications-on-device-using-a-bundle-identifier-is-deprecated-please-pass-the-full-path-to-the-bundle-to-launch"></a>MT1030: Das Starten von Anwendungen auf einem Gerät mithilfe eines Bündel Bezeichners ist veraltet. Übergeben Sie den vollständigen Pfad zum Paket, das gestartet werden soll.

Es wird empfohlen, den Pfad an die APP zu übergeben, um Sie auf dem Gerät zu starten, anstatt nur die Bündel-ID zu verwenden.

<a name="MT1031" />

### <a name="mt1031-could-not-launch-the-app--on-the-device--because-the-device-is-locked-please-unlock-the-device-and-try-again"></a>MT1031: Die app "\*" konnte nicht auf dem Gerät "\*" gestartet werden, da das Gerät gesperrt ist. Entsperren Sie das Gerät, und versuchen Sie es noch mal.

Entsperren Sie das Gerät, und versuchen Sie es noch mal.

<a name="MT1032" />

### <a name="mt1032-this-application-executable-might-be-too-large--mb-to-execute-on-device-if-bitcode-was-enabled-you-might-want-to-disable-it-for-development-it-is-only-required-to-submit-applications-to-apple"></a>MT1032: Die ausführbare Datei der Anwendung ist möglicherweise zu groß (* MB) für die Ausführung auf dem Gerät. Wenn Bitcode aktiviert wurde, können Sie ihn für die Entwicklung deaktivieren, er ist nur erforderlich, um Anwendungen an Apple zu übermitteln.

<a name="MT1033" />

### <a name="mt1033-could-not-uninstall-the-application--from-the-device--"></a>MT1033: Die Anwendung '\*' konnte nicht vom Gerät '\*' deinstalliert werden: *

<!-- 1034 used by mmp -->

<a name="MT1035" />

### <a name="mt1035-cannot-include-different-versions-of-the-framework-name"></a>MT1035: Kann keine unterschiedlichen Versionen des Frameworks "{Name}" einschließen.

Es ist nicht möglich, mit verschiedenen Versionen des gleichen Frameworks zu verknüpfen.

Dies geschieht in der Regel, wenn eine Erweiterung auf eine andere Version eines Frameworks verweist als auf die Container-app (möglicherweise über eine bindungsassembly eines Drittanbieters).

Wenn Sie diesen Fehler befolgen, werden mehrere [MT1036](#MT1036) -Fehler aufgeführt, die die Pfade für die einzelnen Frameworks auflisten.

<a name="MT1036" />

### <a name="mt1036-framework-name-included-from-path-related-to-previous-error"></a>MT1036: Das Framework "{Name}" ist in: {PATH} enthalten (im Zusammenhang mit dem vorherigen Fehler).

Dieser Fehler wird nur in Verbindung mit [MT1036](#MT1036)gemeldet. Weitere Informationen finden Sie unter [MT1036](#MT1036) .

### <a name="mt11xx-debug-service"></a>MT11xx: Debugdienst

<!--
  MT11xx DebugService.cs
  -->

<a name="MT1101" />

### <a name="mt1101-could-not-start-app"></a>MT1101: Die APP konnte nicht gestartet werden.

<a name="MT1102" />

### <a name="mt1102-could-not-attach-to-the-app-to-kill-it-"></a>MT1102: An die APP konnte nicht angefügt werden (um Sie zu beenden): *

<a name="MT1103" />

### <a name="mt1103-could-not-detach"></a>MT1103: Trennen nicht möglich.

<a name="MT1104" />

### <a name="mt1104-failed-to-send-packet-"></a>MT1104: Fehler beim Senden des Pakets: *

<a name="MT1105" />

### <a name="mt1105-unexpected-response-type"></a>MT1105: Unerwarteter Antworttyp

<a name="MT1106" />

### <a name="mt1106-could-not-get-list-of-applications-on-the-device-request-timed-out"></a>MT1106: Die Liste der Anwendungen auf dem Gerät konnte nicht gefunden werden: Timeout bei der Anforderung.

<a name="MT1107" />

### <a name="mt1107-application-failed-to-launch-"></a>MT1107: Fehler beim Starten der Anwendung: *

Überprüfen Sie, ob Ihr Gerät gesperrt ist.

Wenn Sie eine Unternehmens-App bereitstellen oder ein kostenloses Bereitstellungs Profil verwenden, können Sie dem Entwickler Vertrauen (Dies wird <a href="https://stackoverflow.com/a/30726375/183422">hier</a>erläutert).

<a name="MT1108" />

### <a name="mt1108-could-not-find-developer-tools-for-this-xx-yy-device"></a>MT1108: Die Entwicklertools für dieses XX-Gerät (yy) wurden nicht gefunden.

Für einige Vorgänge von mtouchscreen muss die `DeveloperDiskImage.dmg` Datei vorhanden sein.   Diese Datei ist Teil von Xcode und befindet sich normalerweise relativ zu dem SDK, das Sie für die Erstellung `Xcode.app/Contents/Developer/iPhoneOS.platform/DeviceSupport/VERSION/DeveloperDiskImage.dmg`von verwenden.

Dieser Fehler kann auftreten, weil Sie nicht über ein "developerdiskimage. dmg" verfügen, das mit dem Gerät übereinstimmt, mit dem Sie verbunden sind.

<a name="MT1109" />

### <a name="mt1109-application-failed-to-launch-because-the-device-is-locked-please-unlock-the-device-and-try-again"></a>MT1109: Die Anwendung konnte nicht gestartet werden, da das Gerät gesperrt ist. Entsperren Sie das Gerät, und versuchen Sie es noch mal.

Überprüfen Sie, ob Ihr Gerät gesperrt ist.

<a name="MT1110" />

### <a name="mt1110-application-failed-to-launch-because-of-ios-security-restrictions-please-ensure-the-developer-is-trusted"></a>MT1110: Die Anwendung konnte aufgrund von Sicherheitseinschränkungen in ios nicht gestartet werden. Stellen Sie sicher, dass der Entwickler vertrauenswürdig ist.

Wenn Sie eine Unternehmens-App bereitstellen oder ein kostenloses Bereitstellungs Profil verwenden, können Sie dem Entwickler Vertrauen (Dies wird <a href="https://stackoverflow.com/a/30726375/183422">hier</a>erläutert).

<a name="MT1111" />

### <a name="mt1111-application-launched-successfully-but-its-not-possible-to-wait-for-the-app-to-exit-as-requested-because-its-not-possible-to-detect-app-termination-when-launching-using-gdbserver"></a>MT1111: Die Anwendung wurde erfolgreich gestartet, aber es ist nicht möglich, nach der Anforderung zu warten, bis die APP beendet wurde, da es nicht möglich ist, die Beendigung der APP beim Starten mit gdbserver zu erkennen.

### <a name="mt12xx-simulator"></a>MT12xx: Simulator

<!--
  MT12xx simcontroller.cs
  -->

<a name="MT1201" />

### <a name="mt1201-could-not-load-the-simulator-"></a>MT1201: Der Simulator konnte nicht geladen werden: *

<a name="MT1202" />

### <a name="mt1202-invalid-simulator-configuration-"></a>MT1202: Ungültige simulatorkonfiguration: *

<a name="MT1203" />

### <a name="mt1203-invalid-simulator-specification-"></a>MT1203: Ungültige simulatorspezifikation: *

<a name="MT1204" />

### <a name="mt1204-invalid-simulator-specification--runtime-not-specified"></a>MT1204: Ungültige simulatorspezifikation "*": Laufzeit nicht angegeben.

<a name="MT1205" />

### <a name="mt1205-invalid-simulator-specification--device-type-not-specified"></a>MT1205: Ungültige simulatorspezifikation "*": der Gerättyp wurde nicht angegeben.

<a name="MT1206" />

### <a name="mt1206-could-not-find-the-simulator-runtime-"></a>MT1206: Die Simulator-Laufzeit ' * ' wurde nicht gefunden.

<a name="MT1207" />

### <a name="mt1207-could-not-find-the-simulator-device-type-"></a>MT1207: Der simulatorgerätetyp "*" wurde nicht gefunden.

<a name="MT1208" />

### <a name="mt1208-could-not-find-the-simulator-runtime-"></a>MT1208: Die Simulator-Laufzeit ' * ' wurde nicht gefunden.

<a name="MT1209" />

### <a name="mt1209-could-not-find-the-simulator-device-type-"></a>MT1209: Der simulatorgerätetyp "*" wurde nicht gefunden.

<a name="MT1210" />

### <a name="mt1210-invalid-simulator-specification--unknown-key-"></a>MT1210: Ungültige simulatorspezifikation: \*, unbekannter\*Schlüssel "".

<a name="MT1211" />

### <a name="mt1211-the-simulator-version--does-not-support-the-simulator-type-"></a>MT1211: \*Der\*Simulatortyp "" wird von der simulatorversion "" nicht unterstützt.

<a name="MT1212" />

### <a name="mt1212-failed-to-create-a-simulator-version-where-type---and-runtime--"></a>MT1212: Fehler beim Erstellen einer simulatorversion, bei der Type = * und Runtime = * ist.

<a name="MT1213" />

### <a name="mt1213-invalid-simulator-specification-for-xcode-4-"></a>MT1213: Ungültige simulatorspezifikation für Xcode 4: *

<a name="MT1214" />

### <a name="mt1214-invalid-simulator-specification-for-xcode-5-"></a>MT1214: Ungültige simulatorspezifikation für Xcode 5: *

<a name="MT1215" />

### <a name="mt1215-invalid-sdk-specified-"></a>MT1215: Ungültiges SDK angegeben: *

<a name="MT1216" />

### <a name="mt1216-could-not-find-the-simulator-udid-"></a>MT1216: Der simulatorudid "*" wurde nicht gefunden.

<a name="MT1217" />

### <a name="mt1217-could-not-load-the-app-bundle-at-"></a>MT1217: Die APP Bundle konnte nicht unter "*" geladen werden.

<a name="MT1218" />

### <a name="mt1218-no-bundle-identifier-found-in-the-app-at-"></a>MT1218: In der APP unter "*" wurde kein Bündel Bezeichner gefunden.

<a name="MT1219" />

### <a name="mt1219-could-not-find-the-simulator-for-"></a>MT1219: Der Simulator für ' * ' wurde nicht gefunden.

<a name="MT1220" />

### <a name="mt1220-could-not-find-the-latest-simulator-runtime-for-device-"></a>MT1220: Die aktuellste simulatorlaufzeit für das Gerät "*" wurde nicht gefunden.

Dies weist in der Regel auf ein Problem mit Xcode hin.

Problembehebung:

- Verwenden Sie den Simulator einmal in Xcode.
- Übergeben Sie mithilfe der--SDK \<-Version > eine explizite SDK-Version.
- Installieren Sie Xcode neu.

<a name="MT1221" />

### <a name="mt1221-could-not-find-the-paired-iphone-simulator-for-the-watchos-simulator-"></a>MT1221: Der gekoppelte iPhone-Simulator für den watchos-Simulator "*" wurde nicht gefunden.

Wenn eine watchos-app in einem watchos-Simulator gestartet wird, muss auch ein gekoppelter IOS-Simulator vorhanden sein.

Überwachungs Simulatoren können mit IOS-Simulatoren kombiniert werden, indem die Benutzeroberfläche von Xcode-Geräten (Menü Fenster > Geräte) verwendet wird.

### <a name="mt13xx-linkwith"></a>MT13xx: [linkwith]

<!--
  MT13xx [LinkWith]
  -->

<a name="MT1301" />

### <a name="mt1301-native-library---was-ignored-since-it-does-not-match-the-current-build-architectures-"></a>MT1301: Die native `*` Bibliothek\*() wurde ignoriert, da Sie nicht mit der aktuellen\*buildarchitektur (en) identisch ist.

<a name="MT1302" />

### <a name="mt1302-could-not-extract-the-native-library--from--please-ensure-the-native-library-was-properly-embedded-in-the-managed-assembly-if-the-assembly-was-built-using-a-binding-project-the-native-library-must-be-included-in-the-project-and-its-build-action-must-be-objcbindingnativelibrary"></a>MT1302: Die native Bibliothek "*" konnte nicht aus "+" extrahiert werden. Stellen Sie sicher, dass die native Bibliothek ordnungsgemäß in die verwaltete Assembly eingebettet wurde (wenn die Assembly mithilfe eines Bindungs Projekts erstellt wurde, muss die native Bibliothek im Projekt enthalten sein, und die Buildaktion muss "objcbindingnativelibrary" lauten).

<a name="MT1303" />

### <a name="mt1303-could-not-decompress-the-native-framework--from--please-review-the-build-log-for-more-information-from-the-native-zip-command"></a>MT1303: Das Native Framework '\*' konnte nicht von '\*' deaktiviert werden. Überprüfen Sie das Buildprotokoll auf Weitere Informationen aus dem nativen ZIP-Befehl.

Das angegebene Native Framework konnte nicht aus der Bindungs Bibliothek dekomprimiert werden.

Überprüfen Sie das Protokoll "bulid", um weitere Informationen zu diesem Fehler vom systemeigenen ZIP-Befehl zu erhalten.

<a name="MT1304" />

### <a name="mt1304-the-embedded-framework--in--is-invalid-it-does-not-contain-an-infoplist"></a>MT1304: Das eingebettete Framework ' * ' in * ist ungültig: Es enthält keine Info. plist-Datei.

Das angegebene eingebettete Framework enthält keine Info. plist und ist daher kein gültiges Framework.

Stellen Sie sicher, dass das Framework gültig ist.

<a name="MT1305" />

### <a name="mt1305-the-binding-library--contains-a-user-framework--but-embedded-user-frameworks-require-ios-80-the-current-deployment-target-is--please-set-the-deployment-target-in-the-infoplist-file-to-at-least-80"></a>MT1305: Die Bindungs Bibliothek '\*' enthält ein Benutzer Framework (\*), eingebettete Benutzer-Frameworks erfordern jedoch IOS 8,0 (das aktuelle Bereitstellungs Ziel ist *). Legen Sie das Bereitstellungs Ziel in der Datei "Info. plist" auf mindestens 8,0 fest.

Die angegebene Bindungs Bibliothek enthält ein eingebettetes Framework, xamarin. IOS unterstützt jedoch nur eingebettete Frameworks unter IOS 8,0 oder höher.

Legen Sie das Bereitstellungs Ziel in der Info. plist-Datei auf mindestens 8,0 fest, um diesen Fehler zu beheben (oder verwenden Sie keine eingebetteten Frameworks).

### <a name="mt14xx-crash-reports"></a>MT14xx: Absturzberichte

<!--
  MT14xx    CrashReports.cs
  -->

<a name="MT1400" />

### <a name="mt1400-could-not-open-crash-report-service-afcconnectionopen-returned-"></a>MT1400: Der Absturz Bericht Dienst konnte nicht geöffnet werden: Afcconnectionopen zurückgegeben *

Beim Versuch, auf Absturzberichte vom Gerät zuzugreifen, ist ein Fehler aufgetreten.

Dinge, die Sie zu lösen versuchen:

- Löschen Sie die Anwendung vom Gerät, und wiederholen Sie den Vorgang.
- Trennen Sie das Gerät, und verbinden Sie es erneut.
- Starten Sie das Gerät neu.
- Starten Sie den Mac neu.
- Synchronisieren Sie das Gerät mit iTunes (Dadurch werden alle Absturzberichte vom Gerät entfernt).

<a name="MT1401" />

### <a name="mt1401-could-not-close-crash-report-service-afcconnectionclose-returned-"></a>MT1401: Absturz Berichts Dienst konnte nicht geschlossen werden: Afcconnectionclose zurückgegeben *

Beim Versuch, auf Absturzberichte vom Gerät zuzugreifen, ist ein Fehler aufgetreten.

Dinge, die Sie zu lösen versuchen:

- Löschen Sie die Anwendung vom Gerät, und wiederholen Sie den Vorgang.
- Trennen Sie das Gerät, und verbinden Sie es erneut.
- Starten Sie das Gerät neu.
- Starten Sie den Mac neu.
- Synchronisieren Sie das Gerät mit iTunes (Dadurch werden alle Absturzberichte vom Gerät entfernt).

<a name="MT1402" />

### <a name="mt1402-could-not-read-file-info-for--afcfileinfoopen-returned-"></a>MT1402: Die Dateiinformationen für * konnten nicht gelesen werden: Afcfileingefoopen zurückgegeben *

Beim Versuch, auf Absturzberichte vom Gerät zuzugreifen, ist ein Fehler aufgetreten.

Dinge, die Sie zu lösen versuchen:

- Löschen Sie die Anwendung vom Gerät, und wiederholen Sie den Vorgang.
- Trennen Sie das Gerät, und verbinden Sie es erneut.
- Starten Sie das Gerät neu.
- Starten Sie den Mac neu.
- Synchronisieren Sie das Gerät mit iTunes (Dadurch werden alle Absturzberichte vom Gerät entfernt).

<a name="MT1403" />

### <a name="mt1403-could-not-read-crash-report-afcdirectoryopen--returned-"></a>MT1403: Der Absturz Bericht konnte nicht gelesen werden: Afcdirectoriyopen (*) hat Folgendes zurückgegeben: *

Beim Versuch, auf Absturzberichte vom Gerät zuzugreifen, ist ein Fehler aufgetreten.

Dinge, die Sie zu lösen versuchen:

- Löschen Sie die Anwendung vom Gerät, und wiederholen Sie den Vorgang.
- Trennen Sie das Gerät, und verbinden Sie es erneut.
- Starten Sie das Gerät neu.
- Starten Sie den Mac neu.
- Synchronisieren Sie das Gerät mit iTunes (Dadurch werden alle Absturzberichte vom Gerät entfernt).

<a name="MT1404" />

### <a name="mt1404-could-not-read-crash-report-afcfilerefopen--returned-"></a>MT1404: Der Absturz Bericht konnte nicht gelesen werden: Afcfilerefopen (*) hat Folgendes zurückgegeben: *

Beim Versuch, auf Absturzberichte vom Gerät zuzugreifen, ist ein Fehler aufgetreten.

Dinge, die Sie zu lösen versuchen:

- Löschen Sie die Anwendung vom Gerät, und wiederholen Sie den Vorgang.
- Trennen Sie das Gerät, und verbinden Sie es erneut.
- Starten Sie das Gerät neu.
- Starten Sie den Mac neu.
- Synchronisieren Sie das Gerät mit iTunes (Dadurch werden alle Absturzberichte vom Gerät entfernt).

<a name="MT1405" />

### <a name="mt1405-could-not-read-crash-report-afcfilerefread--returned-"></a>MT1405: Der Absturz Bericht konnte nicht gelesen werden: Afcfilerefread (*) hat Folgendes zurückgegeben: *

Beim Versuch, auf Absturzberichte vom Gerät zuzugreifen, ist ein Fehler aufgetreten.

Dinge, die Sie zu lösen versuchen:

- Löschen Sie die Anwendung vom Gerät, und wiederholen Sie den Vorgang.
- Trennen Sie das Gerät, und verbinden Sie es erneut.
- Starten Sie das Gerät neu.
- Starten Sie den Mac neu.
- Synchronisieren Sie das Gerät mit iTunes (Dadurch werden alle Absturzberichte vom Gerät entfernt).

<a name="MT1406" />

### <a name="mt1406-could-not-list-crash-reports-afcdirectoryopen--returned-"></a>MT1406: Absturzberichte konnten nicht aufgelistet werden: Afcdirectoriyopen (*) hat Folgendes zurückgegeben: *

Beim Versuch, auf Absturzberichte vom Gerät zuzugreifen, ist ein Fehler aufgetreten.

Dinge, die Sie zu lösen versuchen:

- Löschen Sie die Anwendung vom Gerät, und wiederholen Sie den Vorgang.
- Trennen Sie das Gerät, und verbinden Sie es erneut.
- Starten Sie das Gerät neu.
- Starten Sie den Mac neu.
- Synchronisieren Sie das Gerät mit iTunes (Dadurch werden alle Absturzberichte vom Gerät entfernt).

<!--- 1407 used by mmp -->

### <a name="mt16xx-macho"></a>MT16xx: MachO

<!--
  MT16xx    MachO.cs
  -->

<a name="MT1600" />

### <a name="mt1600-not-a-mach-o-dynamic-library-unknown-header-0x-"></a>MT1600: Keine dynamische Mach-O-Bibliothek (Unbekannter Header "0x *"): *.

Fehler beim Verarbeiten der fraglichen dynamischen Bibliothek.

Stellen Sie sicher, dass es sich bei der dynamischen Bibliothek um eine gültige dynamische Bibliothek für Mach-O handelt.

Das Format einer Bibliothek kann mit dem `file` Befehl von einem Terminal überprüft werden:

```
file -arch all -l /path/to/library.dylib
```

<a name="MT1601" />

### <a name="mt1601-not-a-static-library-unknown-header--"></a>MT1601: Keine statische Bibliothek (Unbekannter Header "*"): *.

Fehler beim Verarbeiten der fraglichen statischen Bibliothek.

Stellen Sie sicher, dass es sich bei der statischen Bibliothek um eine gültige statische Mach-O-Bibliothek handelt.

Das Format einer Bibliothek kann mit dem `file` Befehl von einem Terminal überprüft werden:

```
file -arch all -l /path/to/library.a
```

<a name="MT1602" />

### <a name="mt1602-not-a-mach-o-dynamic-library-unknown-header-0x-"></a>MT1602: Keine dynamische Mach-O-Bibliothek (Unbekannter Header "0x *"): *.

Fehler beim Verarbeiten der fraglichen dynamischen Bibliothek.

Stellen Sie sicher, dass es sich bei der dynamischen Bibliothek um eine gültige dynamische Bibliothek für Mach-O handelt.

Das Format einer Bibliothek kann mit dem `file` Befehl von einem Terminal überprüft werden:

```
file -arch all -l /path/to/library.dylib
```

<a name="MT1603" />

### <a name="mt1603-unknown-format-for-fat-entry-at-position--in-"></a>MT1603: Unbekanntes Format für FAT-Eintrag an Position * in *.

Fehler beim Verarbeiten des fraglichen FAT-Archivs.

Stellen Sie sicher, dass das FAT-Archiv gültig ist.

Das Format eines FAT-Archivs kann mit dem `file` Befehl von einem Terminal überprüft werden:

```
file -arch all -l /path/to/file
```

<a name="MT1604" />

### <a name="mt1604-file-of-type--is-not-a-macho-file-"></a>MT1604: Die Datei vom Typ * ist keine Macho-Datei (*).

Fehler beim Verarbeiten der fraglichen Macho-Datei.

Stellen Sie sicher, dass es sich bei der Datei um eine gültige Dynamic-O-Bibliothek handelt.

Das Format einer Datei kann mit dem `file` Befehl von einem Terminal überprüft werden:

```
file -arch all -l /path/to/file
```

## <a name="mt2xxx-linker-error-messages"></a>MT2xxx: Linker-Fehlermeldungen

<!--
 MT2xxx Linker
  MT20xx Linker (general) errors
  -->

<a name="MT2001" />

### <a name="mt2001-could-not-link-assemblies"></a>MT2001: Assemblys konnten nicht verknüpft werden

Dieser Fehler weist darauf hin, dass der verwaltete Linker einen unerwarteten Fehler festgestellt hat, z. b. eine Ausnahme, und die verarbeitete Assembly nicht beenden oder speichern konnte. Weitere Informationen über den genauen Fehler sind Teil des Buildprotokolls, z. b.

```
error MT2001: Could not link assemblies.
    Method: `System.Void Todo.TodoListPageCS/<<-ctor>b__1_0>d::SetStateMachine(System.Runtime.CompilerServices.IAsyncStateMachine)`
    Assembly: `QuickTodo, Version=1.0.6297.28241, Culture=neutral, PublicKeyToken=null`
Reason: Value cannot be null.
Parameter name: instruction
```

Es ist wichtig, für solche Probleme einen Fehlerbericht zu melden. In den meisten Fällen kann eine Problem Umgehung bereitgestellt werden, bis eine ordnungsgemäße Behebung veröffentlicht wird. Die obigen Informationen sind wichtig (zusammen mit einem Testfall und/oder der binairy der Assembly), um das Problem zu beheben.

<a name="MT2002" />

### <a name="mt2002-can-not-resolve-reference-"></a>MT2002: Der Verweis kann nicht aufgelöst werden: *

<a name="MT2003" />

### <a name="mt2003-option--will-be-ignored-since-linking-is-disabled"></a>MT2003: Die Option "*" wird ignoriert, da die Verknüpfung deaktiviert ist.

<a name="MT2004" />

### <a name="mt2004-extra-linker-definitions-file--could-not-be-located"></a>MT2004: Die zusätzliche Linker-Definitionsdatei "*" konnte nicht gefunden werden.

<a name="MT2005" />

### <a name="mt2005-definitions-from--could-not-be-parsed"></a>MT2005: Definitionen von ' * ' konnten nicht analysiert werden.

<a name="MT2006" />

### <a name="mt2006-can-not-load-mscorlibdll-from--please-reinstall-xamarinios"></a>MT2006: "Mscorlib. dll" kann nicht aus: * geladen werden. Installieren Sie xamarin. IOS neu.

Dies weist im Allgemeinen darauf hin, dass ein Problem mit der xamarin. IOS-Installation vorliegt. Versuchen Sie, xamarin. IOS erneut zu installieren.

<!--- 2007 used by mmp -->
<!--- 2009 used by mmp -->

<a name="MT2010" />

### <a name="mt2010-unknown-httpmessagehandler--valid-values-are-httpclienthandler-default-cfnetworkhandler-or-nsurlsessionhandler"></a>MT2010: Unbekannter httpmessagehandler `*`. Gültige Werte sind httpclienthandler (Standard), cfnetworkhandler oder nsurlsessionhandler.

<a name="MT2011" />

### <a name="mt2011-unknown-tlsprovider---valid-values-are-default-or-appletls"></a>MT2011: Unbekannter tlsprovider `*`.  Gültige Werte sind Default oder appletls.

Der angegebene `tls-provider=` Wert ist kein gültiger TLS-Anbieter (Transport Layer Security).

`default` Und`appletls` sind die einzigen gültigen Werte, und beide stellen dieselbe Option dar, die die SSL/TLS-Unterstützung unter Verwendung der nativen Apple TLS-API bereitstellt.

<!--- 2012 used by mmp -->
<!--- 2013 used by mmp -->
<!--- 2014 used by mmp -->

<a name="MT2015" />

### <a name="mt2015-invalid-httpmessagehandler--for-watchos-the-only-valid-value-is-nsurlsessionhandler"></a>MT2015: Ungültiger httpmessagehandler `*` für watchos. Der einzige gültige Wert ist "nsurlsessionhandler".

Diese Warnung tritt auf, weil die Projektdatei einen ungültigen httpmessagehandler angibt.

Frühere Versionen unserer Vorschau Tools haben standardmäßig einen ungültigen Wert in der Projektdatei generiert.

Um diese Warnung zu beheben, öffnen Sie die Projektdatei in einem Text-Editor, und entfernen Sie alle httpmessagehandler-Knoten aus dem XML-Code.

<a name="MT2016" />

### <a name="mt2016-invalid-tlsprovider-legacy-option-the-only-valid-value-appletls-will-be-used"></a>MT2016: Ungültige tlsprovider `legacy` -Option. Der einzige gültige Wert `appletls` wird verwendet.

Der `legacy` Anbieter, bei dem es sich um einen vollständig verwalteten SSLv3/TLSv1-Anbieter handelt, wird nicht mehr mit xamarin. IOS ausgeliefert. Projekte, die diesen alten Anbieter verwenden und nun mit dem neueren `appletls` Anbieter erstellt wurden.

Um diese Warnung zu beheben, öffnen Sie die Projektdatei in einem Text-Editor, und entfernen Sie alle Knoten vom Typ ' mtouchtlsprovider ' aus dem XML-Code.

<a name="MT2017" />

### <a name="mt2017-could-not-process-xml-description"></a>MT2017: Die XML-Beschreibung konnte nicht verarbeitet werden.

Dies bedeutet, dass die von Ihnen bereitgestellte [benutzerdefinierte XML-linkerkonfigurationsdatei](~/cross-platform/deploy-test/linker.md) einen Fehler enthält. Überprüfen Sie die Datei.

<a name="MT2018" />

### <a name="mt2018-the-assembly--is-referenced-from-two-different-locations--and-"></a>MT2018: Auf die Assembly\*"" wird von zwei verschiedenen Speicherorten verwiesen\*: "" und "*".

Die in der Fehlermeldung erwähnte Assembly wird von mehreren Speicherorten geladen. Stellen Sie sicher, dass immer die gleiche Version einer Assembly verwendet wird.

<a name="MT2019" />

### <a name="mt2019-can-not-load-the-root-assembly-"></a>MT2019: Die Stammassembly "*" kann nicht geladen werden.

Die Stammassembly konnte nicht geladen werden. Überprüfen Sie, ob der Pfad in der Fehlermeldung auf eine vorhandene Datei verweist und ob es sich um eine gültige .NET-Assembly handelt.

<a name="MT202x" />

### <a name="mt202x-binding-optimizer-failed-processing-"></a>MT202x: Der Bindungs Optimierer konnte `...`nicht verarbeitet werden.

Beim Versuch, generierten Bindungs Code zu optimieren, ist ein unerwartetes Problem aufgetreten. Das Element, das das Problem verursacht, wird in der Fehlermeldung benannt. Um dieses Problem zu beheben, muss die Assembly mit dem Namen (oder mit dem Typ oder der Methode mit dem Namen) in einem neuen Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) zusammen mit einem vollständigen Buildprotokoll mit aktivierter Ausführlichkeit (d. h. `-v -v -v -v` in den **zusätzlichen maddress-Argumenten**) bereitgestellt werden.

Die letzte Ziffer `x` lautet:
- `0`für einen Assemblynamen;
- `1`für einen Typnamen;
- `3`für einen Methodennamen;

<a name="MT2030" />

### <a name="mt2030-remove-user-resources-failed-processing-"></a>MT2030: Entfernen von Benutzerressourcen fehl `...`geschlagen.

Unerwarteter Fehler beim Versuch, Benutzerressourcen zu entfernen. Die Assembly, die das Problem verursacht, wird in der Fehlermeldung benannt. Um dieses Problem zu beheben, muss die Assembly in einem neuen Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) zusammen mit einem vollständigen Buildprotokoll mit aktivierter Ausführlichkeit (d `-v -v -v -v` . h. in den **zusätzlichen maddress-Argumenten**) bereitgestellt werden.

Benutzerressourcen sind Dateien, die in Assemblys (als Ressourcen) enthalten sind, die zur Buildzeit extrahiert werden müssen, um das Anwendungspaket zu erstellen. Dies umfasst Folgendes:

- `__monotouch_content_*`und `__monotouch_pages_*` -Ressourcen und
- Native Bibliotheken, die in eine bindungsassembly eingebettet sind.

<a name="MT2040" />

### <a name="mt2040-default-httpmessagehandler-setter-failed-processing-"></a>MT2040: Der standardmäßige httpmessagehandler- `...`Setter konnte nicht verarbeitet werden.

Beim Versuch, den Standard `HttpMessageHandler` Wert für die Anwendung festzulegen, ist ein unerwartetes Problem aufgetreten. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) zusammen mit einem kompletten Buildprotokoll mit aktivierter Ausführlichkeit `-v -v -v -v` (z. b. in den **zusätzlichen maddress-Argumenten**).

<a name="MT2050" />

### <a name="mt2050-code-remover-failed-processing-"></a>MT2050: Coderemover konnte nicht `...`verarbeitet werden.

Unerwarteter Fehler beim Versuch, Code aus dem BCL-Versand mit der Anwendung zu entfernen. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) zusammen mit einem kompletten Buildprotokoll mit aktivierter Ausführlichkeit `-v -v -v -v` (z. b. in den **zusätzlichen maddress-Argumenten**).

<a name="MT2060" />

### <a name="mt2060-sealer-failed-processing-"></a>MT2060: Fehler bei der Verarbeitung `...`von "".

Beim Versuch, Typen oder Methoden zu versiegeln (Final), oder beim Deserialisieren einiger Methoden ist ein unerwartetes Problem aufgetreten. Die Assembly, die das Problem verursacht, wird in der Fehlermeldung benannt. Um dieses Problem zu beheben, muss die Assembly in einem neuen Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) zusammen mit einem vollständigen Buildprotokoll mit aktivierter Ausführlichkeit (d `-v -v -v -v` . h. in den **zusätzlichen maddress-Argumenten**) bereitgestellt werden.

<a name="MT2070" />

### <a name="mt2070-metadata-reducer-failed-processing-"></a>MT2070: Metadatenreducer konnte `...`nicht verarbeitet werden.

Unerwarteter Fehler beim Versuch, die Metadaten aus der Anwendung zu reduzieren. Die Assembly, die das Problem verursacht, wird in der Fehlermeldung benannt. Um dieses Problem zu beheben, muss die Assembly in einem neuen Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) zusammen mit einem vollständigen Buildprotokoll mit aktivierter Ausführlichkeit (d `-v -v -v -v` . h. in den **zusätzlichen maddress-Argumenten**) bereitgestellt werden.

<a name="MT2080" />

### <a name="mt2080-marknsobjects-failed-processing-"></a>MT2080: Fehler bei der Verarbeitung `...`von marknsobjects.

Unerwarteter Fehler beim Versuch, unter `NSObject` Klassen aus der Anwendung zu markieren. Die Assembly, die das Problem verursacht, wird in der Fehlermeldung benannt. Um dieses Problem zu beheben, muss die Assembly in einem neuen Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) zusammen mit einem vollständigen Buildprotokoll mit aktivierter Ausführlichkeit (d `-v -v -v -v` . h. in den **zusätzlichen maddress-Argumenten**) bereitgestellt werden.

<a name="MT2090" />

### <a name="mt2090-inliner-failed-processing-"></a>MT2090: Fehler bei der Verarbeitung `...`von Inliner.

Unerwarteter Fehler beim Versuch, Code aus der Anwendung Inline zu codieren. Die Assembly, die das Problem verursacht, wird in der Fehlermeldung benannt. Um dieses Problem zu beheben, muss die Assembly in einem neuen Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) zusammen mit einem vollständigen Buildprotokoll mit aktivierter Ausführlichkeit (d. h `-v -v -v -v` . in den **zusätzlichen maddress-Argumenten**) bereitgestellt werden.

<!-- MT21xx: more linker errors -->

<!--- 2100 used by mmp -->

<a name="MT2100" />

### <a name="mt2100-smart-enum-conversion-preserver-failed-processing-"></a>MT2100: Fehler bei der Verarbeitung `...`von intelligenten Enum-Konvertierungs Vorgängen.

Beim Versuch, die Konvertierungs Methoden für intelligente enumeraten aus der Anwendung zu markieren, ist ein unerwartetes Problem aufgetreten. Die Assembly, die das Problem verursacht, wird in der Fehlermeldung benannt. Um dieses Problem zu beheben, muss die Assembly in einem neuen Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) zusammen mit einem vollständigen Buildprotokoll mit aktivierter Ausführlichkeit (d. h `-v -v -v -v` . in den **zusätzlichen maddress-Argumenten**) bereitgestellt werden.

<a name="MT2101" />

### <a name="mt2101-cant-resolve-the-reference--referenced-from-the-method--in-"></a>MT2101: Der Verweis "\*", auf den von der-\*Methode in "*" verwiesen wird, kann nicht aufgelöst werden.

Beim Verarbeiten der in der Fehlermeldung erwähnten Methode ist ein ungültiger Assemblyverweis aufgetreten.

Die Assembly, die das Problem verursacht, wird in der Fehlermeldung benannt. Um dieses Problem zu beheben, muss die Assembly in einem neuen Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) zusammen mit einem vollständigen Buildprotokoll mit aktivierter Ausführlichkeit (d `-v -v -v -v` . h. in den **zusätzlichen maddress-Argumenten**) bereitgestellt werden.

<a name="MT2102" />

### <a name="mt2102-error-processing-the-method--in-the-assembly--"></a>MT2102: Fehler beim Verarbeiten der Methode\*"" in der Assembly\*"": *

Beim Versuch, die in der Fehlermeldung erwähnte Methode zu markieren, ist ein unerwartetes Problem aufgetreten.

Die Assembly, die das Problem verursacht, wird in der Fehlermeldung benannt. Um dieses Problem zu beheben, muss die Assembly in einem neuen Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) zusammen mit einem vollständigen Buildprotokoll mit aktivierter Ausführlichkeit (d `-v -v -v -v` . h. in den **zusätzlichen maddress-Argumenten**) bereitgestellt werden.

<a name="MT2103" />

### <a name="mt2103-error-processing-assembly--"></a>MT2103: Fehler beim Verarbeiten der\*Assembly "": *

Unerwarteter Fehler beim Verarbeiten einer Assembly.

Die Assembly, die das Problem verursacht, wird in der Fehlermeldung benannt. Um dieses Problem zu beheben, muss die Assembly in einem neuen Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) zusammen mit einem vollständigen Buildprotokoll mit aktivierter Ausführlichkeit (d `-v -v -v -v` . h. in den **zusätzlichen maddress-Argumenten**) bereitgestellt werden.

<a name="MT2104" />

### <a name="mm2104-unable-to-link-assembly-0-as-it-is-mixed-mode"></a>MM2104: Die Assembly "{0}" kann nicht verknüpft werden, weil Sie im gemischten Modus ist.

Assemblys im gemischten Modus können nicht vom Linker verarbeitet werden.

Weitere https://docs.microsoft.com/cpp/dotnet/mixed-native-and-managed-assemblies Informationen zu Assemblys mit gemischtem Modus finden Sie unter.

## <a name="mt3xxx-aot-error-messages"></a>MT3xxx: AOT-Fehlermeldungen

<!--
 MT3xxx AOT
  MT30xx AOT (general) errors
  -->

<a name="MT3001" />

### <a name="mt3001-could-not-aot-the-assembly-"></a>MT3001: Die Assembly "*" konnte nicht gefunden werden.

Dies weist im Allgemeinen auf einen Fehler im AOT-Compiler hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) mit einem Projekt, das zum Reproduzieren des Fehlers verwendet werden kann.

Manchmal ist es möglich, dieses Problem zu umgehen, indem inkrementelle Builds in der IOS-Buildoption des Projekts deaktiviert werden (es ist aber immer noch ein Fehler, bitte melden Sie Sie trotzdem).

<a name="MT3002" />

### <a name="mt3002-aot-restriction-method--must-be-static-since-it-is-decorated-with-monopinvokecallback-see-developerxamarincomguidesiosadvanced_topicslimitationsreverse_callbacksiosinternalslimitationsmdreverse-callbacks"></a>MT3002: AOT-Einschränkung: Die ' * '-Methode muss statisch sein, da Sie mit [' monopinvokecallback] ' versehen ist. Siehe [Developer.xamarin.com/Guides/IOS/advanced_topics/Limitations/#Reverse_Callbacks](~/ios/internals/limitations.md#reverse-callbacks)

Diese Fehlermeldung stammt vom AOT-Compiler.

<a name="MT3003" />

### <a name="mt3003-conflicting---debug-and---llvm-options-soft-debugging-is-disabled"></a>MT3003: In Konflikt stehende--Debug-und--llvm-Optionen. Das weiche Debuggen ist deaktiviert.

Das Debuggen wird nicht unterstützt, wenn llvm aktiviert ist. Wenn Sie die APP Debuggen müssen, deaktivieren Sie zuerst llvm.

<a name="MT3004" />

### <a name="mt3004-could-not-aot-the-assembly--because-it-doesnt-exist"></a>MT3004: Die Assembly "*" konnte nicht gefunden werden, da Sie nicht vorhanden ist.

<a name="MT3005" />

### <a name="mt3005-the-dependency--of-the-assembly--was-not-found-please-review-the-projects-references"></a>MT3005: Die Abhängigkeit "\*" der Assembly "\*" wurde nicht gefunden. Überprüfen Sie die Verweise des Projekts.

Dies tritt normalerweise auf, wenn eine Assembly auf eine andere Version einer Plattform-Assembly verweist (in der Regel die .NET 4-Version von mscorlib. dll).

Dies wird nicht unterstützt und kann nicht ordnungsgemäß erstellt oder ausgeführt werden (die Assembly verwendet möglicherweise die API aus der .NET 4-Version von "mscorlib. dll", die in der xamarin. IOS-Version nicht vorhanden ist).

<a name="MT3006" />

### <a name="mt3006-could-not-compute-a-complete-dependency-map-for-the-project-this-will-result-in-slower-build-times-because-xamarinios-cant-properly-detect-what-needs-to-be-rebuilt-and-what-does-not-need-to-be-rebuilt-please-review-previous-warnings-for-more-details"></a>MT3006: Eine komplette Abhängigkeits Zuordnung für das Projekt konnte nicht berechnet werden. Dies führt zu langsameren Buildzeiten, da xamarin. IOS nicht korrekt erkennen kann, was neu erstellt werden muss (und was nicht neu erstellt werden muss). Weitere Informationen finden Sie in den vorherigen Warnungen.

 Erstellen oder ausführen ordnungsgemäß (die Assembly kann eine API aus der .NET 4-Version von "mscorlib. dll" verwenden, die in der xamarin. IOS-Version nicht vorhanden ist).

<a name="MT3007" />

### <a name="mt3007-debug-info-files-mdb-will-not-be-loaded-when-llvm-is-enabled"></a>MT3007: Debuginformationsdateien (*. mdb) werden nicht geladen, wenn llvm aktiviert ist.

<a name="MT3008" />

### <a name="mt3008-bitcode-support-requires-the-use-of-the-llvm-aot-backend---llvm"></a>MT3008: Unterstützung für Bitcode erfordert die Verwendung des llvm-AOT-Back-Ends (--llvm).

Die Unterstützung von bitcodes erfordert die Verwendung des llvm AOT-Back-Ends (--llvm).

Deaktivieren Sie die Bitcode-Unterstützung, oder aktivieren Sie llvm.

<!--- 3009 used by mmp -->
<!--- 3010 used by mmp -->

## <a name="mt4xxx-code-generation-error-messages"></a>MT4xxx: Fehlermeldungen zur Code Generierung

### <a name="mt40xx-main"></a>MT40xx: Main

<!--
 MT4xxx code generation
  MT40xx main.m
  -->

<a name="MT4001" />

### <a name="mt4001-the-main-template-could-not-be-expanded-to-"></a>MT4001: Die Hauptvorlage konnte nicht auf `*`erweitert werden.

Beim Erstellen `main.m`ist ein Fehler aufgetreten. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT4002" />

### <a name="mt4002-failed-to-compile-the-generated-code-for-pinvoke-methods-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4002: Fehler beim Kompilieren des generierten Codes für P/Aufruf-Methoden. Melden Sie einen Fehlerbericht unter http://bugzilla.xamarin.com

Fehler beim Kompilieren des generierten Codes für P/Aufruf-Methoden. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

### <a name="mt41xx-registrar"></a>MT41xx: Registrierung

<!--
  MT41xx registrar.m
  -->

<a name="MT4101" />

### <a name="mt4101-the-registrar-cannot-build-a-signature-for-type-"></a>MT4101: Die Registrierungsstelle kann keine Signatur für den `*`Typ erstellen.

In der exportierten API wurde ein Typ gefunden, der der Common Language Runtime nicht weiß, wie es zu/aus Ziel-C Mars Hallen kann.

Wenn Sie der Meinung sind, dass xamarin. IOS den fraglichen Typ unterstützen soll, müssen Sie eine Erweiterungs Anforderung auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new)einreichen.

<a name="MT4102" />

### <a name="mt4102-the-registrar-found-an-invalid-type--in-signature-for-method--use--instead"></a>MT4102: Die Registrierungsstelle hat in der `*` Signatur für die Methode `*`einen ungültigen Typ gefunden. Verwenden Sie stattdessen `*`.

Dies geschieht zurzeit nur mit einem der folgenden Typen: System. DateTime. Verwenden Sie stattdessen die entsprechende Ziel-C-Entsprechung (nsdate).

<a name="MT4103" />

### <a name="mt4103-the-registrar-found-an-invalid-type--in-signature-for-method--the-type-implements-inativeobject-but-does-not-have-a-constructor-that-takes-two-intptr-bool-arguments"></a>MT4103: Die Registrierungsstelle hat einen ungültigen Typ `*` in der Signatur für die Methode `*`gefunden: Der Typ implementiert "inativeobject", aber keinen Konstruktor, der zwei (IntPtr, bool) Argumente annimmt.

Dies tritt auf, wenn die Registrierungsstelle einen Typ in einer Signatur mit den erwähnten Merkmalen trifft. Die Registrierungsstelle muss möglicherweise neue Instanzen des Typs erstellen, und in diesem Fall ist ein Konstruktor mit der (IntPtr, bool)-Signatur erforderlich-das erste Argument (IntPtr) gibt das verwaltete Handle an, während der zweite, wenn der Aufrufer den Besitz des nativen übergibt. Handle (wenn dieser Wert "false" lautet, wird für das Objekt aufgerufen).

<a name="MT4104" />

### <a name="mt4104-the-registrar-cannot-marshal-the-return-value-for-type--in-signature-for-method-"></a>MT4104: Die Registrierungsstelle kann den Rückgabewert für den `*` Typ in der Signatur `*`für die Methode nicht Mars Hallen.

In der exportierten API wurde ein Typ gefunden, der der Common Language Runtime nicht weiß, wie es zu/aus Ziel-C Mars Hallen kann.

Wenn Sie der Meinung sind, dass xamarin. IOS den fraglichen Typ unterstützen soll, müssen Sie eine Erweiterungs Anforderung auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new)einreichen.

<a name="MT4105" />

### <a name="mt4105-the-registrar-cannot-marshal-the-parameter-of-type--in-signature-for-method-"></a>MT4105: Die Registrierungsstelle kann den Parameter des Typs `*` in der Signatur für `*`die Methode nicht Mars Hallen.

Wenn Sie der Meinung sind, dass xamarin. IOS den fraglichen Typ unterstützen soll, müssen Sie eine Erweiterungs Anforderung auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new)einreichen.

<a name="MT4106" />

### <a name="mt4106-the-registrar-cannot-marshal-the-return-value-for-structure--in-signature-for-method-"></a>MT4106: Die Registrierungsstelle kann den Rückgabewert der Struktur `*` in der Signatur für `*`die-Methode nicht Mars Hallen.

In der exportierten API wurde ein Typ gefunden, der der Common Language Runtime nicht weiß, wie es zu/aus Ziel-C Mars Hallen kann.

Wenn Sie der Meinung sind, dass xamarin. IOS den fraglichen Typ unterstützen soll, müssen Sie eine Erweiterungs Anforderung auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new)einreichen.

<a name="MT4107" />

### <a name="mt4107-the-registrar-cannot-marshal-the-parameter-of-type--in-signature-for-method-"></a>MT4107: Die Registrierungsstelle kann den Parameter des Typs `*` in der Signatur für `+`die Methode nicht Mars Hallen.

In der exportierten API wurde ein Typ gefunden, der der Common Language Runtime nicht weiß, wie es zu/aus Ziel-C Mars Hallen kann.

Wenn Sie der Meinung sind, dass xamarin. IOS den fraglichen Typ unterstützen soll, müssen Sie eine Erweiterungs Anforderung auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new)einreichen.

<a name="MT4108" />

### <a name="mt4108-the-registrar-cannot-get-the-objectivec-type-for-managed-type-"></a>MT4108: Die Registrierungsstelle kann den objectivec-Typ für den `*`verwalteten Typ nicht abrufen.

In der exportierten API wurde ein Typ gefunden, der der Common Language Runtime nicht weiß, wie es zu/aus Ziel-C Mars Hallen kann.

Wenn Sie der Meinung sind, dass xamarin. IOS den fraglichen Typ unterstützen soll, müssen Sie eine Erweiterungs Anforderung auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new)einreichen.

<a name="MT4109" />

### <a name="mt4109-failed-to-compile-the-generated-registrar-code-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4109: Der generierte Registrierungscode konnte nicht kompiliert werden. Melden Sie einen Fehlerbericht unter http://bugzilla.xamarin.com

Der generierte Code für die Registrierungsstelle konnte nicht kompiliert werden. Das Buildprotokoll enthält die Ausgabe des systemeigenen Compilers und erläutert, warum der Code nicht kompiliert wird.

Dies ist immer ein Fehler in xamarin. IOS. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) mit Ihrem Projekt oder einem Testfall.

<a name="MT4110" />

### <a name="mt4110-the-registrar-cannot-marshal-the-out-parameter-of-type--in-signature-for-method-"></a>MT4110: Die Registrierungsstelle kann den out-Parameter des `*` Typs in der Signatur `*`für die Methode nicht Mars Hallen.

<a name="MT4111" />

### <a name="mt4111-the-registrar-cannot-build-a-signature-for-type--in-method-"></a>MT4111: Die Registrierungsstelle kann keine Signatur für den `*` Typ in `*`der-Methode erstellen.

<a name="MT4112" />

### <a name="mt4112-the-registrar-found-an-invalid-type--registering-generic-types-with-objective-c-is-not-supported-and-may-lead-to-random-behavior-andor-crashes-for-backwards-compatibility-with-older-versions-of-xamarinios-it-is-possible-to-ignore-this-error-by-passing---unsupported--enable-generics-in-registrar-as-an-additional-mtouch-argument-in-the-projects-ios-build-options-page-see-developerxamarincomguidesiosadvanced_topicsregistrariosinternalsregistrarmd-for-more-information"></a>MT4112: Die Registrierungsstelle hat einen ungültigen Typ `*`gefunden. Das Registrieren von generischen Typen mit dem Ziel-C wird nicht unterstützt und kann zu zufälligem Verhalten und/oder Abstürzen führen (aus Gründen der Abwärtskompatibilität mit älteren Versionen von xamarin. IOS ist es `--unsupported--enable-generics-in-registrar` möglich, diesen Fehler zu ignorieren, indem er als zusätzliches mtouchscreen übergeben wird). -Argument auf der Seite IOS-Buildoptionen des Projekts. Weitere Informationen finden Sie unter [Developer.xamarin.com/Guides/IOS/advanced_topics/Registrar](~/ios/internals/registrar.md) ).

<a name="MT4113" />

### <a name="mt4113-the-registrar-found-a-generic-method--exporting-generic-methods-is-not-supported-and-will-lead-to-random-behavior-andor-crashes"></a>MT4113: Die Registrierungsstelle hat eine generische Methode gefunden\*:\*".". Das Exportieren von generischen Methoden wird nicht unterstützt und führt zu zufälligem Verhalten und/oder abstürzen.

<a name="MT4114" />

### <a name="mt4114-unexpected-error-in-the-registrar-for-the-method----please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4114: Unerwarteter Fehler in der Registrierungsstelle für die\*Methode\*".". Bitte melden Sie einen Fehlerbericht unter http://bugzilla.xamarin.com

<a name="MT4116" />

### <a name="mt4116-could-not-register-the-assembly--"></a>MT4116: Die Assembly "*" konnte nicht registriert werden: *

<a name="MT4117" />

### <a name="mt4117-the-registrar-found-a-signature-mismatch-in-the-method----the-selector-indicates-the-method-takes--parameters-while-the-managed-method-has--parameters"></a>MT4117: Die Registrierungsstelle hat einen Signatur Konflikt in der Methode " *.* " gefunden. der Selektor gibt an, dass die Methode * Parameter annimmt, während die verwaltete Methode über * Parameter verfügt.

<a name="MT4118" />

### <a name="mt4118-cannot-register-two-managed-types--and--with-the-same-native-name-"></a>MT4118: Es können nicht zwei verwaltete Typen (\*' ' und\*' ') mit demselben systemeigenen Namen (' * ') registriert werden.

<a name="MT4119" />

### <a name="mt4119-could-not-register-the-selector--of-the-member--because-the-selector-is-already-registered-on-a-different-member"></a>MT4119: Der Selektor "\*" des Members "\*. *" konnte nicht registriert werden, da der Selektor bereits auf einem anderen Member registriert ist.

<a name="MT4120" />

### <a name="mt4120-the-registrar-found-an-unknown-field-type--in-field--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4120: Die Registrierungsstelle hat einen unbekannten Feldtyp\*"" im Feld\*". *" gefunden. Melden Sie einen Fehlerbericht unter http://bugzilla.xamarin.com

Dieser Fehler weist auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT4121" />

### <a name="mt4121-cannot-use-gccg-to-compile-the-generated-code-from-the-static-registrar-when-using-the-accounts-framework-the-header-files-provided-by-apple-used-during-the-compilation-require-clang-either-use-clang---compilerclang-or-the-dynamic-registrar---registrardynamic"></a>MT4121: GCC/G + + kann nicht verwendet werden, um den generierten Code bei Verwendung des Accounts-Frameworks aus der statischen Registrierungsstelle zu kompilieren (die von Apple bereitgestellten Header Dateien, die während der Kompilierung verwendet werden, erfordern clang) Verwenden Sie entweder clang (--Compiler: clang) oder die dynamische Registrierungsstelle (--Registrierungsstelle: Dynamic).

<a name="MT4122" />

### <a name="mt4122-cannot-use-the-clang-compiler-provided-in-the--sdk-to-compile-the-generated-code-from-the-static-registrar-when-non-ascii-type-names--are-present-in-the-application-either-use-gccg---compilergccg-the-dynamic-registrar---registrardynamic-or-a-newer-sdk"></a>MT4122: Der in bereitgestellte clang-Compiler kann nicht verwendet werden *.* SDK zum Kompilieren des generierten Codes aus der statischen Registrierungsstelle, wenn nicht-ASCII-Typnamen ("*") in der Anwendung vorhanden sind. Verwenden Sie entweder gcc/g + + (--Compiler: gcc | G + +), die dynamische Registrierungsstelle (--Registrierungsstelle: Dynamic) oder ein neueres SDK.

<a name="MT4123" />

### <a name="mt4123-the-type-of-the-variadic-parameter-in-the-variadic-function--must-be-systemintptr"></a>MT4123: Der Variadic-Parameter in der Variadic-Funktion "*" muss "System. IntPtr" lauten.

<a name="MT4124" />

### <a name="mt4124-invalid--found-on--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4124: Ungültig * in "*" gefunden. Melden Sie einen Fehlerbericht unter http://bugzilla.xamarin.com

Dieser Fehler weist auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT4125" />

### <a name="mt4125-the-registrar-found-an-invalid-type--in-signature-for-method--the-interface-must-have-a-protocol-attribute-specifying-its-wrapper-type"></a>MT4125: Die Registrierungsstelle hat einen ungültigen\*Typ "" in der Signatur\*für die Methode "" gefunden: Die Schnittstelle muss über ein Protokoll Attribut verfügen, das Ihren Wrappertyp angibt.

<a name="MT4126" />

### <a name="mt4126-cannot-register-two-managed-protocols--and--with-the-same-native-name-"></a>MT4126: Es können nicht zwei verwaltete Protokolle (\*"" und\*"") mit demselben systemeigenen Namen ("*") registriert werden.

<a name="MT4127" />

### <a name="mt4127-cannot-register-more-than-one-interface-method-for-the-method--which-is-implementing-"></a>MT4127: Es kann nicht mehr als eine Schnittstellen Methode für die Methode\*"" registriert werden (die\*"" implementiert).

<a name="MT4128" />

### <a name="mt4128-the-registrar-found-an-invalid-generic-parameter-type--in-the-method--the-generic-parameter-must-have-an-nsobject-constraint"></a>MT4128: Die Registrierungsstelle hat einen ungültigen generischen\*Parametertyp ""\*in der-Methode gefunden. Der generische Parameter muss eine NSObject-Einschränkung aufweisen.

<a name="MT4129" />

### <a name="mt4129-the-registrar-found-an-invalid-generic-return-type--in-the-method--the-generic-return-type-must-have-an-nsobject-constraint"></a>MT4129: Die Registrierungsstelle hat einen ungültigen generischen\*Rückgabetyp "\*" in der-Methode gefunden. Der generische Rückgabetyp muss eine NSObject-Einschränkung aufweisen.

<a name="MT4130" />

### <a name="mt4130-the-registrar-cannot-export-static-methods-in-generic-classes-"></a>MT4130: Die Registrierungsstelle kann keine statischen Methoden in generischen Klassen ("*") exportieren.

<a name="MT4131" />

### <a name="mt4131-the-registrar-cannot-export-static-properties-in-generic-classes-"></a>MT4131: Die Registrierungsstelle kann keine statischen Eigenschaften in generischen Klassen\*(\*".") exportieren.

<a name="MT4132" />

### <a name="mt4132-the-registrar-found-an-invalid-generic-return-type--in-the-property--the-return-type-must-have-an-nsobject-constraint"></a>MT4132: Die Registrierungsstelle hat einen ungültigen generischen\*Rückgabetyp '\*' in der-Eigenschaft gefunden. Der Rückgabetyp muss eine NSObject-Einschränkung aufweisen.

<a name="MT4133" />

### <a name="mt4133-cannot-construct-an-instance-of-the-type--from-objective-c-because-the-type-is-generic-runtime-exception"></a>MT4133: Eine Instanz vom Typ ' * ' kann nicht aus dem Ziel-C erstellt werden, da der Typ generisch ist. [Lauf Zeit Ausnahme]

<a name="MT4134" />

### <a name="mt4134-your-application-is-using-the--framework-which-isnt-included-in-the-ios-sdk-youre-using-to-build-your-app-this-framework-was-introduced-in-ios--while-youre-building-with-the-ios--sdk-please-select-a-newer-sdk-in-your-apps-ios-build-options"></a>MT4134: Ihre Anwendung verwendet das Framework "*", das nicht im IOS SDK enthalten ist, das Sie zum Erstellen Ihrer APP verwenden (dieses Framework wurde in ios * eingeführt, während Sie mit dem IOS * SDK arbeiten). Wählen Sie ein neueres SDK in den IOS-Buildoptionen Ihrer APP aus.

<a name="MT4135" />

### <a name="mt4135-the-member--has-an-export-attribute-that-doesnt-specify-a-selector-a-selector-is-required"></a>MT4135: Der Member "\*.\*" verfügt über ein Export Attribut, das keinen Selektor angibt. Eine Auswahl ist erforderlich.

<a name="MT4136" />

### <a name="mt4136-the-registrar-cannot-marshal-the-parameter-type--of-the-parameter--in-the-method-"></a>MT4136: Die Registrierungsstelle kann den Parametertyp\*"" des Parameters\*"" in der Methode\*". *" nicht Mars Hallen.

<!-- MT4137 is unused -->

<a name="MT4138" />

### <a name="mt4138-the-registrar-cannot-marshal-the-property-type--of-the-property-"></a>MT4138: Die Registrierungsstelle kann den Eigenschaftentyp "\*" der Eigenschaft "\*. *" nicht Mars Hallen.

<a name="MT4139" />

### <a name="mt4139-the-registrar-cannot-marshal-the-property-type--of-the-property--properties-with-the-connect-attribute-must-have-a-property-type-of-nsobject-or-a-subclass-of-nsobject"></a>MT4139: Die Registrierungsstelle kann den Eigenschaftentyp "\*" der Eigenschaft "\*. *" nicht Mars Hallen. Eigenschaften mit dem Attribut [Connect] müssen den Eigenschaftentyp NSObject aufweisen (oder eine Unterklasse von NSObject).

<a name="MT4140" />

### <a name="mt4140-the-registrar-found-a-signature-mismatch-in-the-method----the-selector-indicates-the-variadic-method-takes--parameters-while-the-managed-method-has--parameters"></a>MT4140: Die Registrierungsstelle hat einen Signatur Konflikt in der Methode " *.* " gefunden. der Selektor gibt an, dass die Variadic-Methode * Parameter annimmt, während die verwaltete Methode über * Parameter verfügt.

<a name="MT4141" />

### <a name="mt4141-cannot-register-the-selector--on-the-member--because-xamarinios-implicitly-registers-this-selector"></a>MT4141: Der Selektor '\*' kann nicht für den Member\*' ' registriert werden, da xamarin. IOS diese Auswahl implizit registriert.

Dies tritt auf, wenn Sie einen Frameworktyp Unterklassen und versuchen, eine "beibehalten"-, "Release"-oder "dezuzuordc"-Methode zu implementieren:

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

Es ist jedoch möglich, diese Methoden zu überschreiben, wenn die Klasse nicht die erste Unterklasse des frameworktyps ist:

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

In diesem Fall wird xamarin `retain`. IOS `dealloc` überschreiben `release` , und für `MyNSObject` die-Klasse, und es gibt keinen Konflikt.

<a name="MT4142" />

### <a name="mt4142-failed-to-register-the-type-"></a>MT4142: Fehler beim Registrieren des Typs "*".

<a name="MT4143" />

### <a name="mt4143-the-objectivec-class--could-not-be-registered-it-does-not-seem-to-derive-from-any-known-objectivec-class-including-nsobject"></a>MT4143: Die objectivec-Klasse ' * ' konnte nicht registriert werden, Sie scheint nicht von einer bekannten objectivec-Klasse (einschließlich NSObject) abgeleitet zu werden.

<a name="MT4144" />

### <a name="mt4144-cannot-register-the-method--since-it-does-not-have-an-associated-trampoline-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4144: Die Methode "*" kann nicht registriert werden, da ihr kein Trampolin zugeordnet ist. Melden Sie einen Fehlerbericht unter http://bugzilla.xamarin.com.

Dies weist auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT4145" />

### <a name="mt4145-invalid-enum--enums-with-the-native-attribute-must-have-a-underlying-enum-type-of-either-long-or-ulong"></a>MT4145: Ungültige Aufzählung ' * ': für Aufzählungen mit dem [Native]-Attribut muss ein zugrunde liegender Aufzählungstyp entweder ' Long ' oder ' ULONG ' vorhanden sein.

<a name="MT4146" />

### <a name="mt4146-the-name-parameter-of-the-registrar-attribute-on-the-class---contains-an-invalid-character--"></a>MT4146: Der Name-Parameter des Registrierungs-Attributs in der\*-Klasse (\*' ') enthält ein ungültiges Zeichen\*: '\*' ().

Der Name einer objectice-C-Klasse darf keine Leerzeichen enthalten, was bedeutet, dass `Register` das Attribut in der entsprechenden verwalteten Klasse `Name` keinen-Parameter enthalten darf, der keine Leerzeichen enthält.

Stellen Sie sicher, `Register` dass das Attribut in der verwalteten Klasse, das in der Fehlermeldung angegeben ist, keine Leerzeichen enthält.

<a name="MT4147" />

### <a name="mt4147-detected-a-protocol-inheriting-from-the-jsexport-protocol-while-using-the-dynamic-registrar-it-is-not-possible-to-export-protocols-to-javascriptcore-dynamically-the-static-registrar-must-be-used-add---registrarstatic-to-the-additional-mtouch-arguments-in-the-projects-ios-build-options-to-select-the-static-registrar"></a>MT4147: Es wurde ein Protokoll erkannt, das beim Verwenden der dynamischen Registrierungsstelle vom jsexport-Protokoll erbt. Es ist nicht möglich, Protokolle dynamisch in javascriptcore zu exportieren. die statische Registrierungsstelle muss verwendet werden (Hinzufügen von "--Registrar: static" zu den zusätzlichen mberührungs-Argumenten in den IOS-Buildoptionen des Projekts, um die statische Registrierungsstelle auszuwählen).

<a name="MT4148" />

### <a name="mt4148-the-registrar-found-a-generic-protocol--exporting-generic-protocols-is-not-supported"></a>MT4148: Die Registrierungsstelle hat ein generisches Protokoll gefunden: "*". Das Exportieren von generischen Protokollen wird nicht unterstützt.

<a name="MT4149" />

### <a name="mt4149-cannot-register-the-method--because-the-type-of-the-first-parameter--does-not-match-the-category-type-"></a>MT4149: Die\*-Methode\*kann nicht registriert werden, da der Typ des ersten Parameters (\*' ') nicht mit dem Kategorietyp\*(' ') identisch ist.

<a name="MT4150" />

### <a name="mt4150-cannot-register-the-type--because-the-type-property--in-its-category-attribute-does-not-inherit-from-nsobject"></a>MT4150: Der Typ "\*" kann nicht registriert werden, da die Type\*-Eigenschaft ("") in Ihrem Category-Attribut nicht von "NSObject" erbt.

<a name="MT4151" />

### <a name="mt4151-cannot-register-the-type--because-the-type-property-in-its-category-attribute-isnt-set"></a>MT4151: Der Typ "*" kann nicht registriert werden, da die Type-Eigenschaft in Ihrem Category-Attribut nicht festgelegt ist.

<a name="MT4152" />

### <a name="mt4152-cannot-register-the-type--as-a-category-because-it-implements-inativeobject-or-subclasses-nsobject"></a>MT4152: Der Typ "*" kann nicht als Kategorie registriert werden, da er inativeobject oder Unterklassen NSObject implementiert.

<a name="MT4153" />

### <a name="mt4153-cannot-register-the-type--as-a-category-because-its-generic"></a>MT4153: Der Typ "\*" kann nicht als Kategorie registriert werden, da er generisch ist.

<a name="MT4154" />

### <a name="mt4154-cannot-register-the-method--as-a-category-method-because-its-generic"></a>MT4154: Die-Methode\*kann nicht als kategoriemethode registriert werden, da Sie generisch ist.

<a name="MT4155" />

### <a name="mt4155-cannot-register-the-method--with-the-selector--as-a-category-method-on--because-the-objective-c-already-has-an-implementation-for-this-selector"></a>MT4155: Die Methode '\*' kann nicht mit der Auswahl '\*' als kategoriemethode für ' * ' registriert werden, da das Ziel-C bereits über eine Implementierung für diese Auswahl verfügt.

<a name="MT4156" />

### <a name="mt4156-cannot-register-two-categories--and--with-the-same-native-name-"></a>MT4156: Es können nicht zwei Kategorien (\*' ' und\*' ') mit demselben systemeigenen Namen (' * ') registriert werden.

<a name="MT4157" />

### <a name="mt4157-cannot-register-the-category-method--because-at-least-one-parameter-is-required-and-its-type-must-match-the-category-type-"></a>MT4157: Die kategoriemethode "\*" kann nicht registriert werden, weil mindestens ein Parameter erforderlich ist (und der Typ muss mit\*dem Kategorietyp "" identisch sein).

<a name="MT4158" />

### <a name="mt4158-cannot-register-the-constructor--in-the-category--because-constructors-in-categories-are-not-supported"></a>MT4158: Der Konstruktor * kann nicht in der Kategorie * registriert werden, da Konstruktoren in Kategorien nicht unterstützt werden.

<a name="MT4159" />

### <a name="mt4159-cannot-register-the-method--as-a-category-method-because-category-methods-must-be-static"></a>MT4159: Die ' * '-Methode kann nicht als kategoriemethode registriert werden, da kategoriemethoden statisch sein müssen.

<a name="MT4160" />

### <a name="mt4160-invalid-character---found-in-selector--for-"></a>MT4160: Das ungültige\*Zeichen '\*' () wurde in\*der Auswahl '\*' für ' ' gefunden.

<a name="MT4161" />

### <a name="mt4161-the-registrar-found-an-unsupported-structure--all-fields-in-a-structure-must-also-be-structures-field--with-type-2-is-not-a-structure"></a>MT4161: Die Registrierungsstelle hat eine nicht unterstützte\*Struktur "" gefunden: Alle Felder in einer Struktur müssen auch Strukturen sein (Feld "\*" mit dem Typ{2}"" ist keine Struktur).

Die Registrierungsstelle hat eine Struktur mit nicht unterstützten Feldern gefunden.

Alle Felder in einer Struktur, die für "Ziel-C" verfügbar gemacht wird, müssen auch Strukturen sein (keine Klassen).

<a name="MT4162" />

### <a name="mt4162-the-type--used-as--2-is-not-available-in---it-was-introduced-in---please-build-with-a-newer--sdk-usually-done-by-using-the-most-recent-version-of-xcode"></a>MT4162: Der Typ "\*" (verwendet als * {2}) ist in * * nicht verfügbar (er wurde in * * eingeführt)\* , erstellen Sie mit einem neueren * SDK (in der Regel mit der aktuellsten Version von Xcode).

Die Registrierungsstelle hat einen Typ gefunden, der nicht im aktuellen SDK enthalten ist.

Aktualisieren Sie Xcode.

<a name="MT4163" />

### <a name="mt4163-internal-error-in-the-registrar--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4163: Interner Fehler in der Registrierungsstelle (*). Melden Sie einen Fehlerbericht unter http://bugzilla.xamarin.com

Dieser Fehler weist auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT4164" />

### <a name="mt4164-cannot-export-the-property--because-its-selector--is-an-objective-c-keyword-please-use-a-different-name"></a>MT4164: Die Eigenschaft "\*" kann nicht exportiert werden, da\*der Selektor "" ein Schlüsselwort "Keyword-C" ist. Verwenden Sie einen anderen Namen.

Der Selektor für die betreffende Eigenschaft ist kein gültiger Ziel-C-Identifer.

Verwenden Sie einen gültigen Ziel-C-Bezeichner als Selektoren.

<a name="MT4165" />

### <a name="mt4165-the-registrar-couldnt-find-the-type-systemvoid-in-any-of-the-referenced-assemblies"></a>MT4165: Der Typ "System. void" konnte von der Registrierungsstelle nicht in einer der referenzierten Assemblys gefunden werden.

Dieser Fehler weist höchstwahrscheinlich auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT4166" />

### <a name="mt4166-cannot-register-the-method--because-the-signature-contains-a-type--that-isnt-a-reference-type"></a>MT4166: Die Methode "\*" kann nicht registriert werden, da die Signatur einen\*Typ () enthält, der kein Verweistyp ist.

Dies weist normalerweise auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT4167" />

### <a name="mt4167-cannot-register-the-method--because-the-signature-contains-a-generic-type--with-a-generic-argument-type-that-isnt-an-nsobject-subclass-"></a>MT4167: Die-Methode\*kann nicht registriert werden, da die Signatur einen generischen\*Typ () mit einem generischen Argumenttyp enthält, bei dem es sich nicht um eine NSObject-Unterklasse (*) handelt.

Dies weist normalerweise auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT4168" />

### <a name="mt4168-cannot-register-the-type-managed_name-because-its-objective-c-name-exported_name-is-an-objective-c-keyword-please-use-a-different-name"></a>MT4168: Der Typ "{Managed\_Name}" kann nicht registriert werden, da der Ziel-c-Name "{exportierter\_Name}" ein Schlüsselwort der Ziel-c ist. Verwenden Sie einen anderen Namen.

Der Ziel-c-Name für den fraglichen Typ ist kein gültiger Ziel-c-Bezeichner.

Verwenden Sie einen gültigen Ziel-C-Bezeichner.

<a name="MT4169" />

### <a name="mt4169-failed-to-generate-a-pinvoke-wrapper-for-method-message"></a>MT4169: Fehler beim Generieren eines P/Callback-Wrappers für {Method}: {Message}

Xamarin. IOS konnte keine P/Aufruf-Wrapper Funktion für die angegebene generieren.
Überprüfen Sie die gemeldete Fehlermeldung auf die zugrunde liegende Ursache.

<a name="MT4170" />

### <a name="mt4170-the-registrar-cant-convert-from-managed-type-to-native-type-for-the-return-value-in-the-method-method"></a>MT4170: Die Registrierungsstelle kann für den Rückgabewert in der Methode "{Method}" nicht von "{Managed Type}" in "{Native Type}" konvertieren.

Weitere Informationen finden Sie in der Beschreibung von error <a href="#MT4172">MT4172</a>.

<a name="MT4171" />

### <a name="mt4171-the-bindas-attribute-on-the-member-member-is-invalid-the-bindas-type-type-is-different-from-the-property-type-type"></a>MT4171: Das Bindas-Attribut für den Member "{Member}" ist ungültig: der Bindas-Typ "{Type}" unterscheidet sich vom Eigenschaftentyp "{Type}".

Stellen Sie sicher, dass der Typ im Bindas-Attribut mit dem Typ des Elements übereinstimmt, mit dem es verbunden ist.

<a name="MT4172" />

### <a name="mt4172-the-registrar-cant-convert-from-native-type-to-managed-type-for-the-parameter-parameter-name-in-the-method-method"></a>MT4172: Die Registrierungsstelle kann für den Parameter "{Parameter Name}" in der Methode "{Method}" nicht von "{Native Type}" in "{Managed Type}" konvertieren.

Die Registrierung zwischen den erwähnten Typen wird von der Registrierungsstelle nicht unterstützt.

Dies ist ein Fehler in xamarin. IOS, wenn die betreffende API von xamarin. IOS bereitgestellt wird. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

Wenn Sie beim Entwickeln eines Bindungs Projekts für eine native Bibliothek darauf stoßen, können wir Unterstützung für neue Kombinationen von Typen hinzufügen. Wenn dies der Fall ist, müssen Sie eine Erweiterungs Anforderung auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) mit einem Testfall einreichen und evaluieren.

## <a name="mt5xxx-gcc-and-toolchain-error-messages"></a>MT5xxx: GCC-und Toolkette-Fehlermeldungen

### <a name="mt51xx-compilation"></a>MT51xx: Kompilierung

<!--
 MT5xxx GCC and toolchain
  MT51xx compilation
  -->

<a name="MT5101" />

### <a name="mt5101-missing--compiler-please-install-xcode-command-line-tools-component"></a>MT5101: Der ' * '-Compiler fehlt. Installieren Sie die Xcode-Komponente "Befehlszeilen Tools".

<a name="MT5102" />

### <a name="mt5102-failed-to-assemble-the-file--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT5102: Fehler beim Assemblieren der Datei "*". Melden Sie einen Fehlerbericht unter http://bugzilla.xamarin.com

<a name="MT5103" />

### <a name="mt5103-failed-to-compile-the-file--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT5103: Die Datei "*" konnte nicht kompiliert werden. Melden Sie einen Fehlerbericht unter http://bugzilla.xamarin.com

<a name="MT5104" />

### <a name="mt5104-could-not-find-neither-the--nor-the--compiler-please-install-xcode-command-line-tools-component"></a>MT5104: Weder '\*' noch der Compiler '\*' gefunden. Installieren Sie die Xcode-Komponente "Befehlszeilen Tools".

<!-- 5105 is used by mmp -->

<a name="MT5106" />

### <a name="mt5106-could-not-compile-the-files--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT5106: Die Dateien "*" konnten nicht kompiliert werden. Melden Sie einen Fehlerbericht unter http://bugzilla.xamarin.com

Dies weist normalerweise auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

### <a name="mt52xx-linking"></a>MT52xx: Verknüpfen

<!--
  MT52xx linking
  -->

<a name="MT5201" />

### <a name="mt5201-native-linking-failed-please-review-the-build-log-and-the-user-flags-provided-to-gcc-"></a>MT5201: Fehler beim nativen verknüpfen. Überprüfen Sie das Buildprotokoll und die Benutzerflags, die für gcc bereitgestellt werden:

<a name="MT5202" />

### <a name="mt5202-native-linking-failed-please-review-the-build-log"></a>MT5202: Fehler beim nativen verknüpfen. Überprüfen Sie das Buildprotokoll.

<a name="MT5203" />

### <a name="mt5203-native-linking-warning-"></a>MT5203: Native Verknüpfungs Warnung: *

<!--- 5204-5208 are not used -->

<a name="MT5209" />

### <a name="mt5209-native-linking-error-"></a>MT5209: Nativer Verknüpfungs Fehler: *

<a name="MT5210" />

### <a name="mt5210-native-linking-failed-undefined-symbol--please-verify-that-all-the-necessary-frameworks-have-been-referenced-and-native-libraries-are-properly-linked-in"></a>MT5210: Fehler beim nativen verknüpfen, nicht definiertes Symbol: *. Stellen Sie sicher, dass auf alle erforderlichen Frameworks verwiesen wurde und dass Native Bibliotheken ordnungsgemäß verknüpft sind.

Dies ist der Fall, wenn der Native Linker kein Symbol finden kann, auf das irgendwo verwiesen wird. Dies kann verschiedene Ursachen haben:

- Eine Drittanbieter Bindung erfordert ein Framework, aber die Bindung gibt dies nicht in Ihrem `[LinkWith]` -Attribut an. Lösungen
  - Wenn Sie der Autor der Drittanbieter Bindung sind oder Zugriff auf die zugehörige Quelle haben, ändern Sie das- `[LinkWith]` Attribut der Bindung, um das Framework, das es benötigt, einzubeziehen:

    ```csharp
    [LinkWith ("mylib.a", Frameworks = "SystemConfiguration")]
    ```

  - Wenn Sie die Bindung von Drittanbietern nicht ändern können, können Sie manuell eine Verknüpfung mit dem erforderlichen Framework `-gcc_flags '-framework SystemFramework'` herstellen `mtouch` , indem Sie an übergeben. (Dies erfolgt durch Ändern der zusätzlichen mtouchscreen-Argumente auf der Seite IOS-Buildoptionen des Projekts. Beachten Sie, dass dies für jede Projekt Konfiguration erfolgen muss.
- In einigen Fällen besteht eine verwaltete Bindung aus mehreren nativen Bibliotheken, die alle in den Bindungen enthalten sein müssen. Es ist möglich, in jedem Bindungs Projekt mehr als eine systemeigene Bibliothek zu haben. Daher besteht die Lösung darin, dem Bindungs Projekt lediglich alle erforderlichen nativen Bibliotheken hinzuzufügen.</li>
- Eine verwaltete Bindung verweist auf native Symbole, die in der nativen Bibliothek nicht vorhanden sind.
    Dies geschieht normalerweise, wenn eine Bindung für einige Zeit vorhanden ist, und der systemeigene Code wurde während dieser Zeit geändert, sodass eine bestimmte Native Klasse entweder entfernt oder umbenannt wurde, während die Bindung nicht aktualisiert wurde.
- Ein P/-Aufruf verweist auf ein natives Symbol, das nicht vorhanden ist. Ab xamarin. IOS 7,4 wird ein <a href="#MT5214">MT5214</a> -Fehler in diesem Fall gemeldet (Weitere Informationen finden Sie unter MT5214).
- Eine Bindung/Bibliothek eines Drittanbieters wurde mit C++erstellt, aber die Bindung gibt dies nicht im- `[LinkWith]` Attribut an. Dies ist normalerweise recht leicht zu erkennen, da die Symbole über gescholtete C++ Symbole verfügen (ein häufiges `__ZNKSt9exception4whatEv`Beispiel ist).
  - Wenn Sie der Autor der Drittanbieter Bindung sind oder Zugriff auf die zugehörige Quelle haben, ändern Sie das-Attribut `[LinkWith]` der Bindung, um `IsCxx` das-Flag festzulegen:

    ```csharp
    [LinkWith ("mylib.a", IsCxx = true)]
    ```

  - Wenn Sie die Bindung von Drittanbietern nicht ändern können, oder wenn Sie manuell mit einer Drittanbieter Bibliothek verknüpfen, können Sie das entsprechende Flag durch Übergabe `-cxx` an mberühren festlegen (Dies erfolgt durch Ändern der zusätzlichen mberührungs-Argumente auf der Seite IOS-Buildoptionen des Projekts). . Beachten Sie, dass dies für jede Projekt Konfiguration erfolgen muss.

<a name="MT5211" />

### <a name="mt5211-native-linking-failed-undefined-objective-c-class--the-symbol--could-not-be-found-in-any-of-the-libraries-or-frameworks-linked-with-your-application"></a>MT5211: Nativer Verknüpfungs Fehler, nicht definierte Ziel- \*C-Klasse:. Das Symbol "\*" konnte in keiner der Bibliotheken oder Frameworks gefunden werden, die mit Ihrer Anwendung verknüpft sind.

Dies ist der Fall, wenn der Native Linker eine Ziel-C-Klasse nicht finden kann, auf die irgendwo verwiesen wird. Dies kann verschiedene Ursachen haben: das gleiche wie bei [MT5210](#MT5210) und zusätzlich:

- Eine Bindung eines Drittanbieters hat ein Ziel-C-Protokoll gebunden, aber es nicht mit dem `[Protocol]` -Attribut in seiner API-Definition versehen. Lösungen
  - Fügen Sie das `[Protocol]` fehlende Attribut hinzu:

    ```csharp
    [BaseType (typeof (NSObject))]
    [Protocol] // Add this
    public interface MyProtocol
    {
    }
    ```

<a name="MT5212" />

### <a name="mt5212-native-linking-failed-duplicate-symbol-"></a>MT5212: Fehler bei nativem verknüpfen, doppeltes Symbol: *.

Dies ist der Fall, wenn der Native Linker duplizierte Symbole zwischen allen nativen Bibliotheken findet. Bei diesem Fehler kann es sich um einen oder mehrere [MT5213](#MT5213) -Fehler mit dem Speicherort für jedes Vorkommen des Symbols handeln. Mögliche Ursachen für diesen Fehler:

- Die gleiche native Bibliothek ist zweimal enthalten.
- Es gibt zwei unterschiedliche Native Bibliotheken, die dieselben Symbole definieren.
- Eine native Bibliothek ist nicht ordnungsgemäß erstellt und enthält dasselbe Symbol mehrmals.
  Dies können Sie überprüfen, indem Sie die folgenden Befehle von einem Terminal verwenden (ersetzen Sie "i386" durch x86_64/armv7/armv7s/arm64 entsprechend der Architektur, für die Sie aufbauen):

  ```
  # Native libraries are usually fat libraries, containing binary code for
  # several architectures in the same file. First we extract the binary
  # code for the architecture we're interested in.
  lipo libNative.a -thin i386 -output libNative.i386.a

  # Now query the native library for the duplicated symbol.
  nm libNative.i386.a | fgrep 'SYMBOL'

  # You can also list the object files inside the native library.
  # In most cases this will reveal duplicated object files.
  ar -t libNative.i386.a
  ```

  Es gibt mehrere Möglichkeiten, dieses Problem zu beheben:

  - Fordern Sie an, dass der Anbieter der systemeigenen Bibliothek ihn repariert und die aktualisierte Version bereitstellt.
  - Korrigieren Sie es selbst, indem Sie die zusätzlichen Objektdateien entfernen (dies funktioniert nur, wenn das Problem in der Tat duplizierte Objektdateien ist).

  ```
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
  ```

  - Bitten Sie den Linker, nicht verwendeten Code zu entfernen. Xamarin. IOS führt dies automatisch aus, wenn alle der folgenden Bedingungen erfüllt sind:
    - Die `[LinkWith]` Attribute der Bindungen von Drittanbietern haben SmartLink aktiviert:

      ```csharp
      [assembly: LinkWith ("libNative.a", SmartLink = true)]
      ```

    - No `-gcc_flags` wird an mberührungs (im Feld zusätzliche mberührungs-Argumente der IOS-Buildoptionen des Projekts) weitergegeben.
    - Es ist auch möglich, den Linker direkt aufzufordern, nicht verwendeten Code zu entfernen `-gcc_flags -dead_strip` , indem er den zusätzlichen mberührungs-Argumenten in den IOS-Buildoptionen des Projekts hinzufügt.

<a name="MT5213" />

### <a name="mt5213-duplicate-symbol-in--location-related-to-previous-error"></a>MT5213: Doppeltes Symbol in: * (Speicherort im Zusammenhang mit dem vorherigen Fehler)

Dieser Fehler wird nur in Verbindung mit [MT5212](#MT5212)gemeldet. Weitere Informationen finden Sie unter [MT5212](#MT5212) .

<a name="MT5214" />

### <a name="mt5214-native-linking-failed-undefined-symbol--this-symbol-was-referenced-the-managed-member--please-verify-that-all-the-necessary-frameworks-have-been-referenced-and-native-libraries-linked"></a>MT5214: Fehler beim nativen verknüpfen, nicht definiertes Symbol: *. Auf dieses Symbol wurde der verwaltete Member * verwiesen. Überprüfen Sie, ob auf alle erforderlichen Frameworks verwiesen wurde und welche systemeigenen Bibliotheken verknüpft sind.

Dieser Fehler wird gemeldet, wenn der verwaltete Code einen P/-Aufruf für eine native Methode enthält, die nicht vorhanden ist. Beispiel:

```csharp
using System.Runtime.InteropServices;
class MyImports {
   [DllImport ("__Internal")]
   public static extern void MyInexistentFunc ();
}
```

Es gibt einige mögliche Lösungen:

- Entfernen Sie die fraglichen P/Aufrufe aus dem Quellcode.
- Aktivieren Sie den verwalteten Linker für alle Assemblys (Dies erfolgt in den IOS-Buildoptionen des Projekts, indem Sie "Linker Verhalten" auf "alle Assemblys" festlegen). Dadurch werden alle von Ihnen nicht mehr in der APP verwendeten P/Aufrufe (automatisch, anstatt manuell wie der vorherige Punkt) entfernt. Der Nachteil ist, dass dadurch Ihr Simulator etwas langsamer erstellt wird, und die APP wird möglicherweise beeinträchtigt, wenn Sie Reflektion verwendet. Weitere Informationen zum Linker finden Sie [hier](~/ios/deploy-test/linker.md) .)
- Erstellen Sie eine zweite native Bibliothek, die die fehlenden systemeigenen Symbole enthält. Beachten Sie, dass dies lediglich eine Problem Umgehung ist (wenn Sie versuchen, diese Funktionen aufzurufen, stürzt Ihre APP ab).

<a name="MT5215" />

### <a name="mt5215-references-to--might-require-additional--frameworkxxx-or--lxxx-instructions-to-the-native-linker"></a>MT5215: Verweise auf "*" erfordern möglicherweise zusätzliche-Framework = xxx-oder-LXXX-Anweisungen für den nativen Linker.

Dies ist eine Warnung, die angibt, dass ein P/-Aufruf erkannt wurde, um auf die betreffende Bibliothek zu verweisen, die APP jedoch nicht mit ihr verknüpft ist.

<a name="MT5216" />

### <a name="mt5216-native-linking-failed-for--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT5216: Fehler beim nativen verknüpfen für *. Melden Sie einen Fehlerbericht unter http://bugzilla.xamarin.com

Dieser Fehler wird gemeldet, wenn die Ausgabe des AOT-Compilers verknüpft wird.

Dieser Fehler weist höchstwahrscheinlich auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT5217" />

### <a name="mt5217-native-linking-possibly-failed-because-the-linker-command-line-was-too-long--characters"></a>MT5217: Fehler bei der nativen Verknüpfung, weil die Linkerbefehlszeile zu lang war (* Zeichen).

Fehler beim nativen verknüpfen. Dies ist möglich, weil der Linker-Befehl zu lang war.

Xamarin. IOS-Projekte verweisen häufig dynamisch auf native Symbole, was bedeutet, dass der Native Linker solche systemeigenen Symbole während des systemeigenen Verknüpfungs Vorgangs entfernen kann, da der Native Linker nicht feststellt, dass diese Symbole verwendet werden.

Normalerweise wird der Native Linker von xamarin. IOS mithilfe des `-u symbol` Linkerflags aufgefordert, solche Symbole beizubehalten. wenn es jedoch viele derartige Symbole gibt, kann die gesamte Befehlszeile die maximale Befehlszeilen Länge überschreiten, wie vom Betriebssystem angegeben.

Es gibt einige mögliche Quellen für solche dynamischen Symbole:

- Ruft Methoden in statisch verknüpften Bibliotheken auf (wobei der DLL-Name im DllImport-Attribut `__Internal` `[DllImport ("__Internal")]`enthalten ist).
- Feldverweise auf Speicherorte in statisch verknüpften Bibliotheken aus Bindungs Projekten (`[Field]` Attribute).
- Ziel-C-Klassen, auf die in statisch verknüpften Bibliotheken aus Bindungs Projekten verwiesen wird (bei Verwendung von inkrementellen Builds oder bei Verwendung der statischen Registrierungsstelle).

Folgende Lösungen sind möglich:

- Aktivieren Sie den verwalteten Linker (falls möglich, für alle Assemblys anstelle von SDK-Assemblys). Dadurch werden möglicherweise genug Quellen für dynamische Symbole entfernt, damit die Befehlszeile des Linkers den maximalen Wert nicht überschreitet.
- Reduzieren Sie die Anzahl von P/aufruft-, Field References-und/oder Ziel-C-Klassen.
- Schreiben Sie die dynamischen Symbole so um, dass Sie kürzere Namen haben.
- Übergeben `-dlsym:false` Sie als zusätzliches mberührungs-Argument in den IOS-Buildoptionen des Projekts. Mit dieser Option generiert xamarin. IOS einen systemeigenen Verweis im AOT-kompilierten Code und muss den Linker nicht auffordern, dieses Symbol zu behalten. Dies funktioniert jedoch nur bei gerätebuilds, und es werden Linker-Fehler ausgelöst, wenn Funktionen vorhanden sind, die in der statischen Bibliothek nicht vorhanden sind.
- Übergeben `--dynamic-symbol-mode=code` Sie als zusätzliche mberührungs-Argumente in den IOS-Buildoptionen des Projekts. Mit dieser Option generiert xamarin. IOS zusätzlichen systemeigenen Code, der auf diese Symbole verweist, anstatt den systemeigenen Linker aufzufordern, diese Symbole mit Befehlszeilen Argumenten zu speichern. Der Nachteil dieses Ansatzes besteht darin, dass die Größe der ausführbaren Datei etwas vergrößert wird.
- Aktivieren Sie die statische Registrierungsstelle `--registrar:static` , indem Sie als zusätzliches mberührungs-Argument in den IOS-Buildoptionen des Projekts übergeben (für simulatorbuilds, da die statische Registrierungsstelle bereits die Standardeinstellung für gerätebuilds ist). Die statische Registrierungsstelle generiert Code, der Ziel-C-Klassen statisch referenziert, sodass es nicht erforderlich ist, den nativen Linker aufzufordern, solche Klassen beizubehalten.
- Inkrementelle Builds deaktivieren (für gerätebuilds). Wenn inkrementelle Builds aktiviert sind, wird der von der statischen Registrierungsstelle generierte Code vom systemeigenen Linker nicht berücksichtigt. Dies bedeutet, dass xamarin. IOS weiterhin den Linker auffordern muss, referenzierte Ziel-C-Klassen beizubehalten. Dadurch wird die Deaktivierung inkrementeller Builds verhindert.

<a name="MT5218" />

### <a name="mt5218-cant-ignore-the-dynamic-symbol-symbol---ignore-dynamic-symbolsymbol-because-it-was-not-detected-as-a-dynamic-symbol"></a>MT5218: Das dynamische Symbol {Symbol} (--Ignore-Dynamic-Symbol = {Symbol}) kann nicht ignoriert werden, weil es nicht als dynamisches Symbol erkannt wurde.

Das Befehlszeilenargument `--ignore-dynamic-symbol=symbol` wurde erfolgreich, aber dieses Symbol ist kein Symbol, das als dynamisches Symbol erkannt wurde, das manuell beibehalten werden muss.

Hierfür gibt es zwei Hauptgründe:

- Der Symbol Name ist falsch.
  - Fügen Sie dem Symbolnamen keinen Unterstrich voran.
  - Das Symbol für die Ziel-C- `OBJC_CLASS_$_<classname>`Klassen ist.
- Das Symbol ist korrekt, aber es ist ein Symbol, das bereits auf normale Weise beibehalten wird (einige Buildoptionen bewirken, dass die genaue Liste dynamischer Symbole variiert).

### <a name="mt53xx-other-tools"></a>MT53xx: Sonstige Tools

<!--
  MT53xx other tools
  -->

<a name="MT5301" />

### <a name="mt5301-missing-strip-tool-please-install-xcode-command-line-tools-component"></a>MT5301: Fehlendes ' Strip '-Tool. Installieren Sie die Xcode-Komponente "Befehlszeilen Tools".

<a name="MT5302" />

### <a name="mt5302-missing-dsymutil-tool-please-install-xcode-command-line-tools-component"></a>MT5302: Fehlendes dsymutil-Tool. Installieren Sie die Xcode-Komponente "Befehlszeilen Tools".

<a name="MT5303" />

### <a name="mt5303-failed-to-generate-the-debug-symbols-dsym-directory-please-review-the-build-log"></a>MT5303: Fehler beim Generieren der Debugsymbole (dsym-Verzeichnis). Überprüfen Sie das Buildprotokoll.

Fehler beim Ausführen von dsymutil für das endgültige App-Verzeichnis zum Erstellen der Debugsymbole. Überprüfen Sie das Buildprotokoll, um die Ausgabe von dsymutil anzuzeigen, und sehen Sie, wie es korrigiert werden kann.

<a name="MT5304" />

### <a name="mt5304-failed-to-strip-the-final-binary-please-review-the-build-log"></a>MT5304: Fehler beim Entfernen der letzten Binärdatei. Überprüfen Sie das Buildprotokoll.

Fehler beim Ausführen des "Strip"-Tools, um Debuginformationen aus der Anwendung zu entfernen.

<a name="MT5305" />

### <a name="mt5305-missing-lipo-tool-please-install-xcode-command-line-tools-component"></a>MT5305: Fehlendes Lipo-Tool. Installieren Sie die Xcode-Komponente "Befehlszeilen Tools".

<a name="MT5306" />

### <a name="mt5306-failed-to-create-the-a-fat-library-please-review-the-build-log"></a>MT5306: Fehler beim Erstellen einer FAT-Bibliothek. Überprüfen Sie das Buildprotokoll.

Beim Ausführen des Tools "Lipo" ist ein Fehler aufgetreten. Überprüfen Sie das Buildprotokoll, um den von "Lipo" gemeldeten Fehler anzuzeigen.

<a name="MT5307" />

### <a name="mt5307-failed-to-sign-the-executable-please-review-the-build-log"></a>MT5307: Fehler beim Signieren der ausführbaren Datei. Überprüfen Sie das Buildprotokoll.

Beim Signieren der Anwendung ist ein Fehler aufgetreten. Überprüfen Sie das Buildprotokoll, um den von "Codesign" gemeldeten Fehler anzuzeigen.

<!-- 5308 is used by mmp -->
<!-- 5309 is used by mmp -->
<!-- 5310 is used by mmp -->

## <a name="mt6xxx-mtouch-internal-tools-error-messages"></a>MT6xxx: Fehlermeldungen für interne mberührungs-Tools

### <a name="mt600x-stripper"></a>MT600x: Entfernen

<!--
 MT6xxx mtouch internal tools
  MT600x Stripper
  -->

<a name="MT6001" />

### <a name="mt6001-running-version-of-cecil-doesnt-support-assembly-stripping"></a>MT6001: Die laufende Version von Cecil unterstützt das Entfernen von Assemblys

<a name="MT6002" />

### <a name="mt6002-could-not-strip-assembly-"></a>MT6002: Die Assembly `*`konnte nicht entfernt werden.

Fehler beim Entfernen von verwaltetem Code (Entfernen des Il-Codes) aus den Assemblys in der Anwendung.

<a name="MT6003" />

### <a name="mt6003-unauthorizedaccessexception-message"></a>MT6003: [UnauthorizedAccessException-Meldung]

Beim Entfernen von Debugsymbolen aus der Anwendung ist ein Sicherheitsfehler aufgetreten.

## <a name="mt7xxx-msbuild-error-messages"></a>MT7xxx: MSBuild-Fehlermeldungen

<!--
 MT7xxx msbuild errors
  -->

<a name="MT7001" />

### <a name="mt7001-could-not-resolve-host-ips-for-wifi-debugger-settings"></a>MT7001: Host-IPS für Einstellungen des WLAN-Debuggers konnten nicht aufgelöst werden.

*MSBuild-Aufgabe: DetectDebugNetworkConfigurationTaskBase*

Schritte zur Problembehandlung:

- versuchen Sie, `csharp -e 'System.Net.Dns.GetHostEntry (System.Net.Dns.GetHostName ()).AddressList'` auszuführen (Sie sollten eine IP-Adresse und keinen Fehler offenlegen).
- versuchen Sie, "ping \`Hostname\`" auszuführen, wodurch Sie möglicherweise weitere Informationen erhalten, wie z. b.:`cannot resolve MyHost.local: Unknown host`

In einigen Fällen handelt es sich um ein Problem mit dem lokalen Netzwerk, das durch Hinzufügen des unbekannten Hosts `127.0.0.1    MyHost.local` in `/etc/hosts`behoben werden kann.

<a name="MT7002" />

### <a name="mt7002-this-machine-does-not-have-any-network-adapters-this-is-required-when-debugging-or-profiling-on-device-over-wifi"></a>MT7002: Dieser Computer verfügt über keine Netzwerkadapter. Dies ist beim Debuggen oder bei der Profilerstellung auf dem Gerät über WiFi erforderlich.

*MSBuild-Aufgabe: DetectDebugNetworkConfigurationTaskBase*

<a name="MT7003" />

### <a name="mt7003-the-app-extension--does-not-contain-an-infoplist"></a>MT7003: Die APP-Erweiterung "*" enthält keine Info. plist.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7004" />

### <a name="mt7004-the-app-extension--does-not-specify-a-cfbundleidentifier"></a>MT7004: In der APP-Erweiterung "*" ist kein CFBundleIdentifier angegeben.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7005" />

### <a name="mt7005-the-app-extension--does-not-specify-a-cfbundleexecutable"></a>MT7005: In der APP-Erweiterung "*" ist kein "cfbundleexe" angegeben.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7006" />

### <a name="mt7006-the-app-extension--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a>MT7006: Die APP-Erweiterung\*"" weist einen ungültigen CFBundleIdentifier (\*) auf, beginnt nicht mit dem CFBundleIdentifier (*) des Haupt App Bundle.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7007" />

### <a name="mt7007-the-app-extension--has-a-cfbundleidentifier--that-ends-with-the-illegal-suffix-key"></a>MT7007: Die APP-Erweiterung\*' ' weist einen CFBundleIdentifier\*() auf, der mit dem unzulässigen Suffix '. Key ' endet.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7008" />

### <a name="mt7008-the-app-extension--does-not-specify-a-cfbundleshortversionstring"></a>MT7008: In der APP-Erweiterung "*" ist kein cfbundleshortversionstring angegeben.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7009" />

### <a name="mt7009-the-app-extension--has-an-invalid-infoplist-it-does-not-contain-an-nsextension-dictionary"></a>MT7009: Die APP-Erweiterung "*" weist eine ungültige "Info. plist" auf: Sie enthält kein nsextension-Wörterbuch.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7010" />

### <a name="mt7010-the-app-extension--has-an-invalid-infoplist-the-nsextension-dictionary-does-not-contain-an-nsextensionpointidentifier-value"></a>MT7010: Die APP-Erweiterung "*" weist eine ungültige "Info. plist" auf: das nsextension-Wörterbuch enthält keinen nsextensionpointidentifier-Wert.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7011" />

### <a name="mt7011-the-watchkit-extension--has-an-invalid-infoplist-the-nsextension-dictionary-does-not-contain-an-nsextensionattributes-dictionary"></a>MT7011: Die watchkit-Erweiterung "*" hat eine ungültige Info. plist: das nsextension-Wörterbuch enthält kein nsextensionattribute-Wörterbuch.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7012" />

### <a name="mt7012-the-watchkit-extension--does-not-have-exactly-one-watch-app"></a>MT7012: Die watchkit-Erweiterung "*" verfügt nicht über genau eine Watch-app.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7013" />

### <a name="mt7013-the-watchkit-extension--has-an-invalid-infoplist-uirequireddevicecapabilities-must-contain-the-watch-companion-capability"></a>MT7013: Die watchkit-Erweiterung "*" hat eine ungültige Info. plist: Uirequirements ddevicecapcapability muss die Funktion "Watch-Companion" enthalten.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7014" />

### <a name="mt7014-the-watch-app--does-not-contain-an-infoplist"></a>MT7014: Die Watch-app "*" enthält keine Info. plist.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7015" />

### <a name="mt7015-the-watch-app--does-not-specify-a-cfbundleidentifier"></a>MT7015: Die Watch-app "*" gibt keinen CFBundleIdentifier an.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7016" />

### <a name="mt7016-the-watch-app--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a>MT7016: Die Watch-APP\*' ' weist einen ungültigen CFBundleIdentifier (\*) auf, beginnt nicht mit dem CFBundleIdentifier (*) des Haupt App Bundle.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7017" />

### <a name="mt7017-the-watch-app--does-not-have-a-valid-uidevicefamily-value-expected-watch-4-but-found--"></a>MT7017: Die Watch-APP\*' ' weist keinen gültigen uidevicefamily-Wert auf. Es wurde ' Watch (4) ' erwartet,\* aber ' (*) ' gefunden.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7018" />

### <a name="mt7018-the-watch-app--does-not-specify-a-cfbundleexecutable"></a>MT7018: Die Watch-app ' * ' gibt keine cfbundleexe-Datei an.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7019" />

### <a name="mt7019-the-watch-app--has-an-invalid-wkcompanionappbundleidentifier-value--it-does-not-match-the-main-app-bundles-cfbundleidentifier-"></a>MT7019: Die Watch-APP\*' ' weist einen ungültigen wkcompanionappbundleidentifier-\*Wert (' ') auf, stimmt nicht mit dem Haupt App Bundle CFBundleIdentifier (' * ').

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7020" />

### <a name="mt7020-the-watch-app--has-an-invalid-infoplist-the-wkwatchkitapp-key-must-be-present-and-have-a-value-of-true"></a>MT7020: Die Watch-app ' * ' hat eine ungültige Info. plist: der wkwatchkitapp-Schlüssel muss vorhanden sein und den Wert ' true ' aufweisen.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7021" />

### <a name="mt7021-the-watch-app--has-an-invalid-infoplist-the-lsrequiresiphoneos-key-must-not-be-present"></a>MT7021: Die Watch-app ' * ' hat eine ungültige Info. plist: der Schlüssel lsrequirements siphoneos darf nicht vorhanden sein.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7022" />

### <a name="mt7022-the-watch-app--does-not-contain-a-watch-extension"></a>MT7022: Die Watch-app "*" enthält keine Watch-Erweiterung.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7023" />

### <a name="mt7023-the-watch-extension--does-not-contain-an-infoplist"></a>MT7023: Die Watch-Erweiterung "*" enthält keine Info. plist.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7024" />

### <a name="mt7024-the-watch-extension--does-not-specify-a-cfbundleidentifier"></a>MT7024: In der Watch-Erweiterung "*" ist kein CFBundleIdentifier angegeben.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7025" />

### <a name="mt7025-the-watch-extension--does-not-specify-a-cfbundleexecutable"></a>MT7025: In der Watch-Erweiterung "*" ist kein "cfbundleexe" angegeben.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7026" />

### <a name="mt7026-the-watch-extension--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a>MT7026: Die Watch-Erweiterung\*"" weist einen ungültigen CFBundleIdentifier (\*) auf, beginnt nicht mit dem CFBundleIdentifier (*) des Haupt App Bundle.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7027" />

### <a name="mt7027-the-watch-extension--has-a-cfbundleidentifier--that-ends-with-the-illegal-suffix-key"></a>MT7027: Die Watch-Erweiterung\*' ' weist einen CFBundleIdentifier\*() auf, der mit dem unzulässigen Suffix '. Key ' endet.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7028" />

### <a name="mt7028-the-watch-extension--has-an-invalid-infoplist-it-does-not-contain-an-nsextension-dictionary"></a>MT7028: Die Watch-Erweiterung "*" weist eine ungültige "Info. plist" auf: Sie enthält kein nsextension-Wörterbuch.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7029" />

### <a name="mt7029-the-watch-extension--has-an-invalid-infoplist-the-nsextensionpointidentifier-must-be-comapplewatchkit"></a>MT7029: Die Watch-Erweiterung "*" hat eine ungültige Info. plist: der nsextensionpointidentifier muss "com. Apple. watchkit" lauten.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7030" />

### <a name="mt7030-the-watch-extension--has-an-invalid-infoplist-the-nsextension-dictionary-must-contain-nsextensionattributes"></a>MT7030: Die Watch-Erweiterung "*" hat eine ungültige Info. plist: das nsextension-Wörterbuch muss nsextensionattribute enthalten.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7031" />

### <a name="mt7031-the-watch-extension--has-an-invalid-infoplist-the-nsextensionattributes-dictionary-must-contain-a-wkappbundleidentifier"></a>MT7031: Die Watch-Erweiterung "*" hat eine ungültige Info. plist: das nsextensionattributwörterbuch muss einen wkappbundleidentifier enthalten.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7032" />

### <a name="mt7032-the-watchkit-extension--has-an-invalid-infoplist-uirequireddevicecapabilities-should-not-contain-the-watch-companion-capability"></a>MT7032: Die watchkit-Erweiterung "*" hat eine ungültige Info. plist: Uirequirements ddevicecapcapability sollte die Funktion "Watch-Companion" nicht enthalten.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7033" />

### <a name="mt7033-the-watch-app--does-not-contain-an-infoplist"></a>MT7033: Die Watch-app "*" enthält keine Info. plist.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7034" />

### <a name="mt7034-the-watch-app--does-not-specify-a-cfbundleidentifier"></a>MT7034: Die Watch-app "*" gibt keinen CFBundleIdentifier an.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7035" />

### <a name="mt7035-the-watch-app--does-not-have-a-valid-uidevicefamily-value-expected--but-found--"></a>MT7035: Die Watch-APP\*' ' weist keinen gültigen uidevicefamily-Wert auf. Es wurde "\* \*" erwartet, aber "()" gefunden.\*

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7036" />

### <a name="mt7036-the-watch-app--does-not-specify-a-cfbundleexecutable"></a>MT7036: Die Watch-app ' * ' gibt keine cfbundleexe-Datei an.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7037" />

### <a name="mt7037-the-watchkit-extension-extensionname-has-an-invalid-wkappbundleidentifier-value--it-does-not-match-the-watch-apps-cfbundleidentifier-"></a>MT7037: Die watchkit-Erweiterung "{ExtensionName}" weist einen ungültigen wkappbundleidentifier-\*Wert ("") auf, stimmt nicht mit dem CFBundleIdentifier der Watch\*-app ("").

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7038" />

### <a name="mt7038-the-watch-app--has-an-invalid-infoplist-the-wkcompanionappbundleidentifier-must-exist-and-must-match-the-main-app-bundles-cfbundleidentifier"></a>MT7038: Die Watch-app "*" weist eine ungültige Info. plist auf: der wkcompanionappbundleidentifier muss vorhanden sein und muss mit dem CFBundleIdentifier des Haupt App Bundle identisch sein.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7039" />

### <a name="mt7039-the-watch-app--has-an-invalid-infoplist-the-lsrequiresiphoneos-key-must-not-be-present"></a>MT7039: Die Watch-app ' * ' hat eine ungültige Info. plist: der Schlüssel lsrequirements siphoneos darf nicht vorhanden sein.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7040" />

### <a name="mt7040-the-app-bundle-appbundlepath-does-not-contain-an-infoplist"></a>MT7040: Der APP Bundle {appbundlepath} enthält keine Info. plist-Datei.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7041" />

### <a name="mt7041-main-infoplist-path-does-not-specify-a-cfbundleidentifier"></a>MT7041: Im Hauptpfad "Info. plist" ist kein CFBundleIdentifier angegeben.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7042" />

### <a name="mt7042-main-infoplist-path-does-not-specify-a-cfbundleexecutable"></a>MT7042: Im Haupt-Info. plist-Pfad ist keine cfbundleexe-Datei angegeben.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7043" />

### <a name="mt7043-main-infoplist-path-does-not-specify-a-cfbundlesupportedplatforms"></a>MT7043: Im Haupt-Info. plist-Pfad ist keine cfbundlesupportedplatforms angegeben.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7044" />

### <a name="mt7044-main-infoplist-path-does-not-specify-a-uidevicefamily"></a>MT7044: Im Haupt Pfad der Info. plist-Datei wird keine uidevicefamily angegeben.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7045" />

### <a name="mt7045-unrecognized-format-"></a>MT7045: Unbekanntes Format: *.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

Dabei kann * lauten:

- string
- array
- dict
- bool
- Reelle
- Ganze Zahl
- date
- Daten

<a name="MT7046" />

### <a name="mt7046-add-entry--incorrectly-specified"></a>MT7046: Hinzufügen: Der Eintrag (*) wurde fälschlicherweise angegeben.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7047" />

### <a name="mt7047-add-entry--contains-invalid-array-index"></a>MT7047: Hinzufügen: Der Eintrag * enthält einen ungültigen Array Index.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7048" />

### <a name="mt7048-add--entry-already-exists"></a>MT7048: Add: *-Eintrag ist bereits vorhanden.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7049" />

### <a name="mt7049-add-cant-add-entry--to-parent"></a>MT7049: Hinzufügen: Der Eintrag * kann dem übergeordneten Element nicht hinzugefügt werden.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7050" />

### <a name="mt7050-delete-cant-delete-entry--from-parent"></a>MT7050: Löschen: Der Eintrag * kann nicht aus dem übergeordneten Element gelöscht werden.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7051" />

### <a name="mt7051-delete-entry--contains-invalid-array-index"></a>MT7051: Löschen: Der Eintrag * enthält einen ungültigen Array Index.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7052" />

### <a name="mt7052-delete-entry--does-not-exist"></a>MT7052: Löschen: Der Eintrag * ist nicht vorhanden.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7053" />

### <a name="mt7053-import-entry--incorrectly-specified"></a>MT7053: Importieren Der Eintrag (*) wurde fälschlicherweise angegeben.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7054" />

### <a name="mt7054-import-entry--contains-invalid-array-index"></a>MT7054: Importieren Der Eintrag * enthält einen ungültigen Array Index.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7055" />

### <a name="mt7055-import-error-reading-file-"></a>MT7055: Importieren Fehler beim Lesen der Datei: *.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7056" />

### <a name="mt7056-import-cant-add-entry--to-parent"></a>MT7056: Importieren Der Eintrag * kann dem übergeordneten Element nicht hinzugefügt werden.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7057" />

### <a name="mt7057-merge-cant-add-array-entries-to-dict"></a>MT7057: Merge Array Einträge können nicht zum dict hinzugefügt werden.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7058" />

### <a name="mt7058-merge-specified-entry-must-be-a-container"></a>MT7058: Merge Der angegebene Eintrag muss ein Container sein.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7059" />

### <a name="mt7059-merge-entry--contains-invalid-array-index"></a>MT7059: Merge Der Eintrag * enthält einen ungültigen Array Index.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7060" />

### <a name="mt7060-merge-entry--does-not-exist"></a>MT7060: Merge Der Eintrag * ist nicht vorhanden.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7061" />

### <a name="mt7061-merge-error-reading-file-"></a>MT7061: Merge Fehler beim Lesen der Datei: *.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7062" />

### <a name="mt7062-set-entry--incorrectly-specified"></a>MT7062: Legen Sie Folgendes fest: Der Eintrag (*) wurde fälschlicherweise angegeben.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7063" />

### <a name="mt7063-set-entry--contains-invalid-array-index"></a>MT7063: Legen Sie Folgendes fest: Der Eintrag * enthält einen ungültigen Array Index.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7064" />

### <a name="mt7064-set-entry--does-not-exist"></a>MT7064: Legen Sie Folgendes fest: Der Eintrag * ist nicht vorhanden.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7065" />

### <a name="mt7065-unknown-propertylist-editor-action-"></a>MT7065: Unbekannte Aktion für den PropertyList-Editor: *.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7066" />

### <a name="mt7066-error-loading--"></a>MT7066: Fehler beim Laden von "*": *.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7067" />

### <a name="mt7067-error-saving--"></a>MT7067: Fehler beim Speichern von "*": *.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

## <a name="mt8xxx-runtime-error-messages"></a>MT8xxx: Lauf zeitfehlermeldungen

<!--
 MT8xxx runtime
  MT800x misc
  -->

<a name="MT8001" />

### <a name="mt8001-version-mismatch-between-the-native-xamarinios-runtime-and-monotouchdll-please-reinstall-xamarinios"></a>MT8001: Der Versions Konflikt zwischen der systemeigenen xamarin. IOS-Laufzeit und der MonoTouch. dll. Installieren Sie xamarin. IOS neu.

<a name="MT8002" />

### <a name="mt8002-could-not-find-the-method--in-the-type-"></a>MT8002: Die Methode "\*" im Typ "\*" wurde nicht gefunden.

<a name="MT8003" />

### <a name="mt8003-failed-to-find-the-closed-generic-method--on-the-type-"></a>MT8003: Fehler beim Suchen der geschlossenen generischen Methode\*"" für den Typ\*"".

<a name="MT8004" />

### <a name="mt8004-cannot-create-an-instance-of--for-the-native-object-0x-of-type--because-another-instance-already-exists-for-this-native-object-of-type-"></a>MT8004: Für das Native Objekt 0x * (vom Typ "*") kann keine Instanz von * erstellt werden, da bereits eine andere Instanz für dieses systemeigene Objekt (vom Typ *) vorhanden ist.

<a name="MT8005" />

### <a name="mt8005-wrapper-type--is-missing-its-native-objectivec-class-"></a>MT8005: In der System\*eigenen objectivec-Klasse "\*" fehlt der Wrapper-Typ "".

<a name="MT8006" />

### <a name="mt8006-failed-to-find-the-selector--on-the-type-"></a>MT8006: Die Auswahl "\*" für den Typ "\*" wurde nicht gefunden.

<a name="MT8007" />

### <a name="mt8007-cannot-get-the-method-descriptor-for-the-selector--on-the-type--because-the-selector-does-not-correspond-to-a-method"></a>MT8007: Der Methoden Deskriptor für die Auswahl '\*' für Typ '\*' kann nicht abgerufen werden, da der Selektor keiner Methode entspricht.

<a name="MT8008" />

### <a name="mt8008-the-loaded-version-of-xamariniosdll-was-compiled-for--bits-while-the-process-is--bits-please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8008: Die geladene Version von xamarin. IOS. dll wurde für * Bits kompiliert, während der Prozess * Bits ist. Bitte melden Sie einen Fehler http://bugzilla.xamarin.com unter.

Dies weist darauf hin, dass im Buildprozess etwas falsch ist. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT8009" />

### <a name="mt8009-unable-to-locate-the-block-to-delegate-conversion-method-for-the-method-s-parameter--please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8009: Der Block zum Delegieren der Konvertierungsmethode für die Methode wurde nicht gefunden *.* " s-Parameter # *. Bitte melden Sie einen Fehler http://bugzilla.xamarin.com unter.

Dies bedeutet, dass eine API nicht ordnungsgemäß gebunden ist. Wenn es sich um eine API handelt, die von xamarin verfügbar gemacht wird, melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new). Wenn es sich um eine Drittanbieter Bindung handelt, wenden Sie sich an den Hersteller.

<a name="MT8010" />

### <a name="mt8010-native-type-size-mismatch-between-xamariniosmacdll-and-the-executing-architecture-xamariniosmacdll-was-built-for--bit-while-the-current-process-is--bit"></a>MT8010: Die systemeigene Typgröße zwischen xamarin stimmt nicht überein. [IOS | Mac]. dll und die ausführende Architektur. Xamarin. [IOS | Mac]. dll wurde für *-Bit erstellt, während der aktuelle Prozess *-Bit ist.

Dies weist darauf hin, dass im Buildprozess etwas falsch ist. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT8011" />

### <a name="mt8011-unable-to-locate-the-delegate-to-block-conversion-attribute-delegateproxy-for-the-return-value-for-the-method--please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8011: Der Delegat zum Blockieren des Konvertierungs Attributs ([delegateproxy]) für den Rückgabewert *der Methode konnte*nicht gefunden werden. Bitte melden Sie einen Fehler http://bugzilla.xamarin.com unter.

Xamarin. IOS konnte eine erforderliche Methode zur Laufzeit nicht finden (zum Konvertieren eines Delegaten in einen-Block).

Dies weist normalerweise auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT8012" />

### <a name="mt8012-invalid-delegateproxyattribute-for-the-return-value-for-the-method--delegatetype-is-null-please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8012: Ungültiges "delegateproxyattribute" für den Rückgabewert der Methode *.* : DelegateType ist NULL. Bitte melden Sie einen Fehler http://bugzilla.xamarin.com unter.

Das delegateproxy-Attribut für die fragliche Methode ist ungültig.

Dies weist normalerweise auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT8013" />

### <a name="mt8013-invalid-delegateproxyattribute-for-the-return-value-for-the-method--delegatetype-2-specifies-a-type-without-a-handler-field-please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8013: Ungültiges "delegateproxyattribute" für den Rückgabewert der Methode *.* : DelegateType ({2}) gibt einen Typ ohne ein "Handler"-Feld an. Bitte melden Sie einen Fehler http://bugzilla.xamarin.com unter.

Das `[DelegateProxy]` Attribut für die fragliche Methode ist ungültig.

Dies weist normalerweise auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT8014" />

### <a name="mt8014-invalid-delegateproxyattribute-for-the-return-value-for-the-method--the-delegatetypes-2-handler-field-is-null-please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8014: Ungültiges "delegateproxyattribute" für den Rückgabewert der Methode *.* : Das handlerfeld des delegateType ({2}) ist NULL. Bitte melden Sie einen Fehler http://bugzilla.xamarin.com unter.

Das `[DelegateProxy]` Attribut für die fragliche Methode ist ungültig.

Dies weist normalerweise auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT8015" />

### <a name="mt8015-invalid-delegateproxyattribute-for-the-return-value-for-the-method--the-delegatetypes-2-handler-field-is-not-a-delegate-its-a--please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8015: Ungültiges "delegateproxyattribute" für den Rückgabewert der Methode *.* : Das handlerfeld von delegateType ({2}) ist kein Delegat, sondern ist ein *. Bitte melden Sie einen Fehler http://bugzilla.xamarin.com unter.

Das delegateproxy-Attribut für die fragliche Methode ist ungültig.

Dies weist normalerweise auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT8016" />

### <a name="mt8016-unable-to-convert-delegate-to-block-for-the-return-value-for-the-method--because-the-input-isnt-a-delegate-its-a--please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8016: Der Delegat kann für den Rückgabewert der Methode nicht in "Block" konvertiert werden *.* da die Eingabe kein Delegat ist, ist Sie "*". Bitte melden Sie einen Fehler http://bugzilla.xamarin.com unter.

Das `[DelegateProxy]` Attribut für die fragliche Methode ist ungültig.

Dies weist normalerweise auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<!-- 8017 is used by mmp -->

<a name="MT8018" />

### <a name="mt8018-internal-consistency-error-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT8018: Interner Konsistenz Fehler. Melden Sie einen Fehlerbericht unter http://bugzilla.xamarin.com.

Dies weist auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT8019" />

### <a name="mt8019-could-not-find-the-assembly--in-the-loaded-assemblies"></a>MT8019: Die Assembly * konnte in den geladenen Assemblys nicht gefunden werden.

Dies weist auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT8020" />

### <a name="mt8020-could-not-find-the-module-with-metadatatoken--in-the-assembly-"></a>MT8020: Das Modul mit metadataToken * wurde in der Assembly * nicht gefunden.

Dies weist auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT8021" />

### <a name="mt8021-unknown-implicit-token-type-"></a>MT8021: Unbekannter impliziter Tokentyp: *.

Dies weist auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT8022" />

### <a name="mt8022-expected-the-token-reference--to-be-a--but-its-a--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT8022: Es wurde erwartet, dass der Tokenverweis * *, aber ein * ist. Melden Sie einen Fehlerbericht unter http://bugzilla.xamarin.com.

Dies weist auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT8023" />

### <a name="mt8023-an-instance-object-is-required-to-construct-a-closed-generic-method-for-the-open-generic-method--token-reference--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT8023: Ein Instanzobjekt ist erforderlich, um eine geschlossene generische Methode für die geöffnete generische Methode zu erstellen: * (Tokenverweis: *). Melden Sie einen Fehlerbericht unter http://bugzilla.xamarin.com.

Dies weist auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

<a name="MT8024" />

### <a name="mt8024-could-not-find-a-valid-extension-type-for-the-smart-enum-smart_type-please-file-a-bug-at-httpsbugzillaxamarincom"></a>MT8024: Es wurde kein gültiger Erweiterungstyp für die Smart Enumeration "{smart_type}" gefunden. Bitte melden Sie einen Fehler https://bugzilla.xamarin.com unter.

Dies weist auf einen Fehler in xamarin. IOS hin. Bitte melden Sie ein neues Problem auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).
