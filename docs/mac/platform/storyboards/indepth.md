---
title: Arbeiten mit Storyboards
description: "Beim Erstellen von Benutzeroberflächen MacOS, mit Storyboards mithilfe von Xcode."
ms.topic: article
ms.prod: xamarin
ms.assetid: DF4DF7C2-DDD7-4A32-B375-5C5446301EC5
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: effa527b330fb6ca75800392e557289a326f17aa
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="storyboards"></a>Storyboards

Ein Storyboard werden alle für die einer bestimmten app in eine Übersicht über die View-Controller unterteilt die Benutzeroberfläche definiert. In Xcode Schnittstelle-Generator befindet sich jede dieser Controller in einem eigenen Szene ein.

[ ![](indepth-images/intro01.png "Ein Storyboard in Xcodes Benutzeroberflächen-Generator")](indepth-images/intro01.png)

Das Storyboard ist eine Ressourcendatei (mit den Erweiterungen der `.storyboard`), ruft in der Xamarin.Mac-app-Paket enthalten, wenn kompiliert und geliefert wird. Um das Storyboard ab, für die app zu definieren, bearbeiten sie die `Info.plist` Datei, und wählen die **Hauptbenutzeroberfläche** aus dem Dropdownfeld: 

[ ![](indepth-images/sb01.png "Die Datei "Info.plist"-editor")](indepth-images/sb01.png)

<a name="Loading-from-Code" />

## <a name="loading-from-code"></a>Laden aus Code

Es kann vorkommen, dass Sie zum Laden von einem bestimmten Storyboard aus Code, und erstellen Sie manuell eine View-Controller müssen, vorhanden sein. Zum Ausführen dieser Aktion können Sie den folgenden Code verwenden:

```csharp
// Get new window
var storyboard = NSStoryboard.FromName ("Main", null);
var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

// Display
controller.ShowWindow(this);
```

Die `FromName` lädt die Storyboard-Datei mit dem angegebenen Namen, die in der app-Bundles eingeschlossen wurde. Die `InstantiateControllerWithIdentifier` erstellt eine Instanz des View-Controller mit der angegebenen Identität. Legen Sie die Identität im Xcodes Benutzeroberflächen-Generator, beim Entwerfen der Benutzeroberflächenautomatisierungs:

[ ![](indepth-images/sb02.png "Festlegen der Storyboard-ID")](indepth-images/sb02.png)

Optional können Sie die `InstantiateInitialController` -Methode zum Laden von View-Controller, der den ersten Controller im Benutzeroberflächen-Generator zugewiesen wurde:

[ ![](indepth-images/sb03.png "Festlegen des ersten Controllers")](indepth-images/sb03.png)

Es ist gekennzeichnet durch die **Storyboard-Einstiegspunkt** und oben auf den Pfeil nach Öffnen beendet wurde.

<a name="View-Controllers" />

## <a name="view-controllers"></a>View-Controller

View-Controller definieren die Beziehungen zwischen einer bestimmten Ansicht der Informationen innerhalb einer app für Mac und des Datenmodells, die diese Informationen bereitstellt. Jedes oberster Ebene Szene in das Storyboard stellt eine View-Controller in der Xamarin.Mac-app-Code dar.

<a name="The-View-Controller-Lifecycle" />

### <a name="the-view-controller-lifecycle"></a>Der Lebenszyklus der anzeigen-Controller

Mehrere neue Methoden wurden hinzugefügt, um die `NSViewController` Klasse Storyboards in MacOS unterstützt. Allem voran sind die folgenden Methoden verwenden, für den Lebenszyklus der Sicht wird gesteuert, indem der angegebene View-Controller zu reagieren:

- `ViewDidLoad` – Diese Methode wird aufgerufen, wenn die Sicht aus der Storyboard-Datei geladen wird.
- `ViewWillAppear` – Diese Methode wird aufgerufen, kurz bevor die Ansicht auf dem Bildschirm angezeigt wird.
- `ViewDidAppear` – Diese Methode wird direkt aufgerufen werden, nachdem Sie die Ansicht auf dem Bildschirm angezeigt wurde.
- `ViewWillDisappear` – Diese Methode wird aufgerufen, kurz bevor die Sicht aus dem Bildschirm entfernt wird.
- `ViewDidDisappear` – Diese Methode wird direkt aufgerufen werden, nachdem die Sicht aus dem Bildschirm entfernt wurde.
- `UpdateViewConstraints` – Diese Methode wird aufgerufen, wenn die Einschränkungen, die eine Sicht definieren auto-Layout Position und Größe müssen aktualisiert werden.
- `ViewWillLayout` – Diese Methode wird aufgerufen, kurz bevor der Unteransichten dieser Sicht auf Bildschirm angelegt werden.
- `ViewDidLayout` – Diese Methode wird direkt aufgerufen werden, nachdem die Unteransichten der Ansicht auf Bildschirm angelegt werden.

<a name="The-Responder-Chain" />

### <a name="the-responder-chain"></a>Die Kette der Beantworter

Darüber hinaus `NSViewControllers` sind jetzt Teil des Fensters _Beantworter Kette_:

[ ![](indepth-images/vc01.png "Die Kette der Beantworter")](indepth-images/vc01.png)

Und sind daher wired-bis zu empfangen und reagieren auf Ereignisse, z. B. Menüelementauswahlen Ausschneiden, kopieren und einfügen. Diese automatische View Controller-Wire-Up tritt nur bei apps unter MacOS Sierra (10.12) und höher.

<a name="Containment" />

### <a name="containment"></a>Kapselung

View-Controller (z. B. Split-View-Controller und die Registerkarte "-View-Controller) können in Storyboards, nun implementieren _Containment_, sodass sie"andere Sub-View-Controller enthalten können":

[ ![](indepth-images/vc02.png "Ein Beispiel für View Controller-Kapselung")](indepth-images/vc02.png)

Untergeordnete Ansicht Controller enthalten Methoden und Eigenschaften, die sie binden zurück an ihre übergeordneten-View-Controller und Arbeiten mit anzeigen und Entfernen von Ansichten im Bildschirm.

Alle Container-View-Controller integriert MacOS haben ein bestimmtes Layout der Apple wird empfohlen, die Sie befolgen, wenn Sie eine eigene benutzerdefinierte Container-View-Controller zu erstellen:

[ ![](indepth-images/vc03.png "Die View Controller-layout")](indepth-images/vc03.png)

Die Auflistung-View-Controller enthält ein Array von Auflistungselemente anzeigen, von denen jeder ein oder mehrere View-Controller, die eigene Ansichten enthalten enthalten.

<a name="Segues" />

## <a name="segues"></a>Segues

Segues geben die Beziehungen zwischen allen die Szenen, die Benutzeroberfläche Ihrer Anwendung zu definieren. Wenn Sie mit der Arbeit im Storyboards in iOS vertraut sind, wissen Sie, die Segues für iOS in der Regel Übergänge zwischen den gesamten Bildschirmansichten definieren. Dies unterscheidet sich von MacOS, wenn in der Regel Segues definieren "[Containment](#Containment)", wobei eine Szene das untergeordnete Element eines übergeordneten Elements Szene ist.

Die meisten apps tendenziell in MacOS ihre Ansichten in das gleiche Fenster über Benutzeroberflächenelemente, z. B. Split-Sichten und Registerkarten zu gruppieren. Zeigen Sie im Gegensatz zu iOS, in denen Ansichten ein- und ausschalten Bildschirm gewechselt werden müssen, aufgrund der begrenzten physische Speicherplatz.

<a name="Presentation-Segues" />

### <a name="presentation-segues"></a>Präsentation Segues

Angesichts des MacOS Tendenzen für Containment, es gibt Situationen, in denen _Präsentation Segues_ verwendet werden, z. B. modale Fenster, Ansichten und Popovers. MacOS bietet die folgenden integrierten segue Typen:

- **Anzeigen** -zeigt das Ziel der Segue als nicht modalen Fenster an. Verwenden Sie diese Art von Segue z. B. an eine andere Instanz von einem Dokumentfenster in Ihrer app.
- **Modale** – stellt das Ziel der Segue als modales Fenster. Beispielsweise verwenden Sie diese Art von Segue Fenster Voreinstellungen für Ihre app darstellen.
- **Blatt** -das Ziel der Segue zeigt, wie ein Stylesheet für das übergeordnete Fenster angefügt. Beispielsweise verwendet, die diese Art von segue zum Suchen und Ersetzen Sie Blatt zu präsentieren.
- **Popover** – stellt das Ziel der Segue wie in einem Fenster Popover. Verwenden Sie z. B. diesen Segue-Typ, um Optionen zu präsentieren, wenn vom Benutzer ein Element der Benutzeroberfläche geklickt wird.
- **Benutzerdefinierte** -zeigt das Ziel der Segue mithilfe eines benutzerdefinierten Segue Typs, der vom Entwickler definiert. Finden Sie unter der [erstellen benutzerdefinierte Segues](#Creating-Custom-Segues) weiter unten für weitere Details.

Wenn Segues Präsentation verwenden, können Sie überschreiben die `PrepareForSegue` Methode des übergeordneten Elements View Controller für die Präsentation zum Initialisieren und Variablen, und geben Sie alle Daten, die View-Controller angezeigt wird.

<a name="Triggered-Segues" />

### <a name="triggered-segues"></a>Ausgelöst Segues

Ausgelöste Segues ermöglichen es Ihnen, benannte Segues angeben (über ihre **Bezeichner** Eigenschaft im Benutzeroberflächen-Generator) und lassen Sie sie ausgelöst werden, z. B. der Benutzer auf eine Schaltfläche oder durch Aufrufen der `PerformSegue` Methode im Code:

```csharp
// Display the Scene defined by the given Segue ID
PerformSegue("MyNamedSegue", this);
``` 

Die Segue-ID ist innerhalb Xcodes Schnittstelle-Generator definiert, wenn das Layout der Benutzeroberfläche der Anwendung:

[ ![](indepth-images/sg02.png "Eingeben einer Segue Name")](indepth-images/sg02.png)

Sie sollten in die View-Controller, der als Quelle für die Segue fungiert, überschreiben die `PrepareForSegue` -Methode und führen keine Initialisierung erforderlich, bevor die Segue ausgeführt wird und der angegebene View-Controller wird angezeigt:

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on Segue ID
    switch (segue.Identifier) {
    case "MyNamedSegue":
        // Prepare for the segue to happen
        ...
        break;
    }
}
```

Sie können optional überschreiben die `ShouldPerfromSegue` -Methode und das Steuerelement, und zwar unabhängig davon, ob die Segue tatsächlich über C#-Code ausgeführt wird. Rufen Sie für manuell dargestellten View-Controller, deren `DismissController` Methode, um sie aus der Anzeige zu entfernen, wenn sie nicht mehr benötigt werden.

<a name="Creating-Custom-Segues" />

### <a name="creating-custom-segues"></a>Erstellen von benutzerdefinierten Segues

Es gibt möglicherweise Zeiten, wenn die app einen Segue-Typ nicht von der Build in Segues in MacOS definierten bereitgestellt erfordert. Wenn dies der Fall ist, können Sie bei der app UI Layout in Xcodes Benutzeroberflächen-Generator erstellen benutzerdefinierte Segue, die zugewiesen werden können.

Zum Erstellen eines neuen Segue-Typs, das die aktuelle View-Controller in einem Fenster (statt Sie das Ziel Szene in einem neuen Fenster öffnen) ersetzt wird, können wir z. B. den folgenden Code verwenden:

```csharp
using System;
using AppKit;
using Foundation;

namespace OnCardMac
{
    [Register("ReplaceViewSeque")]
    public class ReplaceViewSeque : NSStoryboardSegue
    {
        #region Constructors
        public ReplaceViewSeque() {

        }

        public ReplaceViewSeque (string identifier, NSObject sourceController, NSObject destinationController) : base(identifier,sourceController,destinationController) {

        }

        public ReplaceViewSeque (IntPtr handle) : base(handle) {
        }

        public ReplaceViewSeque (NSObjectFlag x) : base(x) {
        }
        #endregion

        #region Override Methods
        public override void Perform ()
        {
            // Cast the source and destination controllers
            var source = SourceController as NSViewController;
            var destination = DestinationController as NSViewController;

            // Swap the controllers
            source.View.Window.ContentViewController = destination;

            // Release memory
            source.RemoveFromParentViewController ();
        }
        #endregion

    }
        
}
```

Ein paar Dinge zu beachten:

- Wir verwenden die `Register` Attribut, um diese Klasse für Objective-C/MacOS verfügbar machen.
- Wir sind überschreiben die `Perform` Methode, um die Aktion der unsere benutzerdefinierte Segue tatsächlich auszuführen.
- Es ersetzt des Fensters `ContentViewController` -Controller mit der Datei durch das Ziel (Ziel) die Segue definiert.
- Wir werden die ursprünglichen View-Controller, um Arbeitsspeicher freizugeben Entfernen der `RemoveFromParentViewController` Methode.

Um diese neue Art von Segue in Xcodes Benutzeroberflächen-Generator verwenden zu können, müssen wir kompilieren Sie die Anwendung zuerst, dann wechseln Sie zu Xcode, und fügen eine neue Segue zwischen zwei Szenen. Festlegen der **Stil** auf **benutzerdefinierte** und die **Segue Klasse** zu `ReplaceViewSegue` (der Name der unsere benutzerdefinierte Segue-Klasse):

[ ![](indepth-images/sg01.png "Festlegen der Segue-Klasse")](indepth-images/sg01.png)

<a name="Triggered-Segues" />

## <a name="window-controllers"></a>Fenster-Controller

Fenster Controller enthalten und steuern Sie die verschiedenen Fenster-Typen, die Ihre app MacOS erstellen können. Für Storyboards verfügen sie über die folgenden Funktionen:

1. Sie müssen einen Content-View-Controller bieten. Dies ist der gleiche Inhalt View-Controller wird, die die untergeordneten Fenster hat.
2. Die `Storyboard` -Eigenschaft enthält das Storyboard, die dem Fenster Controller aus,, andernfalls geladen wurde `null` Wenn aus einem Storyboard nicht geladen.
3. Sie erreichen die `DismissController` Methode, um das angegebene Fenster schließen und entfernen sie aus der Ansicht.

Wie die View-Controller Fenster Controller implementieren die `PerformSegue`, `PrepareForSegue` und `ShouldPerfromSegue` Methoden und kann als Quelle eines Segue-Vorgangs verwendet werden.

Fenster Controller sind verantwortlich für die folgenden Funktionen von einer MacOS-app:

- Sie verwalten einen bestimmten Fenster zurückzuwechseln.
- Verwalten sie Titelleiste und in der Symbolleiste des Fensters (falls verfügbar).
- Sie verwalten die Content View-Controller, um den Inhalt des Fensters anzuzeigen.

<a name="Gesture-Recognizers" />

## <a name="gesture-recognizers"></a>Geste Prüfer

Geste Prüfer für MacOS sind nahezu identisch mit ihren äquivalenten in iOS und ermöglichen es dem Entwickler auf einfache Weise Gesten (z. B. Maustaste klicken) hinzufügen, Elemente in der app UI.

Allerdings werden, wenn Gesten in iOS mit der app-Design (z. B. Tippen Sie auf dem Bildschirm mit zwei Fingern fest), am häufigsten bestimmt wartungsmoduskonfiguration Gesten in MacOS von Hardware.

Mit Geste Prüfer, können Sie den Umfang des Codes erforderlich, um das Hinzufügen von benutzerdefinierter Aktivitäten zu einem Element in der Benutzeroberfläche erheblich reduzieren. Wie sie zwischen double und einzigen Klicks automatisch ermitteln können, klicken Sie auf, und ziehen Sie, Ereignisse usw.

Anstelle von der `MouseDown` Ereignis in die View-Controller, Sie sollten verwenden eine Geste Erkennung auf der Benutzer bei der Arbeit mit Storyboards behandeln.

Die folgenden Geste Merkmale sind in MacOS verfügbar:

- `NSClickGestureRecognizer` – Registrieren Sie den Mauszeiger nach unten und Ereignisse.
- `NSPanGestureRecognizer` -Register Maustaste unten drag und Freigeben von Ereignissen.
- `NSPressGestureRecognizer` -Register eine Maustaste gedrückt, für eine bestimmte Menge von Ereignissen zur Startzeit.
- `NSMagnificationGestureRecognizer` – Ein Ereignis Vergrößerung von Trackpad Hardware registriert.
- `NSRotationGestureRecognizer` – Ein Ereignis Rotation von Trackpad Hardware registriert.

<a name="Using-Storyboard-References" />

## <a name="using-storyboard-references"></a>Verwenden des Storyboard-Verweise

Ein Storyboard Verweis können Sie einen großen, komplexen Storyboard-Entwurf und teilen sie in kleinere Storyboards, die aus dem ursprünglichen referenziert abrufen, daher entfernen Entfernen von Komplexität und machen das resultierende einzelne Storyboards einfacher zu Entwurf und verwalten.

Darüber hinaus bieten ein Storyboard-Verweis ein _Anker_ in eine andere Szene innerhalb der gleichen Storyboard oder einer bestimmten Szene auf einen anderen.

<a name="Referencing-an-External-Storyboard" />

### <a name="referencing-an-external-storyboard"></a>Verweisen auf eine externe Storyboard

Um einen Verweis auf eine externe Storyboard hinzuzufügen, führen Sie folgende Schritte aus:

1. In der **Projektmappen-Explorer**mit der rechten Maustaste auf den Projektnamen, und wählen Sie **hinzufügen** > **neue Datei...**   >  **Mac** > **Storyboard**. Geben Sie einen **Namen** für das neue Storyboard und auf die **neu** Schaltfläche: 

    [ ![](indepth-images/ref01.png "Hinzufügen eines neuen Storyboards")](indepth-images/ref01.png)
2. In der **Projektmappen-Explorer**, doppelklicken Sie auf das neue Storyboard-Namen, um ihn zur Bearbeitung in Xcodes Benutzeroberflächen-Generator zu öffnen.
2. Entwerfen Sie das Layout der das neue Storyboard Szenen wie gewohnt würde und die Änderungen zu speichern: 

    [ ![](indepth-images/ref02.png "Entwerfen der Benutzeroberfläche")](indepth-images/ref02.png)
3. Wechseln Sie zu der Storyboards, die Sie den Verweis auf im-Generator-Schnittstelle hinzugefügt werden soll.
4. Ziehen Sie eine **Storyboard-Verweis** aus der **Object Library** auf die Entwurfsoberfläche: 

    [ ![](indepth-images/ref03.png "Einen Storyboard-Verweis auswählen in der Bibliothek.")](indepth-images/ref03.png)
5. In der **Attribut Inspektor**, wählen Sie den Namen des der **Storyboard** , die Sie soeben erstellt haben: 

    [ ![](indepth-images/ref04.png "Konfigurieren den Verweis")](indepth-images/ref04.png)
6. STRG-Taste auf ein UI-Widget (z. B. eine Schaltfläche) auf einer vorhandenen Szene und erstellen Sie eine neue Segue auf die **Storyboard Verweis** , die Sie gerade erstellt haben.  Wählen Sie im Popupmenü **anzeigen** der Segue abgeschlossen: 

    [ ![](indepth-images/ref06.png "Festlegen der Segue-Typs")](indepth-images/ref06.png) 
8. Speichern Sie die Änderungen auf das Storyboard.
9. Zurück zu Visual Studio für Mac, um die Änderungen zu synchronisieren.

Wenn die app ausgeführt wird und der Benutzer auf dem UI-Element klickt, dass Sie die Segue aus dem ersten Fenster Controller aus dem externen Storyboard in der Referenz zu Storyboard angegeben erstellt wird angezeigt.

<a name="Referencing-a-Specific-Scene-in-an-External-Storyboard" />

### <a name="referencing-a-specific-scene-in-an-external-storyboard"></a>Verweisen auf eine bestimmte Szene in einem externen Storyboard

Um einen Verweis auf eine bestimmte Szene hinzufügen führen einer externen Storyboard (und nicht der erste Controller Fenster), Sie folgende:

1. In der **Projektmappen-Explorer**, doppelklicken Sie auf das externe Storyboard, um ihn zur Bearbeitung in Xcodes Benutzeroberflächen-Generator zu öffnen.
2. Fügen Sie eine neue Szene hinzu und Entwerfen Sie des Layouts wie gewohnt verwenden: 

    [ ![](indepth-images/ref07.png "Entwerfen des Layouts in Xcode")](indepth-images/ref07.png)
3. In der **Identität Inspektor**, geben Sie einen **Storyboard-ID** für der neuen Szene Fenster Controller: 

    [ ![](indepth-images/ref08.png "Festlegen der Storyboard-ID")](indepth-images/ref08.png)
3. Öffnen Sie das Storyboard, die Sie den Verweis auf in Benutzeroberflächen-Generator hinzugefügt werden soll.
4. Ziehen Sie eine **Storyboard-Verweis** aus der **Object Library** auf die Entwurfsoberfläche: 

    [ ![](indepth-images/ref03.png "Einen Storyboard-Verweis auswählen aus der Bibliothek.")](indepth-images/ref03.png)
5. In der **Identität Inspektor**, wählen Sie den Namen des der **Storyboard** und **Anwendungsverweis-ID** (Storyboard-ID) der Szene, die Sie soeben erstellt haben: 

    [ ![](indepth-images/ref09.png "Festlegen der Verweis-ID")](indepth-images/ref09.png)
6. STRG-Taste auf ein UI-Widget (z. B. eine Schaltfläche) auf einer vorhandenen Szene und erstellen Sie eine neue Segue auf die **Storyboard Verweis** , die Sie gerade erstellt haben. Wählen Sie im Popupmenü **anzeigen** der Segue abgeschlossen: 

    [ ![](indepth-images/ref06.png "Festlegen der Segue-Typs")](indepth-images/ref06.png) 
8. Speichern Sie die Änderungen auf das Storyboard.
9. Zurück zu Visual Studio für Mac, um die Änderungen zu synchronisieren.

Wenn die app ausführen und der Benutzer klickt auf das Element der Benutzeroberfläche, die Sie aus der Szene mit der Segue erstellt den angegebenen **Storyboard-ID** aus dem externen Storyboard in der Storyboard-Verweis angegeben wird angezeigt.

<a name="Referencing-a-Specific-Scene-in-the-Same-Storyboard" />

### <a name="referencing-a-specific-scene-in-the-same-storyboard"></a>Verweisen auf eine bestimmte Szene im gleichen Storyboard

Um einen Verweis auf eine bestimmte Szene das gleiche Storyboard hinzuzufügen, führen Sie folgende Schritte aus:

1. In der **Projektmappen-Explorer**, doppelklicken Sie auf das Storyboard, um sie zur Bearbeitung zu öffnen.
2. Fügen Sie eine neue Szene hinzu und Entwerfen Sie des Layouts wie gewohnt verwenden: 

    [ ![](indepth-images/ref11.png "Bearbeiten das Storyboard in Xcode")](indepth-images/ref11.png)
3. In der **Identität Inspektor**, geben Sie einen **Storyboard-ID** für der neuen Szene Fenster Controller: 

    [ ![](indepth-images/ref12.png "Festlegen der Storyboard-ID")](indepth-images/ref12.png)
3. Ziehen Sie eine **Storyboard-Verweis** aus der **Toolbox** auf die Entwurfsoberfläche: 

    [ ![](indepth-images/ref03.png "Einen Storyboard-Verweis auswählen aus der Bibliothek.")](indepth-images/ref03.png)
5. In **Attribut Inspektor**Option **Anwendungsverweis-ID** (Storyboard-ID) der Szene, die Sie soeben erstellt haben: 

    [ ![](indepth-images/ref13.png "Festlegen der Verweis-ID")](indepth-images/ref13.png)
6. STRG-Taste auf ein UI-Widget (z. B. eine Schaltfläche) auf einer vorhandenen Szene und erstellen Sie eine neue Segue auf die **Storyboard Verweis** , die Sie gerade erstellt haben. Wählen Sie im Popupmenü **anzeigen** der Segue abgeschlossen: 

    [ ![](indepth-images/ref06.png "Segue auswählen")](indepth-images/ref06.png) 
8. Speichern Sie die Änderungen auf das Storyboard.
9. Zurück zu Visual Studio für Mac, um die Änderungen zu synchronisieren.

Wenn die app ausführen und der Benutzer klickt auf das Element der Benutzeroberfläche, die Sie aus der Szene mit der Segue erstellt den angegebenen **Storyboard-ID** im gleichen Storyboard in der Storyboard-Verweis angegeben wird angezeigt.

<a name="Complex-Storyboard-Example" />

## <a name="complex-storyboard-example"></a>Komplexe Storyboard-Beispiel

Eines komplexen Beispiels mit Storyboards in einer app Xamarin.Mac zu arbeiten, finden Sie unter der [SourceWriter Beispiel-App](https://developer.xamarin.com/samples/mac/SourceWriter/). SourceWriter ist ein einfacher Quellcode-Editor, der Unterstützung für die Codevervollständigung und die einfache Syntaxhervorhebung bietet.

Der SourceWriter-Code wurde vollständig kommentiert und es wurden, wenn möglich, Links der Schlüsseltechnologien und -methoden zu wichtigen Informationen in den Leitfäden der Xamarin.Mac-Dokumentation bereitgestellt.

## <a name="related-links"></a>Verwandte Links

- [MacStoryboard (Beispiel)](https://developer.xamarin.com/samples/mac/MacStoryboard/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Arbeiten mit Fenstern](~/mac/user-interface/window.md)
- [OS X Human Richtlinien zur Benutzeroberfläche](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Einführung in Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
