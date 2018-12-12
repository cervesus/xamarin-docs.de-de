---
title: 'Xamarin.Essentials: E-Mail'
description: Die Klasse „Email“ in Xamarin.Essentials ermöglicht einer Anwendung das Öffnen der E-Mail-Standardanwendung mit speziellen Informationen, einschließlich Betreff, Nachrichtentext und Empfänger (to, cc, bcc).
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: d7d2536fca32fe3ae9f9692031645c42edb4ea61
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2018
ms.locfileid: "52898660"
---
# <a name="xamarinessentials-email"></a>Xamarin.Essentials: E-Mail

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


## <a name="platform-differences"></a>Plattformunterschiede

# <a name="androidtabandroid"></a>[Android](#tab/android)

Nicht alle E-Mail-Clients für Android unterstützen `Html`. Da es keine Möglichkeit gibt, dies zu erkennen, wird beim Versenden von E-Mails die Verwendung von `PlainText` empfohlen.

# <a name="iostabios"></a>[iOS](#tab/ios)

Keine Plattformunterschiede.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Unterstützt nur `PlainText`, da die Ausnahme `FeatureNotSupportedException` ausgelöst wird, wenn `BodyFormat` versucht, `Html` zu senden.

-----

## <a name="api"></a>API

- [Email-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Email)
- [Email-API-Dokumentation](xref:Xamarin.Essentials.Email)
