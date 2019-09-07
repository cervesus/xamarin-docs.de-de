---
title: watchos-Tabellen Steuerelemente in xamarin
description: In diesem Dokument wird beschrieben, wie watchos-Tabellen Steuerelemente in xamarin verwendet werden. Es wird das Hinzufügen einer Tabelle, das Hinzufügen eines Zeilen Controllers, das Erstellen und Auffüllen von Zeilen, das reagieren auf TAPS und vieles mehr erläutert.
ms.prod: xamarin
ms.assetid: 7C14126D-9591-4387-A588-3C4521F11C55
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/17/2017
ms.openlocfilehash: 2bed40c3ac2853a5f99c2b487e909164e12e676d
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70766953"
---
# <a name="watchos-table-controls-in-xamarin"></a>watchos-Tabellen Steuerelemente in xamarin

Das watchos `WKInterfaceTable` -Steuerelement ist viel einfacher als das zugehörige IOS-Gegenstück, führt jedoch eine ähnliche Rolle aus. Er erstellt eine scrollliste mit Zeilen, die benutzerdefinierte Layouts aufweisen können und auf Berührungs Ereignisse reagieren.

![](table-images/table-list-sml.png "Liste der Überwachungs Tabellen")![](table-images/table-detail-sml.png)
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

## <a name="adding-a-table"></a>Hinzufügen einer Tabelle

Ziehen Sie das **Table** -Steuerelement in eine Szene. Standardmäßig sieht Sie wie folgt aus (zeigt ein einzelnes nicht spezifiziertes Zeilen Layout an):

[![](table-images/add-table-sml.png "Hinzufügen einer Tabelle")](table-images/add-table.png#lightbox)

Geben Sie der Tabelle einen Namen im Feld **Name** des **eigenschaftenpad** , damit Sie im Code darauf verweisen kann.

## <a name="add-a-row-controller"></a>Hinzufügen eines Zeilen Controllers

Die Tabelle enthält automatisch eine einzelne Zeile, die von einem Zeilen Controller dargestellt wird, der standardmäßig ein **Gruppen** Steuerelement enthält.

Um die- **Klasse** für den Zeilen Controller festzulegen, wählen Sie die Zeile in der **Dokument** Gliederung aus, und geben Sie im **eigenschaftenpad** einen Klassennamen ein:

[![](table-images/add-row-controller-sml.png "Eingeben eines Klassen namens im eigenschaftenpad")](table-images/add-row-controller.png#lightbox)

Nachdem die-Klasse für den Controller der Zeile festgelegt wurde, erstellt die IDE eine C# entsprechende-Datei im Projekt. Ziehen Sie Steuerelemente (z. b. Bezeichnungen) auf die Zeile, und geben Sie Ihnen Namen, damit Sie im Code auf Sie verweisen können.

## <a name="create-and-populate-rows"></a>Erstellen und Auffüllen von Zeilen

`SetNumberOfRows`erstellt die Zeilen Controller Klassen für jede Zeile unter Verwendung `Identifier` von, um die richtige auszuwählen. Wenn Sie Ihren Zeilen Controller als Benutzer `Identifier`definiert angegeben haben, ändern Sie den **Standard** Wert im Code Ausschnitt unten in den Bezeichner, den Sie verwendet haben. Die `RowController` *für jede Zeile* wird erstellt, `SetNumberOfRows` wenn aufgerufen wird und die Tabelle angezeigt wird.

```csharp
myTable.SetNumberOfRows ((nint)rows.Count, "default");
    // loads row controller by identifier
```

> [!IMPORTANT]
> Tabellenzeilen werden nicht wie in ios virtualisiert. Versuchen Sie, die Anzahl der Zeilen einzuschränken (Apple empfiehlt weniger als 20).

Nachdem die Zeilen erstellt wurden, müssen Sie jede Zelle Auffüllen (wie `GetCell` z. b. in IOS). Dieser Code Ausschnitt aus dem [watchtables-Beispiel](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchtables) aktualisiert die Bezeichnung in jeder Zeile.

```csharp
for (var i = 0; i < rows.Count; i++) {
    var elementRow = (RowController)myTable.GetRowController (i);
    elementRow.myRowLabel.SetText (rows [i]);
}
```

> [!IMPORTANT]
> Durch die Verwendung `SetNumberOfRows` von und anschließendes durchlaufen vonwirddiegesamteTabelleandieÜberwachunggesendet.`GetRowController` Wenn Sie in nachfolgenden Sichten der Tabelle bestimmte Zeilen hinzufügen oder entfernen müssen, verwenden `InsertRowsAt` Sie `RemoveRowsAt` und, um die Leistung zu verbessern.

## <a name="respond-to-taps"></a>Auf tippen reagieren

Sie können auf zwei verschiedene Arten auf die Zeilenauswahl reagieren:

- Implementieren Sie `DidSelectRow` die-Methode für den Schnittstellen Controller, oder
- Erstellen Sie auf dem Storyboard einen-Abschnitt, `GetContextForSegue` und implementieren Sie, wenn Sie möchten, dass die Zeilenauswahl eine andere Szene öffnet.

### <a name="didselectrow"></a>Didselectrow

Implementieren Sie die-Methode, um die `DidSelectRow` Zeilenauswahl Programm gesteuert zu verarbeiten. Zum Öffnen einer neuen Szene verwenden `PushController` Sie, und übergeben Sie den Bezeichner der Szene und den zu verwendenden Datenkontext:

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

Ziehen Sie einen Bild-auf dem Storyboard aus der Tabellenzeile in eine andere Szene (halten **Sie die STRG** -Taste gedrückt).
Stellen Sie sicher, dass Sie den-Typ auswählen, und geben Sie ihm einen Bezeichner im `secondLevel` **eigenschaftenpad** (z. b. im folgenden Beispiel).

Implementieren Sie im Interface Controller die `GetContextForSegue` -Methode, und geben Sie den Datenkontext zurück, der für die Szene bereitgestellt werden soll, die von der-Methode dargestellt wird.

```csharp
public override NSObject GetContextForSegue (string segueIdentifier, WKInterfaceTable table, nint rowIndex)
{
    if (segueIdentifier == "secondLevel") {
        return new NSString (rows[(int)rowIndex]);
    }
    return null;
}
```

Diese Daten werden in `Awake` der-Methode an die Ziel Storyboard-Szene übermittelt.

## <a name="multiple-row-types"></a>Mehrere Zeilen Typen

Standardmäßig verfügt das Tabellen Steuerelement über einen einzelnen Zeilentyp, den Sie entwerfen können. Zum Hinzufügen von weiteren Zeilen "Vorlagen" verwenden Sie das Feld **Zeilen** im **eigenschaftenpad** , um weitere Zeilen Controller zu erstellen:

![](table-images/prototype-rows1.png "Festlegen der Anzahl von prototypzeilen")

Wenn Sie die **Rows** -Eigenschaft auf **3** festlegen, werden zusätzliche Zeilen Platzhalter erstellt, in die Sie Steuerelemente ziehen können. Legen Sie für jede Zeile den **Klassen** Namen im **eigenschaftenpad** fest, um sicherzustellen, dass die Row Controller-Klasse erstellt wird.

![](table-images/prototype-rows2.png "Die prototypzeilen im Designer")

Zum Auffüllen einer Tabelle mit unterschiedlichen Zeilen Typen verwenden Sie `SetRowTypes` die-Methode, um den für jede Zeile in der Tabelle zu verwendenden Zeilen Controllertyp anzugeben. Verwenden Sie die Bezeichner der Zeile, um anzugeben, welcher Zeilen Controller für jede Zeile verwendet werden soll.

Die Anzahl der Elemente in diesem Array sollte der Anzahl der Zeilen entsprechen, die in der Tabelle erwartet werden:

```csharp
myTable.SetRowTypes (new [] {"type1", "default", "default", "type2", "default"});
```

Beim Auffüllen einer Tabelle mit mehreren Zeilen Controllern müssen Sie nachverfolgen, welcher Typ erwartet wird, wenn Sie die Benutzeroberfläche Auffüllen:

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

## <a name="vertical-detail-paging"></a>Paging für vertikale Details

watchos 3 hat ein neues Feature für Tabellen eingeführt: die Möglichkeit, einen Bildlauf durch die Detailseiten im Zusammenhang mit den einzelnen Zeilen durchführen zu können, ohne zur Tabelle zurückkehren und eine andere Zeile auswählen zu müssen. Die Detailbildschirm können durch Schwenken nach oben oder unten oder mithilfe des Digital Crown durch ein-oder heruntergefahren werden.

![](table-images/table-scroll-sml.png "Beispiel für vertikales Detail Paging")![](table-images/table-detail-sml.png)

> [!IMPORTANT]
> Diese Funktion ist zurzeit nur verfügbar, wenn das Storyboard in Xcode-Interface Builder bearbeitet wird.

Um dieses Feature zu aktivieren, wählen `WKInterfaceTable` Sie auf der Entwurfs Oberfläche aus, und wählen Sie die Option für die **vertikale Detail Auslagerung** aus:

![](table-images/vertical-detail-paging-sml.png "Auswählen der Option für vertikales detailpaging")

Wie [von Apple erläutert](https://developer.apple.com/reference/watchkit/wkinterfacetable#1682023) , muss die Tabellennavigation für das Paging-Feature für die Paging-Funktion einen anderen Aspekt verwenden. Schreiben Sie ggf. vorhandenen Code, `PushController` der verwendet, um Segues zu verwenden.

<a name="add_row_controller" />

## <a name="appendix-row-controller-code-example"></a>Anhang: Code Beispiel für Zeilen Controller

Die IDE erstellt automatisch zwei Code Dateien, wenn ein Zeilen Controller im Designer erstellt wird. Der Code in diesen generierten Dateien wird im folgenden als Referenz dargestellt.

Der erste wird für die-Klasse benannt, z. b. **RowController.cs**, wie folgt:

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

Die andere **. Designer.cs** -Datei ist eine partielle Klassendefinition, die die Outlets und Aktionen enthält, die auf der Designer Oberfläche erstellt werden, z. `WKInterfaceLabel` b. in diesem Beispiel mit einem Steuerelement:

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

Auf die hier deklarierten Outlets und Aktionen kann im Code verwiesen werden. die **Designer.cs** -Datei sollte jedoch nicht direkt bearbeitet werden.

## <a name="related-links"></a>Verwandte Links

- [WatchTables (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchtables)
- [WatchKitCatalog (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Tabellen Dokument von Apple](https://developer.apple.com/reference/watchkit/wkinterfacetable)
