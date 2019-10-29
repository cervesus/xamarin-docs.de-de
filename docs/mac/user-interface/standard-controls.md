---
title: Standard Steuerelemente in xamarin. Mac
description: In diesem Artikel wird erläutert, wie Sie mit den standardmäßigen AppKit-Steuerelementen wie Schaltflächen, Bezeichnungen, Textfelder, Kontrollkästchen und segmentierten Steuerelementen in einer xamarin. Mac-Anwendung arbeiten. Es beschreibt das Hinzufügen zu einer Schnittstelle mit Interface Builder und die Interaktion mit Ihnen im Code.
ms.prod: xamarin
ms.assetid: d2593883-d255-431f-9781-75f04d8cecea
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: fe887d837930ebc75fed0fb7c163a3f30ad83af9
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73008371"
---
# <a name="standard-controls-in-xamarinmac"></a>Standard Steuerelemente in xamarin. Mac

_In diesem Artikel wird erläutert, wie Sie mit den standardmäßigen AppKit-Steuerelementen wie Schaltflächen, Bezeichnungen, Textfelder, Kontrollkästchen und segmentierten Steuerelementen in einer xamarin. Mac-Anwendung arbeiten. Es beschreibt das Hinzufügen zu einer Schnittstelle mit Interface Builder und die Interaktion mit Ihnen im Code._

Wenn Sie mit C# und .net in einer xamarin. Mac-Anwendung arbeiten, haben Sie Zugriff auf die gleichen AppKit-Steuerelemente, die ein Entwickler in *Ziel-C* und *Xcode* verwendet. Da xamarin. Mac direkt in Xcode integriert ist, können Sie die _Interface Builder_ von Xcode verwenden, um Ihre AppKit-Steuerelemente zu erstellen und zu verwalten C# (oder Sie optional direkt im Code zu erstellen).

AppKit-Steuerelemente sind die Benutzeroberflächen Elemente, die verwendet werden, um die Benutzeroberfläche der xamarin. Mac-Anwendung zu erstellen. Sie bestehen aus Elementen wie Schaltflächen, Bezeichnungen, Text Feldern, Kontrollkästchen und segmentierten Steuerelementen und bewirken sofortige Aktionen oder sichtbare Ergebnisse, wenn ein Benutzer Sie bearbeitet.

[![](standard-controls-images/intro01.png "The example app main screen")](standard-controls-images/intro01.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit AppKit-Steuerelementen in einer xamarin. Mac-Anwendung behandelt. Es wird dringend empfohlen, dass Sie zunächst den Artikel [Hello, Mac](~/mac/get-started/hello-mac.md) , insbesondere die [Einführung in Xcode und die Abschnitte zu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und Outlets und [Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) , verwenden, da er wichtige Konzepte und Techniken behandelt, die wir in verwenden werden. Dieser Artikel.

Sie können sich auch den Abschnitt verfügbar machen von [ C# Klassen/Methoden zu "Ziel-c](~/mac/internals/how-it-works.md) " im Dokument " [xamarin. Mac](~/mac/internals/how-it-works.md) " ansehen. darin werden die`Register`und`Export`Befehle erläutert, die zum Verknüpfen der C# Klassen mit "Ziel-c" verwendet werden. Objekte und UI-Elemente.

<a name="Introduction_to_Controls_and_Views" />

## <a name="introduction-to-controls-and-views"></a>Einführung in Steuerelemente und Ansichten

macOS (früher als Mac OS X bezeichnet) stellt einen Standardsatz von Steuerelementen für die Benutzeroberfläche über das AppKit-Framework bereit. Sie bestehen aus Elementen wie Schaltflächen, Bezeichnungen, Text Feldern, Kontrollkästchen und segmentierten Steuerelementen und bewirken sofortige Aktionen oder sichtbare Ergebnisse, wenn ein Benutzer Sie bearbeitet.

Alle AppKit-Steuerelemente verfügen über eine integrierte Standarddarstellung, die für die meisten Verwendungszwecke geeignet ist. einige geben eine Alternative Darstellung für die Verwendung in einem Fensterrahmen Bereich oder in einem _Vibrations Effekt_ Kontext an, z. b. in einem Rand Leistenbereich oder in einem Benachrichtigungs Center. Program.

Apple empfiehlt beim Arbeiten mit AppKit-Steuerelementen die folgenden Richtlinien:

- Vermeiden Sie das Mischen von Steuerelement Größen in derselben Ansicht.
- Vermeiden Sie im Allgemeinen das vertikale Ändern der Größe von Steuerelementen.
- Verwenden Sie die System Schriftart und die richtige Textgröße innerhalb eines-Steuer Elements.
- Verwenden Sie den richtigen Abstand zwischen Steuerelementen.

Weitere Informationen finden Sie im Abschnitt [über Steuerelemente und Ansichten](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1) von Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

<a name="Using_Controls_in_a_Window_Frame" />

### <a name="using-controls-in-a-window-frame"></a>Verwenden von Steuerelementen in einem Fensterrahmen

Es gibt eine Teilmenge von AppKit-Steuerelementen, die einen Anzeige Stil enthalten, mit dem Sie in den Rahmen Bereich eines Fensters eingeschlossen werden können. Ein Beispiel finden Sie auf der Symbolleiste der Mail-App:

[![](standard-controls-images/mailapp.png "A Mac Window frame")](standard-controls-images/mailapp.png#lightbox)

- **Umschaufene texttaste** -ein `NSButton` mit einem Stil von `NSTexturedRoundedBezelStyle`.
- **Textured abgerundetes segmentiertes Steuer** Element: ein `NSSegmentedControl` mit einem Stil von `NSSegmentStyleTexturedRounded`.
- **Textured abgerundetes segmentiertes Steuer** Element: ein `NSSegmentedControl` mit einem Stil von `NSSegmentStyleSeparated`.
- **Umschlendetes Popup Menü,** ein `NSPopUpButton` mit einer Art `NSTexturedRoundedBezelStyle`.
- **Dropdown Menü** "in der Dropdown Liste", ein `NSPopUpButton` mit einer Art `NSTexturedRoundedBezelStyle`.
- **Suchleiste** -A `NSSearchField`.

Apple empfiehlt beim Arbeiten mit AppKit-Steuerelementen in einem Fensterrahmen die folgenden Richtlinien:

- Verwenden Sie keine Fensterrahmen spezifischen Steuerelement Stile im Fenster Körper.
- Verwenden Sie keine Fenster Text-Steuerelemente oder-Stile im Fensterrahmen.

Weitere Informationen finden Sie im Abschnitt [über Steuerelemente und Ansichten](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1) von Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

<a name="Creating_a_User_Interface_in_Interface_Builder" />

## <a name="creating-a-user-interface-in-interface-builder"></a>Erstellen einer Benutzeroberfläche in Interface Builder

Wenn Sie eine neue xamarin. Mac-Cocoa-Anwendung erstellen, erhalten Sie standardmäßig ein Standardmäßiges leeres Standardfenster. Dieses Fenster wird in einer `.storyboard` Datei definiert, die automatisch im Projekt enthalten ist. Um den Windows-Entwurf zu bearbeiten, doppelklicken Sie im **Projektmappen-Explorer**auf die `Main.storyboard` Datei:

[![](standard-controls-images/edit01.png "Selecting the Main Storyboard in the Solution Explorer")](standard-controls-images/edit01.png#lightbox)

Dadurch wird das Fensterdesign in der Interface Builder von Xcode geöffnet:

[![](standard-controls-images/edit02.png "Editing the storyboard in Xcode")](standard-controls-images/edit02.png#lightbox)

Um die Benutzeroberfläche zu erstellen, ziehen Sie UI-Elemente (AppKit-Steuerelemente) aus der **Bibliothek Inspektor** in den- **Schnittstellen-Editor** in Interface Builder. Im folgenden Beispiel wurde ein **vertikales geteiltes Ansichts** Steuerelement vom **Library Inspector** als Droge und im-Schnittstellen- **Editor**im Fenster abgelegt:

[![](standard-controls-images/edit03.png "Selecting a Split View from the Library")](standard-controls-images/edit03.png#lightbox)

Weitere Informationen zum Erstellen einer Benutzeroberfläche in Interface Builder finden Sie in der [Einführung in Xcode und Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) Dokumentation.

<a name="Sizing_and_Positioning" />

### <a name="sizing-and-positioning"></a>Größenanpassung und Positionierung

Wenn ein Steuerelement in der Benutzeroberfläche enthalten ist, verwenden Sie den Einschränkungs- **Editor** , um seine Position und Größe festzulegen, indem Sie Werte manuell eingeben und Steuern, wie das Steuerelement automatisch positioniert und angepasst wird, wenn die Größe des übergeordneten Fensters oder der Ansicht geändert wird:

[![](standard-controls-images/edit04.png "Setting the constraints")](standard-controls-images/edit04.png#lightbox)

Verwenden Sie den **roten I-Balken** um die außerhalb des **autoresisierungsfelds** , um ein Steuerelement an einem angegebenen (x, y) Speicherort zu _halten_ . Beispiel: 

[![](standard-controls-images/edit05.png "Editing a constraint")](standard-controls-images/edit05.png#lightbox)

Gibt an, dass das ausgewählte Steuerelement (in der **Hierarchie Ansicht** & Schnittstellen- **Editor**) an der oberen und rechten Position des Fensters oder der Ansicht hängen bleibt, wenn die Größe geändert oder verschoben wird. 

Andere Elemente der Editor-Steuerelement Eigenschaften, z. b. Höhe und Breite:

[![](standard-controls-images/edit06.png "Setting the height")](standard-controls-images/edit06.png#lightbox)

Mit dem **Ausrichtungs-Editor**können Sie auch die Ausrichtung von Elementen mit Einschränkungen steuern:

[![](standard-controls-images/edit07.png "The Alignment Editor")](standard-controls-images/edit07.png#lightbox)

> [!IMPORTANT]
> Anders als bei IOS, wobei (0,0) die linke obere Ecke des Bildschirms ist, ist in macOS (0,0) die untere linke Ecke. Dies liegt daran, dass macOS ein mathematisches Koordinatensystem verwendet, bei dem die Zahlenwerte im Wert nach oben und nach rechts steigen. Dies müssen Sie berücksichtigen, wenn Sie AppKit-Steuerelemente auf einer Benutzeroberfläche platzieren.

<a name="Setting_a_Custom_Class" />

### <a name="setting-a-custom-class"></a>Festlegen einer benutzerdefinierten Klasse

Es gibt Zeiten, in denen Sie mit AppKit-Steuerelementen arbeiten, die Sie benötigen, um untergeordnete und vorhandene Steuerelemente zu Unterklassen und zu erstellen. Definieren Sie beispielsweise eine benutzerdefinierte Version der Quell Liste:

```csharp
using System;
using AppKit;
using Foundation;

namespace AppKit
{
    [Register("SourceListView")]
    public class SourceListView : NSOutlineView
    {
        #region Computed Properties
        public SourceListDataSource Data {
            get {return (SourceListDataSource)this.DataSource; }
        }
        #endregion

        #region Constructors
        public SourceListView ()
        {

        }

        public SourceListView (IntPtr handle) : base(handle)
        {

        }

        public SourceListView (NSCoder coder) : base(coder)
        {

        }

        public SourceListView (NSObjectFlag t) : base(t)
        {

        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

        }
        #endregion

        #region Public Methods
        public void Initialize() {

            // Initialize this instance
            this.DataSource = new SourceListDataSource (this);
            this.Delegate = new SourceListDelegate (this);

        }

        public void AddItem(SourceListItem item) {
            if (Data != null) {
                Data.Items.Add (item);
            }
        }
        #endregion

        #region Events
        public delegate void ItemSelectedDelegate(SourceListItem item);
        public event ItemSelectedDelegate ItemSelected;

        internal void RaiseItemSelected(SourceListItem item) {
            // Inform caller
            if (this.ItemSelected != null) {
                this.ItemSelected (item);
            }
        }
        #endregion
    }
}
```

Wenn die `[Register("SourceListView")]`-Anweisung die `SourceListView` Klasse für "Ziel-C" verfügbar macht, sodass in Interface Builder verwendet werden kann. Weitere Informationen finden Sie im Abschnitt "verfügbar machen von [ C# Klassen/Methoden zu Ziel-c](~/mac/internals/how-it-works.md) " im Dokument " [xamarin. Mac Internals](~/mac/internals/how-it-works.md) ". darin werden die`Register`-und`Export`Befehle erläutert, C# die zum Verknüpfen der Klassen mit "Ziel-c" verwendet werden. Objekte und UI-Elemente.

Wenn der obige Code vorhanden ist, können Sie ein AppKit-Steuerelement des Basistyps, den Sie erweitern, auf die Entwurfs Oberfläche ziehen (im folgenden Beispiel eine **Quell Liste**), zum **Identitäts Inspektor** wechseln und die **benutzerdefinierte Klasse** auf den Namen festlegen, der Sie haben sich für "Ziel-C" (Beispiel `SourceListView`) verfügbar gemacht:

[![](standard-controls-images/edit10.png "Setting a custom class in Xcode")](standard-controls-images/edit10.png#lightbox)

<a name="Exposing_Outlets_and_Actions" />

### <a name="exposing-outlets-and-actions"></a>Verfügbar machen von Outlets und Aktionen

Bevor auf ein AppKit-Steuerelement im C# Code zugegriffen werden kann, muss es entweder als ein **Outlet** oder eine **Aktion**verfügbar gemacht werden. Wählen Sie hierzu das angegebene Steuerelement entweder in der **Schnittstellen Hierarchie** oder im Schnittstellen- **Editor** aus, und wechseln Sie zur **Ansicht "Assistant** " (stellen Sie sicher, dass die `.h` des Fensters zum Bearbeiten ausgewählt ist):

[![](standard-controls-images/edit11.png "Selecting the correct file to edit")](standard-controls-images/edit11.png#lightbox)

Steuerelement: ziehen Sie aus dem AppKit-Steuerelement auf die Datei "Give `.h`", um mit dem Erstellen eines **Outlets** oder einer **Aktion**

[![](standard-controls-images/edit12.png "Dragging to create an Outlet or Action")](standard-controls-images/edit12.png#lightbox)

Wählen Sie den Typ der zu erstellenden Übersetzung aus, und geben Sie dem **Outlet** oder der **Aktion** einen **Namen** 

[![](standard-controls-images/edit13.png "Configuring the Outlet or Action")](standard-controls-images/edit13.png#lightbox)

Weitere Informationen zum Arbeiten mit **Outlets** und **Aktionen**finden Sie im Abschnitt [Outlets und Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) in unserer [Einführung in Xcode und Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) Dokumentation.

<a name="Synchronizing_Changes_with_Xcode" />

### <a name="synchronizing-changes-with-xcode"></a>Synchronisieren von Änderungen mit Xcode

Wenn Sie von Xcode zurück zu Visual Studio für Mac wechseln, werden alle Änderungen, die Sie in Xcode vorgenommen haben, automatisch mit Ihrem xamarin. Mac-Projekt synchronisiert.

Wenn Sie die `SplitViewController.designer.cs` in der **Projektmappen-Explorer** auswählen, können Sie sehen, wie Ihr **Outlet** und Ihre **Aktion** in unserem C# Code verknüpft sind:

[![](standard-controls-images/sync01.png "Synchronizing Changes with Xcode")](standard-controls-images/sync01.png#lightbox)

Beachten Sie, wie die Definition in der `SplitViewController.designer.cs`-Datei:

```csharp
[Outlet]
AppKit.NSSplitViewItem LeftController { get; set; }

[Outlet]
AppKit.NSSplitViewItem RightController { get; set; }

[Outlet]
AppKit.NSSplitView SplitView { get; set; }
```

Richten Sie die Definition in der `MainWindow.h`-Datei in Xcode ein:

```csharp
@interface SplitViewController : NSSplitViewController {
    NSSplitViewItem *_LeftController;
    NSSplitViewItem *_RightController;
    NSSplitView *_SplitView;
}

@property (nonatomic, retain) IBOutlet NSSplitViewItem *LeftController;

@property (nonatomic, retain) IBOutlet NSSplitViewItem *RightController;

@property (nonatomic, retain) IBOutlet NSSplitView *SplitView;
```

Wie Sie sehen, werden Visual Studio für Mac Änderungen an der `.h` Datei abhört und diese Änderungen dann automatisch in der entsprechenden `.designer.cs` Datei synchronisieren, um Sie für Ihre Anwendung verfügbar zu machen. Sie können auch bemerken, dass `SplitViewController.designer.cs` eine partielle Klasse ist, sodass Visual Studio für Mac nicht ändern muss `SplitViewController.cs` was alle Änderungen überschreibt, die wir an der Klasse vorgenommen haben.

Normalerweise müssen Sie den `SplitViewController.designer.cs` nicht selbst öffnen, sondern nur zu Schulungszwecken.

> [!IMPORTANT]
> In den meisten Fällen sehen Visual Studio für Mac automatisch alle Änderungen, die in Xcode vorgenommen werden, und synchronisieren Sie mit Ihrem xamarin. Mac-Projekt. Sollte die Synchronisierung nicht automatisch durchgeführt werden – was sehr selten passiert –, wechseln Sie zurück zu Xcode und dann erneut zu Visual Studio für Mac. Dadurch wird normalerweise der Synchronisierungsprozess gestartet.

<a name="Working_with_Buttons" />

## <a name="working-with-buttons"></a>Arbeiten mit Schaltflächen

AppKit bietet verschiedene Arten von Schaltflächen, die in Ihrem Benutzeroberflächen Design verwendet werden können. Weitere Informationen finden Sie im Abschnitt " [Schalt](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsButtons.html#//apple_ref/doc/uid/20000957-CH48-SW1) Flächen" in den [Richtlinien für die Benutzeroberfläche von Apple OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/buttons01.png "An example of the different button types")](standard-controls-images/buttons01.png#lightbox)

Wenn eine Schaltfläche über ein **Outlet**verfügbar gemacht wurde, antwortet der folgende Code darauf, dass Sie gedrückt wird:

```csharp
ButtonOutlet.Activated += (sender, e) => {
        FeedbackLabel.StringValue = "Button Outlet Pressed";
};
```

Für Schaltflächen, die über **Aktionen**verfügbar gemacht wurden, wird automatisch eine `public partial` Methode mit dem Namen erstellt, den Sie in Xcode ausgewählt haben. Um auf die **Aktion**zu reagieren, vervollständigen Sie die partielle Methode in der Klasse, für die die **Aktion** definiert wurde. Beispiel:

```csharp
partial void ButtonAction (Foundation.NSObject sender) {
    // Do something in response to the Action
    FeedbackLabel.StringValue = "Button Action Pressed";
}
```

Für Schaltflächen mit einem Zustand (z. b. **ein-und** **ausschalten**) kann der Zustand überprüft oder mit der `State`-Eigenschaft für die `NSCellStateValue`-Enum festgelegt werden. Beispiel:

```csharp
DisclosureButton.Activated += (sender, e) => {
    LorumIpsum.Hidden = (DisclosureButton.State == NSCellStateValue.On);
};
```

Dabei kann `NSCellStateValue` lauten:

- Die Schaltfläche wird gedrückt, oder das Steuerelement ist ausgewählt (z. b. ein Kontrollkästchen Einchecken).
- **Off** : die Schaltfläche wird nicht per pushübertragung oder das Steuerelement nicht ausgewählt.
- **Gemischt** : eine Mischung aus **-und-aus-** **Zuständen.**

<a name="Mark-a-Button-as-Default-and-Set-Key-Equivalent" />

### <a name="mark-a-button-as-default-and-set-key-equivalent"></a>Markieren einer Schaltfläche als Standard und Festlegen des entsprechenden Schlüssels

Für alle Schaltflächen, die Sie einem Design der Benutzeroberfläche hinzugefügt haben, können Sie diese Schaltfläche als _Standard_ Schaltfläche markieren, die aktiviert wird, wenn der Benutzer die **Rückgabe-** /Eingabetaste auf der Tastatur drückt. Unter macOS erhält diese Schaltfläche standardmäßig eine blaue Hintergrundfarbe.

Um eine Schaltfläche als Standard festzulegen, wählen Sie Sie in der Interface Builder von Xcode aus. Wählen Sie als nächstes im **Attribut Inspektor**das **entsprechende Schlüssel** Feld aus, und drücken Sie die **Rückgabe/Eingabe** Taste:

[![](standard-controls-images/buttons03.png "Editing the Key Equivalent")](standard-controls-images/buttons03.png#lightbox)

Gleichermaßen können Sie jede beliebige Schlüssel Sequenz zuweisen, mit der die Schaltfläche mithilfe der Tastatur anstelle der Maus aktiviert werden kann. Beispielsweise durch Drücken der Befehls-C-Taste in der obigen Abbildung.

Wenn die app ausgeführt wird und das Fenster mit der Schaltfläche Key und fokussiert ist, wird die Aktion für die Schaltfläche aktiviert, wenn der Benutzer auf die Schaltfläche geklickt hat.

<a name="Working_with_Checkboxes_and_Radio_Buttons" />

## <a name="working-with-checkboxes-and-radio-buttons"></a>Arbeiten mit Kontrollkästchen und Options Feldern

AppKit bietet verschiedene Arten von Kontrollkästchen und Optionsfeld Gruppen, die in Ihrem Design der Benutzeroberfläche verwendet werden können. Weitere Informationen finden Sie im Abschnitt " [Schalt](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsButtons.html#//apple_ref/doc/uid/20000957-CH48-SW1) Flächen" in den [Richtlinien für die Benutzeroberfläche von Apple OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/buttons02.png "An example of the available checkbox types")](standard-controls-images/buttons02.png#lightbox)

Kontrollkästchen und Options Felder (die über **Outlets**verfügbar gemacht werden) haben einen Zustand (z. b. ein-und **ausschalten**). der Status kann mit der `State`-Eigenschaft **für** die `NSCellStateValue`-Enum überprüft oder festgelegt werden. Beispiel:

```csharp
AdjustTime.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Adjust Time: {0}",AdjustTime.State == NSCellStateValue.On);
};
```

Dabei kann `NSCellStateValue` lauten:

- Die Schaltfläche wird gedrückt, oder das Steuerelement ist ausgewählt (z. b. ein Kontrollkästchen Einchecken).
- **Off** : die Schaltfläche wird nicht per pushübertragung oder das Steuerelement nicht ausgewählt.
- **Gemischt** : eine Mischung aus **-und-aus-** **Zuständen.**

Zum Auswählen einer Schaltfläche in einer Optionsfeld Gruppe machen Sie das Optionsfeld für die Auswahl als **Outlet** verfügbar, und legen Sie dessen `State`-Eigenschaft fest. Beispiel:

```csharp
partial void SelectCar (Foundation.NSObject sender) {
    TransportationCar.State = NSCellStateValue.On;
    FeedbackLabel.StringValue = "Car Selected";
}
```

Um eine Sammlung von Options Feldern zu erstellen, die als Gruppe fungieren und automatisch den ausgewählten Zustand verarbeiten, erstellen Sie eine neue **Aktion** , und fügen Sie alle Schaltflächen in der Gruppe an den folgenden Punkt an:

![](standard-controls-images/buttons04.png "Creating a new Action")

Als Nächstes weisen Sie jedem Optionsfeld im **Attribut Inspektor**eine eindeutige `Tag` zu:

![](standard-controls-images/buttons05.png "Editing a radio button tag")

Speichern Sie die Änderungen, und kehren Sie zu Visual Studio für Mac zurück. Fügen Sie den Code hinzu, um die **Aktion** zu verarbeiten, an die alle Options Felder angefügt sind:

```csharp
partial void NumberChanged(Foundation.NSObject sender)
{
    var check = sender as NSButton;
    Console.WriteLine("Changed to {0}", check.Tag);
}
```

Sie können die `Tag`-Eigenschaft verwenden, um anzuzeigen, welches Optionsfeld ausgewählt wurde.

<a name="Working_with_Menu_Controls" />

## <a name="working-with-menu-controls"></a>Arbeiten mit Menü Steuerelementen

AppKit stellt verschiedene Typen von Menü Steuerelementen bereit, die in Ihrem Benutzeroberflächen Design verwendet werden können. Weitere Informationen finden Sie im Abschnitt " [Menü Steuerelemente](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlswithMenus.html#//apple_ref/doc/uid/20000957-CH100-SW1) " in den [Richtlinien für die Benutzeroberfläche von Apple OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/menu01.png "Example menu controls")](standard-controls-images/menu01.png#lightbox)

<a name="Providing-Menu-Control-Data" />

### <a name="providing-menu-control-data"></a>Bereitstellen von Menü Steuerungsdaten

Die Menü Steuerelemente, die für macOS verfügbar sind, können so festgelegt werden, dass die Dropdown Liste entweder aus einer internen Liste (die in Interface Builder vordefiniert oder über Code aufgefüllt werden kann) aufgefüllt wird, oder indem Sie eine eigene benutzerdefinierte, externe Datenquelle bereitstellt.

<a name="Working-with-Internal-Data" />

#### <a name="working-with-internal-data"></a>Arbeiten mit internen Daten

Zusätzlich zum Definieren von Elementen in Interface Builder stellen Menü Steuerelemente (z. b. `NSComboBox`) einen kompletten Satz von Methoden bereit, mit denen Sie die Elemente aus der internen Liste hinzufügen, bearbeiten oder löschen können, die Sie beibehalten:

- `Add`: Fügt am Ende der Liste ein neues Element hinzu.
- `GetItem`: gibt das Element am angegebenen Index zurück.
- `Insert`: Fügt ein neues Element an der angegebenen Position in die Liste ein.
- `IndexOf`: gibt den Index des angegebenen Elements zurück.
- `Remove` entfernt das angegebene Element aus der Liste.
- `RemoveAll`: entfernt alle Elemente aus der Liste.
- `RemoveAt`-entfernt das Element am angegebenen Index.
- `Count`: gibt die Anzahl der Elemente in der Liste zurück.

> [!IMPORTANT]
> Wenn Sie eine externe Datenquelle (`UsesDataSource = true`) verwenden, wird beim Aufrufen einer der oben genannten Methoden eine Ausnahme ausgelöst.

<a name="Working-with-an-External-Data-Source" />

#### <a name="working-with-an-external-data-source"></a>Arbeiten mit einer externen Datenquelle

Anstatt die integrierten internen Daten zum Bereitstellen der Zeilen für das Menü Steuerelement zu verwenden, können Sie optional eine externe Datenquelle verwenden und ihren eigenen Sicherungs Speicher für die Elemente (z. b. eine SQLite-Datenbank) bereitstellen.

Wenn Sie mit einer externen Datenquelle arbeiten möchten, erstellen Sie eine Instanz der Datenquelle des Menü Steuer Elements (z. b.`NSComboBoxDataSource`), und überschreiben Sie mehrere Methoden, um die erforderlichen Daten bereitzustellen:

- `ItemCount`: gibt die Anzahl der Elemente in der Liste zurück.
- `ObjectValueForItem`: gibt den Wert des Elements für einen angegebenen Index zurück.
- `IndexOfItem`: gibt den Index für den Wert des Elements "Wert" zurück.
- `CompletedString`: gibt den ersten übereinstimmenden Elementwert für den teilweise typisierten Elementwert zurück. Diese Methode wird nur aufgerufen, wenn AutoComplete aktiviert wurde (`Completes = true`).

Weitere Informationen finden Sie im Abschnitt [Datenbanken und ComboBoxes](~/mac/app-fundamentals/databases.md#Databases-and-ComboBoxes) des Dokuments [Arbeiten mit Datenbanken](~/mac/app-fundamentals/databases.md) .

<a name="Adjusting-the-Lists-Appearance" />

### <a name="adjusting-the-lists-appearance"></a>Anpassen des Erscheinungs Bilds der Liste

Die folgenden Methoden sind verfügbar, um die Darstellung des Menü Steuer Elements anzupassen:

- `HasVerticalScroller`: Wenn `true`, zeigt das Steuerelement eine vertikale Bild Lauf Leiste an. 
- `VisibleItems`: passen Sie die Anzahl der Elemente an, die beim Öffnen des Steuer Elements angezeigt werden. Der Standardwert ist 5 (5).
- `IntercellSpacing`: passen Sie den Leerraum um ein angegebenes Element an, indem Sie eine `NSSize` bereitstellen, in der der `Width` den linken und rechten Rand angibt, und der `Height` den Leerraum vor und nach einem Element angibt.
- `ItemHeight`: gibt die Höhe jedes Elements in der Liste an.

Für Dropdown Typen von `NSPopupButtons`gibt das erste Menü Element den Titel für das Steuerelement an. Beispiel: 

[![](standard-controls-images/menu02.png "An example menu control")](standard-controls-images/menu02.png#lightbox)

Um den Titel zu ändern, machen Sie dieses Element als **Outlet** verfügbar, und verwenden Sie Code wie den folgenden:

```csharp
DropDownSelected.Title = "Item 1";
```

<a name="Manipulating-the-Selected-Items" />

### <a name="manipulating-the-selected-items"></a>Bearbeiten der ausgewählten Elemente

Die folgenden Methoden und Eigenschaften ermöglichen es Ihnen, die ausgewählten Elemente in der Liste des Menü Steuer Elements zu bearbeiten:

- `SelectItem`: wählt das Element am angegebenen Index aus.
- Wählen Sie `Select` den angegebenen Elementwert aus.
- `DeselectItem` das Element am angegebenen Index deaktiviert.
- `SelectedIndex`: gibt den Index des derzeit ausgewählten Elements zurück.
- `SelectedValue`: gibt den Wert des aktuell ausgewählten Elements zurück.

Verwenden Sie die `ScrollItemAtIndexToTop`, um das Element am angegebenen Index oben in der Liste darzustellen, und die `ScrollItemAtIndexToVisible`, um einen Bildlauf zur Liste durchführen zu können, bis das Element am angegebenen Index sichtbar ist.

<a name="Responding to Events" />

### <a name="responding-to-events"></a>Reagieren auf Ereignisse

Menü Steuerelemente bieten die folgenden Ereignisse, um auf Benutzerinteraktionen zu reagieren:

- `SelectionChanged`-wird aufgerufen, wenn der Benutzer einen Wert aus der Liste ausgewählt hat.
- `SelectionIsChanging`-wird aufgerufen, bevor das neue Benutzer ausgewählte Element zur aktiven Auswahl wird.
- `WillPopup`-wird aufgerufen, bevor die Dropdown Liste mit Elementen angezeigt wird.
- `WillDismiss`-wird aufgerufen, bevor die Dropdown Liste mit den Elementen geschlossen wird.

Bei `NSComboBox`-Steuerelementen enthalten Sie alle Ereignisse wie die `NSTextField`, z. b. das `Changed` Ereignis, das aufgerufen wird, wenn der Benutzer den Wert des Texts im Kombinations Feld bearbeitet.

Optional können Sie auf die in Interface Builder ausgewählten internen Menü Elemente für Daten reagieren, indem Sie das Element an eine **Aktion** anfügen und Code wie den folgenden verwenden, um auf **Aktionen** zu reagieren, die vom Benutzer ausgelöst werden:

```csharp
partial void ItemOne (Foundation.NSObject sender) {
    DropDownSelected.Title = "Item 1";
    FeedbackLabel.StringValue = "Item One Selected";
}
```

Weitere Informationen zum Arbeiten mit Menüs und Menü Steuerelementen finden Sie in den [Menüs](~/mac/user-interface/menu.md) und der [Popup-Schaltfläche und in der Dropdown Liste](~/mac/user-interface/menu.md) mit der Dokumentation.

<a name="Working_with_Selection_Controls" />

## <a name="working-with-selection-controls"></a>Arbeiten mit Auswahl Steuerelementen

AppKit stellt verschiedene Typen von Auswahl Steuerelementen bereit, die in Ihrem Design der Benutzeroberfläche verwendet werden können. Weitere Informationen finden Sie im Abschnitt [Auswahl Steuerelemente](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsSelection.html#//apple_ref/doc/uid/20000957-CH49-SW1) in den Richtlinien für die Benutzeroberfläche von Apple [OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/select01.png "Example selection controls")](standard-controls-images/select01.png#lightbox)

Es gibt zwei Möglichkeiten, nachvollziehen zu können, wenn ein Auswahl Steuerelement eine Benutzerinteraktion hat, indem es als **Aktion**verfügbar gemacht wird. Beispiel:

```csharp
partial void SegmentButtonPressed (Foundation.NSObject sender) {
    FeedbackLabel.StringValue = string.Format("Button {0} Pressed",SegmentButtons.SelectedSegment);
}
```

Oder durch **Anhängen eines** Delegaten an das `Activated`-Ereignis. Beispiel:

```csharp
TickedSlider.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Stepper Value: {0:###}",TickedSlider.IntValue);
};
```

Um den Wert eines Auswahl Steuer Elements festzulegen oder zu lesen, verwenden Sie die `IntValue`-Eigenschaft. Beispiel:

```csharp
FeedbackLabel.StringValue = string.Format("Stepper Value: {0:###}",TickedSlider.IntValue);
```

Die spezialsteuerelemente (z. b. Color Well und Image Well) verfügen über bestimmte Eigenschaften für die Werttypen. Beispiel:

```csharp
ColorWell.Color = NSColor.Red;
ImageWell.Image = NSImage.ImageNamed ("tag.png");

```

Die `NSDatePicker` verfügt über die folgenden Eigenschaften für das direkte Arbeiten mit Datum und Uhrzeit:

- **DateValue** : der aktuelle Datums-und Uhrzeitwert als `NSDate`.
- **Lokal** : der Speicherort des Benutzers als `NSLocal`.
- **TimeInterval** : der Zeitwert als `Double`.
- **TimeZone** : die Zeitzone des Benutzers als `NSTimeZone`.

<a name="Working_with_Indicator_Controls" />

## <a name="working-with-indicator-controls"></a>Arbeiten mit Indikator Steuerelementen

AppKit stellt verschiedene Typen von Indikator Steuerelementen bereit, die in Ihrem Design der Benutzeroberfläche verwendet werden können. Weitere Informationen finden Sie im Abschnitt " [Indikator Steuerelemente](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsIndicators.html#//apple_ref/doc/uid/20000957-CH50-SW1) " in den [Richtlinien für die Benutzeroberfläche von Apple OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/level01.png "Example indicator controls")](standard-controls-images/level01.png#lightbox)

Es gibt zwei Möglichkeiten, nachvollziehen zu können, wenn ein Indikator Steuerelement eine Benutzerinteraktion hat, indem es entweder als **Aktion** oder als **Outlet** verfügbar gemacht wird und einen Delegaten **an das** `Activated` Ereignis anfügt. Beispiel:

```csharp
LevelIndicator.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Level: {0:###}",LevelIndicator.DoubleValue);
};
```

Verwenden Sie die `DoubleValue`-Eigenschaft, um den Wert des Indikator Steuer Elements zu lesen oder festzulegen. Beispiel:

```csharp
FeedbackLabel.StringValue = string.Format("Rating: {0:###}",Rating.DoubleValue);
```

Die unbestimmten und asynchronen Status Indikatoren sollten bei der Anzeige animiert werden. Verwenden Sie die `StartAnimation`-Methode, um die Animation zu starten, wenn Sie angezeigt werden. Beispiel:

```csharp
Indeterminate.StartAnimation (this);
AsyncProgress.StartAnimation (this);
```

Wenn Sie die `StopAnimation`-Methode aufrufen, wird die Animation angehalten.

<a name="Working_with_Text_Controls" />

## <a name="working-with-text-controls"></a>Arbeiten mit Text Steuerelementen

AppKit stellt verschiedene Typen von Text Steuerelementen bereit, die in Ihrem Benutzeroberflächen Design verwendet werden können. Weitere Informationen finden Sie im Abschnitt [Text Steuerelemente](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsText.html#//apple_ref/doc/uid/20000957-CH51-SW1) in den Richtlinien für die Benutzeroberfläche von Apple [OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/text01.png "Example text controls")](standard-controls-images/text01.png#lightbox)

Für Text Felder (`NSTextField`) können die folgenden Ereignisse verwendet werden, um die Benutzerinteraktion zu verfolgen:

- **Geändert** : wird jedes Mal ausgelöst, wenn der Wert des Felds vom Benutzer geändert wird. Beispielsweise bei jedem eingegebenen Zeichen.
- **Editingfing** : wird ausgelöst, wenn der Benutzer das Feld für die Bearbeitung auswählt.
- **Editingended** : Wenn der Benutzer die EINGABETASTE im Feld drückt oder das Feld verlässt.

Verwenden Sie die `StringValue`-Eigenschaft, um den Wert des Felds zu lesen oder festzulegen. Beispiel:

```csharp
FeedbackLabel.StringValue = string.Format("User ID: {0}",UserField.StringValue);
```

Für Felder, die numerische Werte anzeigen oder bearbeiten, können Sie die `IntValue`-Eigenschaft verwenden. Beispiel:

```csharp
FeedbackLabel.StringValue = string.Format("Number: {0}",NumberField.IntValue);
```

Eine `NSTextView` bietet einen voll funktionsfähigen Text Bearbeitungs-und Anzeigebereich mit integrierter Formatierung. Verwenden Sie wie eine `NSTextField`die `StringValue`-Eigenschaft, um den Wert des Bereichs zu lesen oder festzulegen.

Ein Beispiel für ein komplexes Beispiel für das Arbeiten mit Text Ansichten in einer xamarin. Mac-app finden Sie in der [sourcewriter](https://docs.microsoft.com/samples/xamarin/mac-samples/sourcewriter)-Beispiel-app. SourceWriter ist ein einfacher Quellcode-Editor, der Unterstützung für die Codevervollständigung und die einfache Syntaxhervorhebung bietet.

Der SourceWriter-Code wurde vollständig kommentiert und es wurden, wenn möglich, Links der Schlüsseltechnologien und -methoden zu wichtigen Informationen in den Leitfäden der Xamarin.Mac-Dokumentation bereitgestellt.

<a name="Working_with_Content_Views" />

## <a name="working-with-content-views"></a>Arbeiten mit Inhalts Ansichten

AppKit bietet verschiedene Typen von Inhalts Ansichten, die im Design der Benutzeroberfläche verwendet werden können. Weitere Informationen finden Sie im Abschnitt " [Inhalts Ansichten](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1) " in den [Richtlinien für die Benutzeroberfläche von Apple OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

[![](standard-controls-images/content01.png "An example content view")](standard-controls-images/content01.png#lightbox)

<a name="Popovers" />

### <a name="popovers"></a>Popovers

Ein popover ist ein vorübergehendes Benutzeroberflächen Element, das Funktionen bereitstellt, die sich direkt auf ein bestimmtes Steuerelement oder einen Bildschirm-Bereich beziehen. Ein popover schwebt oberhalb des Fensters, das das Steuerelement oder den Bereich enthält, mit dem es verknüpft ist, und der Rahmen enthält einen Pfeil, der den Punkt angibt, von dem er hervorgegangen ist.

Gehen Sie folgendermaßen vor, um ein popover zu erstellen:

1. Öffnen Sie die `.storyboard` Datei des Fensters, dem Sie ein popover hinzufügen möchten, indem Sie in der **Projektmappen-Explorer**
2. Ziehen Sie einen **Ansichts Controller** aus der **Bibliothek Inspector** auf den **Schnittstellen-Editor**: 

    [![](standard-controls-images/content02.png "Selecting a View Controller from the Library")](standard-controls-images/content02.png#lightbox)
3. Definieren Sie die Größe und das Layout der **benutzerdefinierten Ansicht**: 

    [![](standard-controls-images/content04.png "Editing the layout")](standard-controls-images/content04.png#lightbox)
4. Klicken Sie mit der Maustaste auf den **Ansichts Controller**, und ziehen Sie ihn von der Quelle des Popup Fensters: 

    [![](standard-controls-images/content05.png "Dragging to create a segue")](standard-controls-images/content05.png#lightbox)
5. Wählen Sie im Popup Menü **popover** aus: 

    [![](standard-controls-images/content06.png "Setting the segue type")](standard-controls-images/content06.png#lightbox)
6. Speichern Sie die Änderungen, und kehren Sie zu Visual Studio für Mac zurück, um mit Xcode zu synchronisieren.

<a name="Tab_Views" />

### <a name="tab-views"></a>Registerkarten Ansichten

Registerkarten Sichten bestehen aus einer Registerkarten Liste (die einem segmentierten Steuerelement ähnelt), kombiniert mit einem Satz von Sichten, die als _Bereiche bezeichnet werden_. Wenn der Benutzer eine neue Registerkarte auswählt, wird der angefügte Bereich angezeigt. Jeder Bereich enthält einen eigenen Satz von Steuerelementen.

Wenn Sie mit einer Registerkarten Ansicht in der Interface Builder von Xcode arbeiten, verwenden Sie den **Attribut Inspektor** , um die Anzahl der Registerkarten festzulegen:

[![](standard-controls-images/content08.png "Editing the number of tabs")](standard-controls-images/content08.png#lightbox)

Wählen Sie in der **Schnittstellen Hierarchie** jede Registerkarte aus, **um deren** **Titel** festzulegen, und fügen Sie Benutzeroberflächen Elemente zum Bereich hinzu

[![](standard-controls-images/content09.png "Editing the tabs in Xcode")](standard-controls-images/content09.png#lightbox)

<a name="Data_Binding_AppKit_Controls" />

## <a name="data-binding-appkit-controls"></a>AppKit-Steuerelemente für Datenbindung

Durch die Verwendung von Schlüssel-Wert-Codierungs-und Daten Bindungs Techniken in ihrer xamarin. Mac-Anwendung können Sie die Menge des Codes, den Sie schreiben und verwalten müssen, erheblich verringern, um Benutzeroberflächen Elemente aufzufüllen und mit Ihnen zu arbeiten. Außerdem profitieren Sie von der weiteren Entkopplung ihrer Sicherungsdaten (_Datenmodell_) von der Front-End-Benutzeroberfläche (_Model-View-Controller_). Dies führt zu einer einfacheren Wartung und einem flexibleren Anwendungs Entwurf.

Key-Value Coding (KVC) ist ein Mechanismus für den indirekten Zugriff auf die Eigenschaften eines Objekts, indem Schlüssel (speziell formatierte Zeichen folgen) verwendet werden, um Eigenschaften zu identifizieren, anstatt über Instanzvariablen oder Accessormethoden (`get/set`) auf Sie zuzugreifen. Durch die Implementierung von Schlüssel-Wert-Codierungs kompatiblen Accessoren in ihrer xamarin. Mac-Anwendung erhalten Sie Zugriff auf andere macOS-Features, wie z. b. Key-Value-Beobachtungen (KVO), Datenbindung, Kerndaten, Cocoa-Bindungen und scriptbarkeit.

Weitere Informationen finden Sie im Abschnitt [einfache Datenbindung](~/mac/app-fundamentals/databinding.md#Simple_Data_Binding) in unserer Datenbindung und in der Dokumentation zum [Schlüssel-Wert-codieren](~/mac/app-fundamentals/databinding.md) .

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie Sie mit den standardmäßigen AppKit-Steuerelementen wie Schaltflächen, Bezeichnungen, Text Feldern, Kontrollkästchen und segmentierten Steuerelementen in einer xamarin. Mac-Anwendung arbeiten. Es wurde beschrieben, wie Sie Sie zu einem Design der Benutzeroberfläche in Xcode-Interface Builder hinzufügen, das Sie für den Code über Outlets und Aktionen C# verfügbar machen und mit AppKit-Steuerelementen im Code arbeiten.

## <a name="related-links"></a>Verwandte Links

- [Maccontrols (Beispiel)](https://docs.microsoft.com/samples/xamarin/mac-samples/maccontrols)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Windows](~/mac/user-interface/window.md)
- [Datenbindung und Schlüssel/Wert-Codierung](~/mac/app-fundamentals/databinding.md)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Informationen zu Steuerelementen und Ansichten](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1)
