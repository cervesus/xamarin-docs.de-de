---
title: Ersetzen der Aktionsleiste
ms.prod: xamarin
ms.assetid: 5341D28E-B203-478D-8464-6FAFDC3A4110
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/27/2018
ms.openlocfilehash: 19ac5a023b1f97b2e08bbe1821a2b9259280fc98
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68645150"
---
# <a name="replacing-the-action-bar"></a>Ersetzen der Aktionsleiste

## <a name="overview"></a>Übersicht

Eine der gängigsten Verwendungsmöglichkeiten von `Toolbar` besteht darin, die Standard Aktionsleiste durch ein benutzerdefiniertes `Toolbar` zu ersetzen (wenn ein neues Android-Projekt erstellt wird, wird die Standard Aktionsleiste verwendet). Da die Möglichkeit bietet, der APP-Leiste auf der Benutzeroberfläche einer Aktivität Marken Logos, Titel, Menü Elemente, Navigations Schaltflächen und sogar benutzerdefinierte Ansichten hinzuzufügen, bietet Sie ein bedeutendes Upgrade auf der Standard Aktionsleiste. `Toolbar`

So ersetzen Sie die Standard Aktionsleiste einer APP durch `Toolbar`: 

1.  Erstellen Sie ein neues benutzerdefiniertes Design, und ändern Sie die Eigenschaften der APP, sodass dieses neue Design verwendet wird. 

2.  Deaktivieren Sie `windowActionBar` das-Attribut im benutzerdefinierten Design, `windowNoTitle` und aktivieren Sie das-Attribut.

3.  Definieren Sie ein Layout für `Toolbar`das.

4.  Fügen Sie `Toolbar` das Layout in die **Main. axml** -Layoutdatei der Aktivität ein. 

5.  Fügen Sie der-Methode der `OnCreate` -Aktivität Code hinzu `Toolbar` , um `SetActionBar` den zu suchen `ToolBar` , und den-Befehl zum Installieren von als Aktionsleiste.

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

Dieser XML-Code definiert ein neues benutzerdefiniertes Design namens **mytheme** , das auf dem Design **. Material. Light. darkaktionbar** -Design in Lollipop basiert. Das `windowNoTitle` -Attribut ist auf `true` festgelegt, um die Titelleiste auszublenden: 

```xml
<item name="android:windowNoTitle">true</item>
```

Der Standard `ActionBar` Wert muss deaktiviert sein, damit die benutzerdefinierte Symbolleiste angezeigt werden kann: 

```xml
<item name="android:windowActionBar">false</item>
```

Für die Hintergrundfarbe `colorPrimary` der Symbolleiste wird eine olivgrüne Einstellung verwendet: 
 
```xml
<item name="android:colorPrimary">#5A8622</item>
```

## <a name="apply-the-custom-theme"></a>Anwenden des benutzerdefinierten Designs

Bearbeiten Sie " **Properties"/"androidmanifest. XML** ", und `<application>` fügen Sie das folgende `android:theme` Attribut zum- `MyTheme` Element hinzu, damit die APP das benutzerdefinierte Design verwendet: 

```xml
<application android:label="@string/app_name" android:theme="@style/MyTheme"></application>
```

Weitere Informationen zum Anwenden eines benutzerdefinierten Designs auf eine App finden [Sie unter Verwenden von benutzerdefinierten](~/android/user-interface/material-theme.md#customtheme)Designs. 



## <a name="define-a-toolbar-layout"></a>Definieren eines Toolbar-Layouts

Erstellen Sie im **Ressourcen-** /Layout-Verzeichnis eine neue Datei mit dem Namen **Toolbar. XML**. Ersetzen Sie den Inhalt durch den folgenden XML-Code: 

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

Dieser XML-Code definiert `Toolbar` das benutzerdefinierte, das die Standard Aktionsleiste ersetzt. Die Mindesthöhe des `Toolbar` wird auf die Größe der Aktionsleiste festgelegt, die ersetzt wird: 

```csharp
android:minHeight="?android:attr/actionBarSize"
```

Die Hintergrundfarbe des `Toolbar` wird auf die zuvor in **Styles. XML**definierte olivgrüne Farbe festgelegt:

```csharp
android:background="?android:attr/colorPrimary"
```

Beginnend mit Lollipop kann das `android:theme` -Attribut verwendet werden, um eine einzelne Ansicht zu formatieren. Das `ThemeOverlay.Material` in Lollipop eingeführte Design ermöglicht das Überlagern der Standard `Theme.Material` Designs, das Überschreiben relevanter Attribute, damit Sie entweder hell oder dunkel werden. In diesem Beispiel verwendet das `Toolbar` Design "dunkel", sodass der Inhalt in der Farbe hell ist: 

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

Dieses Layout schließt den `Toolbar` in " **Toolbar. XML** " definierten ein `RelativeLayout` und verwendet einen, `Toolbar` um anzugeben, dass der oben auf der Benutzeroberfläche (oberhalb der Schaltfläche) platziert werden soll. 



## <a name="find-and-activate-the-toolbar"></a>Suchen und Aktivieren der Symbolleiste

Bearbeiten Sie **MainActivity.cs** , und fügen Sie die folgende using-Anweisung hinzu:

```csharp
using Android.Views;
```

Fügen Sie auch die folgenden Codezeilen am Ende der `OnCreate` -Methode hinzu:

```csharp
var toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
SetActionBar(toolbar);
ActionBar.Title = "My Toolbar";
```

Dieser Code sucht den `Toolbar` -Aufruf `SetActionBar` und den- `Toolbar` Aufruf, sodass der standardmäßige Aktionsleisten Merkmale annimmt. Der Titel der Symbolleiste wird in **Meine Symbolleiste**geändert. Wie in diesem Codebeispiel gezeigt, `ToolBar` kann direkt als Aktionsleiste referenziert werden. Kompilieren und ausführen Sie diese &ndash; app. `Toolbar` das angepasste wird anstelle der Standard Aktionsleiste angezeigt: 

[![Screenshot der angepassten Symbolleiste mit einem grünen Farbschema](replacing-the-action-bar-images/02-after-sml.png)](replacing-the-action-bar-images/02-after.png#lightbox)

Beachten Sie, `Toolbar` dass der unabhängig von dem `Theme.Material.Light.DarkActionBar` Design formatiert ist, das auf den Rest der APP angewendet wird. 

Wenn beim Ausführen der App eine Ausnahme auftritt, lesen Sie den Abschnitt zur [Problem](#troubleshooting) Behandlung weiter unten.

 
## <a name="add-menu-items"></a>Menü Elemente hinzufügen 

In diesem Abschnitt werden die `Toolbar`Menüs hinzugefügt. Der obere rechte Bereich des `ToolBar` ist für Menü Elemente &ndash; reserviert. jedes Menü Element (auch als *Aktions Element*bezeichnet) kann eine Aktion innerhalb der aktuellen Aktivität ausführen oder eine Aktion im Namen der gesamten app ausführen. 

So fügen Sie Menüs `Toolbar`hinzu: 

1.  Fügen Sie den `mipmap-` Ordnern des App-Projekts ggf. Menü Symbole hinzu. Google stellt auf der Seite [Material Symbole](https://design.google.com/icons/) einen Satz von freien Menü Symbolen bereit. 

2.  Definieren Sie den Inhalt der Menü Elemente, indem Sie im **Menü Ressourcen/Menü**eine neue Menü Ressourcen Datei hinzufügen. 

3.  Implementieren Sie `OnCreateOptionsMenu` die-Methode der &ndash; -Aktivität, mit der diese Methode die Menü Elemente auffüllt. 

4.  Implementieren Sie `OnOptionsItemSelected` die-Methode der &ndash; -Aktivität diese Methode führt eine Aktion aus, wenn ein Menü Element getippt wird. 

In den folgenden Abschnitten wird dieser Prozess ausführlich veranschaulicht, indem die Menü Elemente **Bearbeiten** und **Speichern** der `Toolbar`angepassten hinzugefügt werden. 



### <a name="install-menu-icons"></a>Menü Symbole installieren

Wenn Sie mit `ToolbarFun` der Beispiel-App fortfahren, fügen Sie dem App-Projekt Menü Symbole hinzu. Laden [Sie Symbolleisten Symbole](https://github.com/xamarin/monodroid-samples/blob/master/Supportv7/AppCompat/Toolbar/Resources/toolbar-icons-plus.zip?raw=true)herunter, entzippen Sie Sie, und kopieren Sie den Inhalt der extrahierten *MipMap-* Ordner in das Projekt *MipMap-* Folders unter **toolbarfun/Resources** , und schließen Sie jede hinzugefügte Symbol Datei in das Projekt ein.


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

-   Ein **Bearbeitungs** Menü Element, das das `ic_action_content_create.png` Symbol (einen Stift) verwendet. 

-   Ein Menü Element **Speichern** , das das `ic_action_content_save.png` Symbol (eine Diskette) verwendet. 

-   Ein Menü Element " **Einstellungen** ", das kein Symbol enthält.

Die `showAsAction` Attribute der Menü Elemente **Bearbeiten** und **Speichern** werden auf diese Einstellung `ifRoom` festgelegt, &ndash; sodass diese Menü Elemente in der `Toolbar` angezeigt werden, wenn ausreichend Platz vorhanden ist, um angezeigt zu werden. Das Menü Element "Einstellungen `showAsAction` " `never` legt fest &ndash; , dass das Menü " **Einstellungen** " im *Überlauf* Menü angezeigt wird (drei vertikale Punkte). 


### <a name="implement-oncreateoptionsmenu"></a>Onkreateoptionsmenu implementieren

Fügen Sie **MainActivity.cs**die folgende Methode hinzu:

```csharp
public override bool OnCreateOptionsMenu(IMenu menu)
{
    MenuInflater.Inflate(Resource.Menu.top_menus, menu);
    return base.OnCreateOptionsMenu(menu);
}
```

Android Ruft die `OnCreateOptionsMenu` -Methode auf, sodass die APP die Menü Ressource für eine Aktivität angeben kann. In dieser Methode wird die **top_menus. XML** -Ressource in den bestandenen `menu`übertragen. Dieser Code bewirkt, dass die neuen Menü Elemente **Bearbeiten**, **Speichern**und **Einstellungen** in der `Toolbar`angezeigt werden. 



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

Wenn ein Benutzer auf ein Menü Element tippt, ruft Android `OnOptionsItemSelected` die-Methode auf und übergibt das Menü Element, das ausgewählt wurde. In diesem Beispiel zeigt die Implementierung nur einen Toast an, um anzugeben, welches Menü Element getippt wurde. 

Erstellen und Ausführen `ToolbarFun` , um die neuen Menü Elemente in der Symbolleiste anzuzeigen. Das `Toolbar` zeigt nun drei Menü Symbole an, wie in diesem Screenshot gezeigt: 

[![Diagramm zur Veranschaulichung der Positionen der Menü Elemente bearbeiten, speichern und Überlauf](replacing-the-action-bar-images/04-menu-items-sml.png)](replacing-the-action-bar-images/04-menu-items.png#lightbox)

Wenn ein Benutzer auf das Menü Element **Bearbeiten** tippt, wird ein Toast angezeigt, der anzeigt `OnOptionsItemSelected` , dass die-Methode aufgerufen wurde: 

[![Screenshot von Popup, der angezeigt wird, wenn Element bearbeiten angetippt wird](replacing-the-action-bar-images/05-toast-displayed-sml.png)](replacing-the-action-bar-images/05-toast-displayed.png#lightbox)

Wenn ein Benutzer auf das Überlauf Menü tippt, wird das Menü Element " **Einstellungen** " angezeigt. In der Regel sollten weniger häufige &ndash; Aktionen im Überlauf Menü abgelegt werden. in diesem Beispiel wird das Überlauf Menü für die **Einstellungen** verwendet, da es nicht so oft wie " **Bearbeiten** und **Speichern**" verwendet wird: 

[![Screenshot des Menü Elements "Einstellungen", das im Menü "Überlauf" angezeigt wird](replacing-the-action-bar-images/06-preferences-sml.png)](replacing-the-action-bar-images/06-preferences.png#lightbox)

Weitere Informationen zu Android-Menüs finden Sie im Thema Android Developer- [Menüs](https://developer.android.com/guide/topics/ui/menus.html) . 
 

## <a name="troubleshooting"></a>Problembehandlung

Die folgenden Tipps können helfen, Probleme zu beheben, die auftreten können, wenn Sie die Aktionsleiste durch eine Symbolleiste ersetzen.

### <a name="activity-already-has-an-action-bar"></a>Die Aktivität verfügt bereits über eine Aktionsleiste

Wenn die APP nicht ordnungsgemäß für die Verwendung eines benutzerdefinierten Designs konfiguriert ist, wie in [Anwenden des benutzerdefinierten](#apply-the-custom-theme)Designs erläutert, kann beim Ausführen der APP die folgende Ausnahme auftreten:

![Fehler, der auftreten kann, wenn ein benutzerdefiniertes Design nicht verwendet wird](replacing-the-action-bar-images/03-theme-not-defined.png)

Außerdem kann eine Fehlermeldung wie die folgende generiert werden: _Java. lang. IllegalStateException: Diese Aktivität hat bereits eine Aktionsleiste, die vom Fenster Dekor bereitgestellt wird._ 

Um diesen Fehler zu beheben, stellen Sie `android:theme` sicher, dass das-Attribut für das `<application>` benutzerdefinierte Design zu (in " **Properties/androidmanifest. XML**") hinzugefügt wird, wie zuvor in [Anwenden des benutzerdefinierten](#apply-the-custom-theme)Designs beschrieben. Außerdem kann dieser Fehler dadurch verursacht werden, dass das `Toolbar` Layout oder das benutzerdefinierte Design nicht ordnungsgemäß konfiguriert ist.


## <a name="related-links"></a>Verwandte Links

- [Lollipop-Symbolleiste (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-toolbar)
- [AppCompat-Symbolleiste (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar)
