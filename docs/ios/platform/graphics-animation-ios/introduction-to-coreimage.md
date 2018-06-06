---
title: Core-Image in Xamarin.iOS
description: Core-Image ist ein neues Framework mit iOS 5 zum Bereitstellen von bildverarbeitungs- und live-Funktionalität der video-Erweiterung eingeführt wurden. In diesem Artikel stellt diese Funktionen mit Xamarin.iOS Beispiele.
ms.prod: xamarin
ms.assetid: 91E0780B-FF8A-E70D-9CD4-419119612B2D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 6032554a0ddbda26ff5de94f6035bc4f8c15a22a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786631"
---
# <a name="core-image-in-xamarinios"></a>Core-Image in Xamarin.iOS

_Core-Image ist ein neues Framework mit iOS 5 zum Bereitstellen von bildverarbeitungs- und live-Funktionalität der video-Erweiterung eingeführt wurden. In diesem Artikel stellt diese Funktionen mit Xamarin.iOS Beispiele._

Core-Image ist ein neues Framework eingeführt, die unter iOS 5, die eine Reihe von integrierten Filter und die Auswirkungen für Bilder und Videos, einschließlich Gesicht gelten bereitstellt.

Dieses Dokument enthält einfache Beispiele für:

-  Erkennung von Gesicht.
-  Anwenden von Filtern auf ein Bild
-  Eine Liste der verfügbaren Filter.


Diese Beispiele sollen Ihnen beim Einstieg helfen Core-Image-Funktionen in Ihre Anwendungen Xamarin.iOS einbinden.

## <a name="requirements"></a>Anforderungen

Sie müssen die neueste Version von Xcode verwenden.

## <a name="face-detection"></a>Vordere Erkennung

Die Core-Image Gesicht-Funktion zur Erkennung wird nur der Aussage – er versucht, die Flächen in einem Foto zu identifizieren, und gibt die Koordinaten der alle Flächen, die er erkennt. Diese Informationen kann verwendet werden, um die Anzahl der Personen in einem Bild, zeichnen Indikatoren für das Abbild aus (z. b. für "Tags" Personen in einem Foto), oder etwas anderes können Sie sich vorstellen.

Diesen Code aus CoreImage\SampleCode.cs veranschaulicht das Erstellen und Verwenden von Gesicht Erkennung auf ein eingebettetes Bild:

```csharp
var image = new UIImage("photoFace.JPG");
var context = CIContext.FromOptions(null);
var detector = CIDetector.CreateFaceDetector (context, true);
var ciImage = CIImage.FromCGImage(image.CGImage);
CIFeature[] features = detector.FeaturesInImage(ciImage);
```

Wird das Array von Funktionen mit aufgefüllt `CIFaceFeature` Objekte (wenn alle Flächen erkannt wurden). Es ist ein `CIFaceFeature` für jede Fläche. `CIFaceFeature` hat die folgenden Eigenschaften:

-  HasMouthPosition – gibt an, ob ein Mund für diese Fläche erkannt wurde.
-  HasLeftEyePosition – gibt an, ob das linke Auge für diese Fläche erkannt wurde.
-  HasRightEyePosition – gibt an, ob das richtige Auge für diese Fläche erkannt wurde. 
-  MouthPosition – die Koordinaten der den Mund für diese Fläche.
-  LeftEyePosition – die Koordinaten der linken Auge für diese Fläche.
-  RightEyePosition – die Koordinaten des rechten Auges für diese Fläche.


Die Koordinaten für all diese Eigenschaften haben ihren Ursprung in der unteren linken Ecke – im Gegensatz zu UIKit der linken oberen Ecke als Ursprung verwendet. Bei Verwendung der Koordinaten für `CIFaceFeature` Achten Sie darauf, dass Sie diese "blättern". Diese Ansicht sehr einfach benutzerdefiniertes Image in CoreImage\CoreImageViewController.cs veranschaulicht, wie "Gesicht" Indikatordreiecke auf das Bild gezeichnet werden soll (Beachten Sie die `FlipForBottomOrigin` Methode):

```csharp
public class FaceDetectImageView : UIView
{
    public Xamarin.iOS.CoreImage.CIFeature[] Features;
    public UIImage Image;
    public FaceDetectImageView (RectangleF rect) : base(rect) {}
    CGPath path;
    public override void Draw (RectangleF rect) {
        base.Draw (rect);
        if (Image != null)
            Image.Draw(rect);

        using (var context = UIGraphics.GetCurrentContext()) {
            context.SetLineWidth(4);
            UIColor.Red.SetStroke ();
            UIColor.Clear.SetFill ();
            if (Features != null) {
                foreach (var feature in Features) { // for each face
                    var facefeature = (CIFaceFeature)feature;
                    path = new CGPath ();
                    path.AddLines(new PointF[]{ // assumes all 3 features found
                        FlipForBottomOrigin(facefeature.LeftEyePosition, 200),
                        FlipForBottomOrigin(facefeature.RightEyePosition, 200),
                        FlipForBottomOrigin(facefeature.MouthPosition, 200)
                    });
                    path.CloseSubpath();
                    context.AddPath(path);
                    context.DrawPath(CGPathDrawingMode.FillStroke);
                }
            }
        }
    }
    /// <summary>
    /// Face recognition coordinates have their origin in the bottom-left
    /// but we are drawing with the origin in the top-left, so "flip" the point
    /// </summary>
    PointF FlipForBottomOrigin (PointF point, int height)
    {
        return new PointF(point.X, height - point.Y);
    }
}
```

Anschließend werden in der Datei SampleCode.cs dem Bild und Funktionen zugewiesen, bevor das Bild neu gezeichnet wird:

```csharp
faceView.Image = image;
faceView.Features = features;
faceView.SetNeedsDisplay();
```

Der Screenshot zeigt die Ausgabe des Beispiels: die Speicherorte der erkannten gesichtsmerkmale werden in einer UITextView angezeigt und dem Quell-Bild mit CoreGraphics gezeichnet.

Aufgrund der Art und Weise gesichtserkennung Funktionsweise erkennt gelegentlich Dinge neben menschlichen Flächen (z. B. diese Toy Affen!).

## <a name="filters"></a>Filter

Es gibt mehr als 50 unterschiedliche integrierte Filter, und das Framework ist erweiterbar, sodass neue Filter implementiert werden können.

## <a name="using-filters"></a>Mithilfe von Filtern

Anwenden eines Filters auf ein Bild sind vier verschiedene Schritte erforderlich: Laden Sie das Bild, erstellen Sie den Filter, der Filter angewendet und speichern (oder Anzeigen von) das Ergebnis.

Laden Sie zunächst ein Bild in einem `CIImage` Objekt.

```csharp
var uiimage = UIImage.FromFile ("photo.JPG");
var ciimage = new CIImage (uiimage);
```

Zweitens erstellen Sie die Filterklasse, und legen Sie dessen Eigenschaften.

```csharp
var sepia = new CISepiaTone();
sepia.Image = ciimage;
sepia.Intensity = 0.8f;
```

Im dritten Zugriff auf die `OutputImage` -Eigenschaft, und rufen die `CreateCGImage` Methode, um das Endergebnis zu rendern.

```csharp
CIImage output = sepia.OutputImage;
var context = CIContext.FromOptions(null);
var cgimage = context.CreateCGImage (output, output.Extent);
```

Weisen Sie schließlich das Bild in eine Ansicht, um das Resultset anzuzeigen. In einer realen Anwendung könnte das resultierende Image, das Dateisystem, Albums, einen Tweet oder e-Mail-gespeichert werden.

```csharp
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

Diese Screenshots zeigen das Ergebnis der `CISepia` und `CIHueAdjust` Beispielcode für Filter, die in der CoreImage.zip veranschaulicht werden.

Finden Sie unter der [Vertrag anpassen und Helligkeit eine Bild-Rezept](https://developer.xamarin.com/recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image) ein Beispiel für die `CIColorControls` Filter.

```csharp
var uiimage = UIImage.FromFile("photo.JPG");
var ciimage = new CIImage(uiimage);
var hueAdjust = new CIHueAdjust();   // first filter
hueAdjust.Image = ciimage;
hueAdjust.Angle = 2.094f;
var sepia = new CISepiaTone();       // second filter
sepia.Image = hueAdjust.OutputImage; // output from last filter, input to this one
sepia.Intensity = 0.3f;
CIFilter color = new CIColorControls() { // third filter
    Saturation = 2,
    Brightness = 1,
    Contrast = 3,
    Image = sepia.OutputImage    // output from last filter, input to this one
};
var output = color.OutputImage;
var context = CIContext.FromOptions(null);
// ONLY when CreateCGImage is called do all the effects get rendered
var cgimage = context.CreateCGImage (output, output.Extent);
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

```csharp
var context = CIContext.FromOptions (null);
```

```csharp
var context = CIContext.FromOptions(new CIContextOptions() {
    UseSoftwareRenderer = true  // CPU
});
var cgimage = context.CreateCGImage (output, output.Extent);
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

### <a name="listing-filters-and-their-properties"></a>Auflisten von Filter und deren Eigenschaften

Dieser Code von CoreImage\SampleCode.cs gibt die vollständige Liste der integrierten Filter und die zugehörigen Parameter.

```csharp
var filters = CIFilter.FilterNamesInCategories(new string[0]);
foreach (var filter in filters){
   display.Text += filter +"\n";
   var f = CIFilter.FromName (filter);
   foreach (var key in f.InputKeys){
     var attributes = (NSDictionary)f.Attributes[new NSString(key)];
     var attributeClass = attributes[new NSString("CIAttributeClass")];
     display.Text += "   " + key;
     display.Text += "   " + attributeClass + "\n";
   }
}
```

Die [CIFilter-Klassenreferenz](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.html) beschreibt den 50 integrierten Filter und deren Eigenschaften. Mit dem Code weiter oben aufgeführten Fragen die Filterklassen, einschließlich der Standardwerte für Parameter und die maximale und minimale zulässige Werte (die Eingaben zu überprüfen, bevor das Anwenden eines Filters verwendet werden können).

Die Kategorien auflisten Ausgabe sieht wie folgt im Simulator – Sie können Blättern Sie in der Liste, um alle Filter und die zugehörigen Parameter finden Sie unter.

 [![](introduction-to-coreimage-images/coreimage05.png "Die Kategorien auflisten Ausgabe sieht wie folgt im Simulator")](introduction-to-coreimage-images/coreimage05.png#lightbox)

Jeden aufgelisteten Filter wurde als Klasse in Xamarin.iOS, ausgesetzt, sodass Sie auch die Xamarin.iOS.CoreImage-API in der Assembly-Browser oder mithilfe der AutoVervollständigen in Visual Studio für Mac oder Visual Studio untersuchen können. 

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde wie einige der neuen Ios5 Core-Image-Framework-Funktionen wie Gesicht Erkennung und Anwenden von Filtern auf ein Bild verwenden wird. Es gibt Dutzende von anderen Bild-Filter in das Framework für die Verwendung verfügbar.

## <a name="related-links"></a>Verwandte Links

- [Core-Image (Beispiel)](https://developer.xamarin.com/samples/CoreImage/)
- [Vertrag und Helligkeit eine Rezept Bild anpassen](https://developer.xamarin.com/recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image)
- [Mithilfe von Filtern für Core-Image](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Conceptual/CoreImaging/ci_tasks/ci_tasks.html)
- [CIFilter-Klassenreferenz](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.htm)
