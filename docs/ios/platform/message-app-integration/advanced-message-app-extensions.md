---
title: Erweiterte Nachrichten-App-Erweiterungen
description: In diesem Artikel erfahren erweiterte Techniken zum Arbeiten mit Nachricht-App-Erweiterungen in einem Xamarin.iOS-Lösung, die Nachrichten-app integriert und stellt neue Funktionen für den Benutzer.
ms.prod: xamarin
ms.assetid: 394A1FDA-AF70-4493-9B2C-4CFE4BE791B6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: cd2cabf98c83bba7502e8533e482713a9c43f67a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="advanced-message-app-extensions"></a>Erweiterte Nachrichten-App-Erweiterungen

_In diesem Artikel erfahren erweiterte Techniken zum Arbeiten mit Nachricht-App-Erweiterungen in einem Xamarin.iOS-Lösung, die Nachrichten-app integriert und stellt neue Funktionen für den Benutzer._


Neu für iOS-10-App-Erweiterung einer Nachricht integriert die **Nachrichten** app und stellt neue Funktionen für den Benutzer. Die Erweiterung kann Text, Aufkleber, Mediendateien und interaktive Nachrichten senden.

## <a name="about-message-app-extensions"></a>Informationen zu Nachrichten-App-Erweiterungen

Wie bereits erwähnt, eine Nachrichten-App-Erweiterung integriert die **Nachrichten** app und stellt neue Funktionen für den Benutzer. Die Erweiterung kann Text, Aufkleber, Mediendateien und interaktive Nachrichten senden. Zwei Arten von App-Erweiterung der Nachricht sind verfügbar:

- **Der Aufkleber mit Packs** -enthält eine Auflistung von Aufkleber, die der Benutzer eine Nachricht hinzufügen kann. Der Aufkleber mit Packs können erstellt werden, ohne Code schreiben zu müssen.
- **iMessage App** -kann eine benutzerdefinierte Benutzeroberfläche innerhalb der Nachrichten-app für Aufkleber auswählen, Text eingeben, einschließlich Mediendateien (mit optionalen typkonvertierungen) und erstellen, bearbeiten und Senden von Nachrichten der Aktivität darstellen.

Nachrichtenerweiterungen für Apps bieten drei Haupttypen Inhalt:

- **Interaktive Nachrichten** -sind eine Art von Inhalt von benutzerdefinierten Nachrichten, die eine app generiert, wenn der Benutzer die Meldung, die app tippt wird im Vordergrund gestartet werden.
- **Aufkleber** -Images generiert, die von der app, die in die Nachrichten zwischen den Benutzern aufgenommen werden kann. Finden Sie in unserer [Eis rustikal-Generator](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/) Beispiel-app für eine beispielimplementierung einer Aufkleber-Pack-App.
- **Andere Inhalte unterstützte** – die app bereitstellen Inhalte wie Fotos, Videos, Text oder Links des Typs, der immer von der Nachrichten-app unterstützt wird.

Neu für iOS 10, die Nachrichten-app enthält jetzt einen eigenen dedizierten, integrierte App Store. Alle apps, die Apps Nachrichtenerweiterungen enthalten werden angezeigt, und in diesem Speicher höher gestuft werden. Öffnen Sie die neue Nachrichten-App zeigt alle apps, die aus dem App Store Nachrichten, schnellen Zugriff auf die Benutzer bereitstellen heruntergeladen wurden.

Auch neue in iOS 10, Apple fügte Inline App Attribution die dem Benutzer ermöglicht, eine app einfach zu ermitteln. Z. B. wenn ein Benutzer aus einer Anwendung, der der 2. Benutzer aufweist, Inhalt in einen anderen gesendet (z. B. Aufkleber z. B.) installiert, der Namen der sendenden Anwendung unter den Inhalt im Verlauf Nachricht enthält. Wenn der Benutzer der app tippt Benennen der App-Nachrichtenspeicher, die wir geöffnet werden und die app im Store ausgewählt.

Apps Nachrichtenerweiterungen ähneln vorhandenen iOS-apps, dass der Entwickler vertraut sind, für das Erstellen und Zugriff auf die standard-Frameworks und Funktionen von einer standardmäßigen iOS-app. Zum Beispiel:

- Sie haben Zugriff auf In-App-Käufe.
- Sie verfügen über Zugriff auf die Apple Pay.
- Sie haben Zugriff auf Gerätehardware wie z. B. die Verwendung der Kamera.

Nachrichtenerweiterungen-Apps werden nur für iOS 10 unterstützt, der Inhalt, den diese Erweiterungen zu senden ist allerdings auf Geräten mit WatchOS und MacOS angezeigt werden kann. Die neue _Recents Page_ WatchOS 3 hinzugefügt wurde, wird Anzeigen der neuesten Aufkleber, die aus dem Mobiltelefon, einschließlich der von Nachrichtenerweiterungen für Apps, gesendet wurden und ermöglicht dem Benutzer diese Aufkleber von der Apple Watch senden.

## <a name="about-interactive-messages"></a>Zum interaktiven Nachrichten

Interaktive Nachrichten eine benutzerdefinierte Meldung Blase vorhanden und werden durch eine Nachrichten-App-Erweiterung bereitgestellt. Sie ermöglicht dem Benutzer das interaktive Nachrichteninhalte, erstellen, fügen Sie es in das Nachrichtenfeld für die Eingabe und senden.

[![](advanced-message-app-extensions-images/interactive01.png "Erstellen von interaktiven Nachrichteninhalt")](advanced-message-app-extensions-images/interactive01.png#lightbox)

Des Empfängers kann auf eine interaktive Nachricht antworten, tippen Sie auf die Nachricht Blase im Auftragsverlauf wird Nachricht an die Nachricht App-Erweiterung zu laden, der Sie erstellt. Die Erweiterung gestartete Vollbildmodus und ermöglicht den Benutzer eine Antwort zu verfassen und zurück an den ursprünglichen Benutzer zu senden.

[![](advanced-message-app-extensions-images/interactive02.png "Die Erweiterung gestartet Vollbildmodus")](advanced-message-app-extensions-images/interactive02.png#lightbox)


In den folgenden Themen werden nachfolgend detailliert behandelt:

- Übersicht über die Nachrichten-API
- Der Lebenszyklus der Erweiterung
- Erstellen einer Nachricht
- Senden einer Nachricht

## <a name="messages-api-overview"></a>Übersicht über die Nachrichten-API

Wenn vom Benutzer aufgerufen wird, wird eine Meldung-App-Erweiterung am unteren Rand der Versionsgeschichte der Nachricht in der kompakten Ansichtsmodus angezeigt:

[![](advanced-message-app-extensions-images/interactive03.png "Übersicht über die Nachrichten-API")](advanced-message-app-extensions-images/interactive03.png#lightbox)

1. Die `MSMessageAppViewController` Objekt in der Nachrichten-App-Erweiterung ist die Hauptklasse, die aufgerufen wird, wenn die Erweiterung anzeigen, die dem Benutzer angezeigt wird.
2. Die Konversation erhält der Benutzer als ein `MSConversation` Objektinstanz.
3. Die `MSMessage` -Klasse stellt eine bestimmte Nachricht Blase in der Konversation.
4. `MSSession` Steuert, wie eine Nachricht gesendet wird.
5. `MSMessageTemplateLayout` Steuert, wie die Meldung angezeigt wird

## <a name="the-extension-lifecycle"></a>Der Lebenszyklus der Erweiterung

Betrachten Sie den Prozess, der eine Nachricht App-Erweiterung, die aktiv:

[![](advanced-message-app-extensions-images/interactive04.png "Der Prozess aktiv eine Nachricht App-Erweiterung")](advanced-message-app-extensions-images/interactive04.png#lightbox)

1. Wenn eine Erweiterung (z. B. von der App-Fach) gestartet wird, wird die Nachricht app Starten eines Prozesses.
2. Die `DidBecomeActive` Methode wird aufgerufen, und übergeben einer `MSConversation` , die die Konversation, die die Nachrichten-App-Erweiterung, in ausgeführt wird darstellt.
3. Da die Erweiterung der basiert eine `UIViewController` beide `ViewWillAppear` und `ViewDidAppear` aufgerufen werden.

Betrachten Sie als Nächstes den Prozess einer Nachricht App-Erweiterung, die immer deaktiviert:

[![](advanced-message-app-extensions-images/interactive05.png "Der Prozess eine Nachrichten-App-Erweiterung immer deaktiviert")](advanced-message-app-extensions-images/interactive05.png#lightbox)

1. Die Nachrichten-App-Erweiterung deaktiviert werden, die `ViewWillDisappear` -Methode zuerst aufgerufen werden.
2. Die `ViewDidDisappear` Methode wird aufgerufen.
3. Die `WillResignActive` Methode wird aufgerufen, und übergeben einer `MSConversation` , die die Konversation, die die Nachrichten-App-Erweiterung, in ausgeführt wird darstellt. Zu diesem Zeitpunkt die Verbindung zwischen Nachrichten-app und die Erweiterung sind im Begriff, die freigegeben werden.
4. Zu einem späteren Zeitpunkt wird der Prozess von der Nachrichten-app beendet.

Da eine Erweiterung ein kurzlebiges Prozess ist, wird er vom System, Verarbeitung und Akku Energie zu sparen aggressiv beendet. Der Entwickler sollte dies beim Entwerfen und Implementieren einer Nachrichten-App-Erweiterung Bedenken.

## <a name="composing-a-message"></a>Erstellen einer Nachricht

Nachdem die Nachrichten-App-Erweiterung, die in einem Prozess ausgeführt wird und verfügt über seine Benutzeroberfläche angezeigt wird, kann der folgende Code zum Erstellen einer neuen Nachricht verwendet werden:

```csharp
MSMessage ComposeMessage (IceCream iceCream, string caption, MSSession session = null)
{
    var components = new NSUrlComponents {
        QueryItems = iceCream.QueryItems
    };

    var layout = new MSMessageTemplateLayout {
        Image = iceCream.RenderSticker (true),
        Caption = caption
    };

    var message = new MSMessage (session ?? new MSSession()) {
        Url = components.Url,
        Layout = layout
    };

    return message;
}
```

Dieser Code erstellt ein neues `MSMessage` und legt mehrere Eigenschaften (z. B. `Url`). Während die Nachricht nur für iOS erstellt werden kann, können sie für IOS- und Mac OS gesendet werden, angezeigt werden.

Wenn der Benutzer auf die Blase Nachricht in der Konversation auf MacOS klickt, versucht der Mac, in der URL im Webbrowser die angegebene Adresse zu öffnen. Die Developer-Website muss daher so, dass einige Darstellung der Nachricht in den Webbrowser auf MacOS Computer basierte anzeigen.

Die `AccessibilityLabel` Eigenschaft wird vom Sprachausgaben verwendet, um die Aufzeichnung der Konversation an dem Benutzer zu lesen. Die `Layout` Eigenschaft gibt an, wie die Nachricht, zurzeit nur angezeigt werden sollen die `MSMessageTemplateLayout` wird unterstützt und sieht wie folgt:

[![](advanced-message-app-extensions-images/interactive06.png "Die Vorlage MSMessageTemplateLayout")](advanced-message-app-extensions-images/interactive06.png#lightbox)

Die `Image` Eigenschaft von der `MSMessageTemplateLayout` enthält Inhalte für den Hauptteil der MessageBubble auf dem Bildschirm. Die `MediaFileUrl` Eigenschaft auch enthält Inhalte für den Text der Nachricht Blase, erlaubt aber Inhalte, die von nicht unterstützt wird `UIImage` (z. B. eine Videodatei, der im Hintergrund Schleife würde). Wenn beide die `Image` und `MediaFileUrl` Eigenschaften zur Verfügung, die `Image` -Eigenschaft Vorrang. Die `MediaFileUrl` unterstützt PNG, JPEG, GIF und Video (in einem beliebigen Format, die durch das Media Player Framework abgespielt werden kann) Media-Formate.

Die empfohlene Mediengröße ist 300 x 300 Pixel in der 3 X-Auflösung. Etwas größere und kleinere Objekte auch akzeptiert und Apple vorgeschlagen Testen mit wenigen verschiedenen Größen, um die besten Ergebnisse erzielen. Die Nachrichten-App wird nach unten-Beispiel und Datenträger nach Bedarf zu skalieren.

Wenn die Objekte an den Empfänger gesendet werden, werden alle Medien, die angefügten automatisch transcodiert der Nachrichten-App, um sie aus der Übertragung über die Netzwerke zu optimieren. Aus diesem Grund rät ab Apple einschließlich Text in den Medien, die an die Nachricht angehängt werden, da Medien runterskaliert und für die Übertragung, komprimiert werden daher möglicherweise Rendern Text unleserlich.

Die `ImageTitle` und `ImageSubtitle` Eigenschaften geben Sie eine Beschreibung für die Medien, die in die Nachricht Blase angezeigt werden. Diese Eigenschaften werden werden als Text an das empfangende Gerät gesendet, werden sie klar in der unteren linken Ecke des Bilds gerendert werden.

Die `Caption`, `SubCaption`, `TrailingCaption` und `TrailingSubcaption` Eigenschaften beschreiben das Bild außerdem werden in einem Abschnitt unter dem Bild gerendert. Alle diese Eigenschaften zum Festlegen von `null` wird eine Nachricht Blase ohne Beschriftungsbereichs erstellen:

[![](advanced-message-app-extensions-images/interactive07.png "Eine Nachricht Blase ohne Beschriftungsbereichs")](advanced-message-app-extensions-images/interactive07.png#lightbox)

Das letzte Ereignis zu beachten ist, dass die Nachrichten-app Symbol "der Nachricht App-Erweiterung" in der oberen linken Ecke der Nachricht Blase gezeichnet wird.

## <a name="sending-a-message"></a>Senden einer Nachricht

Sobald eine `MSMessage` zusammengesetzt ist, wird der folgende Code dient zum Senden von wurde:

```csharp
public void SendMessage (MSMessage message)
{
    // Insert the message into the conversation
    ActiveConversation.InsertMessage (message, (error) => {
        // Did the message send successfully?
        if (error == null) {
            // Handle successful send
        } else {
            // Report Error
            Console.WriteLine ("Error: {0}", error);
        }
    };

}
```

Die `ActiveConversation` Eigenschaft von der `MSMessagesAppViewController` die aktuellen Konversation, die die Nachrichten-App-Erweiterung in gestartet wurde, gespeichert werden.

Rufen Sie die `InsertMessage` von der `MSConversation` , sodass Sie die Nachricht in der Konversation und Fehler, die möglicherweise auftreten zu behandeln. Wenn die Nachricht erfolgreich enthalten ist, wird die Nachricht Blase im Feld "Input" angezeigt.

Darüber hinaus kann die Erweiterung verschiedene Typen von Daten an der Konversation, z. B. senden:

- **Text** - `ActiveConversation.InsertText ("Message", (error) => {...});`
- **Anlagen** - `ActiveConversation.InsertAttachment (new NSUrl ("path"), "filename", (error) => {...});`
- **Aufkleber**  -  `ActiveConversation.InsertSticker (sticker, (obj) => {...});` , in denen `sticker` ist eine `MSSticker`.

Sobald der neue Inhalt auf das Feld für die Eingabe ist, kann der Benutzer zum Senden der Nachricht durch Tippen auf die blaue **senden** Schaltfläche (genauso wie für eine typische Nachricht). Es gibt keine Möglichkeit für die Nachrichten-App-Erweiterung zum Senden von Inhalt, dieser Vorgang ist völlig unter der Kontrolle des Benutzers.

## <a name="handling-the-compact-and-expanded-modes"></a>Die kompakte und erweiterte Modi zur Behandlung von

Eine Nachrichten-App-Erweiterung kann in einem der zwei unterschiedliche Ansichtsmodi angezeigt werden:

[![](advanced-message-app-extensions-images/interactive08.png "Eine Nachricht App-Erweiterung, die in zwei unterschiedliche Ansichtsmodi angezeigt: Compact & erweitert")](advanced-message-app-extensions-images/interactive08.png#lightbox)

- **Compact** -Dies ist der Standardmodus, in dem die unteren 25 % der Nachricht an die Nachrichten-App-Erweiterung beanspruchen. Im Compact-Modus ist die app keinen Zugriff auf der Tastatur, horizontalen Bildlauf oder Streifen Geste Prüfer. Die app verfügt über Zugriff auf das Feld für die Eingabe und Aufrufe von `InsertMessage` wird sofort, die es dem Benutzer angezeigt werden.
- **Erweitert** -App-Erweiterung für die Nachricht füllt die gesamte Nachricht anzeigen. Hat keinen Zugriff auf das Feld für die Eingabe, aber verfügt über Zugriff auf die Tastatur, horizontalen Bildlauf und Wischen Geste Merkmale.

Eine Nachrichten-App-Erweiterung gewechselt werden, zwischen diesen Modi entweder programmgesteuert oder manuell vom Benutzer zu einem beliebigen Zeitpunkt und sofort reagieren auf eine der in der Sicht Modus ändert sein.

Betrachten Sie das folgende Beispiel der Wechsel zwischen den zwei unterschiedliche Ansichtsmodi verarbeiten. Zwei unterschiedliche View-Controller werden für jeden Status erforderlich. Die `StickerBrowserViewController` Handles die **Compact** anzeigen und die `AddStickerViewController` verarbeitet die **erweitert** anzeigen:

```csharp
using System;

using Messages;
using Foundation;
using UIKit;

namespace MessagesExtension {
    public partial class MessagesViewController : MSMessagesAppViewController, IIceCreamsViewControllerDelegate, IBuildIceCreamViewControllerDelegate {
        public MessagesViewController (IntPtr handle) : base (handle)
        {
        }

        public override void WillBecomeActive (MSConversation conversation)
        {
            base.WillBecomeActive (conversation);

            // Present the view controller appropriate for the conversation and presentation style.
            PresentViewController (conversation, PresentationStyle);
        }

        public override void WillTransition (MSMessagesAppPresentationStyle presentationStyle)
        {
            var conversation = ActiveConversation;
            if (conversation == null)
                throw new Exception ("Expected an active converstation");

            // Present the view controller appropriate for the conversation and presentation style.
            PresentViewController (conversation, presentationStyle);
        }

        void PresentViewController (MSConversation conversation, MSMessagesAppPresentationStyle presentationStyle)
        {
            // Determine the controller to present.
            UIViewController controller;

            if (presentationStyle == MSMessagesAppPresentationStyle.Compact) {
                // Show a list of previously created ice creams.
                controller = InstantiateIceCreamsController ();
            } else {
                var iceCream = new IceCream (conversation.SelectedMessage);
                controller = iceCream.IsComplete ? InstantiateCompletedIceCreamController (iceCream) : InstantiateBuildIceCreamController (iceCream);
            }

            foreach (var child in ChildViewControllers) {
                child.WillMoveToParentViewController (null);
                child.View.RemoveFromSuperview ();
                child.RemoveFromParentViewController ();
            }

            AddChildViewController (controller);
            controller.View.Frame = View.Bounds;
            controller.View.TranslatesAutoresizingMaskIntoConstraints = false;
            View.AddSubview (controller.View);

            controller.View.LeftAnchor.ConstraintEqualTo (View.LeftAnchor).Active = true;
            controller.View.RightAnchor.ConstraintEqualTo (View.RightAnchor).Active = true;
            controller.View.TopAnchor.ConstraintEqualTo (View.TopAnchor).Active = true;
            controller.View.BottomAnchor.ConstraintEqualTo (View.BottomAnchor).Active = true;

            controller.DidMoveToParentViewController (this);
        }

        UIViewController InstantiateIceCreamsController ()
        {
            // Instantiate a `IceCreamsViewController` and present it.
            var controller = Storyboard.InstantiateViewController (IceCreamsViewController.StoryboardIdentifier) as IceCreamsViewController;
            if (controller == null)
                throw new Exception ("Unable to instantiate an IceCreamsViewController from the storyboard");

            controller.Builder = this;
            return controller;
        }

        UIViewController InstantiateBuildIceCreamController (IceCream iceCream)
        {
            // Instantiate a `BuildIceCreamViewController` and present it.
            var controller = Storyboard.InstantiateViewController (BuildIceCreamViewController.StoryboardIdentifier) as BuildIceCreamViewController;
            if (controller == null)
                throw new Exception ("Unable to instantiate an BuildIceCreamViewController from the storyboard");

            controller.IceCream = iceCream;
            controller.Builder = this;

            return controller;
        }

        public UIViewController InstantiateCompletedIceCreamController (IceCream iceCream)
        {
            // Instantiate a `BuildIceCreamViewController` and present it.
            var controller = Storyboard.InstantiateViewController (CompletedIceCreamViewController.StoryboardIdentifier) as CompletedIceCreamViewController;
            if (controller == null)
                throw new Exception ("Unable to instantiate an CompletedIceCreamViewController from the storyboard");

            controller.IceCream = iceCream;
            return controller;
        }

        public void DidSelectAdd (IceCreamsViewController controller)
        {
            Request (MSMessagesAppPresentationStyle.Expanded);
        }

        public void Build (BuildIceCreamViewController controller, IceCreamPart iceCreamPart)
        {
            var conversation = ActiveConversation;
            if (conversation == null)
                throw new Exception ("Expected a conversation");

            var iceCream = controller.IceCream;
            if (iceCream == null)
                throw new Exception ("Expected the controller to be displaying an ice cream");

            string messageCaption = string.Empty;
            var b = iceCreamPart as Base;
            var s = iceCreamPart as Scoops;
            var t = iceCreamPart as Topping;

            if (b != null) {
                iceCream.Base = b;
                messageCaption = "Let's build an ice cream";
            } else if (s != null) {
                iceCream.Scoops = s;
                messageCaption = "I added some scoops";
            } else if (t != null) {
                iceCream.Topping = t;
                messageCaption = "Our finished ice cream";
            } else {
                throw new Exception ("Unexpected type of ice cream part selected.");
            }

            // Create a new message with the same session as any currently selected message.
            var message = ComposeMessage (iceCream, messageCaption, conversation.SelectedMessage?.Session);

            // Add the message to the conversation.
            conversation.InsertMessage (message, null);

            // If the ice cream is complete, save it in the history.
            if (iceCream.IsComplete) {
                var history = IceCreamHistory.Load ();
                history.Append (iceCream);
                history.Save ();
            }

            Dismiss ();
        }

        MSMessage ComposeMessage (IceCream iceCream, string caption, MSSession session = null)
        {
            var components = new NSUrlComponents {
                QueryItems = iceCream.QueryItems
            };

            var layout = new MSMessageTemplateLayout {
                Image = iceCream.RenderSticker (true),
                Caption = caption
            };

            var message = new MSMessage (session ?? new MSSession()) {
                Url = components.Url,
                Layout = layout
            };

            return message;
        }
    }
}
```

Die `DidTransition` Methode wird überschrieben, um den Wechsel zwischen den beiden Modi zu behandeln:

```csharp
public override void DidTransition (MSMessagesAppPresentationStyle presentationStyle)
{
    base.DidTransition (presentationStyle);

    // Take action based on style
    switch (presentationStyle) {
    case MSMessagesAppPresentationStyle.Compact:
        PresentStickerBrowser ();
        break;
    case MSMessagesAppPresentationStyle.Expanded:
        PresentAddSticker ();
        break;
    }
}
```

Optional, hätte die app verwendet die `WillTransition` Methode, um die Änderung der Ansicht Modus verarbeiten, bevor es dem Benutzer angezeigt wird (wie im obigen Beispiel Icecream-Generator ausgeführt wird). Weitere Informationen finden Sie unter unsere [Weitere Aufkleber Anpassung](~/ios/platform/message-app-integration/intro-to-message-app-extensions.md) Dokumentation.

## <a name="replying-to-a-message"></a>Antworten auf eine Nachricht

Es gibt zwei Fälle, die eine Nachrichten-App-Erweiterung beim Antworten auf Nachrichten verarbeiten müssen:

[![](advanced-message-app-extensions-images/interactive09.png "Die Nachrichten-App-Erweiterung in den Modi aktiv und inaktiv")](advanced-message-app-extensions-images/interactive09.png#lightbox)

- **Erweiterung ist inaktiv** -einmal die Nachricht App-Erweiterung Nachrichtenblasen in die Aufzeichnung Nachricht, die der Benutzer nutzen kann, um die Erweiterungen zu aktivieren und die interaktive Konversation weiterhin vorhanden ist.
- **Erweiterung wird aktiv** -Benutzer kann die Nachricht App-Erweiterung Nachricht Blase wird in die Aufzeichnung der Nachricht, geben den Ansichtsmodus erweitert und den interaktiven Prozess aus, wo sie aufgehört haben weiterhin tippen.

### <a name="the-extension-is-inactive"></a>Die Erweiterung ist inaktiv

Wenn eine Blase Nachricht vom Benutzer in der Aufzeichnung der Nachricht abgerufen wird, und die Nachrichten-App-Erweiterung inaktiv ist, erfolgt die folgende Schritte:

[![](advanced-message-app-extensions-images/interactive10.png "Behandlung von einem inaktiven Nachricht Blase")](advanced-message-app-extensions-images/interactive10.png#lightbox)

1. Der Benutzer tippt der Erweiterungs Nachricht Blase.
2. Wenn eine Erweiterung gestartet wird, wird die Nachricht app Starten eines Prozesses.
3. Die `DidBecomeActive` Methode wird aufgerufen, und übergeben einer `MSConversation` , die die Konversation, die die Nachrichten-App-Erweiterung, in ausgeführt wird darstellt.
4. Da die Erweiterung der basiert eine `UIViewController` beide `ViewWillAppear` und `ViewDidAppear` aufgerufen werden.

Wenn der Prozess abgeschlossen ist, wird die Nachricht-App-Erweiterung im Anzeigemodus erweitert angezeigt.

### <a name="the-extension-is-active"></a>Die Erweiterung ist aktiv

Wenn eine Nachricht Blase vom Benutzer in der Aufzeichnung der Nachricht abgerufen wird, und die Nachrichten-App-Erweiterung aktiv ist, wird der folgende Prozess auftreten:

[![](advanced-message-app-extensions-images/interactive11.png "Behandlung von einer aktiven Nachricht Blase")](advanced-message-app-extensions-images/interactive11.png#lightbox)

1. Der Benutzer tippt der Erweiterungs Nachricht Blase.
2. Da die Nachrichten-App-Erweiterung bereits aktiv ist, wird die `WillTransition` Methode der `MSMessagesAppViewController` aufgerufen, um die kompakte umstellen, für den der Expanded-Ansichtsmodus behandeln.
3. Die `DidSelectMessage` Methode der `MSMessagesAppViewController` wird aufgerufen, und übergeben der `MSMessage` und `MSConversation` , zu der die Nachricht Blase gehört.
4. Die `DidTransition` Methode der `MSMessagesAppViewController` aufgerufen, um die kompakte umstellen, für den der Expanded-Ansichtsmodus behandeln.

Erneut, wenn der Prozess abgeschlossen ist, wird die Nachrichten-App-Erweiterung im Anzeigemodus erweitert angezeigt.

### <a name="accessing-the-selected-message"></a>Zugreifen auf die ausgewählte Nachricht

In beiden Fällen wird beim Tippen auf eine Nachricht Blase gehören, die Nachrichten-App-Erweiterung, sie müssen für den Zugriff auf die `MSMessage` , die abgerufen wurde, mithilfe der `SelectedMessage` Eigenschaft von der `MSConversation`.

Zum Beispiel:

```csharp
using System;
using Foundation;
using Messages;
using UIKit;


namespace MessageExtension
{
    public partial class MessagesViewController : MSMessagesAppViewController
    {
        ...

        #region Override Methods
        public override void DidSelectMessage (MSMessage message, MSConversation conversation)
        {
            base.DidSelectMessage (message, conversation);

            // Get selected message
            var selected = conversation.SelectedMessage;

            // Present the user interface to continue editing
            // the conversation
            ...
        }
        #endregion
    }
}
```

Die ausgewählte Nachricht in der Nachricht App-Erweiterung Benutzeroberfläche angezeigt werden soll, und der Benutzer darf eine Antwort zu verfassen.

## <a name="removing-partially-completed-messages"></a>Entfernen einer teilweise Nachrichten abgeschlossen

Bei die einzelnen Schritten eine interaktive Konversation zwischen zwei des Benutzers in der Konversation gesendet wird, können die teilweise abgeschlossene Blasen der Meldung beginnen Sie mit der Nachricht Aufzeichnung überflüssigen Daten gefüllt:

[![](advanced-message-app-extensions-images/interactive12.png "Die teilweise abgeschlossene Nachrichtenblasen können viele Realisierungslinks enthalten die Aufzeichnung der Nachricht")](advanced-message-app-extensions-images/interactive12.png#lightbox)

Stattdessen sollte der Nachrichten-App-Erweiterung der vorherigen Nachrichtenblasen in einem kompakten Kommentar in die Nachricht Aufzeichnung reduzieren:

[![](advanced-message-app-extensions-images/interactive13.png "Reduzieren die vorherige Nachrichtenblasen in die Aufzeichnung der Nachricht")](advanced-message-app-extensions-images/interactive13.png#lightbox)

Dies erfolgt mithilfe einer `MSSession` so reduzieren Sie alle vorhandenen Schritte. Damit die `DidSelectMessage` Methode der `MSMessagesAppViewController` Klasse kann geändert werden, um wie folgt aussehen:

```csharp
public override void DidSelectMessage (MSMessage message, MSConversation conversation)
{
    base.DidSelectMessage (message, conversation);

    MSSession session;
    var summary = "";

    // Get selected message
    var selected = conversation.SelectedMessage;

    // Does the selected message already have a session?
    if (selected.Session == null) {
        // No, create a new session
        session = new MSSession ();
        summary = "Let's build an ice cream";
    } else {
        // Yes, use the existing session
        session = selected.Session;
        summary = "I added some scoops";
    }

    // Create an instance of the message being composed
    var existingMessage = new MSMessage (session);
    message.SummaryText = summary;
    ...

    // Present the user interface to continue editing
    // the conversation
    ...
}
```

Wenn die ausgewählte Nachricht bereits vorhandenen `MSSession`, es wird ein neues else verwendet `MSSession` wird erstellt. Die `SummaryText` Eigenschaft von der `MSMessage` wird verwendet, um eine Beschriftung reduzierten Schritten hinzufügen. Wenn die `SummaryText` -Eigenschaftensatz auf `null`, die vorherigen Schritten in der Konversation werden vollständig entfernt werden, von der Konversation Aufzeichnung.

## <a name="advanced-message-api-features"></a>Erweiterte Nachricht API-Funktionen

Mit den grundlegenden Funktionen der neuen Nachricht-API, die oben genannten ausführlich behandelt nun untersucht einige der erweiterten Funktionen, die von Apple auf das Framework erstellt wurde.

Einerseits bestehen verschiedene Überschreibungsmethoden in die `MSMessagesAppViewController` Klasse, die einen tieferen Zugriff auf die Konversation bereitstellen:

- `DidStartSendingMessage` -Wird aufgerufen, wenn der Benutzer die Sendeschaltfläche tippt. Dies bedeutet nicht, dass die Nachricht tatsächlich Lieferung an den Empfänger, die nur wurde, der Send-Prozess gestartet wurde.
- `DidCancelSendingMessage` -Dies geschieht, wenn der Benutzer tippt der *X* Schaltfläche in der oberen rechten Ecke der Nachricht Blase wird in die Aufzeichnung der Konversation.
- `DidReceiveMessage` – Diese Methode wird aufgerufen, wenn die Nachrichten-App-Erweiterung aktiv ist ein eine neue Nachricht wurde von einer der Teilnehmer der Konversation empfangen.

### <a name="group-conversations"></a>Gruppenunterhaltungen

Eine Nachrichten-App-Erweiterung können verwendet werden, während die Benutzer gruppenunterhaltungen (mit 3 oder mehr Personen) beteiligt sind, und dies müssen Sie beim Entwerfen und Implementieren einer Nachrichten-App-Erweiterung berücksichtigt werden.

Sehen Sie sich die folgenden Interaktion in einer Gruppe Konversation mit drei Benutzer an:

[![](advanced-message-app-extensions-images/interactive14.png "Interaktion in einer Gruppe Konversation mit drei Benutzer")](advanced-message-app-extensions-images/interactive14.png#lightbox)

1. Benutzer 1 sendet eine Gruppe interaktive Meldung gefragt werden, Benutzer 2, sowie Benutzer 3 ein Hamburger häufigsten auszuwählen.
2. Benutzer 2 wählt Tomatoes.
3. Benutzer 3 wählt Gurken.
4. Benutzer 2 und 3 Benutzer Auswahlmöglichkeiten eintreffen an Benutzer 1 zur nahezu gleichen Zeit. Folglich Benutzer-2-Auswahl in eine Zusammenfassungszeile reduziert und ist nicht verfügbar. Dieser Fall kann haben auch wurde gekippt, mit Benutzer-2-Option wird angezeigt, und Benutzer 3 reduziert wird der.

Dieses Verhalten ist in beiden Fällen unerwünschten, wie Benutzer 1 auf Benutzer-2 und 3 Benutzer Optionen zugreifen können soll. Um diese Situation zu behandeln, damit, dass die Nachrichten-App-Erweiterung den nachrichtenzustand in der Cloud gespeichert und mit der URL-Eigenschaft des Apple liegt die `MSMessage` (, werden zwischen den Benutzern gesendet) den Zugriff auf diesen Status.

Wenn der Benutzer eine Nachricht sendet, wird ein Sitzungstoken generiert und in die Cloud mit dem aktuellen Nachrichtenstatus abgelegt. Wenn ein Benutzer auf eine Blase Nachricht in der Konversation Aufzeichnung tippt, wird das Sitzungstoken zum Abrufen der aktuellen Sitzungsstatus aus der Cloud verwendet.

### <a name="sender-identifiers"></a>"MSMQSENDERIDTYPE"

Nehmen Sie als Beispiel einer Gruppe Konversation, die oben genannte, um den Zugriff auf den Bezeichner des Absenders einer Nachricht zu besprechen:

[![](advanced-message-app-extensions-images/interactive15.png "Gruppe Konversation Senden von Bezeichnern")](advanced-message-app-extensions-images/interactive15.png#lightbox)

1. Benutzer 1 sendet erneut, eine Gruppe interaktive Meldung gefragt werden, Benutzer 2, sowie Benutzer 3 ein Hamburger häufigsten auszuwählen.
2. Benutzer 3 wählt Gurken.
3. 3 Benutzerauswahl eingeht, zurück an den Benutzer 1 und Benutzer 2 wurde noch nicht beantwortet.
4. Da Apple Bedenken hinsichtlich der Privatsphäre der Benutzer ist, weiß der Nachrichten-App-Erweiterung nur über einen eindeutigen Bezeichner (als eine `NSUUID`), die jeder Teilnehmer der Konversation zugewiesen ist. Auf dem lokalen Gerät wird nur der aktuelle Benutzer-ID bezeichnet.
5. Die `MSMessage` verfügt über eine `SenderIdentifier` Eigenschaft, das einem Benutzer entspricht der in der Liste der Teilnehmer bekannt, an der Erweiterung.
6. Jede Benutzergerät hat eine eigene Kopie der Teilnehmerliste erneut aus, wobei nur der Bezeichner für den lokalen Benutzer bekannt ist.
7. Wenn eine Nachricht gesendet wird, dessen `SenderIdentifier` Eigenschaft wird auch als die des lokalen Benutzers bezeichnet.

Die "MSMQSENDERIDTYPE" können auf folgende Weise verwendet werden:

- Durch einen Blick auf die Liste der Teilnehmer erhalten die Erweiterung die Anzahl der Benutzer in der Konversation.
- Wenn die Erweiterung von einem Benutzer eine Nachricht empfängt, sie können verfolgen den Absenderbezeichner der. Wenn es sich um eine Nachricht mit den gleichen Absenderbezeichner empfängt, weiß die Erweiterung, dass es vom gleichen Benutzer handelt.
- Sie können verwendet werden, um einen bestimmten Benutzer in der Konversation zu identifizieren.

Den Absenderbezeichner können verwendet werden, in die Text-Felder von der `MSMessageTemplateLayout` , indem Sie es mit einem Dollarzeichen voranstellen (`$`). Zum Beispiel:

```csharp
// Pass along the sender identifier
var layout = new MSMessageTemplateLayout()
layout.Caption = string.format("${0} wants pickles.",Conversation.LocalParticipantIdentifier.UuidString);
```

Wenn eine Nachricht Blase mit dieser Art der Formatierung von Nachrichten-app angezeigt wird, wird es ersetzt die `$uuid...` mit der Name des Ansprechpartners des Teilnehmers der Konversation, die die Nachricht gesendet.

Den Absenderbezeichner wird auf jedes Gerät eindeutig, daher betrachten das Diagramm oben erneut, beachten Sie, dass Benutzer 1 Geräte- und Benutzer-3-Gerät einen anderen eindeutigen Bezeichner für Absender für jeden Teilnehmer der Konversation.

Die "MSMQSENDERIDTYPE" beziehen sich auf die Installation der App-Erweiterung der Nachricht. Also wenn ein Benutzer deinstalliert und neu installiert die Nachricht-App-Erweiterung wurde neue "MSMQSENDERIDTYPE" für die Installation des neuen generiert.

Die Erweiterung kann den folgenden Code verwenden, die "MSMQSENDERIDTYPE" für den Zugriff auf:

```csharp
public override void DidStartSendingMessage (MSMessage message, MSConversation conversation)
{
    base.DidStartSendingMessage (message, conversation);

    // Get the sender's participant ID
    var senderID = message.SenderParticipantIdentifier;

    // Get the local participant ID
    var localID = conversation.LocalParticipantIdentifier;

    // Get the remote participant IDs
    var remoteIDs = conversation.RemoteParticipantIdentifiers;
}
```

## <a name="supported-platforms"></a>Unterstützte Plattformen

Die interaktive Nachrichten durch eine Nachrichten-App-Erweiterung generiert werden in den folgenden Apple-Plattformen übermittelt:

- WatchOS 3
- macOS Sierra
- iOS 10

Drei Plattformen lässt nur iOS 10 den Benutzer eine interaktive Nachricht zu generieren. Auf MacOS Sierra, klickt der Benutzer auf eine interaktive Nachricht Blase URL angefügt die `MSMessage` in Safari geöffnet wird und eine Darstellung der Nachricht sollte es angezeigt werden.

In WatchOS Nachrichten-app, können die Übergabe einer interaktiven Nachricht auf einem angeschlossenen iOS-Gerät, auf dem der Benutzer eine Antwort verfassen können.

Die neue Nachrichten-API bietet Unterstützung für Fallback, falls ältere Apple-Plattformen die interaktive Nachricht empfangen wird:

- WatchOS 2 +
- OS X 10.11 +
- iOS 9 +

Sie werden in einem fallback-Format als zwei separate Nachrichten übermittelt werden:

- Das Bild werden das Layout der Vorlage bereitgestellt.
- Die andere die URL des sein wie angegeben in der `MSMessage`.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel verfügt über erweiterte Techniken zum Arbeiten mit Nachricht-App-Erweiterungen in einem Xamarin.iOS-Lösung, mit den präsentiert der **Nachrichten** app und vorhanden neue Funktionen für den Benutzer.


## <a name="related-links"></a>Verwandte Links

- [Eis rustikal-Generator (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/)
- [Nachrichten-Referenz](https://developer.apple.com/reference/messages)
- [Programmierhandbuch für App-Erweiterung](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)
