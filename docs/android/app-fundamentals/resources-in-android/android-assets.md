---
title: Verwenden von Android-Ressourcen
ms.prod: xamarin
ms.assetid: 70ECDDC9-FA40-03B4-BF04-E7CFFFE4260D
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/13/2018
ms.openlocfilehash: 9c8db5ad7bcb012befb2fa8dcd1ecd13fa355a55
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73025431"
---
# <a name="using-android-assets"></a>Verwenden von Android-Ressourcen

_Assets_ bieten eine Möglichkeit, beliebige Dateien wie Text, XML, Schriftarten, Musik und Videos in Ihre Anwendung einzubeziehen. Wenn Sie versuchen, diese Dateien als "Ressourcen" einzuschließen, verarbeitet Android Sie in das Ressourcensystem, und Sie können die Rohdaten nicht mehr erhalten. Wenn Sie auf Daten nicht auf Daten zugreifen möchten, sind Assets eine Möglichkeit dafür.

Dem Projekt hinzugefügte Ressourcen werden genau wie ein Dateisystem angezeigt, das von Ihrer Anwendung mithilfe von [Assetmanager](xref:Android.Content.Res.AssetManager)gelesen werden kann.
In dieser einfachen Demo fügen wir dem Projekt eine Textdatei hinzu, lesen Sie mit `AssetManager`und zeigen Sie in einer TextView an.

## <a name="add-asset-to-project"></a>Medienobjekt zu Projekt hinzufügen

Assets gelangen in den Ordner `Assets` des Projekts. Fügen Sie diesem Ordner eine neue Textdatei mit dem Namen `read_asset.txt`hinzu. Platzieren Sie Text wie "Ich habe von einem Asset!".

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio muss die **Buildaktion** für diese Datei auf **androidasset**festlegen:

![Festlegen der Buildaktion auf "androidasset"](android-assets-images/asset-properties-vs.png) 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Visual Studio für Mac müssen die **Buildaktion** für diese Datei auf " **androidasset**" festlegen:

[![Festlegen der Buildaktion auf "androidasset"](android-assets-images/asset-properties-xs-sml.png)](android-assets-images/asset-properties-xs.png#lightbox)

-----

Wenn Sie die richtige **BuildAction** auswählen, wird sichergestellt, dass die Datei zur Kompilierzeit in das APK gepackt wird.

## <a name="reading-assets"></a>Lesen von Assets

Assets werden mithilfe eines [assetmanagers](xref:Android.Content.Res.AssetManager)gelesen. Eine Instanz des `AssetManager` ist verfügbar, wenn Sie auf die [Assets](xref:Android.Content.Context.Assets) -Eigenschaft eines `Android.Content.Context`zugreifen, z. b. eine-Aktivität.
Im folgenden Code öffnen wir das **read_asset. txt** -Asset, lesen den Inhalt und zeigen ihn mithilfe einer TextView an.

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

Führen Sie die Anwendung aus, und Folgendes sollte angezeigt werden:

![Beispiel Bildschirm](android-assets-images/screenshot.png)

## <a name="related-links"></a>Verwandte Links

- [Assetmanager](xref:Android.Content.Res.AssetManager)
- [Kontext](xref:Android.Content.Context)
