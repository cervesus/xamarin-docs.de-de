---
title: Vision-Framework in Xamarin.iOS
description: Dieses Dokument beschreibt, wie die iOS 11 Vision-Framework in Xamarin.iOS. Insbesondere erläutert es Rechteck Erkennung und Erkennung stehen.
ms.prod: xamarin
ms.assetid: 7273ED68-7B7D-4252-B3A0-02DB2E357A8C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/31/2016
ms.openlocfilehash: c44c4b3ab12c1ba448f1befb6f831f5ad9119f18
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787418"
---
# <a name="vision-framework-in-xamarinios"></a>Vision-Framework in Xamarin.iOS

Das Vision Framework Fügt eine Anzahl von neuen Features für iOS-11, einschließlich der bildverarbeitung hinzu:

- [Rechteck-Erkennung](#rectangles)
- [Vordere Erkennung](#faces)
- Machine Learning-Bildanalysen (beschrieben [CoreML](~/ios/platform/introduction-to-ios11/coreml.md))
- Barcode-Erkennung
- Ausrichtung Bildanalysen
- Erkennung von Text
- Horizont-Erkennung
- Objekt-Erkennung und Überwachung

![Foto mit drei Rechtecke erkannt](vision-images/found-rectangles-tiny.png) ![Foto mit zwei Flächen erkannt](vision-images/xamarin-home-faces-tiny.png)

Rechteck-Erkennung und Gesicht Erkennung werden im folgenden ausführlicher erläutert.

<a name="rectangles" />

## <a name="rectangle-detection"></a>Rechteck-Erkennung

Die [VisionRects Beispiel](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/) wird gezeigt, wie ein Bild zu verarbeiten, und zeichnen die erkannten Rechtecke darauf.

### <a name="1-initialize-the-vision-request"></a>1. Initialisieren Sie die Anforderung Vision

In `ViewDidLoad`, erstellen Sie eine `VNDetectRectanglesRequest` verweist, die auf die `HandleRectangles` Methode, die am Ende jeder Anforderung aufgerufen wird:

Die `MaximumObservations` Eigenschaft sollte auch festgelegt werden, andernfalls wird standardmäßig auf 1 und nur ein einzelnes Ergebnis zurückgegeben.

```csharp
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
RectangleRequest.MaximumObservations = 10;
```

### <a name="2-start-the-vision-processing"></a>2. Starten Sie die Verarbeitung Vision

Im folgende Code wird die Verarbeitung der Anforderung gestartet. In der **VisionRects** Beispiel dieser Code ausgeführt wird, nachdem der Benutzer ein Bild ausgewählt wurde:

```csharp
// Run the rectangle detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

Dieser Handler übergibt die `ciImage` Vision Framework `VNDetectRectanglesRequest` , die in Schritt 1 erstellt wurde.

### <a name="3-handle-the-results-of-vision-processing"></a>3. Verarbeiten der Ergebnisse der Verarbeitung der Vision

Sobald die Rechteck-Erkennung abgeschlossen ist, führt das Framework der `HandleRectangles` -Methode, eine Zusammenfassung der wird unten gezeigt:

```csharp
private void HandleRectangles(VNRequest request, NSError error){
  var observations = request.GetResults<VNRectangleObservation>();
  // ... omitted error handling ...
  bool atLeastOneValid = false;
  foreach (var o in observations){
    if (InputImage.Extent.Contains(boundingBox)) {
      atLeastOneValid |= true;
    }
  }
  if (!atLeastOneValid) return;
  // Show the pre-processed image
  DispatchQueue.MainQueue.DispatchAsync(() =>
  {
    ClassificationLabel.Text = summary;
    ImageView.Image = OverlayRectangles(RawImage, imageSize, observations);
  });
}
```

### <a name="4-display-the-results"></a>4. Die Ergebnisse werden angezeigt.

Die `OverlayRectangles` Methode in der **VisionRectangles** Beispiel hat drei Funktionen:

- Rendering Quellbilds
- Zeichnen ein Rechteck, um anzugeben, auf denen jeweils erkannt wurde, und
- Hinzufügen einer textbezeichnung für jedes Rechteck CoreGraphics an.

Anzeigen der [des Beispielquelle](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/) für die genaue CoreGraphics-Methode.

![Foto mit drei Rechtecke erkannt](vision-images/found-rectangles-phone-sml.png)

### <a name="5-further-processing"></a>5. Weiterverarbeitung

Rechteck Erkennung ist häufig nur der erste Schritt in einer Kette von Vorgängen, z. B. mit [in diesem Beispiel CoreMLVision](~/ios/platform/introduction-to-ios11/coreml.md#coremlvision), wobei Rechtecke mit einem Modell CoreML beim Analysieren von Hand geschriebene Ziffern übergeben werden.


<a name="faces" />

## <a name="face-detection"></a>Vordere Erkennung

Die [VisionFaces Beispiel](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/) funktioniert in ähnlicher Weise wie die **VisionRectangles** Beispiel mit einer anderen Vision Anforderung-Klasse.

### <a name="1-initialize-the-vision-request"></a>1. Initialisieren Sie die Anforderung Vision

In `ViewDidLoad`, erstellen Sie eine `VNDetectFaceRectanglesRequest` verweist, die auf die `HandleRectangles` Methode, die am Ende jeder Anforderung aufgerufen wird.

```csharp
FaceRectangleRequest = new VNDetectFaceRectanglesRequest(HandleRectangles);
```

### <a name="2-start-the-vision-processing"></a>2. Starten Sie die Verarbeitung Vision

Im folgende Code wird die Verarbeitung der Anforderung gestartet. In der **VisionFaces** Beispiel dieser Code ausgeführt wird, nachdem der Benutzer ein Bild ausgewählt wurde:

```csharp
// Run the face detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {FaceRectangleRequest}, out NSError error);
});
```

Dieser Handler übergibt die `ciImage` Vision Framework `VNDetectFaceRectanglesRequest` , die in Schritt 1 erstellt wurde.

### <a name="3-handle-the-results-of-vision-processing"></a>3. Verarbeiten der Ergebnisse der Verarbeitung der Vision

Sobald die Erkennung Gesicht abgeschlossen ist, führt der Handler der `HandleRectangles` Methode, die Fehlerbehandlung ausgeführt und zeigt die Grenzen der erkannten Gesichter und Aufrufe an die `OverlayRectangles` zum Zeichnen von umgebenden Rechtecken für das ursprüngliche Bild:

```csharp
private void HandleRectangles(VNRequest request, NSError error){
  var observations = request.GetResults<VNFaceObservation>();
  // ... omitted error handling...
  var summary = "";
  var imageSize = InputImage.Extent.Size;
  bool atLeastOneValid = false;
  Console.WriteLine("Faces:");
  summary += "Faces:";
  foreach (var o in observations) {
    // Verify detected rectangle is valid. omitted
    var boundingBox = o.BoundingBox.Scaled(imageSize);
    if (InputImage.Extent.Contains(boundingBox)) {
      atLeastOneValid |= true;
    }
  }
  // Show the pre-processed image (on UI thread)
  DispatchQueue.MainQueue.DispatchAsync(() =>
  {
    ClassificationLabel.Text = summary;
    ImageView.Image = OverlayRectangles(RawImage, imageSize, observations);
  });
}
```

### <a name="4-display-the-results"></a>4. Die Ergebnisse werden angezeigt.

Die `OverlayRectangles` Methode in der **VisionFaces** Beispiel hat drei Funktionen:

- Rendering Quellbilds
- Zeichnen ein Rechteck für jede Schrift erkannt, und
- Hinzufügen eine Beschriftung für jede Fläche CoreGraphics verwenden.

Anzeigen der [des Beispielquelle](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/) für die genaue CoreGraphics-Methode.

![Foto mit zwei Flächen erkannt](vision-images/found-faces-phone-sml.png)

### <a name="5-further-processing"></a>5. Weiterverarbeitung

Das Vision-Framework umfasst zusätzliche Funktionen für gesichtsmerkmale, z. B. die Augen und Mund zu erkennen. Verwenden der `VNDetectFaceLandmarksRequest` -Typ, der zurückliefert `VNFaceObservation` Ergebnisse wie in Schritt 3 oben, jedoch mit zusätzlichen `VNFaceLandmark` Daten.


## <a name="related-links"></a>Verwandte Links

- [Vision Rechtecke (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/)
- [Vision Flächen (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/)
- [Fortschritte in der Core-Image - Filter, Metall Vision und mehr (WWDC) (Video)](https://developer.apple.com/videos/play/wwdc2017/510/)
