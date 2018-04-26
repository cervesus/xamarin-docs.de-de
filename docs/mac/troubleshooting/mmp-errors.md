---
title: Xamarin.Mac-Fehlermeldungen (Mmp)
description: Eine Fehler-Referenzhandbuch für Mmp.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5B26339F-A202-4E41-9229-D0BC9E77868E
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/27/2018
ms.openlocfilehash: 7f0d40e4adb16e47db18d0796afa42c66af096ce
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/26/2018
---
# <a name="xamarinmac-error-messages-mmp"></a>Xamarin.Mac-Fehlermeldungen (Mmp)

## <a name="mm0xxx-mmp-error-messages"></a>MM0xxx: Mmp-Fehlermeldungen

Beispiel: Parameter, Umgebung, Tools fehlt.

<a name="MM0000" />

#### <a name="mm0000-unexpected-error---please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MM0000: Unerwarteter Fehler: die Datei einen Fehler melden an http://bugzilla.xamarin.com

Ein unerwarteter Fehler aufgetreten ist. Bitte [Datei einen Fehlerbericht](https://bugzilla.xamarin.com/enter_bug.cgi?product=Xamarin.Mac) durch so viele Informationen wie möglich, einschließlich:

* Vollständige Buildprotokolle, mit maximale Ausführlichkeit (z. B. `-v -v -v -v` in der **zusätzliche Mmp Argumente**);
* Eine minimale Testfall, der den Fehler reproduzieren; und
* Alle Informationen der version

Die einfachste Möglichkeit zum Abrufen von Informationen zur exakten Version ist die Verwendung der **Xamarin Studio** Menü **über Xamarin Studio** Element **Details anzeigen** Schaltfläche und die Version, kopieren und einfügen Informationen (Sie können die **Informationen kopieren** Schaltfläche).

<a name="MM0001" />

#### <a name="mm0001-this-version-of-xamarinmac-requires-mono-0-the-current-mono-version-is-1-please-update-the-monoframework-from-httpmono-projectcomdownloads"></a>MM0001: Diese Version von Xamarin.Mac erfordert Mono {0} (die aktuelle Mono-Version ist {1}). Aktualisieren Sie die Mono.framework aus http://mono-project.com/Downloads

<a name="MM0003" />

#### <a name="mm0003-application-name-0exe-conflicts-with-an-sdk-or-product-assembly-dll-name"></a>MM0003: Anwendungsname "{0}.exe" steht in Konflikt mit einem Namen der Assembly (.dll) SDK oder Produkt.

<a name="MM0007" />

#### <a name="mm0007-the-root-assembly-0-does-not-exist"></a>MM0007: Die stammassembly "{0}" ist nicht vorhanden

<a name="MM0008" />

#### <a name="mm0008-you-should-provide-one-root-assembly-only-found-0-assemblies-1"></a>MM0008: Sie sollten eine stammassembly, die sich nur bereitstellen {0} Assemblys: "{1}"

<a name="MM0010" />

#### <a name="mm0010-could-not-parse-the-command-line-arguments-0"></a>MM0010: Die Befehlszeilenargumente konnte nicht analysiert werden: {0}

<!-- 0013 is unused -->

<a name="MM0016" />

#### <a name="mm0016-the-option-0-has-been-deprecated"></a>MM0016: Die Option "{0}" ist veraltet.

<a name="MM0017" />

#### <a name="mm0017-you-should-provide-a-root-assembly"></a>MM0017: Sie sollten eine stammassembly bereitstellen.

<a name="MM0018" />

#### <a name="mm0018-unknown-command-line-argument-0"></a>MM0018: Unbekanntes Befehlszeilenargument: "{0}"

<a name="MM0020" />

#### <a name="mm0020-the-valid-options-for-0-are-1"></a>MM0020: Die gültigen Optionen für "{0}"sind"{1}".

<a name="MM0023" />

#### <a name="mm0023-application-name-0exe-conflicts-with-another-user-assembly"></a>MM0023: Anwendungsname "{0}.exe" steht in Konflikt mit einem anderen Benutzerassembly.

<a name="MM0026" />

#### <a name="mm0026-could-not-parse-the-command-line-argument-0-1"></a>MM0026: Konnte nicht analysiert werden. das Befehlszeilenargument "{0}": {1}

<a name="MM0043" />

#### <a name="mm0043-the-boehm-garbage-collector-is-not-supported-the-sgen-garbage-collector-has-been-selected-instead"></a>MM0043: Boehm Garbagecollector wird nicht unterstützt. SGen-Garbagecollector hat stattdessen ausgewählt wurde.

<a name="MM0050" />

#### <a name="mm0050-you-cannot-provide-a-root-assembly-if---no-root-assembly-is-passed"></a>MM0050: Sie können keine stammassembly bereitstellen, wenn – Nein-Stamm-Assembly übergeben wird.

<a name="MM0051" />

#### <a name="mm0051-an-output-directory---output-is-required-if---no-root-assembly-is-passed"></a>MM0051: Ein Ausgabeverzeichnis (--Ausgabe) ist erforderlich, wenn – Nein-Stamm-Assembly übergeben wird.

<a name="MM0053" />

#### <a name="mm0053-cannot-disable-new-refcount-with-the-unified-api"></a>MM0053: Neue Refcount-Wert mit der Unified-API kann nicht deaktiviert werden.

<a name="MM0056" />

#### <a name="mm0056-cannot-find-xcode-in-any-of-our-default-locations-please-install-xcode-or-pass-a-custom-path-using---sdkrootpath"></a>MM0056: Xcode in jedem unserer Standardspeicherorte kann nicht gefunden werden. Installieren Sie Xcode, oder übergeben Sie einen benutzerdefinierten Pfad, die mit--Sdkroot =<path>

<a name="MM0059" />

#### <a name="mm0059-could-not-find-the-currently-selected-xcode-on-the-system-0"></a>MM0059: Der aktuell ausgewählten Xcode auf dem System wurde nicht gefunden werden: {0};

<a name="MM0060" />

#### <a name="mm0060-could-not-find-the-currently-selected-xcode-on-the-system-xcode-select---print-path-returned-0-but-that-directory-does-not-exist"></a>MM0060: Der aktuell ausgewählten Xcode auf dem System wurde nicht gefunden werden. "Xcode auswählen – Print-Path" zurückgegeben "{0}", aber dieser Verzeichnis ist nicht vorhanden.

<a name="MM0068" />

#### <a name="mm0068-invalid-value-for-target-framework-0"></a>MM0068: Ungültiger Wert für Zielframework: {0}.

<a name="MM0071" />

#### <a name="mm0071-unknown-platform--this-usually-indicates-a-bug-in-xamarinmac-please-file-a-bug-report-at-httpsbugzillaxamarincom-with-a-test-case"></a>MM0071: Unbekannte Plattform: *. Dies weist in der Regel ein Fehler im Xamarin.Mac hin; Bitte der Datei eines Fehlerberichts an https://bugzilla.xamarin.com zu einem Testfall.

Dies weist in der Regel ein Fehler im Xamarin.Mac hin; Bitte der Datei eines Fehlerberichts an [ https://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=Xamarin.Mac) zu einem Testfall.

<a name="MM0079" />

#### <a name="mm0079-internal-error---no-executable-was-copied-into-the-app-bundle-please-contact-supportxamarincom"></a>MM0079: Interner Fehler: keine ausführbare Datei in das app-Paket kopiert wurde. Wenden Sie sich an "support@xamarin.com"

<a name="MM0080" />

#### <a name="mm0080-disabling-newrefcount---new-refcountfalse-is-deprecated"></a>MM0080: Deaktivieren von NewRefCount,--new-Refcount:false, ist veraltet.

<!-- 0088 used by mtouch -->
<!-- 0089 used by mtouch -->

<a name="MM0091" />

#### <a name="mm0091-this-version-of-xamarinmac-requires-the--sdk-shipped-with-xcode--either-upgrade-xcode-to-get-the-required-header-files-or-use-the-dynamic-registrar-or-set-the-managed-linker-behaviour-to-link-platform-or-link-framework-sdks-only-to-try-to-avoid-the-new-apis"></a>MM0091: Diese Version von Xamarin.Mac erfordert die * SDK (Xcode Lieferumfang *). Entweder upgrade Xcode erhalten die erforderlichen Header-Dateien oder die dynamische Registrierungsstelle verwenden oder verwalteten Linker Verhalten auf Link Plattform oder nur Link Framework SDKs (versucht, die neuen APIs zu vermeiden).

Xamarin.Mac erfordert die Headerdateien, von der SDK-Version, die in der Fehlermeldung zur Erstellung der Anwendung mit dem statischen Registrar angegeben. Die empfohlene Methode zum Beheben dieses Fehlers Xcode zum Abrufen der erforderlichen SDK zu aktualisieren, dazu gehören die erforderlichen Headerdateien. Wenn Sie mehrere Versionen von Xcode installiert haben oder ein Xcode in einer nicht standardmäßigen Speicherort verwenden möchten, stellen Sie sicher, dass die richtigen Xcode-Speicherort in der IDE Einstellungen.

Eine mögliche, eine alternative Lösung ist zum Aktivieren des verwalteten Linkers. Hiermit entfernen Sie nicht verwendete API einschließen, in den meisten Fällen, die neue API, in dem die Headerdateien fehlt (oder ist unvollständig) sind. Jedoch stellt dies nicht funktioniert, wenn das Projekt-API verwendet, die in einer neueren SDK als an denjenigen Ihrer Xcode eingeführt wurde.

Eine zweite mögliche, alternative Lösung wird stattdessen die dynamische Registrierungsstelle. Dies wird festlegen, indem Sie Typen dynamisch registrieren Sie eine Start-Kosten, aber die Header-Datei-Anforderung entfernen. 

Eine letzte Stroh-Lösung mit einer älteren Version von Xamarin.Mac wäre, erfordert eine, die das SDK das Projekt unterstützt.

<a name="MM0097" />

#### <a name="mm0097-machineconfig-file-0-can-not-be-found"></a>MM0097: "Machine.config" Datei "{0}" kann nicht gefunden werden.

<a name="MM0098" />

#### <a name="mm0098-aot-compilation-is-only-available-on-unified"></a>MM0098: AOT Kompilierung ist nur verfügbar für Unified

<a name="MM0099" />

#### <a name="mm0099-internal-error-0-please-file-a-bug-report-with-a-test-case-httpbugzillaxamarincom"></a>MM0099: Interner Fehler {0}. Bitte der Datei eines Fehlerberichts zu einem Testfall (http://bugzilla.xamarin.com).

<a name="MM0114" />

#### <a name="mm0114-hybrid-aot-compilation-requires-all-assemblies-to-be-aot-compiled"></a>MM0114: Hybrid AOT Kompilierung erfordert, dass alle Assemblys AOT kompiliert werden.

## <a name="mm1xxx-file-copy--symlinks-project-related"></a>MM1xxx: Kopieren der Datei / symbolische Verknüpfungen (Projekt verknüpft)

<a name="MM1034" />

#### <a name="mm1034-could-not-create-symlink-file---target-error-number"></a>MM1034: Konnte nicht erstellt werden "Symlink" "{file}" -> "{target}": Fehler {Number}

### <a name="mm14xx-product-assemblies"></a>MM14xx: Produktassemblys

<a name="MM1401" />

#### <a name="mm1401-the-required-0-assembly-is-missing-from-the-references"></a>MM1401: Das erforderliche '{0}"Assembly fehlt aus den Referenzen

<a name="MM1402" />

#### <a name="mm1402-the-assembly-0-is-not-compatible-with-this-tool"></a>MM1402: Die Assembly "{0}" ist nicht kompatibel mit diesem Tool

<a name="MM1403" />

#### <a name="mm1403-0-1-could-not-be-found-target-framework-0-is-unusable-to-package-the-application"></a>MM1403: {0} "{1}" konnte nicht gefunden werden. Zielframework "{0}" kann nicht verwendet werden, um die Anwendung verpacken.

<a name="MM1404" />

#### <a name="mm1404-target-framework-0-is-invalid"></a>MM1404: Zielframework "{0}" ist ungültig.

<a name="MM1405" />

#### <a name="mm1405-usefullxammacframework-must-always-target-framework-net-45-not-0-which-is-invalid"></a>MM1405: UseFullXamMacFramework muss immer Zielframework .NET 4.5, nicht "{0}" ist ungültig

<a name="MM1406" />

#### <a name="mm1406-target-framework-0-is-invalid-when-targetting-xamarinmac-45-net-framwork"></a>MM1406: Zielframework "{0}" ist ungültig, wenn Anwendung Xamarin.Mac 4.5 .NET Framwork.

<a name="MM1407" />

#### <a name="mm1407-mismatch-between-xamarinmac-reference-0-and-target-framework-selected-1"></a>MM1407: Besteht ein Konflikt zwischen Xamarin.Mac Verweis '{0}"und Zielframework ausgewählt"{1}".

### <a name="mm15xx-assembly-gathering-not-requiring-linker-errors"></a>MM15xx: Sammeln von (keine grafische Linker)-assemblyfehlern

<a name="MM1501" />

#### <a name="mm1501-can-not-resolve-reference-0"></a>MM1501: Der Verweis kann nicht aufgelöst werden: {0}

### <a name="machocs"></a>MachO.cs

<a name="MM1600" />

#### <a name="mm1600-not-a-mach-o-dynamic-library-unknown-header-0x0-1"></a>MM1600: Nicht abstimmen-o dynamische Bibliothek (unbekannte Header ' 0 X{0}"): {1}.

<a name="MM1601" />

#### <a name="mm1601-not-a-static-library-unknown-header-0-1"></a>MM1601: Keine statische Bibliothek (unbekannte Header "{0}"): {1}.

<a name="MM1602" />

#### <a name="mm1602-not-a-mach-o-dynamic-library-unknown-header-0x0-1"></a>MM1602: Nicht abstimmen-o dynamische Bibliothek (unbekannte Header ' 0 X{0}"): {1}.

<a name="MM1603" />

#### <a name="mm1603-unknown-format-for-fat-entry-at-position-0-in-1"></a>MM1603: Unbekanntes Format für fat-Eintrags an Position {0} in {1}.

<a name="MM1604" />

#### <a name="mm1604-file-of-type-0-is-not-a-macho-file-1"></a>MM1604: Datei des Typs {0} ist keine Datei MachO ({1}).

## <a name="mm2xxx-linker"></a>MM2xxx: Linker

### <a name="mm20xx-linker-general-errors"></a>MM20xx: Linkerfehler (Allgemein)

<a name="MM2001" />

#### <a name="mm2001-could-not-link-assemblies"></a>MM2001: Assemblys konnte nicht verknüpft werden.

<a name="MM2002" />

#### <a name="mm2002-can-not-resolve-reference-0"></a>MM2002: Der Verweis kann nicht aufgelöst werden: {0}

<a name="MM2003" />

#### <a name="mm2003-option-0-will-be-ignored-since-linking-is-disabled"></a>MM2003: Option "{0}" wird ignoriert, da verknüpfen deaktiviert ist

<a name="MM2004" />

#### <a name="mm2004-extra-linker-definitions-file-0-could-not-be-located"></a>MM2004: Extra Linker-Definitionsdatei "{0}" konnte nicht gefunden werden.

<a name="MM2005" />

#### <a name="mm2005-definitions-from-0-could-not-be-parsed"></a>MM2005: Definitionen aus "{0}" konnte nicht analysiert werden.

<a name="MM2006" />

#### <a name="mm2006-native-library-0-was-referenced-but-could-not-be-found"></a>MM2006: Systemeigene Bibliothek "{0}" wurde verwiesen, aber nicht gefunden.

<a name="MM2007" />

#### <a name="mm2007-xamarinmac-unified-api-against-a-full-net-profile-does-not-support-linking-pass-the--nolink-flag"></a>MM2007: Xamarin.Mac einheitliche API für ein vollständiges .NET Profil verknüpfen unterstützt keine. Übergeben Sie das Nolink - Flag.

<a name="MM2009" />

#### <a name="mm2009-referenced-by-01------this-message-is-related-to-mm2006-"></a>MM2009: Verweist {0}.{1}     ** Diese Meldung bezieht sich auf MM2006 **

<a name="MM2010" />

#### <a name="mm2010-unknown-httpmessagehandler-0-valid-values-are-httpclienthandler-default-cfnetworkhandler-or-nsurlsessionhandler"></a>MM2010: Unbekannte HttpMessageHandler `{0}`. Gültige Werte sind HttpClientHandler (Standard), CFNetworkHandler oder NSUrlSessionHandler

<a name="MM2011" />

#### <a name="mm2011-unknown-tlsprovider-0--valid-values-are-default-or-appletls"></a>MM2011: Unbekannte TLSProvider "{0}.  Gültige Werte sind Default oder appletls

<a name="MM2012" />

#### <a name="mm2012-only-first-0-of-1-referenced-by-warnings-shown--this-message-related-to-2009-"></a>MM2012: Nur die ersten {0} von {1} "Referenziert von" Warnungen angezeigt. ** Diese Meldung im Zusammenhang mit 2009 **

<a name="MM2013" />

#### <a name="mm2013-failed-to-resolve-the-reference-to-0-referenced-in-1-the-app-will-not-include-the-referenced-assembly-and-may-fail-at-runtime"></a>MM2013: Fehler beim Auflösen des des Verweis auf "{0}", auf die verwiesen wird in"{1}". Die app enthält nicht die referenzierte Assembly und kann zur Laufzeit fehlschlagen.

<a name="MM2014" />

#### <a name="mm2014-xamarinmac-extensions-do-not-support-linking-request-for-linking-will-be-ignored--this-message-is-obsolete-in-xm-36-"></a>MM2014: Xamarin.Mac Erweiterungen bieten keine Unterstützung verknüpfen. Die Anforderung für verknüpfen werden ignoriert. ** Diese Meldung ist in XM 3.6 veraltet **

<!-- 2015 used by mtouch -->

<a name="MM2016" />

#### <a name="mm2016-invalid-tlsprovider-0-option-the-only-valid-value-1-will-be-used"></a>MM2016: Ungültige TlsProvider `{0}` Option. Der einzige gültige Wert `{1}` verwendet werden.

<a name="MM2017" />

#### <a name="mm2017-could-not-process-xml-description-0"></a>MM2017: Nicht verarbeiten konnte, XML-Beschreibung: {0}

<a name="MM202x" />

#### <a name="mm202x-binding-optimizer-failed-processing-"></a>MM202x: Bindung Optimierer Fehler bei Verarbeitung `...`.

<a name="MM2100" />

#### <a name="mm2100-xamarinmac-classic-api-does-not-support-platform-linking"></a>MM2100: Xamarin.Mac klassischen API nicht verknüpfen Plattform unterstützt.

<a name="MM2103" />

#### <a name="mm2103-error-processing-assembly--"></a>MM2103: Fehler beim Verarbeiten der Assembly "\*": *

Unerwarteter Fehler beim Verarbeiten einer Assemblys.

Die Assembly, die Ursache des Problems heißt in der Fehlermeldung. Zur Behebung dieses Problems der Assemblys müssen in bereitgestellt werden, eine [Bericht](https://bugzilla.xamarin.com) zusammen mit einer vollständigen Buildprotokoll mit Ausführlichkeit aktiviert (d. h. `-v -v -v -v` in der **zusätzliche Mtouch Argumente**).

<a name="MM2104" />

#### <a name="mm2104-unable-to-link-assembly-0-as-it-is-mixed-mode"></a>MM2104: Keine Assembly Verknüpfung kann "{0}" unverändert im gemischten Modus.

Gemischte Assemblys können nicht vom Linker verarbeitet werden.

Finden Sie unter https://msdn.microsoft.com/library/x0w2664k.aspx Weitere Informationen zu gemischten Assemblys.

## <a name="mm3xxx-aot"></a>MM3xxx: AOT

### <a name="mm30xx-aot-general-errors"></a>MM30xx: Fehler AOT (Allgemein)

<a name="MM3001" />

#### <a name="mm3001-could-not-aot-the-assembly-0"></a>MM3001: Konnte keine AOT der Assembly "{0}"

<!-- 3002 used by mtouch -->
<!-- 3003 used by mtouch -->
<!-- 3004 used by mtouch -->
<!-- 3005 used by mtouch -->
<!-- 3006 used by mtouch -->
<!-- 3007 used by mtouch -->
<!-- 3008 used by mtouch -->

<a name="MM3009" />

#### <a name="mm3009-aot-of-0-was-requested-but-was-not-found"></a>MM3009: AOT von "{0}" wurde angefordert, aber wurde nicht gefunden

<a name="MM3010" />

#### <a name="mm3010-exclusion-of-aot-of-0-was-requested-but-was-not-found"></a>MM3010: Ausschluss von AOT von "{0}" wurde angefordert, aber wurde nicht gefunden

## <a name="mm4xxx-code-generation"></a>MM4xxx: codegenerierung

### <a name="mm40xx-driverm"></a>MM40xx: driver.m

<a name="MM4001" />

#### <a name="mm4001-the-main-template-could-not-be-expanded-to-0"></a>MM4001: Die Haupt-Vorlage konnte nicht auf erweitert werden `{0}`.

### <a name="mm41xx-registrar"></a>MM41xx: Registrierungsstelle

<a name="MM4134" />

#### <a name="mm4134-your-application-is-using-the-0-framework-which-isnt-included-in-the-macos-sdk-youre-using-to-build-your-app-this-framework-was-introduced-in-osx-2-while-youre-building-with-the-macos-1-sdk-this-configuration-is-not-supported-with-the-static-registrar-pass---registrardynamic-as-an-additional-mmp-argument-in-your-projects-mac-build-option-to-select-alternatively-select-a-newer-sdk-in-your-apps-mac-build-options"></a>MM4134: Ihre Anwendung verwendet die "{0}" Framework, das im MacOS SDK enthalten ist, Sie beim Erstellen Ihrer app verwenden (dieses Framework wurde in OSX eingeführt {2}, während Sie mit der MacOS erstellen {1} SDK.) Diese Konfiguration wird mit dem statischen Registrar (Pass - Registrierungsstelle: dynamische als Argument für zusätzliche Mmp Mac Erstellen Ihres Projekts-Option, um auszuwählen) nicht unterstützt. Wählen Sie alternativ einen neueren SDK in Ihrer app Mac Buildoptionen.

## <a name="mm5xxx-gcc-and-toolchain"></a>MM5xxx: GCC und toolkette

### <a name="mm51xx-compilation"></a>MM51xx: Kompilierung

<a name="MM5101" />

#### <a name="mm5101-missing-0-compiler-please-install-xcode-command-line-tools-component"></a>MM5101: Fehlendes '{0}"Compiler. Installieren Sie Xcode "Befehlszeilentools"-Komponente.

<!-- 5102 used by mtouch -->

<a name="MM5103" />

#### <a name="mm5103-failed-to-compile-error-code---0-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MM5103: Fehler beim Kompilieren. Fehlercode: - {0}. Bitte der Datei eines Fehlerberichts an http://bugzilla.xamarin.com

<!-- 5104 used by mtouch -->

### <a name="mm52xx-linking"></a>MM52xx: Verknüpfen

<a name="MM5202" />

#### <a name="mm5202-monoframework-mdk-is-missing-please-install-the-mdk-for-your-monoframework-version-from-httpmono-projectcomdownloads"></a>MM5202: Mono.framework MDK ist nicht vorhanden. Installieren Sie die MDK für Ihre Version Mono.framework aus http://mono-project.com/Downloads

<a name="MM5203" />

#### <a name="mm5203-cant-find-libxammaca-likely-because-of-a-corrupted-xamarinmac-installation-please-reinstall-xamarinmac"></a>MM5203: Libxammac.a, wahrscheinlich aufgrund einer beschädigten Xamarin.Mac-Installation kann nicht gefunden werden. Installieren Sie den Xamarin.Mac.

<a name="MM5204" />

#### <a name="mm5204-invalid-architecture-x8664-is-only-supported-on-non-classic-profiles"></a>MM5204: Ungültige-Architektur. x86_64 wird nur auf nicht klassischen Profile unterstützt.

<a name="MM5205" />

#### <a name="mm5205-invalid-architecture-0-valid-architectures-are-i386-and-x8664-when---profilemobile"></a>MM5205: Ungültige Architektur "{0}". Gültige Architekturen sind i386 und x86_64 (wenn--Profil mobile =).

<a name="MM5218" />

#### <a name="mm5218-cant-ignore-the-dynamic-symbol-symbol---ignore-dynamic-symbolsymbol-because-it-was-not-detected-as-a-dynamic-symbol"></a>MM5218: Kann nicht das dynamische Symbol {Symbol} ignorieren (--ignorieren-Dynamic-Symbol = {Symbol}), da er nicht als ein dynamisches Symbol erkannt wurde.

Finden Sie unter der [entsprechende Mtouch Warnung](~/ios/troubleshooting/mtouch-errors.md#MT5218).

<!-- 5206 used by mtouch -->
<!-- 5207 used by mtouch -->
<!-- 5208 used by mtouch -->
<!-- 5209 used by mtouch -->
<!-- 5210 used by mtouch -->
<!-- 5211 used by mtouch -->
<!-- 5212 used by mtouch -->
<!-- 5213 used by mtouch -->
<!-- 5214 used by mtouch -->
<!-- 5215 used by mtouch -->
<!-- 5216 used by mtouch -->
<!-- 5217 used by mtouch -->

### <a name="mm53xx-other-tools"></a>MM53xx: andere tools

<a name="MM5301" />

#### <a name="mm5301-pkg-config-could-not-be-found-please-install-the-monoframework-from-httpmono-projectcomdownloads"></a>MM5301: Pkg-Config konnte nicht gefunden werden. Installieren Sie die Mono.framework aus http://mono-project.com/Downloads

<!-- 5302 used by mtouch -->
<!-- 5303 used by mtouch -->
<!-- 5304 used by mtouch -->

<a name="MM5305" />

#### <a name="mm5305-missing-otool-tool-please-install-xcode-command-line-tools-component"></a>MM5305: Fehlender "Otool"-Tool. Installieren Sie Xcode "Befehlszeilentools"-Komponente

<a name="MM5306" />

#### <a name="mm5306-missing-dependencies-please-install-xcode-command-line-tools-component"></a>MM5306: Fehlenden Abhängigkeiten. Installieren Sie Xcode "Befehlszeilentools"-Komponente

<a name="MM5308" />

#### <a name="mm5308-xcode-license-agreement-may-not-have-been-accepted--please-launch-xcode"></a>MM5308: Xcode-Lizenzvertrag möglicherweise nicht akzeptiert wurden.  Bitte starten Sie Xcode an.

<a name="MM5309" />

#### <a name="mm5309-native-linking-failed-with-error-code-1--check-build-log-for-details"></a>MM5309: Verknüpfen von systemeigenen Fehler mit Fehlercode: 1.  Überprüfen Sie Buildprotokolls ausführliche.

<a name="MM5310" />

#### <a name="mm5310-installnametool-failed-with-an-error-code-0-check-build-log-for-details"></a>MM5310: Install_name_tool konnte nicht mit dem Fehlercode "{0}". Überprüfen Sie Buildprotokolls ausführliche.

<!-- MM6xxx: mmp internal tools -->
<!-- MM7xxx: reserved -->

## <a name="mm8xxx-runtime"></a>MM8xxx: Common Language Runtime

### <a name="mm800x-misc"></a>MM800x: Verschiedenes

<!-- 8000 used by mtouch -->
<!-- 8001 used by mtouch -->
<!-- 8002 used by mtouch -->
<!-- 8003 used by mtouch -->
<!-- 8004 used by mtouch -->
<!-- 8005 used by mtouch -->
<!-- 8006 used by mtouch -->
<!-- 8007 used by mtouch -->
<!-- 8008 used by mtouch -->
<!-- 8009 used by mtouch -->
<!-- 8010 used by mtouch -->
<!-- 8011 used by mtouch -->
<!-- 8012 used by mtouch -->
<!-- 8013 used by mtouch -->
<!-- 8014 used by mtouch -->
<!-- 8015 used by mtouch -->
<!-- 8016 used by mtouch -->

<a name="MM8017" />

#### <a name="mm8017-the-boehm-garbage-collector-is-not-supported-please-use-sgen-instead"></a>MM8017: Boehm Garbagecollector wird nicht unterstützt. Verwenden Sie stattdessen SGen.

