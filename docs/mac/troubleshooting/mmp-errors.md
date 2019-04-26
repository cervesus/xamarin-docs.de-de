---
title: Xamarin.Mac-Fehlermeldungen (Mmp)
description: Dieses Dokument listet durch Mmp, das Tool verwendet, um kompilierte Assemblys in eine ausführbare Mac-Anwendung Verpacken generierten Fehler.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5B26339F-A202-4E41-9229-D0BC9E77868E
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/27/2018
ms.openlocfilehash: 640d1adc048bec167508d8c288b62d498f061b0d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61233201"
---
# <a name="xamarinmac-error-messages-mmp"></a>Xamarin.Mac-Fehlermeldungen (Mmp)

## <a name="mm0xxx-mmp-error-messages"></a>MM0xxx: Mmp-Fehlermeldungen

Beispiel: Parameter, Umgebung, Tools fehlt.

<a name="MM0000" />

#### <a name="mm0000-unexpected-error---please-file-a-bug-report-at-httpsgithubcomxamarinxamarin-maciosissuesnew"></a>MM0000: Unerwarteter Fehler: Datei einen Fehler melden an https://github.com/xamarin/xamarin-macios/issues/new

Ein unerwarteter Fehler ist aufgetreten. Bitte [einen Fehlerbericht](https://github.com/xamarin/xamarin-macios/issues/new) so viele Informationen wie möglich, einschließlich:

* Vollständiger Build erstellt Protokolle mit maximaler Ausführlichkeit (z. B. `-v -v -v -v` in die **zusätzlichen Mmp-Argumenten**);
* Eine minimale Testfall, der den Fehler reproduzieren; und
* Alle Informationen für version

Die einfachste Möglichkeit zum Abrufen von Informationen zur genauen Version ist die Verwendung der **Xamarin Studio** Menü **Info zu Xamarin Studio** Element **Details anzeigen** Schaltfläche und die Version, kopieren und einfügen Informationen (Sie können die **Informationen kopieren** Schaltfläche).

<a name="MM0001" />

#### <a name="mm0001-this-version-of-xamarinmac-requires-mono-0-the-current-mono-version-is-1-please-update-the-monoframework-from-httpmono-projectcomdownloads"></a>MM0001: Diese Version von Xamarin.Mac erfordert Mono {0} (die aktuelle Mono-Version ist {1}). Aktualisieren Sie die Mono.framework aus http://mono-project.com/Downloads

<a name="MM0003" />

#### <a name="mm0003-application-name-0exe-conflicts-with-an-sdk-or-product-assembly-dll-name"></a>MM0003: Name der Anwendung "{0}.exe" steht in Konflikt mit einem Namen der Assembly (.dll) SDK oder eines Produkts.

<a name="MM0007" />

#### <a name="mm0007-the-root-assembly-0-does-not-exist"></a>MM0007: Die stammassembly "{0}' ist nicht vorhanden

<a name="MM0008" />

#### <a name="mm0008-you-should-provide-one-root-assembly-only-found-0-assemblies-1"></a>MM0008: Sie sollten eine Assembly auf Stammebene, die sich nur bereitstellen {0} Assemblys: "{1}"

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

#### <a name="mm0023-application-name-0exe-conflicts-with-another-user-assembly"></a>MM0023: Name der Anwendung "{0}.exe" verursacht einen Konflikt mit einem anderen Benutzer-Assembly.

<a name="MM0026" />

#### <a name="mm0026-could-not-parse-the-command-line-argument-0-1"></a>MM0026: Konnte nicht analysiert werden. das Befehlszeilenargument "{0}": {1}

<a name="MM0043" />

#### <a name="mm0043-the-boehm-garbage-collector-is-not-supported-the-sgen-garbage-collector-has-been-selected-instead"></a>MM0043: Boehm Garbagecollector wird nicht unterstützt. Stattdessen wurde der SGen Garbagecollector ausgewählt.

<a name="MM0050" />

#### <a name="mm0050-you-cannot-provide-a-root-assembly-if---no-root-assembly-is-passed"></a>MM0050: Sie können keine stammassembly bereitstellen, falls--ohne-Stamm-Assembly übergeben wird.

<a name="MM0051" />

#### <a name="mm0051-an-output-directory---output-is-required-if---no-root-assembly-is-passed"></a>MM0051: Ein Ausgabeverzeichnis (--Ausgabe) ist erforderlich, wenn--ohne-Stamm-Assembly übergeben wird.

<a name="MM0053" />

#### <a name="mm0053-cannot-disable-new-refcount-with-the-unified-api"></a>MM0053: Neuen Refcount mit der Unified API kann nicht deaktiviert werden.

<a name="MM0056" />

#### <a name="mm0056-cannot-find-xcode-in-any-of-our-default-locations-please-install-xcode-or-pass-a-custom-path-using---sdkrootpath"></a>MM0056: Eines unserer Standardspeicherorte Xcode wurde nicht gefunden. Installieren Sie Xcode, oder übergeben Sie einen benutzerdefinierten Pfad, die mit--Sdkroot =<path>

<a name="MM0059" />

#### <a name="mm0059-could-not-find-the-currently-selected-xcode-on-the-system-0"></a>MM0059: Die aktuell ausgewählte Xcode auf dem System wurde nicht gefunden: {0};

<a name="MM0060" />

#### <a name="mm0060-could-not-find-the-currently-selected-xcode-on-the-system-xcode-select---print-path-returned-0-but-that-directory-does-not-exist"></a>MM0060: Die aktuell ausgewählte Xcode wurde auf dem System nicht gefunden werden. "Xcode-Select--Print-Path" zurückgegebenen "{0}", aber dieses Verzeichnis ist nicht vorhanden.

<a name="MM0068" />

#### <a name="mm0068-invalid-value-for-target-framework-0"></a>MM0068: Ungültiger Wert für das Zielframework: {0}.

<a name="MM0071" />

#### <a name="mm0071-unknown-platform--this-usually-indicates-a-bug-in-xamarinmac-please-file-a-bug-report-at-httpsbugzillaxamarincom-with-a-test-case"></a>MM0071: Unbekannte Plattform: *. In der Regel gibt einen Fehler in Xamarin.Mac. Melden Sie bitte einen Fehlerbericht an https://bugzilla.xamarin.com zu einem Testfall.

In der Regel gibt einen Fehler in Xamarin.Mac. Melden Sie bitte einen Fehlerbericht an [ https://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=Xamarin.Mac) zu einem Testfall.

<a name="MM0079" />

#### <a name="mm0079-internal-error---no-executable-was-copied-into-the-app-bundle-please-contact-supportxamarincom"></a>MM0079: Interner Fehler – keine ausführbare Datei wurde in der app-Bundle kopiert. Wenden Sie sich an "support@xamarin.com"

<a name="MM0080" />

#### <a name="mm0080-disabling-newrefcount---new-refcountfalse-is-deprecated"></a>MM0080: Deaktivieren NewRefCount ","--new-Refcount:false, ist veraltet.

<!-- 0088 used by mtouch -->
<!-- 0089 used by mtouch -->

<a name="MM0091" />

#### <a name="mm0091-this-version-of-xamarinmac-requires-the--sdk-shipped-with-xcode--either-upgrade-xcode-to-get-the-required-header-files-or-use-the-dynamic-registrar-or-set-the-managed-linker-behaviour-to-link-platform-or-link-framework-sdks-only-to-try-to-avoid-the-new-apis"></a>MM0091: Diese Version von Xamarin.Mac erfordert die * SDK (Xcode im Lieferumfang *). Entweder ein upgrade von Xcode zum Abrufen der erforderlichen Headerdateien verwenden Sie die dynamische Registrierung oder legen Sie die verwaltete linkerverhalten auf Plattform verknüpfen oder Verknüpfung nur Framework-SDKs (versuchen Sie es, um die neuen APIs zu vermeiden).

Xamarin.Mac ist erforderlich, die Header-Dateien, aus der SDK-Version, die in der Fehlermeldung zum Erstellen der Anwendung mit der statischen Registrierungsstelle angegeben wird. Die empfohlene Methode zum Beheben dieses Fehlers ist Xcode rufen Sie das erforderliche SDK zu aktualisieren, dazu gehören alle erforderlichen Header-Dateien. Wenn Sie mehrere Versionen von Xcode installiert haben, oder eine Xcode in einem nicht standardmäßigen Speicherort verwenden möchten, stellen Sie sicher, dass den richtige Xcode-Speicherort in der IDE Einstellungen.

Eine mögliche alternative Lösung, ist den verwaltete Linker aktivieren. Dies entfernt nicht verwendeten API, beispielsweise, in den meisten Fällen, die neue API, in denen die Headerdateien fehlt (oder ist unvollständig) sind. Jedoch bietet dies nicht funktionieren, wenn Ihr Projekt API verwendet, die in ein neueres SDK als Ihre Xcode eingeführt wurde.

Eine zweite mögliche, eine alternative Lösung wird stattdessen die dynamische Registrierung. Dadurch werden Kosten beim Start von dynamisch registrieren von Typen zu erzwingen, aber entfernen die Datei wird als Voraussetzung. 

Eine letzte strohhalm Lösung mit einer älteren Version von Xamarin.Mac wäre, ist erforderlich, die das SDK Ihrem Projekt unterstützt.

<a name="MM0097" />

#### <a name="mm0097-machineconfig-file-0-can-not-be-found"></a>MM0097: "Machine.config" Datei "{0}' kann nicht gefunden werden.

<a name="MM0098" />

#### <a name="mm0098-aot-compilation-is-only-available-on-unified"></a>MM0098: AOT-Kompilierung ist nur für einheitliche verfügbar

<a name="MM0099" />

#### <a name="mm0099-internal-error-0-please-file-a-bug-report-with-a-test-case-httpbugzillaxamarincom"></a>MM0099: Interner Fehler {0}. Melden Sie bitte einen Fehlerbericht zu einem Testfall (http://bugzilla.xamarin.com).

<a name="MM0114" />

#### <a name="mm0114-hybrid-aot-compilation-requires-all-assemblies-to-be-aot-compiled"></a>MM0114: Hybriden AOT-Kompilierung erfordert, dass alle Assemblys per AOT kompiliert werden.

## <a name="mm1xxx-file-copy--symlinks-project-related"></a>MM1xxx: Kopieren der Datei / symbolische Verknüpfungen (Projekt verknüpft)

<a name="MM1034" />

#### <a name="mm1034-could-not-create-symlink-file---target-error-number"></a>MM1034: "Symlink" konnte nicht erstellt werden "{file}" -> "{target}": Fehler beim {Number}

### <a name="mm14xx-product-assemblies"></a>MM14xx: Produktassemblys

<a name="MM1401" />

#### <a name="mm1401-the-required-0-assembly-is-missing-from-the-references"></a>MM1401: Der erforderliche '{0}' Assembly ist nicht vorhanden, aus den Referenzen

<a name="MM1402" />

#### <a name="mm1402-the-assembly-0-is-not-compatible-with-this-tool"></a>MM1402: Die Assembly "{0}' ist nicht kompatibel mit diesem Tool

<a name="MM1403" />

#### <a name="mm1403-0-1-could-not-be-found-target-framework-0-is-unusable-to-package-the-application"></a>MM1403: {0} "{1}' wurde nicht gefunden. Zielframework "{0}' kann nicht verwendet werden, um die Anwendung verpacken.

<a name="MM1404" />

#### <a name="mm1404-target-framework-0-is-invalid"></a>MM1404: Zielframework "{0}' ist ungültig.

<a name="MM1405" />

#### <a name="mm1405-usefullxammacframework-must-always-target-framework-net-45-not-0-which-is-invalid"></a>MM1405: UseFullXamMacFramework muss immer Zielframework .NET 4.5, nicht "{0}" Dies ist ungültig

<a name="MM1406" />

#### <a name="mm1406-target-framework-0-is-invalid-when-targetting-xamarinmac-45-net-framwork"></a>MM1406: Zielframework "{0}' ist ungültig, wenn die Xamarin.Mac 4.5 .NET Framework abzielt.

<a name="MM1407" />

#### <a name="mm1407-mismatch-between-xamarinmac-reference-0-and-target-framework-selected-1"></a>MM1407: Konflikt zwischen Xamarin.Mac-Verweis "{0}"und ausgewählte Zielframework"{1}".

### <a name="mm15xx-assembly-gathering-not-requiring-linker-errors"></a>MM15xx: Assemblyfehler-(nicht erforderlich "Linker" ist) werden ermittelt.

<a name="MM1501" />

#### <a name="mm1501-can-not-resolve-reference-0"></a>MM1501: Verweis kann nicht aufgelöst werden: {0}

### <a name="machocs"></a>MachO.cs

<a name="MM1600" />

#### <a name="mm1600-not-a-mach-o-dynamic-library-unknown-header-0x0-1"></a>MM1600: Keine Mach-O dynamische Bibliothek (unbekannte Header "0 X{0}'): {1}.

<a name="MM1601" />

#### <a name="mm1601-not-a-static-library-unknown-header-0-1"></a>MM1601: Keine statische Bibliothek (unbekannte Header '{0}'): {1}.

<a name="MM1602" />

#### <a name="mm1602-not-a-mach-o-dynamic-library-unknown-header-0x0-1"></a>MM1602: Keine Mach-O dynamische Bibliothek (unbekannte Header "0 X{0}'): {1}.

<a name="MM1603" />

#### <a name="mm1603-unknown-format-for-fat-entry-at-position-0-in-1"></a>MM1603: Unbekanntes Format fat Eintrag an der Position {0} in {1}.

<a name="MM1604" />

#### <a name="mm1604-file-of-type-0-is-not-a-macho-file-1"></a>MM1604: Datei des Typs {0} ist keine MachO-Datei ({1}).

## <a name="mm2xxx-linker"></a>MM2xxx: Linker

### <a name="mm20xx-linker-general-errors"></a>MM20xx: Linkerfehler (Allgemein)

<a name="MM2001" />

#### <a name="mm2001-could-not-link-assemblies"></a>MM2001: Assemblys konnte nicht verknüpft werden.

<a name="MM2002" />

#### <a name="mm2002-can-not-resolve-reference-0"></a>MM2002: Verweis kann nicht aufgelöst werden: {0}

<a name="MM2003" />

#### <a name="mm2003-option-0-will-be-ignored-since-linking-is-disabled"></a>MM2003: Option "{0}" wird ignoriert, da die Verknüpfung deaktiviert ist

<a name="MM2004" />

#### <a name="mm2004-extra-linker-definitions-file-0-could-not-be-located"></a>MM2004: Zusätzliche Linker-Definitionsdatei '{0}' konnte nicht gefunden werden.

<a name="MM2005" />

#### <a name="mm2005-definitions-from-0-could-not-be-parsed"></a>MM2005: Definitionen von "{0}' konnte nicht analysiert werden.

<a name="MM2006" />

#### <a name="mm2006-native-library-0-was-referenced-but-could-not-be-found"></a>MM2006: Native Bibliothek '{0}' wurde verwiesen, aber nicht gefunden.

<a name="MM2007" />

#### <a name="mm2007-xamarinmac-unified-api-against-a-full-net-profile-does-not-support-linking-pass-the--nolink-flag"></a>MM2007: Vereinheitlichten Xamarin.Mac-API für ein vollständiges Profil von .NET unterstützt keine Verknüpfung. Übergeben Sie das Flag - Nolink.

<a name="MM2009" />

#### <a name="mm2009-referenced-by-01------this-message-is-related-to-mm2006-"></a>MM2009: Referenziert von {0}.{1}     ** Diese Nachricht bezieht sich auf MM2006 **

<a name="MM2010" />

#### <a name="mm2010-unknown-httpmessagehandler-0-valid-values-are-httpclienthandler-default-cfnetworkhandler-or-nsurlsessionhandler"></a>MM2010: Unbekannter HttpMessageHandler `{0}`. Gültige Werte sind HttpClientHandler (Standard), CFNetworkHandler oder NSUrlSessionHandler

<a name="MM2011" />

#### <a name="mm2011-unknown-tlsprovider-0--valid-values-are-default-or-appletls"></a>MM2011: Unbekannter TLSProvider "{0}.  Gültige Werte sind Default oder appletls

<a name="MM2012" />

#### <a name="mm2012-only-first-0-of-1-referenced-by-warnings-shown--this-message-related-to-2009-"></a>MM2012: Nur die ersten {0} von {1} "Referenziert von" Warnungen angezeigt. ** Diese Meldung im Zusammenhang mit 2009 \*\*

<a name="MM2013" />

#### <a name="mm2013-failed-to-resolve-the-reference-to-0-referenced-in-1-the-app-will-not-include-the-referenced-assembly-and-may-fail-at-runtime"></a>MM2013: Fehler beim Auflösen des Verweises auf "{0}", auf die verwiesen wird in"{1}". Die app umfasst nicht die referenzierte Assembly und kann zur Laufzeit fehlschlagen.

<a name="MM2014" />

#### <a name="mm2014-xamarinmac-extensions-do-not-support-linking-request-for-linking-will-be-ignored--this-message-is-obsolete-in-xm-36-"></a>MM2014: Xamarin.Mac-Erweiterungen unterstützen keine Verknüpfung. Die Anforderung für die Verknüpfung werden ignoriert. ** Diese Meldung ist in XM 3.6 veraltet \*\*

<!-- 2015 used by mtouch -->

<a name="MM2016" />

#### <a name="mm2016-invalid-tlsprovider-0-option-the-only-valid-value-1-will-be-used"></a>MM2016: Ungültige TlsProvider `{0}` Option. Der einzige gültige Wert `{1}` verwendet werden.

<a name="MM2017" />

#### <a name="mm2017-could-not-process-xml-description-0"></a>MM2017: XML-Beschreibung konnte nicht verarbeitet werden: {0}

<a name="MM202x" />

#### <a name="mm202x-binding-optimizer-failed-processing-"></a>MM202x: Binden der Optimierer Verarbeitung Fehler `...`.

<a name="MM2100" />

#### <a name="mm2100-xamarinmac-classic-api-does-not-support-platform-linking"></a>MM2100: Xamarin.Mac Classic API unterstützt Plattformverknüpfung nicht.

<a name="MM2103" />

#### <a name="mm2103-error-processing-assembly--"></a>MM2103: Fehler beim Verarbeiten der Assembly "\*': *

Unerwarteter Fehler beim Verarbeiten einer Assemblys.

Die Assembly, die das Problem verursacht einen Namen in der Fehlermeldung. Um dieses Problem zu beheben, die Assembly im angegeben werden müssen eine [Fehlerbericht](https://bugzilla.xamarin.com) zusammen mit einer vollständigen Buildprotokoll mit Ausführlichkeit aktiviert (d. h. `-v -v -v -v` in die **Weitere Mtouch-Argumente**).

<a name="MM2104" />

#### <a name="mm2104-unable-to-link-assembly-0-as-it-is-mixed-mode"></a>MM2104: So verknüpfen Sie Assembly kann nicht '{0}"unverändert im gemischten Modus.

Gemischte Assemblys können nicht vom Linker verarbeitet werden.

Finden Sie unter https://msdn.microsoft.com/library/x0w2664k.aspx für Weitere Informationen zu gemischten Assemblys.

## <a name="mm3xxx-aot"></a>MM3xxx: AOT

### <a name="mm30xx-aot-general-errors"></a>MM30xx: Fehler der AOT (Allgemein)

<a name="MM3001" />

#### <a name="mm3001-could-not-aot-the-assembly-0"></a>MM3001: Konnte kein AOT der Assembly "{0}"

<!-- 3002 used by mtouch -->
<!-- 3003 used by mtouch -->
<!-- 3004 used by mtouch -->
<!-- 3005 used by mtouch -->
<!-- 3006 used by mtouch -->
<!-- 3007 used by mtouch -->
<!-- 3008 used by mtouch -->

<a name="MM3009" />

#### <a name="mm3009-aot-of-0-was-requested-but-was-not-found"></a>MM3009: AOT von "{0}" wurde angefordert, jedoch wurde nicht gefunden.

<a name="MM3010" />

#### <a name="mm3010-exclusion-of-aot-of-0-was-requested-but-was-not-found"></a>MM3010: Ausschluss von AOT von "{0}" wurde angefordert, jedoch wurde nicht gefunden.

## <a name="mm4xxx-code-generation"></a>MM4xxx: codegenerierung

### <a name="mm40xx-driverm"></a>MM40xx: driver.m

<a name="MM4001" />

#### <a name="mm4001-the-main-template-could-not-be-expanded-to-0"></a>MM4001: Die Hauptvorlage konnten nicht erweitert werden, um `{0}`.

### <a name="mm41xx-registrar"></a>MM41xx: Registrierung

<a name="MM4134" />

#### <a name="mm4134-your-application-is-using-the-0-framework-which-isnt-included-in-the-macos-sdk-youre-using-to-build-your-app-this-framework-was-introduced-in-osx-2-while-youre-building-with-the-macos-1-sdk-this-configuration-is-not-supported-with-the-static-registrar-pass---registrardynamic-as-an-additional-mmp-argument-in-your-projects-mac-build-option-to-select-alternatively-select-a-newer-sdk-in-your-apps-mac-build-options"></a>MM4134: Ihre Anwendung verwendet die "{0}" im MacOS-SDK enthalten ist, Sie zum Erstellen der app verwenden-Framework (dieses Framework seit OSX {2}, während Sie sich im MacOS erstellen {1} SDK.) Diese Konfiguration wird mit der statischen Registrierungsstelle (Pass - Registrierungsstelle: dynamische als zusätzlichen Mmp Argument in der Mac-Build-Option zum Auswählen Ihres Projekts) nicht unterstützt. Wählen Sie alternativ ein neueres SDK in Ihrer app-Mac-Build-Optionen.

## <a name="mm5xxx-gcc-and-toolchain"></a>MM5xxx: GCC und toolkette

### <a name="mm51xx-compilation"></a>MM51xx: Kompilierung

<a name="MM5101" />

#### <a name="mm5101-missing-0-compiler-please-install-xcode-command-line-tools-component"></a>MM5101: Fehlendes '{0}"Compiler. Installieren Sie Xcode "Befehlszeilentools"-Komponente.

<!-- 5102 used by mtouch -->

<a name="MM5103" />

#### <a name="mm5103-failed-to-compile-error-code---0-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MM5103: Fehler beim Kompilieren. Fehlercode: {0}. Melden Sie bitte einen Fehlerbericht an http://bugzilla.xamarin.com

<!-- 5104 used by mtouch -->

### <a name="mm52xx-linking"></a>MM52xx: linking

<a name="MM5202" />

#### <a name="mm5202-monoframework-mdk-is-missing-please-install-the-mdk-for-your-monoframework-version-from-httpmono-projectcomdownloads"></a>MM5202: Mono.Framework MDK ist nicht vorhanden. Installieren Sie die MDK für Ihre Version Mono.framework aus http://mono-project.com/Downloads

<a name="MM5203" />

#### <a name="mm5203-cant-find-libxammaca-likely-because-of-a-corrupted-xamarinmac-installation-please-reinstall-xamarinmac"></a>MM5203: Libxammac.a, wahrscheinlich aufgrund einer beschädigten Xamarin.Mac-Installation wurde nicht gefunden. Installieren Sie Xamarin.Mac erneut.

<a name="MM5204" />

#### <a name="mm5204-invalid-architecture-x8664-is-only-supported-on-non-classic-profiles"></a>MM5204: Ungültige Architektur. x86_64 ist nur für nicht-Classic-Profile unterstützt.

<a name="MM5205" />

#### <a name="mm5205-invalid-architecture-0-valid-architectures-are-i386-and-x8664-when---profilemobile"></a>MM5205: Ungültige Architektur "{0}". Gültige Architekturen sind i386 "und" x86_64 (wenn – Profil = mobile).

<a name="MM5218" />

#### <a name="mm5218-cant-ignore-the-dynamic-symbol-symbol---ignore-dynamic-symbolsymbol-because-it-was-not-detected-as-a-dynamic-symbol"></a>MM5218: Kann nicht das dynamische Symbol {Symbols} ignorieren (--ignorieren-Dynamic-Symbol = {Symbols}), da er nicht als Symbol für die dynamische erkannt wurde.

Finden Sie unter den [entsprechende Mtouch-Warnung](~/ios/troubleshooting/mtouch-errors.md#MT5218).

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

#### <a name="mm5305-missing-otool-tool-please-install-xcode-command-line-tools-component"></a>MM5305: Fehlende 'Otool'-Tool. Installieren Sie Xcode "Befehlszeilentools"-Komponente

<a name="MM5306" />

#### <a name="mm5306-missing-dependencies-please-install-xcode-command-line-tools-component"></a>MM5306: Fehlende Abhängigkeiten. Installieren Sie Xcode "Befehlszeilentools"-Komponente

<a name="MM5308" />

#### <a name="mm5308-xcode-license-agreement-may-not-have-been-accepted--please-launch-xcode"></a>MM5308: Xcode-Software-Lizenzbedingungen kann nicht akzeptiert wurden.  Bitte starten Sie Xcode.

<a name="MM5309" />

#### <a name="mm5309-native-linking-failed-with-error-code-1--check-build-log-for-details"></a>MM5309: Verknüpfen von systemeigenen Fehler mit Fehlercode 1.  Überprüfen Sie die Buildprotokoll für Details.

<a name="MM5310" />

#### <a name="mm5310-installnametool-failed-with-an-error-code-0-check-build-log-for-details"></a>MM5310: Install_name_tool Fehler mit dem Fehlercode "{0}". Überprüfen Sie die Buildprotokoll für Details.

<!-- MM6xxx: mmp internal tools -->
<!-- MM7xxx: reserved -->

## <a name="mm8xxx-runtime"></a>MM8xxx: Common Language Runtime

### <a name="mm800x-misc"></a>MM800x: sonstige

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

