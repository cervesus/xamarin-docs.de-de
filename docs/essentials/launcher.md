---
title: 'Xamarin.Essentials: Startprogramm'
description: Mit der Launcher-Klasse in Xamarin.Essentials kann eine Anwendung einen URI über das System öffnen.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 07/25/2018
ms.openlocfilehash: 8f5ef8ef97999e9e85944d9fa9d4e57660779a48
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115782"
---
# <a name="xamarinessentials-launcher"></a>Xamarin.Essentials: Startprogramm

![NuGet-Vorabrelease](~/media/shared/pre-release.png)

Mit der **Launcher**-Klasse kann eine Anwendung einen URI durch das System öffnen. Das wird häufig für Deep Linking für benutzerdefinierte URI-Schemas einer andere Anwendung verwendet. Wenn Sie im Browser eine Website öffnen möchten, verweisen Sie auf die **[Browser](open-browser.md)** API.

## <a name="using-launcher"></a>Verwenden des Startprogramms

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

Um die Launcher-Funktionalität zu nutzen, rufen Sie die Methode `OpenAsync` auf, und geben Sie `string` oder `Uri` zum Öffnen an. Optional lässt sich mit der `CanOpenAsync`-Methode überprüfen, ob das URI-Schema von einer Anwendung auf dem Gerät verarbeitet werden kann.

```csharp
public class LauncherTest
{
    public async Task OpenRideShareAsync()
    {
        var supportsUri = await Launcher.CanOpenAsync("lyft://");
        if (supportsUri)
            await Launcher.OpenAsync("lyft://ridetype?id=lyft_line");
    }
}
```

## <a name="platform-differences"></a>Plattformunterschiede

# <a name="androidtabandroid"></a>[Android](#tab/android)

Die von `CanOpenAsync` zurückgegebene Aufgabe wird sofort abgeschlossen.

# <a name="iostabios"></a>[iOS](#tab/ios)

Wenn die Zielanwendung auf diesem Gerät noch nie von `OpenAsync` aus Ihrer Anwendung geöffnet wurde, fordert iOS den Benutzer einmal auf, Ihrer Anwendung das Öffnen zu erlauben.

Die von `CanOpenAsync` zurückgegebene Aufgabe wird sofort abgeschlossen.

Weitere Informationen zur iOS-Implementierung [finden Sie hier](https://developer.xamarin.com/api/member/UIKit.UIApplication.CanOpenUrl/p/Foundation.NSUrl/).

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Keine Plattformunterschiede.

-----

## <a name="api"></a>API

- [Launcher-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Launcher)
- [Launcher-API-Dokumentation](xref:Xamarin.Essentials.Launcher)
