---
title: Spracherkennung
description: "Dieser Artikel bietet neue Speech-API, und es wird gezeigt, wie in einer Xamarin.iOS-app fortlaufende Spracherkennung unterstützen und transkribieren Spracherkennung (von Livedaten oder aufgezeichnete Audiodatenströme) in Text zu implementieren."
ms.topic: article
ms.prod: xamarin
ms.assetid: 64FED50A-6A28-4833-BEAE-63CEC9A09010
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: e868c0ee71688e208c5217d9f5a89ea3acec988c
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="speech-recognition"></a>Spracherkennung

_Dieser Artikel bietet neue Speech-API, und es wird gezeigt, wie in einer Xamarin.iOS-app fortlaufende Spracherkennung unterstützen und transkribieren Spracherkennung (von Livedaten oder aufgezeichnete Audiodatenströme) in Text zu implementieren._

Neue iOS 10, Apple hat Freigabe an die Spracherkennung Recognition-API, die eine iOS-app fortlaufende Spracherkennung unterstützen und transkribieren Spracherkennung (von Livedaten oder aufgezeichnete Audiodatenströme) in Text ermöglicht.

Gemäß den Apple hat die Spracherkennung Recognition-API die folgenden Features und Vorteile:

- Äußerst genaue
- Hochmodern
- Benutzerfreundlich
- Fast
- Unterstützt mehrere Sprachen
- Hinsicht Datenschutz

## <a name="how-speech-recognition-works"></a>Wie funktioniert die Spracherkennung

Spracherkennung wird in einer iOS-app durch live oder aufgezeichnete Audio (in einem gesprochenen Sprache, die die API unterstützt) abrufen und Übergabe an ein von der Spracherkennung die zurückgibt, eine nur-Text-Aufzeichnung der gesprochenen Wörter implementiert.

[![](speech-images/speech01.png "Wie funktioniert die Spracherkennung")](speech-images/speech01.png#lightbox)

### <a name="keyboard-dictation"></a>Tastatur diktieren

Wenn die meisten Benutzer auf einem iOS-Gerät Spracherkennung vorstellen, betrachtet werden sie von der integrierten Sprach-Assistenten Siri, die zusammen mit der Tastatur diktieren in iOS 5, mit der iPhone-4 s veröffentlicht wurde.

Tastatur diktieren wird unterstützt, indem alle Benutzeroberflächenelement, mit dem TextKit unterstützt (z. B. `UITextField` oder `UITextArea`) und durch den Benutzer, die Sie auf die Schaltfläche diktieren (direkt an der linken Seite der LEERTASTE) iOS virtuellen Tastatur aktiviert wird.

Apple hat die folgenden Tastenkombinationen diktieren Statistiken, die (seit 2011 erfasst) veröffentlicht:

- Tastatur diktieren ist weit verwendet wurde, da es im iOS 5 veröffentlicht wurde.
- Ungefähr 65.000 apps verwenden sie pro Tag.
- Diktieren erfolgt über eine dritte alle e/as in einer 3rd Party-app.

Tastatur diktieren ist sehr einfach zu verwenden, benötigt wird, keine Aufwand des Entwicklers Teil, nicht mithilfe einer TextKit Benutzeroberflächen-Elements in der app UI-Entwurf. Tastatur diktieren hat außerdem den Vorteil, dass keine speziellen Berechtigungen Anforderungen aus der app, bevor er verwendet werden kann.

Apps, die neue Sprache Recognition-APIs verwenden, benötigen spezielle Berechtigungen, um vom Benutzer gewährt werden, da Spracherkennung die Übertragung und die temporäre Speicherung von Daten auf der Apple Server erforderlich ist. Finden Sie in unserem [Sicherheit und Datenschutz Erweiterungen](~/ios/app-fundamentals/security-privacy.md) Einzelheiten s. Dokumentation.

Während der Tastatur diktieren einfach zu implementieren ist, kommt es mit verschiedenen Einschränkungen und Nachteile:

- Erfordert die Verwendung eines Textfelds für die Eingabe und Anzeige der Tastatur.
- Generische Vergleich funktioniert mit live-Audio nur aus, und die app hat keine Kontrolle über den Prozess Audioaufnahme.
- Es bietet keine Kontrolle über die Sprache, die zum Interpretieren der Sprache des Benutzers verwendet wird.
- Es gibt keine Möglichkeit für die app zu wissen, ob die Schaltfläche "diktieren" auch für den Benutzer verfügbar ist.
- Die app kann nicht die Audioaufnahme-Prozess anzupassen.
- Es bietet eine sehr flache Reihe von Ergebnissen, die Informationen wie z. B. ein Timeout und vertrauen fehlt.

### <a name="speech-recognition-api"></a>Spracherkennung API

Neu für iOS 10, Apple hat die Spracherkennung Recognition-API die bietet ein leistungsfähigeres Verfahren für ein iOS-app zum Implementieren von Spracherkennung veröffentlicht. Diese API ist dasjenige, das Apple So schalten Sie Siri und Tastatur diktieren verwendet und kann schnell Aufzeichnung hochmodern Genauigkeit bieten.

Die Ergebnisse von der Spracherkennung Recognition-API bereitgestellt werden transparent auf die einzelnen Benutzer ohne die app zur Erfassung bzw. zum Zugriff auf private Benutzerdaten müssen angepasst.

Die Sprach-Erkennung-API stellt Ergebnisse zurück an die aufrufende Anwendung im nahezu in Echtzeit, wie der Benutzer spricht und Weitere Informationen zu den Ergebnissen der Übersetzung als nur-Text bietet. Dazu gehören:

- Mehrere Interpretationen verfügen, von dem, was der Benutzer besitzt.
- Vertrauensgrade für einzelnen Übersetzungen.
- Informationen zur zeitlichen Steuerung.

Wie bereits erwähnt, kann Audio für die Übersetzung entweder durch eine live Feed oder von aufgezeichnete Quelle und in über 50 Sprachen und Dialekte von iOS 10 unterstützt bereitgestellt werden.

Die Sprach-Erkennung-API kann auf beliebige iOS-Geräte mit iOS 10 verwendet werden und ist in den meisten Fällen eine aktive Internetverbindung erforderlich, da der Großteil der Übersetzungen für Apple Server stattfindet. Dies bedeutet, dass einige neuere iOS, die Geräte immer unterstützen, die auf dem Gerät Übersetzung von bestimmten Sprachen.

Apple hat eine Verfügbarkeit-API, um festzustellen, ob eine bestimmte Sprache für die Übersetzung zurzeit verfügbar ist, enthalten. Die app sollten diese API verwenden, anstatt direkte Internetverbindung selbst testen.

Wie im Abschnitt Tastatur diktieren oben bereits erwähnt, Spracherkennung erfordert die Übertragung und die temporäre Speicherung von Daten auf der Apple Server über das Internet und daher die app _müssen_ Erlaubnis des Benutzers ausführen Anerkennung durch einschließlich der `NSSpeechRecognitionUsageDescription` -Schlüssel in seiner `Info.plist` Datei und der Aufruf der `SFSpeechRecognizer.RequestAuthorization` Methode. 

Basierend auf der Quellseite der das Audio für die Spracherkennung verwendet wird, weitere Änderungen an der app `Info.plist` Datei ist möglicherweise erforderlich. Finden Sie in unserem [Sicherheit und Datenschutz Erweiterungen](~/ios/app-fundamentals/security-privacy.md) Einzelheiten s. Dokumentation.

## <a name="adopting-speech-recognition-in-an-app"></a>Einsetzen von Spracherkennung in einer App

Es gibt vier wichtigsten Schritte, die der Entwickler ausführen muss, um die Spracherkennung in einer iOS-app zu übernehmen:

- Geben Sie eine Beschreibung der Verwendung in der app `Info.plist` Datei mithilfe der `NSSpeechRecognitionUsageDescription` Schlüssel. Eine Kamera-app kann beispielsweise die folgende Beschreibung enthalten _"Dies ermöglicht ein Foto, indem Sie einfach darauf hingewiesen werden das Wort"zwar"."_
- Autorisierung anfordern, durch Aufrufen der `SFSpeechRecognizer.RequestAuthorization` Methode, um eine Erklärung zu präsentieren (gemäß der `NSSpeechRecognitionUsageDescription` Schlüssel, die oben genannten) warum die app Spracherkennung möchte Recognition Zugriff auf die Benutzer in einem Dialogfeld und ermöglicht es ihnen, entweder annehmen oder ablehnen.
- Erstellen Sie eine Sprache Recognition-Anforderung:
    * Verwenden Sie vorab Audioaufnahmen auf dem Datenträger, die `SFSpeechURLRecognitionRequest` Klasse.
    * Verwenden Sie für live-Audio (oder Audio aus dem Arbeitsspeicher), die `SFSPeechAudioBufferRecognitionRequest` Klasse.
- An eine von der Spracherkennung übergeben die Spracherkennung Recognition anfordern (`SFSpeechRecognizer`) Recognition beginnen. Die app kann optional auf das zurückgegebene enthalten `SFSpeechRecognitionTask` zu überwachen und verfolgen Sie die Erkennungsergebnisse.

Diese Schritte werden nachfolgend detailliert behandelt.

### <a name="providing-a-usage-description"></a>Bereitstellen einer Beschreibung der Verwendung

Um die erforderlichen bereitzustellen `NSSpeechRecognitionUsageDescription` -Schlüssel in der `Info.plist` Datei, gehen Sie folgendermaßen vor:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Doppelklicken Sie auf die `Info.plist` Datei zur Bearbeitung zu öffnen.
2. Wechseln Sie zu der **Quelle** anzeigen: 

    [![](speech-images/speech02.png "Die Datenquellensicht")](speech-images/speech02.png#lightbox)
3. Klicken Sie auf **neuen Eintrag hinzufügen**, geben Sie `NSSpeechRecognitionUsageDescription` für die **Eigenschaft**, `String` für die **Typ** und ein **Beschreibung der Verwendung** als die **Wert**. Zum Beispiel: 

    [![](speech-images/speech03.png "Hinzufügen von NSSpeechRecognitionUsageDescription")](speech-images/speech03.png#lightbox)
4. Wenn die app live-audio-Aufzeichnung behandeln, können auch eine Beschreibung der Verwendung Mikrofon erforderlich. Klicken Sie auf **neuen Eintrag hinzufügen**, geben Sie `NSMicrophoneUsageDescription` für die **Eigenschaft**, `String` für die **Typ** und ein **Beschreibung der Verwendung** als die **Wert**. Zum Beispiel: 

    [![](speech-images/speech04.png "Hinzufügen von NSMicrophoneUsageDescription")](speech-images/speech04.png#lightbox)
4. Speichern Sie die Änderungen in der Datei.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Doppelklicken Sie auf die `Info.plist` Datei zur Bearbeitung zu öffnen.
3. Klicken Sie auf **neuen Eintrag hinzufügen**, geben Sie `NSSpeechRecognitionUsageDescription` für die **Eigenschaft**, `String` für die **Typ** und ein **Beschreibung der Verwendung** als die **Wert**. Zum Beispiel: 

    [![](speech-images/speech03w.png "Hinzufügen von NSSpeechRecognitionUsageDescription")](speech-images/speech03w.png#lightbox)
4. Wenn die app live-audio-Aufzeichnung behandeln, können auch eine Beschreibung der Verwendung Mikrofon erforderlich. Klicken Sie auf **neuen Eintrag hinzufügen**, geben Sie `NSMicrophoneUsageDescription` für die **Eigenschaft**, `String` für die **Typ** und ein **Beschreibung der Verwendung** als die **Wert**. Zum Beispiel: 

    [![](speech-images/speech04w.png "Hinzufügen von NSMicrophoneUsageDescription")](speech-images/speech04w.png#lightbox)
4. Speichern Sie die Änderungen in der Datei.

-----

> [!IMPORTANT]
> Geben Sie entweder die oben genannten Schritte nicht `Info.plist` Schlüssel (`NSSpeechRecognitionUsageDescription` oder `NSMicrophoneUsageDescription`) kann dazu führen, die Fehler ohne Warnung beim Versuch, den Zugriff auf die Spracherkennung oder das Mikrofon Audio live-app.




### <a name="requesting-authorization"></a>Autorisierung anfordern

Um die erforderlichen benutzerautorisierung anfordern, die die app auf Spracherkennung zugreifen können, bearbeiten Sie die wichtigsten View Controller-Klasse, und fügen Sie den folgenden Code hinzu:

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

Die `RequestAuthorization` Methode der `SFSpeechRecognizer` Klasse wird vom Benutzer Berechtigung anfordern, um Zugriff Spracherkennung mithilfe den Grund, die in der Entwickler bereitgestellt der `NSSpeechRecognitionUsageDescription` -Schlüssel des der `Info.plist` Datei.

Ein `SFSpeechRecognizerAuthorizationStatus` Ergebnis wird zurückgegeben, um die `RequestAuthorization` Methode Callback-Routine, die verwendet werden kann, um Maßnahmen basierend auf die Berechtigung des Benutzers. 

> [!IMPORTANT]
> Apple wird vorgeschlagen, warten, bis der Benutzer eine Aktion in der app, die Spracherkennung erfordert gestartet wurde, bevor Sie diese Berechtigung anfordern.

### <a name="recognizing-pre-recorded-speech"></a>Erkennen von aufgezeichnete-Sprache

Wenn die app Spracherkennung aus einer aufgezeichnete WAV- oder MP3-Datei möchte, können sie den folgenden Code:

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

Betrachten diesen Code im Detail, zuerst versucht wird, erstellen Sie eine von der Spracherkennung (`SFSpeechRecognizer`). Wenn die Standardsprache nicht, für die Spracherkennung unterstützt wird `null` wird zurückgegeben, und die Funktionen wird beendet.

Wenn der von der Spracherkennung für die Standardsprache verfügbar ist, die app wird geprüft, um festzustellen, ob sie für die Erkennung mit derzeit verfügbar ist die `Available` Eigenschaft. Z. B. Erkennung möglicherweise nicht verfügbar, wenn das Gerät nicht über eine aktive Internetverbindung verfügt.

Ein `SFSpeechUrlRecognitionRequest` wird erstellt, aus der `NSUrl` Speicherort der aufgezeichnete Datei auf dem iOS-Gerät auf die von der Spracherkennung für die Verarbeitung mit Rückrufroutine weitergegeben wird.

Wenn der Rückruf aufgerufen wird, wenn die `NSError` ist nicht `null` wurde ein Fehler, die verarbeitet werden müssen. Da die Spracherkennung inkrementell erfolgt, die Rückrufroutine aufgerufen werden mehr als einmal, sodass der `SFSpeechRecognitionResult.Final` Eigenschaft wird getestet, um festzustellen, ob die Übersetzung abgeschlossen ist und beste Version der Übersetzung geschrieben (`BestTranscription`).

### <a name="recognizing-live-speech"></a>Erkennen von Live-Sprache

Wenn möchte, dass die app live Spracherkennung, ist das Erkennen von aufgezeichnete Spracherkennung sehr ähnlich. Zum Beispiel:

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

Betrachten diesen Code im Detail, werden mehrere private Variablen zum Behandeln der Erkennungsvorgang erstellt:

```csharp
private AVAudioEngine AudioEngine = new AVAudioEngine ();
private SFSpeechRecognizer SpeechRecognizer = new SFSpeechRecognizer ();
private SFSpeechAudioBufferRecognitionRequest LiveSpeechRequest = new SFSpeechAudioBufferRecognitionRequest ();
private SFSpeechRecognitionTask RecognitionTask;
```

AV-Foundation verwendet, um Audio aufzeichnen, die zu übergebenden ein `SFSpeechAudioBufferRecognitionRequest` zur Verarbeitung der Anforderung Erkennung:

```csharp
var node = AudioEngine.InputNode;
var recordingFormat = node.GetBusOutputFormat (0);
node.InstallTapOnBus (0, 1024, recordingFormat, (AVAudioPcmBuffer buffer, AVAudioTime when) => {
    // Append buffer to recognition request
    LiveSpeechRequest.Append (buffer);
});
```

Die app wird zusammen mit dem Aufzeichnen und alle Fehler werden behandelt, wenn die Aufzeichnung nicht gestartet werden kann:

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

Die Erkennung Aufgabe gestartet wird und ein Handle für den Task Anerkennung beibehalten (`SFSpeechRecognitionTask`):

```csharp
RecognitionTask = SpeechRecognizer.GetRecognitionTask (LiveSpeechRequest, (SFSpeechRecognitionResult result, NSError err) => {
    ...
});
```

Der Rückruf wird in ähnlicher Weise wie oben auf aufgezeichnete Spracherkennung verwendet verwendet.

Wenn Aufzeichnung vom Benutzer angehalten wird, werden sowohl das Audio-Modul und der Sprache Recognition Request informiert:

```csharp
AudioEngine.Stop ();
LiveSpeechRequest.EndAudio ();
```

Wenn der Benutzer Recognition abbricht, werden die Audio-Modul und die Erkennung Aufgaben informiert:

```csharp
AudioEngine.Stop ();
RecognitionTask.Cancel ();
```

Es ist wichtig, rufen Sie `RecognitionTask.Cancel` Wenn der Benutzer die Verschiebung Freigeben von Arbeitsspeicher und geräteprozessors abbricht.

> [!IMPORTANT]
> Fehler beim Bereitstellen der `NSSpeechRecognitionUsageDescription` oder `NSMicrophoneUsageDescription` `Info.plist` Schlüssel können dazu führen, die app ohne Warnung fehl, beim Versuch, den Zugriff auf die Spracherkennung oder das Mikrofon für live-Audio (`var node = AudioEngine.InputNode;`). Finden Sie unter der **Bereitstellen einer Beschreibung der Verwendung** im Abschnitt oben für Weitere Informationen.

## <a name="speech-recognition-limits"></a>Spracherkennung Recognition Grenzwerte

Apple gelten die folgenden Einschränkungen bei der Arbeit mit Spracherkennung in einer iOS-app:

- Spracherkennung ist kostenlos für alle apps, aber seine Verwendung ist nicht unbegrenzt:
    - Einzelne iOS-Geräte haben eine begrenzte Anzahl von Anerkennungen, die pro Tag ausgeführt werden können.
    - Apps werden global auf eine Anforderung pro Tag Basis gedrosselt werden.
- Die app muss für die Spracherkennung Netzwerkverbindung und Nutzung Rate Grenzwert Fehler behandeln vorbereitet werden.
- Spracherkennung kann eine große Aufwand für das schnelle entladen und erhöhtem Netzwerkverkehr auf iOS-Gerät des Benutzers aus diesem Grund, Apple begrenzt eine strikte audio Dauer von ungefähr einer Minute Spracherkennung max.

Wenn eine app routinemäßig seine Grenzen Rate-Einschränkung aktiviert ist, fordert Apple an, dass der Entwickler diese wenden Sie sich an.

## <a name="privacy-and-usability-considerations"></a>Datenschutz und Verwendbarkeit Überlegungen

Apple hat den folgenden Vorschlag für transparent und ressourcenbezogene die Privatsphäre des Benutzers, wenn in einer iOS-app Spracherkennung einschließlich:

- Bei der Sprache des Benutzers aufzeichnen möchten, achten Sie darauf, dass Sie eindeutig angegeben, dass die Aufzeichnung in der app-Benutzeroberfläche ausgeführt wird. Die app kann z. B. eine "Aufzeichnen" sound wiedergeben und Aufzeichnung Indikator anzeigen.
- Verwenden Sie keine Spracherkennung für vertrauliche Benutzerinformationen wie Kennwörter, Health Daten oder Finanzdaten.
- Anzeigen der Erkennungsergebnisse _vor_ darauf fungiert. Dies bietet nicht nur die Rückmeldung über welche die app wird auf diese Weise, aber ermöglicht es dem Benutzer um Erkennungsfehler zu behandeln, wie sie festgelegt wurden.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel wurde der neuen API zum Sprache angezeigt und wurde gezeigt, wie in einer Xamarin.iOS-app fortlaufende Spracherkennung unterstützen und transkribieren Spracherkennung (von Livedaten oder aufgezeichnete Audiodatenströme) in Text zu implementieren. 



## <a name="related-links"></a>Verwandte Links

- [SpeakToMe (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios10/SpeakToMe/)
