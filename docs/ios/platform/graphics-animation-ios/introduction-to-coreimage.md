---
title: Core-Image in xamarin. IOS
description: Das Core-Image ist ein neues Framework, das mit IOS 5 eingeführt wurde, um die Abbild Verarbeitung und die Funktion für die Video Verbesserung In diesem Artikel werden diese Features mit xamarin. IOS-Beispielen vorgestellt.
ms.prod: xamarin
ms.assetid: 91E0780B-FF8A-E70D-9CD4-419119612B2D
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/19/2017
ms.openlocfilehash: a537926ab28bc355af5c5c4993ccff4a736b15aa
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70288576"
---
# <a name="core-image-in-xamarinios"></a>Core-Image in xamarin. IOS

_Das Core-Image ist ein neues Framework, das mit IOS 5 eingeführt wurde, um die Abbild Verarbeitung und die Funktion für die Video Verbesserung In diesem Artikel werden diese Features mit xamarin. IOS-Beispielen vorgestellt._

Das Core-Image ist ein neues in ios 5 eingeführte Framework, das eine Reihe integrierter Filter und Effekte bereitstellt, die auf Bilder und Videos, einschließlich Gesichtserkennung, angewendet werden können.

Dieses Dokument enthält einfache Beispiele für:

- Gesichtserkennung.
- Anwenden von Filtern auf ein Bild
- Auflisten der verfügbaren Filter.


Diese Beispiele sollen Ihnen beim Einstieg in Ihre xamarin. IOS-Anwendungen helfen.

## <a name="requirements"></a>Anforderungen

Sie müssen die neueste Version von Xcode verwenden.

## <a name="face-detection"></a>Gesichtserkennung

Das Feature "Core Image Face Erkennungs" führt genau aus, was es sagt – es versucht, Gesichter in einem Foto zu identifizieren, und gibt die Koordinaten aller Gesichter zurück, die es erkennt. Diese Informationen können verwendet werden, um die Anzahl der Personen in einem Bild zu zählen und Indikatoren für das Bild zu zeichnen (z. b. zum Markieren von Personen in einem Foto) oder etwas anderes, was Sie sich vorstellen können.

Dieser Code aus coreimage\samplecode.cs veranschaulicht, wie Sie die Gesichtserkennung für ein eingebettetes Bild erstellen und verwenden:

```csharp
var image = new UIImage("photoFace.JPG");
var context = CIContext.FromOptions(null);
var detector = CIDetector.CreateFaceDetector (context, true);
var ciImage = CIImage.FromCGImage(image.CGImage);
CIFeature[] features = detector.FeaturesInImage(ciImage);
```

Das featurearray wird mit `CIFaceFeature` -Objekten aufgefüllt (sofern irgendwelche Gesichter erkannt wurden). Es gibt eine `CIFaceFeature` für jede Seite. `CIFaceFeature`verfügt über die folgenden Eigenschaften:

- Hasmundposition – gibt an, ob ein Mund für dieses Gesicht erkannt wurde.
- Haslefteyeposition – gibt an, ob das linke Auge für dieses Gesicht erkannt wurde.
- Hasrechtschaffyeposition – gibt an, ob für dieses Gesicht das rechte Auge erkannt wurde. 
- Mundposition – die Koordinaten des Mundes für dieses Gesicht.
- Lefteyeposition – die Koordinaten des linken Auges für dieses Gesicht.
- Rechtschaffyeposition – die Koordinaten des rechten Auges für dieses Gesicht.


Die Koordinaten für alle diese Eigenschaften haben den Ursprung unten links – im Gegensatz zu UIKit, bei dem die linke obere Seite als Ursprung verwendet wird. Wenn Sie die Koordinaten in `CIFaceFeature` verwenden, sollten Sie Sie "Kippen". Diese sehr grundlegende benutzerdefinierte Bildansicht in coreimage\coreimageviewcontroller.cs veranschaulicht, wie Sie "Gesicht Indikator"-Dreiecke im Bild `FlipForBottomOrigin` zeichnen (Beachten Sie die-Methode):

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

Anschließend werden in der SampleCode.cs-Datei das Image und die Features zugewiesen, bevor das Bild neu gezeichnet wird:

```csharp
faceView.Image = image;
faceView.Features = features;
faceView.SetNeedsDisplay();
```

Der Screenshot zeigt die Beispielausgabe: die Speicherorte der erkannten Gesichts Merkmale werden in einer uitextview angezeigt und mithilfe von CoreGraphics auf das Quell Bild gezeichnet.

Aufgrund der Art und Weise, wie die Gesichtserkennung funktioniert, erkennt Sie gelegentlich Dinge neben menschlichen Gesichtern (z. b. diese Spiel Affen!).

## <a name="filters"></a>Filter

Es gibt mehr als 50 verschiedene integrierte Filter, und das Framework ist erweiterbar, sodass neue Filter implementiert werden können.

## <a name="using-filters"></a>Verwenden von Filtern

Das Anwenden eines Filters auf ein Image umfasst vier verschiedene Schritte: Laden des Bilds, Erstellen des Filters, Anwenden des Filters und speichern (oder anzeigen) des Ergebnisses.

Laden Sie zunächst ein Bild in ein `CIImage` -Objekt.

```csharp
var uiimage = UIImage.FromFile ("photo.JPG");
var ciimage = new CIImage (uiimage);
```

Erstellen Sie anschließend die Filterklasse, und legen Sie deren Eigenschaften fest.

```csharp
var sepia = new CISepiaTone();
sepia.Image = ciimage;
sepia.Intensity = 0.8f;
```

Drittens greifen Sie auf `OutputImage` die-Eigenschaft zu `CreateCGImage` , und wenden Sie die-Methode zum Rendering des Endergebnisses an.

```csharp
CIImage output = sepia.OutputImage;
var context = CIContext.FromOptions(null);
var cgimage = context.CreateCGImage (output, output.Extent);
```

Weisen Sie schließlich das Bild einer Ansicht zu, um das Ergebnis anzuzeigen. In einer realen Anwendung kann das resultierende Image im Dateisystem, im Fotoalbum, in einem Tweet oder in einer e-Mail gespeichert werden.

```csharp
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

Diese Screenshots zeigen das Ergebnis der `CISepia` Filter und `CIHueAdjust` , die im Beispielcode CoreImage. zip veranschaulicht werden.

Ein Beispiel für den `CIColorControls` Filter finden Sie unter [Anpassen des Vertrags und der Helligkeit eines Bild Rezept](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image) .

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

Dieser Code von coreimage\samplecode.cs gibt eine komplette Liste integrierter Filter und ihrer Parameter aus.

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

Der [cifilter-Klassen Verweis](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.html) beschreibt die integrierten 50-Filter und deren Eigenschaften. Mit dem obigen Code können Sie die Filterklassen Abfragen, einschließlich der Standardwerte für Parameter und der maximalen und minimal zulässigen Werte (die zum Validieren von Eingaben vor dem Anwenden eines Filters verwendet werden könnten).

Die Ausgabe der Listen Kategorien sieht im Simulator wie folgt aus – Sie können einen Bildlauf durch die Liste durchführen, um alle Filter und deren Parameter anzuzeigen.

 [![](introduction-to-coreimage-images/coreimage05.png "Die Ausgabe der Listen Kategorien sieht im Simulator wie folgt aus.")](introduction-to-coreimage-images/coreimage05.png#lightbox)

Jeder aufgeführte Filter wurde als Klasse in xamarin. IOS verfügbar gemacht, sodass Sie auch die xamarin. IOS. CoreImage-API im assemblybrowser oder die automatische Vervollständigung in Visual Studio für Mac oder Visual Studio untersuchen können. 

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde gezeigt, wie Sie einige der neuen Features für den IOS 5-Kern Image-Frameworks wie Gesichtserkennung und Anwenden von Filtern auf ein Abbild verwenden können. Im Framework sind Dutzende verschiedener Bild Filter verfügbar, die Sie verwenden können.

## <a name="related-links"></a>Verwandte Links

- [Core-Image (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/coreimage)
- [Anpassen des Vertrags und der Helligkeit eines Bild Rezept](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image)
- [Verwenden von Core-Bild Filtern](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Conceptual/CoreImaging/ci_tasks/ci_tasks.html)
- [Cifilter-Klassen Verweis](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.htm)
