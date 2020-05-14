---
title: Zusammenfassung von Kapitel 4. Scrollen im Stapel
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung von Kapitel 4. Scrollen im Stapel'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 7A39FD4F-15AD-4F94-960E-9FEEB63FFD44
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 5313dd34839d6a5d21432161b9fd3a0ffce6e816
ms.sourcegitcommit: 83cf2a4d99546751c6394510a463a2b2a8bf75b8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2020
ms.locfileid: "83149937"
---
# <a name="summary-of-chapter-4-scrolling-the-stack"></a>Zusammenfassung von Kapitel 4. Scrollen im Stapel

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)

In diesem Kapitel wird in erster Linie das Konzept des *Layouts* eingeführt, wobei es sich um den allgemeinen Begriff für die Klassen und Methoden handelt, die Xamarin.Forms zum Organisieren der visuellen Darstellung mehrerer Ansichten auf der Seite verwendet.

Layout umfasst mehrere Klassen, die von [`Layout`](xref:Xamarin.Forms.Layout) und [`Layout<T>`](xref:Xamarin.Forms.Layout`1) abgeleitet werden. Dieses Kapitel konzentriert sich auf [`StackLayout`](xref:Xamarin.Forms.StackLayout).

> [!NOTE]
> Das [`FlexLayout`](~/xamarin-forms/user-interface/layouts/flex-layout.md), das in Xamarin.Forms 3.0 eingeführt wurde, kann auf ähnliche Weisen wie `StackLayout` verwendet werden, jedoch mit größerer Flexibilität.

In diesem Kapitel werden außerdem die Klassen [`ScrollView`](xref:Xamarin.Forms.ScrollView), [`Frame`](xref:Xamarin.Forms.Frame) und [`BoxView`](xref:Xamarin.Forms.BoxView) eingeführt.

## <a name="stacks-of-views"></a>Stapel von Ansichten

[`StackLayout`](xref:Xamarin.Forms.StackLayout) wird von `Layout<View>` abgeleitet und erbt eine [`Children`](xref:Xamarin.Forms.Layout`1)-Eigenschaft vom Typ `IList<View>`. Sie fügen dieser Sammlung mehrere Ansichtselemente hinzu, und `StackLayout` zeigt diese in einem horizontalen oder vertikalen Stapel an.

Legen Sie die [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation)-Eigenschaft von `StackLayout` auf einen Member der [`StackOrientation`](xref:Xamarin.Forms.StackOrientation)-Enumeration fest, entweder [`Vertical`](xref:Xamarin.Forms.StackOrientation.Vertical) oder [`Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal). Der Standardwert ist `Vertical`.

Legen Sie die [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing)-Eigenschaft von `StackLayout` auf einen `double`-Wert fest, um einen Abstand zwischen den untergeordneten Elementen anzugeben. Der Standardwert ist 6.

In Code können Sie der `Children`-Sammlung von `StackLayout` in einer `for`- oder `foreach`-Schleife Elemente hinzufügen, wie im [**ColorLoop**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorLoop)-Beispiel veranschaulicht, oder Sie können die `Children`-Sammlung mit einer Liste der einzelnen Ansichten initialisieren, wie in [**ColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorList) gezeigt. Die untergeordneten Elemente müssen von `View` abgeleitet werden, können aber andere `StackLayout`-Objekte enthalten.

## <a name="scrolling-content"></a>Scrollen von Inhalt

Wenn ein `StackLayout` zu viele untergeordnete Elemente enthält, um sie auf einer Seite anzuzeigen, können Sie das `StackLayout` in einer [`ScrollView`](xref:Xamarin.Forms.ScrollView) platzieren, um Scrollen zuzulassen.

Legen Sie die [`Content`](xref:Xamarin.Forms.ScrollView.Content)-Eigenschaft von `ScrollView` auf die Ansicht fest, die gescrollt werden können soll. Dies ist häufig ein `StackLayout`, kann aber jede beliebige Ansicht sein.

Legen Sie die [`Orientation`](xref:Xamarin.Forms.ScrollView.Orientation)-Eigenschaft von `ScrollView` auf einen Member der [`ScrollOrientation`](xref:Xamarin.Forms.ScrollOrientation)-Eigenschaft fest, [`Vertical`](xref:Xamarin.Forms.ScrollOrientation.Vertical), [`Horizontal`](xref:Xamarin.Forms.ScrollOrientation.Horizontal) oder [`Both`](xref:Xamarin.Forms.ScrollOrientation.Both). Der Standardwert ist `Vertical`. Wenn der Inhalt einer `ScrollView` ein `StackLayout` ist, sollten die beiden Ausrichtungen konsistent sein.

Das [**ReflectedColors**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ReflectedColors)-Beispiel veranschaulicht die Verwendung von `ScrollView` und `StackLayout`, um die verfügbaren Farben anzuzeigen. In dem Beispiel wird ebenfalls gezeigt, wie .NET-Reflexion verwendet wird, um alle öffentlichen, statischen Eigenschaften und Felder der `Color`-Struktur abzurufen, ohne diese explizit auflisten zu müssen.

## <a name="the-expands-option"></a>Die Option „Expands“

Wenn ein `StackLayout` seine untergeordneten Elemente stapelt, belegt jedes untergeordnete Element einen bestimmten Slot innerhalb der Gesamthöhe des `StackLayout`, der von der Größe des untergeordneten Elements und den Einstellungen seiner `HorizontalOptions`- und `VerticalOptions`-Eigenschaften abhängt. Diesen Eigenschaften werden Werte vom Typ [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) zugewiesen.

Die `LayoutOptions`-Struktur definiert zwei Eigenschaften:

- [`Alignment`](xref:Xamarin.Forms.LayoutOptions.Alignment) des Enumerationstyps [`LayoutAlignment`](xref:Xamarin.Forms.LayoutAlignment) mit vier Membern, [`Start`](xref:Xamarin.Forms.LayoutAlignment.Start), [`Center`](xref:Xamarin.Forms.LayoutAlignment.Center), [`End`](xref:Xamarin.Forms.LayoutAlignment.End) und [`Fill`](xref:Xamarin.Forms.LayoutAlignment.Fill).
- [`Expands`](xref:Xamarin.Forms.LayoutOptions.Expands) vom Typ `bool`

Zu Ihrer Bequemlichkeit definiert die `LayoutOptions`-Struktur definiert auch acht statische, schreibgeschützte Felder vom Typ `LayoutOptions`, die alle Kombinationen der beiden Instanzeigenschaften umfassen:

- [`LayoutOptions.Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`LayoutOptions.Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`LayoutOptions.End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`LayoutOptions.StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`LayoutOptions.CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`LayoutOptions.EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

Die folgende Erörterung umfasst ein `StackLayout` mit einer vertikalen Standardausrichtung. Das horizontale `StackLayout` ist analog.

Bei einem vertikalen `StackLayout` bestimmt die `HorizontalOptions`-Einstellung, wie ein untergeordnetes Element innerhalb der Breite des `StackLayout` horizontal positioniert wird. Eine `Alignment`-Einstellung von `Start`, `Center` oder `End` bewirkt, dass das untergeordnete Element horizontal uneingeschränkt ist. Das untergeordnete Element bestimmt seine eigene Breite und wird links, zentriert oder rechts vom `StackLayout` positioniert. Die Option `Fill` sorgt dafür, dass das untergeordnete Element horizontal eingeschränkt ist und die Breite des `StackLayout` ausfüllt.

Bei einem vertikalen `StackLayout` ist jedes untergeordnete Element vertikal uneingeschränkt und erhält einen vertikalen Slot, der von der Höhe des untergeordneten Elements abhängig ist, sodass in diesem Fall die Einstellung `VerticalOptions` irrelevant ist.

Wenn das vertikale `StackLayout` selbst uneingeschränkt ist – wenn also seine `VerticalOptions`-Einstellung `Start`, `Center` oder `End` ist, entspricht die Höhe des `StackLayout` der Gesamthöhe seiner untergeordneten Elemente.

Wenn jedoch das vertikale `StackLayout` vertikal eingeschränkt ist – wenn also seine `VerticalOptions`-Einstellung `Fill` ist, dann ist die Höhe des `StackLayout` die Höhe seines Containers, der möglicherweise größer als die Gesamthöhe seiner untergeordneten Elemente sein kann. Wenn dies der Fall ist und mindestens ein untergeordnetes Element über eine `VerticalOptions`-Einstellung mit einem `Expands`-Flag von `true` verfügt, wird der zusätzliche Platz im `StackLayout` gleichmäßig allen diesen untergeordneten Elementen mit einem `Expands`-Flag von `true` zugeordnet. Die Gesamthöhe der untergeordneten Elemente entspricht dann der Höhe des `StackLayout`, und der `Alignment`-Teil der `VerticalOptions`-Einstellung bestimmt, wie das untergeordnete Element vertikal im Slot positioniert wird.

Dies wird im [**VerticalOptionsDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/VerticalOptionsDemo)-Beispiel veranschaulicht.

## <a name="frame-and-boxview"></a>„Frame“ und „BoxView“

Diese beiden rechteckigen Ansichten werden häufig zu Präsentationszwecken verwendet.

Die [`Frame`](xref:Xamarin.Forms.Frame)-Ansicht zeigt einen rechteckigen Rahmen um eine weitere Ansicht an, bei der es sich um ein Layout wie `StackLayout` handeln kann. `Frame` erbt eine [`Content`](xref:Xamarin.Forms.ContentView.Content)-Eigenschaft von [`ContentView`](xref:Xamarin.Forms.ContentView), die Sie auf die Ansicht festlegen, die im `Frame` angezeigt werden soll. Der `Frame` ist standardmäßig transparent. Legen Sie die folgenden drei Eigenschaften fest, um die Darstellung des Frames anzupassen:

- Die [`OutlineColor`](xref:Xamarin.Forms.Frame.OutlineColor)-Eigenschaft, um ihn sichtbar zu machen. Es ist üblich, `OutlineColor` auf `Color.Accent` festzulegen, wenn Sie das zugrunde liegende Farbschema nicht kennen.
- Die [`HasShadow`](xref:Xamarin.Forms.Frame.HasShadow)-Eigenschaft kann auf `true` festgelegt werden, um einen schwarzen Schatten auf iOS-Geräten anzuzeigen.
- Legen Sie die [`Padding`](xref:Xamarin.Forms.Layout.Padding)-Eigenschaft auf einen `Thickness`-Wert fest, um zwischen dem Frame und seinem Inhalt Platz zu lassen. Der Standardwert beträgt 20 Einheiten auf allen Seiten.

Der `Frame` verfügt über Standardwerte für `HorizontalOptions` und `VerticalOptions` von `LayoutOptions.Fill`, was bedeutet, dass der `Frame` seinen Container ausfüllt. Bei anderen Einstellungen basiert die Größe des `Frame` auf der Größe seines Inhalts.

Der `Frame` wird im [**FramedText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FramedText)-Beispiel demonstriert.

Die [`BoxView`](xref:Xamarin.Forms.BoxView) zeigt einen rechteckigen Bereich mit einer Farbe an, die durch ihre [`Color`](xref:Xamarin.Forms.BoxView.Color)-Eigenschaft bestimmt wird.

Wenn die `BoxView` eingeschränkt ist (die Standardeinstellungen ihrer Eigenschaften `HorizontalOptions` und `VerticalOptions` sind `LayoutOptions.Fill`), füllt die `BoxView` den für sie verfügbaren Platz aus. Wenn die `BoxView` uneingeschränkt ist (mit `HorizontalOptions`- und `LayoutOptions`-Einstellungen von `Start`, `Center` oder `End`), hat sie eine Standarddimension von 40 Einheiten im Quadrat. Eine `BoxView` kann in einer Dimension eingeschränkt und in einer anderen uneingeschränkt sein.

Häufig legen Sie die Eigenschaften [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) und [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) von `BoxView` fest, um eine bestimmte Größe für sie festzulegen. Dies wird im [**SizedBoxView**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/SizedBoxView)-Beispiel illustriert.

Sie können mehrere Instanzen von `StackLayout` verwenden, um eine `BoxView` und mehrere `Label`-Instanzen in einem `Frame` zu kombinieren, um eine bestimmte Farbe anzuzeigen. Anschließend platzieren Sie jede dieser Ansichten in einem `StackLayout` in einer `ScrollView`, um die attraktive Liste mit Farben zu erstellen, die im [**ColorBlocks**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorBlocks)-Beispiel zu sehen ist:

[![Dreifacher Screenshot von Farbblöcken](images/ch04fg11-small.png "Liste mit Farben")](images/ch04fg11-large.png#lightbox "Liste mit Farben")

## <a name="a-scrollview-in-a-stacklayout"></a>Eine „ScrollView“ in einem „StackLayout“?

Es ist gängig, ein `StackLayout` in eine `ScrollView` einzufügen, aber auch das Einfügen einer `ScrollView` in ein `StackLayout` kann manchmal praktisch sein. Theoretisch sollte dies nicht möglich sein, da die untergeordneten Elemente eines vertikalen `StackLayout` vertikal uneingeschränkt sind. Eine `ScrollView` muss jedoch vertikal eingeschränkt sein. Ihr muss eine bestimmte Höhe zugewiesen werden, damit dann die Größe ihres untergeordneten Elements für das Scrollen bestimmt werden kann.

Der Trick besteht darin, eine `VerticalOptions`-Einstellung des untergeordneten `ScrollView`-Elements von `StackLayout` auf `FillAndExpand` festzulegen. Dies wird im [**BlackCat**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCat)-Beispiel demonstriert.

Im **BlackCat**-Beispiel wird außerdem veranschaulicht, wie Sie Programmressourcen, die in die freigegebene Bibliothek eingebettet sind, definieren und auf diese zugreifen können. Dies kann auch mit Shared Asset Projects (SAPs) erreicht werden, aber der Prozess ist etwas komplexer, wie im [**BlackCatSap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCatSap)-Beispiel zu sehen ist.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 4 – vollständiger Text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch04-Apr2016.pdf)
- [Kapitel 4 – Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)
- [Kapitel 4 – F#-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FS)
- [StackLayout](~/xamarin-forms/user-interface/layouts/stacklayout.md)
- [ScrollView](~/xamarin-forms/user-interface/layouts/scroll-view.md)
- [BoxView](~/xamarin-forms/user-interface/boxview.md)
