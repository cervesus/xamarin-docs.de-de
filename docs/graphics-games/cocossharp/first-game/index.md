---
title: "Einführung in die 3D-Spielentwicklung mit CocosSharp"
description: "Mehrteilige in dieser exemplarischen Vorgehensweise wird das Erstellen eines einfachen 2D Spiels mit CocosSharp veranschaulicht. Es werden allgemeine Spiel Programmierkonzepte erläutert, wie z. B. Grafiken, die Eingabe und die physikalische behandelt."
ms.topic: article
ms.prod: xamarin
ms.assetid: BCA99A61-A48D-4207-9A35-4C62F10C9AA5
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 3b1bb45ab87c85dff42b4f7ea5297eb3596b81a5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-game-development-with-cocossharp"></a>Einführung in die 3D-Spielentwicklung mit CocosSharp

_Mehrteilige in dieser exemplarischen Vorgehensweise wird das Erstellen eines einfachen 2D Spiels mit CocosSharp veranschaulicht. Es werden allgemeine Spiel Programmierkonzepte erläutert, wie z. B. Grafiken, die Eingabe und die physikalische behandelt._

Die CocosSharp 2D Spiele-Engine bietet Technologie für die plattformübergreifende Spiele vornehmen. Eine vollständige Liste der unterstützten Plattformen finden Sie unter der [CocosSharp-Wiki auf GitHub](https://github.com/mono/CocosSharp/wiki). In diesem Lernprogramm wird C#-Codebeispiele für verwendet zwar CocosSharp ebenfalls mit f# voll funktionsfähig ist.

Der Kern der CocosSharp wird bereitgestellt, indem die [MonoGame Framework](http://www.monogame.net/), dies ist selbst ein plattformübergreifendes, Hardware accelerated API Grafiken, audio, Spiel Zustandsverwaltung, Eingabe und eine Content Pipeline zum Importieren von Ressourcen bereitstellen. CocosSharp ist eine effiziente Abstraktionsebene für 2D Spiele geeignet. Darüber hinaus können größere Spiele eigene Optimierungen außerhalb ihrer Core-Bibliotheken ausführen, wenn Spiele Komplexität wächst. CocosSharp bietet also eine Mischung aus einfachen Verwendung und Leistung zu erzielen, und Entwickler können schnell ohne Spiel Größe und Komplexität beginnen.

Der erste Teil dieser exemplarischen Vorgehensweise den Fokus auf ein leeres Projekt einrichten.  Der zweite Teil deckt alle unsere Spiellogik schreiben. 

Am Ende dieser exemplarischen Vorgehensweise wird es ein einfaches Spiel erstellt haben, in denen die Player-Ziel ist es in den Hintergrund einer Paddel horizontal um zu verhindern, dass eine Kugel zurück, die sich außerhalb des Bildschirms. Jede Bounce erhöht der Spieler Ergebnis durch einen Punkt.

![](images/image1.png "Jede Bounce erhöht der Spieler Ergebnis durch einen Punkt")

# <a name="walkthrough-parts"></a>Exemplarische Vorgehensweise Teile

* [Teil 1 – Erstellen eines Projekts CocosSharp](~/graphics-games/cocossharp/first-game/part1.md)
* [Teil 2 – Implementieren der BouncingGame](~/graphics-games/cocossharp/first-game/part2.md)

## <a name="related-links"></a>Verwandte Links

- [Inhalt (Beispiel)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Content.zip?raw=true)
- [Abgeschlossene Projekt (Beispiel)](https://developer.xamarin.com/samples/mobile/BouncingGame/)
- [CocosSharp PCL für NuGet](http://www.nuget.org/packages/CocosSharp.PCL.Shared/)
- [CocosSharp API-Dokumentation](http://developer.xamarin.comhttps://developer.xamarin.com/api/namespace/CocosSharp/)
