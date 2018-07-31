---
title: 'Xamarin.Essentials: Sprachsynthese'
description: Die TextToSpeech-Klasse in Xamarin.Essentials ermöglicht eine Anwendung nutzen die integrierten im Text-Sprach-Engines Back Text sprechen, auf dem Gerät und auch Abfragen verfügbaren Sprachen, die die Engine unterstützen kann.
ms.assetid: AEEF03AE-A047-4DF0-B0E8-CC8D9A7B8351
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: ba822870edafce44140caa66b01f4da242fb7779
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353612"
---
# <a name="xamarinessentials-text-to-speech"></a>Xamarin.Essentials: Sprachsynthese

![Vorabversionen von NuGet](~/media/shared/pre-release.png)

Die **TextToSpeech** Klasse ermöglicht einer Anwendung, die den integrierten nutzen in Text-Sprach-Engines Back Text sprechen, auf dem Gerät und auch Abfragen verfügbaren Sprachen, die die Engine unterstützen kann.

## <a name="using-text-to-speech"></a>Verwenden die Sprachsynthese

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Die Funktionalität für die Sprachsynthese erfolgt durch Aufrufen der `SpeakAsync` -Methode mit Text und optionale Parameter und gibt nach Abschluss der Äußerung. 

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

Diese Methode akzeptiert einen optionalen `CancellationToken` der Äußerung zu beenden, sobald sie gestartet wird.

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

Sprachsynthese wird automatisch die sprachanforderungen aus demselben Thread Warteschlange.

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

### <a name="speech-settings"></a>Einstellungen für die Spracherkennung

Zur besseren Steuerung, wie das Audio gesprochen wird mit später wieder `SpeakSettings` , die ermöglicht das Festlegen von Volumes, Tonhöhe, und dem Gebietsschema.

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

Im folgenden sind die unterstützten Werte für diese Parameter:

| Parameter | Minimum | Maximum |
| --- | :---: | :---: |
| Schriftbreite | 0 | 2.0 |
| Volume | 0 | 1.0 |

### <a name="speech-locales"></a>Spracherkennung Gebietsschemas

Jede Plattform bietet Gebietsschemas um Back Text in mehreren Sprachen und Akzente gesprochen. Jede Plattform gibt es verschiedene Codes und Methoden zum angeben, weshalb Essentials bietet eine plattformübergreifende `Locale` -Klasse und eine Möglichkeit zum Abfragen mit `GetLocalesAsync`.

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

- Äußerung Warteschlange ist nicht garantiert werden, wenn Sie über mehrere Threads aufgerufen wird.
- Hintergrund-Audiowiedergabe wird offiziell nicht unterstützt.

## <a name="api"></a>API

- [TextToSpeech-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/TextToSpeech)
- [TextToSpeech-API-Dokumentation](xref:Xamarin.Essentials.TextToSpeech)
