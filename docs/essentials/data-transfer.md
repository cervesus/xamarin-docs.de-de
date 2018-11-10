---
title: 'Xamarin.Essentials: Datenübertragung'
description: Die Klasse „DataTransfer“ in Xamarin.Essentials ermöglicht einer Anwendung die Freigabe von Daten wie Text und Weblinks zu anderen Anwendungen auf dem Gerät.
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 179d4327aa768e7aa2c81dbbffd694d078327400
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50674833"
---
# <a name="xamarinessentials-data-transfer"></a>Xamarin.Essentials: Datenübertragung

![NuGet-Vorabrelease](~/media/shared/pre-release.png)

Die Klasse **DataTransfer** ermöglicht einer Anwendung die Freigabe von Daten wie Text und Weblinks zu anderen Anwendungen auf dem Gerät.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-data-transfer"></a>Verwenden der Datenübertragung

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

Die Datenübertragungsfunktion ruft die `RequestAsync`-Methode mit einer Datenanforderungsnutzlast auf, die Informationen zur Freigabe für andere Anwendungen enthält. Text und URI können kombiniert werden, und bei jeder Plattform erfolgt die Filterung anhand des Inhalts.

```csharp

public class DataTransferTest
{
    public async Task ShareText(string text)
    {
        await DataTransfer.RequestAsync(new ShareTextRequest
            {
                Text = text,
                Title = "Share Text"
            });
    }

    public async Task ShareUri(string uri)
    {
        await DataTransfer.RequestAsync(new ShareTextRequest
            {
                Uri = uri,
                Title = "Share Web Link"
            });
    }
}
```

Benutzeroberfläche zur Freigabe für eine externe Anwendung, die beim Senden der Anforderung angezeigt wird:

![Datenübertragung](data-transfer-images/data-transfer.png)

## <a name="platform-differences"></a>Plattformunterschiede

# <a name="androidtabandroid"></a>[Android](#tab/android)

* Die Eigenschaft `Subject` wird für den gewünschten Betreff einer Nachricht verwendet.

# <a name="iostabios"></a>[iOS](#tab/ios)

* `Subject` nicht verwendet.
* `Title` nicht verwendet.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

* Als `Title` wird standardmäßig der Anwendungsname verwendet, wenn kein Titel festgelegt wurde.
* `Subject` nicht verwendet.

-----

## <a name="api"></a>API

- [DataTransfer-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DataTransfer)
- [DataTransfer-API-Dokumentation](xref:Xamarin.Essentials.DataTransfer)
