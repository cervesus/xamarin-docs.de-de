---
title: SkiaSharp-Linien und -Pfade
description: In diesem Artikel wird erläutert, wie Sie mit skiasharp Linien und Grafik Pfade in- Xamarin.Forms Anwendungen zeichnen und dies mit Beispielcode veranschaulichen.
ms.prod: xamarin
ms.assetid: 316A15FE-383D-4D06-8641-BAC7EE7474CA
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 97c7305e59a023e65535186bbbe39a9c2b7d4c26
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138995"
---
# <a name="skiasharp-lines-and-paths"></a>SkiaSharp-Linien und -Pfade

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Verwenden von skiasharp zum Zeichnen von Linien und Grafik Pfaden_

Im [vorherigen Abschnitt](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md) wurde gezeigt, dass die skiasharp- `SKCanvas` Klasse mehrere Methoden zum Zeichnen von Kreisen, ovalen, Rechtecke, abgerundeten Rechtecke, Text und Bitmaps enthält. In diesem Abschnitt und späteren Abschnitten werden die verschiedenen mit dem Erstellen und Rendern von *Grafik Pfaden*verbundenen Klassen behandelt.

Der Grafik Pfad ist der allgemeinste Ansatz zum Zeichnen von Linien und Kurven in skiasharp. In diesem Abschnitt wird die Verwendung eines [`SKPath`](xref:SkiaSharp.SKPath) Objekts zum Zeichnen von geraden Linien behandelt, und es wird eine Auflistung von kleinen geraden Linien (als *Polylinie*bezeichnet) zum Zeichnen von Kurven verwendet, die Sie algorithmisch definieren können. In einem späteren Abschnitt zu [**skiasharp-Kurven und-Pfaden**](../curves/index.md) werden die verschiedenen Arten von Kurven erläutert, die von unterstützt werden `SKPath` .

Alle Beispiel Programme in diesem Abschnitt werden unter den Überschriften **Linien und Pfaden** auf der Startseite des [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Programms und im Ordner [**Pfade**](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths) der Projekt Mappe angezeigt.

## <a name="lines-and-stroke-caps"></a>[Linien und Strichenden](lines.md)

Erfahren Sie, wie Sie skiasharp zum Zeichnen von Linien mit unterschiedlichen Strich Strichen verwenden können.

## <a name="path-basics"></a>[Grundlagen zu Pfaden](paths.md)

Erkunden Sie das skiasharp- `SKPath` Objekt zum Kombinieren von Linien und Kurven.

## <a name="the-path-fill-types"></a>[Die Fülltypen für Pfade](fill-types.md)

Entdecken Sie die verschiedenen Effekte, die mit skiasharp-Pfad Füll Typen möglich sind.

## <a name="polylines-and-parametric-equations"></a>[Polylinien und parametrische Formeln](polylines.md)

Verwenden Sie skiasharp zum Rendering beliebiger Zeilen, die mit parametrischen Gleichungen definiert werden können.

## <a name="dots-and-dashes"></a>[Punkte und Striche](dots.md)

Beherrschen Sie die Feinheiten der Zeichnung gepunkteter und gestrichelter Linien in skiasharp.

## <a name="finger-painting"></a>[Zeichnen mit Fingern](finger-paint.md)

Verwenden Sie Ihre Finger, um die Zeichenfläche zu zeichnen.

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
