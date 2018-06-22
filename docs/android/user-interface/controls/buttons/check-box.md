---
title: CheckBox
ms.prod: xamarin
ms.assetid: A884AF10-D5EA-72CA-2301-B80CEC7FFBE7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 85c505d03e7a763b24fb176b6a94c0fe43009b79
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30765752"
---
# <a name="checkbox"></a>CheckBox

In diesem Abschnitt erstellen Sie ein Kontrollkästchen zur Auswahl von Elementen mithilfe der [ `CheckBox` ](https://developer.xamarin.com/api/type/Android.Widget.CheckBox) Widget. Beim Klicken auf das Kontrollkästchen aufgerufen wird, wird eine Toast-Nachricht den aktuellen Status des Kontrollkästchens angeben.

Öffnen der **Resources/layout/Main.axml** und fügen die [ `CheckBox` ](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/) Element (innerhalb der [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout)):

```xml
<CheckBox android:id="@+id/checkbox"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="check it out" />
```

Um etwas tun, wenn der Status geändert wird, fügen Sie den folgenden Code am Ende der [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle) Methode:

```csharp
CheckBox checkbox = FindViewById<CheckBox>(Resource.Id.checkbox);

checkbox.Click += (o, e) => {
    if (checkbox.Checked)
        Toast.MakeText (this, "Selected", ToastLength.Short).Show ();
    else
        Toast.MakeText (this, "Not selected", ToastLength.Short).Show ();
};
```

Zeichnet die [ `CheckBox` ](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/) Element aus dem Layout verarbeitet dann das Click-Ereignis, das definiert die Aktion, die vorgenommen werden, wenn das Kontrollkästchen geklickt wird. Beim Klicken auf die [ `Checked` ](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/) Eigenschaft aufgerufen, um den neuen Zustand des Kontrollkästchens überprüfen. Wenn sie überprüft wurde, wird eine [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) zeigt die Meldung "Ausgewählt", andernfalls "Nicht ausgewählt" angezeigt. Die [ `CheckBox` ](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/) eigene Zustandsänderungen behandelt, daher Sie nur den aktuellen Status abzufragen müssen.

Führen sie aus.

**Tipp:** Wenn Sie den Status zu ändern müssen (z. B. wenn laden eine gespeicherte [ `CheckBoxPreference` ](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference), verwenden Sie die [ `Checked` ](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked) Setter für eine Eigenschaft oder [ `Toggle()` ](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle) Methode.

*Teile dieser Seite werden basierend auf der Arbeit erstellt und von Android Open Source-Projekt gemeinsam genutzt und verwendet entsprechend Begriffe, die in beschriebenen Änderungen der*
[*Creative Commons 2.5 Namensnennung Lizenz* ](http://creativecommons.org/licenses/by/2.5/).
