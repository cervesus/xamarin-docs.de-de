---
title: Text bearbeiten
description: So verwenden Sie das Widget EditText Benutzereingaben akzeptieren.
ms.prod: xamarin
ms.assetid: E513BCBC-438E-15E8-B83A-4B768A8E8B32
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/09/2018
ms.openlocfilehash: bb2cb13472e7e17eb1b0438ed67033f2b04defe2
ms.sourcegitcommit: b6f3e55d4f3dcdc505abc8dc9241cff0bb5bd154
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2018
ms.locfileid: "40251067"
---
# <a name="edit-text"></a>Text bearbeiten

In diesem Abschnitt verwenden Sie die [EditText](https://developer.xamarin.com/api/type/Android.Widget.EditText/) Widgets zu einem Textfeld Benutzer zur Eingabe erstellen. Nach Text in das Feld eingegeben wurde die **EINGABETASTE** Schlüssel wird den Text in einer eingeblendeten Nachricht anzuzeigen.

Open **Resources/layout/activity_main.axml** und Hinzufügen der [EditText](https://developer.xamarin.com/api/type/Android.Widget.EditText/) Element zu einem Layout enthält. Im folgenden Beispiel **activity_main.axml** verfügt über eine `EditText` , die hinzugefügt wurde eine `LinearLayout`:

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

In diesem Codebeispiel wird die `EditText` Attribut `android:imeOptions` nastaven NA hodnotu `actionGo`. Diese Einstellung ändert den Standard [durchgeführt](https://developer.android.com/reference/android/view/inputmethod/EditorInfo#IME_ACTION_DONE) Aktion aus, um die [wechseln Sie](https://developer.android.com/reference/android/view/inputmethod/EditorInfo#IME_ACTION_GO) Aktion, damit durch Tippen auf die **EINGABETASTE** Schlüssel Trigger die `KeyPress` eingabehandler.
(In der Regel `actionGo` wird verwendet, damit die **EINGABETASTE** Schlüssel wird der Benutzer auf das Ziel eine URL, die in eingegeben wird.)

Um Text Benutzereingaben zu behandeln, fügen Sie den folgenden Code am Ende der [OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/) -Methode in der **"mainactivity.cs"**:

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

Darüber hinaus fügen Sie die folgenden `using` Anweisung am Anfang **"mainactivity.cs"** ist dies nicht bereits vorhanden:

```csharp
using Android.Views;
```

In diesem Codebeispiel wird vergrößert die [EditText](https://developer.xamarin.com/api/type/Android.Widget.EditText/) Element aus dem Layout und fügt eine [KeyPress](https://developer.xamarin.com/api/event/Android.Views.View.KeyPress/) Handler, der definiert, die Aktion, die vorgenommen werden, wenn eine Taste gedrückt wird, während das Widget den Fokus besitzt. In diesem Fall wird die Methode zum Abhören von definiert die **EINGABETASTE** Schlüssel (wenn es sich um eine angetippt), und klicken Sie dann angezeigt ein [Toast](https://developer.xamarin.com/api/type/Android.Widget.Toast/) Nachricht mit dem Text, der eingegeben wurde. Beachten Sie, dass die [behandelt](https://developer.xamarin.com/api/property/Android.Views.View+KeyEventArgs.Handled/) Eigenschaft muss immer `true` , wenn das Ereignis behandelt wurde. Dies ist erforderlich, um zu verhindern, dass das Ereignis bubbling oben (Dies wird ein Wagenrücklauf in das Textfeld führen würde).

Führen Sie die Anwendung, und geben Sie Text in das Textfeld ein. Beim Drücken der **EINGABETASTE** Schlüssel, der Toast angezeigt, wie auf der rechten Seite dargestellt:

[![Beispiele für die Eingabe von Text in EditText](edit-text-images/edit-text-sml.png)](edit-text-images/edit-text.png#lightbox)

*Teile dieser Seite werden Änderungen, die basierend auf der Arbeit, die erstellt und* [ *freigegeben, indem Sie das Android Open Source-Projekt* ](http://code.google.com/policies.html) *und gemäß den Bedingungen, die in derbeschriebenenverwendet* [ *Attribution-Lizenz Creative Commons 2.5* ](http://creativecommons.org/licenses/by/2.5/) *. Dieses Tutorial basiert auf der* [ *Android Formular Dinge Tutorial* ](http://developer.android.com/resources/tutorials/views/hello-formstuff.html) *.*


## <a name="related-links"></a>Verwandte Links

- [EditTextSample](https://developer.xamarin.com/samples/monodroid/UserInterface/EditTextSample/)
