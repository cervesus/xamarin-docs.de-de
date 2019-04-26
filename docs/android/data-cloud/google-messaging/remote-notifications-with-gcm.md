---
title: Remotebenachrichtigungen mit Google Cloud Messaging
description: Diese exemplarische Vorgehensweise enthält eine schrittweise Erklärung der Vorgehensweise für die Google Cloud Messaging verwenden, um remotebenachrichtigungen (auch als "Pushbenachrichtigungen" bezeichnet) implementieren in einer Xamarin.Android-Anwendung. Es wird beschrieben, die verschiedene Klassen, die Sie implementieren müssen, um die Kommunikation mit Google Cloud Messaging (GCM), es wird erläutert, wie zum Festlegen von Berechtigungen im Android-Manifest für den Zugriff auf den GCM und End-to-End-messaging mit einem Beispiel-Test-Programm veranschaulicht.
ms.prod: xamarin
ms.assetid: 4FC3C774-EF93-41B2-A81E-C6A08F32C09B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/12/2018
ms.openlocfilehash: e5a5e44a61d352b5de05564ebb7192d21ed83dfa
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61012751"
---
# <a name="remote-notifications-with-google-cloud-messaging"></a>Remotebenachrichtigungen mit Google Cloud Messaging

_Diese exemplarische Vorgehensweise enthält eine schrittweise Erklärung der Vorgehensweise für die Google Cloud Messaging verwenden, um remotebenachrichtigungen (auch als "Pushbenachrichtigungen" bezeichnet) implementieren in einer Xamarin.Android-Anwendung. Es wird beschrieben, die verschiedene Klassen, die Sie implementieren müssen, um die Kommunikation mit Google Cloud Messaging (GCM), es wird erläutert, wie zum Festlegen von Berechtigungen im Android-Manifest für den Zugriff auf den GCM und End-to-End-messaging mit einem Beispiel-Test-Programm veranschaulicht._

> [!NOTE]
> GCM wurde ersetzt wurde, indem [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) (FCM).
> GCM-Server und Client-APIs [sind veraltet](https://firebase.googleblog.com/2018/04/time-to-upgrade-from-gcm-to-fcm.html) und wird nicht mehr zur Verfügung so schnell wie vom 11. April 2019.

## <a name="gcm-notifications-overview"></a>Übersicht über die GCM-Benachrichtigungen

In dieser exemplarischen Vorgehensweise erstellen wir eine Xamarin.Android-Anwendung, die Google Cloud Messaging (GCM) verwendet, um remotebenachrichtigungen zu implementieren (auch bekannt als *Pushbenachrichtigungen*). Implementieren wir die verschiedenen Intents und -Listener-Dienste, die GCM für remote-messaging verwenden, und wir unsere Implementierung mit einem Befehlszeilenprogramm, das einen Anwendungsserver simuliert testen. 

Beachten Sie, dass Firebase Cloud Messaging (FCM) die neue Version von GCM &ndash; Google empfiehlt dringend, statt FCM GCM. Wenn Sie derzeit GCM verwenden, wird empfohlen, ein Upgrade auf FCM. Weitere Informationen zu FCM, finden Sie unter [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md). 

Bevor Sie mit dieser exemplarischen Vorgehensweise fortfahren können, müssen Sie die erforderlichen Anmeldeinformationen zum Verwenden Googles GCM-Server abrufen; Dieser Prozess wird erläutert, [Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md). Insbesondere müssen Sie eine *API-Schlüssel* und *Absender-ID* zum Einfügen in den Beispielcode in dieser exemplarischen Vorgehensweise angezeigt. 

Wir verwenden die folgenden Schritte aus, um eine GCM-fähigen Xamarin.Android-Client-app zu erstellen:

1.  Installieren Sie zusätzlicher Pakete, die für die Kommunikation mit GCM-Server erforderlich.
2.  Konfigurieren Sie app-Berechtigungen für den Zugriff auf den GCM-Server.
3.  Implementieren Sie Code zum Überprüfen auf das Vorhandensein von Google Play-Dienste. 
4.  Implementieren Sie einen Registrierung intent-Dienst, der für ein Registrierungstoken bei GCM handelt.
5.  Implementieren Sie einen Instanz-ID-Listenerdienst, der token registrierungsaktualisierungen von GCM lauscht.
6.  Implementieren Sie einen GCM-Listenerdienst, der remote-Nachrichten von der app-Server über GCM erhält.

Diese app verwendet eine neue GCM-Funktion, die als bekannt *Thema messaging*. Im Thema zu messaging, sendet der app-Server eine Nachricht an, an ein Thema, anstatt eine Liste mit den einzelnen Geräten. Geräte, die für dieses Thema abonnieren können Thema Nachrichten als Pushbenachrichtigungen empfangen. Weitere Informationen zu den GCM-Thema messaging, finden Sie unter Google [implementieren Thema Messaging](https://developers.google.com/cloud-messaging/topic-messaging). 

Wenn die Client-app bereit ist, implementieren wir über eine Befehlszeile C# Anwendung, die eine Pushbenachrichtigung an die Client-app über GCM sendet. 

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

Zunächst erstellen wir eine neue leere Projektmappe namens **RemoteNotifications**. Als Nächstes fügen Sie ein neues Android-Projekt mit dieser Lösung auf der Grundlage der **Android-App** Vorlage. Wir nennen dieses Projekt **ClientApp**. (Wenn Sie nicht mit dem Erstellen von Xamarin.Android-Projekte vertraut sind, finden Sie unter [Hallo, Android](~/android/get-started/hello-android/hello-android-quickstart.md).) Die **ClientApp** Projekt enthält den Code für die Xamarin.Android-Client-Anwendung, die über GCM remotebenachrichtigungen empfängt. 

### <a name="add-required-packages"></a>Hinzufügen von erforderlichen Paketen

Bevor wir unsere Client-app-Code implementieren können, müssen wir mehrere Pakete installieren, die wir für die Kommunikation mit GCM verwenden. Darüber hinaus müssen wir die Google Play Store-Anwendung auf Ihr Gerät hinzufügen, wenn es nicht bereits installiert ist.

#### <a name="add-the-xamarin-google-play-services-gcm-package"></a>Fügen Sie das Xamarin Google Play Services GCM-Paket hinzu.

Zum Empfangen von Nachrichten aus Google Cloud Messaging, die [Google Play Services](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Gcm/) Framework muss auf dem Gerät vorhanden sein. Eine Android-Anwendung kann keine Nachrichten von GCM-Servern empfangen, ohne dieses Framework. Google Play-Dienste während der Ausführung im Hintergrund der Android-Gerät eingeschaltet wird, automatisch Lauschen auf Nachrichten von GCM. Wenn diese Nachrichten eingehen, wird Google Play Services konvertiert die Nachrichten in Intents und sendet dann diese Intent-Elemente für Anwendungen, die für sie registriert haben. 

In Visual Studio mit der Maustaste **Verweise > NuGet-Pakete verwalten...** ; in Visual Studio für Mac, mit der rechten Maustaste **Pakete > Pakete hinzufügen...** . Suchen Sie nach **Xamarin Google Play Services - GCM** und installieren Sie dieses Paket in der **ClientApp** Projekt: 

[![Installieren von Google Play-Dienste](remote-notifications-with-gcm-images/1-google-play-services-sml.png)](remote-notifications-with-gcm-images/1-google-play-services.png#lightbox)

Bei der Installation **Xamarin Google Play Services - GCM**, **Xamarin Google Play Services - Basis** wird automatisch installiert. Wenn Sie eine Fehlermeldung erhalten, Ändern des Projekts *Android-Mindestversion* außer auf einen Wert festlegen **Kompilieren mit der SDK-Version** , und wiederholen Sie die NuGet-Installation. 

Als Nächstes bearbeiten **"mainactivity.cs"** und fügen Sie die folgenden `using` Anweisungen:

```csharp
using Android.Gms.Common;
using Android.Util;
```

Dies macht Typen im Google Play Services GMS Paket unserem Code zur Verfügung, und Protokollierungsfunktion bereit, die wir verwenden unsere Transaktionen mit GMS nachverfolgen hinzugefügt. 

#### <a name="google-play-store"></a>Google Play Store

Um Nachrichten von GCM empfangen zu können, muss die Google Play Store-Anwendung auf dem Gerät installiert werden. (Wenn eine Google Play-Anwendung auf einem Gerät installiert ist, wird Google Play Store auch installiert, kann es sein, dass es auf Ihrem Testgerät bereits installiert ist,.) Eine Android-Anwendung kann keine Nachrichten von GCM empfangen, ohne Sie zu Google Play. Wenn Sie noch nicht über die Google Play Store-app auf Ihrem Gerät installierten verfügen, besuchen Sie die [Google Play](https://support.google.com/googleplay) -Website herunterladen und installieren Google Play. 

Alternativ können Sie einen Android-Emulator Android 2.2 oder höher, anstatt ein Testgerät (Sie müssen keinen So installieren Sie Google Play Store auf einem Android-Emulator) ausführen. Jedoch wenn Sie einen Emulator verwenden, müssen Sie Wi-Fi-Verbindung mit GCM verwenden, und Sie müssen Sie mehrere Ports in der Wi-Fi-Firewall öffnen, wie weiter unten in dieser exemplarischen Vorgehensweise erläutert. 

### <a name="set-the-package-name"></a>Legen Sie den Paketnamen

In [Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md), wir einen Paketnamen für unser GCM-fähige Anwendung angegeben (diese Paketname dient auch als die *Anwendungs-ID* , unsere API-Schlüssel und die Absender-ID zugeordnet ist). Öffnen wir die Eigenschaften für die **ClientApp** Projekt, und legen Sie den Paketnamen in dieser Zeichenfolge. In diesem Beispiel legen wir den Paketnamen `com.xamarin.gcmexample`:

[![Festlegen der Paketname](remote-notifications-with-gcm-images/2-package-name-sml.png)](remote-notifications-with-gcm-images/2-package-name.png#lightbox)

Beachten Sie, dass die Client-app kann dann ein Registrierungstoken von GCM empfangen, wenn dieser Paketname nicht der Fall ist *genau* entsprechen den Paketnamen an, die wir in der Google-Entwicklerkonsole eingegeben. 

### <a name="add-permissions-to-the-android-manifest"></a>Hinzufügen von Berechtigungen zur Android-Manifest

Eine Android-Anwendung müssen die folgenden Berechtigungen, die konfiguriert werden, damit sie Benachrichtigungen von Google Cloud Messaging erhalten kann: 

-   `com.google.android.c2dm.permission.RECEIVE` &ndash; Gewährt die Berechtigung zum Registrieren und Empfangen von Nachrichten aus Google Cloud Messaging für die app. (Funktionsweise `c2dm` bedeuten? Dies steht für _Cloud-zu-Gerät-Messaging_, der jetzt veralteten Vorgänger GCM ist. 
    GCM verwendet weiterhin `c2dm` in vielen der Zeichenfolgen Berechtigung.) 

-   `android.permission.WAKE_LOCK` &ndash; (Optional) Verhindert, dass das Gerät CPU in den Standbymodus wechseln beim Empfang einer Nachricht. 

-   `android.permission.INTERNET` &ndash; Gewährt Zugriff auf das Internet, damit die Client-app bei GCM kommunizieren kann. 

-   *Paketname* `.permission.C2D_MESSAGE` &ndash; registriert die Anwendung mit Android und fordert die Berechtigung zum Empfangen von ausschließlich alle C2D-Nachrichten (Cloud-zu-Gerät). Die *Package_name* Präfix ist identisch mit Ihrem Anwendungs-ID. 

Wir werden diese Berechtigungen im Android-Manifest festlegen. Ermöglicht das Bearbeiten von **"androidmanifest.xml"** und Ersetzen Sie den Inhalt durch folgendes XML: 

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

Ändern Sie im obigen XML *YOUR_PACKAGE_NAME* an den Paketnamen für Ihre Client-app-Projekt. Beispielsweise `com.xamarin.gcmexample`. 

### <a name="check-for-google-play-services"></a>Überprüfung auf Google Play-Dienste

In dieser exemplarischen Vorgehensweise erstellen wir eine app auf das Notwendigste mit einem einzelnen `TextView` in der Benutzeroberfläche. Diese app Interaktion mit GCM nicht direkt angegeben werden. Wir werden stattdessen sehen Sie sich im Ausgabefenster angezeigt wie unsere app-Handshakes mit GCM, wir Prüfen der Komponentenleiste Benachrichtigung für neue Benachrichtigungen, wenn diese eingehen. 

Zunächst erstellen wir ein Layout für den Bereich. Bearbeiten Sie **Resources.layout.Main.axml** und Ersetzen Sie den Inhalt durch folgendes XML: 

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

Wenn die Client-app gestartet wird, möchten wir sie, um sicherzustellen, dass die Google Play Services verfügbar ist, bevor wir GCM wenden Sie sich an. Bearbeiten Sie **"mainactivity.cs"** , und Ersetzen Sie die ``count`` Variablendeklaration mit der folgenden Instanz Variablendeklaration-Instanz: 

```csharp
TextView msgText;
```

Fügen Sie die folgende Methode der **MainActivity** Klasse: 

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

Dieser Code überprüft das Gerät aus, um festzustellen, ob die Google Play Services-APK installiert ist. Wenn es nicht installiert ist, wird eine Meldung im Nachrichtenbereich angezeigt, die der Benutzer ein APK aus dem Google Play Store herunterladen (oder in den Systemeinstellungen des Geräts aktivieren). Denn wir möchten diese Prüfung ausgeführt, wenn die Client-app gestartet wird, werden wir einen Aufruf dieser Methode fügen Sie am Ende der `OnCreate`. 


Ersetzen Sie als Nächstes die `OnCreate` Methode durch den folgenden Code:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    SetContentView (Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);

    IsPlayServicesAvailable ();
}
```

Dieser Code überprüft das Vorhandensein der Google Play Services apk-Datei und schreibt das Ergebnis in den Bereich. 

Lassen Sie uns vollständig neu zu erstellen und Ausführen der app. Sie sollten ein Bildschirm angezeigt, der wie im folgenden Screenshot aussieht: 

[![Google Play Services ist verfügbar](remote-notifications-with-gcm-images/3-first-screen-sml.png)](remote-notifications-with-gcm-images/3-first-screen.png#lightbox)

Wenn Sie dieses Ergebnis nicht erhalten, stellen Sie sicher, dass die Google Play Services-APK auf dem Gerät und, installiert ist die **Xamarin Google Play Services - GCM** -Paket hinzugefügt wird Ihre **ClientApp** Projekt wie beschrieben weiter oben. Wenn Sie einen Buildfehler erhalten, versuchen Sie es die Projektmappe zu bereinigen und das Projekt erneut zu erstellen. 

Als Nächstes schreiben wir Code, um wenden Sie sich an GCM und ein Registrierungstoken zu erhalten.

### <a name="register-with-gcm"></a>Bei GCM registrieren

Bevor die app aus der app-Server die remotebenachrichtigungen empfangen kann, müssen sie bei GCM registrieren und ein Registrierungstoken erhalten. Die Arbeit von der Anwendung bei GCM registrieren erfolgt durch ein `IntentService` , die wir erstellen. Unsere `IntentService` führt die folgenden Schritte aus: 

1.  Verwendet die [InstanceID](https://developers.google.com/instance-id/) -API, um Sicherheitstoken zu generieren, die unsere Client-app Zugriff auf den app-Server zu autorisieren. Im Gegenzug erhalten wir eine Registrierung token von GCM.

2.  Das Registrierungstoken für die app-Server weitergeleitet, (wenn der Server app es erfordert).

3.  Eine oder mehrere Thema benachrichtigungskanäle abonniert.

Nachdem wir dies implementieren `IntentService`, testen, um festzustellen, ob wir eine Registrierung token von GCM erhalten.

Fügen Sie eine neue Datei namens **RegistrationIntentService.cs** , und Ersetzen Sie den Vorlagencode durch den folgenden:


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

Ändern Sie in der obigen Beispielcode *YOUR_SENDER_ID* auf die Anzahl der Absender-ID für Ihre Client-app-Projekt. So erhalten Sie die Absender-ID für Ihr Projekt: 

1.  Melden Sie sich bei der [Google Cloud Console](https://console.cloud.google.com/) , und wählen Sie den Projektnamen aus dem Pulldownmenü. In der **Projekt Informationen** Bereich, der für Ihr Projekt angezeigt wird, klicken Sie auf **wechseln Sie auf die projekteinstellungen**:

    [![Auswählen des XamarinGCM-Projekts](remote-notifications-with-gcm-images/7-choose-project-sml.png)](remote-notifications-with-gcm-images/7-choose-project.png#lightbox)

2.  Auf der **Einstellungen** Seite, suchen Sie nach der **Projektnummer** &ndash; Dies ist die Absender-ID für Ihr Projekt:

    [![Projektnummer angezeigt](remote-notifications-with-gcm-images/9-project-number-sml.png)](remote-notifications-with-gcm-images/9-project-number.png#lightbox)

Wir möchten unsere `RegistrationIntentService` Wenn unsere app ausgeführt wird. Bearbeiten Sie **"mainactivity.cs"** und ändern Sie die `OnCreate` Methode, damit unsere `RegistrationIntentService` wird gestartet, nachdem es das Vorhandensein von Google Play Services überprüft: 

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

Nun werfen wir einen Blick auf die einzelnen Abschnitte des `RegistrationIntentService` zu verstehen, wie es funktioniert. 

Zuerst versehen wir unsere `RegistrationIntentService` mit dem folgenden Attribut, um anzugeben, dass der Dienst ist nicht für vom System instanziiert werden: 

```csharp
[Service (Exported = false)]
```

Die `RegistrationIntentService` Konstruktor benennt den Arbeitsthread *RegistrationIntentService* zum Debugging zu erleichtern. 

```csharp
public RegistrationIntentService() : base ("RegistrationIntentService") { }
```

Die Kernfunktionen von `RegistrationIntentService` befindet sich in der `OnHandleIntent` Methode. Betrachten wir nun diesen Code, um festzustellen, wie sie die app bei GCM registriert.


##### <a name="request-a-registration-token"></a>Anfordern eines Registrierungstokens

`OnHandleIntent` Ruft zunächst die Google [InstanceID.GetToken](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID.html#getToken&#40;java.lang.String,%20java.lang.String&#41;) Methode, um ein Registrierungstoken von GCM anfordern. Wir diesen Code in umschließen einer `lock` zum Schutz gegen die Möglichkeit, mehrere gleichzeitige Registrierung-Intents &ndash; der `lock` wird sichergestellt, dass diese Intent-Elemente nacheinander verarbeitet werden. Zum Abrufen eines registrierungstokens nicht, wird eine Ausnahme ausgelöst, und melden Sie sich einen Fehler. Wenn die Registrierung erfolgreich ist, `token` auf die wir wieder von GCM hier-Registrierungstoken festgelegt ist: 

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

##### <a name="forward-the-registration-token-to-the-app-server"></a>Weiterleiten Sie das Registrierungstoken wird an der App-Server.

Wenn wir ein Registrierungstoken zu erhalten (d. h. keine Ausnahme wurde ausgelöst), rufen wir `SendRegistrationToAppServer` Zuordnen des Benutzers Registrierung token mit dem serverseitigen-Konto (falls vorhanden) verwaltet wird von der Anwendung. Da diese Implementierung für den Entwurf des app-Servers abhängig ist, wird eine leere Methode hier bereitgestellt: 

```csharp
void SendRegistrationToAppServer (string token)
{
    // Add custom implementation here as needed.
}
```

In einigen Fällen benötigt die app-Server nicht Registrierungstoken des Benutzers. In diesem Fall kann dieser Methode ausgelassen werden. Wenn ein Registrierungstoken an der app-Server gesendet wird `SendRegistrationToAppServer` sollten beibehalten, ein boolescher Wert, um anzugeben, ob das Token an den Server gesendet wurde. Wenn dieser Wert auf "false" ist `SendRegistrationToAppServer` sendet das Token an die Anwendungsserver &ndash; , andernfalls das Token bereits an der app-Server in einem vorherigen Aufruf gesendet wurde. 


##### <a name="subscribe-to-the-notification-topic"></a>Das Benachrichtigungsthema abonnieren

Als Nächstes rufen wir unseren `Subscribe` Methode, um anzugeben, GCM, die an ein Benachrichtigungsthema abonnieren soll. In `Subscribe`, rufen wir die [GcmPubSub.Subscribe](https://developers.google.com/android/reference/com/google/android/gms/gcm/GcmPubSub.html#subscribe&#40;java.lang.String,%20java.lang.String,%20android.os.Bundle&#41;) API, abonnieren unseren Client-app für alle Nachrichten, die unter `/topics/global`:

```csharp
void Subscribe (string token)
{
    var pubSub = GcmPubSub.GetInstance(this);
    pubSub.Subscribe(token, "/topics/global", null);
}
```

Der app-Server senden muss benachrichtigungsmeldungen an `/topics/global` Wenn Sie diese empfangen werden. Beachten Sie, die im Thema zu benennen, unter `/topics` kann beliebig, sein, solange der app-Server und die Client-app auf diesen Namen akzeptieren. (Hier haben wir den Namen `global` , um anzugeben, dass zum Empfangen von Nachrichten in allen Themen, die von der app-Server unterstützt werden soll.) 

Informationen zu GCM-Thema, die auf dem Server-messaging finden Sie in der Google [senden Botschaften und Hinweise an Themen](https://developers.google.com/cloud-messaging/topic-messaging). 


#### <a name="implement-an-instance-id-listener-service"></a>Einen Instanz-ID-Listenerdienst implementieren

Registrierungstoken sind eindeutig und sicher. Allerdings kann der Client-app (oder GCM) müssen das Registrierungstoken im Fall von app-Neuinstallation oder ein Sicherheitsproblem aktualisieren. Aus diesem Grund müssen wir implementieren ein `InstanceIdListenerService` , die von GCM tokenaktualisierung-Anforderungen reagiert. 

Fügen Sie eine neue Datei namens **InstanceIdListenerService.cs** , und Ersetzen Sie den Vorlagencode durch den folgenden: 

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

Mit einer Anmerkung versehen `InstanceIdListenerService` mit dem folgenden Attribut, um anzugeben, dass der Dienst ist nicht für vom System instanziiert werden und sie GCM-Registrierungstoken erhalten kann (so genannte *Instanz-ID*) aktualisieren von Anforderungen: 

```csharp
[Service(Exported = false), IntentFilter(new[] { "com.google.android.gms.iid.InstanceID" })]
```

Die `OnTokenRefresh` Methode in unserem Dienst startet die `RegistrationIntentService` , damit sie das Registrierungstoken für die neue abfangen kann.


#### <a name="test-registration-with-gcm"></a>Testen Sie die Registrierung bei GCM

Lassen Sie uns vollständig neu zu erstellen und Ausführen der app. Wenn Sie erfolgreich ein Registrierungstoken von GCM empfangen haben, sollte das Registrierungstoken im Ausgabefenster angezeigt werden. Zum Beispiel: 

```shell
D/Mono    ( 1934): Assembly Ref addref ClientApp[0xb4ac2400] -> Xamarin.GooglePlayServices.Gcm[0xb4ac2640]: 2
I/RegistrationIntentService( 1934): Calling InstanceID.GetToken
I/RegistrationIntentService( 1934): GCM Registration Token: f8LdveCvXig:APA91bFIsjUAbP-V8TPQdLR89qQbEJh1SYG38AcCbBUf34z5gSdUc5OsXrgs93YFiGcRSRafPfzkz23lf3-LvYV1CwrFheMjHgwPeFSh12MywnRIhz

```

### <a name="handle-downstream-messages"></a>Nachgelagerte Verarbeitung von Nachrichten 

Der Code, den wir bisher implementiert haben, ist nur "Einrichtung" Code; Er überprüft, um festzustellen, ob es sich bei Google Play Services ist installiert und mit GCM und der app-Server, um unsere Client-app vorzubereiten, für den Empfang von remotebenachrichtigungen handelt. Allerdings gibt es noch Code implementieren, der tatsächlich empfangen und Verarbeiten von benachrichtigungsmeldungen downstream. Zu diesem Zweck müssen wir implementieren eine *GCM-Listenerdienst*. Dieser Dienst Thema Nachrichten aus der app-Server empfängt und sendet diese lokal als Benachrichtigungen. Nachdem wir diesen Dienst implementiert haben, erstellen wir ein Testprogramm zum Senden von Nachrichten an GCM, sodass wir sehen können, wenn es sich bei unserer Implementierung ordnungsgemäß funktioniert. 


#### <a name="add-a-notification-icon"></a>Fügen Sie ein Symbol hinzu.

Zuerst fügen Sie ein kleines Symbol, das im Infobereich der Taskleiste angezeigt wird, wenn unsere Benachrichtigung gestartet wird. Sie können kopieren [dieses Symbol](remote-notifications-with-gcm-images/ic-stat-ic-notification.png) zu Ihrem Projekt, oder erstellen Sie Ihr eigenes benutzerdefinierte Symbol. Den Namen der Symboldatei **ic_stat_button_click.png** und kopieren Sie sie in der **Ressourcen/drawable** Ordner. Denken Sie daran, verwenden Sie **hinzufügen > Vorhandenes Element...**  auf dieses Symboldatei in Ihr Projekt einbinden.


#### <a name="implement-a-gcm-listener-service"></a>Einen GCM-Listenerdienst implementieren

Fügen Sie eine neue Datei namens **GcmListenerService.cs** , und Ersetzen Sie den Vorlagencode durch den folgenden:

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

Werfen wir einen Blick auf die einzelnen Abschnitte des unsere `GcmListenerService` zu verstehen, wie es funktioniert. 

Zuerst versehen wir `GcmListenerService` mit einem Attribut, um anzugeben, dass dieser Dienst ist nicht für vom System instanziiert werden, und wir eine Zielfilter schließen, um anzugeben, dass der Empfang von GCM-Nachrichten: 

```csharp
[Service (Exported = false), IntentFilter (new [] { "com.google.android.c2dm.intent.RECEIVE" })]
```

Wenn `GcmListenerService` empfängt eine Nachricht von GCM, die `OnMessageReceived` -Methode wird aufgerufen. Diese Methode extrahiert den Nachrichteninhalt aus dem übergebenen `Bundle`, protokolliert der Inhalt der Nachricht (sodass wir sie in das Fenster "Ausgabe" sehen können), und ruft `SendNotification` um eine lokale Benachrichtigung mit dem Inhalt der empfangenen Nachricht zu starten: 

```csharp
var message = data.GetString ("message");
Log.Debug ("MyGcmListenerService", "From:    " + from);
Log.Debug ("MyGcmListenerService", "Message: " + message);
SendNotification (message);
```

Die `SendNotification` -Methode verwendet `Notification.Builder` verwendet zum Erstellen der Benachrichtigung, und klicken Sie dann die `NotificationManager` um die Benachrichtigung zu starten. Effektiv, konvertiert diese die remote-Nachricht in eine lokale Benachrichtigung, die dem Benutzer angezeigt werden.
Weitere Informationen zur Verwendung von `Notification.Builder` und `NotificationManager`, finden Sie unter [lokale Benachrichtigungen](~/android/app-fundamentals/notifications/local-notifications.md).


#### <a name="declare-the-receiver-in-the-manifest"></a>Den Empfänger im Manifest deklariert

Bevor wir die Nachrichten von GCM empfangen kann, muss den GCM-Listener in der Android-Manifest deklariert werden. Ermöglicht das Bearbeiten von **"androidmanifest.xml"** , und Ersetzen Sie die `<application>` Abschnitt mit den folgenden XML-Code: 

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

Ändern Sie im obigen XML *YOUR_PACKAGE_NAME* an den Paketnamen für Ihre Client-app-Projekt. In unserem Beispiel Exemplarische Vorgehensweise der Paketname ist `com.xamarin.gcmexample`. 

Sehen wir uns, was bewirkt, dass jede Einstellung in dieser XML-Code:

|Einstellung|Beschreibung|
|---|---|
|`com.google.android.gms.gcm.GcmReceiver`|Deklariert, dass unsere app einen GCM-Empfänger implementiert, der erfasst und verarbeitet eingehende Nachrichten der Push-Benachrichtigungen.|
|`com.google.android.c2dm.permission.SEND`|Deklariert, dass nur die GCM-Server die Nachrichten direkt an die app senden können.|
|`com.google.android.c2dm.intent.RECEIVE`|Der Zielfilter angekündigt, dass unsere app von GCM-Broadcastmeldungen behandelt.|
|`com.google.android.c2dm.intent.REGISTRATION`|Zielfilter angekündigt, dass unsere app neue Registrierung Intents verarbeitet (d. h. wir implementiert haben einen Instanz-ID-Listenerdienst).|

Sie können alternativ ergänzen `GcmListenerService` mit der Angabe in XML, anstatt diese Attribute hier wir geben Sie sie in **"androidmanifest.xml"** , damit die Beispiele für Code leichter zu verstehen sind. 


### <a name="create-a-message-sender-to-test-the-app"></a>Erstellen Sie den Absender einer Nachricht zum Testen der App

Fügen Sie eine C# desktop-Konsolenanwendung zur Projektmappe, und nennen Sie sie **MessageSender**. Wir verwenden diese Konsolenanwendung zum Simulieren von eines Anwendungsservers &ndash; Traffic Manager sendet benachrichtigungsmeldungen an **ClientApp** über GCM. 


#### <a name="add-the-jsonnet-package"></a>Fügen Sie dem JSON.NET-Paket hinzu.

In dieser Konsolen-app erstellen wir eine JSON-Nutzlast, die die Nachricht enthält, die an die Client-app gesendet werden soll. Wir verwenden die **Json.NET** -Paket in **MessageSender** zu vereinfachen, erstellen Sie das JSON-Objekt, das durch GCM erforderlich. In Visual Studio mit der Maustaste **Verweise > NuGet-Pakete verwalten...** ; in Visual Studio für Mac, mit der rechten Maustaste **Pakete > Pakete hinzufügen...** . 

Wir suchen nach den **Json.NET** Packen und installieren Sie es in das Projekt: 

[![Installieren des Pakets Json.NET](remote-notifications-with-gcm-images/4-add-json.net-sml.png)](remote-notifications-with-gcm-images/4-add-json.net.png#lightbox)


#### <a name="add-a-reference-to-systemnethttp"></a>Hinzufügen eines Verweises auf System.Net.Http

Wir müssen auch einen Verweis hinzufügen `System.Net.Http` , damit wir instanziieren kann ein `HttpClient` für die Testnachricht an GCM sendet. In der **MessageSender** -Projekt mit der rechten Maustaste **Verweise > Verweis hinzufügen** und scrollen Sie nach unten, bis Sie sehen **System.Net.Http**. Fügen ein Häkchen neben **System.Net.Http** , und klicken Sie auf **OK**. 


#### <a name="implement-code-that-sends-a-test-message"></a>Implementieren Sie Code, die eine Testnachricht gesendet.

In **MessageSender**, bearbeiten Sie **"Program.cs"** und Ersetzen Sie den Inhalt durch den folgenden Code:

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

Diese Nachricht an Ihre Client-app wird von GCM, wiederum weitergeleitet. Erstellen Sie **MessageSender** , und öffnen Sie ein Konsolenfenster, in dem wir es über die Befehlszeile ausführen können.



### <a name="try-it"></a>Probieren Sie es aus!

Nun können wir unsere Client-app zu testen. Wenn Sie einen Emulator verwenden, oder wenn Ihr Gerät über WLAN mit GCM kommuniziert, Sie die folgenden TCP-Ports, in der Firewall für GCM-Nachrichten öffnen müssen zu erledigen: 5228 5229 und 5230.

Starten Sie Ihre Client-app, und beobachten Sie das Fenster "Ausgabe". Nach der `RegistrationIntentService` erfolgreich empfängt eine Registrierung von GCM token, das Ausgabefenster sollte angezeigt werden das Token Ausgabeprotokoll wie den folgenden:

```shell
I/RegistrationIntentService(16103): GCM Registration Token: eX9ggabZV1Q:APA91bHjBnQXMUeBOT6JDiLpRt8m2YWtY ...
```

An diesem Punkt ist die Client-app bereit, eine remote-Benachrichtigung zu empfangen. Führen Sie über die Befehlszeile, die **MessageSender.exe** Programm, um eine Meldung "Hello, Xamarin" an die Client-app zu senden.
Wenn Sie noch nicht erstellt haben die **MessageSender** Projekt, tun Sie dies jetzt.

Auszuführende **MessageSender.exe** unter Visual Studio, öffnen Sie eine Eingabeaufforderung, wechseln Sie in der **MessageSender/Bin/Debug** Verzeichnis, und führen Sie den Befehl direkt:

```cmd
MessageSender.exe
```

Auszuführende **MessageSender.exe** unter Visual Studio für Mac öffnen eine Terminalsitzung, **MessageSender/Bin/Debug** das Verzeichnis und das Verwenden von Mono ausführen **MessageSender.exe** 

```bash
mono MessageSender.exe
```

Es kann bis zu einer Minute für die Nachricht über GCM und wieder auf Ihre Client-app verteilt dauern. Wenn die Nachricht erfolgreich empfangen wurde, sollte ähnlich die folgenden in das Fenster "Ausgabe" Ausgabe angezeigt werden: 

```shell
D/MyGcmListenerService(16103): From:    /topics/global
D/MyGcmListenerService(16103): Message: Hello, Xamarin!
```

Darüber hinaus sollten Sie feststellen, dass ein neues Benachrichtigungssymbol im Benachrichtigungsbereich angezeigt wurde: 

[![Symbol "Benachrichtigung" wird auf Gerät angezeigt.](remote-notifications-with-gcm-images/5-icon-appears-sml.png)](remote-notifications-with-gcm-images/5-icon-appears.png#lightbox)

Beim Öffnen der Komponentenleiste Benachrichtigung, um Benachrichtigungen anzuzeigen, sehen Sie unsere remote-Benachrichtigung:

[![Benachrichtigung wird angezeigt.](remote-notifications-with-gcm-images/6-notification-in-tray-sml.png)](remote-notifications-with-gcm-images/6-notification-in-tray.png#lightbox)

Herzlichen Glückwunsch! Ihre app hat seine erste remote-Benachrichtigung erhalten.

Beachten Sie, dass es sich bei GCM nicht mehr empfangen werden sollen, wenn die app-Force-beendet wird. Um Benachrichtigungen nach der ein Force Hand fortzusetzen, muss die app manuell neu gestartet werden. Weitere Informationen zu dieser Android-Richtlinie finden Sie unter [Steuerelemente auf die beendete Anwendungen starten](https://developer.android.com/about/versions/android-3.1.html#launchcontrols) und [stack Overflow-Beitrag](https://stackoverflow.com/questions/5051687/broadcastreceiver-not-receiving-boot-completed/19856267#19856267). 

 
## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise beschrieben, die Schritte zur Implementierung von remotebenachrichtigungen in einer Xamarin.Android-Anwendung. Es wurde beschrieben, wie Sie zusätzliche Pakete, die erforderlich sind, für die Kommunikation mit GCM installieren, und es wurde erklärt, wie app-Berechtigungen für den Zugriff auf den GCM-Server zu konfigurieren. Sie bereitgestellt Beispielcode, der veranschaulicht, wie So prüfen Sie auf das Vorhandensein von Google Play-Dienste und Gewusst wie: Implementieren eines Registrierung intent-Dienst und die Instanz-ID-Listenerdienst, der für ein Registrierungstoken bei GCM verhandelt zum Implementieren von GCM listener Dienst, der empfangen und Verarbeiten von benachrichtigungsmeldungen remote. Schließlich haben wir ein Befehlszeilen-Test-Programm zum Senden von testbenachrichtigungen an die Client-app durch GCM implementiert. 


## <a name="related-links"></a>Verwandte Links

- [GCM-Modes (Beispiel)](https://developer.xamarin.com/samples/monodroid/RemoteNotifications)
- [Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md)
