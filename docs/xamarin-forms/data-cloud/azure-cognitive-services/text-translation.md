---
title: Übersetzung von Text mit der Translator-API
description: Die Microsoft Translator-API kann verwendet werden, um Sprache und Text über eine REST-API zu übersetzen. In diesem Artikel wird erläutert, wie Sie die Microsoft Textübersetzungs-API verwenden, um Text aus einer Sprache in eine andere in einer Xamarin.Forms-Anwendung zu übersetzen.
ms.prod: xamarin
ms.assetid: 68330242-92C5-46F1-B1E3-2395D8823B0C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 841b1d4abab5e4c09249174b221da20794771a86
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725568"
---
# <a name="text-translation-using-the-translator-api"></a>Übersetzung von Text mit der Translator-API

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)

_Die Microsoft Translator-API kann verwendet werden, um Sprache und Text über eine Rest-API zu übersetzen. In diesem Artikel wird erläutert, wie Sie mit dem Microsoft Textübersetzungs-API Text in einer xamarin. Forms-Anwendung in eine andere Sprache übersetzen._

## <a name="overview"></a>Übersicht

Die Translator-API besteht aus zwei Komponenten:

- Eine textübersetzung REST-API zum Text von einer Sprache in Text, der eine andere Sprache übersetzen. Die API erkennt automatisch die Sprache des Texts, die gesendet wurde, bevor Sie übersetzt.
- Eine sprachübersetzung, Spracherkennung, von einer Sprache in Text einer anderen Sprache zu transkribieren-REST-API. Darüber hinaus integriert die API auch Text-to-Speech-Funktionen, damit der übersetzte Text in Form einer Audioaufnahme zurückübertragen werden kann.

Dieser Artikel konzentriert sich auf die Übersetzung von einer Sprache in eine andere mithilfe der Textübersetzungs-API.

> [!NOTE]
> Wenn Sie kein [Azure-Abonnement](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing) besitzen, erstellen Sie ein [kostenloses Konto](https://aka.ms/azfree-docs-mobileapps), bevor Sie beginnen.

API-Schlüssel muss abgerufen werden, um die Textübersetzungs-API zu verwenden. Informationen hierzu finden Sie unter [registrieren für die Microsoft-Textübersetzungs-API](/azure/cognitive-services/translator/translator-text-how-to-signup/).

Weitere Informationen zum Microsoft-Textübersetzungs-API finden Sie in der [Textübersetzungs-API-Dokumentation](/azure/cognitive-services/translator/).

## <a name="authentication"></a>Authentication

Jede Anforderung, die an den Textübersetzungs-API erfolgt, erfordert ein JSON Web Token (JWT)-Zugriffs Token, das beim `https://api.cognitive.microsoft.com/sts/v1.0/issueToken`vom Cognitive Services-Tokendienst abgerufen werden kann. Ein Token kann abgerufen werden, indem eine Post-Anforderung an den Tokendienst gesendet wird. dabei wird eine `Ocp-Apim-Subscription-Key` Kopfzeile angegeben, die den API-Schlüssel als Wert enthält.

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

Das Zugriffs Token muss in jedem Textübersetzungs-API Aufrufen als `Authorization` Header angegeben werden, dem die Zeichenfolge `Bearer`vorangestellt ist, wie im folgenden Codebeispiel gezeigt:

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
```

Weitere Informationen zum Cognitive Services-Tokendienst finden Sie unter [Authentifizierung](/azure/cognitive-services/translator/reference/v3-0-reference#authentication).

## <a name="performing-text-translation"></a>Durchführen der Übersetzung von Texteingabe

Die Text Übersetzung kann erreicht werden, indem Sie eine GET-Anforderung an die `translate`-API bei `https://api.microsofttranslator.com/v2/http.svc/translate`senden. In der Beispielanwendung Ruft die `TranslateTextAsync`-Methode den Text Übersetzungsprozess auf:

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

Die `TranslateTextAsync`-Methode generiert einen Anforderungs-URI und ruft ein Zugriffs Token vom Tokendienst ab. Die Text Übersetzungs Anforderung wird dann an die `translate`-API gesendet, die eine XML-Antwort zurückgibt, die das Ergebnis enthält. Die XML-Antwort wird analysiert, und das übersetzungsergebnis an die aufrufende Methode für die Anzeige zurückgegeben.

Weitere Informationen zu den Rest-APIs für die Text Übersetzung finden Sie unter [Textübersetzungs-API](/azure/cognitive-services/translator/reference/v3-0-reference).

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

Diese Methode wird der Text übersetzt werden, und die Sprache, die den Text zu übersetzen. Eine Liste der Sprachen, die von Microsoft Translator unterstützt werden, finden Sie [unter Unterstützte Sprachen in der Microsoft-Textübersetzungs-API](/azure/cognitive-services/translator/languages/).

> [!NOTE]
> Wenn eine Anwendung wissen muss, in welcher Sprache sich der Text befindet, kann die `Detect`-API aufgerufen werden, um die Sprache der Text Zeichenfolge zu ermitteln.

### <a name="sending-the-request"></a>Senden der Anforderung

Die `SendRequestAsync`-Methode führt die Get-Anforderung an die Rest-API der Text Übersetzung aus und gibt die Antwort zurück:

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

Diese Methode erstellt die Get-Anforderung, indem das Zugriffs Token dem `Authorization`-Header hinzugefügt wird, dem die Zeichenfolge `Bearer`vorangestellt ist. Die Get-Anforderung wird dann an die `translate`-API gesendet, wobei die Anforderungs-URL den Text angibt, der übersetzt werden soll, und die Sprache, in die der Text übersetzt werden soll. Die Antwort ist dann gelesen und an die aufrufende Methode zurückgegeben.

Die `translate`-API sendet in der Antwort den HTTP-Statuscode 200 (OK), vorausgesetzt, die Anforderung ist gültig, was angibt, dass die Anforderung erfolgreich war und die angeforderten Informationen in der Antwort enthalten sind. Eine Liste möglicher Fehler Antworten finden Sie unter Response messages at [Get Translation](/azure/cognitive-services/translator/reference/v3-0-translate).

### <a name="processing-the-response"></a>Verarbeiten der Antwort

Die API-Antwort wird im XML-Format zurückgegeben. Die folgenden XML-Daten zeigt eine typische erfolgreiche Antwortnachricht:

```xml
<string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Morgen kaufen gehen ein</string>
```

In der Beispielanwendung wird die XML-Antwort in eine `XDocument`-Instanz analysiert, wobei der XML-Stamm Wert an die aufrufende Methode für die Anzeige zurückgegeben wird, wie in den folgenden Screenshots gezeigt:

![](text-translation-images/text-translation.png "Text Translation to German")

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie die Microsoft Textübersetzungs-API verwenden, um Text aus einer Sprache in Text von einer anderen Sprache in einer Xamarin.Forms-Anwendung zu übersetzen. Text zu übersetzen, kann die Microsoft Translator-API auch Sprache von einer Sprache in Text einer anderen Sprache transkribieren.

## <a name="related-links"></a>Verwandte Links

- [Dokumentation zu Textübersetzungs-API](/azure/cognitive-services/translator/)
- [Nutzen eines Rest-Webdiensts](~/xamarin-forms/data-cloud/web-services/rest.md)
- [TODO-Cognitive Services (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)
- [Textübersetzungs-API](/azure/cognitive-services/translator/reference/v3-0-reference)
