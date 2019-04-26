---
title: Automatische Größenanpassung der Zeilenhöhe in Xamarin.iOS
description: Dieses Dokument beschreibt, wie Sie Xamarin.iOS-apps Zeilen der Tabelle anzeigen hinzufügen, dessen Höhe basierend auf Inhalt variiert. Es wird erläutert, Zellenlayout im iOS-Designer, und aktivieren, automatische Größenänderung Höhe.
ms.prod: xamarin
ms.assetid: CE45A385-D40A-482A-90A0-E8382C2BFFB9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: e4446abc73817eb0672cd10a69ff6f738de0c1e1
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61029065"
---
# <a name="auto-sizing-row-height-in-xamarinios"></a>Automatische Größenanpassung der Zeilenhöhe in Xamarin.iOS

Ab iOS 8, Apple die Möglichkeit zum Erstellen einer Ansicht der Tabelle hinzugefügt (`UITableView`), die automatisch vergrößern und verkleinern Sie die Höhe einer bestimmten Zeile, die basierend auf der Größe des Inhalts mit Automatisches Layout, Größenklassen und Einschränkungen können.

iOS 11 wurde die Möglichkeit, dass Zeilen erweitert automatisch hinzugefügt. Kopfzeilen, Fußzeilen und Zellen können nun automatisch vergrößert oder verkleinert werden auf Grundlage ihres Inhalts. Wenn die Tabelle, in der iOS-Designer-Interface Builder erstellt wird, oder wenn er die Zeilenhöhe behoben hat Sie manuell müssen aktivieren Sie selbst die größenanpassung der Zellen, jedoch wie in diesem Handbuch beschrieben.

## <a name="cell-layout-in-the-ios-designer"></a>Zellenlayout in der iOS-Designer

Öffnen Sie wählen das Storyboard aus, für die Tabelle an, dass Sie die Zeile für die automatische Größenänderung für in der iOS-Designer haben möchten der Zelle *Prototyp* und Entwerfen des Layouts der Zelle. Zum Beispiel:

[![](autosizing-row-height-images/table01.png "Der Zelle prototypentwurfs")](autosizing-row-height-images/table01.png#lightbox)

Fügen Sie für jedes Element im Prototyp Einschränkungen, um die Elemente in der richtigen Position zu halten, während die Tabellenansicht für Dreh- oder anderen iOS-Gerät Bildschirmgrößen geändert wird. Anheften von z. B. die `Title` oben, links und rechts neben der Zelle *Ansicht "Inhalt"*:

[![](autosizing-row-height-images/table02.png "Den Titel, oben, links und rechts von der Ansicht \"Inhalt\" Zellen anheften")](autosizing-row-height-images/table02.png#lightbox)

Im Fall der Beispieltabelle, das die kleine `Label` (unter der `Title`) ist das Feld, das schrumpfen oder wachsen erhöhen oder verringern die Zeilenhöhe kann. Um diesen Effekt zu erreichen, fügen Sie die folgenden Einschränkungen zum Anheften von links, rechts, oben und unten auf der die Bezeichnung aus:

[![](autosizing-row-height-images/table03.png "Diese Einschränkungen zum Anheften von links, rechts, oben und unten auf der die Bezeichnung")](autosizing-row-height-images/table03.png#lightbox)

Nun, da wir die Elemente in die Zelle vollständig eingeschränkt haben, müssen wir zu verdeutlichen, welches Element gestreckt wird. Zu diesem Zweck legen Sie die **Content Küsse Priorität** und **Content Komprimierung Widerstand gegen Priorität** nach Bedarf die **Layout** Teil der Pad "Eigenschaften":

[![](autosizing-row-height-images/table03a.png "Das Pad \"Eigenschaften\" im Layoutabschnitt")](autosizing-row-height-images/table03a.png#lightbox)

Legen Sie das Element an die gewünschte Kategorie erweitert ist, haben eine **niedrigere** Küsse Priority-Wert, und ein **niedrigere** Komprimierung Widerstand gegen Priority-Wert.

Als Nächstes müssen wir den Prototyp für die Zelle auswählen, und weisen Sie ihm einen eindeutigen **Bezeichner**:

[![](autosizing-row-height-images/table04.png "Erteilen den Prototyp für die Zelle einen eindeutigen Bezeichner")](autosizing-row-height-images/table04.png#lightbox)

Bei unserem Beispiel `GrowCell`. Wir verwenden diesen Wert später, wenn wir die Tabelle zu füllen.

> [!IMPORTANT]
> Wenn die Tabelle mehr als eine Zellentyp enthält (**Prototyp**), müssen Sie sicherstellen, jeder Typ verfügt über eine eigene, eindeutige `Identifier` für das automatische Ändern der Zeilengröße funktioniert.

Für jedes Element von unseren Zelle Prototypen, weisen eine **Namen** zum Bereitstellen auf C# Code. Zum Beispiel:

[![](autosizing-row-height-images/table05.png "Weisen Sie einen Namen, damit verfügbar zu machen C# Code")](autosizing-row-height-images/table05.png#lightbox)

Fügen Sie eine benutzerdefinierte Klasse für den `UITableViewController`, wird die `UITableView` und `UITableCell` (Prototype). Zum Beispiel: 

[![](autosizing-row-height-images/table06.png "Hinzufügen einer benutzerdefinierten Klasse für den UITableViewController, die UITableView und die UITableCell")](autosizing-row-height-images/table06.png#lightbox)

Um sicherzustellen, dass alle erwarteten Inhalt in die Bezeichnung angezeigt wird, legen Sie schließlich die **Zeilen** Eigenschaft `0`:

[![](autosizing-row-height-images/table06.png "Die Zeilen-Eigenschaft auf 0 festgelegt.")](autosizing-row-height-images/table06a.png#lightbox)

Fügen Sie mit der Benutzeroberfläche, die definiert den Code, um das automatische Ändern der Größe Zeile Höhe zu aktivieren.

## <a name="enabling-auto-resizing-height"></a>Aktivieren der automatischen Größenänderung Höhe

In einem unserer Tabellenansicht Datasource (`UITableViewDatasource`) oder Quelle (`UITableViewSource`), wenn wir eine Zelle aus der Warteschlange entfernen wir verwenden müssen die `Identifier` , die wir in den Designer definiert. Zum Beispiel:

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

Standardmäßig wird der Tabellenansicht für die automatische Größenanpassung der Zeilenhöhe festgelegt werden. Um dies sicherzustellen, dass die `RowHeight` Eigenschaft sollte festgelegt werden, um `UITableView.AutomaticDimension`. Wir müssen auch die `EstimatedRowHeight` -Eigenschaft in unserer `UITableViewController`. Zum Beispiel:

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

Bei dieser Schätzung nicht genau ist, werden nur eine grobe Schätzung der durchschnittlichen Höhe der einzelnen Zeilen in der Tabellenansicht.

Mit diesem Code werden Wenn die app ausgeführt wird, wird jede Zeile schrumpfen oder wachsen basierend auf die Höhe der die letzte Bezeichnung in der Zelle Prototyp. Zum Beispiel:

[![](autosizing-row-height-images/table07.png "Eine Beispieltabelle ausführen")](autosizing-row-height-images/table07.png#lightbox)


## <a name="related-links"></a>Verwandte Links

- [GrowRowTable (Beispiel)](https://developer.xamarin.com/samples/monotouch/GrowRowTable/)
