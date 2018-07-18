---
title: 'Xamarin.Essentials: Energiestatus Energie sparen'
description: Die Power-Klasse ermöglicht es sich um ein Programm zum Abrufen des Status energiesparend, um festzustellen, ob das Gerät in einen Energiesparmodus-Modus ausgeführt wird.
ms.assetid: C176D177-8B77-4A9C-9F3B-27852A8DCD5F
author: charlespetzold
ms.author: chape
ms.date: 06/27/2018
ms.openlocfilehash: 6d8ccb5be69eb1ea7ea63d3f5c373d9284089679
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831519"
---
# <a name="xamarinessentials-power-energy-saver-status"></a>Xamarin.Essentials: Energiestatus Energie sparen

![Vorabversionen von NuGet](~/media/shared/pre-release.png)

Die **Power** Klasse enthält Informationen zu der der energiesparend Gerätestatus, das angibt, ob das Gerät in einen energiesparenden-Modus ausgeführt wird. Anwendungen sollten die Verarbeitung im Hintergrund, wenn das Gerät die energiesparend Status auf.

## <a name="background"></a>Hintergrund

Geräte, die Akkus betrieben werden, können in einen energiesparenden energiesparend Modus platziert werden. Manchmal werden Geräte in diesem Modus automatisch, z. B. gewechselt, wenn der Akku unter 20 % Kapazität sinkt. Das Betriebssystem antwortet auf Energiesparmodus durch Reduzierung von Aktivitäten, die tendenziell die Batterie erschöpft sind. Anwendungen können durch die Verarbeitung im Hintergrund oder andere Stromstärke Aktivitäten zu vermeiden, wenn Energiesparmodus aktiviert ist.

Für Android-Geräte die **Power** Klasse gibt sinnvolle Informationen, die nur für Android-Version 5.0 (Lollipop) oder höher zurück.

## <a name="using-the-power-class"></a>Verwenden der Power-Klasse

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Abrufen des aktuellen energiesparend Status des Geräts mit der statischen `Power.EnergySaverStatus` Eigenschaft:

```csharp
// Get energy saver status
var status = Power.EnergySaverStatus;
```

Diese Eigenschaft gibt ein Mitglied der `EnergySaverStatus` -Enumeration, die entweder `On`, `Off`, oder `Unknown`. Wenn die Eigenschaft zurückgibt `On`, die Anwendung sollte kein Verarbeitung im Hintergrund oder andere Aktivitäten, die viel Leistung beanspruchen können.

Außerdem sollten die Anwendung einen Ereignishandler installieren. Die **Power** -Klasse macht ein Ereignis aus, die ausgelöst wird, wenn der energiesparend Status wechselt:

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

Ändert die energiesparend Status auf `On`, Beenden der Anwendung sollten die hintergrundverarbeitung durchführen. Wenn Sie den Status `Unknown` oder `Off`, die Anwendung kann die Verarbeitung im Hintergrund fortgesetzt.

## <a name="api"></a>API

- [Power-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Power)
- [Power-API-Dokumentation](xref:Xamarin.Essentials.Power)
