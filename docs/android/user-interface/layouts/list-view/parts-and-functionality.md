---
title: ListView-Komponenten und -Funktionen
ms.prod: xamarin
ms.assetid: ABA40FED-FF68-C0B0-BC43-C748CEE585D8
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/21/2017
ms.openlocfilehash: b8fd44a70f4c7ecdcf7919dec1c81461200b35bf
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028864"
---
# <a name="xamarinandroid-listview-parts-and-functionality"></a>Xamarin. Android ListView-Teile und-Funktionen

Eine `ListView` besteht aus den folgenden Teilen:

- **Zeilen** &ndash; die sichtbare Darstellung der Daten in der Liste.

- **Adapter** &ndash; eine nicht visuelle Klasse, die die Datenquelle an die Listenansicht bindet.

- **Schnelles Scrollen** &ndash; ein Handle, mit dem der Benutzer einen Bildlauf durch die Länge der Liste durchführen kann.

- Der **Abschnitts Index** &ndash; ein Benutzeroberflächen Element, das über die scrollzeilen bewegt wird, um anzugeben, wo sich die aktuellen Zeilen in der Liste befinden.

In diesen Screenshots wird ein einfaches `ListView`-Steuerelement verwendet, um zu veranschaulichen, wie ein schneller Bildlauf und ein Abschnitts Index

[![Screenshots von apps, die einfache alte Zeilen, ein schnelles Scrollen und einen Abschnitts Index verwenden](parts-and-functionality-images/listviewparts.png)](parts-and-functionality-images/listviewparts.png#lightbox)

Die Elemente, die eine `ListView` bilden, werden im folgenden ausführlicher beschrieben:

## <a name="rows"></a>Zeilen

Jede Zeile verfügt über eine eigene `View`. Die Sicht kann entweder eine der in `Android.Resources`definierten integrierten Sichten oder eine benutzerdefinierte Ansicht sein. Für jede Zeile kann dasselbe Ansichts Layout verwendet werden, oder Sie können unterschiedlich sein. In diesem Dokument finden Sie Beispiele für die Verwendung integrierter Layouts und andere, die erläutern, wie Sie benutzerdefinierte Layouts definieren.

## <a name="adapter"></a>Adapter

Das `ListView` Steuerelement erfordert eine `Adapter`, um für jede Zeile die formatierten `View` bereitzustellen. Android verfügt über integrierte Adapter und Ansichten, die verwendet werden können, oder es können benutzerdefinierte Klassen erstellt werden.

## <a name="fast-scrolling"></a>Schnelles Scrollen

Wenn eine `ListView` viele Daten Zeilen enthält, kann ein schneller Bildlauf aktiviert werden, um dem Benutzer zu helfen, zu einem beliebigen Teil der Liste zu navigieren. Die schnell Bildlauf-Scrollleiste kann optional aktiviert (und in API-Ebene 11 und höher angepasst werden).

## <a name="section-index"></a>Abschnitts Index

Beim Scrollen durch lange Listen bietet der optionale Abschnitts Index dem Benutzer Feedback zu dem Teil der Liste, den er gerade anzeigen wird. Dies ist nur für lange Listen geeignet, in der Regel in Verbindung mit dem schnellen scrollen.

## <a name="classes-overview"></a>Übersicht über Klassen

Die primären Klassen, die zum Anzeigen von `ListViews` verwendet werden, werden hier angezeigt:

[![UML-Diagramm, das die Beziehungen zwischen ListView und zugeordneten Klassen veranschaulicht](parts-and-functionality-images/image2.png)](parts-and-functionality-images/image2.png#lightbox)

Der Zweck der einzelnen Klassen wird im folgenden beschrieben:

- **ListView** &ndash; Benutzeroberflächen Element, das eine scrollbare Auflistung von Zeilen anzeigt. Auf Smartphones verwendet es normalerweise den gesamten Bildschirm (in diesem Fall kann die `ListActivity` Klasse verwendet werden) oder ist Teil eines größeren Layouts auf Smartphones oder Tablet-Geräten.

- **Ansicht** &ndash; eine Ansicht in Android kann ein beliebiges Benutzeroberflächen Element sein, aber im Kontext einer `ListView` muss für jede Zeile ein `View` bereitgestellt werden.

- **Baseadapter** &ndash; Basisklasse für Adapter Implementierungen, um eine `ListView` an eine Datenquelle zu binden.

- **Arrayadapter** &ndash; integrierte Adapter Klasse, die ein Array von Zeichen folgen für die Anzeige an eine `ListView` bindet. Der generische `ArrayAdapter<T>` führt dasselbe für andere Typen aus.

- Der **CursorAdapter** &ndash; `CursorAdapter` oder `SimpleCursorAdapter` zum Anzeigen von Daten auf der Grundlage einer SQLite-Abfrage verwenden.

Dieses Dokument enthält einfache Beispiele, in denen eine `ArrayAdapter` und komplexere Beispiele verwendet werden, die benutzerdefinierte Implementierungen von `BaseAdapter` oder `CursorAdapter`erfordern.
