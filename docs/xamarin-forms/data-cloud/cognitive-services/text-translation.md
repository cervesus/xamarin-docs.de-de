---
title: Übersetzung von Text mit der Translator-API
description: Die Microsoft Translator-API kann verwendet werden, um Sprache und Text über eine REST-API zu übersetzen. In diesem Artikel wird erläutert, wie Sie die Microsoft Textübersetzungs-API verwenden, um Text aus einer Sprache in eine andere in einer Xamarin.Forms-Anwendung zu übersetzen.
ms.prod: xamarin
ms.assetid: 68330242-92C5-46F1-B1E3-2395D8823B0C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 18e5b430d9a56b22a0b4cc72d6aff1c4e3049362
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61329730"
---
# <a name="text-translation-using-the-translator-api"></a>Übersetzung von Text mit der Translator-API

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)

_Die Microsoft Translator-API kann verwendet werden, um Sprache und Text über eine REST-API zu übersetzen. In diesem Artikel wird erläutert, wie Sie die Microsoft Textübersetzungs-API verwenden, um Text aus einer Sprache in eine andere in einer Xamarin.Forms-Anwendung zu übersetzen._

## <a name="overview"></a>Übersicht

Die Translator-API besteht aus zwei Komponenten:

- Eine textübersetzung REST-API zum Text von einer Sprache in Text, der eine andere Sprache übersetzen. Die API erkennt automatisch die Sprache des Texts, die gesendet wurde, bevor Sie übersetzt.
- Eine sprachübersetzung, Spracherkennung, von einer Sprache in Text einer anderen Sprache zu transkribieren-REST-API. Die API integriert auch-Sprach-Funktionen für den übersetzten Text wieder zu sprechen.

Dieser Artikel konzentriert sich auf die Übersetzung von einer Sprache in eine andere mithilfe der Textübersetzungs-API.

API-Schlüssel muss abgerufen werden, um die Textübersetzungs-API zu verwenden. Dadurch erhalten Sie unter [zum Registrieren für die Microsoft Textübersetzungs-API](/azure/cognitive-services/translator/translator-text-how-to-signup/).

Weitere Informationen über die Microsoft Textübersetzungs-API finden Sie unter [Translator Text-API-Dokumentation](/azure/cognitive-services/translator/).

## <a name="authentication"></a>Authentifizierung

Jede Anforderung die Textübersetzungs-API erfordert ein JSON Web Token (JWT)-Zugriffstoken, der vom token cognitive Services-Dienst auf abgerufen werden kann `https://api.cognitive.microsoft.com/sts/v1.0/issueToken`. Ein Token abgerufen werden kann, durch eine POST-Anforderung an den Sicherheitstokendienst angeben einer `Ocp-Apim-Subscription-Key` Header, der den API-Schlüssel als Wert enthält.

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

Das Zugriffstoken muss angegeben werden, in jeder Textübersetzungs-API Aufrufen als ein `Authorization` Header, die mit der Zeichenfolge das Präfix `Bearer`, wie im folgenden Codebeispiel gezeigt:

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
```

Weitere Informationen zu den token cognitive Services-Dienst, finden Sie unter [Authentifizierungs-Token-API](http://docs.microsofttranslator.com/oauth-token.html).

## <a name="performing-text-translation"></a>Durchführen der Übersetzung von Texteingabe

Übersetzung von Texteingabe erzielt werden, indem Sie eine GET-Anforderung an die `translate` -API zu `https://api.microsofttranslator.com/v2/http.svc/translate`. In der beispielanwendung die `TranslateTextAsync` Methode aufruft, den Übersetzungsprozess Text:

```csharp
public async Task<string> TranslateTextAsync(string text)
{
  ...
  string requestUri = GenerateRequestUri(Constants.TextTranslatorEndpoint, text, "en", "de");
  string accessToken = authenticationService.GetAccessToken();
  var response = await SendRequestAsync(requestUri, accessToken);
  var xml = XDocument.Parse(response);
  return xml.Root.Value;
}
```

Die `TranslateTextAsync` Methode generiert einen Anforderungs-URI und ruft ein Zugriffstoken vom Tokendienst ab. Anforderung der Übersetzung wird dann gesendet, um die `translate` -API, die eine XML-Antwort, die das Ergebnis zurückgibt. Die XML-Antwort wird analysiert, und das übersetzungsergebnis an die aufrufende Methode für die Anzeige zurückgegeben.

Weitere Informationen zu den Text Translation-REST-APIs, finden Sie unter [Microsoft Textübersetzungs-API](http://docs.microsofttranslator.com/text-translate.html).

### <a name="configuring-text-translation"></a>Konfigurieren die Übersetzung von Texteingabe

Der Übersetzungsprozess Text kann durch Angabe von Abfrageparametern für HTTP konfiguriert werden:

```csharp
string GenerateRequestUri(string endpoint, string text, string to)
{
  string requestUri = endpoint;
  requestUri += string.Format("?text={0}", Uri.EscapeUriString(text));
  requestUri += string.Format("&to={0}", to);
  return requestUri;
}
```

Diese Methode wird der Text übersetzt werden, und die Sprache, die den Text zu übersetzen. Eine Liste der von Microsoft Translator unterstützten Sprachen, finden Sie unter [unterstützte Sprachen in der Microsoft Translator-Text-API](/azure/cognitive-services/translator/languages/).

> [!NOTE]
> Wenn eine Anwendung muss wissen, welcher Sprache der Text in ist der `Detect` API aufgerufen werden, um die Sprache der Textzeichenfolge zu erkennen.

### <a name="sending-the-request"></a>Senden der Anforderung

Die `SendRequestAsync` Methode stellt die GET-Anforderung an die Text Translation-REST-API und die Antwort zurückgegeben:

```csharp
async Task<string> SendRequestAsync(string url, string bearerToken)
{
    if (httpClient == null)
    {
        httpClient = new HttpClient();
    }
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);

    var response = await httpClient.GetAsync(url);
    return await response.Content.ReadAsStringAsync();
}
```

Diese Methode erstellt die GET-Anforderung durch das Hinzufügen des Zugriffstokens für die `Authorization` -Header, die Zeichenfolge mit dem Präfix `Bearer`. Die GET-Anforderung wird dann gesendet, um die `translate` -API, mit der Anforderungs-URL angeben des Texts übersetzt werden, und die Sprache, die den Text zu übersetzen. Die Antwort ist dann gelesen und an die aufrufende Methode zurückgegeben.

Die `translate` API sendet HTTP-Statuscode 200 (OK) in der Antwort angegeben, dass die Anforderung gültig ist, was bedeutet, dass die Anforderung erfolgreich war, und dass die angeforderte Informationen in der Antwort ist. Eine Liste der möglichen Fehlerantworten, finden Sie unter Antwortnachrichten an [erhalten übersetzen](http://docs.microsofttranslator.com/text-translate.html#!/default/get_Translate).

### <a name="processing-the-response"></a>Verarbeiten der Antwort

Die API-Antwort wird im XML-Format zurückgegeben. Die folgenden XML-Daten zeigt eine typische erfolgreiche Antwortnachricht:

```xml
<string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Morgen kaufen gehen ein</string>
```

In der beispielanwendung wird die XML-Antwort in analysiert eine `XDocument` -Instanz mit den XML-Stamm-Wert an die aufrufende Methode für die Anzeige zurückgegeben wird, wie in den folgenden Screenshots gezeigt:

![](text-translation-images/text-translation.png "Übersetzung von Text auf Deutsch")

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie die Microsoft Textübersetzungs-API verwenden, um Text aus einer Sprache in Text von einer anderen Sprache in einer Xamarin.Forms-Anwendung zu übersetzen. Text zu übersetzen, kann die Microsoft Translator-API auch Sprache von einer Sprache in Text einer anderen Sprache transkribieren.

## <a name="related-links"></a>Verwandte Links

- [Translator-Text-API-Dokumentation](/azure/cognitive-services/translator/).
- [Verwenden eines RESTful-Web-Diensts](~/xamarin-forms/data-cloud/consuming/rest.md)
- [TODO-Cognitive-Services (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Microsoft-Textübersetzungs-API](http://docs.microsofttranslator.com/text-translate.html).
