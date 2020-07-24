---
title: Ereignisse, Protokolle und Delegaten in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie Sie in xamarin. IOS mit Ereignissen, Protokollen und Delegaten arbeiten. Diese grundlegenden Konzepte sind bei der xamarin. IOS-Entwicklung allgegenwärtig.
ms.prod: xamarin
ms.assetid: 7C07F0B7-9000-C540-0FC3-631C29610447
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/17/2017
ms.openlocfilehash: 1a7d7ec017bb226efb05014dc7ac80160aeaae48
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938349"
---
# <a name="events-protocols-and-delegates-in-xamarinios"></a>Ereignisse, Protokolle und Delegaten in xamarin. IOS

Xamarin. IOS verwendet Steuerelemente, um Ereignisse für die meisten Benutzerinteraktionen verfügbar zu machen.
Xamarin. IOS-Anwendungen nutzen diese Ereignisse ähnlich wie herkömmliche .NET-Anwendungen. Die xamarin. IOS-UIButton-Klasse verfügt beispielsweise über ein Ereignis namens touchupinside und nutzt dieses Ereignis genau so, als ob diese Klasse und dieses Ereignis in einer .net-App wären.

Neben diesem .net-Ansatz stellt xamarin. IOS ein anderes Modell zur Verfügung, das für komplexere Interaktionen und Daten Bindungen verwendet werden kann. Diese Methodik verwendet, was Apple zum Aufrufen von Delegaten und Protokollen aufruft. Delegaten sind in c# vergleichbar mit Delegaten, aber anstelle einer einzelnen Methode ist ein Delegat in Ziel-C eine ganze Klasse, die einem Protokoll entspricht. Ein Protokoll ähnelt einer Schnittstelle in c#, mit dem Unterschied, dass seine Methoden optional sein können. Um z. b. eine uitableview mit Daten aufzufüllen, würden Sie eine Delegatklasse erstellen, die die im uitableviewdatasource-Protokoll definierten Methoden implementiert, die von der uitableview aufgerufen werden, um sich selbst aufzufüllen.

In diesem Artikel erhalten Sie Informationen zu all diesen Themen, in denen Sie eine solide Grundlage für die Behandlung von Rückruf Szenarien in xamarin. IOS erhalten, einschließlich:

- **Ereignisse** – Verwenden von .NET-Ereignissen mit UIKit-Steuerelementen.
- **Protokolle** – lernen Sie, welche Protokolle und wie Sie verwendet werden, und erstellen Sie ein Beispiel, das Daten für eine Karten Anmerkung bereitstellt.
- **Delegaten – lernen** über Ziel-C-Delegaten, indem Sie das Map-Beispiel erweitern, um die Benutzerinteraktion zu verarbeiten, die eine Anmerkung enthält, und dann den Unterschied zwischen starken und schwachen Delegaten zu erlernen und zu verwenden.

Um Protokolle und Delegaten zu veranschaulichen, erstellen wir eine einfache Kartenanwendung, die eine Anmerkung zu einer Karte hinzufügt, wie hier gezeigt:

[ ![ Ein Beispiel für eine einfache Kartenanwendung, die](delegates-protocols-and-events-images/01-map-sml.png)](delegates-protocols-and-events-images/01-map.png#lightbox)einer Zuordnung eine Anmerkung hinzufügt, 
 [ ![ eine einer Karte hinzugefügte Beispiel Anmerkung](delegates-protocols-and-events-images/04-annotation-with-callout-sml.png)](delegates-protocols-and-events-images/04-annotation-with-callout.png#lightbox) .

Bevor Sie sich mit dieser APP befassen, sehen Sie sich die .net-Ereignisse unter der UIKit an.

## <a name="net-events-with-uikit"></a>.Net-Ereignisse mit UIKit

Xamarin. IOS macht .net-Ereignisse für UIKit-Steuerelemente verfügbar. UIButton hat z. b. ein touchupinside-Ereignis, das Sie wie in .NET verwenden, wie im folgenden Code gezeigt, in dem ein c#-Lambda-Ausdruck verwendet wird:

```csharp
aButton.TouchUpInside += (o,s) => {
    Console.WriteLine("button touched");
};
```

Sie können dies auch mit einer anonymen c# 2,0-Methode implementieren, die wie folgt aussieht:

```csharp
aButton.TouchUpInside += delegate {
    Console.WriteLine ("button touched");
};
```

Der vorangehende Code wird in der- `ViewDidLoad` Methode von UIViewController eingebunden. Die `aButton` Variable verweist auf eine Schaltfläche, die Sie entweder im IOS-Designer oder mit Code hinzufügen können. Die folgende Abbildung zeigt eine Schaltfläche, die im IOS-Designer hinzugefügt wurde:

[![Eine im IOS-Designer hinzugefügte Schaltfläche](delegates-protocols-and-events-images/02-interface-builder-outlet-sml.png)](delegates-protocols-and-events-images/02-interface-builder-outlet.png#lightbox)

Xamarin. IOS unterstützt auch den Ziel Aktions Stil der Verbindung Ihres Codes mit einer Interaktion, die mit einem-Steuerelement auftritt. Um eine Ziel Aktion für die Schaltfläche " **Hello** " zu erstellen, doppelklicken Sie im IOS-Designer darauf. Die Code-Behind-Datei von UIViewController wird angezeigt, und der Entwickler wird aufgefordert, einen Speicherort zum Einfügen der Verbindungsmethode auszuwählen:

[![Die Code Behind-Datei uiviewcontrollers](delegates-protocols-and-events-images/03-interface-builder-action-sml.png)](delegates-protocols-and-events-images/03-interface-builder-action.png#lightbox)

Nachdem Sie einen Speicherort ausgewählt haben, wird eine neue Methode erstellt und mit dem Steuerelement verknüpft. Im folgenden Beispiel wird eine Meldung in die Konsole geschrieben, wenn auf die Schaltfläche geklickt wird:

[![Wenn auf die Schaltfläche geklickt wird, wird eine Meldung in die Konsole geschrieben.](delegates-protocols-and-events-images/05-interface-builder-action-sml.png)](delegates-protocols-and-events-images/05-interface-builder-action.png#lightbox)

Weitere Informationen zum IOS-Ziel Aktions Muster finden Sie im Abschnitt "target-Action" unter Haupt [Anwendungs Kompetenzen für IOS](https://developer.apple.com/library/ios/#DOCUMENTATION/General/Conceptual/Devpedia-CocoaApp/TargetAction.html) in der IOS-Entwickler Bibliothek von Apple.

Weitere Informationen zur Verwendung des IOS-Designers mit xamarin. IOS finden Sie in der [Übersicht über den IOS-Designer](~/ios/user-interface/designer/index.md) .

## <a name="events"></a>Events

Wenn Sie Ereignisse von UIControl abfangen möchten, stehen Ihnen verschiedene Optionen zur Auswahl: von der Verwendung der c#-Lambdas und der Delegatfunktionen bis zur Verwendung der Low-Level-Ziel-C-APIs.

Der folgende Abschnitt zeigt, wie Sie das Touchdown-Ereignis auf einer Schaltfläche erfassen, je nachdem, wie viel Kontrolle Sie benötigen.

## <a name="c-style"></a>C#-Stil

Verwenden der delegatsyntax:

```csharp
UIButton button = MakeTheButton ();
button.TouchDown += delegate {
    Console.WriteLine ("Touched");
};
```

Wenn Sie stattdessen Lambdas verwenden möchten:

```csharp
button.TouchDown += () => {
   Console.WriteLine ("Touched");
};
```

Wenn Sie mehrere Schaltflächen verwenden möchten, verwenden Sie denselben Handler, um denselben Code gemeinsam zu verwenden:

```csharp
void handler (object sender, EventArgs args)
{
   if (sender == button1)
      Console.WriteLine ("button1");
   else
      Console.WriteLine ("some other button");
}

button1.TouchDown += handler;
button2.TouchDown += handler;
```

## <a name="monitoring-more-than-one-kind-of-event"></a>Überwachen von mehr als einer Art von Ereignis

Die c#-Ereignisse für uicontrolevent-Flags haben eine eins-zu-Eins-Zuordnung zu einzelnen Flags. Verwenden Sie die-Methode, wenn Sie über denselben Code verfügen möchten, der zwei oder mehr Ereignisse behandelt `UIControl.AddTarget` :

```csharp
button.AddTarget (handler, UIControlEvent.TouchDown | UIControlEvent.TouchCancel);
```

Verwenden der Lambda-Syntax:

```csharp
button.AddTarget ((sender, event)=> Console.WriteLine ("An event happened"), UIControlEvent.TouchDown | UIControlEvent.TouchCancel);
```

Wenn Sie Funktionen von Ziel-C auf niedriger Ebene verwenden müssen, z. b. mit einer bestimmten Objektinstanz verknüpfen und eine bestimmte Auswahl aufrufen:

```csharp
[Export ("MySelector")]
void MyObjectiveCHandler ()
{
    Console.WriteLine ("Hello!");
}

// In some other place:

button.AddTarget (this, new Selector ("MySelector"), UIControlEvent.TouchDown);
```

Beachten Sie, dass es sich um eine öffentliche Methode handeln muss, wenn Sie die Instanzmethode in einer geerbten Basisklasse implementieren.

## <a name="protocols"></a>Protokolle

Bei einem Protokoll handelt es sich um eine Programmiersprache, die eine Liste von Methoden Deklarationen bereitstellt. Dies dient einem ähnlichen Zweck wie eine Schnittstelle in c#. der Hauptunterschied besteht darin, dass ein Protokoll optionale Methoden aufweisen kann. Optionale Methoden werden nicht aufgerufen, wenn die Klasse, die ein Protokoll annimmt, Sie nicht implementiert. Außerdem kann eine einzelne Klasse in "Ziel-C" mehrere Protokolle implementieren, ebenso wie eine c#-Klasse mehrere Schnittstellen implementieren kann.

Apple verwendet Protokolle in Ios, um Verträge für zu über mittelende Klassen zu definieren, während gleichzeitig die implementierende Klasse vom Aufrufer entfernt wird, sodass Sie genau wie eine c#-Schnittstelle funktioniert. Protokolle werden sowohl in nicht-delegatszenarios (z. b. im `MKAnnotation` folgenden Beispiel) als auch mit Delegaten (wie weiter unten in diesem Dokument im delegatenabschnitt dargestellt) verwendet.

### <a name="protocols-with-xamarinios"></a>Protokolle mit xamarin. IOS

Werfen wir einen Blick auf ein Beispiel, das ein Ziel-C-Protokoll von xamarin. IOS verwendet. In diesem Beispiel verwenden wir das `MKAnnotation` Protokoll, das Teil des `MapKit` Frameworks ist. `MKAnnotation`ein Protokoll, mit dem jedes Objekt, das es annimmt, Informationen zu einer Anmerkung bereitstellen kann, die einer Zuordnung hinzugefügt werden kann. Ein Objekt, das implementiert, `MKAnnotation` stellt z. b. den Speicherort der Anmerkung und den ihm zugeordneten Titel bereit.

Auf diese Weise wird das `MKAnnotation` Protokoll verwendet, um relevante Daten bereitzustellen, die mit einer Anmerkung einhergehen. Die tatsächliche Ansicht für die Anmerkung selbst wird aus den Daten im Objekt erstellt, das das Protokoll annimmt `MKAnnotation` . Beispielsweise wird der Text für die Legende, der angezeigt wird, wenn der Benutzer auf die-Anmerkung tippt (wie im nachfolgenden Screenshot gezeigt), von der- `Title` Eigenschaft in der-Klasse abgeleitet, die das Protokoll implementiert:

 [![Beispiel Text für die Legende, wenn der Benutzer auf die Anmerkung tippt](delegates-protocols-and-events-images/04-annotation-with-callout-sml.png)](delegates-protocols-and-events-images/04-annotation-with-callout.png#lightbox)

Wie im nächsten Abschnitt beschrieben, werden Protokolle in [Deep Dive](#protocols-deep-dive), xamarin. IOS, an abstrakte Klassen gebunden. Für das `MKAnnotation` Protokoll wird die gebundene c#-Klasse so benannt, dass `MKAnnotation` Sie den Namen des Protokolls imitiert, und Sie ist eine Unterklasse von `NSObject` , der Basisklasse für cocoatouch. Das Protokoll erfordert, dass ein Getter und ein Setter für die Koordinate implementiert werden. ein Titel und Untertitel sind jedoch optional. Daher ist in der- `MKAnnotation` Klasse die `Coordinate` -Eigenschaft *abstrakt*und erfordert, dass Sie implementiert wird und die `Title` -Eigenschaft und die-Eigenschaft `Subtitle` als *virtuell*gekennzeichnet sind, sodass Sie optional sind, wie unten dargestellt:

```csharp
[Register ("MKAnnotation"), Model ]
public abstract class MKAnnotation : NSObject
{
    public abstract CLLocationCoordinate2D Coordinate
    {
        [Export ("coordinate")]
        get;
        [Export ("setCoordinate:")]
        set;
    }

    public virtual string Title
    {
        [Export ("title")]
        get
        {
            throw new ModelNotImplementedException ();
        }
    }

    public virtual string Subtitle
    {
        [Export ("subtitle")]
        get
        {
            throw new ModelNotImplementedException ();
        }
    }
...
}
```

Jede Klasse kann Anmerkung-Daten durch einfaches Ableiten von bereitstellen, solange mindestens `MKAnnotation` die- `Coordinate` Eigenschaft implementiert ist. Im folgenden finden Sie eine Beispiel Klasse, die die-Koordinate im Konstruktor annimmt und eine Zeichenfolge für den Titel zurückgibt:

```csharp
/// <summary>
/// Annotation class that subclasses MKAnnotation abstract class
/// MKAnnotation is bound by Xamarin.iOS to the MKAnnotation protocol
/// </summary>
public class SampleMapAnnotation : MKAnnotation
{
    string title;

    public SampleMapAnnotation (CLLocationCoordinate2D coordinate)
    {
        Coordinate = coordinate;
        title = "Sample";
    }

    public override CLLocationCoordinate2D Coordinate { get; set; }

    public override string Title {
        get {
            return title;
        }
    }
}
```

Durch das Protokoll, an das es gebunden ist, kann jede Klasse, die Unterklassen `MKAnnotation` relevante Daten bereitstellen, die von der Zuordnung verwendet werden, wenn die Ansicht der Anmerkung erstellt wird. Um einer Zuordnung eine Anmerkung hinzuzufügen, nennen Sie einfach die-Methode einer- `AddAnnotation` `MKMapView` Instanz, wie im folgenden Code gezeigt:

```csharp
//an arbitrary coordinate used for demonstration here
var sampleCoordinate =
    new CLLocationCoordinate2D (42.3467512, -71.0969456); // Boston

//create an annotation and add it to the map
map.AddAnnotation (new SampleMapAnnotation (sampleCoordinate));
```

Bei der hier angegebenen Karten Variablen handelt es sich um eine Instanz von `MKMapView` , die die-Klasse darstellt, die die Karte selbst darstellt. `MKMapView`Verwendet die `Coordinate` Daten, die von der- `SampleMapAnnotation` Instanz abgeleitet werden, um die Anmerkung-Ansicht auf der Karte zu positionieren.

Das `MKAnnotation` Protokoll bietet einen bekannten Satz von Funktionen für alle Objekte, die es implementieren, ohne dass der Consumer (in diesem Fall die Zuordnung) über Implementierungsdetails wissen muss. Dadurch wird das Hinzufügen einer Vielzahl möglicher Anmerkungen zu einer Karte optimiert.

### <a name="protocols-deep-dive"></a>Ausführliche Informationen zu Protokollen

Da c#-Schnittstellen keine optionalen Methoden unterstützen, ordnet xamarin. IOS Protokolle abstrakten Klassen zu. Daher wird die Übernahme eines Protokolls in Ziel-C in xamarin. IOS durch Ableiten von der abstrakten-Klasse, die an das Protokoll gebunden ist, und Implementieren der erforderlichen Methoden erreicht. Diese Methoden werden als abstrakte Methoden in der-Klasse verfügbar gemacht. Optionale Methoden aus dem Protokoll werden an virtuelle Methoden der c#-Klasse gebunden.

Hier sehen Sie z. b. einen Teil des Protokolls, der `UITableViewDataSource` in xamarin. IOS gebunden ist:

```csharp
public abstract class UITableViewDataSource : NSObject
{
    [Export ("tableView:cellForRowAtIndexPath:")]
    public abstract UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath);
    [Export ("numberOfSectionsInTableView:")]
    public virtual int NumberOfSections (UITableView tableView){...}
...
}
```

Beachten Sie, dass die-Klasse abstrakt ist. Xamarin. IOS macht die-Klasse abstrakt, um optionale/erforderliche Methoden in Protokollen zu unterstützen.
Anders als bei Ziel-C-Protokollen (oder c#-Schnittstellen) unterstützen c#-Klassen jedoch nicht die mehrfache Vererbung. Dies wirkt sich auf den Entwurf von c#-Code aus, der Protokolle verwendet, und führt in der Regel zu den Klassen. Weitere Informationen zu diesem Problem finden Sie weiter unten in diesem Dokument im Abschnitt Delegaten.

 `GetCell(…)`ist eine abstrakte Methode, die an den Ziel-C- *Selektor*gebunden ist, bei `tableView:cellForRowAtIndexPath:` dem es sich um eine erforderliche Methode des `UITableViewDataSource` Protokolls handelt. Selector ist der Begriff "Ziel-C" für den Methodennamen. Um die Methode nach Bedarf zu erzwingen, deklariert xamarin. IOS Sie als abstrakt. Die andere Methode, `NumberOfSections(…)` , ist an gebunden `numberOfSectionsInTableview:` . Diese Methode ist im Protokoll optional, sodass xamarin. IOS Sie als virtuell deklariert, sodass es optional ist, in c# zu überschreiben.

Xamarin. IOS kümmert sich um alle IOS-Bindungen. Wenn Sie jedoch jemals ein Protokoll manuell von Ziel-C binden müssen, können Sie dazu eine Klasse mit dem versehen `ExportAttribute` . Dabei handelt es sich um dieselbe Methode, die auch von xamarin. IOS selbst verwendet wird.

Weitere Informationen zum Binden von Ziel-c-Typen in xamarin. IOS finden Sie im Artikel [Binden von Ziel-c-Typen](~/ios/platform/binding-objective-c/index.md).

Wir sind jedoch noch nicht mit Protokollen vertraut. Sie werden auch in ios als Grundlage für Ziel-C-Delegaten verwendet. Dies ist das Thema des nächsten Abschnitts.

## <a name="delegates"></a>Delegaten

IOS verwendet Ziel-C-Delegaten, um das Delegierungs Muster zu implementieren, in dem ein Objekt die Arbeit an eine andere übergibt. Das Objekt, das die Arbeit leistet, ist der Delegat des ersten Objekts. Ein-Objekt weist seinen Delegaten an, Aufgaben auszuführen, indem es ihm Nachrichten sendet, nachdem bestimmte Dinge auftreten. Das Senden einer Nachricht wie dieser in Ziel-C ist funktional äquivalent zum Aufrufen einer Methode in c#. Ein Delegat implementiert Methoden als Reaktion auf diese Aufrufe und stellt somit Funktionen für die Anwendung bereit.

Delegaten ermöglichen es Ihnen, das Verhalten von Klassen zu erweitern, ohne Unterklassen erstellen zu müssen. Anwendungen in ios verwenden häufig Delegaten, wenn eine Klasse nach einer wichtigen Aktion an eine andere zurückgibt. Beispielsweise ruft die- `MKMapView` Klasse den Delegaten zurück, wenn der Benutzer auf eine Anmerkung in einer Zuordnung tippt, und gibt dem Autor der Delegatklasse die Möglichkeit, innerhalb der Anwendung zu antworten. Ein Beispiel für diese Art der delegatverwendung finden Sie weiter unten in diesem Artikel, z. b. mit einem Delegaten mit xamarin. IOS.

An diesem Punkt Fragen Sie sich vielleicht, wie eine Klasse bestimmt, welche Methoden für Ihren Delegaten aufgerufen werden müssen. Dies ist ein weiterer Ort, an dem Sie Protokolle verwenden. Normalerweise stammen die Methoden, die für einen Delegaten verfügbar sind, von den Protokollen, die Sie übernehmen

### <a name="how-protocols-are-used-with-delegates"></a>Verwendung von Protokollen mit Delegaten

Wir haben bereits gesehen, wie Protokolle verwendet werden, um das Hinzufügen von Anmerkungen zu einer Karte zu unterstützen.
Protokolle werden auch zum Bereitstellen eines bekannten Satzes von Methoden für Klassen verwendet, die nach bestimmten Ereignissen aufgerufen werden können, z. b. Nachdem der Benutzer auf eine Anmerkung in einer Karte tippt oder eine Zelle in einer Tabelle ausgewählt hat. Die Klassen, die diese Methoden implementieren, werden als Delegaten der Klassen bezeichnet, die diese Methoden aufzurufen.

Klassen, die die Delegierung unterstützen, machen eine delegateigenschaft verfügbar, der eine Klasse zugewiesen ist, die den Delegaten implementiert. Die Methoden, die Sie für den Delegaten implementieren, hängen von dem Protokoll ab, das der jeweilige Delegat annimmt. Für die- `UITableView` Methode implementieren Sie das `UITableViewDelegate` -Protokoll für die `UIAccelerometer` -Methode, die Sie implementieren `UIAccelerometerDelegate` , usw. für alle anderen Klassen in Ios, für die Sie einen Delegaten verfügbar machen möchten.

Die `MKMapView` Klasse, die wir in unserem vorherigen Beispiel gesehen haben, verfügt auch über eine Eigenschaft namens "Delegat", die nach dem Auftreten verschiedener Ereignisse aufgerufen wird. Der Delegat für `MKMapView` ist vom Typ `MKMapViewDelegate` .
Dies wird in Kürze in einem Beispiel verwendet, um auf die-Anmerkung zu reagieren, nachdem Sie ausgewählt wurde, aber zunächst wird der Unterschied zwischen starken und schwachen Delegaten erörtert.

### <a name="strong-delegates-vs-weak-delegates"></a>Starke Delegaten im Vergleich zu schwachen Delegaten

Die bisher untersuchten Delegaten sind starke Delegaten, was bedeutet, dass Sie stark typisiert sind. Die xamarin. IOS-Bindungen liefern eine stark typisierte Klasse für jedes delegatprotokoll in ios. IOS hat jedoch auch das Konzept eines schwachen Delegaten. Anstatt eine Klasse zu Unterklassen, die an das Ziel-C-Protokoll für einen bestimmten Delegaten gebunden ist, können Sie mit IOS auch die Protokoll Methoden selbst an eine beliebige Klasse binden, die von NSObject abgeleitet ist, ihre Methoden mit dem Export Attribute versehen und dann die entsprechenden Selektoren bereitstellt.
Wenn Sie diesen Ansatz verwenden, weisen Sie der weakdelegateigenschaft anstelle der delegateigenschaft eine Instanz der Klasse zu. Ein schwacher Delegat bietet Ihnen die Flexibilität, ihre Delegatklasse in eine andere Vererbungs Hierarchie zu bringen. Sehen wir uns ein xamarin. IOS-Beispiel an, in dem sowohl starke als auch schwache Delegaten verwendet werden.

### <a name="example-using-a-delegate-with-xamarinios"></a>Beispiel für die Verwendung eines Delegaten mit xamarin. IOS

Um Code als Reaktion auf den Benutzer auszuführen, der in unserem Beispiel auf die-Anmerkung tippt, können wir eine-Klasse Unterklassen `MKMapViewDelegate` und der `MKMapView` -Eigenschaft von eine-Instanz zuweisen `Delegate` . Das `MKMapViewDelegate` Protokoll enthält nur optionale Methoden.
Daher sind alle Methoden virtuell, die an dieses Protokoll in der xamarin. IOS-Klasse gebunden sind `MKMapViewDelegate` . Wenn der Benutzer eine Anmerkung auswählt, `MKMapView` sendet die Instanz die `mapView:didSelectAnnotationView:` Nachricht an Ihren Delegaten. Um dies in xamarin. IOS zu behandeln, müssen wir die- `DidSelectAnnotationView (MKMapView mapView, MKAnnotationView annotationView)` Methode in der mkmapviewdelegat-unter Klasse wie folgt überschreiben:

```csharp
public class SampleMapDelegate : MKMapViewDelegate
{
    public override void DidSelectAnnotationView (
        MKMapView mapView, MKAnnotationView annotationView)
    {
        var sampleAnnotation =
            annotationView.Annotation as SampleMapAnnotation;

        if (sampleAnnotation != null) {

            //demo accessing the coordinate of the selected annotation to
            //zoom in on it
            mapView.Region = MKCoordinateRegion.FromDistance(
                sampleAnnotation.Coordinate, 500, 500);

            //demo accessing the title of the selected annotation
            Console.WriteLine ("{0} was tapped", sampleAnnotation.Title);
        }
    }
}
```

Die oben gezeigte samplemapdelegatklasse wird als eine geclusterte Klasse im Controller implementiert, der die `MKMapView` Instanz enthält. In Ziel-C sehen Sie häufig, dass der Controller mehrere Protokolle direkt innerhalb der Klasse übernehmen kann. Da Protokolle jedoch an Klassen in xamarin. IOS gebunden sind, werden die Klassen, die stark typisierte Delegaten implementieren, in der Regel als Klassen eingefügt.

Wenn die Implementierung der Delegatklasse vorhanden ist, müssen Sie nur eine Instanz des Delegaten im Controller instanziieren und Sie der-Eigenschaft des-Delegaten zuweisen, `MKMapView` `Delegate` wie hier gezeigt:

```csharp
public partial class Protocols_Delegates_EventsViewController : UIViewController
{
    SampleMapDelegate _mapDelegate;
    ...
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();

        //set the map's delegate
        _mapDelegate = new SampleMapDelegate ();
        map.Delegate = _mapDelegate;
        ...
    }
    class SampleMapDelegate : MKMapViewDelegate
    {
        ...
    }
}
```

Wenn Sie einen schwachen Delegaten zum gleichen Zweck verwenden möchten, müssen Sie die Methode selbst an eine beliebige Klasse binden, die von abgeleitet ist, `NSObject` und Sie der-Eigenschaft der-Klasse zuweisen `WeakDelegate` `MKMapView` . Da die- `UIViewController` Klasse letztendlich von abgeleitet `NSObject` wird (wie jede Ziel-C-Klasse in cocoatouch), können wir einfach eine Methode implementieren, die `mapView:didSelectAnnotationView:` direkt an den Controller gebunden ist, und den Controller dem Controller zuweisen, sodass die zusätzliche, nicht erforderliche Klasse nicht `MKMapView` `WeakDelegate` mehr benötigt wird. Der folgende Code veranschaulicht diese Vorgehensweise:

```csharp
public partial class Protocols_Delegates_EventsViewController : UIViewController
{
    ...
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
        //assign the controller directly to the weak delegate
        map.WeakDelegate = this;
    }
    //bind to the Objective-C selector mapView:didSelectAnnotationView:
    [Export("mapView:didSelectAnnotationView:")]
    public void DidSelectAnnotationView (MKMapView mapView,
        MKAnnotationView annotationView)
    {
        ...
    }
}
```

Wenn Sie diesen Code ausführen, verhält sich die Anwendung genau so wie beim Ausführen der stark typisierten delegatversion. Der Vorteil dieses Codes besteht darin, dass der schwache Delegat nicht die Erstellung der zusätzlichen Klasse erfordert, die erstellt wurde, als der stark typisierte Delegat verwendet wurde. Dies erfolgt jedoch auf Kosten der Typsicherheit. Wenn Sie einen Fehler in der Auswahl treffen würden, die an das-Modul übermittelt wurde `ExportAttribute` , können Sie bis zur Laufzeit nicht ermitteln.

### <a name="events-and-delegates"></a>Ereignisse und Delegaten

Delegaten werden für Rückrufe in ios ähnlich der Art und Weise verwendet, in der .net Ereignisse verwendet. Um IOS-APIs und die Verwendung von Ziel-C-Delegaten zu ermöglichen, werden .net-Ereignisse an vielen Stellen angezeigt, an denen Delegaten in ios verwendet werden.

Beispielsweise könnte die frühere Implementierung, bei der der `MKMapViewDelegate` auf eine ausgewählte Anmerkung geantwortet hat, auch in xamarin. IOS mithilfe eines .net-Ereignisses implementiert werden. In diesem Fall würde das Ereignis in definiert `MKMapView` und aufgerufen werden `DidSelectAnnotationView` . Sie verfügt über eine `EventArgs` Unterklasse des Typs `MKMapViewAnnotationEventsArgs` . Mit der- `View` Eigenschaft von `MKMapViewAnnotationEventsArgs` erhalten Sie einen Verweis auf die Anmerkung-Ansicht, von der aus Sie mit derselben Implementierung fortfahren können, die Sie zuvor gesehen haben:

```csharp
map.DidSelectAnnotationView += (s,e) => {
    var sampleAnnotation = e.View.Annotation as SampleMapAnnotation;
    if (sampleAnnotation != null) {
        //demo accessing the coordinate of the selected annotation to
        //zoom in on it
        mapView.Region = MKCoordinateRegion.FromDistance (
            sampleAnnotation.Coordinate, 500, 500);

        //demo accessing the title of the selected annotation
        Console.WriteLine ("{0} was tapped", sampleAnnotation.Title);
    }
};
```

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde beschrieben, wie Ereignisse, Protokolle und Delegaten in xamarin. IOS verwendet werden. Wir haben gesehen, wie xamarin. IOS normale .net-Stil Ereignisse für Steuerelemente verfügbar macht.
Als nächstes haben wir uns mit den Zielen der Ziele C vertraut gemacht, einschließlich der Unterschiede zwischen c#-Schnittstellen und der Verwendung von xamarin. IOS. Schließlich haben wir die Ziel-C-Delegaten aus einer xamarin. IOS-Perspektive untersucht. Wir haben gesehen, wie xamarin. IOS sowohl starke als auch schwach typisierte Delegaten unterstützt und wie .net-Ereignisse an Delegatmethoden gebunden werden.

## <a name="related-links"></a>Verwandte Links

- [Protokolle, Delegaten und Ereignisse (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/protocols-delegates-events)
- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [Bindungs Ziel-C-Typen](~/ios/platform/binding-objective-c/index.md)
- [Die Programmiersprache "Ziel-C"](https://developer.apple.com/library/ios/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
- [Entwerfen von Benutzeroberflächen in Xcode 4](https://developer.apple.com/library/ios/#documentation/IDEs/Conceptual/Xcode4TransitionGuide/InterfaceBuilder/InterfaceBuilder.html)
- [Grundlegende Anwendungs Kompetenzen für IOS](https://developer.apple.com/library/ios/#DOCUMENTATION/General/Conceptual/Devpedia-CocoaApp/TargetAction.html)
