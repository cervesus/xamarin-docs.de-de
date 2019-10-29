---
title: RadioButton
ms.prod: xamarin
ms.assetid: 3C32EA3F-D917-C988-72C5-A17354DA791E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: 9f51adcbd1accb4f780318cc0853e612ed8e5bed
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029300"
---
# <a name="radiobutton"></a>RadioButton

In diesem Abschnitt erstellen Sie zwei sich gegenseitig ausschließende Options Felder (durch Aktivieren von eins wird die andere deaktiviert), indem Sie die [`RadioGroup`](xref:Android.Widget.RadioGroup)
und [`RadioButton`](xref:Android.Widget.RadioButton)
Pass. Wenn beide Options Felder gedrückt werden, wird eine Popup Meldung angezeigt.

Öffnen Sie die Datei **Resources/Layout/Main. axml** , und fügen Sie zwei [`RadioButton`](xref:Android.Widget.RadioButton)en hinzu, die in einem [`RadioGroup`](xref:Android.Widget.RadioGroup) (innerhalb des [`LinearLayout`](xref:Android.Widget.LinearLayout)) geschachtelt sind:

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

Es ist wichtig, dass die [`RadioButton`](xref:Android.Widget.RadioButton)s durch das [`RadioGroup`](xref:Android.Widget.RadioGroup) -Element gruppiert werden, damit nicht mehr als ein Element gleichzeitig ausgewählt werden kann. Diese Logik wird automatisch vom Android-System behandelt. Wenn eine [`RadioButton`](xref:Android.Widget.RadioButton)
Wenn eine Gruppe ausgewählt ist, werden alle anderen automatisch deaktiviert.

Um etwas zu tun, wenn jedes [`RadioButton`](xref:Android.Widget.RadioButton) ausgewählt ist, müssen wir einen Ereignishandler schreiben:

```csharp
private void RadioButtonClick (object sender, EventArgs e)
{
    RadioButton rb = (RadioButton)sender;
    Toast.MakeText (this, rb.Text, ToastLength.Short).Show ();
}
```

Zuerst wird der eingeführte Absender in ein Optionsfeld umgewandelt.
Dann eine [`Toast`](xref:Android.Widget.Toast)
Meldung Hiermit wird der Text des ausgewählten Options Felds angezeigt.

Nun unten im [`OnCreate()`](xref:Android.App.Activity.OnCreate*)
Fügen Sie Folgendes hinzu:

```csharp
RadioButton radio_red = FindViewById<RadioButton>(Resource.Id.radio_red);
RadioButton radio_blue = FindViewById<RadioButton>(Resource.Id.radio_blue);

radio_red.Click += RadioButtonClick;
radio_blue.Click += RadioButtonClick;
```

Dadurch werden alle [`RadioButton`](xref:Android.Widget.RadioButton)s aus dem Layout erfasst und die neu erstellten Ereignishandler hinzugefügt.

Führen Sie die Anwendung aus.

> [!TIP]
> Wenn Sie den Zustand selbst ändern müssen (z. b. beim Laden einer gespeicherten [`CheckBoxPreference`](xref:Android.Preferences.CheckBoxPreference)), verwenden Sie die [`Checked`](xref:Android.Widget.CompoundButton.Checked)
> Eigenschaften Setter oder [`Toggle()`](xref:Android.Widget.CompoundButton.Toggle)
> -Methode.

*Teile dieser Seite sind Änderungen, die auf der vom Android Open Source-Projekt erstellten und freigegebenen Arbeit basieren und gemäß den in der*
[*Creative Commons 2,5-Zuweisungs Lizenz*](https://creativecommons.org/licenses/by/2.5/)beschriebenen Begriffen verwendet werden. 
