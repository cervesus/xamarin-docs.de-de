---
title: Remote-Benachrichtigungen mit Firebase Cloud-Messaging
description: "Diese exemplarische Vorgehensweise enthält eine schrittweise Erklärung der Vorgehensweise Firebase Cloud Messaging verwenden, um remote Benachrichtigungen (so genannte Pushbenachrichtigungen) zu implementieren, in einer Anwendung Xamarin.Android. Es wird veranschaulicht, wie die verschiedenen Klassen implementieren, die für die Kommunikation mit Firebase Cloud Messaging (FCM) erforderlich sind, bietet Beispiele für die Vorgehensweise zum Konfigurieren der Android-Manifest für den Zugriff auf FCM und zeigt downstream-messaging mithilfe der Firebase Konsole."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4D7C5F46-C997-49F6-AFDA-6763E68CDC90
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 4e5bf2b24845fa008c6f97a6d55e18a51bc82164
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="remote-notifications-with-firebase-cloud-messaging"></a>Remote-Benachrichtigungen mit Firebase Cloud-Messaging

_Diese exemplarische Vorgehensweise enthält eine schrittweise Erklärung der Vorgehensweise Firebase Cloud Messaging verwenden, um remote Benachrichtigungen (so genannte Pushbenachrichtigungen) zu implementieren, in einer Anwendung Xamarin.Android. Es wird veranschaulicht, wie die verschiedenen Klassen implementieren, die für die Kommunikation mit Firebase Cloud Messaging (FCM) erforderlich sind, bietet Beispiele für die Vorgehensweise zum Konfigurieren der Android-Manifest für den Zugriff auf FCM und zeigt downstream-messaging mithilfe der Firebase Konsole._

## <a name="fcm-notifications-overview"></a>Übersicht über die FCM-Benachrichtigungen

In dieser exemplarischen Vorgehensweise eine einfache app aufgerufen **FCMClient** erstellt werden, um die Grundlagen der FCM messaging zu veranschaulichen. **FCMClient** überprüft das Vorhandensein von Google Play-Dienste, FCM Registrierungstoken Empfangsvorgänge, zeigt der remote-Benachrichtigungen, die Sie über die Konsole Firebase senden und abonniert Nachrichten Thema:

[![Screenshot der Beispiel-App](remote-notifications-with-fcm-images/00-app-example-sml.png)](remote-notifications-with-fcm-images/00-app-example.png)

Die folgenden Themenbereiche werden untersucht werden:

1.  Hintergrund-Benachrichtigungen

2.  Thema Nachrichten

3.  Vordergrund-Benachrichtigungen

In dieser exemplarischen Vorgehensweise wird inkrementell Funktionen zur hinzufügen **FCMClient** und führen Sie es auf einem Gerät oder Emulator zu verstehen, wie sie mit den FCM interagiert. Verwenden Sie Protokollierung, um aktive app Transaktionen mit FCM Servern Zeuge und bemerken Sie, wie Benachrichtigungen aus FCM Nachrichten generiert werden, die Sie in der GUI Firebase Konsole Benachrichtigungen eingeben. 

## <a name="requirements"></a>Anforderungen

Bevor Sie mit dieser exemplarischen Vorgehensweise fortfahren können, müssen Sie die erforderlichen Anmeldeinformationen zur Verwendung Googles FCM Server erwerben; Dieser Prozess wird erläutert, [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md). Sie müssen insbesondere Herunterladen der **Google-services.json** -Datei mit dem Beispielcode in dieser exemplarischen Vorgehensweise dargestellt. Wenn Sie noch keinem Projekt in der Konsole Firebase erstellt haben (oder wenn Sie noch nicht heruntergeladen haben die **Google-services.json** Datei), finden Sie unter [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md). 

Zum Ausführen der Beispiel-app benötigen Sie ein Android-Testgerät oder -Emulator, der mit Firebase kompatibles ist. Firebase Cloud Messaging unterstützt Clients, die unter Android 2.3 (Gingerbread) oder höher, und diese Geräte müssen die installierte Google Play Store-app (Google Play Services 9.2.1 oder höher ist erforderlich). Wenn Sie noch nicht über die Google Play Store-app auf Ihrem Gerät installierten verfügen, besuchen Sie die [Google Play](https://support.google.com/googleplay) -Website herunterladen und installieren. Alternativ können Sie den Android SDK-Emulator Android 2.3 oder höher, anstatt ein Testgerät (Sie müssen keinen Google Play Store installieren, bei Verwendung von Android SDK-Emulator) ausgeführt. 

## <a name="start-an-app-project"></a>Starten Sie ein App-Projekt

Um zu beginnen, erstellen Sie ein neue leeres Xamarin.Android Projekt mit der Bezeichnung **FCMClient**. Wenn Sie nicht mit dem Erstellen von Projekten Xamarin.Android vertraut sind, finden Sie unter [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md). Nachdem neue app erstellt wurde, besteht der nächste Schritt legen Sie den Paketnamen, und installieren mehrere NuGet-Pakete, die für die Kommunikation mit FCM verwendet werden. 

### <a name="set-the-package-name"></a>Legen Sie den Paketnamen

In [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md), einen den Paketnamen für die FCM-fähige Anwendung angegeben. Dieses Paketname dient auch als die *Anwendungs-ID* , API-Schlüssel zugeordnet ist. Konfigurieren Sie die app so, verwenden Sie diese Paketname:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Öffnen Sie die Eigenschaften für die **FCMClient** Projekt. 

2.  In der **Android-Manifest** Seite, legen Sie den Paketnamen. 

Im folgenden Beispiel wird der Paketname so eingerichtet `com.xamarin.fcmexample`: 

[![Der Paketname festlegen](remote-notifications-with-fcm-images/01-package-name-vs-sml.png)](remote-notifications-with-fcm-images/01-package-name-vs.png)

Während Sie aktualisiert werden die **Android-Manifest**, überprüfen Sie auch, um sicherzustellen, dass die `Internet` Berechtigung aktiviert ist. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1.  Öffnen Sie die Eigenschaften für die **FCMClient** Projekt. 

2.  In der **Android-Anwendung** Seite, legen Sie den Paketnamen. 

Im folgenden Beispiel wird der Paketname so eingerichtet `com.xamarin.fcmexample`: 

[![Der Paketname festlegen](remote-notifications-with-fcm-images/01-package-name-xs-sml.png)](remote-notifications-with-fcm-images/01-package-name-xs.png)

Während Sie aktualisiert werden die **Android-Manifest**, überprüfen Sie auch, um sicherzustellen, dass die `INTERNET` Berechtigung aktiviert ist (unter **erforderliche Berechtigungen**). 

-----

Beachten Sie, dass die Client-app in der Lage ist, ein Token für die Registrierung von FCM zu empfangen, wenn keine dieser Paketname *genau* entsprechen den Paketnamen, die in der Konsole Firebase eingegeben wurde. 

### <a name="add-the-xamarin-google-play-services-base-package"></a>Fügen Sie das Basispaket Xamarin Google Play Services hinzu.

Da Firebase Cloud Messaging abhängig mit Google Play-Dienste, die [Xamarin Google Play-Dienste - Basis](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Base/) NuGet-Paket muss dem Projekt Xamarin.Android hinzugefügt werden. Sie benötigen Version 29.0.0.2 oder höher.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  In Visual Studio mit der Maustaste **Verweise > NuGet-Pakete verwalten...** . 

2.  Klicken Sie auf die **Durchsuchen** Registerkarte, und suchen Sie nach **Xamarin.GooglePlayServices.Base**. 

3.  Installieren Sie dieses Paket in der **FCMClient** Projekt: 

    [ ![Installieren Google Play-Dienste-Basis](remote-notifications-with-fcm-images/02-google-play-services-vs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1.  In Visual Studio für Mac, mit der Maustaste **Pakete > Pakete hinzufügen...** . 

2.  Suchen Sie nach **Xamarin.GooglePlayServices.Base**. 

3.  Installieren Sie dieses Paket in der **FCMClient** Projekt: 

    [ ![Installieren Google Play-Dienste-Basis](remote-notifications-with-fcm-images/02-google-play-services-xs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-xs.png)

-----

Wenn Sie während der Installation des NuGet-Fehlermeldung erhalten, schließen Sie die **FCMClient** Projekt öffnen Sie es erneut, und wiederholen Sie die Installation der NuGet. 

Bei der Installation **Xamarin.GooglePlayServices.Base**, alle erforderlichen Abhängigkeiten werden ebenfalls installiert. Bearbeiten Sie **MainActivity.cs** und fügen Sie die folgenden `using` Anweisung: 

```csharp
using Android.Gms.Common;
```

Diese Aussage der `GoogleApiAvailability` -Klasse im **Xamarin.GooglePlayServices.Base** zur **FCMClient** Code. 
`GoogleApiAvailability` wird verwendet, um das Vorhandensein von Google Play-Dienste zu überprüfen. 

### <a name="add-the-xamarin-firebase-messaging-package"></a>Fügen Sie die Xamarin-Firebase Messaging-Paket hinzu

Zum Empfangen von Nachrichten aus FCM, die [Xamarin Firebase - Messaging](https://www.nuget.org/packages/Xamarin.Firebase.Messaging/) NuGet-Paket muss mit der app-Projekt hinzugefügt werden. Ohne dieses Paket kann keine Android-Anwendung Nachrichten aus FCM Server empfangen.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  In Visual Studio mit der Maustaste **Verweise > NuGet-Pakete verwalten...** . 

2.  Überprüfen Sie **Vorabversion einschließen** , suchen Sie nach **Xamarin.Firebase.Messaging**. 

3.  Installieren Sie dieses Paket in der **FCMClient** Projekt: 

    [ ![Installieren von Xamarin Firebase Messaging](remote-notifications-with-fcm-images/03-firebase-messaging-vs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1.  In Visual Studio für Mac, mit der Maustaste **Pakete > Pakete hinzufügen...** . 

2.  Überprüfen Sie **Vorabversion Pakete anzeigen, die** , suchen Sie nach **Xamarin.Firebase.Messaging**. 

3.  Installieren Sie dieses Paket in der **FCMClient** Projekt: 

    [ ![Installieren von Xamarin Firebase Messaging](remote-notifications-with-fcm-images/03-firebase-messaging-xs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-xs.png)

-----
 
Bei der Installation **Xamarin.Firebase.Messaging**, alle erforderlichen Abhängigkeiten werden ebenfalls installiert.

Als Nächstes bearbeiten **MainActivity.cs** und fügen Sie die folgenden `using` Anweisungen:

```csharp
using Firebase.Messaging;
using Firebase.Iid;
using Android.Util;
```

Die ersten beiden Anweisungen stellen die Typen in der **Xamarin.Firebase.Messaging** NuGet-Paket zur Verfügung **FCMClient** Code. **Android.Util** fügt-Protokollfunktion, die verwendet wird, um Transaktionen mit FMS zu beobachten. 


### <a name="add-the-google-services-json-file"></a>Fügen Sie die Google Services JSON-Datei

Der nächste Schritt besteht, zum Hinzufügen der **Google-services.json** Datei in das Stammverzeichnis des Projekts: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Kopie **Google-services.json** in den Projektordner.

2.  Hinzufügen **Google-services.json** mit der app-Projekt (klicken Sie auf **alle Dateien anzeigen** in der **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf **Google-services.json**, und wählen Sie dann **Projekt**). 

3.  Wählen Sie **Google-services.json** in der **Projektmappen-Explorer** Fenster.

4.  In der **Eigenschaften** legen Sie im Bereich der **Buildvorgang** auf **GoogleServicesJson** (wenn die **GoogleServicesJson** Buildvorgang wird nicht angezeigt, Speichern und schließen Sie die Projektmappe, und öffnen Sie sie erneut):

    [![Wenn die Buildaktion auf GoogleServicesJson](remote-notifications-with-fcm-images/04-google-services-json-vs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1.  Kopie **Google-services.json** in den Projektordner.

2.  Hinzufügen **Google-services.json** mit der app-Projekt. 

3.  Mit der rechten Maustaste **Google-services.json**.

4.  Legen Sie die **Buildvorgang** auf **GoogleServicesJson**: 

    [![Wenn die Buildaktion auf GoogleServicesJson](remote-notifications-with-fcm-images/04-google-services-json-xs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-xs.png)
 
-----
 

Wenn **Google-services.json** dem Projekt hinzugefügt wird (und die **GoogleServicesJson** Buildvorgang festgelegt ist), während des Erstellungsprozesses extrahiert die Client-ID und API-Schlüssel und fügt dann diese Anmeldeinformationen, um die zusammengeführte / generiert **AndroidManifest.xml** befindet, die sich auf **obj/Debug/android/AndroidManifest.xml**. Dieser Mergeprozess fügt automatisch alle Berechtigungen und anderen FCM-Elementen, die für die Verbindung zu Servern FCM erforderlich sind. 


## <a name="check-for-google-play-services"></a>Prüfung auf Google Play-Dienste

Zuerst wird eine anfängliche Layout für die Benutzeroberfläche der Anwendung erstellt. Bearbeiten Sie **Resources/layout/Main.axml** und Ersetzen Sie den Inhalt durch folgendes XML: 

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

Dies `TextView` wird verwendet, um Nachrichten anzuzeigen, die angeben, ob es sich bei Google Play-Dienste installiert ist. Speichern Sie die Änderungen zu **Main.axml**. Bearbeiten Sie **MainActivity.cs** und hinzufügen die folgenden Variablendeklaration Instanz um die `MainActivity` Klasse: 

```csharp
TextView msgText;
```

Google empfiehlt, Android-apps für das Vorhandensein der Google-Wiedergabe Services APK zu suchen, bevor Sie den Zugriff auf Google Play-Dienste-Funktionen (Weitere Informationen finden Sie unter [für Google Play-Dienste überprüfen](https://firebase.google.com/docs/cloud-messaging/android/client#sample-play)). Im folgenden Beispiel die `OnCreate` Methode überprüft, dass Google Play-Dienste verfügbar ist, bevor die app versucht, FCM Dienste zu verwenden. Fügen Sie die folgende Methode, die `MainActivity` Klasse: 

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
            msgText.Text = "This device is not supported";
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

Dieser Code überprüft, ob das Gerät, um festzustellen, ob die Google wiedergeben Services APK installiert ist. Wenn es nicht installiert ist, wird eine Meldung angezeigt, der `TextBox` , die durch die der Benutzer, ein APK aus dem Google Play Store herunterladen (oder in den Systemeinstellungen für das Gerät zu aktivieren). 

Ersetzen Sie die `OnCreate`-Methode durch folgenden Code: 

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);
    SetContentView (Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);
    IsPlayServicesAvailable ();
}
```

`IsPlayServicesAvailable` wird aufgerufen, am Ende der `OnCreate` , damit die Google Play-Dienste überprüfen führt jedes Mal die app wird gestartet. Wenn die app kann eine `OnResume` -Methode, die sie aufrufen sollte `IsPlayServicesAvailable` aus `OnResume` ebenfalls. Vollständig neu erstellen und Ausführen der app. Wenn alle ordnungsgemäß konfiguriert ist, sehen Sie einen Bildschirm, der dem folgenden Screenshot ähnelt: 

[![App gibt an, dass Google Play-Dienste verfügbar ist.](remote-notifications-with-fcm-images/05-gps-available-sml.png)](remote-notifications-with-fcm-images/05-gps-available.png)

Wenn Sie dieses Ergebnis nicht erhalten, stellen Sie sicher, dass die Google wiedergeben Services APK auf Ihrem Gerät installiert ist (Weitere Informationen finden Sie unter [Einstellung von Google Play-Dienste](https://developers.google.com/android/guides/setup)). Zudem sicher, dass Sie hinzugefügt haben die **Xamarin.Google.Play.Services.Base** -Pakets mit Ihrem **FCMClient** Projekt wie zuvor erklärt.


## <a name="add-the-instance-id-receiver"></a>Die Instanz-ID-Empfänger hinzufügen

Der nächste Schritt besteht, zum Hinzufügen eines Diensts, der erweitert `FirebaseInstanceIdService` behandelt die Erstellung, Drehung und Registrierungstoken Firebase aktualisieren. (Weitere Informationen zu Registrierungstoken finden Sie unter [Registrierung mit FCM](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md).) Die `FirebaseInstanceIdService` Dienst ist für FCM können zum Senden von Nachrichten an das Gerät erforderlich. Wenn die `FirebaseInstanceIdService` Dienst wird auf dem Client-app hinzugefügt, die app automatisch FCM Nachrichten empfängt und als Benachrichtigungen angezeigt werden können, wenn die app backgrounded ist. 

### <a name="declare-the-receiver-in-the-android-manifest"></a>Deklarieren Sie die Empfänger in der Android-Manifest

Bearbeiten Sie **AndroidManifest.xml** , und fügen Sie die folgenden `<receiver>` Elemente in der `<application>` Abschnitt: 

```xml
<receiver
    android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver"
    android:exported="false" />
<receiver
    android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver"
    android:exported="true"
    android:permission="com.google.android.c2dm.permission.SEND">
    <intent-filter>
        <action android:name="com.google.android.c2dm.intent.RECEIVE" />
        <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
        <category android:name="${applicationId}" />
    </intent-filter>
</receiver>
```

Diese XML-Ausgabe führt Folgendes aus:

-   Deklariert eine `FirebaseInstanceIdReceiver` Implementierung, die bereitstellt eine [eindeutigen Bezeichner](https://developers.google.com/instance-id/) für jede app-Instanz. Dieser Empfänger ist außerdem authentifiziert und autorisiert Aktionen. 

-   Deklariert eine interne `FirebaseInstanceIdInternalReceiver` Implementierung, die beim Starten der Dienste sicher verwendet wird.

Die `FirebaseInstanceIdReceiver` ist ein `WakefulBroadcastReceiver` , empfängt `FirebaseInstanceId` und `FirebaseMessaging` Ereignisse und übermittelt sie an die Klasse, die beim Ableiten von `FirebaseInstanceIdService`. 

### <a name="implement-the-firebase-instance-id-service"></a>Implementieren der Firebase Instanz-ID-Dienst

Die Arbeit von der Anwendung FCM registrieren erfolgt durch die benutzerdefinierte `FirebaseInstanceIdService` Dienst, die Sie bereitstellen. 
`FirebaseInstanceIdService` führt die folgenden Schritte aus: 

1.  Verwendet die [Instanz-ID-API](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID) Sicherheitstoken zu generieren, die die Client-app den Zugriff auf FCM und app-Servers zu autorisieren. Im Gegenzug Ruft die app wieder eine Registrierung token aus FCM. 

2.  Die Registrierung Token an den Anwendungsserver weiterleitet, wenn die app-Servers erforderlich ist.

Fügen Sie eine neue Datei namens **MyFirebaseIIDService.cs** und seine Vorlagencode durch Folgendes ersetzen: 

```csharp
using System;
using Android.App;
using Firebase.Iid;
using Android.Util;

namespace FCMClient
{
    [Service]
    [IntentFilter(new[] { "com.google.firebase.INSTANCE_ID_EVENT" })]
    public class MyFirebaseIIDService : FirebaseInstanceIdService
    {
        const string TAG = "MyFirebaseIIDService";
        public override void OnTokenRefresh()
        {
            var refreshedToken = FirebaseInstanceId.Instance.Token;
            Log.Debug(TAG, "Refreshed token: " + refreshedToken);
            SendRegistrationToServer(refreshedToken);
        }
        void SendRegistrationToServer(string token)
        {
            // Add custom implementation, as needed.
        }
    }
}
```

Dieser Dienst implementiert einen `OnTokenRefresh` -Methode, die aufgerufen wird, wenn das Token für die Registrierung zunächst erstellt oder geändert wird. Wenn `OnTokenRefresh` ausgeführt wird, ruft das aktuelle Token ab der `FirebaseInstanceId.Instance.Token` -Eigenschaft (wodurch asynchron durch FCM aktualisiert wird). In diesem Beispiel wird das aktualisierte Token protokolliert, damit sie im Ausgabefenster angezeigt werden kann: 

```csharp
var refreshedToken = FirebaseInstanceId.Instance.Token;
Log.Debug(TAG, "Refreshed token: " + refreshedToken);
```

`OnTokenRefresh` selten aufgerufen wird: Es wird verwendet, um das Token unter den folgenden Umständen aktualisieren: 

-   Wenn die Anwendung installiert oder deinstalliert wurde.

-   Wenn der Benutzer app-Daten löscht. 

-   Wenn die app löscht die Instanz-ID.

-   Wenn die Sicherheit des Tokens kompromittiert wurde.

Gemäß den von Google [Instanz-ID](https://developers.google.com/instance-id/guides/android-implementation) Dokumentation der FCM Instanz-ID-Dienst fordert an, dass die app in regelmäßigen Abständen dem Token (in der Regel alle sechs Monate) zu aktualisieren. 

`OnTokenRefresh` Ruft auch `SendRegistrationToAppServer` Zuordnen des Benutzers Registrierung token mit dem Konto serverseitige (sofern vorhanden), wird von der Anwendung verwaltet: 

```csharp
void SendRegistrationToAppServer (string token)
{
    // Add custom implementation here as needed.
}
```

Da diese Implementierung für den Entwurf des app-Servers abhängig ist, wird ein leere Methodentext in diesem Beispiel bereitgestellt. Wenn Ihre app-Servers FCM Registrierungsinformationen muss, ändern Sie `SendRegistrationToAppServer` alle serverseitigen Konto verwaltet, die von Ihrer app das Zugriffstoken des Benutzers FCM Instanz-ID zugeordnet werden soll. (Beachten Sie, dass das Token für die Client-app nicht transparent ist.) 

Wenn ein Token an den Anwendungsserver gesendet wird `SendRegistrationToAppServer` sollten beibehalten, einen booleschen Wert ab, der angibt, ob das Token an den Server gesendet wurde. Wenn dieser Wert auf "false" ist `SendRegistrationToAppServer` sendet das Token an den Anwendungsserver &ndash; , andernfalls das Token an den Anwendungsserver in einem vorherigen Aufruf bereits gesendet wurde. In einigen Fällen (z. B. dies `FCMClient` Beispiel), app-Servers ist es das Token nicht erforderlich; diese Methode ist daher nicht für dieses Beispiel erforderlich. 

## <a name="implement-client-app-code"></a>Client-App-Code implementieren

Nun, dass die Empfänger Dienste vorhanden sind, kann Client-app-Code geschrieben werden, diese Dienste nutzen. In den folgenden Abschnitten wird eine Schaltfläche auf der Benutzeroberfläche, melden Sie das Token Registrierung hinzugefügt (so genannte der *Instanz-ID-Token*), und weiteren Code hinzugefügt wird `MainActivity` anzeigen `Intent` Informationen, wenn die app gestartet wird, aus einer Benachrichtigung: 

[![Schaltfläche-Token "Protokoll" Bildschirm app hinzugefügt](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png)

### <a name="log-tokens"></a>Log-Token

Der in diesem Schritt hinzugefügten Code dient nur zu Demonstrationszwecken &ndash; Produktions-Client-app würde keine benötigen Registrierungstoken anmelden. Bearbeiten Sie **Resources/layout/Main.axml** und fügen Sie die folgenden `Button` Deklaration unmittelbar nach der `TextView` Element: 

```xml
<Button
  android:id="@+id/logTokenButton"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  android:layout_gravity="center_horizontal"
  android:text="Log Token" />
```

Bearbeiten Sie **MainActivity.cs** und fügen Sie die folgenden Memberdeklaration zu der `MainActivity` Klasse:

```csharp
const string TAG = "MainActivity";
```

Fügen Sie den folgenden Code am Ende der `OnCreate` Methode: 

```csharp
var logTokenButton = FindViewById<Button>(Resource.Id.logTokenButton);
logTokenButton.Click += delegate {
    Log.Debug(TAG, "InstanceID token: " + FirebaseInstanceId.Instance.Token);
};
```

Dieser Code protokolliert das aktuelle Token in das Fenster "Ausgabe" bei der **Protokoll Token** Schaltfläche abgerufen wird. 

### <a name="handle-notification-intents"></a>Benachrichtigung Intents behandeln

Wenn der Benutzer tippt eine Benachrichtigung ausgegeben **FCMClient**, beliebige Daten, die diese Benachrichtigung begleitende Nachricht im zur Verfügung gestellt wird `Intent` Extras. Bearbeiten Sie **MainActivity.cs** und fügen Sie den folgenden Code am Anfang der `OnCreate` Methode (vor dem Aufruf von `IsPlayServicesAvailable`): 

```csharp
if (Intent.Extras != null)
{
    foreach (var key in Intent.Extras.KeySet())
    {
        var value = Intent.Extras.GetString(key);
        Log.Debug(TAG, "Key: {0} Value: {1}", key, value);
    }
}
```

Die app-Startfeld `Intent` wird ausgelöst, wenn der Benutzer die Benachrichtigung tippt, damit dieser Code alle zugehörigen Daten, in protokolliert werden der `Intent` im Ausgabefenster. Wenn Sie ein anderes `Intent` ausgelöst werden muss, die `click_action` Feld der benachrichtigungsmeldung muss festgelegt werden, mit denen `Intent` (das Startprogramm `Intent` wird verwendet, wenn keine `click_action` angegeben ist). 


## <a name="background-notifications"></a>Hintergrund-Benachrichtigungen

Erstellen und Ausführen der **FCMClient** app. Die **Protokoll Token** -Schaltfläche angezeigt wird:

[![Schaltfläche "Protokoll-Token" wird angezeigt.](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png)

Tippen Sie auf die **Protokoll Token** Schaltfläche. Eine Meldung wie die folgende sollte im Ausgabefenster IDE angezeigt werden: 

[![Instanz-ID-Token im Fenster "Ausgabe" angezeigt](remote-notifications-with-fcm-images/07-token-received-sml.png)](remote-notifications-with-fcm-images/07-token-received.png)

Die lange Zeichenfolge mit der Bezeichnung mit **token** ist die Instanz-ID-Token, die Sie in der Konsole Firebase einfügen werden &ndash; auswählen und diese Zeichenfolge in die Zwischenablage kopieren. Wenn Sie keine Instanz-ID-Token angezeigt werden, fügen Sie die folgende Zeile am Anfang der `OnCreate` Methode, um zu überprüfen, ob **Google-services.json** ordnungsgemäß analysiert wurde:

```csharp
Log.Debug(TAG, "google app id: " + Resource.String.google_app_id);
```

Die `google_app_id` Wert, der im Ausgabefenster protokolliert übereinstimmen, die `mobilesdk_app_id` Wert in aufgezeichnet **Google-services.json**. 

### <a name="send-a-message"></a>Senden einer Nachricht

Melden Sie sich die [Firebase Konsole](https://console.firebase.google.com), wählen Sie Ihr Projekt, klicken Sie auf **Benachrichtigungen**, und klicken Sie auf **ersten Nachricht senden**: 

[![Der erste Nachricht Schaltfläche "Senden"](remote-notifications-with-fcm-images/08-first-notification-sml.png)](remote-notifications-with-fcm-images/08-first-notification.png)

Auf der **Nachricht verfassen** Seite Geben Sie den Nachrichtentext, und wählen Sie **Einzelgerät**. Die Instanz-ID-Token aus dem IDE-Ausgabefenster kopieren und fügen Sie ihn in die **FCM Registrierung Token** Feld der Firebase-Konsole: 

[![Verfassen von Meldungsdialogfeld](remote-notifications-with-fcm-images/09-compose-message-sml.png)](remote-notifications-with-fcm-images/09-compose-message.png)

Für die Android-Gerät (oder -Emulator) im Hintergrund in der app durch Tippen auf das Android **Übersicht** Schaltfläche und den Startbildschirm berühren. Wenn das Gerät bereit ist, klicken Sie auf **SEND MESSAGE** in der Konsole Firebase: 

[![Message-Schaltfläche "Senden"](remote-notifications-with-fcm-images/10-send-message-sml.png)](remote-notifications-with-fcm-images/10-send-message.png)

Wenn die **Überprüfung Nachricht** Dialogfeld angezeigt wird, klicken Sie auf **senden**.
Das Symbol "Benachrichtigung" sollte im Infobereich des Geräts (oder -Emulator) angezeigt werden: 

[![Symbol "Benachrichtigung" wird angezeigt.](remote-notifications-with-fcm-images/11-notification-icon-sml.png)](remote-notifications-with-fcm-images/11-notification-icon.png)

Öffnen Sie das Benachrichtigungssymbol, um die Nachricht anzuzeigen. Die Benachrichtigung der Eingabe in genau muss die **Meldungstext** Feld der Firebase-Konsole: 

[![Benachrichtigung wird auf dem Gerät angezeigt.](remote-notifications-with-fcm-images/12-notification-sml.png)](remote-notifications-with-fcm-images/12-notification.png)

Tippen Sie auf das Benachrichtigungssymbol wieder die **FCMClient** app. Die `Intent` Extras gesendet, um **FCMClient** werden im Ausgabefenster IDE aufgeführt: 

[![Beabsichtigte Extras Listen von Schlüssels, Nachrichten-ID und reduzieren](remote-notifications-with-fcm-images/13-intent-extras-sml.png)](remote-notifications-with-fcm-images/13-intent-extras.png)

In diesem Beispiel wird die **aus** Schlüssel auf die Anzahl der Firebase Projekt der app festgelegt ist (in diesem Beispiel `41590732`), und die **Collapse_key** an den Paketnamen festgelegt ist ( **com.xamarin.fcmexample**). Versuchen Sie es löschen, wenn Sie keine Nachricht erhält, den **FCMClient** app auf dem Gerät (oder -Emulator), und wiederholen Sie die oben genannten Schritte. 


> [!NOTE]
> **Hinweis:** Wenn Sie erzwingen die app schließen, FCM hält Übermitteln von Benachrichtigungen. Android wird verhindert, dass im Hintergrund Dienst Broadcasts versehentlich oder unnötigerweise starten Komponenten von beendete Anwendungen. (Weitere Informationen zu diesem Verhalten finden Sie unter [Steuerelemente auf die beendete Anwendungen starten](https://developer.android.com/about/versions/android-3.1.html#launchcontrols).) Aus diesem Grund ist es erforderlich, manuell deinstallieren der app jedes Mal führen Sie es, und von einer Debugsitzung zu beenden &ndash; Dies zwingt FCM, um ein neues Token zu generieren, damit weiterhin Nachrichten empfangen werden.

### <a name="add-a-custom-default-notification-icon"></a>Fügen Sie einem benutzerdefinierten Symbol hinzu.

Im vorherigen Beispiel wird das Symbol "Benachrichtigung" auf das Symbol "Anwendung" festgelegt. Das folgende XML wird ein benutzerdefinierter Standardsymbol für Benachrichtigungen konfiguriert. Android zeigt dieser benutzerdefinierten Standardsymbol für alle Benachrichtigungen, in dem das Symbol "Benachrichtigung" nicht explizit festgelegt ist. 

Um einem benutzerdefinierten Symbol zu hinzuzufügen, fügen Sie das Symbol auf der **Ressourcen und Ausgaben möglich** Verzeichnis bearbeiten **AndroidManifest.xml**, und fügen Sie die folgenden `<meta-data>` Element in der `<application>`Abschnitt: 

```xml
<meta-data
    android:name="com.google.firebase.messaging.default_notification_icon"
    android:resource="@drawable/ic_stat_ic_notification" />
```

In diesem Beispiel wird das Symbol "Benachrichtigung", die befindet sich am **Ressourcen/zeichenbaren/ic\_Stat\_ic\_notification.png** wird das benutzerdefinierte Benachrichtigung Standardsymbol verwendet werden. Wenn ein benutzerdefinierter Standardsymbol nicht, in konfiguriert ist **AndroidManifest.xml** und in die benachrichtigungsnutzlast Symbol "keine" festgelegt ist, Android verwendet das Symbol "Anwendung" als das Benachrichtigungssymbol, (wie in der oben dargestellten Screenshot der Benachrichtigung Symbol dargestellt). 

## <a name="handle-topic-messages"></a>Thema Nachrichten behandeln

Bisher geschriebenen Codes behandelt Registrierungstoken und die app remote Benachrichtigungsfunktionalität hinzugefügt. Im nächste Beispiel fügt Code, der überwacht *themennachrichten* und weiterleitet, die dem Benutzer als remote-Benachrichtigungen. Thema Nachrichten sind FCM-Nachrichten, die an ein oder mehrere Geräte gesendet werden, die ein bestimmtes Thema abonnieren. Weitere Informationen zum Thema Nachrichten finden Sie unter [Thema Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md). 

### <a name="subscribe-to-a-topic"></a>Abonnieren Sie ein Thema

Bearbeiten Sie **Resources/layout/Main.axml** und fügen Sie die folgenden `Button` unmittelbar nach der vorherigen Deklaration `Button` Element:

```xml
<Button
  android:id="@+id/subscribeButton"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  android:layout_gravity="center_horizontal"
  android:layout_marginTop="20dp"
  android:text="Subscribe to Notifications" />
```

Fügt diese XML-Daten eine **Benachrichtigung abonnieren** Schaltfläche, um das Layout.
Bearbeiten Sie **MainActivity.cs** und fügen Sie den folgenden Code am Ende der `OnCreate` Methode: 

```csharp
var subscribeButton = FindViewById<Button>(Resource.Id.subscribeButton);
subscribeButton.Click += delegate {
    FirebaseMessaging.Instance.SubscribeToTopic("news");
    Log.Debug(TAG, "Subscribed to remote notifications");
};
```

Dieser Code sucht den **Benachrichtigung abonnieren** Schaltfläche im Layout und weist Sie den Klickhandler für Code, der Aufrufe `FirebaseMessaging.Instance.SubscribeToTopic`, und übergeben Sie das abonnierte Thema _News_. Wenn der Benutzer tippt der **abonnieren** Schaltfläche, die app abonniert die _News_ Thema. Im folgenden Abschnitt eine _News_ themanachricht wird aus der GUI Firebase Konsole Benachrichtigungen gesendet werden. 

### <a name="send-a-topic-message"></a>Ein Thema senden

Die app deinstalliert werden, erstellen Sie ihn neu, und führen Sie es erneut. Klicken Sie auf die **Abonnieren von Benachrichtigungen** Schaltfläche:

[![Abonnieren von Benachrichtigungen-Schaltfläche](remote-notifications-with-fcm-images/14-subscribe-sml.png)](remote-notifications-with-fcm-images/14-subscribe.png)

Wenn die app erfolgreich abonniert hat, sehen Sie **Thema Sync erfolgreich** in der IDE Ausgabefenster: 

[![Fenster "Ausgabe" zeigt die erfolgreiche Synchronisierung themanachricht](remote-notifications-with-fcm-images/15-topic-sync-sml.png)](remote-notifications-with-fcm-images/15-topic-sync.png)

Verwenden Sie zum Senden einer themanachricht die folgenden Schritte aus:

1.  Klicken Sie in der Konsole Firebase auf **neue Nachricht**. 

2.  Auf der **Nachricht verfassen** Seite Geben Sie den Nachrichtentext, und wählen Sie **Thema**. 

3.  In der **Thema** Pulldownmenü, wählen Sie das integrierte Thema **News**: 

    [ ![Das Thema News auswählen](remote-notifications-with-fcm-images/16-topic-message-sml.png)](remote-notifications-with-fcm-images/16-topic-message.png)

4.  Für die Android-Gerät (oder -Emulator) im Hintergrund in der app durch Tippen auf das Android **Übersicht** Schaltfläche und den Startbildschirm berühren. 

5.  Wenn das Gerät bereit ist, klicken Sie auf **SEND MESSAGE** in der Konsole Firebase. 

6.  Überprüfen Sie die IDE-Ausgabefenster, finden Sie unter **/Themen/News** in die Ausgabe des Protokolls: 

    [ ![Nachricht von /topic/news wird angezeigt.](remote-notifications-with-fcm-images/17-message-arrived-sml.png)](remote-notifications-with-fcm-images/17-message-arrived.png)

Wenn diese Meldung im Ausgabefenster angezeigt wird, sollte das Symbol "Benachrichtigung" auch im Infobereich auf das Android-Gerät angezeigt werden. Öffnen Sie das Benachrichtigungssymbol, um die themanachricht anzuzeigen: 

[![Die themanachricht wird als eine Benachrichtigung angezeigt.](remote-notifications-with-fcm-images/18-other-news-sml.png)](remote-notifications-with-fcm-images/18-other-news.png)

Versuchen Sie es löschen, wenn Sie keine Nachricht erhält, den **FCMClient** app auf dem Gerät (oder -Emulator), und wiederholen Sie die oben genannten Schritte. 

## <a name="foreground-notifications"></a>Vordergrund-Benachrichtigungen

Um Benachrichtigungen in foregrounded-apps zu erhalten, müssen Sie implementieren `FirebaseMessagingService`. Dieser Dienst ist auch für den Empfang von datennutzlasten und zum Senden von Nachrichten upstream erforderlich. Die folgenden Beispiele veranschaulichen, wie einen Dienst implementiert, der erweitert `FirebaseMessagingService` &ndash; resultierende app wird in der Lage, remote Benachrichtigungen zu verarbeiten, während er im Vordergrund ausgeführt wird. 

### <a name="implement-firebasemessagingservice"></a>Implement FirebaseMessagingService

Die `FirebaseMessagingService` Dienst umfasst eine `OnMessageReceived` -Methode, die Sie schreiben, um eingehende Nachrichten zu verarbeiten. Beachten Sie, dass `OnMessageReceived` wird aufgerufen, für die benachrichtigungsmeldungen *nur* Wenn die app im Vordergrund ausgeführt wird. Wenn die app im Hintergrund ausgeführt wird, wird eine automatisch generierte Benachrichtigung angezeigt (wie weiter oben in dieser exemplarischen Vorgehensweise demonstriert). 

Fügen Sie eine neue Datei namens **MyFirebaseMessagingService.cs** und seine Vorlagencode durch Folgendes ersetzen: 

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Media;
using Android.Util;
using Firebase.Messaging;

namespace FCMClient
{
    [Service]
    [IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
    public class MyFirebaseMessagingService : FirebaseMessagingService
    {
        const string TAG = "MyFirebaseMsgService";
        public override void OnMessageReceived(RemoteMessage message)
        {
            Log.Debug(TAG, "From: " + message.From);
            Log.Debug(TAG, "Notification Message Body: " + message.GetNotification().Body);
        }
    }
}
```

Beachten Sie, dass die `MESSAGING_EVENT` beabsichtigte Filter muss deklariert werden, sodass neue FCM Nachrichten geleitet werden `MyFirebaseMessagingService`: 

```csharp
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
```

Wenn die Client-app aus FCM, eine Nachricht empfängt `OnMessageReceived` extrahiert den Nachrichteninhalt aus dem übergebenen `RemoteMessage` Objekt durch Aufrufen seiner `GetNotification` Methode. Als Nächstes protokolliert es den Nachrichteninhalt, damit es in das Ausgabefenster IDE angezeigt werden kann: 

```csharp
Log.Debug(TAG, "From: " + message.From);
Log.Debug(TAG, "Notification Message Body: " + message.GetNotification().Body);
```

> [!NOTE]
> **Hinweis:** , wenn Sie in Haltepunkte `FirebaseMessagingService`, die Debugsitzung kann oder diese Haltepunkte aufgrund wie FCM Nachrichten übermittelt kann nicht erreicht.
 

### <a name="send-another-message"></a>Eine andere Nachricht senden

Die app deinstalliert werden, erstellen Sie ihn neu, führen Sie es erneut aus und führen Sie diese Schritte aus, um eine weitere Nachricht zu senden:

1.  Klicken Sie in der Konsole Firebase auf **neue Nachricht**. 

2.  Auf der **Nachricht verfassen** Seite Geben Sie den Nachrichtentext, und wählen Sie **Einzelgerät**. 

3.  Kopieren Sie die Tokenzeichenfolge aus dem IDE-Ausgabefenster aus, und fügen Sie ihn in die **FCM Registrierung Token** Feld der Konsole Firebase wie zuvor. 

4.  Stellen Sie sicher, dass die app im Vordergrund ausgeführt wird, und klicken Sie auf **SEND MESSAGE** in der Konsole Firebase: 

    [ ![Senden eine andere Meldung in der Konsole](remote-notifications-with-fcm-images/19-hello-again-sml.png)](remote-notifications-with-fcm-images/19-hello-again.png)

5.  Wenn die **Überprüfung Nachricht** Dialogfeld angezeigt wird, klicken Sie auf **senden**.

6.  Die eingehende Nachricht wird in der IDE-Ausgabefenster protokolliert:

    [ ![Nachrichtentext gedruckt, Fenster "Ausgabe"](remote-notifications-with-fcm-images/20-logged-message.png)](remote-notifications-with-fcm-images/20-logged-message.png)


### <a name="add-a-local-notifications-sender"></a>Hinzufügen eines Absenders lokalen Benachrichtigungen

In diesem Beispiel verbleibenden wird die eingehende Nachricht der FCM in eine lokale Benachrichtigung konvertiert werden, die gestartet wird, während die app im Vordergrund ausgeführt wird. Bearbeiten Sie **MyFirebaseMessageService.cs** und fügen Sie die folgenden `using` Anweisungen: 

```csharp
using FCMClient;
using System.Collections.Generic;
```

Fügen Sie die folgende Methode hinzu `MyFirebaseMessagingService`: 

```csharp
void SendNotification(string messageBody, IDictionary<string, string> data)
{
    var intent = new Intent(this, typeof(MainActivity));
    intent.AddFlags(ActivityFlags.ClearTop);
    foreach (string key in data.Keys)
    {
        intent.PutExtra(key, data[key]);
    }
    var pendingIntent = PendingIntent.GetActivity(this, 0, intent, PendingIntentFlags.OneShot);

    var notificationBuilder = new Notification.Builder(this)
        .SetSmallIcon(Resource.Drawable.ic_stat_ic_notification)
        .SetContentTitle("FCM Message")
        .SetContentText(messageBody)
        .SetAutoCancel(true)
        .SetContentIntent(pendingIntent);

    var notificationManager = NotificationManager.FromContext(this);
    notificationManager.Notify(0, notificationBuilder.Build());
}
```

Um diese Benachrichtigung aus Hintergrund Benachrichtigungen zu unterscheiden, wird dieser Code Benachrichtigungen mit einem Symbol, die das Symbol "Applicatiion" nicht gekennzeichnet. Hinzufügen [ic\_Stat\_ic\_notification.png](remote-notifications-with-fcm-images/ic-stat-ic-notification.png) auf **Ressourcen und Ausgaben möglich** und fügen Sie ihn in die **FCMClient** Projekt. 

Die `SendNotification` -Methode verwendet `Notification.Builder` So erstellen Sie die Benachrichtigung und `NotificationManager` wird verwendet, um die Benachrichtigung zu starten. Die Benachrichtigung enthält eine `PendingIntent` ermöglicht, die dem Benutzer zum Öffnen der app, und zeigen Sie den Inhalt der übergebenen Zeichenfolge `messageBody`. Abhängig von den Versionen von Android, die Sie mit der app abzielen möchten, möchten Sie möglicherweise verwenden `NotificationCompat.Builder` stattdessen. Weitere Informationen zu `Notification.Builder`, finden Sie unter [lokale Benachrichtigungen](~/android/app-fundamentals/notifications/local-notifications.md). 

Rufen Sie die `SendNotification` Methode am Ende der `OnMessageReceived` Methode: 

```csharp
SendNotification(message.GetNotification().Body, message.Data);
```

Als Ergebnis dieser Änderungen `SendNotification` wird ausgeführt, wenn eine Benachrichtigung empfangen wird, während die app im Vordergrund, und die Benachrichtigung wird im Infobereich angezeigt. Wenn die app backgrounded ist, `SendNotification` wird nicht ausgeführt werden soll, und stattdessen eine Hintergrund-Benachrichtigung (weiter oben in dieser exemplarischen Vorgehensweise dargestellt) wird gestartet. 

### <a name="send-the-last-message"></a>Die letzte Nachricht senden

Die app deinstalliert werden, erstellen Sie ihn neu, führen Sie es erneut aus, und verwenden Sie die folgenden Schritte aus, um die letzte Nachricht zu senden:

1.  Klicken Sie in der Konsole Firebase auf **neue Nachricht**. 

2.  Auf der **Nachricht verfassen** Seite Geben Sie den Nachrichtentext, und wählen Sie **Einzelgerät**. 

3.  Kopieren Sie die Tokenzeichenfolge aus dem IDE-Ausgabefenster aus, und fügen Sie ihn in die **FCM Registrierung Token** Feld der Konsole Firebase wie zuvor. 

4.  Stellen Sie sicher, dass die app im Vordergrund ausgeführt wird, und klicken Sie auf **SEND MESSAGE** in der Konsole Firebase: 

    [ ![Senden der Nachricht Vordergrund](remote-notifications-with-fcm-images/21-console-fg-msg-sml.png)](remote-notifications-with-fcm-images/21-console-fg-msg.png)

Dieses Mal die Meldung, die im Ausgabefenster protokolliert wurde auch in eine neue Benachrichtigung verpackt &ndash; das Benachrichtigungssymbol in der Taskleiste angezeigt wird, während die app im Vordergrund ausgeführt wird: 

[![Benachrichtigungssymbol für Vordergrund-Nachricht](remote-notifications-with-fcm-images/22-foreground-icon-sml.png)](remote-notifications-with-fcm-images/22-foreground-icon.png)

Wenn Sie die Benachrichtigung öffnen, sehen Sie die letzte Meldung, die von der GUI Firebase Konsole Benachrichtigungen gesendet wurde: 

[![Vordergrund-Benachrichtigung mit Symbol "Foreground" angezeigt](remote-notifications-with-fcm-images/23-foreground-msg-sml.png)](remote-notifications-with-fcm-images/23-foreground-msg.png)

 
## <a name="troubleshooting"></a>Problembehandlung

Die folgenden beschreiben Probleme und problemumgehungen, die auftreten können, wenn mit Xamarin.Android Firebase Cloud Messaging verwendet werden.

### <a name="firebaseapp-is-not-initialized"></a>FirebaseApp ist nicht initialisiert.

In einigen Fällen können Sie diese Fehlermeldung angezeigt:

```shell
Java.Lang.IllegalStateException: Default FirebaseApp is not initialized in this process
Make sure to call FirebaseApp.initializeApp(Context) first.
```

Dies ist ein bekanntes Problem, das Sie umgehen können, indem Sie die Projektmappe zu bereinigen und erneuten Erstellen des Projekts (**erstellen > Projektmappe bereinigen**, **erstellen > Projektmappe neu erstellen**). Weitere Informationen finden Sie in diesem [Forumsdiskussion](https://forums.xamarin.com/discussion/96263/default-firebaseapp-is-not-initialized-in-this-process).


## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise detailliert die Schritte zur Implementierung von remote Benachrichtigungen Firebase Cloud Messaging in einer Anwendung Xamarin.Android. Er beschreibt, wie zum Installieren der erforderlichen Pakete für die Kommunikation FCM benötigt, und es wird erläutert, wie das Android-Manifest für den Zugriff auf FCM Servern konfigurieren. Diese angegeben, Beispielcode, der veranschaulicht, wie überprüft das Vorhandensein von Google Play-Dienste. Es veranschaulicht, wie eine Instanz-ID-Listenerdienst zu implementieren, die ein Token für die Registrierung mit FCM ausgehandelt, und diese erläutert, wie dieser Code im Hintergrund Benachrichtigungen erstellt, während die app backgrounded ist. Es wird erläutert, wie zum Thema Nachrichten abonnieren, und diese angegeben, dass einer beispielimplementierung von einer Nachricht Listenerdienst, der verwendet wird, zum Empfangen und remote Benachrichtigungen anzeigen, während die app im Vordergrund ausgeführt wird. 


## <a name="related-links"></a>Verwandte Links

- [FCMNotifications (Beispiel)](https://developer.xamarin.com/samples/monodroid/Firebase/FCMNotifications)
- [Firebase Cloud-Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)
