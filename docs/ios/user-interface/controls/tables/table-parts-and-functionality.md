---
title: Teile der Tabelle und die Funktionalität in Xamarin.iOS
description: Dieses Dokument beschreibt die verschiedenen Komponenten von einer UITableView in iOS. Es wird erläutert, Abschnittsnamen, Zellen, Abschnitt Fußzeilen, den Index und Bearbeitungsmodus.
ms.prod: xamarin
ms.assetid: B4139C8B-28F2-4C0F-297F-BF5432C5A915
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: c7c9f810798c3d02078b48e17a2ab951cb36aa12
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789875"
---
# <a name="table-parts-and-functionality-in-xamarinios"></a>Teile der Tabelle und die Funktionalität in Xamarin.iOS

Eine UITableView kann einen Stil "gruppierten" oder "einfachen", und besteht aus folgenden Teilen:

-  [Überschrift des Abschnitts](#Section_Header)
-  [Zellen](#Cells) (oder Zeilen, falls gewünscht)
-  [Abschnitt Fußzeile](#Section_Footer)
-  [Index](#Index)
-  [Bearbeitungsmodus](#Edit_Features) ("Navigieren Sie zum Löschen" enthält, und ziehen Sie die Handles zum Ändern der Reihenfolge der Zeilen) 

Diese Screenshots zeigen, wie der Abschnitt Zeilen, Kopfzeilen, Fußzeilen, Edit-Steuerelemente und den Index angezeigt werden.

 [![](table-parts-and-functionality-images/image1a.png "Diese Screenshots zeigen, wie der Abschnitt Zeilen, Kopfzeilen, Fußzeilen, Edit-Steuerelemente und den Index angezeigt werden")](table-parts-and-functionality-images/image1a.png#lightbox)

Diese Teile werden im folgenden ausführlicher beschrieben:

<a name="Section_Header" />

## <a name="section-header"></a>Überschrift des Abschnitts

Zellen können optional in Abschnitte unterteilt, mit der Bezeichnung und einen benutzerdefinierten Header und/oder mit einer Fußzeile versehen werden. Der Header kann mit einem Zeichenfolgenwert festgelegt werden, oder eine benutzerdefinierte Ansicht bereitgestellt werden kann, um ein anderes Layout oder Stil zu ermöglichen.

<a name="Cells" />

## <a name="cells"></a>Zellen

Zellen, die hauptbenutzeroberflächenelement für eine Tabelle. Bei korrekter Implementierung sind die Zellen für Speichereffizienz erneut verwendet. Es gibt vier integrierte Zellenstilen und können Ihre eigenen benutzerdefinierten Zellen – entweder im Code oder im Designer bei Verwendung von Storyboards.

<a name="Section_Footer"/>

## <a name="section-footer"></a>Abschnitt Fußzeile

Optionaler Abschnitt Fußzeile kann mit einem Zeichenfolgenwert festgelegt werden, oder eine benutzerdefinierte Ansicht bereitgestellt werden kann, um ein anderes Layout oder Stil zu ermöglichen. Kopf- und Fußzeilen können unabhängig voneinander festgelegt werden.

<a name="Index" />

## <a name="index"></a>Index

Der Index wird als Farbstreifen Zeichen nach dem rechten Rand der Tabelle unten angezeigt.
Durch berühren klicken oder ziehen für den Index beschleunigt Bildlauf zu, dass ein Teil der Tabelle. Ein Index ist optional, aber es wird empfohlen, Sie können lange Listen zu navigieren. Ein Index wird nicht in der Regel mit dem gruppierten-Format verwendet.

<a name="Edit_Features" />

## <a name="editing-mode"></a>Bearbeitungsmodus

Es gibt verschiedene andere-Bearbeitungsfeatures stehen zur Verfügung:

- Streichen Sie nach, um einzelne Zellen zu löschen.
- In den Bearbeitungsmodus wechselt, um Delete-Schaltflächen in jeder Zeile anzuzeigen. 
- In den Bearbeitungsmodus wechselt, neu anordnen Handles offengelegt. 
- Einfügen von neuen Zellen (mit Animation).

Im weiteren Verlauf dieses Dokuments veranschaulicht das Implementieren dieser UITableView-Funktionen mit Xamarin.iOS.


## <a name="classes-overview"></a>Übersicht über Klassen

Die primären Klassen verwendet, um die in den Tabellenansichten angezeigt werden hier angezeigt:

[![](table-parts-and-functionality-images/classdiagram.png "Die primären Klassen verwendet, um die in den Tabellenansichten angezeigt werden hier angezeigt.")](table-parts-and-functionality-images/classdiagram.png#lightbox)

Der Zweck der einzelnen Klassen wird im folgenden beschrieben:

- **UITableView** – eine Sicht, die eine Auflistung von Zellen innerhalb eines fortlaufenden Containers enthält. Tabellenansicht in der Regel verwendet den gesamten Bildschirm in einer iPhone-app aber möglicherweise als Teil einer größeren Ansicht auf dem iPad vorhanden sein (oder in einem Popover angezeigt). 
- **UITableViewCell** – eine Sicht, die eine einzelne Zelle (oder Zeile) in einer Tabelle darstellen. Es gibt vier Arten von integrierten Zelle, und es ist möglich, benutzerdefinierte Zellen sowohl in c# oder mit iOS-Designer erstellen. 
- **UITableViewSource** – Xamarin.iOS ausschließliche abstrakte Klasse, die die Methoden, die erforderlich sind, zum Anzeigen einer Tabelle, einschließlich der Anzahl der Zeilen, eine Ansicht der Zelle für jede Zeile zurückgeben, Behandeln von Zeilenauswahl und viele weitere optionale Funktionen bereitstellt. Sie *müssen* Unterklasse diese Option, um eine funktionierende UITableView abrufen. 
- **NSIndexPath** – Contains Zeilen- und Abschnitt Eigenschaften, die die Position einer Zelle in einer Tabelle eindeutig identifizieren. 
- **UITableViewController** – eine sofort verwendbare UIViewController, die eine UITableView hartcodiert als Ansicht und zugegriffen werden kann, über die TableView-Eigenschaft verfügt. 
- **UIViewController** – Wenn die Tabelle nicht den gesamten Bildschirm Sie hinzufügen belegen können, eine UITableView auf alle UIViewController, mit deren Rahmen entsprechend festgelegt. 

UITableViewSource ersetzt die folgenden zwei Klassen, die noch in Xamarin.iOS verfügbar sind, jedoch sind nicht erforderlich:

- **UITableViewDataSource** – ein Objective-C-Protokoll, das im Xamarin.iOS als abstrakte Klasse modelliert wurden. Muss als Unterklasse werden, um eine Tabelle mit einer Ansicht für jede Zelle sowie Informationen über Kopfzeilen, Fußzeilen und die Anzahl der Zeilen und Abschnitte in der Tabelle bereitzustellen. 
- **UITableViewDelegate** – ein Objective-C-Protokoll, das im Xamarin.iOS als Klasse modelliert wurden. Verarbeitet die Auswahl, Funktionen und andere optionale Tabellenfunktionen bearbeiten. 

In diesem Dokument werden die Beispiele alle UITableViewSource verwenden und ignorieren diese beiden Klassen. Sie werden hier erwähnt, da alle Objective-C-Beispiele finden Sie in der Apple-Dokumentation, verwiesen werden, daher ist es hilfreich zu verstehen, was diese tun (und, dass stattdessen der Xamarin.iOS UITableViewSource verwendet werden können).

## <a name="related-links"></a>Verwandte Links

- [WorkingWithTables (Beispiel)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
