---
title: Core-Image in Xamarin.iOS
description: Core-Image ist ein neues Framework mit iOS 5, bildverarbeitung und live-video-Erweiterung Funktionen eingeführt. In diesem Artikel werden diese Funktionen mit Xamarin.iOS-Beispiele erläutert.
ms.prod: xamarin
ms.assetid: 91E0780B-FF8A-E70D-9CD4-419119612B2D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 7af57856079813e8cb1831a7f22a0a098a6be771
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242165"
---
# <a name="core-image-in-xamarinios"></a>Core-Image in Xamarin.iOS

_Core-Image ist ein neues Framework mit iOS 5, bildverarbeitung und live-video-Erweiterung Funktionen eingeführt. In diesem Artikel werden diese Funktionen mit Xamarin.iOS-Beispiele erläutert._

Core-Image ist ein neues Framework IOS 5, die eine Reihe von integrierten Filter und Effekte anwenden, Bilder und Videos, einschließlich der gesichtserkennung bereitstellt, eingeführt.

Dieses Dokument enthält einfache Beispiele:

-  Gesichtserkennung.
-  Anwenden von Filtern zu einem Bild
-  Eine Liste der verfügbaren Filter.


Diese Beispiele sollen Ihnen beim Einstieg helfen Core-Image-Funktionen in Ihrer Xamarin.iOS-Anwendungen zu integrieren.

## <a name="requirements"></a>Anforderungen

Sie müssen die neueste Version von Xcode verwenden.

## <a name="face-detection"></a>Gesichtserkennung

Die Core-Image gesichtserkennungs-Funktion zur Erkennung ist einfach, was es verspricht – es versucht, identifizieren Sie Gesichter auf Fotos, und gibt die Koordinaten der alle, die er erkennt Gesichter zurück. Diese Informationen kann verwendet werden, zählen die Anzahl der Personen in einem Bild, zeichnen Indikatoren auf das Bild (z. b. für "Tags" Personen in einem Foto), oder Sie können sich vorstellen.

Dieser Code von CoreImage\SampleCode.cs veranschaulicht das Erstellen und verwenden die gesichtserkennung auf ein eingebettetes Bild:

```csharp
var image = new UIImage("photoFace.JPG");
var context = CIContext.FromOptions(null);
var detector = CIDetector.CreateFaceDetector (context, true);
var ciImage = CIImage.FromCGImage(image.CGImage);
CIFeature[] features = detector.FeaturesInImage(ciImage);
```

Das Array von Funktionen mit gefüllt `CIFaceFeature` Objekte (sofern Gesichter erkannt wurden). Es gibt eine `CIFaceFeature` für jedes Gesicht. `CIFaceFeature` hat die folgenden Eigenschaften an:

-  HasMouthPosition – gibt an, ob ein Mund für diese Gesicht erkannt wurde.
-  HasLeftEyePosition – gibt an, ob das linke Auge für diese Gesicht erkannt wurde.
-  HasRightEyePosition – gibt an, ob die richtigen Eye für diese Gesicht erkannt wurde. 
-  MouthPosition – die Koordinaten der Mund für diese Gesicht.
-  LeftEyePosition – die Koordinaten der linken Auge für diese Gesicht.
-  RightEyePosition – die Koordinaten des richtigen Auges für diese Gesicht.


Die Koordinaten für all diese Eigenschaften haben ihren Ursprung in der unteren linken Ecke – im Gegensatz zu UIKit auf der linken oberen Ecke als Ursprung verwendet. Bei Verwendung von den Koordinaten auf `CIFaceFeature` Achten Sie darauf, dass Sie diese "flip". Diese Ansicht sehr einfachen benutzerdefinierten Images in CoreImage\CoreImageViewController.cs wird veranschaulicht, wie "gesichtserkennungs" Indikatordreiecke auf das Bild zu zeichnen (Beachten Sie die `FlipForBottomOrigin` Methode):

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

Dann werden in der Datei SampleCode.cs das Image und die Funktionen zugewiesen, bevor das Abbild neu gezeichnet wird:

```csharp
faceView.Image = image;
faceView.Features = features;
faceView.SetNeedsDisplay();
```

Der Screenshot zeigt die Ausgabe des Beispiels: die Standorte von der erkannten gesichtsmerkmale sind in einem UITextView angezeigt und auf das Quellimage mithilfe CoreGraphics gezeichnet.

Aufgrund der Art und Weise gesichtserkennung Funktionsweise erkennt gelegentlich Punkte neben der menschliche Gesichter (z. B. folgenden Toy Affen!).

## <a name="filters"></a>Filter

Es gibt mehr als 50 verschiedenen integrierten Filter, und das Framework ist erweiterbar, sodass neue Filter implementiert werden können.

## <a name="using-filters"></a>Mithilfe von Filtern

Anwenden eines Filters zu einem Bild besteht aus vier unterschiedliche Schritten: Laden Sie das Image, erstellen Sie den Filter, der Filter angewendet und gespeichert (oder anzeigen) das Ergebnis.

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

Drittens: Zugriff auf die `OutputImage` -Eigenschaft, und rufen die `CreateCGImage` Methode, um das endgültige Ergebnis zu rendern.

```csharp
CIImage output = sepia.OutputImage;
var context = CIContext.FromOptions(null);
var cgimage = context.CreateCGImage (output, output.Extent);
```

Weisen Sie das Image schließlich zu einer Ansicht, um das Ergebnis anzuzeigen. In einer echten Anwendung könnte das resultierende Image im Dateisystem, das Fotoalbum, ein Tweet oder e-Mail-Adresse gespeichert werden.

```csharp
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

Diese Screenshots zeigen das Ergebnis der `CISepia` und `CIHueAdjust` Beispielcode für Filter, die in der CoreImage.zip veranschaulicht werden.

Finden Sie unter den [Vertrag anpassen und Helligkeit eine Image-Rezept](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image) ein Beispiel für die `CIColorControls` Filter.

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

### <a name="listing-filters-and-their-properties"></a>Auflisten von Filtern und deren Eigenschaften

Dieser Code von CoreImage\SampleCode.cs gibt die vollständige Liste der integrierten Filter und ihren Parametern.

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

Die [CIFilter Class Reference](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.html) beschreibt den 50 integrierten Filter und deren Eigenschaften. Mit dem Code oben Sie können die Filterklassen, einschließlich der Standardwerte für Parameter und der maximalen und minimalen zulässigen Werte (die Eingaben überprüft vor dem Anwenden eines Filters verwendet werden können) Abfragen.

Die Liste von Kategorien Ausgabe sieht wie folgt aus, auf dem Simulator – Sie können scrollen, durch die Liste aus, um alle Filter und ihren Parametern finden Sie unter.

 [![](introduction-to-coreimage-images/coreimage05.png "Die Kategorien auflisten Ausgabe sieht wie folgt auf dem Simulator aus")](introduction-to-coreimage-images/coreimage05.png#lightbox)

Jeder Filter aufgeführt wurde als Klasse in Xamarin.iOS, ausgesetzt, sodass Sie auch die Xamarin.iOS.CoreImage-API in der Assembly-Browser oder mithilfe von AutoVervollständigen in Visual Studio für Mac oder Visual Studio untersuchen können. 

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde wie einige der neuen iOS 5 Core-Image-Framework-Features wie gesichtserkennung und Anwenden von Filtern auf ein Bild verwenden wird. Es gibt Dutzende von verschiedenen Bildfilter in das Framework für die Verwendung verfügbar.

## <a name="related-links"></a>Verwandte Links

- [Core-Image (Beispiel)](https://developer.xamarin.com/samples/CoreImage/)
- [Passen Sie Vertrag und Helligkeit eine Image-Anleitung](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image)
- [Core-Image-Filter verwenden](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Conceptual/CoreImaging/ci_tasks/ci_tasks.html)
- [CIFilter-Klassenreferenz](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.htm)
