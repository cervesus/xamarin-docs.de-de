---
title: Tabellenkomponenten und Funktionen in Xamarin.iOS
description: Dieses Dokument beschreibt die verschiedenen Teile einer UITableView in iOS. Es wird erläutert, Abschnittsheadern, Zellen, Abschnitt Fußzeilen, den Index und Bearbeitungsmodus.
ms.prod: xamarin
ms.assetid: B4139C8B-28F2-4C0F-297F-BF5432C5A915
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: c4d788cce12a9aabdd1170cd1a52915f3b30285f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61200510"
---
# <a name="table-parts-and-functionality-in-xamarinios"></a>Tabellenkomponenten und Funktionen in Xamarin.iOS

Eine UITableView kann einen Stil "gruppierten" oder "einfachen", und besteht aus den folgenden Teilen:

-  [Überschrift des Abschnitts](#Section_Header)
-  [Zellen](#Cells) (oder Zeilen, falls gewünscht)
-  [Abschnitt Fußzeile](#Section_Footer)
-  [Index](#Index)
-  [Im Bearbeitungsmodus](#Edit_Features) (enthält "Navigieren Sie zum Löschen", und ziehen Sie Handles zum Ändern der Reihenfolge der Zeilen) 

Diese Screenshots zeigen, wie der Abschnitt Zeilen, Kopfzeilen, Fußzeilen, Edit-Steuerelemente und den Index angezeigt werden.

 [![](table-parts-and-functionality-images/image1a.png "Diese Screenshots zeigen, wie der Abschnitt Zeilen, Kopfzeilen, Fußzeilen, Edit-Steuerelemente und den Index angezeigt werden")](table-parts-and-functionality-images/image1a.png#lightbox)

Diese Teile werden im folgenden ausführlicher beschrieben:

<a name="Section_Header" />

## <a name="section-header"></a>Überschrift des Abschnitts

Zellen können optional in Abschnitte gruppiert, einen benutzerdefinierten Header mit der Bezeichnung und/oder mit einer Fußzeile bezeichnet werden. Der Header kann mit einem Zeichenfolgenwert festgelegt werden, oder eine benutzerdefinierte Ansicht kann angegeben werden, um ein anderes Layout oder Stil zu ermöglichen.

<a name="Cells" />

## <a name="cells"></a>Zellen

Zellen, die das hauptbenutzeroberflächenelement für eine Tabelle. Bei korrekter Implementierung ist, sind die Zellen für die Speichereffizienz erneut verwendet. Es gibt vier integrierte Zellstile, und Sie können erstellen Ihre eigenen benutzerdefinierten Zellen – entweder im Code oder im Designer bei Verwendung von Storyboards.

<a name="Section_Footer"/>

## <a name="section-footer"></a>Abschnitt Fußzeile

Die optionalen Abschnitt Fußzeile kann mit einem Zeichenfolgenwert festgelegt werden, oder eine benutzerdefinierte Ansicht kann angegeben werden, um ein anderes Layout oder Stil zu ermöglichen. Abschnitt Kopf- und Fußzeilen können unabhängig voneinander festgelegt werden.

<a name="Index" />

## <a name="index"></a>Index

Der Index wird als Farbstreifen Zeichen nach dem rechten Rand der Tabelle unten angezeigt.
Berühren, oder ziehen für den Index beschleunigt, scrollen zu diesem Teil der Tabelle. Ein Index ist optional, aber es wird empfohlen, um lange Listen zu navigieren. Ein Index wird nicht in der Regel in dem gruppierte-Format verwendet.

<a name="Edit_Features" />

## <a name="editing-mode"></a>Bearbeitungsmodus

Ein paar andere-Bearbeitungsfeatures stehen zur Verfügung:

- Streichen Sie nach, um einzelne Zellen zu löschen.
- In den Bearbeitungsmodus wechselt, um Schaltflächen zum Löschen in jeder Zeile anzuzeigen. 
- In den Bearbeitungsmodus wechselt, um neu sortieren Handles anzuzeigen. 
- Einfügen von neuen Zellen (mit Animation).

Im weiteren Verlauf dieses Dokuments wird gezeigt, wie alle diese UITableView-Features mit Xamarin.iOS implementiert wird.


## <a name="classes-overview"></a>Übersicht über Klassen

Die primären Klassen verwendet, um Tabellenansichten anzuzeigen, werden hier angezeigt:

[![](table-parts-and-functionality-images/classdiagram.png "Die primären Klassen verwendet, um Tabellenansichten anzuzeigen, werden hier angezeigt.")](table-parts-and-functionality-images/classdiagram.png#lightbox)

Der Zweck jeder Klasse lautet wie folgt:

- **UITableView** – eine Ansicht, die eine Auflistung von Zellen in einem fortlaufenden Container enthält. Die Tabellenansicht verwendet den gesamten Bildschirm in der Regel in einer iPhone-app, aber möglicherweise als Teil einer größeren Ansicht auf dem iPad vorhanden sein (oder in einem Popover angezeigt). 
- **UITableViewCell** – eine Ansicht, die eine einzelne Zelle (oder Zeile) in einer Tabelle darstellt. Es gibt vier Arten von integrierten Zelle, und es ist möglich, erstellen Sie benutzerdefinierte Zellen sowohl in C# oder mit iOS-Designer. 
- **UITableViewSource** – Xamarin.iOS ausschließliche abstrakte Klasse, die alle Methoden, die erforderlich sind, zum Anzeigen einer Tabelle, einschließlich der Anzahl der Zeilen, eine Ansicht der Zelle für jede Zeile zurückgibt, Behandeln von Zeilenauswahl und viele weitere optionalen Funktionen bereitstellt. Sie *müssen* Unterklasse diese Option, um eine UITableView funktioniert. 
- **NSIndexPath** – Contains-Zeile "und" Section-Eigenschaften, die die Position einer Zelle in einer Tabelle eindeutig identifizieren. 
- **UITableViewController** – einem bereit zu verwendende uiviewcontroller-Element, das einen hartcodierten UITableView als Ansicht "und" zugegriffen werden kann, über die TableView-Eigenschaft verfügt. 
- **UIViewController** : Wenn die Tabelle nicht den gesamten Bildschirm Sie hinzufügen einnehmen können, eine UITableView auf alle UIViewController mit seinen Frame entsprechend festgelegt. 

UITableViewSource ersetzt die folgenden zwei-Klassen, die sind weiterhin verfügbar in Xamarin.iOS sind jedoch nicht normalerweise erforderlich:

- **UITableViewDataSource** – ein Objective-C-Protokoll, das in Xamarin.iOS als abstrakte Klasse modelliert wird. Muss Unterklassen unterteilt werden, um eine Tabelle mit einer Ansicht für jede Zelle sowie Informationen über Kopfzeilen, Fußzeilen und die Anzahl der Zeilen und Abschnitte in der Tabelle bereitzustellen. 
- **UITableViewDelegate** – ein Objective-C-Protokoll, das in Xamarin.iOS als Klasse modelliert wird. Verarbeitet die Auswahl, Bearbeitung von Funktionen und andere optionale Tabellenfunktionen. 

In diesem Dokument werden die Beispiele verwenden UITableViewSource und ignorieren diese beiden Klassen. Sie werden hier erwähnt, da alle Objective-C-Beispiele finden Sie im Apple Dokumentation, verwiesen werden, daher ist es hilfreich, zu verstehen, was sie tun (und, dass die Xamarin.iOS-UITableViewSource stattdessen verwenden).

## <a name="related-links"></a>Verwandte Links

- [WorkingWithTables (Beispiel)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
