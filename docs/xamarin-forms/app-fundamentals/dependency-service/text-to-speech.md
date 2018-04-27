---
title: Implementieren der Sprachausgabe
description: Verwenden Sie zum Aufrufen von systemeigenen-Sprach-API für jede Plattform DependencyService
ms.prod: xamarin
ms.assetid: 1D6164F9-4ECE-43A6-B583-1F5D5EFC1DDF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/18/2017
ms.openlocfilehash: c9cf700ea798ac316e806c40cb90eedc7ded9fa5
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2018
---
# <a name="implementing-text-to-speech"></a>Implementieren der Sprachausgabe

Dieser Artikel führt Sie wie eine plattformübergreifende-app zu erstellen, die verwendet [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) systemeigene-Sprach-APIs den Zugriff auf:

- **[Zum Erstellen der Schnittstelle](#Creating_the_Interface)**  &ndash; zu verstehen, wie die Schnittstelle im gemeinsamen Code erstellt wird.
- **[iOS Implementierung](#iOS_Implementation)**  &ndash; erfahren Sie, wie die Schnittstelle in systemeigenem Code für iOS zu implementieren.
- **[Android-Implementierung](#Android_Implementation)**  &ndash; erfahren Sie, wie die Schnittstelle für Android in systemeigenem Code zu implementieren.
- **[Uwp-Implementierung](#WindowsImplementation)**  &ndash; erfahren Sie, wie die Schnittstelle in systemeigenem Code für die universelle Windows-Plattform (UWP) zu implementieren.
- **[Implementieren im freigegebenen Code](#Implementing_in_Shared_Code)**  &ndash; erfahren, wie `DependencyService` in die systemeigene Implementierung von freigegebenem Code aufrufen.

Die Anwendung mit `DependencyService` hat die folgende Struktur:

![](text-to-speech-images/tts-diagram.png "DependencyService Anwendungsstruktur")

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Erstellen der Schnittstelle

Erstellen Sie zunächst eine Schnittstelle im freigegebenen Code, der die Funktionalität ausdrückt, die Sie implementieren möchten. In diesem Beispiel die Schnittstelle enthält eine einzelne Methode `Speak`:

```csharp
public interface ITextToSpeech
{
    void Speak (string text);
}
```

Mit der Programmierung für diese Schnittstelle im freigegebenen Code können Xamarin.Forms-app auf die Spracherkennung-APIs auf jeder Plattform zugreifen.

> [!NOTE]
> Klassen, die die Schnittstelle implementieren, benötigen einen parameterlosen Konstruktor zur Bearbeitung der `DependencyService`.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS-Implementierung

In jedem Anwendungsprojekt plattformspezifischen muss die Schnittstelle implementiert werden. Beachten Sie, dass die Klasse über einen parameterlosen Konstruktor verfügt, damit die `DependencyService` können neue Instanzen erstellen.

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

Die `[assembly]` Attribut registriert die Klasse als eine Implementierung von der `ITextToSpeech` -Schnittstelle, dies bedeutet, dass `DependencyService.Get<ITextToSpeech>()` im freigegebenen Code zum Erstellen einer Instanz des Zertifikats verwendet werden können.

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android-Implementierung

Android-Code ist komplizierter als die iOS-Version: dafür, dass die implementierende Klasse zu Vererben von Android-spezifische `Java.Lang.Object` und zum Implementieren der `IOnInitListener` Schnittstelle auch. Darüber hinaus müssen Zugriff auf den aktuellen Android-Kontext, die durch verfügbar gemacht wird die `MainActivity.Instance` Eigenschaft.

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

Die `[assembly]` Attribut registriert die Klasse als eine Implementierung von der `ITextToSpeech` -Schnittstelle, dies bedeutet, dass `DependencyService.Get<ITextToSpeech>()` im freigegebenen Code zum Erstellen einer Instanz des Zertifikats verwendet werden können.

<a name="WindowsImplementation" />

## <a name="universal-windows-platform-implementation"></a>Universelle Windows-Plattform-Implementierung

Die universelle Windows-Plattform verfügt über eine Sprach-API in der `Windows.Media.SpeechSynthesis` Namespace. Der einzige Nachteil ist, denken Sie daran, Teilstrichen der **Mikrofon** Funktion in das Manifest anderweitig darauf zuzugreifen, um die Spracherkennung APIs werden blockiert.

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

Die `[assembly]` Attribut registriert die Klasse als eine Implementierung von der `ITextToSpeech` -Schnittstelle, dies bedeutet, dass `DependencyService.Get<ITextToSpeech>()` im freigegebenen Code zum Erstellen einer Instanz des Zertifikats verwendet werden können.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Implementieren im freigegebenen Code

Jetzt können wir schreiben und Testen von freigegebenen Code, der die Sprachausgabe-Schnittstelle zugreift. Diese einfache Seite enthält eine Schaltfläche, die die Sprachfunktion auslöst. Er verwendet die `DependencyService` zum Abrufen einer Instanz von der `ITextToSpeech` Schnittstelle &ndash; zur Laufzeit werden diese Instanz die Clientplattform-spezifische Implementierung, die uneingeschränkten Zugriff auf die systemeigene SDK verfügt.

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

Diese Anwendung auf iOS, Android oder UWP ausgeführt, und drücken die Schaltfläche führt in der Anwendung, sprechen Sie mit der systemeigenen Sprache SDK auf jeder Plattform.

 ![iOS und Android-Sprach-Schaltfläche](text-to-speech-images/running.png "Text-zu-Sprache-Beispiel")


## <a name="related-links"></a>Verwandte Links

- [Verwenden DependencyService (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample/)
- [Text-zu-Sprache-Arbeitsmappe](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/text-to-speech/text-to-speech.workbook)
