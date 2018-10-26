---
title: RecyclerView-Komponenten und Funktionen
description: Eine Übersicht über die RecyclerView-Layout-Manager, Adapter und Inhaber der Ansicht.
ms.prod: xamarin
ms.assetid: 54F999BE-2732-4BC7-A466-D17373961C48
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/13/2018
ms.openlocfilehash: 13678d3b1bca102e6f608ad1c11838db1f14cd08
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104998"
---
# <a name="recyclerview-parts-and-functionality"></a>RecyclerView-Komponenten und Funktionen


`RecyclerView` Handles manche tasks intern (z. B. das Durchführen eines Bildlaufs und Wiederverwendung von Ansichten), aber es ist im Wesentlichen Manager, der die Hilfsklassen, um eine Sammlung anzeigen, koordiniert. `RecyclerView` die folgenden Hilfsklassen Delegaten Aufgaben:

-   **`Adapter`** &ndash; Vergrößert dieses Element Layouts (instanziiert den Inhalt einer Datei Layout) und bindet Daten an Ansichten, die in angezeigt werden eine `RecyclerView`. Der Adapter meldet auch die Element-Click-Ereignisse.

-   **`LayoutManager`** &ndash; Misst und positioniert die Element-Ansichten innerhalb einer `RecyclerView` und verwaltet die Richtlinie für die Ansicht wiederverwenden.

-   **`ViewHolder`** &ndash; Sucht und speichert die Sicht verweisen. Der Inhaber der Ansicht hilft auch Elementansicht Klicks erkennen zu können.

-   **`ItemDecoration`** &ndash; Ermöglicht eine app, die spezielle Funktionen zum Zeichnen und Layout-Offsets auf bestimmte Ansichten für das Zeichnen von Unterteiler zwischen Elementen, Höhepunkte und visuelle Gruppierung Grenzen hinzufügen.

-   **`ItemAnimator`** &ndash; Definiert die Animationen, die stattfinden, während der Aktionen für Elemente oder als Änderungen an den Adapter gestellt werden.

Die Beziehung zwischen der `RecyclerView`, `LayoutManager`, und `Adapter` Klassen ist in der folgenden Abbildung dargestellt:

![Diagramm der RecyclerView LayoutManager enthält, mithilfe des Adapters auf Daten zugreifen](parts-and-functionality-images/01-recyclerview-diagram.png)

Wie in dieser Abbildung wird veranschaulicht, die `LayoutManager` kann als Vermittler zwischen betrachtet werden die `Adapter` und `RecyclerView`. Die `LayoutManager` aufruft `Adapter` Methoden für die `RecyclerView`. Z. B. die `LayoutManager` Aufrufe einer `Adapter` Methode, wenn es Zeit, erstellen eine neue Ansicht für ein bestimmtes Elementposition in der `RecyclerView`. Die `Adapter` vergrößert das Layout für das betreffende Element und erstellt eine `ViewHolder` Instanz (nicht gezeigt), um Verweise auf die Ansichten an dieser Position zwischenzuspeichern. Wenn die `LayoutManager` Aufrufe der `Adapter` um ein bestimmtes Element auf das Dataset zu binden der `Adapter` sucht die Daten für das Element, aus dem DataSet abgerufen und in der Ansicht zugeordnete Element kopiert.

Bei Verwendung `RecyclerView` in Ihrer app das Erstellen von abgeleiteten Typen in der folgenden Klassen ist erforderlich:

-   **`RecyclerView.Adapter`** &ndash; Bietet eine Bindung aus Ihrer app-DataSet (die spezifisch für Ihre app) für Element-Sichten, die in angezeigt werden die `RecyclerView`. Der Adapter weiß, wie jede Elementansicht Position im Zuordnen der `RecyclerView` zu einer bestimmten Stelle in der Datenquelle. Darüber hinaus wird der Adapter behandelt das Layout des Inhalts in jedes einzelne Element anzeigen und den Inhaber der Ansicht für jede Ansicht erstellt. Der Adapter meldet auch die Element-Click-Ereignisse, die durch die Elementansicht erkannt werden.

-   **`RecyclerView.ViewHolder`** &ndash; Verweise auf die Ansichten in der Layoutdatei Element zwischengespeichert, sodass Ressourcensuche nicht unnötigerweise wiederholt werden. Der Inhaber der Sicht ordnet auch für Element-Click-Ereignisse an den Adapter weitergeleitet werden, wenn ein Benutzer die Ansicht des Karteninhabers zugeordneten Elementansicht tippt.

-   **`RecyclerView.LayoutManager`** &ndash; Platziert Elemente in der `RecyclerView`. Können Sie eine der vordefinierten Layout-Managern, die mehrere, oder Sie können Ihre eigenen benutzerdefinierten Layout-Manager implementieren.
    `RecyclerView` Delegaten ändert die Layoutrichtlinie auf der Layout-Manager, damit Sie in einem anderen Layout-Manager integrieren können, ohne wichtige in Ihrer app.

Darüber hinaus können Sie optional die folgenden Klassen, um das Aussehen und Verhalten ändern erweitern `RecyclerView` in Ihrer app:

-   **`RecyclerView.ItemDecoration`**
-   **`RecyclerView.ItemAnimator`**

Wenn Sie nicht erweitern `ItemDecoration` und `ItemAnimator`, `RecyclerView` verwendet Standard-Implementierungen. Dieses Handbuch wird nicht erläutert, wie zur Erstellung benutzerdefinierter `ItemDecoration` und `ItemAnimator` Klassen; Weitere Informationen zu diesen Klassen finden Sie unter [RecyclerView.ItemDecoration](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemDecoration.html) und [RecyclerView.ItemAnimator](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemAnimator.html).


<a name="recycling" />

### <a name="how-view-recycling-works"></a>Anzeige von Recycling funktioniert

`RecyclerView` die Ansicht ein Element nicht für jedes Element in der Datenquelle reserviert ist. Stattdessen sie weist nur die Anzahl der Item-Sichten, die auf dem Bildschirm zu passen, und die Element-Layouts, wie die Benutzer einen Bildlauf wiederverwendet. Wenn die Ansicht zuerst wieder eingeblendet einen Bildlauf durchführt, durchläuft er des wiederverwendungsprozesses in der folgenden Abbildung dargestellt:

[![Diagramm zur Veranschaulichung die sechs Schritte der Wiederverwendung anzeigen](parts-and-functionality-images/02-view-recycling-sml.png)](parts-and-functionality-images/02-view-recycling.png#lightbox)

1.  Wenn eine Sicht führt einen sichtbar Bildlauf und wird nicht mehr angezeigt, es wird eine *Ausschuss Ansicht*.

2.  Die Ansicht Ausschuss befindet sich in einem Pool und wird ein *Ansicht wiederverwenden*.
    Dieser Pool ist ein Cache von Ansichten, in denen die gleiche Art von Daten angezeigt.

3.  Wenn ein neues Element ist, die angezeigt werden, stammen aus dem Papierkorb-Pool für die Wiederverwendung eine Ansicht. Da in dieser Ansicht vom Adapter erneut gebunden werden muss, bevor er angezeigt wird, heißt es eine *modifizierte Ansicht*.

4.  Die Ansicht geänderte wird wiederverwendet: der Adapter sucht die Daten für das nächste Element angezeigt werden und werden die Daten zu den Ansichten für dieses Element kopiert. Verweise für diese Ansichten werden aus den Inhaber der Ansicht mit der Wiederverwendung Ansicht verknüpfte abgerufen.

5.  Die wiederverwendete Ansicht wird hinzugefügt, um die Liste der Elemente in der `RecyclerView` , die im Begriff sind auf dem Bildschirm zu wechseln.

6.  Die Ansicht wiederverwendete wird auf dem Bildschirm wie der Benutzer einen Bildlauf der `RecyclerView` zum nächsten Element in der Liste. In der Zwischenzeit eine andere Ansicht führt einen Bildlauf sichtbar und entsprechend den oben genannten Schritten wiederverwendet wird.

Zusätzlich zu Elementansicht Wiederverwendung `RecyclerView` verwendet auch eine andere Optimierungsmethode für Effizienz: Platzhalter anzuzeigen. Ein *Ansicht Inhaber* ist eine einfache Klasse, dass die Caches Verweise anzuzeigen. Jedes Mal, die der Adapter eine Element-Layout-Datei, vergrößert wird auch einen entsprechende Ansicht Inhaber. Wird verwendet, der Inhaber der Ansicht `FindViewById` um Verweise auf die Ansichten in der Datei vergrößerte Element-Layout zu erhalten. Diese Verweise werden verwendet, um neue Daten in den Ansichten zu laden, jedes Mal, wenn das Layout wiederverwendet werden, um neue Daten angezeigt wird.
 


### <a name="the-layout-manager"></a>Der Layout-Manager

Der Layout-Manager ist verantwortlich für die Positionierung von Elementen in der `RecyclerView` anzeigen; es bestimmt den Darstellungstyp (eine Liste oder einem Raster), Ausrichtung (gibt an, ob Elemente werden angezeigt, vertikal oder horizontal) und welche Richtung Elemente angezeigt werden sollen (in der normalen Reihenfolge oder in umgekehrter Reihenfolge). Der Layout-Manager ist auch zuständig für die Berechnung der Größe und Position jedes Elements in der **RecycleView** anzuzeigen.

Der Layout-Manager ist einen weiteren Zweck: bestimmt die Richtlinie für die Element-Ansichten wiederverwendet werden, die nicht mehr für den Benutzer sichtbar sind.
Da der Layout-Manager über die Ansichten angezeigt werden (und welche nicht), ist es in der Lage zu entscheiden, wenn eine Ansicht wiederverwendet werden können. Um eine Ansicht wiederverwenden möchten, Aufrufe an den Adapter, um den Inhalt einer wiederverwendet Ansicht mit unterschiedlichen Daten ersetzen der Layout-Manager in der Regel ist, wie zuvor beschrieben in [Ansicht wiederverwenden Funktionsweise](#recycling).

Sie können die erweitern `RecyclerView.LayoutManager` erstellen Ihr eigenes Layout-Manager, oder Sie können einen vordefinierten Layout-Manager. `RecyclerView` bietet die folgenden vordefinierten Layout-Manager:

-   **`LinearLayoutManager`** &ndash; Ordnet die Elemente in einer Spalte, die ein vertikaler Bildlauf durchgeführt werden kann oder in einer Zeile, die ein horizontaler Bildlauf durchgeführt werden kann.

-   **`GridLayoutManager`** &ndash; Elemente in einem Raster angezeigt.

-   **`StaggeredGridLayoutManager`** &ndash; Zeigt Elemente in einem gestaffelte Raster, in dem einige Elemente unterschiedlicher Höhe und Breite verfügen.

Klicken Sie zum Angeben des Layout-Managers instanziieren Ihrer ausgewählten Layout-Manager, und übergeben Sie sie an der `SetLayoutManager` Methode. Beachten Sie, den Sie *müssen* festlegen den Layout-Manager &ndash; `RecyclerView` einen vordefiniertes Layout-Manager wird nicht standardmäßig ausgewählt.

Weitere Informationen zu der Layout-Manager, finden Sie unter den [RecyclerView.LayoutManager Klassenverweis](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.LayoutManager.html).


### <a name="the-view-holder"></a>Der Inhaber der Ansicht

Die Ansicht besitzt eine Klasse, die Sie definieren, für die Zwischenspeicherung der Sicht verweisen. Der Adapter verwendet diese Verweise Anzeigen einer Ansicht jeweils auf seinen Inhalt zu binden. Jedes Element in der `RecyclerView` verfügt über eine zugeordnete Ansicht Inhaber der Instanz, die die Ansicht Verweise für das betreffende Element zwischenspeichert. Um eine Inhaber der Ansicht zu erstellen, gehen Sie zum Definieren einer Klasse zum Halten des genauen Satz von Ansichten pro Element:

1.  Unterklasse `RecyclerView.ViewHolder`.
2.  Implementieren Sie einen Konstruktor, der sucht und speichert die Verweise anzeigen.
3.  Implementieren Sie die Eigenschaften, die den Adapter verwenden können, auf diese verweisen.

Ein ausführliches Beispiel einer `ViewHolder` Implementierung werden im [ein einfaches RecyclerView-Beispiel](~/android/user-interface/layouts/recycler-view/recyclerview-example.md).
Weitere Informationen zu `RecyclerView.ViewHolder`, finden Sie unter den [RecyclerView.ViewHolder Klassenverweis](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html).


### <a name="the-adapter"></a>Der Adapter

Die meisten der "anstrengende" die `RecyclerView` Integrationscode findet im Adapter. `RecyclerView` erfordert, dass Sie einen Adapter, die von abgeleiteten bereitstellen `RecyclerView.Adapter` auf Ihre Datenquelle zugreifen, und füllen alle Elemente mit Inhalten aus der Datenquelle.
Da die Datenquelle app-spezifisch ist, müssen Sie die Funktionalität von Adaptern implementieren, die auf Ihre Daten zugreifen kann. Der Adapter extrahiert Informationen aus der Datenquelle und lädt sie in der jedes Element in der `RecyclerView` Auflistung.

In der folgende Zeichnung wird veranschaulicht, wie der Adapter Inhalte in einer Datenquelle über die Ansicht werden einzelne Ansichten in jedem Zeilenelement in zuordnet, die `RecyclerView`:

[![Diagramm zur Veranschaulichung ViewHolders-Datenquelle mit Adapter](parts-and-functionality-images/03-recyclerviewer-adapter-sml.png)](parts-and-functionality-images/03-recyclerviewer-adapter.png#lightbox)

Der Adapter lädt jede `RecyclerView` Zeile mit Daten für eine bestimmte Zeile-Element. Für die Zeilenposition *P*, z. B. der Adapter auch die zugeordneten Daten an der Position *P* innerhalb der Datenquelle und kopiert diese auf die Zeile Datenelement an Position *P* in der `RecyclerView` Auflistung.
Aus dem oben abgebildeten, z. B. der Adapter verwendet den Inhaber der anzeigen, suchen die Verweise für die `ImageView` und `TextView` an dieser Position aus, sodass es nicht wiederholten Aufrufen `FindViewById` für diese Ansichten als der Benutzer scrollt durch die Auflistung und Ansichten wird wiederverwendet.

Wenn Sie einen Adapter implementieren, müssen Sie die folgenden überschreiben `RecyclerView.Adapter` Methoden:

-   **`OnCreateViewHolder`** &ndash; Instanziiert die Datei und Ansicht Inhaber der Element-Layout.

-   **`OnBindViewHolder`** &ndash; Lädt die Daten an der angegebenen Position in den Ansichten, deren Verweise in den Inhaber der angegebenen Ansicht gespeichert sind.

-   **`ItemCount`** &ndash; Gibt die Anzahl der Elemente in der Datenquelle zurück.

Diese Methoden werden von der Layout-Manager aufgerufen, während sie Elemente in Positionierung ist die `RecyclerView`. 



### <a name="notifying-recyclerview-of-data-changes"></a>Benachrichtigen RecyclerView von Datenänderungen

`RecyclerView` wird nicht automatisch die Anzeige aktualisiert, wenn der Inhalt der Daten Änderungen Datenquelle; der Adapter muss benachrichtigen `RecyclerView` Wenn eine Änderung im DataSet vorhanden ist. Das DataSet kann in vielerlei Hinsicht ändern. Beispielsweise können die Inhalte innerhalb eines Elements ändern, oder die allgemeine Struktur der Daten geändert werden kann.
`RecyclerView.Adapter` bietet eine Reihe von Methoden, die Sie aufrufen können, damit `RecyclerView` reagiert auf datenänderungen in die effizienteste Weise:

-  **`NotifyItemChanged`** &ndash; Signalisiert, die das Element an der angegebenen Position geändert wurde.

-  **`NotifyItemRangeChanged`** &ndash; Signalisiert, die die Elemente im angegebenen Bereich von Positionen geändert haben.

-  **`NotifyItemInserted`** &ndash; Signalisiert, dass das Element in der angegebenen Position neu eingefügt wurde.

-  **`NotifyItemRangeInserted`** &ndash; Signalisiert, dass die Elemente im angegebenen Bereich von Positionen neu eingefügt wurden.

-  **`NotifyItemRemoved`** &ndash; Signalisiert, dass das Element in der angegebenen Position entfernt wurde.

-  **`NotifyItemRangeRemoved`** &ndash; Signalisiert, dass die Elemente im angegebenen Bereich von Positionen entfernt wurden.

-  **`NotifyDataSetChanged`** &ndash; Signale, die das Dataset geändert wurde (erzwingt eine vollständige Aktualisierung).

Wenn Sie genau, wie das Dataset geändert hat wissen, können Sie die entsprechenden Methoden aus, um die aktualisieren aufrufen `RecyclerView` auf möglichst effiziente Weise. Wenn Sie nicht wissen, wie genau das Dataset geändert hat, können Sie aufrufen `NotifyDataSetChanged`, dies ist weit weniger effizient da `RecyclerView` müssen aktualisieren, alle Ansichten, die für den Benutzer sichtbar sind. Weitere Informationen zu diesen Methoden finden Sie unter [RecyclerView.Adapter](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html).

Im nächsten Thema [ein einfaches RecyclerView-Beispiel](~/android/user-interface/layouts/recycler-view/recyclerview-example.md), eine Beispiel-app wird implementiert, um die Komponenten und Funktionen, die oben beschriebenen echte Codebeispielen wird veranschaulicht.


## <a name="related-links"></a>Verwandte Links

- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [Ein einfaches RecyclerView-Beispiel](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [Erweitern Sie im RecyclerView-Beispiel](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
