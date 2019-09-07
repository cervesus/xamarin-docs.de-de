---
title: Hinzufügen von AppCompat und Material Design
description: In diesem Artikel wird erläutert, wie vorhandene xamarin. Forms-Android-Apps für die Verwendung von AppCompat und Material Design konvertiert werden.
ms.prod: xamarin
ms.assetid: 045FBCDF-4D45-48BB-9911-BD3938C87D58
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2017
ms.openlocfilehash: a5b6466b1d2489cced4b1e3205ef672b8f6a4da7
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70770661"
---
# <a name="adding-appcompat-and-material-design"></a>Hinzufügen von AppCompat und Material Design

_Gehen Sie folgendermaßen vor, um vorhandene xamarin. Forms Android-Apps für die Verwendung von AppCompat und Material Design zu konvertieren._

<!-- source https://gist.github.com/jassmith/a3b2a543f99126782936
https://blog.xamarin.com/material-design-for-your-xamarin-forms-android-apps/ -->

## <a name="overview"></a>Übersicht

In diesen Anweisungen wird erläutert, wie Sie Ihre vorhandenen xamarin. Forms Android-Anwendungen aktualisieren, um die AppCompat-Bibliothek zu verwenden und Material Design in der Android-Version Ihrer xamarin. Forms-apps zu aktivieren.

### <a name="1-update-xamarinforms"></a>1. Xamarin. Forms aktualisieren

Stellen Sie sicher, dass die Lösung xamarin. Forms 2,0 oder höher verwendet. Aktualisieren Sie das xamarin. Forms-nuget-Paket bei Bedarf auf 2,0.

### <a name="2-check-android-version"></a>2. Android-Version überprüfen

Stellen Sie sicher, dass das Ziel Framework des Android-Projekts Android 6,0 (Marshmallow) ist. Aktivieren Sie das **Android-Projekt > Optionen > Build > allgemeinen** Einstellungen, um sicherzustellen, dass das Corrent-Framework ausgewählt ist:

 ![](appcompat-images/target-android-6-sml.png "Allgemeine Android-Buildkonfiguration")

### <a name="3-add-new-themes-to-support-material-design"></a>3. Neue Designs zur Unterstützung von Material Design hinzufügen

Erstellen Sie die folgenden drei Dateien in Ihrem Android-Projekt, und fügen Sie den folgenden Inhalt ein. Google stellt einen [Stil Leit Faden](http://www.google.com/design/spec/style/color.html#color-color-palette) und einen [Farbpalette-Generator](http://www.materialpalette.com/) bereit, um Sie bei der Auswahl eines alternativen Farbschemas für das angegebene zu unterstützen.

**Resources/Values/Colors. XML**

```xml
<resources>
  <color name="primary">#2196F3</color>
  <color name="primaryDark">#1976D2</color>
  <color name="accent">#FFC107</color>
  <color name="window_background">#F5F5F5</color>
</resources>
```

**Resources/Values/Style. XML**

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

Im Ordner **Values-V21** muss ein zusätzlicher Stil enthalten sein, um bestimmte Eigenschaften bei der Ausführung unter Android Lollipop und neuer zu verwenden.

**Resources/Values-V21/Style. XML**

```xml
<resources>
  <style name="MyTheme" parent="MyTheme.Base">
    <!--If you are using MasterDetailPage you will want to set these, else you can leave them out-->
    <!--<item name="android:windowDrawsSystemBarBackgrounds">true</item>
    <item name="android:statusBarColor">@android:color/transparent</item>-->
  </style>
</resources>
```

### <a name="4-update-androidmanifestxml"></a>4. Aktualisieren von "androidmanifest. xml"

Um sicherzustellen, dass diese neuen Design Informationen verwendet werden, legen Sie Design in der Datei **androidmanifest** fest, indem Sie hinzufügen `android:theme="@style/MyTheme"` (belassen Sie den Rest des XML-Codes unverändert).

**Properties/AndroidManifest.xml**

```xml
...
<application android:label="AppName" android:icon="@drawable/icon"
  android:theme="@style/MyTheme">
...
```

### <a name="5-provide-toolbar-and-tab-layouts"></a>5. Symbolleisten-und Registerkarten Layouts angeben

Erstellen Sie die Dateien " **Tabbar. axml** " und " **Toolbar. axml** " im **Ressourcen-/Layout-Verzeichnis** , und fügen Sie Ihren Inhalt aus dem folgenden

**Ressourcen/Layout/Tabbar. axml**

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

Einige Eigenschaften für die Registerkarten wurden festgelegt, einschließlich der Schwerkraft der `fill` Registerkarte auf `fixed`und den Modus.
Wenn Sie über eine Vielzahl von Registerkarten verfügen, können Sie diese zu scrollfähig wechseln. Weitere Informationen finden Sie in der Dokumentation zu Android [TabLayout](https://developer.android.com/reference/android/support/design/widget/TabLayout.html) .

**Ressourcen/Layout/Toolbar. axml**

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

In diesen Dateien wird ein bestimmtes Design für die Symbolleiste erstellt, das für Ihre Anwendung variieren kann.
Weitere Informationen finden Sie im Blogbeitrag der [Hello Toolbar](https://blog.xamarin.com/android-tips-hello-toolbar-goodbye-action-bar/) .

### <a name="6-update-the-mainactivity"></a>6. Aktualisieren Sie die`MainActivity`

In vorhandenen xamarin. Forms-apps erbt die **MainActivity.cs** - `FormsApplicationActivity`Klasse von. Dies muss durch ersetzt `FormsAppCompatActivity` werden, um die neue Funktionalität zu aktivieren.

**MainActivity.cs**

```csharp
public class MainActivity : FormsAppCompatActivity  // was FormsApplicationActivity
```

Zum Schluss können Sie die neuen Layouts aus Schritt 5 in der `OnCreate` -Methode übertragen, wie hier gezeigt:

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
