---
title: Bilder in Xamarin.Mac
description: In diesem Artikel wird das Arbeiten mit Bildern und Symbolen in einer Xamarin.Mac-Anwendung behandelt. Erstellen und Verwalten der Images erforderlich, um Ihrer Anwendung erstellen und Verwenden von Bildern in C#-Code und Interface Builder von Xcode beschrieben.
ms.prod: xamarin
ms.assetid: C6B539C2-FC6A-4C38-B839-32BFFB9B16A7
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/15/2017
ms.openlocfilehash: 8bc319b53e4a93d5cac35c4f8c3263b72dfe45e2
ms.sourcegitcommit: 9492e417f739772bf264f5944d6bae056e130480
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2018
ms.locfileid: "53746907"
---
# <a name="images-in-xamarinmac"></a>Bilder in Xamarin.Mac

_In diesem Artikel wird das Arbeiten mit Bildern und Symbolen in einer Xamarin.Mac-Anwendung behandelt. Erstellen und Verwalten der Images erforderlich, um Ihrer Anwendung erstellen und Verwenden von Bildern in C#-Code und Interface Builder von Xcode beschrieben._

## <a name="overview"></a>Übersicht

Bei der Arbeit mit c# und .NET in einer Xamarin.Mac-Anwendung haben Sie Zugang zu denselben Bild- und symboltools ein Entwickler *Objective-C-* und *Xcode* ist.

Es gibt mehrere Möglichkeiten, dieses Image an, die Ressourcen in eine MacOS (ehemals Mac OS X)-Anwendung verwendet werden. Anzeige von einfach ein Bild als Teil der Benutzeroberfläche Ihrer Anwendung, die an ein Benutzeroberflächenelement, z. B. eine Symbolleiste oder die Quelle Listenelement Vorlagenkörpers, für die Bereitstellung von Symbolen, erleichtert Xamarin.Mac hervorragende Grafiken für Ihre MacOS-Anwendungen auf folgende Weise hinzufügen : 

- **Elemente der Benutzeroberfläche** -Bilder können als Hintergrund oder als Teil Ihrer Anwendung in einer Ansicht Bild angezeigt werden (`NSImageView`).
- **Schaltfläche** -Bilder können auf Schaltflächen angezeigt werden (`NSButton`).
- **Bild der Zelle** – im Rahmen eines Steuerelements für die Tabelle auf der Basis (`NSTableView` oder `NSOutlineView`), Bilder können in einer Zelle Image verwendet werden (`NSImageCell`).
- **Symbolleistenelement** -Images eine Symbolleiste hinzugefügt werden können (`NSToolbar`) als ein Symbolleistenelement Image (`NSToolbarItem`).
- **Symbol "Liste" Quelle** – als Teil einer Quellliste (eine speziell formatierte `NSOutlineView`).
- **Symbol "App"** -eine Reihe von Bildern kann gruppiert werden in einer `.icns` festgelegt und als Symbol in Ihrer Anwendung verwendet. Finden Sie in unserer [Anwendungssymbol](~/mac/deploy-test/app-icon.md) Dokumentation zu informieren.

Darüber hinaus bietet MacOS eine Reihe von vordefinierten Images, die in der gesamten Anwendung verwendet werden kann.

[![Führen Sie ein Beispiel für die App](image-images/intro01.png "führen Sie ein Beispiel der app")](image-images/intro01-large.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit Bildern und Symbolen in einer Xamarin.Mac-Anwendung beschrieben. Es wird dringend empfohlen, dass Sie über arbeiten die [Hallo, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst, insbesondere die [Einführung in Xcode und Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und [Outlets und Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) Abschnitte, wie sie behandelt wichtige Konzepte und Techniken, die wir in diesem Artikel verwenden.


## <a name="adding-images-to-a-xamarinmac-project"></a>Hinzufügen von Bildern zu einem Xamarin.Mac-Projekt

Wenn Sie ein Image für die Verwendung in einer Xamarin.Mac-Anwendung hinzufügen, gibt es verschiedene Orte und Methoden, mit denen der Entwickler die Image-Datei des Projekts Quelle enthalten kann:

- **[Veraltet] Hauptprojekt-Struktur** -Images können direkt in der Struktur von Projekten hinzugefügt werden. Beim Aufrufen von Images, die in der Struktur Hauptprojekt aus Code gespeichert, wird kein Ordner angegeben. Beispiel: `NSImage image = NSImage.ImageNamed("tags.png");`. 
- **Ordner "Resources" [veraltet]** -die spezielle **Ressourcen** Ordner ist für jede Datei, die Teil der Anwendung wird die z. B. das Symbol "," Startbildschirm "oder" General bündeln, Images (oder andere Image oder der Entwickler-Datei möchte hinzufügen). Beim Aufrufen gespeicherte Bilder der **Ressourcen** Ordner von Code genau wie Bilder gespeichert, in der Struktur Hauptprojekt, kein Ordner angegeben ist. Beispiel: `NSImage.ImageNamed("tags.png")`.
- **Benutzerdefinierte Ordner oder Unterordner, die [veraltet]** – der Entwickler der Quellstruktur Projekten einen benutzerdefinierten Ordner hinzufügen und speichern Sie die Images vorhanden. Der Speicherort, in dem die Datei hinzugefügt wird, kann in einen Unterordner, um Unterstützung bei der das Projekt zu organisieren geschachtelt werden. Z. B., wenn der Entwickler hinzugefügt eine `Card` Ordner für das Projekt und ein Unterordner des `Hearts` für diesen Ordner ein Bild speichern **Jack.png** in die `Hearts` Ordner `NSImage.ImageNamed("Card/Hearts/Jack.png")` lädt das Bild an der Common Language Runtime.
- **[Bevorzugt] Bildzusammenstellungen für Asset-Katalog** – in OS X El Capitan hinzugefügt **Bildzusammenstellungen für Asset-Kataloge** enthalten alle Versionen oder Darstellungen eines Bilds, das sind erforderlich, um Unterstützung für verschiedene Geräte und Skalierungsfaktoren für Ihre die Anwendung. Anstatt auf den Namen der Bilddatei Assets (**@1x**, **@2x**).

<a name="asset-catalogs" />

### <a name="adding-images-to-an-asset-catalog-image-set"></a>Legen Sie das Hinzufügen von Bildern zu einem Bild, Asset-Katalog

Wie bereits erwähnt, ein **Bildzusammenstellungen für Asset-Kataloge** enthalten alle Versionen oder Darstellungen eines Bilds, die zum unterstützen verschiedener Geräte und Skalierungsfaktoren für Ihre Anwendung erforderlich sind. Anstatt auf den Namen der Bilddatei-Ressourcen (siehe die Auflösung unabhängige Images und Image-Nomenklatur oben), **Bildzusammenstellungen** der Ressourcen-Editor verwenden, um anzugeben, welches Image, welche Geräte und/oder Auflösung angehört.

1. In der **Lösungspad**, doppelklicken Sie auf die **Assets.xcassets** Datei, die sie für die Bearbeitung zu öffnen: 

    ![Auswählen der Assets.xcassets](image-images/imageset01.png "der Assets.xcassets auswählen")
2. Mit der rechten Maustaste auf die **Assetliste** , und wählen Sie **neue Image Menge**: 

    [![Hinzufügen einer neuen Gruppe von Bildern](image-images/imageset02.png "Hinzufügen einer neuen Gruppe von Bildern")](image-images/imageset02-large.png#lightbox)
3. Wählen Sie die neue Gruppe von Bildern, und der Editor wird angezeigt: 

    [![Wählen die neue Gruppe von Bildern](image-images/imageset03.png "Auswählen der neuen Gruppe von Bildern")](image-images/imageset03-large.png#lightbox)
4. Von hier aus können wir für jede der verschiedenen Geräten und Lösungen, die erforderlich sind in Bildern ziehen. 
5. Doppelklicken Sie auf die neue bildzusammenstellung **Namen** in die **Assetliste** zu bearbeiten: 

    [![Bearbeiten das Image znamen](image-images/imageset04.png "bearbeiten das Image Name festlegen")](image-images/imageset04-large.png#lightbox)
    
Eine spezielle **Vektor** Klasse hinzugefügt wurden **Bildzusammenstellungen** diese erlaubt uns, enthalten eine _PDF_ Vektorbild in die Casset stattdessen einschließlich einzelne Bitmapdateien im Format die verschiedenen Auflösungen. Mit dieser Methode können Sie angeben, eine einzelnen Vektor-Datei für die **@1x** Auflösung (formatiert als Vektor PDF-Datei) und die **@2x** und **@3x** Versionen der Datei zum Zeitpunkt der Kompilierung generiert und in der Anwendung Bündel enthalten.

[![Festlegen, welches Bild-Editor-Benutzeroberfläche](image-images/imageset05.png "festlegen, welches Bild-Editor-Benutzeroberfläche")](image-images/imageset05-large.png#lightbox)

Angenommen, Sie enthalten eine `MonkeyIcon.pdf` -Datei als der Vektor, der ein Asset-Katalog mit einer Auflösung von 150 x 150px, die folgenden Bitmap Objekte im endgültigen app-Paket enthalten sein, würde bei der Kompilierung:

1. **MonkeyIcon@1x.png** -150px x 150px Auflösung.
2. **MonkeyIcon@2x.png** -Auflösung von 300 px x 300px.
3. **MonkeyIcon@3x.png** -450px x 450px Auflösung.

Folgendes sollte berücksichtigt werden bei Verwendung von PDF-Vektorgrafiken in Ressourcenkataloge:

- Dies wird nicht vollständige Vektor als PDF-Datei gerasterte zu einer Bitmap zur Kompilierzeit und die Bitmaps, die in der fertigen Anwendung geliefert werden.
- Sie können nicht die Größe des Bilds anpassen, nachdem er in den Asset-Katalog festgelegt wurde. Wenn Sie, zum Ändern der Bildgröße (entweder im Code oder versuchen durch automatisches Layout mit Größenklassen) wird genau wie jede andere Bitmap das Bild verzerrt werden.

Bei Verwendung einer **Images** in Interface Builder von Xcode, können Sie einfach den Namen des Satzes auswählen, aus der Dropdownliste in der **Attributinspektor**: **

![Legen Sie mit der Sie ein Bild ausgewählt in Interface Builder von Xcode](image-images/imageset06.png "in Interface Builder von Xcode legen Sie ein Bild ausgewählt")

<a name="Adding-new-Assets-Collections"/>

### <a name="adding-new-assets-collections"></a>Hinzufügen von neuen Ressourcen-Sammlungen

Beim Arbeiten mit Images im Ressourcen-Kataloge gibt es möglicherweise vorkommen, wenn Sie eine neue Sammlung, anstatt alle Ihre Images erstellen möchten, die **Assets.xcassets** Auflistung. Beispielsweise beim Entwerfen von on-Demand-Ressourcen.

So fügen Sie einen neuen Katalog von Ressourcen zu Ihrem Projekt hinzu:

1. Mit der rechten Maustaste auf das Projekt in der **Lösungspad** , und wählen Sie **hinzufügen** > **neue Datei...**
2. Wählen Sie **Mac** > **Asset-Katalog**, geben Sie einen **Namen** für die Sammlung und klicken Sie auf die **neu** Schaltfläche: 

    ![Einen neuen Ressourcenkatalog hinzufügen](image-images/asset01.png "einen neuen Ressourcenkatalog hinzufügen")

Von hier aus können Sie mit der Sammlung arbeiten, auf die gleiche Weise als Standard **Assets.xcassets** Auflistung, die automatisch im Projekt enthalten ist.


### <a name="adding-images-to-resources"></a>Hinzufügen von Bildern auf Ressourcen

> [!IMPORTANT]
> Diese Methode für das Arbeiten mit Bildern in einer MacOS-app wurde von Apple als veraltet markiert. Verwenden Sie [Bildzusammenstellungen für Asset-Katalog](#asset-catalogs) auf Manager-Ihrer-app stattdessen images.

Bevor Sie eine Bilddatei in Ihre Xamarin.Mac-Anwendung (entweder im C#-Code oder von Interface Builder) verwenden, können sie des Projekts enthalten sein muss **Ressourcen** Ordner wie eine **Bundle Ressource**. Um eine Datei zu einem Projekt hinzuzufügen, führen Sie folgende Schritte aus:

1. Mit der rechten Maustaste auf die **Ressourcen** Ordner in Ihrem Projekt in der **Lösungspad** , und wählen Sie **hinzufügen** > **Dateien hinzufügen...** : 

    ![Hinzufügen einer Datei](image-images/add01.png "Hinzufügen einer Datei")
2. Aus der **Hinzufügen von Dateien** wählen Sie im Dialogfeld die Bilder von Dateien in dem Projekt hinzufügen auswählen `BundleResource` für die **Außerkraftsetzung Buildvorgang** , und klicken Sie auf die **öffnen** Schaltfläche:

    [![Auswählen der zu hinzuzufügenden Dateien](image-images/add02.png "den Dateien zum Hinzufügen auswählen")](image-images/add02-large.png#lightbox)
3. Wenn die Dateien nicht bereits in der **Ressourcen** Ordner, werden Sie aufgefordert, wenn Sie möchten **Kopie**, **verschieben** oder **Link** den Dateien. Auszuwählen, das alle Farben in der Regel, die Ihren Anforderungen **Kopie**:

    ![Bei Auswahl der hinzufügen-Aktion](image-images/add04.png "bei Auswahl der Aktion hinzufügen")
4. Die neuen Dateien werden aus Ihrer Datenbank im Projekt enthalten ist und für die Verwendung gelesen werden: 

    ![Die neue Bilddateien Pad "Projektmappe" hinzugefügt](image-images/add03.png "die neue Bilddateien Pad \"Projektmappe\" hinzugefügt")
5. Wiederholen Sie den Vorgang für alle Image-Dateien, die erforderlich sind.

Sie können alle Png, Jpg oder Pdf-Datei als ein Quellbild in Ihre Xamarin.Mac-Anwendung verwenden. Im nächsten Abschnitt betrachten wir hochauflösende Versionen unserer Bilder hinzufügen, und Symbole zur Unterstützung von Retina-basierten Macintosh-Computer.

> [!IMPORTANT]
> Bilder hinzufügen der **Ressourcen** Ordner lassen Sie die **Außerkraftsetzung Buildvorgang** festgelegt **Standard**. Der Standardwert für diesen Ordner Buildvorgang ist `BundleResource`.

## <a name="provide-high-resolution-versions-of-all-app-graphics-resources"></a>Geben Sie Versionen mit hoher Auflösung all app Graphics-Ressourcen

Alle grafischen Medienobjekt, das Sie einer Xamarin.Mac-Anwendung (Symbole, benutzerdefinierte Steuerelemente, benutzerdefinierte Cursor, benutzerdefinierte Grafiken, usw.) hinzufügen müssen Versionen mit hoher Auflösung, zusätzlich zu ihren standardauflösung-Versionen verfügen. Dies ist erforderlich, damit Ihre Anwendung empfiehlt es sich aussehen wird, bei der Ausführung auf einem Retina-Display Macintosh-Computer verfügen.


### <a name="adopt-the-2x-naming-convention"></a>Übernehmen Sie die @2x Benennungskonvention

> [!IMPORTANT]
> Diese Methode für das Arbeiten mit Bildern in einer MacOS-app wurde von Apple als veraltet markiert. Verwenden Sie [Bildzusammenstellungen für Asset-Katalog](#asset-catalogs) auf Manager-Ihrer-app stattdessen images.

Wenn Sie die standard und hochauflösenden Version des ein Image erstellen, verwenden Sie die Benennungskonvention für das Image-Paar, wenn sie Ihrem Xamarin.Mac-Projekt hinzufügen:

- **Standardauflösung**  - **ImageName.filename-Extension** (Beispiel: **tags.png**)
- **Mit hoher Auflösung**   -  **ImageName@2x.filename-extension** (Beispiel: **tags@2x.png**)

Wenn zu einem Projekt hinzugefügt, würden sie wie folgt aussehen:

![Die Image-Dateien im Projektmappenpad](image-images/add03.png "die Bilddateien im Projektmappenpad")

Wenn ein Element der Benutzeroberfläche in Interface Builder ein Bild zugewiesen wird, werden Sie einfach die Datei im Auswählen der _ImageName_**.** _Dateinamenerweiterung_ Format (Beispiel: **tags.png**). Für identisch mit einem Bild in c#-Code, müssen Sie die Datei im Auswählen der _ImageName_**.** _Dateinamenerweiterung_ Format.

Wenn Sie Xamarin.Mac-Anwendung auf einem Mac Ausführen der _ImageName_**.** _Dateinamenerweiterung_ Bild-Format wird auf Standard auflösenden anzeigen, wird die **ImageName@2x.filename-extension** Image ausgewählt wird automatisch auf Retina-Display verwendet als Basis für Macintosh-Computer.


## <a name="using-images-in-interface-builder"></a>Verwendung von Images in Interface Builder

Alle Image-Ressourcen, die Sie hinzugefügt haben, die **Ressourcen** Ordner in Ihre Xamarin.Mac Projekt, und den Buildvorgang festgelegt haben, um **BundleResource** werden automatisch in Interface Builder angezeigt und kann als Teil eines UI-Elements ausgewählt, (wenn es sich um Bilder behandelt).

Um ein Bild in Interface Builder zu verwenden, führen Sie folgende Schritte aus:

1. Fügen Sie ein Bild, um die **Ressourcen** Ordner mit einer **Buildvorgang** von `BundleResource`: 

     ![Eine Bildressource im Projektmappenpad](image-images/ib00.png "eine Bildressource im Projektmappenpad")
2. Doppelklicken Sie auf die **"Main.Storyboard"** Datei, die sie zur Bearbeitung in Interface Builder zu öffnen: 

     [![Bearbeiten die Haupt-Storyboard](image-images/ib01.png "bearbeiten die Haupt-Storyboard")](image-images/ib01-large.png#lightbox)
3. Ziehen Sie ein Element der Benutzeroberfläche, die Images auf die Entwurfsoberfläche akzeptiert (z. B. eine **Image Symbolleistenelement**): 

     ![Bearbeiten ein Symbolleistenelement](image-images/ib02.png "ein Symbolleistenelement bearbeiten")
4. Wählen Sie das Bild, das Sie hinzugefügt haben die **Ressourcen** Ordner in der **ImageName** Dropdownliste: 

     [![Auswählen eines Bildes für ein Symbolleistenelement](image-images/ib03.png "Auswählen eines Bildes für ein Symbolleistenelement")](image-images/ib03-large.png#lightbox)
5. Das ausgewählte Bild wird in der Entwurfsoberfläche angezeigt werden: 

     ![Das Bild angezeigt wird, in dem Symbolleisten-Editor](image-images/ib04.png "das Bild angezeigt wird, in dem Symbolleisten-Editor")
6. Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode synchronisiert.

Arbeiten Sie die oben genannten Schritte für jedes Benutzeroberflächenelement, das ermöglicht die Image-Eigenschaft festgelegt werden, der **Attributinspektor**. In diesem Fall, wenn Sie eingeschlossen haben eine **@2x** Version der Imagedatei, es wird automatisch auf verwendet Retina-Display-basierten Macintosh-Computer.

> [!IMPORTANT]
> Wenn das Bild nicht verfügbar ist die **ImageName** Dropdown-Liste, schließen Sie das Storyboard-Projekt in Xcode, und öffnen Sie es erneut in Visual Studio für Mac. Wenn das Bild immer noch nicht verfügbar ist, stellen Sie sicher, dass die **Buildvorgang** ist `BundleResource` und, der das Abbild hinzugefügt wurde die **Ressourcen** Ordner.

## <a name="using-images-in-c-code"></a>Verwendung von Images in c#-code

Wenn Sie ein Bild in den Arbeitsspeicher mit c#-Code in Ihre Xamarin.Mac-Anwendung zu laden, wird das Bild gespeichert werden, einem `NSImage` Objekt. Wenn die Image-Datei in das Xamarin.Mac-Anwendung-Paket (enthalten in Ressourcen) enthalten ist, verwenden Sie folgenden Code, um das Image laden:

```csharp
NSImage image = NSImage.ImageNamed("tags.png");
```

Der obige Code verwendet die statische `ImageNamed("...")` Methode der `NSImage` -Klasse zum Laden des angegebenen Bilds in den Speicher aus der **Ressourcen** Ordner, wenn das Bild kann nicht gefunden werden, `null` zurückgegeben werden. Wie Bild-, die in Interface Builder, zugewiesen, wenn Sie eingeschlossen haben eine **@2x** Version der Imagedatei, es wird automatisch auf verwendet Retina-Display-basierten Macintosh-Computer.

Um Bilder außerhalb der Anwendung Bundle (aus dem Mac-Dateisystem) zu laden, verwenden Sie den folgenden Code:

```csharp
NSImage image = new NSImage("/Users/KMullins/Documents/photo.jpg")
```

<a name="Working-with-Template-Images"/>

## <a name="working-with-template-images"></a>Arbeiten mit vorlagenimages

Basierend auf den Entwurf Ihrer App für MacOS, es gibt möglicherweise Zeiten, wenn Sie ein Symbol oder Bild innerhalb der Benutzeroberfläche eine Änderung am Farbschema (z. B. basierend auf benutzereinstellungen) entsprechend anpassen müssen.

Wechseln Sie zu diesem Zweck verwendet werden kann, die _Rendermodus_ Ihres Medienobjekts Image um **Vorlagenimage**:

[![Festlegen eines vorlagenimages](image-images/templateimage01.png "Festlegen eines vorlagenimages")](image-images/templateimage01-large.png#lightbox)

Weisen Sie das Image-Objekt von Interface Builder von Xcode UI-Steuerelement:

![Wählen ein Bild in Interface Builder von Xcode](image-images/templateimage02.png "Sie ein Bild in Interface Builder von Xcode ausgewählt.")

Oder optional die Bildquelle im Code festlegen:

```csharp
MyIcon.Image = NSImage.ImageNamed ("MessageIcon");
```

Fügen Sie die folgenden öffentlichen Funktion in den View-Controller hinzu:

```csharp
public NSImage ImageTintedWithColor(NSImage sourceImage, NSColor tintColor)
    => NSImage.ImageWithSize(sourceImage.Size, false, rect => {
        // Draw the original source image
        sourceImage.DrawInRect(rect, CGRect.Empty, NSCompositingOperation.SourceOver, 1f);

        // Apply tint
        tintColor.Set();
        NSGraphics.RectFill(rect, NSCompositingOperation.SourceAtop);

        return true;
    });
```

> [!IMPORTANT]
> Insbesondere bei der Einführung des dunklen-Modus in MacOS Mojave, es ist wichtig, um zu vermeiden der `LockFocus` API, die beim e rstellen Benutzerdefiniert Rendern `NSImage` Objekte. Diese Images werden statisch und werden auf Darstellung oder Anzeige Dichte Änderungen werden nicht automatisch aktualisiert.
>
> Durch den Einsatz der oben genannte Mechanismus Handler basierendes, erneut zu rendern für dynamische Bedingungen geschieht automatisch bei der `NSImage` gehostet wird, z. B. einer `NSImageView`.

Um eine Vorlagenbild getönt werden soll, rufen Sie anschließend diese Funktion für das Image zum farbigen anzeigen:

```csharp
MyIcon.Image = ImageTintedWithColor (MyIcon.Image, NSColor.Red);
```

<a name="Using_Images_with_Table_Views" />

## <a name="using-images-with-table-views"></a>Verwenden von Bildern mit Tabellenansichten

Um ein Bild im Rahmen der Zelle im enthalten eine `NSTableView`, müssen Sie ändern, wie die Daten zurückgegeben werden, von der `NSTableViewDelegate's` `GetViewForItem` zu verwendende Methode eine `NSTableCellView` statt der normalen `NSTextField`. Zum Beispiel:

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)tableView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTableCellView ();
        if (tableColumn.Title == "Product") {
            view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
            view.AddSubview (view.ImageView);
            view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
        } else {
            view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
        }
        view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
        view.AddSubview (view.TextField);
        view.Identifier = tableColumn.Title;
        view.TextField.BackgroundColor = NSColor.Clear;
        view.TextField.Bordered = false;
        view.TextField.Selectable = false;
        view.TextField.Editable = true;

        view.TextField.EditingEnded += (sender, e) => {

            // Take action based on type
            switch(view.Identifier) {
            case "Product":
                DataSource.Products [(int)view.TextField.Tag].Title = view.TextField.StringValue;
                break;
            case "Details":
                DataSource.Products [(int)view.TextField.Tag].Description = view.TextField.StringValue;
                break; 
            }
        };
    }

    // Tag view
    view.TextField.Tag = row;

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed ("tags.png");
        view.TextField.StringValue = DataSource.Products [(int)row].Title;
        break;
    case "Details":
        view.TextField.StringValue = DataSource.Products [(int)row].Description;
        break;
    }

    return view;
}
```

Es gibt hier ein paar Zeilen. Für Spalten, die ein Bild enthalten soll, wir erstellen Sie zunächst ein neues `NSImageView` der erforderliche Größe und den Speicherort, außerdem erstellen wir ein neues `NSTextField` und platzieren Sie die Position, basierend auf, und zwar unabhängig davon, ob wir ein Bild verwenden:

```csharp
if (tableColumn.Title == "Product") {
    view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
    view.AddSubview (view.ImageView);
    view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
} else {
    view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
}
```

Als Nächstes müssen wir die neuen Image View und Text-Feld in der übergeordneten enthalten `NSTableCellView`:

```csharp
view.AddSubview (view.ImageView);
...

view.AddSubview (view.TextField);
...

```

Abschließend müssen wir das Textfeld zu teilen, verkleinern und wachsen Sie mit der Tabellenzelle anzeigen können:

```csharp
view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
```

Beispielausgabe:

[![Ein Beispiel für das Anzeigen eines Bilds in einer app](image-images/tables01.png "ein Beispiel für das Anzeigen eines Bilds in einer app")](image-images/tables01-large.png#lightbox)

Weitere Informationen zum Arbeiten mit Tabellenansichten finden Sie unserem [Tabellenansichten](~/mac/user-interface/table-view.md) Dokumentation.

<a name="Using_Images_with_Outline_Views" />

## <a name="using-images-with-outline-views"></a>Verwenden von Bildern mit Gliederungsansichten

Um ein Bild enthalten, im Rahmen der Zelle in einer `NSOutlineView`, müssen Sie ändern, wie die Daten durch die Dokumentgliederung-Sicht zurückgegeben werden `NSTableViewDelegate's` `GetView` zu verwendende Methode eine `NSTableCellView` statt der normalen `NSTextField`. Zum Beispiel:

```csharp
public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
    // Cast item
    var product = item as Product;

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)outlineView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTableCellView ();
        if (tableColumn.Title == "Product") {
            view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
            view.AddSubview (view.ImageView);
            view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
        } else {
            view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
        }
        view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
        view.AddSubview (view.TextField);
        view.Identifier = tableColumn.Title;
        view.TextField.BackgroundColor = NSColor.Clear;
        view.TextField.Bordered = false;
        view.TextField.Selectable = false;
        view.TextField.Editable = !product.IsProductGroup;
    }

    // Tag view
    view.TextField.Tag = outlineView.RowForItem (item);

    // Allow for edit
    view.TextField.EditingEnded += (sender, e) => {

        // Grab product
        var prod = outlineView.ItemAtRow(view.Tag) as Product;

        // Take action based on type
        switch(view.Identifier) {
        case "Product":
            prod.Title = view.TextField.StringValue;
            break;
        case "Details":
            prod.Description = view.TextField.StringValue;
            break; 
        }
    };

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed (product.IsProductGroup ? "tags.png" : "tag.png");
        view.TextField.StringValue = product.Title;
        break;
    case "Details":
        view.TextField.StringValue = product.Description;
        break;
    }

    return view;
}
```

Es gibt hier ein paar Zeilen. Für Spalten, die ein Bild enthalten soll, wir erstellen Sie zunächst ein neues `NSImageView` der erforderliche Größe und den Speicherort, außerdem erstellen wir ein neues `NSTextField` und platzieren Sie die Position, basierend auf, und zwar unabhängig davon, ob wir ein Bild verwenden:

```csharp
if (tableColumn.Title == "Product") {
    view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
    view.AddSubview (view.ImageView);
    view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
} else {
    view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
}
```

Als Nächstes müssen wir die neuen Image View und Text-Feld in der übergeordneten enthalten `NSTableCellView`:

```csharp
view.AddSubview (view.ImageView);
...

view.AddSubview (view.TextField);
...

```

Abschließend müssen wir das Textfeld zu teilen, verkleinern und wachsen Sie mit der Tabellenzelle anzeigen können:

```csharp
view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
```

Beispielausgabe:

[![Ein Beispiel für ein Bild angezeigt wird, in einer Gliederungsansicht](image-images/outline01.png "ein Beispiel für ein Bild in einer Gliederungsansicht angezeigt wird")](image-images/outline01-large.png#lightbox)

Weitere Informationen zum Arbeiten mit Gliederungsansichten informieren Sie sich unsere [Gliederungsansichten](~/mac/user-interface/outline-view.md) Dokumentation.


## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird eine ausführliche Übersicht über das Arbeiten mit Bildern und Symbolen in einer Xamarin.Mac-Anwendung verwendet. Wir haben gesehen, die unterschiedlichen Typen und der Bilder, wie Sie Bilder und Symbole in Interface Builder von Xcode verwenden und das Arbeiten mit Bildern und Symbolen in C#-Code verwendet.



## <a name="related-links"></a>Verwandte Links

- [MacImages (Beispiel)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Tabellenansichten](~/mac/user-interface/table-view.md)
- [Gliederungsansichten](~/mac/user-interface/outline-view.md)
- [Mac OS X Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
- [Informationen über hohe Auslösung für OS X](https://developer.apple.com/library/content/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Introduction/Introduction.html)
