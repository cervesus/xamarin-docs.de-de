---
title: Android Speech
description: "Dieser Artikel behandelt die Grundlagen der Verwendung sehr leistungsstarken Android.Speech-Namespaces. Seit der Einführung wurde Android Spracherkennung erkannt und als Text ausgeben können. Es ist ein relativ einfacher Vorgang. Für Text-zu-Sprache, der Prozess ist jedoch komplizierter, da nicht nur das Sprachmodul verfügt, berücksichtigt werden, aber auch die Sprachen verfügbar und aus dem Text-zu-Sprache (Sprachausgabe)-System installiert."
ms.topic: article
ms.prod: xamarin
ms.assetid: FA3B8EC4-34D2-47E3-ACEA-BD34B28115B9
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: e8e56afbdf0b68ecc49a89b08b2e67a9715f2aef
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2018
---
# <a name="android-speech"></a>Android Speech

_Dieser Artikel behandelt die Grundlagen der Verwendung sehr leistungsstarken Android.Speech-Namespaces. Seit der Einführung wurde Android Spracherkennung erkannt und als Text ausgeben können. Es ist ein relativ einfacher Vorgang. Für Text-zu-Sprache, der Prozess ist jedoch komplizierter, da nicht nur das Sprachmodul verfügt, berücksichtigt werden, aber auch die Sprachen verfügbar und aus dem Text-zu-Sprache (Sprachausgabe)-System installiert._

## <a name="speech-overview"></a>Übersicht über die Spracherkennung

Mit einem System "verstanden" menschliche Sprache und enunciates, was eingegeben wird – Sprache Text "und" Text-zu-Sprache – ein zunehmende Bereich im mobile Entwicklung ist, wie die Nachfrage nach natürlichen Kommunikation mit unserer Geräte Zählerwert. Es gibt viele Instanzen, in denen eine Funktion, die konvertiert Text in Sprache oder umgekehrt, ist sehr nützlich, in der android-Anwendung integrieren müssen.

Mit der Clamp unten auf mobilem Gerät verwenden und gleichzeitig möchten Benutzer z. B. eine Hände freien Möglichkeit, ihre Geräte funktioniert. Fülle an verschiedenen Formfaktoren arbeiten Android – z. B. Android Dach – und -erweiternde Aufnahme dieser Android-Geräte (z. B. Tablets und Notizblock) verwenden erstellt hat einen größeren Schwerpunkt auf großartige Sprachausgabe-Anwendungen.

Google bietet den Entwickler einen umfangreichen Satz von APIs im Namespace Android.Speech um decken den meisten Fällen erstellen Sie ein Gerät "Spracherkennung beachten" (z. B. für Blinde entwickelte Software).  Der Namespace beinhaltet die Möglichkeit, damit der Text in der Sprache über übersetzt werden `Android.Speech.Tts`, Kontrolle über das Modul verwendet, um die Übersetzung sowie eine Anzahl von auszuführen `RecognizerIntent`Objekten, die Sprache in Text konvertiert werden können.

Während die Funktionen für die Sprache in sein vorliegen, gelten Einschränkungen, die basierend auf der Hardware verwendet. Es ist unwahrscheinlich, dass das Gerät erfolgreich alles, was in jedes verfügbare Sprache gesprochen interpretiert wird.

## <a name="requirements"></a>Anforderungen

Es gibt keine speziellen Anforderungen für dieses Handbuch als Ihr Gerät mit einem Mikrofons und Lautsprechers.

Der Kern eines Android-Geräts interpretieren Sprache ist die Verwendung von ein `Intent` mit einem entsprechenden `OnActivityResult`.
Es ist wichtig, jedoch zu erkennen, dass die Spracherkennung nicht interpretiert werden kann – jedoch Text interpretiert. Der Unterschied ist wichtig.

### <a name="the-difference-between-understanding-and-interpreting"></a>Der Unterschied zwischen verstehen und interpretieren

Eine einfache Definition Kenntnisstand ist, dass Sie von Ton und dem angegebenen Kontext feststellen, was vorhergesagt wird, wird die echte Bedeutung sind. Um einfach zu interpretieren bedeutet, dass die Wörter dauern und in eine andere Form ausgegeben.

Betrachten Sie das folgende einfache Beispiel, das in alltäglichen Konversation verwendet wird: 

<kbd>Guten Tag, wie geht es dir?</kbd>

Ohne gehen (besonders für bestimmte Wörter oder Teile von Wörtern) ist es eine einfache Fragen. Wenn es sich bei eine langsame Geschwindigkeit auf die Zeile angewendet wird, wird jedoch die Person, die überwacht erkennen, dem Fragesteller ist nicht zu glücklich und eventuell benötigt cheering einrichten oder dass dem Fragesteller nämlich ist. Wenn das Augenmerk auf "are", die Person, die in der Regel größerer Bedeutung in der Antwort ist.

Ohne relativ leistungsfähige Audio Verarbeitung stellen die gehen und ein Maß KI (AI), um den Kontext verstehen, Software darf nicht selbst beginnen, um zu verstehen, was Sie sagen – die am besten, einem einfachen Telefon zu erreichen, ist die Sprache in Text konvertieren.

## <a name="setting-up"></a>Einrichtung

Bevor Sie mit der Sprache, ist es immer ratsam, stellen Sie sicher, dass das Gerät ein Mikrofon verfügt. Gäbe es wenig Sinn, die beim Versuch, Ihre app auf einem Kindle oder Google Notizblock ohne installiertes Mikrofon auszuführen.

Im folgenden Codebeispiel wird veranschaulicht, Abfragen, wenn ein Mikrofon verfügbar ist, und falls nicht, eine Warnung generiert. Wenn kein Mikrofon verfügbar ist würde an diesem Punkt Sie beenden die Aktivität oder deaktivieren die Möglichkeit, die Spracherkennung aufzuzeichnen.

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

### <a name="creating-the-intent"></a>Die Absicht erstellen

Die Absicht für das Spracherkennungssystem verwendet eine bestimmte Art von Absicht bezeichnet den `RecognizerIntent`. Diese Absicht steuert eine große Anzahl von Parametern, wie lange auf Stumme warten Sie, bis die Aufzeichnung, über zusätzlichen Sprachen erkennen und ausgegeben betrachtet wird, einschließlich und den Text auf der `Intent`des modales Dialogfeld als bedeutet, dass der Anweisung. In diesem Codeausschnitt `VOICE` ist ein `readonly int` verwendet für die Erkennung in `OnActivityResult`.

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

### <a name="conversion-of-the-speech"></a>Konvertierung von der Spracherkennung

Der Text, der von der Spracherkennung interpretiert wird übermittelt werden, in der `Intent`, der zurückgegeben wird, wenn die Aktivität abgeschlossen wurde, erfolgt über `GetStringArrayListExtra(RecognizerIntent.ExtraResults)`. Dadurch wird zurückgegeben, eine `IList<string>`, von dem der Index verwendet und angezeigt werden kann, abhängig von der Anzahl der Sprachen, die in der Aufrufer Absicht angefordert (und im angegebenen der `RecognizerIntent.ExtraMaxResults`). Als lohnt sich mit einer Liste jedoch überprüft wird, stellen Sie sicher, dass Daten, die angezeigt werden.

Wenn für den Rückgabewert des überwacht eine `StartActivityForResult`, die `OnActivityResult` -Methode muss angegeben werden.

Im folgenden Beispiel wird `textBox` ist eine `TextBox` für die Ausgabe, was hat vorgegeben wurde. Konnte gleichermaßen, übergeben den Text in eine Form der Interpreter und von dort verwendet werden, die Anwendung kann vergleichen, die Text und die Verzweigung zu einem anderen Teil der Anwendung.

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
                  switch(matches[0].Substring(0,5).ToLower())
                  {
                     case "north":
                      MovePlayer(0);
                     break;
                   case "south":
                     MovePlayer(1);
                     break;
             }
             else
                  textBox.Text = "No speech was recognised";
        }
   }
    base.OnActivityResult(requestCode, resultVal, data);
}
```

## <a name="text-to-speech"></a>Text-zu-Sprache

Text-zu-Sprache ist nicht sehr das Gegenteil der Sprache, Text und basiert auf zwei wichtige Komponenten; ein-Sprach-Modul auf dem Gerät installiert wird, und eine anderen Sprache installiert wird.

Zum größten Teil, Android-Geräte gibt, hat den Standardwert Google TTS-Dienst installiert und mindestens eine Sprache. Dies wird hergestellt, wenn das Gerät zunächst einrichten und basieren auf, in dem das Gerät zu diesem Zeitpunkt ist (z. B. ein Telefon, richten Sie in Deutschland installiert deutschen Sprache, während jeweils eine America amerikanisches Englisch).

### <a name="step-1---instantiating-texttospeech"></a>Step 1 - Instantiating TextToSpeech

`TextToSpeech` bis zu 3 Parameter annehmen können, die ersten beiden sind erforderlich, mit dem dritten wird optional (`AppContext`, `IOnInitListener`, `engine`). Der Listener wird verwendet, um an den Dienst und der Test für Fehler mit dem Modul wird eine beliebige Anzahl von verfügbaren Android Text-zu-Sprache-Module zu binden. Zumindest müssen das Gerät Googles-Modul.

### <a name="step-2---finding-the-languages-available"></a>Schritt 2: Suchen nach verfügbaren Sprachen

Die `Java.Util.Locale` Klasse enthält eine nützliche Methode namens `GetAvailableLocales()`. Diese Liste mit durch das Sprachmodul unterstützten Sprachen kann dann für die installierten Sprachen getestet werden soll.

Es ist einem minimalen Aufwand verbunden, um die Liste der Sprachen "verstanden" zu generieren. Es wird immer eine Standardsprache (die Sprache der Benutzer festlegen, wenn sie ihr Gerät zuerst eingerichtet), werden dies der Fall ist in diesem Beispiel die `List<string>` wurde von "Default" als erster Parameter, der Rest der Liste wird je nach Ergebnis ausgefüllt werden die `textToSpeech.IsLanguageAvailable(locale)`.

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

### <a name="step-3---setting-the-speed-and-pitch"></a>Schritt 3 – Festlegen der Geschwindigkeit und Tonhöhe

Android ermöglicht dem Benutzer den Sound die Spracherkennung durch Ändern von alter der `SpeechRate` und `Pitch` (die Rate der Geschwindigkeit und dem Signalton an, der die Sprache). Dies geht von 0 auf 1 fest, mit der "normale" Sprache wird für beide 1.

### <a name="step-4---testing-and-loading-new-languages"></a>Schritt 4: Testen und das Laden neuer Sprachen

Dies erfolgt mithilfe einer `Intent` mit dem Ergebnis interpretiert wird `OnActivityResult`. Im Gegensatz zu der Sprache-zu-Text-Beispiel, das verwendet die `RecognizerIntent` als eine `PutExtra` Parameter an die `Intent`, die Installation Absicht verwendet eine `Action`.

Es ist möglich, eine neue Sprache von Google mithilfe des folgenden Codes zu installieren. Das Ergebnis der `Activity` geprüft, ob die Sprache erforderlich ist, und wenn dies der Fall, installiert die Sprache nach Nachfrage nach dem Download erfolgen.

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

### <a name="step-5---the-ioninitlistener"></a>Schritt 5 – die IOnInitListener

Für eine Aktivität zum Konvertieren der Text-zu-Sprache, die Schnittstellenmethode können `OnInit` implementiert werden muss (Dies ist der zweite Parameter für die Instanziierung des angegebenen der `TextToSpeech` Klasse). Den Listener initialisiert und überprüft das Ergebnis.

Der Listener für beide testen sollten `OperationResult.Success` und `OperationResult.Failure` mindestens.
Das folgende Beispiel zeigt nur diese:

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

In diesem Handbuch haben wir mit den Grundlagen von Konvertieren von Text-zu-Sprache und Spracherkennung in Text- und mögliche Verfahren zum Einschließen in Ihrer eigenen apps gesucht. Während sie nicht jeder Fall abdecken, sollten Sie jetzt einen grundlegenden Überblick darüber, wie die Spracherkennung interpretiert wird, wie Sie neue Sprachen installieren und zum Erhöhen der Inclusivity Ihrer Apps haben.



## <a name="related-links"></a>Verwandte Links

- [Xamarin.Forms DependencyService](https://developer.xamarin.com/samples/UsingDependencyService/)
- [Text-zu-Sprache (Beispiel)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/TextToSpeech)
- [Sprache zu Text (Beispiel)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/SpeechToText)
- [Android.Speech-namespace](https://developer.xamarin.com/api/namespace/Android.Speech/)
- [Android.Speech.Tts namespace](https://developer.xamarin.com/api/namespace/Android.Speech.Tts/)
