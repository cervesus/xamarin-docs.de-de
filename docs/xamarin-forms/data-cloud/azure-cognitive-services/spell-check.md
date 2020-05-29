---
title: ''
description: Bing-Rechtschreibprüfung führt die kontextbezogene Rechtschreibprüfung für Text durch und stellt Inline Vorschläge für falsch geschriebene Wörter bereit. In diesem Artikel wird erläutert, wie Sie die Bing-Rechtschreibprüfung Rest-API verwenden, um Rechtschreibfehler in einer-Anwendung zu korrigieren Xamarin.Forms .
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1703f0049408381a86da73fb28696ef8708cc790
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139294"
---
# <a name="spell-checking-using-the-bing-spell-check-api"></a>Rechtschreibprüfung mit der Bing-Rechtschreibprüfung-API

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)

_Bing-Rechtschreibprüfung führt die kontextbezogene Rechtschreibprüfung für Text durch und stellt Inline Vorschläge für falsch geschriebene Wörter bereit. In diesem Artikel wird erläutert, wie Sie die Bing-Rechtschreibprüfung Rest-API verwenden, um Rechtschreibfehler in einer-Anwendung zu korrigieren Xamarin.Forms ._

## <a name="overview"></a>Übersicht

Die Bing-Rechtschreibprüfung-Rest-API verfügt über zwei Betriebsmodi, und ein Modus muss angegeben werden, wenn Sie eine Anforderung an die API senden:

- `Spell`korrigiert kurzen Text (bis zu 9 Wörter) ohne Änderungen der Groß-/Kleinschreibung.
- `Proof`korrigiert langen Text, bietet Korrekturen der Groß-und Kleinschreibung und unterdrückt aggressive Korrekturen.

> [!NOTE]
> Wenn Sie kein [Azure-Abonnement](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing) besitzen, erstellen Sie ein [kostenloses Konto](https://aka.ms/azfree-docs-mobileapps), bevor Sie beginnen.

Ein API-Schlüssel muss abgerufen werden, um die Bing-Rechtschreibprüfung-API zu verwenden. Dies kann beim Versuch abgerufen werden [Cognitive Services](https://azure.microsoft.com/try/cognitive-services/)

Eine Liste der Sprachen, die von der Bing-Rechtschreibprüfung-API unterstützt werden, finden Sie [unter Unterstützte Sprachen](/azure/cognitive-services/bing-spell-check/bing-spell-check-supported-languages/). Weitere Informationen zur Bing-Rechtschreibprüfung-API finden Sie in der [Bing-Rechtschreibprüfung-Dokumentation](/azure/cognitive-services/bing-spell-check/).

## <a name="authentication"></a>Authentifizierung

Jede Anforderung, die an die Bing-Rechtschreibprüfung-API gerichtet ist, erfordert einen API-Schlüssel, der als Wert für den-Header angegeben werden muss `Ocp-Apim-Subscription-Key` . Im folgenden Codebeispiel wird gezeigt, wie der API-Schlüssel dem `Ocp-Apim-Subscription-Key` Header einer Anforderung hinzugefügt wird:

```csharp
public BingSpellCheckService()
{
    httpClient = new HttpClient();
    httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", Constants.BingSpellCheckApiKey);
}
```

Wenn ein gültiger API-Schlüssel nicht an die Bing-Rechtschreibprüfung-API übergeben wird, führt dies zu einem Fehler der 401-Antwort.

## <a name="performing-spell-checking"></a>Ausführen von Rechtschreibprüfung

Die Rechtschreibprüfung kann durch Ausführen einer Get-oder Post-Anforderung an die `SpellCheck` API unter erreicht werden `https://api.cognitive.microsoft.com/bing/v7.0/SpellCheck` . Beim Ausführen einer GET-Anforderung wird der Text, der als Rechtschreibprüfung aktiviert werden soll, als Abfrage Parameter gesendet. Wenn eine Post-Anforderung gesendet wird, wird der Text, der als Rechtschreibprüfung markiert werden soll, im Anforderungs Text gesendet. Get-Anforderungen sind aufgrund der Einschränkung der Zeichen folgen Länge von Abfrage Parametern auf die Rechtschreibprüfung 1500 Zeichen beschränkt. Daher sollten Post-Anforderungen normalerweise erfolgen, es sei denn, für kurze Zeichen folgen wird ein Rechtschreibprüfung durchgeführt

In der Beispielanwendung Ruft die- `SpellCheckTextAsync` Methode den Rechtschreib Überprüfungsprozess auf:

```csharp
public async Task<SpellCheckResult> SpellCheckTextAsync(string text)
{
    string requestUri = GenerateRequestUri(Constants.BingSpellCheckEndpoint, text, SpellCheckMode.Spell);
    var response = await SendRequestAsync(requestUri);
    var spellCheckResults = JsonConvert.DeserializeObject<SpellCheckResult>(response);
    return spellCheckResults;
}
```

Die `SpellCheckTextAsync` -Methode generiert einen Anforderungs-URI und sendet die Anforderung dann an die `SpellCheck` API, die eine JSON-Antwort mit dem Ergebnis zurückgibt. Die JSON-Antwort wird deserialisiert, wobei das Ergebnis zur Anzeige an die aufrufende Methode zurückgegeben wird.

### <a name="configuring-spell-checking"></a>Konfigurieren der Rechtschreibprüfung

Der Rechtschreib Überprüfungsprozess kann durch Angabe von HTTP-Abfrage Parametern konfiguriert werden:

```csharp
string GenerateRequestUri(string spellCheckEndpoint, string text, SpellCheckMode mode)
{
  string requestUri = spellCheckEndpoint;
  requestUri += string.Format("?text={0}", text);                         // text to spell check
  requestUri += string.Format("&mode={0}", mode.ToString().ToLower());    // spellcheck mode - proof or spell
  return requestUri;
}
```

Diese Methode legt den Text auf die Rechtschreibprüfung und den Rechtschreib Prüfungs Modus fest.

Weitere Informationen zur Bing-Rechtschreibprüfung Rest-API finden Sie unter [Rechtschreibprüfungs-API V7-Referenz](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference/).

### <a name="sending-the-request"></a>Senden der Anforderung

`SendRequestAsync`Mit der-Methode wird die Get-Anforderung an die Bing-Rechtschreibprüfung Rest-API und die Antwort zurückgegeben:

```csharp
async Task<string> SendRequestAsync(string url)
{
    var response = await httpClient.GetAsync(url);
    return await response.Content.ReadAsStringAsync();
}
```

Diese Methode sendet die Get-Anforderung an die `SpellCheck` API, wobei die Anforderungs-URL den zu über setzenden Text und den Rechtschreib Prüfungs Modus angibt. Die Antwort wird dann gelesen und an die Aufruf Methode zurückgegeben.

Die `SpellCheck` API sendet in der Antwort den HTTP-Statuscode 200 (OK), vorausgesetzt, die Anforderung ist gültig, was angibt, dass die Anforderung erfolgreich war und die angeforderten Informationen in der Antwort enthalten sind. Eine Liste der Antwort Objekte finden Sie unter [Response Objects](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#response-objects).

### <a name="processing-the-response"></a>Verarbeiten der Antwort

Die API-Antwort wird im JSON-Format zurückgegeben. Die folgenden JSON-Daten zeigen die Antwortnachricht für den falsch geschriebenen Text an `Go shappin tommorow` :

```json
{  
   "_type":"SpellCheck",
   "flaggedTokens":[  
      {  
         "offset":3,
         "token":"shappin",
         "type":"UnknownToken",
         "suggestions":[  
            {  
               "suggestion":"shopping",
               "score":1
            }
         ]
      },
      {  
         "offset":11,
         "token":"tommorow",
         "type":"UnknownToken",
         "suggestions":[  
            {  
               "suggestion":"tomorrow",
               "score":1
            }
         ]
      }
   ],
   "correctionType":"High"
}
```

Das `flaggedTokens` Array enthält ein Array von Wörtern im Text, die als nicht ordnungsgemäß geschrieben oder falsch geschrieben wurden. Das Array ist leer, wenn keine Rechtschreib-oder Grammatikfehler gefunden werden. Die Tags innerhalb des Arrays lauten:

- `offset`– ein NULL basierter Offset vom Anfang der Text Zeichenfolge bis zu dem Wort, das gekennzeichnet wurde.
- `token`– das Wort in der Text Zeichenfolge, das nicht richtig geschrieben oder falsch geschrieben ist.
- `type`– der Typ des Fehlers, der bewirkt hat, dass das Wort gekennzeichnet wurde. Es gibt zwei mögliche Werte – `RepeatedToken` und `UnknownToken` .
- `suggestions`– ein Array von Wörtern, mit dem der Rechtschreib-oder Grammatikfehler korrigiert wird. Das Array besteht aus einem `suggestion` -und einem- `score` Wert, der den Grad der Gewissheit angibt, dass die vorgeschlagene Korrektur korrekt ist.

In der Beispielanwendung wird die JSON-Antwort in eine-Instanz deserialisiert `SpellCheckResult` , wobei das Ergebnis zur Anzeige an die aufrufende Methode zurückgegeben wird. Im folgenden Codebeispiel wird gezeigt, wie die- `SpellCheckResult` Instanz zur Anzeige verarbeitet wird:

```csharp
var spellCheckResult = await bingSpellCheckService.SpellCheckTextAsync(TodoItem.Name);
foreach (var flaggedToken in spellCheckResult.FlaggedTokens)
{
  TodoItem.Name = TodoItem.Name.Replace(flaggedToken.Token, flaggedToken.Suggestions.FirstOrDefault().Suggestion);
}
```

Dieser Code durchläuft die `FlaggedTokens` -Auflistung und ersetzt alle falsch geschriebenen oder ungültigen Wörter im Quelltext durch den ersten Vorschlag. Die folgenden Screenshots zeigen vor und nach der Rechtschreibprüfung:

![](spell-check-images/before-spell-check.png "Before Spell Check")

![](spell-check-images/after-spell-check.png "After Spell Check")

> [!NOTE]
> Im obigen Beispiel `Replace` wird der Einfachheit halber verwendet, aber über eine große Menge an Text kann das falsche Token ersetzt werden. Die API stellt den `offset` Wert bereit, der in Produktions-Apps verwendet werden soll, um die richtige Position im Quelltext zum Ausführen eines Updates zu identifizieren.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie Sie die Bing-Rechtschreibprüfung Rest-API verwenden, um Rechtschreibfehler in einer-Anwendung zu korrigieren Xamarin.Forms . Bing-Rechtschreibprüfung führt die kontextbezogene Rechtschreibprüfung für Text durch und stellt Inline Vorschläge für falsch geschriebene Wörter bereit.

## <a name="related-links"></a>Verwandte Links

- [Dokumentation zu Bing-Rechtschreibprüfung](/azure/cognitive-services/bing-spell-check/)
- [Nutzen eines Rest-Webdiensts](~/xamarin-forms/data-cloud/web-services/rest.md)
- [TODO-Cognitive Services (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)
- [Referenz zur Bing-Rechtschreibprüfung-API V7](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference/)
