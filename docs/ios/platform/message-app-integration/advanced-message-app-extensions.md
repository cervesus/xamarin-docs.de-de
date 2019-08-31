---
title: Erweiterte Message-App-Erweiterungen in xamarin. IOS
description: In diesem Artikel werden erweiterte Techniken zum Arbeiten mit Message-App-Erweiterungen in einer xamarin. IOS-Projekt Mappe erläutert, die in die Nachrichten-APP integriert wird und dem Benutzer neue Funktionen präsentiert.
ms.prod: xamarin
ms.assetid: 394A1FDA-AF70-4493-9B2C-4CFE4BE791B6
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: e55e6d908bbeb9b4b42ccbcad8121a1b410b79af
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2019
ms.locfileid: "70200130"
---
# <a name="advanced-message-app-extensions-in-xamarinios"></a>Erweiterte Message-App-Erweiterungen in xamarin. IOS

_In diesem Artikel werden erweiterte Techniken zum Arbeiten mit Message-App-Erweiterungen in einer xamarin. IOS-Projekt Mappe erläutert, die in die Nachrichten-APP integriert wird und dem Benutzer neue Funktionen präsentiert._


Neu bei IOS 10. eine Erweiterung der Nachrichten-APP wird in die **Nachrichten** -app integriert und stellt dem Benutzer neue Funktionen zur Anwendung. Die Erweiterung kann Text, Aufkleber, Mediendateien und interaktive Nachrichten senden.

## <a name="about-message-app-extensions"></a>Informationen zu Message-App-Erweiterungen

Wie bereits erwähnt, ist eine Erweiterung der Nachrichten-APP in die **Nachrichten** -app integriert und stellt dem Benutzer neue Funktionen zur Anwendung. Die Erweiterung kann Text, Aufkleber, Mediendateien und interaktive Nachrichten senden. Es sind zwei Typen von Message App-Erweiterungen verfügbar:

- **Aufkleber Packs** : enthält eine Auflistung von Aufkleber, die der Benutzer einer Nachricht hinzufügen kann. Aufkleber Packs können erstellt werden, ohne Code zu schreiben.
- **IMESS Age-App** : kann eine benutzerdefinierte Benutzeroberfläche innerhalb der Nachrichten-APP zum Auswählen von Stickern, zum Eingeben von Text, einschließlich Mediendateien (mit optionalen Typkonvertierungen) und zum Erstellen, bearbeiten und Senden von Interaktions Nachrichten darstellen.

Erweiterungen von Message apps stellen drei Hauptinhalts Typen bereit:

- **Interaktive Nachrichten** : handelt es sich um einen Typ von benutzerdefiniertem Nachrichten Inhalt, der von einer APP generiert wird, wird die APP im Vordergrund gestartet, wenn der Benutzer auf die Nachricht tippt.
- **Aufkleber** : Bilder, die von der APP generiert werden und in die zwischen Benutzern gesendeten Nachrichten eingeschlossen werden können. Ein Beispiel für die Implementierung einer Aufkleber Pack-App finden Sie in unserer Beispiel-App für den [Ice Cream Builder](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-icecreambuilder) .
- **Anderer unterstützter Inhalt** : die APP kann Inhalte wie Fotos, Videos, Text oder Links des Typs bereitstellen, der von der Nachrichten-APP immer unterstützt wird.

Neu bei IOS 10, die Nachrichten-APP enthält jetzt einen eigenen dedizierten, integrierten App Store. Alle apps, die Erweiterungen für nachrichtenapps enthalten, werden in diesem Speicher angezeigt und höher gestuft. Die neue Nachrichten-APP-Schublade zeigt alle apps an, die aus dem Nachrichten-App-Store heruntergeladen wurden, um schnellen Zugriff auf die Benutzer zu ermöglichen.

Zusätzlich zu IOS 10 hat Apple eine Inline-App-Zuordnung hinzugefügt, die es dem Benutzer ermöglicht, eine APP leicht zu finden. Wenn ein Benutzer beispielsweise Inhalt von einer APP an eine andere sendet, die der zweite Benutzer nicht installiert hat (z. b. ein Aufkleber), wird der Name der sendenden app unter dem Inhalt im Nachrichten Verlauf aufgeführt. Wenn der Benutzer auf den Namen der APP tippt, wird der Nachrichten-APP-Speicher geöffnet, und die APP wird im Store ausgewählt.

Die Erweiterungen von Message apps ähneln vorhandenen IOS-apps, die der Entwickler erstellt hat, und haben Zugriff auf alle Standard Frameworks und Features einer standardmäßigen IOS-app. Beispiel:

- Sie haben Zugriff auf in-App-Käufe.
- Sie haben Zugriff auf Apple Pay.
- Sie haben Zugriff auf Gerätehardware, z. b. die Kamera.

Erweiterungen von Message apps werden nur auf IOS 10 unterstützt. der Inhalt, den diese Erweiterungen senden, kann jedoch auf watchos-und macOS-Geräten angezeigt werden. Die neue _Seite "Recents_ ", die watchos 3 hinzugefügt wird, zeigt aktuelle Aufkleber an, die vom Telefon gesendet wurden, einschließlich der Informationen aus den Erweiterungen von Message apps, und ermöglicht es dem Benutzer, diese Aufkleber von der Überwachung zu senden.

## <a name="about-interactive-messages"></a>Informationen zu interaktiven Nachrichten

Interaktive Nachrichten zeigen eine benutzerdefinierte Nachrichten Blase an und werden durch eine Erweiterung der Nachrichten-APP bereitgestellt. Sie ermöglichen es dem Benutzer, interaktiven Nachrichten Inhalt zu erstellen, ihn in das Eingabefeld der Nachricht einzufügen und ihn zu senden.

[![](advanced-message-app-extensions-images/interactive01.png "Erstellen von interaktiven Nachrichten Inhalten")](advanced-message-app-extensions-images/interactive01.png#lightbox)

Der empfangende Benutzer kann auf eine interaktive Nachricht antworten, indem er auf die Meldungs Blase im Nachrichten Verlauf tippt, um die erstellte App-Erweiterung zu laden. Die Erweiterung wird voll Bildschirm gestartet und ermöglicht dem Benutzer das Verfassen einer Antwort und das Zurücksenden an den ursprünglichen Benutzer.

[![](advanced-message-app-extensions-images/interactive02.png "Die Vollbild-Erweiterung wurde gestartet.")](advanced-message-app-extensions-images/interactive02.png#lightbox)


Die folgenden Themen werden im folgenden ausführlich behandelt:

- API-Übersicht
- Der Lebenszyklus der Erweiterung
- Verfassen einer Nachricht
- Senden einer Nachricht

## <a name="messages-api-overview"></a>API-Übersicht

Wenn Sie vom Benutzer aufgerufen wird, wird unten im Nachrichten Verlauf im Compact-Ansichtsmodus eine APP-Erweiterung für Nachrichten angezeigt:

[![](advanced-message-app-extensions-images/interactive03.png "API-Übersicht")](advanced-message-app-extensions-images/interactive03.png#lightbox)

1. Das `MSMessageAppViewController` -Objekt in der Message-App-Erweiterung ist die Hauptklasse, die aufgerufen wird, wenn der Benutzer die Ansicht der Erweiterung angezeigt wird.
2. Die Konversation wird dem Benutzer als `MSConversation` Objektinstanz präsentiert.
3. Die `MSMessage` -Klasse stellt eine angegebene Nachrichten Blase in der Konversation dar.
4. `MSSession`steuert, wie eine Nachricht gesendet wird.
5. `MSMessageTemplateLayout`steuert, wie die Meldung angezeigt wird.

## <a name="the-extension-lifecycle"></a>Der Lebenszyklus der Erweiterung

Sehen Sie sich an, wie eine Nachricht-App-Erweiterung aktiv wird:

[![](advanced-message-app-extensions-images/interactive04.png "Der Prozess, bei dem eine Erweiterung der Nachrichten-APP aktiv wird.")](advanced-message-app-extensions-images/interactive04.png#lightbox)

1. Wenn eine Erweiterung (z. b. aus der APP-Anzeige) gestartet wird, startet die Nachrichten-APP einen Prozess.
2. Die `DidBecomeActive` -Methode wird aufgerufen und an `MSConversation` eine übermittelt, die die Konversation darstellt, in der die APP-Erweiterung der Nachricht ausgeführt wird.
3. Da die Erweiterung auf einem `UIViewController` basiert, wird sowohl `ViewWillAppear` als auch `ViewDidAppear` aufgerufen.

Betrachten Sie als nächstes den Prozess, bei dem eine Erweiterung der Nachrichten-APP deaktiviert wird:

[![](advanced-message-app-extensions-images/interactive05.png "Der Prozess, bei dem eine Erweiterung der Nachrichten-APP deaktiviert wird")](advanced-message-app-extensions-images/interactive05.png#lightbox)

1. Wenn die APP-Erweiterung der Nachricht deaktiviert wird, `ViewWillDisappear` wird die-Methode zuerst aufgerufen.
2. Anschließend wird `ViewDidDisappear` die-Methode aufgerufen.
3. Die `WillResignActive` -Methode wird aufgerufen und an `MSConversation` eine übermittelt, die die Konversation darstellt, in der die APP-Erweiterung der Nachricht ausgeführt wird. An diesem Punkt werden die Verbindungen zwischen der Nachrichten-APP und der Erweiterung veröffentlicht.
4. Zu einem späteren Zeitpunkt wird der Prozess von der Nachrichten-APP beendet.

Da es sich bei einer Erweiterung um einen kurzlebigen Prozess handelt, wird Sie vom System aggressiv beendet, um Verarbeitungs-und Akkuleistung zu sparen. Der Entwickler sollte dies beim Entwerfen und Implementieren einer Erweiterung der Nachrichten-APP berücksichtigen.

## <a name="composing-a-message"></a>Verfassen einer Nachricht

Sobald die Erweiterung der APP-Erweiterung in einem Prozess ausgeführt wird und die zugehörige Benutzeroberfläche angezeigt wird, kann der folgende Code verwendet werden, um eine neue Nachricht zu erstellen:

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

Mit diesem Code wird ein `MSMessage` neuer erstellt und mehrere Eigenschaften festgelegt `Url`(z. b.). Obwohl die Nachricht nur unter IOS erstellt werden kann, kann Sie an IOS und macOS gesendet werden, um angezeigt zu werden.

Wenn der Benutzer in der Konversation unter macOS auf die Meldungs Blase klickt, versucht der Mac, die in der URL angegebene Adresse im Webbrowser zu öffnen. Folglich sollte die Website des Entwicklers in der Lage sein, eine Darstellung der Nachricht im Webbrowser auf macOS-basierten Computern anzuzeigen.

Die `AccessibilityLabel` -Eigenschaft wird von Bildschirmlesern verwendet, um die Aufzeichnung der Konversation für den Benutzer zu lesen. Die `Layout` -Eigenschaft gibt an, wie die Meldung angezeigt wird, derzeit `MSMessageTemplateLayout` wird nur der unterstützt und sieht wie folgt aus:

[![](advanced-message-app-extensions-images/interactive06.png "Die msmessagetemplatelayout-Vorlage")](advanced-message-app-extensions-images/interactive06.png#lightbox)

Die `Image` -Eigenschaft `MSMessageTemplateLayout` des-Objekts stellt Inhalt für den Haupttext der messageblase auf dem Bildschirm bereit. Die `MediaFileUrl` -Eigenschaft stellt auch Inhalt für den Text der Nachrichten Blase bereit. Sie ermöglicht jedoch Inhalte, die nicht von `UIImage` unterstützt werden (z. b. eine Videodatei, die eine Schleife im Hintergrund durchführt). Wenn sowohl die `Image` - `MediaFileUrl` Eigenschaft als auch die- `Image` Eigenschaft bereitgestellt werden, hat die-Eigenschaft Vorrang. `MediaFileUrl` Unterstützt PNG, JPEG, GIF und Video (in jedem Format, das von den Media Player Framework)-Medienformaten wiedergegeben werden kann.

Die empfohlene Mediengröße beträgt 300 x 300 Pixel bei der 3X-Auflösung. Etwas größere und kleinere Ressourcen werden ebenfalls akzeptiert, und Apple schlägt Tests mit unterschiedlichen Größen vor, um die besten Ergebnisse zu erzielen. Die Nachrichten-APP wird herunterskaliert und skaliert diese Medien nach Bedarf.

Wenn die Assets an den Empfänger gesendet werden, werden alle angeschlossenen Medien automatisch von der Nachrichten-APP transcodiert, um Sie von der Übertragung über die Netzwerke zu optimieren. Aus diesem Grund wird von Apple das Einschließen von Text in das an die Nachricht angefügte Medium verhindert, da die Medien für die Übertragung herunterskaliert und komprimiert werden, wodurch der Text möglicherweise nicht mehr lesbar ist.

Die `ImageTitle` Eigenschaften `ImageSubtitle` und geben eine Beschreibung für das Medium an, das in der Meldungs Blase angezeigt wird. Diese Eigenschaften werden als Text an das empfangende Gerät gesendet, wo Sie in der unteren linken Ecke des Bilds mit einer Skala gerendert werden.

Die `Caption`Eigenschaften `SubCaption` ,und`TrailingSubcaption`beschreiben das Bild weiter und werden in einem Abschnitt unterhalb des Bilds gerendert. `TrailingCaption` Wenn Sie alle diese Eigenschaften auf `null` festlegen, wird eine Nachrichten Blase ohne den Beschriftungs Bereich erstellt:

[![](advanced-message-app-extensions-images/interactive07.png "Eine Meldungs Blase ohne den Beschriftungs Bereich.")](advanced-message-app-extensions-images/interactive07.png#lightbox)

Beachten Sie, dass die Nachrichten-APP das Symbol der APP-Erweiterung der Nachricht in der oberen linken Ecke der Nachrichten Blase zeichnet.

## <a name="sending-a-message"></a>Senden einer Nachricht

Nachdem ein `MSMessage` erstellt wurde, kann der folgende Code verwendet werden, um ihn zu senden:

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

Die `ActiveConversation` -Eigenschaft `MSMessagesAppViewController` des-Objekts enthält die aktuelle Konversation, in der die Message-App-Erweiterung gestartet wurde.

Nennen Sie`MSConversation` die von,umdieNachrichtindieKonversationeinzubeziehen,undbehandelnSieggf.auftretendeFehler.`InsertMessage` Wenn die Nachricht erfolgreich eingeschlossen wurde, wird die Nachrichten Blase im Eingabefeld angezeigt.

Darüber hinaus kann die Erweiterung verschiedene Datentypen an die Konversation senden, z. b.:

- **Text** - `ActiveConversation.InsertText ("Message", (error) => {...});`
- **Loszulassen** - `ActiveConversation.InsertAttachment (new NSUrl ("path"), "filename", (error) => {...});`
- **Aufkleber**  -  ,bei`sticker` denen eine`MSSticker`ist.`ActiveConversation.InsertSticker (sticker, (obj) => {...});`

Sobald sich der neue Inhalt im Eingabefeld befindet, kann der Benutzer die Nachricht senden, indem er auf die blaue **Sende** Schaltfläche tippt (genauso wie eine beliebige typische Meldung). Es gibt keine Möglichkeit, dass die Nachrichten-APP-Erweiterung Inhalt automatisch sendet, da dieser Vorgang vollständig unter der Kontrolle des Benutzers ist.

## <a name="handling-the-compact-and-expanded-modes"></a>Verarbeiten der kompakten und erweiterten Modi

Eine Nachrichten-APP-Erweiterung kann in einem von zwei unterschiedlichen Ansichtsmodi angezeigt werden:

[![](advanced-message-app-extensions-images/interactive08.png "Eine nachrichtenapp-Erweiterung, die in zwei verschiedenen Ansichtsmodi angezeigt wird: Compact & erweitert")](advanced-message-app-extensions-images/interactive08.png#lightbox)

- **Compact** : Dies ist der Standardmodus, in dem die Nachrichten-APP-Erweiterung die untersten 25% der Meldungs Ansicht annimmt. Im Compact-Modus hat die APP keinen Zugriff auf die Tastatur, den horizontalen Bildlauf oder den Schwenk Gesten Erkennungs Modul. Die APP verfügt über Zugriff auf das Eingabefeld, und Aufrufe `InsertMessage` an werden dem Benutzer sofort angezeigt.
- **Erweitert** : die Nachrichten-APP-Erweiterung füllt die gesamte Meldungs Ansicht aus. Er hat keinen Zugriff auf das Eingabefeld, hat jedoch Zugriff auf die Tastatur, den horizontalen Bildlauf und die Dreh Gestenerkennung.

Eine Erweiterung der Nachrichten-APP kann zwischen diesen Modi jederzeit Programm gesteuert oder manuell vom Benutzer gewechselt werden und sollte sofort auf alle Änderungen im Ansichtsmodus reagiert werden.

Sehen Sie sich das folgende Beispiel für die Handhabung des Schalters zwischen den beiden unterschiedlichen Ansichtsmodi an. Für jeden Zustand sind zwei verschiedene Ansichts Controller erforderlich. Der `StickerBrowserViewController` verarbeitet die **Compact** -Ansicht und `AddStickerViewController` verarbeitet die **Erweiterte** Ansicht:

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

Die `DidTransition` -Methode wird überschrieben, um den Wechsel zwischen den beiden Modi zu handhaben:

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

Optional kann die APP die `WillTransition` -Methode verwenden, um die Ansichts Modusänderung zu verarbeiten, bevor Sie dem Benutzer angezeigt wird (wie dies im obigen Beispiel für das icecream Builder erfolgt). Weitere Informationen finden Sie in unserer weiteren Dokumentation zur [Aufkleber-Anpassung](~/ios/platform/message-app-integration/intro-to-message-app-extensions.md) .

## <a name="replying-to-a-message"></a>Antworten auf eine Nachricht

Es gibt zwei Fälle, in denen eine Nachrichten-APP-Erweiterung behandelt werden muss, wenn Sie auf eine Nachricht antwortet:

[![](advanced-message-app-extensions-images/interactive09.png "Die Nachrichten-APP-Erweiterung im inaktiven und aktiven Modus")](advanced-message-app-extensions-images/interactive09.png#lightbox)

- Die **Erweiterung ist inaktiv** . in der Nachrichten Aufzeichnung gibt es eine der Nachrichten Blasen der Nachricht, die der Benutzer tippen kann, um die Erweiterungen zu aktivieren und die interaktive Konversation fortzusetzen.
- Die **Erweiterung ist aktiv** . der Benutzer kann in der Nachrichten Aufzeichnung auf die Meldungs Blase der Message-App-Erweiterung tippen, um den erweiterten Ansichtsmodus einzugeben und den interaktiven Prozess von dort aus fortzusetzen, wo Sie aufgehört haben.

### <a name="the-extension-is-inactive"></a>Die Erweiterung ist inaktiv.

Wenn eine Nachrichten Blase vom Benutzer in der Nachrichten Aufzeichnung getippt wird und die APP-Erweiterung der Nachricht inaktiv ist, wird der folgende Prozess ausgeführt:

[![](advanced-message-app-extensions-images/interactive10.png "Verarbeiten einer inaktiven Nachrichten Blase")](advanced-message-app-extensions-images/interactive10.png#lightbox)

1. Der Benutzer tippt auf die Meldungs Blase der Erweiterung.
2. Wenn eine Erweiterung gestartet wird, startet die Message-APP einen Prozess.
3. Die `DidBecomeActive` -Methode wird aufgerufen und an `MSConversation` eine übermittelt, die die Konversation darstellt, in der die APP-Erweiterung der Nachricht ausgeführt wird.
4. Da die Erweiterung auf einem `UIViewController` basiert, wird sowohl `ViewWillAppear` als auch `ViewDidAppear` aufgerufen.

Wenn der Prozess beendet ist, wird die APP-Erweiterung der Nachricht im erweiterten Ansichtsmodus angezeigt.

### <a name="the-extension-is-active"></a>Die Erweiterung ist aktiv.

Wenn eine Nachrichten Blase vom Benutzer in der Nachrichten Aufzeichnung getippt wird und die APP-Erweiterung der Nachricht aktiv ist, wird der folgende Prozess ausgeführt:

[![](advanced-message-app-extensions-images/interactive11.png "Verarbeiten einer aktiven Nachrichten Blase")](advanced-message-app-extensions-images/interactive11.png#lightbox)

1. Der Benutzer tippt auf die Meldungs Blase der Erweiterung.
2. Da die Nachrichten-APP-Erweiterung bereits aktiv `WillTransition` ist, `MSMessagesAppViewController` wird die-Methode von aufgerufen, um den Wechsel von der Compact-zum erweiterten Ansichtsmodus zu verarbeiten.
3. Die `DidSelectMessage` -`MSMessage` Methode `MSMessagesAppViewController` des wird aufgerufen und übergeben, und `MSConversation` die Nachrichten Blase gehört zu.
4. Die `DidTransition` -Methode `MSMessagesAppViewController` des wird aufgerufen, um den Wechsel vom kompakten zum erweiterten Ansichtsmodus zu verarbeiten.

Wenn der Prozess beendet ist, wird die APP-Erweiterung der Nachricht im erweiterten Ansichtsmodus angezeigt.

### <a name="accessing-the-selected-message"></a>Zugreifen auf die ausgewählte Nachricht

In beiden Fällen, wenn der Benutzer auf eine Nachrichten Blase tippt, die zur Erweiterung der Nachrichten-APP gehört, muss er Zugriff auf `MSMessage` das erhalten, das mithilfe `SelectedMessage` der-Eigenschaft `MSConversation`des-Objekts getippt wurde.

Beispiel:

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

Die ausgewählte Meldung sollte in der Benutzeroberfläche der APP-Erweiterung der Nachricht angezeigt werden, und der Benutzer sollte die Möglichkeit haben, eine Antwort zu verfassen.

## <a name="removing-partially-completed-messages"></a>Entfernen teilweise abgeschlossener Nachrichten

Beim Senden der verschiedenen Schritte einer interaktiven Konversation zwischen den beiden Benutzern in der Konversation können die teilweise abgeschlossenen Nachrichten Blasen mit dem Überladen der Nachrichten Aufzeichnung beginnen:

[![](advanced-message-app-extensions-images/interactive12.png "Die teilweise abgeschlossenen Nachrichten Blasen können das Nachrichten Transkripts gruppieren.")](advanced-message-app-extensions-images/interactive12.png#lightbox)

Stattdessen sollte die APP-Erweiterung der Nachricht die vorherigen Nachrichten Blasen in einen präktkommentar in der Nachrichten Aufzeichnung reduzieren:

[![](advanced-message-app-extensions-images/interactive13.png "Reduzieren der vorherigen Nachrichten Blasen in der Nachrichten Aufzeichnung")](advanced-message-app-extensions-images/interactive13.png#lightbox)

Dies wird mithilfe `MSSession` von gehandhabt, um alle vorhandenen Schritte zu reduzieren. Daher kann `DidSelectMessage` die-Methode `MSMessagesAppViewController` der-Klasse so geändert werden, dass Sie wie folgt aussieht:

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

Wenn für die ausgewählte Nachricht bereits eine `MSSession`vorhanden ist, wird Sie verwendet, da eine neue `MSSession` erstellt wird. Die `SummaryText` -Eigenschaft `MSMessage` von wird verwendet, um den reduzierten vorherigen Schritten eine Beschriftung hinzuzufügen. Wenn die `SummaryText` -Eigenschaft auf `null`festgelegt ist, werden die vorherigen Schritte in der Konversation vollständig aus dem Konversation-Transkripts entfernt.

## <a name="advanced-message-api-features"></a>Erweiterte Message-API-Features

Mit den grundlegenden Features der neuen Nachrichten-API, die oben ausführlich behandelt werden, untersuchen Sie als nächstes einige der erweiterten Features, die Apple in das Framework integriert hat.

Erstens gibt es mehrere weitere Überschreibungs Methoden in der `MSMessagesAppViewController` -Klasse, die einen tieferen Zugriff auf die Konversation bieten:

- `DidStartSendingMessage`-Dies wird aufgerufen, wenn der Benutzer auf die Schaltfläche "Senden" tippt. Dies bedeutet nicht, dass die Nachricht tatsächlich an den Empfänger gesendet wurde, nur dass der Sende Prozess gestartet wurde.
- `DidCancelSendingMessage`-Dies ist der Fall, wenn der Benutzer auf die *X* -Schaltfläche in der oberen rechten Ecke der Nachrichten Blase in der Konversation der Konversation tippt.
- `DidReceiveMessage`-Diese Methode wird aufgerufen, wenn die APP-Erweiterung der Nachricht aktiv ist, und eine neue Nachricht wurde von einem der Teilnehmer der Konversation empfangen.

### <a name="group-conversations"></a>Gruppen Konversationen

Eine Nachrichten-APP-Erweiterung kann verwendet werden, während die Benutzer an Gruppen Konversationen (mit drei oder mehr Personen) beteiligt sind. Dies muss beim Entwerfen und Implementieren einer Erweiterung der Nachrichten-APP berücksichtigt werden.

Sehen Sie sich die folgende Interaktion in einer Gruppen Konversation mit drei Benutzern an:

[![](advanced-message-app-extensions-images/interactive14.png "Interaktion in einer Gruppen Konversation mit drei Benutzern")](advanced-message-app-extensions-images/interactive14.png#lightbox)

1. Benutzer 1 sendet eine interaktive Gruppen Nachricht, in der Benutzer 2 und Benutzer 3 aufgefordert werden, ein Burger-Topping auszuwählen.
2. Benutzer 2 wählt Tomaten aus.
3. Benutzer 3 wählt Pickles aus.
4. Die Auswahl von Benutzer 2 und Benutzer 3 wird zur gleichen Zeit wieder bei Benutzer 1 erreicht. Daher wird die Auswahl von Benutzer 2 in eine Zusammenfassungs Zeile reduziert und ist nicht verfügbar. Dieser Fall könnte auch gekippt werden, wenn Benutzer 2 angezeigt wird und Benutzer 3 reduziert wird.

In beiden Fällen ist dieses Verhalten nicht erwünscht, da Benutzer 1 in der Lage sein sollte, auf Benutzer-2 und Benutzer 3 zuzugreifen. Um diese Situation zu beheben, schlägt Apple vor, dass die APP-Erweiterung der Nachricht den Nachrichten Status in der Cloud speichert und die URL `MSMessage` -Eigenschaft der (die zwischen den Benutzern gesendet wird) für den Zugriff auf diesen Status verwendet.

Wenn der Benutzer eine Nachricht sendet, wird ein Sitzungs Token generiert und mit dem aktuellen Nachrichten Zustand in die Cloud übermittelt. Wenn ein Benutzer in der Konversation auf eine Nachrichten Blase tippt, wird das Sitzungs Token verwendet, um den aktuellen Sitzungs Status aus der Cloud abzurufen.

### <a name="sender-identifiers"></a>Absender Bezeichner

Um auf den Bezeichner des Absenders einer Nachricht zuzugreifen, sehen Sie sich das Beispiel für eine Gruppen Konversation an:

[![](advanced-message-app-extensions-images/interactive15.png "Gruppen Konversation sendende Bezeichner")](advanced-message-app-extensions-images/interactive15.png#lightbox)

1. Erneut sendet Benutzer 1 eine interaktive Gruppen Nachricht, die Benutzer 2 und Benutzer 3 auffordert, eine Burger-Topping-Topping auszuwählen.
2. Benutzer 3 wählt Pickles aus.
3. Die Auswahl von Benutzer 3 geht zurück an Benutzer 1, und Benutzer 2 hat noch nicht geantwortet.
4. Da Apple mit dem Datenschutz in hohem Umfang beschäftigt ist, kennt die APP-Erweiterung der Nachricht nur einen eindeutigen `NSUUID`Bezeichner (als), der jedem Teilnehmer der Konversation zugewiesen wird. Auf dem lokalen Gerät ist nur der Bezeichner des aktuellen Benutzers bekannt.
5. Der `MSMessage` verfügt über `SenderIdentifier` eine-Eigenschaft, die mit einem der Benutzer in der Teilnehmerliste übereinstimmt, die der Erweiterung bekannt ist.
6. Jedes Benutzergerät verfügt über eine eigene Kopie der Teilnehmerliste, in der wiederum nur der Bezeichner des lokalen Benutzers bekannt ist.
7. Wenn eine Nachricht gesendet wird, `SenderIdentifier` wird die zugehörige-Eigenschaft auch als der lokale Benutzer bezeichnet.

Die Absender Bezeichner können wie folgt verwendet werden:

- Durch die Betrachtung der Teilnehmerliste kann die Erweiterung die Anzahl der Benutzer in der Konversation erhalten.
- Wenn die Erweiterung eine Nachricht von einem Benutzer empfängt, kann Sie den Absender Bezeichner nachverfolgen. Wenn eine andere Nachricht mit dem gleichen Absender Bezeichner empfangen wird, weiß die Erweiterung, dass Sie vom gleichen Benutzer ist.
- Sie können verwendet werden, um einen bestimmten Benutzer in der Konversation zu identifizieren.

Der Absender Bezeichner kann in beliebigen Textfeldern von `MSMessageTemplateLayout` verwendet werden, indem er einem Dollarzeichen (`$`) vorangestellt wird. Beispiel:

```csharp
// Pass along the sender identifier
var layout = new MSMessageTemplateLayout()
layout.Caption = string.format("${0} wants pickles.",Conversation.LocalParticipantIdentifier.UuidString);
```

Wenn die Nachrichten-APP eine Meldungs Blase mit dieser Art von Formatierung anzeigt, ersetzt `$uuid...` Sie die durch den Kontaktnamen des Teilnehmers in der Konversation, der die Nachricht gesendet hat.

Die Absender-ID ist auf jedem Gerät eindeutig. Beachten Sie daher, dass das Gerät des Benutzers 1 und das Gerät von Benutzer 1 für jeden Teilnehmer der Konversation über einen anderen eindeutigen Absender Bezeichner verfügen.

Die Absender Bezeichner sind auf die Installation der Message-App-Erweiterung beschränkt. Wenn ein Benutzer die Erweiterung der Nachrichten-APP-Erweiterung deinstalliert und neu installiert, werden die neuen Absender Bezeichner für die neue Installation generiert.

Für den Zugriff auf die Absender Bezeichner kann die Erweiterung den folgenden Code verwenden:

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

Die interaktiven Nachrichten, die von einer Erweiterung für die Nachrichten-APP generiert werden, werden auf den folgenden Apple-Plattformen

- watchos 3
- macOS Sierra
- iOS 10

Von den drei Plattformen ermöglicht nur IOS 10 dem Benutzer das Generieren einer interaktiven Nachricht. Wenn der Benutzer auf macOS Sierra auf eine interaktive Nachrichten Blase klickt, wird die an den `MSMessage` angefügte URL in Safari geöffnet, und dort sollte eine Darstellung der Nachricht angezeigt werden.

Bei watchos kann die Nachrichten-APP eine interaktive Nachricht an ein angefügtes IOS-Gerät weiter senden, auf dem der Benutzer eine Antwort verfassen kann.

Die neue Nachrichten-API unterstützt ein Fall Back, wenn die interaktive Nachricht auf älteren Apple-Plattformen empfangen wird:

- watchos 2 und höher
- OS X 10,11 +
- iOS 9 +

Sie werden in einem Fall Back Format als zwei separate Nachrichten übermittelt:

- Eine davon ist das Bild, das vom Vorlagen Layout bereitgestellt wird.
- Die andere ist die URL, die in der `MSMessage`bereitgestellt wird.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden erweiterte Techniken zum Arbeiten mit Message-App-Erweiterungen in einer xamarin. IOS-Lösung vorgestellt, die in die **Nachrichten** -app integriert werden und dem Benutzer neue Funktionen zur Verfügung stellen.


## <a name="related-links"></a>Verwandte Links

- [Ice Cream Builder (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-icecreambuilder)
- [Nachrichten Verweis](https://developer.apple.com/reference/messages)
- [Programmier Handbuch für die APP-Erweiterung](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)
