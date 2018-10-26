---
title: Mit dem Android-Geräte
ms.prod: xamarin
ms.assetid: 70ECDDC9-FA40-03B4-BF04-E7CFFFE4260D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/13/2018
ms.openlocfilehash: 1e9a71de7725c8382e133d85977407bcc859fc58
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114859"
---
# <a name="using-android-assets"></a>Mit dem Android-Geräte

_Assets_ bieten eine Möglichkeit, beliebige Dateien, wie Text, Xml, Schriftarten, Musik und Videos in Ihre Anwendung einbinden. Wenn Sie versuchen, diese Dateien als "Resources" enthalten, Android werden diese in der Resource-System verarbeitet, und Sie werden nicht in die Rohdaten zu erhalten. Sie können den Zugriff auf Daten, die unverändert sind Ressourcen eine Möglichkeit dafür.

Objekte, die dem Projekt hinzugefügt werden angezeigt, wie ein Dateisystem, das von Ihrer Anwendung mithilfe von lesen kann [AssetManager](https://developer.xamarin.com/api/type/Android.Content.Res.AssetManager/).
In dieser einfachen Demo werden wir ein Text-Datei-Medienobjekt unser Projekt hinzufügen, lesen Sie sie mit `AssetManager`, und in einem TextView angezeigt wird.


## <a name="add-asset-to-project"></a>Anlage zu Projekt hinzufügen

Ressourcen befinden sich der `Assets` Ordner des Projekts. Fügen Sie eine neue Textdatei, in diesem Ordner namens `read_asset.txt`. Platzieren Sie etwas Text darin, wie "Ich aus einem Medienobjekt stammen!".

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio festgelegt haben sollte die **Buildvorgang** für diese Datei, um **AndroidAsset**:

![Die Buildaktion festlegen auf AndroidAsset](android-assets-images/asset-properties-vs.png) 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Visual Studio für Mac festgelegt haben sollte die **Buildvorgang** für diese Datei, um **AndroidAsset**:

[![Die Buildaktion festlegen auf AndroidAsset](android-assets-images/asset-properties-xs-sml.png)](android-assets-images/asset-properties-xs.png#lightbox)

-----

Wählen Sie die richtigen **BuildAction** wird sichergestellt, dass die Datei in das APK zum Zeitpunkt der Kompilierung verpackt werden.


## <a name="reading-assets"></a>Lesen von Ressourcen

Ressourcen werden gelesen, die mit einem [AssetManager](https://developer.xamarin.com/api/type/Android.Content.Res.AssetManager/). Eine Instanz von der `AssetManager` ist verfügbar, die Zugriff auf die [Assets](https://developer.xamarin.com/api/property/Android.Content.Context.Assets/) Eigenschaft für eine `Android.Content.Context`, wie z. B. eine Aktivität.
Öffnen Sie in den folgenden Code, wir unsere **read_asset.txt** Asset, lesen Sie den Inhalt und mit einem TextView anzuzeigen.

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


## <a name="running-the-application"></a>Ausführen der Anwendung

Führen Sie die Anwendung und sollte Folgendes angezeigt:

![Beispielscreenshot](android-assets-images/screenshot.png)


## <a name="related-links"></a>Verwandte Links

- [AssetManager](https://developer.xamarin.com/api/type/Android.Content.Res.AssetManager/)
- [Kontext](https://developer.xamarin.com/api/type/Android.Content.Context/)
