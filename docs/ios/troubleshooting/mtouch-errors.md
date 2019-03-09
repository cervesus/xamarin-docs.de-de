---
title: Xamarin.iOS-Fehler
description: Dieses Dokument beschreibt die verschiedenen Fehler, die von Mtouch, das Tool zum Bündeln von Xamarin.iOS-Anwendungen generiert. Fehler werden aufgelistet, die von Code und erhalten eine vollständige Beschreibung.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9F76162B-D622-45DA-996B-2FBF8017E208
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/06/2018
ms.openlocfilehash: e6e3a989db922dc2941cca4c888c862ffe159241
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57672858"
---
# <a name="xamarinios-errors"></a>Xamarin.iOS-Fehler

## <a name="mt0xxx-mtouch-error-messages"></a>MT0xxx: Mtouch-Fehlermeldungen

Beispiel: Parameter, Umgebung, Tools fehlt.

<!--
 MT0xxx mtouch itself, e.g. parameters, environment (e.g. missing tools)
 https://github.com/xamarin/xamarin-macios/blob/master/tools/mtouch/error.cs
    -->

<a name="MT0000" />

### <a name="mt0000-unexpected-error---please-fill-a-bug-report-at-httpsgithubcomxamarinxamarin-maciosissuesnew"></a>MT0000: Unerwarteter Fehler: Geben Sie bitte einen Fehlerbericht an https://github.com/xamarin/xamarin-macios/issues/new

Ein unerwarteter Fehler ist aufgetreten. Bitte [einen Fehlerbericht](https://github.com/xamarin/xamarin-macios/issues/new) so viele Informationen wie möglich, einschließlich:

* Vollständiger Build erstellt Protokolle mit maximaler Ausführlichkeit (z. B. `-v -v -v -v` in die **Weitere Mtouch-Argumente**);
* Eine minimale Testfall, der den Fehler reproduzieren; und
* Alle Informationen für version

Die einfachste Möglichkeit zum Abrufen von Informationen zur genauen Version ist die Verwendung der **Visual Studio für Mac** Menü **zu Visual Studio für Mac** Element **Details anzeigen** Schaltfläche, und kopieren und Einfügen der Version-Informationen (können Sie die **Informationen kopieren** Schaltfläche).

<a name="MT0001" />

### <a name="mt0001--devname-was-provided-without-any-device-specific-action"></a>MT0001: "-Devname" ohne gerätespezifischen Aktion bereitgestellt wurde

Dies ist eine Warnung, die ausgegeben wird, wenn - Devname an Mtouch, wenn keine gerätespezifischen Aktion übergeben wird (-Logdev /-Installdev /-Killdev /-Launchdev / – Listapps) angefordert wurde.

<a name="MT0002" />

### <a name="mt0002-could-not-parse-the-environment-variable-"></a>MT0002: Konnte nicht analysiert werden. die Umgebungsvariable *.

Dieser Fehler tritt auf, wenn Sie versuchen, einen ungültige umgebungs-Schlüssel / Wert-Paar-Variable. Das richtige Format lautet: `mtouch --setenv=VARIABLE=VALUE`

<a name="MT0003" />

### <a name="mt0003-application-name-exe-conflicts-with-an-sdk-or-product-assembly-dll-name"></a>MT0003: Name der Anwendung "* .exe" steht in Konflikt mit einem Namen der Assembly (.dll) SDK oder eines Produkts.

Ausführbare Datei den Namen der Assembly und den Namen der Anwendung darf nicht den Namen der DLLs in der app übereinstimmen. Ändern Sie den Namen der ausführbaren Datei.

<a name="MT0004" />

### <a name="mt0004-new-refcounting-logic-requires-sgen-to-be-enabled-too"></a>MT0004: Neue referenzzählung Logik erfordert SGen zu aktiviert werden.

Wenn Sie die referenzzählung-Erweiterung aktivieren, müssen Sie auch der SGen Garbagecollector in iOS-Buildoptionen (Registerkarte "Erweitert") des Projekts aktivieren.

Ab Xamarin.iOS 7.2.1 diese Anforderung wurde aufgehoben, die Logik der neuen referenzzählung kann mit Boehm und SGen-Garbage Collectors aktiviert werden.

<a name="MT0005" />

### <a name="mt0005-the-output-directory--does-not-exist"></a>MT0005: Das Ausgabeverzeichnis * ist nicht vorhanden.

Erstellen Sie das Verzeichnis aus.

Dieser Fehler wird nicht mehr generiert, Mtouch erstellt automatisch das Verzeichnis aus, wenn er nicht vorhanden.

<a name="MT0006" />

### <a name="mt0006-there-is-no-devel-platform-at--use---platformplat-to-specify-the-sdk"></a>MT0006: Es gibt keine Entwickler-Plattform auf *, - Plattform verwenden = PLAT, um das SDK anzugeben.

SDK-Verzeichnis an der Position in der Fehlermeldung genannten wurde von Xamarin.iOS nicht gefunden. Stellen Sie sicher, dass der Pfad richtig ist.

<a name="MT0007" />

### <a name="mt0007-the-root-assembly--does-not-exist"></a>MT0007: Die stammassembly * ist nicht vorhanden.

Die Assembly am Speicherort in der Fehlermeldung genannten wurde von Xamarin.iOS nicht gefunden. Stellen Sie sicher, dass der Pfad richtig ist.

<a name="MT0008" />

### <a name="mt0008-you-should-provide-one-root-assembly-only-found--assemblies-"></a>MT0008: Sie sollten eine Stamm-Assembly, die sich nur # Assemblys bereitstellen: *.

Mehr als eine stammassembly wurde an Mtouch, übergeben, während es nur eine Assembly sein kann.

<a name="MT0009" />

### <a name="mt0009-error-while-loading-assemblies-"></a>MT0009: Fehler beim Laden von Assemblys: *.

Fehler beim Laden der Assemblys die Stamm-Assemblyverweise. Weitere Informationen kann in der Buildausgabe bereitgestellt werden.

<a name="MT0010" />

### <a name="mt0010-could-not-parse-the-command-line-arguments-"></a>MT0010: Die Befehlszeilenargumente konnte nicht analysiert werden: *.

Fehler beim Analysieren der Befehlszeilenargumente. Stellen Sie sicher, dass sie richtig sind.

<a name="MT0011" />

### <a name="mt0011--was-built-against-a-more-recent-runtime--than-monotouch-supports"></a>MT0011: * unter einer neueren Laufzeit (*) erstellt wurde, als MonoTouch unterstützt.

Diese Warnung wird in der Regel ausgegeben werden, da das Projekt einen Verweis auf eine Klassenbibliothek enthält, die nicht mit die Xamarin.iOS-BCL erstellt wurde.

Die gleiche Weise, die eine app mit dem .NET 4.0-SDK möglicherweise nicht auf einem System unterstützen nur .NET 2.0 funktioniert eine Bibliothek, die mit .NET 4.0 erstellt wurden, funktionieren nicht in Xamarin.iOS, kann diese API nicht vorhanden in Xamarin.iOS.

Die allgemeine Lösung ist die Bibliothek als eine Xamarin.iOS-Klassenbibliothek zu erstellen. Dies geschieht durch erstellen ein neues Xamarin.iOS-Klassenbibliotheksprojekt und fügen Sie alle Quelldateien hinzu. Wenn Sie nicht über den Quellcode für die Bibliothek verfügen, sollten Sie wenden Sie sich an den Hersteller und anfordern, dass sie eine Xamarin.iOS-kompatible Version ihrer Bibliothek bereitstellen.

<a name="MT0012" />

### <a name="mt0012-incomplete-data-is-provided-to-complete-"></a>MT0012: Unvollständige Daten werden bereitgestellt, um den Abschluss *.

Dieser Fehler wird in der aktuellen Version von Xamarin.iOS nicht mehr gemeldet.

<a name="MT0013" />

### <a name="mt0013-profiling-support-requires-sgen-to-be-enabled-too"></a>MT0013: Profilerstellung erfordert Sgen zu aktiviert werden.

SGen (--Sgen) muss aktiviert sein, wenn ein Profil erstellen (--profilerstellung) aktiviert ist.

<a name="MT0014" />

### <a name="mt0014-the-ios--sdk-does-not-support-building-applications-targeting-"></a>MT0014: IOS * SDK unterstützt keine Erstellung von Anwendungen für *.

Dies kann in folgenden Situationen geschehen:

*  ARMv6 aktiviert ist, und Xcode 4.5 oder höher installiert ist.
*  ARMv7s aktiviert ist, und Xcode 4.4 oder früher installiert ist.

Stellen Sie sicher, dass die installierte Version von Xcode auf die ausgewählten Architekturen unterstützt.

<a name="MT0015" />

### <a name="mt0015-invalid-abi--supported-abis-are-i386-x8664--armv7-armv7llvm-armv7llvmthumb2-armv7s-armv7sllvm-armv7sllvmthumb2-arm64-and-arm64llvm"></a>MT0015: Ungültige ABI: *. Unterstützte ABIs sind: "I386", x86_64, armv7, armv7 + Llvm, armv7 + Llvm + thumb2, armv7s, armv7s + Llvm, armv7s + Llvm + thumb2, arm64 und arm64 + Llvm.

An Mtouch wurde eine ungültige ABI übergeben. Geben Sie eine gültige ABI.

<a name="MT0016" />

### <a name="mt0016-the-option--has-been-deprecated"></a>MT0016: Die Option * ist veraltet.

Die genannten Mtouch-Option ist veraltet und wird ignoriert.

<a name="MT0017" />

### <a name="mt0017-you-should-provide-a-root-assembly"></a>MT0017: Sie sollten eine stammassembly angeben.

Es ist erforderlich, um eine stammassembly (i. d. r. der Hauptausführungsdatei) angeben, wenn eine app erstellen.

<a name="MT0018" />

### <a name="mt0018-unknown-command-line-argument-"></a>MT0018: Unbekanntes Befehlszeilenargument: *.

Das Befehlszeilenargument in der Fehlermeldung genannten erkannt Mtouch nicht.

<a name="MT0019" />

### <a name="mt0019-only-one---loginstallkilllaunchdev-or---launchdebugsim-option-can-be-used"></a>MT0019: Nur ein – [Log | installieren | kill | starten] Dev "oder"--[starten | debug] Sim-Option kann verwendet werden.

Es gibt mehrere Optionen für die Mtouch, die gleichzeitig verwendet werden kann:

-  --logdev
-  --installdev
-  --killdev
-  --launchdev
-  --launchdebug
-  --launchsim

<a name="MT0020" />

### <a name="mt0020-the-valid-options-for--are-"></a>MT0020: Die gültigen Optionen für "\*"sind"\*".

<a name="MT0021" />

### <a name="mt0021-cannot-compile-using-gccg---use-gcc-when-using-the-static-registrar-this-is-the-default-when-compiling-for-device-either-remove-the---use-gcc-flag-or-use-the-dynamic-registrar---registrardynamic"></a>MT0021: Kann nicht kompiliert werden mit Gcc / g++ (--verwenden – Gcc) Verwendung der statischen Registrierungsstelle (Dies ist die Standardeinstellung beim Kompilieren für Geräte). Entfernen Sie entweder--verwenden – Gcc kennzeichnen bzw. zum Verwenden der dynamischen Registrierungsstelle (--Registrierungsstelle: dynamische).

<a name="MT0022" />

### <a name="mt0022-the-options---unsupported--enable-generics-in-registrar-and---registrar-are-not-compatible"></a>MT0022: Der Optionen "– nicht unterstützte--Enable-Generika-in-Registrierungsstelle" und "--Registrierungsstelle" nicht kompatibel sind.

Entfernen Sie beide Optionen `--unsupported--enable-generics-in-registrar` und `--registrar`. Ab Xamarin.iOS 7.2.1 der Standard-Registrierungsstelle unterstützt Generika.

Dieser Fehler wird nicht mehr angezeigt. (das Befehlszeilenargument `--unsupported--enable-generics-in-registrar` von Mtouch entfernt wurde).

<a name="MT0023" />

### <a name="mt0023-application-name-exe-conflicts-with-another-user-assembly"></a>MT0023: Name der Anwendung "* .exe" verursacht einen Konflikt mit einem anderen Benutzer-Assembly.

Ausführbare Datei den Namen der Assembly und den Namen der Anwendung darf nicht den Namen der DLLs in der app übereinstimmen. Ändern Sie den Namen der ausführbaren Datei.

<a name="MT0024" />

### <a name="mt0024-could-not-find-required-file-"></a>MT0024: Die erforderliche Datei nicht gefunden ' *'.

<a name="MT0025" />

### <a name="mt0025-no-sdk-version-was-provided-please-add---sdkxy-to-specify-which-ios-sdk-should-be-used-to-build-your-application"></a>MT0025: Es wurde keine SDK-Version bereitgestellt. Sie fügen `--sdk=X.Y` an die iOS-SDK sollte zum Erstellen der Anwendung verwendet werden.

<a name="MT0026" />

### <a name="mt0026-could-not-parse-the-command-line-argument--"></a>MT0026: Konnte nicht analysiert werden. das Befehlszeilenargument ' *': *

<a name="MT0027" />

### <a name="mt0027-the-options--and--are-not-compatible"></a>MT0027: Der Options'\*'und'\*' sind nicht kompatibel.

<a name="MT0028" />

### <a name="mt0028-cannot-enable-pie--pie-when-targeting-ios-41-or-earlier-please-disable-pie--piefalse-or-set-the-deployment-target-to-at-least-ios-42"></a>MT0028: KREISDIAGRAMM kann nicht aktiviert werden (-Kreis) beim Ziel iOS 4.1 oder früher. Deaktivieren Sie den Kreis (-Kreisdiagramm: False), oder legen Sie das Bereitstellungsziel auf mindestens iOS 4.2

<a name="MT0029" />

### <a name="mt0029-repl---enable-repl-is-only-supported-in-the-simulator---sim"></a>MT0029: REPL (--Enable-Repl) wird nur in der Simulator unterstützt (--Sim).

REPL wird nur unterstützt, wenn Sie für den Simulator erstellen. Dies bedeutet, dass, wenn Sie übergeben `--enable-repl` an Mtouch, müssen Sie auch übergeben `--sim`.

<a name="MT0030" />

### <a name="mt0030-the-executable-name--and-the-app-name--are-different-this-may-prevent-crash-logs-from-getting-symbolicated-properly"></a>MT0030: Name der ausführbaren Datei (\*) und app-Name (\*) sind unterschiedlich, dies kann verhindern, dass absturzprotokolle ordnungsgemäß symbolicated abrufen.

Wenn Xcode symbolicates (Speicheradressen Funktionsnamen und die Datei/Zeile Zahlen übersetzt) während des möglicherweise nicht, wenn die ausführbare Datei und die app unterschiedliche Namen (ohne Erweiterung haben).

Zum Beheben dieser ändern "Anwendungsname" des Projekts Optionen für Build/iOS-Anwendungen oder Änderung "Assemblyname" des Projekts Build/Ausgabeoptionen.

<a name="MT0031" />

### <a name="mt0031-the-command-line-arguments---enable-background-fetch-and---launch-for-background-fetch-require---launchsim-too"></a>MT0031: Die Befehlszeilenargumente "– hintergrundabruf aktivieren" und "--Start für hintergrundabruf" erforderlich "– Launchsim" zu.

<a name="MT0032" />

### <a name="mt0032-the-option---debugtrack-is-ignored-unless---debug-is-also-specified"></a>MT0032: Die Option "--Debugtrack" wird ignoriert, es sei denn, "– debug" ist ebenfalls angegeben.

<a name="MT0033" />

### <a name="mt0033-a-xamarinios-project-must-reference-either-monotouchdll-or-xamariniosdll"></a>MT0033: Ein Xamarin.iOS-Projekt muss entweder "monotouch.dll" oder "Xamarin.iOS.dll verweisen.

<a name="MT0034" />

### <a name="mt0034-cannot-include-both-monotouchdll-and-xamariniosdll-in-the-same-xamarinios-project----is-referenced-explicitly-while--is-referenced-by-"></a>MT0034: Kann nicht sowohl "monotouch.dll" und "Xamarin.iOS.dll" in der gleichen Xamarin.iOS-Projekt - einschließen '\*"ist explizit verwiesen wird, während er sich"\*' verweist auf "*".

<!-- MT0035 unused -->

<a name="MT0036" />

### <a name="mt0036-cannot-launch-a--simulator-for-a--app-please-enable-the-correct-architectures-in-your-projects-ios-build-options-advanced-page"></a>MT0036: Kann nicht gestartet werden eine *-Simulator für eine * app. Aktivieren Sie die richtige Architecture(s) in Ihres Projekts, iOS Build-Optionen (Seite "Erweitert").

<a name="MT0037" />

### <a name="mt0037-monotouchdll-is-not-64-bit-compatible-either-reference-xamariniosdll-or-do-not-build-for-a-64-bit-architecture-arm64-andor-x8664"></a>MT0037: monotouch.dll ist nicht 64-Bit-kompatibel. Xamarin.iOS.dll verweisen oder nicht für eine 64-Bit-Architektur (ARM64 und/oder x86_64) erstellen.

<a name="MT0038" />

### <a name="mt0038-the-old-registrars---registraroldstaticolddynamic-are-not-supported-when-referencing-xamariniosdll"></a>MT0038: Der alte Registrierungsstellen (--Registrierungsstelle: Oldstatic | Olddynamic) werden nicht unterstützt, wenn Sie Xamarin.iOS.dll verweisen.

<a name="MT0039" />

### <a name="mt0039-applications-targeting-armv6-cannot-reference-xamariniosdll"></a>MT0039: Anwendungen für ARMv6 Xamarin.iOS.dll nicht verwiesen werden.

<a name="MT0040" />

### <a name="mt0040-could-not-find-the-assembly--referenced-by-"></a>MT0040: Die Assembly wurde nicht gefunden "\*", auf die verwiesen wird durch"\*".

<a name="MT0041" />

### <a name="mt0041-cannot-reference-both-monotouchdll-and-xamariniosdll"></a>MT0041: Kann nicht sowohl "monotouch.dll" und "Xamarin.iOS.dll" verweisen.

<a name="MT0042" />

### <a name="mt0042-no-reference-to-either-monotouchdll-or-xamariniosdll-was-found-a-reference-to-monotouchdll-will-be-added"></a>MT0042: Es wurde kein Verweis auf monotouch.dll oder Xamarin.iOS.dll gefunden. Ein Verweis auf monotouch.dll wird hinzugefügt werden.

<a name="MT0043" />

### <a name="mt0043-the-boehm-garbage-collector-is-currently-not-supported-when-referencing-xamariniosdll-the-sgen-garbage-collector-has-been-selected-instead"></a>MT0043: Boehm Garbagecollector wird derzeit nicht unterstützt, wenn Sie auf "Xamarin.iOS.dll" verweisen. Stattdessen wurde der SGen Garbagecollector ausgewählt.

Nur der SGen Garbagecollector wird mit Unified-Projekten unterstützt. Stellen Sie sicher, es gibt keine weitere Mtouch-Flags Boehm als der Garbage Collector angeben.

<a name="MT0044" />

### <a name="mt0044---listsim-is-only-supported-with-xcode-60-or-later"></a>MT0044:--Listsim wird nur mit Xcode 6.0 oder höher unterstützt.

Installieren Sie eine neuere Version von Xcode.

<a name="MT0045" />

### <a name="mt0045---extension-is-only-supported-when-using-the-ios-80-or-later-sdk"></a>MT0045:--Erweiterung wird nur unterstützt, wenn die iOS 8.0 (oder höher)-SDK.

<!-- MT0046 is not reported anymore -->

<a name="MT0047" />

### <a name="mt0047-the-minimum-deployment-target-for-unified-applications-is-511-the-current-deployment-target-is--please-select-a-newer-deployment-target-in-your-projects-ios-application-options"></a>MT0047: Die mindestbereitstellungsziels für einheitliche Anwendungen 5.1.1 ist die aktuelles Bereitstellungsziel ist "*". Wählen Sie ein Bereitstellungsziel aus neueren Ihres Projekts, iOS Anwendungsoptionen.

<!-- MT0048 is not reported anymore -->

<a name="MT0049" />

### <a name="mt0049-framework-is-supported-only-if-deployment-target-is-80-or-later--features-might-not-work-correctly"></a>MT0049: *.framework wird nur unterstützt, wenn Bereitstellungsziel 8.0 oder höher ausgeführt wird. * die Features funktionieren möglicherweise nicht ordnungsgemäß.

Des angegebenen Frameworks ist in der iOS-Version, auf die das Bereitstellungsziel verweist, nicht unterstützt. Aktualisieren Sie das Bereitstellungsziel auf eine neuere iOS-Version oder entfernen Sie die Verwendung des angegebenen Frameworks aus der app.

<!-- MT0050 is not reported anymore -->

<a name="MT0051" />

### <a name="mt0051-xamarinios--requires-xcode-50-or-later-the-current-xcode-version-found-in--is-"></a>MT0051: Xamarin.iOS * erfordert Xcode 5.0 oder höher. Die aktuelle Version von Xcode (finden Sie unter *) ist *.

Installieren Sie ein neuer Xcode.

<a name="MT0052" />

### <a name="mt0052-no-command-specified"></a>MT0052: Kein Befehl angegeben.

Es wurde keine Aktion für das Mtouch angegeben.

<!-- 0053 is used by mmp -->

<a name="MT0054" />

### <a name="mt0054-unable-to-canonicalize-the-path--"></a>MT0054: Kann nicht den Pfad zu kanonisierende ' *': *

Dies ist ein interner Fehler. Wenn dieser Fehler angezeigt wird, senden Sie einen Fehlerbericht [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT0055" />

### <a name="mt0055-the-xcode-path--does-not-exist"></a>MT0055: Der Xcode-Pfad ' *' ist nicht vorhanden.

Der Xcode-Pfad mit übergeben `--sdkroot` ist nicht vorhanden. Geben Sie einen gültigen Pfad an.

<a name="MT0056" />

### <a name="mt0056-cannot-find-xcode-in-the-default-location-applicationsxcodeapp-please-install-xcode-or-pass-a-custom-path-using---sdkroot-path"></a>MT0056: Xcode kann nicht gefunden werden, die am Standardort (/ /Anwendungen/Xcode.app). Installieren Sie Xcode, oder übergeben Sie einen benutzerdefinierten Pfad, die mit--Sdkroot <path>.

<a name="MT0057" />

### <a name="mt0057-cannot-determine-the-path-to-xcodeapp-from-the-sdk-root--please-specify-the-full-path-to-the-xcodeapp-bundle"></a>MT0057: Kann nicht bestimmen Sie den Pfad zum Xcode.app, aus dem Sdk-Stamm ' *'. Geben Sie den vollständigen Pfad zum Paket Xcode.app an.

Der Pfad mit übergeben `--sdkroot` gibt keine gültige Xcode-app. Geben Sie einen Pfad zu einem Xcode-app an.

<a name="MT0058" />

### <a name="mt0058-the-xcodeapp--is-invalid-the-file--does-not-exist"></a>MT0058: Die Xcode.app "\*' ist ungültig (die Datei"\*' ist nicht vorhanden).

Der Pfad mit übergeben `--sdkroot` gibt keine gültige Xcode-app. Geben Sie einen Pfad zu einem Xcode-app an.

<a name="MT0059" />

### <a name="mt0059-could-not-find-the-currently-selected-xcode-on-the-system-"></a>MT0059: Die aktuell ausgewählte Xcode auf dem System wurde nicht gefunden: *

<a name="MT0060" />

### <a name="mt0060-could-not-find-the-currently-selected-xcode-on-the-system-xcode-select---print-path-returned--but-that-directory-does-not-exist"></a>MT0060: Die aktuell ausgewählte Xcode wurde auf dem System nicht gefunden werden. "Xcode-Select--Print-Path" zurückgegebenen "*", aber dieses Verzeichnis ist nicht vorhanden.

<a name="MT0061" />

### <a name="mt0061-no-xcodeapp-specified-using---sdkroot-using-the-system-xcode-as-reported-by-xcode-select---print-path-"></a>MT0061: Keine Xcode.app angegeben (mithilfe von--Sdkroot), das System Xcode verwenden, wie von "Xcode-Select--Print-Path" gemeldet: *

Dies ist eine informative Warnung, erläutern die Xcode verwendet werden, da keine angegeben wurde.

<a name="MT0062" />

### <a name="mt0062-no-xcodeapp-specified-using---sdkroot-or-xcode-select---print-path-using-the-default-xcode-instead-"></a>MT0062: Keine Xcode.app angegeben (mit--Sdkroot "oder"Xcode-Select--Print-Path"), verwenden Sie stattdessen die Standard-Xcode: *

Dies ist eine informative Warnung, erläutern die Xcode verwendet werden, da keine angegeben wurde.

<a name="MT0063" />

### <a name="mt0063-cannot-find-the-executable-in-the-extension--no-cfbundleexecutable-entry-in-its-infoplist"></a>MT0063: Die ausführbare Datei kann nicht gefunden werden, in der Erweiterung * (kein CFBundleExecutable-Eintrag in der "Info.plist")

Jede Datei "Info.plist" muss eine ausführbare Datei (mit den CFBundleExecutable-Eintrag), verfügen, aber ein Eintrag automatisch während des Buildvorgangs generiert werden soll.

In der Regel gibt einen Fehler in Xamarin.iOS. Melden Sie bitte einen Fehlerbericht an [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) zu einem Testfall.

<a name="MT0064" />

### <a name="mt0064-xamarinios-only-supports-embedded-frameworks-with-unified-projects"></a>MT0064: Xamarin.iOS unterstützt nur eingebettete Frameworks mit Unified-Projekten.

Xamarin.iOS unterstützt nur eingebettete Frameworks bei Verwendung der Unified API. Aktualisieren Sie Ihr Projekt zum Verwenden der Unified API.

<a name="MT0065" />

### <a name="mt0065-xamarinios-only-supports-embedded-frameworks-when-deployment-target-is-at-least-80-current-deployment-target--embedded-frameworks-"></a>MT0065: Xamarin.iOS unterstützt nur eingebettete Frameworks, wenn das Ziel der Bereitstellung mindestens 8.0 ist (Aktuelles Bereitstellungsziel: * eingebettete Frameworks: *)

Xamarin.iOS unterstützt nur eingebettete Frameworks, wenn das Bereitstellungsziel mindestens 8.0 ist, (da frühere Versionen von iOS unterstützt keine eingebettete Frameworks).

Aktualisieren Sie das Bereitstellungsziel in die "Info.plist" des Projekts auf 8.0 oder höher aus.

<a name="MT0066" />

### <a name="mt0066-invalid-build-registrar-assembly-"></a>MT0066: Ungültiges Build-Registrierungsstelle Assembly: *

In der Regel gibt einen Fehler in Xamarin.iOS. Melden Sie bitte einen Fehlerbericht an [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) zu einem Testfall.

<a name="MT0067" />

### <a name="mt0067-invalid-registrar-"></a>MT0067: Ungültige Registrierungsstelle: *

In der Regel gibt einen Fehler in Xamarin.iOS. Melden Sie bitte einen Fehlerbericht an [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) zu einem Testfall.

<a name="MT0068" />

### <a name="mt0068-invalid-value-for-target-framework-"></a>MT0068: Ungültiger Wert für das Zielframework: *.

Ein ungültiges Ziel-Framework übergeben wurde, mithilfe von--Zielframework Argument. Geben Sie ein gültiges Ziel-Framework.

<a name="MT0069" />

<!--### MT0069: currently unused -->

<a name="MT0070" />

### <a name="mt0070-invalid-target-framework--valid-target-frameworks-are-"></a>MT0070: Ungültiges Ziel-Framework: *. Gültiges Ziel-Frameworks sind: *.

Ein ungültiges Ziel-Framework übergeben wurde, mithilfe von--Zielframework Argument. Geben Sie ein gültiges Ziel-Framework.

<a name="MT0071" />

### <a name="mt0071-unknown-platform--this-usually-indicates-a-bug-in-xamarinios-please-file-a-bug-report-at-httpbugzillaxamarincom-with-a-test-case"></a>MT0071: Unbekannte Plattform: *. In der Regel gibt einen Fehler in Xamarin.iOS. Melden Sie bitte einen Fehlerbericht an http://bugzilla.xamarin.com zu einem Testfall.

In der Regel gibt einen Fehler in Xamarin.iOS. Melden Sie bitte einen Fehlerbericht an [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) zu einem Testfall.

<a name="MT0072" />

### <a name="mt0072-extensions-are-not-supported-for-the-platform-"></a>MT0072: Erweiterungen werden nicht unterstützt, für die Plattform ' *'.

In der Regel gibt einen Fehler in Xamarin.iOS. Melden Sie bitte einen Fehlerbericht an [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) zu einem Testfall.

<a name="MT0073" />

### <a name="mt0073-xamarinios--does-not-support-a-deployment-target-of--the-minimum-is--please-select-a-newer-deployment-target-in-your-projects-infoplist"></a>MT0073: Xamarin.iOS * unterstützt kein Bereitstellungsziel * (das Minimum ist *). Wählen Sie eine neuere Bereitstellungsziel in Ihrem Projekt die Datei "Info.plist" aus.

Das minimale Bereitstellungsziel ist in der Fehlermeldung angegeben. Wählen Sie ein neuer Bereitstellungsziel das Projekt die Datei "Info.plist".

Wenn das Ziel der Bereitstellung aktualisieren nicht möglich ist, verwenden Sie eine ältere Version von Xamarin.iOS.

<a name="MT0074" />

### <a name="mt0074-xamarinios--does-not-support-a-minimum-deployment-target-of--the-maximum-is--please-select-an-older-deployment-target-in-your-projects-infoplist-or-upgrade-to-a-newer-version-of-xamarinios"></a>MT0074: Xamarin.iOS * unterstützt keine minimale Bereitstellungsziel * (maximal: *). Bitte wählen Sie eine ältere Bereitstellungsziel in Ihrem Projekt die Datei "Info.plist" aus, oder ein upgrade auf eine neuere Version von Xamarin.iOS.

Xamarin.iOS unterstützt nicht das Festlegen des Bereitstellungsziels für die minimale auf einer höheren Version als die Version, die, der für diese bestimmte Version von Xamarin.iOS erstellt wurde.

Wählen Sie eine ältere mindestbereitstellungsziels in das Projekt die Datei "Info.plist", oder ein upgrade auf eine neuere Version von Xamarin.iOS.

<a name="MT0075" />

### <a name="mt0075-invalid-architecture--for--projects-valid-architectures-are-"></a>MT0075: Ungültige Architektur ' *' für * Projekte. Gültige Architekturen sind: *

Es wurde eine ungültige Architektur angegeben. Stellen Sie sicher, dass Architektur gültig ist.

<a name="MT0076" />

### <a name="mt0075-no-architecture-specified-using-the---abi-argument-an-architecture-is-required-for--projects"></a>MT0075: Keine Architektur angegeben (mit dem--Abi-Argument). Eine Architektur ist erforderlich, damit * Projekte.

In der Regel gibt einen Fehler in Xamarin.iOS. Melden Sie bitte einen Fehlerbericht an [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) zu einem Testfall.

<a name="MT0077" />

### <a name="mt0076-watchos-projects-must-be-extensions"></a>MT0076: WatchOS-Projekte müssen es sich um Erweiterungen sein.

In der Regel gibt einen Fehler in Xamarin.iOS. Melden Sie bitte einen Fehlerbericht an [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) zu einem Testfall.

<a name="MT0078" />

### <a name="mt0077-incremental-builds-are-enabled-with-a-deployment-target--80-currently--this-is-not-supported-the-resulting-application-will-not-launch-on-ios-9-so-the-deployment-target-will-be-set-to-80"></a>MT0077: Inkrementelle Builds sind aktiviert, mit einer Bereitstellung Target < 8.0 (derzeit *). Dies wird nicht unterstützt (die Anwendung wird nicht gestartet, auf iOS 9), damit das Bereitstellungsziel auf 8.0 festgelegt wird.

Dies ist eine Warnung darüber informiert, dass das Bereitstellungsziel auf 8.0 für diesen Build festgelegt wurde, damit inkrementelle builds ordnungsgemäß arbeiten.

Inkrementelle Builds werden nur unterstützt, wenn das Bereitstellungsziel mindestens 8.0 ist, (da die resultierende Anwendung nicht auf iOS 9 andernfalls zu starten).

<a name="MT0079" />

### <a name="mt0078-the-recommended-xcode-version-for-xamarinios--is-xcode--or-later-the-current-xcode-version-found-in--is-"></a>MT0078: Die empfohlene Xcode-Version für Xamarin.iOS * ist Xcode * oder höher. Die aktuelle Version von Xcode (finden Sie unter *) ist *.

Dies ist eine Warnung darüber informiert, dass die aktuelle Version von Xcode nicht die empfohlene Version von Xcode für diese Version von Xamarin.iOS ist.

Aktualisieren Sie Xcode, um das optimale Verhalten sicherzustellen.

<a name="MT0080" />

### <a name="mt0080-disabling-newrefcount---new-refcountfalse-is-deprecated"></a>MT0080: Deaktivieren NewRefCount ","--new-Refcount:false, ist veraltet.

Dies ist eine Warnung darüber informiert, die die Anforderung so deaktivieren Sie die neue Verweisanzahl (– neu – Refcount:false) wurde ignoriert.

Die neue Refcount-Funktion ist jetzt für alle Projekte erforderlich, und es ist daher nicht möglich, die nicht mehr deaktivieren.

<a name="MT0081" />

### <a name="mt0081-the-command-line-argument---download-crash-report-also-requires---download-crash-report-to"></a>MT0081: Das Befehlszeilenargument – Download Absturzbericht erfordert auch – Download-Absturz-Meldung an.

<a name="MT0082" />

### <a name="mt0082-repl---enable-repl-is-only-supported-when-linking-is-not-used---nolink"></a>MT0082: REPL (--Enable-Repl) wird nur unterstützt, wenn die Verknüpfung nicht verwendet wird (--Nolink).

<a name="MT0083" />

### <a name="mt0083-asm-only-bitcode-is-not-supported-on-watchos-use-either---bitcodemarker-or---bitcodefull"></a>MT0083: Nur-ASM-Bitcode ist für WatchOS nicht unterstützt. Verwenden Sie entweder – Bitcode: ' Marker ' oder--Bitcode: vollständige.

<a name="MT0084" />

### <a name="mt0084-bitcode-is-not-supported-in-the-simulator-do-not-pass---bitcode-when-building-for-the-simulator"></a>MT0084: Bitcode ist in der Simulator nicht unterstützt. Übergeben Sie – Bitcode nicht, wenn Sie für den Simulator zu erstellen.

<a name="MT0085" />

### <a name="mt0085-no-reference-to--was-found-it-will-be-added-automatically"></a>MT0085: Kein Verweis auf ' *' wurde gefunden. Es wird automatisch hinzugefügt.

<a name="MT0086" />

### <a name="mt0086-a-target-framework---target-framework-must-be-specified-when-building-for-tvos-or-watchos"></a>MT0086: Ein Zielframework (--Zielframework) muss angegeben werden, bei der Entwicklung für TVOS oder WatchOS.

In der Regel gibt einen Fehler in Xamarin.iOS. Melden Sie bitte einen Fehlerbericht an [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) zu einem Testfall.

<a name="MT0087" />

### <a name="mt0087-incremental-builds---fastdev-is-not-supported-with-the-boehm-gc-incremental-builds-will-be-disabled"></a>MT0087: Inkrementelle Builds (--Fastdev) wird mit der Boehm Garbage Collector nicht unterstützt. Inkrementelle Builds werden deaktiviert.

<a name="MT0088" />

### <a name="mt0088-the-gc-must-be-in-cooperative-mode-for-watchos-apps-please-remove-the---coopfalse-argument-to-mtouch"></a>MT0088: Der Garbage Collector muss im kooperativen Modus für WatchOS-apps. Entfernen Sie Mtouch-Argument Coop: "false".

<a name="MT0089" />

### <a name="mt0089-the-option--cannot-take-the-value--when-cooperative-mode-is-enabled-for-the-gc"></a>MT0089: Die Option "\*"kann nicht übernommen werden den Wert"\*" Wenn für die Freispeichersammlung kooperativen Modus aktiviert ist.

<a name="MT0091" />

### <a name="mt0091-this-version-of-xamarinios-requires-the--sdk-shipped-with-xcode--either-upgrade-xcode-to-get-the-required-header-files-or-set-the-managed-linker-behaviour-to-link-framework-sdks-only-to-try-to-avoid-the-new-apis"></a>MT0091: Diese Version von Xamarin.iOS erfordert die * SDK (Xcode im Lieferumfang *). Entweder ein upgrade von Xcode werden die erforderlichen Header-Dateien abgerufen oder die verwaltete linkerverhalten auf Link nur Framework-SDKs (zum Testen, um die neuen APIs zu vermeiden).

Xamarin.iOS erfordert die Headerdateien, von der SDK-Version, die in der Fehlermeldung zum Erstellen der Anwendung angegeben. Die empfohlene Methode zum Beheben dieses Fehlers ist Xcode rufen Sie das erforderliche SDK zu aktualisieren, dazu gehören alle erforderlichen Header-Dateien. Wenn Sie mehrere Versionen von Xcode installiert haben, oder eine Xcode in einem nicht standardmäßigen Speicherort verwenden möchten, stellen Sie sicher, dass den richtige Xcode-Speicherort in der IDE Einstellungen.

Potenziell, ist die alternative Lösung zum Aktivieren des verwalteten Linkers. Dies entfernt nicht verwendeten API, beispielsweise, in den meisten Fällen, die neue API, in denen die Headerdateien fehlt (oder ist unvollständig) sind. Jedoch bietet dies nicht funktionieren, wenn Ihr Projekt API verwendet, die in ein neueres SDK als Ihre Xcode eingeführt wurde.

Eine letzte strohhalm Lösung wäre, die eine ältere Version von Xamarin.iOS verwenden, ist erforderlich, die das SDK Ihrem Projekt unterstützt.

<!-- MT0092 used by mlaunch -->

<a name="MT0093" />

### <a name="mt0093-could-not-find-mlaunch"></a>MT0093: "Mlaunch" wurde nicht gefunden werden.

<!-- MT0094 is not reported anymore -->

<a name="MT0095" />

### <a name="mt0095-aot-files-could-not-be-copied-to-the-destination-directory-dest-error"></a>MT0095: AOT-Dateien konnte nicht kopiert werden, in das Zielverzeichnis {Dest}: {Error}

<a name="MT0096" />

### <a name="mt0096-no-reference-to-xamariniosdll-was-found"></a>MT0096: Es wurde kein Verweis auf Xamarin.iOS.dll gefunden.

<!-- MT0097: used by mmp -->
<!-- MT0098: used by mmp -->

<a name="MT0099" />

### <a name="mt0099-internal-error--please-file-a-bug-report-with-a-test-case-httpbugzillaxamarincom"></a>MT0099: Interner Fehler *. Melden Sie bitte einen Fehlerbericht zu einem Testfall (http://bugzilla.xamarin.com).

Diese Fehlermeldung wird gemeldet, wenn in Xamarin.iOS interner konsistenzprüfung ein Fehler auftritt.

Gibt einen Fehler in Xamarin.iOS. Melden Sie bitte einen Fehlerbericht an [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) zu einem Testfall.

<a name="MT0100" />

### <a name="mt0100-invalid-assembly-build-target--please-file-a-bug-report-with-a-test-case-httpbugzillaxamarincom"></a>MT0100: Ungültiger Assembler-Buildziel: ' *'. Melden Sie bitte einen Fehlerbericht zu einem Testfall (http://bugzilla.xamarin.com).

Diese Fehlermeldung wird gemeldet, wenn in Xamarin.iOS interner konsistenzprüfung ein Fehler auftritt.

Dies ist immer ein Fehler in Xamarin.iOS. Melden Sie bitte einen Fehlerbericht an [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) zu einem Testfall.

<a name="MT0101" />

### <a name="mt0101-the-assembly--is-specified-multiple-times-in---assembly-build-target-arguments"></a>MT0101: Die Assembly ' *' wurde mehrmals in – Assembly-Build-Ziel-Argumente angegeben.

Die Assembly, die in der Fehlermeldung genannten wird mehrere Male in – Assembly-Build-Ziel-Argumente angegeben. Stellen Sie sicher, dass jede Assembly nur einmal aufgeführt wird.

<a name="MT0102" />

### <a name="mt0102-the-assemblies--and--have-the-same-target-name--but-different-targets--and-"></a>MT0102: Der Assemblys\*'und'\*"haben den gleichen Target-Namen ("\*"), aber unterschiedliche Ziele ("\*"und" * ").

Die in der Fehlermeldung genannten Assemblys haben in Konflikt stehende Build-Ziele.

Zum Beispiel:

    --assembly-build-target:Assembly1.dll=framework=MyBinary --assembly-build-target:Assembly2.dll=dynamiclibrary=MyBinary

In diesem Beispiel wird versucht, zum Erstellen einer dynamischen Bibliothek und ein Framework mit der gleichen Marke (`MyBinary`).

<a name="MT0103" />

### <a name="mt0103-the-static-object--contains-more-than-one-assembly--but-each-static-object-must-correspond-with-exactly-one-assembly"></a>MT0103: Das statische Objekt "\*' enthält mehr als eine Assembly ("\*"), aber jedes statische Objekt muss mit genau einem Assembly entsprechen.

Die Assemblys, die in der Fehlermeldung genannten, werden alle auf ein einzelnes statische Objekt kompiliert. Dies ist nicht zulässig, muss jede Assembly kompiliert werden, an ein anderes statische Objekt.

Zum Beispiel:

    --assembly-build-target:Assembly1.dll=staticobject=MyBinary --assembly-build-target:Assembly2.dll=staticobject=MyBinary

Dieses Beispiel wird versucht, ein statisches Objekt zu erstellen (`MyBinary`) bestehend aus zwei Assemblys (`Assembly1.dll` und `Assembly2.dll`), dies ist nicht zulässig.

<a name="MT0105" />

### <a name="mt0105-no-assembly-build-target-was-specified-for-"></a>MT0105: Keine Assembly-Build-Ziel wurde angegeben, für die ' *'.

Wenn die Assembly angeben erstellen-Ziel verwendet `--assembly-build-target`, jede Assembly in der app muss ein Buildziel zugewiesen haben.

Dieser Fehler wird gemeldet, wenn die Assembly, die in der Fehlermeldung genannten keine Assembly mit dem Ziel zugewiesen zu erstellen.

Finden Sie in der Dokumentation zu `--assembly-build-target` für Weitere Informationen zu erhalten.

<a name="MT0106" />

### <a name="mt0106-the-assembly-build-target-name--is-invalid-the-character--is-not-allowed"></a>MT0106: Der Baugruppe Zielname "\*' ist ungültig: das Zeichen '\*' ist nicht zulässig.

Der Assemblyname des Build-Ziel muss ein gültiger Dateiname sein.

Diese Werte werden z. B. diesen Fehler auslösen:

    --assembly-build-target:Assembly1.dll=staticobject=my/path.o

Da `my/path.o` ist kein gültiger Dateiname aufgrund der Verzeichnistrennzeichen.

<a name="MT0107" />

### <a name="mt0107-the-assemblies--have-different-custom-llvm-optimizations--which-is-not-allowed-when-they-are-all-compiled-to-a-single-binary"></a>MT0107: Der Assemblys\*' haben unterschiedliche benutzerdefinierte LLVM-Optimierungen (\*), dies ist nicht zulässig, wenn sie alle in einer einzelnen Binärdatei kompiliert werden.

<a name="MT0108" />

### <a name="mt0108-the-assembly-build-target--did-not-match-any-assemblies"></a>MT0108: Die Assembly Buildziel ' *' stimmt nicht mit den Assemblys, auf.

<a name="MT0109" />

### <a name="mt0109-the-assembly-0-was-loaded-from-a-different-path-than-the-provided-path-provided-path-1-actual-path-2"></a>MT0109: Die Assembly "{0}' wurde aus einem anderen Pfad als der angegebene Pfad (angegeben Pfad: {1}, tatsächliche Pfad: {2}).

Dies ist eine Warnung gibt an, dass eine Assembly verwiesen wird, von der Anwendung geladen von einem anderen Speicherort als angefordert wurde.

Dies bedeutet möglicherweise, dass die app mehrere Assemblys mit demselben Namen, aber von verschiedenen Standorten, was zu unerwarteten Ergebnissen führen können (nur die erste Assembly die verwendet werden) verweist.

<a name="MT0110" />

### <a name="mt0110-incremental-builds-have-been-disabled-because-this-version-of-xamarinios-does-not-support-incremental-builds-in-projects-that-include-third-party-binding-libraries-and-that-compiles-to-bitcode"></a>MT0110: Inkrementelle Builds wurde deaktiviert, da es sich bei dieser Version von Xamarin.iOS nicht unterstützt inkrementelle Builds in Projekten, die Bindung von Drittanbieter-Bibliotheken enthalten, und, die in Bitcode kompiliert.

Inkrementelle Builds wurde deaktiviert, da es sich bei dieser Version von Xamarin.iOS nicht unterstützt inkrementelle Builds in Projekten, die Bindung von Drittanbieter-Bibliotheken enthalten, und, die in Bitcode (TvOS und WatchOS-Projekte) kompiliert.

Es ist keine Aktion Ihrerseits erforderlich, diese Meldung dient nur Informationszwecken.

Weitere Informationen finden Sie unter Fehler #[51710](https://bugzilla.xamarin.com/show_bug.cgi?id=51710).

Diese Warnung wird nicht mehr gemeldet.

<a name="MT0111" />

### <a name="mt0111-bitcode-has-been-enabled-because-this-version-of-xamarinios-does-not-support-building-watchos-projects-using-llvm-without-enabling-bitcode"></a>MT0111: Bitcode wurde aktiviert, weil es sich bei dieser Version von Xamarin.iOS unterstützt nicht erstellen von WatchOS-Projekte mit LLVM ohne Aktivierung von Bitcode.

Bitcode wurde automatisch aktiviert, weil es sich bei dieser Version von Xamarin.iOS unterstützt nicht erstellen von WatchOS-Projekte mit LLVM ohne Aktivierung von Bitcode.

Es ist keine Aktion Ihrerseits erforderlich, diese Meldung dient nur Informationszwecken.

Weitere Informationen finden Sie unter Fehler #[51634](https://bugzilla.xamarin.com/show_bug.cgi?id=51634).

<a name="MT0112" />

### <a name="mt0112-native-code-sharing-has-been-disabled-because-"></a>MT0112: Freigeben von systemeigenen Code wurde deaktiviert, weil *

Es gibt mehrere Gründe, die Freigabe von Code deaktiviert werden kann:

* Da der Container-app Bereitstellungsziel vor iOS 8.0 oder höher ist (es hat *)).

Freigeben von systemeigenen Code erfordert iOS 8.0 oder höher da Freigeben von systemeigenen Code mit Benutzer-Frameworks implementiert wird die wurde mit iOS 8.0 oder höher eingeführt.

* Da die Container-app I18N-Assemblys (*) enthält.

Freigeben von systemeigenem Code ist derzeit nicht unterstützt, wenn die Container-app I18N-Assembly enthält.

* Da die Container-app benutzerdefinierte XML-Definitionen für den verwalteten Linker (*) verfügt.

Freigeben von systemeigenen Code erfordert, wird für Projekte, verwenden benutzerdefinierte XML-Definitionen für den verwalteten Linker, nicht unterstützt.

<a name="MT0113" />

### <a name="mt0113-native-code-sharing-has-been-disabled-for-the-extension--because-"></a>MT0113: Freigeben von systemeigenen Code wurde deaktiviert für die Erweiterung "*" da *.

* Da der Container-app die Bitcode Optionen unterscheiden (\*) und die Erweiterung (\*).

  Freigeben von systemeigenen Code erfordert, dass die Optionen Bitcode zwischen den Projekten übereinstimmen, die die gemeinsamen Code nutzen.

* Da das – Assembly-Build-Ziel Optionen unterscheiden sich zwischen der Container-app (\*) und die Erweiterung (\*).

  Freigeben von systemeigenen Code erfordert, dass das – Assembly-Build-Ziel Optionen unterscheiden sich zwischen den Projekten, die die gemeinsamen Code nutzen.

  Diese Bedingung kann auftreten, wenn inkrementelle builds nicht aktiviert oder deaktiviert sind in allen Projekten.

* Da die I18N-Assemblys der Container-app identisch sind (\*) und die Erweiterung (\*).

  Freigeben von systemeigenem Code ist derzeit nicht für Erweiterungen unterstützt, die I18N-Assemblys enthalten.

* Da die Argumente, die der AOT-Compiler der Container-app identisch sind (\*) und die Erweiterung (\*).

  Freigeben von systemeigenen Code erfordert, dass die Argumente, die der AOT-Compiler nicht zwischen Projekten voneinander unterscheiden, die die gemeinsamen Code nutzen.

* Da sich die anderen Argumente für die AOT-Compiler zwischen der Container-app unterscheiden (\*) und die Erweiterung (\*).

  Freigeben von systemeigenen Code erfordert, dass die Argumente, die der AOT-Compiler nicht zwischen Projekten voneinander unterscheiden, die die gemeinsamen Code nutzen.

  Diese Bedingung tritt auf, wenn der "alle 32-Bit-Gleitkommaoperationen als 64-Bit-Gleitkommaoperationen ausführen" zwischen den Projekten unterscheiden.

* Da LLVM nicht aktiviert oder werden, in der Container-app deaktiviert (\*) und die Erweiterung (\*).

  Freigeben von systemeigenen Code erfordert, dass der LLVM entweder aktiviert oder deaktiviert für alle Projekte, die die gemeinsamen Code nutzen.

* Da sich der verwaltete linkereinstellungen zwischen der Container-app unterscheiden (\*) und die Erweiterung (\*).

  Freigeben von systemeigenen Code erfordert, dass es sich bei die verwalteten Linker-Einstellungen für alle Projekte identisch sind, die die gemeinsamen Code nutzen.

* Da sich die übersprungenen Assemblys für den verwalteten Linker zwischen der Container-app unterscheiden (\*) und die Erweiterung (\*).

  Freigeben von systemeigenen Code erfordert, dass es sich bei die verwalteten Linker-Einstellungen für alle Projekte identisch sind, die die gemeinsamen Code nutzen.

* Da die Erweiterung eine benutzerdefinierte XML-Definitionen für den verwalteten Linker (*) aufweist.

  Freigeben von systemeigenen Code erfordert, wird für Projekte, verwenden benutzerdefinierte XML-Definitionen für den verwalteten Linker, nicht unterstützt.

* Da die Container-app nicht für die ABI erstellt * (während die Erweiterung für diese ABI erstellt).

  Freigeben von systemeigenen Code erfordert, dass die Container-app für die alle Architekturen erstellt für jede app-Erweiterung erstellt.

  Zum Beispiel: Diese Bedingung tritt auf, wenn eine Erweiterung für ARM64 + ARMv7 erstellt, aber die Container-app nur für ARM64 erstellt.

* Da es sich bei der Erstellung der Container-app für die ABI ist \*, dies ist nicht kompatibel mit der Erweiterung ABI (\*).

  Freigeben von systemeigenen Code erfordert, dass alle Projekte für genau dieselbe API erstellen.

  Zum Beispiel: Diese Bedingung tritt auf, wenn eine Erweiterung für ARMv7 + Llvm + thumb2 erstellt, aber die Container-app, die nur für ARMv7 + Llvm erstellt werden.

* Da die Container-app auf die Assembly verweist "\*'von'\*", während die Erweiterung verweist, eine andere Version auf "*".

  Freigeben von systemeigenen Code erfordert, dass alle Projekte, die die gemeinsamen Code nutzen die gleichen Versionen für alle Assemblys.

<!-- MT0114: used by mmp -->

<a name="MT0115" />

### <a name="mt0115-it-is-recommended-to-reference-dynamic-symbols-using-code---dynamic-symbol-modecode-when-bitcode-is-enabled"></a>MT0115: Es wird empfohlen, dynamische Symbole, die mithilfe von Code zu verweisen (– dynamische Symbolmodus = Code) Wenn Bitcode aktiviert ist.

Xamarin.iOS-Projekte werden oft native Symbole dynamisch, verweisen die bedeutet, dass der native Linker diese native Symbole während des systemeigenen Verknüpfungsvorgangs ist entfernen kann, da es sich bei der native Linker nicht sieht, dass diese Symbole verwendet werden.

In der Regel fragt Xamarin.iOS den nativen Linker, behalten Sie diese Symbole (mit der `-u symbol` Linker-Flags), aber beim Kompilieren für Bitcode des nativen Linkers akzeptiert keine der `-u` Flag.

Xamarin.iOS verfügt über eine alternative Lösung implementiert: generieren wir zusätzliche systemeigenen Code, der diese Symbole verweist, und daher der native Linker sehen, dass diese Symbole verwendet werden. Dies erfolgt automatisch beim Kompilieren von Bitcode.

Wenn `--dynamic-symbol-mode=linker` an Mtouch, diesem alternativen Lösung wird deaktiviert, und Xamarin.iOS versucht übergeben übergeben `-u` auf dem nativen Linker. Dies führt jedoch wahrscheinlich in nativen Linker-Fehler.

Die Lösung besteht darin, entfernen Sie die `--dynamic-symbol-mode=linker` -Argument aus der weitere Mtouch-Argumente in den Buildoptionen des Projekts.

<!-- 0116 - 0124: free to use -->

<a name="MT0116" />

### <a name="mt0116-invalid-architecture-arch-32-bit-architectures-are-not-supported-when-deployment-target-is-11-or-later-make-sure-the-project-does-not-build-for-a-32-bit-architecture"></a>MT0116: Ungültige Architektur: {Arch}. 32-Bit-Architekturen werden nicht unterstützt, wenn Bereitstellungsziel 11 oder höher ausgeführt wird. Stellen Sie sicher, dass das Projekt für eine 32-Bit-Architektur nicht erstellt werden kann.

iOS 11 enthält keine Unterstützung für 32-Bit-Anwendungen, sodass es nicht unterstützt wird, um für eine 32-Bit-Anwendung zu erstellen, wenn das Bereitstellungsziel iOS 11 oder höher ist.

Ändern Sie die Zielarchitektur in iOS-Buildoptionen des Projekts auf arm64 oder ändern Sie das Bereitstellungsziel in die "Info.plist" des Projekts auf eine frühere Version von iOS.

<a name="MT0117" />

### <a name="mt0117-cant-launch-a-32-bit-app-on-a-simulator-that-only-supports-64-bit"></a>MT0117: Eine 32-Bit-app auf einem Simulator, die nur auf 64-Bit-unterstützt, kann nicht gestartet werden.

<a name="MT0118" />

### <a name="mt0118-aot-files-could-not-be-found-at-the-expected-directory-msymdir"></a>MT0118: AOT-Dateien wurde auf das erwartete Verzeichnis "{Msymdir}" nicht gefunden.

<!-- 0119 - 0123: free to use -->

<a name="MT0123" />

### <a name="mt0123-the-executable-assembly--does-not-reference-"></a>MT0123: Die ausführbare Assembly * verweist nicht auf *.

Konnte kein Verweis auf die Plattformassembly gefunden werden (Xamarin.iOS.dll / Xamarin.TVOS.dll / Xamarin.WatchOS.dll) in die ausführbare Assembly.

Dies tritt meistens dann, wenn kein Code vorhanden ist, in das ausführbare Projekt aus, die von der Plattformassembly verwendet Beispielsweise würde eine leere Main-Methode (und kein weiterer Code) dieser Fehler angezeigt:

```csharp
class Program {
    void Main (string[] args)
    {
    }
}
```

Mithilfe einer API aus der Plattformassembly wird zum Beheben des Fehlers:

```csharp
class Program {
    void Main (string[] args)
    {
        System.Console.WriteLine (typeof (UIKit.UIWindow));
    }
}
```

<a name="MT0124" />

### <a name="mt0124-could-not-set-the-current-language-to-lang-according-to-langlang-exception"></a>MT0124: Die aktuelle Sprache auf '{Lang}' konnte nicht festgelegt (gemäß Sprache = {LANG}): {Exception}

Dies ist eine Warnung, die angibt, dass die aktuelle Sprache für die Sprache in der Fehlermeldung nicht festgelegt werden konnte.

Die aktuelle Sprache wird standardmäßig der Systemsprache verwendet.

<a name="MT0125" />

### <a name="mt0125-the---assembly-build-target-command-line-argument-is-ignored-in-the-simulator"></a>MT0125: --Assembly-Build-Ziel Befehlszeilenargument wird im Simulator ignoriert.

Ist keine Aktion erforderlich, diese Meldung dient nur Informationszwecken.

<a name="MT0126" />

### <a name="mt0126-incremental-builds-have-been-disabled-because-incremental-builds-are-not-supported-in-the-simulator"></a>MT0126: Inkrementelle Builds wurde deaktiviert, da inkrementelle Builds im Simulator nicht unterstützt werden.

Ist keine Aktion erforderlich, diese Meldung dient nur Informationszwecken.

<a name="MT0127" />

### <a name="mt0127-incremental-builds-have-been-disabled-because-this-version-of-xamarinios-does-not-support-incremental-builds-in-projects-that-include-more-than-one-third-party-binding-libraries"></a>MT0127: Inkrementelle Builds wurde deaktiviert, da es sich bei dieser Version von Xamarin.iOS inkrementelle Builds in Projekten nicht unterstützt, die Bindung von mehr als eine Drittanbieter-Bibliotheken enthalten.

Inkrementelle Builds wurde automatisch deaktiviert, da diese Version von Xamarin.iOS nicht immer von Projekten mit mehreren Bibliotheken der Drittanbieter-Bindung ordnungsgemäß erstellt werden kann.

Ist keine Aktion erforderlich, diese Meldung dient nur Informationszwecken.

Weitere Informationen finden Sie unter Fehler #[52727](https://bugzilla.xamarin.com/show_bug.cgi?id=52727).

<a name="MT0128" />

### <a name="mt0128-could-not-touch-the-file--"></a>MT0128: Die Datei nicht genutzt werden kann ' *': *

Wenn eine Datei zu berühren (die ausgeführt wird, um sicherzustellen, dass die ordnungsgemäße Ausführung teilweise Builds), ist ein Fehler aufgetreten.

Diese Warnung kann in den meisten Fällen ignoriert werden; Falls Sie Probleme Datei einen Fehler (https://bugzilla.xamarin.com] (https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)) und es wird untersucht werden.

## <a name="mt1xxx-project-related-error-messages"></a>MT1xxx: Verwandte Fehlermeldungen Projekt

### <a name="mt10xx-installer--mtouch"></a>MT10xx: Installer / Mtouch

<!--
 MT1xxx file copy / symlinks (project related)
  MT10xx installer.cs / mtouch.cs
  -->

<a name="MT1001" />

### <a name="mt1001-could-not-find-an-application-at-the-specified-directory"></a>MT1001: Eine Anwendung wurde im angegebenen Verzeichnis nicht gefunden werden.

<a name="MT1002" />

### <a name="mt1002-could-not-create-symlinks-files-were-copied"></a>MT1002: Symbolische Verknüpfungen konnte nicht erstellt werden, werden Dateien kopiert wurden

<a name="MT1003" />

### <a name="mt1003-could-not-kill-the-application--you-may-have-to-kill-the-application-manually"></a>MT1003: Die Anwendung konnte nicht ebenfalls ' *'. Sie müssen möglicherweise die Anwendung manuell zu beenden.

<a name="MT1004" />

### <a name="mt1004-could-not-get-the-list-of-installed-applications"></a>MT1004: Die Liste der installierten Anwendungen konnte nicht abgerufen werden.

<a name="MT1005" />

### <a name="mt1005-could-not-kill-the-application--on-the-device----you-may-have-to-kill-the-application-manually"></a>MT1005: Die Anwendung konnte nicht beendet "\*"auf dem Gerät"\*': *-Möglicherweise müssen Sie die Anwendung manuell zu beenden.

<a name="MT1006" />

### <a name="mt1006-could-not-install-the-application--on-the-device--"></a>MT1006: Die Anwendung konnte nicht installiert '\*"auf dem Gerät"\*': *.

<a name="MT1007" />

### <a name="mt1007-failed-to-launch-the-application--on-the-device---you-can-still-launch-the-application-manually-by-tapping-on-it"></a>MT1007: Fehler beim Starten der Anwendung "\*"auf dem Gerät"\*': *. Sie können die Anwendung weiterhin manuell durch Tippen gestartet.

<a name="MT1008" />

### <a name="mt1008-failed-to-launch-the-simulator"></a>MT1008: Fehler beim Starten des Simulators

Dieser Fehler wird gemeldet, wenn Mtouch fehlgeschlagen ist, um den Simulator zu starten.   Dies kann auch vorkommen, da bereits ein veralteter oder dead-Simulator-Prozess ausgeführt wird.

Der folgende Befehl für die Unix-Befehlszeile ausgegeben kann zum Beenden von Prozessen für unterbrochene Simulator verwendet werden:

```bash
$ launchctl list|grep UIKitApplication|awk '{print $3}'|xargs launchctl remove
```

<a name="MT1009" />

### <a name="mt1009-could-not-copy-the-assembly--to--"></a>MT1009: Die Assembly konnte nicht kopiert "\*'to'\*': *

Dies ist ein bekanntes Problem in bestimmten Versionen von Xamarin.iOS.

In diesem Fall möchten Probieren Sie die folgende Schritte zur problemumgehung aus:

```bash
sudo chmod 0644 /Library/Frameworks/Xamarin.iOS.framework/Versions/Current/lib/mono/*/*.mdb
```

Aber da in der neuesten Version von Xamarin.iOS dieses Problem gelöst wurde, melden Sie einen neuen Fehler auf [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) mit Ihren vollständigen Versionsinformationen und die Buildausgabe-Protokoll.

<a name="MT1010" />

### <a name="mt1010-could-not-load-the-assembly--"></a>MT1010: Die Assembly nicht geladen werden kann ' *': *

<a name="MT1011" />

### <a name="mt1011-could-not-add-missing-resource-file-"></a>MT1011: Fehlende Ressourcendatei konnte nicht hinzugefügt werden: ' *'

<a name="MT1012" />

### <a name="mt1012-failed-to-list-the-apps-on-the-device--"></a>MT1012: Fehler beim Auflisten der apps auf dem Gerät ' *': *

<a name="MT1013" />

### <a name="mt1013-dependency-tracking-error-no-files-to-compare-please-file-a-bug-report-at-httpbugzillaxamarincom-with-a-test-case"></a>MT1013: Abhängigkeitsüberwachung-Fehler: keine Dateien, verglichen werden soll. Melden Sie bitte einen Fehlerbericht an http://bugzilla.xamarin.com zu einem Testfall.

Hiermit wird einen Fehler in Xamarin.iOS. Melden Sie Fehler auf [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) mit einem Test Caes.

<a name="MT1014" />

### <a name="mt1014-failed-to-re-use-cached-version-of--"></a>MT1014: Fehler beim zwischengespeicherte Version erneut verwenden ' *': *.

<a name="MT1015" />

### <a name="mt1015-failed-to-create-the-executable--"></a>MT1015: Fehler beim Erstellen der ausführbaren Datei "*": *

<a name="MT1015" />

### <a name="mt1015-failed-to-create-the-executable--"></a>MT1015: Fehler beim Erstellen der ausführbaren Datei "*": *

<a name="MT1016" />

### <a name="mt1016-failed-to-create-the-notice-file-because-a-directory-already-exists-with-the-same-name"></a>MT1016: Fehler beim NOTICE-Datei zu erstellen, da bereits ein Verzeichnis mit dem gleichen Namen vorhanden ist.

Entfernen Sie das Verzeichnis `NOTICE` aus dem Projekt.

<a name="MT1017" />

### <a name="mt1017-failed-to-create-the-notice-file-"></a>MT1017: Fehler beim Erstellen der NOTICE-Datei: *.

<a name="MT1018" />

### <a name="mt1018-your-application-failed-code-signing-checks-and-could-not-be-installed-on-the-device--check-your-certificates-provisioning-profiles-and-bundle-ids-probably-your-device-is-not-part-of-the-selected-provisioning-profile-error-0xe8008015"></a>MT1018: Ihre Anwendung bei der Überprüfung Signieren von Code und konnte nicht installiert werden, auf dem Gerät ' *'. Überprüfen Sie die Zertifikate, Profile, Bereitstellung und die bundle-Ids. Ihr Gerät ist wahrscheinlich nicht Teil der das ausgewählte Bereitstellungsprofil (Fehler: 0xe8008015).

<a name="MT1019" />

### <a name="mt1019-your-application-has-entitlements-not-supported-by-your-current-provisioning-profile-and-could-not-be-installed-on-the-device--please-check-the-ios-device-log-for-more-detailed-information-error-0xe8008016"></a>MT1019: Ihre Anwendung verfügt über Berechtigungen, die von Ihrem aktuellen Bereitstellungsprofil nicht unterstützt und konnte nicht installiert werden, auf dem Gerät ' *'. Überprüfen Sie die iOS-Gerät-Protokoll, ausführlichere Informationen (Fehler: 0xe8008016).

Wenn dies:

* Ihre Anwendung verfügt über Berechtigungen, die das aktuelle Profil für die Bereitstellung nicht unterstützt.
  Folgende Lösungen sind möglich:
  - Geben Sie ein anderes Bereitstellungsprofil, das die Berechtigungen unterstützt Ihre anwendungsanforderungen.
  - Entfernen Sie die Berechtigungen, die in der aktuellen Bereitstellungsprofil nicht unterstützt.
* Das Gerät, das Sie bereitstellen, umfasst nicht das Bereitstellungsprofil aus, die Sie verwenden möchten.
  Folgende Lösungen sind möglich:
  - Erstellen einer neuen app aus einer Vorlage in Xcode, das gleiche Bereitstellungsprofil auswählen und auf demselben Gerät bereitstellen. Manchmal kann Xcode automatisch zu aktualisieren, bereitstellungsprofile für neue Geräte (in anderen Fällen, die Xcode Vorgehensweise aufgefordert werden).
  -Wechseln Sie zu der iOS Developer Center, das Bereitstellungsprofil mit dem neuen Gerät zu aktualisieren, und das aktualisierte Bereitstellungsprofil auf Ihren Computer herunterladen.

In den meisten Fällen, die Weitere Informationen zu dem Fehler für den iOS-Gerät Log gedruckt werden, können die Diagnose des Problems.

<a name="MT1020" />

### <a name="mt1020-failed-to-launch-the-application--on-the-device--"></a>MT1020: Fehler beim Starten der Anwendung "\*"auf dem Gerät"\*': *

<a name="MT1021" />

### <a name="mt1021-could-not-copy-the-file--to--2"></a>MT1021: Die Datei konnte nicht kopiert "\*'to'\*": {2}

Eine Datei konnte nicht kopiert werden. Die Fehlermeldung aus der Kopiervorgang hat es sich um weitere Informationen zum Fehler.

<a name="MT1022" />

### <a name="mt1022-could-not-copy-the-directory--to--2"></a>MT1022: Das Verzeichnis konnte nicht kopiert "\*'to'\*": {2}

Ein Verzeichnis konnte nicht kopiert werden. Die Fehlermeldung aus der Kopiervorgang hat es sich um weitere Informationen zum Fehler.

<a name="MT1023" />

### <a name="mt1023-could-not-communicate-with-the-device-to-find-the-application---"></a>MT1023: Keine Kommunikation mit dem Gerät die Anwendung nicht finden "*": *

Fehler beim Versuch, eine Anwendung auf dem Gerät zu suchen.

Um dieses Problem zu lösen versuchen Folgendes:

* Löschen Sie die Anwendung vom Gerät aus, und versuchen Sie es noch mal.
* Trennen Sie das Gerät aus, und verbinden Sie es erneut.
* Starten Sie das Gerät neu.
* Neu starten Sie den Mac.

<a name="MT1024" />

### <a name="mt1024-the-application-signature-could-not-be-verified-on-device--please-make-sure-that-the-provisioning-profile-is-installed-and-not-expired-error-0xe8008017"></a>MT1024: Die Anwendungssignatur konnte nicht überprüft werden, auf Gerät ' *'. Stellen Sie sicher, dass das Bereitstellungsprofil installiert und nicht abgelaufen ist (Fehler: 0xe8008017).

Das Gerät abgewiesen. die Installation der Anwendung die Signatur nicht verifiziert werden konnte.

Stellen Sie sicher, dass das Bereitstellungsprofil installiert und nicht abgelaufen ist.

<a name="MT1025" />

### <a name="mt1025-could-not-list-the-crash-reports-on-the-device-"></a>MT1025: Die Berichte zu Abstürzen auf dem Gerät konnten nicht aufgelistet werden *.

Fehler beim Versuch, die Berichte zu Abstürzen auf dem Gerät auflisten.

Um dieses Problem zu lösen versuchen Folgendes:

* Löschen Sie die Anwendung vom Gerät aus, und versuchen Sie es noch mal.
* Trennen Sie das Gerät aus, und verbinden Sie es erneut.
* Starten Sie das Gerät neu.
* Neu starten Sie den Mac.
* Synchronisieren Sie das Gerät mit iTunes (Dadurch werden alle Berichte zu Abstürzen vom Gerät entfernt).

<a name="MT1026" />

### <a name="mt1026-could-not-download-the-crash-report--from-the-device-"></a>MT1026: Konnte nicht heruntergeladen werden den Absturzbericht * vom Gerät *.

Fehler beim Versuch, die Berichte zu Abstürzen vom Gerät herunterladen.

Um dieses Problem zu lösen versuchen Folgendes:

* Löschen Sie die Anwendung vom Gerät aus, und versuchen Sie es noch mal.
* Trennen Sie das Gerät aus, und verbinden Sie es erneut.
* Starten Sie das Gerät neu.
* Neu starten Sie den Mac.
* Synchronisieren Sie das Gerät mit iTunes (Dadurch werden alle Berichte zu Abstürzen vom Gerät entfernt).

<a name="MT1027" />

### <a name="mt1027-cant-use-xcode-7-to-launch-applications-on-devices-with-ios--xcode-7-only-supports-ios-6"></a>MT1027: Xcode 7 oder höher zum Starten von Anwendungen auf Geräten mit iOS können keine * (Xcode 7 unterstützt nur iOS 6 und höher).

Es ist nicht möglich, Xcode 7 oder höher verwenden, um Anwendungen auf Geräten mit iOS-Version unter 6.0 zu starten.

Verwenden Sie eine ältere Version von Xcode, oder tippen Sie auf die app manuell, um ihn zu starten.

<a name="MT1028" />

### <a name="mt1028-invalid-device-specification--expected-ios-watchos-or-all"></a>MT1028: Ungültiger Gerätename-Spezifikation: ' *'. Erwartete "Ios", "Watchos" oder "all".

Die Device Specification übergeben mit--Gerät ist ungültig. Gültige Werte sind: "Ios", "Watchos" oder "all".

<a name="MT1029" />

### <a name="mt1029-could-not-find-an-application-at-the-specified-directory-"></a>MT1029: Eine Anwendung im angegebenen Verzeichnis wurde nicht gefunden: *

Der Pfad der Anwendung, die an – Launchdev ist nicht vorhanden. Geben Sie ein gültiges app-Paket an.

<a name="MT1030" />

### <a name="mt1030-launching-applications-on-device-using-a-bundle-identifier-is-deprecated-please-pass-the-full-path-to-the-bundle-to-launch"></a>MT1030: Starten von Anwendungen auf dem Gerät über eine Bündel-ID ist veraltet. Übergeben Sie den vollständigen Pfad, auf das Paket aus, um zu starten.

Es wird empfohlen, um den Pfad an der app, starten Sie auf dem Gerät und nicht nur die Bündel-Id übergeben.

<a name="MT1031" />

### <a name="mt1031-could-not-launch-the-app--on-the-device--because-the-device-is-locked-please-unlock-the-device-and-try-again"></a>MT1031: Die app konnte nicht gestartet werden "\*"auf dem Gerät"\*", da das Gerät gesperrt ist. Entsperren des Geräts, und versuchen Sie es erneut.

Entsperren des Geräts, und versuchen Sie es erneut.

<a name="MT1032" />

### <a name="mt1032-this-application-executable-might-be-too-large--mb-to-execute-on-device-if-bitcode-was-enabled-you-might-want-to-disable-it-for-development-it-is-only-required-to-submit-applications-to-apple"></a>MT1032: Diese ausführbare Datei der Anwendung ist möglicherweise zu groß (* MB) auf Gerät auszuführen. Wenn Bitcode aktiviert war es für die Entwicklung deaktivieren möchten, es ist nur erforderlich, um Anwendungen an Apple übermitteln.

<a name="MT1033" />

### <a name="mt1033-could-not-uninstall-the-application--from-the-device--"></a>MT1033: Die Anwendung konnte nicht deinstalliert werden "\*"auf dem Gerät"\*': *

<!-- 1034 used by mmp -->

<a name="MT1035" />

### <a name="mt1035-cannot-include-different-versions-of-the-framework-name"></a>MT1035: Verschiedene Versionen des Frameworks "{Name}" darf keine enthalten werden.

Es ist nicht möglich, mit verschiedenen Versionen desselben Frameworks zu verknüpfen.

Dies geschieht normalerweise, wenn eine Erweiterung eine andere Version eines Frameworks wie bei der Container-app (möglicherweise durch einen Drittanbieter-Assemblybindung) verweist.

Nach diesem Fehler stehen mehrere [MT1036](#MT1036) Fehler, die mit den Pfaden für jedes andere Framework.

<a name="MT1036" />

### <a name="mt1036-framework-name-included-from-path-related-to-previous-error"></a>MT1036: Framework "{Name}" aus der eingeschlossenen: {Path} (verknüpft zum vorherigen Fehler)

Dieser Fehler wird gemeldet, nur zusammen mit [MT1036](#MT1036). Informieren Sie sich [MT1036](#MT1036) für Weitere Informationen.

### <a name="mt11xx-debug-service"></a>MT11xx: Debuggen-Diensts

<!--
  MT11xx DebugService.cs
  -->

<a name="MT1101" />

### <a name="mt1101-could-not-start-app"></a>MT1101: App konnte nicht gestartet werden.

<a name="MT1102" />

### <a name="mt1102-could-not-attach-to-the-app-to-kill-it-"></a>MT1102: Konnte nicht angefügt werden, um die app (für die sie beenden möchten): *

<a name="MT1103" />

### <a name="mt1103-could-not-detach"></a>MT1103: Konnte nicht getrennt werden.

<a name="MT1104" />

### <a name="mt1104-failed-to-send-packet-"></a>MT1104: Paket konnte nicht gesendet: *

<a name="MT1105" />

### <a name="mt1105-unexpected-response-type"></a>MT1105: Unerwarteter Antworttyp

<a name="MT1106" />

### <a name="mt1106-could-not-get-list-of-applications-on-the-device-request-timed-out"></a>MT1106: Liste der Anwendungen konnte auf dem Gerät nicht abgerufen werden: Timeout bei der Anforderung.

<a name="MT1107" />

### <a name="mt1107-application-failed-to-launch-"></a>MT1107: Anwendung konnte nicht gestartet: *

Überprüfen Sie, ob das Gerät gesperrt wird.

Wenn Sie eine Unternehmens-app bereitstellen oder verwenden die kostenlose Bereitstellungsprofil erstellen, Sie haben die Entwickler vertrauen (Dies wird erläutert <a href="https://stackoverflow.com/a/30726375/183422">hier</a>).

<a name="MT1108" />

### <a name="mt1108-could-not-find-developer-tools-for-this-xx-yy-device"></a>MT1108: Entwicklertools wurde für dieses Gerät XX (JJ) nicht gefunden werden.

Einige Vorgänge von Mtouch erfordern die <tt>DeveloperDiskImage.dmg</tt> Datei vorhanden sein.   Diese Datei ist Teil von Xcode und befindet sich in der Regel relativ zum SDK, das Sie verwenden, um die berücksichtigt werden sollen, erstellen in der <tt>Xcode.app/Contents/Developer/iPhoneOS.platform/DeviceSupport/VERSION/DeveloperDiskImage.dmg</tt>.

Dieser Fehler kann entweder auftreten, da Sie nicht über eine DeveloperDiskImage.dmg verfügen, die das Gerät entspricht, das Sie eine Verbindung hergestellt haben.

<a name="MT1109" />

### <a name="mt1109-application-failed-to-launch-because-the-device-is-locked-please-unlock-the-device-and-try-again"></a>MT1109: Anwendung konnte nicht gestartet werden, da das Gerät gesperrt ist. Entsperren des Geräts, und versuchen Sie es erneut.

Überprüfen Sie, ob das Gerät gesperrt wird.

<a name="MT1110" />

### <a name="mt1110-application-failed-to-launch-because-of-ios-security-restrictions-please-ensure-the-developer-is-trusted"></a>MT1110: Anwendung konnte aufgrund von sicherheitseinschränkungen für iOS zu starten. Stellen Sie sicher, dass die Quelle vertrauenswürdig ist.

Wenn Sie eine Unternehmens-app bereitstellen oder verwenden die kostenlose Bereitstellungsprofil erstellen, Sie haben die Entwickler vertrauen (Dies wird erläutert <a href="https://stackoverflow.com/a/30726375/183422">hier</a>).

<a name="MT1111" />

### <a name="mt1111-application-launched-successfully-but-its-not-possible-to-wait-for-the-app-to-exit-as-requested-because-its-not-possible-to-detect-app-termination-when-launching-using-gdbserver"></a>MT1111: Anwendung erfolgreich gestartet, aber es ist nicht möglich, warten Sie, bis die app zu beenden, wie angefordert, da es nicht zum Beenden der app zu erkennen, beim Starten mit Gdbserver möglich ist.

### <a name="mt12xx-simulator"></a>MT12xx: Simulator

<!--
  MT12xx simcontroller.cs
  -->

<a name="MT1201" />

### <a name="mt1201-could-not-load-the-simulator-"></a>MT1201: Den Simulator konnte nicht geladen werden: *

<a name="MT1202" />

### <a name="mt1202-invalid-simulator-configuration-"></a>MT1202: Ungültige Simulator-Konfiguration: *

<a name="MT1203" />

### <a name="mt1203-invalid-simulator-specification-"></a>MT1203: Ungültige Simulator-Spezifikation: *

<a name="MT1204" />

### <a name="mt1204-invalid-simulator-specification--runtime-not-specified"></a>MT1204: Ungültige Simulator Spezifikation ' *': Common Language Runtime nicht angegeben.

<a name="MT1205" />

### <a name="mt1205-invalid-simulator-specification--device-type-not-specified"></a>MT1205: Ungültige Simulator-Spezifikation ' *': Typ des Geräts nicht angegeben.

<a name="MT1206" />

### <a name="mt1206-could-not-find-the-simulator-runtime-"></a>MT1206: Die Simulator-Laufzeit nicht gefunden ' *'.

<a name="MT1207" />

### <a name="mt1207-could-not-find-the-simulator-device-type-"></a>MT1207: Gerätetyp, für den Simulator nicht gefunden ' *'.

<a name="MT1208" />

### <a name="mt1208-could-not-find-the-simulator-runtime-"></a>MT1208: Die Simulator-Laufzeit nicht gefunden ' *'.

<a name="MT1209" />

### <a name="mt1209-could-not-find-the-simulator-device-type-"></a>MT1209: Gerätetyp, für den Simulator nicht gefunden ' *'.

<a name="MT1210" />

### <a name="mt1210-invalid-simulator-specification--unknown-key-"></a>MT1210: Ungültige Simulator-Spezifikation: \*, Unbekannter Schlüssel "\*"

<a name="MT1211" />

### <a name="mt1211-the-simulator-version--does-not-support-the-simulator-type-"></a>MT1211: Der Simulator, Version "\*'unterstützt nicht den Simulator-Typ'\*"

<a name="MT1212" />

### <a name="mt1212-failed-to-create-a-simulator-version-where-type---and-runtime--"></a>MT1212: Fehler beim Erstellen einer Simulator, Version, wobei der Typ = * und = *.

<a name="MT1213" />

### <a name="mt1213-invalid-simulator-specification-for-xcode-4-"></a>MT1213: Ungültige Simulator-Spezifikation für Xcode 4: *

<a name="MT1214" />

### <a name="mt1214-invalid-simulator-specification-for-xcode-5-"></a>MT1214: Ungültige Simulator-Spezifikation für Xcode 5: *

<a name="MT1215" />

### <a name="mt1215-invalid-sdk-specified-"></a>MT1215: Ungültiger SDK angegeben: *

<a name="MT1216" />

### <a name="mt1216-could-not-find-the-simulator-udid-"></a>MT1216: Den Simulator UDID wurde nicht gefunden ' *'.

<a name="MT1217" />

### <a name="mt1217-could-not-load-the-app-bundle-at-"></a>MT1217: Das app-Bündel unter konnte nicht geladen werden "*".

<a name="MT1218" />

### <a name="mt1218-no-bundle-identifier-found-in-the-app-at-"></a>MT1218: In der app zu wurde keine Bündel-ID gefunden. ' *'.

<a name="MT1219" />

### <a name="mt1219-could-not-find-the-simulator-for-"></a>MT1219: Den Simulator für wurde nicht gefunden ' *'.

<a name="MT1220" />

### <a name="mt1220-could-not-find-the-latest-simulator-runtime-for-device-"></a>MT1220: Die aktuellste Laufzeit für den Simulator für Geräte wurde nicht gefunden ' *'.

Dies gibt in der Regel auf ein Problem mit Xcode an.

Zum Beheben dieses Problems versuchen Folgendes:

* Verwenden Sie den Simulator einmal in Xcode.
* Übergeben Sie eine explizite SDK-Version, die mit dem Sdk – <version>.
* Installieren Sie Xcode neu.

<a name="MT1221" />

### <a name="mt1221-could-not-find-the-paired-iphone-simulator-for-the-watchos-simulator-"></a>MT1221: Den gekoppelten iPhone-Simulator für die WatchOS-Simulator wurde nicht gefunden ' *'.

Wenn Sie eine WatchOS-app in einem WatchOS-Simulator zu starten, muss eine gekoppelte iOS-Simulator sowie vorhanden sein.

Sehen Sie sich, Simulatoren mit iOS-Simulatoren, die mithilfe Xcodes Geräte Benutzeroberfläche gekoppelt werden können (Menü "Fenster" -> Geräte).

### <a name="mt13xx-linkwith"></a>MT13xx: [LinkWith]

<!--
  MT13xx [LinkWith]
  -->

<a name="MT1301" />

### <a name="mt1301-native-library---was-ignored-since-it-does-not-match-the-current-build-architectures-"></a>MT1301: Native Bibliothek `*` (\*) wurde ignoriert, da sie nicht den aktuellen Build Architecture(s) übereinstimmt (\*)

<a name="MT1302" />

### <a name="mt1302-could-not-extract-the-native-library--from--please-ensure-the-native-library-was-properly-embedded-in-the-managed-assembly-if-the-assembly-was-built-using-a-binding-project-the-native-library-must-be-included-in-the-project-and-its-build-action-must-be-objcbindingnativelibrary"></a>MT1302: Die native Bibliothek konnte nicht extrahiert ' *' von '+'. Stellen Sie sicher, dass die native Bibliothek korrekt in der verwalteten Assembly eingebettet wurde (wenn die Assembly wurde mit einem bindungsprojekt erstellt, die native Bibliothek im Projekt enthalten sein muss und dessen "Buildvorgang" muss "ObjcBindingNativeLibrary").

<a name="MT1303" />

### <a name="mt1303-could-not-decompress-the-native-framework--from--please-review-the-build-log-for-more-information-from-the-native-zip-command"></a>MT1303: Native Framework konnte nicht dekomprimiert "\*'von'\*". Überprüfen Sie das Buildprotokoll für Weitere Informationen aus dem systemeigenen "Zip"-Befehl.

Das angegebene systemeigene Framework aus der bindungsbibliothek konnte nicht dekomprimiert werden.

Überprüfen Sie das Protokoll Bulid für Weitere Informationen zu diesem Fehler aus dem systemeigenen "Zip"-Befehl.

<a name="MT1304" />

### <a name="mt1304-the-embedded-framework--in--is-invalid-it-does-not-contain-an-infoplist"></a>MT1304: Das eingebettete Framework "*" in * ist ungültig: er enthält keine Datei "Info.plist".

Die angegebene eingebettete Framework enthält eine Datei "Info.plist" nicht und ist daher kein gültiger Framework.

Stellen Sie sicher, dass das Framework gültig ist.

<a name="MT1305" />

### <a name="mt1305-the-binding-library--contains-a-user-framework--but-embedded-user-frameworks-require-ios-80-the-current-deployment-target-is--please-set-the-deployment-target-in-the-infoplist-file-to-at-least-80"></a>MT1305: Die bindungsbibliothek "\*" enthält ein Framework für Benutzer (\*), jedoch eingebettete Benutzer Frameworks erforderlich ist, iOS 8.0 oder höher (die aktuelles Bereitstellungsziel ist *). Legen Sie das Bereitstellungsziel in der Datei "Info.plist" mindestens 8.0 ein.

Die angegebene Bindung-Bibliothek enthält ein eingebettetes Framework, aber Xamarin.iOS unterstützt nur eingebettete Frameworks auf iOS 8.0 oder höher.

Legen Sie das Bereitstellungsziel in der Datei "Info.plist" mindestens 8.0, um diesen Fehler zu beheben (oder verwenden Sie keine eingebettete Frameworks).

### <a name="mt14xx-crash-reports"></a>MT14xx: Absturzberichte

<!--
  MT14xx    CrashReports.cs
  -->

<a name="MT1400" />

### <a name="mt1400-could-not-open-crash-report-service-afcconnectionopen-returned-"></a>MT1400: Berichtsserver-Absturz-Dienst konnte nicht geöffnet werden: AFCConnectionOpen zurückgegeben *

Fehler beim Versuch, die Berichte zu Abstürzen vom Gerät aus zugreifen.

Um dieses Problem zu lösen versuchen Folgendes:

* Löschen Sie die Anwendung vom Gerät aus, und versuchen Sie es noch mal.
* Trennen Sie das Gerät aus, und verbinden Sie es erneut.
* Starten Sie das Gerät neu.
* Neu starten Sie den Mac.
* Synchronisieren Sie das Gerät mit iTunes (Dadurch werden alle Berichte zu Abstürzen vom Gerät entfernt).

<a name="MT1401" />

### <a name="mt1401-could-not-close-crash-report-service-afcconnectionclose-returned-"></a>MT1401: Berichtsserver-Absturz-Dienst konnte nicht geschlossen werden: AFCConnectionClose zurückgegeben *

Fehler beim Versuch, die Berichte zu Abstürzen vom Gerät aus zugreifen.

Um dieses Problem zu lösen versuchen Folgendes:

* Löschen Sie die Anwendung vom Gerät aus, und versuchen Sie es noch mal.
* Trennen Sie das Gerät aus, und verbinden Sie es erneut.
* Starten Sie das Gerät neu.
* Neu starten Sie den Mac.
* Synchronisieren Sie das Gerät mit iTunes (Dadurch werden alle Berichte zu Abstürzen vom Gerät entfernt).

<a name="MT1402" />

### <a name="mt1402-could-not-read-file-info-for--afcfileinfoopen-returned-"></a>MT1402: Konnte nicht gelesen werden Dateiinformationen für *: AFCFileInfoOpen zurückgegeben *

Fehler beim Versuch, die Berichte zu Abstürzen vom Gerät aus zugreifen.

Um dieses Problem zu lösen versuchen Folgendes:

* Löschen Sie die Anwendung vom Gerät aus, und versuchen Sie es noch mal.
* Trennen Sie das Gerät aus, und verbinden Sie es erneut.
* Starten Sie das Gerät neu.
* Neu starten Sie den Mac.
* Synchronisieren Sie das Gerät mit iTunes (Dadurch werden alle Berichte zu Abstürzen vom Gerät entfernt).

<a name="MT1403" />

### <a name="mt1403-could-not-read-crash-report-afcdirectoryopen--returned-"></a>MT1403: Absturzbericht konnten nicht gelesen werden: AFCDirectoryOpen (*) zurückgegeben: *

Fehler beim Versuch, die Berichte zu Abstürzen vom Gerät aus zugreifen.

Um dieses Problem zu lösen versuchen Folgendes:

* Löschen Sie die Anwendung vom Gerät aus, und versuchen Sie es noch mal.
* Trennen Sie das Gerät aus, und verbinden Sie es erneut.
* Starten Sie das Gerät neu.
* Neu starten Sie den Mac.
* Synchronisieren Sie das Gerät mit iTunes (Dadurch werden alle Berichte zu Abstürzen vom Gerät entfernt).

<a name="MT1404" />

### <a name="mt1404-could-not-read-crash-report-afcfilerefopen--returned-"></a>MT1404: Absturzbericht konnten nicht gelesen werden: AFCFileRefOpen (*) zurückgegeben: *

Fehler beim Versuch, die Berichte zu Abstürzen vom Gerät aus zugreifen.

Um dieses Problem zu lösen versuchen Folgendes:

* Löschen Sie die Anwendung vom Gerät aus, und versuchen Sie es noch mal.
* Trennen Sie das Gerät aus, und verbinden Sie es erneut.
* Starten Sie das Gerät neu.
* Neu starten Sie den Mac.
* Synchronisieren Sie das Gerät mit iTunes (Dadurch werden alle Berichte zu Abstürzen vom Gerät entfernt).

<a name="MT1405" />

### <a name="mt1405-could-not-read-crash-report-afcfilerefread--returned-"></a>MT1405: Absturzbericht konnten nicht gelesen werden: AFCFileRefRead (*) zurückgegeben: *

Fehler beim Versuch, die Berichte zu Abstürzen vom Gerät aus zugreifen.

Um dieses Problem zu lösen versuchen Folgendes:

* Löschen Sie die Anwendung vom Gerät aus, und versuchen Sie es noch mal.
* Trennen Sie das Gerät aus, und verbinden Sie es erneut.
* Starten Sie das Gerät neu.
* Neu starten Sie den Mac.
* Synchronisieren Sie das Gerät mit iTunes (Dadurch werden alle Berichte zu Abstürzen vom Gerät entfernt).

<a name="MT1406" />

### <a name="mt1406-could-not-list-crash-reports-afcdirectoryopen--returned-"></a>MT1406: Berichte zu Abstürzen konnten nicht aufgelistet werden: AFCDirectoryOpen (*) zurückgegeben: *

Fehler beim Versuch, die Berichte zu Abstürzen vom Gerät aus zugreifen.

Um dieses Problem zu lösen versuchen Folgendes:

* Löschen Sie die Anwendung vom Gerät aus, und versuchen Sie es noch mal.
* Trennen Sie das Gerät aus, und verbinden Sie es erneut.
* Starten Sie das Gerät neu.
* Neu starten Sie den Mac.
* Synchronisieren Sie das Gerät mit iTunes (Dadurch werden alle Berichte zu Abstürzen vom Gerät entfernt).

<!--- 1407 used by mmp -->

### <a name="mt16xx-macho"></a>MT16xx: MachO

<!--
  MT16xx    MachO.cs
  -->

<a name="MT1600" />

### <a name="mt1600-not-a-mach-o-dynamic-library-unknown-header-0x-"></a>MT1600: Keine Mach-O dynamische Bibliothek (unbekannte Header "0 X *"): *.

Fehler beim Verarbeiten der betreffenden dynamischen Bibliothek.

Stellen Sie sicher, dass die dynamische Bibliothek für eine gültige dynamische Mach-O-Bibliothek ist.

Das Format einer Bibliothek kann überprüft werden, mithilfe der `file` Befehl über das Terminal:

    file -arch all -l /path/to/library.dylib

<a name="MT1601" />

### <a name="mt1601-not-a-static-library-unknown-header--"></a>MT1601: Keine statische Bibliothek (unbekannte Header ' *'): *.

Fehler beim Verarbeiten der statischen Bibliothek, die betreffende.

Stellen Sie sicher, dass die statische Bibliothek eine gültige statische Mach-O-Bibliothek ist.

Das Format einer Bibliothek kann überprüft werden, mithilfe der `file` Befehl über das Terminal:

    file -arch all -l /path/to/library.a

<a name="MT1602" />

### <a name="mt1602-not-a-mach-o-dynamic-library-unknown-header-0x-"></a>MT1602: Keine Mach-O dynamische Bibliothek (unbekannte Header "0 X *"): *.

Fehler beim Verarbeiten der betreffenden dynamischen Bibliothek.

Stellen Sie sicher, dass die dynamische Bibliothek für eine gültige dynamische Mach-O-Bibliothek ist.

Das Format einer Bibliothek kann überprüft werden, mithilfe der `file` Befehl über das Terminal:

    file -arch all -l /path/to/library.dylib

<a name="MT1603" />

### <a name="mt1603-unknown-format-for-fat-entry-at-position--in-"></a>MT1603: Unbekanntes Format fat Eintrag an der Position * in *.

Fehler beim Verarbeiten der betreffenden fat-Archivs.

Stellen Sie sicher, dass das fat-Archiv gültig ist.

Das Format des ein fat-Archiv kann überprüft werden, mithilfe der `file` Befehl über das Terminal:

    file -arch all -l /path/to/file

<a name="MT1604" />

### <a name="mt1604-file-of-type--is-not-a-macho-file-"></a>MT1604: Die Datei vom Typ * ist keine MachO-Datei (*).

Fehler beim Verarbeiten der MachO betreffende Datei.

Stellen Sie sicher, dass die Datei eine gültige dynamische Mach-O-Bibliothek ist.

Das Format für eine Datei kann überprüft werden, mithilfe der `file` Befehl über das Terminal:

    file -arch all -l /path/to/file

## <a name="mt2xxx-linker-error-messages"></a>MT2xxx: Linker-Fehlermeldungen

<!--
 MT2xxx Linker
  MT20xx Linker (general) errors
  -->

<a name="MT2001" />

### <a name="mt2001-could-not-link-assemblies"></a>MT2001: Assemblys konnte nicht verknüpft werden.

Dieser Fehler bedeutet, dass der verwaltete Linker einen unerwarteten Fehler, z. B. eine Ausnahme aufgetreten ist und abgeschlossen oder nicht speichern Sie die Assembly, die verarbeitet werden. Weitere Informationen zu den genauen Fehler wird in das Buildprotokoll, z. B. sein

```
error MT2001: Could not link assemblies.
    Method: `System.Void Todo.TodoListPageCS/<<-ctor>b__1_0>d::SetStateMachine(System.Runtime.CompilerServices.IAsyncStateMachine)`
    Assembly: `QuickTodo, Version=1.0.6297.28241, Culture=neutral, PublicKeyToken=null`
Reason: Value cannot be null.
Parameter name: instruction
```

Es ist wichtig, in der Datei eines Fehlerberichts für derartige Probleme. In den meisten Fällen kann eine problemumgehung bereitgestellt werden, bis eine richtige Lösung veröffentlicht wird. Die obige Informationen unbedingt (zusammen mit einem Testfall und/oder die Assembly Binairy) zur Behebung des Problems zur Verfügung.

<a name="MT2002" />

### <a name="mt2002-can-not-resolve-reference-"></a>MT2002: Verweis kann nicht aufgelöst: *

<a name="MT2003" />

### <a name="mt2003-option--will-be-ignored-since-linking-is-disabled"></a>MT2003: Option "*" wird ignoriert, da die Verknüpfung deaktiviert ist

<a name="MT2004" />

### <a name="mt2004-extra-linker-definitions-file--could-not-be-located"></a>MT2004: Zusätzliche Linker-Definitionsdatei ' *' konnte nicht gefunden werden.

<a name="MT2005" />

### <a name="mt2005-definitions-from--could-not-be-parsed"></a>MT2005: Definitionen von ' *' konnte nicht analysiert werden.

<a name="MT2006" />

### <a name="mt2006-can-not-load-mscorlibdll-from--please-reinstall-xamarinios"></a>MT2006: Kann nicht geladen werden "mscorlib.dll" aus: *. Installieren Sie Xamarin.iOS erneut.

Dies gibt im Allgemeinen, dass ein Problem mit Ihrem Xamarin.iOS-Installation vorhanden ist. Versuchen Sie es neu installieren von Xamarin.iOS.

<!--- 2007 used by mmp -->
<!--- 2009 used by mmp -->

<a name="MT2010" />

### <a name="mt2010-unknown-httpmessagehandler--valid-values-are-httpclienthandler-default-cfnetworkhandler-or-nsurlsessionhandler"></a>MT2010: Unbekannter HttpMessageHandler `*`. Gültige Werte sind HttpClientHandler (Standard), CFNetworkHandler oder NSUrlSessionHandler

<a name="MT2011" />

### <a name="mt2011-unknown-tlsprovider---valid-values-are-default-or-appletls"></a>MT2011: Unbekannter TlsProvider `*`.  Gültige Werte sind Default oder Appletls.

Der angegebene Wert auf `tls-provider=` ist kein gültiger Anbieter TLS (Transport Layer Security).

Die `default` und `appletls` sind die einzigen gültigen Werte und beide die gleiche Option, um die SSL/TLS unterstützt die systemeigene Apple-TLS-API mit darstellen.

<!--- 2012 used by mmp -->
<!--- 2013 used by mmp -->
<!--- 2014 used by mmp -->

<a name="MT2015" />

### <a name="mt2015-invalid-httpmessagehandler--for-watchos-the-only-valid-value-is-nsurlsessionhandler"></a>MT2015: Ungültige HttpMessageHandler `*` für WatchOS. Der einzige gültige Wert ist NSUrlSessionHandler.

Dies ist eine Warnung aus, die auftritt, da die Projektdatei ein ungültiger HttpMessageHandler angibt.

Frühere Versionen der Tools Preview wird standardmäßig generiert einen ungültigen Wert in der Projektdatei.

Um diese Warnung zu beheben, öffnen Sie die Projektdatei in einem Text-Editor, und entfernen Sie alle Knoten der httpmessagehandler, der für aus der XML-Code.

<a name="MT2016" />

### <a name="mt2016-invalid-tlsprovider-legacy-option-the-only-valid-value-appletls-will-be-used"></a>MT2016: Ungültige TlsProvider `legacy` Option. Der einzige gültige Wert `appletls` verwendet werden.

Die `legacy` -Anbieter, der eine vollständig verwaltete SSLv3 wurde / nur TLSv1-Anbieter wird nicht mehr mit Xamarin.iOS ausgeliefert. Projekte, wurden mithilfe dieses alte Anbieters und erstellen Sie jetzt mit der neueren `appletls` eine.

Um diese Warnung zu beheben, öffnen Sie die Projektdatei in einem Text-Editor, und entfernen Sie alle "MtouchTlsProvider'' Knoten aus der XML-Code.

<a name="MT2017" />

### <a name="mt2017-could-not-process-xml-description"></a>MT2017: XML-Beschreibung konnte nicht verarbeitet werden.

Dies bedeutet, dass ein Fehler auf der [Linker für die benutzerdefinierte XML-Konfigurationsdatei](https://developer.xamarin.com/guides/cross-platform/advanced/custom_linking/) Sie angegeben haben, überprüfen Sie die Datei.

<a name="MT2018" />

### <a name="mt2018-the-assembly--is-referenced-from-two-different-locations--and-"></a>MT2018: Die Assembly "\*" wird aus zwei verschiedenen Speicherorten verwiesen: "\*' und ' *'.

Die Assembly, die in der Fehlermeldung genannten wird von mehreren Speicherorten geladen. Stellen Sie sicher, dass immer die gleiche Version einer Assembly verwenden.

<a name="MT2019" />

### <a name="mt2019-can-not-load-the-root-assembly-"></a>MT2019: Die stammassembly nicht geladen werden kann ' *'

Die stammassembly konnte nicht geladen werden. Stellen Sie sicher, dass der Pfad in der Fehlermeldung auf eine vorhandene Datei verweist und sie eine gültige .NET-Assembly ist.

<a name="MT202x" />

### <a name="mt202x-binding-optimizer-failed-processing-"></a>MT202x: Binden der Optimierer Verarbeitung Fehler `...`.

Ein unerwartetes trat beim Optimieren der generierte Code binden. Das Element, das Problem verursachen heißt in der Fehlermeldung. Zum Beheben dieses Problems die Assembly mit dem Namen (oder mit dem Typ oder Methode mit dem Namen) werden in bereitgestellt werden müssen, eine [Fehlerbericht](http://bugzilla.xamarin.com) zusammen mit einer vollständigen Buildprotokoll mit Ausführlichkeit aktiviert (d. h. `-v -v -v -v` in die **Weitere Mtouch Argumente**).

Die letzte Ziffer `x` werden:
* `0` für den Namen einer Assembly;
* `1` für einen Typnamen;
* `3` für den Namen einer Methode;

<a name="MT2030" />

### <a name="mt2030-remove-user-resources-failed-processing-"></a>MT2030: Entfernen Benutzerressourcen Fehler beim Verarbeiten von `...`.

Etwas unerwartetes ist aufgetreten beim Versuch, Benutzerressourcen zu entfernen. Die Assembly, die das Problem verursacht einen Namen in der Fehlermeldung. Zum Beheben dieses Problems der Assemblys werden im bereitgestellt werden müssen, eine [Fehlerbericht](http://bugzilla.xamarin.com) zusammen mit einer vollständigen Buildprotokoll mit Ausführlichkeit aktiviert (d. h. `-v -v -v -v` in die **Weitere Mtouch-Argumente**).

Benutzerressourcen werden Dateien in Assemblys (wie Ressourcen), die beim Erstellen, um das anwendungsbündel zu erstellen, extrahiert werden muss. Dies umfasst Folgendes:

* `__monotouch_content_*` und `__monotouch_pages_*` Ressourcen; und
* Native Bibliotheken, die in einer bindungsassembly eingebettet;

<a name="MT2040" />

### <a name="mt2040-default-httpmessagehandler-setter-failed-processing-"></a>MT2040: Standardmäßige HttpMessageHandler Setter Fehler verarbeiten `...`.

Etwas unerwartetes ist aufgetreten beim Versuch, die der Standardsatz `HttpMessageHandler` für die Anwendung. Melden Sie ein [Fehlerbericht](http://bugzilla.xamarin.com) zusammen mit einer vollständigen Buildprotokoll mit Ausführlichkeit aktiviert (d. h. `-v -v -v -v` in die **Weitere Mtouch-Argumente**).

<a name="MT2050" />

### <a name="mt2050-code-remover-failed-processing-"></a>MT2050: Code Remover Fehler bei Verarbeitung `...`.

Etwas unerwartetes ist aufgetreten beim Versuch, Code aus der BCL, die mit der Anwendung zu entfernen. Melden Sie ein [Fehlerbericht](http://bugzilla.xamarin.com) zusammen mit einer vollständigen Buildprotokoll mit Ausführlichkeit aktiviert (d. h. `-v -v -v -v` in die **Weitere Mtouch-Argumente**).

<a name="MT2060" />

### <a name="mt2060-sealer-failed-processing-"></a>MT2060: Sealer Fehler bei Verarbeitung `...`.

Ein unerwartetes beim versiegeln Sie Typen oder Methoden (final), oder wenn einige Methoden devirtualizing aufgetreten ist. Die Assembly, die das Problem verursacht einen Namen in der Fehlermeldung. Zum Beheben dieses Problems der Assemblys werden im bereitgestellt werden müssen, eine [Fehlerbericht](http://bugzilla.xamarin.com) zusammen mit einer vollständigen Buildprotokoll mit Ausführlichkeit aktiviert (d. h. `-v -v -v -v` in die **Weitere Mtouch-Argumente**).

<a name="MT2070" />

### <a name="mt2070-metadata-reducer-failed-processing-"></a>MT2070: Metadaten Reducer Fehler bei Verarbeitung `...`.

Etwas unerwartetes ist aufgetreten beim Versuch, die Metadaten aus der Anwendung zu reduzieren. Die Assembly, die das Problem verursacht einen Namen in der Fehlermeldung. Zum Beheben dieses Problems der Assemblys werden im bereitgestellt werden müssen, eine [Fehlerbericht](http://bugzilla.xamarin.com) zusammen mit einer vollständigen Buildprotokoll mit Ausführlichkeit aktiviert (d. h. `-v -v -v -v` in die **Weitere Mtouch-Argumente**).

<a name="MT2080" />

### <a name="mt2080-marknsobjects-failed-processing-"></a>MT2080: MarkNSObjects Fehler beim Verarbeiten von `...`.

Ein unerwartetes trat beim Markieren `NSObject` Unterklassen aus der Anwendung. Die Assembly, die das Problem verursacht einen Namen in der Fehlermeldung. Zum Beheben dieses Problems der Assemblys werden im bereitgestellt werden müssen, eine [Fehlerbericht](http://bugzilla.xamarin.com) zusammen mit einer vollständigen Buildprotokoll mit Ausführlichkeit aktiviert (d. h. `-v -v -v -v` in die **Weitere Mtouch-Argumente**).

<a name="MT2090" />

### <a name="mt2090-inliner-failed-processing-"></a>MT2090: Inliners: Fehler bei Verarbeitung `...`.

Etwas unerwartetes ist aufgetreten beim Versuch, Inline-Code aus der Anwendung. Die Assembly, die das Problem verursacht einen Namen in der Fehlermeldung. Um dieses Problem zu beheben, die Assembly im angegeben werden müssen eine [Fehlerbericht](https://bugzilla.xamarin.com) zusammen mit einer vollständigen Buildprotokoll mit Ausführlichkeit aktiviert (d. h. `-v -v -v -v` in die **Weitere Mtouch-Argumente**).

<!-- MT21xx: more linker errors -->

<!--- 2100 used by mmp -->

<a name="MT2100" />

### <a name="mt2100-smart-enum-conversion-preserver-failed-processing-"></a>MT2100: Intelligente Enum Konvertierung beibehalten Fehler bei Verarbeitung `...`.

Etwas unerwartetes ist aufgetreten beim Versuch, die Konvertierungsmethoden für intelligente Enumerationen, die von der Anwendung zu markieren. Die Assembly, die das Problem verursacht einen Namen in der Fehlermeldung. Um dieses Problem zu beheben, die Assembly im angegeben werden müssen eine [Fehlerbericht](https://bugzilla.xamarin.com) zusammen mit einer vollständigen Buildprotokoll mit Ausführlichkeit aktiviert (d. h. `-v -v -v -v` in die **Weitere Mtouch-Argumente**).

<a name="MT2101" />

### <a name="mt2101-cant-resolve-the-reference--referenced-from-the-method--in-"></a>MT2101: Den Verweis kann nicht aufgelöst werden kann '\*", auf die verwiesen wird von der Methode"\*' in ' *'.

Verweis auf eine ungültige Assembly wurde gefunden, bei der Verarbeitung von der Methode, die in der Fehlermeldung genannten.

Die Assembly, die das Problem verursacht einen Namen in der Fehlermeldung. Zum Beheben dieses Problems der Assemblys werden im bereitgestellt werden müssen, eine [Fehlerbericht](https://bugzilla.xamarin.com) zusammen mit einer vollständigen Buildprotokoll mit Ausführlichkeit aktiviert (d. h. `-v -v -v -v` in die **Weitere Mtouch-Argumente**).

<a name="MT2102" />

### <a name="mt2102-error-processing-the-method--in-the-assembly--"></a>MT2102: Fehler beim Verarbeiten der Methode "\*"in der Assembly"\*': *

Etwas unerwartetes ist aufgetreten beim Versuch, die die Methode, die in der Fehlermeldung genannten gekennzeichnet werden.

Die Assembly, die das Problem verursacht einen Namen in der Fehlermeldung. Zum Beheben dieses Problems der Assemblys werden im bereitgestellt werden müssen, eine [Fehlerbericht](https://bugzilla.xamarin.com) zusammen mit einer vollständigen Buildprotokoll mit Ausführlichkeit aktiviert (d. h. `-v -v -v -v` in die **Weitere Mtouch-Argumente**).

<a name="MT2103" />

### <a name="mt2103-error-processing-assembly--"></a>MT2103: Fehler beim Verarbeiten der Assembly "\*': *

Unerwarteter Fehler beim Verarbeiten einer Assemblys.

Die Assembly, die das Problem verursacht einen Namen in der Fehlermeldung. Um dieses Problem zu beheben, die Assembly im angegeben werden müssen eine [Fehlerbericht](https://bugzilla.xamarin.com) zusammen mit einer vollständigen Buildprotokoll mit Ausführlichkeit aktiviert (d. h. `-v -v -v -v` in die **Weitere Mtouch-Argumente**).

<a name="MT2104" />

### <a name="mm2104-unable-to-link-assembly-0-as-it-is-mixed-mode"></a>MM2104: So verknüpfen Sie Assembly kann nicht '{0}"unverändert im gemischten Modus.

Gemischte Assemblys können nicht vom Linker verarbeitet werden.

Finden Sie unter https://msdn.microsoft.com/library/x0w2664k.aspx für Weitere Informationen zu gemischten Assemblys.

## <a name="mt3xxx-aot-error-messages"></a>MT3xxx: AOT-Fehlermeldungen

<!--
 MT3xxx AOT
  MT30xx AOT (general) errors
  -->

<a name="MT3001" />

### <a name="mt3001-could-not-aot-the-assembly-"></a>MT3001: Konnte kein AOT der Assembly ' *'

Dies gibt im Allgemeinen einen Fehler in der AOT-Compiler. Um einen Fehlerbericht [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) mit einem Projekt, das verwendet werden kann, um den Fehler zu reproduzieren.

Manchmal ist es möglich, dieses Problem umgehen, indem inkrementelle Builds des Projekts, IOS-Buildoption deaktivieren (aber immer noch einen Fehler, also trotzdem senden Sie den Bericht).

<a name="MT3002" />

### <a name="mt3002-aot-restriction-method--must-be-static-since-it-is-decorated-with-monopinvokecallback-see-httpsdeveloperxamarincomguidesiosadvancedtopicslimitationsreversecallbackshttpsdeveloperxamarincomguidesiosadvancedtopicslimitationsreversecallbacks"></a>MT3002: AOT-Einschränkung: Methode ' *' muss statisch sein, da sie mit [MonoPInvokeCallback] markiert ist. Sieh [https://developer.xamarin.com/guides/ios/advanced_topics/limitations/#Reverse_Callbacks](https://developer.xamarin.com/guides/ios/advanced_topics/limitations/#Reverse_Callbacks)

Diese Fehlermeldung stammt aus der AOT-Compiler.

<a name="MT3003" />

### <a name="mt3003-conflicting---debug-and---llvm-options-soft-debugging-is-disabled"></a>MT3003: Es wurden widersprüchliche--Debug "und"--Llvm-Optionen. Soft-debugging ist deaktiviert.

Debuggen wird nicht unterstützt, wenn die LLVM aktiviert ist. Wenn Sie die app zu debuggen möchten, müssen Sie zuerst deaktivieren Sie LLVM.

<a name="MT3004" />

### <a name="mt3004-could-not-aot-the-assembly--because-it-doesnt-exist"></a>MT3004: Konnte kein AOT der Assembly ' *', da er nicht vorhanden ist.

<a name="MT3005" />

### <a name="mt3005-the-dependency--of-the-assembly--was-not-found-please-review-the-projects-references"></a>MT3005: Die Abhängigkeit "\*"der Assembly"\*' wurde nicht gefunden. Überprüfen Sie die Verweise des Projekts an.

Dies tritt normalerweise auf, wenn eine andere Version einer Plattform-Assembly (in der Regel die .NET 4-Version von "mscorlib.dll") auf eine Assembly verweist.

Dies wird nicht unterstützt, und möglicherweise nicht erstellen oder die Ausführung ordnungsgemäß (die Assembly möglicherweise API aus der .NET 4-Version von "mscorlib.dll", die die Xamarin.iOS-Version nicht verwenden).

<a name="MT3006" />

### <a name="mt3006-could-not-compute-a-complete-dependency-map-for-the-project-this-will-result-in-slower-build-times-because-xamarinios-cant-properly-detect-what-needs-to-be-rebuilt-and-what-does-not-need-to-be-rebuilt-please-review-previous-warnings-for-more-details"></a>MT3006: Eine vollständige Abhängigkeitsdiagramm für das Projekt konnte nicht berechnet werden. Dies führt zu langsameren Builderstellung zu beschleunigen, da Xamarin.iOS nicht ordnungsgemäß erkannt werden, was neu erstellt werden muss (und was nicht neu erstellt werden muss). Überprüfen Sie vorherige Warnungen, die weitere Details an.

 Erstellen oder die Ausführung ordnungsgemäß (die Assembly möglicherweise API aus der .NET 4-Version von "mscorlib.dll", die die Xamarin.iOS-Version nicht verwenden).

<a name="MT3007" />

### <a name="mt3007-debug-info-files-mdb-will-not-be-loaded-when-llvm-is-enabled"></a>MT3007: Debug-Info-Dateien (*.mdb) werden nicht geladen werden, wenn die Llvm aktiviert ist.

<a name="MT3008" />

### <a name="mt3008-bitcode-support-requires-the-use-of-the-llvm-aot-backend---llvm"></a>MT3008: Unterstützung von Bitcode erfordert die Verwendung des LLVM AOT-Back-Ends (--Llvm)

Unterstützung von Bitcode erfordert die Verwendung des LLVM AOT-Back-Ends (--Llvm).

Deaktivieren Sie die Unterstützung von Bitcode oder aktivieren Sie LLVM.

<!--- 3009 used by mmp -->
<!--- 3010 used by mmp -->

## <a name="mt4xxx-code-generation-error-messages"></a>MT4xxx: Code generieren-Fehlermeldungen

### <a name="mt40xx-main"></a>MT40xx: Main

<!--
 MT4xxx code generation
  MT40xx main.m
  -->

<a name="MT4001" />

### <a name="mt4001-the-main-template-could-not-be-expanded-to-"></a>MT4001: Die Hauptvorlage konnten nicht erweitert werden, um `*`.

Fehler beim Generieren von main.m. Melden Sie Fehler auf [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT4002" />

### <a name="mt4002-failed-to-compile-the-generated-code-for-pinvoke-methods-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4002: Fehler beim Kompilieren des generierten Codes für P/Invoke-Methoden. Melden Sie bitte einen Fehlerbericht an http://bugzilla.xamarin.com

Fehler beim Kompilieren des generierten Codes für P/Invoke-Methoden. Melden Sie bitte einen Fehlerbericht an [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="mt41xx-registrar"></a>MT41xx: Registrierung

<!--
  MT41xx registrar.m
  -->

<a name="MT4101" />

### <a name="mt4101-the-registrar-cannot-build-a-signature-for-type-"></a>MT4101: Die Registrierungsstelle eine Signatur für den Typ kann nicht erstellt werden `*`.

Ein Typ in exportierten API gefunden, der die Laufzeit das Marshallen in Objective-c nicht bekannt

Wenn Sie glauben, Xamarin.iOS, sollte den betreffende Typ unterstützen, und melden Sie bitte eine Verbesserung der Anforderung an [ http://bugzilla.xamarin.com ](http://bugzilla.xamarin.com).

<a name="MT4102" />

### <a name="mt4102-the-registrar-found-an-invalid-type--in-signature-for-method--use--instead"></a>MT4102: Die Registrierungsstelle finden Sie einen ungültigen Typ `*` in der Signatur für Methode `*`. Verwenden Sie stattdessen `*`.

Dies geschieht derzeit nur mit einem Typ: System.DateTime. Verwenden Sie stattdessen die Objective-C-Entsprechung (NSDate).

<a name="MT4103" />

### <a name="mt4103-the-registrar-found-an-invalid-type--in-signature-for-method--the-type-implements-inativeobject-but-does-not-have-a-constructor-that-takes-two-intptr-bool-arguments"></a>MT4103: Die Registrierungsstelle finden Sie einen ungültigen Typ `*` in der Signatur für Methode `*`: Der Typ INativeObject implementiert, jedoch verfügt nicht über einen Konstruktor, der zwei übernimmt (IntPtr, Bool) Argumente

Dies tritt auf, wenn die Registrierungsstelle einen Typ in einer Signatur mit den genannten Eigenschaften auftreten. Die Registrierungsstelle müssen möglicherweise neue Instanzen des Typs zu erstellen und in diesem Fall erfordert einen Konstruktor mit den (IntPtr, Bool) Signatur - das erste Argument (IntPtr) gibt das verwaltete Handle, während die zweite, wenn der Aufrufer über den Besitz der systemeigenen übergibt. Behandeln Sie (Wenn dieser Wert "false" ist, "beibehalten" für das Objekt aufgerufen werden).

<a name="MT4104" />

### <a name="mt4104-the-registrar-cannot-marshal-the-return-value-for-type--in-signature-for-method-"></a>MT4104: Die Registrierungsstelle kann nicht den Rückgabewert für den Typ gemarshallt `*` in der Signatur für Methode `*`.

Ein Typ in exportierten API gefunden, der die Laufzeit das Marshallen in Objective-c nicht bekannt

Wenn Sie glauben, Xamarin.iOS, sollte den betreffende Typ unterstützen, und melden Sie bitte eine Verbesserung der Anforderung an [ http://bugzilla.xamarin.com ](http://bugzilla.xamarin.com).

<a name="MT4105" />

### <a name="mt4105-the-registrar-cannot-marshal-the-parameter-of-type--in-signature-for-method-"></a>MT4105: Die Registrierungsstelle kann nicht den Parameter vom Typ gemarshallt `*` in der Signatur für Methode `*`.

Wenn Sie glauben, Xamarin.iOS, sollte den betreffende Typ unterstützen, und melden Sie bitte eine Verbesserung der Anforderung an [ http://bugzilla.xamarin.com ](http://bugzilla.xamarin.com).

<a name="MT4106" />

### <a name="mt4106-the-registrar-cannot-marshal-the-return-value-for-structure--in-signature-for-method-"></a>MT4106: Die Registrierungsstelle kann nicht den Rückgabewert für Struktur gemarshallt `*` in der Signatur für Methode `*`.

Ein Typ in exportierten API gefunden, der die Laufzeit das Marshallen in Objective-c nicht bekannt

Wenn Sie glauben, Xamarin.iOS, sollte den betreffende Typ unterstützen, und melden Sie bitte eine Verbesserung der Anforderung an [ http://bugzilla.xamarin.com ](http://bugzilla.xamarin.com).

<a name="MT4107" />

### <a name="mt4107-the-registrar-cannot-marshal-the-parameter-of-type--in-signature-for-method-"></a>MT4107: Die Registrierungsstelle kann nicht den Parameter vom Typ gemarshallt `*` in der Signatur für Methode `+`.

Ein Typ in exportierten API gefunden, der die Laufzeit das Marshallen in Objective-c nicht bekannt

Wenn Sie glauben, Xamarin.iOS, sollte den betreffende Typ unterstützen, und melden Sie bitte eine Verbesserung der Anforderung an [ http://bugzilla.xamarin.com ](http://bugzilla.xamarin.com).

<a name="MT4108" />

### <a name="mt4108-the-registrar-cannot-get-the-objectivec-type-for-managed-type-"></a>MT4108: Die Registrierungsstelle kann nicht abgerufen werden den Typ ObjectiveC für verwalteten Typ `*`.

Ein Typ in exportierten API gefunden, der die Laufzeit das Marshallen in Objective-c nicht bekannt

Wenn Sie glauben, Xamarin.iOS, sollte den betreffende Typ unterstützen, und melden Sie bitte eine Verbesserung der Anforderung an [ http://bugzilla.xamarin.com ](http://bugzilla.xamarin.com).

<a name="MT4109" />

### <a name="mt4109-failed-to-compile-the-generated-registrar-code-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4109: Fehler beim Kompilieren des Codes generierte Registrierungsstelle. Melden Sie bitte einen Fehlerbericht an http://bugzilla.xamarin.com

Fehler beim Kompilieren des generierten Codes für die Registrierung. Das Buildprotokoll enthält die Ausgabe des systemeigenen Compilers, die erläutern, warum die Kompilieren des Codes nicht ist.

Dies ist immer ein Fehler in Xamarin.iOS. Melden Sie bitte einen Fehlerbericht an [ http://bugzilla.xamarin.com ](http://bugzilla.xamarin.com) mit Ihrem Projekt oder einem Testfall.

<a name="MT4110" />

### <a name="mt4110-the-registrar-cannot-marshal-the-out-parameter-of-type--in-signature-for-method-"></a>MT4110: Die Registrierungsstelle kann nicht vom Typ den Out-Parameter gemarshallt `*` in der Signatur für Methode `*`.

<a name="MT4111" />

### <a name="mt4111-the-registrar-cannot-build-a-signature-for-type--in-method-"></a>MT4111: Die Registrierungsstelle eine Signatur für den Typ kann nicht erstellt werden `*` in Methode `*`.

<a name="MT4112" />

### <a name="mt4112-the-registrar-found-an-invalid-type--registering-generic-types-with-objective-c-is-not-supported-and-may-lead-to-random-behavior-andor-crashes-for-backwards-compatibility-with-older-versions-of-xamarinios-it-is-possible-to-ignore-this-error-by-passing---unsupported--enable-generics-in-registrar-as-an-additional-mtouch-argument-in-the-projects-ios-build-options-page-see-developerxamarincomguidesiosadvancedtopicsregistrarhttpsdeveloperxamarincomguidesiosadvancedtopicsregistrar-for-more-information"></a>MT4112: Die Registrierungsstelle finden Sie einen ungültigen Typ `*`. Registrieren von generischen Typen mit Objective-C wird nicht unterstützt und können zufälliges Verhalten bzw. abstürzen führen (für Abwärtskompatibilität Kompatibilität mit älteren Versionen von Xamarin.iOS es ist möglich, diesen Fehler ignorieren, indem übergeben `--unsupported--enable-generics-in-registrar` als eine weitere Mtouch ein Argument des Projekts iOS Build-Optionen-Seite. Finden Sie unter [developer.xamarin.com/guides/ios/advanced_topics/registrar](https://developer.xamarin.com/guides/ios/advanced_topics/registrar) Informationen).

<a name="MT4113" />

### <a name="mt4113-the-registrar-found-a-generic-method--exporting-generic-methods-is-not-supported-and-will-lead-to-random-behavior-andor-crashes"></a>MT4113: Die Registrierungsstelle finden Sie eine generische Methode: "\*.\*". Generische Methoden exportieren wird nicht unterstützt und führt zu zufälliges Verhalten bzw. stürzt ab.

<a name="MT4114" />

### <a name="mt4114-unexpected-error-in-the-registrar-for-the-method----please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4114: Unerwarteter Fehler bei der Registrierungsstelle für die Methode "\*.\*'-melden Sie bitte einen Fehlerbericht an http://bugzilla.xamarin.com

<a name="MT4116" />

### <a name="mt4116-could-not-register-the-assembly--"></a>MT4116: Die Assembly konnte nicht registriert "*": *

<a name="MT4117" />

### <a name="mt4117-the-registrar-found-a-signature-mismatch-in-the-method----the-selector-indicates-the-method-takes--parameters-while-the-managed-method-has--parameters"></a>MT4117: Die Registrierungsstelle eine Nichtübereinstimmung der Datenträgersignatur finden Sie in der Methode "*.*" – gibt die Auswahl an die Methode akzeptiert * Parameter, während die verwaltete Methode hat * Parameter.

<a name="MT4118" />

### <a name="mt4118-cannot-register-two-managed-types--and--with-the-same-native-name-"></a>MT4118: Kann nicht registriert werden zwei verwaltete Typen ("\*'und'\*") mit dem gleichen Namen für die native ('* ').

<a name="MT4119" />

### <a name="mt4119-could-not-register-the-selector--of-the-member--because-the-selector-is-already-registered-on-a-different-member"></a>MT4119: Den Selektor konnte nicht registriert "\*"des Elements"\*. *", da die Auswahl auf ein anderes Element bereits registriert ist.

<a name="MT4120" />

### <a name="mt4120-the-registrar-found-an-unknown-field-type--in-field--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4120: Die Registrierungsstelle finden Sie einen Unbekannter Feldtyp '\*"im Feld"\*. * ". Melden Sie bitte einen Fehlerbericht an http://bugzilla.xamarin.com

Dieser Fehler zeigt einen Fehler in Xamarin.iOS. Melden Sie bitte einen Fehlerbericht an [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT4121" />

### <a name="mt4121-cannot-use-gccg-to-compile-the-generated-code-from-the-static-registrar-when-using-the-accounts-framework-the-header-files-provided-by-apple-used-during-the-compilation-require-clang-either-use-clang---compilerclang-or-the-dynamic-registrar---registrardynamic"></a>MT4121: Können keine GCC / G ++, um den generierten Code aus der statischen Registrierungsstelle zu kompilieren, bei Verwendung der Konten Framework (die Headerdateien, die von Apple, die während der Kompilierung verwendeten bereitgestellten erfordern Clang). Verwenden Sie entweder Clang (--Compiler: Clang) oder die dynamische Registrierungsstelle (--Registrierungsstelle: dynamische).

<a name="MT4122" />

### <a name="mt4122-cannot-use-the-clang-compiler-provided-in-the--sdk-to-compile-the-generated-code-from-the-static-registrar-when-non-ascii-type-names--are-present-in-the-application-either-use-gccg---compilergccg-the-dynamic-registrar---registrardynamic-or-a-newer-sdk"></a>MT4122: Kann nicht bereitgestellten, den Clang-Compiler verwendet die *.* SDK zum Kompilieren des generierten Codes von der statischen Registrierungsstelle bei nicht-ASCII-Typnamen ('* ') in der Anwendung vorhanden sind. GCC verwenden / G ++ (--Compiler: Gcc | g++), die dynamische Registrierungsstelle (--Registrierungsstelle: dynamische) oder ein neueres SDK.

<a name="MT4123" />

### <a name="mt4123-the-type-of-the-variadic-parameter-in-the-variadic-function--must-be-systemintptr"></a>MT4123: Der Typ des Parameters "Variadic" in der Variadic-Funktion ' *' muss System.IntPtr sein.

<a name="MT4124" />

### <a name="mt4124-invalid--found-on--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4124: Ungültige * finden Sie auf "*". Melden Sie bitte einen Fehlerbericht an http://bugzilla.xamarin.com

Dieser Fehler zeigt einen Fehler in Xamarin.iOS. Melden Sie bitte einen Fehlerbericht an [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT4125" />

### <a name="mt4125-the-registrar-found-an-invalid-type--in-signature-for-method--the-interface-must-have-a-protocol-attribute-specifying-its-wrapper-type"></a>MT4125: Die Registrierungsstelle finden Sie einen ungültigen Typ "\*"in der Signatur für die Methode"\*": Die Schnittstelle muss es sich um ein Protokoll-Attribut, das Festlegen des Wrappertyps aufweisen.

<a name="MT4126" />

### <a name="mt4126-cannot-register-two-managed-protocols--and--with-the-same-native-name-"></a>MT4126: Zwei verwaltete Protokolle kann nicht registriert werden ("\*'und'\*") mit dem gleichen Namen für die native ('* ').

<a name="MT4127" />

### <a name="mt4127-cannot-register-more-than-one-interface-method-for-the-method--which-is-implementing-"></a>MT4127: Für die Methode mehr als eine Schnittstellenmethode kann nicht registriert werden kann '\*"(implementiert wird, die"\*").

<a name="MT4128" />

### <a name="mt4128-the-registrar-found-an-invalid-generic-parameter-type--in-the-method--the-generic-parameter-must-have-an-nsobject-constraint"></a>MT4128: Die Registrierungsstelle finden Sie einen ungültige generischen Parametertyp '\*"in der Methode"\*". Der generische Parameter muss eine Einschränkung "NSObject" haben.

<a name="MT4129" />

### <a name="mt4129-the-registrar-found-an-invalid-generic-return-type--in-the-method--the-generic-return-type-must-have-an-nsobject-constraint"></a>MT4129: Die Registrierungsstelle finden Sie einen ungültige generischen Rückgabetyp '\*"in der Methode"\*". Der generische Rückgabetyp muss eine Einschränkung "NSObject" haben.

<a name="MT4130" />

### <a name="mt4130-the-registrar-cannot-export-static-methods-in-generic-classes-"></a>MT4130: Die Registrierungsstelle kann nicht statische Methoden in generischen Klassen exportiert ('* ').

<a name="MT4131" />

### <a name="mt4131-the-registrar-cannot-export-static-properties-in-generic-classes-"></a>MT4131: Die Registrierungsstelle kann nicht statische Eigenschaften in generischen Klassen exportiert ("\*.\*").

<a name="MT4132" />

### <a name="mt4132-the-registrar-found-an-invalid-generic-return-type--in-the-property--the-return-type-must-have-an-nsobject-constraint"></a>MT4132: Die Registrierungsstelle finden Sie einen ungültige generischen Rückgabetyp '\*"in der Eigenschaft"\*". Der Rückgabetyp muss eine Einschränkung "NSObject" haben.

<a name="MT4133" />

### <a name="mt4133-cannot-construct-an-instance-of-the-type--from-objective-c-because-the-type-is-generic-runtime-exception"></a>MT4133: Eine Instanz des Typs kann nicht erstellt werden kann ' *' von Objective-C, da der Typ generisch ist. [Runtime Exception]

<a name="MT4134" />

### <a name="mt4134-your-application-is-using-the--framework-which-isnt-included-in-the-ios-sdk-youre-using-to-build-your-app-this-framework-was-introduced-in-ios--while-youre-building-with-the-ios--sdk-please-select-a-newer-sdk-in-your-apps-ios-build-options"></a>MT4134: Ihre Anwendung verwendet die "*" in der iOS-SDK enthalten ist, Sie zum Erstellen der app verwenden-Framework (dieses Framework wurde in iOS eingeführt *, während Sie mit dem iOS entwickeln * SDK.) Wählen Sie ein neueres SDK in Ihrer app iOS-Buildoptionen aus.

<a name="MT4135" />

### <a name="mt4135-the-member--has-an-export-attribute-that-doesnt-specify-a-selector-a-selector-is-required"></a>MT4135: Das Element "\*.\*" verfügt über ein Export-Attribut, das angibt, eine Auswahl. Eine Auswahl ist erforderlich.

<a name="MT4136" />

### <a name="mt4136-the-registrar-cannot-marshal-the-parameter-type--of-the-parameter--in-the-method-"></a>MT4136: Die Registrierungsstelle nicht marshallen kann den Parametertyp "\*"des Parameters"\*"in der Methode"\*. *"

<!-- MT4137 is unused -->

<a name="MT4138" />

### <a name="mt4138-the-registrar-cannot-marshal-the-property-type--of-the-property-"></a>MT4138: Die Registrierungsstelle den Eigenschaftentyp nicht marshallen kann "\*'der Eigenschaft"\*. * ".

<a name="MT4139" />

### <a name="mt4139-the-registrar-cannot-marshal-the-property-type--of-the-property--properties-with-the-connect-attribute-must-have-a-property-type-of-nsobject-or-a-subclass-of-nsobject"></a>MT4139: Die Registrierungsstelle den Eigenschaftentyp nicht marshallen kann "\*'der Eigenschaft"\*. * ". Eigenschaften mit dem [verbinden]-Attribut müssen es sich um einen Eigenschaftentyp von NSObject (oder eine Unterklasse von NSObject) verfügen.

<a name="MT4140" />

### <a name="mt4140-the-registrar-found-a-signature-mismatch-in-the-method----the-selector-indicates-the-variadic-method-takes--parameters-while-the-managed-method-has--parameters"></a>MT4140: Die Registrierungsstelle eine Nichtübereinstimmung der Datenträgersignatur finden Sie in der Methode "*.*" – die Auswahl gibt an, die Variadic-Methode nimmt * Parameter, während die verwaltete Methode hat * Parameter.

<a name="MT4141" />

### <a name="mt4141-cannot-register-the-selector--on-the-member--because-xamarinios-implicitly-registers-this-selector"></a>MT4141: Die Auswahl kann nicht registriert werden kann '\*"für das Element"\*' da Xamarin.iOS implizit diesen Selektor registriert.

Dieser Fehler tritt beim Erstellen von Unterklassen für eine Framework-Typ und Implementierung einer "beibehalten", "release" oder 'Dealloc'-Methode:

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

Es ist jedoch möglich, diese Methoden überschreiben, wenn die Klasse die erste Unterklasse des Frameworks nicht eingeben:

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

In diesem Fall überschreibt Xamarin.iOS `retain`, `release` und `dealloc` auf die `MyNSObject` -Klasse, und kein Konflikt auftritt.

<a name="MT4142" />

### <a name="mt4142-failed-to-register-the-type-"></a>MT4142: Fehler beim Registrieren des Typs ' *'.

<a name="MT4143" />

### <a name="mt4143-the-objectivec-class--could-not-be-registered-it-does-not-seem-to-derive-from-any-known-objectivec-class-including-nsobject"></a>MT4143: Die Klasse ObjectiveC ' *' konnte nicht registriert, es scheint nicht zum Ableiten von einer bekannten ObjectiveC-Klasse (einschließlich NSObject).

<a name="MT4144" />

### <a name="mt4144-cannot-register-the-method--since-it-does-not-have-an-associated-trampoline-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4144: Die Methode kann nicht registriert werden kann ' *', da sie nicht über eine zugeordnete Trampoline verfügt. Melden Sie bitte einen Fehlerbericht an http://bugzilla.xamarin.com.

Hiermit wird einen Fehler in Xamarin.iOS. Melden Sie Fehler auf [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT4145" />

### <a name="mt4145-invalid-enum--enums-with-the-native-attribute-must-have-a-underlying-enum-type-of-either-long-or-ulong"></a>MT4145: Ungültiger Enum ' *': Enumerationen mit dem [systemeigene]-Attribut müssen einen zugrunde liegenden Enumerationstyp "long" oder "Ulong" haben.

<a name="MT4146" />

### <a name="mt4146-the-name-parameter-of-the-registrar-attribute-on-the-class---contains-an-invalid-character--"></a>MT4146: Die Name-Parameter des Attributs Registrierungsstelle für der Klasse\*' ('\*") enthält ein ungültiges Zeichen: '\*" (\*).

Der Name einer Objectice-C-Klasse darf keine Leerzeichen, dies bedeutet, dass enthalten die `Register` -Attribut in der entsprechenden verwalteten Klasse keine `Name` Parameter darf keine Leerzeichen enthalten, entweder.

Stellen Sie sicher, die die `Register` Attribut für die verwaltete Klasse, die in der Fehlermeldung genannten enthält keine Leerzeichen.

<a name="MT4147" />

### <a name="mt4147-detected-a-protocol-inheriting-from-the-jsexport-protocol-while-using-the-dynamic-registrar-it-is-not-possible-to-export-protocols-to-javascriptcore-dynamically-the-static-registrar-must-be-used-add---registrarstatic-to-the-additional-mtouch-arguments-in-the-projects-ios-build-options-to-select-the-static-registrar"></a>MT4147: Ein Protokoll, das Protokoll JSExport erben, bei der Verwendung der dynamischen Registrierungsstelle wird erkannt. Es ist nicht möglich, zum Exportieren von Protokollen in JavaScriptCore dynamisch; die statische Registrierungsstelle muss verwendet werden (hinzufügen "– Registrierungsstelle: Static, auf die weitere Mtouch-Argumente in des Projekts, iOS Build-Optionen, um die statische Registrierungsstelle auszuwählen).

<a name="MT4148" />

### <a name="mt4148-the-registrar-found-a-generic-protocol--exporting-generic-protocols-is-not-supported"></a>MT4148: Die Registrierungsstelle finden Sie ein allgemeines Protokoll: ' *'. Generische Protokolle exportieren wird nicht unterstützt.

<a name="MT4149" />

### <a name="mt4149-cannot-register-the-method--because-the-type-of-the-first-parameter--does-not-match-the-category-type-"></a>MT4149: Die Methode kann nicht registriert werden kann '\*.\*"da der Typ des ersten Parameters ("\*") entspricht nicht den Typ der ("\*").

<a name="MT4150" />

### <a name="mt4150-cannot-register-the-type--because-the-type-property--in-its-category-attribute-does-not-inherit-from-nsobject"></a>MT4150: Den Typ kann nicht registriert werden kann '\*"da die Type-Eigenschaft ("\*") in seiner Kategorie Attribut erbt nicht von NSObject.

<a name="MT4151" />

### <a name="mt4151-cannot-register-the-type--because-the-type-property-in-its-category-attribute-isnt-set"></a>MT4151: Den Typ kann nicht registriert werden kann ' *', da die Type-Eigenschaft in der Category-Attribut nicht festgelegt ist.

<a name="MT4152" />

### <a name="mt4152-cannot-register-the-type--as-a-category-because-it-implements-inativeobject-or-subclasses-nsobject"></a>MT4152: Den Typ kann nicht registriert werden kann ' *' als Kategorie da INativeObject oder Unterklassen von NSObject implementiert.

<a name="MT4153" />

### <a name="mt4153-cannot-register-the-type--as-a-category-because-its-generic"></a>MT4153: Den Typ kann nicht registriert werden kann '\*"als Kategorie, da sie generisch ist.

<a name="MT4154" />

### <a name="mt4154-cannot-register-the-method--as-a-category-method-because-its-generic"></a>MT4154: Die Methode kann nicht registriert werden kann '\*"als Kategorie-Methode, da sie generisch ist.

<a name="MT4155" />

### <a name="mt4155-cannot-register-the-method--with-the-selector--as-a-category-method-on--because-the-objective-c-already-has-an-implementation-for-this-selector"></a>MT4155: Die Methode kann nicht registriert werden "\*"mit der Auswahl"\*" als Kategorie Methode für ' *', da die Objective-C bereits eine Implementierung für diese Auswahl ist.

<a name="MT4156" />

### <a name="mt4156-cannot-register-two-categories--and--with-the-same-native-name-"></a>MT4156: Zwei Kategorien kann nicht registriert werden ("\*'und'\*") mit dem gleichen Namen für die native ('* ').

<a name="MT4157" />

### <a name="mt4157-cannot-register-the-category-method--because-at-least-one-parameter-is-required-and-its-type-must-match-the-category-type-"></a>MT4157: Der Category-Methode kann nicht registriert werden "\*", da mindestens ein Parameter erforderlich ist (und sein Typ muss den Typ der entsprechen "\*")

<a name="MT4158" />

### <a name="mt4158-cannot-register-the-constructor--in-the-category--because-constructors-in-categories-are-not-supported"></a>MT4158: Den Konstruktor kann nicht registriert werden. * in der Kategorie * da Konstruktoren in Kategorien nicht unterstützt werden.

<a name="MT4159" />

### <a name="mt4159-cannot-register-the-method--as-a-category-method-because-category-methods-must-be-static"></a>MT4159: Die Methode kann nicht registriert werden. "*" als Kategorie-Methode, da Kategorie Methoden statisch sein müssen.

<a name="MT4160" />

### <a name="mt4160-invalid-character---found-in-selector--for-"></a>MT4160: Ungültiges Zeichen '\*"(\*) finden Sie in der Auswahl"\*'for'\*".

<a name="MT4161" />

### <a name="mt4161-the-registrar-found-an-unsupported-structure--all-fields-in-a-structure-must-also-be-structures-field--with-type-2-is-not-a-structure"></a>MT4161: Die Registrierungsstelle finden Sie eine nicht unterstützte Struktur '\*": Alle Felder in einer Struktur muss auch Strukturen (Feld "\*"mit Typ'{2}' ist keine Struktur).

Die Registrierungsstelle finden Sie eine Struktur mit nicht unterstützte Felder.

Alle Felder in einer Struktur, die in Objective-C verfügbar gemacht wird, müssen auch Strukturen (nicht-Klassen) sein.

<a name="MT4162" />

### <a name="mt4162-the-type--used-as--2-is-not-available-in---it-was-introduced-in---please-build-with-a-newer--sdk-usually-done-by-using-the-most-recent-version-of-xcode"></a>MT4162: Der Typ "\*" (als * {2}) ist nicht verfügbar in ** (er wurde eingeführt, * *)\* erstellen Sie mit einer neueren * SDK (normalerweise mit der neuesten Version von Xcode vorgenommen.

Die Registrierungsstelle finden Sie einen Typ, der nicht in das aktuelle SDK enthalten ist.

Aktualisieren Sie Xcode.

<a name="MT4163" />

### <a name="mt4163-internal-error-in-the-registrar--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4163: Interner Fehler in der Registrierungsstelle (*). Melden Sie bitte einen Fehlerbericht an http://bugzilla.xamarin.com

Dieser Fehler zeigt einen Fehler in Xamarin.iOS. Melden Sie bitte einen Fehlerbericht an [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT4164" />

### <a name="mt4164-cannot-export-the-property--because-its-selector--is-an-objective-c-keyword-please-use-a-different-name"></a>MT4164: Die Eigenschaft kann nicht exportiert werden kann '\*' da dem Selektor "\*" ist ein Objective-C-Schlüsselwort. Verwenden Sie einen anderen Namen ein.

Der Selektor für die betreffende Eigenschaft ist kein Objective-C-Bezeichner.

Verwenden Sie einen gültigen Objective-C-Bezeichner als Selektoren ein.

<a name="MT4165" />

### <a name="mt4165-the-registrar-couldnt-find-the-type-systemvoid-in-any-of-the-referenced-assemblies"></a>MT4165: Die Registrierungsstelle konnte nicht den Typ 'System.Void' in keinem der referenzierten Assemblys gefunden.

Dieser Fehler wahrscheinlich gibt einen Fehler in Xamarin.iOS an. Melden Sie bitte einen Fehlerbericht an [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT4166" />

### <a name="mt4166-cannot-register-the-method--because-the-signature-contains-a-type--that-isnt-a-reference-type"></a>MT4166: Die Methode kann nicht registriert werden kann '\*", weil die Signatur ein Typs enthält (\*), der kein Verweistyp ist.

In der Regel gibt einen Fehler in Xamarin.iOS. Melden Sie Fehler auf [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT4167" />

### <a name="mt4167-cannot-register-the-method--because-the-signature-contains-a-generic-type--with-a-generic-argument-type-that-isnt-an-nsobject-subclass-"></a>MT4167: Die Methode kann nicht registriert werden kann '\*", weil die Signatur ein generisches Typs enthält (\*) mit einem generischen Argument-Typ, der eine Unterklasse NSObject (*) ist.

In der Regel gibt einen Fehler in Xamarin.iOS. Melden Sie Fehler auf [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT4168" />

### <a name="mt4168-cannot-register-the-type-managedname-because-its-objective-c-name-exportedname-is-an-objective-c-keyword-please-use-a-different-name"></a>MT4168: Den Typ kann nicht registriert werden "{verwaltet\_Name}" da die Objective-C-Name "{exportiert\_Name}" ist ein Objective-C-Schlüsselwort. Verwenden Sie einen anderen Namen ein.

Die Objective-C-Name für den betreffenden Typ ist kein gültiger Objective-C-Bezeichner.

Verwenden Sie einen gültigen Objective-C-Bezeichner.

<a name="MT4169" />

### <a name="mt4169-failed-to-generate-a-pinvoke-wrapper-for-method-message"></a>MT4169: Fehler beim Generieren der P/Invoke-Wrapper für die {Methode}: {Message}

Xamarin.iOS konnte eine P/Invoke-Wrapper-Funktion für die genannten zu generieren.
Überprüfen Sie die gemeldeten Fehlermeldung für die zugrunde liegende Ursache an.

<a name="MT4170" />

### <a name="mt4170-the-registrar-cant-convert-from-managed-type-to-native-type-for-the-return-value-in-the-method-method"></a>MT4170: Aus "{verwalteten Typ}" die Registrierungsstelle kann nicht in "{native Type}" für den Rückgabewert in der Methode ' {-Methode} konvertiert werden.

Siehe die Beschreibung des Fehlers <a href="#MT4172">MT4172</a>.

<a name="MT4171" />

### <a name="mt4171-the-bindas-attribute-on-the-member-member-is-invalid-the-bindas-type-type-is-different-from-the-property-type-type"></a>MT4171: Das Attribut BindAs für das Element {Mitglied} ist ungültig: der Typ der BindAs {Type} unterscheidet sich von den Eigenschaftentyp {Type}.

Stellen Sie sicher, dass der Typ im BindAs-Attribut den Typ des Elements entspricht, die er angefügt ist.

<a name="MT4172" />

### <a name="mt4172-the-registrar-cant-convert-from-native-type-to-managed-type-for-the-parameter-parameter-name-in-the-method-method"></a>MT4172: Aus "{native Type}" die Registrierungsstelle kann nicht in "{verwalteten Type}" für den Parameter "{Parametername}" in der Methode ' {-Methode} konvertiert werden.

Konvertieren zwischen die genannten Typen unterstützt die Registrierungsstelle nicht.

Dies ist ein Fehler in Xamarin.iOS, wenn von Xamarin.iOS unter der betreffenden API bereitgestellt wird; Melden Sie Fehler auf [ http://bugzilla.xamarin.com ] [ 1].

Wenn Sie während der Entwicklung einer bindungsprojekt für eine native Bibliothek in diesen ausführen, sind wir öffnen, zum Hinzufügen der Unterstützung für neue Kombinationen von Typen. Wenn dies der Fall ist, melden Sie eine Anforderung zur Erweiterung ([http://bugzilla.xamarin.com][2]) in einem Test Case und überprüfen Sie es.

[1]: https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS
[2]: https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS&component=General&bug_severity=enhancement

## <a name="mt5xxx-gcc-and-toolchain-error-messages"></a>MT5xxx: GCC und toolkette Fehlermeldungen

### <a name="mt51xx-compilation"></a>MT51xx: Kompilierung

<!--
 MT5xxx GCC and toolchain
  MT51xx compilation
  -->

<a name="MT5101" />

### <a name="mt5101-missing--compiler-please-install-xcode-command-line-tools-component"></a>MT5101: Fehlendes ' *' Compiler. Installieren Sie Xcode "Befehlszeilentools"-Komponente

<a name="MT5102" />

### <a name="mt5102-failed-to-assemble-the-file--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT5102: Fehler beim Assemblieren der das "*". Melden Sie bitte einen Fehlerbericht an http://bugzilla.xamarin.com

<a name="MT5103" />

### <a name="mt5103-failed-to-compile-the-file--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT5103: Fehler beim Kompilieren Sie die Datei "*". Melden Sie bitte einen Fehlerbericht an http://bugzilla.xamarin.com

<a name="MT5104" />

### <a name="mt5104-could-not-find-neither-the--nor-the--compiler-please-install-xcode-command-line-tools-component"></a>MT5104: Konnte nicht gefunden werden weder die "\*'noch'\*" Compiler. Installieren Sie Xcode "Befehlszeilentools"-Komponente

<!-- 5105 is used by mmp -->

<a name="MT5106" />

### <a name="mt5106-could-not-compile-the-files--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT5106: Die Dateien nicht kompiliert werden kann ' *'. Melden Sie bitte einen Fehlerbericht an http://bugzilla.xamarin.com

In der Regel gibt einen Fehler in Xamarin.iOS. Melden Sie Fehler auf [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="mt52xx-linking"></a>MT52xx: Verknüpfen

<!--
  MT52xx linking
  -->

<a name="MT5201" />

### <a name="mt5201-native-linking-failed-please-review-the-build-log-and-the-user-flags-provided-to-gcc-"></a>MT5201: Fehler bei der systemeigenen verknüpfen. Überprüfen Sie das Buildprotokoll und die Benutzerflags bereitgestellt, um den Gcc: *

<a name="MT5202" />

### <a name="mt5202-native-linking-failed-please-review-the-build-log"></a>MT5202: Fehler bei der systemeigenen verknüpfen. Überprüfen Sie das Buildprotokoll an.

<a name="MT5203" />

### <a name="mt5203-native-linking-warning-"></a>MT5203: Verknüpfen die Warnung Native: *

<!--- 5204-5208 are not used -->

<a name="MT5209" />

### <a name="mt5209-native-linking-error-"></a>MT5209: Native Fehler verknüpfen: *

<a name="MT5210" />

### <a name="mt5210-native-linking-failed-undefined-symbol--please-verify-that-all-the-necessary-frameworks-have-been-referenced-and-native-libraries-are-properly-linked-in"></a>MT5210: Native Fehler verknüpfen nicht definiertes Symbol: *. Stellen Sie sicher, dass alle erforderlichen Frameworks auf die verwiesen wurde, und systemeigene Bibliotheken ordnungsgemäß verknüpft sind.

Dies geschieht, wenn es sich bei der native Linker ein Symbol findet, die an einer beliebigen Stelle auf die verwiesen wird. Es gibt verschiedene Gründe, warum dieser Fehler kann auftreten:

* Eine Drittanbieter-Bindung erfordert ein Framework, aber die Bindung gibt keinen in der `[LinkWith]` Attribut. Lösungen:
  - Wenn Sie der Autor der Drittanbieter-Bindung, oder Zugriff auf die Quelle haben, ändern Sie die Bindung des `[LinkWith]` Attribut, um das Framework enthalten muss:

            [LinkWith ("mylib.a", Frameworks = "SystemConfiguration")]

  - Wenn Sie die Drittanbieter-Bindung nicht ändern, Sie können manuell eine Verknüpfung mit des erforderlichen Frameworks durch Übergabe `-gcc_flags '-framework SystemFramework'` zu `mtouch` (Dies erfolgt durch die weitere Mtouch-Argumente in des Projekts iOS-Build-Optionsseite ändern. Beachten Sie, dass dies für jede Projektkonfiguration erfolgen muss).
* In einigen Fällen eine verwaltete Bindung besteht aus mehreren native Bibliotheken aus, und alle in den Bindungen enthalten sein müssen. Es ist möglich, mehr als eine native Bibliothek in jedem bindungsprojekt haben, damit die Lösung besteht darin, alle erforderlichen native Bibliotheken einfach auf das bindungsprojekt hinzufügen.</li>
* Eine verwaltete Bindung bezieht sich auf native Symbole, die in der nativen Bibliothek nicht vorhanden sind.
    Dies geschieht normalerweise, wenn eine Bindung bereits einiger Zeit seit ist, und der native Code, damit eine bestimmte native Klasse entweder entfernt oder umbenannt wurde, während die Bindung nicht aktualisiert wurde während dieser Zeit geändert wurde.
* P/Invoke bezieht sich auf ein systemeigenes-Symbol, das nicht vorhanden ist. Ab Xamarin.iOS 7.4 ein <a href="#MT5214">MT5214</a> Fehler wird für diesen Fall gemeldet werden (Weitere Informationen finden Sie unter "MT5214").
* Eine Drittanbieter-Bindung / Bibliothek mit C++ erstellt wurde, doch die Bindung ist nicht angegeben, in dessen `[LinkWith]` Attribut. Dies ist in der Regel recht einfach zu erkennen, da die Symbole verfügen, sind Sie ergänzte C++-Symbolen (ist ein allgemeines Beispiel `__ZNKSt9exception4whatEv`).
  - Wenn Sie der Autor der Drittanbieter-Bindung, oder Zugriff auf die Quelle haben, ändern Sie die Bindung des `[LinkWith]` Attribut der `IsCxx` Flag:

            [LinkWith ("mylib.a", IsCxx = true)]

  - Wenn die Drittanbieter-Bindung kann nicht geändert werden, oder Sie können manuell mit einer Drittanbieter-Bibliothek verknüpfen, können Sie das entsprechende Flag festlegen, durch Übergabe <code>-cxx</code> zum Mtouch (Dies erfolgt durch Bearbeiten der weitere Mtouch-Argumente des Projekts iOS Build-Optionen-Seite . Beachten Sie, dass dies für jede Projektkonfiguration erfolgen muss).

<a name="MT5211" />

### <a name="mt5211-native-linking-failed-undefined-objective-c-class--the-symbol--could-not-be-found-in-any-of-the-libraries-or-frameworks-linked-with-your-application"></a>MT5211: Native verknüpfen Fehler nicht definiert, Objective-C-Klasse: \*. Das Symbol "\*' konnte nicht gefunden werden, in die Bibliotheken oder Frameworks, die mit der Anwendung verknüpft.

Dies geschieht, wenn der native Linker eine Objective-C-Klasse, die an einer beliebigen Stelle auf die verwiesen wird, nicht finden kann. Es gibt verschiedene Gründe, die möglicherweise: dieselbe wie für [MT5210](#MT5210) und darüber hinaus:

* Eine Drittanbieter-Bindung ein Objective-C-Protokoll gebunden, aber es nicht kommentieren Sie ihn mit der <code>[Protocol]</code> Attribut im entsprechenden API-Definition. Lösungen:
  - Fügen Sie das fehlende `[Protocol]` Attribut:

              [BaseType (typeof (NSObject))]
              [Protocol] // Add this
              public interface MyProtocol
              {
              }

<a name="MT5212" />

### <a name="mt5212-native-linking-failed-duplicate-symbol-"></a>MT5212: Native Fehler verknüpfen doppelte Symbol: *.

Dies geschieht, wenn es sich bei der native Linker doppelte Symbole zwischen alle nativen Bibliotheken auftritt. Nach dieser Fehler möglicherweise eine oder mehrere [MT5213](#MT5213) Fehlern mit Assistenten für den Speicherort für jedes Vorkommen des Symbols. Mögliche Ursachen für diesen Fehler zu erhalten:

* Die gleiche native Bibliothek ist zweimal enthalten.
* Zwei unterschiedliche systemeigene Bibliotheken auftreten, um die gleichen Symbole zu definieren.
* Eine native Bibliothek wurde nicht ordnungsgemäß erstellt, und das gleiche Symbol mehr als einmal enthält.
  Sie können dies nachprüfen, mit den folgenden Satz von Befehlen über ein Terminal (ersetzen Sie "I386", durch x86_64/armv7/armv7s/arm64 entsprechend der Architektur, die Sie für erstellen):

        # Native libraries are usually fat libraries, containing binary code for
        # several architectures in the same file. First we extract the binary
        # code for the architecture we're interested in.
        lipo libNative.a -thin i386 -output libNative.i386.a

        # Now query the native library for the duplicated symbol.
        nm libNative.i386.a | fgrep 'SYMBOL'

        # You can also list the object files inside the native library.
        # In most cases this will reveal duplicated object files.
        ar -t libNative.i386.a

  Es gibt einige Möglichkeiten zum Beheben dieses Problems:

  - Fordern Sie an, dass der Anbieter der nativen Bibliothek korrigieren, und geben Sie die aktualisierte Version.
  - Reparieren Sie sie selbst durch das Entfernen von zusätzlichen Objektdateien (dies funktioniert nur, wenn das Problem tatsächlich doppelte Objektdateien ist)

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

  - Bitten Sie den Linker an, nicht verwendeten Code entfernt. Xamarin.iOS tun automatisch dies, wenn alle der folgenden Bedingungen erfüllt sind:
    - Alle Drittanbieter-Bindungen `[LinkWith]` Attribute erstellen aktiviert haben:

            [assembly: LinkWith ("libNative.a", SmartLink = true)]

    - Keine `-gcc_flags` an Mtouch (in dem weitere Mtouch-Argumente-Feld der iOS-Buildoptionen des Projekts) übergeben wird.
    - Es ist auch möglich, bitten Sie den Linker direkt an das Entfernen nicht verwendeten Codes durch Hinzufügen von `-gcc_flags -dead_strip` auf die weitere Mtouch-Argumente des Projekts, IOS-Buildoptionen.

<a name="MT5213" />

### <a name="mt5213-duplicate-symbol-in--location-related-to-previous-error"></a>MT5213: Doppelte Symbol im: * (Speicherort in Bezug zum vorherigen Fehler)

Dieser Fehler wird gemeldet, nur zusammen mit [MT5212](#MT5212). Informieren Sie sich [MT5212](#MT5212) für Weitere Informationen.

<a name="MT5214" />

### <a name="mt5214-native-linking-failed-undefined-symbol--this-symbol-was-referenced-the-managed-member--please-verify-that-all-the-necessary-frameworks-have-been-referenced-and-native-libraries-linked"></a>MT5214: Native Fehler verknüpfen nicht definiertes Symbol: *. Dieses Symbol verwiesen wurde, wird das verwaltete Element *. Stellen Sie sicher, dass alle erforderlichen Frameworks, auf die verwiesen wird und native Bibliotheken verknüpft wurden.

Dieser Fehler wird gemeldet, wenn der verwaltete Code P/Invoke auf eine native Methode enthält, die nicht vorhanden ist. Zum Beispiel:

```csharp
using System.Runtime.InteropServices;
class MyImports {
   [DllImport ("__Internal")]
   public static extern void MyInexistentFunc ();
}
```

Es gibt einige mögliche Lösungen:

  -  Entfernen Sie die betreffende P/Invokes, aus dem Quellcode.
  -  Aktivieren des verwalteten Linkers für alle Assemblys (dies in iOS-Buildoptionen, des Projekts erfolgt durch Festlegen von "Linkerverhalten" auf "Alle Assemblys"). Dadurch wird effektiv alle der P/Invokes verwenden nicht aus der app entfernen (automatisch anstatt manuell wie vorherigen Punkt). Der Nachteil besteht darin, dass dies, Ihre simulatorbuilds etwas langsamer veranlasst, und er Ihre app unterbrochen wird, kann wenn Reflektion verwendet wird – Weitere Informationen zu den Linker finden [hier](~/ios/deploy-test/linker.md) )
  -  Erstellen Sie eine zweite native Bibliothek, die die fehlende systemeigene Symbole enthält. Beachten Sie, dass dies nur eine problemumgehung ist (Wenn Sie versuchen versehentlich, diese Funktionen aufzurufen, Ihre app stürzt ab).

<a name="MT5215" />

### <a name="mt5215-references-to--might-require-additional--frameworkxxx-or--lxxx-instructions-to-the-native-linker"></a>MT5215: Verweise auf "*" möglicherweise zusätzliche - Framework = XXX "oder" lXXX - Anweisungen, um dem nativen Linker

Dies ist eine Warnung, die angibt, dass P/Invoke erkannt wurde, auf die betreffende Bibliothek verweisen, aber die app ist nicht mit ihm verknüpfen.

<a name="MT5216" />

### <a name="mt5216-native-linking-failed-for--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT5216: Verknüpfen von systemeigenen für Fehler *. Melden Sie bitte einen Fehlerbericht an http://bugzilla.xamarin.com

Dieser Fehler wird gemeldet, wenn Sie die Ausgabe der AOT-Compiler zu verknüpfen.

Dieser Fehler wahrscheinlich gibt einen Fehler in Xamarin.iOS an. Melden Sie bitte einen Fehlerbericht an [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT5217" />

### <a name="mt5217-native-linking-possibly-failed-because-the-linker-command-line-was-too-long--characters"></a>MT5217: Verknüpfen von systemeigenen möglicherweise fehlgeschlagen, da es sich bei der Befehlszeile des Linkers zu lang war (* Zeichen).

Fehler beim Verknüpfen von systemeigenen, und es ist möglich, Fehler aufgetreten, weil der Linker-Befehl zu lang war.

Xamarin.iOS-Projekte werden oft native Symbole dynamisch, verweisen die bedeutet, dass der native Linker diese native Symbole während des systemeigenen Verknüpfungsvorgangs ist entfernen kann, da es sich bei der native Linker nicht sieht, dass diese Symbole verwendet werden.

In der Regel den nativen Linker, behalten Sie diese Symbole, die mit Xamarin.iOS fragt die `-u symbol` Linker-Flags, aber wenn stehen Ihnen viele solcher Symbole, die gesamte Befehlszeile überschreiten die maximale Länge der Befehlszeile wie vom Betriebssystem angegeben.

Es gibt einige mögliche Quellen für diese dynamische Symbole aus:

* P/Invokes in Methoden in statisch verknüpfte Bibliotheken (wobei der Name der Dll ist `__Internal` in das DllImport-Attribut `[DllImport ("__Internal")]`).
* Verweise auf Speicheradressen in statisch verknüpfte Bibliotheken aus bindungsprojekte Feld (`[Field]` Attribute).
* Objective-C-Klassen in statisch verknüpfte Bibliotheken aus bindungsprojekte (wenn inkrementelle Builds zu verwenden oder wenn die statische Registrierungsstelle nicht verwenden) verwiesen wird.

Folgende Lösungen sind möglich:

* Aktivieren Sie den verwalteten Linker (sofern möglich, für alle Assemblys, statt nur SDK-Assemblys). Dies kann genügend Quellen entfernen, für die dynamische Symbole, damit der Linker Befehlszeile ist nicht größer als der maximal.
* Reduzieren Sie die Anzahl der P/Invokes, Feldverweisen und/oder Objective-C-Klassen.
* Schreiben Sie die dynamische Symbole ein, um kürzere Namen haben.
* Übergeben Sie `-dlsym:false` als eine weitere Mtouch-Argument des Projekts, IOS-Buildoptionen. Mit dieser Option Xamarin.iOS ein systemeigenen Verweises in der AOT-kompiliertem Code generiert, und bitten Sie den Linker an, dieses Symbol halten müssen nicht. Aber dies funktioniert nur für Gerät erstellt und es bewirkt Linker-Fehler auf, wenn P/Invokes vorhanden sind, Funktionen, die in der statischen Bibliothek nicht vorhanden sind.
* Übergeben Sie `--dynamic-symbol-mode=code` als eine weitere Mtouch-Argumente des Projekts, IOS-Buildoptionen. Mit dieser Option generiert Xamarin.iOS zusätzlichen systemeigenen Code, der auf diese Symbole werden nicht aufgefordert den nativen Linker, behalten Sie diese Symbole, die Verwendung von Befehlszeilenargumenten verweist. Der Nachteil dieses Ansatzes ist, dass sie die Größe der ausführbaren Datei etwas erhöht wird.
* Aktivieren Sie die statische Registrierungsstelle durch Übergabe `--registrar:static` als eine weitere Mtouch-Argument des Projekts, IOS Build "Optionen" (bei simulatorbuilds, da die statische Registrierungsstelle bereits die Standardeinstellung ist für Geräte-builds,). Die statische Registrierungsstelle generiert Code, Objective-C-Klassen statisch verweist, daher keine Notwendigkeit besteht, die den nativen Linker, behalten Sie diese Klassen stellen.
* Inkrementelle Builds (für Geräte-Builds) zu deaktivieren. Bei inkrementellen Builds aktiviert sind, wird nicht der von der statischen Registrierungsstelle generierte Code von dem nativen Linker, berücksichtigt werden, d. h., die von Xamarin.iOS immer noch den Linker an beibehalten anfordern, muss die Objective-C-Klassen verwiesen. So deaktivieren inkrementelle Builds wird verhindert, dass diese Anforderung.

<a name="MT5218" />

### <a name="mt5218-cant-ignore-the-dynamic-symbol-symbol---ignore-dynamic-symbolsymbol-because-it-was-not-detected-as-a-dynamic-symbol"></a>MT5218: Kann nicht das dynamische Symbol {Symbols} ignorieren (--ignorieren-Dynamic-Symbol = {Symbols}), da er nicht als Symbol für die dynamische erkannt wurde.

Das Befehlszeilenargument `--ignore-dynamic-symbol=symbol` übergeben wurde, dieses Symbol ist jedoch kein Symbol, das als Symbol für die dynamische erkannt wurde, die manuell beibehalten werden sollen.

Es gibt zwei Hauptgründe dafür:

* Der Symbolname ist falsch.
    * Stellen Sie keinen Unterstrich, um den Symbolnamen an.
    * Das Symbol für Objective-C-Klassen ist `OBJC_CLASS_$_<classname>`.
* Das Symbol ist richtig, aber es ist ein Symbol, das bereits normales beibehalten wird (einige build Optionen bewirkt, dass die genaue Liste der dynamischen Symbole variieren).

### <a name="mt53xx-other-tools"></a>MT53xx: Sonstige Tools

<!--
  MT53xx other tools
  -->

<a name="MT5301" />

### <a name="mt5301-missing-strip-tool-please-install-xcode-command-line-tools-component"></a>MT5301: Fehlendes 'entfernen'-Tool an. Installieren Sie Xcode "Befehlszeilentools"-Komponente

<a name="MT5302" />

### <a name="mt5302-missing-dsymutil-tool-please-install-xcode-command-line-tools-component"></a>MT5302: Fehlende 'Dsymutil'-Tool. Installieren Sie Xcode "Befehlszeilentools"-Komponente

<a name="MT5303" />

### <a name="mt5303-failed-to-generate-the-debug-symbols-dsym-directory-please-review-the-build-log"></a>MT5303: Fehler beim Generieren von der Debugsymbolen (dSYM-Verzeichnis). Überprüfen Sie das Buildprotokoll an.

Fehler beim Ausführen von Dsymutil auf die letzte App-Verzeichnis, um die Debugsymbole zu erstellen. Finden Sie in das Buildprotokoll Dsymutil der Ausgabe angezeigt, und sehen, wie es behoben werden kann.

<a name="MT5304" />

### <a name="mt5304-failed-to-strip-the-final-binary-please-review-the-build-log"></a>MT5304: Fehler bei, um die endgültige Binärdatei zu entfernen. Überprüfen Sie das Buildprotokoll an.

Fehler beim Ausführen des Tools 'entfernen', um Informationen zum Debuggen von der Anwendung zu entfernen.

<a name="MT5305" />

### <a name="mt5305-missing-lipo-tool-please-install-xcode-command-line-tools-component"></a>MT5305: Fehlende 'Lipo'-Tool. Installieren Sie Xcode "Befehlszeilentools"-Komponente

<a name="MT5306" />

### <a name="mt5306-failed-to-create-the-a-fat-library-please-review-the-build-log"></a>MT5306: Fehler beim Erstellen der fat-Bibliothek. Überprüfen Sie das Buildprotokoll an.

Fehler beim Ausführen des Tools "Lipo". Finden Sie in das Buildprotokoll an, um Fehler von "Lipo" anzuzeigen.

<a name="MT5307" />

### <a name="mt5307-failed-to-sign-the-executable-please-review-the-build-log"></a>MT5307: Fehler beim Signieren der ausführbaren Datei. Überprüfen Sie das Buildprotokoll an.

Fehler beim Signieren der Anwendungs. Finden Sie in das Buildprotokoll an, um Fehler von "Codesign" anzuzeigen.

<!-- 5308 is used by mmp -->
<!-- 5309 is used by mmp -->
<!-- 5310 is used by mmp -->

## <a name="mt6xxx-mtouch-internal-tools-error-messages"></a>MT6xxx: interne Mtouch-tools: Fehlermeldungen

### <a name="mt600x-stripper"></a>MT600x: Entfernen

<!--
 MT6xxx mtouch internal tools
  MT600x Stripper
  -->

<a name="MT6001" />

### <a name="mt6001-running-version-of-cecil-doesnt-support-assembly-stripping"></a>MT6001: Cecil ausgeführte Version unterstützt keine Assembly entfernen

<a name="MT6002" />

### <a name="mt6002-could-not-strip-assembly-"></a>MT6002: Assembly konnte nicht entfernt `*`.

Fehler beim Entfernen von verwalteten Codes (IL-Code entfernen) aus den Assemblys in der Anwendung.

<a name="MT6003" />

### <a name="mt6003-unauthorizedaccessexception-message"></a>MT6003: [Message UnauthorizedAccessException]

Ein Security Fehler beim ohne Debugsymbole von der Anwendung.

## <a name="mt7xxx-msbuild-error-messages"></a>MT7xxx: MSBuild-Fehlermeldungen

<!--
 MT7xxx msbuild errors
  -->

<a name="MT7001" />

### <a name="mt7001-could-not-resolve-host-ips-for-wifi-debugger-settings"></a>MT7001: Host-IP-Adressen für die WiFi-Debuggereinstellungen konnte nicht aufgelöst werden.

*MSBuild-Aufgabe: DetectDebugNetworkConfigurationTaskBase*

Schritte zur Problembehandlung:

- Versuchen Sie es ausführen `csharp -e 'System.Net.Dns.GetHostEntry (System.Net.Dns.GetHostName ()).AddressList'` (, die Informationen erhalten Sie eine IP-Adresse und nicht um einen Fehler offensichtlich).
- Versuchen Sie, führen Sie "Ping \`Hostname\`" das bietet Ihnen möglicherweise weitere Informationen wie z. B.: `cannot resolve MyHost.local: Unknown host`

In einigen Fällen ist es ein "Lokales Netzwerk"-Problem, und es gelöst werden, indem Sie hinzufügen, unbekannte `127.0.0.1   MyHost.local` in `/etc/hosts`.

<a name="MT7002" />

### <a name="mt7002-this-machine-does-not-have-any-network-adapters-this-is-required-when-debugging-or-profiling-on-device-over-wifi"></a>MT7002: Dieser Computer muss sich nicht auf allen Netzwerkadaptern aus. Dies ist erforderlich, beim Debuggen oder die profilerstellung auf Gerät über WLAN.

*MSBuild-Aufgabe: DetectDebugNetworkConfigurationTaskBase*

<a name="MT7003" />

### <a name="mt7003-the-app-extension--does-not-contain-an-infoplist"></a>MT7003: Die App-Erweiterung "*" enthält eine Datei "Info.plist" nicht.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7004" />

### <a name="mt7004-the-app-extension--does-not-specify-a-cfbundleidentifier"></a>MT7004: Die App-Erweiterung "*" gibt keine CFBundleIdentifier.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7005" />

### <a name="mt7005-the-app-extension--does-not-specify-a-cfbundleexecutable"></a>MT7005: Die App-Erweiterung "*" gibt keine CFBundleExecutable.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7006" />

### <a name="mt7006-the-app-extension--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a>MT7006: Die App-Erweiterung "\*' hat eine ungültige CFBundleIdentifier (\*), beginnt nicht mit der Haupt-app-Bundle CFBundleIdentifier (*).

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7007" />

### <a name="mt7007-the-app-extension--has-a-cfbundleidentifier--that-ends-with-the-illegal-suffix-key"></a>MT7007: Die App-Erweiterung "\*" verfügt über eine CFBundleIdentifier (\*) endet mit dem unzulässige Suffix "Key".

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7008" />

### <a name="mt7008-the-app-extension--does-not-specify-a-cfbundleshortversionstring"></a>MT7008: Die App-Erweiterung "*" gibt keine CFBundleShortVersionString.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7009" />

### <a name="mt7009-the-app-extension--has-an-invalid-infoplist-it-does-not-contain-an-nsextension-dictionary"></a>MT7009: Die App-Erweiterung "*" hat eine ungültige Datei "Info.plist": Es enthält kein NSExtension-Wörterbuch.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7010" />

### <a name="mt7010-the-app-extension--has-an-invalid-infoplist-the-nsextension-dictionary-does-not-contain-an-nsextensionpointidentifier-value"></a>MT7010: Die App-Erweiterung "*" hat eine ungültige Datei "Info.plist": das Wörterbuch NSExtension einen NSExtensionPointIdentifier Wert nicht enthält.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7011" />

### <a name="mt7011-the-watchkit-extension--has-an-invalid-infoplist-the-nsextension-dictionary-does-not-contain-an-nsextensionattributes-dictionary"></a>MT7011: WatchKit-Erweiterung "*" hat eine ungültige Datei "Info.plist": das NSExtension-Wörterbuch enthält kein NSExtensionAttributes-Wörterbuch.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7012" />

### <a name="mt7012-the-watchkit-extension--does-not-have-exactly-one-watch-app"></a>MT7012: WatchKit-Erweiterung "*" verfügt nicht über genau eine Watch-app.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7013" />

### <a name="mt7013-the-watchkit-extension--has-an-invalid-infoplist-uirequireddevicecapabilities-must-contain-the-watch-companion-capability"></a>MT7013: WatchKit-Erweiterung "*" hat eine ungültige Datei "Info.plist": "Uirequireddevicecapabilities" muss die Funktion "Watch-Companion" enthalten.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7014" />

### <a name="mt7014-the-watch-app--does-not-contain-an-infoplist"></a>MT7014: Die Watch-App "*" enthält eine Datei "Info.plist" nicht.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7015" />

### <a name="mt7015-the-watch-app--does-not-specify-a-cfbundleidentifier"></a>MT7015: Die Watch-App "*" gibt keine CFBundleIdentifier.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7016" />

### <a name="mt7016-the-watch-app--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a>MT7016: Die Watch-App "\*' hat eine ungültige CFBundleIdentifier (\*), beginnt nicht mit der Haupt-app-Bundle CFBundleIdentifier (*).

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7017" />

### <a name="mt7017-the-watch-app--does-not-have-a-valid-uidevicefamily-value-expected-watch-4-but-found--"></a>MT7017: Die Watch-App "\*" verfügt nicht über einen gültigen UIDeviceFamily-Wert. Erwartete "sehen Sie sich (4)" wurde erwartet, aber "\* (*)".

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7018" />

### <a name="mt7018-the-watch-app--does-not-specify-a-cfbundleexecutable"></a>MT7018: Die Watch-App "*" gibt keine CFBundleExecutable

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7019" />

### <a name="mt7019-the-watch-app--has-an-invalid-wkcompanionappbundleidentifier-value--it-does-not-match-the-main-app-bundles-cfbundleidentifier-"></a>MT7019: Die Watch-App "\*" weist einen ungültigen WKCompanionAppBundleIdentifier-Wert ("\*"), entspricht nicht dem Haupt-app-Bündel CFBundleIdentifier ('* ').

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7020" />

### <a name="mt7020-the-watch-app--has-an-invalid-infoplist-the-wkwatchkitapp-key-must-be-present-and-have-a-value-of-true"></a>MT7020: Die Watch-App "*" hat eine ungültige Datei "Info.plist": der WKWatchKitApp Schlüssel muss vorhanden sein, und weisen den Wert 'True'.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7021" />

### <a name="mt7021-the-watch-app--has-an-invalid-infoplist-the-lsrequiresiphoneos-key-must-not-be-present"></a>MT7021: Die Watch-App "*" hat eine ungültige Datei "Info.plist": der LSRequiresIPhoneOS Schlüssel dürfen nicht vorhanden sein.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7022" />

### <a name="mt7022-the-watch-app--does-not-contain-a-watch-extension"></a>MT7022: Die Watch-App "*" enthält keine Watch-Erweiterung.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7023" />

### <a name="mt7023-the-watch-extension--does-not-contain-an-infoplist"></a>MT7023: Der Watch-Erweiterung "*" enthält eine Datei "Info.plist" nicht.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7024" />

### <a name="mt7024-the-watch-extension--does-not-specify-a-cfbundleidentifier"></a>MT7024: Der Watch-Erweiterung "*" gibt keine CFBundleIdentifier.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7025" />

### <a name="mt7025-the-watch-extension--does-not-specify-a-cfbundleexecutable"></a>MT7025: Der Watch-Erweiterung "*" gibt keine CFBundleExecutable.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7026" />

### <a name="mt7026-the-watch-extension--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a>MT7026: Der Watch-Erweiterung "\*' hat eine ungültige CFBundleIdentifier (\*), beginnt nicht mit der Haupt-app-Bundle CFBundleIdentifier (*).

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7027" />

### <a name="mt7027-the-watch-extension--has-a-cfbundleidentifier--that-ends-with-the-illegal-suffix-key"></a>MT7027: Der Watch-Erweiterung "\*" verfügt über eine CFBundleIdentifier (\*) endet mit dem unzulässige Suffix "Key".

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7028" />

### <a name="mt7028-the-watch-extension--has-an-invalid-infoplist-it-does-not-contain-an-nsextension-dictionary"></a>MT7028: Der Watch-Erweiterung "*" hat eine ungültige Datei "Info.plist": Es enthält kein NSExtension-Wörterbuch.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7029" />

### <a name="mt7029-the-watch-extension--has-an-invalid-infoplist-the-nsextensionpointidentifier-must-be-comapplewatchkit"></a>MT7029: Der Watch-Erweiterung "*" hat eine ungültige Datei "Info.plist": die NSExtensionPointIdentifier muss "com.apple.watchkit" sein.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7030" />

### <a name="mt7030-the-watch-extension--has-an-invalid-infoplist-the-nsextension-dictionary-must-contain-nsextensionattributes"></a>MT7030: Der Watch-Erweiterung "*" hat eine ungültige Datei "Info.plist": das Wörterbuch NSExtension muss NSExtensionAttributes enthalten.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7031" />

### <a name="mt7031-the-watch-extension--has-an-invalid-infoplist-the-nsextensionattributes-dictionary-must-contain-a-wkappbundleidentifier"></a>MT7031: Der Watch-Erweiterung "*" hat eine ungültige Datei "Info.plist": das Wörterbuch NSExtensionAttributes muss eine WKAppBundleIdentifier enthalten.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7032" />

### <a name="mt7032-the-watchkit-extension--has-an-invalid-infoplist-uirequireddevicecapabilities-should-not-contain-the-watch-companion-capability"></a>MT7032: WatchKit-Erweiterung "*" hat eine ungültige Datei "Info.plist": "Uirequireddevicecapabilities" sollte nicht die Funktion "Watch-Companion" enthalten.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7033" />

### <a name="mt7033-the-watch-app--does-not-contain-an-infoplist"></a>MT7033: Die Watch-App "*" enthält eine Datei "Info.plist" nicht.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7034" />

### <a name="mt7034-the-watch-app--does-not-specify-a-cfbundleidentifier"></a>MT7034: Die Watch-App "*" gibt keine CFBundleIdentifier.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7035" />

### <a name="mt7035-the-watch-app--does-not-have-a-valid-uidevicefamily-value-expected--but-found--"></a>MT7035: Die Watch-App "\*" verfügt nicht über einen gültigen UIDeviceFamily-Wert. Erwartet '\*", gefunden wurde jedoch"\* (\*) ".

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7036" />

### <a name="mt7036-the-watch-app--does-not-specify-a-cfbundleexecutable"></a>MT7036: Die Watch-App "*" gibt keine CFBundleExecutable.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7037" />

### <a name="mt7037-the-watchkit-extension-extensionname-has-an-invalid-wkappbundleidentifier-value--it-does-not-match-the-watch-apps-cfbundleidentifier-"></a>MT7037: Das WatchKit-Erweiterung "{ExtensionName}" weist einen ungültigen WKAppBundleIdentifier-Wert ('\*"), entspricht nicht der Watch-App-CFBundleIdentifier ("\*").

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7038" />

### <a name="mt7038-the-watch-app--has-an-invalid-infoplist-the-wkcompanionappbundleidentifier-must-exist-and-must-match-the-main-app-bundles-cfbundleidentifier"></a>MT7038: Die Watch-App "*" hat eine ungültige Datei "Info.plist": die WKCompanionAppBundleIdentifier muss vorhanden sein und muss mit der Haupt-app-Bundle CFBundleIdentifier übereinstimmen.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7039" />

### <a name="mt7039-the-watch-app--has-an-invalid-infoplist-the-lsrequiresiphoneos-key-must-not-be-present"></a>MT7039: Die Watch-App "*" hat eine ungültige Datei "Info.plist": der LSRequiresIPhoneOS Schlüssel dürfen nicht vorhanden sein.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7040" />

### <a name="mt7040-the-app-bundle-appbundlepath-does-not-contain-an-infoplist"></a>MT7040: Das app-Bundle {AppBundlePath} ist eine Datei "Info.plist" nicht enthalten.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7041" />

### <a name="mt7041-main-infoplist-path-does-not-specify-a-cfbundleidentifier"></a>MT7041: Main "Info.plist" Pfad wird nicht mit einem CFBundleIdentifier angegeben.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7042" />

### <a name="mt7042-main-infoplist-path-does-not-specify-a-cfbundleexecutable"></a>MT7042: Main "Info.plist" Pfad wird nicht mit einem CFBundleExecutable angegeben.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7043" />

### <a name="mt7043-main-infoplist-path-does-not-specify-a-cfbundlesupportedplatforms"></a>MT7043: Main "Info.plist" Pfad wird nicht mit einem CFBundleSupportedPlatforms angegeben.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7044" />

### <a name="mt7044-main-infoplist-path-does-not-specify-a-uidevicefamily"></a>MT7044: Main "Info.plist" Pfad wird nicht mit einem UIDeviceFamily angegeben.

*MSBuild-Aufgabe: ValidateAppBundleTaskBase*

<a name="MT7045" />

### <a name="mt7045-unrecognized-format-"></a>MT7045: Unbekanntes Format: *.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

Wo * sind möglich:

- Zeichenfolge
- array
- dict
- bool
- Reelle
- Ganze Zahl
- date
- Daten

<a name="MT7046" />

### <a name="mt7046-add-entry--incorrectly-specified"></a>MT7046: Hinzufügen: Eintrag, *, falsch angegeben.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7047" />

### <a name="mt7047-add-entry--contains-invalid-array-index"></a>MT7047: Hinzufügen: Eintrag, *, ungültigen Arrayindex enthält.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7048" />

### <a name="mt7048-add--entry-already-exists"></a>MT7048: Fügen Sie hinzu: * Eintrag bereits vorhanden ist.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7049" />

### <a name="mt7049-add-cant-add-entry--to-parent"></a>MT7049: Hinzufügen: Kann nicht hinzugefügt Eintrag, *, zum übergeordneten Element.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7050" />

### <a name="mt7050-delete-cant-delete-entry--from-parent"></a>MT7050: Löschen: Eintrag kann nicht gelöscht werden *, vom übergeordneten Element.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7051" />

### <a name="mt7051-delete-entry--contains-invalid-array-index"></a>MT7051: Löschen: Eintrag, *, ungültigen Arrayindex enthält.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7052" />

### <a name="mt7052-delete-entry--does-not-exist"></a>MT7052: Löschen: Eintrag, *, ist nicht vorhanden.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7053" />

### <a name="mt7053-import-entry--incorrectly-specified"></a>MT7053: Import: Eintrag, *, falsch angegeben.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7054" />

### <a name="mt7054-import-entry--contains-invalid-array-index"></a>MT7054: Import: Eintrag, *, ungültigen Arrayindex enthält.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7055" />

### <a name="mt7055-import-error-reading-file-"></a>MT7055: Import: Fehler beim Lesen der Datei: *.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7056" />

### <a name="mt7056-import-cant-add-entry--to-parent"></a>MT7056: Import: Kann nicht hinzugefügt Eintrag, *, zum übergeordneten Element.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7057" />

### <a name="mt7057-merge-cant-add-array-entries-to-dict"></a>MT7057: Die Mergereplikation: Arrayeinträge kann nicht auf dict. hinzugefügt werden.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7058" />

### <a name="mt7058-merge-specified-entry-must-be-a-container"></a>MT7058: Die Mergereplikation: Angegebene Eintrag muss ein Container sein.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7059" />

### <a name="mt7059-merge-entry--contains-invalid-array-index"></a>MT7059: Die Mergereplikation: Eintrag, *, ungültigen Arrayindex enthält.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7060" />

### <a name="mt7060-merge-entry--does-not-exist"></a>MT7060: Die Mergereplikation: Eintrag, *, ist nicht vorhanden.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7061" />

### <a name="mt7061-merge-error-reading-file-"></a>MT7061: Die Mergereplikation: Fehler beim Lesen der Datei: *.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7062" />

### <a name="mt7062-set-entry--incorrectly-specified"></a>MT7062: Legen Sie Folgendes fest: Eintrag, *, falsch angegeben.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7063" />

### <a name="mt7063-set-entry--contains-invalid-array-index"></a>MT7063: Legen Sie Folgendes fest: Eintrag, *, ungültigen Arrayindex enthält.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7064" />

### <a name="mt7064-set-entry--does-not-exist"></a>MT7064: Legen Sie Folgendes fest: Eintrag, *, ist nicht vorhanden.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7065" />

### <a name="mt7065-unknown-propertylist-editor-action-"></a>MT7065: Unbekannte PropertyList-Editor-Aktion: *.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7066" />

### <a name="mt7066-error-loading--"></a>MT7066: Fehler beim Laden von ' *': *.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

<a name="MT7067" />

### <a name="mt7067-error-saving--"></a>MT7067: Fehler beim Speichern ' *': *.

*MSBuild-Aufgabe: PropertyListEditorTaskBase*

## <a name="mt8xxx-runtime-error-messages"></a>MT8xxx: Laufzeit-Fehlermeldungen

<!--
 MT8xxx runtime
  MT800x misc
  -->

<a name="MT8001" />

### <a name="mt8001-version-mismatch-between-the-native-xamarinios-runtime-and-monotouchdll-please-reinstall-xamarinios"></a>MT8001: Versionskonflikt zwischen dem systemeigenen Xamarin.iOS-Runtime und monotouch.dll. Installieren Sie Xamarin.iOS erneut.

<a name="MT8002" />

### <a name="mt8002-could-not-find-the-method--in-the-type-"></a>MT8002: Die Methode wurde nicht gefunden "\*"im Typ"\*".

<a name="MT8003" />

### <a name="mt8003-failed-to-find-the-closed-generic-method--on-the-type-"></a>MT8003: Fehler beim Suchen der geschlossenen generischen Methode "\*'für den Typ"\*".

<a name="MT8004" />

### <a name="mt8004-cannot-create-an-instance-of--for-the-native-object-0x-of-type--because-another-instance-already-exists-for-this-native-object-of-type-"></a>MT8004: Eine Instanz kann nicht erstellt * für das systemeigene Objekt 0 X * (des Typs ' *'), da eine andere Instanz für diese systemeigene Objekt bereits vorhanden ist (des Typs *).

<a name="MT8005" />

### <a name="mt8005-wrapper-type--is-missing-its-native-objectivec-class-"></a>MT8005: Der Wrappertyp "\*"fehlt die systemeigene ObjectiveC-Klasse"\*".

<a name="MT8006" />

### <a name="mt8006-failed-to-find-the-selector--on-the-type-"></a>MT8006: Fehler beim Suchen des Selektors "\*'für den Typ"\*"

<a name="MT8007" />

### <a name="mt8007-cannot-get-the-method-descriptor-for-the-selector--on-the-type--because-the-selector-does-not-correspond-to-a-method"></a>MT8007: Der Methodendeskriptor kann nicht abgerufen werden, für die Auswahl "\*'für den Typ"\*", da es sich bei die Auswahl einer Methode nicht entsprechen

<a name="MT8008" />

### <a name="mt8008-the-loaded-version-of-xamariniosdll-was-compiled-for--bits-while-the-process-is--bits-please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8008: Die geladene Version des Xamarin.iOS.dll kompiliert wurde, für die * bits, die während der Prozess ist * Bits. Melden Sie Fehler auf http://bugzilla.xamarin.com.

Dies gibt an, dass etwas im Buildprozess nicht stimmt. Melden Sie Fehler auf [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT8009" />

### <a name="mt8009-unable-to-locate-the-block-to-delegate-conversion-method-for-the-method-s-parameter--please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8009: Wurde nicht gefunden der Block, um die Konvertierung für die Methode Delegatmethode *.*" s Parameter #*. Melden Sie Fehler auf http://bugzilla.xamarin.com.

Dies gibt an, dass eine API ordnungsgemäß gebunden war nicht. Ist dies eine API, die von Xamarin verfügbar gemacht werden, melden Sie einen Fehler ein, in unserem Bugzilla ([http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)), sofern sie eine Drittanbieter-Bindung, wenden Sie sich an den Hersteller.

<a name="MT8010" />

### <a name="mt8010-native-type-size-mismatch-between-xamariniosmacdll-and-the-executing-architecture-xamariniosmacdll-was-built-for--bit-while-the-current-process-is--bit"></a>MT8010: Systemeigener Typ-Konflikt zwischen Xamarin. [iOS | Mac-DLL und die ausgeführten-Architektur. Xamarin. [iOS | Mac-DLL erstellt wurde, für die *-bit und während der aktuelle Prozess ist *-Bit.

Dies gibt an, dass etwas im Buildprozess nicht stimmt. Melden Sie Fehler auf [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT8011" />

### <a name="mt8011-unable-to-locate-the-delegate-to-block-conversion-attribute-delegateproxy-for-the-return-value-for-the-method--please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8011: Der Delegat, der Block Konvertierung-Attribut ([DelegateProxy]) für den Rückgabewert für die Methode gefunden *.*. Melden Sie Fehler auf http://bugzilla.xamarin.com.

Xamarin.iOS konnte eine erforderliche Methode zur Laufzeit (Konvertieren einen Delegaten in einen Block) zu finden.

In der Regel gibt einen Fehler in Xamarin.iOS. Melden Sie Fehler auf [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT8012" />

### <a name="mt8012-invalid-delegateproxyattribute-for-the-return-value-for-the-method--delegatetype-is-null-please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8012: Ungültige DelegateProxyAttribute für den Rückgabewert für die Methode *.*: Delegattyp ist null. Melden Sie Fehler auf http://bugzilla.xamarin.com.

Das DelegateProxy-Attribut für die betreffende Methode ist ungültig.

In der Regel gibt einen Fehler in Xamarin.iOS. Melden Sie Fehler auf [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT8013" />

### <a name="mt8013-invalid-delegateproxyattribute-for-the-return-value-for-the-method--delegatetype-2-specifies-a-type-without-a-handler-field-please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8013: Ungültige DelegateProxyAttribute für den Rückgabewert für die Methode *.*: Delegattyp ({2}) gibt einen Typ ohne ein 'Handler'-Feld. Melden Sie Fehler auf http://bugzilla.xamarin.com.

Das DelegateProxy-Attribut für die betreffende Methode ist ungültig.

In der Regel gibt einen Fehler in Xamarin.iOS. Melden Sie Fehler auf [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT8014" />

### <a name="mt8014-invalid-delegateproxyattribute-for-the-return-value-for-the-method--the-delegatetypes-2-handler-field-is-null-please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8014: Ungültige DelegateProxyAttribute für den Rückgabewert für die Methode *.*: Der Delegattyps ({2}) 'Handler'-Feld ist null. Melden Sie Fehler auf http://bugzilla.xamarin.com.

Das DelegateProxy-Attribut für die betreffende Methode ist ungültig.

In der Regel gibt einen Fehler in Xamarin.iOS. Melden Sie Fehler auf [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT8015" />

### <a name="mt8015-invalid-delegateproxyattribute-for-the-return-value-for-the-method--the-delegatetypes-2-handler-field-is-not-a-delegate-its-a--please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8015: Ungültige DelegateProxyAttribute für den Rückgabewert für die Methode *.*: Der Delegattyps ({2}) 'Handler'-Feld ist kein Delegat ist dies eine *. Melden Sie Fehler auf http://bugzilla.xamarin.com.

Das DelegateProxy-Attribut für die betreffende Methode ist ungültig.

In der Regel gibt einen Fehler in Xamarin.iOS. Melden Sie Fehler auf [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT8016" />

### <a name="mt8016-unable-to-convert-delegate-to-block-for-the-return-value-for-the-method--because-the-input-isnt-a-delegate-its-a--please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8016: Kann nicht konvertiert der Delegat, der für den Rückgabewert für die Methode blockiert *.*, da die Eingabe eines Delegaten ist es eine *. Melden Sie Fehler auf http://bugzilla.xamarin.com.

Das DelegateProxy-Attribut für die betreffende Methode ist ungültig.

In der Regel gibt einen Fehler in Xamarin.iOS. Melden Sie Fehler auf [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<!-- 8017 is used by mmp -->

<a name="MT8018" />

### <a name="mt8018-internal-consistency-error-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT8018: Interner Konsistenzfehler. Melden Sie bitte einen Fehlerbericht an http://bugzilla.xamarin.com.

Hiermit wird einen Fehler in Xamarin.iOS. Melden Sie Fehler auf [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT8019" />

### <a name="mt8019-could-not-find-the-assembly--in-the-loaded-assemblies"></a>MT8019: Die Assembly wurde nicht gefunden * in den geladenen Assemblys.

Hiermit wird einen Fehler in Xamarin.iOS. Melden Sie Fehler auf [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT8020" />

### <a name="mt8020-could-not-find-the-module-with-metadatatoken--in-the-assembly-"></a>MT8020: Nicht gefunden. das Modul mit MetadataToken * in der Assembly *.

Hiermit wird einen Fehler in Xamarin.iOS. Melden Sie Fehler auf [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT8021" />

### <a name="mt8021-unknown-implicit-token-type-"></a>MT8021: Unbekannter implizite Tokentyp: *.

Hiermit wird einen Fehler in Xamarin.iOS. Melden Sie Fehler auf [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT8022" />

### <a name="mt8022-expected-the-token-reference--to-be-a--but-its-a--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT8022: Erwartet den Tokenverweis * soll eine *, aber es ist eine *. Melden Sie bitte einen Fehlerbericht an http://bugzilla.xamarin.com.

Hiermit wird einen Fehler in Xamarin.iOS. Melden Sie Fehler auf [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT8023" />

### <a name="mt8023-an-instance-object-is-required-to-construct-a-closed-generic-method-for-the-open-generic-method--token-reference--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT8023: Ein Instanzobjekt ist erforderlich, um eine geschlossene generische Methode für die offene generische Methode zu erstellen: * (token-Referenz: *). Melden Sie bitte einen Fehlerbericht an http://bugzilla.xamarin.com.

Hiermit wird einen Fehler in Xamarin.iOS. Melden Sie Fehler auf [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT8024" />

### <a name="mt8024-could-not-find-a-valid-extension-type-for-the-smart-enum-smarttype-please-file-a-bug-at-httpsbugzillaxamarincom"></a>MT8024: Einen gültiger Erweiterungstyp wurde für die intelligente Enumeration "{Smart_type}" nicht gefunden werden. Melden Sie Fehler auf https://bugzilla.xamarin.com.

Hiermit wird einen Fehler in Xamarin.iOS. Melden Sie Fehler auf [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).
