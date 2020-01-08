---
title: Aktionsleiste für xamarin. Android
ms.prod: xamarin
ms.assetid: 84A79F1F-9E73-4E3E-80FA-B72E5686900B
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: f64b57e73b69b3111087ca1352f5fb9536f855e5
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75488958"
---
# <a name="actionbar-for-xamarinandroid"></a>Aktionsleiste für xamarin. Android

Wenn Sie `TabActivity`verwenden, hat der Code zum Erstellen der Registerkarten Symbole keine Auswirkung, wenn Sie für das Android 4,0-Framework ausgeführt werden. Obwohl es funktionell funktioniert, wie es in Android-Versionen vor 2,3 der Fall war, wurde die `TabActivity` Klasse selbst in 4,0 als veraltet eingestuft. Es wurde eine neue Methode zum Erstellen einer Schnittstelle mit Registerkarten eingeführt, die die Aktionsleiste verwendet, die wir als nächstes erörtern werden.

## <a name="action-bar-tabs"></a>Registerkarten Aktionsleiste

Der Aktionsleiste unterstützt das Hinzufügen von Schnittstellen im Registerkarten Format in Android 4,0.
Der folgende Screenshot zeigt ein Beispiel für eine solche Schnittstelle.

[![Screenshot der APP, die in einem Emulator ausgeführt wird. Es werden zwei Registerkarten angezeigt.](action-bar-images/25-actionbartabs.png)](action-bar-images/25-actionbartabs.png#lightbox)

Um Registerkarten im Aktionsleiste zu erstellen, müssen Sie zuerst die `NavigationMode`-Eigenschaft für die Unterstützung von Registerkarten festlegen. In Android 4 ist eine `ActionBar`-Eigenschaft für die Activity-Klasse verfügbar, die wir verwenden können, um die `NavigationMode` wie folgt festzulegen:

```csharp
this.ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
```

Sobald dies erfolgt ist, können Sie eine Registerkarte erstellen, indem Sie die `NewTab`-Methode für die Aktionsleiste aufrufen. Mit dieser Registerkarten Instanz können wir die Methoden "`SetText`" und "`SetIcon`" zum Festlegen des Beschriftungs Texts und Symbols der Registerkarte aufzurufen. Diese Aufrufe werden in der Reihenfolge in dem unten gezeigten Code vorgenommen:

```csharp
var tab = this.ActionBar.NewTab ();
tab.SetText (tabText);
tab.SetIcon (Resource.Drawable.ic_tab_white);
```

Bevor wir die Registerkarte hinzufügen können, müssen wir das `TabSelected`-Ereignis behandeln. In diesem Handler können wir den Inhalt für die Registerkarte erstellen. Aktionsleiste Registerkarten sind für die Arbeit mit *Fragmenten*konzipiert, bei denen es sich um Klassen handelt, die einen Teil der Benutzeroberfläche in einer Aktivität darstellen. In diesem Beispiel enthält die fragmentansicht einen einzelnen `TextView`, den wir in der `Fragment`-Unterklasse wie folgt erhöhen:

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

Das im `TabSelected` Ereignis weiter gegebene Ereignis Argument hat den Typ `TabEventArgs`, das eine `FragmentTransaction`-Eigenschaft enthält, mit der wir das Fragment hinzufügen können, wie unten dargestellt:

```csharp
tab.TabSelected += delegate(object sender, ActionBar.TabEventArgs e) {             
    e.FragmentTransaction.Add (Resource.Id.fragmentContainer,
        new SampleTabFragment ());
};
```

Zum Schluss können Sie die Registerkarte dem Aktionsleiste hinzufügen, indem Sie die `AddTab`-Methode aufrufen, wie im folgenden Code gezeigt:

```csharp
this.ActionBar.AddTab (tab);
```

Das komplette Beispiel finden Sie im Projekt *hellotabsics* im Beispielcode für dieses Dokument.

## <a name="shareactionprovider"></a>ShareActionProvider

Die `ShareActionProvider`-Klasse ermöglicht das Ausführen einer Freigabe Aktion aus einer Aktionsleiste. Es übernimmt das Erstellen einer Aktions Ansicht mit einer Liste von apps, die eine Freigabe Absicht verarbeiten können, und speichert einen Verlauf der zuvor verwendeten Anwendungen für den einfachen Zugriff auf diese Anwendungen später aus dem Aktionsleiste. Dies ermöglicht Anwendungen die gemeinsame Nutzung von Daten über eine Benutzer Darstellung, die in Android konsistent ist.

### <a name="image-sharing-example"></a>Beispiel für eine Bildfreigabe

Nachstehend sehen Sie beispielsweise einen Screenshot einer Aktionsleiste mit einem Menü Element für die Freigabe eines Bilds (aus dem [shareaktionprovider](https://docs.microsoft.com/samples/xamarin/monodroid-samples/shareactionproviderdemo) -Beispiel entnommen). Wenn der Benutzer auf das Menü Element auf der Aktionsleiste tippt, lädt der shareaktionsanbieter die Anwendung, um eine Absicht zu verarbeiten, die der `ShareActionProvider`zugeordnet ist. In diesem Beispiel wurde die Messaging Anwendung bereits verwendet, sodass Sie in der Aktionsleiste angezeigt wird.

[![Screenshot des Messaging Anwendungs Symbols in der Aktionsleiste](action-bar-images/09-shareactionprovider.png)](action-bar-images/09-shareactionprovider.png#lightbox)

Wenn der Benutzer im Aktionsleiste auf das Element klickt, wird die Messaging-APP, die das freigegebene Image enthält, wie unten dargestellt gestartet:

[![Screenshot der Messaging-APP, die das affbild anzeigt](action-bar-images/10-messagewithimage.png)](action-bar-images/10-messagewithimage.png#lightbox)

### <a name="specifying-the-action-provider-class"></a>Angeben der Aktions Anbieter Klasse

Um die `ShareActionProvider`zu verwenden, legen Sie das `android:actionProviderClass`-Attribut in einem Menü Element im XML-Format für das Aktionsleiste Menü wie folgt fest:

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:id="@+id/shareMenuItem"
      android:showAsAction="always"
      android:title="@string/sharePicture"
      android:actionProviderClass="android.widget.ShareActionProvider" />
</menu>
```

### <a name="inflating-the-menu"></a>Auffüllen des Menüs

Um das Menü zu erhöhen, überschreiben wir `OnCreateOptionsMenu` in der Aktivitäts Unterklasse. Sobald ein Verweis auf das Menü vorhanden ist, können wir die `ShareActionProvider` aus der `ActionProvider`-Eigenschaft des Menü Elements erhalten und dann mithilfe der setshareintent-Methode die Absicht des `ShareActionProvider`festlegen, wie unten dargestellt:

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

### <a name="creating-the-intent"></a>Erstellen der Absicht

Der `ShareActionProvider` verwendet die Absicht, die an die `SetShareIntent`-Methode im obigen Code weitergeleitet wird, um die entsprechende Aktivität zu starten. In diesem Fall erstellen wir eine Absicht, ein Image zu senden, indem wir den folgenden Code verwenden:

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

Das Image im obigen Codebeispiel ist als Medienobjekt mit der Anwendung enthalten und an einen öffentlich zugänglichen Speicherort kopiert, wenn die Aktivität erstellt wird, sodass andere Anwendungen, wie z. b. die Messaging-APP, darauf zugreifen können. Der Beispielcode für diesen Artikel enthält die vollständige Quelle dieses Beispiels, die seine Verwendung veranschaulicht.

## <a name="related-links"></a>Verwandte Themen

- [Hello Tabs ICS (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/hellotabsics)
- [Shareaktionprovider-Demo (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/shareactionproviderdemo)
