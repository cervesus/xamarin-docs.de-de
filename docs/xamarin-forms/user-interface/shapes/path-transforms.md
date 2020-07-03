---
title: 'Xamarin.FormsFormen: Pfad Transformationen'
description: Eine Xamarin.Forms Transformation definiert, wie ein Pfad Objekt von einem Koordinaten Bereich in einen anderen Koordinaten Bereich transformiert wird.
ms.prod: xamarin
ms.assetid: 07DE3D66-1820-4642-BDDF-84146D40C99D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/02/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 41de95c452212dce77d6365265e4813170c9b9b9
ms.sourcegitcommit: a3f13a216fab4fc20a9adf343895b9d6a54634a5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85853051"
---
# <a name="xamarinforms-shapes-path-transforms"></a>Xamarin.FormsFormen: Pfad Transformationen

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

Eine `Transform` definiert, wie ein- `Path` Objekt von einem Koordinaten Bereich in einen anderen Koordinaten Bereich transformiert wird. Wenn eine Transformation auf ein-Objekt angewendet wird `Path` , wird die Darstellung des Objekts in der Benutzeroberfläche geändert.

Transformationen können in vier allgemeine Klassifizierungen eingeteilt werden: Drehung, Skalierung, Schiefe und Übersetzung. Xamarin.Formsdefiniert eine Klasse für jede dieser Transformations Klassifizierungen:

- `RotateTransform`, die eine `Path` durch einen angegebenen rotiert `Angle` .
- `ScaleTransform`, wodurch ein `Path` -Objekt durch angegebene `ScaleX` -und-Beträge skaliert wird `ScaleY` .
- `SkewTransform`, das ein `Path` -Objekt durch angegebene `AngleX` -und-Beträge vergrenzt `AngleY` .
- `TranslateTransform`, wodurch ein `Path` -Objekt durch angegebene `X` -und-Beträge verschoben wird `Y` .

Xamarin.Formsbietet außerdem die folgenden Klassen zum Erstellen komplexerer Transformationen:

- `TransformGroup`stellt eine zusammengesetzte Transformation dar, die aus mehreren Transformations Objekten besteht.
- `CompositeTransform`, wodurch mehrere Transformations Vorgänge auf ein- `Path` Objekt angewendet werden.
- `MatrixTransform`, wodurch benutzerdefinierte Transformationen erstellt werden, die nicht von den anderen Transformations Klassen bereitgestellt werden.

Alle diese Klassen werden von der- `Transform` Klasse abgeleitet, die eine `Value` Eigenschaft vom Typ definiert `Matrix` . Diese Eigenschaft stellt die aktuelle Transformation als- `Matrix` Objekt dar. Weitere Informationen zur `Matrix` Struktur finden Sie unter [Transformationsmatrix](#transform-matrix).

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
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <ScaleTransform CenterX="0"
                        CenterY="0"
                        ScaleX="1.5"
                        ScaleY="1.5" />
    </Path.RenderTransform>
</Path>
```

In diesem Beispiel wird das `Path` Objekt auf das 1,5-fache der Größe skaliert.

## <a name="skew-transform"></a>Verzerrungs Transformation

Eine schiefe Transformation verzerrt ein `Path` -Objekt im x-y-Koordinatensystem 2D und eignet sich zum Erstellen der Illusion von 3D-Tiefe in einem 2D-Objekt.

Die- `SkewTransform` Klasse, die von der- `Transform` Klasse abgeleitet wird, definiert die folgenden Eigenschaften:

- `AngleX`vom Typ `double` , der den Neigungswinkel der x-Achse darstellt, der in Grad gegen den Uhrzeigersinn von der y-Achse aus gemessen wird. Der Standardwert dieser Eigenschaft ist 0,0.
- `AngleY`vom Typ `double` , der den Neigungswinkel der y-Achse darstellt, der in Grad gegen den Uhrzeigersinn von der x-Achse aus gemessen wird. Der Standardwert dieser Eigenschaft ist 0,0.
- `CenterX`vom Typ `double` , der die x-Koordinate des Transformations Mittelpunkts darstellt. Der Standardwert dieser Eigenschaft ist 0,0.
- `CenterY`vom Typ `double` , der die y-Koordinate des Transformations Mittelpunkts darstellt. Der Standardwert dieser Eigenschaft ist 0,0.

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Um die Auswirkung einer Schiefe-Transformation vorherzusagen, sollten Sie die `AngleX` x-Achsen Werte relativ zum ursprünglichen Koordinatensystem neigen. Daher wird `AngleX` die y-Achse für eine von 30 um 30 Grad über den Ursprung rotiert und die Werte in x um 30 Grad von diesem Ursprung. Ebenso wird `AngleY` die y-Werte des-Objekts durch eine von 30 `Path` um 30 Grad vom Ursprung entfernt.

> [!NOTE]
> Um ein `Path` -Objekt zu neigen, legen Sie die `CenterX` -Eigenschaft und die-Eigenschaft `CenterY` auf den Mittelpunkt des Objekts fest.

Im folgenden Beispiel wird gezeigt, wie ein- `Path` Objekt verzerrt wird:

```xaml
<Path Stroke="Black"
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

## <a name="multiple-transforms"></a>Mehrere Transformationen

Xamarin.Formsverfügt über zwei Klassen, die das Anwenden mehrerer Transformationen auf ein-Objekt unterstützen `Path` . Dies sind `TransformGroup` , und `CompositeTransform` . Eine `TransformGroup` führt Transformationen in beliebiger Reihenfolge durch, während eine `CompositeTransform` Transformationen in einer bestimmten Reihenfolge ausführt.

### <a name="transform-groups"></a>Transformieren von Gruppen

Transformationsgruppen stellen zusammengesetzte Transformationen dar, die aus mehreren `Transform` Objekten bestehen.

Die- `TransformGroup` Klasse, die von der- `Transform` Klasse abgeleitet wird, definiert eine `Children` Eigenschaft vom Typ `TransformCollection` , die eine Auflistung von- `Transform` Objekten darstellt. Diese Eigenschaft wird von einem [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekt unterstützt. Dies bedeutet, dass es sich um das Ziel der Daten Bindungen handeln und formatiert werden kann.

Die Reihenfolge der Transformationen ist in einer zusammengesetzten Transformation wichtig, in der die-Klasse verwendet wird `TransformGroup` . Wenn Sie z. b. zuerst drehen, dann skalieren und dann übersetzen, erhalten Sie ein anderes Ergebnis als bei der ersten Übersetzung, dann drehen und skalieren. Eine Grundordnung ist wichtig, wenn Transformationen wie Drehung und Skalierung in Bezug auf den Ursprung des Koordinatensystems ausgeführt werden. Das Skalieren eines Objekts, das sich auf den Ursprung konzentriert, erzeugt ein anderes Ergebnis für das Skalieren eines Objekts, das vom Ursprung entfernt wurde. Entsprechend erzeugt das Drehen eines Objekts, das am Ursprung zentriert ist, ein anderes Ergebnis als das Drehen eines Objekts, das vom Ursprung entfernt wurde.

Im folgenden Beispiel wird gezeigt, wie eine zusammengesetzte Transformation mithilfe der-Klasse durchgeführt wird `TransformGroup` :

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <TransformGroup>
            <ScaleTransform ScaleX="1.5"
                            ScaleY="1.5" />
            <RotateTransform Angle="45" />
        </TransformGroup>
    </Path.RenderTransform>
</Path>
```

In diesem Beispiel wird das `Path` Objekt auf die 1,5-fache Größe skaliert und dann um 45 Grad gedreht.

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
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <CompositeTransform ScaleX="1.5"
                            ScaleY="1.5"
                            Rotation="45"
                            TranslateX="50"
                            TranslateY="50" />
    </Path.RenderTransform>
</Path>
```

In diesem Beispiel wird das `Path` Objekt auf die 1,5-fache Größe skaliert, dann um 45 Grad gedreht und dann durch 50 geräteunabhängige Einheiten übersetzt.

## <a name="transform-matrix"></a>Transformationsmatrix

Eine Transformation kann in Form einer 3X3-Transformationsmatrix beschrieben werden, mit der Transformationen im 2D-Raum durchführt werden. Diese 3X3-Matrix wird durch die-Struktur dargestellt, bei der es sich um `Matrix` eine Auflistung von drei Zeilen und drei Spalten von `double` Werten handelt.

Die `Matrix` Struktur definiert die folgenden Eigenschaften:

- `Determinant`vom Typ `double` , der den Determinanten der Matrix abruft.
- `HasInverse`vom Typ `bool` , der angibt, ob die Matrix invertierbar ist.
- `Identity`vom Typ `Matrix` , mit dem eine Identitätsmatrix abgerufen wird.
- `HasIdentity`vom Typ `bool` , der angibt, ob die Matrix eine Identitätsmatrix ist.
- `M11`vom Typ `double` , der den Wert der ersten Zeile und ersten Spalte der Matrix darstellt.
- `M12`vom Typ `double` , der den Wert der ersten Zeile und zweiten Spalte der Matrix darstellt.
- `M21`vom Typ `double` , der den Wert der zweiten Zeile und ersten Spalte der Matrix darstellt.
- `M22`vom Typ `double` , der den Wert der zweiten Zeile und zweiten Spalte der Matrix darstellt.
- `OffsetX`vom Typ `double` , der den Wert der dritten Zeile und ersten Spalte der Matrix darstellt.
- `OffsetY`vom Typ `double` , der den Wert der dritten Zeile und zweiten Spalte der Matrix darstellt.

Die `OffsetX` -Eigenschaft und die-Eigenschaft `OffsetY` werden so benannt, dass Sie den Betrag angeben, um den Koordinatenraum entlang der x-Achse bzw. y-Achse zu übersetzen.

Außerdem stellt die `Matrix` Struktur eine Reihe von Methoden zur Verfügung, die zum Bearbeiten der Matrix Werte verwendet werden können, `Append` einschließlich `Invert` , `Multiply` , `Prepend` und vieles mehr.

Die folgende Tabelle zeigt die Struktur einer Xamarin.Forms Matrix:

| | | |
|---------|---------|-----|
| M11     | M12     | 0,0 |
| M21     | M22     | 0,0 |
| OffsetX | OffsetY | 1.0 |

> [!NOTE]
> Die letzte Spalte einer affinen Transformationsmatrix ist gleich (0,0), sodass nur die Elemente in den ersten beiden Spalten angegeben werden müssen.

Durch die Bearbeitung von Matrix Werten können Objekte gedreht, skaliert, verzerrt und übersetzt werden `Path` . Wenn Sie z. b. den `OffsetX` Wert in 100 ändern, können Sie ihn verwenden, um ein `Path` -100 Objekt mit geräteunabhängigen Einheiten auf der x-Achse zu verschieben. Wenn Sie den `M22` Wert in "3" ändern, können Sie ihn verwenden, um ein- `Path` Objekt auf das Dreifache seiner aktuellen Höhe auszudehnen. Wenn Sie beide Werte ändern, verschieben Sie das `Path` Objekt 100 geräteunabhängige Einheiten entlang der x-Achse und Strecken die Höhe um den Faktor 3. Außerdem können affine Transformations Matrizen multipliziert werden, um beliebig viele lineare Transformationen zu bilden, z. b. Drehung und Neigung, gefolgt von der Übersetzung.

## <a name="custom-transforms"></a>Benutzerdefinierte Transformationen

Die- `MatrixTransform` Klasse, die von der- `Transform` Klasse abgeleitet wird, definiert eine `Matrix` Eigenschaft vom Typ `Matrix` , die die Matrix darstellt, die die Transformation definiert. Diese Eigenschaft wird von einem [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekt unterstützt. Dies bedeutet, dass es sich um das Ziel der Daten Bindungen handeln und formatiert werden kann.

Jede Transformation, die Sie mit einem-,-,-oder-Objekt beschreiben können, `TranslateTransform` `ScaleTransform` `RotateTransform` `SkewTransform` kann gleichermaßen von einem beschrieben werden `MatrixTransform` . Die `TranslateTransform` `ScaleTransform` Klassen,, `RotateTransform` und sind jedoch `SkewTransform` einfacher zu konzipieren, als die Vektor Komponenten in einem festzulegen `Matrix` . Daher wird die-Klasse in der `MatrixTransform` Regel verwendet, um benutzerdefinierte Transformationen zu erstellen, die nicht von den `RotateTransform` `ScaleTransform` Klassen,, oder bereitgestellt werden `SkewTransform` `TranslateTransform` .

Im folgenden Beispiel wird gezeigt, wie ein- `Path` Objekt mit einem transformiert wird `MatrixTransform` :

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Center"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <MatrixTransform>
            <MatrixTransform.Matrix>
                <!-- M11 stretches, M12 skews -->
                <Matrix OffsetX="10"
                        OffsetY="100"
                        M11="1.5"
                        M12="1" />
            </MatrixTransform.Matrix>
        </MatrixTransform>
    </Path.RenderTransform>
</Path>
```

In diesem Beispiel ist das `Path` Objekt in der X-und der Y-Dimension gestreckt, verzerrt und versetzt.

Alternativ kann dies in einer vereinfachten Form geschrieben werden, die einen Typkonverter verwendet, der in integriert ist Xamarin.Forms :

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Center"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <MatrixTransform Matrix="1.5,1,0,1,10,100" />
    </Path.RenderTransform>
</Path>
```

In diesem Beispiel wird die- `Matrix` Eigenschaft als durch Trennzeichen getrennte Zeichenfolge angegeben, die aus sechs Membern besteht: `M11` , `M12` , `M21` , `M22` , `OffsetX` , `OffsetY` . Obwohl die Member in diesem Beispiel durch Kommas getrennt sind, können Sie auch durch ein oder mehrere Leerzeichen getrennt werden.

Außerdem kann das vorherige Beispiel noch weiter vereinfacht werden, indem die gleichen sechs Member wie der Wert der-Eigenschaft angegeben werden `RenderTransform` :

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Center"
      RenderTransform="1.5 1 0 1 10 100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z" />
```

## <a name="related-links"></a>Verwandte Links

- [Shapedemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.FormsFormen](index.md)
