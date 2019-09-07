---
title: Xamarin. Android-Schalter
description: Verwenden des switchwidgets in einer xamarin. Android-Anwendung
ms.prod: xamarin
ms.assetid: 6E1F3324-EC41-454A-AEC0-0208813C7E50
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/29/2018
ms.openlocfilehash: 82271711864363bbaf593e8c44e31632048399dc
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70764881"
---
# <a name="xamarinandroid-switch"></a>Xamarin. Android-Schalter

Mit `Switch` dem Widget (unten dargestellt) kann ein Benutzer zwischen zwei Zuständen umschalten, z. b. ein-oder ausschalten. Der `Switch` Standardwert ist off. Das Widget wird unten in den Status "ein" und "aus" angezeigt:

[![Screenshots eines switchwidgets in den Zuständen off und on](switch-images/16-switch-onoff.png)](switch-images/16-switch-onoff.png#lightbox)

## <a name="creating-a-switch"></a>Erstellen eines Schalters

Zum Erstellen eines Schalters deklarieren Sie `Switch` einfach ein-Element in XML wie folgt:

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
```

Dadurch wird ein einfacher Schalter erstellt, wie unten dargestellt:

[![Screenshot der Demo-App, die einen Switch im Zustand "aus" anzeigt](switch-images/07-switch.png)](switch-images/07-switch.png#lightbox)

## <a name="changing-default-values"></a>Ändern von Standardwerten

Sowohl der Text, den das Steuerelement für den Status ein und aus anzeigt, als auch der Standardwert sind konfigurierbar. Um z. b. für den Switch standardmäßig on und Read No/Yes anstelle von off/on festzulegen, können `checked`wir `textOn`die Attribute `textOff` , und im folgenden XML-Code festlegen.

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```

## <a name="providing-a-title"></a>Bereitstellen eines Titels

Das `Switch` Widget unterstützt auch das Einschließen einer Text Bezeichnung, `text` indem das-Attribut wie folgt festgelegt wird:

```xml
<Switch android:text="Is Xamarin.Android great?"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```

Dieses Markup erzeugt den folgenden Screenshot zur Laufzeit:

[![Screenshot der Demo-App mit horizontaler Text vor dem switchwidget](switch-images/08-switch.png)](switch-images/08-switch.png#lightbox)

Wenn ein `Switch`-Wert geändert wird, wird ein `CheckedChange` -Ereignis ausgelöst.
`Toast` Im folgenden Code erfassen `isChecked` Siez`Switch`. b. dieses Ereignis und stellen ein Widget mit einer Nachricht auf Grundlage des Werts von dar, der als Teil des- ArgumentsandenEreignishandlerübermitteltwird.`CompoundButton.CheckedChangeEventArg`

```csharp
Switch s = FindViewById<Switch> (Resource.Id.monitored_switch);
           
s.CheckedChange += delegate(object sender, CompoundButton.CheckedChangeEventArgs e) {
    var toast = Toast.MakeText (this, "Your answer is " +
        (e.IsChecked ?  "correct" : "incorrect"), ToastLength.Short);
    toast.Show ();
};
```

## <a name="related-links"></a>Verwandte Links

- [Switchdemo (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/switchdemo)
- [Tutorial zur Registerkarten Layout](~/android/user-interface/layouts/tab-layout/index.md)
