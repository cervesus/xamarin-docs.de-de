---
title: Remote Benachrichtigungen mit Google Cloud Messaging
description: Diese exemplarische Vorgehensweise enthält eine schrittweise Erläuterung der Verwendung von Google Cloud Messaging zum Implementieren von Remote Benachrichtigungen (auch als Pushbenachrichtigungen bezeichnet) in einer xamarin. Android-Anwendung. In diesem Thema werden die verschiedenen Klassen beschrieben, die Sie implementieren müssen, um mit Google Cloud Messaging (GCM) zu kommunizieren. Außerdem wird erläutert, wie Berechtigungen im Android-Manifest für den Zugriff auf GCM festgelegt werden, und es wird ein End-to-End-Messaging mit einem Beispiel Testprogramm veranschaulicht.
ms.prod: xamarin
ms.assetid: 4FC3C774-EF93-41B2-A81E-C6A08F32C09B
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/02/2019
ms.openlocfilehash: b621d61584cc39f669c662db3d1df264d9a92eb9
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76723772"
---
# <a name="remote-notifications-with-google-cloud-messaging"></a>Remote Benachrichtigungen mit Google Cloud Messaging

> [!WARNING]
> Google hat GCM seit dem 10. April 2018 als veraltet markiert. Die folgenden Dokumente und Beispiel Projekte werden möglicherweise nicht mehr verwaltet. Der GCM-Server und die Client-APIs von Google werden ab dem 29. Mai 2019 entfernt. Google empfiehlt, GCM-apps zu Firebase Cloud Messaging (FCM) zu migrieren. Weitere Informationen zur Veraltung und Migration von GCM finden Sie unter [Google Cloud Messaging deprecated](https://developers.google.com/cloud-messaging/).
>
> Informationen zu den ersten Schritten mit Remote Benachrichtigungen mithilfe von Firebase Cloud Messaging mit xamarin finden Sie unter [Remote Benachrichtigungen mit FCM](remote-notifications-with-fcm.md).

_Diese exemplarische Vorgehensweise enthält eine schrittweise Erläuterung der Verwendung von Google Cloud Messaging zum Implementieren von Remote Benachrichtigungen (auch als Pushbenachrichtigungen bezeichnet) in einer xamarin. Android-Anwendung. In diesem Thema werden die verschiedenen Klassen beschrieben, die Sie implementieren müssen, um mit Google Cloud Messaging (GCM) zu kommunizieren. Außerdem wird erläutert, wie Berechtigungen im Android-Manifest für den Zugriff auf GCM festgelegt werden, und es wird ein End-to-End-Messaging mit einem Beispiel Testprogramm veranschaulicht._

## <a name="gcm-notifications-overview"></a>Übersicht über GCM-Benachrichtigungen

In dieser exemplarischen Vorgehensweise erstellen wir eine xamarin. Android-Anwendung, die Google Cloud Messaging (GCM) zum Implementieren von Remote Benachrichtigungen (auch als *Pushbenachrichtigungen*bezeichnet) verwendet. Wir implementieren die verschiedenen Intent-und Listener-Dienste, die GCM für Remote Messaging verwenden. Wir testen unsere Implementierung mit einem Befehlszeilenprogramm, das einen Anwendungsserver simuliert.

Bevor Sie mit dieser exemplarischen Vorgehensweise fortfahren können, müssen Sie die erforderlichen Anmelde Informationen für die Verwendung der GCM-Server von Google erwerben. Dieser Prozess wird in [Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md)erläutert.
Insbesondere benötigen Sie einen *API-Schlüssel* und eine Absender- *ID* , die in den in dieser exemplarischen Vorgehensweise dargestellten Beispielcode eingefügt werden.

Wir führen die folgenden Schritte aus, um eine GCM-aktivierte xamarin. Android-Client-App zu erstellen:

1. Installieren zusätzlicher Pakete, die für die Kommunikation mit GCM-Servern erforderlich sind.
2. Konfigurieren Sie App-Berechtigungen für den Zugriff auf GCM-Server.
3. Implementieren Sie Code, um zu überprüfen, ob Google Play Services vorhanden ist.
4. Implementieren Sie einen Dienst für die Registrierungs Absicht, der mit GCM für ein Registrierungs Token aushandiert.
5. Implementieren Sie einen Instanz-ID-Listenerdienst, der auf registrierungstokenaktualisierungen von GCM lauscht
6. Implementieren Sie einen GCM-Listenerdienst, der Remote Nachrichten vom App-Server über GCM empfängt.

Diese APP verwendet ein neues GCM-Feature, das als *Topic Messaging*bezeichnet wird. Im Thema Messaging sendet der App-Server eine Nachricht an ein Thema und nicht an eine Liste einzelner Geräte. Geräte, die dieses Thema abonnieren, können Thema Nachrichten als Pushbenachrichtigungen empfangen.

Wenn die Client-App bereit ist, implementieren wir eine Befehlszeilen C# Anwendung, die per GCM eine Pushbenachrichtigung an unsere Client-App sendet.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

Erstellen Sie zunächst eine neue leere Lösung namens **remotenotifications**. Als Nächstes fügen wir dieser Lösung ein neues Android-Projekt hinzu, das auf der **Android-App** -Vorlage basiert. Wir nennen dieses Projekt " **ClientApp**". (Wenn Sie mit dem Erstellen von xamarin. Android-Projekten nicht vertraut sind, finden Sie weitere Informationen unter [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md).) Das **ClientApp** -Projekt enthält den Code für die xamarin. Android-Client Anwendung, die Remote Benachrichtigungen über GCM empfängt.

### <a name="add-required-packages"></a>Erforderliche Pakete hinzufügen

Bevor wir unseren Client-App-Code implementieren können, müssen wir mehrere Pakete installieren, die für die Kommunikation mit GCM verwendet werden. Außerdem müssen wir dem Gerät die Google Play Store Anwendung hinzufügen, wenn es nicht bereits installiert ist.

#### <a name="add-the-xamarin-google-play-services-gcm-package"></a>Hinzufügen des xamarin Google Play Services GCM-Pakets

Zum Empfangen von Nachrichten von Google Cloud Messaging muss das [Google Play Services](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Gcm/) Framework auf dem Gerät vorhanden sein. Ohne dieses Framework kann eine Android-Anwendung keine Nachrichten von GCM-Servern empfangen. Google Play Services wird im Hintergrund ausgeführt, während das Android-Gerät eingeschaltet ist, und lauscht in der Ruhe auf Nachrichten aus GCM. Wenn diese Nachrichten eintreffen, konvertiert Google Play Services die Nachrichten in Intents und überträgt diese Intents dann an Anwendungen, die für Sie registriert wurden.

Klicken Sie in Visual Studio mit der rechten Maustaste auf **Verweise > nuget-Pakete verwalten...** ; Klicken Sie in Visual Studio für Mac mit der rechten Maustaste auf **Pakete > Pakete hinzufügen...** . Suchen Sie nach **xamarin Google Play Services-GCM** , und installieren Sie dieses Paket im **ClientApp** -Projekt:

[![Installation Google Play Services](remote-notifications-with-gcm-images/1-google-play-services-sml.png)](remote-notifications-with-gcm-images/1-google-play-services.png#lightbox)

Wenn Sie **xamarin Google Play Services-GCM**installieren, wird **xamarin Google Play Services-Base** automatisch installiert. Wenn Sie eine Fehlermeldung erhalten, ändern Sie die *Mindesteinstellung für Android in* das Projekt auf einen anderen Wert als " **Kompilieren" mit der SDK-Version** , und versuchen Sie die nuget-Installation erneut.

Bearbeiten Sie als nächstes **MainActivity.cs** , und fügen Sie die folgenden `using`-Anweisungen hinzu:

```csharp
using Android.Gms.Common;
using Android.Util;
```

Dadurch sind Typen im Google Play Services GMS-Paket für den Code verfügbar, und es werden Protokollierungsfunktionen hinzugefügt, mit denen wir unsere Transaktionen mit GMS verfolgen.

#### <a name="google-play-store"></a>Google Play Store

Zum Empfangen von Nachrichten aus GCM muss die Google Play Store Anwendung auf dem Gerät installiert sein. (Wenn eine Google Play Anwendung auf einem Gerät installiert ist, wird Google Play Store ebenfalls installiert. Daher ist es wahrscheinlich, dass Sie bereits auf dem Testgerät installiert ist.) Ohne Google Play kann eine Android-Anwendung keine Nachrichten von GCM empfangen.
Wenn Sie die Google Play Store-App noch nicht auf Ihrem Gerät installiert haben, besuchen Sie die [Google Play](https://support.google.com/googleplay) -Website, um Google Play herunterzuladen und zu installieren.

Alternativ können Sie einen Android-Emulator, auf dem Android 2,2 oder höher ausgeführt wird, anstelle eines Testgeräts verwenden (Sie müssen Google Play Store nicht auf einem Android-Emulator installieren). Wenn Sie jedoch einen Emulator verwenden, müssen Sie die WLAN-Verbindung verwenden, um eine Verbindung mit GCM herzustellen, und Sie müssen mehrere Ports in der WLAN-Firewall öffnen, wie weiter unten in dieser exemplarischen Vorgehensweise erläutert.

### <a name="set-the-package-name"></a>Festlegen des Paket namens

In [Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md)haben wir einen Paketnamen für die GCM-aktivierte App angegeben (dieser Paketname dient auch als *Anwendungs-ID* , die mit dem API-Schlüssel und der Absender-ID verknüpft ist). Öffnen Sie die Eigenschaften für das **ClientApp** -Projekt, und legen Sie den Paketnamen auf diese Zeichenfolge fest. In diesem Beispiel wird der Paketname auf `com.xamarin.gcmexample`festgelegt:

[![Festlegen des Paket namens](remote-notifications-with-gcm-images/2-package-name-sml.png)](remote-notifications-with-gcm-images/2-package-name.png#lightbox)

Beachten Sie, dass die Client-App kein Registrierungs Token von GCM empfangen kann, wenn dieser Paketname nicht *exakt* mit dem Paketnamen übereinstimmt, den wir in die Google Developer Console eingegeben haben.

### <a name="add-permissions-to-the-android-manifest"></a>Hinzufügen von Berechtigungen zum Android-Manifest

Für eine Android-Anwendung müssen die folgenden Berechtigungen konfiguriert sein, bevor Benachrichtigungen von Google Cloud Messaging empfangen werden können:

- `com.google.android.c2dm.permission.RECEIVE` &ndash; erteilt der APP die Berechtigung zum Registrieren und empfangen von Nachrichten von Google Cloud Messaging. (Was bedeutet `c2dm`? Dies steht für _Cloud-zu-Gerät-Messaging_, bei dem es sich um den mittlerweile veralteten Vorgänger für GCM handelt.
    GCM verwendet weiterhin `c2dm` in vielen Ihrer Berechtigungs Zeichenfolgen.)

- `android.permission.WAKE_LOCK` &ndash; (optional) verhindert, dass die CPU des Geräts beim lauschen auf eine Nachricht in den Standbymodus wechselt.

- `android.permission.INTERNET` &ndash; gewährt Internet Zugriff, sodass die Client-App mit GCM kommunizieren kann.

- *package_name*`.permission.C2D_MESSAGE` &ndash; die Anwendung bei Android registrieren und die Berechtigung anfordern, exklusiv alle C2D-Nachrichten (Cloud-zu-Gerät) zu empfangen. Das *package_name* Präfix ist mit der Anwendungs-ID identisch.

Diese Berechtigungen werden im Android-Manifest festgelegt. Bearbeiten Sie " **androidmanifest. XML** ", und ersetzen Sie den Inhalt durch den folgenden XML-Code:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="YOUR_PACKAGE_NAME"
    android:versionCode="1"
    android:versionName="1.0"
    android:installLocation="auto">
    <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
    <uses-permission android:name="android.permission.WAKE_LOCK" />
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="YOUR_PACKAGE_NAME.permission.C2D_MESSAGE" />
    <permission android:name="YOUR_PACKAGE_NAME.permission.C2D_MESSAGE"
                android:protectionLevel="signature" />
    <application android:label="ClientApp" android:icon="@drawable/Icon">
    </application>
</manifest>
```

Ändern Sie im obigen XML-Code *YOUR_PACKAGE_NAME* in den Paketnamen für das Client-App-Projekt. Beispiel: `com.xamarin.gcmexample`.

### <a name="check-for-google-play-services"></a>Auf Google Play Services überprüfen

In dieser exemplarischen Vorgehensweise erstellen wir eine Bare-the-app mit einem einzigen `TextView` in der Benutzeroberfläche. Diese APP weist nicht direkt auf die Interaktion mit GCM hin. Stattdessen sehen wir uns das Ausgabefenster an, um zu sehen, wie die APP mit GCM Handshakes, und wir überprüfen die Benachrichtigungsleiste auf neue Benachrichtigungen, sobald sie eintreffen.

Erstellen Sie zunächst ein Layout für den Nachrichtenbereich. Bearbeiten Sie **Resources. Layout. Main. axml** , und ersetzen Sie den Inhalt durch den folgenden XML-Code:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="10dp">
    <TextView
        android:text=" "
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/msgText"
        android:textAppearance="?android:attr/textAppearanceMedium"
        android:padding="10dp" />
</LinearLayout>
```

Speichern Sie die Datei **Main. axml** , und schließen Sie Sie.

Wenn die Client-App gestartet wird, soll überprüft werden, ob Google Play Services verfügbar ist, bevor versucht wird, GCM zu kontaktieren. Bearbeiten Sie **MainActivity.cs** , und ersetzen Sie die Deklaration der ``count``-Instanzvariablen durch die folgende Deklaration der-

```csharp
TextView msgText;
```

Fügen Sie als nächstes der **mainactivity** -Klasse die folgende Methode hinzu:

```csharp
public bool IsPlayServicesAvailable ()
{
    int resultCode = GoogleApiAvailability.Instance.IsGooglePlayServicesAvailable (this);
    if (resultCode != ConnectionResult.Success)
    {
        if (GoogleApiAvailability.Instance.IsUserResolvableError (resultCode))
            msgText.Text = GoogleApiAvailability.Instance.GetErrorString (resultCode);
        else
        {
            msgText.Text = "Sorry, this device is not supported";
            Finish ();
        }
        return false;
    }
    else
    {
        msgText.Text = "Google Play Services is available.";
        return true;
    }
}
```

Dieser Code überprüft das Gerät, um festzustellen, ob das Google Play Services APK installiert ist. Wenn Sie nicht installiert ist, wird eine Meldung im Meldungs Bereich angezeigt, in der der Benutzer angewiesen wird, ein APK vom Google Play Store herunterzuladen (oder in den Systemeinstellungen des Geräts zu aktivieren). Da diese Überprüfung beim Starten der Client-app ausgeführt werden soll, fügen Sie am Ende `OnCreate`einen-Aufrufvorgang hinzu.

Ersetzen Sie als nächstes die `OnCreate`-Methode durch den folgenden Code:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    SetContentView (Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);

    IsPlayServicesAvailable ();
}
```

Dieser Code überprüft, ob das Google Play Services APK vorhanden ist, und schreibt das Ergebnis in den Nachrichtenbereich.

Wir erstellen die APP vollständig neu und führen Sie aus. Es sollte ein Bildschirm angezeigt werden, der dem folgenden Screenshot ähnelt:

[![Google Play Services ist verfügbar.](remote-notifications-with-gcm-images/3-first-screen-sml.png)](remote-notifications-with-gcm-images/3-first-screen.png#lightbox)

Wenn Sie dieses Ergebnis nicht erhalten, vergewissern Sie sich, dass das Google Play Services APK auf Ihrem Gerät installiert ist und dass das **xamarin Google Play Services-GCM-** Paket zum **Client App** -Projekt hinzugefügt wird, wie bereits erläutert. Wenn Sie einen Buildfehler erhalten, versuchen Sie, die Projekt Mappe zu bereinigen und das Projekt neu zu erstellen.

Als nächstes schreiben wir Code, um GCM zu kontaktieren und ein Registrierungs Token zurückzukommen.

### <a name="register-with-gcm"></a>Bei GCM registrieren

Bevor die APP Remote Benachrichtigungen vom App-Server empfangen kann, muss Sie sich bei GCM registrieren und ein Registrierungs Token zurückerhalten. Das Registrieren unserer Anwendung bei GCM erfolgt durch eine `IntentService`, die wir erstellen. Unsere `IntentService` führt die folgenden Schritte aus:

1. Verwendet die [InstanceId-](https://developers.google.com/instance-id/) API zum Generieren von Sicherheits Token, die die Client-App für den Zugriff auf den App-Server autorisieren. Im Gegenzug erhalten wir ein Registrierungs Token aus GCM zurück.

2. Leitet das Registrierungs Token an den App-Server weiter (sofern der App-Server dies erfordert).

3. Abonniert einen oder mehrere Benachrichtigungs Themen Kanäle.

Nachdem wir diese `IntentService`implementiert haben, testen wir Sie, um festzustellen, ob wir ein Registrierungs Token aus GCM zurückerhalten.

Fügen Sie eine neue Datei mit dem Namen **RegistrationIntentService.cs** hinzu, und ersetzen Sie den Vorlagen Code durch Folgendes:

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Util;
using Android.Gms.Gcm;
using Android.Gms.Gcm.Iid;

namespace ClientApp
{
    [Service(Exported = false)]
    class RegistrationIntentService : IntentService
    {
        static object locker = new object();

        public RegistrationIntentService() : base("RegistrationIntentService") { }

        protected override void OnHandleIntent (Intent intent)
        {
            try
            {
                Log.Info ("RegistrationIntentService", "Calling InstanceID.GetToken");
                lock (locker)
                {
                    var instanceID = InstanceID.GetInstance (this);
                    var token = instanceID.GetToken (
                        "YOUR_SENDER_ID", GoogleCloudMessaging.InstanceIdScope, null);

                    Log.Info ("RegistrationIntentService", "GCM Registration Token: " + token);
                    SendRegistrationToAppServer (token);
                    Subscribe (token);
                }
            }
            catch (Exception e)
            {
                Log.Debug("RegistrationIntentService", "Failed to get a registration token");
                return;
            }
        }

        void SendRegistrationToAppServer (string token)
        {
            // Add custom implementation here as needed.
        }

        void Subscribe (string token)
        {
            var pubSub = GcmPubSub.GetInstance(this);
            pubSub.Subscribe(token, "/topics/global", null);
        }
    }
}
```

Ändern Sie im obigen Beispielcode *YOUR_SENDER_ID* in die Absender-ID für das Client-App-Projekt. So erhalten Sie die Absender-ID für Ihr Projekt:

1. Melden Sie sich bei der [Google Cloud Console](https://console.cloud.google.com/) an, und wählen Sie im Pulldownmenü den Projektnamen aus. Klicken Sie im Bereich **Project Info** , der für das Projekt angezeigt wird, auf **zu Projekteinstellungen**wechseln:

    [![auswählen eines xamaringcm-Projekts](remote-notifications-with-gcm-images/7-choose-project-sml.png)](remote-notifications-with-gcm-images/7-choose-project.png#lightbox)

2. Suchen Sie auf der Seite **Einstellungen** nach der **Projekt Nummer** &ndash; dies die Absender-ID für Ihr Projekt ist:

    [![Projekt Nummer angezeigt](remote-notifications-with-gcm-images/9-project-number-sml.png)](remote-notifications-with-gcm-images/9-project-number.png#lightbox)

Wir möchten unsere `RegistrationIntentService` starten, wenn die APP gestartet wird. Bearbeiten Sie **MainActivity.cs** , und ändern Sie die `OnCreate`-Methode so, dass unser `RegistrationIntentService` nach dem Überprüfen auf das vorhanden sein von Google Play Services gestartet wird:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    SetContentView(Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);

    if (IsPlayServicesAvailable ())
    {
        var intent = new Intent (this, typeof (RegistrationIntentService));
        StartService (intent);
    }
}
```

Sehen wir uns nun den einzelnen Abschnitt der `RegistrationIntentService` an, um zu verstehen, wie er funktioniert.

Zuerst kommentieren wir die `RegistrationIntentService` mit dem folgenden Attribut, um anzugeben, dass der Dienst nicht vom System instanziiert werden soll:

```csharp
[Service (Exported = false)]
```

Der `RegistrationIntentService`-Konstruktor benennt den *registrationintentservice* -Arbeitsthread, um das Debuggen zu vereinfachen.

```csharp
public RegistrationIntentService() : base ("RegistrationIntentService") { }
```

Die Kernfunktionalität von `RegistrationIntentService` befindet sich in der `OnHandleIntent`-Methode. Sehen wir uns diesen Code an, um zu erfahren, wie die APP mit GCM registriert wird.

#### <a name="request-a-registration-token"></a>Anfordern eines Registrierungs Tokens

`OnHandleIntent` zuerst die " [InstanceID. GetToken](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID.html#getToken&#40;java.lang.String,%20java.lang.String&#41;) "-Methode von Google aufrufen, um ein Registrierungs Token von GCM anzufordern. Wir wrappen diesen Code in einer `lock`, um die Möglichkeit zu schützen, mehrere Registrierungs Intents gleichzeitig auszuführen &ndash; der `lock` stellt sicher, dass diese Intents sequenziell verarbeitet werden. Wenn ein Registrierungs Token nicht erhalten werden kann, wird eine Ausnahme ausgelöst, und es wird ein Fehler protokolliert. Wenn die Registrierung erfolgreich ist, wird `token` auf das Registrierungs Token festgelegt, das wir aus GCM zurückerhalten haben:

```csharp
static object locker = new object ();
...
try
{
    lock (locker)
    {
        var instanceID = InstanceID.GetInstance (this);
        var token = instanceID.GetToken (
            "YOUR_SENDER_ID", GoogleCloudMessaging.InstanceIdScope, null);
        ...
    }
}
catch (Exception e)
{
    Log.Debug ...
```

#### <a name="forward-the-registration-token-to-the-app-server"></a>Leiten Sie das Registrierungs Token an den App-Server weiter.

Wenn wir ein Registrierungs Token erhalten (das heißt, es wurde keine Ausnahme ausgelöst), wird `SendRegistrationToAppServer` aufgerufen, um das Registrierungs Token des Benutzers dem serverseitigen Konto (sofern vorhanden) zuzuordnen, das von der Anwendung verwaltet wird. Da diese Implementierung vom Entwurf des App-Servers abhängt, wird hier eine leere Methode bereitgestellt:

```csharp
void SendRegistrationToAppServer (string token)
{
    // Add custom implementation here as needed.
}
```

In einigen Fällen benötigt der App-Server das Registrierungs Token des Benutzers nicht. in diesem Fall kann diese Methode ausgelassen werden. Wenn ein Registrierungs Token an den App-Server gesendet wird, sollte `SendRegistrationToAppServer` einen booleschen Wert aufbewahren, um anzugeben, ob das Token an den Server gesendet wurde. Wenn dieser boolesche Wert false ist, sendet `SendRegistrationToAppServer` das Token an den App-Server &ndash; andernfalls wurde das Token bereits in einem vorherigen-Befehl an den App-Server gesendet.

#### <a name="subscribe-to-the-notification-topic"></a>Abonnieren des Benachrichtigungs Themas

Als nächstes nennen wir unsere `Subscribe`-Methode, um dem GCM mitzuteilen, dass ein Benachrichtigungs Thema abonniert werden soll. In `Subscribe`wird die API "gcmpubsub. Subscribe" aufgerufen, um die Client-App für alle Nachrichten unter `/topics/global`zu abonnieren:

```csharp
void Subscribe (string token)
{
    var pubSub = GcmPubSub.GetInstance(this);
    pubSub.Subscribe(token, "/topics/global", null);
}
```

Der App-Server muss Benachrichtigungs Meldungen an `/topics/global` senden, wenn diese empfangen werden. Beachten Sie, dass der Name des Themas unter `/topics` beliebig sein kann, solange der App-Server und die Client-App Diese Namen akzeptieren. (Hier haben wir den Namen `global` ausgewählt, um anzugeben, dass wir Nachrichten für alle vom App-Server unterstützten Themen empfangen möchten.)

#### <a name="implement-an-instance-id-listener-service"></a>Implementieren Sie einen Instanz-ID-Listenerdienst

Registrierungs Token sind eindeutig und sicher. die Client-app (bzw. GCM) muss das Registrierungs Token jedoch möglicherweise aktualisieren, falls die Neuinstallation der APP oder ein Sicherheitsproblem vorliegt. Aus diesem Grund müssen wir eine `InstanceIdListenerService` implementieren, die auf tokenaktualisierungs Anforderungen von GCM antwortet.

Fügen Sie eine neue Datei mit dem Namen **InstanceIdListenerService.cs** hinzu, und ersetzen Sie den Vorlagen Code durch Folgendes:

```csharp
using Android.App;
using Android.Content;
using Android.Gms.Gcm.Iid;

namespace ClientApp
{
    [Service(Exported = false), IntentFilter(new[] { "com.google.android.gms.iid.InstanceID" })]
    class MyInstanceIDListenerService : InstanceIDListenerService
    {
        public override void OnTokenRefresh()
        {
            var intent = new Intent (this, typeof (RegistrationIntentService));
            StartService (intent);
        }
    }
}
```

Kommentieren Sie `InstanceIdListenerService` mit dem folgenden Attribut, um anzugeben, dass der Dienst nicht vom System instanziiert werden kann und dass er GCM-Registrierungs Token (auch als Instanz- *ID*bezeichnet) für Aktualisierungs Anforderungen empfangen kann:

```csharp
[Service(Exported = false), IntentFilter(new[] { "com.google.android.gms.iid.InstanceID" })]
```

Die `OnTokenRefresh`-Methode in unserem Dienst startet die `RegistrationIntentService`, damit Sie das neue Registrierungs Token abfangen kann.

#### <a name="test-registration-with-gcm"></a>Test Registrierung bei GCM

Wir erstellen die APP vollständig neu und führen Sie aus. Wenn Sie erfolgreich ein Registrierungs Token von GCM erhalten haben, sollte das Registrierungs Token im Fenster Ausgabe angezeigt werden. Beispiel:

```shell
D/Mono    ( 1934): Assembly Ref addref ClientApp[0xb4ac2400] -> Xamarin.GooglePlayServices.Gcm[0xb4ac2640]: 2
I/RegistrationIntentService( 1934): Calling InstanceID.GetToken
I/RegistrationIntentService( 1934): GCM Registration Token: f8LdveCvXig:APA91bFIsjUAbP-V8TPQdLR89qQbEJh1SYG38AcCbBUf34z5gSdUc5OsXrgs93YFiGcRSRafPfzkz23lf3-LvYV1CwrFheMjHgwPeFSh12MywnRIhz

```

### <a name="handle-downstream-messages"></a>Verarbeiten von downstreamnachrichten

Der Code, den wir bisher implementiert haben, ist nur "Setup"-Code. Es prüft, ob Google Play Services installiert ist, und verhandelt mit GCM und dem App-Server, um die Client-App für den Empfang von Remote Benachrichtigungen vorzubereiten. Wir haben jedoch noch Code implementiert, der nachgeschaltete Benachrichtigungs Meldungen empfängt und verarbeitet. Zu diesem Zweck muss ein *GCM-Listenerdienst*implementiert werden. Dieser Dienst empfängt Thema Nachrichten vom App-Server und sendet diese lokal als Benachrichtigungen. Nachdem wir diesen Dienst implementiert haben, erstellen wir ein Testprogramm zum Senden von Nachrichten an GCM, damit wir sehen können, ob die Implementierung ordnungsgemäß funktioniert.

#### <a name="add-a-notification-icon"></a>Benachrichtigungssymbol hinzufügen

Fügen Sie zunächst ein kleines Symbol hinzu, das im Benachrichtigungsbereich angezeigt wird, wenn unsere Benachrichtigung gestartet wird. Sie können [dieses Symbol](remote-notifications-with-gcm-images/ic-stat-ic-notification.png) in Ihr Projekt kopieren oder ein eigenes benutzerdefiniertes Symbol erstellen. Wir nennen die Symbol Datei " **ic_stat_button_click. png** " und kopieren Sie in den Ordner " **Resources/drawable** ". Denken Sie daran, **> vorhandenes Element hinzufügen...** zu verwenden, um diese Symbol Datei in das Projekt einzuschließen.

#### <a name="implement-a-gcm-listener-service"></a>Implementieren eines GCM-listenerdienstanbieter

Fügen Sie eine neue Datei mit dem Namen **GcmListenerService.cs** hinzu, und ersetzen Sie den Vorlagen Code durch Folgendes:

```csharp
using Android.App;
using Android.Content;
using Android.OS;
using Android.Gms.Gcm;
using Android.Util;

namespace ClientApp
{
    [Service (Exported = false), IntentFilter (new [] { "com.google.android.c2dm.intent.RECEIVE" })]
    public class MyGcmListenerService : GcmListenerService
    {
        public override void OnMessageReceived (string from, Bundle data)
        {
            var message = data.GetString ("message");
            Log.Debug ("MyGcmListenerService", "From:    " + from);
            Log.Debug ("MyGcmListenerService", "Message: " + message);
            SendNotification (message);
        }

        void SendNotification (string message)
        {
            var intent = new Intent (this, typeof(MainActivity));
            intent.AddFlags (ActivityFlags.ClearTop);
            var pendingIntent = PendingIntent.GetActivity (this, 0, intent, PendingIntentFlags.OneShot);

            var notificationBuilder = new Notification.Builder(this)
                .SetSmallIcon (Resource.Drawable.ic_stat_ic_notification)
                .SetContentTitle ("GCM Message")
                .SetContentText (message)
                .SetAutoCancel (true)
                .SetContentIntent (pendingIntent);

            var notificationManager = (NotificationManager)GetSystemService(Context.NotificationService);
            notificationManager.Notify (0, notificationBuilder.Build());
        }
    }
}
```

Werfen wir einen Blick auf jeden Abschnitt unserer `GcmListenerService`, um zu verstehen, wie er funktioniert.

Zunächst wird `GcmListenerService` mit einem Attribut versehen, um anzugeben, dass dieser Dienst nicht vom System instanziiert werden soll, und es wird ein beabsichtigter Filter hinzugefügt, um anzugeben, dass er GCM-Nachrichten empfängt:

```csharp
[Service (Exported = false), IntentFilter (new [] { "com.google.android.c2dm.intent.RECEIVE" })]
```

Wenn `GcmListenerService` eine Nachricht von GCM empfängt, wird die `OnMessageReceived`-Methode aufgerufen. Diese Methode extrahiert den Nachrichten Inhalt aus der Übergabe `Bundle`, protokolliert den Nachrichten Inhalt (damit wir ihn im Ausgabefenster anzeigen können) und ruft `SendNotification` auf, um eine lokale Benachrichtigung mit dem empfangenen Nachrichten Inhalt zu starten:

```csharp
var message = data.GetString ("message");
Log.Debug ("MyGcmListenerService", "From:    " + from);
Log.Debug ("MyGcmListenerService", "Message: " + message);
SendNotification (message);
```

Die `SendNotification`-Methode verwendet `Notification.Builder`, um die Benachrichtigung zu erstellen, und verwendet dann die `NotificationManager`, um die Benachrichtigung zu starten. Dadurch wird die Remote Benachrichtigungs Meldung in eine lokale Benachrichtigung konvertiert, damit Sie dem Benutzer angezeigt wird.
Weitere Informationen zur Verwendung von `Notification.Builder` und `NotificationManager`finden Sie unter [lokale Benachrichtigungen](~/android/app-fundamentals/notifications/local-notifications.md).

#### <a name="declare-the-receiver-in-the-manifest"></a>Deklarieren des Empfängers im Manifest

Vor dem empfangen von Nachrichten aus GCM müssen wir den GCM-Listener im Android-Manifest deklarieren. Bearbeiten Sie " **androidmanifest. XML** ", und ersetzen Sie den `<application>` Abschnitt durch den folgenden XML-Code:

```xml
<application android:label="RemoteNotifications" android:icon="@drawable/Icon">
    <receiver android:name="com.google.android.gms.gcm.GcmReceiver"
              android:exported="true"
              android:permission="com.google.android.c2dm.permission.SEND">
        <intent-filter>
            <action android:name="com.google.android.c2dm.intent.RECEIVE" />
            <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
            <category android:name="YOUR_PACKAGE_NAME" />
        </intent-filter>
    </receiver>
</application>
```

Ändern Sie im obigen XML-Code *YOUR_PACKAGE_NAME* in den Paketnamen für das Client-App-Projekt. In unserem exemplarischen Vorgehensweise wird der Paketname `com.xamarin.gcmexample`.

Sehen wir uns an, was die einzelnen Einstellungen in dieser XML-Datei sind:

|Einstellung|BESCHREIBUNG|
|---|---|
|`com.google.android.gms.gcm.GcmReceiver`|Deklariert, dass unsere APP einen GCM-Empfänger implementiert, der eingehende pushbenachrichtigungsnachrichten erfasst und verarbeitet.|
|`com.google.android.c2dm.permission.SEND`|Deklariert, dass nur GCM-Server Nachrichten direkt an die APP senden können.|
|`com.google.android.c2dm.intent.RECEIVE`|Beabsichtigte Filter ankündigen, dass unsere App Broadcast Nachrichten aus GCM verarbeitet.|
|`com.google.android.c2dm.intent.REGISTRATION`|Beabsichtigte Filter Werbung, dass unsere App neue Registrierungs Absichten behandelt (d. h., wir haben einen Instanz-ID-Listenerdienst implementiert).|

Alternativ können Sie `GcmListenerService` mit diesen Attributen versehen, anstatt Sie in XML anzugeben. Hier geben wir Sie in " **androidmanifest. XML** " an, damit die Codebeispiele leichter befolgt werden können.

### <a name="create-a-message-sender-to-test-the-app"></a>Erstellen eines Nachrichten Absenders zum Testen der APP

Fügen Sie der Projekt C# Mappe ein Desktop Konsolen Anwendungsprojekt hinzu, und nennen Sie es " **messagesender**". Wir verwenden diese Konsolenanwendung, um einen Anwendungsserver zu simulieren, &ndash; er Benachrichtigungs Meldungen per GCM an **ClientApp** sendet.

#### <a name="add-the-jsonnet-package"></a>Hinzufügen des JSON.net-Pakets

In dieser Konsolen-APP wird eine JSON-Nutzlast mit der Benachrichtigungs Meldung, die an die Client-App gesendet werden soll, in einer JSON-Nutzlast aufgebaut. Wir verwenden das **JSON.net** -Paket in **messagesender** , um das Erstellen des JSON-Objekts zu vereinfachen, das von GCM benötigt wird. Klicken Sie in Visual Studio mit der rechten Maustaste auf **Verweise > nuget-Pakete verwalten...** ; Klicken Sie in Visual Studio für Mac mit der rechten Maustaste auf **Pakete > Pakete hinzufügen...** .

Suchen Sie nach dem **JSON.net** -Paket, und installieren Sie es im Projekt:

[![Installieren des JSON.net-Pakets](remote-notifications-with-gcm-images/4-add-json.net-sml.png)](remote-notifications-with-gcm-images/4-add-json.net.png#lightbox)

#### <a name="add-a-reference-to-systemnethttp"></a>Hinzufügen eines Verweises auf System .net. http

Wir müssen auch einen Verweis auf `System.Net.Http` hinzufügen, damit wir eine `HttpClient` zum Senden unserer Testnachricht an GCM instanziieren können. Klicken Sie im Projekt **messagesender** mit der rechten Maustaste auf **Verweise > fügen Sie einen Verweis hinzu** , und Scrollen Sie nach unten, bis **System .net. http**angezeigt wird. Stellen Sie ein Häkchen neben **System .net. http** ein, und klicken Sie auf **OK**.

#### <a name="implement-code-that-sends-a-test-message"></a>Implementieren von Code, der eine Test Nachricht sendet

Bearbeiten Sie in **messagesender** **Program.cs** , und ersetzen Sie den Inhalt durch den folgenden Code:

```csharp
using System;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading.Tasks;
using Newtonsoft.Json.Linq;

namespace MessageSender
{
    class MessageSender
    {
        public const string API_KEY = "YOUR_API_KEY";
        public const string MESSAGE = "Hello, Xamarin!";

        static void Main (string[] args)
        {
            var jGcmData = new JObject();
            var jData = new JObject();

            jData.Add ("message", MESSAGE);
            jGcmData.Add ("to", "/topics/global");
            jGcmData.Add ("data", jData);

            var url = new Uri ("https://gcm-http.googleapis.com/gcm/send");
            try
            {
                using (var client = new HttpClient())
                {
                    client.DefaultRequestHeaders.Accept.Add(
                        new MediaTypeWithQualityHeaderValue("application/json"));

                    client.DefaultRequestHeaders.TryAddWithoutValidation (
                        "Authorization", "key=" + API_KEY);

                    Task.WaitAll(client.PostAsync (url,
                        new StringContent(jGcmData.ToString(), Encoding.Default, "application/json"))
                            .ContinueWith(response =>
                            {
                                Console.WriteLine(response);
                                Console.WriteLine("Message sent: check the client device notification tray.");
                            }));
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Unable to send GCM message:");
                Console.Error.WriteLine(e.StackTrace);
            }
        }
    }
}
```

Ändern Sie im obigen Code *YOUR_API_KEY* in den API-Schlüssel für das Client-App-Projekt.

Dieser Test-App-Server sendet die folgende JSON-formatierte Nachricht an GCM:

```csharp
{
  "to": "/topics/global",
  "data": {
    "message": "Hello, Xamarin!"
  }
}
```

GCM leitet diese Nachricht ihrerseits an Ihre Client-App weiter. Erstellen Sie nun **messagesender** , und öffnen Sie ein Konsolenfenster, in dem wir es über die Befehlszeile ausführen können.

### <a name="try-it"></a>Probieren Sie es aus!

Nun können wir unsere Client-App testen. Wenn Sie einen Emulator verwenden oder wenn Ihr Gerät mit GCM über Wi-Fi kommuniziert, müssen Sie die folgenden TCP-Ports in der Firewall öffnen, damit GCM-Nachrichten durchgeführt werden können: 5228, 5229 und 5230.

Starten Sie Ihre Client-App, und beobachten Sie das Ausgabefenster. Nachdem der `RegistrationIntentService` erfolgreich ein Registrierungs Token von GCM empfangen hat, sollte im Ausgabefenster das Token mit der Protokoll Ausgabe wie folgt angezeigt werden:

```shell
I/RegistrationIntentService(16103): GCM Registration Token: eX9ggabZV1Q:APA91bHjBnQXMUeBOT6JDiLpRt8m2YWtY ...
```

An diesem Punkt ist die Client-App bereit, eine Remote Benachrichtigungs Meldung zu empfangen. Führen Sie in der Befehlszeile das Programm " **messagesender. exe** " aus, um eine "Hello, xamarin"-Benachrichtigungs Meldung an die Client-App zu senden.
Wenn Sie das Projekt " **messagesender** " noch nicht erstellt haben, können Sie dies jetzt tun.

Um " **messagesender. exe** " unter Visual Studio auszuführen, öffnen Sie eine Eingabeaufforderung, wechseln Sie in das Verzeichnis " **messagesender/bin/debug** ", und führen Sie den Befehl direkt aus:

```cmd
MessageSender.exe
```

Um " **messagesender. exe** " unter Visual Studio für Mac auszuführen, öffnen Sie eine Terminal Sitzung, wechseln Sie zu " **messagesender/bin/debug** ", und verwenden Sie Mono, um " **messagesender. exe** " auszuführen.

```bash
mono MessageSender.exe
```

Es kann bis zu einer Minute dauern, bis die Nachricht über GCM verteilt und wieder an Ihre Client-App weitergeleitet wird. Wenn die Nachricht erfolgreich empfangen wurde, sollte im Ausgabefenster die Ausgabe ähnlich der folgenden angezeigt werden:

```shell
D/MyGcmListenerService(16103): From:    /topics/global
D/MyGcmListenerService(16103): Message: Hello, Xamarin!
```

Außerdem sollten Sie bemerken, dass ein neues Benachrichtigungssymbol in der Info Leiste angezeigt wird:

[auf dem Gerät wird ![Benachrichtigungssymbol angezeigt.](remote-notifications-with-gcm-images/5-icon-appears-sml.png)](remote-notifications-with-gcm-images/5-icon-appears.png#lightbox)

Wenn Sie die Benachrichtigungsleiste öffnen, um Benachrichtigungen anzuzeigen, sollte die Remote Benachrichtigung angezeigt werden:

[Es wird ![Benachrichtigungs Meldung angezeigt.](remote-notifications-with-gcm-images/6-notification-in-tray-sml.png)](remote-notifications-with-gcm-images/6-notification-in-tray.png#lightbox)

Herzlichen Glückwunsch, Ihre APP hat ihre erste Remote Benachrichtigung erhalten.

Beachten Sie, dass GCM-Nachrichten nicht mehr empfangen werden, wenn die APP erzwungen wird. Zum Fortsetzen von Benachrichtigungen nach einem Erzwingen der Beendigung muss die APP manuell neu gestartet werden. Weitere Informationen zu dieser Android-Richtlinie finden Sie unter [Launch Controls on beendete Applications](https://developer.android.com/about/versions/android-3.1.html#launchcontrols) und this [Stack Overflow Post](https://stackoverflow.com/questions/5051687/broadcastreceiver-not-receiving-boot-completed/19856267#19856267).

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise werden die Schritte zum Implementieren von Remote Benachrichtigungen in einer xamarin. Android-Anwendung beschrieben. Es wurde beschrieben, wie Sie zusätzliche Pakete installieren, die für die GCM-Kommunikation benötigt werden, und wie Sie App-Berechtigungen für den Zugriff auf GCM-Server konfigurieren.
Es wurde ein Beispielcode bereitgestellt, der veranschaulicht, wie Sie das vorhanden sein von Google Play Services überprüfen, wie Sie einen Dienst für die Registrierungs Absicht und einen Instanz-ID-Listenerdienst implementieren, der mit GCM für ein Registrierungs Token verhandelt und wie ein GCM-Listener implementiert wird. Dienst, der Remote Benachrichtigungs Meldungen empfängt und verarbeitet. Schließlich haben wir ein Befehlszeilen-Testprogramm implementiert, mit dem Test Benachrichtigungen über GCM an unsere Client-App gesendet werden.

## <a name="related-links"></a>Verwandte Links

- [Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md)
