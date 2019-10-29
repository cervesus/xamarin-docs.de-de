---
title: Ersetzen der Aktionsleiste
ms.prod: xamarin
ms.assetid: 5341D28E-B203-478D-8464-6FAFDC3A4110
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/27/2018
ms.openlocfilehash: f20568b5d76fcc1788d19497e372bcd0cecc61ff
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029084"
---
# <a name="replacing-the-action-bar"></a>Ersetzen der Aktionsleiste

## <a name="overview"></a>Übersicht

Eine der häufigsten Verwendungsmöglichkeiten für die `Toolbar` besteht darin, die Standard Aktionsleiste durch eine benutzerdefinierte `Toolbar` zu ersetzen (wenn ein neues Android-Projekt erstellt wird, wird die Standard Aktionsleiste verwendet). Da die `Toolbar` das Hinzufügen von Marken Logos, Titeln, Menü Elementen, Navigations Schaltflächen und sogar benutzerdefinierten Ansichten zum Bereich der APP-Leiste auf der Benutzeroberfläche einer Aktivität ermöglicht, bietet Sie ein bedeutendes Upgrade auf der Standard Aktionsleiste.

So ersetzen Sie die Standard Aktionsleiste einer APP durch eine `Toolbar`: 

1. Erstellen Sie ein neues benutzerdefiniertes Design, und ändern Sie die Eigenschaften der APP, sodass dieses neue Design verwendet wird. 

2. Deaktivieren Sie das `windowActionBar`-Attribut im benutzerdefinierten Design, und aktivieren Sie das `windowNoTitle`-Attribut.

3. Definieren Sie ein Layout für die `Toolbar`.

4. Fügen Sie das `Toolbar` Layout in die Haupt-und **axml** -Layoutdatei der Aktivität ein. 

5. Fügen Sie der `OnCreate`-Methode der Aktivität Code hinzu, um die `Toolbar` zu suchen und `SetActionBar` aufzurufen, um die `ToolBar` als Aktionsleiste zu installieren.

In den folgenden Abschnitten wird dieser Prozess ausführlich erläutert. Eine einfache APP wird erstellt, und die Aktionsleiste wird durch eine angepasste `Toolbar`ersetzt. 

## <a name="start-an-app-project"></a>Starten eines App-Projekts

Erstellen Sie ein neues Android-Projekt mit dem Namen **toolbarfun** (Weitere Informationen zum Erstellen eines neuen Android-Projekts finden Sie [unter Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) ). Nachdem dieses Projekt erstellt wurde, legen Sie die Ziel-und mindestens Android-API-Ebenen auf **Android 5,0 (API-Ebene 21-Lollipop)** oder höher fest. Weitere Informationen zum Festlegen von Android-Versions Ebenen finden Sie Untergrund Legendes zu [Android-API-Ebenen](~/android/app-fundamentals/android-api-levels.md). Wenn die App erstellt und ausgeführt wird, wird die Standard Aktionsleiste angezeigt, wie im folgenden Screenshot zu sehen:

[![Screenshot der Standard Aktionsleiste](replacing-the-action-bar-images/01-before-sml.png)](replacing-the-action-bar-images/01-before.png#lightbox)

## <a name="create-a-custom-theme"></a>Erstellen eines benutzerdefinierten Designs

Öffnen Sie das Verzeichnis " **Resources/Values** ", und erstellen Sie eine neue Datei namens " **Styles. XML**". Ersetzen Sie den Inhalt durch den folgenden XML-Code: 

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

Dieser XML-Code definiert ein neues benutzerdefiniertes Design namens **mytheme** , das auf dem Design **. Material. Light. darkaktionbar** -Design in Lollipop basiert. Das `windowNoTitle`-Attribut wird auf `true` festgelegt, um die Titelleiste auszublenden: 

```xml
<item name="android:windowNoTitle">true</item>
```

Zum Anzeigen der benutzerdefinierten Symbolleiste müssen die Standard `ActionBar` deaktiviert werden: 

```xml
<item name="android:windowActionBar">false</item>
```

Ein olivgrünes `colorPrimary` Einstellung wird für die Hintergrundfarbe der Symbolleiste verwendet: 

```xml
<item name="android:colorPrimary">#5A8622</item>
```

## <a name="apply-the-custom-theme"></a>Anwenden des benutzerdefinierten Designs

Bearbeiten Sie " **Properties"/"androidmanifest. XML** ", und fügen Sie das folgende `android:theme` Attribut zum `<application>`-Element hinzu, damit die APP das `MyTheme` benutzerdefinierte Design verwendet: 

```xml
<application android:label="@string/app_name" android:theme="@style/MyTheme"></application>
```

Weitere Informationen zum Anwenden eines benutzerdefinierten Designs auf eine App finden [Sie unter Verwenden von benutzerdefinierten](~/android/user-interface/material-theme.md#customtheme)Designs. 

## <a name="define-a-toolbar-layout"></a>Definieren eines Toolbar-Layouts

Erstellen Sie im **Ressourcen-/Layout-Verzeichnis** eine neue Datei mit dem Namen **Toolbar. XML**. Ersetzen Sie den Inhalt durch den folgenden XML-Code: 

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

Dieser XML-Code definiert das benutzerdefinierte `Toolbar`, das die Standard Aktionsleiste ersetzt. Die Mindesthöhe der `Toolbar` wird auf die Größe der Aktionsleiste festgelegt, die ersetzt wird: 

```csharp
android:minHeight="?android:attr/actionBarSize"
```

Die Hintergrundfarbe der `Toolbar` wird auf die zuvor in **Styles. XML**definierte olivgrüne Farbe festgelegt:

```csharp
android:background="?android:attr/colorPrimary"
```

Beginnend mit Lollipop kann das `android:theme`-Attribut verwendet werden, um eine einzelne Ansicht zu formatieren. Mithilfe der in Lollipop eingeführten `ThemeOverlay.Material` Designs ist es möglich, die Standard `Theme.Material` Designs zu überlagern und relevante Attribute zu überschreiben, um Sie entweder hell oder dunkel zu machen. In diesem Beispiel verwendet die `Toolbar` ein dunkles Design, sodass der Inhalt in der Farbe hell ist: 

```csharp
android:theme="@android:style/ThemeOverlay.Material.Dark.ActionBar"
```

Diese Einstellung wird verwendet, damit Menü Elemente mit der dunkleren Hintergrundfarbe im Gegensatz zueinander stehen.

## <a name="include-the-toolbar-layout"></a>Symbolleisten Layout einschließen

Bearbeiten Sie die Layoutdatei **Resources/Layout/Main. axml** , und ersetzen Sie deren Inhalt durch den folgenden XML-Code:

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

Dieses Layout umfasst das in **Toolbar. XML** definierte `Toolbar` und verwendet eine `RelativeLayout`, um anzugeben, dass die `Toolbar` ganz oben in der Benutzeroberfläche (oberhalb der Schaltfläche) platziert werden soll. 

## <a name="find-and-activate-the-toolbar"></a>Suchen und Aktivieren der Symbolleiste

Bearbeiten Sie **MainActivity.cs** , und fügen Sie die folgende using-Anweisung hinzu:

```csharp
using Android.Views;
```

Fügen Sie außerdem am Ende der `OnCreate`-Methode die folgenden Codezeilen hinzu:

```csharp
var toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
SetActionBar(toolbar);
ActionBar.Title = "My Toolbar";
```

Mit diesem Code wird der `Toolbar` gefunden und `SetActionBar` aufgerufen, damit die `Toolbar` standardmäßige Aktionsleisten Merkmale annimmt. Der Titel der Symbolleiste wird in **Meine Symbolleiste**geändert. Wie in diesem Codebeispiel gezeigt, kann auf den `ToolBar` direkt als Aktionsleiste verwiesen werden. Kompilieren Sie diese APP, und führen Sie Sie aus, &ndash; die angepasste `Toolbar` anstelle der Standard Aktionsleiste angezeigt wird: 

[![Screenshot der angepassten Symbolleiste mit einem grünen Farbschema](replacing-the-action-bar-images/02-after-sml.png)](replacing-the-action-bar-images/02-after.png#lightbox)

Beachten Sie, dass die `Toolbar` unabhängig vom `Theme.Material.Light.DarkActionBar` Design formatiert ist, das auf den Rest der APP angewendet wird. 

Wenn beim Ausführen der App eine Ausnahme auftritt, lesen Sie den Abschnitt zur [Problem](#troubleshooting) Behandlung weiter unten.

## <a name="add-menu-items"></a>Menü Elemente hinzufügen 

In diesem Abschnitt werden die Menüs der `Toolbar`hinzugefügt. Der obere rechte Bereich der `ToolBar` ist für Menü Elemente reserviert &ndash; jedes Menü Element (auch als *Aktions Element*bezeichnet) kann eine Aktion innerhalb der aktuellen Aktivität ausführen oder eine Aktion im Namen der gesamten app ausführen. 

So fügen Sie dem `Toolbar`Menüs hinzu: 

1. Fügen Sie (falls erforderlich) Menü Symbole zu den `mipmap-` Ordnern des App-Projekts hinzu. Google stellt auf der Seite [Material Symbole](https://design.google.com/icons/) einen Satz von freien Menü Symbolen bereit. 

2. Definieren Sie den Inhalt der Menü Elemente, indem Sie im **Menü Ressourcen/Menü**eine neue Menü Ressourcen Datei hinzufügen. 

3. Implementieren Sie die `OnCreateOptionsMenu`-Methode der-Aktivität, &ndash; diese Methode die Menü Elemente auffüllt. 

4. Implementieren Sie die `OnOptionsItemSelected`-Methode der-Aktivität &ndash; diese Methode führt eine Aktion aus, wenn ein Menü Element getippt wird. 

In den folgenden Abschnitten wird dieser Prozess ausführlich veranschaulicht, indem die Menü Elemente **Bearbeiten** und **Speichern** der angepassten `Toolbar`hinzugefügt werden. 

### <a name="install-menu-icons"></a>Menü Symbole installieren

Wenn Sie mit der `ToolbarFun` Beispiel-App fortfahren, fügen Sie dem App-Projekt Menü Symbole hinzu. Laden [Sie Symbolleisten Symbole](https://github.com/xamarin/monodroid-samples/blob/master/Supportv7/AppCompat/Toolbar/Resources/toolbar-icons-plus.zip?raw=true)herunter, entzippen Sie Sie, und kopieren Sie den Inhalt der extrahierten *MipMap-* Ordner in das Projekt *MipMap-* Folders unter **toolbarfun/Resources** , und schließen Sie jede hinzugefügte Symbol Datei in das Projekt ein.

### <a name="define-a-menu-resource"></a>Definieren einer Menü Ressource

Erstellen Sie ein neues **Menü** Unterverzeichnis unter **Ressourcen**. Erstellen Sie im **Menü** Unterverzeichnis eine neue Menü Ressourcen Datei mit dem Namen **top_menus. XML** , und ersetzen Sie deren Inhalt durch den folgenden XML-Code: 

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

Dieser XML-Code erstellt drei Menü Elemente:

- Ein **Bearbeitungs** Menü Element, das das `ic_action_content_create.png` Symbol (einen Stift) verwendet. 

- Ein Menü Element **Speichern** , das das `ic_action_content_save.png` Symbol (eine Diskette) verwendet. 

- Ein Menü Element " **Einstellungen** ", das kein Symbol enthält.

Die `showAsAction` Attribute der Menü Elemente **Bearbeiten** und **Speichern** sind auf `ifRoom` festgelegt &ndash; diese Einstellung bewirkt, dass diese Menü Elemente in der `Toolbar` angezeigt werden, wenn ausreichend Platz für die Anzeige vorhanden ist. Mit dem Menü Element " **Einstellungen** " wird `showAsAction` auf `never` festgelegt &ndash; Dies bewirkt, dass das Menü " **Einstellungen** " im *Überlauf* Menü angezeigt wird (drei vertikale Punkte). 

### <a name="implement-oncreateoptionsmenu"></a>Onkreateoptionsmenu implementieren

Fügen Sie **MainActivity.cs**die folgende Methode hinzu:

```csharp
public override bool OnCreateOptionsMenu(IMenu menu)
{
    MenuInflater.Inflate(Resource.Menu.top_menus, menu);
    return base.OnCreateOptionsMenu(menu);
}
```

Android Ruft die `OnCreateOptionsMenu`-Methode auf, sodass die APP die Menü Ressource für eine Aktivität angeben kann. In dieser Methode wird die **top_menus. XML** -Ressource in den bestandenen `menu`aufgeblasen. Dieser Code bewirkt, dass die neuen Menü Elemente **Bearbeiten**, **Speichern**und **Einstellungen** in der `Toolbar`angezeigt werden. 

### <a name="implement-onoptionsitemselected"></a>Implementieren von onoptionsitemselected

Fügen Sie **MainActivity.cs**die folgende Methode hinzu:

```csharp
public override bool OnOptionsItemSelected(IMenuItem item)
{
    Toast.MakeText(this, "Action selected: " + item.TitleFormatted,
        ToastLength.Short).Show();
    return base.OnOptionsItemSelected(item);
}
```

Wenn ein Benutzer auf ein Menü Element tippt, ruft Android die `OnOptionsItemSelected`-Methode auf und übergibt das Menü Element, das ausgewählt wurde. In diesem Beispiel zeigt die Implementierung nur einen Toast an, um anzugeben, welches Menü Element getippt wurde. 

Erstellen und führen Sie `ToolbarFun` aus, um die neuen Menü Elemente in der Symbolleiste anzuzeigen. Die `Toolbar` zeigt nun drei Menü Symbole an, wie in diesem Screenshot gezeigt: 

[![Diagramm, das die Positionen der Menü Elemente bearbeiten, speichern und Überlauf veranschaulicht](replacing-the-action-bar-images/04-menu-items-sml.png)](replacing-the-action-bar-images/04-menu-items.png#lightbox)

Wenn ein Benutzer auf das Menü Element **Bearbeiten** tippt, wird ein Toast angezeigt, der anzeigt, dass die `OnOptionsItemSelected`-Methode aufgerufen wurde: 

[![Screenshot von Popup, der angezeigt wird, wenn Element bearbeiten angetippt wird](replacing-the-action-bar-images/05-toast-displayed-sml.png)](replacing-the-action-bar-images/05-toast-displayed.png#lightbox)

Wenn ein Benutzer auf das Überlauf Menü tippt, wird das Menü Element " **Einstellungen** " angezeigt. In der Regel sollten weniger häufige Aktionen im Überlauf Menü abgelegt werden &ndash; in diesem Beispiel wird das Überlauf Menü für **Einstellungen** verwendet, da es nicht so oft wie " **Bearbeiten** und **Speichern**" verwendet wird: 

[![Screenshot des Menü Elements "Einstellungen", das im Menü "Überlauf" angezeigt wird](replacing-the-action-bar-images/06-preferences-sml.png)](replacing-the-action-bar-images/06-preferences.png#lightbox)

Weitere Informationen zu Android-Menüs finden Sie im Thema Android Developer- [Menüs](https://developer.android.com/guide/topics/ui/menus.html) . 

## <a name="troubleshooting"></a>Problembehandlung

Die folgenden Tipps können helfen, Probleme zu beheben, die auftreten können, wenn Sie die Aktionsleiste durch eine Symbolleiste ersetzen.

### <a name="activity-already-has-an-action-bar"></a>Die Aktivität verfügt bereits über eine Aktionsleiste

Wenn die APP nicht ordnungsgemäß für die Verwendung eines benutzerdefinierten Designs konfiguriert ist, wie in [Anwenden des benutzerdefinierten](#apply-the-custom-theme)Designs erläutert, kann beim Ausführen der APP die folgende Ausnahme auftreten:

![Fehler, der auftreten kann, wenn ein benutzerdefiniertes Design nicht verwendet wird](replacing-the-action-bar-images/03-theme-not-defined.png)

Außerdem kann eine Fehlermeldung wie die folgende generiert werden: _Java. lang. IllegalStateException: Diese Aktivität hat bereits eine Aktionsleiste, die vom Fenster Dekor bereitgestellt wird._ 

Um diesen Fehler zu beheben, stellen Sie sicher, dass das `android:theme`-Attribut für das benutzerdefinierte Design zu `<application>` (in " **Properties/androidmanifest. XML**") hinzugefügt wird, wie zuvor in [Anwenden des benutzerdefinierten](#apply-the-custom-theme)Designs beschrieben. Außerdem kann dieser Fehler auftreten, wenn das `Toolbar` Layout oder das benutzerdefinierte Design nicht ordnungsgemäß konfiguriert ist.

## <a name="related-links"></a>Verwandte Links

- [Lollipop-Symbolleiste (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-toolbar)
- [AppCompat-Symbolleiste (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar)
