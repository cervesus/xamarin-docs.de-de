---
title: Anfordern von App-Prüfung in Xamarin.iOS
description: In diesem Artikel wird beschrieben, der RequestReview-Methode, die Apple iOS 10 hinzugefügt, und es wird erläutert, wie sie in Xamarin.iOS implementieren.
ms.prod: xamarin
ms.assetid: 6408e707-b7dc-4557-b931-16a4d79b8930
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/29/2017
ms.openlocfilehash: f72aaa781b0712e206cf02725cfc434594287f41
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61365801"
---
# <a name="request-app-review-in-xamarinios"></a>Anfordern von App-Prüfung in Xamarin.iOS

_Dieser Artikel behandelt der RequestReview-Methode, die Apple iOS 10 und wie Sie es in Xamarin.iOS implementieren hinzugefügt._

Noch nicht mit iOS 10.3, die `RequestReview()` Methode ermöglicht eine iOS-app, um den Benutzer zu bewerten, oder Überprüfen sie. Wenn diese Methode in einer Protokollversand-app aufgerufen wird, die der Benutzer aus dem App Store installiert wurde, wird mit iOS 10 behandelt die gesamte Bewertung und Überprüfungsprozess für den Entwickler. Da dieser Prozess mit App-Store-Richtlinie gesteuert wird, wird eine Warnung kann oder möglicherweise nicht angezeigt.

![](request-app-review-images/review01.png "Eine beispielwarnung Überprüfung anfordern")

## <a name="requesting-a-rating-or-review"></a>Anfordern, eine Bewertung oder Kritik

Während der `RequestReview()` statische Methode der `SKStoreReviewController` -Klasse kann aufgerufen werden zu jedem Zeitpunkt, wo es sinnvoll, auf der Benutzeroberfläche ist, des Überprüfungsprozesses gesteuert werden, und vom App Store-Richtlinie verarbeitet. Daher diese Methode kann eine Warnung kann nicht angezeigt und sollte nie aufgerufen werden, als Reaktion auf eine Benutzeraktion, z. B. auf eine Schaltfläche tippen.

Beispielsweise kann eine Anwendung anfordern, eine Überprüfung aus, nachdem sie wurde gestartet, eine angegebene Anzahl von Malen oder ein Spiel könnte eine Überprüfung anfordern, nachdem der Spieler eine Ebene abgeschlossen ist.

Für Anforderungen stellen eine Überprüfung, sobald Xamarin.iOS-app abgeschlossen ist, starten, die folgenden Änderungen an der `AppDelegate.cs` Datei:

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
> Aufrufen von `RequestReview()` in eine unterdimensionierte Entwicklung app wird immer die Bewertung anzeigen und überprüfen Sie Dialogfeld so, dass sie getestet werden kann. Dies gilt nicht für apps, die über TestFlight, verteilt wurden, in dem Aufruf der Methode ignoriert werden.

Wenn die `RequestReview()` Methode wird aufgerufen, in einer Protokollversand-app, die der Benutzer aus dem App Store installiert wurde, wird iOS 10 des gesamten Prozess der Bewertung und überprüfen Sie für den Entwickler. In diesem Fall, da dieser Vorgang von der App-Store-Richtlinie unterliegt, eine Warnung kann oder möglicherweise nicht angezeigt.

## <a name="linking-to-an-app-store-product-page"></a>Verknüpfen mit einem App Store-Produktseite 

Zusätzlich zu den neuen `RequestReview` -Methode, die der Entwickler kann einen deep-Link zu der app-Produktseite in den App Store aus einer App weiterhin bereitstellen. Durch Anhängen `action=write-review` an das Ende der Product-Seiten-URL, eine Seite wird geöffnet, in denen der Benutzer eine Übersicht über die app automatisch schreiben kann. 

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden der RequestReview-Methode, die Apple iOS 10 und wie Sie es in Xamarin.iOS implementieren hinzugefügt behandelt.



## <a name="related-links"></a>Verwandte Links

- [iOSTenThree-Beispiel](https://developer.xamarin.com/samples/ios/iOS10/iOSTenThree)
