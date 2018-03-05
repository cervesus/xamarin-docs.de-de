---
title: Standardsteuerelemente
description: "In diesem Artikel wird das Arbeiten mit den standardmäßigen AppKit Steuerelementen wie Schaltflächen, Bezeichnungen, Textfelder, Kontrollkästchen behandelt, und Steuerelemente in einer Anwendung Xamarin.Mac segmentiert. Eine Schnittstelle mit dem Benutzeroberflächen-Generator hinzugefügt und mit ihnen interagieren, im Code beschrieben."
ms.topic: article
ms.prod: xamarin
ms.assetid: d2593883-d255-431f-9781-75f04d8cecea
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: e6df7f9308285b87ff0f42b73c8404b375cbb0de
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="standard-controls"></a>Standardsteuerelemente

_In diesem Artikel wird das Arbeiten mit den standardmäßigen AppKit Steuerelementen wie Schaltflächen, Bezeichnungen, Textfelder, Kontrollkästchen behandelt, und Steuerelemente in einer Anwendung Xamarin.Mac segmentiert. Eine Schnittstelle mit dem Benutzeroberflächen-Generator hinzugefügt und mit ihnen interagieren, im Code beschrieben._

Bei der Arbeit mit c# und .NET in einer Anwendung Xamarin.Mac haben Sie Zugriff auf die gleiche AppKit steuert, die ein Entwickler arbeiten in unter *Objective-C* und *Xcode* verfügt. Da Xamarin.Mac direkt mit Xcode integriert ist, können Sie die Xcode _Schnittstelle-Generator_ zu erstellen und Verwalten Ihrer Appkit-Steuerelemente (oder erstellen sie optional direkt im C#-Code).

AppKit-Steuerelemente sind die Elemente der Benutzeroberfläche, die verwendet werden, um die Benutzeroberfläche der Anwendung Xamarin.Mac zu erstellen. Sie bestehen aus Elementen wie Schaltflächen, Bezeichnungen, Textfelder, Kontrollkästchen und segmentierte Steuerelemente und instant Aktionen oder sichtbar Ergebnisse verursachen, wenn ein Benutzer werden bearbeitet.

[ ![](standard-controls-images/intro01.png "Die Beispiel-app-Hauptbildschirm")](standard-controls-images/intro01.png)

In diesem Artikel werden die Grundlagen der Arbeit mit AppKit-Steuerelementen in einer Anwendung Xamarin.Mac eingegangen. Wird mit hoher vorgeschlagen, dass Sie über arbeiten die [Hello, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst, insbesondere die [Einführung in Xcode und Benutzeroberflächen-Generator](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) und [Steckdosen und Aktionen](~/mac/get-started/hello-mac.md#Outlets_and_Actions) Abschnitte, wie sie behandelt wichtige Konzepte und Techniken, die in diesem Artikel verwendet werden.

Sie möchten einen Blick auf die [Verfügbarmachen von C#-Klassen / Methoden für Objective-C-](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentieren, es wird erläutert, die `Register` und `Export` Befehle verwendet, um über das Netzwerk Ihrer C#-Klassen für Objective-C-Objekte und Elemente der Benutzeroberfläche von Volltextkatalogen.

<a name="Introduction_to_Controls_and_Views" />

## <a name="introduction-to-controls-and-views"></a>Einführung in die Steuerelemente und Ansichten

MacOS (ehemals Mac OS X) bietet einen Standardsatz von Benutzeroberfläche Steuerelemente über die AppKit-Framework. Sie bestehen aus Elementen wie Schaltflächen, Bezeichnungen, Textfelder, Kontrollkästchen und segmentierte Steuerelemente und instant Aktionen oder sichtbar Ergebnisse verursachen, wenn ein Benutzer werden bearbeitet.

Alle Steuerelemente AppKit haben eine standardmäßige, vordefinierte Darstellung, die für die meisten Zwecke geeignet sind, einige Geben Sie einer alternativen Darstellung für die Verwendung in einem Bereich des Fensters Frame oder in einer _Dynamik Auswirkungen_ Kontext, z. B. in einem Randleistenbereich oder in einer Mitteilungszentrale-Widget.

Apple empfehlen die folgenden Richtlinien, bei der Arbeit mit AppKit-Steuerelemente:

- Vermeiden Sie das Mischen von Steuerelementgrößen in derselben Ansicht.
- Vermeiden Sie generell Steuerungselemente vertikal.
- Verwenden Sie die Systemschriftart und die richtigen Textgröße in einem Steuerelement.
- Verwenden Sie den richtigen Abstand zwischen Steuerelementen.

Pleas finden Sie weitere Informationen die [über Steuerelemente und Ansichten](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1) Abschnitt der Apple [OS X-Richtlinien für menschliche Benutzeroberfläche](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

<a name="Using_Controls_in_a_Window_Frame" />

### <a name="using-controls-in-a-window-frame"></a>Verwenden von Steuerelementen in einem Fenster Frame

Es gibt eine Teilmenge der AppKit-Steuerelemente, die eine Anzeigestil enthalten, die sie im Bereich des Fensters "Frame" eingeschlossen werden können. Ein Beispiel finden Sie in der e-Mail-app-Symbolleiste:

[ ![](standard-controls-images/mailapp.png "Einen Mac-Fensterrahmen")](standard-controls-images/mailapp.png)

- **Runden Schaltfläche strukturierten** – eine `NSButton` mit dem Format `NSTexturedRoundedBezelStyle`.
- **Segmentierte Steuerelement gerundet strukturierten** – eine `NSSegmentedControl` mit dem Format `NSSegmentStyleTexturedRounded`.
- **Segmentierte Steuerelement gerundet strukturierten** – eine `NSSegmentedControl` mit dem Format `NSSegmentStyleSeparated`.
- **Runden von strukturierten Popupmenü** – eine `NSPopUpButton` mit dem Format `NSTexturedRoundedBezelStyle`.
- **Runden von strukturierten Dropdown-Menü** – eine `NSPopUpButton` mit dem Format `NSTexturedRoundedBezelStyle`.
- **Suchleiste** – ein `NSSearchField`.

Apple empfehlen die folgenden Richtlinien, bei der Arbeit mit AppKit-Steuerelemente in einem Fenster Frame:

- Verwenden Sie keine bestimmten Steuerelementtypen Fensterrahmen im Hauptteil Fensters.
- Verwenden Sie keinen Text Fenster Steuerelemente oder Stile in der Fensterrahmen.

Pleas finden Sie weitere Informationen die [über Steuerelemente und Ansichten](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1) Abschnitt der Apple [OS X-Richtlinien für menschliche Benutzeroberfläche](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

<a name="Creating_a_User_Interface_in_Interface_Builder" />

## <a name="creating-a-user-interface-in-interface-builder"></a>Erstellen einer Benutzeroberfläche im Benutzeroberflächen-Generator

Wenn Sie eine neue Xamarin.Mac Kakao-Anwendung erstellen, erhalten Sie Standardfensters leer ist, wird standardmäßig an. Dieses Windows wird definiert, einem `.storyboard` automatisch im Projekt enthaltene Datei. So bearbeiten Sie die Windows-Design in der **Projektmappen-Explorer**, doppelklicken klicken Sie auf die `Main.storyboard` Datei:

[ ![](standard-controls-images/edit01.png "Die Haupt-Storyboard auswählen im Projektmappen-Explorer")](standard-controls-images/edit01.png)

Dies öffnet das Fenster Design in Xcodes Benutzeroberflächen-Generator:

[ ![](standard-controls-images/edit02.png "Bearbeiten das Storyboard in Xcode")](standard-controls-images/edit02.png)

Um die Benutzeroberfläche zu erstellen, müssen Sie Benutzeroberflächenelemente (AppKit Steuerelemente) ziehen, aus der **Bibliothek Inspektor** auf die **Benutzeroberflächen-Editors** in Benutzeroberflächen-Generator. Im folgenden Beispiel wird eine **vertikalen geteilten Ansicht** Steuerelement wurde Medikament aus der **Bibliothek Inspektor** und platziert Sie im Fenster in der **Benutzeroberflächen-Editors**:

[ ![](standard-controls-images/edit03.png "Auswählen einer geteilten Ansicht aus der Bibliothek")](standard-controls-images/edit03.png)

Weitere Informationen zum Erstellen einer Benutzeroberfläche im Benutzeroberflächen-Generator finden Sie unter unsere [Einführung in Xcode und Benutzeroberflächen-Generator](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) Dokumentation.

<a name="Sizing_and_Positioning" />

### <a name="sizing-and-positioning"></a>Ändern der Größe und Positionierung

Sobald ein Steuerelement in der Benutzeroberfläche eingeschlossen wurde, verwenden die **-Editors** zum Festlegen von seiner Position und Größe von manuell eingeben von Werten und steuern, wie das Steuerelement automatisch positioniert ist, und wenn die Größe angepasst die übergeordneten Fenster oder eine Sicht die Größe wird geändert:

[ ![](standard-controls-images/edit04.png "Die Einschränkungen festlegen")](standard-controls-images/edit04.png)

Verwenden der **I-Balken rot** auf der Außenseite des der **Autoresizing** Feld _Knüppel_ eines Steuerelements an einen Speicherort (x, y). Zum Beispiel: 

[ ![](standard-controls-images/edit05.png "Eine Einschränkung bearbeiten")](standard-controls-images/edit05.png)

Gibt an, dass das ausgewählte Steuerelement (in der **Ansicht der Gruppenhierarchie** & **Benutzeroberflächen-Editors**) wie geändert oder verschoben wird an den oberen und rechten Speicherort des Fensters bzw. der Sicht hängen. 

Andere Elemente des Editors Steuerelementeigenschaften, z. B. Höhe und Breite:

[ ![](standard-controls-images/edit06.png "Die Höhe festlegen")](standard-controls-images/edit06.png)

Sie können auch die Ausrichtung der Elemente steuern, mit Einschränkungen mithilfe der **Ausrichtung Editor**:

[ ![](standard-controls-images/edit07.png "Die Ausrichtung-Editor")](standard-controls-images/edit07.png)

> [!IMPORTANT]
> Im Gegensatz zu iOS, in denen (0,0) wird von die oberen linken Ecke des Bildschirms in MacOS (0,0) ist der linken unteren Ecke. Dies ist, da MacOS mit der numerischen Werte in den Wert zu erhöhen, nach oben und nach einem mathematischen Koordinatensystem verwendet. Sie müssen dies berücksichtigt werden, wenn AppKit Steuerelemente auf einer Benutzeroberfläche zu platzieren.

<a name="Setting_a_Custom_Class" />

### <a name="setting-a-custom-class"></a>Festlegen einer benutzerdefinierten Klasse

Es gibt Situationen, wenn müssen-Unterklasse und vorhandenen Steuerelement und erstellen Sie eigene angepasste Version dieser Klasse wird mit AppKit Steuerelemente, die Sie arbeiten. Definieren z. B. eine benutzerdefinierte Version der Quellliste aus:

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

In dem die `[Register("SourceListView")]` Anweisung macht die `SourceListView` Klasse Objective-C ist darum der kann in Benutzeroberflächen-Generator verwendet werden. Weitere Informationen finden Sie unter der [Verfügbarmachen von C#-Klassen / Methoden für Objective-C-](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentieren, es wird erläutert, die `Register` und `Export` verwendeten Befehle um C#-Klassen für Objective-C-Objekte und Elemente der Benutzeroberfläche anschließen.

Mit dem oben aufgeführten Code erfüllt, können Sie ein Steuerelement AppKit des Basistyps, die Sie erweitern, in der Entwurfsoberfläche ziehen (im folgenden Beispiel wird eine **Quellliste**), wechseln Sie zu der **Identität Inspektor** und festlegen die **benutzerdefinierte Klasse** auf den Namen, die Sie für Objective-C verfügbar gemacht (Beispiel `SourceListView`):

[ ![](standard-controls-images/edit10.png "Festlegen einer benutzerdefinierten Klasse in Xcode")](standard-controls-images/edit10.png)

<a name="Exposing_Outlets_and_Actions" />

### <a name="exposing-outlets-and-actions"></a>Verfügbarmachen von Steckdosen und Aktionen

Bevor ein Steuerelement AppKit in C#-Code zugegriffen werden kann, muss es entweder als verfügbar gemacht werden ein **Nachrichtenplattform** oder und **Aktion**. Wählen Sie hierzu das angegebene Steuerelement entweder in der **Schnittstellenhierarchie** oder **Benutzeroberflächen-Editors** und wechseln Sie zu der **Assistant Ansicht** (Stellen Sie sicher, dass Sie die verfügen`.h`des Fensters für die Bearbeitung ausgewählt):

[ ![](standard-controls-images/edit11.png "Wählen die richtige Datei bearbeiten")](standard-controls-images/edit11.png)

Steuerelement ziehen aus dem Steuerelement AppKit auf angegebenen `.h` Datei beim Starten der Erstellung einer **Nachrichtenplattform** oder **Aktion**:

[ ![](standard-controls-images/edit12.png "Ziehen zum Erstellen von einer Steckdose oder Aktion")](standard-controls-images/edit12.png)

Wählen Sie den Typ der Offenlegung zu erstellen, und geben die **Nachrichtenplattform** oder **Aktion** eine **Namen**: 

[ ![](standard-controls-images/edit13.png "Konfigurieren den Ausgang oder die Aktion")](standard-controls-images/edit13.png)


Weitere Informationen zum Arbeiten mit **Steckdosen** und **Aktionen**, finden Sie unter der [Steckdosen und Aktionen](~/mac/get-started/hello-mac.md#Outlets_and_Actions) Teil unserer [Einführung in Xcode und -Schnittstelle Builder](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) Dokumentation.

<a name="Synchronizing_Changes_with_Xcode" />

### <a name="synchronizing-changes-with-xcode"></a>Synchronisieren von Änderungen mit Xcode

Wenn Sie wieder zu Visual Studio für Mac von Xcode wechseln, werden alle Änderungen, die Sie in Xcode vorgenommen haben in Ihrem Projekt Xamarin.Mac automatisch synchronisiert.

Bei Auswahl der `SplitViewController.designer.cs` in der **Projektmappen-Explorer** können, finden Sie unter wie Ihre **Nachrichtenplattform** und **Aktion** im C#-Code Verknüpfung:

[ ![](standard-controls-images/sync01.png "Synchronisieren der Änderungen mit Xcode")](standard-controls-images/sync01.png)

Beachten Sie, dass wie die Definition in der `SplitViewController.designer.cs` Datei:

```csharp
[Outlet]
AppKit.NSSplitViewItem LeftController { get; set; }

[Outlet]
AppKit.NSSplitViewItem RightController { get; set; }

[Outlet]
AppKit.NSSplitView SplitView { get; set; }
```

Mit der Definition in der Zeile darüber die `MainWindow.h` Datei in Xcode befinden:

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

Wie Sie sehen können, Visual Studio für Mac überwacht Änderungen an der `.h` Datei, und klicken Sie dann diese Änderungen in der jeweiligen automatisch synchronisiert `.designer.cs` Datei, die sie für Ihre Anwendung verfügbar gemacht werden. Möglicherweise auch feststellen, dass `SplitViewController.designer.cs` ist eine partielle Klasse so, dass Visual Studio für Mac nicht ändern `SplitViewController.cs ` würde die, die wir auf die Klasse vorgenommenen Änderungen überschrieben.

Normalerweise werden nie müssen Sie öffnen die `SplitViewController.designer.cs` selbst, es wurde hier vorgestellten nur zu Lernzwecken.

> [!IMPORTANT]
> Visual Studio für Mac wird in den meisten Fällen finden Sie in Xcode vorgenommenen Änderungen und dem Projekt Xamarin.Mac zu synchronisieren. Sollte die Synchronisierung nicht automatisch durchgeführt werden – was sehr selten passiert –, wechseln Sie zurück zu Xcode und dann erneut zu Visual Studio für Mac. Dadurch wird normalerweise der Synchronisierungsprozess gestartet.

<a name="Working_with_Buttons" />

## <a name="working-with-buttons"></a>Arbeiten mit Schaltflächen

AppKit bietet verschiedene Schaltflächentyp, der in Ihrem Entwurf der Benutzeroberfläche verwendet werden kann. Weitere Informationen finden Sie unter der [Schaltflächen](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsButtons.html#//apple_ref/doc/uid/20000957-CH48-SW1) Abschnitt der Apple [OS X-Richtlinien für menschliche Benutzeroberfläche](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[ ![](standard-controls-images/buttons01.png "Ein Beispiel für die verschiedenen Typen")](standard-controls-images/buttons01.png)

Wenn Sie über eine Schaltfläche verfügbar gemacht wurde ein **Nachrichtenplattform**, der folgende Code wird beantwortet gedrückt wird:

```csharp
ButtonOutlet.Activated += (sender, e) => {
        FeedbackLabel.StringValue = "Button Outlet Pressed";
};
```

Für Schaltflächen, die über verfügbar gemacht wurden **Aktionen**, eine `public partial` Methode wird automatisch für Sie erstellt werden, mit dem Namen, den Sie in Xcode ausgewählt haben. So reagieren Sie auf die **Aktion**, schließen Sie die partielle Methode in der Klasse, die die **Aktion** definiert wurde. Zum Beispiel:

```csharp
partial void ButtonAction (Foundation.NSObject sender) {
    // Do something in response to the Action
    FeedbackLabel.StringValue = "Button Action Pressed";
}
```

Für Schaltflächen, die einen Status aufweisen (wie **auf** und **deaktiviert**), kann der Status überprüft oder festgelegt werden, mit der `State` von Eigenschaften und die `NSCellStateValue` Enum. Zum Beispiel:

```csharp
DisclosureButton.Activated += (sender, e) => {
    LorumIpsum.Hidden = (DisclosureButton.State == NSCellStateValue.On);
};
```

Wobei `NSCellStateValue` sind möglich:

- **Auf** : die Schaltfläche wird per Push übertragen oder das Steuerelement ausgewählt ist (z. B. eine Überprüfung in ein Kontrollkästchen).
- **Deaktiviert** : die Schaltfläche "" ist nicht weitergegeben wurden oder das Steuerelement nicht ausgewählt ist.
- **Gemischte** -eine Mischung von **auf** und **deaktiviert** Status.

<a name="Mark-a-Button-as-Default-and-Set-Key-Equivalent" />

### <a name="mark-a-button-as-default-and-set-key-equivalent"></a>Markieren Sie eine Schaltfläche als Standard, und legen Sie Key Entsprechung

Für keine der Schaltflächen, die Sie einen Entwurf der Benutzeroberfläche hinzugefügt haben, können Sie diese Schaltfläche als kennzeichnen die _Standard_ Schaltfläche, die aktiviert wird, wenn der Benutzer drückt die **Return/EINGABETASTE** auf der Tastatur die Taste. In MacOS erhält diese Schaltfläche wird standardmäßig eine blaue Hintergrundfarbe.

Um eine Schaltfläche als Standard festzulegen, wählen Sie es in Xcodes Benutzeroberflächen-Generator aus. Im nächsten Schritt in der **Attribut Inspektor**, wählen die **Schlüssel entspricht** Feld, und drücken Sie die **Return/EINGABETASTE** Schlüssel:

[ ![](standard-controls-images/buttons03.png "Bearbeiten die wichtigsten Entsprechung")](standard-controls-images/buttons03.png)

Ebenso können Sie eine beliebige Abfolge zuweisen, die zum Aktivieren der Schaltfläche mit des über die Tastatur anstelle der Maus verwendet werden können. Z. B. durch Drücken der Befehl C in der Abbildung oben.

Wenn die app ausgeführt wird und das Fenster mit der Schaltfläche Schlüssel und konzentriert, wenn der Benutzer den Befehl + C drückt, wird die Aktion für die Schaltfläche aktiviert (als – wenn der Benutzer auf die Schaltfläche geklickt wurde).

<a name="Working_with_Checkboxes_and_Radio_Buttons" />

## <a name="working-with-checkboxes-and-radio-buttons"></a>Arbeiten mit Kontrollkästchen und Optionsfelder

AppKit bietet verschiedene Typen von Kontrollkästchen und Optionsfeldgruppen, die in Ihrem Entwurf der Benutzeroberfläche verwendet werden kann. Weitere Informationen finden Sie unter der [Schaltflächen](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsButtons.html#//apple_ref/doc/uid/20000957-CH48-SW1) Abschnitt der Apple [OS X-Richtlinien für menschliche Benutzeroberfläche](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[ ![](standard-controls-images/buttons02.png "Ein Beispiel für die verfügbaren Kontrollkästchen-Typen")](standard-controls-images/buttons02.png)


Kontrollkästchen und Optionsfelder (über verfügbar gemachte **Steckdosen**) verfügen über einen Zustand (wie **auf** und **deaktiviert**), kann der Status überprüft oder festgelegt werden, mit der `State` von Eigenschaften und die `NSCellStateValue` Enum. Zum Beispiel:

```csharp
AdjustTime.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Adjust Time: {0}",AdjustTime.State == NSCellStateValue.On);
};
```

Wobei `NSCellStateValue` sind möglich:

- **Auf** : die Schaltfläche wird per Push übertragen oder das Steuerelement ausgewählt ist (z. B. eine Überprüfung in ein Kontrollkästchen).
- **Deaktiviert** : die Schaltfläche "" ist nicht weitergegeben wurden oder das Steuerelement nicht ausgewählt ist.
- **Gemischte** -eine Mischung von **auf** und **deaktiviert** Status.

Verfügbar zu machen, wählen Sie das Optionsfeld, um eine Schaltfläche in einer Gruppe von Optionsfeldern auswählen, eine **Nachrichtenplattform** und legen Sie dessen `State` Eigenschaft. Beispiel:

```csharp
partial void SelectCar (Foundation.NSObject sender) {
    TransportationCar.State = NSCellStateValue.On;
    FeedbackLabel.StringValue = "Car Selected";
}
```

Um eine Auflistung von Optionsfeldern fungieren als eine Gruppe, und behandeln ggf. automatisch den ausgewählten Status zu erhalten, erstellen Sie ein neues **Aktion** sowie jede Schaltfläche in der Gruppe zuordnen:

![](standard-controls-images/buttons04.png "Erstellen eine neue Aktion")

Als Nächstes weisen Sie einen eindeutigen `Tag` für jedes RadioButton in der **Attribut Inspektor**:

![](standard-controls-images/buttons05.png "Bearbeiten einer Radio Button-tag")

Die Änderungen zu speichern und zurück zu Visual Studio für Mac, fügen Sie den Code für die Behandlung der **Aktion** , dass alle von der Optionsfelder an angefügt sind:

```csharp
partial void NumberChanged(Foundation.NSObject sender)
{
    var check = sender as NSButton;
    Console.WriteLine("Changed to {0}", check.Tag);
}
```

Sie können die `Tag` Eigenschaft, um festzustellen, welches Optionsfeld ausgewählt wurde.

<a name="Working_with_Menu_Controls" />

## <a name="working-with-menu-controls"></a>Arbeiten mit Menüsteuerelemente

AppKit bietet mehrere Typen von Menü-Steuerelemente, die in Ihrem Entwurf der Benutzeroberfläche verwendet werden kann. Weitere Informationen finden Sie unter der [Menüsteuerelemente](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlswithMenus.html#//apple_ref/doc/uid/20000957-CH100-SW1) Abschnitt der Apple [OS X-Richtlinien für menschliche Benutzeroberfläche](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[ ![](standard-controls-images/menu01.png "Beispiel-Menüsteuerelemente")](standard-controls-images/menu01.png)

<a name="Providing-Menu-Control-Data" />

### <a name="providing-menu-control-data"></a>Menü-Steuerelementdaten bereitstellt

Die Menüsteuerelemente MacOS kann festgelegt werden kann, um der Dropdown-Liste entweder von einer internen Aufgabenliste Auffüllen (die können werden vordefinierte in Benutzeroberflächen-Generator oder über Code aufgefüllt) oder durch eine eigene benutzerdefinierte, externe Datenquelle bereitstellen.

<a name="Working-with-Internal-Data" />

#### <a name="working-with-internal-data"></a>Arbeiten mit internen Daten

Zusätzlich zum Definieren von Elementen im Menüsteuerelemente Schnittstelle-Generator (z. B. `NSComboBox`), geben Sie einen vollständigen Satz von Methoden, mit denen Sie hinzufügen, bearbeiten oder löschen die Elemente aus der internen Liste, die sie verwalten:

- `Add` -Fügt ein neues Element an das Ende der Liste.
- `GetItem` -Gibt das Element am angegebenen Index zurück.
- `Insert` -Fügt ein neues Element in der Liste am angegebenen Speicherort.
- `IndexOf` -Gibt den Index des angegebenen Elements zurück.
- `Remove` -Entfernt das angegebene Element aus der Liste.
- `RemoveAll` – Entfernt alle Elemente aus der Liste.
- `RemoveAt` -Entfernt das Element am angegebenen Index.
- `Count` -Die Anzahl der Elemente in der Liste zurückgegeben.

> [!IMPORTANT]
> Bei Verwendung von einer Datenquelle von "extern" (`UsesDataSource = true`), bei dem oben genannten Methoden aufrufen, wird eine Ausnahme ausgelöst.

<a name="Working-with-an-External-Data-Source" />

#### <a name="working-with-an-external-data-source"></a>Arbeiten mit einer externen Datenquelle

Anstatt die integrierte interne Daten, geben Sie die Zeilen für den Menu-Steuerelement zu verwenden, können optional eine externe Datenquelle verwenden und geben Sie Ihren eigenen Sicherungsspeicher für die Elemente (z. B. eine SQLite-Datenbank).

Um mit einer externen Datenquelle arbeiten, erstellen Sie eine Instanz der Datenquelle für das Menüsteuerelement (`NSComboBoxDataSource` z. B.) und mehrere Methoden zum Bereitstellen der erforderlichen Daten überschreiben:

- `ItemCount` -Die Anzahl der Elemente in der Liste zurückgegeben.
- `ObjectValueForItem` -Gibt den Wert des Elements für einen bestimmten Index zurück.
- `IndexOfItem` -Gibt den Index für den Elementwert erteilen.
- `CompletedString` -Gibt den ersten übereinstimmenden Elementwert für den Wert teilweise typisiertes Element zurück. Diese Methode wird nur aufgerufen, wenn die automatische Vervollständigung aktiviert wurde (`Completes = true`).

Finden Sie unter der [Datenbanken und Kombinationsfelder](~/mac/app-fundamentals/databases.md#Databases-and-ComboBoxes) Teil der [arbeiten mit Datenbanken](~/mac/app-fundamentals/databases.md) Dokument Weitere Details.

<a name="Adjusting-the-Lists-Appearance" />

### <a name="adjusting-the-lists-appearance"></a>Anpassen der Darstellung der Liste

Die folgenden Methoden zur Verfügung, das Menüsteuerelement Darstellung anpassen:

- `HasVerticalScroller` -If `true`, das Steuerelement eine vertikale Bildlaufleiste angezeigt wird. 
- `VisibleItems` -Passen Sie die Anzahl der Elemente angezeigt, wenn das Steuerelement geöffnet wird. Der Standardwert ist fünf (5).
- `IntercellSpacing` -Passen Sie die Menge des Speicherplatzes, um ein bestimmtes Element, durch die Bereitstellung einer `NSSize` , in dem die `Width` gibt den linken und rechten Rand und der `Height` gibt den Abstand vor und nach einem Element.
- `ItemHeight` -Gibt die Höhe jedes Elements in der Liste.

Für Dropdown-Typen von `NSPopupButtons`, das erste Menü-Element stellt den Titel für das Steuerelement bereit. Beispiel: 

[ ![](standard-controls-images/menu02.png "Ein Beispiel Menu-Steuerelement")](standard-controls-images/menu02.png)

Um den Titel zu ändern, verfügbar machen dieses Element als ein **Ausgang** und verwenden Sie Code wie folgt:

```csharp
DropDownSelected.Title = "Item 1";
```

<a name="Manipulating-the-Selected-Items" />

### <a name="manipulating-the-selected-items"></a>Bearbeiten von der ausgewählten Elemente.

Die folgenden Methoden und Eigenschaften können Sie die ausgewählten Elemente in das Menüsteuerelement Liste ändern:

- `SelectItem` -Auswählen des Elements am angegebenen Index.
- `Select` -Wählen Sie den Wert des angegebenen Elements.
- `DeselectItem` – Hebt das Element am angegebenen Index.
- `SelectedIndex` -Gibt den Index des derzeit ausgewählten Elements.
- `SelectedValue` -Gibt den Wert des aktuell ausgewählten Elements.

Verwenden der `ScrollItemAtIndexToTop` auf das Element am angegebenen Index am oberen Rand der Liste vorhanden und der `ScrollItemAtIndexToVisible` Liste einen Bildlauf durchführen, bis das Element am angegebenen Index sichtbar ist.

<a name="Responding to Events" />

### <a name="responding-to-events"></a>Reagieren auf Ereignisse

Menüsteuerelemente bieten die folgenden Ereignisse aus, um auf Benutzerinteraktionen reagieren:

- `SelectionChanged` -Wird aufgerufen, wenn der Benutzer einen Wert aus der Liste ausgewählt wurde.
- `SelectionIsChanging` -Wird aufgerufen, bevor das neue Element des ausgewählten Benutzers die aktive Auswahl wird.
- `WillPopup` -Wird aufgerufen, bevor die Dropdown-Liste von Elementen angezeigt wird.
- `WillDismiss` -Wird aufgerufen, bevor die Dropdownliste der Elemente geschlossen wird.

Für `NSComboBox` Steuerelemente, dazu zählen alle die gleichen Ereignisse als die `NSTextField`, wie z. B. die `Changed` Ereignis, das aufgerufen wird, wenn der Benutzer den Wert des Texts im Kombinationsfeld bearbeitet.

Optional können Sie reagieren die eine interne Daten Menüelemente im Benutzeroberflächen-Generator, indem Sie das Element, das Anfügen ausgewählt definiert ein **Aktion** und verwenden Sie Code wie folgt So reagieren Sie auf **Aktion** wird vom Benutzer ausgelöst:

```csharp
partial void ItemOne (Foundation.NSObject sender) {
    DropDownSelected.Title = "Item 1";
    FeedbackLabel.StringValue = "Item One Selected";
}
```

Weitere Informationen zum Arbeiten mit Menüs und Steuerelemente finden Sie unter unsere [Menüs](~/mac/user-interface/menu.md) und [Popup-Schaltfläche und Pulldownliste listet](~/mac/user-interface/menu.md) Dokumentation.

<a name="Working_with_Selection_Controls" />

## <a name="working-with-selection-controls"></a>Arbeiten mit Steuerelementen für die Auswahl

AppKit bietet mehrere Typen von Steuerelementen für die Auswahl, die in Ihrem Entwurf der Benutzeroberfläche verwendet werden kann. Weitere Informationen finden Sie unter der [Auswahlsteuerelemente](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsSelection.html#//apple_ref/doc/uid/20000957-CH49-SW1) Abschnitt der Apple [OS X-Richtlinien für menschliche Benutzeroberfläche](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[ ![](standard-controls-images/select01.png "Beispiel-Auswahlsteuerelemente")](standard-controls-images/select01.png)

Es gibt zwei Möglichkeiten verfolgen, wann ein Auswahlsteuerelement Eingreifen des Benutzers hat, verfügbar machen, als ein **Aktion**. Zum Beispiel:

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

Verwenden Sie zum Festlegen, oder lesen den Wert eines Steuerelements für die Auswahl, die `IntValue` Eigenschaft. Zum Beispiel:

```csharp
FeedbackLabel.StringValue = string.Format("Stepper Value: {0:###}",TickedSlider.IntValue);
```

Die Specialty-Steuerelemente (z. B. Farbe auch und auch Images) haben spezifische Eigenschaften für ihre Werttypen. Beispiel:

```csharp
CollorWell.Color = NSColor.Red;
ImageWell.Image = NSImage.ImageNamed ("tag.png");

```

Die `NSDatePicker` hat die folgenden Eigenschaften für das direkte Arbeiten mit Datums- und Uhrzeitangabe:

- **DateValue** -das aktuelle Datum und Uhrzeit-Wert als eine `NSDate`.
- **Lokale** -der Standort des Benutzers als eine `NSLocal`.
- **TimeInterval** -den Zeitwert als eine `Double`.
- **TimeZone** -Zeitzone als der Benutzer eine `NSTimeZone`.

<a name="Working_with_Indicator_Controls" />

## <a name="working-with-indicator-controls"></a>Arbeiten mit Indikator-Steuerelemente

AppKit enthält unterschiedliche Typen von Steuerungsmechanismen Indikator, der in Ihrem Entwurf der Benutzeroberfläche verwendet werden kann. Weitere Informationen finden Sie unter der [Indikator Steuerelemente](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsIndicators.html#//apple_ref/doc/uid/20000957-CH50-SW1) Abschnitt der Apple [OS X-Richtlinien für menschliche Benutzeroberfläche](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[ ![](standard-controls-images/level01.png "Beispiel-Indikator-Steuerelemente")](standard-controls-images/level01.png)

Es gibt zwei Möglichkeiten, nachverfolgen, wenn ein Steuerelement Indikator Eingreifen des Benutzers, entweder als eine Offenlegung wurde ein **Aktion** oder eine **Steckdose** und Anfügen einer **Delegaten** auf die `Activated`Ereignis. Zum Beispiel:

```csharp
LevelIndicator.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Level: {0:###}",LevelIndicator.DoubleValue);
};
```

Verwenden Sie zum Lesen, oder legen Sie den Wert des Steuerelements Indikator, der `DoubleValue` Eigenschaft. Zum Beispiel:

```csharp
FeedbackLabel.StringValue = string.Format("Rating: {0:###}",Rating.DoubleValue);
```

Bei der Anzeige, sollte die unbestimmt und asynchrone Statusanzeigen animiert werden. Verwenden der `StartAnimation` Methode zum Starten der Animation, wenn sie angezeigt werden. Zum Beispiel:

```csharp
Indeterminate.StartAnimation (this);
AsyncProgress.StartAnimation (this);
```

Aufrufen der `StopAnimation` wird die Methode die Animation beendet.

<a name="Working_with_Text_Controls" />

## <a name="working-with-text-controls"></a>Arbeiten mit Text-Steuerelemente

AppKit bietet mehrere Typen von Steuerelementen, die in Ihrem Entwurf der Benutzeroberfläche verwendet werden kann. Weitere Informationen finden Sie unter der [Textsteuerelemente](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsText.html#//apple_ref/doc/uid/20000957-CH51-SW1) Abschnitt der Apple [OS X-Richtlinien für menschliche Benutzeroberfläche](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[ ![](standard-controls-images/text01.png "Beispiel-Text-Steuerelemente")](standard-controls-images/text01.png)

Für Textfelder (`NSTextField`), können die folgenden Ereignisse zum Nachverfolgen von Eingreifen des Benutzers verwendet werden:

- **Geändert** -jederzeit Benutzer den Wert des Felds ändert ausgelöst wird. Z. B. auf alle eingegebenen Zeichen.
- **EditingBegan** -wird ausgelöst, wenn der Benutzer das Feld für die Bearbeitung auswählt.
- **EditingEnded** – Wenn der Benutzer die EINGABETASTE, in das Feld drückt oder das Feld verlässt.

Verwenden der `StringValue` Eigenschaft lesen oder Festlegen der Wert des Felds. Zum Beispiel:

```csharp
FeedbackLabel.StringValue = string.Format("User ID: {0}",UserField.StringValue);
```

Für Felder, die angezeigt oder numerische Werte zu bearbeiten, können Sie die `IntValue` Eigenschaft. Zum Beispiel:

```csharp
FeedbackLabel.StringValue = string.Format("Number: {0}",NumberField.IntValue);
```

Ein `NSTextView` bietet einen vollständigen Funktionsumfang Text bearbeiten und Anzeigen des Bereichs mit integrierten Formatierung. Wie eine `NSTextField`, verwenden Sie die `StringValue` Eigenschaft, um den Bereich Wert lesen oder festlegen.

Ein Beispiel eines komplexen Beispiels mit Textansichten in einer app Xamarin.Mac zu arbeiten, finden Sie unter der [SourceWriter Beispiel-App](https://developer.xamarin.com/samples/mac/SourceWriter/). SourceWriter ist ein einfacher Quellcode-Editor, der Unterstützung für die Codevervollständigung und die einfache Syntaxhervorhebung bietet.

Der SourceWriter-Code wurde vollständig kommentiert und es wurden, wenn möglich, Links der Schlüsseltechnologien und -methoden zu wichtigen Informationen in den Leitfäden der Xamarin.Mac-Dokumentation bereitgestellt.

<a name="Working_with_Content_Views" />

## <a name="working-with-content-views"></a>Arbeiten mit Content-Ansichten

AppKit bietet mehrere Typen von Inhalt Sichten, die in Ihrem Entwurf der Benutzeroberfläche verwendet werden kann. Weitere Informationen finden Sie unter der [Content Ansichten](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1) Abschnitt der Apple [OS X-Richtlinien für menschliche Benutzeroberfläche](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

[ ![](standard-controls-images/content01.png "Ein Beispiel-Inhaltsansicht")](standard-controls-images/content01.png)

<a name="Popovers" />

### <a name="popovers"></a>Popovers

Eine Popover ist ein vorübergehender Element der Benutzeroberfläche, die Funktionalität bereitstellt, die direkt mit einem bestimmten ein Steuerelement oder einen Bereich auf dem Bildschirm verknüpft ist. Eine Popover gleitet über dem Fenster mit dem Steuerelement oder Bereich an, die mit der Sie verknüpft ist, und eine Rahmenlinie umfasst einen Pfeil, um den Punkt angeben, von dem er entwickelt.

Um eine Popover zu erstellen, führen Sie folgende Schritte aus:

1. Öffnen der `.storyboard` Datei des Fensters, das eine Popover, durch Doppelklick im hinzugefügt werden soll die **Projektmappen-Explorer**
2. Ziehen Sie eine **Domänencontroller anzeigen** aus der **Bibliothek Inspektor** auf die **Schnittstelle Editor**: 

    [ ![](standard-controls-images/content02.png "Auswählen eines Controllers Ansicht aus der Bibliothek")](standard-controls-images/content02.png)
4. Definieren Sie die Größe und das Layout der **benutzerdefinierte Sicht**: 

    [ ![](standard-controls-images/content04.png "Bearbeiten des Layouts")](standard-controls-images/content04.png)
5. Steuerelement klicken und ziehen Sie aus der Quelle des Popups auf die **Modellansichtcontroller**: 

    [ ![](standard-controls-images/content05.png "Ziehen zum Erstellen einer segue")](standard-controls-images/content05.png)
6. Wählen Sie **Popover** im Popupmenü: 

    [ ![](standard-controls-images/content06.png "Festlegen der Segue-Typs")](standard-controls-images/content06.png)
7. Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.

<a name="Tab_Views" />

### <a name="tab-views"></a>Registerkarte "-Ansichten

Registerkartenansichten besteht aus einer Liste auf der Registerkarte (welche sieht ähnlich wie eine segmentierte-Steuerelement) zusammen mit einem Satz von Ansichten, die aufgerufen werden _Bereiche_. Wenn der Benutzer eine neue Registerkarte auswählt, wird der Bereich, die es angefügt ist, angezeigt. Jeder Bereich enthält einen eigenen Satz von Steuerelementen.

Verwenden Sie bei der Arbeit mit einer Registerkartenansicht in Xcodes Benutzeroberflächen-Generator die **Attribut Inspektor** , legen Sie die Anzahl der Registerkarten:

[ ![](standard-controls-images/content08.png "Die Anzahl der Registerkarten bearbeiten")](standard-controls-images/content08.png)

Wählen Sie für jede Registerkarte in der **Schnittstellenhierarchie** Festlegen seiner **Titel** und Hinzufügen von UI-Elemente, um seine **Bereich**:

[ ![](standard-controls-images/content09.png "Bearbeiten die Registerkarten in Xcode")](standard-controls-images/content09.png)

<a name="Data_Binding_AppKit_Controls" />

## <a name="data-binding-appkit-controls"></a>AppKit Daten binden von Steuerelementen

Mithilfe von Techniken von Schlüssel-Wert zu codieren, und Binden von Daten in Ihrer Anwendung Xamarin.Mac, können Sie den Umfang des Codes, die Sie schreiben und verwalten, die zum Auffüllen von und Arbeiten mit UI-Elemente, erheblich verringern. Sie haben außerdem den Vorteil, dass weitere Entkopplung Ihre Daten sichern (_Datenmodell_) von der Vorderseite enden Benutzeroberfläche (_Model-View-Controller_), was zu einfacher zu verwalten, eine flexiblere Anwendung Entwurf.

Schlüssel-Wert-Codierung (KVM) ist ein Mechanismus für den Zugriff auf Eigenschaften eines Objekts ist indirekt, mithilfe von Schlüsseln (speziell formatierte Zeichenfolgen) zum Identifizieren von Eigenschaften, anstatt über Nachrichteninstanzvariablen auf Sie zuzugreifen oder Methoden des Eigenschaftenaccessors (`get/set`). Durch die Implementierung von Schlüssel-Wert-Codierung kompatibel Accessoren in Ihrer Anwendung Xamarin.Mac können haben Sie Zugriff auf andere MacOS-Funktionen, z. B. beobachten von Schlüssel-Wert (KVO), zum Binden von Daten, Core Daten, Kakao Bindungen und Scriptability.

Weitere Informationen finden Sie unter der [einfache Datenbindung](~/mac/app-fundamentals/databinding.md#Simple_Data_Binding) Teil unserer [zum Binden von Daten und Schlüssel-Wert-Codierung](~/mac/app-fundamentals/databinding.md) Dokumentation.


<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat eine ausführliche Übersicht über das Arbeiten mit den standardmäßigen AppKit Steuerelementen wie Schaltflächen, Bezeichnungen, Textfelder, Kontrollkästchen und segmentierte Steuerelemente in einer Anwendung Xamarin.Mac übernommen. Es behandelt ein User Interface Design in Xcodes Benutzeroberflächen-Generator hinzugefügt, diese auf Code per Steckdosen und Aktionen verfügbar zu machen und Arbeiten mit AppKit Steuerelementen im C#-Code.

## <a name="related-links"></a>Verwandte Links

- [MacControls (Beispiel)](https://developer.xamarin.com/samples/mac/MacControls/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Windows](~/mac/user-interface/window.md)
- [Datenbindung und Schlüssel/Wert-Codierung](~/mac/app-fundamentals/databinding.md)
- [OS X Human Richtlinien zur Benutzeroberfläche](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Über Steuerelemente und Ansichten](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1)
