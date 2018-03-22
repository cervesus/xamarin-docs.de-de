---
title: Grundlagen der Nachrichten-App-Erweiterung
description: "Dieser Artikel zeigt enthalten wie eine Nachrichten-App-Erweiterung in einem Xamarin.iOS-Lösung, die Nachrichten-app integriert und stellt neue Funktionen für den Benutzer."
ms.topic: article
ms.prod: xamarin
ms.assetid: 0CFB494C-376C-449D-B714-9E82644F9DA3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 083920aba3c8dc83b157b591e194c43935dcc566
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="message-app-extension-basics"></a>Grundlagen der Nachrichten-App-Erweiterung

_Dieser Artikel zeigt enthalten wie eine Nachrichten-App-Erweiterung in einem Xamarin.iOS-Lösung, die Nachrichten-app integriert und stellt neue Funktionen für den Benutzer._

Neu für iOS-10-App-Erweiterung einer Nachricht integriert die **Nachrichten** app und stellt neue Funktionen für den Benutzer. Die Erweiterung kann Text, Aufkleber, Mediendateien und interaktive Nachrichten senden.

## <a name="about-message-app-extensions"></a>Informationen zu Nachrichten-App-Erweiterungen

Wie bereits erwähnt, eine Nachrichten-App-Erweiterung integriert die **Nachrichten** app und stellt neue Funktionen für den Benutzer. Die Erweiterung kann Text, Aufkleber, Mediendateien und interaktive Nachrichten senden. Zwei Arten von App-Erweiterung der Nachricht sind verfügbar:

- **Der Aufkleber mit Packs** -enthält eine Auflistung von Aufkleber, die der Benutzer eine Nachricht hinzufügen kann. Der Aufkleber mit Packs können erstellt werden, ohne Code schreiben zu müssen.
- **iMessage App** -kann eine benutzerdefinierte Benutzeroberfläche innerhalb der Nachrichten-app für Aufkleber auswählen, Text eingeben, einschließlich Mediendateien (mit optionalen typkonvertierungen) und erstellen, bearbeiten und Senden von Nachrichten der Aktivität darstellen.

Nachrichtenerweiterungen für Apps bieten drei Haupttypen Inhalt:

- **Interaktive Nachrichten** -sind eine Art von Inhalt von benutzerdefinierten Nachrichten, die eine app generiert, wenn der Benutzer die Meldung, die app tippt wird im Vordergrund gestartet werden.
- **Aufkleber** -Images generiert, die von der app, die in die Nachrichten zwischen den Benutzern aufgenommen werden kann.
- **Andere Inhalte unterstützte** – die app bereitstellen Inhalte wie Fotos, Videos, Text oder Links zu anderen Inhalten eingibt, die immer von der Nachrichten-app unterstützt.

Neu für iOS 10, die Nachrichten-app enthält jetzt einen eigenen dedizierten, integrierte App Store. Alle apps, die Apps Nachrichtenerweiterungen enthalten werden angezeigt, und in diesem Speicher höher gestuft werden. Öffnen Sie die neue Nachrichten-App zeigt alle apps, die aus dem App Store Nachrichten, schnellen Zugriff auf die Benutzer bereitstellen heruntergeladen wurden.

Auch neue in iOS 10, Apple fügte Inline App Attribution die dem Benutzer ermöglicht, eine app einfach zu ermitteln. Z. B. wenn ein Benutzer aus einer Anwendung, der der 2. Benutzer aufweist, Inhalt in einen anderen gesendet (z. B. Aufkleber z. B.) installiert, der Namen der sendenden Anwendung unter den Inhalt im Verlauf Nachricht enthält. Wenn der Benutzer der app tippt Benennen der App-Nachrichtenspeicher, die wir geöffnet werden und die app im Store ausgewählt.

Apps Nachrichtenerweiterungen ähneln vorhandenen iOS-apps, dass der Entwickler vertraut, mit dem Erstellen ist und zum Zugriff auf die standard-Frameworks und Funktionen von einer standardmäßigen iOS-app. Zum Beispiel:

- Sie haben Zugriff auf In-App-Käufe.
- Sie verfügen über Zugriff auf die Apple Pay.
- Sie haben Zugriff auf Gerätehardware wie z. B. die Verwendung der Kamera.

Nachrichtenerweiterungen-Apps werden nur für iOS 10 unterstützt, der Inhalt, den diese Erweiterungen zu senden ist allerdings auf Geräten mit WatchOS und MacOS angezeigt werden kann. Die neue _Recents Page_ WatchOS 3 hinzugefügt wurde, wird Anzeigen der neuesten Aufkleber, die aus dem Mobiltelefon, einschließlich der von Nachrichtenerweiterungen für Apps, gesendet wurden und ermöglicht dem Benutzer diese Aufkleber von der Apple Watch senden.

## <a name="about-the-messages-framework"></a>Informationen über die Nachrichten-Framework

Neu für iOS 10, die Nachrichten-Framework bietet, die Schnittstelle zwischen der Nachricht Apps-Erweiterung und die Nachrichten-app auf iOS-Gerät des Benutzers. Wenn der Benutzer eine App aus, in der Nachrichten-app gestartet wird, wird dieses Framework ermöglicht der app, die ermittelt werden und enthält die Daten und den Kontext, um das Layout der Benutzeroberflächenautomatisierungs erforderlich.

Sobald die app gestartet wird, interagiert der Benutzer zum Erstellen von neuen Inhalts über eine Nachricht freigeben. Die app verwendet das Framework Nachrichten klicken Sie dann auf um die neu erstellten Inhalt an die Nachrichten-app für die Verarbeitung zu übertragen.

Die Nachrichten-Framework und Nachrichtenerweiterungen für Apps sind zusätzlich zu bereits vorhandenen iOS-App-Erweiterungen Technologien erstellt. Weitere Informationen zu App-Erweiterungen, finden Sie in der Apple- [Programmierhandbuch für App-Erweiterung](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214).

Im Gegensatz zu anderen Erweiterungspunkten, die von Apple im gesamten System zur Verfügung gestellt hat, muss der Entwickler nicht auf eine Host-app für ihre Apps Nachrichtenerweiterungen bereitstellen, da die Nachrichten-app selbst als Container fungiert. Allerdings muss der Entwickler die Möglichkeit, z. B. die Erweiterung für die Apps von Nachrichten innerhalb einer neuen oder vorhandenen iOS-app und versenden sie zusammen mit dem Paket.

Wenn die Nachrichtenerweiterungen für Apps in einer iOS-app-Paket enthalten ist, wird das Symbol für die app sowohl auf der Startseite des Geräts als auch in der Nachricht App Pushfunktion aus innerhalb der Nachrichten-app angezeigt. Wenn es nicht in einem app-Paket enthalten ist, wird die Erweiterung für die Nachricht Apps nur in-App öffnen Sie die Meldung angezeigt.

Auch wenn die Nachrichtenerweiterungen für die Apps nicht in einem Host-app-Paket enthalten ist, müssen der Entwickler ein app-Symbol in der Nachricht Apps Erweiterung-Paket angeben wie sieht das Symbol ", das in anderen Teilen des Systems wie die Nachrichten-App-Bereich oder die Einstellungen angezeigt werden sollen , für die Erweiterung.

## <a name="about-stickers"></a>Informationen zu Aufkleber

Apple Aufkleber als eine neue Methode iMessage Benutzern kommunizieren, indem Sie Aufkleber Inline als an alle anderen Nachrichteninhalt entwickelt oder können vorherige Nachrichtenblasen innerhalb der Konversation zugeordnet werden.

Was sind Aufkleber?

- Sie sind Bilder, die die Erweiterung für die Nachricht Apps bereitstellt.
- Sie können entweder animierte oder statische Bilder sein.
- Sie bieten eine neue Methode zum Freigeben von Inhalten von Images von innerhalb einer app.

Es gibt zwei Möglichkeiten, Aufkleber erstellen:

1. Ein Aufkleber Pack Nachrichtenerweiterungen Apps können innerhalb von Xcode aus erstellt werden, ohne Code. Erforderlich ist lediglich die Ressourcen für die Aufkleber und die app-Symbole.
2. Durch das Erstellen einer Nachricht Apps dem standard entsprechende Erweiterung bereitstellt, Aufkleber aus Code über die Nachrichten-Framework.

### <a name="creating-sticker-packs"></a>Der Aufkleber mit Packs erstellen

Aufkleber Packs aus einer speziellen Vorlage in Xcode erstellt, und geben Sie einfach einen statischen Satz von Bildanlagen, die als Aufkleber verwendet werden kann. Wie bereits erwähnt, erfordern keinen Code, der Entwickler zieht Bilddateien einfach auf den Aufkleber-Pack-Ordner, in dem Aufkleber Asset-Katalog.

Für ein Bild, in einem Aufkleber Pack eingeschlossen werden sollen müssen sie die folgenden Anforderungen erfüllen:

- Bilder müssen in einem PNG, APNG, GIF- oder JPEG-Format sein. Apple empfiehlt, nur die PNG und APNG Formate bei der Bereitstellung von Aufkleber Bestand.
- Animierte Aufkleber unterstützen nur die APNG und GIF-Formaten.
- Der Aufkleber mit Bildern sollten ein transparentes Hintergrunds bereitstellen, da sie über Blasen der Meldung in der Konversation vom Benutzer hinzugefügt werden können.
- Die einzelnen Bilddateien müssen weniger als 500 kb sein.
- Bilder, darf nicht kleiner als 100 x 100 Punkt oder größer, 206 x 206 zeigt sein.

> [!IMPORTANT]
> Aufkleber Bilder sollte immer angegeben werden, auf die `@3x` Auflösung im Bereich von 300 x 300, 618 x 618 Pixel. Das System generiert automatisch die `@2x` und `@1x` Versionen zur Laufzeit nach Bedarf.

Apple wird vorgeschlagen, Bildanlagen Aufkleber für verschiedene andere farbige Hintergründe (z. B. Whitepaper, Schwarz, Rot, Gelb und mit mehreren farbig) und über Fotos, um sicherzustellen, dass sie die am besten in allen möglichen Situationen testen.

Der Aufkleber mit Packs können Aufkleber in einem der drei verfügbaren Größen bereitstellen:

- **Kleine** - 100 x 100 Punkte.
- **Mittel** - 136 x 136 Punkte. Dies ist die Standardgröße.
- **Große** - 206 x 206 Punkte.

Verwenden Xcodes Attribute Inspektor, legen Sie die Größe für das gesamte Aufkleber Pack, und erteilen Sie nur Bildanlagen, die die angeforderte Größe, die optimale Ergebnisse erzielen Sie im Browser Aufkleber innerhalb der Nachrichten-app entsprechen.

Weitere Informationen finden Sie unter unsere [Eis rustikal-Generator](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/) app und Apple [Nachrichten Verweis](https://developer.apple.com/reference/messages).

## <a name="creating-a-custom-sticker-experience"></a>Erstellen eine benutzerdefinierte Aufkleber-Erfahrung

Falls die app mehr Steuerelement oder Flexibilität als von einem Aufkleber Pack bereitgestellt wird benötigt, es umfassen eine Nachrichten-App-Erweiterung und die Aufkleber über das Framework Nachrichten für eine benutzerdefinierte Aufkleber Erfahrung bereitzustellen.

Was sind die Vorteile der eine benutzerdefinierte Aufkleber Erfahrung?

1. Ermöglicht der app anpassen, wie Aufkleber für die Benutzer der app angezeigt werden. Um z. B. auf vorhanden Aufkleber in einem Format, die als standard Rasterlayout oder auf einem anderen farbigen Hintergrund.
2. Ermöglicht das Aufkleber aus Code statt als statisches Bild Elemente eingeschlossen werden dynamisch erstellt werden.
3. Ermöglicht das Aufkleber Bildanlagen dynamisch von der Entwickler Webserver heruntergeladen werden, ohne eine neue Version auf den App Store freigeben.
4. Ermöglicht den Zugriff des Geräts Kamera Aufkleber auf dynamisch zu erstellen.
5. Können für In-App-Einkäufe, sodass der Benutzer weitere Aufkleber aus innerhalb der app zu erwerben kann.

Erstellen Sie eine benutzerdefinierte Aufkleber Erfahrung, führen Sie folgende Schritte aus:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Starten Sie Visual Studio für Mac.
2. Öffnen Sie die Projektmappe, um eine Nachricht App-Erweiterung hinzufügen. 
3. Wählen Sie **iOS** > **Erweiterungen** > **iMessage Erweiterung** , und klicken Sie auf die **Weiter** Schaltfläche: 

    [![](intro-to-message-app-extensions-images/message01.png "Wählen Sie iMessage Erweiterung")](intro-to-message-app-extensions-images/message01.png#lightbox)
4. Geben Sie eine **Erweiterungsnamen** , und klicken Sie auf die **Weiter** Schaltfläche: 

    [![](intro-to-message-app-extensions-images/message02.png "Geben Sie einen Erweiterungsnamen")](intro-to-message-app-extensions-images/message02.png#lightbox)
5. Klicken Sie auf die **erstellen** Schaltfläche, um die Erweiterung zu erstellen: 

    [![](intro-to-message-app-extensions-images/message03.png "Klicken Sie auf die Schaltfläche "erstellen"")](intro-to-message-app-extensions-images/message03.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Starten Sie Visual Studio.
2. Öffnen Sie die Projektmappe, um eine Nachricht App-Erweiterung hinzufügen. 
3. Wählen Sie **iOS** > **Erweiterungen** > **iMessage Erweiterung** , und klicken Sie auf die **Weiter** Schaltfläche: 

    [![](intro-to-message-app-extensions-images/message01w.png "Wählen Sie iMessage Erweiterung")](intro-to-message-app-extensions-images/message01.png#lightbox)
4. Geben Sie eine **Erweiterungsnamen** , und klicken Sie auf die **OK** Schaltfläche

-----

Wird standardmäßig die `MessagesViewController.cs` Datei wird der Projektmappe hinzugefügt werden. Dies ist der Haupteinstiegspunkt in die Erweiterung und erbt von der `MSMessageAppViewController` Klasse.

Die Nachrichten-Framework stellt Klassen bereit, für die verfügbaren Aufkleber für den Benutzer verfügbar machen:

- `MSStickerBrowserViewController` -Steuert die Sicht, der in der Aufkleber angezeigt werden. Es entspricht auch der `IMSStickerBrowserViewDataSource` Schnittstelle, um die Anzahl der Aufkleber und dem Aufkleber für einen bestimmten Browser Index zurück.
- `MSStickerBrowserView` -Dies ist die Ansicht, der in den verfügbaren Aufkleber angezeigt werden.
- `MSStickerSize` -Entscheidet sich die Größe der einzelnen Zelle für das Raster der Aufkleber in der Browseransicht angezeigt.

### <a name="creating-a-custom-sticker-browser"></a>Erstellen einen benutzerdefinierten Aufkleber-Browser

Entwickler kann die Aufkleber praktisches Beispiel für den Benutzer durch Bereitstellen einer benutzerdefinierten Aufkleber Browser weiter anpassen (`MSMessageAppBrowserViewController`) in der Nachrichten-App-Erweiterung. Der benutzerdefinierte Aufkleber Browser ändert wie Aufkleber für den Benutzer angezeigt werden, wenn sie Aufkleber den Nachrichtenstream einschließt auswählen.

Führen Sie folgende Schritte aus:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. In der **Lösung Pad**mit der rechten Maustaste auf die Erweiterung Projektnamen, und wählen Sie **hinzufügen** > **neue Datei...**   >  **iOS | Apple Watch** > **Schnittstelle Controller**.
2. Geben Sie `StickerBrowserViewController` für die **Namen** , und klicken Sie auf die **neu** Schaltfläche: 

    [![](intro-to-message-app-extensions-images/browser01.png "Geben Sie ein StickerBrowserViewController")](intro-to-message-app-extensions-images/browser01.png#lightbox)
3. Öffnen der `StickerBrowserViewController.cs` Datei zur Bearbeitung.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. In der **Projektmappen-Explorer**mit der rechten Maustaste auf die Erweiterung Projektnamen, und wählen Sie **hinzufügen** > **neue Datei...**   >  **iOS | Apple Watch** > **Schnittstelle Controller**.
2. Geben Sie `StickerBrowserViewController` für die **Namen** , und klicken Sie auf die **neu** Schaltfläche: 

    [![](intro-to-message-app-extensions-images/browser01w.png "Geben Sie ein StickerBrowserViewController")](intro-to-message-app-extensions-images/browser01.png#lightbox)
3. Öffnen der `StickerBrowserViewController.cs` Datei zur Bearbeitung.

-----

Stellen Sie die `StickerBrowserViewController.cs` wie folgt aussehen:

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

Sehen Sie sich den Code oben im Detail an. Speicher erstellt für die Aufkleber, die die Erweiterung:

```csharp
public List<MSSticker> Stickers { get; set; } = new List<MSSticker> ();
```

Und zwei Möglichkeiten, überschreibt die `MSStickerBrowserViewController` Klasse, um Daten für den Browser aus diesen Datenspeicher bereitstellen:

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

Die `CreateSticker` -Methode den Pfad der Standardimage-Medienobjekt aus Paket mit der Erweiterung ab und verwendet, um eine neue Instanz der Erstellen einer `MSSticker` aus diesem Medienobjekt, wodurch es der Auflistung hinzugefügt:

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

Die `LoadSticker` Methode wird aufgerufen, von `ViewDidLoad` Aufkleber aus der benannten imagemedienobjekt (enthalten in der app-Bündel) erstellen und die Auflistung der Aufkleber hinzuzufügen.

Bearbeiten Sie zum Implementieren von benutzerdefinierten Aufkleber Browser die `MessagesViewController.cs` Datei, und stellen sie wie folgt aussehen:

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

Betrachten diesen Code im Detail, erstellt es Speicher für den benutzerdefinierten Browser:

```csharp
public StickerBrowserViewController BrowserViewController { get; set;}
```

Und klicken Sie in der `ViewDidLoad` -Methode wird instanziiert und konfiguriert ein neues Browserfenster:

```csharp
// Create new browser and configure it
BrowserViewController = new StickerBrowserViewController (MSStickerSize.Regular);
BrowserViewController.View.Frame = View.Frame;
BrowserViewController.ChangeBackgroundColor (UIColor.Gray);
```

Klicken Sie dann fügt der Browser auf die Ansicht zum Anzeigen dieser hinzu:

```csharp
// Add to view
AddChildViewController (BrowserViewController);
BrowserViewController.DidMoveToParentViewController (this);
View.AddSubview (BrowserViewController.View);
```

### <a name="further-sticker-customization"></a>Weitere Aufkleber-Anpassung

Weitere Aufkleber Anpassung ist möglich, nur zwei Klassen in der Nachrichten-App-Erweiterung einschließen:

- `MSStickerView`
- `MSSticker`

Verwenden die oben genannten Methoden ein, kann die Erweiterung Aufkleber Auswahl unterstützen, die nicht auf die Standardmethode Aufkleber Browser angewiesen ist. Darüber hinaus kann die Anzeige Aufkleber zwischen zwei unterschiedliche Ansichtsmodi umgeschaltet werden:

- **Compact** -Dies ist der Standardmodus, in dem die unteren 25 % der Nachricht an die Ansicht der Aufkleber mit beanspruchen.
- **Erweitert** -Aufkleber Ansicht füllt die gesamte Nachricht anzeigen.

In dieser Ansicht Aufkleber kann zwischen diesen Modi entweder programmgesteuert oder manuell vom Benutzer umgeschaltet werden.

Betrachten Sie das folgende Beispiel der Wechsel zwischen den zwei unterschiedliche Ansichtsmodi verarbeiten. Zwei unterschiedliche View-Controller werden für jeden Status erforderlich. Die `StickerBrowserViewController` behandelt die **Compact** anzeigen und sieht wie folgt aus:

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

Die `AddStickerViewController` verarbeitet die **erweitert** Aufkleber anzeigen und überprüfen Sie wie folgt:

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

Die `MessageViewController` implementiert diese View-Controller, um die angeforderten Status Laufwerk:

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

Wenn der Benutzer fordert so ihre verfügbaren Auflistung einen neuen Aufkleber hinzu ein neues `AddStickerViewController` erfolgt die sichtbaren Controller und Ansicht Aufkleber eingibt der **erweitert** anzeigen:

```csharp
// Switch to expanded view mode
Request (MSMessagesAppPresentationStyle.Expanded);
```

Wenn der Benutzer wählt Aufkleber hinzufügen, wird er zu ihrer verfügbar Auflistung hinzugefügt und die **Compact** Ansicht angefordert wird:

```csharp
public void AddStickerToCollection (MSSticker sticker)
{
    // Add sticker to collection
    BrowserViewController.Stickers.Add (sticker);

    // Switch to compact view mode
    Request (MSMessagesAppPresentationStyle.Compact);
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

## <a name="summary"></a>Zusammenfassung

Dieser Artikel behandelt hat enthalten eine Nachrichten-App-Erweiterung in einem Xamarin.iOS-Lösung, mit den der **Nachrichten** app und vorhanden neue Funktionen für den Benutzer. Er behandelt die Erweiterung verwenden, um Text, Aufkleber, Mediendateien und interaktive Nachrichten zu senden.



## <a name="related-links"></a>Verwandte Links

- [Eis rustikal-Generator (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/)
- [Nachrichten-Referenz](https://developer.apple.com/reference/messages)
- [Programmierhandbuch für App-Erweiterung](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)
