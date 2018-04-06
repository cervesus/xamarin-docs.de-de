---
title: Signieren mit einer Entwickler-ID
description: Dieser Leitfaden erläutert die Signierung einer Xamarin.Mac-App mit der Entwickler-ID für die Veröffentlichung.
ms.prod: xamarin
ms.assetid: cf7b733b-e08f-4f56-a233-264b29ee4c97
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 1a2726ec46ac51ae9848b318798afba74183360c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="sign-with-developer-id"></a>Signieren mit einer Entwickler-ID

Wenn der Entwickler eine App direkt an macOS-Benutzer verteilen möchte, empfiehlt Apple, sie mit der Entwickler-ID zu codesignieren, damit sie auf macOS-Systemen mit aktiviertem **GateKeeper** installiert werden kann. Wenn die App nicht signiert wurde, hindert **GateKeeper** Benutzer mithilfe einer Warnmeldung an der Installation. Dies kann umgangen werden, indem Sie die Taste STRG während des Starts gedrückt halten.

Auf der Website von Apple erfahren Sie mehr über die [Entwickler-ID und Gatekeeper](https://developer.apple.com/resources/developer-id/) und das [Verteilen außerhalb des Mac App Stores](https://developer.apple.com/library/content/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html).

## <a name="code-signing-options"></a>Optionen für das Codesignieren

Um eine App zu erstellen, die direkt und NICHT über den Mac App Store an Benutzer verteilt werden soll, legen Sie **Signing Settings** auf **Developer ID** fest. Achten Sie darauf, die Konfiguration **Release** zu bearbeiten.

 [![](signing-images/config02.png "Die Mac-Signaturoptionen")](signing-images/config02.png#lightbox)


## <a name="build"></a>Build

Stellen Sie vor dem Erstellen sicher, dass die richtige Konfiguration ausgewählt ist, und erstellen Sie ein Installationspaket in den Einstellungen **Mac Build**:

[![](signing-images/config03.png "Die Buildoptionen")](signing-images/config03.png#lightbox)

Während der Erstellung der App wird der Entwickler aufgefordert, beide Zertifikate zu verwenden:

 [![](signing-images/image57.png "Zulassen des Keychain-Zugriffs")](signing-images/image57.png#lightbox)

 [![](signing-images/image58.png "Zulassen des Keychain-Zugriffs")](signing-images/image58.png#lightbox)

Nachdem die Anwendung erstellt wurde, kann der Entwickler mit der rechten Maustaste auf das Projekt klicken und **Enthaltenden Ordner öffnen** auswählen, um im Verzeichnis `bin/Release` nach der Paketdatei zu suchen. Diese Paketdatei enthält einen Installer für die Anwendung, damit sie für die Installation an jeden macOS-Benutzer verteilt werden kann.

 [![](signing-images/image59.png "Auswählen des App-Pakets in Finder")](signing-images/image59.png#lightbox)

## <a name="related-links"></a>Verwandte Links

- [Installation](~//mac/get-started/installation.md)
- [„Hallo, Mac“-Beispiel](~//mac/get-started/hello-mac.md)
- [Verteilen Ihrer Apps im Mac App Store](https://developer.apple.com/devcenter/mac/checklist/)
- [Tools Guide : Code Signing Your App (Tool-Leitfaden: Codesignieren Ihrer App)](https://developer.apple.com/library/mac/#documentation/ToolsLanguages/Conceptual/OSXWorkflowGuide/CodeSigning/CodeSigning.html)
- [Developer ID and GateKeeper (Entwickler-ID und Gatekeeper)](https://developer.apple.com/resources/developer-id/)
