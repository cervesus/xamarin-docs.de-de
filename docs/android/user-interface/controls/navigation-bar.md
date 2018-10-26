---
title: Navigationsleiste
ms.prod: xamarin
ms.assetid: 6023DB7E-9E72-4B90-A96A-11BC297B8A3D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/01/2017
ms.openlocfilehash: ce80fab39c814204631c5cc408c3f0ee99a329e6
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110556"
---
# <a name="navigation-bar"></a>Navigationsleiste

Android 4 eingeführt wurde, ein neues Systemfeature der Benutzeroberfläche wird aufgerufen, eine *Navigationsleiste*, die Steuerelemente für die Seitennavigation auf Geräten, die keine Hardwaretasten für enthalten bietet **Startseite**, **zurück** , und **Menü**.
Der folgende Screenshot zeigt der Navigationsleiste auf einem Nexus Prime-Gerät:

 [![Beispiel für eine Android-Navigation-Leiste](navigation-bar-images/19-navbar.png)](navigation-bar-images/19-navbar.png#lightbox)

Mehrere neue Optionen zur Verfügung stehen, die steuern, die Sichtbarkeit von der Navigationsleiste und seiner Steuerelemente sowie die Sichtbarkeit der System-Leiste, die in Android 3 eingeführt wurde. Die Flags werden definiert, der `Android.View.View` Klasse und sind nachfolgend aufgeführt:

-   `SystemUiFlagVisible` &ndash; Die Navigationsleiste macht sichtbar. 
-   `SystemUiFlagLowProfile` &ndash; DIMS Steuerelemente in der Navigationsleiste auf. 
-   `SystemUiFlagHideNavigation` &ndash; Blendet die Navigationsleiste aus. 


Diese Flags können auf einer beliebigen Ansicht in der Hierarchie von Inhaltsansichten angewendet werden, durch Festlegen der `SystemUiVisibility` Eigenschaft. Wenn mehrere Ansichten auf diese Eigenschaft festgelegt haben, wird das System mit einer OR-Operation kombiniert und wendet sie an, solange das Fenster, in dem die Flags festgelegt sind, den Fokus beibehalten. Wenn Sie eine Ansicht entfernen, werden alle Flags, die sie festgelegt hat auch entfernt werden.

Das folgende Beispiel zeigt eine einfache Anwendung, in denen Änderungen auf eine der Schaltflächen der `SystemUiVisibility`:

 [![Screenshots veranschaulicht, sichtbar und flacher SystemUiVisibility ausgeblendet](navigation-bar-images/18-systemuivisibility.png)](navigation-bar-images/18-systemuivisibility.png#lightbox)

Der Code zum Ändern der `SystemUiVisibility` wird die Eigenschaft eine `TextView` aus jeder der Schaltfläche die click-Ereignishandler aus, wie unten dargestellt:

```csharp
var tv = FindViewById<TextView> (Resource.Id.systemUiFlagTextView);
var lowProfileButton = FindViewById<Button>(Resource.Id.lowProfileButton);
var hideNavButton = FindViewById<Button> (Resource.Id.hideNavigation);
var visibleButton = FindViewById<Button> (Resource.Id.visibleButton);
           
lowProfileButton.Click += delegate {
    tv.SystemUiVisibility =
        (StatusBarVisibility)View.SystemUiFlagLowProfile;
};
           
hideNavButton.Click += delegate {
    tv.SystemUiVisibility =
       (StatusBarVisibility)View.SystemUiFlagHideNavigation;        
};
           
visibleButton.Click += delegate {
    tv.SystemUiVisibility = (StatusBarVisibility)View.SystemUiFlagVisible;
}
```

Darüber hinaus eine `SystemUiVisibility` ändern löst eine `SystemUiVisibilityChange` Ereignis. Wie Sie die Einstellung der `SystemUiVisibility` -Eigenschaft, einen Handler für die `SystemUiVisibilityChange` Ereignis für jede Ansicht in der Hierarchie registriert werden kann. Beispielsweise der folgende Code verwendet die `TextView` -Instanz, für die Veranstaltung zu registrieren:

```csharp
tv.SystemUiVisibilityChange +=
  delegate(object sender, View.SystemUiVisibilityChangeEventArgs e) {
        tv.Text = String.Format ("Visibility = {0}", e.Visibility);
  };
```



## <a name="related-links"></a>Verwandte Links

- [SystemUIVisibilityDemo (Beispiel)](https://developer.xamarin.com/samples/monodroid/SystemUIVisibilityDemo/)
- [Einführung in die Ice Cream Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0-Plattform](http://developer.android.com/sdk/android-4.0.html)
