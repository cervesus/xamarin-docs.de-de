---
title: 'Xamarin.Essentials: SMS'
description: Mit der SMS-Klasse in Xamarin.Essentials kann eine Anwendung die Standard-SMS-Anwendung mit einer bestimmten Nachricht zum Senden an einen Empfänger öffnen.
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: d0655f3e687750e0fca626fb017096a946c0abb3
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2018
ms.locfileid: "52898770"
---
# <a name="xamarinessentials-sms"></a>Xamarin.Essentials: SMS

Mit der **SMS**-Klasse kann eine Anwendung die Standard-SMS-Anwendung mit einer bestimmten Nachricht zum Senden an einen Empfänger öffnen.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

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
