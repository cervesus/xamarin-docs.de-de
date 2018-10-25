---
title: SkiaSharp-Linien und-Pfade
description: In diesem Artikel wird erläutert, wie SkiaSharp, die zum Zeichnen von Linien und Grafikpfade in Xamarin.Forms-Anwendungen verwenden, und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.assetid: 316A15FE-383D-4D06-8641-BAC7EE7474CA
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: 3f2597c67459e407ac066ee19d54d134d60f3076
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "39615134"
---
# <a name="skiasharp-lines-and-paths"></a>SkiaSharp-Linien und-Pfade

_Verwenden von SkiaSharp zum Zeichnen von Linien und Grafiken Pfade_

Die [vorherigen Abschnitt](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md) veranschaulicht, die die SkiaSharp `SKCanvas` Klasse enthält mehrere Methoden zum Zeichnen der Kreise, Ellipsen, Rechtecken, abgerundete Rechtecke, Text und Bitmaps. Dieser Abschnitt und in späteren Abschnitten behandelt die verschiedenen Klassen, die Verbindung mit dem Erstellen und Rendern *Grafikpfade*.

Der Grafikpfad ist der allgemeinste Ansatz zum Zeichnen von Linien und Kurven in SkiaSharp. Dieser Abschnitt befasst sich mit einer [ `SKPath` ](xref:SkiaSharp.SKPath) Objekt gerade Linien zu zeichnen und eine Auflistung von kleinen gerade Linien mit (wird aufgerufen, eine *Polylinie*) zum Zeichnen von Kurven, die Sie über einen Algorithmus definieren können. In einem späteren Abschnitt [ **SkiaSharp-Kurven und-Pfade** ](../curves/index.md) erläutert die verschiedenen Arten von Kurven von unterstützten `SKPath`.

Die Beispielprogramme in diesem Abschnitt angezeigt wird, unter der Überschrift **Linien und-Pfade** auf der Startseite des der [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) Programm, und klicken Sie in der [ **Pfade** ](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths) Ordner der Lösung.

## <a name="lines-and-stroke-capslinesmd"></a>[Linien und Strichenden](lines.md)

Erfahren Sie, wie SkiaSharp zum Zeichnen von Linien mit anderen Strichenden verwenden.

## <a name="path-basicspathsmd"></a>[Grundlagen zu Pfaden](paths.md)

Untersuchen Sie die SkiaSharp `SKPath` Objekt für das Kombinieren von Linien und Kurven.

## <a name="the-path-fill-typesfill-typesmd"></a>[Die Fülltypen für Pfade](fill-types.md)

Entdecken Sie die verschiedenen Effekte, die mit Fülltypen für Pfade SkiaSharp möglich.

## <a name="polylines-and-parametric-equationspolylinesmd"></a>[Polylinien und parametrische Formeln](polylines.md)

Verwenden von SkiaSharp zum Rendern jeder Zeile, die Sie mit parametrische Formeln definieren können.

## <a name="dots-and-dashesdotsmd"></a>[Punkte und Striche](dots.md)

Meistern Sie die feinheiten der punktierte und gestrichelte Linien im SkiaSharp zu zeichnen.

## <a name="finger-paintingfinger-paintmd"></a>[Zeichnen mit Fingern](finger-paint.md)

Verwenden Sie die Finger, um auf der Leinwand zeichnen.


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
