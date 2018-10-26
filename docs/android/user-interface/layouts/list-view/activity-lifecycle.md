---
title: ListView und der Aktivitätslebenszyklus
ms.prod: xamarin
ms.assetid: 40840D03-6074-30A2-74DA-3664703E3367
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: b2328759b3158920bc8683ec14c2aebefd7a04ae
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117719"
---
# <a name="listview-and-the-activity-lifecycle"></a>ListView und der Aktivitätslebenszyklus

Aktivitäten durchlaufen bestimmte Zustände als die Anwendung ausgeführt wird, z. B. starten, wird ausgeführt, angehalten wird und beendet wurde. Weitere Informationen und besondere Richtlinien zum Behandeln von Zustandsübergängen, finden Sie unter den [Tutorial zur Kopieraktivität Lebenszyklus](~/android/app-fundamentals/activity-lifecycle/index.md).
Es ist wichtig zu verstehen, den aktivitätslebenszyklus und den Ort Ihrer `ListView` Code in den richtigen Verzeichnissen.

Alle Beispiele in diesem Dokument in der Aktivitäts ausführen "Setuptasks" `OnCreate` Methode und (wenn erforderlich) führen Sie "Beendigung" im `OnDestroy`. Die Beispiele verwenden in der Regel kleine Datasets, die nicht geändert werden, sodass erneut häufiger Laden der Daten nicht erforderlich ist.

Aber wenn Ihre Daten häufig ändert oder einen viel Arbeitsspeicher verwendet möglicherweise mit verschiedenen Methoden zum Auffüllen und Aktualisieren Ihrer `ListView`. Z. B., wenn die zugrunde liegenden Daten stetig (oder Updates für andere Aktivitäten betroffen sein können) und dann erstellen den Adapter in `OnStart` oder `OnResume` wird sichergestellt, dass die neuesten Daten werden jedes Mal angezeigt wird die Aktivität angezeigt.

Wenn der Adapter-Ressourcen wie Arbeitsspeicher oder einen verwalteten Cursor verwendet, denken Sie daran, diese Ressourcen in der sich ergänzende-Methode, in dem sie (z.B.) instanziiert wurden freigegeben im erstellten Objekte `OnStart` können freigegeben werden, der in `OnStop`).


## <a name="configuration-changes"></a>Änderungen an der Konfiguration

Es ist wichtig, denken Sie daran, dass die Konfiguration ändert &ndash; besonders Bildschirm Drehungs- und Tastatur Sichtbarkeit &ndash; kann dazu führen, dass die aktuelle Aktivität zerstört und neu erstellt werden (es sei denn, Sie geben Sie andernfalls mit der `ConfigurationChanges` -Attribut). Dies bedeutet, dass unter normalen Bedingungen Drehen eines Geräts bewirkt eine `ListView` und `Adapter` neu erstellt werden und (es sei denn, Sie Code, in geschrieben haben `OnPause` und `OnResume`) die Auswahl scrollen, Position und die Zeile angibt, geht verloren.

Das folgende Attribut würde es sich um eine Aktivität aus zerstört und neu erstellt, als Ergebnis der Änderungen an der Konfiguration verhindern:

```csharp
[Activity(ConfigurationChanges="keyboardHidden|orientation")]
```

Die Aktivität sollte dann überschreiben `OnConfigurationChanged` angemessen auf diese Änderungen reagieren. Weitere Informationen zum Behandeln von konfigurationsänderungen finden Sie in der Dokumentation.

