---
title: Kompatibilität von Symbolleisten
ms.prod: xamarin
ms.assetid: A0798CA1-2C7D-43B6-9E91-4435CC7B6683
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/15/2018
ms.openlocfilehash: 12c19cf1024b78e8be30b7c9f2652019e9854375
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61300344"
---
# <a name="toolbar-compatibility"></a>Kompatibilität von Symbolleisten


## <a name="overview"></a>Übersicht

In diesem Abschnitt wird erläutert, wie Sie mit `Toolbar` auf Android-Versionen vor Android 5.0 Lollipop. Wenn Ihre app von Android-Versionen vor Android 5.0 nicht unterstützt, können Sie diesen Abschnitt überspringen. 

Da `Toolbar` ist Teil von die Unterstützungsbibliothek von Android v7 kann verwendet werden auf Geräten mit Android 2.1 (API-Ebene 7) und höher. Allerdings die [Android Support-Bibliothek v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) NuGet muss installiert sein, und der Code geändert, sodass sie verwendet die `Toolbar` -Implementierung in dieser Bibliothek. In diesem Abschnitt wird erläutert, wie dieses NuGet-Paket installieren, und ändern Sie die **ToolbarFun** app [Hinzufügen einer zweiten Symbolleiste](~/android/user-interface/controls/tool-bar/adding-a-second-toolbar.md) , damit sie auf Android-Versionen als Lollipop 5.0 ausgeführt wird.

So ändern Sie eine app, um die AppCompat-Version der Toolbar verwenden: 

1.  Legen Sie die minimale und die Target Android Version für die app ein.

2.  Installieren Sie die AppCompat-NuGet-Paket.

3.  Verwenden Sie ein AppCompat-Design, anstelle eines integrierten Android Designs.

4.  Ändern Sie `MainActivity` , damit es Unterklassen `AppCompatActivity` statt `Activity`. 

Jeder dieser Schritte wird in den folgenden Abschnitten ausführlich erläutert.



## <a name="set-the-minimum-and-target-android-version"></a>Legen Sie den niedrigsten und den Android-Zielversion

Der app-Zielframework muss festgelegt werden, auf die API Level 21 oder höher, oder die app nicht korrekt bereitgestellt werden. Wenn ein Fehler wie z. B. **keine Ressourcenbezeichner für das Attribut "TileModeX" finden Sie im Paket "Android"** wird angezeigt, während der Bereitstellung der app, dies ist da das Zielframework nicht, um festgelegt ist **Android 5.0 (API Level 21 - Lollipop)**  oder höher. 

Legen Sie das Zielframework auf API Level 21 oder höher, und legen Sie die projekteinstellungen der Android-API-Ebene, auf der Android-Mindestversion, die die app zu unterstützen. Weitere Informationen zum Festlegen von Android-API-Ebenen finden Sie unter [Understanding Android API-Ebenen](~/android/app-fundamentals/android-api-levels.md). In der `ToolbarFun` Beispiel, das Android-Mindestversion ist festgelegt auf KitKat (4.4 für API-Ebene). 


## <a name="install-the-appcompat-nuget-package"></a>Die AppCompat-NuGet-Paket installieren

Fügen Sie als Nächstes die [Android Support-Bibliothek v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) Paket dem Projekt. In Visual Studio mit der Maustaste **Verweise** , und wählen Sie **NuGet-Pakete verwalten...** . Klicken Sie auf **Durchsuchen** und suchen Sie nach **Android Support-Bibliothek v7 AppCompat**. Wählen Sie **Xamarin.Android.Support.v7.AppCompat** , und klicken Sie auf **installieren**: 

[![Screenshot des V7 Appcompat-Paket im NuGet-Pakete verwalten ausgewählt](toolbar-compatibility-images/01-appcompat-nuget-sml.png)](toolbar-compatibility-images/01-appcompat-nuget.png#lightbox)

Wenn dieses NuGet-Paket installiert ist, verschiedene andere NuGet-Pakete werden ebenfalls installiert, sofern nicht bereits vorhanden (z. B. **Xamarin.Android.Support.Animated.Vector.Drawable**, **Xamarin.Android.Support.v4**, und **Xamarin.Android.Support.Vector.Drawable**). Weitere Informationen zum Installieren von NuGet-Pakete finden Sie unter [Exemplarische Vorgehensweise: Einschließen ein NuGet in Ihr Projekt](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough). 


## <a name="use-an-appcompat-theme-and-toolbar"></a>Ein AppCompat-Design und Symbolleiste verwenden

Die AppCompat-Bibliothek enthält mehrere `Theme.AppCompat` Designs, die unter jeder Version von Android unterstützt durch die AppCompat-Bibliothek verwendet werden können. Die `ToolbarFun` Beispiel-app-Design ergibt sich aus `Theme.Material.Light.DarkActionBar`, die nicht für ältere Versionen als Lollipop verfügbar ist. Aus diesem Grund `ToolbarFun` muss angepasst werden die AppCompat-Entsprechung dieses Designs, `Theme.AppCompat.Light.DarkActionBar`. Darüber hinaus da `Toolbar` ist nicht verfügbar ist, klicken Sie auf Android-Versionen älter als Lollipop, muss verwenden wir die AppCompat-Version von `Toolbar`. Aus diesem Grund müssen Layouts verwenden `android.support.v7.widget.Toolbar` anstelle von `Toolbar`. 


### <a name="update-layouts"></a>Aktualisieren des Layouts

Bearbeiten Sie **Resources/layout/Main.axml** , und Ersetzen Sie die `Toolbar` -Element mit dem folgenden XML-Code: 

```xml
<android.support.v7.widget.Toolbar
    android:id="@+id/edit_toolbar"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorAccent"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

Bearbeiten Sie **Resources/layout/toolbar.xml** und Ersetzen Sie den Inhalt durch folgendes XML: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v7.widget.Toolbar xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorPrimary"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"/>
```

Beachten Sie, dass die `?attr` Werte werden nicht mehr mit dem Präfix `android:` (zur Erinnerung: die `?` Notation verweist auf eine Ressource im aktuellen Design). Wenn `?android:attr` immer noch verwendet wurden, würde Android den Wert des Attributs verweisen, von der aktuell ausgeführten Plattform und nicht in der AppCompat-Bibliothek. Da in diesem Beispiel verwendet die `actionBarSize` definiert, die von der AppCompat-Bibliothek, die `android:` Präfix wird gelöscht. Auf ähnliche Weise `@android:style` geändert wird, um `@style` , damit die `android:theme` -Attribut zu einem Design festgelegt ist, in der AppCompat-Bibliothek &ndash; der `ThemeOverlay.AppCompat.Dark.ActionBar` Design wird hier verwendet, statt `ThemeOverlay.Material.Dark.ActionBar`. 


### <a name="update-the-style"></a>Aktualisieren Sie die Formatvorlage

Bearbeiten Sie **Resources/values/styles.xml** und Ersetzen Sie den Inhalt durch folgendes XML: 

```xml
<?xml version="1.0" encoding="utf-8" ?>
<resources>
  <style name="MyTheme" parent="MyTheme.Base"> </style>
  <style name="MyTheme.Base" parent="Theme.AppCompat.Light.DarkActionBar">
    <item name="windowNoTitle">true</item>
    <item name="windowActionBar">false</item>
    <item name="colorPrimary">#5A8622</item>
    <item name="colorAccent">#A88F2D</item>
  </style>
</resources>
```

Den Namen und das übergeordnete Design in diesem Beispiel werden nicht mehr mit dem Präfix `android:` da wir die AppCompat-Bibliothek verwenden. Darüber hinaus wird das übergeordnete Design geändert, auf die AppCompat-Version von `Light.DarkActionBar`. 



### <a name="update-menus"></a>Aktualisieren von Menüs

Zur Unterstützung von früher Versionen von Android die AppCompat-Bibliothek verwendet benutzerdefinierte Attribute, die die Attribute des spiegeln die `android:` Namespace. Allerdings einige Attribute (z. B. die `showAsAction` -Attribut in der `<menu>` Tag) nicht in der Android-Framework bei älteren Geräten vorhanden &ndash; `showAsAction` wurde in der Android-API-11 eingeführt, aber in Android-API-7 nicht verfügbar ist. Aus diesem Grund muss ein benutzerdefinierter Namespace verwendet werden, um alle Attribute, die von der Support-Bibliothek definiert ein Präfix hinzuzufügen. In den Menü-Ressourcendateien, ein Namespace namens `local` ist vorangestellte definiert die `showAsAction` Attribut. 

Bearbeiten Sie **Resources/menu/top_menus.xml** und Ersetzen Sie den Inhalt durch folgendes XML:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:local="http://schemas.android.com/apk/res-auto">
  <item
       android:id="@+id/menu_edit"
       android:icon="@mipmap/ic_action_content_create"
       local:showAsAction="ifRoom"
       android:title="Edit" />
  <item
       android:id="@+id/menu_save"
       android:icon="@mipmap/ic_action_content_save"
       local:showAsAction="ifRoom"
       android:title="Save" />
  <item
       android:id="@+id/menu_preferences"
       local:showAsAction="never"
       android:title="Preferences" />
</menu>
```

Die `local` Namespace wird hinzugefügt, mit der folgenden Zeile:

```xml
xmlns:local="http://schemas.android.com/apk/res-auto">
```

Die `showAsAction` mit diesem Attribut eingeleitet wird `local:` Namespace statt `android:` 

```csharp
local:showAsAction="ifRoom"
```

Auf ähnliche Weise bearbeiten **Resources/menu/edit_menus.xml** und Ersetzen Sie den Inhalt durch folgendes XML:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:local="http://schemas.android.com/apk/res-auto">
  <item
       android:id="@+id/menu_cut"
       android:icon="@mipmap/ic_menu_cut_holo_dark"
       local:showAsAction="ifRoom"
       android:title="Cut" />
  <item
       android:id="@+id/menu_copy"
       android:icon="@mipmap/ic_menu_copy_holo_dark"
       local:showAsAction="ifRoom"
       android:title="Copy" />
  <item
       android:id="@+id/menu_paste"
       android:icon="@mipmap/ic_menu_paste_holo_dark"
       local:showAsAction="ifRoom"
       android:title="Paste" />
</menu>
```

Wie dieser Schalter Namespace bietet Unterstützung für die `showAsAction` Attribut für Android-Versionen vor API-Ebene 11? Das benutzerdefinierte Attribut `showAsAction` und aller seiner möglichen Werte sind in der app enthalten, wenn die AppCompat NuGet installiert ist. 


## <a name="subclass-appcompatactivity"></a>Unterklasse AppCompatActivity

Der letzte Schritt bei der Konvertierung ist so ändern Sie `MainActivity` , damit es sich um eine Unterklasse von ist `AppCompactActivity`. Bearbeiten Sie **"mainactivity.cs"** und fügen Sie die folgenden `using` Anweisungen: 

```csharp
using Android.Support.V7.App;
using Toolbar = Android.Support.V7.Widget.Toolbar;
```

Damit wird deklariert `Toolbar` sollen die AppCompat-Version des `Toolbar`. Als Nächstes ändern Sie die Klassendefinition der `MainActivity`: 

```csharp
public class MainActivity : AppCompatActivity
```

Festlegen die Aktionsleiste auf die AppCompat-Version von `Toolbar`, ersetzen Sie den Aufruf von `SetActionBar` mit `SetSupportActionBar`. In diesem Beispiel der Titel wird auch geändert, um anzugeben, dass die AppCompat-Version des `Toolbar` verwendet wird:

```csharp
SetSupportActionBar (toolbar);
SupportActionBar.Title = "My AppCompat Toolbar";
```

Abschließend ändern Sie die Android-Mindestversion Ebene, auf den vor Lollipop-Wert, der (z. B. API-19) unterstützt werden. 

Erstellen Sie die app aus, und führen Sie es auf einem vorab Lollipop-Gerät oder die Android-Emulator. Der folgende Screenshot zeigt die AppCompat-Version des **ToolbarFun** auf einem Nexus-4 KitKat (API-19) ausführen: 

[![Vollständigen Screenshot der app, die auf einem Gerät KitKat ausgeführt wird, werden mit beiden Symbolleisten angezeigt.](toolbar-compatibility-images/02-running-on-kitkat-sml.png)](toolbar-compatibility-images/02-running-on-kitkat.png#lightbox)

Wenn die AppCompat-Bibliothek verwendet wird, Designs müssen nicht umgeschaltet werden basierend auf der Android-Version &ndash; die AppCompat-Bibliothek ermöglicht eine konsistente benutzererfahrung in allen unterstützten Android-Versionen zu bieten. 




## <a name="related-links"></a>Verwandte Links

- [Lollipop-Symbolleiste (Beispiel)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [AppCompat-Symbolleiste (Beispiel)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
