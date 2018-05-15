---
title: Xamarin.Essentials SMS
description: Die Sms-Klasse ermöglicht einer Anwendung die Standard-SMS-Anwendung mit einer angegebenen Meldung an einen Empfänger senden zu öffnen.
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 1f466e01d9b1e48849e60a364cf1d49d9cbdcb37
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-sms"></a>Xamarin.Essentials SMS

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
