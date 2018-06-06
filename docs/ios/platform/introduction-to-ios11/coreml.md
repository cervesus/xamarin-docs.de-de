---
title: Einführung in CoreML in Xamarin.iOS
description: Dieses Dokument beschreibt CoreML, die Machine Learning für iOS ermöglicht. Dieses Dokument wird erläutert, wie zum Einstieg in CoreML und mit der Vision-Framework verwenden.
ms.prod: xamarin
ms.assetid: BE1E2CA1-E3AE-4C90-914C-CFDBD1DCB82B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2016
ms.openlocfilehash: b893fe5e56cc2d43a71870ffbbd20f0b8c6cfd18
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787494"
---
# <a name="introduction-to-coreml-in-xamarinios"></a>Einführung in CoreML in Xamarin.iOS

_Machine learning-für mobile apps auf iOS 11_

CoreML bringt Machine Learning, um iOS-apps nutzen trainierten Machine Learning-Modellen alle Arten von Aufgaben ausführen, aus der Problembehebung zu bilderkennung.

Diese Einführung werden folgende Themen behandelt:

- [Erste Schritte mit CoreML](#coreml)
- [Verwenden von CoreML mit der Vision-framework](#coremlvision)

<a name="coreml" />

## <a name="getting-started-with-coreml"></a>Erste Schritte mit CoreML

Diese Schritte beschrieben, wie CoreML ein iOS-Projekt hinzufügen. Finden Sie in der [Mars Lebensraum Pricer Beispiel](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/) für eines praktischen Beispiels.

![Screenshot der MARS Lebensraum Preis Vorhersage-Beispiel](coreml-images/marspricer-heading.png)

### <a name="1-add-the-model-to-the-project"></a>1. Fügen Sie das Modell dem Projekt hinzu.

Hinzufügen eines kompiliertes Modells (ein Verzeichnis mit der **.modelc** Erweiterung), die **Ressourcen** Verzeichnis des Projekts. Der Inhalt des Verzeichnisses sollte alle besitzen einen Buildvorgang **BundleResource**:

![Ressourcenordner sollte das kompilierte Modell enthalten.](coreml-images/resources-modelc.png)

Die [Beispiele](https://developer.xamarin.com/samples/monotouch/ios11/) Modelle in Xcode 9 kompiliert oder manuell mithilfe des folgenden Terminaldienste-Befehls verwenden:

```csharp
xcrun coremlcompiler compile {model.mlmodel} {outputFolder}
```

> [!NOTE]
> **.Model** Dateien _müssen_ kompiliert werden, um **.modelc** vor der Verwendung von CoreML können

### <a name="2-load-the-model"></a>2. Laden Sie das Modell

Vor dem Verwenden eines Modells, laden sie mit der `MLModel.FromUrl` statische Methode:

```csharp
var assetPath = NSBundle.MainBundle.GetUrlForResource("NameOfModel", "mlmodelc");
model = MLModel.FromUrl(assetPath, out NSError error1);
```

### <a name="3-set-the-parameters"></a>3. Legen Sie die Parameter

Modellparameter übergeben werden, mithilfe von ein-und eine Containerklasse, die implementiert `IMLFeatureProvider`.

Feature-Anbieterklassen Verhalten sich wie ein Wörterbuch der Zeichenfolge und `MLFeatureValue`s, in denen jeder Wert für die Funktion ist möglicherweise eine einfache Zeichenfolge oder Zahl, ein Array oder Daten oder eine pixelpuffer mit einem Bild.

Code für ein einwertiges Featureanbieter wird unten gezeigt:

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

Mithilfe von Klassen wie folgt, können auf eine Weise Eingabeparameter angegeben werden, die von der CoreML verstanden wird. Die Namen der Funktionen (z. B. `myParam` im Codebeispiel) muss übereinstimmen, was erwartet, dass das Modell.

### <a name="4-run-the-model"></a>4. Führen Sie das Modell

Mit dem Modell erfordert, dass die Funktionsanbieter instanziiert wird und die Parameter, klicken Sie dann festgelegt, die die `GetPrediction` Methode aufgerufen werden:

```csharp
var input = new MyInput {MyParam = 13};
var outFeatures = model.GetPrediction(inputFeatures, out NSError error2);
```

### <a name="5-extract-the-results"></a>5. Extrahieren Sie die Ergebnisse

Das Vorhersageergebnis `outFeatures` ist auch eine Instanz der `IMLFeatureProvider`; Ausgabe Werte möglich, die mit `GetFeatureValue` durch den Namen der einzelnen Output-Parameter (z. B. `theResult`), wie in diesem Beispiel:

```csharp
var result = outFeatures.GetFeatureValue("theResult").DoubleValue; // eg. 6227020800
```

<a name="coremlvision" />

## <a name="using-coreml-with-the-vision-framework"></a>Verwenden von CoreML mit der Vision-Framework

CoreML kann auch in Verbindung mit der Vision-Framework verwendet werden, um Vorgänge für Bilds, z. B. Form Recognition, Objekt-ID und andere Aufgaben auszuführen.

Die folgenden Schritte beschreiben, wie CoreML und Vision in gemeinsam verwendet werden die [CoreMLVision Beispiel](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/). Das Beispiel kombiniert die [Rechtecke Recognition](~/ios/platform/introduction-to-ios11/vision.md#rectangles) aus der Vision-Framework mit der _MNINSTClassifier_ CoreML-Modell, um eine handschriftliche Ziffer in ein Foto zu identifizieren.

![Bilderkennung Zahl 3](coreml-images/vision3.png) ![Bilderkennung der Zahl 5](coreml-images/vision5.png)

### <a name="1-create-a-vision-coreml-model"></a>1. Erstellen Sie ein Vision CoreML-Modell

CoreML Modell _MNISTClassifier_ geladen, und klicken Sie dann die eingebunden in eine `VNCoreMLModel` die stellt des Modells für Vision Aufgaben zur Verfügung. Dieser Code erstellt auch zwei Vision-Anforderungen: zuerst für die Suche nach Rechtecke Bestandteil eines Abbilds und dann für ein Rechteck mit dem Modell CoreML verarbeiten:

```csharp
// Load the ML model
var assetPath = NSBundle.MainBundle.GetUrlForResource("MNISTClassifier", "mlmodelc");
var mlModel = MLModel.FromUrl(assetPath, out NSError mlErr);
var vModel = VNCoreMLModel.FromMLModel(mlModel, out NSError vnErr);

// Initialize Vision requests
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
ClassificationRequest = new VNCoreMLRequest(vModel, HandleClassification);
```

Weiterhin muss die Klasse implementieren die `HandleRectangles` und `HandleClassification` Methoden für die Vision-Anforderungen, die in den Schritten 3 und 4 unten gezeigt.

### <a name="2-start-the-vision-processing"></a>2. Starten Sie die Verarbeitung Vision

Im folgende Code wird die Verarbeitung der Anforderung gestartet. In der **CoreMLVision** Beispiel dieser Code ausgeführt wird, nachdem der Benutzer ein Bild ausgewählt wurde:

```csharp
// Run the rectangle detector, which upon completion runs the ML classifier.
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

Dieser Handler übergibt die `ciImage` Vision Framework `VNDetectRectanglesRequest` , die in Schritt 1 erstellt wurde.

### <a name="3-handle-the-results-of-vision-processing"></a>3. Verarbeiten der Ergebnisse der Verarbeitung der Vision

Sobald die Rechteck-Erkennung abgeschlossen ist, führt er die `HandleRectangles` -Methode, die das Bild, um das erste Rechteck extrahieren Kulturen, konvertiert das Rechteck Bild in Graustufen, und übergibt sie an das CoreML-Modell für die Klassifizierung.

Die `request` an diese Methode übergebene Parameter enthält die Details der Anforderung Vision und mithilfe der `GetResults<VNRectangleObservation>()` -Methode gibt eine Liste von Rechtecken, die im Bild zurück. Das erste Rechteck `observations[0]` extrahiert und an das Modell CoreML übergeben wird:

```csharp
void HandleRectangles(VNRequest request, NSError error) {
  var observations = request.GetResults<VNRectangleObservation>();
  // ... omitted error handling ...
  var detectedRectangle = observations[0]; // first rectangle
  // ... omitted cropping and greyscale conversion ...
  // Run the Core ML MNIST classifier -- results in handleClassification method
  var handler = new VNImageRequestHandler(correctedImage, new VNImageOptions());
  DispatchQueue.DefaultGlobalQueue.DispatchAsync(() => {
    handler.Perform(new VNRequest[] { ClassificationRequest }, out NSError err);
  });
}
```

Die `ClassificationRequest` wurde in Schritt 1 verwenden, initialisiert die `HandleClassification` Methode, die im nächsten Schritt definiert.

### <a name="4-handle-the-coreml"></a>4. Handle der CoreML

Die `request` an diese Methode übergebene Parameter enthält die Details der Anforderung CoreML und mithilfe der `GetResults<VNClassificationObservation>()` -Methode gibt eine Liste mit möglichen Ergebnissen, geordnet nach vertrauen zurück (höchste Zuverlässigkeit ersten):

```csharp
void HandleClassification(VNRequest request, NSError error){
  var observations = request.GetResults<VNClassificationObservation>();
  ... omitted error handling ...
  var best = observations[0]; // first/best classification result
  // render in UI
  DispatchQueue.MainQueue.DispatchAsync(()=>{
    ClassificationLabel.Text = $"Classification: {best.Identifier} Confidence: {best.Confidence * 100f:#.00}%";
  });
}
```



## <a name="samples"></a>Proben

Es gibt drei CoreML Beispiele versuchen:

* Die [Mars Lebensraum Preis Vorhersage Beispiel](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/) einfache numerische Eingaben und Ausgaben verfügt.

* Die [Vision & CoreML Beispiel](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/) einen Imageparameter akzeptiert und verwendet das Framework Vision quadratische Regionen in der Abbildung zu identifizieren, die mit einem Modell CoreML übergeben werden, die einzelne Ziffern erkennt.

* Schließlich die [CoreML Bilderkennung Beispiel](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition/) CoreML verwendet, um Funktionen in ein Foto zu identifizieren. Standardmäßig verwendet die kleinere **SqueezeNet** Modell (5MB), aber es ist geschrieben wurden, sodass Sie herunterladen und die größere einbeziehen **VGG16** Modell (553 MB). Weitere Informationen finden Sie unter der [des Beispiels Infodatei](https://github.com/xamarin/ios-samples/blob/master/ios11/CoreMLImageRecognition/CoreMLImageRecognition/README.md).


## <a name="related-links"></a>Verwandte Links

- [Machine Learning (Apple)](https://developer.apple.com/machine-learning/)
- [CoreML-Beispiel (Mars Lebensraum) (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/)
- [CoreML und Vision (Anzahl Recognition) (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/)
- [CoreML Bilderkennung (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition/)
- [CoreML mit Azure benutzerdefinierte Vision (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLAzureModel)
- [Einführung in CoreML (WWDC) (Video)](https://developer.apple.com/videos/play/wwdc2017/703/)
