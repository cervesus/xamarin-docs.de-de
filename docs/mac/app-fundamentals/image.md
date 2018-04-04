---
title: Bilder
description: In diesem Artikel wird das Arbeiten mit Bildern und Symbolen in einer Anwendung Xamarin.Mac behandelt. Erstellen und Verwalten der Images erforderlich, um das Symbol für Ihre Anwendung erstellen und Verwenden von Bildern in C#-Code und Xcodes Benutzeroberflächen-Generator beschrieben.
ms.prod: xamarin
ms.assetid: C6B539C2-FC6A-4C38-B839-32BFFB9B16A7
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: dc33dc78c09c0b5b7cb7533afdd2f95b8ebd9c4e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="images"></a>Bilder

_In diesem Artikel wird das Arbeiten mit Bildern und Symbolen in einer Anwendung Xamarin.Mac behandelt. Erstellen und Verwalten der Images erforderlich, um das Symbol für Ihre Anwendung erstellen und Verwenden von Bildern in C#-Code und Xcodes Benutzeroberflächen-Generator beschrieben._


## <a name="overview"></a>Übersicht

Bei der Arbeit mit c# und .NET in einer Anwendung Xamarin.Mac haben Sie Zugriff auf das gleiche Bild und Symbol für tools, die ein Entwickler arbeiten in *Objective-C* und *Xcode* verfügt.

Es gibt mehrere Möglichkeiten dieses Abbilds verwendete Ressourcen in einer Anwendung MacOS (ehemals Mac OS X). Aus einfach Anzeigen eines Bilds als Teil der Benutzeroberfläche Ihrer Anwendung, die an ein Benutzeroberflächensteuerelement, wie eine Symbolleiste oder Quelle List-Item, Symbole versichern zuweisen, erleichtert Xamarin.Mac hervorragende Bildmaterial den Clientanwendungen MacOS auf folgende Weise hinzufügen : 

- **Elemente der Benutzeroberfläche** -Bilder können als Hintergrund oder als Teil der Anwendung in einer Ansicht des Bilds angezeigt werden (`NSImageView`).
- **Schaltfläche** -Images in Schaltflächen angezeigt werden können (`NSButton`).
- **Bild der Zelle** – als Teil einer Tabelle auf der Basis-Steuerelements (`NSTableView` oder `NSOutlineView`), Bilder in einer Zelle Image verwendet werden können (`NSImageCell`).
- **Symbolleistenelement** -Images eine Symbolleiste hinzugefügt werden können (`NSToolbar`) als Image Symbolleistenelement (`NSToolbarItem`).
- **Symbol "Liste" Quelle** – als Teil einer Quellliste (eine speziell formatierte `NSOutlineView`).
- **Symbol "App"** -eine Reihe von Bildern kann zusammen gruppiert werden, in einem `.icns` festgelegt und als Symbol für Ihre Anwendung verwendet. Finden Sie in unserem [Anwendungssymbol](~/mac/deploy-test/app-icon.md) Dokumentation weitere Informationen.

Darüber hinaus bietet MacOS einen Satz von vordefinierte Images, die in der gesamten Anwendung verwendet werden kann.

[![Führen Sie ein Beispiel für die App](image-images/intro01.png "führen Sie ein Beispiel der app")](image-images/intro01-large.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit Bildern und Symbolen in einer Anwendung Xamarin.Mac eingegangen. Wird mit hoher vorgeschlagen, dass Sie über arbeiten die [Hello, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst, insbesondere die [Einführung in Xcode und Benutzeroberflächen-Generator](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) und [Steckdosen und Aktionen](~/mac/get-started/hello-mac.md#Outlets_and_Actions) Abschnitte, wie sie behandelt wichtige Konzepte und Techniken, die in diesem Artikel verwendet werden.


## <a name="adding-images-to-a-xamarinmac-project"></a>Hinzufügen von Bildern zu einem Projekt Xamarin.Mac

Wenn Sie ein Bild für die Verwendung in einer Anwendung Xamarin.Mac hinzufügen, gibt es mehrere Orte und Methoden, mit denen Entwickler Bilddatei, die Quelle für das Projekt einschließen kann:

- **[Veraltet] Hauptprojekt-Struktur** -Images können direkt in die Struktur der Projekte hinzugefügt werden. Beim Aufrufen von Bildern in das Hauptprojekt-Struktur aus Code gespeichert, wird kein Speicherort angegeben. Beispiel: `NSImage image = NSImage.ImageNamed("tags.png");`. 
- **[Veraltet] Ressourcenordner** -die speziellen **Ressourcen** Ordner wird für jede Datei, die Bestandteil der Anwendung werden der z. B. Symbol, Bildschirm gestartet oder Allgemein bündeln, Bilder (oder eine beliebige andere Bild- oder -Datei der Entwickler möchte hinzufügen). Beim Aufrufen von Abbildern der **Ressourcen** Ordner aus Code, so wie Bilder gespeichert, in der Struktur Hauptprojekt, keine Ordnerspeicherort angegeben ist. Beispiel: `NSImage.ImageNamed("tags.png")`.
- **Benutzerdefinierte Ordner oder Unterordner [veraltet]** – der Entwickler der Quellstruktur Projekte hinzufügen ein benutzerdefiniertes Ordners und speichern Sie die Bilder vorhanden. Der Speicherort, wo die Datei hinzugefügt wird, kann in einen Unterordner, um weitere Hilfe organisieren das Projekt geschachtelt werden. Z. B., wenn der Entwickler hinzugefügt eine `Card` Ordner für das Projekt und ein Unterordner des `Hearts` zu diesem Ordner speichern ein Bild **Jack.png** in der `Hearts` Ordner `NSImage.ImageNamed("Card/Hearts/Jack.png")` lädt das Abbild Common Language Runtime.
- **Asset-Katalog Image Mengen [empfohlen]** - hinzugefügten OS X El Capitan **Asset Kataloge Image legt** enthalten alle Versionen oder Darstellungen eines Bilds, die zur Unterstützung von verschiedenen Geräten und Skalierungsfaktoren für erforderlich sind Ihre die Anwendung. Anstatt auf den Dateinamen des Bilds Bestand (**@1x**, **@2x**).

<a name="asset-catalogs" />

### <a name="adding-images-to-an-asset-catalog-image-set"></a>Legen Sie das Hinzufügen von Bildern zu einem Bild, Asset-Katalog

Wie bereits erwähnt, ein **Asset Kataloge Image legt** enthalten alle Versionen oder Darstellungen eines Bilds, die zur Unterstützung von verschiedenen Geräten und Skalierungsfaktoren für Ihre Anwendung erforderlich sind. Anstatt auf den Dateinamen des Image-Ressourcen (Siehe Auflösung unabhängige Bilder und Image-Nomenklatur oben), **Image legt** mithilfe des Ressourcen-Editors angeben, welches Abbild welche Gerätetyps und/oder Auflösung gehört.

1. In der **Lösung Pad**, doppelklicken Sie auf die **Assets.xcassets** Datei zur Bearbeitung zu öffnen: 

    ![Auswählen der Assets.xcassets](image-images/imageset01.png "der Assets.xcassets auswählen")
2. Mit der rechten Maustaste auf die **Inventarliste** , und wählen Sie **neue Bildersatz**: 

    [![Hinzufügen eines neuen Image Satzes](image-images/imageset02.png "einen neuer Satz hinzufügen")](image-images/imageset02-large.png#lightbox)
3. Wählen Sie die neue Image und der Editor wird angezeigt: 

    [![Auswählen der neuen Bildersatz](image-images/imageset03.png "den neuen Satz von Image auswählen")](image-images/imageset03-large.png#lightbox)
4. Hier können wir für jeden der verschiedenen Geräten und Lösungen erforderlich in Bildern ziehen. 
5. Doppelklicken Sie auf die neue Bildersatz **Namen** in der **Inventarliste** zu bearbeiten: 

    [![Bearbeiten das Bild Satzname](image-images/imageset04.png "bearbeiten das Bild Satzname")](image-images/imageset04-large.png#lightbox)
    
Eine spezielle **Vektor** Klasse hinzugefügt wurden **Image legt** , die uns ermöglicht, enthalten eine _PDF_ Vektorbild in die Casset stattdessen auch einzelne Bitmapdateien am formatiert die verschiedenen Lösungen. Mit dieser Methode an, Sie geben Sie eine einzelnen Vektor-Datei für die **@1x** Auflösung (formatiert als Vektor PDF-Datei) und die **@2x** und **@3x** Versionen der Datei zum Zeitpunkt der Kompilierung generiert und in der Anwendung-Paket enthalten.

[![Festlegen, welches Bild-Editor-Benutzeroberfläche](image-images/imageset05.png "festlegen, welches Bild-Editor-Benutzeroberfläche")](image-images/imageset05-large.png#lightbox)

Angenommen, Sie enthalten eine `MonkeyIcon.pdf` Datei wie der Vektor, der eine Asset-Katalog mit einer Auflösung von 150px x 150px, Bitmap für die folgenden Objekte im endgültigen app-Paket hinzugefügt werden, würde bei der Kompilierung:

1. **MonkeyIcon@1x.png** -150px x 150px Auflösung.
2. **MonkeyIcon@2x.png** -300px x 300px Auflösung.
3. **MonkeyIcon@3x.png** -450px x 450px Auflösung.

Folgendes sollte berücksichtigt werden bei Verwendung von PDF-Vektorgrafiken in Asset Kataloge:

- Dies ist nicht voll Vektor-Unterstützung, PDF-Datei in eine Bitmap gerasterte zur Kompilierzeit und die Bitmaps, die in der endgültigen Anwendung ausgeliefert werden.
- Sie können nicht die Größe des Bilds anpassen, nachdem er in den Asset-Katalog festgelegt wurde. Wenn Sie, zum Ändern der Bildgröße (entweder im Code oder versuchen mithilfe von Automatisches Layout und Klassen Größe) werden genau wie alle anderen Bitmap das Bild verzerrt werden.

Bei Verwendung einer **Bildersatz** in Xcode Schnittstelle-Generator können Sie einfach den Satz von Namen auswählen, aus der Dropdownliste in der **Attribut Inspektor**: **

![Auswählen eines Bildes in Xcodes Benutzeroberflächen-Generator festlegen](image-images/imageset06.png "wählen ein Image in Xcodes Benutzeroberflächen-Generator festlegen")

<a name="Adding-new-Assets-Collections"/>

### <a name="adding-new-assets-collections"></a>Hinzufügen von neuen Bestand Sammlungen

Beim Arbeiten mit Bildern im Bestand Kataloge möglicherweise manchmal Sie eine neue Sammlung, anstatt alle Bilder zum erstellen möchten der **Assets.xcassets** Auflistung. Beispielsweise, wenn eine bedarfsgesteuerte Ressourcen zu entwerfen.

So fügen Sie einen neuen Katalog von Ressourcen zu Ihrem Projekt hinzu:

1. Mit der rechten Maustaste auf das Projekt in der **Lösung Pad** , und wählen Sie **hinzufügen** > **neue Datei...**
2. Wählen Sie **Mac** > **Asset-Katalog**, geben Sie einen **Namen** für die Sammlung klicken Sie auf die **neu** Schaltfläche: 

    ![Ein neuer Asset-Katalog hinzufügen](image-images/asset01.png "ein neuer Asset-Katalog hinzufügen")

Von hier aus können Sie mit der Auflistung arbeiten, auf die gleiche Weise als Standard **Assets.xcassets** Auflistung, die im Projekt automatisch einbezogen.


### <a name="adding-images-to-resources"></a>Hinzufügen von Bildern auf Ressourcen

> [!IMPORTANT]
> Diese Methode der Arbeit mit Bildern in einer app MacOS wurde von Apple als veraltet markiert. Verwenden Sie [Asset-Katalog-Image-Sätze](#asset-catalogs) -Manager wird Ihre app stattdessen Bilder.

Vor der Verwendung einer Bilddatei in der Xamarin.Mac-Anwendung (entweder im C#-Code oder von Benutzeroberflächen-Generator) muss es in des Projekts aufgenommen werden **Ressourcen** Ordner wie eine **Bundle Ressource**. Um eine Datei zu einem Projekt hinzuzufügen, führen Sie folgende Schritte aus:

1. Mit der rechten Maustaste auf die **Ressourcen** Ordner des Projekts in der **Lösung Pad** , und wählen Sie **hinzufügen** > **Dateien hinzufügen...** : 

    ![Hinzufügen einer Datei](image-images/add01.png "Hinzufügen einer Datei")
2. Aus der **Hinzufügen von Dateien** wählen Sie im Dialogfeld die Bilder dem Projekt hinzufügen-Dateien wählen `BundleResource` für die **Außerkraftsetzung Buildvorgang** , und klicken Sie auf die **öffnen** Schaltfläche:

    [![Auswählen der zu hinzuzufügenden Dateien](image-images/add02.png "Auswählen der zu hinzuzufügenden Dateien")](image-images/add02-large.png#lightbox)
3. Wenn die Dateien nicht bereits Bestandteil der **Ressourcen** Ordner, Sie werden gefragt, ob Sie möchten **Kopie**, **verschieben** oder **Link** Dateien. Auszuwählen, das alle Farben in der Regel, die Ihren Anforderungen **Kopie**:

    ![Auswählen der Aktion hinzufügen](image-images/add04.png "Add-Aktion auswählen")
4. Neue Dateien im Projekt enthalten und zur Verwendung lesen: 

    ![Die neue Bilddateien hinzugefügt, das Auffüllzeichen Lösung](image-images/add03.png "die neue Bilddateien das Auffüllzeichen Lösung hinzugefügt")
5. Wiederholen Sie den Vorgang für alle Image-Dateien, die erforderlich sind.

Sie können eine beliebige Png, Jpg oder Pdf-Datei als ein Quellabbild in Ihrer Anwendung Xamarin.Mac verwenden. Im nächsten Abschnitt betrachten wir hochauflösende Versionen unserer Bilder hinzufügen und Symbole zur Unterstützung von Retina-basierten Macs.

> [!IMPORTANT]
> Wenn Sie Bilder hinzufügen der **Ressourcen** Ordner lassen Sie die **Außerkraftsetzung Buildvorgang** festgelegt **Standard**. Die Standardeinstellung Buildvorgang für diesen Ordner ist `BundleResource`.

## <a name="provide-high-resolution-versions-of-all-app-graphics-resources"></a>Geben Sie die Versionen aus allen Grafikressourcen der app

Ein Grafik Bestand, den Sie einer Anwendung Xamarin.Mac (Symbole, benutzerdefinierte Steuerelemente, benutzerdefiniertem Cursor, benutzerdefinierte Bildmaterial usw.) hinzufügen Versionen zusätzlich zu ihrer Auflösung Standard-Versionen sind erforderlich. Dies ist erforderlich, damit, dass Ihre Anwendung empfiehlt es sich bei der Ausführung auf einem Retina-Display auf Macintosh-Computer ausgestattet aussehen.


### <a name="adopt-the-2x-naming-convention"></a>Übernehmen der @2x Namenskonvention

> [!IMPORTANT]
> Diese Methode der Arbeit mit Bildern in einer app MacOS wurde von Apple als veraltet markiert. Verwenden Sie [Asset-Katalog-Image-Sätze](#asset-catalogs) -Manager wird Ihre app stattdessen Bilder.

Wenn Sie die standard und hochauflösenden Version eines Bilds erstellen, verwenden Sie die Benennungskonvention für das Image-Paar, wenn sie in Ihrem Projekt Xamarin.Mac einschließlich:

- **Standard-Auflösung**  - **ImageName.filename-Erweiterung** (Beispiel: **tags.png**)
- **Hochauflösende**   -  **ImageName@2x.filename-extension** (Beispiel: **tags@2x.png**)

Wenn Sie einem Projekt hinzugefügt, würden sie wie folgt aussehen:

![Der Bilddateien in der Projektmappe Pad](image-images/add03.png "der Bilddateien in der Projektmappe mit Leerstellen auffüllen")

Bei einem Benutzeroberflächenelement im Benutzeroberflächen-Generator wird ein Bild zugeordnet ist, müssen Sie einfach die Datei im Auswählen der _ImageName_**.** _-Erweiterung_ Format (Beispiel: **tags.png**). Gleich für ein Bild im C#-Code verwenden, müssen Sie die Datei im Auswählen der _ImageName_**.** _-Erweiterung_ Format.

Wenn Sie Xamarin.Mac-Anwendung auf einem Mac ausgeführt werden die _ImageName_**.** _-Erweiterung_ Format-Bild wird auf Standard Lösung zeigt, verwendet die **ImageName@2x.filename-extension** Image wird automatisch auf übernommen Retina Display Basen Macs.


## <a name="using-images-in-interface-builder"></a>Verwenden von Bildern in Benutzeroberflächen-Generator

Alle Bildressource, die Sie hinzugefügt haben, die **Ressourcen** Ordner in Ihrem Xamarin.Mac Projekt und den Buildvorgang festgelegt haben, um **BundleResource** werden automatisch in der Benutzeroberflächen-Generator angezeigt und kann als Teil eines Benutzeroberflächenelements ausgewählt, (falls sie Bilder behandelt).

Um ein Bild im Benutzeroberflächen-Generator zu verwenden, führen Sie folgende Schritte aus:

1. Hinzufügen eines Bilds auf der **Ressourcen** Ordner mit einer **Buildvorgang** von `BundleResource`: 

     ![Eine Bildressource in der Projektmappe Pad](image-images/ib00.png "können eine Bildressource in der Projektmappe mit Leerstellen auffüllen")
2. Doppelklicken Sie auf die **Main.storyboard** Datei, um ihn zur Bearbeitung in der Benutzeroberflächen-Generator zu öffnen: 

     [![Bearbeiten die Haupt-Storyboard](image-images/ib01.png "Haupt-Storyboard bearbeiten")](image-images/ib01-large.png#lightbox)
3. Ziehen Sie ein Element der Benutzeroberfläche, die Bilder auf der Entwurfsoberfläche akzeptiert (z. B. eine **Image Symbolleistenelement**): 

     ![Bearbeiten von Symbolleistenelement](image-images/ib02.png "Symbolleistenelement bearbeiten")
4. Wählen Sie das Bild, das Sie hinzugefügt haben die **Ressourcen** Ordner in der **ImageName** Dropdownliste: 

     [![Auswählen eines Bildes für ein Symbolleistenelement](image-images/ib03.png "Auswählen eines Bildes für ein Symbolleistenelement")](image-images/ib03-large.png#lightbox)
5. Das ausgewählte Bild wird in der Entwurfsoberfläche angezeigt werden: 

     ![Das Bild angezeigt wird, in dem Symbolleisten-Editor](image-images/ib04.png "das Bild angezeigt wird, in dem Symbolleisten-Editor")
6. Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.

Die oben genannten Schritte für jedes Benutzeroberflächenelement, das ihre Bildeigenschaft festgelegt werden, ermöglicht funktionieren die **Attribut Inspektor**. Erneut, wenn Sie aufgenommen haben eine **@2x** Version der Imagedatei, es wird automatisch auf verwendet Retina Display basierend Macs.

> [!IMPORTANT]
> Wenn das Bild nicht verfügbar in ist die **ImageName** Dropdownliste das .storyboard-Projekt in Xcode zu schließen und erneut öffnen von Visual Studio für Mac. Wenn das Bild immer noch nicht verfügbar ist, stellen Sie sicher, dass seine **Buildvorgang** ist `BundleResource` und, die das Bild hinzugefügt wurde die **Ressourcen** Ordner.

## <a name="using-images-in-c-code"></a>Verwenden von Bildern in C#-code

Wenn ein Bild in den Arbeitsspeicher, die mithilfe von C#-Code in Ihrer Anwendung Xamarin.Mac zu laden, wird das Abbild gespeichert einer `NSImage` Objekt. Sofern die Bilddatei im Anwendungspaket Xamarin.Mac (enthalten in Ressourcen) aufgenommen wurde, verwenden Sie den folgenden Code beim Laden des Bilds:

```csharp
NSImage image = NSImage.ImageNamed("tags.png");
```

Der obige Code verwendet die statische `ImageNamed("...")` Methode der `NSImage` Klasse beim Laden des angegebenen Bilds in den Arbeitsspeicher aus der **Ressourcen** Ordner, wenn das Bild kann nicht gefunden werden, `null` zurückgegeben werden. Wie Bilder, die in der Schnittstelle-Generator zugewiesen, wenn Sie aufgenommen haben eine **@2x** Version der Imagedatei, es wird automatisch auf verwendet Retina Display basierend Macs.

Um Bilder außerhalb der Anwendungspaket (aus dem Mac-Dateisystem) zu laden, verwenden Sie den folgenden Code ein:

```csharp
NSImage image = new NSImage("/Users/KMullins/Documents/photo.jpg")
```

<a name="Working-with-Template-Images"/>

## <a name="working-with-template-images"></a>Arbeiten mit vorlagenimages

Basierend auf den Entwurf der app MacOS kann es möglicherweise vorkommen, dass Sie ein Symbol oder ein Bild in der Benutzeroberfläche eine Änderung im Farbschema (z. B. basierend auf den benutzereinstellungen) entsprechend anpassen müssen.

Um diesen Effekt zu erzielen, wechseln die _Rendermodus_ von Ihrer Imagemedienobjekt auf **Vorlagenimage**:

[![Festlegen eines vorlagenimages](image-images/templateimage01.png "Festlegen eines vorlagenimages")](image-images/templateimage01-large.png#lightbox)

Weisen Sie die Xcode-Schnittstelle-Generator Standardimage-Medienobjekt an ein Benutzeroberflächensteuerelement:

![Wählen ein Image in Xcodes Benutzeroberflächen-Generator](image-images/templateimage02.png "wählen ein Image in Xcodes Benutzeroberflächen-Generator")

Oder legen Sie optional die Bildquelle im Code:

```csharp
MyIcon.Image = NSImage.ImageNamed ("MessageIcon");
```

Die View-Controller die folgenden öffentliche Funktion hinzufügen:

```csharp
public NSImage ImageTintedWithColor (NSImage image, NSColor tint)
{
    var tintedImage = image.Copy () as NSImage;
    var frame = new CGRect (0, 0, image.Size.Width, image.Size.Height);

    // Apply tint
    tintedImage.LockFocus ();
    tint.Set ();
    NSGraphics.RectFill (frame, NSCompositingOperation.SourceAtop);
    tintedImage.UnlockFocus ();
    tintedImage.Template = false;

    // Return tinted image
    return tintedImage;
}
```

Um eine Vorlagenbild getönt werden soll, rufen Sie schließlich diese Funktion für das Bild, das zur farblichen Kennzeichnung der:

```csharp
MyIcon.Image = ImageTintedWithColor (MyIcon.Image, NSColor.Red);
```

<a name="Using_Images_with_Table_Views" />

## <a name="using-images-with-table-views"></a>Verwenden von Bildern mit Tabellensichten

Um ein Bild als Teil der Zelle enthalten eine `NSTableView`, müssen Sie dagegen ändern, wie die Daten von der Tabellenansicht zurückgegeben werden `NSTableViewDelegate's` `GetViewForItem` zu verwendende Methode eine `NSTableCellView` anstelle der normalen `NSTextField`. Zum Beispiel:

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

Es gibt ein paar Textzeilen hier ein. Für Spalten, die ein Bild enthalten soll, wir erstellen Sie zunächst ein neues `NSImageView` die erforderliche Größe und den Speicherort, wir außerdem erstellen Sie ein neues `NSTextField` und platzieren Sie die Position, basierend auf, und zwar unabhängig davon, ob ein Bild verwenden:

```csharp
if (tableColumn.Title == "Product") {
    view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
    view.AddSubview (view.ImageView);
    view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
} else {
    view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
}
```

Zweitens müssen wir die neuen Bild anzeigen und Text-Feld in der übergeordneten Tabelle umfassen `NSTableCellView`:

```csharp
view.AddSubview (view.ImageView);
...

view.AddSubview (view.TextField);
...

```

Schließlich müssen wir das folgende Textfeld ein, mit der Ansicht Tabellenzelle erweitert und verkleinert werden können:

```csharp
view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
```

Beispielausgabe:

[![Ein Beispiel für das Anzeigen eines Bilds in eine app](image-images/tables01.png "ein Beispiel für das Anzeigen eines Bilds in einer app")](image-images/tables01-large.png#lightbox)

Weitere Informationen zum Arbeiten mit Tabellensichten finden Sie unter unsere [Tabellensichten](~/mac/user-interface/table-view.md) Dokumentation.

<a name="Using_Images_with_Outline_Views" />

## <a name="using-images-with-outline-views"></a>Verwenden von Bildern mit Gliederung Ansichten

Um ein Bild enthalten, als Teil der Zelle in einer `NSOutlineView`, müssen Sie dagegen ändern, wie die Daten über die Gliederungsansicht zurückgegeben werden `NSTableViewDelegate's` `GetView` zu verwendende Methode eine `NSTableCellView` anstelle der normalen `NSTextField`. Zum Beispiel:

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

Es gibt ein paar Textzeilen hier ein. Für Spalten, die ein Bild enthalten soll, wir erstellen Sie zunächst ein neues `NSImageView` die erforderliche Größe und den Speicherort, wir außerdem erstellen Sie ein neues `NSTextField` und platzieren Sie die Position, basierend auf, und zwar unabhängig davon, ob ein Bild verwenden:

```csharp
if (tableColumn.Title == "Product") {
    view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
    view.AddSubview (view.ImageView);
    view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
} else {
    view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
}
```

Zweitens müssen wir die neuen Bild anzeigen und Text-Feld in der übergeordneten Tabelle umfassen `NSTableCellView`:

```csharp
view.AddSubview (view.ImageView);
...

view.AddSubview (view.TextField);
...

```

Schließlich müssen wir das folgende Textfeld ein, mit der Ansicht Tabellenzelle erweitert und verkleinert werden können:

```csharp
view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
```

Beispielausgabe:

[![Ein Beispiel für ein Bild angezeigt wird, in einer Gliederungsansicht](image-images/outline01.png "ein Beispiel für ein Bild in einer Gliederungsansicht angezeigt wird")](image-images/outline01-large.png#lightbox)

Weitere Informationen zum Arbeiten mit Gliederung Ansichten finden Sie unter unsere [Gliederung Ansichten](~/mac/user-interface/outline-view.md) Dokumentation.


## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat eine ausführliche Übersicht über das Arbeiten mit Bildern und Symbolen in einer Anwendung Xamarin.Mac übernommen. Wir gesehen haben die verschiedenen Typen und der Bilder, die zum Verwenden von Bildern und Symbolen im Xcodes Benutzeroberflächen-Generator und zum Arbeiten mit Bildern und Symbolen in C#-Code verwendet.



## <a name="related-links"></a>Verwandte Links

- [MacImages (Beispiel)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Tabellenansichten](~/mac/user-interface/table-view.md)
- [Gliederung-Ansichten](~/mac/user-interface/outline-view.md)
- [Mac OS X-Richtlinien für menschliche Benutzeroberfläche](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
- [Informationen über hohe Auslösung für OS X](https://developer.apple.com/library/content/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Introduction/Introduction.html)
