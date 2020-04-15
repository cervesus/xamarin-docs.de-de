---
title: 'Xamarin.Essentials: E-Mail'
description: Die Klasse „Email“ in Xamarin.Essentials ermöglicht einer Anwendung das Öffnen der E-Mail-Standardanwendung mit speziellen Informationen, einschließlich Betreff, Nachrichtentext und Empfänger (to, cc, bcc).
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.author: jamont
ms.date: 08/20/2019
ms.openlocfilehash: cccbe5f539e2807e749433623e938438e67965e8
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "70060097"
---
# <a name="xamarinessentials-email"></a>Xamarin.Essentials: E-Mail

Die Klasse **Email** ermöglicht einer Anwendung das Öffnen der E-Mail-Standardanwendung mit speziellen Informationen, einschließlich Betreff, Nachrichtentext und Empfänger (to, cc, bcc).

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

> [!TIP]
> Wenn Sie die E-Mail-API unter iOS verwenden möchten, müssen Sie sie auf einem physischen Gerät ausführen, andernfalls wird eine Ausnahme ausgelöst.

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

## <a name="file-attachments"></a>Dateianlagen

Mithilfe dieses Features kann eine App Dateien in E-Mail-Clients auf dem Gerät per E-Mail versenden. Xamarin.Essentials erkennt automatisch den Dateityp (MIME) und fordert an, dass die Datei als Anlage hinzugefügt wird. Jeder E-Mail-Client ist anders und unterstützt möglicherweise nur bestimmte Dateierweiterungen oder überhaupt keine.

Hier sehen Sie ein Beispiel, wie Text auf einen Datenträger geschrieben wird und als E-Mail-Anlage hinzugefügt wird:

```csharp
var message = new EmailMessage
{
    Subject = "Hello",
    Body = "World",
};

var fn = "Attachment.txt";
var file = Path.Combine(FileSystem.CacheDirectory, fn);
File.WriteAllText(file, "Hello World");

message.Attachments.Add(new EmailAttachment(file));

await Email.ComposeAsync(message);
```

## <a name="platform-differences"></a>Plattformunterschiede

# <a name="android"></a>[Android](#tab/android)

Nicht alle E-Mail-Clients für Android unterstützen `Html`. Da es keine Möglichkeit gibt, dies zu erkennen, wird beim Versenden von E-Mails die Verwendung von `PlainText` empfohlen.

# <a name="ios"></a>[iOS](#tab/ios)

Keine Plattformunterschiede.

# <a name="uwp"></a>[UWP](#tab/uwp)

Unterstützt nur `PlainText`, da die Ausnahme `FeatureNotSupportedException` ausgelöst wird, wenn `BodyFormat` versucht, `Html` zu senden.

Nicht alle E-Mail-Clients unterstützen das Senden von Anhängen. Weitere Informationen finden Sie in der [Dokumentation](https://docs.microsoft.com/windows/uwp/contacts-and-calendar/sending-email).

-----

## <a name="api"></a>API

- [Email-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Email)
- [Email-API-Dokumentation](xref:Xamarin.Essentials.Email)
