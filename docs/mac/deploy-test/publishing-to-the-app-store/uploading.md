---
title: Upload in den Mac App Store
description: "Dieser Leitfaden enthält Informationen zum Hochladen einer Xamarin.Mac-App für die Veröffentlichung im Mac App Store."
ms.topic: article
ms.prod: xamarin
ms.assetid: 30cd0e47-1b2e-47ef-93f6-4bed20b15c03
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 30224db257af4de8dd13f362761ecd369b682dda
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="upload-to-mac-app-store"></a>Upload in den Mac App Store

_Dieser Leitfaden enthält Informationen zum Hochladen einer Xamarin.Mac-App für die Veröffentlichung im Mac App Store._

Anwendungen werden über [iTunes Connect](http://itunesconnect.apple.com/) zur Genehmigung im Mac App Store veröffentlicht.

1. Wählen Sie ein **macOS-App** aus, die Sie erstellen möchten: 

    [![](uploading-images/image65.png "iTunes Connect")](uploading-images/image65.png#lightbox)

2. Geben Sie den Namen der App und weitere Details ein. Der Entwickler kann sich nur für eine vorhandene **Bundle-ID** entscheiden, die bereits erstellt wurde: 

    [![](uploading-images/image66.png "Auswählen der Bundle-ID")](uploading-images/image66.png#lightbox)

3. Wählen Sie das Verfügbarkeitsdatum und den Preis aus. Unabhängig vom vom Entwickler gewählten Verfügbarkeitsdatum wird die App erst dann zum Verkauf freigegeben, nachdem Sie genehmigt wurde. Dieser Wert kann weit in der Zukunft liegen, wenn der Entwickler mehr Kontrolle über das tatsächliche Verfügbarkeitsdatum haben möchte: 

    [![](uploading-images/image67.png "Festlegen des Verfügbarkeitsdatums und des Preises")](uploading-images/image67.png#lightbox)

4. Geben Sie die App-Informationen ein, einschließlich der entsprechenden Kategorie im App Store: 

    [![](uploading-images/image68.png "Eingabe der App-Information")](uploading-images/image68.png#lightbox) 

    Wählen Sie die entsprechenden Bewertungen aus: 

    [![](uploading-images/image69.png "Festlegen der App-Bewertungen")](uploading-images/image69.png#lightbox) 

    Beschreibung, Schlüsselwörter und Kontakt-URLs: 

    [![](uploading-images/image70.png "Bearbeiten der Beschreibung, Schlüsselwörter und Kontakt-URLs")](uploading-images/image70.png#lightbox) 

    Kontaktinformation und Empfehlungen für App Store-Prüfer: 

    [![](uploading-images/image71.png "Bearbeiten der Kontaktinformation und Empfehlungen für App Store-Prüfer")](uploading-images/image71.png#lightbox) 

    Und als letztes Screenshots: 

    [![](uploading-images/image72.png "Hinzufügen der erforderlichen Screenshots")](uploading-images/image72.png#lightbox) 

    Screenshots sollten im Format JPG, TIF oder PNG sein und 1280x800, 1440x900, 2880x1800 oder 2560x1600 Pixel aufweisen. Drücken Sie auf **Speichern**, um den Vorgang zu beenden.

5. Die App-Informationen werden zur Überprüfung angezeigt. Klicken Sie auf **Details anzeigen**, um den Status zu ändern: 

    [![](uploading-images/image73.png "Anzeigen der App-Details")](uploading-images/image73.png#lightbox)

6. Klicken Sie in der Detailansicht auf „Ready to Upload Binary“ (Binärdatei zum Upload bereit), um die Anwendungspaketdatei einzureichen: 

    [![](uploading-images/image74.png "Auswählen der Binärdatei „Bereit zum Hochladen“")](uploading-images/image74.png#lightbox)

7. Beantworten Sie die Kryptografiefrage: 

    [![](uploading-images/image75.png "Beantworten der Kryptografiefrage")](uploading-images/image75.png#lightbox)

8. Die Website meldet, wenn sie bereit ist, die Anwendungspaketdatei zu akzeptieren: 

    [![](uploading-images/image76.png "Benachrichtigung zum Akzeptieren")](uploading-images/image76.png#lightbox)

9. Starten Sie das Anwendungsladeprogramm, und achten Sie darauf, dass Sie mit der Apple-ID angemeldet sind.
Klicken Sie auf **Deliver your App** (App liefern), um fortzufahren: 

    [![](uploading-images/image77.png "Die Schnittstelle des Anwendungsladeprogramms")](uploading-images/image77.png#lightbox)

10. Klicken Sie in der Anwendungsliste in **Binärdatei zum Upload bereit** auf „Status“ und dann auf **Weiter**: 

    [![](uploading-images/image78.png "Auswählen der zu ladenden App")](uploading-images/image78.png#lightbox)

11. Überprüfen Sie die Metadaten der Anwendung, und klicken Sie auf **Wählen...**, um die Paketdatei zu suchen: 

    [![](uploading-images/image79.png "Überprüfen der App-Metadaten")](uploading-images/image79.png#lightbox)

12. Suchen Sie die Paketdatei, die in Visual Studio für Mac mit der App Store-Buildkonfiguration erstellt wurde: 

    [![](uploading-images/image80.png "Auswählen der hochzuladenden Datei")](uploading-images/image80.png#lightbox)

13. Drücken Sie **Senden**: 

    [![](uploading-images/image81.png "Senden der App")](uploading-images/image81.png#lightbox)

14. Das Paket wird überprüft und etwaige Fehler werden gemeldet. Korrigieren Sie diese Fehler und laden Sie die App erneut hoch. Wenn der Upload erfolgreich durchgeführt wurde, wird die App automatisch zur Prüfung durch das App Store-Team eingereicht: 

    [![](uploading-images/image82.png "Ein Beispiel für Uploadfehler")](uploading-images/image82.png#lightbox)

Wenn die Anwendung genehmigt wurde, wird sie zum Download oder Kauf im Mac App Store zur Verfügung gestellt.

## <a name="related-links"></a>Verwandte Links

- [Installation](~//mac/get-started/installation.md)
- [„Hallo, Mac“-Beispiel](~//mac/get-started/hello-mac.md)
- [Verteilen Ihrer Apps im Mac App Store](https://developer.apple.com/devcenter/mac/checklist/)
- [Tools Guide : Code Signing Your App (Tool-Leitfaden: Codesignieren Ihrer App)](https://developer.apple.com/library/mac/#documentation/ToolsLanguages/Conceptual/OSXWorkflowGuide/CodeSigning/CodeSigning.html)
- [Developer ID and GateKeeper (Entwickler-ID und Gatekeeper)](https://developer.apple.com/resources/developer-id/)
