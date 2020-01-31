---
title: Bereitstellungsprofile für Xamarin.Mac-Apps
description: Dieser Leitfaden enthält Informationen zum Erstellen der erforderlichen Bereitstellungsprofile, die für das Veröffentlichen einer Xamarin.Mac-App benötigt werden.
ms.prod: xamarin
ms.assetid: bdff6c32-f7e3-4a97-a093-dbda48be8227
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 04/12/2017
ms.openlocfilehash: 00d7610bda3d1b7c9f954df64bb6e3af982b1d06
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725518"
---
# <a name="provisioning-profiles-for-xamarinmac-apps"></a>Bereitstellungsprofile für Xamarin.Mac-Apps

Bereitstellungsprofile ermöglichen es Entwicklern, mehrere spezifische macOS-Funktionen (ehemals Mac OS X), z.B. iCloud und Pushbenachrichtigungen, in ihre Xamarin.Mac-Apps zu integrieren. Sie müssen ein Mac-Bereitstellungsprofil für jede entwickelte Anwendung, die diese Funktionen verwendet, erstellen, herunterladen und installieren.

[![](profiles-images/certif13.png "The Apple Provisioning Portal")](profiles-images/certif13.png#lightbox)

## <a name="development-provisioning-profile"></a>Entwicklungsbereitstellungsprofil

Durch ein Entwicklungsbereitstellungsprofil kann eine App für den Mac App Store auf den bestimmten Computern getestet werden, die im Profil eingerichtet wurden. Dies ist besonders wichtig, wenn Sie macOS-Funktionen wie iCloud und Pushbenachrichtigungen verwenden.

> [!NOTE]
> Der Entwickler muss ein Mac-Entwicklungszertifikat erstellt haben, bevor ein Entwicklungsbereitstellungsprofil erstellt werden kann. Füllen Sie wie in diesem Screenshot gezeigt die Details aus, um ein **Entwicklungsbereitstellungsprofil** zu generieren, das für das Erstellen von Builds verwendet werden kann. Im Feld **Zertifikate** muss ein gültiges Mac-Entwicklungszertifikat zur Auswahl verfügbar sein und mindestens ein für das Testen registriertes System.

Führen Sie folgende Schritte aus:

1. Wählen Sie die Art des Bereitstellungsprofils aus, das Sie erstellen möchten, und klicken Sie auf die Schaltfläche **Weiter**:

    [![](profiles-images/certif14.png "Selecting the profile type")](profiles-images/certif14.png#lightbox)
2. Wählen Sie die ID der Anwendung aus, für die das Profil erstellt werden soll, und klicken Sie auf die Schaltfläche **Weiter**:

    [![](profiles-images/certif15.png "Selecting the app ID")](profiles-images/certif15.png#lightbox)
3. Wählen Sie die Entwickler-ID aus, die für das Signieren des Profils verwendet wurde, und klicken Sie auf **Weiter**:

    [![](profiles-images/certif16.png "Selecting the developer ID")](profiles-images/certif16.png#lightbox)
4. Wählen Sie die Computer aus, auf denen dieses Profil verwendet werden kann, und klicken Sie auf **Weiter**:

    [![](profiles-images/certif17.png "Selecting the allowed computers")](profiles-images/certif17.png#lightbox)
5. Geben Sie nun einen **Profilnamen** ein, und klicken Sie auf die Schaltfläche **Generieren**:

    [![](profiles-images/certif18.png "Generating the profile")](profiles-images/certif18.png#lightbox)
6. Klicken Sie auf die Schaltfläche **Herunterladen**, um das neue Profil herunterzuladen:

    [![](profiles-images/certif19.png "Downloading the profile")](profiles-images/certif19.png#lightbox)
7. Entwicklungsbereitstellungsprofile werden im Bereich „Profileinstellungen“ der Mac-Anwendung **Systemeinstellungen** installiert:

    [![](profiles-images/certif20.png "Installing the profile")](profiles-images/certif20.png#lightbox)
8. Im Bereich „Profileinstellungen“ werden nun alle installierten Profile angezeigt:

    [![](profiles-images/image47.png "Showing all installed profiles")](profiles-images/image47.png#lightbox)
9. Das Profil wird ebenfalls im **Hilfsprogramm für Entwicklerzertifikate** angezeigt, falls es erneut heruntergeladen werden muss:

    [![](profiles-images/image48.png "The Developer Certificate Utility")](profiles-images/image48.png#lightbox)

Ein neues Entwicklungsbereitstellungsprofil muss für jede neue App oder jeden neuen Computer, der für das Testen hinzugefügt wird, erstellt werden.

## <a name="production-provisioning-profile"></a>Produktionsbereitstellungsprofil

Produktionsbereitstellungsprofile sind erforderlich, um ein Paket für die Übermittlung an den Mac App Store zu erstellen.

Führen Sie folgende Schritte aus:

1. Wählen Sie die Art des Profils aus, das Sie erstellen möchten, und klicken Sie auf die Schaltfläche **Weiter**:

    [![](profiles-images/certif21.png "Selecting the type of profile")](profiles-images/certif21.png#lightbox)
2. Wählen Sie die ID der App aus, für die das Profil erstellt werden soll, und klicken Sie auf die Schaltfläche **Weiter**:

    [![](profiles-images/certif15.png "Selecting the app ID")](profiles-images/certif15.png#lightbox)
3. Wählen Sie die Unternehmens-ID aus, mit der das Profil signiert werden soll, und klicken Sie auf die Schaltfläche **Weiter**:

    [![](profiles-images/certif23.png "Selecting the company ID")](profiles-images/certif23.png#lightbox)
4. Geben Sie einen **Profilnamen** ein, und klicken Sie auf die Schaltfläche **Generieren**:

    [![](profiles-images/certif24.png "Generating the profile")](profiles-images/certif24.png#lightbox)
5. Klicken Sie auf **Herunterladen**, um die Datei (Erweiterung `.provisionprofile`) des Bereitstellungsprofils zu erhalten:

    [![](profiles-images/certif25.png "Downloading the profile")](profiles-images/certif25.png#lightbox)
6. Ziehen Sie diese in den **Xcode-Organisator**, oder doppelklicken Sie darauf, um sie zu installieren. Das Profil wird dann im Xcode-Organisator angezeigt:

    [![](profiles-images/image51.png "Installing the profile")](profiles-images/image51.png#lightbox)
7. Das Bereitstellungsprofil wird ebenfalls in der Liste angezeigt:

    [![](profiles-images/certif26.png "Showing the installed profiles")](profiles-images/certif26.png#lightbox)

Wenn der Entwickler die von einer App-ID verwendeten Funktionen ändert (z.B. Aktivieren von iCloud oder Pushbenachrichtigungen), sollten die Bereitstellungsprofile für diese App-ID erneut erstellt werden.

## <a name="related-links"></a>Verwandte Links

- [Installation](~//mac/get-started/installation.md)
- [„Hallo, Mac“-Beispiel](~//mac/get-started/hello-mac.md)
- [Verteilen Ihrer Apps im Mac App Store](https://developer.apple.com/devcenter/mac/checklist/)
- [Tool-Leitfaden: Codesignieren Ihrer App](https://developer.apple.com/library/mac/#documentation/ToolsLanguages/Conceptual/OSXWorkflowGuide/CodeSigning/CodeSigning.html)
- [Developer ID and GateKeeper (Entwickler-ID und Gatekeeper)](https://developer.apple.com/developer-id/)
