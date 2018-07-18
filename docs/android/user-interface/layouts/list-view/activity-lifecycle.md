---
title: ListView-Steuerelement und den Aktivitätenlebenszyklus
ms.prod: xamarin
ms.assetid: 40840D03-6074-30A2-74DA-3664703E3367
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 6e15fb8796ae6a616c5eae44059caae3d9478aef
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30764220"
---
# <a name="listview-and-the-activity-lifecycle"></a>ListView-Steuerelement und den Aktivitätenlebenszyklus

Aktivitäten durchlaufen bestimmte Zustände als die Anwendung ausgeführt wird, z. B. starten, wird ausgeführt, wird angehalten und beendet wurde. Weitere Informationen und bestimmte Richtlinien für die Behandlung von Zustandsübergängen, finden Sie unter der [Lernprogramm zum Lebenszyklus von Aktivität](~/android/app-fundamentals/activity-lifecycle/index.md).
Es ist wichtig zu verstehen, den Aktivitätenlebenszyklus und den Ort Ihrer `ListView` Code in den richtigen Verzeichnissen.

Alle Beispiele in diesem Dokument in der Aktivitätssymbols ausführen "Setuptasks" `OnCreate` Methode und (wenn erforderlich) führen Sie zum "Beenden" im `OnDestroy`. Die Beispiele verwenden im Allgemeinen kleine Datasets, die nicht ändern, damit neu häufiger Laden der Daten nicht aufbewahrt werden.

Allerdings Ihrer Daten werden häufig ändern oder zu viel Arbeitsspeicher verwendet es kann sinnvoll sein, mit anderen Lebenszyklusmethoden aufzufüllen, und aktualisieren Sie Ihre `ListView`. Angenommen, wenn die zugrunde liegenden Daten werden ständig (oder möglicherweise Updates für andere Aktivitäten betroffen) und dann erstellen den Adapter in `OnStart` oder `OnResume` stellt sicher, dass die neuesten Daten werden jedes Mal die Aktivität angezeigt wird angezeigt.

Wenn der Adapter Ressourcen wie Arbeitsspeicher, oder einen verwalteten Cursor verwendet, denken Sie daran, diese Ressourcen in die komplementäre Methode hinzu, in dem sie (z. b. instanziiert wurden freizugeben in der erstellten Objekte `OnStart` können freigegeben werden, der in `OnStop`).


## <a name="configuration-changes"></a>Änderungen an der Konfiguration

Es ist wichtig, daran zu denken, dass die Konfiguration ändert &ndash; besonders Bildschirm Drehung und Tastatur Sichtbarkeit &ndash; kann dazu führen, dass die aktuelle Aktivität zerstört und neu erstellt werden (es sei denn, Sie geben Sie andernfalls mit der `ConfigurationChanges` Attribut ""). Dies bedeutet, dass unter normalen Bedingungen Drehen eines Geräts bewirkt eine `ListView` und `Adapter` neu erstellt werden und (es sei denn, Sie Code, in verfasst haben `OnPause` und `OnResume`) die Scroll Position und Zeile Auswahl Zustände verloren.

Das folgende Attribut würde es sich um eine Aktivität zerstört und neu erstellt, als Ergebnis der Änderungen an der Konfiguration verhindern:

```csharp
[Activity(ConfigurationChanges="keyboardHidden|orientation")]
```

Die Aktivität sollte dann überschreiben `OnConfigurationChanged` auf diese Änderungen entsprechend reagieren. Weitere Informationen zum Behandeln von konfigurationsänderungen finden Sie in der Dokumentation.

