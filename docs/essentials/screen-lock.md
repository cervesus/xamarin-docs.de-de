---
title: 'Xamarin.Essentials: Bildschirmsperre'
description: In diesem Dokument wird die Klasse „ScreenLock“ in Xamarin.Essentials beschrieben, mit der verhindert werden kann, dass der Bildschirm in den Energiesparmodus wechselt, wenn die Anwendung ausgeführt wird.
ms.assetid: 6B67C114-315E-4199-AA72-3F90E85A4909
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3bf8c949650cf9f039a5a516366a90e717dc944b
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675314"
---
# <a name="xamarinessentials-screen-lock"></a>Xamarin.Essentials: Bildschirmsperre

![NuGet-Vorabrelease](~/media/shared/pre-release.png)

Die Klasse **ScreenLock** kann verhindern, dass der Bildschirm in den Energiesparmodus wechselt, wenn die Anwendung ausgeführt wird.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-screenlock"></a>Verwenden der Bildschirmsperre

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

Die Bildschirmsperrfunktion ruft die Methoden `RequestActive` und `RequestRelease` auf, um zu verhindern, dass der Bildschirm deaktiviert wird.

```csharp
public class ScreenLockTest
{
    public void ToggleScreenLock()
    {
        if (!ScreenLock.IsActive)
            ScreenLock.RequestActive();
        else
            ScreenLock.RequestRelease();
    }
}
```

## <a name="api"></a>API

- [Screen Lock-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/ScreenLock)
- [Screen Lock-API-Dokumentation](xref:Xamarin.Essentials.ScreenLock)
