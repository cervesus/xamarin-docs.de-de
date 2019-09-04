---
title: Arbeiten mit Zeilen Aktionen in xamarin. IOS
description: In dieser Anleitung wird veranschaulicht, wie benutzerdefinierte wischen-Aktionen für Tabellenzeilen mit uiswipeactionsconfiguration oder uitableviewrowaction erstellt werden.
ms.prod: xamarin
ms.assetid: 340FB633-0C46-40AA-9963-FF17D7CA6858
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/25/2017
ms.openlocfilehash: e90e108e6b02055a585129b6412641a726afaab4
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2019
ms.locfileid: "70226294"
---
# <a name="working-with-row-actions-in-xamarinios"></a>Arbeiten mit Zeilen Aktionen in xamarin. IOS

_In dieser Anleitung wird veranschaulicht, wie benutzerdefinierte wischen-Aktionen für Tabellenzeilen mit uiswipeactionsconfiguration oder uitableviewrowaction erstellt werden._

![Demonstrieren von wischen-Aktionen in Zeilen](row-action-images/action02.png)

IOS bietet zwei Möglichkeiten zum Ausführen von Aktionen für eine Tabelle `UISwipeActionsConfiguration` : `UITableViewRowAction`und.

`UISwipeActionsConfiguration`wurde in ios 11 eingeführt und wird verwendet, um eine Reihe von Aktionen zu definieren, die durchgeführt werden sollten, wenn der Benutzer in einer der Zeilen in einer Tabellenansicht _in beide Richtungen bewegt_ wird. Dieses Verhalten ähnelt dem der nativen Mail-app.

Die `UITableViewRowAction` -Klasse wird verwendet, um eine Aktion zu definieren, die ausgeführt wird, wenn der Benutzer in einer Zeile in einer Tabellenansicht horizontal nach links bewegt.
Wenn Sie z. b. eine Tabelle bearbeiten, wird die Schaltfläche **Löschen** standardmäßig in einer Zeile nach links angezeigt. Indem mehrere Instanzen der `UITableViewRowAction` Klasse an einen `UITableView`angefügt werden, können mehrere benutzerdefinierte Aktionen definiert werden, jeweils mit eigenem Text, Formatierung und Verhalten.


## <a name="uiswipeactionsconfiguration"></a>UISwipeActionsConfiguration

Zum Implementieren von wischen-Aktionen mit `UISwipeActionsConfiguration`sind drei Schritte erforderlich:

1. Über `GetLeadingSwipeActionsConfiguration` schreiben Sie die `GetTrailingSwipeActionsConfiguration` Methoden und/oder. Diese Methoden geben einen `UISwipeActionsConfiguration`zurück.
2. Instanziieren Sie `UISwipeActionsConfiguration` die, die zurückgegeben werden soll. Diese Klasse nimmt ein-Array `UIContextualAction`an.
3. Erstellen Sie eine `UIContextualAction`.

Diese werden in den folgenden Abschnitten ausführlicher erläutert.

### <a name="1-implementing-the-swipeactionsconfigurations-methods"></a>1. Implementieren der swipeactionskonfigurationen-Methoden

`UITableViewController`(und auch `UITableViewSource` und `UITableViewDelegate`) enthalten zwei Methoden: `GetLeadingSwipeActionsConfiguration` und `GetTrailingSwipeActionsConfiguration`, die zum Implementieren eines Satzes von Schwenk Aktionen für eine Tabellen Ansichts Zeile verwendet werden. Die führende wischen-Aktion verweist auf einen Schwenk von der linken Seite des Bildschirms in einer Sprache von links nach rechts und von der rechten Seite des Bildschirms in einer Sprache von rechts nach links.

Das folgende Beispiel (aus dem [tableswipeer Actions](https://docs.microsoft.com/samples/xamarin/ios-samples/tableswipeactions) -Beispiel) veranschaulicht die Implementierung der führenden wischen-Konfiguration. Aus den kontextbezogenen Aktionen werden zwei Aktionen erstellt, die nach [folgend](#create-uicontextualaction)erläutert werden. Diese Aktionen werden dann an eine neu initialisierte [`UISwipeActionsConfiguration`](#create-uiswipeactionsconfigurations)weitergegeben, die als Rückgabewert verwendet wird.


```csharp
public override UISwipeActionsConfiguration GetLeadingSwipeActionsConfiguration(UITableView tableView, NSIndexPath indexPath)
{
    //UIContextualActions
    var definitionAction = ContextualDefinitionAction(indexPath.Row);
    var flagAction = ContextualFlagAction(indexPath.Row);

    //UISwipeActionsConfiguration
    var leadingSwipe = UISwipeActionsConfiguration.FromActions(new UIContextualAction[] { flagAction, definitionAction });

    leadingSwipe.PerformsFirstActionWithFullSwipe = false;

    return leadingSwipe;
}
```

<a name="create-uiswipeactionsconfigurations" />

### <a name="2-instantiate-a-uiswipeactionsconfiguration"></a>2. Instanziieren eines`UISwipeActionsConfiguration`

Instanziieren Sie `UISwipeActionsConfiguration` einen, indem `FromActions` Sie die-Methode verwenden, um `UIContextualAction`ein neues Array von s hinzuzufügen, wie im folgenden Code Ausschnitt gezeigt:

```csharp
var leadingSwipe = UISwipeActionsConfiguration.FromActions(new UIContextualAction[] { flagAction, definitionAction })

leadingSwipe.PerformsFirstActionWithFullSwipe = false;
```

Beachten Sie, dass die Reihenfolge, in der die Aktionen angezeigt werden, von der Art und Weise abhängt, wie Sie an das Array übermittelt werden. Der obige Code für führende Streif zeigt z. b. die Aktionen wie folgt an:

![führende Schwenk Aktionen, die für eine Tabellenzeile angezeigt werden](row-action-images/action03.png)

Für nachfolgende Streifen werden die Aktionen wie in der folgenden Abbildung dargestellt angezeigt:

![nachfolgende Schwenk Aktionen, die in einer Tabellenzeile angezeigt werden](row-action-images/action04.png)

In diesem Code Ausschnitt wird auch die neue `PerformsFirstActionWithFullSwipe` -Eigenschaft verwendet. Standardmäßig ist diese Eigenschaft auf true festgelegt, was bedeutet, dass die erste Aktion im Array auftritt, wenn ein Benutzer vollständig in einer Zeile schwimmt. Wenn Sie über eine Aktion verfügen, die nicht destruktiv ist (z. b. "Delete"), ist dies möglicherweise kein ideales Verhalten `false`, und Sie sollten es daher auf festlegen.

<a name="create-uicontextualaction" />

### <a name="create-a-uicontextualaction"></a>Erstellen einer `UIContextualAction`

Die kontextbezogene Aktion besteht darin, dass Sie die Aktion erstellen, die angezeigt wird, wenn der Benutzer eine Tabellenzeile durchläuft.

Um eine Aktion zu initialisieren, müssen Sie `UIContextualActionStyle`, einen Titel und einen `UIContextualActionHandler`bereitstellen. Der `UIContextualActionHandler` übernimmt drei Parameter: eine Aktion, die Ansicht, in der die Aktion angezeigt wurde, und ein Vervollständigungs Handler:

```csharp
public UIContextualAction ContextualFlagAction(int row)
{
    var action = UIContextualAction.FromContextualActionStyle
                    (UIContextualActionStyle.Normal,
                        "Flag",
                        (FlagAction, view, success) => {
                            var alertController = UIAlertController.Create($"Report {words[row]}?", "", UIAlertControllerStyle.Alert);
                            alertController.AddAction(UIAlertAction.Create("Cancel", UIAlertActionStyle.Cancel, null));
                            alertController.AddAction(UIAlertAction.Create("Yes", UIAlertActionStyle.Destructive, null));
                            PresentViewController(alertController, true, null);

                            success(true);
                        });

    action.Image = UIImage.FromFile("feedback.png");
    action.BackgroundColor = UIColor.Blue;

    return action;
}
```

Verschiedene visuelle Eigenschaften, z. b. die Hintergrundfarbe oder das Bild der Aktion, können bearbeitet werden. Der obige Code Ausschnitt veranschaulicht das Hinzufügen eines Bilds zur Aktion und das Festlegen der Hintergrundfarbe auf blau.

Nachdem die kontextbezogenen Aktionen erstellt wurden, können Sie verwenden, um das `UISwipeActionsConfiguration` in der `GetLeadingSwipeActionsConfiguration` -Methode zu initialisieren.

## <a name="uitableviewrowaction"></a>UITableViewRowAction

Um eine oder mehrere benutzerdefinierte Zeilen Aktionen für einen `UITableView`zu definieren, müssen Sie eine Instanz `UITableViewDelegate` der-Klasse erstellen und die `EditActionsForRow` -Methode überschreiben. Beispiel:

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using Foundation;
using UIKit;

namespace BasicTable
{
    public class TableDelegate : UITableViewDelegate
    {
        #region Constructors
        public TableDelegate ()
        {
        }

        public TableDelegate (IntPtr handle) : base (handle)
        {
        }

        public TableDelegate (NSObjectFlag t) : base (t)
        {
        }

        #endregion

        #region Override Methods
        public override UITableViewRowAction[] EditActionsForRow (UITableView tableView, NSIndexPath indexPath)
        {
            UITableViewRowAction hiButton = UITableViewRowAction.Create (
                UITableViewRowActionStyle.Default,
                "Hi",
                delegate {
                    Console.WriteLine ("Hello World!");
                });
            return new UITableViewRowAction[] { hiButton };
        }
        #endregion
    }
}
```

Die statische `UITableViewRowAction.Create` -Methode wird verwendet, um eine `UITableViewRowAction` neue zu erstellen, die eine **Hi** -Schaltfläche anzeigt, wenn der Benutzer in einer Zeile in der Tabelle horizontal nach links bewegt wird. Später `TableDelegate` wird eine neue Instanz von erstellt und an die `UITableView`angefügt. Beispiel:

```csharp
TableDelegate tableDelegate;
...

// Replace the standard delete button with a "Hi" button
tableDelegate = new TableDelegate ();
table.Delegate = tableDelegate;

```

Wenn der obige Code ausgeführt wird und der Benutzer in einer Tabellenzeile nach links drückt, wird die Schaltfläche " **Hi** " anstelle der standardmäßig angezeigten Schaltfläche " **Löschen** " angezeigt:

[![](row-action-images/action01.png "Die angezeigte Schaltfläche \"Hi\" anstelle der Schaltfläche \"Löschen\"")](row-action-images/action01.png#lightbox)

Wenn der Benutzer auf die Schaltfläche " `Hello World!` Hi" tippt, wird in Visual Studio für Mac oder Visual Studio in die Konsole geschrieben, wenn die Anwendung im Debugmodus ausgeführt wird.



## <a name="related-links"></a>Verwandte Links

- [Tableswipeer Actions (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/tableswipeactions)
- [Workingwithtables (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithtables)
