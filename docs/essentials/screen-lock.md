---
title: 'Xamarin.Essentials: Bildschirmsperre'
description: Dieses Dokument beschreibt die Klasse ScreenLock in Xamarin.Essentials, die anfordern kann, um zu verhindern, dass der Bildschirm inaktiv zurück, wenn die Anwendung ausgeführt wird.
ms.assetid: 6B67C114-315E-4199-AA72-3F90E85A4909
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3c8110b7abc86fe1d12485579f134997718540e6
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782907"
---
# <a name="xamarinessentials-screen-lock"></a>Xamarin.Essentials: Bildschirmsperre

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
        if (!ScreenLock.IsActive)
            ScreenLock.RequestActive();
        else
            ScreenLock.RequestRelease();
    }
}
```

## <a name="api"></a>API

- [Bildschirm sperren Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/ScreenLock)
- [Bildschirm sperren API-Dokumentation](xref:Xamarin.Essentials.ScreenLock)
