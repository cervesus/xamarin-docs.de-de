---
title: Android Audio
description: Android-Betriebssystems bietet umfangreiche Unterstützung für Multimedia, umfasst, Audio und Video verwendet wird. Dieser Leitfaden konzentriert sich auf Audiodaten in Android und deckt wiedergeben und Aufzeichnen von Audio mithilfe der integrierten Audioplayer und Recorder Klassen als auch die Low-Level-audio-API. Hierin sind auch arbeiten mit Audio-Ereignissen, die von anderen Anwendungen übertragen werden, damit Entwickler kaum Anwendungen erstellen können.
ms.prod: xamarin
ms.assetid: 646ED563-C34E-256D-4B56-29EE99881C27
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/28/2018
ms.openlocfilehash: a1a9dd06fb3cd6899dd3a564072bb63e413edf22
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57667550"
---
# <a name="android-audio"></a>Android Audio

_Android-Betriebssystems bietet umfangreiche Unterstützung für Multimedia, umfasst, Audio und Video verwendet wird. Dieser Leitfaden konzentriert sich auf Audiodaten in Android und deckt wiedergeben und Aufzeichnen von Audio mithilfe der integrierten Audioplayer und Recorder Klassen als auch die Low-Level-audio-API. Hierin sind auch arbeiten mit Audio-Ereignissen, die von anderen Anwendungen übertragen werden, damit Entwickler kaum Anwendungen erstellen können._


## <a name="overview"></a>Übersicht

Moderne mobile Geräte, Funktionen, die früher dedizierte Ausrüstungsteile erfordert hätten gilt &ndash; Kameras, Musikplayer und video Recorder. Aus diesem Grund sind multimedia-Frameworks ein erstklassiges Feature mobile APIs geworden.

Android bietet umfangreiche Unterstützung für Multimedia. In diesem Artikel untersucht die Arbeit mit Audio auf Android und umfasst die folgenden Themen

1.  **Wiedergeben von Audiodaten mit Media Player** &ndash; mithilfe der integrierten `MediaPlayer` Klasse zum Abspielen von Audiodateien, einschließlich lokalen Audiodateien und gestreamte Audiodateien mit der `AudioTrack` Klasse.

2.  **Aufzeichnen von Audio** &ndash; mithilfe der integrierten `MediaRecorder` Klasse Audio aufzeichnen.

3.  **Arbeiten mit Audio Benachrichtigungen** &ndash; mit audio Benachrichtigungen um kaum Anwendungen zu erstellen, die ordnungsgemäß auf Ereignisse (z. B. eingehende Anrufe reagieren) durch das Anhalten oder die Audioausgaben Abbrechen.

4.  **Arbeiten mit Audio auf niedriger Ebene** &ndash; Wiedergabe von audio mithilfe der `AudioTrack` Klasse, indem Sie direkt in Arbeitsspeicher geschrieben. Aufzeichnen von audio mithilfe der `AudioRecord` Klasse und direkt aus dem Arbeitsspeicher zu lesen.


## <a name="requirements"></a>Anforderungen

Für diesen Leitfaden benötigen Android 2.0 (API-Ebene-5) oder höher. Beachten Sie, dass Audio auf Android-Debuggen auf einem Gerät erfolgen muss.

Es ist erforderlich, die Anforderung der `RECORD_AUDIO` Berechtigungen in **"androidmanifest.xml"**:

![Erforderliche Abschnitt "Berechtigungen" des Android-Manifest mit Datensatz\_AUDIO aktiviert](android-audio-images/image01.png)



## <a name="playing-audio-with-the-mediaplayer-class"></a>Wiedergeben von Audiodaten mit der MediaPlayer-Klasse

Die einfachste Möglichkeit zum Abspielen von Audiodateien in Android ist mit der integrierten [MediaPlayer](https://developer.xamarin.com/api/type/Android.Media.MediaPlayer/) Klasse.
`MediaPlayer` durch die Übergabe im Dateipfad können lokale oder remote-Dateien wiedergeben werden. Allerdings `MediaPlayer` ist sehr Zustand aktiviert und Aufrufen einer der Methoden im falschen Zustand führt dazu, dass eine Ausnahme ausgelöst werden. Es ist wichtig für die Interaktion mit `MediaPlayer` in der Reihenfolge, unten, um Fehler zu vermeiden.



### <a name="initializing-and-playing"></a>Initialisieren und Wiedergabe

Die Wiedergabe von Audiodaten mit `MediaPlayer` erfordert die folgende Sequenz:

1. Instanziieren Sie ein neues [MediaPlayer](https://developer.xamarin.com/api/type/Android.Media.MediaPlayer/) Objekt.

1. Konfigurieren Sie die Datei zur Wiedergabe über die [SetDataSource](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.SetDataSource/p/Java.IO.FileDescriptor/) Methode.

1. Rufen Sie die [vorbereiten](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Prepare/) Methode zum Initialisieren des Players.

1. Rufen Sie die [starten](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Start/) Methode zum Starten der audio wiedergeben.


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


### <a name="suspending-and-resuming-playback"></a>Das Anhalten und Fortsetzen der Wiedergabe

Die Wiedergabe kann angehalten werden, durch den Aufruf der [anhalten](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Pause%28%29/) Methode:

```csharp
player.Pause();
```

Rufen Sie zum Fortsetzen angehaltener Wiedergabe der [starten](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Start%28%29/) Methode.
Dies wird von der angehaltenen Position in die Wiedergabe fortgesetzt:

```csharp
player.Start();
```

Aufrufen der [beenden](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Stop%28%29/) Methode über den Player beendet eine laufende Wiedergabe:

```csharp
player.Stop();
```

Wenn der Spieler nicht mehr benötigt wird, müssen die Ressourcen freigegeben werden, durch den Aufruf der [Version](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Release%28%29/) Methode:

```csharp
player.Release();
```



## <a name="using-the-mediarecorder-class-to-record-audio"></a>Verwenden der MediaRecorder-Klasse zum Aufzeichnen von Audiodaten

Die logische Konsequenz `MediaPlayer` für die Aufzeichnung von Audiodaten in Android ist die [MediaRecorder](https://developer.xamarin.com/api/type/Android.Media.MediaRecorder/) Klasse. Wie die `MediaPlayer`, dabei handelt es sich Akzent Zustand geht durchlaufen mehrere Zustände, um den Punkt zu erhalten, in dem sie die Aufzeichnung starten kann. Zum Aufzeichnen von Audio, das `RECORD_AUDIO` Berechtigung muss festgelegt sein. Anweisungen zum Festlegen der Anwendung von Berechtigungen finden Sie unter [arbeiten mit der Datei "androidmanifest.xml"](~/android/platform/android-manifest.md).


### <a name="initializing-and-recording"></a>Initialisieren und aufzeichnen

Aufzeichnen von Audio, mit der `MediaRecorder` erfordert die folgenden Schritte aus:

1. Instanziieren Sie ein neues [MediaRecorder](https://developer.xamarin.com/api/type/Android.Media.MediaRecorder/) Objekt.

2. Geben Sie die Hardwaregerät zum Erfassen der Audioeingabe, über die [SetAudioSource](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetAudioSource/p/Android.Media.AudioSource/) Methode.

3. Legen Sie die Ausgabe Audioformat mithilfe der [SetOutputFormat](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetOutputFormat/p/Android.Media.OutputFormat/) Methode. Eine Liste der unterstützten audio-Typen finden Sie unter [Android unterstützt Medium formatiert](https://developer.android.com/guide/appendix/media-formats.html).

4. Rufen Sie die [SetAudioEncoder](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetAudioEncoder/p/Android.Media.AudioEncoder/) Methode, um das Audio Codierungstyp festzulegen.

5. Rufen Sie die [SetOutputFile](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetOutputFile/p/System.String/) Methode, um den Namen der Ausgabedatei anzugeben, die in die Audiodaten geschrieben werden.

6. Rufen Sie die [vorbereiten](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Prepare%28%29/) Methode, um die Aufzeichnung zu initialisieren.

7. Rufen Sie die [starten](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Start%28%29/) Methode zum Starten der Aufzeichnung.


Das folgende Codebeispiel veranschaulicht diese Sequenz:

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


### <a name="stopping-recording"></a>Beenden der Aufzeichnung

Um die Aufzeichnung zu beenden, rufen die `Stop` Methode für die `MediaRecorder`:

```csharp
recorder.Stop();
```



### <a name="cleaning-up"></a>Bereinigen

Nach der `MediaRecorder` wurde beendet, rufen Sie die [zurücksetzen](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Reset%28%29/) Methode ausgedrückt zurück in den Leerlauf:

```csharp
recorder.Reset();
```

Wenn die `MediaRecorder` ist nicht mehr benötigt, die Ressourcen müssen freigegeben werden durch Aufrufen der [Version](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Release%28%29/) Methode:

```csharp
recorder.Release();
```


## <a name="managing-audio-notifications"></a>Verwalten von Audio-Benachrichtigungen



### <a name="the-audiomanager-class"></a>Die AudioManager-Klasse

Die [AudioManager](https://developer.xamarin.com/api/type/Android.Media.AudioManager/) Klasse ermöglicht den Zugriff auf Audio-Benachrichtigungen, mit denen Anwendungen, die wissen, wann Audioereignisse ausgeführt. Dieser Dienst bietet auch Zugriff auf andere audio Funktionen, z. B. Volume und Ringer-Modus-Steuerelement. Die `AudioManager` ermöglicht eine Anwendung zur Handhabung von audio Benachrichtigungen Audiowiedergabe steuern.



### <a name="managing-audio-focus"></a>Verwalten von Audio-Fokus

Die Audio-Ressourcen des Geräts (integrierte Player und Recorder) werden von allen ausgeführten Anwendungen freigegeben.

Dies ist vom Konzept her vergleichbar mit Anwendungen auf einem Desktopcomputer, in denen nur eine Anwendung den Tastaturfokus hat: nach der Auswahl einer der ausgeführten Anwendungen von der Maus klicken, wird die Tastatureingabe nur für die Anwendung wechselt.

Audio liegt der Fokus einen ähnlichen Ansatz und verhindert, dass mehr als eine Anwendung-Gerät zur gleichen Zeit. Es ist komplizierter als über den Tastaturfokus, da sie freiwillig ist &ndash; die Anwendung kann den Umstand, dass es derzeit nicht audio den Fokus besitzt und, unabhängig davon, ob wiedergeben ignorieren &ndash; und es gibt verschiedene Typen von audio konzentrieren, die sein können angefordert. Wenn der anforderer nur zum Abspielen von Audiodateien für sehr kurze Zeit erwartet wird, kann es z. B. vorübergehend den Fokus anfordern.

Audio Fokus möglicherweise sofort, erteilt oder anfänglich verweigert und später gewährt werden. Wenn eine Anwendung Anforderungen audio Fokus beim tätigen des Anrufs, z. B. wird abgelehnt, aber den Fokus kann auch nach Abschluss des Anrufs erteilt werden. In diesem Fall wird ein Listener registriert, um entsprechend reagieren kann, wenn audio Fokus sofort ausgeführt wird. Anfordern von audio Fokus wird verwendet, um zu bestimmen, ob sie OK zum Wiedergeben oder Audio aufzeichnen.

Weitere Informationen zu audio konzentrieren, finden Sie unter [Audio Fokus verwalten](https://developer.android.com/training/managing-audio/audio-focus.html).



#### <a name="registering-the-callback-for-audio-focus"></a>Registrieren des Rückrufs für Audio-Fokus

Registrieren der `FocusChangeListener` Rückruf von der `IOnAudioChangeListener` ist ein wichtiger Teil der abrufen und Freigeben von audio Fokus. Dies ist, da die Gewährung von audio Fokus bis zu einem späteren Zeitpunkt verzögert werden kann. Beispielsweise kann eine Anwendung anfordern, Musik spielen, es gibt zwar ein Anruf ausgeführt. Audio Fokus wird nicht gewährt werden, bis der Anruf beendet wird.

Aus diesem Grund wird die Callback-Objekt übergeben, als Parameter in der `GetAudioFocus` Methode der `AudioManager`, und dieser Aufruf, der den Rückruf registriert werden kann. Wenn audio Fokus zunächst verweigert jedoch später gewährt, wird die Anwendung durch den Aufruf informiert `OnAudioFocusChange` des Rückrufs. Die gleiche Methode wird verwendet, um der Anwendung mitzuteilen, dass audio Fokus sofort aufgenommen wird.

Wenn die Anwendung mithilfe der audio Ressourcen abgeschlossen ist, ruft er die `AbandonFocus` -Methode der der `AudioManager`, und übergibt erneut in der Rückruffunktion. Dies hebt die Registrierung des Rückrufs und gibt die audio-Ressourcen frei, sodass andere Anwendungen audio den Fokus erhalten können.



#### <a name="requesting-audio-focus"></a>Anfordern von Audio-Fokus

Die erforderlichen Schritte für die Anforderung der audio von Ressourcen des Geräts lautet wie folgt:

1.  Abrufen eines Handles für die `AudioManager` -Systemdienst.

2.  Erstellen Sie eine Instanz der Rückrufklasse.

3.  Anfordern, die audio-Ressourcen des Geräts durch Aufrufen der `RequestAudioFocus` Methode für die `AudioManager` . Die Parameter sind das Rückrufobjekt an, den Datenstromtyp (Musik, Sprachanruf, Ring usw.) und den Typ des Zugriffs direkt angefordert wird (die audio-Ressourcen können angefordert werden vorübergehend oder für eine unbestimmte Zeit, z. B.).

4.  Wenn die Anforderung gewährt wird, die `playMusic` Methode wird sofort aufgerufen, und die Audiodaten zur Wiedergabe startet.

5.  Wenn die Anforderung abgelehnt wird, ist keine weitere Aktion ausgeführt. In diesem Fall werden nur das Audio wiedergegeben, wenn die Anforderung zu einem späteren Zeitpunkt zugewiesen wurde.


Das folgende Codebeispiel zeigt die folgenden Schritte aus:

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


#### <a name="releasing-audio-focus"></a>Freigeben von Audio-Fokus

Nach Abschluss die Wiedergabe des Titels, der `AbandonFocus` Methode `AudioManager` aufgerufen wird. Dadurch wird eine andere Anwendung, die audio Ressourcen des Geräts zu erhalten. Andere Anwendungen erhalten eine Benachrichtigung über diese fokusänderung audio, wenn sie ihre eigenen Listener registriert haben.


## <a name="low-level-audio-api"></a>Niedrige Ebene Webaudio-API

Die Low-Level-audio-APIs bieten eine größere Kontrolle über audio wiedergeben und aufzeichnen, da sie direkt mit der Arbeitsspeicherpuffer anstelle Datei-URIs interagieren. Es gibt einige Szenarien, in dem dieser Ansatz besser ist. Zu diesen Szenarien zählen:

1.  Beim Wiedergeben von Audiodateien verschlüsselt.

2.  Wenn Sie eine Folge von kurzclips wiedergeben zu können.

3.  Audio-streaming.


### <a name="audiotrack-class"></a>"Audiotrack"-Klasse

Die ["Audiotrack"](https://developer.xamarin.com/api/type/Android.Media.AudioTrack/) Klasse verwendet die Low-Level-audio-APIs für die Aufzeichnung und Low-Level entspricht der der `MediaPlayer` Klasse.


#### <a name="initializing-and-playing"></a>Initialisieren und Wiedergabe

Zur Wiedergabe von Audio, eine neue Instanz der `AudioTrack` muss instanziiert werden. Die Argumentliste übergeben wird, in der [Konstruktor](https://developer.xamarin.com/api/type/Android.Media.AudioTrack/#memberlist) gibt an, wie die im Puffer enthaltene audioabtastwert wiedergeben. Die Argumente sind:

1.  Stream Typ &ndash; Voice "," Klingelton "," Musik "," System "oder" Alarm.

2.  Häufigkeit &ndash; den Stichproben-Prozentsatz, ausgedrückt in Hz.

3.  Channel-Konfiguration &ndash; Mono oder Stereo.

4.  Audioformat &ndash; 8-Bit oder 16-Bit-Codierung.

5.  Puffergröße &ndash; in Byte.

6.  Puffermodus &ndash; streaming oder statisch.


Nach der Erstellung die [spielen](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Play%28%29/) -Methode der `AudioTrack` wird aufgerufen, um sie bis zur Wiedergabe von Start festgelegt. Den audio-Puffer zum Schreiben der `AudioTrack` startet die Wiedergabe:

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

Rufen Sie die [anhalten](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Pause%28%29/) Methode, um die Wiedergabe anhalten:

```csharp
audioTrack.Pause();
```

Aufrufen der [beenden](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Stop%28%29/) Methode wird die Wiedergabe dauerhaft beendet:

```csharp
audioTrack.Stop();
```


#### <a name="cleanup"></a>Bereinigen

Wenn die `AudioTrack` ist nicht mehr benötigt, die Ressourcen müssen freigegeben werden durch Aufrufen von [Version](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Release%28%29/):

```csharp
audioTrack.Release();
```


### <a name="the-audiorecord-class"></a>Die AudioRecord-Klasse

Die [AudioRecord](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/) Klasse entspricht der `AudioTrack` auf der Seite für die Aufzeichnung. Wie `AudioTrack`, sie Arbeitsspeicher direkt verwendet, anstelle von Dateien und URIs. Es erfordert, dass die `RECORD_AUDIO` Berechtigungen im Manifest festgelegt werden.


#### <a name="initializing-and-recording"></a>Initialisieren und aufzeichnen

Der erste Schritt ist, um neue [AudioRecord](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/) Objekt. Die Argumentliste übergeben wird, in der [Konstruktor](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/#memberlist) bietet alle Informationen, die für die Aufzeichnung erforderlich sind. Im Gegensatz zu in `AudioTrack`, wobei sich die Argumente zum größten Teil Enumerationen, die entsprechenden Argumente in `AudioRecord` ganze Zahlen sind. Dazu gehören:

1.  Hardware Audioeingabequelle wie z. B. Mikrofon.

2.  Stream Typ &ndash; Voice "," Klingelton "," Musik "," System "oder" Alarm.

3.  Häufigkeit &ndash; den Stichproben-Prozentsatz, ausgedrückt in Hz.

4.  Channel-Konfiguration &ndash; Mono oder Stereo.

5.  Audioformat &ndash; 8-Bit oder 16-Bit-Codierung.

6.  Puffer-Größe in bytes


Sobald die `AudioRecord` erstellt wird, seine [StartRecording](https://developer.xamarin.com/api/member/Android.Media.AudioRecord.StartRecording%28%29/) -Methode wird aufgerufen. Es ist jetzt möchten Sie Aufzeichnung zu beginnen. Die `AudioRecord` kontinuierlich die audio-Puffer für die Eingabe liest und schreibt diese Eingabe eine Audiodatei.

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


#### <a name="stopping-the-recording"></a>Das Beenden der Aufzeichnung

Aufrufen der [beenden](https://developer.xamarin.com/api/member/Android.Media.AudioRecord.Stop%28%29/) Methode beendet die Aufzeichnung:

```csharp
audRecorder.Stop();
```


#### <a name="cleanup"></a>Bereinigen

Bei der `AudioRecord` Objekt nicht mehr benötigt wird, Aufrufen der [Version](https://developer.xamarin.com/api/member/Android.Media.AudioRecord.Release%28%29/) Methode gibt alle zugeordnete Ressourcen frei:

```csharp
audRecorder.Release();
```


## <a name="summary"></a>Zusammenfassung

Android-Betriebssystems bietet ein leistungsfähiges Framework für die Wiedergabe aufzeichnen und Verwaltung der Audio. In diesem Artikel erläutert, wie wiedergegeben und Audio aufzeichnen, die mit der allgemeinen `MediaPlayer` und `MediaRecorder` Klassen. Anschließend haben sie, wie Sie audio Benachrichtigungen zu verwenden, um das audio Ressourcen des Geräts zwischen verschiedenen Anwendungen freizugeben. Schließlich behandelt ihn wie Wiedergabe und Audio aufzeichnen, die über die Low-Level-APIs, die direkt mit Speicherpuffer Schnittstelle.


## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit Audio (Beispiel)](https://developer.xamarin.com/samples/Example_WorkingWithAudio/)
- [MediaPlayer](https://developer.xamarin.com/api/type/Android.Media.MediaPlayer/)
- [Medien-Aufzeichnung](https://developer.xamarin.com/api/type/Android.Media.MediaRecorder/)
- [Audio Manager](https://developer.xamarin.com/api/type/Android.Media.AudioManager/)
- [Audiospur](https://developer.xamarin.com/api/type/Android.Media.AudioTrack/)
- [Audio Recorder](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/)
