---
title: Zusammenfassung der Kapitel 4. Durchführen eines Bildlaufs im Stapel
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 7A39FD4F-15AD-4F94-960E-9FEEB63FFD44
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 09a9d21b7979e0eb3f12d3b1207f2185e059f65c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-4-scrolling-the-stack"></a>Zusammenfassung der Kapitel 4. Durchführen eines Bildlaufs im Stapel

In diesem Kapitel wird in erster Linie gewidmet Einführung in das Konzept der *Layout*, dies ist der allgemeine Begriff für die Klassen und Techniken, mit denen Xamarin.Forms nutzt, um die visuelle Darstellung von mehreren Ansichten auf der Seite zu organisieren.

Layout umfasst mehrere abgeleitete Klassen [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) und [ `Layout<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/). In diesem Kapitel behandelt schwerpunktmäßig [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/).

Außerdem eine Einführung in diesem Kapitel werden die [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/), [ `Frame` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/), und [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) Klassen.

## <a name="stacks-of-views"></a>Aufruflisten von Ansichten

[`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) leitet sich von `Layout<View>` und erbt einen [ `Children` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) Eigenschaft vom Typ `IList<View>`. Sie mehrere Elemente von anzeigen, die dieser Sammlung hinzufügen und `StackLayout` in einem horizontalen oder vertikalen Stapel angezeigt.

Legen Sie die [ `Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Orientation/) Eigenschaft `StackLayout` an ein Mitglied der [ `StackOrientation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackOrientation/) -Enumeration, entweder [ `Vertical` ](https://developer.xamarin.com/api/field/Xamarin.Forms.StackOrientation.Vertical/) oder [ `Horizontal`](https://developer.xamarin.com/api/field/Xamarin.Forms.StackOrientation.Horizontal/). Die Standardeinstellung ist `Vertical`.

Festlegen der [ `Spacing` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Spacing/) Eigenschaft `StackLayout` auf eine `double` -Wert an einen Abstand zwischen den untergeordneten Elementen. Der Standardwert ist 6.

Im Code können Sie Elemente zum Hinzufügen der `Children` Auflistung von `StackLayout` in einer `for` oder `foreach` Schleife, wie in der [ **ColorLoop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorLoop) Beispieldaten, oder Sie können Initialisieren der `Children` Auflistung mit einer Liste der einzelnen Ansichten wie in [ **ColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorList). Die untergeordneten Elemente müssen von abgeleitet `View` können jedoch andere `StackLayout` Objekte.

## <a name="scrolling-content"></a>Durchführen eines Bildlaufs Inhalt

Wenn eine `StackLayout` enthält zu viele Elemente auf einer Seite anzeigen können Sie put der `StackLayout` in einer [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) zum Durchführen eines Bildlaufs zulassen.

Legen Sie die [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ScrollView.Content/) Eigenschaft `ScrollView` zur Ansicht Bildlauf durchgeführt werden soll. Dies ist häufig ein `StackLayout`, aber es kann eine beliebige Ansicht handeln.

Legen Sie die [ `Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ScrollView.Orientation/) Eigenschaft `ScrollView` an ein Mitglied der [ `ScrollOrientation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollOrientation/) -Eigenschaft, [ `Vertical` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ScrollOrientation.Vertical/), [ `Horizontal` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ScrollOrientation.Horizontal/), oder [ `Both` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ScrollOrientation.Both/). Die Standardeinstellung ist `Vertical`. Wenn der Inhalt des eine `ScrollView` ist eine `StackLayout`, sollten die zwei Ausrichtungen konsistent sein.

Die [ **ReflectedColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ReflectedColors) Beispiel veranschaulicht die Verwendung von `ScrollView` und `StackLayout` die verfügbaren Farben angezeigt. Das Beispiel veranschaulicht, wie .NET Reflektion verwenden, um alle öffentlichen statischen Eigenschaften und Felder des erhalten auch die `Color` Struktur ohne die Notwendigkeit, explizit aufgelistet.

## <a name="the-expands-option"></a>Die Option erweitert das Element

Wenn eine `StackLayout` Stapel seiner untergeordneten Elemente jedes untergeordnete Element belegt einen bestimmten Slot in die Gesamthöhe des der `StackLayout` , die Größe des untergeordneten Standorts und der Einstellungen des hängt seine `HorizontalOptions` und `VerticalOptions` Eigenschaften. Diese Eigenschaften sind Werte vom Typ zugewiesen [ `LayoutOptions` ](http://developer.xamstage.com/api/type/Xamarin.Forms.LayoutOptions/).

Die `LayoutOptions` Struktur definiert zwei Eigenschaften:

- [`Alignment`](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Alignment/) des Enumerationstyps [ `LayoutAlignment` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutAlignment/) mit vier Elementen [ `Start` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Start/), [ `Center` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Center/), [ `End` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.End/), und [`Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Fill/)
- [`Expands`](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Expands/) vom Typ `bool`

Der Einfachheit halber der `LayoutOptions` Struktur definiert auch acht statische schreibgeschützte Felder des Typs `LayoutOptions` , die alle Kombinationen aus den zwei Instanzeigenschaften abdecken:

- [`LayoutOptions.Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/)
- [`LayoutOptions.Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/)
- [`LayoutOptions.End`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/)
- [`LayoutOptions.Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/)
- [`LayoutOptions.StartAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/)
- [`LayoutOptions.CenterAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.CenterAndExpand/)
- [`LayoutOptions.EndAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.EndAndExpand/)
- [`LayoutOptions.FillAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/)

Die folgende Darstellung bietet umfasst eine `StackLayout` mit standardmäßige vertikale Ausrichtung. Die horizontale `StackLayout` entspricht.

Bei einem vertikalen `StackLayout`, `HorizontalOptions` Einstellung bestimmt, wie ein untergeordnetes Element innerhalb der Breite des horizontal positioniert wird die `StackLayout`. Ein `Alignment` Einstellung des `Start`, `Center`, oder `End` bewirkt, dass das untergeordnete Element horizontal uneingeschränkte sein. Das untergeordnete Element seine eigenen Breite bestimmt und befindet sich auf die Links, zentriert oder rechts neben der `StackLayout`. Die `Fill` Option bewirkt, dass das untergeordnete Element horizontal eingeschränkt wird, und füllt die Breite der `StackLayout`.

Bei einem vertikalen `StackLayout`, jedes untergeordnete Element wird vertikal uneingeschränkte und ruft eine vertikale slot je nach Höhe des untergeordneten Standorts in diesem Fall die `VerticalOptions` Einstellung ist nicht relevant.

Wenn die vertikale `StackLayout` selbst ist und dass nicht eingeschränkte&mdash;, wenn seine `VerticalOptions` Einstellung ist `Start`, `Center`, oder `End`, klicken Sie dann auf die Höhe des der `StackLayout` wird die gesamte Höhe der untergeordneten.

Jedoch wenn die vertikale `StackLayout` wird vertikal eingeschränkt&mdash;wenn seine `VerticalOptions` Einstellung ist `Fill` &mdash;klicken Sie dann auf die Höhe des der `StackLayout` werden die Höhe des Containers, die möglicherweise größer als die Summe die Höhe der untergeordneten Elemente. Wenn dies der Fall ist, und wenn mindestens ein untergeordnetes Element enthält eine `VerticalOptions` festlegen mit eine `Expands` flag des `true`, zusätzliche Leerzeichen in der `StackLayout` wird gleichmäßig zwischen diesen untergeordneten Elementen mit eine `Expands` flag des `true`. Die gesamte Höhe der untergeordneten Elemente ist dann ist die Höhe des der `StackLayout`, und die `Alignment` Teil der `VerticalOptions` Einstellung bestimmt, wie das untergeordnete Element in einem Steckplatz vertikal positioniert wird.

Dies wird dargestellt, der [ **VerticalOptionsDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/VerticalOptionsDemo) Beispiel.

## <a name="frame-and-boxview"></a>Rahmen und BoxView

Die beiden rechteckigen Ansichten werden häufig für die Darstellung verwendet.

Die [ `Frame` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/) zeigt einen rechteckigen Rahmen um eine andere Ansicht ein Layout, z. B. aufweisen kann `StackLayout`. `Frame` erbt einen [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) Eigenschaft von [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) , mit denen sich die Ansicht innerhalb angezeigt werden soll die `Frame`. Die `Frame` ist standardmäßig transparent. Legen Sie die folgenden drei Eigenschaften zum Anpassen der Darstellung des Frames an:

- Die [ `OutlineColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.OutlineColor/) Eigenschaft um sichtbar zu machen. Es ist üblich, legen Sie `OutlineColor` zu `Color.Accent` nicht das zugrunde liegende Farbschema kennen.
- Die [ `HasShadow` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.HasShadow/) Eigenschaft kann festgelegt werden, um `true` einen schwarzen Schatten auf iOS-Geräte angezeigt werden sollen.
- Legen Sie die [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) Eigenschaft, um eine `Thickness` Wert, der ein Leerzeichen zwischen der und dem Frame, den Inhalt. Der Standardwert ist 20 Einheiten auf allen Seiten.

Die `Frame` hat Standardwert `HorizontalOptions` und `VerticalOptions` Werte `LayoutOptions.Fill`, was bedeutet, dass die `Frame` werden ausgefüllt, den Container. Mit anderen Einstellungen, die Größe der `Frame` basiert auf die Größe seines Inhalts.

Die `Frame` wird veranschaulicht, der [ **FramedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FramedText) Beispiel.

Die [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) zeigt als rechteckige Fläche Farbe gemäß seiner [ `Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) Eigenschaft.

Wenn die `BoxView` beschränkt (seine `HorizontalOptions` und `VerticalOptions` Eigenschaften verfügen über die Standardeinstellungen des `LayoutOptions.Fill`), die `BoxView` füllt den verfügbaren Speicherplatz für sie. Wenn die `BoxView` ist, dass nicht eingeschränkte (mit `HorizontalOptions` und `LayoutOptions` Einstellungen der `Start`, `Center`, oder `End`), wird die Standarddimension 40 Einheiten Quadrat. Ein `BoxView` eingeschränkt, die in einer Dimension und in den anderen uneingeschränkten werden können.

Legen Sie häufig, die die [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) und [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) Eigenschaften des `BoxView` um ihm eine bestimmte Größe zu erteilen. Dies wird anhand der [ **SizedBoxView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/SizedBoxView) Beispiel.

Können Sie mehrere Instanzen von `StackLayout` kombiniert eine `BoxView` und mehrere `Label` -Instanzen in eine `Frame` eine bestimmte Farbe angezeigt, und klicken Sie dann jede dieser Ansichten in einer `StackLayout` in eine `ScrollView` die Attraktivität erstellen Liste der Farben der [ **ColorBlocks** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorBlocks) Beispiel:

[![Dreifacher Screenshot der Farbe Blöcke](images/ch04fg11-small.png "Liste von Farben")](images/ch04fg11-large.png#lightbox "Liste von Farben")

## <a name="a-scrollview-in-a-stacklayout"></a>Eine ScrollView in einem StackLayout?

Einfügen einer `StackLayout` in eine `ScrollView` ist häufig, jedoch Ablegen einer `ScrollView` in eine `StackLayout` eignet sich auch in einigen Fällen. Theoretisch, dies sollte nicht möglich, da die untergeordneten Elemente eines vertikalen `StackLayout` werden vertikal uneingeschränkte. Aber ein `ScrollView` vertikal eingeschränkt werden muss. Es muss eine bestimmte Höhe zugewiesen werden, damit sie dann die Größe des untergeordneten für das Durchführen eines Bildlaufs ermitteln kann.

Der Trick besteht darin, ermöglichen die `ScrollView` untergeordnetes Element von der `StackLayout` eine `VerticalOptions` Einstellung des `FillAndExpand`. Dies wird dargestellt, der [ **BlackCat** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCat) Beispiel.

Die **BlackCat** veranschaulicht das Definieren und Zugriff auf die Anwendung Ressourcen, die in der portablen Klassenbibliothek (PCL) eingebettet sind. Dies kann auch mit freigegebenen Asset-Projekten (SAPs) erreicht werden, aber der Vorgang ist etwas komplizierter als das [ **BlackCatSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCatSap) demonstriert.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 4 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch04-Apr2016.pdf)
- [Kapitel 4-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)
- [Kapitel 4 f#-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FS)
- [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)
- [ScrollView](~/xamarin-forms/user-interface/layouts/scroll-view.md)
