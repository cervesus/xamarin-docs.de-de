---
title: Android-Audiodatei
description: Das Android-Betriebssystem bietet umfassende Unterstützung für Multimedia und umfasst sowohl Audioinformationen als auch Videos. Dieser Leitfaden konzentriert sich auf das Audiomaterial in Android und deckt das Abspielen und Aufzeichnen von Audiodaten mithilfe der integrierten Audioplayer-und Recorder-Klassen sowie der Low-Level-audioapi ab. Außerdem wird die Arbeit mit Audioereignissen behandelt, die von anderen Anwendungen gesendet werden, damit Entwickler gut verhaltene Anwendungen entwickeln können.
ms.prod: xamarin
ms.assetid: 646ED563-C34E-256D-4B56-29EE99881C27
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/28/2018
ms.openlocfilehash: ac59bcbb062dc9dae784d18054048f50094c2145
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73018068"
---
# <a name="android-audio"></a>Android-Audiodatei

_Das Android-Betriebssystem bietet umfassende Unterstützung für Multimedia und umfasst sowohl Audioinformationen als auch Videos. Dieser Leitfaden konzentriert sich auf das Audiomaterial in Android und deckt das Abspielen und Aufzeichnen von Audiodaten mithilfe der integrierten Audioplayer-und Recorder-Klassen sowie der Low-Level-audioapi ab. Außerdem wird die Arbeit mit Audioereignissen behandelt, die von anderen Anwendungen gesendet werden, damit Entwickler gut verhaltene Anwendungen entwickeln können._

## <a name="overview"></a>Übersicht

Moderne mobile Geräte verfügen über Funktionen, die früher bestimmte Geräte &ndash; Kameras, Musik-und Video Recorder erforderte. Aus diesem Grund wurden Multimedia-Frameworks zu einer erstklassigen Funktion in Mobile APIs.

Android bietet umfassende Unterstützung für Multimedia. In diesem Artikel wird das Arbeiten mit Audiodaten in Android erläutert. Außerdem werden die folgenden Themen behandelt.

1. Wieder **Gabe von Audiodaten mit Media Player** &ndash; mithilfe der integrierten `MediaPlayer`-Klasse zur Wiedergabe von Audiodateien, einschließlich lokaler Audiodateien und streamingaudiodateien mit der `AudioTrack`-Klasse.

2. Aufzeichnen von **Audio&ndash;** mithilfe der integrierten `MediaRecorder`-Klasse, um Audiodaten aufzuzeichnen.

3. **Arbeiten mit Audiobenachrichtigungen** &ndash; mithilfe von Audiobenachrichtigungen, um gut verhaltene Anwendungen zu erstellen, die ordnungsgemäß auf Ereignisse (z. b. eingehende Telefonanrufe) reagieren, indem Sie Ihre Audioausgaben anhalten oder Abbrechen.

4. **Arbeiten mit Low-Level-Audio&ndash;** das Abspielen von Audiodaten mithilfe der `AudioTrack`-Klasse durch direktes Schreiben in Speicherpuffer. Aufzeichnen von Audiodaten mit der `AudioRecord`-Klasse und Lesen von Daten aus Speicher Puffern.

## <a name="requirements"></a>Anforderungen

Diese Anleitung erfordert Android 2,0 (API-Ebene 5) oder höher. Beachten Sie, dass das Debuggen von Audiodaten auf Android auf einem Gerät erfolgen muss.

Es ist erforderlich, die `RECORD_AUDIO` Berechtigungen in " **androidmanifest. XML**" anzufordern:

![Abschnitt "erforderliche Berechtigungen" des Android-Manifests mit aktiviertem Daten Satz\_](android-audio-images/image01.png)

## <a name="playing-audio-with-the-mediaplayer-class"></a>Wiedergabe von Audiodaten mit der Media Player-Klasse

Die einfachste Möglichkeit zur Wiedergabe von Audiodaten in Android ist die integrierte [Media Player](xref:Android.Media.MediaPlayer) -Klasse.
`MediaPlayer` können sowohl lokale als auch Remote Dateien wiedergeben, indem Sie den Dateipfad übergeben. `MediaPlayer` ist jedoch sehr Zustands sensibel, und der Aufruf einer der Methoden im falschen Zustand bewirkt, dass eine Ausnahme ausgelöst wird. Es ist wichtig, mit `MediaPlayer` in der unten beschriebenen Reihenfolge zu interagieren, um Fehler zu vermeiden.

### <a name="initializing-and-playing"></a>Initialisieren und wiedergeben

Die Wiedergabe von Audiodaten mit `MediaPlayer` erfordert die folgende Sequenz:

1. Instanziieren Sie ein neues [Media Player](xref:Android.Media.MediaPlayer) -Objekt.

1. Konfigurieren Sie die Datei, die über die [SetDataSource](xref:Android.Media.MediaPlayer.SetDataSource*) -Methode abgespielt werden soll.

1. Ruft die [Prepare](xref:Android.Media.MediaPlayer.Prepare) -Methode auf, um den Player zu initialisieren.

1. Ruft die [Start](xref:Android.Media.MediaPlayer.Start) -Methode auf, um die Audiowiedergabe zu starten.

Das folgende Codebeispiel veranschaulicht diese Verwendung:

```csharp
protected MediaPlayer player;
public void StartPlayer(String  filePath)
{
  if (player == null) {
    player = new MediaPlayer();
  } else {
    player.Reset();
    player.SetDataSource(filePath);
    player.Prepare();
    player.Start();
  }
}
```

### <a name="suspending-and-resuming-playback"></a>Anhalten und Fortsetzen der Wiedergabe

Die Wiedergabe kann durch Aufrufen der [Pause](xref:Android.Media.MediaPlayer.Pause) -Methode angehalten werden:

```csharp
player.Pause();
```

Um die angehaltene Wiedergabe fortzusetzen, müssen Sie die [Start](xref:Android.Media.MediaPlayer.Start) -Methode
Diese wird an der Wiedergabe an der angehaltenen Position fortgesetzt:

```csharp
player.Start();
```

Durch Aufrufen der Methode " [Beenden](xref:Android.Media.MediaPlayer.Stop) " auf dem Player wird eine laufende Wiedergabe beendet:

```csharp
player.Stop();
```

Wenn der Spieler nicht mehr benötigt wird, müssen die Ressourcen freigegeben werden, indem Sie die [releasemethode](xref:Android.Media.MediaPlayer.Release) aufrufen:

```csharp
player.Release();
```

## <a name="using-the-mediarecorder-class-to-record-audio"></a>Verwenden der mediarecorder-Klasse zum Aufzeichnen von Audiodaten

Die `MediaPlayer` für die Aufzeichnung von Audiodaten in Android ist die [mediarecorder](xref:Android.Media.MediaRecorder) -Klasse. Wie das `MediaPlayer`ist die Zustands sensitiv und durchlaufen mehrere Zustände, um den Punkt zu erreichen, an dem die Aufzeichnung beginnen kann. Um Audiodaten aufzuzeichnen, muss die `RECORD_AUDIO` Berechtigung festgelegt werden. Anweisungen zum Festlegen von Anwendungs Berechtigungen finden Sie unter [Arbeiten mit "androidmanifest. XML](~/android/platform/android-manifest.md)".

### <a name="initializing-and-recording"></a>Initialisieren und aufzeichnen

Zum Aufzeichnen von Audiodaten mit dem `MediaRecorder` sind die folgenden Schritte erforderlich:

1. Instanziieren Sie ein neues [mediarecorder](xref:Android.Media.MediaRecorder) -Objekt.

2. Geben Sie an, welches Hardware Gerät zum Erfassen der Audioeingabe über die [setaudiosource](xref:Android.Media.MediaRecorder.SetAudioSource*) -Methode verwendet werden soll.

3. Legen Sie das Audioformat der Ausgabedatei mithilfe der [setoutputformat](xref:Android.Media.MediaRecorder.SetOutputFormat*) -Methode fest. Eine Liste der unterstützten Audiotypen finden Sie [unter von Android Unterstützte Medienformate](https://developer.android.com/guide/appendix/media-formats.html).

4. Aufrufen der [setaudioencoder](xref:Android.Media.MediaRecorder.SetAudioEncoder*) -Methode, um den audiocodierungstyp festzulegen.

5. Aufrufen der [setoutputfile](xref:Android.Media.MediaRecorder.SetOutputFile*) -Methode, um den Namen der Ausgabedatei anzugeben, in die die Audiodaten geschrieben werden.

6. Ruft die [Prepare](xref:Android.Media.MediaRecorder.Prepare) -Methode auf, um die Aufzeichnung zu initialisieren.

7. Ruft die [Start](xref:Android.Media.MediaRecorder.Start) Methode auf, um die Aufzeichnung zu starten.

Diese Sequenz wird im folgenden Codebeispiel veranschaulicht:

```csharp
protected MediaRecorder recorder;
void RecordAudio (String filePath)
{
  try {
    if (File.Exists (filePath)) {
      File.Delete (filePath);
    }
    if (recorder == null) {
      recorder = new MediaRecorder (); // Initial state.
    } else {
      recorder.Reset ();
      recorder.SetAudioSource (AudioSource.Mic);
      recorder.SetOutputFormat (OutputFormat.ThreeGpp);
      recorder.SetAudioEncoder (AudioEncoder.AmrNb);
      // Initialized state.
      recorder.SetOutputFile (filePath);
      // DataSourceConfigured state.
      recorder.Prepare (); // Prepared state
      recorder.Start (); // Recording state.
    }
  } catch (Exception ex) {
    Console.Out.WriteLine( ex.StackTrace);
  }
}
```

### <a name="stopping-recording"></a>Aufzeichnen wird beendet

Um die Aufzeichnung zu verhindern, müssen Sie die `Stop`-Methode für die `MediaRecorder`abrufen:

```csharp
recorder.Stop();
```

### <a name="cleaning-up"></a>Bereinigen

Nachdem der `MediaRecorder` beendet wurde, wenden Sie die [Reset](xref:Android.Media.MediaRecorder.Reset) -Methode an, um Sie wieder in den Leerlaufzustand zu versetzen:

```csharp
recorder.Reset();
```

Wenn der `MediaRecorder` nicht mehr benötigt wird, müssen seine Ressourcen freigegeben werden, indem Sie die [releasemethode](xref:Android.Media.MediaRecorder.Release) aufrufen:

```csharp
recorder.Release();
```

## <a name="managing-audio-notifications"></a>Verwalten von Audiobenachrichtigungen

### <a name="the-audiomanager-class"></a>Die Audiomanager-Klasse

Die [Audiomanager](xref:Android.Media.AudioManager) -Klasse bietet Zugriff auf Audiobenachrichtigungen, mit denen Anwendungen erkennen können, wann Audioereignisse auftreten. Dieser Dienst bietet auch Zugriff auf andere Audiofeatures, wie z. b. die Steuerung von Volumes und ruftmodus. Der `AudioManager` ermöglicht es einer Anwendung, Audiobenachrichtigungen zu verarbeiten, um die Audiowiedergabe zu steuern.

### <a name="managing-audio-focus"></a>Verwalten des audiofokus

Die Audioressourcen des Geräts (integrierter Player und Recorder) werden von allen laufenden Anwendungen gemeinsam genutzt.

Dies ist konzeptionell vergleichbar mit Anwendungen auf einem Desktop Computer, bei denen nur eine Anwendung den Tastaturfokus hat: Nachdem Sie eine der laufenden Anwendungen ausgewählt haben, indem Sie darauf klicken, wird die Tastatureingabe nur an diese Anwendung weitergeleitet.

Der audiofokus ist eine ähnliche Idee und verhindert, dass mehr als eine Anwendung gleichzeitig Audiodaten abspielen oder aufzeichnen kann. Es ist komplizierter als der Tastaturfokus, da es freiwillig ist, &ndash; die Anwendung diese Tatsache ignorieren kann, dass Sie derzeit keinen audiofokus hat und unabhängig &ndash; und dass verschiedene Arten von audiofokus angefordert werden können. Wenn der Anforderer z. b. nur für einen sehr kurzen Zeitraum die Audiowiedergabe erwartet, kann er vorübergehenden Fokus anfordern.

Der audiofokus kann sofort gewährt werden oder anfänglich verweigert und später erteilt werden. Wenn eine Anwendung z. b. den audiofokus während eines Telefonanrufs anfordert, wird Sie verweigert, aber der Fokus kann auch nach Abschluss des Telefonanrufs gewährt werden. In diesem Fall wird ein Listener registriert, um entsprechend zu reagieren, wenn der audiofokus entfernt wird. Der audiofokus wird angefordert, um zu bestimmen, ob es in Ordnung ist, Audioinhalte wiederzugeben oder aufzuzeichnen.

Weitere Informationen zum audiofokus finden Sie unter [Verwalten des audiofokus](https://developer.android.com/training/managing-audio/audio-focus.html).

#### <a name="registering-the-callback-for-audio-focus"></a>Registrieren des Rückrufs für den audiofokus

Das Registrieren des `FocusChangeListener` Rückrufs vom `IOnAudioChangeListener` ist ein wichtiger Bestandteil der Beschaffung und Freigabe des audiofokus. Dies liegt daran, dass die Gewährung des audiofokus bis zu einem späteren Zeitpunkt verzögert werden kann. Eine Anwendung kann z. b. die Wiedergabe von Musik anfordern, während ein Telefonanruf ausgeführt wird. Der audiofokus wird erst nach Abschluss des Telefonanrufs erteilt.

Aus diesem Grund wird das Rückruf Objekt als Parameter an die `GetAudioFocus`-Methode des `AudioManager`übergeben, und dieser Rückruf registriert den Rückruf. Wenn der audiofokus anfänglich verweigert, aber später gewährt wird, wird die Anwendung durch Aufrufen `OnAudioFocusChange` für den Rückruf informiert. Die gleiche Methode wird verwendet, um der Anwendung mitzuteilen, dass der audiofokus entfernt wird.

Wenn die Anwendung die Audioressourcen nicht mehr verwendet, ruft Sie die `AbandonFocus`-Methode der `AudioManager`auf und übergibt erneut den Rückruf. Dadurch wird der Rückruf aufgehoben, und die Audioressourcen werden freigegeben, sodass andere Anwendungen den audiofokus erhalten können.

#### <a name="requesting-audio-focus"></a>Anfordern des audiofokus

Die erforderlichen Schritte zum Anfordern der Audioressourcen des Geräts lauten wie folgt:

1. Abrufen eines Handles für den `AudioManager` Systemdienst.

2. Erstellen Sie eine Instanz der Rückruf Klasse.

3. Fordern Sie die Audioressourcen des Geräts an, indem Sie die `RequestAudioFocus`-Methode für die `AudioManager` aufrufen. Bei den Parametern handelt es sich um das Rückruf Objekt, den Streamtyp (Musik, Sprachanruf, Ring usw.) und den Typ des angeforderten Zugriffsrechts (z. b. die Audioressourcen können vorübergehend oder für einen unbestimmten Zeitraum angefordert werden).

4. Wenn die Anforderung erteilt wird, wird die `playMusic`-Methode sofort aufgerufen, und die Audiowiedergabe beginnt wieder.

5. Wenn die Anforderung verweigert wird, wird keine weitere Aktion durchgeführt. In diesem Fall wird das Audioformat nur dann wiedergegeben, wenn die Anforderung zu einem späteren Zeitpunkt erteilt wird.

Das folgende Codebeispiel zeigt die folgenden Schritte:

```csharp
Boolean RequestAudioResources(INotificationReceiver parent)
{
  AudioManager audioMan = (AudioManager) GetSystemService(Context.AudioService);
  AudioManager.IOnAudioFocusChangeListener listener  = new MyAudioListener(this);
  var ret = audioMan.RequestAudioFocus (listener, Stream.Music, AudioFocus.Gain );
  if (ret == AudioFocusRequest.Granted) {
    playMusic();
    return (true);
  } else if (ret == AudioFocusRequest.Failed) {
    return (false);
  }
  return (false);
}
```

#### <a name="releasing-audio-focus"></a>Freigeben des audiofokus

Wenn die Wiedergabe des Titels beendet ist, wird die `AbandonFocus`-Methode auf `AudioManager` aufgerufen. Dadurch kann eine andere Anwendung die Audioressourcen des Geräts erhalten. Andere Anwendungen erhalten eine Benachrichtigung über diese audiofokus Änderung, wenn Sie Ihre eigenen Listener registriert haben.

## <a name="low-level-audio-api"></a>Low-Level-audioapi

Die Low-Level-audioapis bieten eine bessere Kontrolle über das Abspielen und Aufzeichnen von Audiodaten, da Sie direkt mit Speicher Puffern interagieren, anstatt Datei-URIs zu verwenden. Es gibt einige Szenarien, in denen dieser Ansatz vorzuziehen ist. Zu diesen Szenarien gehören:

1. Bei der Wiedergabe von verschlüsselten Audiodateien.

2. Wenn eine Abfolge kurzer Clips wiedergegeben wird.

3. Audiostreaming.

### <a name="audiotrack-class"></a>Audiotrack-Klasse

Die [Audiotrack](xref:Android.Media.AudioTrack) -Klasse verwendet die Low-Level-Audio-APIs für die Aufzeichnung und ist die Entsprechung der `MediaPlayer`-Klasse auf niedriger Ebene.

#### <a name="initializing-and-playing"></a>Initialisieren und wiedergeben

Zum Abspielen von Audiodaten muss eine neue Instanz von `AudioTrack` instanziiert werden. Die Argumentliste, die an den [Konstruktor](xref:Android.Media.AudioTrack) übergeben wird, gibt an, wie das im Puffer enthaltene Audiobeispiel wiedergegeben wird. Die Argumente lauten:

1. Streamtyp &ndash; Stimme, Rington, Musik, System oder Alarm.

2. Häufigkeit &ndash; die in Hz ausgedrückte Samplingrate.

3. Kanalkonfigurations &ndash; Mono oder Stereo.

4. Audioformat &ndash; 8-Bit-oder 16-Bit-Codierung.

5. Puffergröße &ndash; in Bytes.

6. Der Puffer Modus &ndash; Streaming oder statisch.

Nach der Erstellung wird die [Play](xref:Android.Media.AudioTrack.Play) -Methode von `AudioTrack` aufgerufen, um die Wiedergabe zu starten. Wenn Sie den Audiopuffer in den `AudioTrack` schreiben, wird die Wiedergabe gestartet:

```csharp
void PlayAudioTrack(byte[] audioBuffer)
{
  AudioTrack audioTrack = new AudioTrack(
    // Stream type
    Stream.Music,
    // Frequency
    11025,
    // Mono or stereo
    ChannelOut.Mono,
    // Audio encoding
    Android.Media.Encoding.Pcm16bit,
    // Length of the audio clip.
    audioBuffer.Length,
    // Mode. Stream or static.
    AudioTrackMode.Stream);

    audioTrack.Play();
    audioTrack.Write(audioBuffer, 0, audioBuffer.Length);
}
```

#### <a name="pausing-and-stopping-the-playback"></a>Anhalten und Beenden der Wiedergabe

[Halten Sie](xref:Android.Media.AudioTrack.Pause) die anhaltemethode an, um die Wiedergabe anzuhalten:

```csharp
audioTrack.Pause();
```

Durch Aufrufen der Methode " [Beenden](xref:Android.Media.AudioTrack.Stop) " wird die Wiedergabe dauerhaft beendet:

```csharp
audioTrack.Stop();
```

#### <a name="cleanup"></a>Bereinigung

Wenn die `AudioTrack` nicht mehr benötigt wird, müssen die zugehörigen Ressourcen durch Aufrufen von [Release](xref:Android.Media.AudioTrack.Release)freigegeben werden:

```csharp
audioTrack.Release();
```

### <a name="the-audiorecord-class"></a>Die audiorecord-Klasse

Die [audiorecord](xref:Android.Media.AudioRecord) -Klasse entspricht `AudioTrack` auf der Aufzeichnungs Seite. Wie `AudioTrack`werden Speicherpuffer direkt anstelle von Dateien und URIs verwendet. Hierfür ist es erforderlich, dass die `RECORD_AUDIO`-Berechtigung im Manifest festgelegt wird.

#### <a name="initializing-and-recording"></a>Initialisieren und aufzeichnen

Der erste Schritt besteht darin, ein neues [audiorecord](xref:Android.Media.AudioRecord) -Objekt zu erstellen. Die Argumentliste, die an den- [Konstruktor](xref:Android.Media.AudioRecord) übergeben wird, stellt alle für die Aufzeichnung erforderlichen Informationen bereit. Anders als bei `AudioTrack`, bei dem es sich bei den Argumenten größtenteils um Enumerationen handelt, sind die entsprechenden Argumente in `AudioRecord` ganze Zahlen. Dazu gehören:

1. Hardware-Audioeingabe Quelle, z. b. Mikrofon.

2. Streamtyp &ndash; Stimme, Rington, Musik, System oder Alarm.

3. Häufigkeit &ndash; die in Hz ausgedrückte Samplingrate.

4. Kanalkonfigurations &ndash; Mono oder Stereo.

5. Audioformat &ndash; 8-Bit-oder 16-Bit-Codierung.

6. Puffergröße in Bytes

Nachdem der `AudioRecord` erstellt wurde, wird seine [StartRecording](xref:Android.Media.AudioRecord.StartRecording) -Methode aufgerufen. Jetzt kann die Aufzeichnung begonnen werden. Der `AudioRecord` liest fortlaufend den Audiopuffer für die Eingabe und schreibt diese Eingabe in eine Audiodatei.

```csharp
void RecordAudio()
{
  byte[] audioBuffer = new byte[100000];
  var audRecorder = new AudioRecord(
    // Hardware source of recording.
    AudioSource.Mic,
    // Frequency
    11025,
    // Mono or stereo
    ChannelIn.Mono,
    // Audio encoding
    Android.Media.Encoding.Pcm16bit,
    // Length of the audio clip.
    audioBuffer.Length
  );
  audRecorder.StartRecording();
  while (true) {
    try
    {
      // Keep reading the buffer while there is audio input.
      audRecorder.Read(audioBuffer, 0, audioBuffer.Length);
      // Write out the audio file.
    } catch (Exception ex) {
      Console.Out.WriteLine(ex.Message);
      break;
    }
  }
}
```

#### <a name="stopping-the-recording"></a>Beenden der Aufzeichnung

Durch Aufrufen der Methode " [Beenden](xref:Android.Media.AudioRecord.Stop) " wird die Aufzeichnung beendet:

```csharp
audRecorder.Stop();
```

#### <a name="cleanup"></a>Bereinigung

Wenn das `AudioRecord` Objekt nicht mehr benötigt wird, gibt der Aufruf seiner [releasemethode](xref:Android.Media.AudioRecord.Release) alle zugeordneten Ressourcen frei:

```csharp
audRecorder.Release();
```

## <a name="summary"></a>Zusammenfassung

Das Android-Betriebssystem bietet ein leistungsfähiges Framework zum Abspielen, aufzeichnen und Verwalten von Audiodaten. In diesem Artikel wurde beschrieben, wie Sie Audiodaten mithilfe der `MediaPlayer`-und `MediaRecorder` Klassen auf hoher Ebene abspielen. Als nächstes wurde untersucht, wie Audiobenachrichtigungen verwendet werden, um die Audioressourcen des Geräts für verschiedene Anwendungen freizugeben. Schließlich wurde die Wiedergabe und Aufzeichnung von Audiodaten mithilfe der Low-Level-APIs behandelt, die direkt mit Speicher Puffern über eine Schnittstelle verfügen.

## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit Audiodaten (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/example-workingwithaudio)
- [Media Player](xref:Android.Media.MediaPlayer)
- [Medien Aufzeichnung](xref:Android.Media.MediaRecorder)
- [Audiomanager](xref:Android.Media.AudioManager)
- [Audiotitel](xref:Android.Media.AudioTrack)
- [Audiorecorder](xref:Android.Media.AudioRecord)
