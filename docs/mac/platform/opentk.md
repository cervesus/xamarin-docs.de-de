---
title: Einführung in opentk in xamarin. Mac
description: Dieser Artikel bietet eine Einführung in die Arbeit mit opentk in einer xamarin. Mac-Anwendung. Es wird das Erstellen und Verwalten eines Spielfensters, das Rendern eines einfachen Objekts und das Anzeigen des Objekts für den Benutzer behandelt.
ms.prod: xamarin
ms.assetid: BDE05645-7273-49D3-809B-8642347678D2
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/14/2017
ms.openlocfilehash: 908ed187d8d3d341f8d65e3a3d417588492f325f
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292965"
---
# <a name="introduction-to-opentk-in-xamarinmac"></a>Einführung in opentk in xamarin. Mac

Opentk (das Open Toolkit) ist eine erweiterte Bibliothek auf niedriger Ebene C# , die das Arbeiten mit OpenGL, OpenCL und OpenAL erleichtert. Opentk kann für Spiele, wissenschaftliche Anwendungen oder andere Projekte verwendet werden, für die 3D-Grafiken, Audiodaten oder Berechnungsfunktionen erforderlich sind. Dieser Artikel bietet eine kurze Einführung in die Verwendung von opentk in einer xamarin. Mac-app.

[![](opentk-images/intro01.png "Ein Beispiel für eine APP-Laufzeit")](opentk-images/intro01.png#lightbox)

In diesem Artikel werden die Grundlagen von opentk in einer xamarin. Mac-Anwendung behandelt. Es wird dringend empfohlen, dass Sie zunächst den Artikel [Hello, Mac](~/mac/get-started/hello-mac.md) , insbesondere die [Einführung in Xcode und die Abschnitte zu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und Outlets und [Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) , verwenden, da er wichtige Konzepte und Techniken behandelt, die wir in verwenden werden. Dieser Artikel.

Sie können sich auch den Abschnitt verfügbar machen von [ C# Klassen/Methoden zu "Ziel-C](~/mac/internals/how-it-works.md) " im Dokument " [xamarin. Mac](~/mac/internals/how-it-works.md) " ansehen. darin werden die-und `Register` `Export` -Befehle erläutert, die zum Verknüpfen der C# Klassen mit verwendet werden. Ziel-C-Objekte und UI-Elemente.

<a name="About_OpenTK" />

## <a name="about-opentk"></a>Informationen über opentk

Wie bereits erwähnt, ist opentk (das Open Toolkit) eine erweiterte Bibliothek auf niedriger Ebene C# , die das Arbeiten mit OpenGL, OpenCL und OpenAL erleichtert. Die Verwendung von opentk in einer xamarin. Mac-app bietet die folgenden Features:

- **Schnelle Entwicklung** : opentk bietet starke Datentypen und eine Inline Dokumentation, mit denen Sie Ihren Codierungs Workflow verbessern und Fehler leichter und schneller erfassen können.
- **Easy Integration** : opentk wurde entwickelt, um problemlos in .NET-Anwendungen integriert werden zu können.
- **Permissive License** -opentk wird unter den Lizenzen mit/X11 verteilt und ist vollständig kostenlos.
- Umfassende **, typsichere Bindungen** : opentk unterstützt die neuesten Versionen von OpenGL, OpenGL | es, OpenAL und OpenCL mit automatischem Laden von Erweiterungen, Fehlerüberprüfung und Inline Dokumentation.
- **Flexible GUI-Optionen** : opentk stellt das systemeigene, hochleistungsfähige Spielfenster bereit, das speziell für Spiele und xamarin. Mac entwickelt wurde.
- **Vollständig verwaltetes, CLS-kompatibles Code** -opentk unterstützt 32-Bit-und 64-Bit-Versionen von macOS ohne nicht verwaltete Bibliotheken.
- **3D Math Toolkit** Opentk liefert `Vector` `Matrix` `Bezier` ,- `Quaternion` und-Strukturen über das 3D Math Toolkit.

Opentk kann für Spiele, wissenschaftliche Anwendungen oder andere Projekte verwendet werden, für die 3D-Grafiken, Audiodaten oder Berechnungsfunktionen erforderlich sind.

Weitere Informationen finden Sie auf [der Open Toolkit](http://www.opentk.com) -Website.

<a name="OpenTK_Quickstart" />

## <a name="opentk-quickstart"></a>Opentk-Schnellstart

Als schnelle Einführung in die Verwendung von opentk in einer xamarin. Mac-app erstellen wir eine einfache Anwendung, die eine Spielansicht öffnet, ein einfaches Dreieck in dieser Ansicht rendert und die Spielansicht an das Hauptfenster der Mac-app übergibt, um dem Benutzer das Dreieck anzuzeigen.

<a name="Starting_a_New_Project" />

### <a name="starting-a-new-project"></a>Starten eines neuen Projekts

Starten Sie Visual Studio für Mac, und erstellen Sie eine neue xamarin. Mac-Projekt Mappe. Wählen > Sie > die**Allgemeine**Mac-app, > Cocoa-App:

[![](opentk-images/sample01.png "Hinzufügen einer neuen Cocoa-App")](opentk-images/sample01.png#lightbox)

Geben `MacOpenTK` Sie als **Projektnamen**ein:

[![](opentk-images/sample02.png "Festlegen des Projekt namens")](opentk-images/sample02.png#lightbox)

Klicken Sie auf die Schaltfläche **Erstellen** , um das neue Projekt zu erstellen.

<a name="Including_OpenTK" />

### <a name="including-opentk"></a>Einschließen von opentk

Bevor Sie Open tk in einer xamarin. Mac-Anwendung verwenden können, müssen Sie einen Verweis auf die opentk-Assembly einschließen. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner **Verweise** , und wählen Sie **Verweise bearbeiten...** aus.

Aktivieren Sie eine über `OpenTK` Prüfung, und klicken Sie auf die Schaltfläche **OK** :

[![](opentk-images/sample03.png "Bearbeiten der Projekt Verweise")](opentk-images/sample03.png#lightbox)

<a name="Using_OpenTK" />

### <a name="using-opentk"></a>Verwenden von opentk

Nachdem das neue Projekt erstellt wurde, doppelklicken Sie `MainWindow.cs` auf die Datei im **Projektmappen-Explorer** , um Sie für die Bearbeitung zu öffnen. Legen Sie `MainWindow` die Klasse wie folgt an:

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

Wir wollen diesen Code im folgenden ausführlich erläutern.

<a name="Required_APIs" />

### <a name="required-apis"></a>Erforderliche APIs

Mehrere Verweise sind erforderlich, um opentk in einer xamarin. Mac-Klasse zu verwenden. Zu Beginn der Definition haben wir die folgenden `using` Anweisungen eingefügt:

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

Diese minimale Menge wird für jede Klasse mit opentk benötigt.

<a name="Adding_the_Game_View" />

### <a name="adding-the-game-view"></a>Hinzufügen der Spielansicht

Als nächstes müssen wir eine Spielansicht erstellen, die die gesamte Interaktion mit opentk enthält und die Ergebnisse anzeigt. Wir haben den folgenden Code verwendet:

```csharp
public MonoMacGameView Game { get; set; }
...

// Create new Game View and replace the window content with it
Game = new MonoMacGameView(ContentView.Frame);
ContentView = Game;
```

Hier haben wir die gleiche Größe wie im Mac-Hauptfenster festgestellt und die Inhaltsansicht des Fensters durch die neue `MonoMacGameView`ersetzt. Da wir den vorhandenen Fensterinhalt ersetzt haben, wird unsere Ansicht automatisch in der Größe geändert, wenn die Größe des Hauptfensters geändert wird.

<a name="Responding_to_Events" />

### <a name="responding-to-events"></a>Reagieren auf Ereignisse

Es gibt mehrere Standard Ereignisse, auf die jede Spielansicht reagieren sollte. In diesem Abschnitt werden die Hauptereignisse behandelt, die erforderlich sind.

<a name="The_Load_Event" />

### <a name="the-load-event"></a>Das Lade Ereignis

Das `Load` Ereignis dient zum Laden von Ressourcen von einem Datenträger, z. b. Bilder, Texturen oder Musik. Für unsere einfache Test-App verwenden wir das-Ereignis nicht `Load` , aber Sie haben es für den Verweis eingeschlossen:

```csharp
Game.Load += (sender, e) =>
{
    // TODO: Initialize settings, load textures and sounds here
};
```

<a name="The_Resize_Event" />

### <a name="the-resize-event"></a>Das Ereignis zur Größenänderung

Das `Resize` Ereignis sollte jedes Mal aufgerufen werden, wenn die Größe der Spielansicht geändert wird. Für unsere Beispiel-app erstellen wir den GL-Viewport mit dem folgenden Code in der gleichen Größe wie die Spielansicht (die automatisch mit dem Mac-Hauptfenster geändert wird):

```csharp
Game.Resize += (sender, e) =>
{
    // Adjust the GL view to be the same size as the window
    GL.Viewport(0, 0, Game.Size.Width, Game.Size.Height);
};
```

<a name="The_UpdateFrame_Event" />

### <a name="the-updateframe-event"></a>Das updateframe-Ereignis

Das `UpdateFrame` -Ereignis wird verwendet, um Benutzereingaben zu behandeln, Objektpositionen zu aktualisieren, Physik-oder AI-Berechnungen auszuführen. Für unsere einfache Test-App verwenden wir das-Ereignis nicht `UpdateFrame` , aber Sie haben es für den Verweis eingeschlossen:

```csharp
Game.UpdateFrame += (sender, e) =>
{
    // TODO: Add any game logic or physics
};
```

> [!IMPORTANT]
> Die xamarin. Mac-Implementierung von opentk enthält `Input API`nicht, sodass Sie die von Apple bereitgestellten APIs zum Hinzufügen von Tastatur-und Mausunterstützung verwenden müssen. Optional können Sie eine benutzerdefinierte Instanz von `MonoMacGameView` erstellen und die-Methode und die `KeyDown` - `KeyUp` Methode überschreiben.

<a name="The_RenderFrame_Event" />

### <a name="the-renderframe-event"></a>Das renderframe-Ereignis

Das `RenderFrame` Ereignis enthält den Code, mit dem die Grafiken gerbt (gezeichnet) werden. In unserer Beispiel-App füllen wir die Spielansicht mit einem einfachen Dreieck:

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

Der Rendering-Code wird in der Regel mit einem `GL.Clear` -Befehl aufgerufen, um alle vorhandenen Elemente vor dem Zeichnen der neuen Elemente zu entfernen.

> [!IMPORTANT]
> Für die xamarin. Mac-Version von opentk wird die `SwapBuffers` -Methode der `MonoMacGameView` -Instanz **nicht** am Ende des Renderingcodes aufgerufen. Dies führt dazu, dass die Spielansicht schnell ist, anstatt die gerenderte Ansicht anzuzeigen.

<a name="Running_the_Game_View" />

### <a name="running-the-game-view"></a>Ausführen der Spielansicht

Wenn Sie alle erforderlichen Ereignisse definieren und die Spielansicht, die mit dem Mac-Hauptfenster der App verbunden ist, werden wir gelesen, um die Spielansicht auszuführen und die Grafiken anzuzeigen. Verwenden Sie folgenden Code:

```csharp
// Run the game at 60 updates per second
Game.Run(60.0);
```

Wir übergeben die gewünschte Framerate, bei der die Spielansicht aktualisiert werden soll. in unserem Beispiel haben wir Frames `60` pro Sekunde ausgewählt (dieselbe Aktualisierungsrate wie normale Fernsehsendungen).

Führen Sie unsere App aus, und sehen Sie sich die Ausgabe an:

[![](opentk-images/intro01.png "Ein Beispiel für die Ausgabe der apps.")](opentk-images/intro01.png#lightbox)

Wenn die Größe des Fensters geändert wird, wird auch die Spielansicht angezeigt, und die Größe des Dreiecks wird ebenfalls angepasst und in Echtzeit aktualisiert.

<a name="Where_to_Next" />

### <a name="where-to-next"></a>Wo geht es weiter?

Mit den Grundlagen der Arbeit mit opentk in einer xamarin. Mac-Anwendung finden Sie hier einige Vorschläge für die nächsten Schritte:

- Versuchen Sie, die Farbe des Dreiecks und die Hintergrundfarbe der Spielansicht im `Load` -Ereignis und im- `RenderFrame` Ereignis zu ändern.
- Ändern Sie die Farbe des Dreiecks, wenn der Benutzer eine Taste in `UpdateFrame` den `RenderFrame` Ereignissen und drückt, oder erstellen `MonoMacGameView` Sie eine eigene Benutzer `KeyUp` definierte `KeyDown` Klasse, und überschreiben Sie die Methoden und.
- Bewegen Sie das Dreieck über den Bildschirm, indem Sie die im `UpdateFrame` Ereignis aktivierenden Schlüssel verwenden. Hinweis: Verwenden Sie `Matrix4.CreateTranslation` die-Methode zum Erstellen einer Übersetzungs Matrix, `GL.LoadMatrix` und rufen Sie die-Methode `RenderFrame` auf, um Sie im-Ereignis zu laden.
- Verwenden Sie `for` eine Schleife, um mehrere Dreiecke `RenderFrame` im Ereignis zu Renderern.
- Drehen Sie die Kamera, um eine andere Ansicht des Dreiecks in 3D-Raum zu schaffen. Hinweis: Verwenden Sie `Matrix4.CreateTranslation` die-Methode zum Erstellen einer Übersetzungs Matrix und `GL.LoadMatrix` zum Abrufen der-Methode, um Sie zu laden. Sie können `Vector2`auch die Klassen `Vector4` , `Vector3`und `Matrix4` für Kamera Manipulationen verwenden.

Weitere Beispiele finden Sie im [GitHub-Repository opentk Samples (opentk](https://github.com/opentk/opentk/tree/master/Source/Examples) -Beispiele). Sie enthält eine offizielle Liste der Beispiele für die Verwendung von opentk. Sie müssen diese Beispiele für die Verwendung von mit der xamarin. Mac-Version von opentk anpassen.

Ein komplexeres xamarin. Mac-Beispiel für eine opentk-Implementierung finden Sie in unserem [monomacgameview](https://docs.microsoft.com/samples/xamarin/mac-samples/monomacgamewindow) -Beispiel.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde die Arbeit mit opentk in einer xamarin. Mac-Anwendung kurz erläutert. Wir haben gesehen, wie Sie ein Spielfenster erstellen, das Spielfenster an ein Mac-Fenster anfügen und eine einfache Form im Spielfenster darstellen.

## <a name="related-links"></a>Verwandte Links

- [Macopentk (Beispiel)](https://docs.microsoft.com/samples/xamarin/mac-samples/macopentk)
- [Monomacgameview (Beispiel)](https://docs.microsoft.com/samples/xamarin/mac-samples/monomacgamewindow)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Arbeiten mit Windows](~/mac/user-interface/window.md)
- [Das Open Toolkit](http://www.opentk.com)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Einführung in Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
