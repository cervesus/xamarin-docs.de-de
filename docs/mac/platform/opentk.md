---
title: Einführung in OpenTK in Xamarin.Mac
description: Dieser Artikel bietet eine Einführung in das Arbeiten mit OpenTK in einer Anwendung Xamarin.Mac. Er umfasst das Erstellen und verwalten ein Spiel-Fenster, ein einfaches Objekt rendern und Anzeigen des Objekts für den Benutzer.
ms.prod: xamarin
ms.assetid: BDE05645-7273-49D3-809B-8642347678D2
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 448b8bdba8ccedbb732a73a265d0ce76bb589190
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792605"
---
# <a name="introduction-to-opentk-in-xamarinmac"></a>Einführung in OpenTK in Xamarin.Mac

OpenTK (die Open-Toolkit) ist eine erweiterte, Low-Level C#-Bibliothek, die Arbeit mit OpenGL, OpenCL und OpenAL erleichtert wird. OpenTK kann für Spiele, wissenschaftliche Anwendungen oder andere Projekte, die 3D-Grafiken erfordern, audio oder rechnerischen Funktionalität verwendet werden. Dieser Artikel enthält eine kurze Einführung in OpenTK in einer Xamarin.Mac-app verwenden.

[![](opentk-images/intro01.png "Eine Beispiel-app ausführen")](opentk-images/intro01.png#lightbox)

In diesem Artikel werden die Grundlagen der OpenTK in einer Anwendung Xamarin.Mac eingegangen. Wird mit hoher vorgeschlagen, dass Sie über arbeiten die [Hello, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst, insbesondere die [Einführung in Xcode und Benutzeroberflächen-Generator](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) und [Steckdosen und Aktionen](~/mac/get-started/hello-mac.md#Outlets_and_Actions) Abschnitte, wie sie behandelt wichtige Konzepte und Techniken, die in diesem Artikel verwendet werden.

Sie möchten einen Blick auf die [Verfügbarmachen von C#-Klassen / Methoden für Objective-C-](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentieren, es wird erläutert, die `Register` und `Export` Befehle verwendet, um über das Netzwerk Ihrer C#-Klassen für Objective-C-Objekte und Elemente der Benutzeroberfläche von Volltextkatalogen.

<a name="About_OpenTK" />

## <a name="about-opentk"></a>Informationen zu OpenTK

Wie bereits erwähnt, ist OpenTK (der Open-Toolkit) eine erweiterte, Low-Level C#-Bibliothek, die verwenden OpenGL, OpenCL und OpenAL erleichtert wird. OpenTK bietet in einer app Xamarin.Mac die folgenden Funktionen:

- **Schnelle Entwicklung** -OpenTK bietet sichere Datentypen und Inline-Dokumentation zum Verbessern der workflowinhalts Codierung und Abfangen von Fehlern, schneller und einfacher.
- **Einfache Integration** -OpenTK wurde entworfen, um die einfache Integration mit .NET-Anwendungen.
- **"Liberalen" Lizenz** -OpenTK wird unter der Lizenzen MIT/X11 verteilt und – völlig kostenlos ist.
- **Rich, typsichere Bindungen** -OpenTK unterstützt die neuesten Versionen der OpenGL OpenGL | ES OpenAL "und" OpenCL mit automatische Erweiterung laden, Fehler überprüfen und Inline-Dokumentation.
- **Flexible GUI-Konfigurationsoptionen** -OpenTK stellt das systemeigene, hohe Leistung Spiel Fenster, die speziell für Spiele und Xamarin.Mac entwickelt wurden.
- **Vollständig verwaltet, CLS-kompatiblem Code** -OpenTK unterstützt 32-Bit und 64-Bit-Versionen von MacOS mit keine nicht verwalteten Bibliotheken.
- **3D mathematische Toolkit** OpenTK liefert `Vector`, `Matrix`, `Quaternion` und `Bezier` Strukturen über seine 3D Math-Toolkit.

OpenTK kann für Spiele, wissenschaftliche Anwendungen oder andere Projekte, die 3D-Grafiken erfordern, audio oder rechnerischen Funktionalität verwendet werden.

Weitere Informationen finden Sie unter [The Open Toolkit](http://www.opentk.com) Website.

<a name="OpenTK_Quickstart" />

## <a name="opentk-quickstart"></a>OpenTK-Schnellstart

Als eine kurze Einführung in OpenTK in einer Xamarin.Mac-app verwenden werden wir erstellen eine einfache Anwendung, die ein Spiel Ansicht geöffnet ist, ein einfaches Dreieck in dieser Ansicht und Attachs Spiel Ansicht rendert, um der Mac-app-Hauptfenster auf das Dreieck für den Benutzer anzuzeigen.

<a name="Starting_a_New_Project" />

### <a name="starting-a-new-project"></a>Starten eines neuen Projekts

Starten Sie Visual Studio für Mac, und erstellen Sie eine neue Xamarin.Mac-Projektmappe. Wählen Sie **Mac** > **App** > **allgemeine** > **Kakao App**:

[![](opentk-images/sample01.png "Hinzufügen einer neuen Kakao-App")](opentk-images/sample01.png#lightbox)

Geben Sie `MacOpenTK` für die **Teamprojektnamen**:

[![](opentk-images/sample02.png "Den Namen des Projekts festlegen")](opentk-images/sample02.png#lightbox)

Klicken Sie auf die **erstellen** Schaltfläche, um das neue Projekt zu erstellen.

<a name="Including_OpenTK" />

### <a name="including-opentk"></a>Einschließlich OpenTK

Bevor Sie in einer Anwendung Xamarin.Mac öffnen TK verwenden können, müssen Sie einen Verweis auf die Assembly OpenTK verfügen. In der **Projektmappen-Explorer**, mit der rechten Maustaste die **Verweise** Ordner, und wählen **Verweise bearbeiten...** .

Aktivieren Sie das Kontrollkästchen durch `OpenTK` , und klicken Sie auf die **OK** Schaltfläche:

[![](opentk-images/sample03.png "Bearbeiten die Projektverweise")](opentk-images/sample03.png#lightbox)

<a name="Using_OpenTK" />

### <a name="using-opentk"></a>Verwenden von OpenTK

Die neu erstellte Projekt, und doppelklicken Sie auf die `MainWindow.cs` in der Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen. Stellen Sie die `MainWindow` Klasse aussehen wie folgt:

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

Gehen Sie wir einmal durch diesen Code im folgenden ausführlich.

<a name="Required_APIs" />

### <a name="required-apis"></a>Erforderliche APIs

Mehrere Verweise sind erforderlich, um in einer Klasse Xamarin.Mac OpenTK verwenden. Zu Beginn der Definition haben wir zählen folgende `using` Anweisungen:

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
Dieser minimale Satz wird für jede Klasse, die mit OpenTK erforderlich.

<a name="Adding_the_Game_View" />

### <a name="adding-the-game-view"></a>Hinzufügen der Ansicht für das Spielen

Als Nächstes müssen wir erstellen eine Game-Ansicht, um alle unserer Interaktion mit OpenTK enthalten und die Ergebnisse werden angezeigt. Es verwendet den folgenden Code:

```csharp
public MonoMacGameView Game { get; set; }
...

// Create new Game View and replace the window content with it
Game = new MonoMacGameView(ContentView.Frame);
ContentView = Game;
```

Hier wir haben vorgenommen Spiel Ansicht die gleiche Größe wie unsere Mac-Hauptfenster und die Inhaltsansicht des Fensters mit dem neuen ersetzt `MonoMacGameView`. Da wir den vorhandenen Fensterinhalt ersetzt wird, werden unsere gegeben Sicht beim Ändern der Größe der Main-Windows automatisch einnimmt.

<a name="Responding_to_Events" />

### <a name="responding-to-events"></a>Reagieren auf Ereignisse

Es gibt einige Standardereignisse, denen auf jede Ansicht Spiel reagieren soll. In diesem Abschnitt werden die wichtigsten-Ereignisse behandelt.

<a name="The_Load_Event" />

### <a name="the-load-event"></a>Das Load-Ereignis

Die `Load` Ereignis bietet die Möglichkeit zum Laden von Ressourcen vom Datenträger, z. B. Bilder, Texturen oder Musik. Für unsere einfache, Test-app verwenden wir nicht die `Load` Ereignis jedoch Verweis einbezogen haben:

```csharp
Game.Load += (sender, e) =>
{
    // TODO: Initialize settings, load textures and sounds here
};
```

<a name="The_Resize_Event" />

### <a name="the-resize-event"></a>Das Resize-Ereignis

Die `Resize` Ereignis aufgerufen werden, jedes Mal, wenn die Sicht Spiel angepasst wird. Für unser Beispiel-app sind GL Viewport machen wir die gleiche Größe wie unsere Game-Ansicht (u. automatisch vorgenommen, indem die Mac-Hauptfenster geändert wird) mit den folgenden Code:

```csharp
Game.Resize += (sender, e) =>
{
    // Adjust the GL view to be the same size as the window
    GL.Viewport(0, 0, Game.Size.Width, Game.Size.Height);
};
```

<a name="The_UpdateFrame_Event" />

### <a name="the-updateframe-event"></a>Das UpdateFrame-Ereignis

Die `UpdateFrame` Ereignis wird verwendet, um Benutzereingaben zu behandeln, Objekt Positionen, physikalische ausführen oder AI Berechnungen aktualisieren. Für unsere einfache, Test-app verwenden wir nicht die `UpdateFrame` Ereignis jedoch Verweis einbezogen haben: 

```csharp
Game.UpdateFrame += (sender, e) =>
{
    // TODO: Add any game logic or physics
};
```

> [!IMPORTANT]
> Die Implementierung Xamarin.Mac OpenTK schließt nicht die `Input API`, daher müssen Sie mit der Apple Verfügung-APIs zum Hinzufügen von Tastatur und Maus zur. Optional können Sie eine benutzerdefinierte Instanz von erstellen die `MonoMacGameView` und überschreiben die `KeyDown` und `KeyUp` Methoden.

<a name="The_RenderFrame_Event" />

### <a name="the-renderframe-event"></a>Das RenderFrame-Ereignis

Die `RenderFrame` Ereignis enthält den Code, der verwendet wird, um (Draw) zu Rendern der Grafiken. Für unser Beispiel-app werden wir die Game-Ansicht mit einem einfachen Dreieck ausfüllen: 

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

In der Regel wird der Render-Code wird mit einem Aufruf von `GL.Clear` So entfernen Sie alle vorhandenen Elemente vor dem Zeichnen der neuen Elemente.

> [!IMPORTANT]
> Für die Version Xamarin.Mac OpenTK **nicht** rufen die `SwapBuffers` Methode Ihrer `MonoMacGameView` Instanz am Ende des Renderingcodes. Auf diese Weise führt dazu, dass die Ansicht Spiel Strobe schnell anstelle die gerenderte Ansicht anzeigen.

<a name="Running_the_Game_View" />

### <a name="running-the-game-view"></a>Das Spiele Sicht ausführen

Mit allen erforderlichen Ereignisse definieren und die Game-Ansicht mit der Mac-Hauptfenster unserer App verbunden, wir werden gelesen, führen das Spiel anzeigen, und zeigen unsere Grafiken auf. Verwenden Sie folgenden Code:

```csharp
// Run the game at 60 updates per second
Game.Run(60.0);
```

Wir in die gewünschte Framerate übergeben, die wir der Spiel Ansicht aktualisiert werden sollen, in unserem Beispiel haben wir `60` Frames pro Sekunde (die gleichen Aktualisierungsrate als normale TV).

Wir unsere app ausführen und Ausgabe angezeigt:

[![](opentk-images/intro01.png "Ein Beispiel für die Ausgabe von apps")](opentk-images/intro01.png#lightbox)

Wenn wir unsere Fenster Größe angepasst werden, werden die Game-Sicht auch befinden und das Dreieck angepasst und auch in Echtzeit aktualisiert werden.

<a name="Where_to_Next" />

### <a name="where-to-next"></a>Wo weiter?

Mit den Grundlagen der Arbeit mit OpenTk in einer Xamarin.mac-Anwendung ausgeführt werden soll sind hier einige Vorschläge, wie Sie weiter zu testen:

- Ändern Sie die Farbe des Dreiecks und die Hintergrundfarbe der Sicht Spiel in den `Load` und `RenderFrame` Ereignisse.
- Stellen Sie das Dreieck Farbe zu ändern, wenn der Benutzer eine Taste in der `UpdateFrame` und `RenderFrame` Ereignisse oder stellen Sie Ihre eigene benutzerdefinierte `MonoMacGameView` Klasse, und überschreiben die `KeyUp` und `KeyDown` Methoden.
- Stellen Sie das Dreieck auf dem Bildschirm mit beachten Schlüsseln im Verschieben der `UpdateFrame` Ereignis. Hinweis: Verwenden Sie die `Matrix4.CreateTranslation` -Methode erstellt eine translationsmatrix aus und rufen die `GL.LoadMatrix` -Methode laden in das `RenderFrame` Ereignis.
- Verwenden einer `for` Schleife zum Rendern von mehreren Dreiecke in die `RenderFrame` Ereignis.
- Drehen Sie die Kamera, um eine andere Ansicht des Dreiecks im 3D-Raum zu gewähren. Hinweis: Verwenden Sie die `Matrix4.CreateTranslation` -Methode erstellt eine translationsmatrix aus und rufen die `GL.LoadMatrix` Methode zu laden. Sie können auch die `Vector2`, `Vector3`, `Vector4` und `Matrix4` Klassen für die Kamera Manipulationen.

Weitere Beispiele finden Sie unter der [OpenTK Samples GitHub](https://github.com/opentk/opentk/tree/master/Source/Examples) Repository. Sie enthält eine Liste offizielle Beispiele für die Verwendung von OpenTK. Sie müssen diese Beispiele für die Verwendung mit der Version Xamarin.Mac OpenTK angepasst werden kann.

Ein komplexeres Beispiel für Xamarin.Mac einer OpenTK-Implementierung, finden Sie in unserem [MonoMacGameView](https://developer.xamarin.com/samples/mac/MonoMacGameWindow/) Beispiel.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat einen kurzen Blick auf das Arbeiten mit OpenTK in einer Anwendung Xamarin.Mac übernommen. Wurde erläutert, wie beim Erstellen eines Fensters Spiel Fenster Spiel in einem Fenster Mac Anfügen und wie eine einfache Form im Spiel gerendert.

## <a name="related-links"></a>Verwandte Links

- [MacOpenTK (Beispiel)](https://developer.xamarin.com/samples/mac/MacOpenTK/)
- [MonoMacGameView (Beispiel)](https://developer.xamarin.com/samples/mac/MonoMacGameWindow/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Arbeiten mit Fenstern](~/mac/user-interface/window.md)
- [Das Open-Toolkit](http://www.opentk.com)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Einführung in Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
