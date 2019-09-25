---
title: Erweiterte Benutzer Benachrichtigungen in xamarin. IOS
description: Dieser Artikel bietet einen tieferen Einblick in das Benutzer Benachrichtigungs Framework, das in ios 10 eingeführt wird. Er erläutert Benutzer Benachrichtigungen, die Benutzer Benachrichtigungs Schnittstelle, Medien Anlagen, benutzerdefinierte Benutzeroberflächen usw.
ms.prod: xamarin
ms.assetid: 4E0C60AE-6F54-4098-8FA0-AADF9AC86805
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 05/03/2018
ms.openlocfilehash: cd6458b7d27a50744839fff57b4031943193d7f7
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71250101"
---
# <a name="advanced-user-notifications-in-xamarinios"></a>Erweiterte Benutzer Benachrichtigungen in xamarin. IOS

Das Benutzer Benachrichtigungs Framework ist neu bei IOS 10 und ermöglicht die Übermittlung und Handhabung von lokalen Benachrichtigungen und Remote Benachrichtigungen. Mithilfe dieses Frameworks kann eine APP-oder App-Erweiterung die Übermittlung lokaler Benachrichtigungen planen, indem Sie einen Satz von Bedingungen angibt, wie z. b. den Standort oder die Tageszeit.

## <a name="about-user-notifications"></a>Informationen zu Benutzer Benachrichtigungen

Das neue Benutzer Benachrichtigungs Framework ermöglicht die Übermittlung und Behandlung von lokalen Benachrichtigungen und Remote Benachrichtigungen. Mithilfe dieses Frameworks kann eine APP-oder App-Erweiterung die Übermittlung lokaler Benachrichtigungen planen, indem Sie einen Satz von Bedingungen angibt, wie z. b. den Standort oder die Tageszeit.

Darüber hinaus kann die APP oder Erweiterung lokale Benachrichtigungen und Remote Benachrichtigungen empfangen (und potenziell ändern), wenn Sie an das IOS-Gerät des Benutzers übermittelt werden.

Mit dem neuen Benutzeroberfläche-Framework für Benutzer Benachrichtigungen kann eine APP-oder App-Erweiterung die Darstellung von lokalen Benachrichtigungen und Remote Benachrichtigungen anpassen, wenn Sie dem Benutzer angezeigt werden.

Dieses Framework bietet die folgenden Möglichkeiten, wie eine APP Benachrichtigungen an einen Benutzer übermitteln kann:

- **Visuelle Warnungen** : Hier wird die Benachrichtigung vom oberen Bildschirmrand aus als Banner angezeigt.
- **Sound und Vibrationen** : können mit einer Benachrichtigung verknüpft werden.
- **App-Symbol Badging** : das Symbol der APP zeigt einen Badge an, der anzeigt, dass neuer Inhalt verfügbar ist. Beispielsweise die Anzahl ungelesener e-Mail-Nachrichten.

Außerdem gibt es je nach aktuellem Kontext des Benutzers verschiedene Möglichkeiten, wie eine Benachrichtigung angezeigt wird:

- Wenn das Gerät entsperrt ist, wird die Benachrichtigung vom oberen Bildschirmrand aus als Banner angezeigt.
- Wenn das Gerät gesperrt ist, wird die Benachrichtigung auf dem Sperrbildschirm des Benutzers angezeigt.
- Wenn der Benutzer eine Benachrichtigung verpasst hat, kann er das Benachrichtigungs Center öffnen und alle verfügbaren Warnungs Benachrichtigungen dort anzeigen.

Eine xamarin. IOS-App verfügt über zwei Arten von Benutzer Benachrichtigungen, die Sie senden kann:

- **Lokale Benachrichtigungen** : Diese werden von apps gesendet, die lokal auf dem Gerät des Benutzers installiert werden.
- **Remote Benachrichtigungen** : werden von einem Remote Server gesendet und dem Benutzer angezeigt, oder es wird ein Hintergrund Update des App-Inhalts ausgelöst.

Weitere Informationen finden Sie in unserer Dokumentation zu [erweiterten Benutzer Benachrichtigungen](~/ios/platform/user-notifications/enhanced-user-notifications.md) .

## <a name="the-new-user-notification-interface"></a>Die neue Benutzer Benachrichtigungs Schnittstelle

Benutzer Benachrichtigungen in ios 10 werden mit einem neuen Design der Benutzeroberfläche angezeigt, das weitere Inhalte bietet, z. b. Titel, Untertitel und optionale Medien Anlagen, die auf dem Sperrbildschirm angezeigt werden können, als Banner am oberen Rand des Geräts oder im Benachrichtigungs Center.

Unabhängig davon, wo eine Benutzer Benachrichtigung in ios 10 angezeigt wird, wird Ihnen dasselbe Aussehen und Gefühl und dieselben Features und Funktionen angezeigt.

In ios 8 hat Apple Aktions fähige Benachrichtigungen eingeführt, bei denen der Entwickler benutzerdefinierte Aktionen an eine Benachrichtigung anfügen und dem Benutzer die Möglichkeit geben kann, eine Benachrichtigung zu übernehmen, ohne die app starten zu müssen. In ios 9 hat Apple Aktions fähige Benachrichtigungen mit Quick Reply verbessert, sodass der Benutzer auf eine Benachrichtigung mit Texteingabe Antworten kann.

Da Benutzer Benachrichtigungen ein wesentlicher Bestandteil der Benutzeroberfläche in ios 10 sind, hat Apple die umsetzbaren Benachrichtigungen erweitert, um die 3D-Toucheingabe zu unterstützen, wobei der Benutzer auf eine Benachrichtigung drückt und eine benutzerdefinierte Benutzeroberfläche angezeigt wird, um umfangreiche Interaktionen zu ermöglichen. mit der Benachrichtigung.

Wenn die Benutzeroberfläche für benutzerdefinierte Benutzer Benachrichtigungen angezeigt wird und der Benutzer mit Aktionen interagiert, die an die Benachrichtigung angefügt sind, kann die benutzerdefinierte Benutzeroberfläche sofort aktualisiert werden, um Feedback zu den geänderten Änderungen bereitzustellen.

Neu bei IOS 10: mit der UI-API für Benutzer Benachrichtigungen kann eine xamarin. IOS-APP die neuen Benutzeroberflächen Features von Benutzer Benachrichtigungen problemlos nutzen.

## <a name="adding-media-attachments"></a>Hinzufügen von Medien Anlagen

Eines der gängigeren Elemente, die von Benutzern gemeinsam genutzt werden, sind Fotos. IOS 10 hat die Möglichkeit hinzugefügt, eine Medien Aufgabe (z. b. ein Foto) direkt an eine Benachrichtigung anzufügen, wo Sie dem Benutzer angezeigt wird und wie der Rest des Conte der Benachrichtigung steht. Eis.

Aufgrund der Größen, die beim Senden eines kleinen Bilds anfallen, ist das Anfügen an eine Remote Benachrichtigungs Nutzlast jedoch unpraktisch. Um diese Situation zu beheben, kann der Entwickler die neue Dienst Erweiterung in ios 10 verwenden, um das Image aus einer anderen Quelle (z. b. einem cloudkit-Datenspeicher) herunterzuladen und es an den Inhalt der Benachrichtigung anzufügen, bevor es dem Benutzer angezeigt wird.

Damit eine Remote Benachrichtigung von einer Dienst Erweiterung geändert werden kann, muss die zugehörige Nutzlast als änderbar gekennzeichnet werden. Beispiel:

```csharp
{
    aps : {
        alert : "New Photo Available",
        mutable-content: 1
    },
    my-attachment : "https://example.com/photo.jpg"
}
```

Sehen Sie sich die folgende Übersicht über den Prozess an:

[![](advanced-user-notifications-images/extension02.png "Hinzufügen von Medien Anlagen Prozess")](advanced-user-notifications-images/extension02.png#lightbox)

Nachdem die Remote Benachrichtigung an das Gerät übermittelt wurde (über APNs), kann die Dienst Erweiterung das erforderliche Image über eine beliebige gewünschte Methode herunterladen (z `NSURLSession`. b. ein), und nachdem das Image empfangen wurde, kann es den Inhalt der Benachrichtigung und der Anzeige ändern. für den Benutzer.

Im folgenden finden Sie ein Beispiel dafür, wie dieser Prozess im Code verarbeitet werden kann:

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

Wenn die Benachrichtigung von APNs empfangen wird, wird die benutzerdefinierte Adresse des Images aus dem Inhalt gelesen, und die Datei wird vom Server heruntergeladen. Anschließend wird `UNNotificationAttachement` eine mit einer eindeutigen ID und dem lokalen Speicherort des Bilds ( `NSUrl`als) erstellt. Eine änderbare Kopie des Benachrichtigungs Inhalts wird erstellt, und Medien Anlagen werden hinzugefügt. Schließlich wird die Benachrichtigung für den Benutzer angezeigt, indem aufgerufen `contentHandler`wird.

Nachdem einer Benachrichtigung eine Anlage hinzugefügt wurde, übernimmt das System die Verschiebung und Verwaltung der Datei.

Zusätzlich zu den oben dargestellten Remote Benachrichtigungen werden Medien Anlagen auch von lokalen Benachrichtigungen unterstützt, bei denen das `UNNotificationAttachement` erstellt und zusammen mit dem Inhalt an die Benachrichtigung angefügt wird.

Die Benachrichtigung in ios 10 unterstützt Medien Anlagen von Bildern (statisch und GIFs), Audiodaten und Videos, und das System zeigt automatisch die korrekte benutzerdefinierte Benutzeroberfläche für diese Art von Anlagen an, wenn dem Benutzer die Benachrichtigung angezeigt wird.

> [!NOTE]
> Beachten Sie, dass sowohl die Mediengröße als auch der Zeitaufwand für das Herunterladen der Medien vom Remote Server (oder das Zusammenstellen der Medien für lokale Benachrichtigungen) optimiert werden muss, da das System beim Ausführen der Dienst Erweiterung der APP strikte Beschränkungen für beide festlegt. Sie können beispielsweise eine herunter skalierte Version des Images oder einen kleinen Clip eines Videos senden, das in der Benachrichtigung angezeigt werden soll.

## <a name="creating-custom-user-interfaces"></a>Erstellen von benutzerdefinierten Benutzeroberflächen

Um eine benutzerdefinierte Benutzeroberfläche für die Benutzer Benachrichtigungen zu erstellen, muss der Entwickler der APP-Lösung eine Erweiterung für Benachrichtigungs Inhalte (neu bei IOS 10) hinzufügen.

Die Erweiterung für Benachrichtigungs Inhalte ermöglicht es dem Entwickler, eigene Ansichten zur Benachrichtigungs Benutzeroberfläche hinzuzufügen und alle gewünschten Inhalte zu zeichnen. Ab IOS 12 unterstützen Erweiterungen für Benachrichtigungs Inhalte interaktive UI-Steuerelemente wie Schaltflächen und Schieberegler. Weitere Informationen finden Sie in der Dokumentation zu [interaktiven Benachrichtigungen in IOS 12](~/ios/platform/introduction-to-ios12/notifications/interactive.md) .

Zur Unterstützung der Benutzerinteraktion mit einer Benutzer Benachrichtigung sollten benutzerdefinierte Aktionen erstellt, beim System registriert und an die Benachrichtigung angehängt werden, bevor Sie mit dem System geplant werden. Die Erweiterung für Benachrichtigungs Inhalte wird aufgerufen, um die Verarbeitung dieser Aktionen zu verarbeiten. Weitere Informationen zu benutzerdefinierten Aktionen finden Sie im Abschnitt [Arbeiten mit Benachrichtigungs Aktionen](~/ios/platform/user-notifications/enhanced-user-notifications.md) des Dokuments " [Erweiterte Benutzer Benachrichtigungen](~/ios/platform/user-notifications/enhanced-user-notifications.md) ".

Wenn Benutzern eine Benutzer Benachrichtigung mit einer benutzerdefinierten Benutzeroberfläche angezeigt wird, verfügen Sie über die folgenden Elemente:

[![](advanced-user-notifications-images/customui01.png "Eine Benutzer Benachrichtigung mit benutzerdefinierten Benutzeroberflächen Elementen")](advanced-user-notifications-images/customui01.png#lightbox)

Wenn der Benutzer mit den benutzerdefinierten Aktionen interagiert (die unter der Benachrichtigung angezeigt werden), kann die Benutzeroberfläche aktualisiert werden, um dem Benutzer Feedback zu geben, was geschehen ist, als eine bestimmte Aktion aufgerufen wurde.

### <a name="adding-a-notification-content-extension"></a>Hinzufügen einer Erweiterung für Benachrichtigungs Inhalte

Gehen Sie folgendermaßen vor, um eine benutzerdefinierte Benutzeroberfläche für Benutzer Benachrichtigungen in einer xamarin. IOS-APP zu implementieren:

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Öffnen Sie die Projekt Mappe der app in Visual Studio für Mac.
2. Klicken Sie im **Lösungspad** mit der rechten Maustaste auf den Projektmappennamen, und wählen Sie **hinzu** > fügen**Neues Projekt**hinzufügen
3. Wählen Sie **IOS** > **Extensions** > **Benachrichtigungs Inhalts Erweiterungen** aus, und klicken Sie auf **weiter** : 

    [![](advanced-user-notifications-images/notify01.png "Erweiterungen für Benachrichtigungs Inhalt auswählen")](advanced-user-notifications-images/notify01.png#lightbox)
4. Geben Sie einen **Namen** für die Erweiterung ein, und klicken Sie auf die Schaltfläche **weiter** : 

    [![](advanced-user-notifications-images/notify02.png "Geben Sie einen Namen für die Erweiterung ein.")](advanced-user-notifications-images/notify02.png#lightbox)
5. Passen Sie den **Projektnamen** und/oder Projektmappennamen bei Bedarf an, und klicken Sie auf die Schaltfläche **Erstellen** 

    [![](advanced-user-notifications-images/notify03.png "Anpassen des Projekt namens und/oder Projektmappennamens")](advanced-user-notifications-images/notify03.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Öffnen Sie die Projekt Mappe der app in Visual Studio für Mac.
2. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektmappennamen, und wählen Sie **> Neues Projekt hinzufügen...** aus.
3. Wählen **Sie C# Visual > IOS-Erweiterungen > Erweiterung für Benachrichtigungs Inhalt**aus:

    [![](advanced-user-notifications-images/notify01.w157-sml.png "Erweiterungen für Benachrichtigungs Inhalt auswählen")](advanced-user-notifications-images/notify01.w157.png#lightbox)
4. Geben Sie einen **Namen** für die Erweiterung ein, und klicken Sie auf die Schaltfläche **OK** .

-----

Wenn die Erweiterung für Benachrichtigungs Inhalte der Projekt Mappe hinzugefügt wird, werden im Projekt der Erweiterung drei Dateien erstellt:

1. `NotificationViewController.cs`: Dies ist der Haupt Ansichts Controller für die Erweiterung für Benachrichtigungs Inhalte.
2. `MainInterface.storyboard`: Der Entwickler legt die sichtbare Benutzeroberfläche für die Erweiterung für Benachrichtigungs Inhalte im IOS-Designer fest.
3. `Info.plist`-Steuert die Konfiguration der Erweiterung für Benachrichtigungs Inhalte.

Die Standard `NotificationViewController.cs` Datei sieht wie folgt aus:

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

Die `DidReceiveNotification` -Methode wird aufgerufen, wenn die Benachrichtigung vom Benutzer erweitert wird, sodass die Erweiterung `UNNotification`für Benachrichtigungs Inhalte die benutzerdefinierte Benutzeroberfläche mit dem Inhalt des auffüllen kann. Im obigen Beispiel wurde der Sicht eine Bezeichnung hinzugefügt, die für Code mit dem Namen `label` verfügbar gemacht wird und zum Anzeigen des Text der Benachrichtigung verwendet wird.

### <a name="setting-the-notification-content-extensions-categories"></a>Festlegen der Kategorien von Benachrichtigungs Inhalts Erweiterungen

Das System muss darüber informiert werden, wie die Benachrichtigungs Inhalts Erweiterung der App basierend auf den spezifischen Kategorien gefunden werden kann, auf die es antwortet. Führen Sie folgende Schritte aus:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Doppelklicken Sie auf die `Info.plist` Datei der Erweiterung im **Lösungspad** , um Sie für die Bearbeitung zu öffnen.
2. Wechseln Sie zur **Quell** Ansicht.
3. Erweitern Sie `NSExtension` den Schlüssel.
4. Fügen Sie `UNNotificationExtensionCategory` den Schlüssel als Type- **Zeichenfolge** mit dem Wert der Kategorie hinzu, zu der die Erweiterung gehört (in diesem Beispiel "Event-INVITE"): 

    [![](advanced-user-notifications-images/customui02.png "Fügen Sie den unnotificationextensioncategory-Schlüssel hinzu.")](advanced-user-notifications-images/customui02.png#lightbox)
5. Speichern Sie die Änderungen.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Doppelklicken Sie auf die `Info.plist` Datei der Erweiterung im **Projektmappen-Explorer** , um Sie für die Bearbeitung zu öffnen.
2. Erweitern Sie `NSExtension` den Schlüssel.
3. Fügen Sie `UNNotificationExtensionCategory` den Schlüssel als Type- **Zeichenfolge** mit dem Wert der Kategorie hinzu, zu der die Erweiterung gehört (in diesem Beispiel "Event-INVITE"): 

    [![](advanced-user-notifications-images/customui02w.png "Fügen Sie den unnotificationextensioncategory-Schlüssel hinzu.")](advanced-user-notifications-images/customui02w.png#lightbox)
4. Speichern Sie die Änderungen.

-----

Erweiterungs Kategorien für Benachrichtigungs`UNNotificationExtensionCategory`Inhalte () verwenden die gleichen Kategoriewerte, die zum Registrieren von Benachrichtigungs Aktionen verwendet werden. Wechseln `UNNotificationExtensionCategory` Sie in der Situation, in der die APP dieselbe Benutzeroberfläche für mehrere Kategorien verwendet, in das **typanray** , und stellen Sie alle erforderlichen Kategorien bereit. Beispiel:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![](advanced-user-notifications-images/customui03.png "Kategorien von Benachrichtigungs Inhalts Erweiterungen")](advanced-user-notifications-images/customui03.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](advanced-user-notifications-images/customui03w.png "Kategorien von Benachrichtigungs Inhalts Erweiterungen")](advanced-user-notifications-images/customui03w.png#lightbox)

-----

### <a name="hiding-the-default-notification-content"></a>Ausblenden des Standard Benachrichtigungs Inhalts

Wenn die benutzerdefinierte Benachrichtigungs-UI denselben Inhalt wie die Standard Benachrichtigung anzeigt (Titel, Untertitel und Text werden automatisch am unteren Rand der Benachrichtigungs-UI angezeigt), können Sie diese Standardinformationen ausblenden, indem Sie das `UNNotificationExtensionDefaultContentHidden`Schlüssel für den `NSExtensionAttributes` Schlüssel als Typ **boolescher** Wert mit dem Wert `YES` in der `Info.plist` Datei der Erweiterung:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![](advanced-user-notifications-images/customui04.png "Suchen nach Standardinformationen")](advanced-user-notifications-images/customui04.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](advanced-user-notifications-images/customui04w.png "Suchen nach Standardinformationen")](advanced-user-notifications-images/customui04w.png#lightbox)

-----

### <a name="designing-the-custom-ui"></a>Entwerfen der benutzerdefinierten Benutzeroberfläche

Wenn Sie die benutzerdefinierte Benutzeroberfläche der Benachrichtigungs Inhalts Erweiterung entwerfen möchten, `MainInterface.storyboard` Doppelklicken Sie auf die Datei, um Sie im IOS-Designer zur Bearbeitung zu öffnen, und ziehen Sie die Elemente, die Sie `UILabels` zum `UIImageViews`Erstellen der gewünschten Schnittstelle benötigen (z. b. und).

> [!NOTE]
> Ab IOS 12 kann eine Erweiterung für Benachrichtigungs Inhalte interaktive Steuerelemente enthalten, z. b. Schaltflächen und Textfelder. Weitere Informationen finden Sie in der Dokumentation zu [interaktiven Benachrichtigungen in IOS 12](~/ios/platform/introduction-to-ios12/notifications/interactive.md) .

Nachdem Sie die Benutzeroberfläche angelegt und die erforderlichen Steuerelemente für C# Code verfügbar gemacht haben, `NotificationViewController.cs` öffnen Sie die für die `DidReceiveNotification` Bearbeitung, und ändern Sie die Methode, um die Benutzeroberfläche aufzufüllen, wenn der Benutzer die Benachrichtigung erweitert. Beispiel:

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

### <a name="setting-the-content-area-size"></a>Festlegen der Größe des Inhalts Bereichs

Zum Anpassen der Größe des Inhalts Bereichs, der dem Benutzer angezeigt wird, wird im folgenden Code die `PreferredContentSize` -Eigenschaft in `ViewDidLoad` der-Methode auf die gewünschte Größe festgelegt. Diese Größe kann auch durch Anwenden von Einschränkungen auf die Ansicht im IOS-Designer angepasst werden. Sie wird dem Entwickler überlassen, die Methode auszuwählen, die am besten für Sie geeignet ist.

Da das Benachrichtigungssystem bereits ausgeführt wird, bevor die Erweiterung für Benachrichtigungs Inhalte aufgerufen wird, wird der Inhalts Bereich vollständig gestartet und auf die angeforderte Größe animiert, wenn er dem Benutzer angezeigt wird.

Um diesen Effekt auszuschließen, bearbeiten Sie `Info.plist` die Datei für die Erweiterung, und `UNNotificationExtensionInitialContentSizeRatio` legen Sie den `NSExtensionAttributes` Schlüssel des Schlüssels auf Type **Number** mit einem Wert fest, der das gewünschte Verhältnis darstellt. Beispiel:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![](advanced-user-notifications-images/customui05.png "Der unnotificationextensioninitialcontentsizeratio-Schlüssel")](advanced-user-notifications-images/customui05.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](advanced-user-notifications-images/customui05w.png "Der unnotificationextensioninitialcontentsizeratio-Schlüssel")](advanced-user-notifications-images/customui05w.png#lightbox)

-----

### <a name="using-media-attachments-in-custom-ui"></a>Verwenden von Medien Anlagen in der benutzerdefinierten Benutzeroberfläche

Da Medien Anlagen (wie im Abschnitt [Hinzufügen von Medien Anlagen](#adding-media-attachments) oben) Teil der Benachrichtigungs Nutzlast sind, können Sie auf Sie zugreifen und in der Erweiterung für Benachrichtigungs Inhalte angezeigt werden, so wie Sie in der standardmäßigen Benachrichtigungs-UI angezeigt werden.

Wenn z. b. die oben genannte benutzerdefinierte `UIImageView` Benutzeroberfläche eine enthält C# , die für Code verfügbar gemacht wurde, kann der folgende Code verwendet werden, um Sie von mit der Medien Anlage aufzufüllen:

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

Da die Medien Anlage vom System verwaltet wird, befindet Sie sich außerhalb des Sandkastens der app. Die Erweiterung muss das System darüber informieren, dass der Zugriff auf die Datei durch Aufrufen der `StartAccessingSecurityScopedResource` -Methode erforderlich ist. Wenn die Erweiterung mit der Datei abgeschlossen ist, muss sie aufgerufen `StopAccessingSecurityScopedResource` werden, um die Verbindung freizugeben.

### <a name="adding-custom-actions-to-a-custom-ui"></a>Hinzufügen von benutzerdefinierten Aktionen zu einer benutzerdefinierten Benutzeroberfläche

Mithilfe von benutzerdefinierten Aktions Schaltflächen können Sie einer benutzerdefinierten Benachrichtigungs-UI Interaktivität hinzufügen. Weitere Informationen zu benutzerdefinierten Aktionen finden Sie im Abschnitt [Arbeiten mit Benachrichtigungs Aktionen](~/ios/platform/user-notifications/enhanced-user-notifications.md) des Dokuments " [Erweiterte Benutzer Benachrichtigungen](~/ios/platform/user-notifications/enhanced-user-notifications.md) ".

Zusätzlich zu den benutzerdefinierten Aktionen kann die Erweiterung für Benachrichtigungs Inhalte auch auf die folgenden integrierten Aktionen reagieren:

- **Standardaktion** : Dies ist der Fall, wenn der Benutzer auf eine Benachrichtigung tippt, um die APP zu öffnen und die Details der angegebenen Benachrichtigung anzuzeigen.
- **Aktion verwerfen** : Diese Aktion wird an die APP gesendet, wenn der Benutzer eine bestimmte Benachrichtigung ablehnt.

Erweiterungen für Benachrichtigungs Inhalte können auch Ihre Benutzeroberfläche aktualisieren, wenn der Benutzer eine der benutzerdefinierten Aktionen aufruft, z. b. Wenn ein Datum als akzeptiert angezeigt wird, wenn der Benutzer auf die Schaltfläche "benutzerdefinierte Aktion **annehmen** " tippt. Darüber hinaus können die Erweiterungen für Benachrichtigungs Inhalte dem System mitteilen, dass die Kündigung der Benachrichtigungs-UI verzögert wird, damit der Benutzer die Auswirkungen seiner Aktion sehen kann, bevor die Benachrichtigung geschlossen wird.

Dies erfolgt durch Implementieren einer zweiten Version der `DidReceiveNotification` -Methode, die einen Abschluss Handler enthält. Beispiel:

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

Durch Hinzufügen `Server.PostEventResponse` des Handlers `DidReceiveNotification` zur-Methode der Erweiterung für Benachrichtigungs Inhalte *muss* die Erweiterung alle benutzerdefinierten Aktionen verarbeiten. Die Erweiterung kann die benutzerdefinierten Aktionen auch an die enthaltende App weiterleiten, `UNNotificationContentExtensionResponseOption`indem Sie den ändern. Beispiel:

```csharp
// Close Notification
completionHandler (UNNotificationContentExtensionResponseOption.DismissAndForwardAction);
```

### <a name="working-with-the-text-input-action-in-custom-ui"></a>Arbeiten mit der Texteingabe Aktion in der benutzerdefinierten Benutzeroberfläche

Abhängig vom Entwurf der APP und der Benachrichtigung kann es vorkommen, dass der Benutzer Text in die Benachrichtigung eingeben muss (z. b. das Antworten auf eine Nachricht). Eine Erweiterung für Benachrichtigungs Inhalte kann wie bei einer Standard Benachrichtigung auf die integrierte Texteingabe Aktion zugreifen.

Beispiel:

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

Mit diesem Code wird eine neue Texteingabe Aktion erstellt und der Kategorie der Erweiterung (in der `MakeExtensionCategory`-Methode) hinzugefügt. In der `DidReceive` Überschreibungs Methode verarbeitet Sie den Benutzer, der Text mit folgendem Code eingibt:

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

Wenn der Entwurf aufruft, um dem Text Eingabefeld benutzerdefinierte Schaltflächen hinzuzufügen, fügen Sie den folgenden Code hinzu, um Sie einzuschließen:

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

Wenn die Kommentar Aktion vom Benutzer ausgelöst wird, müssen sowohl der Ansichts Controller als auch das benutzerdefinierte Texteingabefeld aktiviert werden:

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

In diesem Artikel wurde eine erweiterte Betrachtung der Verwendung des neuen Benutzer Benachrichtigungs Frameworks in einer xamarin. IOS-App erläutert. Es umfasste das Hinzufügen von Medien Anlagen sowohl zur lokalen als auch zur Remote Benachrichtigung und wird mithilfe der Benutzeroberfläche für Benutzer Benachrichtigungen zum Erstellen benutzerdefinierter Benachrichtigungs Benutzeroberflächen abgedeckt.

## <a name="related-links"></a>Verwandte Links

- [IOS 10-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
- [User Notification Framework-Referenz](https://developer.apple.com/reference/usernotifications)
- [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui)
- [Leitfaden zur lokalen und Remote Benachrichtigungs Programmierung](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/Introduction.html)
