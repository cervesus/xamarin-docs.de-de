---
title: 'Xamarin.Essentials: SMS'
description: Mit der SMS-Klasse in Xamarin.Essentials kann eine Anwendung die Standard-SMS-Anwendung mit einer bestimmten Nachricht zum Senden an einen Empfänger öffnen.
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: fa55a17e99a11861b98c4d515df882ed3af58a0b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50102736"
---
# <a name="xamarinessentials-sms"></a>Xamarin.Essentials: SMS

![NuGet-Vorabrelease](~/media/shared/pre-release.png)

Mit der **SMS**-Klasse kann eine Anwendung die Standard-SMS-Anwendung mit einer bestimmten Nachricht zum Senden an einen Empfänger öffnen.

## <a name="using-sms"></a>Verwenden von SMS

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

Die SMS-Funktionalität ruft in der `ComposeAsync`-Methode `SmsMessage` auf, worin der Empfänger sowie der Text der Nachricht enthalten sind (beides optional).

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

Außerdem können Sie mehrere Empfänger für `SmsMessage` angeben:

```csharp
public class SmsTest
{
    public async Task SendSms(string messageText, string[] recipients)
    {
        try
        {
            var message = new SmsMessage(messageText, recipients);
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
- [SMS- API-Dokumentation](xref:Xamarin.Essentials.Sms)
