---
title: Hinzufügen von AppCompat und Material Design
description: In diesem Artikel wird erläutert, wie vorhandenen Xamarin.Forms-Android-apps zur Verwendung von AppCompat und Material Design konvertiert wird.
ms.prod: xamarin
ms.assetid: 045FBCDF-4D45-48BB-9911-BD3938C87D58
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2017
ms.openlocfilehash: cade72aaad60c30993f6b11e98704addd218ffae
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61293447"
---
# <a name="adding-appcompat-and-material-design"></a>Hinzufügen von AppCompat und Material Design

_Gehen Sie zum Konvertieren von vorhandenen Xamarin.Forms-Android-apps zur Verwendung von AppCompat und Material Design_

<!-- source https://gist.github.com/jassmith/a3b2a543f99126782936
https://blog.xamarin.com/material-design-for-your-xamarin-forms-android-apps/ -->

## <a name="overview"></a>Übersicht

Diese Anleitung erläutert das Aktualisieren Ihrer vorhandenen Xamarin.Forms-Android-Anwendungen verwenden Sie die Bibliothek AppCompat und Material Design in der Android-Version von Xamarin.Forms-apps aktivieren.

### <a name="1-update-xamarinforms"></a>1. Aktualisieren von Xamarin.Forms

Stellen Sie sicher, dass die Lösung Xamarin.Forms 2.0 oder höher verwendet wird. Aktualisieren Sie das Xamarin.Forms-Nuget-Paket auf 2.0, falls erforderlich.

### <a name="2-check-android-version"></a>2. Überprüfen Sie die Android-version

Stellen Sie sicher, dass das Zielframework für das Android-Projekt auf Android 6.0 (Marshmallow) ist. Überprüfen Sie die **Android-Projekts > Optionen > Erstellen > Allgemein** Einstellungen, um sicherzustellen, dass das Framework korrigieren ausgewählt ist:

 ![](appcompat-images/target-android-6-sml.png "Allgemein für Android-Build-Konfiguration")

### <a name="3-add-new-themes-to-support-material-design"></a>3. Hinzufügen von neuen Designs zur Unterstützung von Material Design

Erstellen Sie die folgenden drei Dateien in Ihrem Android-Projekt, und fügen Sie in den folgenden Inhalt ein. Google bietet eine [Styleguide zur](http://www.google.com/design/spec/style/color.html#color-color-palette) und ein [Color Palette Generator](http://www.materialpalette.com/) können Sie eine alternative Farbschema in das angegebene auswählen.

**Resources/Values/Colors.Xml**

```xml
<resources>
  <color name="primary">#2196F3</color>
  <color name="primaryDark">#1976D2</color>
  <color name="accent">#FFC107</color>
  <color name="window_background">#F5F5F5</color>
</resources>
```

**Resources/values/style.xml**

```xml
<resources>
  <style name="MyTheme" parent="MyTheme.Base">
  </style>
  <style name="MyTheme.Base" parent="Theme.AppCompat.Light.NoActionBar">
    <item name="colorPrimary">@color/primary</item>
    <item name="colorPrimaryDark">@color/primaryDark</item>
    <item name="colorAccent">@color/accent</item>
    <item name="android:windowBackground">@color/window_background</item>
    <item name="windowActionModeOverlay">true</item>
  </style>
</resources>
```

Ein zusätzlicher Stil enthalten sein muss, der **Werte-v21** Ordner, um spezifische Eigenschaften anwenden, bei der Ausführung auf Android Lollipop und höher.

**Resources/values-v21/style.xml**

```xml
<resources>
  <style name="MyTheme" parent="MyTheme.Base">
    <!--If you are using MasterDetailPage you will want to set these, else you can leave them out-->
    <!--<item name="android:windowDrawsSystemBarBackgrounds">true</item>
    <item name="android:statusBarColor">@android:color/transparent</item>-->
  </style>
</resources>
```

### <a name="4-update-androidmanifestxml"></a>4. Update AndroidManifest.xml

Um sicherzustellen, dass das neue Design Informationen verwendet wird, legen Design sind die **AndroidManifest** Datei `android:theme="@style/MyTheme"` (lassen Sie den Rest des XML-aus, wie es der Fall war).

**Properties/AndroidManifest.xml**

```xml
...
<application android:label="AppName" android:icon="@drawable/icon"
  android:theme="@style/MyTheme">
...
```

### <a name="5-provide-toolbar-and-tab-layouts"></a>5. Geben Sie die Symbolleiste und die Registerkarte layouts

Erstellen Sie **Tabbar.axml** und **Toolbar.axml** Dateien in die **Ressourcen/Layout** und fügen Sie ihre Inhalte von unten:

**Resources/layout/Tabbar.axml**

```xml
<android.support.design.widget.TabLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/sliding_tabs"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:background="?attr/colorPrimary"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    app:tabIndicatorColor="@android:color/white"
    app:tabGravity="fill"
    app:tabMode="fixed" />
```

Einige Eigenschaften für die Registerkarten der Registerkarte Schwerkraft, einschließlich festgelegt wurden `fill` Modus für die `fixed`.
Wenn Sie viele Registerkarten verfügen Sie möglicherweise diese wechseln möchten, bildlauffähigen - Android lesen [TabLayout Dokumentation](https://developer.android.com/reference/android/support/design/widget/TabLayout.html) um mehr zu erfahren.

**Resources/layout/Toolbar.axml**

```xml
<android.support.v7.widget.Toolbar
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="?attr/actionBarSize"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorPrimary"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    app:popupTheme="@style/ThemeOverlay.AppCompat.Light"
    app:layout_scrollFlags="scroll|enterAlways" />
```

In diesen Dateien erstellen wir bestimmtes Design für die Symbolleiste, die für Ihre Anwendung variieren.
Finden Sie in der [Hello Symbolleiste](https://blog.xamarin.com/android-tips-hello-toolbar-goodbye-action-bar/) Blogbeitrag, um mehr zu erfahren.


### <a name="6-update-the-mainactivity"></a>6. Update der `MainActivity`

In vorhandenen Xamarin.Forms-apps die **"mainactivity.cs"** Klasse erbt von `FormsApplicationActivity`. Dies ersetzt werden muss, mit `FormsAppCompatActivity` auf die neue Funktionalität zu aktivieren.

**MainActivity.cs**

```csharp
public class MainActivity : FormsAppCompatActivity  // was FormsApplicationActivity
```

Zum Schluss "socialsharing.Share" die neue Layouts aus Schritt 5 in der `OnCreate` Methode, wie hier gezeigt:

```csharp
protected override void OnCreate(Bundle bundle)
{
  // set the layout resources first
  FormsAppCompatActivity.ToolbarResource = Resource.Layout.Toolbar;
  FormsAppCompatActivity.TabLayoutResource = Resource.Layout.Tabbar;

  // then call base.OnCreate and the Xamarin.Forms methods
  base.OnCreate(bundle);
  Forms.Init(this, bundle);
  LoadApplication(new App());
}
```
