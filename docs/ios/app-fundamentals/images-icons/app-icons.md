---
title: Anwendungs Symbole in xamarin. IOS
description: 'In diesem Dokument wird beschrieben, wie Sie mit verschiedenen Anwendungssymbolen in xamarin. IOS arbeiten: das Anwendungssymbol selbst, Spotlight-Symbole, die Einstellungs Symbole und iTunes-Grafiken.'
ms.prod: xamarin
ms.assetid: B7791574-4A0F-4CB6-8C18-36D40B5C91EB
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/22/2017
ms.openlocfilehash: 65e42e824a888934fd0f0a01093a6549dcf3d99d
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997461"
---
# <a name="application-icons-in-xamarinios"></a>Anwendungs Symbole in xamarin. IOS

Die folgenden Themen werden im Detail behandelt:

- [Symbole für Anwendungen, Spotlight und Einstellungen](#icon-types) : die verschiedenen Typen von Symbolen, die für eine IOS-App erforderlich sind.
- [Verwalten von Symbolen mit Asset-Katalogen](#managing) : Verwalten von Anwendungssymbolen mithilfe von Asset-Katalogen
- [iTunes-Grafik](#itunes) : Bereitstellen der erforderlichen iTunes-Grafik für die Ad-hoc-Methode der Bereitstellung Ihrer Anwendung.

<a name="icon-types"></a>

## <a name="application-spotlight-and-settings-icons"></a>Symbole für Anwendung, Spotlight und Einstellungen

Auf dieselbe Weise, wie eine xamarin. IOS-App Image-Assets für UI-Steuerelemente und Dokument Symbole verwenden kann, können Image-Assets zum Bereitstellen von Anwendungssymbolen verwendet werden. Die folgenden Screenshots von einem iPad veranschaulichen die drei Verwendungsmöglichkeiten von Symbolen in ios:

- **Anwendungssymbol** : jede IOS-app muss ein Anwendungssymbol definieren. Dies ist das Symbol, auf das der Benutzer vom IOS-Startbildschirm tippt, um die APP zu starten. Außerdem wird dieses Symbol ggf. von Game Center verwendet. Beispiel:

    [![Anwendungssymbol](app-icons-images/000.png)](app-icons-images/000-full.png#lightbox)
- **Spotlight-Symbol** : Wenn der Benutzer den Namen einer APP in eine Spotlight-Suche eingibt, wird dieses Symbol angezeigt. Beispiel:

    [![Spotlight-Symbol](app-icons-images/000a.png)](app-icons-images/000a-full.png#lightbox)
- **Symbol "Einstellungen** ": Wenn der Benutzer die app " **Einstellungen** " auf seinem IOS-Gerät eingibt, wird dieses Symbol am Ende der **Einstellungs** Liste für die App angezeigt. Beispiel:

    [![Symbol "Einstellungen"](app-icons-images/000b.png)](app-icons-images/000b-full.png#lightbox)

Die folgenden Image assetgrößen und-Lösungen sind erforderlich, um alle für eine xamarin. IOS-App mit IOS 5 bis IOS 9 (oder höher) erforderlichen Symboltypen zu unterstützen:

### <a name="iphone-icon-sizes"></a>iPhone-Symbolgrößen

- **iPhone: IOS 9 & 10 (iPhone 6 & 7 plus)**

    |Symbol|3X|
    |---|---|
    |Anwendungssymbol|180 x 180|
    |Spotlight|120x120|
    |Einstellungen|87x87|

- **iPhone: IOS 7 & 8**

    |Symbol|1x|2x|
    |---|---|---|
    |Anwendungssymbol|60 x 60<sup>1</sup>|120x120|
    |Spotlight|40 x 40<sup>2</sup>|80 x 80|
    |Einstellungen|-|-|

- **iPhone: IOS 5 & 6**

    |Symbol|1x|2x|
    |---|---|---|
    |Anwendungssymbol|57x57|114 x 114|
    |Spotlight|29x29|58x58|
    |Einstellungen|29x29<sup>3, 4</sup>|58x58<sup>3, 4</sup>|

### <a name="ipad-icon-sizes"></a>iPad-Symbolgrößen

- **iPad: IOS 9 & 10**

    |Symbol|2x (iPad pro)|
    |---|---|
    |Anwendungssymbol|167x167<sup>6</sup>|
    |Spotlight|120 x 120<sup>6</sup>|
    |Einstellungen|58x58<sup>5</sup>|

- **iPad: IOS 7 & 8**

    |Symbol|1x|2x|
    |---|---|---|
    |Anwendungssymbol|76x76|152 x 152|
    |Spotlight|40 x 40|80 x 80|
    |Einstellungen|-|-|

- **iPad: IOS 5 & 6**

    |Symbol|1x|2x|
    |---|---|---|
    |Anwendungssymbol|72 x 72|144 x 144|
    |Spotlight|50x50|deren|
    |Einstellungen|29x29<sup>3, 5</sup>|58x58<sup>3, 5</sup>|

 1. Sowohl Visual Studio für Mac als auch Xcode unterstützen nicht mehr das Einrichten von 1 x-Image für IOS 7.
 2. Das Festlegen eines 1X-Images für IOS 7 wird bei der Verwendung von Asset-Katalogen nicht unterstützt.
 3. IOS 7 & 8 verwendet die gleichen Bildgrößen wie IOS 5 & 6.
 4. Verwendet die gleichen Bilder und Größen wie das Spotlight-Symbol.
 5. Verwendet die gleichen Größen Symbole wie das iPhone.
 6. Wird nur mit Ressourcen Katalog-Image Sätzen unterstützt.

 Weitere Informationen zu Symbolen finden Sie in der Dokumentation zu [Symbol-und Bildgrößen](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/IconMatrix.html#//apple_ref/doc/uid/TP40006556-CH27-SW1) von Apple.

<a name="managing"></a>

## <a name="managing-icons-with-asset-catalogs"></a>Verwalten von Symbolen mit Asset-Katalogen

Bei Symbolen `AppIcon` kann der `Assets.xcassets` Datei im Projekt der APP ein spezieller Bildsatz hinzugefügt werden. Alle Versionen des Bilds, die zur Unterstützung aller Auflösungen erforderlich sind, sind im _xcasset_ enthalten und zusammengefasst. Mit einem speziellen Editor in Visual Studio für Mac können Entwickler diese Bilder grafisch einschließen und einrichten.

Um einen Asset-Katalog zu verwenden, führen Sie die folgenden Schritte aus:

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

1. Doppelklicken Sie auf die `Info.plist` Datei im **Projektmappen-Explorer** , um Sie für die Bearbeitung zu öffnen.
2. Scrollen Sie nach unten zum Abschnitt **iPhone-Symbole** .
3. Klicken Sie auf die Schaltfläche **zu Asset Catalog migrieren** :

    ![Sicherstellen, dass AppIcon ausgewählt ist](app-icons-images/migrate01.png)

4. Doppelklicken Sie im **Projektmappen-Explorer**auf die `Assets.xcassets` Datei, um Sie zur Bearbeitung zu öffnen:

    ![Die Datei Assets. xcassets im Projektmappen-Explorer](app-icons-images/asset01.png)

5. Wählen Sie `AppIcon` aus der Liste der Assets aus, um Folgendes anzuzeigen `Icon Editor` :

    ![Der AppIcon-Editor](app-icons-images/asset02.png)

6. Klicken Sie entweder auf den angegebenen Symboltyp, und wählen Sie eine Bilddatei für die erforderliche Art/Größe aus, oder ziehen Sie ein Bild aus einem Ordner, und legen Sie es auf der gewünschten Größe ab.
7. Klicken Sie auf die Schaltfläche **Öffnen** , um das Bild in das Projekt einzuschließen, und legen Sie es im xcasset fest.
8. Wiederholen Sie diesen Schritt für alle benötigten Images.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Doppelklicken Sie auf die * *-Informationen.  * * Datei in der **Projektmappen-Explorer**:

    ![Info. plist auswählen](app-icons-images/icon01w.png)

2. Klicken Sie auf die Registerkarte **visuelle Objekte** und dann unter **App-Symbole**auf die Schaltfläche " **Asset catalog verwenden** ":

    ![Registerkarte "visuelle Objekte" auswählen](app-icons-images/icon02w.png)

    Wenn keine Schaltfläche vorhanden ist, sondern stattdessen eine Dropdown Liste, wurde diesem Projekt bereits ein Asset-Katalog hinzugefügt.

3. Erweitern Sie auf der **Projektmappen-Explorer**den Ordner **Asset Catalog** :

    ![Erweitern Sie den Ordner "Asset catalog"](app-icons-images/image009.png)

4. Doppelklicken Sie auf die **Medien** Datei, um Sie im Editor zu öffnen:

    ![Öffnen Sie die Mediendatei im Editor.](app-icons-images/image010.png)

5. Im **Eigenschaften-Explorer** kann der Entwickler die verschiedenen Typen und Größen der erforderlichen Symbole auswählen.
6. Klicken Sie auf den angegebenen Symboltyp, und wählen Sie eine Bilddatei für die erforderliche Art/Größe aus.
7. Klicken Sie auf die Schaltfläche **Öffnen** , um das Bild in das Projekt einzuschließen, und legen Sie es im xcasset fest.
8. Wiederholen Sie diesen Schritt für alle benötigten Images.

-----

Dies ist die bevorzugte Methode zum einschließen und Verwalten von bildassets, die verwendet werden, um Symbole für Anwendungen, Spotlight und Einstellungen für eine APP bereitzustellen.

<a name="itunes"></a>

## <a name="itunes-artwork"></a>iTunes-Grafik

Bei Verwendung der Ad-hoc-Methode der Bereitstellung der APP (entweder für Unternehmensbenutzer oder für Beta-Tests auf echten Geräten) muss der Entwickler auch ein 512 x512-und 1024 x1024-Bild einschließen, das zur Darstellung der app in iTunes verwendet wird.

Um die iTunes-Grafik festzulegen, führen Sie Folgendes aus:

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

1. Doppelklicken Sie auf die `Info.plist` Datei im **Projektmappen-Explorer** , um Sie für die Bearbeitung zu öffnen.
2. Scrollen Sie zum Abschnitt **iTunes-Grafik** des Editors:

    ![Scrollen Sie zum Abschnitt iTunes-Grafik des Editors.](app-icons-images/itunes01.png)
3. Klicken Sie für ein fehlendes Bild im Editor auf die Miniaturansicht, wählen Sie im Dialogfeld Datei öffnen die Bilddatei für die gewünschte iTunes-Grafik aus, und klicken Sie auf die Schaltfläche **OK** .
4. Wiederholen Sie diesen Schritt, bis alle benötigten Images für die APP angegeben wurden.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Doppelklicken Sie auf die `Info.plist` Datei im **Projektmappen-Explorer** , um Sie für die Bearbeitung zu öffnen.

2. Klicken Sie auf die Registerkarte **visuelle Assets** , und erweitern Sie die **iTunes-Grafik**:

    ![Bearbeiten von iTunes-Grafiken in Visual Studio](app-icons-images/itunes01w.png)
3. Klicken Sie für ein fehlendes Bild im Editor auf die Miniaturansicht, wählen Sie im Dialogfeld Datei öffnen die Bilddatei für die gewünschte iTunes-Grafik aus, und klicken Sie auf die Schaltfläche **Öffnen** .
4. Wiederholen Sie diesen Schritt, bis alle benötigten Images für die APP angegeben wurden.

-----

## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit Bildern (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithimages)
- [Hallo iPhone](~/ios/get-started/hello-ios/index.md)
- [Richtlinien für benutzerdefiniertes Symbol und Bild Erstellung](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
