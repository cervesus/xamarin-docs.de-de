---
title: Anwendungs Symbole in xamarin. IOS
description: 'In diesem Dokument wird beschrieben, wie Sie mit verschiedenen Anwendungssymbolen in xamarin. IOS arbeiten: das Anwendungssymbol selbst, Spotlight-Symbole, die Einstellungs Symbole und iTunes-Grafiken.'
ms.prod: xamarin
ms.assetid: B7791574-4A0F-4CB6-8C18-36D40B5C91EB
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/22/2017
ms.openlocfilehash: 885f5321c10bcbc5389daf7dd7a97d1f9d572499
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73010375"
---
# <a name="application-icons-in-xamarinios"></a>Anwendungs Symbole in xamarin. IOS

Die folgenden Themen werden im Detail behandelt:

- [Symbole für Anwendungen, Spotlight und Einstellungen](#icon-types) : die verschiedenen Typen von Symbolen, die für eine IOS-App erforderlich sind.
- [Verwalten von Symbolen mit Asset-Katalogen](#managing) : Verwalten von Anwendungssymbolen mithilfe von Asset-Katalogen
- [iTunes-Grafik](#itunes) : Bereitstellen der erforderlichen iTunes-Grafik für die Ad-hoc-Methode der Bereitstellung Ihrer Anwendung.

<a name="icon-types" />

## <a name="application-spotlight-and-settings-icons"></a>Symbole für Anwendung, Spotlight und Einstellungen

Auf dieselbe Weise, wie eine xamarin. IOS-App Image-Assets für UI-Steuerelemente und Dokument Symbole verwenden kann, können Image-Assets zum Bereitstellen von Anwendungssymbolen verwendet werden. Die folgenden Screenshots von einem iPad veranschaulichen die drei Verwendungsmöglichkeiten von Symbolen in ios:

- **Anwendungssymbol** : jede IOS-app muss ein Anwendungssymbol definieren. Dies ist das Symbol, auf das der Benutzer vom IOS-Startbildschirm tippt, um die APP zu starten. Außerdem wird dieses Symbol ggf. von Game Center verwendet. Beispiel: 

    [![](app-icons-images/000.png "Application Icon")](app-icons-images/000-full.png#lightbox)
- **Spotlight-Symbol** : Wenn der Benutzer den Namen einer APP in eine Spotlight-Suche eingibt, wird dieses Symbol angezeigt. Beispiel: 

    [![](app-icons-images/000a.png "Spotlight Icon")](app-icons-images/000a-full.png#lightbox)
- **Symbol "Einstellungen** ": Wenn der Benutzer die app " **Einstellungen** " auf seinem IOS-Gerät eingibt, wird dieses Symbol am Ende der **Einstellungs** Liste für die App angezeigt. Beispiel: 

    [![](app-icons-images/000b.png "Settings Icon")](app-icons-images/000b-full.png#lightbox)

Die folgenden Image assetgrößen und-Lösungen sind erforderlich, um alle für eine xamarin. IOS-App mit IOS 5 bis IOS 9 (oder höher) erforderlichen Symboltypen zu unterstützen:

### <a name="iphone-icon-sizes"></a>iPhone-Symbolgrößen

- **iPhone: IOS 9 & 10 (iPhone 6 & 7 plus)**

    ||3X|
    |---|---|
    |Anwendungssymbol|180 x 180|
    |Gerückt|120x120|
    |Einstellungen|87x87|

- **iPhone: IOS 7 & 8**

    ||1X|2x|
    |---|---|---|
    |Anwendungssymbol|60 x 60<sup>1</sup>|120x120|
    |Gerückt|40 x 40<sup>2</sup>|80X 80|
    |Einstellungen|-|-|

- **iPhone: IOS 5 & 6**

    ||1X|2x|
    |---|---|---|
    |Anwendungssymbol|57x57|114 x 114|
    |Gerückt|29x29|58x58|
    |Einstellungen|29x29<sup>3, 4</sup>|58x58<sup>3, 4</sup>|

### <a name="ipad-icon-sizes"></a>iPad-Symbolgrößen

- **iPad: IOS 9 & 10**

    ||2x (iPad pro)|
    |---|---|
    |Anwendungssymbol|167x167<sup>6</sup>|
    |Gerückt|120 x 120<sup>6</sup>|
    |Einstellungen|58x58<sup>5</sup>|

- **iPad: IOS 7 & 8**

    ||1X|2x|
    |---|---|---|
    |Anwendungssymbol|76x76|152 x 152|
    |Gerückt|40x40|80X 80|
    |Einstellungen|-|-|

- **iPad: IOS 5 & 6**

    ||1X|2x|
    |---|---|---|
    |Anwendungssymbol|72x72|144 x 144|
    |Gerückt|50x50|deren|
    |Einstellungen|29x29<sup>3, 5</sup>|58x58<sup>3, 5</sup>|

 1. Sowohl Visual Studio für Mac als auch Xcode unterstützen nicht mehr das Einrichten von 1 x-Image für IOS 7.
 2. Das Festlegen eines 1X-Images für IOS 7 wird bei der Verwendung von Asset-Katalogen nicht unterstützt.
 3. IOS 7 & 8 verwendet die gleichen Bildgrößen wie IOS 5 & 6.
 4. Verwendet die gleichen Bilder und Größen wie das Spotlight-Symbol.
 5. Verwendet die gleichen Größen Symbole wie das iPhone.
 6. Wird nur mit Ressourcen Katalog-Image Sätzen unterstützt.

 Weitere Informationen zu Symbolen finden Sie in der Dokumentation zu [Symbol-und Bildgrößen](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/IconMatrix.html#//apple_ref/doc/uid/TP40006556-CH27-SW1) von Apple.

<a name="managing" />

## <a name="managing-icons-with-asset-catalogs"></a>Verwalten von Symbolen mit Asset-Katalogen

Bei Symbolen kann der `Assets.xcassets`-Datei im Projekt der APP ein spezieller `AppIcon` Bild Satz hinzugefügt werden. Alle Versionen des Bilds, die zur Unterstützung aller Auflösungen erforderlich sind, sind im _xcasset_ enthalten und zusammengefasst. Mit einem speziellen Editor in Visual Studio für Mac können Entwickler diese Bilder grafisch einschließen und einrichten.

Gehen Sie folgendermaßen vor, um einen Asset-Katalog zu verwenden:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Doppelklicken Sie auf die `Info.plist` Datei im **Projektmappen-Explorer** , um Sie für die Bearbeitung zu öffnen.
2. Scrollen Sie nach unten zum Abschnitt **App-Symbole** .
3. Vergewissern Sie sich, dass in der Dropdown Liste **Quelle** die Option **AppIcon** ausgewählt ist: 

    ![](app-icons-images/migrate01.png "Ensure AppIcon is selected")
4. Doppelklicken Sie im **Projektmappen-Explorer**auf die `Assets.xcassets` Datei, um Sie zur Bearbeitung zu öffnen: 

    ![](app-icons-images/asset01.png "The Assets.xcassets file in the Solution Explorer")
5. Wählen Sie aus der Liste der Assets `AppIcon` aus, um die `Icon Editor`anzuzeigen:

    ![](app-icons-images/asset02.png "The AppIcon editor")
6. Klicken Sie entweder auf den angegebenen Symboltyp, und wählen Sie eine Bilddatei für die erforderliche Art/Größe aus, oder ziehen Sie ein Bild aus einem Ordner, und legen Sie es auf der gewünschten Größe ab.
7. Klicken Sie auf die Schaltfläche **Öffnen** , um das Bild in das Projekt einzuschließen, und legen Sie es im xcasset fest.
8. Wiederholen Sie diesen Schritt für alle benötigten Images.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Doppelklicken Sie im **Projektmappen-Explorer**auf die Datei **Info. plist** :

    ![](app-icons-images/icon01w.png "Select Info.plist")
2. Klicken Sie auf die Registerkarte **visuelle Objekte** und dann unter **App-Symbole**auf die Schaltfläche " **Asset catalog verwenden** ": 

    ![](app-icons-images/icon02w.png "Select the Visual Assets tab")
3. Erweitern Sie auf der **Projektmappen-Explorer**den Ordner **Asset Catalog** : 

    ![](app-icons-images/image009.png "Expand the Asset Catalog folder")
4. Doppelklicken Sie auf die **Medien** Datei, um Sie im Editor zu öffnen: 

    ![](app-icons-images/image010.png "Open the Media file in the editor")
5. Im **Eigenschaften-Explorer** kann der Entwickler die verschiedenen Typen und Größen der erforderlichen Symbole auswählen.
6. Klicken Sie auf den angegebenen Symboltyp, und wählen Sie eine Bilddatei für die erforderliche Art/Größe aus.
7. Klicken Sie auf die Schaltfläche **Öffnen** , um das Bild in das Projekt einzuschließen, und legen Sie es im xcasset fest.
8. Wiederholen Sie diesen Schritt für alle benötigten Images.

-----

Dies ist die bevorzugte Methode zum einschließen und Verwalten von bildassets, die verwendet werden, um Symbole für Anwendungen, Spotlight und Einstellungen für eine APP bereitzustellen.

### <a name="migrating-from-infoplist-to-asset-catalogs"></a>Migrieren von "Info. plist" zu Asset-Katalogen

Für eine vorhandene xamarin. IOS-APP, die die `Info.plist` Datei zum Verwalten der Symbole verwendet, wird dringend empfohlen, dass der Entwickler das `AppIcons` Image-Asset innerhalb des `Assets.xcassets`verwendet.

Führen Sie folgende Schritte aus:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Doppelklicken Sie auf die `Info.plist` Datei im **Projektmappen-Explorer** , um Sie für die Bearbeitung zu öffnen.
2. Scrollen Sie nach unten zum Abschnitt **App-Symbole** .
3. Wählen Sie in der Dropdown Liste **Quelle** die Option **zu Asset-Katalogen migrieren**aus: 

    ![](app-icons-images/migrate02.png "Select Migrate to Asset Catalogs")
4. Alle vorhandenen Symbole, die in der `Info.plist` Datei definiert sind, werden zu einem `AppIcons` Abbild Satz migriert, der `Assets.xcassets`hinzugefügt wird: 

     ![](app-icons-images/migrate03.png "The AppIcons Image Set in the Assets.xcassets")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Doppelklicken Sie auf die `Info.plist` Datei im **Projektmappen-Explorer** , um Sie für die Bearbeitung zu öffnen.
2. Klicken Sie auf den Abschnitt iPhone-Symbole: 

    ![](app-icons-images/image007.png "Rhe iPhone Icons editor")
3. Scrollen Sie nach unten zum Abschnitt **Symbole** .
4. Wählen Sie in der Dropdown Liste **Asset Catalog** den Wert **Asset-Kataloge verwenden**aus.
5. Alle vorhandenen Symbole, die in der `Info.plist`-Datei definiert sind, werden zu einem `Images` Satz migriert, der `Assets.xcassets`hinzugefügt wird.
6. Speichern Sie die Änderungen an der Datei `Info.plist`.

-----

<a name="itunes" />

## <a name="itunes-artwork"></a>iTunes-Grafik

Bei Verwendung der Ad-hoc-Methode der Bereitstellung der APP (entweder für Unternehmensbenutzer oder für Beta-Tests auf echten Geräten) muss der Entwickler auch ein 512 x512-und 1024 x1024-Bild einschließen, das zur Darstellung der app in iTunes verwendet wird.

Um die iTunes-Grafik festzulegen, führen Sie Folgendes aus:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Doppelklicken Sie auf die `Info.plist` Datei im **Projektmappen-Explorer** , um Sie für die Bearbeitung zu öffnen.
2. Scrollen Sie zum Abschnitt **iTunes-Grafik** des Editors: 

    ![](app-icons-images/itunes01.png "Scroll to the iTunes Artwork section of the editor")
3. Klicken Sie für ein fehlendes Bild im Editor auf die Miniaturansicht, wählen Sie im Dialogfeld Datei öffnen die Bilddatei für die gewünschte iTunes-Grafik aus, und klicken Sie auf die Schaltfläche **OK** .
4. Wiederholen Sie diesen Schritt, bis alle benötigten Images für die APP angegeben wurden.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Doppelklicken Sie auf die `Info.plist` Datei im **Projektmappen-Explorer** , um Sie für die Bearbeitung zu öffnen.

2. Klicken Sie auf die Registerkarte **visuelle Assets** , und erweitern Sie die **iTunes-Grafik**: 

    ![](app-icons-images/itunes01w.png "Editing iTunes Artwork in Visual Studio")
3. Klicken Sie für ein fehlendes Bild im Editor auf die Miniaturansicht, wählen Sie im Dialogfeld Datei öffnen die Bilddatei für die gewünschte iTunes-Grafik aus, und klicken Sie auf die Schaltfläche **Öffnen** .
4. Wiederholen Sie diesen Schritt, bis alle benötigten Images für die APP angegeben wurden.

-----

## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit Bildern (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithimages)
- [Hallo iPhone](~/ios/get-started/hello-ios/index.md)
- [Richtlinien für benutzerdefiniertes Symbol und Bild Erstellung](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
