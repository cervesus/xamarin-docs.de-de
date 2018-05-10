---
title: Xamarin.Essentials Sprachausgabe
description: Die Klasse TextToSpeech ermöglicht eine Anwendung in Sprach sprechen Back Text vom Gerät und verfügbaren Abfragesprachen, die das Modul unterstützen, kann die integrierten verwenden.
ms.assetid: AEEF03AE-A047-4DF0-B0E8-CC8D9A7B8351
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 2fcb97eafda15a73e2b7e5441f1d6c94f75af0ce
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-text-to-speech"></a>Xamarin.Essentials Sprachausgabe

![Vorabversion NuGet](~/media/shared/pre-release.png)

Die **TextToSpeech** Klasse ermöglicht einer Anwendung in Sprach sprechen Back Text vom Gerät und verfügbaren Abfragesprachen, die das Modul unterstützen, kann die integrierten verwenden.

## <a name="using-text-to-speech"></a>Mithilfe der Sprachausgabe

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Die Sprachausgabe-Funktionalität kann durch Aufrufen der `SpeakAsync` Methode mit Text und optionale Parameter und gibt nach Abschluss der Aussprache. 

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

Diese Methode akzeptiert einen optionalen CancellationToken die Aussprache einer beenden, starten. 
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

Sprachausgabe wird automatisch Speech-Anforderungen aus demselben Thread eine Warteschlange gestellt. 

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

### <a name="speech-settings"></a>Speech-Einstellungen

Sichern Sie zur besseren Steuerung, wie das Audio gesprochen wird mit `SpeakSettings` ermöglicht das Festlegen der Lautstärke, Tonhöhe und Gebietsschema.

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
| Tonhöhe | 0 | 2.0 |
| Volume | 0 | 1.0 |

### <a name="speech-locales"></a>Spracherkennung Gebietsschemas

Jede Plattform bietet Gebietsschemas um Back Text in mehreren Sprachen und Akzente gesprochen werden soll. Jede Plattform weist einen anderen Codes und Methoden zum angeben, weshalb Essentials bietet eine plattformübergreifende `Locale` Klasse und eine Methode für Abfragen darauf mit `GetLocalesAsync`.

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

- Aussprache Warteschlange ist nicht gewährleistet, wenn Sie über mehrere Threads hinweg aufgerufen wird.
- Hintergrund Audiowiedergabe wird offiziell nicht unterstützt.

## <a name="api"></a>API

- [TextToSpeech-Quellcode](https://github.com/xamarin/Essentials/tree/master/Essentials/TextToSpeech)
- [TextToSpeech API-Dokumentation](xref:Xamarin.Essentials.TextToSpeech)
