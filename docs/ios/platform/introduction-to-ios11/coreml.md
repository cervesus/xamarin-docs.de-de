---
title: Einführung in die CoreML in Xamarin.iOS
description: Dieses Dokument beschreibt CoreML, die Machine Learning in iOS ermöglicht. In diesem Dokument wird erläutert, wie erste Schritte mit CoreML und für die Verwendung mit der maschinelles sehen-Framework.
ms.prod: xamarin
ms.assetid: BE1E2CA1-E3AE-4C90-914C-CFDBD1DCB82B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2017
ms.openlocfilehash: 13178d4530e3214c6cf31c1018b21815ccd2227f
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350683"
---
# <a name="introduction-to-coreml-in-xamarinios"></a>Einführung in die CoreML in Xamarin.iOS

CoreML bringt Machine Learning für iOS – apps nutzen trainierten Machine Learning-Modelle alle möglichen Aufgaben ausführen, über die Behebung von Problemen, bilderkennung.

In dieser Einführung werden folgende Themen behandelt:

- [Erste Schritte mit CoreML](#coreml)
- [Verwenden von CoreML mit der maschinelles sehen-framework](#coremlvision)

<a name="coreml" />

## <a name="getting-started-with-coreml"></a>Erste Schritte mit CoreML

Diesen Schritten wird beschrieben, wie ein iOS-Projekt CoreML hinzugefügt wird. Finden Sie in der [Mars Habitat Pricer Beispiel](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/) für ein praktisches Beispiel.

![MARS Habitat Preis Prädiktor-beispielscreenshot:](coreml-images/marspricer-heading.png)

### <a name="1-add-the-coreml-model-to-the-project"></a>1. Das CoreML-Modell zum Projekt hinzufügen

Hinzufügen eines CoreML-Modells (eine Datei mit den **.mlmodel** Erweiterung), die **Ressourcen** Verzeichnis des Projekts. 

Im die Eigenschaften der Modelldatei die **Buildvorgang** nastaven NA hodnotu **CoreMLModel**. Dies bedeutet, dass es in kompiliert werden, wird ein **.mlmodelc** -Datei, wenn die Anwendung erstellt wird.

### <a name="2-load-the-model"></a>2. Laden des Modells

Laden Sie das Modell, indem die `MLModel.Create` statische Methode:

```csharp
var assetPath = NSBundle.MainBundle.GetUrlForResource("NameOfModel", "mlmodelc");
model = MLModel.Create(assetPath, out NSError error1);
```

### <a name="3-set-the-parameters"></a>3. Legen Sie die Parameter

Modellparameter übergeben und verkleinern mithilfe einer Containerklasse, die implementiert `IMLFeatureProvider`.

Feature-Anbieterklassen Verhalten sich wie ein Wörterbuch der Zeichenfolge und `MLFeatureValue`s, in dem jeder Wert für die Funktion ist möglicherweise eine einfache Zeichenfolge oder Zahl, ein Array oder Daten oder einen pixelpuffer mit einem Bild.

Code für ein einzelner Wert Featureanbieter wird unten gezeigt:

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

Mithilfe von Klassen wie folgt, können auf eine Weise Eingabeparameter angegeben werden, die von CoreML verstanden wird. Die Namen der Funktionen (z. B. `myParam` im Codebeispiel wird) muss übereinstimmen, was erwartet, dass das Modell.

### <a name="4-run-the-model"></a>4. Führen Sie das Modell

Mit dem Modell erfordert, dass die Funktionsanbieter instanziiert wird und die Parameter, klicken Sie dann festgelegt, die die `GetPrediction` Methode aufgerufen werden:

```csharp
var input = new MyInput {MyParam = 13};
var outFeatures = model.GetPrediction(inputFeatures, out NSError error2);
```

### <a name="5-extract-the-results"></a>5. Extrahieren Sie die Ergebnisse

Das Vorhersageergebnis `outFeatures` ist auch eine Instanz der `IMLFeatureProvider`; Output Werte kann zugegriffen werden, mithilfe von `GetFeatureValue` durch den Namen der einzelnen Output-Parameter (z. B. `theResult`), wie in diesem Beispiel:

```csharp
var result = outFeatures.GetFeatureValue("theResult").DoubleValue; // eg. 6227020800
```

<a name="coremlvision" />

## <a name="using-coreml-with-the-vision-framework"></a>Verwenden von CoreML mit der maschinelles sehen-Framework

CoreML kann auch in Verbindung mit der maschinelles sehen-Framework verwendet werden, um Vorgänge für Images, z.B. Form Anerkennung, Objekt-ID und andere Aufgaben auszuführen.

Die folgenden Schritte beschreiben, wie CoreML und Vision zusammen in verwendet werden die [CoreMLVision Beispiel](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/). Das Beispiel kombiniert die [Rechtecke Anerkennung](~/ios/platform/introduction-to-ios11/vision.md#rectangles) aus der Vision Framework mit der _MNINSTClassifier_ CoreML-Modell, um eine handgeschriebene Ziffern in ein Foto zu identifizieren.

![Bilderkennung der Nummer 3](coreml-images/vision3.png) ![Bilderkennung, die der Zahl 5](coreml-images/vision5.png)

### <a name="1-create-a-vision-coreml-model"></a>1. Erstellen Sie ein Vision CoreML-Modell

Das CoreML-Modell _MNISTClassifier_ geladen und dann innerhalb einer `VNCoreMLModel` die stellt des Modells für maschinelles sehen-Aufgaben zur Verfügung. Dieser Code erstellt außerdem zwei Vision-Anforderungen: zuerst für Rechtecke in einem Image suchen und dann für die Verarbeitung eines Rechtecks mit dem CoreML-Modell:

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

Die Klasse muss immer noch zum Implementieren der `HandleRectangles` und `HandleClassification` Methoden für die maschinelles sehen-Anforderungen, die in den Schritten 3 und 4 weiter unten gezeigten.

### <a name="2-start-the-vision-processing"></a>2. Starten Sie die Verarbeitung der maschinelles sehen

Der folgende Code startet die Verarbeitung der Anforderung. In der **CoreMLVision** Beispiel dieser Code ausgeführt wird, nachdem der Benutzer ein Bild ausgewählt hat:

```csharp
// Run the rectangle detector, which upon completion runs the ML classifier.
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

Dieser Handler übergibt die `ciImage` Vision Framework `VNDetectRectanglesRequest` , die in Schritt 1 erstellt wurde.

### <a name="3-handle-the-results-of-vision-processing"></a>3. Verarbeiten der Ergebnisse der Verarbeitung der maschinelles sehen

Sobald die Erkennung Rechteck abgeschlossen ist, führt er die `HandleRectangles` Methode, die schneidet das Bild, um das erste Rechteck zu extrahieren, das Rechteck Bild in Graustufen konvertiert, und übergibt sie an das CoreML-Modell für die Klassifizierung.

Die `request` an diese Methode übergebene Parameter enthält die Details der maschinelles sehen-Anforderung, und Verwenden der `GetResults<VNRectangleObservation>()` -Methode gibt eine Liste von Rechtecken, die im Abbild gefundenen zurück. Das erste Rechteck `observations[0]` extrahiert und an das Modell CoreML übergeben:

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

Die `ClassificationRequest` initialisiert wurde, in Schritt 1 verwendet der `HandleClassification` Methode, die im nächsten Schritt definiert.

### <a name="4-handle-the-coreml"></a>4. Handle der CoreML

Die `request` an diese Methode übergebene Parameter enthält die Details der Anforderung CoreML, und Verwenden der `GetResults<VNClassificationObservation>()` -Methode wird eine Liste der möglichen Ergebnisse sortiert nach vertrauen (höchste Zuverlässigkeit erste):

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

## <a name="samples"></a>Proben

Es gibt drei CoreML-Beispiele, um zu versuchen:

* Die [Mars Habitat Preis Prädiktor Beispiel](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/) verfügt über einfache numerische Eingaben und Ausgaben.

* Die [maschinelles sehen und CoreML-Beispiel](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/) einen Imageparameter akzeptiert und verwendet die maschinelles sehen-Framework, um die Square-Regionen in der Abbildung zu identifizieren, die zu einem CoreML-Modell übergeben werden, die einzelne Ziffern erkannt.

* Zum Schluss die [CoreML-Bilderkennung Beispiel](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition/) CoreML verwendet, um Funktionen in ein Foto zu ermitteln. Standardmäßig verwendet die kleinere **SqueezeNet** Modell (5MB), aber es wurde geschrieben, damit können Sie herunterladen und integrieren die größere **VGG16** Modell (553 MB). Weitere Informationen finden Sie unter den [-Infodatei des Beispiels](https://github.com/xamarin/ios-samples/blob/master/ios11/CoreMLImageRecognition/CoreMLImageRecognition/README.md).

## <a name="related-links"></a>Verwandte Links

- [Machine Learning (Apple)](https://developer.apple.com/machine-learning/)
- [CoreML-Beispiel (Mars-Habitat) (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/)
- [CoreML und Anzeigekomponenten (Anzahl Recognition) (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/)
- [CoreML-Bilderkennung (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition/)
- [CoreML mit Azure Custom Vision (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLAzureModel)
- [Einführung in CoreML (WWDC) (Video)](https://developer.apple.com/videos/play/wwdc2017/703/)
