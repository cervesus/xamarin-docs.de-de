---
title: Mithilfe der Gesichtserkennungs-API zur Erkennung von Emotionen
description: Die Gesichtserkennungs-API Gesichtsausdrücke in einem Bild als Eingabe akzeptiert und gibt die Daten, die Vertrauensgrade auf eine Reihe von Emotionen für jedes Gesicht im Bild enthalten. In diesem Artikel wird erläutert, wie Sie mit der Gesichtserkennungs-API zur Erkennung von Emotionen, um eine Xamarin.Forms-Anwendung zu bewerten.
ms.prod: xamarin
ms.assetid: 19D36A7C-E8D8-43D1-BE80-48DE6C02879A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2018
ms.openlocfilehash: d703de90378991d262a4b056b9ebc98d183e3fb8
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2019
ms.locfileid: "67658727"
---
# <a name="emotion-recognition-using-the-face-api"></a>Mithilfe der Gesichtserkennungs-API zur Erkennung von Emotionen

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)

_Die Gesichtserkennungs-API Gesichtsausdrücke in einem Bild als Eingabe akzeptiert und gibt die Daten, die Vertrauensgrade auf eine Reihe von Emotionen für jedes Gesicht im Bild enthalten. In diesem Artikel wird erläutert, wie Sie mit der Gesichtserkennungs-API zur Erkennung von Emotionen, um eine Xamarin.Forms-Anwendung zu bewerten._

## <a name="overview"></a>Übersicht

Die Gesichtserkennungs-API können Erkennung von Emotionen zu erkennen, Wut, verachtung, ekel, Angst, Neutralität, traurigkeit und Überraschung, im Gesichtsausdrücke ausführen. Diese Emotionen werden allgemein und kulturübergreifend über die gleichen grundlegenden gesichtsausdrücken kommuniziert. Sowie eine Emotion-Ergebnis für Gesichtsausdrücke zurückgegeben, können der Gesichtserkennungs-API auch die gibt ein umgebendes Feld für die erkannten Gesichtern. Beachten Sie, dass ein API-Schlüssel abgerufen werden muss, um die Gesichtserkennungs-API zu verwenden. Dadurch erhalten Sie unter [Cognitive Services versuchen](https://azure.microsoft.com/try/cognitive-services/?api=face-api).

Zur Erkennung von Emotionen kann über eine Clientbibliothek und über eine REST-API ausgeführt werden. Dieser Artikel konzentriert sich auf die zur Erkennung von Emotionen über die REST-API ausführen. Weitere Informationen über die REST-API finden Sie unter [Gesichtserkennungs-REST-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

Der Gesichtserkennungs-API kann auch verwendet werden, den Gesichtsausdruck von Personen im Video zu erkennen, und Sie können eine Übersicht über deren Emotionen zurück. Weitere Informationen finden Sie unter [Vorgehensweise Analysieren von Videos in Echtzeit](/azure/cognitive-services/face/face-api-how-to-topics/howtoanalyzevideo_face/).

Weitere Informationen zu der Gesichtserkennungs-API, finden Sie unter [Gesichtserkennungs-API](/azure/cognitive-services/face/overview/).

## <a name="authentication"></a>Authentifizierung

Jede Anforderung, der Gesichtserkennungs-API erfordert einen API-Schlüssel, die als Wert angegeben werden, sollte die `Ocp-Apim-Subscription-Key` Header. Im folgenden Codebeispiel wird veranschaulicht, wie den API-Schlüssel zum Hinzufügen der `Ocp-Apim-Subscription-Key` Header einer Anforderung:

```csharp
public FaceRecognitionService()
{
  _client = new HttpClient();
  _client.DefaultRequestHeaders.Add("ocp-apim-subscription-key", Constants.FaceApiKey);
}
```

Fehler beim Übergeben eines gültigen API-Schlüssels, der Gesichtserkennungs-API führt zu einem Fehler 401-Antwort.

## <a name="performing-emotion-recognition"></a>Ausführen des zur Erkennung von Emotionen

Zur Erkennung von Emotionen wird durchgeführt, indem eine POST-Anforderung mit einem Bild an der `detect` -API zu `https://[location].api.cognitive.microsoft.com/face/v1.0`, wobei `[location]]` ist die Region, die Sie verwendet, um Ihren API-Schlüssel abzurufen. Die mit optionalen Anforderungsparameter sind:

- `returnFaceId` – ob FaceIds von der erkannten Gesichtern zurückgegeben. Der Standardwert ist `true`.
- `returnFaceLandmarks` – ob Orientierungspunkte von der erkannten Gesichtern zurückgegeben. Der Standardwert ist `false`.
- `returnFaceAttributes` – an, ob analysieren und zurückgeben, eine oder mehrere angegebene Attribute stehen. Unterstützte gesichtsmerkmale sind `age`, `gender`, `headPose`, `smile`, `facialHair`, `glasses`, `emotion`, `hair`, `makeup`, `occlusion`, `accessories`, `blur`, `exposure`, und `noise`. Beachten Sie, dass Gesicht Attribut Analysis zusätzliche Kosten für COMPUTE und Zeit.

Bildinhalt muss im Text der POST-Anforderung als URL oder binäre Daten platziert werden.

> [!NOTE]
> Unterstützte Formate für Bilddateien werden JPEG, PNG, GIF und BMP, und die zulässige Dateigröße von 1KB ist, auf 4MB.

In der beispielanwendung-Erkennungsprozess Emotionen wird aufgerufen, durch Aufrufen der `DetectAsync` Methode:

```csharp
Face[] faces = await _faceRecognitionService.DetectAsync(photoStream, true, false, new FaceAttributeType[] { FaceAttributeType.Emotion });
```

Dieser Methodenaufruf gibt an, der Stream, enthält die Bilddaten, die FaceIds zurückgegeben werden soll, dass Orientierungspunkte sollte nicht zurückgegeben werden und die Stimmung des Bilds analysiert werden sollen. Es gibt auch an, dass die Ergebnisse als ein Array von zurückgegeben werden, `Face` Objekte. Wiederum die `DetectAsync` Methode ruft die `detect` REST-API, die zur Erkennung von Emotionen ausführt:

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

Diese Methode generiert einen Anforderungs-URI und sendet dann die Anforderung an die `detect` -API über die `SendRequestAsync` Methode.

> [!NOTE]
> Sie müssen die gleiche Region in Ihren Gesichtserkennungs-API-aufrufen, als Sie zum Abrufen Ihrer Abonnementschlüssel verwenden. Z. B., wenn Sie Ihre Abonnementschlüssel aus erhalten die `westus` , der Endpunkt der gesichtserkennungs-Erkennung werden `https://westus.api.cognitive.microsoft.com/face/v1.0/detect`.

### <a name="sending-the-request"></a>Senden der Anforderung

Die `SendRequestAsync` Methode macht die POST-Anforderung der Gesichtserkennungs-API und gibt das Ergebnis als eine `Face` Array:

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

Wenn das Image über einen Datenstrom angegeben wird, erstellt die Methode die POST-Anforderung durch das wrapping des Bild-Streams in ein `StreamContent` -Instanz, die Grundlage eines Datenstroms HTTP-Inhalt enthält. Wenn das Image über eine URL angegeben ist, erstellt die Methode die POST-Anforderung auch durch umschließen die URL in eine `StringContent` -Instanz, die HTTP-Inhalt basierend auf einer Zeichenfolge bereitstellt.

Anschließend wird die POST-Anforderung an gesendet `detect` API. Die Antwort wird gelesen, deserialisiert und an die aufrufende Methode zurückgegeben.

Die `detect` API sendet HTTP-Statuscode 200 (OK) in der Antwort angegeben, dass die Anforderung gültig ist, was bedeutet, dass die Anforderung erfolgreich war, und dass die angeforderte Informationen in der Antwort ist. Eine Liste der möglichen Fehlerantworten, finden Sie unter [Gesichtserkennungs-REST-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

### <a name="processing-the-response"></a>Verarbeiten der Antwort

Die API-Antwort wird im JSON-Format zurückgegeben. Die folgenden JSON-Daten zeigt eine typische Antwort, die erfolgreich-Nachricht, die die von der beispielanwendung angeforderten Daten bereitstellt:

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

Eine erfolgreiche Antwort-Nachricht besteht aus einem Array von Face-Einträgen, die Rangfolge richtet sich nach Gesicht Rechteck Größe in absteigender Reihenfolge aus, während eine leere Antwort angibt, keine Gesichter erkannt dass. Jede erkannt Gesicht umfasst eine Reihe von optionalen gesichtserkennungs-Attribute, die vom angegebenen die `returnFaceAttributes` Argument für die `DetectAsync` Methode.

In der beispielanwendung, die JSON-Antwort deserialisiert wird, in ein Array von `Face` Objekte. Beim Interpretieren der Ergebnisse, der Gesichtserkennungs-API, der erkannten Emotionen interpretiert werden soll als die Emotion mit der höchsten Bewertung, wie Werte normalisiert werden Summe auf eine. Aus diesem Grund zeigt die beispielanwendung der erkannten Emotionen mit der höchsten Bewertung für die größten erkannte Gesicht im Bild. Dies erfolgt durch den folgenden Code:

```csharp
emotionResultLabel.Text = faces.FirstOrDefault().FaceAttributes.Emotion.ToRankedList().FirstOrDefault().Key;
```

Der folgende Screenshot zeigt das Ergebnis des Prozesses Emotionen erkennen, in der beispielanwendung:

![](emotion-recognition-images/emotion-recognition.png "Zur Erkennung von Emotionen")

## <a name="summary"></a>Zusammenfassung

In diesem Artikel erläutert, wie Sie mit der Gesichtserkennungs-API zur Erkennung von Emotionen, um eine Xamarin.Forms-Anwendung zu bewerten. Die Gesichtserkennungs-API Gesichtsausdrücke in einem Bild als Eingabe akzeptiert und gibt die Daten, die die Zuverlässigkeit der über eine Bandbreite von Emotionen für jedes Gesicht im Bild enthalten.

## <a name="related-links"></a>Verwandte Links

- [Gesichtserkennungs-API](/azure/cognitive-services/face/overview/).
- [TODO-Cognitive-Services (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Gesichtserkennungs-REST-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
