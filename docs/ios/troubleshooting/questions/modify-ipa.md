---
title: Kann ich nach der Erstellung in Visual Studio Dateien zu einer IPA-Datei hinzufügen oder aus ihr entfernen?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 6C3082FB-C3F1-4661-BE45-64570E56DE7C
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 04/03/2018
ms.openlocfilehash: d1546b83304d8c66f7433bd77c5ebecc9dc95aaa
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70278471"
---
# <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studio"></a>Kann ich nach der Erstellung in Visual Studio Dateien zu einer IPA-Datei hinzufügen oder aus ihr entfernen?

Ja, es ist möglich, aber es ist in der Regel erforderlich, dass Sie `.app` das Paket nach der Änderung erneut signieren.

Beachten Sie, dass `.ipa` das Ändern der Datei bei normaler Verwendung nicht erforderlich ist. Dieser Artikel wird ausschließlich zu Informationszwecken bereitgestellt.

## <a name="example-removing-a-file-from-a-ipa-archive"></a>Beispiel: Entfernen einer Datei aus einem `.ipa` Archiv

Bei diesem Beispiel wird angenommen, dass der Name des xamarin. IOS `iPhoneApp1` `generated session id` -Projekts ist, und ist.`cc530d20d6b19da63f6f1c6f67a0a254`

1. Erstellen Sie `.ipa` die Datei in Visual Studio wie gewohnt.

2. Wechseln Sie zum Mac-buildhost.

3. Suchen Sie den Build im `~/Library/Caches/Xamarin/mtbs/builds` Ordner. Sie können diesen Pfad in den **Finder einfügen, > > zum Ordner** wechseln, um den Ordner im Finder zu durchsuchen. Suchen Sie nach dem Ordner, der mit dem Projektnamen übereinstimmt. Suchen Sie in diesem Ordner nach dem Ordner, der mit `generated session id` dem des Builds übereinstimmt. Dabei handelt es sich höchstwahrscheinlich um den Unterordner mit der letzten Änderungszeit.

4. Öffnen Sie ein `Terminal.app` neues Fenster.

5. Geben `cd` Sie im Terminal. app-Fenster ein, und ziehen Sie dann `generated session id` & ablegen des `Terminal.app` Ordners in das Fenster:

    ![](modify-ipa-images/session-id-folder.png "Suchen des generierten Sitzungs-ID-Ordners im Finder")

6. Geben Sie die Rückgabetaste ein, um das `generated session id` Verzeichnis in den Ordner zu ändern.

7. Entzippen `.ipa` Sie die Datei mithilfe `old/` des folgenden Befehls in einen temporären Ordner. Passen Sie `Ad-Hoc` die `iPhoneApp1` Namen und für ihr bestimmtes Projekt nach Bedarf an.

    > ditto -xk bin/iPhone/Ad-Hoc/iPhoneApp1-1.0.ipa old/

8. Lassen Sie `Terminal.app` das Fenster geöffnet.

9. Löschen Sie die gewünschten Dateien aus `.ipa`. Sie können Sie entweder mithilfe von Finder in den Papierkorb verschieben oder mithilfe `Terminal.app`von in der Befehlszeile löschen. Um den Inhalt `Payload/iPhone` der Datei im Finder anzuzeigen, klicken Sie mit der Maustaste auf die Datei, und wählen Sie **Paket Inhalt anzeigen**aus.

10. Wenn Sie denselben allgemeinen Ansatz wie in Schritt 3 verwenden, finden Sie die Protokoll `~/Library/Logs/Xamarin/MonoTouchVS/` Datei unter, die sowohl den Projektnamen `generated session id` als auch den im Namen enthält: ![](modify-ipa-images/build-log.png "Suchen des projektbuildprotokolls in Finder")

11. Öffnen Sie das Buildprotokoll aus Schritt 10, indem Sie beispielsweise darauf doppelklicken.

12. Suchen Sie die Zeile, `tool /usr/bin/codesign execution started with arguments: -v --force --sign`die enthält.

13. Geben `/usr/bin/codesign` Sie aus Schritt 8 in das Fenster Terminal. app ein.

14. Kopieren Sie alle Argumente, die mit `-v` beginnen, in der Zeile in Schritt 12, und fügen Sie Sie in das Terminal. app-Fenster ein.

15. Ändern Sie das letzte Argument in das `.app` Paket, das sich `old/Payload/` im Ordner befindet, und führen Sie dann den Befehl aus.

    ```bash
    /usr/bin/codesign -v --force --sign SOME_LONG_STRING in/iPhone/Ad-Hoc/iPhoneApp1.app/ResourceRules.plist --entitlements obj/iPhone/Ad-Hoc/Entitlements.xcent old/Payload/iPhoneApp1.app
    ```

16. Wechseln Sie im `old/` Terminal in das Verzeichnis:

    ```bash
    cd old
    ```

17. Zippen Sie den Inhalt des Verzeichnisses mithilfe des `.ipa` `zip` Befehls in eine neue Datei. Sie können das `"$HOME/Desktop/iPhoneApp1-1.0.ipa"` Argument so ändern, dass `.ipa` die Datei an einem beliebigen Ort ausgegeben wird:

    ```bash
    zip -yr "$HOME/Desktop/iPhoneApp1-1.0.ipa" *
    ```

## <a name="common-error-messages"></a>Häufige Fehlermeldungen

Wenn Sie sehen `Invalid Signature. A sealed resource is missing or invalid.`, bedeutet dies im Allgemeinen, dass etwas innerhalb des `.app` Bündels geändert wurde und `.app` dass das Paket anschließend nicht ordnungsgemäß neu signiert wurde. Beachten Sie auch Folgendes: Wenn Sie einen `.ipa` mit einem Verteilungs Profil erstellen möchten, _müssen_ Sie den ursprünglichen `.ipa` mit einem Verteilungs Profil erstellen. `Entitlements.xcent` Andernfalls ist falsch.

Wenn Sie den folgenden Befehl im Terminal Fenster nach Schritt 9 ausführen, wird ein konkretes Beispiel `codesign --verify` dafür angezeigt, wie dieser Fehler auftreten kann. Wenn Sie den folgenden Befehl im Terminal Fenster nach Schritt 9 ausführen, wird der Fehler zusammen mit der genauen Fehlerursache angezeigt:

```bash
$ codesign -dvvv --no-strict --verify old/Payload/iPhoneApp1.app
old/Payload/iPhoneApp1.app: a sealed resource is missing or invalid
file missing: /Users/macuser/Library/Caches/Xamarin/mtbs/builds/iPhoneApp1/cc530d20d6b19da63f6f1c6f67a0a254/old/Payload/iPhoneApp1.app/MyFile.png
```

Und der App Store-Überprüfungsprozess meldet eine ähnliche Fehlermeldung:

> FEHLER-ITMS-90035: "Ungültige Signatur. Eine versiegelte Ressource fehlt oder ist ungültig. Der binäre unter Pfad [iPhoneApp1. app/iPhoneApp1] enthält eine ungültige Signatur. Stellen Sie sicher, dass Sie Ihre Anwendung mit einem Verteilungs Zertifikat signiert haben, nicht mit einem Ad-hoc-Zertifikat oder einem Entwicklungs Zertifikat. Überprüfen Sie, ob die Code Signierungs Einstellungen in Xcode auf der Zielebene korrekt sind (die alle Werte auf Projektebene überschreiben). Stellen Sie außerdem sicher, dass das Paket, das Sie hochladen, mit einem releaseziel in Xcode erstellt wurde, nicht mit einem simulatorziel. Wenn Sie sicher sind, dass die Code Signierungs Einstellungen richtig sind, wählen Sie "alle bereinigen" in Xcode aus, löschen Sie das Verzeichnis "Build" im Finder, und erstellen Sie das releaseziel neu. Weitere Informationen finden Sie [https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html](https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html)unter "
