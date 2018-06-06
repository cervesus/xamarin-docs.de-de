---
title: Ereignisse, Protokolle und Delegaten in Xamarin.iOS
description: Dieses Dokument beschreibt das Arbeiten mit Ereignissen, Protokolle, und in Xamarin.iOS delegiert. Diese grundlegenden Konzepte sind weit verbreitete Xamarin.iOS Entwicklung.
ms.prod: xamarin
ms.assetid: 7C07F0B7-9000-C540-0FC3-631C29610447
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: d0e4c23bffe689c9218da2f43b97d98f348513ad
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784009"
---
# <a name="events-protocols-and-delegates-in-xamarinios"></a>Ereignisse, Protokolle und Delegaten in Xamarin.iOS

Xamarin.iOS verwendet Steuerelemente, um Ereignisse für die meisten Benutzerinteraktionen verfügbar zu machen.
Xamarin.iOS Anwendungen nutzen diese Ereignisse im großen und ganzen genauso, wie herkömmliche .NET ausgeführten Aktionen. Beispielsweise die Xamarin.iOS UIButton-Klasse verfügt über ein Ereignis namens TouchUpInside und dieses Ereignis verarbeitet, als ob diese Klasse und das Ereignis in einer .NET-App wurden.

Neben diesen Ansatz .NET macht Xamarin.iOS ein anderes Modell, das für eine komplexere Interaktion und die Datenbindung verwendet werden kann. Bei dieser Methode wird verwendet, Apple Delegaten und Protokolle aufruft. Delegaten ähneln dem Konzept Delegaten in C# geschrieben, aber statt definieren und eine einzelne Methode aufrufen, wird ein Delegat in Objective-C eine gesamte Objektklasse, die ein Protokoll entspricht. Ein Protokoll ist eine Schnittstelle in c# ist ähnlich, mit dem Unterschied, dass seine Methoden optional sein können. Um eine UITableView mit Daten aufzufüllen, würden Sie z. B. eine Delegatklasse erstellen, die in das UITableViewDataSource-Protokoll, das Aufrufen der UITableView selbst auffüllen definierten Methoden implementiert.

In diesem Artikel Sie über alle diese Themen erfahren, und Sie haben eine solide Grundlage für die Behandlung der Rückrufszenarien in Xamarin.iOS, einschließlich:

-  **Ereignisse** – .NET mithilfe von Ereignissen mit UIKit Steuerelemente.
-  **Protokolle** – was Learning Protokolle sind und wie sie verwendet werden, und erstellen ein Beispiel, stellt Daten für eine Karte-Anmerkung.
-  **Delegaten** – lernen über Objective-C-Delegaten durch Erweitern des Beispiels Karte, um die Benutzerinteraktion zu verarbeiten, die eine Anmerkung enthält, und lernen den Unterschied zwischen starke und schwache Delegaten und wann diese verwendet.

Um Protokolle und Delegaten zu veranschaulichen, erstellen wir eine einfache Zuordnung-Anwendung, die von einer Anmerkung zu einer Zuordnung hinzugefügt wird, wie hier gezeigt:

 [![](delegates-protocols-and-events-images/01-map.png "Ein Beispiel für eine einfache Zuordnung-Anwendung, die eine Anmerkung zu einer Zuordnung fügt") ](delegates-protocols-and-events-images/01-map.png#lightbox) [ ![ ] (delegates-protocols-and-events-images/04-annotation-with-callout.png "eine Beispiel-Anmerkung zu einer Zuordnung hinzugefügt")](delegates-protocols-and-events-images/04-annotation-with-callout.png#lightbox)

Vor dem bewältigt diese app Fortschritts, beginnen wir zunächst mit .NET Ereignisse unter der UIKit ansehen.

<a name=".NET_Events_with_UIKit" />

## <a name="net-events-with-uikit"></a>.NET Ereignisse mit UIKit

Xamarin.iOS .NET Ereignisse auf UIKit Steuerelemente verfügbar gemacht werden. Beispielsweise weist UIButton ein TouchUpInside-Ereignis, das Sie behandeln wie gewohnt in .NET verwenden, wie im folgenden Code dargestellt, die einen C#-Lambda-Ausdruck verwendet:

```csharp
aButton.TouchUpInside += (o,s) => {
    Console.WriteLine("button touched");
};
```
Sie können dies auch mit einer C#-2.0-Format anonyme Methode wie die folgende implementieren:

```csharp
aButton.TouchUpInside += delegate {
    Console.WriteLine ("button touched");
};
```

Der vorhergehende Code ist in der Methode ViewDidLoad der UIViewContoller oben wired. Die Variable aButton verweist auf eine Schaltfläche, die Sie in der iOS-Designer oder durch Code hinzufügen können. Die folgende Abbildung zeigt diese Schaltfläche aus, wie es im iOS-Designer aus dem Beispiel, das in diesem Artikel begleitet geschaltet hinzugefügt wird:

 [![](delegates-protocols-and-events-images/02-interface-builder-outlet.png "Eine Schaltfläche in iOS-Designer hinzugefügt")](delegates-protocols-and-events-images/02-interface-builder-outlet.png#lightbox)

Xamarin.iOS unterstützt auch das Ziel-Aktionsformat Herstellen einer Verbindung zwischen Ihren Code auf eine Aktivität, die mit einem Steuerelement auftritt. Um ein Ziel-Aktion für die Schaltfläche "Hello" erstellt haben, doppelklicken klicken sie in der iOS-Designer. Die UIViewController Code-Behind-Datei wird angezeigt, und der Entwickler wird aufgefordert, wählen Sie einen Speicherort für das Herstellen einer Verbindung Methode einfügen:

 [![](delegates-protocols-and-events-images/03-interface-builder-action.png "Die UIViewControllers Code-Behind-Datei")](delegates-protocols-and-events-images/03-interface-builder-action.png#lightbox)

Nachdem Sie einen Standort ausgewählt ist, eine neue Methode erstellt und wired von an das Steuerelement. Im folgenden Beispiel wird eine Nachricht in die Konsole geschrieben werden, wenn auf die Schaltfläche geklickt wird:

 [![](delegates-protocols-and-events-images/05-interface-builder-action.png "Eine Nachricht wird an die Konsole geschrieben werden, wenn die Schaltfläche geklickt wird")](delegates-protocols-and-events-images/05-interface-builder-action.png#lightbox)

Weitere Informationen über das iOS-Ziel-Action-Muster finden Sie im Abschnitt "Ziel-Aktion" von " [Anwendung Kernkompetenzen für iOS](http://developer.apple.com/library/ios/#DOCUMENTATION/General/Conceptual/Devpedia-CocoaApp/TargetAction.html)" in Apple iOS-Entwicklerbibliothek.

Weitere Informationen zur Verwendung von iOS-Designer mit Xamarin.iOS finden Sie unter der " [iOS Designer Overview](~/ios/user-interface/designer/index.md)" Dokumentation.

 <a name="Events" />


## <a name="events"></a>Ereignisse

Wenn Sie Ereignisse aus UIControl abfangen möchten, müssen Sie eine Reihe von Optionen: von C#-Lambda-Ausdrücke und Delegatfunktionen, um die Low-Level Objective-C-APIs verwenden.

Der folgende Abschnitt zeigt, wie Sie das Ereignis TouchDown auf eine Schaltfläche, je nachdem wie viel Kontrolle erfassen würde, die Sie benötigen.

 <a name="C#_Style" />


## <a name="c-style"></a>C#-Stil

Verwenden die Delegaten-Syntax:

```csharp
UIButton button = MakeTheButton ();
button.TouchDown += delegate {    
    Console.WriteLine ("Touched");
};
```

Wenn Sie stattdessen Lambdas zufrieden sind:

```csharp
button.TouchDown += () => {
   Console.WriteLine ("Touched");
};
```

Wenn Sie möchten mit der mehrere Schaltflächen den gleichen Handler denselben Code verwenden:

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

<a name="Monitoring_more_than_one_kind_of_Event" />


## <a name="monitoring-more-than-one-kind-of-event"></a>Mehr als eine Art von Ereignis überwachen

Die C#-Ereignisse für UIControlEvent Flags haben eine 1: 1-Zuordnung zu einzelnen Flags. Wenn Sie den gleichen Codeabschnitt haben möchten zwei oder mehr Ereignisse behandeln, verwenden Sie die `UIControl.AddTarget` Methode:

```csharp
button.AddTarget (handler, UIControlEvent.TouchDown | UIControlEvent.TouchCancel);
```

Mithilfe der Lambda-Syntax:

```csharp
button.AddTarget ((sender, event)=> Console.WriteLine ("An event happened"), UIControlEvent.TouchDown | UIControlEvent.TouchCancel);
```

Wenn in diesem Fall müssen Sie eine Low-Level Objective-C, wie z. B. auf eine bestimmte Objektinstanz einbinden und Aufrufen einer bestimmten Auswahl-Funktionen verwenden:

```csharp
[Export ("MySelector")]
void MyObjectiveCHandler ()
{
    Console.WriteLine ("Hello!");
}

// In some other place:

button.AddTarget (this, new Selector ("MySelector"), UIControlEvent.TouchDown);
```

Bitte beachten Sie, wenn Sie die Instanzmethode in einer geerbten Klasse implementieren, muss er eine öffentliche Methode.

 <a name="Protocols" />


## <a name="protocols"></a>Protokolle

Ein Protokoll ist eine Objective-C-Sprachfunktion, die eine Liste der Methodendeklarationen bereitstellt. Es dient einen ähnlichen Zweck einer Schnittstelle in c# ist der Hauptunterschied besteht darin, dass ein Protokoll die optionale Methoden aufweisen kann. Optionale Methoden werden nicht aufgerufen, wenn die Klasse, die ein Protokoll nimmt nicht implementiert wird. Darüber hinaus kann eine einzelne Klasse in Objective-C mehrere Protokolle implementieren, wie eine C#-Klasse mehrere Schnittstellen implementieren kann.

Apple verwendet Protokolle in der gesamten iOS, um Klassen zu übernehmen, während unterwegs die implementierende Klasse vom Aufrufer, also genau wie ein C#-Schnittstelle funktioniert so abstrahiert, Verträge definieren. Protokolle verwendet werden beide Typen in nicht-Delegat Szenarien (z. B. mit der `MKAnnotation` Beispiel weiter oben), und mit Delegaten (wie weiter unten in diesem Dokument im Abschnitt Delegaten dargestellt).

 <a name="Protocols_with_Monotouch" />


### <a name="protocols-with-xamarinios"></a>Protokolle mit Xamarin.ios

Werfen wir einen Blick auf ein Beispiel, das mithilfe von Xamarin.iOS eines Objective-C-Protokolls. In diesem Beispiel verwenden wir die `MKAnnotation` Protokoll, die Teil von der `MapKit` Framework. `MKAnnotation` ist ein Protokoll, jedes Objekt ermöglicht, die einführt, um Informationen zu einer Anmerkung bereitzustellen, die zu einer Zuordnung hinzugefügt werden können. Angenommen, ein Objekt, durch `MKAnnotation` gibt die Position der Anmerkung und der Titel zugeordnet.

Auf diese Weise die `MKAnnotation` Protokoll wird verwendet, um die relevante Daten bereit, die eine Anmerkung begleitet. Die aktuelle Ansicht für die Anmerkung selbst basiert aus den Daten in das Objekt, das nimmt die `MKAnnotation` Protokoll. Z. B. der Text für die Legende an, die angezeigt wird, wenn der Benutzer auf die Anmerkung tippt, (wie im folgenden Screenshot gezeigt) stammen aus den `Title` Eigenschaften der Klasse, die das Protokoll implementiert:

 [![](delegates-protocols-and-events-images/04-annotation-with-callout.png "Beispieltext für die Legende beim Tippen auf die Anmerkung")](delegates-protocols-and-events-images/04-annotation-with-callout.png#lightbox)

Wie im nächsten Abschnitt werden die Protokolle Deep Dive, beschrieben bindet Xamarin.iOS Protokolle für abstrakte Klassen. Für die `MKAnnotation` -Protokoll die gebundene C#-Klasse heißt `MKAnnotation` zu imitieren, die den Namen des Protokolls verfügbar, und es ist eine Unterklasse von `NSObject`, die Stammbasisklasse für CocoaTouch. Das Protokoll erfordert einen Getter und Setter für die Koordinate implementiert werden. Titel und Untertitel sind jedoch optional. Aus diesem Grund in die `MKAnnotation` -Klasse, die `Coordinate` Eigenschaft ist *abstrakte*, implementiert werden müssen und die `Title` und `Subtitle` Eigenschaften gekennzeichnet sind *virtuellen* , wodurch optional, wie unten dargestellt:

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

Jede Klasse kann Anmerkungsdaten bereitstellen, indem Sie einfach Ableiten von `MKAnnotation`, solange mindestens die `Coordinate` Eigenschaft implementiert wird. Hier ist z. B. eine Beispielklasse, die die Koordinate im Konstruktor akzeptiert und gibt eine Zeichenfolge für den Titel:

```csharp
/// <summary>
/// Annotation class that subclasses MKAnnotation abstract class
/// MKAnnotation is bound by Xamarin.iOS to the MKAnnotation protocol
/// </summary>
public class SampleMapAnnotation : MKAnnotation
{
    string _title;

    public SampleMapAnnotation (CLLocationCoordinate2D coordinate)
    {
        Coordinate = coordinate;
        _title = "Sample";
    }

    public override CLLocationCoordinate2D Coordinate { get; set; }

    public override string Title {
        get {
            return _title;
        }
    }
}
```

Über das Protokoll ist an eine beliebige dieser Unterklassen Klasse `MKAnnotation` können bereitstellen relevante Daten, die von der Zuordnung verwendet werden, wenn sie die Anmerkung Sicht erstellt. Zum Hinzufügen einer Anmerkung zu einer Zuordnung rufen Sie einfach die `AddAnnotation` Methode von einer `MKMapView` Instanz, wie im folgenden Code gezeigt:

```csharp
//an arbitrary coordinate used for demonstration here
var sampleCoordinate =
new CLLocationCoordinate2D (42.3467512, -71.0969456);

//create an annotation and add it to the map
map.AddAnnotation (new SampleMapAnnotation (sampleCoordinate));
```

Die Zuordnung Variable hier ist eine Instanz der ein `MKMapView`, also der Klasse, die die Zuordnung selbst darstellt. Die `MKMapView` verwendet die `Coordinate` Daten stammen aus den `SampleMapAnnotation` Instanz um die Anmerkung Ansicht auf der Karte zu positionieren.

Die `MKAnnotation` -Protokoll bietet einen bekannten Satz von Funktionen für alle Objekte, die ihn implementieren, ohne dass des Consumers (in diesem Fall die Map) müssen Implementierungsdetails kennen. Dies optimiert die eine Vielzahl von möglichen Anmerkungen zu einer Zuordnung hinzufügen.

 <a name="Protocols_Deep_Dive" />


### <a name="protocols-deep-dive"></a>Deep Dive Protokolle

Da C#-Schnittstellen optionale Methoden nicht unterstützen, ordnet Xamarin.iOS Protokolle abstrakte Klassen. Aus diesem Grund ein Protokoll in Objective-C Übernahme erfolgt in Xamarin.iOS durch Ableiten von der abstrakten Klasse, die an das Protokoll gebunden ist und die erforderlichen Methoden implementiert. Diese Methoden werden als abstrakte Methoden in der Klasse verfügbar gemacht werden. Optionale Methoden aus dem Protokoll werden von virtuellen Methoden der C#-Klasse gebunden werden.

Hier ist z. B. ein Teil der `UITableViewDataSource` Protokoll als gebundene in Xamarin.iOS:

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

Beachten Sie, dass die Klasse abstrakt ist. Xamarin.iOS stellt die Klasse abstrakte Methoden optional/erforderlich in Protokolle unterstützen.
Im Gegensatz zu Objective-C-Protokolle (oder C#-Schnittstellen) unterstützen nicht jedoch mehrfache Vererbung C#-Klassen. Dies wirkt sich auf den Entwurf der C#-Code, der Protokolle verwendet und in der Regel führt zu einer geschachtelten Klassen. Weitere Informationen zu diesem Problem weiter unten in diesem Dokument im Abschnitt Delegaten fällt.

 `GetCell(…)` ist eine abstrakte Methode, die an die Objective-C gebunden *Selektor*, `tableView:cellForRowAtIndexPath:`, dies ist eine erforderliche Methode von der `UITableViewDataSource` Protokoll. Die Auswahl ist der Objective-C-Begriff für Methodenname. Um die Methode nach Bedarf zu erzwingen, Xamarin.iOS das es als abstrakt deklariert. Die andere Methode `NumberOfSections(…)`, gebunden ist `numberOfSectionsInTableview:`. Diese Methode ist optional in das Protokoll, sodass Xamarin.iOS als virtuell deklariert somit optional in c# überschreiben.

Xamarin.iOS übernimmt alle iOS-Bindung für Sie. Jedoch, wenn Sie ein Protokoll manuell aus Objective-C binden müssen, Sie können dazu werden, indem eine Klasse mit dem `ExportAttribute`. Dies ist die gleiche Methode, die von Xamarin.iOS selbst verwendet.

Weitere Informationen über das Binden von Objective-C-Typen in Xamarin.iOS finden Sie im Artikel [Objective-C-Typen binden](~/ios/platform/binding-objective-c/index.md).

Wir noch nicht über Protokolle, obwohl. Sie können auch verwendet in iOS als Grundlage für Objective-C-Delegaten, der das Thema des nächsten Abschnitts ist.

 <a name="Delegates" />


## <a name="delegates"></a>Delegaten

iOS verwendet Objective-C-Delegaten, um die Delegierung-Muster zu implementieren, in dem ein Objekt Arbeit zu einem anderen deaktiviert übertragen. Das Objekt, das den Aufgaben wird der Delegat, der das erste Objekt. Ein Objekt weist der Delegat, der durch das Senden Nachrichten nach bestimmten Dinge passieren funktionieren. Senden einer Nachricht wie folgt in Objective-C ist funktional äquivalent zum Aufrufen einer Methode in C# geschrieben. Ein Delegat Methoden als Antwort auf diese Aufrufe implementiert und daher stellt Funktionalität für die Anwendung bereit.

Delegaten können Sie das Verhalten von Klassen erweitern, ohne Unterklassen erstellen zu müssen. Anwendungen in iOS verwenden häufig Delegaten auf, wenn eine Klasse zurück in einen anderen aufruft, nachdem ein wichtiger Vorgang auftritt. Z. B. die `MKMapView` Aufrufe zurück an seine Delegaten Klasse, bei der der Benutzer eine Anmerkung auf einer Karte tippt, mit dem dem Autor die Delegate-Klasse die Möglichkeit, innerhalb der Anwendung reagieren. Sie können über ein Beispiel für diese Art der Verwendung von Delegaten weiter unten in diesem Artikel im Beispiel für die Verwendung ein Delegaten mit Xamarin.iOS arbeiten.

An diesem Punkt Sie sich vielleicht, wie eine Klasse bestimmt, welche Methoden aufrufen, die der Delegat. Dies ist eine andere Stelle, wo Sie Protokolle verwenden. Die verfügbaren Methoden für einen Delegaten stammen in der Regel die Protokolle, die sie übernehmen.

 <a name="How_Protocols_are_used_with_Delegates" />


### <a name="how-protocols-are-used-with-delegates"></a>Wie Protokolle mit Delegaten verwendet werden.

Wir gesehen haben, zuvor wie Protokolle verwendet werden, zum Hinzufügen von Anmerkungen zu einer Zuordnung zu unterstützen.
Protokolle werden auch verwendet, um einem bekannten Satz von Methoden für Klassen, die aufgerufen wird, nachdem bestimmte Ereignisse auftreten, z. B. nach dem Benutzer tippen, eine Anmerkung auf einer Karte oder wählt eine Zelle in einer Tabelle bereitzustellen. Die Klassen, die diese Methoden implementieren bekannten als den Delegaten, der die Klassen, die sie aufrufen.

Klassen, die Unterstützung der Delegierung dafür Verfügbarmachen einer Delegaten-Eigenschaft, die eine Klasse implementiert die Delegaten zugewiesen wird. Die Methoden, die Sie für den Delegaten implementieren hängen von dem Protokoll, die der bestimmten Delegat nimmt. Für die `UITableView` -Methode, die Sie implementieren die `UITableViewDelegate` -Protokoll für die `UIAccelerometer` -Methode, implementieren Sie `UIAccelerometerDelegate`usw. für alle anderen Klassen in der gesamten iOS, die für die Sie möchten einen Delegaten verfügbar gemacht.

Die `MKMapView` im früheren Beispiel veranschaulichte Filtermenü-Klasse verfügt auch über eine Eigenschaft mit dem Namen der Delegat, der es aufgerufen wird, wenn verschiedene Ereignisse auftreten. Der Delegat für `MKMapView` ist vom Typ `MKMapViewDelegate`.
Sie verwenden diese Informationen in Kürze in ein Beispiel für die Anmerkung reagiert werden soll, nachdem diese Option ausgewählt ist, doch zunächst sehen wir erläutert den Unterschied zwischen starke und schwache Delegaten.

 <a name="Strong_Delegates_vs._Weak_Delegates" />


### <a name="strong-delegates-vs-weak-delegates"></a>Starke Delegaten im Vergleich zu Unsichere Delegaten

Die Delegaten auf, denen wir bisher besuchten sind starken Delegaten, was bedeutet, dass sie stark typisiert sind. Im Lieferumfang von Xamarin.iOS Bindungen ist einer stark typisierten Klasse für jedes Protokoll Delegaten in iOS. IOS hat jedoch auch das Konzept eines schwachen Delegaten. Anstatt das Erstellen von Unterklassen für eine Klasse, die auf das Objective-C-Protokoll für einen bestimmten Delegaten gebunden, iOS können Sie auch die Protokollmethoden selbst in einer Klasse, die Ihnen gefällt zu binden, die von NSObject, ergänzen die Methoden mit dem ExportAttribute abgeleitet wird und dann Angeben der entsprechenden Selektoren an.
Wenn Sie diesen Ansatz verwenden, weisen Sie eine Instanz der Klasse der WeakDelegate-Eigenschaft anstelle der Delegat-Eigenschaft. Ein schwacher Delegaten bietet Ihnen die Flexibilität, um Ihre Delegate-Klasse in eine andere Hierarchie nach unten zu nutzen. Sehen wir uns auf starke und schwache Delegaten wird ein Xamarin.iOS Beispiel.

 <a name="Example_using_a_Delegate_with_Xamarin.iOS" />


### <a name="example-using-a-delegate-with-xamarinios"></a>Beispiel für die Verwendung eines Delegaten mit Xamarin.iOS

Zum Ausführen von Code in der Antwort an den Benutzer Tippen auf die Anmerkung in unserem Beispiel können wir die Unterklasse `MKMapViewDelegate` und weisen Sie eine Instanz der `MKMapView`des `Delegate` Eigenschaft. Die `MKMapViewDelegate` Protokoll enthält nur die optionale Methoden.
Aus diesem Grund alle Methoden sind virtuell verbunden sind, das Protokoll in der Xamarin.iOS `MKMapViewDelegate` Klasse. Wenn der Benutzer wählt eine Anmerkung aus der `MKMapView` Instanz sendet die `mapView:didSelectAnnotationView:` Nachricht seines Delegaten. Um dies in Xamarin.iOS behandeln, müssen wir außer Kraft setzen die `DidSelectAnnotationView (MKMapView mapView, MKAnnotationView annotationView)` Methode in der Unterklasse MKMapViewDelegate wie folgt:

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

Die oben gezeigte SampleMapDelegate-Klasse wird als im Controller, die enthält eine geschachtelte Klasse implementiert die `MKMapView` Instanz. In Objective-C sehen Sie häufig den Controller mehrere Protokolle direkt innerhalb der Klasse zu verwenden. Da Klassen im Xamarin.iOS Protokolle gebunden sind, sind die Klassen, die stark typisierte Delegaten zu implementieren als geschachtelte Klassen in der Regel enthalten.

Mit der Implementierung der Delegat Klasse vorhanden, müssen nur instanziieren Sie eine Instanz des Delegaten im Controller, und weisen Sie ihn der `MKMapView`des `Delegate` Eigenschaft wie hier gezeigt:

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

Um einen schwachen Delegaten verwenden, um dasselbe Ziel erreichen, müssen Sie die Methode selbst in einer Klasse gebunden, die abgeleitet `NSObject` und weisen Sie ihn der `WeakDelegate` Eigenschaft von der `MKMapView`. Da die `UIViewController` abgeleitet aus `NSObject` (z. B. jede Klasse Objective-C in CocoaTouch) kann einfach implementieren wir eine Methode gebunden `mapView:didSelectAnnotationView:` direkt im Controller, und weisen Sie den Controller zum `MKMapView`des `WeakDelegate`, sodass für die zusätzliche geschachtelte Klasse. Der Code unten zeigt dies:

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

Wenn dieser Code ausgeführt wird, verhält sich die Anwendung genau wie beim Ausführen der stark typisierten Delegaten-Version. Der Vorteil dieses Codes ist, dass der schwachen Delegat die Erstellung der zusätzliche Klasse erfordert, die erstellt wurde, wenn wir den stark typisierten Delegaten verwendet. Dies ist jedoch zu Lasten der typsicherheit. Würden Sie einen Fehler im Selektor zu machen, die übergeben wurde, der `ExportAttribute`, Sie wäre nicht bis zur Laufzeit herausfinden.

 <a name="Events_and_Delegates" />


### <a name="events-and-delegates"></a>Ereignisse und Delegaten

Delegaten werden für Rückrufe in iOS auf ähnliche Weise auf die Möglichkeit, verwendet, die.NET Ereignisse verwendet. Zum Erstellen von iOS scheint-APIs und wie sie Objective-C-Delegaten mehr wie .NET, Xamarin.iOS .NET Ereignisse an vielen Stellen, wo Delegaten in iOS verwendet werden, verfügbar gemacht werden.

Angenommen, die frühere Implementierung, in dem die `MKMapViewDelegate` reagierte auf eine ausgewählte Anmerkung kann auch in Xamarin.iOS implementiert werden, mithilfe eines Ereignisses .NET. In diesem Fall würde das Ereignis definiert werden, `MKMapView` und dem Namen `DidSelectAnnotationView`. Es müsste eine `EventArgs` Unterklasse des Typs `MKMapViewAnnotationEventsArgs`. Die `View` Eigenschaft `MKMapViewAnnotationEventsArgs` erhalten Sie einen Verweis auf die Anmerkung-Sicht, aus dem Sie mit der gleichen Implementierung fortfahren konnte Sie hatten zuvor ausgehendem hier:

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

 <a name="Summary" />


## <a name="summary"></a>Zusammenfassung

Dieser Artikel behandelt zum Verwenden von Delegaten, Ereignisse und Protokolle im Xamarin.iOS. Wir haben gesehen, wie normale .NET Stil Ereignisse für Steuerelemente von Xamarin.iOS verfügbar macht.
Als Nächstes kennen gelernt Objective-C-Protokolle, z. B., wie sie sich von C#-Schnittstellen sind und wie Sie Xamarin.iOS verwendet. Schließlich untersuchen wir Objective-C-Delegaten im Hinblick auf Xamarin.iOS. Wir haben gesehen, wie Xamarin.iOS stark unterstützt und schwach typisierte Delegaten und Ereignisse Methoden delegieren zu binden.


## <a name="related-links"></a>Verwandte Links

- [Protokolle, Delegaten und Ereignisse (Beispiel)](https://developer.xamarin.com/samples/Protocols_Delegates_Events/)
- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [Binden von Objective-C-Typen](~/ios/platform/binding-objective-c/index.md)
- [Objective-C-Programmiersprache](http://developer.apple.com/library/ios/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
- [Entwerfen von Benutzeroberflächen in Xcode 4](http://developer.apple.com/library/ios/#documentation/IDEs/Conceptual/Xcode4TransitionGuide/InterfaceBuilder/InterfaceBuilder.html)
- [Anwendung Kernkompetenzen für iOS](http://developer.apple.com/library/ios/#DOCUMENTATION/General/Conceptual/Devpedia-CocoaApp/TargetAction.html)
