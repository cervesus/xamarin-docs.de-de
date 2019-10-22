---
title: Anzeigen von Bildern mit xamarin. IOS
description: In diesem Dokument wird beschrieben, wie Bilder in xamarin. IOS angezeigt werden. Es wird das Hinzufügen von Bildern zu einer App entweder Programm gesteuert oder über den IOS-Designer behandelt.
ms.prod: xamarin
ms.assetid: 67CA8DB6-769D-42BB-A137-3AF933789FE1
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 07/13/2018
ms.openlocfilehash: 255f308078c892605b9ce20b17fd737c5582eaed
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "70768987"
---
# <a name="displaying-images-with-xamarinios"></a>Anzeigen von Bildern mit xamarin. IOS

Zum Hinzufügen von Bildern zu Ihrer APP sind zwei Schritte erforderlich: zuerst fügen Sie die Bilder dem Projekt hinzu. Fügen Sie anschließend Steuerelemente und Code hinzu, um Sie auf einem Bildschirm anzuzeigen. Ausführliche Informationen zur Bildverarbeitung in xamarin. IOS finden Sie im Artikel [Arbeiten mit Images](~/ios/app-fundamentals/images-icons/index.md) .

## <a name="adding-images-to-your-app"></a>Hinzufügen von Bildern zu Ihrer APP

Bilder können einem beliebigen Ordner in Ihrer Visual Studio für Mac Projekt Mappe hinzugefügt werden. wenn die **Buildaktion** auf " **Content** " festgelegt ist, wird die Datei in Ihre APP eingeschlossen und kann angezeigt werden.

Visual Studio für Mac unterstützt auch ein spezielles Verzeichnis namens " **Resources** ", das auch Bilddateien enthalten kann. Für Dateien im Ordner "Resources" sollte die **Buildaktion** auf **bundleresource**festgelegt sein.

Dieser Screenshot zeigt die buildaktions Optionen **, die angezeigt** werden, wenn mit der rechten Maustaste auf eine Datei geklickt wird:

 [![](image-images/image30a.png "Build Action menu")](image-images/image30a.png#lightbox)

Visual Studio für Mac wählt in der Regel die richtige **Buildaktion** automatisch aus, aber Sie sollten diese Einstellungen beachten, insbesondere, wenn Sie Dateien in Ihrem Projekt verschieben.

### <a name="adding-an-image-file"></a>Hinzufügen einer Bilddatei

Wenn Sie dem Projekt eine Bilddatei hinzufügen möchten, klicken Sie zunächst mit der rechten Maustaste auf das Projekt, und wählen Sie **Dateien hinzufügen** aus.

 [![](image-images/image31a.png "Add Files... menu")](image-images/image31a.png#lightbox)

Wählen Sie das Bild (oder die Bilder) aus, das Sie in das Standarddatei Dialogfeld einschließen möchten. Die Standardbuildaktion für Images lautet **bundleresource** – überschreiben Sie diesen Wert nur, wenn Sie einen bestimmten Grund haben.

 [![](image-images/image32a.png "Add Files dialog")](image-images/image32a.png#lightbox)

Das Bild wird dem Projekt hinzugefügt und kann im Code geladen und angezeigt werden. Dieser Screenshot zeigt ein Bild, das einem IOS-Anwendungsprojekt hinzugefügt wurde:

 [![](image-images/image33a.png "Image in project")](image-images/image33a.png#lightbox)

### <a name="what-is-the-resources-directory"></a>Was ist das Ressourcenverzeichnis?

Dateien, die im **Ressourcen** Verzeichnis abgelegt werden, werden von regulären Dateien anders behandelt – der Inhalt des Ordners " **Resources** " wird in den Stamm der Anwendung kopiert, und in Ihrem Code kann darauf verwiesen werden. Dies kann aus vielen Gründen nützlich sein:

- Speichern der in den Eigenschaften der Anwendung konfigurierten Images, z. b. der standardmäßigen Start Abbilder und Anwendungs Symbole.
- Speichern von anderen Bildern und Dateien getrennt vom Code, sodass Sie einfacher zu verwalten sind (Unterverzeichnisse bleiben erhalten, wenn der Inhalt des Ressourcen Verzeichnisses kopiert wird).

Das **Ressourcen** Verzeichnis ist besonders nützlich in einem Bibliotheksprojekt, da der Code davon ausgehen kann, dass diese Bilder in den Stamm der konsumierenden Anwendung kopiert werden, sodass freigegebene Codebibliotheken, die Bild-, Sound-, Video-, XML-oder andere Dateien.

Das **Ressourcen** Verzeichnis muss so benannt werden, und für alle Dateien sollte die Buildaktion auf **bundleresource**festgelegt sein.

## <a name="displaying-the-image"></a>Anzeigen des Bilds

Verwenden Sie im IOS-Designer eine **Bildansicht** , um ein Bild oder eine animierte Reihe von Bildern anzuzeigen. Das **Bild Ansichts** Symbol aus der Toolbox wird unten angezeigt:

 [![](image-images/image35a.png "ImageView in Toolbox")](image-images/image35.png#lightbox)

Ziehen Sie die **Bildansicht** aus der **Toolbox** auf den Ansichts Controller. In der Dropdown Liste unter **Bildansicht > Abbildung** wird eine Liste aller verfügbaren Bilddateien in Ihrem Projekt bereitgestellt. Wählen Sie diese Option aus, um Sie der Bildansicht hinzuzufügen.

 [![](image-images/image36a.png "ImageView in Toolbox")](image-images/image36.png#lightbox)

### <a name="displaying-the-image-programmatically"></a>Programm gesteuertes Anzeigen des Bilds

Da sich **SF Monkey. jpg** im Stammverzeichnis des **Ressourcen** Verzeichnisses befindet, steht es zur Laufzeit im Stammverzeichnis des Anwendungspakets zur Verfügung. Verwenden Sie den folgenden Code, um dieses Bild in einem Bildansicht-Steuerelement anzuzeigen:

```csharp
imageview1.Image = UIImage.FromBundle("SF Monkey.png");
```

Wenn Sie das Bild in **/Resources/pics/SF Monkey. jpg**eingefügt haben, enthält der Code den Ordner " **Fotos** " im Pfad:

```csharp
imageview1.Image = UIImage.FromBundle("Pics/SF Monkey.png");
```

Ressourcen Datei Verweise müssen niemals den **Ressourcen** Ordner enthalten.

## <a name="related-links"></a>Verwandte Links

- [Steuerelemente (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/controls)
