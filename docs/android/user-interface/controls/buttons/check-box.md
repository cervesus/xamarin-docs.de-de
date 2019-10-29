---
title: CheckBox
ms.prod: xamarin
ms.assetid: A884AF10-D5EA-72CA-2301-B80CEC7FFBE7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: 06908ad8993eb6d47476006b23865fa1c7fe694f
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029343"
---
# <a name="checkbox"></a>CheckBox

In diesem Abschnitt erstellen Sie ein Kontrollkästchen für die Auswahl von Elementen mithilfe des [`CheckBox`](xref:Android.Widget.CheckBox) Widget verwenden. Wenn das Kontrollkästchen gedrückt ist, gibt eine Popup Meldung den aktuellen Zustand des Kontrollkästchens an.

Öffnen Sie die Datei **Resources/Layout/Main. axml** , und fügen Sie das [`CheckBox`](xref:Android.Widget.CheckBox) -Element (innerhalb des [`LinearLayout`](xref:Android.Widget.LinearLayout)) hinzu:

```xml
<CheckBox android:id="@+id/checkbox"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="check it out" />
```

Fügen Sie den folgenden Code am Ende der [`OnCreate()`](xref:Android.App.Activity.OnCreate*) -Methode hinzu, um etwas zu tun, wenn der Status geändert wird:

```csharp
CheckBox checkbox = FindViewById<CheckBox>(Resource.Id.checkbox);

checkbox.Click += (o, e) => {
    if (checkbox.Checked)
        Toast.MakeText (this, "Selected", ToastLength.Short).Show ();
    else
        Toast.MakeText (this, "Not selected", ToastLength.Short).Show ();
};
```

Dadurch wird das [`CheckBox`](xref:Android.Widget.CheckBox) -Element aus dem Layout erfasst und dann das Click-Ereignis behandelt, das die Aktion definiert, die beim Klicken auf das Kontrollkästchen durchgeführt werden soll. Wenn Sie darauf klicken, wird die [`Checked`](xref:Android.Widget.CompoundButton.Checked) -Eigenschaft aufgerufen, um den neuen Zustand des Kontrollkästchens zu überprüfen. Wenn das Kontrollkästchen aktiviert ist, wird in einem [`Toast`](xref:Android.Widget.Toast) die Meldung "ausgewählt" angezeigt, andernfalls wird "nicht ausgewählt" angezeigt. Der [`CheckBox`](xref:Android.Widget.CheckBox) verarbeitet seine eigenen Zustandsänderungen, sodass Sie nur den aktuellen Statusabfragen müssen.

Führen Sie es aus.

> [!TIP]
> Wenn Sie den Zustand selbst ändern müssen (z. b. beim Laden eines gespeicherten [`CheckBoxPreference`](xref:Android.Preferences.CheckBoxPreference), verwenden Sie den [`Checked`](xref:Android.Widget.CompoundButton.Checked) -Eigenschaften Setter oder die [`Toggle()`](xref:Android.Widget.CompoundButton.Toggle) -Methode.

*Teile dieser Seite sind Änderungen, die auf der vom Android Open Source-Projekt erstellten und freigegebenen Arbeit basieren und gemäß den in der* [*Creative Commons 2,5-Zuweisungs Lizenz*](https://creativecommons.org/licenses/by/2.5/)beschriebenen Begriffen verwendet werden.
