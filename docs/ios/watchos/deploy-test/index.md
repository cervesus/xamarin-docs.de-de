---
title: Bereitstellen und testen die WatchOS-Apps mit Xamarin
description: Dieses Dokument beschreibt das Bereitstellen und testen die WatchOS-apps mit Xamarin erstellt wurde. Es enthält eine Checkliste, explizite erläutert und die Platzhalter-app-IDs, und wirft einen Blick auf die app-Gruppen.
ms.prod: xamarin
ms.assetid: 98257399-E9B3-4BAB-9204-0E89117DEA6D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 7d626b8a968835813d87c93e3cead57a00c14000
ms.sourcegitcommit: 849bf6d1c67df943482ebf3c80c456a48eda1e21
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51528597"
---
# <a name="deploying-and-testing-watchos-apps-with-xamarin"></a>Bereitstellen und testen die WatchOS-Apps mit Xamarin

## <a name="deployment-checklist"></a>Checkliste für die Bereitstellung

Wenn Sie zu einem Test Watch bereitstellen, oder in den App Store hochladen, müssen Sie die Schritte auf dieser Seite:

- In der **iOS Developer Center**:
  - [App-IDs](#App_IDs) erstellt wurden.
  - [App-Gruppen](#App_Groups) konfiguriert (falls erforderlich).
  - Verteilung Provisioning Profile erstellt

- In der Projektmappe:

  - Überprüfen Sie die [Bündel-IDs und Projektverweise](~/ios/watchos/get-started/installation.md) festgelegt sind.
  - Überprüfen Sie die Symbole sind [korrekt](~/ios/watchos/app-fundamentals/icons.md).
  - Überprüfen Sie die Paket-versionsübereinstimmung Zahlen in allen Projekten.
  - Konfigurieren der **"Entitlements.plist"** für App-Gruppen (falls erforderlich).

* Klicken Sie dann wie folgt:
  - [Bereitstellen auf einer Apple Watch zum Testen der](~/ios/watchos/deploy-test/device.md), oder
  - [In den App Store hochladen](~/ios/watchos/deploy-test/appstore.md).

<a name="App_IDs"/>

## <a name="app-ids"></a>App-IDs

Siehe die [Anweisungen zur Einrichtung des](~/ios/watchos/get-started/installation.md), über alle drei Projekte in eine Watch-App-Bündel-IDs, wie z. B. verwandte:

- Vereinheitlichten Xamarin.iOS-Projekt- `com.xamarin.WatchKitCatalog`
- WatchKit-Erweiterung-Projekt – `com.xamarin.WatchKitCatalog.watchkitextension`
- Watch-App-Projekt: `com.xamarin.WatchKitCatalog.watchkitapp`

Alle drei Projekte erfordern, einen übereinstimmenden Verteilungsbereitstellungsprofil, entweder explizite Verwendung von App-IDs für jede oder einen Platzhalter-App-ID

### <a name="explicit-app-ids"></a>Explizite App-IDs

Erstellen Sie eine **App-ID** eines jeden Projekts-Paket-ID (die auf dem iOS Developer Center aussieht):

![Die Paket-IDs in der iOS Developer Center](images/appids-specific-sml.png)

Beim Erstellen oder Konfigurieren von App-IDs, denken Sie daran, die bestimmten Funktionen aktivieren, die Ihre app benötigt. Dies kann es sich um erste Schritte mit Pushbenachrichtigungen und app-Gruppen enthalten.

Sie benötigen zum Erstellen eines Bereitstellungsprofils für die Verteilung für jeden App-ID.

### <a name="wildcard-app-id"></a>Platzhalter-App-ID

Alternativ können Sie einen Platzhalter erstellen **App-ID** entspricht z. B. alle drei Projekte `com.xamarin.*`.

Beachten Sie, dass einige Funktionen, die mit einem Platzhalter-App-ID (z.B. Pushbenachrichtigungen) verwendet werden können. Wenn Ihre app diese Funktionen sollten Sie explizite App-IDs erstellen benötigt.

Für die Verteilung müssen Sie nur eine Verteilungsbereitstellungsprofil für den Platzhalter-App-ID erstellen

<a name="App_Groups" />

## <a name="app-groups"></a>App-Gruppen

Sie können eine App-Gruppe verwenden, zum Freigeben von Daten zwischen Ihrer iOS-App und der Watch-Erweiterung. Sie sollten sicherstellen, dass Ihre Projektmappe enthält:

- Konfiguriert die **App-Gruppe** im Apple Developer Portal **Zertifikate, Bezeichner & Profile** Abschnitt.

- Aktiviert **App-Gruppen** (und die **App-Gruppen-ID**) in *sowohl* iOS-App und der Watch-Erweiterung **App-ID** und  **"Entitlements.plist"**.

### <a name="certificates-identifiers--profiles"></a>Zertifikate, Bezeichner & Profile

Um eine App-Gruppe zu verwenden, erstellen Sie einen Eintrag in der **App-Gruppen** Bildschirm. Im folgenden Beispiel heißt die Gruppe mit dem gleichen Reverse-DNS-Stil, die häufig verwendet wird, für die App-IDs, jedoch mit der `group.` -Präfix (das erforderlich ist):

![Der Bezeichner](images/appgroups-new-sml.png)

Die app-Gruppe wird dann in der Liste angezeigt:

![Der Bezeichner-Liste](images/appgroups-setup-sml.png)

Nachdem die Gruppe erstellt wurde, kann es in verwiesen werden Ihre **App-ID** Konfiguration. Denken Sie daran, die sie im iOS-App "und" Watch-Erweiterung enthalten **App-IDs**.

![Konfigurationen für Verfügbarkeit](images/appgroups-sml.png)

Führen Sie **nicht** Aktivieren von App-Gruppen in der Apple Watch-App-ID Es ist nicht erforderlich, um auf die Überwachung selbst aktiviert werden.

### <a name="entitlementsplist"></a>Entitlements.plist

Einige app-Funktionen (z. b. App-Gruppen) müssen Sie Ihre Berechtigungen festlegen.
Doppelklicken Sie zum Bearbeiten der **"Entitlements.plist"** Datei in diesen Projekten:

- iOS-App-Projekt
- Sehen Sie sich das Projekt

sein.![Die Entitlements.plist-editor](images/entitlements-plist-sml.png)

Führen Sie **nicht** aktivieren Sie Berechtigungen in der Watch-App-Projekt. Es ist nicht erforderlich, um auf die Überwachung selbst aktiviert werden.

## <a name="related-links"></a>Verwandte Links

- [Apple WatchKit-Übermittlung-Handbuch](https://developer.apple.com/app-store/watch/)
