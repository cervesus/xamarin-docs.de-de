---
title: "Automatische Größenanpassung Zeilenhöhe"
ms.topic: article
ms.prod: xamarin
ms.assetid: CE45A385-D40A-482A-90A0-E8382C2BFFB9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: f1b35905d14086dcfc0cb749c8e4cc7de1608dd5
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="auto-sizing-row-height"></a>Automatische Größenanpassung Zeilenhöhe

IOS 8, Apple eingeführt die Fähigkeit zum Erstellen einer Tabellenansicht (`UITableView`), der automatisch vergrößern und verkleinern Sie die Höhe einer Zeile basierend auf der Größe des Inhalts mit Automatisches Layout, Größenklassen und -Einschränkungen können.

iOS 11 wurde hinzugefügt, die Möglichkeit für Zeilen, um automatisch zu erweitern. Kopfzeilen, Fußzeilen und Zellen können jetzt automatisch vergrößert oder verkleinert werden anhand ihres Inhalts. Wenn Ihre Tabelle, in der iOS-Designer-Generator-Schnittstelle erstellt wird, oder es Zeilenhöhe festem Sie manuell müssen aktivieren Sie selbst die Größe von Zellen, jedoch wie in diesem Handbuch beschrieben.

## <a name="cell-layout-in-the-ios-designer"></a>Zellenlayout in der iOS-Designer

Öffnen das Storyboard für die Tabelle an, dass Sie die Zeile Auto-Größe für die in der iOS-Designer haben möchten. Wählen Sie die Zelle *Prototyp* und Entwerfen des Layouts der Zelle. Zum Beispiel:

[![](autosizing-row-height-images/table01.png "Die Zelle Prototypentwurf")](autosizing-row-height-images/table01.png#lightbox)

Fügen Sie für jedes Element im Prototyp Einschränkungen, um die Elemente in der richtigen Position zu halten, während die Tabellenansicht für Drehung oder anderen iOS-Gerät Bildschirmgrößen angepasst wird. Anheften von z. B. die `Title` nach oben, links und rechts neben der Zelle *Inhaltsansicht*:

[![](autosizing-row-height-images/table02.png "Den Titel oben, links und rechts neben der Inhaltsansicht Zellen anheften")](autosizing-row-height-images/table02.png#lightbox)

Im Fall von Beispieltabelle kleine `Label` (unter der `Title`) ist das Feld, die verkleinert und vergrößert werden, um erhöhen oder verringern Sie die Zeilenhöhe kann. Um diesen Effekt zu erreichen, fügen Sie die folgenden Einschränkungen zum Anheften einer der Links, rechts, oben und unten auf der die Bezeichnung hinzu:

[![](autosizing-row-height-images/table03.png "Diese Einschränkungen zu einer der Links, rechts, oben und unten auf der Bezeichnung anheften")](autosizing-row-height-images/table03.png#lightbox)

Nachdem wir die Elemente in die Zelle vollständig eingeschränkt haben, müssen wir, um zu verdeutlichen, welches Element gestreckt werden soll. Legen Sie hierzu die **Content Küsse Priorität** und **Content Komprimierung Widerstand Priorität** nach Bedarf in die **Layout** Abschnitt Pad Eigenschaften:

[![](autosizing-row-height-images/table03a.png "Layoutabschnitt Pad Eigenschaften")](autosizing-row-height-images/table03a.png#lightbox)

Legen Sie das Element, das zu erweiternde damit eine **niedrigeren** Küsse Priority-Wert, und ein **niedrigeren** Komprimierung Widerstand Priority-Wert.

Wir als nächstes wählen Sie den Prototyp für die Zelle, und geben Sie ihm eine eindeutige **Bezeichner**:

[![](autosizing-row-height-images/table04.png "Erteilen den Prototyp für die Zelle einen eindeutigen Bezeichner")](autosizing-row-height-images/table04.png#lightbox)

Bei unserem Beispiel `GrowCell`. Diesen Wert verwenden später, wenn die Tabelle zu füllen.

> [!IMPORTANT]
> Wenn Ihre Tabelle mehr als eine Zellentyp enthält (**Prototyp**), müssen Sie sicherstellen, jeder Typ verfügt über eine eigene, eindeutige `Identifier` für automatische Zeilengröße arbeiten.

Für jedes Element des unsere Zelle Prototyps, weisen eine **Namen** , die sie für C#-Code verfügbar machen. Zum Beispiel:

[![](autosizing-row-height-images/table05.png "Weisen Sie einen Namen für die bereitzustellenden C#-Code")](autosizing-row-height-images/table05.png#lightbox)

Als Nächstes fügen Sie eine benutzerdefinierte Klasse für die `UITableViewController`, `UITableView` und `UITableCell` (Prototype). Zum Beispiel: 

[![](autosizing-row-height-images/table06.png "Hinzufügen einer benutzerdefinierten Klasse für die UITableViewController, die UITableView und die UITableCell")](autosizing-row-height-images/table06.png#lightbox)

Um sicherzustellen, dass alle erwarteten Inhalt in unsere Bezeichnung angezeigt wird, legen Sie schließlich die **Zeilen** Eigenschaft `0`:

[![](autosizing-row-height-images/table06.png "Die Zeilen-Eigenschaft auf 0 festgelegt.")](autosizing-row-height-images/table06a.png#lightbox)

Fügen Sie über die Benutzeroberfläche definiert den Code, um automatische Höhe der Zeilengröße aktivieren wir hinzu.

## <a name="enabling-auto-resizing-height"></a>Aktivieren der automatischen Größenänderung Höhe

In beiden unsere Tabellenansicht Datasource (`UITableViewDatasource`) oder Quelle (`UITableViewSource`), wenn wir eine Zelle in der Warteschlange entfernen zu verwendenden der `Identifier` , die wir in den Designer definiert. Zum Beispiel:

```csharp
public string CellID {
    get { return "GrowCell"; }
}
...

public override UITableViewCell GetCell (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    var cell = tableView.DequeueReusableCell (CellID, indexPath) as GrowRowTableCell;
    var item = Items [indexPath.Row];

    // Setup
    cell.Image = UIImage.FromFile(item.ImageName);
    cell.Title = item.Title;
    cell.Description = item.Description;

    return cell;
}
```

Standardmäßig wird der Tabellenansicht für automatische Größenänderung Zeilenhöhe festgelegt werden. Um dies sicherzustellen, dass die `RowHeight` Eigenschaft sollte festgelegt werden, um `UITableView.AutomaticDimension`. Wir müssen auch Festlegen der `EstimatedRowHeight` Eigenschaft in unsere `UITableViewController`. Zum Beispiel:

```csharp
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Initialize table
    TableView.DataSource = new GrowRowTableDataSource(this);
    TableView.Delegate = new GrowRowTableDelegate (this);
    TableView.RowHeight = UITableView.AutomaticDimension;
    TableView.EstimatedRowHeight = 40f;
    TableView.ReloadData ();
}
```

Diese Schätzung verfügt nicht über genau ist, werden nur eine grobe Schätzung der durchschnittlichen Höhe der einzelnen Zeilen in der Tabellenansicht.

Mit diesem Code werden Wenn die app ausgeführt wird, wird jede Zeile verkleinern und basierend auf der Höhe der letzte Bezeichnung in der Zelle Prototyp vergrößert. Zum Beispiel:

[![](autosizing-row-height-images/table07.png "Eine Beispieltabelle ausführen")](autosizing-row-height-images/table07.png#lightbox)


## <a name="related-links"></a>Verwandte Links

- [GrowRowTable (Beispiel)](https://developer.xamarin.com/samples/monotouch/GrowRowTable/)
