---
title: Xamarin.Essentials Bildschirmsperre
description: Die ScreenLock-Klasse kann anfordern, um zu verhindern, dass der Bildschirm inaktiv zurück, wenn die Anwendung ausgeführt wird.
ms.assetid: 6B67C114-315E-4199-AA72-3F90E85A4909
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 66702e9f8f5e6ad07f8f7c96f739edf1b2e66617
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
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
        if (ScreenLock.IsMonitoring)
            ScreenLock.RequestActive();
        else
            ScreenLock.RequestRelease;
    }
}
```

## <a name="api"></a>API

- [Bildschirm sperren Quellcode](https://github.com/xamarin/Essentials/tree/master/Essentials/ScreenLock)
- [Bildschirm sperren API-Dokumentation](xref:Xamarin.Essentials.ScreenLock)
