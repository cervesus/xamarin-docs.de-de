---
title: Kann ich nach der Erstellung in Visual Studio Dateien zu einer IPA-Datei hinzufügen oder aus ihr entfernen?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 6C3082FB-C3F1-4661-BE45-64570E56DE7C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
ms.openlocfilehash: f63cc544b251ca284a8d087e09f9cbc4dfb3f769
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030955"
---
# <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studio"></a>Kann ich nach der Erstellung in Visual Studio Dateien zu einer IPA-Datei hinzufügen oder aus ihr entfernen?

Ja, es ist zwar möglich, aber es ist in der Regel erforderlich, dass Sie das `.app` Bundle neu signieren, nachdem Sie die Änderung vorgenommen haben.

Beachten Sie, dass das Ändern der `.ipa` Datei bei normaler Verwendung nicht erforderlich ist. Dieser Artikel wird ausschließlich zu Informationszwecken bereitgestellt.

## <a name="example-removing-a-file-from-a-ipa-archive"></a>Beispiel: Entfernen einer Datei aus einem `.ipa` Archiv

Bei diesem Beispiel wird davon ausgegangen, dass der Name des xamarin. IOS-Projekts `iPhoneApp1` und die `generated session id` `cc530d20d6b19da63f6f1c6f67a0a254`

1. Erstellen Sie die `.ipa` Datei in Visual Studio wie gewohnt.

2. Wechseln Sie zum Mac-buildhost.

3. Suchen Sie den Build im Ordner "`~/Library/Caches/Xamarin/mtbs/builds`". Sie können diesen Pfad in den **Finder einfügen, > > zum Ordner** wechseln, um den Ordner im Finder zu durchsuchen. Suchen Sie nach dem Ordner, der mit dem Projektnamen übereinstimmt. Suchen Sie in diesem Ordner nach dem Ordner, der mit dem `generated session id` des Builds übereinstimmt. Dabei handelt es sich höchstwahrscheinlich um den Unterordner mit der letzten Änderungszeit.

4. Öffnen Sie ein neues `Terminal.app` Fenster.

5. Geben Sie `cd` in das Fenster Terminal. app ein, und ziehen Sie & ablegen des `generated session id` Ordners in das `Terminal.app` Fenster:

    ![](modify-ipa-images/session-id-folder.png "Locating the generated session id folder in Finder")

6. Geben Sie die Rückgabetaste ein, um das Verzeichnis in den `generated session id` Ordner zu ändern.

7. Entpacken Sie die `.ipa` Datei mithilfe des folgenden Befehls in einen temporären Ordner `old/`. Passen Sie die `Ad-Hoc`-und `iPhoneApp1` Namen nach Bedarf für das jeweilige Projekt an.

    > Ditto-XK bin/iPhone/Ad-hoc/iphoneapp1-1.0. IPA alt/

8. Lassen Sie das `Terminal.app` Fenster geöffnet.

9. Löschen Sie die gewünschten Dateien aus dem `.ipa`. Sie können Sie entweder mithilfe von Finder in den Papierkorb verschieben oder mithilfe von `Terminal.app`in der Befehlszeile löschen. Um den Inhalt der `Payload/iPhone` Datei im Finder anzuzeigen, klicken Sie mit der Maustaste auf die Datei, und wählen Sie **Paket Inhalt anzeigen**aus.

10. Wenn Sie denselben allgemeinen Ansatz wie in Schritt 3 verwenden, finden Sie die Protokolldatei unter `~/Library/Logs/Xamarin/MonoTouchVS/`, die sowohl den Projektnamen als auch den `generated session id` im Namen hat:![](modify-ipa-images/build-log.png "Suchen des projektbuildprotokolls in Finder")

11. Öffnen Sie das Buildprotokoll aus Schritt 10, indem Sie beispielsweise darauf doppelklicken.

12. Suchen Sie die Zeile, die `tool /usr/bin/codesign execution started with arguments: -v --force --sign`enthält.

13. Geben Sie in Schritt 8 `/usr/bin/codesign` in das Terminal. app-Fenster ein.

14. Kopieren Sie alle Argumente, die mit `-v` beginnen, aus der Zeile in Schritt 12, und fügen Sie Sie in das Terminal. app-Fenster ein.

15. Ändern Sie das letzte Argument in das `.app` Bündel, das sich im Ordner `old/Payload/` befindet, und führen Sie dann den Befehl aus.

    ```bash
    /usr/bin/codesign -v --force --sign SOME_LONG_STRING in/iPhone/Ad-Hoc/iPhoneApp1.app/ResourceRules.plist --entitlements obj/iPhone/Ad-Hoc/Entitlements.xcent old/Payload/iPhoneApp1.app
    ```

16. Wechseln Sie im Terminal in das `old/` Verzeichnis:

    ```bash
    cd old
    ```

17. Verwenden Sie den `zip`-Befehl, um den Inhalt des Verzeichnisses in eine neue `.ipa` Datei zu packen. Sie können das `"$HOME/Desktop/iPhoneApp1-1.0.ipa"`-Argument so ändern, dass die `.ipa` Datei an einem beliebigen Ort ausgegeben wird:

    ```bash
    zip -yr "$HOME/Desktop/iPhoneApp1-1.0.ipa" *
    ```

## <a name="common-error-messages"></a>Häufige Fehlermeldungen

Wenn `Invalid Signature. A sealed resource is missing or invalid.`angezeigt wird, bedeutet dies im Allgemeinen, dass etwas innerhalb des `.app` Bündels geändert wurde und dass das `.app` Bundle anschließend nicht ordnungsgemäß neu signiert wurde. Beachten Sie auch Folgendes: Wenn Sie eine `.ipa` mit einem Verteilungs Profil erstellen möchten, _müssen_ Sie den ursprünglichen `.ipa` mit einem Verteilungs Profil erstellen. Andernfalls ist die `Entitlements.xcent` falsch.

Wenn Sie im Terminal Fenster nach Schritt 9 den folgenden `codesign --verify` Befehl im Terminal Fenster ausführen möchten, können Sie einen konkreten Beispiel dafür anzeigen, dass der Fehler zusammen mit der genauen Fehlerursache angezeigt wird:

```bash
$ codesign -dvvv --no-strict --verify old/Payload/iPhoneApp1.app
old/Payload/iPhoneApp1.app: a sealed resource is missing or invalid
file missing: /Users/macuser/Library/Caches/Xamarin/mtbs/builds/iPhoneApp1/cc530d20d6b19da63f6f1c6f67a0a254/old/Payload/iPhoneApp1.app/MyFile.png
```

Und der App Store-Überprüfungsprozess meldet eine ähnliche Fehlermeldung:

> FEHLER-ITMS-90035: "Ungültige Signatur. Eine versiegelte Ressource fehlt oder ist ungültig. Der binäre unter Pfad [iPhoneApp1. app/iPhoneApp1] enthält eine ungültige Signatur. Stellen Sie sicher, dass Sie Ihre Anwendung mit einem Verteilungs Zertifikat signiert haben, nicht mit einem Ad-hoc-Zertifikat oder einem Entwicklungs Zertifikat. Überprüfen Sie, ob die Code Signierungs Einstellungen in Xcode auf der Zielebene korrekt sind (die alle Werte auf Projektebene überschreiben). Stellen Sie außerdem sicher, dass das Paket, das Sie hochladen, mit einem releaseziel in Xcode erstellt wurde, nicht mit einem simulatorziel. Wenn Sie sicher sind, dass die Code Signierungs Einstellungen richtig sind, wählen Sie "alle bereinigen" in Xcode aus, löschen Sie das Verzeichnis "Build" im Finder, und erstellen Sie das releaseziel neu. Weitere Informationen finden Sie [https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html](https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html)"
