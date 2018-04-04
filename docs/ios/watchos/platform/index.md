---
title: Plattformfeatures
description: Apple Watch-spezifische Funktionen WatchOS apps einschließt.
ms.prod: xamarin
ms.assetid: 13F23E01-BAED-43EB-A70E-3B30EF53D379
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 9b90c799f2635221a2c19bda426c501737600f88
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="platform-features"></a>Plattformfeatures

_Apple Watch-spezifische Funktionen WatchOS apps einschließt._

## <a name="apple-pay-enhancementsioswatchosplatformapple-paymd"></a>[Apple Pay-Erweiterungen](~/ios/watchos/platform/apple-pay.md)

In WatchOS 3 wurde das Framework PassKit erweitert, um Unterstützung für sicheren, in der app Zahlungen (von sowohl physischen waren und Dienstleistungen) für die auf der Apple Watch-apps zu ermöglichen.

## <a name="background-tasksioswatchosplatformbackground-tasksmd"></a>[Hintergrundaufgaben](~/ios/watchos/platform/background-tasks.md)

WatchOS 3 führt mehrere Hintergrundaufgaben, die app nicht verwenden kann, die Informationen sicherstellen, dass er den Inhalt verfügt, die der Benutzer muss, bevor sie es öffnen aktualisieren.

## <a name="introduction-to-watchos-4introduction-to-watchos4md"></a>[Einführung in watchOS 4](introduction-to-watchos4.md)

Neue Funktionen in WatchOS 4.

## <a name="introduction-to-watchos-3introduction-to-watchos3indexmd"></a>[Einführung in watchOS 3](introduction-to-watchos3/index.md)

In diesem Artikel werden alle neuen und geänderten APIs in WatchOS 3 für Xamarin-Entwickler eingeführt.

##  <a name="notificationsnotificationsmd"></a>[Benachrichtigungen](notifications.md)

Erfahren Sie, wie eine benutzerdefinierte Benachrichtigung behandeln in Watch-app bereitzustellen.

##  <a name="complicationscomplicationsmd"></a>[Komplikationen](complications.md)

Fügen Sie Komplikation unterstützt das Anzeigen von aktuellen Daten auf dem Zifferblatt der Uhr hinzu.


## <a name="proactive-suggestionsioswatchosplatformproactive-suggestionsmd"></a>[Proaktive Vorschläge](~/ios/watchos/platform/proactive-suggestions.md)

WatchOS 3 ermöglicht der app, Informationen zum Benutzer in proaktiv präsentieren Kontexten angegeben. Zur Unterstützung dieser Funktion die [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity) enthält jetzt die `MapItem` -Eigenschaft, die die app, Informationen zum Speicherort für die spätere Verwendung durch andere apps bereitstellen kann.

## <a name="quick-interaction-techniquesioswatchosplatformquick-interaction-techniquesmd"></a>[Schnelle Interaktionstechniken](~/ios/watchos/platform/quick-interaction-techniques.md)

Eine schnelle Benutzerinteraktionen sind für das Erstellen von überzeugenden Apple Watch-apps und Komplikationen unverzichtbar. Neue WatchOS 3, ist Apple Unterstützung für den Zugriff auf die digitalen Crown und eine neue Benachrichtigung für Benutzer und Navigation Techniken Geste Merkmale hinzugefügt. Auf diese, zusammen mit Unterstützung für die SceneKit SpriteKit, können Entwickler auf einfache Weise umfangreiche, anzeigbare Schnittstellen zu erstellen, die schnelle und Reaktionsfähigkeit sind.

## <a name="workout-app-enhancementsioswatchosplatformworkout-appsmd"></a>[Trainings-App-Erweiterungen](~/ios/watchos/platform/workout-apps.md)

Neue WatchOS 3, Trainings bezüglich apps haben die Möglichkeit, auf der Apple Watch im Hintergrund ausgeführt. Um dieses Feature aktivieren (und erhalten Zugriff auf Daten HealthKit), muss die app enthalten die `WKBackgroundModes` -Schlüssel in der `Info.plist` Datei mit dem Wert `workout-processing`.
