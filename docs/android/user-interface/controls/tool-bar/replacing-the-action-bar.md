---
title: Ersetzen der Aktionsleiste
ms.prod: xamarin
ms.assetid: 5341D28E-B203-478D-8464-6FAFDC3A4110
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/27/2018
ms.openlocfilehash: 9e9fa1e2651661670f89baac7fcd438b3d14bfb3
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61200913"
---
# <a name="replacing-the-action-bar"></a>Ersetzen der Aktionsleiste

## <a name="overview"></a>Übersicht

Eine der am häufigsten verwendet, für die `Toolbar` ist das Ersetzen der Aktionsleiste standardmäßig mit einem benutzerdefinierten `Toolbar` (wenn ein neues Android-Projekt erstellt wird, verwendet die Aktionsleiste Standard). Da die `Toolbar` bietet die Möglichkeit, die mit Branding Logos, Titel, Menüelemente, Navigationsschaltflächen und auch benutzerdefinierte Ansichten der app-Leiste Teil einer Aktivitätssymbols hinzufügen Benutzeroberfläche er ein wichtigen Upgrade über die Standard-Aktionsleiste bietet.

Ersetzen einer app Aktionsleiste "Standard" mit einem `Toolbar`: 

1.  Erstellen Sie ein neues benutzerdefiniertes Design aus, und ändern Sie die Eigenschaften der app, sodass sie das neue Design verwendet. 

2.  Deaktivieren Sie die `windowActionBar` -Attribut in das benutzerdefinierte Design, und aktivieren Sie die `windowNoTitle` Attribut.

3.  Definieren Sie ein Layout für die `Toolbar`.

4.  Enthalten die `Toolbar` Layout der Aktivität **Main.axml** Layoutdatei. 

5.  Fügen Sie Code der Aktivität `OnCreate` Methode zum Suchen der `Toolbar` , und rufen Sie `SetActionBar` zum Installieren der `ToolBar` als der Aktionsleiste.

In den folgenden Abschnitten wird dieser Prozess im Detail erläutert. Eine einfache app wird erstellt, und die Aktionsleiste ersetzt wird mit einer benutzerdefinierten `Toolbar`. 



## <a name="start-an-app-project"></a>Starten Sie ein App-Projekt

Erstellen Sie ein neues Android-Projekt namens **ToolbarFun** (finden Sie unter [Hallo, Android](~/android/get-started/hello-android/hello-android-quickstart.md) für Weitere Informationen zum Erstellen eines neuen Android-Projekts). Nachdem das Projekt erstellt wurde, legen Sie die Ziel- und mindestplattformversionen Android-API-Ebenen auf **Android 5.0 (API Level 21 - Lollipop)** oder höher. Weitere Informationen zu Android-Version-Ebenen festlegen, finden Sie unter [Understanding Android API-Ebenen](~/android/app-fundamentals/android-api-levels.md). Wenn die app erstellt und ausgeführt wird, wird die Standard-Aktionsleiste wie im folgenden Screenshot zu sehen:

[![Screenshot des Standard-Aktionsleiste](replacing-the-action-bar-images/01-before-sml.png)](replacing-the-action-bar-images/01-before.png#lightbox)



## <a name="create-a-custom-theme"></a>Erstellen Sie ein benutzerdefiniertes Design

Öffnen der **Ressourcen/Values** Verzeichnis und erstellen eine neue Datei namens **styles.xml**. Ersetzen Sie den Inhalt durch folgendes XML: 

```xml
<?xml version="1.0" encoding="utf-8" ?>
<resources>
  <style name="MyTheme" parent="@android:style/Theme.Material.Light.DarkActionBar">
    <item name="android:windowNoTitle">true</item>
    <item name="android:windowActionBar">false</item>
    <item name="android:colorPrimary">#5A8622</item>
  </style>
</resources>
```

Dieser XML-Code definiert ein neues benutzerdefiniertes Design namens **MeineDesigns** auf Grundlage der **Theme.Material.Light.DarkActionBar** Design Lollipop. Die `windowNoTitle` -Attributsatz auf `true` zum Ausblenden der Titelleiste klicken: 

```xml
<item name="android:windowNoTitle">true</item>
```

Zum Anzeigen der benutzerdefinierten Symbolleiste Standard `ActionBar` muss deaktiviert sein: 

```xml
<item name="android:windowActionBar">false</item>
```

Ein Olivgrün gefärbt `colorPrimary` Einstellung wird für die Hintergrundfarbe der Symbolleiste verwendet: 
 
```xml
<item name="android:colorPrimary">#5A8622</item>
```

## <a name="apply-the-custom-theme"></a>Das benutzerdefinierte Design anwenden

Bearbeiten Sie **Properties/Androidmanifest.XML** und fügen Sie die folgenden `android:theme` -Attribut auf die `<application>` Element so, dass die app verwendet die `MyTheme` benutzerdefiniertes Design: 

```xml
<application android:label="@string/app_name" android:theme="@style/MyTheme"></application>
```

Weitere Informationen zum Anwenden eines benutzerdefinierten Designs zu einer app finden Sie unter [mithilfe eines benutzerdefinierten Designs](~/android/user-interface/material-theme.md#customtheme). 



## <a name="define-a-toolbar-layout"></a>Definieren Sie ein Symbolleistenlayout

In der **Ressourcen/Layout** Verzeichnis erstellen Sie eine neue Datei namens **toolbar.xml**. Ersetzen Sie den Inhalt durch folgendes XML: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<Toolbar xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:minHeight="?android:attr/actionBarSize"
    android:background="?android:attr/colorPrimary"
    android:theme="@android:style/ThemeOverlay.Material.Dark.ActionBar"/>
```

Dieser XML-Code definiert die benutzerdefinierte `Toolbar` , die Standard-Aktionsleiste ersetzt. Die Mindesthöhe der `Toolbar` festgelegt ist, auf die Größe des Balkens Aktion, die es ersetzt: 

```csharp
android:minHeight="?android:attr/actionBarSize"
```

Die Farbe des Hintergrunds der `Toolbar` festgelegt ist, auf die Olivgrün gefärbt Farbe, die weiter oben definierten **styles.xml**:

```csharp
android:background="?android:attr/colorPrimary"
```

Lollipop, ab der `android:theme` Attribut kann verwendet werden, um eine einzelne Ansicht ein Format zuzuweisen. Die `ThemeOverlay.Material` Designs in Lollipop eingeführt können sie die Standardeinstellung zu überlagern `Theme.Material` Designs, die relevanten Attribute, um entweder hell oder dunkel zu überschreiben. In diesem Beispiel die `Toolbar` wird ein Design "dunkel" verwendet, sodass ihr Inhalt helle Farbe ist: 

```csharp
android:theme="@android:style/ThemeOverlay.Material.Dark.ActionBar"
```

Diese Einstellung wird verwendet, sodass mit der Hintergrundfarbe der dunkleren Menüelemente vergleichen.



## <a name="include-the-toolbar-layout"></a>Das Symbolleistenlayout enthalten

Bearbeiten Sie die Layoutdatei **Resources/layout/Main.axml** und Ersetzen Sie den Inhalt durch folgendes XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <include
        android:id="@+id/toolbar"
        layout="@layout/toolbar" />
</RelativeLayout>
```

Dieses Layout enthält die `Toolbar` in definierten **toolbar.xml** und verwendet eine `RelativeLayout` an, dass die `Toolbar` der Benutzeroberfläche (über die Schaltfläche ") ganz oben platziert werden soll. 



## <a name="find-and-activate-the-toolbar"></a>Suchen Sie und aktivieren Sie die Symbolleiste

Bearbeiten Sie **"mainactivity.cs"** und fügen Sie die folgenden using-Anweisung:

```csharp
using Android.Views;
```

Darüber hinaus die folgenden Codezeilen hinzufügen, am Ende der `OnCreate` Methode:

```csharp
var toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
SetActionBar(toolbar);
ActionBar.Title = "My Toolbar";
```

Dieser Code sucht die `Toolbar` und ruft `SetActionBar` , damit die `Toolbar` treten am Standardmerkmale Aktion-Leiste. Der Titel der Symbolleiste geändert wird, um **Meine Symbolleiste**. Wie in diesem Codebeispiel wird die `ToolBar` als eine Aktionsleiste direkt verwiesen werden kann. Kompilieren und Ausführen dieser app &ndash; den angepassten `Toolbar` wird anstelle der standardmäßigen Aktionsleiste angezeigt: 

[![Screenshot der angepassten Symbolleiste mit Grün-Farbskala](replacing-the-action-bar-images/02-after-sml.png)](replacing-the-action-bar-images/02-after.png#lightbox)

Beachten Sie, dass die `Toolbar` formatiert ist, unabhängig von der `Theme.Material.Light.DarkActionBar` Design, das auf den Rest der app angewendet wird. 

Wenn eine Ausnahme auftritt, während die app ausgeführt wird, finden Sie unter den [Problembehandlung](#troubleshooting) Abschnitt weiter unten.

 
## <a name="add-menu-items"></a>Hinzufügen von Menüelementen 

In diesem Abschnitt Menüs hinzugefügt, die `Toolbar`. Der rechten oberen Bereich des der `ToolBar` ist reserviert für Menüelemente &ndash; jedes Menüelement (so genannte ein *Aktionselement*) kann eine Aktion innerhalb der aktuellen Aktivität oder sie können eine Aktion für die gesamte app ausführen. 

Hinzufügen von Menüs, die `Toolbar`: 

1.  Fügen Sie das Menüsymbole (falls erforderlich) die `mipmap-` Ordner des app-Projekts. Google bietet eine Reihe von kostenlosen Symbole auf der [Material Symbole](https://design.google.com/icons/) Seite. 

2.  Definieren Sie den Inhalt der Menüelemente durch Hinzufügen einer neuen Menü-Ressourcendatei unter **ressourcenmenü/**. 

3.  Implementieren der `OnCreateOptionsMenu` -Methode der Aktivität &ndash; diese Methode vergrößert die Menüelemente. 

4.  Implementieren der `OnOptionsItemSelected` -Methode der Aktivität &ndash; diese Methode führt eine Aktion aus, wenn ein Menüelement angetippt wird. 

Die folgenden Abschnitte zeigen diesen Prozess im Detail durch Hinzufügen von **bearbeiten** und **speichern** Menüelementen, die die benutzerdefinierte `Toolbar`. 



### <a name="install-menu-icons"></a>Installieren Sie im Menüsymbole

Anhand der `ToolbarFun` Beispiel-app, fügen Sie im Menüsymbole für die app-Projekt. Herunterladen [Symbole](https://github.com/xamarin/monodroid-samples/blob/master/Supportv7/AppCompat/Toolbar/Resources/toolbar-icons-plus.zip?raw=true), entpacken Sie aus, und kopieren Sie den Inhalt, der die extrahierten *Mipmap -* Ordner des Projekts *Mipmap -* Unterordnern **ToolbarFun / Ressourcen** und jede hinzugefügte Symboldatei in das Projekt einfügen.


### <a name="define-a-menu-resource"></a>Definieren Sie eine Menüressource

Erstellen Sie ein neues **Menü** Unterverzeichnis **Ressourcen**. In der **Menü** Unterverzeichnis, erstellen Sie eine neue Menü Ressourcendatei namens **top_menus.xml** und Ersetzen Sie den Inhalt durch folgendes XML: 

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item
       android:id="@+id/menu_edit"
       android:icon="@mipmap/ic_action_content_create"
       android:showAsAction="ifRoom"
       android:title="Edit" />
  <item
       android:id="@+id/menu_save"
       android:icon="@mipmap/ic_action_content_save"
       android:showAsAction="ifRoom"
       android:title="Save" />
  <item
       android:id="@+id/menu_preferences"
       android:showAsAction="never"
       android:title="Preferences" />
</menu>
```

Dieses XML erstellt drei Elemente:

-   Ein **bearbeiten** Menüelement, das verwendet die `ic_action_content_create.png` Symbol (ein Stift). 

-   Ein **speichern** Menüelement, das verwendet die `ic_action_content_save.png` Symbol (eine Diskette). 

-   Ein **Voreinstellungen** Menüelement, das nicht über ein Symbol verfügt.

Die `showAsAction` Attribute der **bearbeiten** und **speichern** Menüelemente festgelegt `ifRoom` &ndash; diese Einstellung bewirkt, dass diese Menüelemente angezeigt werden die `Toolbar` liegt ausreichend Speicherplatz vorhanden, bis sie angezeigt werden soll. Die **Voreinstellungen** Menü Elementsätze `showAsAction` zu `never` &ndash; Dies bewirkt, dass die **Voreinstellungen** Menü angezeigt werden die *Überlauf* Menü (drei vertikal angeordneten Punkte). 


### <a name="implement-oncreateoptionsmenu"></a>Implementieren von OnCreateOptionsMenu

Fügen Sie die folgende Methode **"mainactivity.cs"**:

```csharp
public override bool OnCreateOptionsMenu(IMenu menu)
{
    MenuInflater.Inflate(Resource.Menu.top_menus, menu);
    return base.OnCreateOptionsMenu(menu);
}
```

Android Ruft die `OnCreateOptionsMenu` Methode, damit die app die Menüressource für eine Aktivität angeben kann. Bei dieser Methode die **top_menus.xml** Ressource vergrößert wird in den übergebenen `menu`. Dieser Code bewirkt, dass die neue **bearbeiten**, **speichern**, und **Voreinstellungen** Menüelemente angezeigt werden die `Toolbar`. 



### <a name="implement-onoptionsitemselected"></a>Implementieren von OnOptionsItemSelected

Fügen Sie die folgende Methode **"mainactivity.cs"**:

```csharp
public override bool OnOptionsItemSelected(IMenuItem item)
{
    Toast.MakeText(this, "Action selected: " + item.TitleFormatted,
        ToastLength.Short).Show();
    return base.OnOptionsItemSelected(item);
}
```

Wenn ein Benutzer ein Menüelement tippt, Android Ruft die `OnOptionsItemSelected` -Methode auf und übergibt das Menüelement, das ausgewählt wurde. In diesem Beispiel zeigt die Implementierung nur als Popupbenachrichtigung, um anzugeben, welche Menüelemente angetippt wurde. 

Erstellen und ausführen `ToolbarFun` die neuen Menüelemente in der Symbolleiste angezeigt. Die `Toolbar` zeigt nun drei Symbole aus, wie im folgenden Screenshot zu sehen: 

[![Diagramm zur Veranschaulichung Speicherorte der bearbeiten, speichern und Menüelemente Overflow](replacing-the-action-bar-images/04-menu-items-sml.png)](replacing-the-action-bar-images/04-menu-items.png#lightbox)

Wenn ein Benutzer Taps der **bearbeiten** Menüelement, ein Popup wird angezeigt, um anzugeben, dass die `OnOptionsItemSelected` Methode wurde aufgerufen: 

[![Screenshot der Popupbenachrichtigung angezeigt, wenn das Element bearbeiten getippt wird](replacing-the-action-bar-images/05-toast-displayed-sml.png)](replacing-the-action-bar-images/05-toast-displayed.png#lightbox)

Wenn ein Benutzer das Überlaufmenü tippt der **Voreinstellungen** Menüelement angezeigt wird. In der Regel weniger übliche Aktionen im Überlaufmenü platziert werden sollten &ndash; dieses Beispiel verwendet das Überlaufmenü für **Voreinstellungen** , da sie nicht so häufig verwendet wird als **bearbeiten** und  **Speichern Sie**: 

[![Screenshot der Einstellungen-Menüelement, das im Überlaufmenü angezeigt wird.](replacing-the-action-bar-images/06-preferences-sml.png)](replacing-the-action-bar-images/06-preferences.png#lightbox)

Weitere Informationen zu Android-Menüs, finden Sie in der Android-Entwickler [Menüs](https://developer.android.com/guide/topics/ui/menus.html) Thema. 
 

## <a name="troubleshooting"></a>Problembehandlung

Die folgenden Tipps können helfen, um Probleme zu debuggen, die beim Ersetzen der Aktionsleiste mit einer Symbolleiste auftreten können.

### <a name="activity-already-has-an-action-bar"></a>Aktivität verfügt bereits über eine Aktionsleiste

Wenn die app nicht ordnungsgemäß ein benutzerdefiniertes Design verwenden konfiguriert ist, wie unter [Anwenden der benutzerdefinierten Designs](#apply-the-custom-theme), die folgende Ausnahme kann auftreten, während der Ausführung der app:

![Fehler, die auftreten können, wenn benutzerdefiniertes Design nicht verwendet wird](replacing-the-action-bar-images/03-theme-not-defined.png)

Darüber hinaus kann eine Fehlermeldung wie die folgende erstellt werden: _Java.Lang.IllegalStateException: Diese Aktivität ist bereits eine Aktionsleiste der Fenster Decor vom._ 

Um diesen Fehler zu beheben, überprüfen, ob die `android:theme` -Attribut für das benutzerdefinierte Design hinzugefügt wird `<application>` (in **Properties/Androidmanifest.XML**) wie zuvor im Abschnitt [Anwenden der benutzerdefinierten Designs](#apply-the-custom-theme). Darüber hinaus, dass dieser Fehler kann verursacht werden, wenn die `Toolbar` Layout oder eines benutzerdefinierten Designs ist nicht ordnungsgemäß konfiguriert.


## <a name="related-links"></a>Verwandte Links

- [Lollipop-Symbolleiste (Beispiel)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [AppCompat-Symbolleiste (Beispiel)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
