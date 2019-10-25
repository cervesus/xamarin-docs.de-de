---
title: Bilder in xamarin. Mac
description: In diesem Artikel wird das Arbeiten mit Bildern und Symbolen in einer xamarin. Mac-Anwendung behandelt. Es beschreibt das Erstellen und Verwalten der zum Erstellen des Anwendungs Symbols benötigten Bilder und die Verwendung von Bildern C# sowohl im Code als auch in der Interface Builder von Xcode.
ms.prod: xamarin
ms.assetid: C6B539C2-FC6A-4C38-B839-32BFFB9B16A7
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/15/2017
ms.openlocfilehash: 99604b59e5557ba5a7aa3d5ba61bc1bff414f000
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "70770324"
---
# <a name="images-in-xamarinmac"></a>Bilder in xamarin. Mac

_In diesem Artikel wird das Arbeiten mit Bildern und Symbolen in einer xamarin. Mac-Anwendung behandelt. Es beschreibt das Erstellen und Verwalten der zum Erstellen des Anwendungs Symbols benötigten Bilder und die Verwendung von Bildern C# sowohl im Code als auch in der Interface Builder von Xcode._

## <a name="overview"></a>Übersicht

Wenn Sie mit C# und .net in einer xamarin. Mac-Anwendung arbeiten, haben Sie Zugriff auf dieselben Bild-und Symbol Tools wie ein Entwickler, der in *Ziel-C* und *Xcode* arbeitet.

Es gibt mehrere Möglichkeiten, Image-Assets innerhalb einer macOS-Anwendung (früher als Mac OS X bezeichnet) zu verwenden. Wenn Sie ein Bild einfach als Teil der Benutzeroberfläche Ihrer Anwendung anzeigen, indem Sie es einem UI-Steuerelement zuweisen, wie z. b. einer Symbolleiste oder einem Quell Listenelement, können Sie mit xamarin. Mac auf folgende Weise problemlos großartige Grafiken zu ihren macOS-Anwendungen hinzufügen: : 

- **UI-Elemente** : Bilder können als Hintergründe oder als Teil der Anwendung in einer Bildansicht (`NSImageView`) angezeigt werden.
- **Schalt** Flächen-Bilder können in Schaltflächen (`NSButton`) angezeigt werden.
- **Bildzelle** : als Teil eines Tabellen basierten Steuer Elements (`NSTableView` oder `NSOutlineView`) können Bilder in einer Bildzelle (`NSImageCell`) verwendet werden.
- **Symbolleisten Element** -Bilder können einer Symbolleiste (`NSToolbar`) als Bildsymbol leisten Element (`NSToolbarItem`) hinzugefügt werden.
- **Quell Listen Symbol** -als Teil einer Quell Liste (ein speziell formatiertes `NSOutlineView`).
- **App-Symbol** : eine Reihe von Bildern kann in einer `.icns` Gruppe zusammengefasst und als Symbol der Anwendung verwendet werden. Weitere Informationen finden Sie in der Dokumentation des [Anwendungs Symbols](~/mac/deploy-test/app-icon.md) .

Außerdem bietet macOS eine Reihe vordefinierter Images, die in der gesamten Anwendung verwendet werden können.

[![Ein Beispiel für die APP-Laufzeit](image-images/intro01.png "Ein Beispiel für die APP-Laufzeit")](image-images/intro01-large.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit Bildern und Symbolen in einer xamarin. Mac-Anwendung behandelt. Es wird dringend empfohlen, dass Sie zunächst den Artikel [Hello, Mac](~/mac/get-started/hello-mac.md) , insbesondere die [Einführung in Xcode und die Abschnitte zu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und Outlets und [Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) , verwenden, da er wichtige Konzepte und Techniken behandelt, die wir in verwenden werden. Dieser Artikel.

## <a name="adding-images-to-a-xamarinmac-project"></a>Hinzufügen von Bildern zu einem xamarin. Mac-Projekt

Wenn Sie ein Bild für die Verwendung in einer xamarin. Mac-Anwendung hinzufügen, gibt es mehrere Orte und Möglichkeiten, wie der Entwickler die Bilddatei in die Projekt Quelle einschließen kann:

- **Hauptprojekt Struktur [deprecated]** : Bilder können direkt der Projektstruktur hinzugefügt werden. Wenn Sie in der Hauptprojekt Struktur gespeicherte Bilder aus dem Code aufrufen, wird kein Ordner Speicherort angegeben. Beispiel: `NSImage image = NSImage.ImageNamed("tags.png");`. 
- **Ressourcen Ordner [veraltet]** : der Ordner "besondere **Ressourcen** " ist eine Datei, die Teil des Anwendungspakets wird, z. b. Symbol, Startbildschirm oder allgemeine Bilder (oder ein beliebiges anderes Bild oder eine andere Datei, das der Entwickler hinzufügen möchte). Beim Aufrufen von Bildern, die im Ordner " **Resources** " aus dem Code gespeichert sind, wie Bilder, die in der Hauptprojekt Struktur gespeichert sind, wird kein Ordner Speicherort angegeben. Beispiel: `NSImage.ImageNamed("tags.png")`.
- **Benutzerdefinierter Ordner oder Unterordner [veraltet]** : der Entwickler kann der Quell Struktur der Projekte einen benutzerdefinierten Ordner hinzufügen und die Bilder dort speichern. Der Speicherort, an dem die Datei hinzugefügt wird, kann in einen Unterordner eingefügt werden, um die Organisation des Projekts weiter zu unterstützen. Wenn der Entwickler z. b. einen `Card` Ordner zum Projekt und einen Unterordner `Hearts` zu diesem Ordner hinzugefügt hat, speichern Sie ein Image **Jack. png** im Ordner `Hearts`, `NSImage.ImageNamed("Card/Hearts/Jack.png")` das Image zur Laufzeit laden würde.
- **Asset Catalog-Image Sätze [bevorzugt]** : hinzugefügt in OS X El Capitan, **Asset Katalogen-Image Sätze** enthalten alle Versionen oder Darstellungen eines Bilds, die zur Unterstützung verschiedener Geräte und Skalierungsfaktoren für Ihre Anwendung erforderlich sind. Anstatt sich auf den Image Assets-Dateiname ( **@1x** **@2x** ) zu verlassen.

<a name="asset-catalogs" />

### <a name="adding-images-to-an-asset-catalog-image-set"></a>Hinzufügen von Bildern zu einem Asset Catalog-Image Satz

Wie bereits erwähnt, enthalten die **Image Sätze** eines Bestands Katalogs alle Versionen oder Darstellungen eines Bilds, die zur Unterstützung verschiedener Geräte und Skalierungsfaktoren für Ihre Anwendung erforderlich sind. Anstatt sich auf den Image Assets-Dateiname zu verlassen (siehe die Lösungs unabhängigen Bilder und die Bild Nomenklatur oben), verwenden **Image Gruppen** den Asset-Editor, um anzugeben, welches Image zu welchem Gerät und/oder zur Auflösung gehört.

1. Doppelklicken Sie im **Lösungspad**auf die Datei **Assets. xcassets** , um Sie für die Bearbeitung zu öffnen: 

    ![Auswählen der Assets. xcassets](image-images/imageset01.png "Auswählen der Assets. xcassets")
2. Klicken Sie mit der rechten Maustaste auf die **Liste Assets** , und wählen Sie **neue Bildmenge**aus: 

    [![Hinzufügen eines neuen Image Satzes](image-images/imageset02.png "Hinzufügen eines neuen Image Satzes")](image-images/imageset02-large.png#lightbox)
3. Wählen Sie den neuen Bildsatz aus, und der Editor wird angezeigt: 

    [![Auswählen des neuen Image Satzes](image-images/imageset03.png "Auswählen des neuen Image Satzes")](image-images/imageset03-large.png#lightbox)
4. Von hier aus können Sie Bilder für jedes der verschiedenen Geräte und Auflösungen ziehen, die erforderlich sind. 
5. Doppelklicken Sie auf den **Namen** des neuen Image Satzes in der **Liste Assets** , um ihn zu bearbeiten: 

    [![Bearbeiten des Namens der Bildmenge](image-images/imageset04.png "Bearbeiten des Namens der Bildmenge")](image-images/imageset04-large.png#lightbox)
    
Eine spezielle **Vektor** Klasse, die zu **Bild Sätzen** hinzugefügt wurde und es uns ermöglicht, ein _PDF-_ formatiertes Vektorbild in das Casset einzuschließen, einschließlich einzelner Bitmapdateien in den verschiedenen Auflösungen. Mit dieser Methode stellen Sie eine einzelne Vektor Datei für die **@1x** Auflösung bereit (formatiert als Vektor-PDF-Datei), und die **@2x** -und **@3x** Versionen der Datei werden zur Kompilierzeit generiert und in das Paket der Anwendung eingeschlossen. .

[![Die Bild Satz-Editor-Schnittstelle](image-images/imageset05.png "Die Bild Satz-Editor-Schnittstelle")](image-images/imageset05-large.png#lightbox)

Wenn Sie z. b. eine `MonkeyIcon.pdf`-Datei als Vektor für einen Asset-Katalog mit einer Auflösung von 150px x 150px einschließen, werden die folgenden bitmapassets in die endgültige App Bundle eingefügt, als Sie kompiliert wurde:

1. Auflösung von **MonkeyIcon@1x.png** -150px x 150px.
2. Auflösung von **MonkeyIcon@2x.png** -300 px x 300 px.
3. **MonkeyIcon@3x.png** -450px x 450 px Auflösung.

Bei der Verwendung von PDF-Vektorbildern in Asset-Katalogen sollten folgende Aspekte berücksichtigt werden:

- Dabei handelt es sich nicht um vollständige Vektor Unterstützung, da die PDF-Datei zur Kompilierzeit in eine Bitmap gerengt wird und die in der abschließenden Anwendung enthaltenen Bitmaps.
- Die Größe des Abbilds kann nicht angepasst werden, sobald es im Asset-Katalog festgelegt wurde. Wenn Sie versuchen, die Größe des Bilds (entweder im Code oder mithilfe der Klassen für automatisches Layout und Größe) zu ändern, wird das Bild wie jede andere Bitmap verzerrt.

Wenn Sie ein Bild verwenden, das in der Interface Builder von Xcode **festgelegt** ist, können Sie einfach den Namen der Gruppe in der Dropdown Liste im **Attribut Inspektor**auswählen:

![Auswählen eines in Xcode festgelegten Bilds Interface Builder](image-images/imageset06.png "Auswählen eines in Xcode festgelegten Bilds Interface Builder")

<a name="Adding-new-Assets-Collections"/>

### <a name="adding-new-assets-collections"></a>Neue Ressourcen Sammlungen werden hinzugefügt

Beim Arbeiten mit Bildern in Ressourcen Katalogen kann es vorkommen, dass Sie eine neue Sammlung erstellen möchten, anstatt alle Images der **Assets. xcassets** -Auflistung hinzuzufügen. Beispielsweise beim Entwerfen von bedarfsgesteuerten Ressourcen.

So fügen Sie dem Projekt einen neuen Ressourcen Katalog hinzu:

1. Klicken Sie im **Lösungspad** mit der rechten Maustaste auf das Projekt, und wählen Sie  > **neue Datei** **Hinzufügen** ... aus.
2. Wählen Sie **Mac**  > **Asset Catalog**aus, geben Sie einen **Namen** für die Sammlung ein, und klicken Sie auf die Schaltfläche **neu** 

    ![Hinzufügen eines neuen Ressourcen Katalogs](image-images/asset01.png "Hinzufügen eines neuen Ressourcen Katalogs")

Von hier aus können Sie mit der Auflistung genauso arbeiten wie die standardmäßige **Assets. xcassets** -Auflistung, die automatisch im Projekt enthalten ist.

### <a name="adding-images-to-resources"></a>Hinzufügen von Bildern zu Ressourcen

> [!IMPORTANT]
> Diese Methode zum Arbeiten mit Bildern in einer macOS-App wurde von Apple als veraltet markiert. Sie sollten stattdessen [Asset Catalog Image Sets](#asset-catalogs) verwenden, um die Images Ihrer APP zu überführen.

Bevor Sie eine Bilddatei in ihrer xamarin. Mac-Anwendung (entweder im C# Code oder Interface Builder) verwenden können, muss Sie in den **Ressourcen** Ordner des Projekts als **Bündel Ressource**aufgenommen werden. Gehen Sie folgendermaßen vor, um einem Projekt eine Datei hinzuzufügen:

1. Klicken Sie im **Lösungspad** mit der rechten Maustaste auf den Ordner **Ressourcen** , und wählen Sie **Hinzufügen**  > **Dateien hinzufügen...** aus: 

    ![Hinzufügen einer Datei](image-images/add01.png "Hinzufügen einer Datei")
2. Wählen Sie im Dialogfeld **Dateien hinzufügen** die Image Dateien aus, die dem Projekt hinzugefügt werden sollen, wählen Sie `BundleResource` für die **Aktion Build überschreiben** , und klicken Sie auf die Schaltfläche **Öffnen** :

    [![Auswählen der hinzu zufügenden Dateien](image-images/add02.png "Auswählen der hinzu zufügenden Dateien")](image-images/add02-large.png#lightbox)
3. Wenn sich die Dateien nicht bereits im **Ressourcen** Ordner befinden, werden Sie gefragt, ob Sie die Dateien **Kopieren**, **verschieben** oder **Verknüpfen** möchten. Wählen Sie alle aus, die Ihren Anforderungen entsprechen, in der Regel wird **kopiert**:

    ![Auswählen der Aktion hinzufügen](image-images/add04.png "Auswählen der Aktion hinzufügen")
4. Die neuen Dateien werden in das Projekt eingeschlossen und zur Verwendung gelesen: 

    ![Die neuen Bilddateien, die der Lösungspad hinzugefügt wurden.](image-images/add03.png "Die neuen Bilddateien, die der Lösungspad hinzugefügt wurden.")
5. Wiederholen Sie den Vorgang für alle erforderlichen Bilddateien.

Sie können eine beliebige PNG-, JPG-oder PDF-Datei als Quell Image in ihrer xamarin. Mac-Anwendung verwenden. Im nächsten Abschnitt sehen wir uns das Hinzufügen von hochauflösenden Versionen unserer Bilder und Symbole zur Unterstützung von Retina-basierten Macs an.

> [!IMPORTANT]
> Wenn Sie dem Ordner " **Resources** " Bilder hinzufügen, können Sie die **Aktion "Build außer Kraft** setzen" auf **default**festlegen. Die standardmäßige Buildaktion für diesen Ordner ist `BundleResource`.

## <a name="provide-high-resolution-versions-of-all-app-graphics-resources"></a>Bereitstellen von hochauflösenden Versionen aller App-Grafik Ressourcen

Alle Grafik Medienobjekte, die Sie einer xamarin. Mac-Anwendung hinzufügen (Symbole, benutzerdefinierte Steuerelemente, benutzerdefinierte Cursor, benutzerdefinierte Grafiken usw.) müssen neben ihren standardmäßigen Auflösungs Versionen eine hochauflösende Version aufweisen. Dies ist erforderlich, damit Ihre Anwendung am besten aussehen kann, wenn Sie auf einem mit der Retina-Anzeige ausgestatteten Macintosh-Computer ausgeführt wird.

### <a name="adopt-the-2x-naming-convention"></a>@No__t_0 Benennungs Konvention übernehmen

> [!IMPORTANT]
> Diese Methode zum Arbeiten mit Bildern in einer macOS-App wurde von Apple als veraltet markiert. Sie sollten stattdessen [Asset Catalog Image Sets](#asset-catalogs) verwenden, um die Images Ihrer APP zu überführen.

Wenn Sie die standardmäßigen und die hochauflösende Version eines Images erstellen, befolgen Sie diese Benennungs Konvention für das Image-Paar, wenn Sie Sie in Ihr xamarin. Mac-Projekt einschließen:

- **Standard Auflösung**   - **ImageName. filename-Extension** (Beispiel: **Tags. png**)
- @No__t_1 mit **hoher Auflösung**  **ImageName@2x.filename-extension** (Beispiel: **tags@2x.png** )

Wenn Sie zu einem Projekt hinzugefügt werden, werden Sie wie folgt angezeigt:

![Die Bilddateien im Lösungspad](image-images/add03.png "Die Bilddateien im Lösungspad")

Wenn ein Bild einem Benutzeroberflächen Element in zugewiesen wird Interface Builder Sie die Datei einfach im _ImageName_auswählen **.** _Dateiname-Erweiterungs_ Format (Beispiel: **Tags. png**). Das gleiche gilt für die Verwendung eines C# Bilds im Code, wählen Sie die Datei im _ImageName_aus **.** _Dateiname-Erweiterungs_ Format.

Wenn Sie die xamarin. Mac-Anwendung auf einem Mac ausführen, ist dies der _ImageName_ **.** das Bild " _Dateiname-Erweiterungs_ Format" wird für Standard mäßige Auflösungs anzeigen verwendet, das **ImageName@2x.filename-extension** Bild wird automatisch in den Retina-Anzeige Basen-Macs ausgewählt.

## <a name="using-images-in-interface-builder"></a>Verwenden von Bildern in Interface Builder

Alle Image-Ressourcen, die Sie dem **Ressourcen** Ordner in Ihrem xamarin. Mac-Projekt hinzugefügt und die Buildaktion auf **bundleresource** festgelegt haben, werden automatisch in Interface Builder angezeigt und können als Teil eines Benutzeroberflächen Elements (wenn es behandelt wird) ausgewählt werden. Bilder).

Gehen Sie folgendermaßen vor, um ein Bild in Interface Builder zu verwenden:

1. Fügen Sie dem **Ressourcen** Ordner ein Bild mit der **Buildaktion** `BundleResource` hinzu: 

     ![Eine Bildressource in der Lösungspad](image-images/ib00.png "Eine Bildressource in der Lösungspad")
2. Doppelklicken Sie auf die Datei **Main. Storyboard** , um Sie für die Bearbeitung in Interface Builder zu öffnen: 

     [![Bearbeiten des Haupt Storyboards](image-images/ib01.png "Bearbeiten des Haupt Storyboards")](image-images/ib01-large.png#lightbox)
3. Ziehen Sie ein UI-Element, das Bilder annimmt, auf die Entwurfs Oberfläche (z. b. ein **Bildsymbol**leisten Element): 

     ![Bearbeiten eines Toolbar-Elements](image-images/ib02.png "Bearbeiten eines Toolbar-Elements")
4. Wählen Sie das Image aus, das Sie dem Ordner **Ressourcen** in der Dropdown Liste **Image Name** hinzugefügt haben: 

     [![Auswählen eines Bilds für ein Symbolleisten Element](image-images/ib03.png "Auswählen eines Bilds für ein Symbolleisten Element")](image-images/ib03-large.png#lightbox)
5. Das ausgewählte Bild wird auf der Entwurfs Oberfläche angezeigt: 

     ![Das Bild, das im Symbolleisten-Editor angezeigt wird.](image-images/ib04.png "Das Bild, das im Symbolleisten-Editor angezeigt wird.")
6. Speichern Sie die Änderungen, und kehren Sie zu Visual Studio für Mac zurück, um mit Xcode zu synchronisieren.

Die obigen Schritte funktionieren für alle Benutzeroberflächen Elemente, die die Festlegung der Bild Eigenschaft im **Attribut Inspektor**ermöglichen. Auch hier gilt: Wenn Sie eine **@2x** Version der Abbild Datei eingefügt haben, wird Sie automatisch auf auf der Grundlage der Retina-Anzeige basierenden Macs verwendet.

> [!IMPORTANT]
> Wenn das Bild in der Dropdown Liste **Bildname** nicht verfügbar ist, schließen Sie das Storyboard-Projekt in Xcode, und öffnen Sie es in Visual Studio für Mac. Wenn das Image immer noch nicht verfügbar ist, stellen Sie sicher, dass die **Buildaktion** `BundleResource` ist und dass das Image dem **Ressourcen** Ordner hinzugefügt wurde.

## <a name="using-images-in-c-code"></a>Verwenden von Bildern C# im Code

Wenn Sie ein Bild mithilfe C# von Code in ihrer xamarin. Mac-Anwendung in den Arbeitsspeicher laden, wird das Bild in einem `NSImage`-Objekt gespeichert. Wenn die Bilddatei im xamarin. Mac-Anwendungs Bündel (in Ressourcen enthalten) enthalten ist, verwenden Sie den folgenden Code, um das Image zu laden:

```csharp
NSImage image = NSImage.ImageNamed("tags.png");
```

Der obige Code verwendet die statische `ImageNamed("...")`-Methode der `NSImage`-Klasse, um das angegebene Bild aus dem **Ressourcen** Ordner in den Arbeitsspeicher zu laden. wenn das Bild nicht gefunden werden kann, wird `null` zurückgegeben. Wie Bilder, die in Interface Builder zugewiesen sind, werden Sie, wenn Sie eine **@2x** Version ihrer Abbild Datei eingeschlossen haben, automatisch auf auf der Basis von auf der Basis angezeigten Macs verwendet.

Verwenden Sie den folgenden Code, um Bilder außerhalb des Anwendungspakets (aus dem Mac-Dateisystem) zu laden:

```csharp
NSImage image = new NSImage("/Users/KMullins/Documents/photo.jpg")
```

<a name="Working-with-Template-Images"/>

## <a name="working-with-template-images"></a>Arbeiten mit Vorlagen Images

Basierend auf dem Entwurf Ihrer macOS-App kann es vorkommen, dass Sie ein Symbol oder Bild innerhalb der Benutzeroberfläche anpassen müssen, um eine Änderung des Farbschemas zu erreichen (z. b. basierend auf den Benutzereinstellungen).

Um diesen Effekt zu erzielen, ändern Sie den _Rendermodus_ des Image Assets in das **Vorlagen Image**:

[![Festlegen eines Vorlagen Images](image-images/templateimage01.png "Festlegen eines Vorlagen Images")](image-images/templateimage01-large.png#lightbox)

Weisen Sie im Interface Builder von Xcode das imageasset einem UI-Steuerelement zu:

![Auswählen eines Bilds in der Interface Builder von Xcode](image-images/templateimage02.png "Auswählen eines Bilds in der Interface Builder von Xcode")

Oder legen Sie optional die Bildquelle im Code fest:

```csharp
MyIcon.Image = NSImage.ImageNamed ("MessageIcon");
```

Fügen Sie dem Ansichts Controller die folgende öffentliche Funktion hinzu:

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
> Vor allem bei der Einführung des dunklen Modus in macOS, ist es wichtig, die `LockFocus`-API zu vermeiden, wenn Sie benutzerdefinierte `NSImage` Objekte wiederherstellen. Diese Images werden statisch und werden nicht automatisch aktualisiert, um Darstellung oder Änderungen der Anzeige Dichte zu berücksichtigen.
>
> Durch die Verwendung des obigen handlerbasierten Mechanismus findet das erneute Rendering für dynamische Bedingungen automatisch statt, wenn das `NSImage` gehostet wird, z. b. in einem `NSImageView`.

Um zum Schluss ein Vorlagen Image zu erstellen, müssen Sie diese Funktion für das Bild zum Einfärben von Farben abrufen:

```csharp
MyIcon.Image = ImageTintedWithColor (MyIcon.Image, NSColor.Red);
```

<a name="Using_Images_with_Table_Views" />

## <a name="using-images-with-table-views"></a>Verwenden von Bildern mit Tabellen Sichten

Wenn Sie ein Bild als Teil der Zelle in einem `NSTableView` einschließen möchten, müssen Sie ändern, wie die Daten von der `NSTableViewDelegate's` `GetViewForItem`-Methode der Tabellen Sicht zurückgegeben werden, um anstelle der typischen `NSTextField` eine `NSTableCellView` zu verwenden. Beispiel:

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

Hier sind einige interessante Zeilen. Als erstes erstellen wir für Spalten, die ein Image enthalten sollen, eine neue `NSImageView` der erforderlichen Größe und Position. Außerdem erstellen wir eine neue `NSTextField` und platzieren Ihre Standardposition basierend darauf, ob wir ein Image verwenden. :

```csharp
if (tableColumn.Title == "Product") {
    view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
    view.AddSubview (view.ImageView);
    view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
} else {
    view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
}
```

Zweitens müssen wir die neue Bildansicht und das Textfeld in der übergeordneten `NSTableCellView` einschließen:

```csharp
view.AddSubview (view.ImageView);
...

view.AddSubview (view.TextField);
...

```

Schließlich müssen wir dem Textfeld mitteilen, dass es verkleinert und mit der Tabellen Ansichts Zelle vergrößert werden kann:

```csharp
view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
```

Beispielausgabe:

[![Ein Beispiel für die Anzeige eines Bilds in einer APP](image-images/tables01.png "Ein Beispiel für die Anzeige eines Bilds in einer APP")](image-images/tables01-large.png#lightbox)

Weitere Informationen zum Arbeiten mit Tabellen Sichten finden Sie in der Dokumentation der [Tabellen Sichten](~/mac/user-interface/table-view.md) .

<a name="Using_Images_with_Outline_Views" />

## <a name="using-images-with-outline-views"></a>Verwenden von Bildern mit Gliederungs Ansichten

Wenn Sie ein Bild als Teil der Zelle in einem `NSOutlineView` einschließen möchten, müssen Sie ändern, wie die Daten von der `NSTableViewDelegate's` `GetView`-Methode der Gliederungs Ansicht zurückgegeben werden, um anstelle der typischen `NSTextField` einen `NSTableCellView` zu verwenden. Beispiel:

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

Hier sind einige interessante Zeilen. Als erstes erstellen wir für Spalten, die ein Image enthalten sollen, eine neue `NSImageView` der erforderlichen Größe und Position. Außerdem erstellen wir eine neue `NSTextField` und platzieren Ihre Standardposition basierend darauf, ob wir ein Image verwenden. :

```csharp
if (tableColumn.Title == "Product") {
    view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
    view.AddSubview (view.ImageView);
    view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
} else {
    view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
}
```

Zweitens müssen wir die neue Bildansicht und das Textfeld in der übergeordneten `NSTableCellView` einschließen:

```csharp
view.AddSubview (view.ImageView);
...

view.AddSubview (view.TextField);
...

```

Schließlich müssen wir dem Textfeld mitteilen, dass es verkleinert und mit der Tabellen Ansichts Zelle vergrößert werden kann:

```csharp
view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
```

Beispielausgabe:

[![Ein Beispiel für ein Bild, das in einer Gliederungs Ansicht angezeigt wird.](image-images/outline01.png "Ein Beispiel für ein Bild, das in einer Gliederungs Ansicht angezeigt wird.")](image-images/outline01-large.png#lightbox)

Weitere Informationen zum Arbeiten mit Gliederungs Ansichten finden Sie in der Dokumentation zu den Gliederungs [Ansichten](~/mac/user-interface/outline-view.md) .

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde die Arbeit mit Bildern und Symbolen in einer xamarin. Mac-Anwendung ausführlich erläutert. Wir haben die verschiedenen Typen und Verwendungsmöglichkeiten von Bildern gesehen, die Verwendung von Bildern und Symbolen in der Interface Builder von Xcode und das Arbeiten mit Bildern und C# Symbolen im Code.

## <a name="related-links"></a>Verwandte Links

- [MacImages (Beispiel)](https://docs.microsoft.com/samples/xamarin/mac-samples/macimages)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Tabellen Sichten](~/mac/user-interface/table-view.md)
- [Gliederungs Ansichten](~/mac/user-interface/outline-view.md)
- [Benutzeroberflächen Richtlinien für macOS X](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
- [Informationen über hohe Auslösung für OS X](https://developer.apple.com/library/content/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Introduction/Introduction.html)
