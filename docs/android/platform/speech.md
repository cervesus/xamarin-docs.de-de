---
title: Android-Sprache
description: Dieser Artikel behandelt die Grundlagen der Verwendung des leistungsstarken von Android.Speech-Namespaces. Seit seiner Einführung wurde Android Sprache erkennen und die Ausgabe als Text speichern können. Es ist ein relativ einfacher Prozess. Für Text-zu-Sprache, jedoch ist der Vorgang komplizierter, da nicht nur die spracherkennungs-Engine, berücksichtigt werden muss, aber auch die Sprachen verfügbaren und installierten aus dem System Text To Speech (TTS).
ms.prod: xamarin
ms.assetid: FA3B8EC4-34D2-47E3-ACEA-BD34B28115B9
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/02/2018
ms.openlocfilehash: e88f6e24cbf4c8b2f0c0486c6408e234e87066cc
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104348"
---
# <a name="android-speech"></a>Android-Sprache

_Dieser Artikel behandelt die Grundlagen der Verwendung des leistungsstarken von Android.Speech-Namespaces. Seit seiner Einführung wurde Android Sprache erkennen und die Ausgabe als Text speichern können. Es ist ein relativ einfacher Prozess. Für Text-zu-Sprache, jedoch ist der Vorgang komplizierter, da nicht nur die spracherkennungs-Engine, berücksichtigt werden muss, aber auch die Sprachen verfügbaren und installierten aus dem System Text To Speech (TTS)._

## <a name="speech-overview"></a>Übersicht über die Spracherkennung

Müssen ein System, das "versteht" menschliche Stimme und enunciates, welche Eingabe zu verweisen, Text und Text in Sprache für die Spracherkennung – ist der stetig anwachsenden Bereich in der Entwicklung für mobile Geräte wie die Nachfrage nach natürlicher Kommunikationsmethoden mit unseren Geräte steigt. Es gibt viele Situationen, in denen haben ein Feature, das konvertiert Text in Sprache oder umgekehrt, ist ein sehr nützliches Tool, um in Ihrer android-Anwendung zu integrieren.

Mit der Clamp nach unten auf dem Mobiltelefon verwenden beim vorantreiben möchten Benutzer z. B. eine praktische kostenlose Möglichkeit des Betriebs ihrer Geräte. Der Fülle an verschiedene Android-Formfaktoren – z. B. Android Wear, und die ständig erweiternde Einbeziehung dieser Android-Geräte (z. B. Tablets und Hinweis Pads) verwenden, erstellt hat einen größeren Schwerpunkt auf hervorragende TTS-Anwendungen.

Google bietet den Entwickler einen umfangreichen Satz von APIs im Namespace von Android.Speech um decken die meisten Instanzen von einem Gerät "Speech bewusst" (z. B. für Blinde entwickelte Software).  Der Namespace beinhaltet das Gebäude, damit der Text in Sprache über übersetzt werden `Android.Speech.Tts`, Kontrolle über die Engine verwendet, um die Übersetzung, sowie eine Anzahl von `RecognizerIntent`s die Sprache in Text konvertiert werden können.

Zwar die Funktionen gibt es für die Spracherkennung verstanden werden, es gibt Einschränkungen basierend auf der Hardware, verwendet. Es ist unwahrscheinlich, dass das Gerät, wird alles, was gesprochen, in jeder Sprache zur Verfügung erfolgreich interpretieren.

## <a name="requirements"></a>Anforderungen

Es gibt keine besonderen Anforderungen für dieses Handbuch, als Ihr Gerät mit einem Mikrofon und Redner.

Der Kern von einem Android-Gerät Spracherkennung zu interpretieren ist die Verwendung von einem `Intent` mit einem entsprechenden `OnActivityResult`.
Es ist wichtig, jedoch zu beachten, dass die Spracherkennung nicht verstanden wird, jedoch Text interpretiert. Der Unterschied ist wichtig.

### <a name="the-difference-between-understanding-and-interpreting"></a>Der Unterschied zwischen verstehen und Interpretieren von

Eine einfache Definition Verständnis ist, können Sie durch den Ton und Kontext bestimmen, die wirkliche Bedeutung von Aussagen wird. Um einfach zu interpretieren bedeutet, dass die Wörter werden und in einer anderen Form ausgegeben.

Betrachten Sie das folgende einfache Beispiel, das in alltäglichen Konversation verwendet wird: 

<kbd>Guten Tag, wie geht es dir?</kbd>

Ohne Wendepunkt (besonders für bestimmte Wörter oder Teilen von Wörtern) ist es eine einfache Frage. Wenn es sich bei eine langsame Geschwindigkeit auf die Zeile angewendet wird, wird jedoch die Person, die überwacht erkennen, dem Fragesteller nicht zu viel Spaß beim und vielleicht cheering einrichten oder dem Fragesteller nämlich. Wenn das Augenmerk wird auf "are", die Person, die in der Regel Weitere Informationen über die Antwort ist.

Ohne Recht leistungsstarke Audio Verarbeitung stellen den Tonfall und ein Maß für künstliche Intelligenz (KI) verwenden, um den Kontext zu verstehen, die Software darf nicht auch beginnen, um zu verstehen, was Sie sagen – das beste möglich, eine einfache Phone wird die Sprache in Text konvertieren.

## <a name="setting-up"></a>Einrichtung

Bevor Sie mit der Sprache, ist es immer ratsam zu prüfen, stellen Sie sicher, dass das Gerät über ein Mikrofon verfügt. Wäre es wenig Sinn, die Ihre app auf ein Kindle oder Google-Notizblock ohne installiertes Mikrofon ausführen möchten.

Das folgende Codebeispiel zeigt, Abfragen, wenn ein Mikrofon verfügbar ist, und falls nicht, um eine Warnung zu erstellen. Wenn kein Mikrofon verfügbar ist. würde an diesem Punkt Sie die Aktivität zu beenden oder deaktivieren die Möglichkeit, notieren Sie die Sprache.

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

### <a name="creating-the-intent"></a>Erstellen die Absicht

Das Ziel für das Spracherkennungssystem verwendet, einen bestimmten Typ von der Absicht, die Namen der `RecognizerIntent`. Diese Absicht steuert eine große Anzahl von Parametern, wie lange Stumme zu warten, bis es sich bei die Aufzeichnung betrachtet wird, über zusätzlichen Sprachen zu erkennen und die Ausgabe, einschließlich und dem Text, schließen Sie auf die `Intent`modales Dialogfeld als Mittel der Anweisung. In diesem Ausschnitt `VOICE` ist eine `readonly int` verwendet für die Erkennung in `OnActivityResult`.

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

### <a name="conversion-of-the-speech"></a>Konvertierung von der Sprache

Der Text, der von der Spracherkennung interpretiert werden übermittelt werden, in der `Intent`, der Fehler wird zurückgegeben, wenn die Aktivität abgeschlossen wurde und über erfolgt `GetStringArrayListExtra(RecognizerIntent.ExtraResults)`. Als Ergebnis erhalten eine `IList<string>`, von dem der Index verwendet und angezeigt werden kann, abhängig von der Anzahl von Sprachen, die in der Absicht Aufrufer angefordert (und im angegebenen die `RecognizerIntent.ExtraMaxResults`). Als lohnt sich mit einer beliebigen Liste jedoch durch, um sicherzustellen, dass Daten, die angezeigt werden.

Wenn der Rückgabewert von überwacht eine `StartActivityForResult`, `OnActivityResult` Methode muss angegeben werden.

Im folgenden Beispiel wird `textBox` ist eine `TextBox` für die Ausgabe, was hat vorgegeben wurde. Konnte den Text in eine Form des Interpreters, und von dort übergibt gleichermaßen verwendet werden, die Anwendung kann vergleichen, das Text und den Branch mit einem anderen Teil der Anwendung.

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

## <a name="text-to-speech"></a>Text in Sprache

Text-zu-Sprache ist nicht ganz das Gegenteil von Sprache in Text und basiert auf zwei wichtige Komponenten; eine Text-Sprach-Engine auf dem Gerät installiert wird, und eine Sprache installiert wird.

Zum größten Teil, Android-Geräte gibt, mit dem Google TTS-Dienst installiert und mindestens eine Sprache. Dies wird hergestellt, wenn das Gerät zuerst eingerichtet wird und basieren auf, in dem das Gerät zu diesem Zeitpunkt ist (z. B. einem Smartphone, richten sich in Deutschland wird installiert deutschen Sprache, während jeweils eine America amerikanisches Englisch).

### <a name="step-1---instantiating-texttospeech"></a>Schritt 1: instanziieren TextToSpeech

`TextToSpeech` kann bis zu 3 Parameter annehmen, die ersten beiden sind erforderlich, das dritte als optionale (`AppContext`, `IOnInitListener`, `engine`). Der Listener wird verwendet, um mit dem Dienst und den Test für Fehler mit der Engine wird eine beliebige Anzahl von verfügbaren Android Text in Sprache-Module zu binden. Zumindest müssen das Gerät Googles-Engine.

### <a name="step-2---finding-the-languages-available"></a>Schritt 2: Suchen nach verfügbaren Sprachen

Die `Java.Util.Locale` -Klasse enthält eine nützliche Methode namens `GetAvailableLocales()`. Diese Liste der von der spracherkennungs-Engine unterstützten Sprachen kann dann für die installierten Sprachen getestet werden.

Es ist einem minimalen Aufwand verbunden, um die Liste der Sprachen "verstanden" zu generieren. Sind eine Standardsprache (die Sprache des Benutzers festgelegt werden soll, wenn sie ihr Gerät zuerst eingerichtet), also in diesem Beispiel die `List<string>` ist "Default" als ersten Parameter, der Rest der Liste wird je nach Ergebnis aufgefüllt werden die `textToSpeech.IsLanguageAvailable(locale)`.

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

Dieser Code ruft [TextToSpeech.IsLanguageAvailable](https://developer.xamarin.com/api/member/Android.Speech.Tts.TextToSpeech.IsLanguageAvailable/p/Java.Util.Locale/) zu prüfen, ob das Language Pack für ein bestimmtes Gebietsschema bereits auf dem Gerät vorhanden ist. Diese Methode gibt eine [LanguageAvailableResult](https://developer.xamarin.com/api/type/Android.Speech.Tts.LanguageAvailableResult/), der angibt, ob die Sprache für das übergebene Gebietsschema verfügbar ist. Wenn `LanguageAvailableResult` gibt an, dass die Sprache ist `NotSupported`, dann gibt es kein Voice-Paket verfügbar ist (auch zum Download) für die jeweilige Sprache. Wenn `LanguageAvailableResult` nastaven NA hodnotu `MissingData`, ist es möglich ist, ein neues Sprachpaket herunterzuladen, wie in Schritt 4 unten erläutert.

### <a name="step-3---setting-the-speed-and-pitch"></a>Schritt 3: Festlegen der Geschwindigkeit und pitch

Android ermöglicht dem Benutzer, ändern den Sound die Spracherkennung durch Ändern der `SpeechRate` und `Pitch` (die Rate der Geschwindigkeit und den Farbton der Spracheingabe). Dies geht von 0 auf 1 fest, mit der "normal", 1 für die Sprache.

### <a name="step-4---testing-and-loading-new-languages"></a>Schritt 4: testen, und Laden neue Sprachen

Eine neue Sprache herunterladen erfolgt mithilfe einer `Intent`. Bewirkt, dass das Ergebnis von diesem die [OnActivityResult](https://developer.xamarin.com/api/member/Android.App.Activity.OnActivityResult/) Methode aufgerufen wird. Im Gegensatz zu der Sprache-zu-Text-Beispiel (verwendet, die die [RecognizerIntent](https://developer.xamarin.com/api/type/Android.Speech.RecognizerIntent/) als eine `PutExtra` Parameter, um die `Intent`), das Testen und das Laden von `Intent`sind `Action`-basierend:

-   [TextToSpeech.Engine.ActionCheckTtsData](https://developer.xamarin.com/api/field/Android.Speech.Tts.TextToSpeech+Engine.ActionCheckTtsData/) &ndash; Starts an activity from the platform `TextToSpeech` engine to verify proper installation and availability of language resources on the device.

-   [TextToSpeech.Engine.ActionInstallTtsData](https://developer.xamarin.com/api/field/Android.Speech.Tts.TextToSpeech+Engine.ActionInstallTtsData/) &ndash; Starts an activity that prompts the user to download the necessary languages.

Im folgenden Codebeispiel wird veranschaulicht, wie Sie diese Aktionen mit Sprachressourcen testen und eine neue Sprache herunterladen:

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

`TextToSpeech.Engine.ActionCheckTtsData` testet, ob die Verfügbarkeit von Ressourcen für die Standardsprache. `OnActivityResult` wird aufgerufen, wenn dieser Test abgeschlossen ist. Wenn Ressourcen heruntergeladen werden, müssen `OnActivityResult` ausgelöst wird, deaktiviert der `TextToSpeech.Engine.ActionInstallTtsData` Aktion aus, um eine Aktivität zu starten, die dem Benutzer ermöglicht, die benötigten Sprachen herunterladen. Beachten Sie, das von diesem `OnActivityResult` Implementierung überprüft nicht die `Result` code, da in diesem vereinfachten Beispiel die Überprüfung bereits erstellt wurde, dass das Language Pack heruntergeladen werden muss.

Die `TextToSpeech.Engine.ActionInstallTtsData` Aktion bewirkt, dass die **Google TTS Voice-Daten** Aktivität, die dem Benutzer angezeigt werden, für das Auswählen von Sprachen zum Herunterladen:

![Google-TTS Daten Sprachaktivität](speech-images/01-google-tts-voice-data.png)

Beispielsweise kann der Benutzer Französisch auswählen und klicken Sie auf das Downloadsymbol zum Herunterladen von französischen Voice-Daten:

![Beispiel für die französischen Sprache herunterladen](speech-images/02-selecting-french.png)

Die Installation dieser Daten erfolgt automatisch, nachdem der Download abgeschlossen ist.


### <a name="step-5---the-ioninitlistener"></a>Schritt 5: die IOnInitListener

Für eine Aktivität zum Konvertieren von Text zu Sprache, die Schnittstellenmethode können `OnInit` implementiert werden (Dies ist der zweite Parameter angegeben wird, für die Instanziierung der `TextToSpeech` Klasse). Den Listener initialisiert und überprüft das Ergebnis.

Testen des Listeners muss für beide `OperationResult.Success` und `OperationResult.Failure` mindestens.
Das folgende Beispiel zeigt genau dies:

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

In diesem Handbuch haben wir uns die Grundlagen der Konvertieren von Text in Sprache und Sprache in Text und die möglichen Methoden, wie sie mit Ihren eigenen apps enthalten angesehen haben. Während sie nicht alle speziellen Fall behandelt werden, verfügen Sie nun, einen grundlegenden Überblick darüber, wie Spracherkennung interpretiert werden, wie Sie neue Sprachen installieren und wie Sie die Inclusivity Ihrer Apps erhöhen.



## <a name="related-links"></a>Verwandte Links

- [Xamarin.Forms DependencyService](https://developer.xamarin.com/samples/UsingDependencyService/)
- [Text-zu-Sprache (Beispiel)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/TextToSpeech)
- [Spracherkennung (Beispiel)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/SpeechToText)
- [Von Android.Speech-namespace](https://developer.xamarin.com/api/namespace/Android.Speech/)
- [Android.Speech.Tts-namespace](https://developer.xamarin.com/api/namespace/Android.Speech.Tts/)
