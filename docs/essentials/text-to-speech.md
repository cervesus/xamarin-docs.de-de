---
title: 'Xamarin.Essentials: Text-zu-Sprache'
description: Mit der TextToSpeech-Klasse in Xamarin.Essentials kann eine Anwendung die integrierten Text-to-Speech-Engines verwenden, um Text auf dem Gerät wiederzugeben und verfügbare Sprachen abzufragen, die die Engine unterstützt.
ms.assetid: AEEF03AE-A047-4DF0-B0E8-CC8D9A7B8351
author: jamesmontemagno
ms.custom: video
ms.author: jamont
ms.date: 11/04/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 452a54637c270f80c2e1add4d6cadedbb4b27077
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84801828"
---
# <a name="xamarinessentials-text-to-speech"></a>Xamarin.Essentials: Text-zu-Sprache

Mit der Klasse **TextToSpeech** kann eine Anwendung die integrierten Text-to-Speech-Engines verwenden, um Text auf dem Gerät wiederzugeben und verfügbare Sprachen abzufragen, die die Engine unterstützt.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-text-to-speech"></a>Verwenden von Text-to-Speech

Fügen Sie in Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

Text-to-Speech ruft die `SpeakAsync`-Methode mit Text und optionalen Parametern auf und wird nach dem Ende der Äußerung zurückgegeben.

```csharp
public async Task SpeakNowDefaultSettings()
{
    await TextToSpeech.SpeakAsync("Hello World");

    // This method will block until utterance finishes.
}

public void SpeakNowDefaultSettings2()
{
    TextToSpeech.SpeakAsync("Hello World").ContinueWith((t) =>
    {
        // Logic that will run after utterance finishes.

    }, TaskScheduler.FromCurrentSynchronizationContext());
}
```

Diese Methode beinhaltet ein optionales `CancellationToken`-Element, um die Äußerung zu beenden, sobald sie beginnt.

```csharp
CancellationTokenSource cts;
public async Task SpeakNowDefaultSettings()
{
    cts = new CancellationTokenSource();
    await TextToSpeech.SpeakAsync("Hello World", cancelToken: cts.Token);

    // This method will block until utterance finishes.
}

// Cancel speech if a cancellation token exists & hasn't been already requested.
public void CancelSpeech()
{
    if (cts?.IsCancellationRequested ?? true)
        return;

    cts.Cancel();
}
```

Text-to-Speech fügt Sprachanforderungen vom gleichen Thread automatisch der Warteschlange hinzu.

```csharp
bool isBusy = false;
public void SpeakMultiple()
{
    isBusy = true;
    Task.Run(async () =>
    {
        await TextToSpeech.SpeakAsync("Hello World 1");
        await TextToSpeech.SpeakAsync("Hello World 2");
        await TextToSpeech.SpeakAsync("Hello World 3");
        isBusy = false;
    });

    // or you can query multiple without a Task:
    Task.WhenAll(
        TextToSpeech.SpeakAsync("Hello World 1"),
        TextToSpeech.SpeakAsync("Hello World 2"),
        TextToSpeech.SpeakAsync("Hello World 3"))
        .ContinueWith((t) => { isBusy = false; }, TaskScheduler.FromCurrentSynchronizationContext());
}
```

### <a name="speech-settings"></a>Spracheinstellungen

Sie können gut steuern, wie Audio mit `SpeechOptions` zurückgegeben wird, womit Lautstärke, Tonhöhe und Gebietsschema festgelegt werden können.

```csharp
public async Task SpeakNow()
{
    var settings = new SpeechOptions()
        {
            Volume = .75f,
            Pitch = 1.0f
        };

    await TextToSpeech.SpeakAsync("Hello World", settings);
}
```

Die folgenden Werte werden für diese Parameter unterstützt:

| Parameter | Minimum | Maximum |
| --- | :---: | :---: |
| Tonhöhe | 0 | 2.0 |
| Lautstärke | 0 | 1.0 |

### <a name="speech-locales"></a>Gebietsschema der Sprache

Jede Plattform unterstützt verschiedene Gebietsschemas, um Text in verschiedenen Sprachen und Akzenten wiederzugeben. Plattformen verfügen über unterschiedliche Codes und Möglichkeiten zur Angabe des Gebietsschemas. Deshalb werden in Xamarin.Essentials eine plattformübergreifende `Locale`-Klasse und die Abfrageoption mit `GetLocalesAsync` bereitgestellt.

```csharp
public async Task SpeakNow()
{
    var locales = await TextToSpeech.GetLocalesAsync();

    // Grab the first locale
    var locale = locales.FirstOrDefault();

    var settings = new SpeechOptions()
        {
            Volume = .75f,
            Pitch = 1.0f,
            Locale = locale
        };

    await TextToSpeech.SpeakAsync("Hello World", settings);
}
```

## <a name="limitations"></a>Einschränkungen

- Die Warteschlange für Äußerungen ist nicht gewährleistet, wenn sie über mehrere Threads aufgerufen wird.
- Die Audiowiedergabe im Hintergrund wird offiziell nicht unterstützt.

## <a name="api"></a>API

- [TextToSpeech-Quellcode](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/TextToSpeech)
- [TextToSpeech-API-Dokumentation](xref:Xamarin.Essentials.TextToSpeech)

## <a name="related-video"></a>Zugehörige Videos

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Text-to-Speech-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
