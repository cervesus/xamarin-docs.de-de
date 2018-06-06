---
title: Xamarin.Mac vor der Kompilierung
description: Dieses Dokument beschreibt vor der Kompilierung in Xamarin.Mac. Es vergleicht AOT Kompilierung auf JIT-Kompilierung wird erläutert, wie AOT aktivieren und untersucht, mit Hybrid AOT.
ms.prod: xamarin
ms.assetid: 38B8A017-5A58-429C-A6E9-9860A1DCEF63
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: ec8474293fbb7372529e0f850e2d16db7ebf17be
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792238"
---
# <a name="xamarinmac-ahead-of-time-compilation"></a>Xamarin.Mac vor der Kompilierung

## <a name="overview"></a>Überblick

Zeit (AOT) ist im Voraus Kompilierung eine leistungsstarke Optimierungstechnik, die auch zur Verbesserung der Leistung beim Start an. Allerdings wirkt Sie sich auf auch die Buildzeit, Anwendungsgröße und Ausführung des Programms weitreichende Möglichkeiten. Um die Kompromisse zu verstehen, erzwingt, werden wir ein wenig mit der Kompilierung und Ausführung einer Anwendung beschäftigen.

Geschrieben in anderen verwalteten Sprachen, z. B. c# und F#-Code wird in einen intermediate Darstellung aufgerufen IL kompiliert. Diese IL, in der Bibliothek und Programm Assemblys gespeichert, ist relativ kompakt und portable zwischen Prozessorarchitekturen. IL, ist jedoch nur eine mittlere Festlegen von Anweisungen und irgendwann, die IL in Computercode, die spezifisch für den Prozessor übersetzt werden muss.

Es gibt zwei Punkten, die in denen diese Verarbeitung ausgeführt werden kann:

- **Just-in-Time (JIT)** – beim Starten und die Ausführung der Anwendung die IL im Arbeitsspeicher in Computercode kompiliert wird.
- **Zeit (AOT) Ahead** – während des Buildvorgangs die IL kompiliert und in systemeigene Bibliotheken geschrieben und in Ihrer Anwendungspaket gespeichert.

Jede Option hat eine Anzahl von vor- und Nachteile:

- **JIT**
  - **Startzeit** – JIT-Kompilierung muss beim Start ausgeführt werden. Für die meisten Anwendungen ist dies der Größenordnung von 100 ms, aber für große Anwendungen kann diese Zeit erheblich mehr sein.
  - **Ausführung** – wie der JIT-Code kann optimiert werden, für den bestimmten Prozessor verwendet wird, kann geringfügig besseren Code generiert werden. In den meisten Anwendungen ist dies wenige Prozentpunkte schneller höchstens.
- **AOT**
  - **Startzeit** – laden vorkompilierter Dylibs ist bedeutend schneller als JIT-Assemblys.
  - **Speicherplatz** – diese Dylibs kann jedoch sehr viel Speicherplatz dauern. Doppelklicken sie je nachdem welche Assemblys AOTed befinden, oder mehr der Größe des Anteils Code Ihrer Anwendung.
  - **Zeit der Erstellung** – AOT Kompilierung ist erheblich langsamer, JIT und Builds verwenden, beeinträchtigt wird. Diese Verlangsamung reichen von Sekunden bis zu einer Minute oder mehr, abhängig von der Größe und Anzahl der Assemblys kompiliert.
  - **Verbergen** – als den IL-Code, der wesentlich einfacher als Computercode zurückentwickeln, ist nicht unbedingt erforderlich, können entfernt werden, um Hilfe sicherheitsrelevanten Code zu verbergen. Dies erfordert "Hybrider"-Option werden unten beschrieben.

## <a name="enabling-aot"></a>Aktivieren der AOT

AOT Optionen werden die Mac-Buildbereich in einem zukünftigen Update hinzugefügt werden. Bis dahin müssen Sie zur Aktivierung AOT Befehlszeilenargument über das Feld "zusätzliche Mmp Argumente" in Build Mac übergeben. Dies sind die Optionen:


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



## <a name="hybrid-aot"></a>Hybride AOT

Laden während der Ausführung einer Anwendung MacOS die Laufzeit standardmäßig Computercode von den systemeigenen Bibliotheken von AOT Kompilierung erzeugt. Es gibt jedoch einige Bereiche des Codes z. B. Trampolines, wobei JIT-Kompilierung erheblich mehr optimierte Ergebnissen führen kann. Dies erfordert die IL verwaltete Assemblys verfügbar sein. Bei iOS kann sind Anwendungen von der Verwendung der JIT-Kompilierung ausgeschlossen; Dieser Abschnitt des Codes sind AOT ebenfalls kompiliert.

Die Hybrid-Option weist den Compiler an beide kompilieren diese Abschnitt (z. B. iOS) auf, jedoch darüber hinaus, vorausgesetzt, dass die IL nicht zur Laufzeit zur Verfügung stehen. Diese IL kann dann entfernt Build bereitstellen. Wie oben bereits erwähnt, wird die Common Language Runtime erzwungen werden, verwenden Sie weniger optimierte Routinen an einigen stellen.

## <a name="further-considerations"></a>Weitere Überlegungen

Die negativen Auswirkungen einer unbestimmten AOT skalieren die Größe und Anzahl der verarbeiteten Assemblys. Die vollständige [Zielframework](~/mac/platform/target-framework.md) Beispiel enthält eine erheblich größere Basisklassenbibliothek (BCL) als Modern und daher AOT erheblich länger dauern und größere Pakete erzeugen. Dies wird verschärft, durch das vollständige Zielframework Inkompatibilität mit verknüpfen, die nicht verwendeten Code entfernt. Erwägen Sie Ihre Anwendung in modernen und aktivieren verknüpfen, für die besten Ergebnisse.

Ein weiterer Vorteil beim AOT bietet verbesserte Interaktionen mit systemeigenen Debuggen und profilerstellung Toolchains. Da ein Großteil der Codebasis voraus kompiliert wird, müssen diese Funktionsnamen und Symbole, die einfacher zu lesen in systemeigenen Absturzberichte, profilerstellung und debugging. Generiert JIT-Funktionen haben diese Namen nicht und häufig als unbenannte hex Offsets, die sehr schwer zu beheben sind angezeigt.
