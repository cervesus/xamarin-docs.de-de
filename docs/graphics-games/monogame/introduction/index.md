---
title: Einführung in die Spieleentwicklung mit monogame
description: In dieser mehrteiligen exemplarischen Vorgehensweise wird gezeigt, wie eine einfache 2D-Anwendung mithilfe von monogame erstellt wird.  Es behandelt gängige Konzepte der Spielprogrammierung, wie z. b. Grafiken, Eingaben, Spiel Entitäten und Physik.
ms.prod: xamarin
ms.assetid: D781401F-7A96-4098-9645-5F98AEAF7F71
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 47ed7fc1b4485864646a17940aceed395a4a8983
ms.sourcegitcommit: f255aa286bd52e8a80ffa620c2e93c97f069f8ec
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2019
ms.locfileid: "68680908"
---
# <a name="introduction-to-game-development-with-monogame"></a>Einführung in die Spieleentwicklung mit monogame

_In dieser mehrteiligen exemplarischen Vorgehensweise wird gezeigt, wie eine einfache 2D-Anwendung mithilfe von monogame erstellt wird.  Es behandelt gängige Konzepte der Spielprogrammierung, wie z. b. Grafiken, Eingaben, Spiel Entitäten und Physik._

In diesem Artikel wird die monogame-API-Technologie zum Erstellen von plattformübergreifenden spielen beschrieben. Eine vollständige Liste der Plattformen finden Sie auf der [monogame-Website](http://www.monogame.net/). In diesem Tutorial wird C# für Codebeispiele verwendet, obwohl monogame auch vollständig F# mit funktionsfähig ist.

Monogame ist eine plattformübergreifende, hardwarebeschleunigte API zur Bereitstellung von Grafiken, Audioeingaben, Spiele Zustands Verwaltung, Eingabe und einer Inhalts Pipeline zum Importieren von Assets. Im Gegensatz zu den meisten Spiel-Engines stellt monogame keine Muster-oder Projektstruktur bereit.  Dies bedeutet, dass Entwickler den Code so organisieren können, wie Sie möchten. Dies bedeutet auch, dass beim ersten Starten eines neuen Projekts ein wenig Setup Code benötigt wird.

Im ersten Abschnitt dieser exemplarischen Vorgehensweise wird das Einrichten eines leeren Projekts behandelt. Im letzten Abschnitt wird das Schreiben all unserer Spiellogik und ihrer Inhalte erläutert – die meisten sind plattformübergreifend.

Am Ende dieser exemplarischen Vorgehensweise haben wir ein einfaches Spiel erstellt, in dem der Player ein animiertes Zeichen mit Berührungs Eingaben steuern kann.  Obwohl es sich hierbei nicht um ein vollständiges Spiel handelt (da es keine Gewinn-oder Verlust Bedingungen hat), werden zahlreiche Konzepte der Spielentwicklung veranschaulicht, die als Grundlage für viele Arten von Spielen verwendet werden können. 

Im folgenden wird das Ergebnis dieser exemplarischen Vorgehensweise gezeigt:

![Animation von Beispiel Spiel Zeichen nach der Maus](images/image1.gif)

## <a name="monogame-and-xna"></a>Monogame und XNA

Die monogame-Bibliothek ist dazu gedacht, die XNA-Bibliothek von Microsoft in Syntax und Funktionalität zu imitieren.  Alle monogame-Objekte sind unter dem Microsoft. XNA-Namespace vorhanden – sodass der meiste XNA-Code in monogame ohne Änderung verwendet werden kann. 

Entwickler, die mit XNA vertraut sind, sind bereits mit der Syntax von monogame vertraut, und Entwickler, die nach zusätzlichen Informationen zum Arbeiten mit monogame suchen, können auf vorhandene Online-XNA-Exemplarische Vorgehensweisen, eine API-Dokumentation und Diskussionen verweisen.


## <a name="walkthrough-parts"></a>Exemplarische Vorgehensweise

- [Teil 1 – Erstellen eines plattformübergreifenden monogame-Projekts](~/graphics-games/monogame/introduction/part1.md)
- [Teil 2 – Implementieren von walkinggame](~/graphics-games/monogame/introduction/part2.md)

## <a name="related-links"></a>Verwandte Links

- [Walkinggame monogame-Projekt (Beispiel)](https://docs.microsoft.com/samples/xamarin/mobile-samples/walkinggamemg/)
- [Xnb Fonts (IOS)](https://github.com/mono/CocosSharp/tree/master/Samples/GameStarterKit/GameStarterKit/Content/fonts)
- [Xnb-Schriftarten Android](https://github.com/mono/CocosSharp/tree/master/Samples/GameStarterKit/GameStarterKit/Assets/Content/fonts)
- [Monogame Android auf nuget](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [Monogame IOS auf nuget](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [Monogame-API-Dokumentation](http://www.monogame.net/documentation/?page=main)
