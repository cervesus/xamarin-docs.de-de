---
title: Maschinelles sehen-Framework in Xamarin.iOS
description: Dieses Dokument beschreibt, wie Sie mit der iOS 11 Vision Framework in Xamarin.iOS. Insbesondere erläutert Rechteck Erkennung und gesichtserkennung.
ms.prod: xamarin
ms.assetid: 7273ED68-7B7D-4252-B3A0-02DB2E357A8C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/31/2017
ms.openlocfilehash: 291cbdb93cfb6ac123d740e98065ba877bb44da5
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61159658"
---
# <a name="vision-framework-in-xamarinios"></a>Maschinelles sehen-Framework in Xamarin.iOS

Das Vision Framework bietet es sich um eine Reihe von neuen Funktionen auf iOS 11, einschließlich für die bildverarbeitung:

- [Rechteck-Erkennung](#rectangles)
- [Gesichtserkennung](#faces)
- Machine Learning-Bildanalyse (ausführlicher [CoreML](~/ios/platform/introduction-to-ios11/coreml.md))
- Barcodeerkennung
- Bildanalyse-Ausrichtung
- Erkennung von Text
- Horizont-Erkennung
- Objekterkennung und Überwachung

![Fotografieren Sie mit drei Rechtecken erkannt](vision-images/found-rectangles-tiny.png) ![Fotografieren Sie mit zwei Gesichter erkannt](vision-images/xamarin-home-faces-tiny.png)

Rechteck zur Erkennung und Gesichtserkennung werden im folgenden ausführlicher erläutert.

<a name="rectangles" />

## <a name="rectangle-detection"></a>Rechteck-Erkennung

Die [VisionRects Beispiel](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/) das Verarbeiten eines Images, und zeichnen die erkannten Rechtecke darauf zeigt.

### <a name="1-initialize-the-vision-request"></a>1. Initialisieren der maschinelles sehen-Anforderung

In `ViewDidLoad`, erstellen Sie eine `VNDetectRectanglesRequest` , die auf die `HandleRectangles` Methode, die am Ende jeder Anforderung aufgerufen wird:

Die `MaximumObservations` Eigenschaft auch festgelegt werden soll, andernfalls wird standardmäßig auf 1 und nur ein einzelnes Ergebnis zurückgegeben.

```csharp
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
RectangleRequest.MaximumObservations = 10;
```

### <a name="2-start-the-vision-processing"></a>2. Starten Sie die Verarbeitung der maschinelles sehen

Der folgende Code startet die Verarbeitung der Anforderung. In der **VisionRects** Beispiel dieser Code ausgeführt wird, nachdem der Benutzer ein Bild ausgewählt hat:

```csharp
// Run the rectangle detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

Dieser Handler übergibt die `ciImage` Vision Framework `VNDetectRectanglesRequest` , die in Schritt 1 erstellt wurde.

### <a name="3-handle-the-results-of-vision-processing"></a>3. Verarbeiten der Ergebnisse der Verarbeitung der maschinelles sehen

Sobald die Erkennung Rechteck abgeschlossen ist, führt das Framework die `HandleRectangles` -Methode, eine Zusammenfassung der wird unten gezeigt:

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

### <a name="4-display-the-results"></a>4. Die Ergebnisse werden angezeigt

Die `OverlayRectangles` -Methode in der die **VisionRectangles** Beispiel verfügt über drei Funktionen:

- Das Quellbild Rendering
- Zeichnen eines Rechtecks um anzugeben, in denen jeweils erkannt wurde, und
- Eine textbezeichnung für jedes Rechteck CoreGraphics wird hinzugefügt.

Anzeigen der [des Beispielquelle](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/) für die genaue CoreGraphics-Methode.

![Fotografieren Sie mit drei Rechtecken erkannt](vision-images/found-rectangles-phone-sml.png)

### <a name="5-further-processing"></a>5. Weiterverarbeitung

Rechteck-Erkennung ist häufig nur der erste Schritt in einer Kette von Vorgängen, z. B. mit [in diesem Beispiel CoreMLVision](~/ios/platform/introduction-to-ios11/coreml.md#coremlvision), in denen die Rechtecke mit einem CoreML-Modell zum Analysieren von handgeschriebener Ziffern übergeben werden.


<a name="faces" />

## <a name="face-detection"></a>Gesichtserkennung

Die [VisionFaces Beispiel](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/) funktioniert auf ähnliche Weise wie die **VisionRectangles** Beispiels mit einer anderen Klasse der maschinelles sehen-Anforderung.

### <a name="1-initialize-the-vision-request"></a>1. Initialisieren der maschinelles sehen-Anforderung

In `ViewDidLoad`, erstellen Sie eine `VNDetectFaceRectanglesRequest` , die auf die `HandleRectangles` Methode, die am Ende jeder Anforderung aufgerufen wird.

```csharp
FaceRectangleRequest = new VNDetectFaceRectanglesRequest(HandleRectangles);
```

### <a name="2-start-the-vision-processing"></a>2. Starten Sie die Verarbeitung der maschinelles sehen

Der folgende Code startet die Verarbeitung der Anforderung. In der **VisionFaces** Beispiel dieser Code ausgeführt wird, nachdem der Benutzer ein Bild ausgewählt hat:

```csharp
// Run the face detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {FaceRectangleRequest}, out NSError error);
});
```

Dieser Handler übergibt die `ciImage` Vision Framework `VNDetectFaceRectanglesRequest` , die in Schritt 1 erstellt wurde.

### <a name="3-handle-the-results-of-vision-processing"></a>3. Verarbeiten der Ergebnisse der Verarbeitung der maschinelles sehen

Sobald die gesichtserkennung abgeschlossen ist, führt der Handler die `HandleRectangles` Methode führt eine Fehlerbehandlung und zeigt die Grenzen des erkannten Gesichtern, und ruft die `OverlayRectangles` zum Zeichnen von umgebenden Rechtecken auf das ursprüngliche Bild:

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

### <a name="4-display-the-results"></a>4. Die Ergebnisse werden angezeigt

Die `OverlayRectangles` -Methode in der die **VisionFaces** Beispiel verfügt über drei Funktionen:

- Das Quellbild Rendering
- Zeichnen eines Rechtecks für jedes Gesicht erkannt, und
- Hinzufügen einer textbezeichnung für jedes Gesicht CoreGraphics verwenden.

Anzeigen der [des Beispielquelle](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/) für die genaue CoreGraphics-Methode.

![Fotografieren Sie mit zwei Gesichter erkannt](vision-images/found-faces-phone-sml.png)

### <a name="5-further-processing"></a>5. Weiterverarbeitung

Das Vision Framework enthält zusätzliche Funktionen, um gesichtsmerkmale, z. B. die Auge und Mund zu erkennen. Verwenden der `VNDetectFaceLandmarksRequest` Typ, der zurückgibt `VNFaceObservation` Ergebnisse wie in Schritt 3 oben, jedoch mit zusätzlichen `VNFaceLandmark` Daten.


## <a name="related-links"></a>Verwandte Links

- [Vision Rechtecke (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/)
- [Vision Gesichter (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/)
- [Fortschritte in der Core-Image - Filter, die Metal, maschinelles sehen und mehr (WWDC) (Video)](https://developer.apple.com/videos/play/wwdc2017/510/)
