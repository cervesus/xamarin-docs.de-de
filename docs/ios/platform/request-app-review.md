---
title: "Prüfung der änderungsanforderungen-App"
description: "Dieser Artikel behandelt der RequestReview-Methode, Apple iOS 10 und wie er in Xamarin.iOS implementiert hinzugefügt."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6408e707-b7dc-4557-b931-16a4d79b8930
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: 2e63f2c47bbcd6da0f0d5370ebfc231d19a10e7d
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="request-app-review"></a>Prüfung der änderungsanforderungen-App

_Dieser Artikel behandelt der RequestReview-Methode, Apple iOS 10 und wie er in Xamarin.iOS implementiert hinzugefügt._

Neu bei iOS 10.3, die `RequestReview()` -Methode ermöglicht es eine iOS-app an den Benutzer bitten, bewerten, oder Überprüfen sie. Beim Aufrufen dieser Methode in einer Protokollversand-app ist, die der Benutzer aus dem App Store installiert wurde, werden iOS 10 behandeln die gesamte Bewertung und einen Prüfvorgang für den Entwickler. Da dieser Vorgang von der Richtlinie für die App Store gesteuert wird, wird eine Warnung kann oder möglicherweise nicht angezeigt.

![](request-app-review-images/review01.png "Eine Warnung für die Beispiel-Review-Anforderung")

## <a name="requesting-a-rating-or-review"></a>Eine Bewertung oder Review anfordern

Während der `RequestReview()` statische Methode der `SKStoreReviewController` Klasse aufgerufen werden kann zu jedem Zeitpunkt, in denen es in die Benutzeroberfläche sinnvoll, der Prüfvorgang gesteuert werden, und vom App Store Richtlinien behandelt. Daher diese Methode möglicherweise eine Warnung u. u. nicht angezeigt und sollte nie in Reaktion auf eine Benutzeraktion, z. B. Tippen auf eine Schaltfläche aufgerufen werden.

Beispielsweise kann eine app anfordern, eine Überprüfung, nachdem sie wurde gestartet, eine bestimmte Anzahl von Malen oder ein Spiel möglicherweise einen Review anfordern, nachdem der Spieler eine Ebene abgeschlossen ist.

Anforderungen eine Überprüfung, sobald eine app Xamarin.iOS beendet wurde, wird gestartet, stellen die folgenden Änderungen an der `AppDelegate.cs` Datei:

```csharp
using Foundation;
using StoreKit;
using UIKit;

namespace iOSTenThree
{
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate
    {
        ...

        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            // Request a review from the user
            SKStoreReviewController.RequestReview ();

            return true;
        }
        
        ...
        
    }
}
```

> [!NOTE]
> Aufrufen von `RequestReview()` in eine unzureichende Entwicklung app wird immer die Bewertung anzeigen und überprüfen Sie Dialogfeld damit getestet werden können. Dies gilt nicht für apps, die über TestFlight, verteilt wurden, in dem Aufruf der Methode werden ignoriert.

Wenn die `RequestReview()` Methode wird aufgerufen, in einer Protokollversand-app, die der Benutzer aus dem App Store installiert wurde, iOS 10 wird den gesamten Prozess für die Bewertung und überprüfen Sie für den Entwickler zu behandeln. Erneut aus, da dieser Vorgang von der Richtlinie für die App Store gesteuert wird, eine Warnung kann oder möglicherweise nicht angezeigt.

## <a name="linking-to-an-app-store-product-page"></a>Verknüpfen mit einem App Store-Produktseite 

Zusätzlich zu den neuen `RequestReview` -Methode der Entwickler kann weiterhin einen deep-Link der app-Produktseite im App Store innerhalb einer app bereitstellen. Durch Anhängen `action=write-review` bis zum Ende der Seite "-URL des Produkts, eine Seite geöffnet wird, in denen der Benutzer eine Überprüfung der app automatisch schreiben kann. 

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden der RequestReview-Methode, Apple iOS 10 und wie er in Xamarin.iOS implementiert hinzugefügt behandelt.



## <a name="related-links"></a>Verwandte Links

- [iOSTenThree-Beispiel](https://developer.xamarin.com/samples/ios/iOS10/iOSTenThree)
