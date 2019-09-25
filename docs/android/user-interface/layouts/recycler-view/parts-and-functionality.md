---
title: Recyclerview-Teile und-Funktionalität
description: Eine Übersicht über den wiedergabeview-Layoutmanager,-Adapter und-Ansichts Halter.
ms.prod: xamarin
ms.assetid: 54F999BE-2732-4BC7-A466-D17373961C48
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/13/2018
ms.openlocfilehash: 7aa2cae4c8ca1ef9bb0412a4a62dc619af97b57f
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71249772"
---
# <a name="recyclerview-parts-and-functionality"></a>Recyclerview-Teile und-Funktionalität

`RecyclerView`behandelt einige Aufgaben intern (z. b. den Bildlauf und die Wiederverwendung von Sichten), ist aber im Grunde ein Manager, der Hilfsklassen zum Anzeigen einer Auflistung koordiniert. `RecyclerView`delegiert Tasks an die folgenden Hilfsklassen:

- **`Adapter`** Vergrößert Element Layouts (instanziiert den Inhalt einer Layoutdatei) und bindet Daten an Sichten, die in einer `RecyclerView`angezeigt werden. &ndash; Der Adapter meldet auch Element-Click-Ereignisse.

- **`LayoutManager`** Misst und positioniert Element Sichten innerhalb einer `RecyclerView` und verwaltet die Richtlinie für die Ansicht-Wiederverwendung. &ndash;

- **`ViewHolder`** &ndash; Sucht und speichert Ansichts Verweise. Der Ansichts Halter unterstützt auch das Erkennen von Klicks auf Element Sicht.

- **`ItemDecoration`** &ndash; Ermöglicht es einer APP, bestimmten Ansichten spezielle Zeichnungs-und layoutoffsets hinzuzufügen, um Trennzeichen zwischen Elementen, Hervorhebungen und visuellen Gruppierungs Grenzen zu zeichnen.

- **`ItemAnimator`** &ndash; Definiert die Animationen, die während der Element Aktionen stattfinden, oder, wenn Änderungen am Adapter vorgenommen werden.

Die Beziehung zwischen der `RecyclerView`- `LayoutManager`Klasse, `Adapter` der-Klasse und der-Klasse wird im folgenden Diagramm dargestellt:

![Diagramm der recyclerview mit Layoutmanager, Verwendung des Adapters für den Zugriff auf das DataSet](parts-and-functionality-images/01-recyclerview-diagram.png)

Wie die Abbildung zeigt, `LayoutManager` kann der als Vermittler zwischen dem `Adapter` und dem `RecyclerView`betrachtet werden. Der `LayoutManager` ruft`RecyclerView`Methoden im Namen von auf. `Adapter` Beispielsweise `LayoutManager` Ruft eine `Adapter` -Methode auf, wenn es an der Zeit ist, eine neue Ansicht für eine bestimmte Element Position `RecyclerView`in der zu erstellen. Vergrößert das Layout für dieses Element und erstellt eine `ViewHolder` -Instanz (nicht angezeigt), um Verweise auf die Ansichten an dieser Position zwischenzuspeichern. `Adapter` Wenn der `LayoutManager` `Adapter` aufruft, umeinbestimmtesElementandasDataSetzubinden,suchtdieDatenfürdiesesElement,ruftSieausdemDataSetabundkopiert`Adapter` Sie in die zugeordnete Element Ansicht.

Wenn Sie `RecyclerView` in Ihrer APP verwenden, ist das Erstellen abgeleiteter Typen der folgenden Klassen erforderlich:

- **`RecyclerView.Adapter`** Stellt eine Bindung aus dem DataSet Ihrer APP (die für Ihre APP spezifisch ist) für Element Ansichten bereit, die in der `RecyclerView`angezeigt werden. &ndash; Der Adapter weiß, wie jede Element Ansichts Position in der `RecyclerView` einem bestimmten Speicherort in der Datenquelle zugeordnet wird. Außerdem verarbeitet der Adapter das Layout der Inhalte innerhalb der einzelnen Element Ansichten und erstellt den Ansichts Halter für jede Ansicht. Der Adapter meldet auch Element-Click-Ereignisse, die von der Element Ansicht erkannt werden.

- **`RecyclerView.ViewHolder`** &ndash; Speichert Verweise auf die Ansichten in der Element Layoutdatei, sodass Ressourcen Suchvorgänge nicht unnötig wiederholt werden. Der Ansichts Halter gibt auch an, dass Element-Click-Ereignisse an den Adapter weitergeleitet werden sollen, wenn ein Benutzer auf die verknüpfte Element Ansicht des Ansichts Inhabers tippt.

- **`RecyclerView.LayoutManager`** Positioniert Elemente innerhalb der `RecyclerView`. &ndash; Sie können einen von mehreren vordefinierten layoutmanagern verwenden, oder Sie können einen eigenen benutzerdefinierten LayoutManager implementieren.
    `RecyclerView`delegiert die Layoutrichtlinie an den Layout-Manager, sodass Sie einen anderen LayoutManager einbinden können, ohne dass Sie bedeutende Änderungen an der APP vornehmen müssen.

Außerdem können Sie optional die folgenden Klassen erweitern, um das Erscheinungsbild von `RecyclerView` in Ihrer APP zu ändern:

- **`RecyclerView.ItemDecoration`**
- **`RecyclerView.ItemAnimator`**

Wenn Sie und `ItemAnimator`nicht erweitern `ItemDecoration` , `RecyclerView` verwendet Standard Implementierungen. In diesem Handbuch wird nicht erläutert, wie Benutzer `ItemDecoration` definierte `ItemAnimator` -und-Klassen erstellt werden. Weitere Informationen zu diesen Klassen finden Sie unter " [recyclerview. itemdecoration](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemDecoration.html) " und " [recyclerview. itemanimator](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemAnimator.html)".

<a name="recycling" />

## <a name="how-view-recycling-works"></a>Funktionsweise der Sicht Wiederverwendung

`RecyclerView`weist keine Element Ansicht für jedes Element in der Datenquelle zu. Stattdessen wird nur die Anzahl der Element Ansichten zugewiesen, die auf den Bildschirm passen, und diese Element Layouts werden wieder verwendet, wenn der Benutzer einen Bildlauf ausführt. Wenn die Ansicht zum ersten Mal einen Bildlauf durchführt, wird der in der folgenden Abbildung dargestellte Wiederverwendungs Prozess durchlaufen:

[![Diagramm mit den sechs Schritten der Wiederverwendung von Ansichten](parts-and-functionality-images/02-view-recycling-sml.png)](parts-and-functionality-images/02-view-recycling.png#lightbox)

1. Wenn eine Sicht nicht mehr sichtbar ist und nicht mehr angezeigt wird, wird Sie zu einer Sperr *Ansicht*.

2. Die *Papierkorb*Ansicht wird in einem Pool abgelegt und wird zu einer Wiederverwendungs Ansicht.
    Dieser Pool ist ein Cache von Sichten, die denselben Datentyp anzeigen.

3. Wenn ein neues Element angezeigt werden soll, wird für die Wiederverwendung eine Ansicht aus dem Papierkorb entnommen. Da diese Ansicht durch den Adapter erneut gebunden werden muss, bevor Sie angezeigt wird, wird Sie als modifizierte *Ansicht*bezeichnet.

4. Die geänderte Ansicht wird wieder verwendet: der Adapter sucht die Daten für das nächste Element, das angezeigt werden soll, und kopiert diese Daten in die Sichten für dieses Element. Verweise für diese Sichten werden vom Ansichts Inhaber abgerufen, der der wiederverwendeten Ansicht zugeordnet ist.

5. Die wiederverwendete Ansicht wird der Liste der Elemente in der `RecyclerView` hinzugefügt, die auf dem Bildschirm angezeigt werden.

6. Die wiederverwendete Ansicht wird auf dem Bildschirm angezeigt `RecyclerView` , wenn der Benutzer einen Bildlauf zum nächsten Element in der Liste durchführt. In der Zwischenzeit führt eine andere Ansicht einen Bildlauf aus und wird gemäß den obigen Schritten wieder verwendet.

Zusätzlich zur Wiederverwendung `RecyclerView` von Element-View verwendet auch eine weitere Effizienzoptimierung: Ansichts Halter. Ein *Ansichts Halter* ist eine einfache Klasse, die Ansichts Verweise zwischenspeichert. Jedes Mal, wenn der Adapter eine Element Layoutdatei auffüllt, erstellt er auch einen entsprechenden Ansichts Halter. Der Ansichts Halter `FindViewById` verwendet, um Verweise auf die Sichten in der aufheften Element Layout-Datei zu erhalten. Diese Verweise werden zum Laden neuer Daten in die Ansichten jedes Mal verwendet, wenn das Layout wieder verwendet wird, um neue Daten anzuzeigen.

## <a name="the-layout-manager"></a>Der Layout-Manager

Der LayoutManager ist für das Positionieren von Elementen `RecyclerView` in der Anzeige zuständig. er bestimmt den Präsentationstyp (eine Liste oder ein Raster), die Ausrichtung (ob Elemente vertikal oder horizontal angezeigt werden) und welche Richtungs Elemente angezeigt werden sollen. (in normaler Reihenfolge oder in umgekehrter Reihenfolge). Der LayoutManager ist auch für die Berechnung der Größe und Position der einzelnen Elemente in der Wiederverwendung von " **recycleview** " verantwortlich.

Der Layout-Manager hat einen zusätzlichen Zweck: er bestimmt die Richtlinie, wann Element Sichten wieder verwendet werden sollen, die für den Benutzer nicht mehr sichtbar sind.
Da der LayoutManager weiß, welche Sichten sichtbar sind (und welche nicht), ist er am besten geeignet, um zu entscheiden, wann eine Ansicht wieder verwendet werden kann. Zum wieder verwenden einer Ansicht führt der LayoutManager in der Regel Aufrufe an den Adapter aus, um den Inhalt einer wiederverwendeten Sicht durch andere Daten zu ersetzen, wie zuvor unter [Funktionsweise der Ansicht](#recycling)"Wiederverwendung" beschrieben.

Sie können erweitern `RecyclerView.LayoutManager` , um einen eigenen LayoutManager zu erstellen, oder Sie können einen vordefinierten Layoutmanager verwenden. `RecyclerView`stellt die folgenden vordefinierten LayoutManager bereit:

- **`LinearLayoutManager`** &ndash; Ordnet Elemente in einer Spalte an, die vertikal oder in einer Zeile, für die ein horizontaler Bildlauf ausgeführt werden kann, ausgeführt werden kann.

- **`GridLayoutManager`** &ndash; Zeigt Elemente in einem Raster an.

- **`StaggeredGridLayoutManager`** &ndash; Zeigt Elemente in einem gestaffelten Raster an, in dem einige Elemente über unterschiedliche Höhen und breiten verfügen.

Um den LayoutManager anzugeben, instanziieren Sie den ausgewählten Layoutmanager, und `SetLayoutManager` übergeben Sie ihn an die-Methode. Beachten Sie, dass Sie angeben *müssen* , &ndash; dass der LayoutManager `RecyclerView` standardmäßig keinen vordefinierten LayoutManager auswählt.

Weitere Informationen zum LayoutManager finden Sie in der [Klassenreferenz "recyclerview. LayoutManager](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.LayoutManager.html)".

## <a name="the-view-holder"></a>Der Ansichts Halter

Der Ansichts Halter ist eine Klasse, die Sie für das Zwischenspeichern von Ansichts verweisen definieren. Der Adapter verwendet diese Ansichts Verweise, um jede Ansicht an ihren Inhalt zu binden. Jedes Element in der `RecyclerView` verfügt über eine zugeordnete Ansichts Inhaber Instanz, die die Ansichts Verweise für dieses Element zwischenspeichert. Verwenden Sie zum Erstellen eines Ansichts Besitzers die folgenden Schritte, um eine Klasse zu definieren, die den genauen Satz von Sichten pro Element enthalten soll:

1. Unterklasse `RecyclerView.ViewHolder`.
2. Implementieren Sie einen Konstruktor, der die Ansichts Verweise sucht und speichert.
3. Implementieren Sie Eigenschaften, die der Adapter für den Zugriff auf diese Verweise verwenden kann.

Ein ausführliches Beispiel für `ViewHolder` eine-Implementierung wird in [einem grundlegenden recyclerview-Beispiel](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)dargestellt.
Weitere Informationen zu `RecyclerView.ViewHolder`finden Sie in der [Klasse "recyclerview. viewholder](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html)".

## <a name="the-adapter"></a>Der Adapter

Der größte Teil des- `RecyclerView` Integrations Codes erfolgt im Adapter. `RecyclerView`erfordert, dass Sie einen von `RecyclerView.Adapter` abgeleiteten Adapter angeben, um auf die Datenquelle zuzugreifen und jedes Element mit Inhalt aus der Datenquelle aufzufüllen.
Da die Datenquelle App-spezifisch ist, müssen Sie Adapter Funktionen implementieren, die den Zugriff auf Ihre Daten verstehen. Der Adapter extrahiert Informationen aus der Datenquelle und lädt Sie in jedes Element in der `RecyclerView` Auflistung.

In der folgenden Abbildung wird veranschaulicht, wie der Adapter Inhalt in einer Datenquelle mithilfe von Ansichts Besitzern den einzelnen Ansichten innerhalb der `RecyclerView`einzelnen Zeilen Elemente in der zuordnet:

[![Diagramm zur Veranschaulichung des Adapters zum Verbinden der Datenquelle mit viewholders](parts-and-functionality-images/03-recyclerviewer-adapter-sml.png)](parts-and-functionality-images/03-recyclerviewer-adapter.png#lightbox)

Der Adapter lädt jede `RecyclerView` Zeile mit Daten für ein bestimmtes Zeilen Element. Bei Zeilen Position *p*sucht der Adapter z. b. die zugeordneten Daten an Position *p* innerhalb der Datenquelle und kopiert diese Daten in das Zeilen Element an Position *p* in der `RecyclerView` Auflistung.
In der obigen Zeichnung verwendet der Adapter z. b. den Ansichts Halter, um die Verweise für `ImageView` und `TextView` an dieser Position zu suchen, sodass er für diese Ansichten `FindViewById` nicht wiederholt aufrufen muss, wenn der Benutzer einen Bildlauf durch die Auflistung durchführt und verwendet Sichten erneut.

Wenn Sie einen Adapter implementieren, müssen Sie die folgenden `RecyclerView.Adapter` Methoden außer Kraft setzen:

- **`OnCreateViewHolder`** &ndash; Instanziiert die elementlayoutdatei und den Ansichts Halter.

- **`OnBindViewHolder`** &ndash; Lädt die Daten an der angegebenen Position in die Sichten, deren Verweise im angegebenen Ansichts Halter gespeichert werden.

- **`ItemCount`** &ndash; Gibt die Anzahl der Elemente in der Datenquelle zurück.

Der Layout-Manager ruft diese Methoden auf, während Elemente innerhalb von `RecyclerView`positioniert werden.

## <a name="notifying-recyclerview-of-data-changes"></a>Benachrichtigen von "recyclerview" von Datenänderungen

`RecyclerView`die Anzeige wird nicht automatisch aktualisiert, wenn sich der Inhalt der Datenquelle ändert. der Adapter muss Benachrichtigen `RecyclerView` , wenn das Dataset geändert wurde. Das DataSet kann in vielerlei Hinsicht geändert werden. Beispielsweise kann sich der Inhalt in einem Element ändern, oder die allgemeine Struktur der Daten kann geändert werden.
`RecyclerView.Adapter`stellt eine Reihe von Methoden bereit, die Sie so ausführen `RecyclerView` können, dass auf Datenänderungen auf möglichst effiziente Weise reagiert:

- **`NotifyItemChanged`** &ndash; Signalisiert, dass sich das Element an der angegebenen Position geändert hat.

- **`NotifyItemRangeChanged`** &ndash; Signalisiert, dass sich die Elemente im angegebenen Bereich von Positionen geändert haben.

- **`NotifyItemInserted`** &ndash; Signalisiert, dass das Element an der angegebenen Position neu eingefügt wurde.

- **`NotifyItemRangeInserted`** &ndash; Signalisiert, dass die Elemente im angegebenen Bereich von Positionen neu eingefügt wurden.

- **`NotifyItemRemoved`** &ndash; Signalisiert, dass das Element an der angegebenen Position entfernt wurde.

- **`NotifyItemRangeRemoved`** &ndash; Signalisiert, dass die Elemente im angegebenen Bereich von Positionen entfernt wurden.

- **`NotifyDataSetChanged`** &ndash; Signalisiert, dass sich der Datensatz geändert hat (erzwingt ein vollständiges Update).

Wenn Sie genau wissen, wie sich das Dataset geändert hat, können Sie die entsprechenden Methoden oben zum `RecyclerView` aktualisieren auf die effizienteste Weise aufzurufen. Wenn Sie nicht genau wissen, wie sich das Dataset geändert hat, können Sie `NotifyDataSetChanged`aufrufen, was weitaus weniger effizient ist `RecyclerView` , da alle Sichten aktualisieren muss, die für den Benutzer sichtbar sind. Weitere Informationen zu diesen Methoden finden Sie unter [recyclerview. Adapter](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html).

Im nächsten Thema, [einem grundlegenden recyclerview-Beispiel](~/android/user-interface/layouts/recycler-view/recyclerview-example.md), wird eine Beispiel-App implementiert, um echte Codebeispiele für die oben beschriebenen Teile und Funktionen zu veranschaulichen.

## <a name="related-links"></a>Verwandte Links

- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [Ein grundlegendes recyclerview-Beispiel](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [Erweitern des recyclerview-Beispiels](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
