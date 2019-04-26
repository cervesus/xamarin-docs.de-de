---
title: Advance Message-App-Erweiterungen in Xamarin.iOS
description: Dieser Artikel beschreibt erweiterte Techniken für die Arbeit mit Nachrichten-App-Erweiterungen in einer Xamarin.iOS-Projektmappe, die Nachrichten-app integriert und stellt neue Funktionen für den Benutzer.
ms.prod: xamarin
ms.assetid: 394A1FDA-AF70-4493-9B2C-4CFE4BE791B6
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: baceb59116dd907918b34eca4f44293051190954
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61155570"
---
# <a name="advanced-message-app-extensions-in-xamarinios"></a>Advance Message-App-Erweiterungen in Xamarin.iOS

_Dieser Artikel beschreibt erweiterte Techniken für die Arbeit mit Nachrichten-App-Erweiterungen in einer Xamarin.iOS-Projektmappe, die Nachrichten-app integriert und stellt neue Funktionen für den Benutzer._


Neue IOS 10, integriert eine Nachrichten-App-Erweiterung der **Nachrichten** -app und stellt neue Funktionen für den Benutzer. Die Erweiterung kann Text, Aufkleber, Mediendateien und interaktive Nachrichten senden.

## <a name="about-message-app-extensions"></a>Informationen zu Nachrichten-App-Erweiterungen

Wie bereits erwähnt, ist eine Nachrichten-App-Erweiterung in integriert die **Nachrichten** -app und stellt neue Funktionen für den Benutzer. Die Erweiterung kann Text, Aufkleber, Mediendateien und interaktive Nachrichten senden. Es stehen zwei Arten von Nachrichten-App-Erweiterung:

- **Aufkleber Packs** -enthält eine Auflistung von Aufkleber, die der Benutzer eine Nachricht hinzugefügt werden kann. Aufkleber Packs erstellt werden, ohne Code schreiben zu müssen.
- **iMessage App** -kann eine benutzerdefinierte Benutzeroberfläche innerhalb der Nachrichten-app für Aufkleber auswählen, Text eingeben, einschließlich Media-Dateien (mit optionalen typkonvertierungen) und erstellen, bearbeiten und Senden von Nachrichten von Interaktion darstellen.

Nachrichtenerweiterungen für die Apps bieten drei Haupttypen Inhalt:

- **Interaktive Nachrichten** -sind eine Art von Inhalt für die benutzerdefinierte Nachricht, die eine app generiert werden, wenn sich der Benutzer tippt in der Nachricht, die app auf wird im Vordergrund gestartet werden.
- **Aufkleber** -Images generiert, die von der app, die in die Nachrichten zwischen den Benutzern aufgenommen werden kann. Finden Sie in unserer [Eiskrem-Generator](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/) Beispiel-app für eine beispielimplementierung einer Aufkleber-Pack-App.
- **Andere unterstützte Inhalte** -Geben Sie die app kann Inhalte wie Fotos, Videos, Text oder Links des Typs, der immer von der Nachrichten-app unterstützt wird.

Neue IOS 10, die Nachrichten-app enthält nun eine eigene dedizierte, integrierten App-Store. Alle apps, die Nachrichten-Apps-Erweiterungen enthalten werden angezeigt und in diesem Speicher höher gestuft werden. Die neue Nachrichten-App-Drawer werden alle apps angezeigt, die von der Nachrichten-App Store, um schnellen Zugriff an die Benutzer bereitzustellen heruntergeladen wurden.

Ebenfalls neu in iOS 10, Apple wurde hinzugefügt Inline App Attribution dem der Benutzer eine app leicht ermitteln kann. Wenn ein Benutzer Inhalte in eine andere aus einer app sendet, die der 2. Benutzer keinen ist das installiert (z. B. einem Aufkleber z. B.), z. B. zu der Namen der senden-app unter den Inhalt im Verlauf Nachricht aufgeführt. Wenn der Benutzer der app tippt auf Namen Sie, die Nachrichten-App-Store wir geöffnet werden und die app im Store ausgewählt.

Nachrichtenerweiterungen für Apps sind vergleichbar mit vorhandenen iOS-apps, dass der Entwickler vertraut sind, für das Erstellen und sie Zugriff auf die standard-Frameworks und Funktionen von einer standard-iOS-app haben. Zum Beispiel:

- Sie haben Zugriff auf In-App-Käufe.
- Sie haben Zugriff auf die Apple Pay.
- Sie haben Zugriff auf die Hardware der Geräte wie z. B. die Kamera.

Nachrichtenerweiterungen für Apps werden nur unter iOS 10 unterstützt, die der Inhalt, den diese Erweiterungen zu senden ist jedoch auf WatchOS und MacOS-Geräte. Die neue _Recents Page_ WatchOS 3 hinzugefügt wurde, angezeigt aktuelle Aufkleber, die vom Telefon, einschließlich der von Nachrichtenerweiterungen für Apps, gesendet wurden, und Sie ermöglicht dem Benutzer, die Aufkleber von der Apple Watch zu senden.

## <a name="about-interactive-messages"></a>Zum interaktiven Nachrichten

Interaktive Nachrichten eine benutzerdefinierte Nachricht Blase vorhanden und werden von einer Nachrichten-App-Erweiterung bereitgestellt. Sie ermöglicht dem Benutzer, erstellen Sie interaktive Nachrichteninhalt, fügen Sie ihn in das Feld für die Eingabe und zu senden.

[![](advanced-message-app-extensions-images/interactive01.png "Erstellen von Inhalt von interaktiven Nachrichten")](advanced-message-app-extensions-images/interactive01.png#lightbox)

Der empfangende Benutzer kann auf eine interaktive Nachricht antworten, durch Tippen auf seine Nachricht Blase in den Verlauf der Nachricht, um die Nachrichten-App-Erweiterung zu laden, die sie erstellt haben. Die Erweiterung wird gestartet Vollbildmodus und ermöglicht dem Benutzer zu verfassen eine Antwort zurück an des ursprünglichen Benutzers zu senden.

[![](advanced-message-app-extensions-images/interactive02.png "Die Erweiterung gestartet, Vollbildmodus")](advanced-message-app-extensions-images/interactive02.png#lightbox)


In den folgenden Themen werden unten im Detail behandelt:

- Nachrichten-API – Übersicht
- Die Erweiterung-Anwendungslebenszyklus
- Verfassen einer Nachricht
- Senden einer Nachricht

## <a name="messages-api-overview"></a>Nachrichten-API – Übersicht

Wenn vom Benutzer aufgerufen wird, wird am Ende den Verlauf der Nachricht in der kompakten Ansichtsmodus eine Nachrichten-App-Erweiterung angezeigt:

[![](advanced-message-app-extensions-images/interactive03.png "Nachrichten-API – Übersicht")](advanced-message-app-extensions-images/interactive03.png#lightbox)

1. Die `MSMessageAppViewController` Objekt in die Nachrichten-App-Erweiterung ist die Hauptklasse, die aufgerufen wird, wenn der Erweiterung anzeigen, die dem Benutzer angezeigt wird.
2. Die Konversation wird angezeigt, die dem Benutzer als ein `MSConversation` Objektinstanz.
3. Die `MSMessage` Klasse stellt eine bestimmte Nachricht Blase in der Konversation.
4. `MSSession` Steuert, wie eine Nachricht gesendet wird.
5. `MSMessageTemplateLayout` Steuert, wie die Meldung angezeigt wird

## <a name="the-extension-lifecycle"></a>Die Erweiterung-Anwendungslebenszyklus

Sehen Sie sich den Prozess einer Nachrichten-App-Erweiterung, die aktiv:

[![](advanced-message-app-extensions-images/interactive04.png "Der Vorgang eine Nachrichten-App-Erweiterung nicht aktiviert")](advanced-message-app-extensions-images/interactive04.png#lightbox)

1. Wenn eine Erweiterung (z. B. von App-Drawer) gestartet wird, wird die Nachricht-app einen Prozess gestartet.
2. Die `DidBecomeActive` Methode aufgerufen wird, und übergeben einen `MSConversation` , die die Konversation, die die Nachrichten-App-Erweiterung, im ausgeführt wird darstellt.
3. Da die Erweiterung der basiert eine `UIViewController` sowohl `ViewWillAppear` und `ViewDidAppear` aufgerufen werden.

Als Nächstes sehen Sie sich den Prozess der eine Nachrichten-App-Erweiterung immer deaktiviert:

[![](advanced-message-app-extensions-images/interactive05.png "Der Vorgang eine Nachrichten-App-Erweiterung immer deaktiviert")](advanced-message-app-extensions-images/interactive05.png#lightbox)

1. Wenn die Nachrichten-App-Erweiterung deaktiviert wird, wird die `ViewWillDisappear` Methode zuerst aufgerufen werden.
2. Die `ViewDidDisappear` aufgerufene Methode.
3. Die `WillResignActive` Methode aufgerufen wird, und übergeben einen `MSConversation` , die die Konversation, die die Nachrichten-App-Erweiterung, im ausgeführt wird darstellt. An diesem Punkt die Verbindung zwischen der Nachrichten-app und die Erweiterung sind im Begriff, die freigegeben werden.
4. Zu einem späteren Zeitpunkt wird der Prozess von der Nachrichten-app beendet.

Da eine Erweiterung ein kurzlebiges Prozess ist, wird sie durch das System an die Verarbeitung und Akku zu schonen aggressiv beendet. Der Entwickler sollte diese Punkte beim Entwerfen und Implementieren einer Nachrichten-App-Erweiterung beachtet.

## <a name="composing-a-message"></a>Verfassen einer Nachricht

Nach der die Nachrichten-App-Erweiterung, die in einem Prozess ausgeführt wird und verfügt über die Benutzeroberfläche angezeigt, kann der folgende Code verwendet werden, um eine neue Nachricht erstellen:

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

Dieser Code erstellt ein neues `MSMessage` und verschiedene Eigenschaften werden festgelegt (wie z. B. `Url`). Während die Nachricht nur unter iOS erstellt werden kann, können sie für IOS- und MacOS gesendet werden, angezeigt werden.

Klickt der Benutzer auf die Blase Nachricht in der Konversation unter MacOS, versucht der Mac, die in die URL im Webbrowser angegebene Adresse zu öffnen. Der Entwickler-Website sollte daher an, dass einige Darstellung der Nachricht in einem Webbrowser unter MacOS Computer basieren.

Die `AccessibilityLabel` Eigenschaft wird von der Sprachausgabe verwendet, um die Aufzeichnung der Konversation für dem Benutzer zu lesen. Die `Layout` Eigenschaft gibt an, wie die Nachricht, zurzeit nur angezeigt wird der `MSMessageTemplateLayout` wird unterstützt und sieht wie folgt aus:

[![](advanced-message-app-extensions-images/interactive06.png "Die MSMessageTemplateLayout-Vorlage")](advanced-message-app-extensions-images/interactive06.png#lightbox)

Die `Image` Eigenschaft der `MSMessageTemplateLayout` enthält Inhalte für den Hauptteil des der MessageBubble auf dem Bildschirm. Die `MediaFileUrl` Eigenschaft auch enthält Inhalte für den Text der Nachricht Blase, jedoch können Inhalte, die von nicht unterstützt wird `UIImage` (z. B. eine Videodatei, der im Hintergrund-Schleife würde). Wenn beide die `Image` und `MediaFileUrl` Eigenschaften zur Verfügung, die `Image` -Eigenschaft Vorrang. Die `MediaFileUrl` unterstützt PNG, JPEG, GIF und Video (in einem beliebigen Format, die von Media Player Framework wiedergegeben werden kann) Medienformate.

Die empfohlene Mediengröße ist 300 x 300 Pixel in der 3 X-Auflösung. Etwas größere und kleinere Objekte werden ebenfalls akzeptiert und Apple empfiehlt Tests mit wenigen unterschiedlichen Größen, um die besten Ergebnisse zu erhalten. Die Nachrichten-App werden Stichproben und der Datenträger nach Bedarf skalieren.

Wenn die Objekte an den Empfänger gesendet werden, werden alle angefügten Medien automatisch transcodiert von der Nachrichten-app, die sie aus der Übertragung über Netzwerke zu optimieren. Aus diesem Grund wird eine verhindert Apple einschließlich Text in den Medien, die an die Nachricht angefügt werden, da Media zentral herunterskaliert und für die Übertragung, komprimiert werden daher möglicherweise Rendern von Text unleserlich.

Die `ImageTitle` und `ImageSubtitle` Eigenschaften geben Sie eine Beschreibung für die Medien, die in der Nachricht Blase angezeigt werden. Diese Eigenschaften erhalten als Text an das Gerät empfangen, werden sie klar in der unteren linken Ecke des Bilds gerendert werden.

Die `Caption`, `SubCaption`, `TrailingCaption` und `TrailingSubcaption` Eigenschaften weiter beschreiben das Bild, und in einem Abschnitt unter dem Bild gerendert wird. Alle diese Eigenschaften zum Festlegen von `null` eine Blase Nachricht ohne Beschriftungsbereichs erstellen:

[![](advanced-message-app-extensions-images/interactive07.png "Eine Nachricht Blase ohne Beschriftungsbereichs")](advanced-message-app-extensions-images/interactive07.png#lightbox)

Die letzten ist zu beachten, dass die Nachrichten-app die Nachrichten-App-Erweiterung des Symbols in der oberen linken Ecke der Nachricht Blase gezeichnet wird.

## <a name="sending-a-message"></a>Senden einer Nachricht

Sobald eine `MSMessage` wurde zusammengesetzt ist, wird der folgende Code kann verwendet werden, um sie zu senden:

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

Die `ActiveConversation` Eigenschaft der `MSMessagesAppViewController` enthält die aktuelle Konversation, die in die Nachrichten-App-Erweiterung gestartet wurde.

Rufen Sie die `InsertMessage` von der `MSConversation` die Nachricht in der Konversation und Fehler, die möglicherweise auftreten zu behandeln. Wenn die Nachricht erfolgreich enthalten ist, wird die Blase für die Nachricht in der Input-Feld angezeigt.

Darüber hinaus kann die Erweiterung unterschiedliche Arten von Daten in das Gespräch an, wie z. B. senden:

- **Text** - `ActiveConversation.InsertText ("Message", (error) => {...});`
- **Anlagen** - `ActiveConversation.InsertAttachment (new NSUrl ("path"), "filename", (error) => {...});`
- **Aufkleber**  -  `ActiveConversation.InsertSticker (sticker, (obj) => {...});` , in denen `sticker` ist eine `MSSticker`.

Sobald der neue Inhalt für das Feld für die Eingabe ist, kann der Benutzer zum Senden der Nachricht durch Tippen auf die blaue **senden** Schaltfläche (genauso wie jede typische Nachricht). Es gibt keine Möglichkeit für die Nachrichten-App-Erweiterung automatisch Inhalt zu senden, dieser Vorgang ist völlig unter der Kontrolle des Benutzers.

## <a name="handling-the-compact-and-expanded-modes"></a>Die kompakte und erweiterte Modi zur Behandlung

Eine Nachrichten-App-Erweiterung kann in einem der zwei unterschiedliche Ansichtsmodi angezeigt werden:

[![](advanced-message-app-extensions-images/interactive08.png "Eine Nachrichten-App-Erweiterung in zwei unterschiedliche Ansichtsmodi angezeigt: Kompakte und erweiterte")](advanced-message-app-extensions-images/interactive08.png#lightbox)

- **Compact** – Dies ist der Standardmodus, in denen die unteren 25 %, der die Ansicht "Nachricht" beansprucht die Nachrichten-App-Erweiterung. In Komprimierungsmodus besitzt die app Zugriff auf die Tastatur, horizontalen Bildlauf oder Streifen Geste Erkennungen nicht. Die app verfügt über Zugriff auf das Feld für die Eingabe und Aufrufe von `InsertMessage` wird sofort, die es dem Benutzer angezeigt werden.
- **Erweitert** -App-Erweiterung für die Nachricht füllt die gesamte Nachricht-Ansicht. Es hat keinen Zugriff auf das Feld für die Eingabe, aber keinen Zugriff auf die Tastatur, horizontales scrollen und Wischen Geste Erkennungen.

Eine Nachrichten-App-Erweiterung sollte gewechselt werden, zwischen diesen Modi entweder programmgesteuert oder manuell vom Benutzer zu einem beliebigen Zeitpunkt und sofort reagieren auf eine in der Ansicht Modus ändert.

Sehen Sie sich im folgenden Beispiel der Wechsel zwischen den zwei unterschiedliche Ansichtsmodi zu verarbeiten. Zwei andere Ansichtscontroller werden für die einzelnen Zustände nachgewiesen. Die `StickerBrowserViewController` Handles der **Compact** anzeigen und die `AddStickerViewController` übernimmt die **erweitert** anzeigen:

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

Die `DidTransition` Methode wird überschrieben, um zwischen den beiden Modi wechseln zu behandeln:

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

Optional kann die app verwendet haben die `WillTransition` Methode, um das Ändern der Anzeige im Modus zu verarbeiten, bevor sie dem Benutzer angezeigt werden (wie im obigen Beispiel Icecream-Generator ausgeführt wird). Weitere Informationen finden Sie unserem [Weitere Aufkleber Anpassung](~/ios/platform/message-app-integration/intro-to-message-app-extensions.md) Dokumentation.

## <a name="replying-to-a-message"></a>Antworten auf eine Nachricht

Es gibt zwei Fälle, die eine Nachrichten-App-Erweiterung benötigen, um beim Antworten auf eine Nachricht zu verarbeiten:

[![](advanced-message-app-extensions-images/interactive09.png "Die Nachrichten-App-Erweiterung in den Modi aktiv und inaktiv")](advanced-message-app-extensions-images/interactive09.png#lightbox)

- **Erweiterung ist inaktiv** -einmal Sprechblasen für die Nachrichten-App-Erweiterung in der Nachricht-Aufzeichnung, die der Benutzer tippen kann, um die Erweiterungen zu aktivieren und interaktive Unterhaltung fortsetzen vorhanden ist.
- **Erweiterung wird aktiv** -Benutzer kann die Nachrichten-App-Erweiterung der Nachricht Blase wird in die Aufzeichnung der Nachricht, geben den Ansichtsmodus erweitert, und weiterhin den interaktiven Prozess aus, wo sie aufgehört tippen.

### <a name="the-extension-is-inactive"></a>Die Erweiterung ist inaktiv

Wenn eine Blase für die Nachricht vom Benutzer in der Nachricht Transkription getippt wird, und die Nachrichten-App-Erweiterung nicht aktiv ist, wird der folgende Prozess auftreten:

[![](advanced-message-app-extensions-images/interactive10.png "Eine inaktive Nachricht Blase behandeln")](advanced-message-app-extensions-images/interactive10.png#lightbox)

1. Der Benutzer tippt der Erweiterungs Nachricht Blase.
2. Wenn eine Erweiterung gestartet wird, wird die Nachricht-app einen Prozess gestartet.
3. Die `DidBecomeActive` Methode aufgerufen wird, und übergeben einen `MSConversation` , die die Konversation, die die Nachrichten-App-Erweiterung, im ausgeführt wird darstellt.
4. Da die Erweiterung der basiert eine `UIViewController` sowohl `ViewWillAppear` und `ViewDidAppear` aufgerufen werden.

Wenn der Prozess abgeschlossen ist, wird die Nachricht-App-Erweiterung im Anzeigemodus erweitert angezeigt.

### <a name="the-extension-is-active"></a>Die Erweiterung ist aktiv

Wenn eine Blase für die Nachricht vom Benutzer in der Nachricht Transkription getippt wird, und die Nachrichten-App-Extension aktiv ist, wird der folgende Prozess auftreten:

[![](advanced-message-app-extensions-images/interactive11.png "Behandeln von einer aktiven Nachricht Blase")](advanced-message-app-extensions-images/interactive11.png#lightbox)

1. Der Benutzer tippt der Erweiterungs Nachricht Blase.
2. Da die Nachrichten-App-Erweiterung bereits aktiv ist, wird die `WillTransition` -Methode der der `MSMessagesAppViewController` wird aufgerufen, um das Verarbeiten der Wechsel von der Compact zu den Anzeigemodus erweitert.
3. Die `DidSelectMessage` Methode der `MSMessagesAppViewController` aufgerufen wird, und übergeben die `MSMessage` und `MSConversation` , zu der die Blase für die Nachricht gehört.
4. Die `DidTransition` Methode der `MSMessagesAppViewController` wird aufgerufen, um das Verarbeiten der Wechsel von der Compact zu den Anzeigemodus erweitert.

In diesem Fall, wenn der Prozess abgeschlossen ist, wird die Nachricht-App-Erweiterung im Anzeigemodus erweitert angezeigt.

### <a name="accessing-the-selected-message"></a>Zugreifen auf die ausgewählte Nachricht

In beiden Fällen, wenn der Benutzer eine Nachricht Blase, um die Nachrichten-App-Erweiterung, gehören tippt sie müssen für den Zugriff auf die `MSMessage` mit angetippt wurde die `SelectedMessage` Eigenschaft der `MSConversation`.

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

Die ausgewählte Nachricht in die Nachrichten-App-Erweiterung der Benutzeroberfläche angezeigt werden soll, und der Benutzer sollte ermöglicht werden, um eine Antwort zu erstellen.

## <a name="removing-partially-completed-messages"></a>Teilweise Nachrichten abgeschlossen

Beim Senden von der einzelnen Schritten eine interaktive Konversation zwischen zwei des Benutzers in der Konversation, können die teilweise abgeschlossene Sprechblasen starten, um die Aufzeichnung der Nachricht mit überflüssigen Daten gefüllt:

[![](advanced-message-app-extensions-images/interactive12.png "Die teilweise abgeschlossene Sprechblasen können viele Realisierungslinks enthalten das Message-Protokoll")](advanced-message-app-extensions-images/interactive12.png#lightbox)

Stattdessen sollten die Nachrichten-App-Erweiterung der vorherigen Sprechblasen in einem kurzen Kommentar in die Aufzeichnung der Nachricht zu reduzieren:

[![](advanced-message-app-extensions-images/interactive13.png "Reduzieren die vorherige Nachrichtenblasen in der Aufzeichnung der Nachricht")](advanced-message-app-extensions-images/interactive13.png#lightbox)

Dies erfolgt mithilfe einer `MSSession` so reduzieren Sie alle vorhandenen Schritte. Daher `DidSelectMessage` Methode der `MSMessagesAppViewController` Klasse kann geändert werden, um Sie wie folgt aussehen:

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

Wenn die ausgewählte Nachricht bereits eine verlassen hat `MSSession`, es wird eine neue anderen verwendet `MSSession` erstellt wird. Die `SummaryText` Eigenschaft der `MSMessage` wird verwendet, um eine Beschriftung reduzierten Schritten hinzufügen. Wenn die `SummaryText` -Eigenschaftensatz auf `null`, die vorherigen Schritte in der Konversation werden vollständig über die Aufzeichnung für die Konversation entfernt.

## <a name="advanced-message-api-features"></a>Erweiterte Funktionen für Nachrichten-API

Untersuchen Sie mit den grundlegenden Funktionen der neuen Nachricht API oben ausführlich behandelt als Nächstes einige erweiterte Features, die Apple in das Framework erstellt wurde.

Einerseits bestehen verschiedene andere überschreiben Sie Methoden in der `MSMessagesAppViewController` Klasse, die einen tieferen Zugriff auf die Konversation bereitstellen:

- `DidStartSendingMessage` -Wird aufgerufen, wenn der Benutzer auf die Schaltfläche "Senden" tippt. Dies bedeutet nicht, dass die Nachricht tatsächlich an den Empfänger, die nur wurde, Sendevorgang gestartet wurde.
- `DidCancelSendingMessage` -Dies geschieht, wenn der Benutzer tippt der *X* -Schaltfläche in der oberen rechten Ecke der Nachricht Blase wird in das Protokoll für die Konversation.
- `DidReceiveMessage` – Diese Methode wird aufgerufen, wenn die Nachrichten-App-Extension aktiv ist ein eine neue Nachricht wurde von einer der Teilnehmer der Konversation empfangen.

### <a name="group-conversations"></a>Gruppenunterhaltungen

Eine Nachrichten-App-Erweiterung kann verwendet werden, während Benutzer gruppenunterhaltungen (mit 3 oder mehr Personen) beteiligt sind, und dies beim Entwerfen und implementieren eine App-Erweiterung von Nachrichten berücksichtigt werden müssen.

Sehen Sie sich die folgenden Interaktion in einer Konversation Gruppe mit drei Benutzer an:

[![](advanced-message-app-extensions-images/interactive14.png "Interaktion in einer Konversation Gruppe mit drei Benutzer")](advanced-message-app-extensions-images/interactive14.png#lightbox)

1. Benutzer 1 sendet eine interaktive Nachricht stellen Benutzer 2 und 3 für Benutzer eine Burger kombinieren ganz.
2. Benutzer 2 wählt Tomaten.
3. Benutzer 3 wählt Pickles.
4. Sowohl Benutzer 2 und 3 der Benutzer im Auswahlmöglichkeiten eingehen an Benutzer 1 zur nahezu gleichen Zeit. Daher wird Benutzer 2 die Auswahl in eine Zusammenfassungszeile reduziert, und es ist nicht verfügbar. Dieser Fall kann haben auch wurde gekippt, mithilfe des User-2-Auswahl angezeigt werden und Benutzer 3 reduziert wird der.

Dieses Verhalten ist in beiden Fällen unerwünschte, wie Benutzer 1 Auswahl sowohl Benutzer 2 und 3 Benutzer zugreifen können sollen. Um diese Situation zu behandeln, Apple, dass die Nachrichten-App-Erweiterung speichert den Status der in der Cloud, und verwenden die URL-Eigenschaft des schlägt die `MSMessage` (, ruft zwischen den Benutzern gesendet) auf diesen Zustand zugreifen.

Wenn der Benutzer eine Nachricht sendet, wird ein Sitzungstoken generiert und in die Cloud mit dem aktuellen Nachrichtenstatus übertragen. Wenn ein Benutzer auf eine Blase Nachricht in der Konversation Transkription tippt, wird das Sitzungstoken, das verwendet, um den Status der aktuellen Sitzung aus der Cloud abzurufen.

### <a name="sender-identifiers"></a>Senderbezeichner

Um den Zugriff auf die ID des Absenders einer Nachricht zu besprechen, betrachten Sie beispielsweise eine gruppenunterhaltung oben angegebenen:

[![](advanced-message-app-extensions-images/interactive15.png "Senden von Bezeichnern gruppenunterhaltung")](advanced-message-app-extensions-images/interactive15.png#lightbox)

1. In diesem Fall Benutzer 1 sendet eine interaktive Nachricht stellen Benutzer 2 und 3 für Benutzer eine Burger kombinieren ganz.
2. Benutzer 3 wählt Pickles.
3. Benutzerauswahl 3 eingeht, an die Benutzer 1 und 2 der Benutzer hat noch nicht geantwortet.
4. Da Apple Bedenken hinsichtlich der Privatsphäre der Benutzer ist, weiß der Nachrichten-App-Erweiterung nur über einen eindeutigen Bezeichner (als eine `NSUUID`), die jeder Teilnehmer der Konversation zugewiesen ist. Auf dem lokalen Gerät wird nur der aktuelle Benutzer-ID bezeichnet.
5. Die `MSMessage` verfügt über eine `SenderIdentifier` Eigenschaft, die einer der Benutzernamen übereinstimmt, das in der Liste der Teilnehmer bekannt für die Erweiterung des.
6. Jedes Benutzergerät verfügt über eine eigene Kopie der Liste der Teilnehmer, wobei in diesem Fall nur der lokale Bezeichner des Benutzers bekannt ist.
7. Wenn eine Nachricht gesendet wird, dessen `SenderIdentifier` Eigenschaft auch auf denjenigen des lokalen Benutzers bekannt ist.

Die Absender-Bezeichner kann auf folgende Weise verwendet werden:

- Anhand der Liste der Teilnehmer kann die Anzahl der Benutzer die Erweiterung in der Konversation abgerufen werden.
- Wenn die Erweiterung eine Nachricht von einem Benutzer empfängt, sie können die Sender-ID des nachvollziehen. Empfängt er eine andere Meldung mit dem gleichen Sender-Bezeichner, weiß die Erweiterung an, dass es vom gleichen Benutzer ist.
- Sie können verwendet werden, um zu einen bestimmten Benutzer in der Konversation zu identifizieren.

Die Sender-ID kann verwendet werden, in den Feldern Text, der die `MSMessageTemplateLayout` werden sie mit einem Dollarzeichen (`$`). Zum Beispiel:

```csharp
// Pass along the sender identifier
var layout = new MSMessageTemplateLayout()
layout.Caption = string.format("${0} wants pickles.",Conversation.LocalParticipantIdentifier.UuidString);
```

Wenn eine Nachricht Blase mit dieser Art der Formatierung von Nachrichten-app angezeigt wird, wird es ersetzt das `$uuid...` durch den Namen wenden Sie sich an die Teilnehmer der Konversation, die Nachricht gesendet.

Die Sender-ID ist auf jedes Gerät eindeutig, daher betrachten im Diagramm oben in diesem Fall Beachten Sie, dass Benutzer-1-Gerät und Benutzer 3-Gerät eine andere eindeutige Absender-ID für jeden Teilnehmer der Konversation.

Die Absender-Bezeichner beziehen sich auf die Installation von App-Erweiterung der Nachricht. Also wenn ein Benutzer deinstalliert und neu installiert die Nachrichten-App-Erweiterung werden wird neue Absender-Bezeichner für die neue Installation generiert.

Die Absender-Bezeichner für den Zugriff auf können die Erweiterung über den folgenden Code verwenden:

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

Die interaktive Nachrichten von einer Nachrichten-App-Erweiterung generiert, wird auf den folgenden Apple-Plattformen übertragen werden:

- WatchOS 3
- macOS Sierra
- iOS 10

Drei Plattformen wird nur iOS 10 der Benutzer eine interaktive Nachricht generieren kann. Unter MacOS Sierra, klickt der Benutzer auf eine Blase für interaktive Nachricht, die URL angefügt die `MSMessage` in Safari geöffnet und eine Darstellung der Nachricht sollte es angezeigt werden.

Auf WatchOS-Nachrichten-app können Handoff auf einem angeschlossenen iOS-Gerät, in denen der Benutzer auf eine Antwort zusammenstellen können, eine interaktive Nachricht.

Die neue Nachrichten-API hat Unterstützung für ein Fallback auf älteren Apple-Plattformen wird der interaktive Nachricht empfangen:

- WatchOS 2 und höher
- OS X 10.11 +
- iOS 9 +

Sie werden in einem fallback-Format als zwei separate Nachrichten übermittelt werden:

- Eine werden das Bild, das Layout der Vorlage bereitgestellt.
- Die andere werden wie in der URL der `MSMessage`.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel verfügt über erweiterte Techniken für die Arbeit mit Nachrichten-App-Erweiterungen in einer Xamarin.iOS-Projektmappe, die integriert werden, enthält die **Nachrichten** app "und" vorhanden, neue Funktionen für den Benutzer.


## <a name="related-links"></a>Verwandte Links

- [Eiskrem-Generator (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/)
- [Nachrichten-Referenz](https://developer.apple.com/reference/messages)
- [Programmierhandbuch für App-Erweiterung](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)
