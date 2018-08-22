---
title: Einführung in opentk xamarin.Mac
description: Dieser Artikel enthält eine Einführung in die Arbeit mit OpenTK in einer Xamarin.Mac-Anwendung. Hierin sind erstellen und verwalten ein Spiel-Fenster, ein einfaches Objekt zu rendern und Anzeigen des Objekts für den Benutzer.
ms.prod: xamarin
ms.assetid: BDE05645-7273-49D3-809B-8642347678D2
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 02cd32ac3016a3556cac8611ff839336f3089235
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2018
ms.locfileid: "40251017"
---
# <a name="introduction-to-opentk-in-xamarinmac"></a>Einführung in opentk xamarin.Mac

OpenTK (das Open-Toolkit) ist eine erweiterte, Low-Level C#-Bibliothek, die Arbeit mit OpenGL, OpenCL und OpenAL einfacher macht. OpenTK kann für die Spiele, wissenschaftliche Anwendungen oder andere Projekte, die 3D-Grafiken erfordern, Audio- oder COMPUTE-Funktionen verwendet werden. Dieser Artikel bietet eine kurze Einführung in OpenTK in einer Xamarin.Mac-app verwenden.

[![](opentk-images/intro01.png "Eine Beispiel-app-Ausführung")](opentk-images/intro01.png#lightbox)

In diesem Artikel wird die Grundlagen der OpenTK in einer Xamarin.Mac-Anwendung behandelt. Es wird dringend empfohlen, dass Sie über arbeiten die [Hallo, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst, insbesondere die [Einführung in Xcode und Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und [Outlets und Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) Abschnitte, wie sie behandelt wichtige Konzepte und Techniken, die wir in diesem Artikel verwenden.

Empfiehlt es sich um einen Blick auf die [Verfügbarmachen von c#-Klassen / Methoden mit Objective-C](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac-Interna](~/mac/internals/how-it-works.md) dokumentieren, es wird erläutert, die `Register` und `Export` Befehle verwendet, um Ihre Klassen in c# für Objective-C-Objekte und Elemente der Benutzeroberfläche anschließen.

<a name="About_OpenTK" />

## <a name="about-opentk"></a>Informationen zu OpenTK

Wie bereits erwähnt, ist OpenTK (das Open-Toolkit) eine erweiterte, Low-Level C#-Bibliothek, die Arbeit mit OpenGL, OpenCL und OpenAL vereinfacht. OpenTK in einer Xamarin.Mac-app verwenden, bietet die folgenden Features:

- **Eine schnelle Entwicklung** -OpenTK bietet, starke Typen und Inline-Dokumentation verbessern Ihren Workflow Schreiben von Code und Abfangen von Fehlern, schneller und einfacher.
- **Einfache Integration** -OpenTK wurde entworfen, um die einfache Integration in Anwendungen für .NET.
- **"Liberalen" Lizenz** -OpenTK Rahmen der Lizenzen MIT/X11 verteilt werden und ist völlig kostenlos.
- **Umfassende, typsichere Bindungen** -OpenTK unterstützt die neuesten Versionen der OpenGL, OpenGL | ES, OpenAL und OpenCL automatische Erweiterung Loading, Fehler überprüfen und Inline-Dokumentation.
- **Flexible Optionen für die grafische Benutzeroberfläche** -OpenTK stellt das systemeigene, Spielefenster für hohe Leistung entworfen werden, insbesondere für Spiele und Xamarin.Mac.
- **Ein vollständig verwalteter CLS-kompatiblem Code** -OpenTK unterstützt 32-Bit- und 64-Bit-Versionen von MacOS mit keine nicht verwalteten Bibliotheken.
- **3D Math-Toolkit** OpenTK stellt `Vector`, `Matrix`, `Quaternion` und `Bezier` Strukturen über zugehörige 3D Math-Toolkit.

OpenTK kann für die Spiele, wissenschaftliche Anwendungen oder andere Projekte, die 3D-Grafiken erfordern, Audio- oder COMPUTE-Funktionen verwendet werden.

Weitere Informationen finden Sie unter [das Open-Toolkit](http://www.opentk.com) Website.

<a name="OpenTK_Quickstart" />

## <a name="opentk-quickstart"></a>OpenTK-Schnellstart

Als eine kurze Einführung in OpenTK in einer Xamarin.Mac-app verwenden werden wir erstellen eine einfache Anwendung, die eine Spiel-Ansicht geöffnet ist, rendert ein einfaches Dreieck in dieser Ansicht und Attachs der Game-Ansicht für der Mac-app-Main-Fenster, das das Dreieck für den Benutzer anzuzeigen.

<a name="Starting_a_New_Project" />

### <a name="starting-a-new-project"></a>Starten eines neuen Projekts

Starten Sie Visual Studio für Mac, und Erstellen einer neuen Xamarin.Mac-Projektmappe. Wählen Sie **Mac** > **App** > **allgemeine** > **Cocoa-App**:

[![](opentk-images/sample01.png "Hinzufügen einer neuen Cocoa-App")](opentk-images/sample01.png#lightbox)

Geben Sie `MacOpenTK` für die **Projektname**:

[![](opentk-images/sample02.png "Den Namen des Projekts festlegen")](opentk-images/sample02.png#lightbox)

Klicken Sie auf die **erstellen** um das neue Projekt zu erstellen.

<a name="Including_OpenTK" />

### <a name="including-opentk"></a>Einschließlich OpenTK

Bevor Sie Sie öffnen TK in einer Xamarin.Mac-Anwendung verwenden können, müssen Sie einen Verweis auf die Assembly OpenTK enthalten. In der **Projektmappen-Explorer**, mit der rechten Maustaste die **Verweise** Ordner, und wählen **Verweise bearbeiten...** .

Aktivieren Sie das Kontrollkästchen von `OpenTK` , und klicken Sie auf die **OK** Schaltfläche:

[![](opentk-images/sample03.png "Bearbeiten die Projektverweise")](opentk-images/sample03.png#lightbox)

<a name="Using_OpenTK" />

### <a name="using-opentk"></a>Verwenden von OpenTK

Die neu erstellte Projekt, und doppelklicken Sie auf die `MainWindow.cs` Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen. Stellen Sie die `MainWindow` Klasse sehen wie folgt:

```csharp
using System;
using System.Drawing;
using OpenTK;
using OpenTK.Graphics;
using OpenTK.Graphics.OpenGL;
using OpenTK.Platform.MacOS;
using Foundation;
using AppKit;
using CoreGraphics;

namespace MacOpenTK
{
    public partial class MainWindow : NSWindow
    {
        #region Computed Properties
        public MonoMacGameView Game { get; set; }
        #endregion

        #region Constructors
        public MainWindow (IntPtr handle) : base (handle)
        {
        }

        [Export ("initWithCoder:")]
        public MainWindow (NSCoder coder) : base (coder)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Create new Game View and replace the window content with it
            Game = new MonoMacGameView(ContentView.Frame);
            ContentView = Game;

            // Wire-up any required Game events
            Game.Load += (sender, e) =>
            {
                // TODO: Initialize settings, load textures and sounds here
            };

            Game.Resize += (sender, e) =>
            {
                // Adjust the GL view to be the same size as the window
                GL.Viewport(0, 0, Game.Size.Width, Game.Size.Height);
            };

            Game.UpdateFrame += (sender, e) =>
            {
                // TODO: Add any game logic or physics
            };

            Game.RenderFrame += (sender, e) =>
            {
                // Setup buffer
                GL.Clear(ClearBufferMask.ColorBufferBit | ClearBufferMask.DepthBufferBit);
                GL.MatrixMode(MatrixMode.Projection);

                // Draw a simple triangle
                GL.LoadIdentity();
                GL.Ortho(-1.0, 1.0, -1.0, 1.0, 0.0, 4.0);
                GL.Begin(BeginMode.Triangles);
                GL.Color3(Color.MidnightBlue);
                GL.Vertex2(-1.0f, 1.0f);
                GL.Color3(Color.SpringGreen);
                GL.Vertex2(0.0f, -1.0f);
                GL.Color3(Color.Ivory);
                GL.Vertex2(1.0f, 1.0f);
                GL.End();

            };

            // Run the game at 60 updates per second
            Game.Run(60.0);
        }
        #endregion
    }
}
```

Gehen Sie an diesem Code unten im Detail.

<a name="Required_APIs" />

### <a name="required-apis"></a>Erforderlichen APIs

Mehrere Verweise müssen OpenTK in einer Xamarin.Mac-Klasse zu verwenden. Zu Beginn der Definition haben wir Folgendes enthalten `using` Anweisungen:

```csharp
using System;
using System.Drawing;
using OpenTK;
using OpenTK.Graphics;
using OpenTK.Graphics.OpenGL;
using OpenTK.Platform.MacOS;
using Foundation;
using CoreGraphics;
```
Dieser minimale Satz werden für jede Klasse, die mithilfe von OpenTK nachgewiesen.

<a name="Adding_the_Game_View" />

### <a name="adding-the-game-view"></a>Hinzufügen der Game-Ansicht

Als Nächstes müssen wir erstellen eine Spiel-Ansicht, um alle unsere Interaktionen mit OpenTK enthalten, und die Ergebnisse anzuzeigen. Es verwendet den folgenden Code:

```csharp
public MonoMacGameView Game { get; set; }
...

// Create new Game View and replace the window content with it
Game = new MonoMacGameView(ContentView.Frame);
ContentView = Game;
```

Hier haben Sie die Spiel-Ansicht die gleiche Größe wie unsere Mac-Hauptfenster und die Inhaltsansicht im Fenster mit den neuen ersetzt `MonoMacGameView`. Da wir die vorhandenen Inhalte der Fenster ersetzt, wird die unserer Ansicht hat automatisch angepasst, beim Ändern der Größe der Main-Windows.

<a name="Responding_to_Events" />

### <a name="responding-to-events"></a>Reagieren auf Ereignisse

Es gibt mehrere Standardereignisse, denen auf jede Game-Ansicht reagieren soll. In diesem Abschnitt werden die wichtigsten Ereignisse behandelt.

<a name="The_Load_Event" />

### <a name="the-load-event"></a>Die Load-Ereignis

Die `Load` Ereignis ist der Ort, an die Ressourcen von einem Datenträger wie Bilder, Texturen oder Musik zu laden. Für unser einfaches, Test-app verwenden wir nicht die `Load` Ereignis jedoch zu Referenzzwecken eingeschlossen haben:

```csharp
Game.Load += (sender, e) =>
{
    // TODO: Initialize settings, load textures and sounds here
};
```

<a name="The_Resize_Event" />

### <a name="the-resize-event"></a>Resize-Ereignis

Die `Resize` Ereignis jedes Mal, wenn die Größe der Game-Ansicht geändert wird, aufgerufen werden soll. Für unser Beispiel-app werden wir dem Viewport GL vornehmen die gleiche Größe wie unsere Game-Ansicht (die automatisch vorgenommen, indem Sie die Mac-Hauptfenster sein wird) mit dem folgenden Code:

```csharp
Game.Resize += (sender, e) =>
{
    // Adjust the GL view to be the same size as the window
    GL.Viewport(0, 0, Game.Size.Width, Game.Size.Height);
};
```

<a name="The_UpdateFrame_Event" />

### <a name="the-updateframe-event"></a>Das Ereignis UpdateFrame

Die `UpdateFrame` Ereignis wird verwendet, um Benutzereingaben zu behandeln, update Objekt Positionen "," Ausführen Physik "und" AI-Berechnungen. Für unser einfaches, Test-app verwenden wir nicht die `UpdateFrame` Ereignis jedoch zu Referenzzwecken eingeschlossen haben: 

```csharp
Game.UpdateFrame += (sender, e) =>
{
    // TODO: Add any game logic or physics
};
```

> [!IMPORTANT]
> Die Xamarin.Mac-Implementierung von OpenTK schließt nicht die `Input API`, daher müssen Sie die Apple verwenden, vorausgesetzt, für die Unterstützung von APIs zum Hinzufügen von Tastatur und Maus. Optional können Sie erstellen eine benutzerdefinierte Instanz von der `MonoMacGameView` und überschreiben die `KeyDown` und `KeyUp` Methoden.

<a name="The_RenderFrame_Event" />

### <a name="the-renderframe-event"></a>Das Ereignis RenderFrame

Die `RenderFrame` Ereignis enthält den Code, der verwendet wird, um (Zeichnen) Rendern von Grafiken. Für die Beispiel-app werden wir die Game-Ansicht mit einem einfachen Dreieck ausfüllen: 

```csharp
Game.RenderFrame += (sender, e) =>
{
    // Setup buffer
    GL.Clear(ClearBufferMask.ColorBufferBit | ClearBufferMask.DepthBufferBit);
    GL.MatrixMode(MatrixMode.Projection);

    // Draw a simple triangle
    GL.LoadIdentity();
    GL.Ortho(-1.0, 1.0, -1.0, 1.0, 0.0, 4.0);
    GL.Begin(BeginMode.Triangles);
    GL.Color3(Color.MidnightBlue);
    GL.Vertex2(-1.0f, 1.0f);
    GL.Color3(Color.SpringGreen);
    GL.Vertex2(0.0f, -1.0f);
    GL.Color3(Color.Ivory);
    GL.Vertex2(1.0f, 1.0f);
    GL.End();

};
```

Die Render-Code wird in der Regel wird durch einen Aufruf von `GL.Clear` So entfernen Sie alle vorhandenen Elemente vor dem Zeichnen Sie die neuen Elemente.

> [!IMPORTANT]
> Für die Xamarin.Mac-Version des OpenTK **nicht** rufen Sie die `SwapBuffers` -Methode der Ihre `MonoMacGameView` Instanz am Ende des Renderingcodes. Auf diese Weise führt dazu, dass die Ansicht Spiel Strobe schnell anstatt der gerenderten Ansicht.

<a name="Running_the_Game_View" />

### <a name="running-the-game-view"></a>Ausführen der Game-Ansicht

Mit der alle erforderlichen Ereignisse definieren und der Game-Ansicht, die an die Mac-Hauptfenster unserer App angefügt, wir werden gelesen, führen Sie die Ansicht des Spiels und unsere Grafiken anzeigen. Verwenden Sie folgenden Code:

```csharp
// Run the game at 60 updates per second
Game.Run(60.0);
```

Wir übergeben die gewünschte Einzelbildrate, der der Game-Ansicht aktualisiert werden soll, in unserem Beispiel haben wir uns entschieden `60` Frames pro Sekunde (die gleichen Aktualisierungsrate als normale TV).

Wir führen Sie die app und die Ausgabe angezeigt:

[![](opentk-images/intro01.png "Ein Beispiel für die apps-Ausgabe")](opentk-images/intro01.png#lightbox)

Wenn wir unser Fenster Größe angepasst werden, werden der Game-Ansicht auch befinden, und das Dreieck geändert und auch in Echtzeit aktualisiert werden.

<a name="Where_to_Next" />

### <a name="where-to-next"></a>Wo weiter?

Mit den Grundlagen der Arbeit mit OpenTk in einer Xamarin.mac-Anwendung, die ausgeführt werden soll sind hier einige Vorschläge, was Sie weiter zu testen:

- Ändern Sie die Farbe des Dreiecks und Farbe des Hintergrunds der Game-Ansicht in der `Load` und `RenderFrame` Ereignisse.
- Stellen Sie das Dreieck Farbe zu ändern, wenn der Benutzer eine Taste in der `UpdateFrame` und `RenderFrame` Ereignisse oder Ihre eigene benutzerdefinierte `MonoMacGameView` Klasse, und überschreiben die `KeyUp` und `KeyDown` Methoden.
- Stellen Sie das Dreieck, verschieben Sie auf dem Bildschirm mit den Schlüsseln beachten, in der `UpdateFrame` Ereignis. Hinweis: Verwenden Sie die `Matrix4.CreateTranslation` -Methode erstellt eine translationsmatrix aus und rufen die `GL.LoadMatrix` Methode zum Laden der in der `RenderFrame` Ereignis.
- Verwenden einer `for` Schleife zum Rendern von mehreren Dreiecke in der `RenderFrame` Ereignis.
- Drehen Sie die Kamera, um eine andere Ansicht des Dreiecks im 3D-Raum zu erhalten. Hinweis: Verwenden Sie die `Matrix4.CreateTranslation` -Methode erstellt eine translationsmatrix aus und rufen die `GL.LoadMatrix` Methode zum Laden der Daten. Sie können auch die `Vector2`, `Vector3`, `Vector4` und `Matrix4` Klassen für die Kamera Bearbeitungen.

Weitere Beispiele finden Sie unter den [OpenTK Beispiele GitHub](https://github.com/opentk/opentk/tree/master/Source/Examples) Repository. Sie enthält eine Liste der Beispiele für die Verwendung von OpenTK offizielle. Sie müssen diese Beispiele für die Verwendung mit der Xamarin.Mac-Version von OpenTK anpassen.

Für eine komplexere Xamarin.Mac-Beispiel, das für die Implementierung OpenTK, informieren Sie sich unsere [MonoMacGameView](https://developer.xamarin.com/samples/mac/MonoMacGameWindow/) Beispiel.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat es sich um einen kurzen Blick auf die Arbeit mit OpenTK in einer Xamarin.Mac-Anwendung geführt. Erläutert, wie beim Erstellen eines Fensters Spiel wie das Spiel-Fenster an ein Mac-Fenster angefügt und wie Sie eine einfache Form im Spielefenster gerendert.

## <a name="related-links"></a>Verwandte Links

- [MacOpenTK (Beispiel)](https://developer.xamarin.com/samples/mac/MacOpenTK/)
- [MonoMacGameView (Beispiel)](https://developer.xamarin.com/samples/mac/MonoMacGameWindow/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Arbeiten mit Windows](~/mac/user-interface/window.md)
- [Das Open-Toolkit](http://www.opentk.com)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Einführung in Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
