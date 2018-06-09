---
title: SkiaSharp Linien und Pfade
description: In diesem Artikel wird erläutert, wie SkiaSharp zum Zeichnen von Linien und Grafikpfade in Xamarin.Forms-Anwendungen verwenden, und wird dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.assetid: 316A15FE-383D-4D06-8641-BAC7EE7474CA
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: a922f7ec91624e20a9afedb8063328bdb7a4d42d
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243752"
---
# <a name="skiasharp-lines-and-paths"></a>SkiaSharp Linien und Pfade

_Verwenden Sie SkiaSharp zum Zeichnen von Linien und Grafiken Pfade_

Die [vorherigen Abschnitt](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md) veranschaulicht, die die SkiaSharp `SKCanvas` Klasse enthält mehrere Methoden zum Zeichnen von Rechtecken, Ellipsen und abgerundeten Rechtecken. Inhalt dieses Abschnitts und in späteren Abschnitten behandelt die verschiedenen Klassen, die durch das Erstellen und Rendern verbunden *Grafikpfade*.

Der Grafikpfad ist der allgemeinste Ansatz zum Zeichnen von Linien und Kurven in SkiaSharp. In diesem Abschnitt erläutert die Verwendung einer `SKPath` Objekt gerade Linien gezeichnet werden soll, und verwenden eine Auflistung von kleinen geraden (aufgerufen eine *Polylinie*) Kurven gezeichnet werden soll, die Sie, mathematisch definieren können. Ein später Abschnitt wird erläutert, die verschiedene Arten von Kurven von unterstützten `SKPath`.

Die Beispielprogramme in diesem Abschnitt angezeigt wird, unter der Überschrift **Linien und Pfade** auf der Startseite von der [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) Programm, und klicken Sie in der [ **Pfade** ](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths) Ordner der Projektmappe.

## <a name="lines-and-stroke-capslinesmd"></a>[Linien und Strichenden](lines.md)

Erfahren Sie, wie SkiaSharp zum Zeichnen von Linien mit anderen Strichabschlüssen verwenden.

## <a name="path-basicspathsmd"></a>[Grundlagen zu Pfaden](paths.md)

Untersuchen Sie das SkiaSharp SKPath-Objekt für das Kombinieren von Linien und Kurven.

## <a name="the-path-fill-typesfill-typesmd"></a>[Die Fülltypen für Pfade](fill-types.md)

Ermitteln Sie die verschiedenen Effekte mit SkiaSharp Pfad ausfüllen Typen möglich.

## <a name="polylines-and-parametric-equationspolylinesmd"></a>[Polylinien und parametrische Formeln](polylines.md)

Verwenden Sie SkiaSharp, um eine beliebige Zeile zu rendern, die Sie mit parametrische Formeln definieren können.

## <a name="dots-and-dashesdotsmd"></a>[Punkte und Striche](dots.md)

Die Komplexität der Zeichnung SkiaSharp punktierter und gestrichelter Linien "Master".

## <a name="finger-paintingfinger-paintmd"></a>[Zeichnen mit Fingern](finger-paint.md)

Verwenden Sie die Finger im Zeichenbereich Paint-Ereignis.


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
