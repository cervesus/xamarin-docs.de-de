---
title: Auflistungsansichten
description: "Dieser Artikel beschreibt das Arbeiten mit Auflistungsansichten in einer app Xamarin.Mac. Es umfasst erstellen und Verwalten von Auflistungsansichten in Xcode und Benutzeroberflächen-Generator und programmgesteuert mit ihnen arbeiten."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6EE32256-5948-4AE4-8133-6D0B3F4173E8
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/24/2017
ms.openlocfilehash: 9aa66a531b723f176b940ba35ee4e86eae711f7d
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="collection-views"></a>Auflistungsansichten

_Dieser Artikel beschreibt das Arbeiten mit Auflistungsansichten in einer app Xamarin.Mac. Es umfasst erstellen und Verwalten von Auflistungsansichten in Xcode und Benutzeroberflächen-Generator und programmgesteuert mit ihnen arbeiten._

Beim Arbeiten mit c# und .NET in einer Xamarin.Mac app dem Entwickler Zugriff auf das gleiche hat AppKit Auflistungsansicht steuert, die ein Entwickler arbeiten in unter *Objective-C* und *Xcode* verfügt. Da Xamarin.Mac direkt mit Xcode integriert ist, verwendet der Entwickler des Xcode _Schnittstelle-Generator_ erstellen und Verwalten von Auflistungsansichten.

Ein `NSCollectionView` zeigt ein Raster Unteransichten organisiert mithilfe einer `NSCollectionViewLayout`. Jede Unteransicht im Raster wird dargestellt, durch eine `NSCollectionViewItem` verwaltet das Laden des Inhalts der Ansicht eine `.xib` Datei.

[![Eine Beispiel-app ausführen](collection-view-images/intro01.png)](collection-view-images/intro01.png#lightbox)

Dieser Artikel behandelt die Grundlagen der Arbeit mit Auflistungsansichten in einer app Xamarin.Mac. Wird mit hoher vorgeschlagen, dass Sie über arbeiten die [Hello, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst, insbesondere die [Einführung in Xcode und Benutzeroberflächen-Generator](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) und [Steckdosen und Aktionen](~/mac/get-started/hello-mac.md#Outlets_and_Actions) Abschnitte, wie sie behandelt wichtige Konzepte und Techniken, die in diesem Artikel verwendet werden.

Sie möchten einen Blick auf die [Verfügbarmachen von C#-Klassen / Methoden für Objective-C-](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentieren, es wird erläutert, die `Register` und `Export` Befehle verwendet, um über das Netzwerk Ihrer C#-Klassen für Objective-C-Objekte und Elemente der Benutzeroberfläche von Volltextkatalogen.

<a name="About_Collection_Views"/>

## <a name="about-collection-views"></a>Informationen zu Auflistungsansichten

Hauptzweck des einer Auflistungsansicht (`NSCollectionView`) ist eine Gruppe von Objekten in einer strukturierten Weise mit einer Auflistung Ansichtslayout visuell anordnen (`NSCollectionViewLayout`), mit der jedes einzelne Objekt (`NSCollectionViewItem`) eine eigene Ansicht in der größeren Auflistung abrufen. Auflistungsansichten arbeiten, über Techniken zum Binden von Daten und Schlüssel-Wert zu codieren, und Lesen Sie deshalb die [zum Binden von Daten und Schlüssel-Wert-Codierung](~/mac/app-fundamentals/databinding.md) Dokumentation vor dem Fortfahren mit der in diesem Artikel.

Die Auflistungsansicht hat keine standardmäßigen, vordefinierten Sammlung Element anzeigen (z. B. eine Gliederung oder die Tabellenansicht wird), also der Entwickler verantwortlich für das Entwerfen und Implementieren einer _Prototyp Ansicht_ andere AppKit Steuerelemente wie z. B. Bildfelder verwenden , Textfelder, Bezeichnungen usw. In dieser Ansicht Prototyp wird verwendet, um das Anzeigen von und Arbeiten mit der jedes Element über die Auflistungsansicht und befindet sich in einem `.xib` Datei.

Da der Entwickler für das Aussehen und Verhalten eines Sammelelements Ansicht zuständig ist, hat die Auflistungsansicht keine integrierte Unterstützung für ein ausgewähltes Element im Raster hervorheben. Diese Funktion wird in diesem Artikel erläutert.

<a name="Defining_your_Data_Model"/>

## <a name="defining-the-data-model"></a>Definieren des Datenmodells

Vor dem Datenbindung einer Auflistungsansicht im Benutzeroberflächen-Generator, ein Schlüssel-Wert-Codierung (KVM) / beobachten von Schlüssel-Wert (KVO) kompatible Klasse muss definiert werden, in der app Xamarin.Mac fungiert als die _Datenmodell_ für die Bindung. Das Datenmodell enthält alle Daten, die in der Auflistung angezeigt und alle Änderungen an den Daten, mit der der Benutzer in der Benutzeroberfläche während der Ausführung der Anwendung, empfängt.

Nehmen Sie als Beispiel eine App, die eine Gruppe von Mitarbeitern verwaltet, kann die folgende Klasse verwendet werden, um das Datenmodell zu definieren:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        #region Private Variables
        private string _name = "";
        private string _occupation = "";
        private bool _isManager = false;
        private NSMutableArray _people = new NSMutableArray();
        #endregion

        #region Computed Properties
        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }

        [Export("Occupation")]
        public string Occupation {
            get { return _occupation; }
            set {
                WillChangeValue ("Occupation");
                _occupation = value;
                DidChangeValue ("Occupation");
            }
        }

        [Export("isManager")]
        public bool isManager {
            get { return _isManager; }
            set {
                WillChangeValue ("isManager");
                WillChangeValue ("Icon");
                _isManager = value;
                DidChangeValue ("isManager");
                DidChangeValue ("Icon");
            }
        }

        [Export("isEmployee")]
        public bool isEmployee {
            get { return (NumberOfEmployees == 0); }
        }

        [Export("Icon")]
        public NSImage Icon
        {
            get
            {
                if (isManager)
                {
                    return NSImage.ImageNamed("IconGroup");
                }
                else
                {
                    return NSImage.ImageNamed("IconUser");
                }
            }
        }

        [Export("personModelArray")]
        public NSArray People {
            get { return _people; }
        }

        [Export("NumberOfEmployees")]
        public nint NumberOfEmployees {
            get { return (nint)_people.Count; }
        }
        #endregion

        #region Constructors
        public PersonModel ()
        {
        }

        public PersonModel (string name, string occupation)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
        }

        public PersonModel (string name, string occupation, bool manager)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
            this.isManager = manager;
        }
        #endregion

        #region Array Controller Methods
        [Export("addObject:")]
        public void AddPerson(PersonModel person) {
            WillChangeValue ("personModelArray");
            isManager = true;
            _people.Add (person);
            DidChangeValue ("personModelArray");
        }

        [Export("insertObject:inPersonModelArrayAtIndex:")]
        public void InsertPerson(PersonModel person, nint index) {
            WillChangeValue ("personModelArray");
            _people.Insert (person, index);
            DidChangeValue ("personModelArray");
        }

        [Export("removeObjectFromPersonModelArrayAtIndex:")]
        public void RemovePerson(nint index) {
            WillChangeValue ("personModelArray");
            _people.RemoveObject (index);
            DidChangeValue ("personModelArray");
        }

        [Export("setPersonModelArray:")]
        public void SetPeople(NSMutableArray array) {
            WillChangeValue ("personModelArray");
            _people = array;
            DidChangeValue ("personModelArray");
        }
        #endregion
    }
}
```

Die `PersonModel` Datenmodell wird im weiteren Verlauf dieses Artikels verwendet werden.

<a name="Working_with_a_Collection_View"/>

## <a name="working-with-a-collection-view"></a>Arbeiten mit einer Auflistungsansicht

Datenbindung mit einer Auflistungsansicht ist sehr viel like Bindung mit einer Tabellenansicht als `NSCollectionViewDataSource` wird verwendet, um Daten für die Sammlung bereitstellen. Da die Auflistungsansicht einen voreingestellten Anzeigeformat besitzt, sind mehr Schritte erforderlich, zum Bereitstellen von Feedback der Benutzer-Interaktion und zum Nachverfolgen der Auswahl des Benutzers.

<a name="Creating-the-Cell-Prototype"/>

### <a name="creating-the-cell-prototype"></a>Erstellen den Prototyp für die Zelle

Da die Auflistungsansicht nicht standardmäßige Zelle Prototyp enthält, muss der Entwickler eine oder mehrere hinzuzufügende `.xib` Dateien zu den Xamarin.Mac-app für das Layout und Inhalt der einzelnen Zellen definieren.

Führen Sie folgende Schritte aus:

1. In der **Projektmappen-Explorer**mit der rechten Maustaste auf den Projektnamen, und wählen Sie **hinzufügen** > **neue Datei...**
2. Wählen Sie **Mac** > **Modellansichtcontroller**, geben sie einen Namen (z. B. `EmployeeItem` in diesem Beispiel), und klicken Sie auf die **neu** Schaltfläche zum Erstellen: 

    ![Hinzufügen eines neuen Ansicht-Controllers](collection-view-images/proto01.png)

    Dadurch wird hinzugefügt ein `EmployeeItem.cs`, `EmployeeItemController.cs` und `EmployeeItemController.xib` Datei in der Projektmappe.
3. Doppelklicken Sie auf die `EmployeeItemController.xib` Datei, um ihn zur Bearbeitung in Xcodes Benutzeroberflächen-Generator zu öffnen.
4. Hinzufügen einer `NSBox`, `NSImageView` und zwei `NSLabel` Steuerelemente zur Ansicht und ordnen sie Sie wie folgt:

    ![Entwerfen des Layouts des Prototyps Zelle](collection-view-images/proto02.png)
5. Öffnen der **Assistant Editor** , und erstellen Sie ein **Steckdose** für die `NSBox` , damit es verwendet werden kann, um den Auswahlstatus einer Zelle anzugeben:

    ![Verfügbarmachen der NSBox in eine Steckdose](collection-view-images/proto03.png)
6. Zurück zu den **Standard-Editor** , und wählen Sie die Bild-Sicht.
7. In der **Inspektor binden**wählen **binden,** > **des Dateibesitzer** , und geben Sie eine **Schlüsselpfad Modell** von `self.Person.Icon`:

    ![Binden das Symbol "](collection-view-images/proto04.png)
8. Wählen Sie die erste Bezeichnung und klicken Sie in der **Inspektor binden**Option **binden,** > **des Dateibesitzer** , und geben Sie eine **Modell Schlüsselpfad**von `self.Person.Name`:

    ![Binden der](collection-view-images/proto05.png)
9. Wählen Sie das zweite Bezeichnungsfeld und klicken Sie in der **Inspektor binden**Option **binden,** > **der Dateibesitzer** , und geben Sie eine **Modell Schlüsselpfad**von `self.Person.Occupation`:

    ![Binden der Beruf](collection-view-images/proto06.png)
10. Speichern Sie die Änderungen an der `.xib` Datei und zurück zu Visual Studio, um die Änderungen zu synchronisieren.

Bearbeiten der `EmployeeItemController.cs` Datei, und stellen sie wie folgt aussehen:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Foundation;
using AppKit;

namespace MacCollectionNew
{
    /// <summary>
    /// The Employee item controller handles the display of the individual items that will
    /// be displayed in the collection view as defined in the associated .XIB file.
    /// </summary>
    public partial class EmployeeItemController : NSCollectionViewItem
    {
        #region Private Variables
        /// <summary>
        /// The person that will be displayed.
        /// </summary>
        private PersonModel _person;
        #endregion

        #region Computed Properties
        // strongly typed view accessor
        public new EmployeeItem View
        {
            get
            {
                return (EmployeeItem)base.View;
            }
        }

        /// <summary>
        /// Gets or sets the person.
        /// </summary>
        /// <value>The person that this item belongs to.</value>
        [Export("Person")]
        public PersonModel Person
        {
            get { return _person; }
            set
            {
                WillChangeValue("Person");
                _person = value;
                DidChangeValue("Person");
            }
        }

        /// <summary>
        /// Gets or sets the color of the background for the item.
        /// </summary>
        /// <value>The color of the background.</value>
        public NSColor BackgroundColor {
            get { return Background.FillColor; }
            set { Background.FillColor = value; }
        }

        /// <summary>
        /// Gets or sets a value indicating whether this <see cref="T:MacCollectionNew.EmployeeItemController"/> is selected.
        /// </summary>
        /// <value><c>true</c> if selected; otherwise, <c>false</c>.</value>
        /// <remarks>This also changes the background color based on the selected state
        /// of the item.</remarks>
        public override bool Selected
        {
            get
            {
                return base.Selected;
            }
            set
            {
                base.Selected = value;

                // Set background color based on the selection state
                if (value) {
                    BackgroundColor = NSColor.DarkGray;
                } else {
                    BackgroundColor = NSColor.LightGray;
                }
            }
        }
        #endregion

        #region Constructors
        // Called when created from unmanaged code
        public EmployeeItemController(IntPtr handle) : base(handle)
        {
            Initialize();
        }

        // Called when created directly from a XIB file
        [Export("initWithCoder:")]
        public EmployeeItemController(NSCoder coder) : base(coder)
        {
            Initialize();
        }

        // Call to load from the XIB/NIB file
        public EmployeeItemController() : base("EmployeeItem", NSBundle.MainBundle)
        {
            Initialize();
        }

        // Added to support loading from XIB/NIB
        public EmployeeItemController(string nibName, NSBundle nibBundle) : base(nibName, nibBundle) {

            Initialize();
        }

        // Shared initialization code
        void Initialize()
        {
        }
        #endregion
    }
}
```

Betrachten diesen Code im Detail, die Klasse erbt von `NSCollectionViewItem` damit es als Prototyp für eine Zelle Auflistungsansicht fungieren kann. Die `Person` -Eigenschaft verfügbar macht, die Klasse, die zur Datenbindung an die Image-Ansicht und Bezeichnungen in Xcode verwendet wurde. Dies ist eine Instanz von der `PersonModel` oben erstellt.

Die `BackgroundColor` Eigenschaft ist eine Verknüpfung zu den `NSBox` des Steuerelements `FillColor` , wird verwendet, um den Auswahlstatus einer Zelle angezeigt. Durch Überschreiben der `Selected` Eigenschaft von der `NSCollectionViewItem`, der folgende Code legt fest oder löscht diese Auswahlzustand:

```csharp
public override bool Selected
{
    get
    {
        return base.Selected;
    }
    set
    {
        base.Selected = value;

        // Set background color based on the selection state
        if (value) {
            BackgroundColor = NSColor.DarkGray;
        } else {
            BackgroundColor = NSColor.LightGray;
        }
    }
}
```

<a name="Creating-the-Collection-View-Data-Source"/>

### <a name="creating-the-collection-view-data-source"></a>Erstellen der Datenquelle für Sammlung anzeigen

Eine Datenquelle für Sammlung anzeigen (`NSCollectionViewDataSource`) enthält alle Daten für eine Auflistungsansicht erstellt und füllt eine Auflistung Ansicht Zelle (mithilfe der `.xib` Prototyp) wie für jedes Element in der Auflistung erforderlich.

Fügen Sie eine neue Klasse der das Projekt aufrufen `CollectionViewDataSource` , und stellen sie wie folgt aussehen:

```csharp
using System;
using System.Collections.Generic;
using AppKit;
using Foundation;

namespace MacCollectionNew
{
    /// <summary>
    /// Collection view data source provides the data for the collection view.
    /// </summary>
    public class CollectionViewDataSource : NSCollectionViewDataSource
    {
        #region Computed Properties
        /// <summary>
        /// Gets or sets the parent collection view.
        /// </summary>
        /// <value>The parent collection view.</value>
        public NSCollectionView ParentCollectionView { get; set; }

        /// <summary>
        /// Gets or sets the data that will be displayed in the collection.
        /// </summary>
        /// <value>A collection of PersonModel objects.</value>
        public List<PersonModel> Data { get; set; } = new List<PersonModel>();
        #endregion

        #region Constructors
        /// <summary>
        /// Initializes a new instance of the <see cref="T:MacCollectionNew.CollectionViewDataSource"/> class.
        /// </summary>
        /// <param name="parent">The parent collection that this datasource will provide data for.</param>
        public CollectionViewDataSource(NSCollectionView parent)
        {
            // Initialize
            ParentCollectionView = parent;

            // Attach to collection view
            parent.DataSource = this;

        }
        #endregion

        #region Override Methods
        /// <summary>
        /// Gets the number of sections.
        /// </summary>
        /// <returns>The number of sections.</returns>
        /// <param name="collectionView">The parent Collection view.</param>
        public override nint GetNumberOfSections(NSCollectionView collectionView)
        {
            // There is only one section in this view
            return 1;
        }

        /// <summary>
        /// Gets the number of items in the given section.
        /// </summary>
        /// <returns>The number of items.</returns>
        /// <param name="collectionView">The parent Collection view.</param>
        /// <param name="section">The Section number to count items for.</param>
        public override nint GetNumberofItems(NSCollectionView collectionView, nint section)
        {
            // Return the number of items
            return Data.Count;
        }

        /// <summary>
        /// Gets the item for the give section and item index.
        /// </summary>
        /// <returns>The item.</returns>
        /// <param name="collectionView">The parent Collection view.</param>
        /// <param name="indexPath">Index path specifying the section and index.</param>
        public override NSCollectionViewItem GetItem(NSCollectionView collectionView, NSIndexPath indexPath)
        {
            var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
            item.Person = Data[(int)indexPath.Item];

            return item;
        }
        #endregion
    }
}
```

Betrachten diesen Code im Detail, die Klasse erbt von `NSCollectionViewDataSource` und stellt eine Liste der `PersonModel` Instanzen über seine `Data` Eigenschaft.

Da diese Auflistung nur ein Abschnitt aufweist, wird der Code überschreibt die `GetNumberOfSections` -Methode und gibt immer `1`. Darüber hinaus die `GetNumberofItems` -Methode überschrieben wird, am gibt es der Anzahl der Elemente in der `Data` Eigenschaftenliste.

Die `GetItem` Methode wird aufgerufen, wenn eine neue Zelle erforderlich ist und wie folgt sieht:

```csharp
public override NSCollectionViewItem GetItem(NSCollectionView collectionView, NSIndexPath indexPath)
{
    var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
    item.Person = Data[(int)indexPath.Item];

    return item;
}
```

Die `MakeItem` die Auflistungsansicht Methode wird aufgerufen, um erstellen oder eine wieder verwendbare Instanz zurückgeben der `EmployeeItemController` und dessen `Person` Eigenschaft auf festgelegt ist Element in der angeforderten Zelle angezeigt wird. 

Die `EmployeeItemController` muss mit der zuvor mithilfe des folgenden Codes Auflistung-View-Controller registriert werden:

```csharp
EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");
``` 

Die **Bezeichner** (`EmployeeCell`) verwendet, der `MakeItem` Aufrufen _muss_ entsprechen den Namen des View-Controller, die in der Auflistungsansicht registriert wurde. Dieser Schritt wird im folgenden ausführlich erläutert.

<a name="Handling-Item-Selection"/>

### <a name="handling-item-selection"></a>Behandlung Elementauswahl

Behandeln Sie die Auswahl und die Aufhebung der Auswahl von Elementen in der Auflistung eine `NSCollectionViewDelegate` erforderlich. Da in diesem Beispiel wird die integrierte in nutzt `NSCollectionViewFlowLayout` Layouttyp, eine `NSCollectionViewDelegateFlowLayout` werden bestimmte Version dieses Delegaten.

Fügen Sie dem Projekt eine neue Klasse aufrufen, es `CollectionViewDelegate` , und stellen sie wie folgt aussehen:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacCollectionNew
{
    /// <summary>
    /// Collection view delegate handles user interaction with the elements of the 
    /// collection view for the Flow-Based layout type.
    /// </summary>
    public class CollectionViewDelegate : NSCollectionViewDelegateFlowLayout
    {
        #region Computed Properties
        /// <summary>
        /// Gets or sets the parent view controller.
        /// </summary>
        /// <value>The parent view controller.</value>
        public ViewController ParentViewController { get; set; }
        #endregion

        #region Constructors
        /// <summary>
        /// Initializes a new instance of the <see cref="T:MacCollectionNew.CollectionViewDelegate"/> class.
        /// </summary>
        /// <param name="parentViewController">Parent view controller.</param>
        public CollectionViewDelegate(ViewController parentViewController)
        {
            // Initialize
            ParentViewController = parentViewController;
        }
        #endregion

        #region Override Methods
        /// <summary>
        /// Handles one or more items being selected.
        /// </summary>
        /// <param name="collectionView">The parent Collection view.</param>
        /// <param name="indexPaths">The Index paths of the items being selected.</param>
        public override void ItemsSelected(NSCollectionView collectionView, NSSet indexPaths)
        {
            // Dereference path
            var paths = indexPaths.ToArray<NSIndexPath>();
            var index = (int)paths[0].Item;

            // Save the selected item
            ParentViewController.PersonSelected = ParentViewController.Datasource.Data[index];

        }

        /// <summary>
        /// Handles one or more items being deselected.
        /// </summary>
        /// <param name="collectionView">The parent Collection view.</param>
        /// <param name="indexPaths">The Index paths of the items being deselected.</param>
        public override void ItemsDeselected(NSCollectionView collectionView, NSSet indexPaths)
        {
            // Dereference path
            var paths = indexPaths.ToArray<NSIndexPath>();
            var index = paths[0].Item;

            // Clear selection
            ParentViewController.PersonSelected = null;
        }
        #endregion
    }
}
``` 

Die `ItemsSelected` und `ItemsDeselected` Methoden außer Kraft gesetzt werden, und zum Aktivieren bzw. Deaktivieren der `PersonSelected` Eigenschaft des View-Controller, die die Auflistungsansicht behandelt wird, wenn der Benutzer wählt aus, oder hebt die Auswahl eines Elements. Dies wird nachfolgend detailliert angezeigt werden.

<a name="Creating-the-Collection-View-in-Interface-Builder"/>

### <a name="creating-the-collection-view-in-interface-builder"></a>Erstellen die Auflistungsansicht in Benutzeroberflächen-Generator

Klicken Sie mit allen erforderlichen unterstützenden Bestandteile vorhanden die Haupt-Storyboard bearbeitet werden kann, und einer Auflistungsansicht hinzugefügt.

Führen Sie folgende Schritte aus:

1. Doppelklicken Sie auf die `Main.Storyboard` in der Datei die **Projektmappen-Explorer** zur Bearbeitung in Xcode Öffnen des Benutzeroberflächen-Generator.
2. Ziehen Sie eine Auflistungsansicht in die Hauptansicht und ihre Größe um die Sicht ausfüllen:

    ![Das Layout hinzugefügt einer Auflistungsansicht](collection-view-images/collection01.png)
3. Mit der Auflistungsansicht ausgewählt wird mithilfe des Editors für die Einschränkung zur Ansicht anheften, wenn er geändert wird:

    ![Hinzufügen von Einschränkungen](collection-view-images/collection02.png)
4. Stellen Sie sicher, dass in der Auflistungsansicht ausgewählt ist die **Entwurfsoberfläche** (und nicht die **umrandet, Scroll Ansicht** oder **Clip-Ansicht** , die Sie enthält), wechseln Sie zu der  **Assistent Editor** , und erstellen Sie eine **Nachrichtenplattform** für die Auflistungsansicht:

    ![Hinzufügen von Einschränkungen](collection-view-images/collection03.png)
5. Die Änderungen zu speichern und zurück zu Visual Studio synchronisiert werden.

<a name="Bringing-it-all-Together"/>

## <a name="bringing-it-all-together"></a>Schalten sie alle zusammen

Alle unterstützenden Teile haben jetzt wurde cookiepoolauslastung mit einer Klasse, das als das Datenmodell fungiert (`PersonModel`), eine `NSCollectionViewDataSource` wurde hinzugefügt, um die Daten angeben einer `NSCollectionViewDelegateFlowLayout` erstellt wurde, um die Auswahl von Elementen zu behandeln und eine `NSCollectionView` wurde hinzugefügt, um die Haupt-Storyboard und verfügbar gemacht werden, als eine Steckdose (`EmployeeCollection`).

Der letzte Schritt ist das Bearbeiten von View-Controller, die die Auflistungsansicht enthält, und schalten Sie alle Teile zusammen, um die Auflistung aufzufüllen, und behandeln Elementauswahl.

Bearbeiten der `ViewController.cs` Datei, und stellen sie wie folgt aussehen:

```csharp
using System;
using AppKit;
using Foundation;
using CoreGraphics;

namespace MacCollectionNew
{
    /// <summary>
    /// The View controller controls the main view that houses the Collection View.
    /// </summary>
    public partial class ViewController : NSViewController
    {
        #region Private Variables
        private PersonModel _personSelected;
        private bool shouldEdit = true;
        #endregion

        #region Computed Properties
        /// <summary>
        /// Gets or sets the datasource that provides the data to display in the 
        /// Collection View.
        /// </summary>
        /// <value>The datasource.</value>
        public CollectionViewDataSource Datasource { get; set; }

        /// <summary>
        /// Gets or sets the person currently selected in the collection view.
        /// </summary>
        /// <value>The person selected or <c>null</c> if no person is selected.</value>
        [Export("PersonSelected")]
        public PersonModel PersonSelected
        {
            get { return _personSelected; }
            set
            {
                WillChangeValue("PersonSelected");
                _personSelected = value;
                DidChangeValue("PersonSelected");
                RaiseSelectionChanged();
            }
        }
        #endregion

        #region Constructors
        /// <summary>
        /// Initializes a new instance of the <see cref="T:MacCollectionNew.ViewController"/> class.
        /// </summary>
        /// <param name="handle">Handle.</param>
        public ViewController(IntPtr handle) : base(handle)
        {
        }
        #endregion

        #region Override Methods
        /// <summary>
        /// Called after the view has finished loading from the Storyboard to allow it to
        /// be configured before displaying to the user.
        /// </summary>
        public override void ViewDidLoad()
        {
            base.ViewDidLoad();

            // Initialize Collection View
            ConfigureCollectionView();
            PopulateWithData();
        }
        #endregion

        #region Private Methods
        /// <summary>
        /// Configures the collection view.
        /// </summary>
        private void ConfigureCollectionView()
        {
            EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");

            // Create a flow layout
            var flowLayout = new NSCollectionViewFlowLayout()
            {
                ItemSize = new CGSize(150, 150),
                SectionInset = new NSEdgeInsets(10, 10, 10, 20),
                MinimumInteritemSpacing = 10,
                MinimumLineSpacing = 10
            };
            EmployeeCollection.WantsLayer = true;

            // Setup collection view
            EmployeeCollection.CollectionViewLayout = flowLayout;
            EmployeeCollection.Delegate = new CollectionViewDelegate(this);

        }

        /// <summary>
        /// Populates the Datasource with data and attaches it to the collection view.
        /// </summary>
        private void PopulateWithData()
        {
            // Make datasource
            Datasource = new CollectionViewDataSource(EmployeeCollection);

            // Build list of employees
            Datasource.Data.Add(new PersonModel("Craig Dunn", "Documentation Manager", true));
            Datasource.Data.Add(new PersonModel("Amy Burns", "Technical Writer"));
            Datasource.Data.Add(new PersonModel("Joel Martinez", "Web & Infrastructure"));
            Datasource.Data.Add(new PersonModel("Kevin Mullins", "Technical Writer"));
            Datasource.Data.Add(new PersonModel("Mark McLemore", "Technical Writer"));
            Datasource.Data.Add(new PersonModel("Tom Opgenorth", "Technical Writer"));
            Datasource.Data.Add(new PersonModel("Larry O'Brien", "API Docs Manager", true));
            Datasource.Data.Add(new PersonModel("Mike Norman", "API Documentor"));

            // Populate collection view
            EmployeeCollection.ReloadData();
        }
        #endregion

        #region Events
        /// <summary>
        /// Selection changed delegate.
        /// </summary>
        public delegate void SelectionChangedDelegate();

        /// <summary>
        /// Occurs when selection changed.
        /// </summary>
        public event SelectionChangedDelegate SelectionChanged;

        /// <summary>
        /// Raises the selection changed event.
        /// </summary>
        internal void RaiseSelectionChanged() {
            // Inform caller
            if (this.SelectionChanged != null) SelectionChanged();
        }
        #endregion
    }
}
```

Einen Blick auf diesen Code im Detail, dauert eine `Datasource` Eigenschaft wird definiert, um eine Instanz des enthalten die `CollectionViewDataSource` liefert, die die Daten für die Auflistungsansicht. Ein `PersonSelected` Eigenschaft definiert ist, enthält die `PersonModel` , das aktuell ausgewählte Element in der Auflistungsansicht darstellt. Diese Eigenschaft löst außerdem die `SelectionChanged` Ereignis aus, wenn die Auswahl ändert.

Die `ConfigureCollectionView` Klasse wird verwendet, um die View-Controller zu registrieren, die als Prototyp Zelle fungiert, die mit der Auflistungsansicht mithilfe der folgenden Zeile:

```csharp
EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");
```

Beachten Sie, dass die **Bezeichner** (`EmployeeCell`) verwendet, um die Prototyp Übereinstimmungen Registrieren der Namen in der `GetItem` Methode der `CollectionViewDataSource` oben definierten:

```csharp
var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
...
```

Darüber hinaus den Typ des View-Controller **müssen** entsprechen den Namen des der `.xib` -Datei, des Prototyps definiert **genau**. Bei diesem Beispiel `EmployeeItemController` und `EmployeeItemController.xib`.

Das tatsächliche Layout der Elemente in der Auflistungsansicht wird durch eine Auflistung Ansichtslayout Klasse gesteuert und können durch Zuweisen einer neuen Instanz dynamisch zur Laufzeit geändert werden die `CollectionViewLayout` Eigenschaft. Das Ändern dieser Eigenschaft aktualisiert die Auflistungsansicht Darstellung ohne die Änderung zu animieren.

Apple geliefert wird, zwei Typen von integrierten Layout mit der Sammlung angezeigt, die am häufigsten verwendete verwendet behandelt: `NSCollectionViewFlowLayout` und `NSCollectionViewGridLayout`. Der Entwickler ggf. ein benutzerdefiniertes Format, z. B. zur Festlegung der Elemente in einen Kreis, können sie erstellen eine benutzerdefinierte Instanz von `NSCollectionViewLayout` und überschreiben Sie die erforderlichen Methoden, um den gewünschten Effekt zu erzielen.

Dieses Beispiel verwendet die Standard-Flow-Layout so die Erstellung einer Instanz von der `NSCollectionViewFlowLayout` Klasse, und es wie folgt konfiguriert:

```csharp
var flowLayout = new NSCollectionViewFlowLayout()
{
    ItemSize = new CGSize(150, 150),
    SectionInset = new NSEdgeInsets(10, 10, 10, 20),
    MinimumInteritemSpacing = 10,
    MinimumLineSpacing = 10
};
```

Die `ItemSize` -Eigenschaft definiert die Größe jeder einzelnen Zelle in der Auflistung. Die `SectionInset` Eigenschaft definiert die Abstände zwischen dem Rand der Auflistung, die Zellen in angeordnet. `MinimumInteritemSpacing` definiert den minimalen Abstand zwischen Elementen und `MinimumLineSpacing` definiert den minimalen Abstand zwischen den Zeilen in der Auflistung.

Das Layout der Auflistungsansicht und einer Instanz von zugewiesen ist die `CollectionViewDelegate` zum Behandeln von Elementauswahl angefügt ist:

```csharp
// Setup collection view
EmployeeCollection.CollectionViewLayout = flowLayout;
EmployeeCollection.Delegate = new CollectionViewDelegate(this);
```

Die `PopulateWithData` Methode erstellt eine neue Instanz der dem `CollectionViewDataSource`, mit Daten aufgefüllt, fügt ihn der Auflistungsansicht und ruft die `ReloadData` Methode zum Anzeigen von Elementen:

```csharp
private void PopulateWithData()
{
    // Make datasource
    Datasource = new CollectionViewDataSource(EmployeeCollection);

    // Build list of employees
    Datasource.Data.Add(new PersonModel("Craig Dunn", "Documentation Manager", true));
    ...

    // Populate collection view
    EmployeeCollection.ReloadData();
}
```

Die `ViewDidLoad` Methode überschrieben wird, und ruft die `ConfigureCollectionView` und `PopulateWithData` Methoden, um die endgültige Auflistungsansicht für den Benutzer anzuzeigen:

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();

    // Initialize Collection View
    ConfigureCollectionView();
    PopulateWithData();
}
```

<a name="Summary"/>

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat eine ausführliche Übersicht über das Arbeiten mit Auflistungsansichten in einer Anwendung Xamarin.Mac übernommen. Zuerst sucht er am Verfügbarmachen von einer c#-Klasse für Objective-C mit Schlüssel-Wert-Codierung (KVM) und beobachten von Schlüssel-Wert (KVO). Als Nächstes, es wurde gezeigt, wie eine kompatible KVO-Klasse verwenden, und Daten an Auflistungsansichten in Xcodes Benutzeroberflächen-Generator gebunden wird. Schließlich wurde es gezeigt, wie mit Auflistungsansichten in C#-Code interagieren.

## <a name="related-links"></a>Verwandte Links

- [MacCollectionNew (Beispiel)](https://developer.xamarin.com/samples/mac/MacCollectionNew/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Datenbindung und Schlüssel/Wert-Codierung](~/mac/app-fundamentals/databinding.md)
- [NSCollectionView](https://developer.apple.com/reference/appkit/nscollectionview)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
