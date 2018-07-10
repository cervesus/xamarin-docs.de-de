---
title: Zusammenfassung der Kapitel 26. Benutzerdefinierte layouts
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 26. Benutzerdefinierte layouts'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2B7F4346-414E-49FF-97FB-B85E92D98A21
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: b6ef23364cac0dd1459681aa92c7a7db58bc81f0
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935640"
---
# <a name="summary-of-chapter-26-custom-layouts"></a>Zusammenfassung der Kapitel 26. Benutzerdefinierte layouts

Xamarin.Forms umfasst mehrere Klassen, die von [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/):

* `StackLayout`,
* `Grid`,
* `AbsoluteLayout` und
* `RelativeLayout`

In diesem Kapitel wird beschrieben, wie Sie eigene Klassen erstellen, die abgeleitet `Layout<View>`.

## <a name="an-overview-of-layout"></a>Eine Übersicht über layout

Es ist keine zentrale System, das Xamarin.Forms-Layouts behandelt. Jedes Element ist zuständig für das ermitteln, was eine eigene groß sein sollte und wie Sie sich selbst zu einem bestimmten Bereich zu rendern.

### <a name="parents-and-children"></a>Eltern und Kinder

Jedes Element, das über untergeordnete Elemente verfügt, ist verantwortlich für die Positionierung dieser untergeordneten Elemente innerhalb des Workflows. Es ist das übergeordnete Element, die letztlich bestimmt, was die Größe seiner untergeordneten Elemente sollte darauf basieren auf der Größe verfügbar sind und möchte, dass die Größe der untergeordneten sein.

### <a name="sizing-and-positioning"></a>Größenanpassung und Positionierung

Layout beginnt am oberen Rand der visuellen Struktur mit der Seite, und klicken Sie dann alle Branches durchläuft. Die wichtigste öffentliche Methode im Layout ist [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) durch definierten `VisualElement`. Jedes Element, das ein übergeordnetes Element anderer Elemente Aufrufe ist `Layout` für jeden seiner untergeordneten Elemente für dem untergeordnete Element geben Sie eine Größe und Position relativ zu sich selbst in Form einer [ `Rectangle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Rectangle/) Wert. Diese `Layout` Aufrufe, die von der visuellen Struktur weitergegeben werden.

Ein Aufruf von `Layout` ist erforderlich, für ein Element, auf dem Bildschirm angezeigt, und führt dazu, dass die folgenden schreibgeschützten Eigenschaften festgelegt werden. Sie sind konsistent mit der `Rectangle` an die Methode übergeben:

- [`Bounds`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Bounds/) Der Typ `Rectangle`
- [`X`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.X/) Der Typ `double`
- [`Y`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Y/) Der Typ `double`
- [`Width`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/) Der Typ `double`
- [`Height`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) Der Typ `double`

Vor der `Layout` aufrufen, `Height` und `Width` mock Werte von &ndash;1.

Ein Aufruf von `Layout` auch Aufrufe an den folgenden geschützten Methoden ausgelöst:

- [`SizeAllocated`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.SizeAllocated/p/System.Double/System.Double/), der Aufrufe
- [`OnSizeAllocated`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnSizeAllocated/p/System.Double/System.Double/), die überschrieben werden können.

Schließlich wird das folgende Ereignis ausgelöst:

- [`SizeChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.SizeChanged/)

Die `OnSizeAllocated` Methode wird überschrieben, indem `Page` und `Layout`, welche sind die nur zwei Klassen in Xamarin.Forms verwenden, die untergeordnete Elemente aufweisen können. Ruft die überschriebene Methode

- [`UpdateChildrenLayout`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.UpdateChildrenLayout()/) für `Page` ableitungen und [ `UpdateChildrenLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.UpdateChildrenLayout()/) für `Layout` ableitungen handeln, die aufruft
- [`LayoutChildren`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) für `Page` ableitungen und [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) für `Layout` ableitungen.

`LayoutChildren` Ruft dann `Layout` aller seiner untergeordneten Elemente. Verfügt über mindestens ein untergeordnetes Element ein neues `Bounds` festlegen, und klicken Sie dann das folgende Ereignis ausgelöst wird:

- [`LayoutChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.LayoutChanged/) für `Page` ableitungen und [ `LayoutChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Layout.LayoutChanged/) für `Layout` ableitungen

### <a name="constraints-and-size-requests"></a>Einschränkungen und Anforderungen der Größe

Für `LayoutChildren` auf intelligente Weise aufrufen `Layout` auf alle untergeordneten Elemente, benötigen sie eine *bevorzugte* oder *gewünschten* Größe für die untergeordneten Elemente. Aus diesem Grund die Aufrufe an `Layout` für jedes untergeordnete Element in der Regel durch Aufrufe von beginnen

- [`GetSizeRequest`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.GetSizeRequest/p/System.Double/System.Double/)

Nachdem das Buch veröffentlicht wurde, die `GetSizeRequest` Methode wurde als veraltet markiert und durch ersetzt

- [`Measure`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/)

Die `Measure` Methode ermöglicht die [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) Eigenschaft und enthält ein Argument des Typs [ `MeasureFlag` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MeasureFlags/), die verfügt über zwei Member:

- [`IncludeMargins`](xref:Xamarin.Forms.MeasureFlags.IncludeMargins)
- [`None`](xref:Xamarin.Forms.MeasureFlags.None) Ränder nicht eingeschlossen

Für viele Elemente `GetSizeRequest` oder `Measure` ruft Sie die systemeigene Größe des Elements aus der Renderer ab. Beide Methoden verfügen über Parameter für Breite und Höhe *Einschränkungen*. Z. B. eine `Label` wird Einschränkung für die Breite verwenden, um zu bestimmen, wie Sie mehrere Textzeilen zu umschließen.

Beide `GetSizeRequest`und `Measure` einen Rückgabewert vom Typ [ `SizeRequest` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SizeRequest/), die über zwei Eigenschaften verfügt:

- [`Request`](https://developer.xamarin.com/api/property/Xamarin.Forms.SizeRequest.Request/) Der Typ `Size`
- [`Minimum`](https://developer.xamarin.com/api/property/Xamarin.Forms.SizeRequest.Minimum/) Der Typ `Size`

Sehr häufig auf diese beiden Werte sind identisch, und die `Minimum` Wert kann in der Regel ignoriert werden.

`VisualElement` definiert auch eine geschützte Methode ähnelt `GetSizeRequest` aus namens `GetSizeRequest`:

- [`OnSizeRequest`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnSizeRequest/p/System.Double/System.Double/) Gibt eine `SizeRequest` Wert

Diese Methode ist jetzt als veraltet markiert und ersetzt durch:

- [`OnMeasure`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/)

Jede abgeleitete Klasse `Layout` oder `Layout<T>` müssen überschreiben `OnSizeRequest` oder `OnMeasure`. Dies ist, in denen eine Layoutklasse eigene Größe bestimmt, welche die in der Regel auf die Größe der untergeordneten, die er basiert durch den Aufruf erhält `GetSizeRequest` oder `Measure` in den untergeordneten Elementen. Vor und nach dem Aufruf `OnSizeRequest` oder `OnMeasure`, `GetSizeRequest` oder `Measure` werden Anpassungen in Schritten basierend auf den folgenden Eigenschaften:

- [`WidthRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/)Der Typ `double`, wirkt sich auf die `Request` Eigenschaft `SizeRequest`
- [`HeightRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) Der Typ `double`, wirkt sich auf die `Request` Eigenschaft `SizeRequest`
- [`MinimumWidthRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.MinimumWidthRequest/) Der Typ `double`, wirkt sich auf die `Minimum` Eigenschaft `SizeRequest`
- [`MinimumHeightRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.MinimumHeightRequest/) Der Typ `double`, wirkt sich auf die `Minimum` Eigenschaft `SizeRequest`

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

Einer der Aufträge, die `VerticalStack` müssen ausführen, tritt bei der die `LayoutChildren` außer Kraft setzen. Die Methode verwendet des untergeordnete Element des `HorizontalOptions` Eigenschaft, um zu bestimmen, wie das untergeordnete Element in seinem Steckplatz im Positionieren der `VerticalStack`. Sie können stattdessen die statische Methode aufrufen [ `Layout.LayoutChildIntoBoundingRect` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/). Diese Methode ruft `Measure` auf den untergeordneten und verwendet die `HorizontalOptions` und `VerticalOptions` Eigenschaften, die das untergeordnete Element innerhalb des angegebenen Rechtecks positionieren.

### <a name="invalidation"></a>Aufheben einer Validierung

Häufig wirkt sich auf eine Änderung in die Eigenschaft eines Elements wie dieses Element im Layout angezeigt wird. Das Layout muss ein neues Layout auslösen ungültig werden.

`VisualElement` definiert eine geschützte Methode [ `InvalidateMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.InvalidateMeasure()/), die in der Regel durch den Handler durch geänderte Eigenschaften ausgelöste, bindbare Eigenschaft, deren Änderung aufgerufen wird, wirkt sich auf die Größe des Elements. Die `InvalidateMeasure` Methode löst eine [ `MeasureInvalidated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.MeasureInvalidated/) Ereignis.

Die `Layout` -Klasse definiert eine ähnliche geschützte Methode, die mit dem Namen [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/), wodurch eine `Layout` Ableitung rufen Sie für jede Änderung, die betroffen sind, wie es passt Position und Größe der untergeordneten Elemente.

### <a name="some-rules-for-coding-layouts"></a>Einige Regeln für die Codierung von layouts

1. Definierten Eigenschaften `Layout<T>` ableitungen sollten durch bindbare Eigenschaften unterstützt werden, und es sollte die Eigenschaft änderbaren Handler aufrufen `InvalidateLayout`.

2. Ein `Layout<T>` Ableitung, die definiert, angefügte bindbare Eigenschaften sollten außer Kraft setzen [ `OnAdded` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout%3CT%3E.OnAdded/p/T/) ein PropertyChanged-Handler seine untergeordneten Elemente hinzu und [ `OnRemoved` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout%3CT%3E.OnRemoved/p/T/) , das Entfernen -Handler. Der Handler überprüfen Sie, ob Änderungen in den folgenden bindbare Eigenschaften zugeordnet und durch den Aufruf zu reagieren sollte `InvalidateLayout`.

3. Ein `Layout<T>` Ableitung, die einen Cache der untergeordneten Größen implementiert sollten außer Kraft setzen `InvalidateLayout` und [ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/) und den Cache zu löschen, wenn diese Methoden aufgerufen werden.

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

- [`LowerChild`](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LowerChild/p/Xamarin.Forms.View/) Um ein untergeordnetes Element am Anfang der Auflistung zu verschieben.
- [`RaiseChild`](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.RaiseChild/p/Xamarin.Forms.View/) Um ein untergeordnetes Element am Ende der Auflistung zu verschieben.

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
