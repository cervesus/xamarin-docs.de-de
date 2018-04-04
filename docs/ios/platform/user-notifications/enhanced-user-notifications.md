---
title: Verbesserte Benutzerbenachrichtigungen
description: Dieser Artikel umfasst alle von den Verfahren, die Benachrichtigung der Benutzer von iOS 10 und wie Sie Ihre Verwendung in einer app Xamarin.iOS erweitert wurden.
ms.prod: xamarin
ms.assetid: 4E1FF652-28F0-4566-B383-9D12664401A4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 9fd3ff17dc9af3fd30a7d5b31e8cea7ff8669a51
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="enhanced-user-notifications"></a>Verbesserte Benutzerbenachrichtigungen

_Dieser Artikel umfasst alle von den Verfahren, die Benachrichtigung der Benutzer von iOS 10 und wie Sie Ihre Verwendung in einer app Xamarin.iOS erweitert wurden._

Neue für iOS-10, ermöglicht das Framework für die Bereitstellung und Verarbeitung von lokalen und remote-Benachrichtigungen, erhalten. Mit diesem Framework, kann eine app oder App-Erweiterung die Übermittlung von lokalen Benachrichtigungen planen durch eine Reihe von Bedingungen, z. B. Speicherort oder die Uhrzeit angeben.

## <a name="about-user-notifications"></a>Informationen zu Benutzerbenachrichtigungen

Wie bereits erwähnt, können das neue Benutzerbenachrichtigung-Framework für die Bereitstellung und Verarbeitung von lokalen und remote-Benachrichtigungen. Mit diesem Framework, kann eine app oder App-Erweiterung die Übermittlung von lokalen Benachrichtigungen planen durch eine Reihe von Bedingungen, z. B. Speicherort oder die Uhrzeit angeben.

Darüber hinaus die app oder eine Erweiterung kann empfangen (und möglicherweise ändern) sowohl lokale als auch Benachrichtigungen, wie sie iOS-Gerät des Benutzers übermittelt werden.

Der neue Benutzer Benachrichtigung Benutzeroberflächen-Framework ermöglicht es, eine Anwendung oder ein App-Erweiterung, um die Darstellung des sowohl lokale als auch Benachrichtigungen anpassen, wenn sie dem Benutzer angezeigt werden.

Dieses Framework bietet, die eine app Benachrichtigungen an Benutzer übermitteln kann folgendermaßen:

- **Visual Warnungen** –, in dem die Benachrichtigung vom oberen Rand des Bildschirms als Banner hinunter.
- **Sound und Vibrationen** -kann eine Benachrichtigung zugeordnet werden.
- **App-Symbol Signalisierung** – die app-Symbol angezeigt wird, in dem ein Signal, das anzeigt, dass neuer Inhalte verfügbar, z. B. die Anzahl der ungelesene e-Mail-Nachrichten ist.

Darüber hinaus, abhängig vom aktuellen Kontext des Benutzers gibt es verschiedene Möglichkeiten, die eine Benachrichtigung angezeigt werden:

- Wenn das Gerät entsperrt wird, wird die Benachrichtigung vom oberen Rand des Bildschirms als Banner Abonnentenoptionen.
- Wenn das Gerät gesperrt ist, wird die Benachrichtigung auf dem Sperrbildschirm des Benutzers angezeigt werden.
- Wenn der Benutzer eine Benachrichtigung verpasst hat, können die Mitteilungszentrale öffnen und alle verfügbaren wartenden Benachrichtigungen anzeigen.

Eine Xamarin.iOS app verfügt über zwei Arten von Benachrichtigungen für Benutzer, die sie senden kann:

- **Lokale Benachrichtigungen** – diese werden von apps, die lokal auf dem Gerät des Benutzers installiert gesendet.
- **Remote Benachrichtigungen** -gesendet werden, von einem Remote-Server als auch dem Benutzer angezeigt, oder sie trigger ein Update im Hintergrund der app-Inhalte.

### <a name="about-local-notifications"></a>Informationen zu lokalen Benachrichtigungen

Die lokale Benachrichtigungen, die eine iOS-app senden kann haben die folgenden Features und Attribute:

- Sie werden von apps gesendet, die sich lokal auf dem Gerät des Benutzers befinden. 
- Sie sind kann so konfiguriert werden entsprechend Trigger Zeit oder Speicherort zu verwenden. 
- Die app plant die Benachrichtigung mit dem Gerät des Benutzers, und es wird angezeigt, wenn die auslöserbedingung erfüllt ist.
- Wenn der Benutzer mit der Benachrichtigung interagiert, wird die app einen Rückruf erhalten.

Einige Beispiele für lokale Benachrichtigungen:

- Calendar-Warnungen
- Erinnerung Warnungen
- Beachten Sie Trigger Speicherort

Weitere Informationen finden Sie in der Apple- [lokal und Remote Benachrichtigung Programmierhandbuch](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/) Dokumentation.

### <a name="about-remote-notifications"></a>Informationen zu Remote Benachrichtigungen

Die Remote-Benachrichtigungen, die eine iOS-app senden kann haben die folgenden Features und Attribute:

- Die app verfügt über eine serverseitige Komponente, der mit dem er kommuniziert.
- Apple Push Notification Service (APNs) Dient zum eine Best-Effort-Übermittlung von Remote-Benachrichtigungen auf dem Gerät des Benutzers von der Entwickler Cloud-basierten Servern übertragen werden können.
- Wenn die app die Remote-Benachrichtigung erhält, wird es dem Benutzer angezeigt.
- Wenn der Benutzer bei der Benachrichtigung interagiert, wird die app einen Rückruf erhalten.

Einige Beispiele für Remote Benachrichtigungen:

- News-Warnungen
- Sport-Updates
- Instant Messaging-Nachrichten

Es gibt zwei Arten von Remote-Benachrichtigungen an eine iOS-app verfügbar:

- **Verbundene Benutzer** -der Benutzer auf dem Gerät angezeigt werden.
- **Automatische Updates** -diese bieten einen Mechanismus zum Aktualisieren des Inhalts einer iOS-app im Hintergrund. Wenn eine automatische Aktualisierung empfangen wird, kann die app das Pull-Server entfernen Sie die neuesten Inhalte Remoteknoten.

Weitere Informationen finden Sie in der Apple- [lokal und Remote Benachrichtigung Programmierhandbuch](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/) Dokumentation.

### <a name="about-the-existing-notifications-api"></a>Über die vorhandenen API-Benachrichtigungen

Vor dem iOS-10, eine iOS-app verwenden `UIApplication` so eine Benachrichtigung mit dem System registriert und planen, wie dieser Benachrichtigung (entweder nach Zeit oder Speicherort) ausgelöst werden soll.

Es gibt mehrere Problem, das ein Entwickler bei der Arbeit mit der vorhandenen Benachrichtigung API auftreten kann:

- Es wurden verschiedene Rückrufe für lokales oder Remote Benachrichtigungen zu einer Duplizierung des Codes kommen könnte erforderlich.
- Die app hatte Kontrolle über die Benachrichtigung beschränkt, nachdem er mit dem System geplant wurde.
- Es wurden bietet unterschiedliche Stufen der Unterstützung für alle vorhandenen Apple-Plattformen.

### <a name="about-the-new-user-notification-framework"></a>Zu den neuen Benutzer Benachrichtigungsframeworks

Mit iOS-10, Apple wurde das neue Framework von Benachrichtigung für Benutzer, der ersetzt den vorhandenen eingeführt `UIApplication` Methode wie oben beschrieben.

Die Benutzerbenachrichtigung-Framework bietet Folgendes:

- Eine vertraut-API, die mit den vorherigen Methoden, die Dies erleichtert die Port-Code aus dem vorhandenen Framework Funktionsparität enthält.
- Enthält einen erweiterten Satz von Inhaltsoptionen, der umfangreichere Benachrichtigungen an Benutzer gesendet werden kann.
- Durch den Code und die Rückrufe können sowohl lokale als auch Remote Benachrichtigungen behandelt werden.
- Vereinfacht das Behandeln von Rückrufe, die an eine Anwendung gesendet werden, wenn der Benutzer mit der Benachrichtigung interagiert.
- Verbesserte Verwaltung von ausstehenden und übermittelten Benachrichtigungen, einschließlich der Möglichkeit zum Entfernen oder Aktualisieren von Benachrichtigungen.
- Fügt die Möglichkeit, die in-app-Darstellung von Benachrichtigungen führen.
- Bietet die Möglichkeit zum Planen und Verarbeiten von Benachrichtigungen von in-App-Erweiterungen.
- Fügt neue Erweiterungspunkt für die Benachrichtigungen selbst. 

Das neue Benutzerbenachrichtigung-Framework bietet eine einheitliche Benachrichtigung API für die mehrere Plattformen, einschließlich Apple unterstützt: 

- **iOS** -vollständige Unterstützung zum Verwalten und Planen von Benachrichtigungen.
- **tvos. außerdem wurden** -Signal-app-Symbole für lokale und remote-Benachrichtigungen die Möglichkeit hinzugefügt.
- **WatchOS** – bietet die Möglichkeit zum Weiterleiten von Benachrichtigungen von gepaarten iOS-Gerät des Benutzers auf ihrer Apple Watch und ermöglicht es Watch-apps, die lokale Benachrichtigungen direkt auf die Überwachung selbst tun.

Weitere Informationen finden Sie in der Apple- [UserNotifications Frameworkverweis](https://developer.apple.com/reference/usernotifications) und [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui) Dokumentation.

## <a name="preparing-for-notification-delivery"></a>Vorbereiten für die Übermittlung der Benachrichtigung

Bevor Sie ein iOS app Benachrichtigungen senden kann des Benutzers, den die app im System registriert werden muss und da eine Benachrichtigung eine Unterbrechung der Benutzer ist, muss eine app explizit Berechtigung vor dem Senden anfordert.

Es gibt drei verschiedene Stufen der benachrichtigungsanforderungen, die der Benutzer für eine app genehmigen kann:

- Banner angezeigt.
- Sound Warnungen.
- Signalisierung das Symbol "app".

Darüber hinaus müssen diese Genehmigung Ebenen angefordert und für sowohl lokale als auch Benachrichtigungen festgelegt werden.

Notification-Berechtigung darf angefordert werden, sobald die app gestartet wird, durch den folgenden Code zum Hinzufügen der `FinishedLaunching` Methode der `AppDelegate` und zum Festlegen des gewünschten Benachrichtigungstyp (`UNAuthorizationOptions`):

```csharp
using UserNotifications;
...

public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    // Request notification permissions from the user
    UNUserNotificationCenter.Current.RequestAuthorization (UNAuthorizationOptions.Alert, (approved, err) => {
        // Handle approval
    });

    return true;
}
```

Darüber hinaus kann ein Benutzer immer die Benachrichtigung über Berechtigungen für eine app aus einem beliebigen Zeitpunkt ändern die **Einstellungen** app auf dem Gerät. Die app sollte für die angeforderte Benachrichtigung Berechtigungen des Benutzers überprüfen Sie vor der Bereitstellung einer benachrichtigungs mit dem folgenden Code:

```csharp
// Get current notification settings
UNUserNotificationCenter.Current.GetNotificationSettings ((settings) => {
    var alertsAllowed = (settings.AlertSetting == UNNotificationSetting.Enabled);
}); 
``` 

### <a name="configuring-the-remote-notifications-environment"></a>Konfigurieren der Umgebung Remote Benachrichtigungen

Neu für iOS 10, muss der Entwickler informiert das Betriebssystem an, welche Umgebung Pushbenachrichtigung als Entwicklung oder Produktion ausgeführt werden. Geben Sie diese Informationen können in der app abgelehnt werden, bei der Übermittlung an den iTune Store-App mit der Benachrichtigung ähnlich der folgenden ansonsten:

> Fehlende Push Notification-Berechtigung - Ihre app enthält eine API für Apple Push Notification Service, aber die `aps-environment` Berechtigung fehlt in der app-Signatur.

Um die erforderliche Berechtigung zu ermöglichen, führen Sie folgende Schritte aus:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Doppelklicken Sie auf die `Entitlements.plist` in der Datei die **Lösung Pad** um ihn zur Bearbeitung zu öffnen.
2. Wechseln Sie zu der **Quelle** anzeigen: 

    [![](enhanced-user-notifications-images/setup01.png "Die Datenquellensicht")](enhanced-user-notifications-images/setup01.png#lightbox)
3. Klicken Sie auf die **+** Schaltfläche, um einen neuen Schlüssel hinzuzufügen.
4. Geben Sie `aps-environment` für die **Eigenschaft**, lassen Sie die **Typ** als `String` , und geben Sie entweder `development` oder `production` für die **Wert**: 

    [![](enhanced-user-notifications-images/setup02.png "Die Eigenschaft der Aps-Umgebung")](enhanced-user-notifications-images/setup02.png#lightbox)
5. Speichern Sie die Änderungen in der Datei.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Doppelklicken Sie auf die `Entitlements.plist` in der Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen.
3. Klicken Sie auf die **+** Schaltfläche, um einen neuen Schlüssel hinzuzufügen.
4. Geben Sie `aps-environment` für die **Eigenschaft**, lassen Sie die **Typ** als `String` , und geben Sie entweder `development` oder `production` für die **Wert**: 

    [![](enhanced-user-notifications-images/setup02w.png "Die Eigenschaft der Aps-Umgebung")](enhanced-user-notifications-images/setup02.png#lightbox)
5. Speichern Sie die Änderungen in der Datei.

-----

### <a name="registering-for-remote-notifications"></a>Registrieren für Remote-Benachrichtigungen

Wenn die app senden und Empfangen von Benachrichtigungen zu Remote, es müssen dennoch dazu _Token Registrierung_ mithilfe des vorhandenen `UIApplication` API. Diese Registrierung muss das Gerät eine live-Verbindung Netzwerkzugriff APNs aufweisen, die erforderliche Token generiert, das an die app gesendet werden. Die Anwendung muss dann dieses Token des Entwicklers Server Seite App registrieren für remote-Benachrichtigungen weiterleiten:

[![](enhanced-user-notifications-images/token01.png "Token-Registrierung (Übersicht)")](enhanced-user-notifications-images/token01.png#lightbox)

Verwenden Sie den folgenden Code zum Initialisieren der erforderlichen Registrierungs:

```csharp
UIApplication.SharedApplication.RegisterForRemoteNotifications ();
```

Das Token, das sich der Entwickler Server Side-app gesendet werden müssen als Teil der Benachrichtigungsnutzlast, Get vom Server zum APNs gesendet wird, wenn eine Remote-Benachrichtigung senden eingeschlossen werden sollen:

[![](enhanced-user-notifications-images/token02.png "Das Token als Teil der Nutzlast der Benachrichtigung enthalten")](enhanced-user-notifications-images/token02.png#lightbox)

Das Token dient als Schlüssel, der miteinander verknüpft werden, die Benachrichtigung und der app zu öffnen oder auf die Benachrichtigung reagieren, verwendet.

Weitere Informationen finden Sie in der Apple- [lokal und Remote Benachrichtigung Programmierhandbuch](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/) Dokumentation.

## <a name="notification-delivery"></a>Übermittlung der Benachrichtigung

Vollständig mit der app registriert und die erforderlichen Berechtigungen, die vom angefordert und erteilt durch den Benutzer die app kann jetzt zum Senden und Empfangen von Benachrichtigungen. 

### <a name="providing-notification-content"></a>Bereitstellen von Benachrichtigungsinhalt

Neu für iOS 10, alle Benachrichtigungen enthalten beide eine **Titel** und **Untertitel** , erscheint immer mit der **Text** der Inhalt der Benachrichtigung. Zudem neu, ist die Möglichkeit zum Hinzufügen **Medien Anlagen** auf den Inhalt der Benachrichtigung.

Um den Inhalt einer lokalen Anmeldung zu erstellen, verwenden Sie den folgenden Code:

```csharp
var content = new UNMutableNotificationContent();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;
```

Für Remote-Benachrichtigungen ist der Prozess ähnlich:

```csharp
{
    "aps":{
        "alert":{
            "title":"Notification Title",
            "subtitle":"Notification Subtitle",
            "body":"This is the message body of the notification."
        },
        "badge":1
    }
}
```

### <a name="scheduling-when-a-notification-is-sent"></a>Planen bei einer Benachrichtigung wird gesendet.

Mit dem Inhalt der Benachrichtigung erstellt, die app planen, wann die Benachrichtigung festlegen, die dem Benutzer angezeigt werden muss eine *Trigger*. iOS 10 bietet vier verschiedene Typen von Trigger:

- **Pushbenachrichtigungen** - dient ausschließlich mit Remote-Benachrichtigungen und wird ausgelöst, wenn ein APNs-sendet eine Benachrichtigung, die auf dem Gerät ausgeführte app-Paket.
- **Zeitintervall** -können Sie eine lokale Benachrichtigung aus einer Zeit geplant werden Intervall mit sofort aus, und Beenden einen einen zukünftigen Punkt beginnen. Beispiel: `var trigger =  UNTimeIntervalNotificationTrigger.CreateTrigger (5, false);`
- **Datum "Kalender"** -ermöglicht lokalen Benachrichtigungen für ein bestimmtes Datum und eine Uhrzeit geplant werden.
- **Speicherort basiert** -ermöglicht lokalen Benachrichtigungen, wenn das iOS-Gerät eingeben oder Verlassen einer bestimmten geografischen Ort oder in einer bestimmten Nähe Bluetooth-Beacons geplant werden sollen.

Wenn eine lokale Benachrichtigung bereit ist, muss die app zum Aufrufen der `Add` Methode der `UNUserNotificationCenter` Objekt, um die Anzeige für den Benutzer zu planen. Für Remote Benachrichtigungen sendet die serverseitige app eine Benachrichtigungsnutzlast an den APNs klicken Sie dann das Paket auf dem Gerät des Benutzers gesendet.

Zusammenführen alle Teile, könnte ein Beispiel für lokale Benachrichtigung formuliert werden:

```csharp
using UserNotifications;
...

var content = new UNMutableNotificationContent ();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;

var trigger =  UNTimeIntervalNotificationTrigger.CreateTrigger (5, false);

var requestID = "sampleRequest";
var request = UNNotificationRequest.FromIdentifier (requestID, content, trigger);

UNUserNotificationCenter.Current.AddNotificationRequest (request, (err) => {
    if (err != null) {
        // Do something with error...
    }
});
```

## <a name="handling-foreground-app-notifications"></a>Behandeln von Vordergrund-App-Benachrichtigungen

Neu für iOS 10, eine app kann behandeln Benachrichtigungen anders, wenn es im Vordergrund ist und eine Benachrichtigung ausgelöst wird. Durch die Bereitstellung einer `UNUserNotificationCenterDelegate` und Implementieren der `UserNotificationCenter` -Methode, die app kann übernehmen Verantwortung für die Benachrichtigung anzeigen. Zum Beispiel:

```csharp
using System;
using UserNotifications;

namespace MonkeyNotification
{
    public class UserNotificationCenterDelegate : UNUserNotificationCenterDelegate
    {
        #region Constructors
        public UserNotificationCenterDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void WillPresentNotification (UNUserNotificationCenter center, UNNotification notification, Action<UNNotificationPresentationOptions> completionHandler)
        {
            // Do something with the notification
            Console.WriteLine ("Active Notification: {0}", notification);

            // Tell system to display the notification anyway or use
            // `None` to say we have handled the display locally.
            completionHandler (UNNotificationPresentationOptions.Alert);
        }
        #endregion
    }
}
```

Dieser Code einfach schreiben den Inhalt der `UNNotification` der Anwendungsausgabe und fragt das System die standard-Warnung für die Benachrichtigung angezeigt. 

Wenn die app sollte die Benachrichtigung selbst angezeigt wird, wenn es in den Vordergrund gestellt wurde und nicht die Standardeinstellungen des Systems verwenden, übergeben `None` auf den Abschlusshandler. Beispiel:

```csharp
completionHandler (UNNotificationPresentationOptions.None);
```

Mit diesem Code werden, öffnen die `AppDelegate.cs` zum Bearbeiten der Datei, und ändern Sie die `FinishedLaunching` Methode gesucht werden soll, wie folgt:

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    // Request notification permissions from the user
    UNUserNotificationCenter.Current.RequestAuthorization (UNAuthorizationOptions.Alert, (approved, err) => {
        // Handle approval
    });

    // Watch for notifications while the app is active
    UNUserNotificationCenter.Current.Delegate = new UserNotificationCenterDelegate ();

    return true;
}
```

Mit diesem Code wird die benutzerdefinierte Anfügen `UNUserNotificationCenterDelegate` oben auf den aktuellen `UNUserNotificationCenter` , damit die Anwendung Benachrichtigung verarbeiten kann, während er aktiv ist und im Vordergrund.

## <a name="notification-management"></a>Benachrichtigung Management

Neue iOS 10, Benachrichtigung Management bietet Zugriff auf ausstehend und übermittelten Benachrichtigungen und fügt die Möglichkeit, entfernen, aktualisieren oder höher stufen diese Benachrichtigungen.

Ist ein wichtiger Bestandteil der Benachrichtigung Management der _Anforderungsbezeichner_ , wurde auf die Benachrichtigung zugewiesen, wenn es erstellt und mit dem System geplant wurde. Für Remote-Benachrichtigungen, hat dies über das neue `apps-collapse-id` -Feld in der HTTP-Anforderungsheader.

Den Anforderungsbezeichner wird verwendet, um die Benachrichtigung auszuwählen, die möchte, dass die app-Benachrichtigung auf auszuführen.

### <a name="removing-notifications"></a>Entfernen von Benachrichtigungen

Um eine ausstehende Benachrichtigung aus dem System zu entfernen, verwenden Sie den folgenden Code:

```csharp
var requests = new string [] { "sampleRequest" };
UNUserNotificationCenter.Current.RemovePendingNotificationRequests (requests);
```

Um eine bereits gelieferten Benachrichtigung zu entfernen, verwenden Sie den folgenden Code ein:

```csharp
var requests = new string [] { "sampleRequest" };
UNUserNotificationCenter.Current.RemoveDeliveredNotifications (requests);
```

### <a name="updating-an-existing-notification"></a>Aktualisieren eine vorhandene Benachrichtigung

Um eine vorhandene Benachrichtigung zu aktualisieren, erstellen Sie eine neue Benachrichtigung mit den gewünschten Parametern geändert (z. B. eine neue triggerzeit) und fügen Sie es in das System mit dem gleichen Bezeichner der Anforderung als die Benachrichtigung, die geändert werden muss. Beispiel:


```csharp
using UserNotifications;
...

// Rebuild notification
var content = new UNMutableNotificationContent ();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;

// New trigger time
var trigger = UNTimeIntervalNotificationTrigger.CreateTrigger (10, false);

// ID of Notification to be updated
var requestID = "sampleRequest";
var request = UNNotificationRequest.FromIdentifier (requestID, content, trigger);

// Add to system to modify existing Notification
UNUserNotificationCenter.Current.AddNotificationRequest (request, (err) => {
    if (err != null) {
        // Do something with error...
    }
});
```

Für bereits übermittelten Benachrichtigungen wird die vorhandene Benachrichtigung aktualisiert und an den Anfang der Liste auf der Startseite "und" Lock-Bildschirme und die mitteilungszentrale höher gestuft, wenn er vom Benutzer bereits gelesen wurde abgerufen.

## <a name="working-with-notification-actions"></a>Arbeiten mit Benachrichtigungsaktionen

In iOS-10 Benachrichtigungen, die für den Benutzer übermittelt werden sind nicht statisch, und geben Sie mehrere Möglichkeiten, die der Benutzer interagieren kann mit ihnen (über die integrierten für benutzerdefinierte Aktionen).

Es gibt drei Arten von Aktionen, die eine iOS-app auf reagieren kann:

- **Standardaktion** -Dies ist beim Tippen auf einer Benachrichtigungs an die app zu öffnen und die Details der angegebenen Benachrichtigung angezeigt.
- **Benutzerdefinierte Aktionen** -diesen in iOS 8 hinzugefügt wurden, und bieten eine schnelle Möglichkeit für den Benutzer, einen benutzerdefinierten Task direkt in die Benachrichtigung auszuführen, ohne die app zu starten. Sie können dargestellt werden, als eine Liste von Schaltflächen mit anpassbare Titel oder ein Texteingabefeld die ausgeführt werden kann, entweder im Hintergrund (, in dem die app eine kleine Menge Zeit zur Erfüllung der Anforderung angegeben ist) oder den Vordergrund (wobei die app in den Vordergrund Fu gestartet wird Lfill der Anforderung). Benutzerdefinierte Aktionen sind auf IOS- und WatchOS verfügbar.
- **Schließen Sie die Aktion** -dieser Aktion wird für die app gesendet, wenn der Benutzer eine bestimmte Benachrichtigung schließt.

### <a name="creating-custom-actions"></a>Erstellen von benutzerdefinierten Aktionen

Verwenden Sie zum Erstellen und registrieren eine benutzerdefinierte Aktion mit dem System, den folgenden Code ein:

```csharp
// Create action
var actionID = "reply";
var title = "Reply";
var action = UNNotificationAction.FromIdentifier (actionID, title, UNNotificationActionOptions.None);

// Create category
var categoryID = "message";
var actions = new UNNotificationAction [] { action };
var intentIDs = new string [] { };
var categoryOptions = new UNNotificationCategoryOptions [] { };
var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.None);
    
// Register category
var categories = new UNNotificationCategory [] { category };
UNUserNotificationCenter.Current.SetNotificationCategories (new NSSet<UNNotificationCategory>(categories)); 
```

Beim Erstellen eines neuen `UNNotificationAction`, hat eine eindeutige ID und den Titel, der auf die Schaltfläche angezeigt wird. Standardmäßig wird die Aktion als Aktion im Hintergrund, erstellt jedoch Optionen angegeben werden können, um das Verhalten der Aktion (z. B. das Festlegen auf eine Aktion Vordergrund sein) anzupassen.

Jede der Aktionen erstellt eine Kategorie zugeordnet werden müssen. Beim Erstellen eines neuen `UNNotificationCategory`, eine eindeutige ID zugewiesen wird, wird eine Liste von Aktionen sie ausführen kann eine Liste mit Absicht-IDs, um weitere Informationen zu den Zweck der Aktionen in der Kategorie und einige Optionen zum Steuern des Verhaltens der Kategorie zu ermöglichen.

Schließlich werden alle Kategorien mit dem System registriert die `SetNotificationCategories` Methode.

### <a name="presenting-custom-actions"></a>Benutzerdefinierte Aktionen darstellen

Sobald eine Reihe von benutzerdefinierten Aktionen und Kategorien erstellt und mit dem System registriert wurden, können sie einer lokalen oder Remote-Benachrichtigungen dargestellt werden.

Legen Sie für die Remote-Benachrichtigung einen `category` in der Remote-Benachrichtigung-Nutzlast, die eine der oben erstellten Kategorien entspricht. Zum Beispiel:

```csharp
{
    aps:{
        alert:"Hello world!",
        category:"message"
    }
}
```

Für lokale Benachrichtigungen Festlegen der `CategoryIdentifier` Eigenschaft von der `UNMutableNotificationContent` Objekt. Zum Beispiel:

```csharp
var content = new UNMutableNotificationContent ();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;
content.CategoryIdentifier = "message";
...
```

Diese ID muss erneut entsprechen, eine der Kategorien, die oben erstellt wurde.

### <a name="handling-dismiss-actions"></a>Behandlung verworfen werden, Aktionen

Wie oben erwähnt, kann eine Aktion verworfen werden, für die app gesendet werden, wenn der Benutzer eine Benachrichtigung schließt. Da dies nicht über eine Standardaktion ist, müssen eine Option festgelegt werden, wenn die Kategorie erstellt wird. Zum Beispiel:

```csharp
var categoryID = "message";
var actions = new UNNotificationAction [] { action };
var intentIDs = new string [] { };
var categoryOptions = new UNNotificationCategoryOptions [] { };
var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.CustomDismissAction);

```

### <a name="handling-action-responses"></a>Behandeln von Antworten Aktion

Wenn der Benutzer mit dem benutzerdefinierte Aktionen und die Kategorien, die interagiert oben erstellt wurden, muss die app die angeforderte Aufgabe zu erfüllen. Dies erfolgt durch die Bereitstellung einer `UNUserNotificationCenterDelegate` und Implementieren der `UserNotificationCenter` Methode. Zum Beispiel:

```csharp
using System;
using UserNotifications;

namespace MonkeyNotification
{
    public class UserNotificationCenterDelegate : UNUserNotificationCenterDelegate
    {
        ...

        #region Override Methods
        public override void DidReceiveNotificationResponse (UNUserNotificationCenter center, UNNotificationResponse response, Action completionHandler)
        {
            // Take action based on Action ID
            switch (response.ActionIdentifier) {
            case "reply":
                // Do something
                break;
            default:
                // Take action based on identifier
                if (response.IsDefaultAction) {
                    // Handle default action...
                } else if (response.IsDismissAction) {
                    // Handle dismiss action
                }
                break;
            }

            // Inform caller it has been handled
            completionHandler();
        }
        #endregion
    }
}
```

Die übergebene in `UNNotificationResponse` -Klasse verfügt über eine `ActionIdentifier` -Eigenschaft, die entweder kann sein, die Default-Aktion oder die Aktion zu schließen. Verwendung `response.Notification.Request.Identifier` für benutzerdefinierten Aktionen zu testen.

Die `UserText` Eigenschaft enthält den Wert der Benutzertext eingeben. Die `Notification` Eigenschaft enthält die Benachrichtigung Inhalt und die ursprünglichen Benachrichtigung, die die Anforderung mit dem Trigger enthält. Die app kann entscheiden, ob es handelte es sich um eine lokale oder Remote-Benachrichtigung auf den Typ des Triggers basierend.

## <a name="working-with-service-extensions"></a>Arbeiten mit Service-Erweiterungen

Bei der Arbeit mit Remote Benachrichtigungen _Webdiensterweiterungen_ bieten eine Möglichkeit, innerhalb der Benachrichtigungsnutzlast End-to-End-Verschlüsselung zu aktivieren. Dienst-Erweiterungen sind eine nicht - Benutzeroberfläche-Erweiterung (verfügbar im iOS 10), ausgeführt wird, im Hintergrund mit der Hauptzweck erweitern oder ersetzen den sichtbaren Inhalt einer Benachrichtigung ein, bevor es dem Benutzer angezeigt wird. 

[![](enhanced-user-notifications-images/extension01.png "Dienst-Erweiterung (Übersicht)")](enhanced-user-notifications-images/extension01.png#lightbox)

Webdiensterweiterungen schneller ausgeführt werden sollen und nur eine kurze Zeitspanne auszuführende vom System erhalten. Wenn die Webdiensterweiterung nicht seine Aufgabe in der vorgesehenen Zeit abschließen, wird eine alternative Methode aufgerufen werden. Wenn das Fallback fehlschlägt, wird die ursprüngliche Benachrichtigung Inhalte für den Benutzer angezeigt.

Einige möglichen Verwendungen von Service-Erweiterungen gehören:

- Bereitstellen von End-to-End-Verschlüsselung des Inhalts Remote-Benachrichtigung.
- Hinzufügen von Anlagen zu Remote Benachrichtigungen an diese ergänzen.

### <a name="implementing-a-service-extension"></a>Implementieren eine Erweiterung

Führen Sie folgende Schritte aus, um eine Erweiterung der in einem Xamarin.iOS-app zu implementieren:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Öffnen Sie die app-Projektmappe in Visual Studio für Mac.
2. Mit der rechten Maustaste auf den Namen der Projektmappe in der **Lösung Pad** , und wählen Sie **hinzufügen** > **neues Projekt hinzufügen**.
3. Wählen Sie **iOS** > **Erweiterungen** > **Notification Service-Erweiterungen** , und klicken Sie auf die **Weiter** Schaltfläche: 

    [![](enhanced-user-notifications-images/extension02.png "Wählen Sie Notification Service-Erweiterungen")](enhanced-user-notifications-images/extension02.png#lightbox)
4. Geben Sie einen **Namen** für die Erweiterung, und klicken Sie auf die **Weiter** Schaltfläche: 

    [![](enhanced-user-notifications-images/extension03.png "Geben Sie einen Namen für die Erweiterung")](enhanced-user-notifications-images/extension03.png#lightbox)
5. Anpassen der **Projektname** und/oder **Projektmappenname** Wenn erforderlich, und klicken Sie auf die **erstellen** Schaltfläche: 

    [![](enhanced-user-notifications-images/extension04.png "Passen Sie die Projektnamen und/oder die Namen der Projektmappe")](enhanced-user-notifications-images/extension04.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Öffnen Sie die app-Projektmappe in Visual Studio.
2. Mit der rechten Maustaste auf den Namen der Projektmappe in der **Projektmappen-Explorer** , und wählen Sie **hinzufügen** > **neues Projekt hinzufügen**.
3. Wählen Sie **iOS** > **Erweiterungen** > **Notification Service-Erweiterungen**: 

    [![](enhanced-user-notifications-images/extension01w.png "Wählen Sie Notification Service-Erweiterungen")](enhanced-user-notifications-images/extension01w.png#lightbox)
4. Geben Sie einen **Namen** für die Erweiterung, und klicken Sie auf die **OK** Schaltfläche.

-----

> [!IMPORTANT]
> Die Paket-ID für die Webdiensterweiterung übereinstimmen, die Paket-ID der Haupt-app mit `.appnameserviceextension` am Ende angefügt. Beispielsweise wäre die Haupt-app eine Paket-ID des `com.xamarin.monkeynotify`, die Webdiensterweiterung müsste eine Paket-ID des `com.xamarin.monkeynotify.monkeynotifyserviceextension`. Dies sollte automatisch festgelegt werden, wenn die Erweiterung der Projektmappe hinzugefügt wird. 

Es ist eine Hauptklasse in Notification Service-Erweiterung, die geändert werden, um die erforderliche Funktionalität bereitstellen müssen. Zum Beispiel:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;

namespace MonkeyChatServiceExtension
{
    [Register ("NotificationService")]
    public class NotificationService : UNNotificationServiceExtension
    {
        #region Computed Properties
        public Action<UNNotificationContent> ContentHandler { get; set; }
        public UNMutableNotificationContent BestAttemptContent { get; set; }
        #endregion

        #region Constructors
        protected NotificationService (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void DidReceiveNotificationRequest (UNNotificationRequest request, Action<UNNotificationContent> contentHandler)
        {
            ContentHandler = contentHandler;
            BestAttemptContent = (UNMutableNotificationContent)request.Content.MutableCopy ();

            // Modify the notification content here...
            BestAttemptContent.Title = $"{BestAttemptContent.Title}[modified]";

            ContentHandler (BestAttemptContent);
        }

        public override void TimeWillExpire ()
        {
            // Called just before the extension will be terminated by the system.
            // Use this as an opportunity to deliver your "best attempt" at modified content, otherwise the original push payload will be used.

            ContentHandler (BestAttemptContent);
        }
        #endregion
    }
}
```

Die erste Methode `DidReceiveNotificationRequest`, wird der Bezeichner für die Benachrichtigung sowie die Benachrichtigung über Inhalte übergeben der `request` Objekt. Die übergebene in `contentHandler` müssen aufgerufen werden, um die Benachrichtigung für den Benutzer anzuzeigen.

Die zweite Methode `TimeWillExpire`, wird direkt vor dem Zeitpunkt vor der Ausführung für die Dienst-Erweiterung zum Verarbeiten der Anforderung aufgerufen werden. Wenn die Erweiterung des Diensts aufrufen, um kann die `contentHandler` in der vorgesehenen Zeit, wird der ursprüngliche Inhalt für den Benutzer angezeigt werden.

### <a name="triggering-a-service-extension"></a>Eine Erweiterung der auslösen

Mit einer Dienst-Erweiterung erstellt und mit der app übermittelt kann es durch Ändern der Remote Benachrichtigungsnutzlast an das Gerät gesendet ausgelöst werden. Zum Beispiel:

```csharp
{
    aps : {
        alert : "New Message Available",
        mutable-content: 1
    },
    encrypted-content : "#theencryptedcontent"
}
```

Die neue `mutable-content` Schlüssel gibt an, dass die Webdiensterweiterung werden, um den Inhalt der Remote-Benachrichtigung zu aktualisieren gestartet müssen. Die `encrypted-content` Schlüssel enthält, das die verschlüsselten Daten, die die Webdiensterweiterung entschlüsselt werden kann, bevor Sie dem Benutzer.

Betrachten Sie das folgende Beispiel-Erweiterung:

```csharp
using UserNotification;

namespace myApp {
    public class NotificationService : UNNotificationServiceExtension {
    
        public override void DidReceiveNotificationRequest(UNNotificationRequest request, contentHandler) {
            // Decrypt payload
            var decryptedBody = Decrypt(Request.Content.UserInfo["encrypted-content"]);
            
            // Modify Notification body
            var newContent = new UNMutableNotificationContent();
            newContent.Body = decryptedBody;
            
            // Present to user
            contentHandler(newContent);
        }
        
        public override void TimeWillExpire() {
            // Handle out-of-time fallback event
            ...
        }
        
    }
}
```

Dieser Code entschlüsselt den verschlüsselten Inhalt aus der `encrypted-content` Schlüssel, erstellt ein neues `UNMutableNotificationContent`, legt der `Body` -Eigenschaft auf den entschlüsselten Inhalt und verwendet die `contentHandler` auf die Benachrichtigung für den Benutzer anzuzeigen.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden alle Arten behandelt, dass die Benachrichtigung der Benutzer von iOS 10 erweitert wurden. Sie präsentiert des neuen Benutzers benachrichtigungsframeworks und wie Sie es in einem Xamarin.iOS-app oder die App-Erweiterung verwenden.



## <a name="related-links"></a>Verwandte Links

- [iOS-10-Beispiele](https://developer.xamarin.com/samples/ios/iOS10/)
- [UserNotifications Frameworkverweis](https://developer.apple.com/reference/usernotifications)
- [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui)
- [Programmierhandbuch für lokale und Remote-Benachrichtigung](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
