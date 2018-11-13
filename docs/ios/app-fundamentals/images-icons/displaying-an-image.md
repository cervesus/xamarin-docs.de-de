---
title: Anzeigen eines Bilds in Xamarin.iOS
description: Dieser Artikel behandelt, einschließlich ein Bildobjekt in einer Xamarin.iOS-app und dieses Bild angezeigt, entweder mithilfe von C#-Code oder durch Zuweisen eines Steuerelements in der iOS-Designer.
ms.prod: xamarin
ms.assetid: 60288B12-49E3-4E87-8690-D04A5EC7A664
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/24/2018
ms.openlocfilehash: 325f4e99e70f88ccf642253720f4229142a169ec
ms.sourcegitcommit: 7eed80186e23e6aff3ddbbf7ce5cd1fa20af1365
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2018
ms.locfileid: "51526559"
---
# <a name="displaying-an-image-in-xamarinios"></a>Anzeigen eines Bilds in Xamarin.iOS

_Dieser Artikel behandelt, einschließlich ein Bildobjekt in einer Xamarin.iOS-app und dieses Bild angezeigt, entweder mithilfe von C#-Code oder durch Zuweisen eines Steuerelements in der iOS-Designer._

## <a name="adding-and-organizing-images-in-a-xamarinios-app"></a>Hinzufügen und Organisieren von Bildern in einer Xamarin.iOS-app

Wenn Sie ein Image für die Verwendung in einer Xamarin.iOS-app hinzufügen, wird der Entwickler verwenden eine _Asset-Katalog_ zur Unterstützung jedes iOS-Gerät und der Auflösung, die von einer app erforderlich.

Hinzugefügt in iOS 7 **Bildzusammenstellungen für Asset-Kataloge** enthalten alle Versionen oder Darstellungen eines Bilds, die zum unterstützen verschiedener Geräte und Skalierungsfaktoren für eine app erforderlich sind. Statt die standardremotedatenbank von der Dateiname des Bilds für Ressourcen, **Bildzusammenstellungen** eine JSON-Datei verwenden, um anzugeben, welches Image, welche Geräte und/oder Auflösung angehört. Dies ist die bevorzugte Methode zum Verwalten und unterstützen von Bildern in iOS (von iOS 9 oder höher).

## <a name="adding-images-to-an-asset-catalog-image-set"></a>Legen Sie das Hinzufügen von Bildern zu einem Bild, Asset-Katalog

Wie bereits erwähnt, ein **Bildzusammenstellungen für Asset-Kataloge** enthalten alle Versionen oder Darstellungen eines Bilds, die zum unterstützen verschiedener Geräte und Skalierungsfaktoren für eine app erforderlich sind. Statt die standardremotedatenbank von der Dateiname des Bilds für Ressourcen, **Bildzusammenstellungen** eine JSON-Datei verwenden, um anzugeben, welches Image, welche Geräte und/oder Auflösung angehört.

Zum Erstellen einer neuen Gruppe von Bildern und Hinzufügen von Bildern, gehen Sie folgendermaßen vor:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. In der **Projektmappen-Explorer**, doppelklicken Sie auf die `Assets.xcassets` Datei, die sie für die Bearbeitung zu öffnen:

    ![](displaying-an-image-images/imageset01.png "Die Assets.xcassets im Projektmappen-Explorer")
2. Mit der rechten Maustaste auf die **Assetliste** , und wählen Sie **neue Image Menge**:

    ![](displaying-an-image-images/imageset02.png "Hinzufügen einer neuen Gruppe von Bildern")
3. Wählen Sie die neue Gruppe von Bildern, und der Editor wird angezeigt:

    ![](displaying-an-image-images/imageset03.png "Der Set-Image-editor")
4. Ziehen Sie von hier aus für jede der verschiedenen Geräten und Lösungen, die erforderlichen Bilder auf. 
5. Doppelklicken Sie auf die neue bildzusammenstellung **Namen** in die **Assetliste** zu bearbeiten: ![](displaying-an-image-images/imageset04.png "Bearbeiten des Namens für die neue bildzusammenstellung")

Bei Verwendung einer **Images** im iOS-Designer, wählen Sie einfach der Name der aus der Dropdown-Liste in den Eigenschaften-Editor:

![](displaying-an-image-images/imageset06.png "Wählen Sie ein Image der Name aus der Dropdown-Liste")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Öffnen Sie den Asset-Katalog aus der **Projektmappen-Explorer**, und klicken Sie in der oberen linken Ecke auf die **Plus** Schaltfläche:

    ![](displaying-an-image-images/asset5.png "Klicken Sie auf das Pluszeichen Schaltfläche")

2. Wählen Sie **Bild hinzufügen, legen Sie** und der Set-Image-Editor für die neue Gruppe von Bildern angezeigt. Ziehen Sie von hier aus für jede der verschiedenen Geräten und Lösungen, die erforderlichen Bilder auf. 

    ![](displaying-an-image-images/asset7.png "Legen Sie die Bild-editor")

### <a name="renaming-an-image-set"></a>Umbenennen einer Gruppe von Bildern

Zum Umbenennen einer-Image festzulegen, führen Sie folgende Schritte aus:

1. In der **Projektmappen-Explorer**, doppelklicken Sie auf die **Asset-Katalog** Datei, die sie für die Bearbeitung zu öffnen:

    ![](displaying-an-image-images/rename01.png "Die Asset-Katalog im Projektmappen-Explorer")
2. Wählen Sie die **Images** umbenennen:

    ![](displaying-an-image-images/rename02.png "Wählen Sie das Image festlegen umbenennen")
3. In der **Eigenschaften-Explorer**, scrollen Sie nach unten, und wählen Sie **Namen**(unter der **Verschiedenes** Abschnitt):

    ![](displaying-an-image-images/rename03.png "Wählen Sie die Namen der im Abschnitt \"Sonstiges\"")
4. Geben Sie einen neuen **Namen** für die **Images** und die Änderungen zu speichern.

-----

Bei Verwendung einer **-Images** im Code darauf verweisen anhand des Namens durch Aufrufen der `FromBundle` -Methode der der `UIImage` Klasse. Beispiel:

```csharp
MonkeyImage.Image = UIImage.FromBundle ("PurpleMonkey");
```

> [!IMPORTANT]
> Wenn die Images zugewiesen, da ein Image nicht ordnungsgemäß angezeigt werden, stellen Sie sicher, dass der richtige Dateiname verwendet wird die `FromBundle` Methode (die **-Images** und nicht das übergeordnete Element **Asset-Katalog** Namen). Für PNG-Dateien die `.png` Erweiterung kann ausgelassen werden. Für andere Bildformate ist die Erweiterung erforderlich (z. b. `PurpleMonkey.jpg`) angezeigt wird.

### <a name="using-vector-images-in-asset-catalogs"></a>Verwenden von Vektorgrafiken in ressourcenkataloge

Ab iOS 8, spezielle **Vektor** Klasse hinzugefügt wurden **Bildzusammenstellungen** , ermöglicht den Entwickler, enthalten eine **PDF** Vektorbild in der stattdessen einschließlich Kassette formatiert einzelne Bitmap-Dateien in unterschiedlichen Auflösungen. Mit dieser Methode geben Sie eine einzelnen Vektor-Datei für die `@1x` Auflösung (formatiert als Vektor PDF-Datei) und die `@2x` und `@3x` Versionen der Datei zum Zeitpunkt der Kompilierung generiert und in der Anwendung Bündel enthalten.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![](displaying-an-image-images/imageset05.png "Vektorbilder im Editor Ressourcenkataloge")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](displaying-an-image-images/asset8.png "Vektorbilder im Editor Ressourcenkataloge")

-----

Wenn der Entwickler enthält z. B. eine `MonkeyIcon.pdf` -Datei als der Vektor, der ein Asset-Katalog mit einer Auflösung von 150 x 150px, die folgenden Bitmap Objekte im endgültigen app-Paket enthalten sein, würde bei der Kompilierung:

- `MonkeyIcon@1x.png` -150px x 150px Auflösung.
- `MonkeyIcon@2x.png` -Auflösung von 300 px x 300px.
- `MonkeyIcon@3x.png` -450px x 450px Auflösung.

Folgendes sollte berücksichtigt werden bei Verwendung von PDF-Vektorgrafiken in Ressourcenkataloge:

- Dies wird nicht vollständige Vektor als PDF-Datei gerasterte zu einer Bitmap zur Kompilierzeit und die Bitmaps, die in der fertigen Anwendung geliefert werden.
- Die Größe des Bilds kann nicht angepasst werden, sobald er in den Asset-Katalog festgelegt wurde. Wenn der Entwickler, zum Ändern der Bildgröße (entweder im Code oder versucht durch automatisches Layout mit Größenklassen) wird genau wie jede andere Bitmap das Bild verzerrt werden.
- Ressourcenkataloge sind nur kompatibel mit iOS 7 und höher, wenn eine app unterstützt iOS 6 oder niedriger ist, es keine Ressourcenkataloge können.

## <a name="working-with-template-images"></a>Arbeiten mit vorlagenimages

Basierend auf den Entwurf einer iOS-app, es gibt möglicherweise Zeiten, wenn der Entwickler ein Symbol oder Bild innerhalb der Benutzeroberfläche eine Änderung am Farbschema (z. B. basierend auf benutzereinstellungen) entsprechend anpassen muss.

Um ganz einfach diesen Effekt zu erzielen, wechseln die _Rendermodus_ die Image-Ressource für **Vorlagenimage**:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![](displaying-an-image-images/templateimage01.png "Legen Sie den Modus zum Rendern auf Vorlagenimage")](displaying-an-image-images/templateimage01.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](displaying-an-image-images/templateimage01vs.png "Legen Sie den Modus zum Rendern Vorlage")](displaying-an-image-images/templateimage01vs.png#lightbox)

-----

Aus dem iOS-Designer, ein UI-Steuerelement-Standardimage-Medienobjekt zuweisen und dann legen Sie die **Farbton** zum farbigen Anzeigen von der Abbildung:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![](displaying-an-image-images/templateimage03.png "Legen Sie den Farbton zum farbigen Anzeigen von das Bild")](displaying-an-image-images/templateimage03.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](displaying-an-image-images/templateimage03vs.png "Legen Sie den Farbton zum farbigen Anzeigen von das Bild")](displaying-an-image-images/templateimage03vs.png#lightbox)

-----

Optional können das Standardimage-Medienobjekt und den Farbton direkt im Code festgelegt werden:

```csharp
MyIcon.Image = UIImage.FromBundle ("MessageIcon");
MyIcon.TintColor = UIColor.Red;
```

Um ein Vorlagenimage aus Code vollständig zu verwenden, führen Sie folgende Schritte aus:

```csharp
if (MyIcon.Image != null) {
    var mutableImage = MyIcon.Image.ImageWithRenderingMode (UIImageRenderingMode.AlwaysTemplate);
    MyIcon.Image = mutableImage;
    MyIcon.TintColor = UIColor.Red;
}
```

Da die `RenderMode` Eigenschaft eine `UIImage` ist schreibgeschützt, verwenden Sie die `ImageWithRenderingMode` Methode, um eine neue Instanz des Bilds die gewünschte Einstellung für die Render-Modus zu erstellen.

Es gibt drei möglicherweise die Einstellungen für `UIImage.RenderMode` über die `UIImageRenderingMode` Enumeration:

* `AlwaysOriginal` -Erzwingt, dass das Bild, das als die ursprüngliche Quelldatei Bild ohne Änderungen gerendert werden.
* `AlwaysTemplate` -Erzwingt, dass das Bild als ein Vorlagenimage gerendert werden soll, indem die farbliche Kennzeichnung von Pixel mit dem angegebenen `Tint` Farbe.
* `Automatic` – Entweder rendert das Bild, wie eine Vorlage oder die ursprüngliche basiert auf der die Umgebung, die sie in verwendet wird. Angenommen, das Image verwendet wird, in eine `UIToolBar`, `UINavigationBar`, `UITabBar` oder `UISegmentControl` wird es als Vorlage behandelt.

## <a name="adding-new-assets-collections"></a>Hinzufügen von neuen Ressourcen-Sammlungen

Beim Arbeiten mit Images im Ressourcen-Kataloge gibt es möglicherweise vorkommen, wenn werden eine neue Sammlung erforderlich ist, anstatt alle von der app-Images, die `Assets.xcassets` Auflistung. Beispielsweise beim Entwerfen von On-Demand-Ressourcen.

So fügen Sie einen neuen Katalog von Ressourcen zum Projekt hinzu:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Mit der rechten Maustaste auf die **Projektname** in die **Projektmappen-Explorer** , und wählen Sie **hinzufügen** > **neue Datei...**
2. Wählen Sie **iOS** > **Asset-Katalog**, geben Sie einen **Namen** für die Sammlung und klicken Sie auf die **neu** Schaltfläche:

    ![](displaying-an-image-images/asset01.png "Erstellen einen neuen Ressourcenkatalog")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Im Projektmappen-Explorer mit der Maustaste auf **Ressourcenkataloge** Ordner, und wählen **hinzufügen > neue Asset-Katalog**.
2. Geben sie einen Namen ein, und klicken Sie auf **hinzufügen**:

    ![](displaying-an-image-images/asset1.png "Erstellen einen neuen Ressourcenkatalog")

-----

Von hier aus die Auflistung umgangen werden mit in die gleiche Weise wie Standard `Assets.xcassets` Auflistung, die automatisch im Projekt enthalten ist.

## <a name="using-images-with-controls"></a>Verwenden von Bildern mit Steuerelementen

Zusätzlich zur Verwendung von Bildern um eine app zu unterstützen, verwendet auch iOS Bilder mit app-Steuerelementtypen, z. B. Registerkartenleisten, Symbolleisten, Navigationsleisten, Tabellen und Schaltflächen. Eine einfache Möglichkeit, stellen ein Bild auf einem Steuerelement angezeigt wird, Zuweisen einer `UIImage` Instanz des Steuerelements `Image` Eigenschaft.

### <a name="frombundle"></a>FromBundle

Die `FromBundle` Methodenaufruf ist ein synchrone (blockierende)-Aufruf, die eine Reihe von Image laden und verwalten Funktionen integriert, z. B. das Zwischenspeichern, Support und die automatische Behandlung von Bilddateien für verschiedene Auflösungen.  

Das folgende Beispiel zeigt, wie Sie das Abbild des Festlegen einer `UITabBarItem` auf eine `UITabBar`:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage");
```

Vorausgesetzt, dass `MyImage` ist der Name des ein Bildobjekt oben ein Asset-Katalog hinzugefügt. Bei der Arbeit Asset-Katalog-Images Geben Sie einfach den Namen des legen das Image in die `FromBundle` -Methode für **PNG** Images formatiert:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage");
```

Alle anderen Abbildformat enthalten Sie die Erweiterung mit dem Namen. Zum Beispiel:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage.jpg");
```

Weitere Informationen über Symbole und Bilder, finden Sie unter der Apple-Dokumentation auf [benutzerdefinierte Symbol und Richtlinien für die Erstellung von Images](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html).

## <a name="displaying-an-image-in-a-storyboards"></a>Anzeigen eines Bilds in einen storyboards

Sobald ein Image zu einer Xamarin.iOS-Projekt mit einer Asset-Katalogs hinzugefügt wurde, kann es sein, einfach angezeigt werden, auf ein Storyboard mit einem `UIImageView` im iOS-Designer. Wenn beispielsweise die folgenden-Standardimage-Medienobjekt hinzugefügt wurde:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![](displaying-an-image-images/display01.png "Ein Beispiel-Standardimage-Medienobjekt wurde hinzugefügt.")

Führen Sie Folgendes ein, um es für ein Storyboard anzuzeigen:

1. Doppelklicken Sie auf die `Main.storyboard` Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung in der iOS-Designer zu öffnen.
2. Wählen Sie eine **Image View** aus der **Toolbox**:

     ![](displaying-an-image-images/display02.png "Wählen Sie eine Image-Ansicht aus der Toolbox")
3. Ziehen Sie die Image-Ansicht auf die Entwurfsoberfläche und die Position und Größe nach Bedarf:

    ![](displaying-an-image-images/display03.png "Auf der Entwurfsoberfläche eine neue Image-Ansicht")
4. In der **Widget** Teil der **Eigenschaften-Explorer** wählen Sie den gewünschten **Image** Ressource angezeigt werden:

    ![](displaying-an-image-images/display04.png "Wählen Sie das gewünschte Image-Objekt, die angezeigt werden")
5. In der **Ansicht** Abschnitt der **Modus** steuern, wie das Bild kann angepasst, wenn der **Image View** geändert wird.
6. Mit der **Image View** ausgewählt haben, klicken Sie darauf, um hinzuzufügen **Einschränkungen**:

    ![](displaying-an-image-images/display05.png "Hinzufügen von Einschränkungen")
7. Ziehen Sie das "T" strukturiert Handle an jeder der **Image View** auf die entsprechende Seite des Bildschirms, um das Bild an den Seiten "anheften". Auf diese Weise die **Image View** wird verkleinert und wächst die Größe der Bildschirm geändert wird.
8. Speichern Sie die Änderungen auf das Storyboard an.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](displaying-an-image-images/display01vs.png "Ein Beispiel-Standardimage-Medienobjekt wurde hinzugefügt.")

Führen Sie Folgendes ein, um es für ein Storyboard anzuzeigen:

1. Doppelklicken Sie auf die `Main.storyboard` Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung in der iOS-Designer zu öffnen.
2. Wählen Sie eine **Image View** aus der **Toolbox**:

     ![](displaying-an-image-images/display02vs.png "Wählen Sie eine Image-Ansicht aus der Toolbox")
3. Ziehen Sie die Image-Ansicht auf die Entwurfsoberfläche und die Position und Größe nach Bedarf:

    ![](displaying-an-image-images/display03vs.png "Auf der Entwurfsoberfläche eine neue Image-Ansicht")
4. In der **Widget** Teil der **Eigenschaften-Explorer** wählen Sie den gewünschten **Image** Ressource angezeigt werden:

    ![](displaying-an-image-images/display04vs.png "Wählen Sie das gewünschte Image-Objekt, die angezeigt werden")
5. In der **Ansicht** Abschnitt der **Modus** steuern, wie das Bild kann angepasst, wenn der **Image View** geändert wird.
6. Mit der **Image View** ausgewählt haben, klicken Sie darauf, um hinzuzufügen **Einschränkungen**:

    ![](displaying-an-image-images/display05vs.png "Hinzufügen von Einschränkungen")
7. Ziehen Sie das "T" strukturiert Handle an jeder der **Image View** auf die entsprechende Seite des Bildschirms, um das Bild an den Seiten "anheften". Auf diese Weise die **Image View** wird verkleinert und wächst die Größe der Bildschirm geändert wird.
8. Speichern Sie die Änderungen auf das Storyboard an.

-----

## <a name="displaying-an-image-in-code"></a>Anzeigen eines Bilds im code

Genau wie das Anzeigen eines Bilds in einem Storyboard, sobald ein Xamarin.iOS-Projekt mit einer Asset-Kataloge, ein Bild hinzugefügt wurde können sie ganz einfach angezeigt werden mithilfe von C#-Code.

Betrachten Sie das folgende Beispiel:

```csharp
// Create an image view that will fill the
// parent image view and set the image from
// an Image Asset

var imageView = new UIImageView (View.Frame);
imageView.Image = UIImage.FromBundle ("Kemah");

// Add the Image View to the parent view
View.AddSubview (imageView);
```

Dieser Code erstellt ein neues `UIImageView` und weist ihm eine anfängliche Größe und Position. Anschließend das Bild aus einem Image-Medienobjekt, das dem Projekt hinzugefügt lädt, und fügt die `UIImageView` zum übergeordneten Element `UIView` angezeigt.

## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit Bildern (Beispiel)](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Hallo iPhone](~/ios/get-started/hello-ios/index.md)
- [Imagegröße und Auflösung (Apple)](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/image-size-and-resolution/)
