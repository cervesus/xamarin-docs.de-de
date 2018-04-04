---
title: ListView-Komponenten und Funktionen
ms.prod: xamarin
ms.assetid: ABA40FED-FF68-C0B0-BC43-C748CEE585D8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/21/2017
ms.openlocfilehash: ec9b6583cac9478957e14f709c7c7ee8eb273cbc
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="listview-parts-and-functionality"></a>ListView-Komponenten und Funktionen


## <a name="overview"></a>Übersicht

Ein `ListView` besteht aus folgenden Teilen:

- **Zeilen** &ndash; sichtbare Darstellung der Daten in der Liste.

- **Adapter** &ndash; eine nicht visuelle-Klasse, die der Listenansicht die Datenquelle gebunden wird.

- **Schnelle Bildlauf** &ndash; ein Handle, das dem Benutzer, der die Länge der Liste einen Bildlauf durchführen kann.

- **Im Abschnitt Index** &ndash; ein Benutzeroberflächenelement, das über das Durchführen eines Bildlaufs gleitet Zeilen, um anzugeben, wo sich die aktuellen Zeilen in der Liste befinden.

Diese Screenshots verwenden einen grundlegenden `ListView` -Steuerelement anzeigen, wie schnell durchführen eines Bildlaufs und Abschnittsindex gerendert werden:

[![Screenshots der apps mit einfachen alte Zeilen, für die schnelle Durchführen eines Bildlaufs und Abschnittsindex](parts-and-functionality-images/listviewparts.png)](parts-and-functionality-images/listviewparts.png#lightbox)

Die Elemente, aus denen eine `ListView` werden im folgenden ausführlicher beschrieben:


## <a name="rows"></a>Zeilen

Jede Zeile verfügt über eine eigene `View`. Die Sicht kann entweder eine der integrierten in definierten Ansichten `Android.Resources`, oder eine benutzerdefinierte Ansicht. Jede Zeile kann dasselbe Ansichtslayout verwenden, oder sie können alle unterschiedlich sein. Es gibt Beispiele in diesem Dokument des integrierten Layouts und andere erläutern, wie Sie benutzerdefinierte Layouts definieren.


## <a name="adapter"></a>Adapter

Die `ListView` Steuerelement erfordert eine `Adapter` formatierten angeben `View` für jede Zeile. Android bietet integrierte Adapter und Sichten, die verwendet werden können, oder benutzerdefinierte Klassen können erstellt werden.


## <a name="fast-scrolling"></a>Schnelle Durchführen eines Bildlaufs

Wenn eine `ListView` enthält viele Zeilen von Daten durchführen eines Bildlaufs Fast kann aktiviert werden, können Benutzer auf einen beliebigen Teil der Liste navigieren. Die schnelle Bildläufe "Bildlaufleiste" kann optional aktiviert (und angepasste in API-Ebene 11 und höher) sein.


## <a name="section-index"></a>Abschnittsindex

Beim Durchführen eines Bildlaufs durch lange Listen, bietet die optionale Abschnittsindex dem Benutzer Feedback auf welcher Teil der Liste der aktuell angezeigten. Es eignet sich nur auf lange Listen, in der Regel in Verbindung mit der schnell durchführen eines Bildlaufs.


## <a name="classes-overview"></a>Übersicht über Klassen

Die primären Klassen, die zur Anzeige verwendet `ListViews` werden hier angezeigt:

[![UML-Diagramm zur Veranschaulichung der Beziehungen zwischen ListView-Steuerelement und die zugehörigen Klassen](parts-and-functionality-images/image2.png)](parts-and-functionality-images/image2.png#lightbox)

Der Zweck der einzelnen Klassen wird im folgenden beschrieben:

- **ListView** &ndash; Benutzeroberflächenelement, das eine bildlauffähige Auflistung von Zeilen anzeigt. Auf Mobiltelefonen es in der Regel verwendet den gesamten Bildschirm (in diesem Fall die `ListActivity` -Klasse kann verwendet werden) oder ein Teil eines größeren Layouts auf Smartphones oder tabletgeräte sein.

- **Ansicht** &ndash; eine Sicht in Android kann keinem Element der Benutzeroberfläche, aber im Rahmen einer `ListView` dafür eine `View` für jede Zeile bereitgestellt werden.

- **BaseAdapter** &ndash; Basisklasse für Implementierungen der Adapter zum Binden einer `ListView` mit einer Datenquelle.

- **ArrayAdapter** &ndash; integrierten Adapter-Klasse, die ein Array von Zeichenfolgen, bindet ein `ListView` für die Anzeige. Die generische `ArrayAdapter<T>` hat die gleiche Funktion für andere Anwendungstypen.

- **CursorAdapter** &ndash; verwenden `CursorAdapter` oder `SimpleCursorAdapter` zum Anzeigen von Daten basierend auf einer SQLite-Abfrage.

Dieses Dokument enthält einfache Beispiele, in denen ein `ArrayAdapter` sowie komplexere Beispiele, die benutzerdefinierte Implementierungen der erfordern `BaseAdapter` oder `CursorAdapter`.

