---
title: Emotionen-Erkennung mit Emotionen-API
description: "Emotionen-API einen Gesichtsausdruck Bestandteil eines Abbilds als Eingabe akzeptiert und Vertrauensgrade auf eine Reihe von Emotionen für jede Fläche in der Abbildung gibt. In diesem Artikel erläutert, wie die Emotionen-API verwenden, um Emotionen, um zu bewerten, eine Xamarin.Forms-Anwendung zu erkennen."
ms.topic: article
ms.prod: xamarin
ms.assetid: 19D36A7C-E8D8-43D1-BE80-48DE6C02879A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 159bd1b23eb7505c5d5629570a34d54e0525567e
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="emotion-recognition-using-the-emotion-api"></a>Emotionen-Erkennung mit Emotionen-API

_Emotionen-API einen Gesichtsausdruck Bestandteil eines Abbilds als Eingabe akzeptiert und Vertrauensgrade auf eine Reihe von Emotionen für jede Fläche in der Abbildung gibt. In diesem Artikel erläutert, wie die Emotionen-API verwenden, um Emotionen, um zu bewerten, eine Xamarin.Forms-Anwendung zu erkennen._

![](~/media/shared/preview.png "Diese API ist derzeit Vorabversion")

> [!NOTE]
> Die Emotionen-API ist noch in der Vorschau. Es kann an die API vor der endgültigen Version um unterbrechende Änderungen werden.

## <a name="overview"></a>Übersicht

Emotionen-API können Anger, Contempt Abschicken, befürchten, Glück, Neutral, Sadness und Überraschung eines Gesichtsausdrucks erkennen. Diese Emotionen werden über die gleichen grundlegenden Gesichtsausdrücke universell und cross-culturally übermittelt werden. Sowie ein Emotionen-Ergebnis für einen Gesichtsausdruck zurückgibt, gibt die Emotionen-API auch einen Begrenzungsrahmen für erkannte gesichtsabbildungen mithilfe der API Gesicht zurück. Wenn ein Benutzer bereits die Fläche-API aufgerufen hat, können sie das vordere Rechteck als optionale Eingabe senden. Beachten Sie, dass ein API-Schlüssel abgerufen werden muss, um die Emotionen-API zu verwenden. Dies kann abgerufen werden, auf [Einstieg kostenlos](https://www.microsoft.com/cognitive-services/sign-up) auf "Microsoft.com".

Emotionen-Erkennung kann über eine Clientbibliothek und über eine REST-API ausgeführt werden. Dieser Artikel konzentriert sich auf die Durchführung Emotionen Erkennung über die [Microsoft.ProjectOxford.Emotion](https://www.nuget.org/packages/Microsoft.ProjectOxford.Emotion/) -Clientbibliothek, die von NuGet heruntergeladen werden können.

Die Emotionen-API kann auch verwendet werden, erkennen die Gesichtsausdrücke von Personen in Video und gibt einen Überblick über ihre Emotionen. Weitere Informationen finden Sie unter [Emotionen im Video](https://www.microsoft.com/cognitive-services/emotion-api/documentation#emotion-in-video) auf "Microsoft.com".

Weitere Informationen zur Emotionen-API finden Sie unter [Emotionen-API-Dokumentation](https://www.microsoft.com/cognitive-services/emotion-api/documentation) auf "Microsoft.com".

## <a name="performing-emotion-recognition"></a>Emotionen Recognition ausführen

Emotionen Recognition erfolgt durch einen Bildstream an die API Emotionen hochladen. Die Größe der Image darf nicht größer als 4MB betragen, und die unterstützten Dateiformate sind, JPEG, PNG, GIF und BMP. In der Abbildung ist erkennbar Gesicht Größenbereichs 36 x 36 auf 4096 x 4096 Pixel. Alle Flächen außerhalb dieses Bereichs wird nicht erkannt werden.

Im folgenden Codebeispiel wird veranschaulicht, der Erkennungsvorgang Emotionen:

```csharp
using Microsoft.ProjectOxford.Emotion;
using Microsoft.ProjectOxford.Emotion.Contract;

var emotionClient = new EmotionServiceClient(Constants.EmotionApiKey);

using (var photoStream = photo.GetStream())
{
  Emotion[] emotionResult = await emotionClient.RecognizeAsync(photoStream);
  if (emotionResult.Any())
  {
    // Emotions detected are happiness, sadness, surprise, anger, fear, contempt, disgust, or neutral.
    emotionResultLabel.Text = emotionResult.FirstOrDefault().Scores.ToRankedList().FirstOrDefault().Key;
  }
  // Store emotion as app rating
  ...
}
```

Ein `EmotionServiceClient` Instanz muss erstellt werden, um Emotionen Recognition, mit dem Emotionen-API-Schlüssel, der als Argument übergebenen Ausführen der `EmotionServiceClient` Konstruktor.

Die `RecognizeAsync` -Methode, die aufgerufen wird, auf die `EmotionServiceClient` Instanz ist, ein Bild an der API Emotionen hochladen, als eine `Stream`. API-Schlüssel wird an die Emotionen-API übermittelt werden, wenn dieser Vorgang aufgerufen wird. Fehler beim Übermitteln eines gültigen API-Schlüssels führt zu einem `Microsoft.ProjectOxford.Common.ClientException` ausgelöst wird, mit der Ausnahme, die angibt, dass ein ungültiger API-Schlüssel übermittelt wurde.

Die `RecognizeAsync` Methodenrückgabewert wird ein `Emotion` array, vorausgesetzt, dass ein Schriftbild erkannt wurde. Für jedes Bild die maximale Anzahl von gesichtsabbildungen, die erkannt werden können, beträgt 64, und die Gesichter nach Gesicht Rechteck Größe in absteigender Reihenfolge geordnet sind. Wenn keine Gesicht erkannt wird, eine leere `Emotion` Array zurückgegeben werden.

Beim Interpretieren der Ergebnisse aus der Emotionen-API, die erkannte Emotionen interpretiert werden soll als die Emotionen mit der höchsten Bewertung wie Bewertungen normalisiert werden zu einer Summe. Daher zeigt die beispielanwendung die erkannten Emotionen mit der höchsten Bewertung für die größte erkannte Oberfläche in der Abbildung, wie in den folgenden Screenshots dargestellt:

![](emotion-recognition-images/emotion-recognition.png "Emotionen-Erkennung")

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird erläutert, wie die Emotionen-API verwenden, um Emotionen, um zu bewerten, eine Xamarin.Forms-Anwendung zu erkennen. Emotionen-API einen Gesichtsausdruck Bestandteil eines Abbilds als Eingabe akzeptiert und gibt das Vertrauen auf eine Reihe von Emotionen für jede Fläche in der Abbildung.


## <a name="related-links"></a>Verwandte Links

- [Emotionen-API-Dokumentation](https://www.microsoft.com/cognitive-services/emotion-api/documentation)
- [TODO-Cognitive Services (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Microsoft.ProjectOxford.Emotion](https://www.nuget.org/packages/Microsoft.ProjectOxford.Emotion/)
- [Emotionen-API](https://dev.projectoxford.ai/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa)
