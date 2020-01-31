---
title: Konfiguration einer Mac-App
description: In diesem Dokument wird die Konfiguration einer Xamarin.Mac-App für die Veröffentlichung beschrieben. Dabei werden Anwendungseinstellungen, Signatureinstellungen und Buildeinstellungen erläutert.
ms.prod: xamarin
ms.assetid: fea66a34-1581-4cd6-b714-3fbff215a542
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 04/12/2017
ms.openlocfilehash: f008ac42bfffeda2a47ca30aa2991d91f990732f
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725037"
---
# <a name="mac-app-configuration"></a>Konfiguration einer Mac-App

## <a name="mac-app-configuration"></a>Konfiguration einer Mac-App

Klicken Sie in Visual Studio für Mac mit der rechten Maustaste auf das Mac-Anwendungsprojekt, und klicken Sie dann auf **Optionen**.

### <a name="application-settings"></a>Anwendungseinstellungen

Um die Anwendungseinstellung einer Xamarin.Mac-Anwendung zu ändern, doppelklicken Sie auf die **info.plist**-Datei im **Lösungspad**:

![Bearbeiten der Info.plist-Datei](app-configuration-images/config04.png "Bearbeiten der Info.plist-Datei")

Dann werden die für die App verfügbaren Optionen angezeigt:

 [![Bearbeiten der Info.plist-Datei](app-configuration-images/config01.png "Bearbeiten der Info.plist-Datei")](app-configuration-images/config01-large.png#lightbox)

Für das Ausführen von mit Xamarin.Mac erstellten Mac-Anwendungen müssen die folgenden Systemanforderungen erfüllt werden:

- ein Mac-Computer mit Mac OS X 10.7 oder höher

### <a name="signing-settings"></a>Signatureinstellungen

Im Abschnitt **Mac-Signierung** des Dialogfeld **Projektoptionen** kann der Entwickler die Xamarin.Mac-App für die Überprüfung, die eigenständige Veröffentlichung oder die Veröffentlichung über den Apple App Store signieren:

[![Das Fenster „Mac-Signierung“](app-configuration-images/config02.png "Das Fenster „Mac-Signierung“")](app-configuration-images/config02-large.png#lightbox)

Wählen Sie hier die Identität, das Bereitstellungsprofil und alle weiteren benutzerdefinierten Berechtigungen aus, die zum Signieren der App bei deren Kompilierung verwendet werden. Der Entwickler kann den Installer optional signieren, der zum Installieren der App auf einem anderen Mac-Computer verwendet wird.

### <a name="build-settings"></a>Buildeinstellungen

Im Abschnitt **Mac-Build** des Dialogfeld **Projektoptionen** kann der Entwickler die Architektur der Xamarin.Mac-App auswählen, um zu steuern, welche Version von macOS die App unterstützt. Optional kann er ein Installationspaket erstellen, wenn die App erfolgreich kompiliert wurde:

 [![Bearbeiten der Buildeinstellungen](app-configuration-images/config03.png "Bearbeiten der Buildeinstellungen")](app-configuration-images/config03-large.png#lightbox)

## <a name="related-links"></a>Verwandte Links

- [Installation](/visualstudio/mac/installation/)
- [„Hallo, Mac“-Beispiel](~/mac/get-started/hello-mac.md)
- [Verteilen Ihrer Apps im Mac App Store](https://developer.apple.com/devcenter/mac/checklist/)
- [Developer ID and GateKeeper (Entwickler-ID und Gatekeeper)](https://developer.apple.com/developer-id/)
