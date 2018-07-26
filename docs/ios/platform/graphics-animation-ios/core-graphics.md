---
title: Core-Grafiken in Xamarin.iOS
description: Dieser Artikel beschreibt die wichtigste Grafik-iOS-Frameworks. Es zeigt, wie auf die wichtigste Grafik, die zum Zeichnen von Geometry, Bilder und PDF-Dateien verwenden.
ms.prod: xamarin
ms.assetid: 4A30F480-0723-4B8A-9049-7CEB6211304A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 82c54074db722824c56ae3ae86620c804b8d109e
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241948"
---
# <a name="core-graphics-in-xamarinios"></a>Core-Grafiken in Xamarin.iOS

_Dieser Artikel beschreibt die wichtigste Grafik-iOS-Frameworks. Es zeigt, wie auf die wichtigste Grafik, die zum Zeichnen von Geometry, Bilder und PDF-Dateien verwenden._

iOS enthält die [ *Core Graphics* ](https://developer.apple.com/library/prerelease/ios/documentation/CoreGraphics/Reference/CoreGraphics_Framework/index.html) Framework auf niedriger Ebene zeichnen zu unterstützen. Diese Frameworks eignen, was die umfassenden grafische Funktionen in UIKit aktivieren. 

Wichtigste Grafik ist ein auf niedriger Ebene 2D-Grafiken-Framework, mit der Zeichnung geräteunabhängige Grafiken. Alle Zeichnungen in UIKit 2D Core Graphics wird intern verwendet.

Wichtigste Grafik unterstützt das Zeichnen in eine Reihe von Szenarien, einschließlich:

-  [Zeichnen auf dem Bildschirm über einen `UIView` ](#Drawing_in_a_UIView_Subclass) .
-  [Darstellen von Bildern im Arbeitsspeicher oder auf dem Bildschirm](#Drawing_Images_and_Text).
-  Erstellen und zeichnen eine PDF-Format.
-  Lesen und zeichnen eine vorhandene PDF-Datei.


## <a name="geometric-space"></a>Geometrische Speicherplatz

Unabhängig vom Szenario, gilt alle Zeichnungen mit Core Graphics fertig erfolgt in geometrische Leerzeichen, was bedeutet, dass die Funktionsweise in abstrakten Punkte statt in Pixel. Sie beschreiben, was in Bezug auf die Geometrie und zeichnen Status, z. B. Farben, Linienstile usw. gezeichnet werden soll, und Core Graphics behandelt alles, was in Pixel zu übersetzen. Solchen Zustand wird auf einen Grafikkontext hinzugefügt, die Sie z. B. ein übertragen der Canvas vorstellen können.

Es gibt einige Vorteile dieses Ansatzes:

-  Zeichencode wird dynamisch und kann anschließend Grafiken zur Laufzeit ändern.
-  Reduziert die Notwendigkeit für statische Bilder im Anwendungspaket kann die Anwendungsgröße reduzieren.
-  Grafiken werden auf Geräten ausfallsicherer Änderungen.

<a name="Drawing_in_a_UIView_Subclass"/>

## <a name="drawing-in-a-uiview-subclass"></a>Zeichnen in einer UIView-Unterklasse

Jede `UIView` verfügt über eine `Draw` -Methode, die vom System aufgerufen wird, wenn es gezeichnet werden muss. Hinzufügen von Code zum Zeichnen auf eine Ansicht, Unterklasse `UIView` und überschreiben `Draw`:

```csharp
public class TriangleView : UIView
{
    public override void Draw (CGRect rect)
    {
        base.Draw (rect);
    }
}
```

Zeichnen-Befehl sollte nie direkt aufgerufen werden. Es wird vom System aufgerufen, während der Verarbeitung führen. Beim ersten Durchlaufen der ausführen-Schleife, nachdem eine Ansicht der Hierarchie von Inhaltsansichten hinzugefügt wird seine `Draw` Methode wird aufgerufen. Nachfolgende Aufrufe von `Draw` auftreten, wenn die Ansicht markiert ist, müssen Sie gezeichnet werden soll, durch Aufrufen von entweder `SetNeedsDisplay` oder `SetNeedsDisplayInRect` für die Sicht.

### <a name="pattern-for-graphics-code"></a>Muster für die Grafik-Code

Der Code in die `Draw` Implementierung sollte beschreiben, was gezeichnet werden sollen. Der Code zum Zeichnen folgt einem Muster, in dem er legt einige zeichnen Zustand, und ruft eine Methode, um anzufordern, es gezeichnet werden. Dieses Muster kann wie folgt generalisiert werden:

1. Ein grafikontext zu erhalten.

2. Richten Sie die Attribute zu zeichnen.

3. Erstellen Sie eine Geometrie aus primitiven zeichnen.

4. Rufen Sie eine zeichnen- oder Stroke-Methode.

### <a name="basic-drawing-example"></a>Beispiel für die grundlegenden Funktionen zum Zeichnen

Betrachten Sie z. B. des folgende Codeausschnitts aus:

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

Wir unterteilen diesen Code:

```csharp
using (CGContext g = UIGraphics.GetCurrentContext ()) {
...
}
```
Mit der folgenden Zeile ruft er zuerst dem aktuellen Grafikkontext zum Zeichnen verwendete ab. Sie können ein grafikontext des Zeichenbereichs vorstellen, dass die Zeichnung, erfolgt, enthält alle Zustände über das Zeichnen, z. B. Strich und Füllfarben als auch die Geometrie gezeichnet werden soll.

```csharp
g.SetLineWidth (10);
UIColor.Blue.SetFill ();
UIColor.Red.SetStroke ();
``` 

Nach dem Abrufen eines Grafikkontexts richtet der Code einige Attribute zur Verwendung beim Zeichnen, oben gezeigt. In diesem Fall werden die Linienfarben für Breite, Strich und Füllung festgelegt. Alle folgenden Zeichnungen wird dieser Attribute verwendet, da sie in den Grafikkontext Zustand verwaltet werden.

So erstellen Sie den Code Geometrie verwendet eine `CGPath`, dadurch kann es sich um einen Grafikpfad aus Linien und Kurven beschrieben werden. In diesem Fall fügt der Pfad Pfeile, die ein Array von Punkten, um ein Dreieck zu erstellen. Wie unten Core Graphics verwendet ein Koordinatensystem für zeichnen-Ansicht angezeigt, ist der Ursprung, in dem in der oberen linken Ecke, durch die positive X-direkte nach rechts und die Positive-y-Richtung nach unten:

```csharp
var path = new CGPath ();

path.AddLines (new CGPoint[]{
new CGPoint (100, 200),
new CGPoint (160, 100), 
new CGPoint (220, 200)});

path.CloseSubpath ();
``` 

Nachdem der Pfad erstellt wurde, es ist hinzugefügt dem Grafikkontext, damit Aufrufen `AddPath` und `DrawPath` bzw. können ziehen Sie es.

Die resultierende Ansicht ist unten dargestellt:

 ![](core-graphics-images/00-bluetriangle.png "Das Beispiel-Ausgabe-Dreieck")

## <a name="creating-gradient-fills"></a>Erstellen von Farbverläufen

Umfangreichere Formen der Zeichnung sind ebenfalls verfügbar. Beispielsweise können Core Graphics, Farbverläufe erstellen und Anwenden einer Beschneidungspfade. Um eine graduelle Füllung innerhalb des Pfads aus dem vorherigen Beispiel zu zeichnen, muss zunächst der Pfad als des Freistellungspfads festgelegt werden:

```csharp
// add the path back to the graphics context so that it is the current path
g.AddPath (path);
// set the current path to be the clipping path
g.Clip ();
```

Den aktuellen Pfad festlegen, wie des Freistellungspfads in allen folgenden Zeichnungen innerhalb der Geometrie des Pfads, wie z. B. den folgenden Code, schränkt zeichnet die einen linearen Farbverlauf:

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

Diese Änderungen eine graduelle Füllung erstellen, wie unten dargestellt:

 ![](core-graphics-images/01-gradient-fill.png "Im Beispiel mit Farbverlauf")

## <a name="modifying-line-patterns"></a>Ändern Zeilenmuster

Die Zeichnungsattribute Zeilen können auch mit Core Graphics geändert werden. Dies schließt das Ändern der Breite und Strich die Linienfarbe sowie die Zeile-Muster selbst, wie im folgenden Code dargestellt:

```csharp
//use a dashed line
g.SetLineDash (0, new nfloat[] { 10, 4 * (nfloat)Math.PI });
```

Dieser Code vor dem Zeichnen Vorgänge Ergebnisse hinzufügen in gestrichelte Konturen 10 Einheiten mit 4 Einheiten Abstand zwischen der Gedankenstriche, long, wie unten dargestellt:

 ![](core-graphics-images/02-dashed-stroke.png "Dieser Code vor dem Zeichnen Vorgänge Ergebnisse hinzufügen in gestrichelten Striche")
 
Beachten Sie, dass bei der Verwendung der Unified API in Xamarin.iOS der Arraytyp benötigt, ein `nfloat`, und auch explizit in Math.PI umgewandelt werden muss.

<a name="Drawing_Images_and_Text"/>

## <a name="drawing-images-and-text"></a>Zeichnen von Bildern und Text

Zusätzlich zum Zeichnen von Pfaden in einer Ansicht der Grafikkontext, unterstützt die wichtigste Grafik auch Bilder und Text. Um ein Bild zu zeichnen, erstellen Sie einfach eine `CGImage` und übergeben es an eine `DrawImage` aufrufen:

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);

    using(CGContext g = UIGraphics.GetCurrentContext ()){
        g.DrawImage (rect, UIImage.FromFile ("MyImage.png").CGImage);
    }
}
```

Allerdings erzeugt dies ein Bild zeichnen auf dem Kopf stehend, wie unten dargestellt:

 ![](core-graphics-images/03-upside-down-monkey.png "Ein Bild auf dem Kopf stehend gezeichnet")

Der Grund dafür ist Core Graphics-Ursprung für das Zeichnen des Bilds in der unteren linken Ecke, während die Ansicht seinen Ursprung in der oberen linken Ecke aufweist. Aus diesem Grund, um das Image ordnungsgemäß anzuzeigen, der Ursprung muss geändert werden, kann durch Ändern erreicht werden die *aktuellen Transformationsmatrix* *(CTM)*. Die CTM definiert, in dem Punkt, auch bekannt als Leben *Benutzerbereich*. Invertieren der CTM in y-Richtung und verschieben es durch die Begrenzungen Höhe in negative y-Richtung können das Bild gekippt.

Der Grafikkontext verfügt über Hilfsmethoden für die CTM zu transformieren. In diesem Fall `ScaleCTM` "spiegelt" die Zeichnung und `TranslateCTM` verschiebt er an der oberen linken Ecke, wie unten dargestellt:

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

Klicken Sie dann das resultierende Image wird aufrechte:

 ![](core-graphics-images/04-upright-monkey.png "Das Beispiel angezeigte Bild aufrechten")

> [!IMPORTANT]
> Änderungen an den Grafikkontext gelten für alle nachfolgenden Zeichenvorgänge. Aus diesem Grund wirkt die CTM transformiert wird, alle zusätzliche Zeichnung. Z. B. Wenn Sie das Dreieck nach der Transformation CTM gezeichnet haben, scheint finden Sie verkehrt herum.

### <a name="adding-text-to-the-image"></a>Hinzufügen von Text in der Abbildung

Wie auch Zeichnen von Text mit Core Graphics beim Pfade und Bilder, die gleiche grundlegende Muster einige Grafikzustand und Aufrufen einer Methode zum Zeichnen. Im Fall von Text, die Methode zum Anzeigen von Text ist `ShowText`. Wenn auf das Bild zeichnen Beispiel hinzugefügt wird, zeichnet der folgende Code mit Core Graphics Text:

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

Wie Sie sehen, ähnelt festlegen den Status der Grafik für das Zeichnen von Text zum Zeichnen der Geometrie. Jedoch Zeichnen von Text, der Text zeichnen-Modus und die Schriftart angewendet sowie. Ein Schatten wird in diesem Fall auch angewendet, obwohl das Anwenden von Tiefen funktioniert gleichermaßen für den Pfad gezeichnet.

Der resultierende Text wird mit dem Bild angezeigt, wie unten dargestellt:

 ![](core-graphics-images/05-text-on-image.png "Der resultierende Text wird mit dem Bild angezeigt.")

## <a name="memory-backed-images"></a>Arbeitsspeicher-Backup-Images

Zusätzlich zum Zeichnen in einer Ansicht der Grafikkontext gesichert Core Graphics unterstützt das Zeichnen von Arbeitsspeicher Bilder, auch bekannt als außerhalb des Bildschirms gezeichnet. Auf diese Weise sind erforderlich:

-  Ein grafikontext erstellen, basiert auf einer in-Memory Bitmap
-  Festlegen von Zeichnen-Zustand und dem Ausgeben von Zeichnen-Befehle
-  Das Bild abrufen aus dem Kontext
-  Entfernen den Kontext


Im Gegensatz zu den `Draw` -Methode, die der Kontext durch die Ansicht bereitgestellt wird in diesem Fall Sie erstellen den Kontext auf zwei Arten:

1. Durch Aufrufen von `UIGraphics.BeginImageContext` (oder `BeginImageContextWithOptions`)

2. Durch Erstellen eines neuen `CGBitmapContextInstance`

 `CGBitmapContextInstance` ist nützlich, wenn Sie direkt mit der Bild-Bits, z. B. für Fälle arbeiten, in dem Sie einen benutzerdefiniertes Image Manipulation-Algorithmus verwenden. In allen anderen Fällen sollten Sie verwenden `BeginImageContext` oder `BeginImageContextWithOptions`.

Nachdem Sie den Kontext eines Image haben, Hinzufügen von Zeichencode ist genau so wie in einem `UIView` Unterklasse. Beispielsweise kann im Codebeispiel weiter oben verwendet, um ein Dreieck zu zeichnen verwendet werden, zum Zeichnen zu einem Bild im Speicher statt in einer `UIView`, wie unten dargestellt:

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

Eine übliche Verwendung dieser Funktionen zum Zeichnen einer Bitmap im Speicher gesicherte wird zum Erfassen eines Images aus einer `UIView`. Z. B. der folgende Code der ansichtsschicht auf einen Kontext für die Bitmap gerendert, und erstellt eine `UIImage` aus:

```csharp
UIGraphics.BeginImageContext (cellView.Frame.Size);

//render the view's layer in the current context
anyView.Layer.RenderInContext (UIGraphics.GetCurrentContext ());

//get a UIImage from the context
UIImage anyViewImage = UIGraphics.GetImageFromCurrentImageContext ();
UIGraphics.EndImageContext ();
```

## <a name="drawing-pdfs"></a>Zeichnen von PDF-Dateien

Zusätzlich zu Bildern unterstützt die wichtigste Grafik PDF-Zeichnung. Wie Bilder, Sie können eine PDF-Datei im Arbeitsspeicher zu rendern sowie zum Rendern in das PDF-Datei lesen eine `UIView`.

### <a name="pdf-in-a-uiview"></a>PDF-Datei in eine UIView

Wichtigste Grafik unterstützt auch das Lesen von PDF-Datei aus einer Datei, und Rendern sie in einer Ansicht mithilfe der `CGPDFDocument` Klasse. Die `CGPDFDocument` Klasse stellt eine PDF-Datei im Code, und können gelesen, und Zeichnen von Seiten verwendet werden.

Im folgenden code wird beispielsweise einem `UIView` Unterklasse liest eine PDF-Datei aus einer Datei in eine `CGPDFDocument`:

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

Die `Draw` Methode können Sie dann die `CGPDFDocument` zum Lesen einer Seite in `CGPDFPage` und Rendern Sie sie durch Aufrufen von `DrawPDFPage`, wie unten dargestellt:

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

### <a name="memory-backed-pdf"></a>Arbeitsspeichergestützte PDF-Datei

Für eine in-Memory-PDF, müssen Sie PDF-Datei durch den Aufruf für die kontexterstellung `BeginPDFContext`. Zeichnen in PDF-Datei ist zu Seiten, die präzise. Jede Seite wird durch den Aufruf gestartet `BeginPDFPage` und durch den Aufruf abgeschlossen `EndPDFContent`, mit dem Grafik in der Zwischenzeit code. Ebenso wie mit Bild zeichnen Arbeitsspeicher PDF Zeichnen verwendet einen Ursprung in der unteren linken, die berücksichtigt werden kann unterstützt, durch die CTM einfach ändern wie mit Bildern.

Der folgende Code zeigt, wie Zeichnen von Text auf einer PDF-Datei:

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

Der resultierende Text gezeichnet wird, um die PDF-Datei, klicken Sie dann im enthalten eine `NSData` , können gespeichert werden, hochgeladene, e-Mail usw.


## <a name="summary"></a>Zusammenfassung

In diesem Artikel erläutert, die per Grafikfunktionen der *Core Graphics* Framework. Erläutert, wie Sie mit, dass die wichtigste Grafik um Geometrie, Bilder und PDF-Dateien innerhalb des Kontexts zu zeichnen eine `UIView,` sowie für Grafiken arbeitsspeichergestützte Kontexte.

## <a name="related-links"></a>Verwandte Links

- [Beispiel: Core-Grafiken](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/)
- [Grafiken und Animation Exemplarische Vorgehensweise](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [Core Animation](~/ios/platform/graphics-animation-ios/core-animation.md)
- [Core Animation Rezepte](https://github.com/xamarin/recipes/tree/master/Recipes/ios/animation/coreanimation)
