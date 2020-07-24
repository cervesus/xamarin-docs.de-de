---
title: Grundlagen der Nachrichten-APP-Erweiterung in xamarin. IOS
description: In diesem Artikel wird gezeigt, wie Sie eine Erweiterung der Nachrichten-APP in eine xamarin. IOS-Projekt Mappe einschließen, die in die Nachrichten-APP integriert wird und dem Benutzer neue Funktionen bietet
ms.prod: xamarin
ms.assetid: 0CFB494C-376C-449D-B714-9E82644F9DA3
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/02/2017
ms.openlocfilehash: 2cc27b18bdb58ee633cae2d61e8cc6a8064df581
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937122"
---
# <a name="message-app-extension-basics-in-xamarinios"></a>Grundlagen der Nachrichten-APP-Erweiterung in xamarin. IOS

_In diesem Artikel wird gezeigt, wie Sie eine Erweiterung der Nachrichten-APP in eine xamarin. IOS-Projekt Mappe einschließen, die in die Nachrichten-APP integriert wird und dem Benutzer neue Funktionen bietet_

Neu bei IOS 10. eine Erweiterung der Nachrichten-APP wird in die **Nachrichten** -app integriert und stellt dem Benutzer neue Funktionen zur Anwendung. Die Erweiterung kann Text, Aufkleber, Mediendateien und interaktive Nachrichten senden.

## <a name="about-message-app-extensions"></a>Informationen zu Message-App-Erweiterungen

Wie bereits erwähnt, ist eine Erweiterung der Nachrichten-APP in die **Nachrichten** -app integriert und stellt dem Benutzer neue Funktionen zur Anwendung. Die Erweiterung kann Text, Aufkleber, Mediendateien und interaktive Nachrichten senden. Es sind zwei Typen von Message App-Erweiterungen verfügbar:

- **Aufkleber Packs** : enthält eine Auflistung von Aufkleber, die der Benutzer einer Nachricht hinzufügen kann. Aufkleber Packs können erstellt werden, ohne Code zu schreiben.
- **IMESS Age-App** : kann eine benutzerdefinierte Benutzeroberfläche innerhalb der Nachrichten-APP zum Auswählen von Stickern, zum Eingeben von Text, einschließlich Mediendateien (mit optionalen Typkonvertierungen) und zum Erstellen, bearbeiten und Senden von Interaktions Nachrichten darstellen.

Erweiterungen von Message apps stellen drei Hauptinhalts Typen bereit:

- **Interaktive Nachrichten** : handelt es sich um einen Typ von benutzerdefiniertem Nachrichten Inhalt, der von einer APP generiert wird, wird die APP im Vordergrund gestartet, wenn der Benutzer auf die Nachricht tippt.
- **Aufkleber** : Bilder, die von der APP generiert werden und in die zwischen Benutzern gesendeten Nachrichten eingeschlossen werden können.
- **Anderer unterstützter Inhalt** : die APP kann Inhalte wie Fotos, Videos, Text oder Links zu anderen Inhaltstypen bereitstellen, die von der Nachrichten-APP immer unterstützt werden.

Neu bei IOS 10, die Nachrichten-APP enthält jetzt einen eigenen dedizierten, integrierten App Store. Alle apps, die Erweiterungen für nachrichtenapps enthalten, werden in diesem Speicher angezeigt und höher gestuft. Die neue Nachrichten-APP-Schublade zeigt alle apps an, die aus dem Nachrichten-App-Store heruntergeladen wurden, um schnellen Zugriff auf die Benutzer zu ermöglichen.

Zusätzlich zu IOS 10 hat Apple eine Inline-App-Zuordnung hinzugefügt, die es dem Benutzer ermöglicht, eine APP leicht zu finden. Wenn ein Benutzer beispielsweise Inhalt von einer APP an eine andere sendet, die der zweite Benutzer nicht installiert hat (z. b. ein Aufkleber), wird der Name der sendenden app unter dem Inhalt im Nachrichten Verlauf aufgeführt. Wenn der Benutzer auf den Namen der APP tippt, wird der Nachrichten-APP-Speicher geöffnet, und die APP wird im Store ausgewählt.

Die Erweiterungen von Message apps ähneln vorhandenen IOS-apps, die der Entwickler bereits erstellt hat, und haben Zugriff auf alle Standard Frameworks und Features einer standardmäßigen IOS-app. Beispiel:

- Sie haben Zugriff auf in-App-Käufe.
- Sie haben Zugriff auf Apple Pay.
- Sie haben Zugriff auf Gerätehardware, z. b. die Kamera.

Erweiterungen von Message apps werden nur auf IOS 10 unterstützt. der Inhalt, den diese Erweiterungen senden, kann jedoch auf watchos-und macOS-Geräten angezeigt werden. Die neue _Seite "Recents_ ", die watchos 3 hinzugefügt wird, zeigt aktuelle Aufkleber an, die vom Telefon gesendet wurden, einschließlich der Informationen aus den Erweiterungen von Message apps, und ermöglicht es dem Benutzer, diese Aufkleber von der Überwachung zu senden.

## <a name="about-the-messages-framework"></a>Informationen zum Nachrichten Framework

Neu bei IOS 10. das Nachrichten-Framework stellt die Schnittstelle zwischen der Message apps-Erweiterung und der Nachrichten-APP auf dem IOS-Gerät des Benutzers bereit. Wenn der Benutzer eine APP aus der Nachrichten-APP heraus öffnet, ermöglicht dieses Framework die Ermittlung der APP und bietet die Daten und den Kontext, die für das Layout der Benutzeroberfläche erforderlich sind.

Nachdem die APP gestartet wurde, interagiert der Benutzer mit der Anwendung, um neue Inhalte zu erstellen, die über eine Nachricht freigegeben werden. Die APP verwendet dann das Nachrichten Framework, um den neu erstellten Inhalt zur Verarbeitung an die Nachrichten-APP zu übertragen.

Die Erweiterungen Messages Framework und Message apps basieren auf bereits vorhandenen IOS-App-Erweiterungs Technologien. Weitere Informationen zu app-Erweiterungen finden Sie im [Programmier Handbuch zur APP-Erweiterung](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)von Apple.

Im Gegensatz zu anderen Erweiterungs Punkten, die Apple im gesamten System bereitgestellt hat, muss der Entwickler keine Host-App für seine Message apps-Erweiterungen bereitstellen, da die Nachrichten-APP selbst als Container fungiert. Der Entwickler hat jedoch die Möglichkeit, die Erweiterung "Message Apps" in eine neue oder vorhandene IOS-App einzubeziehen und diese zusammen mit dem Paket zu versenden.

Wenn die Message apps-Erweiterungen in einem IOS-App-Paket enthalten sind, wird das App-Symbol sowohl auf dem Startbildschirm des Geräts als auch in der Nachricht der Nachrichten-APP angezeigt. Wenn Sie nicht in einem App Bundle enthalten ist, wird die Erweiterung für die Erweiterung von Nachrichten nur in der Nachrichten-APP angezeigt.

Auch wenn die Erweiterungen der Message-apps nicht in einem Host App Bundle enthalten sind, muss der Entwickler ein App-Symbol im Bundle der Erweiterung "Message Apps" bereitstellen, da dies das Symbol ist, das in anderen Teilen des Systems, z. b. der Nachrichten-APP-Einschub Fach oder den Einstellungen, für die Erweiterung angezeigt wird.

## <a name="about-stickers"></a>Informationen zu Aufkleber

Apple hat Aufkleber als neue Möglichkeit für IMESS-Benutzer zur Kommunikation entwickelt, indem das Senden von Haltepunkten Inline als beliebiger anderer Nachrichten Inhalt zugelassen oder an vorherige Nachrichten Blasen innerhalb der Konversation angehängt werden kann.

Was sind Aufkleber?

- Dabei handelt es sich um Bilder, die von der Erweiterung "Message Apps"
- Sie können entweder animierte oder statische Bilder sein.
- Sie bieten eine neue Möglichkeit, Bildinhalte aus einer APP gemeinsam zu nutzen.

Es gibt zwei Möglichkeiten, Aufkleber zu erstellen:

1. Eine Aufkleber-App-Erweiterung kann aus Xcode heraus erstellt werden, ohne dass Code eingeschlossen werden muss. Alles, was erforderlich ist, sind die Ressourcen für die Aufkleber und die APP-Symbole.
2. Durch Erstellen einer standardmäßigen Message apps-Erweiterung, die Aufkleber aus Code über das Nachrichten-Framework bereitstellt.

### <a name="creating-sticker-packs"></a>Erstellen von Aufkleber Packs

Aufkleber Packs werden aus einer speziellen Vorlage in Xcode erstellt und stellen einfach einen statischen Satz von Bild Objekten bereit, die als Aufkleber verwendet werden können. Wie bereits erwähnt, erfordern Sie keinen Code, der Entwickler zieht einfach Bilddateien in den Ordner "Sticker Pack" innerhalb des Aufkleber-Ressourcen Katalogs.

Damit ein Abbild in einem Aufkleber Pack enthalten ist, muss es die folgenden Anforderungen erfüllen:

- Bilder müssen ein PNG-, APNG-, GIF-oder JPEG-Format aufweisen. Apple schlägt bei der Bereitstellung von Aufkleber-Objekten nur die PNG-und APNG-Formate vor.
- Animierte Aufkleber unterstützen nur die Formate APNG und GIF.
- Aufkleber-Bilder sollten einen transparenten Hintergrund bereitstellen, da Sie vom Benutzer über Nachrichten Blasen in der Konversation platziert werden können.
- Die einzelnen Bilddateien müssen kleiner als 500 KB sein.
- Images dürfen nicht kleiner als 100 x 100 Punkte oder größer als 206 x 206 Punkte sein.

> [!IMPORTANT]
> Aufkleber-Bilder sollten immer bei der `@3x` Auflösung im Pixel Bereich 300 x 300 bis 618 x 618 bereitgestellt werden. Das System generiert automatisch die `@2x` -und- `@1x` Versionen zur Laufzeit nach Bedarf.

Apple schlägt vor, die Aufkleber-Bild Ressourcen auf verschiedene farbige Hintergründe (z. b. weiß, schwarz, rot, gelb und mehrfarbig) und auf Fotos zu testen, um sicherzustellen, dass Sie in allen möglichen Situationen am besten aussehen.

Aufkleber Packs können Aufkleber in einer von drei verfügbaren Größen bereitstellen:

- **Kleine** 100 x 100 Punkte.
- **Mittel** -136 x 136 Punkte. Dies ist die Standardgröße.
- **Große** 206 x 206 Punkte.

Verwenden Sie die Attribute Inspector von Xcode, um die Größe des gesamten Aufkleber-Pakets festzulegen, und geben Sie nur Bild Ressourcen an, die der angeforderten Größe entsprechen, um die besten Ergebnisse im Aufkleber-Browser innerhalb der Nachrichten-APP zu erzielen.

Weitere Informationen finden Sie in unserer [Ice Cream Builder](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-icecreambuilder) -APP und in der Apple [Messages-Referenz](https://developer.apple.com/reference/messages).

## <a name="creating-a-custom-sticker-experience"></a>Erstellen eines benutzerdefinierten Aufkleber-Erlebnisses

Wenn die APP mehr Kontrolle oder Flexibilität erfordert, als durch ein Aufkleber Pack bereitgestellt wird, kann Sie eine Nachrichten-APP-Erweiterung enthalten und die Aufkleber über das Nachrichten-Framework für eine benutzerdefinierte Aufkleber-Darstellung bereitstellen.

Was sind die Vorteile der Erstellung eines benutzerdefinierten Aufkleber-Erlebnisses?

1. Ermöglicht der APP, anzupassen, wie Aufkleber Benutzern der App angezeigt werden. So können Sie beispielsweise Aufkleber in einem anderen Format als dem Standard Raster Layout oder einem anderen farbigen Hintergrund darstellen.
2. Ermöglicht, dass Aufkleber dynamisch aus Code erstellt werden, anstatt als statische bildassets eingeschlossen zu werden.
3. Ermöglicht das dynamische herunterladen von Aufkleber-bildassets vom Webserver des Entwicklers, ohne dass eine neue Version im App Store veröffentlicht werden muss.
4. Ermöglicht den Zugriff auf die Kamera des Geräts, um Aufkleber im Handumdrehen zu erstellen.
5. Ermöglicht in-App-Käufe, sodass der Benutzer mehr Aufkleber aus der APP kaufen kann.

Gehen Sie folgendermaßen vor, um eine benutzerdefinierte Aufkleber-Darstellung zu erstellen:

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

1. Starten Sie Visual Studio für Mac.
2. Öffnen Sie die Projekt Mappe, um eine Erweiterung der Nachrichten-APP hinzuzufügen.
3. Wählen Sie **IOS**  >  **Extensions**  >  **IMESS Age Extension** aus, und klicken Sie auf die Schaltfläche **weiter** :

    [![IMESS Age-Erweiterung auswählen](intro-to-message-app-extensions-images/message01.png)](intro-to-message-app-extensions-images/message01.png#lightbox)
4. Geben Sie einen **Namen** ein, und klicken Sie auf die Schaltfläche **weiter** :

    [![Eingeben eines Erweiterungs namens](intro-to-message-app-extensions-images/message02.png)](intro-to-message-app-extensions-images/message02.png#lightbox)
5. Klicken Sie auf die Schaltfläche **Erstellen** , um die Erweiterung zu erstellen:

    [![Klicken Sie auf die Schaltfläche erstellen](intro-to-message-app-extensions-images/message03.png)](intro-to-message-app-extensions-images/message03.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Starten Sie Visual Studio.
2. Öffnen Sie die Projekt Mappe, um eine nachrichtenapp-Erweiterung hinzuzufügen.
3. Wählen Sie **IOS Extensions > IMESS Age Extension (IOS)** aus, und klicken Sie auf die Schaltfläche **weiter** :

    [![IMESS-Erweiterung auswählen (IOS)](intro-to-message-app-extensions-images/message01.w157-sml.png)](intro-to-message-app-extensions-images/message01.w157.png#lightbox)

4. Geben Sie einen **Namen** ein, und klicken Sie auf **OK** .

-----

Standardmäßig wird die `MessagesViewController.cs` Datei der Projekt Mappe hinzugefügt. Dies ist der Haupteinstiegspunkt in die Erweiterung und erbt von der- `MSMessageAppViewController` Klasse.

Das Messages-Framework stellt Klassen bereit, mit denen dem Benutzer verfügbare Aufkleber angezeigt werden können:

- `MSStickerBrowserViewController`-Steuert die Ansicht, in der die Aufkleber angezeigt werden. Außerdem entspricht Sie der `IMSStickerBrowserViewDataSource` -Schnittstelle, um die Anzahl der Aufkleber und den Aufkleber für einen gegebenen Browser Index zurückzugeben.
- `MSStickerBrowserView`-Dies ist die Ansicht, in der die verfügbaren Aufkleber angezeigt werden.
- `MSStickerSize`: Bestimmt die einzelnen Zellgrößen für das Raster der in der Browseransicht dargestellten Aufkleber.

### <a name="creating-a-custom-sticker-browser"></a>Erstellen eines benutzerdefinierten Aufkleber-Browsers

Der Entwickler kann die Aufkleber-Darstellung für den Benutzer weiter anpassen, indem er einen benutzerdefinierten Aufkleber-Browser ( `MSMessageAppBrowserViewController` ) in der Message-App-Erweiterung bereitstellt. Der benutzerdefinierte Aufkleber-Browser ändert, wie Aufkleber Benutzern angezeigt werden, wenn Sie einen Aufkleber auswählen, der in den Nachrichtenstrom eingeschlossen werden soll.

Gehen Sie wie folgt vor:

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

1. Klicken Sie im **Lösungspad**mit der rechten Maustaste auf den Projektnamen der Erweiterung, und wählen Sie neue Datei **Hinzufügen**  >  **...**  >  aus. **IOS | Apple Watch**  >  **Schnittstellen Controller**.
2. Geben Sie `StickerBrowserViewController` als **Namen** ein, und klicken Sie auf die Schaltfläche **neu** :

    [![Geben Sie stickerbrowserviewcontroller als Name ein.](intro-to-message-app-extensions-images/browser01.png)](intro-to-message-app-extensions-images/browser01.png#lightbox)
3. Öffnet die Datei `StickerBrowserViewController.cs` zur Bearbeitung.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf den Projektnamen der Erweiterung, und wählen Sie neue Datei **Hinzufügen**  >  **...**  >  aus. **IOS | Apple Watch**  >  **Schnittstellen Controller**.
2. Geben Sie `StickerBrowserViewController` als **Namen** ein, und klicken Sie auf die Schaltfläche **neu** :

    [![Geben Sie stickerbrowserviewcontroller als Name ein.](intro-to-message-app-extensions-images/browser01.w157-sml.png)](intro-to-message-app-extensions-images/browser01.w157.png#lightbox)
3. Öffnet die Datei `StickerBrowserViewController.cs` zur Bearbeitung.

-----

Machen Sie `StickerBrowserViewController.cs` folgendes Aussehen:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Messages;
using Foundation;

namespace MonkeyStickers
{
    public partial class StickerBrowserViewController : MSStickerBrowserViewController
    {
        #region Computed Properties
        public List<MSSticker> Stickers { get; set; } = new List<MSSticker> ();
        #endregion

        #region Constructors
        public StickerBrowserViewController (MSStickerSize stickerSize) : base (stickerSize)
        {
        }
        #endregion

        #region Private Methods
        private void CreateSticker (string assetName, string localizedDescription)
        {

            // Get path to asset
            var path = NSBundle.MainBundle.PathForResource (assetName, "png");
            if (path == null) {
                Console.WriteLine ("Couldn't create sticker {0}.", assetName);
                return;
            }

            // Build new sticker
            var stickerURL = new NSUrl (path);
            NSError error = null;
            var sticker = new MSSticker (stickerURL, localizedDescription, out error);
            if (error == null) {
                // Add to collection
                Stickers.Add (sticker);
            } else {
                // Report error
                Console.WriteLine ("Error, couldn't create sticker {0}: {1}", assetName, error);
            }
        }

        private void LoadStickers ()
        {

            // Load sticker assets from disk
            CreateSticker ("canada", "Canada Sticker");
            CreateSticker ("clouds", "Clouds Sticker");
            ...
            CreateSticker ("tree", "Tree Sticker");
        }
        #endregion

        #region Public Methods
        public void ChangeBackgroundColor (UIColor color)
        {
            StickerBrowserView.BackgroundColor = color;

        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            LoadStickers ();
        }

        public override nint GetNumberOfStickers (MSStickerBrowserView stickerBrowserView)
        {
            return Stickers.Count;
        }

        public override MSSticker GetSticker (MSStickerBrowserView stickerBrowserView, nint index)
        {
            return Stickers[(int)index];
        }
        #endregion
    }
}
```

Sehen Sie sich den obigen Code im Detail an. Er erstellt Speicher für die von der Erweiterung bereitgestellten Aufkleber:

```csharp
public List<MSSticker> Stickers { get; set; } = new List<MSSticker> ();
```

Und überschreibt zwei Methoden der- `MSStickerBrowserViewController` Klasse, um Daten für den Browser aus diesem Datenspeicher bereitzustellen:

```csharp
public override nint GetNumberOfStickers (MSStickerBrowserView stickerBrowserView)
{
    return Stickers.Count;
}

public override MSSticker GetSticker (MSStickerBrowserView stickerBrowserView, nint index)
{
    return Stickers[(int)index];
}
```

Die `CreateSticker` -Methode ruft den Pfad eines imageassets aus dem Erweiterungspaket ab und verwendet diese zum Erstellen einer neuen Instanz eines `MSSticker` aus diesem Asset, das der Auflistung hinzugefügt wird:

```csharp
private void CreateSticker (string assetName, string localizedDescription)
{

    // Get path to asset
    var path = NSBundle.MainBundle.PathForResource (assetName, "png");
    if (path == null) {
        Console.WriteLine ("Couldn't create sticker {0}.", assetName);
        return;
    }

    // Build new sticker
    var stickerURL = new NSUrl (path);
    NSError error = null;
    var sticker = new MSSticker (stickerURL, localizedDescription, out error);
    if (error == null) {
        // Add to collection
        Stickers.Add (sticker);
    } else {
        // Report error
        Console.WriteLine ("Error, couldn't create sticker {0}: {1}", assetName, error);
    }
}
```

Die `LoadSticker` -Methode wird von aufgerufen `ViewDidLoad` , um einen Aufkleber aus dem benannten imageasset (enthalten im Paket der APP) zu erstellen und Sie der Auflistung der Aufkleber hinzuzufügen.

Bearbeiten `MessagesViewController.cs` Sie die Datei, und führen Sie Sie wie folgt aus, um den benutzerdefinierten Aufkleber-Browser zu implementieren:

```csharp
using System;
using UIKit;
using Messages;

namespace MonkeyStickers
{
    public partial class MessagesViewController : MSMessagesAppViewController
    {
        #region Computed Properties
        public StickerBrowserViewController BrowserViewController { get; set;}
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Create new browser and configure it
            BrowserViewController = new StickerBrowserViewController (MSStickerSize.Regular);
            BrowserViewController.View.Frame = View.Frame;
            BrowserViewController.ChangeBackgroundColor (UIColor.Gray);

            // Add to view
            AddChildViewController (BrowserViewController);
            BrowserViewController.DidMoveToParentViewController (this);
            View.AddSubview (BrowserViewController.View);
        }
        #endregion
    }
}
```

Wenn Sie diesen Code detailliert betrachten, erstellt er Speicher für den benutzerdefinierten Browser:

```csharp
public StickerBrowserViewController BrowserViewController { get; set;}
```

In der- `ViewDidLoad` Methode wird ein neuer Browser instanziiert und konfiguriert:

```csharp
// Create new browser and configure it
BrowserViewController = new StickerBrowserViewController (MSStickerSize.Regular);
BrowserViewController.View.Frame = View.Frame;
BrowserViewController.ChangeBackgroundColor (UIColor.Gray);
```

Anschließend wird der Browser der Ansicht hinzugefügt, um Sie anzuzeigen:

```csharp
// Add to view
AddChildViewController (BrowserViewController);
BrowserViewController.DidMoveToParentViewController (this);
View.AddSubview (BrowserViewController.View);
```

### <a name="further-sticker-customization"></a>Weitere Aufkleber-Anpassung

Eine weitere Aufkleber-Anpassung ist möglich, indem nur zwei Klassen in die Erweiterung der Nachrichten-APP eingeschlossen werden:

- `MSStickerView`
- `MSSticker`

Mithilfe der oben genannten Methoden kann die Erweiterung eine Aufkleber-Auswahl unterstützen, die nicht auf der standardmäßigen Aufkleber-Browser Methode beruht. Außerdem kann die Aufkleber-Anzeige zwischen zwei unterschiedlichen Ansichtsmodi gewechselt werden:

- **Compact** : Dies ist der Standardmodus, in dem in der Ansicht "Aufkleber" die untersten 25% der Meldungs Ansicht angezeigt werden.
- **Erweitert** : die Ansicht "Aufkleber" füllt die gesamte Meldungs Ansicht aus.

Diese halte Ansicht kann zwischen diesen Modi entweder Programm gesteuert oder manuell vom Benutzer gewechselt werden.

Sehen Sie sich das folgende Beispiel für die Handhabung des Schalters zwischen den beiden unterschiedlichen Ansichtsmodi an. Für jeden Zustand sind zwei verschiedene Ansichts Controller erforderlich. Der `StickerBrowserViewController` verarbeitet die **Compact** -Ansicht und sieht wie folgt aus:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Messages;
using Foundation;

namespace MessageExtension
{
    public partial class StickerBrowserViewController : MSStickerBrowserViewController
    {
        #region Computed Properties
        public MessagesViewController MessagesAppViewController { get; set; }
        public List<MSSticker> Stickers { get; set; } = new List<MSSticker> ();
        #endregion

        #region Constructors
        public StickerBrowserViewController (MessagesViewController messagesAppViewController, MSStickerSize stickerSize) : base (stickerSize)
        {
            // Initialize
            this.MessagesAppViewController = messagesAppViewController;
        }
        #endregion

        #region Private Methods
        private void CreateSticker (string assetName, string localizedDescription)
        {

            // Get path to asset
            var path = NSBundle.MainBundle.PathForResource (assetName, "png");
            if (path == null) {
                Console.WriteLine ("Couldn't create sticker {0}.", assetName);
                return;
            }

            // Build new sticker
            var stickerURL = new NSUrl (path);
            NSError error = null;
            var sticker = new MSSticker (stickerURL, localizedDescription, out error);
            if (error == null) {
                // Add to collection
                Stickers.Add (sticker);
            } else {
                // Report error
                Console.WriteLine ("Error, couldn't create sticker {0}: {1}", assetName, error);
            }
        }

        private void LoadStickers ()
        {

            // Load sticker assets from disk
            CreateSticker ("add", "Add New Sticker");
            CreateSticker ("canada", "Canada Sticker");
            CreateSticker ("clouds", "Clouds Sticker");
            CreateSticker ("tree", "Tree Sticker");
        }
        #endregion

        #region Public Methods
        public void ChangeBackgroundColor (UIColor color)
        {
            StickerBrowserView.BackgroundColor = color;

        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            LoadStickers ();

        }

        public override nint GetNumberOfStickers (MSStickerBrowserView stickerBrowserView)
        {
            return Stickers.Count;
        }

        public override MSSticker GetSticker (MSStickerBrowserView stickerBrowserView, nint index)
        {
            // Wanting to add a new sticker?
            if (index == 0) {
                // Yes, ask controller to present add sticker interface
                MessagesAppViewController.AddNewSticker ();
                return null;
            } else {
                // No, return existing sticker
                return Stickers [(int)index];
            }
        }
        #endregion
    }
}
```

Der `AddStickerViewController` verarbeitet die **Erweiterte** Aufkleber-Ansicht und sieht wie folgt aus:

```csharp
using System;
using Foundation;
using UIKit;
using Messages;

namespace MessageExtension
{
    public class AddStickerViewController : UIViewController
    {
        #region Computed Properties
        public MessagesViewController MessagesAppViewController { get; set;}
        public MSSticker NewSticker { get; set;}
        #endregion

        #region Constructors
        public AddStickerViewController (MessagesViewController messagesAppViewController)
        {
            // Initialize
            this.MessagesAppViewController = messagesAppViewController;
        }
        #endregion

        #region Override Method
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Build interface to create new sticker
            var cancelButton = new UIButton (UIButtonType.RoundedRect);
            cancelButton.TouchDown += (sender, e) => {
                // Cancel add new sticker
                MessagesAppViewController.CancelAddNewSticker ();
            };
            View.AddSubview (cancelButton);

            var doneButton = new UIButton (UIButtonType.RoundedRect);
            doneButton.TouchDown += (sender, e) => {
                // Add new sticker to collection
                MessagesAppViewController.AddStickerToCollection (NewSticker);
            };
            View.AddSubview (doneButton);

            ...
        }
        #endregion
    }
}
```

`MessageViewController`Implementiert diese Ansichts Controller zum Steuern des angeforderten Zustands:

```csharp
using System;
using UIKit;
using Messages;

namespace MessageExtension
{
    public partial class MessagesViewController : MSMessagesAppViewController
    {
        #region Computed Properties
        public bool IsAddingSticker { get; set;}
        public StickerBrowserViewController BrowserViewController { get; set; }
        public AddStickerViewController AddStickerController { get; set;}
        #endregion

        #region Constructors
        protected MessagesViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Public Methods
        public void PresentStickerBrowser ()
        {
            // Is the Add sticker view being displayed?
            if (IsAddingSticker) {
                // Yes, remove it from view
                AddStickerController.RemoveFromParentViewController ();
                AddStickerController.View.RemoveFromSuperview ();
            }

            // Add to view
            AddChildViewController (BrowserViewController);
            BrowserViewController.DidMoveToParentViewController (this);
            View.AddSubview (BrowserViewController.View);

            // Save mode
            IsAddingSticker = false;
        }

        public void PresentAddSticker ()
        {
            // Is the sticker browser being displayed?
            if (!IsAddingSticker) {
                // Yes, remove it from view
                BrowserViewController.RemoveFromParentViewController ();
                BrowserViewController.View.RemoveFromSuperview ();
            }

            // Add to view
            AddChildViewController (AddStickerController);
            AddStickerController.DidMoveToParentViewController (this);
            View.AddSubview (AddStickerController.View);

            // Save mode
            IsAddingSticker = true;
        }

        public void AddNewSticker ()
        {
            // Switch to expanded view mode
            Request (MSMessagesAppPresentationStyle.Expanded);
        }

        public void CancelAddNewSticker ()
        {
            // Switch to compact view mode
            Request (MSMessagesAppPresentationStyle.Compact);
        }

        public void AddStickerToCollection (MSSticker sticker)
        {
            // Add sticker to collection
            BrowserViewController.Stickers.Add (sticker);

            // Switch to compact view mode
            Request (MSMessagesAppPresentationStyle.Compact);
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Create new browser and configure it
            BrowserViewController = new StickerBrowserViewController (this, MSStickerSize.Regular);
            BrowserViewController.View.Frame = View.Frame;
            BrowserViewController.ChangeBackgroundColor (UIColor.Gray);

            // Create new Add controller and configure it as well
            AddStickerController = new AddStickerViewController (this);
            AddStickerController.View.Frame = View.Frame;

            // Initially present the sticker browser
            PresentStickerBrowser ();
        }

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
        #endregion
    }
}
```

Wenn der Benutzer anfordert, der verfügbaren Sammlung einen neuen Aufkleber hinzuzufügen, `AddStickerViewController` wird ein neuer zum sichtbaren Controller gemacht und die Ansicht "Aufkleber" in die **Erweiterte** Ansicht:

```csharp
// Switch to expanded view mode
Request (MSMessagesAppPresentationStyle.Expanded);
```

Wenn der Benutzer einen Aufkleber auswählt, der hinzugefügt werden soll, wird er zu der verfügbaren Sammlung hinzugefügt, und die **Compact** -Ansicht wird angefordert:

```csharp
public void AddStickerToCollection (MSSticker sticker)
{
    // Add sticker to collection
    BrowserViewController.Stickers.Add (sticker);

    // Switch to compact view mode
    Request (MSMessagesAppPresentationStyle.Compact);
}
```

Die- `DidTransition` Methode wird überschrieben, um den Wechsel zwischen den beiden Modi zu handhaben:

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

## <a name="summary"></a>Zusammenfassung

Dieser Artikel behandelt eine Erweiterung der Nachrichten-APP in einer xamarin. IOS-Projekt Mappe, die in die **Nachrichten** -app integriert wird und dem Benutzer neue Funktionen präsentiert. Es wird die Verwendung der Erweiterung zum Senden von Text, Aufkleber, Mediendateien und interaktiven Nachrichten behandelt.

## <a name="related-links"></a>Verwandte Links

- [Ice Cream Builder (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-icecreambuilder)
- [Nachrichten Verweis](https://developer.apple.com/reference/messages)
- [Programmier Handbuch für die APP-Erweiterung](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)
