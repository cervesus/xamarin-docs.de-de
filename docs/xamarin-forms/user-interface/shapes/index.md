---
title: Xamarin.FormsFormen
description: Xamarin.FormsFormen sind Typen von Sichten, mit denen Sie Formen auf dem Bildschirm zeichnen können.
ms.prod: xamarin
ms.assetid: 4E749FE8-852C-46DA-BB1E-652936106357
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/22/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 053da71fdd91af91f0a037e7573def91c36df503
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86935511"
---
# <a name="xamarinforms-shapes"></a>Xamarin.FormsFormen

![Vorabversion-API](~/media/shared/preview.png "Diese API ist derzeit als Vorabversion erhältlich.")

Ein `Shape` ist ein Typ von [`View`](xref:Xamarin.Forms.View) , mit dem Sie eine Form auf dem Bildschirm zeichnen können. `Shape`-Objekte können in layoutklassen und den meisten Steuerelementen verwendet werden, da die- `Shape` Klasse von der-Klasse abgeleitet wird `View` .

Xamarin.FormsFormen sind im `Xamarin.Forms.Shapes` -Namespace unter IOS, Android, macOS, den universelle Windows-Plattform (UWP) und Windows Presentation Foundation (WPF) verfügbar.

> [!IMPORTANT]
> Xamarin.FormsFormen sind zurzeit experimentell und können nur verwendet werden, indem das-Flag festgelegt wird `Shapes_Experimental` . Weitere Informationen finden Sie unter [experimentelle Flags](~/xamarin-forms/internals/experimental-flags.md).

`Shape` definiert die folgenden Eigenschaften:

- `Aspect`Gibt an, `Stretch` wie die Form den zugeordneten Bereich füllt. Der Standardwert dieser Eigenschaft ist `Stretch.None`.
- `Fill`Gibt die Farbe an, mit der [`Color`](xref:Xamarin.Forms.Color) das Innere der Form gezeichnet wird.
- `Stroke`[`Color`](xref:Xamarin.Forms.Color)gibt die Farbe an, die zum Zeichnen der Kontur der Form verwendet wird.
- `StrokeDashArray`vom Typ `DoubleCollection` , der eine Auflistung von Werten darstellt, die `double` das Muster von Bindestrichen und Lücken angeben, die zum Gliedern einer Form verwendet werden.
- `StrokeDashOffset``double`gibt den Abstand innerhalb des Strich Musters an, in dem ein Bindestrich beginnt. Der Standardwert dieser Eigenschaft ist 0,0.
- `StrokeLineCap`, vom Typ `PenLineCap` , beschreibt die Form am Anfang und am Ende einer Linie oder eines Segments. Der Standardwert dieser Eigenschaft ist `PenLineCap.Flat`.
- `StrokeLineJoin`Gibt den Typ des Joins an, der in den Scheitel Punkten `PenLineJoin` einer Form verwendet wird. Der Standardwert dieser Eigenschaft ist `PenLineJoin.Miter`.
- `StrokeThickness``double`gibt die Breite der Form Gliederung des Typs an. Der Standardwert dieser Eigenschaft ist 1,0.

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Xamarin.Formsdefiniert eine Reihe von-Objekten, die von der-Klasse abgeleitet werden `Shape` . Dabei handelt es sich um `Ellipse` , `Line` , `Path` , `Polygon` , `Polyline` und `Rectangle` .

## <a name="paint-shapes"></a>Zeichnen von Formen

[`Color`](xref:Xamarin.Forms.Color)-Objekte werden zum Zeichnen der Formen `Stroke` und verwendet `Fill` :

```xaml
<Ellipse Fill="DarkBlue"
         Stroke="Red"
         StrokeThickness="4"
         WidthRequest="150"
         HeightRequest="50"
         HorizontalOptions="Start" />
```

In diesem Beispiel werden der Strich und die Füllung eines `Ellipse` angegeben:

![Zeichnen von Formen](images/ellipse.png "Zeichnen von Formen")

> [!IMPORTANT]
> Wenn Sie keinen [`Color`](xref:Xamarin.Forms.Color) Wert für angeben `Stroke` , oder wenn Sie `StrokeThickness` auf 0 festlegen, wird der Rahmen um die Form nicht gezeichnet.

Weitere Informationen zu gültigen [`Color`](xref:Xamarin.Forms.Color) Werten finden Sie unter [Farben in Xamarin.Forms ](~/xamarin-forms/user-interface/colors.md).

## <a name="stretch-shapes"></a>Stretch-Formen

`Shape`-Objekte verfügen über eine- `Aspect` Eigenschaft vom Typ `Stretch` . Diese Eigenschaft bestimmt, wie `Shape` der Inhalt eines-Objekts gestreckt wird, um den `Shape` Layoutbereich des Objekts zu füllen. Der `Shape` Layoutbereich eines-Objekts ist die Menge des Speicherplatzes `Shape` , der vom Xamarin.Forms Layoutsystem aufgrund einer expliziten `WidthRequest` und- `HeightRequest` Einstellung oder aufgrund der `HorizontalOptions` -und- `VerticalOptions` Einstellungen zugeordnet wird.

Die `Stretch`-Enumeration definiert die folgenden Member:

- `None`Gibt an, dass der Inhalt seine ursprüngliche Größe beibehält. Dies ist der Standardwert der `Shape.Aspect`-Eigenschaft.
- `Fill`Gibt an, dass die Größe des Inhalts angepasst wird, um die Zieldimensionen auszufüllen. Das Seitenverhältnis wird nicht beibehalten.
- `Uniform`Gibt an, dass die Größe des Inhalts angepasst wird, um die Zieldimensionen zu erfüllen, wobei das Seitenverhältnis beibehalten wird.
- `UniformToFill`Gibt an, dass die Größe des Inhalts angepasst wird, um die Zieldimensionen auszufüllen, wobei das Seitenverhältnis beibehalten wird. Falls das Seitenverhältnis des Zielrechtecks von der Quelle abweicht, wird der Quellinhalt entsprechend den Zieldimensionen beschnitten.

Der folgende XAML-Code zeigt, wie die-Eigenschaft festgelegt wird `Aspect` :

```xaml
<Path Aspect="Uniform"
      Stroke="Yellow"
      Fill="Red"
      BackgroundColor="LightGray"
      HorizontalOptions="Start"
      HeightRequest="100"
      WidthRequest="100">
    <Path.Data>
        <!-- Path data goes here -->
    </Path.Data>  
</Path>      
```

In diesem Beispiel zeichnet ein- `Path` Objekt ein Herz. Die `Path` -und-Eigenschaften des-Objekts `WidthRequest` `HeightRequest` werden auf 100 geräteunabhängige Einheiten festgelegt, und die- `Aspect` Eigenschaft ist auf festgelegt `Uniform` . Demzufolge wird der Inhalt des Objekts an die Zieldimensionen angepasst, wobei das Seitenverhältnis beibehalten wird:

![Stretch-Formen](images/aspect.png "Stretch-Formen")

## <a name="draw-dashed-shapes"></a>Gestrichelte Formen zeichnen

`Shape`-Objekte verfügen über eine `StrokeDashArray` Eigenschaft vom Typ `DoubleCollection` . Diese Eigenschaft stellt eine Auflistung von `double` Werten dar, die das Muster von Bindestrichen und Lücken angeben, die zum Gliedern einer Form verwendet werden. Ein `DoubleCollection` ist ein `ObservableCollection` von- `double` Werten. Jede `double` in der Auflistung gibt die Länge eines Bindestrichs oder einer Lücke an. Das erste Element in der Auflistung, das sich bei Index 0 befindet, gibt die Länge eines Bindestrichs an. Das zweite Element in der Auflistung, das sich bei Index 1 befindet, gibt die Länge einer Lücke an. Daher geben Objekte mit einem geraden Indexwert Bindestriche an, während Objekte mit einem ungeraden Indexwert Lücken enthalten.

`Shape`-Objekte verfügen auch `StrokeDashOffset` über eine-Eigenschaft vom Typ `double` , die den Abstand innerhalb des Strich Musters angibt, in dem ein Bindestrich beginnt. Wenn diese Eigenschaft nicht festgelegt wird, führt dies zu `Shape` einer voll soliden Kontur.

Gestrichelte Formen können durch Festlegen der `StrokeDashArray` Eigenschaften und gezeichnet werden `StrokeDashOffset` . Die- `StrokeDashArray` Eigenschaft sollte auf einen oder mehrere Werte festgelegt werden `double` , wobei jedes Paar durch ein einzelnes Komma und/oder ein oder mehrere Leerzeichen getrennt ist. Beispielsweise sind "0,5 1,0" und "0,5, 1.0" beide gültig.

Das folgende XAML-Beispiel zeigt, wie Sie ein gestricheltes Rechteck zeichnen:

```xaml
<Rectangle Fill="DarkBlue"
           Stroke="Red"
           StrokeThickness="4"
           StrokeDashArray="1,1"
           StrokeDashOffset="6"
           WidthRequest="150"
           HeightRequest="50"
           HorizontalOptions="Start" />
```

In diesem Beispiel wird ein ausgefülltes Rechteck mit einem gestrichelten Strich gezeichnet:

![Gestricheltes Rechteck](images/dashed-rectangle.png "Gestrichelte Linie")

## <a name="control-line-ends"></a>Steuerungs Zeilenenden

Eine Linie besteht aus drei Teilen: Start Abdeckung, Zeilen Text und Ende der Obergrenze. Die Start-und ENDCAPS beschreiben die Form am Anfang und Ende einer Linie oder eines Segments.

`Shape`-Objekte verfügen über eine `StrokeLineCap` Eigenschaft vom Typ `PenLineCap` , die die Form am Anfang und Ende einer Linie oder eines Segments beschreibt. Die `PenLineCap`-Enumeration definiert die folgenden Member:

- `Flat`, das eine Obergrenze darstellt, die nicht über den letzten Punkt der Zeile hinausgeht. Dies ist vergleichbar mit keinem Linien Ende und ist der Standardwert der `StrokeLineCap` Eigenschaft.
- `Square`, das ein Rechteck darstellt, dessen Höhe gleich der Linienstärke und eine Länge gleich der halben Linienstärke ist.
- `Round`, der einen Halbkreis darstellt, dessen Durchmesser gleich der Linienstärke ist.

> [!IMPORTANT]
> Die- `StrokeLineCap` Eigenschaft hat keine Auswirkung, wenn Sie Sie für eine Form ohne Start-oder Endpunkte festgelegt haben. Diese Eigenschaft hat z. b. keine Auswirkung, wenn Sie Sie für einen oder einen festgelegt haben `Ellipse` `Rectangle` .

Der folgende XAML-Code zeigt, wie die-Eigenschaft festgelegt wird `StrokeLineCap` :

```xaml
<Line X1="0"
      Y1="20"
      X2="300"
      Y2="20"
      StrokeLineCap="Round"
      Stroke="Red"
      StrokeThickness="12" />
```

In diesem Beispiel wird die rote Linie am Anfang und am Ende der Zeile gerundet:

![Linien Kappen](images/linecap.png "Linien Kappen")

## <a name="control-line-joins"></a>Steuer Zeilen Joins

`Shape`-Objekte verfügen über eine `StrokeLineJoin` Eigenschaft vom Typ `PenLineJoin` , die den Typ des Joins angibt, der an den Scheitel Punkten der Form verwendet wird. Die `PenLineJoin`-Enumeration definiert die folgenden Member:

- `Miter`stellt reguläre Winkel Scheitel Punkte dar. Dies ist der Standardwert der `StrokeLineJoin`-Eigenschaft.
- `Bevel`, das gefragte Vertices darstellt.
- `Round`, das abgerundete Vertices darstellt.

Der folgende XAML-Code zeigt, wie die-Eigenschaft festgelegt wird `StrokeLineJoin` :

```xaml
<Polyline Points="20 20,250 50,20 120"
          Stroke="DarkBlue"
          StrokeThickness="20"
          StrokeLineJoin="Round" />
```

In diesem Beispiel enthält die dunkelblaue Polylinie gerundete Joins an den Scheitel Punkten:

![Linienjoins](images/linejoin.png "Linienjoins")

## <a name="related-links"></a>Verwandte Links

- [Shapedemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Farben inXamarin.Forms](~/xamarin-forms/user-interface/colors.md)
