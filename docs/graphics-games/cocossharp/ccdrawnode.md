---
title: Zeichnen von Geometrie mit CCDrawNode
description: Dieses Dokument beschreibt CCDrawNode, die Methoden für primitive Zeichenobjekte wie Linien, Kreise und Dreiecke bereitstellt.
ms.prod: xamarin
ms.assetid: 46A3C3CE-74CC-4A3A-AB05-B694AE182ADB
author: conceptdev
ms.author: crdun
ms.date: 03/24/2017
ms.openlocfilehash: b910e136366c429de8bd2ba1ac959882b4d7201d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61377592"
---
# <a name="drawing-geometry-with-ccdrawnode"></a>Zeichnen von Geometrie mit CCDrawNode

_`CCDrawNode` Stellt Methoden zum Zeichnen primitive Objekte wie Linien, Kreise und Dreiecke._

Die `CCDrawNode` Klasse in CocosSharp stellt mehrere Methoden zum Zeichnen von allgemeinen geometrischer Formen bereit. Sie erbt von der `CCNode` -Klasse und wird in der Regel hinzugefügt `CCLayer` Instanzen. Diesem Leitfaden wird beschrieben, wie Sie mit `CCDrawNode` Instanzen zum benutzerdefinierten ausgeben. Darüber hinaus eine umfassende Liste der verfügbaren Draw-Funktionen mit Screenshots und Codebeispiele.


## <a name="creating-a-ccdrawnode"></a>Erstellen eine CCDrawNode

Die `CCDrawNode` Klasse kann verwendet werden, um die geometrische Objekte wie Kreise, Rechtecke und Linien zu zeichnen. Das folgende Codebeispiel zeigt z. B. Vorgehensweise: Erstellen einer `CCDrawNode` -Instanz, die einen Kreis in zeichnet eine `CCLayer` implementierende Klasse:


```csharp
public class GameLayer : CCLayer
{
    public GameLayer ()
    {
        var drawNode = new CCDrawNode ();
        this.AddChild (drawNode);
        // Origin is bottom-left of the screen. This moves
        // the drawNode 100 pixels to the right and 100 pixels up
        drawNode.PositionX = 100;
        drawNode.PositionY = 100;

        drawNode.DrawCircle (
            center: new CCPoint (0, 0),
            radius: 20,
            color: CCColor4B.White);

    }
} 
```

Dieser Code erzeugt die folgenden Kreis Laufzeit:

![](ccdrawnode-images/image1.png "Dieser Code führt diesen Vertrauenskreis aufgenommen, zur Laufzeit")


## <a name="draw-method-details"></a>Details der Draw-Methode

Werfen wir einen Blick auf einige Details im Zusammenhang mit der Zeichnung mit einem `CCDrawNode`:


### <a name="draw-methods-positions-are-relative-to-the-ccdrawnode"></a>Positionen der Draw-Methoden sind relativ zu den CCDrawNode

Alle Draw-Methoden erfordern mindestens ein Positionswert für das Zeichnen. Der Wert für diese Position ist relativ zum die `CCDrawNode` Instanz. Dies bedeutet, dass die `CCDrawNode` selbst hat es sich um eine Position, und zeichnen Sie alle Aufrufe, die auf die `CCDrawNode` akzeptieren auch ein oder mehrere Positionswerte. Um besser zu verstehen, wie diese Werte kombinieren, wir sehen uns einige Beispiele.

Zunächst betrachten wir die `DrawCircle` obigen Beispiel:


```csharp
...
drawNode.PositionX = 100;
drawNode.PositionY = 100;

drawNode.DrawCircle (center: new CCPoint (0, 0),
...
```

In diesem Fall die `CCDrawNode` positioniert ist (100,100), und der Kreis gezeichnete bei (0,0) relativ zu den `CCDrawNode`, wodurch des Kreis wird zentriert 100 Pixel, nach oben und nach rechts neben die linke obere Ecke des Bildschirms spielen.

Die `CCDrawNode` können auch auf den Ursprung (unten links im Bildschirm), positioniert werden der vertrauenden Seite, auf den Kreis für Offsets:


```csharp
...
drawNode.PositionX = 0;
drawNode.PositionY = 0;

drawNode.DrawCircle (center: new CCPoint (50, 60),
...
```

Der Code oben Ergebnisse in den Mittelpunkt auf 50 Einheiten (`drawNode.PositionX` + `CCPoint.X`) rechts von der linken Seite des Bildschirms, und 60 (`drawNode.PositionY` + `CCPoint.Y`) Einheiten über dem unteren Rand des Bildschirms.

Sobald eine Draw-Methode aufgerufen wurde, den gezeichnete Objekt nicht geändert werden, wenn die `CCDrawNode.Clear` -Methode aufgerufen wird, daher wird jeder neu positionieren muss auf erfolgen die `CCDrawNode` selbst.

Objekte, die vom `CCNodes` hängen auch davon ab, der `CCNode` Instanz `Rotation` und `Scale` Eigenschaften.


### <a name="draw-methods-do-not-need-to-be-called-every-frame"></a>Draw-Methoden müssen nicht jedes Bild aufgerufen werden

Draw-Methoden müssen nur einmal aufgerufen werden, um eine persistente Visualisierung zu erstellen. Im obigen Beispiel ist der Aufruf von `DrawCircle` im Konstruktor der `GameLayer` – `DrawCircle` muss nicht aufgerufen werden alle Rahmen um den Kreis neu zu zeichnen, wenn der Bildschirm wird aktualisiert.

Dies unterscheidet sich von Draw-Methoden in MonoGame, die in der Regel rendert etwas zum Bildschirm für die nur einen Frame aus, und der every-Frame aufgerufen werden.

Wenn Draw-Methoden werden als "jedes Einzelbild" bezeichnet, und klicken Sie dann Objekte in der aufrufenden schließlich sammelt `CCDrawNode` Instanz, was einen Abfall der Framerate wie weitere Objekte gezeichnet werden.


### <a name="each-ccdrawnode-supports-multiple-draw-calls"></a>Jede CCDrawNode unterstützt mehrere Draw-Aufrufe

`CCDrawNode` Instanzen können verwendet werden, um mehrere Formen zu zeichnen. Dies ermöglicht komplexe visuelle Objekte, die in einem einzelnen Objekt enthalten sein. Beispielsweise kann der folgende Code verwendet werden, zum Rendern von mehreren Kreise mit einem `CCDrawNode`:


```csharp
for (int i = 0; i < 8; i++)
{
    drawNode.DrawCircle (
        center: new CCPoint (i*15, 0),
        radius: 20,
        color: CCColor4B.White);
} 
```

Dadurch wird in der folgenden Abbildung:

![](ccdrawnode-images/image2.png "Dadurch wird die folgende Grafik")


## <a name="draw-call-examples"></a>Beispiele für die Draw-Aufruf

Die folgenden zeichnen-Befehlen finden Sie in `CCDrawNode`:

- [`DrawCatmullRom`](#drawcatmullrom)
- [`DrawCircle`](#drawcircle)
- [`DrawCubicBezier`](#drawcubicbezier)
- [`DrawEllipse`](#drawellipse)
- [`DrawLineList`](#drawlinelist)
- [`DrawPolygon`](#drawpolygon)
- [`DrawQuadBezier`](#drawquadbezier)
- [`DrawRect`](#drawrect)
- [`DrawSegment`](#drawsegment)
- [`DrawSolidArc`](#drawsolidarc)
- [`DrawSolidCircle`](#drawsolidcircle)
- [`DrawTriangleList`](#drawtrianglelist)


### <a name="drawcardinalspline"></a>DrawCardinalSpline

`DrawCardinalSpline` erstellt eine gekrümmte Linie durch eine Variable Anzahl von Punkten an. 

Die `config` Parameter definiert, die die Splinekurve durchläuft verweist. Das folgende Beispiel zeigt eine Splinekurve die durchläuft vier Punkte.

Die `tension` parametersteuerung wie scharfen oder rundet die Punkte auf dem Spline angezeigt werden. Ein `tension` Wert von 0 führt zu einem gekrümmten Spline an, und ein `tension` Wert von 1 führt zu einem Spline von geraden Linien und schwer der Kanten gezeichnet.

Obwohl Splines gekrümmte Linien sind, zeichnet CocosSharp Splines mit geraden Linien an. Die `segments` Parameter wird gesteuert, wie viele Segmente, um die Splinekurve gezeichnet werden soll. Eine größere Anzahl führt zu einem reibungslosen gekrümmte Spline auf Kosten der Leistung. 

Codebeispiel:


```csharp
var splinePoints = new List<CCPoint> ();
splinePoints.Add (new CCPoint (0, 0));
splinePoints.Add (new CCPoint (50, 70));
splinePoints.Add (new CCPoint (0, 140));
splinePoints.Add (new CCPoint (100, 210));

drawNode.DrawCardinalSpline (
    config: splinePoints,
    tension: 0,
    segments: 64,
    color:CCColor4B.Red); 
```

![](ccdrawnode-images/image3.png "Die Segmente-Parameter wird gesteuert, wie viele Segmente um verwenden, um die Splinekurve zeichnen")


### <a name="drawcatmullrom"></a>DrawCatmullRom

`DrawCatmullRom` erstellt eine gekrümmte Linie durch eine Variable Anzahl von Punkten, ähnlich wie `DrawCardinalLine`. Diese Methode schließt einen Spannungsparameter nicht.

Codebeispiel:

```csharp
var splinePoints = new List<CCPoint> ();
splinePoints.Add (new CCPoint (0, 0));
splinePoints.Add (new CCPoint (80, 90));
splinePoints.Add (new CCPoint (100, 0));
splinePoints.Add (new CCPoint (0, 130)); 

drawNode.DrawCatmullRom (
    points: splinePoints,
    segments: 64); 
```

![](ccdrawnode-images/image4.png "DrawCatmullRom erstellt eine gekrümmte Linie durch eine Variable Anzahl von Punkten ähnlich DrawCardinalLine")


### <a name="drawcircle"></a>DrawCircle

`DrawCircle` erstellt einen Umkreis eines Kreises, der einen bestimmten `radius`.

Codebeispiel:

```csharp
drawNode.DrawCircle (
    center:new CCPoint (0, 0),
    radius:20,
    color:CCColor4B.Yellow); 
```

![](ccdrawnode-images/image5.png "DrawCircle erstellt einen Umkreis eines Kreises mit einem angegebenen RADIUS")


### <a name="drawcubicbezier"></a>DrawCubicBezier

`DrawCubicBezier` Zeichnet eine gekrümmte Linie zwischen zwei Punkten, die mit der Control-Punkte des Pfads zwischen den beiden Punkten.

Codebeispiel:

```csharp
drawNode.DrawCubicBezier (
    origin: new CCPoint (0, 0),
    control1: new CCPoint (50, 150),
    control2: new CCPoint (250, 150),
    destination: new CCPoint (170, 0),
    segments: 64,
    lineWidth: 1,
    color: CCColor4B.Green); 
```

 ![](ccdrawnode-images/image6.png "DrawCubicBezier zeichnet eine gekrümmte Linie zwischen zwei Punkten")


### <a name="drawellipse"></a>DrawEllipse

`DrawEllipse` erstellt den Umriss einer *Ellipse*, dies wird häufig als bezeichnet ein Oval (obwohl die beiden nicht geometrisch identisch sind). Die Form der Ellipse definiert werden, indem eine `CCRect` Instanz.

Codebeispiel:


```csharp
drawNode.DrawEllipse (
    rect: new CCRect (0, 0, 130, 90),
    lineWidth: 2,
    color: CCColor4B.Gray); 
```

![](ccdrawnode-images/image8.png "\"DrawEllipse\" erstellt, die Kontur einer Ellipse, die häufig als Oval bezeichnet wird")


### <a name="drawline"></a>DrawLine

`DrawLine` Stellt eine Verbindung her, um Punkte mit einer Zeile mit einer angegebenen Breite. Diese Methode ähnelt `DrawSegment`, außer dass die Flatfile-Endpunkten im Gegensatz zu runden Endpunkte erstellt.

Codebeispiel:


```csharp
drawNode.DrawLine (
    from: new CCPoint (0, 0),
    to: new CCPoint (150, 30),
    lineWidth: 5,
    color:CCColor4B.Orange); 
```

![](ccdrawnode-images/image9.png "DrawLine eine Verbindung mit Punkten mit einer Codezeile bestimmter Breite her.")


### <a name="drawlinelist"></a>DrawLineList

`DrawLineList` mehrere Zeilen erstellt, durch das Verbinden von jedem Standortpaar der gemäß einem `CCV3F_C4B` Array. Die `CCV3F_C4B` Struktur enthält Werte für die Position und Farbe. Die `verts` Parameter sollte immer eine gerade Anzahl von Punkten enthalten, da jede Zeile durch zwei Punkte definiert wurde.

Codebeispiel:


```csharp
CCV3F_C4B[] verts = new CCV3F_C4B[] {
    // First line:
    new CCV3F_C4B( new CCPoint(0,0), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(30,60), CCColor4B.White),
    // second line, will blend from white to red:
    new CCV3F_C4B( new CCPoint(60,0), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(120,120), CCColor4B.Red)
};

drawNode.DrawLineList (verts); 
```

 ![](ccdrawnode-images/image10.png "Der Verts-Parameter sollte immer eine gerade Anzahl von Punkten enthalten, da jede Zeile durch zwei Punkte definiert wurde")




### <a name="drawpolygon"></a>DrawPolygon

`DrawPolygon` erstellt eine ausgefüllte Polygon mit einem Rand von variabler Breite und Farbe.

Codebeispiel:


```csharp
CCPoint[] verts = new CCPoint[] {
    new CCPoint(0,0),
    new CCPoint(0, 100),
    new CCPoint(50, 150),
    new CCPoint(100, 100),
    new CCPoint(100, 0)
};

drawNode.DrawPolygon (verts,
    count: verts.Length,
    fillColor: CCColor4B.White,
    borderWidth: 5,
    borderColor: CCColor4B.Red,
    closePolygon: true); 
```

![](ccdrawnode-images/image11.png "DrawPolygon erzeugt ein Polygon, ausgefüllt mit variabler Breite und Farbe im Überblick")


### <a name="drawquadbezier"></a>DrawQuadBezier

`DrawQuadBezier` verbindet zwei Punkten durch eine Linie an. Es verhält sich ähnlich wie `DrawCubicBezier` unterstützt jedoch nur einen einzigen Steuerungspunkt.

Codebeispiel:


```csharp
drawNode.DrawQuadBezier (
    origin:new CCPoint (0, 0),
    control:new CCPoint (200, 0),
    destination:new CCPoint (0, 300),
    segments:64,
    lineWidth:1,
    color:CCColor4B.White);
```

![](ccdrawnode-images/image12.png "DrawQuadBezier verbindet zwei Punkte mit einer Linie")


### <a name="drawrect"></a>DrawRect

`DrawRect` erstellt ein Rechteck ausgefüllt mit einem Rand von variabler Breite und Farbe.

Codebeispiel: 


```csharp
var shape = new CCRect (
    0, 0, 100, 200);
drawNode.DrawRect(shape,
    fillColor:CCColor4B.Blue,
    borderWidth: 4,
    borderColor:CCColor4B.White); 
```

![](ccdrawnode-images/image13.png "DrawRect erstellt ein Rechteck ausgefüllt mit variabler Breite und Farbe im Überblick")


### <a name="drawsegment"></a>DrawSegment

`DrawSegment` verbindet zwei Punkte mit einer Zeile mit variabler Breite und Farbe. Sie ähnelt damit `DrawLine`, es sei denn round-Endpunkte statt der Flatfile-Endpunkten erstellt.

Codebeispiel:


```csharp
drawNode.DrawSegment (from: new CCPoint (0, 0),
    to: new CCPoint (100, 200),
    radius: 5,
    color:new CCColor4F(1,1,1,1)); 
```

![](ccdrawnode-images/image14.png "DrawSegment verbindet zwei Punkte mit einer Zeile mit variabler Breite und Farbe")


### <a name="drawsolidarc"></a>DrawSolidArc

`DrawSolidArc` erstellt eine ausgefüllte Keil einer angegebenen Farbe und die RADIUS.

Codebeispiel:


```csharp
drawNode.DrawSolidArc(
    pos:new CCPoint(100, 100),
    radius:200,
    startAngle:0,
    sweepAngle:CCMathHelper.Pi/2, // this is in radians, clockwise
    color:CCColor4B.White); 
```

![](ccdrawnode-images/image15.png "DrawSolidArc erstellt eine ausgefüllte Keil einer angegebenen Farbe und die RADIUS-")


### <a name="drawsolidcircle"></a>DrawSolidCircle

`DrawCircle` erstellt einen ausgefüllten Kreis von einem angegebenen Radius.

Codebeispiel:


```csharp
drawNode.DrawSolidCircle(
    pos: new CCPoint (100, 100),
    radius: 50,
    color: CCColor4B.Yellow); 
```

![](ccdrawnode-images/image16.png "DrawCircle erstellt einen ausgefüllten Kreis von einem angegebenen radius")


### <a name="drawtrianglelist"></a>DrawTriangleList

`DrawTriangleList` erstellt eine Liste von Dreiecken. Jedes Dreiecks wird definiert, um drei `CCV3F_C4B` Instanzen in einem Array. Die Anzahl der Scheitelpunkte im Array übergeben wird, um die `verts` -Parameter muss ein Vielfaches von drei sein. Beachten Sie, die in die einzige Information enthalten `CCV3F_C4B` ist die Position der Verts und ihre Farbe – die `DrawTriangleList` Methode zeichnen Dreiecke mit Texturen nicht unterstützt.

Codebeispiel:


```csharp
CCV3F_C4B[] verts = new CCV3F_C4B[] {
    // First triangle:
    new CCV3F_C4B( new CCPoint(0,0), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(30,60), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(60,0), CCColor4B.White),
    // second triangle, each point has different colors:
    new CCV3F_C4B( new CCPoint(90,0), CCColor4B.Yellow),
    new CCV3F_C4B( new CCPoint(120,60), CCColor4B.Red),
    new CCV3F_C4B( new CCPoint(150,0), CCColor4B.Blue)
};

drawNode.DrawTriangleList (verts); 
```

![](ccdrawnode-images/image17.png "DrawTriangleList erstellt eine Liste von Dreiecken")


## <a name="summary"></a>Zusammenfassung

Dieser Leitfaden erläutert, wie zum Erstellen einer `CCDrawNode` und Primitive-basierte Renderingvorgänge. Es bietet ein Beispiel für jede der Draw-Aufrufe.

## <a name="related-links"></a>Verwandte Links

- [CCDrawNode API](https://developer.xamarin.com/api/type/CocosSharp.CCDrawNode/)
- [Das vollständige Codebeispiel](https://developer.xamarin.com/samples/mobile/CCDrawNode/)
