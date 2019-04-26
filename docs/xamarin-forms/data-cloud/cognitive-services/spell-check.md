---
title: Rechtschreibprüfung mithilfe der Bing-Rechtschreibprüfungs-API
description: Bing-Rechtschreibprüfung führt kontextbezogene Rechtschreibprüfung für Text Inline Empfehlungen für falsch geschriebene Wörter. In diesem Artikel wird erläutert, wie Sie mit der Bing-Rechtschreibprüfungs REST-API, korrigieren Sie Rechtschreibfehler in einer Xamarin.Forms-Anwendung.
ms.prod: xamarin
ms.assetid: B40EB103-FDC0-45C6-9940-FB4ACDC2F4F9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 79b973784dd978bff15c7a706083c70df83ba1c8
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61330040"
---
# <a name="spell-checking-using-the-bing-spell-check-api"></a>Rechtschreibprüfung mithilfe der Bing-Rechtschreibprüfungs-API

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)

_Bing-Rechtschreibprüfung führt kontextbezogene Rechtschreibprüfung für Text Inline Empfehlungen für falsch geschriebene Wörter. In diesem Artikel wird erläutert, wie Sie mit der Bing-Rechtschreibprüfungs REST-API, korrigieren Sie Rechtschreibfehler in einer Xamarin.Forms-Anwendung._

## <a name="overview"></a>Übersicht

Die Bing-Rechtschreibprüfungs REST-API verfügt über zwei Betriebsmodi, und ein Modus muss angegeben werden, wenn eine Anforderung an die API vornehmen:

- `Spell` kurzen Text (bis zu 9 Wörter) ohne Änderungen zur Groß-und Kleinschreibung wird automatisch behoben.
- `Proof` langen Text korrigiert, bietet Schreibweise Korrekturen und grundlegende Zeichensetzung und unterdrückt die aggressive Korrekturen.

API-Schlüssel muss abgerufen werden, um die Bing-Rechtschreibprüfungs-API zu verwenden. Dadurch erhalten Sie unter [Cognitive Services testen](https://azure.microsoft.com/try/cognitive-services/)

Eine Liste der von der Bing-Rechtschreibprüfungs-API unterstützten Sprachen, finden Sie unter [unterstützte Sprachen](/azure/cognitive-services/bing-spell-check/bing-spell-check-supported-languages/). Weitere Informationen zu den Bing-Rechtschreibprüfungs-API, finden Sie unter [Rechtschreibung prüfen Dokumentation zur Bing](/azure/cognitive-services/bing-spell-check/).

## <a name="authentication"></a>Authentifizierung

Jeder Anforderung an die Bing-Rechtschreibprüfungs-API erfordert einen API-Schlüssel, die als Wert angegeben werden, sollte die `Ocp-Apim-Subscription-Key` Header. Im folgenden Codebeispiel wird veranschaulicht, wie den API-Schlüssel zum Hinzufügen der `Ocp-Apim-Subscription-Key` Header einer Anforderung:

```csharp
public BingSpellCheckService()
{
    httpClient = new HttpClient();
    httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", Constants.BingSpellCheckApiKey);
}
```

Fehler beim Übergeben eines gültigen API-Schlüssels an die Bing-Rechtschreibprüfungs-API führt zu einem Fehler 401-Antwort.

## <a name="performing-spell-checking"></a>Ausführen der Rechtschreibprüfung

Rechtschreibprüfung erzielt werden, indem eine Get- oder POST-Anforderung, die `SpellCheck` -API zu `https://api.cognitive.microsoft.com/bing/v7.0/SpellCheck`. Wenn Sie eine GET-Anforderung ausführen, wird der Text, der Rechtschreibung geprüft werden als Query-Parameter gesendet. Wenn Sie eine POST-Anforderung ausführen, wird der Text, der Rechtschreibung geprüft werden im Hauptteil Anforderung gesendet. GET-Anforderungen sind Rechtschreibprüfung 1.500 Zeichen umfassen, aufgrund der Abfrage Parameter Zeichenfolge Länge auf. Aus diesem Grund sollten die POST-Anforderungen in der Regel vorgenommen werden, es sei denn, kurze Zeichenfolgen die Rechtschreibung geprüft werden.

In der beispielanwendung die `SpellCheckTextAsync` Methode aufruft, die Rechtschreibprüfung für Prozess:

```csharp
public async Task<SpellCheckResult> SpellCheckTextAsync(string text)
{
    string requestUri = GenerateRequestUri(Constants.BingSpellCheckEndpoint, text, SpellCheckMode.Spell);
    var response = await SendRequestAsync(requestUri);
    var spellCheckResults = JsonConvert.DeserializeObject<SpellCheckResult>(response);
    return spellCheckResults;
}
```

Die `SpellCheckTextAsync` Methode generiert einen Anforderungs-URI und sendet dann die Anforderung an die `SpellCheck` -API, die eine JSON-Antwort, die das Ergebnis zurückgibt. Die JSON-Antwort wird deserialisiert, mit dem Ergebnis, das an die aufrufende Methode für die Anzeige zurückgegeben wird.

### <a name="configuring-spell-checking"></a>Konfigurieren der Rechtschreibprüfung

Prozess für die Rechtschreibprüfung kann durch Angabe von Abfrageparametern für HTTP konfiguriert werden:

```csharp
string GenerateRequestUri(string spellCheckEndpoint, string text, SpellCheckMode mode)
{
  string requestUri = spellCheckEndpoint;
  requestUri += string.Format("?text={0}", text);                         // text to spell check
  requestUri += string.Format("&mode={0}", mode.ToString().ToLower());    // spellcheck mode - proof or spell
  return requestUri;
}
```

Diese Methode wird den Text, der Rechtschreibung überprüft und die Rechtschreibprüfungs-Überprüfungsmodus sein.

Weitere Informationen zu den Bing-Rechtschreibprüfungs REST-API, finden Sie unter [Rechtschreibprüfungs-API v7 Verweis](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference/).

### <a name="sending-the-request"></a>Senden der Anforderung

Die `SendRequestAsync` Methode stellt die GET-Anforderung an die Bing-Rechtschreibprüfungs REST-API und die Antwort zurückgegeben:

```csharp
async Task<string> SendRequestAsync(string url)
{
    var response = await httpClient.GetAsync(url);
    return await response.Content.ReadAsStringAsync();
}
```

Diese Methode sendet die GET-Anforderung an die `SpellCheck` -API, mit der Anforderungs-URL angeben des Texts übersetzt werden, und die Rechtschreibprüfungs-Überprüfungsmodus. Die Antwort ist dann gelesen und an die aufrufende Methode zurückgegeben.

Die `SpellCheck` API sendet HTTP-Statuscode 200 (OK) in der Antwort angegeben, dass die Anforderung gültig ist, was bedeutet, dass die Anforderung erfolgreich war, und dass die angeforderte Informationen in der Antwort ist. Eine Liste der Antwortobjekte, finden Sie unter [Antwortobjekte](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#response-objects).

### <a name="processing-the-response"></a>Verarbeiten der Antwort

Die API-Antwort wird im JSON-Format zurückgegeben. Die folgenden JSON-Daten enthalten, die Antwortnachricht für den falsch geschriebenen Text `Go shappin tommorow`:

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

Die `flaggedTokens` Array enthält ein Array von Wörtern im Text, die als nicht richtig geschrieben werden oder sind grammatisch falsch gekennzeichnet wurden. Das Array ist, leer, wenn keine Rechtschreib- oder Grammatikfehler gefunden werden. Die Tags innerhalb des Arrays sind:

- `offset` – Ein nullbasierter Offset vom Anfang der Zeichenfolge, die das Wort, das als gekennzeichnet wurde.
- `token` – das Wort in die Textzeichenfolge, die nicht richtig geschrieben ist, oder grammatisch falsch ist.
- `type` – Der Typ des Fehlers, der das Wort angezeigt, dass verursacht. Es gibt zwei mögliche Werte: `RepeatedToken` und `UnknownToken`.
- `suggestions` – ein Array von Worten, die von der Rechtschreibung und Grammatik Fehler korrigiert wird. Das Array besteht aus einer `suggestion` und `score`, womit das Maß an Gewissheit, dass die vorgeschlagene Korrektur richtig ist.

In diesem Beispiel, in die JSON-Antwort deserialisiert wird eine `SpellCheckResult` -Instanz, mit das Ergebnis an die aufrufende Methode für die Anzeige zurückgegeben wird. Das folgende Codebeispiel zeigt die `SpellCheckResult` Instanz für die Anzeige verarbeitet wird:

```csharp
var spellCheckResult = await bingSpellCheckService.SpellCheckTextAsync(TodoItem.Name);
foreach (var flaggedToken in spellCheckResult.FlaggedTokens)
{
  TodoItem.Name = TodoItem.Name.Replace(flaggedToken.Token, flaggedToken.Suggestions.FirstOrDefault().Suggestion);
}
```

Dieser Code durchläuft die `FlaggedTokens` Auflistung und ersetzt alle falsch geschrieben oder grammatisch falsch geschriebene Wörter im Quelltext mit der erste Vorschlag. Die folgenden Screenshots zeigen vor und nach dem die Rechtschreibung überprüfen:

![](spell-check-images/before-spell-check.png "Vor der Rechtschreibprüfung")

![](spell-check-images/after-spell-check.png "Nach der Rechtschreibprüfung")

> [!NOTE]
> Im Beispiel oben wird `Replace` aus Gründen der Einfachheit, aber für eine große Menge an Text können sie das falsche Token ersetzen. Die-API bietet die `offset` Wert, der zur Identifizierung des richtigen Speicherorts im Quelltext vorkommen, Durchführen eines Updates in Produktions-apps verwendet werden soll.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie die Bing-Rechtschreibprüfungs REST-API zu verwenden, um korrigieren Sie Rechtschreibfehler in einer Xamarin.Forms-Anwendung. Bing-Rechtschreibprüfung führt kontextbezogene Rechtschreibprüfung für Text Inline Empfehlungen für falsch geschriebene Wörter.

## <a name="related-links"></a>Verwandte Links

- [Dokumentation zur Bing-Rechtschreibprüfungs-Überprüfung](/azure/cognitive-services/bing-spell-check/)
- [Verwenden eines RESTful-Web-Diensts](~/xamarin-forms/data-cloud/consuming/rest.md)
- [TODO-Cognitive-Services (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Bing-Rechtschreibprüfungs-API v7-Referenz](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference/)
