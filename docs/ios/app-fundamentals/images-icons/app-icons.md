---
title: Anwendungssymbole
description: Dieser Artikel behandelt die einschließlich und verwalten ein Standardimage-Medienobjekt in einem Xamarin.iOS-app als ein Symbol "App" verwendet werden soll.
ms.prod: xamarin
ms.assetid: B7791574-4A0F-4CB6-8C18-36D40B5C91EB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/22/2017
ms.openlocfilehash: 3bf9286fe641d836f0877bfe2e17263bf9d7356e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="application-icons"></a>Anwendungssymbole

_Dieser Artikel behandelt die einschließlich und verwalten ein Standardimage-Medienobjekt in einem Xamarin.iOS-app als ein Symbol "App" verwendet werden soll._

Die folgenden Themen werden im Detail behandelt:

* [Anwendung, Spotlight und Einstellungen Symbole](#icon-types) -die verschiedenen Typen von Symbolen, die für ein iOS-app erforderlich sind.
* [Verwalten von Symbolen mit Asset Kataloge](#managing) - Verwaltung von Anwendungssymbolen Asset Kataloge verwenden.
* [iTunes Bildmaterial](#itunes) -erforderlichen iTunes Bildmaterial für die Ad-hoc-Methode der Lieferung Ihrer Anwendungsstatus angeben.

<a name="icon-types" />

## <a name="application-spotlight-and-settings-icons"></a>Anwendung Spotlight und Einstellungen-Symbole

Auf die gleiche Weise, dass eine app Xamarin.iOS Bildanlagen für UI-Steuerelemente und als Dokument Symbole verwenden kann können Bildanlagen verwendet werden, um Anwendungssymbolen bereitzustellen. Die folgenden Screenshots aus einem iPad werden drei Verwendungszwecke von Symbolen in iOS veranschaulicht:

- **Symbol "Anwendung"** -alle iOS-app muss ein Anwendungssymbol definieren. Dies ist das Symbol, das der Benutzer tippen sollen auf der iOS-Startseite auf die app zu starten. Darüber hinaus wird dieses Symbol von Game Center "," ggf. verwendet. Beispiel: 

    [![](app-icons-images/000.png "Symbol "Anwendung"")](app-icons-images/000-full.png#lightbox)
- **Symbol "Spotlight** – Wenn der Benutzer den Namen einer App in einem Spotlight-Suche eingibt dieses Symbol wird angezeigt. Beispiel: 

    [![](app-icons-images/000a.png "Symbol "Spotlight"")](app-icons-images/000a-full.png#lightbox)
- **Symbol "Einstellungen"** – Wenn der Benutzer gibt die **Einstellungen** app auf seinem iOS-Gerät, dieses Symbol wird am Ende angezeigt werden die **Einstellungen** Liste für die app. Beispiel: 

    [![](app-icons-images/000b.png "Symbol "Einstellungen"")](app-icons-images/000b-full.png#lightbox)

Die folgenden Asset Bildgrößen und Lösungen ist erforderlich sein, um unterstützen alle die Symboltypen, die von einer Xamarin.iOS-app für iOS 5 über iOS 9 (oder höher) erforderlich sind:

### <a name="iphone-icon-sizes"></a>iPhone Symbolgrößen

- **iPhone: iOS 9 und 10 (iPhone 6 und 7 Plus)**

    ||3x|
    |---|---|
    |Anwendungssymbol|180 x 180|
    |Spotlight|120x120|
    |Einstellungen|87x87|

- **iPhone: iOS 7 & 8**

    ||1x|2x|
    |---|---|---|
    |Anwendungssymbol|60x60<sup>1</sup>|120x120|
    |Spotlight|40x40<sup>2</sup>|80x80|
    |Einstellungen|-|-|

- **iPhone: iOS 5 & 6**

    ||1x|2x|
    |---|---|---|
    |Anwendungssymbol|57x57|114x114|
    |Spotlight|29x29|58 x 58|
    |Einstellungen|29x29<sup>3, 4</sup>|58 x 58<sup>3, 4</sup>|

### <a name="ipad-icon-sizes"></a>iPad Symbolgrößen

- **iPad: iOS 9 & 10**

    ||2x (iPad Pro)|
    |---|---|
    |Anwendungssymbol|167x167<sup>6</sup>|
    |Spotlight|120x120<sup>6</sup>|
    |Einstellungen|58x58<sup>5</sup>|

- **iPad: iOS 7 & 8**

    ||1x|2x|
    |---|---|---|
    |Anwendungssymbol|76x76|152x152|
    |Spotlight|40x40|80x80|
    |Einstellungen|-|-|

- **iPad: iOS 5 & 6**

    ||1x|2x|
    |---|---|---|
    |Anwendungssymbol|72x72|144x144|
    |Spotlight|50x50|100x100|
    |Einstellungen|29x29<sup>3, 5</sup>|58x58<sup>3, 5</sup>|

 1. Sowohl Visual Studio für Mac und Xcode unterstützt Festlegen von 1 X-Image für iOS 7 nicht mehr.
 2. Festlegen eines 1 X-Images für iOS 7 wird nicht unterstützt, wenn Asset Kataloge verwenden.
 3. iOS 7 und 8 verwenden dieselbe Bildgrößen als iOS 5 und 6 an.
 4. Verwendet die Bilder und die Größe der Spotlight-Symbol.
 5. Verwendet die gleiche Größe Symbole als dem iPhone an.
 6. Nur unterstützt mit Asset-Katalog-Image-Sätze.
 
 Weitere Informationen zu Symbolen finden Sie in der Apple- [Symbol und Bildgrößen](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/IconMatrix.html#//apple_ref/doc/uid/TP40006556-CH27-SW1) Dokumentation.

<a name="managing" />

## <a name="managing-icons-with-asset-catalogs"></a>Verwalten von Symbolen mit Asset-Kataloge

Für Symbole, eine spezielle `AppIcons` Bildersatz hinzugefügt werden kann die `Assets.xcassets` Datei in die app-Projekt. Alle Versionen des Bilds erforderlich, um alle Lösungen unterstützen befinden sich die _Xcasset_ und gruppiert. Ein spezieller-Editor in Visual Studio für Mac kann der Entwickler und diese Abbilder grafisch einrichten.

Wenn eine Asset-Katalog verwenden möchten, führen Sie folgende Schritte aus:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Doppelklicken Sie auf die `Info.plist` in der Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen.
2. Führen Sie einen Bildlauf nach unten, um die **App-Symbole** Abschnitt.
3. Aus der **Quelle** Dropdownliste sicher **AppIcons** ausgewählt ist: 

    ![](app-icons-images/migrate01.png "Stellen Sie sicher, dass AppIcons ausgewählt ist")
4. Aus der **Projektmappen-Explorer**, doppelklicken Sie auf die `Assets.xcassets` Datei zur Bearbeitung zu öffnen: 

    ![](app-icons-images/asset01.png "Die Assets.xcassets-Datei im Projektmappen-Explorer")
5. Wählen Sie `AppIcons` aus der Liste der Elemente zum Anzeigen der `Icon Editor`: 

    ![](app-icons-images/asset02.png "Der Editor AppIcons")
6. Entweder klicken Sie auf das angegebene Symboltyp und wählen Sie eine Abbilddatei für die erforderlichen Typgröße oder ziehen Sie ein Bild aus einem Ordner und legen Sie ihn auf die gewünschte Größe.
7. Klicken Sie auf die **öffnen** Schaltfläche, um das Bild in das Projekt einfügen, und legen Sie ihn in die Xcasset.
8. Wiederholen Sie für alle Abbilder erforderlich sind.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Doppelklicken Sie auf die **"Info.plist"** in der Datei die **Projektmappen-Explorer**:

    ![](app-icons-images/icon01w.png "Wählen Sie die Datei "Info.plist"")
2. Klicken Sie auf die **visuelle Anlagen** Registerkarte, und klicken Sie auf die **verwenden Asset-Katalog** unter Schaltfläche **App-Symbole**: 

    ![](app-icons-images/icon02w.png "Wählen Sie die Registerkarte visuelle Objekte")
4. Aus der **Projektmappen-Explorer**, erweitern Sie die **Asset-Katalog** Ordner: 

    ![](app-icons-images/image009.png "Erweitern Sie den Ordner Asset-Katalog")
5. Doppelklicken Sie auf die **Medien** Datei im Editor zu öffnen: 

    ![](app-icons-images/image010.png "Öffnen Sie die Media-Datei im editor")
6. Klicken Sie unter der **Eigenschaften-Explorer** der Entwickler kann auswählen, die verschiedene Typen und Größen von Symbolen, die erforderlich sind.
7. Klicken Sie auf das angegebene Symboltyp, und wählen Sie eine Bilddatei für die erforderlichen Typgröße.
8. Klicken Sie auf die **öffnen** Schaltfläche, um das Bild in das Projekt einfügen, und legen Sie ihn in die Xcasset.
9. Wiederholen Sie für alle Abbilder erforderlich sind.

-----

Dies ist die bevorzugte Methode der einschließlich und Verwaltung von Bildanlagen, die verwendet wird, um Symbole der Anwendung, Spotlight und Einstellungen für eine Anwendung bereitzustellen.

### <a name="migrating-from-infoplist-to-asset-catalogs"></a>Migrieren von Datei "Info.plist" Asset-Katalog

Für eine vorhandene Xamarin.iOS app mithilfe der `Info.plist` -Datei, um die zugehörigen Symbole zu verwalten, wird dringend vorgeschlagen, dass der Entwickler es wechseln, verwenden Sie die `AppIcons` Imagemedienobjekt innerhalb der `Assets.xcassets`.

Führen Sie folgende Schritte aus:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Doppelklicken Sie auf die `Info.plist` in der Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen.
2. Führen Sie einen Bildlauf nach unten, um die **App-Symbole** Abschnitt.
3. Aus der **Quelle** Dropdownliste **Migrieren zu Asset Kataloge**: 

    ![](app-icons-images/migrate02.png "Wählen Sie migrieren, Asset-Katalog")
4. Alle vorhandenen Symbole im definiert die `Info.plist` Datei migriert wird eine `AppIcons` Bildersatz hinzugefügt `Assets.xcassets`: 

     ![](app-icons-images/migrate03.png "Das Bild AppIcons festgelegt, in der Assets.xcassets")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Doppelklicken Sie auf die `Info.plist` in der Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen.
2. Klicken Sie auf dem iPhone Symbolabschnitt aus: 

    ![](app-icons-images/image007.png "Rhe iPhone Symbole-editor")
3. Führen Sie einen Bildlauf nach unten, um die **Symbole** Abschnitt.
4. Aus der **Asset-Katalog** Dropdownliste **verwenden Asset Kataloge**.
5. Alle vorhandenen Symbole im definiert die `Info.plist` Datei migriert wird eine `Images` Satz hinzugefügt `Assets.xcassets`.
6. Speichern Sie die Änderungen an der Datei `Info.plist`.

-----

<a name="itunes" />

## <a name="itunes-artwork"></a>iTunes-Grafik

Wenn Sie die Ad-hoc-Methode der Lieferung der app (entweder für Benutzer im Unternehmen oder Betatests auf echten Geräten) zu verwenden, muss der Entwickler auch umfassen eine 512 x 512 und ein 1024 x 1024-Image, das verwendet wird, um die app im iTunes darzustellen.

Um die iTunes-Grafik festzulegen, führen Sie Folgendes aus:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Doppelklicken Sie auf die `Info.plist` in der Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen.
2. Führen Sie einen Bildlauf zu der **iTunes Bildmaterial** Abschnitt des Editors: 

    ![](app-icons-images/itunes01.png "Führen Sie einen Bildlauf zum iTunes Bildmaterial Abschnitt des Editors")
3. Für alle Fehlendes Bild klicken Sie auf die Miniaturansicht im Editor, wählen Sie die Bilddatei für das gewünschte iTunes-Bildmaterial, über das Dialogfeld Datei öffnen, und klicken Sie auf die **OK** Schaltfläche.
4. Wiederholen Sie diesen Schritt, bis alle Bilder für die app erforderlich angegeben wurden.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Doppelklicken Sie auf die `Info.plist` in der Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen.

2. Klicken Sie auf die **visuelle Anlagen** Registerkarte, und erweitern Sie die **iTunes Bildmaterial**: 

    ![](app-icons-images/itunes01w.png "ITunes Vorlagen in Visual Studio bearbeiten")
4. Für alle Fehlendes Bild klicken Sie auf die Miniaturansicht im Editor, wählen Sie die Bilddatei für das gewünschte iTunes-Bildmaterial, über das Dialogfeld Datei öffnen, und klicken Sie auf die **öffnen** Schaltfläche.
5. Wiederholen Sie diesen Schritt, bis alle Bilder für die app erforderlich angegeben wurden.

-----

## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit Bildern (Beispiel)](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Hallo iPhone](~/ios/get-started/hello-ios/index.md)
- [Benutzerdefiniertes Symbol und Richtlinien für die Erstellung von Image](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html))
