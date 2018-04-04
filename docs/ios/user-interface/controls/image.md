---
title: Anzeigen von Bildern
ms.prod: xamarin
ms.assetid: 67CA8DB6-769D-42BB-A137-3AF933789FE1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 406cfe813cbb58111769203f3b6c3fb0c2edad3c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="displaying-images"></a>Anzeigen von Bildern

Hinzufügen von Bildern auf Ihre app sind zwei Schritte erforderlich: Fügen Sie zunächst die Bilder dem Projekt; Anschließend fügen Sie Steuerelemente und Code, um sie auf einem Bildschirm anzuzeigen. Finden Sie in der [arbeiten mit Bildern](~/ios/app-fundamentals/images-icons/index.md) Artikel ausführlichere Abdeckung des Bilds in Xamarin.iOS behandeln.

## <a name="adding-images-to-your-app"></a>Hinzufügen von Bildern zu Ihrer App

Bilder in einen beliebigen Ordner in Ihrem Visual Studio für Mac-Lösung hinzugefügt werden können und, wenn die **Buildvorgang** auf festgelegt ist **Content** und dann die Datei mit Ihrer app eingeschlossen und angezeigt werden können.

Visual Studio für Mac unterstützt auch ein besonderes Verzeichnis mit dem Namen der Ressourcen, die auch Bilddateien enthalten kann. Dateien im Ordner "Ressourcen" müssen die **Buildvorgang** festgelegt **BundleResource**.

Diese bildschirmabbildung zeigt die **Buildvorgang** Optionen werden angezeigt, wenn eine Datei geklickt wird:

 [![](image-images/image30a.png "Erstellen Sie im Menü Aktion")](image-images/image30a.png#lightbox)

Visual Studio für Mac werden in der Regel, wählen Sie den richtigen **Buildvorgang** automatisch, aber Sie sollten diese Einstellungen beachtet werden, insbesondere dann, wenn Sie Dateien in Ihrem Projekt verschieben.

### <a name="adding-an-image-file"></a>Hinzufügen einer Bilddatei

Um eine Bilddatei dem Projekt hinzuzufügen, zunächst mit der rechten Maustaste des Projekts und wählen Sie **Dateien hinzufügen...**

 [![](image-images/image31a.png "Hinzufügen von Dateien im Menü")](image-images/image31a.png#lightbox)

Wählen Sie das Bild (oder Bilder) in das Standarddialogfeld eingeschlossen werden sollen. Die Standardeinstellung Buildvorgang Bilder werden **BundleResource** – dieser Wert nicht überschrieben werden, sofern Sie keinen bestimmten Grund haben.

 [![](image-images/image32a.png "Dateien-Dialogfeld "hinzufügen"")](image-images/image32a.png#lightbox)

Das Bild wird hinzugefügt werden, auf das Projekt und geladen und im Code angezeigt werden kann. Diese bildschirmabbildung zeigt ein Bild, ein Projekt für iOS-Anwendung hinzugefügt:

 [![](image-images/image33a.png "Bild in-Projekt")](image-images/image33a.png#lightbox)

### <a name="what-is-the-resources-directory"></a>Was ist die Ressourcen-Verzeichnis?

Dateien, die im Ressourcenverzeichnis platziert werden anders behandelt als reguläre Dateien – den Inhalt des Ordners Ressourcen werden in das Stammverzeichnis der Anwendung kopiert und von dort in Ihrem Code verwiesen werden können. Dies kann aus vielen Gründen nützlich sein:

-  Speichern die Bilder, die in der Anwendung-Eigenschaften, z. B. die Standard-Startbildern und Anwendungssymbolen konfiguriert.
-  Speichern die anderen Bilder und Dateien separat aus dem Code, sie sind also einfacher zu verwalten (Unterverzeichnisse werden beibehalten, wenn die Ressourcen der Verzeichnisinhalt kopiert werden).


Ressourcenverzeichnis ist besonders nützlich, ein Klassenbibliotheksprojekt, da der Code verarbeiten kann, dass diese Bilder in das Stammverzeichnis der Anwendung, erleichtert es, gemeinsam genutzte Codebibliotheken zu schreiben, die erfordern, Image, Audio-, Video-, XML- oder anderen kopiert werden Dateien.



Ressourcenverzeichnis muss daher mit dem Namen, und alle Dateien sollte die Buildaktion festgelegt **BundleResource**

## <a name="displaying-the-image"></a>Das Bild anzuzeigen

Um ein Image mithilfe des Designers anzuzeigen, ein Bild anzeigen als Container verwendet werden sollte und ein einzelnes Bild oder eine Animation des Images anzeigen. Die **Image Ansicht** Symbol aus der Toolbox wird unten gezeigt:

 [![](image-images/image35a.png "ImageView in Toolbox")](image-images/image35.png#lightbox)

Ziehen Sie die **Image Ansicht** aus der **Toobox** auf die View-Controller. Klicken Sie anschließend unter ** Anzeigen > Image ** die Dropdown-Liste stellt eine Liste aller verfügbaren Images-Dateien im Projekt bereit. Wählen Sie eins der Image-Ansicht hinzuzufügen.

 [![](image-images/image36a.png "ImageView in Toolbox")](image-images/image36.png#lightbox)

### <a name="displaying-the-image-programmatically"></a>Das Bild anzeigen programmgesteuert

Da blocks.jpg sich im Stammverzeichnis des Ressourcenverzeichnis befindet wird zur Laufzeit in das Anwendungspaket Stamm verfügbar sein. Zum Anzeigen dieses Bilds in einem ImageView Steuerelement verwenden Sie folgenden Code:

```csharp
imageview1.Image = UIImage.FromBundle ("SF Monkey.png");
```

Wenn wir das Bild im platziert hatten `/Resources/Pics/blocks.jpg` und der Code den Pics-Ordner im Pfad enthalten:

```csharp
imageview1.Image = UIImage.FromBundle ("Pics/SF Monkey.png");
```

Ressourcendatei verweist müssen niemals auf die `Resources` Ordner.


## <a name="related-links"></a>Verwandte Links

- [Steuerelemente (Beispiel)](https://developer.xamarin.com/samples/Controls/)
