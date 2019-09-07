---
title: Xamarin. Android-ratingleiste
description: Hinzufügen eines ratingbar-Widgets zu einer Android-Aktivität
ms.prod: xamarin
ms.assetid: d7a1f9bb-926d-4f93-9e8e-0fa933e330e7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
ms.openlocfilehash: 51f88dba25ca2b4f7e33bb8b5c813c43a214c062
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70764850"
---
# <a name="xamarinandroid-ratingbar"></a>Xamarin. Android-ratingleiste

Eine ratingleiste ist ein UI-Widget, das eine Bewertung von einem bis fünf Sternen anzeigt. Der Benutzer kann eine Bewertung auswählen, indem er einen Stern in diesem Abschnitt tippen. Sie erstellen ein Widget, mit dem der Benutzer mit dem [`RatingBar`](xref:Android.Widget.RatingBar) Widget eine Bewertung bereitstellen kann.

![Beispiel für eine ratingleiste](ratingbar-images/01-ratingbar.png)

## <a name="creating-a-ratingbar"></a>Erstellen einer ratingleiste

1. Öffnen Sie die Datei " **Resource/Layout/Main. axml** ", und fügen Sie das[`RatingBar`](xref:Android.Widget.RatingBar)
   -Element (in [`LinearLayout`](xref:Android.Widget.LinearLayout)):

   ```xml
   <RatingBar android:id="@+id/ratingbar"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:numStars="5"
            android:stepSize="1.0"/>
   ```

   Das `android:numStars` -Attribut definiert, wie viele Sterne für die Bewertungs Leiste angezeigt werden. Das `android:stepSize` -Attribut definiert die Granularität für jeden Stern (z. b., `0.5` wenn ein Wert von halb Stern Bewertungen zulässt).

2. Um etwas zu tun, wenn eine neue Bewertung festgelegt wurde, fügen Sie den folgenden Code am Ende der[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
   anzuwenden

    ```csharp
    RatingBar ratingbar = FindViewById<RatingBar>(Resource.Id.ratingbar);

    ratingbar.RatingBarChange += (o, e) => {
            Toast.MakeText(this, "New Rating: " + ratingbar.Rating.ToString (), ToastLength.Short).Show ();
    };
    ```

    Dadurch wird das [`RatingBar`](xref:Android.Widget.RatingBar) widget aus dem Layout mit [`FindViewById`](xref:Android.App.Activity.FindViewById*) aufgezeichnet. Anschließend wird eine Ereignismethode festgelegt und dann die Aktion definiert, die durchgeführt werden soll, wenn der Benutzer eine Bewertung festlegt. In diesem Fall wird eine einfache [`Toast`](xref:Android.Widget.Toast) Meldung mit der neuen Bewertung angezeigt.

3. Führen Sie die Anwendung aus.
