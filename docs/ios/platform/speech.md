---
title: Spracherkennung in xamarin. IOS
description: In diesem Artikel wird die neue Speech-API vorgestellt und gezeigt, wie Sie Sie in einer xamarin. IOS-App implementieren, um die fortlaufende Spracherkennung und die Sprachaufzeichnung (aus Live-oder aufgezeichneten Audiodatenströmen) in Text zu unterstützen.
ms.prod: xamarin
ms.assetid: 64FED50A-6A28-4833-BEAE-63CEC9A09010
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: c4b818bcf3c4a5280c0280a2e28e2f59c65c8c81
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86930220"
---
# <a name="speech-recognition-in-xamarinios"></a>Spracherkennung in xamarin. IOS

_In diesem Artikel wird die neue Speech-API vorgestellt und gezeigt, wie Sie Sie in einer xamarin. IOS-App implementieren, um die fortlaufende Spracherkennung und die Sprachaufzeichnung (aus Live-oder aufgezeichneten Audiodatenströmen) in Text zu unterstützen._

Neu bei IOS 10 hat Apple die Spracherkennungs-API freigegeben, mit der eine IOS-App fortlaufende sprach Erkennungs-und transkribiersprache (aus Live-oder aufgezeichneten Audiodatenströmen) in Text unterstützt.

Gemäß Apple hat die Spracherkennungs-API die folgenden Features und Vorteile:

- Streng genau
- Der Zustand der Kunst
- Benutzerfreundlich
- Schnell
- Unterstützt mehrere Sprachen
- Respektiert den Benutzer Datenschutz

## <a name="how-speech-recognition-works"></a>Funktionsweise der Spracherkennung

Die Spracherkennung wird in einer IOS-App implementiert, indem entweder Live oder vorab aufgezeichnete Audiodaten (in den von der API unterstützten Sprachen) abgerufen und an eine Spracherkennung übergeben werden, die eine klar Text Aufzeichnung der gesprochenen Wörter zurückgibt.

[![Funktionsweise der Spracherkennung](speech-images/speech01.png)](speech-images/speech01.png#lightbox)

### <a name="keyboard-dictation"></a>Tastatur Diktat

Wenn die meisten Benutzer die Spracherkennung auf einem IOS-Gerät vorstellen, stellen Sie sich den integrierten Siri-sprach-Assistenten vor, der zusammen mit der Tastatur Diktat Erstellung in ios 5 mit dem iPhone 4S freigegeben wurde.

Tastatur Diktat werden von jedem Interface-Element unterstützt, das textkit unterstützt (z. b. `UITextField` oder `UITextArea` ), und es wird durch den Benutzer aktiviert, indem er auf die Schaltfläche "Diktat" (direkt links neben der Leertaste) in der virtuellen IOS-Tastatur klickt.

Apple hat die folgende Tastatur Diktat Statistik veröffentlicht (seit 2011 erfasst):

- Die Tastatur Diktat wurde häufig verwendet, seit Sie in ios 5 veröffentlicht wurde.
- Ungefähr 65.000 Apps verwenden Sie pro Tag.
- In einer Drittanbieter-APP wird ein Drittel aller IOS-Diktat Aktionen ausgeführt.

Die Tastatur Diktat ist äußerst einfach zu verwenden, da es keinen Aufwand für den Entwickler Teil erfordert, außer die Verwendung eines textkit-Schnittstellen Elements im UI-Design der app. Die Tastatur Diktat hat auch den Vorteil, dass keine speziellen Berechtigungsanforderungen von der APP erforderlich sind, bevor Sie verwendet werden können.

Apps, die die neuen Spracherkennungs-APIs verwenden, erfordern spezielle Berechtigungen, die vom Benutzer erteilt werden müssen, da die Spracherkennung die Übertragung und temporäre Speicherung von Daten auf den Servern von Apple erfordert. Ausführliche Informationen finden Sie in unserer Dokumentation zu [Sicherheits-und Datenschutz Erweiterungen](~/ios/app-fundamentals/security-privacy.md) .

Obwohl die Tastatur Diktat leicht implementiert werden kann, gibt es einige Einschränkungen und Nachteile:

- Hierfür müssen ein Text Eingabefeld und die Anzeige einer Tastatur verwendet werden.
- Es funktioniert nur mit liveaudioeingaben, und die APP hat keine Kontrolle über den audioaufzeichnungs Prozess.
- Sie bietet keine Kontrolle über die Sprache, die zum Interpretieren der Sprache des Benutzers verwendet wird.
- Es gibt keine Möglichkeit, dass die APP weiß, ob die Diktat Schaltfläche für den Benutzer überhaupt verfügbar ist.
- Die APP kann den audioaufzeichnungs Prozess nicht anpassen.
- Er bietet einen sehr flachen Satz von Ergebnissen, bei denen keine Informationen wie zeitliche Steuerung und Zuverlässigkeit fehlen.

### <a name="speech-recognition-api"></a>Spracherkennungs-API

Neu bei IOS 10 hat Apple die Spracherkennungs-API veröffentlicht, die eine leistungsfähigere Methode für eine IOS-App zur Implementierung der Spracherkennung bereitstellt. Bei dieser API handelt es sich um die gleiche API, die von Apple zur Unterstützung von Siri und Tastatur Diktat verwendet wird, und es ist in der Lage, eine schnelle Aufzeichnung mit dem Zustand der Kunst Genauigkeit bereitzustellen

Die von der Spracherkennungs-API bereitgestellten Ergebnisse werden transparent an die einzelnen Benutzer angepasst, ohne dass die APP private Benutzerdaten erfassen oder darauf zugreifen muss.

Die Spracherkennungs-API liefert die Ergebnisse der aufrufenden App nahezu in Echtzeit zurück, wenn der Benutzer spricht, und er bietet weitere Informationen über die Ergebnisse der Übersetzung als nur Text. Dazu gehören:

- Mehrere Interpretationen der Aussagen des Benutzers.
- Vertrauens Ebenen für die einzelnen Übersetzungen.
- Informationen zur zeitlichen Steuerung.

Wie bereits erwähnt, kann Audioübertragung für die Übersetzung entweder durch einen LiveFeed oder von vorab aufgezeichneten Quellen und in allen mehr als 50 Sprachen und von IOS 10 unterstützten Dialekten bereitgestellt werden.

Die Spracherkennungs-API kann auf allen IOS-Geräten verwendet werden, auf denen IOS 10 ausgeführt wird. in den meisten Fällen ist eine Live-Internetverbindung erforderlich, da der Großteil der Übersetzungen auf den Servern von Apple stattfindet. Das heißt, einige neuere IOS-Geräte unterstützen die Always on-Übersetzung für bestimmte Sprachen.

Apple hat eine Verfügbarkeits-API enthalten, um zu bestimmen, ob eine bestimmte Sprache zum aktuellen Zeitpunkt für die Übersetzung verfügbar ist. Die APP sollte diese API verwenden, anstatt direkt auf die Internet Konnektivität zu testen.

Wie oben im Abschnitt "Tastatur Diktat" erwähnt, erfordert die Spracherkennung die Übertragung und temporäre Speicherung von Daten auf Apple-Servern über das Internet. daher _muss_ die APP die Berechtigung des Benutzers zum Durchführen der Erkennung anfordern, indem der `NSSpeechRecognitionUsageDescription` Schlüssel in seine `Info.plist` Datei eingeschlossen und die-Methode aufgerufen wird `SFSpeechRecognizer.RequestAuthorization` . 

Basierend auf der Quelle der Audiodaten, die für die Spracherkennung verwendet werden, sind möglicherweise andere Änderungen an der Datei der APP `Info.plist` erforderlich. Ausführliche Informationen finden Sie in unserer Dokumentation zu [Sicherheits-und Datenschutz Erweiterungen](~/ios/app-fundamentals/security-privacy.md) .

## <a name="adopting-speech-recognition-in-an-app"></a>Anwenden der Spracherkennung in einer APP

Der Entwickler muss vier wichtige Schritte ausführen, um die Spracherkennung in einer IOS-APP zu übernehmen:

- Geben `Info.plist` Sie mithilfe des Schlüssels eine Verwendungs Beschreibung in der Datei der APP an `NSSpeechRecognitionUsageDescription` . Eine Kamera-APP könnte z. b. die folgende Beschreibung enthalten: _"Dies ermöglicht es Ihnen, ein Foto zu nehmen, indem Sie das Wort" Käse "sagen._
- Fordern Sie eine Autorisierung durch Aufrufen der- `SFSpeechRecognizer.RequestAuthorization` Methode an, um eine Erläuterung (im obigen Schlüssel bereitgestellt) anzuzeigen, `NSSpeechRecognitionUsageDescription` warum die APP sprach Erkennungs Zugriff auf den Benutzer in einem Dialogfeld wünscht, damit Sie akzeptiert oder abgelehnt werden kann.
- Erstellen Sie eine sprach Erkennungs Anforderung:
  - Verwenden Sie für vorab aufgezeichnete Audiodaten auf dem Datenträger die- `SFSpeechURLRecognitionRequest` Klasse.
  - Verwenden Sie für liveaudiodaten (oder Audiodateien aus dem Arbeitsspeicher) die- `SFSPeechAudioBufferRecognitionRequest` Klasse.
- Übergeben Sie die sprach Erkennungs Anforderung an eine sprach Erkennungsfunktion ( `SFSpeechRecognizer` ), um die Erkennung zu beginnen. Die APP kann optional auf dem zurückgegebenen enthalten `SFSpeechRecognitionTask` , um die Erkennungsergebnisse zu überwachen und zu verfolgen.

Diese Schritte werden im folgenden ausführlich beschrieben.

### <a name="providing-a-usage-description"></a>Bereitstellen einer Nutzungs Beschreibung

`NSSpeechRecognitionUsageDescription`Gehen Sie folgendermaßen vor, um den erforderlichen Schlüssel in der Datei bereitzustellen `Info.plist` :

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

1. Doppelklicken Sie `Info.plist` auf die Datei, um Sie zur Bearbeitung zu öffnen.
2. Wechseln Sie zur **Quell** Ansicht: 

    [![Die Quell Ansicht](speech-images/speech02.png)](speech-images/speech02.png#lightbox)
3. Klicken Sie auf **neuen Eintrag hinzufügen**, geben Sie `NSSpeechRecognitionUsageDescription` als- **Eigenschaft**für `String` den **Typ** und eine **Verwendungs Beschreibung** als **Wert**ein. Beispiel: 

    [![Nssprachlos erkentionusagedescription wird hinzugefügt](speech-images/speech03.png)](speech-images/speech03.png#lightbox)
4. Wenn die APP die Live-Audioaufzeichnung verarbeitet, wird auch eine Beschreibung der Mikrofon Verwendung benötigt. Klicken Sie auf **neuen Eintrag hinzufügen**, geben Sie `NSMicrophoneUsageDescription` als- **Eigenschaft**für `String` den **Typ** und eine **Verwendungs Beschreibung** als **Wert**ein. Beispiel: 

    [![Hinzufügen von nsmikrophoneusagedescription](speech-images/speech04.png)](speech-images/speech04.png#lightbox)
5. Speichern Sie die Änderungen in der Datei.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Doppelklicken Sie `Info.plist` auf die Datei, um Sie zur Bearbeitung zu öffnen.
2. Klicken Sie auf **neuen Eintrag hinzufügen**, geben Sie `NSSpeechRecognitionUsageDescription` als- **Eigenschaft**für `String` den **Typ** und eine **Verwendungs Beschreibung** als **Wert**ein. Beispiel: 

    [![Nssprachlos erkentionusagedescription wird hinzugefügt](speech-images/speech03w.png)](speech-images/speech03w.png#lightbox)
3. Wenn die APP die Live-Audioaufzeichnung verarbeitet, wird auch eine Beschreibung der Mikrofon Verwendung benötigt. Klicken Sie auf **neuen Eintrag hinzufügen**, geben Sie `NSMicrophoneUsageDescription` als- **Eigenschaft**für `String` den **Typ** und eine **Verwendungs Beschreibung** als **Wert**ein. Beispiel: 

    [![Hinzufügen von nsmikrophoneusagedescription](speech-images/speech04w.png)](speech-images/speech04w.png#lightbox)
4. Speichern Sie die Änderungen in der Datei.

-----

> [!IMPORTANT]
> Wenn Sie keinen der oben genannten `Info.plist` Schlüssel (oder) bereitstellen, `NSSpeechRecognitionUsageDescription` `NSMicrophoneUsageDescription` kann dies dazu führen, dass die APP ohne Warnung fehlschlägt, wenn Sie versuchen, auf die Spracherkennung oder das Mikrofon für liveaudiodaten zuzugreifen.

### <a name="requesting-authorization"></a>Anfordern der Autorisierung

Um die erforderliche Benutzer Autorisierung anzufordern, die der APP den Zugriff auf die Spracherkennung ermöglicht, bearbeiten Sie die Main View Controller-Klasse, und fügen Sie den folgenden Code hinzu:

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

Die- `RequestAuthorization` Methode der- `SFSpeechRecognizer` Klasse fordert vom Benutzer die Berechtigung für den Zugriff auf die Spracherkennung mit dem Grund an, den der Entwickler im `NSSpeechRecognitionUsageDescription` Schlüssel der Datei bereitgestellt hat `Info.plist` .

Ein `SFSpeechRecognizerAuthorizationStatus` Ergebnis wird an die `RequestAuthorization` Rückruf Routine der Methode zurückgegeben, die verwendet werden kann, um auf der Grundlage der Benutzer Berechtigung Aktionen auszuführen. 

> [!IMPORTANT]
> Apple schlägt vor, bis der Benutzer eine Aktion in der APP gestartet hat, die Spracherkennung erfordert, bevor Sie diese Berechtigung anfordern.

### <a name="recognizing-pre-recorded-speech"></a>Erkennen von voraufgezeichneten Spracheingaben

Wenn die APP Sprache aus einer vorab aufgezeichneten WAV-oder MP3-Datei erkennen will, kann Sie den folgenden Code verwenden:

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

Wenn Sie diesen Code ausführlich betrachten, versucht er zunächst, eine Spracherkennung () zu erstellen `SFSpeechRecognizer` . Wenn die Standardsprache für die Spracherkennung nicht unterstützt wird, `null` wird zurückgegeben, und die Funktionen werden beendet.

Wenn die Spracherkennung für die Standardsprache verfügbar ist, wird von der APP überprüft, ob Sie zurzeit für die Erkennung mithilfe der-Eigenschaft verfügbar ist `Available` . Beispielsweise ist die Erkennung möglicherweise nicht verfügbar, wenn das Gerät nicht über eine aktive Internetverbindung verfügt.

Eine `SFSpeechUrlRecognitionRequest` wird aus dem `NSUrl` Speicherort der vorab aufgezeichneten Datei auf dem IOS-Gerät erstellt und an die Spracherkennung übergeben, damit Sie mit einer Rückruf Routine verarbeitet werden kann.

Wenn der Rückruf aufgerufen wird, `NSError` `null` ist ein Fehler aufgetreten, der behandelt werden muss. Da die Spracherkennung inkrementell erfolgt, kann die Rückruf Routine mehrmals aufgerufen werden, damit die-Eigenschaft überprüft wird, `SFSpeechRecognitionResult.Final` ob die Übersetzung abgeschlossen ist und die beste Version der Übersetzung geschrieben wird ( `BestTranscription` ).

### <a name="recognizing-live-speech"></a>Erkennen von Live Speech

Wenn die APP Live Speech erkennen möchte, ist der Prozess sehr ähnlich wie die Erkennung vorab aufgezeichneter Spracheingaben. Beispiel:

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

Wenn Sie diesen Code ausführlich betrachten, werden mehrere private Variablen erstellt, um den Erkennungsprozess zu behandeln:

```csharp
private AVAudioEngine AudioEngine = new AVAudioEngine ();
private SFSpeechRecognizer SpeechRecognizer = new SFSpeechRecognizer ();
private SFSpeechAudioBufferRecognitionRequest LiveSpeechRequest = new SFSpeechAudioBufferRecognitionRequest ();
private SFSpeechRecognitionTask RecognitionTask;
```

Es verwendet die AV Foundation zum Aufzeichnen von Audiodaten, die an einen `SFSpeechAudioBufferRecognitionRequest` zur Verarbeitung der Erkennungs Anforderung übermittelt werden:

```csharp
var node = AudioEngine.InputNode;
var recordingFormat = node.GetBusOutputFormat (0);
node.InstallTapOnBus (0, 1024, recordingFormat, (AVAudioPcmBuffer buffer, AVAudioTime when) => {
    // Append buffer to recognition request
    LiveSpeechRequest.Append (buffer);
});
```

Die APP versucht, die Aufzeichnung zu starten, und alle Fehler werden behandelt, wenn die Aufzeichnung nicht gestartet werden kann:

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

Die Erkennungs Aufgabe wird gestartet, und ein Handle wird an die Erkennungs Aufgabe ( `SFSpeechRecognitionTask` ) beibehalten:

```csharp
RecognitionTask = SpeechRecognizer.GetRecognitionTask (LiveSpeechRequest, (SFSpeechRecognitionResult result, NSError err) => {
    ...
});
```

Der Rückruf wird in ähnlicher Weise verwendet wie der oben aufgezeichnete sprach Abruf.

Wenn die Aufzeichnung vom Benutzer angehalten wird, werden sowohl die Audioengine als auch die sprach Erkennungs Anforderung informiert:

```csharp
AudioEngine.Stop ();
LiveSpeechRequest.EndAudio ();
```

Wenn der Benutzer die Erkennung abbricht, werden die Audioengine und die Erkennungs Aufgabe informiert:

```csharp
AudioEngine.Stop ();
RecognitionTask.Cancel ();
```

Es ist wichtig, dass aufgerufen wird, `RecognitionTask.Cancel` Wenn der Benutzer die Übersetzung abbricht, um sowohl Speicher als auch den Prozessor des Geräts freizugeben.

> [!IMPORTANT]
> Wenn Sie die `NSSpeechRecognitionUsageDescription` -oder-Schlüssel nicht bereitstellen, `NSMicrophoneUsageDescription` `Info.plist` kann dies dazu führen, dass die APP ohne Warnung fehlschlägt, wenn Sie versuchen, auf die Spracherkennung oder das Mikrofon für Livedaten zuzugreifen `var node = AudioEngine.InputNode;` Weitere Informationen finden Sie im obigen Abschnitt **Bereitstellen einer Verwendungs Beschreibung** .

## <a name="speech-recognition-limits"></a>Grenzen der Spracherkennung

Apple erzwingt bei der Arbeit mit der Spracherkennung in einer IOS-APP die folgenden Einschränkungen:

- Die Spracherkennung ist für alle apps kostenlos, aber Ihre Nutzung ist nicht unbegrenzt:
  - Einzelne IOS-Geräte verfügen über eine begrenzte Anzahl von erkenungen, die pro Tag ausgeführt werden können.
  - Apps werden pro Tag Global gedrosselt.
- Die APP muss für die Behandlung von Fehlern bei der sprach Erkennungs Netzwerkverbindung und bei Verwendungsraten Limits vorbereitet sein.
- Die Spracherkennung kann sowohl für den Akku-als auch für den hohen Netzwerkverkehr auf dem IOS-Gerät des Benutzers einen hohen Kostenaufwand haben. daher erzwingt Apple eine strikte Audiodauer von ungefähr einer Minute sprach Maximum.

Wenn eine APP regelmäßig die Grenzwerte für die Raten Drosselung trifft, fordert Apple Sie dazu auf, Sie zu kontaktieren.

## <a name="privacy-and-usability-considerations"></a>Überlegungen zum Datenschutz

Apple hat den folgenden Vorschlag, um transparent zu sein und den Datenschutz des Benutzers beim einschließen der Spracherkennung in eine IOS-APP zu berücksichtigen:

- Wenn Sie die Sprache des Benutzers aufzeichnen, stellen Sie sicher, dass die Aufzeichnung in der Benutzeroberfläche der APP stattfindet. So kann die APP beispielsweise einen Sound "Aufzeichnung" Abspielen und einen Aufzeichnungs Indikator anzeigen.
- Verwenden Sie die Spracherkennung nicht für sensible Benutzerinformationen wie Kenn Wörter, Integritäts Daten oder Finanzinformationen.
- Zeigen Sie die Erkennungsergebnisse an, _bevor_ Sie Sie ausführen. Dadurch erhalten Sie nicht nur Feedback, wie die app ausgeführt wird, sondern ermöglicht es dem Benutzer, Erkennungs Fehler zu behandeln, sobald sie auftreten.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde die neue Speech-API vorgestellt und gezeigt, wie Sie Sie in einer xamarin. IOS-App implementieren, um fortlaufende Spracherkennung und transcriwriter (aus Live-oder aufgezeichneten Audiodatenströmen) in Text zu unterstützen. 

## <a name="related-links"></a>Verwandte Links

- [Speaktome (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-speaktome)
