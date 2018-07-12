---
title: Öffnen Sie Xamarin.Essentials Browser
description: Die Browser-Klasse in Xamarin.Essentials ermöglicht eine Anwendung auf einen Weblink im bevorzugten Browser optimierten System oder der externen Browser öffnen.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 563d3899cffb80c0215d90e8e4392046c4635256
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815705"
---
# <a name="xamarinessentials-browser"></a>Xamarin.Essentials: Browser

![Vorabversionen von NuGet](~/media/shared/pre-release.png)

Die **Browser** Klasse ermöglicht einer Anwendung auf einen Weblink im bevorzugten Browser optimierten System oder der externen Browser öffnen.

## <a name="using-browser"></a>Browser verwenden

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Die Funktionalität des Eigenschaftenbrowsers erfolgt durch Aufrufen der `OpenAsync` -Methode mit dem `Uri` und `BrowserLaunchType`.

```csharp

public class BrowserTest
{
    public async Task OpenBrowser(Uri uri)
    {
        await Browser.OpenAsync(uri, BrowserLaunchType.SystemPreferred);
    }
}
```

## <a name="platform-implementation-specifics"></a>Implementierung von Plattformeigenschaften

# <a name="androidtabandroid"></a>[Android](#tab/android)

Der starten-Typ bestimmt, wie der Browser gestartet wird:

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

Standardbrowser des Benutzers werden immer gestartet werden, unabhängig von der `BrowserLaunchType`.

--------------

## <a name="api"></a>API

- [Browser-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Browser)
- [API-Browser-Dokumentation](xref:Xamarin.Essentials.Browser)
