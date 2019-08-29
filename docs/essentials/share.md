---
title: 'Xamarin.Essentials: Freigeben'
description: Die Share-Klasse in Xamarin.Essentials ermöglicht einer Anwendung das Teilen von Daten wie Text und Weblinks mit anderen Anwendungen auf dem Gerät.
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 08/20/2019
ms.custom: video
ms.openlocfilehash: b889573c0cefbd692b1e879c1612a143d4bb7c99
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70120752"
---
# <a name="xamarinessentials-share"></a>Xamarin.Essentials: Freigeben

Die **Share**-Klasse ermöglicht einer Anwendung das Teilen von Daten wie Text und Weblinks mit anderen Anwendungen auf dem Gerät.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-share"></a>Verwenden von „Teilen“

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

Die „Teilen“-Funktion ruft die `RequestAsync`-Methode mit einer Datenanforderungsnutzlast auf, die Informationen zum Teilen mit anderen Anwendungen enthält. Text und URI können kombiniert werden, und bei jeder Plattform erfolgt die Filterung anhand des Inhalts.

```csharp

public class ShareTest
{
    public async Task ShareText(string text)
    {
        await Share.RequestAsync(new ShareTextRequest
            {
                Text = text,
                Title = "Share Text"
            });
    }

    public async Task ShareUri(string uri)
    {
        await Share.RequestAsync(new ShareTextRequest
            {
                Uri = uri,
                Title = "Share Web Link"
            });
    }
}
```

Benutzeroberfläche zur Freigabe für eine externe Anwendung, die beim Senden der Anforderung angezeigt wird:

![Freigeben](images/share.png)

## <a name="files"></a>Dateien

Mithilfe dieses Features kann eine App Dateien für andere Anwendungen auf dem Gerät freigeben. Xamarin.Essentials erkennt automatisch den Dateityp (MIME) und fordert eine Dateifreigabe an. Jede Plattform unterstützt möglicherweise nur bestimmte Dateierweiterungen.

Hier sehen Sie ein Beispiel, wie Text auf einen Datenträger geschrieben wird und für andere Apps freigegeben wird:

```csharp
var fn =  "Attachment.txt";
var file = Path.Combine(FileSystem.CacheDirectory, fn);
File.WriteAllText(file, "Hello World");

await Share.RequestAsync(new ShareFileRequest
{
    Title = Title,
    File = new ShareFile(file)
});
```

## <a name="platform-differences"></a>Plattformunterschiede

# <a name="androidtabandroid"></a>[Android](#tab/android)

- Die Eigenschaft `Subject` wird für den gewünschten Betreff einer Nachricht verwendet.

# <a name="iostabios"></a>[iOS](#tab/ios)

- `Subject` nicht verwendet.
- `Title` nicht verwendet.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

- Als `Title` wird standardmäßig der Anwendungsname verwendet, wenn kein Titel festgelegt wurde.
- `Subject` nicht verwendet.

-----


## <a name="api"></a>API

- [Share-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Share)
- [Share-API-Dokumentation](xref:Xamarin.Essentials.Share)

## <a name="related-video"></a>Zugehörige Videos

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Share-Essential-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
