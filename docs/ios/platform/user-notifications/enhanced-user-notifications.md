---
title: Verbesserte Benutzerbenachrichtigungen in Xamarin.iOS
description: Dieser Artikel beschreibt das Benutzerbenachrichtigungen-Framework, die unter iOS 10 eingeführt. Es wird erläutert, lokale Benachrichtigungen, remotebenachrichtigungen, Verwaltung von Notification, Benachrichtigung-Aktionen und mehr.
ms.prod: xamarin
ms.assetid: 4E1FF652-28F0-4566-B383-9D12664401A4
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/02/2017
ms.openlocfilehash: d1b1a59b432315532844f8fca3b613ff3392a7b5
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108241"
---
# <a name="enhanced-user-notifications-in-xamarinios"></a>Verbesserte Benutzerbenachrichtigungen in Xamarin.iOS

Noch nicht mit iOS 10, ermöglicht das Framework für die Bereitstellung und Verarbeitung von lokalen und remote-Benachrichtigungen, erhalten. Durch die Verwendung dieses Frameworks kann eine app oder App-Erweiterung die Übermittlung von lokalen Benachrichtigungen planen eine Reihe von Bedingungen wie z. B. Speicherort oder Uhrzeit angeben.

## <a name="about-user-notifications"></a>Informationen zu Benutzerbenachrichtigungen

Wie bereits erwähnt, können das neue Benutzerbenachrichtigung-Framework für die Bereitstellung und Verarbeitung von lokalen und remote-Benachrichtigungen. Durch die Verwendung dieses Frameworks kann eine app oder App-Erweiterung die Übermittlung von lokalen Benachrichtigungen planen eine Reihe von Bedingungen wie z. B. Speicherort oder Uhrzeit angeben.

Darüber hinaus die app oder die Erweiterung kann empfangen (und potenziell ändern) sowohl lokale als auch Benachrichtigungen, wie sie iOS-Gerät des Benutzers übermittelt werden.

Das neue Benachrichtigung User Interface, UI-Framework ermöglicht es, eine app oder App-Erweiterung, die Darstellung von lokalen und Remotecomputern Benachrichtigungen anpassen, wenn sie dem Benutzer angezeigt werden.

Dieses Framework bietet es sich um die folgenden Möglichkeiten, die eine app Benachrichtigungen für einen Benutzer bereitstellen kann:

- **Visual Warnungen** –, in dem die Benachrichtigung vom oberen Rand des Bildschirms als Banner hinunter.
- **Sound- und Vibrationen** -kann eine Benachrichtigung zugeordnet werden.
- **App-Symbol Badging** -, in dem das Symbol der app zeigt eines Badges, die anzeigt, dass neuer Inhalte verfügbar, z. B. die Anzahl der ungelesene e-Mail-Nachrichten ist.

Darüber hinaus je nach den aktuellen Kontext des Benutzers gibt es verschiedene Möglichkeiten, die eine Benachrichtigung angezeigt werden:

- Wenn das Gerät entsperrt wird, wird die Benachrichtigung vom oberen Rand des Bildschirms als Banner Abonnentenoptionen.
- Wenn das Gerät gesperrt ist, wird die Benachrichtigung auf dem Sperrbildschirm des Benutzers angezeigt werden.
- Wenn der Benutzer eine Benachrichtigung verpasst hat, können sie die Mitteilungszentrale zu öffnen und keine verfügbar ist, warten Benachrichtigungen anzuzeigen.

Eine Xamarin.iOS-app verfügt über zwei Arten von Benachrichtigungen für Benutzer, die sie senden kann:

- **Lokale Benachrichtigungen** -diese gesendet werden, indem apps, die lokal auf dem Gerät des Benutzers installiert.
- **Remotebenachrichtigungen** -werden von einem Remotecomputer gesendet, Server und entweder dem Benutzer angezeigt, oder sie ein Update der Hintergrund der der Inhalt der app auslösen.

### <a name="about-local-notifications"></a>Informationen zu lokalen Benachrichtigungen

Die lokale Benachrichtigungen, die eine iOS-app senden können haben die folgenden Features und die folgenden Attribute:

- Sie werden von apps gesendet, die lokal auf dem Gerät des Benutzers sind. 
- Sie sind kann konfiguriert werden, verwenden Sie entweder "Uhrzeit" oder "Speicherort entsprechend Trigger. 
- Die app plant die Benachrichtigung mit dem Gerät des Benutzers ein, und es wird angezeigt, wenn die auslöserbedingung erfüllt ist.
- Wenn der Benutzer mit der Benachrichtigung interagiert, wird die app einen Rückruf erhalten.

Einige Beispiele für lokale Benachrichtigungen sind:

- Kalender-Warnungen
- Erinnerung-Warnungen
- Beachten Sie Standort-Trigger

Weitere Informationen finden Sie unter Apple [lokale und Remote Notification Programming Guide](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/) Dokumentation.

### <a name="about-remote-notifications"></a>Informationen zu Remote-Benachrichtigungen

Die Remote-Benachrichtigungen, die eine iOS-app senden können haben die folgenden Features und die folgenden Attribute:

- Die app verfügt über eine serverseitige Komponente, der mit dem er kommuniziert.
- Das Apple Push Notification Service (APNs) wird verwendet, einen Best-Effort-Prinzip-Übermittlung von Remotebenachrichtigungen auf dem Gerät des Benutzers von des Entwicklers cloudbasierten Servern zu übertragen.
- Wenn die app die Remotebenachrichtigung erhält wird es dem Benutzer angezeigt.
- Wenn der Benutzer mit der Benachrichtigung interagiert, wird die app einen Rückruf erhalten.

Einige Beispiele von Remotebenachrichtigungen:

- News-Warnungen
- Sport-Updates
- Instant Messaging-Nachrichten

Es gibt zwei Arten von Remotebenachrichtigungen eine iOS-app zur Verfügung:

- **Benutzer** – der Benutzer auf dem Gerät angezeigt werden.
- **Automatische Updates** – diese bieten einen Mechanismus, um den Inhalt einer iOS-app im Hintergrund aktualisieren. Wenn eine automatische Aktualisierung empfangen wird, kann die app das Pull-Server entfernen Sie die neuesten Inhalte wenden Sie sich.

Weitere Informationen finden Sie unter Apple [lokale und Remote Notification Programming Guide](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/) Dokumentation.

### <a name="about-the-existing-notifications-api"></a>Über die vorhandenen API-Benachrichtigungen

Bevor Sie iOS 10, eine iOS-app verwenden `UIApplication` eine Benachrichtigung mit dem System zu registrieren und planen, wie diese Benachrichtigung (entweder nach Zeit oder Speicherort) ausgelöst werden soll.

Es gibt mehrere Problem, das ein Entwickler bei der Arbeit mit der vorhandenen API-Benachrichtigung auftreten kann:

- Gab es verschiedene Rückrufe erforderlich für lokales oder Remote Notifications, was zu einer Duplizierung von Code führen konnte.
- Die app musste Kontrolle über die Benachrichtigung beschränkt, nachdem es mit dem System geplant wurde.
- Gab es unterschiedliche Ebenen der Unterstützung für alle vorhandenen Apple-Plattformen.

### <a name="about-the-new-user-notification-framework"></a>Über das neue Benutzer-Notification-Framework

Mit iOS 10, hat Apple eingeführt, das neue Benutzerbenachrichtigung-Framework, ersetzt die vorhandene `UIApplication` Methode wie oben beschrieben.

Die Benutzerbenachrichtigung-Framework bietet Folgendes:

- Eine vertraute API, die vollständig gleichen Funktionsumfang wie die vorherigen Methoden, sodass sie ganz leicht Code Portieren aus dem vorhandenen Framework enthält.
- Enthält eine erweiterte Reihe von Optionen für Inhalte, die umfangreichere Benachrichtigungen an Benutzer gesendet werden kann.
- Sowohl lokale als auch Remote Notifications können von den gleichen Code und Rückrufe behandelt werden.
- Vereinfacht das Verarbeiten der Rückrufe, die in einer app gesendet werden, wenn der Benutzer mit der Benachrichtigung interagiert.
- Verbesserte Verwaltung der ausstehenden und übermittelten Benachrichtigungen, einschließlich der Möglichkeit, entfernen oder Aktualisieren von Benachrichtigungen.
- Fügt die Möglichkeit zur Präsentation von in-app-Benachrichtigungen an.
- Bietet die Möglichkeit zum Planen und Verarbeiten von Benachrichtigungen von in-App-Erweiterungen.
- Fügt neue Erweiterungspunkt für Benachrichtigungen hinzu. 

Das neue Framework für die Benachrichtigung für Benutzer bietet eine einheitliche Benachrichtigung API für die mehrere Plattformen unterstützt, einschließlich der Apple: 

- **iOS** -vollständige Unterstützung zum Verwalten und Planen Sie Benachrichtigungen.
- **TvOS** -Badge-app-Symbole für lokale und remote-Benachrichtigungen die Möglichkeit hinzugefügt.
- **WatchOS** : Fügt die Fähigkeit zum Weiterleiten von Benachrichtigungen vom gekoppelten iOS-Gerät des Benutzers auf ihrer Apple Watch und bietet Watch-apps die Möglichkeit, lokale Benachrichtigungen direkt auf der Watch selbst vorzunehmen.

Weitere Informationen finden Sie unter Apple ["usernotifications" Frameworkverweis](https://developer.apple.com/reference/usernotifications) und [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui) Dokumentation.

## <a name="preparing-for-notification-delivery"></a>Vorbereiten für die Übermittlung der Benachrichtigung

Bevor eine iOS app Benachrichtigungen senden kann des Benutzers, die, den die app mit dem System registriert sein muss, und da eine Benachrichtigung eine Unterbrechung der Benutzer ist, muss eine Anwendung explizit Berechtigungen vor dem Senden anfordern.

Es gibt drei verschiedene Stufen der fordert der Benachrichtigung, die der Benutzer für eine app genehmigt werden kann:

- Banner angezeigt.
- Sound Warnungen.
- Signalisierung des Anwendungssymbols.

Darüber hinaus müssen diese Ebenen für die Genehmigung angefordert und für lokale und remote-Benachrichtigungen festgelegt werden.

Notification-Berechtigung darf nur angefordert werden, sobald die app gestartet wird, indem Sie den folgenden Code Hinzufügen der `FinishedLaunching` Methode der `AppDelegate` und festlegen den gewünschten Benachrichtigungstyp (`UNAuthorizationOptions`):

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

Darüber hinaus kann Benutzer immer die Benachrichtigung über Berechtigungen für eine app jederzeit ändern der **Einstellungen** app auf dem Gerät. Die app sollte für die Berechtigungen des Benutzers angeforderte Benachrichtigung überprüfen, bevor Sie die Präsentation einer benachrichtigungs mit dem folgenden Code:

```csharp
// Get current notification settings
UNUserNotificationCenter.Current.GetNotificationSettings ((settings) => {
    var alertsAllowed = (settings.AlertSetting == UNNotificationSetting.Enabled);
}); 
``` 

### <a name="configuring-the-remote-notifications-environment"></a>Konfigurieren der Umgebung Remotebenachrichtigungen

Neue IOS 10, der Entwickler muss das Betriebssystem an, welche Umgebung Pushbenachrichtigung ausgeführt werden, entweder als Entwicklung oder Produktion, informieren. Diese Informationen nicht bereitstellen kann in der app abgelehnt wird, bei der Übermittlung an den iTunes App Store eine Benachrichtigung ähnlich der folgenden führen:

> Fehlende Push Notification-Berechtigung - Ihrer-app enthält eine API für den Apple Push Notification Service, aber die `aps-environment` Berechtigung ist nicht vorhanden, aus der app-Signatur.

Um die erforderliche Berechtigung bereitzustellen, führen Sie folgende Schritte aus:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Doppelklicken Sie auf die `Entitlements.plist` Datei die **Lösungspad** um ihn zur Bearbeitung zu öffnen.
2. Wechseln Sie zu der **Quelle** anzeigen: 

    [![](enhanced-user-notifications-images/setup01.png "Die Datenquellensicht")](enhanced-user-notifications-images/setup01.png#lightbox)
3. Klicken Sie auf die **+** , um einen neuen Schlüssel hinzuzufügen.
4. Geben Sie `aps-environment` für die **Eigenschaft**, lassen Sie die **Typ** als `String` , und geben Sie entweder `development` oder `production` für die **Wert**: 

    [![](enhanced-user-notifications-images/setup02.png "Der Aps-Environment-Eigenschaft")](enhanced-user-notifications-images/setup02.png#lightbox)
5. Speichern Sie die Änderungen in der Datei.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Doppelklicken Sie auf die `Entitlements.plist` Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen.
3. Klicken Sie auf die **+** , um einen neuen Schlüssel hinzuzufügen.
4. Geben Sie `aps-environment` für die **Eigenschaft**, lassen Sie die **Typ** als `String` , und geben Sie entweder `development` oder `production` für die **Wert**: 

    [![](enhanced-user-notifications-images/setup02w.png "Der Aps-Environment-Eigenschaft")](enhanced-user-notifications-images/setup02.png#lightbox)
5. Speichern Sie die Änderungen in der Datei.

-----

### <a name="registering-for-remote-notifications"></a>Registrieren von Remotebenachrichtigungen

Wenn die app wird senden und Empfangen von Remotebenachrichtigungen, sie wird immer noch ausführen muss _Token Registrierung_ mithilfe des vorhandenen `UIApplication` API. Diese Registrierung muss das Gerät eine live-Verbindung Netzwerkzugriff APNs, haben die erforderlichen Token generiert wird, das für die app gesendet werden. Die app muss dieses Token zum des Entwicklers serverseitige app zum Registrieren von remotebenachrichtigungen weiterzuleiten:

[![](enhanced-user-notifications-images/token01.png "Token-Geräteregistrierung – Übersicht")](enhanced-user-notifications-images/token01.png#lightbox)

Verwenden Sie den folgenden Code zum Initialisieren der erforderlichen Registrierungs:

```csharp
UIApplication.SharedApplication.RegisterForRemoteNotifications ();
```

Das Token, das an der Entwickler die serverseitige app gesendet werden müssen eingeschlossen werden, da Sie Teil der Nutzlast der Benachrichtigung, Get vom Server an APNs gesendet, wenn Sie eine Remote-Benachrichtigung senden:

[![](enhanced-user-notifications-images/token02.png "Das Token, die als Teil der Nutzlast der Benachrichtigung enthalten")](enhanced-user-notifications-images/token02.png#lightbox)

Das Token dient als Schlüssel, der miteinander verknüpft werden, die Benachrichtigung und die app zum Öffnen oder reagiert auf die Benachrichtigung verwendet.

Weitere Informationen finden Sie unter Apple [lokale und Remote Notification Programming Guide](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/) Dokumentation.

## <a name="notification-delivery"></a>Übermittlung der Benachrichtigung

Vollständig mit der app registriert und die erforderlichen Berechtigungen, die vom angefordert und erteilt durch den Benutzer die app ist jetzt bereit zum Senden und Empfangen von Benachrichtigungen. 

### <a name="providing-notification-content"></a>Bereitstellen von Benachrichtigungsinhalt

Neue alle Benachrichtigungen für iOS 10, enthalten beide eine **Titel** und **Untertitel** , immer angezeigt wird mit der **Text** der Inhalt der Benachrichtigung. Ebenfalls neu, ist die Möglichkeit zum Hinzufügen **Medienanhänge** auf den Inhalt der Benachrichtigung.

Um den Inhalt der eine lokale Benachrichtigung zu erstellen, verwenden Sie den folgenden Code:

```csharp
var content = new UNMutableNotificationContent();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;
```

Von Remotebenachrichtigungen ist der Prozess ähnlich:

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

### <a name="scheduling-when-a-notification-is-sent"></a>Planen bei der eine Benachrichtigung wird gesendet.

Mit dem Inhalt der Benachrichtigung erstellt, die app planen, wann die Benachrichtigung durch Festlegen, der dem Benutzer präsentiert wird muss ein *Trigger*. iOS 10 bietet vier verschiedene Typen von Trigger:

- **Pushbenachrichtigung** : wird ausschließlich mit Remotebenachrichtigungen verwendet und wird ausgelöst, wenn es sich bei APNs, die eine Benachrichtigung gesendet, die auf dem Gerät ausgeführten app verpacken.
- **Zeitintervall** -können Sie eine lokale Benachrichtigung von einem Zeitpunkt geplant werden, das Intervall mit beginnen jetzt und einen einen zukünftigen Punkt endet. Beispiel: `var trigger =  UNTimeIntervalNotificationTrigger.CreateTrigger (5, false);`
- **Calendar Date** -lokale Benachrichtigungen für ein bestimmtes Datum und Uhrzeit geplant werden können.
- **Speicherort basierend** -lokale Benachrichtigungen geplant werden, wenn das iOS-Gerät eingeben oder verlassen einen bestimmten geografischen Standort oder in einem angegebenen Abstand zur alle Bluetooth-Beacons können.

Wenn eine lokale Benachrichtigung bereit ist, muss die app zum Aufrufen der `Add` Methode der `UNUserNotificationCenter` Objekt, das die Anzeige für den Benutzer zu planen. Von Remotebenachrichtigungen sendet die serverseitige app einer Nutzlast der Benachrichtigung an APNs-Zertifikate, die dann das Paket auf dem Gerät des Benutzers.

Bringen alle Bestandteile zusammen, kann ein Beispiel für lokale Benachrichtigung formuliert werden:

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

## <a name="handling-foreground-app-notifications"></a>Behandeln von Benachrichtigungen der Vordergrund-App

Neue auf iOS 10, eine app kann behandeln Benachrichtigungen anders, wenn es im Vordergrund und eine Benachrichtigung wird ausgelöst. Durch die Bereitstellung einer `UNUserNotificationCenterDelegate` und Implementieren der `UserNotificationCenter` -Methode, die app kann verwendet werden, die Verantwortung für die Anzeige der Benachrichtigungs. Zum Beispiel:

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

Dieser Code ist einfach schreiben, den Inhalt der `UNNotification` Ausgabe der Anwendung und auffordern des Systems, um die standard-Warnung für die Benachrichtigung anzuzeigen. 

Wenn die app beispielsweise die Benachrichtigung selbst angezeigt wird, wenn es im Vordergrund war und nicht die Standardeinstellungen des Systems, übergeben Sie `None` an den Abschlusshandler. Beispiel:

```csharp
completionHandler (UNNotificationPresentationOptions.None);
```

Mit diesem Code werden, öffnen die `AppDelegate.cs` -Datei zur Bearbeitung, und ändern Sie die `FinishedLaunching` Methode, um das Aussehen wie folgt:

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

Dieser Code ist das Anfügen des benutzerdefiniertes `UNUserNotificationCenterDelegate` oben auf den aktuellen `UNUserNotificationCenter` , damit die app eine Benachrichtigung behandeln kann, während es aktiv ist und im Vordergrund.

## <a name="notification-management"></a>Benachrichtigung Management

Neue IOS 10, Benachrichtigung Management ermöglicht den Zugriff auf die ausstehende und empfangene Benachrichtigungen, und die Möglichkeit, zu entfernen, aktualisieren oder diese Benachrichtigungen fördern.

Ein wichtiger Bestandteil der Verwaltung der Benachrichtigung ist die _Anforderungsbezeichner_ , auf die Benachrichtigung zugewiesen wurde, bei der es erstellt und mit dem System geplant wurde. Von Remotebenachrichtigungen, erhält diese über das neue `apps-collapse-id` -Feld im Header HTTP-Anforderung.

Die Request-ID wird verwendet, wählen Sie die Benachrichtigung aus, die möchte, dass die app auf Benachrichtigungen durchzuführen.

### <a name="removing-notifications"></a>Entfernen von Benachrichtigungen

Um eine ausstehende Benachrichtigung aus dem System entfernen, verwenden Sie den folgenden Code:

```csharp
var requests = new string [] { "sampleRequest" };
UNUserNotificationCenter.Current.RemovePendingNotificationRequests (requests);
```

Um einer bereits übermittelten Benachrichtigungen zu entfernen, verwenden Sie den folgenden Code:

```csharp
var requests = new string [] { "sampleRequest" };
UNUserNotificationCenter.Current.RemoveDeliveredNotifications (requests);
```

### <a name="updating-an-existing-notification"></a>Aktualisiert eine vorhandene Benachrichtigung

Um eine vorhandene Benachrichtigung zu aktualisieren, erstellen Sie eine neue Benachrichtigung mit den gewünschten Parametern (z. B. eine neue triggerzeit) geändert und fügen Sie es in das System mit dem gleichen Bezeichner der Anforderung als die Benachrichtigung, die geändert werden muss. Beispiel:


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

Für bereits übermittelten Benachrichtigungen die vorhandene Benachrichtigung aktualisiert und an den Anfang der Liste auf der Startseite und vom Sperrbildschirm-Bildschirmen und in der Mitteilungszentrale höher gestuft, wenn er vom Benutzer bereits gelesen wurde.

## <a name="working-with-notification-actions"></a>Arbeiten mit Benachrichtigungsaktionen

In iOS 10 Benachrichtigungen, die für den Benutzer übermittelt werden sind nicht statisch, und geben Sie mehrere Möglichkeiten, die der Benutzer interagieren kann mit ihnen (über die integrierten zu benutzerdefinierten Aktionen).

Es gibt drei Arten von Aktionen, die eine iOS-app reagieren kann:

- **Standardaktion** – Dies ist, wenn der Benutzer tippt auf eine Benachrichtigung an die app zu öffnen und die Details der angegebenen Benachrichtigung angezeigt.
- **Benutzerdefinierte Aktionen** – diese wurden in iOS 8 hinzugefügt, und geben Sie eine schnelle Möglichkeit für den Benutzer direkt über die Benachrichtigung ein benutzerdefiniertes Tasks ausführen, ohne die app zu starten. Sie können dargestellt werden, entweder als eine Liste der Schaltflächen mit anpassbaren Titeln oder ein Texteingabefeld, die ausgeführt werden können, im Hintergrund (, in dem die app ein wenig Zeit zur Verarbeitung der Anforderung angegeben ist) oder den Vordergrund (, in dem die app im Vordergrund Fu gestartet wird Lfill der Anforderung). Benutzerdefinierte Aktionen sind für iOS und WatchOS verfügbar.
- **Schließen Sie die Aktion** – diese Aktion wird für die app gesendet, wenn der Benutzer eine bestimmte Benachrichtigung schließt.

### <a name="creating-custom-actions"></a>Erstellen von benutzerdefinierten Aktionen

Verwenden Sie zum Erstellen und registrieren eine benutzerdefinierte Aktion, mit dem System, den folgenden Code ein:

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

Beim Erstellen eines neuen `UNNotificationAction`, erhält er eine eindeutige ID und den Titel, der auf die Schaltfläche angezeigt wird. Standardmäßig wird die Aktion als Aktion Hintergrund, erstellt jedoch Optionen angegeben werden können, um das Verhalten der Aktion (z. B. Festlegen einer Vordergrund-Aktion sein) anzupassen.

Jede der Aktionen erstellt eine Kategorie zugeordnet werden müssen. Beim Erstellen eines neuen `UNNotificationCategory`, eine eindeutige ID zugewiesen wird, wird eine Liste von Aktionen er ausführen kann, eine Liste der Intent-IDs, um weitere Informationen zu den Zweck der Aktionen in der Kategorie und einige Optionen zur Steuerung des Verhaltens der Kategorie zu ermöglichen.

Schließlich werden alle Kategorien mit dem System registriert die `SetNotificationCategories` Methode.

### <a name="presenting-custom-actions"></a>Bereitstellen von benutzerdefinierten Aktionen

Nachdem ein Satz von benutzerdefinierten Aktionen und Kategorien erstellt und im System registriert wurden, können sie entweder lokal oder Remote Notifications angezeigt.

Legen Sie für die Remote-Benachrichtigung einen `category` in der Remote-Notification-Nutzlast, die eine der oben erstellten Kategorien entspricht. Zum Beispiel:

```csharp
{
    aps:{
        alert:"Hello world!",
        category:"message"
    }
}
```

Legen Sie für lokale Benachrichtigungen, die `CategoryIdentifier` Eigenschaft der `UNMutableNotificationContent` Objekt. Zum Beispiel:

```csharp
var content = new UNMutableNotificationContent ();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;
content.CategoryIdentifier = "message";
...
```

In diesem Fall muss diese ID entspricht einer der Kategorien, die weiter oben erstellt wurde.

### <a name="handling-dismiss-actions"></a>Behandlung von schließen Aktionen

Wie bereits erwähnt, können eine Schließen-Aktion für die app gesendet werden, wenn der Benutzer eine Benachrichtigung schließt. Da dies keine standard-Aktion ist, müssen eine Option festgelegt werden, wenn die Kategorie erstellt wird. Zum Beispiel:

```csharp
var categoryID = "message";
var actions = new UNNotificationAction [] { action };
var intentIDs = new string [] { };
var categoryOptions = new UNNotificationCategoryOptions [] { };
var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.CustomDismissAction);

```

### <a name="handling-action-responses"></a>Behandeln von Antworten auf Aktionen

Wenn der Benutzer mit dem benutzerdefinierte Aktionen und Kategorien, die interagiert zuvor erstellt haben, muss die app, um die angeforderte Aufgabe zu erfüllen. Dies erfolgt durch die Bereitstellung einer `UNUserNotificationCenterDelegate` und Implementieren der `UserNotificationCenter` Methode. Zum Beispiel:

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

Der übergebene in `UNNotificationResponse` -Klasse verfügt über eine `ActionIdentifier` -Eigenschaft, die entweder kann sein, die Default-Aktion oder die Aktion zu schließen. Verwendung `response.Notification.Request.Identifier` für alle benutzerdefinierten Aktionen zu testen.

Die `UserText` Eigenschaft enthält den Wert der Benutzertext eingeben. Die `Notification` Eigenschaft enthält die ursprüngliche Benachrichtigung, die die Anforderung mit dem Trigger enthält und Benachrichtigungsinhalt. Die app kann entscheiden, wenn es eine lokale war oder Remote-Benachrichtigung auf des Typs des Triggers Grundlage.

> [!NOTE]
> iOS 12 ermöglicht es einer benutzerdefinierten Benachrichtigung UI die Aktionsschaltflächen zur Laufzeit zu ändern. Weitere Informationen sehen Sie sich die [dynamische Benachrichtigung Aktionsschaltflächen](~/ios/platform/introduction-to-ios12/notifications/dynamic-actions.md) Dokumentation.

## <a name="working-with-service-extensions"></a>Arbeiten mit Service-Erweiterungen

Beim Arbeiten mit Remote Notifications _Webdiensterweiterungen_ bieten eine Möglichkeit zum Aktivieren von End-to-End-Verschlüsselung in die Nutzlast der Benachrichtigung. Service-Erweiterungen sind eine nicht - Benutzeroberfläche-Erweiterung (verfügbar in iOS 10), die ausgeführt werden, im Hintergrund mit der Hauptzweck von erweitern oder ersetzen den sichtbaren Inhalt, der eine Benachrichtigung aus, bevor sie dem Benutzer angezeigt werden. 

[![](enhanced-user-notifications-images/extension01.png "Dienst-Erweiterung (Übersicht)")](enhanced-user-notifications-images/extension01.png#lightbox)

Webdiensterweiterungen sollen schnell ausgeführt und erhalten nur einen Moment Zeit, die vom System auszuführen. Falls bei der die Webdiensterweiterung ein Fehler auftritt, seine Aufgabe in der vorgesehenen Zeit abgeschlossen, wird eine alternative Methode aufgerufen werden. Wenn das Fallback fehlschlägt, wird die ursprüngliche Benachrichtigungsinhalt für den Benutzer angezeigt.

Einige möglichen Verwendungen von Service-Erweiterungen gehören:

- Bereitstellen von End-to-End-Verschlüsselung des Inhalts Remotebenachrichtigung.
- Hinzufügen von Anhängen, um Remotebenachrichtigungen sie erweitern.

### <a name="implementing-a-service-extension"></a>Implementieren eine Diensterweiterung

Um eine Dienst-Erweiterung in einer Xamarin.iOS-app zu implementieren, führen Sie folgende Schritte aus:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Öffnen der Projektmappe der app in Visual Studio für Mac.
2. Mit der rechten Maustaste auf den Namen der Projektmappe in die **Lösungspad** , und wählen Sie **hinzufügen** > **neues Projekt hinzufügen**.
3. Wählen Sie **iOS** > **Erweiterungen** > **Notification Service-Erweiterungen** , und klicken Sie auf die **Weiter** Schaltfläche: 

    [![](enhanced-user-notifications-images/extension02.png "Wählen Sie Notification Service-Erweiterungen")](enhanced-user-notifications-images/extension02.png#lightbox)
4. Geben Sie einen **Namen** für die Erweiterung und auf die **Weiter** Schaltfläche: 

    [![](enhanced-user-notifications-images/extension03.png "Geben Sie einen Namen für die Erweiterung")](enhanced-user-notifications-images/extension03.png#lightbox)
5. Anpassen der **Projektname** und/oder **Projektmappenname** Wenn erforderlich, und klicken Sie auf die **erstellen** Schaltfläche: 

    [![](enhanced-user-notifications-images/extension04.png "Passen Sie den Projektnamen und/oder die Namen der Projektmappe")](enhanced-user-notifications-images/extension04.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Öffnen der app-Projektmappe in Visual Studio.
2. Mit der rechten Maustaste auf den Namen der Projektmappe in die **Projektmappen-Explorer** , und wählen Sie **hinzufügen > Neues Projekt...** .
3. Wählen Sie **Visual C# > iOS-Erweiterungen > Erweiterung für Benachrichtigungsdienst**:

    [![](enhanced-user-notifications-images/extension01.w157-sml.png "Wählen Sie Notification Service-Erweiterungen")](enhanced-user-notifications-images/extension01.w157.png#lightbox)
4. Geben Sie einen **Namen** für die Erweiterung und auf die **OK** Schaltfläche.

-----

> [!IMPORTANT]
> Die Bündel-ID für die Service-Erweiterung sollte der Haupt-app mit der Bündel-ID entsprechen `.appnameserviceextension` an das Ende angefügt. Beispielsweise hätte die Haupt-app eine Bündel-ID des `com.xamarin.monkeynotify`, die Service-Erweiterung müssen eine Bündel-ID des `com.xamarin.monkeynotify.monkeynotifyserviceextension`. Dies sollte automatisch festgelegt werden, wenn die Erweiterung der Projektmappe hinzugefügt wird. 

Es ist eine main-Klasse in der Erweiterung für Benachrichtigungsdienst, die geändert werden, um die erforderliche Funktionalität bereitzustellen. Zum Beispiel:

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

Die erste Methode `DidReceiveNotificationRequest`, übergeben den Benachrichtigungsbezeichner als auch dem Benachrichtigungsinhalt über die `request` Objekt. Der übergebene in `contentHandler` aufgerufen werden, um dem Benutzer die Benachrichtigung anzeigen müssen.

Die zweite Methode, `TimeWillExpire`, wird direkt vor dem Zeitpunkt zu erschöpft für die Dienst-Erweiterung zum Verarbeiten der Anforderung aufgerufen werden. Wenn Sie die Webdiensterweiterung aufrufen kann, um die `contentHandler` in der vorgesehenen Zeit, wird der ursprüngliche Inhalt für den Benutzer angezeigt werden.

### <a name="triggering-a-service-extension"></a>Eine Diensterweiterung auslösen

Mit einer Dienst-Erweiterung erstellt und mit der app übermittelt kann es durch Ändern der Remote Nutzlast der Benachrichtigung an das Gerät gesendet ausgelöst werden. Zum Beispiel:

```csharp
{
    aps : {
        alert : "New Message Available",
        mutable-content: 1
    },
    encrypted-content : "#theencryptedcontent"
}
```

Die neue `mutable-content` Schlüssel gibt an, dass die Webdiensterweiterung werden, um den Inhalt der Remote-Benachrichtigung zu aktualisieren gestartet müssen. Die `encrypted-content` Schlüssel enthält, die verschlüsselten Daten, die die Webdiensterweiterung entschlüsselt werden kann, bevor Sie die Präsentation für dem Benutzer.

Sehen Sie sich das folgende Beispiel-Erweiterung:

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

Dieser Code entschlüsselt den verschlüsselten Inhalt aus der `encrypted-content` Schlüssel, erstellt ein neues `UNMutableNotificationContent`, legt diese fest der `Body` Eigenschaft in das entschlüsselter Inhalte und verwendet die `contentHandler` , die Benutzer die Benachrichtigung angezeigt.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden alle Möglichkeiten behandelt, dass die Benachrichtigung der Benutzer von iOS 10 erweitert wurden. Sie präsentiert das neue Framework für die Benachrichtigung für Benutzer und wie Sie es in einer Xamarin.iOS-app oder App-Erweiterung verwenden.



## <a name="related-links"></a>Verwandte Links

- [iOS 10-Beispiele](https://developer.xamarin.com/samples/ios/iOS10/)
- ["Usernotifications"-Framework-Referenz](https://developer.apple.com/reference/usernotifications)
- [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui)
- [Programmierhandbuch für lokale und Remote-Benachrichtigung](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
