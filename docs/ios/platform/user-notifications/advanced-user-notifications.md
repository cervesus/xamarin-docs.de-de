---
title: Fortgeschrittene Benutzerbenachrichtigungen
description: "Dieser Artikel hat einen tieferen Einblick in das neue Benutzerbenachrichtigungen-Framework und wie Sie vollständige in einem Xamarin.iOS-app nutzen."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4e1ff652-28f0-4566-b383-9d12664401a4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 6408f3b45f93413fa814e410f07e7b71179b7338
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="advanced-user-notifications"></a>Fortgeschrittene Benutzerbenachrichtigungen

_Dieser Artikel hat einen tieferen Einblick in das neue Benutzerbenachrichtigungen-Framework und wie Sie vollständige in einem Xamarin.iOS-app nutzen._

Neue für iOS-10, ermöglicht das Framework für die Bereitstellung und Verarbeitung von lokalen und remote-Benachrichtigungen, erhalten. Mit diesem Framework, kann eine app oder app-Erweiterung die Übermittlung von lokalen Benachrichtigungen planen durch eine Reihe von Bedingungen, z. B. Speicherort oder die Uhrzeit angeben.

## <a name="about-user-notifications"></a>Informationen zu Benutzerbenachrichtigungen

Das neue Framework für die Benutzerbenachrichtigung kann für die Bereitstellung und Verarbeitung von lokalen und remote-Benachrichtigungen. Mit diesem Framework, kann eine app oder App-Erweiterung die Übermittlung von lokalen Benachrichtigungen planen durch eine Reihe von Bedingungen, z. B. Speicherort oder die Uhrzeit angeben.

Darüber hinaus die app oder eine Erweiterung kann empfangen (und möglicherweise ändern) sowohl lokale als auch Benachrichtigungen, wie sie iOS-Gerät des Benutzers übermittelt werden.

Der neue Benutzer Benachrichtigung Benutzeroberflächen-Framework ermöglicht es, eine Anwendung oder ein App-Erweiterung, um die Darstellung des sowohl lokale als auch Benachrichtigungen anpassen, wenn sie dem Benutzer angezeigt werden.

Dieses Framework bietet, die eine app Benachrichtigungen an Benutzer übermitteln kann folgendermaßen:

- **Visual Warnungen** –, in dem die Benachrichtigung vom oberen Rand des Bildschirms als Banner hinunter.
- **Sound und Vibrationen** -kann eine Benachrichtigung zugeordnet werden.
- **App-Symbol Signalisierung** – Wenn das Symbol für die app eine Badge anzeigt zeigt, dass der neue Inhalt verfügbar ist. Z. B. die Anzahl der ungelesene e-Mail-Nachrichten.

Darüber hinaus, abhängig vom aktuellen Kontext des Benutzers gibt es verschiedene Möglichkeiten, die eine Benachrichtigung angezeigt werden:

- Wenn das Gerät entsperrt wird, wird die Benachrichtigung vom oberen Rand des Bildschirms als Banner Abonnentenoptionen.
- Wenn das Gerät gesperrt ist, wird die Benachrichtigung auf dem Sperrbildschirm des Benutzers angezeigt werden.
- Wenn der Benutzer eine Benachrichtigung verpasst hat, können die Mitteilungszentrale öffnen und alle verfügbaren wartenden Benachrichtigungen anzeigen.

Eine Xamarin.iOS app verfügt über zwei Arten von Benachrichtigungen für Benutzer, die sie senden kann:

- **Lokale Benachrichtigungen** – diese werden von apps, die lokal auf dem Gerät des Benutzers installiert gesendet.
- **Remote Benachrichtigungen** - gesendet werden, von einem Remoteserver und entweder dem Benutzer angezeigt oder Trigger ein Update im Hintergrund der app-Inhalte.

Weitere Informationen finden Sie unter unsere [Benutzerbenachrichtigungen erweiterte](~/ios/platform/user-notifications/enhanced-user-notifications.md) Dokumentation.

## <a name="the-new-user-notification-interface"></a>Die neue Benutzeroberfläche für die Benachrichtigung

Benutzerbenachrichtigungen in iOS 10 sind mit einem neuen Benutzeroberflächenentwurf präsentiert mehr Inhalt wie z. B. ein Titel und Untertitel optionale Medien Anlagen, die angezeigt werden kann auf dem Sperrbildschirm als Banner am Anfang des Geräts oder die mitteilungszentrale.

Unabhängig davon, wo eine Benachrichtigung für Benutzer in iOS 10 angezeigt wird wird sie mit der gleichen Aussehen und Verhalten und die gleichen Features und Funktionen angezeigt.

Apple iOS 8 eingeführt aussagefähige Benachrichtigungen, in denen Entwickler benutzerdefinierte Aktionen eine Benachrichtigung angefügt und ermöglicht dem Benutzer Aktionen für eine Benachrichtigung ausführen können, ohne die app zu starten konnte. In iOS 9 erweitert, Apple aussagefähige Benachrichtigungen mit schnelle Antwort, die dem Benutzer so reagieren Sie auf eine Benachrichtigung mit der Texteingabe von ermöglicht.

Da Benutzerbenachrichtigungen mehr Bestandteil der benutzererfahrung in iOS 10 sind, verfügt über Apple erweitert aussagefähige Benachrichtigungen zur Unterstützung 3D Touch, in denen der Benutzer klickt auf eine Benachrichtigung und eine benutzerdefinierte Benutzeroberfläche anzeigen, um umfangreiche Interaktion bereitzustellen Bei der Benachrichtigung.

Wenn die benutzerdefinierte Benutzer-Benachrichtigung-Benutzeroberfläche angezeigt wird, wenn die Interaktion des Benutzers mit Maßnahmen, die auf die Benachrichtigung angefügt, kann die benutzerdefinierte Benutzeroberfläche sofort aktualisiert werden, zum Bereitstellen von Feedback, was geändert wurde.

Neu für iOS 10, ermöglicht der Benutzer Notification-UI-API eine Xamarin.iOS-app, diese neuen Features für den Benutzer Benachrichtigung UI problemlos nutzen.

## <a name="adding-media-attachments"></a>Hinzufügen von Anlagen

Eines der gängigeren Elemente, die Benutzer gemeinsam erhalten also Fotos, iOS 10 die Möglichkeit, fügen Sie ein Medienobjekt (z. B. ein Foto) hinzugefügt direkt an eine Benachrichtigung, in denen es präsentiert und dem Benutzer zusammen mit dem Rest der die Benachrichtigung Konte unmittelbar verfügbar sein wird NT.

Aufgrund der beteiligten senden Größen wird jedoch auch ein kleines Bild, an einen Remoteserver Benachrichtigungsnutzlast anzufügen alleine nicht durchführbar. Um diese Situation zu behandeln, kann der Entwickler mithilfe der neuen Webdiensterweiterung in iOS-10 das Bild aus einer anderen Quelle (z. B. einen Datenspeicher CloudKit) herunterladen und auf den Inhalt der Benachrichtigung anfügen, bevor er dem Benutzer angezeigt wird.

Für eine Remote-Benachrichtigung durch eine Erweiterung der geändert werden muss die Nutzlast als änderbare markiert sein. Zum Beispiel:

```csharp
{
    aps : {
        alert : "New Photo Available",
        mutable-content: 1
    },
    my-attachment : "https://example.com/photo.jpg"
}
```

Sehen Sie sich in der folgenden Übersicht des Prozesses an:

[ ![](advanced-user-notifications-images/extension02.png "Hinzufügen von Anlagen für Media-Prozess")](advanced-user-notifications-images/extension02.png)

Sobald der Remote-Benachrichtigung an das Gerät (über APNs) übermittelt wird, können die Webdiensterweiterung dann die erforderliche Image über gewünschten Mitteln herunterladen (z. B. ein `NSURLSession`) und nachdem sie das Bild erhalten hat, können sie den Inhalt der Benachrichtigung und zur Anzeige ändern es dem Benutzer.

Im folgenden finden ein Beispiel, wie dieser Vorgang im Code verarbeitet werden kann:

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

Wenn vom APNs die Benachrichtigung empfangen wird, wird die benutzerdefinierte Adresse des Images aus dem Inhalt gelesen und -Datei wird vom Server heruntergeladen. Ein `UNNotificationAttachement` mit einer eindeutigen ID und den lokalen Speicherort des Bilds erstellt wird (als eine `NSUrl`). Erstellt eine änderbare Kopie der Benachrichtigung Inhalte und Anlagen werden hinzugefügt. Schließlich die Benachrichtigung wird angezeigt, die dem Benutzer durch Aufrufen der `contentHandler`.

Sobald eine Benachrichtigung eine Anlage hinzugefügt wurde, übernimmt das System die datenverschiebung und die Verwaltung der Datei.

Zusätzlich zu den oben aufgeführten Remote Benachrichtigungen Medien Anlagen werden auch unterstützt von lokalen Benachrichtigungen, in dem die `UNNotificationAttachement` erstellt und auf die Benachrichtigung zusammen mit seinen Inhalt angefügt.

Benachrichtigung in iOS 10 unterstützen Medien Anlagen von Bildern (statische und GIF-Dateien), Audio oder Video und das System zeigt automatisch die richtige benutzerdefinierte Benutzeroberfläche für jeden dieser Typen von Anlagen, wenn der Benutzer die Benachrichtigung angezeigt wird.

> [!NOTE]
> **Hinweis:** sollte geachtet werden sowohl die Mediengröße zu optimieren und die benötigte Zeit zum Herunterladen von Medien vom Remoteserver (oder das Medium für lokale Benachrichtigungen zusammenstellen) des Systems erzwingt strenge Grenzwerte sowohl beim Ausführen der app-Diensts Erweiterung. Betrachten Sie beispielsweise das Senden einer nach unten skalierte Version des Bilds oder einen kleinen Clip eines Videos in der Benachrichtigung dargestellt werden soll.

## <a name="creating-custom-user-interfaces"></a>Erstellen benutzerdefinierter Benutzeroberflächen

Um eine benutzerdefinierte Benutzeroberfläche für ihre Benutzerbenachrichtigungen zu erstellen, muss der Entwickler der app-Projektmappe eine Benachrichtigung Erweiterung Inhalt (neu in iOS 10) hinzugefügt.

Die Benachrichtigung Inhalt Erweiterung kann der Entwickler auf die Benachrichtigung Benutzeroberfläche eigene Ansichten hinzu, und zeichnen Sie alle gewünschten Inhalte. Interaktive Benutzeroberflächenwidgets (z. B. Schaltflächen und Textfeldern) sind jedoch **nicht** unterstützt über die Benutzeroberfläche für die Benachrichtigung und nie eine benutzerdefinierte Benutzeroberflächenentwurf hinzugefügt werden sollen.

Unterstützung von Interaktionen der Benutzer mit einer Benachrichtigung für Benutzer, benutzerdefinierte Aktionen erstellt, mit dem System registriert und auf die Benachrichtigung angefügt wird, bevor sie mit dem System geplant ist. Die Erweiterung für die Benachrichtigung-Inhalt wird für die Verarbeitung dieser Aktionen aufgerufen werden. Finden Sie unter der [arbeiten mit Benachrichtigungsaktionen](~/ios/platform/user-notifications/enhanced-user-notifications.md) Teil der [Benutzerbenachrichtigungen erweiterte](~/ios/platform/user-notifications/enhanced-user-notifications.md) Dokument Weitere Informationen zu benutzerdefinierten Aktionen.

Wenn der Benutzer eine Benachrichtigung für Benutzer mit einer benutzerdefinierten Benutzeroberfläche angezeigt wird, müssen sie die folgenden Elemente:

[ ![](advanced-user-notifications-images/customui01.png "Eine Benachrichtigung für Benutzer mit einem benutzerdefinierten Benutzeroberflächenelemente")](advanced-user-notifications-images/customui01.png)

Wenn der Benutzer mit den benutzerdefinierten Aktionen (unten die Benachrichtigung dargestellt) interagiert, kann die Benutzeroberfläche aktualisiert werden, um die Benutzer Ihr Feedback als das was geschah, als sie eine bestimmte Aktion aufgerufen.

### <a name="adding-a-notification-content-extension"></a>Hinzufügen einer Benachrichtigung Content-Erweiterung

Führen Sie folgende Schritte aus, um eine benutzerdefinierte Benutzer-Benachrichtigung-Benutzeroberfläche in einem Xamarin.iOS-app zu implementieren:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Öffnen Sie die app-Projektmappe in Visual Studio für Mac.
2. Mit der rechten Maustaste auf den Namen der Projektmappe in der **Lösung Pad** , und wählen Sie **hinzufügen** > **neues Projekt hinzufügen**.
3. Wählen Sie **iOS** > **Erweiterungen** > **Benachrichtigung Content Erweiterungen** , und klicken Sie auf die **Weiter** Schaltfläche: 

    [ ![](advanced-user-notifications-images/notify01.png "Wählen Sie die Benachrichtigung Content-Erweiterungen")](advanced-user-notifications-images/notify01.png)
4. Geben Sie einen **Namen** für die Erweiterung, und klicken Sie auf die **Weiter** Schaltfläche: 

    [ ![](advanced-user-notifications-images/notify02.png "Geben Sie einen Namen für die Erweiterung")](advanced-user-notifications-images/notify02.png)
5. Anpassen der **Projektname** und/oder **Projektmappenname** Wenn erforderlich, und klicken Sie auf die **erstellen** Schaltfläche: 

    [ ![](advanced-user-notifications-images/notify03.png "Passen Sie die Projektnamen und/oder die Namen der Projektmappe")](advanced-user-notifications-images/notify03.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Öffnen Sie die app-Projektmappe in Visual Studio für Mac.
2. Mit der rechten Maustaste auf den Namen der Projektmappe in der **Projektmappen-Explorer** , und wählen Sie **hinzufügen** > **neues Projekt hinzufügen**.
3. Wählen Sie **iOS** > **Erweiterungen** > **Benachrichtigung Content Erweiterungen**: 

    [ ![](advanced-user-notifications-images/notify01w.png "Wählen Sie die Benachrichtigung Content-Erweiterungen")](advanced-user-notifications-images/notify01w.png)
4. Geben Sie einen **Namen** für die Erweiterung, und klicken Sie auf die **OK** Schaltfläche.

-----

Wenn die Erweiterung für die Inhalt der Benachrichtigung zur Projektmappe hinzugefügt wird, werden drei Dateien im Projekt für die Erweiterung erstellt werden:

1. `NotificationViewController.cs` -Dies ist die Hauptansicht Controller für die Erweiterung für die Inhalt der Benachrichtigung.
2. `MainInterface.storyboard` -, In denen der Entwickler der sichtbare Benutzeroberfläche für die Erweiterung der Benachrichtigung Inhalt in der iOS-Designer angeordnet.
3. `Info.plist` -Steuert die Konfiguration der Erweiterung der Benachrichtigung-Inhalt.

Die Standardeinstellung `NotificationViewController.cs` Datei sieht wie folgt:

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

Die `DidReceiveNotification` Methode wird aufgerufen, wenn die Benachrichtigung vom Benutzer erweitert wird, damit die Erweiterung für die Inhalt der Benachrichtigung die benutzerdefinierte Benutzeroberfläche mit dem Inhalt des auffüllen können die `UNNotification`. Für das obige Beispiel eine Bezeichnung hinzugefügt wurde in die Ansicht verfügbar gemacht werden, um Code mit dem Namen `label` und wird verwendet, um den Text der Benachrichtigung angezeigt.

### <a name="setting-the-notification-content-extensions-categories"></a>Die Benachrichtigung Content Erweiterung Kategorien festlegen

Das System muss darüber informiert werden, finden Sie die Erweiterung für der app-Benachrichtigung Inhalt basierend auf bestimmten Kategorien reagiert werden soll. Führen Sie folgende Schritte aus:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Doppelklicken Sie auf der Erweiterungs `Info.plist` in der Datei die **Lösung Pad** um ihn zur Bearbeitung zu öffnen.
2. Wechseln Sie zu der **Quelle** anzeigen.
3. Erweitern Sie die `NSExtension` Schlüssel.
4. Hinzufügen der `UNNotificationExtensionCategory` als Typ Key **Zeichenfolge** mit dem Wert der Kategorie die Erweiterung gehört (in diesem Beispiel "Ereignis-Einladung): 

    [ ![](advanced-user-notifications-images/customui02.png "Fügen Sie der UNNotificationExtensionCategory-Schlüssel")](advanced-user-notifications-images/customui02.png)
5. Speichern Sie die Änderungen.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Doppelklicken Sie auf der Erweiterungs `Info.plist` in der Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen.
3. Erweitern Sie die `NSExtension` Schlüssel.
4. Hinzufügen der `UNNotificationExtensionCategory` als Typ Key **Zeichenfolge** mit dem Wert der Kategorie die Erweiterung gehört (in diesem Beispiel "Ereignis-Einladung): 

    [ ![](advanced-user-notifications-images/customui02w.png "Fügen Sie der UNNotificationExtensionCategory-Schlüssel")](advanced-user-notifications-images/customui02w.png)
5. Speichern Sie die Änderungen.

-----

Benachrichtigung Content Erweiterung Kategorien (`UNNotificationExtensionCategory`) verwenden Sie die gleiche Kategoriewerte, die verwendet werden, um Benachrichtigungsaktionen zu registrieren. Wechseln Sie in der Lage, in dem die app die gleiche UI für mehrere Kategorien verwendet, die `UNNotificationExtensionCategory` in den Typ **Array** , und geben Sie alle erforderlichen Kategorien. Zum Beispiel:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[ ![](advanced-user-notifications-images/customui03.png "Benachrichtigung Erweiterung Inhalt Kategorien")](advanced-user-notifications-images/customui03.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![](advanced-user-notifications-images/customui03w.png "Benachrichtigung Erweiterung Inhalt Kategorien")](advanced-user-notifications-images/customui03w.png)

-----

### <a name="hiding-the-default-notification-content"></a>Ausblenden von den Standardinhalt der Benachrichtigung

In der Situation, in dem die Benutzeroberfläche des benutzerdefinierten Benachrichtigung wird werden Anzeigen von den gleichen Inhalt als Standard-Benachrichtigung (Titel, einen Untertitel und Text am unteren Rand der Benachrichtigung-Benutzeroberfläche automatisch angezeigt), kann diese Standardeinstellung Informationen durch Hinzufügen der ausgeblendetwerden`UNNotificationExtensionDefaultContentHidden`um die `NSExtensionAttributes` als Typ Key **booleschen** mit einem Wert von `YES` in der Erweiterungs `Info.plist` Datei:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[ ![](advanced-user-notifications-images/customui04.png "Suchen von Standardinformationen")](advanced-user-notifications-images/customui04.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![](advanced-user-notifications-images/customui04w.png "Suchen von Standardinformationen")](advanced-user-notifications-images/customui04w.png)

-----

### <a name="designing-the-custom-ui"></a>Entwerfen der benutzerdefinierten Benutzeroberflächenautomatisierungs

Um die Benachrichtigung Content Erweiterung benutzerdefinierte Oberfläche zu entwerfen, doppelklicken Sie auf die `MainInterface.storyboard` Datei zur Bearbeitung in der iOS-Designer öffnen zu ziehen, in den Elementen, die Sie benötigen, um die gewünschte Schnittstelle zu erstellen (z. B. `UILabels` und `UIImageViews`).

> [!NOTE]
> **Hinweis:** wird die Benutzeroberfläche für die Benachrichtigung _nicht_ interaktive Steuerelementen wie z. B. Textfelder oder Schaltflächen in einer Benachrichtigung Content-Erweiterung zu unterstützen. Während sie das Storyboard hinzugefügt werden können, wird der Benutzer nicht mit ihnen interagieren können. Um eine benutzerdefinierte Benutzeroberfläche für die Benachrichtigung Benutzerinteraktion hinzuzufügen, verwenden Sie stattdessen benutzerdefinierte Aktionen.

Öffnen Sie nach die Benutzeroberfläche angeordnet, und die erforderlichen Steuerelemente, die für C#-Code verfügbar gemacht, die `NotificationViewController.cs` zum Bearbeiten und ändern die `DidReceiveNotification` Methode, um die Benutzeroberfläche zu füllen, wenn der Benutzer die Benachrichtigung erweitert. Zum Beispiel:

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

Der folgende Code ist zum Anpassen der Größe des Inhaltsbereichs, die dem Benutzer angezeigten Einstellung der `PreferredContentSize` Eigenschaft in der `ViewDidLoad` Methode, um die gewünschte Größe. Diese Größe kann auch durch Anwenden von Einschränkungen auf die Ansicht in der iOS-Designer angepasst werden, es obliegt dem Entwickler, die Methode auswählen, die für sie am besten geeignet ist.

Da die Benachrichtigung System bereits, vor der Erweiterung Inhalt aufgerufen wird, Benachrichtigung Inhaltsbereichs ausgeführt wird fangen Größe vollständige Installation und animiert werden, auf die angeforderte Größe, wenn dem Benutzer angezeigt.

Um diesen Effekt zu vermeiden, bearbeiten die `Info.plist` -Datei für die Erweiterung, und legen die `UNNotificationExtensionInitialContentSizeRatio` -Schlüssel des der `NSExtensionAttributes` Schlüssel eingeben **Anzahl** mit einem Wert, der das gewünschte Verhältnis darstellt. Zum Beispiel:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[ ![](advanced-user-notifications-images/customui05.png "Der Schlüssel UNNotificationExtensionInitialContentSizeRatio")](advanced-user-notifications-images/customui05.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![](advanced-user-notifications-images/customui05w.png "Der Schlüssel UNNotificationExtensionInitialContentSizeRatio")](advanced-user-notifications-images/customui05w.png)

-----

### <a name="using-media-attachments-in-custom-ui"></a>Verwenden von Medien Anlagen in benutzerdefinierten Benutzeroberfläche

Da Media Anlagen (wie in der [Hinzufügen von Anlagen](#Adding-Media-Attachments) obigen Abschnitt "") sind Teil der Benachrichtigungsnutzlast, können sie Zugriff auf und in die Erweiterung für die Inhalt der Benachrichtigung angezeigt, wie sie in der Standardeinstellung werden würde Benachrichtigung über die Benutzeroberfläche.

Angenommen, die benutzerdefinierte Benutzeroberfläche, die oben genannten enthalten eine `UIImageView` , die für C#-Code verfügbar gemacht wurde, konnte der folgende Code aus mit der Media-Anlage Auffüllung verwendet werden:

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

Da die Anlage Medien vom System verwaltet wird, ist es außerhalb der app-Sandkasten. Die Erweiterung muss dem System zu informieren, dass sie Zugriff auf die Datei durch Aufrufen möchte die `StartAccessingSecurityScopedResource` Methode. Wenn die Erweiterung mit der Datei abgeschlossen ist, muss Sie zum Aufrufen der `StopAccessingSecurityScopedResource` um die Verbindung freizugeben.

### <a name="adding-custom-actions-to-a-custom-ui"></a>Hinzufügen von benutzerdefinierten Aktionen zu einer benutzerdefinierten Benutzeroberfläche

Wie bereits erwähnt, unterstützt die Benutzeroberfläche für die Benachrichtigung keine Benutzerinteraktion, daher Steuerelemente, z. B. Textfelder oder Schaltflächen in das Design der Benutzeroberfläche nicht unterstützt werden.

Stattdessen wird der benutzerdefinierte Standardaktionen Mechanismus verwendet, um eine benutzerdefinierte Benutzeroberfläche für die Benachrichtigung Interaktivität hinzuzufügen. Finden Sie unter der [arbeiten mit Benachrichtigungsaktionen](~/ios/platform/user-notifications/enhanced-user-notifications.md) Teil der [Benutzerbenachrichtigungen erweiterte](~/ios/platform/user-notifications/enhanced-user-notifications.md) Dokument detaillierte Informationen zu benutzerdefinierten Aktionen.

Zusätzlich zu benutzerdefinierten Aktionen können die Erweiterung für die Inhalt der Benachrichtigung für die folgenden integrierten Aktionen auch Antworten:

- **Standardaktion** -Dies ist beim Tippen auf einer benachrichtigungs an die app zu öffnen und die Details der angegebenen Benachrichtigung angezeigt.
- **Schließen Sie die Aktion** -diese Aktion wird für die app gesendet, wenn der Benutzer eine bestimmte Benachrichtigung schließt.

Benachrichtigung Content Erweiterungen haben auch die Möglichkeit, ihre Benutzeroberfläche zu aktualisieren, wenn der Benutzer eine benutzerdefinierte Aktionen aufruft, z. B. das Anzeigen eines Datums als akzeptiert, wenn der Benutzer tippt der **Accept** Custom Action-Schaltfläche. Darüber hinaus können die Benachrichtigung Content Erweiterungen feststellen, das System für die Kündigung der Benachrichtigung Benutzeroberfläche zu verzögern, damit der Benutzer die Auswirkung ihre Aktion sehen kann, bevor die Benachrichtigung geschlossen wird.

Dies erfolgt durch eine zweite Version der Implementierung der `DidReceiveNotification` Methode, die eine Abschlusshandler enthält. Zum Beispiel:

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

Durch Hinzufügen der `Server.PostEventResponse` -Handler die `DidReceiveNotification` -Methode Content Benachrichtigung-Erweiterung, die Erweiterung *müssen* behandeln alle benutzerdefinierte Aktionen. Die Erweiterung kann auch die benutzerdefinierten Aktionen auf die enthaltende Anwendung weiterleiten, durch Ändern der `UNNotificationContentExtensionResponseOption`. Zum Beispiel:

```csharp
// Close Notification
completionHandler (UNNotificationContentExtensionResponseOption.DismissAndForwardAction);
```

### <a name="working-with-the-text-input-action-in-custom-ui"></a>Arbeiten mit dem Text Eingabeaktion in benutzerdefinierten Benutzeroberfläche

Je nach Entwurf der app und die Benachrichtigung es gibt möglicherweise Fälle, in denen der Benutzer zur Eingabe von Text in der Benachrichtigung (z. B. auf eine Nachricht Antworten). Eine Erweiterung der Benachrichtigung Inhalt hat Zugriff auf die integrierte Text Eingabeaktion, ähnlich wie eine standard-Benachrichtigung.

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

Dieser Code erstellt einen neuen Text Eingabeaktion und fügt es die Erweiterung Kategorie hinzu (in der `MakeExtensionCategory`) Methode. In der `DidReceive` Überschreibungsmethode, verarbeitet der Eingabe von Text mit den folgenden Code:

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

Entwurf, die für das Hinzufügen von benutzerdefinierten Schaltflächen in das Textfeld für die Eingabe aufgerufen werden, fügen Sie den folgenden Code aus, um diese aufzunehmen:

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

Wenn die Aktion Kommentar vom Benutzer ausgelöst wird, müssen sowohl die View-Controller und das Eingabefeld für benutzerdefinierten Text aktiviert werden:

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

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat einen erweiterte Einblick in die Verwendung von neuen Benutzerbenachrichtigung-Frameworks in einer app Xamarin.iOS übernommen. Es fallen Medien Anlagen von lokalen und Remote-Benachrichtigung hinzufügen und abgedeckt werden mithilfe der Benutzeroberfläche für neue Benutzer Benachrichtigung Benutzeroberflächen für benutzerdefinierte Benachrichtigung zu erstellen.


## <a name="related-links"></a>Verwandte Links

- [iOS-10-Beispiele](https://developer.xamarin.com/samples/ios/iOS10/)
- [UserNotifications Frameworkverweis](https://developer.apple.com/reference/usernotifications)
- [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui)
- [Programmierhandbuch für lokale und Remote-Benachrichtigung](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/Introduction.html)
