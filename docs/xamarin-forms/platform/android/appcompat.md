---
title: "Hinzufügen von AppCompat und Material Entwurf"
description: "Befolgen Sie diese Schritte für das Konvertieren von vorhandenen Xamarin.Forms Android-apps zur Verwendung von AppCompat und Material Entwurf"
ms.topic: article
ms.prod: xamarin
ms.assetid: 045FBCDF-4D45-48BB-9911-BD3938C87D58
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2017
ms.openlocfilehash: 3014db91ff87f0e73291595a17dd780b5e3cd3c2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="adding-appcompat-and-material-design"></a>Hinzufügen von AppCompat und Material Entwurf

_Befolgen Sie diese Schritte für das Konvertieren von vorhandenen Xamarin.Forms Android-apps zur Verwendung von AppCompat und Material Entwurf_

<!-- source https://gist.github.com/jassmith/a3b2a543f99126782936
https://blog.xamarin.com/material-design-for-your-xamarin-forms-android-apps/ -->

## <a name="overview"></a>Übersicht

Diese Anleitung erläutert die Aktualisieren der vorhandenen Xamarin.Forms Android-Anwendungen so verwenden Sie die Bibliothek AppCompat und Material Entwurf in der Android-Version Ihrer Xamarin.Forms-Apps aktivieren.

### <a name="1-update-xamarinforms"></a>1. Xamarin.Forms aktualisieren

Stellen Sie sicher, dass die Lösung Xamarin.Forms 2.0 oder höher verwendet wird. Aktualisieren Sie das Xamarin.Forms-NuGet-Paket auf 2.0, falls erforderlich.

### <a name="2-check-android-version"></a>2. Überprüfen Sie die Android-version

Stellen Sie sicher, dass das Android-Projekt Zielframework Android 6.0 (Marshmallow) ist. Überprüfen Sie die **Android-Projekts > Optionen > Erstellen > Allgemein** Einstellungen, um sicherzustellen, dass das Framework korrigieren ausgewählt ist:

 ![](appcompat-images/target-android-6-sml.png "Android-Allgemein Buildkonfiguration")

### <a name="3-add-new-themes-to-support-material-design"></a>3. Hinzufügen von neuen Designs zur Unterstützung von Material Entwurf

Erstellen Sie die folgenden drei Dateien im Android-Projekt, und fügen Sie in den folgenden Inhalt. Google bietet eine [Stilvorgaben](http://www.google.com/design/spec/style/color.html#color-color-palette) und ein [Farbe Palette Generator](http://www.materialpalette.com/) zur Auswahl einer alternativen Farbschema angegeben.

**Resources/Values/Colors.Xml**

```xml
<resources>
  <color name="primary">#2196F3</color>
  <color name="primaryDark">#1976D2</color>
  <color name="accent">#FFC107</color>
  <color name="window_background">#F5F5F5</color>
</resources>
```

**Resources/Values/Style.Xml**

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

Ein zusätzlicher Stil enthalten sein muss, der **Werte v21** Ordner bestimmte Eigenschaften anwenden, wenn auf Android Lollipop und höher ausgeführt.

**Resources/Values-v21/Style.Xml**

```xml
<resources>
  <style name="MyTheme" parent="MyTheme.Base">
    <!--If you are using MasterDetailPage you will want to set these, else you can leave them out-->
    <!--<item name="android:windowDrawsSystemBarBackgrounds">true</item>
    <item name="android:statusBarColor">@android:color/transparent</item>-->
  </style>
</resources>
```

### <a name="4-update-androidmanifestxml"></a>4. Aktualisieren von AndroidManifest.xml

Um sicherzustellen, dass das neue Design ist Informationen verwendet wird, legen Design in der **AndroidManifest** Datei durch Hinzufügen von `android:theme="@style/MyTheme"` (lassen Sie den Rest des XML-vorlag).

**Properties/AndroidManifest.xml**

```xml
...
<application android:label="AppName" android:icon="@drawable/icon"
  android:theme="@style/MyTheme">
...
```

### <a name="5-provide-toolbar-and-tab-layouts"></a>5. Geben Sie die Symbolleiste und der Registerkarte layouts

Erstellen Sie **Tabbar.axml** und **Toolbar.axml** Dateien in der **Ressourcen/Layout** Verzeichnis und fügen Sie den Inhalt von unten:

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

Einige Eigenschaften für die Registerkarten haben festgelegt wurde, einschließlich der Registerkarte "Schwerpunkt auf `fill` Modus für die `fixed`.
Wenn Sie viele Registerkarten haben soll dies wechseln in bildlauffähigen - gelesen werden soll, über den Android [TabLayout Dokumentation](http://developer.android.com/reference/android/support/design/widget/TabLayout.html) erfahren Sie mehr.

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

In diesen Dateien erstellen wir bestimmtes Design für die Symbolleiste, die für Ihre Anwendung variieren kann.
Finden Sie in der [Hello Symbolleiste](https://blog.xamarin.com/android-tips-hello-toolbar-goodbye-action-bar/) Blogbeitrag um mehr zu erfahren.


### <a name="6-update-the-mainactivity"></a>6. Update der `MainActivity`

In vorhandenen Xamarin.Forms apps die **MainActivity.cs** Klasse erbt von `FormsApplicationActivity`. Dies ersetzt werden muss, mit `FormsAppCompatActivity` um die neuen Funktionen zu aktivieren.

**MainActivity.cs**

```csharp
public class MainActivity : FormsAppCompatActivity  // was FormsApplicationActivity
```

Zum Schluss "verknüpfen" neue Layouts aus Schritt 5 in der `OnCreate` Methode, wie hier gezeigt:

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
