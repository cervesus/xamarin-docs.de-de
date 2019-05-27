---
title: Grundlagen der Nachrichten-App-Erweiterung in Xamarin.iOS
description: In diesem Artikel zeigt wie eine Nachrichten-App-Erweiterung in einer Xamarin.iOS-Projektmappe, die Nachrichten-app integriert und stellt neue Funktionen für den Benutzer hinzufügen.
ms.prod: xamarin
ms.assetid: 0CFB494C-376C-449D-B714-9E82644F9DA3
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/02/2017
ms.openlocfilehash: cdeaae6cb83062f0d84a3605582b9779c9f36145
ms.sourcegitcommit: 6ad272c2c7b0c3c30e375ad17ce6296ac1ce72b2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2019
ms.locfileid: "66178045"
---
# <a name="message-app-extension-basics-in-xamarinios"></a>Grundlagen der Nachrichten-App-Erweiterung in Xamarin.iOS

_In diesem Artikel zeigt wie eine Nachrichten-App-Erweiterung in einer Xamarin.iOS-Projektmappe, die Nachrichten-app integriert und stellt neue Funktionen für den Benutzer hinzufügen._

Neue IOS 10, integriert eine Nachrichten-App-Erweiterung der **Nachrichten** -app und stellt neue Funktionen für den Benutzer. Die Erweiterung kann Text, Aufkleber, Mediendateien und interaktive Nachrichten senden.

## <a name="about-message-app-extensions"></a>Informationen zu Nachrichten-App-Erweiterungen

Wie bereits erwähnt, ist eine Nachrichten-App-Erweiterung in integriert die **Nachrichten** -app und stellt neue Funktionen für den Benutzer. Die Erweiterung kann Text, Aufkleber, Mediendateien und interaktive Nachrichten senden. Es stehen zwei Arten von Nachrichten-App-Erweiterung:

- **Aufkleber Packs** -enthält eine Auflistung von Aufkleber, die der Benutzer eine Nachricht hinzugefügt werden kann. Aufkleber Packs erstellt werden, ohne Code schreiben zu müssen.
- **iMessage App** -kann eine benutzerdefinierte Benutzeroberfläche innerhalb der Nachrichten-app für Aufkleber auswählen, Text eingeben, einschließlich Media-Dateien (mit optionalen typkonvertierungen) und erstellen, bearbeiten und Senden von Nachrichten von Interaktion darstellen.

Nachrichtenerweiterungen für die Apps bieten drei Haupttypen Inhalt:

- **Interaktive Nachrichten** -sind eine Art von Inhalt für die benutzerdefinierte Nachricht, die eine app generiert werden, wenn sich der Benutzer tippt in der Nachricht, die app auf wird im Vordergrund gestartet werden.
- **Aufkleber** -Images generiert, die von der app, die in die Nachrichten zwischen den Benutzern aufgenommen werden kann.
- **Andere unterstützte Inhalte** : Geben Sie die app kann Inhalte wie Fotos, Videos, Text oder Links zu allen anderen Inhalten zu Typen, haben immer von der Nachrichten-app unterstützt.

Neue IOS 10, die Nachrichten-app enthält nun eine eigene dedizierte, integrierten App-Store. Alle apps, die Nachrichten-Apps-Erweiterungen enthalten werden angezeigt und in diesem Speicher höher gestuft werden. Die neue Nachrichten-App-Drawer werden alle apps angezeigt, die von der Nachrichten-App Store, um schnellen Zugriff an die Benutzer bereitzustellen heruntergeladen wurden.

Ebenfalls neu in iOS 10, Apple wurde hinzugefügt Inline App Attribution dem der Benutzer eine app leicht ermitteln kann. Wenn ein Benutzer Inhalte in eine andere aus einer app sendet, die der 2. Benutzer keinen ist das installiert (z. B. einem Aufkleber z. B.), z. B. zu der Namen der senden-app unter den Inhalt im Verlauf Nachricht aufgeführt. Wenn der Benutzer der app tippt auf Namen Sie, die Nachrichten-App-Store wir geöffnet werden und die app im Store ausgewählt.

Nachrichtenerweiterungen für Apps sind vergleichbar mit vorhandenen iOS-apps, dass der Entwickler vertraut sind, mit dem Erstellen und sie Zugriff auf die standard-Frameworks und Funktionen von einer standard-iOS-app haben. Zum Beispiel:

- Sie haben Zugriff auf In-App-Käufe.
- Sie haben Zugriff auf die Apple Pay.
- Sie haben Zugriff auf die Hardware der Geräte wie z. B. die Kamera.

Nachrichtenerweiterungen für Apps werden nur unter iOS 10 unterstützt, die der Inhalt, den diese Erweiterungen zu senden ist jedoch auf WatchOS und MacOS-Geräte. Die neue _Recents Page_ WatchOS 3 hinzugefügt wurde, angezeigt aktuelle Aufkleber, die vom Telefon, einschließlich der von Nachrichtenerweiterungen für Apps, gesendet wurden, und Sie ermöglicht dem Benutzer, die Aufkleber von der Apple Watch zu senden.

## <a name="about-the-messages-framework"></a>Informationen zu den Nachrichten-Framework

Neue auf iOS 10, stellt das Framework Nachrichten die Schnittstelle zwischen der Nachrichten-Apps-Erweiterung und die Nachrichten-app auf iOS-Gerät des Benutzers bereit. Wenn der Benutzer eine App aus, in der Nachrichten-app gestartet wird, wird dieses Framework ermöglicht der app, die ermittelt werden und enthält die Daten und Layout der Benutzeroberfläche erforderlich.

Sobald die app gestartet wird, interagiert der Benutzer neue Inhalte über eine Nachricht Teilen zu erstellen. Die app verwendet das Framework Nachrichten klicken Sie dann auf um den neu erstellten Inhalt für die Verarbeitung der Nachrichten-App zu übertragen.

Die Nachrichten-Framework und die Nachrichtenerweiterungen für Apps basieren auf vorhandenen iOS-App-Erweiterungen-Technologien. Weitere Informationen zu App-Erweiterungen finden Sie unter Apple [Programmierhandbuch für App-Erweiterung](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214).

Im Gegensatz zu anderen Erweiterungspunkten, die im gesamten System Apple bereitgestellt wurde, muss der Entwickler nicht um eine Host-app für ihre Apps Nachrichtenerweiterungen bereitzustellen, da die Nachrichten-app selbst als Container fungiert. Allerdings muss der Entwickler die Möglichkeit, einschließlich der Nachrichten-Apps-Erweiterung in einer neuen oder vorhandenen iOS-app und das Senden und das Paket.

Wenn die Nachrichtenerweiterungen für Apps in einer iOS-app-Paket enthalten ist, wird das Symbol der app sowohl auf der Startseite des Geräts als auch in der Nachricht App-Drawer von innerhalb der Nachrichten-app angezeigt werden. Wenn es nicht in einem app-Paket enthalten ist, wird die Erweiterung der Nachrichten-Apps nur im App-Drawer Nachricht angezeigt.

Auch wenn die Nachrichtenerweiterungen für die Apps nicht in einem Host-app-Paket enthalten ist, muss der Entwickler zu app-Symbol in die Nachrichten-Apps-Erweiterung des Pakets, da es sich um das Symbol handelt, das in anderen Teilen des Systems wie die App-Drawer-Nachricht oder die Einstellungen angezeigt werden , für die Erweiterung.

## <a name="about-stickers"></a>Informationen zu Aufkleber

Apple Aufkleber entwickelt, als neue visualisierungsmöglichkeit für iMessage-Benutzer für die Kommunikation ermöglicht Aufkleber Inline als alle anderen Inhalt der Nachricht gesendet werden, oder sie können vorherigen Sprechblasen innerhalb der Konversation zugeordnet werden.

Was sind Aufkleber?

- Sie sind Images, die die Nachrichten-Apps-Erweiterung bietet.
- Sie können Bilder für animierten oder statischen sein.
- Sie bieten eine neue Möglichkeit zur Freigabe von Inhalten von Images von innerhalb einer app.

Es gibt zwei Möglichkeiten zum Erstellen von Stickern:

1. Eine Aufkleber Pack Nachrichtenerweiterungen Apps können von in Xcode erstellt werden, ohne Code. Erforderlich ist lediglich die Objekte für die Aufkleber und die app-Symbole.
2. Durch das Erstellen einer Nachricht Standarderweiterung für Apps, die Aufkleber von Code über die Nachrichten-Framework bereitstellt.

### <a name="creating-sticker-packs"></a>Aufkleber Inhaltspakete erstellen

Aufkleber Packs aus einer speziellen Vorlage in Xcode erstellt, und geben Sie einfach einen statischen Satz von Image-Ressourcen, die als Aufkleber verwendet werden kann. Wie bereits erwähnt, sie erfordern keine Code, der Entwickler einfach zieht der Bilddateien in den Aufkleber-Pack-Ordner, in dem Aufkleber Asset-Katalog.

Um ein Bild in einem Aufkleber Pack einbezogen werden müssen sie die folgenden Anforderungen erfüllen:

- Images müssen in einem PNG, APNG, GIF- oder JPEG-Format sein. Apple empfiehlt, nur die Formaten PNG und APNG beim Aufkleber Ressourcen bereitstellen.
- Animierte Aufkleber unterstützen nur das APNG und GIF-Format.
- Aufkleber-Images sollte einen transparenten Hintergrund enthalten, da sie über Sprechblasen in der Konversation durch den Benutzer platziert werden können.
- Die einzelnen Bild-Dateien müssen weniger als 500 kb sein.
- Bilder, darf nicht kleiner als 100 x 100 Punkte oder größer, 206 x 206 zeigt sein.

> [!IMPORTANT]
> Aufkleber-Images sollte immer angegeben werden, auf die `@3x` Auflösung im Bereich von 300 x 300, 618 x 618 Pixel. Das System generiert automatisch die `@2x` und `@1x` Versionen zur Laufzeit nach Bedarf.

Apple empfiehlt Bildanlagen Aufkleber mit verschiedenen anderen farbigen Hintergrund (z. B. weiß, Schwarz, Rot, Gelb und mit mehreren farbig) und über Fotos, um sicherzustellen, dass sie die am besten suchen Sie in allen möglichen Situationen zu testen.

Aufkleber Packs können es sich um Aufkleber in einem der drei verfügbaren Größen bereitstellen:

- **Kleine** : 100 x 100 Punkte.
- **Mittel** – 136 x 136 Punkte. Dies ist die Standardgröße.
- **Große** - 206 x 206 Punkte.

Verwenden Xcodes Attributes Inspector zu Festlegen der Größe für das gesamte Aufkleber Pack nur Bildanlagen, die die angeforderte Größe für die besten Ergebnisse im Browser Aufkleber innerhalb der Nachrichten-app zu entsprechen.

Weitere Informationen finden Sie unsere [Eiskrem-Generator](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/) -app und Apple [Nachrichten Verweis](https://developer.apple.com/reference/messages).

## <a name="creating-a-custom-sticker-experience"></a>Erstellen eine benutzerdefinierte Aufkleber-Erfahrung

Wenn die app mehr Steuerelement oder Flexibilität als die von einem Aufkleber Pack bereitgestellt wird erfordert, kann enthalten eine Nachrichten-App-Erweiterung und Sticker über das Framework Nachrichten für eine benutzerdefinierte Aufkleber-Erfahrung bereitzustellen.

Was sind die Vorteile von einer benutzerdefinierten Aufkleber Erfahrung zu bieten?

1. Ermöglicht der app anpassen, wie die Aufkleber für die Benutzer der app angezeigt werden. Z. B. zum vorhanden Aufkleber in einem Format, die als standard-Grid-Layouts oder auf einem anderen farbigen Hintergrund.
2. Ermöglicht die Aufkleber von Code anstelle von insgesamt als statisches Bild Objekte dynamisch erstellt werden.
3. Ermöglicht die Aufkleber Bildanlagen dynamisch vom Webserver des Entwicklers heruntergeladen werden, ohne dass eine neue Version auf dem App Store veröffentlichen.
4. Können für den Zugriff, die der Kamera des Geräts, um Aufkleber auf dynamisch zu erstellen.
5. Können für In-App-Käufe, sodass der Benutzer weitere Aufkleber von innerhalb der app erwerben kann.

Erstellen Sie eine benutzerdefinierte Aufkleber Erfahrung, führen Sie folgende Schritte aus:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Starten Sie Visual Studio für Mac.
2. Öffnen Sie die Projektmappe, um eine Nachrichten-App-Erweiterung hinzufügen. 
3. Wählen Sie **iOS** > **Erweiterungen** > **iMessage-Erweiterung** , und klicken Sie auf die **Weiter** Schaltfläche: 

    [![](intro-to-message-app-extensions-images/message01.png "Wählen Sie iMessage-Erweiterung")](intro-to-message-app-extensions-images/message01.png#lightbox)
4. Geben Sie eine **Erweiterungsnamen** , und klicken Sie auf die **Weiter** Schaltfläche: 

    [![](intro-to-message-app-extensions-images/message02.png "Geben Sie einen Erweiterungsnamen")](intro-to-message-app-extensions-images/message02.png#lightbox)
5. Klicken Sie auf die **erstellen** um die Erweiterung zu erstellen: 

    [![](intro-to-message-app-extensions-images/message03.png "Klicken Sie auf die Schaltfläche \"erstellen\"")](intro-to-message-app-extensions-images/message03.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Starten Sie Visual Studio.
2. Öffnen Sie die Projektmappe, um eine Nachrichten-App-Erweiterung hinzufügen.
3. Wählen Sie **iOS-Erweiterungen > iMessage-Erweiterung (iOS)** , und klicken Sie auf die **Weiter** Schaltfläche:

    [![Wählen Sie iMessage-Erweiterung (iOS)](intro-to-message-app-extensions-images/message01.w157-sml.png)](intro-to-message-app-extensions-images/message01.w157.png#lightbox)

4. Geben Sie einen **Namen** , und klicken Sie auf die **OK** Schaltfläche

-----

In der Standardeinstellung die `MessagesViewController.cs` Datei wird zur Projektmappe hinzugefügt werden. Dies ist der Haupteinstiegspunkt bei der Erweiterung und erbt von der `MSMessageAppViewController` Klasse.

Das Nachrichten-Framework stellt Klassen zum Darstellen von verfügbaren Aufkleber auf den Benutzer:

- `MSStickerBrowserViewController` -Steuert die Ansicht, der im Sticker angezeigt werden. Es entspricht auch der `IMSStickerBrowserViewDataSource` Schnittstelle, um die Anzahl der Aufkleber und dem Aufkleber für einen bestimmten Browser Index zurückzugeben.
- `MSStickerBrowserView` : Dies ist die Ansicht, der in verfügbaren Sticker angezeigt werden.
- `MSStickerSize` -Entscheidet sich der Größe der einzelnen Zelle für das Raster der Aufkleber, die in der Browseransicht angezeigt.

### <a name="creating-a-custom-sticker-browser"></a>Erstellen einen benutzerdefinierten Aufkleber-Browser

Entwickler kann die Aufkleber-Erfahrung für den Benutzer durch die Bereitstellung von einem benutzerdefinierten Aufkleber Browser weiter anpassen (`MSMessageAppBrowserViewController`) in der Nachrichten-App-Erweiterung. Die benutzerdefinierte Aufkleber browseränderungen wie Aufkleber auf den Benutzer dargestellt werden, wenn sie den Aufkleber der Nachrichtendatenstrom einschließt auswählen.

Führen Sie folgende Schritte aus:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. In der **Lösungspad**mit der rechten Maustaste auf den Projektnamen für die Erweiterung, und wählen Sie **hinzufügen** > **neue Datei...**   >  **iOS | Apple Watch** > **Schnittstellencontroller**.
2. Geben Sie `StickerBrowserViewController` für die **Namen** , und klicken Sie auf die **neu** Schaltfläche: 

    [![](intro-to-message-app-extensions-images/browser01.png "Geben Sie ein StickerBrowserViewController")](intro-to-message-app-extensions-images/browser01.png#lightbox)
3. Öffnen der `StickerBrowserViewController.cs` Datei zur Bearbeitung.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. In der **Projektmappen-Explorer**mit der rechten Maustaste auf den Projektnamen für die Erweiterung, und wählen Sie **hinzufügen** > **neue Datei...**   >  **iOS | Apple Watch** > **Schnittstellencontroller**.
2. Geben Sie `StickerBrowserViewController` für die **Namen** , und klicken Sie auf die **neu** Schaltfläche: 

    [![](intro-to-message-app-extensions-images/browser01.w157-sml.png "Geben Sie ein StickerBrowserViewController")](intro-to-message-app-extensions-images/browser01.w157.png#lightbox)
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

Sehen Sie sich den Code oben im Detail an. Es wird Speicher für die Aufkleber, die die Erweiterung bietet erstellt:

```csharp
public List<MSSticker> Stickers { get; set; } = new List<MSSticker> ();
```

Und zwei Methoden überschreibt die `MSStickerBrowserViewController` Klasse, um Daten für diesen Datenspeicher durch den Browser bereitzustellen:

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

Die `CreateSticker` Methode ruft den Pfad ein Bildobjekt aus Paket mit der Erweiterung ab und verwendet, um eine neue Instanz der Erstellen einer `MSSticker` aus diesem Objekt, der der Auflistung hinzugefügt:

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

Die `LoadSticker` Methode wird aufgerufen, von `ViewDidLoad` Aufkleber aus dem benannten Image Asset (enthalten in der app Bundle) erstellen und auf die Auflistung von stickern hinzufügen.

Um den benutzerdefinierten Aufkleber Browser implementieren möchten, bearbeiten Sie die `MessagesViewController.cs` Datei, und stellen sie wie folgt aussehen:

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

Ein Blick auf diesen Code im Detail, wird der Speicher für den benutzerdefinierten Browser erstellt:

```csharp
public StickerBrowserViewController BrowserViewController { get; set;}
```

Und klicken Sie in der `ViewDidLoad` -Methode, es wird instanziiert und konfiguriert einen neuen Browser:

```csharp
// Create new browser and configure it
BrowserViewController = new StickerBrowserViewController (MSStickerSize.Regular);
BrowserViewController.View.Frame = View.Frame;
BrowserViewController.ChangeBackgroundColor (UIColor.Gray);
```

Klicken Sie dann wird den Browser an die Ansicht angezeigt:

```csharp
// Add to view
AddChildViewController (BrowserViewController);
BrowserViewController.DidMoveToParentViewController (this);
View.AddSubview (BrowserViewController.View);
```

### <a name="further-sticker-customization"></a>Weitere Aufkleber-Anpassung

Weitere Aufkleber Anpassung ist dazu nur zwei Klassen in der App-Erweiterung für Nachricht möglich:

- `MSStickerView`
- `MSSticker`

Verwenden die oben genannten Methoden ein, kann die Erweiterung Aufkleber Auswahl unterstützen, die nicht die standard-Aufkleber browsermethode abhängig ist. Darüber hinaus kann die Anzeige Aufkleber zwischen zwei unterschiedliche Ansichtsmodi umgeschaltet werden:

- **Compact** – Dies ist der Standardmodus, in denen die unteren 25 %, der die Ansicht "Nachricht" beansprucht die Aufkleber-Ansicht.
- **Erweitert** -Ansicht für die Aufkleber füllt die gesamte Nachrichtenansicht.

In dieser Ansicht Aufkleber kann zwischen diesen Modi entweder programmgesteuert oder manuell vom Benutzer umgeschaltet werden.

Sehen Sie sich im folgenden Beispiel der Wechsel zwischen den zwei unterschiedliche Ansichtsmodi zu verarbeiten. Zwei andere Ansichtscontroller werden für die einzelnen Zustände nachgewiesen. Die `StickerBrowserViewController` Handles der **Compact** anzeigen und sieht wie folgt aus:

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

Die `AddStickerViewController` übernimmt die **erweitert** Aufkleber anzeigen und sehen Sie wie folgt:

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

Die `MessageViewController` implementiert diese View-Controller, um den angeforderten Zustand zu steuern:

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

Wenn der Benutzer anfordert, um ihre verfügbar Auflistung einen neuen Aufkleber hinzuzufügen ein neues `AddStickerViewController` wird versucht, die sichtbar Controller und Ansicht-Aufkleber wechselt die **erweitert** anzeigen:

```csharp
// Switch to expanded view mode
Request (MSMessagesAppPresentationStyle.Expanded);
```

Wenn der Benutzer wählt einen Aufkleber hinzuzufügen, wird es ihre verfügbaren Auflistung hinzugefügt und die **Compact** Ansicht angefordert wird:

```csharp
public void AddStickerToCollection (MSSticker sticker)
{
    // Add sticker to collection
    BrowserViewController.Stickers.Add (sticker);

    // Switch to compact view mode
    Request (MSMessagesAppPresentationStyle.Compact);
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

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt enthalten eine Nachrichten-App-Erweiterung in einer Xamarin.iOS-Projektmappe, die integriert werden, die **Nachrichten** app "und" vorhanden, neue Funktionen für den Benutzer. Es wird beschrieben, mit der Erweiterung, um Text, Aufkleber, Mediendateien und interaktive Nachrichten zu senden.



## <a name="related-links"></a>Verwandte Links

- [Eiskrem-Generator (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/)
- [Nachrichten-Referenz](https://developer.apple.com/reference/messages)
- [Programmierhandbuch für App-Erweiterung](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)
