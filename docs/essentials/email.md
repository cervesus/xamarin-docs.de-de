---
title: 'Xamarin.Essentials: E-Mail'
description: Die Klasse „Email“ in Xamarin.Essentials ermöglicht einer Anwendung das Öffnen der E-Mail-Standardanwendung mit speziellen Informationen, einschließlich Betreff, Nachrichtentext und Empfänger (to, cc, bcc).
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.author: jamont
ms.date: 04/02/2019
ms.openlocfilehash: f2c275260625fe3842b4473e404f49c71d1d28ae
ms.sourcegitcommit: 9f37dc00c2adab958025ad1cdba9c37f0acbccd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2019
ms.locfileid: "69012494"
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

![Feature der Vorschauversion](~/media/shared/preview.png)

Das Versenden von Dateien per E-Mail steht in einer experimentellen Vorschauversion in der Xamarin.Essentials-Version 1.1.0 zur Verfügung. Mithilfe dieses Features kann eine App Dateien in E-Mail-Clients auf dem Gerät per E-Mail versenden. Wenn Sie dieses Feature aktivieren möchten, legen Sie die folgende Eigenschaft im Startcode Ihrer App fest:

```csharp
ExperimentalFeatures.Enable(ExperimentalFeatures.EmailAttachments);
```

Nach der Aktivierung des Features kann jede Datei per E-Mail versendet werden. Xamarin.Essentials erkennt automatisch den Dateityp (MIME) und fordert an, dass die Datei als Anlage hinzugefügt wird. Jeder E-Mail-Client ist anders und unterstützt möglicherweise nur bestimmte Dateierweiterungen oder überhaupt keine.

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

# <a name="androidtabandroid"></a>[Android](#tab/android)

Nicht alle E-Mail-Clients für Android unterstützen `Html`. Da es keine Möglichkeit gibt, dies zu erkennen, wird beim Versenden von E-Mails die Verwendung von `PlainText` empfohlen.

# <a name="iostabios"></a>[iOS](#tab/ios)

Keine Plattformunterschiede.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Unterstützt nur `PlainText`, da die Ausnahme `FeatureNotSupportedException` ausgelöst wird, wenn `BodyFormat` versucht, `Html` zu senden.

Nicht alle E-Mail-Clients unterstützen das Senden von Anhängen. Weitere Informationen finden Sie in der [Dokumentation](https://docs.microsoft.com/windows/uwp/contacts-and-calendar/sending-email).

-----

## <a name="api"></a>API

- [Email-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Email)
- [Email-API-Dokumentation](xref:Xamarin.Essentials.Email)
