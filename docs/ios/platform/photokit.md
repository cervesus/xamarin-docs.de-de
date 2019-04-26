---
title: PhotoKit in Xamarin.iOS
description: Dieses Dokument beschreibt PhotoKit, erläutern die Model-Objekte, wie Sie Abfragen von Modelldaten, und Speichern von Änderungen an der Fotobibliothek.
ms.prod: xamarin
ms.assetid: 7FDEE394-3787-40FA-8372-76A05BF184B3
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/14/2017
ms.openlocfilehash: d59cac9403a244ce553d84e0590b8a9c3d4d2f30
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61365869"
---
# <a name="photokit-in-xamarinios"></a>PhotoKit in Xamarin.iOS

PhotoKit ist ein neues Framework, das ermöglicht es Anwendungen, die systembilderbibliothek Abfragen und die Erstellung benutzerdefinierter Benutzeroberflächen zum Anzeigen und ändern seinen Inhalt. Sie enthält eine Reihe von Klassen, die Images und Videoassets sowie Sammlungen von Ressourcen wie z. B. Alben und Ordner darstellen.

## <a name="model-objects"></a>Model-Objekte

PhotoKit stellt diese Objekte in den es Model-Objekte dar. Die Modellobjekte, Fotos und Videos selbst darstellen, sind vom Typ `PHAsset`. Ein `PHAsset` enthält Metadaten, wie z. B. des Medienobjekts Medientyp und dem Erstellungsdatum.
Auf ähnliche Weise die `PHAssetCollection` und `PHCollectionList` Klassen enthalten bzw. die Metadaten über Asset-Auflistungen und die Auflistung enthält. Asset-Sammlungen sind Gruppen von Ressourcen, wie z. B. alle Fotos und Videos für ein bestimmtes Jahr. Ebenso werden die Sammlung Listen Gruppen der Medienobjekt-Auflistungen, z. B. Fotos und Videos, gruppiert nach Jahr.

## <a name="querying-model-data"></a>Abfragen von Modelldaten

PhotoKit erleichtert den Modelldaten für die Abfrage über eine Vielzahl von Fetch-Methoden. Z. B. zum Abrufen aller Images, Sie würden Aufrufen `PFAsset.Fetch`, und übergeben Sie die `PHAssetMediaType.Image` Medientyp.

    PHFetchResult fetchResults = PHAsset.FetchAssets (PHAssetMediaType.Image, null);

Die `PHFetchResult` Instanz würde dann alle enthalten die `PFAsset` Instanzen, die Images darstellen. Um die Bilder sich selbst zu erhalten, verwenden Sie die `PHImageManager` (oder die Zwischenspeicherung Version `PHCachingImageManager`) auf eine Anforderung für das Image durch Aufrufen von `RequestImageForAsset`. Der folgende Code ruft z. B. ein Bild für jedes Medienobjekt in einem `PHFetchResult` in einer sammlungsansichtszelle angezeigt:


    public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
    {
              var imageCell = (ImageCell)collectionView.DequeueReusableCell (cellId, indexPath);
            imageMgr.RequestImageForAsset ((PHAsset)fetchResults [(uint)indexPath.Item], thumbnailSize,
        PHImageContentMode.AspectFill, new PHImageRequestOptions (), (img, info) => {
            imageCell.ImageView.Image = img;
        });
        return imageCell;
    }

Dadurch wird ein Raster mit Bildern, wie unten dargestellt:

![](photokit-images/image4.png "Der ausgeführten app, die ein Raster mit Images anzeigen")
 
## <a name="saving-changes-to-the-photo-library"></a>Speichern von Änderungen in der Fotobibliothek

Dies ist das Durchführen von Abfragen und Lesen von Daten. Sie können die Änderungen auch wieder in die Bibliothek schreiben. Da mehrere möchte Anwendungen mit der Fotobibliothek System interagieren können, können Sie einen Beobachter für Änderungen, die mit einem PhotoLibraryObserver informiert werden registrieren. Klicken Sie dann, wenn Änderungen eingehen, kann Ihre Anwendung entsprechend aktualisieren. Hier ist beispielsweise eine einfache Implementierung, die oben genannten Auflistungsansicht neu zu laden:

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
    
Um tatsächlich Änderungen wieder in Ihrer Anwendung zu schreiben, erstellen Sie einen Change Request. Jede der ViewModel-Klassen verfügt über eine ihr verbundenen Change Request-Klasse. Um eine PHAsset zu ändern, erstellen Sie z. B. eine PHAssetChangeRequest. Die Schritte zum Durchführen von Änderungen, die in der Fotobibliothek zurückgeschrieben und an die Beobachter wie oben gesendet werden:

-   Führen Sie die Bearbeitung des.
-   Speichern Sie die gefilterte Image-Daten in eine PHContentEditingOutput-Instanz ein.
-   Stellen Sie eine änderungsanforderung, um dem Formular für die Änderungen die Bearbeitung Ausgabe veröffentlichen.

Hier ist ein Beispiel, das wieder eine Änderung zu einem Bild schreibt, die einen Core-Image Noir Filter angewendet wird:

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
    
Wenn der Benutzer die Schaltfläche auswählt, wird der Filter angewendet:

![](photokit-images/image5.png "Ein Beispiel der Filter angewendet wird")
 
Und Dank der PHPhotoLibraryChangeObserver, wird die Änderung in der Auflistungsansicht übernommen, wenn der Benutzer, zurück navigiert:

![](photokit-images/image6.png "Wenn der Benutzer zurück navigiert, wird die Änderung in der Auflistungsansicht übernommen")
