---
title: Arbeiten mit Tabellen und Zellen
description: Anzeigen von Xamarin.iOS UITableView mit Daten
ms.topic: article
ms.prod: xamarin
ms.assetid: 04DF47DD-4E17-75D7-AC7C-8CF4A574CD21
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/06/2016
ms.openlocfilehash: f6abcaa3a771954785df83e80c7e46dd200e6986
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-tables-and-cells"></a>Arbeiten mit Tabellen und Zellen


In diesem Abschnitt stellt die Klassen, die zum Erstellen und Anzeige von Tabellen und enthält Beispiele zur Verwendung in Xamarin.iOS. Es wird behandelt, verwenden die standarddarstellung für Tabellen, Anpassen des Layouts, implementieren bearbeiten und die Verwendung der Xamarin-iOS-Designer eine Tabelle visuell zu entwerfen. In einigen Fällen ist die Anzeige offensichtlich eine Liste von Zeilen (z. B. die Musik-app) und anderen Fällen ist es schwierig zu erkennen, das Table-Steuerelement (z. B. die Bearbeitung in der Kontakte-app oder einer Konversation in Nachrichten-app).

Für diejenigen, die auf plattformübergreifende Anwendungen mit Xamarin.Android arbeiten das UITableView-Steuerelement ist vergleichbar mit dem ListView-Klasse in Android (und die UITableViewSource-Klasse ist ähnlich wie Android des Adapter-Klassen).

In diesen Artikeln dauert einen umfassenden Einblick in die Arbeit mit Tabellen, einschließlich:

-   **Tabelle Teile** – Einführung in und erläutern die visuellen Elemente von der `UITableView` Steuerelement. 
-   **Anzeigen von Daten in Tabellen** – veranschaulichen das Erstellen und Auffüllen einer Tabelle, verwenden unterschiedliche Formate für Tabellen und Zellen und Vermeiden von Arbeitsspeicherproblemen durch Wiederverwenden der Zelle Objekte. 
-   **Erweiterte Verwendung** – Erstellen von benutzerdefinierten Zellen, und verwenden Sie die Bearbeitungsfunktionen der UITableView-Klasse. 
-   **Erstellen einer Tabelle visuell** – mit dem Xamarin-Designer für iOS eine tabellenbasierten-Schnittstelle mit einem Storyboard erstellen. 


## <a name="contents"></a>Inhalt

 [Tabelle Teile &amp; Funktionalität](~/ios/user-interface/controls/tables/table-parts-and-functionality.md)

 [Auffüllen einer Tabelle mit Daten](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)

 [Anpassen der Darstellung einer Tabelle](~/ios/user-interface/controls/tables/customizing-table-appearance.md)

 [Bearbeiten](~/ios/user-interface/controls/tables/editing.md)
 
 [Zeilenaktionen](~/ios/user-interface/controls/tables/row-action.md)

 [Erstellen von Tabellen in einem Storyboard](~/ios/user-interface/controls/tables/creating-tables-in-a-storyboard.md)
 
 [Automatische Größenanpassung Zeilenhöhe](~/ios/user-interface/controls/tables/autosizing-row-height.md)


## <a name="related-links"></a>Verwandte Links

- [WorkingWithTables (Beispiel)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables/)
- [Tabellen in Storyboards (Beispiel)](https://developer.xamarin.com/samples/monotouch/StoryboardTable/)
- [Einführung in die Storyboards](~/ios/user-interface/storyboards/index.md)
- [Storyboard TableView Rezept](https://developer.xamarin.com/recipes/ios/general/storyboard/storyboard_a_tableview)
- [Einführung in MonoTouch.Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [Beispiel für TableEditing auf Github](https://github.com/xamarin/monotouch-samples/tree/master/TableEditing)
- [Beispiel für TableParts auf Github](https://github.com/xamarin/monotouch-samples/tree/master/TableParts)
- [Beispiel für TableAndCellStyles auf Github](https://github.com/xamarin/mobile-samples/tree/master/TablesLists)
- [UITableView-Klassenreferenz](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableView_Class/)
- [UITableViewCell-Klassenreferenz](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewCell_Class/)
- [UITableViewDelegate](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDelegate_Protocol/)
- [UITableViewDataSource](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDataSource_Protocol/)
