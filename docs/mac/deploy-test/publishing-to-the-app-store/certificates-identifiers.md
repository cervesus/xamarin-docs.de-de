---
title: Zertifikate und Bezeichner in Xamarin.Mac
description: Dieser Leitfaden enthält Informationen zum Erstellen der notwendigen Zertifikate und Bezeichner, die für das Veröffentlichen einer Xamarin.Mac-App benötigt werden.
ms.prod: xamarin
ms.assetid: 393d0066-7f6f-4ac3-a48d-4b5db65bc4cd
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 12/17/2019
ms.openlocfilehash: 2b2bfe9925a99c2ba7f1366ea28d5c72e2e1da88
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "76725540"
---
# <a name="certificates-and-identifiers-in-xamarinmac"></a>Zertifikate und Bezeichner in Xamarin.Mac

_Dieser Leitfaden enthält Informationen zum Erstellen der notwendigen Zertifikate und Bezeichner, die für das Veröffentlichen einer Xamarin.Mac-App benötigt werden._

## <a name="setup"></a>Setup

Besuchen Sie das [Apple Developer Member Center](https://developer.apple.com), um Ihren Mac für die Entwicklung zu konfigurieren. Klicken Sie auf den **Konto**-Link, und melden Sie sich an. Das Hauptmenü ist unten dargestellt:

> [!div class="mx-imgBorder"]
> [![Das Apple Developer Member Center](certificates-identifiers-images/devcenter01.png)](certificates-identifiers-images/devcenter01-large.png#lightbox)

Klicken Sie auf die Schaltfläche **Zertifikate, IDs & Profile** (oder auf die Plus-Schaltfläche neben der Überschrift **Zertifikate**):

> [!div class="mx-imgBorder"]
> [![Auswählen von Zertifikaten, IDs und Profilen](certificates-identifiers-images/devcenter02.png)](certificates-identifiers-images/devcenter02-large.png#lightbox)

Wählen Sie ein Zertifikattyp aus, und klicken Sie auf **Weiter**:

> [!div class="mx-imgBorder"]
> [![Auswählen des Zertifikatlinks](certificates-identifiers-images/devcenter03.png)](certificates-identifiers-images/devcenter03-large.png#lightbox)

Von hier können Sie die **Zwischenzertifikate** (Worldwide Developer Relations Certificate Authority und Developer ID Certificate Authority) falls nötig herunterladen (letztes Element am Fuß der Seite). Diese sollten jedoch automatisch für den Entwickler durch Xcode eingerichtet werden.

Im Rest dieses Abschnitts werden Sie durch die Abschnitte geführt, die für Mac-Entwickler relevant sind:

- **Registrieren der Mac-App-ID**: Der Entwickler muss diese Schritte für jeden Anwendung, die von ihm geschrieben werden, befolgen.
- **Registrieren von macOS-Systemen**: Dieser Schritt ist nur nötig, wenn Sie Computer zum Testen hinzufügen.
- **Erstellen von Zertifikaten**: Dieser Schritt ist nur einmal bei der Einrichtung der Zertifikate erforderlich oder später, wenn diese erneuert werden.
- **Erstellen eines Bereitstellungsprofils**: Der Entwickler muss diese Schritte für jede neue Anwendung, die geschrieben wird, befolgen, und wenn neue Systeme hinzugefügt werden.

## <a name="register-mac-app-id"></a>Registrieren einer Mac-App-ID

Sie müssen eine App-ID für jede Anwendung registrieren. Führen Sie die unten aufgeführten Schritte aus, um einen Eintrag zu erstellen:

1. Drücken Sie das Pluszeichen („+“), oder **registrieren Sie eine App-ID**:

    > [!div class="mx-imgBorder"]
    > [![Erste Schritte mit App-IDs](certificates-identifiers-images/appid01.png)](certificates-identifiers-images/appid01-large.png#lightbox)

1. Auswählen von **App-IDs**

    > [!div class="mx-imgBorder"]
    > [![Erste Schritte mit App-IDs](certificates-identifiers-images/appid02.png)](certificates-identifiers-images/appid02-large.png#lightbox)

1. Geben Sie eine **Beschreibung** ein, und wählen Sie beliebige **App-Dienste**, die die Anwendung benötigt: a. Die Plattform sollte **macOS** sein. a. Wählen Sie eine **Beschreibungs** aus (wird nur in diesem Portal verwendet). a. Geben Sie die **Bündel-ID** ein, die Ihrer **Info.plist** entsprechen soll. a. Wählen Sie die Funktionen aus, die für Ihre App erforderlich sind.

    > [!div class="mx-imgBorder"]
    > [![Eingeben der Beschreibung und von App-Diensten](certificates-identifiers-images/appid03.png)](certificates-identifiers-images/appid03-large.png#lightbox)

    Drücken Sie **Weiter**, um Ihre Auswahl zu überprüfen.

1. Wenn die Informationen richtig sind, klicken Sie auf **Registrieren**, um die Einrichtung abzuschließen:

    > [!div class="mx-imgBorder"]
    > [![Überprüfen der eingegebenen Daten](certificates-identifiers-images/appid04.png)](certificates-identifiers-images/appid04-large.png#lightbox)

1. Überprüfen Sie die Informationen, und klicken Sie auf **Senden**:

    > [!div class="mx-imgBorder"]
    > ![Überprüfen der Informationen](certificates-identifiers-images/appid05.png)

Einige **App-Dienste** erfordern möglicherweise weitere Konfigurationen (z.B. iCloud). Sollte dies der Fall sein, wählen Sie die soeben erstellte neue App-ID aus, und klicken Sie auf **Bearbeiten**:

> [!div class="mx-imgBorder"]
> [![Bearbeiten der neuen App-ID](certificates-identifiers-images/appid06.png)](certificates-identifiers-images/appid06-large.png#lightbox)

Um die iCloud-Dienste zu konfigurieren, klicken Sie z. B. auf die Schaltfläche **Bearbeiten**:

> [!div class="mx-imgBorder"]
> [![Konfigurieren der iCloud-Dienste](certificates-identifiers-images/appid07.png)](certificates-identifiers-images/appid07-large.png#lightbox)

## <a name="register-macos-devices"></a>Registrieren von macOS-Geräten

Um ein Bereitstellungsprofil für Testzwecke zu erstellen, muss der Entwickler seine Mac-Computer registrieren. Es können maximal 100 Computer zum Testen registriert werden.

1. Wählen Sie im Mac Developer Center **Alle** aus dem Bereich **Geräte** aus, und klicken Sie auf die Schaltfläche **+** :

    > [!div class="mx-imgBorder"]
    > [![Hinzufügen eines neuen Computers](certificates-identifiers-images/device01.png)](certificates-identifiers-images/device01-large.png#lightbox)

1. Geben Sie einen **Namen** und den **UUID** des hinzuzufügenden Computers an, und klicken Sie auf **Weiter**. Überprüfen Sie die Informationen, und klicken Sie auf die **Registrieren**-Schaltfläche:

    > [!div class="mx-imgBorder"]
    > [![Eingeben der neuen Computerinformationen](certificates-identifiers-images/device02.png)](certificates-identifiers-images/device02-large.png#lightbox)

1. Überprüfen und Bestätigen der eingegebenen Daten:

    > [!div class="mx-imgBorder"]
    > [![Eingeben der neuen Computerinformationen](certificates-identifiers-images/device03.png)](certificates-identifiers-images/device03-large.png#lightbox)

## <a name="create-certificates"></a>Erstellen von Zertifikaten

Verwenden Sie den Abschnitt „Zertifikate“, um unterschiedliche Arten von Zertifikaten zu erstellen, die zum Signieren von Mac-Anwendungen verwendet werden:

> [!div class="mx-imgBorder"]
> [![Neues Zertifikat erstellen](certificates-identifiers-images/devcenter04.png)](certificates-identifiers-images/devcenter04-large.png#lightbox)

Es gibt fünf Haupttypen von Zertifikaten, die für die macOS-Entwicklung relevant sind:

- **Mac-Entwicklung**: Optional für die allgemeine App-Entwicklung, jedoch erforderlich, wenn der Entwickler plant, Funktionen wie iCloud oder Pushbenachrichtigungen zu verwenden. Der Entwickler benötigt ein Entwicklungszertifikat, bevor die Erstellung von Bereitstellungsprofilen möglich ist, mit denen er auf diese Funktionen zugreifen kann.
- **Mac App-Verteilung**: Der Entwickler benötigt ein Zertifikat für seine App und ein weiteres für den Installer.
- **Mac Installer-Verteilung**: Der Entwickler benötigt ein Zertifikat für seine App und ein weiteres für den Installer.
- **Entwickler-ID Installer**: Zertifikate für den Installer, um ihn außerhalb des Mac App Store zu verteilen.
- **Entwickler-ID-Anwendung**: Zertifikate für die App, um sie außerhalb des Mac App Store zu verteilen.

In den folgenden Abschnitten finden Sie Beispiele zum Erstellen einiger dieser Typen von Zertifikattypen.

### <a name="mac-development-certificate"></a>Mac-Entwicklungszertifikat

Wie zuvor erwähnt, ist das Mac-Entwicklungszertifikat nicht erforderlich, es sei denn, es werden macOS-Funktionen wie iCloud oder Pushbenachrichtigungen verwendet.

Führen Sie folgenden Schritt aus, um ein neues Entwicklungszertifikat zu erstellen:

1. Wählen Sie das Optionsfeld **Mac Development** (Mac-Entwicklung), und klicken Sie auf **Weiter**:

    > [!div class="mx-imgBorder"]
    > [![Hinzufügen eines Entwicklungszertifikats](certificates-identifiers-images/certif02.png)](certificates-identifiers-images/certif02-large.png#lightbox)

1. Laden Sie eine _Zertifikatsignieranforderung_ hoch. Die Zertifikatanforderungsdatei (Erweiterung `.certSigningRequest`) wird lokal auf dem Mac gespeichert. Klicken Sie auf **Datei auswählen**, um die Zertifikatanforderung auszuwählen, und drücken Sie dann **Weiter**.

    > [!div class="mx-imgBorder"]
    > [![Hochladen einer Zertifikatanforderungsdatei](certificates-identifiers-images/certif03.png)](certificates-identifiers-images/certif03-large.png#lightbox)

    Anleitungen zum Verwenden des **Keychain-Zugriffs** zum Erstellen einer Zertifikatanforderungsdatei finden Sie unter dem Link [Weitere Informationen >](https://help.apple.com/developer-account/#/devbfa00fef7).

1. Drücken Sie **Herunterladen**, um die Zertifikatdatei abzurufen, und doppelklicken Sie darauf, um sie zu installieren:

    > [!div class="mx-imgBorder"]
    > [![Herunterladen der Zertifikatdatei](certificates-identifiers-images/certif04.png)](certificates-identifiers-images/certif04-large.png#lightbox)

Wie bereits erwähnt ist das Entwicklerzertifikat nicht immer erforderlich, es sei denn, der Entwickler implementiert macOS-Funktionen wie iCloud oder Pushbenachrichtigungen. Zudem ist erforderlich, ein **Entwicklungsbereitstellungsprofil** zu erstellen, das Sie zum Testen von Mac App Store-Apps benötigen.

### <a name="mac-app-store-certificates"></a>Mac App Store-Zertifikate

Um eine App im App Store freizugeben, benötigen Sie zwei Zertifikate:

- **Mac App-Verteilung**szertifikat, das zum Signieren der Anwendung verwendet wird, und
- **Mac Installer-Verteilung**szertifikat, um den Installer zu signieren.

> [!TIP]
> Seien Sie bei der Benennung der Zertifikatanforderung für diese Schlüssel vorsichtig: Verwenden Sie aussagekräftige Namen, die `Application` und `Installer` enthalten, damit man sie später voneinander unterscheiden kann.

Erstellen Sie als Erstes das Installerzertifikat:

1. Wählen Sie **Mac Installer-Verteilung** als Zertifikattyp aus, und klicken Sie auf die Schaltfläche **Weiter**:

    > [!div class="mx-imgBorder"]
    > [![Erstellen eines App Store-Zertifikats](certificates-identifiers-images/certif05.png)](certificates-identifiers-images/certif05-large.png#lightbox)

1. Auf der nächsten Seite wird erläutert, wie Sie den **Keychain-Zugriff** verwenden können, um eine Zertifikatanforderungsdatei zu erstellen. Befolgen Sie diese Anweisungen:

    > [!div class="mx-imgBorder"]
    > [![Hochladen einer Zertifikatanforderung](certificates-identifiers-images/certif06.png)](certificates-identifiers-images/certif06-large.png#lightbox)

    Anleitungen zum Verwenden des **Keychain-Zugriffs** zum Erstellen einer Zertifikatanforderungsdatei finden Sie unter dem Link [Weitere Informationen >](https://help.apple.com/developer-account/#/devbfa00fef7). Denken Sie daran, einen Zertifikatnamen auszuwählen, der den _Typ_ des Zertifikats wiedergibt (Anwendung oder Installer).

1. Klicken Sie auf **Download**, um Ihr Zertifikat abzurufen, und doppelklicken Sie sie, um Sie in der **Keychain** zu speichern:

    > [!div class="mx-imgBorder"]
    > [![Herunterladen des App Store-Zertifikats](certificates-identifiers-images/certif07.png)](certificates-identifiers-images/certif07-large.png#lightbox)

**Befolgen Sie dieselben Schritte für das Mac App-Verteilungszertifikat.**

![Mac App-Verteilungszertifikat](certificates-identifiers-images/certif08.png)

### <a name="developer-id-certificates"></a>Entwickler-ID-Zertifikate

Wenn sie eine Xamarin.Mac-Anwendung selbst veröffentlichen möchten (also nicht über den Apple App Store), benötigen Sie zwei Zertifikate:

- **Entwickler-ID Installer**-Zertifikat, das zum Signieren der Anwendung verwendet wird, und
- **Entwickler-ID-Anwendung**szertifikat, um den Installer zu signieren.

> [!TIP]
> Seien Sie bei der Benennung der Zertifikatanforderung für diese Schlüssel vorsichtig: Verwenden Sie aussagekräftige Namen, die `Application` und `Installer` enthalten, damit man sie später voneinander unterscheiden kann.

Nachdem Sie Zertifikate erstellt, heruntergeladen und installiert haben, werden Sie im **Keychain-Zugriff** angezeigt:

[Zertifikatliste des Keychain-Zugriffs](certificates-identifiers-images/certif09.png)

## <a name="related-links"></a>Verwandte Links

- [Installation](/visualstudio/mac/installation/)
- [„Hallo, Mac“-Beispiel](~/mac/get-started/hello-mac.md)
- [Verteilen Ihrer Apps im Mac App Store](https://developer.apple.com/devcenter/mac/checklist/)
- [Developer ID and GateKeeper (Entwickler-ID und Gatekeeper)](https://developer.apple.com/developer-id/)
