---
title: Senden von Pushbenachrichtigungen in iOS
description: Dieses Dokument beschreibt das Arbeiten mit Pushbenachrichtigungen in iOS 9 und früher. Es wird erläutert, Zertifikaten, Registrieren mit der Apple Push Notifications Gateway Service (APNS) und vieles mehr.
ms.prod: xamarin
ms.assetid: 64B3BE6A-A3E2-4B1B-95ED-02D27A8FDAAC
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 8ad742607e506df436a5526d31621ac7636ac29b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61087046"
---
# <a name="push-notifications-in-ios"></a>Senden von Pushbenachrichtigungen in iOS

> [!IMPORTANT]
> Die Informationen in diesem Abschnitt beziehen sich auf iOS 9 und vorherigen, es wurde keine hier zur Unterstützung von älterer iOS-Versionen. IOS 10 und höher, finden Sie unter den [Handbuch für die Benutzer-Notification-Framework](~/ios/platform/user-notifications/index.md) für die Unterstützung von sowohl lokale als auch Remote-Benachrichtigung auf iOS-Geräten.

Erste Schritte mit Pushbenachrichtigungen sollte kurz gehalten werden und nur genügend Daten für der mobilen Anwendung zu benachrichtigen, dass sie die Server-Anwendung nach einem Update wenden. Z. B., wenn neue e-Mail eingeht, würde die Server-Anwendung nur die mobile Anwendung benachrichtigen, die neue e-Mail angekommen ist. Die Benachrichtigung würde die neue e-Mail selbst nicht enthalten. Die mobile Anwendung würde der neuen e-Mails klicken Sie dann vom Server abgerufen werden, während sie die entsprechenden war

In der Mitte des Push-Benachrichtigungen unter iOS ist die *Apple Push Notification-Gateway-Dienst (APNS)*. Dies ist ein Dienst von Apple, die für das routing von Benachrichtigungen von einem Anwendungsserver auf iOS-Geräten zuständig ist.
Die folgende Abbildung veranschaulicht die Topologie des Push-Benachrichtigung für iOS: ![](remote-notifications-in-ios-images/image4.png "Diese Abbildung zeigt die Push-Benachrichtigung-Topologie für iOS")

Remotebenachrichtigungen selbst sind JSON-formatierte Zeichenfolgen, die das Format entsprechen, und Protokolle, die im angegebenen [die Benachrichtigungsnutzlast](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/CreatingtheNotificationPayload.html#//apple_ref/doc/uid/TP40008194-CH10-SW1) Teil der [lokalen und Push Notification Programming Guide](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)in die [iOS-Entwicklerdokumentation](https://developer.apple.com/devcenter/ios/index.action).

Apple verwaltet zwei Umgebungen von APNS: eine *Sandbox* und *Produktion* Umgebung. Die Sandbox-Umgebung für Tests während der Entwicklungsphase gedacht ist, und finden Sie unter `gateway.sandbox.push.apple.com` 2195 am TCP-port. Die produktionsumgebung dient in Anwendungen verwendet werden, die bereitgestellt wurden, und finden Sie unter `gateway.push.apple.com` 2195 am TCP-port.

## <a name="requirements"></a>Anforderungen

Pushbenachrichtigung erhalten, muss die folgenden Regeln beachten, die von der Architektur von APNS vorgegeben werden:

-  **256 Byte Nachrichtengrenze** – die Größe der gesamten Nachricht der Benachrichtigung darf 256 Bytes nicht überschreiten.
-  **Keine Empfangsbestätigung** -APNS bietet keine den Absender Benachrichtigung, die eine Nachricht an dem vorgesehenen Empfänger vorgenommen. Wenn das Gerät nicht erreichbar ist, und mehrere sequenzielle Benachrichtigungen gesendet werden, gehen alle Benachrichtigungen, mit Ausnahme der letzten verloren. Nur die neueste Benachrichtigung wird an das Gerät übermittelt werden.
-  **Jede Anwendung erfordert ein Zertifikat für sicheres** -Kommunikation mit APNS über SSL erfolgen muss.


## <a name="creating-and-using-certificates"></a>Erstellen und Verwenden von Zertifikaten

Ihr eigenes Zertifikat für jede der im vorherigen Abschnitt erwähnten Umgebungen erforderlich. In diesem Abschnitt wird beschrieben, wie ein Zertifikat erstellen, ordnen sie ein Bereitstellungsprofil und rufen Sie anschließend ein privater Informationsaustausch-Zertifikat für die Verwendung mit PushSharp.

1.  Zum Erstellen wechseln Sie einer Zertifikate für den iOS-Bereitstellungsportals von Apple-Website wie im folgenden Screenshot (Beachten Sie das App-IDs-Menüelement auf der linken Seite) gezeigt:

    [![](remote-notifications-in-ios-images/image5new.png "Die iOS-Bereitstellungsportal auf Apple-website")](remote-notifications-in-ios-images/image5new.png#lightbox)

2.  Klicken Sie anschließend navigieren Sie zu der App-IDs-Bereich, und erstellen Sie eine neue app-ID ein, wie im folgenden Screenshot gezeigt:

    [![](remote-notifications-in-ios-images/image6new.png "Navigieren Sie zu der App-IDs-Bereich, und erstellen Sie eine neue app-ID")](remote-notifications-in-ios-images/image6new.png#lightbox)

3.  Wenn Sie beim Klicken auf die **+** Schaltfläche, Sie werden in der Lage, geben die Beschreibung und eine Bündel-ID für die app-ID, wie im folgenden Screenshot gezeigt:

    [![](remote-notifications-in-ios-images/image7new.png "Geben Sie die Beschreibung und eine Bündel-ID für die app-ID")](remote-notifications-in-ios-images/image7new.png#lightbox)

4. Stellen Sie sicher, dass **Explicit App ID** und, die die Bündel-ID endet nicht mit einem `*` . Hiermit wird einen Bezeichner, der für mehrere Anwendungen gut ist, und Notification-Push-Zertifikate müssen für eine einzelne Anwendung sein.

1. Wählen Sie die App-Dienste, **Pushbenachrichtigungen**:

    [![](remote-notifications-in-ios-images/image8new.png "Erste Schritte mit Pushbenachrichtigungen aktivieren")](remote-notifications-in-ios-images/image8new.png#lightbox)

2. Drücken Sie **senden** , Registrierung von in die neue App-ID zu bestätigen:

    [![](remote-notifications-in-ios-images/image9new.png "Bestätigen Sie die Registrierung von in die neue App-ID")](remote-notifications-in-ios-images/image9new.png#lightbox)

3.  Als Nächstes müssen Sie das Zertifikat für die app-ID erstellen. Navigieren Sie im linken Navigationsbereich zu **Zertifikate > alle** , und wählen Sie die `+` Schaltfläche wie im folgenden Screenshot gezeigt:

    [![](remote-notifications-in-ios-images/image10new.png "Erstellen Sie das Zertifikat für die app-ID")](remote-notifications-in-ios-images/image8.png#lightbox)

4.  Wählen Sie, ob Sie ein Zertifikat für Entwicklung oder Produktion verwenden möchten:

    [![](remote-notifications-in-ios-images/image11new.png "Wählen Sie ein Zertifikat für Entwicklung oder Produktion")](remote-notifications-in-ios-images/image11new.png#lightbox)

5. Ein, und wählen Sie dann auf die neue App-ID, die wir gerade erstellt haben:

    [![](remote-notifications-in-ios-images/image12new.png "Wählen Sie die soeben erstellte neue App-ID")](remote-notifications-in-ios-images/image12new.png#lightbox)

6.  Dadurch werden die Anweisungen, mit denen Sie über den Prozess der Erstellung werden angezeigt ein *-Zertifikatsignieranforderung anfordern* mithilfe der **Keychain-Zugriff** Anwendung auf Ihrem Mac.

7.  Nun, dass das Zertifikat erstellt wurde, muss sie verwendet werden als Teil des Buildprozesses zum Signieren der Anwendung, sodass es bei APNs registriert werden kann. Dies erfordert, erstellen und installieren ein Bereitstellungsprofil erstellen, die das Zertifikat verwendet.

8.  Navigieren Sie zum Erstellen eines entwicklungsbereitstellungsprofils zu den **Bereitstellungsprofile** aus, und führen Sie die Schritte zum Erstellen, verwenden die App-Id, die wir gerade erstellt haben.

9.  Nachdem Sie das Bereitstellungsprofil erstellt haben, öffnen Sie **Xcode-Organisator** und aktualisiert. Wenn das Bereitstellungsprofil, die Sie erstellt wird, dass es möglicherweise erforderlich sein, das Profil aus der iOS-Bereitstellungsportal herunterladen und manuell importieren nicht angezeigt. Der folgende Screenshot zeigt ein Beispiel der Planer, mit der Bereitstellung-Profil hinzugefügt:  
    [![](remote-notifications-in-ios-images/image13new.png "Dieser Screenshot zeigt ein Beispiel für den Planer mit der Bereitstellung-Profil hinzugefügt")](remote-notifications-in-ios-images/image13new.png#lightbox)

10.  An dieser Stelle müssen wir so konfigurieren Sie das Xamarin.iOS-Projekt, um diese neu erstellte Bereitstellungsprofil verwenden. Dies erfolgt durch **Projektoptionen** Dialogfeld unter **iOS Bundle-Signierung** Registerkarte wie im folgenden Screenshot gezeigt:  
    [![](remote-notifications-in-ios-images/image11.png "Konfigurieren Sie das Xamarin.iOS-Projekt, um diese neu erstellte Bereitstellungsprofil verwenden")](remote-notifications-in-ios-images/image11.png#lightbox)

An diesem Punkt ist die Anwendung für die Arbeit mit Pushbenachrichtigungen konfiguriert. Es gibt jedoch immer noch ein paar mehr Schritte erforderlich, mit dem Zertifikat. Dieses Zertifikat wird in DER-Format, das nicht kompatibel mit PushSharp, ist ein Personal Information Exchange (PKCS12)-Zertifikat erfordert. Führen Sie diese letzte Schritte aus, um das Zertifikat zu konvertieren, damit er von PushSharp verwendet werden kann:

1.  **Herunterladen der Zertifikatdatei** -Anmeldung für den iOS-Bereitstellungsportal, wählen die Registerkarte "Zertifikate", wählen Sie das Zertifikat, das korrekte Bereitstellungsprofil, und wählen zugeordnet **herunterladen** .
1.  **Öffnen Sie die Schlüsselbundverwaltung** -Anwendung ist eine Grafische Benutzeroberfläche, um das Kennwort-Management-System unter OS X.
1.  **Importieren Sie das Zertifikat** : Wenn der zertifizierte noch nicht installiert ist, **Datei... Importieren von Elementen** Menü der Keychain-Zugriff. Navigieren Sie zu dem Zertifikat, das oben exportiert, und wählen Sie ihn.
1.  **Exportieren des Zertifikats** : das Zertifikat zu erweitern, und der zugehörigen private Schlüssel angezeigt wird, mit der rechten Maustaste auf den Schlüssel und ausgewählten exportieren. Sie werden für einen Dateinamen und ein Kennwort für die exportierte Datei aufgefordert werden.

An diesem Punkt sind wir mit Zertifikaten fertig. Wir haben erstellt ein Zertifikat, das zum Signieren von iOS-Anwendungen verwendet werden, und konvertiert dieses Zertifikat in einem Format, das in einer Serveranwendung mit PushSharp verwendet werden kann. Nächste sehen wir uns an, wie iOS-Anwendungen mit APNS zu interagieren.

## <a name="registering-with-apns"></a>Registrierung mit APNS

Bevor eine iOS kann Anwendung remote Benachrichtigung empfangen, die sie mit APNS registrieren muss. APNS wird eine eindeutige Geräte-Token generieren und zurückgeben, um die iOS-Anwendung. Die iOS-Anwendung wird führen Sie dann das gerätetoken und selbst dann mit dem Anwendungsserver registrieren. Nachdem Sie alle in diesem Fall Registrierung abgeschlossen ist, und der Anwendungsserver kann Pushbenachrichtigungen an das mobile Gerät.

Theoretisch kann das gerätetoken jedes Mal eine iOS-Anwendung selbst bei APNS registriert ändern, jedoch in der Praxis dies nicht, die häufig geschieht. Als Optimierung kann eine Anwendung das neueste gerätetoken Zwischenspeichern und nur Application Server aktualisieren, wenn es geändert wird. Das folgende Diagramm veranschaulicht den Prozess der Registrierung und Abrufen eines gerätetokens:

 ![](remote-notifications-in-ios-images/image12.png "Dieses Diagramm veranschaulicht den Prozess der Registrierung und eine Geräte-Token abrufen")

Die Registrierung mit APNS erfolgt der `FinishedLaunching` Methode der Klasse durch Aufrufen von Delegaten `RegisterForRemoteNotificationTypes` auf dem aktuellen `UIApplication` Objekt. Wenn eine iOS-Anwendung mit APNS registriert wird, müssen sie auch angeben, welche Arten von remotebenachrichtigungen es empfangen möchte. Diese remote-Benachrichtigung-Typen werden in der Enumeration deklariert `UIRemoteNotificationType`. Der folgende Codeausschnitt ist ein Beispiel wie eine iOS-Anwendung registrieren kann, um die Warnung "und"-Badge remotebenachrichtigungen zu erhalten:

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
    var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                       UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
                       new NSSet ());

    UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
    UIApplication.SharedApplication.RegisterForRemoteNotifications ();
} else {
    UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
    UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (notificationTypes);
}
```

Die Anforderung der APNS-Registrierung erfolgt im Hintergrund – Wenn die Antwort empfangen wird, iOS wird die-Methode aufgerufen `RegisteredForRemoteNotifications` in die `AppDelegate` Klasse und übergeben Sie das Token registriertes Gerät. Das Token sind in einer `NSData` Objekt. Der folgende Codeausschnitt zeigt, wie das gerätetoken abrufen, APNS bereitgestellt:

```csharp
public override void RegisteredForRemoteNotifications (
UIApplication application, NSData deviceToken)
{
    // Get current device token
    var DeviceToken = deviceToken.Description;
    if (!string.IsNullOrWhiteSpace(DeviceToken)) {
        DeviceToken = DeviceToken.Trim('<').Trim('>');
    }

    // Get previous device token
    var oldDeviceToken = NSUserDefaults.StandardUserDefaults.StringForKey("PushDeviceToken");

    // Has the token changed?
    if (string.IsNullOrEmpty(oldDeviceToken) || !oldDeviceToken.Equals(DeviceToken))
    {
        //TODO: Put your own logic here to notify your server that the device token has changed/been created!
    }

    // Save new device token
    NSUserDefaults.StandardUserDefaults.SetString(DeviceToken, "PushDeviceToken");
}
```

Wenn die Registrierung ein Fehler auftritt, die aus irgendeinem Grund (z. B. das Gerät ist nicht verbunden mit dem Internet), ruft iOS `FailedToRegisterForRemoteNotifications` Delegatklasse für die Anwendung. Der folgende Codeausschnitt zeigt, wie eine Warnung für den Benutzer darüber zu informieren, die Fehler bei der Registrierung angezeigt wird:

```csharp
public override void FailedToRegisterForRemoteNotifications (UIApplication application , NSError error)
{
    new UIAlertView("Error registering push notifications", error.LocalizedDescription, null, "OK", null).Show();
}
```

### <a name="device-token-housekeeping"></a>Geräte-Token-Wartungsaufgaben

Gerätetoken werden ablaufen oder sich die im Laufe der Zeit ändern. Aufgrund dieser Tatsache müssen serveranwendungen einige Haus bereinigen und löschen diese Token ist abgelaufen oder geänderten. Wenn eine Anwendung als Pushbenachrichtigungen an ein Gerät, das ein abgelaufenes Token verfügt sendet, wird APNS aufzeichnen und speichern Sie das abgelaufene Token. Server können dann Abfragen, APNS, um herauszufinden, welche Token abgelaufen sind.

APNS verwendet wird, zu einer *Feedback Service* -einen HTTPS-Endpunkt, der über das Zertifikat authentifiziert, die erstellt wurde, zum Senden von Push Benachrichtigungen und sendet Daten über welche Token abgelaufen sind. Dies wurde als veraltet markiert, von Apple und entfernt.

Stattdessen wird ein neuer HTTP-Statuscode für den Fall, der zuvor von der Feedback-Dienst gemeldet wurde:

> 410 - das Geräte-Token ist nicht mehr aktiv ist, für das Thema.

Darüber hinaus eine neue `timestamp` Schlüssel der JSON-Daten werden im Antworttext:

> Wenn der Wert in der: Status-Header ist 410, der Wert dieses Schlüssels ist der letzte Zeitpunkt, die an der APNs wurde bestätigt, dass das gerätetoken nicht mehr gültig ist, für das Thema.
>
> Beendet das Senden von Pushbenachrichtigungen, bis das Gerät ein Token mit dem späteren Zeitstempel bei Ihrem Anbieter registriert.

## <a name="summary"></a>Zusammenfassung

In diesem Abschnitt stellen die grundlegenden Konzepte, um Pushbenachrichtigungen in iOS vor. Es erläutert die Rolle von der Apple Push Notification Gateway Service (APNS). Sie behandelt dann die Erstellung und Verwendung der Sicherheitszertifikate, die für APNS unverzichtbar sind. Zum Schluss in diesem Dokument fertig, mit einer Erläuterung zur Verwendung der Anwendungsserver die *Feedback Services* zum Beenden der nachverfolgung abgelaufen gerätetoken.


## <a name="related-links"></a>Verwandte Links

- [Benachrichtigungen – lokal und remote-Benachrichtigungen (Beispiel) veranschaulicht](https://developer.xamarin.com/samples/monotouch/Notifications/)
- [Lokale und Pushbenachrichtigungen für Entwickler](https://developer.apple.com/notifications/)
- [UIApplication](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIApplication)
- [UIRemoteNotificationType](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIRemoteNotificationType)
