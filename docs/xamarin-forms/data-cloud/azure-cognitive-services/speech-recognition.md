---
title: Spracherkennung, die mit der Microsoft-Spracheingabe-API
description: Der Microsoft Speech-API ist eine Cloud-basierte API, Algorithmen zur Verarbeitung gesprochener Sprache. In diesem Artikel wird erläutert, wie der Microsoft Speech Recognition-REST-API zu verwenden, um Audiodaten in Text in einer Xamarin.Forms-Anwendung zu konvertieren.
ms.prod: xamarin
ms.assetid: B435FF6B-8785-48D9-B2D9-1893F5A87EA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 97997a527647ae972eadff47da8c1321d5d55daa
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68655457"
---
# <a name="speech-recognition-using-the-microsoft-speech-api"></a>Spracherkennung, die mit der Microsoft-Spracheingabe-API

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)

_Der Microsoft Speech-API ist eine Cloud-basierte API, Algorithmen zur Verarbeitung gesprochener Sprache. In diesem Artikel wird erläutert, wie der Microsoft Speech Recognition-REST-API zu verwenden, um Audiodaten in Text in einer Xamarin.Forms-Anwendung zu konvertieren._

## <a name="overview"></a>Übersicht

Der Microsoft Speech-API besteht aus zwei Komponenten:

- Eine Spracherkennung API für die Konvertierung von gesprochener Wörtern in Text. Spracherkennung kann über eine REST-API, -Clientbibliothek oder -Dienstbibliothek ausgeführt werden.
- Ein Text in Sprache API für die Konvertierung von Text in gesprochenen Wörtern. Konvertierung von Text in Sprache erfolgt über eine REST-API.

Dieser Artikel konzentriert sich auf die Spracherkennung, über die REST-API ausführt. Während der Client und Dienst-Bibliotheken partielle Ergebnisse zurückgeben unterstützt, kann der REST-API nur eine einzelne Erkennungsergebnis, ohne alle partiellen Ergebnisse zurückgeben.

API-Schlüssel muss zum Verwenden der Microsoft Speech-API abgerufen werden. Dadurch erhalten Sie über das Azure [Portal](https://portal.azure.com/). Weitere Informationen finden Sie unter [Cognitive Services-Konto im Azure-Portal erstellen](/azure/cognitive-services/cognitive-services-apis-create-account).

Weitere Informationen zu der Microsoft Speech-API, finden Sie unter [Microsoft Speech-API-Dokumentation](/azure/cognitive-services/speech/home/).

## <a name="authentication"></a>Authentifizierung

Jeder Anforderung an die REST-API von Microsoft erfordert ein JSON Web Token (JWT)-Zugriffstoken, der vom token cognitive Services-Dienst auf abgerufen werden kann `https://api.cognitive.microsoft.com/sts/v1.0/issueToken`. Ein Token abgerufen werden kann, durch eine POST-Anforderung an den Sicherheitstokendienst angeben einer `Ocp-Apim-Subscription-Key` Header, der den API-Schlüssel als Wert enthält.

Das folgende Codebeispiel veranschaulicht eine zugriffsanforderung token vom Tokendienst:

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

Das zurückgegebene Zugriffstoken, das Base64-Text ist, hat eine Ablaufzeit von 10 Minuten. Aus diesem Grund erneuert die beispielanwendung das Zugriffstoken 9 Minuten.

Das Zugriffstoken muss angegeben werden, in den einzelnen REST-API von Microsoft rufen Sie als ein `Authorization` Header, die mit der Zeichenfolge das Präfix `Bearer`, wie im folgenden Codebeispiel gezeigt:

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
```

Fehler beim Übergeben eines gültiges Zugriffstokens an die REST-API von Microsoft führt zu einem Fehler 403-Antwort.

## <a name="performing-speech-recognition"></a>Spracherkennung ausführt

Spracherkennung wird erreicht, indem Sie eine POST-Anforderung, sodass die `recognition` -API zu `https://speech.platform.bing.com/speech/recognition/`. Eine einzelne Anforderung darf nicht mehr als 10 Sekunden Audio enthalten, und die gesamte Anforderung kann nicht länger als 14 Sekunden.

Audioinhalte muss im POST-Text der Anforderung in die Wav-Format platziert werden.

In der beispielanwendung die `RecognizeSpeechAsync` Methode aufruft, den Speech Recognition Prozess:

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

Audio wird in den einzelnen plattformspezifischen Projekten PCM-Wav-Daten, aufgezeichnet und die `RecognizeSpeechAsync` -Methode verwendet die `PCLStorage` NuGet-Paket, um die audio-Datei als Stream zu öffnen. Der Speech Recognition Anforderungs-URI generiert wird und ein Zugriffstoken wird vom Tokendienst abgerufen. Die Speech Recognition-Anforderung wird gesendet, um die `recognition` -API, die eine JSON-Antwort, die das Ergebnis zurückgibt. Die JSON-Antwort wird deserialisiert, mit dem Ergebnis, das an die aufrufende Methode für die Anzeige zurückgegeben wird.

### <a name="configuring-speech-recognition"></a>Konfigurieren der Spracherkennung

Der Speech Recognition Prozess kann durch Angabe von Abfrageparametern für HTTP konfiguriert werden:

```csharp
string GenerateRequestUri(string speechEndpoint)
{
    // To build a request URL, you should follow:
    // https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedrest
    string requestUri = speechEndpoint;
    requestUri += @"dictation/cognitiveservices/v1?";
    requestUri += @"language=en-us";
    requestUri += @"&format=simple";
    System.Diagnostics.Debug.WriteLine(requestUri.ToString());
    return requestUri;
}
```

Die wichtigsten Konfiguration durch die `GenerateRequestUri` Methode besteht darin, das Gebietsschema der Audioinhalt festgelegt. Eine Liste der unterstützten Gebiets Schemas finden Sie [unter Unterstützte Sprachen](/azure/cognitive-services/speech/api-reference-rest/supportedlanguages/).

### <a name="sending-the-request"></a>Senden der Anforderung

Die `SendRequestAsync` Methode macht die POST-Anforderung an die REST-API von Microsoft und die Antwort zurückgegeben:

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

Diese Methode erstellt die POST-Anforderung durch:

- Umschließen den Audiostream in einem `StreamContent` -Instanz, die Grundlage eines Datenstroms HTTP-Inhalt enthält.
- Festlegen der `Content-Type` -Header der Anforderung um `audio/wav; codec="audio/pcm"; samplerate=16000`.
- Hinzufügen des Zugriffstokens für die `Authorization` -Header, die Zeichenfolge mit dem Präfix `Bearer`.

Anschließend wird die POST-Anforderung an gesendet `recognition` API. Die Antwort ist dann gelesen und an die aufrufende Methode zurückgegeben.

Die `recognition` API sendet HTTP-Statuscode 200 (OK) in der Antwort angegeben, dass die Anforderung gültig ist, was bedeutet, dass die Anforderung erfolgreich war, und dass die angeforderte Informationen in der Antwort ist. Eine Liste der möglichen Fehlerantworten, finden Sie unter [Problembehandlung](/azure/cognitive-services/speech/troubleshooting).

### <a name="processing-the-response"></a>Verarbeiten der Antwort

Die API-Antwort wird im JSON-Format zurückgegeben, mit der erkannte Text wird im der `name` Tag. Die folgenden JSON-Daten zeigt eine typische erfolgreiche Antwortnachricht:

```json
{  
   "RecognitionStatus":"Success",
   "DisplayText":"Go shopping tomorrow.",
   "Offset":16000000,
   "Duration":17100000
}
```

In diesem Beispiel, in die JSON-Antwort deserialisiert wird eine `SpeechResult` -Instanz, mit das Ergebnis an die aufrufende Methode für die Anzeige zurückgegeben wird, wie in den folgenden Screenshots gezeigt:

![](speech-recognition-images/speech-recognition.png "Spracherkennung")

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie der REST-API von Microsoft zu verwenden, um Audiodaten in Text in einer Xamarin.Forms-Anwendung zu konvertieren. Zusätzlich zur Spracherkennung ausführt, können die Microsoft-Spracheingabe-API auch Text in gesprochenen Wörtern konvertieren.

## <a name="related-links"></a>Verwandte Links

- [Dokumentation zu Microsoft Speech API](/azure/cognitive-services/speech/home/).
- [Nutzen eines Rest-Webdiensts](~/xamarin-forms/data-cloud/web-services/rest.md)
- [TODO-Cognitive-Services (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)
