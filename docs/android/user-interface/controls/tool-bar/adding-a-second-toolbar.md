---
title: Hinzufügen einer zweiten Symbolleiste
ms.prod: xamarin
ms.assetid: FCE0AD27-8B6B-47C6-AD19-2B1C12E1BBBF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: f2255ab5ac7e153266093877a1c52bfc08927a09
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30766636"
---
# <a name="adding-a-second-toolbar"></a>Hinzufügen einer zweiten Symbolleiste


## <a name="overview"></a>Übersicht 

Die `Toolbar` möglich, mehr als ersetzen die Aktionsleiste &ndash; mehrmals in einer Aktivität verwendet werden, kann es sein, werden für die Platzierung an einer beliebigen Stelle auf dem Bildschirm angepasst, und es kann konfiguriert werden, damit nur eine teilweise Breite des Bildschirms umfassen. Die folgenden Beispiele veranschaulichen, wie erstellen Sie eine zweite `Toolbar` und platziert ihn am unteren Rand des Bildschirms. Dies `Toolbar` implementiert **Kopie**, **Ausschneiden**, und **einfügen** Menüelemente. 


## <a name="define-the-second-toolbar"></a>Definieren Sie die zweite Symbolleiste 

Bearbeiten Sie die Layoutdatei **Main.axml** und seinen Inhalt mit mit den folgenden XML-Code ersetzen:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <include
        android:id="@+id/toolbar"
        layout="@layout/toolbar" />
    <LinearLayout
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:id="@+id/main_content"
        android:layout_below="@id/toolbar">
      <ImageView
          android:layout_width="fill_parent"
          android:layout_height="0dp"
          android:layout_weight="1" />
      <Toolbar
          android:id="@+id/edit_toolbar"
          android:minHeight="?android:attr/actionBarSize"
          android:background="?android:attr/colorAccent"
          android:theme="@android:style/ThemeOverlay.Material.Dark.ActionBar"
          android:layout_width="match_parent"
          android:layout_height="wrap_content" />
    </LinearLayout>
</RelativeLayout>
```

Diese XML-Daten Fügt eine zweite `Toolbar` zum unteren Rand des Bildschirms mit einem leeren `ImageView` Füllen von der Mitte des Bildschirms. Die Höhe dieser `Toolbar` festgelegt ist, der die Höhe des eine Aktionsleiste: 

```xml
android:minHeight="?android:attr/actionBarSize"
```

Die Farbe des Hintergrunds dieses `Toolbar` festgelegt ist, um eine Akzentfarbe, die als Nächstes definiert wird:

```xml
android:background="?android:attr/colorAccent
```

Beachten Sie, die von diesem `Toolbar` basiert auf einem anderen Design (**ThemeOverlay.Material.Dark.ActionBar**) als die von der `Toolbar` erstellt [Ersetzen der Aktionsleiste](~/android/user-interface/controls/tool-bar/replacing-the-action-bar.md) &ndash;es nicht gebunden, auf die Aktivität im Fenster Dekoration oder zum Design verwendet, in der ersten `Toolbar`.

Bearbeiten Sie **Resources/values/styles.xml** und die Formatdefinition folgenden Akzentfarbe hinzuzufügen: 

```xml
<item name="android:colorAccent">#C7A935</item>
```

Dadurch werden der unteren Symbolleiste ein dunkles gelb. Erstellen und Ausführen der app können Sie eine leere zweite Symbolleiste am unteren Rand des Bildschirms angezeigt: 

[![Screenshot der app mit dem gelben zweite Symbolleiste am unteren Rand des Bildschirms](adding-a-second-toolbar-images/01-second-toolbar-sml.png)](adding-a-second-toolbar-images/01-second-toolbar.png#lightbox)


 
## <a name="add-edit-menu-items"></a>Hinzufügen von Menüelementen bearbeiten 

In diesem Abschnitt wird erläutert, wie im unteren Bereich Menüelemente bearbeiten hinzuzufügende `Toolbar`. 

Zum Hinzufügen von Menüelementen zu einem sekundären `Toolbar`: 

1.  Fügen Sie im Menüsymbole auf der `mipmap-` Ordner des app-Projekts (falls erforderlich).

2.  Definieren Sie den Inhalt der Menüelemente durch Hinzufügen einer Ressourcendatei zusätzliche Menü zu **Ressourcen/Menü**. 

3.  In der Aktivitätssymbols `OnCreate` -Methode, suchen die `Toolbar` (durch Aufrufen `FindViewById`) vergrößert werden soll, und die `Toolbar`des Menüs.

4.  Implementieren Sie einen Click-Ereignishandler in `OnCreate` für die neuen Menüelemente. 

Die folgenden Abschnitte zeigen diesen Vorgang im Detail: **Ausschneiden**, **Kopie**, und **einfügen** Menüelemente werden hinzugefügt, die letzte `Toolbar`. 



### <a name="define-the-edit-menu-resource"></a>Definieren Sie die Menüressource bearbeiten

In der **Ressourcen/Menü** Unterverzeichnis, erstellen Sie eine neue XML-Datei namens **edit_menus.xml** und Ersetzen Sie den Inhalt durch folgendes XML:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item
       android:id="@+id/menu_cut"
       android:icon="@mipmap/ic_menu_cut_holo_dark"
       android:showAsAction="ifRoom"
       android:title="Cut" />
  <item
       android:id="@+id/menu_copy"
       android:icon="@mipmap/ic_menu_copy_holo_dark"
       android:showAsAction="ifRoom"
       android:title="Copy" />
  <item
       android:id="@+id/menu_paste"
       android:icon="@mipmap/ic_menu_paste_holo_dark"
       android:showAsAction="ifRoom"
       android:title="Paste" />
</menu>
```

Diesen XML-Code erstellt die **Ausschneiden**, **Kopie**, und **einfügen** Menüelemente (mit Symbolen, die hinzugefügt wurden, die `mipmap-` Ordner im [Ersetzen der Aktionsleiste ](~/android/user-interface/controls/tool-bar/replacing-the-action-bar.md)).



### <a name="inflate-the-menus"></a>Vergrößern Sie die Menüs

Am Ende der `OnCreate` Methode im **MainActivity.cs**, fügen Sie die folgenden Codezeilen hinzu: 

```csharp
var editToolbar = FindViewById<Toolbar>(Resource.Id.edit_toolbar);
editToolbar.Title = "Editing";
editToolbar.InflateMenu (Resource.Menu.edit_menus);
editToolbar.MenuItemClick += (sender, e) => {
    Toast.MakeText(this, "Bottom toolbar tapped: " + e.Item.TitleFormatted, ToastLength.Short).Show();
};
```

Dieser Code sucht den `edit_toolbar` Sicht definiert, die **Main.axml**, wird den Titel auf **bearbeiten**, und vergrößert die Menüelemente (definiert **edit_menus.xml**). Definiert ein Menü click-Ereignishandler, die zeigt einen Toast, um anzugeben, welches Symbol "Bearbeiten" abgerufen wurde. 

Erstellen Sie die App, und führen Sie sie aus. Wenn die app ausgeführt wird, werden der Text und Symbole, die oben hinzugefügten angezeigt, wie hier gezeigt: 

[![Diagramm der unteren Symbolleiste mit Ausschneiden, kopieren und Einfügen von Symbolen](adding-a-second-toolbar-images/02-bottom-toolbar-sml.png)](adding-a-second-toolbar-images/02-bottom-toolbar.png#lightbox)

Tippen Sie auf die **Ausschneiden** Menüsymbol bewirkt, dass die folgenden Toast angezeigt werden: 

[![Screenshot der Toast zeigt an, dass wurde das Symbol "Ausschneiden-Menü" abgerufen werden.](adding-a-second-toolbar-images/03-bottom-tapped-sml.png)](adding-a-second-toolbar-images/03-bottom-tapped.png#lightbox)

Tippen auf Menüelemente auf einer Symbolleiste wird die resultierende Popups angezeigt: 

[![Screenshots des Popups für speichern, kopieren, und fügen Sie Menüelemente werden abgerufen](adding-a-second-toolbar-images/04-menu-action-sml.png)](adding-a-second-toolbar-images/04-menu-action.png#lightbox)



## <a name="the-up-button"></a>Die Schaltfläche "oben" 

Abhängig von den meisten Android-apps die **wieder** Schaltfläche für die app Navigation; Drücken der **wieder** Schaltfläche wird der Benutzer zum vorherigen Bildschirm.
Allerdings kann auch bieten möchten ein **einrichten** Schaltfläche, die Benutzern das Navigieren "bis", um die app-Hauptbildschirm erleichtert. Wenn der Benutzer wählt die **einrichten** Schaltfläche der Benutzer auf einer höheren Ebene in der app-Hierarchie nach oben &ndash; , also die app wird dem Benutzer wieder mehrere Aktivitäten in die rückwärts-anstelle von Abholvorgänge zurück, die zuvor besucht Aktivität. 

So aktivieren Sie die **einrichten** Schaltfläche in einer zweiten Aktivität, die verwendet eine `Toolbar` rufen Sie als seine Aktionsleiste auf die `SetDisplayHomeAsUpEnabled` und `SetHomeButtonEnabled` Methoden in der zweiten Aktivitätssymbols `OnCreate` Methode:

```csharp
SetActionBar (toolbar);
...
ActionBar.SetDisplayHomeAsUpEnabled (true);
ActionBar.SetHomeButtonEnabled (true);
```

Die [unterstützen v7-Symbolleiste](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/) Codebeispiel veranschaulicht die **einrichten** der Schaltfläche. In diesem Beispiel (befindlichen AppCompat-Bibliothek, die nachfolgend beschrieben) implementiert eine zweite Aktivität, die die Symbolleiste verwendet **einrichten** Schaltfläche für die hierarchische Navigation zurück zur vorherigen Aktivität. In diesem Beispiel wird die `DetailActivity` Schaltfläche "Start" ermöglicht die **einrichten** Schaltfläche, indem Sie die folgenden `SupportActionBar` Methodenaufrufe: 

```csharp
SetSupportActionBar (toolbar);
...
SupportActionBar.SetDisplayHomeAsUpEnabled (true);
SupportActionBar.SetHomeButtonEnabled (true);
```

Wenn der Benutzer navigiert von `MainActivity` auf `DetailActivity`, die `DetailActivity` zeigt ein **einrichten** Schaltfläche (zeigenden Pfeil nach links), wie im Screenshot dargestellt:

[![Bildschirmabbildung von Beispiel für eine nach-oben Schaltfläche-links-Taste auf der Symbolleiste](adding-a-second-toolbar-images/05-up-button-sml.png)](adding-a-second-toolbar-images/05-up-button.png#lightbox)

Tippen Sie auf diese **einrichten** Schaltfläche bewirkt, dass die app zurückkehren zu `MainActivity`. In eine komplexere Anwendung mit mehreren Ebenen der Hierarchie würde Tippen Sie auf diese Schaltfläche den Benutzer auf die nächsthöhere Ebene in der app anstatt zum vorherigen Bildschirm zurück. 



## <a name="related-links"></a>Verwandte Links

- [Lollipop-Symbolleiste (Beispiel)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [AppCompat-Symbolleiste (Beispiel)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
