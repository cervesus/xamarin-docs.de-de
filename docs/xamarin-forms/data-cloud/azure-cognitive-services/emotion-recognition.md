---
title: ''
description: Der Gesichtserkennungs-API nimmt einen Gesichtsausdruck in einem Bild als Eingabe an und gibt Daten zurück, die Vertrauens Ebenen für eine Reihe von Emotionen für jedes Gesicht im Bild enthalten. In diesem Artikel wird erläutert, wie Sie mit dem Gesichtserkennungs-API Emotionen erkennen, um eine-Anwendung zu bewerten Xamarin.Forms .
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ff384605b35f6406b628da99de500b550da811c9
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136057"
---
# <a name="perceived-emotion-recognition-using-the-face-api"></a>Wahrgenommene Emotionen erkennen mithilfe der Gesichtserkennungs-API

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)

Der Gesichtserkennungs-API kann eine Emotions Erkennung durchführen, um Ärger, Verachtung, Verschleierung, Angst, Glück, neutral, traurig und überraschend in einem Gesichtsausdruck zu erkennen, der auf wahrgenommenen Anmerkungen durch menschliche Programmierer basiert. Es ist jedoch wichtig zu beachten, dass Gesichtsausdrücke nicht notwendigerweise die internen Zustände von Personen darstellen.

Zusätzlich zum Zurückgeben eines Emotions Ergebnisses für einen Gesichtsausdruck kann die Gesichtserkennungs-API auch ein Begrenzungsfeld für erkannte Gesichter zurückgeben.

Die Emotions Erkennung kann über eine Client Bibliothek und über eine Rest-API durchgeführt werden. Dieser Artikel konzentriert sich auf die Durchführung der Emotions Erkennung über die Rest-API. Weitere Informationen zur Rest-API finden Sie unter [Face Rest API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

Der Gesichtserkennungs-API kann auch verwendet werden, um die Gesichtsausdrücke von Personen in Videos zu erkennen, und kann eine Zusammenfassung ihrer Emotionen zurückgeben. Weitere Informationen finden Sie unter [Analysieren von Videos in Echtzeit](/azure/cognitive-services/face/face-api-how-to-topics/howtoanalyzevideo_face/).

> [!NOTE]
> Wenn Sie kein [Azure-Abonnement](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing) besitzen, erstellen Sie ein [kostenloses Konto](https://aka.ms/azfree-docs-mobileapps), bevor Sie beginnen.

Ein API-Schlüssel muss für die Verwendung des Gesichtserkennungs-API abgerufen werden. Dies kann bei [try Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=face-api)abgerufen werden.

Weitere Informationen zum Gesichtserkennungs-API finden Sie unter [Gesichtserkennungs-API](/azure/cognitive-services/face/overview/).

## <a name="authentication"></a>Authentifizierung

Jede Anforderung, die an den Gesichtserkennungs-API gerichtet ist, erfordert einen API-Schlüssel, der als Wert des-Headers angegeben werden muss `Ocp-Apim-Subscription-Key` . Im folgenden Codebeispiel wird gezeigt, wie der API-Schlüssel dem `Ocp-Apim-Subscription-Key` Header einer Anforderung hinzugefügt wird:

```csharp
public FaceRecognitionService()
{
  _client = new HttpClient();
  _client.DefaultRequestHeaders.Add("ocp-apim-subscription-key", Constants.FaceApiKey);
}
```

Wenn ein gültiger API-Schlüssel nicht an die Gesichtserkennungs-API übergeben wird, führt dies zu einem Fehler der 401-Antwort.

## <a name="perform-emotion-recognition"></a>Durchführen der Emotions Erkennung

Die Emotions Erkennung wird durchgeführt, indem eine Post-Anforderung mit einem Bild an die `detect` API unter gesendet `https://[location].api.cognitive.microsoft.com/face/v1.0` wird, wobei `[location]]` die Region ist, die Sie zum Abrufen ihres API-Schlüssels verwendet haben. Die optionalen Anforderungs Parameter lauten wie folgt:

- `returnFaceId`– Gibt an, ob fakeids der erkannten Gesichter zurückgegeben werden sollen. Standardwert: `true`.
- `returnFaceLandmarks`– Gibt an, ob Gesichtspunkte der erkannten Gesichter zurückgegeben werden sollen. Standardwert: `false`.
- `returnFaceAttributes`– Gibt an, ob ein oder mehrere angegebene Gesichts Attribute analysiert und zurückgegeben werden sollen. Zu den unterstützten Gesichts Attributen zählen `age` , `gender` , `headPose` , `smile` , `facialHair` , `glasses` , `emotion` , `hair` , `makeup` , `occlusion` , `accessories` , `blur` , `exposure` und `noise` . Beachten Sie, dass die Gesichts Attribut Analyse zusätzliche Berechnungs-und Zeit Kosten hat.

Bildinhalte müssen im Text der Post-Anforderung als URL oder Binärdaten abgelegt werden.

> [!NOTE]
> Unterstützte Bild Dateiformate sind JPEG, PNG, GIF und BMP, und die zulässige Dateigröße liegt zwischen 1 KB und 4 MB.

In der Beispielanwendung wird der Emotions Erkennungsprozess aufgerufen, indem die- `DetectAsync` Methode aufgerufen wird:

```csharp
Face[] faces = await _faceRecognitionService.DetectAsync(photoStream, true, false, new FaceAttributeType[] { FaceAttributeType.Emotion });
```

Dieser Methodenaufrufe gibt den Stream mit den Bilddaten an, die faceids zurückgegeben werden sollen, die Gesichtspunkte sollten nicht zurückgegeben werden, und die Emotionen des Bilds sollten analysiert werden. Außerdem wird angegeben, dass die Ergebnisse als Array von-Objekten zurückgegeben werden `Face` . Die- `DetectAsync` Methode ruft wiederum die Rest-API auf, die die `detect` Emotions Erkennung durchführt:

```csharp
public async Task<Face[]> DetectAsync(Stream imageStream, bool returnFaceId, bool returnFaceLandmarks, IEnumerable<FaceAttributeType> returnFaceAttributes)
{
  var requestUrl =
    $"{Constants.FaceEndpoint}/detect?returnFaceId={returnFaceId}" +
    "&returnFaceLandmarks={returnFaceLandmarks}" +
    "&returnFaceAttributes={GetAttributeString(returnFaceAttributes)}";
  return await SendRequestAsync<Stream, Face[]>(HttpMethod.Post, requestUrl, imageStream);
}
```

Diese Methode generiert einen Anforderungs-URI und sendet die Anforderung dann `detect` über die-Methode an die API `SendRequestAsync` .

> [!NOTE]
> Sie müssen die gleiche Region in ihren Gesichtserkennungs-API aufrufen verwenden, die Sie zum Abrufen Ihrer Abonnement Schlüssel verwendet haben. Wenn Sie Ihre Abonnement Schlüssel z. b. aus der `westus` Region abgerufen haben, ist der Gesichts Erkennungs Endpunkt `https://westus.api.cognitive.microsoft.com/face/v1.0/detect` .

### <a name="send-the-request"></a>Senden der Anforderung

Die `SendRequestAsync` -Methode stellt die Post-Anforderung an den Gesichtserkennungs-API und gibt das Ergebnis als- `Face` Array zurück:

```csharp
async Task<TResponse> SendRequestAsync<TRequest, TResponse>(HttpMethod httpMethod, string requestUrl, TRequest requestBody)
{
  var request = new HttpRequestMessage(httpMethod, Constants.FaceEndpoint);
  request.RequestUri = new Uri(requestUrl);
  if (requestBody != null)
  {
    if (requestBody is Stream)
    {
      request.Content = new StreamContent(requestBody as Stream);
      request.Content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
    }
    else
    {
      // If the image is supplied via a URL
      request.Content = new StringContent(JsonConvert.SerializeObject(requestBody, s_settings), Encoding.UTF8, "application/json");
    }
  }

  HttpResponseMessage responseMessage = await _client.SendAsync(request);
  if (responseMessage.IsSuccessStatusCode)
  {
    string responseContent = null;
    if (responseMessage.Content != null)
    {
      responseContent = await responseMessage.Content.ReadAsStringAsync();
    }
    if (!string.IsNullOrWhiteSpace(responseContent))
    {
      return JsonConvert.DeserializeObject<TResponse>(responseContent, s_settings);
    }
    return default(TResponse);
  }
  else
  {
    ...
  }
  return default(TResponse);
}
```

Wenn das Bild über einen Stream bereitgestellt wird, erstellt die-Methode die Post-Anforderung, indem der Bildstream in eine-Instanz umgeschrieben wird `StreamContent` , die HTTP-Inhalt basierend auf einem Stream bereitstellt. Wenn das Bild über eine URL bereitgestellt wird, erstellt die-Methode die Post-Anforderung, indem die URL in eine-Instanz umgebunden wird `StringContent` , die HTTP-Inhalt auf Grundlage einer Zeichenfolge bereitstellt.

Die Post-Anforderung wird dann an die `detect` API gesendet. Die Antwort wird gelesen, deserialisiert und an die Aufruf Methode zurückgegeben.

Die `detect` API sendet in der Antwort den HTTP-Statuscode 200 (OK), vorausgesetzt, die Anforderung ist gültig, was angibt, dass die Anforderung erfolgreich war und die angeforderten Informationen in der Antwort enthalten sind. Eine Liste möglicher Fehler Antworten finden Sie unter [Face Rest API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

### <a name="process-the-response"></a>Verarbeiten der Antwort

Die API-Antwort wird im JSON-Format zurückgegeben. Die folgenden JSON-Daten zeigen eine typische erfolgreiche Antwortnachricht, die die von der Beispielanwendung angeforderten Daten bereitstellt:

```json
[  
   {  
      "faceId":"8a1a80fe-1027-48cf-a7f0-e61c0f005051",
      "faceRectangle":{  
         "top":192,
         "left":164,
         "width":339,
         "height":339
      },
      "faceAttributes":{  
         "emotion":{  
            "anger":0.0,
            "contempt":0.0,
            "disgust":0.0,
            "fear":0.0,
            "happiness":1.0,
            "neutral":0.0,
            "sadness":0.0,
            "surprise":0.0
         }
      }
   }
]
```

Eine erfolgreiche Antwortnachricht besteht aus einem Array von Gesichts Einträgen, das nach dem Rechteck in absteigender Reihenfolge sortiert wird, während eine leere Antwort darauf hinweist, dass keine Gesichter erkannt werden. Jedes erkannte Gesicht enthält eine Reihe optionaler Gesichts Attribute, die durch das- `returnFaceAttributes` Argument der-Methode angegeben werden `DetectAsync` .

In der Beispielanwendung wird die JSON-Antwort in ein Array von-Objekten deserialisiert `Face` . Beim Interpretieren der Ergebnisse aus der Gesichtserkennungs-API sollte die erkannte Emotionen als Emotionen mit der höchsten Bewertung interpretiert werden, da die Ergebnisse normalisiert werden, um zu einer Summe zu addieren. Aus diesem Grund zeigt die Beispielanwendung die erkannte Emotionen mit der höchsten Bewertung für das größte erkannte Gesicht im Bild an. Verwenden Sie dafür den folgenden Code:

```csharp
emotionResultLabel.Text = faces.FirstOrDefault().FaceAttributes.Emotion.ToRankedList().FirstOrDefault().Key;
```

Der folgende Screenshot zeigt das Ergebnis des Emotions Erkennungsprozesses in der Beispielanwendung:

![](emotion-recognition-images/emotion-recognition.png "Emotion Recognition")

## <a name="related-links"></a>Verwandte Links

- [Gesichtserkennungs-API](/azure/cognitive-services/face/overview/).
- [TODO-Cognitive Services (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)
- [Face-Rest-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
