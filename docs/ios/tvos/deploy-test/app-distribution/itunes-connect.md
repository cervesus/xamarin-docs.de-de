---
title: Konfigurieren Ihrer tvOS-App in iTunes Connect
description: Dieser Artikel bietet eine zusätzliche Anleitung für den iOS-Konfigurieren der App in iTunes Connect für den speziellen Konfigurationen für TvOS.
ms.prod: xamarin
ms.assetid: 86C7C5BD-C97D-4F1D-B611-A7694557BFDF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 3f4ef00cfe990de2d5afd461d7a110d32bc4a236
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61413237"
---
# <a name="configure-your-tvos-app-in-itunes-connect"></a>Konfigurieren Ihrer tvOS-App in iTunes Connect

_Dieser Artikel bietet eine zusätzliche Anleitung für den iOS-Konfigurieren der App in iTunes Connect für den speziellen Konfigurationen für TvOS._


Zusätzlich zu den Konfigurationen und die Einstellung, die Sie vornehmen, indem Sie die folgenden iOS müssen [Konfigurieren Ihrer App in iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) Guide, dieses Dokument behandelt die speziellen Konfigurationen, die erforderlich sind, um ein Xamarin.tvOS-release in der Apple TV App Store-App.

<a name="Adding-a-tvOS-Release-Version" />

## <a name="adding-a-tvos-release-version"></a>Hinzufügen einer TvOS-Releaseversion

Ob Sie erstellen eine neue App auf die Apple TV App Store veröffentlicht werden, oder Hinzufügen von Apple TV-Unterstützung zu einer vorhandenen iOS-app aus, Sie eines iTunes Connect-Eintrag erstellt und konfiguriert müssen werden mit dem folgenden iOS-spezifischen geführt:

- [Erstellen eines iTunes Connect-Datensatzes](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#creating)
- [Verwalten von App-Videos und -Screenshots](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#managing)
- [Verwalten des Namens, der Felder „Description“ (Beschreibung) und „What's New“ (Neues in dieser Version) sowie der Schlüsselwörter und URLs](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#metadata)
- [Verwalten von allgemeinen Informationen](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#general)

Optional können Sie auch benötigen:

- [Verwalten von Informationen im Game Center](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#game-center)
- [Verwalten des Bereichs „In-App Purchases“ (In-App-Käufe)](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#iap)

Alle oben genannten Schritte abgeschlossen sind öffnen Sie iTunes Connect-Datensatz und wählen Sie zum Hinzufügen der TvOS-Unterstützung, die mit der linken Randleiste von Ihrer app:

[![](itunes-connect-images/connect01.png "Fügen Sie TvOS-Unterstützung, die mit der linken Randleiste hinzu")](itunes-connect-images/connect01.png#lightbox)

Die Bildschirme der TvOS-spezifische Informationen werden für den angegebenen iTunes Connect-Datensatz verfügbar:

[![](itunes-connect-images/connect02.png "Der Bildschirm TvOS-spezifische Informationen")](itunes-connect-images/connect02.png#lightbox)

<a name="tvOS-Version-Information" />

## <a name="tvos-version-information"></a>TvOS-Versionsinformationen

Wählen Sie in der linken Randleiste **1.0 Vorbereiten für die Übermittlung** Abschnitt TvOS-APP:

[![](itunes-connect-images/connect03.png "TvOS-Versionsinformationen")](itunes-connect-images/connect03.png#lightbox)

Geben Sie auf diesem Bildschirm die folgenden Informationen ein:

- Die erforderlichen Screenshots, Beschreibung, Schlüsselwörter und URLs.
- Allgemeine App-Informationen wie z. B. die Versionsnummer, Copyrightinformationen und Altersfreigabe.
- Optionale In-App-Käufe.
- Optional Game Center-Unterstützung mit Leaderboards und Erfolge.
- Erforderliche Prüfung der App-Informationen, z. B. Wenden Sie sich an, Demokonten und Hinweise an.

Nachdem Sie die erforderliche Informationen eingegeben haben, klicken Sie auf die **speichern** -Schaltfläche in der oberen rechten Ecke des Bildschirms, um die Änderungen zu speichern:

[![](itunes-connect-images/connect04.png "TvOS-Versionsinformationen, die für die Übermittlung bereit")](itunes-connect-images/connect04.png#lightbox)

<a name="Submitting-for-Review" />

## <a name="preparing-to-submit-for-review"></a>Vorbereiten der zur Überprüfung übermitteln

Wenn Sie schließlich Ihre Xamarin.tvOS-app an das Apple TV App Store zur Überprüfung übermitteln möchten, kehren Sie zu der app iTunes Connect-Datensatz zurück, und klicken Sie auf die **zur Überprüfung übermitteln** -Schaltfläche in der oberen rechten Ecke des Bildschirms:

[![](itunes-connect-images/connect05.png "Zur Überprüfung übermitteln")](itunes-connect-images/connect05.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel gegeben haben, Übersicht über die bestimmte TvOS-Einstellung, die in iTunes Connect für eine TvOS-app, um die App-Store von Apple TV-Version erforderlich.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [TvOS Human Interface-Handbücher](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos verwendet.](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
