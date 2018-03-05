---
title: "Verwenden das API-Konvertierungsprogramm Übersetzung von Text"
description: "Die Microsoft Translator-API kann verwendet werden, um Spracherkennung und Text über einen REST-API zu übersetzen. In diesem Artikel wird erläutert, wie der Ausdrucksübersetzungs-API von Microsoft Text Übersetzen von Text von einer Sprache in eine andere in einer Xamarin.Forms-Anwendung verwendet wird."
ms.topic: article
ms.prod: xamarin
ms.assetid: 68330242-92C5-46F1-B1E3-2395D8823B0C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: f403ebaffdf742c22e61b73aee7a42648fe597dc
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="text-translation-using-the-translator-api"></a>Verwenden das API-Konvertierungsprogramm Übersetzung von Text

_Die Microsoft Translator-API kann verwendet werden, um Spracherkennung und Text über einen REST-API zu übersetzen. In diesem Artikel wird erläutert, wie der Ausdrucksübersetzungs-API von Microsoft Text Übersetzen von Text von einer Sprache in eine andere in einer Xamarin.Forms-Anwendung verwendet wird._

## <a name="overview"></a>Übersicht

Der Konvertierer-API besteht aus zwei Komponenten:

- Eine textübersetzung REST-API zum Text von einer Sprache in Text, der eine andere Sprache übersetzen. Die API erkennt automatisch die Sprache des Texts, die gesendet wurde, bevor umgewandelt wird.
- Eine Übersetzung der Sprache zu Sprache, die von einer Sprache in einer anderen Sprache Text transkribieren-REST-API. Die API integriert auch-Sprach-Funktionen, um wieder die übersetzten Text gesprochen werden soll.

Dieser Artikel konzentriert sich auf die Übersetzung von einer Sprache in eine andere mithilfe der Ausdrucksübersetzungs-API von Text.

Ein API-Schlüssel muss zum Verwenden der Text Übersetzung API abgerufen werden. Dadurch erhalten Sie, indem die Anweisungen unter folgendem [Einstieg](http://docs.microsofttranslator.com/text-translate.html) auf [docs.microsofttranslator.com](http://docs.microsofttranslator.com/).

Weitere Informationen zur Microsoft Translator-API finden Sie unter [Microsoft Translator-Dokumentation](https://www.microsoft.com/cognitive-services/translator-api/documentation/TranslatorInfo/overview) auf "Microsoft.com".

## <a name="authentication"></a>Authentifizierung

Jeder Anforderung an den Text Ausdrucksübersetzungs-API erfordert eine JSON-Webtoken (JWT)-Zugriffstoken, die aus den cognitive token Services am abgerufen werden kann `https://api.cognitive.microsoft.com/sts/v1.0/issueToken`. Ein Token erhalten Sie, indem Sie eine POST-Anforderung an den Sicherheitstokendienst angeben einer `Ocp-Apim-Subscription-Key` Header, der den API-Schlüssel als Wert enthält.

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

Das Zugriffstoken muss angegeben werden, in jeder Text Ausdrucksübersetzungs-API aufrufen, wie ein `Authorization` Header, die die Zeichenfolge mit dem Präfix `Bearer`, wie im folgenden Codebeispiel wird gezeigt:

```csharp
using (var httpClient = new HttpClient())
{
  httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
  ...
}  
```

Weitere Informationen zu den cognitive token Services, finden Sie unter [Authentifizierung Token API](http://docs.microsofttranslator.com/oauth-token.html) auf [docs.microsofttranslator.com](http://docs.microsofttranslator.com/).

## <a name="performing-text-translation"></a>In Textform durchführen

Textübersetzung erreichen, indem Sie eine GET-Anforderung zum Ausgeben der `Translate` -API bei `https://api.microsofttranslator.com/v2/http.svc/Translate`. In der beispielanwendung die `TranslateTextAsync` Methode ruft den Textvorlagen Übersetzung ab:

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

Die `TranslateTextAsync` Methode generiert einen Anforderungs-URI und ein Zugriffstoken vom token-Dienst abgerufen. Die Text Übersetzung-Anforderung wird dann gesendet, um die `Translate` -API, die eine XML-Antwort mit dem Ergebnis zurückgibt. Die XML-Antwort wird analysiert, und das Ergebnis der Übersetzung an die aufrufende Methode für die Anzeige zurückgegeben wird.

Weitere Informationen zu den Text Übersetzung REST-APIs finden Sie unter [Beispielcode](http://docs.microsofttranslator.com/text-translate.html#/default) auf [docs.microsofttranslator.com](http://docs.microsofttranslator.com/).

### <a name="configuring-text-translation"></a>Konfigurieren der Übersetzung von Text

Des Übersetzungsprozesses Text kann durch Angeben von Abfrageparametern für HTTP konfiguriert werden. Es gibt obligatorischen und optionalen Parameter, durch die folgende Methode zeigt die obligatorischen Parameter, die festgelegt werden müssen:

```csharp
string GenerateRequestUri(string endpoint, string text, string to)
{
  string requestUri = endpoint;
  requestUri += string.Format("?text={0}", Uri.EscapeUriString(text));
  requestUri += string.Format("&to={0}", to);
  return requestUri;
}
```

Diese Methode wird der Text übersetzt werden und die Sprache zum Übersetzen des Texts zu. Eine Liste der Sprachen, die von Microsoft Translator unterstützt, finden Sie unter [Sprachen](https://www.microsoft.com/translator/languages.aspx) auf "Microsoft.com".

> [!NOTE]
> Wenn eine Anwendung erfordert, welche Sprache wissen, ob der Text in der `Detect` API aufgerufen werden, um die Sprache der Textzeichenfolge erkennen.

Weitere Informationen zu den obligatorischen und optionalen Parametern finden Sie unter [Text Ausdrucksübersetzungs-API](http://docs.microsofttranslator.com/text-translate.html#!/default/get_Translate) auf [docs.microsofttranslator.com](http://docs.microsofttranslator.com/).

### <a name="sending-the-request"></a>Senden der Anforderung

Die `SendRequestAsync` Methode stellt die GET-Anforderung an die REST-API von Text Übersetzung und gibt die Antwort zurück:

```csharp
async Task<string> SendRequestAsync(string url, string bearerToken)
{
  using (var httpClient = new HttpClient())
  {
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
    var response = await httpClient.GetAsync(url);
    return await response.Content.ReadAsStringAsync();
  }
}
```

Diese Methode baut die GET-Anforderung das Zugriffstoken an die `Authorization` -Header, die Zeichenfolge mit dem Präfix `Bearer`. Die GET-Anforderung wird dann gesendet, um die `Translate` -API, mit der Anforderungs-URL angeben des Texts zu übersetzende und die Sprache zum Übersetzen des Texts an. Die Antwort wird dann gelesen und an die aufrufende Methode zurückgegeben.

Die `Translate` API wird HTTP-Statuscode 200 (OK) gesendet, in der Antwort angegeben, dass die Anforderung gültig ist, was bedeutet, dass die Anforderung erfolgreich war und dass die angeforderte Informationen in der Antwort ist. Eine Liste der möglichen Fehlerantworten, finden Sie unter Antwortnachrichten an [übersetzen abrufen](http://docs.microsofttranslator.com/text-translate.html#!/default/get_Translate) auf [docs.microsofttranslator.com](http://docs.microsofttranslator.com/).

### <a name="processing-the-response"></a>Verarbeiten der Antwort

Die API-Antwort wird im XML-Format zurückgegeben. Die folgenden XML-Daten zeigt eine typische erfolgreiche Antwortnachricht:

```xml
<string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Morgen kaufen gehen ein</string>
```

In der beispielanwendung wird die XML-Antwort in analysiert eine `XDocument` -Instanz, mit der XML-Stamm-Wert an die aufrufende Methode für die Anzeige zurückgegeben wird, wie in den folgenden Screenshots dargestellt:

![](text-translation-images/text-translation.png "Die Übersetzung von Text auf Deutsch")

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie der Ausdrucksübersetzungs-API von Microsoft Text verwenden, um Text von einer Sprache in einer anderen Sprache in einer Anwendung Xamarin.Forms-Text zu übersetzen. Die Microsoft Translator-API können Übersetzen von Text, Spracherkennung aus einer Sprache auch in einer anderen Sprache Text transkribieren.



## <a name="related-links"></a>Verwandte Links

- [Microsoft Translator-Dokumentation](https://www.microsoft.com/cognitive-services/translator-api/documentation/TranslatorInfo/overview)
- [Nutzen einen RESTful-Webdienst](~/xamarin-forms/data-cloud/consuming/rest.md)
- [TODO-Cognitive Services (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Textübersetzung API](http://docs.microsofttranslator.com/text-translate.html)
