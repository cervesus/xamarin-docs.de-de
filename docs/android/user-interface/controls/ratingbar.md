---
title: RatingBar
description: Wie eine Android-Aktivität ein RatingBar-Widgets hinzugefügt.
ms.prod: xamarin
ms.assetid: d7a1f9bb-926d-4f93-9e8e-0fa933e330e7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
ms.openlocfilehash: 97d2a126be70e210d2e8f4ebf4d7a25ff8777a02
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "60945440"
---
# <a name="ratingbar"></a>RatingBar

Eine RatingBar ist eine UI-Widgets, die auf eine Skala von 1 bis 5 Sternen anzeigt. Der Benutzer kann eine Bewertung von Berichtsinhalt auf einem Stern In diesem Abschnitt auswählen, erstellen Sie ein Widget, das dem Benutzer ermöglicht, eine Bewertung, bieten die [ `RatingBar` ](https://developer.xamarin.com/api/type/Android.Widget.RatingBar/) Widget.

![Beispiel für eine RatingBar](ratingbar-images/01-ratingbar.png)


## <a name="creating-a-ratingbar"></a>Erstellen eine RatingBar

1. Öffnen der **Resource/layout/Main.axml** -Datei und fügen die [`RatingBar`](https://developer.xamarin.com/api/type/Android.Widget.RatingBar/)
   -Element (innerhalb der [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)):

    ```xml
    <RatingBar android:id="@+id/ratingbar"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:numStars="5"
            android:stepSize="1.0"/>
    ```
   Die `android:numStars` Attribut wird definiert, wie viele Sterne für die Bewertung Leiste anzeigen möchten. Die `android:stepSize` Attribut definiert die Granularität für jedes Sterns (z. B. den Wert `0.5` halben Stern-Bewertungen können).

2. Um etwas tun, wenn eine neue Bewertung festgelegt wurde, fügen Sie den folgenden Code am Ende der [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle)
   Methode:

    ```csharp
    RatingBar ratingbar = FindViewById<RatingBar>(Resource.Id.ratingbar);

    ratingbar.RatingBarChange += (o, e) => {
            Toast.MakeText(this, "New Rating: " + ratingbar.Rating.ToString (), ToastLength.Short).Show ();
    };
    ```

    Hierbei werden zusammengefasst, die [ `RatingBar` ](https://developer.xamarin.com/api/type/Android.Widget.RatingBar/) Widgets aus dem Layout mit [ `FindViewById` ](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/) und legt eine Ereignismethode anschließend definiert, welche Aktion ausgeführt wird, wenn der Benutzer eine Bewertung festlegt. In diesem Fall eine einfache [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) Meldung angezeigt, das die neue Bewertung.

3.  Führen Sie die Anwendung aus.

