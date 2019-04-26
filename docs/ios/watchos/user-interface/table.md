---
title: WatchOS-Table-Steuerelemente in Xamarin
description: Dieses Dokument beschreibt, wie Sie WatchOS-Table-Steuerelemente in Xamarin zu verwenden. Es wird erläutert, Hinzufügen einer Tabelle, das Hinzufügen eines Controllers Zeile, das Erstellen und Auffüllen von Zeilen, die Reaktion auf Klicks und vieles mehr.
ms.prod: xamarin
ms.assetid: 7C14126D-9591-4387-A588-3C4521F11C55
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: cd5e7299874bbfb1b652315a549b9d067d58e9a0
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "60881287"
---
# <a name="watchos-table-controls-in-xamarin"></a>WatchOS-Table-Steuerelemente in Xamarin

WatchOS `WKInterfaceTable` Steuerelement ist viel einfacher als die iOS-Version, sondern führt eine ähnliche Rolle. Erstellen eine bildlauffähigen Liste mit Zeilen, die können benutzerdefinierte Layouts, und das Reagieren auf Fingereingabeereignisse.

![](table-images/table-list-sml.png "Überwachungsliste für Tabelle") ![](table-images/table-detail-sml.png)
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

## <a name="adding-a-table"></a>Hinzufügen einer Tabelle

Ziehen Sie die **Tabelle** Steuerelement in einer Szene. Standardmäßig sieht es diese (Anzeigen eines Layouts einzeiligen nicht angegeben):

[![](table-images/add-table-sml.png "Hinzufügen einer Tabelle")](table-images/add-table.png#lightbox)

Geben Sie einen Namen in der Tabelle der **Eigenschaften** Pads **Namen** Feld, damit er im Code verwiesen werden kann.

## <a name="add-a-row-controller"></a>Hinzufügen eines Controllers Zeile

Die Tabelle schließt automatisch eine einzelne Zeile dargestellt, die von einem zeilencontroller, die enthält eine **Gruppe** Steuerelement standardmäßig.

Festlegen der **Klasse** wählen Sie für den zeilencontroller die Zeile in der **Dokumentgliederung** , und geben Sie einen Klassennamen in der **Eigenschaften** Pad:

[![](table-images/add-row-controller-sml.png "Einen Klassennamen im Bereich \"Eigenschaften\" eingeben")](table-images/add-row-controller.png#lightbox)

Sobald die Klasse für die Zeile des Controllers festgelegt ist, die IDE erstellt eine entsprechende C# Datei im Projekt. Ziehen Sie Steuerelemente (z. B. Bezeichnungen) auf die Zeile, und geben sie Namen, damit sie im Code verwiesen werden können.




## <a name="create-and-populate-rows"></a>Erstellen und Auffüllen von Zeilen

`SetNumberOfRows` erstellt die Zeile Controllerklassen für jede Zeile mit der `Identifier` um das richtige Objekt auszuwählen. Wenn Sie eine benutzerdefinierte Ihres Controllers Zeile zugewiesen `Identifier`, ändern Sie **Standard** im Codeausschnitt unten auf den Bezeichner, die Sie verwendet haben. Die `RowController` *für jede Zeile* wird erstellt, wenn `SetNumberOfRows` aufgerufen wird und der Tabelle angezeigt.

```csharp
myTable.SetNumberOfRows ((nint)rows.Count, "default");
        // loads row controller by identifier
```

> [!IMPORTANT]
> Zeilen werden nicht virtualisiert werden, wie sie in iOS sind. Versuchen Sie es, um die Anzahl der Zeilen zu beschränken (Apple empfiehlt kleiner als 20).

Sobald die Zeilen erstellt wurden, müssen Sie jede Zelle füllen (z. B. `GetCell` dazu in iOS). Dieser Codeausschnitt aus der [WatchTables Beispiel](https://developer.xamarin.com/samples/monotouch/watchOS/WatchTables/) aktualisiert die Bezeichnung in jeder Zeile

```csharp
for (var i = 0; i < rows.Count; i++) {
    var elementRow = (RowController)myTable.GetRowController (i);
    elementRow.myRowLabel.SetText (rows [i]);
}
```

> [!IMPORTANT]
> Mithilfe von `SetNumberOfRows` , und klicken Sie dann durch die Verwendung von Schleifen `GetRowController` bewirkt, dass die gesamte Tabelle an der Apple Watch gesendet werden. Für nachfolgende Sichten der Tabelle, wenn Sie zum Hinzufügen oder entfernen müssen bestimmte Zeilen verwenden `InsertRowsAt` und `RemoveRowsAt` für eine bessere Leistung.


## <a name="respond-to-taps"></a>Reagieren auf Datenabzweigungen

Sie können auf zwei unterschiedliche Arten zum Zeilenauswahl reagieren:

- Implementieren der `DidSelectRow` Methode im Controller Schnittstelle oder
- Erstellen ein segues auf dem Storyboard und implementieren Sie `GetContextForSegue` sollten Sie die Auswahl der Zeile um einen anderen Szene zu öffnen.

### <a name="didselectrow"></a>DidSelectRow

Um die programmgesteuerte Behandlung der Zeilenauswahl, implementieren die `DidSelectRow` Methode. Verwenden Sie zum Öffnen einer neuen Szene `PushController` und der Szene Bezeichner und der, die zu verwendende Datenkontext übergeben:

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

Ziehen Sie ein segues für das Storyboard aus Ihrem Tabellenzeile in einer anderen Szene (halten Sie die **Steuerelement** gedrückt, während Sie ziehen).
Achten Sie darauf, dass Sie den Segue auswählen und weisen Sie ihm einen Bezeichner in der **Eigenschaften** Pad (z. B. `secondLevel` im folgenden Beispiel).

Im Controller-Schnittstelle, implementieren die `GetContextForSegue` Methode, und geben Sie den Datenkontext, der bereitgestellt werden sollen, um der Szene, die von der Segue angezeigt werden.

```csharp
public override NSObject GetContextForSegue (string segueIdentifier, WKInterfaceTable table, nint rowIndex)
{
    if (segueIdentifier == "secondLevel") {
        return new NSString (rows[(int)rowIndex]);
    }
    return null;
}
```

Diese Daten werden übergeben, der Ziel-Storyboard-Szene in seine `Awake` Methode.

## <a name="multiple-row-types"></a>Mehrere Zeilentypen

Das Table-Steuerelement verfügt standardmäßig über ein einzelne Zeile, die Sie entwerfen können. Hinzufügen weitere Zeile "Vorlagen" Verwenden der **Zeilen** im Feld der **Eigenschaften** Pad klicken, um eine weitere Zeile Controller erstellen:

![](table-images/prototype-rows1.png "Festlegen der Anzahl der Zeilen des Prototyps")

Festlegen der **Zeilen** Eigenschaft **3** erstellt zusätzliche Zeile Platzhalter für Sie Steuerelemente in ziehen. Legen Sie für jede Zeile der **Klasse** Name in der die **Eigenschaften** Pad, um sicherzustellen, dass die Klasse der Controller wird erstellt.

![](table-images/prototype-rows2.png "Der Prototyp Zeilen im designer")

Zum Auffüllen einer Tabelle mit einer anderen Zeile Typen verwenden die `SetRowTypes` -Methode zur Angabe des Typs des Controllers Zeile für jede Zeile in der Tabelle verwendet werden. Verwenden Sie Bezeichner von der Zeile, um anzugeben, welcher Domänencontroller für die Zeile für jede Zeile verwendet werden soll.

Die Anzahl der Elemente im Array sollte die Anzahl der Zeilen übereinstimmen, die Sie erwarten, in der Tabelle sein:

```csharp
myTable.SetRowTypes (new [] {"type1", "default", "default", "type2", "default"});
```

Wenn eine Tabelle mit mehreren Zeile Controllern aufgefüllt, müssen Sie zum Nachverfolgen der Datentyp Sie erwarten, wie Sie auf die Benutzeroberfläche aufzufüllen:

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

ein neues Feature für Tabellen, WatchOS 3 eingeführt: die Möglichkeit, den Detailseiten einen Bildlauf im Zusammenhang mit jeder Zeile wird ohne wechseln zurück in die Tabelle, und wählen Sie eine neue Zeile. Dem Detailbildschirm können durch Wischen nach oben und unten, oder verwenden die digitale Crown gescrollt werden.

![](table-images/table-scroll-sml.png "Vertikale Paging Detail-Beispiel") ![](table-images/table-detail-sml.png)

> [!IMPORTANT]
> Dieses Feature steht derzeit nur das Storyboard in Xcode Interface Builder zu bearbeiten.

Um dieses Feature zu aktivieren, wählen die `WKInterfaceTable` auf die Entwurfsoberfläche und -Tick der **vertikale Detail Paging** Option:

![](table-images/vertical-detail-paging-sml.png "Auswählen der Option vertikale Detail Paging")

Als [erläutert, die von Apple](https://developer.apple.com/reference/watchkit/wkinterfacetable#1682023) muss die Tabelle – Navigation verwenden. Übergänge für das Pagingfeature zu arbeiten. Neu schreiben jeglicher Code, der verwendet `PushController` mit segues stattdessen.

<a name="add_row_controller" />

## <a name="appendix-row-controller-code-example"></a>Anhang: Zeile Controller-Codebeispiel

Die IDE erstellt automatisch zwei Codedateien, wenn ein zeilencontroller im Designer erstellt wird. Der Code in diese generierten Dateien wird unten als Referenz angezeigt.

Die erste erhält den Namen für die Klasse, z. B. **RowController.cs**, wie folgt aus:

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

Die andere **. designer.cs** Datei ist eine partielle Klassendefinition mit dem Outlets und Aktionen, die auf der Designeroberfläche, wie in diesem Beispiel mit einem erstellt werden `WKInterfaceLabel` Steuerelement:

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

Die Outlets und Aktionen, die hier deklariert können dann im Code - verwiesen werden jedoch die **. designer.cs** Datei sollte nicht direkt bearbeitet werden.



## <a name="related-links"></a>Verwandte Links

- [WatchTables (Beispiel)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchTables/)
- [WatchKitCatalog (Beispiel)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Apple Tabelle-doc](https://developer.apple.com/reference/watchkit/wkinterfacetable)
