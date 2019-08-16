---
title: Kompatibilität von Symbolleisten
ms.prod: xamarin
ms.assetid: A0798CA1-2C7D-43B6-9E91-4435CC7B6683
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/15/2018
ms.openlocfilehash: 31602b14179691d13d8058c90cf20a6f7f667124
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69522823"
---
# <a name="toolbar-compatibility"></a>Kompatibilität von Symbolleisten


## <a name="overview"></a>Übersicht

In diesem Abschnitt wird erläutert, `Toolbar` wie Sie auf Android-Versionen vor Android 5,0 Lollipop verwenden können. Wenn Ihre APP keine früheren Versionen von Android als Android 5,0 unterstützt, können Sie diesen Abschnitt überspringen. 

Da `Toolbar` Teil der Android V7-Unterstützungs Bibliothek ist, kann Sie auf Geräten verwendet werden, auf denen Android 2,1 (API-Ebene 7) und höher ausgeführt wird. Die Android- [Unterstützungs Bibliothek V7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) -nuget muss jedoch installiert sein, und der Code muss so geändert werden `Toolbar` , dass die in dieser Bibliothek bereitgestellte Implementierung verwendet wird. In diesem Abschnitt wird erläutert, wie Sie diese nuget-App installieren und die **toolbarfun** -App ändern können, [indem Sie eine zweite Symbolleiste hinzufügen](~/android/user-interface/controls/tool-bar/adding-a-second-toolbar.md) , sodass Sie unter Android-Versionen vor Lollipop 5,0 ausgeführt wird.

So ändern Sie eine APP für die Verwendung der AppCompat-Version der Symbolleiste: 

1. Legen Sie die Mindest-und Ziel-Android-Versionen für die APP fest.

2. Installieren Sie das nuget-Paket "AppCompat".

3. Verwenden Sie ein AppCompat-Design anstelle eines integrierten Android-Designs.

4. Ändern `MainActivity` Sie so, dass es `AppCompatActivity` Unterklassen `Activity`anstelle von ist. 

Jeder dieser Schritte wird in den folgenden Abschnitten ausführlich erläutert.



## <a name="set-the-minimum-and-target-android-version"></a>Festlegen der minimal-und Android-Ziel Version

Das Ziel Framework der APP muss auf API-Ebene 21 oder höher festgelegt werden, oder die APP wird nicht ordnungsgemäß bereitgestellt. Wenn während der Bereitstellung der APP ein Fehler wie **kein Ressourcen Bezeichner für das Attribut "tilemudex" in Paket "Android" gefunden** wird, liegt dies daran, dass das Ziel Framework nicht auf **Android 5,0 (API-Ebene 21-Lollipop)** oder höher festgelegt ist. 

Legen Sie die zielframeworkebene auf API-Ebene 21 oder höher fest, und legen Sie die Projekteinstellungen der Android-API-Ebene auf die Android-Mindestversion fest, die von der APP Weitere Informationen zum Festlegen von Android-API-Ebenen finden Sie Untergrund Legendes zu [Android-API-Ebenen](~/android/app-fundamentals/android-api-levels.md). `ToolbarFun` Im Beispiel ist die Android-Mindestversion auf KitKat (API-Ebene 4,4) festgelegt. 


## <a name="install-the-appcompat-nuget-package"></a>Installieren des AppCompat-nuget-Pakets

Fügen Sie als nächstes dem Projekt das Paket mit der [Android-Unterstützungs Bibliothek V7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) hinzu. Klicken Sie in Visual Studio mit der rechten Maustaste auf **Verweise** , und wählen Sie **nuget-Pakete verwalten...** aus. Klicken Sie auf **Durchsuchen** , und suchen Sie nach **Android Support Library V7 AppCompat**. Wählen Sie **xamarin. Android. Support. V7. AppCompat** aus, und klicken Sie auf **Installieren**: 

[![Screenshot des in "nuget-Pakete verwalten" ausgewählten "V7 AppCompat Package"](toolbar-compatibility-images/01-appcompat-nuget-sml.png)](toolbar-compatibility-images/01-appcompat-nuget.png#lightbox)

Wenn diese nuget-Version installiert ist, werden auch einige andere nuget-Pakete installiert, sofern Sie noch nicht vorhanden sind (z. b. **xamarin. Android. Support. animiert. Vector. drawable**, **xamarin. Android. Support. v4**) und  **Xamarin. Android. Support. Vector. drawable**). Weitere Informationen zum Installieren von nuget-Paketen finden [Sie unter Exemplarische Vorgehensweise: Einschließen eines nuget-Projekts in](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)Ihr Projekt. 


## <a name="use-an-appcompat-theme-and-toolbar"></a>Verwenden eines AppCompat-Designs und einer Symbolleiste

Die AppCompat-Bibliothek enthält mehrere `Theme.AppCompat` Themen, die auf jeder von der AppCompat-Bibliothek unterstützten Android-Version verwendet werden können. Das `ToolbarFun` Beispiel-App-Design wird `Theme.Material.Light.DarkActionBar`von abgeleitet, das auf Android-Versionen vor Lollipop nicht verfügbar ist. Daher muss angepasst werden, damit die AppCompat-Entsprechung für dieses `Theme.AppCompat.Light.DarkActionBar`Design verwendet wird. `ToolbarFun` Da `Toolbar` auch in Versionen von Android älter als Lollipop nicht verfügbar ist, müssen wir die AppCompat-Version von `Toolbar`verwenden. Daher müssen Layouts anstelle von `android.support.v7.widget.Toolbar` `Toolbar`verwendet werden. 


### <a name="update-layouts"></a>Update Layouts

Bearbeiten Sie **Resources/Layout/Main. axml** , und `Toolbar` ersetzen Sie das-Element durch den folgenden XML-Code: 

```xml
<android.support.v7.widget.Toolbar
    android:id="@+id/edit_toolbar"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorAccent"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

Bearbeiten Sie **Resources/Layout/Toolbar. XML** , und ersetzen Sie den Inhalt durch den folgenden XML-Code: 

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

Beachten Sie, `?attr` dass den Werten kein `android:` Präfix mehr vorangestellt wird ( `?` denken Sie daran, dass die Notation auf eine Ressource im aktuellen Design verweist). Wenn `?android:attr` Sie hier immer noch verwendet werden, würde Android auf den Attribut Wert von der derzeit laufenden Plattform und nicht aus der AppCompat-Bibliothek verweisen. Da in diesem Beispiel das `actionBarSize` von der AppCompat-Bibliothek definierte verwendet `android:` wird, wird das Präfix gelöscht. &ndash; `android:theme` `ThemeOverlay.AppCompat.Dark.ActionBar` `ThemeOverlay.Material.Dark.ActionBar`Entsprechend wird in `@style` geändert, sodass das-Attribut in der AppCompat-Bibliothek auf ein Design festgelegt wird. das Design wird hier anstelle von verwendet. `@android:style` 


### <a name="update-the-style"></a>Aktualisieren des Stils

Bearbeiten Sie **Resources/Values/Styles. XML** , und ersetzen Sie den Inhalt durch den folgenden XML-Code: 

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

Die Elementnamen und das übergeordnete Design in diesem Beispiel sind nicht mehr mit `android:` dem Präfix versehen, da wir die AppCompat-Bibliothek verwenden. Außerdem wird das übergeordnete Design in die AppCompat-Version von `Light.DarkActionBar`geändert. 



### <a name="update-menus"></a>Menüs aktualisieren

Zur Unterstützung früherer Versionen von Android verwendet die AppCompat-Bibliothek benutzerdefinierte Attribute, die die Attribute `android:` des-Namespace widerspiegeln. Einige Attribute (z. b. das `showAsAction` `<menu>` im-Tag verwendete-Attribut) sind jedoch nicht in Android-Framework &ndash; `showAsAction` vorhanden, die in Android API 11 eingeführt wurden, aber in Android-API 7 nicht verfügbar sind. Aus diesem Grund muss ein benutzerdefinierter Namespace verwendet werden, um allen von der Unterstützungs Bibliothek definierten Attributen ein Präfix zu geben. In den Menü Ressourcen Dateien wird ein Namespace mit `local` dem Namen für die präfixkorrektur des `showAsAction` Attributs definiert. 

Bearbeiten Sie **Resources/Menu/top_menus. XML** , und ersetzen Sie den Inhalt durch den folgenden XML-Code:

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

Der `local` Namespace wird mit dieser Zeile hinzugefügt:

```xml
xmlns:local="http://schemas.android.com/apk/res-auto">
```

Das `showAsAction` Attribut wird diesem `local:` Namespace vorangestellt, anstatt`android:` 

```csharp
local:showAsAction="ifRoom"
```

Bearbeiten Sie auch **Resources/Menu/edit_menus. XML** , und ersetzen Sie den Inhalt durch den folgenden XML-Code:

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

Wie bietet dieser Namespace Switch Unterstützung für das `showAsAction` -Attribut in Android-Versionen vor API-Ebene 11? Das benutzerdefinierte `showAsAction` Attribut und alle möglichen Werte sind in der App enthalten, wenn der AppCompat-nuget installiert ist. 


## <a name="subclass-appcompatactivity"></a>Unterklasse appcompatactivity

Der letzte Schritt in der Konvertierung besteht darin, `MainActivity` so zu ändern, dass es sich um `AppCompactActivity`eine Unterklasse von handelt. Bearbeiten Sie **MainActivity.cs** , und fügen `using` Sie die folgenden Anweisungen hinzu: 

```csharp
using Android.Support.V7.App;
using Toolbar = Android.Support.V7.Widget.Toolbar;
```

Dies deklariert `Toolbar` als die AppCompat-Version von `Toolbar`. Ändern Sie anschließend die Klassendefinition von `MainActivity`: 

```csharp
public class MainActivity : AppCompatActivity
```

Um die Aktionsleiste auf die AppCompat-Version von `Toolbar`festzulegen, ersetzen Sie `SetActionBar` durch `SetSupportActionBar`den-Befehl durch. In diesem Beispiel wird der Titel ebenfalls geändert, um anzugeben, dass die AppCompat- `Toolbar` Version von verwendet wird:

```csharp
SetSupportActionBar (toolbar);
SupportActionBar.Title = "My AppCompat Toolbar";
```

Ändern Sie abschließend die minimale Android-Ebene in den Wert für Pre-Lollipop, der unterstützt werden soll (z. b. API 19). 

Erstellen Sie die APP, und führen Sie Sie auf einem Pre-Lollipop-Gerät oder Android-Emulator aus. Der folgende Screenshot zeigt die AppCompat-Version von **toolbarfun** auf einem Nexus 4 mit KitKat (API 19): 

[![Vollständiger Screenshot der APP, die auf einem KitKat-Gerät ausgeführt wird. beide Symbolleisten werden angezeigt.](toolbar-compatibility-images/02-running-on-kitkat-sml.png)](toolbar-compatibility-images/02-running-on-kitkat.png#lightbox)

Wenn die AppCompat-Bibliothek verwendet wird, müssen Designs nicht basierend auf der Android-Version &ndash; gewechselt werden. die AppCompat-Bibliothek ermöglicht es, eine konsistente Benutzer Darstellung für alle unterstützten Android-Versionen bereitzustellen. 




## <a name="related-links"></a>Verwandte Links

- [Lollipop-Symbolleiste (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-toolbar)
- [AppCompat-Symbolleiste (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar)
