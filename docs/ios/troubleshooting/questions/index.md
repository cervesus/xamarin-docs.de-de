---
title: Xamarin.iOS-häufig gestellte Fragen
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 65E04188-185D-493D-BA3C-A89711CB6CAF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 8b90b76cad5277fe76fc476a0bcd6f600e91b256
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61421826"
---
# <a name="ios-frequently-asked-questions"></a>iOS-häufig gestellte Fragen

## <a name="general-questions"></a>Allgemeine Fragen

### <a name="can-i-use-a-mac-vm-with-xamarinmac-vmmd"></a>[Kann ich eine Mac-VM mit Xamarin verwenden?](mac-vm.md)
Ja, aber nur auf Mac-Hardware.

### <a name="how-can-i-downgrade-xcodedowngrade-xcodemd"></a>[Wie kann ich Xcode herabstufen?](downgrade-xcode.md)
Dieses Handbuch enthält Links zu früheren Versionen von Xcode sowie die neueste Version zugreifen.

### <a name="where-can-i-set-my-ios-sdk-locationsios-sdkmd"></a>[Wo kann ich meine iOS SDK-Speicherorte festlegen?](ios-sdk.md)
Bei den meisten Benutzern werden diese automatisch in die richtigen Speicherorte festgelegt. Dieses Handbuch enthält die SDK-Standardspeicherorte und wie sie bei Bedarf ändern.

### <a name="how-can-i-reenable-developer-options-after-updating-iosupdate-developer-optionsmd"></a>[Wie kann ich Entwickleroptionen wieder aktivieren, nach der iOS-Update?](update-developer-options.md)
Ein iOS-Fehler verursacht möglicherweise die Entwickleroptionen auf nicht mehr angezeigt, nach dem Aktualisieren von iOS-Versionen, dies festgestellt wurde beim Wechsel in iOS 8.x. Dieser Leitfaden beschreibt, wie die Optionen erneut aktiviert werden können.

### <a name="user-location-not-working-in-ios-8ios8-user-locationmd"></a>[Standort des Benutzers funktioniert in iOS 8 nicht](ios8-user-location.md)
Dieses Handbuch beschreibt, wie Sie das Bearbeiten der Datei "Info.plist", um Benutzerstandort in iOS 8 zu aktivieren.

### <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logssymbolicate-ios-crashmd"></a>[Wo finde ich die DSYM-Datei zum Ersetzen der iOS-Absturzprotokolle durch Symbole?](symbolicate-ios-crash.md)
Dieses Handbuch beschreibt die grundlegenden Schritte für iOS-absturzprotokolle beim Diagnostizieren von Abstürzen symbolicating. Auch verknüpft auf zusätzliche Ressourcen für erweiterte Techniken der absturzgruppierung & Informationen zum Interpretieren von iOS-absturzprotokolle.


### <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studioxs-mono-runtimemd"></a>[Wie richte ich Mono-Runtime-Umgebungsvariablen für iOS-Projekte in Xamarin Studio ein?](xs-mono-runtime.md)
Wenn Sie Common Language Runtime-Umgebungsvariablen für Mono festlegen müssen, können festgelegt werden der **Projektoptionen > ausführen > Allgemein** Seite.

## <a name="publishing-questions"></a>Veröffentlichen Fragen

### <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submissioninvalid-bundle-bitcodemd"></a>[Fehler beim Senden von App-Store: "Ungültiges Paket: Optionen, die nicht in Bitcode eingebettet werden können, werden in der Übermittlung erkannt"](invalid-bundle-bitcode.md)

Übermitteln von apps, die _erfordern_ Bitcode, z. B. WatchOS und TvOS-apps muss Xcode 9 ausgeführt werden.

### <a name="can-i-change-the-output-path-of-the-ipa-fileipa-output-pathmd"></a>[Kann ich den Ausgabepfad der IPA-Datei ändern?](ipa-output-path.md)
Ab Xamarin Cycle 7 können Sie benutzerdefinierte MSBuild-Ziele verwenden, um dies zu erreichen.

### <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folderipa-tfsmd"></a>[Wie kann ich die IPA-Ausgabedateien in den TFS-Ablageordner kopieren?](ipa-tfs.md)
Ja, diese Anleitung wird beschrieben, wie.

### <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studiomodify-ipamd"></a>[Kann ich Dateien hinzufügen oder Entfernen von Dateien aus einer IPA-Datei nach der Erstellung in Visual Studio?](modify-ipa.md)
Ja, es ist möglich, aber es ist in der Regel erforderlich, wenn Sie sich erneut anmelden die `.app` Paket nach der Änderung. Beachten Sie, dass das Ändern der `.ipa` Datei ist nicht erforderlich, während normaler Verwendung. In diesem Artikel wird nur zu Informationszwecken bereitgestellt.

### <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studiocreate-xcarchivemd"></a>[Ist es möglich, ein xcarchive-Archivs in Visual Studio zu erstellen?](create-xcarchive.md)
Ab Xamarin 4, ist es jetzt möglich, erstellen Sie eine `.xcarchive` von Windows durch Festlegen der `ArchiveOnBuild` Eigenschaft `true`.

### <a name="why-does-my-app-submission-fail-with-disallowed-paths--itunesmetadataplist--found-at--itunesmetadata-disallowed-pathsmd"></a>[Warum schlägt meine App-Übermittlung wie folgt fehl: "Disallowed paths ( "iTunesMetadata.plist" ) found at ..." (Nicht zulässige Pfade ( "iTunesMetadata.plist" ) unter...)](itunesmetadata-disallowed-paths.md)
Dieser Fehler ist das Ergebnis einer Änderung im Apple App Store-Überprüfungsprozess. Dieser Fehler _nicht_ im Zusammenhang mit der bestimmten Version von Xamarin, die Sie installiert haben, wird also ein Downgrade _nicht_ helfen. Dieses Handbuch enthält Links zu weiteren Informationen dazu, wie zum Beheben des Problems.


## <a name="diagnosing-specific-error-messages"></a>Diagnose von Fehlermeldungen

### <a name="ios-designer-error-with-registerserviceporterror-registerserviceportmd"></a>[iOS-Designer-Fehler mit RegisterServicePort](error-registerserviceport.md)
Fehler bei `RegisterServicePort` und ähnliche Fehlermeldungen wie oben sind häufig ein Problem mit Spyware/Schadsoftware auf dem Computer. In dieser Anleitung, bestätigen die Diagnose und die Informationen zum Entfernen von Spyware/Malware.

### <a name="why-does-my-ios-build-fail-with-no-valid-iphone-code-signing-keys-found-in-keychainno-codesigning-keysmd"></a>[Warum schlägt mein iOS-Build fehl mit: Keine gültigen iPhone-Codesignaturschlüssel in Keychain gefunden?](no-codesigning-keys.md)
Diese Fehlermeldung tritt auf, wenn das betreffende Projekt gültige Anmeldeinformationen im Code-signing sucht jedoch nicht zu finden. Signieren von Code ist für Tests und Bereitstellungen auf physischen iOS-Geräten erforderlich. als auch Ad-hoc- und App Builds zu speichern.

### <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-objectexception-marshal-obj-cmd"></a>[Warum schlägt meine iOS 9-App wie folgt fehl: System.Exception: Failed to marshal the Objective-C object (Das Objective-C-Objekt konnte nicht gemarshallt werden.)](exception-marshal-obj-c.md)
API-Änderungen in iOS 9 erfordern, dass ein Rückruf-Konstruktor verwendet werden, beim Aufrufen von nicht verwalteten Codes, als die zugrunde liegende API jetzt erwartet.

### <a name="runtime-error-the-assembly-mscorlibdll-was-not-found-or-could-not-be-loadederror-mscorlib-not-foundmd"></a>[Laufzeitfehler: The assembly mscorlib.dll was not found or could not be loaded (Die Assembly „mscorlib.dll“ wurde nicht gefunden oder konnte nicht geladen werden).](error-mscorlib-not-found.md)
Dieses Problem tritt auf, wenn die *ausgeblendete* `.monotouch-32` und `.monotouch-64` Ordner fehlen die `.xcarchive` zum Signieren / IPA-Datei erstellen, die Laufzeitfehler ausgelöst.

## <a name="deprecated"></a>Als veraltet markiert

> [!IMPORTANT]
> Die folgenden Artikel gelten für Probleme, die in früheren Versionen von Xamarin aufgelöst wurden. Jedoch, wenn das Problem auf die neueste Version der Software auftritt, melden Sie bitte eine [neuer Fehler](~/cross-platform/troubleshooting/questions/howto-file-bug.md) erstellen Sie mit der Ihre vollständige versionsverwaltung Informationen "und" vollständig "Protokollausgabe.



### <a name="ipa-file-is-0-bytesipa-zero-bytesmd"></a>[IPA-Datei hat 0 Byte](ipa-zero-bytes.md)
Gab es einige bekannte Probleme in früheren Versionen von Xamarin, die die IPA-Datei für Windows auf 0 Byte sein verursachen können.

### <a name="ibtool-error-the-operation-couldnt-be-completederror-ibtoolmd"></a>[IBTool-Fehler: Der Vorgang konnte nicht abgeschlossen werden.](error-ibtool.md)
Apple [festen](https://developer.apple.com/library/ios/releasenotes/DeveloperTools/RN-Xcode/Chapters/xc6_release_notes.html) dies `ibtool` Fehler in Xcode 6.1.1, also mit Xcode 6.1.1 aktualisieren oder höher ist die einfachste Lösung.

### <a name="error-mt1009-could-not-copy-the-assemblyerror-mt1009md"></a>[Fehler MT1009: Die Assembly konnte nicht kopiert werden.](error-mt1009.md)
Dies wirkt sich auf Benutzer, die mit Xamarin.iOS 7.2.6 aus. Dieses Problem ist aufgrund der Dateiberechtigungen, die erhöhte Rechte erforderlich, wenn Xamarin.iOS, und ein anderes Benutzerkonto installiert wird Hauptkonto des Entwicklers.

### <a name="systemexception-amdevicenotificationsubscribe-returned-exception-amddevicenotificationsubscribemd"></a>[System.Exception AMDeviceNotificationSubscribe zurückgegeben ...](exception-amddevicenotificationsubscribe.md)
Diese Meldung kann in ein Fehlerdialogfeld angezeigt, wenn Sie Visual Studio für Mac oder in zunächst die `mtbserver.log` Datei. Beachten Sie, dass es sich um ein ungewöhnlich, dass Problem handelt. Wenn Visual Studio beim Herstellen einer Verbindung mit dem Mac-buildhost Probleme sollten ist, stehen Ihnen andere Fehler, die eher in angezeigt werden, sind die `mtbserver.log` Datei.

### <a name="mdocarchivetomsxdocconverterexe-not-found-rverbasecommandonrequestmdocarchivetomsxdocconverter-not-foundmd"></a>[„MDocArchiveToMsxDocConverter.exe“ nicht gefunden. rver.BaseCommand.OnRequest](mdocarchivetomsxdocconverter-not-found.md)
Dieser Fehler kann auftreten, der `Mac Server Log` in Visual Studio.
