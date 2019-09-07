---
title: Xamarin. Mac-Vorkompilierung vor der Zeit
description: In diesem Dokument wird die vorab Kompilierung in xamarin. Mac beschrieben. Es vergleicht die AOT-Kompilierung mit der JIT-Kompilierung, erläutert, wie AOT aktiviert wird, und zeigt einen Einblick in Hybrid-AOT.
ms.prod: xamarin
ms.assetid: 38B8A017-5A58-429C-A6E9-9860A1DCEF63
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 11/10/2017
ms.openlocfilehash: 6797428596fddb0361fb307240bf8237a1e8554d
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769832"
---
# <a name="xamarinmac-ahead-of-time-compilation"></a>Xamarin. Mac-Vorkompilierung vor der Zeit

## <a name="overview"></a>Übersicht

Die AOT-Kompilierung (Ahead of Time) ist eine leistungsstarke Optimierungsmethode zum Verbessern der Startleistung. Dies wirkt sich jedoch auf die Buildzeit, die Anwendungs Größe und die Programmausführung aus. Um die vor-und Nachteile zu verstehen, werden wir uns ein wenig mit der Kompilierung und Ausführung einer Anwendung beschäftigen.

In verwalteten Sprachen geschriebene Codes, wie z C# . F#b. und, werden in eine zwischen Darstellung namens IL kompiliert. Diese Il, die in der Bibliothek und den programmassemblys gespeichert ist, ist relativ kompakt und portabel zwischen Prozessorarchitekturen. Il ist jedoch nur ein Zwischensatz von Anweisungen, und zu einem bestimmten Zeitpunkt muss Il in Computercode übersetzt werden, der für den Prozessor spezifisch ist.

Es gibt zwei Punkte, an denen diese Verarbeitung ausgeführt werden kann:

- **Just-in-time-– (Just-in-Time)** während des Starts und der Ausführung der Anwendung wird der IL-Code im Arbeitsspeicher für den Computer
- **Ahead-of-Time (AOT)** – während des Builds wird die IL kompiliert und in Native Bibliotheken geschrieben und in Ihrem Anwendungs Bündel gespeichert.

Jede Option bietet eine Reihe von Vorteilen und Kompromisse:

- **JIT**
  - **Startzeit** – die JIT-Kompilierung muss beim Start ausgeführt werden. Bei den meisten Anwendungen liegt dieser Wert in der Reihenfolge von 100 MS, aber für große Anwendungen kann diese Zeit deutlich mehr sein.
  - **Ausführung** – da der JIT-Code für den verwendeten Prozessor optimiert werden kann, kann etwas besserer Code generiert werden. In den meisten Anwendungen sind dies höchstens einige Prozentpunkte schneller.
- **AOT**
  - **Startzeit** – das Laden vorkompilierter dylisb ist erheblich schneller als JIT-Assemblys.
  - **Speicherplatz** – diese dylisb können einen erheblichen Speicherplatz beanspruchen. Je nachdem, welche Assemblys erstellt werden, kann die Größe des Codeteils der Anwendung doppelt oder mehr betragen.
  - **Buildzeit** – die AOT-Kompilierung ist erheblich langsamer als die JIT-Kompilierung und verlangsamt Builds mit der Anwendung. Diese Verlangsamung kann je nach Größe und Anzahl der kompilierten Assemblys von Sekunden bis zu einer Minute liegen.
  - **Obfuskations** – da die Il, die wesentlich einfacher umzukehren ist als Computercode, nicht unbedingt erforderlich ist, ist Sie nicht unbedingt erforderlich, um den sensiblen Code zu verbergen. Hierfür ist die Option "Hybrid" erforderlich.

## <a name="enabling-aot"></a>Aktivieren von AOT

In einem zukünftigen Update werden dem Mac-Buildbereich AOT-Optionen hinzugefügt. Bis dahin erfordert die Aktivierung von AOT das Übergeben eines Befehlszeilen Arguments über das Feld "zusätzliche MMP-Argumente" im Mac-Build. Dies sind die Optionen:

```
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
```

## <a name="hybrid-aot"></a>Hybrid-AOT

Während der Ausführung einer macOS-Anwendung verwendet die Laufzeit standardmäßig Computercode, der aus den von der AOT-Kompilierung erstellten nativen Bibliotheken geladen wurde. Es gibt jedoch einige Code Bereiche, wie z. b. Trampoline, bei denen die JIT-Kompilierung erheblich optimierte Ergebnisse liefern kann. Hierfür ist es erforderlich, dass die verwalteten Assemblys Il verfügbar sind. Unter IOS sind Anwendungen von der Verwendung der JIT-Kompilierung eingeschränkt; Diese Code Abschnitte werden ebenfalls mit AOT kompiliert.

Die Hybrid-Option weist den Compiler an, diesen Abschnitt (z. b. IOS) zu kompilieren, wobei jedoch davon ausgegangen wird, dass die Il zur Laufzeit nicht verfügbar ist. Diese Il kann dann nach dem Build entfernt werden. Wie bereits erwähnt, wird die Laufzeit gezwungen, weniger optimierte Routinen an manchen Stellen zu verwenden.

## <a name="further-considerations"></a>Weitere Überlegungen

Die negativen Konsequenzen der AOT-Skalierung in Bezug auf die Größe und Anzahl der verarbeiteten Assemblys. Das vollständige [Ziel Framework](~/mac/platform/target-framework.md) enthält beispielsweise eine deutlich größere Basisklassen Bibliothek (Base Class Library, BCL) als modern. Folglich nimmt AOT erheblich mehr Zeit in Anspruch und erzeugt größere Bündel. Dies wird durch die Inkompatibilität des vollständigen Ziel Frameworks mit der Verknüpfung verstärkt, wodurch nicht verwendeter Code entfernt wird. Erwägen Sie, die Anwendung auf die moderne zu verschieben und die Verknüpfung für die besten Ergebnisse zu ermöglichen.

Ein zusätzlicher Vorteil von AOT bietet verbesserte Interaktionen mit nativen Debugging-und Profil Erstellungs Toolketten. Da eine große Mehrheit der Codebasis im Voraus kompiliert wird, enthält Sie Funktionsnamen und Symbole, die in nativen Absturzberichten, Profilerstellung und Debuggen leichter lesbar sind. JIT-generierte Funktionen haben diese Namen nicht und werden oft als unbenannte Hex-Offsets angezeigt, die sehr schwer zu beheben sind.
