---
title: 'Xamarin.Essentials: Wählhilfe'
description: Mit der Klasse „PhoneDialer“ in Xamarin.Essentials kann eine Anwendung eine Telefonnummer im Wählprogramm öffnen.
ms.assetid: E7457942-4D7B-4195-A2FF-417919B9537F
author: jamesmontemagno
ms.author: jamont
ms.date: 07/02/2019
ms.openlocfilehash: 0b00307759d95b8e2efe27bd9a17d2c6efd3d5c0
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "67538675"
---
# <a name="xamarinessentials-phone-dialer"></a>Xamarin.Essentials: Wählhilfe

Mit der Klasse **PhoneDialer** in Xamarin.Essentials kann eine Anwendung eine Telefonnummer im Wählprogramm öffnen.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-phone-dialer"></a>Verwenden der Wählhilfe

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

Die Wählhilfe-Funktion ruft die Methode `Open` mit einer Telefonnummer auf, mit der das Wählprogramm geöffnet werden soll. Bei Anforderung von `Open` versucht die API automatisch, die Nummer anhand des angegebenen Ländercodes zu formatieren.

```csharp
public class PhoneDialerTest
{
    public void PlacePhoneCall(string number)
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

- [PhoneDialer-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/PhoneDialer)
- [PhoneDialer-API-Dokumentation](xref:Xamarin.Essentials.PhoneDialer)
