---
title: "Häufig gestellte Fragen (FAQs)"
ms.topic: article
ms.prod: xamarin
ms.assetid: 65E04188-185D-493D-BA3C-A89711CB6CAF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 1477d991fadfe381032e4009fa0bf0faf0518b8e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="frequently-asked-questions"></a>Häufig gestellte Fragen (FAQs)

## <a name="general-questions"></a>Allgemeine Fragen

### <a name="can-i-use-a-mac-vm-with-xamarinmac-vmmd"></a>[Kann ich eine Mac-VM mit Xamarin verwenden?](mac-vm.md)
Ja, aber nur auf Mac-Hardware.

### <a name="how-can-i-downgrade-xcodedowngrade-xcodemd"></a>[Wie kann ich Xcode heruntergestuft?](downgrade-xcode.md)
Dieses Handbuch enthält Links zu früheren Versionen von Xcode sowie die aktuelle Version zugreifen.

### <a name="where-can-i-set-my-ios-sdk-locationsios-sdkmd"></a>[Wo kann ich mein iOS-SDK-Verzeichnissen festlegen?](ios-sdk.md)
Für die meisten Benutzer werden diese automatisch in die richtigen Speicherorte festgelegt. Dieses Handbuch enthält die Standardspeicherorte für das SDK und wie sie bei Bedarf ändern.

### <a name="how-can-i-reenable-developer-options-after-updating-iosupdate-developer-optionsmd"></a>[Wie kann ich Entwickleroptionen erneut aktivieren, aktualisieren Sie den iOS?](update-developer-options.md)
Ein iOS-Fehler verursachen, dass die Entwickleroptionen nicht mehr angezeigt, nach der Aktualisierung von iOS-Versionen, dies festgestellt wurde beim Wechsel zur iOS 8.x. Dieses Handbuch beschreibt, wie die Optionen erneut aktiviert werden können.

### <a name="user-location-not-working-in-ios-8ios8-user-locationmd"></a>[Standort des Benutzers in iOS 8 nicht funktionsfähig.](ios8-user-location.md)
Dieses Handbuch beschreibt, wie so bearbeiten Sie die Datei "Info.plist", um Benutzerstandort in iOS 8 zu aktivieren.

### <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logssymbolicate-ios-crashmd"></a>[Wo finde ich die .dSYM Datei iOS Absturz Protokolle symbolicate?](symbolicate-ios-crash.md)
Dieses Handbuch beschreibt die grundlegenden Schritte zum symbolicating iOS Absturz Protokolle unterstützen Sie beim Diagnostizieren von abstürzen. Es wird auch auf zusätzliche Ressourcen für erweiterte Techniken Symbolication & Informationen zum Interpretieren von iOS-Crash-Protokolle verknüpft.


### <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studioxs-mono-runtimemd"></a>[Wie richte ich Mono-Runtime-Umgebungsvariablen für iOS-Projekten in Xamarin Studio?](xs-mono-runtime.md)
Wenn Sie Common Language Runtime für Mono Umgebungsvariablen festlegen müssen, können festgelegt werden der **Projektoptionen > ausführen > Allgemein** Seite.

## <a name="publishing-questions"></a>Veröffentlichen Fragen

### <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submissioninvalid-bundle-bitcodemd"></a>[Fehler beim Senden von App-Speicher: "Ungültige Paket - Optionen, die nicht zulässig, die in Bitcode eingebettet werden in die Übermittlung erkannt werden"](invalid-bundle-bitcode.md)

Senden von apps, die _erfordern_ Bitcode, z. B. WatchOS und tvos. außerdem wurden-apps muss mit Xcode 9 ausgeführt werden.

### <a name="can-i-change-the-output-path-of-the-ipa-fileipa-output-pathmd"></a>[Kann ich den Ausgabepfad der Datei IPA-Datei ändern?](ipa-output-path.md)
Zum Zeitpunkt der Xamarin-Zyklus-7 können Sie benutzerdefinierte MSBuild-Ziele verwenden, um dies zu erreichen.

### <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folderipa-tfsmd"></a>[Wie kann ich IPA-Ausgabedateien in den TFS-Ablageordner kopieren?](ipa-tfs.md)
Ja, diese Anleitung wird beschrieben, wie.

### <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studiomodify-ipamd"></a>[Kann ich Dateien hinzufügen oder Entfernen von Dateien aus einer IPA-Datei nach der Erstellung in Visual Studio?](modify-ipa.md)
Ja, es ist möglich, aber in der Regel, dass Sie sich erneut anmelden muss die `.app` Paket nach der Änderung. Beachten Sie, dass das Ändern der `.ipa` Datei ist nicht erforderlich, während normaler Verwendung. In diesem Artikel wird nur zu Informationszwecken bereitgestellt.

### <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studiocreate-xcarchivemd"></a>[Ist es möglich, ein Archiv .xcarchive aus Visual Studio zu erstellen?](create-xcarchive.md)
As of Xamarin 4, ist es jetzt möglich, erstellen eine `.xcarchive` von Windows durch Festlegen der `ArchiveOnBuild` Eigenschaft `true`.

### <a name="why-does-my-app-submission-fail-with-disallowed-paths--itunesmetadataplist--found-at--itunesmetadata-disallowed-pathsmd"></a>[Warum schlägt mein app-Übermittlung fehl mit: "Unzulässiger Pfade ("iTunesMetadata.plist") finden Sie unter..."?](itunesmetadata-disallowed-paths.md)
Dieser Fehler ist das Ergebnis einer Änderung in der Apple App Store-Überprüfung. Dieser spezifische Fehler ist _nicht_ im Zusammenhang mit bestimmten Version von Xamarin, die Sie installiert haben, wird daher Downgrade _nicht_ helfen. Dieses Handbuch enthält Links zu weiteren Informationen zum Beheben des Problems.


## <a name="diagnosing-specific-error-messages"></a>Diagnostizieren von Fehlermeldungen

### <a name="ios-designer-error-with-registerserviceporterror-registerserviceportmd"></a>[iOS-Designer-Fehler mit RegisterServicePort](error-registerserviceport.md)
Fehler mit `RegisterServicePort` und ähnliche Fehlermeldungen wie oben sind häufig ein Problem mit Spyware/Malware auf dem Computer. In dieser Anleitung bestätigt die Diagnose und die Informationen zum Entfernen von Spyware/Malware.

### <a name="why-does-my-ios-build-fail-with-no-valid-iphone-code-signing-keys-found-in-keychainno-codesigning-keysmd"></a>[Warum schlägt mein iOS-Build fehl mit: kein Signaturschlüssel gültig iPhone-Code in einem Schlüssel gefunden?](no-codesigning-keys.md)
Diese Fehlermeldung tritt auf, wenn das betreffende Projekt gültigen Anmeldeinformationen im Code-signing sucht jedoch nicht an diese zu finden. Signieren von Code ist für das Testen und Bereitstellungen auf physischen iOS-Geräten erforderlich. als auch Ad-hoc- & Anwendung Builds zu speichern.

### <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-objectexception-marshal-obj-cmd"></a>[Warum wird meine app für iOS 9 ausgeführt, mit: System.Exception: Fehler beim Objective-C-Objekt Marshallen?](exception-marshal-obj-c.md)
API-Änderungen in iOS 9 erfordern, dass ein Rückruf-Konstruktor verwendet werden, wenn Aufrufen von nicht verwaltetem Code als der zugrunde liegenden API jetzt er davon ausgeht.

### <a name="runtime-error-the-assembly-mscorlibdll-was-not-found-or-could-not-be-loadederror-mscorlib-not-foundmd"></a>[Laufzeitfehler: die Assembly "mscorlib.dll" wurde nicht gefunden oder konnte nicht geladen werden.](error-mscorlib-not-found.md)
Dieses Problem tritt auf, wenn die *ausgeblendete* `.monotouch-32` und `.monotouch-64` Ordner fehlen die `.xcarchive` zum Signieren / IPA erstellen, die Common Language Runtime-Fehler auslösen.

## <a name="deprecated"></a>Als veraltet markiert

> [!IMPORTANT]
> Die folgenden Artikel gelten für Probleme, die in den neuesten Versionen von Xamarin aufgelöst wurden. Allerdings tritt das Problem auf die neueste Version der Software, bitte der Datei eine [neuen Fehler](~/cross-platform/troubleshooting/questions/howto-file-bug.md) mit Ihrer vollständigen versionsverwaltung-Informationen "und" vollständig "Ausgabeprotokoll erstellen.



### <a name="ipa-file-is-0-bytesipa-zero-bytesmd"></a>[IPA-Datei ist 0 Byte](ipa-zero-bytes.md)
Es wurden einige bekannte Probleme in früheren Versionen von Xamarin, die die IPA-Datei unter Windows auf 0 Byte sein könnte.

### <a name="ibtool-error-the-operation-couldnt-be-completederror-ibtoolmd"></a>[IBTool-Fehler: Der Vorgang konnte nicht abgeschlossen werden.](error-ibtool.md)
Apple [fester](https://developer.apple.com/library/ios/releasenotes/DeveloperTools/RN-Xcode/Chapters/xc6_release_notes.html) dies `ibtool` Bug in Xcode 6.1.1, also Upgrade to Xcode 6.1.1 oder höher ist die einfachste Lösung.

### <a name="error-mt1009-could-not-copy-the-assemblyerror-mt1009md"></a>[Fehler MT1009: Die Assembly konnte nicht kopiert werden.](error-mt1009.md)
Dies wirkt sich auf Benutzer, die Xamarin.iOS 7.2.6. Dieses Problem ist aufgrund von Dateiberechtigungen, die erhöhte Rechte erforderlich, wenn Xamarin.iOS, und ein anderes Benutzerkonto installiert wird Hauptkonto des Entwicklers.

### <a name="systemexception-amdevicenotificationsubscribe-returned-exception-amddevicenotificationsubscribemd"></a>[System.Exception AMDeviceNotificationSubscribe zurückgegebene...](exception-amddevicenotificationsubscribe.md)
Diese Meldung kann in ein Fehlerdialogfeld angezeigt, wenn Sie Visual Studio für Mac oder in zuerst starten die `mtbserver.log` Datei. Beachten Sie, dass dies ungewöhnlich, dass ein Problem aufgetreten ist. Wenn Visual Studio Probleme beim Herstellen einer Verbindung mit dem Mac-Build-Host aufgetreten sind, sind andere Fehler, die wahrscheinlich in angezeigt werden die `mtbserver.log` Datei.

### <a name="mdocarchivetomsxdocconverterexe-not-found-rverbasecommandonrequestmdocarchivetomsxdocconverter-not-foundmd"></a>[MDocArchiveToMsxDocConverter.exe Rver nicht gefunden. BaseCommand.OnRequest](mdocarchivetomsxdocconverter-not-found.md)
Dieser Fehler kann auftreten, der `Mac Server Log` in Visual Studio.
