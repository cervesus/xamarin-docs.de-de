---
title: Einführung in coreml in xamarin. IOS
description: In diesem Dokument wird coreml beschrieben, das Machine Learning unter IOS ermöglicht. In diesem Dokument werden die ersten Schritte mit coreml und deren Verwendung mit dem Vision Framework erläutert.
ms.prod: xamarin
ms.assetid: BE1E2CA1-E3AE-4C90-914C-CFDBD1DCB82B
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/30/2017
ms.openlocfilehash: 572ba31a1f19ab099765cc92bb1b389ba1115d1b
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84564692"
---
# <a name="introduction-to-coreml-in-xamarinios"></a>Einführung in coreml in xamarin. IOS

Coreml bringt Machine Learning zu IOS – Apps können trainierte Machine Learning-Modelle nutzen, um alle möglichen Aufgaben auszuführen, von der Problemlösung bis hin zur Bild Erkennung.

Diese Einführung umfasst Folgendes:

- [Einstieg in coreml](#coreml)
- [Verwenden von coreml mit dem Vision-Framework](#coremlvision)

<a name="coreml"></a>

## <a name="getting-started-with-coreml"></a>Einstieg in coreml

In den folgenden Schritten wird beschrieben, wie coreml einem IOS-Projekt hinzugefügt wird. Ein praktisches Beispiel finden Sie im Beispiel für das [Mars-Habitat-pricer](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-marshabitatcoremltimer/) .

![Screenshot des Mars-Habitat-Preis präditors](coreml-images/marspricer-heading.png)

### <a name="1-add-the-coreml-model-to-the-project"></a>1. Fügen Sie das coreml-Modell dem Projekt hinzu.

Fügen Sie dem **Ressourcen** Verzeichnis des Projekts ein coreml-Modell (eine Datei mit der Erweiterung **. mlmodel** ) hinzu. 

In den Eigenschaften der Modelldatei ist die **Buildaktion** auf **coremlmodel**festgelegt. Dies bedeutet, dass Sie beim Erstellen der Anwendung in eine **mlmodelc** -Datei kompiliert wird.

### <a name="2-load-the-model"></a>2. Laden des Modells

Laden Sie das Modell mithilfe der `MLModel.Create` statischen-Methode:

```csharp
var assetPath = NSBundle.MainBundle.GetUrlForResource("NameOfModel", "mlmodelc");
model = MLModel.Create(assetPath, out NSError error1);
```

### <a name="3-set-the-parameters"></a>3. Festlegen der Parameter

Modellparameter werden mithilfe einer Container Klasse, die implementiert, an ein-und ausgehenden `IMLFeatureProvider` .

Funktions Anbieter Klassen Verhalten sich wie ein Wörterbuch von Zeichen folgen und `MLFeatureValue` e, wobei jeder Merkmals Wert eine einfache Zeichenfolge oder Zahl, ein Array oder eine Daten oder ein Pixel Puffer mit einem Bild sein könnte.

Der Code für einen Einzelwert-Funktions Anbieter wird unten dargestellt:

```csharp
public class MyInput : NSObject, IMLFeatureProvider
{
  public double MyParam { get; set; }
  public NSSet<NSString> FeatureNames => new NSSet<NSString>(new NSString("myParam"));
  public MLFeatureValue GetFeatureValue(string featureName)
  {
    if (featureName == "myParam")
      return MLFeatureValue.FromDouble(MyParam);
    return MLFeatureValue.FromDouble(0); // default value
  }
```

Mithilfe von Klassen wie diesem können Eingabeparameter auf eine Weise bereitgestellt werden, die von coreml verstanden wird. Die Namen der Features (z. b. `myParam` im Codebeispiel) müssen mit den Anforderungen des Modells identisch sein.

### <a name="4-run-the-model"></a>4. Ausführen des Modells

Die Verwendung des Modells erfordert, dass der Funktions Anbieter instanziiert und Parameter festgelegt wird, und dass die- `GetPrediction` Methode aufgerufen wird:

```csharp
var input = new MyInput {MyParam = 13};
var outFeatures = model.GetPrediction(inputFeatures, out NSError error2);
```

### <a name="5-extract-the-results"></a>5. Extrahieren der Ergebnisse

Das Vorhersage Ergebnis `outFeatures` ist auch eine Instanz von `IMLFeatureProvider` . auf Ausgabewerte kann mithilfe `GetFeatureValue` von mit dem Namen jedes Ausgabe Parameters (z. b.) zugegriffen werden `theResult` , wie in diesem Beispiel:

```csharp
var result = outFeatures.GetFeatureValue("theResult").DoubleValue; // eg. 6227020800
```

<a name="coremlvision"></a>

## <a name="using-coreml-with-the-vision-framework"></a>Verwenden von coreml mit dem Vision-Framework

Coreml kann auch zusammen mit dem Vision-Framework verwendet werden, um Vorgänge für ein Bild auszuführen, z. b. die Form Erkennung, Objektidentifikation und andere Aufgaben.

In den folgenden Schritten wird beschrieben, wie coreml und Vision zusammen im [coremlvision-Beispiel](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlvision)verwendet werden. Das Beispiel kombiniert die [Rechtecke-Erkennung](~/ios/platform/introduction-to-ios11/vision.md#rectangles) aus dem Vision-Framework mit dem _mninstclassifier_ -coreml-Modell, um eine handschriftliche Ziffer in einem Foto zu identifizieren.

![Bild Erkennung von Zahl 3](coreml-images/vision3.png) ![Bild Erkennung von Zahl 5](coreml-images/vision5.png)

### <a name="1-create-a-vision-coreml-model"></a>1. Erstellen eines Vision-coreml-Modells

Das coreml-Modell " _mnistclassifier_ " wird geladen und anschließend in ein umschließt `VNCoreMLModel` , wodurch das Modell für die Vision von Aufgaben verfügbar ist. Außerdem erstellt dieser Code zwei Anforderungs Anforderungen: zuerst zum Suchen von Rechtecke in einem Bild und dann zur Verarbeitung eines Rechtecks mit dem coreml-Modell:

```csharp
// Load the ML model
var bundle = NSBundle.MainBundle;
var assetPath = bundle.GetUrlForResource("MNISTClassifier", "mlmodelc");
NSError mlErr, vnErr;
var mlModel = MLModel.Create(assetPath, out mlErr);
var model = VNCoreMLModel.FromMLModel(mlModel, out vnErr);

// Initialize Vision requests
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
ClassificationRequest = new VNCoreMLRequest(model, HandleClassification);
```

Die-Klasse muss die `HandleRectangles` -Methode und die- `HandleClassification` Methode für die Anforderungs Anforderungen implementieren, wie in den Schritten 3 und 4 unten gezeigt.

### <a name="2-start-the-vision-processing"></a>2. Starten der Maschinelles Verarbeitung

Der folgende Code beginnt mit der Verarbeitung der Anforderung. Im **coremlvision** -Beispiel wird dieser Code ausgeführt, nachdem der Benutzer ein Bild ausgewählt hat:

```csharp
// Run the rectangle detector, which upon completion runs the ML classifier.
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

Dieser Handler übergibt das `ciImage` an das Vision Framework `VNDetectRectanglesRequest` , das in Schritt 1 erstellt wurde.

### <a name="3-handle-the-results-of-vision-processing"></a>3. behandeln der Ergebnisse der Maschinelles Verarbeitung

Sobald die Rechteck Erkennung vollständig ist, wird die- `HandleRectangles` Methode ausgeführt, die das Bild zum Extrahieren des ersten Rechtecks hochfährt, das Rechteck Bild in Graustufen konvertiert und es für die Klassifizierung an das coreml-Modell übergibt.

Der `request` an diese Methode übergebenen-Parameter enthält die Details der-Anforderungs Anforderung. bei Verwendung der- `GetResults<VNRectangleObservation>()` Methode wird eine Liste der Rechtecke im Bild zurückgegeben. Das erste Rechteck `observations[0]` wird extrahiert und an das coreml-Modell übermittelt:

```csharp
void HandleRectangles(VNRequest request, NSError error) {
  var observations = request.GetResults<VNRectangleObservation>();
  // ... omitted error handling ...
  var detectedRectangle = observations[0]; // first rectangle
  // ... omitted cropping and greyscale conversion ...
  // Run the Core ML MNIST classifier -- results in handleClassification method
  var handler = new VNImageRequestHandler(correctedImage, new VNImageOptions());
  DispatchQueue.DefaultGlobalQueue.DispatchAsync(() => {
    handler.Perform(new VNRequest[] {ClassificationRequest}, out NSError err);
  });
}
```

`ClassificationRequest`Wurde in Schritt 1 initialisiert, um die `HandleClassification` im nächsten Schritt definierte-Methode zu verwenden.

### <a name="4-handle-the-coreml"></a>4. behandeln der coreml

Der `request` -Parameter, der an diese Methode übergeben wird, enthält die Details der coreml-Anforderung. bei Verwendung der- `GetResults<VNClassificationObservation>()` Methode wird eine Liste möglicher Ergebnisse nach Vertrauen (höchste Zuverlässigkeit zuerst) zurückgegeben:

```csharp
void HandleClassification(VNRequest request, NSError error){
  var observations = request.GetResults<VNClassificationObservation>();
  // ... omitted error handling ...
  var best = observations[0]; // first/best classification result
  // render in UI
  DispatchQueue.MainQueue.DispatchAsync(()=>{
    ClassificationLabel.Text = $"Classification: {best.Identifier} Confidence: {best.Confidence * 100f:#.00}%";
  });
}
```

## <a name="samples"></a>Beispiele

Es gibt drei zu Versuchs Bare coreml-Beispiele:

- Das [Beispiel für das Mars-Habitat-Preis präditor](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-marshabitatcoremltimer/) enthält einfache numerische Eingaben und Ausgaben.

- Das [Vision & coreml-Beispiel](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlvision) akzeptiert einen Image-Parameter und verwendet das Vision-Framework, um quadratische Bereiche im Bild zu identifizieren, die an ein coreml-Modell übergeben werden, das einzelne Ziffern erkennt.

- Zum Schluss verwendet das [coreml-Bild Erkennungs Beispiel](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlimagerecognition) coreml, um Funktionen in einem Foto zu identifizieren. Standardmäßig wird das kleinere **presszenetmodell** (5 MB) verwendet, aber es wurde so geschrieben, dass Sie das größere **VGG16** -Modell (553mb) herunterladen und integrieren können. Weitere Informationen finden Sie in der Infodatei des [Beispiels.](https://github.com/xamarin/ios-samples/blob/master/ios11/CoreMLImageRecognition/CoreMLImageRecognition/README.md)

## <a name="related-links"></a>Verwandte Links

- [Machine Learning (Apple)](https://developer.apple.com/machine-learning/)
- [Coreml-Beispiel (Mars-Habitat) (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-marshabitatcoremltimer/)
- [Coreml und Vision (Zahlen Erkennung) (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlvision)
- [Coreml-Bild Erkennung (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlimagerecognition)
- [Coreml mit Azure Custom Vision (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlazuremodel)
- [Einführung in coreml (WWDC) (Video)](https://developer.apple.com/videos/play/wwdc2017/703/)
