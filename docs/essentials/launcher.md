---
title: 'Xamarin.Essentials: Startprogramm'
description: Mit der Launcher-Klasse in Xamarin.Essentials kann eine Anwendung einen URI über das System öffnen.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: 26ab3687e9bcfecbd003ddd0a97f3e46a0cd92b2
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "58870052"
---
# <a name="xamarinessentials-launcher"></a>Xamarin.Essentials: Startprogramm

Mit der **Launcher**-Klasse kann eine Anwendung einen URI durch das System öffnen. Das wird häufig für Deep Linking für benutzerdefinierte URI-Schemas einer andere Anwendung verwendet. Wenn Sie im Browser eine Website öffnen möchten, verweisen Sie auf die **[Browser](open-browser.md)** API.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

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

Weitere Informationen zur iOS-Implementierung [finden Sie hier](xref:UIKit.UIApplication.CanOpenUrl*).

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Keine Plattformunterschiede.

-----

## <a name="api"></a>API

- [Launcher-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Launcher)
- [Launcher-API-Dokumentation](xref:Xamarin.Essentials.Launcher)
