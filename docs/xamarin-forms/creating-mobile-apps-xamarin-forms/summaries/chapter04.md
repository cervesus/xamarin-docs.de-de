---
title: Zusammenfassung der Kapitel 4. Scrollen im Stapel
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 4. Scrollen im Stapel'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 7A39FD4F-15AD-4F94-960E-9FEEB63FFD44
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 7860df998fbfe580362aff0f4f01374a4ae1f923
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935552"
---
# <a name="summary-of-chapter-4-scrolling-the-stack"></a>Zusammenfassung der Kapitel 4. Scrollen im Stapel

In diesem Kapitel in erster Linie verwendet wird, in der Einführung des Konzepts der *Layout*, dies ist der allgemeine Begriff für die Klassen und Methoden, die Xamarin.Forms verwendet, um die visuelle Darstellung von mehreren Ansichten auf der Seite zu organisieren.

Layout umfasst mehrere abgeleitete Klassen [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) und [ `Layout<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/). In diesem Kapitel konzentriert sich auf [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/).

Außerdem eine Einführung in diesem Kapitel werden die [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/), [ `Frame` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/), und [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) Klassen.

## <a name="stacks-of-views"></a>Stapel von Ansichten

[`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) leitet sich von `Layout<View>` und erbt einen [ `Children` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) Eigenschaft vom Typ `IList<View>`. Sie Elemente mit mehreren anzeigen, die dieser Sammlung, hinzufügen und `StackLayout` zeigt sie in einem horizontalen oder vertikalen Stapel.

Legen Sie die [ `Orientation` ](xref:Xamarin.Forms.StackLayout.Orientation) Eigenschaft `StackLayout` auf einen Member der [ `StackOrientation` ](xref:Xamarin.Forms.StackOrientation) Enumeration, entweder [ `Vertical` ](xref:Xamarin.Forms.StackOrientation.Vertical) oder [ `Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal). Die Standardeinstellung ist `Vertical`.

Legen Sie die [ `Spacing` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Spacing/) Eigenschaft `StackLayout` auf eine `double` Wert, der einen Abstand zwischen der untergeordneten Elemente angegeben. Der Standardwert ist 6.

Im Code können Sie Elemente hinzufügen das `Children` Auflistung von `StackLayout` in eine `for` oder `foreach` Schleife, wie in der [ **ColorLoop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorLoop) Beispiel, oder Sie können Initialisieren der `Children` Auflistung mit einer Liste der einzelnen Ansichten als im veranschaulicht [ **ColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorList). Die untergeordneten Elemente müssen abgeleitet `View` aber andere `StackLayout` Objekte.

## <a name="scrolling-content"></a>Scrollen von Inhalt

Wenn eine `StackLayout` enthält zu viele untergeordnete Elemente, die auf einer Seite angezeigt können Sie legen die `StackLayout` in einer [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) zum Durchführen eines Bildlaufs zu ermöglichen.

Legen Sie die [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ScrollView.Content/) Eigenschaft `ScrollView` an die Ansicht Bildlauf durchgeführt werden soll. Dies ist häufig eine `StackLayout`, aber es kann Sicht sein.

Legen Sie die [ `Orientation` ](xref:Xamarin.Forms.ScrollView.Orientation) Eigenschaft `ScrollView` auf einen Member der [ `ScrollOrientation` ](xref:Xamarin.Forms.ScrollOrientation) -Eigenschaft [ `Vertical` ](xref:Xamarin.Forms.ScrollOrientation.Vertical), [ `Horizontal` ](xref:Xamarin.Forms.ScrollOrientation.Horizontal), oder [ `Both` ](xref:Xamarin.Forms.ScrollOrientation.Both). Die Standardeinstellung ist `Vertical`. Wenn der Inhalt des eine `ScrollView` ist eine `StackLayout`, die zwei Ausrichtungen müssen konsistent sein.

Die [ **ReflectedColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ReflectedColors) Beispiel veranschaulicht die Verwendung von `ScrollView` und `StackLayout` die verfügbaren Farben angezeigt. Im Beispiel wird auch veranschaulicht, wie .NET Reflektion zu verwenden, um alle öffentlichen statischen Eigenschaften und Felder des erhalten die `Color` Struktur ohne explizit aufgelistet werden.

## <a name="the-expands-option"></a>Die Option erweitert das Element

Wenn eine `StackLayout` Stapel dessen untergeordneten Elementen jedes untergeordnete Element belegt einen bestimmten Slot in die Gesamthöhe des der `StackLayout` , abhängig ist, auf das untergeordnete Element die Größe und die Einstellungen des seine `HorizontalOptions` und `VerticalOptions` Eigenschaften. Diese Eigenschaften werden die Werte des Typs zugewiesen [ `LayoutOptions` ](http://developer.xamstage.com/api/type/Xamarin.Forms.LayoutOptions/).

Die `LayoutOptions` Struktur definiert zwei Eigenschaften:

- [`Alignment`](xref:Xamarin.Forms.LayoutOptions.Alignment) des Enumerationstyps [ `LayoutAlignment` ](xref:Xamarin.Forms.LayoutAlignment) mit vier Elementen, [ `Start` ](xref:Xamarin.Forms.LayoutAlignment.Start), [ `Center` ](xref:Xamarin.Forms.LayoutAlignment.Center), [ `End` ](xref:Xamarin.Forms.LayoutAlignment.End), und [`Fill`](xref:Xamarin.Forms.LayoutAlignment.Fill)
- [`Expands`](xref:Xamarin.Forms.LayoutOptions.Expands) Der Typ `bool`

Der Einfachheit halber die `LayoutOptions` Struktur definiert auch acht statische schreibgeschützte Felder vom Typ `LayoutOptions` , die alle Kombinationen von Eigenschaften für die zwei abdecken:

- [`LayoutOptions.Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/)
- [`LayoutOptions.Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/)
- [`LayoutOptions.End`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/)
- [`LayoutOptions.Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/)
- [`LayoutOptions.StartAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/)
- [`LayoutOptions.CenterAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.CenterAndExpand/)
- [`LayoutOptions.EndAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.EndAndExpand/)
- [`LayoutOptions.FillAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/)

Umfasst die folgende erläuterungen ein `StackLayout` mit einer standardmäßigen vertikalen Ausrichtung. Die horizontale `StackLayout` entspricht.

Bei einem vertikalen `StackLayout`, `HorizontalOptions` Einstellung bestimmt, wie ein untergeordnetes Element innerhalb der Breite des horizontal positioniert wird die `StackLayout`. Ein `Alignment` -Einstellung `Start`, `Center`, oder `End` bewirkt, dass das untergeordnete Element horizontal uneingeschränkte sein. Das untergeordnete Element eine eigene Breite bestimmt und befindet sich auf die Links, zentriert oder rechts neben der `StackLayout`. Die `Fill` Option bewirkt, dass das untergeordnete Element horizontal eingeschränkt werden, und füllt die Breite der `StackLayout`.

Bei einem vertikalen `StackLayout`jedes untergeordnete Element vertikal uneingeschränkt ist, und ruft eine vertikale slot je nach Höhe des untergeordneten Standortes in diesem Fall die `VerticalOptions` Einstellung nicht relevant ist.

Wenn die vertikale `StackLayout` selbst ist uneingeschränkte&mdash;, wenn die `VerticalOptions` Einstellung ist `Start`, `Center`, oder `End`, klicken Sie dann auf die Höhe des der `StackLayout` wird die gesamte Höhe des untergeordneten.

Jedoch wenn die vertikale `StackLayout` wird vertikal eingeschränkt&mdash;wenn seine `VerticalOptions` Einstellung `Fill` &mdash;klicken Sie dann auf die Höhe des der `StackLayout` werden die Höhe des Containers, die möglicherweise größer als die Summe die Höhe der untergeordneten Elemente. Wenn dies der Fall ist und mindestens ein untergeordnetes Element verfügt über eine `VerticalOptions` Einstellung mit einer `Expands` flag von `true`, klicken Sie dann den zusätzlichen Platz in der `StackLayout` wird gleichmäßig auf alle diese untergeordneten zugeordnet ein `Expands` flag von `true`. Die gesamte Höhe der untergeordneten Elemente entspricht die Höhe des klicken Sie dann die `StackLayout`, und die `Alignment` Teil der `VerticalOptions` Einstellung bestimmt, wie das untergeordnete Element in seinem Steckplatz vertikal positioniert wird.

Dies wird veranschaulicht, der [ **VerticalOptionsDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/VerticalOptionsDemo) Beispiel.

## <a name="frame-and-boxview"></a>Frame- und BoxView

Diese beiden rechteckigen Ansichten werden häufig für die Darstellung verwendet werden.

Die [ `Frame` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/) zeigt einen rechteckigen Rahmen um eine andere Ansicht, die ein Layout, z. B. aufweisen kann `StackLayout`. `Frame` erbt einen [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) Eigenschaft [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) , die Sie festlegen, die in angezeigt werden an die Ansicht der `Frame`. Die `Frame` ist standardmäßig transparent. Legen Sie die folgenden drei Eigenschaften des Frames Darstellung anpassen:

- Die [ `OutlineColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.OutlineColor/) Eigenschaft, um es sichtbar zu machen. Es ist üblich, legen Sie `OutlineColor` zu `Color.Accent` , wenn Sie nicht wissen des zugrunde liegenden Farbschemas.
- Die [ `HasShadow` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.HasShadow/) Eigenschaft kann festgelegt werden, um `true` einen schwarzen Schatten auf iOS-Geräten angezeigt.
- Legen Sie die [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) Eigenschaft, um eine `Thickness` Wert, der ein Leerzeichen zwischen der und der Frame des Inhalts. Der Standardwert ist 20 Einheiten auf allen Seiten.

Die `Frame` Standard `HorizontalOptions` und `VerticalOptions` Werte `LayoutOptions.Fill`, was bedeutet, dass die `Frame` füllt den Container. Mit anderen Einstellungen, die Größe der `Frame` basiert auf der Größe des Inhalts.

Die `Frame` wird veranschaulicht, der [ **FramedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FramedText) Beispiel.

Die [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) zeigt einen rechteckigen Bereich der Farbe, die gemäß der [ `Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) Eigenschaft.

Wenn die `BoxView` beschränkt (die `HorizontalOptions` und `VerticalOptions` Eigenschaften verfügen, die Standardeinstellungen des `LayoutOptions.Fill`), wird die `BoxView` füllt den verfügbaren Speicherplatz dafür. Wenn die `BoxView` uneingeschränkte ist (mit `HorizontalOptions` und `LayoutOptions` Einstellungen `Start`, `Center`, oder `End`), eine Standarddimension 40 Einheiten Quadrats hat. Ein `BoxView` in einer Dimension beschränkt und im anderen uneingeschränkt werden kann.

Legen Sie häufig die der [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) und [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) Eigenschaften `BoxView` um es zu eine bestimmte Größe gewähren. Zum besseren Verständnis der [ **SizedBoxView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/SizedBoxView) Beispiel.

Können Sie mehrere Instanzen von `StackLayout` kombiniert eine `BoxView` und mehrere `Label` -Instanzen in einer `Frame` Anzeigen einer bestimmten Farbe an, und klicken Sie dann jede dieser Ansichten in eine `StackLayout` in eine `ScrollView` der attraktive erstellen Liste der Farben angezeigt, der [ **ColorBlocks** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorBlocks) Beispiel:

[![Dreifacher Screenshot der Farbblöcke](images/ch04fg11-small.png "Liste von Farben")](images/ch04fg11-large.png#lightbox "Liste von Farben")

## <a name="a-scrollview-in-a-stacklayout"></a>Eine ScrollView in einem StackLayout?

Einfügen einer `StackLayout` in eine `ScrollView` ist häufig, aber eben eine `ScrollView` in einer `StackLayout` eignet sich auch in einigen Fällen. In der Theorie, dies sollte nicht möglich, da die untergeordneten Elemente eines vertikalen `StackLayout` werden vertikal uneingeschränkt. Aber ein `ScrollView` vertikal eingeschränkt werden müssen. Es muss eine bestimmte Höhe zugewiesen werden, sodass für den Bildlauf klicken Sie dann die Größe seines untergeordneten ermittelt werden können.

Der Trick besteht darin, geben Sie die `ScrollView` untergeordnetes Element des der `StackLayout` eine `VerticalOptions` -Einstellung `FillAndExpand`. Dies wird veranschaulicht, der [ **BlackCat** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCat) Beispiel.

Die **BlackCat** Beispiel zeigt auch zum Definieren und die Anwendung zugreifen, die in der portablen Klassenbibliothek (PCL) eingebettet sind. Dies kann auch mit freigegebenen Asset-Projekten (SAPs) erreicht werden, aber der Prozess ist ein wenig komplizierter, als die [ **BlackCatSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCatSap) veranschaulicht.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 4 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch04-Apr2016.pdf)
- [Kapitel 4-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)
- [Kapitel 4 F#-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FS)
- [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)
- [ScrollView](~/xamarin-forms/user-interface/layouts/scroll-view.md)
