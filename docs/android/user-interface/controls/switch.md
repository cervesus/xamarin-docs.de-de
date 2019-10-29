---
title: Xamarin. Android-Schalter
description: Verwenden des switchwidgets in einer xamarin. Android-Anwendung
ms.prod: xamarin
ms.assetid: 6E1F3324-EC41-454A-AEC0-0208813C7E50
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/29/2018
ms.openlocfilehash: 73becb5e4424854c9be6cdc3554f6cf93507b9a9
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029140"
---
# <a name="xamarinandroid-switch"></a>Xamarin. Android-Schalter

Das `Switch`-Widget (unten dargestellt) ermöglicht einem Benutzer das Umschalten zwischen zwei Zuständen (z. b. ein-oder ausschalten). Der `Switch` Standardwert ist off. Das Widget wird unten in den Status "ein" und "aus" angezeigt:

[![Screenshots eines switchwidgets in den Zuständen off und on](switch-images/16-switch-onoff.png)](switch-images/16-switch-onoff.png#lightbox)

## <a name="creating-a-switch"></a>Erstellen eines Schalters

Zum Erstellen eines Schalters deklarieren Sie einfach ein `Switch` Element in XML wie folgt:

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
```

Dadurch wird ein einfacher Schalter erstellt, wie unten dargestellt:

[![Screenshot der Demo-App, die einen Switch im Zustand "aus" anzeigt](switch-images/07-switch.png)](switch-images/07-switch.png#lightbox)

## <a name="changing-default-values"></a>Ändern von Standardwerten

Sowohl der Text, den das Steuerelement für den Status ein und aus anzeigt, als auch der Standardwert sind konfigurierbar. Um z. b. für den Switch standardmäßig on und Read No/Yes anstelle von off/on festzulegen, können wir die Attribute `checked`, `textOn`und `textOff` im folgenden XML-Code festlegen.

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```

## <a name="providing-a-title"></a>Bereitstellen eines Titels

Das `Switch`-Widget unterstützt auch das Einschließen einer Text Bezeichnung, indem das `text`-Attribut wie folgt festgelegt wird:

```xml
<Switch android:text="Is Xamarin.Android great?"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```

Dieses Markup erzeugt den folgenden Screenshot zur Laufzeit:

[![Screenshot der Demo-App mit horizontaler Text, der dem switchwidget vorangestellt ist](switch-images/08-switch.png)](switch-images/08-switch.png#lightbox)

Wenn sich der Wert eines `Switch`ändert, löst er ein `CheckedChange` Ereignis aus.
Im folgenden Code erfassen Sie z. b. dieses Ereignis und stellen ein `Toast`-Widget mit einer Meldung auf Grundlage des `isChecked` Werts von `Switch`dar, der als Teil des `CompoundButton.CheckedChangeEventArg` Arguments an den Ereignishandler übermittelt wird.

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
