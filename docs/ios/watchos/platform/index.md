---
title: watchos-Platt Form Features
description: Dieses Dokument enthält Links zu verschiedenen Leitfäden, in denen watchos-Platt Form Features beschrieben werden, z. b. Apple Pay, Benachrichtigungen, Komplikationen, proaktive Vorschläge, Workout-apps und mehr.
ms.prod: xamarin
ms.assetid: 13F23E01-BAED-43EB-A70E-3B30EF53D379
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 10/05/2018
ms.openlocfilehash: 2b987992bcb3dd4d2575a46e21a2302ed78d8d70
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70282288"
---
# <a name="watchos-platform-features"></a>watchos-Platt Form Features

## <a name="introduction-to-watchos-5introduction-to-watchos5indexmd"></a>[Einführung in watchOS 5](introduction-to-watchos5/index.md)

Dieses Dokument enthält eine allgemeine Übersicht über neue und aktualisierte Features in watchos 5, die für die Erstellung von watchos-apps mit xamarin verfügbar sind.

## <a name="introduction-to-watchos-4introduction-to-watchos4md"></a>[Einführung in watchOS 4](introduction-to-watchos4.md)

Dieses Dokument enthält eine allgemeine Übersicht über die in watchos 4 hinzugefügten und aktualisierten Features.

## <a name="introduction-to-watchos-3introduction-to-watchos3indexmd"></a>[Einführung in watchOS 3](introduction-to-watchos3/index.md)

In diesem Artikel werden neue und aktualisierte APIs in watchos 3 beschrieben.

## <a name="apple-pay-enhancementsioswatchosplatformapple-paymd"></a>[Apple Pay Erweiterungen](~/ios/watchos/platform/apple-pay.md)

In watchos 3 wurde das passkit-Framework erweitert, um Unterstützung für sichere, in-App-Zahlungen (sowohl physischer waren als auch Dienste) für die apps, die auf dem Apple Watch ausgeführt werden, zu ermöglichen.

## <a name="background-tasksioswatchosplatformbackground-tasksmd"></a>[Hintergrundaufgaben](~/ios/watchos/platform/background-tasks.md)

watchos 3 führt mehrere Hintergrundaufgaben ein, die eine APP zum Aktualisieren Ihrer Informationen verwenden kann, um sicherzustellen, dass Sie die Inhalte der Benutzer benötigt, bevor Sie Sie öffnen.

## <a name="notificationsnotificationsmd"></a>[Benachrichtigungen](notifications.md)

Erfahren Sie, wie Sie eine benutzerdefinierte Benachrichtigungs Behandlung in ihrer Watch-App bereitstellen

## <a name="complicationscomplicationsmd"></a>[Komplikationen](complications.md)

Fügen Sie komplikationsunterstützung hinzu, um aktuelle Daten auf dem Überwachungs Gesicht anzuzeigen.

## <a name="proactive-suggestionsioswatchosplatformproactive-suggestionsmd"></a>[Proaktive Vorschläge](~/ios/watchos/platform/proactive-suggestions.md)

watchos 3 ermöglicht der APP, den Benutzern in den angegebenen Kontexten proaktiv Informationen zur Verfügung zu stellen. Zur Unterstützung dieser Funktion enthält [nsuseractivity](https://developer.apple.com/reference/foundation/nsuseractivity) nun die `MapItem` -Eigenschaft, mit der die APP Standortinformationen zur späteren Verwendung durch andere apps bereitstellen kann.

## <a name="quick-interaction-techniquesioswatchosplatformquick-interaction-techniquesmd"></a>[Techniken für die schnelle Interaktion](~/ios/watchos/platform/quick-interaction-techniques.md)

Das Bereitstellen von schnellen Benutzerinteraktionen ist wichtig für das Erstellen von überzeugenden Apple Watch apps und Komplikationen. Ab watchos 3 hat Apple Unterstützung für Gesten Erkennungs Tools, den Zugriff auf die Digital Crown und neue Benutzerbenachrichtigungs-und Navigationsverfahren hinzugefügt. Neben der zusätzlichen Unterstützung von scenekit und spritekit ermöglicht es dem Entwickler, problemlos umfangreiche, schnell zu erstellen, die schnell und reaktionsfähig sind.

## <a name="workout-app-enhancementsioswatchosplatformworkout-appsmd"></a>[Erweiterungen für App-Erweiterungen](~/ios/watchos/platform/workout-apps.md)

Neu bei watchos 3: mit dem Training verbundene Apps können im Hintergrund des Apple Watch ausgeführt werden. Um dieses Feature zu aktivieren (und Zugriff auf healthkit-Daten zu erhalten), muss die `WKBackgroundModes` App den Schlüssel `Info.plist` in die Datei mit `workout-processing`dem Wert einschließen.
