---
title: Zusammenfassung zu Kapitel 4. Scrollen des Stapels
description: 'Erstellen von Mobile Apps mit xamarin. Forms: Zusammenfassung von Kapitel 4. Scrollen des Stapels'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 7A39FD4F-15AD-4F94-960E-9FEEB63FFD44
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: bda9d5cb323524981bed9c3bb55998513dd69aab
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032877"
---
# <a name="summary-of-chapter-4-scrolling-the-stack"></a>Zusammenfassung zu Kapitel 4. Scrollen des Stapels

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)

In diesem Kapitel wird in erster Linie das Konzept des *Layouts*vorgestellt. Dies ist der allgemeine Begriff für die Klassen und Techniken, die xamarin. Forms zum Organisieren der visuellen Darstellung mehrerer Ansichten auf der Seite verwendet.

Das Layout umfasst mehrere Klassen, die von [`Layout`](xref:Xamarin.Forms.Layout) und [`Layout<T>`](xref:Xamarin.Forms.Layout`1)abgeleitet werden. In diesem Kapitel liegt der Schwerpunkt auf [`StackLayout`](xref:Xamarin.Forms.StackLayout).

> [!NOTE]
> Der in xamarin. Forms 3,0 eingeführte [`FlexLayout`](~/xamarin-forms/user-interface/layouts/flex-layout.md) kann auf ähnliche Weise verwendet werden wie `StackLayout`, aber mit mehr Flexibilität.

In diesem Kapitel werden auch die Klassen [`ScrollView`](xref:Xamarin.Forms.ScrollView), [`Frame`](xref:Xamarin.Forms.Frame)und [`BoxView`](xref:Xamarin.Forms.BoxView) vorgestellt.

## <a name="stacks-of-views"></a>Stapel von Sichten

[`StackLayout`](xref:Xamarin.Forms.StackLayout) von `Layout<View>` abgeleitet und erbt eine [`Children`](xref:Xamarin.Forms.Layout`1) -Eigenschaft vom Typ `IList<View>`. Sie fügen dieser Sammlung mehrere Ansichts Elemente hinzu und `StackLayout` Sie in einem horizontalen oder vertikalen Stapel anzeigen.

Legen Sie die [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) -Eigenschaft von `StackLayout` auf einen Member der [`StackOrientation`](xref:Xamarin.Forms.StackOrientation) -Enumeration fest, entweder [`Vertical`](xref:Xamarin.Forms.StackOrientation.Vertical) oder [`Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal). Der Standardwert ist `Vertical`.

Legen Sie die [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) -Eigenschaft von `StackLayout` auf einen `double` Wert fest, um einen Abstand zwischen den untergeordneten Elementen anzugeben. Der Standardwert ist 6.

Im Code können Sie der `Children` Auflistung von `StackLayout` in einer `for` oder `foreach` Schleife Elemente hinzufügen, wie im [**colorloop**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorLoop) -Beispiel veranschaulicht, oder Sie können die `Children` Auflistung mit einer Liste der einzelnen Ansichten initialisieren, wie in [**colorlist gezeigt.** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorList). Die untergeordneten Elemente müssen von `View` abgeleitet werden, können aber auch andere `StackLayout` Objekte enthalten.

## <a name="scrolling-content"></a>Scrollen von Inhalten

Wenn eine `StackLayout` zu viele untergeordnete Elemente enthält, die auf einer Seite angezeigt werden sollen, können Sie die `StackLayout` in einem [`ScrollView`](xref:Xamarin.Forms.ScrollView) ablegen, um den Bildlauf zuzulassen.

Legen Sie die [`Content`](xref:Xamarin.Forms.ScrollView.Content) -Eigenschaft von `ScrollView` auf die Ansicht fest, die Sie scrollen möchten. Dies ist oft eine `StackLayout`, aber es kann eine beliebige Ansicht sein.

Legen Sie die [`Orientation`](xref:Xamarin.Forms.ScrollView.Orientation) -Eigenschaft von `ScrollView` auf einen Member der [`ScrollOrientation`](xref:Xamarin.Forms.ScrollOrientation) -Eigenschaft fest, [`Vertical`](xref:Xamarin.Forms.ScrollOrientation.Vertical), [`Horizontal`](xref:Xamarin.Forms.ScrollOrientation.Horizontal)oder [`Both`](xref:Xamarin.Forms.ScrollOrientation.Both). Der Standardwert ist `Vertical`. Wenn der Inhalt einer `ScrollView` ein `StackLayout`ist, sollten die beiden Ausrichtungen konsistent sein.

Das [**reflectedcolors**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ReflectedColors) -Beispiel veranschaulicht die Verwendung von `ScrollView` und `StackLayout`, um die verfügbaren Farben anzuzeigen. Das Beispiel zeigt auch, wie die .NET-Reflektion verwendet wird, um alle öffentlichen statischen Eigenschaften und Felder der `Color` Struktur abzurufen, ohne diese explizit auflisten zu müssen.

## <a name="the-expands-option"></a>Die erweitert-Option

Wenn ein `StackLayout` seine untergeordneten Elemente stapelt, belegt jedes untergeordnete Element einen bestimmten Slot innerhalb der Gesamthöhe des `StackLayout`, der von der Größe des untergeordneten Elements und den Einstellungen seiner `HorizontalOptions` und `VerticalOptions` Eigenschaften abhängt. Diesen Eigenschaften werden Werte vom Typ [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions)zugewiesen.

Die `LayoutOptions`-Struktur definiert zwei Eigenschaften:

- [`Alignment`](xref:Xamarin.Forms.LayoutOptions.Alignment) des Enumerationstyps [`LayoutAlignment`](xref:Xamarin.Forms.LayoutAlignment) mit vier Membern, [`Start`](xref:Xamarin.Forms.LayoutAlignment.Start), [`Center`](xref:Xamarin.Forms.LayoutAlignment.Center), [`End`](xref:Xamarin.Forms.LayoutAlignment.End)und [`Fill`](xref:Xamarin.Forms.LayoutAlignment.Fill)
- [`Expands`](xref:Xamarin.Forms.LayoutOptions.Expands) vom Typ `bool`

Der `LayoutOptions` Struktur definiert auch acht statische schreibgeschützte Felder vom Typ `LayoutOptions`, die alle Kombinationen der beiden Instanzeigenschaften umfassen:

- [`LayoutOptions.Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`LayoutOptions.Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`LayoutOptions.End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`LayoutOptions.StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`LayoutOptions.CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`LayoutOptions.EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

Die folgende Erörterung umfasst eine `StackLayout` mit einer vertikalen Standardausrichtung. Der horizontale `StackLayout` ist analog.

Bei einer vertikalen `StackLayout`bestimmt die `HorizontalOptions` Einstellung, wie ein untergeordnetes Element innerhalb der Breite des `StackLayout`horizontal positioniert wird. Eine `Alignment` Einstellung von `Start`, `Center`oder `End` bewirkt, dass das untergeordnete Element horizontal nicht eingeschränkt wird. Das untergeordnete Element bestimmt seine eigene Breite und befindet sich Links, zentriert oder rechts von der `StackLayout`. Die Option `Fill` bewirkt, dass das untergeordnete Element horizontal eingeschränkt wird und die Breite des `StackLayout`füllt.

Bei einem vertikalen `StackLayout`wird jedes untergeordnete Element vertikal eingeschränkt, und es wird ein vertikaler Slot abhängig von der Höhe des Kindes abgerufen. in diesem Fall ist die `VerticalOptions` Einstellung irrelevant.

Wenn die vertikale `StackLayout` selbst nicht eingeschränkt ist&mdash;wenn die `VerticalOptions` Einstellung `Start`, `Center`oder `End`ist, entspricht die Höhe des `StackLayout` der Gesamthöhe der untergeordneten Elemente.

Wenn jedoch die vertikale `StackLayout` vertikal eingeschränkt ist&mdash;wenn die `VerticalOptions` Einstellung `Fill`ist&mdash;dann ist die Höhe des `StackLayout` die Höhe des Containers, der möglicherweise größer als die Gesamthöhe seiner untergeordneten Elemente ist. Wenn dies der Fall ist und mindestens ein untergeordnetes Element über eine `VerticalOptions`-Einstellung mit einem `Expands`-Flag von `true`verfügt, wird der zusätzliche Speicherplatz in der `StackLayout` gleichmäßig unter allen diesen untergeordneten Elementen mit dem `Expands` Flag `true`zugeordnet. Die Gesamthöhe der untergeordneten Elemente entspricht dann der Höhe des `StackLayout`, und der `Alignment` Teil der `VerticalOptions` Einstellung bestimmt, wie das untergeordnete Element vertikal im Slot positioniert wird.

Dies wird im [**verticaloptionsdemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/VerticalOptionsDemo) -Beispiel veranschaulicht.

## <a name="frame-and-boxview"></a>Frame und boxview

Diese beiden rechteckigen Sichten werden häufig für Präsentationszwecke verwendet.

Die [`Frame`](xref:Xamarin.Forms.Frame) Ansicht zeigt einen rechteckigen Rahmen um eine weitere Ansicht an, bei dem es sich um ein Layout wie `StackLayout`handeln kann. `Frame` erbt eine [`Content`](xref:Xamarin.Forms.ContentView.Content) Eigenschaft von [`ContentView`](xref:Xamarin.Forms.ContentView) , die Sie auf die Ansicht festlegen, die innerhalb der `Frame`angezeigt werden soll. Der `Frame` ist standardmäßig transparent. Legen Sie die folgenden drei Eigenschaften fest, um die Darstellung des Frames anzupassen:

- Die [`OutlineColor`](xref:Xamarin.Forms.Frame.OutlineColor) -Eigenschaft, um Sie sichtbar zu machen. Es ist üblich, `OutlineColor` auf `Color.Accent` festzulegen, wenn Sie das zugrunde liegende Farbschema nicht kennen.
- Die [`HasShadow`](xref:Xamarin.Forms.Frame.HasShadow) -Eigenschaft kann auf `true` festgelegt werden, um einen schwarzen Schatten auf IOS-Geräten anzuzeigen.
- Legen Sie die [`Padding`](xref:Xamarin.Forms.Layout.Padding) -Eigenschaft auf einen `Thickness` Wert fest, um ein Leerzeichen zwischen dem Frame und dem Inhalt des Frames zu belassen. Der Standardwert ist 20 Einheiten auf allen Seiten.

Der `Frame` verfügt über Standardwerte für `HorizontalOptions` und `VerticalOptions` von `LayoutOptions.Fill`, was bedeutet, dass der `Frame` seinen Container ausfüllen wird. Bei anderen Einstellungen basiert die Größe der `Frame` auf der Größe des Inhalts.

Die `Frame` wird im [**framedtext**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FramedText) -Beispiel veranschaulicht.

Die [`BoxView`](xref:Xamarin.Forms.BoxView) zeigt einen rechteckigen Bereich von Farben an, der durch die [`Color`](xref:Xamarin.Forms.BoxView.Color) -Eigenschaft angegeben wird.

Wenn die `BoxView` eingeschränkt ist (die Eigenschaften `HorizontalOptions` und `VerticalOptions` haben die Standardeinstellungen `LayoutOptions.Fill`), füllt der `BoxView` den verfügbaren Speicherplatz aus. Wenn das `BoxView` nicht eingeschränkt ist (mit `HorizontalOptions` und `LayoutOptions` Einstellungen von `Start`, `Center`oder `End`), hat es eine Standard Dimension von 40 Einheiten Square. Eine `BoxView` kann in einer Dimension eingeschränkt und in der anderen nicht eingeschränkt werden.

Häufig legen Sie die [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) -und [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) Eigenschaften von `BoxView` fest, um eine bestimmte Größe anzugeben. Dies wird durch das [**sizedboxview**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/SizedBoxView) -Beispiel veranschaulicht.

Sie können mehrere Instanzen von `StackLayout` verwenden, um eine `BoxView` und mehrere `Label` Instanzen in einer `Frame` zu kombinieren, um eine bestimmte Farbe anzuzeigen, und dann jede dieser Ansichten in einer `StackLayout` in einer `ScrollView` ablegen, um die attraktive Liste der Farben zu erstellen. im [**colorblocks**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorBlocks) -Beispiel gezeigt:

[![Dreifacher Screenshot von Farbblöcken](images/ch04fg11-small.png "Liste der Farben")](images/ch04fg11-large.png#lightbox "Liste der Farben")

## <a name="a-scrollview-in-a-stacklayout"></a>Eine ScrollView in einem Stacklayout?

Das Einfügen einer `StackLayout` in eine `ScrollView` ist häufig, aber auch das Einfügen eines `ScrollView` in einem `StackLayout` ist manchmal praktisch. Theoretisch sollte dies nicht möglich sein, da die untergeordneten Elemente eines vertikalen `StackLayout` vertikal nicht eingeschränkt sind. Eine `ScrollView` muss jedoch vertikal eingeschränkt werden. Ihm muss eine bestimmte Höhe zugewiesen werden, damit die Größe des untergeordneten Elements für den Bildlauf bestimmt werden kann.

Der Trick besteht darin, dem `ScrollView` untergeordneten `StackLayout` eine `VerticalOptions` Einstellung `FillAndExpand`zu übergeben. Dies wird im Beispiel [**BlackCat**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCat) veranschaulicht.

Das **BlackCat** -Beispiel veranschaulicht auch, wie Sie Programmressourcen definieren und auf diese zugreifen können, die in die freigegebene Bibliothek eingebettet sind. Dies kann auch mit Shared Asset Projects (SAPS) erreicht werden, aber der Prozess ist ein wenig schwieriger, wie im Beispiel [**blackkatsap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCatSap) veranschaulicht.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 4 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch04-Apr2016.pdf)
- [Kapitel 4 Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)
- [Kapitel 4 F# Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FS)
- [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)
- [ScrollView](~/xamarin-forms/user-interface/layouts/scroll-view.md)
- [BoxView](~/xamarin-forms/user-interface/boxview.md)
