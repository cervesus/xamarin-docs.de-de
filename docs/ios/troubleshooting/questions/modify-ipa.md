---
title: Kann ich nach der Erstellung in Visual Studio Dateien zu einer IPA-Datei hinzufügen oder aus ihr entfernen?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 6C3082FB-C3F1-4661-BE45-64570E56DE7C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
ms.openlocfilehash: e3b535b8624f03e9c832f1719237409cb9ff26c2
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86939905"
---
# <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studio"></a>Kann ich nach der Erstellung in Visual Studio Dateien zu einer IPA-Datei hinzufügen oder aus ihr entfernen?

Ja, es ist möglich, aber es ist in der Regel erforderlich, dass Sie das `.app` Paket nach der Änderung erneut signieren.

Beachten Sie, dass das Ändern der `.ipa` Datei bei normaler Verwendung nicht erforderlich ist. Dieser Artikel wird ausschließlich zu Informationszwecken bereitgestellt.

## <a name="example-removing-a-file-from-a-ipa-archive"></a>Beispiel: Entfernen einer Datei aus einem `.ipa` Archiv

Bei diesem Beispiel wird angenommen, dass der Name des xamarin. IOS-Projekts ist, `iPhoneApp1` und `generated session id` ist.`cc530d20d6b19da63f6f1c6f67a0a254`

1. Erstellen Sie die `.ipa` Datei in Visual Studio wie gewohnt.

2. Wechseln Sie zum Mac-buildhost.

3. Suchen Sie den Build im `~/Library/Caches/Xamarin/mtbs/builds` Ordner. Sie können diesen Pfad in den **Finder einfügen, > > zum Ordner** wechseln, um den Ordner im Finder zu durchsuchen. Suchen Sie nach dem Ordner, der mit dem Projektnamen übereinstimmt. Suchen Sie in diesem Ordner nach dem Ordner, der `generated session id` mit dem des Builds übereinstimmt. Dabei handelt es sich höchstwahrscheinlich um den Unterordner mit der letzten Änderungszeit.

4. Öffnen Sie ein neues `Terminal.app` Fenster.

5. Geben Sie `cd` im Terminal. app-Fenster ein, und ziehen Sie dann & ablegen des `generated session id` Ordners in das `Terminal.app` Fenster:

    ![Suchen des generierten Sitzungs-ID-Ordners im Finder](modify-ipa-images/session-id-folder.png)

6. Geben Sie die Rückgabetaste ein, um das Verzeichnis in den Ordner zu ändern `generated session id` .

7. Entzippen `.ipa` Sie die Datei `old/` mithilfe des folgenden Befehls in einen temporären Ordner. Passen `Ad-Hoc` Sie die `iPhoneApp1` Namen und für ihr bestimmtes Projekt nach Bedarf an.

    > Ditto-XK bin/iPhone/Ad-hoc/iphoneapp1-1.0. IPA alt/

8. Lassen Sie das `Terminal.app` Fenster geöffnet.

9. Löschen Sie die gewünschten Dateien aus `.ipa` . Sie können Sie entweder mithilfe von Finder in den Papierkorb verschieben oder mithilfe von in der Befehlszeile löschen `Terminal.app` . Um den Inhalt der `Payload/iPhone` Datei im Finder anzuzeigen, klicken Sie mit der Maustaste auf die Datei, und wählen Sie **Paket Inhalt anzeigen**aus.

10. Wenn Sie denselben allgemeinen Ansatz wie in Schritt 3 verwenden, finden Sie die Protokolldatei unter `~/Library/Logs/Xamarin/MonoTouchVS/` , die sowohl den Projektnamen als auch den `generated session id` im Namen hat: ![ Suchen Sie das projektbuildprotokoll in Finder.](modify-ipa-images/build-log.png)

11. Öffnen Sie das Buildprotokoll aus Schritt 10, indem Sie beispielsweise darauf doppelklicken.

12. Suchen Sie die Zeile, die enthält `tool /usr/bin/codesign execution started with arguments: -v --force --sign` .

13. Geben Sie `/usr/bin/codesign` aus Schritt 8 in das Fenster Terminal. app ein.

14. Kopieren Sie alle Argumente, die mit beginnen `-v` , in der Zeile in Schritt 12, und fügen Sie Sie in das Terminal. app-Fenster ein.

15. Ändern Sie das letzte Argument in das `.app` Paket, das sich im `old/Payload/` Ordner befindet, und führen Sie dann den Befehl aus.

    ```bash
    /usr/bin/codesign -v --force --sign SOME_LONG_STRING in/iPhone/Ad-Hoc/iPhoneApp1.app/ResourceRules.plist --entitlements obj/iPhone/Ad-Hoc/Entitlements.xcent old/Payload/iPhoneApp1.app
    ```

16. Wechseln Sie `old/` im Terminal in das Verzeichnis:

    ```bash
    cd old
    ```

17. Zippen Sie den Inhalt des Verzeichnisses `.ipa` mithilfe des Befehls in eine neue Datei `zip` . Sie können das `"$HOME/Desktop/iPhoneApp1-1.0.ipa"` Argument so ändern, dass die Datei an einem beliebigen Ort ausgegeben wird `.ipa` :

    ```bash
    zip -yr "$HOME/Desktop/iPhoneApp1-1.0.ipa" *
    ```

## <a name="common-error-messages"></a>Häufige Fehlermeldungen

Wenn Sie sehen `Invalid Signature. A sealed resource is missing or invalid.` , bedeutet dies im Allgemeinen, dass etwas innerhalb des `.app` Bündels geändert wurde und dass das `.app` Paket anschließend nicht ordnungsgemäß neu signiert wurde. Beachten Sie auch Folgendes: Wenn Sie einen `.ipa` mit einem Verteilungs Profil erstellen möchten, _müssen_ Sie den ursprünglichen `.ipa` mit einem Verteilungs Profil erstellen. Andernfalls ist `Entitlements.xcent` falsch.

Wenn Sie den folgenden Befehl im Terminal Fenster nach Schritt 9 ausführen, wird ein konkretes Beispiel dafür angezeigt, wie dieser Fehler auftreten kann. Wenn Sie den folgenden `codesign --verify` Befehl im Terminal Fenster nach Schritt 9 ausführen, wird der Fehler zusammen mit der genauen Fehlerursache angezeigt:

```bash
$ codesign -dvvv --no-strict --verify old/Payload/iPhoneApp1.app
old/Payload/iPhoneApp1.app: a sealed resource is missing or invalid
file missing: /Users/macuser/Library/Caches/Xamarin/mtbs/builds/iPhoneApp1/cc530d20d6b19da63f6f1c6f67a0a254/old/Payload/iPhoneApp1.app/MyFile.png
```

Und der App Store-Überprüfungsprozess meldet eine ähnliche Fehlermeldung:

> Fehler iTMS-90035: "Ungültige Signatur. Eine versiegelte Ressource fehlt oder ist ungültig. Der binäre unter Pfad [iPhoneApp1. app/iPhoneApp1] enthält eine ungültige Signatur. Stellen Sie sicher, dass Sie Ihre Anwendung mit einem Verteilungs Zertifikat signiert haben, nicht mit einem Ad-hoc-Zertifikat oder einem Entwicklungs Zertifikat. Überprüfen Sie, ob die Code Signierungs Einstellungen in Xcode auf der Zielebene korrekt sind (die alle Werte auf Projektebene überschreiben). Stellen Sie außerdem sicher, dass das Paket, das Sie hochladen, mit einem releaseziel in Xcode erstellt wurde, nicht mit einem simulatorziel. Wenn Sie sicher sind, dass die Code Signierungs Einstellungen richtig sind, wählen Sie "alle bereinigen" in Xcode aus, löschen Sie das Verzeichnis "Build" im Finder, und erstellen Sie das releaseziel neu. Weitere Informationen finden Sie unter [https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html](https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html) "
