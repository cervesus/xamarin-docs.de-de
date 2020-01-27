---
title: Text bearbeiten
description: Verwenden des EDITTEXT-Widgets, um Benutzereingaben zu akzeptieren.
ms.prod: xamarin
ms.assetid: E513BCBC-438E-15E8-B83A-4B768A8E8B32
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/09/2018
ms.openlocfilehash: 6180896002d19c51bce47bf53aaecdc11b0cae6e
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725149"
---
# <a name="xamarinandroid-edit-text"></a>Xamarin. Android-Bearbeitungs Text

In diesem Abschnitt verwenden Sie das [EDITTEXT](xref:Android.Widget.EditText) -Widget, um ein Textfeld für Benutzereingaben zu erstellen. Nachdem Text in das Feld eingegeben wurde, wird in der **Eingabe** Taste der Text in einer Popup Meldung angezeigt.

Öffnen Sie **Resources/Layout/activity_main. axml** , und fügen Sie das [EDITTEXT](xref:Android.Widget.EditText) -Element einem enthaltenden Layout hinzu. Das folgende Beispiel **activity_main. axml** verfügt über eine `EditText`, die einer `LinearLayout`hinzugefügt wurde:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <EditText
        android:id="@+id/edittext"
        android:layout_width="match_parent"
        android:imeOptions="actionGo"
        android:inputType="text"
        android:layout_height="wrap_content" />
</LinearLayout>
```

In diesem Codebeispiel wird das `EditText`-Attribut `android:imeOptions` auf `actionGo`festgelegt. Mit dieser Einstellung wird die Standard- [done](https://developer.android.com/reference/android/view/inputmethod/EditorInfo#IME_ACTION_DONE) -Aktion in die [go](https://developer.android.com/reference/android/view/inputmethod/EditorInfo#IME_ACTION_GO) -Aktion geändert, sodass das Tippen auf die **EINGABETASTE den** `KeyPress` Eingabe Handlers auslöst.
(In der Regel wird `actionGo` verwendet, damit die **Eingabe** Taste den Benutzer zum Ziel einer URL, die eingegeben wird, verwendet.)

Fügen Sie den folgenden Code am Ende der [OnCreate](xref:Android.App.Activity.OnCreate*) -Methode in **MainActivity.cs**ein, um Benutzer Texteingaben zu behandeln:

```csharp
EditText edittext = FindViewById<EditText>(Resource.Id.edittext);
edittext.KeyPress += (object sender, View.KeyEventArgs e) => {
    e.Handled = false;
    if (e.Event.Action == KeyEventActions.Down && e.KeyCode == Keycode.Enter)
    {
        Toast.MakeText(this, edittext.Text, ToastLength.Short).Show();
        e.Handled = true;
    }
};
```

Fügen Sie außerdem die folgende `using`-Anweisung am Anfang von **MainActivity.cs** hinzu, wenn Sie nicht bereits vorhanden ist:

```csharp
using Android.Views;
```

In diesem Codebeispiel wird das [EDITTEXT](xref:Android.Widget.EditText) -Element aus dem Layout vergrößert und ein [KeyPress](xref:Android.Views.View.KeyPress) -Handler hinzugefügt, der die Aktion definiert, die durchgeführt wird, wenn eine Taste gedrückt wird, während das Widget den Fokus besitzt. In diesem Fall wird die-Methode so definiert, dass **Sie die Eingabe** Taste (beim Tippen) abhört und dann [eine Popup](xref:Android.Widget.Toast) Meldung mit dem eingegebenen Text öffnet. Beachten Sie, dass die [behandelte](xref:Android.Views.View.KeyEventArgs.Handled) Eigenschaft immer `true` werden sollte, wenn das Ereignis behandelt wurde. Dies ist erforderlich, um zu verhindern, dass das Ereignis nach oben blindelt (was zu einem Wagen Rücklauf im Textfeld führen würde).

Führen Sie die Anwendung aus, und geben Sie Text in das Textfeld ein. Wenn Sie die **Eingabe** Taste drücken, wird der Toast wie auf der rechten Seite angezeigt:

[![Beispiele für das Eingeben von Text in EDITTEXT](edit-text-images/edit-text-sml.png)](edit-text-images/edit-text.png#lightbox)

*Teile dieser Seite sind Änderungen, die auf der vom Android Open Source-Projekt erstellten und freigegebenen Arbeit basieren und gemäß den in der* [*Creative Commons 2,5-Zuweisungs Lizenz*](https://creativecommons.org/licenses/by/2.5/) beschriebenen Begriffen verwendet werden *. Dieses Tutorial basiert auf dem* Android-Lernprogramm für [*Formular Inhalte*](https://developer.android.com/resources/tutorials/views/hello-formstuff.html) *.*

## <a name="related-links"></a>Verwandte Themen

- [EditTextSample](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-edittextsample)
