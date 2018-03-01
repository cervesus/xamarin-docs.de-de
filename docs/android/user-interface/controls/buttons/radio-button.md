---
title: RadioButton
ms.topic: article
ms.prod: xamarin
ms.assetid: 3C32EA3F-D917-C988-72C5-A17354DA791E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: c4f4909813f5c82a49ec51278b3b50cc36a8e17b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="radiobutton"></a>RadioButton

In diesem Abschnitt erstellen Sie zwei sich gegenseitig ausschließende Optionsfelder (aktivieren 1 deaktiviert die andere), mithilfe der [ `RadioGroup` ](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/) und [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/) Widgets. Wenn entweder Optionsfeld gedrückt wird, wird eine Toast-Meldung angezeigt werden.


Öffnen der **Resources/layout/Main.axml** Datei, und fügen Sie zwei [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)s, geschachtelte eine [ `RadioGroup` ](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/) (innerhalb der [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)):

```xml
<RadioGroup
  android:layout_width="fill_parent"
  android:layout_height="wrap_content"
  android:orientation="vertical">
  <RadioButton android:id="@+id/radio_red"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:text="Red" />
  <RadioButton android:id="@+id/radio_blue"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:text="Blue" />
</RadioGroup>
```

Es ist wichtig, die die [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)s werden gruppiert, indem Sie die [ `RadioGroup` ](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/) Element, damit mehr als eine gleichzeitig ausgewählt werden kann. Diese Logik wird automatisch vom System Android behandelt. Wenn bei mindestens einer [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/) innerhalb eine Gruppe ausgewählt ist, alle anderen sind automatisch deaktiviert.

Eine Aktion bei jeder [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/) ist ausgewählt, um einen Ereignishandler schreiben müssen:

```csharp
private void RadioButtonClick (object sender, EventArgs e)
{
    RadioButton rb = (RadioButton)sender;
    Toast.MakeText (this, rb.Text, ToastLength.Short).Show ();
}
```

Zunächst wird der Absender das übergebene in einer "RadioButton" umgewandelt werden.
Ein [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) Meldung der ausgewählten Optionsfelds Text anzeigt.

Nun am unteren Rand der [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle) -Methode, fügen Sie Folgendes hinzu:

```csharp
RadioButton radio_red = FindViewById<RadioButton>(Resource.Id.radio_red);
RadioButton radio_blue = FindViewById<RadioButton>(Resource.Id.radio_blue);

radio_red.Click += RadioButtonClick;
radio_blue.Click += RadioButtonClick;
```

Zeichnet alle der [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)s aus dem Layout und fügt die neu erstellte Ereignis Handlerto jedes.

Führen Sie die Anwendung aus.

**Tipp:** Wenn Sie den Status zu ändern müssen (z. B. wenn laden eine gespeicherte [ `CheckBoxPreference` ](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference/)), verwenden Sie die [ `Checked` ](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/) Setter für eine Eigenschaft oder [ `Toggle()` ](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle/) Methode.

*Teile dieser Seite werden basierend auf der Arbeit erstellt und von Android Open Source-Projekt gemeinsam genutzt und verwendet entsprechend Begriffe, die in beschriebenen Änderungen der*
[*Creative Commons 2.5 Namensnennung Lizenz* ](http://creativecommons.org/licenses/by/2.5/). 
