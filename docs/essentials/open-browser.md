---
title: Öffnen Sie Xamarin.Essentials Browser
description: Die Browser-Klasse in Xamarin.Essentials ermöglicht eine Anwendung auf einen Weblink im bevorzugten Browser optimierten System oder der externen Browser öffnen.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 7e58d439f5a6eaafe9b1b5e7ca874a986e468cb9
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353281"
---
# <a name="xamarinessentials-browser"></a>Xamarin.Essentials: Browser

![Vorabversionen von NuGet](~/media/shared/pre-release.png)

Die **Browser** Klasse ermöglicht einer Anwendung auf einen Weblink im bevorzugten Browser optimierten System oder der externen Browser öffnen.

## <a name="using-browser"></a>Browser verwenden

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Die Funktionalität des Eigenschaftenbrowsers erfolgt durch Aufrufen der `OpenAsync` -Methode mit dem `Uri` und `BrowserLaunchMode`.

```csharp

public class BrowserTest
{
    public async Task OpenBrowser(Uri uri)
    {
        await Browser.OpenAsync(uri, BrowserLaunchMode.SystemPreferred);
    }
}
```

## <a name="platform-implementation-specifics"></a>Implementierung von Plattformeigenschaften

# <a name="androidtabandroid"></a>[Android](#tab/android)

Der Modus starten wird bestimmt, wie der Browser gestartet wird:

## <a name="system-preferred"></a>Bevorzugte System

[Benutzerdefinierte Registerkarten in Chrome](https://developer.chrome.com/multidevice/android/customtabs) wird versucht, die verwendet werden, laden Sie den Uri, und halten Sie die Unterstützung der Navigation.

## <a name="external"></a>Extern

Ein `Intent` wird verwendet, um die Anforderung der Uri über den normalen Systeme-Browser geöffnet werden.

# <a name="iostabios"></a>[iOS](#tab/ios)

## <a name="system-preferred"></a>Bevorzugte System

[SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) ist, der zum Laden des Uri, und halten die Navigation Awareness verwendet.

## <a name="external"></a>Extern

Der Standard `OpenUrl` für die hauptanwendung verwendet, um den Standardbrowser außerhalb der Anwendung zu starten.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Standardbrowser des Benutzers werden immer gestartet werden, unabhängig von der `BrowserLaunchMode`.

--------------

## <a name="api"></a>API

- [Browser-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Browser)
- [API-Browser-Dokumentation](xref:Xamarin.Essentials.Browser)
