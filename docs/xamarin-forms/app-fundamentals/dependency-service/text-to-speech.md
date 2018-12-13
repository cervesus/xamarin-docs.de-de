---
title: Implementieren von Text-zu-Sprache
description: In diesem Artikel wird erläutert, wie Sie mit der DependencyService-Klasse von Xamarin.Forms die native Text-zu-Sprache-API jeder Plattform aufrufen.
ms.prod: xamarin
ms.assetid: 1D6164F9-4ECE-43A6-B583-1F5D5EFC1DDF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/18/2017
ms.openlocfilehash: 6d1948214b97a1b536b07b6420c32e4d27124518
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2018
ms.locfileid: "38997542"
---
# <a name="implementing-text-to-speech"></a>Implementieren von Text-zu-Sprache

Dieser Artikel führt Sie durch das Erstellen einer plattformübergreifenden App, die mit [`DependencyService`](xref:Xamarin.Forms.DependencyService) auf native Text-zu-Sprache-APIs zugreift:

- **[Erstellen der Schnittstelle](#Creating_the_Interface)** &ndash; Erfahren Sie, wie die Schnittstelle in freigegebenem Code erstellt wird.
- **[iOS-Implementierung:](#iOS_Implementation)** Erfahren Sie, wie die Schnittstelle in nativem Code für iOS implementiert wird.
- **[Android-Implementierung:](#Android_Implementation)** Erfahren Sie, wie die Schnittstelle in nativem Code für Android implementiert wird.
- **[UWP-Implementierung:](#WindowsImplementation)** Erfahren Sie, wie die Schnittstelle in nativem Code für die Universelle Windows-Plattform implementiert wird.
- **[Implementieren in freigegebenem Code:](#Implementing_in_Shared_Code)** Erfahren Sie, wie Sie mit `DependencyService` die native Implementierung von freigegebenem Code aufrufen.

Die App, die `DependencyService` verwendet, hat folgende Struktur:

![](text-to-speech-images/tts-diagram.png "Anwendungsstruktur von DependencyService")

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Erstellen der Schnittstelle

Erstellen Sie als Erstes eine Schnittstelle im freigegebenen Code, die die Funktion ausdrückt, die Sie implementieren möchten. In diesem Beispiel enthält die Schnittstelle die einzelne Methode `Speak`:

```csharp
public interface ITextToSpeech
{
    void Speak (string text);
}
```

Die Kodierung mit dieser Schnittstelle im freigegebenen Code ermöglicht der Xamarin.Forms-App den Zugriff auf die Sprach-APIs auf jeder Plattform.

> [!NOTE]
> Klassen, die die Schnittstelle implementieren, müssen einen parameterlosen Konstruktor aufweisen, damit sie mit `DependencyService` funktionieren können.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS-Implementierung

Die Schnittstelle muss in jedem plattformspezifischen App-Projekt implementiert werden. Beachten Sie, dass die Klasse einen parameterlosen Konstruktor aufweist, sodass `DependencyService` neue Instanzen erstellen kann.

```csharp
[assembly: Dependency(typeof(TextToSpeechImplementation))]
namespace DependencyServiceSample.iOS
{

    public class TextToSpeechImplementation : ITextToSpeech
    {
        public TextToSpeechImplementation() { }

        public void Speak(string text)
        {
            var speechSynthesizer = new AVSpeechSynthesizer();
            var speechUtterance = new AVSpeechUtterance(text)
            {
                Rate = AVSpeechUtterance.MaximumSpeechRate / 4,
                Voice = AVSpeechSynthesisVoice.FromLanguage("en-US"),
                Volume = 0.5f,
                PitchMultiplier = 1.0f
            };

            speechSynthesizer.SpeakUtterance(speechUtterance);
        }
    }
}
```

Das Attribut `[assembly]` registriert die Klasse als eine Implementierung der Schnittstelle `ITextToSpeech`, d.h., dass `DependencyService.Get<ITextToSpeech>()` im freigegebenen Code verwendet werden kann, um eine Instanz davon zu erstellen.

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android-Implementierung

Der Android-Code ist komplexer als die iOS-Version: Er erfordert, dass die implementierende Klasse vom Android-spezifischen `Java.Lang.Object` erbt und zudem die Schnittstelle `IOnInitListener` implementiert. Außerdem benötigt sie Zugriff auf den aktuellen Android-Kontext, der durch die Eigenschaft `MainActivity.Instance` verfügbar gemacht wird.

```csharp
[assembly: Dependency(typeof(TextToSpeechImplementation))]
namespace DependencyServiceSample.Droid
{
    public class TextToSpeechImplementation : Java.Lang.Object, ITextToSpeech, TextToSpeech.IOnInitListener
    {
        TextToSpeech speaker;
        string toSpeak;

        public void Speak(string text)
        {
            toSpeak = text;
            if (speaker == null)
            {
                speaker = new TextToSpeech(MainActivity.Instance, this);
            }
            else
            {
                speaker.Speak(toSpeak, QueueMode.Flush, null, null);
            }
        }

        public void OnInit(OperationResult status)
        {
            if (status.Equals(OperationResult.Success))
            {
                speaker.Speak(toSpeak, QueueMode.Flush, null, null);
            }
        }
    }
}
```

Das Attribut `[assembly]` registriert die Klasse als eine Implementierung der Schnittstelle `ITextToSpeech`, d.h., dass `DependencyService.Get<ITextToSpeech>()` im freigegebenen Code verwendet werden kann, um eine Instanz davon zu erstellen.

<a name="WindowsImplementation" />

## <a name="universal-windows-platform-implementation"></a>UWP-Implementierung

Die Universelle Windows-Plattform verfügt über eine Sprach-API im Namespace `Windows.Media.SpeechSynthesis`. Der einzige Nachteil ist, dass Sie daran denken müssen, die Funktion **Mikrofon** im Manifest zu aktivieren. Andernfalls wird der Zugriff auf die Sprach-APIs blockiert.

```csharp
[assembly:Dependency(typeof(TextToSpeechImplementation))]
public class TextToSpeechImplementation : ITextToSpeech
{
    public async void Speak(string text)
    {
        var mediaElement = new MediaElement();
        var synth = new Windows.Media.SpeechSynthesis.SpeechSynthesizer();
        var stream = await synth.SynthesizeTextToStreamAsync(text);

        mediaElement.SetSource(stream, stream.ContentType);
        mediaElement.Play();
    }
}
```

Das Attribut `[assembly]` registriert die Klasse als eine Implementierung der Schnittstelle `ITextToSpeech`, d.h., dass `DependencyService.Get<ITextToSpeech>()` im freigegebenen Code verwendet werden kann, um eine Instanz davon zu erstellen.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Implementieren in freigegebenem Code

Nun können wir freigegebenen Code schreiben und testen, der auf die Text-zu-Sprache-Schnittstelle zugreift. Diese einfache Seite enthält eine Schaltfläche, die die Sprachfunktion auslöst. Sie verwendet `DependencyService`, um eine Instanz der Schnittstelle `ITextToSpeech` zu erhalten. In der Common Language Runtime ist diese Instanz die plattformspezifische Implementierung, die über Vollzugriff auf das native SDK verfügt.

```csharp
public MainPage ()
{
    var speak = new Button {
        Text = "Hello, Forms !",
        VerticalOptions = LayoutOptions.CenterAndExpand,
        HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
    speak.Clicked += (sender, e) => {
        DependencyService.Get<ITextToSpeech>().Speak("Hello from Xamarin Forms");
    };
    Content = speak;
}
```

Wenn Sie diese Anwendung unter iOS, Android oder auf der UWP ausführen und die Schaltfläche drücken, wird die Anwendung mit Ihnen sprechen und dabei das native Sprach-SDK auf jeder Plattform verwenden.

 ![Text-zu-Sprache-Schaltfläche unter iOS und Android](text-to-speech-images/running.png "Beispiel für Text-zu-Sprache")


## <a name="related-links"></a>Verwandte Links

- [Verwenden von DependencyService (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample/)

