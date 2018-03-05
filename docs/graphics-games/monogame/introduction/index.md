---
title: "Einführung in die 3D-Spielentwicklung mit MonoGame"
description: "Mehrteilige in dieser exemplarischen Vorgehensweise zeigt, wie MonoGame mithilfe eine einfache 2D-Anwendung zu erstellen.  Dazu gehören allgemeine Spiel Programmierkonzepte erläutert, wie z. B. Grafiken, die Eingabe Spiel, Entitäten und physikalische."
ms.topic: article
ms.prod: xamarin
ms.assetid: D781401F-7A96-4098-9645-5F98AEAF7F71
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 8161920139413cd1b28adcebaf56e6d8265120ef
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-game-development-with-monogame"></a>Einführung in die 3D-Spielentwicklung mit MonoGame

_Mehrteilige in dieser exemplarischen Vorgehensweise zeigt, wie MonoGame mithilfe eine einfache 2D-Anwendung zu erstellen.  Dazu gehören allgemeine Spiel Programmierkonzepte erläutert, wie z. B. Grafiken, die Eingabe Spiel, Entitäten und physikalische._

Dieser Artikel beschreibt MonoGame-API-Technologie für die plattformübergreifende Spiele vornehmen. Eine vollständige Liste der Plattformen, finden Sie unter der [MonoGame Website](http://www.monogame.net/). In diesem Lernprogramm wird C#-Codebeispiele für verwendet zwar MonoGame ebenfalls mit f# voll funktionsfähig ist.

MonoGame ist eine plattformübergreifende, Hardware accelerated API Grafiken, audio, Spiel Zustandsverwaltung, Eingabe und eine Content Pipeline zum Importieren von Ressourcen bereitstellen. Im Gegensatz zu den meisten game-Module MonoGame nicht bereitstellen oder jede Struktur Muster oder ein Projekt zu erzwingen.  Dies bedeutet, dass Entwickler frei, um ihren Code zu organisieren, wie sie sind, bedeutet dies auch, dass einem gewissen Grad Setupcode benötigt wird, wenn zunächst ein neues Projekt zu starten.

Der erste Teil dieser exemplarischen Vorgehensweise konzentriert sich auf ein leeres Projekt einrichten. Der letzte Abschnitt behandelt, Schreiben alle unsere Spiellogik und den Inhalt – am häufigsten die plattformübergreifende sein wird.

Am Ende dieser exemplarischen Vorgehensweise wird es ein einfaches Spiel erstellt haben, in dem der Spieler ein animiertes Zeichen für die Fingereingabe steuern können.  Dies ist, zwar kein technisch eines vollständigen Spiels (da er keine gewinnen oder verlieren Bedingungen hat) veranschaulicht zahlreiche 3D-Spielentwicklung Konzepte und kann als Grundlage für viele Arten von Spiele verwendet werden. 

Das folgende Beispiel zeigt das Ergebnis dieser exemplarischen Vorgehensweise aus:

![](images/image1.gif "Die app, die in der exemplarischen Vorgehensweise erstellt werden")

# <a name="monogame-and-xna"></a>Monogame und XNA

Die Bibliothek MonoGame dient Microsoft XNA-Bibliothek in der Syntax und Funktionalität zu imitieren.  Alle MonoGame-Objekte, die unter dem Microsoft.Xna-Namespace – da die meisten XNA-Code in MonoGame verwendet werden, ohne weitere Änderungen vorhanden sind. 

Entwickler, die mit XNA vertraut bereits mit MonoGames-Syntax vertraut sein, und Entwickler zusätzliche Informationen zum Arbeiten mit MonoGame suchen werden in der Lage, vorhandene online XNA Exemplarische Vorgehensweisen, API-Dokumentation und Diskussionen zu verweisen.


# <a name="walkthrough-parts"></a>Exemplarische Vorgehensweise Teile

- [Teil 1 – Erstellen eines plattformübergreifenden MonoGame-Projekts](~/graphics-games/monogame/introduction/part1.md)
- [Teil 2 – Implementieren der WalkingGame](~/graphics-games/monogame/introduction/part2.md)

## <a name="related-links"></a>Verwandte Links

- [WalkingGame MonoGame Projekt (Beispiel)](https://developer.xamarin.com/samples/mobile/WalkingGameMG/)
- [XNB Schriftarten iOS](https://github.com/mono/CocosSharp/tree/master/Samples/GameStarterKit/GameStarterKit/Content/fonts)
- [XNB Schriftarten Android](https://github.com/mono/CocosSharp/tree/master/Samples/GameStarterKit/GameStarterKit/Assets/Content/fonts)
- [MonoGame Android für NuGet](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [MonoGame iOS für NuGet](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [MonoGame API-Dokumentation](http://www.monogame.net/documentation/?page=main)
