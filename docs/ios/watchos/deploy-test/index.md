---
title: Bereitstellen und Testen von watchos-apps mit xamarin
description: In diesem Dokument wird beschrieben, wie Sie mit xamarin erstellten watchos-apps bereitstellen und testen. Es stellt eine Bereitstellungs Prüfliste bereit, erläutert explizite und Platzhalter-App-IDs und berücksichtigt die APP-Gruppen.
ms.prod: xamarin
ms.assetid: 98257399-E9B3-4BAB-9204-0E89117DEA6D
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 4e2ff46174d9dbb9171a470c389ffe301f6d0d60
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84569646"
---
# <a name="deploying-and-testing-watchos-apps-with-xamarin"></a>Bereitstellen und Testen von watchos-apps mit xamarin

## <a name="deployment-checklist"></a>Checkliste für das Bereitstellen von

Unabhängig davon, ob Sie die Bereitstellung für eine Testuhr oder das Hochladen in den App Store ausführen, müssen Sie die Schritte auf dieser Seite ausführen:

- Im **IOS dev Center**:
  - [App-IDs](#App_IDs) wurden erstellt.
  - Konfigurierte [App-Gruppen](#App_Groups) (falls erforderlich).
  - Verteilungs Bereitstellungs Profil (en) erstellt

- In der Projekt Mappe:

  - Überprüfen Sie, ob [Bündel-IDs und Projekt Verweise](~/ios/watchos/get-started/installation.md) festgelegt sind.
  - Überprüfen Sie, ob Ihre Symbole [ordnungsgemäß konfiguriert](~/ios/watchos/app-fundamentals/icons.md)sind.
  - Überprüfen Sie, ob die Bündel Versionsnummern in allen Projekten abgleichen.
  - Konfigurieren Sie die "Berechtigungs **. plist** " für App-Gruppen (falls erforderlich).

- Befolgen Sie dann die Anweisungen für Folgendes:
  - Bereitstellung [für eine Apple Watch zum Testen](~/ios/watchos/deploy-test/device.md)oder
  - [Hochladen in den App Store](~/ios/watchos/deploy-test/appstore.md).

<a name="App_IDs"></a>

## <a name="app-ids"></a>App-IDs

Wie in den [Setup Anweisungen](~/ios/watchos/get-started/installation.md)erläutert, weisen alle drei Projekte in einer Watch-App zugehörige Bündel-IDs auf, wie z. b.:

- Einheitliches xamarin. IOS-Projekt:`com.xamarin.WatchKitCatalog`
- Watchkit-Erweiterungsprojekt-`com.xamarin.WatchKitCatalog.watchkitextension`
- App-Projekt überwachen:`com.xamarin.WatchKitCatalog.watchkitapp`

Alle drei Projekte erfordern ein entsprechendes Verteilungs Bereitstellungs Profil, das entweder explizit App-IDs für jede oder eine Platzhalter-APP-ID verwendet.

### <a name="explicit-app-ids"></a>Explizite App-IDs

Erstellen Sie eine **App-ID** für die Paket-ID jedes Projekts (die im IOS dev Center wie folgt aussehen soll):

![Die Bündel-IDs im IOS dev Center](images/appids-specific-sml.png)

Beachten Sie beim Erstellen oder Konfigurieren von App-IDs, dass Sie die für Ihre APP erforderlichen Features aktivieren. Dies kann Pushbenachrichtigungen und App-Gruppen umfassen.

Sie müssen ein Verteilungs Bereitstellungs Profil für jede APP-ID erstellen.

### <a name="wildcard-app-id"></a>Platzhalter APP-ID

Alternativ können Sie eine Platzhalter- **App-ID** erstellen, die allen drei Projekten entspricht, z `com.xamarin.*` . b..

Beachten Sie, dass einige Funktionen nicht mit einer Platzhalter-APP-ID (z. b. Pushbenachrichtigungen) verwendet werden können. Wenn Ihre APP diese Features erfordert, sollten Sie explizite App-IDs erstellen.

Für die Verteilung müssen Sie nur ein Verteilungs Bereitstellungs Profil für die Platzhalter-APP-ID erstellen.

<a name="App_Groups"></a>

## <a name="app-groups"></a>App-Gruppen

Sie können eine APP-Gruppe zum Freigeben von Daten zwischen Ihrer IOS-APP und der Watch-Erweiterung verwenden. Stellen Sie sicher, dass die Lösung über Folgendes verfügt:

- Die **App-Gruppe** wurde im Abschnitt Zertifikate, Bezeichner **& profile** des Apple-Entwickler Portals konfiguriert.

- Aktivierte **App-Gruppen** (und die **App-Gruppen-ID**) in der IOS-APP und der **App-ID** der Überwachungs Erweiterung und der Datei " *both* **Berechtigungen. plist**".

### <a name="certificates-identifiers--profiles"></a>Zertifikate, Bezeichner und Profile

Um eine APP-Gruppe zu verwenden, erstellen Sie auf dem Bildschirm **App-Gruppen** einen Eintrag. Im folgenden Beispiel wird die Gruppe mit dem gleichen Reverse-DNS-Stil benannt, der häufig für App-IDs verwendet wird, jedoch mit dem `group.` Präfix (das erforderlich ist):

![Der Bezeichner](images/appgroups-new-sml.png)

Die APP-Gruppe wird dann in der Liste angezeigt:

![Die bezeichnerliste](images/appgroups-setup-sml.png)

Nachdem die Gruppe erstellt wurde, kann in Ihrer **App-ID** -Konfiguration auf Sie verwiesen werden. Beachten Sie, dass Sie sowohl die **App-IDs**der IOS-App als auch die Watch-Erweiterung

![Verfügbare Konfigurationen](images/appgroups-sml.png)

Aktivieren Sie App-Gruppen **nicht** in der Apple Watch APP-ID. Es ist nicht erforderlich, auf der Überwachung selbst aktiviert zu werden.

### <a name="entitlementsplist"></a>Entitlements.plist

Einige App-Features (z. b. App-Gruppen) erfordert, dass Sie Ihre Berechtigungen festlegen.
Doppelklicken Sie, um die **Datei "** Berechtigungsdatei" in diesen Projekten zu bearbeiten:

- IOS-App-Projekt
- Überwachungs Erweiterungsprojekt

.![Der Berechtigungen. plist-Editor](images/entitlements-plist-sml.png)

Aktivieren Sie **keine** Berechtigungen im Überwachungs-App-Projekt. Es ist nicht erforderlich, auf der Überwachung selbst aktiviert zu werden.

## <a name="related-links"></a>Verwandte Links

- [Leitfaden für die Apple watchkit-Übermittlung](https://developer.apple.com/app-store/watch/)
