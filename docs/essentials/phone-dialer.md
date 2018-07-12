---
title: 'Xamarin.Essentials: Wählhilfe'
description: Die PhoneDialer-Klasse in Xamarin.Essentials ermöglicht eine Anwendung auf einen Weblink im bevorzugten Browser optimierten System oder der externen Browser öffnen.
ms.assetid: E7457942-4D7B-4195-A2FF-417919B9537F
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 6733e43ed4174d1dd78b2e8f70268eb54adadb98
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831396"
---
# <a name="xamarinessentials-phone-dialer"></a>Xamarin.Essentials: Wählhilfe

![Vorabversionen von NuGet](~/media/shared/pre-release.png)

Die **PhoneDialer** Klasse ermöglicht einer Anwendung auf einen Weblink im bevorzugten Browser optimierten System oder der externen Browser öffnen.

## <a name="using-phone-dialer"></a>Verwenden der Wählhilfe

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Die Funktionalität der Wählhilfe erfolgt durch Aufrufen der `Open` Methode mit einer Telefonnummer, um das Einwählprogramm mit zu öffnen. Wenn `Open` angefordert wird, die API versucht automatisch, zum Formatieren der Zahl, die basierend auf den Ländercode, falls angegeben.

```csharp
public class PhoneDialerTest
{
    public async Task PlacePhoneCall(string number)
    {
        try
        {
            PhoneDialer.Open(number);
        }
        catch (ArgumentNullException anEx)
        {
            // Number was null or white space
        }
        catch (FeatureNotSupportedException ex)
        {
            // Phone Dialer is not supported on this device.
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

## <a name="api"></a>API

- [Wählhilfe Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/PhoneDialer)
- [Phone Einwählprogramm-API-Dokumentation](xref:Xamarin.Essentials.PhoneDialer)
