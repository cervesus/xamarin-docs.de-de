---
title: Xamarin. Mac-Fehlermeldungen (MMP)
description: In diesem Dokument werden die von MMP generierten Fehler aufgeführt, das Tool zum Verpacken kompilierter Assemblys in eine ausführbare Macintosh-Anwendung.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5B26339F-A202-4E41-9229-D0BC9E77868E
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/27/2018
ms.openlocfilehash: 48399d35d27a700fa0b24583cce9cd0335f0e354
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84572077"
---
# <a name="xamarinmac-error-messages-mmp"></a>Xamarin. Mac-Fehlermeldungen (MMP)

## <a name="mm0xxx-mmp-error-messages"></a>MM0xxx: MMP-Fehlermeldungen

Beispiel: Parameter, Umgebung, fehlende Tools.

<a name="MM0000"></a>

#### <a name="mm0000-unexpected-error---please-file-a-bug-report-at-httpsgithubcomxamarinxamarin-maciosissuesnew"></a>MM0000: Unerwarteter Fehler: Bitte melden Sie einen Fehlerbericht unterhttps://github.com/xamarin/xamarin-macios/issues/new

Es ist ein unerwarteter Fehler aufgetreten. Melden Sie [einen Fehlerbericht](https://github.com/xamarin/xamarin-macios/issues/new) mit so vielen Informationen wie möglich, einschließlich:

* Vollständige Buildprotokolle mit maximaler Ausführlichkeit (z. b. `-v -v -v -v` in den **zusätzlichen MMP-Argumenten**);
* Ein minimaler Testfall, der den Fehler reproduziert. immer
* Alle Versionsinformationen

Die einfachste Möglichkeit, um genaue Versionsinformationen zu erhalten, ist die Verwendung des Menüs " **Xamarin Studio** ", Informationen zu **Xamarin Studio** Element, Schaltfläche " **Details anzeigen** " und Kopieren/Einfügen der Versionsinformationen (die Schaltfläche " **Informationen kopieren** " können Sie verwenden).

<a name="MM0001"></a>

#### <a name="mm0001-this-version-of-xamarinmac-requires-mono-0-the-current-mono-version-is-1-please-update-the-monoframework-from-httpmono-projectcomdownloads"></a>MM0001: für diese Version von xamarin. Mac ist Mono erforderlich {0} (die aktuelle Mono-Version ist {1} ). Aktualisieren Sie Mono. Framework vonhttp://mono-project.com/Downloads

<a name="MM0003"></a>

#### <a name="mm0003-application-name-0exe-conflicts-with-an-sdk-or-product-assembly-dll-name"></a>MM0003: der Anwendungsname " {0} . exe" steht in Konflikt mit einem SDK-oder produktassemblynamen (. dll).

<a name="MM0007"></a>

#### <a name="mm0007-the-root-assembly-0-does-not-exist"></a>MM0007: die Stammassembly " {0} " ist nicht vorhanden.

<a name="MM0008"></a>

#### <a name="mm0008-you-should-provide-one-root-assembly-only-found-0-assemblies-1"></a>MM0008: Sie sollten nur eine Stammassembly bereitstellen, Assemblys gefunden {0} : " {1} "

<a name="MM0009"></a>

#### <a name="mm0009-error-while-loading-assemblies-"></a>MM0009: Fehler beim Laden von Assemblys: *.

Fehler beim Laden der Assemblys aus den Stamm-Assemblyverweisen. Weitere Informationen finden Sie möglicherweise in der Buildausgabe.

<a name="MM0010"></a>

#### <a name="mm0010-could-not-parse-the-command-line-arguments-0"></a>MM0010: die Befehlszeilenargumente konnten nicht analysiert werden:{0}

<!-- 0013 is unused -->

<a name="MM0016"></a>

#### <a name="mm0016-the-option-0-has-been-deprecated"></a>MM0016: die Option " {0} " wurde als veraltet markiert.

<a name="MM0017"></a>

#### <a name="mm0017-you-should-provide-a-root-assembly"></a>MM0017: Geben Sie eine Stammassembly an.

<a name="MM0018"></a>

#### <a name="mm0018-unknown-command-line-argument-0"></a>MM0018: unbekanntes Befehlszeilenargument: " {0} "

<a name="MM0020"></a>

#### <a name="mm0020-the-valid-options-for-0-are-1"></a>MM0020: die gültigen Optionen für " {0} " sind " {1} ".

<a name="MM0023"></a>

#### <a name="mm0023-application-name-0exe-conflicts-with-another-user-assembly"></a>MM0023: der Anwendungsname " {0} . exe" steht in Konflikt mit einer anderen Benutzerassembly.

<a name="MM0026"></a>

#### <a name="mm0026-could-not-parse-the-command-line-argument-0-1"></a>MM0026: das Befehlszeilenargument "" konnte nicht analysiert werden {0} :{1}

<a name="MM0043"></a>

#### <a name="mm0043-the-boehm-garbage-collector-is-not-supported-the-sgen-garbage-collector-has-been-selected-instead"></a>MM0043: der Boehm-Garbage Collector wird nicht unterstützt. Stattdessen wird die SGen-Garbage Collector ausgewählt.

<a name="MM0050"></a>

#### <a name="mm0050-you-cannot-provide-a-root-assembly-if---no-root-assembly-is-passed"></a>MM0050: eine Stammassembly kann nicht bereitgestellt werden, wenn "--No-root-Assembly" nicht erfolgreich ist.

<a name="MM0051"></a>

#### <a name="mm0051-an-output-directory---output-is-required-if---no-root-assembly-is-passed"></a>MM0051: ein Ausgabeverzeichnis (--Output) ist erforderlich, wenn "--No-root-Assembly" erfolgreich ist.

<a name="MM0053"></a>

#### <a name="mm0053-cannot-disable-new-refcount-with-the-unified-api"></a>MM0053: der neue Ref count-Befehl kann nicht mit dem Unified API deaktiviert werden.

<a name="MM0056"></a>

#### <a name="mm0056-cannot-find-xcode-in-any-of-our-default-locations-please-install-xcode-or-pass-a-custom-path-using---sdkrootpath"></a>MM0056: Xcode kann in keinem unserer Standard Speicherorte gefunden werden. Installieren Sie Xcode, oder übergeben Sie mithilfe von--SDKRoot = einen benutzerdefinierten Pfad.\<path>

<a name="MM0059"></a>

#### <a name="mm0059-could-not-find-the-currently-selected-xcode-on-the-system-0"></a>MM0059: der derzeit ausgewählte Xcode wurde im System nicht gefunden: {0} ;

<a name="MM0060"></a>

#### <a name="mm0060-could-not-find-the-currently-selected-xcode-on-the-system-xcode-select---print-path-returned-0-but-that-directory-does-not-exist"></a>MM0060: der aktuell ausgewählte Xcode wurde im System nicht gefunden. ' Xcode-Select--Print-Path ' hat ' {0} ' zurückgegeben, aber dieses Verzeichnis ist nicht vorhanden.

<a name="MM0068"></a>

#### <a name="mm0068-invalid-value-for-target-framework-0"></a>MM0068: Ungültiger Wert für Ziel Framework: {0} .

<a name="MM0071"></a>

#### <a name="mm0071-unknown-platform--this-usually-indicates-a-bug-in-xamarinmac-please-file-a-bug-report-at-httpsbugzillaxamarincom-with-a-test-case"></a>MM0071: Unbekannte Plattform: *. Dies weist normalerweise auf einen Fehler in xamarin. Mac hin. Melden Sie einen Fehlerbericht bei https://bugzilla.xamarin.com mit einem Testfall.

Dies weist normalerweise auf einen Fehler in xamarin. Mac hin. Melden Sie einen Fehlerbericht bei [https://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=Xamarin.Mac) mit einem Testfall.

<a name="MM0073"></a>

#### <a name="mm0073-xamarinmac--does-not-support-a-deployment-target-of--the-minimum-is--please-select-a-newer-deployment-target-in-your-projects-infoplist"></a>MM0073: xamarin. Mac \* unterstützt kein Bereitstellungs Ziel von \* (das minimal ist \* ). Wählen Sie in der Datei "Info. plist" des Projekts ein neueres Bereitstellungs Ziel aus.

Das minimale Bereitstellungs Ziel ist das in der Fehlermeldung angegebene. Wählen Sie in der Datei "Info. plist" des Projekts ein neueres Bereitstellungs Ziel aus.

Wenn das Aktualisieren des Bereitstellungs Ziels nicht möglich ist, verwenden Sie eine ältere Version von xamarin. Mac.

<a name="MM0074"></a>

#### <a name="mm0074-xamarinmac--does-not-support-a-deployment-target-of--the-maximum-is--please-select-an-older-deployment-target-in-your-projects-infoplist-or-upgrade-to-a-newer-version-of-xamarinmac"></a>MM0074: xamarin. Mac \* unterstützt kein Bereitstellungs Ziel von \* (das Maximum ist \* ). Wählen Sie in der Info. plist-Datei des Projekts ein älteres Bereitstellungs Ziel aus, oder führen Sie ein Upgrade auf eine neuere Version von xamarin. Mac aus.

Xamarin. Mac unterstützt nicht das Festlegen des minimalen Bereitstellungs Ziels auf eine höhere Version als die Version, für die diese bestimmte Version von xamarin. Mac erstellt wurde.

Wählen Sie ein älteres Minimum Bereitstellungs Ziel in der Info. plist-Datei des Projekts aus, oder führen Sie ein Upgrade auf eine neuere Version von xamarin. Mac aus.

<a name="MM0079"></a>

#### <a name="mm0079-internal-error---no-executable-was-copied-into-the-app-bundle-please-contact-supportxamarincom"></a>MM0079: Interner Fehler: Es wurde keine ausführbare Datei in die APP Bundle kopiert. Wenden Sie sich an support@xamarin.com

<a name="MM0080"></a>

#### <a name="mm0080-disabling-newrefcount---new-refcountfalse-is-deprecated"></a>MM0080: die Deaktivierung von newrefcount,--New-refcount: false, ist veraltet.

<!-- 0088 used by mtouch -->
<!-- 0089 used by mtouch -->

<a name="MM0091"></a>

#### <a name="mm0091-this-version-of-xamarinmac-requires-the--sdk-shipped-with-xcode--either-upgrade-xcode-to-get-the-required-header-files-or-use-the-dynamic-registrar-or-set-the-managed-linker-behaviour-to-link-platform-or-link-framework-sdks-only-to-try-to-avoid-the-new-apis"></a>MM0091: für diese Version von xamarin. Mac ist das \* SDK erforderlich (geliefert mit Xcode \* ). Aktualisieren Sie Xcode, um die erforderlichen Header Dateien zu erhalten, oder verwenden Sie die dynamische Registrierungsstelle, oder legen Sie das verwaltete Linker-Verhalten so fest, dass nur Plattform-oder Verknüpfungs Framework-sdgs verknüpft werden (um die neuen APIs zu vermeiden

Xamarin. Mac erfordert, dass die Header Dateien aus der in der Fehlermeldung angegebenen SDK-Version erstellt werden, um die Anwendung mit der statischen Registrierungsstelle zu erstellen. Die empfohlene Vorgehensweise zum Beheben dieses Fehlers besteht darin, Xcode zu aktualisieren, um das erforderliche SDK zu erhalten. Dies schließt alle erforderlichen Header Dateien ein. Wenn Sie mehrere Versionen von Xcode installiert haben oder einen Xcode an einem nicht standardmäßigen Speicherort verwenden möchten, stellen Sie sicher, dass Sie den korrekten Xcode-Speicherort in den Einstellungen Ihrer IDE festlegen.

Eine mögliche alternative Lösung besteht darin, den verwalteten Linker zu aktivieren. Dadurch wird die nicht verwendete API entfernt. Dies gilt in den meisten Fällen für die neue API, bei der die Header Dateien fehlen (oder unvollständig). Dies funktioniert jedoch nicht, wenn Ihr Projekt API verwendet, die in einem neueren SDK eingeführt wurde, als das, das Ihr Xcode bereitstellt.

Eine zweite mögliche alternative Lösung besteht darin, stattdessen die dynamische Registrierungsstelle zu verwenden. Dies führt zu Startkosten durch dynamisches Registrieren von Typen, wobei jedoch die Anforderung der Header Datei entfernt wird.

Eine letzte Lösung besteht darin, eine ältere Version von xamarin. Mac zu verwenden, eine, die das für Ihr Projekt erforderliche SDK unterstützt.

<a name="MM0097"></a>

#### <a name="mm0097-machineconfig-file-0-can-not-be-found"></a>MM0097: die Datei "Machine. config" {0} kann nicht gefunden werden.

<a name="MM0098"></a>

#### <a name="mm0098-aot-compilation-is-only-available-on-unified"></a>MM0098: die AOT-Kompilierung ist nur auf Unified verfügbar.

<a name="MM0099"></a>

#### <a name="mm0099-internal-error-0-please-file-a-bug-report-with-a-test-case-httpsbugzillaxamarincom"></a>MM0099: Interner Fehler {0} . Melden Sie einen Fehlerbericht mit einem Testfall () https://bugzilla.xamarin.com) .

<a name="MM0114"></a>

#### <a name="mm0114-hybrid-aot-compilation-requires-all-assemblies-to-be-aot-compiled"></a>MM0114: für die Hybrid-AOT-Kompilierung müssen alle Assemblys mit AOT kompiliert werden.

<a name="MM0129"></a>

#### <a name="mm0129-debugging-symbol-file-for--does-not-match-the-assembly-and-is-ignored"></a>MM0129: die Debugsymboldatei für "*" entspricht nicht der Assembly und wird ignoriert.

Die Debugsymbole – entweder eine PDB-Datei (nur Portable PDB) oder eine MDB-Datei – für die angegebene Assembly konnte nicht geladen werden.

Dies bedeutet im Allgemeinen, dass die Assembly neuer oder älter als die Symbole ist. Da Sie nicht identisch sind, können Sie nicht verwendet werden, und die Symbole werden ignoriert.

Diese Warnung wirkt sich nicht auf die zu erstellende Anwendung aus, kann jedoch möglicherweise nicht vollständig debuggt werden (insbesondere der Code aus der angegebenen Assembly). Außerdem fehlen bei Ausnahmen, Stapel Überwachungen und Absturzberichten möglicherweise einige Informationen.

Melden Sie dieses Problem dem Verleger des assemblypakets (z. b. nuget-Autor), damit dies in zukünftigen Versionen behoben werden kann.

<a name="MM0130"></a>

#### <a name="mm0130-no-root-assemblies-found-you-should-provide-at-least-one-root-assembly"></a>MM0130: Es wurden keine Stammassemblys gefunden. Sie sollten mindestens eine Stammassembly bereitstellen.

Wenn Sie Ausführen `--runregistrar` , sollte mindestens eine Stammassembly bereitgestellt werden.

<a name="MM0131"></a>

#### <a name="mm0131-product-assembly-0-not-found-in-assembly-list-1"></a>MM0131: die produktassembly " {0} " wurde nicht in der Assemblyliste gefunden: " {1} "

Wenn Sie Ausführen `--runregistrar` , sollte die Assemblyliste die produktassembly xamarin. Mac, xammac enthalten.

<a name="MM0132"></a>

#### <a name="mm0132-unknown-optimization--valid-values-are-"></a>MM0132: Unbekannte Optimierung: \* . Gültige Werte sind:\*

Die angegebene Optimierung wurde nicht erkannt.

Das akzeptierte Format ist `[+|-]optimization-name` , wobei `optimization-name` einer der in der Fehlermeldung aufgelisteten Werte ist.

Eine umfassende Beschreibung der einzelnen Optimierungen finden Sie unter [buildoptimierungen](~/cross-platform/macios/optimizations.md) .

<a name="MM0133"></a>

#### <a name="mm0133-found-more-than-1-assembly-matching-0-choosing-first-1"></a>MM0133: Es wurden mehr als 1 Assemblys gefunden, die mit " {0} " zuerst ausgewählt werden: " {1} "

<a name="MM0134"></a>

#### <a name="mm0134-32-bit-applications-should-be-migrated-to-64-bit"></a>MM0134:32-Bit-Anwendungen sollten zu 64-Bit migriert werden.

Apple hat angekündigt, dass keine Übermittlungen von macOS-App-Stores von 32-Bit-Apps (ab dem 2018. Januar) zugelassen werden.

Darüber hinaus werden 32-Bit-Anwendungen nicht auf der macOS-Version ausgeführt, nachdem High Sierra "ohne Kompromisse" ausgeführt wurde.

Weitere Informationen finden Sie hier:https://developer.apple.com/news/?id=06282017a

Sie sollten Ihre Anwendung und alle Abhängigkeiten auf 64-Bit aktualisieren.

<a name="MM0135"></a>

#### <a name="mm0135-did-not-link-system-framework-0-referenced-by-assembly-1-because-it-was-introduced-in-2-3-and-were-using-the-2-4-sdk"></a>MM0135: das System Framework ' ', {0} auf das von Assembly ' {1} ' verwiesen wurde, wurde nicht verknüpft, weil es in eingeführt wurde {2} {3} , und das SDK wird verwendet {2} {4} .

Zum Erstellen der Anwendung muss xamarin. Mac mit Systembibliotheken verknüpft werden, von denen einige von der in der Fehlermeldung angegebenen SDK-Version abhängen. Da Sie eine ältere Version des SDK verwenden, können Aufrufe dieser APIs zur Laufzeit fehlschlagen.

Die empfohlene Vorgehensweise zum Beheben dieses Fehlers besteht darin, Xcode zu aktualisieren, um das erforderliche SDK zu erhalten. Wenn Sie mehrere Versionen von Xcode installiert haben oder einen Xcode an einem nicht standardmäßigen Speicherort verwenden möchten, stellen Sie sicher, dass Sie den korrekten Xcode-Speicherort in den Einstellungen Ihrer IDE festlegen.

Sie können auch den verwalteten [Linker](https://docs.microsoft.com/xamarin/mac/deploy-test/linker) zum Entfernen nicht verwendeter APIs aktivieren, einschließlich (in den meisten Fällen) der neuen, die die angegebene Bibliothek erfordern. Dies funktioniert jedoch nicht, wenn Ihr Projekt APIs erfordert, die in einem neueren SDK eingeführt werden, als das, das Ihr Xcode bereitstellt.

Verwenden Sie als letzte Lösung eine ältere Version von xamarin. Mac, bei der diese neuen SDOs während des Buildprozesses nicht vorhanden sein müssen.

## <a name="mm1xxx-file-copy--symlinks-project-related"></a>MM1xxx: File Copy/symlinks (projektbezogen)

<a name="MM1034"></a>

#### <a name="mm1034-could-not-create-symlink-file---target-error-number"></a>MM1034: der Symlink "{file}" konnte nicht erstellt werden-> "{target}": Fehler {Number}

### <a name="mm14xx-product-assemblies"></a>MM14xx: produktenassemblys

<a name="MM1401"></a>

#### <a name="mm1401-the-required-0-assembly-is-missing-from-the-references"></a>MM1401: die erforderliche {0} Assembly "" fehlt in den verweisen.

<a name="MM1402"></a>

#### <a name="mm1402-the-assembly-0-is-not-compatible-with-this-tool"></a>MM1402: die Assembly " {0} " ist mit diesem Tool nicht kompatibel.

<a name="MM1403"></a>

#### <a name="mm1403-0-1-could-not-be-found-target-framework-0-is-unusable-to-package-the-application"></a>MM1403: {0} " {1} " konnte nicht gefunden werden. Das Ziel Framework ' {0} ' ist zum Packen der Anwendung nicht verwendbar.

<a name="MM1404"></a>

#### <a name="mm1404-target-framework-0-is-invalid"></a>MM1404: das Ziel Framework " {0} " ist ungültig.

<a name="MM1405"></a>

#### <a name="mm1405-usefullxammacframework-must-always-target-framework-net-45-not-0-which-is-invalid"></a>MM1405: usefullxammacframework muss immer auf Framework .NET 4,5 abzielen, nicht auf " {0} ", was ungültig ist.

<a name="MM1406"></a>

#### <a name="mm1406-target-framework-0-is-invalid-when-targeting-xamarinmac-45-net-framwork"></a>MM1406: das Ziel Framework " {0} " ist ungültig, wenn xamarin. Mac 4,5 .net framwork als Ziel verwendet wird.

<a name="MM1407"></a>

#### <a name="mm1407-mismatch-between-xamarinmac-reference-0-and-target-framework-selected-1"></a>MM1407: Konflikt zwischen xamarin. Mac-Verweis ' {0} ' und ausgewähltem Ziel Framework ' {1} '.

### <a name="mm15xx-assembly-gathering-not-requiring-linker-errors"></a>MM15xx: Fehler beim Sammeln von Assemblys (keine Linker-Anforderung)

<a name="MM1501"></a>

#### <a name="mm1501-can-not-resolve-reference-0"></a>MM1501: der Verweis kann nicht aufgelöst werden:{0}

### <a name="machocs"></a>MachO.cs

<a name="MM1600"></a>

#### <a name="mm1600-not-a-mach-o-dynamic-library-unknown-header-0x0-1"></a>MM1600: keine dynamische Mach-O-Bibliothek (Unbekannter Header "0x {0} "): {1} .

<a name="MM1601"></a>

#### <a name="mm1601-not-a-static-library-unknown-header-0-1"></a>MM1601: keine statische Bibliothek (Unbekannter Header " {0} "): {1} .

<a name="MM1602"></a>

#### <a name="mm1602-not-a-mach-o-dynamic-library-unknown-header-0x0-1"></a>MM1602: keine dynamische Mach-O-Bibliothek (Unbekannter Header "0x {0} "): {1} .

<a name="MM1603"></a>

#### <a name="mm1603-unknown-format-for-fat-entry-at-position-0-in-1"></a>MM1603: unbekanntes Format für FAT-Eintrag an Position {0} in {1} .

<a name="MM1604"></a>

#### <a name="mm1604-file-of-type-0-is-not-a-macho-file-1"></a>MM1604: die Datei vom Typ {0} ist keine Macho-Datei ( {1} ).

## <a name="mm2xxx-linker"></a>MM2xxx: Linker

### <a name="mm20xx-linker-general-errors"></a>MM20xx: Linker-Fehler (allgemein)

<a name="MM2001"></a>

#### <a name="mm2001-could-not-link-assemblies"></a>MM2001: Assemblys konnten nicht verknüpft werden.

<a name="MM2002"></a>

#### <a name="mm2002-can-not-resolve-reference-0"></a>MM2002: der Verweis kann nicht aufgelöst werden:{0}

<a name="MM2003"></a>

#### <a name="mm2003-option-0-will-be-ignored-since-linking-is-disabled"></a>MM2003: die Option " {0} " wird ignoriert, da die Verknüpfung deaktiviert ist.

<a name="MM2004"></a>

#### <a name="mm2004-extra-linker-definitions-file-0-could-not-be-located"></a>MM2004: die zusätzliche Linker-Definitionsdatei " {0} " wurde nicht gefunden.

<a name="MM2005"></a>

#### <a name="mm2005-definitions-from-0-could-not-be-parsed"></a>MM2005: Definitionen von ' {0} ' konnten nicht analysiert werden.

<a name="MM2006"></a>

#### <a name="mm2006-native-library-0-was-referenced-but-could-not-be-found"></a>MM2006: die native Bibliothek " {0} " wurde referenziert, konnte aber nicht gefunden werden.

<a name="MM2007"></a>

#### <a name="mm2007-xamarinmac-unified-api-against-a-full-net-profile-does-not-support-linking-pass-the--nolink-flag"></a>MM2007: xamarin. Mac-Unified API für ein vollständiges .NET-Profil unterstützt keine Verknüpfungen. Übergeben Sie das-nolink-Flag.

<a name="MM2009"></a>

#### <a name="mm2009-referenced-by-01------this-message-is-related-to-mm2006-"></a>MM2009: referenziert von {0} . {1} \* \* Diese Nachricht bezieht sich auf MM2006     \*\*

<a name="MM2010"></a>

#### <a name="mm2010-unknown-httpmessagehandler-0-valid-values-are-httpclienthandler-default-cfnetworkhandler-or-nsurlsessionhandler"></a>MM2010: Unbekannter httpmessagehandler `{0}` . Gültige Werte sind httpclienthandler (Standard), cfnetworkhandler oder nsurlsessionhandler.

<a name="MM2011"></a>

#### <a name="mm2011-unknown-tlsprovider-0--valid-values-are-default-or-appletls"></a>MM2011: Unbekanntes tlsprovider " {0} .  Gültige Werte sind Default oder appletls.

<a name="MM2012"></a>

#### <a name="mm2012-only-first-0-of-1-referenced-by-warnings-shown--this-message-related-to-2009-"></a>MM2012: nur zuerst {0} {1} werden Warnungen angezeigt, auf die verwiesen wird. \*\*Diese Meldung bezieht sich auf 2009\*\*

<a name="MM2013"></a>

#### <a name="mm2013-failed-to-resolve-the-reference-to-0-referenced-in-1-the-app-will-not-include-the-referenced-assembly-and-may-fail-at-runtime"></a>MM2013: Fehler beim Auflösen des Verweises auf " {0} ", auf den in "" verwiesen wird {1} . Die app enthält nicht die Assembly, auf die verwiesen wird, und kann zur Laufzeit fehlschlagen.

<a name="MM2014"></a>

#### <a name="mm2014-xamarinmac-extensions-do-not-support-linking-request-for-linking-will-be-ignored--this-message-is-obsolete-in-xm-36-"></a>MM2014: xamarin. Mac-Erweiterungen unterstützen keine Verknüpfungen. Die Verbindungsanforderung wird ignoriert. \*\*Diese Meldung ist in XM 3.6 und höher veraltet.\*\*

<!-- 2015 used by mtouch -->

<a name="MM2016"></a>

#### <a name="mm2016-invalid-tlsprovider-0-option-the-only-valid-value-1-will-be-used"></a>MM2016: Ungültige tlsprovider- `{0}` Option. Der einzige gültige Wert `{1}` wird verwendet.

<a name="MM2017"></a>

#### <a name="mm2017-could-not-process-xml-description-0"></a>MM2017: die XML-Beschreibung konnte nicht verarbeitet werden:{0}

<a name="MM202x"></a>

#### <a name="mm202x-binding-optimizer-failed-processing-"></a>MM202x: Fehler bei der Verarbeitung des Bindungs Optimierers `...` .

<a name="MM2100"></a>

#### <a name="mm2100-xamarinmac-classic-api-does-not-support-platform-linking"></a>MM2100: xamarin. Mac Classic API unterstützt keine Platt Form Verknüpfung.

<a name="MM2103"></a>

#### <a name="mm2103-error-processing-assembly--"></a>MM2103: Fehler beim Verarbeiten der Assembly " \* ": *

Unerwarteter Fehler beim Verarbeiten einer Assembly.

Die Assembly, die das Problem verursacht, wird in der Fehlermeldung benannt. Um dieses Problem zu beheben, muss die Assembly in einem [Fehlerbericht](https://bugzilla.xamarin.com) zusammen mit einem vollständigen Buildprotokoll mit aktivierter Ausführlichkeit bereitgestellt werden (d. h. `-v -v -v -v` in den **zusätzlichen maddress-Argumenten**).

<a name="MM2104"></a>

#### <a name="mm2104-unable-to-link-assembly-0-as-it-is-mixed-mode"></a>MM2104: die Assembly "" kann nicht verknüpft werden, {0} weil Sie im gemischten Modus ist.

Assemblys im gemischten Modus können nicht vom Linker verarbeitet werden.

https://docs.microsoft.com/cpp/dotnet/mixed-native-and-managed-assembliesWeitere Informationen zu Assemblys mit gemischtem Modus finden Sie unter.

<a name="MM2106"></a>

#### <a name="mm2106-could-not-optimize-the-call-to-blockliteralsetupblockunsafe-in--at-offset--because-"></a>MM2106: der aufzurufende blockliteral. setupblock [unsicher] konnte \* aufgrund von nicht optimiert werden \* \* .

Der Linker meldet diese Warnung, wenn er einen Rückruf von oder nicht optimieren kann `BlockLiteral.SetupBlock` `Block.SetupBlockUnsafe` .

Die Meldung verweist auf die Methode, die aufruft `BlockLiteral.SetupBlock[Unsafe]` , und kann auch darauf hinweisen, warum der Aufruf nicht optimiert werden konnte.

Bitte melden Sie ein [Problem](https://github.com/xamarin/xamarin-macios/issues/new) zusammen mit einem kompletten Buildprotokoll, damit wir untersuchen können, was schief gelaufen ist, und möglicherweise weitere Szenarios in Zukunft aktivieren.

<a name="MM2107"></a>

#### <a name="mm2107-its-not-safe-to-remove-the-dynamic-registrar-because-reasons"></a>MM2107: die dynamische Registrierungsstelle kann aufgrund von {Reasons} nicht sicher entfernt werden.

Der Linker meldet diese Warnung, wenn der Entwickler das Entfernen der dynamischen Registrierungsstelle (durch Übergabe `--optimize:remove-dynamic-registrar` an MMP) anfordert, der Linker aber feststellt, dass dies nicht sicher ist.

Um die Warnung zu entfernen, entfernen Sie entweder das Optimierungs Argument zu MMP, oder übergeben `--nowarn:2107` Sie, um es zu ignorieren.

Diese Option wird standardmäßig automatisch aktiviert, wenn dies möglich und sicher ist.

<a name="MM2108"></a>

#### <a name="mm2108-0-was-stripped-of-architectures-except-1-to-comply-with-app-store-restrictions-this-could-break-exisiting-codesigning-signatures-consider-stripping-the-library-with-lipo-or-disabling-with---optimize-trim-architectures"></a>MM2108: ' {0} ' wurde von Architekturen außer ' {1} ' entfernt, um die App Store-Einschränkungen einzuhalten. Dies könnte das Beenden der Signatur des Codierung unterbrechen. Entfernen Sie die Bibliothek mit der Lipo, oder deaktivieren Sie mit--optimieren =-Trim-Architekturen);

Der App Store lehnt nun Anwendungen ab, die Bibliotheken und Frameworks enthalten, die 32-Bit-Varianten enthalten. Die Bibliothek wurde beim Kopieren in das endgültige Anwendungspaket nicht verwendete Architekturen entfernt.

Diese ist allgemein sicher und reduziert die Anwendungs bündelgröße als zusätzlichen Vorteil. Allerdings wird für jedes gebündelte Framework, das Code signiert ist, die Signatur ungültig (und später zurückgesetzt, wenn die Anwendung signiert ist).

Verwenden Sie ggf `lipo` ., um die unnötigen Architekturen dauerhaft aus der Quell Bibliothek zu entfernen. Wenn die Anwendung nicht im App Store veröffentlicht wird, kann diese Entfernung durch das übergeben `--optimize=-trim-architectures` zusätzlicher MMP-Argumente deaktiviert werden.

<a name="MM2109"></a>

#### <a name="mm2109-xamarinmac-classic-api-does-not-support-platform-linking"></a>MM2109: xamarin. Mac Classic API unterstützt keine Platt Form Verknüpfung.

## <a name="mm3xxx-aot"></a>MM3xxx: AOT

### <a name="mm30xx-aot-general-errors"></a>MM30xx: AOT (allgemein)-Fehler

<a name="MM3001"></a>

#### <a name="mm3001-could-not-aot-the-assembly-0"></a>MM3001: die Assembly "" konnte nicht gefunden werden. {0}

<!-- 3002 used by mtouch -->
<!-- 3003 used by mtouch -->
<!-- 3004 used by mtouch -->
<!-- 3005 used by mtouch -->
<!-- 3006 used by mtouch -->
<!-- 3007 used by mtouch -->
<!-- 3008 used by mtouch -->

<a name="MM3009"></a>

#### <a name="mm3009-aot-of-0-was-requested-but-was-not-found"></a>MM3009: AOT von ' {0} ' wurde angefordert, wurde jedoch nicht gefunden.

<a name="MM3010"></a>

#### <a name="mm3010-exclusion-of-aot-of-0-was-requested-but-was-not-found"></a>MM3010: der Ausschluss von AOT von " {0} " wurde angefordert, wurde jedoch nicht gefunden.

## <a name="mm4xxx-code-generation"></a>MM4xxx: Codegenerierung

### <a name="mm40xx-driverm"></a>MM40xx: Driver. m

<a name="MM4001"></a>

#### <a name="mm4001-the-main-template-could-not-be-expanded-to-0"></a>MM4001: die Hauptvorlage konnte nicht auf erweitert werden `{0}` .

### <a name="mm41xx-registrar"></a>MM41xx: Registrierungsstelle

<a name="MM4134"></a>

#### <a name="mm4134-your-application-is-using-the-0-framework-which-isnt-included-in-the-macos-sdk-youre-using-to-build-your-app-this-framework-was-introduced-in-osx-2-while-youre-building-with-the-macos-1-sdk-this-configuration-is-not-supported-with-the-static-registrar-pass---registrardynamic-as-an-additional-mmp-argument-in-your-projects-mac-build-option-to-select-alternatively-select-a-newer-sdk-in-your-apps-mac-build-options"></a>MM4134: die Anwendung verwendet das Framework " {0} ", das nicht im MacOS SDK enthalten ist, das Sie zum Erstellen Ihrer APP verwenden (dieses Framework wurde in OSX eingeführt {2} , während Sie mit dem MacOS SDK arbeiten {1} ). Diese Konfiguration wird bei der statischen Registrierungsstelle (Pass--Registrar: Dynamic als zusätzliches MMP-Argument in der Mac-Buildoption Ihres Projekts für die Auswahl) nicht unterstützt. Wählen Sie alternativ ein neueres SDK in den Mac-Buildoptionen Ihrer APP aus.

<a name="MM4173"></a>

#### <a name="mm4173-the-registrar-cant-compute-the-block-signature-for-the-delegate-of-type-delegate-type-in-the-method-method-because-"></a>MM4173: die Registrierungsstelle kann die Block Signatur für den Delegaten vom Typ "{delegattype}" in der Methode "{Method}" nicht berechnen, weil *.

Diese Warnung gibt an, dass die Registrierungsstelle die Block Signatur der angegebenen Methode nicht in den generierten Registrierungscode einfügen konnte, da Sie von der Registrierungsstelle nicht berechnet werden konnte.

Dies bedeutet, dass die Block Signatur zur Laufzeit berechnet werden muss, was etwas langsamer ist.

Es gibt derzeit zwei mögliche Gründe für diese Warnung:

1. Der Typ des verwalteten Delegaten ist entweder `System.Delegate` oder `System.MulticastDelegate` . Diese Typen stellen keine bestimmte Signatur dar. Dies bedeutet, dass die Registrierungsstelle die entsprechende systemeigene Signatur nicht berechnen kann. In diesem Fall ist die Behebung der Verwendung eines bestimmten Delegattyps für den Block (Alternativ kann die Warnung ignoriert werden, indem Sie `--nowarn:4173` als zusätzliches MMP-Argument in den Mac-Buildoptionen des Projekts hinzugefügt wird).
2. Die Registrierungsstelle kann die-Methode des Delegaten nicht finden `Invoke` . Dies sollte nicht passieren. Bitte melden Sie ein [Problem](https://github.com/xamarin/xamarin-macios/issues/new) mit einem Testprojekt, damit wir es beheben können.

<a name="MT4174"></a>

#### <a name="mt4174-unable-to-locate-the-block-to-delegate-conversion-method-for-the-method-methods-parameter-parameter"></a>MT4174: der Block zum Delegieren der Konvertierungsmethode für den Parameter "# {Parameter}" der Methode "{Method}" wurde nicht gefunden.

Diese Warnung weist darauf hin, dass die statische Registrierungsstelle die Methode nicht finden konnte, um einen Delegaten für einen Ziel-C-Block zu erstellen. Zur Laufzeit wird ein Versuch unternommen, die Methode zu finden, aber es wird wahrscheinlich auch ein Fehler auftreten (mit einer MT8009-Ausnahme).

Ein möglicher Grund für diese Warnung ist das manuelle Schreiben von Bindungen für eine API, die Blöcke verwendet. Es wird empfohlen, ein Bindungs Projekt zu verwenden, um den Ziel-C-Code zu binden – insbesondere, wenn er Blöcke umfasst – da es recht kompliziert ist, ihn bei manueller Ausführung richtig zu machen.

Wenn dies nicht der Fall ist, melden Sie ein [Problem](https://github.com/xamarin/xamarin-macios/issues/new) mit einem Testfall.

## <a name="mm5xxx-gcc-and-toolchain"></a>MM5xxx: gcc und Toolkette

### <a name="mm51xx-compilation"></a>MM51xx: Kompilierung

<a name="MM5101"></a>

#### <a name="mm5101-missing-0-compiler-please-install-xcode-command-line-tools-component"></a>MM5101: der {0} Compiler ' ' fehlt. Installieren Sie die Xcode-Komponente "Befehlszeilen Tools".

<!-- 5102 used by mtouch -->

<a name="MM5103"></a>

#### <a name="mm5103-failed-to-compile-error-code---0-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MM5103: Kompilierung fehlgeschlagen. Fehlercode- {0} . Melden Sie einen Fehlerbericht unterhttp://bugzilla.xamarin.com

<!-- 5104 used by mtouch -->

### <a name="mm52xx-linking"></a>MM52xx: verknüpfen

<a name="MM5202"></a>

#### <a name="mm5202-monoframework-mdk-is-missing-please-install-the-mdk-for-your-monoframework-version-from-httpmono-projectcomdownloads"></a>MM5202: Mono. Framework-MDK fehlt. Installieren Sie das MDK für Ihre Mono. Framework-Version vonhttp://mono-project.com/Downloads

<a name="MM5203"></a>

#### <a name="mm5203-cant-find-libxammaca-likely-because-of-a-corrupted-xamarinmac-installation-please-reinstall-xamarinmac"></a>MM5203: libxammac. a kann aufgrund einer beschädigten xamarin. Mac-Installation nicht gefunden werden. Installieren Sie xamarin. Mac neu.

<a name="MM5204"></a>

#### <a name="mm5204-invalid-architecture-x86_64-is-only-supported-on-non-classic-profiles"></a>MM5204: Ungültige Architektur. x86_64 wird nur für nicht klassische Profile unterstützt.

<a name="MM5205"></a>

#### <a name="mm5205-invalid-architecture-0-valid-architectures-are-i386-and-x86_64-when---profilemobile"></a>MM5205: Ungültige Architektur " {0} ". Gültige Architekturen sind i386 und x86_64 (bei--profile = Mobile).

<a name="MM5218"></a>

#### <a name="mm5218-cant-ignore-the-dynamic-symbol-symbol---ignore-dynamic-symbolsymbol-because-it-was-not-detected-as-a-dynamic-symbol"></a>MM5218: das dynamische Symbol {Symbol} (--Ignore-Dynamic-Symbol = {Symbol}) kann nicht ignoriert werden, da es nicht als dynamisches Symbol erkannt wurde.

Weitere Informationen finden Sie in der [entsprechenden mberührungs-Warnung](~/ios/troubleshooting/mtouch-errors.md#MT5218).

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

### <a name="mm53xx-other-tools"></a>MM53xx: andere Tools

<a name="MM5301"></a>

#### <a name="mm5301-pkg-config-could-not-be-found-please-install-the-monoframework-from-httpmono-projectcomdownloads"></a>MM5301: pkg-config konnte nicht gefunden werden. Installieren Sie Mono. Framework vonhttp://mono-project.com/Downloads

<!-- 5302 used by mtouch -->
<!-- 5303 used by mtouch -->
<!-- 5304 used by mtouch -->

<a name="MM5305"></a>

#### <a name="mm5305-missing-otool-tool-please-install-xcode-command-line-tools-component"></a>MM5305: Fehlendes otool-Tool. Installieren Sie die Xcode-Komponente "Befehlszeilen Tools".

<a name="MM5306"></a>

#### <a name="mm5306-missing-dependencies-please-install-xcode-command-line-tools-component"></a>MM5306: fehlende Abhängigkeiten. Installieren Sie die Xcode-Komponente "Befehlszeilen Tools".

<a name="MM5308"></a>

#### <a name="mm5308-xcode-license-agreement-may-not-have-been-accepted--please-launch-xcode"></a>MM5308: die Xcode-Lizenzvereinbarung wurde möglicherweise nicht akzeptiert.  Starten Sie Xcode.

<a name="MM5309"></a>

#### <a name="mm5309-native-linking-failed-with-error-code-1-check-build-log-for-details"></a>MM5309: Fehler bei nativem verknüpfen mit Fehlercode 1. Weitere Informationen finden Sie im Buildprotokoll.

<a name="MM5310"></a>

#### <a name="mm5310-install_name_tool-failed-with-an-error-code-0-check-build-log-for-details"></a>MM5310: Fehler bei install_name_tool mit dem Fehlercode " {0} ". Weitere Informationen finden Sie im Buildprotokoll.

<a name="MM5311"></a>

#### <a name="mm5311-lipo-failed-with-an-error-code-0-check-build-log-for-details"></a>MM5311: Fehler bei der Lipo mit dem Fehlercode " {0} ". Weitere Informationen finden Sie im Buildprotokoll.

<!-- MM6xxx: mmp internal tools -->
<!-- MM7xxx: reserved -->

## <a name="mm8xxx-runtime"></a>MM8xxx: Laufzeit

### <a name="mm800x-misc"></a>MM800x: Sonstige

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

<a name="MM8017"></a>

#### <a name="mm8017-the-boehm-garbage-collector-is-not-supported-please-use-sgen-instead"></a>MM8017: der Boehm-Garbage Collector wird nicht unterstützt. Verwenden Sie stattdessen Sgen.

<a name="MM8025"></a>

#### <a name="mm8025-failed-to-compute-the-token-reference-for-the-type-typeassemblyqualifiedname-because-reasons"></a>MM8025: Fehler beim Berechnen des tokenverweises für den Typ "{Type. AssemblyQualifiedName} "wegen {Reasons}

Dies weist auf einen Fehler in xamarin. Mac hin. Bitte melden Sie einen Fehler unter [https://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=Xamarin.Mac) .

Eine mögliche Problem Umgehung besteht darin, die `register-protocols` Optimierung zu deaktivieren, indem Sie `--optimize:-register-protocols` als zusätzliches MMP-Argument in den Mac-Buildoptionen des Projekts übergeben wird.

<a name="MM8026"></a>

#### <a name="mm8026--is-not-supported-when-the-dynamic-registrar-has-been-linked-away"></a>MM8026: * wird nicht unterstützt, wenn die dynamische Registrierungsstelle entfernt wurde.

Dies weist normalerweise auf einen Fehler in xamarin. Mac hin, da die dynamische Registrierungsstelle bei Bedarf nicht verknüpft werden sollte. Bitte melden Sie einen Fehler unter [https://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) .

Es ist möglich, den Linker zu zwingen, die dynamische Registrierungsstelle beizubehalten, indem Sie `--optimize=-remove-dynamic-registrar` den zusätzlichen MMP-Argumenten in den Mac-Buildoptionen des Projekts hinzufügen.
