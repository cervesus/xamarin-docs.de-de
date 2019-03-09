---
title: ToggleButton
ms.prod: xamarin
ms.assetid: 9ADA8FCF-63ED-897A-DD56-D7D86535A92C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 841a11f9cb48786a046802288ce05e7ae186688a
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57671715"
---
# <a name="togglebutton"></a>ToggleButton

In diesem Abschnitt erstellen Sie eine Schaltfläche verwendet, die speziell für das Umschalten zwischen zwei Zuständen, mit der [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) Widget. Dieses Widget ist eine hervorragende Alternative für Optionsfelder aus, wenn Sie zwei einfache Status verfügen, die sich gegenseitig ausschließende sind ("on" und "off", z. B.). Android 4.0 (API-Ebene 14) eingeführt, eine Alternative zu der Umschaltfläche, bekannt als eine [ `Switch` ](https://developer.xamarin.com/api/type/Android.Widget.Switch/).

Ein Beispiel für eine **ToggleButton** finden Sie im linken-Paar von Abbildern, während die beiden rechten Bilder ein Beispiel stellt eine **Switch**:

![Beispiele für Switches und ToggleButtons sowohl die ein- und Ausschalten der Zustände](toggle-button-images/togglebutton-switch.png)  

Das Steuerelement, das eine Anwendung verwendet wird, eine Frage des Stils. Beide Widgets sind funktional äquivalent.

Öffnen der **Resources/layout/Main.axml** -Datei und fügen die [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) Element (innerhalb der [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)):

Etwas erfolgt, wenn der Status geändert wird, fügen Sie den folgenden Code am Ende der [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle)
Methode:

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

Hierbei werden zusammengefasst, die [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) Element aus dem Layout und behandelt das Click-Ereignis, das definiert, welche Aktion ausgeführt wird, wenn die Schaltfläche geklickt wird. In diesem Beispiel ist die Methode überprüft den neuen Status der Schaltfläche, und zeigt eine [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) Meldung, die den aktuellen Status angibt.

Beachten Sie, dass die [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) seinen eigenen Status zwischen aktiviert oder deaktiviert ist, ändern Sie Sie also nur Fragen, die es verarbeitet.

Führen Sie die Anwendung aus.


**Tipp:** Wenn Sie den Status selbst ändern möchten (z. B. beim Laden eine gespeicherte [ `CheckBoxPreference` ](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference/)), verwenden Sie die [`Checked`](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/)
Eigenschaften-Setter oder [`Toggle()`](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle/)
-Methode.


## <a name="related-links"></a>Verwandte Links

- [ToggleButton](https://developer.android.com/reference/android/widget/ToggleButton.html)
- [Schalter](https://developer.android.com/reference/android/widget/Switch.html)
