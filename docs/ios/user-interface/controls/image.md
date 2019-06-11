---
title: Anzeigen von Bildern mit Xamarin.iOS
description: Dieses Dokument beschreibt die Vorgehensweise beim Anzeigen von Bildern in Xamarin.iOS. Hinzufügen von Bildern zu einer app wird hierin, entweder programmgesteuert oder über das iOS-Designer.
ms.prod: xamarin
ms.assetid: 67CA8DB6-769D-42BB-A137-3AF933789FE1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/13/2018
ms.openlocfilehash: 25c5595fac2ea2aa7a87d6640cc6b5c399ab5e5e
ms.sourcegitcommit: 2eb8961dd7e2a3e06183923adab6e73ecb38a17f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2019
ms.locfileid: "66827807"
---
# <a name="displaying-images-with-xamarinios"></a>Anzeigen von Bildern mit Xamarin.iOS

Hinzufügen von Bildern zu Ihrer app sind zwei Schritte erforderlich: Fügen Sie zunächst die Images auf Ihr Projekt, Anschließend fügen Sie Steuerelemente und Code auf einem Bildschirm anzeigen. Finden Sie in der [arbeiten mit Bildern](~/ios/app-fundamentals/images-icons/index.md) detaillierte Abdeckung der Bildbehandlung in Xamarin.iOS-Artikel.

## <a name="adding-images-to-your-app"></a>Hinzufügen von Bildern zu Ihrer app

Bilder können in einen beliebigen Ordner in Ihrer Visual Studio für Mac-Lösung hinzugefügt werden und, wenn die **Buildvorgang** nastaven NA hodnotu **Content** und klicken Sie dann die Datei in Ihrer app enthalten sein wird und angezeigt werden kann.

Visual Studio für Mac unterstützt auch ein spezielles Verzeichnis namens **Ressourcen** kann auch Bilddateien enthalten. Dateien im Ordner "Ressourcen" müssen die **Buildvorgang** festgelegt **BundleResource**.

Dieser Screenshot zeigt die **Buildvorgang** Optionen werden angezeigt, wenn eine Datei geklickt wird:

 [![](image-images/image30a.png "Erstellen Sie im Menü Aktion")](image-images/image30a.png#lightbox)

Visual Studio für Mac wird in der Regel wählen Sie den richtigen **Buildvorgang** automatisch Sie sollten jedoch beachten Sie diese Einstellungen, insbesondere dann, wenn Sie Dateien in Ihrem Projekt verschieben.

### <a name="adding-an-image-file"></a>Bilddatei hinzufügen

Um Ihr Projekt eine Bilddatei hinzugefügt haben, zuerst mit der rechten Maustaste in des Projekts, und wählen Sie **Dateien hinzufügen...**

 [![](image-images/image31a.png "Hinzufügen von Dateien im Menü")](image-images/image31a.png#lightbox)

Wählen Sie das Bild (oder Bilder) in das Dialogfeld für die standard-Datei eingeschlossen werden sollen. Der Standardwert Buildvorgang für Bilder gelten **BundleResource** – diesen Wert nicht überschreiben, es sei denn, Sie einen bestimmten Grund haben.

 [![](image-images/image32a.png "Dateien, Dialogfeld \"hinzufügen\"")](image-images/image32a.png#lightbox)

Das Bild wird hinzugefügt werden, zu Ihrem Projekt und verfügbar sind, geladen und im Code angezeigt werden. Dieser Screenshot zeigt ein Bild, ein iOS-Anwendungsprojekt hinzugefügt:

 [![](image-images/image33a.png "Bild in-Projekt")](image-images/image33a.png#lightbox)

### <a name="what-is-the-resources-directory"></a>Was ist das Verzeichnis "Resources"?

Dateien aus der **Ressourcen** Verzeichnis werden anders behandelt als reguläre Dateien – den Inhalt der **Ressourcen** Ordner werden in das Stammverzeichnis der Anwendung kopiert und dort in verwiesen werden kann Ihren Code. Dies kann aus vielen Gründen nützlich sein:

-  Speichern die Images, die in den Eigenschaften der Anwendung, z. B. den Start Standardbilder und Anwendungssymbole konfiguriert.
-  Speichern die anderen Bilder und Dateien aus dem Code getrennt, daher sind einfacher zu verwalten (Unterverzeichnisse werden beibehalten, wenn die Inhalte des Ressourcen-Verzeichnisses kopiert werden).


Die **Ressourcen** Directory ist ein Bibliotheksprojekt, besonders nützlich, da der Code annehmen kann, dass es sich bei dieser Images kopiert werden sollen, in das Stammverzeichnis der verarbeitenden Anwendung leichter schreiben, die freigegebene Codebibliotheken, die erfordern Bild, sound, Video, XML oder andere Dateien.

Die **Ressourcen** Verzeichnis muss also den Namen, und alle Dateien sollte die Buildaktion festgelegt **BundleResource**.

## <a name="displaying-the-image"></a>Das Bild anzeigen

Verwenden Sie in der iOS-Designer, ein **Image View** um ein Bild oder animiertes Reihe von Bildern anzuzeigen. Die **Image View** Symbol aus der Toolbox wird unten gezeigt:

 [![](image-images/image35a.png "ImageView in Toolbox")](image-images/image35.png#lightbox)

Ziehen Sie die **Image View** aus der **Toolbox** auf den View-Controller. Klicken Sie unter **Image View > Image** die Dropdown Liste wird eine Liste aller verfügbaren Images-Dateien in Ihrem Projekt bereitzustellen. Wählen Sie diese um es die Image-Ansicht hinzuzufügen.

 [![](image-images/image36a.png "ImageView in Toolbox")](image-images/image36.png#lightbox)

### <a name="displaying-the-image-programmatically"></a>Das Bild anzeigen programmgesteuert

Da **SF Monkey.jpg** befindet sich im Stammverzeichnis der **Ressourcen** Directory, sie werden zur Laufzeit in das anwendungsbündel Stamm verfügbar. Um dieses Image in ein Image-Steuerelement anzuzeigen, verwenden Sie den folgenden Code:

```csharp
imageview1.Image = UIImage.FromBundle("SF Monkey.png");
```

Wenn wir das Bild in platziert hat **Ressourcen/Pics/SF Monkey.jpg**, und klicken Sie dann den Code einfügen, würde die **Pics** Ordner im Pfad:

```csharp
imageview1.Image = UIImage.FromBundle("Pics/SF Monkey.png");
```

Ressourcendatei verweist nicht erforderlich, auf die **Ressourcen** Ordner.

## <a name="related-links"></a>Verwandte Links

- [Steuerelemente (Beispiel)](https://developer.xamarin.com/samples/monotouch/Controls/)
