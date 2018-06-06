---
title: Arbeiten mit Zeilenaktionen in Xamarin.iOS
description: Dieses Handbuch veranschaulicht, wie benutzerdefinierte Wischen Aktionen für die Tabellenzeilen mit UISwipeActionsConfiguration oder UITableViewRowAction
ms.prod: xamarin
ms.assetid: 340FB633-0C46-40AA-9963-FF17D7CA6858
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/25/2017
ms.openlocfilehash: 4be8b6dc66c9c047e6662067e7e3ecf81ab22893
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789940"
---
# <a name="working-with-row-actions-in-xamarinios"></a>Arbeiten mit Zeilenaktionen in Xamarin.iOS

_Dieses Handbuch veranschaulicht, wie benutzerdefinierte Wischen Aktionen für die Tabellenzeilen mit UISwipeActionsConfiguration oder UITableViewRowAction_

![Demonstrieren der Wischen Aktionen in Zeilen](row-action-images/action02.png)

iOS bietet zwei Möglichkeiten zum Ausführen von Aktionen für eine Tabelle: `UISwipeActionsConfiguration` und `UITableViewRowAction`.

`UISwipeActionsConfiguration` wurde in iOS 11 eingeführt und wird verwendet, um einen Satz von definieren, die auszuführenden Aktionen vorhanden, wenn der Benutzer Kundenkarte _in beide Richtungen_ auf eine Zeile in einer Tabelle. Dieses Verhalten ähnelt dem systemeigenen Mail.app 

Die `UITableViewRowAction` Klasse wird verwendet, um eine Aktion zu definieren, die ausgeführt wird, wenn der Benutzer Kundenkarte horizontal auf eine Zeile in einer Tabelle nach links.
Zeigt z. B. wenn eine Tabelle, ein Lesegerät Links in einer Zeile Bearbeiten einer **löschen** Schaltfläche standardmäßig. Durch das Anfügen von mehreren Instanzen von der `UITableViewRowAction` Klasse, um eine `UITableView`, mehrere benutzerdefinierte Aktionen definiert werden können, jeweils über einen eigenen Text, Formatierung und Verhalten.


## <a name="uiswipeactionsconfiguration"></a>UISwipeActionsConfiguration

Ein drei Schritte sind erforderlich, um Wischen Aktionen mit implementieren `UISwipeActionsConfiguration`:

1. Überschreiben Sie `GetLeadingSwipeActionsConfiguration` und/oder `GetTrailingSwipeActionsConfiguration` Methoden. Diese Methoden geben ein `UISwipeActionsConfiguration`. 
2. Instanziieren der `UISwipeActionsConfiguration` zurückgegeben werden. Diese Klasse wird ein Array von `UIContextualAction`.
3. Erstellen Sie eine `UIContextualAction`.

Diese werden in den folgenden Abschnitten ausführlicher erläutert.

### <a name="1-implementing-the-swipeactionsconfigurations-methods"></a>1. Implementieren der SwipeActionsConfigurations-Methoden

`UITableViewController` (und auch `UITableViewSource` und `UITableViewDelegate`) enthält zwei Methoden: `GetLeadingSwipeActionsConfiguration` und `GetTrailingSwipeActionsConfiguration`, werden verwendet, um eine Reihe von Aktionen Wischen auf eine Zeile einer Tabelle zu implementieren. Die Aktion für führende Wischen verweist auf ein Streifen, aus der linken Seite des Bildschirms in einer Sprache von links nach rechts und von der rechten Seite des Bildschirms in einer rechts-nach-links-Sprache. 

Im folgenden Beispiel (aus der [TableSwipeActions](https://developer.xamarin.com/samples/monotouch/TableSwipeActions) Beispiel) wird die Implementierung der führenden Wischen Konfigurations veranschaulicht. Zwei Aktionen werden erstellt, der kontextbezogenen Aktionen, die ausführlich erläutert werden [unten](#create-uicontextualaction). Diese Aktionen werden dann an eine neu initialisierte übergeben [ `UISwipeActionsConfiguration` ](#create-uiswipeactionsconfigurations), der als Rückgabewert verwendet wird.


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

### <a name="2-instantiate-a-uiswipeactionsconfiguration"></a>2. Instanziieren einer `UISwipeActionsConfiguration`

Instanziieren einer `UISwipeActionsConfiguration` mithilfe der `FromActions` Methode, um ein neues Array mit hinzuzufügen `UIContextualAction`s, wie im folgenden Codeausschnitt gezeigt:

```csharp
var leadingSwipe = UISwipeActionsConfiguration.FromActions(new UIContextualAction[] { flagAction, definitionAction })

leadingSwipe.PerformsFirstActionWithFullSwipe = false;
```

Es ist wichtig zu beachten, dass die Reihenfolge, in der Ihre Aktionen anzuzeigen, ist davon abhängig, wie sie in das Array übergeben werden. Beispielsweise zeigt der Code oben für führende Kundenkarte als die Aktionen aus:

![führende Wischen Aktionen auf eine Tabellenzeile angezeigt](row-action-images/action03.png)

Für nachfolgende Kundenkarte, werden die Aktionen angezeigt werden, wie in der folgenden Abbildung dargestellt:

![nachfolgende Wischen Aktionen auf eine Tabellenzeile angezeigt](row-action-images/action04.png)

Dieser Codeausschnitt macht auch verwenden, der neuen `PerformsFirstActionWithFullSwipe` Eigenschaft. Standardmäßig ist diese Eigenschaft festgelegt, auf "true", was bedeutet, dass die erste Aktion im Array ausgeführt wird, wenn ein Benutzer vollständig in einer Zeile Kundenkarte. Wenn Sie eine Aktion nicht destruktiver ist (z. B. "Delete", möglicherweise nicht ideal Verhalten, und Sie sollten daher legen Sie sie auf `false`.

<a name="create-uicontextualaction" />

### <a name="create-a-uicontextualaction"></a>Erstellen einer `UIContextualAction`

Die kontextabhängige Aktion ist, in dem Sie tatsächlich die Aktion erstellt haben, die angezeigt wird, wenn der Benutzer eine Tabellenzeile swipes.

Um eine Aktion zu initialisieren, müssen Sie angeben, einer `UIContextualActionStyle`, einen Titel und eine `UIContextualActionHandler`. Die `UIContextualActionHandler` akzeptiert drei Parameter: eine Aktion, die Sicht, die die Aktion angezeigt wurde und eine Abschlusshandler:

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

Unterschiedliche visuelle Eigenschaften, z. B. die Hintergrundfarbe oder ein Bild der Aktion können bearbeitet werden. Der obige Codeausschnitt wird veranschaulicht, Hinzufügen eines Bilds an die Aktion und Festlegen der Hintergrundfarbe für Blau.

Nach der kontextbezogenen Aktionen erstellt wurden, können sie zum Initialisieren der `UISwipeActionsConfiguration` in die `GetLeadingSwipeActionsConfiguration` Methode.

## <a name="uitableviewrowaction"></a>UITableViewRowAction

So definieren Sie eine oder mehrere benutzerdefinierte Zeilenaktionen für eine `UITableView`, müssen Sie zum Erstellen einer Instanz von der `UITableViewDelegate` Klasse, und überschreiben die `EditActionsForRow` Methode. Zum Beispiel:

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

Die statische `UITableViewRowAction.Create` Methode dient zum Erstellen eines neuen `UITableViewRowAction` , die zeigt eine **Hi** -Schaltfläche, wenn der Benutzer Kundenkarte horizontal auf eine Zeile in der Tabelle nach links. Später eine neue Instanz der dem `TableDelegate` erstellt und angefügt, um die `UITableView`. Zum Beispiel:

```csharp
TableDelegate tableDelegate;
...

// Replace the standard delete button with a "Hi" button
tableDelegate = new TableDelegate ();
table.Delegate = tableDelegate;

```

Wenn der obige Code ausgeführt wird und der Benutzer Kundenkarte auf eine Tabellenzeile Links der **Hi** Schaltfläche wird angezeigt, statt die **löschen** Schaltfläche, die standardmäßig angezeigt wird:

[![](row-action-images/action01.png "Die Schaltfläche \"Hi\" angezeigt wird, anstatt die Schaltfläche \"löschen\"")](row-action-images/action01.png#lightbox)

Wenn der Benutzer tippt der **Hi** Schaltfläche `Hello World!` geschrieben wird an die Konsole in Visual Studio für Mac oder Visual Studio, wenn die Anwendung im Debugmodus ausgeführt wird.



## <a name="related-links"></a>Verwandte Links

- [TableSwipeActions (Beispiel)](https://developer.xamarin.com/samples/monotouch/TableSwipeActions)
- [WorkingWithTables (Beispiel)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
