---
title: Zusammenfassung der Kapitel 14. Absolute layout
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 88882A48-3226-42D1-96ED-241250B64A84
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 394e1722c79bac5f034e9ad88eb1fed7e5090f8c
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-14-absolute-layout"></a>Zusammenfassung der Kapitel 14. Absolute layout

Wie `StackLayout`, [ `AbsoluteLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/) leitet sich von `Layout<View>` und erbt einen `Children` Eigenschaft. `AbsoluteLayout` implementiert ein Layoutsystem, das den Programmierer, die die Positionen der untergeordneten Elemente und optional ihre Größe angeben muss. Die Position wird angegeben, von der linken oberen Ecke des untergeordneten Elements relativ zur linken oberen Ecke von der `AbsoluteLayout` in geräteunabhängigen Einheiten. `AbsoluteLayout` implementiert außerdem eine proportionale positionieren und Sizing-Funktion.

`AbsoluteLayout` muss als eine spezielle Layoutsystem verwendet werden, nur, wenn der Programmierer eine Größe in den untergeordneten Elementen vorgeben kann betrachtet werden (z. B. `BoxView` Elemente) oder wenn die Größe des Elements wirkt sich nicht aus der Positionierung von weiteren untergeordneten Elementen. Die `HorizontalOptions` und `VerticalOptions` Eigenschaften haben keine Auswirkung auf untergeordnete Elemente des ein `AbsoluteLayout`.

In diesem Kapitel führt auch eine wichtige Funktion von *angefügte bindbare Eigenschaften* , mit denen in einer Klasse definierte Eigenschaften (in diesem Fall `AbsoluteLayout`) mit einer anderen Klasse angefügt werden (ein untergeordnetes Element eines der `AbsoluteLayout`).

## <a name="absolutelayout-in-code"></a>AbsoluteLayout in code

Sie können ein untergeordnetes Element hinzufügen der `Children` Auflistung von ein `AbsoluteLayout` mit den standardmäßigen [ `Add` ](https://developer.xamarin.com/api/member/System.Collections.Generic.ICollection%3CT%3E.Add/p/T/) -Methode, aber `AbsoluteLayout` bietet auch eine erweiterte [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout+IAbsoluteList%3CT%3E.Add/p/Xamarin.Forms.View/Xamarin.Forms.Rectangle/Xamarin.Forms.AbsoluteLayoutFlags/) Methode, die Sie angeben kann eine [ `Rectangle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Rectangle/). Eine andere [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout+IAbsoluteList%3CT%3E.Add/p/Xamarin.Forms.View/Xamarin.Forms.Point/) Methode erfordert nur eine [ `Point` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Point/), in diesem Fall das untergeordnete Element ist, dass nicht eingeschränkte und -Größen von selbst.

Können Sie erstellen eine `Rectangle` Wert mit einer [Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Rectangle.Rectangle/p/System.Double/System.Double/System.Double/System.Double/) erfordert, dass vier Werte & #x 2014; die ersten beiden, der angibt, der Position der linken oberen Ecke des untergeordneten Elements relativ zu seinem übergeordneten und die zweiten zwei, der angibt, die die Größe des untergeordneten Elements. Oder Sie können eine [Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Rectangle.Rectangle/p/Xamarin.Forms.Point/Xamarin.Forms.Size/) erfordert, dass eine `Point` und ein [ `Size` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Size/) Wert.

Diese `Add` Methoden werden im veranschaulicht [ **AbsoluteDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteDemo), welche Positionen `BoxView` Elemente mit `Rectangle` Werte, und ein `Label` Element mit nur einem `Point` Wert.

Die [ **ChessboardFixed** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardFixed) -Beispiel verwendet 32 `BoxView` Elemente, die das Muster Schachbrett zu erstellen. Das Programm bietet den `BoxView` Größe der Elemente, die einen hartcodierten 35 Einheiten Quadrats. Die `AbsoluteLayout` hat seine `HorizontalOptions` und `VerticalOptions` festgelegt `LayoutOptions.Center`, wodurch die `AbsoluteLayout` eine Gesamtgröße von 280 Einheiten Quadrat haben.

## <a name="attached-bindable-properties"></a>Angefügte bindbare Eigenschaften

Es ist auch möglich, legen Sie die Position und optional die Größe eines untergeordneten Elements des ein `AbsoluteLayout` , nachdem er hinzugefügt wurde die `Children` Auflistung, die mithilfe der statischen Methode [ `AbsoluteLayout.SetLayoutBounds` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.SetLayoutBounds/p/Xamarin.Forms.BindableObject/Xamarin.Forms.Rectangle/). Das erste Argument ist das untergeordnete Element. Das zweite ist ein `Rectangle` Objekt. Sie können angeben, dass das untergeordnete Element selbst horizontal und/oder vertikal Größen durch Festlegen der Breite und Höhe Werte auf die [ `AbsoluteLayout.AutoSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.AbsoluteLayout.AutoSize/) konstant.

Die [ **ChessboardDynamic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardDynamic) Beispiel setzt die `AbsoluteLayout` in eine `ContentView` mit einer `SizeChanged` Handler Aufrufen `AbsoluteLayout.SetLayoutBounds` auf allen untergeordneten Standorten, um sie so groß wie möglich zu machen.  

Die angefügte bindbare Eigenschaft, die `AbsoluteLayout` definiert ist statischen schreibgeschützten Felds des Typs `BindableProperty` mit dem Namen [ `AbsoluteLayout.LayoutBoundsProperty` ](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty/). Die statische `AbsoluteLayout.SetLayoutBounds` Methode wird durch den Aufruf implementiert `SetValue` auf dem untergeordneten Element mit dem `AbsoluteLayout.LayoutBoundsProperty`. Das untergeordnete Element enthält ein Wörterbuch, in dem die angefügte bindbare Eigenschaft und ihren Wert gespeichert werden. Während des Layouts das `AbsoluteLayout` erhalten diesen Wert durch den Aufruf [ `AbsoluteLayout.GetLayoutBounds` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.GetLayoutBounds/p/Xamarin.Forms.BindableObject/), die mit implementiert eine `GetValue` aufrufen.

## <a name="proportional-sizing-and-positioning"></a>Proportional Größe und Positionierung

`AbsoluteLayout` implementiert eine proportionale Größe und Positionierung der Funktion. Die Klasse definiert eine zweite bindbare Eigenschaft [ `LayoutFlagsProperty` ](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty/), mit den zugehörigen statischen Methoden [ `AbsoluteLayout.SetLayoutFlags` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.SetLayoutFlags/p/Xamarin.Forms.BindableObject/Xamarin.Forms.AbsoluteLayoutFlags/) und [ `AbsoluteLayout.GetLayoutFlags` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.GetLayoutFlags/p/Xamarin.Forms.BindableObject/).

Das Argument für `AbsoluteLayout.SetLayoutFlags` und der Rückgabewert der `AbsoluteLayout.GetLayoutFlags` ist ein Wert vom Typ [ `AbsoluteLayoutFlags` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayoutFlags/), eine Enumeration mit den folgenden Elementen:

- [`None`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.None/) (ungleich 0)
- [`XProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.XProportional/) (1)
- [`YProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.YProportional/) (2)
- [`PositionProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.PositionProportional/) (3)
- [`WidthProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.WidthProportional/) (4)
- [`HeightProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.HeightProportional/) (8)
- [`SizeProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.SizeProportional/) (12)
- [`All`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.All/) (\xFFFFFFFF)

Sie können diese mit den C#-bitweise OR-Operator kombinieren.

Diese Flags festgelegt ist, bestimmte Eigenschaften der `Rectangle` Layoutstruktur Grenzen verwendet, um die position und Größe der untergeordneten proportional interpretiert werden.

Wenn die `WidthProportional` Flag festgelegt ist, eine `Width` Wert von 1 bedeutet, dass das untergeordnete Element die gleiche Breite wie die `AbsoluteLayout`. Ein ähnlichen Ansatz wird für die Höhe verwendet.

Die proportional Positionierung berücksichtigt die Größe. Wenn die `XProportional` Flag festgelegt ist, die `X` Eigenschaft von der `Rectangle` Layout Grenzen ist proportional. Der Wert 0 bedeutet, dass das untergeordnete Element Rand linke am linken Rand positioniert ist der `AbsoluteLayout`, jedoch eine Position 1 bedeutet, dass die Rechte Kante des untergeordneten Standorts am rechten Rand des positioniert ist die `AbsoluteLayout`, nicht hinter dem rechten Rand des der `AbsoluteLayout` wie Expec t. Ein `X` Eigenschaft von 0,5 Zentriert das untergeordnete Element horizontal in der `AbsoluteLayout`.

Die [ **ChessboardProportional** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardProportional) Beispiel veranschaulicht die Verwendung von proportional Größe und Positionierung.

## <a name="working-with-proportional-coordinates"></a>Arbeiten mit proportional Koordinaten

Mitunter ist es einfacher, vorstellen proportional Positionierung anders als die Implementierung in der `AbsoluteLayout`. Möglicherweise bevorzugen Sie arbeiten mit proportional Koordinaten, in denen ein `X` Eigenschaft 1 Positionen linken Rand des untergeordneten Standortes (anstatt den rechten Rand) für den rechten Rand der `AbsoluteLayout`.

Diese alternative Positionierung Schema kann "Bruchteilen untergeordneten Coordinates." aufgerufen werden Sie können in der Layout-Grenzen erforderlich für Sekundenbruchteile untergeordneten Koordinaten konvertieren `AbsoluteLayout` mithilfe der folgenden Formeln:

layoutBounds.X = (fractionalChildCoordinate.X / (1 - layoutBounds.Width))

layoutBounds.Y = (fractionalChildCoordinate.Y / (1 - layoutBounds.Height))

Die [ **ProportionalCoordinateCalc** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/PropCoordCalc) Beispiel veranschaulicht dies.

## <a name="absolutelayout-and-xaml"></a>AbsoluteLayout und XAML

Können Sie eine `AbsoluteLayout` in XAML und legen Sie die angefügten bindbaren Eigenschaften in den untergeordneten Elementen von einer `AbsoluteLayout` Attributwerte mit `AbsoluteLayout.LayoutBounds` und `AbsoluteLayout.LayoutFlags`. Dies wird dargestellt, der [ **AbsoluteXamlDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteXamlDemo) und [ **ChessboardXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardXaml) Beispiele. Das zweite Programm enthält 32 `BoxView` Elemente verwendet jedoch einen impliziten `Style` , umfasst die `AbsoluteLayout.LayoutFlags` Eigenschaft, um das Markup auf ein Minimum zu halten.

Ein Attribut in XAML, der einen Klassennamen, einem Punkt und einen Eigenschaftsnamen besteht, ist *immer* einer angefügten bindbare Eigenschaft.

## <a name="overlays"></a>Überlagert

Können Sie `AbsoluteLayout` zum Erstellen einer *Overlay*, der kompletten der Seite mit anderen Steuerelementen vielleicht um zu verhindern, dass den Benutzer die normale Steuerelemente auf der Seite interagieren. 

Die [ **SimpleOverlay** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/SimpleOverlay) Beispiel veranschaulicht dieses Verfahren, und auch die [ `ProgressBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/), die anzeigt, dass der Block, der ein Programm wurde, ein Aufgabe.

## <a name="some-fun"></a>Einige fun

Die [ **DotMatrixClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/DotMatrixClock) Beispiel zeigt die aktuelle Zeit mit einer simulierten 5 x 7 Punkt Matrix anzeigen. Jeder Punkt ist eine `BoxView` (es gibt davon 228) angepasst und positioniert die `AbsoluteLayout`.

[![Dreifacher Screenshot Punkt Matrix Uhr](images/ch14fg08-small.png "Punktmatrix Uhr")](images/ch14fg08-large.png#lightbox "Punktmatrix Uhr")

Die [ **BouncingText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/BouncingText) Programm erstellt eine Animation zwei `Label` Objekte horizontal und vertikal über den Bildschirm zu springen.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 14 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch14-Apr2016.pdf)
- [Kapitel 14-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14)
- [AbsoluteLayout](~/xamarin-forms/user-interface/layouts/absolute-layout.md)
