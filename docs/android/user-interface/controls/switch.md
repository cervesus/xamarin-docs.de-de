---
title: Schalter
ms.topic: article
ms.prod: xamarin
ms.assetid: 6E1F3324-EC41-454A-AEC0-0208813C7E50
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: 0cf1c4d749eb85a7e0f4c035e10e2e7a40e0c711
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="switch"></a>Schalter

Die `Switch` Widget (siehe unten) ermöglicht Benutzern das Umschalten zwischen zwei Zuständen, z. B. unter oder deaktiviert. Die `Switch` Standardwert ist OFF. Das Widget wird in ihrer ON und OFF-Status unten gezeigt:

[![Screenshots von Switch-Widgets in deaktivieren und Aktivieren von Zuständen](switch-images/16-switch-onoff.png)](switch-images/16-switch-onoff.png#lightbox)


## <a name="creating-a-switch"></a>Erstellen einen Switch

Um einem Switch zu erstellen, deklarieren Sie einfach eine `Switch` -Element im XML-Code wie folgt:

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
```

Dadurch wird einen grundlegende Switch erstellt, wie unten dargestellt:

[![Screenshot der Demo-app, die einen Schalter im Status "OFF" anzeigen](switch-images/07-switch.png)](switch-images/07-switch.png#lightbox)


## <a name="changing-default-values"></a>Ändern der Standardwerte

Der Text, den vom Steuerelement angezeigt wird, für die ON und OFF angegeben und der Standardwert sind konfigurierbar. Angenommen, um den Switch auf den Standardwert, und Lesen von Nein Ja/anstelle von OFF/ON zu machen, wir können legen die `checked`, `textOn`, und `textOff` Attribute in der folgenden XML-Code.

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```



## <a name="providing-a-title"></a>Bereitstellen eines Titels

Die `Switch` auch Widget unterstützt wird, einschließlich einer textbezeichnung durch Festlegen der `text` -Attribut wie folgt:

```xml
<Switch android:text="Is Xamarin.Android great?"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```

Dieses Markup erstellt der folgende Screenshot zur Laufzeit:

[![Screenshot der Demo-app mit Text horizontal vor der Switch-widget](switch-images/08-switch.png)](switch-images/08-switch.png#lightbox)

Wenn eine `Switch`des geändert wird, löst es eine `CheckedChange` Ereignis.
Z. B. im folgenden Code wird dieses Ereignis erfassen und präsentieren einer `Toast` Widget mit einer Meldung auf Grundlage der `isChecked` Wert `Switch`, der an den Ereignishandler übergeben wird, als Teil der `CompoundButton.CheckedChangeEventArg` Argument.

```csharp
Switch s = FindViewById<Switch> (Resource.Id.monitored_switch);
           
s.CheckedChange += delegate(object sender, CompoundButton.CheckedChangeEventArgs e) {
    var toast = Toast.MakeText (this, "Your answer is " +
        (e.IsChecked ?  "correct" : "incorrect"), ToastLength.Short);
    toast.Show ();
};
```


## <a name="related-links"></a>Verwandte Links

- [SwitchDemo (Beispiel)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ICS_Samples/SwitchDemo/)
- [Registerkarte Layout-Lernprogramm](~/android/user-interface/layouts/tab-layout/index.md)
- [Einführung in Eis Rustikal Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 Platform](http://developer.android.com/sdk/android-4.0.html)
