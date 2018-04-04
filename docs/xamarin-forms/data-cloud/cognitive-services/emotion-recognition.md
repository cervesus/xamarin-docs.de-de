---
title: Emotionen-Erkennung, die mit dem Zifferblatt der API
description: Die Fläche-API einen Gesichtsausdruck Bestandteil eines Abbilds als Eingabe akzeptiert und gibt Daten, die über einen Satz von Emotionen für jede Fläche in der Abbildung Vertrauensgrade enthält. Dieser Artikel beschreibt die Verwendung der Oberfläche-API Emotionen, um zu bewerten, eine Xamarin.Forms-Anwendung zu erkennen.
ms.prod: xamarin
ms.assetid: 19D36A7C-E8D8-43D1-BE80-48DE6C02879A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 49e53425dbaf3aadd74d02ab030929e3311c7c8c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="emotion-recognition-using-the-face-api"></a>Emotionen-Erkennung, die mit dem Zifferblatt der API

_Die Fläche-API einen Gesichtsausdruck Bestandteil eines Abbilds als Eingabe akzeptiert und gibt Daten, die über einen Satz von Emotionen für jede Fläche in der Abbildung Vertrauensgrade enthält. Dieser Artikel beschreibt die Verwendung der Oberfläche-API Emotionen, um zu bewerten, eine Xamarin.Forms-Anwendung zu erkennen._

## <a name="overview"></a>Übersicht

Die Fläche-API können Emotionen Erkennung erkennen Anger, Contempt, Abschicken, befürchten, Glück neutrale Sadness und Überraschung in einen Gesichtsausdruck ausführen. Diese Emotionen werden über die gleichen grundlegenden Gesichtsausdrücke universell und cross-culturally übermittelt werden. Sowie ein Emotionen-Ergebnis für einen Gesichtsausdruck zurückgegeben, können die Fläche-API auch gibt einen Begrenzungsrahmen für erkannte gesichtsabbildungen. Beachten Sie, dass ein API-Schlüssel abgerufen werden muss, um die Fläche-API zu verwenden. Dies kann abgerufen werden, auf [Cognitive Services versuchen](https://azure.microsoft.com/try/cognitive-services/?api=face-api).

Emotionen-Erkennung kann über eine Clientbibliothek und über eine REST-API ausgeführt werden. Dieser Artikel konzentriert sich auf die Durchführung Emotionen Erkennung über die [Microsoft.ProjectOxford.Face](https://www.nuget.org/packages/Microsoft.ProjectOxford.Face/) -Clientbibliothek, die von NuGet heruntergeladen werden können.

Die Fläche-API kann auch verwendet werden, erkennen die Gesichtsausdrücke von Personen in Video und einen Überblick über ihre Emotionen zurückgeben kann. Weitere Informationen finden Sie unter [zum Analysieren von Videos in Echtzeit](/azure/cognitive-services/face/face-api-how-to-topics/howtoanalyzevideo_face/).

Weitere Informationen über die Oberfläche-API finden Sie unter [Gesicht API](/azure/cognitive-services/face/overview/).

## <a name="performing-emotion-recognition"></a>Emotionen Recognition ausführen

Emotionen Recognition erfolgt durch einen Bildstream an die API Gesicht hochladen. Die Größe der Image darf nicht größer als 4MB betragen, und die unterstützten Dateiformate sind, JPEG, PNG, GIF und BMP.

Im folgenden Codebeispiel wird veranschaulicht, der Erkennungsvorgang Emotionen:

```csharp
using Microsoft.ProjectOxford.Face;
using Microsoft.ProjectOxford.Face.Contract;

var faceServiceClient = new FaceServiceClient(Constants.FaceApiKey, Constants.FaceEndpoint);
// e.g. var faceServiceClient = new FaceServiceClient("a3dbe2ed6a5a9231bb66f9a964d64a12", "https://westus.api.cognitive.microsoft.com/face/v1.0/detect");

var faceAttributes = new FaceAttributeType[] { FaceAttributeType.Emotion };
using (var photoStream = photo.GetStream())
{
    Face[] faces = await faceServiceClient.DetectAsync(photoStream, true, false, faceAttributes);
    if (faces.Any())
    {
        // Emotions detected are happiness, sadness, surprise, anger, fear, contempt, disgust, or neutral.
        emotionResultLabel.Text = faces.FirstOrDefault().FaceAttributes.Emotion.ToRankedList().FirstOrDefault().Key;
    }
    // Store emotion as app rating
    ...
}
```

Ein `FaceServiceClient` Instanz erstellt werden muss, zum Ausführen von Emotionen-Erkennung, mit der Oberfläche API-Schlüssel und der Endpunkt, die als Argumente übergeben werden die `FaceServiceClient` Konstruktor.

> [!NOTE]
> Sie müssen in Ihre Gesicht-API-Aufrufe wie verwendet, um Ihre Abonnement-Schlüssel erhalten die gleiche Region verwenden. Z. B., wenn Sie Ihre Abonnement-Schlüssel von erworben haben die `westus` Region der Fläche Erkennung Endpunkt werden `https://westus.api.cognitive.microsoft.com/face/v1.0/detect`.

Die `DetectAsync` -Methode, die aufgerufen wird, auf die `FaceServiceClient` Instanz ist, ein Bild an der API Gesicht hochladen, als eine `Stream`. API-Schlüssel wird an die Oberfläche-API übermittelt werden, wenn dieser Vorgang aufgerufen wird. Fehler beim Übermitteln eines gültigen API-Schlüssels führt zu einem `Microsoft.ProjectOxford.Face.FaceAPIException` ausgelöst wird, mit der Ausnahme, die angibt, dass ein ungültiger API-Schlüssel übermittelt wurde.

Die `DetectAsync` Methodenrückgabewert wird ein `Face` array, vorausgesetzt, dass ein Schriftbild erkannt wurde. Jede zurückgegebene Gesicht enthält ein Rechteck, um seine Position, kombiniert mit einer Reihe von optionalen Gesicht-Attribute, die vom angegebenen anzugeben, die `faceAttributes` Argument an die `DetectAsync` Methode. Wenn keine Gesicht erkannt wird, eine leere `Face` Array zurückgegeben werden.

Beim Interpretieren der Ergebnisse aus der Fläche-API, die erkannte Emotionen interpretiert werden soll als die Emotionen mit der höchsten Bewertung wie Bewertungen normalisiert werden zu einer Summe. Daher zeigt die beispielanwendung die erkannten Emotionen mit der höchsten Bewertung für die größte erkannte Oberfläche in der Abbildung, wie in den folgenden Screenshots dargestellt:

![](emotion-recognition-images/emotion-recognition.png "Emotionen-Erkennung")

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird erläutert, wie die Fläche-API verwenden, um Emotionen, um zu bewerten, eine Xamarin.Forms-Anwendung zu erkennen. Die Fläche-API einen Gesichtsausdruck Bestandteil eines Abbilds als Eingabe akzeptiert und gibt Daten, die über einen Satz von Emotionen für jede Fläche in der Abbildung ist die Gewissheit enthält.

## <a name="related-links"></a>Verwandte Links

- [API werdender](/azure/cognitive-services/face/overview/).
- [TODO-Cognitive Services (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Microsoft.ProjectOxford.Face](https://www.nuget.org/packages/Microsoft.ProjectOxford.Face/)
