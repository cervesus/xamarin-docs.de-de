---
title: Xamarin.Essentials Bildschirmsperre
description: Die ScreenLock-Klasse kann anfordern, um zu verhindern, dass der Bildschirm inaktiv zurück, wenn die Anwendung ausgeführt wird.
ms.assetid: 6B67C114-315E-4199-AA72-3F90E85A4909
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 0bdf75825d9c6dc594749fe7aa1e133207cfa0fa
ms.sourcegitcommit: 4db5f5c93f79f273d8fc462de2f405458b62fc02
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2018
---
# <a name="xamarinessentials-screen-lock"></a>Xamarin.Essentials Bildschirmsperre

![Vorabversion NuGet](~/media/shared/pre-release.png)

Die **ScreenLock** Klasse kann anfordern, um zu verhindern, dass der Bildschirm inaktiv zurück, wenn die Anwendung ausgeführt wird.

## <a name="using-screenlock"></a>Verwenden von ScreenLock

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Der Bildschirm sperren Funktionsweise durch Aufrufen der `RequestActive` und `RequestRelease` Methoden zum anfordern, dass der Bildschirm zu deaktivieren.

```csharp
public class ScreenLockTest
{
    public void ToggleScreenLock()
    {
        if (ScreenLock.IsActive)
            ScreenLock.RequestActive();
        else
            ScreenLock.RequestRelease();
    }
}
```

## <a name="api"></a>API

- [Bildschirm sperren Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/ScreenLock)
- [Bildschirm sperren API-Dokumentation](xref:Xamarin.Essentials.ScreenLock)
