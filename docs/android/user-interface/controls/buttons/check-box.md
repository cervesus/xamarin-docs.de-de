---
title: CheckBox
ms.prod: xamarin
ms.assetid: A884AF10-D5EA-72CA-2301-B80CEC7FFBE7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: b947217706fc8ef7ce7945bf4c88349f4367ffcd
ms.sourcegitcommit: cf56d2bae34dc0f8e94c2d3d28d5f460d59807bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2019
ms.locfileid: "70985673"
---
# <a name="checkbox"></a>CheckBox

In diesem Abschnitt erstellen Sie ein Kontrollkästchen für die Auswahl von Elementen mithilfe des [`CheckBox`](xref:Android.Widget.CheckBox) Widget verwenden. Wenn das Kontrollkästchen aktiviert ist, gibt eine Popupmeldung den aktuellen Zustand an.

Öffnen Sie die Datei **Resources/Layout/Main. axml**, und fügen Sie das [`CheckBox`](xref:Android.Widget.CheckBox)-Element (innerhalb des [`LinearLayout`](xref:Android.Widget.LinearLayout)-Elements) hinzu:

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

Dadurch wird das [`CheckBox`](xref:Android.Widget.CheckBox) -Element aus dem Layout erfasst und dann das Click-Ereignis behandelt, das die Aktion definiert, die beim Klicken auf das Kontrollkästchen durchgeführt werden soll. Wenn Sie darauf klicken [`Checked`](xref:Android.Widget.CompoundButton.Checked) , wird die-Eigenschaft aufgerufen, um den neuen Zustand des Kontrollkästchens zu überprüfen. Wenn das Kontrollkästchen aktiviert ist, [`Toast`](xref:Android.Widget.Toast) wird die Meldung "ausgewählt" angezeigt, andernfalls wird "nicht ausgewählt" angezeigt. Der [`CheckBox`](xref:Android.Widget.CheckBox) verarbeitet seine eigenen Zustandsänderungen, sodass Sie nur den aktuellen Statusabfragen müssen.

Führen Sie es aus.

> [!TIP]
> Wenn Sie den Zustand selbst ändern müssen (z. B. beim Laden einer gespeicherten [`CheckBoxPreference`](xref:Android.Preferences.CheckBoxPreference)-Klasse), verwenden Sie den Setter der [`Checked`](xref:Android.Widget.CompoundButton.Checked)-Eigenschaft oder die [`Toggle()`](xref:Android.Widget.CompoundButton.Toggle)-Methode.

*Teile dieser Seite sind Änderungen, die auf der vom Android Open Source-Projekt erstellten und freigegebenen Arbeit basieren und gemäß den Begriffen verwendet werden, die im* folgenden Abschnitt beschrieben werden. [*Creative Commons 2,5-Zuweisungs Lizenz*](http://creativecommons.org/licenses/by/2.5/).
