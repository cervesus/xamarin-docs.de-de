---
title: Zeichnen von Geometry mit CCDrawNode
description: CCDrawNode stellt Methoden zum Zeichnen primitive Objekte, z. B. Linien, Kreise und Dreiecke bereit.
ms.topic: article
ms.prod: xamarin
ms.assetid: 46A3C3CE-74CC-4A3A-AB05-B694AE182ADB
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: a7b62b131db3fc224ef59bdb9189b96d61129f30
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="drawing-geometry-with-ccdrawnode"></a>Zeichnen von Geometry mit CCDrawNode

_CCDrawNode stellt Methoden zum Zeichnen primitive Objekte, z. B. Linien, Kreise und Dreiecke bereit._

Die `CCDrawNode` Klasse in CocosSharp bietet mehrere Methoden zum Zeichnen von allgemeinen geometrischer Formen. Es erbt von der `CCNode` Klasse, und wird in der Regel hinzugefügt `CCLayer` Instanzen. Diese Anleitung enthält Informationen zum Verwenden `CCDrawNode` Instanzen, um benutzerdefiniertes Rendering auszuführen. Darüber hinaus eine umfassende Liste der verfügbaren Draw-Funktionen mit Screenshots und Codebeispiele.


# <a name="creating-a-ccdrawnode"></a>Erstellen eine CCDrawNode

Die `CCDrawNode` Klasse kann verwendet werden, um geometrische Objekte, z. B. Kreise, Rechtecke und Linien zu zeichnen. Beispielsweise wird im folgenden Codebeispiel wird gezeigt, wie zum Erstellen einer `CCDrawNode` -Instanz, die einen Kreis in zeichnet einer `CCLayer` implementierende Klasse:


```csharp
public class GameLayer : CCLayer
{
    public GameLayer ()
    {
        var drawNode = new CCDrawNode ();
        this.AddChild (drawNode);
        // Origin is bottom-left of the screen. This moves
        // the drawNode 100 pixels to the right and 100 pixels up
        drawNode.PositionX = 100;
        drawNode.PositionY = 100;

        drawNode.DrawCircle (
            center: new CCPoint (0, 0),
            radius: 20,
            color: CCColor4B.White);

    }
} 
```

Dieser Code erzeugt die folgenden Kreis zur Laufzeit:

![](ccdrawnode-images/image1.png "Dieser Code erzeugt diese Kreis zur Laufzeit")


# <a name="draw-method-details"></a>Zeichnen Sie Methodendetails

Werfen wir einen Blick auf einige Details im Zusammenhang mit der Zeichnung mit einem `CCDrawNode`:


## <a name="draw-methods-positions-are-relative-to-the-ccdrawnode"></a>Zeichnen Methoden Positionen werden relativ zu den CCDrawNode

Alle Draw-Methoden erfordern mindestens ein Positionswert zum Zeichnen. Dieser Positionswert ist relativ zum die `CCDrawNode` Instanz. Dies bedeutet, dass die `CCDrawNode` selbst hat es sich um eine Position aus, und zeichnen Sie alle Aufrufe, die auf die `CCDrawNode` akzeptieren auch ein oder mehrere Positionswerte. Um besser zu verstehen, wie diese Werte kombinieren, sehen wir uns einige Beispiele.

Zunächst betrachten wir die `DrawCircle` oben angeführten Beispiel:


```csharp
...
drawNode.PositionX = 100;
drawNode.PositionY = 100;

drawNode.DrawCircle (center: new CCPoint (0, 0),
...
```

In diesem Fall die `CCDrawNode` positioniert ist (100,100), und der gezeichnete Kreis entspricht bei (0,0) relativ zu der `CCDrawNode`, wodurch des Kreis wird zentriert 100 Pixel nach oben und rechts von der unteren linken Ecke des Spiels Bildschirms.

Die `CCDrawNode` können auch positioniert werden am ursprünglichen Speicherort (unten links im Bildschirm) der vertrauenden Seite auf den Kreis für Offsets:


```csharp
...
drawNode.PositionX = 0;
drawNode.PositionY = 0;

drawNode.DrawCircle (center: new CCPoint (50, 60),
...
```

Der Code oben führt Kreismittelpunkts um 50 Einheiten (`drawNode.PositionX` + `CCPoint.X`) rechts von der linken Seite des Bildschirms und 60 (`drawNode.PositionY` + `CCPoint.Y`) Einheiten über den unteren Rand des Bildschirms.

Sobald eine Draw-Methode aufgerufen wurde, der gezeichnete-Objekt nicht geändert werden, wenn die `CCDrawNode.Clear` -Methode aufgerufen wird, werden neu positionieren muss erfolgen, auf die `CCDrawNode` selbst.

Objekte, die gezeichnet `CCNodes` ebenfalls betroffen sind der `CCNode` Instanz `Rotation` und `Scale` Eigenschaften.


## <a name="draw-methods-do-not-need-to-be-called-every-frame"></a>Zeichnen Sie die Methoden müssen nicht jeden Frame aufgerufen werden.

Draw-Methoden müssen nur einmal aufgerufen werden, um eine permanente Visualisierung zu erstellen. In der oben gezeigten Beispiel wird der Aufruf von `DrawCircle` im Konstruktor der `GameLayer` – `DrawCircle` muss nicht alle Rahmen um den Kreis erneut zu zeichnen, wenn der Bildschirm aktualisiert wird, aufgerufen werden.

Dies unterscheidet sich von Draw-Methoden in MonoGame, die in der Regel wird Rendern etwas, um den Bildschirm für nur einen Rahmen und dem muss jedem Frame aufgerufen werden.

Wenn Draw-Methoden, dass jeder Frame aufgerufen werden, dann werden innerhalb der aufrufenden Objekte gesammelt `CCDrawNode` Instanz, was einen Abfall der Framerate weitere Objekte gezeichnet werden.


## <a name="each-ccdrawnode-supports-multiple-draw-calls"></a>Jede CCDrawNode unterstützt mehrere Draw-Aufrufe

`CCDrawNode` Instanzen können verwendet werden, um mehrere Formen zu zeichnen. Dadurch können komplexe visuelle Objekte in ein einzelnes Objekt eingeschlossen werden. Beispielsweise kann der folgende Code verwendet werden, zum Rendern von mehreren Kreise mit einem `CCDrawNode`:


```csharp
for (int i = 0; i < 8; i++)
{
    drawNode.DrawCircle (
        center: new CCPoint (i*15, 0),
        radius: 20,
        color: CCColor4B.White);
} 
```

Dadurch wird in der folgenden Abbildung:

![](ccdrawnode-images/image2.png "Dadurch wird in dieser Abbildung")


# <a name="draw-call-examples"></a>Beispiele für Draw-Aufruf

Die folgenden Zeichnen stehen im `CCDrawNode`:

- [DrawCatmullRom](#DrawCatmullRom)
- [DrawCircle](#DrawCircle)
- [DrawCubicBezier](#DrawCubicBezier)
- [DrawEllipse](#DrawEllipse)
- [DrawLineList](#DrawLineList)
- [DrawPolygon](#DrawPolygon)
- [DrawQuadBezier](#DrawQuadBezier)
- [DrawRect](#DrawRect)
- [DrawSegment](#DrawSegment)
- [DrawSolidArc](#DrawSolidArc)
- [DrawSolidCircle](#DrawSolidCircle)
- [DrawTriangleList](#DrawTriangleList)


## <a name="drawcardinalspline"></a>DrawCardinalSpline

`DrawCardinalSpline` erstellt eine gekrümmte über eine Variable Anzahl von Punkten an. 

Die `config` die zeigt, die die Splinekurve-through Pass wird (differenzierungsparameter) definiert. Das folgende Beispiel zeigt eine Splinekurve über vier Punkte übergibt.

Die `tension` Parameter wie scharfe Kontrollen rundet die Punkte auf die Splinekurve angezeigt werden. Ein `tension` Wert 0 führt zu einem gekrümmten Spline und ein `tension` Wert 1 führt zu einem Spline gerade Linien und schwer Rahmen gezeichnet.

Obwohl Splines gekrümmte Linien sind, zeichnet CocosSharp Splines mit gerade Linien an. Die `segments` Parameter wird gesteuert, wie viele Segmente um verwenden, die die Splinekurve gezeichnet werden soll. Eine größere Anzahl führt zu einem reibungslosen gekrümmte Spline auf Kosten der Leistung. 

Codebeispiel:


```csharp
var splinePoints = new List<CCPoint> ();
splinePoints.Add (new CCPoint (0, 0));
splinePoints.Add (new CCPoint (50, 70));
splinePoints.Add (new CCPoint (0, 140));
splinePoints.Add (new CCPoint (100, 210));

drawNode.DrawCardinalSpline (
    config: splinePoints,
    tension: 0,
    segments: 64,
    color:CCColor4B.Red); 
```

![](ccdrawnode-images/image3.png "Die Segmente-Parameter wird gesteuert, wie viele Segmente um gezeichnet, die die Splinekurve")


## <a name="drawcatmullrom"></a>DrawCatmullRom

`DrawCatmullRom` erstellt eine gekrümmte über eine Variable Anzahl von Punkten, ähnlich wie `DrawCardinalLine`. Diese Methode schließt einen Spannungsparameter nicht.

Codebeispiel:

```csharp
var splinePoints = new List<CCPoint> ();
splinePoints.Add (new CCPoint (0, 0));
splinePoints.Add (new CCPoint (80, 90));
splinePoints.Add (new CCPoint (100, 0));
splinePoints.Add (new CCPoint (0, 130)); 

drawNode.DrawCatmullRom (
    points: splinePoints,
    segments: 64); 
```

![](ccdrawnode-images/image4.png "DrawCatmullRom erstellt eine gekrümmte über eine Variable Anzahl von Punkten, ähnlich wie DrawCardinalLine")


## <a name="drawcircle"></a>DrawCircle

`DrawCircle` erstellt eine Umkreis eines Kreises, der einen bestimmten `radius`.

Codebeispiel:

```csharp
drawNode.DrawCircle (
    center:new CCPoint (0, 0),
    radius:20,
    color:CCColor4B.Yellow); 
```

![](ccdrawnode-images/image5.png "DrawCircle erstellt einen Umkreis eines Kreises mit einem angegebenen radius")


## <a name="drawcubicbezier"></a>DrawCubicBezier

`DrawCubicBezier` Zeichnet eine gekrümmte zwischen zwei Punkten, die mit Steuerpunkte des Pfads zwischen den beiden Punkten festlegen.

Codebeispiel:

```csharp
drawNode.DrawCubicBezier (
    origin: new CCPoint (0, 0),
    control1: new CCPoint (50, 150),
    control2: new CCPoint (250, 150),
    destination: new CCPoint (170, 0),
    segments: 64,
    lineWidth: 1,
    color: CCColor4B.Green); 
```

 ![](ccdrawnode-images/image6.png "DrawCubicBezier zeichnet eine gekrümmte Linie zwischen zwei Punkten")


## <a name="drawellipse"></a>DrawEllipse

`DrawEllipse` erstellt den Umriss einer *Ellipse*, dies wird häufig als bezeichnet eine Ellipse (obwohl die beiden nicht geometrisch identisch sind). Die Form der Ellipse kann definiert werden, indem eine `CCRect` Instanz.

Codebeispiel:


```csharp
drawNode.DrawEllipse (
    rect: new CCRect (0, 0, 130, 90),
    lineWidth: 2,
    color: CCColor4B.Gray); 
```

![](ccdrawnode-images/image8.png "DrawEllipse erstellt der Kontur einer Ellipse, die häufig als Oval bezeichnet wird")


## <a name="drawline"></a>DrawLine

`DrawLine` wird eine Verbindung mit einer Zeile mit einer angegebenen Breite Punkte. Diese Methode ist vergleichbar mit `DrawSegment`, abgesehen von den Flatfile-Endpunkte im Gegensatz zu runden Endpunkte erstellt.

Codebeispiel:


```csharp
drawNode.DrawLine (
    from: new CCPoint (0, 0),
    to: new CCPoint (150, 30),
    lineWidth: 5,
    color:CCColor4B.Orange); 
```

![](ccdrawnode-images/image9.png "DrawLine stellt eine Verbindung mit Verwaltungspunkten mit einer Zeile mit einer angegebenen Breite her.")


## <a name="drawlinelist"></a>DrawLineList

`DrawLineList` erstellt mehrere Zeilen durch Herstellen einer Verbindung jedes Paar von Punkten, die gemäß einer `CCV3F_C4B` Array. Die `CCV3F_C4B` Struktur enthält die Werte für die Position und Farbe. Die `verts` Parameter sollte immer eine gerade Anzahl von Punkten enthalten, wie jede Zeile mit zwei Punkten definiert ist.

Codebeispiel:


```csharp
CCV3F_C4B[] verts = new CCV3F_C4B[] {
    // First line:
    new CCV3F_C4B( new CCPoint(0,0), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(30,60), CCColor4B.White),
    // second line, will blend from white to red:
    new CCV3F_C4B( new CCPoint(60,0), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(120,120), CCColor4B.Red)
};

drawNode.DrawLineList (verts); 
```

 ![](ccdrawnode-images/image10.png "Der Verts-Parameter sollte immer eine gerade Anzahl von Punkten enthalten, wie jede Zeile mit zwei Punkten definiert ist")




## <a name="drawpolygon"></a>DrawPolygon

`DrawPolygon` erstellt ein Polygon ausgefüllt mit variabler Breite und Farbe Konturen.

Codebeispiel:


```csharp
CCPoint[] verts = new CCPoint[] {
    new CCPoint(0,0),
    new CCPoint(0, 100),
    new CCPoint(50, 150),
    new CCPoint(100, 100),
    new CCPoint(100, 0)
};

drawNode.DrawPolygon (verts,
    count: verts.Length,
    fillColor: CCColor4B.White,
    borderWidth: 5,
    borderColor: CCColor4B.Red,
    closePolygon: true); 
```

![](ccdrawnode-images/image11.png "DrawPolygon erstellt ein Polygon ausgefüllt mit variabler Breite und Farbe Konturen")


## <a name="drawquadbezier"></a>DrawQuadBezier

`DrawQuadBezier` verbindet zwei Punkten Linie als Trennzeichen. Verhält sich ähnlich wie `DrawCubicBezier` unterstützt jedoch nur einen einzigen Steuerungspunkt.

Codebeispiel:


```csharp
drawNode.DrawQuadBezier (
    origin:new CCPoint (0, 0),
    control:new CCPoint (200, 0),
    destination:new CCPoint (0, 300),
    segments:64,
    lineWidth:1,
    color:CCColor4B.White);
```

![](ccdrawnode-images/image12.png "DrawQuadBezier verbindet zwei Punkte mit einer Linie")


## <a name="drawrect"></a>DrawRect

`DrawRect` erstellt ein Rechteck ausgefüllt mit variabler Breite und Farbe Konturen.

Codebeispiel: 


```csharp
var shape = new CCRect (
    0, 0, 100, 200);
drawNode.DrawRect(shape,
    fillColor:CCColor4B.Blue,
    borderWidth: 4,
    borderColor:CCColor4B.White); 
```

![](ccdrawnode-images/image13.png "DrawRect erstellt ein Rechteck ausgefüllt mit variabler Breite und Farbe Konturen")


## <a name="drawsegment"></a>DrawSegment

`DrawSegment` verbindet zwei Punkten mit einer Zeile mit variabler Breite und Farbe. Sie ähnelt damit `DrawLine`, außer dass es round Endpunkte anstatt Flatfile Endpunkte erstellt.

Codebeispiel:


```csharp
drawNode.DrawSegment (from: new CCPoint (0, 0),
    to: new CCPoint (100, 200),
    radius: 5,
    color:new CCColor4F(1,1,1,1)); 
```

![](ccdrawnode-images/image14.png "DrawSegment verbindet zwei Punkte mit einer Zeile mit variabler Breite und Farbe")


## <a name="drawsolidarc"></a>DrawSolidArc

`DrawSolidArc` erstellt eine Keil ausgefüllt, einer angegebenen Farbe und den Radius.

Codebeispiel:


```csharp
drawNode.DrawSolidArc(
    pos:new CCPoint(100, 100),
    radius:200,
    startAngle:0,
    sweepAngle:CCMathHelper.Pi/2, // this is in radians, clockwise
    color:CCColor4B.White); 
```

![](ccdrawnode-images/image15.png "DrawSolidArc erstellt eine Keil ausgefüllt, der angegebenen Farbe und die RADIUS-")


## <a name="drawsolidcircle"></a>DrawSolidCircle

`DrawCircle` erstellt einen Kreis ausgefüllt, der dem angegebenen Radius.

Codebeispiel:


```csharp
drawNode.DrawSolidCircle(
    pos: new CCPoint (100, 100),
    radius: 50,
    color: CCColor4B.Yellow); 
```

![](ccdrawnode-images/image16.png "DrawCircle erstellt einen Kreis ausgefüllt, der dem angegebenen radius")


## <a name="drawtrianglelist"></a>DrawTriangleList

`DrawTriangleList` erstellt eine Liste der Dreiecke ab. Jedes Dreieck wird definiert, um drei `CCV3F_C4B` Instanzen in einem Array. Die Anzahl der Scheitelpunkte im Array übergeben wird, um die `verts` -Parameter muss ein Vielfaches von 3 sein. Beachten Sie, die in die einzige Information enthalten `CCV3F_C4B` ist die Position der Verts und ihre Farbe – die `DrawTriangleList` zeichnen Dreiecke mit Texturen werden nicht unterstützt.

Codebeispiel:


```csharp
CCV3F_C4B[] verts = new CCV3F_C4B[] {
    // First triangle:
    new CCV3F_C4B( new CCPoint(0,0), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(30,60), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(60,0), CCColor4B.White),
    // second triangle, each point has different colors:
    new CCV3F_C4B( new CCPoint(90,0), CCColor4B.Yellow),
    new CCV3F_C4B( new CCPoint(120,60), CCColor4B.Red),
    new CCV3F_C4B( new CCPoint(150,0), CCColor4B.Blue)
};

drawNode.DrawTriangleList (verts); 
```

![](ccdrawnode-images/image17.png "DrawTriangleList erstellt eine Liste von Dreiecken")


# <a name="summary"></a>Zusammenfassung

Diese Anleitung wird erläutert, wie zum Erstellen einer `CCDrawNode` und Primitive basierende Renderingvorgänge ausgeführt. Es bietet ein Beispiel für jeden Draw-Aufrufe.

## <a name="related-links"></a>Verwandte Links

- [CCDrawNode API](https://developer.xamarin.com/api/type/CocosSharp.CCDrawNode/)
- [Das vollständige Codebeispiel](https://developer.xamarin.com/samples/mobile/CCDrawNode/)
