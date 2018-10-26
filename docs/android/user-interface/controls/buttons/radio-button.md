---
title: RadioButton
ms.prod: xamarin
ms.assetid: 3C32EA3F-D917-C988-72C5-A17354DA791E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: be473580b24dba6b4f08384771e2097d368f8dc8
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50123595"
---
# <a name="radiobutton"></a>RadioButton

In diesem Abschnitt erstellen Sie zwei sich gegenseitig ausschließende Optionsfelder (Aktivieren einer deaktiviert die andere), mithilfe der [`RadioGroup`](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/)
Und [`RadioButton`](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)
Widgets. Wenn entweder Optionsfeld gedrückt wird, wird eine eingeblendeten Nachricht angezeigt werden.


Öffnen der **Resources/layout/Main.axml** -Datei und fügen Sie zwei [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)s, die in geschachtelten eine [ `RadioGroup` ](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/) (innerhalb der [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)):

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

Es ist wichtig, die die [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)s werden gruppiert, indem die [ `RadioGroup` ](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/) Element, damit nicht mehr als einen gleichzeitig ausgewählt werden kann. Diese Logik wird automatisch vom System Android behandelt. Wenn bei mindestens einer [`RadioButton`](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)
innerhalb eine Gruppe ausgewählt ist, alle anderen werden nicht automatisch ausgewählt.

Bestimmte Aktionen ausführen müssen bei jeder [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/) ist ausgewählt haben, müssen wir einen Ereignishandler schreiben:

```csharp
private void RadioButtonClick (object sender, EventArgs e)
{
    RadioButton rb = (RadioButton)sender;
    Toast.MakeText (this, rb.Text, ToastLength.Short).Show ();
}
```

Zunächst wird der Absender, der übergeben wird, in einem RadioButton umgewandelt.
Ein [`Toast`](https://developer.xamarin.com/api/type/Android.Widget.Toast/)
Meldung zeigt an, dem ausgewählten Optionsfeld Text.

Jetzt am unteren Rand der [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle)
Methode, fügen Sie Folgendes hinzu:

```csharp
RadioButton radio_red = FindViewById<RadioButton>(Resource.Id.radio_red);
RadioButton radio_blue = FindViewById<RadioButton>(Resource.Id.radio_blue);

radio_red.Click += RadioButtonClick;
radio_blue.Click += RadioButtonClick;
```

Hierbei werden zusammengefasst, jede der [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)s aus dem Layout und fügt die neu erstellte Ereignis Handlerto jeder.

Führen Sie die Anwendung aus.

**Tipp:** Wenn Sie den Status zu ändern müssen (z. B. beim Laden eine gespeicherte [ `CheckBoxPreference` ](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference/)), verwenden Sie die [`Checked`](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/)
Eigenschaften-Setter oder [`Toggle()`](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle/)
-Methode.

*Teile dieser Seite werden Änderungen, die basierend auf der Arbeit erstellt und freigegeben werden, indem Sie das Android Open Source-Projekt, und gemäß den Bedingungen, die in beschriebenen verwendet die*
[*Creative Commons 2.5 Attribution-Lizenz* ](http://creativecommons.org/licenses/by/2.5/). 
