---
title: Core-Grafiken
description: Dieser Artikel beschreibt die Core-Grafiken iOS-Frameworks. Es wird gezeigt, wie mit Graphics Core um Geometry, Bilder und PDF-Dateien zu zeichnen.
ms.topic: article
ms.prod: xamarin
ms.assetid: 4A30F480-0723-4B8A-9049-7CEB6211304A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: fe1796839524a271760a9beb82895fd6e93c7ad0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="core-graphics"></a>Core-Grafiken

_Dieser Artikel beschreibt die Core-Grafiken iOS-Frameworks. Es wird gezeigt, wie mit Graphics Core um Geometry, Bilder und PDF-Dateien zu zeichnen._

iOS umfasst die [ *Graphics Core* ](https://developer.apple.com/library/prerelease/ios/documentation/CoreGraphics/Reference/CoreGraphics_Framework/index.html) Framework auf niedriger Ebene zeichnen unterstützen. Diese Frameworks sind, was die umfassenden grafische Funktionen innerhalb UIKit zu aktivieren. 

Core-Grafiken ist ein auf niedriger Ebene 2D Graphics-Framework, mit der Zeichnung geräteunabhängige Grafiken. Alle 2D Zeichnen in UIKit verwendet intern Core Grafiken.

Core-Grafiken unterstützt Zeichnen in eine Reihe von Szenarien, einschließlich:

-  [Zeichnen auf dem Bildschirm über einen `UIView` ](#Drawing_in_a_UIView_Subclass) .
-  [Darstellen von Bildern im Arbeitsspeicher oder auf dem Bildschirm](#Drawing_Images_and_Text).
-  Erstellen und Zeichnen in einer PDF-Datei.
-  Lesen und zeichnen eine vorhandene PDF-Datei.


## <a name="geometric-space"></a>Geometrische Speicherplatz

Unabhängig von dem Szenario alle Zeichnungen mit Graphics Core ausgeführt erfolgt in geometrische Speicherplatz, was bedeutet, dass die Funktionsweise in abstrakten Punkte statt in Pixel. Sie beschreiben, was gezeichneten hinsichtlich der Geometrie und Status, z. B. Farben, Linienarten usw. zeichnen soll, und Core Grafiken behandelt übersetzen alles in Pixel. Dieser Status wird ein grafikontext hinzugefügt, die Sie z. B. eine übertragen Canvas vorstellen können.

Es gibt einige Vorteile dieses Ansatzes:

-  Zeichnen von Code wird die dynamische und kann anschließend Grafiken zur Laufzeit ändern.
-  Mindert die Notwendigkeit für statische Bilder im Anwendungspaket kann Anwendungsgröße reduzieren.
-  Grafiken werden auf vielen Geräten stabiler zu Änderungen der Bildschirmauflösung.


## <a name="drawing-in-a-uiview-subclass"></a>Zeichnen in einer Unterklasse UIView

Jede `UIView` verfügt über eine `Draw` Methode, die vom System aufgerufen wird, wenn diese gezeichnet werden muss. Zum Hinzufügen von Code zum Zeichnen einer Ansicht Unterklasse `UIView` und überschreiben `Draw`:

```csharp
public class TriangleView : UIView
{
    public override void Draw (CGRect rect)
    {
        base.Draw (rect);
    }
}
```

Zeichnen-Befehl sollte nie direkt aufgerufen werden. Er wird während der Ausführung der Schleife Verarbeitung vom System aufgerufen. Die erste Schleifendurchlauf ausführen, nachdem die Hierarchie anzeigen eine Sicht hinzugefügt wird seine `Draw` -Methode aufgerufen wird. Nachfolgende Aufrufe `Draw` auftreten, wenn die Sicht gekennzeichnet ist, gezeichnet werden soll, durch den Aufruf eines Kennzeichnung angegeben, dass `SetNeedsDisplay` oder `SetNeedsDisplayInRect` für die Sicht.

### <a name="pattern-for-graphics-code"></a>Muster für den Grafik-Code

Der Code in der `Draw` Implementierung sollte beschreiben, was sie gezeichneten benötigt. Der Code zum Zeichnen folgt einem Muster, in dem legt einige zeichnen Zustand und ruft eine Methode, um anzufordern, es gezeichnet werden. Dieses Muster kann wie folgt verallgemeinert werden:

1. Abgerufen Sie ein grafikontext werden.

2. Richten Sie Zeichnungsattribute.

3. Erstellen Sie einige Geometry primitive zeichnen.

4. Rufen Sie eine Zeichnen-Befehl oder Striche-Methode.

### <a name="basic-drawing-example"></a>Beispiel für die grundlegenden Funktionen zum Zeichnen

Betrachten Sie beispielsweise den folgenden Codeausschnitt an:

```csharp
//get graphics context
using (CGContext g = UIGraphics.GetCurrentContext ()) {
            
    //set up drawing attributes
    g.SetLineWidth (10);
    UIColor.Blue.SetFill ();
    UIColor.Red.SetStroke ();

    //create geometry
    var path = new CGPath ();

    path.AddLines (new CGPoint[]{
    new CGPoint (100, 200),
    new CGPoint (160, 100), 
    new CGPoint (220, 200)});

    path.CloseSubpath ();

    //add geometry to graphics context and draw it
    g.AddPath (path);       
    g.DrawPath (CGPathDrawingMode.FillStroke);
}
```

Wir aufgliedern mit diesem Code:

```csharp
using (CGContext g = UIGraphics.GetCurrentContext ()) {
...
}
```
Durch diese Zeile ruft er zuerst dem aktuellen Grafikkontext zum Zeichnen verwendete ab. Sie können ein grafikontext als im Zeichenbereich vorstellen, dass die Zeichnung, erfolgt, enthält alle Status zu zeichnen, z. B. Strich und Füllfarben sowie die Geometrie gezeichnet werden soll.

```csharp
g.SetLineWidth (10);
UIColor.Blue.SetFill ();
UIColor.Red.SetStroke ();
``` 

Nach Eingang eines Grafikkontexts richtet der Code einige Attribute zu verwendende zeichnen, oben gezeigt. In diesem Fall werden die Linienfarben Breite, Strich und Füllung festgelegt. Alle nachfolgenden Zeichnen verwendet dann diese Attribute, da sie in den Grafikkontext Zustand verwaltet werden.

So erstellen Sie Geometrie im Code verwendet eine `CGPath`, sodass einen Grafikpfad aus Linien und Kurven beschrieben werden. In diesem Fall fügt den Pfad Verbindungslinien ein Array von Punkten, die zum Bilden eines Dreiecks. Wie unten Graphics Core verwendet ein Koordinatensystem für Ansicht zeichnen angezeigt, ist der Ursprung, in dem in der oberen linken Ecke mit positiven X (direkt) nach rechts und Positive y-Richtung nach unten:

```csharp
var path = new CGPath ();

path.AddLines (new CGPoint[]{
new CGPoint (100, 200),
new CGPoint (160, 100), 
new CGPoint (220, 200)});

path.CloseSubpath ();
``` 

Nachdem der Pfad erstellt wird, es ist hinzugefügt der Grafikkontext, damit Aufrufen `AddPath` und `DrawPath` bzw. zeichnen kann es.

Die Ansicht wird unten gezeigt:

 ![](core-graphics-images/00-bluetriangle.png "Das Beispiel Ausgabe Dreieck")

## <a name="creating-gradient-fills"></a>Erstellen von Farbverläufen

Umfangreichere Formen der Zeichnung sind auch verfügbar. Beispielsweise ermöglicht Graphics Core Farbverläufe erstellen und Anwenden von Freistellungspfade. Um eine graduelle Füllung innerhalb des Pfads aus dem vorherigen Beispiel zu zeichnen, muss zunächst der Pfad als des Freistellungspfads festgelegt werden:

```csharp
// add the path back to the graphics context so that it is the current path
g.AddPath (path);
// set the current path to be the clipping path
g.Clip ();
```

Festlegen des Freistellungspfads alle nachfolgenden Zeichnen innerhalb der Geometrie des Pfads, z. B. der folgende Code schränkt den aktuellen Pfad zeichnet die einen linearen Farbverlauf:

```csharp
// the color space determines how Core Graphics interprets color information
    using (CGColorSpace rgb = CGColorSpace.CreateDeviceRGB()) {
        CGGradient gradient = new CGGradient (rgb, new CGColor[] {
        UIColor.Blue.CGColor,
        UIColor.Yellow.CGColor
    });

// draw a linear gradient
    g.DrawLinearGradient (
        gradient, 
        new CGPoint (path.BoundingBox.Left, path.BoundingBox.Top), 
        new CGPoint (path.BoundingBox.Right, path.BoundingBox.Bottom), 
        CGGradientDrawingOptions.DrawsBeforeStartLocation);
    }
```

Diese Änderungen erzeugt einen Farbverlauf an, wie unten dargestellt:

 ![](core-graphics-images/01-gradient-fill.png "Im Beispiel mit gradueller Füllung")

## <a name="modifying-line-patterns"></a>Ändern von Linien

Die Zeichnungsattribute Zeilen können auch mit Graphics Core geändert werden. Dies schließt das ändern, die Linienfarbe für Breite und Strich als auch das Muster der Linie selbst, wie im folgenden Code dargestellt:

```csharp
//use a dashed line
g.SetLineDash (0, new nfloat[] { 10, 4 * (nfloat)Math.PI });
```

Dieser Code vor dem Zeichnen Vorgänge Ergebnisse hinzufügen in gestrichelten Striche 10 Einheiten mit 4 Einheiten des Abstands zwischen Bindestriche, long, wie unten dargestellt:

 ![](core-graphics-images/02-dashed-stroke.png "Dieser Code vor dem Zeichnen Operations Ergebnisse hinzufügen gestrichelte Konturen")
 
Beachten Sie, dass bei Verwendung der einheitliche API in Xamarin.iOS der Arraytyp benötigt, ein `nfloat`, und auch explizit in Math.PI umgewandelt werden muss.

## <a name="drawing-images-and-text"></a>Zeichnen von Bildern und Text

Zusätzlich zum Zeichnen von Pfaden in einer Ansicht Grafikkontext unterstützt Core Grafiken auch Bilder und Text. Um ein Bild gezeichnet werden soll, erstellen Sie einfach eine `CGImage` und übergeben sie an einer `DrawImage` aufrufen:

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);

    using(CGContext g = UIGraphics.GetCurrentContext ()){
        g.DrawImage (rect, UIImage.FromFile ("MyImage.png").CGImage);
    }
}
```

Allerdings ergibt dies ein Bild gezeichnet stehend, wie unten dargestellt:

 ![](core-graphics-images/03-upside-down-monkey.png "Ein Bild gezeichnet stehend")

Der Grund dafür ist Core Grafiken Ursprung zum Zeichnen des Bilds in der unteren linken Ecke, während die Ansicht in der oberen linken Ecke Ursprungssite verfügt. Aus diesem Grund, um das Bild ordnungsgemäß angezeigt wird, der Ursprung muss geändert werden, kann durch Ändern von erreicht werden die *aktuellen Transformationsmatrix* *(CTM)*. Die CTM definiert, in dem Punkt, auch bekannt als Leben *Benutzerspeicherplatz*. Invertieren der CTM entlang der y-Achse an, und verschieben sie durch die Grenzen Höhe in negative y-Richtung können das Bild gekippt.

Der Grafikkontext hat Hilfsmethoden zum Transformieren der CTM. In diesem Fall `ScaleCTM` "spiegelt" die Zeichnung und `TranslateCTM` an der oberen linken Ecke verlagert, wie unten dargestellt:

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);
    
    using (CGContext g = UIGraphics.GetCurrentContext ()) {

        // scale and translate the CTM so the image appears upright
        g.ScaleCTM (1, -1);
        g.TranslateCTM (0, -Bounds.Height);
        g.DrawImage (rect, UIImage.FromFile ("MyImage.png").CGImage);
}   
```

Wird das resultierende Image angezeigt Schriften aufrecht dargestellt:

 ![](core-graphics-images/04-upright-monkey.png "Das Beispiel angezeigte Bild aufrechten")

> [!IMPORTANT]
>  **Hinweis:** Änderungen der Grafikkontext für alle nachfolgenden zeichnen Vorgänge angewendet werden sollen. Aus diesem Grund wirkt die CTM transformiert wird, zusätzliche Zeichnung sich. Beispielsweise, wenn Sie das Dreieck nach der Transformation CTM gezeichnet wurde, würde es stehend angezeigt.

### <a name="adding-text-to-the-image"></a>Hinzufügen von Text zu dem Bild

Als umfasst Zeichnen von Text mit Graphics Core mit Pfade und Bilder, die gleiche Basismuster einige Grafikzustand und Aufrufen einer Methode gezeichnet werden soll. Im Fall von Text, wird die Methode zum Anzeigen von Text `ShowText`. Wenn auf das Abbild zeichnen Beispiel hinzugefügt wird, zeichnet der folgende Code mit Graphics Core Text:

```csharp
public override void Draw (RectangleF rect)
{
    base.Draw (rect);
    
    // image drawing code omitted for brevity ...

    // translate the CTM by the font size so it displays on screen
    float fontSize = 35f;
    g.TranslateCTM (0, fontSize);

    // set general-purpose graphics state
    g.SetLineWidth (1.0f);
    g.SetStrokeColor (UIColor.Yellow.CGColor);
    g.SetFillColor (UIColor.Red.CGColor);
    g.SetShadow (new CGSize (5, 5), 0, UIColor.Blue.CGColor);

    // set text specific graphics state
    g.SetTextDrawingMode (CGTextDrawingMode.FillStroke);
    g.SelectFont ("Helvetica", fontSize, CGTextEncoding.MacRoman);

    // show the text
    g.ShowText ("Hello Core Graphics");
}
```

Wie Sie sehen können, gleicht der Status der Grafik für das Zeichnen von Text festlegen Geometry zeichnen. Jedoch Zeichnen von Text, der Text zeichnen-Modus und der Schriftart angewendet sowie. Ein Schatten ist in diesem Fall auch angewendet, obwohl das Anwenden von Tiefen funktioniert gleichermaßen für den Pfad zeichnen.

Der resultierende Text wird mit dem Bild angezeigt, wie unten dargestellt:

 ![](core-graphics-images/05-text-on-image.png "Der resultierende Text wird mit dem Bild angezeigt.")

## <a name="memory-backed-images"></a>Speicher gesicherte Bilder

Zusätzlich zu der Zeichnung Grafikkontext für eine Sicht gesichert Graphics Core unterstützt das Zeichnen von Arbeitsspeicher Bilder, auch bekannt als Zeichnen außerhalb des Bildschirms. Auf diese Weise benötigen Sie Folgendes:

-  Ein grafikontext erstellen, die durch ein im Arbeitsspeicher unterstützt wird mithilfe einer Bitmap
-  Festlegen von Zeichnen-Status und dem Ausgeben von Zeichnen-Befehle
-  Das Bild abrufen aus dem Kontext
-  Entfernen den Kontext


Im Gegensatz zu den `Draw` -Methode, sofern der Kontext durch die Sicht angegeben wird in diesem Fall erstellen Kontext auf zwei Arten:

1. Durch Aufrufen von `UIGraphics.BeginImageContext` (oder `BeginImageContextWithOptions`)

2. Durch Erstellen eines neuen `CGBitmapContextInstance`

 `CGBitmapContextInstance` ist nützlich, wenn Sie direkt mit dem Bild-Bits wie z. B. für Fälle arbeiten, in dem Sie einen benutzerdefiniertes Image Manipulation-Algorithmus verwenden. In allen anderen Fällen sollten Sie verwenden `BeginImageContext` oder `BeginImageContextWithOptions`.

Nachdem Sie einen Image Kontext haben, Hinzufügen von Zeichencode ist, genau so wie er in einem `UIView` Unterklasse. Beispielsweise kann das Codebeispiel oben verwendet, um ein Dreieck gezeichnet verwendet werden, auf ein Bild im Arbeitsspeicher nicht in dem gezeichnet werden soll. eine `UIView`, wie unten dargestellt:

```csharp
UIImage DrawTriangle ()
{
    UIImage triangleImage;

    //push a memory backed bitmap context on the context stack
    UIGraphics.BeginImageContext (new CGSize (200.0f, 200.0f));

    //get graphics context
    using(CGContext g = UIGraphics.GetCurrentContext ()){

        //set up drawing attributes
        g.SetLineWidth(4);
        UIColor.Purple.SetFill ();
        UIColor.Black.SetStroke ();

        //create geometry
        path = new CGPath ();

        path.AddLines(new CGPoint[]{
            new CGPoint(100,200),
            new CGPoint(160,100), 
            new CGPoint(220,200)});

        path.CloseSubpath();

        //add geometry to graphics context and draw it
        g.AddPath(path);
        g.DrawPath(CGPathDrawingMode.FillStroke);

        //get a UIImage from the context
        triangleImage = UIGraphics.GetImageFromCurrentImageContext ();
    }
    
    return triangleImage;
}
```

Eine übliche Verwendung dieser Zeichnen einer Bitmap Speicher gesicherte wird zum Erfassen eines Abbilds von einem beliebigen `UIView`. Z. B. der folgende Code rendert eine Ansichtsebene an einen Kontext für die Bitmap und erstellt eine `UIImage` aus:

```csharp
UIGraphics.BeginImageContext (cellView.Frame.Size);

//render the view's layer in the current context
anyView.Layer.RenderInContext (UIGraphics.GetCurrentContext ());

//get a UIImage from the context
UIImage anyViewImage = UIGraphics.GetImageFromCurrentImageContext ();
UIGraphics.EndImageContext ();
```

## <a name="drawing-pdfs"></a>Zeichnen von PDF-Dateien

Zusätzlich zu den Bildern unterstützt Graphics Core PDF-Zeichnung. Wie Bilder, Sie können eine PDF-Datei im Arbeitsspeicher zu rendern sowie zum Rendern in das PDF-Datei lesen eine `UIView`.

### <a name="pdf-in-a-uiview"></a>PDF-Datei in eine UIView

Core-Grafiken unterstützt auch das Lesen von PDF-Datei aus einer Datei und Rendern es in einer Ansicht mit der `CGPDFDocument` Klasse. Die `CGPDFDocument` Klasse stellt eine PDF-Datei im Code und zum Lesen und Seiten Zeichnen verwendet werden können.

Im folgenden code wird beispielsweise einer `UIView` Unterklasse liest eine PDF-Datei aus einer Datei in eine `CGPDFDocument`:

```csharp
public class PDFView : UIView
{
    CGPDFDocument pdfDoc;

    public PDFView ()
    {
        //create a CGPDFDocument from file.pdf included in the main bundle
        pdfDoc = CGPDFDocument.FromFile ("file.pdf");
    }
  
     public override void Draw (Rectangle rect)
    {
        ...
    }
}
```

Die `Draw` Methode können Sie dann die `CGPDFDocument` zum Lesen einer Seite in `CGPDFPage` und durch den Aufruf zu rendern `DrawPDFPage`, wie unten dargestellt:

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);
        
    //flip the CTM so the PDF will be drawn upright
    using (CGContext g = UIGraphics.GetCurrentContext ()) {
        g.TranslateCTM (0, Bounds.Height);
        g.ScaleCTM (1, -1);
        
        // render the first page of the PDF
        using (CGPDFPage pdfPage = pdfDoc.GetPage (1)) {
            
        //get the affine transform that defines where the PDF is drawn
        CGAffineTransform t = pdfPage.GetDrawingTransform (CGPDFBox.Crop, rect, 0, true);
        
        //concatenate the pdf transform with the CTM for display in the view
        g.ConcatCTM (t);
        
        //draw the pdf page
        g.DrawPDFPage (pdfPage);
        }
    }
}
```

### <a name="memory-backed-pdf"></a>Memory-Backed PDF

Für eine in-Memory-PDF, müssen Sie ein PDF-Kontext erstellt werden, durch den Aufruf `BeginPDFContext`. Zeichnung nach PDF ist präzise zu Seiten. Jeder Seite wird durch den Aufruf gestartet `BeginPDFPage` und durch den Aufruf abgeschlossen `EndPDFContent`, Grafiken, die den code in der Zwischenzeit. Ebenso wie mit Image-Zeichnung Arbeitsspeicher PDF Zeichnen verwendet einen Ursprung in der unteren linken, die berücksichtigt werden können unterstützt, indem Sie die CTM einfach ändern wie mit Bildern.

Der folgende Code zeigt das Zeichnen von Text in eine PDF-Datei:

```csharp
//data buffer to hold the PDF
NSMutableData data = new NSMutableData ();

//create a PDF with empty rectangle, which will configure it for 8.5x11 inches
UIGraphics.BeginPDFContext (data, CGRect.Empty, null);

//start a PDF page
UIGraphics.BeginPDFPage ();
       
using (CGContext g = UIGraphics.GetCurrentContext ()) {
    g.ScaleCTM (1, -1);
    g.TranslateCTM (0, -25);      
    g.SelectFont ("Helvetica", 25, CGTextEncoding.MacRoman);
    g.ShowText ("Hello Core Graphics");
    }
    
//complete a PDF page
UIGraphics.EndPDFContent ();
```

Der resultierende Text gezeichnet wird, um die PDF-Datei, klicken Sie dann im enthalten eine `NSData` , können gespeichert werden, hochgeladen, e-Mail usw.


## <a name="summary"></a>Zusammenfassung

In diesem Artikel erläutert die Grafikfunktionen über die *Graphics Core* Framework. Wurde erläutert, wie mit Graphics Core Zeichnen von Geometry, Bilder und PDF-Dateien im Kontext einer `UIView,` sowie für Grafiken Speicher gesicherte Kontexte.

## <a name="related-links"></a>Verwandte Links

- [Core Graphics-Beispiel](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/)
- [Grafiken und Animationen Exemplarische Vorgehensweise](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [Core Animation](~/ios/platform/graphics-animation-ios/core-animation.md)
- [Core Animation Rezepte](https://developer.xamarin.com/recipes/ios/animation/coreanimation)
