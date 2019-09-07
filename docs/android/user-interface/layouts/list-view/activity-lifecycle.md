---
title: Xamarin. Android ListView und der Aktivitäts Lebenszyklus
ms.prod: xamarin
ms.assetid: 40840D03-6074-30A2-74DA-3664703E3367
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 1c093d3d67ace3b0f9186fca8226d4ef631d9af0
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70762304"
---
# <a name="xamarinandroid-listview-and-the-activity-lifecycle"></a>Xamarin. Android ListView und der Aktivitäts Lebenszyklus

Aktivitäten durchlaufen bestimmte Zustände, wenn Ihre Anwendung ausgeführt wird, z. b. starten, ausführen, anhalten und beenden. Weitere Informationen und spezifische Richtlinien zur Verarbeitung von Zustands Übergängen finden Sie im [Tutorial zum Aktivitäts Lebenszyklus](~/android/app-fundamentals/activity-lifecycle/index.md).
Es ist wichtig, den Aktivitäts Lebenszyklus zu verstehen und `ListView` den Code an den richtigen Positionen zu platzieren.

Alle Beispiele in diesem Dokument führen "Setup Tasks" in der- `OnCreate` Methode der Aktivität durch und (bei Bedarf) "Teardown" in. `OnDestroy` In den Beispielen werden im allgemeinen kleine Datasets verwendet, die sich nicht ändern, sodass das erneute Laden der Daten häufiger erforderlich ist.

Wenn sich Ihre Daten jedoch häufig ändern oder viel Arbeitsspeicher belegen, kann es sinnvoll sein, unterschiedliche Lebenszyklus Methoden zum Auffüllen und Aktualisieren Ihres `ListView`zu verwenden. Wenn sich die zugrunde liegenden Daten z. b. ständig ändern (oder von Updates anderer Aktivitäten betroffen sein können), wird beim Erstellen `OnStart` des `OnResume` Adapters in oder sichergestellt, dass die neuesten Daten jedes Mal angezeigt werden, wenn die Aktivität angezeigt wird.

Wenn der Adapter Ressourcen wie Arbeitsspeicher oder einen verwalteten Cursor verwendet, denken Sie daran, diese Ressourcen in der ergänzenden Methode an die Stelle, an der Sie instanziiert wurden, freizugeben (z. b. in erstellte `OnStart` Objekte können in `OnStop`gelöscht werden.)

## <a name="configuration-changes"></a>Konfigurationsänderungen

Beachten Sie, dass Konfigurationsänderungen &ndash; , insbesondere Bildschirmdrehung und Tastatur Sichtbarkeit &ndash; , dazu führen können, dass die aktuelle Aktivität zerstört und neu erstellt wird (es sei denn, Sie geben andernfalls mit dem `ConfigurationChanges` -Attribut). Dies bedeutet, dass unter normalen Bedingungen das Drehen eines Geräts dazu führt `ListView` , `Adapter` dass und neu erstellt werden. (sofern Sie keinen Code in `OnPause` und `OnResume`geschrieben haben), gehen die Scrollposition und die Zeilenauswahl Zustände verloren.

Durch das folgende Attribut wird verhindert, dass eine Aktivität aufgrund von Konfigurationsänderungen zerstört und neu erstellt wird:

```csharp
[Activity(ConfigurationChanges="keyboardHidden|orientation")]
```

Die Aktivität sollte dann über `OnConfigurationChanged` schreiben, um auf diese Änderungen entsprechend zu reagieren. Weitere Informationen zum Behandeln von Konfigurationsänderungen finden Sie in der Dokumentation.
