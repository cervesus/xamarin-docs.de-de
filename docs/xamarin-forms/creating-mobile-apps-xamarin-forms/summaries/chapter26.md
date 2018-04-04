---
title: Zusammenfassung der Kapitel 26. Benutzerdefinierte layouts
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2B7F4346-414E-49FF-97FB-B85E92D98A21
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 5d2dc3e809026a36d186c9a582fcd047f6be24fe
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-26-custom-layouts"></a>Zusammenfassung der Kapitel 26. Benutzerdefinierte layouts

Xamarin.Forms enthält mehrere Klassen abgeleitet [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/):

* `StackLayout`,
* `Grid`,
* `AbsoluteLayout` und
* `RelativeLayout`

In diesem Kapitel wird beschrieben, wie Ihre eigenen Klassen erstellen, die Ableitung `Layout<View>`.

## <a name="an-overview-of-layout"></a>Eine Übersicht über layout

Es gibt keine zentrale System, das Layout von Xamarin.Forms behandelt. Jedes Element ist zuständig für das ermitteln, was eine eigene Größe werden sollen und wie selbst innerhalb eines bestimmten Gebiets gerendert werden.

### <a name="parents-and-children"></a>Übergeordnete und untergeordnete Elemente

Jedes Element, das über untergeordnete Elemente verfügt ist verantwortlich für die Positionierung dieser untergeordneten Elemente innerhalb des Workflows. Es ist das übergeordnete Element, die letztlich bestimmt, welche untergeordneten Größe sollte basieren auf der Größe verfügbar sind und möchte, dass die Größe der untergeordneten sein.

### <a name="sizing-and-positioning"></a>Ändern der Größe und Positionierung

Layout beginnt am oberen Rand der visuellen Struktur mit der Seite und dann fortgesetzt, bis alle Verzweigungen. Die wichtigste öffentliche Methode im Layout ist [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) durch definierten `VisualElement`. Jedes Element, das ein übergeordnetes Element anderer Elemente Aufrufe ist `Layout` für jede seiner untergeordneten Elemente, um dem untergeordnete Element zu gewähren, eine Größe und Position relativ zum selbst in Form einer [ `Rectangle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Rectangle/) Wert. Diese `Layout` Aufrufe über die visuelle Struktur weitergeben.

Ein Aufruf von `Layout` ist erforderlich für ein Element auf dem Bildschirm angezeigt werden und bewirkt, dass die folgenden schreibgeschützten Eigenschaften festgelegt werden. Sie sind konsistent mit der `Rectangle` an die Methode übergeben:

- [`Bounds`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Bounds/) vom Typ `Rectangle`
- [`X`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.X/) vom Typ `double`
- [`Y`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Y/) vom Typ `double`
- [`Width`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/) vom Typ `double`
- [`Height`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) vom Typ `double`

Vor der `Layout` aufzurufen, `Height` und `Width` simulierten Wert haben &ndash;1.

Ein Aufruf von `Layout` löst auch Aufrufe an den folgenden geschützten Methoden:

- [`SizeAllocated`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.SizeAllocated/p/System.Double/System.Double/), welche Aufrufe
- [`OnSizeAllocated`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnSizeAllocated/p/System.Double/System.Double/), die überschrieben werden können.

Schließlich wird das folgende Ereignis ausgelöst:

- [`SizeChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.SizeChanged/)

Die `OnSizeAllocated` Methode wird überschrieben, indem `Page` und `Layout`, wobei es sich um die nur zwei Klassen in Xamarin.Forms, die über untergeordnete Elemente verfügen können. Ruft die außer Kraft gesetzte Methode

- [`UpdateChildrenLayout`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.UpdateChildrenLayout()/) für `Page` ableitungen und [ `UpdateChildrenLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.UpdateChildrenLayout()/) für `Layout` ableitungen, der aufgerufen wird,
- [`LayoutChildren`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) für `Page` ableitungen und [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) für `Layout` ableitungen.

`LayoutChildren` Ruft dann `Layout` aller seiner untergeordneten Elemente. Verfügt über mindestens ein untergeordnetes Element ein neues `Bounds` festlegen, und klicken Sie dann das folgende Ereignis ausgelöst wird:

- [`LayoutChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.LayoutChanged/) für `Page` ableitungen und [ `LayoutChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Layout.LayoutChanged/) für `Layout` ableitungen

### <a name="constraints-and-size-requests"></a>Einschränkungen und Anforderungen der Größe

Für `LayoutChildren` Intelligent Aufrufen `Layout` auf alle untergeordneten benötigen sie ein *bevorzugte* oder *gewünschten* Größe für die untergeordneten Elemente. Aus diesem Grund die Aufrufe von `Layout` für jede der untergeordneten Elemente in der Regel durch Aufrufe von voranstehen

- [`GetSizeRequest`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.GetSizeRequest/p/System.Double/System.Double/)

Nachdem das Buch veröffentlicht wurde, die `GetSizeRequest` Methode wurde als veraltet markiert und ersetzt durch

- [`Measure`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/)

Die `Measure` Methode verfügt die [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) Eigenschaft und enthält ein Argument des Typs [ `MeasureFlag` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MeasureFlags/), die zwei Member aufweist:

- [`IncludeMargins`](https://developer.xamarin.com/api/field/Xamarin.Forms.MeasureFlags.IncludeMargins/)
- [`None`](https://developer.xamarin.com/api/field/Xamarin.Forms.MeasureFlags.None/) Ränder, nicht eingeschlossen.

Für viele Elemente `GetSizeRequest` oder `Measure` Ruft die systemeigene Größe des Elements aus der Renderer ab. Beide Methoden verfügen über Parameter für Breite und Höhe *Einschränkungen*. Z. B. eine `Label` Einschränkung für die Breite bestimmen, wie mehrere Textzeilen umschließen verwenden.

Beide `GetSizeRequest`und `Measure` einen Rückgabewert vom Typ [ `SizeRequest` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SizeRequest/), die über zwei Eigenschaften verfügt:

- [`Request`](https://developer.xamarin.com/api/property/Xamarin.Forms.SizeRequest.Request/) vom Typ `Size`
- [`Minimum`](https://developer.xamarin.com/api/property/Xamarin.Forms.SizeRequest.Minimum/) vom Typ `Size`

Sehr häufig diese zwei Werte identisch sind, und die `Minimum` Wert kann normalerweise ignoriert werden.

`VisualElement` definiert auch eine geschützte Methode, die ähnlich wie `GetSizeRequest` , die aus aufgerufen wird `GetSizeRequest`:

- [`OnSizeRequest`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnSizeRequest/p/System.Double/System.Double/) Gibt eine `SizeRequest` Wert

Diese Methode ist jetzt als veraltet markiert und ersetzt durch:

- [`OnMeasure`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/)

Jede Klasse, die abgeleitet `Layout` oder `Layout<T>` müssen überschreiben `OnSizeRequest` oder `OnMeasure`. Dies ist, in dem eine Layoutklasse eine eigene Größe bestimmt, welche die in der Regel auf die Größe der untergeordneten, die sie basiert durch den Aufruf abruft `GetSizeRequest` oder `Measure` in den untergeordneten Elementen. Vor und nach dem Aufruf `OnSizeRequest` oder `OnMeasure`, `GetSizeRequest` oder `Measure` macht Anpassungen basierend auf den folgenden Eigenschaften:

- [`WidthRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/)Der Typ `double`, wirkt sich auf die `Request` Eigenschaft `SizeRequest`
- [`HeightRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) Der Typ `double`, wirkt sich auf die `Request` Eigenschaft `SizeRequest`
- [`MinimumWidthRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.MinimumWidthRequest/) Der Typ `double`, wirkt sich auf die `Minimum` Eigenschaft `SizeRequest`
- [`MinimumHeightRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.MinimumHeightRequest/) Der Typ `double`, wirkt sich auf die `Minimum` Eigenschaft `SizeRequest`

### <a name="infinite-constraints"></a>Unbegrenzte Einschränkungen

Die Einschränkung Argumente zu übergeben, um `GetSizeRequest` (oder `Measure`) und `OnSizeRequest` (oder `OnMeasure`) unendlich sein (d. h., die Werte der `Double.PositiveInfinity`). Allerdings die `SizeRequest` zurückgegeben, die von diesen Methoden dürfen nicht unendliche Dimensionen.

Unbegrenzte Einschränkungen Geben Sie an, dass die angeforderte Größe natürliche Elementgröße widerspiegeln soll. Eine vertikale `StackLayout` Aufrufe `GetSizeRequest` (oder `Measure`) für seine untergeordneten Elemente mit einer Einschränkung unbegrenzte Höhe. Ruft eine horizontale Stapellayout `GetSizeRequest` (oder `Measure`) für seine untergeordneten Elemente mit einer Einschränkung für eine unendliche Breite. Ein `AbsoluteLayout` Aufrufe `GetSizeRequest` (oder `Measure`) für seine untergeordneten Elemente mit unendliche Breite und Höhe Einschränkungen.

### <a name="peeking-inside-the-process"></a>Innerhalb des Prozesses einsehen

Die [ **ExploreChildSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/ExploreChildSizes) zeigt Einschränkung und Größe Anforderung zu einem einfachen Layout.

## <a name="deriving-from-layoutview"></a>Ableiten von Layout<View>

Eine benutzerdefiniertes Layout-Klasse abgeleitet `Layout<View>`. Es sind zwei Aufgaben:

- Überschreiben Sie `OnMeasure` Aufrufen `Measure` auf alle Layout untergeordneten Standorten. Eine angeforderte Größe für das Layout selbst zurückzugeben
- Überschreiben Sie `LayoutChildren` Aufrufen `Layout` auf alle Layout untergeordneten Standorten

Die `for` oder `foreach` Schleife in den überschreibungen allen untergeordneten Elementen überspringen soll, deren `IsVisible` -Eigenschaftensatz auf `false`.

Ein Aufruf von `OnMeasure` ist nicht garantiert. `OnMeasure` wird nicht aufgerufen werden, wenn das übergeordnete Element des Layouts wird das Layout-Größe (z. B. ein Layout, die eine Seite gefüllt wird) steuern. Aus diesem Grund `LayoutChildren` nicht verlässlich untergeordneten Größen abgerufen werden, während die `OnMeasure` aufrufen. Sehr häufig `LayoutChildren` muss selbst aufrufen `Measure` auf das Layout der untergeordneten Elemente, oder Sie können eine Art von Größe cacheprogrammierlogik (um später besprochen zu werden) implementieren.

### <a name="an-easy-example"></a>Ein Beispiel für einfache

Die [ **VerticalStackDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/VerticalStackDemo) Beispiel enthält eine vereinfachte [ `VerticalStack` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter26/VerticalStackDemo/VerticalStackDemo/VerticalStackDemo/VerticalStack.cs) Klasse und eine Demonstration der Verwendungsmöglichkeiten.

### <a name="vertical-and-horizontal-positioning-simplified"></a>Vertikale und horizontale Positionierung vereinfacht

Einer der Aufträge, die `VerticalStack` durchzuführenden tritt während der `LayoutChildren` außer Kraft setzen. Die Methode verwendet der untergeordnetes `HorizontalOptions` -Eigenschaft bestimmen, wie das untergeordnete Element in seinem Steckplatz im Positionieren der `VerticalStack`. Sie können stattdessen die statische Methode aufrufen [ `Layout.LayoutChildIntoBoundingRect` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/). Diese Methode ruft `Measure` auf der untergeordneten und verwendet die `HorizontalOptions` und `VerticalOptions` Eigenschaften, um das untergeordnete Element innerhalb des angegebenen Rechtecks zu positionieren.

### <a name="invalidation"></a>Aufheben einer Validierung

Häufig wirkt sich auf eine Änderung in der Eigenschaft für ein Element wie das Element im Layout angezeigt wird. Das Layout muss ein neues Layout auslösen für ungültig erklärt.

`VisualElement` definiert eine geschützte Methode [ `InvalidateMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.InvalidateMeasure()/), die in der Regel durch den Handler durch geänderte Eigenschaften ausgelöste einer beliebigen bindbare Eigenschaft, deren Änderung aufgerufen wird, wirkt sich auf die Größe des Elements. Die `InvalidateMeasure` Methode ausgelöst wird eine [ `MeasureInvalidated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.MeasureInvalidated/) Ereignis.

Die `Layout` Klasse definiert eine ähnliche geschützte Methode, die mit dem Namen [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/), wodurch eine `Layout` Ableitung rufen Sie für jede Änderung, die beeinflussen, wie es positioniert und passt seine untergeordneten Elemente.

### <a name="some-rules-for-coding-layouts"></a>Einige Regeln zum Codieren von layouts

1. Eigenschaften von definiert `Layout<T>` ableitungen bindbare Eigenschaften gesichert werden soll, und die geänderte Eigenschaften ausgelöste Handler sollte Aufrufen `InvalidateLayout`.

2. Ein `Layout<T>` Ableitung, die angefügten bindbare Eigenschaften definiert, sollte außer Kraft gesetzt [ `OnAdded` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout%3CT%3E.OnAdded/p/T/) einen durch geänderte Eigenschaften ausgelöste Handler mit seinen untergeordneten Elementen hinzugefügt und [ `OnRemoved` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout%3CT%3E.OnRemoved/p/T/) zu entfernen Handler. Der Handler überprüfen Sie, ob Änderungen in diesen bindbare Eigenschaften angefügt und reagieren Sie durch den Aufruf sollte `InvalidateLayout`.

3. Ein `Layout<T>` Ableitung, die einen Cache der untergeordneten Größen implementiert sollten überschreiben `InvalidateLayout` und [ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/) und den Cache zu löschen, wenn diese Methoden aufgerufen werden.

### <a name="a-layout-with-properties"></a>Ein Layout mit Eigenschaften

Die [ `WrapLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/WrapLayout.cs) -Klasse in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) wird davon ausgegangen, dass alle zugehörigen untergeordneten Elemente sind die gleiche Größe und bricht die untergeordneten Elemente von einer Zeile (oder Spalte) mit das nächste. Definiert eine `Orientation` Eigenschaft wie `StackLayout`, und `ColumnSpacing` und `RowSpacing` Eigenschaften, z. B. `Grid`, und untergeordnete Größen zwischenspeichert.

Die [ **PhotoWrap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoWrap) Beispiel setzt eine `WrapLayout` in einem `ScrollView` für die Anzeige von Standardbilder.

### <a name="no-unconstrained-dimensions-allowed"></a>Keine uneingeschränkten Dimensionen zulässig.

Die [ `UniformGridLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/UniformGridLayout.cs) in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek richtet sich an alle untergeordneten Elemente innerhalb des Workflows angezeigt. Aus diesem Grund werden kann nicht mit nicht eingeschränkten Dimensionen umgehen und löst eine Ausnahme aus, wenn eine gefunden wird.

Die [ **PhotoGrid** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoGrid) demonstriert `UniformGridLayout`:

[![Dreifacher Screenshot des Rasters Foto](images/ch26fg08-small.png "Uniform Rasterlayout")](images/ch26fg08-large.png#lightbox "Uniform Rasterlayout")

### <a name="overlapping-children"></a>Überlappende untergeordnete Elemente

Ein `Layout<T>` Ableitung kann seine untergeordneten Elemente überlappen. Allerdings werden die untergeordneten Elemente gerendert, entsprechend ihrer Reihenfolge in der `Children` Sammlung und nicht die Reihenfolge, in der ihre `Layout` Methoden aufgerufen werden.

Die `Layout` Klasse definiert zwei Methoden, die Ihnen ermöglichen, ein untergeordnetes Element in der Auflistung zu verschieben:

- [`LowerChild`](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LowerChild/p/Xamarin.Forms.View/) So verschieben Sie ein untergeordnetes Element am Anfang der auflistungs
- [`RaiseChild`](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.RaiseChild/p/Xamarin.Forms.View/) So verschieben Sie ein untergeordnetes Element am Ende der Auflistung

Für überlappende Kinder werden untergeordnete Elemente am Ende der Auflistung über untergeordnete Elemente am Anfang der auflistungs visuell angezeigt.

Die [ `OverlapLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/OverlapLayout.cs) -Klasse in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Library definiert eine angefügte Eigenschaft können daher einer der und geben Sie die Renderreihenfolge seiner untergeordnete Elemente, die zusätzlich zu den anderen angezeigt werden. Die [ **StudentCardFile** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/StudentCardFile) Beispiel veranschaulicht dies:

[![Dreifacher Screenshot der Student Karte Datei Raster](images/ch26fg10-small.png "überlappende Layout Kinder")](images/ch26fg10-large.png#lightbox "überlappende Layout untergeordnete Elemente")

### <a name="more-attached-bindable-properties"></a>Bindbare Eigenschaften angefügt

Die [ `CartesianLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CartesianLayout.cs) -Klasse in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Library definiert angefügte bindbare Eigenschaften, um zwei anzugeben `Point` Werte und eine stärkenwert und bearbeitet `BoxView` Elemente, die Zeilen ähnelt.

Die [ **UnitCube** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/UnitCube) Beispiel verwendet, die einen Cube 3D gezeichnet werden soll.

### <a name="layout-and-layoutto"></a>Layout und LayoutTo

Ein `Layout<T>` Ableitung Aufrufen `LayoutTo` statt `Layout` um das Layout zu animieren. Die [ `AnimatedCartesianLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnimatedCartesianLayout.cs) Klasse ist, und die [ **AnimatedUnitCube** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/AnimatedUnitCube) demonstriert, die es.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 26 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch26-Apr2016.pdf)
- [Kapitel 26-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26)
- [Erstellen von benutzerdefinierten Layouts](~/xamarin-forms/user-interface/layouts/custom.md)
