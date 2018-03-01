---
title: 2D mathematischen Funktionen mit CocosSharp
description: "Dieser Leitfaden behandelt, für die Entwicklung von 2D Mathematik. Er CocosSharp zeigen, wie allgemeine Aufgaben zur 3D-Spielentwicklung verwendet und erläutert die mathematischen hinter dieser Aufgaben."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5C241AB4-F97E-4B61-B93C-F5D307BCD517
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 7573ca423c3d9462d400f117c2116209e7c2a410
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="2d-math-with-cocossharp"></a>2D mathematischen Funktionen mit CocosSharp

_Dieser Leitfaden behandelt, für die Entwicklung von 2D Mathematik. Er CocosSharp zeigen, wie allgemeine Aufgaben zur 3D-Spielentwicklung verwendet und erläutert die mathematischen hinter dieser Aufgaben._

Positionieren und Verschieben von Objekten mit Code die ist ein zentraler Bestandteil der Entwicklung von Spielen beliebiger Größe aus. Positionieren und verschieben erfordern die Verwendung von Mathematik an, ob ein Spiel erfordert das Verschieben eines Objekts entlang einer geraden Linie oder die Verwendung von trigonometrische für Drehung. Dieses Dokument befasst sich mit die folgenden Themen:

 - Geschwindigkeit
 - Acceleration
 - Drehen von CocosSharp-Objekte
 - Verwenden von Drehung mit Geschwindigkeit

Entwickler, die kein sicheres mathematische Hintergrund besitzen, oder, die lange-vergessen haben diese Themen Schule, erübrigt sich Gedanken machen – dieses Dokument wird Gliedern Konzepte Ansatzpunkt Größe Teile und theoretische erläuterungen mit praktischen Beispielen begleitet werden. Kurz gesagt, in diesem Artikel wird die Frage beantwortet wird uralte mathematische Student: "Wenn ich tatsächlich müssen diese Dinge verwenden?"


# <a name="requirements"></a>Anforderungen

Zwar in erster Linie auf der mathematischen CocosSharp-Seite dieses Dokument konzentriert sich, vorausgesetzt Codebeispiele arbeiten mit Objekten erben Formular `CCNode`. Darüber hinaus seit `CCNode` enthält keine Werte für Geschwindigkeit und Beschleunigung der Code geht davon aus arbeiten mit Entitäten an, die Werte, z. B. VelocityX, VelocityY AccelerationX und AccelerationY bereitzustellen. Weitere Informationen zu Entitäten, finden Sie unter exemplarischen auf [Entitäten in CocosSharp](~/graphics-games/cocossharp/entities.md).


# <a name="velocity"></a>Geschwindigkeit

Entwickler verwenden den Begriff *Geschwindigkeit* beschreiben, wie ein Objekt gleitenden – ist insbesondere wie schnell schließt ein Element verschoben werden und die Richtung, dass die It bewegt wird. 

Geschwindigkeit wird mithilfe von zwei Arten von Einheiten definiert: eine Position Einheit und eine Zeiteinheit. Beispielsweise wird eine Car-Geschwindigkeit als pro Stunde Meilen oder Kilometer pro Stunde definiert. Spiel Entwickler häufig verwenden Pixel pro Sekunde, die definieren, wie schnell ein Objekt bewegt. Aufzählungszeichen bewegen kann z. B. mit einer Geschwindigkeit von 300 Pixel pro Sekunde. D. h. Wenn Aufzählungszeichen auf 300 Pixel pro Sekunde verschoben werden, wird dann er 600 Einheiten in zwei Sekunden und 900 Einheiten in drei Sekunden usw. verschoben. Allgemeiner gesagt ist der Wert für die Geschwindigkeit *fügt* auf die Position eines Objekts (wie im folgenden ersichtlich).

Obwohl wir Geschwindigkeit verwendet, um die Einheiten der Geschwindigkeit zu erläutern, verweist die Begriff Geschwindigkeit auf einen Wert, der unabhängig von der Richtung, in der Regel ein, und während die Begriff Geschwindigkeit auf Geschwindigkeit und Richtung verweist. Aus diesem Grund kann die Zuweisung von einem Aufzählungszeichen Geschwindigkeit (vorausgesetzt, Aufzählungszeichen eine Klasse ist, enthält die erforderlichen Eigenschaften) sieht wie folgt aus:


```csharp
// This bullet is not moving horizontally, so set VelocityX to 0:
bulletInstance.VelocityX = 0;
// Positive Y is "up" so move the bullet up 300 units per second:
bulletInstance.VelocityY = 300;
```


## <a name="implementing-velocity"></a>Implementieren der Geschwindigkeit

CocosSharp implementiert nicht Geschwindigkeit, damit Objekte, die Bewegung erfordern ihre eigene Logik Bewegung implementieren müssen. Neue Spiel Entwickler implementierende Geschwindigkeit oftmals machen Sie den Fehler, sodass ihre Geschwindigkeit Framerate abhängig. D. h. die folgenden *falsche Implementierung* anscheinend um richtige Ergebnisse bereitzustellen, aber als Basis für das Spiel Framerate wird:

```csharp
// VelocityX and VelocityY will be added every time this code executes
this.PositionX += this.VelocityX;
this.PositionY += this.VelocityY;
```

Wenn das Spiel an eine höhere Framerate (z. B. 60 Frames pro Sekunde anstelle von 30 Frames pro Sekunde) ausgeführt wird, wird das Objekt angezeigt, so verschieben Sie schneller als bei einer langsameren Framerate ausgeführt wird. Wenn das Spiel kann nicht Frames am als eine Framerate hohen verarbeitet werden (die durch Hintergrundprozesse, die mit dem Gerät Ressourcen verursacht werden kann), wird auf ähnliche Weise das Spiel angezeigt, verlangsamt werden.

Um dies zu berücksichtigen, wird Geschwindigkeit häufig implementiert einen Zeitwert. Beispielsweise, wenn die `seconds` Variable stellt die Anzahl (oder Bruch) Sekunden seit die letzten Zeit Geschwindigkeit angewendet wurde, und klicken Sie dann der folgende Code Objekts bedingt, dessen konsistent Bewegung unabhängig von der Framerate:

```csharp
// VelocityX and VelocityY will be added every time this code executes
this.PositionX += this.VelocityX * seconds;
this.PositionY += this.VelocityY * seconds;
```

Beachten Sie, dass ein Spiel der Frame langsamer ausgeführt wird, das die Position der Objekte kleiner häufig aktualisiert. Aus diesem Grund führt jedes Update in den Objekten verschieben weiter als auch der Fall ist, wenn das Spiel häufiger aktualisiert haben. Die `seconds` Wert Konten hierzu melden, wie viel Zeit verstrichen ist, seit der letzten Aktualisierung.

Ein Beispiel zum Verschieben von zeitbasierten hinzufügen, finden Sie unter [diese Anleitung abdecken Zeit basierten verschieben](https://developer.xamarin.com/recipes/cross-platform/game_development/time_based_movement/).


## <a name="calculating-positions-using-velocity"></a>Berechnen der Geschwindigkeit mit Positionen

Geschwindigkeit kann verwendet werden, in der Vorhersage zu, in dem ein Objekt nach einer gewissen Zeit werden, oder für die Objekte Verhalten zu optimieren, ohne das Spiel auszuführen. Beispielsweise muss ein Entwickler, der die Übertragung der protokollsicherungsdaten ausgelöst Aufzählungszeichen implementiert das Aufzählungszeichen Geschwindigkeit festgelegt, nachdem er instanziiert wird. Eine Grundlage zum Festlegen der Geschwindigkeit bereitstellen, kann die Größe des Bildschirms verwendet werden. Wenn der Entwickler informiert wird, die das Aufzählungszeichen sollten die Höhe des Bildschirms in 2 Sekunden verschieben, wird die Geschwindigkeit sollte festgelegt werden, der die Höhe des Bildschirms geteilt durch 2. Wenn der Bildschirm 800 Pixel hoch ist, würde das Aufzählungszeichen Geschwindigkeit festgelegt werden, und 400 (also 800/2).

Auf ähnliche Weise müssen im Spiel Logik berechnen, wie lange ein Objekt dauern wird, um ein Ziel, erhält die Geschwindigkeit zu erreichen. Dies kann durch Dividieren des Abstands von der mobilen objektspezifischen Geschwindigkeit übertragen berechnet werden. Der folgende Code zeigt z. B. das Zuweisen von Text zu einer Bezeichnung wie lange angezeigt, bis ein Problem ihr Ziel erreicht:


```csharp
// We'll assume only the X axis for this example
float distanceX = target.PositionX - missile.PositionX;

float secondsToReachTarget = distanceX / missile.VelocityX;

label.Text = secondsToReachTarget + " seconds to reach target"; 
```


# <a name="acceleration"></a>Acceleration

*Acceleration* ist eine allgemeine Konzept in 3D-Spielentwicklung und, die viele ähnlichkeiten mit der Geschwindigkeit geteilt. Acceleration misst, ob ein Objekt beschleunigen oder verlangsamen (wie der Wert der Geschwindigkeit des im Zeitverlauf ändert). Acceleration *fügt* Geschwindigkeit, genau wie Geschwindigkeit hinzugefügt, um zu positionieren. Allgemeine anwendungsfallbeispiele der Beschleunigung Schwerpunkt, ein Auto beschleunigen und eine Speicherplatz Lieferadresse seine Thrusters auslösen. 

Ähnlich wie Geschwindigkeit ist Beschleunigung in eine Position und eine Zeiteinheit definiert. Allerdings Beschleunigung der Zeiteinheit wird als bezeichnet eine *quadratischen* Einheit, die angibt, wie Beschleunigung mathematisch definiert ist. D. h. Spiel Beschleunigung in oft gemessen wird *Pixel pro Sekunde Quadrat*.

Wenn ein Objekt über ein X Beschleunigung von 10 Einheiten pro Sekunde, die das Quadrat verfügt, bedeutet, dass, die Geschwindigkeit von 10 pro Sekunde erhöht. Wenn von einem Stillstand ab, nach einer Sekunde es verschieben, um 10 Einheiten pro Sekunde, nach zwei Sekunden 20 Einheiten pro Sekunde usw.

Acceleration in zwei Dimensionen erfordert eine X- und Y-Komponente, daher wie folgt zugewiesen werden kann:


```csharp
// No horizontal acceleration:
icicle.AccelerationX = 0;
// Simulate gravity with Y acceleration. Negative Y is down, so assign a negative value:
icicle.AccelerationY = -50; 
```


## <a name="acceleration-vs-deceleration"></a>Acceleration im Vergleich zu Verzögerung

Obwohl Beschleunigung und Verzögerung manchmal in täglich Sprache unterscheiden, ist gibt es keine technische Unterschied zwischen den beiden. Schwerpunkt ist ein Force Acceleration vortäuschen. Wenn ein Objekt, das nach oben ausgelöst wird Klicken Sie dann Schwerpunkt verlangsamt es (verlangsamen), aber um, sobald das Objekt wird nicht mehr climbing und ist in der gleichen Richtung wie Schwerpunkt fallen dann Schwerpunkt ist beschleunigen sie (beschleunigt). Wie unten dargestellt, ist die Anwendung eine Beschleunigung, ob es in die gleiche Richtung oder die umgekehrte Richtung des Verschiebens angewendet wird. 


## <a name="implementing-acceleration"></a>Acceleration implementieren

Acceleration ist ähnlich wie Geschwindigkeit bei der Implementierung – es wird nicht automatisch durch CocosSharp implementiert, und zeitbasierte Acceleration ist die gewünschte Implementierung (im Gegensatz zu framebasierte Acceleration). Aus diesem Grund kann eine einfache Acceleration (zusammen mit der Geschwindigkeit) Implementierung formuliert werden:

```csharp
this.VelocityX += this.AccelerationX * seconds;
this.VelocityY += this.AccelerationY * seconds;
this.PositionX += this.VelocityX * seconds;
this.PositionY += this.VelocityY * seconds;
```

Der obige Code wird als bezeichnet ist ein *lineare Näherung* für Beschleunigung-Implementierung. Effektiv, Acceleration mit einer engen Grad an Genauigkeit implementiert, es ist jedoch keine perfekte genaues Modell der Beschleunigung. Um besser zu erläutern das Konzept der Implementierung von Acceleration ist oben enthalten.

Die folgende Implementierung handelt es sich um eine mathematisch korrekt Anwendung Beschleunigung und Geschwindigkeit:


```csharp
float halfSecondsSquared = (seconds * seconds) / 2.0f;

this.PositionX += 
    this.Velocity.X * seconds + this.AccelerationX * halfSecondsSquared;
this.PositionY += 
    this.Velocity.Y * seconds + this.AccelerationY * halfSecondsSquared;

this.VelocityX += this.AccelerationX * seconds;
this.VelocityY += this.AccelerationY * seconds; 
```

Der offensichtlichste Unterschied zu den oben aufgeführten Code wird die `halfSecondsSquared` Variablen und ihre Verwendung anzuwendende Acceleration zu positionieren. Die mathematische Grund dafür ist, den Rahmen dieses lehrprogramms sprengen, aber Entwickler die mathematischen dafür interessiert finden weitere Informationen in [diese Diskussion zur Beschleunigung Integration.](http://www.cliffsnotes.com/math/calculus/calculus/integration/distance-velocity-and-acceleration)

Die praktische Auswirkung `halfSecondSquare` darin, dass Acceleration unabhängig von der Framerate mathematisch genau und erwartungsgemäß verhält. Die lineare Näherung der Beschleunigung unterliegt Framerate – je niedriger löscht die Framerate, desto weniger genau ist die Näherung ab. Mit `halfSecondsSquared` wird sichergestellt, dass der Code unabhängig von der Framerate identisch verhält.


# <a name="angles-and-rotation"></a>Winkel und Drehung

Visuelle Objekte wie z. B. `CCSprite` unterstützen Drehung über eine `Rotation` Variable. Dies kann auf einen Wert zugewiesen werden, die Drehung in Grad festgelegt. Der folgende Code zeigt z. B. Gewusst wie: Drehen eine `CCSprite` Instanz:


```csharp
CCSprite unrotatedSprite = new CCSprite("star.png");
unrotatedSprite.IsAntialiased = false;
unrotatedSprite.PositionX = 100;
unrotatedSprite.PositionY = 100;
this.AddChild (unrotatedSprite);

CCSprite rotatedSprite = new CCSprite("star.png");
rotatedSprite.IsAntialiased = false;
// This sprite is moved to the right so it doesn’t overlap the first
rotatedSprite.PositionX = 130;
rotatedSprite.PositionY = 100;
rotatedSprite.Rotation = 45;
this.AddChild (rotatedSprite); 
```

Daraus ergibt sich Folgendes:

![](math-images/image1.png "Dies führt in diesem screenshot")

Beachten Sie, dass die Drehung um 45 Grad im Uhrzeigersinn (das Verlaufsgründen ist das Gegenteil des wie Drehung mathematisch angewendet wird):

![](math-images/image2.png "Beachten Sie, dass die Drehung um 45 Grad im Uhrzeigersinn aus Verlaufsgründen ist das Gegenteil des wie Drehung mathematisch angewendet wird")

Im Allgemeinen kann Rotation für CocosSharp und reguläre Mathematik wie folgt visuell dargestellt werden:

![](math-images/image3.png "Im Allgemeinen kann Rotation für CocosSharp und reguläre Mathematik wie folgt visuell dargestellt werden")

![](math-images/image4.png "Im Allgemeinen kann Rotation für CocosSharp und reguläre Mathematik wie folgt visuell dargestellt werden")

Diese Unterscheidung ist wichtig, da die `System.Math` Klasse gegen den Uhrzeigersinn drehen verwendet, daher müssen Entwickler vertraut sind mit dieser Klasse Winkel umkehren, bei der Arbeit mit `CCNode` Instanzen.

Es sollten beachten, dass die oben aufgeführten Diagramme Drehung in Grad angezeigt; allerdings einige mathematischen Funktionen (z. B. die Funktionen in der `System.Math` Namespace) erwarten und Rückgabewerte in *Bogenmaß* statt Grad. Sehen wir uns Gewusst wie: Konvertieren zwischen den zwei Einheitentypen etwas weiter unten in diesem Handbuch.


## <a name="rotating-to-face-a-direction"></a>Um eine Richtung zeigen drehen

Wie oben gezeigt `CCSprite` können gedreht werden, mithilfe der `Rotation` Eigenschaft. Die `Rotation` Eigenschaft wird bereitgestellt, indem Sie `CCNode` (die Basisklasse für `CCSprite`), womit der Drehung kann angewendet werden, um Entitäten an, die von erben `CCNode` auch. 

Einige Spiele erfordern Objekte rotiert werden soll, damit sie ein Ziel sind. Beispiele hierfür sind eine Computer gesteuerte Feind an ein Ziel Player oder ein Speicherplatz liefern, die für den Punkt, in dem der Benutzer den Bildschirm berührt, fliegen behandeln. Allerdings muss ein Drehungswert zuerst basierend auf den Speicherort der rotierenden Entität und den Speicherort des Ziels auf stoßen, berechnet werden.

Dieser Prozess erfordert eine Reihe von Schritten:

 - Identifizieren der *Offset* des Ziels. Offset bezieht sich auf den X- und Y-Abstand zwischen rotierenden Entität und der Zielentität.
 - Berechnet den Winkel ab dem Offset mithilfe der Arkustangens trigonometrische Funktion (Dies wird nachfolgend detailliert erläutert).
 - Anpassen der für einen Unterschied zwischen 0 Grad auf der rechten Seite und die Richtung, die Drehen von Entität verweist, wenn nicht gedreht.
 - Anpassen der für den Unterschied zwischen mathematische Drehung (gegen den Uhrzeigersinn) und CocosSharp Drehung (im Uhrzeigersinn).

Die folgende Funktion (davon ausgegangen, dass in einer Entität geschrieben werden) wird die Entität aus, um ein Ziel sind:


```csharp
// This function assumes that it is contained in a CCNode-inheriting object
public void FacePoint(float targetX, float targetY)
{
    // Calculate the offset - the target's position relative to "this"
    float xOffset = targetX - this.PositionX;
    float yOffset = targetY - this.PositionY;

    // Make sure the target isn't the same point as "this". If so,
    // then rotation cannot be calculated.
    if (targetX != this.PositionX || targetY != this.Position.Y)
    {

        // Call Atan2 to get the radians representing the angle from 
        // "this" to the target
        float radiansToTarget = (float)System.Math.Atan2 (yOffset, xOffset);

        // Since CCNode uses degrees for its rotation, we need to convert
        // from radians
        float degreesToTarget = CCMathHelper.ToDegrees (radiansToTarget);

        // The direction that the entity faces when unrotated. In this case
        // the entity is facing "up", which is 90 degrees 
        const float forwardAngle = 90;

        // Adjust the angle we want to rotate by subtracting the
        // forward angle.
        float adjustedForDirecitonFacing = degreesToTarget - forwardAngle;

        // Invert the angle since CocosSharp uses clockwise rotation
        float cocosSharpAngle = adjustedForDirecitonFacing * -1;

        // Finally assign the rotation
        this.Rotation = rotation = cocosSharpAngle;
    }
} 
```

Eine Entität drehen, sodass den Punkt gewinnen, in denen der Benutzer wie folgt den Bildschirm berührt, konnte der obige Code verwendet werden:


```csharp
private void HandleInput(System.Collections.Generic.List<CCTouch> touches, CCEvent touchEvent)
{
    if(touches.Count > 0)
    {
        CCTouch firstTouch = touches[0];
        FacePoint (firstTouch.Location.X, firstTouch.Location.Y);
    }
} 
```

Dieser Code führt zu folgendem Verhalten:

![](math-images/image5.gif "Dieser Code führt zu diesem Verhalten")

### <a name="using-atan2-to-convert-offsets-to-angles"></a>Verwenden zum Konvertieren von Offsets in Winkel Atan2
`System.Math.Atan2` kann verwendet werden, um ein Offset in einen Winkel zu konvertieren. Der Funktionsname `Atan2` stammen aus den Arkustangens trigonometrische Funktion. Das Suffix "2" wird diese Funktion unterscheidet, vom Standard `Atan` -Funktion, die das mathematische Verhalten des Arkustangens streng entspricht. Arkustangens ist eine Funktion, die zwischen-90 und einen Wert zurückgibt und bis + 90 Grad (oder der entsprechende im Bogenmaß). Viele Clientanwendungen, einschließlich Spiele, erfordern häufig eine vollständige 360 Grad der Werte, sodass der `Math` Klasse enthält `Atan2` auf diese Anforderung zu erfüllen.

Beachten Sie, dass der obige Code Y-Parameter zunächst den X-Parameter dann beim Aufrufen übergibt der `Atan2` Methode. Dies ist rückwärts von den üblichen X, Y Sortierung der Positionskoordinaten. Weitere Informationen [finden Sie unter Atan2 Dokumente](https://msdn.microsoft.com/en-us/library/system.math.atan2(v=vs.110).aspx).

Es ist auch Folgendes zu beachten, dass der Rückgabewert von `Atan2` wird im Bogenmaß, also von einer anderen Einheit verwendet, um den Winkel zu messen. Dieses Handbuch behandelt die Details der Bogenmaß nicht bietet, aber beachten Sie, alle trigonometrischen Funktionen in der `System.Math` Namespace verwenden von Radians, damit alle Werte in Grad konvertiert werden müssen, bevor Sie für CocosSharp Objekte verwendet werden. Weitere Informationen zu Bogenmaß verwendbaren [in Radiant Wikipedia-Seite](http://en.wikipedia.org/wiki/Radian).

### <a name="forward-angle"></a>Forward-Spitzen
Einmal die `FacePoint` Methode den Winkel in Bogenmaß konvertiert, definiert einen `forwardAngle` Wert. Dieser Wert stellt den Winkel, in dem die Entität mit Internetzugriff wird bei der Rotation Wert gleich 0 ist. In diesem Beispiel wird angenommen, dass die Entität nach oben, mit Internetzugriff wird also um 90° eine mathematische Drehung (im Gegensatz zu CocosSharp Drehung) verwenden. Wir verwenden die mathematische Drehung hier, da wir noch die Rotation für CocosSharp invertiert nicht geschehen.

Das folgende Beispiel zeigt, welche eine Entität mit einem `forwardAngle` von 90 Grad kann folgendermaßen aussehen:

![](math-images/image6.png "Dies zeigt, wie eine Entität mit einem ForwardAngle von 90 Grad aussehen könnte")


## <a name="angled-velocity"></a>Spitzen Geschwindigkeit

Bisher haben wir an, wie einen Offset in einen Winkel konvertieren gesucht. In diesem Abschnitt wird eine andere Möglichkeit – einen Winkel und konvertiert sie in X und Y-Werte. Typische Beispiele dafür sind ein Auto in die Richtung, die sie mit Internetzugriff wird oder ein Speicherplatz Ship behandeln die in Richtung verschoben wird, die der Liefernamen mit Internetzugriff wird Aufzählungszeichen verschieben. 

Grundsätzlich kann Geschwindigkeit berechnet werden, indem zunächst definieren die gewünschte Geschwindigkeit, wenn nicht gedreht, und klicken Sie dann drehen, Geschwindigkeit zu der Winkel, den eine Entität mit Internetzugriff wird. Dieses Konzept erklären sollen, können als ein 2-dimensionale Geschwindigkeits-(und) visualisiert werden *Vektor* (dem als Pfeil in der Regel gezeichnet wird). Einen Vektor mit X-Wert Geschwindigkeit = 100 "und" Y = 0 kann wie folgt visuell dargestellt werden:

![](math-images/image7.png "Einen Vektor mit X-Wert Geschwindigkeit = 100 "und" Y = 0 kann wie folgt visuell dargestellt werden")

Dieses Vektors kann gedreht werden, um eine neue Geschwindigkeit führen. Den Vektor durch (mithilfe von gegen den Uhrzeigersinn drehen) in 45-Grad drehen ergibt sich z. B. sich Folgendes:

![](math-images/image8.png "Den Vektor Drehen von 45 Grad gegen den Uhrzeigersinn drehen Ergebnisse in diesem")

Glücklicherweise können Geschwindigkeit, Beschleunigung und sogar Position alle gedreht werden mit den gleichen Code unabhängig davon, wie die Werte angewendet werden. Die folgende allgemeine Funktion kann durch eine CocosSharp Drehungswert zu drehende Vektor verwendet werden:


```csharp
// Rotates the argument vector by degrees specified by
// cocosSharpDegrees. In other words, the rotation
// value is expected to be clockwise.
// The vector parameter is modified, so it is both an in and out value
void RotateVector(ref CCVector2 vector, float cocosSharpDegrees)
{
    // Invert the rotation to get degrees as is normally
    // used in math (counterclockwise)
    float mathDegrees = -cocosSharpDegrees;

    // Convert the degrees to radians, as the System.Math
    // object expects arguments in radians
    float radians = CCMathHelper.ToRadians (mathDegrees);

    // Calculate the "up" and "right" vectors. This is essentially
    // a 2x2 matrix that we'll use to rotate the vector
    float xAxisXComponent = (float)System.Math.Cos (radians);
    float xAxisYComponent = (float)System.Math.Sin (radians);
    float yAxisXComponent = (float)System.Math.Cos (radians + CCMathHelper.Pi / 2.0f);
    float yAxisYComponent = (float)System.Math.Sin (radians + CCMathHelper.Pi / 2.0f);

    // Store the original vector values which will be used
    // below to perform the final operation of rotation.
    float originalX = vector.X;
    float originalY = vector.Y;

    // Use the axis values calculated above (the matrix values)
    // to rotate and assign the vector.
    vector.X = originalX * xAxisXComponent + originalY * yAxisXComponent;
    vector.Y = originalX * xAxisYComponent + originalY * yAxisYComponent;
} 
```

Einen vollständigen Überblick über die `RotateVector` Methode muss mit dem Kosinus und Sinus trigonometrische Funktionen zusammen mit einigen lineare Algebra verglichen werden, die nicht Gegenstand dieses Artikels ist vertraut wird. Allerdings einmal implementiert die `RotateVector` Methode kann verwendet werden, um alle Vektor, einschließlich einer normalverteilt drehen. Z. B. der folgende Code möglicherweise ausgelöst, Aufzählungszeichen in eine Richtung gemäß der `rotation` Wert:


```csharp
// Create a Bullet instance
Bullet newBullet = new Bullet();

// Define the velocity of the bullet when 
// rotation is 0
CCVector2 velocity = new CCVector2 (0, 100);

// Modify the velocity according to rotation
RotateVector (ref velocity, rotation);

// Assign the newBullet's velocity using the
// rotated vector
newBullet.VelocityX = velocity.X;
newBullet.VelocityY = velocity.Y;

// Set the bullet's rotation so it faces
// the direction that it's flying
newBullet.Rotation = rotation; 
```

Dieser Code kann etwa generiert:

![](math-images/image9.png "Dieser Code kann in etwa diesen Screenshot erzeugen.")


# <a name="summary"></a>Zusammenfassung

Dieser Leitfaden behandelt allgemeine mathematische Konzepte der Entwicklung von 2D. Es wird gezeigt, wie zuweisen und Implementierung Geschwindigkeit und Acceleration und beschrieben, wie Objekte und Vektoren für das Verschieben in eine beliebige Richtung zu drehen.