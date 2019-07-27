---
title: ListView-Komponenten und -Funktionen
ms.prod: xamarin
ms.assetid: ABA40FED-FF68-C0B0-BC43-C748CEE585D8
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/21/2017
ms.openlocfilehash: 3ab7a923dabd6b98c509870abaa51b12fb63c8d2
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510122"
---
# <a name="xamarinandroid-listview-parts-and-functionality"></a>Xamarin. Android ListView-Teile und-Funktionen

Ein `ListView` besteht aus den folgenden Teilen:

- **Zeilen** &ndash; Die sichtbare Darstellung der Daten in der Liste.

- **Adapter** &ndash; Eine nicht visuelle Klasse, die die Datenquelle an die Listenansicht bindet.

- **Schnelles Scrollen** &ndash; Ein Handle, mit dem der Benutzer einen Bildlauf durch die Länge der Liste durchführen kann.

- **Abschnitts Index** &ndash; Ein Benutzeroberflächen Element, das über die scrollzeilen bewegt wird, um anzugeben, wo sich die aktuellen Zeilen in der Liste befinden.

In diesen Screenshots wird ein `ListView` einfaches Steuerelement verwendet, um anzuzeigen, wie ein schnelles Scrollen und ein Abschnitts Index gerendert

[![Screenshots von apps, die einfache alte Zeilen, ein schnelles Scrollen und einen Abschnitts Index verwenden](parts-and-functionality-images/listviewparts.png)](parts-and-functionality-images/listviewparts.png#lightbox)

Die Elemente, die eine `ListView` bilden, werden im folgenden ausführlicher beschrieben:


## <a name="rows"></a>Zeilen

Jede Zeile verfügt über eine `View`eigene Zeile. Die Sicht kann entweder eine der in `Android.Resources`definierten integrierten Sichten oder eine benutzerdefinierte Ansicht sein. Für jede Zeile kann dasselbe Ansichts Layout verwendet werden, oder Sie können unterschiedlich sein. In diesem Dokument finden Sie Beispiele für die Verwendung integrierter Layouts und andere, die erläutern, wie Sie benutzerdefinierte Layouts definieren.


## <a name="adapter"></a>Adapter

Das `ListView` -Steuerelement `Adapter` erfordert, dass das `View` formatierte für jede Zeile bereitstellt. Android verfügt über integrierte Adapter und Ansichten, die verwendet werden können, oder es können benutzerdefinierte Klassen erstellt werden.


## <a name="fast-scrolling"></a>Schnelles Scrollen

Wenn eine `ListView` viele Zeilen mit Daten enthält, kann ein schnell Bildlauf aktiviert werden, um dem Benutzer zu helfen, zu einem beliebigen Teil der Liste zu navigieren. Die schnell Bildlauf-Scrollleiste kann optional aktiviert (und in API-Ebene 11 und höher angepasst werden).


## <a name="section-index"></a>Abschnitts Index

Beim Scrollen durch lange Listen bietet der optionale Abschnitts Index dem Benutzer Feedback zu dem Teil der Liste, den er gerade anzeigen wird. Dies ist nur für lange Listen geeignet, in der Regel in Verbindung mit dem schnellen scrollen.


## <a name="classes-overview"></a>Übersicht über Klassen

Die zum Anzeigen `ListViews` verwendeten primären Klassen werden hier angezeigt:

[![UML-Diagramm, das die Beziehungen zwischen ListView und zugeordneten Klassen veranschaulicht](parts-and-functionality-images/image2.png)](parts-and-functionality-images/image2.png#lightbox)

Der Zweck der einzelnen Klassen wird im folgenden beschrieben:

- **ListView** &ndash; ein Benutzeroberflächen Element, das eine scrollbare Auflistung von Zeilen anzeigt. Auf Smartphones verwendet es normalerweise den gesamten Bildschirm (in diesem Fall kann `ListActivity` die Klasse verwendet werden) oder ist Teil eines größeren Layouts auf Smartphones oder Tablet-Geräten.

- **Anzeigen** eine Ansicht in Android kann ein beliebiges Benutzeroberflächen Element sein, aber im Kontext `ListView` eines muss für jede `View` Zeile eine bereitgestellt werden. &ndash;

- **Baseadapter** Basisklasse für Adapter Implementierungen zum Binden einer `ListView` an eine Datenquelle. &ndash;

- **Arrayadapter** Integrierte Adapter Klasse, die ein Array von Zeichen folgen für die Anzeige `ListView` an einen bindet. &ndash; Der generische `ArrayAdapter<T>` führt dasselbe für andere Typen aus.

- **CursorAdapter** &ndash; Verwenden`CursorAdapter` Sie oder`SimpleCursorAdapter` , um Daten auf der Grundlage einer SQLite-Abfrage anzuzeigen.

Dieses Dokument enthält einfache Beispiele, in denen `ArrayAdapter` ein und komplexere Beispiele verwendet werden, die benutzerdefinierte Implementierungen `BaseAdapter` von `CursorAdapter`oder erfordern.

