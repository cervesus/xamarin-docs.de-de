---
title: 'Xamarin.Essentials: Wählhilfe'
description: Mit der Klasse „PhoneDialer“ in Xamarin.Essentials kann eine Anwendung eine Telefonnummer im Wählprogramm öffnen.
ms.assetid: E7457942-4D7B-4195-A2FF-417919B9537F
author: jamesmontemagno
ms.custom: video
ms.author: jamont
ms.date: 07/02/2019
ms.openlocfilehash: a955399b40f26d4a03a4047d56f7bebe3ad5b5c4
ms.sourcegitcommit: 83cf2a4d99546751c6394510a463a2b2a8bf75b8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/13/2020
ms.locfileid: "83150046"
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

## <a name="related-video"></a>Zugehörige Videos

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Phone-Dialer-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
