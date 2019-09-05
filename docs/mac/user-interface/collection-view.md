---
title: Auflistungs Ansichten in xamarin. Mac
description: Dieser Artikel beschreibt das Arbeiten mit Auflistungs Ansichten in einer xamarin. Mac-app. Sie behandelt das Erstellen und Verwalten von Auflistungs Ansichten in Xcode und Interface Builder und Programm gesteuertes arbeiten mit Ihnen.
ms.prod: xamarin
ms.assetid: 6EE32256-5948-4AE4-8133-6D0B3F4173E8
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 05/24/2017
ms.openlocfilehash: a3673f017a5dd50e5cc3ae44790bf359c2871440
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70279623"
---
# <a name="collection-views-in-xamarinmac"></a>Auflistungs Ansichten in xamarin. Mac

_Dieser Artikel beschreibt das Arbeiten mit Auflistungs Ansichten in einer xamarin. Mac-app. Sie behandelt das Erstellen und Verwalten von Auflistungs Ansichten in Xcode und Interface Builder und Programm gesteuertes arbeiten mit Ihnen._

Beim Arbeiten mit C# und .net in einer xamarin. Mac-app hat der Entwickler Zugriff auf die gleichen AppKit-Auflistungs Ansicht-Steuerelemente, die ein Entwickler in *Ziel-C* und *Xcode* verwendet. Da xamarin. Mac direkt in Xcode integriert ist, verwendet der Entwickler Xcode- _Interface Builder_ , um Auflistungs Ansichten zu erstellen und zu verwalten.

Ein `NSCollectionView` zeigt ein Raster von unter Sichten an, die `NSCollectionViewLayout`mithilfe eines organisiert wurden. Jede untergeordnete Sicht im Raster wird durch einen `NSCollectionViewItem` dargestellt, der das Laden des Inhalts der Ansicht aus einer `.xib` Datei verwaltet.

[![Ein Beispiel für eine APP-Laufzeit](collection-view-images/intro01.png)](collection-view-images/intro01.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit Auflistungs Ansichten in einer xamarin. Mac-app behandelt. Es wird dringend empfohlen, dass Sie zunächst den Artikel [Hello, Mac](~/mac/get-started/hello-mac.md) , insbesondere die [Einführung in Xcode und die Abschnitte zu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und Outlets und [Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) , durcharbeiten, da er wichtige Konzepte und Techniken behandelt, die verwendet werden. in diesem Artikel.

Sie können sich auch den Abschnitt verfügbar machen von [ C# Klassen/Methoden zu "Ziel-C](~/mac/internals/how-it-works.md) " im Dokument " [xamarin. Mac](~/mac/internals/how-it-works.md) " ansehen. darin werden die-und `Register` `Export` -Befehle erläutert, die zum Verknüpfen der C# Klassen mit verwendet werden. Ziel-C-Objekte und UI-Elemente.

<a name="About_Collection_Views"/>

## <a name="about-collection-views"></a>Informationen zu Sammlungs Ansichten

Das Hauptziel einer Sammlungsansicht (`NSCollectionView`) besteht darin, eine Gruppe von Objekten mithilfe eines Auflistungs Ansichts Layouts (`NSCollectionViewLayout`) visuell visuell anzuordnen, wobei jedes einzelne Objekt (`NSCollectionViewItem`) eine eigene Ansicht in der größeren Auflistung erhält. Auflistungs Ansichten funktionieren über Daten Bindungen und Schlüssel-Wert-Codierungstechniken. Daher sollten Sie die [Datenbindung und die Schlüssel-Wert-Codierungs](~/mac/app-fundamentals/databinding.md) Dokumentation lesen, bevor Sie mit diesem Artikel fortfahren.

Die Sammlungsansicht verfügt über kein integriertes Standard Auflistungs Ansichts Element (wie z. b. eine Gliederung oder Tabellenansicht), sodass der Entwickler für das Entwerfen und Implementieren einer _prototypansicht_ mit anderen AppKit-Steuerelementen, wie z. b. Bildfeldern, Text Feldern, Bezeichnungen, zuständig ist. etc. Diese prototypansicht wird verwendet, um jedes Element, das von der Auflistungs Ansicht verwaltet wird, anzuzeigen und zu `.xib` bearbeiten, und wird in einer Datei gespeichert.

Da der Entwickler für das Aussehen und das Gefühl eines Auflistungs Ansichts Elements verantwortlich ist, verfügt die Sammlungsansicht nicht über integrierte Unterstützung für das Markieren eines ausgewählten Elements im Raster. Die Implementierung dieses Features wird in diesem Artikel behandelt.

<a name="Defining_your_Data_Model"/>

## <a name="defining-the-data-model"></a>Definieren des Datenmodells

Vor der Datenbindung einer Auflistungs Ansicht in Interface Builder muss in der xamarin. Mac-app eine KVO-kompatible Klasse (Key-Value Coding, KVC) definiert werden, die als _Datenmodell_ für die Bindung fungiert. Das Datenmodell stellt alle Daten bereit, die in der Auflistung angezeigt werden, und empfängt Änderungen an den Daten, die der Benutzer während der Ausführung der Anwendung in der Benutzeroberfläche vornimmt.

Sehen Sie sich das Beispiel für eine APP an, die eine Gruppe von Mitarbeitern verwaltet. die folgende Klasse kann zum Definieren des Datenmodells verwendet werden:

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

Im `PersonModel` restlichen Verlauf dieses Artikels wird das Datenmodell verwendet.

<a name="Working_with_a_Collection_View"/>

## <a name="working-with-a-collection-view"></a>Arbeiten mit einer Sammlungsansicht

Die Datenbindung mit einer Auflistungs Ansicht ähnelt der Bindung mit einer Tabellen Sicht, `NSCollectionViewDataSource` die zum Bereitstellen von Daten für die Auflistung verwendet wird. Da die Sammlungsansicht nicht über ein voreingestelltes Anzeige Format verfügt, ist mehr Arbeit erforderlich, um Feedback zur Benutzerinteraktion bereitzustellen und die Benutzer Auswahl zu verfolgen.

<a name="Creating-the-Cell-Prototype"/>

### <a name="creating-the-cell-prototype"></a>Erstellen des Zellen Prototyps

Da die Sammlungsansicht keinen Standardzellen Prototyp enthält, muss der Entwickler der xamarin. Mac-app `.xib` eine oder mehrere Dateien hinzufügen, um Layout und Inhalt der einzelnen Zellen zu definieren.

Führen Sie folgende Schritte aus:

1. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf den Projektnamen, und wählen Sie**neue Datei** **Hinzufügen** > ... aus.
2. Wählen Sie **Mac** > **View Controller** `EmployeeItem` aus, geben Sie ihm einen Namen (z.b. in diesem Beispiel), und klicken Sie auf die Schaltfläche **neu** , um Folgendes zu erstellen: 

    ![Hinzufügen eines neuen Ansichts Controllers](collection-view-images/proto01.png)

    Dadurch wird der Projekt `EmployeeItem.cs`Mappe `EmployeeItemController.cs` eine `EmployeeItemController.xib` -Datei und eine-Datei hinzugefügt.
3. Doppelklicken Sie auf `EmployeeItemController.xib` die Datei, um Sie in der Interface Builder von Xcode zur Bearbeitung zu öffnen.
4. Fügen Sie `NSBox`der `NSImageView` Ansicht ein `NSLabel` -Steuerelement und zwei-Steuerelemente hinzu, und legen Sie Sie wie folgt fest:

    ![Entwerfen des Layouts des Zellen Prototyps](collection-view-images/proto02.png)
5. Öffnen Sie den **Assistenten-Editor** , und erstellen Sie ein **Outlet** für das `NSBox` , sodass es verwendet werden kann, um den Auswahl Zustand einer Zelle anzugeben:

    ![Verfügbar machen der NSBox in einem Outlet](collection-view-images/proto03.png)
6. Wechseln Sie zurück zum **Standard-Editor** , und wählen Sie die Bildansicht aus.
7. Wählen Sie im **Bindungs Inspektor**die **Option an** > **Dateibesitzer** binden aus, und geben Sie den **Modell Schlüssel Pfad** für `self.Person.Icon`Folgendes ein:

    ![Binden des Symbols](collection-view-images/proto04.png)
8. Wählen Sie die erste Bezeichnung aus, und wählen Sie im **Bindungs Inspektor**die Option **an** > **Dateibesitzer** binden aus, und geben `self.Person.Name`Sie den **Modell Schlüssel Pfad** für Folgendes ein:

    ![Binden des Namens](collection-view-images/proto05.png)
9. Wählen Sie die zweite Bezeichnung aus, und wählen Sie im **Bindungs Inspektor**die Option **an** > **Dateibesitzer** binden aus, und geben `self.Person.Occupation`Sie den **Modell Schlüssel Pfad** für Folgendes ein:

    ![Binden der Tätigkeit](collection-view-images/proto06.png)
10. Speichern Sie die Änderungen in `.xib` der Datei, und kehren Sie zu Visual Studio zurück, um die Änderungen zu synchronisieren.

Bearbeiten Sie `EmployeeItemController.cs` die Datei, und führen Sie Sie wie folgt aus:

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

Wenn Sie diesen Code ausführlich betrachten, erbt die-Klasse `NSCollectionViewItem` von, sodass Sie als Prototyp für eine Sammlungs Ansichts Zelle fungieren kann. Die `Person` -Eigenschaft macht die-Klasse verfügbar, die zur Datenbindung an die Bildansicht und Bezeichnungen in Xcode verwendet wurde. Dies ist eine Instanz von, `PersonModel` die oben erstellt wurde.

Die `BackgroundColor` -Eigenschaft ist eine Verknüpfung `NSBox` zum- `FillColor` Steuerelement, das verwendet wird, um den Auswahl Status einer Zelle anzuzeigen. Durch Überschreiben `Selected` der-Eigenschaft `NSCollectionViewItem`von legt der folgende Code diesen Auswahl Zustand fest oder löscht ihn:

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

### <a name="creating-the-collection-view-data-source"></a>Erstellen der Sammlungs Ansichts Datenquelle

Eine Auflistungs Ansichts`NSCollectionViewDataSource`Datenquelle () stellt alle Daten für eine Auflistungs Ansicht bereit und erstellt und füllt eine Auflistungs `.xib` Ansichts Zelle (mit dem Prototyp), wie für jedes Element in der Sammlung erforderlich.

Fügen Sie dem Projekt eine neue Klasse hinzu, `CollectionViewDataSource` nennen Sie Sie, und führen Sie Sie wie folgt aus:

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

Wenn Sie diesen Code ausführlich betrachten, erbt die-Klasse `NSCollectionViewDataSource` von und stellt eine Liste `PersonModel` von-Instanzen `Data` durch die-Eigenschaft zur Verfügung.

Da diese Auflistung nur über einen Abschnitt verfügt, überschreibt der Code `GetNumberOfSections` die-Methode und `1`gibt immer zurück. Außerdem wird die `GetNumberofItems` -Methode überschrieben, wenn Sie die Anzahl der Elemente in der `Data` Eigenschaften Liste zurückgibt.

Die `GetItem` -Methode wird immer dann aufgerufen, wenn eine neue Zelle erforderlich ist und wie folgt aussieht:

```csharp
public override NSCollectionViewItem GetItem(NSCollectionView collectionView, NSIndexPath indexPath)
{
    var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
    item.Person = Data[(int)indexPath.Item];

    return item;
}
```

Die `MakeItem` `EmployeeItemController` -Methode der Auflistungs Ansicht wird aufgerufen, um eine wiederverwendbare Instanz von zu `Person` erstellen oder zurückzugeben, und die-Eigenschaft wird auf das in der angeforderten Zelle angezeigte Element festgelegt. 

`EmployeeItemController` Muss beim Sammlungs Ansichts Controller im Voraus mit folgendem Code registriert werden:

```csharp
EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");
``` 

Der im-`EmployeeCell`Befehl verwendete **Bezeichner** ( `MakeItem` ) _muss_ mit dem Namen des Ansichts Controllers, der in der Auflistungs Ansicht registriert wurde, identisch sein. Dieser Schritt wird im folgenden ausführlich beschrieben.

<a name="Handling-Item-Selection"/>

### <a name="handling-item-selection"></a>Behandeln der Elementauswahl

Um die Auswahl und Auswahl von Elementen in der Auflistung zu verarbeiten, ist `NSCollectionViewDelegate` ein erforderlich. Da in diesem Beispiel der integrierte `NSCollectionViewFlowLayout` Layouttyp verwendet wird, wird eine `NSCollectionViewDelegateFlowLayout` bestimmte Version dieses Delegaten benötigt.

Fügen Sie dem Projekt eine neue Klasse hinzu, nennen `CollectionViewDelegate` Sie Sie, und machen Sie Sie wie folgt:

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

Die `ItemsSelected` - `ItemsDeselected` Methode und die-Methode werden überschrieben und verwendet, `PersonSelected` um die-Eigenschaft des Ansichts Controllers festzulegen oder zu löschen, der die Auflistungs Ansicht behandelt, wenn der Benutzer ein Element auswählt oder deaktiviert. Dies wird im folgenden ausführlich dargestellt.

<a name="Creating-the-Collection-View-in-Interface-Builder"/>

### <a name="creating-the-collection-view-in-interface-builder"></a>Erstellen der Sammlungsansicht in Interface Builder

Wenn alle erforderlichen unterstützenden Elemente vorhanden sind, kann das Haupt Storyboard bearbeitet und eine Auflistungs Ansicht hinzugefügt werden.

Führen Sie folgende Schritte aus:

1. Doppelklicken Sie auf `Main.Storyboard` die Datei im **Projektmappen-Explorer** , um Sie in der Interface Builder von Xcode zur Bearbeitung zu öffnen.
2. Ziehen Sie eine Auflistungs Ansicht in die Hauptansicht, und ändern Sie Sie, um die Ansicht auszufüllen:

    ![Hinzufügen einer Auflistungs Ansicht zum Layout](collection-view-images/collection01.png)
3. Wenn die Auflistungs Ansicht ausgewählt ist, verwenden Sie den Einschränkungs-Editor, um Sie bei der Größenänderung an die Ansicht anzuheften:

    ![Hinzufügen von Einschränkungen](collection-view-images/collection02.png)
4. Stellen Sie sicher, dass die Auflistungs Ansicht im **Designoberfläche** ausgewählt ist (nicht in der markierten Bild Lauf **Ansicht** oder **Clip Ansicht** , in der Sie enthalten ist), wechseln Sie zum **Assistenten-Editor** , und erstellen Sie ein **Outlet** für die Sammlungsansicht:

    ![Hinzufügen von Einschränkungen](collection-view-images/collection03.png)
5. Speichern Sie die Änderungen, und kehren Sie zur Synchronisierung zu Visual Studio zurück.

<a name="Bringing-it-all-Together"/>

## <a name="bringing-it-all-together"></a>Alles zusammenbringen

Alle unterstützenden Teile wurden nun mit einer Klasse platziert, die als`PersonModel`Datenmodell fungiert (), ein `NSCollectionViewDataSource` wurde zum Bereitstellen von Daten hinzugefügt, ein `NSCollectionViewDelegateFlowLayout` wurde erstellt, um die Elementauswahl zu verarbeiten, `NSCollectionView` und eine wurde dem Haupt Storyboard hinzugefügt. und werden als Outlet (`EmployeeCollection`) verfügbar gemacht.

Der letzte Schritt besteht darin, den Ansichts Controller zu bearbeiten, der die Auflistungs Ansicht enthält, und alle Teile zusammenzufassen, um die Auflistung zu füllen und die Elementauswahl zu verarbeiten.

Bearbeiten Sie `ViewController.cs` die Datei, und führen Sie Sie wie folgt aus:

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

Wenn Sie diesen Code ausführlich betrachten, wird eine `Datasource` -Eigenschaft definiert, die `CollectionViewDataSource` eine Instanz von enthält, die die Daten für die Auflistungs Ansicht bereitstellt. Eine `PersonSelected` Eigenschaft ist so definiert, dass `PersonModel` Sie die enthält, die das aktuell ausgewählte Element in der Auflistungs Ansicht darstellt. Diese Eigenschaft löst auch das `SelectionChanged` -Ereignis aus, wenn sich die Auswahl ändert.

Die `ConfigureCollectionView` -Klasse wird verwendet, um den Ansichts Controller, der als Zellen Prototyp fungiert, mit der Sammlungsansicht in der folgenden Zeile zu registrieren:

```csharp
EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");
```

Beachten Sie, dass der`EmployeeCell` **Bezeichner** (), der zum Registrieren des Prototyps verwendet `GetItem` wird, mit `CollectionViewDataSource` dem in der-Methode der oben definierten-Methode aufgerufenen

```csharp
var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
...
```

Außerdem **muss** der Typ des Ansichts Controllers mit `.xib` dem Namen der Datei übereinstimmen, die den Prototyp **exakt**definiert. Im Fall dieses Beispiels `EmployeeItemController` und. `EmployeeItemController.xib`

Das tatsächliche Layout der Elemente in der Auflistungs Ansicht wird durch eine Auflistungs Ansichts Layout-Klasse gesteuert und kann zur Laufzeit dynamisch geändert werden, indem `CollectionViewLayout` der-Eigenschaft eine neue Instanz zugewiesen wird. Wenn Sie diese Eigenschaft ändern, wird die Darstellung der Sammlungsansicht aktualisiert, ohne die Änderung zu animieren.

Apple gibt zwei integrierte Layouttypen mit der Sammlungsansicht aus, die die meisten typischen Verwendungen `NSCollectionViewFlowLayout` behandelt `NSCollectionViewGridLayout`: und. Wenn der Entwickler ein benutzerdefiniertes Format benötigt, z. b. das Ablegen der Elemente in einem Kreis, können Sie eine `NSCollectionViewLayout` benutzerdefinierte Instanz von erstellen und die erforderlichen Methoden überschreiben, um den gewünschten Effekt zu erzielen.

In diesem Beispiel wird das standardmäßige Fluss Layout verwendet, sodass es eine `NSCollectionViewFlowLayout` Instanz der-Klasse erstellt und wie folgt konfiguriert:

```csharp
var flowLayout = new NSCollectionViewFlowLayout()
{
    ItemSize = new CGSize(150, 150),
    SectionInset = new NSEdgeInsets(10, 10, 10, 20),
    MinimumInteritemSpacing = 10,
    MinimumLineSpacing = 10
};
```

Die `ItemSize` -Eigenschaft definiert die Größe jeder einzelnen Zelle in der Auflistung. Die `SectionInset` -Eigenschaft definiert die Insets von der Kante der Auflistung, in der Zellen angeordnet werden. `MinimumInteritemSpacing`definiert den minimalen Abstand zwischen Elementen und `MinimumLineSpacing` definiert den minimalen Abstand zwischen Zeilen in der Auflistung.

Das Layout wird der Auflistungs Ansicht zugewiesen, und eine Instanz `CollectionViewDelegate` von wird an die Elementauswahl angehängt:

```csharp
// Setup collection view
EmployeeCollection.CollectionViewLayout = flowLayout;
EmployeeCollection.Delegate = new CollectionViewDelegate(this);
```

Die `PopulateWithData` -Methode erstellt eine neue Instanz `CollectionViewDataSource`von, füllt sie mit Daten auf, fügt Sie an die Sammlungsansicht an und ruft `ReloadData` die-Methode auf, um die Elemente anzuzeigen:

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

Die `ViewDidLoad` -Methode wird überschrieben und ruft `ConfigureCollectionView` die `PopulateWithData` -Methode und die-Methode auf, um dem Benutzer die endgültige Auflistungs Ansicht anzuzeigen:

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

In diesem Artikel wurde die Arbeit mit Auflistungs Ansichten in einer xamarin. Mac-Anwendung ausführlich erläutert. Zuerst wurde das verfügbar machen einer C# Klasse für "Ziel-C" mithilfe von Key-Value Coding (KVC) und Key-Value-Beobachtungen (KVO) untersucht. Im nächsten Schritt wurde gezeigt, wie eine KVO-kompatible Klasse verwendet wird und wie Daten an Auflistungs Ansichten in der Interface Builder von Xcode gebunden werden. Schließlich wurde gezeigt, wie Sie mit Auflistungs Ansichten C# im Code interagieren.

## <a name="related-links"></a>Verwandte Links

- [Maccollectionnew (Beispiel)](https://docs.microsoft.com/samples/xamarin/mac-samples/maccollectionnew)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Datenbindung und Schlüssel/Wert-Codierung](~/mac/app-fundamentals/databinding.md)
- [NSCollectionView](https://developer.apple.com/reference/appkit/nscollectionview)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
