---
title: App-Store Änderungen in iOS 11
description: In diesem Dokument werden Änderungen an den Store-App unter iOS 11. Es beschreibt Store-Symbol der Anwendung, höher gestuften in-app-Käufe, neu entworfenes Produkt-Seite, Kundenkommunikation und in mehreren Phasen Releases.
ms.prod: xamarin
ms.assetid: 4A7A03FD-B4F2-4969-8676-A17260730FD6
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/13/2016
ms.openlocfilehash: 022d6b5c3f85863352dd1343752e934240b357aa
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113689"
---
# <a name="app-store-changes-in-ios-11"></a>App-Store Änderungen in iOS 11

Der iOS App Store hatte völlig neu entworfen, die nicht nur Benutzer den Store effizient zu navigieren können, jedoch Sie auch als Entwickler, um die app Benutzern höher zu stufen können. Diese heraufstufungen umfassen Updates für in-app-Käufe und Updates für die Seite "Product". iOS 11 bietet außerdem die laufenden mit Benutzern kommunizieren, wie Sie Ihr app-Symbol hinzufügen und wie Sie Ihre app öffentlich freigeben.

![Neue app-Store-layout](app-store-changes-images/image3.jpg)

Neu gestaltete appstore enthält die folgenden Abschnitte:

- **Heute** – auf dieser Registerkarte enthält apps, die einer Pick"Anmerkung der Redaktion" oder app vorgestellt. Förderung Ihrer app Hier geben Sie Informationen auf der [höher stufen](https://developer.apple.com//contact/app-store/promote/) Seite.
- **Spiele** – alle apps, die festgelegt wird, um die **Spiel** Kategorie in iTunes Connect finden Sie unter dieser Registerkarte.
- **Apps** – auf dieser Registerkarte verfügt über alle anderen apps. Benutzer können empfohlene apps "," Kategorien "," Top bezahlt/free durchsuchen.
- **Updates** : Diese app zeigt die Updates für Ihre apps. Auch wenn Sie eine app über freigeben [stufenweise Veröffentlichung](#Phased_Release), Benutzer können weiterhin auf dieser Registerkarte finden Sie unter und die neueste Version herunterladen.
- **Suche** – auf dieser Registerkarte können Sie Benutzern, für Ihre app zu suchen.

## <a name="store-icon"></a>Store-Symbol

Store-Symbole (oder marketing-Symbole) werden nicht mehr in iTunes Connect verwaltet und müssen stattdessen enthalten sein, als ein [Asset-Katalog](~/ios/app-fundamentals/images-icons/app-icons.md) in Ihre app Binärdaten, wie app-Symbole. Ein 1024 x 1024-Store-Symbol im PNG-Format muss in einen Ressourcenkatalog für die erfolgreiche Übermittlung von apps für iOS 11 enthalten sein.

Einhaltung der Regeln für App stellt sicher, dass es sich bei diesen zusätzlichen Asset-Katalog app vergrößert nicht.


## <a name="in-app-purchases-promoted-in-the-app-store"></a>In-App-Käufen, die höher gestuft, in dem App Store

Apple hat in-app-Käufen in den App Store leichter auffindbar vorgenommen. Sie können nun bis zu 20 hinzufügen _App Store, die höher gestuft_ in-app-Käufen, die auf der Seite "Apps", auf der Produktseite von Ihrer app, oder indem Sie suchen jetzt befinden.

Um Ihre Einkäufe in der app, die in den App Store angezeigt haben, müssen Sie die folgenden Daten einschließen:

- **Image** – Sie müssen ein spezielles Image für das Symbol angeben, die beschreibt, was bewirkt, dass die in-app-Käufe. Dieses Image muss sich von dem app-Symbol, und es nicht möglich, einen Screenshot.
- **Namen** – der Name kann nur maximal 30 Zeichen lang sein.
- **Beschreibung** – die Beschreibung kann nur maximal 45 Zeichen sein.

Alle Erweiterungen in app-Käufe werden ein app-Store-Review, bevor er veröffentlicht werden kann.

Um Ihre Einkäufe in der app zur Förderung der verfügbar zu machen, öffnen Sie die app aus, und navigieren Sie zu **Features > In App-Käufe**. Wechseln Sie zu der **App Store Promotion (Optional)** Abschnitt, und fügen Sie ein Image von 1024 x 1024 und **speichern**, wie in der folgenden Abbildung dargestellt:

![App-Store-heraufstufung-Abschnitt in iTunes Connect](app-store-changes-images/image4.png)

Sie müssen auch hinzufügen der `ShouldAddStorePayment` Methode, um die `SKPaymentTransactionObserver` -Protokolls in Ihrer app.

Weitere Informationen zu in-app-Käufe Werbeaktionen, finden Sie unter Apple [Förderung Ihrer In-App-Käufe](https://developer.apple.com/app-store/promoting-in-app-purchases/) Seite.

## <a name="redesigned-product-page"></a>Neu gestaltete-Produktseite

Klicken Sie auf der Produktseite von haben die folgenden Änderungen vorgenommen wurden:

- Titel werden jetzt auf maximal 30 Zeichen festgelegt.
- Ein Subtitle wurde hinzugefügt.
    - Es sollte eine kurze Zusammenfassung, die den Titel ergänzt.
    - Untertitel sollte maximal 30 Zeichen haben.
- App-Vorschau
    - Sie haben jetzt drei Videos oder Screenshots.
    - Videos-Wiedergabe, wenn ein Benutzer die Seite besucht.
    - Weitere Informationen finden Sie auf der Apple [App-Vorschau](https://developer.apple.com/app-store/app-previews/) Seite.
- Werbe-text
    - Dieses neue Feature enthält 170 Zeichen des Texts, sodass Sie häufig ändernden Informationen zu Ihrer app zu beschreiben.
    - Sie können jederzeit aktualisiert werden, ohne eine neue Version der app zu senden.

## <a name="customer-communication"></a>Kommunikation mit Ihren Kunden

Apple gestartet 10.3 eine neue Möglichkeit für Entwickler kommunizieren direkt mit app-Benutzer durch die Möglichkeit, auf Testberichte zu reagieren. Sie können auf zugreifen, diese neue Funktion in iTunes eine Verbindung herstellen, indem Sie zu **App > Aktivität > Bewertungen und Rezensionen**, wie in der folgenden Abbildung dargestellt:

![Dialogfeld mit der Bereich, um die Antwort, um einen Kommentar zu geben.](app-store-changes-images/image5.png)

Es gibt einige Dinge zu beachten beim Antworten auf Benutzer aus:

- Sie können nur nach Antworten, aber beide Parteien können ihre Kommentare bearbeiten, wie sie möchten.
- Benutzer erhalten eine Benachrichtigung, wenn Sie auf einen Kommentar antworten.
- Verbinden Sie ein neues Supportangebot zu Kunden, die Rolle in iTunes erstellt wurde. Benutzer mit dieser Rolle oder eine Administratorrolle können auf Kommentare reagieren.

Weitere Informationen finden Sie auf der Apple [reagieren auf Überprüfungen](https://developer.apple.com/app-store/responding-to-reviews/) Seite.

<a name="Phased_Release"/>

## <a name="phased-release"></a>Stufenweise Bereitstellung

In iOS 11 hat Apple die Möglichkeit, in Phasen Releases für Updates für Ihre app implementiert. Sie können in mehreren Phasen Versionen können Sie Ihre app ein Update auf die nach und nach freigegeben werden, um Kunden, um sicherzustellen, dass die Nachfrage Ihrer produktionsumgebung nicht überlädt.

In mehreren Phasen Versionen sind in iTunes Connect aktiviert. Klicken Sie auf Ihre app in der Randleiste, scrollen Sie zu der **stufenweise Release für automatische Updates** im Abschnitt unten, und wählen Sie **Sprachrelease-Updates über 7 Tage mithilfe stufenweise Veröffentlichung**:

![Mit der Option stufenweise Veröffentlichung für automatische updates](app-store-changes-images/image6.png)

Das Update ist sofort zum Download in der Registerkarte "Updates" der App-Store verfügbar. In mehreren Phasen Versionen sind nur verfügbar für Benutzer, die automatische Downloads ausgewählt haben.


## <a name="related-links"></a>Verwandte Links

- [Was ist neu in iOS 11 (Apple)](https://developer.apple.com/ios/)
- [Aktualisierte App-Store-Produktseite (Apple)](https://developer.apple.com/app-store/product-page/)
- [Aktualisieren Ihrer App für iOS 11 (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/204/)
