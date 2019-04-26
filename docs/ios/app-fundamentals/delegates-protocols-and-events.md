---
title: Ereignisse, Protokolle und Delegaten in Xamarin.iOS
description: Dieses Dokument beschreibt das Arbeiten mit Ereignissen, Protokolle und Delegaten in Xamarin.iOS. Diese grundlegende Konzepte werden in der Entwicklung mit Xamarin.iOS allgegenwärtig.
ms.prod: xamarin
ms.assetid: 7C07F0B7-9000-C540-0FC3-631C29610447
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 09/17/2017
ms.openlocfilehash: 5ccefdb5e527e67338714896905734c74278d00a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61392472"
---
# <a name="events-protocols-and-delegates-in-xamarinios"></a>Ereignisse, Protokolle und Delegaten in Xamarin.iOS

Xamarin.iOS verwendet Steuerelemente, um Ereignisse für die meisten Interaktionen der Benutzer verfügbar zu machen.
Xamarin.iOS-Anwendungen nutzen diese Ereignisse in die gleiche Weise, wie herkömmliche Anwendungen für .NET. Beispielsweise wird die Xamarin.iOS-UIButton-Klasse hat ein Ereignis namens TouchUpInside und dieses Ereignis verarbeitet, als ob diese Klasse und das Ereignis in einer .NET-App wären.

Neben diesem Ansatz .NET macht Xamarin.iOS ein anderes Modell, das für eine komplexere Interaktion und die Datenbindung verwendet werden kann. Diese Methode wird verwendet, was Apple Delegaten und Protokolle aufruft. Delegaten ähneln in Ihrem Konzept an Delegaten in C#, aber anstatt definieren und Aufrufen einer einzelnen Methode, ist ein Delegat in Objective-C eine ganze Klasse, die ein Protokoll entspricht. Ein Protokoll ähnelt eine Schnittstelle in C#, außer dass seine Methoden optional sein können. Um eine UITableView mit Daten aufzufüllen, würden Sie z. B. eine "Delegate"-Klasse erstellen, in das UITableViewDataSource-Protokoll, das zum Auffüllen der UITableView aufrief definierten Methoden implementiert, ist.

In diesem Artikel Sie, alle diese Themen erfahren bietet Ihnen eine solide Grundlage für die Behandlung von Rückrufszenarien in Xamarin.iOS, einschließlich:

- **Ereignisse** – .NET mithilfe von Ereignissen mit UIKit-Steuerelementen.
- **Protokolle** – was Protokolle sind und wie sie verwendet werden, und erstellen ein Beispiel, stellt Daten für eine Anmerkung für die Zuordnung bereit.
- **Delegaten** – Learning zu Objective-C-Delegaten durch Erweitern des Beispiels Karte, um die Verarbeitung von Benutzerinteraktionen, die eine Anmerkung enthält, und lernen den Unterschied zwischen starke und schwache Delegaten und wann diese verwendet.

Um Protokolle und Delegaten zu veranschaulichen, erstellen wir eine einfache Zuordnung-Anwendung, die eine Anmerkung zu einer Zuordnung hinzugefügt wird, wie hier gezeigt:

[![](delegates-protocols-and-events-images/01-map-sml.png "Ein Beispiel für eine einfache Zuordnung-Anwendung, die eine Anmerkung zu einer Zuordnung fügt")](delegates-protocols-and-events-images/01-map.png#lightbox)
[![](delegates-protocols-and-events-images/04-annotation-with-callout-sml.png "eine Beispiel-Anmerkung zu einer Zuordnung hinzugefügt")](delegates-protocols-and-events-images/04-annotation-with-callout.png#lightbox)

Vor dem Umgang mit dieser app ein, lassen Sie uns loslegen indem Sie Ereignisse unter der UIKit ansehen

## <a name="net-events-with-uikit"></a>Ereignisse mit UIKit

Xamarin.iOS macht .NET-Ereignisse auf UIKit-Steuerelementen verfügbar. UIButton hat beispielsweise ein TouchUpInside-Ereignis, das Sie behandeln wie gewohnt in .NET verwenden, wie im folgenden Code dargestellt, die verwendet eine C# Lambda-Ausdruck:

```csharp
aButton.TouchUpInside += (o,s) => {
    Console.WriteLine("button touched");
};
```

Sie könnten auch implementieren, dies mit einem C# anonyme Methode 2.0-Format, wie die folgende:

```csharp
aButton.TouchUpInside += delegate {
    Console.WriteLine ("button touched");
};
```

Der vorangehende Code läuft der `ViewDidLoad` Methode der UIViewController. Die `aButton` Variable verweist auf eine Schaltfläche mit der Sie in der iOS-Designer oder mit Code hinzufügen können. Die folgende Abbildung zeigt eine Schaltfläche, die in der iOS-Designer hinzugefügt wurde:

[![](delegates-protocols-and-events-images/02-interface-builder-outlet-sml.png "Eine Schaltfläche hinzugefügt, die im iOS-Designer")](delegates-protocols-and-events-images/02-interface-builder-outlet.png#lightbox)

Xamarin.iOS unterstützt auch das Ziel-Aktionsformat Herstellen einer Verbindung eine Interaktion, die mit einem Steuerelement auftritt, mit Ihrem Code. Erstellen Sie eine Ziel-Aktion für die **Hello** Schaltfläche, doppelklicken sie in der iOS-Designer. Der UIViewControllers Code-Behind-Datei wird angezeigt, und der Entwickler aufgefordert, wählen Sie einen Speicherort für die Verbindung Methode eingefügt wird:

[![](delegates-protocols-and-events-images/03-interface-builder-action-sml.png "UIViewControllers Code-Behind-Datei")](delegates-protocols-and-events-images/03-interface-builder-action.png#lightbox)

Nachdem Sie ein Standort ausgewählt ist, eine neue Methode erstellt und wired oben auf das Steuerelement. Im folgenden Beispiel wird eine Nachricht an die Konsole geschrieben werden, wenn die Schaltfläche geklickt wird:

[![](delegates-protocols-and-events-images/05-interface-builder-action-sml.png "Eine Nachricht wird an die Konsole geschrieben werden, wenn die Schaltfläche geklickt wird")](delegates-protocols-and-events-images/05-interface-builder-action.png#lightbox)

Weitere Informationen über das iOS-Ziel-Action-Muster finden Sie im Abschnitt Ziel-Aktion [Anwendung Kernkompetenzen für iOS](https://developer.apple.com/library/ios/#DOCUMENTATION/General/Conceptual/Devpedia-CocoaApp/TargetAction.html) in Apple iOS-Entwicklerbibliothek.

Weitere Informationen zur Verwendung des iOS-Designers mit Xamarin.iOS finden Sie unter den [iOS Designer Overview](~/ios/user-interface/designer/index.md) Dokumentation.

## <a name="events"></a>Ereignisse

Wenn Sie Ereignisse aus UIControl abfangen möchten, müssen Sie eine Reihe von Möglichkeiten: von der Verwendung der C# Lambda-Ausdrücke und Delegatfunktionen, um unter Verwendung der Low-Level Objective-C-APIs.

Der folgende Abschnitt zeigt, wie Sie auf eine Schaltfläche, je nachdem wie viel Kontrolle des TouchDown-Ereignisses erfassen würden, die Sie benötigen.

## <a name="c-style"></a>C#Stil

Verwenden die Syntax für Delegaten ein:

```csharp
UIButton button = MakeTheButton ();
button.TouchDown += delegate {
    Console.WriteLine ("Touched");
};
```

Wenn Sie Lambdas stattdessen etwa:

```csharp
button.TouchDown += () => {
   Console.WriteLine ("Touched");
};
```

Wenn Sie möchten mithilfe mehrerer Schaltflächen den gleichen Handler denselben Code verwenden:

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

## <a name="monitoring-more-than-one-kind-of-event"></a>Überwachen von mehr als eine Art von Ereignissen

Die C# Ereignisse für UIControlEvent Flags haben eine 1: 1-Zuordnung zu einzelnen Flags. Wenn Sie möchten den gleichen Codeabschnitt zwei oder mehr Ereignisse behandeln, verwenden Sie die `UIControl.AddTarget` Methode:

```csharp
button.AddTarget (handler, UIControlEvent.TouchDown | UIControlEvent.TouchCancel);
```

Mithilfe der lambdasyntax:

```csharp
button.AddTarget ((sender, event)=> Console.WriteLine ("An event happened"), UIControlEvent.TouchDown | UIControlEvent.TouchCancel);
```

Wenn Sie die Low-Level-Funktionen von Objective-C, wie auf eine bestimmte Objektinstanz einbinden und Aufrufen einer bestimmten Auswahl verwenden möchten:

```csharp
[Export ("MySelector")]
void MyObjectiveCHandler ()
{
    Console.WriteLine ("Hello!");
}

// In some other place:

button.AddTarget (this, new Selector ("MySelector"), UIControlEvent.TouchDown);
```

Beachten Sie, wenn Sie die Instanzmethode in einer geerbten Klasse implementieren, muss er eine öffentliche Methode sein.

## <a name="protocols"></a>Protokolle

Ein Protokoll ist ein Objective-C-Sprachfunktion, die eine Liste der Deklarationen von Instanzenmethoden bereitstellt. Es dient einen ähnlichen Zweck einer Schnittstelle in C#, der Hauptunterschied besteht darin, dass ein Protokoll die optionale Methoden haben kann. Optionale Methoden werden nicht aufgerufen, wenn die Klasse, die ein Protokoll übernimmt nicht implementiert wird. Darüber hinaus kann eine einzelne Klasse in Objective-C-mehrere Protokolle implementieren, wie eine C# Klasse kann mehrere Schnittstellen implementieren.

Apple verwendet Protokolle in der gesamten iOS Verträge für Klassen, übernehmen während die implementierende Klasse aus dem Aufrufer so ausgeführt wie außer acht gelassen, definieren eine C# Schnittstelle. Protokolle werden verwendet, sowohl in Szenarien mit nicht-Delegaten (z. B. mit der `MKAnnotation` Beispiel weiter), und mit Delegaten (wie weiter unten in diesem Dokument im Abschnitt Delegaten dargestellt wird).

### <a name="protocols-with-xamarinios"></a>Protokolle mit Xamarin.ios

Werfen wir einen Blick auf ein Beispiel zur Verwendung von Xamarin.iOS auf ein Objective-C-Protokoll. In diesem Beispiel verwenden wir die `MKAnnotation` Protokoll, das ist Teil von der `MapKit` Framework. `MKAnnotation` ist ein Protokoll, das jedes beliebige Objekt ermöglicht, die übernimmt, um Informationen zu einer Anmerkung zu liefern, die zu einer Zuordnung hinzugefügt werden können. Z. B. ein Objekt, das `MKAnnotation` gibt die Position der Anmerkung und den Titel zugeordnet.

Auf diese Weise die `MKAnnotation` Protokoll wird verwendet, um relevante Daten bereitstellen, die mit einer Anmerkung. Die tatsächliche Ansicht für die Anmerkung selbst basiert auf dem die Daten in das Objekt, das übernimmt die `MKAnnotation` Protokoll. Zum Beispiel der Text für die Legende, die angezeigt wird, wenn der Benutzer auf die Anmerkung tippt, (wie im folgenden Screenshot gezeigt) stammt aus dem `Title` Eigenschaft in der Klasse, die das Protokoll implementiert:

 [![](delegates-protocols-and-events-images/04-annotation-with-callout-sml.png "Beispieltext für die Legende aus, wenn der Benutzer, auf die Anmerkung tippt")](delegates-protocols-and-events-images/04-annotation-with-callout.png#lightbox)

Im nächsten Abschnitt beschriebenen [tieferer Einblick in Protokolle](#protocols-deep-dive), Xamarin.iOS bindet die Protokolle an abstrakte Klassen. Für die `MKAnnotation` Protokoll, die Grenze C# Klasse `MKAnnotation` imitieren den Namen des Protokolls, und es ist eine Unterklasse von `NSObject`, die Stamm-Basisklasse für CocoaTouch. Das Protokoll muss ein Getter und Setter für die Koordinate implementiert werden; Titel und Untertitel sind jedoch optional. Aus diesem Grund in die `MKAnnotation` -Klasse, die `Coordinate` -Eigenschaft ist *abstrakte*, implementiert werden muss und die `Title` und `Subtitle` Eigenschaften sind markiert *virtuellen* , sodass sie optional, wie unten dargestellt:

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

Jede Klasse kann Anmerkungsdaten bereitstellen, indem Sie einfach eine Ableitung von `MKAnnotation`, solange mindestens die `Coordinate` Eigenschaft implementiert wird. So sieht z. B. eine Beispielklasse, die Koordinate im Konstruktor akzeptiert und gibt eine Zeichenfolge für den Titel, aus:

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

Über das Protokoll ist es an, die eine beliebige Klasse diese Unterklassen `MKAnnotation` bieten relevante Daten, die von der Zuordnung verwendet werden, wenn die Anmerkung in der Ansicht erstellt. Um eine Anmerkung zu einer Zuordnung hinzuzufügen, rufen Sie einfach die `AddAnnotation` Methode eine `MKMapView` -Instanz, wie im folgenden Code gezeigt:

```csharp
//an arbitrary coordinate used for demonstration here
var sampleCoordinate =
    new CLLocationCoordinate2D (42.3467512, -71.0969456); // Boston

//create an annotation and add it to the map
map.AddAnnotation (new SampleMapAnnotation (sampleCoordinate));
```

Die Map-Variable hier ist eine Instanz einer `MKMapView`, dies ist die Klasse, die die Karte selbst darstellt. Die `MKMapView` verwendet die `Coordinate` Daten stammen aus der `SampleMapAnnotation` Instanz zum Positionieren der Ansicht "Anmerkung" auf der Karte.

Die `MKAnnotation` -Protokoll bietet einen bekannten Satz von Funktionen für alle Objekte, die sie implementieren, ohne dass des Consumers (die Schritte in diesem Fall) Implementierungsdetails kennen müssen. Dies optimiert die eine Vielzahl von möglichen Anmerkungen zu einer Karte hinzufügen.

### <a name="protocols-deep-dive"></a>Tieferer Einblick in Protokolle

Da C# Schnittstellen unterstützen keine optionale Methoden, Xamarin.iOS Protokolle abstrakte Klassen zugeordnet. Aus diesem Grund ein Protokoll in Objective-C-Übernahme erfolgt in Xamarin.iOS durch Ableiten von der abstrakten Klasse, die für das Protokoll gebunden ist, und die erforderlichen Methoden implementiert. Diese Methoden werden als abstrakte Methoden in der Klasse verfügbar gemacht werden. Optionale Methoden vom Protokoll werden an virtuellen Methoden gebunden werden die C# Klasse.

Hier ist beispielsweise ein Teil der `UITableViewDataSource` gebundenen in Xamarin.iOS-Protokoll:

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

Beachten Sie, dass die Klasse abstrakt ist. Xamarin.iOS macht die Klasse, die abstrakte Methoden optional/erforderlich in Protokolle zu unterstützen.
Jedoch im Gegensatz zu Objective-C-Protokolle (oder C# Schnittstellen), C# Klassen keine mehrfache Vererbung unterstützen. Dies wirkt sich auf den Entwurf der C# Code, der Protokolle verwendet und in der Regel führt zu einer geschachtelte Klassen. Weitere Informationen zu diesem Problem weiter unten in diesem Dokument im Abschnitt Delegaten fällt.

 `GetCell(…)` ist eine abstrakte Methode, die an die Objective-C gebunden *Selektor*, `tableView:cellForRowAtIndexPath:`, dies ist eine erforderliche Methode von der `UITableViewDataSource` Protokoll. Die Auswahl ist die Objective-C-Laufzeit der Methodenname. Um die Methode nach Bedarf zu erzwingen, Xamarin.iOS das es als abstrakt deklariert. Die andere Methode `NumberOfSections(…)`, gebunden ist `numberOfSectionsInTableview:`. Diese Methode ist in das Protokoll, optional, sodass Xamarin.iOS als virtuell somit optional zum Überschreiben Sie deklariert in C#.

Xamarin.iOS ist für alle iOS-Bindung für Sie übernimmt. Jedoch wenn Sie ein Protokoll manuell von Objective-C binden müssen, Sie können dazu werden, indem eine Klasse mit dem `ExportAttribute`. Dies ist die gleiche Methode, die von Xamarin.iOS selbst verwendet.

Weitere Informationen dazu, wie in Xamarin.iOS Objective-C-Typen zu binden, finden Sie im Artikel [Binden von Objective-C-Typen](~/ios/platform/binding-objective-c/index.md).

Wir sind noch nicht so über mit Protokollen, aber. Sie werden auch verwendet unter iOS als Grundlage für Objective-C-Delegaten, die das Thema des nächsten Abschnitts.

## <a name="delegates"></a>Delegaten

iOS verwendet Objective-C-Delegaten, um das Muster für Delegierung zu implementieren, in dem ein einzelnes Objekt Arbeit auf eine andere übertragen aus. Das Objekt, das die Arbeit ist der Delegat, der das erste Objekt. Ein Objekt weist der Delegat, der durch das Senden sie Nachrichten nach dem Auftreten von bestimmte Dinge funktionieren. Senden eine Meldung wie diese in Objective-C ist funktionell gleichwertig mit Aufrufen einer Methode C#. Ein Delegat Methoden als Reaktion auf diese Aufrufe implementiert und daher bietet Funktionen für die Anwendung.

Delegaten bieten die Möglichkeit, um das Verhalten der Klassen zu erweitern, ohne Unterklassen erstellen zu müssen. Anwendungen in iOS verwenden häufig Delegaten auf, wenn eine Klasse zurück in einen anderen aufruft, nachdem ein wichtiger Vorgang auftritt. Z. B. die `MKMapView` Klasse Sie Aufrufe zurück an die Delegaten aus, wenn der Benutzer tippt eine Anmerkung zu einer Zuordnung auf, mit dem dem Autor der "Delegate"-Klasse die Möglichkeit, innerhalb der Anwendung zu reagieren. Sie können einen Delegaten mit Xamarin.iOS über ein Beispiel für diese Art der Verwendung von Delegaten weiter unten in diesem Artikel unter Beispiel arbeiten.

An diesem Punkt möchten Sie wissen, wie eine Klasse bestimmt, welche Methoden für die Delegaten aufrufen. Dies ist eine andere Stelle, wo Sie Protokolle verwenden. Die verfügbaren Methoden für einen Delegaten stammen in der Regel die Protokolle, mit denen, die Sie übernehmen.

### <a name="how-protocols-are-used-with-delegates"></a>Wie Protokolle mit Delegaten verwendet werden.

Wir haben gesehen, wie zuvor Protokolle verwendet werden soll, Hinzufügen von Anmerkungen zu einer Zuordnung zu unterstützen.
Protokolle werden auch verwendet, um einen bekannten Satz von Methoden für Klassen aufgerufen, wenn bestimmte Ereignisse auftreten, z. B. nach dem der Benutzer tippen einer Anmerkung auf einer Karte oder wählt eine Zelle in einer Tabelle bereitzustellen. Die Klassen, die diese Methoden implementieren werden als Delegaten der Klassen bezeichnet, die sie aufrufen.

Klassen, die Unterstützung der Delegierung Zweck Verfügbarmachen einer Delegat-Eigenschaft, die eine Klasse implementiert die Delegaten zugewiesen wird. Die Methoden, die Sie für den Delegaten implementieren, werden das Protokoll abhängig, die den Delegaten übernimmt. Für die `UITableView` -Methode, die Sie implementieren die `UITableViewDelegate` -Protokoll für die `UIAccelerometer` -Methode implementieren Sie `UIAccelerometerDelegate`usw. für alle anderen Klassen in iOS, die für die Sie möchten einen Delegaten verfügbar gemacht.

Die `MKMapView` Klasse, die wir in unserem vorherigen Beispiel gesehen haben, verfügt auch über eine Eigenschaft namens-Delegaten, der es aufgerufen wird, wenn verschiedene Ereignisse auftreten. Der Delegat für `MKMapView` ist vom Typ `MKMapViewDelegate`.
Verwenden Sie es in Kürze ein Beispiel für die Anmerkung zu reagieren, wenn diese Option ausgewählt ist, aber zunächst sehen wir besprechen Sie den Unterschied zwischen starke und schwache Delegaten.

### <a name="strong-delegates-vs-weak-delegates"></a>Starke Delegaten im Vergleich zu Schwache Delegaten

Die Delegaten, die, denen wir uns bisher angesehen haben haben, sind starke Delegaten, was bedeutet, dass sie stark typisiert sind. Die Xamarin.iOS-Bindungen sind im Lieferumfang einer stark typisierten Klasse für jedes Protokoll Delegaten in iOS. IOS hat jedoch auch das Konzept eines schwachen Delegaten. Anstelle von Unterklassen aus einer Klasse, die an die Objective-C-Protokoll für Delegaten gebunden, iOS können Sie auch festlegen, dass die Protokollmethoden selbst in einer Klasse, die Sie z. B. binden, die von NSObject, versehen Sie Ihre Methoden mit dem ExportAttribute abgeleitet wird und dann liefern die entsprechenden Selektoren ein.
Wenn Sie diesen Ansatz verwenden, weisen Sie eine Instanz der Klasse die WeakDelegate-Eigenschaft anstelle der Delegat-Eigenschaft. Ein schwacher Delegat bietet Ihnen die Flexibilität, Ihre Delegatklasse Sie einer anderen Vererbungshierarchie zu nutzen. Wir sehen uns ein Xamarin.iOS-Beispiel, starke und schwache Delegaten verwendet.

### <a name="example-using-a-delegate-with-xamarinios"></a>Beispiel für die Verwendung eines Delegaten mit Xamarin.iOS

Um Code in der Antwort an den Benutzer Tippen auf die Anmerkung in unserem Beispiel ausführen zu können, können wir eine Unterklasse `MKMapViewDelegate` und weisen Sie eine Instanz der `MKMapView`des `Delegate` Eigenschaft. Die `MKMapViewDelegate` Protokoll enthält nur die optionale Methoden.
Aus diesem Grund sind alle Methoden virtuell, die an das Protokoll in die Xamarin.iOS gebunden sind `MKMapViewDelegate` Klasse. Bei der Auswahl einer Anmerkung, die `MKMapView` Instanz sendet die `mapView:didSelectAnnotationView:` Nachricht an die Delegaten. Um dies in Xamarin.iOS zu behandeln, müssen wir außer Kraft setzen der `DidSelectAnnotationView (MKMapView mapView, MKAnnotationView annotationView)` -Methode in der die Unterklasse MKMapViewDelegate wie folgt:

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

Die oben gezeigte SampleMapDelegate-Klasse wird als im Controller, der enthält eine geschachtelte Klasse implementiert die `MKMapView` Instanz. In Objective-C sehen Sie häufig den Controller, der mehrere Protokolle direkt innerhalb der Klasse zu übernehmen. Da Protokolle an Klassen in Xamarin.iOS gebunden sind, sind die Klassen, die stark typisierte Delegaten implementieren jedoch in der Regel als geschachtelte Klassen enthalten.

Bei der Implementierung der Delegat-Klasse vorhanden, müssen Sie nur, instanziiere eine Instanz des Delegaten im Controller, und weisen sie Sie der `MKMapView`des `Delegate` Eigenschaft wie hier gezeigt:

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

Um einen schwachen Delegaten verwenden, die den gleichen Zweck erfüllen, müssen Sie die Methode selbst in einer beliebigen Klasse binden, die abgeleitet `NSObject` und weisen sie Sie der `WeakDelegate` Eigenschaft der `MKMapView`. Da die `UIViewController` abgeleitet aus `NSObject` (z. B. jede Objective-C-Klasse in CocoaTouch), können wir einfach eine Methode, um gebunden implementieren `mapView:didSelectAnnotationView:` direkt in den Controller, und weisen Sie den Controller zum `MKMapView`des `WeakDelegate`, vermeiden die Notwendigkeit, die zusätzliche geschachtelte Klasse. Der folgende Code veranschaulicht diesen Ansatz:

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

Wenn dieser Code ausgeführt wird, verhält sich die Anwendung, genauso wie bei der Ausführung der stark typisierten Delegaten-Version. Der Vorteil aus diesem Code ist, dass der schwachen Delegat die Erstellung der zusätzlichen Klasse erfordern nicht, die erstellt wurde, als wir den stark typisierten Delegaten verwendet. Dies geht jedoch auf Kosten der typsicherheit. Würden Sie einen Fehler in der Auswahl, die übergeben wurde, machen die `ExportAttribute`, Sie wäre nicht bis zur Laufzeit ermitteln.

### <a name="events-and-delegates"></a>Ereignisse und Delegaten

Delegaten werden für Rückrufe in iOS auf ähnliche Weise wie verwendet, die.NET Ereignisse verwendet. Zum Stellen von iOS scheint-APIs und die Möglichkeit, die Verwendung von Objective-C-Delegaten mehr wie .NET, Xamarin.iOS macht .NET Ereignisse an vielen Stellen, in denen Delegaten in iOS verwendet werden.

Z. B. die frühere Implementierung, in dem die `MKMapViewDelegate` beantwortet eine ausgewählte Anmerkung kann auch in Xamarin.iOS mithilfe eines Ereignisses für die .NET implementiert werden. In diesem Fall würde das Ereignis festgelegt werden, in `MKMapView` und `DidSelectAnnotationView`. Es müsste eine `EventArgs` Unterklasse des Typs `MKMapViewAnnotationEventsArgs`. Die `View` Eigenschaft `MKMapViewAnnotationEventsArgs` würde erhalten Sie einen Verweis auf die Anmerkung-Sicht, aus dem Sie mit der gleichen Implementierung fortfahren kann mussten Sie zuvor wie dargestellt hier:

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

In diesem Artikel erläutert, wie Sie Ereignisse, Protokolle und Delegaten in Xamarin.iOS verwendet wird. Wir haben gesehen, wie normale .NET Style-Ereignisse für Steuerelemente in Xamarin.iOS verfügbar macht.
Als Nächstes wir haben die Objective-C-Protokolle, einschließlich, wie sie sich von unterscheiden C# Schnittstellen und wie Sie Xamarin.iOS verwendet. Schließlich haben wir untersucht, Objective-C-Delegaten vom Standpunkt der Xamarin.iOS. Wir haben gesehen, wie Xamarin.iOS beide stark unterstützt und schwach typisierte Delegate, und binden Sie Ereignisse, Methoden zu delegieren.

## <a name="related-links"></a>Verwandte Links

- [Protokolle, Delegaten und Ereignissen (Beispiel)](https://developer.xamarin.com/samples/Protocols_Delegates_Events/)
- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [Binden von Objective-C-Typen](~/ios/platform/binding-objective-c/index.md)
- [Die Objective-C-Programmiersprache](https://developer.apple.com/library/ios/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
- [Entwerfen von Benutzeroberflächen in Xcode 4](https://developer.apple.com/library/ios/#documentation/IDEs/Conceptual/Xcode4TransitionGuide/InterfaceBuilder/InterfaceBuilder.html)
- [Anwendung Kernkompetenzen für iOS](https://developer.apple.com/library/ios/#DOCUMENTATION/General/Conceptual/Devpedia-CocoaApp/TargetAction.html)
