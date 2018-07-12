---
title: 'Xamarin.Essentials: Bildschirmsperre'
description: Dieses Dokument beschreibt die ScreenLock-Klasse in Xamarin.Essentials, dem anfordern können, um zu verhindern, dass den Bildschirm im Standbymodus zurück, wenn die Anwendung ausgeführt wird.
ms.assetid: 6B67C114-315E-4199-AA72-3F90E85A4909
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3c8110b7abc86fe1d12485579f134997718540e6
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38848569"
---
# <a name="xamarinessentials-screen-lock"></a>Xamarin.Essentials: Bildschirmsperre

![Vorabversionen von NuGet](~/media/shared/pre-release.png)

Die **ScreenLock** Klasse kann anfordern, um zu verhindern, dass den Bildschirm im Standbymodus zurück, wenn die Anwendung ausgeführt wird.

## <a name="using-screenlock"></a>Verwenden von ScreenLock

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Der Bildschirm sperren funktioniert durch Aufrufen der `RequestActive` und `RequestRelease` Methoden, um den Bildschirm anfordern, zu deaktivieren.

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
- [Bildschirm sperren-API-Dokumentation](xref:Xamarin.Essentials.ScreenLock)
