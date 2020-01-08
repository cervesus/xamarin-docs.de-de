---
title: Photokit in xamarin. IOS
description: In diesem Dokument werden die Informationen zu den Modell Objekten, zum Abfragen von Modelldaten und zum Speichern von Änderungen an der Fotobibliothek beschrieben.
ms.prod: xamarin
ms.assetid: 7FDEE394-3787-40FA-8372-76A05BF184B3
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/14/2017
ms.openlocfilehash: def34efd1fd48cc0e7dd802a6d3e843be1e156a4
ms.sourcegitcommit: 5ddb107b0a56bef8a16fce5bc6846f9673b3b22e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/31/2019
ms.locfileid: "75558806"
---
# <a name="photokit-in-xamarinios"></a>Photokit in xamarin. IOS

[Beispiel für ![herunterladen](~/media/shared/download.png) Codebeispiel herunterladen](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-samplephotoapp/)

Photokit ist ein Framework, das es Anwendungen ermöglicht, die System Image Bibliothek abzufragen und benutzerdefinierte Benutzeroberflächen zu erstellen, um deren Inhalte anzuzeigen und zu ändern. Sie enthält eine Reihe von Klassen, die Bild-und Video Medienobjekte sowie Sammlungen von Assets wie z. b. Alben und Ordner darstellen.

## <a name="permissions"></a>Berechtigungen

Bevor Ihre APP auf die Fotobibliothek zugreifen kann, wird dem Benutzer ein Berechtigungs Dialogfeld angezeigt. Sie müssen erklärenden Text in der Datei " **Info. plist** " bereitstellen, um zu erläutern, wie Ihre APP die Fotobibliothek verwendet, z. b.:

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>Applies filters to photos and updates the original image</string>
```

## <a name="model-objects"></a>Modell Objekte

Photokit stellt diese Assets in der Art dar, in der Modell Objekte aufgerufen werden. Die Modell Objekte, die die Fotos und Videos selbst darstellen, sind vom Typ `PHAsset`. Eine `PHAsset` die Metadaten wie den Medientyp des Assets und das Erstellungsdatum enthält.
Entsprechend enthalten die Klassen "`PHAssetCollection`" und "`PHCollectionList`" Metadaten zu Ressourcen Sammlungen und Sammlungs Listen. Ressourcen Sammlungen sind Gruppen von Assets, wie z. b. Alle Fotos und Videos für ein bestimmtes Jahr. Ebenso sind Sammlungs Listen Gruppen von Ressourcen Sammlungen, z. b. Fotos und Videos nach Jahr gruppiert.

## <a name="querying-model-data"></a>Abfragen von Modelldaten

Mit photokit können Sie Modelldaten problemlos über eine Vielzahl von Abruf Methoden Abfragen. Wenn Sie z. b. alle Images abrufen möchten, rufen Sie `PHAsset.Fetch`auf, und übergeben Sie den `PHAssetMediaType.Image` Medientyp.

```csharp
PHFetchResult fetchResults = PHAsset.FetchAssets (PHAssetMediaType.Image, null);
```

Die `PHFetchResult` Instanz enthält dann alle `PHAsset` Instanzen, die Bilder darstellen. Um die Images selbst abzurufen, verwenden Sie die `PHImageManager` (oder die cachingversion `PHCachingImageManager`), um eine Anforderung für das Image durch Aufrufen von `RequestImageForAsset`zu erstellen. Der folgende Code ruft z. b. ein Bild für jede Ressource in einem `PHFetchResult` ab, das in einer Auflistungs Ansichts Zelle angezeigt werden soll:

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    var imageCell = (ImageCell)collectionView.DequeueReusableCell (cellId, indexPath);
    imageMgr.RequestImageForAsset (
        (PHAsset)fetchResults [(uint)indexPath.Item],
        thumbnailSize,
        PHImageContentMode.AspectFill, new PHImageRequestOptions (),
        (img, info) => {
            imageCell.ImageView.Image = img;
        }
    );
    return imageCell;
}
```

Dies führt zu einem Raster mit Bildern, wie unten dargestellt:

![](photokit-images/image4.png "The running app displaying a grid of images")

## <a name="saving-changes-to-the-photo-library"></a>Speichern von Änderungen an der Fotobibliothek

So wird das Abfragen und Lesen von Daten behandelt. Sie können auch Änderungen zurück in die Bibliothek schreiben. Da mehrere interessierte Anwendungen mit der System Fotobibliothek interagieren können, können Sie einen Beobachter registrieren, um über Änderungen mithilfe eines `PhotoLibraryObserver`benachrichtigt zu werden. Wenn dann Änderungen auftreten, kann die Anwendung entsprechend aktualisiert werden. Hier ist beispielsweise eine einfache Implementierung zum erneuten Laden der oben aufgeführten Sammlungsansicht:

```csharp
class PhotoLibraryObserver : PHPhotoLibraryChangeObserver
{
    readonly PhotosViewController controller;
    public PhotoLibraryObserver (PhotosViewController controller)

    {
        this.controller = controller;
    }

    public override void PhotoLibraryDidChange (PHChange changeInstance)
    {
        DispatchQueue.MainQueue.DispatchAsync (() => {
            var changes = changeInstance.GetFetchResultChangeDetails (controller.fetchResults);
            controller.fetchResults = changes.FetchResultAfterChanges;
            controller.CollectionView.ReloadData ();
        });
    }
}
```

Wenn Sie tatsächlich Änderungen von Ihrer Anwendung Zurückschreiben möchten, erstellen Sie eine Change Request. Jede Modell Klasse verfügt über eine zugeordnete Change Request-Klasse. Wenn Sie z. b. eine `PHAsset`ändern möchten, erstellen Sie eine `PHAssetChangeRequest`. Die Schritte zum Ausführen von Änderungen, die in die Fotobibliothek zurückgeschrieben und an Beobachter wie die oben gesendet werden, lauten wie folgt:

1. Führt den Bearbeitungsvorgang aus.
2. Speichert die gefilterten Bilddaten in einer `PHContentEditingOutput`-Instanz.
3. Erstellen Sie eine Change Request, um die Änderungen aus der Bearbeitungs Ausgabe zu veröffentlichen.

Hier ist ein Beispiel, das eine Änderung an ein Bild zurückgibt, das einen Core-Bild-WF-Filter anwendet:

```csharp
void ApplyNoirFilter (object sender, EventArgs e)
{
    Asset.RequestContentEditingInput (new PHContentEditingInputRequestOptions (), (input, options) => {

        // perform the editing operation, which applies a noir filter in this case
        var image = CIImage.FromUrl (input.FullSizeImageUrl);
        image = image.CreateWithOrientation((CIImageOrientation)input.FullSizeImageOrientation);
        var noir = new CIPhotoEffectNoir {
            Image = image
        };
        var ciContext = CIContext.FromOptions (null);
        var output = noir.OutputImage;
        var uiImage = UIImage.FromImage (ciContext.CreateCGImage (output, output.Extent));
        imageView.Image = uiImage;
        //
        // save the filtered image data to a PHContentEditingOutput instance
        var editingOutput = new PHContentEditingOutput(input);
        var adjustmentData = new PHAdjustmentData();
        var data = uiImage.AsJPEG();
        NSError error;
        data.Save(editingOutput.RenderedContentUrl, false, out error);
        editingOutput.AdjustmentData = adjustmentData;
        //
        // make a change request to publish the changes form the editing output
        PHPhotoLibrary.GetSharedPhotoLibrary.PerformChanges (() => {
            PHAssetChangeRequest request = PHAssetChangeRequest.ChangeRequest(Asset);
            request.ContentEditingOutput = editingOutput;
        },
        (ok, err) => Console.WriteLine ("photo updated successfully: {0}", ok));
    });
}
```

Wenn der Benutzer die Schaltfläche auswählt, wird der Filter angewendet:

![Zwei Beispiele, in denen das Foto vor und nach dem Anwenden des Filters angezeigt wird](photokit-images/image5.png)

Dank der `PHPhotoLibraryChangeObserver`wird die Änderung in der Auflistungs Ansicht widergespiegelt, wenn der Benutzer zurück navigiert:

![Foto-Sammlungsansicht mit geändertem Foto](photokit-images/image6.png)
