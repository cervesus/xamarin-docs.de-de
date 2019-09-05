---
title: Arbeiten mit Storyboards in xamarin. Mac
description: In diesem Dokument wird beschrieben, wie Sie mit Storyboards in xamarin. Mac arbeiten und wie Sie diese aus Code, dem Lebenszyklus des Ansichts Controllers, der Beantworter-Kette, den Segues, Fenster Controllern, Gesten Erkennungs Tools usw. laden.
ms.prod: xamarin
ms.assetid: DF4DF7C2-DDD7-4A32-B375-5C5446301EC5
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/14/2017
ms.openlocfilehash: 6aca181b2942bbde854df41c8f9741106cda6776
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70279308"
---
# <a name="working-with-storyboards-in-xamarinmac"></a>Arbeiten mit Storyboards in xamarin. Mac

Ein Storyboard definiert alle Benutzeroberflächen für eine bestimmte APP, die in einer funktionalen Übersicht der zugehörigen Ansichts Controller aufgeschlüsselt sind. In der Interface Builder von Xcode befindet sich jeder dieser Controller in einer eigenen Szene.

[![Ein Storyboard in der Interface Builder von Xcode](indepth-images/intro01.png)](indepth-images/intro01.png#lightbox)

Beim Storyboard handelt es sich um eine Ressourcen Datei (mit `.storyboard`der Erweiterung), die beim Kompilieren und versenden in das Paket der xamarin. Mac-app eingeschlossen wird. Um das Storyboard für die APP zu definieren, bearbeiten Sie `Info.plist` die Datei, und wählen Sie im Dropdown Feld die **Hauptschnittstelle** aus: 

[![Der Info. plist-Editor](indepth-images/sb01.png)](indepth-images/sb01.png#lightbox)

<a name="Loading-from-Code" />

## <a name="loading-from-code"></a>Laden aus Code

Es kann vorkommen, dass Sie ein bestimmtes Storyboard aus dem Code laden und einen Ansichts Controller manuell erstellen müssen. Sie können den folgenden Code verwenden, um diese Aktion auszuführen:

```csharp
// Get new window
var storyboard = NSStoryboard.FromName ("Main", null);
var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

// Display
controller.ShowWindow(this);
```

Das `FromName` lädt die storyboarddatei mit dem angegebenen Namen, der im Paket der App enthalten ist. `InstantiateControllerWithIdentifier` Erstellt eine Instanz des Ansichts Controllers mit der angegebenen Identität. Beim Entwerfen der Benutzeroberfläche wird die Identität in der Interface Builder von Xcode festgelegt:

[![Festlegen der Storyboard-ID](indepth-images/sb02.png)](indepth-images/sb02.png#lightbox)

Optional können Sie die `InstantiateInitialController` -Methode verwenden, um den Ansichts Controller zu laden, dem der anfängliche Controller in Interface Builder zugewiesen wurde:

[![Festlegen des ersten Controllers](indepth-images/sb03.png)](indepth-images/sb03.png#lightbox)

Sie wird durch den **Einstiegspunkt des Storyboards** und den obigen geöffneten Pfeil gekennzeichnet.

<a name="View-Controllers" />

## <a name="view-controllers"></a>Controller anzeigen

Ansichts Controller definieren die Beziehungen zwischen einer bestimmten Ansicht von Informationen in einer Mac-app und dem Datenmodell, das diese Informationen bereitstellt. Jede Szene der obersten Ebene im Storyboard stellt einen Ansichts Controller im Code der xamarin. Mac-app dar.

<a name="The-View-Controller-Lifecycle" />

### <a name="the-view-controller-lifecycle"></a>Der Lebenszyklus des Ansichts Controllers

Der `NSViewController` -Klasse wurden mehrere neue Methoden hinzugefügt, um Storyboards in macOS zu unterstützen. Am wichtigsten ist, dass die folgenden Methoden verwenden, um auf den Lebenszyklus der Ansicht zu reagieren, die vom angegebenen Ansichts Controller gesteuert wird:

- `ViewDidLoad`-Diese Methode wird aufgerufen, wenn die Ansicht aus der Storyboard-Datei geladen wird.
- `ViewWillAppear`-Diese Methode wird aufgerufen, kurz bevor die Ansicht auf dem Bildschirm angezeigt wird.
- `ViewDidAppear`-Diese Methode wird direkt aufgerufen, nachdem die Ansicht auf dem Bildschirm angezeigt wurde.
- `ViewWillDisappear`-Diese Methode wird aufgerufen, kurz bevor die Ansicht vom Bildschirm entfernt wird.
- `ViewDidDisappear`-Diese Methode wird direkt aufgerufen, nachdem die Ansicht vom Bildschirm entfernt wurde.
- `UpdateViewConstraints`-Diese Methode wird aufgerufen, wenn die Einschränkungen, die eine Ansicht für die automatische Layoutposition und-Größe definieren, aktualisiert werden müssen.
- `ViewWillLayout`-Diese Methode wird aufgerufen, kurz bevor die untergeordneten Sichten dieser Ansicht auf dem Bildschirm angeordnet werden.
- `ViewDidLayout`-Diese Methode wird direkt aufgerufen, nachdem die unter Sichten der Ansicht auf dem Bildschirm angeordnet wurden.

<a name="The-Responder-Chain" />

### <a name="the-responder-chain"></a>Die Responder-Kette

Außerdem sind Sie nun Teil der _Responder-Kette_des Fensters: `NSViewControllers`

[![Die Responder-Kette](indepth-images/vc01.png)](indepth-images/vc01.png#lightbox)

Daher sind Sie miteinander verknüpft, um Ereignisse wie Ausschneiden, kopieren und Einfügen (Menü Elementauswahl) zu empfangen und darauf zu reagieren. Diese automatische Ansichts Controller Verbindung tritt nur bei apps auf, die auf macOS Sierra (10,12) und höher ausgeführt werden.

<a name="Containment" />

### <a name="containment"></a>Kapselung

In Storyboards können Ansichts Controller (z. b. der Split View Controller und der Registerkarten Ansichts Controller) nun die Kapselung _implementieren,_ sodass Sie andere Sub-View-Controller enthalten können:

[![Ein Beispiel für die Sicht Controller Kapselung](indepth-images/vc02.png)](indepth-images/vc02.png#lightbox)

Untergeordnete Ansichts Controller enthalten Methoden und Eigenschaften, um Sie zurück an ihren übergeordneten Ansichts Controller zu binden und um mit dem anzeigen und Entfernen von Ansichten auf dem Bildschirm zu arbeiten.

Alle in macOS integrierten Container Ansichts Controller verfügen über ein bestimmtes Layout, das Apple vorschlägt, wenn Sie Ihre eigenen benutzerdefinierten Container Ansichts Controller erstellen:

[![Das Ansichts Controller Layout](indepth-images/vc03.png)](indepth-images/vc03.png#lightbox)

Der Sammlungs Ansichts Controller enthält ein Array von Auflistungs Ansichts Elementen, von denen jedes einen oder mehrere Ansichts Controller enthält, die eigene Sichten enthalten.

<a name="Segues" />

## <a name="segues"></a>Segues

Die Beziehungen zwischen den Kulissen, in denen die Benutzeroberfläche Ihrer APP definiert ist, werden durch die Beziehungen bereitgestellt. Wenn Sie mit der Arbeit in Storyboards in ios vertraut sind, wissen Sie, dass die Seiten für IOS in der Regel Übergänge zwischen den voll Bildansichten definieren. Dies unterscheidet sich von macOS, wenn in der Regel "[Containment](#Containment)" definiert wird, wobei eine Szene das untergeordnete Element einer übergeordneten Szene ist.

Unter macOS gruppieren die meisten apps ihre Ansichten tendenziell innerhalb desselben Fensters mithilfe von Benutzeroberflächen Elementen wie z. b. geteilten Sichten und Registerkarten. Anders als bei IOS, bei dem Ansichten aufgrund von eingeschränktem physischem Anzeigebereich ein-und ausgeschaltet werden müssen.

<a name="Presentation-Segues" />

### <a name="presentation-segues"></a>Präsentations-Segues

Bei den Tendenzen von macOS in Bezug auf die _Kapselung_ gibt es Situationen, in denen Präsentationsseiten verwendet werden, wie z. b. modale Fenster, Blatt Ansichten und popovers. macOS bietet die folgenden integrierten Enumerationstypen:

- **Anzeigen** : zeigt das Ziel des-Ziels als nicht modales Fenster an. Verwenden Sie z. b. diesen Typ von "log", um eine weitere Instanz eines Dokument Fensters in der APP darzustellen.
- **Modal** : zeigt das Ziel des-Ziels als modales Fenster an. Verwenden Sie z. b. diesen Typ von "log", um das Fenster "Einstellungen" für Ihre APP darzustellen.
- **Sheet** -zeigt das Ziel des-Objekts als ein Blatt an, das an das übergeordnete Fenster angefügt ist. Verwenden Sie z. b. diesen Typ von "log", um ein Blatt "suchen und ersetzen" darzustellen.
- **Popover** : zeigt das Ziel des segue als in einem popover-Fenster an. Verwenden Sie z. b. diesen Typ, um Optionen zu präsentieren, wenn der Benutzer auf ein Benutzeroberflächen Element klickt.
- **Custom** : zeigt das Ziel des-Ziels mit einem vom Entwickler definierten benutzerdefinierten Typ an. Weitere Informationen finden Sie weiter unten im Abschnitt [Erstellen von benutzerdefinierten](#Creating-Custom-Segues) Abschnitten.

Wenn Sie präsentationsegues verwenden, können Sie `PrepareForSegue` die-Methode des übergeordneten Ansichts Controllers überschreiben, um die Präsentation zu initialisieren und Variablen zu erstellen und dem Ansichts Controller alle Daten bereitzustellen.

<a name="Triggered-Segues" />

### <a name="triggered-segues"></a>Ausgelöste Trigger

Ausgelöste Segues ermöglichen es Ihnen, benannte Segues (über deren **Bezeichnereigenschaft** in Interface Builder) anzugeben und Sie durch Ereignisse wie den Benutzer, der auf eine Schaltfläche klickt `PerformSegue` , oder durch Aufrufen der-Methode im Code auszulösen:

```csharp
// Display the Scene defined by the given Segue ID
PerformSegue("MyNamedSegue", this);
``` 

Die segue-ID wird in der Interface Builder von Xcode definiert, wenn Sie die Benutzeroberfläche der APP festlegen:

[![Eingeben eines Namens für den Namen](indepth-images/sg02.png)](indepth-images/sg02.png#lightbox)

Im Ansichts Controller, der als Quelle des segue fungiert, sollten Sie die `PrepareForSegue` -Methode überschreiben und jede Initialisierung durchführen, die vor der Ausführung von Segue erforderlich ist, und der angegebene Ansichts Controller wird angezeigt:

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

Optional können Sie die `ShouldPerformSegue` -Methode überschreiben und Steuern, ob der segue tatsächlich über C# Code ausgeführt wird. Aufrufen Sie für manuell dargestellte Ansichts `DismissController` Controller ihre-Methode, um Sie aus der Anzeige zu entfernen, wenn Sie nicht mehr benötigt werden.

<a name="Creating-Custom-Segues" />

### <a name="creating-custom-segues"></a>Erstellen von benutzerdefinierten.

Es kann vorkommen, dass Ihre APP einen "*"-Typ erfordert, der nicht von den in macOS definierten Build-in-Standpunkten bereitgestellt wird. Wenn dies der Fall ist, können Sie einen benutzerdefinierten segue erstellen, der in der Interface Builder von Xcode zugewiesen werden kann, wenn Sie die Benutzeroberfläche der APP anordnen.

Um z. b. einen neuen segue-Typ zu erstellen, der den aktuellen Ansichts Controller innerhalb eines Fensters ersetzt (anstatt die zielszene in einem neuen Fenster zu öffnen), können wir den folgenden Code verwenden:

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

Hier sind einige Punkte zu beachten:

- Wir verwenden das `Register` -Attribut, um diese Klasse für "Ziel-C/macOS" verfügbar zu machen.
- Wir überschreiben die `Perform` -Methode, um die Aktion des benutzerdefinierten-Angs tatsächlich auszuführen.
- Wir ersetzen den `ContentViewController` Controller des Fensters durch den, der durch das Ziel (Ziel) des-Ziels definiert wird.
- Wir entfernen den ursprünglichen Ansichts Controller, um Speicher mithilfe der `RemoveFromParentViewController` -Methode freizugeben.

Um diesen neuen segue-Typ in der Interface Builder von Xcode zu verwenden, müssen wir zuerst die APP kompilieren, dann zu Xcode wechseln und einen neuen segue zwischen zwei Kulissen hinzufügen. Legen Sie den **Stil** auf **Custom** und die **segue-Klasse** auf `ReplaceViewSegue` (den Namen der benutzerdefinierten segue-Klasse) fest:

[![Festlegen der Klasse "Klasse"](indepth-images/sg01.png)](indepth-images/sg01.png#lightbox)

<a name="Triggered-Segues" />

## <a name="window-controllers"></a>Fenster Controller

Fenster Controller enthalten und steuern die unterschiedlichen Fenstertypen, die von ihrer macOS-App erstellt werden können. Für Storyboards verfügen Sie über die folgenden Features:

1. Sie müssen einen Inhalts Ansichts Controller bereitstellen. Dabei handelt es sich um denselben Inhalts Ansichts Controller, den das untergeordnete Fenster besitzt.
2. Die `Storyboard` -Eigenschaft enthält das Storyboard, aus dem der Fenster Controller geladen wurde `null` , andernfalls, wenn es nicht von einem Storyboard geladen wird.
3. Sie können die `DismissController` -Methode aufzurufen, um das angegebene Fenster zu schließen und aus der Ansicht zu entfernen.

Wie Ansichts Controller implementieren Fenster Controller die `PerformSegue`- `PrepareForSegue` Methode und `ShouldPerformSegue` die-Methode, und Sie können als Quelle für einen-Vorgang verwendet werden.

Der Fenster Controller ist für die folgenden Funktionen einer macOS-App verantwortlich:

- Sie verwalten ein bestimmtes Fenster.
- Sie verwalten die Titelleiste und Symbolleiste des Fensters (falls verfügbar).
- Sie verwalten den Content View Controller, um den Inhalt des Fensters anzuzeigen.

<a name="Gesture-Recognizers" />

## <a name="gesture-recognizers"></a>Gesten Erkennungs Tools

Gesten erkentungen für macOS sind fast identisch mit ihren Gegenstücken in IOS und ermöglichen es dem Entwickler, einfach Gesten (z. b. das Klicken auf eine Maustaste) auf Elemente in der Benutzeroberfläche Ihrer APP hinzuzufügen.

Wenn Gesten in ios jedoch durch das Design der APP bestimmt werden (z. b. das Tippen auf den Bildschirm mit zwei Fingern), werden die meisten Gesten in macOS durch Hardware bestimmt.

Mithilfe von Gesten Erkennungs Tools können Sie die Menge des Codes, der zum Hinzufügen benutzerdefinierter Interaktionen zu einem Element in der Benutzeroberfläche erforderlich ist, erheblich reduzieren. Da Sie automatisch zwischen doppelten und einfachen Klicks ermitteln können, klicken und ziehen Sie Ereignisse usw.

Anstatt das `MouseDown` Ereignis in Ihrem Ansichts Controller zu überschreiben, sollten Sie beim Arbeiten mit Storyboards eine Gestenerkennung verwenden, um das Benutzereingabe Ereignis zu behandeln.

Die folgenden Gesten Erkennungs Tools sind in macOS verfügbar:

- `NSClickGestureRecognizer`: Hiermit werden MouseDown-und aufwärts Ereignisse registriert.
- `NSPanGestureRecognizer`: Hiermit werden MouseButton-, Drag-und releaseereignisse registriert.
- `NSPressGestureRecognizer`: Registriert das halten einer Maustaste für eine bestimmte Zeitspanne.
- `NSMagnificationGestureRecognizer`: Registriert ein Vergrößerungs Ereignis von der Trackpad-Hardware.
- `NSRotationGestureRecognizer`: Registriert ein Rotations Ereignis von der Trackpad-Hardware.

<a name="Using-Storyboard-References" />

## <a name="using-storyboard-references"></a>Verwenden von Storyboard-verweisen

Mithilfe eines storyboardverweises können Sie einen großen, komplexen Storyboard-Entwurf erstellen und in kleinere Storyboards unterteilen, auf die von der ursprünglichen verwiesen wird. Dadurch können Sie die Komplexität entfernen und die resultierenden einzelnen Storyboards leichter entwerfen und warten.

Außerdem kann eine storyboardreferenz einen _Anker_ für eine andere Szene innerhalb desselben Storyboards oder eine bestimmte Szene auf einer anderen Szene bereitstellen.

<a name="Referencing-an-External-Storyboard" />

### <a name="referencing-an-external-storyboard"></a>Verweisen auf ein externes Storyboard

Gehen Sie folgendermaßen vor, um einen Verweis auf ein externes Storyboard hinzuzufügen:

1. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf den Projektnamen, und wählen Sie**neue Datei** **Hinzufügen** > ... aus.  > Mac- > **Storyboard**. Geben Sie einen **Namen** für das neue Storyboard ein, und klicken Sie auf die Schaltfläche **neu** : 

    [![Hinzufügen eines neuen Storyboards](indepth-images/ref01.png)](indepth-images/ref01.png#lightbox)
2. Doppelklicken Sie im **Projektmappen-Explorer**auf den neuen Storyboardnamen, um ihn für die Bearbeitung in der Interface Builder von Xcode zu öffnen.
3. Entwerfen Sie das Layout der neuen Storyboard-Szenen wie gewohnt, und speichern Sie die Änderungen: 

    [![Entwerfen der Schnittstelle](indepth-images/ref02.png)](indepth-images/ref02.png#lightbox)
4. Wechseln Sie in das Storyboard, dem Sie den Verweis hinzufügen möchten, in der Interface Builder.
5. Ziehen Sie einen **storyboardverweis** aus der **Objektbibliothek** auf den Designoberfläche: 

    [![Auswählen eines storyboardverweises in der Bibliothek](indepth-images/ref03.png)](indepth-images/ref03.png#lightbox)
6. Wählen Sie im **Attribut Inspektor**den Namen des **Storyboards** aus, das Sie oben erstellt haben: 

    [![Konfigurieren des Verweises](indepth-images/ref04.png)](indepth-images/ref04.png#lightbox)
7. Klicken Sie mit der Maus auf ein UI-Widget (z. b. eine Schaltfläche) in einer vorhandenen Szene, und erstellen Sie eine neue Tabelle für den soeben erstellten **storyboardverweis** .  Wählen Sie im Popupmenü die Option **anzeigen** aus, um den Vorgang abzuschließen: 

    [![Festlegen des Typs "Typ"](indepth-images/ref06.png)](indepth-images/ref06.png#lightbox) 
8. Speichern Sie die Änderungen am Storyboard.
9. Kehren Sie zu Visual Studio für Mac zurück, um die Änderungen zu synchronisieren.

Wenn die app ausgeführt wird und der Benutzer auf das Benutzeroberflächen Element klickt, das Sie aus dem-Element erstellt haben, wird der anfängliche Fenster Controller aus dem externen Storyboard angezeigt, das in der storyboardreferenz angegeben ist.

<a name="Referencing-a-Specific-Scene-in-an-External-Storyboard" />

### <a name="referencing-a-specific-scene-in-an-external-storyboard"></a>Verweisen auf eine bestimmte Szene in einem externen Storyboard

Gehen Sie folgendermaßen vor, um einen Verweis auf eine bestimmte Szene zu einem externen Storyboard (und nicht zum anfänglichen Fenster Controller) hinzuzufügen:

1. Doppelklicken Sie im **Projektmappen-Explorer**auf das externe Storyboard, um es für die Bearbeitung in der Interface Builder von Xcode zu öffnen.
2. Fügen Sie eine neue Szene hinzu, und entwerfen Sie das Layout wie gewohnt: 

    [![Entwerfen des Layouts in Xcode](indepth-images/ref07.png)](indepth-images/ref07.png#lightbox)
3. Geben Sie im **Identitäts Inspektor**eine **Storyboard-ID** für den Fenster Controller der neuen Szene ein: 

    [![Festlegen der Storyboard-ID](indepth-images/ref08.png)](indepth-images/ref08.png#lightbox)
4. Öffnen Sie das Storyboard, dem Sie den Verweis hinzufügen möchten, in Interface Builder.
5. Ziehen Sie einen **storyboardverweis** aus der **Objektbibliothek** auf den Designoberfläche: 

    [![Auswählen eines storyboardverweises aus der Bibliothek](indepth-images/ref03.png)](indepth-images/ref03.png#lightbox)
6. Wählen Sie im **Identitäts Inspektor**den Namen des **Storyboards** und die Verweis- **ID** (Storyboard-ID) der Szene aus, die Sie oben erstellt haben: 

    [![Festlegen der Verweis-ID](indepth-images/ref09.png)](indepth-images/ref09.png#lightbox)
7. Klicken Sie mit der Maus auf ein UI-Widget (z. b. eine Schaltfläche) in einer vorhandenen Szene, und erstellen Sie eine neue Tabelle für den soeben erstellten **storyboardverweis** . Wählen Sie im Popupmenü die Option **anzeigen** aus, um den Vorgang abzuschließen: 

    [![Festlegen des Typs "Typ"](indepth-images/ref06.png)](indepth-images/ref06.png#lightbox) 
8. Speichern Sie die Änderungen am Storyboard.
9. Kehren Sie zu Visual Studio für Mac zurück, um die Änderungen zu synchronisieren.

Wenn die app ausgeführt wird und der Benutzer auf das Benutzeroberflächen Element klickt, das Sie in erstellt haben, wird die Szene mit der angegebenen **Storyboard-ID** aus dem externen Storyboard angezeigt, das in der storyboardreferenz angegeben ist.

<a name="Referencing-a-Specific-Scene-in-the-Same-Storyboard" />

### <a name="referencing-a-specific-scene-in-the-same-storyboard"></a>Verweisen auf eine bestimmte Szene im gleichen Storyboard

Gehen Sie folgendermaßen vor, um einem Storyboard einen Verweis auf eine bestimmte Szene hinzuzufügen:

1. Doppelklicken Sie im **Projektmappen-Explorer**auf das Storyboard, um es zur Bearbeitung zu öffnen.
2. Fügen Sie eine neue Szene hinzu, und entwerfen Sie das Layout wie gewohnt: 

    [![Bearbeiten des Storyboards in Xcode](indepth-images/ref11.png)](indepth-images/ref11.png#lightbox)
3. Geben Sie im **Identitäts Inspektor**eine **Storyboard-ID** für den Fenster Controller der neuen Szene ein: 

    [![Festlegen der Storyboard-ID](indepth-images/ref12.png)](indepth-images/ref12.png#lightbox)
4. Ziehen Sie einen **storyboardverweis** aus der **Toolbox** auf den Designoberfläche: 

    [![Auswählen eines storyboardverweises aus der Bibliothek](indepth-images/ref03.png)](indepth-images/ref03.png#lightbox)
5. Wählen Sie in **Attribut Inspektor**die **Verweis-ID** (Storyboard-ID) der Szene aus, die Sie oben erstellt haben: 

    [![Festlegen der Verweis-ID](indepth-images/ref13.png)](indepth-images/ref13.png#lightbox)
6. Klicken Sie mit der Maus auf ein UI-Widget (z. b. eine Schaltfläche) in einer vorhandenen Szene, und erstellen Sie eine neue Tabelle für den soeben erstellten **storyboardverweis** . Wählen Sie im Popupmenü die Option **anzeigen** aus, um den Vorgang abzuschließen: 

    [![Auswählen des Typs "Typ"](indepth-images/ref06.png)](indepth-images/ref06.png#lightbox) 
7. Speichern Sie die Änderungen am Storyboard.
8. Kehren Sie zu Visual Studio für Mac zurück, um die Änderungen zu synchronisieren.

Wenn die app ausgeführt wird und der Benutzer auf das Benutzeroberflächen Element klickt, das Sie aus erstellt haben, wird die Szene mit der angegebenen **Storyboard-ID** im gleichen Storyboard angezeigt, das in der storyboardreferenz angegeben ist.

<a name="Complex-Storyboard-Example" />

## <a name="complex-storyboard-example"></a>Beispiel für ein komplexes Storyboard

Ein komplexes Beispiel für das Arbeiten mit Storyboards in einer xamarin. Mac-app finden Sie in der [sourcewriter-Beispiel-App](https://docs.microsoft.com/samples/xamarin/mac-samples/sourcewriter). SourceWriter ist ein einfacher Quellcode-Editor, der Unterstützung für die Codevervollständigung und die einfache Syntaxhervorhebung bietet.

Der SourceWriter-Code wurde vollständig kommentiert und es wurden, wenn möglich, Links der Schlüsseltechnologien und -methoden zu wichtigen Informationen in den Leitfäden der Xamarin.Mac-Dokumentation bereitgestellt.

## <a name="related-links"></a>Verwandte Links

- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Arbeiten mit Windows](~/mac/user-interface/window.md)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Einführung in Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
