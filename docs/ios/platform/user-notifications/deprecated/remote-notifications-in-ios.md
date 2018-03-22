---
title: Pushbenachrichtigungen in iOS
description: "Dieser Abschnitt befasst sich mit Pushbenachrichtigungen in iOS. Apple Push Notifications Gateway Service und die Rolle, die Wiedergabe in die publishing Benachrichtigungen an iOS-Anwendungen eingeführt. Es wird erläutert, wie die Sicherheitszertifikate erstellen, die notwendig sind, um Pushbenachrichtigungen zu aktivieren und zu besprechen. In diesem Abschnitt werden schließlich Teil der ordnungsaufgaben erläutert, die Anwendungsserver, zum Nachverfolgen der Clients für mobile Geräte ausführen müssen."
ms.topic: article
ms.prod: xamarin
ms.assetid: 64B3BE6A-A3E2-4B1B-95ED-02D27A8FDAAC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 3af74fb9d93e22e361f2e3db00961d7955eda689
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="push-notifications-in-ios"></a>Pushbenachrichtigungen in iOS

_Dieser Abschnitt befasst sich mit Pushbenachrichtigungen in iOS. Apple Push Notifications Gateway Service und die Rolle, die Wiedergabe in die publishing Benachrichtigungen an iOS-Anwendungen eingeführt. Es wird erläutert, wie die Sicherheitszertifikate erstellen, die notwendig sind, um Pushbenachrichtigungen zu aktivieren und zu besprechen. In diesem Abschnitt werden schließlich Teil der ordnungsaufgaben erläutert, die Anwendungsserver, zum Nachverfolgen der Clients für mobile Geräte ausführen müssen._

> [!IMPORTANT]
> Die Informationen in diesem Abschnitt beziehen sich auf iOS 9 und vorherigen, es ist noch hier zur Unterstützung von älterer iOS-Versionen. IOS 10 und höher, finden Sie unter der [Benachrichtigungsframeworks User Guide](~/ios/platform/user-notifications/index.md) für die Unterstützung von lokalen und Remote-Benachrichtigung auf einem iOS-Gerät.

Pushbenachrichtigungen sollten kurze beibehalten und nur wenig Daten, um der mobilen Anwendung zu benachrichtigen, dass er die Serveranwendung für ein Update kontaktiert werden sollte. Z. B., wenn neue e-Mail eingeht, würde die Anwendung für die nur die mobile Anwendung benachrichtigen, die neue e-Mail angekommen ist. Die Benachrichtigung würde nicht die neue e-Mail sich selbst enthalten. Die mobile Anwendung würde dann rufen Sie die neue e-Mail-Nachrichten vom Server entsprechende wurde

In der Mitte der Push-Benachrichtigungen in iOS ist die *Apple Push Notification-Gateway-Dienst (APNS)*. Dies ist ein Dienst bereitgestellt, die von Apple, die für das routing Benachrichtigungen von einem Anwendungsserver für iOS-Geräte zuständig ist.
Das folgende Bild zeigt die Push Notification-Topologie für iOS: ![ ] (remote-notifications-in-ios-images/image4.png "dieses Bild zeigt die Push Notification-Topologie für iOS")

Remote Benachrichtigungen selbst werden JSON-formatierte Zeichenfolgen, die das Format entsprechen und Protokolle, die im angegebenen [die Benachrichtigungsnutzlast](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/CreatingtheNotificationPayload.html#//apple_ref/doc/uid/TP40008194-CH10-SW1) Teil der [lokale und Push Notification Programmierhandbuch](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)in der [iOS-Entwicklerdokumentation](https://developer.apple.com/devcenter/ios/index.action).

Apple verwaltet zwei APNS-Umgebungen: eine *Sandkasten* und ein *Produktion* Umgebung. Der Sandkasten-Umgebung ist für den Test während der Entwicklungsphase gedacht und finden Sie unter `gateway.sandbox.push.apple.com` auf TCP-port 2195. Die produktionsumgebung sollen in Anwendungen verwendet werden, die bereitgestellt wurden, finden Sie unter `gateway.push.apple.com` auf TCP-port 2195.

## <a name="requirements"></a>Anforderungen

Pushbenachrichtigung muss die folgenden Regeln beachten, die von der Architektur des APNS vorgegeben sind:

-  **256 Byte Nachrichtengrenze** -die gesamte Nachrichtengröße der Benachrichtigung darf 256 Bytes nicht überschreiten.
-  **Keine Empfangsbestätigung** -APNS bietet keine den Absender mit Benachrichtigungen, die eine Nachricht an dem beabsichtigten Empfänger vorgenommen. Wenn das Gerät nicht erreichbar ist, und mehrere sequenzielle Benachrichtigungen gesendet werden, gehen alle Benachrichtigungen mit Ausnahme der letzten verloren. Nur die letzte Benachrichtigung wird an das Gerät übermittelt werden.
-  **Jede Anwendung erfordert ein gesichertes Zertifikat** -Kommunikation mit APNS über SSL erfolgen muss.


## <a name="creating-and-using-certificates"></a>Erstellen und Verwenden von Zertifikaten

Einem eigenen Zertifikat ist für die Umgebungen, die im vorherigen Abschnitt erwähnten all erforderlich. In diesem Abschnitt wird beschrieben, wie ein Zertifikat erstellen, mit einem Bereitstellungsprofil zuordnen, und klicken Sie dann ein privater Informationsaustausch-Zertifikat für die Verwendung mit PushSharp erhalten.

1.  Zum Erstellen go ein Zertifikate für den iOS-Bereitstellungsportal auf der Website von Apple, wie im folgenden Screenshot (Beachten Sie das Menüelement App-IDs auf der linken Seite) dargestellt:

    [![](remote-notifications-in-ios-images/image5new.png "Die iOS-Bereitstellungsportal Apples-Website")](remote-notifications-in-ios-images/image5new.png#lightbox)

2.  Als Nächstes navigieren Sie zum Abschnitt App-IDs aus, und erstellen Sie eine neue app-ID ein, wie im folgenden Screenshot gezeigt:

    [![](remote-notifications-in-ios-images/image6new.png "Navigieren Sie zum Abschnitt App-IDs und erstellen Sie eine neue app-ID")](remote-notifications-in-ios-images/image6new.png#lightbox)

3.  Wenn Sie beim Klicken auf die  **+**  Schaltfläche, Sie werden in der Lage, geben die Beschreibung und eine Paket-ID für die app-ID, wie in der nächste Screenshot dargestellt:

    [![](remote-notifications-in-ios-images/image7new.png "Geben Sie die Beschreibung und eine Paket-ID für die app-ID")](remote-notifications-in-ios-images/image7new.png#lightbox)

4. Stellen Sie sicher, dass **explizite App-ID** und, die die Paket-ID ist nicht mit Enden einer `*` . Dadurch wird einen Bezeichner, die sich gut für mehrere Anwendungen erstellt und Push Notification-Zertifikate müssen für eine einzelne Anwendung sein.

1. Wählen Sie unter "App-Dienste" **Pushbenachrichtigungen**:

    [![](remote-notifications-in-ios-images/image8new.png "Aktivieren Sie Pushbenachrichtigungen")](remote-notifications-in-ios-images/image8new.png#lightbox)

2. Drücken Sie **Absenden** Registrierung des neuen App-ID bestätigen:

    [![](remote-notifications-in-ios-images/image9new.png "Registrierung der neuen App-ID bestätigen")](remote-notifications-in-ios-images/image9new.png#lightbox)

3.  Als Nächstes müssen Sie das Zertifikat für die app-ID erstellen. Suchen Sie im linken Navigationsbereich, **Zertifikate > alle** , und wählen Sie die `+` Schaltfläche, wie im folgenden Screenshot gezeigt:

    [![](remote-notifications-in-ios-images/image10new.png "Erstellen Sie das Zertifikat für die app-ID")](remote-notifications-in-ios-images/image8.png#lightbox)

4.  Bestimmen Sie, ob Sie ein Zertifikat Entwicklung oder Produktion verwenden möchten:

    [![](remote-notifications-in-ios-images/image11new.png "Wählen Sie ein Zertifikat Entwicklung oder Produktion")](remote-notifications-in-ios-images/image11new.png#lightbox)

5. Und wählen Sie dann die neue App-ID, die wir gerade erstellt haben:

    [![](remote-notifications-in-ios-images/image12new.png "Wählen Sie die soeben erstellte neue App-ID")](remote-notifications-in-ios-images/image12new.png#lightbox)

6.  Anweisungen, die Sie durch die Erstellung dauert wird angezeigt ein *Zertifikatsignieranforderung* mithilfe der **Schlüsselbundverwaltung** Anwendung auf Ihrem Mac.

7.  Nun, dass das Zertifikat erstellt wurde, muss sie verwendet werden als Teil des Buildprozesses zum Signieren der Anwendung, damit es bei APNs registrieren kann. Dies erfordert, erstellen und installieren Sie ein Bereitstellungsprofil, die das Zertifikat verwendet.

8.  Um eine Entwicklung Bereitstellungsprofil erstellen, navigieren Sie zu der **Provisioning Profile** Abschnitt, und befolgen Sie die Schritte zum Erstellen, mit der App-Id, die wir gerade erstellt haben.

9.  Öffnen Sie nach der Erstellung der Bereitstellungsprofil **Xcode-Planer** und aktualisieren Sie sie. Wenn die provisioning-Profil, die Sie erstellt wird, dass es möglicherweise erforderlich sein, das Profil von iOS-Bereitstellungsportal herunterladen und importieren Sie ihn manuell nicht angezeigt. Der folgende Screenshot zeigt ein Beispiel für den Planer mit dem für die Bereitstellung Profil hinzugefügt:

    [![](remote-notifications-in-ios-images/image13new.png "Dieser Screenshot zeigt ein Beispiel für den Planer mit dem für die Bereitstellung Profil hinzugefügt")](remote-notifications-in-ios-images/image13new.png#lightbox)

10.  An diesem Punkt müssen wir das Xamarin.iOS-Projekt, um die neu erstellten provisioning-Profil verwenden, konfigurieren. Dies erfolgt aus **Projektoptionen** Dialogfeld unter **iOS Bundle Signing** Registerkarte, wie im folgenden Screenshot gezeigt:

    [![](remote-notifications-in-ios-images/image11.png "Konfigurieren Sie das Projekt Xamarin.iOS Verwendung neu erstellten provisioning-Profil")](remote-notifications-in-ios-images/image11.png#lightbox)



An diesem Punkt wird die Anwendung für die Zusammenarbeit mit Pushbenachrichtigungen konfiguriert. Es gibt jedoch immer noch einige weitere Schritte, die mit dem Zertifikat erforderlich. Dieses Zertifikat wird im Format der-KODIERTE, die nicht kompatibel mit PushSharp, wird ein Personal Information Exchange (PKCS12) Zertifikat erforderlich ist. Um das Zertifikat zu konvertieren, damit er von PushSharp verwendet werden kann, führen Sie folgenden abschließenden Schritte aus:

1.  **Laden Sie die Zertifikatdatei** -Anmeldung für den iOS-Bereitstellungsportal, ausgewählt haben, die Registerkarte "Zertifikate", wählen Sie das Zertifikat zugeordnet ist, mit der richtigen provisioning-Profil, und wählen Sie **herunterladen** .
1.  **Öffnen Sie Schlüsselbundverwaltung** -Anwendung ist eine GUI-Schnittstelle, um das Verwaltungssystem Kennwort unter OS X.
1.  **Importieren Sie das Zertifikat** – Wenn der zertifizierte noch nicht installiert ist, **Datei... Importieren von Elementen** aus dem Menü Schlüsselbundverwaltung. Navigieren Sie zu dem Zertifikat, das oben exportiert, und wählen Sie ihn.
1.  **Exportieren des Zertifikats** – das Zertifikat zu erweitern, sodass Sie der zugehörigen private Schlüssel angezeigt wird, mit der rechten Maustaste auf den Schlüssel und Export auswählen. Sie werden für einen Dateinamen und ein Kennwort für die exportierte Datei aufgefordert werden.

An diesem Punkt sind wir mithilfe von Zertifikaten fertig. Wir haben erstellt ein Zertifikat, das zum Signieren von iOS-Anwendungen verwendet werden, und konvertiert dieses Zertifikat in einem Format, das in einer Serveranwendung mit PushSharp verwendet werden kann. Nächste sehen wir uns die iOS-Anwendungen wie bei APNS zu interagieren.

## <a name="registering-with-apns"></a>Registrierung mit APNS

Bevor Sie ein iOS kann Anwendung remote Benachrichtigung erhalten, die sie bei APNS registrieren muss. APNS wird eine eindeutige Geräte-Token generieren und zurückzugeben, die für die iOS-Anwendung. IOS-Anwendung wird dann das gerätetoken vornehmen und registrieren Sie selbst dann für den Anwendungsserver ausgeführt. Sobald alle in diesem Fall Registrierung abgeschlossen ist, und der Anwendungsserver kann Pushbenachrichtigungen an das mobile Gerät.

Theoretisch kann das gerätetoken jedes Mal eine iOS-Anwendung mit APNS registriert ändern, jedoch in der Praxis dies nicht, die häufig geschieht. Zur Optimierung kann eine Anwendung die neueste gerätetoken Zwischenspeichern und aktualisiert nur den Anwendungsserver ausgeführt, wenn er geändert wird. Das folgende Diagramm veranschaulicht den Prozess der Registrierung und Bezug eines gerätetokens:

 ![](remote-notifications-in-ios-images/image12.png "Dieses Diagramm veranschaulicht den Prozess der Registrierung und Bezug eines gerätetokens")

Registrierung mit APNS erfolgt der `FinishedLaunching` Methode durch Aufrufen der Anwendung-Delegatklasse `RegisterForRemoteNotificationTypes` auf dem aktuellen `UIApplication` Objekt. Bei der Registrierung von iOS-Anwendung mit APNS, müssen sie auch angeben, welche Arten von remote Benachrichtigungen sie erhalten sollen. Diese remote-Benachrichtigung-Typen sind in der Enumeration deklariert `UIRemoteNotificationType`. Der folgende Codeausschnitt ist ein Beispiel, wie eine iOS-Anwendung registrieren kann, um remote Warnung und Badge-Benachrichtigungen zu erhalten:

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

APNs-registrierungsanforderung geschieht im Hintergrund - beim Empfang der Antwort wird iOS wird rufen Sie die Methode `RegisteredForRemoteNotifications` in die `AppDelegate` -Klasse und übergeben Sie das Token registriertes Gerät. Das Token ist enthalten ein `NSData` Objekt. Der folgende Codeausschnitt zeigt, wie das gerätetoken abgerufen, APNS bereitgestellt:

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

Wenn die Registrierung schlägt fehl (z. B. das Gerät ist nicht mit dem Internet verbunden), iOS ruft `FailedToRegisterForRemoteNotifications` für die Anwendung Delegatklasse. Der folgende Codeausschnitt zeigt, wie eine Warnung für den Benutzer informiert wird, die Fehler bei der Registrierung angezeigt wird:

```csharp
public override void FailedToRegisterForRemoteNotifications (UIApplication application , NSError error)
{
    new UIAlertView("Error registering push notifications", error.LocalizedDescription, null, "OK", null).Show();
}
```

### <a name="device-token-housekeeping"></a>Gerät Token Housekeeping

Gerätetoken werden ablaufen oder sich im Laufe der Zeit ändern. Aufgrund dieser Tatsache müssen serveranwendungen führen einige House bereinigen und löschen diese Token ist abgelaufen oder geändert wurden. Wenn eine Anwendung als Pushbenachrichtigung an ein Gerät, die einem abgelaufenen Token verfügt sendet, wird APNS erfassen und speichern Sie dieses Token abgelaufen. Server können dann abgefragt werden APNS, um herauszufinden, welche Token abgelaufen sind.

APNS verwendet, um bereitzustellen eine *Feedback Dienst* -einen HTTPS-Endpunkt, der über das Zertifikat authentifiziert, die erstellt wurde, um das Senden von Benachrichtigungen und sendet Back Daten per Push über welche Token abgelaufen sind. Dies wurde als veraltet eingestuft Apple und entfernt wurde.

Stattdessen wird ein neue HTTP-Statuscode für die Groß-/Kleinschreibung, die zuvor von der Feedback-Dienst gemeldet wurde:

> 410 - das gerätetoken ist nicht mehr für das Thema aktiv.

Darüber hinaus einen neuen `timestamp` Schlüssel der JSON-Daten werden im Antworttext gezeigt:

> Wenn der Wert in der: Status 410 ist, der Wert dieses Schlüssels wird der letzten, an dem APNs wurde bestätigt, dass das gerätetoken nicht mehr gültig ist, für das Thema.
>
> Beenden Sie die Übertragung von Benachrichtigungen, bis das Gerät ein Token mit einem höheren Zeitstempel mit Ihrem Anbieter registriert.

## <a name="summary"></a>Zusammenfassung

In diesem Abschnitt stellen die grundlegenden Konzepte, um Pushbenachrichtigungen in iOS vor. Diese erläutert der Rolle von den Apple Push Notification Gateway Service (APNS). Es behandelt dann die Erstellung und Verwendung der Sicherheitszertifikate, die für APNS unverzichtbar sind. Schließlich dieses Dokuments fertig, mit einer Erläuterung dazu, wie Anwendungsserver können die *Feedback Services* abgelaufene gerätetoken Überwachung zu beenden.


## <a name="related-links"></a>Verwandte Links

- [Benachrichtigungen – Demonstrieren der lokalen und remote-Benachrichtigungen (Beispiel)](https://developer.xamarin.com/samples/monotouch/Notifications/)
- [Lokale und Pushbenachrichtigungen für Entwickler](https://developer.apple.com/notifications/)
- [UIApplication](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIApplication)
- [UIRemoteNotificationType](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIRemoteNotificationType)
