---
title: 'Xamarin.Essentials: SMS'
description: Die Sms-Klasse in Xamarin.Essentials ermöglicht eine Anwendung, öffnen Sie die standardmäßige SMS-Anwendung mit einer angegebenen Meldung an einen Empfänger zu senden.
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: a93a67b83ea8f435a5e3ad5d26e1d6cbbb7092f7
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815596"
---
# <a name="xamarinessentials-sms"></a>Xamarin.Essentials: SMS

![Vorabversionen von NuGet](~/media/shared/pre-release.png)

Die **Sms** Klasse ermöglicht einer Anwendung, öffnen Sie die standardmäßige SMS-Anwendung mit einer angegebenen Meldung an einen Empfänger zu senden.

## <a name="using-sms"></a>Mithilfe von Sms

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Die SMS-Funktionen erfolgt durch Aufrufen der `ComposeAsync` Methode eine `SmsMessage` , enthält die Nachricht den Empfänger und der Text der Nachricht, beide optional sind.

```csharp
public class SmsTest
{
    public async Task SendSms(string messageText, string recipient)
    {
        try
        {
            var message = new SmsMessage(messageText, recipient);
            await Sms.ComposeAsync(message);
        }
        catch (FeatureNotSupportedException ex)
        {
            // Sms is not supported on this device.
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

## <a name="api"></a>API

- [SMS-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Sms)
- [SMS-API-Dokumentation](xref:Xamarin.Essentials.Sms)
