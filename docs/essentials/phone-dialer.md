---
title: Xamarin.Essentials Wählhilfe
description: Die Klasse PhoneDialer ermöglicht einer Anwendung auf einen Weblink im optimierten System bevorzugten Browser oder den externen Browser öffnen.
ms.assetid: E7457942-4D7B-4195-A2FF-417919B9537F
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 112cc305457413ad057e390d46c5a765ea29514f
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-phone-dialer"></a>Xamarin.Essentials Wählhilfe

![Vorabversion NuGet](~/media/shared/pre-release.png)

Die **PhoneDialer** -Klasse kann eine Anwendung auf einen Weblink im optimierten System bevorzugten Browser oder den externen Browser öffnen.

## <a name="using-phone-dialer"></a>Verwenden der Wählhilfe

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Die Funktionsweise der Wählhilfe durch Aufrufen der `Open` Methode zum Öffnen der Wählhilfe mit einer Telefonnummer. Wenn `Open` angefordert wird, die die API versucht automatisch zum Formatieren der Zahl, die basierend auf der Landeskennzahl, falls angegeben.

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
- [Phone Wählhilfe-API-Dokumentation](xref:Xamarin.Essentials.PhoneDialer)
