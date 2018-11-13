---
title: Quelllisten in Xamarin.Mac
description: Dieser Artikel behandelt die Arbeit mit Quelllisten in einer Xamarin.Mac-Anwendung. Es wird beschrieben, erstellen und verwalten Quelllisten in Xcode und Interface Builder in C#-Code mit ihnen interagieren.
ms.prod: xamarin
ms.assetid: 651A3649-5AA8-4133-94D6-4873D99F7FCC
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 82e4dfb9add7002fd7d3568d0ec946ea38dfd530
ms.sourcegitcommit: 7eed80186e23e6aff3ddbbf7ce5cd1fa20af1365
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2018
ms.locfileid: "51526396"
---
# <a name="source-lists-in-xamarinmac"></a>Quelllisten in Xamarin.Mac

_Dieser Artikel behandelt die Arbeit mit Quelllisten in einer Xamarin.Mac-Anwendung. Es wird beschrieben, erstellen und verwalten Quelllisten in Xcode und Interface Builder in C#-Code mit ihnen interagieren._

Bei der Arbeit mit C# und .NET in einer Xamarin.Mac-Anwendung, Sie haben Zugriff auf die gleiche Quelle aufgeführt werden, die ein Entwickler *Objective-C-* und *Xcode* ist. Da Xamarin.Mac direkt in Xcode integriert ist, können Sie von Xcode _Interface Builder_ erstellen und Verwalten Ihrer Quelllisten (oder erstellen sie optional direkt in c#-Code).

Eine Liste der Quelle ist eine besondere Art von Gliederungsansicht verwendet, um die Quelle der Aktion, wie die Seitenleiste in Finder oder iTunes anzuzeigen.

[![](source-list-images/source05.png "Eine Liste der Beispiel-Quelle")](source-list-images/source05.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit Quelllisten in einer Xamarin.Mac-Anwendung beschrieben. Es wird dringend empfohlen, dass Sie über arbeiten die [Hallo, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst, insbesondere die [Einführung in Xcode und Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und [Outlets und Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) Abschnitte, wie sie behandelt wichtige Konzepte und Techniken, die wir in diesem Artikel verwenden.

Empfiehlt es sich um einen Blick auf die [Verfügbarmachen von c#-Klassen / Methoden mit Objective-C](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac-Interna](~/mac/internals/how-it-works.md) dokumentieren, es wird erläutert, die `Register` und `Export` Befehle verwendet, um Ihre Klassen in c# für Objective-C-Objekte und Elemente der Benutzeroberfläche anschließen.

<a name="Introduction_to_Outline_Views" />

## <a name="introduction-to-source-lists"></a>Einführung in die Quelllisten

Wie bereits erwähnt, ist eine Quelle-Liste an, dass eine besondere Art von Gliederungsansicht verwendet wird, um die Quelle der Aktion, wie die Seitenleiste in Finder oder iTunes anzuzeigen. Eine Liste der Quelle wird einer Tabelle, die dem Benutzer ermöglicht erweitern oder Reduzieren von Zeilen von hierarchischen Daten. Im Gegensatz zu einer Tabellenansicht Elemente in einer Liste der Quelle nicht in einer flachen Liste, deren Organisation in einer Hierarchie, wie Dateien und Ordner auf einer Festplatte. Wenn ein Element in einer Liste der Datenquellen auf andere Elemente enthält, kann er erweitert oder reduziert, die vom Benutzer sein.

Der Quellliste ist eine speziell formatierte Gliederungsansicht (`NSOutlineView`), die selbst handelt es sich um eine Unterklasse der Tabellenansicht (`NSTableView`) und der Großteil des Verhaltens, von der übergeordneten Klasse erbt. Daher werden viele Vorgänge, die von einer Gliederungsansicht unterstützt auch unterstützt von einer Quelle-Liste. Eine Xamarin.Mac-Anwendung hat die Kontrolle über diese Features, und Sie können der Quellliste-Parameter (entweder im Code oder in Interface Builder) zum Zulassen oder verbieten des bestimmte Vorgänge konfigurieren.

Eine Liste der Datenquellen speichert keine eigenen Daten, die stattdessen greift Sie auf eine Datenquelle (`NSOutlineViewDataSource`) um die Zeilen und Spalten, die erforderlich sind, klicken Sie auf einen Bedarf bereitzustellen.

Eine Quellliste Verhalten kann angepasst werden, durch die Bereitstellung einer Unterklasse des Delegaten Gliederung anzeigen (`NSOutlineViewDelegate`) um Konturtyp zum Auswählen von Funktionen zu unterstützen, Element Auswahl "und" Bearbeiten "," benutzerdefinierte nachverfolgung "und" benutzerdefinierte Ansichten für einzelne Elemente.

Da eine Quellliste Großteil dessen Verhalten und die Funktionen für eine Ansicht der Tabelle und einer Gliederungsansicht freigegeben hat, möchten Sie möglicherweise durchlaufen unsere [Tabellenansichten](~/mac/user-interface/table-view.md) und [Gliederungsansichten](~/mac/user-interface/outline-view.md) Dokumentation, bevor Sie den Vorgang fortsetzen In diesem Artikel.

<a name="Working_with_Source_Lists" />

## <a name="working-with-source-lists"></a>Arbeiten mit Quelllisten

Eine Liste der Quelle ist eine besondere Art von Gliederungsansicht verwendet, um die Quelle der Aktion, wie die Seitenleiste in Finder oder iTunes anzuzeigen. Im Gegensatz zu Gliederungsansichten bevor wir unser Quellliste in Interface Builder definieren, erstellen wir die unterstützenden Klassen in Xamarin.Mac.

Zunächst erstellen wir ein neues `SourceListItem` Klasse zum Speichern der Daten für unsere Quellliste. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen** > **neue Datei...** Wählen Sie **allgemeine** > **leere Klasse**, geben Sie `SourceListItem` für die **Namen** , und klicken Sie auf die **neu** Schaltfläche:

[![](source-list-images/source01.png "Eine leere Klasse hinzufügen")](source-list-images/source01.png#lightbox)

Stellen Sie die `SourceListItem.cs` Datei aussehen wie folgt: 

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using AppKit;
using Foundation;

namespace MacOutlines
{
    public class SourceListItem: NSObject, IEnumerator, IEnumerable
    {
        #region Private Properties
        private string _title;
        private NSImage _icon;
        private string _tag;
        private List<SourceListItem> _items = new List<SourceListItem> ();
        #endregion

        #region Computed Properties
        public string Title {
            get { return _title; }
            set { _title = value; }
        }

        public NSImage Icon {
            get { return _icon; }
            set { _icon = value; }
        }

        public string Tag {
            get { return _tag; }
            set { _tag=value; }
        }
        #endregion

        #region Indexer
        public SourceListItem this[int index]
        {
            get
            {
                return _items[index];
            }

            set
            {
                _items[index] = value;
            }
        }

        public int Count {
            get { return _items.Count; }
        }

        public bool HasChildren {
            get { return (Count > 0); }
        }
        #endregion

        #region Enumerable Routines
        private int _position = -1;

        public IEnumerator GetEnumerator()
        {
            _position = -1;
            return (IEnumerator)this;
        }

        public bool MoveNext()
        {
            _position++;
            return (_position < _items.Count);
        }

        public void Reset()
        {_position = -1;}

        public object Current
        {
            get 
            { 
                try 
                {
                    return _items[_position];
                }

                catch (IndexOutOfRangeException)
                {
                    throw new InvalidOperationException();
                }
            }
        }
        #endregion

        #region Constructors
        public SourceListItem ()
        {
        }

        public SourceListItem (string title)
        {
            // Initialize
            this._title = title;
        }

        public SourceListItem (string title, string icon)
        {
            // Initialize
            this._title = title;
            this._icon = NSImage.ImageNamed (icon);
        }

        public SourceListItem (string title, string icon, ClickedDelegate clicked)
        {
            // Initialize
            this._title = title;
            this._icon = NSImage.ImageNamed (icon);
            this.Clicked = clicked;
        }

        public SourceListItem (string title, NSImage icon)
        {
            // Initialize
            this._title = title;
            this._icon = icon;
        }

        public SourceListItem (string title, NSImage icon, ClickedDelegate clicked)
        {
            // Initialize
            this._title = title;
            this._icon = icon;
            this.Clicked = clicked;
        }

        public SourceListItem (string title, NSImage icon, string tag)
        {
            // Initialize
            this._title = title;
            this._icon = icon;
            this._tag = tag;
        }

        public SourceListItem (string title, NSImage icon, string tag, ClickedDelegate clicked)
        {
            // Initialize
            this._title = title;
            this._icon = icon;
            this._tag = tag;
            this.Clicked = clicked;
        }
        #endregion

        #region Public Methods
        public void AddItem(SourceListItem item) {
            _items.Add (item);
        }

        public void AddItem(string title) {
            _items.Add (new SourceListItem (title));
        }

        public void AddItem(string title, string icon) {
            _items.Add (new SourceListItem (title, icon));
        }

        public void AddItem(string title, string icon, ClickedDelegate clicked) {
            _items.Add (new SourceListItem (title, icon, clicked));
        }

        public void AddItem(string title, NSImage icon) {
            _items.Add (new SourceListItem (title, icon));
        }

        public void AddItem(string title, NSImage icon, ClickedDelegate clicked) {
            _items.Add (new SourceListItem (title, icon, clicked));
        }

        public void AddItem(string title, NSImage icon, string tag) {
            _items.Add (new SourceListItem (title, icon, tag));
        }

        public void AddItem(string title, NSImage icon, string tag, ClickedDelegate clicked) {
            _items.Add (new SourceListItem (title, icon, tag, clicked));
        }

        public void Insert(int n, SourceListItem item) {
            _items.Insert (n, item);
        }

        public void RemoveItem(SourceListItem item) {
            _items.Remove (item);
        }

        public void RemoveItem(int n) {
            _items.RemoveAt (n);
        }

        public void Clear() {
            _items.Clear ();
        }
        #endregion

        #region Events
        public delegate void ClickedDelegate();
        public event ClickedDelegate Clicked;

        internal void RaiseClickedEvent() {
            // Inform caller
            if (this.Clicked != null)
                this.Clicked ();
        }
        #endregion
    }
}
```

In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen** > **neue Datei...** Wählen Sie **allgemeine** > **leere Klasse**, geben Sie `SourceListDataSource` für die **Namen** , und klicken Sie auf die **neu** Schaltfläche. Stellen Sie die `SourceListDataSource.cs` Datei aussehen wie folgt:

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using AppKit;
using Foundation;

namespace MacOutlines
{
    public class SourceListDataSource : NSOutlineViewDataSource
    {
        #region Private Variables
        private SourceListView _controller;
        #endregion

        #region Public Variables
        public List<SourceListItem> Items = new List<SourceListItem>();
        #endregion

        #region Constructors
        public SourceListDataSource (SourceListView controller)
        {
            // Initialize
            this._controller = controller;
        }
        #endregion

        #region Override Properties
        public override nint GetChildrenCount (NSOutlineView outlineView, Foundation.NSObject item)
        {
            if (item == null) {
                return Items.Count;
            } else {
                return ((SourceListItem)item).Count;
            }
        }

        public override bool ItemExpandable (NSOutlineView outlineView, Foundation.NSObject item)
        {
            return ((SourceListItem)item).HasChildren;
        }

        public override NSObject GetChild (NSOutlineView outlineView, nint childIndex, Foundation.NSObject item)
        {
            if (item == null) {
                return Items [(int)childIndex];
            } else {
                return ((SourceListItem)item) [(int)childIndex]; 
            }
        }

        public override NSObject GetObjectValue (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item)
        {
            return new NSString (((SourceListItem)item).Title);
        }
        #endregion

        #region Internal Methods
        internal SourceListItem ItemForRow(int row) {
            int index = 0;

            // Look at each group
            foreach (SourceListItem item in Items) {
                // Is the row inside this group?
                if (row >= index && row <= (index + item.Count)) {
                    return item [row - index - 1];
                }

                // Move index
                index += item.Count + 1;
            }

            // Not found 
            return null;
        }
        #endregion
    }
}
```

Dies stellt die Daten für unsere Quellliste bereit.

In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen** > **neue Datei...** Wählen Sie **allgemeine** > **leere Klasse**, geben Sie `SourceListDelegate` für die **Namen** , und klicken Sie auf die **neu** Schaltfläche. Stellen Sie die `SourceListDelegate.cs` Datei aussehen wie folgt:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacOutlines
{
    public class SourceListDelegate : NSOutlineViewDelegate
    {
        #region Private variables
        private SourceListView _controller;
        #endregion

        #region Constructors
        public SourceListDelegate (SourceListView controller)
        {
            // Initialize
            this._controller = controller;
        }
        #endregion

        #region Override Methods
        public override bool ShouldEditTableColumn (NSOutlineView outlineView, NSTableColumn tableColumn, Foundation.NSObject item)
        {
            return false;
        }

        public override NSCell GetCell (NSOutlineView outlineView, NSTableColumn tableColumn, Foundation.NSObject item)
        {
            nint row = outlineView.RowForItem (item);
            return tableColumn.DataCellForRow (row);
        }

        public override bool IsGroupItem (NSOutlineView outlineView, Foundation.NSObject item)
        {
            return ((SourceListItem)item).HasChildren;
        }

        public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item)
        {
            NSTableCellView view = null;

            // Is this a group item?
            if (((SourceListItem)item).HasChildren) {
                view = (NSTableCellView)outlineView.MakeView ("HeaderCell", this);
            } else {
                view = (NSTableCellView)outlineView.MakeView ("DataCell", this);
                view.ImageView.Image = ((SourceListItem)item).Icon;
            }

            // Initialize view
            view.TextField.StringValue = ((SourceListItem)item).Title;

            // Return new view
            return view;
        }

        public override bool ShouldSelectItem (NSOutlineView outlineView, Foundation.NSObject item)
        {
            return (outlineView.GetParent (item) != null);
        }

        public override void SelectionDidChange (NSNotification notification)
        {
            NSIndexSet selectedIndexes = _controller.SelectedRows;

            // More than one item selected?
            if (selectedIndexes.Count > 1) {
                // Not handling this case
            } else {
                // Grab the item
                var item = _controller.Data.ItemForRow ((int)selectedIndexes.FirstIndex);

                // Was an item found?
                if (item != null) {
                    // Fire the clicked event for the item
                    item.RaiseClickedEvent ();

                    // Inform caller of selection
                    _controller.RaiseItemSelected (item);
                }
            }
        }
        #endregion
    }
}
```

Dadurch wird das Verhalten von unserem Quellliste bereitgestellt.

Schließlich wird in der **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen** > **neue Datei...** Wählen Sie **allgemeine** > **leere Klasse**, geben Sie `SourceListView` für die **Namen** , und klicken Sie auf die **neu** Schaltfläche. Stellen Sie die `SourceListView.cs` Datei aussehen wie folgt:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacOutlines
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

Dies erstellt eine benutzerdefinierte, wiederverwendbare Unterklasse von `NSOutlineView` (`SourceListView`), die wir verwenden können, die der Quellliste in einer Xamarin.Mac-Anwendung zu versorgen, die wir stellen.

<a name="Creating_and_Maintaining_Source_Lists_in_Xcode" />

## <a name="creating-and-maintaining-source-lists-in-xcode"></a>Erstellen und verwalten Quelllisten in Xcode

Nun, wir entwerfen unsere Quellliste in Interface Builder. Doppelklicken Sie auf die `Main.storyboard` Datei zur Bearbeitung in Interface Builder öffnen, und ziehen eine geteilte Ansicht aus der **Bibliotheksinspektor**, fügen Sie es mit dem Ansicht-Controller hinzu, und legen Sie sie mit der Ansicht im Ändern der Größe der **Einschränkungs-Editors** :

[![](source-list-images/source00.png "Bearbeiten von Einschränkungen")](source-list-images/source00.png#lightbox)

Ziehen Sie anschließend eine Liste der Quelle aus der **Bibliotheksinspektor**, fügen sie Sie im linken Bildschirmbereich der geteilten Ansicht hinzu, und legen Sie sie mit der Ansicht im Ändern der Größe der **Einschränkungs-Editors**:

[![](source-list-images/source02.png "Bearbeiten von Einschränkungen")](source-list-images/source02.png#lightbox)

Als Nächstes wechseln Sie zu der **Identität Ansicht**, wählen Sie die Quelle aus, und ändern Sie ihn der **Klasse** zu `SourceListView`:

[![](source-list-images/source03.png "Den Namen der Klasse festlegen")](source-list-images/source03.png#lightbox)

Erstellen Sie abschließend eine **Outlet** für unsere Quellliste aufgerufen `SourceList` in die `ViewController.h` Datei:

[![](source-list-images/source04.png "Konfigurieren eines Outlets")](source-list-images/source04.png#lightbox)

Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode synchronisiert.

<a name="Populating the Source List" />

## <a name="populating-the-source-list"></a>Auffüllen der Quellliste

Ermöglicht das Bearbeiten der `RotationWindow.cs` Datei in Visual Studio für Mac, und legen Sie ihn der `AwakeFromNib` Methode sehen wie folgt:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Populate source list
    SourceList.Initialize ();

    var library = new SourceListItem ("Library");
    library.AddItem ("Venues", "house.png", () => {
        Console.WriteLine("Venue Selected");
    });
    library.AddItem ("Singers", "group.png");
    library.AddItem ("Genre", "cards.png");
    library.AddItem ("Publishers", "box.png");
    library.AddItem ("Artist", "person.png");
    library.AddItem ("Music", "album.png");
    SourceList.AddItem (library);

    // Add Rotation 
    var rotation = new SourceListItem ("Rotation"); 
    rotation.AddItem ("View Rotation", "redo.png");
    SourceList.AddItem (rotation);

    // Add Kiosks
    var kiosks = new SourceListItem ("Kiosks");
    kiosks.AddItem ("Sign-in Station 1", "imac");
    kiosks.AddItem ("Sign-in Station 2", "ipad");
    SourceList.AddItem (kiosks);

    // Display side list
    SourceList.ReloadData ();
    SourceList.ExpandItem (null, true);

}
```

Die `Initialize ()` Methode für unsere Quellliste aufgerufen werden müssen, **Outlet** _vor_ keine Elemente hinzugefügt werden. Für jede Gruppe von Elementen erstellen wir ein übergeordnetes Element und dann die untergeordneten Elemente auf dieses Gruppenelement hinzufügen. Jede Gruppe wird anschließend der Quellliste Auflistung hinzuzufügenden `SourceList.AddItem (...)`. Die letzten beiden Zeilen laden Sie die Daten für die Source-Liste, und alle Gruppen erweitert:

```csharp
// Display side list
SourceList.ReloadData ();
SourceList.ExpandItem (null, true);
```

Schließlich Bearbeiten der `AppDelegate.cs` Datei, und stellen die `DidFinishLaunching` Methode sehen wie folgt:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    mainWindowController = new MainWindowController ();
    mainWindowController.Window.MakeKeyAndOrderFront (this);

    var rotation = new RotationWindowController ();
    rotation.Window.MakeKeyAndOrderFront (this);
}
```

Wenn wir unsere Anwendung ausführen, wird Folgendes angezeigt:

[![](source-list-images/source05.png "Eine Beispiel-app-Ausführung")](source-list-images/source05.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat es sich um einen detaillierten Einblick in die Arbeit mit Quelllisten in einer Xamarin.Mac-Anwendung geführt. Wir haben gesehen, wie zum Erstellen und verwalten Quelllisten in Interface Builder von Xcode und wie Sie mit Quelllisten in C#-Code zu arbeiten.

## <a name="related-links"></a>Verwandte Links

- [MacOutlines (Beispiel)](https://developer.xamarin.com/samples/mac/MacOutlines/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Tabellenansichten](~/mac/user-interface/table-view.md)
- [Gliederungsansichten](~/mac/user-interface/outline-view.md)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Einführung in die Ansichten beschrieben.](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [NSOutlineView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [NSOutlineViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [NSOutlineViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
