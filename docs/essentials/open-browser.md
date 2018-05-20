---
title: Öffnen Sie Xamarin.Essentials Browser
description: Die Browser-Klasse ermöglicht einer Anwendung auf einen Weblink im optimierten System bevorzugten Browser oder den externen Browser öffnen.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 2a95d0b89b78ce8b0ddfdc94d86c284a6f8b2bbe
ms.sourcegitcommit: 4db5f5c93f79f273d8fc462de2f405458b62fc02
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2018
---
# <a name="xamarinessentials-browser"></a>Xamarin.Essentials-Browser

![Vorabversion NuGet](~/media/shared/pre-release.png)

Die **Browser** -Klasse kann eine Anwendung auf einen Weblink im optimierten System bevorzugten Browser oder den externen Browser öffnen.

## <a name="using-browser"></a>Webbrowser

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Der Webbrowser-Funktionen arbeitet, durch Aufrufen der `OpenAsync` Methode mit dem `Uri` und `BrowserLaunchType`.

```csharp

public class BrowserTest
{
    public async Task OpenBrowser(Uri uri)
    {
        await Browser.OpenAsync(uri, BrowserLaunchType.SystemPreferred);
    }
}
```

## <a name="platform-implementation-specifics"></a>Plattform Implementierungsspezifika

# <a name="androidtabandroid"></a>[Android](#tab/android)

Der Typ starten bestimmt, wie der Browser gestartet wird:

## <a name="system-preferred"></a>Bevorzugte System

[Chrom benutzerdefinierte Registerkarten](https://developer.chrome.com/multidevice/android/customtabs) wird versucht, die verwendet werden, laden Sie die Uri und Awareness Navigations-beibehalten.

## <a name="external"></a>Extern

Ein `Intent` wird verwendet, um anzufordern, der Uri über den normalen Systeme-Browser geöffnet werden.

# <a name="iostabios"></a>[iOS](#tab/ios)

## <a name="system-preferred"></a>Bevorzugte System

[SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) ist, der zum Laden des Uri, und halten Awareness Navigations-verwendet.

## <a name="external"></a>Extern

Der Standard `OpenUrl` auf die Hauptassembly der Anwendung wird verwendet, um den Standardbrowser außerhalb der Anwendung zu starten.

# <a name="uwptabuwp"></a>[UNIVERSELLE WINDOWS-PLATTFORM](#tab/uwp)

Standard-Browser des Benutzers wird immer gestartet werden, unabhängig von der `BrowserLaunchType`.

--------------

## <a name="api"></a>API

- [Browser-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Browser)
- [-API-Browserdokumentation](xref:Xamarin.Essentials.Browser)
