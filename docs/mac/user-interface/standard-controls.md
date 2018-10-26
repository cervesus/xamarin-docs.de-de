---
title: Standardsteuerelemente in Xamarin.Mac
description: In diesem Artikel wird beschrieben, wie mit den standardmäßigen AppKit-Steuerelementen wie Schaltflächen, Bezeichnungen, Textfelder, Kontrollkästchen und segmentierte Steuerelemente in einer Xamarin.Mac-Anwendung. Hinzufügen zu einer Benutzeroberfläche mit Interface Builder und mit ihnen interagieren, im Code beschrieben.
ms.prod: xamarin
ms.assetid: d2593883-d255-431f-9781-75f04d8cecea
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 9f5bdc9a79c514f0310d29b3d054fb7e9659d669
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50123790"
---
# <a name="standard-controls-in-xamarinmac"></a>Standardsteuerelemente in Xamarin.Mac

_In diesem Artikel wird beschrieben, wie mit den standardmäßigen AppKit-Steuerelementen wie Schaltflächen, Bezeichnungen, Textfelder, Kontrollkästchen und segmentierte Steuerelemente in einer Xamarin.Mac-Anwendung. Hinzufügen zu einer Benutzeroberfläche mit Interface Builder und mit ihnen interagieren, im Code beschrieben._

Bei der Arbeit mit c# und .NET in einer Xamarin.Mac-Anwendung haben Sie Zugriff auf die gleichen AppKit steuert, die ein Entwickler *Objective-C-* und *Xcode* ist. Da Xamarin.Mac direkt in Xcode integriert ist, können Sie von Xcode _Interface Builder_ erstellen und verwalten Ihre Appkit-Steuerelementen (oder erstellen sie optional direkt in c#-Code).

AppKit-Steuerelementen, sind die Elemente der Benutzeroberfläche, die verwendet werden, um die Benutzeroberfläche der Xamarin.Mac-Anwendung zu erstellen. Sie bestehen aus Elementen, z. B. Schaltflächen, Bezeichnungen, Textfelder, Kontrollkästchen und segmentierte Steuerelemente und sofortige Aktionen oder direkten Ergebnisse verursachen, wenn ein Benutzer, die sie bearbeitet haben.

[![](standard-controls-images/intro01.png "Die Beispiel-app-Hauptbildschirm")](standard-controls-images/intro01.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit AppKit-Steuerelementen in einer Xamarin.Mac-Anwendung beschrieben. Es wird dringend empfohlen, dass Sie über arbeiten die [Hallo, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst, insbesondere die [Einführung in Xcode und Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und [Outlets und Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) Abschnitte, wie sie behandelt wichtige Konzepte und Techniken, die wir in diesem Artikel verwenden.

Empfiehlt es sich um einen Blick auf die [Verfügbarmachen von c#-Klassen / Methoden mit Objective-C](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac-Interna](~/mac/internals/how-it-works.md) dokumentieren, es wird erläutert, die `Register` und `Export` Befehle verwendet, um Ihre Klassen in c# für Objective-C-Objekte und Elemente der Benutzeroberfläche anschließen.

<a name="Introduction_to_Controls_and_Views" />

## <a name="introduction-to-controls-and-views"></a>Einführung in die Steuerelemente und Ansichten

MacOS (ehemals Mac OS X) stellt eine Gruppe von Steuerelementen der Benutzeroberfläche über das AppKit-Framework. Sie bestehen aus Elementen, z. B. Schaltflächen, Bezeichnungen, Textfelder, Kontrollkästchen und segmentierte Steuerelemente und sofortige Aktionen oder direkten Ergebnisse verursachen, wenn ein Benutzer, die sie bearbeitet haben.

Alle von AppKit-Steuerelementen haben eine integrierten Darstellung, die für die meisten verwendet werden, einige Geben Sie einer Alternative Darstellung für die Verwendung in einem Fenster-Frame-Bereich oder in einer _Dynamik Auswirkungen_ z. B. wie eine Randleistenbereich oder im Kontext einer Mitteilungszentrale-Widget.

Apple wird bei der Verwendung von AppKit-Steuerelementen, die folgenden Richtlinien empfohlen:

- Mischen Sie Steuerelementgrößen in derselben Ansicht.
- Vermeiden Sie generell die vertikale Größenänderung von Steuerelementen.
- Verwenden Sie die Systemschriftart und die richtigen Textgröße in einem Steuerelement.
- Verwenden Sie den richtigen Abstand zwischen Steuerelementen.

Pleas finden Sie weitere Informationen die [zu Steuerelementen und Ansichten](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1) Abschnitt des Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

<a name="Using_Controls_in_a_Window_Frame" />

### <a name="using-controls-in-a-window-frame"></a>Verwenden von Steuerelementen in einem Fensterrahmen

Es gibt eine Teilmenge von AppKit-Steuerelementen, die eine Anzeigestil enthalten, die sie im Bereich eines Fensters-Frame eingeschlossen werden können. Ein Beispiel finden Sie in der Mail-app-Symbolleiste:

[![](standard-controls-images/mailapp.png "Ein Fensterrahmen für Mac")](standard-controls-images/mailapp.png#lightbox)

- **Runden Sie die Schaltfläche mit Textur versehen habe** – eine `NSButton` mit einem der `NSTexturedRoundedBezelStyle`.
- **Gerundet segmentiertes Steuerelement mit Textur versehen habe** – eine `NSSegmentedControl` mit einem der `NSSegmentStyleTexturedRounded`.
- **Gerundet segmentiertes Steuerelement mit Textur versehen habe** – eine `NSSegmentedControl` mit einem der `NSSegmentStyleSeparated`.
- **Round-Popupmenü Texturen versehenen** – eine `NSPopUpButton` mit einem der `NSTexturedRoundedBezelStyle`.
- **Round-Dropdownmenü mit Textur versehen habe** – eine `NSPopUpButton` mit einem der `NSTexturedRoundedBezelStyle`.
- **Suchleiste** – eine `NSSearchField`.

Apple empfehlen die folgenden Richtlinien, bei der Verwendung von AppKit-Steuerelementen in einem Fensterrahmen:

- Verwenden Sie keine bestimmte Steuerelementstile Window Frame in den Text im Fenster.
- Verwenden Sie keine Fenster-Text-Steuerelemente oder Stile im Fensterrahmen an.

Pleas finden Sie weitere Informationen die [zu Steuerelementen und Ansichten](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1) Abschnitt des Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

<a name="Creating_a_User_Interface_in_Interface_Builder" />

## <a name="creating-a-user-interface-in-interface-builder"></a>Erstellen eine Benutzeroberfläche in Interface Builder

Wenn Sie eine neue Xamarin.Mac-Cocoa-Anwendung erstellen, erhalten Sie ein Standardfenster leer ist, wird standardmäßig an. Dieses Windows wird definiert, einem `.storyboard` Datei automatisch im Projekt enthalten ist. So bearbeiten Sie Ihre Windows-Design, in der **Projektmappen-Explorer**, Doppelklick der `Main.storyboard` Datei:

[![](standard-controls-images/edit01.png "Wählen im Projektmappen-Explorer die Haupt-Storyboard")](standard-controls-images/edit01.png#lightbox)

Dies öffnet das Fenster Design in Interface Builder von Xcode:

[![](standard-controls-images/edit02.png "Bearbeiten das Storyboard in Xcode")](standard-controls-images/edit02.png#lightbox)

Um die Benutzeroberfläche zu erstellen, ziehen Sie Elemente der Benutzeroberfläche (AppKit-Steuerelementen), aus der **Bibliotheksinspektor** auf die **Schnittstellen-Editor** in Interface Builder. Im folgenden Beispiel wird eine **vertikale geteilte Ansicht** Steuerelement wurde von Medikamenten aus der **Bibliotheksinspektor** und platziert Sie im Fenster in der **Schnittstellen-Editor**:

[![](standard-controls-images/edit03.png "Wählen aus der Bibliothek eine geteilte Ansicht")](standard-controls-images/edit03.png#lightbox)

Weitere Informationen zum Erstellen einer Benutzeroberfläche in Interface Builder finden Sie unserem [Einführung in Xcode und Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) Dokumentation.

<a name="Sizing_and_Positioning" />

### <a name="sizing-and-positioning"></a>Größenanpassung und Positionierung

Nachdem ein Steuerelement in der Benutzeroberfläche enthalten ist, verwenden Sie die **-Editors** seine Position und Größe festlegen, indem Sie Werte manuell eingeben, und steuern, wie das Steuerelement wird automatisch so angeordnet, und wenn die Größe der übergeordneten Fenster oder eine Sicht die Größe wird geändert:

[![](standard-controls-images/edit04.png "Festlegen von Einschränkungen")](standard-controls-images/edit04.png#lightbox)

Verwenden der **roten Balken** außerhalb eines der **Autoresizing** Feld _Stick_ eines Steuerelements an einen Speicherort (x, y). Zum Beispiel: 

[![](standard-controls-images/edit05.png "Eine Einschränkung bearbeiten")](standard-controls-images/edit05.png#lightbox)

Gibt an, dass das ausgewählte Steuerelement (in der **Hierarchieansicht** & **Schnittstellen-Editor**) wird auf den oberen und rechten-Speicherort, der das Fenster oder einer Ansicht unterbrochen, wie sie die Größe geändert oder verschoben wird. 

Andere Elemente des Editors steuern Eigenschaften wie z. B. Höhe und Breite:

[![](standard-controls-images/edit06.png "Festlegen der Höhe")](standard-controls-images/edit06.png#lightbox)

Sie können auch die Ausrichtung der Elemente steuern, mit Einschränkungen, die mit der **Ausrichtung Editor**:

[![](standard-controls-images/edit07.png "Die Ausrichtung-Editor")](standard-controls-images/edit07.png#lightbox)

> [!IMPORTANT]
> Im Gegensatz zu iOS, in denen (0,0) wird von die oberen linken Ecke des Bildschirms in MacOS (0,0) ist der unteren linken Ecke. Dies ist, da MacOS eine mathematische Koordinatensystem mit der numerischen Werte, dessen Wert sich vergrößert nach oben und nach rechts verwendet. Sie müssen dieses beim Platzieren von AppKit-Steuerelementen auf einer Benutzeroberfläche berücksichtigt werden.

<a name="Setting_a_Custom_Class" />

### <a name="setting-a-custom-class"></a>Festlegen einer benutzerdefinierten Klasse

Es gibt Situationen, die beim Verwenden von AppKit-Steuerelementen, die Sie benötigen Unterklasse und vorhandenen steuern und erstellen Sie eigene angepasste Version dieser Klasse. Definieren z. B. eine benutzerdefinierte Version des der Quellliste aus:

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

In denen die `[Register("SourceListView")]` Anweisung macht die `SourceListView` Klasse mit Objective-C ist in Interface Builder verwendet werden kann. Weitere Informationen finden Sie unter den [Verfügbarmachen von c#-Klassen / Methoden mit Objective-C](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac-Interna](~/mac/internals/how-it-works.md) zu dokumentieren, es wird erläutert, die `Register` und `Export` verwendeten Befehle um Ihre Klassen in c# für Objective-C-Objekte und Elemente der Benutzeroberfläche anschließen.

Sie können ein Steuerelement AppKit des Basistyps, die Sie erweitern, auf die Entwurfsoberfläche ziehen, mit dem obigen Code vorhanden, (im folgenden Beispiel wird eine **Quellliste**), wechseln Sie zu der **Identitätsinspektor** und legen Sie die **benutzerdefinierte Klasse** auf den Namen, die Sie mit Objective-C verfügbar gemacht (Beispiel `SourceListView`):

[![](standard-controls-images/edit10.png "Festlegen einer benutzerdefinierten Klasse in Xcode")](standard-controls-images/edit10.png#lightbox)

<a name="Exposing_Outlets_and_Actions" />

### <a name="exposing-outlets-and-actions"></a>Verfügbarmachen von Outlets und Aktionen

Bevor ein AppKit-Steuerelement in C#-Code zugegriffen werden kann, muss es entweder als verfügbar gemacht werden ein **Outlet** oder und **Aktion**. Wählen Sie hierzu das angegebene Steuerelement entweder in der **Schnittstellenhierarchie** oder **Schnittstellen-Editor** und wechseln Sie zu der **Assistant Ansicht** (Stellen Sie sicher, dass Sie die verfügen`.h`Rand des Fensters, das zur Bearbeitung ausgewählte):

[![](standard-controls-images/edit11.png "So bearbeiten Sie die richtige Datei auswählen")](standard-controls-images/edit11.png#lightbox)

Steuerelement ziehen aus dem AppKit-Steuerelement auf der angegebenen `.h` Datei zum Erstellen einer **Outlet** oder **Aktion**:

[![](standard-controls-images/edit12.png "Ziehen zum Erstellen einer Outlet oder einer Aktion")](standard-controls-images/edit12.png#lightbox)

Wählen Sie den Typ der Gefährdung für das Erstellen, und geben die **Outlet** oder **Aktion** eine **Namen**: 

[![](standard-controls-images/edit13.png "Konfigurieren des Outlet oder Aktion")](standard-controls-images/edit13.png#lightbox)


Weitere Informationen zum Arbeiten mit **Outlets** und **Aktionen**, finden Sie unter den [Outlets und Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) Teil unserer [Einführung in Xcode und Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) Dokumentation.

<a name="Synchronizing_Changes_with_Xcode" />

### <a name="synchronizing-changes-with-xcode"></a>Synchronisieren von Änderungen mit Xcode

Wenn Sie zurück zu Visual Studio für Mac in Xcode wechseln, werden alle Änderungen, die Sie in Xcode vorgenommenen automatisch mit dem Xamarin.Mac-Projekt synchronisiert werden.

Bei Auswahl der `SplitViewController.designer.cs` in die **Projektmappen-Explorer** können, finden Sie unter wie Ihre **Outlet** und **Aktion** verbunden wurden im C#-Code:

[![](standard-controls-images/sync01.png "Synchronisieren von Änderungen mit Xcode")](standard-controls-images/sync01.png#lightbox)

Beachten Sie, dass wie die Definition in der `SplitViewController.designer.cs` Datei:

```csharp
[Outlet]
AppKit.NSSplitViewItem LeftController { get; set; }

[Outlet]
AppKit.NSSplitViewItem RightController { get; set; }

[Outlet]
AppKit.NSSplitView SplitView { get; set; }
```

Zeile nach oben mit der Definition in der `MainWindow.h` -Datei in Xcode:

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

Wie Sie sehen können, Visual Studio für Mac Lauscht auf Änderungen an der `.h` Datei, und klicken Sie dann synchronisiert diese automatisch der entsprechenden `.designer.cs` Datei, die sie für Ihre Anwendung verfügbar zu machen. Sie können auch feststellen, dass `SplitViewController.designer.cs` wird von eine partielle Klasse sind, damit Visual Studio für Mac nicht so ändern Sie `SplitViewController.cs ` überschreibt die Änderungen, die wir auf die Klasse vorgenommen haben.

Sie normalerweise müssen nie zum Öffnen der `SplitViewController.designer.cs` selbst, es wurde hier vorgestellten zum besseren.

> [!IMPORTANT]
> In den meisten Fällen Visual Studio für Mac automatisch finden Sie in Xcode vorgenommenen Änderungen und synchronisiert diese mit dem Xamarin.Mac-Projekt. Sollte die Synchronisierung nicht automatisch durchgeführt werden – was sehr selten passiert –, wechseln Sie zurück zu Xcode und dann erneut zu Visual Studio für Mac. Dadurch wird normalerweise der Synchronisierungsprozess gestartet.

<a name="Working_with_Buttons" />

## <a name="working-with-buttons"></a>Arbeiten mit Schaltflächen

AppKit enthält mehrere Typen von Schaltfläche, die in Ihrem Entwurf der Benutzeroberfläche verwendet werden kann. Weitere Informationen finden Sie unter den [Schaltflächen](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsButtons.html#//apple_ref/doc/uid/20000957-CH48-SW1) Abschnitt des Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/buttons01.png "Ein Beispiel für die verschiedenen Typen")](standard-controls-images/buttons01.png#lightbox)

Wenn Sie über eine Schaltfläche verfügbar gemacht wurde ein **Outlet**, der folgende Code reagiert auf sie geklickt wird:

```csharp
ButtonOutlet.Activated += (sender, e) => {
        FeedbackLabel.StringValue = "Button Outlet Pressed";
};
```

Für Schaltflächen, die über verfügbar gemacht wurden **Aktionen**, `public partial` Methode wird automatisch für Sie erstellt werden, mit dem Namen, die Sie in Xcode ausgewählt haben. Reagieren auf die **Aktion**, schließen Sie die partielle Methode in der Klasse, die die **Aktion** definiert wurde. Zum Beispiel:

```csharp
partial void ButtonAction (Foundation.NSObject sender) {
    // Do something in response to the Action
    FeedbackLabel.StringValue = "Button Action Pressed";
}
```

Für Schaltflächen, die einen Status haben (wie **auf** und **aus**), der Status überprüft oder festgelegt werden kann die `State` -Eigenschaft anhand der `NSCellStateValue` Enum. Zum Beispiel:

```csharp
DisclosureButton.Activated += (sender, e) => {
    LorumIpsum.Hidden = (DisclosureButton.State == NSCellStateValue.On);
};
```

Wo `NSCellStateValue` sind möglich:

- **Auf** : die Schaltfläche wird mithilfe von Push übertragen oder das Steuerelement (z. B. eine Überprüfung in einem Kontrollkästchen) ausgewählt ist.
- **Off** : die Schaltfläche wird nicht abgelegt oder das Steuerelement nicht ausgewählt ist.
- **Gemischte** -eine Kombination von **auf** und **aus** Zustände.

<a name="Mark-a-Button-as-Default-and-Set-Key-Equivalent" />

### <a name="mark-a-button-as-default-and-set-key-equivalent"></a>Markieren Sie eine Schaltfläche als Standard, und legen Sie die Schlüssel entspricht.

Für keine der Schaltflächen, die Sie einen Entwurf der Benutzeroberfläche hinzugefügt haben, können Sie diese Schaltfläche als kennzeichnen die _Standard_ Schaltfläche, die aktiviert wird, wenn der Benutzer drückt die **Rückgabe/EINGABETASTE** auf der Tastatur die Taste. Unter MacOS erhält diese Schaltfläche wird standardmäßig eine blaue Hintergrundfarbe.

Um eine Schaltfläche als Standard festzulegen, wählen sie in Interface Builder von Xcode. Dann in der **Attributinspektor**, wählen die **Schlüssel entspricht** Feld, und drücken Sie die **Rückgabe/EINGABETASTE** Schlüssel:

[![](standard-controls-images/buttons03.png "Bearbeiten die wichtigsten Entsprechung")](standard-controls-images/buttons03.png#lightbox)

Ebenso können Sie jede Tastenkombination zuweisen, die verwendet werden können, um die Schaltfläche mit der Tastatur anstelle der Maus zu aktivieren. Z. B. indem Sie bei der Tastenkombination Befehl + C in der Abbildung oben.

Wenn die app ausgeführt wird und das Fenster mit der Schaltfläche ist der Schlüssel und mit Fokus, wenn der Benutzer den Befehl + C drückt, wird die Aktion für die Schaltfläche aktiviert werden (wie – wenn der Benutzer auf die Schaltfläche geklickt hätte).

<a name="Working_with_Checkboxes_and_Radio_Buttons" />

## <a name="working-with-checkboxes-and-radio-buttons"></a>Arbeiten mit Kontrollkästchen und Optionsfelder

AppKit enthält verschiedene Typen von Kontrollkästchen und Optionsfeldgruppen, die in Ihrem Entwurf der Benutzeroberfläche verwendet werden kann. Weitere Informationen finden Sie unter den [Schaltflächen](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsButtons.html#//apple_ref/doc/uid/20000957-CH48-SW1) Abschnitt des Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/buttons02.png "Ein Beispiel für die Typen der verfügbaren Kontrollkästchen.")](standard-controls-images/buttons02.png#lightbox)


Kontrollkästchen und Optionsfelder (über verfügbar gemachte **Outlets**) verfügen über einen Zustand (wie **auf** und **aus**), der Zustand kann überprüft oder festgelegt werden, mit der `State` -Eigenschaft anhand der `NSCellStateValue` Enum. Zum Beispiel:

```csharp
AdjustTime.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Adjust Time: {0}",AdjustTime.State == NSCellStateValue.On);
};
```

Wo `NSCellStateValue` sind möglich:

- **Auf** : die Schaltfläche wird mithilfe von Push übertragen oder das Steuerelement (z. B. eine Überprüfung in einem Kontrollkästchen) ausgewählt ist.
- **Off** : die Schaltfläche wird nicht abgelegt oder das Steuerelement nicht ausgewählt ist.
- **Gemischte** -eine Kombination von **auf** und **aus** Zustände.

Zum Auswählen einer Schaltfläche in eine Gruppe von Optionsfeldern verfügbar zu machen als wählen Sie das Optionsfeld ein **Outlet** und legen Sie seine `State` Eigenschaft. Beispiel:

```csharp
partial void SelectCar (Foundation.NSObject sender) {
    TransportationCar.State = NSCellStateValue.On;
    FeedbackLabel.StringValue = "Car Selected";
}
```

Um eine Sammlung von Optionsfeldern an, wie eine Gruppe aus, und behandelt automatisch den Auswahlzustand zu erhalten, erstellen Sie ein neues **Aktion** , und fügen Sie jeder Schaltfläche in der Gruppe zuzuweisen:

![](standard-controls-images/buttons04.png "Erstellen einer neuen Aktion")

Als Nächstes weisen Sie einen eindeutigen `Tag` auf jedes Optionsfeld in der **Attributinspektor**:

![](standard-controls-images/buttons05.png "Bearbeiten einer Radio Button-tag")

Die Änderungen zu speichern und zurück zu Visual Studio für Mac, fügen Sie den Code zum Behandeln der **Aktion** , dass alle Optionsfelder angefügt werden:

```csharp
partial void NumberChanged(Foundation.NSObject sender)
{
    var check = sender as NSButton;
    Console.WriteLine("Changed to {0}", check.Tag);
}
```

Sie können die `Tag` Eigenschaft, um anzuzeigen, welches Optionsfeld ausgewählt wurde.

<a name="Working_with_Menu_Controls" />

## <a name="working-with-menu-controls"></a>Arbeiten mit Menu-Steuerelemente

AppKit enthält mehrere Typen von Menü-Steuerelemente, die in Ihrem Entwurf der Benutzeroberfläche verwendet werden kann. Weitere Informationen finden Sie unter den [Menüsteuerelemente](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlswithMenus.html#//apple_ref/doc/uid/20000957-CH100-SW1) Abschnitt des Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/menu01.png "Beispiel-Menu-Steuerelemente")](standard-controls-images/menu01.png#lightbox)

<a name="Providing-Menu-Control-Data" />

### <a name="providing-menu-control-data"></a>Bereitstellen von Menü-Steuerelementdaten

Die Menü-Steuerelemente für MacOS verfügbar, kann der Dropdown-Liste entweder aus einer internen Liste aufgefüllt (die können vordefinierte in Interface Builder oder über Code aufgefüllt) festgelegt werden oder durch Ihre eigenen benutzerdefinierten, externe Datenquelle bereitstellen.

<a name="Working-with-Internal-Data" />

#### <a name="working-with-internal-data"></a>Arbeiten mit internen Daten

Neben der Definition der Elemente in Interface Builder, Menu-Steuerelemente (z. B. `NSComboBox`), geben Sie einen vollständigen Satz von Methoden, mit denen Sie hinzufügen, bearbeiten oder löschen Sie die Elemente aus der internen Liste, die sie verwalten:

- `Add` -Fügt ein neues Element am Ende der Liste.
- `GetItem` -Gibt das Element am angegebenen Index.
- `Insert` -Fügt ein neues Element in der Liste am angegebenen Speicherort.
- `IndexOf` -Gibt den Index des angegebenen Elements zurück.
- `Remove` : Entfernt das angegebene Element aus der Liste.
- `RemoveAll` : Entfernt alle Elemente aus der Liste.
- `RemoveAt` : Entfernt das Element am angegebenen Index.
- `Count` -Gibt die Anzahl der Elemente in der Liste.

> [!IMPORTANT]
> Wenn Sie eine externe Datenquelle verwenden (`UsesDataSource = true`), bei dem oben genannten Methoden aufrufen, wird eine Ausnahme ausgelöst.

<a name="Working-with-an-External-Data-Source" />

#### <a name="working-with-an-external-data-source"></a>Arbeiten mit einer externen Datenquelle

Anstelle von verwenden die integrierte interne Daten, um die Zeilen für das Menü-Steuerelement zu ermöglichen, können Sie optional verwenden einer externen Datenquelle und bieten Ihren eigenen Sicherungsspeicher für die Elemente (z. B. eine SQLite-Datenbank).

Um mit einer externen Datenquelle arbeiten, erstellen Sie eine Instanz der Datenquelle für das Menüsteuerelement (`NSComboBoxDataSource` z. B.) und mehrere Methoden zum Bereitstellen der erforderlichen Daten zu überschreiben:

- `ItemCount` -Gibt die Anzahl der Elemente in der Liste.
- `ObjectValueForItem` -Gibt den Wert des Elements, für einen bestimmten Index.
- `IndexOfItem` -Gibt den Index für den Elementwert gewähren.
- `CompletedString` -Gibt den ersten übereinstimmenden Elementwert für den Wert für die teilweise typisiertes Element. Diese Methode wird nur aufgerufen, wenn die automatische Vervollständigung aktiviert wurde (`Completes = true`).

Informieren Sie sich die [Datenbanken und ComboBoxes](~/mac/app-fundamentals/databases.md#Databases-and-ComboBoxes) im Abschnitt der [arbeiten mit Datenbanken](~/mac/app-fundamentals/databases.md) Dokument Weitere Informationen.

<a name="Adjusting-the-Lists-Appearance" />

### <a name="adjusting-the-lists-appearance"></a>Die Darstellung der Liste anpassen

Die folgenden Methoden sind für das Menüsteuerelement Darstellung angepasst werden:

- `HasVerticalScroller` -If `true`, das Steuerelement wird eine vertikale Bildlaufleiste angezeigt. 
- `VisibleItems` – Passen Sie die Anzahl der Elemente, die angezeigt wird, wenn das Steuerelement geöffnet wird. Der Standardwert beträgt fünf (5).
- `IntercellSpacing` -Passen Sie die Menge des Speicherplatzes für ein bestimmtes Element, durch die Bereitstellung einer `NSSize` , in denen die `Width` gibt den linken und rechten Rand und die `Height` gibt den Speicherplatz an, vor und nach einem Element.
- `ItemHeight` -Gibt die Höhe jedes Elements in der Liste.

Für Drop-Down-Typen von `NSPopupButtons`, das erste Menüelement stellt den Titel für das Steuerelement bereit. Beispiel: 

[![](standard-controls-images/menu02.png "Eine Beispiel-Menu-Steuerelement")](standard-controls-images/menu02.png#lightbox)

Um den Titel ändern möchten, machen Sie dieses Element als ein **Outlet** und verwenden Sie Code wie folgt:

```csharp
DropDownSelected.Title = "Item 1";
```

<a name="Manipulating-the-Selected-Items" />

### <a name="manipulating-the-selected-items"></a>Bearbeiten die ausgewählten Elemente

Die folgenden Methoden und Eigenschaften können Sie die ausgewählten Elemente aus den Menu-Steuerelement zu bearbeiten:

- `SelectItem` -Das Element am angegebenen Index ausgewählt.
- `Select` – Wählen Sie den Wert des angegebenen Elements.
- `DeselectItem` – Hebt die Auswahl des Elements am angegebenen Index.
- `SelectedIndex` -Gibt den Index des derzeit ausgewählten Elements.
- `SelectedValue` -Gibt den Wert des derzeit ausgewählten Elements.

Verwenden der `ScrollItemAtIndexToTop` um das Element am angegebenen Index am oberen Rand der Liste zu präsentieren und `ScrollItemAtIndexToVisible` Liste scrollen, bis das Element am angegebenen Index angezeigt wird.

<a name="Responding to Events" />

### <a name="responding-to-events"></a>Reagieren auf Ereignisse

Menu-Steuerelemente bieten die folgenden Ereignisse ein, um auf Benutzerinteraktionen reagieren:

- `SelectionChanged` -Wird aufgerufen, wenn der Benutzer einen Wert aus der Liste ausgewählt hat.
- `SelectionIsChanging` -Wird aufgerufen, bevor das neue vom Benutzer ausgewählten Element der aktuellen Auswahl wird.
- `WillPopup` -Wird aufgerufen, bevor die Dropdown-Liste von Elementen angezeigt wird.
- `WillDismiss` -Wird aufgerufen, bevor die Dropdown-Liste der Elemente geschlossen wird.

Für `NSComboBox` -Steuerelemente, dazu gehören alle die gleichen Ereignisse als die `NSTextField`, z. B. die `Changed` -Ereignis, das aufgerufen wird, sobald der Benutzer den Wert des Texts im Kombinationsfeld bearbeitet.

Optional können Sie reagieren die eine interne Daten Menüelemente definiert in Interface Builder ausgewählt werden, indem Sie das Element, das Anfügen einer **Aktion** und verwenden Sie Code wie folgt auf reagieren **Aktion** wird vom Benutzer ausgelöst:

```csharp
partial void ItemOne (Foundation.NSObject sender) {
    DropDownSelected.Title = "Item 1";
    FeedbackLabel.StringValue = "Item One Selected";
}
```

Weitere Informationen zum Arbeiten mit Menüs und Menu-Steuerelemente finden Sie unserem [Menüs](~/mac/user-interface/menu.md) und [Popup-Schaltfläche und Dropdownmenü listet](~/mac/user-interface/menu.md) Dokumentation.

<a name="Working_with_Selection_Controls" />

## <a name="working-with-selection-controls"></a>Arbeiten mit Auswahlsteuerelemente

AppKit enthält mehrere Typen von Steuerelementen für die Auswahl, die in Ihrem Entwurf der Benutzeroberfläche verwendet werden kann. Weitere Informationen finden Sie unter den [Auswahlsteuerelemente](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsSelection.html#//apple_ref/doc/uid/20000957-CH49-SW1) Abschnitt des Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/select01.png "Beispiel für Auswahlsteuerelemente")](standard-controls-images/select01.png#lightbox)

Es gibt zwei Möglichkeiten, um nachzuverfolgen, wann ein Auswahlsteuerelement Eingreifen des Benutzers, wurde als machen eine **Aktion**. Zum Beispiel:

```csharp
partial void SegmentButtonPressed (Foundation.NSObject sender) {
    FeedbackLabel.StringValue = string.Format("Button {0} Pressed",SegmentButtons.SelectedSegment);
}
```

Oder durch das Anfügen einer **Delegaten** auf die `Activated` Ereignis. Zum Beispiel:

```csharp
TickedSlider.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Stepper Value: {0:###}",TickedSlider.IntValue);
};
```

Verwenden Sie zum Festlegen, oder lesen den Wert des einem Auswahl-Steuerelement, das `IntValue` Eigenschaft. Zum Beispiel:

```csharp
FeedbackLabel.StringValue = string.Format("Stepper Value: {0:###}",TickedSlider.IntValue);
```

Die spezielle Steuerelemente (z. B. Farbe auch und auch Images) müssen bestimmte Eigenschaften für die Werttypen. Beispiel:

```csharp
CollorWell.Color = NSColor.Red;
ImageWell.Image = NSImage.ImageNamed ("tag.png");

```

Die `NSDatePicker` hat die folgenden Eigenschaften zum direkten Arbeiten mit Datum und Uhrzeit:

- **DateValue** – der aktuelle Datum und Uhrzeit-Wert als eine `NSDate`.
- **Lokale** -Standort des Benutzers, als eine `NSLocal`.
- **TimeInterval** -den Zeitwert als eine `Double`.
- **TimeZone** -Zeitzone des Benutzers, wie eine `NSTimeZone`.

<a name="Working_with_Indicator_Controls" />

## <a name="working-with-indicator-controls"></a>Arbeiten mit Indikator-Steuerelementen

AppKit enthält mehrere Typen von Indicator-Steuerelemente, die in Ihrem Entwurf der Benutzeroberfläche verwendet werden kann. Weitere Informationen finden Sie unter den [Indikator Steuerelemente](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsIndicators.html#//apple_ref/doc/uid/20000957-CH50-SW1) Abschnitt des Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/level01.png "Beispiel-Indikator-Steuerelemente")](standard-controls-images/level01.png#lightbox)

Es gibt zwei Möglichkeiten, um nachzuverfolgen, wann ein Indikator-Steuerelement auf Benutzerinteraktionen, entweder als machen verfügt über eine **Aktion** oder **Outlet** und Anfügen einer **Delegaten** auf die `Activated`Ereignis. Zum Beispiel:

```csharp
LevelIndicator.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Level: {0:###}",LevelIndicator.DoubleValue);
};
```

Verwenden Sie zum Lesen, oder legen Sie den Wert des Steuerelements Indikator, der `DoubleValue` Eigenschaft. Zum Beispiel:

```csharp
FeedbackLabel.StringValue = string.Format("Rating: {0:###}",Rating.DoubleValue);
```

Bei der Anzeige, sollte die unbestimmt und asynchrone Statusanzeigen animiert werden. Verwenden der `StartAnimation` Methode, um die Animation zu starten, wenn sie angezeigt werden. Zum Beispiel:

```csharp
Indeterminate.StartAnimation (this);
AsyncProgress.StartAnimation (this);
```

Aufrufen der `StopAnimation` Methode wird die Animation beendet.

<a name="Working_with_Text_Controls" />

## <a name="working-with-text-controls"></a>Arbeiten mit Text-Steuerelemente

AppKit enthält mehrere Typen von Text-Steuerelemente, die in Ihrem Entwurf der Benutzeroberfläche verwendet werden kann. Weitere Informationen finden Sie unter den [Textsteuerelemente](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsText.html#//apple_ref/doc/uid/20000957-CH51-SW1) Abschnitt des Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/text01.png "Beispiel-Text-Steuerelemente")](standard-controls-images/text01.png#lightbox)

Für Textfelder (`NSTextField`), die folgenden Ereignisse können verwendet werden, um die Benutzerinteraktion zu verfolgen:

- **Geändert** -wird jedes Mal, die der Benutzer den Wert des Felds ändert ausgelöst. Z. B. auf jedem Zeichen eingegeben.
- **EditingBegan** -wird ausgelöst, wenn der Benutzer das Feld für die Bearbeitung ausgewählt.
- **EditingEnded** : Wenn der Benutzer die EINGABETASTE in das Feld drückt oder das Feld lässt.

Verwenden der `StringValue` Eigenschaft lesen oder Festlegen der Wert des Felds. Zum Beispiel:

```csharp
FeedbackLabel.StringValue = string.Format("User ID: {0}",UserField.StringValue);
```

Für Felder, die anzeigen oder Bearbeiten von numerischen Werten, können Sie die `IntValue` Eigenschaft. Zum Beispiel:

```csharp
FeedbackLabel.StringValue = string.Format("Number: {0}",NumberField.IntValue);
```

Ein `NSTextView` bietet eine Volltextsuche für die ausgewählte bearbeiten und Anzeigen des Bereichs mit den integrierten formatieren. Wie eine `NSTextField`, verwenden Sie die `StringValue` Eigenschaft lesen oder Festlegen des Bereichs-Wert.

Ein Beispiel eines komplexen Beispiels arbeiten mit Text-Ansichten in einer Xamarin.Mac-app, finden Sie unter den [SourceWriter-Beispiel-App](https://developer.xamarin.com/samples/mac/SourceWriter/). SourceWriter ist ein einfacher Quellcode-Editor, der Unterstützung für die Codevervollständigung und die einfache Syntaxhervorhebung bietet.

Der SourceWriter-Code wurde vollständig kommentiert und es wurden, wenn möglich, Links der Schlüsseltechnologien und -methoden zu wichtigen Informationen in den Leitfäden der Xamarin.Mac-Dokumentation bereitgestellt.

<a name="Working_with_Content_Views" />

## <a name="working-with-content-views"></a>Arbeiten mit Inhalten Ansichten

AppKit enthält mehrere Typen von Content-Sichten, die in Ihrem Entwurf der Benutzeroberfläche verwendet werden kann. Weitere Informationen finden Sie unter den [Inhaltsanzeigen](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1) Abschnitt des Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

[![](standard-controls-images/content01.png "Eine Beispiel-Ansicht \"Inhalt\"")](standard-controls-images/content01.png#lightbox)

<a name="Popovers" />

### <a name="popovers"></a>Popovers

Eine im Popover ist ein vorübergehender UI-Element, das Funktionen bereitstellt, die direkt mit einem bestimmten ein Steuerelement oder auf einen Bereich auf dem Bildschirm zusammenhängt. Eine im Popover über dem Fenster mit dem Steuerelement oder ein Bereich, der zugehörigen floats-Werte und der Rahmen enthält einen Pfeil, um den Punkt angeben, von dem er entwickelt.

Führen Sie folgende Schritte aus, um eine im Popover zu erstellen:

1. Öffnen der `.storyboard` Datei des Fensters, das eine Popover, durch Doppelklick im hinzugefügt werden soll die **Projektmappen-Explorer**
2. Ziehen Sie eine **Domänencontroller anzeigen** aus der **Bibliotheksinspektor** auf die **Schnittstellen-Editor**: 

    [![](standard-controls-images/content02.png "Wenn Sie einen Ansichtscontroller auswählen, aus der Bibliothek")](standard-controls-images/content02.png#lightbox)
4. Definieren Sie die Größe und das Layout der **benutzerdefinierte Sicht**: 

    [![](standard-controls-images/content04.png "Bearbeiten des Layouts")](standard-controls-images/content04.png#lightbox)
5. Strg + Klick und ziehen Sie aus der Quelle des Popups auf den **Ansichtscontroller**: 

    [![](standard-controls-images/content05.png "Ziehen zum Erstellen eines segues")](standard-controls-images/content05.png#lightbox)
6. Wählen Sie **Popover** im Popup-Menü: 

    [![](standard-controls-images/content06.png "Segue-Typ")](standard-controls-images/content06.png#lightbox)
7. Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode synchronisiert.

<a name="Tab_Views" />

### <a name="tab-views"></a>Registerkartenansichten

Registerkartenansichten besteht aus einer Liste auf der Registerkarte (die ähnelt ein segmentiertes Steuerelement) in Kombination mit einer Gruppe von Ansichten, die aufgerufen werden _Bereiche_. Wenn der Benutzer eine neue Registerkarte auswählt, wird der Bereich, der an sie angefügt ist angezeigt. Jeder Bereich enthält einen eigenen Satz von Steuerelementen.

Verwenden Sie bei der Arbeit mit Interface Builder von Xcode eine Registerkartenansicht der **Attributinspektor** festlegen die Anzahl der Registerkarten:

[![](standard-controls-images/content08.png "Bearbeiten die Anzahl der Registerkarten")](standard-controls-images/content08.png#lightbox)

Wählen Sie für jede Registerkarte in der **Schnittstellenhierarchie** Festlegen seiner **Titel** und Hinzufügen von UI-Elemente für die **Bereich**:

[![](standard-controls-images/content09.png "Bearbeiten die Registerkarten in Xcode")](standard-controls-images/content09.png#lightbox)

<a name="Data_Binding_AppKit_Controls" />

## <a name="data-binding-appkit-controls"></a>Datenbindung AppKit-Steuerelementen

Techniken für Schlüssel / Wert-Codierung und Binden von Daten in Ihre Xamarin.Mac-Anwendung verwenden, können Sie die Menge des Codes, die Sie schreiben und verwalten, die zum Auffüllen und zum Arbeiten mit UI-Elemente, erheblich reduzieren. Sie haben außerdem den Vorteil, dass weitere entkoppeln Ihre Daten sichern (_Datenmodell_) aus Ihrem Front end-Benutzeroberfläche (_Model-View-Controller_), wodurch einfacher zu verwalten, eine flexiblere Anwendung Entwurf.

Schlüssel-Wert-Codierung (KVM) ist ein Mechanismus für den Zugriff auf die Eigenschaften eines Objekts ist indirekt, mithilfe von Schlüsseln (speziell formatierte Zeichenfolgen) zum Identifizieren von Eigenschaften, statt den Zugriff über Instanzvariablen oder Methoden der eigenschaftenzugriffsmethode (`get/set`). Durch die Implementierung von Schlüssel / Wert-Codierung kompatibel Accessors in Ihre Xamarin.Mac-Anwendung, erhalten Sie Zugriff auf andere MacOS-Funktionen wie z. B. das beobachten von Schlüssel-Wert (KVO), Datenbindung, Kerndaten, Cocoa-Bindungen und Scriptability.

Weitere Informationen finden Sie unter den [einfache Datenbindung](~/mac/app-fundamentals/databinding.md#Simple_Data_Binding) Teil unserer [Datenbindung und Schlüssel / Wert-Codierung](~/mac/app-fundamentals/databinding.md) Dokumentation.


<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat es sich um einen detaillierten Einblick in die Arbeit mit den standardmäßigen AppKit-Steuerelementen wie Schaltflächen, Bezeichnungen, Textfelder, Kontrollkästchen und segmentierte Steuerelemente in einer Xamarin.Mac-Anwendung geführt. Sie behandelt, hinzufügen zu einem Entwurf der Benutzeroberfläche in Interface Builder von Xcode für Code mit Outlets und Aktionen verfügbar zu machen und Verwenden von AppKit-Steuerelementen in C#-Code.

## <a name="related-links"></a>Verwandte Links

- [MacControls (Beispiel)](https://developer.xamarin.com/samples/mac/MacControls/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Windows](~/mac/user-interface/window.md)
- [Datenbindung und Schlüssel/Wert-Codierung](~/mac/app-fundamentals/databinding.md)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Informationen zu Steuerelementen und Ansichten](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1)
