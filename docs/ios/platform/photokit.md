---
title: PhotoKit
description: Foto Kit kann apps für die System-Bildbibliothek Abfragen und erstellen Sie benutzerdefinierte Benutzeroberfläche zum Anzeigen und ändern den Inhalt.
ms.prod: xamarin
ms.assetid: 7FDEE394-3787-40FA-8372-76A05BF184B3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: c721064f62f8e2255de2b4ea2d0438e3ed630d39
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="photokit"></a>PhotoKit

_Foto Kit kann apps für die System-Bildbibliothek Abfragen und erstellen Sie benutzerdefinierte Benutzeroberfläche zum Anzeigen und ändern den Inhalt._

Foto-Kit ist ein neues Framework, das ermöglicht es Anwendungen, die systembilderbibliothek Abfragen und die Erstellung benutzerdefinierter Benutzeroberflächen zum Anzeigen und ändern den Inhalt. Umfasst eine Reihe von Klassen, die Bild und video Bestand sowie Auflistungen von Ressourcen wie z. B. Alben und Ordner darstellen.

## <a name="model-objects"></a>Model-Objekte
Foto Kit stellt diese Ressourcen in was Modellobjekte aufgerufen. Die Model-Objekte, die Fotos und Videos selbst darstellen, sind vom Typ `PHAsset`. Ein `PHAsset` enthält Metadaten, z. B. Medientyp für das Medienobjekt und seine Erstellungsdatum.
Auf ähnliche Weise die `PHAssetCollection` und `PHCollectionList` Klassen enthalten Metadaten Asset Sammlungen und Sammlung Listen bzw. Asset-Auflistungen sind Gruppen von Ressourcen, wie z. B. alle Fotos und Videos für ein bestimmtes Jahr. Sammlung Listen auch und Gruppen der Medienobjekt-Auflistungen, z. B. Fotos und Videos, die nach Jahr gruppiert.

## <a name="querying-model-data"></a>Abfragen von Modelldaten
Foto Kit vereinfacht den Modelldaten Abfrage über eine Vielzahl von Fetch-Methoden. Beispielsweise, um alle Images abzurufen, würde rufen Sie `PFAsset.Fetch`, und übergeben Sie die `PHAssetMediaType.Image` Medientyp.

    PHFetchResult fetchResults = PHAsset.FetchAssets (PHAssetMediaType.Image, null);

Die `PHFetchResult` Instanz würde dann alle enthalten die `PFAsset` Instanzen, die Bilder darstellen. Um die Bilder selbst zu erhalten, verwenden Sie die `PHImageManager` (oder die Zwischenspeichern Version `PHCachingImageManager`) auf eine Anforderung für das Image durch Aufrufen von `RequestImageForAsset`. Der folgende Code ruft z. B. ein Bild für jedes Medienobjekt in einen `PHFetchResult` in einer Sammlung anzeigen Zelle angezeigt:


    public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
    {
              var imageCell = (ImageCell)collectionView.DequeueReusableCell (cellId, indexPath);
            imageMgr.RequestImageForAsset ((PHAsset)fetchResults [(uint)indexPath.Item], thumbnailSize,
        PHImageContentMode.AspectFill, new PHImageRequestOptions (), (img, info) => {
            imageCell.ImageView.Image = img;
        });
        return imageCell;
    }

Daraus ergibt sich ein Raster von Bildern, wie unten dargestellt:

![](photokit-images/image4.png "Der ausgeführten app anzeigen ein Raster von Bildern")
 
## <a name="saving-changes-to-the-photo-library"></a>Speichern von Änderungen auf die Fotobibliothek

Gewusst wie: Behandeln von Abfragen und Lesen von Daten ist. Sie können Änderungen auch in der Bibliothek wieder schreiben. Da mehrere Interesse Anwendungen mit dem Foto-Systembibliothek interagieren können, können Sie eine "Beobachter" Änderungen mit einem PhotoLibraryObserver benachrichtigt werden registrieren. Klicken Sie dann bei Änderungen eingehen, kann Ihre Anwendung entsprechend aktualisieren. Hier ist z. B. eine einfache Implementierung, die oben genannten Auflistungsansicht erneut zu laden:

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
    
Um tatsächlich ändert sich von Ihrer Anwendung zurückzuschreiben, erstellen Sie einen Change Request. Jedes der Modellklassen verfügt über eine ihr verbundenen Change Request-Klasse. Um eine PHAsset zu ändern, erstellen Sie z. B. eine PHAssetChangeRequest. Die Schritte zum Durchführen von Änderungen, die zurück in die Fotobibliothek geschrieben und an die Beobachter wie oben gesendet werden:

-   Führen Sie die Bearbeitung des.
-   Speichern Sie die gefilterten Bilddaten in eine PHContentEditingOutput-Instanz.
-   Stellen Sie eine änderungsanforderung, um die Änderungen die Bearbeitung Ausgabe veröffentlichen.

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

![](photokit-images/image5.png "Ein Beispiel für den filter")
 
Und Dank der PHPhotoLibraryChangeObserver wird die Änderung in der Auflistungsansicht übernommen, wenn der Benutzer zurück navigiert werden:

![](photokit-images/image6.png "Wenn der Benutzer zurück navigiert, wird die Änderung in der Auflistungsansicht übernommen")
