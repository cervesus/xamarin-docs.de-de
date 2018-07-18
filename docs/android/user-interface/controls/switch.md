---
title: Schalter
description: Verwenden Sie das Widget "Switch" in einer Xamarin.Android-Anwendung
ms.prod: xamarin
ms.assetid: 6E1F3324-EC41-454A-AEC0-0208813C7E50
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/29/2018
ms.openlocfilehash: e3bcce48a675a9ba3d1d41f93babc7fcb26448c8
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403275"
---
# <a name="switch"></a>Schalter

Die `Switch` Widget (siehe unten) ermöglicht Benutzern das Umschalten zwischen zwei Zuständen, z. B. auf oder deaktiviert. Die `Switch` Standardwert ist OFF. Das Widget ist in ihrer ON und OFF-Status dargestellt:

[![Screenshots von einem Switch-Widget und Status](switch-images/16-switch-onoff.png)](switch-images/16-switch-onoff.png#lightbox)


## <a name="creating-a-switch"></a>Erstellen einen Switch

Um einen Switch zu erstellen, deklarieren Sie einfach eine `Switch` -Element im XML-Code wie folgt:

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
```

Dadurch wird einen grundlegende Switch erstellt, wie unten dargestellt:

[![Bildschirmabbildung von Demo-App, die einen Switch im Zustand "OFF" anzeigen](switch-images/07-switch.png)](switch-images/07-switch.png#lightbox)


## <a name="changing-default-values"></a>Ändern der Standardwerte

Der Text, den im Steuerelement angezeigt wird, für die ON und OFF gibt an, und der Standardwert sind konfigurierbar. Z. B. Damit wird den Switch standardmäßig auf ON, und Lesen Nein Ja/statt auf ein/aus, wir können festlegen, die `checked`, `textOn`, und `textOff` Attribute in den folgenden XML-Code.

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```



## <a name="providing-a-title"></a>Angabe eines Titels

Die `Switch` Widget unterstützt auch, einschließlich einer textbezeichnung durch Festlegen der `text` -Attribut wie folgt:

```xml
<Switch android:text="Is Xamarin.Android great?"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```

Dieses Markup wird der folgende Screenshot zeigt die zur Laufzeit:

[![Screenshot des Demo-app mit Text horizontal vor der Switch-widget](switch-images/08-switch.png)](switch-images/08-switch.png#lightbox)

Wenn eine `Switch`des geändert wird, löst es eine `CheckedChange` Ereignis.
Z. B. in den folgenden Code wir dieses Ereignis erfassen, und stellen eine `Toast` Widget mit einer Nachricht basierend auf der `isChecked` Wert `Switch`, die an den Ereignishandler übergeben wird, als Teil der `CompoundButton.CheckedChangeEventArg` Argument.

```csharp
Switch s = FindViewById<Switch> (Resource.Id.monitored_switch);
           
s.CheckedChange += delegate(object sender, CompoundButton.CheckedChangeEventArgs e) {
    var toast = Toast.MakeText (this, "Your answer is " +
        (e.IsChecked ?  "correct" : "incorrect"), ToastLength.Short);
    toast.Show ();
};
```


## <a name="related-links"></a>Verwandte Links

- [SwitchDemo (Beispiel)](https://developer.xamarin.com/samples/monodroid/SwitchDemo/)
- [Registerkarte Layout-Tutorial](~/android/user-interface/layouts/tab-layout/index.md)
