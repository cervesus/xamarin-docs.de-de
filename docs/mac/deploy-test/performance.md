---
title: Xamarin.Mac-Leistung
description: Dieses Dokument enthält einige Überlegungen zur Leistung von Xamarin.Mac-Apps. Dabei werden das moderne Zielframework, der Linker, AOT, Delegaten, Cocoa-APIs für das Wiederverwenden von Ansichten und asynchroner Code erläutert.
ms.prod: xamarin
ms.assetid: 54B07DED-FDF2-49B2-A5FB-3A9357E65922
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 11/10/2017
ms.openlocfilehash: a5a759ae9f156eec71706d9681fac2a94995848e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021675"
---
# <a name="xamarinmac-performance"></a>Xamarin.Mac-Leistung

## <a name="overview"></a>Übersicht

Xamarin.Mac-Anwendung ähneln Xamarin.iOS-Anwendungen. Daher gelten ähnliche Leistungsempfehlungen für beide Anwendungsarten:

- [Xamarin.iOS-Leistung](~/ios/deploy-test/performance.md)
- [Plattformübergreifende Leistung](~/cross-platform/deploy-test/memory-perf-best-practices.md)

Es gibt allerdings auch einige nützliche Empfehlungen, die nur für macOS gelten.

## <a name="prefer-modern-target-framework"></a>Bevorzugen des Zielframeworks „Modern“

Es stehen mehrere [Zielframeworks](~/mac/platform/target-framework.md) für Xamarin.Mac-Anwendungen mit unterschiedlichen Merkmalen und Features zur Verfügung.

Ziehen Sie wenn möglich immer das Framework „Modern“ vor, und arbeiten Sie mit abhängigen Bibliotheken, um die Unterstützung zu erhöhen. Nur das Zielframework „Modern“ lässt Verknüpfungen zu, wodurch Ihre Assemblygrößen drastisch reduziert werden können. Dies wird bei der Aktivierung von AOT besonders wichtig, das die AOT-Kompilierung (Ahead-of-time-Kompilierung) vollständiger Assemblys zu großen endgültigen Bündeln führen kann.

## <a name="enable-the-linker"></a>Aktivieren des Linkers

Die Startzeit wird sowohl beim Laden als auch bei der JIT-Kompilierung (Just-In-Time) ein Stück weit linear mit der Größe Ihrer endgültigen Binärdateien skaliert. Dies können Sie am einfachsten optimieren, indem Sie toten Code mit dem [Linker](~/mac/deploy-test/linker.md) entfernen.

Während diese Empfehlung hauptsächlich für Benutzer des Zielframeworks „Modern“ gilt, kann die [Plattformverknüpfung](~/mac/deploy-test/linker.md) die Leistung ebenfalls geringfügig erhöhen.

## <a name="enable-aot-when-appropriate"></a>Aktivieren der AOT-Kompilierung (falls angemessen)

Ein weiterer Faktor der Startleistung ist die JIT-Kompilierung von Assemblys in Computercode. Die Ahead-of-time-Kompilierung (AOT) kann die Startzeit deutlich senken, führt aber auch zu Einbußen. Informationen dazu finden Sie in der [AOT-Dokumentation](~/mac/internals/aot.md).

## <a name="ensure-performant-delegates"></a>Bereitstellen leistungsfähiger Delegaten

Viele Xamarin.Mac-Anwendungen bauen auf Cocoa-Ansichten wie etwa `NSCollectionView`, `NSOutlineView` oder `NSTableView` auf. Diese Ansichten werden häufig durch die Klassen `Delegate` und `DataSource` unterstützt, die Sie für Cocoa bereitstellen. Dadurch wird die Anzeige bestimmt.

Viele dieser Einstiegspunkte werden häufig aufgerufen, beim Scrollen manchmal mehrmals in der Sekunde.

Achten Sie darauf, dass diese Funktionen Werte zurückgeben, die leicht zu berechnen sind oder die zwischengespeicherte Informationen verwenden, um ein Blockieren der Benutzeroberfläche zu verhindern.

## <a name="use-cocoa-provided-apis-for-reusing-views"></a>Verwenden von Cocoa-APIs für das Wiederverwenden von Ansichten

Viele Cocoa-Ansichten, die viele untergeordnete Ansichten oder Zellen beinhalten (wie z.B. `NSCollectionView`, `NSOutlineView` und `NSTableView`) bieten APIs zum Erstellen und Wiederverwenden von Ansichten. Diese erstellen Pools aus gemeinsam verwendeten Elementen und verhindern Leistungsprobleme beim schnellen Scrollen durch Ansichten.

## <a name="use-async-and-do-not-block-the-ui"></a>Verwenden von „Async“ und Vermeiden der Blockierung der Benutzeroberfläche

Desktopanwendungen verarbeiten häufig große Datenmengen. Daher ist es sehr leicht, den UI-Thread durch das Warten auf einen synchronen Vorgang zu blockieren.

Verwenden Sie deshalb wenn möglich [async](~/cross-platform/platform/async.md) und Threads, um das Blockieren der UI zu verhindern.

Ziehen Sie für lange Vorgänge [NSProgressIndicator](https://docs.microsoft.com/samples/xamarin/mac-samples/progressbarexample) oder andere in den [Benutzeroberflächen-Richtlinien](https://developer.apple.com/macos/human-interface-guidelines/indicators/progress-indicators/) von Apple erklärte Optionen in Betracht, um Benutzer zu benachrichtigen.

## <a name="related-links"></a>Verwandte Links

- [Plattformübergreifende Leistung](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [Xamarin.iOS-Leistung](~/ios/deploy-test/performance.md)
