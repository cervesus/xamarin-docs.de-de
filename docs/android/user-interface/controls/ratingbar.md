---
title: Xamarin. Android-ratingleiste
description: Hinzufügen eines ratingbar-Widgets zu einer Android-Aktivität
ms.prod: xamarin
ms.assetid: d7a1f9bb-926d-4f93-9e8e-0fa933e330e7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/29/2018
ms.openlocfilehash: 529fecb4e24e83ef7b783815843e132347d99262
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029151"
---
# <a name="xamarinandroid-ratingbar"></a>Xamarin. Android-ratingleiste

Eine ratingleiste ist ein UI-Widget, das eine Bewertung von einem bis fünf Sternen anzeigt. Der Benutzer kann eine Bewertung auswählen, indem er einen Stern in diesem Abschnitt tippen. Sie erstellen ein Widget, das es dem Benutzer ermöglicht, mit dem [`RatingBar`](xref:Android.Widget.RatingBar) -Widget eine Bewertung bereitzustellen.

![Beispiel für eine ratingleiste](ratingbar-images/01-ratingbar.png)

## <a name="creating-a-ratingbar"></a>Erstellen einer ratingleiste

1. Öffnen Sie die Datei " **Resource/Layout/Main. axml** ", und fügen Sie die [`RatingBar`](xref:Android.Widget.RatingBar)
   -Element (innerhalb des [`LinearLayout`](xref:Android.Widget.LinearLayout)):

   ```xml
   <RatingBar android:id="@+id/ratingbar"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:numStars="5"
            android:stepSize="1.0"/>
   ```

   Das `android:numStars`-Attribut definiert, wie viele Sterne für die Bewertungs Leiste angezeigt werden. Mit dem `android:stepSize`-Attribut wird die Granularität für jeden Stern definiert (z. b. der Wert `0.5` der die Hälfte der Bewertung zulässt).

2. Wenn eine neue Bewertung festgelegt wurde, fügen Sie den folgenden Code am Ende der [`OnCreate()`](xref:Android.App.Activity.OnCreate*)
   anzuwenden

    ```csharp
    RatingBar ratingbar = FindViewById<RatingBar>(Resource.Id.ratingbar);

    ratingbar.RatingBarChange += (o, e) => {
            Toast.MakeText(this, "New Rating: " + ratingbar.Rating.ToString (), ToastLength.Short).Show ();
    };
    ```

    Dadurch wird das [`RatingBar`](xref:Android.Widget.RatingBar) -widget aus dem Layout mit [`FindViewById`](xref:Android.App.Activity.FindViewById*) erfasst. Anschließend wird eine Ereignismethode festgelegt und dann die Aktion definiert, die durchgeführt werden soll, wenn der Benutzer eine Bewertung festlegt. In diesem Fall wird eine einfache [`Toast`](xref:Android.Widget.Toast) Meldung mit der neuen Bewertung angezeigt.

3. Führen Sie die Anwendung aus.
