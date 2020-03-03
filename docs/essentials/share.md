---
title: 'Xamarin.Essentials: Freigeben'
description: Die Share-Klasse in Xamarin.Essentials ermöglicht einer Anwendung das Teilen von Daten wie Text und Weblinks mit anderen Anwendungen auf dem Gerät.
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 01/06/2020
ms.custom: video
ms.openlocfilehash: f964967dce0dbb7e49d52a7f865b0fe8a9957bbe
ms.sourcegitcommit: fec87846fcb262fc8b79774a395908c8c8fc8f5b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2020
ms.locfileid: "77545200"
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

## <a name="presentation-location"></a>Präsentationsspeicherort

Wenn Sie unter iPadOS eine Freigabe anfordern, haben Sie die Möglichkeit, die Präsentation in einem Popover-Steuerelement zu zeigen. Sie können den Speicherort mithilfe der `PresentationSourceBounds`-Eigenschaft angeben:

```csharp
await Share.RequestAsync(new ShareFileRequest
{
    Title = Title,
    File = new ShareFile(file),
    PresentationSourceBounds = DeviceInfo.Platform== DevicePlatform.iOS && DeviceInfo.Idiom == DeviceIdiom.Tablet
                            ? new System.Drawing.Rectangle(0, 20, 0, 0)
                            : System.Drawing.Rectangle.Empty
});
```

## <a name="platform-differences"></a>Plattformunterschiede

# <a name="android"></a>[Android](#tab/android)

- Die Eigenschaft `Subject` wird für den gewünschten Betreff einer Nachricht verwendet.

# <a name="ios"></a>[iOS](#tab/ios)

- `Subject` nicht verwendet.
- `Title` nicht verwendet.

# <a name="uwp"></a>[UWP](#tab/uwp)

- Als `Title` wird standardmäßig der Anwendungsname verwendet, wenn kein Titel festgelegt wurde.
- `Subject` nicht verwendet.

-----

## <a name="api"></a>API

- [Share-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Share)
- [Share-API-Dokumentation](xref:Xamarin.Essentials.Share)

## <a name="related-video"></a>Zugehörige Videos

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Share-Essential-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
