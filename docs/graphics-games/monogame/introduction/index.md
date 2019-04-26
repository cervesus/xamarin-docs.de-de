---
title: Einführung in die Entwicklung von Spielen mit MonoGame
description: Mehrteilige in dieser exemplarischen Vorgehensweise veranschaulicht zum Erstellen einer einfachen Direct2D-Anwendung, die mithilfe von MonoGame.  Hierin sind allgemeine Spiel Programmierkonzepte kennen, wie z. B. Grafiken, die Eingabe von Entitäten und Physik spielen.
ms.prod: xamarin
ms.assetid: D781401F-7A96-4098-9645-5F98AEAF7F71
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 4ab98d59bc74672f9531f4dbd3c33a6270582612
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61386267"
---
# <a name="introduction-to-game-development-with-monogame"></a>Einführung in die Entwicklung von Spielen mit MonoGame

_Mehrteilige in dieser exemplarischen Vorgehensweise veranschaulicht zum Erstellen einer einfachen Direct2D-Anwendung, die mithilfe von MonoGame.  Hierin sind allgemeine Spiel Programmierkonzepte kennen, wie z. B. Grafiken, die Eingabe von Entitäten und Physik spielen._

Dieser Artikel beschreibt MonoGame-API-Technologie zum Erstellen von plattformübergreifenden spielen. Eine vollständige Liste der Plattformen, finden Sie unter den [MonoGame Website](http://www.monogame.net/). In diesem Tutorial verwenden C# Codebeispiele, obwohl MonoGame mit vollständig funktionsfähig ist F# ebenfalls.

MonoGame ist eine plattformübergreifende, Hardwarebeschleunigung-API-Grafiken, Audio- und Spiele für die Zustandsverwaltung, Eingabe und eine Pipeline für Bildinhalte zum Importieren von Ressourcen bereitstellen. Im Gegensatz zu den meisten Spiele-Engines MonoGame nicht angeben oder jede Struktur Muster oder ein Projekt zu erzwingen.  Dies bedeutet, dass Entwickler können ihren Code zu organisieren, wie sie möchten, bedeutet auch, dass ein paar Codezeilen Setup erforderlich ist, wenn zuerst ein neues Projekt beginnen.

Der erste Teil dieser exemplarischen Vorgehensweise konzentriert sich auf ein leeres Projekt einrichten. Im letzte Abschnitt behandelt das Schreiben aller unserer Spiellogik und Text – die meisten der plattformübergreifend sein wird.

Am Ende dieser exemplarischen Vorgehensweise werden wir ein einfaches Spiels erstellt haben, in dem der Spieler ein animiertes Zeichen mit Touch-Punkts steuern können.  Obwohl dies technisch gesehen eine vollständige Spiel (da es keine gewinnen oder verlieren Bedingungen hat) nicht ist, veranschaulicht zahlreiche Konzepte der Entwicklung von Spielen und dienen als Grundlage für eine Vielzahl von Spielen. 

Das folgende Beispiel zeigt das Ergebnis dieser exemplarischen Vorgehensweise aus:

![Animation der Beispiel-game-Zeichen nach der Maus](images/image1.gif)

## <a name="monogame-and-xna"></a>Monogame und XNA

Die MonoGame-Bibliothek ist vorgesehen, um die Bibliothek von Microsoft XNA in Syntax und Funktionalität zu imitieren.  Alle MonoGame-Objekte, die unter dem Microsoft.Xna-Namespace – sodass die meisten XNA-Code in MonoGame verwendet werden, ohne weitere Änderungen vorhanden sind. 

Mit XNA vertraute Entwickler bereits mit MonoGames-Syntax vertraut sein, und Entwickler, die Weitere Informationen zum Arbeiten mit MonoGame werden in der Lage, vorhandene online XNA-Anleitungen, API-Dokumentation und Diskussionen zu verweisen.


## <a name="walkthrough-parts"></a>Exemplarische Vorgehensweise Teilen

- [Teil 1 – Erstellen eines plattformübergreifenden MonoGame-Projekts](~/graphics-games/monogame/introduction/part1.md)
- [Teil 2 – Implementieren von WalkingGame](~/graphics-games/monogame/introduction/part2.md)

## <a name="related-links"></a>Verwandte Links

- [WalkingGame MonoGame-Projekts (Beispiel)](https://developer.xamarin.com/samples/mobile/WalkingGameMG/)
- [XNB Schriftarten iOS](https://github.com/mono/CocosSharp/tree/master/Samples/GameStarterKit/GameStarterKit/Content/fonts)
- [XNB Schriftarten Android](https://github.com/mono/CocosSharp/tree/master/Samples/GameStarterKit/GameStarterKit/Assets/Content/fonts)
- [MonoGame Android auf NuGet](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [MonoGame iOS auf NuGet](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [MonoGame-API-Dokumentation](http://www.monogame.net/documentation/?page=main)
