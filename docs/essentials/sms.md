---
title: 'Xamarin.Essentials: SMS'
description: Die Sms-Klasse in Xamarin.Essentials ermöglicht einer Anwendung die Standard-SMS-Anwendung mit einer angegebenen Meldung an einen Empfänger senden zu öffnen.
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: a93a67b83ea8f435a5e3ad5d26e1d6cbbb7092f7
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783086"
---
# <a name="xamarinessentials-sms"></a>Xamarin.Essentials: SMS

![Vorabversion NuGet](~/media/shared/pre-release.png)

Die **Sms** Klasse ermöglicht einer Anwendung, öffnen Sie die SMS-standardanwendung mit einer angegebenen Meldung an einen Empfänger zu senden.

## <a name="using-sms"></a>Mithilfe von Sms

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Der SMS-Funktionalität kann durch Aufrufen der `ComposeAsync` Methode eine `SmsMessage` enthält Empfänger der Nachricht und der Text der Nachricht, beide optional sind.

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
