---
title: Startbildschirme für Xamarin.iOS-Apps
description: In diesem Artikel wird erläutert, wie Sie eine app Startbildschirm für alle iOS-Geräte mit jeder Auflösung und Ausrichtung, die Verwendung eines einzelnen Unified Storyboards zu erstellen.
ms.prod: xamarin
ms.assetid: 31A489CA-756B-4B9B-B386-4BADF18EDD33
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/02/2018
ms.openlocfilehash: 0ec1defa29a4fe85c4ae3e809d8733e68cc268ac
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61087535"
---
# <a name="launch-screens-for-xamarinios-apps"></a>Startbildschirme für Xamarin.iOS-Apps

_In diesem Artikel wird erläutert, wie Sie eine app Startbildschirm für alle iOS-Geräte mit jeder Auflösung und Ausrichtung, die Verwendung eines einzelnen Unified Storyboards zu erstellen._

Vor iOS 8 erforderlich, erstellen einen Startbildschirm für eine iOS-app vom Entwickler bieten ein Bildobjekt für jede der verschiedenen geräteausführungen und Lösungen, die in denen die app ausgeführt werden konnte. Seit der Veröffentlichung von iOS 8 hat allerdings war es möglich, eines einzelnen Unified Storyboards zu verwenden, um einen Startbildschirm zu erstellen, die in allen Fällen korrekt aussieht.

Diese kurze exemplarische Vorgehensweise beschreibt, wie erstellen Sie einen Startbildschirm mit entweder einem Storyboard, die standardmäßig in einem neuen Projekt bereitgestellt wird oder mit einem Storyboard manuell zu einem vorhandenen Projekt hinzugefügt wird. Es veranschaulicht dann mithilfe der iOS-Designer eine Image-Sicht und eine Bezeichnung hinzufügen, auf das Storyboard, um Einschränkungen für diese Ansichten festzulegen und um sicherzustellen, dass das Storyboard für verschiedene Geräte und Ausrichtungen korrekt aussieht.

<a name="storyboard" />

## <a name="managing-launch-screens-with-storyboards"></a>Verwalten von Startbildschirme mit Storyboards

In iOS 8 (und höher) kann Entwickler von speziellen Unified Storyboards Geben Sie den Startbildschirm, anstatt eine oder mehrere statische startbilder erstellen. Beim Start eines Storyboards in der iOS-Designer zu erstellen, verwenden Sie Größenklassen und automatisches Layout, um verschiedene Layouts für andere Umgebungen zu definieren. Der Entwickler kann mithilfe von Größenklassen und automatisches Layout, erstellen eine einzelne Startbildschirm, die auf allen Geräten gut aussieht und Umgebungen anzeigen.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Erstellen Sie ein neues Projekt in Visual Studio für Mac dazu **Datei > neue Projektmappe** auswählen und dann **Einzelansicht-App**: 

    ![Das neue Projektfenster mit Einzelansicht-App ausgewählt](launch-screens-images/launch01.png)

    - Standardmäßig enthält ein neues Projekt eine **LaunchScreen.storyboard** -Datei, die die Startbildschirm-Schnittstelle definiert. 
    - Um stattdessen Storyboard starten Bildschirm zu einem vorhandenen Projekt hinzuzufügen, mit der Maustaste, auf den Namen des Projekts in der **Lösungspad** , und wählen Sie **hinzufügen > neue Datei...**  und wählen Sie dann **Startbildschirm**:

    ![Klicken Sie im neuen Datei mit iOS Startbildschirm ausgewählt](launch-screens-images/launch01b.png)

    - Nennen Sie die Datei **LaunchScreen** oder einen anderen Namen Ihrer Wahl.

2. Konfigurieren Sie das Projekt, um das entsprechende Storyboard für seine Startbildschirm verwenden:

    - Doppelklicken Sie auf die **"Info.plist"** Datei die **Lösungspad** um ihn zur Bearbeitung zu öffnen.
    - In der **Startbilder** Abschnitt, stellen Sie sicher, dass **Startbildschirm** auf den Namen des entsprechenden Storyboards festgelegt ist:

    ![Der Startbildschirm-Selektor in "Info.plist"](launch-screens-images/launch02.png)

    - Wird standardmäßig ein neues Projekt für die Verwendung konfiguriert **LaunchScreen.storyboard** als der Startbildschirm.

3. Fügen Sie ein Bild, um die **Assets.xcassets** Asset zu katalogisieren, sodass sie für die Verwendung auf dem Startbildschirm verfügbar ist. Weitere Informationen finden Sie unter den [Bilder hinzufügen, um eine Asset-Katalog-Image festzulegen](~/ios/app-fundamentals/images-icons/displaying-an-image.md) Teil der [Anzeigen eines Bilds](~/ios/app-fundamentals/images-icons/displaying-an-image.md) Guide.

4. Open **LaunchScreen.storyboard** für die Bearbeitung durch Doppelklick im der **Lösungspad**.

5. Wählen Sie ein Gerät und die Ausrichtung für die das Starten Bildschirm Storyboard in der iOS-Designer-Vorschau. Öffnen Sie den Gerätebereich der Auswahl auf der unteren Symbolleiste, und wählen **iPhone 4 s** und **Hochformat**.

    ![Der Geräte-Auswahl-Symbolleiste](launch-screens-images/launch05.png)

    - Beachten Sie, dass durch Auswahl von einem Gerät und Ausrichtung nur ändert sich wie der iOS-Designer das Design zeigt eine Vorschau. Unabhängig davon, die hier vorgenommene Auswahl neu hinzugefügte Einschränkungen gelten für alle Geräte und -Ausrichtungen, es sei denn, die **bearbeiten "traits"** Schaltfläche wurde verwendet, um nichts anderes angeben. 

6. Legen Sie die **Hintergrund** Farbe der Hauptansicht der View-Controller. Wählen Sie die Ansicht, indem Sie in der Mitte des Ansichtscontrollers auf, und passen Sie die Background-Farbe mit der **Pad "Eigenschaften"**:

    ![Eine einzige Ansicht mit einem violetten Hintergrundfarbe](launch-screens-images/launch06.png)

7. Hinzufügen einer **Image View** auf dem Bildschirm zu starten, und legen seine Quelle **Image**:

    - Ziehen Sie ein **Image View** aus der **Pad "Toolbox"** in die Mitte der Ansicht.
    - Mit der **Image View** ausgewählt haben, in der **Widget** Teil der **Pad "Eigenschaften"** Festlegen der **Image** Eigenschaft, um das Image wurde bereits festgelegt hinzugefügt, die **Assets.xcassets** Asset-Katalog. Position und Größe der **Image View** nach Bedarf:
    
    ![Ein Image View mit festgelegter Eigenschaft "Image"](launch-screens-images/launch07.png)

8. Hinzufügen einer **Bezeichnung** unten die **Image View** und verwenden Sie die **Pad "Eigenschaften"** , dessen Attribute festzulegen: 

    ![Eine Bezeichnung mit einem Satz von Text und Farbe](launch-screens-images/launch08.png)

9. Wechseln Sie zur Einschränkungsbearbeitungsmodus, mit der rechten Schaltfläche in der **Symbolleiste für Einschränkungen**:
    
    ![Die Schaltfläche "Einschränkungsbearbeitungsmodus"](launch-screens-images/launch09.png)

10. Hinzufügen von Einschränkungen, die die **Image View**, Höhe und Breite festlegen und es horizontal und vertikal zentrieren:

    ![Ein Image View Layout-Einschränkungen](launch-screens-images/launch10.png)

    - Weitere Informationen zum Hinzufügen von Einschränkungen finden Sie unter [Automatisches Layout mit dem Xamarin-Designer für iOS](~/ios/user-interface/designer/designer-auto-layout.md).

11. Hinzufügen von Einschränkungen, die die **Bezeichnung**, es horizontal zentrieren, legen sie eine Höhe und Breite und positionieren Sie sie einer festen distance vertikal von der **Image View**:

    ![Eine Bezeichnung mit dem Layout-Einschränkungen](launch-screens-images/launch11.png)

12. Testen Sie andere Geräte und Ausrichtungen, um sicherzustellen, dass der Entwurf sieht aus wie in allen Szenarien vorgesehen. In Fällen, in denen Anpassungen für ein bestimmtes Gerät oder Ausrichtung vorgenommen werden müssen, verwenden die **bearbeiten "traits"** um Nebenbedingungen für bestimmte Größenklassen hinzuzufügen:

    ![Der Startbildschirm als ein iPhone X-Querformat](launch-screens-images/launch12.png)

13. Speichern Sie die Änderungen auf das Storyboard an. Führen Sie die app auf ein Gerät oder Simulator, und der Startbildschirm werden angezeigt, wie die app gestartet wird.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Erstellen Sie ein neues Projekt. Wählen Sie in Visual Studio **Datei > Neu > Projekt > Visual c# > iPhone & iPad > iOS-App (Xamarin)**:

    ![Das neue Projektfenster, mit der iOS-App (Xamarin) ausgewählt](launch-screens-images/launch01.w157.png)

    Wählen Sie die **Einzelansicht-App** Vorlage, und klicken Sie dann auf **OK**:

    ![Einzelvorlage App anzeigen](launch-screens-images/launch01-2.w157.png)

2. Wenn **Ressourcen > LaunchScreen.xib** vorhanden ist, der **Projektmappen-Explorer**, löschen, indem Sie mit der rechten Maustaste auf die Datei, und wählen **löschen**. Diese Datei wird durch ein Storyboard im nächsten Schritt ersetzt werden.

3. Erstellen Sie ein Storyboard, als der Startbildschirm verwenden. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen > Neues Element...**  gefolgt von **leeres Storyboard**. Nennen Sie das Storyboard **LaunchScreen.storyboard** , und klicken Sie auf **hinzufügen**:

    ![Neues Element hinzufügen Fensters leeres Storyboard ausgewählt](launch-screens-images/launch03.w157.png)

4. Konfigurieren Sie das Projekt mit **LaunchScreen.storyboard** als der Bildschirm-Storyboard starten:

    - Doppelklicken Sie auf die **info.plist**-Datei im **Projektmappen-Explorer**, um sie zu öffnen und zu bearbeiten. 
    - Auf der **visuelle Anlagen** Registerkarte **Startbildschirm** zu **LaunchScreen**.

    ![Der Startbildschirm-Selektor in "Info.plist"](launch-screens-images/launch04-vs.png)

5. Fügen Sie ein Bild hinzu ein Asset-Katalog im Projekt, sodass sie für die Verwendung auf dem Startbildschirm verfügbar ist:

    - In der **Projektmappen-Explorer**, mit der rechten Maustaste auf **Ressourcenkataloge** , und wählen Sie **Add Asset Catalog**. Nennen Sie diese neuen Ressourcenkatalog **Assets**:

    ![Neues Element hinzufügen Fensters Asset-Katalog ausgewählt](launch-screens-images/launch05.w157.png)

    - Fügen Sie ein neues Image auf festgelegt der **Assets** Asset-Katalog, wie in beschrieben die [Bilder hinzufügen, um eine Asset-Katalog-Image festzulegen](~/ios/app-fundamentals/images-icons/displaying-an-image.md) im Abschnitt der [Anzeigen eines Bilds](~/ios/app-fundamentals/images-icons/displaying-an-image.md) Guide.

6. Open **LaunchScreen.storyboard** für die Bearbeitung durch Doppelklick im der **Projektmappen-Explorer**.

    - Um eine Storyboard-Datei bearbeiten zu können, benötigt Visual Studio eine aktive Verbindung mit einem Mac-buildhost. Finden Sie unter den [Herstellen einer Verbindung mit dem Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) -Handbuch.

7. Wählen Sie ein Gerät und die Ausrichtung für die das Starten Bildschirm Storyboard in der iOS-Designer-Vorschau. Öffnen Sie den Gerätebereich der Auswahl auf der unteren Symbolleiste, und wählen **iPhone 4 s** und **Hochformat**: 
 
    ![Der Geräte-Auswahl-Symbolleiste](launch-screens-images/launch07-vs.png)

    - Beachten Sie, dass durch Auswahl von einem Gerät und Ausrichtung nur ändert sich wie der iOS-Designer das Design zeigt eine Vorschau. Unabhängig davon, die hier vorgenommene Auswahl neu hinzugefügte Einschränkungen gelten für alle Geräte und -Ausrichtungen, es sei denn, die **bearbeiten "traits"** Schaltfläche wurde verwendet, um nichts anderes angeben. 

8. Hinzufügen einer **Ansichtscontroller** zum Storyboard durch Ziehen aus der **Toolbox** auf die Entwurfsoberfläche: 

    ![Ein leeres View Controller hinzugefügt, auf die Entwurfsoberfläche](launch-screens-images/launch08-vs.png)

9. Legen Sie die **Hintergrund** Farbe der Hauptansicht der View-Controller. Wählen Sie die Ansicht, indem Sie in der Mitte des Ansichtscontrollers auf, und passen Sie die Background-Farbe mit der **Fenster "Eigenschaften"**:
    
    ![Eine einzige Ansicht mit einem violetten Hintergrundfarbe](launch-screens-images/launch09-vs.png)

10. Hinzufügen einer **Image View** auf dem Bildschirm zu starten, und legen seine Quelle **Image**:

    - Ziehen Sie ein **Image View** aus der **Toolbox** in die Mitte der Ansicht.
    - Mit der **Image View** noch ausgewählt ist, in der **Widget** Teil der **Fenster "Eigenschaften"** Festlegen der **Image** festzulegende Eigenschaft das Bild bereits hinzugefügt, um die **Assets** Asset-Katalog. Position und Größe der **Image View** nach Bedarf:
    
    ![Ein Image View mit festgelegter Eigenschaft "Image"](launch-screens-images/launch10-vs.png)

11. Hinzufügen einer **Bezeichnung** unterhalb der **Image View**:

    - Ziehen Sie eine **Bezeichnung** aus der **Toolbox** auf der Entwurfsoberfläche ablegen, damit er unten die **Image View**.
    - Legen Sie Attribute für die **Bezeichnung** mithilfe der **Fenster "Eigenschaften"**:

    ![Eine Bezeichnung mit einem Satz von Text und Farbe](launch-screens-images/launch11-vs.png) 

12. Wechseln Sie zur Einschränkungsbearbeitungsmodus, mit der rechten Schaltfläche in der **Symbolleiste für Einschränkungen**:
    
    ![Die Schaltfläche "Einschränkungsbearbeitungsmodus"](launch-screens-images/launch12-vs.png) 

13. Hinzufügen von Einschränkungen, die die **Image View**, Höhe und Breite festlegen und es horizontal und vertikal zentrieren:

    ![Ein Image View Layout-Einschränkungen](launch-screens-images/launch13-vs.png) 

    - Weitere Informationen zum Hinzufügen von Einschränkungen, finden Sie unter [Automatisches Layout mit dem Xamarin-Designer für iOS](~/ios/user-interface/designer/designer-auto-layout.md).

14. Hinzufügen von Einschränkungen, die die **Bezeichnung**, es horizontal zentrieren, legen sie eine Höhe und Breite und positionieren Sie sie einer festen distance vertikal von der **Image View**:
    
    ![Eine Bezeichnung mit dem Layout-Einschränkungen](launch-screens-images/launch14-vs.png) 

15. Testen Sie andere Geräte und Ausrichtungen, um sicherzustellen, dass der Entwurf sieht aus wie in allen Szenarien vorgesehen. In Fällen, in denen Anpassungen für ein bestimmtes Gerät oder Ausrichtung vorgenommen werden müssen, verwenden die **bearbeiten "traits"** um Nebenbedingungen für bestimmte Größenklassen hinzuzufügen:

    ![Der Startbildschirm als ein iPhone X-Querformat](launch-screens-images/launch15-vs.png) 

16. Speichern Sie die Änderungen auf das Storyboard an. Führen Sie die app auf ein Gerät oder Simulator, und der Startbildschirm werden angezeigt, wie die app gestartet wird.

-----

> [!NOTE]
> Ein Storyboard als einen Startbildschirm _müssen_ nur einfache, integrierte UI-Elemente enthalten und **kann nicht** Berechnungen ausführen oder eine benutzerdefinierte Klasse abgeleitet.

Weitere Informationen zum Erstellen eines Bildschirms starten mit einem Storyboard Unified finden Sie unter den [dynamische Startbildschirme](~/ios/user-interface/storyboards/unified-storyboards.md#dynamic-launch-screens) Teil der [Unified Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md) Guide.

## <a name="migrating-to-launch-screen-storyboards"></a>Migrieren zum Bildschirm Storyboards zu starten

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Bei der Aktualisierung einer vorhandenen app, um Storyboards zu verwenden, für die Startbildschirme mit der rechten Maustaste die **Projektname** in die **Projektmappen-Explorer** , und wählen Sie **hinzufügen**  >  **Neue Datei...** . Wählen Sie **iOS** > **Startbildschirm** , und klicken Sie auf die **neu** Schaltfläche:

![](launch-screens-images/storyboard02.png "Wählen Sie ein iOS-Startbildschirm")

Doppelklicken Sie dann auf die `Info.plist` Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen. Klicken Sie unter **Startbildschirm**, wählen Sie die neue Storyboard-Datei, die oben erstellt haben.

![](launch-screens-images/storyboard09.png "Wählen Sie die oben erstellte neue Storyboard-Datei")


Führen Sie folgende Schritte aus, um das neue Storyboard als einen Startbildschirm zu verwenden:

1. Doppelklicken Sie auf die `Info.plist` Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen.
2. Scrollen Sie zu der **universelle Startbilder** im Editor, öffnen die **Startbildschirm** Dropdownliste und wählen Sie der Namen des Storyboards oben erstellten: 

    ![](launch-screens-images/storyboard08.png "Im Startbildschirm festlegen auf das storyboard")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Mit der rechten Maustaste auf den Projektnamen in der **Projektmappen-Explorer** , und wählen Sie **hinzufügen** > **neue Datei...** : 

    ![](launch-screens-images/image012.png "Neue Datei hinzufügen")
2. Geben Sie einen Namen für den Startbildschirm, und klicken Sie auf die **hinzufügen** Schaltfläche: 

    ![](launch-screens-images/image013.png "Geben Sie einen Namen für den Startbildschirm")
3. In der **Projektmappen-Explorer**, doppelklicken Sie auf die neu erstellte Storyboarddatei, um es zur Bearbeitung zu öffnen.
4. Sicherstellen, dass die **Größenklasse** nastaven NA hodnotu **alle: alle** und **anzeigen als** ist **generische**: 

    ![](launch-screens-images/image016.png "Stellen Sie sicher, dass die Größenklasse auf einen festgelegt ist: jeder und die Ansicht ist generisch")
5. Assembly der Startbildschirm von Größenklassen, einfache Benutzeroberflächenelemente (z. B. `UIImageView`) und Images, die Sie in der Anwendung Paket aufgenommen haben: 

    ![](launch-screens-images/image017.png "Assembly der Startbildschirm im iOS-Designer")
6. Speichern Sie die Änderungen auf das Storyboard an.

-----

## <a name="related-links"></a>Verwandte Links

- [Dynamische Startbildschirme (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/)
- [Einheitliche Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md)
- [iOS-Designer-Grundlagen](~/ios/user-interface/designer/index.md)
- [Hinzufügen von Bildern zu einem Bild, Asset-Katalog festlegen](~/ios/app-fundamentals/images-icons/displaying-an-image.md#adding-images-to-an-asset-catalog-image-set)
- [Automatisches Layout mit dem Xamarin-Designer für iOS](~/ios/user-interface/designer/designer-auto-layout.md)
- [Human Interface Guidelines: Startbildschirm](https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/launch-screen/)
