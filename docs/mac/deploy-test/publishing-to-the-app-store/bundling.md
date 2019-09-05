---
title: Bündeln für den Mac App Store
description: In diesem Dokument wird das Bündeln einer Xamarin.Mac-App für die Veröffentlichung im Mac App Store beschrieben. Dabei werden Optionen zum Codesignieren und das Erstellen von Code erläutert.
ms.prod: xamarin
ms.assetid: 00a36d7c-937d-4657-bf6a-0de9684b8f94
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/14/2017
ms.openlocfilehash: 283a057723ae399ead65c0c3e40347dbbd1e189e
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70281183"
---
# <a name="bundling-for-the-mac-app-store"></a>Bündeln für den Mac App Store

In diesem Abschnitt werden die Grundlagen der Erstellung einer Anwendung mit Visual Studio für Mac beschrieben, die im Mac App Store angeboten werden soll. Auf der Grundlage von zusätzlichen Funktionen (wie dem Zugang zu iCloud und Pushbenachrichtigungen) ist möglicherweise zusätzliches Setup notwendig, dessen Beschreibung den Rahmen dieses Artikels sprengen würde.

> [!NOTE]
> Bevor der Entwickler mit den folgenden Anweisungen beginnen kann, muss er zunächst ein sogenanntes Produktionsbereitstellungsprofil erstellen, um eine Anwendung für den Mac App Store entwickeln zu können. Sehen Sie sich die Anweisungen oben zur Erstellung des erforderlichen Bereitstellungsprofils an.

## <a name="code-signing-options"></a>Optionen zum Codesignieren

Ändern Sie die **Konfiguration** auf **Release**, bevor Sie die Optionen zum Codesignieren und Komprimieren aktualisieren. Sie müssen sicherstellen, dass Sie Ihre **Unternehmensidentität** und das von ihnen erstellte Provisioning-Profil verwenden, wenn Sie die Anwendung im App Store anbieten wollen.

 [![Bearbeiten der Optionen zum Codesignieren](bundling-images/config02.png "Editing the code signing options")](bundling-images/config02-large.png#lightbox)

Stellen Sie sicher, dass die Option zur Erstellung eines Installer-Pakets in den **Mac Build**-Einstellungen aktiviert ist:

[![Bearbeiten der Buildoptionen](bundling-images/config03.png "Editing the build options")](bundling-images/config03-large.png#lightbox)

## <a name="build"></a>Build

Stellen Sie vor dem Erstellen der App sicher, dass Sie die Konfiguration **Release** ausgewählt haben. Beim Erstellen der App wird der Entwickler aufgefordert, beide Zertifikate zu verwenden:

 ![Zulassen, dass die App das Zertifikat verwendet](bundling-images/image62.png "Allowing the app to use the certificate")

 ![Zulassen, dass die App das Zertifikat verwendet](bundling-images/image63.png "Allowing the app to use the certificate")

Wenn die Anwendung erstellt wurde, kann der Entwickler mit der rechten Maustaste auf das Projekt klicken und **Open Containing Folder** (enthaltenen Ordner öffnen) auswählen, um die Paketdatei (im Verzeichnis `bin/x86/AppStore` wie im Beispiel unten) zu finden.  Diese Paketdatei beinhaltet einen Installer für die App, der an Apple zur Aufnahme in den Mac App Store übermittelt werden kann.

 ![Auswählen des Buildpakets im Finder](bundling-images/image64.png "Selecting the build package in Finder")


## <a name="related-links"></a>Verwandte Links

- [Installation](/visualstudio/mac/installation/)
- [„Hallo, Mac“-Beispiel](~/mac/get-started/hello-mac.md)
- [Verteilen Ihrer Apps im Mac App Store](https://developer.apple.com/devcenter/mac/checklist/)
- [Developer ID and GateKeeper (Entwickler-ID und Gatekeeper)](https://developer.apple.com/resources/developer-id/)
