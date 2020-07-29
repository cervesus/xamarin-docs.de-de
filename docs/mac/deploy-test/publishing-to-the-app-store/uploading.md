---
title: Upload in den Mac App Store
description: In diesem Dokument wird beschrieben, wie Sie iTunes Connect verwenden können, um eine Xamarin.Mac-App in den Mac App Store hochzuladen. Dabei werden die Informationen erläutert, die iTunes Connect erfordert, um den Vorgang abzuschließen.
ms.prod: xamarin
ms.assetid: 30cd0e47-1b2e-47ef-93f6-4bed20b15c03
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 57591a6e9fcaa3c8271ab27756160ee1c46a4fa3
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86935927"
---
# <a name="upload-to-mac-app-store"></a>Upload in den Mac App Store

_Dieser Leitfaden enthält Informationen zum Hochladen einer Xamarin.Mac-App für die Veröffentlichung im Mac App Store._

Anwendungen werden über [iTunes Connect](https://itunesconnect.apple.com/) zur Genehmigung im Mac App Store veröffentlicht. Sie benötigen auch das [**Transporter**](https://apps.apple.com/us/app/transporter/id1450874784?mt=12)-Tool aus dem App Store.

1. Wählen Sie ein **macOS-App** aus, die Sie erstellen möchten:

    [![iTunes Connect](uploading-images/image65.png)](uploading-images/image65.png#lightbox)

2. Geben Sie den Namen der App und weitere Details ein. Der Entwickler kann sich nur für eine vorhandene **Bundle-ID** entscheiden, die bereits erstellt wurde:

    [![Auswählen der Bundle-ID](uploading-images/image66.png)](uploading-images/image66.png#lightbox)

3. Wählen Sie das Verfügbarkeitsdatum und den Preis aus. Unabhängig vom vom Entwickler gewählten Verfügbarkeitsdatum wird die App erst dann zum Verkauf freigegeben, nachdem Sie genehmigt wurde. Dieser Wert kann weit in der Zukunft liegen, wenn der Entwickler mehr Kontrolle über das tatsächliche Verfügbarkeitsdatum haben möchte:

    [![Festlegen des Verfügbarkeitsdatums und des Preises](uploading-images/image67.png)](uploading-images/image67.png#lightbox)

4. Geben Sie die App-Informationen ein, einschließlich der entsprechenden Kategorie im App Store:

    [![Eingeben der App-Informationen](uploading-images/image68.png)](uploading-images/image68.png#lightbox)

    Wählen Sie die entsprechenden Bewertungen aus:

    [![Festlegen der App-Bewertungen](uploading-images/image69.png)](uploading-images/image69.png#lightbox)

    Beschreibung, Schlüsselwörter und Kontakt-URLs:

    [![Bearbeiten der Beschreibung, Schlüsselwörter und Kontakt-URLs](uploading-images/image70.png)](uploading-images/image70.png#lightbox)

    Kontaktinformation und Empfehlungen für App Store-Prüfer:

    [![Bearbeiten der Kontaktinformation und Empfehlungen für App Store-Prüfer](uploading-images/image71.png)](uploading-images/image71.png#lightbox)

    Und als letztes Screenshots:

    [![Hinzufügen der erforderlichen Screenshots](uploading-images/image72.png)](uploading-images/image72.png#lightbox)

    Screenshots sollten im Format JPG, TIF oder PNG sein und 1280x800, 1440x900, 2880x1800 oder 2560x1600 Pixel aufweisen. Drücken Sie auf **Speichern**, um den Vorgang zu beenden.

5. Die App-Informationen werden zur Überprüfung angezeigt. Klicken Sie auf **Details anzeigen**, um den Status zu ändern:

    [![Anzeigen der App-Details](uploading-images/image73.png)](uploading-images/image73.png#lightbox)

6. Klicken Sie in der Detailansicht auf „Ready to Upload Binary“ (Binärdatei zum Upload bereit), um die Anwendungspaketdatei einzureichen:

    [![Auswählen von „Binärdatei zum Upload bereit“](uploading-images/image74.png)](uploading-images/image74.png#lightbox)

7. Beantworten Sie die Kryptografiefrage:

    [![Beantworten der Kryptografiefrage](uploading-images/image75.png)](uploading-images/image75.png#lightbox)

8. Die Website meldet, wenn sie bereit ist, die Anwendungspaketdatei zu akzeptieren:

    [![Benachrichtigung zum Akzeptieren](uploading-images/image76.png)](uploading-images/image76.png#lightbox)

9. Starten Sie **Transporter**, melden Sie sich mit Ihrer Apple ID an, und wählen Sie dann **APP HINZUFÜGEN** aus:

    [![Benutzeroberfläche des Anwendungsladeprogramms](uploading-images/transporter01-sml.png)](uploading-images/transporter01.png#lightbox)

    Befolgen Sie die Anweisungen zum Hochladen Ihres App-Pakets in iTunes Connect.

    > [!NOTE]
    > [**Transporter**](https://apps.apple.com/us/app/transporter/id1450874784?mt=12) ersetzt das **Anwendungslade**tool (Application Loader), das mit Xcode 10 und niedriger verwendet wurde.
    > Application Loader ist in Xcode 11 oder höher nicht mehr verfügbar.

Wenn die Anwendung genehmigt wurde, wird sie zum Download oder Kauf im Mac App Store zur Verfügung gestellt.

## <a name="related-links"></a>Verwandte Links

- [Installation](~//mac/get-started/installation.md)
- [„Hallo, Mac“-Beispiel](~/mac/get-started/hello-mac.md)
- [Verteilen Ihrer Apps im Mac App Store](https://developer.apple.com/devcenter/mac/checklist/)
- [Tool-Leitfaden: Codesignieren Ihrer App](https://developer.apple.com/library/mac/#documentation/ToolsLanguages/Conceptual/OSXWorkflowGuide/CodeSigning/CodeSigning.html)
- [Developer ID and GateKeeper (Entwickler-ID und Gatekeeper)](https://developer.apple.com/developer-id/)
