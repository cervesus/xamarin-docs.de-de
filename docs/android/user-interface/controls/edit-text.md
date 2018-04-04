---
title: Text bearbeiten
ms.prod: xamarin
ms.assetid: E513BCBC-438E-15E8-B83A-4B768A8E8B32
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: d6be8ae1587742a8c2a37b22b2da3701187dde2a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="edit-text"></a>Text bearbeiten

In diesem Abschnitt erstellen Sie ein Textfeld für die Benutzereingabe, die [EditText](https://developer.xamarin.com/api/type/Android.Widget.EditText/) Widget. Nach der Eingabe von Text in das Feld "EINGABETASTE" der Text wird in einer Toast-Nachricht angezeigt.

Öffnen der <code>Resources\layout\main.xml</code> und fügen die [EditText](https://developer.xamarin.com/api/type/Android.Widget.EditText/) Element (innerhalb der [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)):

```xml
<pre><code class=" syntax brush-C#">&lt;EditText
    android:id="@+id/edittext"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"/&gt;</code></pre>
```

Möchten Sie etwas mit dem Text, die vom Benutzer eingegebenen, den folgenden Code am Ende hinzufügen der [OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/) Methode:

```csharp
EditText edittext = FindViewById<EditText>(Resource.Id.edittext);
edittext.KeyPress += (object sender, View.KeyEventArgs e) => {
 e.Handled = false;
 if (e.Event.Action == KeyEventActions.Down && e.KeyCode == Keycode.Enter) {
  Toast.MakeText (this, edittext.Text, ToastLength.Short).Show ();
  e.Handled = true;
         }
};
```

Zeichnet die [ `EditText` ](https://developer.xamarin.com/api/type/Android.Widget.EditText/) Element aus dem Layout und legt ein [ `KeyPress` ](https://developer.xamarin.com/api/event/Android.Views.View.KeyPress/) Handler, der definiert, die Aktion, die vorgenommen werden, wenn eine Taste gedrückt wird, während das Widget den Fokus besitzt. In diesem Fall wird die Methode zum Lauschen auf die EINGABETASTE (wenn gedrückt), und klicken Sie dann Popup-definiert eine [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) Nachricht mit dem Text, der eingegeben wurde. Die [ `Handled` ](https://developer.xamarin.com/api/property/Android.Views.View+KeyEventArgs.Handled/) Eigenschaft muss immer `true` , wenn das Ereignis behandelt wurde, sodass das Ereignis Blase von Volltextkatalogen nicht (die einen Wagenrücklauf in das Textfeld "zur Folge hätte).

Führen Sie die Anwendung aus.

*Teile dieser Seite werden die Änderungen, die basierend auf der Arbeit, die erstellt und* [ *von Android Open Source-Projekt gemeinsam* ](http://code.google.com/policies.html) *und entsprechend in derbeschriebenenBegriffeverwendet* [ *Lizenz der Creative Commons 2.5-Namensnennung* ](http://creativecommons.org/licenses/by/2.5/) *. Dieses Lernprogramm basiert auf der* [ *Android Formular Stuff-Lernprogramm* ](http://developer.android.com/resources/tutorials/views/hello-formstuff.html) *.*



## <a name="related-links"></a>Verwandte Links

- [EditTextSample](https://developer.xamarin.com/samples/monodroid/UserInterface/EditTextSample/)
