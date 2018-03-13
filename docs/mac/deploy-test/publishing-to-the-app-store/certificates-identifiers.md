---
title: Zertifikate und Bezeichner
description: "Dieser Leitfaden enthält Informationen zum Erstellen der notwendigen Zertifikate und Bezeichner, die für das Veröffentlichen einer Xamarin.Mac-App benötigt werden."
ms.topic: article
ms.prod: xamarin
ms.assetid: 393d0066-7f6f-4ac3-a48d-4b5db65bc4cd
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: a1065fb91a23827c4876654470cda5022aa1d3b8
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="certificates-and-identifiers"></a>Zertifikate und Bezeichner

_Dieser Leitfaden enthält Informationen zum Erstellen der notwendigen Zertifikate und Bezeichner, die für das Veröffentlichen einer Xamarin.Mac-App benötigt werden._

## <a name="certificates-and-identifiers"></a>Zertifikate und Bezeichner

Besuchen Sie das [Apple Developer Member Center](http://developer.apple.com), um Ihren Mac für die Entwicklung zu konfigurieren. Das Hauptmenü ist unten dargestellt:

[![Das Apple Developer Member Center](certificates-identifiers-images/devcenter01.png "The Apple Developer Member Center")](certificates-identifiers-images/devcenter01-large.png#lightbox)

Klicken Sie auf den Link **Zertifikate, Bezeichner & Profile**:

[![Auswählen von „Zertifikate, Bezeichner und Profile“](certificates-identifiers-images/devcenter02.png "Selecting Certificates, Identifiers & Profiles")](certificates-identifiers-images/devcenter02-large.png#lightbox)

Klicken Sie anschließend auf den **Zertifikate-Link** unter dem Abschnitt **Mac-Apps**:

[![Auswählen der Verknüpfung „Zertifikate“](certificates-identifiers-images/devcenter03.png "Selecting the Certificates link")](certificates-identifiers-images/devcenter03-large.png#lightbox)

Klicken Sie auf den Link **Alle** und anschließend auf die Schaltfläche **+**:

[![Auswählen von „Alle“ und Hinzufügen eines neuen Elements](certificates-identifiers-images/certif01.png "Selecting all and adding a new item")](certificates-identifiers-images/certif01-large.png#lightbox)

Laden Sie von hier die **Zwischenzertifikate** (Worldwide Developer Relations Certificate Authority und Developer ID Certificate Authority) falls nötig herunter. Diese sollten jedoch automatisch für den Entwickler durch Xcode eingerichtet werden.

Im verbleibenden Abschnitt werden Sie zu jedem der vier Abschnitte geführt, um die Einrichtung eines Mac-Entwicklerkontos abzuschließen.

-   **Registrieren der Mac-App-ID**: Der Entwickler muss diese Schritte für jeden Anwendung, die von ihm geschrieben werden, befolgen.
-   **Registrieren von macOS-Systemen**: Dieser Schritt ist nur nötig, wenn Sie Computer zum Testen hinzufügen.
-   **Erstellen von Zertifikaten**: Dieser Schritt ist nur einmal bei der Einrichtung der Zertifikate erforderlich oder später, wenn diese erneuert werden.
-   **Erstellen eines Bereitstellungsprofils**: Der Entwickler muss diese Schritte für jede neue Anwendung, die geschrieben wird, befolgen, und wenn neue Systeme hinzugefügt werden.

Klicken Sie oben links auf der Seite auf den Link **Übersicht**, um jederzeit zu diesem Menü zurückzukehren.

### <a name="register-mac-app-id"></a>Registrieren einer Mac-App-ID

Der Entwickler muss eine App-ID für jede geschriebene Anwendung registrieren. Befolgen Sie die unten stehenden Schritte, um einen Eintrag für eine einfache Beispiel-App namens „MacWriter“ zu erstellen.

1. Geben Sie eine **App-ID-Beschreibung** ein, und wählen Sie beliebige **App-Dienste**, die die Anwendung benötigt: 

    [![Eingeben der Beschreibung und des App-Diensts](certificates-identifiers-images/devcenter04.png "Entering the description and app services")](certificates-identifiers-images/devcenter04-large.png#lightbox)
2. Geben Sie eine **Bündel-ID** für die App ein, und klicken Sie auf die Schaltfläche **Weiter**: 

    [![Eingeben einer Bündel-ID](certificates-identifiers-images/devcenter05.png "Entering a bundle ID")](certificates-identifiers-images/devcenter05-large.png#lightbox)
3. Überprüfen Sie die Informationen, und klicken Sie auf **Senden**: 

    [![Überprüfen der Informationen](certificates-identifiers-images/devcenter06.png "Verifying the information")](certificates-identifiers-images/devcenter06-large.png#lightbox)

Einige **App-Dienste** erfordern möglicherweise weitere Konfigurationen (z.B. iCloud). Sollte dies der Fall sein, wählen Sie die soeben erstellte neue App-ID aus, und klicken Sie auf **Bearbeiten**:

[![Bearbeiten der neuen App-ID](certificates-identifiers-images/devcenter07.png "Editing the new App ID")](certificates-identifiers-images/devcenter07-large.png#lightbox)

Um die iCloud-Dienste zu konfigurieren, klicken Sie auf die Schaltfläche **Bearbeiten**:

[![Konfigurieren der iCloud-Dienste](certificates-identifiers-images/devcenter08.png "Configuring the iCloud services")](certificates-identifiers-images/devcenter08-large.png#lightbox)

Von hier aus kann der Entwickler die Datenbanken, die er verwenden wird, konfigurieren:

[![Konfigurieren der Datenbanken](certificates-identifiers-images/devcenter09.png "Configuring the databases")](certificates-identifiers-images/devcenter09-large.png#lightbox)

### <a name="register-macos-systems"></a>Registrieren von macOS-Systemen

Um ein Bereitstellungsprofil für Testzwecke zu erstellen, muss der Entwickler seine Mac-Computer registrieren. Entwickler können maximal 100 Computer zum Testen ihrer Mac-Apps registrieren.

Wählen Sie im Mac Developer Center **Alle** aus dem Bereich **Geräte** aus, und klicken Sie auf die Schaltfläche **+**:

[![Hinzufügen eines neuen Computers](certificates-identifiers-images/devcenter10.png "Adding a new computer")](certificates-identifiers-images/devcenter10-large.png#lightbox)

Geben Sie einen **Namen** und den **UUID** des hinzuzufügenden Computers an, und klicken Sie auf **Weiter**. Überprüfen Sie die Informationen, und klicken Sie auf die **Registrieren**-Schaltfläche:

[![Eingeben der neuen Computerinformationen](certificates-identifiers-images/devcenter11.png "Entering the new computer information")](certificates-identifiers-images/devcenter11-large.png#lightbox)

### <a name="create-certificates"></a>Erstellen von Zertifikaten

Verwenden Sie den Abschnitt „Zertifikate“, um unterschiedliche Arten von Zertifikaten zu erstellen, die zum Signieren von Mac-Anwendungen verwendet werden:

[![Erstellen eines neuen Zertifikats](certificates-identifiers-images/certif01.png "Creating a new certificate")](certificates-identifiers-images/certif01-large.png#lightbox)

Bei Zertifikaten wird zwischen drei Haupttypen unterschieden:

-   **Mac-Entwicklungszertifikat**: Optional für die allgemeine App-Entwicklung, jedoch erforderlich, wenn der Entwickler plant, Funktionen wie iCloud oder Pushbenachrichtigungen zu verwenden. Der Entwickler benötigt ein Entwicklungszertifikat, bevor die Erstellung von Bereitstellungsprofilen möglich ist, mit denen er auf diese Funktionen zugreifen kann.
-   **Mac App Store**: Der Entwickler benötigt ein Zertifikat für seine App und ein weiteres für den Installer.
-   **Entwickler-ID**: Zertifikate für die App und den Installer, wenn Sie außerhalb des Mac App Store verteilen möchten.

In den folgenden Abschnitten finden Sie Beispiele zum Erstellen jedes Typs der oben genannten Zertifikattypen.

#### <a name="mac-development-certificate"></a>Mac-Entwicklungszertifikat

Wie zuvor erwähnt, ist das Mac-App-Entwicklungszertifikat nicht erforderlich, es sei denn, es werden macOS-Funktionen wie iCloud oder Pushbenachrichtigungen verwendet.

Führen Sie folgenden Schritt aus, um ein neues Entwicklungszertifikat zu erstellen:

1. Wählen Sie das Optionsfeld **Mac Development** (Mac-Entwicklung), und klicken Sie auf **Weiter**: 

     [![Hinzufügen eines Entwicklungszertifikats](certificates-identifiers-images/certif02.png "Adding a development certificate")](certificates-identifiers-images/certif02-large.png#lightbox)
2. Auf dem nächsten Bildschirm ist dargestellt,wie Sie den Keychain-Zugriff zum Erstellen einer Anforderungsdatei zur Zertifikatsignierung, die hochgeladen werden kann, verwenden: 

    [![Uploadanzeige für den Keychain-Zugriff](certificates-identifiers-images/certif03.png "The keychain access upload screen")](certificates-identifiers-images/certif03-large.png#lightbox)
3. Wählen Sie einen aussagekräftigen allgemeinen Namen für das Zertifikat, so dass er einfach für später zu merken ist, wenn das finale Zertifikat erstellt wird. Merken Sie sich, wo die Datei gespeichert wurde, sodass Sie sie im nächsten Schritt wieder finden: 

     ![Exportieren eines Zertifikats](certificates-identifiers-images/image12.png "Exporting a certificate")
4. Eine Zertifikatanforderungsdatei (Erweiterung `.certSigningRequest`) wird lokal auf dem Mac gespeichert. Merken Sie sich, wo die Datei gespeichert wurde (der Standardspeicherort ist der Desktop), da Sie sie im nächsten Schritt auswählen müssen: 

     [![Hochladen der Zertifikatdatei](certificates-identifiers-images/image13.png "Uploading the certificate file")](certificates-identifiers-images/image13-large.png#lightbox)
5. Klicken Sie auf **Download**, um das Zertifikat abzurufen, und doppelklicken Sie sie, um Sie in der **Keychain** zu speichern: 

     [![Herunterladen eines Entwicklungszertifikats](certificates-identifiers-images/image15.png "Downloading a development certificate")](certificates-identifiers-images/image15-large.png#lightbox)
6. Klicken Sie auf **Download**, um das Zertifikat abzurufen, und doppelklicken Sie sie, um Sie in der **Keychain** zu speichern. Das **Dienstprogramm „Entwicklerzertifikat“** zeigt die Zertifikate folgendermaßen an: 

     [![Hilfsprogramm „Entwicklerzertifikat“](certificates-identifiers-images/image16.png "The Developer Certificate Utility")](certificates-identifiers-images/image16-large.png#lightbox)
7. Außerdem wird dies folgendermaßen in der **Keychain** angezeigt: 

     ![Das Zertifikat im Keychain-Zugriff](certificates-identifiers-images/image17.png "The certificate in Keychain Access")

Wie bereits erwähnt ist das Entwicklerzertifikat nicht immer erforderlich, es sei denn, der Entwickler implementiert macOS-Funktionen wie iCloud oder Pushbenachrichtigungen. Zudem ist erforderlich, ein **Entwicklungsbereitstellungsprofil** zu erstellen, das Sie zum Testen von Mac App Store-Apps benötigen.

#### <a name="mac-app-store-certificates"></a>Mac App Store-Zertifikate

Um eine App im App Store zu veröffentlichen, muss ein **Mac App Store**-Zertifikat erstellt werden, das verwendet wir, um die Anwendung und das Mac-Installerpaket zu signieren.

1. Wählen Sie **Mac App Store** als Zertifikattyp, und klicken Sie auf **Weiter**: 

    [![Erstellen eines App-Store-Zertifikats](certificates-identifiers-images/certif04.png "Creating an App Store Certificate")](certificates-identifiers-images/certif04-large.png#lightbox)
2. Wählen Sie den Typ des Zertifikats aus, das erstellt werden soll (Sie benötigen jeweils ein Zertifikat jeden Typs zur Veröffentlichung im App Store): 

    [![Auswählen des Zertifikattyps](certificates-identifiers-images/certif05.png "Selecting the certificate type")](certificates-identifiers-images/certif05-large.png#lightbox)
3. Auf der nächsten Seite wird erläutert, wie Sie den **Keychain-Zugriff** verwenden können, um eine Zertifikatanforderungsdatei zu erstellen. Befolgen Sie diese Anweisungen: 

     [![Generieren der Keychain-Anforderung](certificates-identifiers-images/certif06.png "Generating the keychain request")](certificates-identifiers-images/certif06-large.png#lightbox)
4. Wählen Sie einen aussagekräftigen **allgemeinen Namen** aus. Verwenden Sie z.B. „App Store-Anwendung“ im Namen: 

     ![Eingeben eines beschreibenden Namens](certificates-identifiers-images/image20.png "Entering a descriptive name")
5. Eine Zertifikatanforderungsdatei (Erweiterung `.certSigningRequest`) wird lokal auf dem Mac gespeichert. Merken Sie sich, wo sie gespeichert wurde (der Standardspeicherort ist der Desktop): 

     [![Speichern des Zertifikats](certificates-identifiers-images/image21.png "Saving the certificate")](certificates-identifiers-images/image21-large.png#lightbox)
6. Klicken Sie auf **Download**, um Ihr Zertifikat abzurufen, und doppelklicken Sie sie, um Sie in der **Keychain** zu speichern: 

      [![Herunterladen des Zertifikats für den App Store](certificates-identifiers-images/image23.png "Downloading the App Store certificate")](certificates-identifiers-images/image23-large.png#lightbox)
7. Klicken Sie auf **Weiter**, und führen Sie genau diese Schritte erneut durch, um ein weiteres Zertifikat herunterzuladen, dieses Mal für den *Installer*: 

     [![Auswählen des Installationsprogramms](certificates-identifiers-images/image24.png "Selecting the installer")](certificates-identifiers-images/image24-large.png#lightbox)
8. Wählen Sie einen aussagekräftigen **allgemeinen Namen** aus. Verwenden Sie z.B. „App Store-Installer“ im Namen: 

     ![Festlegen des Zertifikatnamens](certificates-identifiers-images/image25.png "Setting the name of the certificate")
9. Eine Zertifikatanforderungsdatei (Erweiterung `.certSigningRequest`) wird lokal auf dem Mac gespeichert. Merken Sie sich, wo sie gespeichert wurde (der Standardspeicherort ist der Desktop): 

     [![Hochladen des Zertifikats](certificates-identifiers-images/image26.png "Uploading the certificate")](certificates-identifiers-images/image26-large.png#lightbox) 

     [![Herunterladen des Verteilungszertifikats](certificates-identifiers-images/image28.png "Downloading the distribution certificate")](certificates-identifiers-images/image28-large.png#lightbox)
10. Klicken Sie auf **Download**, um das Zertifikat abzurufen, und doppelklicken Sie sie, um Sie in der **Keychain** zu speichern. Das Dienstprogramm „Entwicklerzertifikat“ zeigt die Zertifikate folgendermaßen an: 

     [![Hilfsprogramm „Entwicklerzertifikat“](certificates-identifiers-images/image29.png "The Developer Certificate Utility")](certificates-identifiers-images/image29-large.png#lightbox)
11. Die beiden neuen Zertifikate werden in der **Keychain** angezeigt: 

     [![Das Zertifikat im Keychain-Zugriff](certificates-identifiers-images/image30.png "The certificate in Keychain Access")](certificates-identifiers-images/image30-large.png#lightbox)

#### <a name="developer-id-certificates"></a>Entwickler-ID-Zertifikate

Wenn sie die Xamarin.Mac-App selbst veröffentlichen möchten (also nicht über den Apple App Store), benötigen Entwickler ein Entwickler-ID-Zertifikat, um die App für die Veröffentlichung und Installation zu signieren.

Führen Sie folgende Schritte aus:

1. Beginnen Sie im Abschnitt **Zertifikate**, und klicken Sie auf **+** und dann auf das Optionsfeld **Entwickler-ID**: 

    [![Hinzufügen einer Entwickler-ID](certificates-identifiers-images/certif07.png "Adding a developer ID")](certificates-identifiers-images/certif07-large.png#lightbox)
2. Klicken Sie auf **Weiter**, und wählen Sie dann den Typ der zu erstellenden Entwickler-ID aus: 

    [![Auswählen des Typs der Entwickler-ID](certificates-identifiers-images/certif08.png "Selecting the developer ID type")](certificates-identifiers-images/certif08-large.png#lightbox)
3. Es sind zwei nötig: eine zum Signieren der Anwendung selbst und eine zum Signieren des Installers der Anwendung. Seien Sie bei der Benennung der Zertifikatanforderung für diese Schlüssel vorsichtig: Verwenden Sie aussagekräftige Namen, die `Application` und `Installer` enthalten, damit man sie später voneinander unterscheiden kann.
4. Auf dem nächsten Bildschirm erhalten Sie ausführliche Anweisungen zum Erstellen von Zertifikaten. Klicken Sie auf **Weiter**: 

    [![Erstellen eines Zertifikats](certificates-identifiers-images/certif09.png "How to create the certificate")](certificates-identifiers-images/certif09-large.png#lightbox)
5. Wählen Sie einen aussagekräftigen **allgemeinen Namen** aus. Verwenden Sie z.B. „Entwickler-ID-Anwendung“ im Namen: 

     ![Eingeben eines Namens für das Zertifikat](certificates-identifiers-images/image33.png "Entering a name for the certificate")
6. Eine Zertifikatanforderungsdatei (Erweiterung `.certSigningRequest`) wird lokal auf dem Mac gespeichert. Merken Sie sich, wo sie gespeichert wurde (der Standardspeicherort ist der Desktop): 

     [![Hochladen des Zertifikats](certificates-identifiers-images/certif10.png "Uploading the certificate")](certificates-identifiers-images/certif10-large.png#lightbox) 

     [![Herunterladen der Entwickler-ID](certificates-identifiers-images/certif11.png "Downloading the developer ID")](certificates-identifiers-images/certif11-large.png#lightbox)
7. Klicken Sie auf **Download**, um das Zertifikat abzurufen, und doppelklicken Sie sie, um Sie in der **Keychain** zu speichern.
8. Klicken Sie auf **Weiter**, und führen Sie genau diese Schritte erneut durch, um ein weiteres Zertifikat herunterzuladen, dieses Mal für den *Installer*.
9. Wählen Sie einen aussagekräftigen **allgemeinen Namen** aus. Verwenden Sie z.B. „Entwickler-ID-Installer“ im Namen: 

     ![Eingeben eines allgemeinen Namens](certificates-identifiers-images/image38.png "Entering a common name")
10. Eine Zertifikatanforderungsdatei (Erweiterung `.certSigningRequest`) wird lokal auf dem Mac gespeichert. Merken Sie sich, wo sie gespeichert wurde (der Standardspeicherort ist der Desktop): 

     [![Hochladen eines Zertifikats](certificates-identifiers-images/certif10.png "Uploading a certificate")](certificates-identifiers-images/certif10-large.png#lightbox)
11. Dann steht das Zertifikat zum Download zur Verfügung. Klicken Sie auf **Download** und dann auf **Fertig**: 

     [![Herunterladen eines Zertifikats](certificates-identifiers-images/certif11.png "Downloading a certificate")](certificates-identifiers-images/certif11-large.png#lightbox)
12. Klicken Sie auf **Download**, um das Zertifikat abzurufen, und doppelklicken Sie sie, um Sie in der **Keychain** zu speichern. Das **Dienstprogramm „Entwicklerzertifikat“** zeigt die Zertifikate folgendermaßen an: 

     [![Hilfsprogramm „Entwicklerzertifikat“](certificates-identifiers-images/certif12.png "The Developer Certificate Utility")](certificates-identifiers-images/certif12-large.png#lightbox)
13. Die folgenden Elemente werden in der **Keychain** angezeigt: 

     [![Das Zertifikat im Keychain-Zugriff](certificates-identifiers-images/image43.png "The certificate in Keychain Access")](certificates-identifiers-images/image43-large.png#lightbox)


## <a name="related-links"></a>Verwandte Links

- [Installation](/visualstudio/mac/installation/)
- [„Hallo, Mac“-Beispiel](~/mac/get-started/hello-mac.md)
- [Verteilen Ihrer Apps im Mac App Store](https://developer.apple.com/devcenter/mac/checklist/)
- [Developer ID and GateKeeper (Entwickler-ID und Gatekeeper)](https://developer.apple.com/resources/developer-id/)
