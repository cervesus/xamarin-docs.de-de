---
title: ToggleButton
ms.prod: xamarin
ms.assetid: 9ADA8FCF-63ED-897A-DD56-D7D86535A92C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 8323c456a97a033e19374a4bd3ea7468ecafa608
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="togglebutton"></a>ToggleButton

In diesem Abschnitt erstellen Sie eine Schaltfläche verwendet, die speziell für das Umschalten zwischen zwei Zuständen, mit der [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) Widget. Dieses Widget ist eine hervorragende Alternative für Optionsfelder aus, wenn Sie zwei einfache Zustände haben, die sich gegenseitig ausschließende sind ("on" und "off", z. B.). Android 4.0 (API-Ebene 14) eingeführt, eine Alternative zum die Umschaltfläche als bezeichnet eine [ `Switch` ](https://developer.xamarin.com/api/type/Android.Widget.Switch/).

Ein Beispiel für eine **ToggleButton** im linken Paar von Bildern angezeigt werden, während das rechte-Paar von Bildern ein Beispiel zeigt eine **Switch**:

![Beispiele für Switches und ToggleButtons in beiden ein- und ausschalten Zustände](toggle-button-images/togglebutton-switch.png)  

Welches Steuerelement eine Anwendung verwendet, der Stil ist. Beide Widgets sind funktional äquivalent.

Öffnen der **Resources/layout/Main.axml** und fügen die [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) Element (innerhalb der [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)):

Um etwas tun, wenn der Status geändert wird, fügen Sie den folgenden Code am Ende der [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle) Methode:

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

Zeichnet die [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) Element aus dem Layout und behandelt das Click-Ereignis, das definiert, welche Aktion ausgeführt wird, wenn die Schaltfläche geklickt wird. In diesem Beispiel wird die Methode überprüft den neuen Status der Schaltfläche, und zeigt eine [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) Meldung, die den aktuellen Status angibt.

Beachten Sie, dass die [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) Handles, die ihren eigenen Zustand zwischen checked und unchecked, ändern, damit Sie es also nur bitten.

Führen Sie die Anwendung aus.


**Tipp:** Wenn Sie den Status zu ändern müssen (z. B. wenn laden eine gespeicherte [ `CheckBoxPreference` ](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference/)), verwenden Sie die [ `Checked` ](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/) Setter für eine Eigenschaft oder [ `Toggle()` ](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle/) Methode.


## <a name="related-links"></a>Verwandte Links

- [ToggleButton](http://developer.android.com/reference/android/widget/ToggleButton.html)
- [Schalter](http://developer.android.com/reference/android/widget/Switch.html)
