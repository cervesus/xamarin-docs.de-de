---
title: Anfordern der APP-Überprüfung in xamarin. IOS
description: In diesem Artikel wird die requestreview-Methode beschrieben, die von Apple zu IOS 10 hinzugefügt wurde, und es wird erläutert, wie Sie in xamarin. IOS implementiert wird.
ms.prod: xamarin
ms.assetid: 6408e707-b7dc-4557-b931-16a4d79b8930
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/29/2017
ms.openlocfilehash: ca26951784e330b52059a3d2a7519e35013ab1bf
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2019
ms.locfileid: "70198507"
---
# <a name="request-app-review-in-xamarinios"></a>Anfordern der APP-Überprüfung in xamarin. IOS

_In diesem Artikel wird die requestreview-Methode behandelt, die Apple IOS 10 hinzufügt und erläutert, wie Sie in xamarin. IOS implementiert wird._

Neu bei IOS 10,3, mit `RequestReview()` der-Methode kann eine IOS-App den Benutzer auffordern, ihn zu bewerten oder zu überprüfen. Wenn diese Methode in einer Versand-app aufgerufen wird, die der Benutzer aus dem App Store installiert hat, übernimmt IOS 10 den gesamten Bewertungs-und Überprüfungsprozess für den Entwickler. Da dieser Prozess von der App Store-Richtlinie gesteuert wird, wird möglicherweise eine Warnung angezeigt oder nicht angezeigt.

![](request-app-review-images/review01.png "Eine Beispiel-Überprüfungs Anforderungs Warnung")

## <a name="requesting-a-rating-or-review"></a>Anfordern einer Bewertung oder Überprüfung

Obwohl die `RequestReview()` statische-Methode `SKStoreReviewController` der-Klasse an jedem Punkt aufgerufen werden kann, an dem Sie in der Benutzer Darstellung sinnvoll ist, wird der Überprüfungsprozess von der App Store-Richtlinie gesteuert und behandelt. Folglich kann diese Methode eine Warnung anzeigen und nicht als Antwort auf eine Benutzeraktion aufgerufen werden, z. b. das Tippen auf eine Schaltfläche.

Eine APP kann z. b. eine Überprüfung anfordern, nachdem Sie eine bestimmte Anzahl von Zeiten gestartet hat, oder ein Spiel kann eine Überprüfung anfordern, nachdem der Spieler eine Ebene beendet hat.

Wenn eine Überprüfung angefordert werden soll, sobald eine xamarin. IOS-App gestartet wird, nehmen Sie die `AppDelegate.cs` folgenden Änderungen an der Datei vor:

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
> Wenn `RequestReview()` Sie in einer unter entwicklungsapp aufrufen, wird das Dialogfeld "Bewertung und Review" immer angezeigt, sodass es getestet werden kann. Dies gilt nicht für apps, die über Testflight verteilt wurden, wobei der Methodenaufrufe ignoriert wird.

Wenn die `RequestReview()` -Methode in einer Versand-app aufgerufen wird, die der Benutzer aus dem App Store installiert hat, übernimmt IOS 10 den gesamten Bewertungs-und Überprüfungsprozess für den Entwickler. Da dieser Prozess von der App Store-Richtlinie unterliegt, kann es sein, dass eine Warnung angezeigt wird oder nicht.

## <a name="linking-to-an-app-store-product-page"></a>Verknüpfen mit einer App Store-Produktseite 

Zusätzlich zur neuen `RequestReview` Methode kann der Entwickler weiterhin einen Deep-Link zur Produktseite der APP im App Store innerhalb einer APP bereitstellen. Wenn `action=write-review` Sie am Ende der URL der Produktseite anhängen, wird eine Seite geöffnet, auf der der Benutzer automatisch eine Überprüfung der app schreiben kann. 

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde die requestreview-Methode behandelt, die Apple IOS 10 hinzugefügt hat, und wie diese in xamarin. IOS implementiert wird.



## <a name="related-links"></a>Verwandte Links

- [iostenthree-Beispiel](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-iostenthree/)
