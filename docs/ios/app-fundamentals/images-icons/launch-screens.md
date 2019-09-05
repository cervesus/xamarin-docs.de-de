---
title: Startbildschirme für xamarin. IOS-apps
description: In diesem Artikel wird erläutert, wie Sie einen app-Startbildschirm für alle IOS-Geräte in beliebiger Auflösung und Ausrichtung mithilfe eines einzelnen vereinheitlichten Storyboards erstellen.
ms.prod: xamarin
ms.assetid: 31A489CA-756B-4B9B-B386-4BADF18EDD33
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 05/02/2018
ms.openlocfilehash: d0d5452c2b79fb674e473efd50aaf587d64c4544
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290251"
---
# <a name="launch-screens-for-xamarinios-apps"></a>Startbildschirme für xamarin. IOS-apps

_In diesem Artikel wird erläutert, wie Sie einen app-Startbildschirm für alle IOS-Geräte in beliebiger Auflösung und Ausrichtung mithilfe eines einzelnen vereinheitlichten Storyboards erstellen._

Vor IOS 8 erforderte das Erstellen eines Startbildschirms für eine IOS-APP, dass der Entwickler für jeden der verschiedenen Geräte Formfaktoren und-Lösungen, in denen die app ausgeführt werden konnte, ein Image-Asset bereitstellt. Seit der Veröffentlichung von IOS 8 war es jedoch möglich, ein einzelnes einheitliches Storyboard zu verwenden, um einen Startbildschirm zu erstellen, der in allen Fällen richtig aussieht.

In dieser kurzen exemplarischen Vorgehensweise wird beschrieben, wie ein Startbildschirm entweder mit einem in einem neuen Projekt standardmäßig bereitgestellten Storyboard oder mit einem manuell hinzugefügten Storyboard in einem vorhandenen Projekt erstellt wird. Anschließend wird veranschaulicht, wie der IOS-Designer verwendet wird, um dem Storyboard eine Bildansicht und eine Bezeichnung hinzuzufügen, um Einschränkungen für diese Ansichten festzulegen und um zu überprüfen, ob das Storyboard für verschiedene Geräte und Ausrichtungen korrekt aussieht.

<a name="storyboard" />

## <a name="managing-launch-screens-with-storyboards"></a>Verwalten von Start Bildschirmen mit Storyboards

In ios 8 (und höher) kann der Entwickler ein spezielles einheitliches Storyboard erstellen, um den Startbildschirm anstelle eines oder mehrerer statischer Start Images bereitzustellen. Wenn Sie ein Launch-Storyboard im IOS-Designer erstellen, verwenden Sie Größenklassen und automatisches Layout, um verschiedene Layouts für verschiedene Anzeige Umgebungen zu definieren. Mithilfe von Größenklassen und automatischem Layout kann der Entwickler einen einzelnen Startbildschirm erstellen, der auf allen Geräten und Anzeige Umgebungen gut aussieht.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Erstellen Sie in Visual Studio für Mac ein neues Projekt, indem Sie **Datei > neue Projekt Mappe** und dann **Einzelansicht-App**auswählen: 

    ![Das Fenster "Neues Projekt" mit ausgewählter Einzelansicht-App](launch-screens-images/launch01.png)

    - Standardmäßig enthält ein neues Projekt eine **launchscreen. Storyboard** -Datei, die die Startbildschirm-Schnittstelle definiert. 
    - Wenn Sie einem vorhandenen Projekt stattdessen ein Storyboard für den Startbildschirm hinzufügen möchten, klicken Sie im **Lösungspad** mit der rechten Maustaste auf den Projektnamen, wählen Sie **> neue Datei hinzufügen** aus, und klicken Sie dann auf **Startbildschirm**:

    ![Das neue Datei Fenster mit ausgewähltem IOS-Startbildschirm](launch-screens-images/launch01b.png)

    - Nennen Sie die Datei " **launchscreen** " oder einen anderen Namen Ihrer Wahl.

2. Konfigurieren Sie das Projekt so, dass das entsprechende Storyboard für den Startbildschirm verwendet wird:

    - Doppelklicken Sie auf die **Info. plist** -Datei im **Lösungspad** , um Sie zur Bearbeitung zu öffnen.
    - Stellen Sie im Abschnitt **Start Images** sicher, dass **Startbildschirm** auf den Namen des entsprechenden Storyboards festgelegt ist:

    ![Die Startbildschirm Auswahl in "Info. plist"](launch-screens-images/launch02.png)

    - Standardmäßig ist ein neues Projekt so konfiguriert, dass **launchscreen. Storyboard** als Startbildschirm verwendet wird.

3. Fügen Sie dem **Assets. xcassets** -Asset-Katalog ein Bild hinzu, damit es für die Verwendung auf dem Startbildschirm verfügbar ist. Weitere Informationen finden Sie im Abschnitt [Hinzufügen von Bildern zu einem Asset-Katalog Image Satz](~/ios/app-fundamentals/images-icons/displaying-an-image.md) des Handbuchs [Anzeigen eines Bild](~/ios/app-fundamentals/images-icons/displaying-an-image.md) Handbuchs.

4. Öffnen Sie **launchscreen. Storyboard** zur Bearbeitung, indem Sie in der **Lösungspad**auf die Datei doppelklicken.

5. Wählen Sie ein Gerät und eine Ausrichtung aus, um eine Vorschau des Storyboards für den Startbildschirm im IOS-Designer anzuzeigen. Öffnen Sie den Bereich Geräteauswahl in der unteren Symbolleiste **, und wählen**Sie **iPhone 4S** und Hochformat aus.

    ![Die Geräteauswahl-Symbolleiste](launch-screens-images/launch05.png)

    - Beachten Sie, dass beim Auswählen eines Geräts und einer Ausrichtung nur die Vorschau des Entwurfs durch den IOS-Designer geändert wird. Unabhängig von der hier getroffenen Auswahl werden neu hinzugefügte Einschränkungen auf alle Geräte und Ausrichtungen angewendet, es sei denn, die Schaltfläche zum **Bearbeiten von Merkmalen** wurde verwendet, um anderweitig anzugeben. 

6. Legen Sie die **Hintergrund** Farbe der Hauptansicht des Ansichts Controllers fest. Wählen Sie die Ansicht aus, indem Sie in der Mitte des Ansichts Controllers klicken und die Hintergrundfarbe mit dem **Eigenschaftenpad**anpassen:

    ![Eine einzelne Ansicht mit einer lila Hintergrundfarbe](launch-screens-images/launch06.png)

7. Fügen Sie dem Startbildschirm eine **Bildansicht** hinzu, und legen Sie das zugehörige Quell **Image**fest:

    - Ziehen Sie eine **Bildansicht** aus dem **Werkzeugkasten der Toolbox** in den Mittelpunkt der Ansicht.
    - Wenn die **Bildansicht** ausgewählt ist, legen Sie im **Widget** -Abschnitt des **Eigenschaftenpad** die **Image** -Eigenschaft auf die Image-Eigenschaft fest, die dem **Assets. xcassets** -Asset-Katalog bereits hinzugefügt wurde. Positionieren Sie die **Bildansicht nach** Bedarf neu, und geben Sie Sie an:
    
    ![Eine Bildansicht mit fest gelegteter Bild Eigenschaft](launch-screens-images/launch07.png)

8. Fügen Sie unterhalb der **Bildansicht** eine **Bezeichnung** hinzu, und verwenden Sie die **Eigenschaftenpad** , um die zugehörigen Attribute festzulegen: 

    ![Eine Bezeichnung mit dem Text und dem Farbsatz](launch-screens-images/launch08.png)

9. Wechseln Sie zum Einschränkungs Bearbeitungsmodus, indem Sie die Rechte Schaltfläche in der **Einschränkungs Symbolleiste**verwenden:
    
    ![Schaltfläche zum Bearbeiten des Einschränkungs Modus](launch-screens-images/launch09.png)

10. Fügen Sie der **Bildansicht**Einschränkungen hinzu, legen Sie Ihre Höhe und Breite fest, und zentrieren Sie Sie horizontal und vertikal:

    ![Eine Bildansicht mit Layouteinschränkungen](launch-screens-images/launch10.png)

    - Weitere Informationen zum Hinzufügen von Einschränkungen finden Sie unter [Automatisches Layout mit dem Xamarin Designer für IOS](~/ios/user-interface/designer/designer-auto-layout.md).

11. Fügen Sie der **Bezeichnung**Einschränkungen hinzu, zentrieren Sie Sie horizontal, geben Sie eine Höhe und Breite an, und positionieren Sie sie vertikal aus der **Bildansicht**:

    ![Eine Bezeichnung mit Layouteinschränkungen](launch-screens-images/launch11.png)

12. Testen Sie andere Geräte und Ausrichtungen, um zu überprüfen, ob der Entwurf in allen Szenarien als beabsichtigt aussieht. In Fällen, in denen Anpassungen für ein bestimmtes Gerät oder eine bestimmte Ausrichtung vorgenommen werden müssen, verwenden Sie die Schaltfläche zum **Bearbeiten von Merkmalen** , um für bestimmte Größenklassen eine Verbindung hinzuzufügen:

    ![Der als iPhone X gerenderte Startbildschirm mithilfe der quer Ausrichtung](launch-screens-images/launch12.png)

13. Speichern Sie die Änderungen am Storyboard. Führen Sie die APP auf einem Simulator oder Gerät aus, und der Startbildschirm wird angezeigt, wenn die APP gestartet wird.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Erstellen Sie ein neues Projekt. Wählen Sie in Visual Studio **Datei > Neues > Projekt > Visual C# > iPhone & iPad > IOS-app (xamarin)** aus:

    ![Das Fenster "Neues Projekt" mit ausgewählter IOS-app (xamarin)](launch-screens-images/launch01.w157.png)

    Wählen Sie die Vorlage **Einzelansicht-App** aus, und klicken Sie dann auf **OK**:

    ![Einzelansicht-App-Vorlage](launch-screens-images/launch01-2.w157.png)

2. Wenn **Ressourcen > launchscreen. XIb** im **Projektmappen-Explorer**vorhanden sind, löschen Sie Sie, indem Sie mit der rechten Maustaste auf die Datei klicken und **Löschen**auswählen. Diese Datei wird im nächsten Schritt durch ein Storyboard ersetzt.

3. Erstellen Sie ein Storyboard, das als Startbildschirm verwendet werden soll. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **> Neues Element hinzufügen...** gefolgt von **leerem Storyboard**. Benennen Sie das Storyboard **launchscreen. Storyboard** , und klicken Sie auf **Hinzufügen**:

    ![Fenster "Neues Element hinzufügen", bei dem leeres Storyboard ausgewählt ist](launch-screens-images/launch03.w157.png)

4. Konfigurieren Sie das Projekt so, dass **launchscreen. Storyboard** als Startbildschirm-Storyboard verwendet wird:

    - Doppelklicken Sie auf die **info.plist**-Datei im **Projektmappen-Explorer**, um sie zu öffnen und zu bearbeiten. 
    - Legen Sie auf der Registerkarte **visuelle Objekte** **Startbildschirm** auf **launchscreen**fest.

    ![Die Startbildschirm Auswahl in "Info. plist"](launch-screens-images/launch04-vs.png)

5. Fügen Sie einem Asset-Katalog im Projekt ein Bild hinzu, sodass es für die Verwendung auf dem Startbildschirm verfügbar ist:

    - Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf **Asset-Kataloge** , und wählen Sie Ressourcen **Katalog hinzufügen**aus. Benennen Sie die neuen Asset Catalog- **Assets**:

    ![Fenster "Neues Element hinzufügen" mit ausgewähltem Asset Catalog](launch-screens-images/launch05.w157.png)

    - Fügen Sie dem Asset-Katalog **Assets** neue Bilder hinzu. Informationen hierzu finden Sie im Abschnitt [Hinzufügen von Bildern zu Bildern des Asset-Katalogs](~/ios/app-fundamentals/images-icons/displaying-an-image.md) des Leifadens [Anzeigen eines Bilds](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

6. Öffnen Sie **launchscreen. Storyboard** zur Bearbeitung, indem Sie in der **Projektmappen-Explorer**auf die Datei doppelklicken.

    - Um eine storyboarddatei zu bearbeiten, benötigt Visual Studio eine aktive Verbindung mit einem Mac-buildhost. Weitere Informationen finden Sie im Leitfaden zum [Herstellen einer Verbindung mit dem Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) .

7. Wählen Sie ein Gerät und eine Ausrichtung aus, um eine Vorschau des Storyboards für den Startbildschirm im IOS-Designer anzuzeigen. Öffnen Sie den Bereich Geräteauswahl auf der unteren Symbolleiste **, und wählen**Sie **iPhone 4S** und Hochformat aus: 
 
    ![Die Geräteauswahl-Symbolleiste](launch-screens-images/launch07-vs.png)

    - Beachten Sie, dass beim Auswählen eines Geräts und einer Ausrichtung nur die Vorschau des Entwurfs durch den IOS-Designer geändert wird. Unabhängig von der hier getroffenen Auswahl werden neu hinzugefügte Einschränkungen auf alle Geräte und Ausrichtungen angewendet, es sei denn, die Schaltfläche zum **Bearbeiten von Merkmalen** wurde verwendet, um anderweitig anzugeben. 

8. Fügen Sie dem Storyboard einen Ansichts Controller hinzu, indem Sie einen **Ansichts Controller** aus der **Toolbox** auf die Entwurfs Oberfläche ziehen: 

    ![Ein leerer Ansichts Controller, der der Entwurfs Oberfläche hinzugefügt wurde.](launch-screens-images/launch08-vs.png)

9. Legen Sie die **Hintergrund** Farbe der Hauptansicht des Ansichts Controllers fest. Wählen Sie die Ansicht aus, indem Sie in der Mitte des Ansichts Controllers klicken und die Hintergrundfarbe mithilfe des **Fensters Eigenschaften**anpassen:
    
    ![Eine einzelne Ansicht mit einer lila Hintergrundfarbe](launch-screens-images/launch09-vs.png)

10. Fügen Sie dem Startbildschirm eine **Bildansicht** hinzu, und legen Sie das zugehörige Quell **Image**fest:

    - Ziehen Sie eine **Bildansicht** aus der **Toolbox** in die Mitte der Ansicht.
    - Wenn die **Bildansicht** noch ausgewählt ist, legen Sie im Abschnitt **Widget** des **Fensters Eigenschaften** die Eigenschaft **Image** auf den Image Satz fest, der dem Asset- **Ressourcen** Katalog bereits hinzugefügt wurde. Positionieren Sie die **Bildansicht nach** Bedarf neu, und geben Sie Sie an:
    
    ![Eine Bildansicht mit fest gelegteter Bild Eigenschaft](launch-screens-images/launch10-vs.png)

11. Fügen Sie unterhalb der **Bildansicht**eine **Bezeichnung** hinzu:

    - Ziehen Sie eine **Bezeichnung** aus der **Toolbox** auf die Entwurfs Oberfläche, und platzieren Sie Sie unterhalb der **Bildansicht**.
    - Legen Sie die Attribute für die **Bezeichnung** mithilfe des **Fensters Eigenschaften**fest:

    ![Eine Bezeichnung mit dem Text und dem Farbsatz](launch-screens-images/launch11-vs.png) 

12. Wechseln Sie zum Einschränkungs Bearbeitungsmodus, indem Sie die Rechte Schaltfläche in der **Einschränkungs Symbolleiste**verwenden:
    
    ![Schaltfläche zum Bearbeiten des Einschränkungs Modus](launch-screens-images/launch12-vs.png) 

13. Fügen Sie der **Bildansicht**Einschränkungen hinzu, legen Sie Ihre Höhe und Breite fest, und zentrieren Sie Sie horizontal und vertikal:

    ![Eine Bildansicht mit Layouteinschränkungen](launch-screens-images/launch13-vs.png) 

    - Weitere Informationen zum Hinzufügen von Einschränkungen finden Sie unter [Automatisches Layout mit dem Xamarin Designer für IOS](~/ios/user-interface/designer/designer-auto-layout.md).

14. Fügen Sie der **Bezeichnung**Einschränkungen hinzu, zentrieren Sie Sie horizontal, geben Sie eine Höhe und Breite an, und positionieren Sie sie vertikal aus der **Bildansicht**:
    
    ![Eine Bezeichnung mit Layouteinschränkungen](launch-screens-images/launch14-vs.png) 

15. Testen Sie andere Geräte und Ausrichtungen, um zu überprüfen, ob der Entwurf in allen Szenarien als beabsichtigt aussieht. In Fällen, in denen Anpassungen für ein bestimmtes Gerät oder eine bestimmte Ausrichtung vorgenommen werden müssen, verwenden Sie die Schaltfläche zum **Bearbeiten von Merkmalen** , um für bestimmte Größenklassen eine Verbindung hinzuzufügen:

    ![Der als iPhone X gerenderte Startbildschirm mithilfe der quer Ausrichtung](launch-screens-images/launch15-vs.png) 

16. Speichern Sie die Änderungen am Storyboard. Führen Sie die APP auf einem Simulator oder Gerät aus, und der Startbildschirm wird angezeigt, wenn die APP gestartet wird.

-----

> [!NOTE]
> Ein als Startbildschirm verwendetes Storyboard _darf_ nur einfache, integrierte Benutzeroberflächen Elemente enthalten und **kann** keine Berechnungen ausführen oder von einer benutzerdefinierten Klasse abgeleitet werden.

Weitere Informationen zum Erstellen eines Startbildschirms mit einem einheitlichen Storyboard finden Sie im Abschnitt [dynamische Startbildschirme](~/ios/user-interface/storyboards/unified-storyboards.md#dynamic-launch-screens) des Themas [Unified Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md) .

## <a name="migrating-to-launch-screen-storyboards"></a>Migrieren zu Startbildschirm-Storyboards

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Wenn Sie eine vorhandene App für die Verwendung von Storyboards für die Startbildschirme aktualisieren, klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den **Projektnamen** , und wählen Sie**neue Datei** **Hinzufügen** > aus. Wählen Sie **IOS** > -**Startbildschirm** aus, und klicken Sie auf **neu** :

![](launch-screens-images/storyboard02.png "IOS-Startbildschirm auswählen")

Doppelklicken Sie dann auf die `Info.plist` Datei im **Projektmappen-Explorer** , um Sie zur Bearbeitung zu öffnen. Wählen Sie unter **Startbildschirm**die neue storyboarddatei aus, die oben erstellt wurde.

![](launch-screens-images/storyboard09.png "Wählen Sie die oben erstellte neue storyboarddatei aus.")


Gehen Sie folgendermaßen vor, um das neue Storyboard als Startbildschirm zu verwenden:

1. Doppelklicken Sie auf `Info.plist` die Datei im **Projektmappen-Explorer** , um Sie für die Bearbeitung zu öffnen.
2. Scrollen Sie zum Abschnitt **universelle Start Bilder** des Editors, öffnen Sie die Dropdown Liste **Startbildschirm** , und wählen Sie den Namen des oben erstellten Storyboards aus: 

    ![](launch-screens-images/storyboard08.png "Festlegen des Startbildschirms auf das Storyboard")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektnamen, und wählen Sie**neue Datei** **Hinzufügen** > ... aus: 

    ![](launch-screens-images/image012.png "Neue Datei hinzufügen")
2. Geben Sie einen Namen für den Startbildschirm ein, und klicken Sie auf **Hinzufügen** : 

    ![](launch-screens-images/image013.png "Geben Sie einen Namen für den Startbildschirm ein.")
3. Doppelklicken Sie im **Projektmappen-Explorer**auf die neu erstellte storyboarddatei, um Sie zur Bearbeitung zu öffnen.
4. Stellen Sie sicher, dass die **size-Klasse** auf **any** festgelegt ist und die **Sicht** **generisch**ist: 

    ![](launch-screens-images/image016.png "Stellen Sie sicher, dass die Size-Klasse auf Any festgelegt ist, und die Sicht ist generisch.")
5. Assembly der Startbildschirm von Größenklassen, einfache Benutzeroberflächen Elemente (z `UIImageView`. b.) und Bilder, die Sie im Paket der Anwendung enthalten haben: 

    ![](launch-screens-images/image017.png "Assembly der Startbildschirm im IOS-Designer")
6. Speichern Sie die Änderungen am Storyboard.

-----

## <a name="related-links"></a>Verwandte Links

- [Dynamische Startbildschirme (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-dynamiclaunchscreen)
- [Einheitliche Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md)
- [iOS-Designer-Grundlagen](~/ios/user-interface/designer/index.md)
- [Hinzufügen von Bildern zu einem Asset Catalog-Image Satz](~/ios/app-fundamentals/images-icons/displaying-an-image.md#adding-images-to-an-asset-catalog-image-set)
- [Automatisches Layout mit dem Xamarin Designer für IOS](~/ios/user-interface/designer/designer-auto-layout.md)
- [Richtlinien für die Benutzeroberfläche: Startbildschirm](https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/launch-screen/)
