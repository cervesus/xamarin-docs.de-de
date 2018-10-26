---
title: Animieren mit CCAction
description: Die Klasse CCAction vereinfacht Hinzufügen von Animationen zu CocosSharp-spielen. Diese Animationen können verwendet werden, um Funktionalität zu implementieren oder Polish hinzuzufügen.
ms.prod: xamarin
ms.assetid: 74DBD02A-6F10-4104-A61B-08CB49B733FB
author: conceptdev
ms.author: crdun
ms.date: 03/24/2017
ms.openlocfilehash: c486bb2e78579360e0f935219cd82958fedee34b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120969"
---
# <a name="animating-with-ccaction"></a>Animieren mit CCAction

_Die Klasse CCAction vereinfacht Hinzufügen von Animationen zu CocosSharp-spielen. Diese Animationen können verwendet werden, um Funktionalität zu implementieren oder Polish hinzuzufügen._

`CCAction` ist eine Basisklasse, die zu animierende CocosSharp-Objekte verwendet werden kann. Dieses Handbuch enthält integrierte `CCAction` Implementierungen für allgemeine Aufgaben wie das positionieren, Skalierung und Rotation. Er befasst sich auch mit Erstellen benutzerdefinierte Implementierungen durch Erben von `CCAction`.

Dieser Anleitung wird verwendet, ein Projekt namens **ActionProject** die [kann hier heruntergeladen werden](https://developer.xamarin.com/samples/mobile/CCAction). Dieses Handbuch verwendet die `CCDrawNode` -Klasse, die in behandelt wird die [Zeichnung Geometrie mit CCDrawNode](~/graphics-games/cocossharp/ccdrawnode.md) Guide.


## <a name="running-the-actionproject"></a>Ausführen der ActionProject

**ActionProject** ist eine CocosSharp-Lösung, die für iOS und Android erstellt werden kann. Sie dient als ein Codebeispiel für die Verwendung der `CCAction` Klasse sowie einer Echtzeit-Demo von allgemeinen `CCAction` Implementierungen.

Bei der Ausführung ActionProject zeigt drei `CCLabel` Instanzen auf der linken Seite des Bildschirms und eines visuellen Objekts gezeichnet wird, um zwei `CCDrawNode` -Instanzen für die verschiedenen Aktionen anzeigen:

![](ccaction-images/image1.png "ActionProject zeigt drei CCLabel-Instanzen auf der linken Seite des Bildschirms und ein visuelles Objekt, das von zwei Instanzen von CCDrawNode gezeichnet wird, für die verschiedenen Aktionen anzeigen")

Die Bezeichnungen auf der linken Seite anzugeben, welchen Typ von `CCAction` wird erstellt, wenn auf dem Bildschirm durch Tippen auf. In der Standardeinstellung die **Position** Wert ausgewählt ist, was zu einer `CCMove` Aktion erstellt und auf den roten Kreis angewendet wird:

![](ccaction-images/image2.gif "Den-Positionswerts ausgewählt ist, und eine CCMove Aktion wird erstellt, und klicken Sie auf den roten Kreis angewendet,")

Die Bezeichnungen auf der linken Seite auf welche Art von Änderungen `CCAction` für den Kreis ausgeführt wird. Klicken Sie z. B. die **Position** Bezeichnung durchläuft die verschiedenen Werte, die geändert werden können:

![](ccaction-images/image3.gif "Klicken Sie auf die positionsbeschriftung werden die verschiedenen Werte durchlaufen, die geändert werden können")

## <a name="common-variable-changing-ccaction-classes"></a>Klassen für allgemeine Variablen ändern CCAction

Die **ActionProject** verwendet die folgenden `CCAction`-erbende Klassen, die einen Teil des CocosSharp sind:

 - `CCMoveTo` : Ändert eine `CCNode` Instanz `Position` Eigenschaft
 - `CCScaleTo` : Ändert eine `CCNode` Instanz `Scale` Eigenschaft
 - `CCRotateTo` : Ändert eine `CCNode` Instanz `Rotation` Eigenschaft

Dieses Handbuch bezieht sich auf diese Aktionen als *Variable ändern*, was bedeutet, dass sie die Variable vom direkten Einfluss auf die `CCNode` , die sie hinzugefügt werden. Andere Arten von Aktionen als bezeichnet *Beschleunigungsfunktion* Aktionen, die später in diesem Handbuch behandelt werden.

Die **ActionProject** wird veranschaulicht, dass der Zweck dieser Aktionen wird eine Variable im Laufe der Zeit ändern. Insbesondere diese `CCActions` Konstruktoren werden zwei Argumente: Länge der Zeit, zu heben und der Wert zugewiesen. Der folgende Codeabschnitt zeigt, wie die drei Typen von Aktionen erstellt werden:


```csharp
switch (VariableOptions [currentVariableIndex])
{
    case "Position":
        coreAction = new CCMoveTo(timeToTake, touch.Location);

        break;
    case "Scale":
        var distance = CCPoint.Distance (touch.Location, drawNodeRoot.Position);
        var desiredScale = distance / DefaultCircleRadius;
        coreAction = new CCScaleTo(timeToTake, desiredScale);

        break;
    case "Rotation":
        float differenceY = touch.Location.Y - drawNodeRoot.PositionY;
        float differenceX = touch.Location.X - drawNodeRoot.PositionX;

        float angleInDegrees = -1 * CCMathHelper.ToDegrees(
            (float)System.Math.Atan2(differenceY, differenceX));

        coreAction = new CCRotateTo (timeToTake, angleInDegrees);

        break; 
...
}
```

Nachdem die Aktion erstellt wurde, wird es einem CCNode wie folgt hinzugefügt:


```csharp
nodeToAddTo.AddAction (coreAction); 
```

`AddAction` Startet die `CCAction` Instanz Verhalten und führt automatisch die Logik Frame nach dem anderen bis zur Beendigung.

Alle Typen obigen endet mit dem Wort *zu* d. h. der `CCAction` Ändern der `CCNode` so, dass den Wert des Arguments den endgültigen Zustand ergibt, wenn die Aktion abgeschlossen ist. Erstellen Sie z. B. eine `CCMoveTo` mit einer Position x = 100 "und" Y = 200 führt in der `CCNode` Instanz `Position` X festgelegt wird = 100 "," Y = 200 am Ende der Zeit angegeben wird, unabhängig davon, die `CCNode` Instanz Anfangsposition.

Jede "To"-Klasse verfügt auch über eine "Von" Version, die den aktuellen Wert auf den Wert des Arguments hinzugefügt wird die `CCNode`. Erstellen Sie z. B. eine `CCMoveBy` mit einer Position x = 100 "und" Y = 200 führt zu den `CCNode` Instanz, die die Einheiten rechts 100 und 200 Einheiten von der Position, war es, die Aktion gestartet wurde, verschoben wird.


## <a name="easing-actions"></a>Vereinfachen von Aktionen

Standardmäßig führt die Aktionen Variable ändern *linearer Interpolation* : die Aktion wird an den gewünschten Wert mit konstanter Geschwindigkeit verschieben. Wenn interpolieren *Position* linear, sich das Bewegungsobjekt sofort starten und beenden, Verschieben am Anfang und Ende der Aktion und die Geschwindigkeit wird konstant bleiben, während die Aktion ausführt. 

Nicht lineare Interpolation ist weniger irritierend und fügt ein Element der Ausgereiftheit, damit CocosSharp eine Vielzahl bietet von Aktionen, die verwendet werden können, so ändern Sie die Variable ändern Aktionen zu vereinfachen.

In der **ActionProject** Beispiel können wir zwischen diesen Typen von Ads Aktionen durch Klicken auf die zweite Bezeichnung wechseln (Standardwert: **<None>**):

![](ccaction-images/image4.gif "Der Benutzer kann zwischen diesen Typen von Ads Aktionen durch Klicken auf die zweite Bezeichnung wechseln.")

ADS Aktionen sind besonders interessant, da sie nicht an einer bestimmten Variable-Setting-Aktion gebunden sind. Dies bedeutet, dass die gleiche Ads-Aktion verwendet werden kann, Position, Drehung, Skalierung oder benutzerdefinierte Aktionen zuweisen (siehe weiter unten in diesem Handbuch werden wird).

ADS Aktionen umschließen Variable-Setting-Aktionen (sofern die Variable-einstellungsaktion erbt `CCFiniteTimeAction`) durch eine Variable einstellungsaktion als Argument in ihren Konstruktoren akzeptieren.

Z. B., wenn die Bezeichnungen so **Position**, **CCEaseElastic**, und klicken Sie dann der folgende Code ausgeführt wird, wenn eine Fingereingabe erkannt wird (Beachten Sie, dass Code weggelassen wurde, markieren Sie die entsprechenden Zeilen):


```csharp
CCFiniteTimeAction coreAction = null; 
...
coreAction = new CCMoveTo(timeToTake, touch.Location); 
...
CCAction easing = null; 
...
easing = new CCEaseSineOut (coreAction); 
...
nodeToAddTo.AddAction (easing); 
```

Wie von der Anwendung dargestellt, die genau gleiche vereinfachen kann angewendet werden für andere Variable-Setting-Aktionen wie z. B. `CCRotateTo`:

![](ccaction-images/image5.gif "Die genau gleiche vereinfachen kann auf andere Variablen-Setting-Aktionen, z. B. CCRotateTo angewendet werden")


## <a name="easing-in-out-and-inout"></a>Einfachere In, Out, und InOut

Alle Migrationstoolkit Aktionen haben `In`, `Out`, oder `InOut` der Ads-Typ hinzugefügt. Diese Begriffe beziehen sich auf, wenn die Beschleunigungsfunktion angewendet wird: `In` Übergängen angewendet am Anfang, `Out` bedeutet, dass am Ende und `InOut` bedeutet, dass sowohl am Anfang und Ende.

Ein `In` Aktion Beschleunigungsfunktion wirkt sich auf die Möglichkeit, eine Variable in der gesamten Interpolation (beide am Anfang und am Ende) angewendet wird, aber in der Regel die bekanntesten Merkmale der Ads-Aktion erfolgt am Anfang. Auf ähnliche Weise `Out` Migrationstoolkit Aktionen sind gekennzeichnet durch die ihr Verhalten am Ende der Interpolation. Z. B. `CCEaseBounceOut` führt dazu, dass ein Objekt, das am Ende der Aktion springen.


### <a name="out"></a>Out

`Out` im Allgemeinen erleichtert, gilt die wichtigsten Änderungen am Ende der Interpolation. Z. B. `CCEaseExponentialOut` die Änderungsrate der veränderlichen Variablen wird beeinträchtigt werden, wenn sie den Zielwert nähert:

![](ccaction-images/image6.gif "CCEaseExponentialOut wird die Geschwindigkeit der Änderung der veränderlichen Variablen beeinträchtigt, wenn sie den Zielwert nähert")


### <a name="in"></a>In

`In` im Allgemeinen erleichtert, gilt die offensichtlichste Änderung am Anfang der Interpolation. Z. B. `CCEaseExponentialIn` wird am Anfang der Aktion langsamer verschoben:

![](ccaction-images/image7.gif "Am Anfang der Aktion wird langsamer CCEaseExponentialIn verschoben.")


### <a name="inout"></a>InOut

`InOut` im Allgemeinen gilt die wichtigsten Änderungen sowohl am Anfang und Ende. `InOut` einfachere ist normalerweise symmetrisch. Z. B. `CCEaseExponentialInOut` wird am Anfang und Ende der Aktion langsam verschoben:

![](ccaction-images/image8.gif "Am Anfang und Ende der Aktion wird langsam CCEaseExponentialInOut verschoben.")


## <a name="implementing-a-custom-ccaction"></a>Implementieren eine benutzerdefinierte CCAction

Alle bisher besprochenen Klassen sind in CocosSharp bieten allgemeine Funktionen enthalten. Benutzerdefinierte `CCAction` Implementierungen bieten zusätzliche Flexibilität. Z. B. eine `CCAction` welche das ausgefüllte Verhältnis von eine Leiste Erfahrung Steuerelemente kann verwendet werden, sodass die Leiste Erfahrung reibungslos wächst, wenn der Benutzer die Erfahrung verdient.

`CCAction` Implementierungen erfordern in der Regel zwei Klassen:

 - `CCFiniteTimeAction` Implementierung – ist die Action-Klasse für begrenzte Zeit zum Starten der Aktions verantwortlich. Es ist die Klasse instanziiert wird und entweder direkt hinzugefügt eine `CCNode` oder einer Migrationstoolkit Aktion. Sie instanziieren und zurückgeben muss eine `CCFiniteTimeActionState`, werden die Updates ausführen.
 - `CCFiniteTimeActionState` Implementierung – ist die begrenzte Zeit Action-Status-Klasse, die für die Variablen, die in der Aktion betroffenen aktualisiert verantwortlich. Es muss eine Update-Funktion implementieren, die den Wert für das Ziel nach einer Zeitwert zuweist. Diese Klasse nicht explizit verwiesen wird außerhalb von den `CCFiniteTimeAction` die wird erstellt. Es funktioniert einfach "hinter den Kulissen".

**ActionProject** bietet ein benutzerdefiniertes `CCFiniteTimeAction` Implementierung mit dem Namen `LineWidthAction,` die wird verwendet, um die Breite des der gelbe Linie, die auf den roten Kreis gezeichnet anzupassen. Beachten Sie, dass seine einzige Aufgabe zu instanziieren und Zurückgeben einer `LineWidthState` Instanz:


```csharp
public class LineWidthAction : CCFiniteTimeAction
{
    float endWidth;

    public LineWidthAction (float duration, float width) : base(duration)
    {
        endWidth = width;
    }

    public override CCFiniteTimeAction Reverse ()
    {
        throw new NotImplementedException ();
    }

    protected override CCActionState StartAction (CCNode target)
    {
        return new LineWidthState (this, target, endWidth);
    }
}
```

Wie bereits erwähnt, die `LineWidthState` übernimmt das Zuweisen der Zeile `Width` Eigenschaft nach, wie viel `time` wurde übergeben:


```csharp
public class LineWidthState : CCFiniteTimeActionState
{
    float deltaWidth;
    float startWidth;

    LineNode castedTarget;

    public LineWidthState(LineWidthAction action, CCNode target, float endWidth) : base(action, target)
    {
        castedTarget = target as LineNode;

        if (castedTarget == null)
        {
            throw new InvalidOperationException ("The argument target must be a LineNode");
        }

        startWidth = castedTarget.Width;
        deltaWidth = endWidth - startWidth;
    }

    public override void Update (float time)
    {
        castedTarget.Width = startWidth + deltaWidth * time;
    }
} 
```

Die LineWidthAction kann mit einer beliebigen Aktion Migrationstoolkit, der die Linienstärke in verschiedene Art und Weise ändern kombiniert werden, wie in der folgenden Animation dargestellt:

![](ccaction-images/image9.gif "Die LineWidthAction kann mit einer beliebigen Aktion Migrationstoolkit, der die Linienstärke in verschiedene Art und Weise ändern kombiniert werden, wie gezeigt in der animation")


### <a name="interpolation-and-the-update-method"></a>Zeichenfolgeninterpolation und der Update-Methode

Die einzige Logik, abgesehen von der Speichern von Werten in den oben genannten Klassen befinden sich in der `LineWidthState.Update` Methode. Die `startWidth` -Variable speichert die Breite des Ziels `LineNode` am Anfang der Aktion und die `deltaWidth` -Variable speichert, wie viel der Wert im Verlauf der Aktion ändern wird.

Durch Ersetzen der `time` Variablen mit einem Wert von 0, sehen wir, dass das Ziel `LineNode` an seine Startposition werden:


```csharp
castedTarget.Width = startWidth + deltaWidth * 0; 
```

Analog dazu sehen wir, dass das Ziel `LineNode` an ihrem Ziel durch Ersetzen der Zeitvariablen mit einem Wert von 1 werden:


```csharp
castedTarget.Width = startWidth + deltaWidth * 1; 
```

Die `time` Wert wird in der Regel zwischen 0 und 1 – aber nicht immer – und `Update` Implementierungen sollten diese Grenzen nicht angenommen. Einige Ads-Methoden (z. B. `CCEaseBackIn` und `CCEaseBackOut`) bietet einen Time-Wert außerhalb des Bereichs 0 bis 1.


## <a name="conclusion"></a>Schlussfolgerung

Zeichenfolgeninterpolation und vereinfachen der sind ein wichtiger Bestandteil beim Erstellen eines Spiels ansprechenden, besonders beim Erstellen von Benutzeroberflächen. Diesem Leitfaden wird beschrieben, wie Sie mit `CCActions` Standardwerte wie z. B. die Position und Drehung sowie benutzerdefinierten Werten interpolieren. Die `LineWidthState` und `LineWidthAction` Klassen gezeigt, wie eine benutzerdefinierte Aktion implementiert.

## <a name="related-links"></a>Verwandte Links

- [CCAction](https://developer.xamarin.com/api/type/CocosSharp.CCAction)
- [CCMoveTo](https://developer.xamarin.com/api/type/CocosSharp.CCMoveTo)
- [CCScaleTo](https://developer.xamarin.com/api/type/CocosSharp.CCScaleTo)
- [CCRotateTo](https://developer.xamarin.com/api/type/CocosSharp.CCRotateTo)
- [CCDrawNode](https://developer.xamarin.com/api/type/CocosSharp.CCDrawNode)
- [Das vollständige Codebeispiel](https://developer.xamarin.com/samples/mobile/CCAction/)
