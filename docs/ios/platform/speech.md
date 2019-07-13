---
title: Spracherkennung in Xamarin.iOS
description: Dieser Artikel stellt die neue Spracheingabe-API und zeigt, wie sie in einer Xamarin.iOS-app fortlaufende Spracherkennung unterstützen und Spracherkennung (von Livedaten oder aufgezeichnete Audiodatenströme) in Text zu transkribieren implementieren.
ms.prod: xamarin
ms.assetid: 64FED50A-6A28-4833-BEAE-63CEC9A09010
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 8af7474036eb0fd6e2236cf52e96b8d12c8bc44e
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2019
ms.locfileid: "67865704"
---
# <a name="speech-recognition-in-xamarinios"></a>Spracherkennung in Xamarin.iOS

_Dieser Artikel stellt die neue Spracheingabe-API und zeigt, wie sie in einer Xamarin.iOS-app fortlaufende Spracherkennung unterstützen und Spracherkennung (von Livedaten oder aufgezeichnete Audiodatenströme) in Text zu transkribieren implementieren._

Neue auf iOS 10, Version Apple hat der Spracheingabe-API, die eine iOS-app fortlaufende Spracherkennung unterstützen und transkribieren Speech (von Livedaten oder aufgezeichnete Audiodatenströme) in Text ermöglicht.

Gemäß den Apple hat der Spracheingabe-API die folgenden Features und Vorteile:

- Äußerst präzise
- Stand der Technik
- Einfach zu verwenden
- Fast
- Unterstützt mehrere Sprachen
- Benutzerdatenschutz Hinsicht

## <a name="how-speech-recognition-works"></a>Funktionsweise der Spracherkennung

Spracherkennung wird in einer iOS-app implementiert, durch das Erfassen von live oder aufgezeichnete Audio (in einem der gesprochene Sprachen, die die API unterstützt wird) und Übergabe an eine Spracherkennung die Erkennung der eine nur-Text-Transkription der gesprochenen Worte zurückgibt.

[![](speech-images/speech01.png "Funktionsweise der Spracherkennung")](speech-images/speech01.png#lightbox)

### <a name="keyboard-dictation"></a>Tastaturdiktierfunktion

Wenn die meisten Benutzer der Spracherkennung auf iOS-Geräten denken, betrachten sie integrierte Sprach-Assistenten Siri, die zusammen mit Tastaturdiktierfunktion IOS 5, mit dem iPhone 4 s veröffentlicht wurde.

Tastaturdiktierfunktion wird unterstützt durch beliebige Elemente der Benutzeroberfläche, die TextKit unterstützt (z. B. `UITextField` oder `UITextArea`) und durch den Benutzer, die Sie auf die Schaltfläche Spracheingaben (direkt auf der linken Seite der LEERTASTE) auf der virtuellen iOS-Tastatur aktiviert wird.

Apple hat die folgenden Tastaturdiktierfunktion-Statistiken (seit 2011 aufgelistet):

- Tastaturdiktierfunktion wurde seit vertriebsbeginn IOS 5 häufig verwendet.
- Ungefähr 65.000 apps pro Tag verwenden.
- Diktat erfolgt über ein Drittel aller iOS in einer 3rd Party-app verwendet wird.

Tastaturdiktierfunktion ist sehr benutzerfreundlich, da kein Aufwand des Entwicklers Teil, abgesehen von der Verwendung von einem Element der Debuggerbenutzeroberfläche befindet TextKit im Design der Benutzeroberfläche der app erforderlich ist. Tastaturdiktierfunktion verfügt auch über den Vorteil, dass keine Sonderberechtigung Anforderungen aus der app aus, bevor sie verwendet werden kann.

Apps, die die neuen Speech Recognition-APIs verwenden müssen besondere Berechtigungen vom Benutzer erteilt werden, da Spracherkennung die Übertragung und die temporäre Speicherung von Daten auf Apple Server erforderlich ist. Informieren Sie sich unsere [Verbesserungen Sicherheit und Datenschutz](~/ios/app-fundamentals/security-privacy.md) Dokumentation.

Tastaturdiktierfunktion ist, zwar einfach zu implementieren ist es eine Frage mit mehreren Einschränkungen und Nachteile:

- Sie erfordert die Verwendung von einem Textfeld für die Eingabe und Anzeige der Tastatur.
- Es funktioniert mit live-Audiodaten nur aus, und die app hat keine Kontrolle über den Prozess der audioaufzeichnung.
- Es bietet keine Kontrolle über die Sprache, die zum Interpretieren des Benutzers Sprache verwendet wird.
- Es gibt keine Möglichkeit für die app zu wissen, ob die Schaltfläche mit den Diktatmodus auch für den Benutzer verfügbar ist.
- Die app kann nicht den audioaufzeichnung Prozess anpassen.
- Es bietet einen sehr oberflächlichen Satz von Ergebnissen, die Informationen wie z. B. der zeitlichen Steuerung und das Selbstvertrauen, verfügt nicht über.

### <a name="speech-recognition-api"></a>API-Spracherkennung

Neue Apple hat für iOS 10, der Spracheingabe-API bietet eine leistungsstärkere Möglichkeit für eine iOS-app Spracherkennung zu implementieren. Diese API ist dasjenige, das Apple Siri und Tastaturdiktierfunktion sowohl Power verwendet und kann schnell Aufzeichnung mit modernsten Genauigkeit bereitstellen.

Die Ergebnisse der Spracheingabe-API sind transparent für die einzelnen Benutzer, ohne die app keine sammeln oder den Zugriff auf private Daten angepasst.

Der Spracheingabe-API stellt die Ergebnisse zurück an die aufrufende Anwendung in nahezu Echtzeit, wie der Benutzer spricht, und Weitere Informationen zu den Ergebnissen der Übersetzung als nur-Text bietet bereit. Dazu gehören:

- Mehrere Interpretationen verfügen, der der Benutzer als bezeichnet.
- Vertrauensgrade für die einzelnen Übersetzungen.
- Informationen zur zeitlichen Steuerung.

Wie bereits erwähnt, kann Audio für die Übersetzung entweder durch eine live Feed oder von aufgezeichnete Quelle und in über 50 Sprachen und Regionalsprachen von iOS 10 unterstützt bereitgestellt werden.

Der Spracheingabe-API kann verwendet werden, auf allen iOS-Geräten mit iOS 10 und in den meisten Fällen erfordert eine aktive Internetverbindung, da der Großteil der Übersetzungen in Apple Server ausgeführt wird. Allerdings einige neuere iOS-Geräte immer unterstützen, die Übersetzung auf dem Gerät bestimmte Sprachen.

Apple hat eine Verfügbarkeit-API, um festzustellen, ob eine bestimmte Sprache für Übersetzung zum aktuellen Zeitpunkt verfügbar ist, enthalten. Die app sollte diese API verwenden, anstelle der direkten Internetverbindung selbst testen zu können.

Wie oben im Abschnitt Tastaturdiktierfunktion erwähnt, Spracherkennung erfordert die Übertragung und die temporäre Speicherung von Daten auf Apple Server über das Internet und als solche, die app _müssen_ fordern Sie die Berechtigung des Benutzers ausführen Anerkennung durch Einschließen der `NSSpeechRecognitionUsageDescription` -Schlüssel in der `Info.plist` Datei ab, und rufen die `SFSpeechRecognizer.RequestAuthorization` Methode. 

Auf Grundlage der Quelle für das Audio für die Spracherkennung verwendet wird, weitere Änderungen an der app `Info.plist` Datei ist möglicherweise erforderlich. Informieren Sie sich unsere [Verbesserungen Sicherheit und Datenschutz](~/ios/app-fundamentals/security-privacy.md) Dokumentation.

## <a name="adopting-speech-recognition-in-an-app"></a>Übernehmen die Spracherkennung in einer App

Es gibt vier wichtigsten Schritte, die Entwickler durchführen muss, um die Spracherkennung in einer iOS-app zu übernehmen:

- Geben Sie eine nutzungsbeschreibung der App `Info.plist` Datei mithilfe der `NSSpeechRecognitionUsageDescription` Schlüssel. Für eine Kamera-app kann beispielsweise die folgende Beschreibung _"Dadurch können Sie ein Bild aufnehmen kann, indem Sie einfach das Wort"Cheese"."_
- Autorisierung anfordern, durch den Aufruf der `SFSpeechRecognizer.RequestAuthorization` Methode, um eine Erklärung zu präsentieren (bereitgestellt, der `NSSpeechRecognitionUsageDescription` Taste oben) warum die app Spracherkennung möchte Anerkennung Zugriff auf den Benutzer in einem Dialogfeld und ermöglicht es ihnen, annehmen oder ablehnen.
- Erstellen Sie eine Speech Recognition-Anforderung:
    * Verwenden Sie für aufgezeichnete Audioinhalte auf dem Datenträger, die `SFSpeechURLRecognitionRequest` Klasse.
    * Verwenden Sie für die live-Audio (oder Audiodaten aus dem Arbeitsspeicher), die `SFSPeechAudioBufferRecognitionRequest` Klasse.
- Übergeben Sie die Speech Recognition-Anforderung an eine Spracherkennung die Erkennung (`SFSpeechRecognizer`) um Erkennung zu beginnen. Die app kann optional auf die zurückgegebene enthalten `SFSpeechRecognitionTask` überwachen und verfolgen Sie die Erkennungsergebnisse.

Diese Schritte werden weiter unten ausführlich behandelt.

### <a name="providing-a-usage-description"></a>Bereitstellen einer Beschreibung der Verwendung

Zu den erforderlichen `NSSpeechRecognitionUsageDescription` -Schlüssel in der `Info.plist` Datei, gehen Sie folgendermaßen vor:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Doppelklicken Sie auf die `Info.plist` Datei, die sie für die Bearbeitung zu öffnen.
2. Wechseln Sie zu der **Quelle** anzeigen: 

    [![](speech-images/speech02.png "Die Datenquellensicht")](speech-images/speech02.png#lightbox)
3. Klicken Sie auf **neuen Eintrag hinzufügen**, geben Sie `NSSpeechRecognitionUsageDescription` für die **Eigenschaft**, `String` für die **Typ** und **Nutzungsbeschreibung** als die **Wert**. Beispiel: 

    [![](speech-images/speech03.png "Hinzufügen von NSSpeechRecognitionUsageDescription")](speech-images/speech03.png#lightbox)
4. Wenn die app live audiotranskription durchführen, müssen sie auch eine Nutzungsbeschreibung für Mikrofon. Klicken Sie auf **neuen Eintrag hinzufügen**, geben Sie `NSMicrophoneUsageDescription` für die **Eigenschaft**, `String` für die **Typ** und **Nutzungsbeschreibung** als die **Wert**. Beispiel: 

    [![](speech-images/speech04.png "Adding NSMicrophoneUsageDescription")](speech-images/speech04.png#lightbox)
5. Speichern Sie die Änderungen in der Datei.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Doppelklicken Sie auf die `Info.plist` Datei, die sie für die Bearbeitung zu öffnen.
2. Klicken Sie auf **neuen Eintrag hinzufügen**, geben Sie `NSSpeechRecognitionUsageDescription` für die **Eigenschaft**, `String` für die **Typ** und **Nutzungsbeschreibung** als die **Wert**. Beispiel: 

    [![](speech-images/speech03w.png "Hinzufügen von NSSpeechRecognitionUsageDescription")](speech-images/speech03w.png#lightbox)
3. Wenn die app live audiotranskription durchführen, müssen sie auch eine Nutzungsbeschreibung für Mikrofon. Klicken Sie auf **neuen Eintrag hinzufügen**, geben Sie `NSMicrophoneUsageDescription` für die **Eigenschaft**, `String` für die **Typ** und **Nutzungsbeschreibung** als die **Wert**. Beispiel: 

    [![](speech-images/speech04w.png "Adding NSMicrophoneUsageDescription")](speech-images/speech04w.png#lightbox)
4. Speichern Sie die Änderungen in der Datei.

-----

> [!IMPORTANT]
> Geben Sie entweder die oben genannten Schritte nicht `Info.plist` Schlüssel (`NSSpeechRecognitionUsageDescription` oder `NSMicrophoneUsageDescription`) kann dazu führen, die app, die Fehler ohne Warnung, wenn Sie versuchen, auf die Spracherkennung oder das Mikrofon für live-Audio.




### <a name="requesting-authorization"></a>Eine Autorisierung anfordert

Um die erforderliche Autorisierung anzufordern, die in der app auf die Spracherkennung zugreifen können, bearbeiten Sie die wichtigsten View Controller-Klasse, und fügen Sie den folgenden Code hinzu:

```csharp
using System;
using UIKit;
using Speech;

namespace MonkeyTalk
{
    public partial class ViewController : UIViewController
    {
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Request user authorization
            SFSpeechRecognizer.RequestAuthorization ((SFSpeechRecognizerAuthorizationStatus status) => {
                // Take action based on status
                switch (status) {
                case SFSpeechRecognizerAuthorizationStatus.Authorized:
                    // User has approved speech recognition
                    ...
                    break;
                case SFSpeechRecognizerAuthorizationStatus.Denied:
                    // User has declined speech recognition
                    ...
                    break;
                case SFSpeechRecognizerAuthorizationStatus.NotDetermined:
                    // Waiting on approval
                    ...
                    break;
                case SFSpeechRecognizerAuthorizationStatus.Restricted:
                    // The device is not permitted
                    ...
                    break;
                }
            });
        }
    }
}
```

Die `RequestAuthorization` -Methode der der `SFSpeechRecognizer` Klasse anfordert, der Benutzer zum Zugriff Spracherkennung mithilfe des Grunds, die in der Entwickler bereitgestellt der `NSSpeechRecognitionUsageDescription` Schlüssel, der die `Info.plist` Datei.

Ein `SFSpeechRecognizerAuthorizationStatus` Ergebnis wird zurückgegeben, um die `RequestAuthorization` Methode Rückrufroutine, die verwendet werden kann, eine Aktion basierend auf die Berechtigung des Benutzers. 

> [!IMPORTANT]
> Apple empfiehlt warten, bis der Benutzer eine Aktion in der app, die Spracherkennung muss gestartet wurde, bevor Sie diese Berechtigung anfordern.

### <a name="recognizing-pre-recorded-speech"></a>Aufgezeichnete Sprache erkennen

Wenn möchte, dass die app Spracherkennung aus einer zuvor erstelltem WAV- oder MP3-Datei zu erkennen, können sie den folgenden Code:

```csharp
using System;
using UIKit;
using Speech;
using Foundation;
...

public void RecognizeFile (NSUrl url)
{
    // Access new recognizer
    var recognizer = new SFSpeechRecognizer ();

    // Is the default language supported?
    if (recognizer == null) {
        // No, return to caller
        return;
    }

    // Is recognition available?
    if (!recognizer.Available) {
        // No, return to caller
        return;
    }

    // Create recognition task and start recognition
    var request = new SFSpeechUrlRecognitionRequest (url);
    recognizer.GetRecognitionTask (request, (SFSpeechRecognitionResult result, NSError err) => {
        // Was there an error?
        if (err != null) {
            // Handle error
            ...
        } else {
            // Is this the final translation?
            if (result.Final) {
                Console.WriteLine ("You said, \"{0}\".", result.BestTranscription.FormattedString);
            }
        }
    });
}
```

Betrachten diesen Code im Detail, zuerst versucht wird, erstellen Sie eine Spracherkennung (`SFSpeechRecognizer`). Wenn die Standardsprache für die Spracherkennung, nicht unterstützt wird `null` wird zurückgegeben, und die Funktionen wird beendet.

Wenn die freigegebene Spracherkennung. verfügbar für die Standardsprache ist, wird die app überprüft, um festzustellen, ob sie derzeit für die Verwendung von Erkennung verfügbar ist die `Available` Eigenschaft. Z. B. Erkennung möglicherweise nicht verfügbar, wenn das Gerät nicht über eine aktive Internetverbindung verfügt.

Ein `SFSpeechUrlRecognitionRequest` wird erstellt, aus der `NSUrl` Speicherort der aufgezeichnete Datei auf dem iOS-Gerät, und es wird für die Spracherkennung mit Rückrufroutine übergeben.

Wenn der Rückruf aufgerufen wird, wenn die `NSError` ist nicht `null` wurde ein Fehler auf, die verarbeitet werden müssen. Da Spracherkennung inkrementell ausgeführt wird, die Rückrufroutine kann aufgerufen werden mehr als einmal, sodass die `SFSpeechRecognitionResult.Final` Eigenschaft wird getestet, um festzustellen, ob die Übersetzung abgeschlossen ist, und am besten geeignete Version der Übersetzung geschrieben (`BestTranscription`).

### <a name="recognizing-live-speech"></a>Erkennen von Live-Sprache

Wenn möchte, dass die app live Sprache zu erkennen, ist der Prozess ähnelt aufgezeichnete Sprache erkennen. Beispiel:

```csharp
using System;
using UIKit;
using Speech;
using Foundation;
using AVFoundation;
...

#region Private Variables
private AVAudioEngine AudioEngine = new AVAudioEngine ();
private SFSpeechRecognizer SpeechRecognizer = new SFSpeechRecognizer ();
private SFSpeechAudioBufferRecognitionRequest LiveSpeechRequest = new SFSpeechAudioBufferRecognitionRequest ();
private SFSpeechRecognitionTask RecognitionTask;
#endregion
...

public void StartRecording ()
{
    // Setup audio session
    var node = AudioEngine.InputNode;
    var recordingFormat = node.GetBusOutputFormat (0);
    node.InstallTapOnBus (0, 1024, recordingFormat, (AVAudioPcmBuffer buffer, AVAudioTime when) => {
        // Append buffer to recognition request
        LiveSpeechRequest.Append (buffer);
    });

    // Start recording
    AudioEngine.Prepare ();
    NSError error;
    AudioEngine.StartAndReturnError (out error);

    // Did recording start?
    if (error != null) {
        // Handle error and return
        ...
        return;
    }

    // Start recognition
    RecognitionTask = SpeechRecognizer.GetRecognitionTask (LiveSpeechRequest, (SFSpeechRecognitionResult result, NSError err) => {
        // Was there an error?
        if (err != null) {
            // Handle error
            ...
        } else {
            // Is this the final translation?
            if (result.Final) {
                Console.WriteLine ("You said \"{0}\".", result.BestTranscription.FormattedString);
            }
        }
    });
}

public void StopRecording ()
{
    AudioEngine.Stop ();
    LiveSpeechRequest.EndAudio ();
}

public void CancelRecording ()
{
    AudioEngine.Stop ();
    RecognitionTask.Cancel ();
}
```

Betrachten diesen Code im Detail, erstellt es etliche private Variablen, um den Erkennungsvorgang zu behandeln:

```csharp
private AVAudioEngine AudioEngine = new AVAudioEngine ();
private SFSpeechRecognizer SpeechRecognizer = new SFSpeechRecognizer ();
private SFSpeechAudioBufferRecognitionRequest LiveSpeechRequest = new SFSpeechAudioBufferRecognitionRequest ();
private SFSpeechRecognitionTask RecognitionTask;
```

AV-Foundation verwendet, um Audio aufzeichnen, die zum übergeben werden eine `SFSpeechAudioBufferRecognitionRequest` zur Verarbeitung der Anforderung verwendet:

```csharp
var node = AudioEngine.InputNode;
var recordingFormat = node.GetBusOutputFormat (0);
node.InstallTapOnBus (0, 1024, recordingFormat, (AVAudioPcmBuffer buffer, AVAudioTime when) => {
    // Append buffer to recognition request
    LiveSpeechRequest.Append (buffer);
});
```

Die app versucht, starten Sie die Aufzeichnung, und alle Fehler werden behandelt, wenn die Aufzeichnung nicht gestartet werden kann:

```csharp
AudioEngine.Prepare ();
NSError error;
AudioEngine.StartAndReturnError (out error);

// Did recording start?
if (error != null) {
    // Handle error and return
    ...
    return;
}
```

Die Erkennung Aufgabe gestartet wird und ein Handle an die Aufgabe verwendet wird (`SFSpeechRecognitionTask`):

```csharp
RecognitionTask = SpeechRecognizer.GetRecognitionTask (LiveSpeechRequest, (SFSpeechRecognitionResult result, NSError err) => {
    ...
});
```

Der Rückruf wird in ähnlicher Weise wie höher aufgezeichnete Spracherkennung verwendet verwendet.

Wenn vom Benutzer Aufzeichnung gestoppt wird, werden sowohl die Audio-Engine als auch die Speech Recognition Anforderung informiert:

```csharp
AudioEngine.Stop ();
LiveSpeechRequest.EndAudio ();
```

Wenn der Benutzer die Anerkennung abbricht, werden die Audio-Engine und die Erkennung informiert:

```csharp
AudioEngine.Stop ();
RecognitionTask.Cancel ();
```

Es ist wichtig, rufen Sie `RecognitionTask.Cancel` , wenn der Benutzer, die Übersetzung abbricht in die Arbeitsspeicher- und des geräteprozessors freizugeben.

> [!IMPORTANT]
> Fehler beim Bereitstellen der `NSSpeechRecognitionUsageDescription` oder `NSMicrophoneUsageDescription` `Info.plist` Schlüssel können dazu führen, die app, die Fehler ohne Warnung, wenn Sie versuchen, auf die Spracherkennung oder das Mikrofon für live-Audio (`var node = AudioEngine.InputNode;`). Informieren Sie sich die **Bereitstellen einer Beschreibung der Verwendung** Abschnitt Weitere Informationen.

## <a name="speech-recognition-limits"></a>Speech Recognition-Grenzwerte

Apple gelten die folgenden Einschränkungen bei der Verwendung der Spracherkennung in einer iOS-app:

- Spracherkennung ist kostenlos für alle apps, aber die Verwendung ist nicht unbegrenzt:
    - Einzelne iOS-Geräte haben eine begrenzte Anzahl von Erkennungsvorgänge, die pro Tag ausgeführt werden können.
    - Apps werden global pro Anforderung pro Tag gedrosselt.
- Die app muss vorbereitet sein, um Netzwerkverbindung für die Spracherkennung verwendet und Nutzung Rate-Limit-Fehler zu behandeln.
- Spracherkennung kann hohe Kosten in akkuentleerung und hoher Netzwerkdatenverkehr auf iOS-Gerät des Benutzers aus diesem Grund, Apple unterstützt maximal strikte audio-Dauer ungefähr eine Minute Spracheingabe max.

Wenn eine app regelmäßig die Einschränkung der Bitrate Grenzwerte erreicht, fordert Apple an, dass der Entwickler an diesen Partner wenden.

## <a name="privacy-and-usability-considerations"></a>Datenschutz und Benutzerfreundlichkeit Überlegungen

Apple hat den folgenden Vorschlag für transparent und der Rücksichtnahme auf die Privatsphäre des Benutzers bei der Spracherkennung in einer iOS-app einschließen:

- Bei der Aufzeichnung des Benutzers Spracherkennung, achten Sie darauf, dass Sie eindeutig anzugeben, dass die Aufzeichnung in der app-Benutzeroberfläche durchgeführt wird. Die app könnte z. B. eine "Aufzeichnen" sound wiedergeben und aufzeichnen Indikator anzeigen.
- Verwenden Sie nicht die Spracherkennung für vertrauliche Benutzerinformationen wie Kennwörter, Daten oder Finanzdaten.
- Anzeigen der Ergebnisse bei der _vor_ , die auf diese. Dies bietet nicht nur Feedback, was die app macht, aber ermöglicht dem Benutzer, der Anerkennung Fehler zu behandeln, wie sie gemacht werden.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel verfügt über die neuen Spracheingabe-API angezeigt und wurde gezeigt, wie sie in einer Xamarin.iOS-app fortlaufende Spracherkennung unterstützen und Spracherkennung (von Livedaten oder aufgezeichnete Audiodatenströme) in Text zu transkribieren implementieren. 



## <a name="related-links"></a>Verwandte Links

- [SpeakToMe (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios10/SpeakToMe/)
