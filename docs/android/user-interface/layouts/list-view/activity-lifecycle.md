---
title: Xamarin. Android ListView und der Aktivitäts Lebenszyklus
ms.prod: xamarin
ms.assetid: 40840D03-6074-30A2-74DA-3664703E3367
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: a4ebda89ad3bdcb663dc9d7cd513e45760163c34
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028935"
---
# <a name="xamarinandroid-listview-and-the-activity-lifecycle"></a>Xamarin. Android ListView und der Aktivitäts Lebenszyklus

Aktivitäten durchlaufen bestimmte Zustände, wenn Ihre Anwendung ausgeführt wird, z. b. starten, ausführen, anhalten und beenden. Weitere Informationen und spezifische Richtlinien zur Verarbeitung von Zustands Übergängen finden Sie im [Tutorial zum Aktivitäts Lebenszyklus](~/android/app-fundamentals/activity-lifecycle/index.md).
Es ist wichtig, den Aktivitäts Lebenszyklus zu verstehen und den `ListView` Code an den richtigen Positionen zu platzieren.

Alle Beispiele in diesem Dokument führen ' Setup Tasks ' in der `OnCreate`-Methode der Aktivität aus und führen (falls erforderlich) ' Teardown ' in `OnDestroy`aus. In den Beispielen werden im allgemeinen kleine Datasets verwendet, die sich nicht ändern, sodass das erneute Laden der Daten häufiger erforderlich ist.

Wenn sich Ihre Daten jedoch häufig ändern oder viel Arbeitsspeicher belegen, kann es sinnvoll sein, unterschiedliche Lebenszyklus Methoden zum Auffüllen und Aktualisieren Ihrer `ListView`zu verwenden. Wenn sich die zugrunde liegenden Daten z. b. ständig ändern (oder von Updates anderer Aktivitäten betroffen sein können), wird beim Erstellen des Adapters in `OnStart` oder `OnResume` sichergestellt, dass die neuesten Daten jedes Mal angezeigt werden, wenn die Aktivität angezeigt wird.

Wenn der Adapter Ressourcen wie Arbeitsspeicher oder einen verwalteten Cursor verwendet, denken Sie daran, diese Ressourcen in der ergänzenden Methode an die Stelle, an der Sie instanziiert wurden, freizugeben (z. b. in `OnStart` erstellte Objekte können in `OnStop`gelöscht werden).

## <a name="configuration-changes"></a>Konfigurationsänderungen

Beachten Sie, dass Konfigurationsänderungen &ndash; vor allem die Bildschirmdrehung und die Sichtbarkeit der Tastatur &ndash; bewirken können, dass die aktuelle Aktivität zerstört und neu erstellt wird (es sei denn, Sie geben andernfalls das `ConfigurationChanges`-Attribut an). Dies bedeutet, dass unter normalen Bedingungen das Drehen eines Geräts dazu führt, dass eine `ListView` und `Adapter` neu erstellt werden. Wenn Sie keinen Code in `OnPause` und `OnResume`geschrieben haben, gehen die Scrollposition und die Zeilenauswahl Zustände verloren.

Durch das folgende Attribut wird verhindert, dass eine Aktivität aufgrund von Konfigurationsänderungen zerstört und neu erstellt wird:

```csharp
[Activity(ConfigurationChanges="keyboardHidden|orientation")]
```

Die Aktivität sollte dann `OnConfigurationChanged` überschreiben, um auf diese Änderungen entsprechend zu reagieren. Weitere Informationen zum Behandeln von Konfigurationsänderungen finden Sie in der Dokumentation.
