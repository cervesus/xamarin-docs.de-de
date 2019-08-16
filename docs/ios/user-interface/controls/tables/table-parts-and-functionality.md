---
title: Tabellen Teile und-Funktionen in xamarin. IOS
description: In diesem Dokument werden die verschiedenen Bestandteile von uitableview in ios beschrieben. Sie erläutert Abschnitts Header, Zellen, Abschnitts Fußzeilen, den Index und den Bearbeitungsmodus.
ms.prod: xamarin
ms.assetid: B4139C8B-28F2-4C0F-297F-BF5432C5A915
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: ff0773b3073221bead48439b7d99f1db993e3353
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69528592"
---
# <a name="table-parts-and-functionality-in-xamarinios"></a>Tabellen Teile und-Funktionen in xamarin. IOS

Eine uitableview kann einen "gruppierten" oder "Plain"-Stil aufweisen und besteht aus den folgenden Teilen:

- [Abschnitts Kopfzeile](#Section_Header)
- [Zellen](#Cells) (oder Zeilen, wenn Sie dies bevorzugen)
- [Abschnitts Fußzeile](#Section_Footer)
- [Index](#Index)
- [Bearbeitungsmodus](#Edit_Features) (enthält "Swipe zum Löschen" und Zieh Punkte zum Ändern der Zeilen Reihenfolge) 

Diese Screenshots zeigen, wie Abschnitts Zeilen, Kopfzeilen, Fußzeilen, Bearbeitungs Steuerelemente und der Index angezeigt werden.

 [![](table-parts-and-functionality-images/image1a.png "Diese Screenshots zeigen, wie Abschnitts Zeilen, Kopfzeilen, Fußzeilen, Bearbeitungs Steuerelemente und der Index angezeigt werden.")](table-parts-and-functionality-images/image1a.png#lightbox)

Diese Teile werden im folgenden ausführlicher beschrieben:

<a name="Section_Header" />

## <a name="section-header"></a>Abschnitts Kopfzeile

Zellen können optional in Abschnitte gruppiert werden, mit einem benutzerdefinierten Header versehen und/oder mit einer Fußzeile beschriftet werden. Der Header kann mit einem Zeichen folgen Wert festgelegt werden, oder es kann eine benutzerdefinierte Ansicht bereitgestellt werden, um ein anderes Layout oder Format zu ermöglichen.

<a name="Cells" />

## <a name="cells"></a>Zellen

Zellen sind das Hauptbenutzer Oberflächen-Element für eine Tabelle. Wenn die Implementierung ordnungsgemäß implementiert ist, werden Zellen für die Arbeitsspeicher Effizienz wieder verwendet. Es gibt vier integrierte Zell Stile, und Sie können eigene benutzerdefinierte Zellen erstellen – entweder im Code oder im Designer, wenn Storyboards verwendet werden.

<a name="Section_Footer"/>

## <a name="section-footer"></a>Abschnitts Fußzeile

Die optionale Abschnitts Fußzeile kann mit einem Zeichen folgen Wert festgelegt werden, oder es kann eine benutzerdefinierte Ansicht bereitgestellt werden, um ein anderes Layout oder Format zu ermöglichen. Abschnitts Kopfzeilen und-Fußzeilen können unabhängig festgelegt werden.

<a name="Index" />

## <a name="index"></a>Index

Der Index wird als Zeichenbereich am rechten Rand der Tabelle angezeigt.
Das berühren oder ziehen des Indexes beschleunigt den Bildlauf zu diesem Teil der Tabelle. Ein Index ist optional, wird aber für die Navigation in langen Listen empfohlen. Ein Index wird normalerweise nicht mit dem gruppierten Stil verwendet.

<a name="Edit_Features" />

## <a name="editing-mode"></a>Bearbeitungsmodus

Es stehen einige verschiedene Bearbeitungs Features zur Verfügung:

- Wischen Sie, um einzelne Zellen zu löschen.
- Wechseln in den Bearbeitungsmodus zum Anzeigen von Lösch Schaltflächen für jede Zeile 
- Wechseln in den Bearbeitungsmodus, um die Neuanordnung von Handles anzuzeigen. 
- Einfügen neuer Zellen (mit Animation).

Im restlichen Teil dieses Dokuments wird gezeigt, wie alle diese uitableview-Funktionen mit xamarin. IOS implementiert werden.


## <a name="classes-overview"></a>Übersicht über Klassen

Die primären Klassen, die zum Anzeigen von Tabellen Sichten verwendet werden, werden hier angezeigt:

[![](table-parts-and-functionality-images/classdiagram.png "Die primären Klassen, die zum Anzeigen von Tabellen Sichten verwendet werden, werden hier angezeigt")](table-parts-and-functionality-images/classdiagram.png#lightbox)

Der Zweck der einzelnen Klassen wird im folgenden beschrieben:

- **Uitableview** – eine Ansicht, die eine Auflistung von Zellen in einem scrollcontainer enthält. In der Tabellenansicht wird in der Regel der gesamte Bildschirm in einer iPhone-App verwendet, kann jedoch als Teil einer größeren Ansicht auf dem iPad (oder in einem popover) vorhanden sein. 
- **Uitableviewcell** – eine Ansicht, die eine einzelne Zelle (oder Zeile) in einer Tabellenansicht darstellt. Es gibt vier integrierte Zelltypen, und es ist möglich, benutzerdefinierte Zellen sowohl in C# als auch mit dem IOS-Designer zu erstellen. 
- **Uitableviewsource** – xamarin. IOS-exklusive abstrakte Klasse, die alle Methoden bereitstellt, die zum Anzeigen einer Tabelle erforderlich sind, einschließlich der Zeilen Anzahl, der Rückgabe einer Zellen Ansicht für jede Zeile, der Verarbeitung von Zeilenauswahl und vielen anderen optionalen Features. Sie *müssen* diese Unterklasse aufrufen, um eine uitableview zu erhalten. 
- **Nsindexpath** – enthält Zeilen-und Abschnitts Eigenschaften, mit denen die Position einer Zelle in einer Tabelle eindeutig identifiziert wird. 
- **Uitableviewcontroller** – ein sofort verwendende UIViewController-Objekt, das eine "uitableview" als Ansicht hart codiert und über die TableView-Eigenschaft zugänglich ist. 
- **UIViewController** – wenn die Tabelle nicht den gesamten Bildschirm einnimmt, können Sie einen "uitableview" zu jedem "UIViewController" hinzufügen, dessen Frame entsprechend festgelegt ist. 

"Uitableviewsource" ersetzt die folgenden beiden Klassen, die weiterhin in xamarin. IOS verfügbar sind, aber normalerweise nicht erforderlich sind:

- **Uitableviewdatasource** – ein Ziel-C-Protokoll, das in xamarin. IOS als abstrakte Klasse modelliert ist. Muss unter klassifiziert werden, um eine Tabelle mit einer Ansicht für jede Zelle sowie Informationen über Kopfzeilen, Fußzeilen und die Anzahl der Zeilen und Abschnitte in der Tabelle bereitzustellen. 
- **Uitableviewdelegat** – ein Ziel-C-Protokoll, das in xamarin. IOS als Klasse modelliert ist. Behandelt Auswahl, Bearbeitungsfunktionen und andere optionale Tabellen Funktionen. 

In diesem Dokument wird für die Beispiele "uitableviewsource" verwendet, und diese beiden Klassen werden ignoriert. Sie werden hier erwähnt, da alle in der Apple-Dokumentation gefundenen Ziel-C-Beispiele darauf verweisen. es ist daher hilfreich zu verstehen, was Sie tun (und dass Sie stattdessen "uitableviewsource" von xamarin. IOS verwenden können).

## <a name="related-links"></a>Verwandte Links

- [Workingwithtables (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithtables)
