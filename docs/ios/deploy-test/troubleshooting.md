---
title: 'Tests und Bereitstellung für Xamarin.iOS: Problembehandlung'
description: Dieses Dokument enthält Tipps zur Problembehandlung beim Codesignieren und Bereitstellen, bei TestFlight und beim Kopieren des iOS-App-Bündels vom Mac-Buildhost zu Windows.
ms.prod: xamarin
ms.assetid: 65286D09-F74D-4F22-B6CD-D1BCD7FC7992
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/23/2017
ms.openlocfilehash: 1c8eddcf16c8513852c21babf34d81c9a3290406
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028438"
---
# <a name="xamarinios-testing-and-deployment---troubleshooting"></a>Tests und Bereitstellung für Xamarin.iOS: Problembehandlung

## <a name="code-signing--provisioning"></a>Hinzufügen von Codesignaturen und Bereitstellen von Code

Das Hinzufügen von Codesignaturen und Bereitstellen von Code mit iOS kann mühsam sein. Es ist daher wichtig, sicherzustellen, dass Codesignaturzertifikate und Bereitstellungsprofile richtig konfiguriert sind.

- Große Entwicklerteams sollten nicht die im Folgenden dargestellte Schaltfläche „Fix issue“ (Problem beheben) in Xcode verwenden:

    [![](troubleshooting-images/fixissue.png "The Fix Issues dialog")](troubleshooting-images/fixissue.png#lightbox)

    Über diese Schaltfläche werden neue Bereitstellungsprofile und Zertifikate erstellt. Bestenfalls wird immer dann ein Bereitstellungsprofil erstellt, wenn ein Teammitglied auf die Schaltfläche klickt, was lediglich zu einer unübersichtlichen Verwaltung von Profilen führt. Im schlimmsten Fall werden jedoch die Zertifikate aller Personen im Unternehmen widerrufen, sodass deren Apps nicht mehr funktionieren.

- Behalten Sie beim Keychain-Zugriff den Überblick, und löschen Sie abgelaufene Zertifikate und Profile. Enterprise-Zertifikate sind drei Jahre lang gültig, während andere Zertifikate bereits nach einem Jahr ablaufen. Zertifikate können nicht erneuert werden. Es müssen daher kurz vor Ablauf der alten Zertifikate neue erstellt werden. Achten Sie darauf, alte Zertifikate zu widerrufen und zu löschen und Apps mit neuen Zertifikaten erneut zu signieren.

- Entfernen Sie beim Installieren der neuen Bereitstellungsprofile die alten Profile. Dies bedeutet, dass Visual Studio für Mac nicht entscheiden muss, welches Profil verwendet werden soll. Löschen Sie hierzu zunächst das Profil im Apple Developer Center, und navigieren Sie anschließend zu *Preferences > Your Account > View Details...* (Einstellungen > Ihr Konto > Details anzeigen... ). Wählen Sie das Bereitstellungsprofil aus, und klicken Sie auf **Show in Finder** (Im Finder anzeigen). Dadurch wird der Speicherort des Profils im Mac-Dateisystem angezeigt. Anschließend kann es mit dem Finder gelöscht werden.

- Stellen Sie sicher, dass alle erforderlichen Zertifikate und die zugehörigen privaten Schlüssel verfügbar sind. Jedes Team benötigt ein Entwicklerzertifikat zur Installation von Apps auf eigenen Geräten und ein Verteilungszertifikat zur Installation auf anderen Geräten.

- Starten Sie Xcode und Visual Studio für Mac/Visual Studio neu, wenn ein neues Bereitstellungsprofil oder Zertifikat installiert wird.

## <a name="testflight"></a>TestFlight

In manchen Fällen verlaufen Tests nicht problemlos.  Mit den folgenden Schritten können Sie Probleme im Zusammenhang mit TestFlight lösen:

- TestFlight ist nur für Apps für iOS 8 und höher verfügbar.

- Es muss ein *App Store-Verteilungsprofil* mit Betaberechtigung vorliegen.

- Im Fenster **New iOS App submission** (Neue iOS-App-Einsendung) müssen die gleichen Informationen wie in der Datei **Info.plist** enthalten sein. Außerdem müssen alle Abschnitte ausgefüllt werden. Vor dem Upload auf TestFlight müssen für die App Symbole festgelegt werden.

- Der Upload eines neuen Builds dauert ca. ein bis fünf Minuten. Anschließend wird der Build in iTunes Connect angezeigt.

- Die Option [TestFlight Beta Test](~/ios/deploy-test/testflight.md#beta-testing) (TestFlight-Betatest) muss für jede Version der Anwendung mit dem Schalter aktiviert werden.

- Jedes Mitglied des Entwicklerteams, das gleichzeitig interner Tester ist, muss die Option **Internal Test** (Interner Tester) mit dem Schalter aktivieren.

- Benutzer, die einem iTunes Connect-Konto zugeordnet sind oder ein anderes iTunes Connect-Konto besitzen, können nicht interne Tester sein. Diese Benutzer können nur als externe Tester hinzugefügt werden.

- Das Hinzufügen, Auswählen und Einladen von internen und externen Benutzern erfolgt durch unterschiedliche Schritte. Jede Liste muss separat verwaltet werden.

- Apple muss jeden Build genehmigen, der an externe Tester verteilt werden soll. Wenn sich die Version eines Builds ändert, ist ein neuer Betareview durch Apple erforderlich. Wenn sich die Buildnummer ändert, ist der Review optional.

- Builds, die an externe Tester verteilt werden, müssen mit Metadaten ergänzt werden. Klicken Sie hierzu auf die Buildnummer in **My Apps > Prerelease** (Meine Apps > Prerelease).

- Pro Tag können höchstens zwei Builds für einen Review eingereicht werden. Da eine Versionsänderung einen Review erzwingt, können Versionsnummern maximal zweimal pro Tag geändert werden.

<a name="Automatically_copy_app_bundles_back_to_Windows" />

## <a name="automatically-copy-app-bundles-back-to-windows"></a>Automatisches Zurückkopieren von App Bundles nach Windows

[!include[](~/ios/includes/copy-app-bundle-to-windows.md)]
