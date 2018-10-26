---
title: ListView-Komponenten und Funktionen
ms.prod: xamarin
ms.assetid: ABA40FED-FF68-C0B0-BC43-C748CEE585D8
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/21/2017
ms.openlocfilehash: 248baa5daceff6db01098a155600ea204547e845
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116112"
---
# <a name="listview-parts-and-functionality"></a>ListView-Komponenten und Funktionen


## <a name="overview"></a>Übersicht

Ein `ListView` besteht aus den folgenden Teilen:

- **Zeilen** &ndash; die sichtbare Darstellung der Daten in der Liste.

- **Adapter** &ndash; eine nicht-Visual-Klasse, die der Listenansicht die Datenquelle gebunden wird.

- **Schnelle fortlaufender** &ndash; ein Handle, das der Benutzer, die die Länge der Liste einen Bildlauf durchführen kann.

- **Im Abschnitt Index** &ndash; ein Element der Benutzeroberfläche, die über das Durchführen eines Bildlaufs gleitet Zeilen, um anzugeben, wo sich die aktuellen Zeilen in der Liste befinden.

Diese Screenshots verwenden eine einfache `ListView` -Steuerelement anzeigen, wie schnell der Bildlauf und Abschnittsindex gerendert werden:

[![Screenshots der apps, die einfache alte Zeilen, schnelle, Bildlauf und Abschnittsindex](parts-and-functionality-images/listviewparts.png)](parts-and-functionality-images/listviewparts.png#lightbox)

Die Elemente, aus denen ein `ListView` werden im folgenden ausführlicher beschrieben:


## <a name="rows"></a>Zeilen

Jede Zeile verfügt über eine eigene `View`. Die Ansicht ist einer der integrierten in definierten Ansichten `Android.Resources`, oder eine benutzerdefinierte Ansicht. Jede Zeile kann dasselbe Layout anzeigen oder sie können sich alle unterscheiden. Es gibt Beispiele in diesem Dokument von integrierten Layouts und andere erläutert, wie Sie benutzerdefinierte Layouts definieren.


## <a name="adapter"></a>Adapter

Die `ListView` Steuerelement erfordert ein `Adapter` formatierten angeben `View` für jede Zeile. Android bietet integrierte Adapter und Ansichten, die verwendet werden können, oder benutzerdefinierte Klassen können erstellt werden.


## <a name="fast-scrolling"></a>Schnelles Scrollen

Wenn eine `ListView` enthält viele Zeilen von Daten schnell Bildlauf kann aktiviert werden, um die Benutzer auf einen beliebigen Teil der Liste navigieren können. Die schnelle Bildläufe "Bildlaufleiste" kann optional aktiviert (und benutzerdefinierte in-API-Ebene 11 und höher) sein.


## <a name="section-index"></a>Abschnitt-Index

Beim Durchführen eines Bildlaufs durch lange Listen, bietet der optionalen Abschnittsindex dem Benutzer Feedback auf welchen Teil der Liste der sie gerade angezeigt werden. Es eignet sich nur auf langen Listen, in der Regel in Verbindung mit schnellen scrollen.


## <a name="classes-overview"></a>Übersicht über Klassen

Die primären Klassen verwendet, um anzuzeigen `ListViews` werden hier angezeigt:

[![UML-Diagramm zur Veranschaulichung der Beziehungen zwischen ListView und verknüpfter Klassen](parts-and-functionality-images/image2.png)](parts-and-functionality-images/image2.png#lightbox)

Der Zweck jeder Klasse lautet wie folgt:

- **ListView** &ndash; Benutzeroberflächenelement, das eine bildlauffähige Sammlung von Zeilen anzeigt. Auf Smartphones es in der Regel verwendet den gesamten Bildschirm (in diesem Fall die `ListActivity` Klasse kann verwendet werden) oder Teil eines größeren Layouts auf Smartphones oder Tablets sein.

- **Ansicht** &ndash; eine Ansicht in Android kann jedem Element der Benutzeroberfläche, werden aber im Kontext des eine `ListView` erfordert eine `View` für jede Zeile bereitgestellt werden.

- **BaseAdapter** &ndash; Basisklasse für Implementierungen der Adapter zum Binden einer `ListView` mit einer Datenquelle.

- **ArrayAdapter** &ndash; integrierten Adapter-Klasse, die ein Array von Zeichenfolgen, bindet ein `ListView` für die Anzeige. Die generische `ArrayAdapter<T>` führt dies für andere Typen.

- **CursorAdapter** &ndash; verwenden `CursorAdapter` oder `SimpleCursorAdapter` zum Anzeigen von Daten basierend auf einer SQLite-Abfrage.

Dieses Dokument enthält einfache Beispiele, in denen ein `ArrayAdapter` sowie komplexere Beispiele, die benutzerdefinierte Implementierungen der erfordern `BaseAdapter` oder `CursorAdapter`.

