---
title: "Einführung in die Storyboards"
description: "Ein Storyboard ist eine visuelle Darstellung der Darstellung und Ablaufs der Anwendung. Xamarin einen Designer zum Zulassen von Xamarin.iOS Anwendungen nutzen von Storyboards, damit Sie Ihren Anwendungsbildschirm visuell entwerfen und Zugriff auf die Ansichten wurde eingeführt, Controller und segues mit c# mehr Kontrolle."
ms.topic: article
ms.prod: xamarin
ms.assetid: A3339BD2-9F56-7965-25F5-4B7C991EB775
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 85c05145ce2490468ac5d5fb9b8524853d46a9e3
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="introduction-to-storyboards"></a>Einführung in die Storyboards

_Ein Storyboard ist eine visuelle Darstellung der Darstellung und Ablaufs der Anwendung. Xamarin einen Designer zum Zulassen von Xamarin.iOS Anwendungen nutzen von Storyboards, damit Sie Ihren Anwendungsbildschirm visuell entwerfen und Zugriff auf die Ansichten wurde eingeführt, Controller und segues mit c# mehr Kontrolle._

In diesem Handbuch wird erläutert, welche ein Storyboard ist und einige der Hauptkomponenten – z. B. Segues untersuchen. Betrachten wir wie Storyboards erstellt und verwendet wird, werden können und welche Vorteile für Entwickler haben.

Vor der Einführung von Storyboard-Dateiformat von Apple, als eine visuelle Darstellung der Benutzeroberfläche einer iOS-Anwendung wurde Entwickler XIB-Dateien für jeden Controller Ansicht erstellt, und die Navigation zwischen jeder Ansicht manuell programmiert.  Ein Storyboard mit kann Entwickler View-Controller und die Navigation zwischen diesen auf einer Entwurfsoberfläche definieren und bietet WYSIWYG-Bearbeitung der Benutzeroberfläche der Anwendung.

Ein Storyboard kann erstellt, geöffnet und mit dem Xamarin iOS-Designer bearbeitet werden. Dieses Handbuch wird auch Exemplarische Vorgehensweise zum Designer zu verwenden, um die Storyboards erstellen, bei der Verwendung von C#-Programmierung die Navigation.


## <a name="requirements"></a>Anforderungen

Storyboards können mit der iOS-Designer in Visual Studio für Mac oder mit Visual Studio 2015 und 2017 mit der Xamarin-Arbeitslasten installiert verwendet werden.

## <a name="what-is-a-storyboard"></a>Was ist ein Storyboard?

Ein Storyboard ist die visuelle Darstellung von allen Bildschirmen in einer Anwendung. Sie enthält eine Sequenz von Szenen, mit jeweils Szene darstellen eine *Modellansichtcontroller* und dessen *Ansichten*. Diese Sichten können Objekte enthalten und [Steuerelemente](~/ios/user-interface/controls/index.md) ermöglicht, die Ihre Benutzer die Interaktion mit der Anwendung. Diese Auflistung von Ansichten und Steuerelemente (oder *Unteransichten*) wird als bezeichnet eine *Content Ansichtshierarchie*. Szenen verbunden sind durch segue Objekte, die einen Übergang zwischen Domänencontrollern Ansicht darstellen. Dies erfolgt normalerweise durch das Erstellen einer Segue zwischen einem Objekt auf die anfängliche Ansicht und das Herstellen einer Verbindung anzeigen. Die Beziehungen auf der Entwurfsoberfläche sind in der folgenden Abbildung dargestellt:

 [![](images/storyboardsview.png "Die Beziehungen auf der Entwurfsoberfläche sind in dieser Abbildung dargestellt.")](images/storyboardsview.png#lightbox)

Wie gezeigt, das Storyboard wird gestalten aller Ihrer Szenen mit Inhalt bereits gerendert und zeigt die Verbindungen zwischen ihnen.  Zertifikatvorgang an diesem Punkt, wenn wir im Hintergrund auf einem iPhone erörtern, vorausgesetzt, dass eine sicher ist *Szene* auf das Storyboard ist gleich eins *Bildschirm* des Inhalts auf dem Gerät. Wenn jedoch verwenden mit einem iPad ist es möglich, dass mehrere Szenen, die gleichzeitig – angezeigt werden z. B. einen Popover-View-Controller.

Es gibt viele Vorteile gegenüber der Verwendung von Storyboards So erstellen Sie Ihre Anwendung-Benutzeroberfläche, insbesondere wenn Sie Xamarin verwendet. Erstens ist eine visuelle Darstellung der Benutzeroberfläche, wie alle Objekte – einschließlich [benutzerdefinierte Steuerelemente](~/ios/user-interface/designer/ios-designable-controls-overview.md) – werden zur Entwurfszeit gerendert wird. Dies bedeutet, dass vor der Erstellung oder Bereitstellung der Anwendung, die die Darstellung und Datenfluss visuell darstellen können. Nehmen Sie z. B. der oben angezeigten Abbildung. Wir können feststellen, über einen kurzen Blick auf den Entwurf-Oberfläche wie viele Szenen sind, das Layout der einzelnen Ansichten und wie alles in Beziehung stehen. Dies ist, was Storyboards so leistungsfähig.

Ereignisse werden mit Storyboards, besser verwaltbare, insbesondere dann, wenn Sie den iOS-Designer verwenden. Die meisten UI-Steuerelemente müssen eine Liste der möglichen Ereignisse in den Eigenschaften aufgefüllt. Der Ereignishandler kann hier auch hinzugefügt und wurde in einer partiellen Methode in der Ansicht Controller-Klasse werden..

Der Inhalt eines Drehbuchs wird als eine XML-Datei gespeichert. Zur Buildzeit alle `.storyboard` Dateien in binäre Dateien, die als Nibs bezeichnet kompiliert werden. Zur Laufzeit werden diese Nibs initialisiert und instanziiert, um neue Ansichten erstellen.

## <a name="segues"></a>Segues

Ein *Segue*, oder *Segue Objekt*, wird in der iOS-Entwicklung verwendet, um einen Übergang zwischen Szenen darzustellen. Um eine Segue zu erstellen, halten Sie die **STRG** Schlüssel, und klicken und ziehen aus eine Szene in eine andere. Wie wir unsere Maus ziehen, wird eine blauen Konnektor angezeigt, der angibt, in dem die Segue zur Folge hat, wie in der folgenden Abbildung gezeigt:

 [![](images/createsegue.png "Eine blauen Konnektor angezeigt, der angibt, wie in dieser Abbildung gezeigt, in dem die Segue führen")](images/createsegue.png#lightbox)

Auf MouseUp wird ein Menü angezeigt werden, lassen uns die Aktion für unsere Segue auswählen. Sie können die folgenden Bilder aussehen: 

**Erforderliche iOS 8 und Größe Klassen**:

[![](images/segue1.png "Die Aktion Segue Dropdownliste ohne Größe-Klassen")](images/segue1.png#lightbox)

**Bei Verwendung der Größenklassen und Adaptive Segues**:

[![](images/16new.png "Die Aktion Segue Dropdownliste mit Größe-Klassen")](images/16new.png#lightbox)

> [!IMPORTANT]
> Wenn Sie VMWare für Ihre virtuellen Windows-Computer verwenden, wird STRG + Klicken zugeordnet, als die _mit der rechten Maustaste_ los standardmäßig. Um eine Segue zu erstellen, bearbeiten Sie Ihre Voreinstellungen für die Tastatur über **Voreinstellungen** > **Tastatur und Maus** > **Mausaktionen** und ordnen Ihr **Sekundären Schaltfläche** wie unten gezeigt:
> 
> [![](images/image22.png "Tastatur und Maus-Voreinstellungen")](images/image22.png#lightbox)
> 
> Sie sollten jetzt eine Segue zwischen die View-Controller als normale hinzufügen können.

Es gibt verschiedene Typen von Übergängen, jede gewähren steuern, wie ein neuen Domänencontroller für die Sicht, die dem Benutzer angezeigt wird, und Interaktion mit anderen View-Controller in das Storyboard. Diese werden im folgenden erläutert. Es ist auch möglich, Unterklasse ein Segue-Objekt, das einen benutzerdefinierten Übergang zu implementieren:

-  **Anzeigen / Push** – ein Push segue Navigationsstapel die View-Controller hinzugefügt. Angenommen, dass der Ursprung des Push modellansichtcontroller Teil des gleichen Navigation Controllers als die View-Controller ist, die auf dem Stapel hinzugefügt wird. Dies bewirkt, dass dasselbe als `pushViewController` , und wird im Allgemeinen verwendet, wenn einige Beziehung zwischen den Daten auf den Bildschirmen vorhanden ist. Mithilfe der pushinstallationsmethode segue bietet Ihnen die Luxus, dass eine Navigationsleiste mit einer Schaltfläche "zurück" und der Titel hinzugefügt werden, zu den einzelnen Sichten im Stapel befindet, sodass Drilldown Navigation durch die Hierarchie anzeigen.
-  **Modale** – ein modales Segue Erstellen einer Beziehung zwischen jeder zwei View-Controller in Ihrem Projekt, mit der Option für eine animierte Übergang angezeigt wird. Der Domänencontroller der untergeordneten Ansicht wird den übergeordneten Ansicht Controller in den Anzeigebereich geschaltete vollständig verdecken. Im Gegensatz zu einem Push segue, wodurch eine zurück-Schaltfläche für uns hinzugefügt; Wenn eine modale mit segue `DismissViewController` muss verwendet werden, um zum vorherigen Ansicht Controller zurück.
-  **Benutzerdefinierte** – einem beliebigen benutzerdefinierten Segue kann erstellt werden, wenn eine Unterklasse von ` UIStoryboardSegue`.
-  **Entladen** – ein Entladung segue dienen zum Navigieren durch ein Push- oder modale segue – z. B. durch verwerfen den Controller Ansicht modal angezeigt. Darüber hinaus können Sie über nicht nur einen entladen, aber eine Reihe von "Push" und modale segues und wechseln Sie zurück, dass mehrere Schritte in Ihrer Navigationshierarchie mit einer einzelnen Aktion entladen. Um zu verstehen, wie segue eine Entladung in iOS, lesen Sie die [erstellen Segues entladen](https://developer.xamarin.com/recipes/ios/general/storyboard/unwind_segue/) Rezept.
-  **Sourceless** – eine sourceless Segue gibt an, der Szene haben, enthält die anfängliche Ansicht Controller und kann daher die anzeigen, dass des Benutzers zunächst angezeigt werden. Es wird durch die Segue unten dargestellt:  

    [![](images/sourcelesssegue.png "Eine sourceless segue")](images/sourcelesssegue.png#lightbox)

### <a name="adaptive-segue-types"></a>Adaptive Segue Typen

 iOS 8 eingeführte [Größenklassen](~/ios/user-interface/storyboards/unified-storyboards.md#size-classes) eine iOS-Storyboard-Datei zum Arbeiten mit allen verfügbaren Bildschirmgrößen Entwicklern das Erstellen einer Benutzeroberfläche für alle iOS-Geräte aktivieren zuzulassen. Standardmäßig werden alle neue Xamarin.iOS Anwendungen Größenklassen verwenden. Um ein älteres Projekt Größenklassen verwenden zu können, finden Sie in der [Einführung in Storyboards Unified](~/ios/user-interface/storyboards/unified-storyboards.md) Handbuch. 
 
Jede Anwendung, die mithilfe von Klassen Größe verwenden ebenfalls das neue [ *Adaptive Segues*](~/ios/user-interface/storyboards/unified-storyboards.md). Wenn Größenklassen verwenden möchten, denken Sie daran, dass wir Communications direkt angeben, werden nicht verwenden wir ein iPhone oder iPad. Wir werden also einer Benutzeroberfläche erstellen, die immer gleich, unabhängig davon, wie viel Immobilien sehen, die sie zur Bearbeitung. Adaptive Segues Arbeit Beurteilung der umgebungs, und legen Sie optimale Inhalt darstellen. Die Adaptive Segues sind unten aufgeführt: 

[![](images/adaptivesegue.png "Die Adaptive Segues-Dropdownliste")](images/adaptivesegue.png#lightbox)

|Segue|Beschreibung|
|--- |--- |
|Anzeigen|Dies ist vergleichbar mit dem ein Pushabonnement segue, führt aber den Inhalt des Bildschirms berücksichtigt.|
|Anzeigen von Details|Wenn die app eine Master- und Detailtabelle anzeigen (z. B. in einer geteilten Ansicht Controller auf einem iPad) angezeigt wird, wird der Inhalt die Detailansicht ersetzt. Wenn die app nur die Master oder Detail angezeigt wird, ersetzen den Inhalt der Anfang des Stapels Controller anzeigen.|
|Präsentation|Dies ähnelt der modale Segue und ermöglicht die Auswahl der Präsentation und Übergang-Formate.|
|Popover Präsentation|Dies stellt Inhalt dar, wie eine popover|

### <a name="transferring-data-with-segues"></a>Übertragen von Daten mit Segues

Die Vorteile von einem Segue enden keine Übergänge. Sie können auch verwendet werden, um die Übertragung von Daten zwischen Domänencontrollern Ansicht zu verwalten. Dies erfolgt durch Überschreiben der `PrepareForSegue` Methode für die anfängliche Ansicht Controller und die Handhabung der Daten selbst. Beim Auslösen der Segue – z. B. mit der Drücken einer Schaltfläche – die Anwendung wird rufen Sie diese Methode, einnahmemöglichkeiten vorbereiten den neuen Sicht Controller *vor* alle navigiert wird. Im nachfolgenden Code, von dem [Phoneword](https://developer.xamarin.com/samples/monotouch/Hello_iOS/) zugreifen können, wird dies veranschaulicht: 


```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, 
NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    var callHistoryContoller = segue.DestinationViewController 
                                  as CallHistoryController;

    if (callHistoryContoller != null) {
        callHistoryContoller.PhoneNumbers = PhoneNumbers;
    }
}
```

In diesem Beispiel wird die `PrepareForSegue` Methode wird aufgerufen, wenn die Segue vom Benutzer ausgelöst wird. Wir müssen zuerst eine Instanz des Controllers "empfangen" Ansicht erstellen, und legen Sie diese als Ziel für die Segue View Controller. Dies erfolgt durch die folgende Codezeile:

```csharp
var callHistoryContoller = segue.DestinationViewController as CallHistoryController;
```

Die Methode verfügt jetzt über die Möglichkeit zum Festlegen von Eigenschaften für die `DestinationViewController`. In diesem Beispiel haben wir diesen Vorteil durch Übergeben einer Liste aufgerufen ergriffen `PhoneNumbers` auf die `CallHistoryController` und ein Objekt mit demselben Namen zuweisen:

```csharp
if (callHistoryContoller != null) {
        callHistoryContoller.PhoneNumbers = PhoneNumbers;
    }
```

Sobald der Übergang abgeschlossen ist, sieht der Benutzer die `CallHistoryController` mit aufgefüllte Liste.

## <a name="adding-a-storyboard-to-a-non-storyboard-project"></a>Hinzufügen eines Storyboards zu einem nicht-Storyboard-Projekt

In manchen Fällen müssen Sie möglicherweise ein Storyboard einer zuvor nicht Storyboard-Datei hinzufügen. Einmal dies in Visual Studio für Mac kann optimiert werden, durch die folgenden Schritte aus:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Erstellen Sie eine neue Storyboarddatei zu suchen und **Datei > Neues Datei > iOS > Storyboard**, wie unten gezeigt: 
    
    [![](images/new-storyboard-xs.png "Das Dialogfeld "neue Datei"")](images/new-storyboard-xs.png#lightbox)

2. Fügen Sie Ihr Name Storyboard der **Hauptbenutzeroberfläche** Teil der **"Info.plist"**, wie unten dargestellt:
    
    [![](images/infoplist.png "Die Datei "Info.plist"-editor")](images/infoplist.png#lightbox)
    
    Dies ist die Entsprechung der Instanziierung der anfänglichen View-Controller in der `FinishedLaunching` Methode innerhalb der App-Delegat. Diese Option festgelegt ist, die Anwendung instanziiert ein Fenster (siehe unten), lädt die Haupt-Storyboard, und weist Sie eine Instanz von des Storyboards anfängliche-View-Controller (derjenige, der sich neben der sourceless Segue) als die `RootViewController` Eigenschaft des Fensters, und klicken Sie dann macht Das Fenster auf dem Bildschirm sichtbar ist.

3. In der `AppDelegate`, überschreiben die Standardeinstellung `Window` Methode mit den folgenden Code zum Implementieren des Window-Eigenschaft:
        
        public override UIWindow Window {
            get;
            set;
            }
            
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Erstellen Sie eine neue Storyboarddatei, indem Sie mit der rechten Maustaste auf das Projekt **hinzufügen > neuen Datei > iOS > leer Storyboard**, wie unten gezeigt: 
    
    [![](images/new-storyboard-vs.png "Das Dialogfeld "Neues Element"")](images/new-storyboard-vs.png#lightbox)

2. Fügen Sie Ihr Storyboard Name der **Hauptbenutzeroberfläche** Abschnitt des iOS-Anwendung, wie unten dargestellt:
    
    [![](images/ios-app.png "Die Datei "Info.plist"-editor")](images/ios-app.png#lightbox)
    
    Dies ist die Entsprechung der Instanziierung der anfänglichen View-Controller in der `FinishedLaunching` Methode innerhalb der App-Delegat. Diese Option festgelegt ist, die Anwendung instanziiert ein Fenster (siehe unten), lädt die Haupt-Storyboard, und weist Sie eine Instanz von des Storyboards anfängliche-View-Controller (derjenige, der sich neben der sourceless Segue) als die `RootViewController` Eigenschaft des Fensters, und klicken Sie dann macht Das Fenster auf dem Bildschirm sichtbar ist.

3. In der `AppDelegate`, überschreiben die Standardeinstellung `Window` Methode mit den folgenden Code zum Implementieren des Window-Eigenschaft:

        public override UIWindow Window {
            get;
            set;
            }
            
-----

## <a name="creating-a-storyboard-with-the-ios-designer"></a>Erstellen ein Storyboard mit dem iOS-Designer

Ein Storyboard kann erstellt werden, mit dem Xamarin-Designer für iOS, das nahtlos mit Visual Studio für Mac und Visual Studio integriert wurde.

Zunächst verwenden die iOS-Designer zum Erstellen von Storyboards, befolgen Sie die [Hello, iOS Multiscreen](~/ios/get-started/hello-ios-multiscreen/index.md) Handbuch. In dieser exemplarischen Vorgehensweise untersuchen Sie die Navigation zwischen View-Controller mit Segues und wie Ereignisse auf die Steuerelemente behandelt.

## <a name="instantiate-storyboards-manually"></a>Instanziieren Sie Storyboards manuell

Storyboards ersetzen völlig einzelne XIB-Dateien in Ihrem Projekt jedoch einzelne Ansicht Controller in einem Storyboard noch instanziiert werden können, mit `Storyboard.InstantiateViewController`.

Manchmal müssen Anwendungen besondere Anforderungen, die mit der integrierten Storyboard Übergängen, die vom Designer bereitgestellte behandelt werden können. Beispielsweise wäre eine Anwendung erstellen, die unterschiedliche Bildschirme über die gleiche Schaltfläche je nach den aktuellen Status einer Anwendung startet möchte wir die View-Controller manuell instanziieren und Programmieren des Übergangs uns.

Der folgende Screenshot zeigt, dass zwei View-Controller auf unserem Entwurfsoberfläche ohne dazwischen segue. Im nächste Abschnitt wird exemplarisch erklärt wie diesen Übergang übernehmen im Code eingerichtet werden kann.

 [![](images/viewcontrollerspink.png "Diese bildschirmabbildung zeigt, dass zwei Domänencontroller in der Ansicht auf der Entwurfsoberfläche ohne dazwischen segue")](images/viewcontrollerspink.png#lightbox)

1. Hinzufügen einer _leeren iPhone Storyboard_ zu einem vorhandenen Projekt Projekt:
    
    [![](images/add-storyboard1.png "Hinzufügen von storyboard")](images/add-storyboard1.png#lightbox)

2. Klicken Sie mit der Doppelklicken auf das neu erstellte Storyboard, um ihn zu öffnen, und fügen Sie einen neuen **Navigation Controller** auf die Entwurfsoberfläche. Entspricht dem Navigationsbereich Controller UI-weniger Daten aus, in der Standardeinstellung mit einem Stamm-View-Controller, stammen wird wie unten gezeigt:

    [![](images/uinavigationcontroller.png "View-Controller mit Segues")](images/uinavigationcontroller.png#lightbox)

3. Wählen Sie die _Modellansichtcontroller_ durch Klicken auf der schwarzen Leiste am unteren. In des Designers **Eigenschaft Pad**unter **Identität** können wir eine benutzerdefinierte Klasse als auch eine eindeutige ID für die View-Controller angeben. Legen Sie die **Klassenname** und **Storyboard-ID** auf `MainViewController`.

    [![](images/identitypanelnew.png "Geben Sie die benutzerdefinierte Klasse")](images/identitypanelnew.png#lightbox)

4. Später, müssen wir unsere View-Controller aus dem Storyboard instanziieren und verwenden Sie die Storyboard-ID wird im Code verweisen. Festlegen der Wiederherstellung ID entsprechend der Storyboard-ID wird sichergestellt, dass die modellansichtcontroller ordnungsgemäß neu erstellt Ruft, wenn der Zustand wiederhergestellt werden muss.

5. Wir haben derzeit nur einen View-Controller. Ziehen Sie eine andere Ansicht Controller auf der Entwurfsoberfläche aus. In der **Eigenschaft Pad**, unter der Identität, legen Sie die Klasse und das Storyboard-ID, `PinkViewController`, wie unten gezeigt:

    [![](images/pinkvcnew.png "Die Eigenschaft mit Leerstellen auffüllen")](images/pinkvcnew.png#lightbox)
    
    Die IDE erstellt diese benutzerdefinierte Klassen für die View-Controller. Diese können angezeigt werden, der **Lösung Pad**, wie im folgenden Screenshot gezeigt:
    
    [![](images/solution-pad.png "Lösung mit Leerstellen auffüllen")](images/solution-pad.png#lightbox)

6. In der `PinkViewController`, wählen Sie die Ansicht, indem Sie die Richtung der Mitte des Rahmens des Controllers auf. Ändern Sie in das Auffüllzeichen Eigenschaften unter Ansicht der **Hintergrund** zu Magenta:
    
    [![](images/pinkcontroller.png "Hintergrundfarbe festlegen")](images/pinkcontroller.png#lightbox)

7. Ziehen Sie schließlich eine Schaltfläche aus der **ToolBox** auf die `MainViewController`. In den Eigenschaften aufgefüllt, geben Sie ihm den Namen `PinkButton` und der Titel GoToPink, wie unten gezeigt:

    [![](images/pinkbutton.png "Name der Schaltfläche festlegen")](images/pinkbutton.png#lightbox)

Das Storyboard ist abgeschlossen, aber wenn wir jetzt das Projekt bereitstellen, erhalten wir ein leeres Fenster angezeigt. Hierfür müssen wir noch Teilen Sie die IDE unsere Storyboard verwenden, und klicken Sie zum Einrichten einer Stamm-modellansichtcontroller zur dienen als die erste Ansicht ist. Normalerweise kann dies über unsere Projektoptionen erfolgen wie oben gezeigt. In diesem Beispiel werden wir jedoch das gleiche Ergebnis im Code, durch das Hinzufügen der folgenden, erreicht die **AppDelegate**:

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

Die viel Code ist allerdings nur einige Zeilen nicht vertraut sind. Zuerst registrieren wir unsere Storyboard mit dem **AppDelegate** durch Übergeben des Storyboards-Namen **MainStoryboard**. Als Nächstes geben wir die Anwendung durch Aufrufen ein Controllers anfängliche Ansicht aus dem Storyboard instanziieren `InstantiateInitialViewController` auf unserem Storyboard und wir diese modellansichtcontroller als unsere Anwendung Stamm modellansichtcontroller festlegen. Diese Methode bestimmt den erste Bildschirm, der dem Benutzer angezeigt wird, und erstellt eine neue Instanz dieses Controllers anzeigen.

Beachten Sie, dass im Bereich Projektmappen, die die IDE erstellt hat eine `MainViewcontroller.cs` -Klasse, und die zugehörige `corresponding designer.cs` Wenn wir den Klassennamen für das Auffüllzeichen Eigenschaften in Schritt 4 hinzugefügt. Wir können sehen, dass diese Klasse erstellt einen speziellen Konstruktor, der eine Basisklasse enthält:

```csharp
public MainViewController (IntPtr handle) : base (handle) 
{
}
```


Wenn Sie ein Storyboard mit dem Designer zu erstellen, wird die IDE automatisch hinzufügen der [[registrieren]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) Attribut am oberen Rand der `designer.cs` -Klasse auf, und übergeben ein Zeichenfolgenbezeichner, die identisch mit der im angegebenen Storyboard-ID ist die vorherigen Schritt. Dadurch wird der C#-mit der relevanten Szene in das Storyboard verknüpfen.

Irgendwann empfiehlt eine vorhandene Klasse hinzufügen, die wurde **nicht** im Designer erstellt. In diesem Fall würden Sie diese Klasse als normale registrieren:

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

Der letzte Schritt in dieser Klasse ist, auf die Schaltfläche und die Umstellung auf die rosa modellansichtcontroller verknüpfen. Wir müssen Instanziieren der `PinkViewController` aus dem Storyboard; anschließend wir programmiert einen Push mit segue `PushViewController`, wie in den folgenden Beispielcode dargestellt:

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

Ausführen der Anwendung erzeugt eine 2-Bildschirm-Anwendung:

![](images/finishedstoryboard.png "Beispiel-app ausführen Bildschirme")

## <a name="conditional-segues"></a>Bedingte Segues

Häufig ist das Verschieben von einem View-Controller auf die nächste eine bestimmte Bedingung abhängig. Z. B. wenn wir einen einfache Anmeldebildschirm machen wurden würde nur möchten wir zum nächsten Bildschirm verschieben *Wenn* hatte den Benutzernamen und das Kennwort überprüft wurde.

Im nächsten Beispiel fügen wir ein Kennwortfeld zum oben genannten Beispiel. Der Benutzer wird nur in der Lage, den Zugriff auf die *PinkViewController* Wenn sie das richtige Kennwort eingeben, andernfalls ein Fehler angezeigt.

Bevor wir beginnen, führen Sie die Schritte 1 bis 8 oben. In den folgenden Schritten wir unsere Storyboard erstellen, beginnen Sie unsere UI erstellen und teilen Sie die View-Controller mit unserer App-Delegat RootViewController unverändert.

1. Nun wir unsere Benutzeroberfläche erstellen, und fügen Sie die zusätzlichen Ansichten aufgeführt die `MainViewController` , wie im folgenden Screenshot unübersichtlich, sind:

    - UITextField
        - Name: PasswordTextField
        - Platzhalter: "Geben Sie das Kennwort für den geheimen"
    - UILabel
        - Text: "Fehler: falsches Kennwort. Sie sind nicht erfolgreich! "
        - Farbe: Rot
        - Ausrichtung: Center
        - Linien: 2
        - 'Hidden' Kontrollkästchen aktiviert 
        
    [![](images/passwordvc.png "Center Zeilen")](images/passwordvc.png#lightbox)
    
2. Erstellen Sie eine Segue zwischen der Go rosa Schaltfläche und die modellansichtcontroller durch STRG Ziehen aus der *PinkButton* auf der *PinkViewController*, auswählen und **Push** auf Maus-nach-oben . 

3. Klicken Sie auf die Segue und weisen Sie ihm die *Bezeichner* `SegueToPink`:

    [![](images/namesegue.png "Klicken Sie auf die Segue und weisen Sie ihm den Bezeichner SegueToPink")](images/namesegue.png#lightbox)  
    

4. Abschließend fügen Sie die folgenden ShouldPerformSegue-Methode, die `MainViewController` Klasse:

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

In diesem Code haben wir die SegueIdentifier um abgeglichen unsere `SegueToPink` segue, sodass wir dann eine Bedingung zu testen, können in diesem Fall ein gültiges Kennwort. Wenn unsere Bedingung zurückgegeben werden, `true`, die Segue wird ausgeführt, und bietet die `PinkViewController`. Wenn `false`, neue View-Controller wird nicht angezeigt werden.

Wir können Segue auf diesem Controller Anzeigen dieser Ansatz anwenden, überprüft das SegueIdentifier-Argument für die ShouldPerformSegue-Methode. In diesem Fall haben wir nur eine Segue Bezeichner – `SegueToPink`.

Finden Sie in der Projektmappe Storyboards.Conditional in der [manuell Storyboards Beispiel](https://developer.xamarin.com/samples/monotouch/ManualStoryboard/) ein funktionsfähiges Beispiel.

<a name="Using-Storyboard-References" />

## <a name="using-storyboard-references"></a>Verwenden des Storyboard-Verweise

Ein Storyboard Verweis können Sie einen großen, komplexen Storyboard-Entwurf und teilen sie in kleinere Storyboards, die aus dem ursprünglichen referenziert abrufen, daher entfernen Entfernen von Komplexität und machen das resultierende einzelne Storyboards einfacher zu Entwurf und verwalten.

Darüber hinaus bieten ein Storyboard-Verweis ein _Anker_ in eine andere Szene innerhalb der gleichen Storyboard oder einer bestimmten Szene auf einen anderen.

<a name="Referencing-an-External-Storyboard" />

### <a name="referencing-an-external-storyboard"></a>Verweisen auf eine externe Storyboard

Um einen Verweis auf eine externe Storyboard hinzuzufügen, führen Sie folgende Schritte aus:

1. In der **Projektmappen-Explorer**mit der rechten Maustaste auf den Projektnamen, und wählen Sie **hinzufügen** > **neue Datei...**   >  **iOS** > **Storyboard**. Geben Sie einen **Namen** für das neue Storyboard und auf die **neu** Schaltfläche:
    
    [![](images/ref01.png "Das Dialogfeld "neue Datei"")](images/ref01.png#lightbox)
    
2. Entwerfen Sie das Layout der das neue Storyboard Szenen wie gewohnt würde und die Änderungen zu speichern: 
    
    [![](images/ref02.png "Das Layout der neuen Szene")](images/ref02.png#lightbox)
    
3. Öffnen Sie das Storyboard, die Sie den Verweis auf in der iOS-Designer hinzugefügt werden soll.

4. Ziehen Sie eine **Storyboard-Verweis** aus der **Toolbox** auf die Entwurfsoberfläche: 
    
    [![](images/ref03.png "Ein Storyboard-Verweis")](images/ref03.png#lightbox)
    
5. In der **Widget** auf der Registerkarte die **Eigenschaften-Explorer**, wählen Sie den Namen des der **Storyboard** , die Sie soeben erstellt haben: 

    [![](images/ref04.png "Die Registerkarte "Widget"")](images/ref04.png#lightbox)
    
6. STRG-Taste auf ein UI-Widget (z. B. eine Schaltfläche) auf einer vorhandenen Szene und erstellen Sie eine neue Segue auf die **Storyboard Verweis** , die Sie soeben erstellt haben: 

    [![](images/ref05.png "Erstellen eine segue")](images/ref05.png#lightbox) 
    
7. Wählen Sie im Popupmenü **anzeigen** der Segue abgeschlossen: 

    [![](images/ref06.png "Anzeigen der Segue abgeschlossen")](images/ref06.png#lightbox) 
    
8. Speichern Sie die Änderungen auf das Storyboard.

Wenn die app ausgeführt wird und der Benutzer auf dem UI-Element klickt, dass Sie die Segue aus der ersten View-Controller aus dem externen Storyboard in der Referenz zu Storyboard angegeben erstellt wird angezeigt.

<a name="Referencing-a-Specific-Scene-in-an-External-Storyboard" />

### <a name="referencing-a-specific-scene-in-an-external-storyboard"></a>Verweisen auf eine bestimmte Szene in einem externen Storyboard

Um einen Verweis auf eine bestimmte Szene hinzufügen wie einer externen Storyboard (und nicht die erste View Controller) folgt:

1. In der **Projektmappen-Explorer**, doppelklicken Sie auf das externe Storyboard um sie zur Bearbeitung zu öffnen.

2. Fügen Sie eine neue Szene hinzu und Entwerfen Sie des Layouts wie gewohnt verwenden: 

    [![](images/ref07.png "Das neue Layout der Szene")](images/ref07.png#lightbox)
    
3. In der **Widget** auf der Registerkarte die **Eigenschaften-Explorer**, geben Sie einen **Storyboard-ID** für der neuen Szene-View-Controller: 

    [![](images/ref08.png "Geben Sie ein Storyboard-ID für die neue Szenen-View-Controller")](images/ref08.png#lightbox)
    
3. Öffnen Sie das Storyboard, die Sie den Verweis auf in der iOS-Designer hinzugefügt werden soll.

4. Ziehen Sie eine **Storyboard-Verweis** aus der **Toolbox** auf die Entwurfsoberfläche: 

    [![](images/ref03.png "Ein Storyboard-Verweis")](images/ref03.png#lightbox)
    
5. In der **Widget** auf der Registerkarte die **Eigenschaften-Explorer**, wählen Sie den Namen des der **Storyboard** und die **Anwendungsverweis-ID** (Storyboard-ID) von der Szene, die Sie soeben erstellt haben: 

    [![](images/ref09.png "Die Registerkarte "Widget" ")](images/ref09.png#lightbox)
    
6. STRG-Taste auf ein UI-Widget (z. B. eine Schaltfläche) auf einer vorhandenen Szene und erstellen Sie eine neue Segue auf die **Storyboard Verweis** , die Sie soeben erstellt haben: 

    [![](images/ref10.png "Erstellen eine segue")](images/ref10.png#lightbox) 
    
7. Wählen Sie im Popupmenü **anzeigen** der Segue abgeschlossen: 

    [![](images/ref06.png "Anzeigen der Segue abgeschlossen")](images/ref06.png#lightbox) 
    
8. Speichern Sie die Änderungen auf das Storyboard.

Wenn die app ausführen und der Benutzer klickt auf das Element der Benutzeroberfläche, die Sie aus der Szene mit der Segue erstellt den angegebenen **Storyboard-ID** aus dem externen Storyboard in der Storyboard-Verweis angegeben wird angezeigt.

<a name="Referencing-a-Specific-Scene-in-the-Same-Storyboard" />

### <a name="referencing-a-specific-scene-in-the-same-storyboard"></a>Verweisen auf eine bestimmte Szene im gleichen Storyboard

Um einen Verweis auf eine bestimmte Szene das gleiche Storyboard hinzuzufügen, führen Sie folgende Schritte aus:

1. In der **Projektmappen-Explorer**, doppelklicken Sie auf das Storyboard, um sie zur Bearbeitung zu öffnen.

2. Fügen Sie eine neue Szene hinzu und Entwerfen Sie des Layouts wie gewohnt verwenden: 

    [![](images/ref11.png "Das neue Layout der Szene")](images/ref11.png#lightbox)

3. In der **Widget** auf der Registerkarte die **Eigenschaften-Explorer**, geben Sie einen **Storyboard-ID** für der neuen Szene-View-Controller: 

    [![](images/ref12.png "Die Registerkarte "Widget"")](images/ref12.png#lightbox)
    
3. Ziehen Sie eine **Storyboard-Verweis** aus der **Toolbox** auf die Entwurfsoberfläche: 

    [![](images/ref03.png "Ein Storyboard-Verweis")](images/ref03.png#lightbox)
    
5. In der **Widget** auf der Registerkarte die **Eigenschaften-Explorer**Option **Anwendungsverweis-ID** (Storyboard-ID) der Szene, die Sie soeben erstellt haben: 

    [![](images/ref13.png "Die Registerkarte "Widget"")](images/ref13.png#lightbox)
    
6. STRG-Taste auf ein UI-Widget (z. B. eine Schaltfläche) auf einer vorhandenen Szene und erstellen Sie eine neue Segue auf die **Storyboard Verweis** , die Sie soeben erstellt haben: 

    [![](images/ref14.png "Erstellen eine segue")](images/ref14.png#lightbox) 
    
7. Wählen Sie im Popupmenü **anzeigen** der Segue abgeschlossen: 

    [![](images/ref06.png "Anzeigen der Segue abgeschlossen")](images/ref06.png#lightbox) 
    
8. Speichern Sie die Änderungen auf das Storyboard.

Wenn die app ausführen und der Benutzer klickt auf das Element der Benutzeroberfläche, die Sie aus der Szene mit der Segue erstellt den angegebenen **Storyboard-ID** im gleichen Storyboard in der Storyboard-Verweis angegeben wird angezeigt.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel führt das Konzept des Storyboards und wie sie bei der Entwicklung von iOS-Anwendungen nützlich sein können. Es wird erläutert, Szenen, View-Controller, Ansichten und Ansicht Hierarchien und wie Szenen zusammen mit anderen Typen von Segues verknüpft sind.  Es wird erklärt, auch instanziieren View-Controller manuell von einem Storyboard und Erstellen von bedingten Segues.



## <a name="related-links"></a>Verwandte Links

- [Manuelle Storyboard (Beispiel)](https://developer.xamarin.com/samples/ManualStoryboard/)
- [Einführung in die iOS-Designer](~/ios/user-interface/designer/introduction.md)
- [Konvertieren in Storyboards](http://developer.apple.com/library/ios/#releasenotes/Miscellaneous/RN-AdoptingStoryboards/)
- [UIStoryboard-Klassenreferenz](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UIStoryboard_Class/Reference/Reference.html)
