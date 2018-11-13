---
title: Arbeiten mit Storyboards in Xamarin.Mac
description: Dieses Dokument beschreibt das Arbeiten mit Storyboards in Xamarin.Mac, feststellen, wie Sie sie aus Code, den Lebenszyklus der Ansicht-Controller, der Beantworter Kette zu laden, Übergänge, Fenster-Controller, Geste Erkennungen und mehr.
ms.prod: xamarin
ms.assetid: DF4DF7C2-DDD7-4A32-B375-5C5446301EC5
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 61d598f90747cf47b613012328f77b4bd8953a41
ms.sourcegitcommit: 849bf6d1c67df943482ebf3c80c456a48eda1e21
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51528457"
---
# <a name="working-with-storyboards-in-xamarinmac"></a>Arbeiten mit Storyboards in Xamarin.Mac

Ein Storyboard definiert die Benutzeroberfläche für eine bestimmte app in eine funktionale Übersicht über die View-Controller unterteilt werden. In Interface Builder von Xcode befindet sich jede dieser Controller in einen eigenen Szene.

[![](indepth-images/intro01.png "Ein Storyboard im Interface Builder von Xcode")](indepth-images/intro01.png#lightbox)

Das Storyboard ist eine Ressourcendatei (mit den Erweiterungen der `.storyboard`), ruft ab, in dem Xamarin.Mac-app Bundle enthalten, wenn er kompiliert und ausgeliefert wird. Um das Storyboard ab, für Ihre app zu definieren, bearbeiten sie die `Info.plist` und wählen Sie die **Hauptschnittstelle** aus dem Dropdownfeld: 

[![](indepth-images/sb01.png "Die Datei \"Info.plist\"-editor")](indepth-images/sb01.png#lightbox)

<a name="Loading-from-Code" />

## <a name="loading-from-code"></a>Laden aus Code

Es gibt möglicherweise Zeiten, wenn Sie bestimmte Storyboard aus Code zu laden und einen Ansichtscontroller manuell erstellen müssen. Sie können den folgenden Code verwenden, zum Ausführen dieser Aktion:

```csharp
// Get new window
var storyboard = NSStoryboard.FromName ("Main", null);
var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

// Display
controller.ShowWindow(this);
```

Die `FromName` lädt die Storyboard-Datei mit dem angegebenen Namen, die in der app Bundle eingeschlossen wurde. Die `InstantiateControllerWithIdentifier` erstellt eine Instanz des Ansichtscontrollers mit der angegebenen Identität. Legen Sie die Identität im Interface Builder von Xcode, beim Entwerfen der Benutzeroberflächenautomatisierungs:

[![](indepth-images/sb02.png "Festlegen der Storyboard-ID")](indepth-images/sb02.png#lightbox)

Optional können Sie die `InstantiateInitialController` Methode, um die View-Controller zu laden, der dem ersten Controller in Interface Builder zugewiesen wurde:

[![](indepth-images/sb03.png "Den erste Controller festlegen")](indepth-images/sb03.png#lightbox)

Gekennzeichnet durch die **Storyboard-Einstiegspunkt** und oben auf den Pfeil nach offenem Ende.

<a name="View-Controllers" />

## <a name="view-controllers"></a>Ansichtscontroller

Ansichtscontroller definieren die Beziehungen zwischen einer bestimmten Ansicht der Informationen in einer Mac-app und das Datenmodell, das die Informationen bereitstellt. Jede Szene auf oberster Ebene im Storyboard stellt einen Ansichtscontroller im Code der Xamarin.Mac-app dar.

<a name="The-View-Controller-Lifecycle" />

### <a name="the-view-controller-lifecycle"></a>Der Lebenszyklus der Ansicht-Controller

Mehrere neue Methoden hinzugefügt wurden die `NSViewController` Klasse, um Storyboards in MacOS zu unterstützen. Sind am wichtigsten, dass die folgenden Methoden verwenden, um auf den Lebenszyklus der Ansicht wird gesteuert, indem der angegebene View-Controller zu reagieren:

- `ViewDidLoad` – Diese Methode wird aufgerufen, wenn die Sicht aus der Storyboard-Datei geladen wird.
- `ViewWillAppear` – Diese Methode wird aufgerufen, kurz bevor die Ansicht auf dem Bildschirm angezeigt wird.
- `ViewDidAppear` – Diese Methode wird direkt aufgerufen, nachdem Sie die Ansicht auf dem Bildschirm angezeigt wurde.
- `ViewWillDisappear` – Diese Methode wird aufgerufen, kurz bevor die Ansicht vom Bildschirm entfernt wird.
- `ViewDidDisappear` – Diese Methode wird direkt aufgerufen, nachdem die Ansicht vom Bildschirm entfernt wurde.
- `UpdateViewConstraints` – Diese Methode wird aufgerufen, wenn die Einschränkungen, die eine Sicht definiert Layout Position und Größe müssen aktualisiert werden, automatisch.
- `ViewWillLayout` – Diese Methode wird aufgerufen, kurz bevor die Unteransichten dieser Ansicht auf dem Bildschirm angeordnet werden.
- `ViewDidLayout` – Diese Methode wird direkt aufgerufen, nachdem die Unteransichten der Ansicht auf dem Bildschirm angeordnet werden.

<a name="The-Responder-Chain" />

### <a name="the-responder-chain"></a>Der Responder-Kette

Darüber hinaus `NSViewControllers` sind jetzt Bestandteil des Fensters _Beantworter Kette_:

[![](indepth-images/vc01.png "Der Responder-Kette")](indepth-images/vc01.png#lightbox)

Und daher werden diese wired – bis zu empfangen und reagieren auf Ereignisse wie Ausschneiden, kopieren und fügen Sie die Auswahl von Menüelementen. Diese automatische View Controller anschließen tritt nur auf apps, die unter MacOS Sierra (10.12) und höher.

<a name="Containment" />

### <a name="containment"></a>Kapselung

In Storyboards, View Controller (z. B. den Controller für geteilte Ansicht und der Registerkarte "-View-Controller) können nun implementieren _Kapselung_, sodass sie"andere Sub View Controller enthalten können,":

[![](indepth-images/vc02.png "Ein Beispiel für View Controller-Kapselung")](indepth-images/vc02.png#lightbox)

Untergeordnete View Controller enthalten die Methoden und Eigenschaften, die sie verknüpfen zurück, deren übergeordnete-View-Controller und Arbeiten mit anzeigen und Löschen von Sichten auf dem Bildschirm.

Alle Container-View-Controller, die in MacOS integriert haben, ein bestimmtes Layout auf die Apple wird empfohlen, die Sie ausführen, wenn Sie Ihre eigenen benutzerdefinierten Container-View-Controller zu erstellen:

[![](indepth-images/vc03.png "Das View Controller-layout")](indepth-images/vc03.png#lightbox)

Der Ansichtscontroller für die Auflistung enthält ein Array von Auflistungselemente anzeigen, von denen jeder enthalten mindestens eine View-Controller, die ihre eigenen Ansichten enthalten.

<a name="Segues" />

## <a name="segues"></a>Segues

Segues geben die Beziehungen zwischen allen im Hintergrund, die UIs Ihrer app zu definieren. Wenn Sie mit der Arbeiten in Storyboards unter iOS vertraut sind, beachten Sie, dass Segues für iOS die Übergänge zwischen Vollbild-Ansichten in der Regel definieren. Dies unterscheidet sich von Mac OS, wenn Übergänge in der Regel definieren "[Kapselung](#Containment)", wobei eine Szene das untergeordnete Element eines übergeordneten Elements Szene ist.

Die meisten apps tendenziell unter MacOS ihre Ansichten zusammen im gleichen Fenster mithilfe von UI-Elemente wie z. B. geteilten Ansichten und Registerkarten gruppiert. Zeigen Sie im Gegensatz zu iOS, in denen Sichten ein- und ausschalten Bildschirm umgestellt werden müssen, aufgrund der begrenzten physische Speicherplatz.

<a name="Presentation-Segues" />

### <a name="presentation-segues"></a>Präsentation Segues

Angesichts des MacOS-Tendenzen zu Kapselung, es gibt Situationen, in denen _Präsentation Segues_ verwendet werden, z. B. modalen Windows, Tabellenansichten und Popovers. MacOS bietet die folgende integrierten segue-Typen:

- **Anzeigen** -das Ziel der Segue als nicht modales Fenster angezeigt. Verwenden Sie z. B. diese Art von Segue, um eine andere Instanz eines Dokumentfensters in Ihrer app vorhanden.
- **Modale** – stellt das Ziel der Segue als modales Fenster. Beispielsweise verwenden Sie diese Art von Segue, um das Fenster "Einstellungen" für Ihre app darstellen.
- **Blatt** -das Ziel der Segue zeigt, wie eine Tabelle für das übergeordnete Fenster angefügt. Beispielsweise verwendet, die diese Art von segue, einen suchen und Ersetzen Blatt präsentieren.
- **Im Popover** -das Ziel der Segue zeigt, wie in einem Fenster im Popover. Verwenden Sie z. B. dieser Segue-Typ, um Optionen zu präsentieren, wenn ein Element der Benutzeroberfläche geklickt wird.
- **Benutzerdefinierte** – stellt das Ziel der Segue, verwenden einen benutzerdefinierten Segue-Typ vom Entwickler definiert. Finden Sie unter den [Erstellen von benutzerdefinierten Segues](#Creating-Custom-Segues) Informationen weiter unten im Abschnitt.

Wenn Segues Präsentation verwenden, können Sie überschreiben die `PrepareForSegue` -Methode des übergeordneten Ansichtscontroller für Präsentationen zum Initialisieren und Variablen, und geben Sie alle Daten auf den View-Controller, der angezeigt wird.

<a name="Triggered-Segues" />

### <a name="triggered-segues"></a>Ausgelöst Segues

Ausgelöste Segues ermöglichen es Ihnen, benannte Segues angeben (über ihre **Bezeichner** -Eigenschaft in Interface Builder) zu bitten, ausgelöst durch Ereignisse wie z. B. der Benutzer auf eine Schaltfläche oder durch Aufrufen der `PerformSegue` Methode im Code:

```csharp
// Display the Scene defined by the given Segue ID
PerformSegue("MyNamedSegue", this);
``` 

Der Segue-ID ist innerhalb von Interface Builder von Xcode definiert, wenn das Layout der Benutzeroberfläche der Anwendung sind:

[![](indepth-images/sg02.png "Eingeben einer Segue Name")](indepth-images/sg02.png#lightbox)

Sie sollten in der View-Controller, der als Quelle für die Segue fungiert, überschreiben die `PrepareForSegue` -Methode, und führen alle Initialisierungsschritte erforderlich, bevor der Segue ausgeführt wird und der angegebene View-Controller wird angezeigt:

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

Optional können Sie überschreiben die `ShouldPerformSegue` -Methode und -Steuerelement, und zwar unabhängig davon, ob der Segue tatsächlich über ausgeführt C# Code. Rufen Sie für die manuell angegebene View-Controller, deren `DismissController` Methode, die sie aus der Anzeige zu entfernen, wenn sie nicht mehr benötigt werden.

<a name="Creating-Custom-Segues" />

### <a name="creating-custom-segues"></a>Erstellen von benutzerdefinierten Segues

Es gibt möglicherweise Zeiten, wenn Ihre app einen Segue-Typ nicht angegeben wird, durch die integrierte Segues in MacOS definiert benötigt. Wenn dies der Fall ist, können Sie bei Ihrer app-Benutzeroberfläche zur Festlegung in Interface Builder von Xcode erstellen eine benutzerdefinierte Segue, die zugewiesen werden kann.

Um einen neuen Segue-Typ zu erstellen, der der aktuelle Ansichtscontroller in einem Fenster (statt Sie das Ziel Szene in einem neuen Fenster öffnen) ersetzt, können wir beispielsweise den folgenden Code verwenden:

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

Ein paar Punkte zu beachten:

- Wir verwenden die `Register` Attribut, um diese Klasse in Objective-C/MacOS verfügbar zu machen.
- Überschreiben wir die `Perform` Methode, um die Aktion der unsere benutzerdefinierte Segue tatsächlich auszuführen.
- Wir Ersetzen des Fensters `ContentViewController` Controller mit dem durch das Ziel (Ziel) der Segue definiert.
- Entfernen wir den ursprünglichen Ansichtscontroller, um Arbeitsspeicher freizugeben der `RemoveFromParentViewController` Methode.

Um diesen neuen Segue-Typ in Interface Builder von Xcode verwenden zu können, müssen wir zuerst kompiliert die app dann wechseln Sie zu Xcode und Hinzufügen einer neuen Segue zwischen den beiden Szenen. Festlegen der **Stil** zu **benutzerdefinierte** und **Segue-Klasse** zu `ReplaceViewSegue` (den Namen unserer benutzerdefinierten Segue-Klasse):

[![](indepth-images/sg01.png "Festlegen der Segue-Klasse")](indepth-images/sg01.png#lightbox)

<a name="Triggered-Segues" />

## <a name="window-controllers"></a>Fenster-Controller

Fenster Controller enthalten und steuern die verschiedenen Fenster-Typen, die Ihre app für MacOS erstellen können. Nach Storyboards haben sie die folgenden Funktionen:

1. Sie müssen einen Content View-Controller bereitstellen. Dies ist der gleiche Inhalt View-Controller wird, die die untergeordneten Fenster.
2. Die `Storyboard` Eigenschaft enthält das Storyboard, das dem Controller im Fenster, andernfalls geladen wurde `null` Wenn nicht von einem Storyboard geladen.
3. Rufen Sie die `DismissController` Methode, um das angegebene Fenster zu schließen und aus Ansicht entfernen.

Wie die View-Controller, Fenster-Controller implementieren die `PerformSegue`, `PrepareForSegue` und `ShouldPerformSegue` Methoden und kann als Quelle eines Segues-Vorgangs verwendet werden.

Fenstercontroller sind verantwortlich für die folgenden Funktionen von einem MacOS-app:

- Sie verwalten, ein bestimmtes Fenster.
- Sie verwalten Titelleiste angezeigt wird und in der Symbolleiste des Fensters (falls verfügbar).
- Sie verwalten die Content View-Controller zum Anzeigen des Fensters.

<a name="Gesture-Recognizers" />

## <a name="gesture-recognizers"></a>Geste Erkennungen

Geste Erkennungen für MacOS sind nahezu identisch mit ihren Gegenstücken in iOS und ermöglichen es dem Entwickler auf einfache Weise Aktionen (z. B. das Klicken auf die zweite Mausklick) hinzufügen, Elemente in Ihrer app Benutzeroberfläche.

Allerdings werden, in dem Gesten in iOS der app-Entwurf (z. B. durch Tippen auf dem Bildschirm mit zwei Fingern), am häufigsten ermittelt, Gesten in MacOS von Hardware, die bestimmt werden.

Verwenden Sie die Bewegung Erkennungen, können Sie die Menge an Code zum Hinzufügen von benutzerdefinierter Aktivitäten zu einem Element in der Benutzeroberfläche erforderlich erheblich reduzieren. Wie sie zwischen den doppelte und einfache Klicks automatisch ermitteln können, klicken Sie auf, und ziehen Sie, Ereignisse usw. aus.

Anstelle von der `MouseDown` Ereignis in Ihrem Ansichtscontroller sollten verwenden Sie eine Stiftbewegungs-Erkennung, um das Eingabeereignis des Benutzers ein, bei der Arbeit mit Storyboards zu behandeln.

Die folgenden Gesten Erkennungen sind in MacOS verfügbar:

- `NSClickGestureRecognizer` – Registrieren Sie die Maus Down- und up-Ereignisse.
- `NSPanGestureRecognizer` -Register Maustaste unten drag und Freigeben von Ereignissen.
- `NSPressGestureRecognizer` : Registriert eine Maustaste gedrückt, für eine bestimmte Menge an Ereignissen zur Startzeit.
- `NSMagnificationGestureRecognizer` – Registriert ein Ereignis für die Vergrößerung aus Trackpad Hardware.
- `NSRotationGestureRecognizer` – Registriert ein Ereignis für die Drehung aus Trackpad Hardware.

<a name="Using-Storyboard-References" />

## <a name="using-storyboard-references"></a>Verwenden von Storyboard-verweisen

Ein Storyboard-Verweis können Sie einen großen, komplexen Storyboard-Entwurf und teilen sie in kleinere Storyboards, die aus der ursprünglichen referenziert erhalten daher entfernen die Komplexität und erleichtert die sich ergebende einzelne Storyboards zum Entwerfen und verwalten.

Darüber hinaus bieten ein Storyboard-Verweis ein _Anker_ zu einem anderen Szene innerhalb des gleichen Storyboards oder eine bestimmte Szene auf einem anderen.

<a name="Referencing-an-External-Storyboard" />

### <a name="referencing-an-external-storyboard"></a>Verweisen auf ein externes Storyboard

Um einen Verweis auf ein externes Storyboard hinzuzufügen, führen Sie folgende Schritte aus:

1. In der **Projektmappen-Explorer**mit der rechten Maustaste auf den Projektnamen, und wählen Sie **hinzufügen** > **neue Datei...**   >  **Mac** > **Storyboard**. Geben Sie einen **Namen** für das neue Storyboard und auf die **neu** Schaltfläche: 

    [![](indepth-images/ref01.png "Hinzufügen eines neuen Storyboards")](indepth-images/ref01.png#lightbox)
2. In der **Projektmappen-Explorer**, doppelklicken Sie auf der neue Name des Storyboards, die sie zur Bearbeitung in Xcode Interface Builder zu öffnen.
2. Entwerfen Sie das Layout der das neue Storyboard Szenen wie gewohnt würde und die Änderungen zu speichern: 

    [![](indepth-images/ref02.png "Entwurf der Schnittstelle")](indepth-images/ref02.png#lightbox)
3. Wechseln Sie zu das Storyboard, das auf den Verweis auf in Interface Builder hinzugefügt werden.
4. Ziehen Sie eine **Storyboard-Verweis** aus der **Object Library** auf die Entwurfsoberfläche: 

    [![](indepth-images/ref03.png "Wählen einen Storyboard-Verweis in der Bibliothek")](indepth-images/ref03.png#lightbox)
5. In der **Attributinspektor**, wählen Sie den Namen der **Storyboard** , die Sie soeben erstellt haben: 

    [![](indepth-images/ref04.png "Konfigurieren den Verweis")](indepth-images/ref04.png#lightbox)
6. Strg + Klick auf ein UI-Widget (z. B. eine Schaltfläche) auf einer vorhandenen Szene, und erstellen Sie eine neue Segue, die **Storyboard Verweis** , die Sie gerade erstellt haben.  Wählen Sie im Popupmenü **anzeigen** zum Abschließen der Segue: 

    [![](indepth-images/ref06.png "Segue-Typ")](indepth-images/ref06.png#lightbox) 
8. Speichern Sie die Änderungen auf das Storyboard an.
9. Zurück zu Visual Studio für Mac, um Ihre Änderungen zu synchronisieren.

Wenn die app ausgeführt wird und der Benutzer auf das Element der Benutzeroberfläche klickt der Erstellung der Segue aus, die anfängliche Fenstercontroller aus dem externen Storyboard in der Storyboard-Verweis angegeben wird angezeigt.

<a name="Referencing-a-Specific-Scene-in-an-External-Storyboard" />

### <a name="referencing-a-specific-scene-in-an-external-storyboard"></a>Verweisen auf eine bestimmte Szene in einer externen Storyboard

Um einen Verweis auf eine bestimmte Szene hinzufügen wie eines externen Storyboards (und nicht das erste Fenster-Controller) folgt vor:

1. In der **Projektmappen-Explorer**, doppelklicken Sie auf das externe Storyboard aus, um ihn zur Bearbeitung in Xcode Interface Builder zu öffnen.
2. Fügen Sie eine neue Szene, und Entwerfen Sie des Layouts, wie gewohnt aus: 

    [![](indepth-images/ref07.png "Entwerfen des Layouts in Xcode")](indepth-images/ref07.png#lightbox)
3. In der **Identitätsinspektor**, geben Sie einen **Storyboard-ID** für der neuen Szene Fenstercontroller: 

    [![](indepth-images/ref08.png "Festlegen der Storyboard-ID")](indepth-images/ref08.png#lightbox)
3. Öffnen Sie das Storyboard, das auf den Verweis auf in Interface Builder hinzugefügt werden.
4. Ziehen Sie eine **Storyboard-Verweis** aus der **Object Library** auf die Entwurfsoberfläche: 

    [![](indepth-images/ref03.png "Wählen einen Storyboard-Verweis aus der Bibliothek")](indepth-images/ref03.png#lightbox)
5. In der **Identitätsinspektor**, wählen Sie den Namen des der **Storyboard** und **Referenz-ID** (Storyboard-ID) der Szene, die Sie soeben erstellt haben: 

    [![](indepth-images/ref09.png "Die Referenz-ID festlegen")](indepth-images/ref09.png#lightbox)
6. Strg + Klick auf ein UI-Widget (z. B. eine Schaltfläche) auf einer vorhandenen Szene, und erstellen Sie eine neue Segue, die **Storyboard Verweis** , die Sie gerade erstellt haben. Wählen Sie im Popupmenü **anzeigen** zum Abschließen der Segue: 

    [![](indepth-images/ref06.png "Segue-Typ")](indepth-images/ref06.png#lightbox) 
8. Speichern Sie die Änderungen auf das Storyboard an.
9. Zurück zu Visual Studio für Mac, um Ihre Änderungen zu synchronisieren.

Wenn die app ausführen und dem Benutzer klickt auf das Element der Benutzeroberfläche, die Sie aus, mit der Segue erstellt die angegebenen **Storyboard-ID** aus dem externen Storyboard in der Storyboard-Verweis angegeben wird angezeigt.

<a name="Referencing-a-Specific-Scene-in-the-Same-Storyboard" />

### <a name="referencing-a-specific-scene-in-the-same-storyboard"></a>Verweisen auf eine bestimmte Szene im gleichen Storyboard

Um einen Verweis auf eine bestimmte Szene das gleiche Storyboard hinzuzufügen, führen Sie folgende Schritte aus:

1. In der **Projektmappen-Explorer**, doppelklicken Sie auf das Storyboard, um es zur Bearbeitung zu öffnen.
2. Fügen Sie eine neue Szene, und Entwerfen Sie des Layouts, wie gewohnt aus: 

    [![](indepth-images/ref11.png "Bearbeiten das Storyboard in Xcode")](indepth-images/ref11.png#lightbox)
3. In der **Identitätsinspektor**, geben Sie einen **Storyboard-ID** für der neuen Szene Fenstercontroller: 

    [![](indepth-images/ref12.png "Festlegen der Storyboard-ID")](indepth-images/ref12.png#lightbox)
3. Ziehen Sie eine **Storyboard-Verweis** aus der **Toolbox** auf die Entwurfsoberfläche: 

    [![](indepth-images/ref03.png "Wählen einen Storyboard-Verweis aus der Bibliothek")](indepth-images/ref03.png#lightbox)
5. In **Attributinspektor**Option **Referenz-ID** (Storyboard-ID) der Szene, die Sie soeben erstellt haben: 

    [![](indepth-images/ref13.png "Die Referenz-ID festlegen")](indepth-images/ref13.png#lightbox)
6. Strg + Klick auf ein UI-Widget (z. B. eine Schaltfläche) auf einer vorhandenen Szene, und erstellen Sie eine neue Segue, die **Storyboard Verweis** , die Sie gerade erstellt haben. Wählen Sie im Popupmenü **anzeigen** zum Abschließen der Segue: 

    [![](indepth-images/ref06.png "Segue-Typ auswählen")](indepth-images/ref06.png#lightbox) 
8. Speichern Sie die Änderungen auf das Storyboard an.
9. Zurück zu Visual Studio für Mac, um Ihre Änderungen zu synchronisieren.

Wenn die app ausführen und dem Benutzer klickt auf das Element der Benutzeroberfläche, die Sie aus, mit der Segue erstellt die angegebenen **Storyboard-ID** im gleichen Storyboard in der Storyboard-Verweis angegeben wird angezeigt.

<a name="Complex-Storyboard-Example" />

## <a name="complex-storyboard-example"></a>Komplexe Storyboard-Beispiel

Ein komplexes Beispiel die Arbeit mit Storyboards in einer Xamarin.Mac-app, finden Sie unter den [SourceWriter-Beispiel-App](https://developer.xamarin.com/samples/mac/SourceWriter/). SourceWriter ist ein einfacher Quellcode-Editor, der Unterstützung für die Codevervollständigung und die einfache Syntaxhervorhebung bietet.

Der SourceWriter-Code wurde vollständig kommentiert und es wurden, wenn möglich, Links der Schlüsseltechnologien und -methoden zu wichtigen Informationen in den Leitfäden der Xamarin.Mac-Dokumentation bereitgestellt.

## <a name="related-links"></a>Verwandte Links

- [MacStoryboard (Beispiel)](https://developer.xamarin.com/samples/mac/MacStoryboard/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Arbeiten mit Windows](~/mac/user-interface/window.md)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Einführung in Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
