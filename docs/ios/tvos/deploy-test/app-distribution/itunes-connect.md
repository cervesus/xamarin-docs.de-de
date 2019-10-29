---
title: Konfigurieren Ihrer tvOS-App in iTunes Connect
description: Dieser Artikel bietet einen ergänzenden Leitfaden zu IOS Konfigurieren ihrer app in iTunes Connect für die tvos-spezifischen Konfigurationen.
ms.prod: xamarin
ms.assetid: 86C7C5BD-C97D-4F1D-B611-A7694557BFDF
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: 01ab48f68656dcabdf2a6cfc286dfcd8850454f8
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030763"
---
# <a name="configure-your-tvos-app-in-itunes-connect"></a>Konfigurieren Ihrer tvOS-App in iTunes Connect

_Dieser Artikel bietet einen ergänzenden Leitfaden zu IOS Konfigurieren ihrer app in iTunes Connect für die tvos-spezifischen Konfigurationen._

Zusätzlich zu den Konfigurationen und Einstellungen, die Sie durch Befolgen der IOS-Anleitung zum [Konfigurieren ihrer app in iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) durchführen müssen, werden in diesem Dokument die spezifischen Konfigurationen erläutert, die erforderlich sind, um eine xamarin. tvos-app in der Apple TV-App freizugeben. Speicher.

<a name="Adding-a-tvOS-Release-Version" />

## <a name="adding-a-tvos-release-version"></a>Hinzufügen einer tvos-Releaseversion

Unabhängig davon, ob Sie eine neue APP erstellen, die im Apple TV App Store veröffentlicht werden soll, oder Apple TV-Unterstützung zu einer vorhandenen IOS-app hinzufügen, müssen Sie einen iTunes Connect-Datensatz erstellt und mit den folgenden IOS-spezifischen Handbüchern konfiguriert haben:

- [Erstellen eines iTunes Connect-Datensatzes](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#creating)
- [Verwalten von App-Videos und -Screenshots](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#managing)
- [Verwalten des Namens, der Felder „Description“ (Beschreibung) und „What's New“ (Neues in dieser Version) sowie der Schlüsselwörter und URLs](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#metadata)
- [Beibehalten allgemeiner Informationen](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#general)

Optional ist auch Folgendes erforderlich:

- [Verwalten von Informationen im Game Center](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#game-center)
- [Verwalten des Bereichs „In-App Purchases“ (In-App-Käufe)](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#iap)

Öffnen Sie nach Abschluss der obigen Schritte den iTunes Connect-Datensatz Ihrer APP, und wählen Sie die Option zum Hinzufügen von tvos mithilfe der linken Rand Leiste aus:

[![](itunes-connect-images/connect01.png "Add tvOS support using the left hand sidebar")](itunes-connect-images/connect01.png#lightbox)

Die tvos-spezifischen Informationsbildschirme sind dann für den angegebenen iTunes Connect-Datensatz verfügbar:

[![](itunes-connect-images/connect02.png "The tvOS specific information screen")](itunes-connect-images/connect02.png#lightbox)

<a name="tvOS-Version-Information" />

## <a name="tvos-version-information"></a>Informationen zur tvos-Version

Wählen Sie in der linken Rand Leiste im Abschnitt "tvos-app" die Option " **1,0 Prepare for Submission** " aus:

[![](itunes-connect-images/connect03.png "tvOS Version Information")](itunes-connect-images/connect03.png#lightbox)

Geben Sie auf diesem Bildschirm die folgenden Informationen an:

- Die erforderlichen Screenshots, Beschreibungen, Schlüsselwörter und URLs.
- Allgemeine app-Informationen, z. b. Versionsnummer, Copyright und Altersbewertung.
- Optionale in-App-Käufe.
- Optionale Game Center Unterstützung mit Bestenlisten und Leistungen.
- Erforderliche App-Überprüfungs Informationen, z. b. Kontakt, Demokonten und Notizen.

Nachdem Sie die erforderlichen Informationen eingegeben haben, klicken Sie in der oberen rechten Ecke des Bildschirms auf die Schaltfläche **Speichern** , um die Änderungen zu speichern:

[![](itunes-connect-images/connect04.png "tvOS Version Information ready for submission")](itunes-connect-images/connect04.png#lightbox)

<a name="Submitting-for-Review" />

## <a name="preparing-to-submit-for-review"></a>Die Übermittlung zur Überprüfung wird vorbereitet.

Wenn Sie Ihre xamarin. tvos-App zur Überprüfung an den Apple TV App Store übermitteln möchten, kehren Sie zum iTunes Connect-Datensatz der APP zurück, und klicken Sie in der oberen rechten Ecke des Bildschirms auf die Schaltfläche **zur Überprüfung einreichen** :

[![](itunes-connect-images/connect05.png "Submit for Review")](itunes-connect-images/connect05.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde eine Übersicht über die in iTunes Connect erforderliche tvos-spezifische Einstellung zum Freigeben einer tvos-App im Apple TV App Store angegeben.

## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Leitfaden zur APP-Programmierung für tvos](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
