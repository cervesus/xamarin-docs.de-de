---
title: Bereitstellen und Testen von WatchOS-Apps mit Xamarin
description: Dieses Dokument beschreibt das Bereitstellen und Testen von WatchOS-apps mit Xamarin erstellten. Es bietet eine Checkliste für die Bereitstellung, explizite erläutert und die Platzhalter-app-IDs und untersucht, mit der app-Gruppen.
ms.prod: xamarin
ms.assetid: 98257399-E9B3-4BAB-9204-0E89117DEA6D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 778583456e74bb7ed3a85dce96bcdbc487aef57a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790941"
---
# <a name="deploying-and-testing-watchos-apps-with-xamarin"></a>Bereitstellen und Testen von WatchOS-Apps mit Xamarin

## <a name="deployment-checklist"></a>Bereitstellungsprüfliste

An, ob Sie zu einem Test Überwachungsfenster bereitstellen, oder auf den App Store hochladen, müssen Sie die Schritte auf dieser Seite:

- In der **iOS Dev Center**:
  - [App-IDs](#App_IDs) erstellt wurden.
  - [App-Gruppen](#App_Groups) konfiguriert (falls erforderlich).
  - Verteilung Provisioning Profile erstellt

- In der Projektmappe:

  - Überprüfen Sie die [-Paket-IDs und Projektverweise](~/ios/watchos/get-started/installation.md) festgelegt sind.
  - Überprüfen Sie die Symbole sind [richtig konfiguriert](~/ios/watchos/app-fundamentals/icons.md).
  - Überprüfen Sie die Paket-Version Zahlen Übereinstimmung in allen Projekten.
  - Konfigurieren der **Entitlements.plist** für App-Gruppen (falls erforderlich).

* Befolgen Sie dann die Anweisungen auf:
  - [Bereitstellen auf einem Apple Watch zu Testzwecken](~/ios/watchos/deploy-test/device.md), oder
  - [In den App Store hochladen](~/ios/watchos/deploy-test/appstore.md).

<a name="App_IDs"/>

## <a name="app-ids"></a>App-IDs

Entsprechend der Anleitung unter dem [setupanweisungen](~/ios/watchos/get-started/installation.md), alle drei Projekte in einer Watch-App-Bündel-IDs, wie z. B. verknüpften zulässig:

- Xamarin.iOS Unified Projekt- `com.xamarin.WatchKitCatalog`
- WatchKit Erweiterungsprojekt- `com.xamarin.WatchKitCatalog.watchkitextension`
- Watch-App-Projekt- `com.xamarin.WatchKitCatalog.watchkitapp`

Alle drei Projekte erfordern ein übereinstimmendes Verteilung Bereitstellungsprofil, entweder mit explizit für jede App-IDs oder einen Platzhalter-ID der App

### <a name="explicit-app-ids"></a>Explizite App-IDs

Erstellen einer **App-ID** jedes einzelnen Projekt Paket-ID (die auf dem iOS Dev Center aussehen wird):

![Die Paket-IDs in der iOS Developer Center](images/appids-specific-sml.png)

Beim Erstellen oder Konfigurieren von App-IDs, denken Sie daran, die bestimmten Funktionen zu aktivieren, die Ihre app benötigt. Dadurch kann es sich um Pushbenachrichtigungen und app-Gruppen enthalten.

Sie benötigen zum Erstellen einer Bereitstellung Verteilungsprofil für jede App-ID.

### <a name="wildcard-app-id"></a>Platzhalter-App-ID

Alternativ können Sie einen Platzhalter erstellen **App-ID** entspricht, die z. B. alle drei Projekte `com.xamarin.*`.

Beachten Sie, dass einige Funktionen nicht mit einem Platzhalter-App-ID (z. B. Pushbenachrichtigungen) verwendet werden können. Falls Ihre app diese Funktionen sollten Sie explizite App-IDs erstellen benötigt.

Für die Verteilung müssen Sie nur eine Bereitstellung Verteilungsprofil für den Platzhalter-ID der App zu erstellen.

<a name="App_Groups" />

## <a name="app-groups"></a>App-Gruppen

Sie können ein App-Gruppe zur Freigabe von Daten zwischen Ihrer iOS-App und der Watch-Erweiterung verwenden. Sie sollten sicherstellen, dass Ihre Projektmappe enthält:

- Konfiguriert die **App-Gruppe** in der Apple-Entwicklerportal **Zertifikate "," Bezeichner "und" Profile** Abschnitt.

- Aktiviert **App-Gruppen** (und die **App-Gruppen-ID**) in *beide* iOS-App und der Watch-Erweiterung **App-ID** und  **Entitlements.plist**.

### <a name="certificates-identifiers--profiles"></a>Zertifikate, Bezeichner und Profile

Um eine App-Gruppe zu verwenden, erstellen Sie einen Eintrag in der **App-Gruppen** Bildschirm. Im folgenden Beispiel wird die Gruppe benannt, mit dem gleichen Reverse-DNS-Stil, die häufig verwendet wird, für die App-IDs, jedoch mit der `group.` Präfix (das ist erforderlich):

![Der Bezeichner](images/appgroups-new-sml.png)

Die app-Gruppe wird dann in der Liste angezeigt:

![Der Bezeichner-Liste](images/appgroups-setup-sml.png)

Nachdem die Gruppe erstellt wurde, kann es in verwiesen werden Ihre **App-ID** Konfiguration. Denken Sie daran, die sie der iOS-App und Watch-Erweiterung enthalten **App-IDs**.

![Verfügbare consifurations](images/appgroups-sml.png)

Führen Sie **nicht** Aktivieren der App-Gruppen in der Apple Watch-App-ID Es ist nicht erforderlich, um die Überwachung selbst aktiviert sein.

### <a name="entitlementsplist"></a>Entitlements.plist

Einige app-Funktionen (z. b. App-Gruppen) müssen Sie Ihre Berechtigungen festlegen.
Doppelklicken Sie zum Bearbeiten der **Entitlements.plist** Datei in diesen Projekten:

- iOS-App-Projekt
- Überwachen-Erweiterungsprojekt

.![Der Editor Entitlements.plist](images/entitlements-plist-sml.png)

Führen Sie **nicht** aktivieren Sie Berechtigungen in das Watch-App-Projekt. Es ist nicht erforderlich, um die Überwachung selbst aktiviert sein.

## <a name="related-links"></a>Verwandte Links

- [Apple WatchKit Übermittlung-Handbuch](https://developer.apple.com/app-store/watch/)
