---
title: "Symbolleiste-Kompatibilität"
ms.topic: article
ms.prod: xamarin
ms.assetid: A0798CA1-2C7D-43B6-9E91-4435CC7B6683
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: a17ad79d3f3b537332494fc368c878f2733d5db2
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="toolbar-compatibility"></a>Symbolleiste-Kompatibilität


## <a name="overview"></a>Übersicht

In diesem Abschnitt wird erläutert, wie `Toolbar` Android-Versionen vor Android 5.0 Lollipop. Wenn Ihre app von Android-Versionen vor Android 5.0 nicht unterstützt, können Sie diesen Abschnitt überspringen. 

Da `Toolbar` ist Bestandteil des Android v7-Unterstützungsbibliothek, kann Sie auf Geräten mit Android 2.1 (API-Ebene 7) verwendet und höher. Allerdings die [Android Unterstützungsbibliothek v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) NuGet muss installiert sein und der Code so geändert, dass er verwendet die `Toolbar` Implementierung, die in dieser Bibliothek bereitgestellt. In diesem Abschnitt wird erläutert, wie diese NuGet installiert und angepasst der **ToolbarFun** app aus [Hinzufügen einer zweiten Symbolleiste](~/android/user-interface/controls/tool-bar/adding-a-second-toolbar.md) , damit sie auf Android-Versionen vor Lollipop 5.0 ausgeführt wird.

So ändern Sie eine Anwendung, um die Version AppCompat Symbolleiste verwenden: 

1.  Legen Sie die Mindest- und Ziel-Android-Versionen für die app an.

2.  Installieren Sie das AppCompat NuGet-Paket.

3.  Verwenden Sie ein Design AppCompat anstelle eines integrierten Android Designs.

4.  Ändern Sie `MainActivity` , damit es Unterklassen `AppCompatActivity` statt `Activity`. 

Jeder dieser Schritte wird in den folgenden Abschnitten ausführlich erläutert.



## <a name="set-the-minimum-and-target-android-version"></a>Legen Sie den niedrigsten und den Android-Zielversion

Zielframework für der app muss festgelegt sein, auf API-Ebene 21 oder höher, oder die app nicht korrekt bereitgestellt werden. Wenn ein Fehler wie z. B. **keine Ressourcenbezeichner gefunden für Attribut "TileModeX" in Paket "Android"** wird angezeigt, beim Bereitstellen der app, Grund hierfür ist das Zielframework nicht festgelegt ist, um **Android 5.0.x (API-Ebene 21 - Lollipop)**  oder höher. 

Legen Sie das Zielframework auf API-Ebene 21 oder höher, und legen Sie die Android-API-Ebene projekteinstellungen auf die minimale Android-Version, die die app zu unterstützen. Weitere Informationen zum Festlegen von Android-API-Ebenen, finden Sie unter [Grundlegendes zu Android-API-Ebenen](~/android/app-fundamentals/android-api-levels.md). In der `ToolbarFun` Beispiel, die mindestens erforderliche Android-Version ist festgelegt auf KitKat (4.4 für API-Ebene). 


## <a name="install-the-appcompat-nuget-package"></a>Installieren Sie das AppCompat NuGet-Paket

Als Nächstes fügen Sie der [Android Unterstützungsbibliothek v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) Paket zum Projekt. In Visual Studio mit der Maustaste **Verweise** , und wählen Sie **NuGet-Pakete verwalten...** . Klicken Sie auf **Durchsuchen** , suchen Sie nach **Android Unterstützungsbibliothek v7 AppCompat**. Wählen Sie **Xamarin.Android.Support.v7.AppCompat** , und klicken Sie auf **installieren**: 

[![Screenshot der V7 Appcompat-Paket ausgewählt in NuGet-Pakete verwalten](toolbar-compatibility-images/01-appcompat-nuget-sml.png)](toolbar-compatibility-images/01-appcompat-nuget.png#lightbox)

Wenn dieses NuGet installiert ist, mehrere andere NuGet-Pakete werden ebenfalls installiert Falls noch nicht vorhanden (z. B. **Xamarin.Android.Support.Animated.Vector.Drawable**, **Xamarin.Android.Support.v4**, und **Xamarin.Android.Support.Vector.Drawable**). Weitere Informationen zum Installieren von NuGet-Pakete finden Sie unter [Exemplarische Vorgehensweise: einschließlich eines NuGet in Ihrem Projekt](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough). 


## <a name="use-an-appcompat-theme-and-toolbar"></a>Verwenden Sie ein Design AppCompat und Symbolleiste

AppCompat-Bibliothek enthält mehrere `Theme.AppCompat` Designs bereit, die auf einer beliebigen Version von Android unterstützt durch die AppCompat-Bibliothek verwendet werden können. Die `ToolbarFun` Beispiel-app-Design stammt aus `Theme.Material.Light.DarkActionBar`, die nicht auf Android-Versionen als Lollipop verfügbar ist. Aus diesem Grund `ToolbarFun` muss angepasst werden die AppCompat Entsprechung für das Design `Theme.AppCompat.Light.DarkActionBar`. Darüber hinaus da `Toolbar` ist nicht verfügbar auf Android-Versionen älter als Lollipop, wir muss AppCompat-Version verwenden `Toolbar`. Deshalb müssen Layouts verwenden `android.support.v7.widget.Toolbar` anstelle von `Toolbar`. 


### <a name="update-layouts"></a>Aktualisieren von Layouts

Bearbeiten Sie **Resources/layout/Main.axml** , und Ersetzen Sie die `Toolbar` Element mit dem folgenden XML-Code: 

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

Beachten Sie, dass die `?attr` Werte sind nicht mehr mit dem Präfix `android:` (Bedenken Sie, dass die `?` Notation verweist auf eine Ressource in das aktuelle Design). Wenn `?android:attr` wurden weiterhin verwendet Hier würden Android Wert des Attributs verweisen, von der Plattform, der derzeit ausgeführten anstatt von der AppCompat-Bibliothek. Da in diesem Beispiel verwendet die `actionBarSize` von der Bibliothek AppCompat definiert die `android:` Präfix wird gelöscht. Auf ähnliche Weise `@android:style` geändert wird, um `@style` , damit die `android:theme` -Attribut auf ein Design in der Bibliothek AppCompat festgelegt ist &ndash; der `ThemeOverlay.AppCompat.Dark.ActionBar` Design wird hier verwendet statt `ThemeOverlay.Material.Dark.ActionBar`. 


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

Die Elementnamen und übergeordneten Design in diesem Beispiel werden nicht mehr mit dem Präfix `android:` da AppCompat-Bibliothek verwendet wird. Darüber hinaus wird das übergeordnete Design geändert, auf die AppCompat-Version von `Light.DarkActionBar`. 



### <a name="update-menus"></a>Aktualisieren von Menüs

Unterstützung von früheren Versionen von Android, die AppCompat-Bibliothek verwendet benutzerdefinierte Attribute, die die Attribute des spiegeln die `android:` Namespace. Allerdings einige Attribute (z. B. die `showAsAction` Attribut verwendet, der `<menu>` Tag) nicht in der Android-Framework auf ältere Geräte vorhanden &ndash; `showAsAction` Android-API 11 eingeführt wurde, aber ist in der Android-API-7 nicht verfügbar. Aus diesem Grund muss ein benutzerdefinierter Namespace verwendet werden, um alle Attribute, die von der Support-Bibliothek definiert ein Präfix voranzustellen. In den Menü-Ressourcendateien als Namespace bezeichnet `local` wird vorangestellte definiert die `showAsAction` Attribut. 

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

Die `local` Namespace wird durch diese Zeile hinzugefügt:

```xml
xmlns:local="http://schemas.android.com/apk/res-auto">
```

Die `showAsAction` Attribut wird mit diesem Präfix `local:` Namespace statt `android:` 

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

Wie dieser Namespace Schalter bieten Unterstützung für die `showAsAction` Attribut auf Android-Versionen vor der API-Ebene 11? Das benutzerdefinierte Attribut `showAsAction` und aller seiner möglichen Werte sind in der app enthalten, wenn die AppCompat NuGet installiert ist. 


## <a name="subclass-appcompatactivity"></a>Unterklasse AppCompatActivity

Der letzte Schritt bei der Konvertierung ist so ändern Sie `MainActivity` , eine Unterklasse von ist `AppCompactActivity`. Bearbeiten Sie **MainActivity.cs** und fügen Sie die folgenden `using` Anweisungen: 

```csharp
using Android.Support.V7.App;
using Toolbar = Android.Support.V7.Widget.Toolbar;
```

Damit wird deklariert `Toolbar` werden die AppCompat-Version des `Toolbar`. Als Nächstes ändern Sie die Klassendefinition der `MainActivity`: 

```csharp
public class MainActivity : AppCompatActivity
```

Festlegen die Aktionsleiste auf die AppCompat-Version von `Toolbar`, ersetzen Sie durch den Aufruf von `SetActionBar` mit `SetSupportActionBar`. In diesem Beispiel wird die Position auch um anzugeben, dass geändert AppCompat-Versionen `Toolbar` verwendet wird:

```csharp
SetSupportActionBar (toolbar);
SupportActionBar.Title = "My AppCompat Toolbar";
```

Abschließend ändern Sie die Minimum Android Ebene, auf den vor Lollipop-Wert, der (z. B. API-19) unterstützt werden. 

Erstellen Sie die app, und führen Sie es auf einem vorab Lollipop-Gerät oder Android-Emulator. Der folgende Screenshot zeigt die AppCompat-Version des **ToolbarFun** auf einem Nexus 4 KitKat (API-19) ausführen: 

[![Vollständigen Screenshot der app, die auf einem Gerät KitKat ausgeführt wird, werden beide Symbolleisten angezeigt.](toolbar-compatibility-images/02-running-on-kitkat-sml.png)](toolbar-compatibility-images/02-running-on-kitkat.png#lightbox)

Wenn die AppCompat-Bibliothek verwendet wird, Designs müssen nicht umgeschaltet werden basierend auf der Android-Version &ndash; die Bibliothek AppCompat ermöglicht eine konsistente benutzererfahrung in allen unterstützten Android-Versionen. 




## <a name="related-links"></a>Verwandte Links

- [Lollipop-Symbolleiste (Beispiel)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [AppCompat-Symbolleiste (Beispiel)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
