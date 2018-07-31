---
title: Xamarin.Essentials-Startprogramm
description: Die Startprogramm-Klasse in Xamarin.Essentials ermöglicht eine Anwendung, die einen URI zu öffnen, indem Sie das System.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 07/25/2018
ms.openlocfilehash: 3ab820ece127558c1a5ca8b13c05fc4d6f20646e
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353939"
---
# <a name="xamarinessentials-launcher"></a>Xamarin.Essentials: Startprogramm

![Vorabversionen von NuGet](~/media/shared/pre-release.png)

Die **Startprogramm** Klasse ermöglicht einer Anwendung, die einen URI zu öffnen, indem Sie das System. Dies wird häufig bei Tiefe Verknüpfungen in einer anderen Anwendung benutzerdefinierte URI-Schemas verwendet. Wenn Sie um den Browser, um eine Website zu öffnen möchten, und klicken Sie dann Sie lesen die **[Browser](open-browser.md)** API.

## <a name="using-launcher"></a>Verwenden von Startprogramm

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Den Startprogramm Funktionalität-Aufruf verwendet die `OpenAsync` -Methode und übergeben Sie einen `string` oder `Uri` zu öffnen. Optional können die `CanOpenAsync` Methode kann verwendet werden, zu überprüfen, ob das URI-Schema von einer Anwendung auf dem Gerät verarbeitet werden kann.

```csharp
public class LauncherTest
{
    public async Task OpenRideShare()
    {
        var supportsUri = await Launcher.CanOpenAsync("lyft://");
        if (supportsUri)
            await Launcher.OpenAsync("lyft://ridetype?id=lyft_line");
    }
}
```

## <a name="api"></a>API

- [Startprogramm-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Launcher)
- [Startprogramm für API-Dokumentation](xref:Xamarin.Essentials.Launcher)
