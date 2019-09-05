---
title: Quell Listen in xamarin. Mac
description: In diesem Artikel wird die Arbeit mit Quell Listen in einer xamarin. Mac-Anwendung behandelt. Es beschreibt das Erstellen und Verwalten von Quell Listen in Xcode und Interface Builder und die Interaktion C# mit Ihnen im Code.
ms.prod: xamarin
ms.assetid: 651A3649-5AA8-4133-94D6-4873D99F7FCC
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/14/2017
ms.openlocfilehash: 63ce931abfbe7a39108ae3f8210209b7d43827ed
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70278555"
---
# <a name="source-lists-in-xamarinmac"></a>Quell Listen in xamarin. Mac

_In diesem Artikel wird die Arbeit mit Quell Listen in einer xamarin. Mac-Anwendung behandelt. Es beschreibt das Erstellen und Verwalten von Quell Listen in Xcode und Interface Builder und die Interaktion C# mit Ihnen im Code._

Wenn Sie mit C# und .net in einer xamarin. Mac-Anwendung arbeiten, haben Sie Zugriff auf die gleichen Quell Listen, die von einem Entwickler in *Ziel-C* und *Xcode* verwendet werden. Da xamarin. Mac direkt in Xcode integriert ist, können Sie die _Interface Builder_ von Xcode verwenden, um die Quell Listen zu erstellen und zu verwalten (oder C# Sie optional direkt im Code zu erstellen).

Eine Quell Liste ist eine besondere Art von Gliederungs Ansicht, die verwendet wird, um die Quelle einer Aktion anzuzeigen, wie z. b. die Seitenleiste in Finder oder iTunes.

[![](source-list-images/source05.png "Eine Beispiel Quell Liste")](source-list-images/source05.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit Quell Listen in einer xamarin. Mac-Anwendung behandelt. Es wird dringend empfohlen, dass Sie zunächst den Artikel [Hello, Mac](~/mac/get-started/hello-mac.md) , insbesondere die [Einführung in Xcode und die Abschnitte zu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und Outlets und [Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) , verwenden, da er wichtige Konzepte und Techniken behandelt, die wir in verwenden werden. Dieser Artikel.

Sie können sich auch den Abschnitt verfügbar machen von [ C# Klassen/Methoden zu "Ziel-C](~/mac/internals/how-it-works.md) " im Dokument " [xamarin. Mac](~/mac/internals/how-it-works.md) " ansehen. darin werden die-und `Register` `Export` -Befehle erläutert, die zum Verknüpfen der C# Klassen mit verwendet werden. Ziel-C-Objekte und UI-Elemente.

<a name="Introduction_to_Outline_Views" />

## <a name="introduction-to-source-lists"></a>Einführung in Quell Listen

Wie bereits erwähnt, ist eine Quell Liste eine besondere Art von Gliederungs Ansicht, die verwendet wird, um die Quelle einer Aktion anzuzeigen, wie z. b. die Seitenleiste in Finder oder iTunes. Eine Quell Liste ist ein Typ von Tabelle, der es dem Benutzer ermöglicht, Zeilen hierarchischer Daten zu erweitern oder zu reduzieren. Im Gegensatz zu einer Tabellenansicht befinden sich Elemente in einer Quell Liste nicht in einer flachen Liste, Sie sind in einer Hierarchie angeordnet, wie z. b. Dateien und Ordner auf einer Festplatte. Wenn ein Element in einer Quell Liste weitere Elemente enthält, kann es vom Benutzer erweitert oder reduziert werden.

Die Quell Liste ist eine speziell formatierte Gliederungs Ansicht (`NSOutlineView`), die selbst eine Unterklasse der Tabellenansicht (`NSTableView`) ist und daher einen Großteil ihres Verhaltens von der übergeordneten Klasse erbt. Infolgedessen werden viele Vorgänge, die von einer Gliederungs Ansicht unterstützt werden, auch von einer Quell Liste unterstützt. Eine xamarin. Mac-Anwendung verfügt über die Kontrolle über diese Features und kann die Parameter der Quell Liste (entweder in Code oder Interface Builder) so konfigurieren, dass bestimmte Vorgänge zugelassen oder verweigert werden.

In einer Quell Liste werden die eigenen Daten nicht gespeichert. stattdessen wird eine Datenquelle (`NSOutlineViewDataSource`) verwendet, um sowohl die erforderlichen Zeilen als auch die Spalten bereitzustellen.

Das Verhalten einer Quell Liste kann angepasst werden, indem eine Unterklasse des Gliederungs Ansichts`NSOutlineViewDelegate`Delegaten () bereitgestellt wird, um die Auswahl von Funktionen, Elementauswahl und-Bearbeitung, benutzerdefinierte Nachverfolgung und benutzerdefinierte Ansichten für einzelne Elemente zu unterstützen.

Da eine Quell Liste viel von Ihrem Verhalten und ihrer Funktionalität mit einer Tabellenansicht und einer Gliederungs Ansicht gemeinsam nutzt, sollten Sie die Dokumentation zu den [Tabellen Sichten](~/mac/user-interface/table-view.md) und Gliederungs [Ansichten](~/mac/user-interface/outline-view.md) durchlaufen, bevor Sie mit diesem Artikel fortfahren.

<a name="Working_with_Source_Lists" />

## <a name="working-with-source-lists"></a>Arbeiten mit Quell Listen

Eine Quell Liste ist eine besondere Art von Gliederungs Ansicht, die verwendet wird, um die Quelle einer Aktion anzuzeigen, wie z. b. die Seitenleiste in Finder oder iTunes. Anders als bei Gliederungs Ansichten, bevor wir die Quell Liste in Interface Builder definieren, erstellen wir die Unterstützungs Klassen in xamarin. Mac.

Zunächst erstellen wir eine neue `SourceListItem` Klasse zum Speichern der Daten für die Quell Liste. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie**neue Datei** **Hinzufügen** > ... aus. Wählen Sie **Allgemeine** > **leere Klasse**aus `SourceListItem` , geben Sie als **Namen** ein, und klicken Sie auf die Schaltfläche **neu** :

[![](source-list-images/source01.png "Hinzufügen einer leeren Klasse")](source-list-images/source01.png#lightbox)

Erstellen Sie `SourceListItem.cs` die Datei wie folgt: 

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

Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie**neue Datei** **Hinzufügen** > ... aus. Wählen Sie **Allgemeine** > **leere Klasse**aus `SourceListDataSource` , geben Sie als **Namen** ein, und klicken Sie auf die Schaltfläche **neu** . Erstellen Sie `SourceListDataSource.cs` die Datei wie folgt:

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

Dadurch werden die Daten für die Quell Liste bereitgestellt.

Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie**neue Datei** **Hinzufügen** > ... aus. Wählen Sie **Allgemeine** > **leere Klasse**aus `SourceListDelegate` , geben Sie als **Namen** ein, und klicken Sie auf die Schaltfläche **neu** . Erstellen Sie `SourceListDelegate.cs` die Datei wie folgt:

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

Dadurch wird das Verhalten der Quell Liste bereitgestellt.

Klicken Sie abschließend im **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie**neue Datei** **Hinzufügen** > ... aus. Wählen Sie **Allgemeine** > **leere Klasse**aus `SourceListView` , geben Sie als **Namen** ein, und klicken Sie auf die Schaltfläche **neu** . Erstellen Sie `SourceListView.cs` die Datei wie folgt:

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

Dadurch wird eine benutzerdefinierte, wiederverwendbare `NSOutlineView` unter`SourceListView`Klasse von () erstellt, die wir zum Steuern der Quell Liste in einer beliebigen xamarin. Mac-Anwendung verwenden können.

<a name="Creating_and_Maintaining_Source_Lists_in_Xcode" />

## <a name="creating-and-maintaining-source-lists-in-xcode"></a>Erstellen und Verwalten von Quell Listen in Xcode

Nun entwerfen wir unsere Quell Liste in Interface Builder. Doppelklicken Sie auf `Main.storyboard` die Datei, um Sie in Interface Builder zu bearbeiten, und ziehen Sie eine geteilte Ansicht aus dem **Bibliotheks Inspektor**, fügen Sie Sie dem Ansichts Controller hinzu, und legen Sie fest, dass die Größe mit der Ansicht im **Einschränkungs-Editor**geändert wird:

[![](source-list-images/source00.png "Bearbeitungs Einschränkungen")](source-list-images/source00.png#lightbox)

Ziehen Sie als nächstes eine Quell Liste aus dem **Bibliotheks Inspektor**, fügen Sie Sie auf der linken Seite der geteilten Ansicht ein, und legen Sie fest, dass die Größe mit der Ansicht im **Einschränkungs-Editor**geändert wird:

[![](source-list-images/source02.png "Bearbeitungs Einschränkungen")](source-list-images/source02.png#lightbox)

Wechseln Sie als nächstes zur **Ansicht Identität**, wählen Sie die Liste Quelle aus, und ändern Sie die `SourceListView`Klasse in:

[![](source-list-images/source03.png "Festlegen des Klassen namens")](source-list-images/source03.png#lightbox)

Erstellen Sie abschließend ein **Outlet** für die Quell Liste, `SourceList` die in `ViewController.h` der Datei aufgerufen wird:

[![](source-list-images/source04.png "Konfigurieren eines Outlets")](source-list-images/source04.png#lightbox)

Speichern Sie die Änderungen, und kehren Sie zu Visual Studio für Mac zurück, um mit Xcode zu synchronisieren.

<a name="Populating the Source List" />

## <a name="populating-the-source-list"></a>Auffüllen der Quell Liste

Wir bearbeiten die `RotationWindow.cs` Datei in Visual Studio für Mac und machen `AwakeFromNib` die Methode wie folgt:

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

Vor `Initialize ()` dem Hinzufügen von Elementen muss die-Methode für das **Outlet** der Quell Liste aufgerufen werden. Für jede Gruppe von Elementen wird ein übergeordnetes Element erstellt, und anschließend werden die untergeordneten Elemente diesem Gruppenelement hinzugefügt. Jede Gruppe wird dann der Sammlung `SourceList.AddItem (...)`der Quell Liste hinzugefügt. Die letzten zwei Zeilen laden die Daten für die Quell Liste und erweitern alle Gruppen:

```csharp
// Display side list
SourceList.ReloadData ();
SourceList.ExpandItem (null, true);
```

Bearbeiten Sie schließlich die `AppDelegate.cs` Datei, und führen `DidFinishLaunching` Sie die folgende Methode aus:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    mainWindowController = new MainWindowController ();
    mainWindowController.Window.MakeKeyAndOrderFront (this);

    var rotation = new RotationWindowController ();
    rotation.Window.MakeKeyAndOrderFront (this);
}
```

Wenn wir die Anwendung ausführen, wird Folgendes angezeigt:

[![](source-list-images/source05.png "Ein Beispiel für eine APP-Laufzeit")](source-list-images/source05.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde die Arbeit mit Quell Listen in einer xamarin. Mac-Anwendung ausführlich erläutert. Wir haben gesehen, wie Sie Quell Listen in Xcode-Interface Builder erstellen und verwalten und wie Sie mit Quell Listen C# in Code arbeiten.

## <a name="related-links"></a>Verwandte Links

- [Macskizziert (Beispiel)](https://docs.microsoft.com/samples/xamarin/mac-samples/macoutlines)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Tabellenansichten](~/mac/user-interface/table-view.md)
- [Gliederungsansichten](~/mac/user-interface/outline-view.md)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Einführung in Gliederungs Ansichten](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [NSOutlineView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [NSOutlineViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [NSOutlineViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
