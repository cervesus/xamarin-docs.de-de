---
title: Navigationsleiste
ms.topic: article
ms.prod: xamarin
ms.assetid: 6023DB7E-9E72-4B90-A96A-11BC297B8A3D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/01/2017
ms.openlocfilehash: fe76c93afc149553e44b5e8fa29a21767becf5c5
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="navigation-bar"></a>Navigationsleiste

Android 4 eingeführt, ein neues System Benutzer Schnittstelle Feature Namens eine *Navigationsleiste*, stellt Steuerelemente für die Seitennavigation auf Geräten, die keine Hardwaretasten für enthalten **Home**, **zurück** , und **Menü**.
Der folgende Screenshot zeigt die Navigationsleiste über ein Gerät Nexus Primzahlen:

 [![Beispiel für ein Android-Navigationsleiste](navigation-bar-images/19-navbar.png)](navigation-bar-images/19-navbar.png#lightbox)

Mehrere neue Flags sind verfügbar, die steuern, die Sichtbarkeit der Navigationsleiste und seiner Steuerelemente sowie die Sichtbarkeit der System-Leiste, die in Android 3 eingeführt wurde. Die Flags werden definiert, der `Android.View.View` -Klasse und sind nachfolgend aufgeführt:

-   `SystemUiFlagVisible` &ndash; Macht die Navigationsleiste sichtbar. 
-   `SystemUiFlagLowProfile` &ndash; DIMS Steuerelemente in der Navigationsleiste. 
-   `SystemUiFlagHideNavigation` &ndash; Blendet die Navigationsleiste. 


Diese Flags können auf einer beliebigen Ansicht in der Ansichtshierarchie angewendet werden, durch Festlegen der `SystemUiVisibility` Eigenschaft. Wenn mehrere Ansichten auf diese Eigenschaft festgelegt haben, wird das System mit einer OR-Operation kombiniert und wendet sie an, solange das Fenster, in dem die Flags festgelegt sind, den Fokus beibehalten. Wenn Sie eine Ansicht entfernen, werden auch Flags festgelegt wurde entfernt.

Das folgende Beispiel zeigt eine einfache Anwendung, in denen Änderungen durch Klicken auf die Schaltflächen der `SystemUiVisibility`:

 [![Screenshots veranschaulicht sichtbar, flaches und SystemUiVisibility ausgeblendet](navigation-bar-images/18-systemuivisibility.png)](navigation-bar-images/18-systemuivisibility.png#lightbox)

Der Code zum Ändern der `SystemUiVisibility` wird die Eigenschaft eine `TextView` click-Ereignishandler von einzelnen Schaltflächen wie unten dargestellt:

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

Darüber hinaus eine `SystemUiVisibility` ändern löst eine `SystemUiVisibilityChange` Ereignis. Wie Sie die Einstellung der `SystemUiVisibility` Eigenschaft, einen Handler für das `SystemUiVisibilityChange` Ereignis registriert werden kann, für jede Ansicht in der Hierarchie. Z. B. der Code unten verwendet die `TextView` Instanz für das Ereignis zu registrieren:

```csharp
tv.SystemUiVisibilityChange +=
  delegate(object sender, View.SystemUiVisibilityChangeEventArgs e) {
        tv.Text = String.Format ("Visibility = {0}", e.Visibility);
  };
```



## <a name="related-links"></a>Verwandte Links

- [SystemUIVisibilityDemo (Beispiel)](https://developer.xamarin.com/samples/monodroid/SystemUIVisibilityDemo/)
- [Einführung in Eis Rustikal Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 Platform](http://developer.android.com/sdk/android-4.0.html)
