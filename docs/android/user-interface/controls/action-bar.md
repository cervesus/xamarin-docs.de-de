---
title: ActionBar
ms.prod: xamarin
ms.assetid: 84A79F1F-9E73-4E3E-80FA-B72E5686900B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 9d9bef6d1a0817abc12b5a9bd266b1e1e7d38348
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57667543"
---
# <a name="actionbar"></a>ActionBar


## <a name="overview"></a>Übersicht

Bei Verwendung `TabActivity`, der Code zum Erstellen von der Registerkarte Symbole hat keine Auswirkungen, die Ausführung für das Android 4.0-Framework. Zwar funktional können sie wie zuvor im Android-Versionen vor 2.3, funktioniert die `TabActivity` Klasse selbst ist in 4.0 veraltet. Eine neue Methode zum Erstellen einer Schnittstelle im Registerkartenformat wurde eingeführt, die der Aktionsleiste verwendet, die wir als Nächstes eingehen werde.


## <a name="action-bar-tabs"></a>Registerkarten für Aktionsleiste

Der Aktionsleiste umfasst Unterstützung für das Hinzufügen von Registerkarten Schnittstellen in Android 4.0.
Der folgende Screenshot zeigt ein Beispiel für eine solche Schnittstelle.

[![Screenshot der app, die in einem Emulator ausgeführt wird; Es werden zwei Registerkarten angezeigt.](action-bar-images/25-actionbartabs.png)](action-bar-images/25-actionbartabs.png#lightbox)

Um die Registerkarten in der Aktionsleiste erstellen zu können, müssen wir zuerst festlegen seiner `NavigationMode` -Eigenschaft für die Unterstützung von Registerkarten. In Android 4 ein `ActionBar` Eigenschaft ist verfügbar in der Activity-Klasse, die wir verwenden können, legen Sie die `NavigationMode` wie folgt aus:

```csharp
this.ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
```

Sobald dies geschehen ist, können wir eine Registerkarte erstellen, durch den Aufruf der `NewTab` Methode in der Aktionsleiste. Mit dieser Registerkarte-Instanz, können wir Aufrufen der `SetText` und `SetIcon` Methoden zum Festlegen der Registerkarte Bezeichnungstext und Symbol; diese Aufrufe in der Reihenfolge, in den folgenden Code:

```csharp
var tab = this.ActionBar.NewTab ();
tab.SetText (tabText);
tab.SetIcon (Resource.Drawable.ic_tab_white);
```

Bevor wir jedoch die Registerkarte hinzufügen können, müssen wir behandeln die `TabSelected` Ereignis. In diesem Handler können wir den Inhalt der Registerkarte "erstellen. Aktionsleiste Registerkarten dienen zum Arbeiten mit *Fragmente*, welche sind Klassen, die einen Teil der Benutzeroberfläche in einer Aktivität darstellen. In diesem Beispiel enthält das Fragment der Ansicht eine einzelne `TextView`, die wir in Vergrößerung unsere `Fragment` Unterklasse wie folgt:

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

Das Ereignisargument übergeben wird, der `TabSelected` Ereignis ist vom Typ `TabEventArgs`, wozu ein `FragmentTransaction` -Eigenschaft, die wir verwenden können, um das Fragment hinzuzufügen, wie unten dargestellt:

```csharp
tab.TabSelected += delegate(object sender, ActionBar.TabEventArgs e) {             
    e.FragmentTransaction.Add (Resource.Id.fragmentContainer,
        new SampleTabFragment ());
};
```

Schließlich können wir die Registerkarte auch auf der Aktionsleiste hinzufügen, durch den Aufruf der `AddTab` -Methode wie im folgenden Code gezeigt:

```csharp
this.ActionBar.AddTab (tab);
```

Das vollständige Beispiel finden Sie unter den *HelloTabsICS* Projekt im Beispielcode für dieses Dokument.


## <a name="shareactionprovider"></a>ShareActionProvider

Die `ShareActionProvider` Klasse ermöglicht, eine Freigabe Aktion auf eine Aktionsleiste erfolgen. Sie übernimmt, erstellen Sie eine Ansicht der Aktion mit einer Liste von apps, die eine Freigabe Absicht verarbeiten kann und einen Verlauf der zuvor verwendeten Anwendungen für den leichteren Zugriff auf diese später in der Aktionsleiste verfolgt. Dadurch können Anwendungen zum Freigeben von Daten über eine Benutzeroberfläche, die während des gesamten Android konsistent ist.


### <a name="image-sharing-example"></a>Image-Freigabe-Beispiel

Unten ist beispielsweise ein Screenshot, der eine Aktionsleiste zu einem Menüeintrag zum Freigeben einer Abbilddatei (stammt aus der [ShareActionProvider](https://developer.xamarin.com/samples/monodroid/ShareActionProviderDemo/) Beispiel). Wenn der Benutzer das Menüelement in der Aktionsleiste tippt, lädt die ShareActionProvider die Anwendung, ein Intent-Objekt zu behandeln, die zugeordnet wird die `ShareActionProvider`. In diesem Beispiel wurde die messaging-Anwendung zuvor verwendet, damit sie in der Aktionsleiste angezeigt werden.

[![Screenshot der messaging-Symbol in der Aktionsleiste](action-bar-images/09-shareactionprovider.png)](action-bar-images/09-shareactionprovider.png#lightbox)


Klickt der Benutzer auf das Element in der Aktionsleiste auf, wird die messaging-app mit dem freigegebenen Image gestartet, wie unten dargestellt:

[![Screenshot der messaging-app anzeigen Monkey-Bilds](action-bar-images/10-messagewithimage.png)](action-bar-images/10-messagewithimage.png#lightbox)


### <a name="specifying-the-action-provider-class"></a>Angeben der Aktion Provider-Klasse

Verwenden der `ShareActionProvider`legen die `android:actionProviderClass` -Attribut auf ein Menüelement in der XML-Code für die Aktionsleiste Menü wie folgt:

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:id="@+id/shareMenuItem"
      android:showAsAction="always"
      android:title="@string/sharePicture"
      android:actionProviderClass="android.widget.ShareActionProvider" />
</menu>
```


### <a name="inflating-the-menu"></a>Klicken Sie im Menü jedoch

Um das Menü vergrößert werden soll, überschreiben `OnCreateOptionsMenu` in der Aktivität-Unterklasse. Nachdem wir einen Verweis auf das Menü "haben, erhalten wir die `ShareActionProvider` aus der `ActionProvider` Eigenschaft des Menüelements, und klicken Sie dann die SetShareIntent-Methode zum Festlegen der `ShareActionProvider`die Absicht, wie unten dargestellt:

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


### <a name="creating-the-intent"></a>Erstellen die Absicht

Die `ShareActionProvider` verwendet die Absicht, die an die `SetShareIntent` -Methode in der obigen Code, um die passende Aktivität zu starten. In diesem Fall erstellen wir ein Intent-Objekt um ein Bild zu senden, mit dem folgenden Code:

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

Das Bild im obigen Codebeispiel als Ressource mit der Anwendung enthalten ist, und kopiert an einen öffentlich zugänglichen Speicherort, wenn die Aktivität erstellt wird, damit er für andere Anwendungen, z. B. die messaging-app zugegriffen werden kann. Der Beispielcode, der diesen Artikel begleitet, enthält den vollständigen Quellcode dieses Beispiel veranschaulicht die Verwendung.



## <a name="related-links"></a>Verwandte Links

- [Hallo Registerkarten ICS (Beispiel)](https://developer.xamarin.com/samples/HelloTabsICS/)
- [ShareActionProvider-Demo (Beispiel)](https://developer.xamarin.com/samples/monodroid/ShareActionProviderDemo/)
- [Einführung in die Ice Cream Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 Platform](https://developer.android.com/sdk/android-4.0.html)
