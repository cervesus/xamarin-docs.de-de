---
title: ToggleButton
ms.prod: xamarin
ms.assetid: 9ADA8FCF-63ED-897A-DD56-D7D86535A92C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 91003f9a23c667b38028a9852b28dba656ba13db
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510333"
---
# <a name="togglebutton"></a>ToggleButton

In diesem Abschnitt erstellen Sie mithilfe des Widgets eine Schaltfläche, die speziell für das [`ToggleButton`](xref:Android.Widget.ToggleButton) Umschalten zwischen zwei Zuständen verwendet wird. Dieses Widget ist eine hervorragende Alternative zu Options Feldern, wenn Sie zwei einfache Zustände haben, die sich gegenseitig ausschließen (z. b. "on" und "Off"). Android 4,0 (API-Ebene 14) hat eine Alternative zu der UMSCHALT Fläche eingeführt, die [`Switch`](xref:Android.Widget.Switch)als bezeichnet wird.

Ein Beispiel für eine **ToggleButton-Taste** kann im linken paar von Bildern angezeigt werden, während das Rechte paar von Bildern ein Beispiel für einen **Switch**darstellt:

![Beispiele für Switches und umgglebuttons in den Zuständen "ein" und "aus"](toggle-button-images/togglebutton-switch.png)  

Welches Steuerelement eine Anwendung verwendet, ist eine Frage des Stils. Beide Widgets sind funktionell gleichwertig.

Öffnen Sie die Datei **Resources/Layout/Main. axml** , und [`ToggleButton`](xref:Android.Widget.ToggleButton) fügen Sie das- [`LinearLayout`](xref:Android.Widget.LinearLayout)Element (innerhalb der) hinzu:

Um etwas zu tun, wenn der Status geändert wird, fügen Sie den folgenden Code am Ende der[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
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

Beachten Sie, [`ToggleButton`](xref:Android.Widget.ToggleButton) dass die die eigene Zustandsänderung zwischen aktivierten und nicht aktivierten behandelt, sodass Sie nur die Frage stellen, welche ist.

Führen Sie die Anwendung aus.


> [!TIP]
> Wenn Sie den Zustand selbst ändern müssen (z. b. beim Laden eines [`CheckBoxPreference`](xref:Android.Preferences.CheckBoxPreference)gespeicherten), verwenden Sie das[`Checked`](xref:Android.Widget.CompoundButton.Checked)
> Eigenschaften Setter oder[`Toggle()`](xref:Android.Widget.CompoundButton.Toggle)
> -Methode.


## <a name="related-links"></a>Verwandte Links

- [ToggleButton](https://developer.android.com/reference/android/widget/ToggleButton.html)
- [Schalter](https://developer.android.com/reference/android/widget/Switch.html)
