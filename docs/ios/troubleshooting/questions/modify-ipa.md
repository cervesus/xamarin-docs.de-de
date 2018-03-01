---
title: "Kann ich Dateien hinzufügen oder Entfernen von Dateien aus einer IPA-Datei nach der Erstellung in Visual Studio?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6C3082FB-C3F1-4661-BE45-64570E56DE7C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: be792c95de7aa66d64278e47b2ca6b354e611273
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studio"></a>Kann ich Dateien hinzufügen oder Entfernen von Dateien aus einer IPA-Datei nach der Erstellung in Visual Studio?

Ja, es ist möglich, aber in der Regel, dass Sie sich erneut anmelden muss die `.app` Paket nach der Änderung.

Beachten Sie, dass das Ändern der `.ipa` Datei ist nicht erforderlich, während normaler Verwendung. In diesem Artikel wird nur zu Informationszwecken bereitgestellt.

## <a name="example-removing-a-file-from-a-ipa-archive"></a>Beispiel: Entfernen einer Datei aus einem `.ipa` Archiv

In diesem Beispiel wird davon ausgegangen, dass der Name des Projekts Xamarin.iOS `iPhoneApp1` und `generated session id` ist `cc530d20d6b19da63f6f1c6f67a0a254`

1.  Erstellen der `.ipa` Datei aus Visual Studio wie gewohnt.

2.  Wechseln in den Mac Host erstellen.

3.  Suchen Sie den Build in die `~/Library/Caches/Xamarin/mtbs/builds` Ordner. Sie können diesen Pfad in einfügen **Finder > wechseln Sie > wechseln Sie zum Ordner** auf den Ordner im Finder durchsuchen. Suchen Sie nach dem Ordner, der den Namen des Projekts entspricht. In diesem Ordner zu suchen, für den Ordner, der entspricht der `generated session id` des Builds. Dadurch werden wahrscheinlich die Unterordner, der Zeitpunkt der letzten Änderung hat.

4.  Öffnen Sie ein neues `Terminal.app` Fenster.

5.  Typ `cd ` in der Terminal.app-Fenster, und klicken Sie dann Drag & Drop die `generated session id` Ordner in der `Terminal.app` Fenster:

    ![](modify-ipa-images/session-id-folder.png "Suchen den Ordner der generierten Session-Id im Finder")

6.  Geben Sie die EINGABETASTE zum Ändern des Verzeichnisses, in dem `generated session id` Ordner.

7.  Entpacken Sie die `.ipa` -Datei in eine temporäre `old/` Ordner mit dem folgenden Befehl. Anpassen der `Ad-Hoc` und `iPhoneApp1` Namen für Ihr jeweiliges Projekt nach Bedarf.

    > ditto -xk bin/iPhone/Ad-Hoc/iPhoneApp1-1.0.ipa old/

8.  Behalten Sie die `Terminal.app` Fenster geöffnet.

9.  Löschen Sie die gewünschten Dateien aus dem `.ipa`. Sie können auf das Papierkorbsymbol, mit dem Finder verschieben oder löschen Sie sie in der Befehlszeile mithilfe `Terminal.app`. Zum Anzeigen des Inhalts der `Payload/iPhone` Datei im Finder-Steuerelement auf die Datei, und wählen Sie **Paketinhalt anzeigen**.

10.  Mit den gleichen allgemeinen Ansatz wie in Schritt 3, suchen Sie nach der Protokolldatei unter `~/Library/Logs/Xamarin/MonoTouchVS/` , die beide den Namen des Projekts wurde und die `generated session id` im Namen: ![ ] (modify-ipa-images/build-log.png "suchen Sie das Projekt Buildprotokoll im Finder")

11.  Öffnen Sie das Buildprotokoll aus Schritt 10, z. B. indem Sie darauf doppelklicken.

12.  Suchen Sie die Zeile, die enthält `tool /usr/bin/codesign execution started with arguments: -v --force --sign`.

13.  Typ `/usr/bin/codesign ` in das Fenster Terminal.app aus Schritt 8.

14.  Kopieren Sie alle Argumente ab `-v` über die Befehlszeile in Schritt 12, und fügen Sie sie in das Terminal.app ein.

15.  Ändern Sie das letzte Argument ist der `.app` Paket befindet sich innerhalb der `old/Payload/` Ordner, und führen Sie den Befehl.

```bash
/usr/bin/codesign -v --force --sign SOME_LONG_STRING in/iPhone/Ad-Hoc/iPhoneApp1.app/ResourceRules.plist --entitlements obj/iPhone/Ad-Hoc/Entitlements.xcent old/Payload/iPhoneApp1.app
```

16.  Ändern Sie in der `old/` Terminalfenster Verzeichnis:

```bash
cd old
```

17.  Den Inhalt des Verzeichnisses in ein neues ZIP `.ipa` Datei mithilfe der `zip` Befehl. Sie können ändern, die `"$HOME/Desktop/iPhoneApp1-1.0.ipa"` Argument zur Ausgabe der `.ipa` Datei, wo Sie möchten:

```bash
zip -yr "$HOME/Desktop/iPhoneApp1-1.0.ipa" *
```

## <a name="common-error-messages"></a>Fehlermeldungen

Wenn dort `Invalid Signature. A sealed resource is missing or invalid.`, bedeutet, die in der Regel etwas innerhalb geändert wurde die `.app` -Bundle, und dass die `.app` Paket wurde nicht ordnungsgemäß erneut signiert anschließend. Beachten Sie, dass auch, wenn Sie möchten eine `.ipa` mit einem Verteilungsprofil Sie _müssen_ erstellen Sie die ursprüngliche `.ipa` mit einem Verteilungsprofil. Andernfalls die `Entitlements.xcent` nicht korrekt.

So erteilen Sie ein konkrete Beispiel wie dieser Fehler auftreten kann, wenn das Ausführen der folgenden `codesign --verify` Befehl in der Terminal-Fenster nach Schritt 9 wird der Fehler zusammen mit dem die genaue Ursache des Fehlers angezeigt:

```bash
$ codesign -dvvv --no-strict --verify old/Payload/iPhoneApp1.app
old/Payload/iPhoneApp1.app: a sealed resource is missing or invalid
file missing: /Users/macuser/Library/Caches/Xamarin/mtbs/builds/iPhoneApp1/cc530d20d6b19da63f6f1c6f67a0a254/old/Payload/iPhoneApp1.app/MyFile.png
```

Und die App Store-Überprüfung meldet eine ähnliche Fehlermeldung angezeigt:

> Fehler ITMS-90035: "Ungültige Signatur. Eine versiegelte Ressource ist nicht vorhanden oder ungültig. Der binäre Pfad [iPhoneApp1.app/iPhoneApp1] enthält eine ungültige Signatur. Stellen Sie sicher, dass Sie Ihre Anwendung mit einer Verteilung, kein ad-hoc-Zertifikat oder ein Zertifikat Entwicklung angemeldet haben. Stellen Sie sicher, dass die Einstellungen der Code Signaturzertifikat in Xcode korrekt, auf der Zielebene sind (der alle Werte auf Projektebene außer Kraft). Vergewissern Sie sich außerdem, dass das Paket, das Sie hochladen mit einer Release-Ziel in Xcode nicht Simulator-Ziel erstellt wurde. Wenn Sie sicher, dass Ihr Code Signaturzertifikat-Einstellungen richtig sind sind, wählen Sie in Xcode "bereinigen All", löschen Sie das Verzeichnis "Build" im Finder und rebuild-Ziel-Version. Weitere Informationen finden Sie in [https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html](https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html)"
