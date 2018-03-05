---
title: Starten Sie Bildschirme
description: "Dieser Artikel beschreibt, wie eine app starten Bildschirm für alle iOS-Geräten bei jeder Auflösung und Ausrichtung, die mit einem einzelnen Unified Storyboard zu erstellen."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7A3F3C94-2578-4F53-9B8E-25714F48BDD6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/19/2018
ms.openlocfilehash: 48dc2e7a270c4e12c4e3dc9d1e2ce14fb0d41249
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="launch-screens"></a>Starten Sie Bildschirme

_Dieser Artikel beschreibt, wie eine app starten Bildschirm für alle iOS-Geräten bei jeder Auflösung und Ausrichtung, die mit einem einzelnen Unified Storyboard zu erstellen._

Bevor Sie iOS 8 erforderlich, Erstellen eines Bildschirms Starten einer iOS-App den Entwickler, geben Sie ein Standardimage-Medienobjekt für jede der verschiedenen Formfaktoren Gerät und Lösungen, die in denen die app ausgeführt werden konnte. Seit iOS 8-Version jedoch wurde ursprünglich möglich, eine einzelne einheitliche Storyboard zu verwenden, erstellen Sie einen Bildschirm zu starten, die in allen Fällen richtig aussieht.

Diese kurze exemplarische Vorgehensweise wird beschrieben, wie ein starten Bildschirm mit entweder ein Storyboard in einem neuen Projekt standardmäßig bereitgestellten oder ein Storyboard manuell hinzugefügt werden, um ein vorhandenes Projekt erstellt wird. Dann wird veranschaulicht, wie mithilfe der iOS-Designer fügen eine Bild-Ansicht und eine Bezeichnung Storyboard, um Einschränkungen für diese Sichten festzulegen und um sicherzustellen, dass das Storyboard für verschiedene Geräte und Ausrichtungen richtig aussieht.

<a name="storyboard" />

## <a name="managing-launch-screens-with-storyboards"></a>Sie Startbildschirme mit Storyboards verwalten

In iOS 8 (und höher) kann der Entwickler eine spezielle Unified Storyboard um den Bildschirm starten, anstatt ein oder mehrere statische Launch-Abbilder bereitzustellen erstellen. Beim Starten Storyboard in der iOS-Designer zu erstellen, verwenden Sie Größenklassen und automatisches Layout, um unterschiedliche Layouts für andere Umgebungen zu definieren. Größenklassen und automatisches Layout verwenden, kann der Entwickler erstellen eine einzelne Startbildschirm, das auf allen Geräten gut aussieht und Umgebungen angezeigt.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Erstellen Sie ein neues Projekt in Visual Studio für Mac dazu **Datei > neue Projektmappe** auswählen und dann **einzelne Ansicht App**: 

    ![Das Fenster Neues Projekt mit einzelne Ansicht Apps ausgewählt](launch-screens-images/launch01.png)

    - Wird standardmäßig ein neues Projekt umfasst eine **LaunchScreen.storyboard** Datei, die die Schnittstelle starten Bildschirm definiert. 
    - Um ein vorhandenes Projekt stattdessen ein starten Bildschirm Storyboard hinzuzufügen, mit der Maustaste, auf den Namen des Projekts in der **Lösung Pad** , und wählen Sie **hinzufügen > neue Datei...**  und wählen Sie dann **starten Bildschirm**:

    ![Die neue Datei angezeigt, mit iOS starten Bildschirm ausgewählt](launch-screens-images/launch01b.png)

    - Nennen Sie die Datei **LaunchScreen** oder einen anderen Namen Ihrer Wahl.

2. Konfigurieren Sie das Projekt, um das entsprechende Storyboard für seine starten Bildschirm verwenden:

    - Doppelklicken Sie auf die **"Info.plist"** in der Datei die **Lösung Pad** um ihn zur Bearbeitung zu öffnen.
    - In der **starten Bilder** Abschnitt, stellen Sie sicher, dass **starten Bildschirm** auf den Namen des entsprechenden Storyboards festgelegt ist:

    ![Der Bildschirm starten-Selektor in der Datei "Info.plist"](launch-screens-images/launch02.png)

    - Wird standardmäßig ein neues Projekt für die Verwendung konfiguriert **LaunchScreen.storyboard** als der Bildschirm zu starten.

3. Hinzufügen eines Bilds auf der **Assets.xcassets** Asset Katalog, damit es auf dem Bildschirm starten verfügbar ist. Weitere Informationen finden Sie unter der [Bilder hinzufügen, eine Asset-Katalog Image festgelegt](~/ios/app-fundamentals/images-icons/displaying-an-image.md) Teil der [Anzeigen eines Bilds](~/ios/app-fundamentals/images-icons/displaying-an-image.md) Handbuch.

4. Öffnen **LaunchScreen.storyboard** durch Doppelklick im für die Bearbeitung der **Lösung Pad**.

5. Wählen Sie ein Gerät und die Ausrichtung auf dem das Starten Bildschirm Storyboard in der iOS-Designer in der Vorschau anzeigen. Öffnen Sie Gerätebereich Auswahl auf der unteren Symbolleiste, und wählen Sie **iPhone 4 s** und **Hochformat**.

    ![Der Gerätesymbolleiste-Auswahl](launch-screens-images/launch05.png)

    - Hinweis: Auswählen eines Geräts und die Ausrichtung nur ändert, wie in der iOS-Designer den Entwurf Vorschau anzeigt. Unabhängig von der hier vorgenommenen Auswahl neu hinzugefügten Einschränkungen gelten für alle Geräte sowie Ausrichtungen, es sei denn, die **bearbeiten Traits** Schaltfläche wurde verwendet, um nichts anderes angeben. 

6. Legen Sie die **Hintergrund** Farbe des Hauptansicht die View-Controller. Wählen Sie die Ansicht, indem Sie in der Mitte der View-Controller auf, und passen Sie die Farbe für den Hintergrund mithilfe der **Eigenschaften Pad**:

    ![Eine einzelne Ansicht durch eine violette Hintergrundfarbe](launch-screens-images/launch06.png)

7. Hinzufügen einer **anzeigen** auf dem Bildschirm zu starten, und legen Sie als Quelle **Image**:

    - Ziehen Sie ein **Image Ansicht** aus der **Toolbox Pad** in die Mitte der Ansicht.
    - Mit der **Bild anzeigen** ausgewählt, in der **Widget** im Abschnitt der **Eigenschaften aufgefüllt** Festlegen der **Image** Eigenschaft, um das Image wurde bereits festgelegt hinzugefügt, um die **Assets.xcassets** Asset-Katalog. Position und Größe der **Image Ansicht** nach Bedarf:
    
    ![Eine Sicht Image mit einer Gruppe der Image-Eigenschaft](launch-screens-images/launch07.png)

8. Hinzufügen einer **Bezeichnung** unterhalb der **Bild anzeigen** und Verwenden der **Eigenschaften mit Leerstellen auffüllen** Festlegen seiner Attribute: 

    ![Eine Bezeichnung mit einer Gruppe von Text und Farbe](launch-screens-images/launch08.png)

9. Wechseln Sie zur Einschränkung-Bearbeitungsmodus mit der rechten Schaltfläche in der **Einschränkungen Symbolleiste**:
    
    ![Die Einschränkung Bearbeitungsmodus-Schaltfläche](launch-screens-images/launch09.png)

10. Fügen Sie eine Beschränkung der **anzeigen**, das die Höhe und Breite festlegen und es horizontal und vertikal zentrieren:

    ![Ein Image-Ansicht mit Layout-Einschränkungen](launch-screens-images/launch10.png)

    - Weitere Informationen zum Hinzufügen von Einschränkungen finden Sie unter [Automatisches Layout mit dem Xamarin-Designer für iOS](~/ios/user-interface/designer/designer-auto-layout.md).

11. Einschränkungen zum Hinzufügen der **Bezeichnung**, es horizontal zentrieren, und geben sie Ihnen eine Höhe und Breite und positionieren Sie es eine feste vertikalen Abstand von der **Image Ansicht**:

    ![Eine Bezeichnung mit Layout-Einschränkungen](launch-screens-images/launch11.png)

12. Testen Sie andere Geräte und die Ausrichtungen, um sicherzustellen, dass der Entwurf aussieht, wie in allen Szenarien vorgesehen. In Fällen, in denen Anpassungen für ein bestimmtes Gerät oder Ausrichtung vorgenommen werden müssen, verwenden die **bearbeiten Traits** Schaltfläche zum Hinzufügen von Nebenbedingungen für bestimmte Größenklassen:

    ![Der Bildschirm starten als iPhone X Querformat](launch-screens-images/launch12.png)

13. Speichern Sie die Änderungen auf das Storyboard. Führen Sie die app auf einem Simulator oder ein Gerät, und der Bildschirm gestartet werden angezeigt, wie die app gestartet wird.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Erstellen Sie ein neues Projekt. Wählen Sie in Visual Studio **Datei > Neu > Projekt**, und wählen Sie dann **einzelne Ansicht App (iPhone)**:
    
    ![Das Fenster Neues Projekt, mit der App für einzelne anzeigen (iPhone) ausgewählt](launch-screens-images/launch01-vs.png)

    - Nennen Sie das Projekt, wählen Sie einen Speicherort aus, und wählen **OK**.

2. Wenn **Ressourcen > LaunchScreen.xib** vorhanden ist, der **Projektmappen-Explorer**, löschen Sie ihn, indem Sie mit der rechten Maustaste auf die Datei und **löschen**. Diese Datei wird durch ein Storyboard im nächsten Schritt ersetzt werden.

3. Erstellen Sie ein Storyboard mithilfe des Bildschirms starten. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen > Neues Element...**  gefolgt von **leere Storyboard**. Nennen Sie das Storyboard **LaunchScreen.storyboard** , und klicken Sie auf **hinzufügen**:

    ![Klicken Sie im Fenster Neues Element hinzufügen mit leeren Storyboard ausgewählt](launch-screens-images/launch03-vs.png)

4. Konfigurieren Sie das Projekt mit **LaunchScreen.storyboard** als der Bildschirm-Storyboard starten:

    - Doppelklicken Sie auf die **info.plist**-Datei im **Projektmappen-Explorer**, um sie zu öffnen und zu bearbeiten. 
    - Auf der **visuelle Anlagen** Registerkarte **starten Bildschirm** auf **LaunchScreen**.

    ![Der Bildschirm starten-Selektor in der Datei "Info.plist"](launch-screens-images/launch04-vs.png)

5. Hinzufügen eines Bilds eine Asset-Katalog im Projekt, damit es zur Verwendung auf dem Bildschirm gestartet wird:

    - In der **Projektmappen-Explorer**, mit der rechten Maustaste auf **Asset Kataloge** , und wählen Sie **Asset-Katalog hinzufügen**. Nennen Sie diesen neuen Asset-Katalog **Bestand**:

    ![Klicken Sie im Fenster Neues Element hinzufügen mit Asset-Katalog ausgewählt](launch-screens-images/launch05-vs.png)

    - Hinzufügen einer neuen Bildersatz der **Bestand** Asset-Katalog, wie in beschrieben die [Bilder hinzufügen, eine Asset-Katalog Image festgelegt](~/ios/app-fundamentals/images-icons/displaying-an-image.md) Teil der [Anzeigen eines Bilds](~/ios/app-fundamentals/images-icons/displaying-an-image.md) Handbuch.

6. Open **LaunchScreen.storyboard** durch Doppelklick im für die Bearbeitung der **Projektmappen-Explorer**.

    - Um ein Storyboarddatei bearbeiten zu können, benötigt Visual Studio eine aktive Verbindung mit einem Mac-Build-Host. Finden Sie unter der [Herstellen einer Verbindung mit dem Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) -Handbuch.

7. Wählen Sie ein Gerät und die Ausrichtung auf dem das Starten Bildschirm Storyboard in der iOS-Designer in der Vorschau anzeigen. Öffnen Sie Gerätebereich Auswahl auf der unteren Symbolleiste, und wählen Sie **iPhone 4 s** und **Hochformat**: 
 
    ![Der Gerätesymbolleiste-Auswahl](launch-screens-images/launch07-vs.png)

    - Hinweis: Auswählen eines Geräts und die Ausrichtung nur ändert, wie in der iOS-Designer den Entwurf Vorschau anzeigt. Unabhängig von der hier vorgenommenen Auswahl neu hinzugefügten Einschränkungen gelten für alle Geräte sowie Ausrichtungen, es sei denn, die **bearbeiten Traits** Schaltfläche wurde verwendet, um nichts anderes angeben. 

8. Hinzufügen einer **Modellansichtcontroller** auf das Storyboard durch Ziehen von der **Toolbox** auf die Entwurfsoberfläche: 

    ![Ein leeres View Controller auf die Entwurfsoberfläche hinzugefügt](launch-screens-images/launch08-vs.png)

9. Legen Sie die **Hintergrund** Farbe des Hauptansicht die View-Controller. Wählen Sie die Ansicht, indem Sie in der Mitte der View-Controller auf, und passen Sie die Farbe für den Hintergrund mit der die **Fenster "Eigenschaften"**:
    
    ![Eine einzelne Ansicht durch eine violette Hintergrundfarbe](launch-screens-images/launch09-vs.png)

10. Hinzufügen einer **anzeigen** auf dem Bildschirm zu starten, und legen Sie als Quelle **Image**:

    - Ziehen Sie ein **Image Ansicht** aus der **Toolbox** in die Mitte der Ansicht.
    - Mit der **Bild anzeigen** noch ausgewählt, in der **Widget** im Abschnitt der **Fenster "Eigenschaften"** Festlegen der **Image** festzulegende Eigenschaft das Bild bereits hinzugefügt, um die **Bestand** Asset-Katalog. Position und Größe der **Image Ansicht** nach Bedarf:
    
    ![Eine Sicht Image mit einer Gruppe der Image-Eigenschaft](launch-screens-images/launch10-vs.png)

11. Hinzufügen einer **Bezeichnung** unterhalb der **Image Ansicht**:

    - Ziehen Sie eine **Bezeichnung** aus der **Toolbox** auf der Entwurfsoberfläche platzieren sie unten die **anzeigen**.
    - Legen Sie Attribute für die **Bezeichnung** mithilfe der **Fenster "Eigenschaften"**:

    ![Eine Bezeichnung mit einer Gruppe von Text und Farbe](launch-screens-images/launch11-vs.png) 

12. Wechseln Sie zur Einschränkung-Bearbeitungsmodus mit der rechten Schaltfläche in der **Einschränkungen Symbolleiste**:
    
    ![Die Einschränkung Bearbeitungsmodus-Schaltfläche](launch-screens-images/launch12-vs.png) 

13. Fügen Sie eine Beschränkung der **anzeigen**, das die Höhe und Breite festlegen und es horizontal und vertikal zentrieren:

    ![Ein Image-Ansicht mit Layout-Einschränkungen](launch-screens-images/launch13-vs.png) 

    - Informationen zum Hinzufügen von Einschränkungen finden Sie unter [Automatisches Layout mit dem Xamarin-Designer für iOS](~/ios/user-interface/designer/designer-auto-layout.md).

14. Einschränkungen zum Hinzufügen der **Bezeichnung**, es horizontal zentrieren, und geben sie Ihnen eine Höhe und Breite und positionieren Sie es eine feste vertikalen Abstand von der **Image Ansicht**:
    
    ![Eine Bezeichnung mit Layout-Einschränkungen](launch-screens-images/launch14-vs.png) 

15. Testen Sie andere Geräte und die Ausrichtungen, um sicherzustellen, dass der Entwurf aussieht, wie in allen Szenarien vorgesehen. In Fällen, in denen Anpassungen für ein bestimmtes Gerät oder Ausrichtung vorgenommen werden müssen, verwenden die **bearbeiten Traits** Schaltfläche zum Hinzufügen von Nebenbedingungen für bestimmte Größenklassen:

    ![Der Bildschirm starten als iPhone X Querformat](launch-screens-images/launch15-vs.png) 

16. Speichern Sie die Änderungen auf das Storyboard. Führen Sie die app auf einem Simulator oder ein Gerät, und der Bildschirm gestartet werden angezeigt, wie die app gestartet wird.

-----

> [!NOTE]
> **Hinweis**: ein Storyboard verwendet als Bildschirm starten _müssen_ enthalten nur die einfache, integrierte Benutzeroberflächenelemente und **kann nicht** Berechnungen ausführen oder eine benutzerdefinierte Klasse abgeleitet.

Weitere Informationen zum Erstellen eines Bildschirms starten mit einem Storyboard Unified finden Sie unter der [dynamische starten Bildschirme](~/ios/user-interface/storyboards/unified-storyboards.md#dynamic-launch-screens) Teil der [Unified Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md) Handbuch.

## <a name="migrating-to-launch-screen-storyboards"></a>Migrieren von Storyboards Bildschirm gestartet

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Bei der Aktualisierung einer vorhandenen app Verwendung des Storyboards für seine Bildschirme starten mit der rechten Maustaste klicken Sie auf die **Projektname** in der **Projektmappen-Explorer** , und wählen Sie **hinzufügen**  >  **Neue Datei...** . Wählen Sie **iOS** > **starten Bildschirm** , und klicken Sie auf die **neu** Schaltfläche:

![](launch-screens-images/storyboard02.png "Wählen Sie ein iOS-Bildschirm starten")

Doppelklicken Sie anschließend auf die `Info.plist` in der Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen. Klicken Sie unter **starten Bildschirm**, wählen Sie die neue Storyboard-Datei, die oben erstellte.

![](launch-screens-images/storyboard09.png "Wählen Sie die neue Storyboard-Datei, die oben erstellte")


Das neue Storyboard als einen Startbildschirm verwenden möchten, führen Sie folgende Schritte aus:

1. Doppelklicken Sie auf die `Info.plist` in der Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen.
2. Einen Bildlauf zu der **Universal starten Bilder** Abschnitt des Editors öffnen die **starten Bildschirm** Dropdownliste, und wählen Sie dann der Namen des Storyboards oben erstellten: 

    ![](launch-screens-images/storyboard08.png "Festlegen des Startbildschirms auf das storyboard")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Mit der rechten Maustaste auf den Projektnamen in der **Projektmappen-Explorer** , und wählen Sie **hinzufügen** > **neue Datei...** : 

    ![](launch-screens-images/image012.png "Neue Datei hinzufügen")
2. Geben Sie einen Namen für den Startbildschirm, und klicken Sie auf die **hinzufügen** Schaltfläche: 

    ![](launch-screens-images/image013.png "Geben Sie einen Namen für den Startbildschirm")
3. In der **Projektmappen-Explorer**, doppelklicken Sie auf die neu erstellte Storyboarddatei, um ihn zur Bearbeitung zu öffnen.
4. Sicherstellen, dass die **Größe Klasse** auf festgelegt ist **alle: alle** und die **Ansicht als** ist **generische**: 

    ![](launch-screens-images/image016.png "Stellen Sie sicher, dass die Größe-Klasse auf einen festgelegt ist: alle und die Sicht als generische")
5. Assembly Startbildschirms aus Größenklassen einfache Benutzeroberflächenelemente (z. B. `UIImageView`) und Bilder, die Sie in der Anwendung Paket aufgenommen haben: 

    ![](launch-screens-images/image017.png "Assembly der Startbildschirm im iOS-Designer")
6. Speichern Sie die Änderungen auf das Storyboard.

-----

## <a name="related-links"></a>Verwandte Links

- [Dynamische starten Bildschirme (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/)
- [Einheitliche Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md)
- [iOS-Designer-Grundlagen](~/ios/user-interface/designer/index.md)
- [Hinzufügen von Bildern zu einem Bild, Asset-Katalog festgelegt.](~/ios/app-fundamentals/images-icons/displaying-an-image.md#asset-catalogs)
- [Automatisches Layout mit dem Xamarin-Designer für iOS](~/ios/user-interface/designer/designer-auto-layout.md)
- [Human Interface-Richtlinien: Startbildschirm](https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/launch-screen/)
