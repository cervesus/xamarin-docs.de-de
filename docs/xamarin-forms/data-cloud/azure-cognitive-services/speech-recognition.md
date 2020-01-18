---
title: Spracherkennung mithilfe der Sprachdienst-API
description: In diesem Artikel wird erläutert, wie Sie mit der Azure Speech Service-API Sprache in Text in einer xamarin. Forms-Anwendung umwandeln.
ms.prod: xamarin
ms.assetid: B435FF6B-8785-48D9-B2D9-1893F5A87EA1
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 01/14/2020
ms.openlocfilehash: c10b8feea5fbec21fc127262c3f1bfda50beba7f
ms.sourcegitcommit: ba83c107c87b015dbcc9db13964fe111a0573dca
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2020
ms.locfileid: "76265149"
---
# <a name="speech-recognition-using-azure-speech-service"></a>Spracherkennung mit Azure Speech Service

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-cognitivespeechservice)

Der Azure Speech Service ist eine cloudbasierte API, die die folgenden Funktionen bietet:

- **Sprache-zu-Text** -Daten werden Audiodateien oder Streams in Text umgewandelt.
- **Text-zu-Sprache** konvertiert Eingabe Text in Menschen ähnliche Sprache mit synthetischer Sprache.
- Die **Sprachübersetzung** ermöglicht die Echt Zeit übergreifende Übersetzung von Sprache zu Sprache und Spracherkennung.
- **Sprach-Assistenten** können Menschen ähnliche Konversations Schnittstellen für Anwendungen erstellen.

In diesem Artikel wird erläutert, wie Sprache-zu-Text in der xamarin. Forms-Beispielanwendung mit dem Azure Speech-Dienst implementiert wird. Die folgenden Screenshots zeigen die Beispielanwendung unter IOS und Android:

[![Screenshots der Beispielanwendung unter IOS und Android](speech-recognition-images/speech-recognition-cropped.png)](speech-recognition-images/speech-recognition.png#lightbox "Screenshots der Beispielanwendung unter IOS und Android")

## <a name="create-an-azure-speech-service-resource"></a>Erstellen einer Azure Speech Service-Ressource

Der Azure Speech Service ist Teil von Azure Cognitive Services und bietet cloudbasierte APIs für Aufgaben wie Bild Erkennung, Spracherkennung und-Übersetzung sowie die Suche nach Azure. Weitere Informationen finden Sie unter [Was sind Azure-Cognitive Services?](https://docs.microsoft.com/azure/cognitive-services/welcome).

Für das Beispiel Projekt muss eine Azure Cognitive Services-Ressource in Ihrem Azure-Portal erstellt werden. Eine Cognitive Services Ressource kann für einen einzelnen Dienst, z. b. den Sprachdienst, oder als multidienstressource erstellt werden. Die Schritte zum Erstellen einer Sprachdienst Ressource lauten wie folgt:

1. Melden Sie sich bei Ihrem [Azure-Portal](https://portal.azure.com)an.
1. Erstellen Sie eine Multiservice-oder eine Einzel Dienst Ressource.
1. Abrufen der API-Schlüssel-und Regions Informationen für Ihre Ressource.
1. Aktualisieren Sie die **Constants.cs** -Beispieldatei.

Eine Schritt-für-Schritt-Anleitung zum Erstellen einer Ressource finden Sie unter [Erstellen einer Cognitive Services Ressource](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account).

> [!NOTE]
> Wenn Sie kein [Azure-Abonnement](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing) besitzen, erstellen Sie ein [kostenloses Konto](https://aka.ms/azfree-docs-mobileapps), bevor Sie beginnen. Sobald Sie über ein Konto verfügen, können Sie eine einzelne Dienst Ressource im Free-Tarif erstellen, um den Dienst zu testen.

## <a name="configure-your-app-with-the-speech-service"></a>Konfigurieren Ihrer APP mit dem Sprachdienst

Nachdem Sie eine Cognitive Services Ressource erstellt haben, kann die Datei **Constants.cs** mit der Region und dem API-Schlüssel aus ihrer Azure-Ressource aktualisiert werden:

```csharp
public static class Constants
{
    public static string CognitiveServicesApiKey = "YOUR_KEY_GOES_HERE";
    public static string CognitiveServicesRegion = "westus";
}
```

## <a name="install-nuget-speech-service-package"></a>Nuget-Sprachdienst Paket installieren

In der Beispielanwendung wird das nuget-Paket **Microsoft. cognitiveservices. Speech** verwendet, um eine Verbindung mit dem Azure Speech-Dienst herzustellen. Installieren Sie dieses nuget-Paket im Projekt "freigegeben" und in jedem Platt Form Projekt.

## <a name="create-an-imicrophoneservice-interface"></a>Erstellen einer imikro phoneservice-Schnittstelle

Jede Plattform erfordert die Berechtigung für den Zugriff auf das Mikrofon. Das Beispiel Projekt stellt eine `IMicrophoneService`-Schnittstelle im freigegebenen Projekt bereit und verwendet die xamarin. Forms-`DependencyService`, um Platt Form Implementierungen der-Schnittstelle abzurufen.

```csharp
public interface IMicrophoneService
{
    Task<bool> GetPermissionAsync();
    void OnRequestPermissionResult(bool isGranted);
}
```

## <a name="create-the-page-layout"></a>Erstellen des Seitenlayouts

Das Beispiel Projekt definiert ein einfaches Seitenlayout in der Datei " **MainPage. XAML** ". Bei den schlüssellayoutelementen handelt es sich um eine `Button`, die den Aufzeichnungsprozess startet, eine `Label`, die den Transkriptions Text enthält, sowie eine `ActivityIndicator`, die angezeigt wird, wenn die Aufzeichnung ausgeführt wird:

```xaml
<ContentPage ...>
    <StackLayout>
        <Frame ...>
            <ScrollView x:Name="scroll"
                        ...>
                <Label x:Name="transcribedText"
                       ... />
            </ScrollView>
        </Frame>

        <ActivityIndicator x:Name="transcribingIndicator"
                           IsRunning="False" />
        <Button x:Name="transcribeButton"
                ...
                Clicked="TranscribeClicked"/>
    </StackLayout>
</ContentPage>
```

## <a name="implement-the-speech-service"></a>Implementieren des sprach Dienstanbieter

Die **MainPage.XAML.cs** -Code Behind-Datei enthält die gesamte Logik zum Senden von Audiodaten und zum Empfangen von Text aus dem Azure Speech-Dienst.

Der `MainPage`-Konstruktor ruft eine Instanz der `IMicrophoneService` Schnittstelle aus dem `DependencyService`ab:

```csharp
public partial class MainPage : ContentPage
{
    SpeechRecognizer recognizer;
    IMicrophoneService micService;
    bool isTranscribing = false;

    public MainPage()
    {
        InitializeComponent();

        micService = DependencyService.Resolve<IMicrophoneService>();
    }

    // ...
}
```

Die `TranscribeClicked`-Methode wird aufgerufen, wenn die `transcribeButton` Instanz abgetippt wird:

```csharp
async void TranscribeClicked(object sender, EventArgs e)
{
    bool isMicEnabled = await micService.GetPermissionAsync();

    // EARLY OUT: make sure mic is accessible
    if (!isMicEnabled)
    {
        UpdateTranscription("Please grant access to the microphone!");
        return;
    }

    // initialize speech recognizer 
    if (recognizer == null)
    {
        var config = SpeechConfig.FromSubscription(Constants.CognitiveServicesApiKey, Constants.CognitiveServicesRegion);
        recognizer = new SpeechRecognizer(config);
        recognizer.Recognized += (obj, args) =>
        {
            UpdateTranscription(args.Result.Text);
        };
    }

    // if already transcribing, stop speech recognizer
    if (isTranscribing)
    {
        try
        {
            await recognizer.StopContinuousRecognitionAsync();
        }
        catch(Exception ex)
        {
            UpdateTranscription(ex.Message);
        }
        isTranscribing = false;
    }

    // if not transcribing, start speech recognizer
    else
    {
        Device.BeginInvokeOnMainThread(() =>
        {
            InsertDateTimeRecord();
        });
        try
        {
            await recognizer.StartContinuousRecognitionAsync();
        }
        catch(Exception ex)
        {
            UpdateTranscription(ex.Message);
        }
        isTranscribing = true;
    }
    UpdateDisplayState();
}
```

Die `TranscribeClicked`-Methode führt folgende Schritte aus:

1. Überprüft, ob die Anwendung Zugriff auf das Mikrofon hat, und wird vorzeitig beendet, wenn dies nicht der Fall ist.
1. Erstellt eine Instanz `SpeechRecognizer`-Klasse, wenn Sie nicht bereits vorhanden ist.
1. Beendet die fortlaufende Aufzeichnung, wenn Sie ausgeführt wird.
1. Fügt einen Zeitstempel ein und startet fortlaufende Aufzeichnung, wenn er nicht ausgeführt wird.
1. Benachrichtigt die Anwendung, ihre Darstellung basierend auf dem neuen Anwendungs Zustand zu aktualisieren.

Der Rest der `MainPage`-Klassen Methoden sind Hilfsprogramme zum Anzeigen des Anwendungs Zustands:

```csharp
void UpdateTranscription(string newText)
{
    Device.BeginInvokeOnMainThread(() =>
    {
        if (!string.IsNullOrWhiteSpace(newText))
        {
            transcribedText.Text += $"{newText}\n";
        }
    });
}

void InsertDateTimeRecord()
{
    var msg = $"=================\n{DateTime.Now.ToString()}\n=================";
    UpdateTranscription(msg);
}

void UpdateDisplayState()
{
    Device.BeginInvokeOnMainThread(() =>
    {
        if (isTranscribing)
        {
            transcribeButton.Text = "Stop";
            transcribeButton.BackgroundColor = Color.Red;
            transcribingIndicator.IsRunning = true;
        }
        else
        {
            transcribeButton.Text = "Transcribe";
            transcribeButton.BackgroundColor = Color.Green;
            transcribingIndicator.IsRunning = false;
        }
    });
}
```

Die `UpdateTranscription`-Methode schreibt die angegebene `newText` `string` in das `Label` Element mit dem Namen `transcribedText`. Es erzwingt, dass dieses Update im UI-Thread ausgeführt wird, sodass es von jedem Kontext aus aufgerufen werden kann, ohne dass Ausnahmen ausgelöst werden. Der `InsertDateTimeRecord` schreibt das aktuelle Datum und die aktuelle Uhrzeit in die `transcribedText` Instanz, um den Anfang einer neuen Aufzeichnung zu markieren. Zum Schluss aktualisiert die `UpdateDisplayState`-Methode die `Button` und `ActivityIndicator` Elemente, um widerzuspiegeln, ob eine Aufzeichnung ausgeführt wird oder nicht.

## <a name="create-platform-microphone-services"></a>Erstellen von Platform-Mikrofon-Diensten

Die Anwendung muss über Mikrofon Zugriff verfügen, um Sprach Daten zu erfassen. Die `IMicrophoneService`-Schnittstelle muss implementiert und bei der `DependencyService` auf jeder Plattform registriert werden, damit die Anwendung funktioniert.

### <a name="android"></a>Android

Das Beispiel Projekt definiert eine `IMicrophoneService` Implementierung für Android namens `AndroidMicrophoneService`:

```csharp
[assembly: Dependency(typeof(AndroidMicrophoneService))]
namespace CognitiveSpeechService.Droid.Services
{
    public class AndroidMicrophoneService : IMicrophoneService
    {
        public const int RecordAudioPermissionCode = 1;
        private TaskCompletionSource<bool> tcsPermissions;
        string[] permissions = new string[] { Manifest.Permission.RecordAudio };

        public Task<bool> GetPermissionAsync()
        {
            tcsPermissions = new TaskCompletionSource<bool>();

            if ((int)Build.VERSION.SdkInt < 23)
            {
                tcsPermissions.TrySetResult(true);
            }
            else
            {
                var currentActivity = MainActivity.Instance;
                if (ActivityCompat.CheckSelfPermission(currentActivity, Manifest.Permission.RecordAudio) != (int)Permission.Granted)
                {
                    RequestMicPermissions();
                }
                else
                {
                    tcsPermissions.TrySetResult(true);
                }

            }

            return tcsPermissions.Task;
        }

        public void OnRequestPermissionResult(bool isGranted)
        {
            tcsPermissions.TrySetResult(isGranted);
        }

        void RequestMicPermissions()
        {
            if (ActivityCompat.ShouldShowRequestPermissionRationale(MainActivity.Instance, Manifest.Permission.RecordAudio))
            {
                Snackbar.Make(MainActivity.Instance.FindViewById(Android.Resource.Id.Content),
                        "Microphone permissions are required for speech transcription!",
                        Snackbar.LengthIndefinite)
                        .SetAction("Ok", v =>
                        {
                            ((Activity)MainActivity.Instance).RequestPermissions(permissions, RecordAudioPermissionCode);
                        })
                        .Show();
            }
            else
            {
                ActivityCompat.RequestPermissions((Activity)MainActivity.Instance, permissions, RecordAudioPermissionCode);
            }
        }
    }
}
```

Der `AndroidMicrophoneService` verfügt über die folgenden Features:

1. Das `Dependency`-Attribut registriert die Klasse beim `DependencyService`.
1. Die `GetPermissionAsync`-Methode überprüft, ob Berechtigungen auf der Grundlage der Android SDK Version erforderlich sind, und ruft `RequestMicPermissions` auf, wenn die Berechtigung noch nicht erteilt wurde.
1. Die `RequestMicPermissions`-Methode verwendet die `Snackbar`-Klasse, um Berechtigungen für den Benutzer anzufordern, wenn eine Begründung erforderlich ist; andernfalls werden direkt audioaufzeichnungs Berechtigungen angefordert.
1. Die `OnRequestPermissionResult`-Methode wird mit einem `bool` Ergebnis aufgerufen, sobald der Benutzer auf die Berechtigungs Anforderung geantwortet hat.

Die `MainActivity`-Klasse wird angepasst, um die `AndroidMicrophoneService` Instanz zu aktualisieren, wenn Berechtigungsanforderungen erfüllt sind:

```csharp
public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
{
    IMicrophoneService micService;
    internal static MainActivity Instance { get; private set; }
    
    protected override void OnCreate(Bundle savedInstanceState)
    {
        Instance = this;
        // ...
        micService = DependencyService.Resolve<IMicrophoneService>();
    }
    public override void OnRequestPermissionsResult(int requestCode, string[] permissions, [GeneratedEnum] Android.Content.PM.Permission[] grantResults)
    {
        // ...
        switch(requestCode)
        {
            case AndroidMicrophoneService.RecordAudioPermissionCode:
                if (grantResults[0] == Permission.Granted)
                {
                    micService.OnRequestPermissionResult(true);
                }
                else
                {
                    micService.OnRequestPermissionResult(false);
                }
                break;
        }
    }
}
```

Die `MainActivity`-Klasse definiert einen statischen Verweis namens `Instance`, der beim Anfordern von Berechtigungen vom `AndroidMicrophoneService`-Objekt benötigt wird. Er überschreibt die `OnRequestPermissionsResult`-Methode, um das `AndroidMicrophoneService` Objekt zu aktualisieren, wenn die Berechtigungs Anforderung vom Benutzer genehmigt oder verweigert wird.

Schließlich muss die Android-Anwendung die Berechtigung zum Aufzeichnen von Audiodaten in der Datei " **androidmanifest. XML** " enthalten:

```xml
<manifest ...>
    ...
    <uses-permission android:name="android.permission.RECORD_AUDIO" />
</manifest>
```

### <a name="ios"></a>iOS

Das Beispiel Projekt definiert eine `IMicrophoneService` Implementierung für IOS namens `iOSMicrophoneService`:

```csharp
[assembly: Dependency(typeof(iOSMicrophoneService))]
namespace CognitiveSpeechService.iOS.Services
{
    public class iOSMicrophoneService : IMicrophoneService
    {
        TaskCompletionSource<bool> tcsPermissions;

        public Task<bool> GetPermissionAsync()
        {
            tcsPermissions = new TaskCompletionSource<bool>();
            RequestMicPermission();
            return tcsPermissions.Task;
        }

        public void OnRequestPermissionResult(bool isGranted)
        {
            tcsPermissions.TrySetResult(isGranted);
        }

        void RequestMicPermission()
        {
            var session = AVAudioSession.SharedInstance();
            session.RequestRecordPermission((granted) =>
            {
                tcsPermissions.TrySetResult(granted);
            });
        }
    }
}
```

Der `iOSMicrophoneService` verfügt über die folgenden Features:

1. Das `Dependency`-Attribut registriert die Klasse beim `DependencyService`.
1. Mit der `GetPermissionAsync`-Methode werden `RequestMicPermissions` aufgerufen, um Berechtigungen vom Gerätebenutzer anzufordern.
1. Die `RequestMicPermissions`-Methode verwendet die Shared `AVAudioSession`-Instanz, um Aufzeichnungs Berechtigungen anzufordern.
1. Die `OnRequestPermissionResult`-Methode aktualisiert die `TaskCompletionSource` Instanz mit dem angegebenen `bool` Wert.

Zum Schluss muss die IOS-APP **Info. plist** eine Meldung enthalten, die den Benutzer darüber informiert, warum die APP Zugriff auf das Mikrofon anfordert. Bearbeiten Sie die Datei "Info. plist", um die folgenden Tags in das `<dict>`-Element einzubeziehen:

```xml
<plist>
    <dict>
        ...
        <key>NSMicrophoneUsageDescription</key>
        <string>Voice transcription requires microphone access</string>
    </dict>
</plist>
```

### <a name="uwp"></a>UWP

Das Beispiel Projekt definiert eine `IMicrophoneService` Implementierung für UWP namens `UWPMicrophoneService`:

```csharp
[assembly: Dependency(typeof(UWPMicrophoneService))]
namespace CognitiveSpeechService.UWP.Services
{
    public class UWPMicrophoneService : IMicrophoneService
    {
        public async Task<bool> GetPermissionAsync()
        {
            bool isMicAvailable = true;
            try
            {
                var mediaCapture = new MediaCapture();
                var settings = new MediaCaptureInitializationSettings();
                settings.StreamingCaptureMode = StreamingCaptureMode.Audio;
                await mediaCapture.InitializeAsync(settings);
            }
            catch(Exception ex)
            {
                isMicAvailable = false;
            }

            if(!isMicAvailable)
            {
                await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-microphone"));
            }

            return isMicAvailable;
        }

        public void OnRequestPermissionResult(bool isGranted)
        {
            // intentionally does nothing
        }
    }
}
```

Der `UWPMicrophoneService` verfügt über die folgenden Features:

1. Das `Dependency`-Attribut registriert die Klasse beim `DependencyService`.
1. Die `GetPermissionAsync`-Methode versucht, eine `MediaCapture` Instanz zu initialisieren. Wenn dies nicht der Fall ist, wird eine Benutzer Anforderung zum Aktivieren des Mikrofons gestartet.
1. Die `OnRequestPermissionResult`-Methode ist vorhanden, um die-Schnittstelle zu erfüllen, ist aber für die UWP-Implementierung nicht erforderlich

Zum Schluss muss das UWP- **Paket. appxmanifest** angeben, dass die Anwendung das Mikrofon verwendet. Doppelklicken Sie auf die Datei Package. appxmanifest, und wählen Sie die Option **Mikrofon** auf der Registerkarte **Funktionen** in Visual Studio 2019 aus:

[![Screenshot des Manifests in Visual Studio 2019](speech-recognition-images/package-manifest-cropped.png)](speech-recognition-images/package-manifest.png#lightbox "Screenshot des Manifests in Visual Studio 2019")

## <a name="test-the-application"></a>Testen der Anwendung

Führen Sie die APP aus, und klicken Sie auf die Schaltfläche **transcri** Die APP sollte Mikrofon Zugriff anfordern und den Aufzeichnungsprozess starten. Der `ActivityIndicator` wird animiert und zeigt an, dass die Aufzeichnung aktiv ist. Wenn Sie sprechen, werden die Audiodaten von der APP an die Azure Speech Services-Ressource gestreamt, die mit einem Text mit Text überschrieben wird. Der Text mit Text wird im `Label` Element angezeigt, wenn er empfangen wird.

> [!NOTE]
> Android-Emulatoren können die Sprachdienst Bibliotheken nicht laden und initialisieren. Das Testen auf einem physischen Gerät wird für die Android-Plattform empfohlen.

## <a name="related-links"></a>Verwandte Links

- [Azure Speech Service-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-cognitivespeechservice)
- [Übersicht über den Azure-Sprachdienst](https://docs.microsoft.com/azure/cognitive-services/speech-service/overview)
- [Erstellen einer Cognitive Services-Ressource](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)
- [Schnellstart: Erkennen von Sprache von einem Mikrofon](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/speech-to-text-from-microphone)