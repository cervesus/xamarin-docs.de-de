---
title: ActionBar
ms.prod: xamarin
ms.assetid: 84A79F1F-9E73-4E3E-80FA-B72E5686900B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 29726450e0899177f3223deeade1cb4766d933a5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="actionbar"></a>ActionBar


## <a name="overview"></a>Übersicht

Bei Verwendung `TabActivity`, der Code zum Erstellen der Registerkarte "-Symbole hat keine Auswirkungen, die Ausführung von für das Android 4.0-Framework. Obwohl funktional es funktioniert, wie in den Vorgängerversionen von Android 2.3 die `TabActivity` Klasse selbst ist in 4.0 veraltet. Eine neue Methode zum Erstellen einer Schnittstelle im Registerkartenformat wurde eingeführt, die der Aktionsleiste verwendet wird, das als Nächstes erläutert.


## <a name="action-bar-tabs"></a>Aktionsleiste-Registerkarten

Der Aktionsleiste unterstützt hinzufügen im Registerkartenformat Schnittstellen in Android 4.0.
Der folgende Screenshot zeigt ein Beispiel für eine solche Schnittstelle.

[![Screenshot der app, die in einem Emulator; Es werden zwei Registerkarten angezeigt.](action-bar-images/25-actionbartabs.png)](action-bar-images/25-actionbartabs.png#lightbox)

Um Registerkarten in der Aktionsleiste zu erstellen, müssen Sie zuerst festlegen seiner `NavigationMode` Eigenschaft Registerkarten zu unterstützen. Android 4 ein `ActionBar` Eigenschaft steht auf der Activity-Klasse, die wir verwenden können, legen Sie die `NavigationMode` wie folgt:

```csharp
this.ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
```

Sobald dies geschehen ist, können wir eine Registerkarte erstellen, durch Aufrufen der `NewTab` Methode in der Aktionsleiste. Mit dieser Registerkarte Instanz rufen wir die `SetText` und `SetIcon` Methoden, um die Registerkarte Bezeichnungstext und Symbol; festgelegt diese Aufrufe in der Reihenfolge, in den folgenden Code:

```csharp
var tab = this.ActionBar.NewTab ();
tab.SetText (tabText);
tab.SetIcon (Resource.Drawable.ic_tab_white);
```

Bevor wir jedoch die Registerkarte hinzufügen können, müssen wir behandeln das `TabSelected` Ereignis. In diesem Handler können wir den Inhalt für die Registerkarte "erstellen. Aktionsleiste Registerkarten sind dienen zum Arbeiten mit *Fragmente*, wobei es sich um Klassen, die einen Teil der Benutzeroberfläche in einer Aktivität darstellen. In diesem Beispiel enthält das Fragment-Sicht eine einzelnen `TextView`, die wir in vergrößern unserer `Fragment` Unterklasse wie folgt:

```csharp
class SampleTabFragment: Fragment
{           
    public override View OnCreateView (LayoutInflater inflater,
        ViewGroup container, Bundle savedInstanceState)
    {
        base.OnCreateView (inflater, container, savedInstanceState);
       
        var view = inflater.Inflate (
            Resource.Layout.Tab, container, false);

        var sampleTextView =
            view.FindViewById<TextView> (Resource.Id.sampleTextView);            
        sampleTextView.Text = "sample fragment text";


        return view;
    }
}
```

Das Ereignisargument übergeben wird, der `TabSelected` Ereignis ist vom Typ `TabEventArgs`, darunter eine `FragmentTransaction` -Eigenschaft, die es verwenden können, um das Fragment hinzuzufügen, wie unten dargestellt:

```csharp
tab.TabSelected += delegate(object sender, ActionBar.TabEventArgs e) {             
    e.FragmentTransaction.Add (Resource.Id.fragmentContainer,
        new SampleTabFragment ());
};
```

Schließlich kann die Registerkarte zur Hinzufügen der Aktionsleiste durch Aufrufen der `AddTab` Methode, wie in diesem Code dargestellt:

```csharp
this.ActionBar.AddTab (tab);
```

Das vollständige Beispiel finden Sie unter der *HelloTabsICS* Projekt im Beispielcode für dieses Dokument.


## <a name="shareactionprovider"></a>ShareActionProvider

Die `ShareActionProvider` Klasse ermöglicht, einen Freigabe-Vorgang aus einem Aktionsleiste erfolgen. Es übernimmt eine Aktion Sicht mit einer Liste von apps, die eine Freigabe Absicht behandeln und protokolliert den Verlauf der zuvor verwendeten Anwendungen für den leichteren Zugriff auf diese später aus der Aktionsleiste erstellen. Dadurch können Anwendungen zum Freigeben von Daten über eine Benutzeroberfläche, die in der gesamten Android konsistent ist.


### <a name="image-sharing-example"></a>Image-Freigabe-Beispiel

Beispielsweise folgt ein Screenshot, der eine Aktion Leiste mit einem Menüelement zum Freigeben einer Abbilddatei (entnommen der [ShareActionProvider](https://developer.xamarin.com/samples/monodroid/ShareActionProviderDemo/) Beispiel). Wenn der Benutzer das Menüelement in der Aktionsleiste tippt, die ShareActionProvider lädt die Anwendung die Priorität zu behandeln, die mit zugeordnetem der `ShareActionProvider`. In diesem Beispiel hat die Messaginganwendung zuvor verwendet wurde, damit sie auf der Aktionsleiste angezeigt wird.

[![Screenshot der Anwendungssymbol in der Aktionsleiste messaging](action-bar-images/09-shareactionprovider.png)](action-bar-images/09-shareactionprovider.png#lightbox)


Klickt der Benutzer auf das Element in der Aktionsleiste auf, wird die messaging-app mit dem freigegebene Bild gestartet, wie unten dargestellt:

[![Screenshot der messaging-app anzeigen Affe Bilds](action-bar-images/10-messagewithimage.png)](action-bar-images/10-messagewithimage.png#lightbox)


### <a name="specifying-the-action-provider-class"></a>Angeben der Aktion Anbieterklasse

Verwenden der `ShareActionProvider`legen die `android:actionProviderClass` -Attribut auf ein Menüelement in der XML for der Aktionsleiste Menü wie folgt:

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:id="@+id/shareMenuItem"
      android:showAsAction="always"
      android:title="@string/sharePicture"
      android:actionProviderClass="android.widget.ShareActionProvider" />
</menu>
```


### <a name="inflating-the-menu"></a>Klicken Sie im Menü überhöhte

Um das Menü vergrößert werden soll, überschreiben `OnCreateOptionsMenu` in der Aktivität-Unterklasse. Nachdem wir einen Verweis auf das Menü haben, erhalten wir die `ShareActionProvider` aus der `ActionProvider` Eigenschaft des Menüelements, und klicken Sie dann verwenden Sie die SetShareIntent-Methode festgelegt die `ShareActionProvider`des beabsichtigte Lesevorgänge, wie unten dargestellt:

```csharp
public override bool OnCreateOptionsMenu (IMenu menu)
{
    MenuInflater.Inflate (Resource.Menu.ActionBarMenu, menu);       
           
    var shareMenuItem = menu.FindItem (Resource.Id.shareMenuItem);           
    var shareActionProvider =
       (ShareActionProvider)shareMenuItem.ActionProvider;
    shareActionProvider.SetShareIntent (CreateIntent ());
}
```


### <a name="creating-the-intent"></a>Die Absicht erstellen

Die `ShareActionProvider` verwendet die Absicht, das an die `SetShareIntent` Methode im obigen Code, um die passende Aktivität zu starten. In diesem Fall erstellen wir eine versucht, ein Bild zu senden, indem Sie mit dem folgenden Code:

```csharp
Intent CreateIntent ()
{  
    var sendPictureIntent = new Intent (Intent.ActionSend);
    sendPictureIntent.SetType ("image/*");
    var uri = Android.Net.Uri.FromFile (GetFileStreamPath ("monkey.png"));          
    sendPictureIntent.PutExtra (Intent.ExtraStream, uri);
    return sendPictureIntent;
}
```

Das Bild im obigen Beispiel ist als ein Medienobjekt mit der Anwendung enthalten und kopiert an einem öffentlich zugänglichen Speicherort, wenn die Aktivität erstellt wird, damit er für andere Anwendungen, z. B. die messaging-app zugegriffen werden kann. Der Beispielcode in diesem Artikel enthält den vollständigen Quellcode dieses Beispiel zur Veranschaulichung Verwendungsmöglichkeiten.



## <a name="related-links"></a>Verwandte Links

- [Hello Registerkarten ICS (Beispiel)](https://developer.xamarin.com/samples/HelloTabsICS/)
- [ShareActionProvider Demo (Beispiel)](https://developer.xamarin.com/samples/monodroid/ShareActionProviderDemo/)
- [Einführung in Eis Rustikal Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 Platform](http://developer.android.com/sdk/android-4.0.html)
