---
title: App Store-Änderungen in ios 11
description: In diesem Dokument werden Änderungen am App Store in ios 11 erläutert. Es erläutert das Store-Symbol einer Anwendung, höher gestufte in-App-Käufe, die neu gestaltete Produktseite, die Kundenkommunikation und Phasen Freigaben.
ms.prod: xamarin
ms.assetid: 4A7A03FD-B4F2-4969-8676-A17260730FD6
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/13/2016
ms.openlocfilehash: 356509fb6f588b96a2a1224879675bbad36f8524
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032136"
---
# <a name="app-store-changes-in-ios-11"></a>App Store-Änderungen in ios 11

Der IOS App Store hat eine komplette Umgestaltung vorgenommen, die Benutzern nicht nur die effiziente Navigation im Store ermöglicht, sondern auch als Entwickler die herauf Stufung Ihrer APP zu den Benutzern ermöglicht. Diese Aktionen umfassen Updates für in-App-Käufe und Aktualisierungen der Produktseite. IOS 11 fügt außerdem Updates zum kommunizieren mit Benutzern, zum Hinzufügen des App-Symbols und zum Freigeben der APP auf der Öffentlichkeit hinzu.

![Neues App Store-Layout](app-store-changes-images/image3.jpg)

Der neu gestaltete App Store umfasst die folgenden Abschnitte:

- **Heute** – diese Registerkarte enthält apps, bei denen es sich um eine Auswahl des Editors oder um eine ausgewählte App handelt. Informationen zur herauf Stufung ihrer App finden Sie [auf der Seite](https://developer.apple.com//contact/app-store/promote/) höher stufen.
- **Games** – alle apps, die auf die **Spiel** Kategorie in iTunes Connect festgelegt sind, finden Sie auf dieser Registerkarte.
- **Apps** – diese Registerkarte enthält alle anderen apps. Benutzer können nach vorgestellten Apps, Kategorien, Top Paid/Free suchen.
- **Updates** – diese APP zeigt die Updates für Ihre apps an. Auch wenn Sie eine APP über die [stufenweise](#Phased_Release)Veröffentlichung veröffentlichen, können Benutzer weiterhin auf diese Registerkarte klicken und die neueste Version herunterladen.
- **Suchen** – diese Registerkarte ermöglicht es Benutzern, nach Ihrer APP zu suchen.

## <a name="store-icon"></a>Store-Symbol

Store-Symbole (oder Marketing Symbole) werden nicht mehr in iTunes Connect verwaltet und müssen stattdessen in der APP-Binärdatei als Ressourcen [Katalog](~/ios/app-fundamentals/images-icons/app-icons.md) enthalten sein, ähnlich wie App-Symbole. Ein 1024 x 1024-speichersymbol im PNG-Format muss in einen Asset-Katalog eingeschlossen werden, damit IOS 11-apps erfolgreich übermittelt werden können.

Durch die APP-Verdünnung wird sichergestellt, dass der zusätzliche Ressourcen Katalog die APP-Größe nicht erhöht.

## <a name="in-app-purchases-promoted-in-the-app-store"></a>In-App-Käufe höher gestuft im App Store

Apple hat in-App-Käufe im App Store besser auffallen. Sie können nun bis zu 20 _App Store_ herauf gestuft in-App-Käufe hinzufügen, die Sie jetzt auf der Seite "Apps", auf der Produktseite Ihrer APP oder durchsuchen können.

Damit Ihre in-App-Käufe im App Store angezeigt werden, müssen Sie die folgenden Daten einschließen:

- **Image** – Sie müssen ein speziell entwickeltes Image für das Symbol bereitstellen, das beschreibt, was der in-App-Einkauf durchführt. Dieses Bild muss sich vom App-Symbol unterscheiden und darf kein Screenshot sein.
- **Name** – der Name darf maximal 30 Zeichen umfassen.
- **Beschreibung** – die Beschreibung darf maximal 45 Zeichen umfassen.

Alle in-App-Käufe unterliegen einer App Store-Überprüfung, bevor Sie veröffentlicht werden kann.

Um Ihre in-App-Käufe zur herauf Stufung verfügbar zu machen, öffnen Sie Ihre APP, und navigieren Sie zu **Features > in-App-Käufe**. Wechseln Sie zum Abschnitt App Store-herauf Stufung **(optional)** , und fügen Sie ein 1024 x 1024-Image hinzu, und **Speichern**Sie, wie in der folgenden Abbildung dargestellt:

![App Store-herauf Stufung im Abschnitt "iTune Connect"](app-store-changes-images/image4.png)

Außerdem müssen Sie die `ShouldAddStorePayment`-Methode dem `SKPaymentTransactionObserver`-Protokoll in Ihrer APP hinzufügen.

Weitere Informationen zu in-App-Kaufaktionen finden Sie unter Apple [promoten in-App-Käufe](https://developer.apple.com/app-store/promoting-in-app-purchases/) .

## <a name="redesigned-product-page"></a>Neu gestaltete Produktseite

Die folgenden Änderungen wurden an der Produktseite vorgenommen:

- Titel sind nun auf maximal 30 Zeichen festgelegt.
- Ein Untertitel wurde hinzugefügt.
  - Es sollte eine kurze Zusammenfassung für den Titel sein.
  - Untertitel dürfen höchstens 30 Zeichen lang sein.
- App-Vorschau
  - Sie können jetzt über drei Videos oder Screenshots verfügen.
  - Video automatische Wiedergabe, wenn ein Benutzer die Seite besucht.
  - Weitere Informationen finden Sie auf der [App-Vorschau](https://developer.apple.com/app-store/app-previews/) Seite von Apple.
- Aktions Text
  - Dieses neue Feature stellt 170 Textzeichen bereit, die es Ihnen ermöglichen, häufig geänderte Informationen über Ihre APP zu beschreiben.
  - Sie kann jederzeit aktualisiert werden, ohne dass eine neue Version der APP übermittelt werden muss.

## <a name="customer-communication"></a>Kundenkommunikation

In 10,3 startete Apple eine neue Methode, mit der Entwickler direkt mit App-Benutzern kommunizieren können, indem Sie auf Überprüfungen Antworten. Sie können auf diese neue Funktion in iTunes Connect zugreifen, indem Sie zu **app-> Aktivitäts > Bewertungen und Reviews**navigieren, wie in der folgenden Abbildung veranschaulicht:

![Dialog Feld zum Eingeben der Antwort auf einen Kommentar](app-store-changes-images/image5.png)

Beim Antworten auf die Benutzer sind einige Punkte zu beachten:

- Sie können nur einmal Antworten, aber beide Parteien können Ihre Kommentare beliebig bearbeiten.
- Benutzer erhalten eine Benachrichtigung, wenn Sie auf einen Kommentar Antworten.
- In iTunes Connect wurde eine neue Kundendienst Rolle erstellt. Benutzer mit dieser Rolle oder Administrator Rolle können auf Kommentare Antworten.

Weitere Informationen finden Sie auf der Seite [Antworten auf Reviews](https://developer.apple.com/app-store/responding-to-reviews/) von Apple.

<a name="Phased_Release"/>

## <a name="phased-release"></a>Release in Phasen

Mit IOS 11 hat Apple die Option von Phasen Releases für Updates Ihrer APP implementiert. Sie können stufenweise Releases aktivieren, damit Ihr App-Update nach und nach für Kunden freigegeben werden kann. Dadurch wird sichergestellt, dass die Nachfrage Ihre Produktionsumgebung nicht überlädt.

Stufenweise Releases sind in iTunes Connect aktiviert. Klicken Sie in der Rand Leiste auf Ihre APP, Scrollen Sie im unteren Bereich zum automatische Updates Abschnitt, und wählen Sie unter Verwendung von Phasen Release das **Release Update über einen Zeitraum von sieben Tagen** **aus** :

![Option, die in Phasen Release für automatische Updates angezeigt wird](app-store-changes-images/image6.png)

Ihr Update steht sofort auf der Registerkarte Updates des App Stores zum Download zur Verfügung. Phasen Releases sind nur für Benutzer verfügbar, die automatische Downloads ausgewählt haben.

## <a name="related-links"></a>Verwandte Links

- [Neuerungen in ios 11 (Apple)](https://developer.apple.com/ios/)
- [Aktualisierte App Store-Produktseite (Apple)](https://developer.apple.com/app-store/product-page/)
- [Aktualisieren Ihrer APP für IOS 11 (WWDC) (Video)](https://developer.apple.com/videos/play/wwdc2017/204/)
