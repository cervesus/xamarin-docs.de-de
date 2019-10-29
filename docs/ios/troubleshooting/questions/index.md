---
title: Häufig gestellte Fragen zu xamarin. IOS
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 65E04188-185D-493D-BA3C-A89711CB6CAF
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: e79fca8c59ae49d27cd335106ca57945be106031
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031085"
---
# <a name="ios-frequently-asked-questions"></a>häufig gestellte Fragen zu IOS

## <a name="general-questions"></a>Allgemeine Fragen

### <a name="can-i-use-a-mac-vm-with-xamarinmac-vmmd"></a>[Kann ich eine Mac-VM mit Xamarin verwenden?](mac-vm.md)
Ja, aber nur auf Mac-Hardware.

### <a name="how-can-i-downgrade-xcodedowngrade-xcodemd"></a>[Wie kann ich Xcode herabstufen?](downgrade-xcode.md)
Dieses Handbuch enthält Links für den Zugriff auf frühere Versionen von Xcode und die neueste Version.

### <a name="where-can-i-set-my-ios-sdk-locationsios-sdkmd"></a>[Wo kann ich meine iOS SDK-Speicherorte festlegen?](ios-sdk.md)
Bei den meisten Benutzern werden diese automatisch auf die richtigen Speicherorte festgelegt. In diesem Handbuch werden die standardmäßigen SDK-Speicherorte und deren Änderung bei Bedarf aufgelistet.

### <a name="how-can-i-reenable-developer-options-after-updating-iosupdate-developer-optionsmd"></a>[Wie kann ich die Entwickler Optionen nach dem Aktualisieren von IOS erneut aktivieren?](update-developer-options.md)
Ein IOS-Fehler kann dazu führen, dass die Entwickler Optionen nach dem Aktualisieren der IOS-Versionen nicht mehr angezeigt werden. Dies wurde beim Wechsel zu IOS 8. x beobachtet. In diesem Leitfaden wird beschrieben, wie die Optionen erneut aktiviert werden können.

### <a name="user-location-not-working-in-ios-8ios8-user-locationmd"></a>[Standort des Benutzers funktioniert in iOS 8 nicht](ios8-user-location.md)
In dieser Anleitung erfahren Sie, wie Sie "Info. plist" Bearbeiten, um den Benutzer Standort in ios 8 zu aktivieren.

### <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logssymbolicate-ios-crashmd"></a>[Wo finde ich die DSYM-Datei zum Ersetzen der iOS-Absturzprotokolle durch Symbole?](symbolicate-ios-crash.md)
In dieser Anleitung werden die grundlegenden Schritte zum symbolisieren von IOS-Absturz Protokollen beschrieben, die bei der Diagnose von Abstürzen helfen. Außerdem gibt es Links zu weiteren Ressourcen für erweiterte Methoden zur symbolischen Vorgehensweisen & Informationen zum Interpretieren von IOS-Absturz Protokollen.

### <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studioxs-mono-runtimemd"></a>[Wie richte ich Mono-Runtime-Umgebungsvariablen für iOS-Projekte in Xamarin Studio ein?](xs-mono-runtime.md)
Wenn Sie Lauf Zeit Umgebungsvariablen für Mono festlegen müssen, können Sie auf den **Projektoptionen > >** Seite "Allgemein" ausführen festgelegt werden.

## <a name="publishing-questions"></a>Veröffentlichen von Fragen

### <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submissioninvalid-bundle-bitcodemd"></a>[Fehler bei der Übermittlung an den App Store: "Ungültige Bundle-Optionen, die nicht in Bitcode eingebettet werden dürfen, werden in der Übermittlung erkannt"](invalid-bundle-bitcode.md)

Das Übermitteln von apps, die Bitcode _erfordern_ (z. b. watchos-und tvos-Apps), muss mit Xcode 9 ausgeführt werden.

### <a name="can-i-change-the-output-path-of-the-ipa-fileipa-output-pathmd"></a>[Kann ich den Ausgabepfad der IPA-Datei ändern?](ipa-output-path.md)
Ab xamarin Cycle 7 können Sie hierfür angepasste MSBuild-Ziele verwenden.

### <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folderipa-tfsmd"></a>[Wie kann ich IPA-Ausgabedateien in den TFS-Ablage Ordner kopieren?](ipa-tfs.md)
Ja, in diesem Handbuch wird beschrieben, wie.

### <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studiomodify-ipamd"></a>[Kann ich nach der Erstellung in Visual Studio Dateien zu einer IPA-Datei hinzufügen oder aus ihr entfernen?](modify-ipa.md)
Ja, es ist zwar möglich, aber es ist in der Regel erforderlich, dass Sie das `.app` Bundle neu signieren, nachdem Sie die Änderung vorgenommen haben. Beachten Sie, dass das Ändern der `.ipa` Datei bei normaler Verwendung nicht erforderlich ist. Dieser Artikel wird ausschließlich zu Informationszwecken bereitgestellt.

### <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studiocreate-xcarchivemd"></a>[Ist es möglich, ein. xcarchive-Archiv aus Visual Studio zu erstellen?](create-xcarchive.md)
Ab xamarin 4 ist es nun möglich, eine `.xcarchive` aus Windows zu erstellen, indem die `ArchiveOnBuild`-Eigenschaft auf `true`festgelegt wird.

### <a name="why-does-my-app-submission-fail-with-disallowed-paths--itunesmetadataplist--found-at--itunesmetadata-disallowed-pathsmd"></a>[Warum schlägt meine App-Übermittlung wie folgt fehl: "Disallowed paths ( "iTunesMetadata.plist" ) found at ..." (Nicht zulässige Pfade ( "iTunesMetadata.plist" ) unter...)](itunesmetadata-disallowed-paths.md)
Dieser Fehler ist das Ergebnis einer Änderung im App Store-Überprüfungsprozess von Apple. Dieser spezifische Fehler ist _nicht_ mit der bestimmten Version von xamarin verknüpft, die Sie installiert haben, sodass das Downgrade _nicht_ unterstützt wird. In dieser Anleitung finden Sie Links zu weiteren Informationen zum Beheben des Problems.

## <a name="diagnosing-specific-error-messages"></a>Diagnostizieren bestimmter Fehlermeldungen

### <a name="ios-designer-error-with-registerserviceporterror-registerserviceportmd"></a>[iOS-Designer-Fehler mit RegisterServicePort](error-registerserviceport.md)
Fehler mit `RegisterServicePort` und ähnlichen Fehlermeldungen wie oben sind häufig ein Problem mit Spyware/Schadsoftware auf dem Computer. Dieses Handbuch enthält ausführliche Informationen zum Bestätigen der Diagnose und Informationen zum Entfernen von Spyware und Schadsoftware.

### <a name="why-does-my-ios-build-fail-with-no-valid-iphone-code-signing-keys-found-in-keychainno-codesigning-keysmd"></a>[Warum schlägt mein iOS-Build fehl mit: Keine gültigen iPhone-Codesignaturschlüssel in Keychain gefunden?](no-codesigning-keys.md)
Diese Fehlermeldung tritt auf, wenn das betreffende Projekt nach gültigen Code Signatur Anmelde Informationen sucht, diese jedoch nicht finden kann. Code Signierung ist für Tests und bereit Stellungen auf physischen IOS-Geräten erforderlich. ebenso wie Ad-hoc-& App Store-Builds.

### <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-objectexception-marshal-obj-cmd"></a>[Warum schlägt meine iOS 9-App wie folgt fehl: System.Exception: Failed to marshal the Objective-C object (Das Objective-C-Objekt konnte nicht gemarshallt werden.)](exception-marshal-obj-c.md)
API-Änderungen in ios 9 erfordern, dass ein Rückruf Konstruktor verwendet wird, wenn nicht verwalteter Code aufgerufen wird, da die zugrunde liegende API dies nun erwartet.

### <a name="runtime-error-the-assembly-mscorlibdll-was-not-found-or-could-not-be-loadederror-mscorlib-not-foundmd"></a>[Laufzeitfehler: The assembly mscorlib.dll was not found or could not be loaded (Die Assembly „mscorlib.dll“ wurde nicht gefunden oder konnte nicht geladen werden).](error-mscorlib-not-found.md)
Dieses Problem tritt auf, wenn die ausgeblendeten `.monotouch-32` und `.monotouch-64` Ordner in der `.xcarchive` *für die* Signierung/IPA-Erstellung fehlen und der Laufzeitfehler ausgelöst wird.

### <a name="compile-error-can-not-encode-offset-x-in-resulting-scattered-relocationerror-encode-offset-scattered-relocationmd"></a>[Kompilierungsfehler: Offset X kann in der resultierenden verstreuten Verschiebung nicht codiert werden](error-encode-offset-scattered-relocation.md)
Dieses Problem tritt auf, wenn für 32-Bit-Architekturen, wie z. b. ARMv7, eine zu hohe Binärdatei für die systemeigene Toolkette entsteht.

## <a name="deprecated"></a>Als veraltet markiert

> [!IMPORTANT]
> Die folgenden Artikel gelten für Probleme, die in neueren Versionen von xamarin behoben wurden. Wenn das Problem jedoch in der aktuellen Version der Software auftritt, melden Sie einen [neuen Fehler](~/cross-platform/troubleshooting/questions/howto-file-bug.md) mit den vollständigen Versionsinformationen und der vollständigen buildprotokolleausgabe.

### <a name="ipa-file-is-0-bytesipa-zero-bytesmd"></a>[IPA-Datei hat 0 Byte](ipa-zero-bytes.md)
In früheren Versionen von xamarin gab es einige bekannte Probleme, die dazu führen konnten, dass die IPA-Datei unter Windows 0 Bytes groß war.

### <a name="ibtool-error-the-operation-couldnt-be-completederror-ibtoolmd"></a>[IBTool-Fehler: Der Vorgang konnte nicht abgeschlossen werden.](error-ibtool.md)
Apple hat diesen `ibtool`-Fehler in Xcode 6.1.1 [behoben](https://developer.apple.com/library/ios/releasenotes/DeveloperTools/RN-Xcode/Chapters/xc6_release_notes.html), sodass ein Upgrade auf Xcode 6.1.1 oder höher die einfachste Möglichkeit zur Fehlerbehebung darstellt.

### <a name="error-mt1009-could-not-copy-the-assemblyerror-mt1009md"></a>[Fehler MT1009: Die Assembly konnte nicht kopiert werden.](error-mt1009.md)
Dies wirkt sich auf Benutzer aus, die xamarin. IOS 7.2.6 ausführen. Dieses Problem ist darauf zurückzuführen, dass Dateiberechtigungen höhere Berechtigungen erfordern, wenn xamarin. IOS mit einem anderen Benutzerkonto als das Hauptkonto des Entwicklers installiert wird.

### <a name="systemexception-amdevicenotificationsubscribe-returned-exception-amddevicenotificationsubscribemd"></a>[System.Exception AMDeviceNotificationSubscribe zurückgegeben ...](exception-amddevicenotificationsubscribe.md)
Diese Meldung kann in einem Fehler Dialogfeld angezeigt werden, wenn Sie zum ersten Mal Visual Studio für Mac oder in der `mtbserver.log` Datei starten. Beachten Sie, dass dies ein ungewöhnliches Problem ist. Wenn Visual Studio Probleme beim Herstellen einer Verbindung mit dem Mac-buildhost hat, gibt es weitere Fehler, die in der `mtbserver.log`-Datei wahrscheinlicher erscheinen.

### <a name="mdocarchivetomsxdocconverterexe-not-found-rverbasecommandonrequestmdocarchivetomsxdocconverter-not-foundmd"></a>[„MDocArchiveToMsxDocConverter.exe“ nicht gefunden. rver.BaseCommand.OnRequest](mdocarchivetomsxdocconverter-not-found.md)
Dieser Fehler wird möglicherweise im `Mac Server Log` in Visual Studio angezeigt.
