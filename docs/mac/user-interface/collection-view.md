---
title: Auflistungsansichten in Xamarin.Mac
description: Dieser Artikel beschreibt das Arbeiten mit Auflistungsansichten in einer Xamarin.Mac-app. Hierin sind erstellen und verwalten Auflistungsansichten in Xcode und Interface Builder und Programmgesteuertes Arbeiten mit ihnen.
ms.prod: xamarin
ms.assetid: 6EE32256-5948-4AE4-8133-6D0B3F4173E8
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/24/2017
ms.openlocfilehash: c8b4b5ff8bebf5fbbded410ae84d1aefcca2d6cc
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2018
ms.locfileid: "40251157"
---
# <a name="collection-views-in-xamarinmac"></a>Auflistungsansichten in Xamarin.Mac

_Dieser Artikel beschreibt das Arbeiten mit Auflistungsansichten in einer Xamarin.Mac-app. Hierin sind erstellen und verwalten Auflistungsansichten in Xcode und Interface Builder und Programmgesteuertes Arbeiten mit ihnen._

Wenn die Arbeit mit c# und .NET in einer Xamarin.Mac-app muss der Entwickler Zugriff auf die gleichen hat AppKit Auflistungsansicht steuert, die ein Entwickler *Objective-C-* und *Xcode* ist. Da Xamarin.Mac direkt in Xcode integriert ist, verwendet der Entwickler von Xcode _Interface Builder_ erstellen und verwalten Auflistungsansichten.

Ein `NSCollectionView` zeigt ein Raster mit Unteransichten Organisation mit einem `NSCollectionViewLayout`. Durch jede Unteransicht im Raster dargestellt wird ein `NSCollectionViewItem` verwaltet das Laden der Ansicht Inhalt aus einem `.xib` Datei.

[![Eine Beispiel-app-Ausführung](collection-view-images/intro01.png)](collection-view-images/intro01.png#lightbox)

Dieser Artikel behandelt die Grundlagen der Arbeit mit Auflistungsansichten in einer Xamarin.Mac-app. Es wird dringend empfohlen, dass Sie über arbeiten die [Hallo, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst, insbesondere die [Einführung in Xcode und Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und [Outlets und Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) Abschnitte, wie sie behandelt wichtige Konzepte und Techniken, die in diesem Artikel verwendet werden.

Empfiehlt es sich um einen Blick auf die [Verfügbarmachen von c#-Klassen / Methoden mit Objective-C](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac-Interna](~/mac/internals/how-it-works.md) dokumentieren, es wird erläutert, die `Register` und `Export` Befehle verwendet, um Ihre Klassen in c# für Objective-C-Objekte und Elemente der Benutzeroberfläche anschließen.

<a name="About_Collection_Views"/>

## <a name="about-collection-views"></a>Über die Auflistungsansichten

Das Hauptziel der eine Auflistungsansicht (`NSCollectionView`) besteht darin, eine Gruppe von Objekten in einer strukturierten Weise mit einem Ansichtslayout Auflistung visuell anordnen (`NSCollectionViewLayout`), mit der jedes einzelne Objekt (`NSCollectionViewItem`) ihre eigene Ansicht abrufen, in der Auflistung größer. Auflistungsansichten zu arbeiten, über Techniken für die Datenbindung und Schlüssel / Wert-Codierung und Lesen Sie daher die [Datenbindung und Schlüssel / Wert-Codierung](~/mac/app-fundamentals/databinding.md) Dokumentation, bevor Sie mit diesem Artikel fortfahren.

Die Auflistungsansicht hat keine integrierten Auflistungselement anzeigen (wie eine Gliederung oder die Tabellenansicht der Fall ist), also der Entwickler verantwortlich für das Entwerfen und Implementieren einer _Prototyp Ansicht_ mithilfe von anderen AppKit-Steuerelementen wie z. B. Image-Feldern , Textfelder, Bezeichnungen usw. In dieser Ansicht Prototyp verwendet werden, um anzuzeigen, und Arbeiten mit jedem Element, die von der Auflistungsansicht verwaltet und befindet sich in einem `.xib` Datei.

Da der Entwickler für das Aussehen und Verhalten eines Sammelelements Ansicht verantwortlich ist, hat die Auflistungsansicht keine integrierte Unterstützung für ein ausgewähltes Element im Raster hervorheben. Implementieren dieses Feature wird in diesem Artikel erläutert.

<a name="Defining_your_Data_Model"/>

## <a name="defining-the-data-model"></a>Definieren des Datenmodells

Bevor Sie eine Auflistungsansicht-Datenbindung in Interface Builder, ein Schlüssel-Wert-Codierung (KVM) / beobachten von Schlüssel-Wert (KVO) kompatible Klasse muss definiert werden, in dem Xamarin.Mac-app, die als fungiert die _Datenmodell_ für die Bindung. Das Datenmodell enthält alle Daten, die in der Auflistung angezeigt und empfängt keine Änderungen an den Daten, die der Benutzer in der Benutzeroberfläche vorgenommen werden, beim Ausführen der Anwendung.

Betrachten Sie beispielsweise eine app, die eine Gruppe von Mitarbeitern verwaltet, die folgende Klasse verwendet werden, um das Datenmodell zu definieren:

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

Datenbindung mit einer Auflistungsansicht ist sehr ähnlich wie Bindung mit einer Tabellenansicht `NSCollectionViewDataSource` wird verwendet, um Daten für die Sammlung bereitstellen. Da die Auflistungsansicht nicht über einen vordefinierten Anzeigeformat verfügt, sind mehr Schritte erforderlich, um Feedback für die Interaktion von Benutzern bereitzustellen und zum Nachverfolgen der Auswahl des Benutzers.

<a name="Creating-the-Cell-Prototype"/>

### <a name="creating-the-cell-prototype"></a>Erstellung des Prototyps Zelle

Da einen Standard-Zelle-Prototyp in der Auflistungsansicht nicht enthalten ist, muss der Entwickler eine oder mehrere hinzuzufügende `.xib` Dateien in die Xamarin.Mac-app, um das Layout und Inhalt der einzelnen Zellen zu definieren.

Führen Sie folgende Schritte aus:

1. In der **Projektmappen-Explorer**mit der rechten Maustaste auf den Projektnamen, und wählen Sie **hinzufügen** > **neue Datei...**
2. Wählen Sie **Mac** > **Ansichtscontroller**, geben sie einen Namen (z. B. `EmployeeItem` in diesem Beispiel), und klicken Sie auf der **neu** zu erstellen: 

    ![Hinzufügen eines neuen ansichtscontrollers](collection-view-images/proto01.png)

    Hiermit wird ein `EmployeeItem.cs`, `EmployeeItemController.cs` und `EmployeeItemController.xib` Datei, die der Projektmappe.
3. Doppelklicken Sie auf die `EmployeeItemController.xib` Datei, die sie zur Bearbeitung in Xcode Interface Builder zu öffnen.
4. Hinzufügen einer `NSBox`, `NSImageView` und zwei `NSLabel` Steuerelemente an die Ansicht und ordnen sie Sie wie folgt:

    ![Entwerfen des Layouts des Prototyps Zelle](collection-view-images/proto02.png)
5. Öffnen der **Assistant Editor** , und erstellen Sie eine **Outlet** für die `NSBox` , damit sie verwendet werden kann, um den Auswahlzustand einer Zelle anzugeben:

    ![Verfügbarmachen der NSBox in eines Outlets](collection-view-images/proto03.png)
6. Wechseln Sie zurück zur der **Standard-Editor** , und wählen Sie die Image-Ansicht.
7. In der **Inspektor Bindung**wählen **binden,** > **Besitzer der Datei** , und geben Sie eine **Schlüsselpfad Modell** von `self.Person.Icon`:

    ![Binden das Symbol "](collection-view-images/proto04.png)
8. Wählen Sie die erste Bezeichnung und klicken Sie in der **Inspektor Bindung**Option **binden,** > **Besitzer der Datei** , und geben Sie eine **Modell Schlüsselpfad**von `self.Person.Name`:

    ![Binden den Namen](collection-view-images/proto05.png)
9. Wählen Sie die zweite Bezeichnung und klicken Sie in der **Inspektor Bindung**Option **binden,** > **Besitzer der Datei** , und geben Sie eine **Modell Schlüsselpfad**von `self.Person.Occupation`:

    ![Binden die "Occupation"](collection-view-images/proto06.png)
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

Betrachten diesen Code im Detail, die Klasse erbt von `NSCollectionViewItem` , damit sie als einen Prototyp für eine Auflistungsansicht Zelle fungieren kann. Die `Person` -Eigenschaft verfügbar macht, die Klasse, die für die Datenbindung an die Image-Ansicht und die Bezeichnungen in Xcode verwendet wurde. Dies ist eine Instanz der `PersonModel` oben erstellt haben.

Die `BackgroundColor` Eigenschaft ist eine Verknüpfung zu den `NSBox` des Steuerelements `FillColor` , wird verwendet, um den Auswahlstatus einer Zelle anzuzeigen. Durch Überschreiben der `Selected` Eigenschaft der `NSCollectionViewItem`, der folgende Code legt fest oder löscht diese Auswahlzustand:

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

### <a name="creating-the-collection-view-data-source"></a>Erstellen der Sammlung anzeigen-Datenquelle

Eine Auflistung anzeigen-Datenquelle (`NSCollectionViewDataSource`) enthält alle Daten für eine Auflistungsansicht erstellt und füllt einer Sammlungsansichtszelle (mithilfe der `.xib` Prototyp) für jedes Element in der Auflistung nach Bedarf.

Fügen Sie eine neue Klasse der das Projekt, nenne `CollectionViewDataSource` und legen Sie ihn wie folgt aussehen:

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

Da diese Auflistung nur ein Abschnitt hat, überschreibt der Code die `GetNumberOfSections` -Methode und gibt immer `1`. Darüber hinaus die `GetNumberofItems` Methode wird überschrieben, an, die es gibt die Anzahl der Elemente in der `Data` Eigenschaftenliste.

Die `GetItem` Methode wird aufgerufen, wenn eine neue Zelle erforderlich ist und sieht wie folgt aus:

```csharp
public override NSCollectionViewItem GetItem(NSCollectionView collectionView, NSIndexPath indexPath)
{
    var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
    item.Person = Data[(int)indexPath.Item];

    return item;
}
```

Die `MakeItem` der Auflistungsansicht aufgerufen, um erstellen oder eine wieder verwendbare Instanz zurückgeben der `EmployeeItemController` und die zugehörige `Person` -Eigenschaftensatz auf Element in der angeforderten Zelle angezeigt wird. 

Die `EmployeeItemController` müssen in der Auflistung View-Controller, die zuvor mithilfe des folgenden Codes registriert werden:

```csharp
EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");
``` 

Die **Bezeichner** (`EmployeeCell`) verwendet, der `MakeItem` Aufrufen _müssen_ mit dem Namen des View-Controller, die mit der Auflistungsansicht registriert wurde. Dieser Schritt wird weiter unten ausführlich erläutert.

<a name="Handling-Item-Selection"/>

### <a name="handling-item-selection"></a>Behandlung von Elementauswahl

Verarbeiten Sie die Auswahl und die Aufhebung der Auswahl der Elemente in der Auflistung eine `NSCollectionViewDelegate` erforderlich. Da in diesem Beispiel wird in der integrierten verwenden werden `NSCollectionViewFlowLayout` Layouttyp, eine `NSCollectionViewDelegateFlowLayout` bestimmte Version dieses Delegaten wird erforderlich sein.

Fügen Sie dem Projekt eine neue Klasse aufrufen `CollectionViewDelegate` und legen Sie ihn wie folgt aussehen:

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

Die `ItemsSelected` und `ItemsDeselected` Methoden außer Kraft gesetzt werden, und zum Aktivieren oder deaktivieren Sie die `PersonSelected` Eigenschaft des Ansichtscontrollers, der die Auflistungsansicht behandeln wird, wenn der Benutzer wählt oder hebt die Auswahl eines Elements. Dies wird weiter unten ausführlich angezeigt.

<a name="Creating-the-Collection-View-in-Interface-Builder"/>

### <a name="creating-the-collection-view-in-interface-builder"></a>Erstellen die Auflistungsansicht in Interface Builder

Alle erforderlichen unterstützenden Komponenten beisammen die Haupt-Storyboard bearbeitet werden kann, und eine Auflistungsansicht hinzugefügt wird.

Führen Sie folgende Schritte aus:

1. Doppelklicken Sie auf die `Main.Storyboard` Datei die **Projektmappen-Explorer** es für die Bearbeitung in Xcode öffnen, die Sie Interface Builder.
2. Ziehen Sie eine Auflistungsansicht in die Hauptansicht, und passen Sie es die Sicht ausfüllen:

    ![Das Layout hinzugefügt einer Auflistungsansicht](collection-view-images/collection01.png)
3. Verwenden Sie mit der Auflistungsansicht ausgewählt haben des-Editors, um heften sie an die Ansicht aus, wenn seine Größe geändert wird:

    ![Hinzufügen von Einschränkungen](collection-view-images/collection02.png)
4. Stellen Sie sicher, dass die Auflistungsansicht, in ausgewählt ist der **Entwurfsoberfläche** (und nicht die **Scrollansicht umrandet,** oder **Clip-Ansicht** , die Sie enthält), wechseln Sie zu der  **Assistenten-Editor** , und erstellen Sie eine **Outlet** für die Sammlung an:

    ![Hinzufügen von Einschränkungen](collection-view-images/collection03.png)
5. Die Änderungen zu speichern und zurück zu Visual Studio, um zu synchronisieren.

<a name="Bringing-it-all-Together"/>

## <a name="bringing-it-all-together"></a>Bringen sie alle zusammen

Alle Bestandteile unterstützen jetzt an der Stelle mit einer Klasse, die als das Datenmodell fungiert eingefügt (`PersonModel`), ein `NSCollectionViewDataSource` hinzugefügt wurde, geben Sie die Daten eine `NSCollectionViewDelegateFlowLayout` wurde entwickelt, um die Auswahl von Listenelementen zu behandeln und eine `NSCollectionView` wurde hinzugefügt, um die Haupt-Storyboard und als eines Outlets verfügbar gemacht (`EmployeeCollection`).

Der letzte Schritt ist zum Bearbeiten der View-Controller, die die Auflistungsansicht enthält, und schalten alle Bestandteile zusammen, um füllen Sie die Sammlung und Auswahl von Listenelementen zu behandeln.

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

Ein Blick auf diesen Code im Detail, eine `Datasource` Eigenschaft wird definiert, um eine Instanz von aufzunehmen der `CollectionViewDataSource` liefert, die die Daten für die Sammlung an. Ein `PersonSelected` Eigenschaft wird definiert, enthält die `PersonModel` , das das aktuell ausgewählte Element in der Auflistung darstellt. Diese Eigenschaft löst auch die `SelectionChanged` Ereignis aus, wenn die Auswahl geändert wird.

Die `ConfigureCollectionView` Klasse wird verwendet, um die View-Controller zu registrieren, die als Prototyp Zelle fungiert, mit der Sammlung angezeigt, die mit der folgenden Befehlszeile:

```csharp
EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");
```

Beachten Sie, dass die **Bezeichner** (`EmployeeCell`) verwendet, um den Prototyp Übereinstimmungen zu registrieren, den Namen in, der `GetItem` Methode der `CollectionViewDataSource` oben definierten:

```csharp
var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
...
```

Darüber hinaus den Typ des Ansichtscontrollers **müssen** mit dem Namen des der `.xib` Datei, die den Prototyp definiert **genau**. In diesem Beispiel `EmployeeItemController` und `EmployeeItemController.xib`.

Das tatsächliche Layout der Elemente in der Auflistung wird durch eine Auflistung der Standardankündigung Klasse gesteuert und können durch Zuweisen einer neuen Instanz dynamisch zur Laufzeit geändert werden die `CollectionViewLayout` Eigenschaft. Das Ändern dieser Eigenschaft aktualisiert die Auflistungsansicht-Darstellung, ohne die Änderung zu animieren.

Apple ausgeliefert wird, zwei Typen von integrierten Layout mit der Sammlung angezeigt, die meisten typische Verwendungen behandelt: `NSCollectionViewFlowLayout` und `NSCollectionViewGridLayout`. Der Entwickler ggf. ein benutzerdefiniertes Format, z. B. zur Festlegung der Elemente, im Kreis, können sie eine benutzerdefinierte Instanz von erstellen `NSCollectionViewLayout` und überschreiben Sie die erforderlichen Methoden, um den gewünschten Effekt zu erzielen.

In diesem Beispiel verwendet das Standardlayout für den Flow aus, sodass es sich um eine Instanz erstellt die `NSCollectionViewFlowLayout` -Klasse und konfiguriert sie wie folgt:

```csharp
var flowLayout = new NSCollectionViewFlowLayout()
{
    ItemSize = new CGSize(150, 150),
    SectionInset = new NSEdgeInsets(10, 10, 10, 20),
    MinimumInteritemSpacing = 10,
    MinimumLineSpacing = 10
};
```

Die `ItemSize` -Eigenschaft definiert die Größe jeder einzelnen Zelle in der Auflistung. Die `SectionInset` -Eigenschaft definiert die Abstände zwischen dem Rand der Auflistung, die Zellen in angeordnet werden. `MinimumInteritemSpacing` definiert den minimalen Abstand zwischen Elementen und `MinimumLineSpacing` definiert den minimalen Abstand zwischen den Zeilen in der Auflistung.

Das Layout der Auflistungsansicht und einer Instanz von zugewiesen wird die `CollectionViewDelegate` angefügt ist, um die Auswahl von Listenelementen zu behandeln:

```csharp
// Setup collection view
EmployeeCollection.CollectionViewLayout = flowLayout;
EmployeeCollection.Delegate = new CollectionViewDelegate(this);
```

Die `PopulateWithData` Methode erstellt eine neue Instanz der dem `CollectionViewDataSource`, es mit Daten aufgefüllt wird, hängt es an die Auflistungsansicht und ruft die `ReloadData` Methode zum Anzeigen von Elementen:

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

In diesem Artikel wird eine ausführliche Übersicht über das Arbeiten mit Auflistungsansichten in einer Xamarin.Mac-Anwendung verwendet. Zunächst sah im Endeffekt sehr auf das Verfügbarmachen von c#-Klasse mit Objective-C mithilfe von Schlüssel-Wert-Codierung (KVM), und beobachten von Schlüssel-Wert (KVO). Anschließend wurde erläutert, wie mit einer kompatiblen KVO-Klasse aus, und Daten zu Auflistungsansichten in Interface Builder von Xcode binden. Zum Schluss wird aufgezeigt, wie Sie mit Auflistungsansichten in C#-Code zu interagieren.

## <a name="related-links"></a>Verwandte Links

- [MacCollectionNew (Beispiel)](https://developer.xamarin.com/samples/mac/MacCollectionNew/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Datenbindung und Schlüssel/Wert-Codierung](~/mac/app-fundamentals/databinding.md)
- [NSCollectionView](https://developer.apple.com/reference/appkit/nscollectionview)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
