---
title: "Mit dem Android-Geräte"
ms.topic: article
ms.prod: xamarin
ms.assetid: 70ECDDC9-FA40-03B4-BF04-E7CFFFE4260D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/30/2017
ms.openlocfilehash: bbf20155fe097f0229aa28c1f0d046cb3ef31a63
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="using-android-assets"></a>Mit dem Android-Geräte

_Bestand_ bieten eine Möglichkeit, beliebige Dateien, wie Text, Xml, Schriftarten, Musik und Videos in die Anwendung aufnehmen. Wenn Sie versuchen, diese Dateien als "Resources" einzufügen, Android verarbeitet sie in das Ressourcensystem und nicht die unformatierten Daten abgerufen werden. Wenn Sie die Daten unverändert zugreifen möchten, sind "Assets" eine Möglichkeit dafür.

Objekte, die dem Projekt hinzugefügt werden genau wie ein Dateisystem, das in Ihrer Anwendung mit lesbar angezeigt [AssetManager](https://developer.xamarin.com/api/type/Android.Content.Res.AssetManager/).
In dieser einfachen Demo Kegel zum Medienobjekt einen Text-Dateien, hinzufügen zu lesen mit `AssetManager`, und zeigen Sie es in einem TextView.

<a name="Add_Asset_to_Project" />

## <a name="add-asset-to-project"></a>Asset-Projekt hinzufügen

Bestand wechseln Sie der `Assets` -Ordner des Projekts. Fügen Sie eine neue Textdatei in diesem Ordner namens `read_asset.txt`. Platzieren Sie etwas Text darin, wie "Ich ein Medienobjekt stammt!".

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio festgelegt haben sollten die **Buildvorgang** für diese Datei zu **AndroidAsset**:

![Wenn die Buildaktion auf AndroidAsset](android-assets-images/asset-properties-vs.png) 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Visual Studio für Mac festgelegt haben sollten die **Buildvorgang** für diese Datei zu **AndroidAsset**:

[![Wenn die Buildaktion auf AndroidAsset](android-assets-images/asset-properties-xs-sml.png)](android-assets-images/asset-properties-xs.png)

-----

Auswählen der richtigen **BuildAction** wird sichergestellt, dass die Datei in die APK zum Zeitpunkt der Kompilierung verpackt werden.

<a name="Reading_Assets" />

## <a name="reading-assets"></a>Lesen von Ressourcen

Objekte werden mit gelesen ein [AssetManager](https://developer.xamarin.com/api/type/Android.Content.Res.AssetManager/). Eine Instanz von der `AssetManager` ist verfügbar, die Zugriff der [Bestand](https://developer.xamarin.com/api/property/Android.Content.Context.Assets/) Eigenschaft auf eine `Android.Contet.Context`, wie etwa eine Aktivität.
Öffnen Sie in den folgenden Code, wir unsere **read_asset.txt** Asset, den Inhalt lesen und Anzeigen einer TextView.

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Create a new TextView and set it as our view
    TextView tv = new TextView (this);
    
    // Read the contents of our asset
    string content;
    AssetManager assets = this.Assets;
    using (StreamReader sr = new StreamReader (assets.Open ("read_asset.txt")))
    {
        content = sr.ReadToEnd ();
    }

    // Set TextView.Text to our asset content
    tv.Text = content;
    SetContentView (tv);
}
```

<a name="Running_the_Application" />

## <a name="running-the-application"></a>Ausführen der Anwendung

Führen Sie die Anwendung, und sehen Sie Folgendes:

![Beispiel-screenshot](android-assets-images/screenshot.png)


## <a name="related-links"></a>Verwandte Links

- [AssetManager](https://developer.xamarin.com/api/type/Android.Content.Res.AssetManager/)
- [Context](https://developer.xamarin.com/api/type/Android.Content.Context/)
