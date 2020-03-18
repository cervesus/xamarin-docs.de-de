---
title: 'Zusammenfassung für Kapitel 26: Benutzerdefinierte Layouts'
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung für Kapitel 26: Benutzerdefinierte Layouts'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2B7F4346-414E-49FF-97FB-B85E92D98A21
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 1eb5153f8ab295696e373f4fdb65a4f8820a05bc
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "70770943"
---
# <a name="summary-of-chapter-26-custom-layouts"></a>Zusammenfassung für Kapitel 26: Benutzerdefinierte Layouts

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26)

Xamarin.Forms umfasst eine Reihe von Klassen, die von [`Layout<View>`](xref:Xamarin.Forms.Layout`1) abgeleitet sind:

- `StackLayout`,
- `Grid`,
- `AbsoluteLayout` und
- `RelativeLayout`

In diesem Kapitel wird beschrieben, wie Sie eigene Klassen erstellen, die von `Layout<View>` abgeleitet werden.

## <a name="an-overview-of-layout"></a>Das Layout im Überblick

Es gibt kein zentralisiertes System zur Verarbeitung des Xamarin.Form-Layouts. Jedes Element bestimmt die eigene Größe selbst und ermittelt, wie es innerhalb eines bestimmten Bereichs gerendert wird.

### <a name="parents-and-children"></a>Über- und untergeordnete Elemente

Jedes Element, das über untergeordnete Elemente verfügt, ist für die Positionierung dieser untergeordneten Elemente verantwortlich. Die Größe der untergeordneten Elemente wird dabei vom übergeordneten Element basierend auf der verfügbaren Größe und der vom untergeordneten Element angestrebten Größe bestimmt.

### <a name="sizing-and-positioning"></a>Festlegung der Größe und Positionierung

Für das Layout wird im oberen Bereich der visuellen Struktur mit der Seite begonnen, auf die alle Branches folgen. Die wichtigste öffentliche Methode innerhalb des Layouts ist [`Layout`](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)), die durch `VisualElement` definiert wird. Jedes übergeordnete Element ruft für jedes seiner untergeordneten Elemente `Layout` auf, um die Größe des Elements festzulegen und seine Position zu bestimmen (relativ zum übergeordneten Element). Zu diesem Zweck wird ein [`Rectangle`](xref:Xamarin.Forms.Rectangle)-Wert angegeben. Diese Aufrufe von `Layout` werden innerhalb der visuellen Struktur propagiert.

Damit ein Element auf dem Bildschirm angezeigt wird, muss `Layout` aufgerufen werden. Dadurch werden die folgenden schreibgeschützten Eigenschaften festgelegt. Diese Eigenschaften stimmen mit dem `Rectangle`-Wert überein, der an die Methode übergeben wird:

- [`Bounds`](xref:Xamarin.Forms.VisualElement.Bounds) vom Typ `Rectangle`
- [`X`](xref:Xamarin.Forms.VisualElement.X) vom Typ `double`
- [`Y`](xref:Xamarin.Forms.VisualElement.Y) vom Typ `double`
- [`Width`](xref:Xamarin.Forms.VisualElement.Width) vom Typ `double`
- [`Height`](xref:Xamarin.Forms.VisualElement.Height) vom Typ `double`

Vor dem Aufruf von `Layout` weisen `Height` und `Width` den Pseudowert &ndash;1 auf.

Durch den Aufruf von `Layout` wird zudem der Aufruf der folgenden geschützten Methoden ausgelöst:

- [`SizeAllocated`](xref:Xamarin.Forms.VisualElement.SizeAllocated(System.Double,System.Double)), die wiederum zum Aufruf von
- [`OnSizeAllocated`](xref:Xamarin.Forms.VisualElement.OnSizeAllocated(System.Double,System.Double)) führt, die außer Kraft gesetzt werden kann.

Schließlich wird das folgende Ereignis ausgelöst:

- [`SizeChanged`](xref:Xamarin.Forms.VisualElement.SizeChanged)

Die Methode `OnSizeAllocated` wird durch `Page` und `Layout` außer Kraft gesetzt, den einzigen beiden Klassen in Xamarin.Forms, die über untergeordnete Elemente verfügen können. Die außer Kraft gesetzten Methodenaufrufe

- [`UpdateChildrenLayout`](xref:Xamarin.Forms.Page.UpdateChildrenLayout) für `Page`-Ableitungen und [`UpdateChildrenLayout`](xref:Xamarin.Forms.Layout.UpdateChildrenLayout) für `Layout`-Ableitungen. Dadurch wird folgender Aufruf ausgelöst:
- [`LayoutChildren`](xref:Xamarin.Forms.Page.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) für `Page`-Ableitungen und [`LayoutChildren`](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) für `Layout`-Ableitungen.

`LayoutChildren` ruft dann für jedes untergeordnete Element des Elements `Layout` auf. Wenn mindestens ein untergeordnetes Element über eine neue `Bounds`-Einstellung verfügt, wird das folgende Ereignis ausgelöst:

- [`LayoutChanged`](xref:Xamarin.Forms.Page.LayoutChanged) für `Page`-Ableitungen und [`LayoutChanged`](xref:Xamarin.Forms.Layout.LayoutChanged) für `Layout`-Ableitungen

### <a name="constraints-and-size-requests"></a>Constraints und Größenanforderungen

Damit `LayoutChildren` einen intelligenten Aufruf von `Layout` für alle seine untergeordneten Elemente durchführen kann, muss eine *bevorzugte* oder *gewünschte* Größe der untergeordneten Elemente bekannt sein. Aus diesem Grund wird den Aufrufen von `Layout` für die einzelnen untergeordneten Element in der Regel folgender Aufruf vorangestellt:

- [`GetSizeRequest`](xref:Xamarin.Forms.VisualElement.GetSizeRequest(System.Double,System.Double))

Nach der Veröffentlichung dieses Buchs wurde die Methode `GetSizeRequest` durch folgende Methode ersetzt:

- [`Measure`](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags))

Die Methode `Measure` umfasst die [`Margin`](xref:Xamarin.Forms.View.Margin)-Eigenschaft sowie ein Argument vom Typ [`MeasureFlag`](xref:Xamarin.Forms.MeasureFlags), das über zwei Member verfügt:

- [`IncludeMargins`](xref:Xamarin.Forms.MeasureFlags.IncludeMargins)
- [`None`](xref:Xamarin.Forms.MeasureFlags.None), um Ränder auszuschließen

Bei vielen Elementen ruft `GetSizeRequest` oder `Measure` die native Größe des Elements von seinem Renderer ab. Beide Methoden verfügen über *Constraints* für Breite und Höhe. `Label` verwendet z.B. die Breitenbeschränkung, um zu ermitteln, wie mehrere Textzeilen umbrochen werden.

Sowohl `GetSizeRequest` als auch `Measure` geben einen Wert vom Typ [`SizeRequest`](xref:Xamarin.Forms.SizeRequest)zurück, der über zwei Eigenschaften verfügt:

- [`Request`](xref:Xamarin.Forms.SizeRequest.Request) vom Typ `Size`
- [`Minimum`](xref:Xamarin.Forms.SizeRequest.Minimum) vom Typ `Size`

Diese beiden Werte sind häufig identisch, und der `Minimum`-Wert kann üblicherweise ignoriert werden.

`VisualElement` definiert zudem eine geschützte Methode, die `GetSizeRequest` ähnelt und von `GetSizeRequest` aufgerufen wird:

- [`OnSizeRequest`](xref:Xamarin.Forms.VisualElement.OnSizeRequest(System.Double,System.Double)) gibt einen `SizeRequest`-Wert zurück

Diese Methode ist inzwischen veraltet und wurde wie folgt ersetzt:

- [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double))

Jede Klasse, die von `Layout` oder `Layout<T>` abgeleitet wird, muss `OnSizeRequest` oder `OnMeasure` außer Kraft setzen. An dieser Stelle bestimmt eine Layoutklasse ihre eigene Größe, die im Allgemeinen auf der Größe ihrer untergeordneten Elemente basiert, die sie durch einen Aufruf von `GetSizeRequest` oder `Measure` für die untergeordneten Elemente erhält. Vor und nach dem Aufruf von `OnSizeRequest` oder `OnMeasure` nimmt `GetSizeRequest` oder `Measure` basierend auf den folgenden Eigenschaften Anpassungen vor:

- [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) vom Typ `double`, wirkt sich auf die `Request`-Eigenschaft von `SizeRequest` aus
- [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) vom Typ `double`, wirkt sich auf die `Request`-Eigenschaft von `SizeRequest` aus
- [`MinimumWidthRequest`](xref:Xamarin.Forms.VisualElement.MinimumWidthRequest) vom Typ `double`, wirkt sich auf die `Minimum`-Eigenschaft von `SizeRequest` aus
- [`MinimumHeightRequest`](xref:Xamarin.Forms.VisualElement.MinimumHeightRequest) vom Typ `double`, wirkt sich auf die `Minimum`-Eigenschaft von `SizeRequest` aus

### <a name="infinite-constraints"></a>Unbegrenzte Constraints

Die an `GetSizeRequest` (oder `Measure`) und `OnSizeRequest` (oder `OnMeasure`) übergebenen Argumente können unbegrenzt sein (d.h. Werte von `Double.PositiveInfinity`). Der von diesen Methoden zurückgegebene `SizeRequest`-Wert kann jedoch keine unbegrenzten Dimensionen enthalten.

Unbegrenzte Constraints geben an, dass die angeforderte Größe der natürlichen Größe des Elements entsprechen soll. Ein vertikales `StackLayout` ruft `GetSizeRequest` (oder `Measure`) für seine untergeordneten Elemente mit einer unbegrenzten Höheneinschränkung auf. Ein horizontales Stapellayout ruft `GetSizeRequest` (oder `Measure`) für seine untergeordneten Elemente mit einer unbegrenzten Breiteneinschränkung auf. Ein `AbsoluteLayout` ruft `GetSizeRequest` (oder `Measure`) für seine untergeordneten Elemente mit einer unbegrenzten Breiten- und Höheneinschränkung auf.

### <a name="peeking-inside-the-process"></a>Der Prozess im Detail

[**ExploreChildSize**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/ExploreChildSizes) zeigt Informationen zu Constraints und Größenanforderungen für ein einfaches Layout.

## <a name="deriving-from-layoutview"></a>Ableitung von Layout\<View>

Eine benutzerdefinierte Layoutklasse wird von `Layout<View>` abgeleitet. Sie hat zwei Aufgaben:

- Außerkraftsetzen von `OnMeasure`, um `Measure` für alle untergeordneten Elemente im Layout aufzurufen Zurückgeben einer angeforderten Größe für das Layout selbst
- Außerkraftsetzen von `LayoutChildren`, um `Layout` für alle untergeordneten Elemente im Layout aufzurufen

Die `for`- oder `foreach`-Schleife dieser Außerkraftsetzungen sollte alle untergeordneten Elemente überspringen, deren `IsVisible`-Eigenschaft auf `false` festgelegt ist.

`OnMeasure` wird nicht in jedem Fall aufgerufen. Wenn das übergeordnete Element des Layouts die Größe des Layouts bestimmt (z.B. ein Layout, das eine Seite füllt), wird `OnMeasure` nicht aufgerufen. Aus diesem Grund kann sich `LayoutChildren` nicht darauf verlassen, dass die Größe der untergeordneten Elemente beim Aufruf von `OnMeasure` bestimmt wird. Häufig muss `LayoutChildren` den Aufruf von `Measure` selbst für die untergeordneten Elemente des Layouts durchführen. Alternativ können Sie eine Logik zum Zwischenspeichern der Größe implementieren (dieses Thema wird später näher besprochen).

### <a name="an-easy-example"></a>Ein einfaches Beispiel

Das Beispiel [**VerticalStackDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/VerticalStackDemo) enthält eine vereinfachte [`VerticalStack`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter26/VerticalStackDemo/VerticalStackDemo/VerticalStackDemo/VerticalStack.cs)-Klasse und zeigt, wie diese Klasse verwendet wird.

### <a name="vertical-and-horizontal-positioning-simplified"></a>Vereinfachte vertikale und horizontale Positionierung

Eine der Aufgaben, die `VerticalStack` ausführen muss, findet während der Außerkraftsetzung von `LayoutChildren` statt. Die Methode ermittelt anhand der Eigenschaft `HorizontalOptions` des untergeordneten Elements, wie das untergeordnete Element innerhalb von `VerticalStack` positioniert werden soll. Sie können stattdessen die statische Methode [`Layout.LayoutChildIntoBoundingRect`](xref:Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle)) aufrufen. Diese Methode ruft `Measure` für das untergeordnete Element auf und positioniert das untergeordnete Element basierend auf den Eigenschaften `HorizontalOptions` und `VerticalOptions` innerhalb des angegebenen Rechtecks.

### <a name="invalidation"></a>Aufheben einer Validierung

Häufig wirkt sich eine Änderung der Eigenschaft eines Elements darauf aus, wie das Element im Layout angezeigt wird. Das Layout muss ungültig gemacht werden, um ein neues Layout zu starten.

`VisualElement` definiert die geschützte Methode [`InvalidateMeasure`](xref:Xamarin.Forms.VisualElement.InvalidateMeasure), die üblicherweise vom Handler für geänderte Eigenschaften einer bindbaren Eigenschaft aufgerufen wird, deren Änderung sich auf die Größe des Elements auswirkt. Die Methode `InvalidateMeasure` löst ein [`MeasureInvalidated`](xref:Xamarin.Forms.VisualElement.MeasureInvalidated)-Ereignis aus.

Die Klasse `Layout` definiert eine ähnliche geschützte Methode [`InvalidateLayout`](xref:Xamarin.Forms.Layout.InvalidateLayout), die von einer `Layout`-Ableitung für jede Änderung aufgerufen werden sollte, die sich auf die Position und Größe ihrer untergeordneten Elemente auswirkt.

### <a name="some-rules-for-coding-layouts"></a>Regeln für die Layoutprogrammierung

1. Eigenschaften, die durch `Layout<T>`-Ableitungen definiert werden, sollten durch bindbare Eigenschaften gestützt werden, und die Handler für geänderte Eigenschaften sollten `InvalidateLayout` aufrufen.

2. Eine `Layout<T>`-Ableitung, die angefügte bindbare Eigenschaften definiert, sollte [`OnAdded`](xref:Xamarin.Forms.Layout`1.OnAdded*) außer Kraft setzen, um einen Handler für geänderte Eigenschaften zu ihren untergeordneten Elementen hinzuzufügen, und [`OnRemoved`](xref:Xamarin.Forms.Layout`1.OnRemoved*) außer Kraft setzen, um diesen Handler zu entfernen. Der Handler sollte diese angefügten bindbaren Eigenschaften auf Änderungen überprüfen und ggf. mit einem Aufruf von `InvalidateLayout` reagieren.

3. Eine `Layout<T>`-Ableitung, die einen Cache mit der Größe untergeordneter Elemente implementiert, sollte `InvalidateLayout` und [`OnChildMeasureInvalidated`](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated) außer Kraft setzen und den Cache leeren, wenn diese Methoden aufgerufen werden.

### <a name="a-layout-with-properties"></a>Ein Layout mit Eigenschaften

Die Klasse [`WrapLayout`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/WrapLayout.cs) im [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) setzt voraus, dass alle untergeordneten Elemente dieselbe Größe aufweisen, und führt für die untergeordneten Elemente einen Zeilen- oder Spaltenumbruch durch. Die Klasse definiert eine `Orientation`-Eigenschaft wie `StackLayout`sowie `ColumnSpacing`- und `RowSpacing`-Eigenschaften wie `Grid`. Darüber hinaus wird mit dieser Klasse die Größe von untergeordneten Elementen zwischengespeichert.

Im Beispiel [**PhotoWrap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoWrap) wird ein `WrapLayout` mit einer `ScrollView` kombiniert, um Fotos anzuzeigen.

### <a name="no-unconstrained-dimensions-allowed"></a>Dimensionen ohne Einschränkungen sind nicht zulässig!

Das [`UniformGridLayout`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/UniformGridLayout.cs) in der [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)-Bibliothek ist so ausgelegt, dass alle untergeordneten Elemente innerhalb des Layouts angezeigt werden. Aus diesem Grund können keine Dimensionen ohne Einschränkungen verarbeitet werden. Wird eine solche Dimension erkannt, wird eine Ausnahme ausgelöst.

Das `UniformGridLayout` wird im Beispiel [**PhotoGrid**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoGrid) veranschaulicht:

[![Screenshot von PhotoGrid](images/ch26fg08-small.png "Einheitliches Rasterlayout")](images/ch26fg08-large.png#lightbox "Einheitliches Rasterlayout")

### <a name="overlapping-children"></a>Überlappende untergeordnete Elemente

Bei einer `Layout<T>`-Ableitung ist eine Überlappung von untergeordneten Elementen möglich. Die untergeordneten Elemente werden jedoch in ihrer Reihenfolge in der `Children`-Auflistung gerendert, nicht in der Reihenfolge, in der ihre `Layout`-Methoden aufgerufen werden.

Die Klasse `Layout` definiert zwei Methoden, mit denen Sie ein untergeordnetes Element innerhalb der Auflistung verschieben können:

- [`LowerChild`](xref:Xamarin.Forms.Layout.LowerChild(Xamarin.Forms.View)), um ein untergeordnetes Element an den Anfang der Auflistung zu verschieben
- [`RaiseChild`](xref:Xamarin.Forms.Layout.RaiseChild(Xamarin.Forms.View)), um ein untergeordnetes Element ans Ende der Auflistung zu verschieben

Bei überlappenden untergeordneten Elementen werden untergeordnete Elemente am Ende der Auflistung oberhalb von untergeordneten Elementen am Anfang der Auflistung angezeigt.

Die Klasse [`OverlapLayout`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/OverlapLayout.cs) in der [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)-Bibliothek definiert eine angefügte Eigenschaft zur Angabe der Renderreihenfolge. Auf diese Weise kann eins der zugehörigen untergeordneten Elemente oberhalb von anderen Elementen angezeigt werden. Dies wird im Beispiel [**StudentCardFile**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/StudentCardFile) veranschaulicht:

[![Screenshot des StudentCardFile-Rasters](images/ch26fg10-small.png "Überlappende Layouts mit untergeordneten Elementen")](images/ch26fg10-large.png#lightbox "Überlappende Layouts mit untergeordneten Elementen")

### <a name="more-attached-bindable-properties"></a>Weitere angefügte bindbare Eigenschaften

Die Klasse [`CartesianLayout`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CartesianLayout.cs) in der [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)-Bibliothek definiert angefügte bindbare Eigenschaften, mit denen zwei `Point`-Werte und ein Wert für die Dicke angegeben werden können. Außerdem werden `BoxView`-Elemente so bearbeitet, dass sie Zeilen ähneln.

Im Beispiel [**UnitCube**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/UnitCube) wird diese Klasse für einen 3D-Würfel verwendet.

### <a name="layout-and-layoutto"></a>Layout und LayoutTo

Eine `Layout<T>`-Ableitung kann `LayoutTo` anstelle von `Layout` aufrufen, um das Layout zu animieren. Für diesen Vorgang wird die Klasse [`AnimatedCartesianLayout`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnimatedCartesianLayout.cs) verwendet, wie im Beispiel [**AnimatedUnitCube**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/AnimatedUnitCube) gezeigt.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 26 – vollständiger Text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch26-Apr2016.pdf)
- [Kapitel 26 – Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26)
- [Erstellen von benutzerdefinierten Layouts](~/xamarin-forms/user-interface/layouts/custom.md)
