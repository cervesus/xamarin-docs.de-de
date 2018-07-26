---
title: Arbeiten mit Tabellen und Zellen in Xamarin.iOS
description: Dieses Dokument enthält Links zu verschiedenen Anleitungen, die beschreiben, wie Daten mit dem UITableView-Steuerelement in einer Xamarin.iOS-app angezeigt.
ms.prod: xamarin
ms.assetid: 04DF47DD-4E17-75D7-AC7C-8CF4A574CD21
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/06/2016
ms.openlocfilehash: ea5b6ba532d577bd503529065eef803acf3a7aa9
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241697"
---
# <a name="working-with-tables-and-cells-in-xamarinios"></a>Arbeiten mit Tabellen und Zellen in Xamarin.iOS

In diesem Abschnitt werden die Klassen, die zum Erstellen und zeigen Sie Tabellen vorgestellt, und klicken Sie dann enthält Beispiele dafür, wie Sie diese in Xamarin.iOS verwenden. Es wird behandelt, verwenden die standarddarstellung für Tabellen, Anpassen des Layouts, das Implementieren von bearbeiten und die Verwendung der Xamarin.IOS-Designer, um eine Tabelle visuell zu entwerfen. Manchmal ist die Anzeige offensichtlich eine Liste von Zeilen (z. B. die Musik-app) und in anderen Situationen ist es schwer zu erkennen, das Table-Steuerelement (z. B. die Bearbeitung in die Kontakte-app oder einer Konversation in der Nachrichten-app).

Für alle Arbeiten an plattformübergreifenden Anwendungen, die mit Xamarin.Android das UITableView-Steuerelement ist vergleichbar mit der Klasse "ListView" unter Android (und die UITableViewSource-Klasse ist vergleichbar mit der Android-Adapterklassen).

Diese Artikel nehmen einen umfassenden Überblick über das Arbeiten mit Tabellen, einschließlich:

-   **Tabelle Teile** – Einführung in und erläutert die visuellen Elemente der `UITableView` Steuerelement. 
-   **Anzeigen von Daten in Tabellen** – veranschaulicht, wie zum Erstellen und Auffüllen einer Tabelle, verwenden Sie verschiedene Formate für Tabellen und Zellen und Speicherproblemen zu vermeiden, indem Sie die Wiederverwendung von Cell-Objekte. 
-   **Erweiterte Verwendung** – Erstellen von benutzerdefinierten Zellen und verwenden Sie die Bearbeitungsfunktionen der UITableView-Klasse. 
-   **Erstellen einer Tabelle visuell** – mit dem Xamarin-Designer für iOS eine tabellenbasierten-Schnittstelle mit einem Storyboard zu erstellen. 

## <a name="contents"></a>Inhalt

 [Tabelle Teile &amp; Funktionen](~/ios/user-interface/controls/tables/table-parts-and-functionality.md)

 [Auffüllen einer Tabelle mit Daten](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)

 [Anpassen der Darstellung einer Tabelle](~/ios/user-interface/controls/tables/customizing-table-appearance.md)

 [Bearbeiten](~/ios/user-interface/controls/tables/editing.md)
 
 [Zeilenaktionen](~/ios/user-interface/controls/tables/row-action.md)

 [Erstellen von Tabellen in einem Storyboard](~/ios/user-interface/controls/tables/creating-tables-in-a-storyboard.md)
 
 [Automatische Größenanpassung der Zeilenhöhe](~/ios/user-interface/controls/tables/autosizing-row-height.md)

## <a name="related-links"></a>Verwandte Links

- [WorkingWithTables (Beispiel)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables/)
- [Tabellen in Storyboards (Beispiel)](https://developer.xamarin.com/samples/monotouch/StoryboardTable/)
- [Einführung in Storyboards](~/ios/user-interface/storyboards/index.md)
- [Eine Anleitung TableView Storyboard](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/storyboard/storyboard_a_tableview)
- [Einführung in die MonoTouch.Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [TableEditing-Beispiel auf Github](https://github.com/xamarin/monotouch-samples/tree/master/TableEditing)
- [TableParts-Beispiel auf Github](https://github.com/xamarin/monotouch-samples/tree/master/TableParts)
- [TableAndCellStyles-Beispiel auf Github](https://github.com/xamarin/mobile-samples/tree/master/TablesLists)
- [UITableView-Klassenreferenz](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableView_Class/)
- [UITableViewCell-Klassenreferenz](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewCell_Class/)
- [UITableViewDelegate](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDelegate_Protocol/)
- [UITableViewDataSource](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDataSource_Protocol/)
