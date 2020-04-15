---
title: Android Speech
description: In diesem Artikel werden die Grundlagen der Verwendung des sehr leistungsfähigen Android.Speech-Namespace behandelt. Seit seiner Einführung war Android in der Lage, Sprache zu erkennen und als Text auszugeben. Dies ist ein relativ einfacher Prozess. Für Sprachsynthese (Text-zu-Sprache) ist der Prozess jedoch aufwändiger, da nicht nur die Sprach-Engine, sondern auch die Sprachen berücksichtigt werden müssen, die für das TTS-System (Text To Speech, Text-zu-Sprache) installiert und verfügbar sind.
ms.prod: xamarin
ms.assetid: FA3B8EC4-34D2-47E3-ACEA-BD34B28115B9
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/02/2018
ms.openlocfilehash: e8c7d1a4fb3537644ed3b7737158a5e50abcdae5
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "73019769"
---
# <a name="android-speech"></a>Android Speech

_In diesem Artikel werden die Grundlagen der Verwendung des sehr leistungsfähigen Android.Speech-Namespace behandelt. Seit seiner Einführung war Android in der Lage, Sprache zu erkennen und als Text auszugeben. Dies ist ein relativ einfacher Prozess. Für Sprachsynthese (Text-zu-Sprache) ist der Prozess jedoch aufwändiger, da nicht nur die Sprach-Engine, sondern auch die Sprachen berücksichtigt werden müssen, die für das TTS-System (Text To Speech, Text-zu-Sprache) installiert und verfügbar sind._

## <a name="speech-overview"></a>Übersicht zu Speech

Der Wunsch nach einem System, das die menschliche Sprache „versteht“ und ausspricht, was getippt wurde – Spracherkennung Sprachsynthese – ist ein ständig wachsender Bereich innerhalb der mobilen Entwicklung, da die Nachfrage nach natürlicher Kommunikation mit unseren Geräten steigt. Es gibt viele Fälle, in denen eine Funktion, die Text in Sprache oder umgekehrt umwandelt, ein sehr nützliches Werkzeug ist, um es in Ihre Android-Anwendung zu integrieren.

Wegen der beschränkten Nutzung von Mobiltelefonen während der Fahrt möchten Benutzer ihre Geräte beispielsweise ohne Einsatz der Hände bedienen können. Die Vielzahl verschiedener Android-Formfaktoren – etwa Android Wear – und die sich immer weiter ausdehnende Gruppe von Personen, die Android-Geräte (etwa Tablets und Notepads) verwenden können, hat zu einem größeren Fokus auf großartige TTS-Anwendungen geführt.

Google stellt Entwicklern eine nützliche Menge von APIs im Android.Speech-Namespace zur Verfügung, um die meisten Fälle abzudecken, in denen ein Gerät „sprachbewusst“ gemacht werden soll (z. B. Software, die für Blinde konzipiert ist).  Der Namespace bietet die Möglichkeit, Text über `Android.Speech.Tts` in Sprache zu übersetzen und die Engine zu steuern, die zum Ausführen der Übersetzung verwendet wird, und umfasst eine Reihe von `RecognizerIntent`s, die das Umwandeln von Sprache in Text ermöglichen.

Obwohl die Funktionen für Spracherkennung vorhanden sind, gibt es Beschränkungen, die sich aus der verwendeten Hardware ergeben. Es ist unwahrscheinlich, dass das Gerät in der Lage ist, alle in es gesprochene Wörter in jeder verfügbaren Sprache erfolgreich zu interpretieren.

## <a name="requirements"></a>Anforderungen

Es gibt keine besonderen Anforderungen für diesen Leitfaden. Es ist lediglich erforderlich, dass Ihr Gerät ein Mikrofon und einen Lautsprecher hat.

Der Kern für ein Android-Gerät, das Sprache interpretiert, ist die Verwendung eines `Intent`-Objekts (Absicht) mit einem entsprechenden `OnActivityResult`.
Es ist jedoch wichtig, sich klarzumachen, dass die Sprache nicht verstanden, sondern als Text interpretiert wird. Dieser Unterschied ist wichtig.

### <a name="the-difference-between-understanding-and-interpreting"></a>Der Unterschied zwischen Verstehen und Interpretieren

Eine einfache Definition für Verstehen ist, dass Sie in der Lage sind, anhand von Tonfall und Kontext die wirkliche Bedeutung des Gesagten zu erfassen. Interpretieren bedeutet nur, dass die Wörter übernommen und in einer anderen Form ausgegeben werden.

Betrachten Sie das folgende einfache Beispiel, das in einer alltäglichen Unterhaltung vorkommt:

<kbd>Hallo, wie geht es dir?</kbd>

Ohne Tonfall (Betonung, die auf bestimmte Wörter oder Teile von Wörtern gelegt wird) ist dies eine einfache Frage. Wenn die Zeile jedoch langsam gesprochen wird, erkennt die zuhörende Person, dass die fragende Person nicht besonders zufrieden ist und Aufmunterung benötigt oder dass sie sich unwohl fühlt. Wenn die Betonung auf „Wie“ gelegt wurde, ist die fragende Person in der Regel eher an der Antwort interessiert.

Ohne eine ziemlich leistungsfähige Audioverarbeitung, um den Tonfall zu nutzen, und ohne ein bestimmtes Maß an künstlicher Intelligenz (KI), um den Kontext zu verstehen, kann die Software nicht einmal ansatzweise verstehen, was gesagt wurde – das Beste, was ein einfaches Telefon erledigen kann, ist die Umwandlung der Sprache in Text.

## <a name="setting-up"></a>Einrichtung

Vor der Nutzung des Sprachsystems ist es immer ratsam, sich zu vergewissern, dass das Gerät ein Mikrofon hat. Es würde wenig Sinn machen, Ihre App auf einem Kindle- oder Google-E-Book-Reader ohne eingebautes Mikrofon auszuführen.

Im folgenden Codebeispiel ist die Abfrage veranschaulicht, ob ein Mikrofon verfügbar ist, und falls nicht, wie eine Warnung erstellt wird. Ist an diesem Punkt kein Mikrofon verfügbar, würden Sie so vorgehen, dass entweder die Aktivität beendet oder die Möglichkeit, Sprache aufzuzeichnen, deaktiviert wird.

```csharp
string rec = Android.Content.PM.PackageManager.FeatureMicrophone;
if (rec != "android.hardware.microphone")
{
    var alert = new AlertDialog.Builder(recButton.Context);
    alert.SetTitle("You don't seem to have a microphone to record with");
    alert.SetPositiveButton("OK", (sender, e) =>
    {
        return;
    });
    alert.Show();
}
```

### <a name="creating-the-intent"></a>Erstellen des Intent-Objekts (Absicht)

In der Absicht für das Sprachsystem wird eine bestimmte Art von Absicht verwendet, die als `RecognizerIntent` bezeichnet wird. Diese Absicht steuert eine große Anzahl von Parametern, darunter die Dauer, wie lange bei Schweigen gewartet werden soll, bis die Aufnahme als beendet betrachtet wird, alle weiteren Sprachen, die erkannt und ausgegeben werden sollen, und jeglicher Text, der als Anweisung in den modalen Dialog von `Intent` aufgenommen werden soll. In diesem Codeausschnitt ist `VOICE` eine schreibgeschützte Absicht (`readonly int`), die für die Erkennung in `OnActivityResult` verwendet wird.

```csharp
var voiceIntent = new Intent(RecognizerIntent.ActionRecognizeSpeech);
voiceIntent.PutExtra(RecognizerIntent.ExtraLanguageModel, RecognizerIntent.LanguageModelFreeForm);
voiceIntent.PutExtra(RecognizerIntent.ExtraPrompt, Application.Context.GetString(Resource.String.messageSpeakNow));
voiceIntent.PutExtra(RecognizerIntent.ExtraSpeechInputCompleteSilenceLengthMillis, 1500);
voiceIntent.PutExtra(RecognizerIntent.ExtraSpeechInputPossiblyCompleteSilenceLengthMillis, 1500);
voiceIntent.PutExtra(RecognizerIntent.ExtraSpeechInputMinimumLengthMillis, 15000);
voiceIntent.PutExtra(RecognizerIntent.ExtraMaxResults, 1);
voiceIntent.PutExtra(RecognizerIntent.ExtraLanguage, Java.Util.Locale.Default);
StartActivityForResult(voiceIntent, VOICE);
```

### <a name="conversion-of-the-speech"></a>Konvertieren der Sprache

Der Text, der aus der Sprache interpretiert wird, wird im `Intent`-Objekt übermittelt, das zurückgegeben wird, wenn die Aktivität abgeschlossen wurde und über `GetStringArrayListExtra(RecognizerIntent.ExtraResults)` auf sie zugegriffen wird. Dadurch wird eine `IList<string>`-Zeichenfolge zurückgegeben, von der der Index verwendet und angezeigt werden kann, je nach der Anzahl der Sprachen, die in der Absicht des Aufrufers angefordert wurden (und für `RecognizerIntent.ExtraMaxResults` angegeben sind). Wie bei jeder Liste lohnt es sich jedoch, zu prüfen, ob Daten vorhanden sind, die angezeigt werden sollen.

Wird auf den Rückgabewert einer `StartActivityForResult`-Methode gelauscht, muss die `OnActivityResult`-Methode bereitgestellt werden.

Im folgenden Beispiel ist `textBox` ein `TextBox`-Objekt, das zur Ausgabe des Texts verwendet wird, der diktiert wurde. Es könnte ebenso verwendet werden, den Text an eine Form von Interpreter zu übergeben, und von dort aus kann die Anwendung den Text vergleichen und zu einem anderen Teil der Anwendung verzweigen.

```csharp
protected override void OnActivityResult(int requestCode, Result resultVal, Intent data)
{
    if (requestCode == VOICE)
    {
        if (resultVal == Result.Ok)
        {
            var matches = data.GetStringArrayListExtra(RecognizerIntent.ExtraResults);
            if (matches.Count != 0)
            {
                string textInput = textBox.Text + matches[0];
                textBox.Text = textInput;
                switch (matches[0].Substring(0, 5).ToLower())
                {
                    case "north":
                        MovePlayer(0);
                        break;
                    case "south":
                        MovePlayer(1);
                        break;
                }
            }
            else
            {
                textBox.Text = "No speech was recognised";
            }
        }
        base.OnActivityResult(requestCode, resultVal, data);
    }
}
```

## <a name="text-to-speech"></a>Text-zu-Sprache

Sprachsynthese (Text-zu-Sprache, Text to Speech) ist nicht ganz das Gegenteil von Spracherkennung und beruht auf zwei Schlüsselkomponenten: einer Sprachsynthese-Engine, die auf dem Gerät installiert wird, und einer Sprache, die installiert wird.

Auf den meisten Android-Geräten sind zunächst der standardmäßige Google-TTS-Dienst und mindestens eine Sprache installiert. Dies wird bei der Ersteinrichtung des Geräts eingerichtet und ergibt sich daraus, wo sich das Gerät zu diesem Zeitpunkt befindet (bei einem in Deutschland eingerichteten Smartphone wird z. B. Deutsch, bei einem Smartphone in Amerika wird dagegen amerikanisches Englisch installiert).

### <a name="step-1---instantiating-texttospeech"></a>Schritt 1: Instanziieren von „TextToSpeech“

`TextToSpeech` kann bis zu drei Parameter haben, wobei die ersten beiden erforderlich sind und der dritte optional ist (`AppContext`, `IOnInitListener`, `engine`). Der Listener wird dazu verwendet, die Bindung zum Dienst herzustellen und auf Fehler zu testen, wobei „engine“ eine beliebige Anzahl verfügbarer Android-Sprachsynthese-Engines sein kann. Auf dem Gerät ist mindestens die Google-eigene Engine vorhanden.

### <a name="step-2---finding-the-languages-available"></a>Schritt 2: Suchen nach den verfügbaren Sprachen

Die `Java.Util.Locale`-Klasse enthält die nützliche Methode `GetAvailableLocales()`. Diese Liste der Sprachen, die von der Sprach-Engine unterstützt werden, kann dann gegen die installierten Sprachen getestet werden.

Es ist eine triviale Angelegenheit, die Liste der „verstandenen“ Sprachen zu generieren. Es gibt immer eine Standardsprache (die Sprache, die der Benutzer beim ersten Einrichten seines Geräts festgelegt hat). In diesem Beispiel hat die `List<string>`-Zeichenfolge daher „Default“ als ersten Parameter, und der Rest der Liste wird abhängig vom Ergebnis der `textToSpeech.IsLanguageAvailable(locale)`-Methode ausgefüllt.

```csharp
var langAvailable = new List<string>{ "Default" };
var localesAvailable = Java.Util.Locale.GetAvailableLocales().ToList();
foreach (var locale in localesAvailable)
{
    var res = textToSpeech.IsLanguageAvailable(locale);
    switch (res)
    {
        case LanguageAvailableResult.Available:
          langAvailable.Add(locale.DisplayLanguage);
          break;
        case LanguageAvailableResult.CountryAvailable:
          langAvailable.Add(locale.DisplayLanguage);
          break;
        case LanguageAvailableResult.CountryVarAvailable:
          langAvailable.Add(locale.DisplayLanguage);
          break;
    }
}
langAvailable = langAvailable.OrderBy(t => t).Distinct().ToList();
```

In diesem Code wird [TextToSpeech.IsLanguageAvailable](xref:Android.Speech.Tts.TextToSpeech.IsLanguageAvailable*) aufgerufen, um zu testen, ob das Sprachpaket für ein bestimmtes Gebietsschema bereits auf dem Gerät vorhanden ist.
Diese Methode gibt eine [LanguageAvailableResult](xref:Android.Speech.Tts.LanguageAvailableResult)-Aufzählung zurück, die angibt, ob die Sprache für das übergebene Gebietsschema verfügbar ist. Wenn `LanguageAvailableResult` angibt, dass die Sprache nicht unterstützt wird (`NotSupported`), ist für diese Sprache kein Sprachpaket verfügbar (und kann auch keines heruntergeladen werden). Wenn `LanguageAvailableResult` auf `MissingData` festgelegt ist, kann ein neues Sprachpaket heruntergeladen werden, wie dies weiter unten in Schritt 4 erläutert ist.

### <a name="step-3---setting-the-speed-and-pitch"></a>Schritt 3: Festlegen der Geschwindigkeit und der Tonhöhe

Android ermöglicht es dem Benutzer, den Klang der Sprache zu ändern, indem er das Sprechtempo (`SpeechRate`) und die Tonhöhe (`Pitch`) der Sprache ändert. Dies erstreckt sich von „0“ bis „1“, wobei für „normale“ Sprache beide Parameter gleich „1“ sind.

### <a name="step-4---testing-and-loading-new-languages"></a>Schritt 4: Testen und Laden neuer Sprachen

Das Herunterladen einer neuen Sprache erfolgt über ein`Intent`-Objekt. Das Ergebnis dieses Intent-Objekts bewirkt, dass die [OnActivityResult](xref:Android.App.Activity.OnActivityResult*)-Methode aufgerufen wird. Im Gegensatz zum Spracherkennungsbeispiel (in dem das [RecognizerIntent](xref:Android.Speech.RecognizerIntent)-Objekt als `PutExtra`Parameter für das `Intent`-Objekt verwendet wird) sind die `Intent`-Objekte für Testen und Laden `Action`-basiert:

- [TextToSpeech.Engine.ActionCheckTtsData](xref:Android.Speech.Tts.TextToSpeech.Engine.ActionCheckTtsData) &ndash; Startet eine Aktivität von der `TextToSpeech`-Engine der Plattform, um auf ordnungsgemäße Installation und Verfügbarkeit von Sprachressourcen auf dem Gerät zu prüfen.

- [TextToSpeech.Engine.ActionInstallTtsData](xref:Android.Speech.Tts.TextToSpeech.Engine.ActionInstallTtsData) &ndash; Startet eine Aktivität, in der der Benutzer aufgefordert wird, die erforderlichen Sprachen herunterzuladen.

Im folgenden Codebeispiel wird veranschaulicht, wie diese Aktionen verwendet werden, um auf Sprachressourcen zu testen und eine neue Sprache herunterzuladen:

```csharp
var checkTTSIntent = new Intent();
checkTTSIntent.SetAction(TextToSpeech.Engine.ActionCheckTtsData);
StartActivityForResult(checkTTSIntent, NeedLang);
//
protected override void OnActivityResult(int req, Result res, Intent data)
{
    if (req == NeedLang)
    {
        var installTTS = new Intent();
        installTTS.SetAction(TextToSpeech.Engine.ActionInstallTtsData);
        StartActivity(installTTS);
    }
}
```

`TextToSpeech.Engine.ActionCheckTtsData` testet auf die Verfügbarkeit von Sprachressourcen. `OnActivityResult` wird aufgerufen, wenn dieser Test abgeschlossen ist. Wenn Sprachressourcen heruntergeladen werden müssen, löst `OnActivityResult` die `TextToSpeech.Engine.ActionInstallTtsData`-Aktion aus, um eine Aktivität zu starten, in der der Benutzer die erforderlichen Sprachen herunterladen kann. Beachten Sie, dass in dieser `OnActivityResult`-Implementierung der `Result`-Code nicht überprüft wird, da in diesem vereinfachten Beispiel bereits festgestellt wurde, dass das Sprachpaket heruntergeladen werden muss.

In der `TextToSpeech.Engine.ActionInstallTtsData`-Aktion wird veranlasst, dass die **Google TTS-Sprachdaten**-Aktivität angezeigt wird, sodass der Benutzer die herunterzuladenden Sprachen auswählen kann:

![Google TTS-Sprachdaten-Aktivität](speech-images/01-google-tts-voice-data.png)

Als Beispiel kann der Benutzer „Französisch“ auswählen und auf das Downloadsymbol tippen, um die französischen Sprachdaten herunterzuladen:

![Beispiel für das Herunterladen der Sprache Französisch](speech-images/02-selecting-french.png)

Die Installation dieser Daten erfolgt automatisch, nachdem der Download abgeschlossen wurde.

### <a name="step-5---the-ioninitlistener"></a>Schritt 5: der „IOnInitListener“

Damit eine Aktivität den Text in Sprache konvertieren kann, muss die Schnittstellenmethode `OnInit` implementiert werden (dies ist der zweite Parameter, der für die Instanziierung der `TextToSpeech`-Klasse angegeben ist). Dadurch wird der Listener initialisiert und das Ergebnis getestet.

Im Listener sollte mindestens auf `OperationResult.Success` und `OperationResult.Failure` getestet werden.
Das folgende Beispiel zeigt genau das:

```csharp
void TextToSpeech.IOnInitListener.OnInit(OperationResult status)
{
    // if we get an error, default to the default language
    if (status == OperationResult.Error)
        textToSpeech.SetLanguage(Java.Util.Locale.Default);
    // if the listener is ok, set the lang
    if (status == OperationResult.Success)
        textToSpeech.SetLanguage(lang);
}
```

## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden ging es um die Grundlagen der Konvertierung von Text in Sprache und Sprache in Text sowie um mögliche Methoden, wie diese Konvertierungen in eigene Apps eingebunden werden können. Obwohl nicht jeder spezielle Fall abgedeckt ist, sollten Sie nun ein grundlegendes Verständnis dazu haben, wie Sprache interpretiert wird, wie neue Sprachen installiert werden und wie die Inklusivität Ihrer Apps erhöht wird.

## <a name="related-links"></a>Verwandte Links

- [Xamarin.Forms-DependencyService](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/dependencyservice//)
- [Text to Speech (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-texttospeech)
- [Speech to Text (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-speechtotext)
- [Android.Speech-Namespace](xref:Android.Speech)
- [Android.Speech.Tts-Namespace](xref:Android.Speech.Tts)
