---
title: 'Xamarin.Essentials: Text-to-Speech'
description: Mit der TextToSpeech-Klasse in Xamarin.Essentials kann eine Anwendung die integrierten Text-to-Speech-Engines verwenden, um Text auf dem Gerät wiederzugeben und verfügbare Sprachen abzufragen, die die Engine unterstützt.
ms.assetid: AEEF03AE-A047-4DF0-B0E8-CC8D9A7B8351
author: jamesmontemagno
ms.author: jamont
ms.date: 08/30/2018
ms.openlocfilehash: 5b0d6bb2ab2621de5322a13b3b0e56e5bc6e93d8
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50123582"
---
# <a name="xamarinessentials-text-to-speech"></a>Xamarin.Essentials: Text-to-Speech

![NuGet-Vorabrelease](~/media/shared/pre-release.png)

Mit der Klasse **TextToSpeech** kann eine Anwendung die integrierten Text-to-Speech-Engines verwenden, um Text auf dem Gerät wiederzugeben und verfügbare Sprachen abzufragen, die die Engine unterstützt.

## <a name="using-text-to-speech"></a>Verwenden von Text-to-Speech

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

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

public void CancelSpeech()
{
    if (cts?.IsCancellationRequested ?? false)
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

Sie können gut steuern, wie Audio mit `SpeakSettings` zurückgegeben wird, womit Lautstärke, Tonhöhe und Gebietsschema festgelegt werden können.

```csharp
public async Task SpeakNow()
{
    var settings = new SpeakSettings()
        {
            Volume = .75,
            Pitch = 1.0
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

Jede Plattform unterstützt verschiedene Gebietsschemas, um Text in verschiedenen Sprachen und Akzenten wiederzugeben. Plattformen verfügen über unterschiedliche Codes und Möglichkeiten zur Angabe des Gebietsschemas. Deshalb stellt Xamarin.Essentials eine plattformübergreifende `Locale`-Klasse bereit und die Abfrageoption mit `GetLocalesAsync`.

```csharp
public async Task SpeakNow()
{
    var locales = await TextToSpeech.GetLocalesAsync();

    // Grab the first locale
    var locale = locales.FirstOrDefault();

    var settings = new SpeakSettings()
        {
            Volume = .75,
            Pitch = 1.0,
            Locale = locale
        };

    await TextToSpeech.SpeakAsync("Hello World", settings);
}
```

## <a name="limitations"></a>Einschränkungen

- Die Warteschlange für Äußerungen ist nicht gewährleistet, wenn sie über mehrere Threads aufgerufen wird.
- Die Audiowiedergabe im Hintergrund wird offiziell nicht unterstützt.

## <a name="api"></a>API

- [TextToSpeech-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/TextToSpeech)
- [TextToSpeech-API-Dokumentation](xref:Xamarin.Essentials.TextToSpeech)
