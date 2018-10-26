---
title: Erweiterte Benutzerbenachrichtigungen in Xamarin.iOS
description: In diesem Artikel werden, einen tieferen Einblick in die Benutzerbenachrichtigungen-Framework, die unter iOS 10 eingeführt wird. Es wird erläutert, benutzerbenachrichtigungen, die Benutzeroberfläche für die Benachrichtigung, medienanhänge, benutzerdefinierte Benutzeroberflächen und mehr.
ms.prod: xamarin
ms.assetid: 4E0C60AE-6F54-4098-8FA0-AADF9AC86805
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/03/2018
ms.openlocfilehash: b0571f826101576b402368923c2147e35aa9299e
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116328"
---
# <a name="advanced-user-notifications-in-xamarinios"></a>Erweiterte benutzerbenachrichtigungen in Xamarin.iOS

Noch nicht mit iOS 10, ermöglicht das Framework für die Bereitstellung und Verarbeitung von lokalen und remote-Benachrichtigungen, erhalten. Durch die Verwendung dieses Frameworks kann eine app oder app-Erweiterung die Übermittlung von lokalen Benachrichtigungen planen eine Reihe von Bedingungen wie z. B. Speicherort oder Uhrzeit angeben.

## <a name="about-user-notifications"></a>Informationen zu benutzerbenachrichtigungen

Das neue Benutzerbenachrichtigung-Framework kann für die Bereitstellung und Verarbeitung von lokalen und remote-Benachrichtigungen. Durch die Verwendung dieses Frameworks kann eine app oder App-Erweiterung die Übermittlung von lokalen Benachrichtigungen planen eine Reihe von Bedingungen wie z. B. Speicherort oder Uhrzeit angeben.

Darüber hinaus die app oder die Erweiterung kann empfangen (und potenziell ändern) sowohl lokale als auch Benachrichtigungen, wie sie iOS-Gerät des Benutzers übermittelt werden.

Das neue Benachrichtigung User Interface, UI-Framework ermöglicht es, eine app oder App-Erweiterung, die Darstellung von lokalen und Remotecomputern Benachrichtigungen anpassen, wenn sie dem Benutzer angezeigt werden.

Dieses Framework bietet es sich um die folgenden Möglichkeiten, die eine app Benachrichtigungen für einen Benutzer bereitstellen kann:

- **Visual Warnungen** –, in dem die Benachrichtigung vom oberen Rand des Bildschirms als Banner hinunter.
- **Sound- und Vibrationen** -kann eine Benachrichtigung zugeordnet werden.
- **App-Symbol Badging** – Wenn das Symbol der app einen Badge, die anzeigt zeigt, dass der neue Inhalt verfügbar ist. Z. B. die Anzahl der ungelesene e-Mail-Nachrichten.

Darüber hinaus je nach den aktuellen Kontext des Benutzers gibt es verschiedene Möglichkeiten, die eine Benachrichtigung angezeigt werden:

- Wenn das Gerät entsperrt wird, wird die Benachrichtigung vom oberen Rand des Bildschirms als Banner Abonnentenoptionen.
- Wenn das Gerät gesperrt ist, wird die Benachrichtigung auf dem Sperrbildschirm des Benutzers angezeigt werden.
- Wenn der Benutzer eine Benachrichtigung verpasst hat, können sie die Mitteilungszentrale zu öffnen und keine verfügbar ist, warten Benachrichtigungen anzuzeigen.

Eine Xamarin.iOS-app verfügt über zwei Arten von Benachrichtigungen für Benutzer, die sie senden kann:

- **Lokale Benachrichtigungen** -diese gesendet werden, indem apps, die lokal auf dem Gerät des Benutzers installiert.
- **Remotebenachrichtigungen** – werden gesendet, von einem Remoteserver und entweder dem Benutzer angezeigt, oder löst einen Hintergrund aktualisieren, der der Inhalt der app.

Weitere Informationen finden Sie unserem [erweiterte Benutzerbenachrichtigungen](~/ios/platform/user-notifications/enhanced-user-notifications.md) Dokumentation.

## <a name="the-new-user-notification-interface"></a>Die neue Benutzeroberfläche für die Benachrichtigung

Benutzerbenachrichtigungen unter iOS 10 sind mit einem neuen UI-Entwurf präsentiert mehr Inhalte wie einen Titel, Untertitel und eine optionale Medienanhänge, die angezeigt werden kann auf dem Sperrbildschirm angezeigt wird, als Banner oben auf dem Gerät oder in der Mitteilungszentrale.

Unabhängig davon, in denen eine Benachrichtigung für Benutzer unter iOS 10 angezeigt wird wird es mit der gleichen Erscheinungsbild und die gleichen Features und Funktionen angezeigt.

IOS 8 hat Apple umsetzbare Benachrichtigungen, in denen der Entwickler benutzerdefinierte Aktionen eine Benachrichtigung angefügt und ermöglicht dem Benutzer für die Reaktion auf eine Benachrichtigung zum Starten der app ohne konnte, eingeführt. In iOS 9 erweitert Apple umsetzbare Benachrichtigungen mit schnelle Antwort auf dem der Benutzer auf eine Benachrichtigung mit der Texteingabe reagieren kann.

Da Benutzerbenachrichtigungen mehr ganzzahligen Teil der Benutzeroberfläche in iOS 10 sind, hat Apple erweitert umsetzbare Benachrichtigungen für die Unterstützung von 3D Touch, in dem der Benutzer drückt sich auf eine Benachrichtigung und eine benutzerdefinierte Benutzeroberfläche Anzeige umfassende Interaktionen bereitstellen mit der Benachrichtigung.

Bei der benutzerdefinierten Benutzer-Notification-Benutzeroberfläche, angezeigt wird wenn alle Aktionen, die angefügt werden, auf die Benachrichtigung der Benutzer interagiert, kann die benutzerdefinierte Benutzeroberfläche sofort aktualisiert werden, um Ihr Feedback, was geändert wurde.

Neue iOS 10, die Benutzer-Notification-UI-API ermöglicht eine Xamarin.iOS-app problemlos nutzen diese neuen Funktionen für die User Interface, UI-Benachrichtigung.

## <a name="adding-media-attachments"></a>Hinzufügen von Medien-Anlagen

Eines der gängigeren Elemente, die von Benutzern gemeinsam zu erhalten ist Fotos, damit iOS 10 die Möglichkeit, einen Medien-Element (z. B. ein Foto) anzuhängen hinzugefügt direkt an eine Benachrichtigung, in denen diese präsentiert und für den Benutzer zusammen mit dem restlichen der Benachrichtigung Conte unmittelbar zur Verfügung stehen NT.

Aufgrund der Größe, die beim Senden wird jedoch auch ein kleines Bild enthält, an eine Remote Benachrichtigungsnutzlast anzufügen unpraktisch. Um diese Situation zu behandeln, kann der Entwickler verwenden die neue Webdiensterweiterung unter iOS 10 zum Herunterladen des Images aus einer anderen Quelle (z. B. ein CloudKit-Datenspeicher), und fügen Sie ihn an die benachrichtigungs Inhalt, bevor sie dem Benutzer angezeigt wird.

Für eine Remotebenachrichtigung geändert werden, indem eine Diensterweiterung muss die Nutzlast als änderbaren markiert werden. Zum Beispiel:

```csharp
{
    aps : {
        alert : "New Photo Available",
        mutable-content: 1
    },
    my-attachment : "https://example.com/photo.jpg"
}
```

Sehen Sie sich in der folgenden Übersicht des Prozesses:

[![](advanced-user-notifications-images/extension02.png "Prozess des Hinzufügens Medienanhänge")](advanced-user-notifications-images/extension02.png#lightbox)

Nach der Remote-Benachrichtigung an das Gerät (über APNs) gesendet wird, können die Webdiensterweiterung dann der erforderlichen Bildgröße keiner Weise gewünscht herunterladen (z. B. eine `NSURLSession`) und nach dem Erhalt der Abbildung können sie den Inhalt der Benachrichtigung und der Anzeige ändern Es ist für den Benutzer.

Im folgenden finden ein Beispiel, wie dieser Prozess in Code behandelt werden kann:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;

namespace MonkeyNotification
{
    public class NotificationService : UNNotificationServiceExtension
    {
        #region Constructors
        public NotificationService ()
        {
        }
        #endregion

        #region Override Methods
        public override void DidReceiveNotificationRequest (UNNotificationRequest request, Action<UNNotificationContent> contentHandler)
        {
            // Get file URL
            var attachementPath = request.Content.UserInfo.ObjectForKey (new NSString ("my-attachment"));
            var url = new NSUrl (attachementPath.ToString ());

            // Download the file
            var localURL = new NSUrl ("PathToLocalCopy");

            // Create attachment
            var attachmentID = "image";
            var options = new UNNotificationAttachmentOptions ();
            NSError err;
            var attachment = UNNotificationAttachment.FromIdentifier (attachmentID, localURL, options , out err);

            // Modify contents
            var content = request.Content.MutableCopy() as UNMutableNotificationContent;
            content.Attachments = new UNNotificationAttachment [] { attachment };

            // Display notification
            contentHandler (content);
        }

        public override void TimeWillExpire ()
        {
            // Handle service timing out
        }
        #endregion
    }
}
```

Wenn vom APNs die Benachrichtigung empfangen wird, wird die benutzerdefinierte Adresse des Images aus dem Inhalt gelesen und Datei wird vom Server heruntergeladen. Ein `UNNotificationAttachement` mit einer eindeutigen ID und den lokalen Speicherort für das Image erstellt wird (als eine `NSUrl`). Erstellt eine änderbare Kopie der Benachrichtigungsinhalt und Medienanhänge hinzugefügt werden. Zum Schluss die Benachrichtigung wird angezeigt, die dem Benutzer durch Aufrufen der `contentHandler`.

Sobald eine Benachrichtigung eine Anlage hinzugefügt wurde, hat das System die datenverschiebung und die Verwaltung der Datei.

Zusätzlich zu den Remotebenachrichtigungen oben angeführte, Medienanhänge auch von lokalen Benachrichtigungen unterstützt, in denen die `UNNotificationAttachement` erstellt und die Benachrichtigung zusammen mit seinen Inhalt zugeordnet ist.

Notification in iOS 10 unterstützt Medienanhänge von Bildern (statische und GIF-Dateien), Audio oder Video und das System zeigt automatisch die richtigen benutzerdefinierte Benutzeroberfläche für jeden dieser Typen von Anlagen bei der die Benachrichtigung für den Benutzer angezeigt wird.

> [!NOTE]
> Sollte geachtet werden, sowohl die Mediengröße zu optimieren und den Zeitaufwand, der von diesem Server die Medien herunterladen (oder das Medium für lokale Benachrichtigungen zusammenstellen) wie das System erzwingt strenge Beschränkungen, sowohl bei der Ausführung der app Service-Erweiterung. Betrachten Sie beispielsweise senden einen nach unten skalierte Version des Bilds oder einen kleinen Audioclip eines Videos die Benachrichtigung angezeigt werden sollen.

## <a name="creating-custom-user-interfaces"></a>Erstellen von benutzerdefinierten Benutzeroberflächen

Um eine benutzerdefinierte Benutzeroberfläche für Benutzerbenachrichtigungen zu erstellen, muss der Entwickler einer Erweiterung für Benachrichtigungsinhalte (IOS 10 neue) der app-Projektmappe hinzufügen.

Der Notification-Content-Erweiterung können Entwickler ihre eigenen Ansichten der Benutzeroberfläche für die Benachrichtigung hinzufügen, und zeichnen Sie alle gewünschten Inhalte. Ab iOS 12, unterstützt Notification-Content-Erweiterungen die interaktiven Benutzeroberflächen-Steuerelemente wie Schaltflächen und Regler an. Weitere Informationen finden Sie unter den [interaktive Benachrichtigungen unter iOS 12](~/ios/platform/introduction-to-ios12/notifications/interactive.md) Dokumentation.

Um die Benutzerinteraktion mit der eine Benutzerbenachrichtigung zu unterstützen, benutzerdefinierte Aktionen erstellt, mit dem System registriert und auf die Benachrichtigung angefügt wird, bevor sie mit dem System geplant ist. Die Erweiterung für Benachrichtigungsinhalte wird aufgerufen werden, um die Verarbeitung dieser Aktionen zu behandeln. Finden Sie unter den [arbeiten mit Benachrichtigungsaktionen](~/ios/platform/user-notifications/enhanced-user-notifications.md) Teil der [erweiterte Benutzerbenachrichtigungen](~/ios/platform/user-notifications/enhanced-user-notifications.md) Dokument Weitere Informationen zu benutzerdefinierten Aktionen.

Wenn eine Benachrichtigung für Benutzer mit einer benutzerdefinierten Benutzeroberfläche, die dem Benutzer angezeigt wird, müssen sie die folgenden Elemente:

[![](advanced-user-notifications-images/customui01.png "Eine Benachrichtigung für Benutzer mit einem benutzerdefinierten UI-Elementen")](advanced-user-notifications-images/customui01.png#lightbox)

Wenn der Benutzer mit benutzerdefinierten Aktionen (unter die Benachrichtigung angezeigt) interagiert, kann die Benutzeroberfläche aktualisiert werden, um gibt dem Benutzerfeedback als die Was ist passiert, wenn sie eine bestimmte Aktion aufgerufen.

### <a name="adding-a-notification-content-extension"></a>Hinzufügen einer Erweiterungs für benachrichtigungsinhalte

Um eine benutzerdefinierte Benutzer-Notification-Benutzeroberfläche in einer Xamarin.iOS-app zu implementieren, führen Sie folgende Schritte aus:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Öffnen der Projektmappe der app in Visual Studio für Mac.
2. Mit der rechten Maustaste auf den Namen der Projektmappe in die **Lösungspad** , und wählen Sie **hinzufügen** > **neues Projekt hinzufügen**.
3. Wählen Sie **iOS** > **Erweiterungen** > **Notification-Content-Erweiterungen** , und klicken Sie auf die **Weiter** Schaltfläche: 

    [![](advanced-user-notifications-images/notify01.png "Wählen Sie Notification-Content-Erweiterungen")](advanced-user-notifications-images/notify01.png#lightbox)
4. Geben Sie einen **Namen** für die Erweiterung und auf die **Weiter** Schaltfläche: 

    [![](advanced-user-notifications-images/notify02.png "Geben Sie einen Namen für die Erweiterung")](advanced-user-notifications-images/notify02.png#lightbox)
5. Anpassen der **Projektname** und/oder **Projektmappenname** Wenn erforderlich, und klicken Sie auf die **erstellen** Schaltfläche: 

    [![](advanced-user-notifications-images/notify03.png "Passen Sie den Projektnamen und/oder die Namen der Projektmappe")](advanced-user-notifications-images/notify03.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Öffnen der Projektmappe der app in Visual Studio für Mac.
2. Mit der rechten Maustaste auf den Namen der Projektmappe in die **Projektmappen-Explorer** , und wählen Sie **hinzufügen > Neues Projekt...** .
3. Wählen Sie **Visual C# > iOS-Erweiterungen > Erweiterung für Benachrichtigungsinhalte**:

    [![](advanced-user-notifications-images/notify01.w157-sml.png "Wählen Sie Notification-Content-Erweiterungen")](advanced-user-notifications-images/notify01.w157.png#lightbox)
4. Geben Sie einen **Namen** für die Erweiterung und auf die **OK** Schaltfläche.

-----

Wenn die Erweiterung für Benachrichtigungsinhalte zur Projektmappe hinzugefügt wird, werden drei Dateien im Projekt mit der Erweiterung erstellt werden:

1. `NotificationViewController.cs` : Dies ist der zentrale View-Controller für die Erweiterung für Benachrichtigungsinhalte.
2. `MainInterface.storyboard` : Der Speicherort der Entwickler der sichtbaren Benutzeroberfläche für die Erweiterung für Benachrichtigungsinhalte im iOS-Designer anordnet.
3. `Info.plist` -Die Konfiguration der Erweiterung für Benachrichtigungsinhalte steuert.

Der Standardwert `NotificationViewController.cs` Datei sieht wie folgt aus:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;


namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

        }
        #endregion
    }
}
```

Die `DidReceiveNotification` Methode wird aufgerufen, wenn die Benachrichtigung vom Benutzer erweitert wird, sodass die Erweiterung für Benachrichtigungsinhalte die benutzerdefinierte Benutzeroberfläche mit dem Inhalt der auffüllen können die `UNNotification`. Im obigen Beispiel eine Bezeichnung wurde in die Ansicht verfügbar gemacht werden, um Code mit dem Namen `label` und wird verwendet, um den Text der Benachrichtigung anzuzeigen.

### <a name="setting-the-notification-content-extensions-categories"></a>Festlegen der Erweiterung für Benachrichtigungsinhalte Kategorien

Das System muss angewiesen werden, wo finden der app Erweiterung für Benachrichtigungsinhalte basierend auf die einzelnen Kategorien reagiert werden soll. Führen Sie folgende Schritte aus:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Doppelklicken Sie auf der Erweiterungs `Info.plist` Datei die **Lösungspad** um ihn zur Bearbeitung zu öffnen.
2. Wechseln Sie zu der **Quelle** anzeigen.
3. Erweitern Sie die `NSExtension` Schlüssel.
4. Hinzufügen der `UNNotificationExtensionCategory` Schlüssel wird als Typ **Zeichenfolge** mit dem Wert der Kategorie die Erweiterung gehört (in diesem Beispiel "ereigniseinladung): 

    [![](advanced-user-notifications-images/customui02.png "Fügen Sie den Schlüssel UNNotificationExtensionCategory")](advanced-user-notifications-images/customui02.png#lightbox)
5. Speichern Sie die Änderungen.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Doppelklicken Sie auf der Erweiterungs `Info.plist` Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen.
3. Erweitern Sie die `NSExtension` Schlüssel.
4. Hinzufügen der `UNNotificationExtensionCategory` Schlüssel wird als Typ **Zeichenfolge** mit dem Wert der Kategorie die Erweiterung gehört (in diesem Beispiel "ereigniseinladung): 

    [![](advanced-user-notifications-images/customui02w.png "Fügen Sie den Schlüssel UNNotificationExtensionCategory")](advanced-user-notifications-images/customui02w.png#lightbox)
5. Speichern Sie die Änderungen.

-----

Benachrichtigung Inhaltskategorien-Erweiterung (`UNNotificationExtensionCategory`) verwenden Sie die gleiche Kategoriewerte, die zum Registrieren der Benachrichtigung-Aktionen verwendet werden. Wechseln Sie in der Situation, in dem die app die gleiche Benutzeroberfläche für mehrere Kategorien verwendet, die `UNNotificationExtensionCategory` in den Typ **Array** , und geben Sie alle erforderlichen Kategorien. Zum Beispiel:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![](advanced-user-notifications-images/customui03.png "Benachrichtigung Inhaltserweiterung Kategorien")](advanced-user-notifications-images/customui03.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](advanced-user-notifications-images/customui03w.png "Benachrichtigung Inhaltserweiterung Kategorien")](advanced-user-notifications-images/customui03w.png#lightbox)

-----

### <a name="hiding-the-default-notification-content"></a>Durch das Ausblenden des Standardinhalt der Benachrichtigung

In der Situation, in dem die benutzerdefinierte Benachrichtigung-Benutzeroberfläche wird werden Anzeigen von den gleichen Inhalt als Standard Notification (Titel, Untertitel und Text automatisch am unteren Rand der Benutzeroberfläche für die Benachrichtigung angezeigt wird), kann diese Standardeinstellung Informationen durch Hinzufügen der ausgeblendetwerden`UNNotificationExtensionDefaultContentHidden`-Schlüssel in der `NSExtensionAttributes` Schlüssel wird als Typ **booleschen** mit einem Wert von `YES` in der Erweiterungs `Info.plist` Datei:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![](advanced-user-notifications-images/customui04.png "Suchen von Standardinformationen")](advanced-user-notifications-images/customui04.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](advanced-user-notifications-images/customui04w.png "Suchen von Standardinformationen")](advanced-user-notifications-images/customui04w.png#lightbox)

-----

### <a name="designing-the-custom-ui"></a>Entwerfen der benutzerdefinierten Benutzeroberflächenautomatisierungs

Um die Erweiterung für Benachrichtigungsinhalte der benutzerdefinierten Benutzeroberfläche entwerfen möchten, doppelklicken Sie auf die `MainInterface.storyboard` Datei, die sie zur Bearbeitung in der iOS-Designer zu öffnen, die in den Elementen, die Sie benötigen, um die gewünschte Schnittstelle zu erstellen. ziehen Sie (wie z. B. `UILabels` und `UIImageViews`).

> [!NOTE]
> Ab iOS 12 kann eine Erweiterung für Benachrichtigungsinhalte interaktive Steuerelemente wie Schaltflächen und Textfelder enthalten. Weitere Informationen finden Sie unter den [interaktive Benachrichtigungen unter iOS 12](~/ios/platform/introduction-to-ios12/notifications/interactive.md) Dokumentation.

Sobald der Benutzeroberflächenautomatisierungs Layout und der erforderlichen Steuerelemente zur Verfügung C# Code öffnen die `NotificationViewController.cs` zum Bearbeiten und ändern Sie die `DidReceiveNotification` Methode, um die Benutzeroberfläche zu füllen, wenn der Benutzer die Benachrichtigung erweitert. Zum Beispiel:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;


namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = content.UserInfo ["location"].ToString ();
            if (location != null) {
                Event.Location.Text = location;
            }

        }
        #endregion
    }
}
```

### <a name="setting-the-content-area-size"></a>Festlegen der Größe des Inhaltsbereichs

Stellen Sie die Größe des Inhaltsbereichs für den Benutzer angezeigt, der folgende Code ist das der `PreferredContentSize` -Eigenschaft in der `ViewDidLoad` Methode, um die gewünschte Größe. Diese Größe kann auch durch Anwenden von Einschränkungen an die Ansicht in der iOS-Designer angepasst werden, es obliegt dem Entwickler, die die Methode auswählen, die für sie am besten geeignet ist.

Da die Benachrichtigung mit dem System bereits, bevor die Benachrichtigung Inhaltserweiterung aufgerufen wird, den Inhaltsbereich ausgeführt wird gestartet wird wird vollständige Größe und animiert werden, auf die angeforderte Größe, bei dem Benutzer angezeigt.

Um diesen Effekt zu entfernen, bearbeiten die `Info.plist` Datei für die Erweiterung, und legen die `UNNotificationExtensionInitialContentSizeRatio` -Schlüssel mit der die `NSExtensionAttributes` Schlüssel in den Typ **Anzahl** mit einem Wert, der das gewünschte Verhältnis darstellt. Zum Beispiel:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![](advanced-user-notifications-images/customui05.png "Der Schlüssel UNNotificationExtensionInitialContentSizeRatio")](advanced-user-notifications-images/customui05.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](advanced-user-notifications-images/customui05w.png "Der Schlüssel UNNotificationExtensionInitialContentSizeRatio")](advanced-user-notifications-images/customui05w.png#lightbox)

-----

### <a name="using-media-attachments-in-custom-ui"></a>Verwenden von medienanhänge in benutzerdefinierten Benutzeroberfläche

Da Medienanhänge (wie in der [Medienanhänge hinzufügen](#Adding-Media-Attachments) weiter oben) sind Teil der Nutzlast der Benachrichtigung, die sie Zugriff auf und in der Erweiterung für Benachrichtigungsinhalte angezeigt wird, wie sie in der standardmäßigen wäre werden Benachrichtigung über die Benutzeroberfläche.

Beispielsweise, wenn die benutzerdefinierte Benutzeroberfläche, die oben genannten enthalten eine `UIImageView` , die für verfügbar gemacht wurde C# den folgenden code, Code verwendet werden, um es aus mit der Media-Anlage zu füllen:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;

namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = content.UserInfo ["location"].ToString ();
            if (location != null) {
                Event.Location.Text = location;
            }

            // Get Media Attachment
            if (content.Attachements.Length > 1) {
                var attachment = content.Attachments [0];
                if (attachment.Url.StartAccessingSecurityScopedResource ()) {
                    EventImage.Image = UIImage.FromFile (attachment.Url.Path);
                    attachment.Url.StopAccessingSecurityScopedResource ();
                }
            }
        }
        #endregion
    }
}
```

Da die Anlage mit Medien, die vom System verwaltet wird, ist es außerhalb der app-Sandbox. Die Erweiterung muss das System zu informieren, dass er Zugriff auf die Datei durch aufrufen will die `StartAccessingSecurityScopedResource` Methode. Wenn die Erweiterung mit der Datei abgeschlossen ist, muss zum Aufrufen der `StopAccessingSecurityScopedResource` seine Verbindung freigeben.

### <a name="adding-custom-actions-to-a-custom-ui"></a>Hinzufügen von benutzerdefinierten Aktionen, um eine benutzerdefinierte Benutzeroberfläche

Benutzerdefinierte Aktionsschaltflächen können verwendet werden, um eine benutzerdefinierte Benachrichtigung Benutzeroberfläche Interaktivität hinzuzufügen. Finden Sie unter den [arbeiten mit Benachrichtigungsaktionen](~/ios/platform/user-notifications/enhanced-user-notifications.md) Teil der [erweiterte Benutzerbenachrichtigungen](~/ios/platform/user-notifications/enhanced-user-notifications.md) Dokument Weitere Informationen zu benutzerdefinierten Aktionen.

Zusätzlich zu den benutzerdefinierten Aktionen können die Erweiterung für Benachrichtigungsinhalte auf sowie die folgenden integrierten Aktionen reagieren:

- **Standardaktion** – Dies ist, wenn der Benutzer tippt auf eine Benachrichtigung an die app zu öffnen und die Details der angegebenen Benachrichtigung angezeigt.
- **Schließen Sie die Aktion** – diese Aktion wird für die app gesendet, wenn der Benutzer eine bestimmte Benachrichtigung schließt.

Notification-Content-Erweiterungen haben auch die Möglichkeit, ihre Benutzeroberfläche zu aktualisieren, wenn der Benutzer eine der benutzerdefinierten Aktionen aufruft, wie z.B. das Anzeigen eines Datums als wenn akzeptiert der Benutzer tippt der **Accept** Schaltfläche "benutzerdefinierte Aktion". Darüber hinaus können die Notification-Content-Erweiterungen das System informiert, um die Entlassung der Benachrichtigung Benutzeroberfläche zu verzögern, damit der Benutzer die Auswirkung ihrer Aktion sehen kann, bevor die Benachrichtigung geschlossen wird.

Dies erfolgt durch eine zweite Version der Implementierung der `DidReceiveNotification` Methode, die einen Abschlusshandler enthält. Zum Beispiel:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;
using CoreGraphics;

namespace myApp {
    public class NotificationViewController : UIViewController, UNNotificationContentExtension {

        public override void ViewDidLoad() {
            base.ViewDidLoad();

            // Adjust the size of the content area
            var size = View.Bounds.Size
            PreferredContentSize = new CGSize(size.Width, size.Width/2);
        }

        public void DidReceiveNotification(UNNotification notification) {

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = Content.UserInfo["location"] as string;
            if (location != null) {
                Event.Location.Text = location;
            }

            // Get Media Attachment
            if (content.Attachements.Length > 1) {
                var attachment = content.Attachments[0];
                if (attachment.Url.StartAccessingSecurityScopedResource()) {
                    EventImage.Image = UIImage.FromFile(attachment.Url.Path);
                    attachment.Url.StopAccessingSecurityScopedResource();
                }
            }
        }

        [Export ("didReceiveNotificationResponse:completionHandler:")]
        public void DidReceiveNotification (UNNotificationResponse response, Action<UNNotificationContentExtensionResponseOption> completionHandler)
        {

            // Update UI when the user interacts with the
            // Notification
            Server.PostEventResponse += (response) {
                // Take action based on the response
                switch(response.ActionIdentifier){
                case "accept":
                    EventResponse.Text = "Going!";
                    EventResponse.TextColor = UIColor.Green;
                    break;
                case "decline":
                    EventResponse.Text = "Not Going.";
                    EventResponse.TextColor = UIColor.Red;
                    break;
                }

                // Close Notification
                completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
            };
        }
    }
}
```

Durch Hinzufügen der `Server.PostEventResponse` Handler, der die `DidReceiveNotification` Methode von der Erweiterung für Benachrichtigungsinhalte, die Erweiterung *müssen* allen benutzerdefinierte Aktionen zu behandeln. Die Erweiterung kann auch die benutzerdefinierten Aktionen auf den enthaltenden app weiterleiten, durch Ändern der `UNNotificationContentExtensionResponseOption`. Zum Beispiel:

```csharp
// Close Notification
completionHandler (UNNotificationContentExtensionResponseOption.DismissAndForwardAction);
```

### <a name="working-with-the-text-input-action-in-custom-ui"></a>Arbeiten mit der Eingabeaktion von Text in benutzerdefinierten Benutzeroberfläche

Je nach Entwurf der app und die Benachrichtigung es gibt möglicherweise Fälle, in denen der Benutzer zur Eingabe von Text in der Benachrichtigung (z. B. auf eine Nachricht Antworten) erforderlich. Eine Erweiterung für Benachrichtigungsinhalte hat Zugriff auf die integrierte Text input-Aktion an, wie eine standard-Benachrichtigung wird.

Zum Beispiel:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;


namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Computed Properties
        // Allow to take input
        public override bool CanBecomeFirstResponder {
            get { return true; }
        }

        // Return the custom created text input view with the
        // required buttons and return here
        public override UIView InputAccessoryView {
            get { return InputView; }
        }
        #endregion

        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }
        #endregion

        #region Private Methods
        private UNNotificationCategory MakeExtensionCategory ()
        {

            // Create Accept Action
            ...

            // Create decline Action
            ...

            // Create Text Input Action
            var commentID = "comment";
            var commentTitle = "Comment";
            var textInputButtonTitle = "Send";
            var textInputPlaceholder = "Enter comment here...";
            var commentAction = UNTextInputNotificationAction.FromIdentifier (commentID, commentTitle, UNNotificationActionOptions.None, textInputButtonTitle, textInputPlaceholder);

            // Create category
            var categoryID = "event-invite";
            var actions = new UNNotificationAction [] { acceptAction, declineAction, commentAction };
            var intentIDs = new string [] { };
            var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.None);

            // Return new category
            return category;

        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = content.UserInfo ["location"].ToString ();
            if (location != null) {
                Event.Location.Text = location;
            }

            // Get Media Attachment
            if (content.Attachements.Length > 1) {
                var attachment = content.Attachments [0];
                if (attachment.Url.StartAccessingSecurityScopedResource ()) {
                    EventImage.Image = UIImage.FromFile (attachment.Url.Path);
                    attachment.Url.StopAccessingSecurityScopedResource ();
                }
            }
        }

        [Export ("didReceiveNotificationResponse:completionHandler:")]
        public void DidReceiveNotification (UNNotificationResponse response, Action<UNNotificationContentExtensionResponseOption> completionHandler)
        {

            // Is text input?
            if (response is UNTextInputNotificationResponse) {
                var textResponse = response as UNTextInputNotificationResponse;
                Server.Send (textResponse.UserText, () => {
                    // Close Notification
                    completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
                });
            }

            // Update UI when the user interacts with the
            // Notification
            Server.PostEventResponse += (response) {
                // Take action based on the response
                switch (response.ActionIdentifier) {
                case "accept":
                    EventResponse.Text = "Going!";
                    EventResponse.TextColor = UIColor.Green;
                    break;
                case "decline":
                    EventResponse.Text = "Not Going.";
                    EventResponse.TextColor = UIColor.Red;
                    break;
                }

                // Close Notification
                completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
            };
        }
        #endregion
    }
}
```

Dieser Code erstellt eine neue Eingabe Aktion für Text und fügt es der Erweiterung Kategorie hinzu (in der `MakeExtensionCategory`) Methode. In der `DidReceive` außer Kraft setzen-Methode verarbeitet den Benutzer eingeben von Text mit den folgenden Code:

```csharp
// Is text input?
if (response is UNTextInputNotificationResponse) {
    var textResponse = response as UNTextInputNotificationResponse;
    Server.Send (textResponse.UserText, () => {
        // Close Notification
        completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
    });
}
```

Wenn der Entwurf benutzerdefinierte Schaltflächen hinzufügen, um das Textfeld für die Eingabe erfordert, fügen Sie den folgenden Code aus, um sie einzubeziehen:

```csharp
// Allow to take input
public override bool CanBecomeFirstResponder {
    get {return true;}
}

// Return the custom created text input view with the
// required buttons and return here
public override UIView InputAccessoryView {
    get {return InputView;}
}
```

Wenn die Kommentar-Aktion, die vom Benutzer ausgelöst wird, müssen sowohl der ansichtscontroller und das Eingabefeld mit benutzerdefiniertem Text aktiviert werden:

```csharp
// Update UI when the user interacts with the
// Notification
Server.PostEventResponse += (response) {
    // Take action based on the response
    switch(response.ActionIdentifier){
    ...
    case "comment":
        BecomeFirstResponder();
        TextField.BecomeFirstResponder();
        break;
    }

    // Close Notification
    completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);

};
```

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde eine erweiterte ansehen, verwenden das neue Framework für die Benachrichtigung für Benutzer in einer Xamarin.iOS-app unternommen. Es behandelt Media-Anlagen in sowohl lokale als auch Remote-Benachrichtigung hinzufügen, und sie behandelt, mit der Benutzeroberfläche für die neue Benutzer-Benachrichtigung Benutzeroberflächen für benutzerdefinierte Benachrichtigung zu erstellen.

## <a name="related-links"></a>Verwandte Links

- [iOS 10-Beispiele](https://developer.xamarin.com/samples/ios/iOS10/)
- ["Usernotifications"-Framework-Referenz](https://developer.apple.com/reference/usernotifications)
- [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui)
- [Programmierhandbuch für lokale und Remote-Benachrichtigung](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/Introduction.html)
