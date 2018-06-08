---
title: Spracherkennung, die mit dem Microsoft-Speech-API
description: Die Microsoft-Speech-API ist eine Cloud-basierte API, die Algorithmen zum Verarbeiten der gesprochenen Sprache bereitstellt. In diesem Artikel erläutert die Microsoft Speech Recognition REST-API zu verwenden, um Audio in Text in einer Xamarin.Forms-Anwendung zu konvertieren.
ms.prod: xamarin
ms.assetid: B435FF6B-8785-48D9-B2D9-1893F5A87EA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: c8eb991f67d54f9bacbb776b350cc5649a04ab2b
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34846853"
---
# <a name="speech-recognition-using-the-microsoft-speech-api"></a>Spracherkennung, die mit dem Microsoft-Speech-API

_Die Microsoft-Speech-API ist eine Cloud-basierte API, die Algorithmen zum Verarbeiten der gesprochenen Sprache bereitstellt. In diesem Artikel erläutert die Microsoft Speech Recognition REST-API zu verwenden, um Audio in Text in einer Xamarin.Forms-Anwendung zu konvertieren._

## <a name="overview"></a>Übersicht

Die Microsoft-Speech-API besteht aus zwei Komponenten:

- Eine Spracherkennung API für gesprochenen Wörter in Text konvertieren. Spracherkennung kann über einen REST-API oder die Clientbibliothek bzw. Dienstbibliothek ausgeführt werden.
- Ein Text-zu-Sprache API für das Konvertieren von Text in gesprochenen Wörter. Text-zu-Sprache Konvertierung erfolgt über eine REST-API.

Dieser Artikel konzentriert sich auf die Spracherkennung über die REST-API ausführen. Während der Client und Dienst Bibliotheken partielle Ergebnisse zurückgeben unterstützen, können die REST-API nur ein einzelnes Recognition Ergebnis war, ohne alle Teilergebnisse zurück.

Ein API-Schlüssel muss zum Verwenden der Microsoft-Speech-API abgerufen werden. Dies kann abgerufen werden, auf [Cognitive Services versuchen](https://azure.microsoft.com/try/cognitive-services/).

Weitere Informationen über die Microsoft-Speech-API finden Sie unter [Microsoft Speech-API-Dokumentation](/azure/cognitive-services/speech/home/).

## <a name="authentication"></a>Authentifizierung

Jeder Anforderung an den Microsoft-Speech-REST-API erfordert eine JSON-Webtoken (JWT)-Zugriffstoken, die aus den cognitive token Services am abgerufen werden kann `https://api.cognitive.microsoft.com/sts/v1.0/issueToken`. Ein Token erhalten Sie, indem Sie eine POST-Anforderung an den Sicherheitstokendienst angeben einer `Ocp-Apim-Subscription-Key` Header, der den API-Schlüssel als Wert enthält.

Im folgenden Codebeispiel wird gezeigt, wie ein Zugriff token aus dem token-Dienst:

```csharp
public AuthenticationService(string apiKey)
{
    subscriptionKey = apiKey;
    httpClient = new HttpClient();
    httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
}
...
async Task<string> FetchTokenAsync(string fetchUri)
{
    UriBuilder uriBuilder = new UriBuilder(fetchUri);
    uriBuilder.Path += "/issueToken";
    var result = await httpClient.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
    return await result.Content.ReadAsStringAsync();
}
```

Das zurückgegebene Zugriffstoken, das Base64-Text ist, hat eine Ablaufzeit von 10 Minuten. Aus diesem Grund erneuert die beispielanwendung das Zugriffstoken Minuten 9.

Das Zugriffstoken muss angegeben werden, in jeder Sprache-REST-API von Microsoft rufen Sie als ein `Authorization` Header, die die Zeichenfolge mit dem Präfix `Bearer`, wie im folgenden Codebeispiel wird dargestellt:

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
```

Fehler beim Übergeben eines gültigen Zugriffstokens der REST-API von Microsoft-Sprache führt zu einer Antwort 403-Fehler.

## <a name="performing-speech-recognition"></a>Spracherkennung ausführen

Spracherkennung erfolgt durch Herstellen einer POST-Anforderung an die `recognition` -API bei `https://speech.platform.bing.com/speech/recognition/`. Eine einzelne Anforderung kann nicht mehr als 10 Sekunden Audio enthalten, und die gesamtanforderung-Dauer darf 14 Sekunden nicht überschreiten.

Audioinhalte muss in der POST-Text der Anforderung in Wav-Format eingefügt werden.

In der beispielanwendung die `RecognizeSpeechAsync` Methode ruft die Spracherkennung Erkennungsvorgang:

```csharp
public async Task<SpeechResult> RecognizeSpeechAsync(string filename)
{
    ...

    // Read audio file to a stream
    var file = await PCLStorage.FileSystem.Current.LocalStorage.GetFileAsync(filename);
    var fileStream = await file.OpenAsync(PCLStorage.FileAccess.Read);

    // Send audio stream to Bing and deserialize the response
    string requestUri = GenerateRequestUri(Constants.SpeechRecognitionEndpoint);
    string accessToken = authenticationService.GetAccessToken();
    var response = await SendRequestAsync(fileStream, requestUri, accessToken, Constants.AudioContentType);
    var speechResult = JsonConvert.DeserializeObject<SpeechResult>(response);

    fileStream.Dispose();
    return speechResult;
}
```

Audio in jedem Projekt plattformspezifischen als PCM Wav Daten aufgezeichnet wird und die `RecognizeSpeechAsync` -Methode verwendet die `PCLStorage` NuGet-Paket zu die Audiodatei als Datenstrom zu öffnen. Die Spracherkennung Recognition Anforderung URI generiert und ein Zugriffstoken wird vom token-Dienst abgerufen. Die Spracherkennung Recognition-Anforderung übermittelt wird, um die `recognition` -API, die eine JSON-Antwort mit dem Ergebnis zurückgibt. Die JSON-Antwort wird deserialisiert, mit der Rückgabe an die aufrufende Methode für die Anzeige.

### <a name="configuring-speech-recognition"></a>Spracherkennung konfigurieren

Spracherkennung Erkennungsvorgang kann durch Angeben von Abfrageparametern für HTTP konfiguriert werden:

```csharp
string GenerateRequestUri(string speechEndpoint)
{
    // To build a request URL, you should follow:
    // https://docs.microsoft.com/en-us/azure/cognitive-services/speech/getstarted/getstartedrest
    string requestUri = speechEndpoint;
    requestUri += @"dictation/cognitiveservices/v1?";
    requestUri += @"language=en-us";
    requestUri += @"&format=simple";
    System.Diagnostics.Debug.WriteLine(requestUri.ToString());
    return requestUri;
}
```

Die wichtigsten Konfiguration durch die `GenerateRequestUri` Methode besteht darin, das Gebietsschema des audio Inhalts festgelegt. Eine Liste der unterstützten Gebietsschemas, finden Sie unter [unterstützte Sprachen ](/azure/cognitive-services/speech/api-reference-rest/supportedlanguages/).

### <a name="sending-the-request"></a>Senden der Anforderung

Die `SendRequestAsync` Methode macht die POST-Anforderung an den Microsoft Speech-REST-API und gibt die Antwort zurück:

```csharp
async Task<string> SendRequestAsync(Stream fileStream, string url, string bearerToken, string contentType)
{
    if (httpClient == null)
    {
        httpClient = new HttpClient();
    }
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);

    var content = new StreamContent(fileStream);
    content.Headers.TryAddWithoutValidation("Content-Type", contentType);
    var response = await httpClient.PostAsync(url, content);
    return await response.Content.ReadAsStringAsync();
}
```

Mit dieser Methode wird die POST-Anforderung durch:

- Umschließen den Audiostream in einer `StreamContent` -Instanz, die HTTP-Inhalt basierend auf einem Datenstrom bereitstellt.
- Festlegen der `Content-Type` Header der Anforderung zum `audio/wav; codec="audio/pcm"; samplerate=16000`.
- Das Zugriffstoken zum Hinzufügen der `Authorization` -Header, die Zeichenfolge mit dem Präfix `Bearer`.

Die POST-Anforderung wird dann an gesendet `recognition` API. Die Antwort wird dann gelesen und an die aufrufende Methode zurückgegeben.

Die `recognition` API wird HTTP-Statuscode 200 (OK) gesendet, in der Antwort angegeben, dass die Anforderung gültig ist, was bedeutet, dass die Anforderung erfolgreich war und dass die angeforderte Informationen in der Antwort ist. Eine Liste der möglichen Fehlerantworten, finden Sie unter [Problembehandlung](/azure/cognitive-services/speech/troubleshooting).

### <a name="processing-the-response"></a>Verarbeiten der Antwort

Die API-Antwort im JSON-Format zurückgegeben, mit der erkannte Text wird in der `name` Tag. Die folgenden JSON-Daten enthalten eine typische erfolgreiche Antwortnachricht:

```json
{  
   "RecognitionStatus":"Success",
   "DisplayText":"Go shopping tomorrow.",
   "Offset":16000000,
   "Duration":17100000
}
```

In der beispielanwendung wird deserialisiert die JSON-Antwort in eine `SpeechResult` Instanz, mit dem Ergebnis an die aufrufende Methode für die Anzeige zurückgegeben wird, wie in den folgenden Screenshots dargestellt:

![](speech-recognition-images/speech-recognition.png "Spracherkennung")

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird erläutert, wie mithilfe der REST-API von Microsoft Spracherkennung um Audio auf Text in einer Xamarin.Forms-Anwendung zu konvertieren. Zusätzlich zum Ausführen von der Spracherkennung, können die Microsoft-Speech-API auch Text in gesprochenen Wörter konvertieren.

## <a name="related-links"></a>Verwandte Links

- [Microsoft-Speech-API-Dokumentation](/azure/cognitive-services/speech/home/).
- [Nutzen einen RESTful-Webdienst](~/xamarin-forms/data-cloud/consuming/rest.md)
- [TODO-Cognitive Services (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
