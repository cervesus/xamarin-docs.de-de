---
title: Erweiterte Benutzer Benachrichtigungen in xamarin. IOS
description: In diesem Artikel wird das in ios 10 eingeführte Framework für Benutzer Benachrichtigungen beschrieben. Es werden lokale Benachrichtigungen, Remote Benachrichtigungen, Benachrichtigungs Verwaltung, Benachrichtigungs Aktionen und mehr erläutert.
ms.prod: xamarin
ms.assetid: 4E1FF652-28F0-4566-B383-9D12664401A4
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 05/02/2017
ms.openlocfilehash: 0ec63162a21333d0ff831ded1ab17a3d8bb0efaa
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769374"
---
# <a name="enhanced-user-notifications-in-xamarinios"></a>Erweiterte Benutzer Benachrichtigungen in xamarin. IOS

Das Benutzer Benachrichtigungs Framework ist neu bei IOS 10 und ermöglicht die Übermittlung und Handhabung von lokalen Benachrichtigungen und Remote Benachrichtigungen. Mithilfe dieses Frameworks kann eine APP-oder App-Erweiterung die Übermittlung lokaler Benachrichtigungen planen, indem Sie einen Satz von Bedingungen angibt, wie z. b. den Standort oder die Tageszeit.

## <a name="about-user-notifications"></a>Informationen zu Benutzer Benachrichtigungen

Wie bereits erwähnt, ermöglicht das neue Benutzer Benachrichtigungs Framework die Übermittlung und Behandlung von lokalen Benachrichtigungen und Remote Benachrichtigungen. Mithilfe dieses Frameworks kann eine APP-oder App-Erweiterung die Übermittlung lokaler Benachrichtigungen planen, indem Sie einen Satz von Bedingungen angibt, wie z. b. den Standort oder die Tageszeit.

Darüber hinaus kann die APP oder Erweiterung lokale Benachrichtigungen und Remote Benachrichtigungen empfangen (und potenziell ändern), wenn Sie an das IOS-Gerät des Benutzers übermittelt werden.

Mit dem neuen Benutzeroberfläche-Framework für Benutzer Benachrichtigungen kann eine APP-oder App-Erweiterung die Darstellung von lokalen Benachrichtigungen und Remote Benachrichtigungen anpassen, wenn Sie dem Benutzer angezeigt werden.

Dieses Framework bietet die folgenden Möglichkeiten, wie eine APP Benachrichtigungen an einen Benutzer übermitteln kann:

- **Visuelle Warnungen** : Hier wird die Benachrichtigung vom oberen Bildschirmrand aus als Banner angezeigt.
- **Sound und Vibrationen** : können mit einer Benachrichtigung verknüpft werden.
- **App-Symbol Badging** : das Symbol der APP zeigt einen Badge an, der anzeigt, dass neuer Inhalt verfügbar ist, z. b. die Anzahl der ungelesenen e-Mails.

Außerdem gibt es je nach aktuellem Kontext des Benutzers verschiedene Möglichkeiten, wie eine Benachrichtigung angezeigt wird:

- Wenn das Gerät entsperrt ist, wird die Benachrichtigung vom oberen Bildschirmrand aus als Banner angezeigt.
- Wenn das Gerät gesperrt ist, wird die Benachrichtigung auf dem Sperrbildschirm des Benutzers angezeigt.
- Wenn der Benutzer eine Benachrichtigung verpasst hat, kann er das Benachrichtigungs Center öffnen und alle verfügbaren Warnungs Benachrichtigungen dort anzeigen.

Eine xamarin. IOS-App verfügt über zwei Arten von Benutzer Benachrichtigungen, die Sie senden kann:

- **Lokale Benachrichtigungen** : Diese werden von apps gesendet, die lokal auf dem Gerät des Benutzers installiert werden.
- **Remote Benachrichtigungen** : werden von einem Remote Server gesendet und dem Benutzer angezeigt, oder es wird ein Hintergrund Update des App-Inhalts auslöst.

### <a name="about-local-notifications"></a>Informationen zu lokalen Benachrichtigungen

Die lokalen Benachrichtigungen, die eine IOS-App senden kann, haben die folgenden Features und Attribute:

- Sie werden von apps gesendet, die auf dem Gerät des Benutzers lokal sind. 
- Sie können so konfiguriert werden, dass Sie entweder Zeit-oder Location-basierte Trigger verwenden. 
- Die APP plant die Benachrichtigung mit dem Gerät des Benutzers, und Sie wird angezeigt, wenn die Auslöserbedingung erfüllt ist.
- Wenn der Benutzer mit einer Benachrichtigung interagiert, empfängt die APP einen Rückruf.

Beispiele für lokale Benachrichtigungen:

- Kalenderwarnungen
- Erinnerungs Warnungen
- Location-Aware-Trigger

Weitere Informationen finden Sie in der Dokumentation zum [lokalen und Remote Benachrichtigungs Programmier Handbuch](https://developer.apple.com/documentation/usernotifications) von Apple.

### <a name="about-remote-notifications"></a>Informationen zu Remote Benachrichtigungen

Die Remote Benachrichtigungen, die eine IOS-App senden kann, haben folgende Features und Attribute:

- Die APP verfügt über eine serverseitige Komponente, mit der kommuniziert wird.
- Der Apple Push Notification Service (APNs) wird verwendet, um eine bestmögliche Bereitstellung von Remote Benachrichtigungen an das Gerät des Benutzers von den cloudbasierten Servern des Entwicklers zu übertragen.
- Wenn die APP die Remote Benachrichtigung empfängt, wird Sie dem Benutzer angezeigt.
- Wenn der Benutzer mit der Benachrichtigung interagiert, empfängt die APP einen Rückruf.

Einige Beispiele für Remote Benachrichtigungen sind:

- Nachrichten Warnungen
- Sport Updates
- Instant Messaging-Nachrichten

Für eine IOS-App stehen zwei Arten von Remote Benachrichtigungen zur Verfügung:

- **Benutzer mit** Zugriff: Diese werden dem Benutzer auf dem Gerät angezeigt.
- Automatische **Updates** : Diese bieten einen Mechanismus, mit dem der Inhalt einer IOS-App im Hintergrund aktualisiert wird. Wenn ein automatisches Update empfangen wird, kann die APP sich an die Remote Server wenden, um den neuesten Inhalt abzurufen.

Weitere Informationen finden Sie in der Dokumentation zum [lokalen und Remote Benachrichtigungs Programmier Handbuch](https://developer.apple.com/documentation/usernotifications) von Apple.

### <a name="about-the-existing-notifications-api"></a>Informationen zur vorhandenen Benachrichtigungs-API

Vor IOS 10 würde eine IOS-App verwenden `UIApplication` , um eine Benachrichtigung beim System zu registrieren und zu planen, wie diese Benachrichtigung ausgelöst werden soll (entweder nach Zeit oder Speicherort).

Es gibt mehrere Probleme, die ein Entwickler bei der Arbeit mit der vorhandenen Benachrichtigungs-API treffen kann:

- Es wurden verschiedene Rückrufe für lokale oder Remote Benachrichtigungen benötigt, die zu einer Duplizierung von Code führen können.
- Die APP verfügte über eingeschränkte Kontrolle über die Benachrichtigung, nachdem Sie mit dem System geplant wurde.
- Es gab verschiedene Ebenen der Unterstützung für alle vorhandenen Plattformen von Apple.

### <a name="about-the-new-user-notification-framework"></a>Informationen zum neuen Benutzer Benachrichtigungs Framework

Mit IOS 10 hat Apple das neue Benutzer Benachrichtigungs Framework eingeführt, das die oben beschriebene `UIApplication` vorhandene Methode ersetzt.

Das Benutzer Benachrichtigungs Framework bietet Folgendes:

- Eine vertraute API, die Funktions Parität mit den vorherigen Methoden enthält, die das Portieren von Code aus dem vorhandenen Framework erleichtern.
- Enthält einen erweiterten Satz an Inhalts Optionen, mit dem umfangreichere Benachrichtigungen an den Benutzer gesendet werden können.
- Sowohl lokale als auch Remote Benachrichtigungen können durch denselben Code und dieselben Rückrufe behandelt werden.
- Vereinfacht den Prozess der Handhabung von Rückrufen, die an eine APP gesendet werden, wenn der Benutzer mit einer Benachrichtigung interagiert.
- Erweiterte Verwaltung von ausstehenden und übermittelten Benachrichtigungen, einschließlich der Möglichkeit, Benachrichtigungen zu entfernen oder zu aktualisieren.
- Bietet die Möglichkeit, Benachrichtigungen in der APP zu übernehmen.
- Fügt die Möglichkeit hinzu, Benachrichtigungen innerhalb von App-Erweiterungen zu planen und zu verarbeiten.
- Fügt einen neuen Erweiterungs Punkt für die Benachrichtigungen selbst hinzu. 

Das neue Benutzer Benachrichtigungs Framework bietet eine einheitliche Benachrichtigungs-API auf dem Vielfachen der von Apple unterstützten Plattformen, einschließlich: 

- vollständige **IOS** -Unterstützung zum Verwalten und Planen von Benachrichtigungen.
- **tvos** : fügt die Möglichkeit hinzu, App-Symbole für lokale und Remote Benachrichtigungen zu kennzeichnen.
- **watchos** : fügt die Möglichkeit hinzu, Benachrichtigungen vom gekoppelten IOS-Gerät des Benutzers an den Apple Watch weiterzuleiten, und ermöglicht es den Überwachungs-apps, lokale Benachrichtigungen direkt auf der überwachen selbst durchzuführen.

Weitere Informationen finden Sie in der [usernotification Framework-Referenz](https://developer.apple.com/reference/usernotifications) von Apple und in der [usernotificationsui](https://developer.apple.com/reference/usernotificationsui) -Dokumentation.

## <a name="preparing-for-notification-delivery"></a>Vorbereiten der Benachrichtigungs Übermittlung

Bevor eine IOS-App Benachrichtigungen an den Benutzer senden kann, muss die APP beim System registriert werden, und da eine Benachrichtigung für den Benutzer eine Unterbrechung ist, muss eine APP vor dem Senden explizit eine Berechtigung anfordern.

Es gibt drei verschiedene Ebenen von Benachrichtigungs Anforderungen, die der Benutzer für eine APP genehmigen kann:

- Banner angezeigt.
- Sound Warnungen.
- Badging das App-Symbol.

Außerdem müssen diese Genehmigungs Stufen angefordert und sowohl für lokale als auch für Remote Benachrichtigungen festgelegt werden.

Die Benachrichtigungs Berechtigung sollte angefordert werden, sobald die APP gestartet wird, `FinishedLaunching` indem der `AppDelegate` -Methode von folgender Code hinzugefügt und der gewünschte Benachrichtigungstyp (`UNAuthorizationOptions`) festgelegt wird:

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

Außerdem kann ein Benutzer jederzeit jederzeit die Benachrichtigungs Berechtigungen für eine App mithilfe der App " **Einstellungen** " auf dem Gerät ändern. Die APP sollte die angeforderten Benachrichtigungs Berechtigungen des Benutzers überprüfen, bevor eine Benachrichtigung mit dem folgenden Code angezeigt wird:

```csharp
// Get current notification settings
UNUserNotificationCenter.Current.GetNotificationSettings ((settings) => {
    var alertsAllowed = (settings.AlertSetting == UNNotificationSetting.Enabled);
});    
``` 

### <a name="configuring-the-remote-notifications-environment"></a>Konfigurieren der Remote Benachrichtigungs Umgebung

Neu bei IOS 10 ist, dass der Entwickler das Betriebssystem informieren muss, in welcher Umgebungs-Pushbenachrichtigung als Entwicklungs-oder Produktionsumgebung ausgeführt wird. Wenn diese Informationen nicht bereitgestellt werden, kann die APP zurückgewiesen werden, wenn Sie an den iTune App Store übermittelt wird und eine Benachrichtigung ähnlich der folgenden angezeigt wird:

> Fehlende pushbenachrichtigungsberechtigung: Ihre APP enthält eine API für den pushbenachrichtigungsdienst von Apple `aps-environment` , aber die Berechtigung fehlt in der Signatur der app.

Gehen Sie folgendermaßen vor, um die erforderliche Berechtigung zu erteilen:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Doppelklicken Sie auf `Entitlements.plist` die Datei im **Lösungspad** , um Sie für die Bearbeitung zu öffnen.
2. Wechseln Sie zur **Quell** Ansicht: 

    [![](enhanced-user-notifications-images/setup01.png "Die Quell Ansicht")](enhanced-user-notifications-images/setup01.png#lightbox)
3. Klicken Sie **+** auf die Schaltfläche, um einen neuen Schlüssel hinzuzufügen.
4. Geben `aps-environment` Sie für die `String` - **Eigenschaft**ein, belassen Sie den **Typ** , und geben Sie entweder `development` oder `production` für den folgenden **Wert**ein: 

    [![](enhanced-user-notifications-images/setup02.png "Die APS-Environment-Eigenschaft")](enhanced-user-notifications-images/setup02.png#lightbox)
5. Speichern Sie die Änderungen in der Datei.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Doppelklicken Sie auf `Entitlements.plist` die Datei im **Projektmappen-Explorer** , um Sie für die Bearbeitung zu öffnen.
2. Klicken Sie **+** auf die Schaltfläche, um einen neuen Schlüssel hinzuzufügen.
3. Geben `aps-environment` Sie für die `String` - **Eigenschaft**ein, belassen Sie den **Typ** , und geben Sie entweder `development` oder `production` für den folgenden **Wert**ein: 

    [![](enhanced-user-notifications-images/setup02w.png "Die APS-Environment-Eigenschaft")](enhanced-user-notifications-images/setup02.png#lightbox)
4. Speichern Sie die Änderungen in der Datei.

-----

### <a name="registering-for-remote-notifications"></a>Registrieren für Remote Benachrichtigungen

Wenn die APP Remote Benachrichtigungen sendet und empfängt, muss Sie weiterhin mithilfe der vorhandenen `UIApplication` API eine _tokenregistrierung_ durchführen. Diese Registrierung erfordert, dass das Gerät über einen Live-Netzwerk Verbindungs Zugriff auf APNs verfügt, wodurch das erforderliche Token generiert wird, das an die APP gesendet wird. Die APP muss dieses Token dann an die serverseitige App des Entwicklers weiterleiten, um sich für Remote Benachrichtigungen zu registrieren:

[![](enhanced-user-notifications-images/token01.png "Übersicht der tokenregistrierung")](enhanced-user-notifications-images/token01.png#lightbox)

Verwenden Sie den folgenden Code, um die erforderliche Registrierung zu initialisieren:

```csharp
UIApplication.SharedApplication.RegisterForRemoteNotifications ();
```

Das Token, das an die serverseitige App des Entwicklers gesendet wird, muss als Teil der Benachrichtigungs Nutzlast enthalten sein, die beim Senden einer Remote Benachrichtigung vom Server an APNs gesendet wird:

[![](enhanced-user-notifications-images/token02.png "Das Token, das als Teil der Benachrichtigungs Nutzlast enthalten ist.")](enhanced-user-notifications-images/token02.png#lightbox)

Das Token fungiert als Schlüssel, mit dem die Benachrichtigung verknüpft wird, und die APP, mit der die Benachrichtigung geöffnet oder beantwortet wird.

Weitere Informationen finden Sie in der Dokumentation zum [lokalen und Remote Benachrichtigungs Programmier Handbuch](https://developer.apple.com/documentation/usernotifications) von Apple.

## <a name="notification-delivery"></a>Benachrichtigungs Übermittlung

Wenn die APP vollständig registriert ist und die erforderlichen Berechtigungen vom Benutzer angefordert und erteilt wurden, ist die App nun bereit, Benachrichtigungen zu senden und zu empfangen. 

### <a name="providing-notification-content"></a>Angeben von Benachrichtigungs Inhalten

Neu bei IOS 10: alle Benachrichtigungen enthalten einen **Titel** und einen unter **Titel** , die immer mit dem **Text** des Benachrichtigungs Inhalts angezeigt werden. Außerdem ist die Möglichkeit, dem Benachrichtigungs Inhalt **Medien Anlagen** hinzuzufügen.

Um den Inhalt einer lokalen Benachrichtigung zu erstellen, verwenden Sie den folgenden Code:

```csharp
var content = new UNMutableNotificationContent();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;
```

Bei Remote Benachrichtigungen ist der Prozess ähnlich:

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

### <a name="scheduling-when-a-notification-is-sent"></a>Planen, wann eine Benachrichtigung gesendet wird

Nachdem der Inhalt der Benachrichtigung erstellt wurde, muss die APP planen, wann die Benachrichtigung für den Benutzer durch Festlegen eines *Auslösers*angezeigt wird. IOS 10 bietet vier verschiedene Auslösertypen:

- **Pushbenachrichtigung** : wird ausschließlich mit Remote Benachrichtigungen verwendet und wird ausgelöst, wenn APNs ein Benachrichtigungs Paket an die APP sendet, die auf dem Gerät ausgeführt wird.
- **Zeitintervall** : ermöglicht, dass eine lokale Benachrichtigung von einem Zeitintervall aus geplant wird, das mit der Zeit beginnt und einen späteren Zeitpunkt endet. Beispiel: `var trigger =  UNTimeIntervalNotificationTrigger.CreateTrigger (5, false);`
- **Kalenderdatum** : Hiermit können lokale Benachrichtigungen für ein bestimmtes Datum und eine bestimmte Uhrzeit geplant werden.
- **Speicherort basiert** : Hiermit können lokale Benachrichtigungen geplant werden, wenn das IOS-Gerät in einem bestimmten geografischen Standort eintritt oder verlässt oder sich in einer bestimmten Nähe von Bluetooth-Beacons befindet.

Wenn eine lokale Benachrichtigung bereit ist, muss die APP die `Add` -Methode `UNUserNotificationCenter` des-Objekts aufrufen, um die Anzeige für den Benutzer zu planen. Bei Remote Benachrichtigungen sendet die serverseitige App eine Benachrichtigungs Nutzlast an das APNs, das das Paket dann an das Gerät des Benutzers sendet.

Wenn Sie alle Teile zusammenbringen, könnte eine lokale Beispiel Benachrichtigung wie folgt aussehen:

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

## <a name="handling-foreground-app-notifications"></a>Behandeln von Benachrichtigungen im Vordergrund

Neu bei IOS 10. eine APP kann Benachrichtigungen anders verarbeiten, wenn Sie sich im Vordergrund befindet, und eine Benachrichtigung ausgelöst wird. Durch bereitstellen `UNUserNotificationCenterDelegate` eines und implementieren `WillPresentNotification` der-Methode kann die APP die Verantwortung für die Anzeige der Benachrichtigung übernehmen. Beispiel:

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

`UNNotification` Mit diesem Code wird lediglich der Inhalt von in die Anwendungs Ausgabe geschrieben, und das System wird aufgefordert, die Standard Warnung für die Benachrichtigung anzuzeigen. 

Wenn die APP die Benachrichtigung Selbstanzeigen soll, wenn Sie sich im Vordergrund befunden hat, und nicht die System Standardwerte verwenden, übergeben `None` Sie an den Abschluss Handler. Beispiel:

```csharp
completionHandler (UNNotificationPresentationOptions.None);
```

Öffnen Sie mit diesem Code die `AppDelegate.cs` Datei für die Bearbeitung, und ändern Sie die `FinishedLaunching` Methode so, dass Sie wie folgt aussieht:

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

Mit diesem Code wird der Benutzer `UNUserNotificationCenterDelegate` definierte von oben an den `UNUserNotificationCenter` aktuellen angehängt, sodass die APP Benachrichtigungen verarbeiten kann, während Sie aktiv ist und sich im Vordergrund befindet.

## <a name="notification-management"></a>Benachrichtigungs Verwaltung

Die Benachrichtigungs Verwaltung ist neu bei IOS 10 und bietet Zugriff auf ausstehende und übermittelte Benachrichtigungen und bietet die Möglichkeit, diese Benachrichtigungen zu entfernen, zu aktualisieren oder zu bewerben.

Ein wichtiger Teil der Benachrichtigungs Verwaltung ist die _Anforderungs_ -ID, die der Benachrichtigung zugewiesen wurde, als Sie erstellt und mit dem System geplant wurde. Bei Remote Benachrichtigungen wird dies über das neue `apps-collapse-id` Feld im HTTP-Anforderungs Header zugewiesen.

Der Anforderungs Bezeichner wird verwendet, um die Benachrichtigung auszuwählen, auf der die APP die Benachrichtigungs Verwaltung ausführen möchte.

### <a name="removing-notifications"></a>Entfernen von Benachrichtigungen

Verwenden Sie den folgenden Code, um eine ausstehende Benachrichtigung aus dem System zu entfernen:

```csharp
var requests = new string [] { "sampleRequest" };
UNUserNotificationCenter.Current.RemovePendingNotificationRequests (requests);
```

Zum Entfernen einer bereits übermittelten Benachrichtigung verwenden Sie den folgenden Code:

```csharp
var requests = new string [] { "sampleRequest" };
UNUserNotificationCenter.Current.RemoveDeliveredNotifications (requests);
```

### <a name="updating-an-existing-notification"></a>Aktualisieren einer vorhandenen Benachrichtigung

Um eine vorhandene Benachrichtigung zu aktualisieren, erstellen Sie einfach eine neue Benachrichtigung mit den gewünschten Parametern (z. b. einer neuen auslöserzeit), und fügen Sie Sie dem System mit dem gleichen Anforderungs Bezeichner wie die Benachrichtigung hinzu, die geändert werden muss. Beispiel:

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

Bei bereits übermittelten Benachrichtigungen wird die vorhandene Benachrichtigung aktualisiert und in der Liste auf dem Start-und Sperrbildschirm und im Benachrichtigungs Center hochgestuft, wenn Sie bereits vom Benutzer gelesen wurde.

## <a name="working-with-notification-actions"></a>Arbeiten mit Benachrichtigungs Aktionen

In ios 10 sind Benachrichtigungen, die an den Benutzer übermittelt werden, nicht statisch und bieten verschiedene Möglichkeiten, mit denen der Benutzer interagieren kann (von integrierten zu benutzerdefinierten Aktionen).

Es gibt drei Arten von Aktionen, auf die eine IOS-App reagieren kann:

- **Standardaktion** : Dies ist der Fall, wenn der Benutzer auf eine Benachrichtigung tippt, um die APP zu öffnen und die Details der angegebenen Benachrichtigung anzuzeigen.
- **Benutzerdefinierte Aktionen** : Diese wurden in ios 8 hinzugefügt und bieten dem Benutzer eine schnelle Möglichkeit, eine benutzerdefinierte Aufgabe direkt aus der Benachrichtigung auszuführen, ohne die app starten zu müssen. Sie können als Liste von Schaltflächen mit anpassbaren Titeln oder einem Texteingabefeld dargestellt werden, das entweder im Hintergrund (in dem die APP eine kurze Zeit zum erfüllen der Anforderung erhält) oder im Vordergrund (wo die APP im Vordergrund für Fu gestartet wird) ausgeführt werden kann. Füllen Sie die Anforderung aus.) Benutzerdefinierte Aktionen sind sowohl unter IOS als auch in watchos verfügbar.
- **Aktion verwerfen** : Diese Aktion wird an die APP gesendet, wenn der Benutzer eine bestimmte Benachrichtigung ablehnt.

### <a name="creating-custom-actions"></a>Erstellen von benutzerdefinierten Aktionen

Verwenden Sie den folgenden Code, um eine benutzerdefinierte Aktion im System zu erstellen und zu registrieren:

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

Beim Erstellen eines neuen `UNNotificationAction`wird ihm eine eindeutige ID und der Titel zugewiesen, der auf der Schaltfläche angezeigt wird. Standardmäßig wird die Aktion als Hintergrund Aktion erstellt, aber es können Optionen bereitgestellt werden, um das Verhalten der Aktion anzupassen (z. b. festlegen als Vordergrund Aktion).

Jede der erstellten Aktionen muss einer Kategorie zugeordnet werden. Beim Erstellen eines neuen `UNNotificationCategory`wird ihm eine eindeutige ID, eine Liste von Aktionen, die er ausführen kann, eine Liste mit Intent-IDs zugewiesen, um weitere Informationen über die Absicht der Aktionen in der Kategorie und einige Optionen zum Steuern des Verhaltens der Kategorie bereitzustellen.

Schließlich werden alle Kategorien mithilfe der `SetNotificationCategories` -Methode beim System registriert.

### <a name="presenting-custom-actions"></a>Präsentieren von benutzerdefinierten Aktionen

Nachdem eine Reihe benutzerdefinierter Aktionen und Kategorien erstellt und beim System registriert wurden, können Sie entweder lokal oder Remote Benachrichtigungen angezeigt werden.

Legen Sie für Remote Benachrichtigung einen `category` in der Remote Benachrichtigungs Nutzlast fest, der einer der oben erstellten Kategorien entspricht. Beispiel:

```csharp
{
    aps:{
        alert:"Hello world!",
        category:"message"
    }
}
```

Legen Sie für lokale Benachrichtigungen die `CategoryIdentifier` -Eigenschaft `UNMutableNotificationContent` des-Objekts fest. Beispiel:

```csharp
var content = new UNMutableNotificationContent ();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;
content.CategoryIdentifier = "message";
...
```

Diese ID muss mit einer der oben erstellten Kategorien verglichen werden.

### <a name="handling-dismiss-actions"></a>Behandeln von Verwerfen von Aktionen

Wie bereits erwähnt, kann eine Aktion zum verwerfen an die APP gesendet werden, wenn der Benutzer eine Benachrichtigung ablehnt. Da es sich hierbei nicht um eine Standardaktion handelt, muss beim Erstellen der Kategorie eine Option festgelegt werden. Beispiel:

```csharp
var categoryID = "message";
var actions = new UNNotificationAction [] { action };
var intentIDs = new string [] { };
var categoryOptions = new UNNotificationCategoryOptions [] { };
var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.CustomDismissAction);

```

### <a name="handling-action-responses"></a>Behandeln von Aktions Antworten

Wenn der Benutzer mit den benutzerdefinierten Aktionen und Kategorien interagiert, die oben erstellt wurden, muss die APP die angeforderte Aufgabe erfüllen. Dies erfolgt durch Bereitstellen eines `UNUserNotificationCenterDelegate` und Implementieren der `UserNotificationCenter` -Methode. Beispiel:

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

Die über `ActionIdentifier` gegebene `UNNotificationResponse` Klasse verfügt über eine-Eigenschaft, die entweder die Standardaktion oder die verwerfen-Aktion sein kann. Verwenden `response.Notification.Request.Identifier` Sie, um auf benutzerdefinierte Aktionen zu testen.

Die `UserText` -Eigenschaft enthält den Wert einer beliebigen Benutzer Texteingabe. Die `Notification` -Eigenschaft enthält die Ursprungs Benachrichtigung, die die Anforderung mit dem auslöserinhalt und dem Benachrichtigungs Inhalt enthält. Die APP kann entscheiden, ob es sich um eine lokale oder Remote Benachrichtigung handelt, die auf dem Typ des Auslösers basiert.

> [!NOTE]
> IOS 12 ermöglicht es, dass eine benutzerdefinierte Benachrichtigungs-UI ihre Aktions Schaltflächen zur Laufzeit ändert. Weitere Informationen finden Sie in der Dokumentation zu den Schaltflächen für die [dynamische Benachrichtigungs Aktion](~/ios/platform/introduction-to-ios12/notifications/dynamic-actions.md) .

## <a name="working-with-service-extensions"></a>Arbeiten mit Dienst Erweiterungen

Beim Arbeiten mit Remote Benachrichtigungen bieten _Dienst Erweiterungen_ eine Möglichkeit, die End-to-End-Verschlüsselung innerhalb der Benachrichtigungs Nutzlast zu aktivieren. Bei Dienst Erweiterungen handelt es sich um eine Nichtbenutzer Oberflächen Erweiterung (in ios 10 verfügbar), die im Hintergrund ausgeführt wird, wobei der Hauptzweck besteht, den sichtbaren Inhalt einer Benachrichtigung zu erweitern oder zu ersetzen, bevor Sie dem Benutzer angezeigt wird. 

[![](enhanced-user-notifications-images/extension01.png "Übersicht über die Dienst Erweiterung")](enhanced-user-notifications-images/extension01.png#lightbox)

Dienst Erweiterungen sollen schnell ausgeführt werden und erhalten nur einen kurzen Zeitraum für die Ausführung durch das System. Wenn die Dienst Erweiterung die Aufgabe nicht im zugewiesenen Zeitraum ausführen kann, wird eine Fall backmethode aufgerufen. Wenn das Fall Back fehlschlägt, wird dem Benutzer der ursprüngliche Benachrichtigungs Inhalt angezeigt.

Zu den möglichen Verwendungsmöglichkeiten von Dienst Erweiterungen gehören:

- Bereitstellen einer End-to-End-Verschlüsselung des Remote Benachrichtigungs Inhalts.
- Hinzufügen von Anlagen zu Remote Benachrichtigungen, um Sie zu erweitern.

### <a name="implementing-a-service-extension"></a>Implementieren einer Dienst Erweiterung

Gehen Sie folgendermaßen vor, um eine Dienst Erweiterung in einer xamarin. IOS-APP zu implementieren:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Öffnen Sie die Projekt Mappe der app in Visual Studio für Mac.
2. Klicken Sie im **Lösungspad** mit der rechten Maustaste auf den Projektmappennamen, und wählen Sie **hinzu** > fügen**Neues Projekt**hinzufügen
3. Wählen Sie **IOS** > **Extensions** > **Notification Service Extensions** aus, und klicken Sie auf die Schaltfläche **weiter** : 

    [![](enhanced-user-notifications-images/extension02.png "Benachrichtigungsdienst Erweiterungen auswählen")](enhanced-user-notifications-images/extension02.png#lightbox)
4. Geben Sie einen **Namen** für die Erweiterung ein, und klicken Sie auf die Schaltfläche **weiter** : 

    [![](enhanced-user-notifications-images/extension03.png "Geben Sie einen Namen für die Erweiterung ein.")](enhanced-user-notifications-images/extension03.png#lightbox)
5. Passen Sie den **Projektnamen** und/oder Projektmappennamen bei Bedarf an, und klicken Sie auf die Schaltfläche **Erstellen** 

    [![](enhanced-user-notifications-images/extension04.png "Anpassen des Projekt namens und/oder Projektmappennamens")](enhanced-user-notifications-images/extension04.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Öffnen Sie die Projekt Mappe der app in Visual Studio.
2. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektmappennamen, und wählen Sie **> Neues Projekt hinzufügen...** aus.
3. Wählen **Sie C# Visual > IOS-Erweiterungen > Benachrichtigungsdienst Erweiterung**aus:

    [![](enhanced-user-notifications-images/extension01.w157-sml.png "Benachrichtigungsdienst Erweiterungen auswählen")](enhanced-user-notifications-images/extension01.w157.png#lightbox)
4. Geben Sie einen **Namen** für die Erweiterung ein, und klicken Sie auf die Schaltfläche **OK** .

-----

> [!IMPORTANT]
> Die Bündel-ID für die Dienst Erweiterung sollte mit `.appnameserviceextension` der Bündel-ID der Haupt-APP, die an das Ende angehängt ist, entsprechen. Wenn die Haupt-App z `com.xamarin.monkeynotify` `com.xamarin.monkeynotify.monkeynotifyserviceextension`. b. einen Bündel Bezeichner von enthielt, sollte die Dienst Erweiterung eine Bündel-ID aufweisen. Dieser Wert sollte automatisch festgelegt werden, wenn die Erweiterung der Projekt Mappe hinzugefügt wird. 

Es gibt eine Hauptklasse in der Benachrichtigungsdienst Erweiterung, die geändert werden muss, um die erforderliche Funktionalität bereitzustellen. Beispiel:

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

Der ersten Methode, `DidReceiveNotificationRequest`, wird die Benachrichtigungs-ID sowie der Benachrichtigungs Inhalt über das `request` -Objekt übermittelt. `contentHandler` Der über gegebene muss aufgerufen werden, um dem Benutzer die Benachrichtigung zu präsentieren.

Die zweite Methode wird `TimeWillExpire`aufgerufen, kurz bevor die Zeit für die Dienst Erweiterung zur Verarbeitung der Anforderung abgelaufen ist. Wenn die Dienst Erweiterung den `contentHandler` nicht im zugewiesenen Zeitraum aufruft, wird der ursprüngliche Inhalt für den Benutzer angezeigt.

### <a name="triggering-a-service-extension"></a>Auslösen einer Dienst Erweiterung

Wenn eine Dienst Erweiterung erstellt und mit der APP bereitgestellt wird, kann Sie durch Ändern der an das Gerät gesendeten Remote Benachrichtigungs Nutzlast ausgelöst werden. Beispiel:

```csharp
{
    aps : {
        alert : "New Message Available",
        mutable-content: 1
    },
    encrypted-content : "#theencryptedcontent"
}
```

Der neue `mutable-content` Schlüssel gibt an, dass die Dienst Erweiterung gestartet werden muss, um den Inhalt der Remote Benachrichtigung zu aktualisieren. Der `encrypted-content` Schlüssel enthält die verschlüsselten Daten, die von der Dienst Erweiterung entschlüsselt werden können, bevor Sie dem Benutzer präsentiert werden.

Sehen Sie sich die folgende Beispiel-Dienst Erweiterung an:

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

`encrypted-content` Dieser Code entschlüsselt den verschlüsselten Inhalt aus dem Schlüssel, erstellt einen neuen `UNMutableNotificationContent`, legt die `Body` Eigenschaft auf den entschlüsselten Inhalt fest und verwendet das `contentHandler` , um die Benachrichtigung an den Benutzer zu präsentieren.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden alle Möglichkeiten beschrieben, wie Benutzer Benachrichtigungen durch IOS 10 erweitert wurden. Es wurde das neue Benutzer Benachrichtigungs Framework vorgestellt und erläutert, wie es in einer xamarin. IOS-APP oder-App-Erweiterung verwendet wird.

## <a name="related-links"></a>Verwandte Links

- [IOS 10-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
- [User Notification Framework-Referenz](https://developer.apple.com/reference/usernotifications)
- [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui)
- [Leitfaden zur lokalen und Remote Benachrichtigungs Programmierung](https://developer.apple.com/documentation/usernotifications)
