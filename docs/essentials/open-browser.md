---
title: 'Xamarin.Essentials: „Browser öffnen“'
description: Mit der Browser-Klasse in Xamarin.Essentials kann eine Anwendung einen Weblink im optimierten und vom System bevorzugten Browser oder im externen Browser öffnen.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: a68837ac4447dabcf52a1d1b27913adf80b4cbd7
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675392"
---
# <a name="xamarinessentials-browser"></a>Xamarin.Essentials: Browser

![NuGet-Vorabrelease](~/media/shared/pre-release.png)

Mit der Klasse **Browser** kann eine Anwendung einen Weblink im optimierten und vom System bevorzugten Browser oder im externen Browser öffnen.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-browser"></a>Verwenden des Browsers

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

Die Browser-Funktionalität ruft die `OpenAsync` -Methode mit `Uri` und `BrowserLaunchMode` auf.

```csharp

public class BrowserTest
{
    public async Task OpenBrowser(Uri uri)
    {
        await Browser.OpenAsync(uri, BrowserLaunchMode.SystemPreferred);
    }
}
```

## <a name="platform-implementation-specifics"></a>Besonderheiten bei der Plattformimplementierung

# <a name="androidtabandroid"></a>[Android](#tab/android)

Der Startmodus bestimmt, wie der Browser gestartet wird:

## <a name="system-preferred"></a>Systempräferenz

Mit den [benutzerdefinierten Chrome-Registerkarten](https://developer.chrome.com/multidevice/android/customtabs) wird versucht, den URI zu laden und Navigationsinformationen abzurufen.

## <a name="external"></a>Extern

Mit `Intent` wird angefordert, dass der URI über den normalen Systembrowser geöffnet wird.

# <a name="iostabios"></a>[iOS](#tab/ios)

## <a name="system-preferred"></a>Systempräferenz

Mit [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) werden der URI geladen und Navigationsinformationen abgerufen.

## <a name="external"></a>Extern

Mit dem Standardcode `OpenUrl` in der Hauptanwendung wird der Standardbrowser außerhalb der Anwendung gestartet.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Der Standardbrowser des Benutzers wird immer gestartet, unabhängig von `BrowserLaunchMode`.

--------------

## <a name="api"></a>API

- [Browser-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Browser)
- [Browser-API-Dokumentation](xref:Xamarin.Essentials.Browser)
