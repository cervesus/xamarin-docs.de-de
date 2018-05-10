---
title: CocosSharp
description: Dieses enthält Dokumentenlinks zu verschiedenen Artikeln über 3D-Spielentwicklung mit CocosSharp.
ms.prod: xamarin
ms.assetid: 5E72869D-3541-408B-AB64-D34C777AFB79
author: charlespetzold
ms.author: chape
ms.date: 03/29/2018
ms.openlocfilehash: a188863cf57706e3f9dd6c8f4d2d3e60b2591e0b
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="cocossharp"></a>CocosSharp

_CocosSharp ist eine Bibliothek zum Erstellen von 2D Spiele, die mit c# und f#. Es ist ein .NET Port des beliebten Cocos2D-Moduls._

## <a name="introduction-to-cocossharp"></a>Einführung in CocosSharp

Die CocosSharp 2D Spiele-Engine bietet Technologie für die plattformübergreifende Spiele vornehmen. Eine vollständige Liste der unterstützten Plattformen finden Sie unter der [CocosSharp-Wiki auf GitHub](https://github.com/mono/CocosSharp/wiki).
Diese Handbücher verwenden C#-Codebeispiele, obwohl CocosSharp ebenfalls mit f# voll funktionsfähig ist.

Der Kern der CocosSharp wird bereitgestellt, indem die [MonoGame Framework](http://www.monogame.net/), dies ist selbst ein plattformübergreifendes, Hardware accelerated API Grafiken, audio, Spiel Zustandsverwaltung, Eingabe und eine Content Pipeline zum Importieren von Ressourcen bereitstellen.
CocosSharp ist eine effiziente Abstraktionsebene für 2D Spiele geeignet.
Darüber hinaus können größere Spiele eigene Optimierungen außerhalb ihrer Core-Bibliotheken ausführen, wenn Spiele Komplexität wächst. CocosSharp bietet also eine Mischung aus einfachen Verwendung und Leistung zu erzielen, und Entwickler können schnell ohne Spiel Größe und Komplexität beginnen.

Diese praktische Video wird gezeigt, wie zum Erstellen einer einfachen plattformübergreifende CocosSharp Spiel:

> [!Video https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Developing-Cross-platform-2D-Games-in-C-and-CocosSharp/player]

## <a name="bouncinggamegraphics-gamescocossharpbouncing-gamemd"></a>[BouncingGame](~/graphics-games/cocossharp/bouncing-game.md)

![BouncingGame](images/bouncing-game.png "BouncingGame")

Dieses Handbuch beschreibt BouncingGame, einschließlich Informationen zum Arbeiten mit Inhalt, die verschiedenen visuellen Elemente, die zum Erstellen eines Spiels, Hinzufügen von Spiellogik und mehr.

## <a name="fruity-falls-gamegraphics-gamescocossharpfruity-fallsmd"></a>[Fruity greift Spiel](~/graphics-games/cocossharp/fruity-falls.md)

![Fruity greift Spiel Screenshot](images/fruity-falls.png "Fruity greift game-bildschirmabbildung")

Dieses Handbuch beschreibt das Spiel Fruity fällt, die allgemeine CocosSharp und 3D-Spielentwicklung-Konzepte, wie z. B. physikalische, inhaltsverwaltung Spiel Zustand und Entwurf abdecken.  

## <a name="coin-time-gamegraphics-gamescocossharpcointimemd"></a>[Münzwurfs Zeit Spiel](~/graphics-games/cocossharp/cointime.md)

![Münzwurfs Zeit game-bildschirmabbildung](images/cointime.png "Münzwurfs Zeit game-bildschirmabbildung")

Münzwurfs ist eine vollständige Platformer Spiele für iOS und Android. Das Ziel des Spiels ist, erfassen alle die Münzen in einer Ebene, und klicken Sie dann die Tür beenden zu erreichen, und vermeidet Feinde und Hindernisse verursacht.

## <a name="drawing-geometry-with-ccdrawnodegraphics-gamescocossharpccdrawnodemd"></a>[Zeichnen von Geometry mit CCDrawNode](~/graphics-games/cocossharp/ccdrawnode.md)

![Formen gezeichnet mit CCDrawNode](images/ccdrawnode.png "Formen gezeichnet mit CCDrawNode")

CCDrawNode stellt Methoden zum Zeichnen primitive Objekte, z. B. Linien, Kreise und Dreiecke bereit.

## <a name="animating-with-ccactiongraphics-gamescocossharpccactionmd"></a>[Animieren mit CCAction](~/graphics-games/cocossharp/ccaction.md)

![Eine Animation CCAction](images/ccaction.png "ein CCAction Animation")

`CCAction` ist eine Basisklasse, die zu animierende CocosSharp Objekte verwendet werden kann. Diese Anleitung enthält integrierte `CCAction` Implementierungen für allgemeine Aufgaben wie das positionieren, skalieren und drehen. Sie prüft auch benutzerdefinierte Implementierungen zu erstellen, durch Vererbung von `CCAction`.

## <a name="using-tiled-with-cocossharpgraphics-gamescocossharptiledmd"></a>[Verwenden von Tiled mit CocosSharp](~/graphics-games/cocossharp/tiled.md)

![Eine Ebene in einem Spiel](images/tiled.png "einer Ebene in einem Spiel")

Kacheleffekt ist ein leistungsfähiges, flexibles und ordnet ausgereifte Anwendung zum Erstellen der Kachel "orthogonale und isometrische" für Spiele. CocosSharp bietet eine integrierte Integration für systemeigene Dateiformat der Fläche.

## <a name="entities-in-cocossharpgraphics-gamescocossharpentitiesmd"></a>[Entitäten in CocosSharp](~/graphics-games/cocossharp/entities.md)

![Eine Raumschiffs aus ein Spiel](images/entities.png "eine Raumschiffs aus ein Spiel")

Das Muster für die Entität ist eine leistungsfähige Möglichkeit zur spielcode zu organisieren. Es verbessert die Lesbarkeit, wird Code einfacher zu verwalten und nutzt die Funktionen der integrierten über-und untergeordneten Elementen.

## <a name="handling-multiple-resolutions-in-cocossharpgraphics-gamescocossharpresolutionsmd"></a>[Behandlung von mehrere Auflösungen in CocosSharp](~/graphics-games/cocossharp/resolutions.md)

![Ein Raster darstellt Bildschirmauflösung](images/resolutions.png "ein Raster, die Bildschirmauflösung darstellt.")

Dieses Handbuch veranschaulicht das Arbeiten mit CocosSharp Spiele entwickeln, die auf Geräten von unterschiedlichen Auflösungen ordnungsgemäß angezeigt.

## <a name="cocossharp-content-pipelinegraphics-gamescocossharpcontent-pipelineindexmd"></a>[CocosSharp-Inhaltspipeline](~/graphics-games/cocossharp/content-pipeline/index.md)

![XNB](images/content-pipeline.png "XNB")

Content Pipelines werden häufig in 3D-Spielentwicklung verwendet, um Inhalt zu optimieren und formatieren Sie ihn so, dass sie auf bestimmte Hardware oder mit bestimmten Frameworks 3D-Spielentwicklung geladen werden können.

## <a name="improving-frame-rate-with-ccspritesheetgraphics-gamescocossharpccspritesheetmd"></a>[Verbessern der Framerate mit CCSpriteSheet](~/graphics-games/cocossharp/ccspritesheet.md)

![Eine Struktur aus einem CCSpriteSheet](images/ccspritesheet.png "eine Struktur aus einem CCSpriteSheet")

`CCSpriteSheet` Stellt Funktionen für viele Bilddateien in eine Textur verwendet und kombiniert. Anzahl der Textur verringern kann ein Spiel Ladezeiten und Framerate verbessert werden.

## <a name="texture-caching-using-cctexturecachegraphics-gamescocossharptexture-cachemd"></a>[Textur Zwischenspeichern mithilfe von CCTextureCache](~/graphics-games/cocossharp/texture-cache.md)

![Eine Darstellung wie CocosSharp Bilder zwischenspeichert](images/texture-cache.png "eine Darstellung wie CocosSharp Bilder zwischenspeichert")

Der CocosSharp `CCTextureCache` -Klasse stellt ein gängiges Verfahren zum Organisieren, Zwischenspeichern und Entladen von Inhalt. 

## <a name="2d-math-with-cocossharpgraphics-gamescocossharpmathmd"></a>[2D mathematischen Funktionen mit CocosSharp](~/graphics-games/cocossharp/math.md)

![Ein Bild rotierenden](images/math.png "ein Bild gedreht wird")

Dieser Leitfaden behandelt, für die Entwicklung von 2D Mathematik. Er CocosSharp zeigen, wie allgemeine Aufgaben zur 3D-Spielentwicklung verwendet und erläutert die mathematischen hinter dieser Aufgaben.

## <a name="performance-and-visual-effects-with-ccrendertexturegraphics-gamescocossharpccrendertexturemd"></a>[Leistung und visuelle Effekte mit CCRenderTexture](~/graphics-games/cocossharp/ccrendertexture.md)

![Ein Sprite aus ein Spiel](images/ccrendertexture.png "ein Sprite aus ein Spiel")

Die `CCRenderTexture` -Klasse bietet eine Funktionalität für das Rendern mehrerer CocosSharp-Objekte, um eine einzelne Struktur. Nachdem dieses erstellt wurde, `CCRenderTexture` Instanzen verwendet werden können, Grafiken effizientes Rendering und visuelle Effekte zu implementieren.
