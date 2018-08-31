---
title: Mithilfe des Tools MonoGame-Pipeline
description: Das Pipelinetool MonoGame dient zum Erstellen und Verwalten von Projekten mit MonoGame Inhalten. Die Dateien in Projekten mit Inhalten vom Tool MonoGame-Pipeline verarbeitet und als .xnb-Dateien für die Verwendung in Anwendungen von CocosSharp "und" MonoGame "ausgegeben.
ms.prod: xamarin
ms.assetid: CACFBF5F-BBD4-4D46-8DDA-1F46466725FD
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 347cb7e9d417f97cb6e8d78e67b1c76a378cd188
ms.sourcegitcommit: 7ffbecf4a44c204a3fce2a7fb6a3f815ac6ffa21
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "34783290"
---
# <a name="using-the-monogame-pipeline-tool"></a>Mithilfe des Tools MonoGame-Pipeline

_Das Pipelinetool MonoGame dient zum Erstellen und Verwalten von Projekten mit MonoGame Inhalten. Die Dateien in Projekten mit Inhalten vom Tool MonoGame-Pipeline verarbeitet und als .xnb-Dateien für die Verwendung in Anwendungen von CocosSharp "und" MonoGame "ausgegeben._

Das MonoGame-Pipeline-Tool bietet eine einfache zu bedienende Umgebung für die Konvertierung von Inhaltsdateien in **.xnb** Dateien zur Verwendung in CocosSharp "und" MonoGame "-Anwendungen. Weitere Informationen zu inhaltspipelines und warum sie in der Entwicklung von Spielen nützlich sind, finden Sie unter [dieser Einführung in inhaltspipelines](~/graphics-games/cocossharp/content-pipeline/introduction.md)

In dieser exemplarischen Vorgehensweise wird Folgendes behandelt:

 - Installieren das Tool MonoGame-Pipeline
 - Erstellen eines CocosSharp-Projekts
 - Erstellen ein Inhaltsprojekt.
 - Verarbeiten von Dateien in das Pipelinetool MonoGame
 - Mithilfe von Dateien zur Laufzeit

Diese exemplarische Vorgehensweise ein CocosSharp-Projekts verwendet, um zu veranschaulichen wie **.xnb** Dateien geladen und in einer Anwendung verwendet werden können. Benutzer von MonoGame werden auch in dieser exemplarischen Vorgehensweise zu verweisen, wie CocosSharp und MonoGame dasselbe verwenden **.xnb** Inhaltsdateien.

Die fertige app zeigt ein einzelnes Sprite, das Anzeigen einer Textur aus einer **.xnb** -Datei und einer einzelnen Bezeichnung eine Sprite-Schriftart aus einem **.xnb** Datei:

![](walkthrough-images/image1.png "Die fertige app wird ein einzelnes Sprite, das Anzeigen einer Textur aus einer Datei .xnb angezeigt.")


## <a name="monogame-pipeline-tool-discussion"></a>MonoGame Pipelinetool Diskussion

Das Tool der MonoGame-Pipeline ist unter Windows, OS X und Linux verfügbar. In dieser exemplarischen Vorgehensweise wird das Tool für Windows ausgeführt, aber er kann folgen zusammen unter Mac und Linux ebenfalls. Informationen zum Abrufen der Tools auf maximale oder Linux einrichten können, finden Sie unter [auf dieser Seite](http://www.monogame.net/2015/01/09/monogame-pipeline-tool-available-for-macos-and-linux/).

Das Pipelinetool MonoGame kann Inhalte erstellen für iOS-Anwendungen auch auf Windows, also Entwickler, die mit Ausführung von [Xamarin Mac Agent](~/ios/get-started/installation/windows/connecting-to-mac/index.md) weiter auf Windows entwickeln können.


## <a name="installing-the-monogame-pipeline-tool"></a>Installieren das Tool MonoGame-Pipeline

Wir zunächst die MonoGame, einschließlich der Pipeline für Bildinhalte MonoGame installieren. Beachten Sie, dass die Pipeline für Bildinhalte MonoGame separater Download für Mac. Alle MonoGame-Installationsprogramme befinden sich die [MonoGame-Downloadseite](http://www.monogame.net/downloads/). Wir werden herunterladen MonoGame für Visual Studio, sondern nach der Installation des Entwicklers können MonoGame in Visual Studio für Mac zu:

![](walkthrough-images/image2.png "Herunterladen von MonoGame für Visual Studio, aber sobald die entwicklereinsatz MonoGame in Visual Studio für Mac zu installiert")

Nach dem Herunterladen, wir führen Sie das Installationsprogramm und akzeptieren die Standardoptionen. Nachdem das Installationsprogramm abgeschlossen ist, wird das Pipelinetool MonoGame installiert ist, und finden Sie in der Start-Menü-Suche:

![](walkthrough-images/image3.png "Nachdem das Installationsprogramm abgeschlossen ist, das Pipelinetool MonoGame installiert ist, und finden Sie in der Start-Menü-Suche")

Starten Sie das Tool MonoGame-Pipeline:

![](walkthrough-images/image4.png "Starten Sie das Tool MonoGame-Pipeline")

Sobald das Pipelinetool MonoGame ausgeführt wird, können wir unserer Projekte Spiele und Inhalt zu starten.


## <a name="creating-an-empty-cocossharp-project"></a>Erstellen eines leeren CocosSharp-Projekts

Der nächste Schritt ist die Erstellung ein CocosSharp-Projekts. Es ist wichtig, dass die CocosSharp-Projekts erstellen wir zunächst, damit wir unsere Content-Projekt in der Ordnerstruktur, die von der CocosSharp-Projekts erstellt speichern können. Um die Struktur eines CocosSharp-Projekts zu verstehen, sehen Sie sich die [BouncingGame](~/graphics-games/cocossharp/bouncing-game.md), die in diesem Leitfaden verwenden werden. Jedoch wenn Sie ein vorhandenes CocosSharp-Projekt, dem Sie Inhalt hinzufügen möchten verfügen, können dieses Projekt anstelle von BouncingGame verwenden.

Nachdem das Projekt erstellt wurde, führen wir sie, um sicherzustellen, dass er erstellt und haben wir alles ordnungsgemäß eingerichtet:

![](walkthrough-images/image5.png "Nachdem das Projekt erstellt wurde, führen Sie es, stellen Sie sicher, dass sie erstellt und, dass alles ordnungsgemäß eingerichtet")


## <a name="creating-a-content-project"></a>Erstellen ein Inhaltsprojekt.

Nun, da wir ein Spiele-Projekt haben, können wir ein MonoGame-Pipeline-Projekt erstellen. Dazu in der Select MonoGame Pipelinetool **Datei > neu...**  und navigieren Sie zum Ordner "Content" Ihres Projekts. Für Android, befindet sich der Ordner unter **[Projekt root]\BouncingGame.Android\Assets\Content\**. Für iOS, befindet sich der Ordner unter **[Projekt root]\BouncingGame.iOS\Content\**.

Ändern der **Dateiname** zu **ContentProject** , und klicken Sie auf die **speichern** Schaltfläche:

![](walkthrough-images/image8.png "Ändern Sie den Dateinamen in ContentProject, und klicken Sie auf die Schaltfläche \"Speichern\"")

Nachdem das Projekt erstellt wurde, zeigt das Pipelinetool MonoGame Informationen über das Projekt beim Stamm **ContentProject** Element ausgewählt ist:

![](walkthrough-images/image9.png "Nachdem das Projekt erstellt wurde, zeigt MonoGame Pipeline das Tool Informationen über das Projekt, wenn das Element der Stamm-ContentProject ausgewählt ist")

Betrachten wir nun einige der wichtigsten Optionen für das Content-Projekt.


### <a name="output-folder"></a>Ordner "Output"

Dies ist der Ordner (in Bezug auf das Content-Projekt selbst), in dem die Ausgabe **.xnb** Dateien gespeichert werden. Um Sie der Einfachheit halber verwenden wir den gleichen Ordner unsere Eingabe und Ausgabe von Dateien. Anders ausgedrückt: wir ändern die **Ausgabeordner** sollen **.\**  :

![](walkthrough-images/image10.png "")


### <a name="platform"></a>Plattform

Dadurch wird die Zielplattform für den Inhalt definiert. Beachten Sie, dass dies **Windows** standardmäßig, daher sollten wir dies unsere Zielplattform ändern, das wird **Android** (oder iOS Wenn zusammen mit einem iOS-Projekt).

![](walkthrough-images/image11.png "Beachten Sie, dass dies standardmäßig Windows ist, so ändern Sie die Zielplattform die Android- oder iOS ist, wenn zusammen mit einem iOS-Projekt")


## <a name="processing-files-in-the-monogame-pipeline-tool"></a>Verarbeiten von Dateien in das Pipelinetool MonoGame

Als Nächstes, wir hinzufügen Inhalte unserer **ContentProject**. Wir werden Sie in das Stammverzeichnis des Projekts werden Dateien hinzufügen, aber größere Projekte werden in der Regel ihre Inhalte in Ordnern organisieren, für dieses Projekt.

Wir werden zwei Dateien Ihrem Projekt hinzuzufügen:

 - Ein **PNG** Datei, die verwendet werden wird, um ein Sprite zeichnen. Diese Datei kann [hier zum Download](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/ball.png?raw=true).
 - Ein **.spritefont** Datei, die zum Zeichnen des Texts auf dem Bildschirm verwendet wird. Das Content Pipeline-Tool unterstützt neue .spritefont Dateien erstellen, damit es keine Datei zum Herunterladen ist.


### <a name="adding-a-png-file"></a>Eine PNG-Datei hinzufügen

Hinzufügen einer **PNG** Datei zum Projekt, wir werden kopieren Sie es zunächst in dasselbe Verzeichnis wie das pipelineprojekt, mit der **.mgcb** Erweiterung.

![](walkthrough-images/image12.png "Fügen Sie dem Projekt eine PNG-Datei")

Als Nächstes fügen wir die Datei dem Projekt für die Pipeline. Wählen Sie hierzu im Tool Pipeline MonoGame **Bearbeiten > Element hinzufügen...** , wählen die **ball.png** Datei, und klicken Sie auf **öffnen**. Die Datei ist nun Teil der Content-Projekt und, bei Auswahl dieser Option werden die Eigenschaften angezeigt:

![](walkthrough-images/image13.png "Die Datei ist nun Teil der Content-Projekt und, bei Auswahl dieser Option werden die Eigenschaften angezeigt")

Wir werden alle Werte die Standardwerte verlassen, da keine Änderungen beim Laden der Datei .xnb in CocosSharp erforderlich sind. Wir können die Datei erstellen, durch Auswahl der **erstellen > Erstellen** Menüoption, starten Sie einen Build und die Ausgabe über den Build angezeigt. Wir können überprüfen, ob der Build ordnungsgemäß anhand funktioniert die **Content** Ordner für einen neuen **ball.xnb** Datei:

![](walkthrough-images/image14.png "Stellen Sie sicher, dass der Build ordnungsgemäß funktioniert, indem Sie den Inhaltsordner für eine neue ball.xnb-Datei überprüfen")


### <a name="adding-a-spritefont-file"></a>Hinzufügen einer Datei .spritefont

Wir können .spritefont-Datei über das Tool der MonoGame-Pipeline erstellen. CocosSharp erfordert Schriftarten in einer **Schriftarten** Ordner und CocosSharp-Vorlagen, die automatisch einen Schriftartenordner automatisch erstellt. Wir können das Pipelinetool MonoGame in diesem Ordner hinzugefügt, dazu **Bearbeiten > Hinzufügen > vorhandenen Ordner...** . Navigieren Sie zu der **Content** Ordner, und wählen die **Schriftarten** Ordner, und klicken Sie auf **OK**:

![](walkthrough-images/browsetofonts.png "Navigieren Sie zu dem Inhaltsordner ein, und wählen Sie den Ordner für die Schriftarten, und klicken Sie auf OK")

Um eine neue .sprintefont Datei hinzuzufügen, mit der rechten Maustaste auf den Ordner Schriftarten, und wählen Sie **hinzufügen > Neues Element...**, wählen die **SpriteFont Beschreibung** aus, geben Sie den Namen **arial 36**, und klicken Sie auf **Ok**. CocosSharp erfordert spezifische Benennung von Schriftartdateien – sie muss im Format [FontType]-[FontSize]. Wenn eine Schriftart dieses Namensformat nicht übereinstimmt, wird es nicht von CocosSharp zur Laufzeit geladen werden.

![](walkthrough-images/image15.png "Wenn eine Schriftart nicht dieses Namensformat übereinstimmt, die sie nicht von CocosSharp zur Laufzeit geladen werden")

Die Datei .spritefont ist tatsächlich eine XML-Datei die in einem Text-Editor, einschließlich Visual Studio für Mac bearbeitet werden kann Die am häufigsten verwendeten Variablen in einer Datei .spritefont bearbeitet die `FontName` und `Size` Eigenschaft:


```xml
    <!-- Modify this string to change the font that will be imported. -->
    <FontName>Arial</FontName>

    <!-- Size is a float value, measured in points. 
    Modify this value to change the size of the font. -->
    <Size>12</Size> 
```

Es wird die Datei in einem Text-Editor geöffnet. Als unsere **arial 36.spritefont** Name schon sagt, lassen wir die `FontName` als `Arial` jedoch ändern der `Size` Wert `36`:


```xml
    <!-- Modify this string to change the font that will be imported. -->
    <FontName>Arial</FontName>   
  
    <!-- Size is a float value, measured in points. 
    Modify this value to change the size of the font. -->4/10/2016 12:57:28 PM 
    <Size>36</Size>
```
 
## <a name="using-files-at-runtime"></a>Mithilfe von Dateien zur Laufzeit

Die .xnb-Dateien werden jetzt erstellt und in unserem Projekt verwendet werden können. Wir Hinzufügen der Dateien für Visual Studio für Mac, und klicken Sie dann fügen wir Code, um unsere `GameScene.cs` Datei laden diese Dateien und anzeigen.


### <a name="adding-xnb-files-to-visual-studio-for-mac"></a>Hinzufügen von .xnb-Dateien für Visual Studio für Mac

Zunächst fügen wir die Dateien zu Ihrem Projekt. In Visual Studio für Mac erweitern wir die **BouncingGame.Android** projizieren, erweitern Sie die **Assets** Ordner mit der rechten Maustaste auf die **Content** Ordner, und wählen Sie **Hinzufügen > Dateien hinzufügen...** Zunächst wählen wir die **ball.xnb** wir zuvor erstellt, und klicken Sie auf **öffnen**. Klicken Sie dann die oben genannten Schritte wiederholen, aber das Hinzufügen der **arial 36.xnb** Datei. Ich wähle die **behalten Sie die Datei in der aktuellen Unterverzeichnis** option, wenn Visual Studio für Mac die Vorgehensweise zum Hinzufügen der Datei aufgefordert werden. Nach Abschluss des Vorgangs beide Dateien muss Teil unseres Projekts:

![](walkthrough-images/image20.png "Nach Abschluss des Vorgangs beide Dateien muss Teil des Projekts")


### <a name="adding-gamescenecs"></a>Hinzufügen von **GameScene.cs**

Wir erstellen eine Klasse namens `GameScene,` unsere Objekte Sprite und den Text enthält. Zu diesem Zweck mit der Maustaste auf die **BouncingGame** (nicht BouncingGame.Android) Projekt, und wählen Sie **hinzufügen > neuen Datei...**. Wählen Sie die **allgemeine** Kategorie der **leere Klasse** aus, und geben Sie den Namen **GameScene**.

Nach der Erstellung ändern wir die `GameScene.cs` -Datei den folgenden Code:


```csharp
using System;
using CocosSharp;

namespace BouncingGame
{
    public class GameScene : CCScene
    {
        // All visual elements must be added to a CCLayer:
        CCLayer mainLayer;

        // The CCSprite is used to display the "ball" texture
        CCSprite sprite;
        // The CCLabelTtf is used to display the Arial36 sprite font
        CCLabelTtf label;

        public GameScene(CCWindow mainWindow) : base(mainWindow)
        {
            // Instantiate the CCLayer first:
            mainLayer = new CCLayer ();
            AddChild (mainLayer);

            // Now we can create the Sprite using the ball.xnb file:
            sprite = new CCSprite ("ball");
            sprite.PositionX = 200;
            sprite.PositionY = 200;
            mainLayer.AddChild (sprite);

            // The font name (arial) and size (36) need to match 
            // the .spritefont definition and file name.  
            label = new CCLabelTtf ("Using font 36", "arial", 36);
            label.PositionX = 200;
            label.PositionY = 300;
            mainLayer.AddChild (label);
        }
    }
} 
```

Es wird nicht näher auf den obige Code seit der Arbeit mit CocosSharp visuelle Objekte wie CCSprite und CCLabelTtf abgedeckt wird, in der [BouncingGame Handbuch](~/graphics-games/cocossharp/bouncing-game.md).

Wir müssen auch Code hinzufügen, um unseren neu erstellten laden `GameScene`. Hierzu wir öffnen die `GameAppDelegate.cs` Datei (befindet sich in der **BouncingGame** PCL), und Ändern der `ApplicationDidFinishLaunching` Methode, sodass es aussieht:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    application.PreferMultiSampling = false;
    application.ContentRootDirectory = "Content";

    // New code:
    GameScene gameScene = new GameScene (mainWindow);
    mainWindow.RunWithScene (gameScene);
} 
```

Bei der Ausführung sieht unser Spiel wie aus:

![](walkthrough-images/image1.png "Bei der Ausführung sieht des Spiels so aus wie")


## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise wurde gezeigt, wie Sie mit, dass das Pipelinetool MonoGame um .xnb Dateien aus einer Eingabe PNG-Datei zu erstellen sowie wie Sie eine neue .xnb-Datei aus einer neu erstellten .sprintefont-Datei zu erstellen. Es wurde auch erläutert, wie CocosSharp-Projekte zur Verwendung von .xnb Dateien strukturiert und wie Sie diese Dateien zur Laufzeit zu laden.

## <a name="related-links"></a>Verwandte Links

- [MonoGame-Downloads](http://www.monogame.net/downloads/)
- [MonoGame-Pipeline-Dokumentation](http://www.monogame.net/documentation/?page=Pipeline)
- [BouncingGame-Projekt für Android (Beispiel) starten](https://developer.xamarin.com/samples/BouncingGameCompleteAndroid/)
- [ball.png Graphic (sample)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/ball.png?raw=true)
