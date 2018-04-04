---
title: Android Audio
description: Android-Betriebssysteme bietet umfangreiche Unterstützung für Multimedia, Audio und Video umfasst. Dieses Handbuch konzentriert sich auf Audio in Android und wiedergeben und Aufzeichnen von Audio mithilfe der integrierten Audioplayer und Recorder Klassen sowie die Low-Level audio-API behandelt. Außerdem werden behandelt, arbeiten mit Audio-Ereignissen, die von anderen Anwendungen übertragen, damit Entwickler gut konzipierte Anwendungen erstellen können.
ms.prod: xamarin
ms.assetid: 646ED563-C34E-256D-4B56-29EE99881C27
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/28/2018
ms.openlocfilehash: aff0d67549707129bfc85246318c33c522e4f1f6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="android-audio"></a>Android Audio

_Android-Betriebssysteme bietet umfangreiche Unterstützung für Multimedia, Audio und Video umfasst. Dieses Handbuch konzentriert sich auf Audio in Android und wiedergeben und Aufzeichnen von Audio mithilfe der integrierten Audioplayer und Recorder Klassen sowie die Low-Level audio-API behandelt. Außerdem werden behandelt, arbeiten mit Audio-Ereignissen, die von anderen Anwendungen übertragen, damit Entwickler gut konzipierte Anwendungen erstellen können._


## <a name="overview"></a>Übersicht

Moderne mobile Geräte implementiert haben Funktionen, die früher dedizierten Teile der Ausrüstung erfordert hätten &ndash; Kameras, Musikplayer und Videorecorder. Aus diesem Grund haben multimedia-Frameworks ein erstklassiges Feature in mobile-APIs werden.

Android bietet umfangreiche Unterstützung für Multimedia. In diesem Artikel untersucht arbeiten mit Audio in Android und umfasst die folgenden Themen

1.  **Wiedergeben von Audio mit MediaPlayer** &ndash; unter Verwendung der integrierten `MediaPlayer` Klasse zur Wiedergabe von Audiodateien, einschließlich lokaler Audiodateien und gestreamte audio-Dateien mit der `AudioTrack` Klasse.

2.  **Aufzeichnen von Audio** &ndash; unter Verwendung der integrierten `MediaRecorder` Klasse Audio aufzeichnen.

3.  **Arbeiten mit Audio Benachrichtigungen** &ndash; mit audio Benachrichtigungen gut konzipierte Anwendungen erstellen, die ordnungsgemäß auf Ereignisse (z. B. eingehende Anrufe reagieren) durch das Anhalten oder deren audio Ausgaben Abbrechen.

4.  **Arbeiten mit Audio auf niedriger Ebene** &ndash; wiedergeben audio mithilfe der `AudioTrack` Klasse, indem Sie direkt auf Speicherpuffer schreiben. Aufzeichnen von audio mithilfe der `AudioRecord` Klasse und das direkte Lesen aus Speicherpuffern.


## <a name="requirements"></a>Anforderungen

Dieses Handbuch erfordert Android 2.0 (API-Ebene 5) oder höher. Bitte beachten Sie, dass Audio auf Android-Geräten Debuggen auf einem Gerät erfolgen muss.

Es ist erforderlich, die Anforderung der `RECORD_AUDIO` Berechtigungen in **AndroidManifest.XML**:

![Abschnitt "Berechtigungen" der Android-Manifest mit Datensatz erforderlich\_AUDIO aktiviert](android-audio-images/image01.png)



## <a name="playing-audio-with-the-mediaplayer-class"></a>Wiedergeben von Audio mit der Media Player-Klasse

Die einfachste Möglichkeit für die Audiowiedergabe in Android ist mit der integrierten [MediaPlayer](https://developer.xamarin.com/api/type/Android.Media.MediaPlayer/) Klasse.
`MediaPlayer` kann entweder lokale oder remote-Dateien durch die Übergabe des Dateipfads spielen. Allerdings `MediaPlayer` ist sehr Zustand Akzent und Aufrufen einer der Methoden im falschen Zustand führt dazu, dass eine Ausnahme ausgelöst werden. Es ist wichtig für die Interaktion mit `MediaPlayer` in der Reihenfolge, die unten beschriebenen, um Fehler zu vermeiden.



### <a name="initializing-and-playing"></a>Initialisieren und beim Spielen

Audiowiedergabe mit `MediaPlayer` erfordert die folgende Sequenz:

1. Instanziieren Sie ein neues [MediaPlayer](https://developer.xamarin.com/api/type/Android.Media.MediaPlayer/) Objekt.

1. Konfigurieren Sie die Datei über die [SetDataSource](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.SetDataSource/p/Java.IO.FileDescriptor/) Methode.

1. Rufen Sie die [vorbereiten](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Prepare/) Methode, um dem Spieler zu initialisieren.

1. Rufen Sie die [starten](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Start/) -Methode zum Starten der audio wiedergegeben.


Das folgende Codebeispiel veranschaulicht diese Syntax:

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

Die Wiedergabe kann angehalten werden, durch Aufrufen der [anhalten](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Pause%28%29/) Methode:

```csharp
player.Pause();
```

Rufen Sie zum Fortsetzen von angehaltenen Wiedergabe der [starten](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Start%28%29/) Methode.
Dies wird vom angehaltenen Speicherort in die Wiedergabe fortgesetzt:

```csharp
player.Start();
```

Aufrufen der [beenden](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Stop%28%29/) Methode über den Player beendet eine laufende Wiedergabe:

```csharp
player.Stop();
```

Wenn der Spieler nicht mehr benötigt wird, müssen die Ressourcen freigegeben werden, durch Aufrufen der [Version](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Release%28%29/) Methode:

```csharp
player.Release();
```



## <a name="using-the-mediarecorder-class-to-record-audio"></a>Verwenden der MediaRecorder-Klasse, um Audio aufzeichnen

Die begleitende auf `MediaPlayer` für Audioaufnahme in Android ist die [MediaRecorder](https://developer.xamarin.com/api/type/Android.Media.MediaRecorder/) Klasse. Wie die `MediaPlayer`, ist der Status Akzent und geht durchlaufen mehrere Zustände, um den Punkt zu erhalten, in dem sie die Aufzeichnung starten kann. Um Audiodateien Aufzeichnen der `RECORD_AUDIO` Berechtigung muss festgelegt sein. Anweisungen zum Festlegen der Anwendung Berechtigungen finden Sie unter [arbeiten mit AndroidManifest.xml](~/android/platform/android-manifest.md).


### <a name="initializing-and-recording"></a>Initialisieren und aufzeichnen

Aufzeichnen von Audio mit der `MediaRecorder` erfordert die folgenden Schritte aus:

1. Instanziieren Sie ein neues [MediaRecorder](https://developer.xamarin.com/api/type/Android.Media.MediaRecorder/) Objekt.

2. Geben Sie die Hardwaregerät zum Erfassen der Audioeingabe über die [SetAudioSource](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetAudioSource/p/Android.Media.AudioSource/) Methode.

3. Legen Sie die Ausgabe Audioformat mit der [SetOutputFormat](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetOutputFormat/p/Android.Media.OutputFormat/) Methode. Eine Liste der unterstützten audio Typen finden Sie unter [Android unterstützt Medium formatiert](http://developer.android.com/guide/appendix/media-formats.html).

4. Rufen Sie die [SetAudioEncoder](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetAudioEncoder/p/Android.Media.AudioEncoder/) Methode, um das Audio Codierungstyp einzurichten.

5. Rufen Sie die [SetOutputFile](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetOutputFile/p/System.String/) Methode, um den Namen der Ausgabedatei anzugeben, die in die Audiodaten geschrieben werden.

6. Rufen Sie die [vorbereiten](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Prepare%28%29/) Methode, um die Aufzeichnung zu initialisieren.

7. Rufen Sie die [starten](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Start%28%29/) Methode zum Starten der Aufzeichnung.


Im folgenden Codebeispiel veranschaulicht diese Reihenfolge:

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

Einmal die `MediaRecorder` wurde beendet, rufen Sie die [zurücksetzen](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Reset%28%29/) Methode, um dies in Leerlauf zurück:

```csharp
recorder.Reset();
```

Wenn die `MediaRecorder` wird nicht mehr benötigt, die Ressourcen müssen freigegeben werden durch Aufrufen der [Version](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Release%28%29/) Methode:

```csharp
recorder.Release();
```


## <a name="managing-audio-notifications"></a>Audio Benachrichtigungen verwalten



### <a name="the-audiomanager-class"></a>Die AudioManager-Klasse

Die [AudioManager](https://developer.xamarin.com/api/type/Android.Media.AudioManager/) Klasse ermöglicht den Zugriff auf Audio-Benachrichtigungen, mit denen Anwendungen, die wissen, wann Audioereignisse ausgeführt. Dieser Dienst bietet auch Zugriff auf andere Audio / Funktionen, z. B. Volume und Ringer modussteuerung. Die `AudioManager` ermöglicht eine Anwendung zur Handhabung von audio Benachrichtigungen Audiowiedergabe steuern.



### <a name="managing-audio-focus"></a>Verwalten von Audio Fokus

Das audio Ressourcen des Geräts (integrierte Player und Recorder) werden von allen laufenden Anwendungen gemeinsam genutzt.

Dies ist vom Konzept her vergleichbar mit der Anwendungen auf einem Desktopcomputer, auf dem nur eine Anwendung den Tastaturfokus hat: nach dem Auswählen einer der ausgeführten Anwendungen durch Maus-klicken, wechselt die Tastatureingabe nur für diese Anwendung.

Audio Fokus ist eine ähnliche Idee und verhindert, dass mehr als eine Anwendung mit unterschiedlichen oder Audioaufnahme zur gleichen Zeit. Es ist komplizierter als über den Tastaturfokus, da sie freiwillig ist &ndash; die Anwendung kann diese Tatsache, dass sie derzeit nicht audio den Fokus besitzen und, unabhängig davon wiedergeben ignorieren &ndash; und da es gibt verschiedene Typen von audio Fokus, die sein können angefordert. Beispielsweise kann ein vom Absender nur erwartet wird, für einen sehr kurzen Zeitraum audio wiedergegeben, vorübergehende Fokus anfordern.

Audio Fokus möglicherweise sofort erteilt oder anfänglich verweigert und später erteilt werden. Wenn eine Anwendung Anforderungen audio Fokus beim Anruf, z. B. wird abgelehnt, aber den Fokus auch nach Abschluss des Anrufs gewährt werden kann. In diesem Fall wird ein Listener registriert, um entsprechend zu reagieren, wenn audio Fokus entzogen wird. Anfordern von audio Fokus wird verwendet, um zu bestimmen, ob es OK wiedergegeben oder Audio aufzeichnen.

Weitere Informationen zu audio Fokus, finden Sie unter [Audio Fokus verwalten](http://developer.android.com/training/managing-audio/audio-focus.html).



#### <a name="registering-the-callback-for-audio-focus"></a>Den Rückruf registrieren für Audio Fokus

Registrieren der `FocusChangeListener` -Rückruf von dem `IOnAudioChangeListener` ist ein wichtiger Bestandteil der abrufen und Freigeben von audio Fokus. Dies ist, da das Erteilen von audio Fokus bis zu einem späteren Zeitpunkt verzögert werden kann. Beispielsweise kann eine Anwendung anfordern, Musik wiedergeben, es gibt zwar ein Anruf ausgeführt. Audio Fokus wird bis zum Abschluss des Anrufs nicht erteilt.

Aus diesem Grund wird das Callback-Objekt übergeben, als Parameter in der `GetAudioFocus` Methode der `AudioManager`, und dieser Aufruf, der den Rückruf registriert ist. Wenn audio Fokus wird anfänglich verweigert aber später erteilt wird, wird die Anwendung informiert, durch den Aufruf `OnAudioFocusChange` bei Rückruf. Dieselbe Methode wird verwendet, um der Anwendung mitzuteilen, dass es sich bei audio Fokus sofort vorgenommen wird.

Wenn die Anwendung mithilfe der audio Ressourcen abgeschlossen ist, ruft er die `AbandonFocus` Methode der `AudioManager`, und erneut in den Rückruf übergeben. Hebt die Registrierung des Rückrufs, und das audio Ressourcen frei, sodass andere Anwendungen audio den Fokus erhalten können.



#### <a name="requesting-audio-focus"></a>Anfordern von Audio Fokus

Die erforderlichen Schritte zum Anfordern audio Ressourcen des Geräts lautet wie folgt:

1.  Abrufen eines Handles für die `AudioManager` -Systemdienst.

2.  Erstellen Sie eine Instanz der Rückrufklasse.

3.  Anfordern, dass das audio Ressourcen des Geräts durch Aufrufen der `RequestAudioFocus` Methode für die `AudioManager` . Die Parameter sind das Rückrufobjekt, den Streamtyp (Musik, Anruf, Ring usw.) und den Typ des Zugriffs direkt angefordert wird (audio Ressourcen können angefordert werden vorübergehend oder für eine unbestimmte Zeit, z. B.).

4.  Wenn die Anforderung nicht gewährt wird, die `playMusic` Methode wird sofort aufgerufen, und die Audiodatei zur Wiedergabe wird gestartet.

5.  Wenn die Anforderung abgelehnt wird, ist keine weitere Aktion erforderlich. In diesem Fall wird nur das Audio wiedergegeben, wenn die Anforderung zu einem späteren Zeitpunkt gewährt wird.


Im folgenden Codebeispiel wird gezeigt, folgende Schritte aus:

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


#### <a name="releasing-audio-focus"></a>Freigeben von Audio Fokus

Nach Abschluss die Wiedergabe des Titels der `AbandonFocus` Methode `AudioManager` aufgerufen wird. Dadurch kann es sich um eine andere Anwendung das audio Ressourcen des Geräts zu erhalten. Andere Anwendungen erhalten Sie eine Benachrichtigung dieser Änderung audio Fokus, wenn sie ihre eigenen Listeners registriert haben.


## <a name="low-level-audio-api"></a>Niedrige Ebene Audio-API

Die Low-Level-audio-APIs bieten eine größere Kontrolle über audio wiedergeben und aufzeichnen, da sie direkt mit Speicherpuffer anstelle der Datei URIs interagieren. Es gibt einige Szenarios, in denen dieser Ansatz zu bevorzugen ist. Solche Szenarien gehören:

1.  Beim Wiedergeben von audio-Dateien verschlüsselt.

2.  Bei der Wiedergabe von einer Folge von kurzen zuschneidet.

3.  Audio streaming.


### <a name="audiotrack-class"></a>AudioTrack-Klasse

Die [AudioTrack](https://developer.xamarin.com/api/type/Android.Media.AudioTrack/) Klasse verwendet die Low-Level-audio-APIs für die Aufzeichnung und entspricht dem Low-Level der `MediaPlayer` Klasse.


#### <a name="initializing-and-playing"></a>Initialisieren und beim Spielen

Wiedergeben von Audio, eine neue Instanz der `AudioTrack` instanziiert werden müssen. Die Argumentliste übergeben wird, in der [Konstruktor](https://developer.xamarin.com/api/type/Android.Media.AudioTrack/#memberlist) gibt an, wie die im Puffer enthaltene audioabtastwert wiedergeben. Die Argumente sind:

1.  Streamtyp &ndash; Stimme Klingelton "," Musik "," System "oder" Alarm.

2.  Häufigkeit &ndash; die Abtastrate, ausgedrückt in Hz.

3.  Channel-Konfiguration &ndash; Mono oder Stereo.

4.  Audioformat &ndash; 8-Bit- oder 16-Bit-Codierung.

5.  Puffergröße &ndash; in Bytes.

6.  Puffermodus &ndash; streaming oder statisch.


Nach der Erstellung der [wiedergeben](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Play%28%29/) Methode `AudioTrack` aufgerufen, um es bis zum Beginn Wiedergabe festzulegen. Schreiben des audio Puffers, in dem `AudioTrack` startet die Wiedergabe:

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

Rufen Sie die [anhalten](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Pause%28%29/) Methode, um die Wiedergabe anzuhalten:

```csharp
audioTrack.Pause();
```

Aufrufen der [beenden](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Stop%28%29/) Methode wird die Wiedergabe endgültig beenden:

```csharp
audioTrack.Stop();
```


#### <a name="cleanup"></a>Bereinigung

Wenn die `AudioTrack` wird nicht mehr benötigt, die Ressourcen müssen freigegeben werden durch den Aufruf [Version](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Release%28%29/):

```csharp
audioTrack.Release();
```


### <a name="the-audiorecord-class"></a>Die AudioRecord-Klasse

Die [AudioRecord](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/) Klasse ist die Entsprechung von `AudioTrack` auf der Seite aufzeichnen. Wie `AudioTrack`, es Speicherpuffer direkt verwendet, statt die Dateien und URIs. Es erfordert, dass die `RECORD_AUDIO` Berechtigung im Manifest festgelegt werden.


#### <a name="initializing-and-recording"></a>Initialisieren und aufzeichnen

Der erste Schritt besteht, um neue [AudioRecord](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/) Objekt. Die Argumentliste übergeben wird, in der [Konstruktor](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/#memberlist) enthält die Informationen, die für die Aufzeichnung erforderlich. Im Gegensatz zu in `AudioTrack`, wobei sich die Argumente zum größten Teil Enumerationen, die entsprechenden Argumente in `AudioRecord` sind ganze Zahlen. Dazu gehören:

1.  Hardware audio Eingabequelle z. B. Mikrofon.

2.  Streamtyp &ndash; Stimme Klingelton "," Musik "," System "oder" Alarm.

3.  Häufigkeit &ndash; die Abtastrate, ausgedrückt in Hz.

4.  Channel-Konfiguration &ndash; Mono oder Stereo.

5.  Audioformat &ndash; 8-Bit- oder 16-Bit-Codierung.

6.  Puffer Größe in bytes


Einmal die `AudioRecord` erstellt wird, dessen [StartRecording](https://developer.xamarin.com/api/member/Android.Media.AudioRecord.StartRecording%28%29/) Methode aufgerufen wird. Es ist jetzt bereit, um die Aufzeichnung zu starten. Die `AudioRecord` fortlaufend die audio Puffer für die Eingabe liest und schreibt diese Eingabe für eine Audiodatei.

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


#### <a name="stopping-the-recording"></a>Die Aufzeichnung beenden

Aufrufen der [beenden](https://developer.xamarin.com/api/member/Android.Media.AudioRecord.Stop%28%29/) Methode beendet die Aufzeichnung:

```csharp
audRecorder.Stop();
```


#### <a name="cleanup"></a>Bereinigung

Wenn die `AudioRecord` Objekt nicht mehr benötigt wird, Aufrufen seiner [Version](https://developer.xamarin.com/api/member/Android.Media.AudioRecord.Release%28%29/) Methode alle zugeordnete Ressourcen frei:

```csharp
audRecorder.Release();
```


## <a name="summary"></a>Zusammenfassung

Android-Betriebssysteme bietet einen leistungsfähigen Rahmen für die Wiedergabe aufzeichnen und Verwalten von Audio. In diesem Artikel behandelt, wie abgespielt und Audio aufzeichnen, die mithilfe der allgemeinen `MediaPlayer` und `MediaRecorder` Klassen. Als Nächstes untersucht es wie audio Benachrichtigungen verwenden, um das audio Ressourcen des Geräts zwischen verschiedenen Anwendungen freizugeben. Schließlich behandelt diese wie Wiedergabe und Audio aufzeichnen, die mithilfe der Low-Level-APIs, die direkt mit Speicherpuffer Schnittstelle.


## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit Audio (Beispiel)](https://developer.xamarin.com/samples/Example_WorkingWithAudio/)
- [MediaPlayer](https://developer.xamarin.com/api/type/Android.Media.MediaPlayer/)
- [Medien Recorder](https://developer.xamarin.com/api/type/Android.Media.MediaRecorder/)
- [Audio Manager](https://developer.xamarin.com/api/type/Android.Media.AudioManager/)
- [Audiotitel](https://developer.xamarin.com/api/type/Android.Media.AudioTrack/)
- [Audio-Recorder](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/)
