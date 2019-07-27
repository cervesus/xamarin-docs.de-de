---
title: RadioButton
ms.prod: xamarin
ms.assetid: 3C32EA3F-D917-C988-72C5-A17354DA791E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 2279282b08c9d97b239de424cf38aa6f1463dc4d
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510353"
---
# <a name="radiobutton"></a>RadioButton

In diesem Abschnitt erstellen Sie zwei sich gegenseitig ausschließende Options Felder (durch Aktivieren von eins wird die andere deaktiviert), indem Sie das[`RadioGroup`](xref:Android.Widget.RadioGroup)
immer[`RadioButton`](xref:Android.Widget.RadioButton)
Pass. Wenn beide Options Felder gedrückt werden, wird eine Popup Meldung angezeigt.


Öffnen Sie die Datei **Resources/Layout/Main. axml** , und [`RadioButton`](xref:Android.Widget.RadioButton)fügen Sie zwei s hinzu, [`RadioGroup`](xref:Android.Widget.RadioGroup) die in einem [`LinearLayout`](xref:Android.Widget.LinearLayout)(innerhalb der) geschachtelt sind:

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

Es ist wichtig, dass [`RadioButton`](xref:Android.Widget.RadioButton)die e nach dem [`RadioGroup`](xref:Android.Widget.RadioGroup) -Element gruppiert werden, damit nicht mehr als ein Element gleichzeitig ausgewählt werden kann. Diese Logik wird automatisch vom Android-System behandelt. Wenn eine[`RadioButton`](xref:Android.Widget.RadioButton)
Wenn eine Gruppe ausgewählt ist, werden alle anderen automatisch deaktiviert.

Um etwas zu tun, [`RadioButton`](xref:Android.Widget.RadioButton) wenn jedes ausgewählt ist, müssen wir einen Ereignishandler schreiben:

```csharp
private void RadioButtonClick (object sender, EventArgs e)
{
    RadioButton rb = (RadioButton)sender;
    Toast.MakeText (this, rb.Text, ToastLength.Short).Show ();
}
```

Zuerst wird der eingeführte Absender in ein Optionsfeld umgewandelt.
Dann ein[`Toast`](xref:Android.Widget.Toast)
Meldung Hiermit wird der Text des ausgewählten Options Felds angezeigt.

Nun unten im[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
Fügen Sie Folgendes hinzu:

```csharp
RadioButton radio_red = FindViewById<RadioButton>(Resource.Id.radio_red);
RadioButton radio_blue = FindViewById<RadioButton>(Resource.Id.radio_blue);

radio_red.Click += RadioButtonClick;
radio_blue.Click += RadioButtonClick;
```

Dadurch werden alle [`RadioButton`](xref:Android.Widget.RadioButton)e aus dem Layout erfasst und die neu erstellten Ereignishandler hinzugefügt.

Führen Sie die Anwendung aus.

> [!TIP]
> Wenn Sie den Zustand selbst ändern müssen (z. b. beim Laden eines [`CheckBoxPreference`](xref:Android.Preferences.CheckBoxPreference)gespeicherten), verwenden Sie das[`Checked`](xref:Android.Widget.CompoundButton.Checked)
> Eigenschaften Setter oder[`Toggle()`](xref:Android.Widget.CompoundButton.Toggle)
> -Methode.

*Teile dieser Seite sind Änderungen, die auf der vom Android Open Source-Projekt erstellten und freigegebenen Arbeit basieren und gemäß den in der*
[*Creative Commons 2,5-Zuweisungs Lizenz*](http://creativecommons.org/licenses/by/2.5/)beschriebenen Begriffen verwendet werden. 
