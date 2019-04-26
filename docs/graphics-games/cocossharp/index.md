---
title: CocosSharp 2D-Spiele-Engine
description: Dieses Dokument enthält Links zu verschiedenen Artikeln über die Spieleentwicklung mit CocosSharp. Verknüpfter Inhalt beschreibt die Beispiel-apps, zeichnen, Animation und vieles mehr.
ms.prod: xamarin
ms.assetid: 5E72869D-3541-408B-AB64-D34C777AFB79
author: conceptdev
ms.author: crdun
ms.date: 03/29/2018
ms.openlocfilehash: add73360ea98d8c516e413f0cc0264f68c58d79d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61024456"
---
# <a name="cocossharp-2d-game-engine"></a>CocosSharp 2D-Spiele-Engine

_CocosSharp ist eine Bibliothek zum Erstellen von 2D-Spielen mit C# und F#. Es handelt sich um eine .NET-Port, der beliebten Cocos2D-Engine._

## <a name="introduction-to-cocossharp"></a>Einführung in CocosSharp

Die CocosSharp-2D-Spiele-Engine bietet Technologien zum Erstellen von plattformübergreifenden spielen. Eine vollständige Liste der unterstützten Plattformen finden Sie unter den [CocosSharp-Wiki zu GitHub](https://github.com/mono/CocosSharp/wiki).
Verwenden Sie diese Handbücher C# Codebeispiele, obwohl CocosSharp mit vollständig funktionsfähig ist F# ebenfalls.

Der Kern der CocosSharp erfolgt über die [MonoGame-Framework](http://www.monogame.net/), dies ist selbst ein plattformübergreifendes, Hardwarebeschleunigung-API-Grafiken, Audio- und Spiele für die Zustandsverwaltung, Eingabe und eine Pipeline für Bildinhalte zum Importieren von Ressourcen bereitstellen.
CocosSharp ist eine effiziente Abstraktionsschicht eignet sich gut für 2D-Spiele.
Darüber hinaus können größere Spiele eigene Optimierungen außerhalb ihrer Kernbibliotheken ausführen, mit zunehmender Spiele Komplexität. CocosSharp enthält also eine Mischung aus einfachen Verwendung und Leistung zu erzielen, ermöglicht es Entwicklern, schnell ohne Einschränkung Spiel Größe und Komplexität beginnen.

Dieses praktische Video zeigt die Vorgehensweise: Erstellen Sie eine einfache plattformübergreifende CocosSharp Spiel:

> [!Video https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Developing-Cross-platform-2D-Games-in-C-and-CocosSharp/player]

## <a name="bouncinggamegraphics-gamescocossharpbouncing-gamemd"></a>[BouncingGame](~/graphics-games/cocossharp/bouncing-game.md)

![BouncingGame](images/bouncing-game.png "BouncingGame")

Dieses Handbuch beschreibt BouncingGame, einschließlich Informationen zum Arbeiten mit Spielen Inhalt, der verschiedenen visuellen Elemente, die zum Erstellen eines Spiels, Hinzufügen von Spiellogik und mehr.

## <a name="fruity-falls-gamegraphics-gamescocossharpfruity-fallsmd"></a>[Fruity in diesem Spiel](~/graphics-games/cocossharp/fruity-falls.md)

![Screenshot von Spielen Fruity fällt](images/fruity-falls.png "Fruity in diesem Screenshot von Spielen")

Dieses Handbuch beschreibt das Spiel Fruity liegt, die allgemeine CocosSharp und Entwicklung von Spielen-Konzepte wie z. B. Physik, inhaltsverwaltung, Spielzustands und Spiele entwerfen abdecken.  

## <a name="coin-time-gamegraphics-gamescocossharpcointimemd"></a>[Coin Time-Spiel](~/graphics-games/cocossharp/cointime.md)

![Coin Time-game-Screenshot](images/cointime.png "Münze Zeit game-bildschirmabbildung")

Münze wird eine vollständige Platformer Spiele für iOS und Android. Ziel des Spiels ist es, erfassen alle die Münzen auf einer Ebene, und klicken Sie dann die Tür beenden zugleich Feinde und Hindernisse zu erreichen.

## <a name="drawing-geometry-with-ccdrawnodegraphics-gamescocossharpccdrawnodemd"></a>[Zeichnen von Geometrie mit CCDrawNode](~/graphics-games/cocossharp/ccdrawnode.md)

![Formen, Zeichnen mit CCDrawNode](images/ccdrawnode.png "Formen, Zeichnen mit CCDrawNode")

CCDrawNode stellt Methoden zum Zeichnen primitive Objekte wie Linien, Kreise und Dreiecke bereit.

## <a name="animating-with-ccactiongraphics-gamescocossharpccactionmd"></a>[Animieren mit CCAction](~/graphics-games/cocossharp/ccaction.md)

![Eine Animation CCAction](images/ccaction.png "ein CCAction-Animation")

`CCAction` ist eine Basisklasse, die zum Animieren von CocosSharp-Objekte verwendet werden kann. Dieses Handbuch enthält integrierte `CCAction` Implementierungen für allgemeine Aufgaben wie das positionieren, Skalierung und Rotation. Er befasst sich auch mit Erstellen benutzerdefinierte Implementierungen durch Erben von `CCAction`.

## <a name="using-tiled-with-cocossharpgraphics-gamescocossharptiledmd"></a>[Verwenden von Tiled mit CocosSharp](~/graphics-games/cocossharp/tiled.md)

![Eine Ebene in einem Spiel](images/tiled.png "eine Ebene in einem Spiel")

Kacheleffekt ist ein leistungsfähiges, flexibles und ordnet ausgereifte Anwendung zum Erstellen der Kachel "orthogonal und isometrischen" für Spiele. CocosSharp bietet eine integrierte Integration für systemeigene Dateiformat der Fläche.

## <a name="entities-in-cocossharpgraphics-gamescocossharpentitiesmd"></a>[Entitäten in CocosSharp](~/graphics-games/cocossharp/entities.md)

![Ein Raumschiff aus einem Spiel](images/entities.png "ein Raumschiff aus einem Spiel")

Das Muster für die Entität ist eine leistungsfähige Möglichkeit zur Spiele-Code zu organisieren. Es verbessert die Lesbarkeit, wird Code einfacher zu verwalten und nutzt integrierte über-und untergeordneten Funktionen.

## <a name="handling-multiple-resolutions-in-cocossharpgraphics-gamescocossharpresolutionsmd"></a>[Verarbeiten mehrerer Auflösungen in CocosSharp](~/graphics-games/cocossharp/resolutions.md)

![Ein Raster darstellt Bildschirmauflösung](images/resolutions.png "ein Raster, die Bildschirmauflösung darstellt.")

Dieses Handbuch zeigt das Arbeiten mit CocosSharp um Spiele zu entwickeln, die auf Geräten von unterschiedlichen Auflösungen korrekt angezeigt.

## <a name="cocossharp-content-pipelinegraphics-gamescocossharpcontent-pipelineindexmd"></a>[CocosSharp-Inhaltspipeline](~/graphics-games/cocossharp/content-pipeline/index.md)

![XNB](images/content-pipeline.png "XNB")

Inhaltspipelines werden häufig in der Entwicklung von Spielen verwendet, zum Optimieren von Inhalten und formatieren Sie ihn so, dass sie auf bestimmte Hardware oder mit bestimmten Frameworks Spieleentwicklung geladen werden können.

## <a name="improving-frame-rate-with-ccspritesheetgraphics-gamescocossharpccspritesheetmd"></a>[Verbessern der Framerate mit CCSpriteSheet](~/graphics-games/cocossharp/ccspritesheet.md)

![Eine Struktur aus einem CCSpriteSheet](images/ccspritesheet.png "eine Struktur aus einem CCSpriteSheet")

`CCSpriteSheet` Stellt Funktionen für viele Bilddateien in einer Textur verwendet und kombiniert. Anzahl der Textur verringern kann Ladezeiten des Spiels und der Bildrate verbessern.

## <a name="texture-caching-using-cctexturecachegraphics-gamescocossharptexture-cachemd"></a>[Zwischenspeichern von Texturen mit CCTextureCache](~/graphics-games/cocossharp/texture-cache.md)

![Eine Darstellung wie Bilder von CocosSharp zwischenspeichert](images/texture-cache.png "eine Darstellung wie Bilder von CocosSharp zwischenspeichert")

CocosSharp `CCTextureCache` Klasse bietet eine standardisierte Möglichkeit zum Organisieren, Zwischenspeichern und Entladen von Inhalt. 

## <a name="2d-math-with-cocossharpgraphics-gamescocossharpmathmd"></a>[2D-Mathematik mit CocosSharp](~/graphics-games/cocossharp/math.md)

![Ein Bild, das gedreht](images/math.png "ein Bild, das gedreht")

Dieser Leitfaden behandelt 2D Mathematik für die Entwicklung von Spielen. Verwendet von CocosSharp veranschaulichen, wie Sie allgemeine Aufgaben zur Entwicklung von Spielen und Mathematik diese Aufgaben erläutert.

## <a name="performance-and-visual-effects-with-ccrendertexturegraphics-gamescocossharpccrendertexturemd"></a>[Leistung und visuelle Effekte mit CCRenderTexture](~/graphics-games/cocossharp/ccrendertexture.md)

![Ein Sprite aus einem Spiel](images/ccrendertexture.png "ein Sprite aus einem Spiel")

Die `CCRenderTexture` -Klasse enthält Funktionen für das Rendern mehrerer CocosSharp-Objekte, um eine einzelne Textur. Nach der Erstellung `CCRenderTexture` Instanzen können verwendet werden, um effizientes Rendering der Grafiken und visuelle Effekte zu implementieren.
