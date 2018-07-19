---
title: 'Xamarin.Essentials: Wählhilfe'
description: Die PhoneDialer-Klasse in Xamarin.Essentials ermöglicht einer Anwendung, eine Telefonnummer in das Einwählprogramm öffnen
ms.assetid: E7457942-4D7B-4195-A2FF-417919B9537F
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 34a6c80836d8cb42b1f8fd95718fe248d4701c0f
ms.sourcegitcommit: 7f2e44e6f628753e06a5fe2a3076fc2ec5baa081
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2018
ms.locfileid: "39130792"
---
# <a name="xamarinessentials-phone-dialer"></a>Xamarin.Essentials: Wählhilfe

![Vorabversionen von NuGet](~/media/shared/pre-release.png)

Die **PhoneDialer** Klasse ermöglicht einer Anwendung, eine Telefonnummer in das Einwählprogramm zu öffnen.

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
