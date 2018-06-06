---
title: WatchOS Table-Steuerelemente in Xamarin
description: Dieses Dokument beschreibt, wie WatchOS Table-Steuerelemente in Xamarin verwendet wird. Es wird erläutert, Hinzufügen einer Tabelle, eine Zeile hinzufügen, erstellen und Auffüllen von Zeilen, reagieren auf datenabzweigungen und vieles mehr.
ms.prod: xamarin
ms.assetid: 7C14126D-9591-4387-A588-3C4521F11C55
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: afb8f9a96fa14877cbd0352869e23972719a4480
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791357"
---
# <a name="watchos-table-controls-in-xamarin"></a>WatchOS Table-Steuerelemente in Xamarin

Die WatchOS `WKInterfaceTable` Steuerelement ist deutlich einfacher als Gegenstück iOS, jedoch führt eine ähnliche Funktion. Erstellen eine bildlauffähigen Liste mit Zeilen, können benutzerdefinierte Layouts besitzen, und das Reagieren auf um Ereignisse zu berühren.

![](table-images/table-list-sml.png "Überwachungsliste für Tabelle") ![](table-images/table-detail-sml.png)
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

## <a name="adding-a-table"></a>Hinzufügen einer Tabelle

Ziehen Sie die **Tabelle** Steuerelement in eine Szene. Standardmäßig sieht er diese (anzeigt, die ein nicht angegebener einzeiligen Layout):

[![](table-images/add-table-sml.png "Hinzufügen einer Tabelle")](table-images/add-table.png#lightbox)

Benennen Sie der Tabelle der **Eigenschaften** Pads **Namen** Feld, sodass im Code verwiesen werden kann.

## <a name="add-a-row-controller"></a>Fügen Sie einen Controller Zeile

Die Tabelle schließt automatisch eine einzelne Zeile, dargestellt durch einen Zeile Controller, der enthält einem **Gruppe** Steuerelement standardmäßig.

Festlegen der **Klasse** für den Controller Zeile wählen Sie die Zeile in der **Dokumentgliederung** , und geben Sie einen Klassennamen in der **Eigenschaften** aufgefüllt:

[![](table-images/add-row-controller-sml.png "Einen Klassennamen eingeben, in den Eigenschaften mit Leerstellen auffüllen")](table-images/add-row-controller.png#lightbox)

Sobald die Klasse für die Zeile Controller festgelegt ist, wird die IDE eine entsprechende C#-Datei im Projekt erstellt. Ziehen Sie Steuerelemente (z. B. Bezeichnungen) auf die Zeile, und stellen Sie diesen Namen, damit sie auf im Code verwiesen werden können.




## <a name="create-and-populate-rows"></a>Erstellen und Auffüllen von Zeilen

`SetNumberOfRows` erstellt die Zeile Controllerklassen für jede Zeile mithilfe der `Identifier` um das richtige Zertifikat auszuwählen. Wenn Sie Ihren Zeile in der Regel eine benutzerdefinierte mitgeteilt `Identifier`, ändern **Standard** im Codeausschnitt unten auf den Bezeichner, die Sie verwendet haben. Die `RowController` *für jede Zeile* wird erstellt, wenn `SetNumberOfRows` aufgerufen wird und der Tabelle angezeigt.

```csharp
myTable.SetNumberOfRows ((nint)rows.Count, "default");
        // loads row controller by identifier
```

> [!IMPORTANT]
> Tabellenzeilen sind nicht virtualisiert werden, wie sie iOS werden. Versuchen Sie, um die Anzahl der Zeilen einzuschränken (Apple empfiehlt kleiner als 20).

Nachdem die Zeilen erstellt wurden, müssen Sie jede Zelle Auffüllen (z. B. `GetCell` in iOS führen würde). Dieser Codeausschnitt aus der [WatchTables Beispiel](https://developer.xamarin.com/samples/monotouch/watchOS/WatchTables/) aktualisiert die Bezeichnung in jeder Zeile

```csharp
for (var i = 0; i < rows.Count; i++) {
    var elementRow = (RowController)myTable.GetRowController (i);
    elementRow.myRowLabel.SetText (rows [i]);
}
```

> [!IMPORTANT]
> Mit `SetNumberOfRows` und klicken Sie dann in der Verwendung von Schleifen `GetRowController` bewirkt, dass die gesamte Tabelle an die Überwachung gesendet werden. Für nachfolgende Sichten der Tabelle, wenn Sie zum Hinzufügen oder Entfernen von bestimmte Zeilen verwenden `InsertRowsAt` und `RemoveRowsAt` für eine bessere Leistung.


## <a name="respond-to-taps"></a>Reagieren auf Datenabzweigungen

Sie können auf zwei unterschiedliche Arten Zeilenauswahl beantworten:

- Implementieren der `DidSelectRow` Methode auf dem Controller Schnittstelle oder
- Erstellen Sie auf das Storyboard eine Segue und implementieren Sie `GetContextForSegue` , wenn Sie die Auswahl der Zeile zu einem anderen Szene öffnen möchten.

### <a name="didselectrow"></a>DidSelectRow

Um Zeilenauswahl programmgesteuert zu behandeln, implementieren die `DidSelectRow` Methode. Verwenden Sie zum Öffnen einer neuen Szene `PushController` und übergeben der Szene Bezeichner und den Datenkontext zu verwenden:

```csharp
public override void DidSelectRow (WKInterfaceTable table, nint rowIndex)
{
    var rowData = rows [(int)rowIndex];
    Console.WriteLine ("Row selected:" + rowData);
    // if selection should open a new scene
    PushController ("secondInterface", rows[(int)rowIndex]);
}
```

### <a name="getcontextforsegue"></a>GetContextForSegue

Ziehen Sie eine Segue auf das Storyboard aus einer Zeile der Tabelle in eine andere Szene (halten Sie die **Steuerelement** gedrückt, während Sie ziehen).
Achten Sie darauf, wählen Sie die Segue, und geben Sie ihm einen Bezeichner der **Eigenschaften** Auffüllzeichen (z. B. `secondLevel` im Beispiel unten).

In den Controller Schnittstelle implementieren die `GetContextForSegue` -Methode und der Rückgabewert den Datenkontext, die bereitgestellt werden muss, der Szene haben, die durch die Segue dargestellt werden.

```csharp
public override NSObject GetContextForSegue (string segueIdentifier, WKInterfaceTable table, nint rowIndex)
{
    if (segueIdentifier == "secondLevel") {
        return new NSString (rows[(int)rowIndex]);
    }
    return null;
}
```

Diese Daten an die Ziel-Storyboard im Szene übergeben werden seine `Awake` Methode.

## <a name="multiple-row-types"></a>Mehrere Zeilentypen

Standardmäßig verfügt das Table-Steuerelement ein einzelne Zeile, die Sie entwerfen können. Hinzufügen weitere Zeile "Vorlagen" Verwenden der **Zeilen** Feld der **Eigenschaften** mit Leerstellen auffüllen, um weitere Zeile Controller zu erstellen:

![](table-images/prototype-rows1.png "Festlegen der Anzahl der Prototyp Zeilen")

Festlegen der **Zeilen** Eigenschaft **3** zusätzliche Zeile Platzhalter für das Ziehen von Steuerelementen in erstellen. Legen Sie für jede Zeile der **Klasse** Name in der die **Eigenschaften** mit Leerstellen auffüllen, um sicherzustellen, dass die Klasse der Controller wird erstellt.

![](table-images/prototype-rows2.png "Der Prototyp Zeilen im designer")

Zum Auffüllen einer Tabelle mit anderen Zeile Typen verwenden die `SetRowTypes` Methode an, dass der Typ des Controllers Zeile für jede Zeile in der Tabelle verwendet werden soll. Verwenden Sie die Zeilen-IDs, um anzugeben, welcher Controller Zeile für jede Zeile verwendet werden soll.

Die Anzahl der Elemente in diesem Array übereinstimmen, die Anzahl der Zeilen, die Sie erwarten, in der Tabelle sein:

```csharp
myTable.SetRowTypes (new [] {"type1", "default", "default", "type2", "default"});
```

Beim Auffüllen einer Tabelle mit mehreren Zeile Domänencontrollern müssen Sie zum Nachverfolgen welchen Typ Sie erwarten, wie Sie die Benutzeroberfläche auffüllen:

```csharp
for (var i = 0; i < rows.Count; i++) {
    if (i == 0) {
        var elementRow = (Type1RowController)myTable.GetRowController (i);
        // populate UI controls
    } else if (i == 3) {
        var elementRow = (Type2RowController)myTable.GetRowController (i);
        // populate UI controls
    } else {
        var elementRow = (DefaultRowController)myTable.GetRowController (i);
        // populate UI controls
    }
}
```


## <a name="vertical-detail-paging"></a>Vertikale Detail Paging

WatchOS 3 eingeführt, ein neues Feature für Tabellen: die Möglichkeit, den Detailseiten einen Bildlauf im Zusammenhang mit jeder Zeile wird ohne wechseln zurück zur Tabelle, und wählen eine neue Zeile. Die Bildschirme Detail können, indem ein Lesegerät nach oben oder unten oder digitale Kronenlänge ein Bildlauf durchgeführt werden.

![](table-images/table-scroll-sml.png "Vertikale Paging Detail-Beispiel") ![](table-images/table-detail-sml.png)

> [!IMPORTANT]
> Dieses Feature steht zurzeit nur durch das Storyboard in Xcode Schnittstelle-Generator bearbeiten.

Um dieses Feature zu aktivieren, wählen die `WKInterfaceTable` auf der Entwurfsoberfläche und Tick der **vertikale Detail Paging** Option:

![](table-images/vertical-detail-paging-sml.png "Wählen Sie die Option vertikale Detail Paging")

Als [erläutert, die von Apple](https://developer.apple.com/reference/watchkit/wkinterfacetable#1682023) muss die Tabelle – Navigation verwenden segues für das Pagingfeature funktioniert. Schreiben Sie vorhandenen Code, der verwendet `PushController` , verwenden Sie stattdessen segues.

<a name="add_row_controller" />

## <a name="appendix-row-controller-code-example"></a>Anhang: Zeile Controller-Codebeispiel

Die IDE erstellt automatisch zwei Codedateien, wenn ein Controller für die Zeile im Designer erstellt wird. Der Code in diesen generierten Dateien wird Referenzzwecken unten gezeigt.

Das erste Beispiel für die Klasse namens **RowController.cs**, wie folgt:

```csharp
using System;
using Foundation;

namespace WatchTablesExtension
{
    public partial class RowController : NSObject
    {
        public RowController ()
        {
        }
    }
}
```

Die andere **. designer.cs** Datei ist eine partielle Klassendefinition, die Aktionen, die auf der Designeroberfläche, wie das folgende Beispiel mit einem erstellt werden und Ausgänge enthält `WKInterfaceLabel` Steuerelement:

```csharp
using Foundation;
using System;
using System.CodeDom.Compiler;
using UIKit;

namespace WatchTables.OnWatchExtension
{
    [Register ("RowController")]
    partial class RowController
    {
        [Outlet]
        [GeneratedCode ("iOS Designer", "1.0")]
        public WatchKit.WKInterfaceLabel MyLabel { get; set; }

        void ReleaseDesignerOutlets ()
        {
            if (MyLabel != null) {
                MyLabel.Dispose ();
                MyLabel = null;
            }
        }
    }
}
```

Die Steckdosen und Aktionen, die hier deklariert können dann im Code - verwiesen werden jedoch die **. designer.cs** Datei sollte nicht direkt bearbeitet werden.



## <a name="related-links"></a>Verwandte Links

- [WatchTables (Beispiel)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchTables/)
- [WatchKitCatalog (Beispiel)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Apple Tabelle doc](https://developer.apple.com/reference/watchkit/wkinterfacetable)
