---
title: Direct2D-Zeichnung mit SkiaSharp
description: Dieses Dokument enthält einen Überblick über die plattformübergreifende 2D-mit SkiaSharp zeichnen. Es enthält links zu verschiedenen Anleitungen, die SkiaSharp beschreiben und die verschiedenen APIs.
ms.prod: xamarin
ms.assetid: A8A61421-4544-422A-A7E0-9355C67DF21E
author: charlespetzold
ms.author: chape
ms.date: 07/17/2018
ms.openlocfilehash: 0c8cbc14308c8c4131e5aaa2bcc0ddfa798af610
ms.sourcegitcommit: 7f2e44e6f628753e06a5fe2a3076fc2ec5baa081
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2018
ms.locfileid: "39130919"
---
# <a name="2d-drawing-with-skiasharp"></a>Direct2D-Zeichnung mit SkiaSharp

SkiaSharp bietet eine leistungsfähige C#-API für die 2D-Grafiken. Es basiert auf [Skia-Bibliothek von Google](http://skia.org), der gleichen Bibliothek, die Google Chrome, Firefox und Android-Grafik Stapel zugrunde liegen.

[![](images/ide-sml.png "SkiaSharp bietet eine leistungsfähige C#-API zum Ausführen von 2D-Grafiken")](images/ide.png#lightbox)

SkiaSharp ist eine Portable Bibliothek und wird einfach als eine [plattformübergreifendes NuGet-Paket](https://www.nuget.org/packages/SkiaSharp), und unterstützt standardmäßig die folgenden Plattformen: MacOS, Xamarin.iOS, Xamarin.Android und den Windows-Desktop.

## <a name="introduction-to-skiasharpgraphics-gamesskiasharpintroductionmd"></a>[Einführung in die SkiaSharp](~/graphics-games/skiasharp/introduction.md)

Eine Übersicht über die grundlegenden Konzepte von SkiaSharp und Beispiel code zum Rendern von Grafiken, Text, Bitmaps und Image-Filter verwenden.

## <a name="skiasharp-tutorials-for-xamarinformsxamarin-formsuser-interfacegraphicsskiasharpindexmd"></a>[SkiaSharp-Lernprogramme für Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)

Informationen Sie zum Arbeiten mit cross-Platform-Grafiken, die in Xamarin.Forms gerendert:

- [Zeichnen die Grundlagen](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md)
  * [Zeichnen eines einfachen Kreises](~/xamarin-forms/user-interface/graphics/skiasharp/basics/circle.md)
  * [Integrieren von Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md)
  * [Pixel und geräteunabhängige Einheiten](~/xamarin-forms/user-interface/graphics/skiasharp/basics/pixels.md)
  * [Grundlegende animation](~/xamarin-forms/user-interface/graphics/skiasharp/basics/animation.md)
  * [Die Integration von Text und Grafiken](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md)
  * [Grundlagen zu Bitmaps](~/xamarin-forms/user-interface/graphics/skiasharp/basics/bitmaps.md)
- [Linien und-Pfade](~/xamarin-forms/user-interface/graphics/skiasharp/paths/index.md)
  * [Linien und Strichenden](~/xamarin-forms/user-interface/graphics/skiasharp/paths/lines.md)
  * [Grundlagen zu Pfaden](~/xamarin-forms/user-interface/graphics/skiasharp/paths/paths.md)
  * [Die Fülltypen für Pfade](~/xamarin-forms/user-interface/graphics/skiasharp/paths/fill-types.md)
  * [Polylinien und parametrische Formeln](~/xamarin-forms/user-interface/graphics/skiasharp/paths/polylines.md)
  * [Punkte und Bindestriche enthalten](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md)
  * [Zeichnen mit Fingern](~/xamarin-forms/user-interface/graphics/skiasharp/paths/finger-paint.md)
- [Transformationen](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/index.md)
  * [Die Verschiebungstransformation](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/translate.md)
  * [Die Skalierungstransformation](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/scale.md)
  * [Die Drehungstransformation](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md)
  * [Die Scherungstransformation](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/skew.md)
  * [Matrixtransformationen](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/matrix.md)
  * [Manipulationen durch toucheingaben](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md)
  * [Nicht affine Transformationen](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md)
  * [3D-Drehung](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md)
- [Kurven und-Pfade](~/xamarin-forms/user-interface/graphics/skiasharp/curves/index.md)
  * [Drei Möglichkeiten, einen Bogen zu zeichnen](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md)
  * [Drei Typen von Bézier-Kurven](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md)
  * [SVG-Pfaddaten](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md)
  * [Schneiden mit Pfaden und Regionen](~/xamarin-forms/user-interface/graphics/skiasharp/curves/clipping.md)
  * [Pfadeffekte](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md)
  * [Pfade und Text](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md)
  * [Pfadinformationen und -enumeration](~/xamarin-forms/user-interface/graphics/skiasharp/curves/information.md)
- [Bitmaps](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/index.md)
  * [Anzeigen von Bitmaps](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/displaying.md)
  * [Erstellen und Zeichnen in Bitmaps](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/drawing.md)
  * [Zuschneiden von Bitmaps](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/cropping.md)
  * [Segmentierte Anzeige der Bitmaps](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/segmented.md)
  * [Speichern von Bitmaps in Dateien](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/saving.md)
  * [Zugreifen auf die Bitmap Pixelbits](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/pixel-bits.md)
  * [Animieren von Bitmaps](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/animating.md)

## <a name="platform-specific-notesgraphics-gamesskiasharpplatformmd"></a>[Plattformspezifische Hinweise](~/graphics-games/skiasharp/platform.md)

Diese Seite beschreibt die setupanweisungen für SkiaSharp auf verschiedenen Plattformen einschließlich iOS, Android, MacOS und Windows.

## <a name="api-documentationhttpsdeveloperxamarincomapinamespaceskiasharp"></a>[API-Dokumentation](https://developer.xamarin.com/api/namespace/SkiaSharp/)

Sie können Durchsuchen der [-API-Dokumentation](https://developer.xamarin.com/api/namespace/SkiaSharp/) für SkiaSharp auf unserer Website.

## <a name="work-in-progress"></a>Aktuelle Arbeiten

SkiaSharp ist noch In Bearbeitung, die wir in unserer Community gemeinsam nutzen. Während wir die wichtigen Teile der API Skia gebunden haben, bleibt viel Arbeit ausgeführt werden. Verwenden wir die stabile C-API von Skia eingeblendet, und unser Plan besteht darin beitragen unserer Arbeit für die C-Bindungen von Skia volle Abdeckung für die APIs bereitstellen.

Können wir unsere bemühungen Bindung geführt, hinterlassen Sie Kommentare oder Vorschläge als Probleme auf dem GitHub-Repository [ http://github.com/mono/SkiaSharp ](http://github.com/mono/SkiaSharp).
