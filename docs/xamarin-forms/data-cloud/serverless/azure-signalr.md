---
title: Azure SignalR-Dienst mit Xamarin.Forms
description: Erste Schritte mit Azure SignalR Service und Azure Functions mit Xamarin.Forms
ms.prod: xamarin
ms.assetid: 1B9A69EF-C200-41BF-B098-D978D7F9CD8F
author: profexorgeek
ms.author: jusjohns
ms.date: 06/07/2019
ms.openlocfilehash: d79ffb844a65c145fd6cb22a5a3e1dfc7a6bfe41
ms.sourcegitcommit: 0f78ec17210b915b43ddab75937de8063e472c70
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2019
ms.locfileid: "67418123"
---
# <a name="azure-signalr-service-with-xamarinforms"></a>Azure SignalR-Dienst mit Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://github.com/xamarin/xamarin-forms-samples/tree/master/WebServices/AzureSignalR)

ASP.NET Core SignalR handelt es sich um ein Anwendungsmodell, die das Hinzufügen von Kommunikation in Echtzeit zu Anwendungen vereinfacht. Der Azure SignalR Service ermöglicht eine schnelle Entwicklung und Bereitstellung skalierbarer SignalR-Anwendungen. Azure-Funktionen sind kurzlebige, serverlosen Codemethoden, die zum Formular das ereignisgesteuerte, skalierbare Anwendungen kombiniert werden können.

Dieser Artikel und Beispiel zeigen, wie Azure SignalR Service und Azure Functions mit Xamarin.Forms zum Übermitteln von Nachrichten in Echtzeit an verbundene Clients zu kombinieren.

## <a name="create-an-azure-signalr-service-and-azure-functions-app"></a>Erstellen Sie eine der Azure SignalR Service und Azure Functions-App

Die beispielanwendung umfasst drei Hauptkomponenten: einen Hub Azure SignalR Service, eine Azure Functions-Instanz mit zwei Funktionen und eine mobile Anwendung, die zu senden und Empfangen von Nachrichten kann. Diese Komponenten Zusammenwirken wie folgt aus:

1. Die mobile Anwendung ruft eine **Negotiate** Azure-Funktion zum Abrufen von Informationen zu den SignalR-Hub.
1. Die mobile Anwendung verwendet die Aushandlung-Informationen, um sich mit den SignalR-Hub zu registrieren und bildet eine Verbindung.
1. Nach der Registrierung die mobile Anwendung Nachrichten sendet die **sprechen** Azure-Funktion.
1. Die **sprechen** Funktion übergibt die eingehende Nachricht an den SignalR-Hub.
1. Der SignalR-Hub sendet die Nachricht an alle verbundenen mobilen Anwendungen-Instanzen, einschließlich von den ursprünglichen Absender.

> [!NOTE]
> Die **Negotiate** und **sprechen** Funktionen in diesem Beispiel können lokal mit Visual Studio-2019 und die Azure-Laufzeit-Tools ausgeführt werden. Jedoch des Azure SignalR Service kann nicht lokal emuliert werden, und es ist schwierig, physische oder virtuelle Geräte zu Testzwecken Azure Funktionen lokal gehostet werden können. Es wird empfohlen, dass Sie Azure Functions mit einer Instanz von Azure Functions-App bereitstellen, da Sie plattformübergreifende Tests ermöglicht. Ausführlichere Informationen zur Bereitstellung, finden Sie unter [Bereitstellen von Azure Functions mit Visual Studio-2019](#deploy-azure-functions-with-visual-studio-2019).

### <a name="create-an-azure-signalr-service"></a>Erstellen von Azure SignalR Service

Azure SignalR Service erstellt werden, indem Sie die Auswahl **erstellen Sie eine Ressource** in der linken oberen Ecke des Azure-Portal und die Suche nach **SignalR**. Der Azure SignalR Service kann auf den free-Tarif erstellt werden. Der Azure SignalR Service muss im **serverlose** Dienstmodus. Wenn Sie versehentlich den Standardwert oder einen klassischen Service-Modus auswählen, können Sie es später in den Azure SignalR Service-Eigenschaften ändern.

Der folgende Screenshot zeigt die Erstellung eines neuen Azure SignalR Service:

![Screenshot des Azure SignalR Service-Erstellung im Azure-Portal](azure-signalr-images/azure-signalr-create.png "erstellen eine Azure SignalR Service")

Nach der Erstellung der **Schlüssel** Abschnitt der Azure SignalR Service enthält einen **Verbindungszeichenfolge**, dem wird verwendet, um die Azure-Funktionen-App mit dem SignalR-Hub herstellen. Der folgende Screenshot zeigt, wo Sie die Verbindungszeichenfolge in der Azure SignalR Service finden:

![Screenshot des Azure SignalR-Verbindungszeichenfolge im Azure-Portal](azure-signalr-images/azure-signalr-connection-string.png "Azure SignalR-Verbindungszeichenfolge")

Diese Verbindungszeichenfolge wird verwendet, um [Bereitstellen von Azure Functions mit Visual Studio-2019](#deploy-azure-functions-with-visual-studio-2019).

### <a name="create-an-azure-functions-app"></a>Erstellen einer Azure-Funktionen-App

Um die beispielanwendung zu testen, sollten Sie eine neue Azure-Funktionen-App im Azure-Portal erstellen. Notieren Sie sich die **App-Namen** wie diese URL, in der beispielanwendung verwendet wird **"Constants.cs"** Datei. Der folgende Screenshot zeigt die Erstellung einer neuen Azure-Funktionen-App namens "Xdocsfunctions":

[![Screenshot des Azure Functions-App-Erstellung](azure-signalr-images/azure-functions-app-cropped.png)](azure-signalr-images/azure-functions-app-full.png#lightbox)

Azure-Funktionen können mit einer Instanz von Azure Functions-App aus Visual Studio-2019 bereitgestellt werden. In den folgenden Abschnitten wird die Bereitstellung von zwei Funktionen in der beispielanwendung mit einer Instanz von Azure Functions-App beschrieben.

### <a name="build-azure-functions-in-visual-studio-2019"></a>Erstellen von Azure Functions in Visual Studio-2019

Die beispielanwendung enthält eine Klassenbibliothek mit dem Namen **ChatServer**, darunter zwei serverlose Azure Functions in Dateien namens **Negotiate.cs** und **Talk.cs**.

Die `Negotiate` Funktion antwortet auf webanforderungen mit einem `SignalRConnectionInfo` -Objekt, enthält ein `AccessToken` Eigenschaft und eine `Url` Eigenschaft. Die mobile Anwendung verwendet diese Werte selbst den SignalR-Hub registrieren. Der folgende code zeigt die `Negotiate` Funktion:

```csharp
[FunctionName("Negotiate")]
public static SignalRConnectionInfo GetSignalRInfo(
    [HttpTrigger(AuthorizationLevel.Anonymous,"get",Route = "negotiate")]
    HttpRequest req,
    [SignalRConnectionInfo(HubName = "simplechat")]
    SignalRConnectionInfo connectionInfo)
{
    return connectionInfo;
}
```

Die `Talk` Funktion antwortet auf HTTP POST-Anforderungen, die ein Nachrichtenobjekt in der POST-Text zu bieten. POST-Text wird umgewandelt in ein `SignalRMessage` und an den SignalR-Hub weitergeleitet. Der folgende code zeigt die `Talk` Funktion:

```csharp
[FunctionName("Talk")]
public static async Task<IActionResult> Run(
    [HttpTrigger(
        AuthorizationLevel.Anonymous,
        "post",
        Route = "talk")]
    HttpRequest req,
    [SignalR(HubName = "simplechat")]
    IAsyncCollector<SignalRMessage> questionR,
    ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    try
    {
        string json = await new StreamReader(req.Body).ReadToEndAsync();
        dynamic obj = JsonConvert.DeserializeObject(json);

        var name = obj.name.ToString();
        var text = obj.text.ToString();

        var jObject = new JObject(obj);

        await questionR.AddAsync(
            new SignalRMessage
            {
                Target = "newMessage",
                Arguments = new[] { jObject }
            });

        return new OkObjectResult($"Hello {name}, your message was '{text}'");
    }
    catch (Exception ex)
    {
        return new BadRequestObjectResult("There was an error: " + ex.Message);
    }
}
```

Weitere Informationen zu Azure Functions und Azure Functions-Apps finden Sie unter [Dokumentation zu Azure Functions](/azure/azure-functions/).

### <a name="deploy-azure-functions-with-visual-studio-2019"></a>Bereitstellen von Azure Functions mit Visual Studio-2019

Visual Studio-2019 können Sie Funktionen für eine Azure Functions-App bereitstellen. Azure gehosteten Funktionen vereinfachen, Cross-Platform zu testen, indem Sie einen zugänglichen Endpunkt für die Testen für alle Geräte bereitstellen.

Mit der rechten Maustaste in der Beispiel-Funktionen-app und auswählen **veröffentlichen** Öffnet das Dialogfeld zum Veröffentlichen von Funktionen in Ihre Azure-Funktions-App. Wenn Sie die vorherigen Schritte zum Einrichten einer Azure-Funktionen-App haben, können Sie **vorhandene auswählen** die Beispielanwendungen, die Ihre Azure-Funktions-App zu veröffentlichen. Der folgende Screenshot zeigt die veröffentlichen Dialogfeldoptionen in Visual Studio-2019:

![Dialogfeld "Veröffentlichen Optionen" in Visual Studio-2019](azure-signalr-images/vs-publish-target.png "Veröffentlichungsoptionen in Visual Studio-2019")

Nachdem Sie sich mit Ihrem Microsoft-Konto angemeldet haben, können Sie suchen und wählen Sie Ihre Azure-Funktionen-App als Ziel für die Veröffentlichung. Der folgende Screenshot zeigt ein Beispiel für Azure Functions-App in der Visual Studio-2019-veröffentlichungsdialogfeld:

![Eine Azure Functions-App in der Visual Studio-2019 veröffentlichungsdialogfeld](azure-signalr-images/vs-app-selection.png "Azure Functions-App im Visual Studio-2019 – Dialogfeld \"Veröffentlichen\"")

Nach dem Auswählen einer Azure-Funktionen-App-Instanz, die Website-URL, Konfiguration und andere Informationen über die Azure Functions-App-Ziel angezeigt. Wählen Sie **Azure App Service-Einstellungen bearbeiten** , und geben Sie die Verbindungszeichenfolge in der **Remote** Feld. Die Verbindungszeichenfolge wird verwendet, durch die **Negotiate** und **sprechen** zur Verbindung mit der Azure SignalR Service-Funktionen und steht in der **Schlüssel** Teil der Azure SignalR Der Dienst in Ihrem Azure-Portal. Weitere Informationen zu Verbindungszeichenfolgen finden Sie unter [erstellen Sie eine Azure SignalR Service](#create-an-azure-signalr-service).

Sobald Sie die Verbindungszeichenfolge eingegeben haben, klicken Sie auf **veröffentlichen** Ihrer Funktionen in der Azure-Funktionen-App bereitstellen. Nach Abschluss des Vorgangs werden die Funktionen in der Azure Functions-App im Azure-Portal aufgeführt werden. Der folgende Screenshot zeigt die veröffentlichten Funktionen im Azure-Portal:

![Funktionen, die in der Azure Functions-App veröffentlicht](azure-signalr-images/azure-functions-deployed.png "Funktionen, die in der Azure Functions-App veröffentlicht wurden")

## <a name="integrate-azure-signalr-service-with-xamarinforms"></a>Integrieren von Azure SignalR-Dienst mit Xamarin.Forms

Die Integration zwischen Azure SignalR Service und die Xamarin.Forms-Anwendung ist eine SignalR-Dienst-Klasse, die in instanziiert wird die `MainPage` Klasse mit Ereignishandlern, die drei Ereignisse zugewiesen. Weitere Informationen zu diesen Ereignishandlern finden Sie unter [verwenden Sie die SignalR-Dienstklasse in Xamarin.Forms](#use-the-signalr-service-class-in-xamarinforms).

Die beispielanwendung enthält einen **"Constants.cs"** -Klasse, die mit dem URL-Endpunkt Ihrer Azure-Funktionen-App angepasst werden muss. Legen Sie den Wert, der die `HostName` Eigenschaft, um Ihre Azure Functions-App-Adresse. Der folgende code zeigt die **"Constants.cs"** Eigenschaften mit einem Beispiel `HostName` Wert:

```csharp
public static class Constants
{
    public static string HostName { get; set; } = "https://example-functions-app.azurewebsites.net/";

    // Used to differentiate message types sent via SignalR. This
    // sample only uses a single message type.
    public static string MessageName { get; set; } = "newMessage";

    public static string Username
    {
        get
        {
            return $"{Device.RuntimePlatform} User";
        }
    }
}
```

> [!NOTE]
> Die `Username` Eigenschaft in der beispielanwendung **"Constants.cs"** -Datei verwendet des Geräts die `RuntimePlatform` Wert als Benutzernamen. Dies erleichtert das Testen von Geräten – plattformübergreifend und zu identifizieren, welches Gerät die Nachricht sendet. In einer realen Anwendung wäre wahrscheinlich einen eindeutigen Benutzernamen dieser Wert immer dann, während ein gesammelt werden, um oder melden Sie sich den Vorgang.

### <a name="the-signalr-service-class"></a>Der SignalR Service-Klasse

Die `SignalRService` -Klasse in der **ChatClient** Projekt in der beispielanwendung zeigt eine Implementierung, die Funktionen in einer Azure-Funktionen-App für die Verbindung mit einem Azure SignalR Service aufruft.

Die `SendMessageAsync` -Methode in der die `SignalRService` Klasse dient zum Senden von Nachrichten für Clients, die mit den Azure SignalR-Dienst verbunden sind. Diese Methode führt eine HTTP POST-Anforderung an die **sprechen** Funktion, die in der Azure Functions-App, einschließlich einer JSON-serialisierten gehosteten `Message` -Objekt als der POST-Nutzlast. Die **sprechen** Funktion übergibt die Nachricht an den Azure SignalR Service per broadcast an alle verbundenen Clients. Der folgende Code veranschaulicht die `SendMessageAsync`-Methode:

```csharp
public async Task SendMessageAsync(string username, string message)
{
    IsBusy = true;

    var newMessage = new Message
    {
        Name = username,
        Text = message
    };

    var json = JsonConvert.SerializeObject(newMessage);
    var content = new StringContent(json, Encoding.UTF8, "application/json");
    var result = await client.PostAsync($"{Constants.HostName}/api/talk", content);

    IsBusy = false;
}
```

Die `ConnectAsync` -Methode in der die `SignalRService` Klasse führt eine HTTP GET-Anforderung an die **Negotiate** Funktion, die in der Azure Functions-App gehostet. Die **Negotiate** Funktionsergebnis ist JSON, das deserialisiert wird, wird in eine Instanz von der `NegotiateInfo` Klasse. Sobald die `NegotiateInfo` Objekt abgerufen wird, es wird verwendet, um direkt auf die Verwendung einer Instanz von Azure SignalR Service registrieren der `HubConnection` Klasse.

ASP.NET Core SignalR eingehende Daten über die offene Verbindung in Nachrichten übersetzt und ermöglicht es Entwicklern, definieren die Nachrichtentypen und Binden von Ereignishandlern auf eingehende Nachrichten nach Typ. Die `ConnectAsync` Methode registriert einen Ereignishandler für den Namen der definiert, in der beispielanwendung **"Constants.cs"** -Datei, die "NewMessage" in der Standardeinstellung ist.

Der folgende Code veranschaulicht die `ConnectAsync`-Methode:

```csharp
public async Task ConnectAsync()
{
    try
    {
        IsBusy = true;

        string negotiateJson = await client.GetStringAsync($"{Constants.HostName}/api/negotiate");
        NegotiateInfo negotiate = JsonConvert.DeserializeObject<NegotiateInfo>(negotiateJson);
        HubConnection connection = new HubConnectionBuilder()
            .WithUrl(negotiate.Url, options =>
            {
                options.AccessTokenProvider = async () => negotiate.AccessToken;
            })
            .Build();

        connection.On<JObject>(Constants.MessageName, AddNewMessage);
        await connection.StartAsync();

        IsConnected = true;
        IsBusy = false;

        Connected?.Invoke(this, true, "Connection successful.");
    }
    catch (Exception ex)
    {
        ConnectionFailed?.Invoke(this, false, ex.Message);
    }
}
```

Die `AddNewMessage` Methode gebunden ist, als der Ereignishandler in der `ConnectAsync` Meldung wie im vorherigen Code gezeigt. Wenn eine Nachricht empfangen wird, die `AddNewMessage` Methode wird aufgerufen, mit den Nachrichtendaten, die als eine `JObject`. Die `AddNewMessage` Methode konvertiert die `JObject` mit einer Instanz von der `Message` Klasse, und klicken Sie dann ruft der Handler für `NewMessageReceived` Wenn eine gebunden wurde. Der folgende Code veranschaulicht die `AddNewMessage`-Methode:

```csharp
public void AddNewMessage(JObject message)
{
    Message messageModel = new Message
    {
        Name = message.GetValue("name").ToString(),
        Text = message.GetValue("text").ToString(),
        TimeReceived = DateTime.Now
    };

    NewMessageReceived?.Invoke(this, messageModel);
}
```

### <a name="use-the-signalr-service-class-in-xamarinforms"></a>Verwenden Sie die SignalR-Dienstklasse in Xamarin.Forms

Nutzen die SignalR-Dienst-Klasse in Xamarin.Forms wird erreicht, indem Sie die Bindung der `SignalRService` Klasse Ereignisse in der `MainPage` Code-Behind-Klasse.

Die `Connected` Ereignis in der `SignalRService` Klasse wird ausgelöst, wenn es sich bei eine SignalR-Verbindung erfolgreich abgeschlossen wurde. Die `ConnectionFailed` Ereignis in der `SignalRService` Klasse wird ausgelöst, wenn es sich bei eine SignalR-Verbindung ein Fehler auftritt. Die `SignalR_ConnectionChanged` Ereignishandlermethode gebunden ist, auf beide Ereignisse in der `MainPage` Konstruktor. Dieser Ereignishandler aktualisiert die Schaltflächenstatus verbinden und senden basierend auf der Verbindung `success` -Argument, und fügt die Nachricht, die bereitgestellt werden, durch das Ereignis, das die Chat-Warteschlange mithilfe der `AddMessage` Methode. Der folgende code zeigt die `SignalR_ConnectionChanged` Ereignishandlermethode:

```csharp
void SignalR_ConnectionChanged(object sender, bool success, string message)
{
    connectButton.Text = "Connect";
    connectButton.IsEnabled = !success;
    sendButton.IsEnabled = success;

    AddMessage($"Server connection changed: {message}");
}
```

Die `NewMessageReceived` Ereignis in der `SignalRService` Klasse wird ausgelöst, wenn eine neue Nachricht aus dem Azure SignalR-Dienst empfangen wird. Die `SignalR_NewMessageReceived` Ereignishandlermethode gebunden ist, um die `NewMessageReceived` Ereignis in der `MainPage` Konstruktor. Dieser Ereignishandler konvertiert die eingehende `Message` Objekt in eine Zeichenfolge und fügt es der Chat-Warteschlange mit hinzu der `AddMessage` Methode. Der folgende code zeigt die `SignalR_NewMessageReceived` Ereignishandlermethode:

```csharp
void SignalR_NewMessageReceived(object sender, Model.Message message)
{
    string msg = $"{message.Name} ({message.TimeReceived}) - {message.Text}";
    AddMessage(msg);
}
```

Die `AddMessage` Methode fügt eine neue Nachricht als eine `Label` Objekt, das die Chat-Warteschlange. Die `AddMessage` Methode wird von Ereignishandlern, die von außerhalb der Hauptthread der Benutzeroberfläche, häufig aufgerufen, damit es erzwingt, die Aktualisierungen der Benutzeroberfläche im Hauptthread dass, um zu verhindern, dass Ausnahmen auftreten. Der folgende Code veranschaulicht die `AddMessage`-Methode:

```csharp
void AddMessage(string message)
{
    Device.BeginInvokeOnMainThread(() =>
    {
        Label label = new Label
        {
            Text = message,
            HorizontalOptions = LayoutOptions.Start,
            VerticalOptions = LayoutOptions.Start
        };

        messageList.Children.Add(label);
    });
}
```

## <a name="test-the-application"></a>Testen der Anwendung

Der SignalR Chat-Anwendung kann auf iOS, Android und UWP getestet werden, vorausgesetzt, Sie haben:

1. Erstellt einen Azure SignalR-Dienst.
1. Erstellt eine Azure Functions-App.
1. Angepasste der **"Constants.cs"** -Datei mit der Azure Functions-App-Endpunkt.

Nach Abschluss dieser Schritte und die Anwendung ausgeführt wird, auf die **Connect** Schaltfläche Forms eine Verbindung mit dem Azure SignalR-Dienst. Eine Nachricht eingeben und auf die **senden** Schaltfläche führt diese Nachrichten ständig erhalten die Chat-Warteschlange auf einem verbundenen mobilen Anwendung.

## <a name="related-links"></a>Verwandte Links

* [Erstellen in Echtzeit mobiler apps mit Xamarin und SignalR](https://mybuild.techcommunity.microsoft.com/sessions/77333/)
* [Einführung zu SignalR](/aspnet/signalr/overview/getting-started/introduction-to-signalr)
* [Einführung in Azure Functions](/azure/azure-functions/functions-overview)
* [Dokumentation zu Azure Functions](/azure/azure-functions/)
* [MVVM SignalR-chatbeispiel](https://github.com/lbugnion/sample-xamarin-signalr)
