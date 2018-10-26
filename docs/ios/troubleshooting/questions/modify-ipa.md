---
title: Kann ich Dateien hinzufügen oder Entfernen von Dateien aus einer IPA-Datei nach der Erstellung in Visual Studio?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 6C3082FB-C3F1-4661-BE45-64570E56DE7C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/03/2018
ms.openlocfilehash: bf135755f64e4d17db2c187d58572c525dfee559
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117407"
---
# <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studio"></a>Kann ich Dateien hinzufügen oder Entfernen von Dateien aus einer IPA-Datei nach der Erstellung in Visual Studio?

Ja, es ist möglich, aber es ist in der Regel erforderlich, wenn Sie sich erneut anmelden die `.app` Paket nach der Änderung.

Beachten Sie, dass das Ändern der `.ipa` Datei ist nicht erforderlich, während normaler Verwendung. In diesem Artikel wird nur zu Informationszwecken bereitgestellt.

## <a name="example-removing-a-file-from-a-ipa-archive"></a>Beispiel: Entfernen einer Datei aus einem `.ipa` Archiv

In diesem Beispiel wird davon ausgegangen, dass der Name des Xamarin.iOS-Projekts `iPhoneApp1` und `generated session id` ist `cc530d20d6b19da63f6f1c6f67a0a254`

1.  Erstellen der `.ipa` Datei wie gewohnt in Visual Studio.

2.  Mit dem Mac-buildhost wechseln.

3.  Suchen Sie den Build in die `~/Library/Caches/Xamarin/mtbs/builds` Ordner. Sie können diesen Pfad in einfügen **Finder > wechseln > wechseln Sie zum Ordner** auf den Ordner im Finder durchsuchen. Suchen Sie nach dem Ordner, der den Namen des Projekts entspricht. In diesem Ordner zu suchen, für den Ordner, der entspricht der `generated session id` des Builds. Dies wird in den meisten Fällen den Unterordner sein, der Zeitpunkt der letzten Änderung aufweist.

4.  Öffnen Sie ein neues `Terminal.app` Fenster.

5.  Typ `cd ` in die Terminal.app-Fenster, und klicken Sie dann Drag & Drop die `generated session id` Ordner in der `Terminal.app` Fenster:

    ![](modify-ipa-images/session-id-folder.png "Suchen die generierte Sitzungs-ID-Ordner in Finder")

6.  Geben Sie die EINGABETASTE zum Ändern des Verzeichnisses, in der `generated session id` Ordner.

7.  Entzippen Sie die `.ipa` -Datei in ein temporäres `old/` Ordner mit dem folgenden Befehl. Anpassen der `Ad-Hoc` und `iPhoneApp1` Namen wie für Ihr spezielles Projekt erforderlich.

    > ditto -xk bin/iPhone/Ad-Hoc/iPhoneApp1-1.0.ipa old/

8.  Behalten Sie die `Terminal.app` Fenster geöffnet.

9.  Löschen Sie die gewünschten Dateien aus dem `.ipa`. Sie können entweder in den Papierkorb, die mit dem Finder verschieben oder löschen Sie sie in der Befehlszeile mit `Terminal.app`. Zum Anzeigen des Inhalts der `Payload/iPhone` im Finder zeigen, Strg + Klick die Datei und wählen Sie **Paketinhalt anzeigen**.

10.  Mit den gleichen allgemeinen Ansatz, wie in Schritt 3, suchen Sie nach der Protokolldatei unter `~/Library/Logs/Xamarin/MonoTouchVS/` , die beide den Namen des Projekts ist und die `generated session id` im Namen: ![](modify-ipa-images/build-log.png "finden Sie im Finder zeigen das Buildprotokoll für Projekt")

11.  Öffnen Sie das Buildprotokoll aus Schritt 10, z. B. indem Sie darauf doppelklicken.

12.  Suchen Sie die Zeile, die enthält `tool /usr/bin/codesign execution started with arguments: -v --force --sign`.

13.  Typ `/usr/bin/codesign ` in das Fenster Terminal.app aus Schritt 8.

14.  Kopieren Sie alle Argumente ab `-v` über die Befehlszeile in Schritt 12, und fügen Sie sie in das Fenster Terminal.app.

15.  Ändern Sie das letzte Argument werden der `.app` Paket befindet sich innerhalb der `old/Payload/` Ordner, und führen Sie den Befehl.

```bash
/usr/bin/codesign -v --force --sign SOME_LONG_STRING in/iPhone/Ad-Hoc/iPhoneApp1.app/ResourceRules.plist --entitlements obj/iPhone/Ad-Hoc/Entitlements.xcent old/Payload/iPhoneApp1.app
```

16.  Wechseln Sie in der `old/` Verzeichnis im Terminal:

```bash
cd old
```

17.  Zippen Sie den Inhalt des Verzeichnisses in ein neues `.ipa` Datei mithilfe der `zip` Befehl. Sie können ändern, die `"$HOME/Desktop/iPhoneApp1-1.0.ipa"` Argument für die Ausgabe der `.ipa` Datei, wo Sie möchten:

```bash
zip -yr "$HOME/Desktop/iPhoneApp1-1.0.ipa" *
```

## <a name="common-error-messages"></a>Häufige Fehlermeldungen

Wenn dort `Invalid Signature. A sealed resource is missing or invalid.`, im Allgemeinen also etwas in geändert wurde der `.app` Paket, und dass die `.app` Paket wurde nicht korrekt neu signiert anschließend. Beachten Sie, dass wenn Sie möchten ein `.ipa` mit eines verteilungsprofils Sie _muss_ erstellen Sie die ursprüngliche `.ipa` mit einem Verteilungsprofil. Andernfalls die `Entitlements.xcent` wird "falsch" sein.

Ein konkretes Beispiel wie dieser Fehler auftreten kann, gewähren, wenn Sie Folgendes ausführen, `codesign --verify` Befehl im Terminal-Fenster nach dem Schritt 9, wird Ihnen den Fehler zusammen mit die genaue Ursache des Fehlers:

```bash
$ codesign -dvvv --no-strict --verify old/Payload/iPhoneApp1.app
old/Payload/iPhoneApp1.app: a sealed resource is missing or invalid
file missing: /Users/macuser/Library/Caches/Xamarin/mtbs/builds/iPhoneApp1/cc530d20d6b19da63f6f1c6f67a0a254/old/Payload/iPhoneApp1.app/MyFile.png
```

Und der App Store-Überprüfungsprozess meldet eine ähnliche Fehlermeldung angezeigt:

> Fehler ITMS-90035: "Ungültige Signatur. Eine versiegelte Ressource ist nicht vorhanden oder ungültig. Die Binärdatei im Pfad [iPhoneApp1.app/iPhoneApp1] enthält eine ungültige Signatur. Stellen Sie sicher, dass Sie Ihre Anwendung ein verteilungszertifikat, nicht auf ein ad-hoc-Zertifikat oder ein Entwicklungszertifikat angemeldet haben. Stellen Sie sicher, dass die Einstellungen für die Codesignatur in Xcode auf der Zielebene (die alle Werte auf Projektebene außer Kraft setzen) korrekt sind. Darüber hinaus stellen Sie sicher, dass das Paket, das Sie hochladen erstellt wurde, verwenden ein Ziel für die Veröffentlichung in Xcode kein simulatorziel. Wenn Sie sicher, dass Ihre Einstellungen für die Codesignatur korrekt sind sind, wählen Sie "Clean All" in Xcode, löschen Sie das Verzeichnis "Build" im Finder und erstellen Sie Ihr Ziel Version neu. Weitere Informationen finden Sie in [ https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html ](https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html)"
