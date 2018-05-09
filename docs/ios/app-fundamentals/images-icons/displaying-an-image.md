---
title: Anzeigen eines Bilds
description: Dieser Artikel umfasst z. B. ein Standardimage-Medienobjekt in einem Xamarin.iOS-app und sich das Bild anzeigen, mithilfe von C#-Code oder indem Sie ein Steuerelement in der iOS-Designer zuweisen.
ms.prod: xamarin
ms.assetid: 60288B12-49E3-4E87-8690-D04A5EC7A664
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/24/2018
ms.openlocfilehash: f1f733fa91be7bf76e19896e78809d18494891d3
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="displaying-an-image"></a>Anzeigen eines Bilds

_Dieser Artikel umfasst z. B. ein Standardimage-Medienobjekt in einem Xamarin.iOS-app und sich das Bild anzeigen, mithilfe von C#-Code oder indem Sie ein Steuerelement in der iOS-Designer zuweisen._

In diesem Artikel werden die folgenden Themen detailliert behandelt:

- [Hinzufügen und Organisieren von Bildern in einer App Xamarin.iOS](#adding-assets) – Bildanlagen behandelt und wie diese eingefügt werden können, organisiert und verwaltet, die innerhalb eines Projekts Xamarin.iOS.
- [Hinzufügen von Bildern zu Asset Kataloge](#asset-catalogs) -Verwalten von Images mit Asset Kataloge.
    - [Vektorgrafiken in Asset-Katalogen mit](#Using-Vector-Images-in-Asset-Catalogs) -Bereitstellung aller die Bildgrößen durch einen einzelnen Vektor.
- [Arbeiten mit Vorlagenimages](#Working-with-Template-Images) -durch Festlegen einer Imagemedienobjekt ein Vorlagenimage sein, der Entwickler kann problemlos zur farblichen Kennzeichnung, um Änderungen an einer app UI-Design übereinstimmen, durch Festlegen des Bilds `Tint` Eigenschaft.
- [Verwenden von Bildern mit Steuerelementen](#controls) – behandelt die Verwendung von Bildanlagen enthalten, die in einem Xamarin.iOS-Projekt mit UI-Steuerelemente, z. B. `UIButton` und `UIImageView` und zum Arbeiten mit Bildern in c# verwenden das `UIImage` des Objekts [FromBundle](#frombundle) Methode.
- [Anzeigen eines Bilds in einen Storyboards](#Displaying-an-Image-in-a-Storyboards) -enthält ein Beispiel für das Anzeigen eines Bilds mithilfe eines Storyboards.
- [Anzeigen eines Bilds in Code](#Displaying-an-Image-in-Code) -Anzeigen eines Bilds mit C#-Code wird veranschaulicht.

<a name="adding-assets" />

## <a name="adding-and-organizing-images-in-a-xamarinios-app"></a>Hinzufügen und Organisieren von Bildern in einem Xamarin.iOS-App

Wenn Sie ein Bild für die Verwendung in einer app Xamarin.iOS hinzufügen, wird der Entwickler verwenden ein _Asset-Katalog_ zur Unterstützung jedes iOS-Gerät und die Auflösung von einer app erforderlich.

Hinzugefügt in iOS 7 **Asset Kataloge Image legt** enthalten alle Versionen oder Darstellungen eines Bilds, die zur Unterstützung von verschiedenen Geräten und Skalierungsfaktoren für eine app erforderlich sind. Anstatt auf den Dateinamen des Bilds Bestand (finden Sie unter [unabhängige Bilder Auflösung und Image-Nomenklatur](~/ios/app-fundamentals/images-icons/displaying-an-image.md)), **Image legt** eine Json-Datei verwenden, um anzugeben, welches Abbild, welche Geräte und/oder Auflösung angehört . Dies ist die bevorzugte Methode zum Verwalten und Images in iOS (von iOS 9 oder höher) unterstützt.

<a name="asset-catalogs" />

## <a name="adding-images-to-an-asset-catalog-image-set"></a>Hinzufügen von Bildern zu einem Bild, Asset-Katalog festgelegt.

Wie bereits erwähnt, ein **Asset Kataloge Image legt** enthalten alle Versionen oder Darstellungen eines Bilds, die zur Unterstützung von verschiedenen Geräten und Skalierungsfaktoren für eine app erforderlich sind. Anstatt auf den Namen der Bilddatei Bestand **Image legt** eine Json-Datei verwenden, um anzugeben, welches Abbild, welche Geräte und/oder Auflösung angehört.

Um einen neuen Satz von Image erstellen, und fügen Sie Bilder hinzu, führen Sie folgende Schritte aus:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. In der **Projektmappen-Explorer**, doppelklicken Sie auf die `Assets.xcassets` Datei zur Bearbeitung zu öffnen:

    ![](displaying-an-image-images/imageset01.png "Die Assets.xcassets im Projektmappen-Explorer")
2. Mit der rechten Maustaste auf die **Inventarliste** , und wählen Sie **neue Bildersatz**:

    ![](displaying-an-image-images/imageset02.png "Hinzufügen eines neuen Image-Satzes")
3. Wählen Sie die neue Image und der Editor wird angezeigt:

    ![](displaying-an-image-images/imageset03.png "Legen Sie die Bild-editor")
4. Von hier aus ziehen in Bildern für jeden der verschiedenen Geräten und und Lösungen, die erforderlich sind. (Hinweis:, die in den Lösungen, die im angegebenen passen diese Lösungen die [Bildgrößen und Dateinamen](~/ios/app-fundamentals/images-icons/displaying-an-image.md) Dokument.)
5. Doppelklicken Sie auf die neue Bildersatz **Namen** in der **Inventarliste** zu bearbeiten: ![ ] (displaying-an-image-images/imageset04.png "Name für die das neue Image Gruppe bearbeiten")

Bei Verwendung einer **Bildersatz** in der iOS-Designer, wählen Sie einfach den Satz Namen aus der Dropdownliste aus den Eigenschaften-Editor:

![](displaying-an-image-images/imageset06.png "Wählen Sie eine Bildersatz Namen aus der Dropdown-Liste")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Öffnen Sie das Asset-Katalog aus der **Projektmappen-Explorer**, und klicken Sie in der oberen linken Ecke auf die **Plus** Schaltfläche:

    ![](displaying-an-image-images/asset5.png "Klicken Sie auf das Pluszeichen Schaltfläche")

2. Wählen Sie **Bild hinzufügen, legen Sie** und der Bild-Editor für den neuen Bildersatz angezeigt. Von hier aus ziehen in Bildern für jeden der verschiedenen Geräten und und Lösungen, die erforderlich sind. (Hinweis:, die in den Lösungen, die im angegebenen passen diese Lösungen die [Bildgrößen und Dateinamen](~/ios/app-fundamentals/images-icons/displaying-an-image.md) Dokument):

    ![](displaying-an-image-images/asset7.png "Legen Sie die Bild-editor")

### <a name="renaming-an-image-set"></a>Einen Satz umbenennen

Um ein Bild festlegen umzubenennen, führen Sie folgende Schritte aus:

1. In der **Projektmappen-Explorer**, doppelklicken Sie auf die **Asset-Katalog** Datei zur Bearbeitung zu öffnen:

    ![](displaying-an-image-images/rename01.png "Asset-Katalog im Projektmappen-Explorer")
2. Wählen Sie die **Bildersatz** umbenennen:

    ![](displaying-an-image-images/rename02.png "Wählen Sie die Image festgelegt ist, Umbenennen")
3. In der **Eigenschaften-Explorer**, führen Sie einen Bildlauf nach unten, und wählen Sie **Namen**(unter der **Verschiedenes** Abschnitt):

    ![](displaying-an-image-images/rename03.png "Wählen Sie im Abschnitt Verschiedenes Namen")
4. Geben Sie einen neuen **Namen** für die **Bildersatz** und die Änderungen zu speichern.

-----

Bei Verwendung einer **Bildersatz** im Code darauf verweisen anhand des Namens durch Aufrufen der `FromBundle` Methode der `UIImage` Klasse. Beispiel:

```csharp
MonkeyImage.Image = UIImage.FromBundle ("PurpleMonkey");
```

> [!IMPORTANT]
> Wenn zugewiesen, da ein Bild Bilder nicht richtig angezeigt werden, stellen Sie sicher, dass der richtige Dateiname verwendet wird die `FromBundle` Methode (die **Bildersatz** und nicht das übergeordnete Element **Asset-Katalog** Namen). PNG-Bilder die `.png` Erweiterung kann ausgelassen werden. Für andere Bildformate ist die Erweiterung erforderlich (z. b. `PurpleMonkey.jpg`) angezeigt wird.

<a name="Using-Vector-Images-in-Asset-Catalogs" />

### <a name="using-vector-images-in-asset-catalogs"></a>Verwenden von Vektorgrafiken in Asset-Kataloge

Ab iOS 8, spezielle **Vektor** Klasse hinzugefügt wurden **Image legt** , mit dessen Hilfe Entwickler enthalten eine **PDF** Vektor-Image in der Kassette stattdessen einschließlich formatiert einzelne Bitmap-Dateien in unterschiedlichen Auflösungen. Mit dieser Methode geben Sie eine einzelnen Vektor-Datei für die `@1x` Auflösung (formatiert als Vektor PDF-Datei) und die `@2x` und `@3x` Versionen der Datei zum Zeitpunkt der Kompilierung generiert und in der Anwendung-Paket enthalten.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![](displaying-an-image-images/imageset05.png "Vektorgrafiken im Editor Asset-Kataloge")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](displaying-an-image-images/asset8.png "Vektorgrafiken im Editor Asset-Kataloge")

-----

Wenn der Entwickler enthält beispielsweise eine `MonkeyIcon.pdf` Datei wie der Vektor, der eine Asset-Katalog mit einer Auflösung von 150px x 150px, Bitmap für die folgenden Objekte im endgültigen app-Paket hinzugefügt werden, würde bei der Kompilierung:

- `MonkeyIcon@1x.png` -150px x 150px Auflösung.
- `MonkeyIcon@2x.png` -300px x 300px Auflösung.
- `MonkeyIcon@3x.png` -450px x 450px Auflösung.

Folgendes sollte berücksichtigt werden bei Verwendung von PDF-Vektorgrafiken in Asset Kataloge:

- Dies ist nicht voll Vektor-Unterstützung, PDF-Datei in eine Bitmap gerasterte zur Kompilierzeit und die Bitmaps, die in der endgültigen Anwendung ausgeliefert werden.
- Die Größe des Bilds kann nicht angepasst werden, sobald es in den Asset-Katalog festgelegt wurde. Wenn der Entwickler, zum Ändern der Bildgröße (entweder im Code oder versucht mithilfe von Automatisches Layout und Klassen Größe) werden genau wie alle anderen Bitmap das Bild verzerrt werden.
- Asset-Kataloge sind nur kompatibel mit iOS 7 und höher, wenn eine app müssen zur Unterstützung der iOS 6 oder niedriger ist, es keine Asset Kataloge können.

<a name="Working-with-Template-Images" />

## <a name="working-with-template-images"></a>Arbeiten mit Vorlagenimages

Basierend auf den Entwurf einer iOS-App, es gibt möglicherweise Zeiten, wenn der Entwickler ein Symbol oder ein Bild in der Benutzeroberfläche eine Änderung im Farbschema (z. B. basierend auf den benutzereinstellungen) entsprechend anpassen muss.

Um problemlos diesen Effekt zu erzielen, wechseln die _Rendermodus_ des Medienobjekts Bild zu **Vorlagenimage**:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![](displaying-an-image-images/templateimage01.png "Legen Sie auf dem Vorlagenimage Render-Modus")](displaying-an-image-images/templateimage01.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](displaying-an-image-images/templateimage01vs.png "Legen Sie auf der Vorlage Render-Modus")](displaying-an-image-images/templateimage01vs.png#lightbox)

-----

Im IOS-Designer, weisen Sie die Imagemedienobjekt an ein Benutzeroberflächensteuerelement, dann Festlegen der **Farbton** auf das Bild zur farblichen Kennzeichnung der:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![](displaying-an-image-images/templateimage03.png "Legen Sie den Farbton, um das Bild zur farblichen Kennzeichnung")](displaying-an-image-images/templateimage03.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](displaying-an-image-images/templateimage03vs.png "Legen Sie den Farbton, um das Bild zur farblichen Kennzeichnung")](displaying-an-image-images/templateimage03vs.png#lightbox)

-----

Optional können das Standardimage-Medienobjekt und Farbton direkt im Code festgelegt werden:

```csharp
MyIcon.Image = UIImage.FromBundle ("MessageIcon");
MyIcon.TintColor = UIColor.Red;
```

Um ein Vorlagenimage vollständig im Code zu verwenden, führen Sie folgende Schritte aus:

```csharp
if (MyIcon.Image != null) {
    var mutableImage = MyIcon.Image.ImageWithRenderingMode (UIImageRenderingMode.AlwaysTemplate);
    MyIcon.Image = mutableImage;
    MyIcon.TintColor = UIColor.Red;
}
```

Da die `RenderMode` Eigenschaft eine `UIImage` ist schreibgeschützt, mit der `ImageWithRenderingMode` Methode, um eine neue Instanz des Bilds die gewünschte Einstellung der Render-Modus zu erstellen.

Es gibt drei möglicherweise die Einstellungen für `UIImage.RenderMode` über die `UIImageRenderingMode` Enum:

* `AlwaysOriginal` -Erzwingt, dass das Bild, das als die ursprüngliche Quelldatei des Abbilds unverändert gerendert werden.
* `AlwaysTemplate` -Erzwingt, dass das Bild, das als ein Vorlagenimage gerendert werden, durch die farbliche Kennzeichnung der Pixel mit dem angegebenen `Tint` Farbe.
* `Automatic` -Entweder rendert das Bild, wie eine Vorlage oder die ursprüngliche basiert auf der die Umgebung, die in der Sie verwendet wird. Beispielsweise das Bild verwendet wird, in eine `UIToolBar`, `UINavigationBar`, `UITabBar` oder `UISegmentControl` wird Sie als Vorlage behandelt werden.

<a name="Adding-new-Assets-Collections" />

## <a name="adding-new-assets-collections"></a>Hinzufügen von neuen Bestand Sammlungen

Beim Arbeiten mit Bildern im Bestand Kataloge möglicherweise beim werden einer neuen Auflistung erforderlich ist, anstatt alle von der app-Bilder für die `Assets.xcassets` Auflistung. Beispielsweise, wenn On-Demand-Ressourcen zu entwerfen.

So fügen Sie einen neuen Katalog von Ressourcen zum Projekt hinzu:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Mit der rechten Maustaste auf die **Projektname** in der **Projektmappen-Explorer** , und wählen Sie **hinzufügen** > **neue Datei...**
2. Wählen Sie **iOS** > **Asset-Katalog**, geben Sie einen **Namen** für die Sammlung klicken Sie auf die **neu** Schaltfläche:

    ![](displaying-an-image-images/asset01.png "Erstellen einen neuen Asset-Katalog")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Projektmappen-Explorer mit der Maustaste auf **Asset Kataloge** Ordner, und wählen **hinzufügen > neuen Asset-Katalog**.
2. Geben sie einen Namen ein, und klicken Sie auf **hinzufügen**:

    ![](displaying-an-image-images/asset1.png "Erstellen einen neuen Asset-Katalog")

-----


Von hier aus die Auflistung umgangen werden kann mit in die gleiche Weise wie Standard `Assets.xcassets` Auflistung, die im Projekt automatisch einbezogen.

<a name="controls" />

## <a name="using-images-with-controls"></a>Verwenden von Bildern mit Steuerelementen

Zusätzlich zur Verwendung von Bildern um eine app zu unterstützen, verwendet iOS auch Bilder mit app-Steuerelementtypen, z. B. Registerkarte Balken, Symbolleisten, Navigationsleisten, Tabellen und Schaltflächen. Eine einfache Möglichkeit, stellen ein Bild auf einem Steuerelement angezeigt wird, weisen Sie einem `UIImage` Instanz des Steuerelements `Image` Eigenschaft.

<a name="frombundle" />

### <a name="frombundle"></a>FromBundle

Die `FromBundle` Methodenaufruf ist eine synchrone (blockierende) Aufruf, die eine Reihe von Image laden und verwalten Funktionen integriert, z. B. caching, Unterstützung und die automatische Handhabung von Abbilddateien für verschiedene Auflösungen verfügt.  

Das folgende Beispiel zeigt, wie Sie das Abbild des Festlegen einer `UITabBarItem` auf eine `UITabBar`:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage");
```

Vorausgesetzt, dass `MyImage` ist der Name des ein Standardimage-Medienobjekt eine Asset-Katalog, die oben genannten hinzugefügt. Beim Asset-Katalog-Images zu arbeiten, geben Sie einfach den Namen des Bildersatz in der `FromBundle` Methode zum **PNG** Bilder formatiert:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage");
```

Schließen Sie für andere Bildformat die Erweiterung mit dem Namen. Zum Beispiel:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage.jpg");
```

Weitere Informationen zu Symbolen und Bilder, finden Sie in der Apple-Dokumentation auf [Symbol "Custom" und Richtlinien für die Erstellung von Image](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html).

<a name="Displaying-an-Image-in-a-Storyboards" />

## <a name="displaying-an-image-in-a-storyboards"></a>Anzeigen eines Bilds in einen Storyboards

Nachdem ein Bild zu einem Xamarin.iOS-Projekt mit einer Anlage Kataloge hinzugefügt wurde, können sie problemlos werden angezeigt, die auf ein Storyboard mit einem `UIImageView` in der iOS-Designer. Wenn beispielsweise die folgenden Imagemedienobjekt hinzugefügt wurde:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![](displaying-an-image-images/display01.png "Ein Beispiel-Imagemedienobjekt wurde hinzugefügt.")

Führen Sie Folgendes ein, um es für ein Storyboard anzuzeigen:

1. Doppelklicken Sie auf die `Main.storyboard` in der Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung in der iOS-Designer zu öffnen.
2. Wählen Sie eine **Image Ansicht** aus der **Toolbox**:

     ![](displaying-an-image-images/display02.png "Wählen Sie eine Image-Ansicht aus der Toolbox")
3. Ziehen Sie die Image-Sicht auf die Entwurfsoberfläche und die Position und Größe es nach Bedarf:

    ![](displaying-an-image-images/display03.png "Auf der Entwurfsoberfläche eine neue Image-Ansicht")
4. In der **Widget** Teil der **Property Explorer** wählen Sie den gewünschten **Image** Asset angezeigt werden:

    ![](displaying-an-image-images/display04.png "Wählen Sie die gewünschten imagemedienobjekt angezeigt werden")
5. In der **Ansicht** Abschnitt der **Modus** zu steuern, wie das Bild angepasst, wenn die **Bild anzeigen** angepasst wird.
6. Mit der **Image Ansicht** ausgewählt haben, klicken Sie darauf, erneut aus, um hinzufügen **Einschränkungen**:

    ![](displaying-an-image-images/display05.png "Hinzufügen von Einschränkungen")
7. Ziehen Sie das "T" geformten Handle an allen Rändern des der **Image Ansicht** auf die entsprechende Seite des Bildschirms auf das Bild an den Seiten "anheften". Auf diese Weise die **Image Ansicht** wird verkleinert und vergrößert werden, während der Bildschirm angepasst wird.
8. Speichern Sie die Änderungen auf das Storyboard.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](displaying-an-image-images/display01vs.png "Ein Beispiel-Imagemedienobjekt wurde hinzugefügt.")

Führen Sie Folgendes ein, um es für ein Storyboard anzuzeigen:

1. Doppelklicken Sie auf die `Main.storyboard` in der Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung in der iOS-Designer zu öffnen.
2. Wählen Sie eine **Image Ansicht** aus der **Toolbox**:

     ![](displaying-an-image-images/display02vs.png "Wählen Sie eine Image-Ansicht aus der Toolbox")
3. Ziehen Sie die Image-Sicht auf die Entwurfsoberfläche und die Position und Größe es nach Bedarf:

    ![](displaying-an-image-images/display03vs.png "Auf der Entwurfsoberfläche eine neue Image-Ansicht")
4. In der **Widget** Teil der **Property Explorer** wählen Sie den gewünschten **Image** Asset angezeigt werden:

    ![](displaying-an-image-images/display04vs.png "Wählen Sie die gewünschten imagemedienobjekt angezeigt werden")
5. In der **Ansicht** Abschnitt der **Modus** zu steuern, wie das Bild angepasst, wenn die **Bild anzeigen** angepasst wird.
6. Mit der **Image Ansicht** ausgewählt haben, klicken Sie darauf, erneut aus, um hinzufügen **Einschränkungen**:

    ![](displaying-an-image-images/display05vs.png "Hinzufügen von Einschränkungen")
7. Ziehen Sie das "T" geformten Handle an allen Rändern des der **Image Ansicht** auf die entsprechende Seite des Bildschirms auf das Bild an den Seiten "anheften". Auf diese Weise die **Image Ansicht** wird verkleinert und vergrößert werden, während der Bildschirm angepasst wird.
8. Speichern Sie die Änderungen auf das Storyboard.

-----


<a name="Displaying-an-Image-in-Code" />

## <a name="displaying-an-image-in-code"></a>Anzeigen eines Bilds in Code

Genau wie das Anzeigen eines Bilds in einem Storyboard, sobald ein Bild zu einem Xamarin.iOS-Projekt mit einer Anlage Kataloge hinzugefügt wurde können sie einfach angezeigt werden mithilfe von C#-Code.

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

Dieser Code erstellt ein neues `UIImageView` und weist ihm eine anfängliche Größe und Position. Es lädt das Bild aus einem Image-Medienobjekt, das dem Projekt hinzugefügt, und fügt die `UIImageView` an der übergeordneten Tabelle `UIView` angezeigt.

## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit Bildern (Beispiel)](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Hallo iPhone](~/ios/get-started/hello-ios/index.md)
- [Benutzerdefiniertes Symbol und Richtlinien für die Erstellung von Images](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
