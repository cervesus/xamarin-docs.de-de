---
title: Verwenden die Rechtschreibprüfung für die Bing-API-Rechtschreibprüfung
description: Bing-Rechtschreibprüfung führt kontextbezogene Rechtschreibprüfung für Text, die mit Inline-Empfehlungen für falsch geschriebene Wörter. In diesem Artikel erläutert die Bing Rechtschreibprüfung überprüfen REST-API verwenden, um Rechtschreibfehler in einer Xamarin.Forms-Anwendung zu korrigieren.
ms.prod: xamarin
ms.assetid: B40EB103-FDC0-45C6-9940-FB4ACDC2F4F9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 41bd79b22aa193dd5303847997bc07e8e8d12e58
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="spell-checking-using-the-bing-spell-check-api"></a>Verwenden die Rechtschreibprüfung für die Bing-API-Rechtschreibprüfung

_Bing-Rechtschreibprüfung führt kontextbezogene Rechtschreibprüfung für Text, die mit Inline-Empfehlungen für falsch geschriebene Wörter. In diesem Artikel erläutert die Bing Rechtschreibprüfung überprüfen REST-API verwenden, um Rechtschreibfehler in einer Xamarin.Forms-Anwendung zu korrigieren._

## <a name="overview"></a>Übersicht

Die Bing Rechtschreibprüfung überprüfen REST-API bietet zwei Modi für den Betrieb und ein Modus muss angegeben werden, wenn eine Anforderung an die API ausführen:

- `Spell` behebt kurze Texte (bis zu 9 Wörter) ohne Schreibweise Änderungen zu speichern.
- `Proof` langen Text korrigiert, bietet Schreibweise Korrekturen und grundlegende Interpunktion und aggressive Korrekturen unterdrückt.

Ein API-Schlüssel muss zum Verwenden der Bing Rechtschreibprüfung aktivieren-API abgerufen werden. Dies kann abgerufen werden, auf [Cognitive Services versuchen](https://azure.microsoft.com/try/cognitive-services/)

Eine Liste der von der Bing Rechtschreibprüfung aktivieren-API unterstützten Sprachen finden Sie [unterstützte Sprachen](/azure/cognitive-services/bing-spell-check/bing-spell-check-supported-languages/). Weitere Informationen über die Bing Rechtschreibprüfung aktivieren-API finden Sie unter [Bing Rechtschreibprüfung überprüfen Dokumentation](/azure/cognitive-services/bing-spell-check/).

## <a name="authentication"></a>Authentifizierung

Jeder Anforderung an die Bing Rechtschreibprüfung aktivieren-API erfordert einen API-Schlüssel, der als Wert des angegeben werden, sollte die `Ocp-Apim-Subscription-Key` Header. Das folgende Codebeispiel veranschaulicht das hinzufügen den API-Schlüssel für die `Ocp-Apim-Subscription-Key` -Header der Anforderung:

```csharp
public BingSpellCheckService()
{
    httpClient = new HttpClient();
    httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", Constants.BingSpellCheckApiKey);
}
```

Fehler einen gültigen API-Schlüssel an die Bing Rechtschreibprüfung aktivieren-API übergeben führt zu einem Fehler 401-Antwort.

## <a name="performing-spell-checking"></a>Ausführen der Rechtschreibprüfung

Rechtschreibprüfung kann erreicht werden, indem Sie eine GET oder POST-Anforderung zum Herstellen der `SpellCheck` -API bei `https://api.cognitive.microsoft.com/bing/v7.0/SpellCheck`. Wenn Sie eine GET-Anforderung ausführen, wird der Text, der Rechtschreibprüfung überprüft werden als Abfrageparameter gesendet. Wenn Sie eine POST-Anforderung ausführen, wird der Text, der Rechtschreibprüfung überprüft werden im Anforderungstext gesendet. GET-Anforderungen sind 1.500 Zeichen umfassen, die Abfrage Parameter Zeichenfolge Länge Einschränkung für die Rechtschreibprüfung beschränkt. Aus diesem Grund sollten POST-Anforderungen in der Regel vorgenommen werden, es sei denn, kurze Zeichenfolgen Rechtschreibprüfung überprüft werden.

In der beispielanwendung die `SpellCheckTextAsync` Methode ruft die Rechtschreibprüfung Prozess:

```csharp
public async Task<SpellCheckResult> SpellCheckTextAsync(string text)
{
    string requestUri = GenerateRequestUri(Constants.BingSpellCheckEndpoint, text, SpellCheckMode.Spell);
    var response = await SendRequestAsync(requestUri);
    var spellCheckResults = JsonConvert.DeserializeObject<SpellCheckResult>(response);
    return spellCheckResults;
}
```

Die `SpellCheckTextAsync` Methode generiert einen Anforderungs-URI und sendet dann die Anforderung an die `SpellCheck` -API, die eine JSON-Antwort mit dem Ergebnis zurückgibt. Die JSON-Antwort wird deserialisiert, mit der Rückgabe an die aufrufende Methode für die Anzeige.

### <a name="configuring-spell-checking"></a>Konfigurieren der Rechtschreibprüfung

Prozess für die Rechtschreibprüfung kann durch Angeben von Abfrageparametern für HTTP konfiguriert werden:

```csharp
string GenerateRequestUri(string spellCheckEndpoint, string text, SpellCheckMode mode)
{
  string requestUri = spellCheckEndpoint;
  requestUri += string.Format("?text={0}", text);                         // text to spell check
  requestUri += string.Format("&mode={0}", mode.ToString().ToLower());    // spellcheck mode - proof or spell
  return requestUri;
}
```

Diese Methode legt den Text an die Rechtschreibprüfung aktiviert und den Überprüfungsmodus zur Rechtschreibprüfung verwendet werden.

Weitere Informationen über die Bing Rechtschreibprüfung überprüfen REST-API finden Sie unter [Rechtschreibprüfung überprüfen v7-APIs](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference/).

### <a name="sending-the-request"></a>Senden der Anforderung

Die `SendRequestAsync` Methode stellt die GET-Anforderung an die Bing Rechtschreibprüfung überprüfen REST-API und gibt die Antwort zurück:

```csharp
async Task<string> SendRequestAsync(string url)
{
    var response = await httpClient.GetAsync(url);
    return await response.Content.ReadAsStringAsync();
}
```

Mit dieser Methode wird die GET-Anforderung durch Hinzufügen des API-Schlüssels als Wert für die `Ocp-Apim-Subscription-Key` Header. Die GET-Anforderung wird dann gesendet, um die `SpellCheck` -API, mit der Anforderungs-URL angeben des Texts zu übersetzende und den Überprüfungsmodus zur Rechtschreibprüfung verwendet. Die Antwort wird dann gelesen und an die aufrufende Methode zurückgegeben.

Die `SpellCheck` API wird HTTP-Statuscode 200 (OK) gesendet, in der Antwort angegeben, dass die Anforderung gültig ist, was bedeutet, dass die Anforderung erfolgreich war und dass die angeforderte Informationen in der Antwort ist. Eine Liste von Objekten der Antwort, finden Sie unter [Antwortobjekte](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#response-objects).

### <a name="processing-the-response"></a>Verarbeiten der Antwort

Die API-Antwort im JSON-Format zurückgegeben. Die folgenden JSON-Daten enthalten, die Antwortnachricht für das falsch geschriebenen Text `Go shappin tommorow`:

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

Die `flaggedTokens` Array enthält ein Array von Wörtern im Text, der als grammatisch falsch sind oder nicht richtig geschrieben gekennzeichnet wurden. Das Array ist, leer, wenn keine Rechtschreib- oder Grammatikfehler gefunden werden. Die Tags innerhalb des Arrays sind:

- `offset` – eine nullbasierte Offset vom Anfang der Zeichenfolge auf das Wort, das gekennzeichnet wurde.
- `token` – das Wort in die Textzeichenfolge, die nicht richtig geschrieben ist, oder grammatisch falsch ist.
- `type` – Der Typ des Fehlers, der das Wort gekennzeichnet wird verursacht hat. Es gibt zwei mögliche Werte: `RepeatedToken` und `UnknownToken`.
- `suggestions` – ein Array von Wörtern, die den Rechtschreibung und Grammatik Fehler behoben wird. Das Array besteht aus einem `suggestion` und ein `score`, gibt das Maß an Gewissheit, dass die vorgeschlagene Korrektur richtig an.

In der beispielanwendung wird deserialisiert die JSON-Antwort in eine `SpellCheckResult` -Instanz, mit der Rückgabe an die aufrufende Methode für die Anzeige. Im folgenden Codebeispiel wird veranschaulicht wie die `SpellCheckResult` Instanz für die Anzeige verarbeitet wird:

```csharp
var spellCheckResult = await bingSpellCheckService.SpellCheckTextAsync(TodoItem.Name);
foreach (var flaggedToken in spellCheckResult.FlaggedTokens)
{
  TodoItem.Name = TodoItem.Name.Replace(flaggedToken.Token, flaggedToken.Suggestions.FirstOrDefault().Suggestion);
}
```

Dieser Code iteriert durch die `FlaggedTokens` Auflistung und ersetzt alle falsch geschrieben oder grammatisch falsch geschriebene Wörter im Quelltext, mit der erste Vorschlag. Die folgenden Screenshots zeigen vor und nach der Rechtschreibprüfung:

![](spell-check-images/before-spell-check.png "Vor der Rechtschreibprüfung")

![](spell-check-images/after-spell-check.png "Nach der Rechtschreibprüfung")

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie die Bing Rechtschreibprüfung überprüfen REST-API verwenden, um Rechtschreibfehler in einer Xamarin.Forms-Anwendung zu korrigieren. Bing-Rechtschreibprüfung führt kontextbezogene Rechtschreibprüfung für Text, die mit Inline-Empfehlungen für falsch geschriebene Wörter.

## <a name="related-links"></a>Verwandte Links

- [Bing Rechtschreibprüfung Sie in der Dokumentation](/azure/cognitive-services/bing-spell-check/)
- [Nutzen einen RESTful-Webdienst](~/xamarin-forms/data-cloud/consuming/rest.md)
- [TODO-Cognitive Services (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Bing Rechtschreibprüfung überprüfen v7-APIs](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference/)
