---
title: ToggleButton
ms.prod: xamarin
ms.assetid: 9ADA8FCF-63ED-897A-DD56-D7D86535A92C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: fe444d255beb9c08b4b5bcf5de36a8740e503b55
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029294"
---
# <a name="togglebutton"></a>ToggleButton

In diesem Abschnitt erstellen Sie eine Schaltfläche, die speziell für das Umschalten zwischen zwei Zuständen verwendet wird, indem Sie das [`ToggleButton`](xref:Android.Widget.ToggleButton) -Widget verwenden. Dieses Widget ist eine hervorragende Alternative zu Options Feldern, wenn Sie zwei einfache Zustände haben, die sich gegenseitig ausschließen (z. b. "on" und "Off"). Android 4,0 (API-Ebene 14) hat eine Alternative zu der UMSCHALT Fläche eingeführt, die als [`Switch`](xref:Android.Widget.Switch)bezeichnet wird.

Ein Beispiel für eine **ToggleButton-Taste** kann im linken paar von Bildern angezeigt werden, während das Rechte paar von Bildern ein Beispiel für einen **Switch**darstellt:

![Beispiele für Switches und umgglebuttons in den Zuständen "ein" und "aus"](toggle-button-images/togglebutton-switch.png)  

Welches Steuerelement eine Anwendung verwendet, ist eine Frage des Stils. Beide Widgets sind funktionell gleichwertig.

Öffnen Sie die Datei **Resources/Layout/Main. axml** , und fügen Sie das [`ToggleButton`](xref:Android.Widget.ToggleButton) -Element (innerhalb des [`LinearLayout`](xref:Android.Widget.LinearLayout)) hinzu:

Fügen Sie den folgenden Code am Ende [`OnCreate()`](xref:Android.App.Activity.OnCreate*) der hinzu, um etwas zu tun, wenn der Status geändert wird.
anzuwenden

```csharp
ToggleButton togglebutton = FindViewById<ToggleButton>(Resource.Id.togglebutton);

togglebutton.Click += (o, e) => {
    // Perform action on clicks
    if (togglebutton.Checked)
        Toast.MakeText(this, "Checked", ToastLength.Short).Show ();
    else
        Toast.MakeText(this, "Not checked", ToastLength.Short).Show ();
};
```

Dadurch wird das [`ToggleButton`](xref:Android.Widget.ToggleButton) -Element aus dem Layout erfasst und das Click-Ereignis behandelt, das die Aktion definiert, die beim Klicken auf die Schaltfläche ausgeführt werden soll. In diesem Beispiel überprüft die-Methode den neuen Status der Schaltfläche und zeigt dann eine [`Toast`](xref:Android.Widget.Toast) Meldung an, die den aktuellen Zustand angibt.

Beachten Sie, dass die [`ToggleButton`](xref:Android.Widget.ToggleButton) die eigene Zustandsänderung zwischen überprüft und deaktiviert verarbeitet, sodass Sie nur die Frage stellen, ob Sie ist.

Führen Sie die Anwendung aus.

> [!TIP]
> Wenn Sie den Zustand selbst ändern müssen (z. b. beim Laden einer gespeicherten [`CheckBoxPreference`](xref:Android.Preferences.CheckBoxPreference)), verwenden Sie die [`Checked`](xref:Android.Widget.CompoundButton.Checked)
> Eigenschaften Setter oder [`Toggle()`](xref:Android.Widget.CompoundButton.Toggle)
> -Methode.

## <a name="related-links"></a>Verwandte Links

- [ToggleButton](https://developer.android.com/reference/android/widget/ToggleButton.html)
- [Schalter](https://developer.android.com/reference/android/widget/Switch.html)
