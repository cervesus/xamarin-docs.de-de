---
title: "RecyclerView Teile und Funktionalität"
ms.topic: article
ms.prod: xamarin
ms.assetid: 54F999BE-2732-4BC7-A466-D17373961C48
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: b1ddcca25fd83a806e8383a5717462b518b46d0b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="recyclerview-parts-and-functionality"></a>RecyclerView Teile und Funktionalität


`RecyclerView` Handles manche tasks intern (z. B. das Durchführen eines Bildlaufs und Wiederverwendung von Sichten), aber es ist im Wesentlichen eine-Manager, der zur Koordinierung von Hilfsklassen, um eine Auflistung anzuzeigen. `RecyclerView` die folgenden Hilfsklassen Delegaten Aufgaben:

-   **`Adapter`** &ndash; Vergrößert dieses Element Layouts (instanziiert den Inhalt einer Datei Layout) und bindet Daten für Sichten, die in angezeigt werden eine `RecyclerView`. Der Adapter übermittelt außerdem die Element-Click-Ereignisse.

-   **`LayoutManager`** &ndash; Misst und positioniert Item Sichten innerhalb einer `RecyclerView` und verwaltet die Richtlinie für die Wiederverwendung der Ansicht.

-   **`ViewHolder`** &ndash; Sucht und speichert Verweise anzeigen. Der Inhaber der Ansicht trägt auch zur erkennen Ansicht Element klickt.

-   **`ItemDecoration`** &ndash; Ermöglicht einer app auf bestimmte Ansichten für das Zeichnen von Unterteiler zwischen Elementen, Highlights und visuelle Gruppierung Grenzen spezielle zeichnen und Layout Offsets hinzu.

-   **`ItemAnimator`** &ndash; Definiert die Animationen an, die stattfinden, während der Aktionen für Elemente oder als Änderungen an den Adapter gestellt werden.

Die Beziehung zwischen der `RecyclerView`, `LayoutManager`, und `Adapter` Klassen wird in der folgenden Abbildung dargestellt:

![Diagramm der RecyclerView LayoutManager enthält, mithilfe der Adapter auf Daten zuzugreifen](parts-and-functionality-images/01-recyclerview-diagram.png)

Wie in dieser Abbildung verdeutlicht, die `LayoutManager` kann als Vermittler zwischen betrachtet werden die `Adapter` und `RecyclerView`. Die `LayoutManager` führt Aufrufe `Adapter` Methoden auf die `RecyclerView`. Z. B. die `LayoutManager` Aufrufe einer `Adapter` Methode, wenn es Zeit zum Erstellen einer neuen Ansicht für eine Position eines bestimmten Elements in der `RecyclerView`. Die `Adapter` vergrößert das Layout für dieses Element aus, und erstellt eine `ViewHolder` Instanz (nicht gezeigt), um Verweise auf die Ansichten an dieser Position zwischenzuspeichern. Wenn die `LayoutManager` Aufrufe der `Adapter` ein bestimmtes Element auf das DataSet binden der `Adapter` sucht die Daten für dieses Element, aus dem DataSet abgerufen und kopiert ihn in der Ansicht zugeordnete Element.

Bei Verwendung `RecyclerView` in Ihrer app das Erstellen von abgeleiteten Typen die folgenden Klassen ist erforderlich:

-   **`RecyclerView.Adapter`** &ndash; Bietet eine Bindung aus der app-DataSet (die speziell auf Ihre app ist) für Element-Sichten, die in angezeigt werden die `RecyclerView`. Der Adapter weiß, wie jede Elementansicht Position im Zuordnen der `RecyclerView` an einem bestimmten Speicherort in der Datenquelle. Darüber hinaus wird der Adapter behandelt das Layout des Inhalts in jeder einzelnen Elementansicht und den Inhaber der Ansicht für jede Ansicht erstellt. Der Adapter übermittelt außerdem die Element-Click-Ereignisse, die durch die Elementansicht erkannt werden.

-   **`RecyclerView.ViewHolder`** &ndash; Verweise auf die Ansichten in der Layoutdatei Element zwischenspeichert, sodass Ressourcensuche nicht unnötigerweise wiederholt werden. Der Inhaber der Sicht ordnet auch für Element-Click-Ereignisse an den Adapter weitergeleitet werden, wenn ein Benutzer der anzeigen-Inhaber zugeordneten Elementansicht tippt.

-   **`RecyclerView.LayoutManager`** &ndash; Positioniert die Elemente innerhalb der `RecyclerView`. Können Sie eine von mehreren vordefinierten Layout-Managern, oder Sie können eigene benutzerdefinierte Layout-Manager implementieren.
    `RecyclerView` Delegaten ändert die Layoutrichtlinie der Layout-Manager, in ein anderes Layout-Manager eingebunden werden kann, ohne signifikante Stellen in Ihrer app.

Darüber hinaus können Sie optional die folgenden Klassen, um das Aussehen und Verhalten zu ändern erweitern `RecyclerView` in Ihrer app:

-   **`RecyclerView.ItemDecoration`**
-   **`RecyclerView.ItemAnimator`**

Wenn Sie nicht erweitern `ItemDecoration` und `ItemAnimator`, `RecyclerView` standardimplementierungen für verwendet. Dieses Handbuch wird nicht erläutert, wie zur Erstellung benutzerdefinierter `ItemDecoration` und `ItemAnimator` Klassen; Weitere Informationen zu diesen Klassen finden Sie unter [RecyclerView.ItemDecoration](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemDecoration.html) und [RecyclerView.ItemAnimator](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemAnimator.html).


<a name="recycling" />

### <a name="how-view-recycling-works"></a>Anzeige von Works wiederverwenden

`RecyclerView` die Ansicht ein Element nicht für jedes Element in der Datenquelle reserviert ist. Stattdessen es weist nur die Anzahl der Element-Sichten, die auf den Bildschirm passen, und diese Layouts Element als der Benutzer einen Bildlauf wiederverwendet. Wenn die Ansicht ausgeblendet zuerst einen Bildlauf, durchläuft er des wiederverwendungsprozesses in der folgenden Abbildung veranschaulicht:

[ ![Diagramm zur Veranschaulichung die sechs Schritte der Wiederverwendung anzeigen](parts-and-functionality-images/02-view-recycling-sml.png)](parts-and-functionality-images/02-view-recycling.png)

1.  Wenn eine Sicht führt einen ausgeblendet Bildlauf und nicht mehr angezeigt wird, wird eine *Ausschuss Ansicht*.

2.  Die Ausschuss-Ansicht in einem Pool befindet, und wird eine *wiederverwenden Ansicht*.
    Dieser Pool ist ein Cache von Sichten, die den gleichen Typ der Daten anzuzeigen.

3.  Wird ein neues Element angezeigt werden, wird eine Sicht aus dem Papierkorb Pool zur Wiederverwendung übernommen. Da in dieser Ansicht vom Adapter erneut gebunden werden muss, bevor Sie angezeigt wird, heißt es eine *modifizierte Seiten Ansicht*.

4.  Die modifizierte Ansicht wird wiederverwendet: der Adapter sucht die Daten für das nächste Element angezeigt werden und werden die Daten auf die Ansichten für dieses Element kopiert. Verweise für diese Sichten werden aus den Inhaber der Ansicht der Wiederverwendung Sicht zugeordneten abgerufen.

5.  Die wiederverwendete Ansicht wird hinzugefügt, um die Liste der Elemente in der `RecyclerView` , sind im Begriff, die auf dem Bildschirm wechseln.

6.  Die Sicht wiederverwendete wird auf dem Bildschirm als der Benutzer einen Bildlauf der `RecyclerView` zum nächsten Element in der Liste. In der Zwischenzeit, einer anderen Ansicht scrollt ausgeblendet und entsprechend der oben genannten Schritte wiederverwendet wird.

Zusätzlich zu den Elementansicht Wiederverwendung `RecyclerView` verwendet auch einen anderen Optimierung der Effizienz: Inhaber anzeigen. Ein *Ansicht Inhaber* ist eine einfache Klasse von Caches Verweise angezeigt werden. Jedes Mal, die der Adapter eine Element-Layout-Datei vergrößert wird erstellt auch einen entsprechenden Inhaber der Ansicht. Verwendet der Inhaber der Ansicht `FindViewById` um Verweise auf die Ansichten in der Datei vergrößerte Elementlayout abzurufen. Diese Verweise werden verwendet, um neue Daten in den Ansichten laden, jedes Mal, wenn das Layout wiederverwendet werden, um neue Daten anzeigt wird.
 

<a name="layoutmanager" />

### <a name="the-layout-manager"></a>Der Layout-Manager

Der Layout-Manager ist verantwortlich für Positionieren von Elementen in der `RecyclerView` angezeigt; es bestimmt der Präsentation-Typ (eine Liste oder einem Raster), die Ausrichtung (gibt an, ob Elemente werden angezeigt, vertikal oder horizontal) und welche Richtung Elemente angezeigt werden sollen (in der normalen Reihenfolge oder in umgekehrter Reihenfolge). Der Layout-Manager dient auch zum Berechnen der Größe und Position der einzelnen Elemente in der **RecycleView** anzuzeigen.

Der Layout-Manager hat einen zusätzlichen Zweck: es feststellt, dass die Richtlinie für den Fall Item Sichten wiederverwendet, die nicht mehr für den Benutzer sichtbar sind.
Der Layout-Manager, welche Ansichten angezeigt werden (und welche ignoriert werden) zu beachten, ist es in der Lage zu entscheiden, wenn eine Sicht wiederverwendet werden kann. Um eine Ansicht wiederzuverwenden, der Layout-Manager in der Regel aufruft an den Adapter ersetzen Sie den Inhalt einer wiederverwendet Sicht mit unterschiedlichen Daten wie im zuvor beschriebenen [Ansicht Wiederverwendung Funktionsweise](#recycling).

Sie können erweitern `RecyclerView.LayoutManager` eigene Layouts Erstellen einer-Manager, oder Sie können einen vordefinierten Layout-Manager verwenden. `RecyclerView` bietet die folgenden vordefinierten LayoutManager:

-   **`LinearLayoutManager`** &ndash; Ordnet die Elemente in einer Spalte, die vertikal gescrollt werden kann, oder in einer Zeile, die horizontal gescrollt werden kann.

-   **`GridLayoutManager`** &ndash; Elemente in einem Raster angezeigt.

-   **`StaggeredGridLayoutManager`** &ndash; Zeigt Elemente in einem gestaffelten Raster, wobei einige Elemente unterschiedliche Höhe und Breite haben.

Klicken Sie zum Angeben des Layout-Managers instanziieren Sie den ausgewählten Layout-Manager, und übergeben sie an die `SetLayoutManager` Methode. Beachten Sie, die Sie *müssen* Geben Sie den Layout-Manager &ndash; `RecyclerView` einen vordefinierten Layout-Manager wird nicht standardmäßig ausgewählt.

Weitere Informationen zum Layout-Manager finden Sie unter der [RecyclerView.LayoutManager-Klassenreferenz](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.LayoutManager.html).

<a name="viewholder" />

### <a name="the-view-holder"></a>Der Inhaber der Ansicht

Die Sicht besitzt eine Klasse, die Sie definieren, für die Zwischenspeicherung der Sicht verweisen. Der Adapter verwendet diese Verweise anzeigen, um jede Ansicht zum jeweiligen Inhalt binden. Jedes Element in der `RecyclerView` verfügt über eine zugeordnete Ansicht Inhaber der Instanz, die die Sicht Verweise für dieses Element zwischenspeichert. Um eine Inhaber der Ansicht zu erstellen, gehen Sie zum Definieren einer Klasse zum Halten des exakten Satz an Ansichten pro Element:

1.  Unterklasse `RecyclerView.ViewHolder`.
2.  Implementieren Sie einen Konstruktor, der gesucht und speichert die Sicht verweisen.
3.  Implementieren Sie die Eigenschaften, mit denen der Adapter kann diese Verweise zugreifen.

Ein ausführliches Beispiel eine `ViewHolder` Implementierung erhält [ein einfaches RecyclerView Beispiel](~/android/user-interface/layouts/recycler-view/recyclerview-example.md).
Weitere Informationen zu `RecyclerView.ViewHolder`, finden Sie unter der [RecyclerView.ViewHolder-Klassenreferenz](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html).

<a name="adapter" />

### <a name="the-adapter"></a>Der Adapter

Die meisten der "anstrengende" die `RecyclerView` Integrationscode findet im Adapter. `RecyclerView` erfordert, dass Sie einen Adapter abgeleitet bereitstellen `RecyclerView.Adapter` auf Ihre Datenquelle zugreifen, und jedes Element mit dem Inhalt aus der Datenquelle aufgefüllt.
Da die Datenquelle anwendungsspezifischen ist, müssen Sie Adapterfunktionalität implementieren, die zum Zugreifen auf Ihre Daten zu verstehen. Der Adapter extrahiert Informationen aus der Datenquelle und lädt sie in jedes Element in der `RecyclerView` Auflistung.

In der folgende Zeichnung wird veranschaulicht, wie der Adapter Inhalt in einer Datenquelle über Sicht sind die Urheberrechtsinhaber für einzelne Sichten in jedem Zeilenelement in ordnet die `RecyclerView`:

[ ![Diagramm zur Veranschaulichung ViewHolders-Datenquelle mit Adapter](parts-and-functionality-images/03-recyclerviewer-adapter-sml.png)](parts-and-functionality-images/03-recyclerviewer-adapter.png)

Der Adapter lädt jedes `RecyclerView` Zeile mit Daten für eine bestimmte Zeilenelement. Für die Zeilenposition *P*, z. B. sucht der Adapter die zugeordneten Daten an der Position *P* innerhalb der Datenquelle und kopiert diese auf die Zeile Datenelement an Position *P* in der `RecyclerView` Auflistung.
In dem oben abgebildeten, z. B. der Adapter verwendet den Inhaber der Ansicht nachgeschlagen werden die Verweise für die `ImageView` und `TextView` an dieser Position, daher keine wiederholten Aufrufen müssen `FindViewById` für diesen Ansichten als der Benutzer führt einen Bildlauf durch die Auflistung und Sichten wird wiederverwendet.

Wenn Sie einen Adapter implementieren, müssen Sie die folgenden überschreiben `RecyclerView.Adapter` Methoden:

-   **`OnCreateViewHolder`** &ndash; Instanziiert den Inhaber für das Layout Element der Datei- und anzeigen.

-   **`OnBindViewHolder`** &ndash; Lädt die Daten der angegebenen Position in der Sichten, deren Verweise in den Inhaber der angegebenen Ansicht gespeichert sind.

-   **`ItemCount`** &ndash; Gibt die Anzahl der Elemente in der Datenquelle zurück.

Der Layout-Manager ruft diese Methoden beim Positionieren von Elementen in ist die `RecyclerView`. 


<a name="datachanges" />

### <a name="notifying-recyclerview-of-data-changes"></a>Benachrichtigen RecyclerView von Datenänderungen

`RecyclerView` wird nicht automatisch die Anzeige aktualisiert, wenn der Inhalt der Daten Änderungen Datenquelle; der Adapter muss benachrichtigen `RecyclerView` Wenn eine Änderung im DataSet vorhanden ist. Das DataSet können in vielerlei Hinsicht ändern. Beispielsweise können der Inhalt eines Elements ändern oder die allgemeine Struktur der Daten geändert werden kann.
`RecyclerView.Adapter` bietet eine Reihe von Methoden, die Sie aufrufen können, damit `RecyclerView` reagiert auf Änderungen der Daten in die effizienteste Art und Weise:

-  **`NotifyItemChanged`** &ndash; Signalisiert, die das Element an der angegebenen Position geändert wurde.

-  **`NotifyItemRangeChanged`** &ndash; Signalisiert, die die Elemente im angegebenen Bereich von Positionen geändert haben.

-  **`NotifyItemInserted`** &ndash; Signalisiert, dass das Element an der angegebenen Position neu eingefügt wurden.

-  **`NotifyItemRangeInserted`** &ndash; Signalisiert, dass die Elemente im angegebenen Bereich von Positionen neu eingefügt wurden.

-  **`NotifyItemRemoved`** &ndash; Signalisiert, dass das Element an der angegebenen Position entfernt wurde.

-  **`NotifyItemRangeRemoved`** &ndash; Signalisiert, dass die Elemente im angegebenen Bereich von Positionen entfernt wurden.

-  **`NotifyDataSetChanged`** &ndash; Signale, die das Dataset geändert wurde (erzwingt ein vollständiges Update).

Wenn Sie wissen, wie genau das Dataset geändert wurde, rufen Sie die entsprechenden Methoden, die oben beschriebenen aktualisieren `RecyclerView` in die effizienteste Art und Weise. Wenn Sie nicht kennen, genau wie das Dataset geändert wurde, können Sie aufrufen `NotifyDataSetChanged`, weit weniger effizient ist da `RecyclerView` muss aktualisiert werden alle Ansichten, die für den Benutzer sichtbar sind. Weitere Informationen zu diesen Methoden finden Sie unter [RecyclerView.Adapter](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html).

Im nächsten Thema [ein einfaches RecyclerView Beispiel](~/android/user-interface/layouts/recycler-view/recyclerview-example.md), eine Beispiel-app wird implementiert, um echte Codebeispielen die Teile und die Funktionalität, die oben beschriebenen.


## <a name="related-links"></a>Verwandte Links

- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [Ein einfaches RecyclerView-Beispiel](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [Erweitern Sie im RecyclerView-Beispiel](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
