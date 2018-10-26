---
title: Hinzufügen einer zweiten Symbolleiste
ms.prod: xamarin
ms.assetid: FCE0AD27-8B6B-47C6-AD19-2B1C12E1BBBF
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/15/2018
ms.openlocfilehash: b8da13afb7fd8d7198e8bfe7476b40a5cd09769a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112549"
---
# <a name="adding-a-second-toolbar"></a>Hinzufügen einer zweiten Symbolleiste


## <a name="overview"></a>Übersicht 

Die `Toolbar` erreichen mehr als ersetzen die Aktionsleiste &ndash; mehrmals innerhalb einer Aktivität verwendet werden, es kann sein, werden für die Platzierung an einer beliebigen Stelle auf dem Bildschirm angepasst, und es kann nur eine partielle Breite des Bildschirms erstreckt konfiguriert werden. Die folgenden Beispiele veranschaulichen, wie Sie ein zweites `Toolbar` und platzieren Sie sie am unteren Rand des Bildschirms. Dies `Toolbar` implementiert **Kopie**, **Ausschneiden**, und **einfügen** Menüelemente. 


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

Dieser XML-Code fügt eine zweite `Toolbar` zum unteren Rand des Bildschirms mit einer leeren `ImageView` füllen die Mitte des Bildschirms. Die Höhe dieses `Toolbar` festgelegt ist, der die Höhe des eine Aktionsleiste: 

```xml
android:minHeight="?android:attr/actionBarSize"
```

Die Hintergrundfarbe der `Toolbar` festgelegt ist, um eine Farbe für Akzente, die als Nächstes definiert werden:

```xml
android:background="?android:attr/colorAccent
```

Beachten Sie, das von diesem `Toolbar` basiert auf einem anderen Design (**ThemeOverlay.Material.Dark.ActionBar**) als durch die `Toolbar` in erstellt [Ersetzen der Aktionsleiste](~/android/user-interface/controls/tool-bar/replacing-the-action-bar.md) &ndash;es ist nicht gebunden, auf der Aktivität im Fenster Decor oder auf das Design verwendet, in der ersten `Toolbar`.

Bearbeiten Sie **Resources/values/styles.xml** und der Style-Definition die folgenden Akzentfarbe hinzu: 

```xml
<item name="android:colorAccent">#C7A935</item>
```

Dadurch wird der unteren Symbolleiste eine dunkle gelbe Farbe. Erstellen und Ausführen der app können Sie eine leere zweite Symbolleiste am unteren Rand des Bildschirms angezeigt: 

[![Screenshot der app mit dem gelben zweite Symbolleiste am unteren Rand des Bildschirms](adding-a-second-toolbar-images/01-second-toolbar-sml.png)](adding-a-second-toolbar-images/01-second-toolbar.png#lightbox)


 
## <a name="add-edit-menu-items"></a>Hinzufügen von Menüelementen bearbeiten 

In diesem Abschnitt wird erläutert, wie im unteren Bereich Menüelemente bearbeiten hinzuzufügende `Toolbar`. 

Hinzufügen von Menüelementen auf eine sekundäre Datenbank `Toolbar`: 

1.  Fügen Sie im Menüsymbole, um die `mipmap-` Ordner des app-Projekts (falls erforderlich).

2.  Definieren Sie den Inhalt der Menüelemente durch Hinzufügen einer zusätzlichen Menü-Ressourcendatei zu **ressourcenmenü/**. 

3.  In der Aktivitäts `OnCreate` -Methode finden Sie die `Toolbar` (durch Aufrufen von `FindViewById`) und vergrößert werden soll die `Toolbar`des Menüs.

4.  Implementieren Sie einen Click-Ereignishandler in `OnCreate` für die neuen Menüelemente. 

Die folgenden Abschnitte zeigen, diesen Prozess im Detail: **Ausschneiden**, **Kopie**, und **einfügen** Menüelemente werden hinzugefügt, die nach-unten `Toolbar`. 



### <a name="define-the-edit-menu-resource"></a>Definieren Sie die Menüressource bearbeiten

In der **ressourcenmenü/** Unterverzeichnis, erstellen Sie eine neue XML-Datei namens **edit_menus.xml** und Ersetzen Sie den Inhalt durch folgendes XML:

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

Dieser XML-Code erstellt die **Ausschneiden**, **Kopie**, und **einfügen** Menüelemente (mithilfe von Symbolen, die hinzugefügt wurden, die `mipmap-` Ordner im [Ersetzen der Aktionsleiste ](~/android/user-interface/controls/tool-bar/replacing-the-action-bar.md)).



### <a name="inflate-the-menus"></a>Vergrößerung der Menüs

Am Ende der `OnCreate` -Methode in der **"mainactivity.cs"**, fügen Sie die folgenden Codezeilen hinzu: 

```csharp
var editToolbar = FindViewById<Toolbar>(Resource.Id.edit_toolbar);
editToolbar.Title = "Editing";
editToolbar.InflateMenu (Resource.Menu.edit_menus);
editToolbar.MenuItemClick += (sender, e) => {
    Toast.MakeText(this, "Bottom toolbar tapped: " + e.Item.TitleFormatted, ToastLength.Short).Show();
};
```

Dieser Code sucht nach der `edit_toolbar` anzeigen, die in definierten **Main.axml**, legt den Titel auf **bearbeiten**, und vergrößert die Menüelemente (definiert **edit_menus.xml**). Definiert ein Menü click-Ereignishandler, die ein Popup, um anzugeben, welches Symbol Bearbeiten angetippt wurde anzeigt. 

Erstellen Sie die App, und führen Sie sie aus. Wenn die app ausgeführt wird, werden der Text und Symbole, die weiter oben hinzugefügt angezeigt, wie hier gezeigt: 

[![Diagramm der unteren Symbolleiste mit Ausschneiden, kopieren und Einfügen von Symbolen](adding-a-second-toolbar-images/02-bottom-toolbar-sml.png)](adding-a-second-toolbar-images/02-bottom-toolbar.png#lightbox)

Durch Tippen auf die **Ausschneiden** Symbol "Menü" bewirkt, dass den folgenden Toast angezeigt werden: 

[![Screenshot des Toasts gibt an, dass das Menüsymbol Ausschneiden angetippt wurde](adding-a-second-toolbar-images/03-bottom-tapped-sml.png)](adding-a-second-toolbar-images/03-bottom-tapped.png#lightbox)

Durch Tippen auf die Menüelemente in beiden Symbolleiste wird das resultierende Popups angezeigt: 

[![Screenshots des Popups für speichern, kopieren, und fügen Sie Menüelemente getippt wird](adding-a-second-toolbar-images/04-menu-action-sml.png)](adding-a-second-toolbar-images/04-menu-action.png#lightbox)



## <a name="the-up-button"></a>Die oben-Taste 

Die meisten Android-apps basieren auf der **wieder** für app-Navigation Schaltfläche; durch Drücken der **wieder** Schaltfläche wird der Benutzer zum vorherigen Bildschirm.
Aber Sie können auch bereitstellen möchten eine **einrichten** Schaltfläche, die zum Navigieren zum Hauptbildschirm von der app "oben" vereinfacht. Wenn der Benutzer wählt die **einrichten** Schaltfläche der Benutzer nach oben zu einer höheren Ebene in der app-Hierarchie verschoben &ndash; , also die app wird dem Benutzer wieder mehrere Aktivitäten in den Backstack anstelle von Abholvorgänge wieder an die zuvor besuchten Aktivität. 

So aktivieren Sie die **einrichten** Schaltfläche in einer zweiten Aktivität, die verwendet eine `Toolbar` aufrufen, als der Aktionsleiste der `SetDisplayHomeAsUpEnabled` und `SetHomeButtonEnabled` Methoden in der zweiten Aktivitätssymbols `OnCreate` Methode:

```csharp
SetActionBar (toolbar);
...
ActionBar.SetDisplayHomeAsUpEnabled (true);
ActionBar.SetHomeButtonEnabled (true);
```

Die [Unterstützung des v7-Symbolleiste](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/) Codebeispiel wird veranschaulicht, die **einrichten** der Schaltfläche. In diesem Beispiel (das die Anwendungsentwicklung AppCompat-Bibliothek verwendet wird) implementiert, eine zweite Aktivität, die die Symbolleiste verwendet **einrichten** Schaltfläche für die hierarchische Navigation zurück zur vorherigen Aktivität. In diesem Beispiel die `DetailActivity` Schaltfläche "Start" ermöglicht die **einrichten** Schaltfläche dazu die folgenden `SupportActionBar` Methodenaufrufe: 

```csharp
SetSupportActionBar (toolbar);
...
SupportActionBar.SetDisplayHomeAsUpEnabled (true);
SupportActionBar.SetHomeButtonEnabled (true);
```

Wenn der Benutzer navigiert `MainActivity` zu `DetailActivity`, `DetailActivity` zeigt eine **einrichten** Schaltfläche (nach links zeigenden Pfeil), wie im Screenshot dargestellt:

[![Beispielhafter Screenshot von einer linken Schaltfläche Pfeil auf der Symbolleiste](adding-a-second-toolbar-images/05-up-button-sml.png)](adding-a-second-toolbar-images/05-up-button.png#lightbox)

Tippen Sie auf diese **einrichten** -Schaltfläche bewirkt, dass die app wieder `MainActivity`. In eine komplexere Anwendung mit mehreren Ebenen der Hierarchie würde auf diese Schaltfläche den Benutzer auf die nächsthöhere Ebene in der app und nicht auf dem vorherigen Bildschirm zurück. 



## <a name="related-links"></a>Verwandte Links

- [Lollipop-Symbolleiste (Beispiel)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [AppCompat-Symbolleiste (Beispiel)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
