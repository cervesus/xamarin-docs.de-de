---
title: 'Xamarin.FormsShapes: Pfad Markup Syntax'
description: Xamarin.FormsMit der Pfad Markup Syntax können Sie Pfadgeometrien in XAML kompakt angeben.
ms.prod: xamarin
ms.assetid: A2C1BD59-1A16-4E26-A825-0338E2AF9E65
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/19/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 68b7f4a245a60df1723f5a6442f30dc2b1a15932
ms.sourcegitcommit: 91b4d2f93687fadec5c3f80aadc8f7298d911624
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2020
ms.locfileid: "85794983"
---
# <a name="xamarinforms-shapes-path-markup-syntax"></a>Xamarin.FormsShapes: Pfad Markup Syntax

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

Xamarin.Formsmit der Pfad Markup Syntax können Sie Pfadgeometrien in XAML kompakt angeben. Die Syntax wird als Zeichen folgen Wert für die- `Path.Data` Eigenschaft angegeben:

```xaml
<Path Stroke="Black"
      Data="M13.908992,16.207977 L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983Z" />
```

Die Pfad Markup Syntax besteht aus einem optionalen `FillRule` Wert und einer oder mehreren Abbildung Beschreibungen. Diese Syntax kann wie folgt ausgedrückt werden: `<Path Data="` *[FillRule]* *figuredescription* *[figuredescription]* * `" ... />`

In dieser Syntax:

- *FillRule* ist ein optionaler Wert `Xamarin.Forms.Shapes.FillRule` , der angibt, ob die Geometrie oder verwenden soll `EvenOdd` `Nonzero` `FillRule` . `F0`wird verwendet, um die `EvenOdd` Füllregel anzugeben, während `F1` zum Angeben der `Nonzero` Füllregel verwendet wird. Weitere Informationen zu Füll Regeln finden Sie unter [ Xamarin.Forms Shapes: Füll Regeln](fillrules.md).
- *figuredescription* stellt eine Abbildung dar, die aus einem Move-Befehl, zeichnen-Befehlen und einem optionalen Close-Befehl besteht. Ein Move-Befehl gibt den Startpunkt der Abbildung an. Zeichnen-Befehle beschreiben den Inhalt der Abbildung, und der optionale Close-Befehl schließt die Figur.

Im obigen Beispiel gibt die Pfad Markup Syntax einen Startpunkt mithilfe des Verschiebungs Befehls ( `M` ), eine Reihe von geraden Linien mit dem Zeilen Befehl ( `L` ) an und schließt den Pfad mit dem Befehl "Schließen" ( `Z` ).

In der Pfad Markup Syntax sind vor oder nach Befehlen keine Leerzeichen erforderlich. Außerdem müssen zwei Zahlen nicht durch ein Komma oder Leerraum voneinander getrennt werden. Dies ist jedoch nur möglich, wenn die Zeichenfolge eindeutig ist.

> [!TIP]
> Die Pfad Markup Sprache verwendet eine Syntax, die mit den Bild Pfad Definitionen für die skalierbare Vektorgrafik (SVG) kompatibel ist, und kann daher zum Portieren von Grafiken aus dem SVG-Format verwendet werden.

## <a name="move-command"></a>Verschieben-Befehl

Der Move-Befehl gibt den Startpunkt einer neuen Abbildung an. Die Syntax für diesen Befehl lautet: `M` *StartPoint* oder `m` *StartPoint*.

In dieser Syntax ist *StartPoint* eine [`Point`](xref:Xamarin.Forms.Point) Struktur, die den Ausgangspunkt einer neuen Abbildung angibt. Wenn Sie nach dem Move-Befehl mehrere Punkte auflisten, wird eine Linie zu diesen Punkten gezeichnet.

`M 10,10`ist ein Beispiel für einen gültigen Move-Befehl.

## <a name="draw-commands"></a>Zeichnen-Befehle

Ein Draw-Befehl kann aus mehreren Shape-Befehlen bestehen. Die folgenden draw-Befehle sind verfügbar:

- Zeile ( `L` oder `l` ).
- Horizontale Linie ( `H` oder `h` ).
- Vertikale Linie ( `V` oder `v` ).
- Kubische Bezier-Kurve ( `C` oder `c` ).
- Quadratische Bezier-Kurve ( `Q` oder `q` ).
- Glatte kubische Bezier-Kurve ( `S` oder `s` ).
- Glatte quadratische Bezier-Kurve ( `T` oder `t` ).
- Elliptischer Bogen ( `A` oder `a` ).

Jeder zeichnen-Befehl wird ohne Beachtung der Groß-/Kleinschreibung angegeben. Wenn Sie nacheinander mehrere Befehle des gleichen Typs eingeben, müssen Sie die Befehle nicht doppelt eingeben. Beispielsweise `L 100,200 300,400` entspricht `L 100,200 L 300,400` .

### <a name="line-command"></a>Linienbefehl

Der Line-Befehl erstellt eine gerade Linie zwischen dem aktuellen Punkt und dem angegebenen Endpunkt. Die Syntax für diesen Befehl lautet: `L` *Endpunkt* oder `l` *Endpunkt*.

In dieser Syntax ist *EndPoint* eine [`Point`](xref:Xamarin.Forms.Point) , die den Endpunkt der Zeile darstellt.

`L 20,30` und `L 20 30` sind Beispiele für gültige Linienbefehle.

### <a name="horizontal-line-command"></a>Befehl für eine horizontale Linie

Der Befehl für die horizontale Linie erstellt eine horizontale Linie zwischen dem aktuellen Punkt und der angegebenen x-Koordinate. Die Syntax für diesen Befehl lautet: `H` *x* oder `h` *x*.

In dieser Syntax ist *x* eine `double` , die die x-Koordinate des Endpunkts der Linie darstellt.

`H 90` ist ein Beispiel für einen gültigen Befehl für eine horizontale Linie.

### <a name="vertical-line-command"></a>Befehl für eine vertikale Linie

Der Befehl für die vertikale Linie erstellt eine vertikale Linie zwischen dem aktuellen Punkt und der angegebenen y-Koordinate. Die Syntax für diesen Befehl lautet: `V` *y* oder `v` *y*.

In dieser Syntax ist *y* eine `double` , die die y-Koordinate des Endpunkts der Linie darstellt.

`V 90` ist ein Beispiel für einen gültigen Befehl für eine vertikale Linie.

### <a name="cubic-bezier-curve-command"></a>Befehl "kubisch Bezier Kurve"

Der Befehl kubische Bezier-Kurve erstellt eine kubische Bezier-Kurve zwischen dem aktuellen Punkt und dem angegebenen Endpunkt, indem der zwei angegebene Steuerungspunkt verwendet wird. Die Syntax für diesen Befehl lautet: `C` *controlPoint1* *controlPoint2* *EndPoint* oder `c` *controlPoint1* *controlPoint2* *EndPoint*.

In dieser Syntax:

- *controlPoint1* ist ein [`Point`](xref:Xamarin.Forms.Point) , der den ersten Kontrollpunkt der Kurve darstellt, der den Anfangs Tangenten der Kurve bestimmt.
- *controlPoint2* ist ein [`Point`](xref:Xamarin.Forms.Point) , der den zweiten Kontrollpunkt der Kurve darstellt, der den endetangens der Kurve bestimmt.
- der *Endpunkt* ist eine [`Point`](xref:Xamarin.Forms.Point) , die den Punkt darstellt, an dem die Kurve gezeichnet wird.

`C 100,200 200,400 300,200`ist ein Beispiel für einen gültigen Befehl für eine kubische Bezier-Kurve.

### <a name="quadratic-bezier-curve-command"></a>Befehl "Quadratic Bezier Curve"

Der Befehl quadratischen Bezier Curve erstellt eine quadratische Bezier-Kurve zwischen dem aktuellen Punkt und dem angegebenen Endpunkt, indem der angegebene Steuerungspunkt verwendet wird. Die Syntax für diesen Befehl lautet: `Q` *ControlPoint* *EndPoint* oder `q` *ControlPoint* *EndPoint*.

In dieser Syntax:

- *ControlPoint* ist ein [`Point`](xref:Xamarin.Forms.Point) -Element, das den Steuerungspunkt der Kurve darstellt, der die Start-und End-Tangenten der Kurve bestimmt.
- der *Endpunkt* ist eine [`Point`](xref:Xamarin.Forms.Point) , die den Punkt darstellt, an dem die Kurve gezeichnet wird.

`Q 100,200 300,200`ist ein Beispiel für einen gültigen Befehl für eine quadratische Bezier-Kurve.

### <a name="smooth-cubic-bezier-curve-command"></a>Befehl "glatte kubische Bezier-Kurve"

Der Smooth kubische Bezier-Kurven Befehl erstellt eine kubische Bezier-Kurve zwischen dem aktuellen Punkt und dem angegebenen Endpunkt, indem der angegebene Steuerungspunkt verwendet wird. Die Syntax für diesen Befehl lautet: `S` *controlPoint2* *EndPoint* oder `s` *controlPoint2* *EndPoint*.  

In dieser Syntax:

- *controlPoint2* ist ein [`Point`](xref:Xamarin.Forms.Point) , der den zweiten Kontrollpunkt der Kurve darstellt, der den endetangens der Kurve bestimmt.
- der *Endpunkt* ist eine [`Point`](xref:Xamarin.Forms.Point) , die den Punkt darstellt, an dem die Kurve gezeichnet wird.

Der erste Kontrollpunkt wird als Reflektion des zweiten Kontroll Punkts des vorherigen Befehls relativ zum aktuellen Punkt angenommen. Wenn kein vorheriger Befehl vorhanden ist oder der vorherige Befehl kein kubischer Bezier-Kurven Befehl oder ein Smooth kubischer Bezier-Kurven Befehl war, wird angenommen, dass der erste Kontrollpunkt mit dem aktuellen Punkt Coincident ist.

`S 100,200 200,300`ist ein Beispiel für einen gültigen Befehl für eine Smooth kubische Bezier-Kurve.

### <a name="smooth-quadratic-bezier-curve-command"></a>Smooth quadratischen Bezier-Kurven Befehl

Der Smooth quadratischen Bezier-Kurven Befehl erstellt mithilfe eines Steuerungs Punkts eine quadratische Bezier-Kurve zwischen dem aktuellen Punkt und dem angegebenen Endpunkt. Die Syntax für diesen Befehl lautet: `T` *Endpunkt* oder `t` *Endpunkt*.

In dieser Syntax ist *EndPoint* eine [`Point`](xref:Xamarin.Forms.Point) , die den Punkt darstellt, an dem die Kurve gezeichnet wird.

Der Kontrollpunkt soll die Reflektion des Kontrollpunkts des vorherigen Befehls relativ zum aktuellen Punkt sein. Wenn kein vorheriger Befehl vorhanden ist, oder wenn der vorherige Befehl keine quadratische und keine quadratische Bezier-Kurve war, wird davon ausgegangen, dass der Kontrollpunkt mit dem aktuellen Punkt koincident ist.

`T 100,30`ist ein Beispiel für einen gültigen Smooth quadratischen-Befehl, der sich in der quadratischen Befehls Kurve befindet.

### <a name="elliptical-arc-command"></a>Befehl für einen Ellipsenbogen

Der Befehl elliptischer Bogen erstellt einen elliptischen Bogen zwischen dem aktuellen Punkt und dem angegebenen Endpunkt. Die Syntax für diesen Befehl lautet: `A` *size* *RotationAngle* *islargearcflag* *sweepdirectionflag* *EndPoint* oder `a` *size* *RotationAngle* *islargearcflag* *sweepdirectionflag* *EndPoint*.

In dieser Syntax:

- `size`ist ein [`Size`](xref:Xamarin.Forms.Size) , das den x-und y-Radius des Bogens darstellt.
- `rotationAngle`ist eine `double` , die die Drehung der Ellipse in Grad darstellt.
- `isLargeArcFlag`sollte auf 1 festgelegt werden, wenn der Winkel des Bogens 180 Grad oder größer sein soll; andernfalls wird der Wert auf 0 festgelegt.
- `sweepDirectionFlag`sollte auf 1 festgelegt werden, wenn der Bogen in einer Richtung mit positivem Winkel gezeichnet wird; andernfalls wird 0 festgelegt.
- `endPoint`ist ein [`Point`](xref:Xamarin.Forms.Point) , zu dem der Bogen gezeichnet wird.

`A 150,150 0 1,0 150,-150`ist ein Beispiel für einen gültigen Befehl für einen Ellipsen Bogen.

## <a name="close-command"></a>Schließen-Befehl

Der Close-Befehl beendet die aktuelle Figur und erstellt eine Zeile, die den aktuellen Punkt mit dem Ausgangspunkt der Abbildung verbindet. Daher erstellt dieser Befehl einen Zeilen Beitritt zwischen dem letzten Segment und dem ersten Segment der Abbildung.

Die Syntax für den Close-Befehl lautet: `Z` oder `z` .

## <a name="additional-values"></a>Zusätzliche Werte

Anstelle eines numerischen Standardwerts können Sie auch die folgenden Sonderwerte unter Berücksichtigung der Groß-/Kleinschreibung verwenden:

- `Infinity`stellt dar `double.PositiveInfinity` .
- `-Infinity`stellt dar `double.NegativeInfinity` .
- `NaN`stellt dar `double.NaN` .

Darüber hinaus können Sie auch die Groß-/Kleinschreibung in wissenschaftlicher Notation verwenden. Daher `+1.e17` ist ein gültiger-Wert.

## <a name="related-links"></a>Verwandte Links

- [Shapedemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.FormsFormen: Geometrien](geometries.md)
- [Xamarin.FormsFormen: Füll Regeln](fillrules.md)
