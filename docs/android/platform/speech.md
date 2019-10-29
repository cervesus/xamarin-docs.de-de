---
title: Android-Sprache
description: In diesem Artikel werden die Grundlagen der Verwendung des sehr leistungsfähigen Android. Speech-Namespace behandelt. Seit seiner Einführung ist Android in der Lage, die Sprache zu erkennen und als Text auszugeben. Es handelt sich um einen relativ einfachen Vorgang. Für Text-zu-Sprache ist der Prozess jedoch eher beteiligt, da nicht nur die Sprach-Engine berücksichtigt werden muss, sondern auch die Sprachen, die verfügbar sind und aus dem "Text-to-Speech"-System (TTS) installiert werden.
ms.prod: xamarin
ms.assetid: FA3B8EC4-34D2-47E3-ACEA-BD34B28115B9
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/02/2018
ms.openlocfilehash: e8c7d1a4fb3537644ed3b7737158a5e50abcdae5
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73019769"
---
# <a name="android-speech"></a>Android-Sprache

_In diesem Artikel werden die Grundlagen der Verwendung des sehr leistungsfähigen Android. Speech-Namespace behandelt. Seit seiner Einführung ist Android in der Lage, die Sprache zu erkennen und als Text auszugeben. Es handelt sich um einen relativ einfachen Vorgang. Für Text-zu-Sprache ist der Prozess jedoch eher beteiligt, da nicht nur die Sprach-Engine berücksichtigt werden muss, sondern auch die Sprachen, die verfügbar sind und aus dem "Text-to-Speech"-System (TTS) installiert werden._

## <a name="speech-overview"></a>Sprach Übersicht

Ein System, das die Menschen Sprache versteht und eingibt, was typisiert wird – Sprache und Text-to-Speech – ist ein ständig wachsender Bereich in der mobilen Entwicklung, da die Nachfrage nach natürlicher Kommunikation mit unseren Geräten steigt. Es gibt viele Instanzen, bei denen eine Funktion, mit der Text in Sprache konvertiert wird, oder umgekehrt, ein sehr nützliches Tool ist, das in Ihre Android-Anwendung integriert werden kann.

Beispielsweise können Benutzer mit der unter Verwendung der Mobil Telefonverwendung während des Fahrens eine Hand freie Methode zum Betrieb Ihrer Geräte wünschen. Die Vielzahl von verschiedenen Android-Formfaktoren – wie z. b. Android Wear – und die immer erweiternde Einbindung von Geräten, die Android-Geräte (z. b. Tablets und Notizzettel) verwenden können, haben einen größeren Fokus auf großartige TTS-Anwendungen geschaffen.

Google stellt dem Entwickler einen umfangreichen Satz von APIs im Android. Speech-Namespace zur Verfügung, um die meisten Instanzen der Spracherkennung durch das Gerät (z. b. Software, die für Blind konzipiert ist) abzudecken.  Der-Namespace enthält die Möglichkeit, Text über `Android.Speech.Tts`in Sprache zu übersetzen, die Kontrolle über die Engine, die zum Durchführen der Übersetzung verwendet wird, sowie eine Reihe von `RecognizerIntent`en, die die Umwandlung von Sprache in Text ermöglichen.

Obwohl die Funktionen für die Spracherkennung verfügbar sind, gibt es Beschränkungen, die auf der verwendeten Hardware beruhen. Es ist unwahrscheinlich, dass das Gerät alle darin gesprochenen Elemente in jeder verfügbaren Sprache erfolgreich interpretiert.

## <a name="requirements"></a>Anforderungen

Es gibt keine besonderen Anforderungen für dieses Handbuch, außer wenn Ihr Gerät über ein Mikrofon und einen Redner verfügt.

Der Kern einer sprach Interpretation von Android-Geräten ist die Verwendung einer `Intent` mit einer entsprechenden `OnActivityResult`.
Es ist jedoch wichtig zu erkennen, dass die Sprache nicht verstanden wird – aber als Text interpretiert wird. Der Unterschied ist wichtig.

### <a name="the-difference-between-understanding-and-interpreting"></a>Der Unterschied zwischen verstehen und interpretieren

Eine einfache Definition des Verständnisses besteht darin, dass Sie nach Ton und Kontext ermitteln können, welche Bedeutung der Bedeutung hat. Die Interpretation der Wörter und die Ausgabe in einer anderen Form.

Sehen Sie sich das folgende einfache Beispiel an, das in täglicher Konversation verwendet wird:

<kbd>Guten Tag, wie geht es dir?</kbd>

Ohne Wende (Betonung auf bestimmte Wörter oder Teile von Wörtern) ist es eine einfache Frage. Wenn jedoch eine langsame Geschwindigkeit auf die Linie angewendet wird, erkennt die Person, die lauscht, dass der Fragende nicht zu zufrieden ist und möglicherweise Aufrufer oder dass der Fragende nicht gut ist. Wenn der Schwerpunkt auf "are" gesetzt wird, ist die Person, die in Frage kommt, in der Regel eher interessiert an der Antwort.

Ohne eine relativ leistungsstarke Audioverarbeitung, um die Anwendung und einen gewissen Grad an künstlicher Intelligenz (KI) zum Verständnis des Kontexts zu verwenden, kann die Software nicht einmal anfangen, zu verstehen, was gesagt wurde – das beste ist ein einfaches Telefon, das die Sprache in Text konvertieren kann.

## <a name="setting-up"></a>Einrichtung

Vor der Verwendung des Sprachsystems ist es immer ratsam, sicherzustellen, dass das Gerät über ein Mikrofon verfügt. Es wäre kaum sinnvoll, die APP auf einem Kindle-oder Google-Notizzettel auszuführen, ohne dass ein Mikrofon installiert ist.

Das folgende Codebeispiel veranschaulicht die Abfrage, ob ein Mikrofon verfügbar ist, und falls nicht, um eine Warnung zu erstellen. Wenn an dieser Stelle kein Mikrofon verfügbar ist, beenden Sie entweder die Aktivität, oder deaktivieren Sie die Möglichkeit, die Sprache aufzuzeichnen.

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

### <a name="creating-the-intent"></a>Erstellen der Absicht

Die Absicht für das Sprachsystem verwendet eine bestimmte Art von Absicht, die als `RecognizerIntent`bezeichnet wird. Diese Absicht steuert eine große Anzahl von Parametern, einschließlich der Dauer, die mit dem Ruhezustand gewartet werden muss, bis die Aufzeichnung berücksichtigt wird, weitere Sprachen erkannt und ausgegeben werden können, sowie Text, der als Anweisung in das modale Dialogfeld der `Intent`eingeschlossen werden soll. In diesem Code Ausschnitt ist `VOICE` ein `readonly int`, der für die Erkennung in `OnActivityResult`verwendet wird.

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

### <a name="conversion-of-the-speech"></a>Konvertierung der Sprache

Der Text, der aus der Sprache interpretiert wird, wird im `Intent`übermittelt, der zurückgegeben wird, wenn die Aktivität abgeschlossen wurde und über `GetStringArrayListExtra(RecognizerIntent.ExtraResults)`auf Sie zugegriffen wird. Dadurch wird eine `IList<string>`zurückgegeben, von der der Index verwendet und angezeigt werden kann, abhängig von der Anzahl der Sprachen, die in der Anforderung des Aufrufers angefordert (und im `RecognizerIntent.ExtraMaxResults`angegeben) werden. Wie bei jeder Liste ist es jedoch sinnvoll, sicherzustellen, dass Daten angezeigt werden.

Beim lauschen auf den Rückgabewert eines `StartActivityForResult`muss die `OnActivityResult`-Methode bereitgestellt werden.

Im folgenden Beispiel ist `textBox` ein `TextBox`, der verwendet wird, um das, was vorgegeben wurde, auszumachen. Es könnte auch verwendet werden, um den Text an eine Form des interpreters zu übergeben. von dort aus kann die Anwendung den Text vergleichen und mit einem anderen Teil der Anwendung verzweigen.

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

## <a name="text-to-speech"></a>Text-to-Speech

Text-zu-Sprache ist nicht das Gegenteil von Sprache zu Text und basiert auf zwei Hauptkomponenten: eine Text-zu-Sprache-Engine, die auf dem Gerät installiert wird, und eine installierte Sprache.

Größtenteils sind auf Android-Geräten der standardmäßige Google TTS-Dienst und mindestens eine Sprache installiert. Dies wird bei der Ersteinrichtung des Geräts eingerichtet und basiert auf dem Zeitpunkt, an dem sich das Gerät befindet (z. b. Wenn ein in Deutschland eingelegtes Telefon in Deutschland die deutschsprachige Sprache installiert).

### <a name="step-1---instantiating-texttospeech"></a>Schritt 1: Instanziieren von textpospeech

`TextToSpeech` kann bis zu drei Parameter annehmen, die ersten beiden sind erforderlich, damit das dritte optional ist (`AppContext`, `IOnInitListener`, `engine`). Der Listener wird zum Binden an den Dienst und zum Testen des Fehlers verwendet, wenn die Engine eine beliebige Anzahl von verfügbaren Android-Text für Sprach-Engines hat. Das Gerät hat mindestens das eigene Modul von Google.

### <a name="step-2---finding-the-languages-available"></a>Schritt 2: Suchen der verfügbaren Sprachen

Die `Java.Util.Locale`-Klasse enthält eine hilfreiche Methode namens `GetAvailableLocales()`. Diese Liste der Sprachen, die von der Sprach-Engine unterstützt werden, kann dann anhand der installierten Sprachen getestet werden.

Es ist sehr wichtig, die Liste der "verstanden"-Sprachen zu generieren. Es gibt immer eine Standardsprache (die Sprache, die der Benutzer beim ersten festlegen seines Geräts festgelegt hat). in diesem Beispiel hat die `List<string>` "Default" als ersten Parameter, der Rest der Liste wird abhängig vom Ergebnis der `textToSpeech.IsLanguageAvailable(locale)`ausgefüllt.

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

Mit diesem Code wird [textdespeech. islanguageavailable](xref:Android.Speech.Tts.TextToSpeech.IsLanguageAvailable*) aufgerufen, um zu testen, ob das Sprachpaket für ein bestimmtes Gebiets Schema bereits auf dem Gerät vorhanden ist.
Diese Methode gibt einen [languageavailableresult](xref:Android.Speech.Tts.LanguageAvailableResult)zurück, der angibt, ob die Sprache für das übergebene Gebiets Schema verfügbar ist. Wenn `LanguageAvailableResult` angibt, dass die Sprache `NotSupported`ist, ist für diese Sprache kein Sprachpaket verfügbar (selbst beim Herunterladen). Wenn `LanguageAvailableResult` auf `MissingData`festgelegt ist, ist es möglich, ein neues Sprachpaket herunterzuladen, wie unten in Schritt 4 erläutert.

### <a name="step-3---setting-the-speed-and-pitch"></a>Schritt 3: Festlegen der Geschwindigkeit und der Tonhöhe

Android ermöglicht dem Benutzer, den Ton der Sprache zu ändern, indem er die `SpeechRate` und `Pitch` (die Geschwindigkeit und den Ton der Sprache) ändert. Dies geht zwischen 0 und 1, wobei "normale" Sprache 1 für beides ist.

### <a name="step-4---testing-and-loading-new-languages"></a>Schritt 4: Testen und Laden neuer Sprachen

Das Herunterladen einer neuen Sprache erfolgt mithilfe eines `Intent`. Das Ergebnis dieser Absicht bewirkt, dass die [onactivityresult](xref:Android.App.Activity.OnActivityResult*) -Methode aufgerufen wird. Im Gegensatz zum sprach-zu-Text-Beispiel (in dem die [Erkennungs Absicht](xref:Android.Speech.RecognizerIntent) als `PutExtra` Parameter für die `Intent`verwendet wurde) werden die Test-und lade `Intent`s `Action`basiert:

- Mit dem [textdespeech. Engine. aktioncheckttsdata](xref:Android.Speech.Tts.TextToSpeech.Engine.ActionCheckTtsData) -&ndash; wird eine Aktivität der Plattform `TextToSpeech`-Engine gestartet, um die ordnungsgemäße Installation und Verfügbarkeit von Sprachressourcen auf dem Gerät zu überprüfen.

- [Textdespeech. Engine. aktioninstallttsdata](xref:Android.Speech.Tts.TextToSpeech.Engine.ActionInstallTtsData) &ndash; startet eine Aktivität, die den Benutzer auffordert, die erforderlichen Sprachen herunterzuladen.

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

`TextToSpeech.Engine.ActionCheckTtsData` testet auf die Verfügbarkeit von Sprachressourcen. `OnActivityResult` wird aufgerufen, wenn dieser Test abgeschlossen ist. Wenn Sprachressourcen heruntergeladen werden müssen, `OnActivityResult` die `TextToSpeech.Engine.ActionInstallTtsData` Aktion auslösen, um eine Aktivität zu starten, mit der der Benutzer die erforderlichen Sprachen herunterladen kann. Beachten Sie, dass diese `OnActivityResult` Implementierung den `Result` Code nicht überprüft, da in diesem vereinfachten Beispiel bereits feststellt, dass das Sprachpaket heruntergeladen werden muss.

Durch die `TextToSpeech.Engine.ActionInstallTtsData` Aktion wird dem Benutzer die **Google TTS Voice-Daten** Aktivität zum Auswählen von Sprachen zum Herunterladen angezeigt:

![Google TTS-Sprach Daten Aktivität](speech-images/01-google-tts-voice-data.png)

Beispielsweise kann der Benutzer Französisch auswählen und auf das Download Symbol klicken, um französische Sprach Daten herunterzuladen:

![Beispiel für das Herunterladen von französischer Sprache](speech-images/02-selecting-french.png)

Die Installation dieser Daten erfolgt automatisch, nachdem der Download abgeschlossen wurde.

### <a name="step-5---the-ioninitlistener"></a>Schritt 5: der ioninitlistener

Damit eine Aktivität den Text in Sprache konvertieren kann, muss die Schnittstellen Methode `OnInit` implementiert werden (Dies ist der zweite Parameter, der für die Instanziierung der `TextToSpeech`-Klasse angegeben wird). Dadurch wird der Listener initialisiert und das Ergebnis getestet.

Der Listener sollte mindestens auf `OperationResult.Success` und `OperationResult.Failure` getestet werden.
Das folgende Beispiel zeigt genau Folgendes:

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

In dieser Anleitung haben wir uns mit den Grundlagen der Umwandlung von Text in Sprache und Sprache in Text sowie mit möglichen Methoden zum Einschließen von Text in Ihre eigenen apps beschäftigt. Obwohl Sie nicht jeden bestimmten Fall abdecken, sollten Sie nun ein grundlegendes Verständnis der Interpretation von Sprache, der Installation neuer Sprachen und der Erhöhung der Inklusivität Ihrer apps haben.

## <a name="related-links"></a>Verwandte Links

- [Xamarin. Forms dependencyservice](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/dependencyservice//)
- [Text-to-Speech (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-texttospeech)
- [Sprache für Text (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-speechtotext)
- [Android. Speech-Namespace](xref:Android.Speech)
- [Android. Speech. TTS-Namespace](xref:Android.Speech.Tts)
