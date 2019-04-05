---
title: Einführung in Storyboards in Xamarin.iOS
description: Dieses Dokument enthält eine Einführung in Storyboards in Xamarin.iOS. Es wird beschrieben, wie ein Storyboard verwendet wird, um eine Benutzeroberfläche zu definieren, Übergänge, und wie Sie iOS-Designer verwenden, um Storyboard-Dateien zu bearbeiten.
ms.prod: xamarin
ms.assetid: A3339BD2-9F56-7965-25F5-4B7C991EB775
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: f24be635afcba181efcab85d81a984d93dae4bc8
ms.sourcegitcommit: 650458de1d362cd7de174cacef7838f0e74426f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2019
ms.locfileid: "58071111"
---
# <a name="introduction-to-storyboards-in-xamarinios"></a>Einführung in Storyboards in Xamarin.iOS

In diesem Handbuch wird erläutert, welche ein Storyboard ist, und überprüfen Sie einige der wichtigsten Komponenten – z. B. segues beschrieben. Betrachten wir wie Storyboards erstellt und verwendet wird, werden können und welche Vorteile sie haben für einen Entwickler.

Vor der Einführung der Storyboard-Dateiformat von Apple als eine visuelle Darstellung der Benutzeroberfläche einer iOS-Anwendung Entwickler XIB-Dateien für jedes View Controller erstellt, und die Navigation zwischen den einzelnen Ansichten manuell programmiert.  Mit einem Storyboard kann der Entwickler, die sowohl View Controller als auch die Navigation zwischen ihnen auf einer Entwurfsoberfläche definieren und bietet WYSIWYG-Bearbeitung der Benutzeroberfläche der Anwendung.

Ein Storyboard kann mit dem Xamarin.IOS-Designer erstellt, geöffnet und bearbeitet werden. Dieses Handbuch wird auch Exemplarische Vorgehensweise wie den Designer verwendet, um die Storyboards zu erstellen, bei der Verwendung von C#-Programmierung im Navigationsbereich.


## <a name="requirements"></a>Anforderungen

Storyboards können mit dem iOS-Designer in Visual Studio für Mac oder mit Visual Studio 2017 mit Xamarin Workloads installiert verwendet werden.

## <a name="what-is-a-storyboard"></a>Was ist ein Storyboard?

Ein Storyboard ist die visuelle Darstellung aller Bildschirme in einer Anwendung. Sie enthält eine Abfolge von Szenen, mit jeweils Szene darstellen einer *View Controller* und die zugehörige *Ansichten*. Diese Ansichten enthalten möglicherweise Objekte und [Steuerelemente](~/ios/user-interface/controls/index.md) ermöglichen, die Ihre Benutzer für die Interaktion mit Ihrer Anwendung. Diese Sammlung von Ansichten und Steuerelemente (oder *Unteransichten*) wird als bezeichnet ein *Hierarchie von Inhaltsansichten*. Im Hintergrund verbunden sind von segue-Objekte, die einen Übergang zwischen ansichtscontrollern darstellen. Dies erfolgt normalerweise durch das Erstellen eines segues zwischen einem Objekt auf die anfängliche Ansicht, und die Sicht eine Verbindung herstellt. Die Beziehungen auf der Entwurfsoberfläche sind in der folgenden Abbildung dargestellt:

 [![](images/storyboardsview.png "Die Beziehungen auf der Entwurfsoberfläche sind in dieser Abbildung dargestellt.")](images/storyboardsview.png#lightbox)

Wie gezeigt, wird das Storyboard wird das Layout aller den Szenen mit Inhalt, die bereits gerendert und zeigt die Verbindungen zwischen ihnen.  Beachten Sie an diesem Punkt, wenn wir Informationen über Szenen auf einem iPhone sprechen, sicher, dass eine angenommen wird *Szene* auf dem Storyboard entspricht einem *Bildschirm* des Inhalts auf dem Gerät. Allerdings verwenden mit einem iPad ist es möglich, dass mehrere Szenen, die gleichzeitig – angezeigt werden sollen Sie beispielsweise einen Popover-View-Controller.

Es gibt viele Vorteile gegenüber der Verwendung von Storyboards Benutzeroberfläches Ihrer Anwendung, insbesondere bei Verwendung von Xamarin zu erstellen. Zunächst einmal ist es eine visuelle Darstellung der Benutzeroberfläche, wie alle Objekte – einschließlich [benutzerdefinierte Steuerelemente](~/ios/user-interface/designer/ios-designable-controls-overview.md) – werden zur Entwurfszeit gerendert wird. Dies bedeutet, dass vor dem Erstellen oder Bereitstellen der Anwendung, die Sie die Darstellung und Flow visuell darstellen können. Nehmen Sie zum Beispiel der Abbildung oben wird. Wir können feststellen, über einen kurzen Blick auf das Design Surface wie viele Szenen vorhanden sind, das Layout der einzelnen Ansichten und wie alles verknüpft ist. Dies ist, was Storyboards so leistungsfähig machen.

Ereignisse sind in Storyboards, besser verwaltbare, insbesondere bei Verwendung von iOS-Designer. Die meisten Benutzeroberflächen-Steuerelemente müssen eine Liste der möglichen Ereignisse im Bereich "Eigenschaften". Der Ereignishandler hier hinzugefügt, und in einer partiellen Methode in der View Controller-Klasse abgeschlossen werden kann...

Der Inhalt des Storyboards wird als eine XML-Datei gespeichert. Zur Erstellungszeit, alle `.storyboard` Dateien in binäre Dateien, die als Nibs bezeichnet kompiliert werden. Zur Laufzeit werden diese Nibs initialisiert und instanziiert, um neue Ansichten erstellen.

## <a name="segues"></a>Segues

Ein *Segue*, oder *Segue Objekt*, wird in der iOS-Entwicklung verwendet, um einen Übergang zwischen Szenen darstellen. Um einem Segue zu erstellen, halten Sie die **STRG** Schlüssel und ziehen Sie aus einer Szene in einen anderen. Wie wir unsere Maus ziehen, wird eine blauen Konnektor angezeigt, in, der angibt, wie in der folgenden Abbildung gezeigt, in dem der Segue führen:

 [![](images/createsegue.png "Einen blauen Konnektor angezeigt, der angibt, wie in dieser Abbildung gezeigt, in dem der Segue führen")](images/createsegue.png#lightbox)

Auf Maus-nach-oben wird ein Menü angezeigt, und lassen Sie uns die Aktion für unsere Segue auswählen. Sie können die Bilder unten ähneln: 

**Vor iOS 8 und Größe Klassen**:

[![](images/segue1.png "Der Dropdownliste \"Aktion Segue\" ohne Größenklassen")](images/segue1.png#lightbox)

**Bei Verwendung von Größenklassen und Adaptive Segues**:

[![](images/16new.png "Die Aktion Segue-Dropdownliste mit Größenklassen")](images/16new.png#lightbox)

> [!IMPORTANT]
> Wenn Sie VMWare für Ihre virtuellen Windows-Computer verwenden, wird Strg + Klick zugeordnet, als die _mit der rechten Maustaste_ los standardmäßig. Bearbeiten Sie zum Erstellen eines Segues Ihre Tastatur-Einstellungen über **Voreinstellungen** > **Tastatur und Maus** > **Mausaktionen** und Neuzuordnen von Ihrem **Sekundären Schaltfläche** wie unten gezeigt:
> 
> [![](images/image22.png "Tastatur und Maus-Voreinstellungen")](images/image22.png#lightbox)
> 
> Sie sollten jetzt eine Segue zwischen Ihrem Ansichtscontroller wie gewohnt hinzufügen können.

Es gibt verschiedene Arten von Übergängen, jedes haben Kontrolle darüber, wie ein neuen View-Controller für den Benutzer angezeigt wird und wie sie mit anderen ansichtscontroller im Storyboard interagiert. Diese werden nachfolgend erklärt. Es ist auch möglich, um eine Unterklasse einer Segue-Objekt, um einen benutzerdefinierten Übergang zu implementieren:

-  **Ein- / mithilfe von Push übertragen werden** – ein Push segue Navigationsstapel des View Controller hinzugefügt. Es wird davon ausgegangen, dass der ansichtscontroller, die den Push stammen Teil der gleichen navigationscontroller als ansichtscontroller ist, die auf dem Stapel hinzugefügt wird. Dies ist dasselbe wie `pushViewController` , und wird im Allgemeinen verwendet, wenn eine Beziehung zwischen den Daten auf dem Bildschirm vorhanden ist. Mit den Push-segue erhalten Sie den Luxus, dass eine Navigationsleiste mit einer Schaltfläche "zurück" und der Titel jeder Ansicht auf dem Stapel, sodass Drilldown Navigation durch die Hierarchie von Inhaltsansichten hinzugefügt.
-  **Modale** : eine modale Segue Erstellen einer Beziehung zwischen jeder zwei View-Controller im Projekt, durch die Möglichkeit, einen animierten Übergang, der angezeigt wird. Der untergeordnete-View-Controller wird den übergeordneten ansichtscontroller, wenn in der Ansicht vollständig verdecken. Im Gegensatz zu einem Push segue, die eine zurück-Schaltfläche für uns hinzufügt; Wenn eine modale mit segue `DismissViewController` muss verwendet werden, um auf den vorherigen View-Controller zurückzugeben.
-  **Benutzerdefinierte** – einem beliebigen benutzerdefinierten Segue kann erstellt werden, wenn eine Unterklasse von ` UIStoryboardSegue`.
-  **Entladen** – eine Entladung segue können verwendet werden, um das Navigieren durch ein Push- oder modale segue – beispielsweise durch Schließen des ansichtscontrollers mit modal angezeigt. Darüber hinaus können Sie über nicht nur eine entladen, aber eine Reihe von Push- und modale segues und wechseln Sie zurück, dass mehrere Schritte in Ihrer Navigationshierarchie mit einer einzelnen Aktion entladen. Um zu verstehen, wie Sie mit der eine Entladung segue im iOS-, lesen Sie die [erstellen Segues entladen](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/storyboard/unwind_segue) Anleitung.
-  **Sourceless** – ein sourceless Segue gibt an, der Szene, die den ersten ansichtscontroller enthält und daher die anzeigen, dass des Benutzers als erstes angezeigt wird. Es wird durch die Segue unten dargestellt:  

    [![](images/sourcelesssegue.png "Ein sourceless segue")](images/sourcelesssegue.png#lightbox)

### <a name="adaptive-segue-types"></a>Adaptive Segue Typen

 iOS 8 eingeführten [Größenklassen](~/ios/user-interface/storyboards/unified-storyboards.md#size-classes) um ein iOS-Storyboard-Datei mit allen verfügbaren Bildschirmgrößen, ermöglicht es Entwicklern, die zum Erstellen einer Benutzeroberfläche für alle iOS-Geräte arbeiten zu ermöglichen. Standardmäßig werden alle neue Xamarin.iOS-Anwendungen Größenklassen verwenden. Um Größenklassen aus ein älteres Projekt verwenden zu können, finden Sie in der [Einführung in Storyboards Unified](~/ios/user-interface/storyboards/unified-storyboards.md) Guide. 
 
Jede Anwendung, der Größenklassen verwendet auch das neue [ *Adaptive Segues*](~/ios/user-interface/storyboards/unified-storyboards.md). Wenn Größenklassen verwenden zu können, denken Sie daran, dass wir direkt durchgeführt angegeben, werden nicht verwenden wir eine iPhone oder iPad. Anders ausgedrückt: Wir erstellen eine Benutzeroberfläche, die immer gleich, unabhängig davon, wie viel Platz, die Arbeit dargestellt werden mit. Adaptive Segues Arbeit beurteilt die Umgebung, und legen Sie optimal von Inhalt. Die Adaptive Segues sind im folgenden dargestellt: 

[![](images/adaptivesegue.png "Der Adaptive Segues-Dropdownliste")](images/adaptivesegue.png#lightbox)

|Segue|Beschreibung|
|--- |--- |
|Einblenden|Dies ist sehr ähnlich ein Push-segue, jedoch berücksichtigt den Inhalt des Bildschirms.|
|Details anzeigen|Wenn die app eine Master- und Detailtabelle anzeigen (z. B. in einem Controller für geteilte Ansicht auf einem iPad) angezeigt wird, ersetzt der Inhalt die Detailansicht. Wenn die app nur die Master oder Detail angezeigt wird, ersetzt der Inhalt der Anfang des Stapels Controller anzeigen.|
|Präsentation|Dies ist vergleichbar mit der modale Segue und ermöglicht die Auswahl des Präsentation und Übergang.|
|Im Popover Präsentation|Dies stellt die Inhalte als ein popover|

### <a name="transferring-data-with-segues"></a>Übertragen von Daten mit Segues

Die Vorteile eines segues enden keine Übergänge. Sie können auch verwendet werden, um die Übertragung von Daten zwischen ansichtscontrollern zu verwalten. Dies erfolgt durch Überschreiben der `PrepareForSegue` Methode für den ersten ansichtscontroller und Verarbeiten von Daten selbst. Wenn der Segue ausgelöst wird – z. B. mit Klicken einer Schaltfläche – die Anwendung wird rufen Sie diese Methode, einnahmemöglichkeiten zum Vorbereiten des neuen View-Controllers *vor dem* alle navigiert wird. Die folgenden Code aus der [Phoneword](https://developer.xamarin.com/samples/monotouch/Hello_iOS/) zugreifen können, wird dies veranschaulicht: 


```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, 
NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    var callHistoryController = segue.DestinationViewController 
                                  as CallHistoryController;

    if (callHistoryController != null) {
        callHistoryController.PhoneNumbers = PhoneNumbers;
    }
}
```

In diesem Beispiel die `PrepareForSegue` Methode wird aufgerufen, wenn der Segue durch den Benutzer ausgelöst wird. Wir müssen zunächst erstellen Sie eine Instanz des ansichtscontrollers "empfangen" und als Ziel für die Segue des View Controller festlegen. Dies erfolgt durch die folgende Codezeile:

```csharp
var callHistoryController = segue.DestinationViewController as CallHistoryController;
```

Die Methode verfügt jetzt über die Möglichkeit zum Festlegen von Eigenschaften auf der `DestinationViewController`. In diesem Beispiel habe ich diesen Vorteil durch Übergabe einer Liste namens `PhoneNumbers` auf die `CallHistoryController` und auf ein Objekt mit dem gleichen Namen zuweisen:

```csharp
if (callHistoryController != null) {
        callHistoryController.PhoneNumbers = PhoneNumbers;
    }
```

Sobald der Übergang abgeschlossen ist, sieht der Benutzer die `CallHistoryController` mit der aufgefüllten Liste.

## <a name="adding-a-storyboard-to-a-non-storyboard-project"></a>Hinzufügen eines Storyboards zu einem nicht-Storyboard-Projekt

Gelegentlich müssen Sie ein Storyboard zu einer zuvor nicht-Storyboard-Datei hinzufügen. Sobald dies in Visual Studio für Mac kann optimiert werden, durch die folgenden Schritte aus:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Erstellen Sie eine neue Storyboarddatei, indem Sie zu **Datei > neue Datei > iOS > Storyboard**, wie unten gezeigt: 
    
    [![](images/new-storyboard-xs.png "Das neue Dialogfeld \"Datei\"")](images/new-storyboard-xs.png#lightbox)

2. Fügen Sie den Namen Ihres Storyboards, um die **Hauptschnittstelle** Teil der **"Info.plist"**, wie unten dargestellt:
    
    [![](images/infoplist.png "Die Datei \"Info.plist\"-editor")](images/infoplist.png#lightbox)
    
    Dies entspricht dem von den ersten Ansichtscontroller im Instanziieren der `FinishedLaunching` Methode innerhalb der App-Delegat. Mit dieser Option festgelegt ist, die Anwendung instanziiert ein Fenster (siehe unten), lädt die Haupt-Storyboard aus, und weist eine Instanz der Storyboard Initial View Controller (denjenigen neben der sourceless Segue) als die `RootViewController` Eigenschaft des Fensters, und klicken Sie dann macht Das Fenster auf dem Bildschirm sichtbar ist.

3. In der `AppDelegate`, überschreiben die Standardeinstellung `Window` Methode mit den folgenden Code hinzu, die Window-Eigenschaft zu implementieren:
        
        public override UIWindow Window {
            get;
            set;
            }
            
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Erstellen Sie eine neue Storyboarddatei mit der rechten Maustaste auf das Projekt, um **hinzufügen > neue Datei > iOS > leeres Storyboard**, wie unten gezeigt: 
    
    [![](images/new-storyboard-vs.png "Das Dialogfeld \"Neues Element\"")](images/new-storyboard-vs.png#lightbox)

2. Fügen Sie den Namen Ihres Storyboards, um die **Hauptschnittstelle** Teil der iOS-Anwendung, wie unten dargestellt:
    
    [![](images/ios-app.png "Die Datei \"Info.plist\"-editor")](images/ios-app.png#lightbox)
    
    Dies entspricht dem von den ersten Ansichtscontroller im Instanziieren der `FinishedLaunching` Methode innerhalb der App-Delegat. Mit dieser Option festgelegt ist, die Anwendung instanziiert ein Fenster (siehe unten), lädt die Haupt-Storyboard aus, und weist eine Instanz der Storyboard Initial View Controller (denjenigen neben der sourceless Segue) als die `RootViewController` Eigenschaft des Fensters, und klicken Sie dann macht Das Fenster auf dem Bildschirm sichtbar ist.

3. In der `AppDelegate`, überschreiben die Standardeinstellung `Window` Methode mit den folgenden Code hinzu, die Window-Eigenschaft zu implementieren:

        public override UIWindow Window {
            get;
            set;
            }
            
-----

## <a name="creating-a-storyboard-with-the-ios-designer"></a>Erstellen ein Storyboard mit dem iOS-Designer

Ein Storyboard kann mit dem Xamarin-Designer für iOS, die nahtlos in Visual Studio für Mac und Visual Studio integriert ist, erstellt werden.

Um erste Schritte mit der iOS-Designer zum Erstellen von Storyboards, befolgen Sie die [Hallo, iOS Multiscreen](~/ios/get-started/hello-ios-multiscreen/index.md) Guide. In dieser exemplarischen Vorgehensweise untersuchen Sie die Navigation zwischen Ansichtscontrollern mit segues beschrieben und wie Ereignisse auf die Steuerelemente behandelt.

## <a name="instantiate-storyboards-manually"></a>Manuell instanziieren von Storyboards

Storyboards völlig einzelne XIB-Dateien in Ihrem Projekt, ersetzen Sie jedoch einzelne View-Controller in einem Storyboard weiterhin instanziiert werden können, mit `Storyboard.InstantiateViewController`.

In einigen Fällen verfügen Anwendungen über besondere Anforderungen, die mit der integrierten Storyboard-Übergänge, die vom Designer bereitgestellte nicht behandelt werden können. Würden wir eine Anwendung erstellen, die unterschiedliche Bildschirmen über die gleiche Schaltfläche je nach den aktuellen Zustand einer Anwendung startet sollten wir z. B. der ansichtscontroller manuell instanziieren und den Übergang Programm selbst.

Der folgende Screenshot zeigt, dass zwei ansichtscontrollern auf unsere Entwurfsoberfläche ohne dazwischen segue an. Im nächste Abschnitt wird erläutert wie diesen Übergang im Code eingerichtet werden kann.

 [![](images/viewcontrollerspink.png "Dieser Screenshot zeigt, dass zwei ansichtscontrollern auf der Entwurfsoberfläche ohne dazwischen segue")](images/viewcontrollerspink.png#lightbox)

1. Hinzufügen einer _leeres iPhone-Storyboard_ zu einem vorhandenen Projekt-Projekt:
    
    [![](images/add-storyboard1.png "Hinzufügen von Storyboards")](images/add-storyboard1.png#lightbox)

2. Klicken Sie mit der Doppelklicken auf das neu erstellte Storyboard aus, um ihn zu öffnen, und fügen Sie einen neuen **Navigationscontroller** auf die Entwurfsoberfläche. Wie wird der Navigationscontroller UI-lose, standardmäßig, es mit einem Root View Controller geliefert wird, wie unten gezeigt:

    [![](images/uinavigationcontroller.png "View-Controller mit Segues")](images/uinavigationcontroller.png#lightbox)

3. Wählen Sie die _Ansichtscontroller_ durch Klicken auf die schwarze Leiste am unteren Rand. Im Designer des **Pad "Eigenschaft"** unter **Identität** können wir eine benutzerdefinierte Klasse als auch eine eindeutige ID für den Ansichtscontroller festlegen. Legen Sie die **Klassenname** und **Storyboard-ID** zu `MainViewController`.

    [![](images/identitypanelnew.png "Geben Sie die benutzerdefinierte Klasse")](images/identitypanelnew.png#lightbox)

4. Später müssen wir unsere ansichtscontroller aus dem Storyboard instanziieren und verwenden Sie die Storyboard-ID wird auf sie in unserem Code verweisen. Festlegen der ID "Wiederherstellung" entsprechend der Storyboard-ID wird sichergestellt, dass der ansichtscontroller ordnungsgemäß neu erstellt Ruft, wenn der Zustand wiederhergestellt werden muss.

5. Wir stellen Ihnen derzeit nur ein View-Controller. Ziehen Sie eine andere ansichtscontroller, auf die Entwurfsoberfläche. In der **Pad "Eigenschaft"**, unter der Identität, legen Sie die Klasse und ein Storyboard-ID, `PinkViewController`, wie unten gezeigt:

    [![](images/pinkvcnew.png "Das Pad \"Eigenschaft\"")](images/pinkvcnew.png#lightbox)
    
    Die IDE erstellt diese benutzerdefinierten Klassen für den View-Controller. Diese können angezeigt werden, der **Lösungspad**, wie im folgenden Screenshot dargestellt:
    
    [![](images/solution-pad.png "Projektmappenpad")](images/solution-pad.png#lightbox)

6. In der `PinkViewController`, wählen Sie die Ansicht, indem Sie in Richtung der Mitte des Controllers Frames auf. Ändern Sie im Bereich "Eigenschaften" unter Ansicht die **Hintergrund** , Magenta:
    
    [![](images/pinkcontroller.png "Festlegen der Hintergrundfarbe")](images/pinkcontroller.png#lightbox)

7. Ziehen Sie abschließend eine Schaltfläche aus der **ToolBox** auf die `MainViewController`. Im Bereich "Eigenschaften", geben Sie ihm den Namen `PinkButton` und den Titel GoToPink, wie unten gezeigt:

    [![](images/pinkbutton.png "Name der Schaltfläche festlegen")](images/pinkbutton.png#lightbox)

Das Storyboard ist abgeschlossen, aber wenn wir jetzt das Projekt bereitstellen, erhalten wir einen leeren Bildschirm. Wir müssen noch mitteilen von der IDE verwenden Sie unsere Storyboard und einen Root View Controller einrichten, als die erste Ansicht dient der Grund ist. Normalerweise kann dies über unsere Projektoptionen erfolgen wie oben gezeigt. In diesem Beispiel werden wir jedoch dazu, dass das gleiche Ergebnis im Code Folgendes hinzufügen der **AppDelegate**:

```csharp
public partial class AppDelegate : UIApplicationDelegate
    {
        UIWindow window;
        public static UIStoryboard Storyboard = UIStoryboard.FromName ("MainStoryboard", null);
        public static UIViewController initialViewController;

        public override bool FinishedLaunching (UIApplication app, NSDictionary options)
        {
            window = new UIWindow (UIScreen.MainScreen.Bounds);

            initialViewController = Storyboard.InstantiateInitialViewController () as UIViewController;

            window.RootViewController = initialViewController;
            window.MakeKeyAndVisible ();
            return true;
        }

    }
```

Das ist viel Code, aber nur einige Zeilen nicht vertraut sind. Zuerst registrieren wir unsere Storyboard mit den **AppDelegate** durch Übergabe des Storyboards Namen **MainStoryboard**. Als Nächstes teilen wir die Anwendung zum Instanziieren eines Controllers anfängliche Ansicht, aus dem Storyboard durch Aufrufen von `InstantiateInitialViewController` für unsere Storyboard, und wir legen Sie diese ansichtscontroller als stammansichtscontroller der Anwendung. Diese Methode bestimmt den ersten Bildschirm, der dem Benutzer angezeigt wird, und erstellt eine neue Instanz der, View-Controller.

Beachten Sie, dass im projektmappenbereich, die die IDE erstellt hat eine `MainViewcontroller.cs` -Klasse, und die zugehörige `corresponding designer.cs` Wenn wir den Namen der Klasse für das Pad "Eigenschaften" in Schritt 4 hinzugefügt. Wir können sehen, dass diese Klasse erstellt einen speziellen Konstruktors, der eine Basisklasse enthält:

```csharp
public MainViewController (IntPtr handle) : base (handle) 
{
}
```


Wenn Sie ein Storyboard mit dem Designer zu erstellen, wird die IDE automatisch hinzufügen der [[registrieren]](xref:Foundation.RegisterAttribute) Attribut am Anfang der `designer.cs` Klasse, und übergeben Sie einen Zeichenfolgenbezeichner, die mit der Storyboard-ID, die im angegebenen identisch ist die vorherigen Schritt. Es werden die C# -Code der relevanten Szene im Storyboard verknüpft.

Sie möchten eine vorhandene Klasse hinzufügen, die wurde irgendwann **nicht** im Designer erstellt. In diesem Fall würden Sie diese Klasse als normale registrieren:

```csharp
[Register ("MainViewController")]
public partial class MainViewController : UIViewController
{
public MainViewController (IntPtr handle) : base (handle) 
{
}

...
}
```

Weitere Informationen zum Registrieren von Klassen und Methoden finden Sie in der [Typ Registrierungsstelle](http://docs.xamarin.com/guides/ios/advanced_topics/registrar/) Dokumentation.

Der letzte Schritt in dieser Klasse wird zum Verknüpfen der Schaltfläche und den Übergang in den rosa ansichtscontroller. Es instanziiert werden die `PinkViewController` aus dem Storyboard; dann wird programmieren Sie eine Pushbenachrichtigung mit segue `PushViewController`, wie in den folgenden Beispielcode dargestellt:

```csharp
public partial class MainViewController : UIViewController
{
    UIViewController pinkViewController;

    public MainViewController (IntPtr handle) : base (handle)
    {

    }

    public override void AwakeFromNib ()
    {
    // Called when loaded from xib or storyboard.

    this.Initialize ();
    }

    public void Initialize(){

    //Instantiating View Controller with Storyboard ID 'PinkViewController'
    pinkViewController = Storyboard.InstantiateViewController ("PinkViewController") as PinkViewController;
    }

    public override void ViewDidLoad ()
    {
    base.ViewDidLoad ();

    //When we push the button, we will push the pinkViewController onto our current Navigation Stack
    PinkButton.TouchUpInside += (o, e) =&gt; {
        this.NavigationController.PushViewController (pinkViewController, true);
    };
    }

}
```

Ausführen der Anwendung erzeugt eine 2-Bildschirm der Anwendung:

![](images/finishedstoryboard.png "Beispielapp-Ausführung von Bildschirmen")

## <a name="conditional-segues"></a>Bedingte Segues

Von einem View-Controller mit dem nächsten fortgefahren ist häufig eine bestimmte Bedingung abhängig. Z. B. wenn wir einen einfachen Anmeldebildschirm vornehmen wurden wir würden nur mit dem nächsten Bildschirm verschieben möchten *Wenn* des Benutzernamens und Kennworts war überprüft worden.

Im nächsten Beispiel fügen wir ein Feld "Kennwort" im Beispiel oben. Der Benutzer wird nur in der Lage, den Zugriff auf die *PinkViewController* Wenn sie das richtige Kennwort eingeben, andernfalls ein Fehler wird angezeigt.

Bevor wir beginnen, befolgen Sie die Schritte 1 – 8 oben. In den folgenden Schritten können wir auch unsere Storyboard erstellen, beginnen zu unserer Benutzeroberfläche zu erstellen und teilen Sie die View-Controller mit unserer App-Delegat RootViewController unverändert.

1. Nun, wir unserer Benutzeroberfläche erstellen, und fügen Sie die zusätzlichen Ansichten aufgeführt die `MainViewController` dialogfeldbasiert im folgenden Screenshot aussehen:

    - UITextField
        - Name: PasswordTextField
        - Platzhalter: 'Das geheime Kennwort eingeben'
    - UILabel
        - Text: "Fehler: Falsches Kennwort. Sie sollte nicht erfolgreich. "
        - Farbe: Rot
        - Ausrichtung: Center
        - Zeilen: 2
        - Kontrollkästchen 'Hidden' 
        
    [![](images/passwordvc.png "Center Zeilen")](images/passwordvc.png#lightbox)
    
2. Erstellen Sie einen Segue zwischen die wechseln, sondern rosa und der View-Controller durch STRG Ziehen aus der *PinkButton* auf die *PinkViewController*, und auswählen **mithilfe von Push übertragen** auf Maus-nach-oben . 

3. Klicken Sie auf der Segue und ihm die *Bezeichner* `SegueToPink`:

    [![](images/namesegue.png "Klicken Sie auf der Segue, und geben sie den Bezeichner SegueToPink")](images/namesegue.png#lightbox)  
    

4. Abschließend fügen Sie die folgende ShouldPerformSegue-Methode, die `MainViewController` Klasse:

    ```csharp
    public override bool ShouldPerformSegue (string segueIdentifier, NSObject sender)
    {
        
        if(segueIdentifier == "SegueToPink"){
            if (PasswordTextField.Text == "password") {
                PasswordTextField.ResignFirstResponder ();
                return true;
            }
            else{
                ErrorLabel.Hidden = false;
                return false;
            }
        }
        return base.ShouldPerformSegue (segueIdentifier, sender);
    }
    ```

In diesem Code haben wir die SegueIdentifier auf Übereinstimmung unsere `SegueToPink` segue, damit wir dann, eine Bedingung testen können; in diesem Fall ein gültiges Kennwort. Wenn unsere Bedingung zurückgegeben werden, `true`, der Segue wird ausgeführt, und bietet die `PinkViewController`. Wenn `false`, die neue View Controller werden nicht angezeigt werden.

Wir können diesen Ansatz auf alle Segue auf diesem ansichtscontroller anwenden, anhand der SegueIdentifier-Argument für die ShouldPerformSegue-Methode. In diesem Fall haben wir nur einen Segue Bezeichner – `SegueToPink`.

Finden Sie in der Storyboards.Conditional-Lösung in die [manuelle Storyboards Beispiel](https://developer.xamarin.com/samples/monotouch/ManualStoryboard/) ein funktionsfähiges Beispiel.

<a name="Using-Storyboard-References" />

## <a name="using-storyboard-references"></a>Verwenden von Storyboard-verweisen

Ein Storyboard-Verweis können Sie einen großen, komplexen Storyboard-Entwurf und teilen sie in kleinere Storyboards, die aus der ursprünglichen referenziert erhalten daher entfernen die Komplexität und erleichtert die sich ergebende einzelne Storyboards zum Entwerfen und verwalten.

Darüber hinaus bieten ein Storyboard-Verweis ein _Anker_ zu einem anderen Szene innerhalb des gleichen Storyboards oder eine bestimmte Szene auf einem anderen.

<a name="Referencing-an-External-Storyboard" />

### <a name="referencing-an-external-storyboard"></a>Verweisen auf ein externes Storyboard

Um einen Verweis auf ein externes Storyboard hinzuzufügen, führen Sie folgende Schritte aus:

1. In der **Projektmappen-Explorer**mit der rechten Maustaste auf den Projektnamen, und wählen Sie **hinzufügen** > **neue Datei...**   >  **iOS** > **Storyboard**. Geben Sie einen **Namen** für das neue Storyboard und auf die **neu** Schaltfläche:
    
    [![](images/ref01.png "Das neue Dialogfeld \"Datei\"")](images/ref01.png#lightbox)
    
2. Entwerfen Sie das Layout der das neue Storyboard Szenen wie gewohnt würde und die Änderungen zu speichern: 
    
    [![](images/ref02.png "Das Layout der neuen Szene")](images/ref02.png#lightbox)
    
3. Öffnen Sie das Storyboard, das auf den Verweis in der iOS-Designer hinzugefügt werden.

4. Ziehen Sie eine **Storyboard-Verweis** aus der **Toolbox** auf die Entwurfsoberfläche: 
    
    [![](images/ref03.png "Ein Storyboard-Verweis")](images/ref03.png#lightbox)
    
5. In der **Widget** Registerkarte die **Eigenschaften-Explorer**, wählen Sie den Namen des der **Storyboard** , die Sie soeben erstellt haben: 

    [![](images/ref04.png "Die Registerkarte \"Widget\"")](images/ref04.png#lightbox)
    
6. Strg + Klick auf ein UI-Widget (z. B. eine Schaltfläche) auf einer vorhandenen Szene, und erstellen Sie eine neue Segue, die **Storyboard Verweis** , die Sie gerade erstellt haben: 

    [![](images/ref05.png "Erstellen eines segues")](images/ref05.png#lightbox) 
    
7. Wählen Sie im Popupmenü **anzeigen** zum Abschließen der Segue: 

    [![](images/ref06.png "Anzeigen, die zum Abschließen der Segue auswählen")](images/ref06.png#lightbox) 
    
8. Speichern Sie die Änderungen auf das Storyboard an.

Wenn die app ausgeführt wird und der Benutzer auf das Element der Benutzeroberfläche klickt der Erstellung der Segue aus, dem Initial View Controller aus dem externen Storyboard in der Storyboard-Verweis angegeben wird angezeigt.

<a name="Referencing-a-Specific-Scene-in-an-External-Storyboard" />

### <a name="referencing-a-specific-scene-in-an-external-storyboard"></a>Verweisen auf eine bestimmte Szene in einer externen Storyboard

Um einen Verweis auf eine bestimmte Szene hinzufügen wie eines externen Storyboards (und nicht die Initial View Controller) folgt vor:

1. In der **Projektmappen-Explorer**, doppelklicken Sie auf das externe Storyboard aus, um sie für die Bearbeitung zu öffnen.

2. Fügen Sie eine neue Szene, und Entwerfen Sie des Layouts, wie gewohnt aus: 

    [![](images/ref07.png "Das neue Layout der Szene")](images/ref07.png#lightbox)
    
3. In der **Widget** Registerkarte die **Eigenschaften-Explorer**, geben Sie einen **Storyboard-ID** für der neuen Szene-View-Controller: 

    [![](images/ref08.png "Geben Sie ein Storyboard-ID für den neuen Szenen-Ansichtscontroller")](images/ref08.png#lightbox)
    
3. Öffnen Sie das Storyboard, das auf den Verweis in der iOS-Designer hinzugefügt werden.

4. Ziehen Sie eine **Storyboard-Verweis** aus der **Toolbox** auf die Entwurfsoberfläche: 

    [![](images/ref03.png "Ein Storyboard-Verweis")](images/ref03.png#lightbox)
    
5. In der **Widget** auf der Registerkarte die **Eigenschaften-Explorer**, wählen Sie den Namen des der **Storyboard** und **Referenz-ID** (Storyboard-ID) von der Eine Szene, die Sie soeben erstellt haben: 

    [![](images/ref09.png "Die Registerkarte \"Widget\" ")](images/ref09.png#lightbox)
    
6. Strg + Klick auf ein UI-Widget (z. B. eine Schaltfläche) auf einer vorhandenen Szene, und erstellen Sie eine neue Segue, die **Storyboard Verweis** , die Sie gerade erstellt haben: 

    [![](images/ref10.png "Erstellen eines segues")](images/ref10.png#lightbox) 
    
7. Wählen Sie im Popupmenü **anzeigen** zum Abschließen der Segue: 

    [![](images/ref06.png "Anzeigen, die zum Abschließen der Segue auswählen")](images/ref06.png#lightbox) 
    
8. Speichern Sie die Änderungen auf das Storyboard an.

Wenn die app ausführen und dem Benutzer klickt auf das Element der Benutzeroberfläche, die Sie aus, mit der Segue erstellt die angegebenen **Storyboard-ID** aus dem externen Storyboard in der Storyboard-Verweis angegeben wird angezeigt.

<a name="Referencing-a-Specific-Scene-in-the-Same-Storyboard" />

### <a name="referencing-a-specific-scene-in-the-same-storyboard"></a>Verweisen auf eine bestimmte Szene im gleichen Storyboard

Um einen Verweis auf eine bestimmte Szene das gleiche Storyboard hinzuzufügen, führen Sie folgende Schritte aus:

1. In der **Projektmappen-Explorer**, doppelklicken Sie auf das Storyboard, um es zur Bearbeitung zu öffnen.

2. Fügen Sie eine neue Szene, und Entwerfen Sie des Layouts, wie gewohnt aus: 

    [![](images/ref11.png "Das neue Layout der Szene")](images/ref11.png#lightbox)

3. In der **Widget** Registerkarte die **Eigenschaften-Explorer**, geben Sie einen **Storyboard-ID** für der neuen Szene-View-Controller: 

    [![](images/ref12.png "Die Registerkarte \"Widget\"")](images/ref12.png#lightbox)
    
3. Ziehen Sie eine **Storyboard-Verweis** aus der **Toolbox** auf die Entwurfsoberfläche: 

    [![](images/ref03.png "Ein Storyboard-Verweis")](images/ref03.png#lightbox)
    
5. In der **Widget** Registerkarte die **Eigenschaften-Explorer**Option **Referenz-ID** (Storyboard-ID) der Szene, die Sie soeben erstellt haben: 

    [![](images/ref13.png "Die Registerkarte \"Widget\"")](images/ref13.png#lightbox)
    
6. Strg + Klick auf ein UI-Widget (z. B. eine Schaltfläche) auf einer vorhandenen Szene, und erstellen Sie eine neue Segue, die **Storyboard Verweis** , die Sie gerade erstellt haben: 

    [![](images/ref14.png "Erstellen eines segues")](images/ref14.png#lightbox) 
    
7. Wählen Sie im Popupmenü **anzeigen** zum Abschließen der Segue: 

    [![](images/ref06.png "Anzeigen, die zum Abschließen der Segue auswählen")](images/ref06.png#lightbox) 
    
8. Speichern Sie die Änderungen auf das Storyboard an.

Wenn die app ausführen und dem Benutzer klickt auf das Element der Benutzeroberfläche, die Sie aus, mit der Segue erstellt die angegebenen **Storyboard-ID** im gleichen Storyboard in der Storyboard-Verweis angegeben wird angezeigt.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel führt das Konzept von Storyboards und wie sie bei der Entwicklung von iOS-Anwendungen nützlich sein können. Es wird erläutert, Szenen, View Controller, Ansichten und Hierarchien von Inhaltsansichten und wie Szenen mit unterschiedlichen Segues verknüpft sind.  Es wird untersucht, auch instanziieren ansichtscontroller manuell von einem Storyboard und die Erstellung bedingter segues beschrieben.



## <a name="related-links"></a>Verwandte Links

- [Manuelle Storyboard (Beispiel)](https://developer.xamarin.com/samples/ManualStoryboard/)
- [Einführung in iOS-Designer](~/ios/user-interface/designer/introduction.md)
- [Konvertieren von Storyboards](https://developer.apple.com/library/ios/#releasenotes/Miscellaneous/RN-AdoptingStoryboards/)
- [UIStoryboard-Klassenreferenz](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UIStoryboard_Class/Reference/Reference.html)
