---
title: 2D-Mathematik mit CocosSharp
description: Dieser Leitfaden behandelt 2D Mathematik für die Entwicklung von Spielen. Verwendet von CocosSharp veranschaulichen, wie Sie allgemeine Aufgaben zur Entwicklung von Spielen und Mathematik diese Aufgaben erläutert.
ms.prod: xamarin
ms.assetid: 5C241AB4-F97E-4B61-B93C-F5D307BCD517
author: conceptdev
ms.author: crdun
ms.date: 03/27/2017
ms.openlocfilehash: ac84d5b28b0f211dccb1697a4b3dbbc9cedf81e9
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61030185"
---
# <a name="2d-math-with-cocossharp"></a>2D-Mathematik mit CocosSharp

_Dieser Leitfaden behandelt 2D Mathematik für die Entwicklung von Spielen. Verwendet von CocosSharp veranschaulichen, wie Sie allgemeine Aufgaben zur Entwicklung von Spielen und Mathematik diese Aufgaben erläutert._

Zu positionieren, und verschieben Sie Objekte mit Code die ist ein wesentlicher Bestandteil der Entwicklung von Spielen aller Größen aus. Positionieren und verschieben erfordern die Verwendung der mathematischen, ob ein Spiel erfordert das Verschieben eines Objekts entlang einer geraden Linie oder die Verwendung von trigonometrische für die Rotation. In diesem Dokument werden die folgenden Themen behandelt:

 - Geschwindigkeit
 - Beschleunigung
 - Drehen von Objekten mit CocosSharp
 - Drehung verwenden mit Geschwindigkeit

Entwickler, die nicht über einen starken Math-Hintergrund oder haben, die diese Themen aus der Schule, Long-Wert-vergessen, müssen keine Gedanken machen – in diesem Dokument wird unterteilen Konzepte in kleinen Größe Teile und werden theoretische Erklärungen mit praktischen Beispielen begleitet. In diesem Artikel wird kurz gesagt, die uralte Math "Student" Frage beantworten: "Wenn eigentlich muss ich diese Dinge zu verwenden?"


## <a name="requirements"></a>Anforderungen

Obwohl es in erster Linie auf der mathematischen Seite CocosSharp in diesem Dokument geht davon Codebeispiele aus arbeiten mit Objekten, die Form erben `CCNode`. Darüber hinaus seit `CCNode` enthält keine Werte für Velocity- und Beschleunigung der Code geht davon aus arbeiten mit Entitäten, die Werte wie z. B. VelocityX, VelocityY, AccelerationX und AccelerationY bereitzustellen. Weitere Informationen zu Entitäten, finden Sie in dieser exemplarischen Vorgehensweise auf [Entitäten in CocosSharp](~/graphics-games/cocossharp/entities.md).


## <a name="velocity"></a>Geschwindigkeit

Spieleentwickler verwenden den Begriff *Geschwindigkeit* beschrieben, wie ein Objekt verschieben – ist insbesondere wie schnell etwas verschoben werden und die Richtung, dass die It bewegt wird. 

Geschwindigkeit wird mithilfe von zwei Arten von Einheiten definiert: eine Position-Einheit und eine Zeiteinheit. Beispielsweise wird die Geschwindigkeit einer als Meilen pro Stunde oder Kilometer pro Stunde definiert. Spiele-Entwickler häufig verwenden Pixel pro Sekunde, die definieren, wie schnell ein Objekt verschoben wird. Beispielsweise kann ein Aufzählungszeichen bei einer Geschwindigkeit von 300 Pixeln pro Sekunde verschoben. D. h. ist ein Aufzählungszeichen auf 300 Pixel pro Sekunde verschieben, werden klicken Sie dann es 600 Einheiten in zwei Sekunden und 900 Einheiten in drei Sekunden usw. verschoben. Allgemeiner ausgedrückt, der Wert für die Geschwindigkeit *fügt* an die Position eines Objekts (wie weiter unten werde).

Obwohl wir Geschwindigkeit verwendet, um die Einheiten der Geschwindigkeit wird erläutert, verweist die Begriff Geschwindigkeit auf einen Wert, der unabhängig von der Richtung, in der Regel ein, und während die Laufzeit-Geschwindigkeit auf Geschwindigkeit und Richtung bezieht. Aus diesem Grund kann die Zuweisung von Aufzählungszeichen der Geschwindigkeit (vorausgesetzt, Aufzählungszeichen eine Klasse ist, enthält die erforderlichen Eigenschaften) wie folgt aussehen:


```csharp
// This bullet is not moving horizontally, so set VelocityX to 0:
bulletInstance.VelocityX = 0;
// Positive Y is "up" so move the bullet up 300 units per second:
bulletInstance.VelocityY = 300;
```


### <a name="implementing-velocity"></a>Implementieren der Geschwindigkeit

CocosSharp implementiert nicht Geschwindigkeit, sodass Objekte verschieben, ihre eigene Logik für die datenverschiebung zu implementieren müssen. Entwicklern neuer Spiele implementieren oft Geschwindigkeit machen Sie den Fehler, sodass ihre Geschwindigkeit Framerate abhängig. D. h. die folgenden *falsche Implementierung* scheinbar Geben Sie die richtigen Ergebnisse, aber auf die Bildrate des Spiels basieren:

```csharp
// VelocityX and VelocityY will be added every time this code executes
this.PositionX += this.VelocityX;
this.PositionY += this.VelocityY;
```

Wenn das Spiel an eine höhere Framerate (z. B. 60 bps und anstelle von 30 Frames pro Sekunde) ausgeführt wird, wird das Objekt angezeigt, schneller, als wenn die Ausführung an eine langsamere Framerate verschieben. Wenn das Spiel ist kann nicht verarbeitet werden Frames auf als hoch, der eine Framerate (die Hintergrundprozesse, die mithilfe des Geräts Ressourcen zurückzuführen sein kann), wird auf ähnliche Weise das Spiel angezeigt, zu einer Verlangsamung führt.

Um dies zu berücksichtigen, wird oft Geschwindigkeit implementiert einen Time-Wert verwenden. Z. B. wenn die `seconds` Variable darstellt die Anzahl (oder einen Teil davon) von Sekunden seit die letzten Zeit Geschwindigkeit angewendet wurden, und klicken Sie dann der folgende Code im Objekt müssen konsistente Bewegung unabhängig von der Einzelbildrate führen würde:

```csharp
// VelocityX and VelocityY will be added every time this code executes
this.PositionX += this.VelocityX * seconds;
this.PositionY += this.VelocityY * seconds;
```

Beachten Sie, dass ein Spiel, das auf eine niedrigere Framerate ausgeführt wird die Position der Objekte weniger häufig aktualisiert wird. Aus diesem Grund führt jedes Update in den Objekten, das Verschieben von weiteren, als wäre das Spiel wird häufiger aktualisiert wurden. Die `seconds` Wert von Konten für diese, indem melden, wie viel Zeit verstrichen ist, seit der letzten Aktualisierung.

Ein Beispiel für die Bewegung eines zeitbasierten hinzufügen, finden Sie unter [diese Anleitung deckt Zeit basierend Bewegung](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/game_development/time_based_movement).


### <a name="calculating-positions-using-velocity"></a>Berechnen der Positionen, die mit der Geschwindigkeit

Geschwindigkeit kann verwendet werden, Vorhersagen zu, wo ein Objekt nach einer gewissen Zeit übergeben werden sollen, oder um Objekte Verhalten zu optimieren, ohne das Spiel auszuführen. Beispielsweise muss ein Entwickler, der das Verschieben von ausgelösten Aufzählungszeichen implementiert die Aufzählungszeichen Geschwindigkeit festgelegt werden soll, nachdem er instanziiert wird. Die Größe des Bildschirms kann verwendet werden, um eine Basis für das Festlegen der Geschwindigkeit zu schaffen. Wenn der Entwickler weiß, dass, also die Aufzählungszeichen sollte die Höhe des Bildschirms in 2 Sekunden verschieben, und die Geschwindigkeit auf die Höhe des Bildschirms geteilt durch 2 festgelegt werden. Wenn der Bildschirm 800 Pixel hoch ist, würde dann Aufzählungszeichen der Geschwindigkeit festgelegt werden, 400 (die 800/2).

Auf ähnliche Weise müssen in-Game-Logik zum Berechnen, wie lange ein Objekt dauern wird, ein Ziel, erhält die Geschwindigkeit zu erreichen. Dies kann durch Dividieren den Abstand von der einrichteten objektspezifischen Geschwindigkeit zu folgen, berechnet werden. Der folgende Code zeigt z. B. das Zuweisen von Text in eine Bezeichnung angezeigt wie lange bis eine marschflugkörper sein Ziel erreicht:


```csharp
// We'll assume only the X axis for this example
float distanceX = target.PositionX - missile.PositionX;

float secondsToReachTarget = distanceX / missile.VelocityX;

label.Text = secondsToReachTarget + " seconds to reach target"; 
```


## <a name="acceleration"></a>Beschleunigung

*Acceleration* ist ein gängiges Konzept in die Spieleentwicklung, und es viele ähnlichkeiten mit der Geschwindigkeit gemeinsam nutzt. Acceleration misst, ob ein Objekt beschleunigen oder verlangsamen (wie Wert für die Geschwindigkeit im Laufe der Zeit ändert). Acceleration *fügt* Geschwindigkeit, genau wie Geschwindigkeit hinzugefügt, um zu positionieren. Den häufigsten Anwendungsfällen Acceleration enthalten Schwerkraft, ein Auto beschleunigen und eine Speicherplatz ausliefern, die seine Thrusters ausgelöst. 

Ähnlich wie Geschwindigkeit, Beschleunigung wird in eine Position und die Zeiteinheit definiert. Allerdings Zeiteinheit für die Beschleunigung wird als bezeichnet ein *quadratischen* Einheit, die angibt, wie Beschleunigung mathematisch definiert ist. Spiele-Beschleunigung ist, also häufig liegt diese bei *Pixeln pro Sekunde im Quadrat*.

Wenn ein Objekt eine X-Beschleunigung von 10 Einheiten pro Sekunde im Quadrat besitzt, bedeutet, dass, dass die Geschwindigkeit von 10 pro Sekunde vergrößert wird. Wenn von einem Stillstand ab, nach einer Sekunde es verschieben, um 10 Einheiten pro Sekunde, nach zwei Sekunden 20 Einheiten pro Sekunde usw.

Acceleration in zwei Dimensionen ist eine X- und Y-Komponente, erforderlich, damit es wie folgt zugewiesen werden kann:


```csharp
// No horizontal acceleration:
icicle.AccelerationX = 0;
// Simulate gravity with Y acceleration. Negative Y is down, so assign a negative value:
icicle.AccelerationY = -50; 
```


### <a name="acceleration-vs-deceleration"></a>Beschleunigung und Verlangsamung

Beschleunigung und Verlangsamung manchmal in täglich Sprache unterscheiden, zwar gibt es keinen technischen Unterschied zwischen den beiden. Schwerkraft ist eine Kraft, was zu einer Beschleunigung führt. Wenn ein Objekt, das nach oben ausgelöst wird Klicken Sie dann Schwerkraft wird verlangsamen sie verlangsamen (verwendet wird), aber sobald das Objekt wurde beendet climbing und wird in die gleiche Richtung wie der Schwerkraft fallen dann Schwerkraft ist beschleunigen es (beschleunigt). Wie unten dargestellt, ist die Anwendung von einer Beschleunigung identisch, ob es in die gleiche Richtung bzw. das Gegenteil der Richtung der datenverschiebung angewendet wird. 


### <a name="implementing-acceleration"></a>Implementieren die Hardwarebeschleunigung

Beschleunigung ist ähnlich wie Geschwindigkeit, bei der Implementierung – es wird nicht automatisch von CocosSharp implementiert, und zeitbasierte Beschleunigung ist die gewünschte Implementierung (im Gegensatz zu framebasierte Acceleration). Aus diesem Grund kann eine einfache Acceleration (zusammen mit der Geschwindigkeit) Implementierung formuliert werden:

```csharp
this.VelocityX += this.AccelerationX * seconds;
this.VelocityY += this.AccelerationY * seconds;
this.PositionX += this.VelocityX * seconds;
this.PositionY += this.VelocityY * seconds;
```

Der obige Code ist als bezeichnet ein *linearen Näherung* für Beschleunigung-Implementierung. Effektiv implementiert Beschleunigung mit einem ziemlich nahe Grad an Genauigkeit, aber es ist dabei nicht um ein vollkommen exakt Modell der Beschleunigung. Er ist über enthalten, um zu erläutern das Konzept der Implementierung von Beschleunigung.

Die folgende Implementierung ist ein mathematisch korrekt-Anwendung die Beschleunigung und Geschwindigkeit:


```csharp
float halfSecondsSquared = (seconds * seconds) / 2.0f;

this.PositionX += 
    this.Velocity.X * seconds + this.AccelerationX * halfSecondsSquared;
this.PositionY += 
    this.Velocity.Y * seconds + this.AccelerationY * halfSecondsSquared;

this.VelocityX += this.AccelerationX * seconds;
this.VelocityY += this.AccelerationY * seconds; 
```

Der offensichtlichste Unterschied zu den obigen Code wird die `halfSecondsSquared` Variablen und ihrer Verwendung anzuwendende Beschleunigung zu positionieren. Der mathematische Grund dafür ist, würde den Rahmen dieses Lernprogramms, aber Entwickler mit Interesse an der Mathematik dahinter finden weitere Informationen zur [Diskussion über die Integration von Beschleunigung.](http://www.cliffsnotes.com/math/calculus/calculus/integration/distance-velocity-and-acceleration)

Die praktischen Auswirkungen `halfSecondSquare` besteht darin, dass die Hardwarebeschleunigung unabhängig von der Einzelbildrate mathematisch genau und zuverlässig Verhalten. Die lineare Näherung der Beschleunigung unterliegt den Framerate – je niedriger die Framerate umso ungenauer Angleichung wird gelöscht. Mithilfe von `halfSecondsSquared` wird sichergestellt, dass der Code unabhängig von der Framerate verhält.


## <a name="angles-and-rotation"></a>Winkel und Drehung

Visuellen Objekte wie z. B. `CCSprite` unterstützen Drehung über einen `Rotation` Variable. Dies kann auf einen Wert zugewiesen werden, können Sie die Drehung in Grad festlegen. Der folgende Code zeigt z. B. wie Sie eine `CCSprite` Instanz:


```csharp
CCSprite unrotatedSprite = new CCSprite("star.png");
unrotatedSprite.IsAntialiased = false;
unrotatedSprite.PositionX = 100;
unrotatedSprite.PositionY = 100;
this.AddChild (unrotatedSprite);

CCSprite rotatedSprite = new CCSprite("star.png");
rotatedSprite.IsAntialiased = false;
// This sprite is moved to the right so it doesn’t overlap the first
rotatedSprite.PositionX = 130;
rotatedSprite.PositionY = 100;
rotatedSprite.Rotation = 45;
this.AddChild (rotatedSprite); 
```

Dies hat folgende Auswirkungen:

![](math-images/image1.png "Dies führt in diesem screenshot")

Beachten Sie, dass die Drehung um 45 Grad im Uhrzeigersinn (der verlaufsbezogenen Gründen ist das Gegenteil des wie Drehung mathematisch angewendet wird):

![](math-images/image2.png "Beachten Sie, dass die Drehung um 45 Grad im Uhrzeigersinn, die aus historischen Gründen ist das Gegenteil des wie Drehung mathematisch angewendet wird")

Im Allgemeinen kann Rotation für CocosSharp und reguläre Mathematik wie folgt visuell dargestellt werden:

![](math-images/image3.png "Im Allgemeinen kann Rotation für CocosSharp und reguläre Mathematik wie folgt visuell dargestellt werden")

![](math-images/image4.png "Im Allgemeinen kann Rotation für CocosSharp und reguläre Mathematik wie folgt visuell dargestellt werden")

Diese Unterscheidung ist wichtig, da die `System.Math` Klasse verwendet die Drehung gegen den Uhrzeigersinn, daher müssen Entwickler vertraut sind, mit dieser Klasse Winkel umkehren, bei der Arbeit mit `CCNode` Instanzen.

Wir sollten beachten, dass es sich bei die oben genannten Diagrammen Drehung in Grad anzuzeigen; allerdings einige mathematischen Funktionen (z. B. die Funktionen in der `System.Math` Namespace) erwarten und Zurückgeben von Werten in *Bogenmaß* statt Grad. Wir werden wie für die Konvertierung zwischen den beiden Typen weiter unten in diesem Handbuch betrachten.


### <a name="rotating-to-face-a-direction"></a>Um eine Richtung drehen

Wie oben gezeigt `CCSprite` können gedreht werden, mithilfe der `Rotation` Eigenschaft. Die `Rotation` Eigenschaft wird bereitgestellt, indem `CCNode` (die Basisklasse für `CCSprite`), womit Drehung kann angewendet werden, um Entitäten, die von erben `CCNode` auch. 

Einige Spiele erfordern Objekte gedreht werden soll, damit sie ein Ziel auftreten. Beispiele hierfür sind eine Computer gesteuerte Gegner an ein Ziel Player oder ein Speicherplatz liefern, die für den Punkt, in dem der Benutzer den Bildschirm berührt, fliegen behandeln. Allerdings muss ein Drehungswert zuerst basierend auf den Speicherort der rotierenden Entität und den Speicherort des Ziels auf berechnet werden.

Dieser Prozess erfordert eine Reihe von Schritten:

 - Identifizieren der *Offset* des Ziels. Offset bezieht sich auf den X- und Y Abstand zwischen der Drehen von Entität und die Zielentität.
 - Den Winkel zwischen der Offset wird mithilfe der Arkustangens trigonometrische Funktion (unten im Detail erläutert) berechnet.
 - Anpassen der für einen Unterschied zwischen 0 Grad auf der rechten Seite und die Richtung, die die Drehen von Entität verweist, bei nicht gedreht.
 - Anpassen der für den Unterschied zwischen mathematische Drehung (gegen den Uhrzeigersinn) und CocosSharp Drehung (im Uhrzeigersinn).

Die folgende Funktion (vorausgesetzt, in einer Entität geschrieben werden soll) dreht es sich um die Entität, einem Ziel auftreten:


```csharp
// This function assumes that it is contained in a CCNode-inheriting object
public void FacePoint(float targetX, float targetY)
{
    // Calculate the offset - the target's position relative to "this"
    float xOffset = targetX - this.PositionX;
    float yOffset = targetY - this.PositionY;

    // Make sure the target isn't the same point as "this". If so,
    // then rotation cannot be calculated.
    if (targetX != this.PositionX || targetY != this.Position.Y)
    {

        // Call Atan2 to get the radians representing the angle from 
        // "this" to the target
        float radiansToTarget = (float)System.Math.Atan2 (yOffset, xOffset);

        // Since CCNode uses degrees for its rotation, we need to convert
        // from radians
        float degreesToTarget = CCMathHelper.ToDegrees (radiansToTarget);

        // The direction that the entity faces when unrotated. In this case
        // the entity is facing "up", which is 90 degrees 
        const float forwardAngle = 90;

        // Adjust the angle we want to rotate by subtracting the
        // forward angle.
        float adjustedForDirecitonFacing = degreesToTarget - forwardAngle;

        // Invert the angle since CocosSharp uses clockwise rotation
        float cocosSharpAngle = adjustedForDirecitonFacing * -1;

        // Finally assign the rotation
        this.Rotation = rotation = cocosSharpAngle;
    }
} 
```

Der obige Code könnte eine Entität zu rotieren, damit sie den Punkt steht, in denen der Benutzer wie folgt den Bildschirm berührt, verwendet werden:


```csharp
private void HandleInput(System.Collections.Generic.List<CCTouch> touches, CCEvent touchEvent)
{
    if(touches.Count > 0)
    {
        CCTouch firstTouch = touches[0];
        FacePoint (firstTouch.Location.X, firstTouch.Location.Y);
    }
} 
```

Dieser Code führt zu folgendem Verhalten:

![](math-images/image5.gif "Dieser Code löst dieses Verhalten")

#### <a name="using-atan2-to-convert-offsets-to-angles"></a>Konvertieren mithilfe von "ATAN2" offsets Winkel

`System.Math.Atan2` kann verwendet werden, um einen Offset in einen Winkel zu konvertieren. Der Funktionsname `Atan2` stammen aus den Arkustangens trigonometrische Funktion. Das Suffix "2" wird diese Funktion unterscheidet, aus dem Standard `Atan` -Funktion, die das mathematische Verhalten der Arkustangens genau übereinstimmt. Arkustangens ist eine Funktion, die zwischen-90 und einen Wert zurückgibt und + 90 Grad (oder die Entsprechung im Bogenmaß). Viele Anwendungen, einschließlich Computerspiele erfordern häufig eine vollständige 360 Grad von Werten, sodass die `Math` Klasse enthält `Atan2` auf diese Anforderung zu erfüllen.

Beachten Sie, dass der obige Code dem Y-Parameter zunächst den X-Parameter dann beim Aufrufen übergibt der `Atan2` Methode. Dies ist rückwärts von den üblichen X, Y-Reihenfolge von Positionskoordinaten. Weitere Informationen [finden Sie in der Dokumentation "ATAN2"](https://msdn.microsoft.com/library/system.math.atan2(v=vs.110).aspx).

Es ist auch erwähnenswert, dass der Rückgabewert von `Atan2` ist im Bogenmaß im Bereich einer anderen Einheit verwendet, um Winkel zu messen. Dieses Handbuch behandelt die Details im Bogenmaß nicht, aber denken Sie daran, alle trigonometrischen Funktionen in der `System.Math` Namespace verwenden von Radians, damit alle Werte in Grad konvertiert werden müssen, bevor Sie für CocosSharp-Objekte verwendet werden. Weitere Informationen zu Bogenmaß finden [in der Wikipedia-Seite Radiant](https://en.wikipedia.org/wiki/Radian).

#### <a name="forward-angle"></a>Forward-Winkel

Sobald die `FacePoint` -Methode konvertiert Winkels in Bogenmaß (Radiant), wird eine `forwardAngle` Wert. Dieser Wert stellt den Winkel, in dem die Entität mit Internetzugriff wird bei der Drehungswert gleich 0 ist. In diesem Beispiel wird davon ausgegangen, dass die Entität nach oben, die um 90 Grad ist zeigt, wenn Sie eine mathematische Drehung (im Gegensatz zu CocosSharp-Drehung) verwenden. Wir verwenden die mathematische Drehung hier, da wir noch die Rotation für CocosSharp invertiert nicht getan haben.

Das folgende Beispiel zeigt, welche eine Entität mit einem `forwardAngle` von 90 Grad könnte folgendermaßen aussehen:

![](math-images/image6.png "Dies zeigt, wie eine Entität mit einem ForwardAngle von 90 Grad aussehen könnte")


### <a name="angled-velocity"></a>Abgewinkelter Geschwindigkeit

Bisher haben wir uns konvertieren Sie einen Offset in einen Winkel angesehen haben. In diesem Abschnitt wird die andere Richtung – einen Winkel und konvertiert sie in X und Y-Werte. Häufige Beispiele sind ein Verschieben in die Richtung, bei denen es ist, oder ein Speicherplatz ausliefern, Schießen ein Aufzählungszeichen aus dem in der Richtung verschoben wird, bei denen die Lieferadresse ist Auto. 

Geschwindigkeit kann im Prinzip berechnet werden, vom ersten definieren die gewünschte Geschwindigkeit aus, wenn nicht gedreht, und klicken Sie dann drehen, Geschwindigkeit aus, um den Winkel an, bei denen eine Entität ist. Um dieses Konzept zu erklären, Geschwindigkeit (und Acceleration) als ein 2-dimensionalen visualisiert werden können *Vektor* (die als Pfeil in der Regel gezeichnet wird). Ein Vektor für den Wert einer Geschwindigkeit, mit X = 100 "und" Y = 0 kann wie folgt visuell dargestellt werden:

![](math-images/image7.png "Ein Vektor für den Wert einer Geschwindigkeit, mit X = 100 \"und\" Y = 0 kann wie folgt visuell dargestellt werden")

Dieser Vektor kann gedreht werden, um eine neue Geschwindigkeit führen. Z. B. den Vektor (mit gegen den Uhrzeigersinn) um 45 Grad drehen hat folgende Auswirkungen:

![](math-images/image8.png "Den Vektor Drehen um 45 Grad gegen den Uhrzeigersinn Ergebnisse in diesem mit")

Glücklicherweise können Geschwindigkeit, Beschleunigung und sogar Position alle gedreht werden mit dem gleichen Code unabhängig davon, wie die Werte angewendet werden. Die folgende allgemeine Funktion kann durch einen CocosSharp Drehung Wert zum Drehen eines Vektors verwendet werden:


```csharp
// Rotates the argument vector by degrees specified by
// cocosSharpDegrees. In other words, the rotation
// value is expected to be clockwise.
// The vector parameter is modified, so it is both an in and out value
void RotateVector(ref CCVector2 vector, float cocosSharpDegrees)
{
    // Invert the rotation to get degrees as is normally
    // used in math (counterclockwise)
    float mathDegrees = -cocosSharpDegrees;

    // Convert the degrees to radians, as the System.Math
    // object expects arguments in radians
    float radians = CCMathHelper.ToRadians (mathDegrees);

    // Calculate the "up" and "right" vectors. This is essentially
    // a 2x2 matrix that we'll use to rotate the vector
    float xAxisXComponent = (float)System.Math.Cos (radians);
    float xAxisYComponent = (float)System.Math.Sin (radians);
    float yAxisXComponent = (float)System.Math.Cos (radians + CCMathHelper.Pi / 2.0f);
    float yAxisYComponent = (float)System.Math.Sin (radians + CCMathHelper.Pi / 2.0f);

    // Store the original vector values which will be used
    // below to perform the final operation of rotation.
    float originalX = vector.X;
    float originalY = vector.Y;

    // Use the axis values calculated above (the matrix values)
    // to rotate and assign the vector.
    vector.X = originalX * xAxisXComponent + originalY * yAxisXComponent;
    vector.Y = originalX * xAxisYComponent + originalY * yAxisYComponent;
} 
```

Einen vollständigen Überblick über die `RotateVector` Methode muss mit dem Kosinus und Sinus trigonometrischen Funktionen zusammen mit einigen lineare Algebra, die den Rahmen dieses Artikels sprengen. Jedoch nach der Implementierung der `RotateVector` Methode kann verwendet werden, um vektorgrößenbeschränkungen, einschließlich einer Geschwindigkeitsvektors drehen. Z. B. der folgende Code möglicherweise ausgelöst, Aufzählungszeichen in eine Richtung, die gemäß der `rotation` Wert:


```csharp
// Create a Bullet instance
Bullet newBullet = new Bullet();

// Define the velocity of the bullet when 
// rotation is 0
CCVector2 velocity = new CCVector2 (0, 100);

// Modify the velocity according to rotation
RotateVector (ref velocity, rotation);

// Assign the newBullet's velocity using the
// rotated vector
newBullet.VelocityX = velocity.X;
newBullet.VelocityY = velocity.Y;

// Set the bullet's rotation so it faces
// the direction that it's flying
newBullet.Rotation = rotation; 
```

Dieser Code kann etwa erzeugt:

![](math-images/image9.png "Dieser Code kann in etwa diesen Screenshot erstellen")


## <a name="summary"></a>Zusammenfassung

Dieser Leitfaden behandelt allgemeine mathematische Konzepte in 2D-Spiele-Entwicklung. Es wird gezeigt, wie zugewiesen, und Implementieren von Geschwindigkeit und Beschleunigung und beschrieben, wie Objekte und Vektoren für das Verschieben in eine beliebige Richtung drehen.
