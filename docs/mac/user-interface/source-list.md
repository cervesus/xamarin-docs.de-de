---
title: Quelllisten
description: In diesem Artikel wird das Arbeiten mit Quelllisten sind in einer Anwendung Xamarin.Mac behandelt. Es beschreibt die Erstellung und Verwaltung von Listen in Xcode und Benutzeroberflächen-Generator aus, und sie in C#-Code interagieren.
ms.prod: xamarin
ms.assetid: 651A3649-5AA8-4133-94D6-4873D99F7FCC
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: a8d3a67768b9e47833d1819c3bf44774a52d2438
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="source-lists"></a>Quelllisten

_In diesem Artikel wird das Arbeiten mit Quelllisten sind in einer Anwendung Xamarin.Mac behandelt. Es beschreibt die Erstellung und Verwaltung von Listen in Xcode und Benutzeroberflächen-Generator aus, und sie in C#-Code interagieren._

Bei der Arbeit mit c# und .NET in einer Anwendung Xamarin.Mac haben Sie Zugriff auf die gleiche Datenquelle aufgelistet, die ein Entwickler arbeiten in unter *Objective-C* und *Xcode* verfügt. Da Xamarin.Mac direkt mit Xcode integriert ist, können Sie die Xcode _Schnittstelle-Generator_ erstellen und verwalten Ihre Quelle Listen (oder erstellen sie optional direkt im C#-Code).

Eine Liste der Quelle ist eine besondere Art von Ansicht der Gliederung verwendet, um die Quelle einer Aktion, wie der Randleiste in Finder oder iTunes anzuzeigen.

[![](source-list-images/source05.png "Ein Beispiel-Quellliste")](source-list-images/source05.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit Listen Quelle in einer Anwendung Xamarin.Mac eingegangen. Wird mit hoher vorgeschlagen, dass Sie über arbeiten die [Hello, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst, insbesondere die [Einführung in Xcode und Benutzeroberflächen-Generator](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) und [Steckdosen und Aktionen](~/mac/get-started/hello-mac.md#Outlets_and_Actions) Abschnitte, wie sie behandelt wichtige Konzepte und Techniken, die in diesem Artikel verwendet werden.

Sie möchten einen Blick auf die [Verfügbarmachen von C#-Klassen / Methoden für Objective-C-](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentieren, es wird erläutert, die `Register` und `Export` Befehle verwendet, um über das Netzwerk Ihrer C#-Klassen für Objective-C-Objekte und Elemente der Benutzeroberfläche von Volltextkatalogen.

<a name="Introduction_to_Outline_Views" />

## <a name="introduction-to-source-lists"></a>Einführung in Listen

Wie bereits erwähnt, ist eine Quellliste an, dass eine besondere Art von Gliederungsansicht verwendet wird, um die Quelle einer Aktion, wie der Randleiste in Finder oder iTunes anzuzeigen. Eine Liste der Quelle ist ein Typ der Tabelle, die dem Benutzer ermöglicht erweitern oder reduzieren Zeilen mit hierarchischen Daten. Im Gegensatz zu einer Tabellenansicht Elemente in einer Quellliste sind nicht in einer flachen Liste, sie sind in einer Hierarchie, z. B. Dateien und Ordnern auf einer Festplatte organisiert. Wenn ein Element in einer Quellliste Weitere Elemente enthält, können Sie erweitert oder vom Benutzer reduziert werden.

Der Quellliste ist eine speziell formatierte Gliederungsansicht (`NSOutlineView`), die selbst ist eine Unterklasse der Tabellenansicht (`NSTableView`) und der Großteil des Verhaltens daher von der übergeordneten Klasse erbt. Daher werden viele Vorgänge, die von einer Gliederungsansicht unterstützt auch unterstützt von einer Quellliste. Eine Anwendung Xamarin.Mac hat die Kontrolle über diese Funktionen und der Quellliste-Parameter (entweder im Code oder Schnittstelle-Generator) konfigurieren, um gestattet oder verweigert bestimmte Vorgänge.

Eine Quellliste seinem eigenen Daten nicht gespeichert, er verlässt sich stattdessen auf einer Datenquelle (`NSOutlineViewDataSource`), die Zeilen und Spalten, die erforderlich sind, klicken Sie auf einen Bedarf bereitzustellen.

Eine Quellliste Verhalten kann angepasst werden, durch die Bereitstellung einer Unterklasse des Delegaten Gliederung anzeigen (`NSOutlineViewDelegate`) zur Unterstützung von Gliederung-Typ, um Funktionen auszuwählen, Artikel, Auswahl und Bearbeiten von benutzerdefinierten Nachverfolgungsdatensatzes und benutzerdefinierte Ansichten für einzelne Elemente.

Da eine Quellliste Großteil der Verhalten und die Funktionen mit einer Tabellenansicht und einer Gliederungsansicht gemeinsam verwendet wird, möchten Sie möglicherweise durchlaufen unsere [Tabellensichten](~/mac/user-interface/table-view.md) und [Gliederung Ansichten](~/mac/user-interface/outline-view.md) Dokumentation vor dem Fortsetzen mit diesem Artikel.

<a name="Working_with_Source_Lists" />

## <a name="working-with-source-lists"></a>Arbeiten mit Quelllisten

Eine Liste der Quelle ist eine besondere Art von Ansicht der Gliederung verwendet, um die Quelle einer Aktion, wie der Randleiste in Finder oder iTunes anzuzeigen. Im Gegensatz zu Gliederung Ansichten bevor wir unsere Quellliste in Benutzeroberflächen-Generator zu definieren, erstellen wir die unterstützende Klassen in Xamarin.Mac.

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

Dadurch werden die Daten für unsere Quellliste angegeben.

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

Dies stellt das Verhalten von unseren Quellliste bereit.

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

Dies erstellt eine benutzerdefinierte, wiederverwendbare Unterklasse von `NSOutlineView` (`SourceListView`), die wir in der Quellliste in jeder Anwendung Xamarin.Mac Laufwerk, die wir machen können.

<a name="Creating_and_Maintaining_Source_Lists_in_Xcode" />

## <a name="creating-and-maintaining-source-lists-in-xcode"></a>Erstellen und Verwalten von Listen in Xcode

Jetzt sehen wir unsere Quellliste in Benutzeroberflächen-Generator entwerfen. Doppelklicken Sie auf die `Main.storyboard` Datei zur Bearbeitung in der Benutzeroberflächen-Generator geöffnet, und ziehen eine geteilte Ansicht aus der **Bibliothek Inspektor**es die View-Controller hinzu, und legen Sie sie mit der Ansicht im Ändern der Größe der **Einschränkungen-Editor** :

[![](source-list-images/source00.png "Bearbeiten von Einschränkungen")](source-list-images/source00.png#lightbox)

Ziehen Sie anschließend eine Liste der Quelle aus der **Bibliothek Inspektor**, fügen Sie es auf die linke Seite der geteilten Ansicht, und legen Sie ihn mit der Ansicht im Ändern der Größe der **Einschränkungen Editor**:

[![](source-list-images/source02.png "Bearbeiten von Einschränkungen")](source-list-images/source02.png#lightbox)

Als Nächstes wechseln Sie zu der **Identität Ansicht**, wählen Sie die Quelle, und ändern Sie ihn des **Klasse** auf `SourceListView`:

[![](source-list-images/source03.png "Der Name der Klasse festlegen")](source-list-images/source03.png#lightbox)

Erstellen Sie abschließend eine **Nachrichtenplattform** für unsere Quellliste aufgerufen `SourceList` in die `ViewController.h` Datei:

[![](source-list-images/source04.png "Konfigurieren von einer Steckdose")](source-list-images/source04.png#lightbox)

Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.

<a name="Populating the Source List" />

## <a name="populating-the-source-list"></a>Auffüllen der Quellliste aus

Ermöglicht das Bearbeiten der `RotationWindow.cs` Datei in Visual Studio für Mac, und stellen sie die `AwakeFromNib` Methode aussehen wie folgt:

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

Die `Initialize ()` Methode gegen unsere Quellliste aufgerufen werden müssen **Nachrichtenplattform** _vor_ keine Elemente hinzugefügt werden. Für jede Gruppe von Elementen wir erstellen ein übergeordneten Elements und fügen Sie dann die untergeordneten Elemente mit dem Group-Element. Jede Gruppe wird dann der Quellliste Sammlung hinzugefügt `SourceList.AddItem (...)`. Die letzten beiden Zeilen laden Sie die Daten für die Quellliste, und es werden alle Gruppen erweitert:

```csharp
// Display side list
SourceList.ReloadData ();
SourceList.ExpandItem (null, true);
```

Bearbeiten Sie schließlich die `AppDelegate.cs` Datei, und stellen die `DidFinishLaunching` Methode aussehen wie folgt:

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

[![](source-list-images/source05.png "Eine Beispiel-app ausführen")](source-list-images/source05.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat eine ausführliche Übersicht über das Arbeiten mit Listen Quelle in einer Anwendung Xamarin.Mac übernommen. Wir haben gesehen, wie zum Erstellen und verwalten die Quelle führt in Xcodes Benutzeroberflächen-Generator und zum Arbeiten mit Datenquellen Listen in C#-Code.

## <a name="related-links"></a>Verwandte Links

- [MacOutlines (Beispiel)](https://developer.xamarin.com/samples/mac/MacOutlines/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Tabellenansichten](~/mac/user-interface/table-view.md)
- [Gliederungsansichten](~/mac/user-interface/outline-view.md)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Einführung in die Ansichten werden kann](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [NSOutlineView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [NSOutlineViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [NSOutlineViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
