---
title: Zusammenfassung der Kapitel 14. Absolutes layout
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 14. Absolutes layout'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 88882A48-3226-42D1-96ED-241250B64A84
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 7371f134944d7492e51aa2d02247c0ab48345a47
ms.sourcegitcommit: 495680e74c72e7c570e68cde95d3d3643b1fcc8a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2019
ms.locfileid: "58870234"
---
# <a name="summary-of-chapter-14-absolute-layout"></a>Zusammenfassung der Kapitel 14. Absolutes layout

[![Downloadliste Beispiel](~/media/shared/download.png) Herunterladen des Beispiels](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14)

Wie `StackLayout`, [ `AbsoluteLayout` ](xref:Xamarin.Forms.AbsoluteLayout) leitet sich von `Layout<View>` und erbt einen `Children` Eigenschaft. `AbsoluteLayout` implementiert eine Layoutsystem, das den Programmierer, geben Sie die Positionen der untergeordneten Elemente und optional ihre Größe erfordert. Die Position wird angegeben, indem die linke obere Ecke des untergeordneten Elements relativ zur oberen linken Ecke des der `AbsoluteLayout` in geräteunabhängigen Einheiten. `AbsoluteLayout` implementiert außerdem eine proportionale Positionierung und größenanpassung-Funktion aus.

`AbsoluteLayout` muss als ein spezielles Layoutsystem verwendet werden, nur dann, wenn der Programmierer eine Größe für die untergeordneten Elemente darstellen, kann Sie betrachtet werden (z. B. `BoxView` Elemente) oder wenn die Größe des Elements hat keine Auswirkungen auf die Positionierung des anderen untergeordneten Knoten. Die `HorizontalOptions` und `VerticalOptions` Eigenschaften wirken sich nicht auf die untergeordneten Elemente ein `AbsoluteLayout`.

In diesem Kapitel führt auch die wichtige Funktion von *angefügten bindbare Eigenschaften* können Sie Eigenschaften in einer Klasse definiert (in diesem Fall `AbsoluteLayout`) zu einer anderen Klasse angefügt werden (ein untergeordnetes Element des der `AbsoluteLayout`).

## <a name="absolutelayout-in-code"></a>Von "AbsoluteLayout" im code

Können Sie ein untergeordnetes Element zum Hinzufügen der `Children` Auflistung von eine `AbsoluteLayout` mit dem Standard [ `Add` ](xref:System.Collections.Generic.ICollection`1.Add*) -Methode, aber `AbsoluteLayout` bietet auch eine erweiterte [ `Add` ](xref:Xamarin.Forms.AbsoluteLayout.IAbsoluteList`1.Add*) Methode, die Sie angeben kann eine [ `Rectangle` ](xref:Xamarin.Forms.Rectangle). Eine andere [ `Add` ](xref:Xamarin.Forms.AbsoluteLayout.IAbsoluteList`1.Add*) Methode erfordert nur eine [ `Point` ](xref:Xamarin.Forms.Point), in diesem Fall das untergeordnete Element ist, uneingeschränkte und passt sich.

Können Sie erstellen eine `Rectangle` Wert mit einer [Konstruktor](xref:Xamarin.Forms.Rectangle.%23ctor(System.Double,System.Double,System.Double,System.Double)) , erfordert vier Werte &mdash; die ersten beiden, der angibt, der der Position der linken oberen Ecke des untergeordneten Elements relativ zum übergeordneten Element und die zweiten beiden, der angibt, die die Größe des untergeordneten Elements. Oder Sie können eine [Konstruktor](xref:Xamarin.Forms.Rectangle.%23ctor(Xamarin.Forms.Point,Xamarin.Forms.Size)) erfordert, dass eine `Point` und [ `Size` ](xref:Xamarin.Forms.Size) Wert.

Diese `Add` Methoden werden veranschaulicht [ **AbsoluteDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteDemo), welche Positionen `BoxView` Elemente mit `Rectangle` Werte, und ein `Label` Element mit nur einem `Point` Wert.

Die [ **ChessboardFixed** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardFixed) -Beispiel verwendet 32 `BoxView` Elementen, die das Schachbrett-Muster zu erstellen. Das Programm bietet den `BoxView` Elemente, die einen hartcodierten Größe 35 Einheiten Quadrats. Die `AbsoluteLayout` hat seine `HorizontalOptions` und `VerticalOptions` festgelegt `LayoutOptions.Center`, bewirkt, dass die `AbsoluteLayout` um eine Gesamtgröße von 280 Einheiten Quadrat zu erhalten.

## <a name="attached-bindable-properties"></a>Angefügte bindbare Eigenschaften

Es ist auch möglich, legen Sie die Position und optional die Größe eines untergeordneten Elements von einer `AbsoluteLayout` , nachdem er hinzugefügt wurde die `Children` Auflistung, die mithilfe der statischen Methode [ `AbsoluteLayout.SetLayoutBounds` ](xref:Xamarin.Forms.AbsoluteLayout.SetLayoutBounds(Xamarin.Forms.BindableObject,Xamarin.Forms.Rectangle)). Das erste Argument ist das untergeordnete Element. die zweite ist ein `Rectangle` Objekt. Sie können angeben, dass das untergeordnete Element selbst horizontal und/oder vertikal Größe durch Festlegen der Werte für Breite und Höhe auf die [ `AbsoluteLayout.AutoSize` ](xref:Xamarin.Forms.AbsoluteLayout.AutoSize) Konstanten.

Die [ **ChessboardDynamic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardDynamic) Beispiel setzt die `AbsoluteLayout` in eine `ContentView` mit einem `SizeChanged` Handler, der aufgerufen `AbsoluteLayout.SetLayoutBounds` auf allen untergeordneten Standorten, um sie so groß wie möglich zu machen.  

Die angefügte bindbare Eigenschaft, die `AbsoluteLayout` definiert, ist das statische schreibgeschützte Feld des Typs `BindableProperty` mit dem Namen [ `AbsoluteLayout.LayoutBoundsProperty` ](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty). Die statische `AbsoluteLayout.SetLayoutBounds` Methode wird implementiert, durch den Aufruf `SetValue` auf dem untergeordneten Element mit dem `AbsoluteLayout.LayoutBoundsProperty`. Das untergeordnete Element enthält ein Wörterbuch, in dem die angefügte bindbare Eigenschaft und der Wert gespeichert werden. Während des Layouts der `AbsoluteLayout` erhalten diesen Wert durch den Aufruf [ `AbsoluteLayout.GetLayoutBounds` ](xref:Xamarin.Forms.AbsoluteLayout.GetLayoutBounds(Xamarin.Forms.BindableObject)), die implementiert wird, mit einer `GetValue` aufrufen.

## <a name="proportional-sizing-and-positioning"></a>Proportional zur größenanpassung und Positionierung

`AbsoluteLayout` implementiert eine proportional zur größenanpassung und Positionierung von Features. Die Klasse definiert eine zweite angefügten bindbare Eigenschaft [ `LayoutFlagsProperty` ](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty), mit den entsprechenden statischen Methoden [ `AbsoluteLayout.SetLayoutFlags` ](xref:Xamarin.Forms.AbsoluteLayout.SetLayoutFlags(Xamarin.Forms.BindableObject,Xamarin.Forms.AbsoluteLayoutFlags)) und [ `AbsoluteLayout.GetLayoutFlags` ](xref:Xamarin.Forms.AbsoluteLayout.GetLayoutFlags(Xamarin.Forms.BindableObject)).

Das Argument für `AbsoluteLayout.SetLayoutFlags` und der Rückgabewert von `AbsoluteLayout.GetLayoutFlags` ist ein Wert vom Typ [ `AbsoluteLayoutFlags` ](xref:Xamarin.Forms.AbsoluteLayoutFlags), eine Enumeration mit folgenden Membern:

- [`None`](xref:Xamarin.Forms.AbsoluteLayoutFlags.None) (gleich 0)
- [`XProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.XProportional) (1)
- [`YProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.YProportional) (2)
- [`PositionProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.PositionProportional) (3)
- [`WidthProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.WidthProportional) (4)
- [`HeightProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.HeightProportional) (8)
- [`SizeProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.SizeProportional) (12)
- [`All`](xref:Xamarin.Forms.AbsoluteLayoutFlags.All) (\xFFFFFFFF)

Sie können diese mit den C#-bitweise OR-Operator kombinieren.

Diese Flags festgelegt ist, bestimmte Eigenschaften der `Rectangle` Grenzen Layoutstruktur zum Positionieren und ihre Größe das untergeordnete Element proportional interpretiert werden.

Wenn die `WidthProportional` Flag festgelegt ist, eine `Width` Wert 1 bedeutet, dass das untergeordnete Element die gleiche Breite wie die `AbsoluteLayout`. Ein ähnlicher Ansatz wird für die Höhe verwendet.

Die proportionale Positionierung berücksichtigt die Größe. Wenn der `XProportional` Flag festgelegt ist, die `X` Eigenschaft der `Rectangle` Layout Grenzen ist proportional. Der Wert 0 bedeutet, dass das untergeordnete Element linken Rand des befindet sich am linken Rand des der `AbsoluteLayout`, aber eine Position 1 bedeutet, dass am rechten Rand des rechten Rands des untergeordneten Standortes positioniert ist die `AbsoluteLayout`, nicht über dem rechten Rand des der `AbsoluteLayout` Expec z. B. t. Ein `X` Eigenschaft von 0,5 konzentriert sich das untergeordnete Element horizontal in der `AbsoluteLayout`.

Die [ **ChessboardProportional** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardProportional) Beispiel veranschaulicht die Verwendung von proportional zur größenanpassung und Positionierung.

## <a name="working-with-proportional-coordinates"></a>Arbeiten mit proportional Koordinaten

In einigen Fällen ist es einfacher, stellen Sie sich anders als in Implementierung ist proportional Positionierung der `AbsoluteLayout`. Möglicherweise möchten Sie mit proportional Koordinaten arbeiten, in denen ein `X` -Eigenschaft von 1 Positionen linken Rand des untergeordneten Standortes (und nicht mit dem rechten Rand) für den rechten Rand der `AbsoluteLayout`.

Diese alternative positionierungs-Schema kann "Bruchteilen untergeordneten Coordinates." aufgerufen werden Sie können in der Layout-Grenzen erforderlich für Sekundenbruchteile untergeordneten Koordinaten konvertieren `AbsoluteLayout` mithilfe der folgenden Formeln:

layoutBounds.X = (fractionalChildCoordinate.X / (1 - layoutBounds.Width))

layoutBounds.Y = (fractionalChildCoordinate.Y / (1 - layoutBounds.Height))

Die [ **ProportionalCoordinateCalc** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/PropCoordCalc) veranschaulicht dies.

## <a name="absolutelayout-and-xaml"></a>Von "AbsoluteLayout" und XAML

Können Sie eine `AbsoluteLayout` in XAML, und legen Sie in den untergeordneten Elementen der angefügten bindbare Eigenschaften eine `AbsoluteLayout` mit Attributwerte des `AbsoluteLayout.LayoutBounds` und `AbsoluteLayout.LayoutFlags`. Dies wird veranschaulicht, der [ **AbsoluteXamlDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteXamlDemo) und [ **ChessboardXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardXaml) Beispiele. Das zweite Programm enthält 32 `BoxView` Elemente verwendet jedoch eine implizite `Style` , enthält die `AbsoluteLayout.LayoutFlags` Eigenschaft, um das Markup auf ein Minimum zu halten.

Ein Attribut in XAML, der einen Klassennamen, einem Punkt und einem Eigenschaftennamen besteht, ist *immer* angefügte bindbare Eigenschaft.

## <a name="overlays"></a>Überlagerungen

Sie können `AbsoluteLayout` zum Erstellen einer *Overlay*, dort die Seite mit den anderen Steuerelementen, z. B. um zu verhindern, dass den Benutzer die Interaktion mit den normalen Steuerelementen auf der Seite.

Die [ **SimpleOverlay** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/SimpleOverlay) Beispiel veranschaulicht dieses Verfahren, und außerdem wird veranschaulicht, die [ `ProgressBar` ](xref:Xamarin.Forms.ProgressBar), wird angezeigt, die das Ausmaß, ein Programm wurde, ein Aufgabe.

## <a name="some-fun"></a>Etwas Spaß

Die [ **DotMatrixClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/DotMatrixClock) Beispiel wird die aktuelle Uhrzeit mit einem simulierten 5 Stunden, 7 Punktmatrix-Display angezeigt. Jeder Punkt ist eine `BoxView` (es gibt 228 davon) die Größe und positioniert die `AbsoluteLayout`.

[![Dreifacher Screenshot Punktmatrix Uhr](images/ch14fg08-small.png "Punktmatrix Uhr")](images/ch14fg08-large.png#lightbox "Punktmatrix-Uhr")

Die [ **BouncingText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/BouncingText) Programm erstellt eine Animation zwei `Label` Objekte horizontal und vertikal über den Bildschirm zu springen.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 14 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch14-Apr2016.pdf)
- [Kapitel 14-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14)
- [AbsoluteLayout](~/xamarin-forms/user-interface/layouts/absolute-layout.md)
- [Angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md)
