---
title: ''
description: ''
Creating Mobile Apps with Xamarin.Forms: Summary of Chapter 14. Absolute layout''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 72ee9c4a481388e69aeeb52dbd5b8eeaabb164f6
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136759"
---
# <a name="summary-of-chapter-14-absolute-layout"></a>Zusammenfassung von Kapitel 14. Absolutes Layout

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14)

Wie `StackLayout` wird [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) von `Layout<View>` abgeleitet und erbt eine `Children`-Eigenschaft. `AbsoluteLayout` implementiert ein Layoutsystem, das erfordert, dass der Programmierer die Positionen seiner untergeordneten Elemente sowie optional deren Größe angibt. Die Position wird über die obere linke Ecke des untergeordneten Elements relativ zur oberen linken Ecke des `AbsoluteLayout` in geräteunabhängigen Einheiten angegeben. `AbsoluteLayout` implementiert außerdem eine Funktion für proportionale Positionierung und Größenanpassung.

`AbsoluteLayout` sollte als Layoutsystem für spezielle Zwecke betrachtet werden, das nur verwendet werden sollte, wenn der Programmierer den untergeordneten Elementen (z. B. `BoxView`-Elementen) eine Größe vorgeben kann, oder wenn die Größe des Elements sich nicht auf die Positionierung anderer untergeordneter Elemente auswirkt. Die Eigenschaften `HorizontalOptions` und `VerticalOptions` haben keinen Effekt auf untergeordnete Elemente eines `AbsoluteLayout`.

In diesem Kapitel wird außerdem die wichtige Funktion der *angefügten bindbaren Eigenschaften* eingeführt, die es ermöglichen, dass in einer Klasse definierte Eigenschaften (in diesem Fall `AbsoluteLayout`) einer anderen Klasse (einem untergeordneten Element von `AbsoluteLayout`) angefügt werden können.

## <a name="absolutelayout-in-code"></a>„AbsoluteLayout“ in Code

Sie können der `Children`-Sammlung eines `AbsoluteLayout` mithilfe der [`Add`](xref:System.Collections.Generic.ICollection`1.Add*)-Standardmethode ein untergeordnetes Element hinzufügen, doch `AbsoluteLayout` bietet auch eine erweiterte [`Add`](xref:Xamarin.Forms.AbsoluteLayout.IAbsoluteList`1.Add*)-Methode, mit der Sie ein [`Rectangle`](xref:Xamarin.Forms.Rectangle) angeben können. Eine andere [`Add`](xref:Xamarin.Forms.AbsoluteLayout.IAbsoluteList`1.Add*)-Methode erfordert nur einen [`Point`](xref:Xamarin.Forms.Point), wobei dann das untergeordnete Element uneingeschränkt ist und seine Größe selber anpasst.

Sie können einen `Rectangle`-Wert mit einem [Konstruktor](xref:Xamarin.Forms.Rectangle.%23ctor(System.Double,System.Double,System.Double,System.Double)) erstellen, der vier Werte erfordert, wobei die ersten beiden die Position der oberen linken Ecke des untergeordneten Elements relativ zu seinem übergeordneten Element anzeigen und die andern zwei die Größe des untergeordneten Elements. Alternativ können Sie auch einen [Konstruktor](xref:Xamarin.Forms.Rectangle.%23ctor(Xamarin.Forms.Point,Xamarin.Forms.Size)) verwenden, für den ein Wert für `Point` und für [`Size`](xref:Xamarin.Forms.Size) benötigt wird.

Diese `Add`-Methoden werden im [**AbsoluteDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteDemo)-Beispiel veranschaulicht, in dem `BoxView`-Elemente mithilfe von `Rectangle`-Werten und ein `Label`-Element mithilfe nur eines `Point`-Werts positioniert werden.

Im [**ChessboardFixed**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardFixed)-Beispiel werden 32 `BoxView`-Elemente verwendet, um das Schachbrettmuster zu erstellen. Das Programm gibt den `BoxView`-Elementen eine hartcodierte Größe von 35 Einheiten im Quadrat. Für das `AbsoluteLayout` sind seine `HorizontalOptions` und `VerticalOptions` auf `LayoutOptions.Center` festgelegt, wodurch das `AbsoluteLayout` eine Gesamtgröße von 280 Einheiten im Quadrat erhält.

## <a name="attached-bindable-properties"></a>Angefügte bindbare Eigenschaften

Es ist auch möglich, die Position und optional die Größe eines untergeordneten Elements von `AbsoluteLayout` festzulegen, nachdem es der `Children`-Auflistung hinzugefügt wurde. Hierfür wird die statische Methode [`AbsoluteLayout.SetLayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.SetLayoutBounds(Xamarin.Forms.BindableObject,Xamarin.Forms.Rectangle)) verwendet. Das erste Argument ist das untergeordnete Element, das zweite ist ein `Rectangle`-Objekt. Sie können angeben, dass das untergeordnete Element seine Größe horizontal und/oder vertikal selbst anpasst, indem Sie die Breiten- und Höhenwerte auf die Konstante [`AbsoluteLayout.AutoSize`](xref:Xamarin.Forms.AbsoluteLayout.AutoSize) festlegen.

Im [**ChessboardDynamic**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardDynamic)-Beispiel wird das `AbsoluteLayout` in einer `ContentView` mit einem `SizeChanged`-Handler zum Aufrufen von `AbsoluteLayout.SetLayoutBounds` für alle untergeordneten Elemente positioniert, um sie so groß wie möglich zu machen.  

Die angefügte bindbare Eigenschaft, die von `AbsoluteLayout` definiert wird, ist das statische, schreibgeschützte Feld vom Typ `BindableProperty` namens [`AbsoluteLayout.LayoutBoundsProperty`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty). Die statische `AbsoluteLayout.SetLayoutBounds`-Methode wird durch Aufrufen von `SetValue` für das untergeordnete Element mit der `AbsoluteLayout.LayoutBoundsProperty` implementiert. Das untergeordnete Element enthält ein Verzeichnis, in dem die angefügte bindbare Eigenschaft mit ihrem Wert gespeichert ist. Beim Erstellen des Layouts kann mit `AbsoluteLayout` der Wert abgerufen werden, indem [`AbsoluteLayout.GetLayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.GetLayoutBounds(Xamarin.Forms.BindableObject)) aufgerufen wird. Dies wird per `GetValue`-Aufruf implementiert.

## <a name="proportional-sizing-and-positioning"></a>Proportionale Anpassung der Größe und Positionierung

`AbsoluteLayout` implementiert außerdem eine Funktion für proportionale Größenanpassung und Positionierung. Die Klasse definiert eine zweite angefügte bindbare Eigenschaft [`LayoutFlagsProperty`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty) mit den zugehörigen statischen Methoden [`AbsoluteLayout.SetLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.SetLayoutFlags(Xamarin.Forms.BindableObject,Xamarin.Forms.AbsoluteLayoutFlags)) und [`AbsoluteLayout.GetLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.GetLayoutFlags(Xamarin.Forms.BindableObject)).

Das Argument für `AbsoluteLayout.SetLayoutFlags` und der Rückgabewert von `AbsoluteLayout.GetLayoutFlags` sind Werte vom Typ [`AbsoluteLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayoutFlags), eine Enumeration mit den folgenden Membern:

- [`None`](xref:Xamarin.Forms.AbsoluteLayoutFlags.None) (gleich 0)
- [`XProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.XProportional) (1)
- [`YProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.YProportional) (2)
- [`PositionProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.PositionProportional) (3)
- [`WidthProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.WidthProportional) (4)
- [`HeightProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.HeightProportional) (8)
- [`SizeProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.SizeProportional) (12)
- [`All`](xref:Xamarin.Forms.AbsoluteLayoutFlags.All) (\xFFFFFFFF)

Sie können diese mit dem bitweisen OR-Operator von C# kombinieren.

Wenn diese Flags festgelegt sind, werden bestimmte Eigenschaften der `Rectangle`-Layoutgrenzenstruktur, die zur Positionierung und Größenanpassung des untergeordneten Elements verwendet wird, proportional interpretiert.

Wenn das `WidthProportional`-Flag festgelegt ist, bedeutet ein `Width`-Wert von 1, dass das untergeordnete Element dieselbe Breite wie das `AbsoluteLayout` hat. Derselbe Ansatz wird für die Höhe verwendet.

Die proportionale Positionierung berücksichtigt die Größe. Wenn das `XProportional`-Flag festgelegt ist, ist die `X`-Eigenschaft der `Rectangle`-Layoutgrenzen proportional. Der Wert 0 bedeutet, dass der linke Rand des untergeordneten Elements am linken Rand des `AbsoluteLayout` positioniert wird, während eine Position von 1 bedeutet, dass der rechte Rand des untergeordneten Elements am rechten Rand des `AbsoluteLayout` positioniert wird, nicht über den rechten Rand des `AbsoluteLayout` hinaus, wie Sie es möglicherweise erwarten würden. Eine `X`-Eigenschaft von 0,5 zentriert das untergeordnete Element horizontal im `AbsoluteLayout`.

Das [**ChessboardProportional**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardProportional)-Beispiel veranschaulicht die Verwendung der proportionalen Größenanpassung und Positionierung.

## <a name="working-with-proportional-coordinates"></a>Arbeiten mit proportionalen Koordinaten

Manchmal ist es einfacher, sich die proportionale Positionierung anders vorzustellen, als sie im `AbsoluteLayout` implementiert ist. Möglicherweise bevorzugen Sie die Arbeit mit proportionalen Koordinaten, bei denen eine `X`-Eigenschaft von 1 den linken Rand des untergeordneten Elements (anstelle des rechten Rands) am rechten Rand des `AbsoluteLayout` positioniert.

Dieses alternative Positionierungsschema kann als „Bruchteilskoordinaten untergeordneter Elemente“ bezeichnet werden. Mithilfe der folgenden Formeln können Sie Bruchteilskoordinaten untergeordneter Elemente in die für das `AbsoluteLayout` erforderlichen Layoutgrenzen konvertieren:

layoutBounds.X = (fractionalChildCoordinate.X / (1 - layoutBounds.Width))

layoutBounds.Y = (fractionalChildCoordinate.Y / (1 - layoutBounds.Height))

Dies wird im [**ProportionalCoordinateCalc**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/PropCoordCalc)-Beispiel veranschaulicht.

## <a name="absolutelayout-and-xaml"></a>„AbsoluteLayout“ und XAML

Sie können ein `AbsoluteLayout` in XAML verwenden und die angefügten bindbaren Eigenschaften für die untergeordneten Elemente eines `AbsoluteLayout` mithilfe der Attributwerte von `AbsoluteLayout.LayoutBounds` und `AbsoluteLayout.LayoutFlags` festlegen. Dies wird in den Beispielen [**AbsoluteXamlDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteXamlDemo) und [**ChessboardXaml**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardXaml) veranschaulicht. Das letztgenannte Programm enthält 32 `BoxView` Elemente, verwendet aber einen impliziten `Style`, der die `AbsoluteLayout.LayoutFlags`-Eigenschaft enthält, um das Markup auf ein Mindestmaß zu beschränken.

Ein Attribut in XAML, das aus einem Klassennamen, einem Punkt und einem Eigenschaftsnamen besteht, ist *immer* eine angefügte bindbare Eigenschaft.

## <a name="overlays"></a>Überlagerungen

Sie können `AbsoluteLayout` verwenden, um eine *Überlagerung* zu erstellen, die die Seite mit anderen Steuerelementen bedeckt, vielleicht um den Benutzer vor der Interaktion mit den normalen Steuerelementen auf der Seite zu schützen.

Das [**SimpleOverlay**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/SimpleOverlay)-Beispiel veranschaulicht diese Methode und veranschaulicht ferner die [`ProgressBar`](xref:Xamarin.Forms.ProgressBar), in der das Ausmaß angezeigt wird, in dem ein Programm eine Aufgabe erledigt hat.

## <a name="some-fun"></a>Etwas Spaßiges

Das [**DotMatrixClock**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/DotMatrixClock)-Beispiel zeigt die aktuelle Uhrzeit mit einer simulierten 5x7-Punktmatrixanzeige an. Jeder Punkt ist eine `BoxView` (davon gibt es 228) und wird auf dem `AbsoluteLayout` positioniert.

[![Dreifacher Screenshot der Punktmatrixuhr](images/ch14fg08-small.png "Punktmatrixuhr")](images/ch14fg08-large.png#lightbox "Punktmatrixuhr")

Das [**BouncingText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/BouncingText)-Programm animiert zwei `Label`-Objekte, sodass sie horizontal und vertikal über den Bildschirm springen.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 14 – vollständiger Text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch14-Apr2016.pdf)
- [Kapitel 14 – Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14)
- [AbsoluteLayout](~/xamarin-forms/user-interface/layouts/absolute-layout.md)
- [Angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md)
