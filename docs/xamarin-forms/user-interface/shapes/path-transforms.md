---
title: 'Xamarin.FormsFormen: Pfad Transformationen'
description: Eine Xamarin.Forms Transformation definiert, wie ein Pfad Objekt von einem Koordinaten Bereich in einen anderen Koordinaten Bereich transformiert wird.
ms.prod: xamarin
ms.assetid: 07DE3D66-1820-4642-BDDF-84146D40C99D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ab759428c1bc5de8840808443ba40c501fb43b51
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84947297"
---
# <a name="xamarinforms-shapes-path-transforms"></a>Xamarin.FormsFormen: Pfad Transformationen

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

Eine `Transform` definiert, wie ein- `Path` Objekt von einem Koordinaten Bereich in einen anderen Koordinaten Bereich transformiert wird. Diese Zuordnung wird durch eine Transformation beschrieben `Matrix` , bei der es sich um eine Sammlung von drei Zeilen mit drei Spalten von `double` Werten handelt.

Eine 3X3-Matrix wird für Transformationen in einer 2D-x-y-Ebene verwendet. Affine Transformations Matrizen können so multipliziert werden, dass Sie beliebig viele lineare Transformationen bilden, z. b. Drehung und Schiefe, gefolgt von der Übersetzung. Die folgende Tabelle zeigt die Struktur einer Xamarin.Forms Matrix:

| | | |
|---------|---------|-----|
| M11     | M12     | 0,0 |
| M21     | M22     | 0,0 |
| OffsetX | OffsetY | 1.0 |

Durch die Bearbeitung von Matrix Werten können Objekte gedreht, skaliert, verzerrt und übersetzt werden `Path` . Wenn Sie z. b. den `OffsetX` Wert in 100 ändern, können Sie ihn verwenden, um ein `Path` -100 Objekt mit geräteunabhängigen Einheiten auf der x-Achse zu verschieben. Wenn Sie den `M22` Wert in "3" ändern, können Sie ihn verwenden, um ein- `Path` Objekt auf das Dreifache seiner aktuellen Höhe auszudehnen. Wenn Sie beide Werte ändern, verschieben Sie das `Path` Objekt 100 geräteunabhängige Einheiten entlang der x-Achse und Strecken die Höhe um den Faktor 3.

> [!NOTE]
> Die letzte Spalte einer affinen Transformationsmatrix ist gleich (0,0), sodass nur die Elemente in den ersten beiden Spalten angegeben werden müssen. Die Elemente in der letzten Zeile, `OffsetX` und `OffsetY` , stellen Übersetzungs Werte dar.

Obwohl Sie eine `Matrix` Struktur direkt verwenden können, um einzelne Punkte zu übersetzen, Xamarin.Forms bietet auch die folgenden Klassen, die es Ihnen ermöglichen, Objekte zu transformieren, `Path` ohne direkt mit Matrizen zu arbeiten:

- `RotateTransform`, die eine `Path` durch einen angegebenen rotiert `Angle` .
- `ScaleTransform`, wodurch ein `Path` -Objekt durch angegebene `ScaleX` -und-Beträge skaliert wird `ScaleY` .
- `SkewTransform`, das ein `Path` -Objekt durch angegebene `AngleX` -und-Beträge vergrenzt `AngleY` .
- `TranslateTransform`, wodurch ein `Path` -Objekt durch angegebene `X` -und-Beträge verschoben wird `Y` .

Xamarin.Formsbietet außerdem die folgenden Klassen zum Erstellen komplexerer Transformationen:

- `TransformGroup`stellt eine zusammengesetzte dar, die aus `Transform` anderen- `Transform` Objekten besteht.
- `CompositeTransform`stellt eine zusammengesetzte dar, die aus `Transform` anderen- `Transform` Objekten besteht.
- `MatrixTransform`, wodurch benutzerdefinierte Transformationen erstellt werden, die nicht von den anderen Klassen bereitgestellt werden `Transform` .

Alle diese Klassen werden von der- `Transform` Klasse abgeleitet, die eine `Value` Eigenschaft vom Typ definiert `Matrix` . Diese Eigenschaft stellt die aktuelle Transformation als- `Matrix` Objekt dar.

Wenn Sie eine Transformation auf eine anwenden möchten `Path` , erstellen Sie eine Transformations Klasse und legen Sie als Wert der- `Path.RenderTransform` Eigenschaft fest.

## <a name="rotation-transform"></a>Drehungs Transformation

Eine Transformation zum drehen dreht ein- `Path` Objekt im Uhrzeigersinn um einen angegebenen Punkt in einem 2D-x-y-Koordinatensystem.

Die- `RotateTransform` Klasse, die von der- `Transform` Klasse abgeleitet wird, definiert die folgenden Eigenschaften:

- `Angle``double`stellt den Winkel der Drehung im Uhrzeigersinn in Grad dar. Der Standardwert dieser Eigenschaft ist 0,0.
- `CenterX``double`stellt die x-Koordinate des Mittelpunkts der Drehung dar. Der Standardwert dieser Eigenschaft ist 0,0.
- `CenterY``double`stellt die y-Koordinate des Mittelpunkts der Drehung dar. Der Standardwert dieser Eigenschaft ist 0,0.

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Die `CenterX` `CenterY` Eigenschaften und geben den Punkt an, an dem das `Path` Objekt gedreht wird. Dieser Mittelpunkt wird im Koordinaten Bereich des Objekts ausgedrückt, das transformiert wird. Standardmäßig wird die Drehung auf (0,0) angewendet. Dies ist die linke obere Ecke des `Path` Objekts.

Im folgenden Beispiel wird gezeigt, wie Sie ein- `Path` Objekt drehen:

```xaml
<Path Stroke="Black"
      StrokeThickness="1"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <RotateTransform CenterX="0"
                         CenterY="0"
                         Angle="45" />
    </Path.RenderTransform>
</Path>
```

In diesem Beispiel wird das `Path` Objekt um die linke obere Ecke um 45 Grad gedreht.

## <a name="scale-transform"></a>Skalierungs Transformation

Eine Skalierungs Transformation skaliert ein- `Path` Objekt im 2D-x-y-Koordinatensystem.

Die- `ScaleTransform` Klasse, die von der- `Transform` Klasse abgeleitet wird, definiert die folgenden Eigenschaften:

- `ScaleX`vom Typ `double` , der den Skalierungsfaktor für die x-Achse darstellt. Der Standardwert dieser Eigenschaft ist 1,0.
- `ScaleY`vom Typ `double` , der den Skalierungsfaktor für die y-Achse darstellt. Der Standardwert dieser Eigenschaft ist 1,0.
- `CenterX`vom Typ `double` , der die x-Koordinate des Mittelpunkts dieser Transformation darstellt. Der Standardwert dieser Eigenschaft ist 0,0.
- `CenterY`vom Typ `double` , der die y-Koordinate des Mittelpunkts dieser Transformation darstellt. Der Standardwert dieser Eigenschaft ist 0,0.

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Der Wert von `ScaleX` und `ScaleY` hat eine große Auswirkung auf die resultierende Skalierung:

- Werte zwischen 0 und 1 verringern die Breite und Höhe des skalierten Objekts.
- Werte, die größer als 1 sind, erhöhen die Breite und Höhe des skalierten Objekts.
- Werte von 1 geben an, dass das Objekt nicht skaliert wird.
- Negative Werte kippen das Skalierungs Objekt horizontal und vertikal.
- Werte zwischen 0 und-1 kippen das Skalierungs Objekt und verringern dessen Breite und Höhe.
- Werte, die kleiner als-1 sind, kippen das Objekt und vergrößern seine Breite und Höhe.
- Werte von-1 kippen das skalierte Objekt, ändern die horizontale oder vertikale Größe jedoch nicht.

Die `CenterX` `CenterY` Eigenschaften und geben den Punkt an, an dem das `Path` Objekt skaliert wird. Dieser Mittelpunkt wird im Koordinaten Bereich des Objekts ausgedrückt, das transformiert wird. Standardmäßig wird die Skalierung auf (0,0) angewendet. Dies ist die linke obere Ecke des `Path` Objekts. Dies hat den Effekt `Path` , dass das Objekt verschoben und es größer erscheint, denn wenn Sie eine Transformation anwenden, ändern Sie den Koordinaten Bereich, in dem sich das `Path` Objekt befindet.

Im folgenden Beispiel wird gezeigt, wie ein-Objekt skaliert wird `Path` :

```xaml
<Path Stroke="Black"
      StrokeThickness="1"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <ScaleTransform CenterX="0"
                        CenterY="0"
                        ScaleX="2"
                        ScaleY="2" />
    </Path.RenderTransform>
</Path>
```

In diesem Beispiel wird das `Path` Objekt auf die doppelte Größe skaliert.

## <a name="skew-transform"></a>Verzerrungs Transformation

Eine schiefe Transformation verzerrt ein `Path` -Objekt im x-y-Koordinatensystem 2D und eignet sich zum Erstellen der Illusion von 3D-Tiefe in einem 2D-Objekt.

Die- `SkewTransform` Klasse, die von der- `Transform` Klasse abgeleitet wird, definiert die folgenden Eigenschaften:

- `AngleX`vom Typ `double` , der den Neigungswinkel der x-Achse darstellt, der in Grad gegen den Uhrzeigersinn von der y-Achse aus gemessen wird. Der Standardwert dieser Eigenschaft ist 0,0.
- `AngleY`vom Typ `double` , der den Neigungswinkel der y-Achse darstellt, der in Grad gegen den Uhrzeigersinn von der x-Achse aus gemessen wird. Der Standardwert dieser Eigenschaft ist 0,0.
- `CenterX`vom Typ `double` , der die x-Koordinate des Transformations Mittelpunkts darstellt. Der Standardwert dieser Eigenschaft ist 0,0.
- `CenterY`vom Typ `double` , der die y-Koordinate des Transformations Mittelpunkts darstellt. Der Standardwert dieser Eigenschaft ist 0,0.

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Um die Auswirkung einer Schiefe-Transformation vorherzusagen, sollten Sie die `AngleX` x-Achsen Werte relativ zum ursprünglichen Koordinatensystem neigen. Daher wird `AngleX` die y-Achse für eine von 30 um 30 Grad über den Ursprung rotiert und die Werte in x um 30 Grad von diesem Ursprung. Ebenso wird `AngleY` die y-Werte des-Objekts durch eine von 30 `Path` um 30 Grad vom Ursprung entfernt.

Um ein `Path` -Objekt zu neigen, legen Sie die `CenterX` -Eigenschaft und die-Eigenschaft `CenterY` auf den Mittelpunkt des Objekts fest.

Im folgenden Beispiel wird gezeigt, wie ein- `Path` Objekt verzerrt wird:

```xaml
<Path Stroke="Black"
      StrokeThickness="1"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <SkewTransform CenterX="0"
                       CenterY="0"
                       AngleX="45"
                       AngleY="0" />
    </Path.RenderTransform>
</Path>
```

In diesem Beispiel wird eine horizontale Schiefe von 45 Grad auf das- `Path` Objekt angewendet, von einem Mittelpunkt (0,0).

## <a name="translate-transform"></a>Transformation übersetzen

Eine Translation-Transformation verschiebt ein Objekt im 2D-x-y-Koordinatensystem.

Die- `TranslateTransform` Klasse, die von der- `Transform` Klasse abgeleitet wird, definiert die folgenden Eigenschaften:

- `X`vom Typ `double` , der den Abstand zum Verschieben entlang der x-Achse darstellt. Der Standardwert dieser Eigenschaft ist 0,0.
- `Y`vom Typ `double` , der den Abstand zum Verschieben entlang der y-Achse darstellt. Der Standardwert dieser Eigenschaft ist 0,0.

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Negative `X` Werte verschieben ein Objekt nach links, während positive Werte ein Objekt nach rechts verschieben. Mit negativen `Y` Werten wird ein Objekt nach oben verschoben, während positive Werte ein Objekt nach unten verschieben.

Im folgenden Beispiel wird gezeigt, wie ein-Objekt übersetzt wird `Path` :

```xaml
<Path Stroke="Black"
      StrokeThickness="1"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <TranslateTransform X="50"
                            Y="50" />
    </Path.RenderTransform>
</Path>
```

In diesem Beispiel wird das `Path` Objekt 50 geräteunabhängige Einheiten nach rechts und 50 geräteunabhängige Einheiten nach unten verschoben.

## <a name="apply-multiple-transforms"></a>Anwenden mehrerer Transformationen

Xamarin.Formsverfügt über zwei Klassen, die das Anwenden mehrerer Transformationen auf ein-Objekt unterstützen `Path` . Dies sind `TransformGroup` , und `CompositeTransform` . Eine `TransformGroup` führt Transformationen in beliebiger Reihenfolge durch, während eine `CompositeTransform` Transformationen in einer bestimmten Reihenfolge ausführt.

### <a name="transform-groups"></a>Transformieren von Gruppen

Transformationsgruppen stellen zusammengesetzte Transformationen dar, die aus mehreren `Transform` Objekten bestehen.

Die- `TransformGroup` Klasse, die von der- `Transform` Klasse abgeleitet wird, definiert die folgenden Eigenschaften:

- `Children`vom Typ `TransformCollection` , der eine Auflistung von-Objekten darstellt `Transform` .

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Die Reihenfolge der Transformationen ist in einer zusammengesetzten Transformation wichtig, in der die-Klasse verwendet wird `TransformGroup` . Wenn Sie z. b. zuerst drehen, dann skalieren und dann übersetzen, erhalten Sie ein anderes Ergebnis als bei der ersten Übersetzung, dann drehen und skalieren. Eine Grundordnung ist wichtig, wenn Transformationen wie Drehung und Skalierung in Bezug auf den Ursprung des Koordinatensystems ausgeführt werden. Das Skalieren eines Objekts, das sich auf den Ursprung konzentriert, erzeugt ein anderes Ergebnis für das Skalieren eines Objekts, das vom Ursprung entfernt wurde. Entsprechend erzeugt das Drehen eines Objekts, das am Ursprung zentriert ist, ein anderes Ergebnis als das Drehen eines Objekts, das vom Ursprung entfernt wurde.

Im folgenden Beispiel wird gezeigt, wie eine zusammengesetzte Transformation mithilfe der-Klasse durchgeführt wird `TransformGroup` :

```xaml
<Path Stroke="Black"
      StrokeThickness="1"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <TransformGroup>
            <ScaleTransform ScaleY="2" />
            <RotateTransform Angle="45" />
        </TransformGroup>
    </Path.RenderTransform>
</Path>
```

In diesem Beispiel wird das `Path` Objekt auf die doppelte Größe skaliert und um 45 Grad gedreht.

## <a name="composite-transforms"></a>Zusammengesetzte Transformationen

Eine zusammengesetzte Transformation wendet mehrere Transformationen auf ein Objekt an.

Die- `CompositeTransform` Klasse, die von der- `Transform` Klasse abgeleitet wird, definiert die folgenden Eigenschaften:

- `CenterX`vom Typ `double` , der die x-Koordinate des Mittelpunkts dieser Transformation darstellt. Der Standardwert dieser Eigenschaft ist 0,0.
- `CenterY`vom Typ `double` , der die y-Koordinate des Mittelpunkts dieser Transformation darstellt. Der Standardwert dieser Eigenschaft ist 0,0.
- `ScaleX`vom Typ `double` , der den Skalierungsfaktor für die x-Achse darstellt. Der Standardwert dieser Eigenschaft ist 1,0.
- `ScaleY`vom Typ `double` , der den Skalierungsfaktor für die y-Achse darstellt. Der Standardwert dieser Eigenschaft ist 1,0.
- `SkewX`vom Typ `double` , der den Neigungswinkel der x-Achse darstellt, der in Grad gegen den Uhrzeigersinn von der y-Achse aus gemessen wird. Der Standardwert dieser Eigenschaft ist 0,0.
- `SkewY`vom Typ `double` , der den Neigungswinkel der y-Achse darstellt, der in Grad gegen den Uhrzeigersinn von der x-Achse aus gemessen wird. Der Standardwert dieser Eigenschaft ist 0,0.
- `Rotation``double`stellt den Winkel der Drehung im Uhrzeigersinn in Grad dar. Der Standardwert dieser Eigenschaft ist 0,0.
- `TranslateX`vom Typ `double` , der den Abstand zum Verschieben entlang der x-Achse darstellt. Der Standardwert dieser Eigenschaft ist 0,0.
- `TranslateY`vom Typ `double` , der den Abstand zum Verschieben entlang der y-Achse darstellt. Der Standardwert dieser Eigenschaft ist 0,0.

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Ein `CompositeTransform` wendet Transformationen in dieser Reihenfolge an:

1. Skalieren Sie ( `ScaleX` und `ScaleY` ).
1. Schiefe ( `SkewX` und `SkewY` ).
1. Drehen ( `Rotation` ).
1. Übersetzen ( `TranslateX` , `TranslateY` ).

Wenn Sie mehrere Transformationen auf ein Objekt in einer anderen Reihenfolge anwenden möchten, sollten Sie eine erstellen `TransformGroup` und die Transformationen in der gewünschten Reihenfolge einfügen.

> [!IMPORTANT]
> Ein `CompositeTransform` verwendet die gleichen Mittelpunkte, `CenterX` und `CenterY` für alle Transformationen. Wenn Sie unterschiedliche Mittelpunkte pro Transformation angeben möchten, verwenden Sie `TransformGroup` ,

Im folgenden Beispiel wird gezeigt, wie eine zusammengesetzte Transformation mithilfe der-Klasse durchgeführt wird `CompositeTransform` :

```xaml
<Path Stroke="Black"
      StrokeThickness="1"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <CompositeTransform ScaleX="2"
                            ScaleY="2"
                            Rotation="45"
                            TranslateX="50"
                            TranslateY="50" />
    </Path.RenderTransform>
</Path>
```

In diesem Beispiel wird das `Path` Objekt auf die doppelte Größe skaliert, um 45 Grad gedreht und von 50 geräteunabhängigen Einheiten übersetzt.

## <a name="custom-transforms"></a>Benutzerdefinierte Transformationen

Eine Matrix Transformation manipuliert Objekte oder Koordinatensysteme in einer 2D-Ebene mithilfe einer affinen Matrix. Für Transformationen wird eine 3X3-Matrix verwendet. Sie können affine Matrix Transformationen multiplizieren, um lineare Transformationen zu bilden, z. b. Drehung und Neigung, auf die die Übersetzung folgt.

Die- `MatrixTransform` Klasse, die von der- `Transform` Klasse abgeleitet wird, definiert die folgenden Eigenschaften:

- `Matrix`vom Typ `Matrix` , der die Matrix darstellt, die die Transformation definiert.

Diese Eigenschaften werden durch ein [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekt unterstützt. Dies bedeutet, dass es sich um Ziele von Daten Bindungen und formatierten handelt.

Die- `MatrixTransform` Klasse wird verwendet, um eine benutzerdefinierte Transformation zu erstellen, die nicht von den `RotateTransform` `ScaleTransform` Klassen,, oder bereitgestellt wird `SkewTransform` `TranslateTransform` .

Im folgenden Beispiel wird gezeigt, wie ein- `Path` Objekt mit einem transformiert wird `MatrixTransform` :

```xaml
<Path Stroke="Black"
      StrokeThickness="1"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <MatrixTransform>
            <MatrixTransform.Matrix>
                <!-- M11 stretches, M12 skews -->
                <Matrix OffsetX="10"
                        OffsetY="100"
                        M11="3"
                        M12="2" />
            </MatrixTransform.Matrix>
        </MatrixTransform>
    </Path.RenderTransform>
</Path>
```

In diesem Beispiel ist das `Path` Objekt in der X-und der Y-Dimension gestreckt, verzerrt und versetzt.

Alternativ kann dies in einer vereinfachten Form geschrieben werden, die einen Typkonverter verwendet, der in integriert ist Xamarin.Forms :

```xaml
<Path Stroke="Black"
      StrokeThickness="1"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <MatrixTransform Matrix="3,2,0,1,10,100" />
    </Path.RenderTransform>
</Path>
```

In diesem Beispiel wird die- `Matrix` Eigenschaft als durch Trennzeichen getrennte Zeichenfolge angegeben, die aus sechs Membern besteht: `M11` , `M12` , `M21` , `M22` , `OffsetX` , `OffsetY` .

## <a name="related-links"></a>Verwandte Links

- [Shapedemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapedemos/)
- [Xamarin.FormsFormen](index.md)
