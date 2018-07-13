---
title: Implementieren von Sprachsynthese
description: In diesem Artikel wird erläutert, wie Sie die Xamarin.Forms DependencyService-Klasse verwenden, um die einzelnen Plattformen native-Sprach-API aufrufen.
ms.prod: xamarin
ms.assetid: 1D6164F9-4ECE-43A6-B583-1F5D5EFC1DDF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/18/2017
ms.openlocfilehash: d39902f2269d3eb241b48831b8eb1b181ff11e7a
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997542"
---
# <a name="implementing-text-to-speech"></a>Implementieren von Sprachsynthese

In diesem Artikel führen Sie darüber, wie Sie eine plattformübergreifende app erstellen, verwendet [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) den Zugriff auf native-Sprach-APIs:

- **[Zum Erstellen der Schnittstelle](#Creating_the_Interface)**  &ndash; zu verstehen, wie die Schnittstelle in freigegebenem Code erstellt wird.
- **[iOS-Implementierung](#iOS_Implementation)**  &ndash; erfahren Sie, wie die Schnittstelle in nativem Code für iOS zu implementieren.
- **[Android-Implementierung](#Android_Implementation)**  &ndash; erfahren Sie, wie die Schnittstelle für Android in nativem Code zu implementieren.
- **[UWP-Implementierung](#WindowsImplementation)**  &ndash; erfahren Sie, wie die Schnittstelle in nativem Code für die universelle Windows-Plattform (UWP) zu implementieren.
- **[In freigegebenem Code implementieren](#Implementing_in_Shared_Code)**  &ndash; erfahren Sie, wie Sie mit `DependencyService` , die native Implementierung von freigegebenem Code aufzurufen.

Die Anwendung mit `DependencyService` wird die folgende Struktur aufweisen:

![](text-to-speech-images/tts-diagram.png "DependencyService Anwendungsstruktur")

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Erstellen der Schnittstelle

Erstellen Sie zunächst eine Schnittstelle im freigegebenen Code, der die Funktionalität ausdrückt, die Sie implementieren möchten. In diesem Beispiel die Schnittstelle enthält eine einzelne Methode, `Speak`:

```csharp
public interface ITextToSpeech
{
    void Speak (string text);
}
```

Mit der Programmierung für diese Schnittstelle in den freigegebenen Code lässt sich auf die Xamarin.Forms-app auf die Sprach-APIs auf jeder Plattform aus.

> [!NOTE]
> Klassen, die die Schnittstelle implementieren, müssen einen parameterlosen Konstruktor für die Arbeit mit der `DependencyService`.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS-Implementierung

Die Schnittstelle muss in jedem Anwendungsprojekt plattformspezifischen-implementiert werden. Beachten Sie, dass die Klasse über einen parameterlosen Konstruktor hat, damit die `DependencyService` neue Instanzen erstellen können.

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

Die `[assembly]` Attribut registriert die Klasse als eine Implementierung der `ITextToSpeech` -Schnittstelle, die das bedeutet, dass `DependencyService.Get<ITextToSpeech>()` kann in den freigegebenen Code verwendet werden, um eine Instanz davon erstellen.

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android-Implementierung

Die Android-Code ist komplexer als die iOS-Version: Es erfordert, dass die implementierende Klasse das erben von Android-spezifische `Java.Lang.Object` und zum Implementieren der `IOnInitListener` Schnittstelle auch. Darüber hinaus den Zugriff auf die aktuelle Android-Kontext, der verfügbar gemacht werden müssen die `MainActivity.Instance` Eigenschaft.

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

Die `[assembly]` Attribut registriert die Klasse als eine Implementierung der `ITextToSpeech` -Schnittstelle, die das bedeutet, dass `DependencyService.Get<ITextToSpeech>()` kann in den freigegebenen Code verwendet werden, um eine Instanz davon erstellen.

<a name="WindowsImplementation" />

## <a name="universal-windows-platform-implementation"></a>Implementierung der universellen Windows-Plattform

Die universelle Windows-Plattform verfügt über eine Spracheingabe-API in der `Windows.Media.SpeechSynthesis` Namespace. Der einzige Nachteil ist, daran denken müssen, aktivieren Sie die **Mikrofon** -Funktion in das Manifest andernfalls zugreifen, auf die Sprach-APIs werden blockiert.

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

Die `[assembly]` Attribut registriert die Klasse als eine Implementierung der `ITextToSpeech` -Schnittstelle, die das bedeutet, dass `DependencyService.Get<ITextToSpeech>()` kann in den freigegebenen Code verwendet werden, um eine Instanz davon erstellen.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>In freigegebenem Code implementieren

Jetzt können wir schreiben und Testen freigegebenen Code, der die Text-Sprach-Schnittstelle zugreift. Diese einfache Seite enthält eine Schaltfläche, die die Funktion "Sprache" auslöst. Er verwendet die `DependencyService` zum Abrufen einer Instanz von der `ITextToSpeech` Schnittstelle &ndash; zur Laufzeit werden diese Instanz die plattformspezifischen Implementierungen, die uneingeschränkten Zugriff auf das systemeigene SDK verfügt.

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

Ausführen der Anwendung auf iOS-, Android- oder UWP und auf die Schaltfläche führt in der Anwendung, die Sie mithilfe des systemeigenen Sprach-SDKS auf jeder Plattform sprechen.

 ![iOS und Android-Sprach-Schaltfläche](text-to-speech-images/running.png "Sprachsynthese-Beispiel")


## <a name="related-links"></a>Verwandte Links

- [Verwenden von DependencyService (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample/)
- [Text-zu-Sprache-Arbeitsmappe](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/text-to-speech/text-to-speech.workbook)
