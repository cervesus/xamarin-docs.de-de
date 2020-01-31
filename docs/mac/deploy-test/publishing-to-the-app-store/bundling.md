---
title: Bündeln für den Mac App Store
description: In diesem Dokument wird das Bündeln einer Xamarin.Mac-App für die Veröffentlichung im Mac App Store beschrieben. Dabei werden Optionen zum Codesignieren und das Erstellen von Code erläutert.
ms.prod: xamarin
ms.assetid: 00a36d7c-937d-4657-bf6a-0de9684b8f94
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: f4d38bb66a34257c1e0a27c5fbbfe16f59743e83
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725505"
---
# <a name="bundling-for-the-mac-app-store"></a>Bündeln für den Mac App Store

In diesem Abschnitt werden die Grundlagen der Erstellung einer Anwendung mit Visual Studio für Mac beschrieben, die im Mac App Store angeboten werden soll. Auf der Grundlage von zusätzlichen Funktionen (wie dem Zugang zu iCloud und Pushbenachrichtigungen) ist möglicherweise zusätzliches Setup notwendig, dessen Beschreibung den Rahmen dieses Artikels sprengen würde.

> [!NOTE]
> Bevor der Entwickler mit den folgenden Anweisungen beginnen kann, muss er zunächst ein sogenanntes Produktionsbereitstellungsprofil erstellen, um eine Anwendung für den Mac App Store entwickeln zu können. Informationen zum Erstellen der erforderlichen Bereitstellungsprofile finden Sie in den [Profilanleitungen](profiles.md).

## <a name="code-signing-options"></a>Optionen zum Codesignieren

Ändern Sie die **Konfiguration** auf **Release**, bevor Sie die Optionen zum Codesignieren und Komprimieren aktualisieren. Sie müssen sicherstellen, dass Sie Ihre **Unternehmensidentität** und das von ihnen erstellte Provisioning-Profil verwenden, wenn Sie die Anwendung im App Store anbieten wollen.

[![Bearbeiten der Optionen zum Codesignieren](bundling-images/sign.png)](bundling-images/sign-large.png#lightbox)

Stellen Sie sicher, dass die Option zur Erstellung eines Installer-Pakets in den **Mac Build**-Einstellungen aktiviert ist:

[![Bearbeiten der Buildoptionen](bundling-images/build.png "Bearbeiten der Buildoptionen")](bundling-images/build-large.png#lightbox)

## <a name="build"></a>Build

Stellen Sie vor dem Erstellen der App sicher, dass Sie die Konfiguration **Release** ausgewählt haben. Beim Erstellen der App wird der Entwickler _zweimal_ aufgefordert (sowohl die Anwendung als auch die Installerzertifikate zu verwenden):

![Zulassen, dass die App das Zertifikat verwendet (wird zweimal angezeigt)](bundling-images/perms02.png)

Wenn die Anwendung erstellt wurde, kann der Entwickler mit der rechten Maustaste auf das Projekt klicken und **Reveal in Finder** (In Finder anzeigen) auswählen, um die Paketdatei (im Verzeichnis `bin/Release/AppStore` wie im Beispiel unten) zu finden.  Diese Paketdatei beinhaltet einen Installer für die App, der an Apple zur Aufnahme in den Mac App Store übermittelt werden kann.

> [!div class="mx-imgBorder"]
> ![Auswählen des Buildpakets in Finder](bundling-images/path.png)

## <a name="related-links"></a>Verwandte Links

- [Installation](/visualstudio/mac/installation/)
- [„Hallo, Mac“-Beispiel](~/mac/get-started/hello-mac.md)
- [Verteilen Ihrer Apps im Mac App Store](https://developer.apple.com/devcenter/mac/checklist/)
- [Developer ID and GateKeeper (Entwickler-ID und Gatekeeper)](https://developer.apple.com/developer-id/)
