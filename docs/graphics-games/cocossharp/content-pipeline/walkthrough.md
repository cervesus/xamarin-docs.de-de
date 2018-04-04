---
title: Verwenden die MonoGame PipelineTool
description: Das Pipelinetool MonoGame dient zum Erstellen und Verwalten von Inhalt MonoGame-Projekte. Die Dateien in Projekten auf Inhalt vom Tool Monogame Pipeline verarbeitet und als .xnb-Dateien zur Verwendung in CocosSharp und MonoGame Anwendungen ausgegeben.
ms.prod: xamarin
ms.assetid: CACFBF5F-BBD4-4D46-8DDA-1F46466725FD
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 672c57aded149200b32501a6b48165ca88726ee1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="using-the-monogame-pipeline-tool"></a>Mithilfe des Tools MonoGame-Pipeline

_Das Pipelinetool MonoGame dient zum Erstellen und Verwalten von Inhalt MonoGame-Projekte. Die Dateien in Projekten auf Inhalt vom Tool Monogame Pipeline verarbeitet und als .xnb-Dateien zur Verwendung in CocosSharp und MonoGame Anwendungen ausgegeben._

Das Pipelinetool MonoGame stellt eine einfach zu bedienenden-Umgebung für die Konvertierung der Inhaltsdateien in **.xnb** Dateien zur Verwendung in CocosSharp und MonoGame-Anwendungen. Informationen zum Inhalt Pipelines und warum sie 3D-Spielentwicklung nützlich sind, finden Sie unter [dieser Einführung in das Content Pipelines](~/graphics-games/cocossharp/content-pipeline/introduction.md)

In dieser exemplarischen Vorgehensweise wird Folgendes behandelt:

 - Installieren das Tool MonoGame Pipeline
 - Erstellen eines Projekts CocosSharp
 - Erstellen einen Content-Projekt
 - Verarbeiten von Dateien in das Pipelinetool MonoGame
 - Verwenden von Dateien zur Laufzeit

In dieser exemplarischen Vorgehensweise mithilfe ein Projekts CocosSharp veranschaulicht, wie **.xnb** Dateien geladen und in einer Anwendung verwendet werden können. Benutzer von MonoGame werden auch in dieser exemplarischen Vorgehensweise verwiesen werden, da CocosSharp und MonoGame die gleiche verwenden **.xnb** Inhaltsdateien.

Die fertige app zeigt eine einzelne Sprite Anzeigen einer Textur aus einer **.xnb** Datei und einer einzelnen Beschriftung anzeigen einer Sprite Schriftart aus einer **.xnb** Datei:

![](walkthrough-images/image1.png "Die fertige app zeigt eine einzelne Sprite eine Textur aus einer Datei .xnb anzeigen")


## <a name="monogame-pipeline-tool-discussion"></a>MonoGame Pipelinetool Diskussion

Das MonoGame Pipeline-Tool steht auf OS X, Windows und Linux zur Verfügung. In dieser exemplarischen Vorgehensweise wird das Tool unter Windows ausgeführt, aber es gefolgt werden zusammen unter Mac und Linux auch. Informationen zum Abrufen des Tools unter Max oder Linux einrichten, finden Sie unter [auf dieser Seite](http://www.monogame.net/2015/01/09/monogame-pipeline-tool-available-for-macos-and-linux/).

Das Pipelinetool MonoGame ist in der Lage, um Inhalte zu erstellen, bei Wenn auch unter Windows ist dies der Fall ist Entwickler, die mit iOS-Anwendungen führen [Xamarin Mac Agent](~/ios/get-started/installation/windows/connecting-to-mac/index.md) wird in der Lage, die Entwicklung unter Windows fortsetzen.


## <a name="installing-the-monogame-pipeline-tool"></a>Installieren das Tool MonoGame Pipeline

Beginnen wir damit, durch die Installation der MonoGame, einschließlich der Inhalte MonoGame-Pipeline. Beachten Sie, dass der Inhalt MonoGame-Pipeline ein separater Download für Mac. Alle MonoGame Installationsprogramme befinden sich auf die [MonoGame-Downloadseite](http://www.monogame.net/downloads/). Wir heruntergeladene MonoGame für Visual Studio, aber nach der Installation des Entwicklers können MonoGame in Visual Studio für Mac zu:

![](walkthrough-images/image2.png "Herunterladen von MonoGame für Visual Studio, aber nachdem die Developer-Verwendung MonoGame in Visual Studio für Mac zu installiert")

Nach dem Herunterladen, wir durch das Installationsprogramm ausführen und akzeptieren die Standardoptionen. Nach Abschluss der Installation das Pipelinetool MonoGame installiert ist, und suchen Sie im Menü Start befinden:

![](walkthrough-images/image3.png "Nach Abschluss der Installation das Pipelinetool MonoGame ist installiert, und suchen Sie im Menü Start finden")

Starten Sie das Tool MonoGame-Pipeline:

![](walkthrough-images/image4.png "Starten Sie das Tool MonoGame-Pipeline")

Sobald das Pipelinetool MonoGame ausgeführt wird, können wir unsere Spiel und Content-Projekte zu starten.


## <a name="creating-an-empty-cocossharp-project"></a>Erstellen eines leeren CocosSharp-Projekts

Der nächste Schritt besteht darin ein CocosSharp-Projekt zu erstellen. Es ist wichtig, dass wir CocosSharp zunächst erstellen Sie das Projekt, damit wir unsere Content-Projekt in der Ordnerstruktur, die vom Projekt CocosSharp erstellt speichern können. Um die Struktur eines Projekts CocosSharp zu verstehen, sehen Sie sich die [BouncingGame](~/graphics-games/cocossharp/bouncing-game.md), die in diesem Handbuch verwendet wird. Jedoch wenn Sie ein vorhandenes CocosSharp-Projekt, dem Sie Inhalt hinzufügen möchten verfügen, können Sie das Projekt anstelle von BouncingGame verwenden.

Nachdem das Projekt erstellt wurde, müssen wir ausführen, um sicherzustellen, dass es erstellt und haben wir alles ordnungsgemäß eingerichtet ist:

![](walkthrough-images/image5.png "Nachdem das Projekt erstellt wurde, führen Sie es, sicherzustellen, dass sie builds und, dass alles richtig eingerichtet")


## <a name="creating-a-content-project"></a>Erstellen einen Content-Projekt

Nun, da wir einen Spielprojekt haben, können wir eine MonoGame Pipeline-Projekt erstellen. Dazu in der Select MonoGame Pipelinetool **Datei > neu...**  und navigieren Sie zu Ihrem Projekt Ordners "Content". Für Android, befindet sich der Ordner unter **[Projekt root]\BouncingGame.Android\Assets\Content\**. Für iOS, befindet sich der Ordner unter **[Projekt root]\BouncingGame.iOS\Content\**.

Ändern der **Dateiname** auf **ContentProject** , und klicken Sie auf die **speichern** Schaltfläche:

![](walkthrough-images/image8.png "Ändern Sie den Dateinamen in ContentProject, und klicken Sie auf die Schaltfläche "Speichern"")

Nachdem das Projekt erstellt wurde, zeigt das Pipelinetool MonoGame Informationen über das Projekt beim Stamm **ContentProject** Element ausgewählt ist:

![](walkthrough-images/image9.png "Nachdem das Projekt erstellt wurde, zeigen das Pipelinetool MonoGame Informationen zum des Projekts, wenn das Stammverzeichnis ContentProject Element ausgewählt ist")

Sehen wir uns einige der wichtigsten Optionen für das Content-Projekt.


### <a name="output-folder"></a>Ausgabeordner

Dies ist der Ordner (in Bezug auf das Content-Projekt selbst), in dem die Ausgabe **.xnb** Dateien gespeichert werden. Um die Dinge einfach zu halten, verwenden wir den gleichen Ordner für die bereitzustellenden unsere Eingabe Ausgabedateien. Ändern wir also die **Ausgabeordner** werden **.\**  :

![](walkthrough-images/image10.png "")


### <a name="platform"></a>Plattform

Dadurch wird die Zielplattform für den Inhalt definiert. Beachten Sie, dass dies **Windows** standardmäßig, sodass wir möchten dies in unserem Zielplattform ändern also **Android** (oder iOS, wenn Sie zusammen mit einem iOS-Projekt).

![](walkthrough-images/image11.png "Beachten Sie, dass dies Windows standardmäßig aktiviert ist, so ändern Sie die Zielplattform aus die Android oder iOS ist, wenn Sie zusammen mit einem iOS-Projekt")


## <a name="processing-files-in-the-monogame-pipeline-tool"></a>Verarbeiten von Dateien in das Pipelinetool MonoGame

Als Nächstes wir werden werden hinzufügen Inhalt an unsere **ContentProject**. Für dieses Projekt wir werden Sie in das Stammverzeichnis des Projekts werden Dateien hinzufügen, aber größere Projekte werden in der Regel ihre Inhalte in Ordnern organisieren.

Es müssen zwei Dateien hinzufügen:

 - Ein **PNG** Datei, die verwendet werden soll, ein Sprite gezeichnet werden soll. Diese Datei kann [hier herunterladen](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/ball.png?raw=true).
 - Ein **.spritefont** Datei, die zum Zeichnen des Texts auf dem Bildschirm verwendet werden soll. Der Inhalt Pipeline-Tool unterstützt die neue .spritefont-Dateien erstellen, damit es keine Datei zum Herunterladen ist.


### <a name="adding-a-png-file"></a>Eine PNG-Datei hinzufügen

Hinzufügen einer **PNG** Datei dem Projekt wird zunächst kopieren wir es in dasselbe Verzeichnis wie das pipelineprojekt besitzt die **.mgcb** Erweiterung.

![](walkthrough-images/image12.png "Eine PNG-Datei zum Projekt hinzufügen")

Als Nächstes fügen wir die Datei dem Projekt Pipeline. Wählen Sie dazu in das Pipelinetool MonoGame **Bearbeiten > Element hinzufügen...** , wählen die **ball.png** Datei, und klicken Sie auf **öffnen**. Die Datei ist nun Teil der Content-Projekt und, bei Auswahl dieser Option werden die Eigenschaften angezeigt:

![](walkthrough-images/image13.png "Die Datei werden jetzt Teil des Content-Projekts, und bei Auswahl dieser Option werden die Eigenschaften anzeigen")

Wir werden lassen alle Werte auf ihre Standardwerte sind keine Änderungen erforderlich, um die Datei .xnb in CocosSharp zu laden. Wir können die Datei erstellen, indem Sie die Auswahl der **erstellen > Erstellen** Menüoption, Starten eines Builds und Ausgabe zum Build anzuzeigen. Wir können überprüfen, ob der Build ordnungsgemäß überprüft funktioniert die **Content** für einen neuen Ordner **ball.xnb** Datei:

![](walkthrough-images/image14.png "Stellen Sie sicher, dass der Build ordnungsgemäß funktioniert, indem Sie den Inhaltsordner für eine neue ball.xnb-Datei überprüfen")


### <a name="adding-a-spritefont-file"></a>Hinzufügen einer Datei .spritefont

Wir können .spritefont-Datei durch das Pipelinetool MonoGame erstellen. CocosSharp erfordert Schriftarten in einer **Schriftarten** Ordner und CocosSharp Vorlagen, die automatisch eine Schriftartordner automatisch erstellt. Wir können das Pipelinetool MonoGame in diesem Ordner hinzufügen, durch Auswahl **Bearbeiten > Hinzufügen > vorhandenen Ordner...** . Navigieren Sie zu der **Content** Ordner, und wählen die **Schriftarten** Ordner, und klicken Sie auf **OK**:

![](walkthrough-images/browsetofonts.png "In den Inhaltsordner durchsuchen Sie, wählen Sie den Ordner Schriftarten, und klicken Sie auf OK")

Um eine neue .sprintefont Datei hinzuzufügen, mit der rechten Maustaste auf den Ordner Schriftarten, und wählen Sie **hinzufügen > Neues Element...**, wählen die **SpriteFont Beschreibung** aus, geben Sie den Namen **arial 36**, und klicken Sie auf **Ok**. CocosSharp erfordert spezifische Benennung von Schriftartdateien – sie muss im Format [FontType]-[FontSize]. Wenn eine Schriftart nicht dieses Namensformat übereinstimmt, wird es nicht von CocosSharp zur Laufzeit geladen werden.

![](walkthrough-images/image15.png "Wenn eine Schriftart nicht dieses Namensformat übereinstimmt wird es nicht von CocosSharp zur Laufzeit geladen werden")

Die Datei .spritefont ist tatsächlich eine XML-Datei, die in einem Texteditor, einschließlich Visual Studio für Mac bearbeitet werden können Die am häufigsten verwendeten Variablen in einer Datei .spritefont bearbeitet werden die `FontName` und `Size` Eigenschaft:


```xml
    <!-- Modify this string to change the font that will be imported. -->
    <FontName>Arial</FontName>

    <!-- Size is a float value, measured in points. 
    Modify this value to change the size of the font. -->
    <Size>12</Size> 
```

Wir müssen die Datei in einem beliebigen Texteditor geöffnet. Als unsere **arial 36.spritefont** Name bereits vermuten lässt, wir werden lassen die `FontName` als `Arial` jedoch ändern, die `Size` Wert `36`:


```xml
    <!-- Modify this string to change the font that will be imported. -->
    <FontName>Arial</FontName>   
  
    <!-- Size is a float value, measured in points. 
    Modify this value to change the size of the font. -->4/10/2016 12:57:28 PM 
    <Size>36</Size>
```
 
## <a name="using-files-at-runtime"></a>Verwenden von Dateien zur Laufzeit

.Xnb Dateien werden jetzt erstellt und in unserem Projekt verwendet werden können. Wir müssen hinzufügen werden die Dateien in Visual Studio für Mac, und klicken Sie dann wir fügen Code zu unserer `GameScene.cs` Datei laden diese Dateien und angezeigt werden können.


### <a name="adding-xnb-files-to-visual-studio-for-mac"></a>Hinzufügen von .xnb-Dateien in Visual Studio für Mac

Zunächst fügen wir die Dateien zum Projekt. In Visual Studio für Mac müssen erweitern die **BouncingGame.Android** Projekt, erweitern Sie die **Bestand** Ordner mit der rechten Maustaste auf die **Content** Ordner, und wählen Sie **Hinzufügen > Hinzufügen von Dateien...** Wir wählen zuerst die **ball.xnb** wir zuvor erstellt, und klicken Sie auf **öffnen**. Klicken Sie dann die oben genannten Schritte wiederholen, aber Hinzufügen der **arial 36.xnb** Datei. Wir wählen die **behalten Sie die Datei in seiner aktuellen Unterverzeichnis** option, wenn Sie Visual Studio für Mac zum Hinzufügen der das aufgefordert werden. Einmal nicht mehr benötigen beide Dateien sollten Teil unserer Projekts werden:

![](walkthrough-images/image20.png "Einmal nicht mehr benötigen beide Dateien muss Teil des Projekts")


### <a name="adding-gamescenecs"></a>Hinzufügen von **GameScene.cs**

Wir erstellen eine Klasse mit dem Namen `GameScene,` unsere Objekte Sprite und Text enthalten. Zu diesem Zweck mit der Maustaste auf die **BouncingGame** (nicht BouncingGame.Android) Projekt, und wählen Sie **hinzufügen > neuen Datei...**. Wählen Sie die **allgemeine** Kategorie, wählen die **leere Klasse** aus, und geben Sie dann den Namen **GameScene**.

Nach der Erstellung ändern wir die `GameScene.cs` Datei enthält den folgenden Code:


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

Gehen wir wird nicht näher auf die oben aufgeführten Code seit der Arbeit mit CocosSharp visueller Objekte wie CCSprite und CCLabelTtf in abgedeckt wird die [BouncingGame Handbuch](~/graphics-games/cocossharp/bouncing-game.md).

Wir müssen auch Code hinzufügen, um unseren neu erstellten laden `GameScene`. Hierzu wir öffnen die `GameAppDelegate.cs` Datei (befindet sich im die **BouncingGame** PCL) und ändern die `ApplicationDidFinishLaunching` Methode, sodass es aussieht:


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

Bei der Ausführung wird unsere Spiel formuliert werden:

![](walkthrough-images/image1.png "Bei der Ausführung sieht des Spiels wie")


## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise wurde gezeigt, wie Sie das Pipelinetool MonoGame um .xnb Dateien aus einer Eingabe PNG-Datei zu erstellen sowie Gewusst wie: Erstellen einer neuen .xnb-Datei aus einer Datei .sprintefont neu erstellt. Es wurde auch erläutert, wie CocosSharp-Projekte zur Verwendung von .xnb Dateien strukturiert und wie diese Dateien zur Laufzeit geladen.

## <a name="related-links"></a>Verwandte links

- [MonoGame Downloads](http://www.monogame.net/downloads/)
- [MonoGame Pipeline-Dokumentation](http://www.monogame.net/documentation/?page=Pipeline)
- [Starten BouncingGame-Projekts für Android (Beispiel)](https://developer.xamarin.com/samples/BouncingGameCompleteAndroid/)
- [ball.png Graphic (sample)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/ball.png?raw=true)
