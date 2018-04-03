---
title: Animieren mit CCAction
description: Die CCAction-Klasse vereinfacht das Hinzufügen von Animationen CocosSharp spielen. Diese Animationen können verwendet werden, um Funktionalität zu implementieren oder um Polnisch hinzuzufügen.
ms.topic: article
ms.prod: xamarin
ms.assetid: 74DBD02A-6F10-4104-A61B-08CB49B733FB
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: 7e64789f4e86dbcd47fc760fd9d4d7fb61c76121
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="animating-with-ccaction"></a>Animieren mit CCAction

_Die CCAction-Klasse vereinfacht das Hinzufügen von Animationen CocosSharp spielen. Diese Animationen können verwendet werden, um Funktionalität zu implementieren oder um Polnisch hinzuzufügen._

`CCAction` ist eine Basisklasse, die zu animierende CocosSharp Objekte verwendet werden kann. Diese Anleitung enthält integrierte `CCAction` Implementierungen für allgemeine Aufgaben wie das positionieren, skalieren und drehen. Sie prüft auch benutzerdefinierte Implementierungen zu erstellen, durch Vererbung von `CCAction`.

Dieses Handbuch verwendet ein Projekt mit der Bezeichnung **ActionProject** der [kann hier heruntergeladen werden](https://developer.xamarin.com/samples/mobile/CCAction). Dieses Handbuch verwendet die `CCDrawNode` -Klasse, die in abgedeckt wird die [Zeichnung Geometrie mit CCDrawNode](~/graphics-games/cocossharp/ccdrawnode.md) Handbuch.


## <a name="running-the-actionproject"></a>Ausführen der ActionProject

**ActionProject** ist eine Lösung CocosSharp für iOS und Android erstellt werden kann. Es dient sowohl als ein Codebeispiel für die Verwendung der `CCAction` Klasse und als eine Echtzeit Demo zu gemeinsamen `CCAction` Implementierungen.

Bei der Ausführung ActionProject zeigt drei `CCLabel` Instanzen auf der linken Seite des Bildschirms und ein visuelles Objekt, das durch zwei gezeichnet `CCDrawNode` Instanzen für die verschiedenen Aktionen anzeigen:

![](ccaction-images/image1.png "ActionProject zeigt drei CCLabel-Instanzen auf der linken Seite des Bildschirms und ein visuelles Objekt, das von zwei Instanzen von CCDrawNode gezeichnet werden, für die verschiedenen Aktionen anzeigen")

Die Bezeichnungen auf der linken Seite anzugeben, welche `CCAction` wird erstellt, wenn auf dem Bildschirm tippen. Wird standardmäßig die **Position** Wert ausgewählt ist, was zu einer `CCMove` Aktion erstellt und auf den roten Kreis angewendet wird:

![](ccaction-images/image2.gif "Der Positionswert ausgewählt ist, wodurch eine CCMove Aktion wird erstellt und auf den roten Kreis angewendet,")

Durch Klicken auf die Bezeichnungen auf der linken Seite verändert welche Art von `CCAction` für den Kreis ausgeführt wird. Klicken Sie z. B. die **Position** Bezeichnung durchläuft die verschiedenen Werte, die geändert werden können:

![](ccaction-images/image3.gif "Klicken auf die positionsbeschriftung werden die verschiedenen Werte durchzugehen, die geändert werden können")

## <a name="common-variable-changing-ccaction-classes"></a>Allgemeine Variable wechselnde CCAction-Klassen

Die **ActionProject** verwendet die folgenden `CCAction`-erbende Klassen, die einen Teil einer CocosSharp sind:

 - `CCMoveTo` – Ändert eine `CCNode` Instanz `Position` Eigenschaft
 - `CCScaleTo` – Ändert eine `CCNode` Instanz `Scale` Eigenschaft
 - `CCRotateTo` – Ändert eine `CCNode` Instanz `Rotation` Eigenschaft

Dieses Handbuch bezieht sich auf diese Aktionen als *Variable wechselnde*, was bedeutet, dass sie die Variable der direkte Auswirkungen auf die `CCNode` , die sie hinzugefügt werden. Andere Arten von Aktionen als bezeichnet *Beschleunigungsfunktionen* Aktionen, die weiter unten in diesem Handbuch behandelt werden.

Die **ActionProject** wird veranschaulicht, dass der Zweck dieser Aktionen wird eine Variable mit der Zeit ändern. Insbesondere diese `CCActions` Konstruktoren zwei Argumente: Länge der auszuführenden Zeit und der Wert zugewiesen. Im folgenden Codeabschnitt zeigt, wie die drei Typen von Aktionen erstellt werden:


```csharp
switch (VariableOptions [currentVariableIndex])
{
    case "Position":
        coreAction = new CCMoveTo(timeToTake, touch.Location);

        break;
    case "Scale":
        var distance = CCPoint.Distance (touch.Location, drawNodeRoot.Position);
        var desiredScale = distance / DefaultCircleRadius;
        coreAction = new CCScaleTo(timeToTake, desiredScale);

        break;
    case "Rotation":
        float differenceY = touch.Location.Y - drawNodeRoot.PositionY;
        float differenceX = touch.Location.X - drawNodeRoot.PositionX;

        float angleInDegrees = -1 * CCMathHelper.ToDegrees(
            (float)System.Math.Atan2(differenceY, differenceX));

        coreAction = new CCRotateTo (timeToTake, angleInDegrees);

        break; 
...
}
```

Nachdem die Aktion erstellt wird, wird es einem CCNode wie folgt hinzugefügt:


```csharp
nodeToAddTo.AddAction (coreAction); 
```

`AddAction` Startet die `CCAction` Verhalten der Instanz, und es führt automatisch ihre Logik Frame nach dem anderen bis zur Fertigstellung.

Alle Typen aufgeführten endet mit dem Wort *auf* Dies bedeutet, dass die `CCAction` ändern die `CCNode` so, dass der Argumentwert den endgültigen Zustand darstellt, wenn die Aktion beendet hat. Z b. Erstellen einer `CCMoveTo` mit einer Position x = 100 "und" Y = 200 führt in der `CCNode` -Instanz `Position` wird auf X = 100, Y = 200 am Ende die Zeit angegeben wird, unabhängig von der `CCNode` Instanz Anfangsposition.

Jeder "Zu"-Klasse verfügt auch über eine "Durch" Version, die den Wert des Arguments auf den aktuellen Wert hinzugefügt wird die `CCNode`. Z b. Erstellen einer `CCMoveBy` mit einer Position x = 100 "und" Y = 200 führt zu den `CCNode` Instanz verschobene Einheiten rechts 100 und 200 Einheiten von der Position an, die Aktion gestartet wurde.


## <a name="easing-actions"></a>Einfachere Aktionen

Standardmäßig führt Aktionen Variable ändern *linearen Interpolation* – die Aktion wird für den gewünschten Wert mit einer Konstante Rate verschoben. Wenn interpolieren *Position* linear, gleitende Objekt sofort starten und beenden, Verschieben am Anfang und Ende der Aktion und seiner Geschwindigkeit wird konstant bleiben. Dies ist die Aktion ausführt. 

Nicht lineare Interpolation ist weniger berücksichtigen und fügt ein Element der Polnisch, hinzu, damit CocosSharp bietet eine Vielzahl von Beschleunigungsfunktionen Aktionen, die zum Ändern der Variablen ändern Aktionen verwendet werden können.

In der **ActionProject** Beispiel können wir zwischen diesen Typen von Beschleunigungsfunktionen Aktionen durch Klicken auf das zweite Bezeichnungsfeld wechseln (die standardmäßig die **<None>**):

![](ccaction-images/image4.gif "Der Benutzer kann zwischen diesen Typen von Beschleunigungsfunktionen Aktionen durch Klicken auf das zweite Bezeichnungsfeld wechseln.")

Migrationstoolkit Aktionen sind besonders leistungsstark, da sie nicht auf bestimmte Variable Einstellung Maßnahmen verbunden sind. Dies bedeutet, dass dieselbe Beschleunigungsfunktionen Aktion zuweisen Position, Drehung, Skalierung oder benutzerdefinierte Aktionen (wie weiter unten in diesem Handbuch dargestellt) verwendet werden kann.

Migrationstoolkit Aktionen umschließen Aktionen Variable festlegen (solange die Variable einstellungsaktion erbt `CCFiniteTimeAction`) durch eine Variable einstellungsaktion als Argument in ihren Konstruktoren akzeptieren.

Angenommen, wenn die Bezeichnungen so **Position**, **CCEaseElastic**, und klicken Sie dann der folgende Code ausgeführt wird, wenn eine Fingereingabe erkannt wird (Beachten Sie, dass der Code zum Hervorheben der relevanten Zeilen ausgelassen wurde):


```csharp
CCFiniteTimeAction coreAction = null; 
...
coreAction = new CCMoveTo(timeToTake, touch.Location); 
...
CCAction easing = null; 
...
easing = new CCEaseSineOut (coreAction); 
...
nodeToAddTo.AddAction (easing); 
```

Wie von der Anwendung gezeigt, die genau dieselbe Beschleunigungsfunktionen kann angewendet werden auf andere Variablen festlegende Aktionen wie z. B. `CCRotateTo`:

![](ccaction-images/image5.gif "Die genaue dieselbe Beschleunigungsfunktionen kann auf andere Variablen Einstellung Aktionen wie z. B. CCRotateTo angewendet werden")


## <a name="easing-in-out-and-inout"></a>In, Out und InOut Beschleunigungsfunktionen

Alle Beschleunigungsfunktionen Aktionen haben `In`, `Out`, oder `InOut` Beschleunigungsfunktionen Typ angefügt. Diese Begriffe beziehen sich auf, wenn die einfachere angewendet wird: `In` Beschleunigungsfunktionen angewendet am Anfang, `Out` bedeutet, dass am Ende und `InOut` bedeutet, dass sowohl am Anfang und Ende.

Ein `In` Beschleunigungsfunktionen Aktion wirkt sich auf die Möglichkeit, eine Variable in der gesamten Interpolation (beide am Anfang und am Ende) angewendet wird, aber in der Regel die offensichtlichste Merkmale der Beschleunigungsfunktionen Aktion erfolgt am Anfang. Auf ähnliche Weise `Out` Beschleunigungsfunktionen Aktionen sind gekennzeichnet durch ihr Verhalten am Ende der Interpolation. Beispielsweise `CCEaseBounceOut` führt dazu, ein Objekt, das am Ende der Aktion verarbeit.


### <a name="out"></a>Out

`Out` im Allgemeinen Beschleunigungsfunktionen übernimmt die am deutlichsten bemerkbar macht Änderungen am Ende der Interpolation. Beispielsweise `CCEaseExponentialOut` verlangsamt die Änderungsrate der Variablen ändern sie den Zielwert erreicht:

![](ccaction-images/image6.gif "CCEaseExponentialOut verlangsamt die Änderungsrate der Variablen ändern, wie sie den Zielwert nähert")


### <a name="in"></a>In

`In` im Allgemeinen Beschleunigungsfunktionen gilt die auffälligste Änderung am Anfang der Interpolation. Beispielsweise `CCEaseExponentialIn` wird am Anfang der Aktion langsamer verschoben:

![](ccaction-images/image7.gif "CCEaseExponentialIn wird am Anfang der Aktion langsamer verschoben.")


### <a name="inout"></a>InOut

`InOut` im Allgemeinen gilt die am deutlichsten bemerkbar macht Änderungen sowohl am Anfang und Ende. `InOut` einfachere ist normalerweise symmetrisch. Beispielsweise `CCEaseExponentialInOut` wird am Anfang und Ende der Aktion langsam verschoben:

![](ccaction-images/image8.gif "CCEaseExponentialInOut wird am Anfang und Ende der Aktion langsam verschoben.")


## <a name="implementing-a-custom-ccaction"></a>Implementieren eine benutzerdefinierte CCAction

Alle bisher besprochenen Klassen sind in CocosSharp bieten allgemeine Funktionen enthalten. Benutzerdefinierte `CCAction` Implementierungen bieten zusätzliche Flexibilität. Z. B. eine `CCAction` , die das ausgefüllte Verhältnis von eine Leiste Erfahrung steuert genutzt werden, damit die Leiste Erfahrung reibungslos wächst, wenn der Benutzer Erfahrung verdient.

`CCAction` Implementierungen erfordern in der Regel zwei Klassen:

 - `CCFiniteTimeAction` Implementierung – ist die Klasse für begrenzte Zeit Aktion zum Starten der Aktions verantwortlich. Es ist die Klasse instanziiert wird und entweder direkt hinzugefügt eine `CCNode` oder einer Beschleunigungsfunktionen Aktion. Muss es instanziieren und Zurückgeben einer `CCFiniteTimeActionState`, werden die Updates ausführen.
 - `CCFiniteTimeActionState` Implementierung – die Status-Klasse für begrenzte Zeit Aktion ist für die Aktualisierung der Variablen, die in der Aktion betroffenen verantwortlich. Es muss eine Update-Funktion implementieren, die weist den Wert auf dem Ziel nach einer Zeitwert. Diese Klasse nicht explizit verwiesen wird außerhalb von der `CCFiniteTimeAction` der wird erstellt. Es funktioniert einfach "im Hintergrund".

**ActionProject** stellt eine benutzerdefinierte `CCFiniteTimeAction` -Implementierung mit der Bezeichnung `LineWidthAction,` der verwendet wird, passen Sie die Breite der gelbe Linie, die zusätzlich zu den roten Kreis gezeichnet. Beachten Sie, dass der Auftrag nur zu instanziieren und Zurückgeben einer `LineWidthState` Instanz:


```csharp
public class LineWidthAction : CCFiniteTimeAction
{
    float endWidth;

    public LineWidthAction (float duration, float width) : base(duration)
    {
        endWidth = width;
    }

    public override CCFiniteTimeAction Reverse ()
    {
        throw new NotImplementedException ();
    }

    protected override CCActionState StartAction (CCNode target)
    {
        return new LineWidthState (this, target, endWidth);
    }
}
```

Wie bereits erwähnt, die `LineWidthState` funktioniert die Zuweisung von der Zeile `Width` Eigenschaft entsprechend, wie viel `time` übergeben wurde:


```csharp
public class LineWidthState : CCFiniteTimeActionState
{
    float deltaWidth;
    float startWidth;

    LineNode castedTarget;

    public LineWidthState(LineWidthAction action, CCNode target, float endWidth) : base(action, target)
    {
        castedTarget = target as LineNode;

        if (castedTarget == null)
        {
            throw new InvalidOperationException ("The argument target must be a LineNode");
        }

        startWidth = castedTarget.Width;
        deltaWidth = endWidth - startWidth;
    }

    public override void Update (float time)
    {
        castedTarget.Width = startWidth + deltaWidth * time;
    }
} 
```

Die LineWidthAction kann mit einer beliebigen Beschleunigungsfunktionen Aktion so ändern Sie die Linienstärke auf verschiedene Weise kombiniert werden, wie in der folgenden Animation gezeigt:

![](ccaction-images/image9.gif "Der LineWidthAction kann mit einer beliebigen Beschleunigungsfunktionen Aktion so ändern Sie die Linienstärke auf verschiedene Weise kombiniert werden, wie in dieser Animation dargestellt")


### <a name="interpolation-and-the-update-method"></a>Interpolation und die Update-Methode

Die einzige Logik, abgesehen von der Speichern von Werten in den oben genannten Klassen befindet sich in der `LineWidthState.Update` Methode. Die `startWidth` Variable speichert die Breite des Ziels `LineNode` am Anfang der Aktion und die `deltaWidth` Variable speichert, wie viel der Wert im Verlauf der Aktion geändert wird.

Durch das Ersetzen der `time` -Variable mit dem Wert 0 ist, wie Sie sehen, die das Ziel `LineNode` , findet sich auf seine Anfangsposition:


```csharp
castedTarget.Width = startWidth + deltaWidth * 0; 
```

Auf ähnliche Weise, wie Sie sehen, die das Ziel `LineNode` wird durch das Ersetzen der Time-Variablen mit einem Wert von 1 am Ziel werden:


```csharp
castedTarget.Width = startWidth + deltaWidth * 1; 
```

Die `time` Wert wird in der Regel zwischen 0 und 1 – werden jedoch nicht immer – und `Update` Implementierungen sollten nicht davon, dass diese Grenzen. Einige Beschleunigungsfunktionen Methoden (z. B. `CCEaseBackIn` und `CCEaseBackOut`) bietet einen Time-Wert außerhalb des Bereichs 0 bis 1.


## <a name="conclusion"></a>Schlussbemerkung

Interpolation und einfachere sind ein wichtiger Bestandteil der Erstellung ein professionelles Spiel, insbesondere, wenn Sie Benutzeroberflächen erstellen. Diese Anleitung enthält Informationen zum Verwenden `CCActions` um Standardwerte z. B. Positions- und drehungswerten sowie benutzerdefinierte Werte zu interpolieren. Die `LineWidthState` und `LineWidthAction` Klassen wird gezeigt, wie eine benutzerdefinierte Aktion implementiert.

## <a name="related-links"></a>Verwandte links

- [CCAction](https://developer.xamarin.com/api/type/CocosSharp.CCAction)
- [CCMoveTo](https://developer.xamarin.com/api/type/CocosSharp.CCMoveTo)
- [CCScaleTo](https://developer.xamarin.com/api/type/CocosSharp.CCScaleTo)
- [CCRotateTo](https://developer.xamarin.com/api/type/CocosSharp.CCRotateTo)
- [CCDrawNode](https://developer.xamarin.com/api/type/CocosSharp.CCDrawNode)
- [Das vollständige Codebeispiel](https://developer.xamarin.com/samples/mobile/CCAction/)
