---
title: Hinzufügen einer zweiten Symbolleiste
ms.prod: xamarin
ms.assetid: FCE0AD27-8B6B-47C6-AD19-2B1C12E1BBBF
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/15/2018
ms.openlocfilehash: 43030d4a221ceadf68c30df2e37449e7f996fa6d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029105"
---
# <a name="adding-a-second-toolbar"></a>Hinzufügen einer zweiten Symbolleiste

## <a name="overview"></a>Übersicht 

Der `Toolbar` kann die Aktionsleiste ersetzen, &ndash; er mehrmals innerhalb einer Aktivität verwendet werden kann. er kann an beliebiger Stelle auf dem Bildschirm angepasst werden, und er kann so konfiguriert werden, dass er nur eine partielle Breite des Bildschirms umfasst. In den folgenden Beispielen wird veranschaulicht, wie ein zweites `Toolbar` erstellt und am unteren Bildschirmrand platziert wird. In diesem `Toolbar` werden die Menü Elemente **Kopieren**, **Ausschneiden**und **Einfügen** implementiert. 

## <a name="define-the-second-toolbar"></a>Definieren der zweiten Symbolleiste 

Bearbeiten Sie die Layoutdatei " **Main. axml** ", und ersetzen Sie deren Inhalt durch den folgenden XML-Code:

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

Dieses XML fügt am unteren Bildschirmrand ein zweites `Toolbar` mit einem leeren `ImageView`, das die Mitte des Bildschirms füllt. Die Höhe dieses `Toolbar` wird auf die Höhe einer Aktionsleiste festgelegt: 

```xml
android:minHeight="?android:attr/actionBarSize"
```

Die Hintergrundfarbe dieses `Toolbar` wird auf eine Akzentfarbe festgelegt, die als nächstes definiert wird:

```xml
android:background="?android:attr/colorAccent
```

Beachten Sie, dass diese `Toolbar` auf einem anderen Design ("**meoverlay. Material. Dark. Aktionsleiste**") basiert als das, das von der `Toolbar` erstellt wird, [die beim Ersetzen des Aktionsleiste](~/android/user-interface/controls/tool-bar/replacing-the-action-bar.md) erstellt wurde &ndash; es nicht an das Fenster Dekor der Aktivität oder an das in verwendete Design gebunden ist. der erste `Toolbar`.

Bearbeiten Sie **Resources/Values/Styles. XML** , und fügen Sie der Format Definition die folgende Akzentfarbe hinzu: 

```xml
<item name="android:colorAccent">#C7A935</item>
```

Dadurch wird der unteren Symbolleiste eine dunkle Bernsteinfarbe angezeigt. Beim entwickeln und Ausführen der APP wird unten auf dem Bildschirm eine leere zweite Symbolleiste angezeigt: 

[![Screenshot der APP mit gelber zweiter Symbolleiste am unteren Bildschirmrand](adding-a-second-toolbar-images/01-second-toolbar-sml.png)](adding-a-second-toolbar-images/01-second-toolbar.png#lightbox)

## <a name="add-edit-menu-items"></a>Bearbeitungs Menü Elemente hinzufügen 

In diesem Abschnitt wird erläutert, wie dem unteren `Toolbar`Bearbeitungs Menü Elemente hinzugefügt werden. 

So fügen Sie einem sekundären `Toolbar`Menü Elemente hinzu: 

1. Fügen Sie den `mipmap-` Ordnern des App-Projekts (falls erforderlich) Menü Symbole hinzu.

2. Hiermit wird der Inhalt der Menü Elemente durch Hinzufügen einer zusätzlichen Menü Ressourcen Datei zu **Ressourcen/Menü**definiert. 

3. Suchen Sie in der `OnCreate`-Methode der Aktivität den `Toolbar` (durch Aufrufen von `FindViewById`), und erhöhen Sie die Menüs des `Toolbar`.

4. Implementieren Sie einen Click-Handler in `OnCreate` für die neuen Menü Elemente. 

In den folgenden Abschnitten wird dieser Prozess ausführlich veranschaulicht: Die Menü Elemente **Ausschneiden**, **Kopieren**und **Einfügen** werden dem unteren `Toolbar`hinzugefügt. 

### <a name="define-the-edit-menu-resource"></a>Definieren der Menü Ressource "Bearbeiten"

Erstellen Sie im Unterverzeichnis **Ressourcen/Menü** eine neue XML-Datei mit dem Namen **edit_menus. XML** , und ersetzen Sie den Inhalt durch den folgenden XML-Code:

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

Dieser XML-Code erstellt die Menü Elemente **Ausschneiden**, **Kopieren**und **Einfügen** (mithilfe von Symbolen, die den `mipmap-` Ordnern in zum [Ersetzen der Aktionsleiste](~/android/user-interface/controls/tool-bar/replacing-the-action-bar.md)hinzugefügt wurden).

### <a name="inflate-the-menus"></a>Die Menüs erhöhen

Fügen Sie am Ende der `OnCreate`-Methode in **MainActivity.cs**die folgenden Codezeilen hinzu: 

```csharp
var editToolbar = FindViewById<Toolbar>(Resource.Id.edit_toolbar);
editToolbar.Title = "Editing";
editToolbar.InflateMenu (Resource.Menu.edit_menus);
editToolbar.MenuItemClick += (sender, e) => {
    Toast.MakeText(this, "Bottom toolbar tapped: " + e.Item.TitleFormatted, ToastLength.Short).Show();
};
```

In diesem Code wird die in " **Main. axml**" definierte `edit_toolbar` Sicht angezeigt, deren Titel auf " **Bearbeitung**" festgelegt und die Menü Elemente (definiert in **edit_menus. XML**) vergrößert. Er definiert einen Menü Klick Handler, der einen Toast anzeigt, um anzugeben, welches Bearbeitungs Symbol angetippt wurde. 

Erstellen Sie die App, und führen Sie sie aus. Wenn die app ausgeführt wird, werden der oben hinzugefügte Text und die Symbole wie hier dargestellt angezeigt: 

[![Diagramm der unteren Symbolleiste mit Ausschneide-, Kopier-und Einfüge Symbolen](adding-a-second-toolbar-images/02-bottom-toolbar-sml.png)](adding-a-second-toolbar-images/02-bottom-toolbar.png#lightbox)

Wenn Sie auf das Menü " **Ausschneiden** " tippen, wird der folgende Toast angezeigt: 

[![Screenshot des Popups, der anzeigt, dass das Menü Symbol "Ausschneiden" ange](adding-a-second-toolbar-images/03-bottom-tapped-sml.png)](adding-a-second-toolbar-images/03-bottom-tapped.png#lightbox)

Wenn Sie auf eine der beiden Symbolleisten tippen, werden die resultierenden Elemente angezeigt: 

[![Screenshots von Popups zum Speichern, kopieren und Einfügen von Menü Elementen, die getippt werden](adding-a-second-toolbar-images/04-menu-action-sml.png)](adding-a-second-toolbar-images/04-menu-action.png#lightbox)

## <a name="the-up-button"></a>Die Schaltfläche nach oben 

Die meisten Android-Apps basieren auf der Schaltfläche **zurück** für die APP-Navigation. durch Drücken der Schaltfläche " **zurück** " wird der Benutzer zum vorherigen Bildschirm.
Sie können jedoch auch eine Schaltfläche "nach **oben** " bereitstellen, die es Benutzern leicht macht, zum Hauptbildschirm der APP zu navigieren. Wenn der Benutzer die Schaltfläche "nach **oben** " auswählt, wird der Benutzer in der APP-Hierarchie um eine höhere Ebene verschoben &ndash; d. h., die APP holt den Benutzer auf mehrere Aktivitäten im BackStack, anstatt zurück zur zuvor besuchten Aktivität zurückzukehren. 

Um die Schaltfläche nach **oben** in einer zweiten Aktivität zu aktivieren, die eine `Toolbar` als Aktionsleiste verwendet, können Sie die Methoden `SetDisplayHomeAsUpEnabled` und `SetHomeButtonEnabled` in der `OnCreate`-Methode der zweiten Aktivität abrufen:

```csharp
SetActionBar (toolbar);
...
ActionBar.SetDisplayHomeAsUpEnabled (true);
ActionBar.SetHomeButtonEnabled (true);
```

Das Codebeispiel für die [Unterstützung von V7-Symbolleiste](https://docs.microsoft.com/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar) zeigt die Schaltfläche **up** in Aktion In diesem Beispiel (das die im folgenden beschriebene AppCompat-Bibliothek verwendet) wird eine zweite-Aktivität implementiert, die **die Symbolleisten** -Schaltfläche für die hierarchische Navigation zurück zur vorherigen Aktivität verwendet. In diesem Beispiel wird die Schaltfläche nach **oben** durch die `DetailActivity` Start Schaltfläche aktiviert, indem die folgenden `SupportActionBar` Methodenaufrufe durchführen: 

```csharp
SetSupportActionBar (toolbar);
...
SupportActionBar.SetDisplayHomeAsUpEnabled (true);
SupportActionBar.SetHomeButtonEnabled (true);
```

Wenn der Benutzer von `MainActivity` zu `DetailActivity`navigiert, zeigt der `DetailActivity` eine nach- **oben** -Taste (nach links zeigenden Pfeil) an, wie im Screenshot gezeigt:

[![Screenshot: Beispiel für einen Pfeil nach links auf der Symbolleiste](adding-a-second-toolbar-images/05-up-button-sml.png)](adding-a-second-toolbar-images/05-up-button.png#lightbox)

Wenn **Sie** auf diese Schaltfläche klicken, wird die APP zu `MainActivity`zurückgegeben. Wenn Sie in einer komplexeren App mit mehreren Ebenen der Hierarchie auf diese Schaltfläche tippen, wird der Benutzer auf die nächsthöhere Ebene der APP und nicht auf den vorherigen Bildschirm zurückgegeben. 

## <a name="related-links"></a>Verwandte Links

- [Lollipop-Symbolleiste (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-toolbar)
- [AppCompat-Symbolleiste (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar)
