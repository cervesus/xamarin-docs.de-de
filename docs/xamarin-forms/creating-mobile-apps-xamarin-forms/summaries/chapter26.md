---
title: Zusammenfassung der Kapitel 26. Benutzerdefinierte layouts
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 26. Benutzerdefinierte layouts'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2B7F4346-414E-49FF-97FB-B85E92D98A21
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 9fa9802f94e10612c4b0fe02c84ddcabc89820a8
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "53053129"
---
# <a name="summary-of-chapter-26-custom-layouts"></a>Zusammenfassung der Kapitel 26. Benutzerdefinierte layouts

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26)

Xamarin.Forms umfasst mehrere Klassen, die von [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1):

* `StackLayout`,
* `Grid`,
* `AbsoluteLayout` und
* `RelativeLayout`.

In diesem Kapitel wird beschrieben, wie Sie eigene Klassen erstellen, die abgeleitet `Layout<View>`.

## <a name="an-overview-of-layout"></a>Eine Übersicht über layout

Es ist keine zentrale System, das Xamarin.Forms-Layouts behandelt. Jedes Element ist zuständig für das ermitteln, was eine eigene groß sein sollte und wie Sie sich selbst zu einem bestimmten Bereich zu rendern.

### <a name="parents-and-children"></a>Eltern und Kinder

Jedes Element, das über untergeordnete Elemente verfügt, ist verantwortlich für die Positionierung dieser untergeordneten Elemente innerhalb des Workflows. Es ist das übergeordnete Element, die letztlich bestimmt, was die Größe seiner untergeordneten Elemente sollte darauf basieren auf der Größe verfügbar sind und möchte, dass die Größe der untergeordneten sein.

### <a name="sizing-and-positioning"></a>Größenanpassung und Positionierung

Layout beginnt am oberen Rand der visuellen Struktur mit der Seite, und klicken Sie dann alle Branches durchläuft. Die wichtigste öffentliche Methode im Layout ist [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) durch definierten `VisualElement`. Jedes Element, das ein übergeordnetes Element anderer Elemente Aufrufe ist `Layout` für jeden seiner untergeordneten Elemente für dem untergeordnete Element geben Sie eine Größe und Position relativ zu sich selbst in Form einer [ `Rectangle` ](xref:Xamarin.Forms.Rectangle) Wert. Diese `Layout` Aufrufe, die von der visuellen Struktur weitergegeben werden.

Ein Aufruf von `Layout` ist erforderlich, für ein Element, auf dem Bildschirm angezeigt, und führt dazu, dass die folgenden schreibgeschützten Eigenschaften festgelegt werden. Sie sind konsistent mit der `Rectangle` an die Methode übergeben:

- [`Bounds`](xref:Xamarin.Forms.VisualElement.Bounds) Der Typ `Rectangle`
- [`X`](xref:Xamarin.Forms.VisualElement.X) Der Typ `double`
- [`Y`](xref:Xamarin.Forms.VisualElement.Y) Der Typ `double`
- [`Width`](xref:Xamarin.Forms.VisualElement.Width) Der Typ `double`
- [`Height`](xref:Xamarin.Forms.VisualElement.Height) Der Typ `double`

Vor der `Layout` aufrufen, `Height` und `Width` mock Werte von &ndash;1.

Ein Aufruf von `Layout` auch Aufrufe an den folgenden geschützten Methoden ausgelöst:

- [`SizeAllocated`](xref:Xamarin.Forms.VisualElement.SizeAllocated(System.Double,System.Double)), der Aufrufe
- [`OnSizeAllocated`](xref:Xamarin.Forms.VisualElement.OnSizeAllocated(System.Double,System.Double)), die überschrieben werden können.

Schließlich wird das folgende Ereignis ausgelöst:

- [`SizeChanged`](xref:Xamarin.Forms.VisualElement.SizeChanged)

Die `OnSizeAllocated` Methode wird überschrieben, indem `Page` und `Layout`, welche sind die nur zwei Klassen in Xamarin.Forms verwenden, die untergeordnete Elemente aufweisen können. Ruft die überschriebene Methode

- [`UpdateChildrenLayout`](xref:Xamarin.Forms.Page.UpdateChildrenLayout) für `Page` ableitungen und [ `UpdateChildrenLayout` ](xref:Xamarin.Forms.Layout.UpdateChildrenLayout) für `Layout` ableitungen handeln, die aufruft
- [`LayoutChildren`](xref:Xamarin.Forms.Page.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) für `Page` ableitungen und [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) für `Layout` ableitungen.

`LayoutChildren` Ruft dann `Layout` aller seiner untergeordneten Elemente. Verfügt über mindestens ein untergeordnetes Element ein neues `Bounds` festlegen, und klicken Sie dann das folgende Ereignis ausgelöst wird:

- [`LayoutChanged`](xref:Xamarin.Forms.Page.LayoutChanged) für `Page` ableitungen und [ `LayoutChanged` ](xref:Xamarin.Forms.Layout.LayoutChanged) für `Layout` ableitungen

### <a name="constraints-and-size-requests"></a>Einschränkungen und Anforderungen der Größe

Für `LayoutChildren` auf intelligente Weise aufrufen `Layout` auf alle untergeordneten Elemente, benötigen sie eine *bevorzugte* oder *gewünschten* Größe für die untergeordneten Elemente. Aus diesem Grund die Aufrufe an `Layout` für jedes untergeordnete Element in der Regel durch Aufrufe von beginnen

- [`GetSizeRequest`](xref:Xamarin.Forms.VisualElement.GetSizeRequest(System.Double,System.Double))

Nachdem das Buch veröffentlicht wurde, die `GetSizeRequest` Methode wurde als veraltet markiert und durch ersetzt

- [`Measure`](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags))

Die `Measure` Methode ermöglicht die [ `Margin` ](xref:Xamarin.Forms.View.Margin) Eigenschaft und enthält ein Argument des Typs [ `MeasureFlag` ](xref:Xamarin.Forms.MeasureFlags), die verfügt über zwei Member:

- [`IncludeMargins`](xref:Xamarin.Forms.MeasureFlags.IncludeMargins)
- [`None`](xref:Xamarin.Forms.MeasureFlags.None) Ränder nicht eingeschlossen

Für viele Elemente `GetSizeRequest` oder `Measure` ruft Sie die systemeigene Größe des Elements aus der Renderer ab. Beide Methoden verfügen über Parameter für Breite und Höhe *Einschränkungen*. Z. B. eine `Label` wird Einschränkung für die Breite verwenden, um zu bestimmen, wie Sie mehrere Textzeilen zu umschließen.

Beide `GetSizeRequest`und `Measure` einen Rückgabewert vom Typ [ `SizeRequest` ](xref:Xamarin.Forms.SizeRequest), die über zwei Eigenschaften verfügt:

- [`Request`](xref:Xamarin.Forms.SizeRequest.Request) Der Typ `Size`
- [`Minimum`](xref:Xamarin.Forms.SizeRequest.Minimum) Der Typ `Size`

Sehr häufig auf diese beiden Werte sind identisch, und die `Minimum` Wert kann in der Regel ignoriert werden.

`VisualElement` definiert auch eine geschützte Methode ähnelt `GetSizeRequest` aus namens `GetSizeRequest`:

- [`OnSizeRequest`](xref:Xamarin.Forms.VisualElement.OnSizeRequest(System.Double,System.Double)) Gibt eine `SizeRequest` Wert

Diese Methode ist jetzt als veraltet markiert und ersetzt durch:

- [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double))

Jede abgeleitete Klasse `Layout` oder `Layout<T>` müssen überschreiben `OnSizeRequest` oder `OnMeasure`. Dies ist, in denen eine Layoutklasse eigene Größe bestimmt, welche die in der Regel auf die Größe der untergeordneten, die er basiert durch den Aufruf erhält `GetSizeRequest` oder `Measure` in den untergeordneten Elementen. Vor und nach dem Aufruf `OnSizeRequest` oder `OnMeasure`, `GetSizeRequest` oder `Measure` werden Anpassungen in Schritten basierend auf den folgenden Eigenschaften:

- [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)Der Typ `double`, wirkt sich auf die `Request` Eigenschaft `SizeRequest`
- [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) Der Typ `double`, wirkt sich auf die `Request` Eigenschaft `SizeRequest`
- [`MinimumWidthRequest`](xref:Xamarin.Forms.VisualElement.MinimumWidthRequest) Der Typ `double`, wirkt sich auf die `Minimum` Eigenschaft `SizeRequest`
- [`MinimumHeightRequest`](xref:Xamarin.Forms.VisualElement.MinimumHeightRequest) Der Typ `double`, wirkt sich auf die `Minimum` Eigenschaft `SizeRequest`

### <a name="infinite-constraints"></a>Unbegrenzte Einschränkungen

Die Einschränkung Argumente zu übergeben, um `GetSizeRequest` (oder `Measure`) und `OnSizeRequest` (oder `OnMeasure`) unendlich sein (d. h. die Werte der `Double.PositiveInfinity`). Allerdings die `SizeRequest` zurückgegeben, die von diesen Methoden können nicht unendliche Abmessungen enthalten.

Unbegrenzte Einschränkungen anzugeben, dass die angeforderte Größe des Elements natürlichen Größe hinweisen. Ein vertikales `StackLayout` Aufrufe `GetSizeRequest` (oder `Measure`) für seine untergeordneten Elemente mit einer Einschränkung unbegrenzte Höhe. Ruft von einem Layout mit einem horizontalen `GetSizeRequest` (oder `Measure`) für seine untergeordneten Elemente mit einer Einschränkung unendliche Breite. Ein `AbsoluteLayout` Aufrufe `GetSizeRequest` (oder `Measure`) für seine untergeordneten Elemente mit unendliche Breite und Höhe Einschränkungen.

### <a name="peeking-inside-the-process"></a>Innerhalb des Prozesses einsehen

Die [ **ExploreChildSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/ExploreChildSizes) zeigt Einschränkung und die Größe, fordern Sie Informationen für ein einfaches Layout.

## <a name="deriving-from-layoutview"></a>Ableiten von Layout<View>

Eine benutzerdefiniertes Layout-Klasse leitet sich von `Layout<View>`. Es hat zwei Aufgaben:

- Außer Kraft setzen `OnMeasure` aufzurufende `Measure` auf alle das Layout der untergeordneten Elemente. Keine angeforderte Größe für das Layout selbst zurückgeben
- Außer Kraft setzen `LayoutChildren` aufzurufende `Layout` auf alle das Layout der untergeordneten Elemente

Die `for` oder `foreach` Schleife in den überschreibungen alle untergeordneten überspringen soll, deren `IsVisible` -Eigenschaftensatz auf `false`.

Ein Aufruf von `OnMeasure` ist nicht garantiert. `OnMeasure` wird nicht aufgerufen werden, wenn das übergeordnete Element des Layouts ist, die die Layoutgröße (z. B. ein Layout, das eine Seite ausgefüllt) steuern. Aus diesem Grund `LayoutChildren` können nicht auf untergeordnete Größen erhalten während der basieren die `OnMeasure` aufrufen. Sehr oft `LayoutChildren` muss sich nicht selbst aufrufen `Measure` auf das Layout der untergeordneten Elemente, oder Sie können eine Art von Größe, die Logik (wird später angesprochen werden) für das Zwischenspeichern implementieren.

### <a name="an-easy-example"></a>Ein Beispiel für einfache

Die [ **VerticalStackDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/VerticalStackDemo) Beispiel enthält eine vereinfachte [ `VerticalStack` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter26/VerticalStackDemo/VerticalStackDemo/VerticalStackDemo/VerticalStack.cs) -Klasse und eine Demonstration dessen Verwendung.

### <a name="vertical-and-horizontal-positioning-simplified"></a>Vertikale und horizontale Positionierung vereinfacht

Einer der Aufträge, die `VerticalStack` müssen ausführen, tritt bei der die `LayoutChildren` außer Kraft setzen. Die Methode verwendet des untergeordnete Element des `HorizontalOptions` Eigenschaft, um zu bestimmen, wie das untergeordnete Element in seinem Steckplatz im Positionieren der `VerticalStack`. Sie können stattdessen die statische Methode aufrufen [ `Layout.LayoutChildIntoBoundingRect` ](xref:Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle)). Diese Methode ruft `Measure` auf den untergeordneten und verwendet die `HorizontalOptions` und `VerticalOptions` Eigenschaften, die das untergeordnete Element innerhalb des angegebenen Rechtecks positionieren.

### <a name="invalidation"></a>Aufheben einer Validierung

Häufig wirkt sich auf eine Änderung in die Eigenschaft eines Elements wie dieses Element im Layout angezeigt wird. Das Layout muss ein neues Layout auslösen ungültig werden.

`VisualElement` definiert eine geschützte Methode [ `InvalidateMeasure` ](xref:Xamarin.Forms.VisualElement.InvalidateMeasure), die in der Regel durch den Handler durch geänderte Eigenschaften ausgelöste, bindbare Eigenschaft, deren Änderung aufgerufen wird, wirkt sich auf die Größe des Elements. Die `InvalidateMeasure` Methode löst eine [ `MeasureInvalidated` ](xref:Xamarin.Forms.VisualElement.MeasureInvalidated) Ereignis.

Die `Layout` -Klasse definiert eine ähnliche geschützte Methode, die mit dem Namen [ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout), wodurch eine `Layout` Ableitung rufen Sie für jede Änderung, die betroffen sind, wie es passt Position und Größe der untergeordneten Elemente.

### <a name="some-rules-for-coding-layouts"></a>Einige Regeln für die Codierung von layouts

1. Definierten Eigenschaften `Layout<T>` ableitungen sollten durch bindbare Eigenschaften unterstützt werden, und es sollte die Eigenschaft änderbaren Handler aufrufen `InvalidateLayout`.

2. Ein `Layout<T>` Ableitung, die definiert, angefügte bindbare Eigenschaften sollten außer Kraft setzen [ `OnAdded` ](xref:Xamarin.Forms.Layout`1.OnAdded*) ein PropertyChanged-Handler seine untergeordneten Elemente hinzu und [ `OnRemoved` ](xref:Xamarin.Forms.Layout`1.OnRemoved*) , das Entfernen -Handler. Der Handler überprüfen Sie, ob Änderungen in den folgenden bindbare Eigenschaften zugeordnet und durch den Aufruf zu reagieren sollte `InvalidateLayout`.

3. Ein `Layout<T>` Ableitung, die einen Cache der untergeordneten Größen implementiert sollten außer Kraft setzen `InvalidateLayout` und [ `OnChildMeasureInvalidated` ](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated) und den Cache zu löschen, wenn diese Methoden aufgerufen werden.

### <a name="a-layout-with-properties"></a>Ein Layout mit Eigenschaften

Die [ `WrapLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/WrapLayout.cs) -Klasse in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) wird davon ausgegangen, dass alle untergeordneten Elemente sind die gleichen Größe, und umschließt die untergeordneten Elemente aus einer Zeile (oder Spalte) die nächste. Definiert eine `Orientation` Eigenschaft wie z.B. `StackLayout`, und `ColumnSpacing` und `RowSpacing` Eigenschaften wie `Grid`, und es werden die Größen der untergeordneten.

Die [ **PhotoWrap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoWrap) Beispiel setzt eine `WrapLayout` in einem `ScrollView` für die Anzeige von Archivfotos.

### <a name="no-unconstrained-dimensions-allowed"></a>Keine uneingeschränkten Dimensionen zulässig.

Die [ `UniformGridLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/UniformGridLayout.cs) in die [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) soll alle seine untergeordneten Elemente innerhalb des Workflows anzuzeigen. Aus diesem Grund Es kann nicht mit uneingeschränkten Dimensionen umgehen und löst eine Ausnahme aus, wenn eines gefunden wird.

Die [ **PhotoGrid** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoGrid) veranschaulicht `UniformGridLayout`:

[![Dreifacher Screenshot der Foto-Raster](images/ch26fg08-small.png "Uniform Rasterlayout")](images/ch26fg08-large.png#lightbox "Uniform Rasterlayout")

### <a name="overlapping-children"></a>Überlappende untergeordnete Elemente

Ein `Layout<T>` Ableitung kann es sich um die untergeordneten Elemente überschneiden. Allerdings werden die untergeordneten Elemente gerendert, entsprechend ihrer Reihenfolge in der `Children` Sammlung und nicht die Reihenfolge, in dem ihre `Layout` Methoden aufgerufen werden.

Die `Layout` -Klasse definiert zwei Methoden, mit denen Sie ein untergeordnetes Element in der Auflistung zu verschieben:

- [`LowerChild`](xref:Xamarin.Forms.Layout.LowerChild(Xamarin.Forms.View)) Um ein untergeordnetes Element am Anfang der Auflistung zu verschieben.
- [`RaiseChild`](xref:Xamarin.Forms.Layout.RaiseChild(Xamarin.Forms.View)) Um ein untergeordnetes Element am Ende der Auflistung zu verschieben.

Für überlappende Kinder werden untergeordnete Elemente am Ende der Auflistung visuell dargestellt über untergeordnete Elemente am Anfang der Auflistung.

Die [ `OverlapLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/OverlapLayout.cs) -Klasse in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Library definiert eine angefügte Eigenschaft geben Sie die Rendering-Reihenfolge und bieten daher eine der seine die untergeordneten Elemente, die zusätzlich zu den anderen angezeigt werden. Die [ **StudentCardFile** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/StudentCardFile) veranschaulicht dies:

[![Dreifacher Screenshot von "Student" Karte Dateiraster](images/ch26fg10-small.png "überlappende Layout untergeordneten")](images/ch26fg10-large.png#lightbox "überlappende Layout Kinder")

### <a name="more-attached-bindable-properties"></a>Bindbare Eigenschaften angefügt

Die [ `CartesianLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CartesianLayout.cs) -Klasse in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek definiert angefügte bindbare Eigenschaften gitattributes werden zwei `Point` Werte und ein Wert für die Breite und bearbeitet diesen `BoxView` Elementen, die Zeilen entsprechen.

Die [ **UnitCube** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/UnitCube) Beispiel verwendet, um ein 3D-Würfel zu zeichnen.

### <a name="layout-and-layoutto"></a>Layout und LayoutTo

Ein `Layout<T>` Ableitung Aufrufen `LayoutTo` statt `Layout` um das Layout zu animieren. Die [ `AnimatedCartesianLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnimatedCartesianLayout.cs) Klasse erfolgt, und die [ **AnimatedUnitCube** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/AnimatedUnitCube) veranschaulicht sie.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 26 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch26-Apr2016.pdf)
- [Kapitel 26-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26)
- [Erstellen von benutzerdefinierten Layouts](~/xamarin-forms/user-interface/layouts/custom.md)
