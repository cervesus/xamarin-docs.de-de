---
title: WatchOS-Plattformfeatures
description: Dieses Dokument enthält Links zu verschiedenen Leitfäden, die WatchOS-Plattformfeatures wie z. B. Apple Pay, Benachrichtigungen, Komplikationen, proaktive Vorschläge, Trainings-apps und vieles mehr zu beschreiben.
ms.prod: xamarin
ms.assetid: 13F23E01-BAED-43EB-A70E-3B30EF53D379
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/05/2018
ms.openlocfilehash: 09200ba5968838edf829b30a50a8ad0f4a3ab3aa
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2018
ms.locfileid: "39030677"
---
# <a name="watchos-platform-features"></a>WatchOS-Plattformfeatures

## <a name="introduction-to-watchos-5introduction-to-watchos5indexmd"></a>[Einführung in watchOS 5](introduction-to-watchos5/index.md)

Dieses Dokument enthält eine allgemeine Übersicht über neue und aktualisierte Features in WatchOS 5, die für die Verwendung verfügbar, beim Erstellen von WatchOS-apps mit Xamarin sind.

## <a name="introduction-to-watchos-4introduction-to-watchos4md"></a>[Einführung in watchOS 4](introduction-to-watchos4.md)

Dieses Dokument enthält eine allgemeine Übersicht über Features hinzugefügt und in WatchOS 4 aktualisiert.

## <a name="introduction-to-watchos-3introduction-to-watchos3indexmd"></a>[Einführung in watchOS 3](introduction-to-watchos3/index.md)

Dieser Artikel beschreibt die neuen und aktualisierten-APIs in WatchOS 3.

## <a name="apple-pay-enhancementsioswatchosplatformapple-paymd"></a>[Apple Pay-Verbesserungen](~/ios/watchos/platform/apple-pay.md)

In WatchOS 3 wurde das PassKit-Framework erweitert, um Unterstützung für sichere, in-app-Zahlungen (von sowohl physischen waren und Dienstleistungen) für die auf der Apple Watch-apps zu ermöglichen.

## <a name="background-tasksioswatchosplatformbackground-tasksmd"></a>[Hintergrundaufgaben](~/ios/watchos/platform/background-tasks.md)

WatchOS 3 führt mehrere Aufgaben im Hintergrund, die eine app verwenden können, aktualisieren Sie die zugehörigen Informationen stellt sicher, dass sie den Inhalt, die der Benutzer muss, bevor sie es öffnen.

## <a name="notificationsnotificationsmd"></a>[Benachrichtigungen](notifications.md)

Erfahren Sie, wie eine benutzerdefinierte Benachrichtigung behandeln in die Watch-app bereitzustellen.

## <a name="complicationscomplicationsmd"></a>[Komplikationen](complications.md)

Fügen Sie Komplikation-Unterstützung, um aktuelle Daten anzuzeigen, auf dem Zifferblatt Ihrer Apple Watch hinzu.

## <a name="proactive-suggestionsioswatchosplatformproactive-suggestionsmd"></a>[Proaktive Vorschläge](~/ios/watchos/platform/proactive-suggestions.md)

WatchOS 3 ermöglicht der app, proaktiv Informationen für den Benutzer in präsentieren Kontexten angegeben. Zur Unterstützung dieser Funktion, die [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity) enthält jetzt die `MapItem` -Eigenschaft, die der app, die Informationen zum Speicherort für die spätere Verwendung von anderen apps bereitstellen kann.

## <a name="quick-interaction-techniquesioswatchosplatformquick-interaction-techniquesmd"></a>[Schnelle interaktionstechniken](~/ios/watchos/platform/quick-interaction-techniques.md)

Bereitstellung schnell Benutzerinteraktionen sind Bedeutung für die Erstellung, tolle apps für Apple Watch und Schwierigkeiten. Neue watchos 3, Apple wurde Unterstützung für Gesten Erkennungen, Zugriff auf die digitale Crown und neue Benutzerbenachrichtigung und Navigation-Methoden hinzugefügt. Auf diese, zusammen mit Unterstützung für SceneKit und SpriteKit, dem können Entwickler auf einfache Weise umfassende, glanceable Schnittstellen zu erstellen, die schnelle und reaktionsfähige sind.

## <a name="workout-app-enhancementsioswatchosplatformworkout-appsmd"></a>[Trainings-app-Erweiterungen](~/ios/watchos/platform/workout-apps.md)

Neue WatchOS 3 Trainings im Zusammenhang mit apps haben die Möglichkeit, im Hintergrund ausgeführt werden soll, klicken Sie auf der Apple Watch. Um dieses Feature aktivieren (und erhalten Zugriff auf Daten von HealthKit), muss die app enthalten die `WKBackgroundModes` -Schlüssel in der `Info.plist` Datei mit dem Wert `workout-processing`.
