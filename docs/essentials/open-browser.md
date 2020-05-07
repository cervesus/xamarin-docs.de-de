---
title: 'Xamarin.Essentials: „Browser öffnen“'
description: Mit der Browser-Klasse in Xamarin.Essentials kann eine Anwendung einen Weblink im optimierten und vom System bevorzugten Browser oder im externen Browser öffnen.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 04/02/2019
ms.custom: video
ms.openlocfilehash: 26c4c2487bf2ec32ade3ceef9db73b4809053fec
ms.sourcegitcommit: 154a3e7aec775327565bb54eda1a610976af1d6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2020
ms.locfileid: "82624721"
---
# <a name="xamarinessentials-browser"></a>Xamarin.Essentials: Browser

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

Diese Methode wird zurückgegeben, nachdem der Browser _gestartet_ wurde und nicht notwendigerweise durch den Benutzer _geschlossen_ wurde.  Das `bool`-Ergebnis gibt an, ob der Start erfolgreich war oder nicht.

## <a name="customization"></a>Anpassung

Bei der Verwendung des Standardbrowsers stehen mehrere Anpassungsoptionen für iOS und Android zur Verfügung. Dazu gehört die `TitleMode`-Eigenschaft (nur Android) und bevorzugte Farboptionen für die angezeigten `Toolbar`-(iOS und Android) und `Controls`-Elemente (nur iOS). 

Diese Optionen werden beim Aufruf von `OpenAsync` mithilfe von `BrowserLaunchOptions` festgelegt.

```csharp
await Browser.OpenAsync(uri, new BrowserLaunchOptions
                {
                    LaunchMode = BrowserLaunchMode.SystemPreferred,
                    TitleMode = BrowserTitleMode.Show,
                    PreferredToolbarColor = Color.AliceBlue,
                    PreferredControlColor = Color.Violet
                });
```

![Browseroptionen](images/browser-options.png)

## <a name="platform-implementation-specifics"></a>Besonderheiten bei der plattformspezifischen Implementierung

# <a name="android"></a>[Android](#tab/android)

Der Startmodus bestimmt, wie der Browser gestartet wird:

## <a name="system-preferred"></a>Systempräferenz

Mit den [benutzerdefinierten Chrome-Registerkarten](https://developer.chrome.com/multidevice/android/customtabs) wird versucht, den URI zu laden und Navigationsinformationen abzurufen.

## <a name="external"></a>Extern

Mit `Intent` wird angefordert, dass der URI über den normalen Systembrowser geöffnet wird.

# <a name="ios"></a>[iOS](#tab/ios)

## <a name="system-preferred"></a>Systempräferenz

Mit [SFSafariViewController](xref:SafariServices.SFSafariViewController) werden der URI geladen und Navigationsinformationen abgerufen.

## <a name="external"></a>Extern

Mit dem Standardcode `OpenUrl` in der Hauptanwendung wird der Standardbrowser außerhalb der Anwendung gestartet.

# <a name="uwp"></a>[UWP](#tab/uwp)

Der Standardbrowser des Benutzers wird immer gestartet, unabhängig von `BrowserLaunchMode`.

--------------

## <a name="api"></a>API

- [Browser-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Browser)
- [Browser-API-Dokumentation](xref:Xamarin.Essentials.Browser)

## <a name="related-video"></a>Zugehörige Videos

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Open-Browser-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
