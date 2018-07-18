---
title: Emotionen-Erkennung, die mit dem Zifferblatt der API
description: Die Fläche-API einen Gesichtsausdruck Bestandteil eines Abbilds als Eingabe akzeptiert und gibt Daten, die über einen Satz von Emotionen für jede Fläche in der Abbildung Vertrauensgrade enthält. Dieser Artikel beschreibt die Verwendung der Oberfläche-API Emotionen, um zu bewerten, eine Xamarin.Forms-Anwendung zu erkennen.
ms.prod: xamarin
ms.assetid: 19D36A7C-E8D8-43D1-BE80-48DE6C02879A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2018
ms.openlocfilehash: 4dc04cb077b894b255eb496b2cb2983626573897
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
ms.locfileid: "34049765"
---
# <a name="emotion-recognition-using-the-face-api"></a>Emotionen-Erkennung, die mit dem Zifferblatt der API

_Die Fläche-API einen Gesichtsausdruck Bestandteil eines Abbilds als Eingabe akzeptiert und gibt Daten, die über einen Satz von Emotionen für jede Fläche in der Abbildung Vertrauensgrade enthält. Dieser Artikel beschreibt die Verwendung der Oberfläche-API Emotionen, um zu bewerten, eine Xamarin.Forms-Anwendung zu erkennen._

## <a name="overview"></a>Übersicht

Die Fläche-API können Emotionen Erkennung erkennen Anger, Contempt, Abschicken, befürchten, Glück neutrale Sadness und Überraschung in einen Gesichtsausdruck ausführen. Diese Emotionen werden über die gleichen grundlegenden Gesichtsausdrücke universell und cross-culturally übermittelt werden. Sowie ein Emotionen-Ergebnis für einen Gesichtsausdruck zurückgegeben, können die Fläche-API auch gibt einen Begrenzungsrahmen für erkannte gesichtsabbildungen. Beachten Sie, dass ein API-Schlüssel abgerufen werden muss, um die Fläche-API zu verwenden. Dies kann abgerufen werden, auf [Cognitive Services versuchen](https://azure.microsoft.com/try/cognitive-services/?api=face-api).

Emotionen-Erkennung kann über eine Clientbibliothek und über eine REST-API ausgeführt werden. Dieser Artikel konzentriert sich auf Emotionen Erkennung über die REST-API ausführen. Weitere Informationen über die REST-API finden Sie unter [Gesicht REST-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

Die Fläche-API kann auch verwendet werden, erkennen die Gesichtsausdrücke von Personen in Video und einen Überblick über ihre Emotionen zurückgeben kann. Weitere Informationen finden Sie unter [zum Analysieren von Videos in Echtzeit](/azure/cognitive-services/face/face-api-how-to-topics/howtoanalyzevideo_face/).

Weitere Informationen über die Oberfläche-API finden Sie unter [Gesicht API](/azure/cognitive-services/face/overview/).

## <a name="authentication"></a>Authentifizierung

Jeder Anforderung an die Oberfläche-API erfordert einen API-Schlüssel, der als Wert des angegeben werden, sollte die `Ocp-Apim-Subscription-Key` Header. Das folgende Codebeispiel veranschaulicht das hinzufügen den API-Schlüssel für die `Ocp-Apim-Subscription-Key` -Header der Anforderung:

```csharp
public FaceRecognitionService()
{
  _client = new HttpClient();
  _client.DefaultRequestHeaders.Add("ocp-apim-subscription-key", Constants.FaceApiKey);
}
```

Einen gültigen API-Schlüssel an die Oberfläche-API übergeben werden in einer Antwort 401-Fehler kommen.

## <a name="performing-emotion-recognition"></a>Emotionen Recognition ausführen

Emotionen-Erkennung wird durchgeführt, indem eine POST-Anforderung mit einem Bild an der `detect` -API bei `https://[location].api.cognitive.microsoft.com/face/v1.0`, wobei `[location]]` ist die Region, die Sie verwendet, um Ihren API-Schlüssel abzurufen. Die optionale Anforderungsparameter sind:

- `returnFaceId` – an, ob der erkannten Flächen FaceIds zurückgegeben. Der Standardwert ist `true`.
- `returnFaceLandmarks` – ob Gesicht Informationen von der erkannten gesichtsabbildungen zurückgegeben. Der Standardwert ist `false`.
- `returnFaceAttributes` – angibt, ob analysieren und zurückgeben, eine oder mehrere angegebene Attribute sind. Unterstützte Gesicht Attributen zählen `age`, `gender`, `headPose`, `smile`, `facialHair`, `glasses`, `emotion`, `hair`, `makeup`, `occlusion`, `accessories`, `blur`, `exposure`, und `noise`. Beachten Sie, dass Gesicht Attribut Analysis zusätzliche rechnerischen und Zeit Kosten.

Bildinhalt muss im Text der POST-Anforderung als URL oder binäre Daten platziert werden.

> [!NOTE]
> Unterstützte Bilddateiformate sind, JPEG, PNG, GIF und BMP, und die zulässige Dateigröße von 1KB ist, auf 4MB.

In der beispielanwendung wird aufgerufen, der Erkennungsvorgang Emotionen durch Aufrufen der `DetectAsync` Methode:

```csharp
Face[] faces = await _faceRecognitionService.DetectAsync(photoStream, true, false, new FaceAttributeType[] { FaceAttributeType.Emotion });
```

Dieser Methodenaufruf gibt an, der Stream, enthält die Bilddaten, die FaceIds zurückgegeben werden soll, dass Gesicht Informationen sollten nicht zurückgegeben werden und die Stimmung des Bilds analysiert werden soll. Es gibt auch an, dass die Ergebnisse als ein Array von zurückgegeben werden `Face` Objekte. Wiederum die `DetectAsync` Methode ruft die `detect` REST-API, die Emotionen Recognition ausführt:

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

Diese Methode generiert eine URI-Anforderung und sendet dann die Anforderung an die `detect` -API über die `SendRequestAsync` Methode.

> [!NOTE]
> Sie müssen in Ihre Gesicht-API-Aufrufe wie verwendet, um Ihre Abonnement-Schlüssel erhalten die gleiche Region verwenden. Z. B., wenn Sie Ihre Abonnement-Schlüssel von erworben haben die `westus` Region der Fläche Erkennung Endpunkt werden `https://westus.api.cognitive.microsoft.com/face/v1.0/detect`.

### <a name="sending-the-request"></a>Senden der Anforderung

Die `SendRequestAsync` Methode macht die POST-Anforderung an die API Fläche und gibt das Ergebnis als eine `Face` Array:

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

Wenn das Bild über einen Stream angegeben ist, erstellt die Methode die POST-Anforderung durch wrapping in den bilddatenstrom eine `StreamContent` -Instanz, die HTTP-Inhalt basierend auf einem Datenstrom bereitstellt. Wenn das Bild über eine URL angegeben wird, erstellt die Methode die POST-Anforderung Alternativ durch umschließen die URL in eine `StringContent` -Instanz, die HTTP-Inhalt auf der Grundlage einer Zeichenfolge enthält.

Die POST-Anforderung wird dann an gesendet `detect` API. Die Antwort wird gelesen, deserialisiert und an die aufrufende Methode zurückgegeben.

Die `detect` API wird HTTP-Statuscode 200 (OK) gesendet, in der Antwort angegeben, dass die Anforderung gültig ist, was bedeutet, dass die Anforderung erfolgreich war und dass die angeforderte Informationen in der Antwort ist. Eine Liste der möglichen Fehlerantworten, finden Sie unter [Gesicht REST-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

### <a name="processing-the-response"></a>Verarbeiten der Antwort

Die API-Antwort im JSON-Format zurückgegeben. Die folgenden JSON-Daten enthalten eine typische erfolgreiche Antwortnachricht, die die von der beispielanwendung angeforderten Daten bereitstellt:

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

Eine erfolgreiche Antwortnachricht besteht aus einem Array Gesicht Einträge mit dem höchsten Gesicht Rechteck Größe in absteigender Reihenfolge aus, während eine leere Antwort keine Flächen erkannt angibt. Jede Fläche umfasst eine Reihe von optionalen Gesicht-Attribute, die vom angegebenen erkannt die `returnFaceAttributes` Argument an die `DetectAsync` Methode.

In der beispielanwendung wird deserialisiert die JSON-Antwort in ein Array von `Face` Objekte. Beim Interpretieren der Ergebnisse aus der Fläche-API, die erkannte Emotionen interpretiert werden soll als die Emotionen mit der höchsten Bewertung wie Bewertungen normalisiert werden zu einer Summe. Die beispielanwendung zeigt daher die erkannten Emotionen mit der höchsten Bewertung für die größte erkannte Oberfläche in der Abbildung an. Dies erfolgt durch den folgenden Code:

```csharp
emotionResultLabel.Text = faces.FirstOrDefault().FaceAttributes.Emotion.ToRankedList().FirstOrDefault().Key;
```

Das folgende Bildschirmfoto zeigt das Ergebnis des Prozesses Recognition Emotionen in der beispielanwendung:

![](emotion-recognition-images/emotion-recognition.png "Emotionen-Erkennung")

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird erläutert, wie die Fläche-API verwenden, um Emotionen, um zu bewerten, eine Xamarin.Forms-Anwendung zu erkennen. Die Fläche-API einen Gesichtsausdruck Bestandteil eines Abbilds als Eingabe akzeptiert und gibt Daten, die über einen Satz von Emotionen für jede Fläche in der Abbildung ist die Gewissheit enthält.

## <a name="related-links"></a>Verwandte Links

- [API werdender](/azure/cognitive-services/face/overview/).
- [TODO-Cognitive Services (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [REST-API-Oberfläche](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
