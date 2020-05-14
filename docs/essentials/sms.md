---
title: 'Xamarin.Essentials: SMS'
description: Mit der SMS-Klasse in Xamarin.Essentials kann eine Anwendung die Standard-SMS-Anwendung mit einer bestimmten Nachricht zum Senden an einen Empfänger öffnen.
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
author: jamesmontemagno
ms.custom: video
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: 38651b8e29a2119f777fdbcdd82c9a8277c02497
ms.sourcegitcommit: 83cf2a4d99546751c6394510a463a2b2a8bf75b8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/13/2020
ms.locfileid: "83149907"
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
            var message = new SmsMessage(messageText, new []{ recipient });
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

## <a name="related-video"></a>Zugehörige Videos

> [!Video https://channel9.msdn.com/Shows/XamarinShow/SMS-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
