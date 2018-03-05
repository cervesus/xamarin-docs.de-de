---
title: Spracherkennung, die mithilfe der Bing-Speech-API
description: "Die Bing-Speech-API ist eine Cloud-basierte API, die Algorithmen zum Verarbeiten der gesprochenen Sprache bereitstellt. In diesem Artikel erläutert die Bing-Sprache Recognition REST-API zu verwenden, um Audio in Text in einer Xamarin.Forms-Anwendung zu konvertieren."
ms.topic: article
ms.prod: xamarin
ms.assetid: B435FF6B-8785-48D9-B2D9-1893F5A87EA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 186ea6277ec7bd4ceb52855186e6fd88344b1b86
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="speech-recognition-using-the-bing-speech-api"></a>Spracherkennung, die mithilfe der Bing-Speech-API

_Die Bing-Speech-API ist eine Cloud-basierte API, die Algorithmen zum Verarbeiten der gesprochenen Sprache bereitstellt. In diesem Artikel erläutert die Bing-Sprache Recognition REST-API zu verwenden, um Audio in Text in einer Xamarin.Forms-Anwendung zu konvertieren._

![](~/media/shared/preview.png "Diese API ist derzeit Vorabversion")

> [!NOTE]
> Die Bing-Speech-API ist noch in der Vorschau. Es kann an die API vor der endgültigen Version um unterbrechende Änderungen werden.

## <a name="overview"></a>Übersicht

Die Bing-Speech-API besteht aus zwei Komponenten:

- Eine Spracherkennung API für gesprochenen Wörter in Text konvertieren. Spracherkennung kann über einen REST-API oder die Clientbibliothek bzw. Dienstbibliothek ausgeführt werden.
- Ein Text-zu-Sprache API für das Konvertieren von Text in gesprochenen Wörter. Text-zu-Sprache Konvertierung erfolgt über eine REST-API.

Dieser Artikel konzentriert sich auf die Spracherkennung über die REST-API ausführen. Während der Client und Dienst Bibliotheken partielle Ergebnisse zurückgeben unterstützen, können die REST-API nur ein einzelnes Recognition Ergebnis war, ohne alle Teilergebnisse zurück.

Ein API-Schlüssel muss zum Verwenden der Bing-Sprache Recognition-API abgerufen werden. Dies kann abgerufen werden, auf [Einstieg kostenlos](https://www.microsoft.com/cognitive-services/sign-up?ReturnUrl=/cognitive-services/subscriptions?productId=%2fproducts%2fBing.Speech.Preview) auf "Microsoft.com".

Weitere Informationen über die Bing-Speech-API finden Sie unter [Bing Spracherkennung Dokumentation](https://www.microsoft.com/cognitive-services/Speech-api/documentation/overview) auf "Microsoft.com".

## <a name="authentication"></a>Authentifizierung

Jeder Anforderung an die Bing-Sprache Recognition REST-API erfordert eine JSON-Webtoken (JWT)-Zugriffstoken, die aus den cognitive token Services am abgerufen werden kann `https://api.cognitive.microsoft.com/sts/v1.0/issueToken`. Ein Token erhalten Sie, indem Sie eine POST-Anforderung an den Sicherheitstokendienst angeben einer `Ocp-Apim-Subscription-Key` Header, der den API-Schlüssel als Wert enthält.

Im folgenden Codebeispiel wird gezeigt, wie ein Zugriff token aus dem token-Dienst:

```csharp
async Task<string> FetchTokenAsync(string fetchUri, string apiKey)
{
  using (var client = new HttpClient())
  {
    client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
    UriBuilder uriBuilder = new UriBuilder(fetchUri);
    uriBuilder.Path += "/issueToken";

    var result = await client.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
    return await result.Content.ReadAsStringAsync();
  }
}
```

Das zurückgegebene Zugriffstoken, das Base64-Text ist, hat eine Ablaufzeit von 10 Minuten. Aus diesem Grund erneuert die beispielanwendung das Zugriffstoken Minuten 9.

Das Zugriffstoken muss angegeben werden, in jeder Bing Speech Recognition REST-API aufrufen, wie ein `Authorization` Header, die die Zeichenfolge mit dem Präfix `Bearer`, wie im folgenden Codebeispiel wird gezeigt:

```csharp
using (var httpClient = new HttpClient())
{
  httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
  ...
}  
```

Wenn eine gültige Zugriffstoken an die Bing-Sprache Recognition REST-API übergeben werden, in einer Antwort 403-Fehler führt.

## <a name="performing-speech-recognition"></a>Spracherkennung ausführen

Spracherkennung erfolgt durch Herstellen einer POST-Anforderung an `recognize` -API bei `https://speech.platform.bing.com/recognize`. Eine einzelne Anforderung kann nicht mehr als 10 Sekunden Audio enthalten, und die gesamtanforderung-Dauer darf 14 Sekunden nicht überschreiten.

Audioinhalte muss in der POST-Text der Anforderung in Wav-Format eingefügt werden. Weitere Informationen zu unterstützten Codecs, finden Sie unter [unterstützten Codecs](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#supported-codecs) auf "Microsoft.com".

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
    var speechResults = JsonConvert.DeserializeObject<SpeechResults>(response);

    fileStream.Dispose();
    return speechResults.results.FirstOrDefault();
}
```

Audio in jedem Projekt plattformspezifischen als PCM Wav Daten aufgezeichnet wird und die `RecognizeSpeechAsync` -Methode verwendet die `PCLStorage` NuGet-Paket zu die Audiodatei als Datenstrom zu öffnen. Die Spracherkennung Recognition Anforderung URI generiert und ein Zugriffstoken wird vom token-Dienst abgerufen. Die Spracherkennung Recognition-Anforderung übermittelt wird, um die `recognize` -API, die eine JSON-Antwort mit dem Ergebnis zurückgibt. Die JSON-Antwort wird deserialisiert, mit der Rückgabe an die aufrufende Methode für die Anzeige.

### <a name="configuring-speech-recognition"></a>Spracherkennung konfigurieren

Die Spracherkennung Erkennungsvorgang kann durch Angeben von Abfrageparametern für HTTP konfiguriert werden. Es gibt obligatorischen und optionalen Parameter, durch die folgende Methode zeigt die obligatorischen Parameter, die festgelegt werden müssen:

```csharp
string GenerateRequestUri(string speechEndpoint)
{
    string requestUri = speechEndpoint;
    requestUri += @"?scenarios=ulm";                                    // websearch is the other option
    requestUri += @"&appid=D4D52672-91D7-4C74-8AD8-42B1D98141A5";       // You must use this ID
    requestUri += @"&locale=en-US";                                     // Other languages supported
    requestUri += string.Format("&device.os={0}", operatingSystem);     // Open field
    requestUri += @"&version=3.0";                                      // Required value
    requestUri += @"&format=json";                                      // Required value
    requestUri += @"&instanceid=fe34a4de-7927-4e24-be60-f0629ce1d808";  // GUID for device making the request
    requestUri += @"&requestid=" + Guid.NewGuid().ToString();           // GUID for the request
    return requestUri;
}
```

Die wichtigsten Konfiguration durch die `GenerateRequestUri` Methode besteht darin, das Gebietsschema des audio Inhalts festgelegt. Eine Liste der unterstützten Gebietsschemas, finden Sie unter [unterstützt Gebietsschemas ](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#supported-locales) auf "Microsoft.com".

Weitere Informationen zu den möglichen Werten für die obligatorischen Parameter finden Sie unter [erforderliche Parameter](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#required-parameters) auf "Microsoft.com". Informationen zu den optionalen Parametern finden Sie unter [optionale Parameter](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition) auf "Microsoft.com".

### <a name="sending-the-request"></a>Senden der Anforderung

Die `SendRequestAsync` Methode macht die POST-Anforderung an die Bing-Sprache Recognition REST-API und gibt die Antwort zurück:

```csharp
async Task<string> SendRequestAsync(Stream fileStream, string url, string bearerToken, string contentType)
{
    var content = new StreamContent(fileStream);
    content.Headers.TryAddWithoutValidation("Content-Type", contentType);

    using (var httpClient = new HttpClient())
    {
        httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
        var response = await httpClient.PostAsync(url, content);

        return await response.Content.ReadAsStringAsync();
    }
}
```

Mit dieser Methode wird die POST-Anforderung durch:

- Umschließen den Audiostream in einer `StreamContent` -Instanz, die HTTP-Inhalt basierend auf einem Datenstrom bereitstellt.
- Festlegen der `Content-Type` Header der Anforderung zum `audio/wav; codec="audio/pcm"; samplerate=16000`.
- Das Zugriffstoken zum Hinzufügen der `Authorization` -Header, die Zeichenfolge mit dem Präfix `Bearer`.

Die POST-Anforderung wird dann an gesendet `recognize` API. Die Antwort wird dann gelesen und an die aufrufende Methode zurückgegeben.

Die `recognize` API wird HTTP-Statuscode 200 (OK) gesendet, in der Antwort angegeben, dass die Anforderung gültig ist, was bedeutet, dass die Anforderung erfolgreich war und dass die angeforderte Informationen in der Antwort ist. Eine Liste der möglichen Fehlerantworten, finden Sie unter [Fehlerantworten](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#error-responses) auf "Microsoft.com".

### <a name="processing-the-response"></a>Verarbeiten der Antwort

Die API-Antwort im JSON-Format zurückgegeben, mit der erkannte Text wird in der `name` Tag. Die folgenden JSON-Daten enthalten eine typische erfolgreiche Antwortnachricht:

```csharp
{
  "version": "3.0",
  "header": {
    "status": "success",
    "scenario": "ulm",
    "name": "go shopping tomorrow",
    "lexical": "go shopping tomorrow",
    "properties": {
      "requestid": "e06c059d-fa37-4bb1-843f-4914350279a8",
      "HIGHCONF": "1"
    }
  },
  "results": [
    {
      "scenario": "ulm",
      "name": "go shopping tomorrow",
      "lexical": "go shopping tomorrow",
      "confidence": "0.9493451",
      "properties": {
        "HIGHCONF": "1"
      }
    }
  ]
}
```

In der beispielanwendung wird deserialisiert die JSON-Antwort in eine `SpeechResult` Instanz, mit dem Ergebnis an die aufrufende Methode für die Anzeige zurückgegeben wird, wie in den folgenden Screenshots dargestellt:

![](speech-recognition-images/speech-recognition.png "Spracherkennung")

Informationen zu den Werten der einzelnen JSON-Tags finden Sie unter [Speech Recognition Antworten](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#speech-recognition-responses) auf "Microsoft.com".

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie die Bing-Sprache Recognition REST-API zu verwenden, um Audio in Text in einer Xamarin.Forms-Anwendung zu konvertieren. Zusätzlich zum Ausführen von der Spracherkennung, können die Bing-Speech-API auch Text in gesprochenen Wörter konvertieren.



## <a name="related-links"></a>Verwandte Links

- [Bing-Speech-Dokumentation](https://www.microsoft.com/cognitive-services/Speech-api/documentation/overview)
- [Nutzen einen RESTful-Webdienst](~/xamarin-forms/data-cloud/consuming/rest.md)
- [TODO-Cognitive Services (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Bing-Spracherkennung REST-API](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition)
