---
title: ''Xamarin.Essentials: SMS'' description: 'Mit der SMS-Klasse in Xamarin.Essentials kann eine Anwendung die Standard-SMS-Anwendung mit einer bestimmten Nachricht zum Senden an einen Empfänger öffnen.'
ms.assetid: author: ms.custom: ms.author: ms.date: no-loc:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

---

# <a name="xamarinessentials-sms"></a>Xamarin.Essentials: SMS

Mit der **SMS**-Klasse kann eine Anwendung die Standard-SMS-Anwendung mit einer bestimmten Nachricht zum Senden an einen Empfänger öffnen.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-sms"></a>Verwenden von SMS

Fügen Sie in Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

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
