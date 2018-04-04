---
title: Store-App-Änderungen
description: Untersuchen die neuen Funktionen von iOS 11
ms.prod: xamarin
ms.assetid: 4A7A03FD-B4F2-4969-8676-A17260730FD6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 9c9355c047d2e7083ca787f473515163d8e6f5f1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="app-store-changes"></a>Store-App-Änderungen

_Untersuchen die neuen Funktionen von iOS 11_

IOS App Store wurde völlig neu entworfen, die nicht nur Benutzer den Store effizient zu navigieren können, sondern auch, die Sie als Entwickler, um die app Benutzern höher zu stufen. Diese Erweiterungen umfassen mit app-Käufen und Updates auf der Seite "Product". iOS 11 fügt auch Updates hinsichtlich kommunizieren mit Benutzern, das Symbol für Ihre app hinzufügen und Freigeben von Ihrer app für die Öffentlichkeit.

![Das neue Layout für app-store](app-store-changes-images/image3.jpg)

Neu gestaltete app Store enthält die folgenden Abschnitte:

- **Heute** – auf dieser Registerkarte enthält apps, die ein "des Editors Pick" sind oder die gewählte app. Heraufstufen die app Hier geben Sie Informationen auf der [heraufstufen](https://developer.apple.com//contact/app-store/promote/) Seite.
- **Spiele** – alle apps, die festgelegt wird, um die **Spiel** Kategorie in iTunes Connect finden Sie unter dieser Registerkarte.
- **Apps** – auf dieser Registerkarte verfügt über alle anderen apps. Benutzer können empfohlene apps "," Kategorien "," Top bezahlt/frei durchsuchen.
- **Updates** – diese app zeigt die Updates für Ihre apps. Auch wenn Sie eine app über freigeben [Phasen von Version](#Phased_Release), Benutzer können weiterhin auf dieser Registerkarte aufrufen und die neueste Version herunter.
- **Suche** – auf dieser Registerkarte können Sie Benutzern, für die app zu suchen.

## <a name="store-icon"></a>Symbol "Speicher"

Speichersymbole (oder marketing Symbole) nicht mehr in iTunes Connect verwaltet werden und muss stattdessen enthalten sein, als ein [Asset-Katalog](~/ios/app-fundamentals/images-icons/app-icons.md) in Ihrer app Binärdatei, app-Symbole ähnelt. Ein Symbol "1024 x 1024-Speicher" im PNG-Format muss in eine Asset-Katalog für die erfolgreiche Übermittlung von apps für iOS 11 enthalten sein.

App Ausdünnung wird sichergestellt, dass es sich bei diesen zusätzlichen Asset-Katalog app vergrößern nicht.


## <a name="in-app-purchases-promoted-in-the-app-store"></a>In App-Käufen, die im App Store höher gestuft

Apple hat in app-Einkäufe im App Store mehr erkennbar gemacht. Sie können jetzt bis zu 20 hinzufügen _App Store höher gestuft_ in-app-Käufen, die jetzt auf der Seite Apps auf Ihre app-Produktseite oder durch Suchen befinden.

Um Ihre app-Käufe im App Store angezeigt haben, müssen Sie die folgenden Daten einschließen:

- **Bild** – müssen Sie ein spezielles Image für das Symbol angeben, die beschreibt, was bewirkt, dass die in-app-Käufe. Dieses Bild muss nicht mit dem Symbol "app" identisch sein und einen Screenshot ist nicht möglich.
- **Namen** – der Name darf nur maximal 30 Zeichen sein.
- **Beschreibung** – die Beschreibung kann nur maximal 45 Zeichen sein.

Gewährten Rabatten in app-Käufe unterliegen einem app Store Review aus, bevor sie veröffentlicht werden kann.

Um in-app-Käufe höher stufen verfügbar machen möchten, öffnen Sie die app, und navigieren Sie zu **Features > In App-Käufe**. Wechseln Sie zu der **App Store Promotion (Optional)** Abschnitt, und fügen Sie ein Bild 1024 x 1024 und **speichern**, wie in der folgenden Abbildung dargestellt:

![App Store Promotion Abschnitt iTune verbinden](app-store-changes-images/image4.png)

Müssen Sie auch hinzufügen der `ShouldAddStorePayment` Methode, um die `SKPaymentTransactionObserver` Protokoll in Ihrer app.

Weitere Informationen zu in-app-Käufe Werbeaktionen, finden Sie in der Apple [Heraufstufen des In-App-Käufe](https://developer.apple.com/app-store/promoting-in-app-purchases/) Seite.

## <a name="redesigned-product-page"></a>Neu gestaltete-Produktseite

Auf der Seite "Product" wurden die folgenden Änderungen vorgenommen:

- Titel werden jetzt auf maximal 30 Zeichen festgelegt.
- Untertitel wurde hinzugefügt.
    - Es sollte eine kurze Zusammenfassung, die den Titel ergänzt.
    - Untertitel sollte maximal 30 Zeichen haben.
- App-Vorschau
    - Sie haben jetzt drei Videos oder Screenshots.
    - Videos Autoplay, wenn ein Benutzer auf die Seite besucht wird.
    - Weitere Informationen finden Sie auf der Apple [App-Vorschau](https://developer.apple.com/app-store/app-previews/) Seite.
- Werbe-text
    - Diese neue Funktion bietet 170 Zeichen des Texts, weil Sie damit häufig ändernden Informationen über die app beschreiben.
    - Es kann jederzeit aktualisiert werden, ohne eine neue Version der app zu senden.

## <a name="customer-communication"></a>Kommunikation mit Ihren Kunden

Im 10.3 gestartet, Apple eine neue Möglichkeit für Entwickler, kommunizieren direkt mit dem Benutzer der app durch die Möglichkeit, auf Testberichte zu reagieren. Sie können auf diese neue Funktion in iTunes zugreifen durch Navigieren zum Verbinden **App > Aktivität > Bewertungen und Kritiken**, wie in der folgenden Abbildung dargestellt:

![Dialogfeld zeigt Bereich Antwort Kommentar eingeben.](app-store-changes-images/image5.png)

Es gibt einige Punkte, die beim Antworten auf Benutzer geachtet werden:

- Sie können nur einmal Antworten, aber beide Parteien können ihre Kommentare bearbeiten, wie sie möchten.
- Benutzer erhalten eine Benachrichtigung, wenn Sie in einen Kommentar antworten.
- Verbinden Sie einen neuen Kundensupport, die App im iTunes Rolle erstellt wurde. Kommentare können Benutzer mit dieser Rolle oder eine Rolle "Admin" beantworten.

Weitere Informationen finden Sie auf der Apple [reagieren auf Testberichte](https://developer.apple.com/app-store/responding-to-reviews/) Seite.

<a name="Phased_Release"/>

## <a name="phased-release"></a>Stufenweise Bereitstellung

Mit iOS 11 implementiert Apple hat die Möglichkeit, in Phasen Versionen nach Updates für Ihre app. Sie können in mehreren Phasen Versionen zum ermöglichen der app-Updates allmählich für Kunden, die sicherstellen, dass die Anforderung nicht mit die produktionsumgebung überlädt veröffentlicht werden soll.

Stufenweise Versionen sind in iTunes Connect aktiviert. Klicken Sie auf Ihre app in der Randleiste, einen Bildlauf zu der **in Phasen Release für automatische Updates** im Abschnitt unten, und wählen Sie **Release von Update 7-Tage-Zeitraum mit Phasen Release**:

![Mit der Option in Phasen Release für automatische updates](app-store-changes-images/image6.png)

Der Updates ist sofort zum Download in der Registerkarte "Updates" im App Store verfügbar. Stufenweise Versionen sind nur verfügbar für Benutzer, die automatische Downloads ausgewählt haben.


## <a name="related-links"></a>Verwandte Links

- [Was ist neu in iOS 11 (Apple)](https://developer.apple.com/ios/)
- [Aktualisierte App Store-Produktseite (Apple)](https://developer.apple.com/app-store/product-page/)
- [Aktualisieren Ihre App für iOS 11 (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/204/)
