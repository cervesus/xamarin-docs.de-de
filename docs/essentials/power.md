---
title: 'Xamarin.Essentials: Energiesparstatus'
description: Mithilfe der Power-Klasse erhält ein Programm den Energiesparstatus, um festzustellen, ob sich das Gerät im Energiesparmodus befindet.
ms.assetid: C176D177-8B77-4A9C-9F3B-27852A8DCD5F
author: jamesmontemagno
ms.author: jamont
ms.date: 06/27/2018
ms.openlocfilehash: 5a89dba16a93b007c5d7312221d8d33e00c7404a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110003"
---
# <a name="xamarinessentials-power-energy-saver-status"></a>Xamarin.Essentials: Energiesparstatus

![NuGet-Vorabrelease](~/media/shared/pre-release.png)

Die Klasse **Power** stellt Informationen zum Energiesparstatus des Geräts bereit, der angibt, ob sich das Gerät im Energiesparmodus befindet. Anwendungen sollten Hintergrundverarbeitung vermeiden, wenn der Energiesparmodus des Geräts aktiviert ist.

## <a name="background"></a>Hintergrund

Für Geräte, die mit Akku betrieben werden, lässt sich der Energiesparmodus aktivieren. Manchmal werden Geräte automatisch in diesen Modus umgeschaltet, z.B. wenn der Akku unter 20 % fällt. Das Betriebssystem reagiert auf den Energiesparmodus, indem Aktivitäten reduziert werden, die den Akku stark beanspruchen. Anwendungen können unterstützend wirken, indem sie Hintergrundverarbeitung oder andere Hochleistungsaktivitäten im Energiesparmodus vermeiden.

Für Android-Geräte gibt die Klasse **Power** nützliche Informationen nur für Android-Version 5.0 (Lollipop) und höher zurück.

## <a name="using-the-power-class"></a>Verwenden der Power-Klasse

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

Ermitteln Sie den aktuellen Energiesparstatus des Geräts über die statische Eigenschaft `Power.EnergySaverStatus`:

```csharp
// Get energy saver status
var status = Power.EnergySaverStatus;
```

Diese Eigenschaft gibt ein Mitglied der `EnergySaverStatus`-Enumeration zurück: `On`, `Off` oder `Unknown`. Wenn die Eigenschaft `On` zurückgibt, sollte die Anwendung Hintergrundverarbeitung oder andere Aktivitäten vermeiden, die ggf. viel Energie verbrauchen.

Außerdem sollte die Anwendung einen Ereignishandler installieren. Die Klasse **Power** zeigt ein Ereignis an, das ausgelöst wird, wenn sich der Energiesparstatus ändert:

```csharp
public class EnergySaverTest
{
    public EnergySaverTest()
    {
        // Subscribe to changes of energy-saver status
        Power.EnergySaverStatusChanged += OnEnergySaverStatusChanged;
    }

    private void OnEnergySaverStatusChanged(EnergySaverStatusChangedEventArgs e)
    {
        // Process change
        var status = e.EnergySaverStatus;
    }
}
```

Wenn sich der Energiesparstatus auf `On` ändert, sollte die Anwendung die Hintergrundverarbeitung deaktivieren. Wenn der Status auf `Unknown` oder `Off` wechselt, kann die Anwendung die Hintergrundverarbeitung wieder aufnehmen.

## <a name="api"></a>API

- [Power-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Power)
- [Power-API-Dokumentation](xref:Xamarin.Essentials.Power)
