---
title: Xamarin.Mac vor der Time-Kompilierung
description: Dieses Dokument beschreibt einen Schritt voraus Time-Kompilierung in Xamarin.Mac. Vergleicht die AOT-Kompilierung JIT-Kompilierung, erläutert das Aktivieren der AOT und wirft einen Blick auf hybrides AOT.
ms.prod: xamarin
ms.assetid: 38B8A017-5A58-429C-A6E9-9860A1DCEF63
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 11/10/2017
ms.openlocfilehash: e155a394afd68d9970ee32785f6d0aeda6e2d129
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61034203"
---
# <a name="xamarinmac-ahead-of-time-compilation"></a>Xamarin.Mac vor der Time-Kompilierung

## <a name="overview"></a>Übersicht

Zeit (AOT) ist jetzt Kompilierung eine leistungsstarke Optimierungstechnik, die auch zur Verbesserung der Leistung beim Start an. Allerdings wirkt sich auf sie auch Ihre Buildzeit, Anwendungsgröße und Ausführung des Programms in fundierter Weise. Um die vor-und Nachteile zu verstehen, die erzwingt, werden wir ein bisschen tiefer in die Kompilierung und Ausführung einer Anwendung.

Code in der verwalteten Sprachen, wie z. B. C# und F#, um eine zwischendarstellung namens IL kompiliert wird. Diese IL, gespeichert in Ihre Assemblys-Bibliothek und das Programm ist relativ compact und zwischen Prozessorarchitekturen übertragbar. IL, ist jedoch nur eine mittlere festgelegt wird, Anweisungen und an einem bestimmten Punkt, die IL in Computercode, die spezifisch für den Prozessor übersetzt werden muss.

Es gibt zwei Punkten, die in denen diese Verarbeitung erfolgen kann:

- **Just-in-Time (JIT)** : beim Starten und die Ausführung Ihrer Anwendung, die der IL-Code im Arbeitsspeicher in Computercode kompiliert wird.
- **Zeit (AOT) Ahead** – während des Buildvorgangs der IL-Code wird kompiliert und in systemeigene Bibliotheken geschrieben und in Ihr Anwendungspaket gespeichert.

Jede Option verfügt über eine Anzahl von vor- und Nachteile:

- **JIT**
  - **Dauer des verbindungsstarts** – JIT-Kompilierung muss beim Start ausgeführt werden. Für die meisten Anwendungen ist dies der Größenordnung von 100 ms, aber für große Anwendungen kann diese Zeit deutlich mehr sein.
  - **Ausführung** – wie der JIT-Code kann optimiert werden, für den bestimmten Prozessor verwendet wird, kann etwas besserer Code generiert werden. In den meisten Fällen ist dies schneller wenigen Prozentpunkten höchstens.
- **AOT**
  - **Dauer des verbindungsstarts** : Laden von vorkompilierten Dylibs ist bedeutend schneller als JIT-Assemblys.
  - **Speicherplatz** – diese Dylibs kann jedoch eine erhebliche Menge an Speicherplatz dauern. Je nachdem welche Assemblys AOTed befinden, können sie doppelklicken oder mehr die Größe des Teils Code Ihrer Anwendung.
  - **Buildzeit** – AOT-Kompilierung ist erheblich langsamer, JIT-Kompilierung und wird beeinträchtigt, Builds, die es verwenden. Diese Verlangsamung reichen von Sekunden bis zu eine Minute oder mehr, je nach Größe und Anzahl der Assemblys kompiliert.
  - **Obfuskation** – als den IL-Code, der ist wesentlich einfacher, als Computercode zurückentwickeln, ist nicht unbedingt erforderlich, sie entfernt werden kann, um Hilfe von vertraulichem Code zu verbergen. Dies erfordert die "Hybrid" die Option unten beschrieben.

## <a name="enabling-aot"></a>AOT aktivieren

AOT-Optionen werden in den Mac-Build-Bereich in einem späteren Update hinzugefügt. Bis dahin erfordert die Aktivierung von AOT, übergeben ein Befehlszeilenargument über das Feld "zusätzlichen Mmp-Argumenten" in der Mac-Build. Dies sind die Optionen:


      --aot[=VALUE]          Specify assemblies that should be AOT compiled
                               - none - No AOT (default)
                               - all - Every assembly in MonoBundle
                               - core - Xamarin.Mac, System, mscorlib
                               - sdk - Xamarin.Mac.dll and BCL assemblies
                               - |hybrid after option enables hybrid AOT which
                               allows IL stripping but is slower (only valid
                               for 'all')
                                - Individual files can be included for AOT via +
                               FileName.dll and excluded via -FileName.dll

                               Examples:
                                 --aot:all,-MyAssembly.dll
                                 --aot:core,+MyOtherAssembly.dll,-mscorlib.dll



## <a name="hybrid-aot"></a>Hybrides AOT

Aus der erzeugten AOT-Kompilierung nativen Bibliotheken geladen verwendet standardmäßig Computercode, während der Ausführung einer MacOS-Anwendung die Common Language Runtime. Es gibt jedoch einige Bereiche des Codes, wie z. B. Trampolines, in dem JIT-Kompilierung auf deutlich optimierte Ergebnissen führen können. Dies erfordert die IL verwaltete Assemblys verfügbar sein. Unter iOS sind die Anwendungen von der Verwendung der JIT-Kompilierung ausgeschlossen; Dieser Abschnitt des Codes sind per AOT kompiliert auch.

Die Hybrid-Option weist den Compiler an beide kompilieren diesen Abschnitt (wie iOS) auf, aber darüber hinaus, vorausgesetzt, dass die IL nicht zur Laufzeit zur Verfügung stehen. Diese IL dann entfernt werden kann nach dem Build. Wie oben erwähnt, wird die Laufzeit erzwungen werden, um weniger optimierte Routinen an einigen Stellen verwenden.

## <a name="further-considerations"></a>Weitere Überlegungen

Die negativen Konsequenzen der AOT-Skalierungsgruppe mit der Größe und Anzahl der Assemblys, die verarbeitet werden soll. Die vollständige [Zielframework](~/mac/platform/target-framework.md) für Beispiel eine erheblich größere Basisklassenbibliothek (BCL) als Modern enthält, und daher AOT erheblich länger dauern wird, und erzeugen größere Pakete. Dies wird verschärft, durch das vollständige Zielframework Inkompatibilität mit verknüpfen, die nicht verwendeter Code entfernt. Sollten Sie Ihre Anwendung zum Verknüpfen von modernen und aktivieren die besten Ergebnisse zu verschieben.

Ein weiterer Vorteil der AOT enthält verbesserte Interaktionen mit native Debuggen und profilerstellung toolketten. Da ein Großteil der Codebasis vorab kompiliert wird, müssen sie Funktions-als auch Symbole, die einfacher zu lesen in systemeigenen Absturzberichte, profilerstellung und Debuggen sind. JIT-Kompilierung generierten Funktionen nicht diese Namen haben, und zeigen sich oft als unbenannte hex Offsets, die sehr schwer zu beheben sind.
