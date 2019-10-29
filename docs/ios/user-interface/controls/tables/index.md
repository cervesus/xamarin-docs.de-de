---
title: Arbeiten mit Tabellen und Zellen in xamarin. IOS
description: Dieses Dokument verweist auf verschiedene Anleitungen, in denen beschrieben wird, wie Daten mit dem uitableview-Steuerelement in einer xamarin. IOS-App angezeigt werden.
ms.prod: xamarin
ms.assetid: 04DF47DD-4E17-75D7-AC7C-8CF4A574CD21
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 01/06/2016
ms.openlocfilehash: 7f5af84a8dfb9f774822e28e50cf8bbca9acf94b
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021888"
---
# <a name="working-with-tables-and-cells-in-xamarinios"></a>Arbeiten mit Tabellen und Zellen in xamarin. IOS

In diesem Abschnitt werden die Klassen vorgestellt, die zum Erstellen und Anzeigen von Tabellen verwendet werden, und es werden Beispiele für die Verwendung in xamarin. IOS bereitgestellt. Dabei wird die Standarddarstellung für Tabellen, das Anpassen des Layouts, das Implementieren der Bearbeitung und die Verwendung des xamarin IOS-Designers zum visuellen Entwerfen einer Tabelle behandelt. Manchmal ist die Anzeige offensichtlich eine Liste von Zeilen (z. b. die Musik-app), und in anderen Fällen ist es schwierig, das Tabellen Steuerelement zu erkennen (z. b. die Bearbeitung in der App "Kontakte" oder eine Konversation in der Nachrichten-

Bei der Verwendung von plattformübergreifenden Anwendungen mit xamarin. Android ähnelt das uitableview-Steuerelement der ListView-Klasse in Android (und die uitableviewsource-Klasse ähnelt den Adapter Klassen von Android).

Diese Artikel machen einen umfassenden Einblick in die Arbeit mit Tabellen, einschließlich der folgenden:

- **Tabellen Teile** – Einführung und Erläuterung der visuellen Elemente des Steuer Elements `UITableView`. 
- **Anzeigen von Daten in Tabellen** – veranschaulichen, wie Sie eine Tabelle erstellen und Auffüllen, verschiedene Tabellen-und Zell Stile verwenden und Speicherprobleme vermeiden, indem Sie Zellen Objekte wieder verwenden. 
- **Erweiterte Verwendung** – das Entwickeln von benutzerdefinierten Zellen und das Verwenden der Bearbeitungsfunktionen der uitableview-Klasse. 
- **Visuelles Erstellen einer Tabelle** – mithilfe der Xamarin Designer für IOS, um eine Tabellen gesteuerte Schnittstelle mit einem Storyboard zu erstellen. 

## <a name="contents"></a>Inhalt

 [Tabellen Teile &amp;-Funktionalität](~/ios/user-interface/controls/tables/table-parts-and-functionality.md)

 [Auffüllen einer Tabelle mit Daten](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)

 [Anpassen der Darstellung einer Tabelle](~/ios/user-interface/controls/tables/customizing-table-appearance.md)

 [Bearbeiten](~/ios/user-interface/controls/tables/editing.md)

 [Zeilen Aktionen](~/ios/user-interface/controls/tables/row-action.md)

 [Erstellen von Tabellen in einem Storyboard](~/ios/user-interface/controls/tables/creating-tables-in-a-storyboard.md)

 [Automatische Größenanpassung der Zeilenhöhe](~/ios/user-interface/controls/tables/autosizing-row-height.md)

## <a name="related-links"></a>Verwandte Links

- [Workingwithtables (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithtables)
- [Tabellen in Storyboards (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/storyboardtable)
- [Einführung in Storyboards](~/ios/user-interface/storyboards/index.md)
- [Storyboard für eine TableView-Anleitung](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/storyboard/storyboard_a_tableview)
- [Einführung in MonoTouch. Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [Tableediting-Beispiel auf GitHub](https://github.com/xamarin/monotouch-samples/tree/master/TableEditing)
- [Tableparts-Beispiel auf GitHub](https://github.com/xamarin/monotouch-samples/tree/master/TableParts)
- [Tableandcellstyles-Beispiel auf GitHub](https://github.com/xamarin/mobile-samples/tree/master/TablesLists)
- [Uitableview-Klassenreferenz](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableView_Class/)
- [Uitableviewcell-Klassenreferenz](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewCell_Class/)
- [Uitableviewdelegat](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDelegate_Protocol/)
- [Uitableviewdatasource](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDataSource_Protocol/)
