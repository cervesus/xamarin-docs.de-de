---
title: Xamarin. Android-Navigationsleiste
ms.prod: xamarin
ms.assetid: 6023DB7E-9E72-4B90-A96A-11BC297B8A3D
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/01/2017
ms.openlocfilehash: 99d0303dc1560796cb372d0b8af2fafd16c6097f
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725140"
---
# <a name="xamarinandroid-navigation-bar"></a>Xamarin. Android-Navigationsleiste

Mit Android 4 wurde eine neue Systembenutzer Oberfläche eingeführt, die als *Navigationsleiste*bezeichnet wird, die Navigations Steuerelemente auf Geräten bereitstellt, die keine Hardware Schaltflächen für **Home**, **Back**und **Menü**enthalten.
Der folgende Screenshot zeigt die Navigationsleiste von einem Nexus-Prim Gerät:

 [![Beispiel für eine Android-Navigationsleiste](navigation-bar-images/19-navbar.png)](navigation-bar-images/19-navbar.png#lightbox)

Es sind mehrere neue Flags verfügbar, die die Sichtbarkeit der Navigationsleiste und deren Steuerelemente steuern, sowie die Sichtbarkeit der System Leiste, die in Android 3 eingeführt wurde. Die Flags werden in der `Android.View.View`-Klasse definiert und sind im folgenden aufgeführt:

- mit `SystemUiFlagVisible` &ndash; wird die Navigationsleiste sichtbar.
- `SystemUiFlagLowProfile` &ndash; die Steuerelemente in der Navigationsleiste aus.
- `SystemUiFlagHideNavigation` &ndash; die Navigationsleiste ausgeblendet.

Diese Flags können auf jede Sicht in der Ansichts Hierarchie angewendet werden, indem die `SystemUiVisibility`-Eigenschaft festgelegt wird. Wenn diese Eigenschaft in mehreren Ansichten festgelegt ist, werden Sie vom System mit einer OR-Operation kombiniert und angewendet, solange das Fenster, in dem die Flags festgelegt sind, den Fokus erhält. Wenn Sie eine Ansicht entfernen, werden alle festgelegten Flags ebenfalls entfernt.

Das folgende Beispiel zeigt eine einfache Anwendung, bei der durch Klicken auf eine der Schaltflächen die `SystemUiVisibility`geändert wird:

 [![Screenshots, die sichtbare, niedrige Profile und verborgene System Sichtbarkeit veranschaulichen](navigation-bar-images/18-systemuivisibility.png)](navigation-bar-images/18-systemuivisibility.png#lightbox)

Mit dem Code zum Ändern des `SystemUiVisibility` wird die-Eigenschaft auf einem `TextView` von jedem Click-Ereignishandler der Schaltfläche festgelegt, wie unten dargestellt:

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

Außerdem löst eine `SystemUiVisibility` Änderung ein `SystemUiVisibilityChange` Ereignis aus. Ebenso wie das Festlegen der `SystemUiVisibility`-Eigenschaft kann ein Handler für das `SystemUiVisibilityChange` Ereignis für jede Ansicht in der Hierarchie registriert werden. Der folgende Code verwendet beispielsweise die `TextView`-Instanz, um sich für das-Ereignis zu registrieren:

```csharp
tv.SystemUiVisibilityChange +=
  delegate(object sender, View.SystemUiVisibilityChangeEventArgs e) {
        tv.Text = String.Format ("Visibility = {0}", e.Visibility);
  };
```

## <a name="related-links"></a>Verwandte Themen

- [Systemuivisibilitydemo (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/systemuivisibilitydemo)
