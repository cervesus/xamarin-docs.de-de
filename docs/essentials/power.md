---
title: 'Xamarin.Essentials: Energiezustand Energie sparen'
description: Die Power-Klasse ermöglicht es sich um ein Programm zum Abrufen des Status Energie sparen, um festzustellen, ob das Gerät in einem LP-Modus ausgeführt wird.
ms.assetid: C176D177-8B77-4A9C-9F3B-27852A8DCD5F
author: charlespetzold
ms.author: chape
ms.date: 06/27/2018
ms.openlocfilehash: 6d8ccb5be69eb1ea7ea63d3f5c373d9284089679
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080530"
---
# <a name="xamarinessentials-power-energy-saver-status"></a>Xamarin.Essentials: Energiezustand Energie sparen

![Vorabversion NuGet](~/media/shared/pre-release.png)

Die **Power** Klasse enthält Informationen über das Gerät Energie sparen Status, das angibt, ob das Gerät im Energiesparmodus-Modus ausgeführt wird. Anwendungen sollten Verarbeitung im Hintergrund aus, wenn das Gerät Energie sparen Status auf.

## <a name="background"></a>Hintergrund

Geräte, die Akkus laufen können in einen Energiesparmodus Energiesparmodus versetzt werden. In einigen Fällen werden Geräte in diesem Modus automatisch, z. B. umgeschaltet, wenn der Akku unter 20 % Kapazität fällt. Das Betriebssystem reagiert auf Energiesparmodus, durch das Reduzieren von Aktivitäten, die tendenziell Akkus verbraucht. Anwendungen können durch die Verarbeitung im Hintergrund oder andere Stromstärke Aktivitäten zu vermeiden, wenn Energiesparmodus aktiviert ist.

Für Android-Geräte die **Power** Klasse gibt sinnvolle Informationen nur für Android, Version 5.0 (Lollipop) und höher zurück.

## <a name="using-the-power-class"></a>Verwenden der Power-Klasse

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Abrufen des aktuellen Status der Energie-Bildschirmschoner des Geräts mithilfe der statischen `Power.EnergySaverStatus` Eigenschaft:

```csharp
// Get energy saver status
var status = Power.EnergySaverStatus;
```

Diese Eigenschaft gibt ein Mitglied der `EnergySaverStatus` -Enumeration, die entweder `On`, `Off`, oder `Unknown`. Wenn die Eigenschaft zurückgibt `On`, die Anwendung sollten Verarbeitung im Hintergrund oder andere Aktivitäten, die viel Energie belegen können.

Die Anwendung sollte auch einen Ereignishandler installieren. Die **Power** Klasse macht ein Ereignis, das ausgelöst wird, wenn der Energie-Bildschirmschoner Status wechselt:

```csharp
public class EnergySaverTest
{
    public EnergySaverTest()
    {
        // Subscribe to changes of energy-saver status
        Power.EnergySaverStatusChanaged += OnEnergySaverStatusChanaged;
    }

    private void OnEnergySaverStatusChanaged(EnergySaverStatusChanagedEventArgs e)
    {
        // Process change
        var status = e.EnergySaverStatus;
    }
}
```

Ändert den Status der Energie-Bildschirmschoner auf `On`, sollte die Anwendung beenden, Verarbeitung im Hintergrund ausführen. Ändert den Status auf `Unknown` oder `Off`, die Anwendung kann die Verarbeitung im Hintergrund fortgesetzt.

## <a name="api"></a>API

- [Power-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Power)
- [Power-API-Dokumentation](xref:Xamarin.Essentials.Power)
