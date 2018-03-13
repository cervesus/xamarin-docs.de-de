---
title: 2D Zeichnung
description: Cross Platform 2D Zeichnen mit SkiaSharp
ms.topic: article
ms.prod: xamarin
ms.assetid: A8A61421-4544-422A-A7E0-9355C67DF21E
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 09/14/2017
ms.openlocfilehash: ee0625f22062fef3c27a697ce33488274abc24d9
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="2d-drawing"></a>2D Zeichnung

SkiaSharp bietet eine leistungsstarke C#-API auf diese Weise 2D Grafiken. Es beruht [Googles Skia Bibliothek](http://skia.org), die gleiche Bibliothek, die Google Chrome, Firefox und Android-Grafik Stacks verwendet.

[![](images/ide-sml.png "SkiaSharp bietet eine leistungsstarke C#-API auf diese Weise 2D Grafiken")](images/ide.png#lightbox)

SkiaSharp ist eine Portable Bibliothek und bequem als liefert eine [plattformübergreifende NuGet-Paket](https://www.nuget.org/packages/SkiaSharp), und unterstützt die folgenden Plattformen ausgegeben: MacOS, Xamarin.Android, Xamarin.iOS und dem Windows-Desktop.

## <a name="introduction-to-skiasharpgraphics-gamesskiasharpintroductionmd"></a>[Einführung in SkiaSharp](~/graphics-games/skiasharp/introduction.md)

Eine Übersicht über die grundlegenden Konzepte von SkiaSharp und Beispiel code zum Rendern von Grafiken, Text, Bitmaps und Image-Filter zu verwenden.

## <a name="skiasharp-tutorials-for-xamarinformsxamarin-formsuser-interfacegraphicsskiasharpindexmd"></a>[SkiaSharp Lernprogramme für Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)

Weitere Informationen Sie zum Arbeiten mit cross-Platform-Grafiken, die in Xamarin.Forms gerendert:

- [Grundlagen](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md)
  * [Zeichnen einen einfachen Kreis](~/xamarin-forms/user-interface/graphics/skiasharp/basics/circle.md)
  * [Integrieren von Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md)
  * [Pixel und geräteunabhängigen Einheiten](~/xamarin-forms/user-interface/graphics/skiasharp/basics/pixels.md)
  * [Grundlegende animation](~/xamarin-forms/user-interface/graphics/skiasharp/basics/animation.md)
  * [Integrieren von Text und Grafiken](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md)
  * [Bitmap-Grundlagen](~/xamarin-forms/user-interface/graphics/skiasharp/basics/bitmaps.md)
- [Zeilen und Pfaden](~/xamarin-forms/user-interface/graphics/skiasharp/paths/index.md)
  * [Linien und Strich caps](~/xamarin-forms/user-interface/graphics/skiasharp/paths/lines.md)
  * [Pfad-Grundlagen](~/xamarin-forms/user-interface/graphics/skiasharp/paths/paths.md)
  * [Die Typen der Pfad ausfüllen](~/xamarin-forms/user-interface/graphics/skiasharp/paths/fill-types.md)
  * [Polylinien und parametrische Formeln](~/xamarin-forms/user-interface/graphics/skiasharp/paths/polylines.md)
  * [Punkte und Bindestriche enthalten](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md)
  * [Zeichnen mit Fingern](~/xamarin-forms/user-interface/graphics/skiasharp/paths/finger-paint.md)
- [Transformationen](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/index.md)
  * [Der Translationstransformation](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/translate.md)
  * [Die Skalierungstransformation für die](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/scale.md)
  * [Die Rotationstransformation](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md)
  * [Die schiefsymmetrische Transformation](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/skew.md)
  * [Matrixtransformationen](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/matrix.md)
  * [Tippen Sie auf Manipulationen](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md)
  * [Nicht-affine Transformationen](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md)
  * [3D-Drehung](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md)
- [Kurven und Pfaden](~/xamarin-forms/user-interface/graphics/skiasharp/curves/index.md)
  * [Drei Möglichkeiten, einen Bogen zu zeichnen](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md)
  * [Drei Typen von Bézier-Kurven](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md)
  * [SVG-Pfaddaten](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md)
  * [Schneiden mit Pfaden und Regionen](~/xamarin-forms/user-interface/graphics/skiasharp/curves/clipping.md)
  * [Pfadeffekte](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md)
  * [Pfade und Text](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md)
  * [Pfadinformationen und -enumeration](~/xamarin-forms/user-interface/graphics/skiasharp/curves/information.md)

## <a name="platform-specific-notesgraphics-gamesskiasharpplatformmd"></a>[Plattformspezifische Hinweise](~/graphics-games/skiasharp/platform.md)

Diese Seite beschreibt die setupanweisungen für SkiaSharp auf verschiedenen Plattformen, einschließlich iOS, Android, MacOS und Windows.

## <a name="api-documentationhttpsdeveloperxamarincomapinamespaceskiasharp"></a>[API-Dokumentation](https://developer.xamarin.com/api/namespace/SkiaSharp/)

Sie können Durchsuchen der [API-Dokumentation](https://developer.xamarin.com/api/namespace/SkiaSharp/) für SkiaSharp auf unserer Website.

## <a name="work-in-progress"></a>Aktuelle Arbeiten

SkiaSharp ist eine In Bearbeitung, die wir mit unserer Community gemeinsam nutzen. Während es wichtige Teile der API Skia gebunden haben, bleibt viel Arbeit ausgeführt werden. Verwenden wir die stabile C#-API von Skia angefügt und unser Plan wird fortgesetzt, Beitragende unsere Arbeit für die C-Bindungen des Skia vollständige Coverage an die-APIs bereitstellen.

Führt die Bindung bemühungen, lassen Sie Kommentare oder Vorschläge für das GitHub-Repository als Probleme helfen [http://github.com/mono/SkiaSharp](http://github.com/mono/SkiaSharp).
