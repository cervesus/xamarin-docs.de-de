---
title: 'Xamarin.Essentials: E-Mail'
description: Die Klasse „Email“ in Xamarin.Essentials ermöglicht einer Anwendung das Öffnen der E-Mail-Standardanwendung mit speziellen Informationen, einschließlich Betreff, Nachrichtentext und Empfänger (to, cc, bcc).
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: c8d4a83caf6832f911193067324915fd6226b380
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50674963"
---
# <a name="xamarinessentials-email"></a>Xamarin.Essentials: E-Mail

![NuGet-Vorabrelease](~/media/shared/pre-release.png)

Die Klasse **Email** ermöglicht einer Anwendung das Öffnen der E-Mail-Standardanwendung mit speziellen Informationen, einschließlich Betreff, Nachrichtentext und Empfänger (to, cc, bcc).

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-email"></a>Verwenden von E-Mail

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

Die E-Mail-Funktion ruft die `ComposeAsync`-Methode mit einer `EmailMessage` auf, die Informationen zur E-Mail enthält:

```csharp
public class EmailTest
{
    public async Task SendEmail(string subject, string body, List<string> recipients)
    {
        try
        {
            var message = new EmailMessage
            {
                Subject = subject,
                Body = body,
                To = recipients,
                //Cc = ccRecipients,
                //Bcc = bccRecipients
            };
            await Email.ComposeAsync(message);
        }
        catch (FeatureNotSupportedException fbsEx)
        {
            // Email is not supported on this device
        }
        catch (Exception ex)
        {
            // Some other exception occurred
        }
    }
}
```

## <a name="api"></a>API

- [Email-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Email)
- [Email-API-Dokumentation](xref:Xamarin.Essentials.Email)
