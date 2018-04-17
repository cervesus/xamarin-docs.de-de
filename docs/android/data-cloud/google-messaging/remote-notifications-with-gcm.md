---
title: Remote-Benachrichtigungen mit Google Cloud Messaging
description: Diese exemplarische Vorgehensweise enthält eine schrittweise Erklärung der Vorgehensweise Google Cloud Messaging verwenden, um remote Benachrichtigungen (so genannte Pushbenachrichtigungen) zu implementieren, in einer Anwendung Xamarin.Android. Es beschreibt die verschiedenen Klassen, die Sie implementieren müssen, um die Kommunikation mit Google Cloud Messaging (GCM), es wird erläutert, wie zum Festlegen von Berechtigungen in der Android-Manifest für den Zugriff auf GCM und End-to-End-messaging mit einem Test-Beispielprogramm veranschaulicht.
ms.prod: xamarin
ms.assetid: 4FC3C774-EF93-41B2-A81E-C6A08F32C09B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/12/2018
ms.openlocfilehash: f4a1451cb848f4da1f595c15d946f4e05292900d
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="remote-notifications-with-google-cloud-messaging"></a>Remote-Benachrichtigungen mit Google Cloud Messaging

_Diese exemplarische Vorgehensweise enthält eine schrittweise Erklärung der Vorgehensweise Google Cloud Messaging verwenden, um remote Benachrichtigungen (so genannte Pushbenachrichtigungen) zu implementieren, in einer Anwendung Xamarin.Android. Es beschreibt die verschiedenen Klassen, die Sie implementieren müssen, um die Kommunikation mit Google Cloud Messaging (GCM), es wird erläutert, wie zum Festlegen von Berechtigungen in der Android-Manifest für den Zugriff auf GCM und End-to-End-messaging mit einem Test-Beispielprogramm veranschaulicht._

> [!NOTE]
> GCM wurde ersetzt wurde, indem Sie [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) (FCM).
> GCM-Server und Client-APIs [sind veraltet](https://firebase.googleblog.com/2018/04/time-to-upgrade-from-gcm-to-fcm.html) und wird nicht mehr verfügbar sein so bald wie 11. April 2019.

## <a name="gcm-notifications-overview"></a>Übersicht über die GCM-Benachrichtigungen

In dieser exemplarischen Vorgehensweise erstellen wir eine Xamarin.Android-Anwendung, die Google Cloud Messaging (GCM) verwendet, um remote Benachrichtigungen zu implementieren (auch bekannt als *Pushbenachrichtigungen*). Implementieren wir die verschiedenen Absicht und Listener-Dienste, die GCM für remote-messaging verwenden, und wir werden unsere Implementierung mit ein Befehlszeilenprogramm, das einen Anwendungsserver simuliert testen. 

Beachten Sie, dass Firebase Cloud Messaging (FCM) der neuen Version von GCM &ndash; Google empfiehlt dringend, FCM statt GCM verwenden. Wenn Sie derzeit GCM verwenden, wird empfohlen, ein Upgrade auf FCM. Weitere Informationen zu FCM, finden Sie unter [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md). 

Bevor Sie mit dieser exemplarischen Vorgehensweise fortfahren können, müssen Sie die erforderlichen Anmeldeinformationen zur Verwendung von Google GCM-Server erwerben; Dieser Prozess wird erläutert, [Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md). Insbesondere müssen Sie ein *API-Schlüssel* und ein *Absender-ID* So fügen Sie den Beispielcode in dieser exemplarischen Vorgehensweise dargestellt. 

Wir verwenden die folgenden Schritte zum Erstellen einer Xamarin.Android GCM-fähigen Client-app:

1.  Installieren Sie zusätzlicher Pakete, die für die Kommunikation mit GCM-Server erforderlich.
2.  Konfigurieren Sie app-Berechtigungen für den Zugriff auf GCM-Server.
3.  Implementieren Sie Code zum Überprüfen auf des Vorhandensein von Google Play-Dienste. 
4.  Implementieren Sie eine beabsichtigte geräteregistrierungsdienst, die ein Token für die Registrierung mit GCM ausgehandelt.
5.  Implementieren Sie eine Instanz-ID Listenerdienst, der Registrierung token Updates von GCM überwacht.
6.  Implementieren Sie einen GCM-Listener-Dienst, der Remotenachrichten von app-Servers durch GCM empfängt.

Diese app verwendet eine neue GCM-Funktion genannt *Thema messaging*. Im Thema zu messaging, sendet der app-Servers eine Nachricht an ein Thema, anstatt auf eine Liste von einzelnen Geräten. Geräte, die für dieses Thema abonnieren können themennachrichten als Pushbenachrichtigungen empfangen. Weitere Informationen zu GCM Thema messaging, finden Sie in der Google [implementieren Thema Messaging](https://developers.google.com/cloud-messaging/topic-messaging). 

Wenn die Client-app bereit ist, implementieren wir eine c#-befehlszeilenanwendung, die eine Pushbenachrichtigung an unsere Client-app über GCM sendet. 

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

Um zu beginnen, erstellen wir eine neue leere Projektmappe aufgerufen **RemoteNotifications**. Als Nächstes fügen Sie ein neues Android-Projekt für diese Lösung auf der Grundlage der **Android-App** Vorlage. Wir nennen Sie dieses Projekt **ClientApp**. (Wenn Sie nicht mit dem Erstellen von Projekten Xamarin.Android vertraut sind, finden Sie unter [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md).) Die **ClientApp** Projekt enthält den Code für die Xamarin.Android-Clientanwendung, die remote Benachrichtungen per GCM empfängt. 

### <a name="add-required-packages"></a>Hinzufügen der erforderlichen Pakete

Bevor wir unsere Client-app-Code implementieren können, müssen wir mehrere Pakete installieren, die für die Kommunikation mit GCM verwendet wird. Außerdem müssen wir die Google Play Store-Anwendung auf Ihr Gerät hinzufügen, wenn es nicht bereits installiert ist.

#### <a name="add-the-xamarin-google-play-services-gcm-package"></a>Fügen Sie die Xamarin-Google Play Services GCM-Paket hinzu

Zum Empfangen von Nachrichten von Google Cloud Messaging, die [Google Play-Dienste](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Gcm/) Framework muss auf dem Gerät vorhanden sein. Ohne dieses Framework kann keine Android-Anwendung Nachrichten aus GCM-Server empfangen. Google Play-Dienste während der Ausführung im Hintergrund der Android-Gerät eingeschaltet wird nach Nachrichten vom GCM – überwacht. Wenn diese Nachrichten eingehen, wird von Google Play-Dienste konvertiert die Nachrichten in Intents und überträgt dann diese Prioritäten für Anwendungen, die für sie registriert haben. 

In Visual Studio mit der Maustaste **Verweise > NuGet-Pakete verwalten...** ; in Visual Studio für Mac, mit der rechten Maustaste **Pakete > Pakete hinzufügen...** . Suchen Sie nach **Xamarin Google Play-Dienste - GCM** und installieren Sie dieses Paket in der **ClientApp** Projekt: 

[![Installieren Google Play-Dienste](remote-notifications-with-gcm-images/1-google-play-services-sml.png)](remote-notifications-with-gcm-images/1-google-play-services.png#lightbox)

Bei der Installation **Xamarin Google Play-Dienste - GCM**, **Xamarin Google Play-Dienste - Basis** wird automatisch installiert. Wenn Sie eine Fehlermeldung erhalten, ändern Sie des Projekts *Minimum Android Ziel* außer auf einen Wert festlegen **kompilieren Sie mit der SDK-Version** , und wiederholen Sie die Installation von NuGet. 

Als Nächstes bearbeiten **MainActivity.cs** und fügen Sie die folgenden `using` Anweisungen:

```csharp
using Android.Gms.Common;
using Android.Util;
```

Dies stellt Typen in der Google wiedergeben Services GMS-Paket zur Verfügung getesteten Codes, und fügt-Protokollfunktion, mit der wir unsere Transaktionen mit GMS verfolgen. 

#### <a name="google-play-store"></a>Google Play Store

Um Nachrichten von GCM zu erhalten, muss die Google Play Store-Anwendung auf dem Gerät installiert werden. (Wenn eine Google Play-Anwendung auf einem Gerät installiert ist, Google Play Store auch, installiert ist es wahrscheinlich ist, dass es auf Ihrem Testgerät bereits installiert ist.) Eine Android-Anwendung kann keine Nachrichten von GCM empfangen, ohne Google Play. Wenn Sie noch nicht über die Google Play Store-app auf Ihrem Gerät installierten verfügen, besuchen Sie die [Google Play](https://support.google.com/googleplay) -Website herunterladen und installieren Google Play. 

Alternativ können Sie einen Android-Emulator Android 2.2 oder höher, anstatt ein Testgerät (Sie müssen keinen So installieren Sie Google Play Store auf Android-Emulator) ausgeführt. Jedoch wenn Sie einen Emulator verwenden, müssen Sie Wi-Fi-Verbindung mit GCM verwenden, und Sie müssen Sie mehrere Ports in Ihrer Wi-Fi-Firewall öffnen, wie später in dieser exemplarischen Vorgehensweise erläutert. 

### <a name="set-the-package-name"></a>Legen Sie den Paketnamen

In [Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md), wir für unsere GCM-fähige Anwendung einen Paketnamen angegeben (diese Paketname dient auch als die *Anwendungs-ID* , unsere API-Schlüssel und die Absender-ID zugeordnet ist). Wir öffnen Sie die Eigenschaften für die **ClientApp** Projekt, und legen Sie den Paketnamen in dieser Zeichenfolge. In diesem Beispiel legen wir den Paketnamen zu `com.xamarin.gcmexample`:

[![Der Paketname festlegen](remote-notifications-with-gcm-images/2-package-name-sml.png)](remote-notifications-with-gcm-images/2-package-name.png#lightbox)

Beachten Sie, dass die Client-app nicht mehr um ein Token für die Registrierung von GCM zu erhalten, wenn diese Paketname, das nicht *genau* entsprechen den Paketnamen, die wir in der Google-Entwicklerkonsole eingegeben. 

### <a name="add-permissions-to-the-android-manifest"></a>Berechtigungen auf der Android-Manifest hinzufügen

Eine Android-Anwendung benötigen die folgenden Berechtigungen konfiguriert sein, bevor es von Google Cloud Messaging-Benachrichtigungen empfangen kann: 

-   `com.google.android.c2dm.permission.RECEIVE` &ndash; Gewährt die Berechtigung zum Registrieren und Empfangen von Nachrichten von Google Cloud Messaging unserer app. (Wirkungsweise `c2dm` bedeuten? Dies steht für _Cloud Device Messaging_, die nun veraltete Vorgänger von GCM ist. 
    GCM verwendet weiterhin `c2dm` in vielen der zugehörigen Berechtigung-Zeichenfolgen.) 

-   `android.permission.WAKE_LOCK` &ndash; (Optional) Verhindert, dass das Gerät aus CPU beim Empfang einer Nachricht in den Ruhezustand versetzt. 

-   `android.permission.INTERNET` &ndash; Gewährt Zugriff auf das Internet, sodass die Client-app mit GCM kommunizieren kann. 

-   *Paketname* `.permission.C2D_MESSAGE` &ndash; registriert die Anwendung mit Android und fordert die Berechtigung alle C2D ausschließlich empfangen Nachrichten (Cloud Gerät). Die *Paketname* Präfix ist identisch mit Ihrer Anwendung-ID. 

In der Android-Manifest setzen wir diese Berechtigungen. Ermöglicht das Bearbeiten **AndroidManifest.xml** und Ersetzen Sie den Inhalt durch folgendes XML: 

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

Ändern Sie im obigen XML *YOUR_PACKAGE_NAME* , der Paketname für das Client-app-Projekt. Beispielsweise `com.xamarin.gcmexample`. 

### <a name="check-for-google-play-services"></a>Prüfung auf Google Play-Dienste

In dieser exemplarischen Vorgehensweise erstellen wir eine minimalen-app mit einem einzelnen `TextView` in der Benutzeroberfläche. Diese app nicht die Interaktion mit GCM direkt anzugeben. Es wird stattdessen sehen Sie sich im Ausgabefenster angezeigt wie unserer app-Handshakes mit GCM, und wir prüfen Taskleiste für neue Benachrichtigungen, wenn diese eingehen. 

Zunächst erstellen Sie ein Layout für den Bereich. Bearbeiten Sie **Resources.layout.Main.axml** und Ersetzen Sie den Inhalt durch folgendes XML: 

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

Speichern Sie **Main.axml** und schließen Sie sie.

Die Client-app gestartet wird, möchten wir sie um sicherzustellen, dass Google Play-Dienste verfügbar ist, bevor Sie versuchen, wenden Sie sich an der GCM. Bearbeiten Sie **MainActivity.cs** , und Ersetzen Sie die ``count`` -Instanz Variablendeklaration mit der folgenden Variablendeklaration Instanz: 

```csharp
TextView msgText;
```

Fügen Sie die folgende Methode, die die **Activity\_main** Klasse: 

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

Dieser Code überprüft, ob das Gerät, um festzustellen, ob die Google wiedergeben Services APK installiert ist. Wenn es nicht installiert ist, wird eine Meldung in den Bereich angezeigt, die durch die der Benutzer ein APK aus dem Google Play Store herunterladen (oder aktivieren Sie ihn in das Gerät den Systemeinstellungen). Da diese Überprüfung ausgeführt werden, wenn die Client-app wird gestartet werden soll, müssen wir einen Aufruf dieser Methode hinzufügen, am Ende der `OnCreate`. 


Als Nächstes ersetzen die `OnCreate` -Methode durch folgenden Code:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    SetContentView (Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);

    IsPlayServicesAvailable ();
}
```

Dieser Code überprüft das Vorhandensein von Google wiedergeben Services-APK und schreibt das Ergebnis in den Bereich. 

Wir vollständig neu erstellen und Ausführen der app. Daraufhin sollte einen Bildschirm, der dem folgenden Screenshot ähnelt: 

[![Google Play-Dienste zur Verfügung steht](remote-notifications-with-gcm-images/3-first-screen-sml.png)](remote-notifications-with-gcm-images/3-first-screen.png#lightbox)

Wenn Sie dieses Ergebnis nicht erhalten, stellen Sie sicher, dass auf Ihrem Gerät wird und dass die Google wiedergeben Services APK installiert ist die **Xamarin Google Play-Dienste - GCM** Paket hinzugefügt wird Ihre **ClientApp** Projekt wie beschrieben weiter oben. Wenn Sie einen Build-Fehlermeldung erhalten, versuchen Sie es, bereinigen die Projektmappe, und das Projekt erneut erstellen. 

Als Nächstes fügen wir Code schreiben, um wenden Sie sich an GCM und erhalten Registrierung-Token.

### <a name="register-with-gcm"></a>Registrieren bei GCM

Bevor die app aus dem app-Server die remote-Benachrichtigungen empfangen kann, muss er mit GCM registrieren und rufen Sie eine Registrierung Token zurück. Die Arbeit der Anwendung mit GCM-Registrierung erfolgt durch eine `IntentService` , die wir erstellen. Unsere `IntentService` führt die folgenden Schritte aus: 

1.  Verwendet die [InstanceID](https://developers.google.com/instance-id/) -API, um Sicherheitstoken zu generieren, die unsere Client-app Zugriff auf den app-Server zu autorisieren. Im Gegenzug erhalten Registrierung ein token von GCM.

2.  Die Registrierung Token an den Anwendungsserver weiterleitet, (wenn es der app-Servers ist erforderlich).

3.  Mindestens ein Thema benachrichtigungskanäle abonniert.

Nachdem wir dies implementieren `IntentService`, wir testen, um festzustellen, ob es eine Registrierung token aus GCM abzurufen.

Fügen Sie eine neue Datei namens **RegistrationIntentService.cs** und Ersetzen Sie den Vorlagencode durch Folgendes:


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

Ändern Sie in der obigen Beispielcode *YOUR_SENDER_ID* , die Absender-ID-Nummer für das Client-app-Projekt. Beim Abrufen der Absender-ID für Ihr Projekt: 

1.  Melden Sie sich bei der [Konsole der Google Cloud](https://console.cloud.google.com/) , und wählen Sie den Projektnamen Ihres aus der Pull-Menü. In der **Projekt Info** Bereich, der für das Projekt angezeigt wird, klicken Sie auf **wechseln Sie in den projekteinstellungen**:

    [![XamarinGCM Projekt auswählen](remote-notifications-with-gcm-images/7-choose-project-sml.png)](remote-notifications-with-gcm-images/7-choose-project.png#lightbox)

2.  Auf der **Einstellungen** Seite, suchen Sie nach der **Projektnummer** &ndash; Dies ist die Absender-ID für Ihr Projekt:

    [![Projektnummer angezeigt](remote-notifications-with-gcm-images/9-project-number-sml.png)](remote-notifications-with-gcm-images/9-project-number.png#lightbox)

Wir beginnen möchten unsere `RegistrationIntentService` unserer app gestartet wird, ausgeführt wird. Bearbeiten **MainActivity.cs** und Ändern der `OnCreate` Methode so, dass unsere `RegistrationIntentService` wird gestartet, nachdem es das Vorhandensein von Google Play-Dienste überprüft: 

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

Nun schauen wir uns jeder Abschnitt des `RegistrationIntentService` zu verstehen, wie sie funktioniert. 

Zunächst wird mit einer Anmerkung versehen unserer `RegistrationIntentService` mit dem folgenden Attribut, um anzugeben, dass unsere Dienst nicht vom System instanziiert werden: 

```csharp
[Service (Exported = false)]
```

Die `RegistrationIntentService` Konstruktor benennt den Arbeitsthread *RegistrationIntentService* können das debugging erleichtern. 

```csharp
public RegistrationIntentService() : base ("RegistrationIntentService") { }
```

Die Kernfunktionen von `RegistrationIntentService` befindet sich in der `OnHandleIntent` Methode. Wir führen Sie durch diesen Code, um festzustellen, wie sie unsere app bei GCM registriert.


##### <a name="request-a-registration-token"></a>Anfordern eines Tokens für die Registrierung

`OnHandleIntent` Ruft zuerst Googles [InstanceID.GetToken](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID.html#getToken&#40;java.lang.String,%20java.lang.String&#41;) Methode, um ein Token für die Registrierung von GCM anfordern. Wir diesen Code im umschließen einer `lock` zum Schutz gegen die Möglichkeit, mehrere gleichzeitige Registrierung-Intents &ndash; der `lock` wird sichergestellt, dass diese Intents nacheinander verarbeitet werden. Wenn ein beim Abrufen eines Tokens für die Registrierung Fehler, wird eine Ausnahme ausgelöst, und wir einen Fehler melden. Wenn die Registrierung erfolgreich ist, `token` auf die Registrierung Token wir wieder von GCM haben festgelegt ist: 

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

##### <a name="forward-the-registration-token-to-the-app-server"></a>Das Token für die Registrierung mit dem App-Server weiterleiten

Wenn wir ein Token für die Registrierung erhalten (d. h. keine Ausnahme wurde ausgelöst), nennen wir `SendRegistrationToAppServer` Zuordnen des Benutzers Registrierung token mit dem Konto serverseitige (sofern vorhanden), bleibt durch die Anwendung. Da diese Implementierung für den Entwurf des app-Servers abhängig ist, wird hier eine leere Methode bereitgestellt: 

```csharp
void SendRegistrationToAppServer (string token)
{
    // Add custom implementation here as needed.
}
```

In einigen Fällen benötigt die app-Servers nicht Registrierung-Token des Benutzers. In diesem Fall kann diese Methode ausgelassen werden. Wenn eine Registrierung-Token an den Anwendungsserver gesendet wird `SendRegistrationToAppServer` sollten beibehalten, einen booleschen Wert ab, der angibt, ob das Token an den Server gesendet wurde. Wenn dieser Wert auf "false" ist `SendRegistrationToAppServer` sendet das Token an den Anwendungsserver &ndash; , andernfalls das Token an den Anwendungsserver in einem vorherigen Aufruf bereits gesendet wurde. 


##### <a name="subscribe-to-the-notification-topic"></a>Das Benachrichtigungsthema abonnieren

Wir rufen Sie als Nächstes unsere `Subscribe` Methode, um anzugeben, GCM, die wir auf ein Benachrichtigungsthema abonnieren möchten. In `Subscribe`, nennen wir die [GcmPubSub.Subscribe](https://developers.google.com/android/reference/com/google/android/gms/gcm/GcmPubSub.html#subscribe&#40;java.lang.String,%20java.lang.String,%20android.os.Bundle&#41;) -API, um unsere Client-app unter allen Nachrichten abonnieren `/topics/global`:

```csharp
void Subscribe (string token)
{
    var pubSub = GcmPubSub.GetInstance(this);
    pubSub.Subscribe(token, "/topics/global", null);
}
```

App-Servers muss benachrichtigungsmeldungen an senden `/topics/global` Wenn wir diese empfangen werden. Beachten Sie, die im Thema benennt unter `/topics` kann beliebig, solange der app-Servers und der Client-app auf diesen Namen akzeptieren. (Hier haben wir den Namen `global` , um anzugeben, dass zum Empfangen von Nachrichten in allen Themen, die von der app-Server unterstützt werden soll.) 

Informationen zu GCM-Thema auf der Serverseite messaging finden Sie in Google [senden zu Themen Messaging](https://developers.google.com/cloud-messaging/topic-messaging). 


#### <a name="implement-an-instance-id-listener-service"></a>Implementieren Sie eine Instanz-ID Listenerdienst

Registrierungstoken sind eindeutig und sicher. Allerdings kann die Client-app (oder GCM) müssen den Registrierung-Token im Fall von app-Neuinstallation oder ein Sicherheitsproblem aktualisieren. Aus diesem Grund implementieren wir ein `InstanceIdListenerService` , die Aktualisierung des Zugriffstokens Anfragen von GCM reagiert. 

Fügen Sie eine neue Datei namens **InstanceIdListenerService.cs** und Ersetzen Sie den Vorlagencode durch Folgendes: 

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

Mit einer Anmerkung versehen `InstanceIdListenerService` mit dem folgenden Attribut, um anzugeben, dass der Dienst wird nicht vom System instanziiert werden, sodass es GCM-Registrierung Token empfangen kann (so genannte *Instanz-ID*) aktualisieren von Anforderungen: 

```csharp
[Service(Exported = false), IntentFilter(new[] { "com.google.android.gms.iid.InstanceID" })]
```

Die `OnTokenRefresh` Methode in unseren Dienst startet die `RegistrationIntentService` , damit das neue Registrierung Token abgefangen werden können.


#### <a name="test-registration-with-gcm"></a>Testen Sie die Registrierung mit GCM

Wir vollständig neu erstellen und Ausführen der app. Wenn Sie erfolgreich ein Token für die Registrierung von GCM erhalten, sollte die Registrierung-Token im Ausgabefenster angezeigt werden. Zum Beispiel: 

```shell
D/Mono    ( 1934): Assembly Ref addref ClientApp[0xb4ac2400] -> Xamarin.GooglePlayServices.Gcm[0xb4ac2640]: 2
I/RegistrationIntentService( 1934): Calling InstanceID.GetToken
I/RegistrationIntentService( 1934): GCM Registration Token: f8LdveCvXig:APA91bFIsjUAbP-V8TPQdLR89qQbEJh1SYG38AcCbBUf34z5gSdUc5OsXrgs93YFiGcRSRafPfzkz23lf3-LvYV1CwrFheMjHgwPeFSh12MywnRIhz

```

### <a name="handle-downstream-messages"></a>Nachfolgende Nachrichten behandeln 

Der Code, den wir bisher implementiert haben, wird nur "Setup" Code; Er überprüft, um festzustellen, ob es sich bei Google Play-Dienste installiert ist und verhandelt mit GCM und app-Servers unserer Client-app für den Empfang von Benachrichtigungen remote vorbereiten. Allerdings haben wir noch Code zu implementieren, die tatsächlich empfangen und Verarbeiten von benachrichtigungsmeldungen downstream. Zu diesem Zweck implementieren wir ein *GCM-Listenerdienst*. Dieser Dienst empfängt Thema Nachrichten aus dem app-Server und überträgt diese lokal als Benachrichtigungen. Nachdem wir diesen Dienst implementiert haben, erstellen wir ein Testprogramm zum Senden von Nachrichten an GCM, sodass wir sehen können, wenn die Implementierung ordnungsgemäß funktioniert. 


#### <a name="add-a-notification-icon"></a>Fügen Sie einem Symbol hinzu.

Zunächst fügen wir ein kleines Symbol, das im Infobereich angezeigt wird, wenn unsere Benachrichtigung gestartet wird. Sie können kopieren [dieses Symbol](remote-notifications-with-gcm-images/ic-stat-ic-notification.png) zu Ihrem Projekt oder ein eigene benutzerdefinierte Symbol "erstellen". Den Namen der Symboldatei **ic_stat_button_click.png** und kopieren Sie sie in der **Ressourcen und Ausgaben möglich** Ordner. Denken Sie daran, verwenden Sie **hinzufügen > Vorhandenes Element...**  auf dieses Symboldatei im Projekt enthalten.


#### <a name="implement-a-gcm-listener-service"></a>Implementieren eines GCM-Listener-Diensts

Fügen Sie eine neue Datei namens **GcmListenerService.cs** und Ersetzen Sie den Vorlagencode durch Folgendes:

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

Werfen wir einen Blick auf die einzelnen Abschnitte unsere `GcmListenerService` zu verstehen, wie sie funktioniert. 

Zunächst wird mit einer Anmerkung versehen `GcmListenerService` mit einem Attribut, um anzugeben, dass dieser Dienst ist nicht für vom System instanziiert werden, und wir einen beabsichtigten Filter schließen, um anzugeben, dass es GCM-Nachrichten empfängt: 

```csharp
[Service (Exported = false), IntentFilter (new [] { "com.google.android.c2dm.intent.RECEIVE" })]
```

Wenn `GcmListenerService` empfängt eine Nachricht vom GCM, die `OnMessageReceived` Methode aufgerufen wird. Diese Methode extrahiert den Nachrichteninhalt aus dem übergebenen `Bundle`, protokolliert den Nachrichteninhalt (damit wir ihn im Ausgabefenster anzeigen können), und ruft `SendNotification` um eine lokale Benachrichtigung mit dem Inhalt der empfangenen Nachricht zu starten: 

```csharp
var message = data.GetString ("message");
Log.Debug ("MyGcmListenerService", "From:    " + from);
Log.Debug ("MyGcmListenerService", "Message: " + message);
SendNotification (message);
```

Die `SendNotification` -Methode verwendet `Notification.Builder` verwendet zum Erstellen der Benachrichtigung, und klicken Sie dann die `NotificationManager` um die Benachrichtigung zu starten. Effektiv, konvertiert diese die remote-Nachricht in eine lokale Benachrichtigung an den Benutzer angezeigt werden.
Weitere Informationen zur Verwendung von `Notification.Builder` und `NotificationManager`, finden Sie unter [lokale Benachrichtigungen](~/android/app-fundamentals/notifications/local-notifications.md).


#### <a name="declare-the-receiver-in-the-manifest"></a>Deklarieren Sie die Empfänger im Manifest

Bevor wir von GCM Nachrichten empfangen kann, muss die GCM-Listener in der Android-Manifest deklariert werden. Ermöglicht das Bearbeiten **AndroidManifest.xml** , und Ersetzen Sie die `<application>` Abschnitt mit den folgenden XML-Code: 

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

Ändern Sie im obigen XML *YOUR_PACKAGE_NAME* , der Paketname für das Client-app-Projekt. In unserem Beispiel Exemplarische Vorgehensweise der Paketname ist `com.xamarin.gcmexample`. 

Sehen wir uns, was bewirkt, dass jede Einstellung in dieser XML-Code:

|Einstellung|Beschreibung|
|---|---|
|`com.google.android.gms.gcm.GcmReceiver`|Deklariert, dass unsere app einen GCM-Empfänger implementiert, der erfasst und verarbeitet eingehende Nachrichten der Push-Benachrichtigung.|
|`com.google.android.c2dm.permission.SEND`|Deklariert, dass nur GCM-Server Nachrichten direkt an die Anwendung senden können.|
|`com.google.android.c2dm.intent.RECEIVE`|Beabsichtigte Filter ankündigt, dass unsere app Broadcastmeldungen von GCM behandelt.|
|`com.google.android.c2dm.intent.REGISTRATION`|Beabsichtigte Filter ankündigt, dass unsere app neue Registrierung Intents verarbeitet (Wir haben eine Instanz-ID-Listenerdienst implementiert).|

Sie können alternativ ergänzen `GcmListenerService` mit diesen Attributen, anstatt in XML; Angabe wir hier in **AndroidManifest.xml** , damit die Codebeispiele leichter zu befolgen sind. 


### <a name="create-a-message-sender-to-test-the-app"></a>Erstellen Sie den Absender einer Nachricht zum Testen der App

Sehen wir die Projektmappe ein C#-desktop Konsolenanwendungsprojekt hinzu, und rufen sie **MessageSender**. Wir verwenden diese Konsolenanwendung zum Simulieren von eines Anwendungsservers &ndash; Traffic Manager sendet benachrichtigungsmeldungen an **ClientApp** über GCM. 


#### <a name="add-the-jsonnet-package"></a>Fügen Sie das Paket Json.NET

In dieser Konsolen-app erstellen wir eine JSON-Nutzlast, die die benachrichtigungsmeldung enthält, die an die Client-app gesendet werden soll. Wir verwenden die **Json.NET** im Paket **MessageSender** zu vereinfachen, erstellen die JSON-Objekt, das durch GCM erforderlich. In Visual Studio mit der Maustaste **Verweise > NuGet-Pakete verwalten...** ; in Visual Studio für Mac, mit der rechten Maustaste **Pakete > Pakete hinzufügen...** . 

Suchen Sie nach der **Json.NET** Packen und im Projekt zu installieren: 

[![Installieren des Pakets Json.NET](remote-notifications-with-gcm-images/4-add-json.net-sml.png)](remote-notifications-with-gcm-images/4-add-json.net.png#lightbox)


#### <a name="add-a-reference-to-systemnethttp"></a>Hinzufügen eines Verweises auf System.Net.Http

Wir müssen auch einen Verweis hinzufügen `System.Net.Http` , damit wir instanziieren kann ein `HttpClient` für unsere Testnachricht an GCM senden. In der **MessageSender** -Projekt mit der rechten Maustaste **Verweise > Verweis hinzufügen** und einen Bildlauf nach unten, bis Sie finden Sie unter **System.Net.Http**. Setzen ein Häkchen neben **System.Net.Http** , und klicken Sie auf **OK**. 


#### <a name="implement-code-that-sends-a-test-message"></a>Implementieren Sie Code, die eine Testnachricht gesendet.

In **MessageSender**, bearbeiten **"Program.cs"** und Ersetzen Sie den Inhalt durch folgenden Code:

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

Ändern Sie im obigen Code *YOUR_API_KEY* auf den API-Schlüssel für das Client-app-Projekt. 

Dieser Test-app-Server sendet die folgende JSON-formatierte Meldung an GCM:

```csharp
{
  "to": "/topics/global",
  "data": {
    "message": "Hello, Xamarin!"
  }
}
```

GCM, werden wiederum diese Meldung, um Ihre Client-app weitergeleitet. Erstellen Sie **MessageSender** und eine Konsolenfenster, in dem wir es über die Befehlszeile ausführen können, zu öffnen.



### <a name="try-it"></a>Probieren Sie es aus!

Nun können wir unsere Client-app zu testen. Wenn Sie einen Emulator verwenden oder wenn Ihr Gerät mit GCM über Wi-Fi kommuniziert, Sie die folgenden TCP-Ports, in der Firewall für GCM-Nachrichten öffnen müssen über abrufen: 5228 5229 und 5230.

Starten Sie Ihre Client-app, und beobachten Sie das Fenster "Ausgabe". Nach der `RegistrationIntentService` erfolgreich empfängt eine Registrierung-token von GCM, Anzeigen des Ausgabefensters sollte das Token Ausgabeprotokoll wie den folgenden:

```shell
I/RegistrationIntentService(16103): GCM Registration Token: eX9ggabZV1Q:APA91bHjBnQXMUeBOT6JDiLpRt8m2YWtY ...
```

An diesem Punkt ist die Client-app zum Empfangen einer Nachricht für die remote-Benachrichtigung bereit. Führen Sie über die Befehlszeile der **MessageSender.exe** Programm, eine "Hello, Xamarin" Meldung an den Client-app zu senden.
Wenn Sie noch nicht erstellt haben die **MessageSender** Projekt, holen Sie dies jetzt.

Auszuführende **MessageSender.exe** unter Visual Studio, öffnen Sie eine Eingabeaufforderung, wechseln Sie in der **MessageSender/Bin/Debug** Verzeichnis, und führen Sie den Befehl direkt:

```cmd
MessageSender.exe
```

Auszuführende **MessageSender.exe** unter Visual Studio für Mac, öffnen Sie eine Terminalsitzung, wechseln Sie in **MessageSender/Bin/Debug** das Verzeichnis und die Verwendung Mono auszuführende **MessageSender.exe** 

```bash
mono MessageSender.exe
```

Es kann bis zu einer Minute für die Nachricht durch GCM und zurück zum Client-app verteilt dauern. Wenn die Nachricht erfolgreich empfangen wurde, sollte Ausgabe wie den folgenden im Ausgabefenster angezeigt werden: 

```shell
D/MyGcmListenerService(16103): From:    /topics/global
D/MyGcmListenerService(16103): Message: Hello, Xamarin!
```

Darüber hinaus sollten Sie feststellen, dass ein neuer Benachrichtigungssymbol in der Taskleiste angezeigt wird: 

[![Symbol "Notiication" wird auf Gerät angezeigt.](remote-notifications-with-gcm-images/5-icon-appears-sml.png)](remote-notifications-with-gcm-images/5-icon-appears.png#lightbox)

Wenn Sie auf der Taskleiste anzeigen von Benachrichtigungen öffnen, sehen Sie unsere remote-Benachrichtigung:

[![Meldung wird angezeigt.](remote-notifications-with-gcm-images/6-notification-in-tray-sml.png)](remote-notifications-with-gcm-images/6-notification-in-tray.png#lightbox)

Herzlichen Glückwunsch, Ihre app hat seine erste remote-Benachrichtigung erhalten!

Beachten Sie, dass GCM-Nachrichten nicht mehr empfangen soll, wenn die app Force beendet ist. Um Benachrichtigungen nach eine erzwungene Beendigung fortzusetzen, muss die app manuell neu gestartet werden. Weitere Informationen zu dieser Android-Richtlinie finden Sie unter [starten Sie die Steuerelemente auf die beendete Anwendungen](https://developer.android.com/about/versions/android-3.1.html#launchcontrols) und dadurch [stack Overflow Post](http://stackoverflow.com/questions/5051687/broadcastreceiver-not-receiving-boot-completed/19856267#19856267). 

 
## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise detailliert die Schritte zur Implementierung von remote-Benachrichtigungen in einer Anwendung Xamarin.Android. Er beschreibt, wie Sie zusätzliche für die GCM-Kommunikation benötigten Pakete installieren, und es wird erläutert, wie app-Berechtigungen für den Zugriff auf GCM-Server konfigurieren. Beispielcode, der veranschaulicht, wie überprüft das Vorhandensein von Google Play-Dienste, wie eine beabsichtigte geräteregistrierungsdienst und die Instanz-ID Listenerdienst, der für ein Token für die Registrierung mit GCM verhandelt implementiert und Gewusst wie: Implementieren eines GCM-Listeners darauf Dienst, der empfängt und verarbeitet Remoteserver benachrichtigungsmeldungen. Schließlich implementiert haben wir ein Befehlszeilen-Testprogramm zum Senden von testbenachrichtigungen an unsere Client-app durch GCM. 


## <a name="related-links"></a>Verwandte Links

- [GCM-RemoteNotifications (Beispiel)](https://developer.xamarin.com/samples/monodroid/RemoteNotifications)
- [Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md)
