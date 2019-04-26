---
title: Arbeiten mit Zeilenaktionen in Xamarin.iOS
description: Diese Anleitung veranschaulicht, wie Sie benutzerdefinierte Wischen Aktionen für die Tabellenzeilen mit UISwipeActionsConfiguration oder UITableViewRowAction erstellen
ms.prod: xamarin
ms.assetid: 340FB633-0C46-40AA-9963-FF17D7CA6858
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/25/2017
ms.openlocfilehash: 6d41f37d4a63db710bb04e35e6e1a4be0dd4f7a4
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61408280"
---
# <a name="working-with-row-actions-in-xamarinios"></a>Arbeiten mit Zeilenaktionen in Xamarin.iOS

_Diese Anleitung veranschaulicht, wie Sie benutzerdefinierte Wischen Aktionen für die Tabellenzeilen mit UISwipeActionsConfiguration oder UITableViewRowAction erstellen_

![Demonstration Wischen Aktionen für Zeilen](row-action-images/action02.png)

iOS bietet zwei Möglichkeiten zum Ausführen von Aktionen für eine Tabelle: `UISwipeActionsConfiguration` und `UITableViewRowAction`.

`UISwipeActionsConfiguration` wurde in iOS 11 eingeführt und wird verwendet, um einen Satz von definieren Aktionen ausführen, sollte statt, wenn der Benutzer Kundenkarte _in beide Richtungen_ für eine Zeile in einer Tabellenansicht. Dieses Verhalten ist vergleichbar mit der systemeigenen Mail.app 

Die `UITableViewRowAction` Klasse wird verwendet, um eine Aktion definieren, die stattfinden soll, wenn der Benutzer Kundenkarte horizontal auf eine Zeile in einer Tabellenansicht bleibt.
Beim Bearbeiten einer Tabelle, Wischen auf eine Zeile nach links zeigt beispielsweise eine **löschen** Schaltfläche standardmäßig. Durch Anfügen von mehreren Instanzen von der `UITableViewRowAction` -Klasse eine `UITableView`, mehrere benutzerdefinierte Aktionen definiert werden können, jeweils einen eigenen Text, Formatierung und Verhalten.


## <a name="uiswipeactionsconfiguration"></a>UISwipeActionsConfiguration

Eine drei Schritte sind erforderlich, um streichen Sie nach Aktionen mit implementieren `UISwipeActionsConfiguration`:

1. Außer Kraft setzen `GetLeadingSwipeActionsConfiguration` und/oder `GetTrailingSwipeActionsConfiguration` Methoden. Diese Methoden zurück, eine `UISwipeActionsConfiguration`. 
2. Instanziieren der `UISwipeActionsConfiguration` zurückgegeben werden. Diese Klasse wird ein Array von `UIContextualAction`.
3. Erstellen Sie eine `UIContextualAction`.

Diese werden in den folgenden Abschnitten ausführlicher erläutert.

### <a name="1-implementing-the-swipeactionsconfigurations-methods"></a>1. Implementieren der SwipeActionsConfigurations-Methoden

`UITableViewController` (und auch `UITableViewSource` und `UITableViewDelegate`) enthält zwei Methoden: `GetLeadingSwipeActionsConfiguration` und `GetTrailingSwipeActionsConfiguration`, mit denen eine Reihe von Wischen Aktionen auf eine Zeile einer Tabelle zu implementieren. Die führenden Wischen-Aktion verweist auf ein Wischen nach, von der linken Seite des Bildschirms in einer links-nach-rechts-Sprache und von der rechten Seite des Bildschirms in einer rechts-nach-links-Sprache. 

Im folgenden Beispiel (aus der [TableSwipeActions](https://developer.xamarin.com/samples/monotouch/TableSwipeActions) Beispiel) demonstriert die Implementierung der führenden Wischen-Konfigurations. Zwei Aktionen werden erstellt, der kontextbezogenen Aktionen, die ausführlich erläutert werden [unten](#create-uicontextualaction). Diese Aktionen werden dann an eine neu initialisierte übergeben [ `UISwipeActionsConfiguration` ](#create-uiswipeactionsconfigurations), die als Rückgabewert verwendet wird.


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

### <a name="2-instantiate-a-uiswipeactionsconfiguration"></a>2. Instanziieren Sie ein `UISwipeActionsConfiguration`

Instanziieren einer `UISwipeActionsConfiguration` mithilfe der `FromActions` Methode, um ein neues Array vom hinzuzufügen `UIContextualAction`s, wie im folgenden Codeausschnitt gezeigt:

```csharp
var leadingSwipe = UISwipeActionsConfiguration.FromActions(new UIContextualAction[] { flagAction, definitionAction })

leadingSwipe.PerformsFirstActionWithFullSwipe = false;
```

Es ist wichtig zu beachten, dass die Reihenfolge, in der sich Ihre Aktionen anzeigen, abhängig, wie sie in Ihrem Array übergeben werden. Beispielsweise zeigt den Code oben führende Kundenkarte wie also die Aktionen aus:

![führende Wischen Aktionen angezeigt, auf eine Zeile einer Tabelle](row-action-images/action03.png)

Für nachfolgende Kundenkarte, werden die Aktionen angezeigt werden, wie in der folgenden Abbildung dargestellt:

![nachfolgende Wischen Aktionen angezeigt, auf eine Zeile einer Tabelle](row-action-images/action04.png)

Dieser Codeausschnitt verwendet auch das neue `PerformsFirstActionWithFullSwipe` Eigenschaft. Standardmäßig ist diese Eigenschaft auf festgelegt "true", was bedeutet, dass die erste Aktion in das Array, wenn ein Benutzer vollständig in einer Zeile mit einer wischbewegung erfolgt. Wenn Sie eine Aktion verfügen, die nicht destruktiv ist (z. B. "löschen", dies möglicherweise nicht die ideale Verhalten und legen sie daher auf `false`.

<a name="create-uicontextualaction" />

### <a name="create-a-uicontextualaction"></a>Erstellen einer `UIContextualAction`

Die kontextbezogene Aktion ist, in dem Sie tatsächlich die Aktion erstellt, die angezeigt wird, wenn der Benutzer eine Zeile einer Tabelle mit einer wischbewegung.

Um eine Aktion zu initialisieren, müssen Sie angeben, einer `UIContextualActionStyle`, einen Titel und einen `UIContextualActionHandler`. Die `UIContextualActionHandler` akzeptiert drei Parameter: eine Aktion, die an, die die Aktion im angezeigt wurde und einen Abschlusshandler:

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

Unterschiedliche visuelle Eigenschaften, z. B. die Hintergrundfarbe oder ein Bild der Aktion können bearbeitet werden. Der obige Codeausschnitt veranschaulicht, Hinzufügen eines Bilds an die Aktion und Festlegen der Hintergrundfarbe in Blau.

Nachdem der kontextbezogenen Aktionen erstellt wurden, können sie Sie initialisieren den `UISwipeActionsConfiguration` in die `GetLeadingSwipeActionsConfiguration` Methode.

## <a name="uitableviewrowaction"></a>UITableViewRowAction

So definieren Sie eine oder mehrere benutzerdefinierte Zeilenaktionen für eine `UITableView`, Sie benötigen zum Erstellen einer Instanz von der `UITableViewDelegate` Klasse, und überschreiben die `EditActionsForRow` Methode. Zum Beispiel:

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

Die statische `UITableViewRowAction.Create` Methode dient zum Erstellen eines neuen `UITableViewRowAction` zeigt, die eine **Hi** -Schaltfläche, wenn der Benutzer Kundenkarte horizontal auf eine Zeile in der Tabelle nach links. Später eine neue Instanz der dem `TableDelegate` erstellt und angefügt ist die `UITableView`. Zum Beispiel:

```csharp
TableDelegate tableDelegate;
...

// Replace the standard delete button with a "Hi" button
tableDelegate = new TableDelegate ();
table.Delegate = tableDelegate;

```

Wenn der obige Code ausgeführt wird, und die Benutzer-Kundenkarte auf die Zeile einer Tabelle links, die **Hi** Schaltfläche nicht angezeigt wird der **löschen** Schaltfläche, die standardmäßig angezeigt wird:

[![](row-action-images/action01.png "Die Schaltfläche \"Hi\" angezeigt wird, anstatt die Schaltfläche \"löschen\"")](row-action-images/action01.png#lightbox)

Wenn der Benutzer tippt der **Hi** Schaltfläche `Hello World!` geschrieben wird an die Konsole in Visual Studio für Mac oder Visual Studio, wenn die Anwendung im Debugmodus ausgeführt wird.



## <a name="related-links"></a>Verwandte Links

- [TableSwipeActions (Beispiel)](https://developer.xamarin.com/samples/monotouch/TableSwipeActions)
- [WorkingWithTables (Beispiel)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
