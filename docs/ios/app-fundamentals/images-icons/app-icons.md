---
title: Anwendungssymbole in Xamarin.iOS
description: 'Dieses Dokument beschreibt das Arbeiten mit verschiedenen Anwendungssymbole in Xamarin.iOS: das Symbol der Anwendung selbst, Spotlight-Symbole, die Einstellungen für Symbole und iTunes-Grafik.'
ms.prod: xamarin
ms.assetid: B7791574-4A0F-4CB6-8C18-36D40B5C91EB
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/22/2017
ms.openlocfilehash: cf2da9f7fe8d90c52517118cc3b4aa0e046d1f69
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2019
ms.locfileid: "67864553"
---
# <a name="application-icons-in-xamarinios"></a>Anwendungssymbole in Xamarin.iOS

Die folgenden Themen werden im Detail behandelt:

* [Anwendungen, Spotlight und Einstellungen Symbole](#icon-types) -die verschiedenen Arten von Symbolen, die für eine iOS-app erforderlich sind.
* [Verwalten von Symbolen mit Ressourcenkataloge](#managing) – Verwalten von Anwendungssymbolen Ressourcenkataloge verwenden.
* [iTunes-Grafik](#itunes) -angeben der erforderlichen iTunes-Grafik für die Ad-hoc-Methode der Bereitstellung Ihrer Anwendung.

<a name="icon-types" />

## <a name="application-spotlight-and-settings-icons"></a>Anwendungen, Spotlight und Einstellungen für Symbole

Auf die gleiche Weise, dass eine Xamarin.iOS-app Bildanlagen für UI-Steuerelemente sowie Dokumentsymbole verwenden kann können Bildanlagen verwendet werden, um Anwendungssymbole bereitzustellen. Die folgenden Screenshots von einem iPad zeigt die drei Verwendungen der Symbole im iOS:

- **Anwendungssymbol** – alle iOS-app muss ein Anwendungssymbol definieren. Dies ist das Symbol, das der Benutzer tippen, wird auf der Startseite für iOS zum Starten der app. Darüber hinaus wird dieses Symbol von Game Center, ggf. verwendet. Beispiel: 

    [![](app-icons-images/000.png "Anwendungssymbol")](app-icons-images/000-full.png#lightbox)
- **Spotlight-Symbol** : Wenn der Benutzer den Namen einer App in eine Spotlight-Suche, gibt dieses Symbol wird angezeigt. Beispiel: 

    [![](app-icons-images/000a.png "Spotlight-Symbol")](app-icons-images/000a-full.png#lightbox)
- **Symbol "Einstellungen"** : Wenn der Benutzer gibt der **Einstellungen** app auf ihrem iOS-Gerät, dieses Symbol wird angezeigt, am Ende der **Einstellungen** Liste für die app. Beispiel: 

    [![](app-icons-images/000b.png "Symbol \"Einstellungen\"")](app-icons-images/000b-full.png#lightbox)

Die folgenden Asset Bildgrößen und Lösungen sind erforderlich zur Unterstützung aller die Symboltypen, die von einer Xamarin.iOS-app für iOS 5, über den iOS 9 (oder höher) erforderlich:

### <a name="iphone-icon-sizes"></a>iPhone Symbolgrößen

- **iPhone: iOS 9 und 10 (iPhone 6 und 7 Plus)**

    ||3x|
    |---|---|
    |Anwendungssymbol|180x180|
    |Spotlight|120x120|
    |Einstellungen|87x87|

- **iPhone: iOS 7 & 8**

    ||1 x|2x|
    |---|---|---|
    |Anwendungssymbol|60x60<sup>1</sup>|120x120|
    |Spotlight|40x40<sup>2</sup>|80x80|
    |Einstellungen|-|-|

- **iPhone: iOS 5 & 6**

    ||1 x|2x|
    |---|---|---|
    |Anwendungssymbol|57x57|114x114|
    |Spotlight|29x29|58x58|
    |Einstellungen|29x29<sup>3, 4</sup>|58x58<sup>3, 4</sup>|

### <a name="ipad-icon-sizes"></a>iPad Symbolgrößen

- **iPad: iOS 9 & 10**

    ||2x (iPad Pro)|
    |---|---|
    |Anwendungssymbol|167x167<sup>6</sup>|
    |Spotlight|120x120<sup>6</sup>|
    |Einstellungen|58x58<sup>5</sup>|

- **iPad: iOS 7 & 8**

    ||1 x|2x|
    |---|---|---|
    |Anwendungssymbol|76x76|152x152|
    |Spotlight|40x40|80x80|
    |Einstellungen|-|-|

- **iPad: iOS 5 & 6**

    ||1 x|2x|
    |---|---|---|
    |Anwendungssymbol|72x72|144x144|
    |Spotlight|50x50|100x100|
    |Einstellungen|29x29<sup>3, 5</sup>|58x58<sup>3, 5</sup>|

 1. Sowohl Visual Studio für Mac und Xcode unterstützt nicht mehr 1 X-Abbild für iOS 7 festlegen.
 2. Eine 1 X-Abbild für iOS 7 festlegen wird nicht unterstützt, wenn Ressourcenkataloge verwenden.
 3. iOS 7 und 8 verwenden die gleichen Bildgrößen als iOS 5 und 6.
 4. Verwendet die gleichen Images und die Größe als die Spotlight-Symbol an.
 5. Verwendet die gleichen Symbole für die Größe wie das iPhone.
 6. Nur unterstützt mit Bildzusammenstellungen für Asset-Katalog.
 
 Weitere Informationen zu Symbolen finden Sie unter Apple [Symbol und Bildgrößen](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/IconMatrix.html#//apple_ref/doc/uid/TP40006556-CH27-SW1) Dokumentation.

<a name="managing" />

## <a name="managing-icons-with-asset-catalogs"></a>Verwalten von Symbolen mit Ressourcenkataloge

Für Symbole, einem speziellen `AppIcon` Gruppe von Bildern hinzugefügt werden kann die `Assets.xcassets` -Datei in der app-Projekt. Alle Version des Abbilds erforderlich, um die übrigen Auflösungen unterstützen, sind in enthalten die _Xcasset_ und gruppiert. Ein spezieller-Editor in Visual Studio für Mac kann Entwickler und diese Images grafisch einrichten.

Um einen Ressourcenkatalog verwenden zu können, führen Sie folgende Schritte aus:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Doppelklicken Sie auf die `Info.plist` Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen.
2. Führen Sie einen Bildlauf nach unten, um die **-App-Symbole** Abschnitt.
3. Von der **Quelle** Dropdownliste sicher **AppIcon** ausgewählt ist: 

    ![](app-icons-images/migrate01.png "Stellen Sie sicher, dass AppIcon ausgewählt ist")
4. Von der **Projektmappen-Explorer**, doppelklicken Sie auf die `Assets.xcassets` Datei, die sie für die Bearbeitung zu öffnen: 

    ![](app-icons-images/asset01.png "Die Assets.xcassets-Datei im Projektmappen-Explorer")
5. Wählen Sie `AppIcon` aus der Liste der Objekte zum Anzeigen der `Icon Editor`:

    ![](app-icons-images/asset02.png "Der Editor AppIcon")
6. Auf auf angegebene Art von Symbol und wählen Sie eine Bilddatei für die erforderliche Typ/Größe oder Ziehen aus einem Ordner in einem Bild und legen Sie diese auf die gewünschte Größe.
7. Klicken Sie auf die **öffnen** Schaltfläche, um das Bild in das Projekt einfügen, und legen Sie es in der Xcasset.
8. Wiederholen Sie für alle Images, die erforderlich sind.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Doppelklicken Sie auf die **"Info.plist"** Datei die **Projektmappen-Explorer**:

    ![](app-icons-images/icon01w.png "Wählen Sie die Datei \"Info.plist\"")
2. Klicken Sie auf die **visuelle Anlagen** Registerkarte, und klicken Sie auf die **Ressourcenkatalog verwenden** Schaltfläche **-App-Symbole**: 

    ![](app-icons-images/icon02w.png "Wählen Sie die Registerkarte \"visuelle Anlagen\"")
3. Von der **Projektmappen-Explorer**, erweitern Sie die **Asset-Katalog** Ordner: 

    ![](app-icons-images/image009.png "Erweitern Sie den Ordner Asset-Katalog")
4. Doppelklicken Sie auf die **Media** Datei, die sie im Editor zu öffnen: 

    ![](app-icons-images/image010.png "Öffnen Sie die Media-Datei im editor")
5. Unter den **Eigenschaften-Explorer** der Entwickler kann auswählen, die verschiedene Arten und Größen von Symbolen, die erforderlich sind.
6. Klicken Sie auf angegebene Art von Symbol, und wählen Sie eine Bilddatei für die erforderliche Typ/Größe.
7. Klicken Sie auf die **öffnen** Schaltfläche, um das Bild in das Projekt einfügen, und legen Sie es in der Xcasset.
8. Wiederholen Sie für alle Images, die erforderlich sind.

-----

Dies ist die bevorzugte Methode der einschließlich und Verwalten von Bildressourcen, die verwendet wird, eine app, Spotlight und Einstellungen Anwendung Symbole bereit.

### <a name="migrating-from-infoplist-to-asset-catalogs"></a>Migrieren von Datei "Info.plist" zu Ressourcenkataloge

Für eine vorhandene Xamarin.iOS-app, mit der `Info.plist` -Datei, um dessen Symbole zu verwalten, es wird dringend empfohlen, dass der Entwickler es wechseln Sie um zu verwenden die `AppIcons` -Standardimage-Medienobjekt in der `Assets.xcassets`.

Führen Sie folgende Schritte aus:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Doppelklicken Sie auf die `Info.plist` Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen.
2. Führen Sie einen Bildlauf nach unten, um die **-App-Symbole** Abschnitt.
3. Von der **Quelle** Dropdownliste **Migrieren zu Asset-Katalogs**: 

    ![](app-icons-images/migrate02.png "Wählen Sie migrieren in Asset-Kataloge")
4. Keine vorhandenen Symbole definiert, der `Info.plist` Datei migriert wird eine `AppIcons` -Images hinzugefügt `Assets.xcassets`: 

     ![](app-icons-images/migrate03.png "Legen Sie das Image AppIcons in die Assets.xcassets")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Doppelklicken Sie auf die `Info.plist` Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen.
2. Klicken Sie auf dem iPhone Symbole-Abschnitt: 

    ![](app-icons-images/image007.png "Rhe iPhone-Symbole-editor")
3. Führen Sie einen Bildlauf nach unten, um die **Symbole** Abschnitt.
4. Von der **Asset-Katalog** Dropdownliste **verwenden Ressourcenkataloge**.
5. Keine vorhandenen Symbole definiert, der `Info.plist` Datei migriert wird eine `Images` Set für `Assets.xcassets`.
6. Speichern Sie die Änderungen an der Datei `Info.plist`.

-----

<a name="itunes" />

## <a name="itunes-artwork"></a>iTunes-Grafik

Wenn Sie die Ad-hoc-Methode der Bereitstellung der app (entweder für Unternehmensbenutzer oder beta-Tests auf echten Geräten) verwenden zu können, muss der Entwickler auch ein 512 x 512 und ein 1024 x 1024-Image, das zur Darstellung der app in iTunes verwendet wird.

Um die iTunes-Grafik festzulegen, führen Sie Folgendes aus:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Doppelklicken Sie auf die `Info.plist` Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen.
2. Scrollen Sie zu der **iTunes-Grafik** im Editor: 

    ![](app-icons-images/itunes01.png "Führen Sie einen Bildlauf zum iTunes Artwork-Abschnitt des Editors")
3. Bild fehlt, klicken Sie auf die Miniaturansicht im Editor, wählen Sie die Bilddatei für die gewünschte iTunes-Grafik aus das Dialogfeld "Datei öffnen" aus, und klicken Sie auf die **OK** Schaltfläche.
4. Wiederholen Sie diesen Schritt, bis alle Images für die app erforderlichen angegeben wurden.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Doppelklicken Sie auf die `Info.plist` Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen.

2. Klicken Sie auf die **visuelle Anlagen** Registerkarte, und erweitern Sie die **iTunes-Grafik**: 

    ![](app-icons-images/itunes01w.png "Bearbeiten die iTunes-Grafik in Visual Studio")
3. Bild fehlt, klicken Sie auf die Miniaturansicht im Editor, wählen Sie die Bilddatei für die gewünschte iTunes-Grafik aus das Dialogfeld "Datei öffnen" aus, und klicken Sie auf die **öffnen** Schaltfläche.
4. Wiederholen Sie diesen Schritt, bis alle Images für die app erforderlichen angegeben wurden.

-----

## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit Bildern (Beispiel)](https://developer.xamarin.com/samples/monotouch/WorkingWithImages/)
- [Hallo iPhone](~/ios/get-started/hello-ios/index.md)
- [Benutzerdefinierte Symbol und Richtlinien für die Erstellung von Images](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html))
