---
title: Ihre tvos. außerdem wurden App im iTunes Connect konfigurieren
description: Dieser Artikel bietet eine zusätzliche Anleitung für den iOS-Konfigurieren der App im iTunes Connect für tvos. außerdem wurden bestimmte Konfigurationen.
ms.prod: xamarin
ms.assetid: 86C7C5BD-C97D-4F1D-B611-A7694557BFDF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: d082a980572349da1b7e6155b3aa4de41512796f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30779825"
---
# <a name="configure-your-tvos-app-in-itunes-connect"></a>Ihre tvos. außerdem wurden App im iTunes Connect konfigurieren

_Dieser Artikel bietet eine zusätzliche Anleitung für den iOS-Konfigurieren der App im iTunes Connect für tvos. außerdem wurden bestimmte Konfigurationen._


Zusätzlich zu den Konfigurationen und die Einstellung, die Sie durch das Ausführen von iOS erstellen müssen [Konfigurieren der App im iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) Handbuch, dieses Dokument behandelt die spezifischen Konfigurationen, die zum Freigeben einer Xamarin.tvOS erforderlich sind die App im App Store Apple TV.

<a name="Adding-a-tvOS-Release-Version" />

## <a name="adding-a-tvos-release-version"></a>Hinzufügen einer tvos. außerdem wurden Releaseversion

Ob Sie erstellen eine neue App auf dem Apple TV-App Store veröffentlicht werden soll, oder Hinzufügen von Apple TV-Unterstützung zu einer vorhandenen iOS-app aus, Sie müssen eine iTunes Connect-Eintrag erstellt und konfiguriert haben führt mit den folgenden iOS spezifisch:

- [Erstellen eines iTunes Connect-Datensatzes](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#creating)
- [Verwalten von App-Videos und -Screenshots](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#managing)
- [Verwalten des Namens, der Felder „Description“ (Beschreibung) und „What's New“ (Neues in dieser Version) sowie der Schlüsselwörter und URLs](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#metadata)
- [Verwalten Sie allgemeine Informationen](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#general)

Optional können Sie auch erfordern:

- [Verwalten von Informationen im Game Center](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#game-center)
- [Verwalten des Bereichs „In-App Purchases“ (In-App-Käufe)](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#iap)

Alle oben genannten Schritte abgeschlossen öffnen Sie iTunes Connect Datensatz und wählen Sie aus, mit der linken Randleiste tvos. außerdem wurden-Unterstützung hinzufügen Ihrer app:

[![](itunes-connect-images/connect01.png "Mit der linken Randleiste tvos. außerdem wurden-Unterstützung hinzufügen")](itunes-connect-images/connect01.png#lightbox)

Die Bildschirme des tvos. außerdem wurden bestimmte Informationen können für bestimmte iTunes Connect Datensatz werden:

[![](itunes-connect-images/connect02.png "Bestimmte Informationsbildschirm des tvos. außerdem wurden")](itunes-connect-images/connect02.png#lightbox)

<a name="tvOS-Version-Information" />

## <a name="tvos-version-information"></a>tvos. außerdem wurden Informationen zur Version

Wählen Sie in der Randleiste linken **1.0 Vorbereiten für die Übermittlung** unter dem Abschnitt tvos. außerdem wurden APP:

[![](itunes-connect-images/connect03.png "tvos. außerdem wurden Informationen zur Version")](itunes-connect-images/connect03.png#lightbox)

Geben Sie auf diesem Bildschirm die folgenden Informationen ein:

- Die erforderliche Screenshots, Beschreibung, Schlüsselwörter und URLs.
- Allgemein-App-Informationen wie z. B. Versionsnummer und Copyright mit ALTER-Bewertung.
- Dies ist optional In-App-Käufen.
- Optional Game Center-Unterstützung mit setzen und Erfolge.
- Erforderliche App Prüfen von Informationen wie z. B. Wenden Sie sich an, die Demokonten und Hinweise.

Nachdem Sie die erforderliche Informationen eingegeben haben, klicken Sie auf die **speichern** Schaltfläche in der oberen rechten Ecke des Bildschirms, um die Änderungen zu speichern:

[![](itunes-connect-images/connect04.png "tvos. außerdem wurden Informationen zur Version für die Übermittlung bereit")](itunes-connect-images/connect04.png#lightbox)

<a name="Submitting-for-Review" />

## <a name="preparing-to-submit-for-review"></a>Senden Sie zur Überprüfung wird vorbereitet.

Wenn Sie schließlich zum Übermitteln Ihrer Apps Xamarin.tvOS auf den Apple TV-App-Store zur Überprüfung bereit sind, zurück an die app iTunes Connect-Datensatz, und klicken Sie auf die **zur Überprüfung übermitteln** Schaltfläche in der oberen rechten Ecke des Bildschirms:

[![](itunes-connect-images/connect05.png "Zur Prüfung senden")](itunes-connect-images/connect05.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel habe Überblick über die tvos. außerdem wurden bestimmte Einstellung in iTunes Connect erforderlich, um eine app tvos. außerdem wurden auf den Apple TV-App-Store veröffentlichen.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos. außerdem wurden Handbücher für interaktive Workflowdienste-Schnittstelle](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos. außerdem wurden](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
