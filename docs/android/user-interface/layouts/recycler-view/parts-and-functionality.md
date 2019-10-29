---
title: Recyclerview-Teile und-Funktionalität
description: Eine Übersicht über den wiedergabeview-Layoutmanager,-Adapter und-Ansichts Halter.
ms.prod: xamarin
ms.assetid: 54F999BE-2732-4BC7-A466-D17373961C48
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/13/2018
ms.openlocfilehash: 823215b7cc1bac8a8f6bffc6e480400135e88ce8
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028819"
---
# <a name="recyclerview-parts-and-functionality"></a>Recyclerview-Teile und-Funktionalität

`RecyclerView` behandelt einige Aufgaben intern (z. b. den Bildlauf und die Wiederverwendung von Sichten), aber es handelt sich im Grunde um einen Vorgesetzten, der Hilfsklassen zum Anzeigen einer Auflistung koordiniert. `RecyclerView` delegiert Tasks an die folgenden Hilfsklassen:

- **`Adapter`** &ndash; die Element Layouts vergrößert (instanziiert den Inhalt einer Layoutdatei) und Daten an Sichten bindet, die innerhalb eines `RecyclerView`angezeigt werden. Der Adapter meldet auch Element-Click-Ereignisse.

- **`LayoutManager`** &ndash; Measures und positioniert Element Sichten innerhalb eines `RecyclerView` und verwaltet die Richtlinie für die Sicht Wiederverwendung.

- **`ViewHolder`** &ndash; sucht nach Ansichts verweisen und speichert Sie. Der Ansichts Halter unterstützt auch das Erkennen von Klicks auf Element Sicht.

- **`ItemDecoration`** &ndash; ermöglicht einer APP das Hinzufügen spezieller Zeichnungs-und layoutoffsets zu bestimmten Ansichten für das Zeichnen von Trennzeichen zwischen Elementen, Hervorhebungen und visuellen Gruppierungs Grenzen.

- **`ItemAnimator`** &ndash; werden die Animationen definiert, die während der Element Aktionen stattfinden, oder, wenn Änderungen am Adapter vorgenommen werden.

Die Beziehung zwischen den Klassen `RecyclerView`, `LayoutManager`und `Adapter` wird in der folgenden Abbildung dargestellt:

![Diagramm der recyclerview mit Layoutmanager, Verwendung des Adapters für den Zugriff auf das DataSet](parts-and-functionality-images/01-recyclerview-diagram.png)

Wie in dieser Abbildung veranschaulicht, kann die `LayoutManager` als Vermittler zwischen dem `Adapter` und dem `RecyclerView`betrachtet werden. Der `LayoutManager` führt im Auftrag des `RecyclerView`Aufrufe `Adapter` Methoden aus. Beispielsweise ruft die `LayoutManager` eine `Adapter` Methode auf, wenn es an der Zeit ist, eine neue Ansicht für eine bestimmte Element Position in der `RecyclerView`zu erstellen. Der `Adapter` vergrößert das Layout für dieses Element und erstellt eine `ViewHolder` (nicht angezeigte) Instanz, um Verweise auf die Ansichten an dieser Position zwischenzuspeichern. Wenn das `LayoutManager` die `Adapter` aufruft, um ein bestimmtes Element an das DataSet zu binden, sucht der `Adapter` die Daten für dieses Element, ruft es aus dem DataSet ab und kopiert es in die zugeordnete Element Ansicht.

Wenn Sie `RecyclerView` in Ihrer APP verwenden, ist das Erstellen abgeleiteter Typen der folgenden Klassen erforderlich:

- **`RecyclerView.Adapter`** &ndash; stellt eine Bindung aus dem DataSet Ihrer APP (spezifisch für Ihre APP) für Element Ansichten bereit, die im `RecyclerView`angezeigt werden. Der Adapter weiß, wie jede Element Ansichts Position im `RecyclerView` einer bestimmten Position in der Datenquelle zugeordnet wird. Außerdem verarbeitet der Adapter das Layout der Inhalte innerhalb der einzelnen Element Ansichten und erstellt den Ansichts Halter für jede Ansicht. Der Adapter meldet auch Element-Click-Ereignisse, die von der Element Ansicht erkannt werden.

- **`RecyclerView.ViewHolder`** &ndash; speichert Verweise auf die Sichten in der Element Layoutdatei, sodass Ressourcen Suchvorgänge nicht unnötig wiederholt werden. Der Ansichts Halter gibt auch an, dass Element-Click-Ereignisse an den Adapter weitergeleitet werden sollen, wenn ein Benutzer auf die verknüpfte Element Ansicht des Ansichts Inhabers tippt.

- **`RecyclerView.LayoutManager`** &ndash; positioniert Elemente innerhalb der `RecyclerView`. Sie können einen von mehreren vordefinierten layoutmanagern verwenden, oder Sie können einen eigenen benutzerdefinierten LayoutManager implementieren.
    `RecyclerView` delegiert die Layoutrichtlinie an den Layout-Manager, sodass Sie einen anderen LayoutManager einbinden können, ohne dass Sie bedeutende Änderungen an Ihrer APP vornehmen müssen.

Außerdem können Sie optional die folgenden Klassen erweitern, um das Erscheinungsbild von `RecyclerView` in Ihrer APP zu ändern:

- **`RecyclerView.ItemDecoration`**
- **`RecyclerView.ItemAnimator`**

Wenn Sie `ItemDecoration` und `ItemAnimator`nicht erweitern, verwendet `RecyclerView` Standard Implementierungen. In diesem Handbuch wird nicht erläutert, wie benutzerdefinierte `ItemDecoration` und `ItemAnimator` Klassen erstellt werden. Weitere Informationen zu diesen Klassen finden Sie unter " [recyclerview. itemdecoration](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemDecoration.html) " und " [recyclerview. itemanimator](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemAnimator.html)".

<a name="recycling" />

## <a name="how-view-recycling-works"></a>Funktionsweise der Sicht Wiederverwendung

`RecyclerView` weist keine Element Ansicht für jedes Element in der Datenquelle zu. Stattdessen wird nur die Anzahl der Element Ansichten zugewiesen, die auf den Bildschirm passen, und diese Element Layouts werden wieder verwendet, wenn der Benutzer einen Bildlauf ausführt. Wenn die Ansicht zum ersten Mal einen Bildlauf durchführt, wird der in der folgenden Abbildung dargestellte Wiederverwendungs Prozess durchlaufen:

[![Diagramm mit den sechs Schritten der Wiederverwendung von Ansichten](parts-and-functionality-images/02-view-recycling-sml.png)](parts-and-functionality-images/02-view-recycling.png#lightbox)

1. Wenn eine Sicht nicht mehr sichtbar ist und nicht mehr angezeigt wird, wird Sie zu einer Sperr *Ansicht*.

2. Die *Papierkorb*Ansicht wird in einem Pool abgelegt und wird zu einer Wiederverwendungs Ansicht.
    Dieser Pool ist ein Cache von Sichten, die denselben Datentyp anzeigen.

3. Wenn ein neues Element angezeigt werden soll, wird für die Wiederverwendung eine Ansicht aus dem Papierkorb entnommen. Da diese Ansicht durch den Adapter erneut gebunden werden muss, bevor Sie angezeigt wird, wird Sie als modifizierte *Ansicht*bezeichnet.

4. Die geänderte Ansicht wird wieder verwendet: der Adapter sucht die Daten für das nächste Element, das angezeigt werden soll, und kopiert diese Daten in die Sichten für dieses Element. Verweise für diese Sichten werden vom Ansichts Inhaber abgerufen, der der wiederverwendeten Ansicht zugeordnet ist.

5. Die wiederverwendete Ansicht wird der Liste der Elemente in der `RecyclerView` hinzugefügt, die im Begriff sind, auf dem Bildschirm zu wechseln.

6. Die wiederverwendete Ansicht wird auf dem Bildschirm angezeigt, wenn der Benutzer den `RecyclerView` zum nächsten Element in der Liste führt. In der Zwischenzeit führt eine andere Ansicht einen Bildlauf aus und wird gemäß den obigen Schritten wieder verwendet.

Zusätzlich zur Wiederverwendung von Element-View verwendet `RecyclerView` auch eine andere Effizienzoptimierung: Ansichts Halter. Ein *Ansichts Halter* ist eine einfache Klasse, die Ansichts Verweise zwischenspeichert. Jedes Mal, wenn der Adapter eine Element Layoutdatei auffüllt, erstellt er auch einen entsprechenden Ansichts Halter. Der Ansichts Halter verwendet `FindViewById`, um Verweise auf die Sichten innerhalb der aufgeblähten Element Layout-Datei zu erhalten. Diese Verweise werden zum Laden neuer Daten in die Ansichten jedes Mal verwendet, wenn das Layout wieder verwendet wird, um neue Daten anzuzeigen.

## <a name="the-layout-manager"></a>Der Layout-Manager

Der LayoutManager ist für das Positionieren von Elementen in der `RecyclerView` Anzeige zuständig. Es bestimmt den Präsentationstyp (eine Liste oder ein Raster), die Ausrichtung (ob Elemente vertikal oder horizontal angezeigt werden) und welche Richtungs Elemente angezeigt werden sollen (in normaler Reihenfolge oder in umgekehrter Reihenfolge). Der LayoutManager ist auch für die Berechnung der Größe und Position der einzelnen Elemente in der Wiederverwendung von " **recycleview** " verantwortlich.

Der Layout-Manager hat einen zusätzlichen Zweck: er bestimmt die Richtlinie, wann Element Sichten wieder verwendet werden sollen, die für den Benutzer nicht mehr sichtbar sind.
Da der LayoutManager weiß, welche Sichten sichtbar sind (und welche nicht), ist er am besten geeignet, um zu entscheiden, wann eine Ansicht wieder verwendet werden kann. Zum wieder verwenden einer Ansicht führt der LayoutManager in der Regel Aufrufe an den Adapter aus, um den Inhalt einer wiederverwendeten Sicht durch andere Daten zu ersetzen, wie zuvor unter [Funktionsweise der Ansicht](#recycling)"Wiederverwendung" beschrieben.

Sie können `RecyclerView.LayoutManager` erweitern, um einen eigenen LayoutManager zu erstellen, oder Sie können einen vordefinierten Layoutmanager verwenden. `RecyclerView` stellt die folgenden vordefinierten LayoutManager bereit:

- **`LinearLayoutManager`** &ndash; ordnet Elemente in einer Spalte an, die vertikal oder in einer Zeile, für die ein horizontaler Bildlauf ausgeführt werden kann, ausgeführt werden kann.

- **`GridLayoutManager`** &ndash; zeigt Elemente in einem Raster an.

- in **`StaggeredGridLayoutManager`** &ndash; werden Elemente in einem gestaffelten Raster angezeigt, in dem einige Elemente verschiedene Höhen und breiten aufweisen.

Um den LayoutManager anzugeben, instanziieren Sie den ausgewählten Layoutmanager, und übergeben Sie ihn an die `SetLayoutManager`-Methode. Beachten Sie, dass Sie den LayoutManager angeben *müssen* , &ndash; `RecyclerView` standardmäßig keinen vordefinierten LayoutManager auswählt.

Weitere Informationen zum LayoutManager finden Sie in der [Klassenreferenz "recyclerview. LayoutManager](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.LayoutManager.html)".

## <a name="the-view-holder"></a>Der Ansichts Halter

Der Ansichts Halter ist eine Klasse, die Sie für das Zwischenspeichern von Ansichts verweisen definieren. Der Adapter verwendet diese Ansichts Verweise, um jede Ansicht an ihren Inhalt zu binden. Jedes Element in der `RecyclerView` verfügt über eine zugeordnete Ansichts Inhaber Instanz, die die Ansichts Verweise für dieses Element zwischenspeichert. Verwenden Sie zum Erstellen eines Ansichts Besitzers die folgenden Schritte, um eine Klasse zu definieren, die den genauen Satz von Sichten pro Element enthalten soll:

1. Unterklasse `RecyclerView.ViewHolder`.
2. Implementieren Sie einen Konstruktor, der die Ansichts Verweise sucht und speichert.
3. Implementieren Sie Eigenschaften, die der Adapter für den Zugriff auf diese Verweise verwenden kann.

Ein ausführliches Beispiel für eine `ViewHolder` Implementierung finden Sie in [einem grundlegenden Beispiel für eine recyclerview](~/android/user-interface/layouts/recycler-view/recyclerview-example.md).
Weitere Informationen zu `RecyclerView.ViewHolder`finden Sie in der [Klassenreferenz "recyclerview. viewholder](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html)".

## <a name="the-adapter"></a>Der Adapter

Der größte Teil des `RecyclerView` Integrations Codes findet im Adapter statt. `RecyclerView` erfordert, dass Sie einen von `RecyclerView.Adapter` abgeleiteten Adapter bereitstellen, um auf Ihre Datenquelle zuzugreifen und jedes Element mit Inhalt aus der Datenquelle aufzufüllen.
Da die Datenquelle App-spezifisch ist, müssen Sie Adapter Funktionen implementieren, die den Zugriff auf Ihre Daten verstehen. Der Adapter extrahiert Informationen aus der Datenquelle und lädt Sie in jedes Element in der `RecyclerView` Auflistung.

In der folgenden Abbildung wird veranschaulicht, wie der Adapter Inhalte in einer Datenquelle mithilfe von Ansichts Besitzern den einzelnen Ansichten innerhalb der einzelnen Zeilen Elemente in der `RecyclerView`zuordnet:

[![Diagramm zum veranschaulichen des Adapters zum Verbinden der Datenquelle mit viewholders](parts-and-functionality-images/03-recyclerviewer-adapter-sml.png)](parts-and-functionality-images/03-recyclerviewer-adapter.png#lightbox)

Der Adapter lädt jede `RecyclerView` Zeile mit Daten für ein bestimmtes Zeilen Element. Bei Zeilen Position *p*sucht der Adapter z. b. die zugeordneten Daten an Position *p* innerhalb der Datenquelle und kopiert diese Daten in das Zeilen Element an Position *p* in der `RecyclerView` Auflistung.
In der obigen Zeichnung verwendet der Adapter z. b. den Ansichts Halter, um die Verweise für die `ImageView` zu suchen und an dieser Position `TextView`, damit nicht wiederholt `FindViewById` für diese Ansichten aufgerufen werden muss, wenn der Benutzer einen Bildlauf durch die Auflistung durchführt und die Ansichten erneut verwendet.

Wenn Sie einen Adapter implementieren, müssen Sie die folgenden `RecyclerView.Adapter` Methoden überschreiben:

- **`OnCreateViewHolder`** &ndash; instanziiert die elementlayoutdatei und den Ansichts Halter.

- **`OnBindViewHolder`** &ndash; lädt die Daten an der angegebenen Position in die Sichten, deren Verweise im angegebenen Ansichts Halter gespeichert werden.

- **`ItemCount`** &ndash; gibt die Anzahl der Elemente in der Datenquelle zurück.

Der Layout-Manager ruft diese Methoden auf, während Elemente innerhalb der `RecyclerView`positioniert werden.

## <a name="notifying-recyclerview-of-data-changes"></a>Benachrichtigen von "recyclerview" von Datenänderungen

`RecyclerView` aktualisiert seine Anzeige nicht automatisch, wenn sich der Inhalt der Datenquelle ändert. der Adapter muss `RecyclerView` Benachrichtigen, wenn das Dataset geändert wurde. Das DataSet kann in vielerlei Hinsicht geändert werden. Beispielsweise kann sich der Inhalt in einem Element ändern, oder die allgemeine Struktur der Daten kann geändert werden.
`RecyclerView.Adapter` stellt eine Reihe von Methoden bereit, die aufgerufen werden können, damit `RecyclerView` auf die effizienteste Weise auf Datenänderungen reagiert:

- **`NotifyItemChanged`** &ndash; signalisiert, dass sich das Element an der angegebenen Position geändert hat.

- **`NotifyItemRangeChanged`** &ndash; signalisiert, dass sich die Elemente im angegebenen Bereich von Positionen geändert haben.

- **`NotifyItemInserted`** &ndash; signalisiert, dass das Element an der angegebenen Position neu eingefügt wurde.

- **`NotifyItemRangeInserted`** &ndash; signalisiert, dass die Elemente im angegebenen Bereich von Positionen neu eingefügt wurden.

- **`NotifyItemRemoved`** &ndash; signalisiert, dass das Element an der angegebenen Position entfernt wurde.

- **`NotifyItemRangeRemoved`** &ndash; signalisiert, dass die Elemente im angegebenen Bereich von Positionen entfernt wurden.

- **`NotifyDataSetChanged`** &ndash; signalisiert, dass sich der Datensatz geändert hat (erzwingt ein vollständiges Update).

Wenn Sie genau wissen, wie sich das Dataset geändert hat, können Sie die oben genannten Methoden zum Aktualisieren `RecyclerView` auf die effizienteste Weise aufzurufen. Wenn Sie nicht genau wissen, wie sich das Dataset geändert hat, können Sie `NotifyDataSetChanged`aufrufen. Dies ist weitaus weniger effizient, da `RecyclerView` alle Sichten aktualisieren muss, die für den Benutzer sichtbar sind. Weitere Informationen zu diesen Methoden finden Sie unter [recyclerview. Adapter](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html).

Im nächsten Thema, [einem grundlegenden recyclerview-Beispiel](~/android/user-interface/layouts/recycler-view/recyclerview-example.md), wird eine Beispiel-App implementiert, um echte Codebeispiele für die oben beschriebenen Teile und Funktionen zu veranschaulichen.

## <a name="related-links"></a>Verwandte Links

- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [Ein grundlegendes recyclerview-Beispiel](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [Erweitern des recyclerview-Beispiels](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
