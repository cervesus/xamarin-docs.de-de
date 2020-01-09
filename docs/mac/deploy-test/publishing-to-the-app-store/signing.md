---
title: Signieren von Xamarin.Mac-Apps mit einer Entwickler-ID
description: In diesem Dokument wird beschrieben, wie eine Xamarin.Mac-App mit einer Entwickler-ID signiert werden kann, um diese außerhalb des Mac App Store zu verteilen. Dabei werden Optionen zum Codesignieren und das Erstellen von Code erläutert.
ms.prod: xamarin
ms.assetid: cf7b733b-e08f-4f56-a233-264b29ee4c97
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: de858049ef899134d61596cabfcf81fca973e8b0
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75489348"
---
# <a name="signing-xamarinmac-apps-with-a-developer-id"></a>Signieren von Xamarin.Mac-Apps mit einer Entwickler-ID

Wenn der Entwickler eine App direkt an macOS-Benutzer verteilen möchte, empfiehlt Apple, sie mit der Entwickler-ID zu codesignieren, damit sie auf macOS-Systemen mit aktiviertem **GateKeeper** installiert werden kann. Wenn die App nicht signiert wurde, hindert **GateKeeper** Benutzer mithilfe einer Warnmeldung an der Installation. Dies kann umgangen werden, indem Sie die Taste STRG während des Starts gedrückt halten.

Auf der Website von Apple erfahren Sie mehr über die [Entwickler-ID und Gatekeeper](https://developer.apple.com/resources/developer-id/) und das [Verteilen außerhalb des Mac App Stores](https://developer.apple.com/library/content/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html).

## <a name="code-signing-options"></a>Optionen zum Codesignieren

Um eine App zu erstellen, die direkt und NICHT über den Mac App Store an Benutzer verteilt werden soll, legen Sie **Signing Settings** auf **Developer ID** fest. Achten Sie darauf, die Konfiguration **Release** zu bearbeiten.

 [![](signing-images/config02.png "The Mac Signing options")](signing-images/config02.png#lightbox)

## <a name="build"></a>Build

Stellen Sie vor dem Erstellen sicher, dass die richtige Konfiguration ausgewählt ist, und erstellen Sie ein Installationspaket in den Einstellungen **Mac Build**:

[![](signing-images/config03.png "The build options")](signing-images/config03.png#lightbox)

Während der Erstellung der App wird der Entwickler aufgefordert, beide Zertifikate zu verwenden:

 [![](signing-images/image57.png "Allowing keychain access")](signing-images/image57.png#lightbox)

 [![](signing-images/image58.png "Allowing keychain access")](signing-images/image58.png#lightbox)

Nachdem die Anwendung erstellt wurde, kann der Entwickler mit der rechten Maustaste auf das Projekt klicken und **Enthaltenden Ordner öffnen** auswählen, um im Verzeichnis `bin/Release` nach der Paketdatei zu suchen. Diese Paketdatei enthält einen Installer für die Anwendung, damit sie für die Installation an jeden macOS-Benutzer verteilt werden kann.

 [![](signing-images/image59.png "Selecting the app package in Finder")](signing-images/image59.png#lightbox)

## <a name="related-links"></a>Verwandte Links

- [Installation](~//mac/get-started/installation.md)
- [„Hallo, Mac“-Beispiel](~//mac/get-started/hello-mac.md)
- [Verteilen Ihrer Apps im Mac App Store](https://developer.apple.com/devcenter/mac/checklist/)
- [Tool-Leitfaden: Codesignieren Ihrer App](https://developer.apple.com/library/mac/#documentation/ToolsLanguages/Conceptual/OSXWorkflowGuide/CodeSigning/CodeSigning.html)
- [Developer ID and GateKeeper (Entwickler-ID und Gatekeeper)](https://developer.apple.com/resources/developer-id/)
