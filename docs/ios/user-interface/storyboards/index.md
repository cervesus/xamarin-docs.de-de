---
title: Einführung in Storyboards in xamarin. IOS
description: Dieses Dokument bietet eine Einführung in Storyboards in xamarin. IOS. Es wird beschrieben, wie ein Storyboard verwendet wird, um eine Benutzeroberfläche, Segues und die Verwendung des IOS-Designers zum Bearbeiten von Storyboard-Dateien zu definieren.
ms.prod: xamarin
ms.assetid: A3339BD2-9F56-7965-25F5-4B7C991EB775
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 13f5c594543934e14295615517e3de01a98a69a5
ms.sourcegitcommit: 52fb214c0e0243587d4e9ad9306b75e92a8cc8b7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2020
ms.locfileid: "76940996"
---
# <a name="introduction-to-storyboards-in-xamarinios"></a>Einführung in Storyboards in xamarin. IOS

In diesem Handbuch wird erläutert, worum es sich bei einem Storyboard handelt, und es werden einige der wichtigsten Komponenten erläutert – z. b. "log". Wir sehen uns an, wie Storyboards erstellt und verwendet werden können und welche Vorteile Sie für einen Entwickler haben.

Bevor das Storyboard-Dateiformat von Apple als visuelle Darstellung der Benutzeroberfläche einer IOS-Anwendung eingeführt wurde, haben Entwickler XIb-Dateien für jeden Ansichts Controller erstellt und die Navigation zwischen den einzelnen Ansichten manuell programmiert.  Mithilfe eines Storyboards können Entwickler sowohl Ansichts Controller als auch die Navigation zwischen Ihnen auf einer Entwurfs Oberfläche definieren und die WYSIWYG-Bearbeitung der Benutzeroberfläche der Anwendung ermöglichen.

Ein Storyboard kann mit dem xamarin IOS-Designer erstellt, geöffnet und bearbeitet werden. In dieser Anleitung wird auch erläutert, wie Sie den Designer verwenden, um Ihre Storyboards zu C# erstellen, während Sie zum Programmieren der Navigation verwenden.

## <a name="requirements"></a>-Anforderungen

Storyboards können mit Xcode, dem IOS-Designer in Visual Studio für Mac und Visual Studio 2019 mit installierter xamarin-Workloads verwendet werden.

## <a name="what-is-a-storyboard"></a>Was ist ein Storyboard?

Ein Storyboard ist die visuelle Darstellung aller Bildschirme in einer Anwendung. Sie enthält eine Abfolge von Szenen, wobei jede Szene einen *Ansichts Controller* und seine *Ansichten*darstellt. Diese Sichten können Objekte und Steuer [Elemente](~/ios/user-interface/controls/index.md) enthalten, die dem Benutzer die Interaktion mit Ihrer Anwendung ermöglichen. Diese Auflistung von Sichten und Steuerelementen (oder *unter Ansichten*) wird als *Hierarchie der Inhaltsansicht*bezeichnet. Szenen sind durch Segue-Objekte verbunden, die einen Übergang zwischen Ansichts Controllern darstellen. Dies wird normalerweise erreicht, indem ein segue zwischen einem Objekt in der Anfangs Ansicht und der Verbindungs Ansicht erstellt wird. Die Beziehungen auf der Entwurfs Oberfläche werden in der folgenden Abbildung veranschaulicht:

 [![](images/storyboardsview.png "The relationships on the design surface are illustrated in this image")](images/storyboardsview.png#lightbox)

Wie gezeigt, erstellt das Storyboard jeden Ihrer Szenen mit Inhalt, der bereits gerendert wird, und veranschaulicht die Verbindungen zwischen Ihnen.  An dieser Stelle ist zu beachten, dass bei Szenen auf einem iPhone sicher angenommen werden kann, dass eine *Szene* auf dem Storyboard einem *Bildschirm* mit Inhalt auf dem Gerät entspricht. Bei einem iPad kann es jedoch vorkommen, dass mehrere Szenen gleichzeitig angezeigt werden – z. b. mit einem popover-Ansichts Controller.

Es gibt viele Vorteile bei der Verwendung von Storyboards zum Erstellen der Benutzeroberfläche Ihrer Anwendung, insbesondere bei der Verwendung von xamarin. Erstens handelt es sich um eine visuelle Darstellung der Benutzeroberfläche, da alle Objekte – einschließlich [benutzerdefinierter Steuerelemente](~/ios/user-interface/designer/ios-designable-controls-overview.md) – zur Entwurfszeit gerendert werden.
Dies bedeutet, dass Sie vor dem entwickeln oder Bereitstellen Ihrer Anwendung das Aussehen und den Flow visualisieren können. Nehmen Sie z. b. das obige Bild. Wir können von einem kurzen Blick auf die Entwurfs Oberfläche erfahren, wie viele Szenen vorhanden sind, welches Layout jede Ansicht und wie alles verknüpft ist. Dadurch sind Storyboards so leistungsfähig.

Ereignisse können mit Storyboards besser verwaltet werden, insbesondere bei Verwendung des IOS-Designers. Die meisten Steuerelemente der Benutzeroberfläche verfügen über eine Liste möglicher Ereignisse in der Eigenschaftenpad. Der Ereignishandler kann hier hinzugefügt und in einer partiellen Methode in der Ansichts Controller Klasse abgeschlossen werden.

Der Inhalt eines Storyboards wird als XML-Datei gespeichert. Zum Zeitpunkt der Erstellung werden alle `.storyboard` Dateien in Binärdateien kompiliert, die als NISB bezeichnet werden. Zur Laufzeit werden diese NISB initialisiert und instanziiert, um neue Sichten zu erstellen.

## <a name="segues"></a>Segues

Ein *segue*-oder *segue-Objekt*wird in der IOS-Entwicklung verwendet, um einen Übergang zwischen Szenen darzustellen. Halten Sie die **STRG** -Taste gedrückt, und klicken Sie auf von einer Szene in eine andere, um einen "*" zu erstellen. Wenn wir die Maus ziehen, wird ein blauer Connector angezeigt, der angibt, wo der segue (wie in der folgenden Abbildung gezeigt) zu sehen ist:

 [![](images/createsegue.png "A blue connector appears, indicating where the segue will lead as demonstrated in this image")](images/createsegue.png#lightbox)

Bei einem Mausklick wird ein Menü angezeigt, in dem wir die Aktion für den segue auswählen können. Es kann etwa wie folgt aussehen:

**Klassen vor IOS 8 und Größe**:

[![](images/segue1.png "The Action Segue dropdown without Size Classes")](images/segue1.png#lightbox)

**Bei der Verwendung von Größenklassen und adaptiven Klassen**:

[![](images/16new.png "The Action Segue dropdown with Size Classes")](images/16new.png#lightbox)

> [!IMPORTANT]
> Wenn Sie VMware für Ihren virtuellen Windows-Computer verwenden, wird der Strg-Klick standardmäßig mit der _rechten Maustaste_ angezeigt. Um einen segue zu erstellen, bearbeiten Sie Ihre Tastatur Einstellungen über **Einstellungen** > **Tastatur & Maus** - > **Maus** Verknüpfungen, und ordnen Sie die **sekundäre Schaltfläche** neu zu, wie unten dargestellt:
>
> [![](images/image22.png "Keyboard and Mouse preference settings")](images/image22.png#lightbox)
>
> Sie sollten jetzt in der Lage sein, einen "seegue" zwischen den Ansichts Controllern als normal hinzuzufügen.

Es gibt verschiedene Arten von Übergängen, die jeweils steuern, wie ein neuer Ansichts Controller dem Benutzer präsentiert wird und wie er mit anderen Ansichts Controllern im Storyboard interagiert. Diese werden unten erläutert. Es ist auch möglich, ein segue-Objekt zu unterteilen, um einen benutzerdefinierten Übergang zu implementieren:

- **Anzeigen/pushen** – ein Push-Ziel fügt dem Navigations Stapel den Ansichts Controller hinzu. Dabei wird davon ausgegangen, dass der Ansichts Controller, von dem der Push stammt, Teil desselben Navigations Controllers wie der Ansichts Controller ist, der dem Stapel hinzugefügt wird. Dies erfolgt genauso wie `pushViewController`, und wird im Allgemeinen verwendet, wenn zwischen den Daten auf den Bildschirmen eine Beziehung besteht. Die Verwendung von Push-abgue bietet Ihnen den Luxus, eine Navigationsleiste mit einer Schaltfläche "zurück" und einem Titel zu jeder Ansicht auf dem Stapel hinzuzufügen, sodass drilldownnavigation durch die Ansichts Hierarchie ermöglicht wird.
- **Modal** – ein modaler segue erstellt eine Beziehung zwischen zwei beliebigen Ansichts Controllern in Ihrem Projekt, wobei die Option eines animierten Übergangs angezeigt wird. Der untergeordnete Ansichts Controller verwirft den übergeordneten Ansichts Controller in der Ansicht vollständig. Im Gegensatz zu einem Push-abgue, der eine Schaltfläche "zurück" für uns hinzufügt. bei der Verwendung eines modalen seegue-`DismissViewController` muss verwendet werden, um zum vorherigen Ansichts Controller zurückzukehren.
- **Benutzer** definiert – alle benutzerdefinierten Klassen können als eine Unterklasse von `UIStoryboardSegue`erstellt werden.
- Entladen **– ein** Entlade-/zusicherungstyp kann verwendet werden, um durch einen Push-oder modalen Status zurückzukehren – z. b. durch Verwerfen des modalen dargestellten Ansichts Controllers. Darüber hinaus können Sie nicht nur einen, sondern auch eine Reihe von Push-und modalen aufrufen und mehrere Schritte in der Navigations Hierarchie mit einer einzelnen Entlade Aktion zurückgehen. Um zu erfahren, wie Sie eine Entlade Entladung in ios verwenden, lesen Sie die Anleitung zum Erstellen von Entlade [Gründen](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/storyboard/unwind_segue) .
- **Sourbstbston** – ein Ressourcen loser Wert gibt die Szene an, die den anfänglichen Ansichts Controller enthält, und daher die Ansicht, die der Benutzer zuerst sehen wird. Sie wird durch den unten gezeigten-Abschnitt dargestellt:  

    [![](images/sourcelesssegue.png "A sourceless segue")](images/sourcelesssegue.png#lightbox)

### <a name="adaptive-segue-types"></a>Adaptive Enumerationstypen

 IOS 8 hat [Größenklassen](~/ios/user-interface/storyboards/unified-storyboards.md#size-classes) eingeführt, damit eine IOS-storyboarddatei mit allen verfügbaren Bildschirmgrößen funktioniert, sodass Entwickler eine Benutzeroberfläche für alle IOS-Geräte erstellen können. Standardmäßig werden in allen neuen xamarin. IOS-Anwendungen Größenklassen verwendet. Informationen zur Verwendung von Größenklassen aus einem älteren Projekt finden Sie im Handbuch [Introduction to Unified Storyboards (Einführung in Unified Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md) ).

Jede Anwendung, die Größenklassen verwendet, verwendet auch die neuen [*adaptiven Segues*](~/ios/user-interface/storyboards/unified-storyboards.md). Beachten Sie bei der Verwendung von Größenklassen, dass wir das Wetter nicht direkt angeben, dass wir ein iPhone oder iPad verwenden. Anders ausgedrückt: Wir erstellen eine Benutzeroberfläche, die immer gleich aussieht, unabhängig davon, wie viel tatsächlich Sie verwendet werden muss. Adaptive Kalender arbeiten daran, die Umgebung zu beurteilen und zu bestimmen, wie Inhalte am besten präsentiert werden. Die adaptiven-Sekunden sind unten dargestellt:

[![](images/adaptivesegue.png "The Adaptive Segues dropdown")](images/adaptivesegue.png#lightbox)

|Segue|Beschreibung|
|--- |--- |
|Anzeigen|Dies ähnelt einem Push-Vorgang, aber der Inhalt des Bildschirms wird berücksichtigt.|
|Details anzeigen|Wenn die APP eine Master-und Detailansicht anzeigt (z. b. in einem Split View Controller auf einem iPad), ersetzt der Inhalt die Detailansicht. Wenn die app nur den Master oder das Detail anzeigt, ersetzt der Inhalt den Anfang des Ansichts Controller Stapels.|
|Präsentation|Dies ähnelt dem modalen segue und ermöglicht die Auswahl von Präsentations-und Übergangs Stilen.|
|Popover-Präsentation|Dies stellt Inhalte als popover dar.|

### <a name="transferring-data-with-segues"></a>Übertragen von Daten mit einem anderen

Die Vorteile einer-Tabelle enden nicht mit Übergängen. Sie können auch verwendet werden, um die Übertragung von Daten zwischen Ansichts Controllern zu verwalten. Dies wird erreicht, indem die `PrepareForSegue`-Methode auf dem anfänglichen Ansichts Controller überschrieben und die Daten selbst verarbeitet werden. Wenn der segue ausgelöst wird – z. b. durch Drücken der Schaltfläche –, ruft die Anwendung diese Methode auf und bietet die Möglichkeit, den neuen Ansichts Controller *vor* der Navigation vorzubereiten. Der folgende Code aus dem [phoneword](https://docs.microsoft.com/samples/xamarin/ios-samples/hello-ios) -Beispiel veranschaulicht dies:

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

In diesem Beispiel wird die `PrepareForSegue`-Methode aufgerufen, wenn die-Methode vom Benutzer ausgelöst wird. Zuerst müssen wir eine Instanz des "empfangenden" Ansichts Controllers erstellen und diese als Ziel Ansichts Controller des segue festlegen. Dies erfolgt mithilfe der folgenden Codezeile:

```csharp
var callHistoryController = segue.DestinationViewController as CallHistoryController;
```

Die-Methode verfügt jetzt über die Möglichkeit, Eigenschaften für die `DestinationViewController`festzulegen. In diesem Beispiel haben wir dies genutzt, indem wir eine Liste mit dem Namen `PhoneNumbers` an die `CallHistoryController` übergeben und Sie einem Objekt mit demselben Namen zuweisen:

```csharp
if (callHistoryController != null) {
        callHistoryController.PhoneNumbers = PhoneNumbers;
    }
```

Sobald der Übergang abgeschlossen ist, wird dem Benutzer die `CallHistoryController` mit der Liste aufgefüllt angezeigt.

## <a name="adding-a-storyboard-to-a-non-storyboard-project"></a>Hinzufügen eines Storyboards zu einem nicht-Storyboard-Projekt

Gelegentlich müssen Sie möglicherweise ein Storyboard zu einer zuvor nicht-Storyboard-Datei hinzufügen. Wenn Sie dies in Visual Studio für Mac ausführen, können Sie die folgenden Schritte ausführen:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Erstellen Sie eine neue Storyboard-Datei, indem Sie zu **Datei > neue Datei > IOS > Storyboard**navigieren, wie unten dargestellt:

    [![](images/new-storyboard-xs.png "The new file dialog")](images/new-storyboard-xs.png#lightbox)

2. Fügen Sie den Storyboardnamen zum **Haupt Schnittstellen** Abschnitt der Datei " **Info. plist**" hinzu, wie unten dargestellt:

    [![](images/infoplist.png "The Info.plist editor")](images/infoplist.png#lightbox)

    Dies entspricht der Instanziierung des anfänglichen Ansichts Controllers in der `FinishedLaunching`-Methode innerhalb des App-Delegaten. Wenn diese Option festgelegt ist, instanziiert die Anwendung ein Fenster (siehe unten), lädt das Haupt-Storyboard und weist eine Instanz des anfänglichen Ansichts Controllers des Storyboards (der neben dem sourloads-segue) als `RootViewController`-Eigenschaft des Fensters auf. Anschließend wird das Fenster auf dem Bildschirm sichtbar.

3. Überschreiben Sie im `AppDelegate`die standardmäßige `Window`-Methode mit folgendem Code, um die Window-Eigenschaft zu implementieren:

    ```csharp
    public override UIWindow Window {
        get;
        set;
    }
    ```

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Erstellen Sie eine neue Storyboard-Datei, indem Sie mit der rechten Maustaste auf das Projekt klicken, um **> neue Datei > IOS > leeres Storyboard hinzuzufügen**, wie unten dargestellt:

    [![](images/new-storyboard-vs.png "The new item dialog")](images/new-storyboard-vs.png#lightbox)

2. Fügen Sie den Storyboardnamen zum **Haupt Schnittstellen** Abschnitt der IOS-Anwendung hinzu, wie unten dargestellt:

    [![](images/ios-app.png "The Info.plist editor")](images/ios-app.png#lightbox)

    Dies entspricht der Instanziierung des anfänglichen Ansichts Controllers in der `FinishedLaunching`-Methode innerhalb des App-Delegaten. Wenn diese Option festgelegt ist, instanziiert die Anwendung ein Fenster (siehe unten), lädt das Haupt-Storyboard und weist eine Instanz des anfänglichen Ansichts Controllers des Storyboards (der neben dem sourloads-segue) als `RootViewController`-Eigenschaft des Fensters auf. Anschließend wird das Fenster auf dem Bildschirm sichtbar.

3. Überschreiben Sie im `AppDelegate`die standardmäßige `Window`-Methode mit folgendem Code, um die Window-Eigenschaft zu implementieren:

    ```csharp
    public override UIWindow Window {
        get;
        set;
    }
    ```

-----

## <a name="creating-a-storyboard-with-xcode"></a>Erstellen eines Storyboards mit Xcode

Ein Storyboard kann mithilfe von Xcode erstellt und geändert werden, um Sie in ihren IOS-apps zu verwenden, die mit Visual Studio für Mac entwickelt wurden.

Storyboards ersetzen vollständig einzelne XIb-Dateien in Ihrem Projekt, jedoch können einzelne Ansichts Controller in einem Storyboard weiterhin mithilfe von `Storyboard.InstantiateViewController`instanziiert werden.

Manchmal haben Anwendungen besondere Anforderungen, die nicht mit den integrierten Storyboard-Übergängen, die vom Designer bereitgestellt werden, verarbeitet werden können. Wenn Sie z. b. eine Anwendung erstellen, die unterschiedliche Bildschirme von derselben Schaltfläche aus aufruft, empfiehlt es sich, die Ansichts Controller in Abhängigkeit vom aktuellen Status einer Anwendung manuell zu instanziieren und den Übergang selbst zu programmieren.

Der folgende Screenshot zeigt zwei Ansichts Controller auf unserer Entwurfs Oberfläche ohne einen anderen. Im nächsten Abschnitt wird erläutert, wie dieser Übergang im Code eingerichtet werden kann.

1. Fügen Sie einem vorhandenen Projekt Projekt ein _leeres iPhone-Storyboard_ hinzu:

    [![](images/add-storyboard2.png "Adding storyboard")](images/add-storyboard2.png#lightbox)

2. Klicken Sie mit der rechten Maustaste auf die storyboarddatei, und wählen Sie **Öffnen mit > Xcode Interface Builder** aus, um Sie in Xcode zu öffnen

3. Öffnen Sie in Xcode die Bibliothek (über **Ansicht > Bibliothek anzeigen** oder *Umschalt + Befehl + L*), um eine Liste von Objekten anzuzeigen, die dem Storyboard hinzugefügt werden können. Fügen Sie dem Storyboard eine `Navigation Controller` hinzu, indem Sie das Objekt aus der Liste auf das Storyboard ziehen. Standardmäßig werden die `Navigation Controller` zwei Bildschirme bereitstellen: der Bildschirm auf der rechten Seite ist eine `TableViewController` die wir durch eine einfachere Ansicht ersetzen werden, sodass Sie durch Klicken auf die Ansicht und drücken der ENTF-Taste entfernt werden kann.

    [![](images/add-navigation-controller.png "Adding a NavigationController from the Library")](images/add-navigation-controller.png#lightbox)

4. Dieser Ansichts Controller verfügt über eine eigene benutzerdefinierte Klasse und benötigt auch eine eigene Storyboard-ID. Wenn Sie auf das Kontrollkästchen oberhalb dieser neu hinzugefügten Ansicht klicken, werden drei Symbole angezeigt, von denen der linke Teil den Ansichts Controller für die Ansicht darstellt. Wenn Sie dieses Symbol auswählen, können Sie die Klassen-und ID-Werte auf der Registerkarte Identität des rechten Bereichs festlegen. Legen Sie diese Werte auf `MainViewController` fest, und stellen Sie sicher, dass Sie `Use Storyboard ID`überprüfen.

    [![](images/identity-panel.png "Setting the MainViewController in the identity panel")](images/identity-panel.png#lightbox)

5. Wenn Sie die Bibliothek erneut verwenden, ziehen Sie einen Ansichts Controller auf den Bildschirm. Diese wird als root View Controller festgelegt. Wenn Sie die Steuertaste gedrückt halten, klicken und ziehen Sie vom Navigations Controller auf der linken Seite auf den neu hinzugefügten Ansichts Controller auf der rechten Seite, und klicken Sie im Menü auf *root View Controller* .

    [![](images/add-view-controller.png "Adding a NavigationController from the Library and setting the MainViewController as a Root View Controller")](images/add-view-controller.png#lightbox)

6. Diese APP wird zu einer anderen Ansicht navigiert. Fügen Sie dem Storyboard also genau wie zuvor eine weitere Ansicht hinzu. Dies wird als `PinkViewController`bezeichnet, und diese Werte können auf die gleiche Weise wie bei der `MainViewController`festgelegt werden.

    [![](images/add-additional-view-controller.png "Adding an additional View Controller")](images/add-additional-view-controller.png#lightbox)

7. Da der Ansichts Controller einen rosa Hintergrund hat, kann diese Eigenschaft mithilfe der Dropdown Liste neben `Background`im Bereich Attribute festgelegt werden.

    [![](images/set-pink-background.png "Adding an additional View Controller")](images/set-pink-background.png#lightbox)

8. Da das `MainViewController` zum `PinkViewController`navigieren soll, benötigt das erste eine Schaltfläche für die Interaktion mit. Mithilfe der Bibliothek können wir dem `MainViewController`eine Schaltfläche hinzufügen.

    [![](images/add-button.png "Adding a Button to the MainViewController")](images/add-button.png#lightbox)

Das Storyboard ist vollständig, aber wenn wir das Projekt jetzt bereitstellen, wird ein leerer Bildschirm angezeigt. Das liegt daran, dass wir die IDE weiterhin auffordern müssen, unser Storyboard zu verwenden, und einen Stamm Ansichts Controller einrichten, der als erste Ansicht fungiert. Normalerweise kann dies über unsere Projektoptionen erfolgen, wie oben gezeigt. In diesem Beispiel erzielen wir jedoch dasselbe Ergebnis im Code, indem wir Folgendes zum **appdelegaten**hinzufügen:

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
        window.AddSubview(initialViewController.View);
        window.MakeKeyAndVisible ();
        return true;
    }
}
```

Das ist viel Code, aber nur ein paar Zeilen sind nicht vertraut. Zuerst registrieren wir unser Storyboard beim **appdelegaten** , indem wir den Namen des Storyboards, **mainstoryboard**, übergeben. Als Nächstes weisen Sie der Anwendung an, einen anfänglichen Ansichts Controller aus dem Storyboard zu instanziieren, indem Sie `InstantiateInitialViewController` im Storyboard aufrufen und diesen Ansichts Controller als root View Controller der Anwendung festlegen. Diese Methode bestimmt den ersten Bildschirm, den der Benutzer sieht, und erstellt eine neue Instanz dieses Ansichts Controllers.

Beachten Sie im Lösungs Bereich, dass die IDE eine `MainViewcontroller.cs` Klasse erstellt hat, und ihre `corresponding designer.cs`, als wir dem Eigenschaftenpad in Schritt 4 den Klassennamen hinzugefügt haben. Wir können sehen, dass diese Klasse einen speziellen Konstruktor erstellt hat, der eine Basisklasse enthält:

```csharp
public MainViewController (IntPtr handle) : base (handle)
{
}
```

Wenn Sie ein Storyboard mithilfe von Xcode erstellen, fügt die IDE automatisch das Attribut [[Register]](xref:Foundation.RegisterAttribute) am Anfang der `designer.cs` Klasse hinzu und übergibt einen Zeichen folgen Bezeichner, der mit der im vorherigen Schritt angegebenen Storyboard-ID identisch ist. Dadurch wird der C# mit der relevanten Szene im Storyboard verknüpft.

```csharp
[Register ("MainViewController")]
public partial class MainViewController : UIViewController
{
    public MainViewController (IntPtr handle) : base (handle)
    {
    }
    //...
}
```

Weitere Informationen zum Registrieren von Klassen und Methoden finden Sie in der Dokumentation zur [typregistrierungs](https://docs.microsoft.com/xamarin/ios/internals/registrar) Stelle.

Der letzte Schritt in dieser Klasse besteht darin, die Schaltfläche und den Übergang zum Rosa Ansichts Controller zu übertragen. Wir werden die `PinkViewController` aus dem Storyboard instanziieren. Anschließend wird ein Push-segue mit `PushViewController`programmiert, wie im folgenden Beispielcode veranschaulicht:

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

    public void Initialize()
    {
        //Instantiating View Controller with Storyboard ID 'PinkViewController'
        pinkViewController = Storyboard.InstantiateViewController ("PinkViewController") as PinkViewController;
    }

    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();

        //When we push the button, we will push the pinkViewController onto our current Navigation Stack
        PinkButton.TouchUpInside += (o, e) =&gt;
        {
            this.NavigationController.PushViewController (pinkViewController, true);
        };
    }
}
```

Beim Ausführen der Anwendung wird eine Anwendung mit zwei Bildschirmen erzeugt:

![](images/finishedstoryboard.png "Sample app run screens")

## <a name="conditional-segues"></a>Bedingte Sekunden

Häufig ist der Wechsel von einem Ansichts Controller zum nächsten auf eine bestimmte Bedingung angewiesen. Wenn wir beispielsweise einen einfachen Anmeldebildschirm erstellen, möchten wir nur zum nächsten Bildschirm wechseln, *Wenn* der Benutzername und das Kennwort überprüft wurden.

Im nächsten Beispiel fügen wir dem obigen Beispiel ein Kenn Wortfeld hinzu. Der Benutzer kann nur auf den *pinkviewcontroller* zugreifen, wenn er das richtige Kennwort eingegeben hat; andernfalls wird ein Fehler angezeigt.

Bevor wir beginnen, befolgen Sie die obigen Schritte 1 – 8. In den folgenden Schritten erstellen wir unser Storyboard, erstellen die Benutzeroberfläche und teilen dem App-Delegaten mit, welcher Controller als rootviewcontroller verwendet werden soll.

1. Nun erstellen wir unsere Benutzeroberfläche und fügen die zusätzlichen Ansichten hinzu, die dem `MainViewController` aufgelistet werden, damit Sie wie im folgenden Screenshot aussehen:

    - UITextField
        - Name: passwordtextfield
        - Platzhalter: "geben Sie das geheime Kennwort ein"
    - UILabel
        - Text: "Error: falsches Kennwort. Sie dürfen! ' nicht übergeben.
        - Farbe: rot
        - Ausrichtung: zentrieren
        - Zeilen: 2
        - Kontrollkästchen ' ausgeblendet ' aktiviert    

    [![](images/passwordvc.png "Center Lines")](images/passwordvc.png#lightbox)

2. Erstellen Sie einen segue zwischen der Schaltfläche Gehe zu Rosa und dem Ansichts Controller, indem Sie von der *PinkButton-Taste* auf den *pinkviewcontroller*ziehen und bei der Maus auf **Push drücken** .

3. Klicken Sie auf die Datei "*", und legen Sie Ihr den *Bezeichner* `SegueToPink`:

    [![](images/namesegue.png "Click on the Segue and give it the Identifier SegueToPink")](images/namesegue.png#lightbox)  

4. Fügen Sie schließlich der `MainViewController`-Klasse die folgende "dendperformabgue"-Methode hinzu:

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

In diesem Code haben wir den segueidentifier mit unserem `SegueToPink` segue abgeglichen, damit wir eine Bedingung testen können. ein gültiges Kennwort in diesem Fall. Wenn unsere Bedingung `true`zurückgibt, wird der-Vorgang durchgeführt, und der `PinkViewController`wird angezeigt. Wenn `false`, wird der neue Ansichts Controller nicht angezeigt.

Wir können diesen Ansatz auf jeden beliebigen Eintrag auf diesem Ansichts Controller anwenden, indem wir das Argument "sgueidentifier" auf die Methode "verweidperformtengue" überprüfen. In diesem Fall verfügen wir nur über einen "–"--`SegueToPink`.

Ein funktionierendes Beispiel finden Sie in der Projekt Mappe "Storyboards. Conditional" im [Beispiel "manuelle Storyboards](https://docs.microsoft.com/samples/xamarin/ios-samples/manualstoryboard) ".

<a name="Using-Storyboard-References" />

## <a name="using-storyboard-references"></a>Verwenden von Storyboard-verweisen

Mithilfe eines storyboardverweises können Sie einen großen, komplexen Storyboard-Entwurf erstellen und in kleinere Storyboards unterteilen, auf die von der ursprünglichen verwiesen wird. Dadurch können Sie die Komplexität entfernen und die resultierenden einzelnen Storyboards leichter entwerfen und warten.

Außerdem kann eine storyboardreferenz einen _Anker_ für eine andere Szene innerhalb desselben Storyboards oder eine bestimmte Szene auf einer anderen Szene bereitstellen.

<a name="Referencing-an-External-Storyboard" />

### <a name="referencing-an-external-storyboard"></a>Verweisen auf ein externes Storyboard

Gehen Sie folgendermaßen vor, um einen Verweis auf ein externes Storyboard hinzuzufügen:

1. Klicken Sie **im Projektmappen-Explorer**mit der rechten Maustaste auf den Projektnamen, und wählen Sie > **neue Datei** **Hinzufügen** ... > **IOS** > **Storyboard**aus. Geben Sie einen **Namen** für das neue Storyboard ein, und klicken Sie auf die Schaltfläche **neu** :

    [![](images/ref01.png "The New File Dialog")](images/ref01.png#lightbox)

2. Entwerfen Sie das Layout der neuen Storyboard-Szenen wie gewohnt, und speichern Sie die Änderungen:

    [![](images/ref02.png "The layout of the new scene")](images/ref02.png#lightbox)

3. Öffnen Sie das Storyboard, dem Sie den Verweis hinzufügen möchten, im IOS-Designer.

4. Ziehen Sie einen **storyboardverweis** aus der **Toolbox** auf den Designoberfläche:

    [![](images/ref03.png "A Storyboard Reference")](images/ref03.png#lightbox)

5. Wählen Sie im **Eigenschaften-Explorer**auf der Registerkarte **Widget** den Namen des **Storyboards** aus, das Sie oben erstellt haben:

    [![](images/ref04.png "The Widget tab")](images/ref04.png#lightbox)

6. Klicken Sie mit der Maus auf ein UI-Widget (z. b. eine Schaltfläche) in einer vorhandenen Szene, und erstellen Sie eine neue Tabelle für den **storyboardverweis** , den Sie soeben erstellt haben:

    [![](images/ref05.png "Creating a segue")](images/ref05.png#lightbox)

7. Wählen Sie im Popupmenü die Option **anzeigen** aus, um den Vorgang abzuschließen:

    [![](images/ref06.png "Selecting Show to complete the Segue")](images/ref06.png#lightbox)

8. Speichern Sie die Änderungen am Storyboard.

Wenn die app ausgeführt wird und der Benutzer auf das Benutzeroberflächen Element klickt, das Sie aus dem-Element erstellt haben, wird der anfängliche Ansichts Controller aus dem externen Storyboard angezeigt, das in der storyboardreferenz angegeben ist.

<a name="Referencing-a-Specific-Scene-in-an-External-Storyboard" />

### <a name="referencing-a-specific-scene-in-an-external-storyboard"></a>Verweisen auf eine bestimmte Szene in einem externen Storyboard

Gehen Sie folgendermaßen vor, um einen Verweis auf eine bestimmte Szene zu einem externen Storyboard (und nicht zum anfänglichen Ansichts Controller) hinzuzufügen:

1. Doppelklicken Sie im **Projektmappen-Explorer**auf das externe Storyboard, um es zur Bearbeitung zu öffnen.

2. Fügen Sie eine neue Szene hinzu, und entwerfen Sie das Layout wie gewohnt:

    [![](images/ref07.png "The new scene layout")](images/ref07.png#lightbox)

3. Geben Sie im **Eigenschaften-Explorer**auf der Registerkarte **Widget** eine **Storyboard-ID** für den Ansichts Controller der neuen Szene ein:

    [![](images/ref08.png "Enter a Storyboard ID for the new Scenes View Controller")](images/ref08.png#lightbox)

4. Öffnen Sie das Storyboard, dem Sie den Verweis hinzufügen möchten, im IOS-Designer.

5. Ziehen Sie einen **storyboardverweis** aus der **Toolbox** auf den Designoberfläche:

    [![](images/ref03.png "A Storyboard Reference")](images/ref03.png#lightbox)

6. Wählen Sie im **Eigenschaften-Explorer**auf der Registerkarte **Widget** den Namen des **Storyboards** und die **Verweis-ID** (Storyboard-ID) der Szene aus, die Sie oben erstellt haben:

    [![](images/ref09.png "The Widget tab ")](images/ref09.png#lightbox)

7. Klicken Sie mit der Maus auf ein UI-Widget (z. b. eine Schaltfläche) in einer vorhandenen Szene, und erstellen Sie eine neue Tabelle für den **storyboardverweis** , den Sie soeben erstellt haben:

    [![](images/ref10.png "Creating a segue")](images/ref10.png#lightbox)

8. Wählen Sie im Popupmenü die Option **anzeigen** aus, um den Vorgang abzuschließen:

    [![](images/ref06.png "Selecting Show to complete the Segue")](images/ref06.png#lightbox)

9. Speichern Sie die Änderungen am Storyboard.

Wenn die app ausgeführt wird und der Benutzer auf das Benutzeroberflächen Element klickt, das Sie in erstellt haben, wird die Szene mit der angegebenen **Storyboard-ID** aus dem externen Storyboard angezeigt, das in der storyboardreferenz angegeben ist.

<a name="Referencing-a-Specific-Scene-in-the-Same-Storyboard" />

### <a name="referencing-a-specific-scene-in-the-same-storyboard"></a>Verweisen auf eine bestimmte Szene im gleichen Storyboard

Gehen Sie folgendermaßen vor, um einem Storyboard einen Verweis auf eine bestimmte Szene hinzuzufügen:

1. Doppelklicken Sie im **Projektmappen-Explorer**auf das Storyboard, um es zur Bearbeitung zu öffnen.

2. Fügen Sie eine neue Szene hinzu, und entwerfen Sie das Layout wie gewohnt:

    [![](images/ref11.png "The new scene layout")](images/ref11.png#lightbox)

3. Geben Sie im **Eigenschaften-Explorer**auf der Registerkarte **Widget** eine **Storyboard-ID** für den Ansichts Controller der neuen Szene ein:

    [![](images/ref12.png "The Widget tab")](images/ref12.png#lightbox)

4. Ziehen Sie einen **storyboardverweis** aus der **Toolbox** auf den Designoberfläche:

   [![](images/ref03.png "A Storyboard Reference")](images/ref03.png#lightbox)

5. Wählen Sie im **Eigenschaften-Explorer**auf der Registerkarte **Widget** die **Verweis-ID** (Storyboard-ID) der Szene aus, die Sie oben erstellt haben:

    [![](images/ref13.png "The Widget tab")](images/ref13.png#lightbox)

6. Klicken Sie mit der Maus auf ein UI-Widget (z. b. eine Schaltfläche) in einer vorhandenen Szene, und erstellen Sie eine neue Tabelle für den **storyboardverweis** , den Sie soeben erstellt haben:

    [![](images/ref14.png "Creating a segue")](images/ref14.png#lightbox)

7. Wählen Sie im Popupmenü die Option **anzeigen** aus, um den Vorgang abzuschließen:

    [![](images/ref06.png "Selecting Show to complete the Segue")](images/ref06.png#lightbox)

8. Speichern Sie die Änderungen am Storyboard.

Wenn die app ausgeführt wird und der Benutzer auf das Benutzeroberflächen Element klickt, das Sie aus erstellt haben, wird die Szene mit der angegebenen **Storyboard-ID** im gleichen Storyboard angezeigt, das in der storyboardreferenz angegeben ist.

## <a name="summary"></a>Summary

In diesem Artikel wird das Konzept von Storyboards vorgestellt und erläutert, wie Sie bei der Entwicklung von IOS-Anwendungen von Nutzen sein können. Darin werden Szenen, Ansichts Controller, Ansichten und Sicht Hierarchien erläutert, und es wird erläutert, wie Szenen mit unterschiedlichen Arten von Enumerationstypen verknüpft werden  Außerdem wird beschrieben, wie Sie Ansichts Controller manuell aus einem Storyboard instanziieren und bedingte Listen erstellen.

## <a name="related-links"></a>Verwandte Themen

- [Manuelles Storyboard (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/manualstoryboard/)
- [Einführung in den IOS-Designer](~/ios/user-interface/designer/introduction.md)
- [Wechseln in Storyboards](https://developer.apple.com/library/ios/#releasenotes/Miscellaneous/RN-AdoptingStoryboards/)
- [Uistoryboard-Klassenreferenz](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UIStoryboard_Class/Reference/Reference.html)
