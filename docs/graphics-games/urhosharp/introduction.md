---
title: Eine Einführung in die UrhoSharp
description: Dies bietet eine kurze Einführung in die Konzepte hinter UrhoSharp
ms.prod: xamarin
ms.assetid: 18041443-5093-4AF7-8B20-03E00478EF35
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: 6fb53aa6d4bad8f8fcae8608ab0192af15fe87b1
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="an-introduction-to-urhosharp"></a>Eine Einführung in die UrhoSharp

_Dies bietet eine kurze Einführung in die Konzepte hinter UrhoSharp_

![UrhoSharp-logo](introduction-images/urhosharp-icon.png)

UrhoSharp ist eine leistungsstarke 3D Spiel-Engine für Entwickler von Xamarin und .NET.  Es ähnelt Willen SceneKit von Apple und SpriteKit und physikalische einschließen, Navigation, Netzwerk- und viel mehr zwar weiterhin plattformübergreifende wird.

Es ist eine .NET Bindung an die [Urho3D](http://urho3d.github.io/) -engine und ermöglicht es Entwicklern, plattformübergreifenden Code schreiben, der Android, iOS Ziel verwendet werden können, Windows- und Mac mit dem codebase und können für OpenGL- und Direct3D-Systeme rendern.

UrhoSharp ist eine Spiele-Engine mit einer Vielzahl von Funktionen ausgegeben:

- Leistungsstarke Grafik 3D-Rendering
- [Physikalische Simulation](https://developer.xamarin.com/api/namespace/Urho.Physics/) (mithilfe der Aufzählungszeichen-Bibliothek)
- [Behandlung der Szene](https://developer.xamarin.com/api/type/Urho.Scene/)
- Async/await-Unterstützung
- [Angezeigter Aktionen API](https://developer.xamarin.com/api/namespace/Urho.Actions/)
- [2D Integration in 3D-Szenen](https://developer.xamarin.com/api/namespace/Urho.Urho2D/)
- [Schriftart-Rendering mit FreeType](https://developer.xamarin.com/api/type/Urho.Gui.FontFaceFreeType/)
- [Client- und Netzwerkfunktionen](https://developer.xamarin.com/api/namespace/Urho.Network/)
- [Importieren Sie eine Vielzahl von Bestand](https://developer.xamarin.com/api/namespace/Urho.Resources/) (mit Bestand-Bibliothek öffnen)
- [Navigation Netz- und Pathfinding](https://developer.xamarin.com/api/namespace/Urho.Navigation/) (mit dem neu gefasst/Umleitung)
- [Konvexe Hülle-Generierung für Collision Detection](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape/) (mit StanHull)
- [Audiowiedergabe](https://developer.xamarin.com/api/namespace/Urho.Audio/) (mit **Libvorbis**)

## <a name="getting-started"></a>Erste Schritte

UrhoSharp wird als bequem verteilt eine [NuGet-Paket](https://www.nuget.org/) und kann hinzugefügt werden zu C#- oder F#-Projekten, die auf Windows-, Mac-, Android oder iOS.  Die NuGet enthält sowohl die Bibliotheken, um das Programm auszuführen, als auch die grundlegende Ressourcen (CoreData), die vom Modul verwendet.

### <a name="urho-as-a-portable-class-library"></a>Urho als eine Portable Klassenbibliothek

Das Paket Urho kann aus einem Projekt plattformspezifischen oder aus einem Portable Class Library-Projekt, und Sie können den gesamten Code auf allen Plattformen wiederverwenden genutzt werden.  Dies bedeutet, dass alles, was Sie auf jeder Plattform werden müssten Schreiben Ihrer bestimmten Einstiegspunkt für die Plattform und übertragen Sie die Steuerung an den freigegebenen spielcode.

### <a name="samples"></a>Proben

Sie können einen Eindruck von den Möglichkeiten von Urho abrufen, öffnen Sie in Visual Studio für Mac oder in Visual Studio die Projektmappe aus:

[https://github.com/xamarin/urho-samples](https://github.com/xamarin/urho-samples)

Die Standard-Projektmappe enthält Projekte für Android, iOS, Windows und Mac  Wir haben diese Lösung so strukturiert, dass wir eine bestimmte denkbar Plattform-Startprogramm haben und aller Beispielcode und game Code befindet sich in einer portablen Klassenbibliothek zeigen, wie auf allen Plattformen wiederverwendeten Codes zu maximieren.

Wenden Sie sich an den [Urho und Ihre Plattform](~/graphics-games/urhosharp/platform/index.md) Seite Weitere Informationen zu Ihren eigenen Lösungen zu erstellen.

Da alle Beispiele einen gemeinsamen Satz von Benutzeroberflächenelemente gemeinsam verwenden, haben die Beispiele sind die grundlegenden Setup in dieser Datei abstrahiert:

[https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs](https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs)

Dies stellt eine Beispiel-Basisklasse, die einige grundlegende Tastatureingaben behandelt und Berührungsereignisse, Setups eine Kamera, bietet grundlegende Benutzeroberflächenelemente, wodurch sich können jedes Beispiel auf die spezifische Funktionalität zu konzentrieren, der präsentiert wird.

Das folgende Beispiel zeigt, was das Modul kann dadurch ist:

- [Samply Spiel](https://github.com/xamarin/urho-samples/tree/master/SamplyGame) einen einfachen Klon des ShootySkies.

Während den anderen Beispielen einzelne Eigenschaften jedes Beispiel wird.

## <a name="basic-structure"></a>Grundlegende Struktur

Spiel, sollte die Unterklasse der [ `Application` ](https://developer.xamarin.com/api/type/Urho.Application/) -Klasse, dies ist, in dem Sie ein Spiel einrichten, werden (auf der [ `Setup` ](https://developer.xamarin.com/api/member/Urho.Application.Setup/) Methode) und Starten des Spiels (in der [ `Start` ](https://developer.xamarin.com/api/member/Urho.Application.Start) Methode).  Erstellen Sie dann die Haupt-Benutzeroberfläche.  Wir werden einen kleinen Teil durchlaufen, die zeigt, die APIs für das Einrichten einer 3D-Szene, einige Elemente der Benutzeroberfläche und ein einfaches Verhalten zuordnen.

```csharp
class MySample : Application {
    protected override void Start ()
    {
        CreateScene ();
        Input.KeyDown += (args) => {
            if (args.Key == Key.Esc) Exit ();
        };
    }

    async void CreateScene()
    {
        // UI text
        var helloText = new Text()
        {
            Value = "Hello World from MySample",
            HorizontalAlignment = HorizontalAlignment.Center,
            VerticalAlignment = VerticalAlignment.Center
        };
        helloText.SetColor(new Color(0f, 1f, 1f));
        helloText.SetFont(
            font: ResourceCache.GetFont("Fonts/Font.ttf"),
            size: 30);
        UI.Root.AddChild(helloText);

        // Create a top-level scene, must add the Octree
        // to visualize any 3D content.
        var scene = new Scene();
        scene.CreateComponent<Octree>();
        // Box
        Node boxNode = scene.CreateChild();
        boxNode.Position = new Vector3(0, 0, 5);
        boxNode.Rotation = new Quaternion(60, 0, 30);
        boxNode.SetScale(0f);
        StaticModel modelObject = boxNode.CreateComponent<StaticModel>();
        modelObject.Model = ResourceCache.GetModel("Models/Box.mdl");
        // Light
        Node lightNode = scene.CreateChild(name: "light");
        lightNode.SetDirection(new Vector3(0.6f, -1.0f, 0.8f));
        lightNode.CreateComponent<Light>();
        // Camera
        Node cameraNode = scene.CreateChild(name: "camera");
        Camera camera = cameraNode.CreateComponent<Camera>();
        // Viewport
        Renderer.SetViewport(0, new Viewport(scene, camera, null));
        // Perform some actions
        await boxNode.RunActionsAsync(
            new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1)));
        await boxNode.RunActionsAsync(
            new RepeatForever(new RotateBy(duration: 1,
                deltaAngleX: 90, deltaAngleY: 0, deltaAngleZ: 0)));
     }
}
```

Wenn Sie diese Anwendung ausführen, werden Sie schnell feststellen, dass die Common Language Runtime über die besagt, dass ist Ihre Medienobjekte sind nicht vorhanden.  Müssen Sie ist eine Hierarchie in Ihrem Projekt zu erstellen, die mit dem speziellen Verzeichnisnamen "Daten" beginnt, und innerhalb dieser, sollten Sie die Ressourcen, die Sie in Ihrem Programm verweisen.  Sie müssen dann festlegen, in den Eigenschaften für jedes Medienobjekt der "in Ausgabeverzeichnis kopieren" auf "Kopieren Wenn neuere", ist sicherzustellen, dass Ihre Daten vorhanden sind.

Lassen Sie uns erklären Sie, was wir hier auf.

Zum Starten der Anwendungsstatus rufen Sie die Initialisierungsfunktion Engine, gefolgt vom Erstellen einer neuen Instanz der Anwendungsklasse wie folgt:

```csharp
new MySample().Run();
```

Die Laufzeit ruft der `Setup` und `Start` Methoden.  Wenn Sie außer Kraft setzen `Setup` können Sie die Modul-Parameter (nicht mehr anzeigen in diesem Beispiel) konfigurieren.

Sie überschreiben müssen `Start` wie Ihr Spiel hierdurch.  Bei dieser Methode laden Ihre Medienobjekte, verbinden Sie Ereignishandler, die Szene einrichten und starten alle Aktionen, die Sie verwendet werden soll.  In diesem Beispiel erstellen wir beide ein wenig von der Benutzeroberfläche, um den Benutzer sowie das Einrichten einer 3D-Szene anzuzeigen.

Im folgenden Codeabschnitt verwendet der Benutzeroberflächen-Framework, um ein Textelement erstellen und die Anwendung hinzuzufügen:

```csharp
// UI text
var helloText = new Text()
{
    Value = "Hello World from UrhoSharp",
    HorizontalAlignment = HorizontalAlignment.Center,
    VerticalAlignment = VerticalAlignment.Center
};
helloText.SetColor(new Color(0f, 1f, 1f));
helloText.SetFont(
    font: ResourceCache.GetFont("Fonts/Font.ttf"),
    size: 30);
UI.Root.AddChild(helloText);
```

Benutzeroberflächen-Framework gibt es eine sehr einfache Benutzeroberfläche für die im Spiel bereitstellen, und durch Hinzufügen neuer Knoten, um die Funktionsweise der [ `UI.Root` ](https://developer.xamarin.com/api/property/Urho.Gui.UI.Root/) Knoten.

Der zweite Teil unserer Beispiel-Setups main der Szene haben.  Dies umfasst eine Reihe von Schritten, Erstellen einer 3D-Szene, erstellen ein 3D Feld auf dem Bildschirm eine helle, Kamera und einen Viewport hinzuzufügen.  Diese werden ausführlicher im Abschnitt untersucht [Szene, Knoten, Komponenten und Kameras](~/graphics-games/urhosharp/using.md#scenenodescomponentsandcameras).

Der dritte Teil von diesem Beispiel wird eine Reihe von Aktionen ausgelöst.  Aktionen sind Rezepte, die einen bestimmten Effekt beschreiben, und sie nach der Erstellung von einem Knoten bei Bedarf ausgeführt werden können, durch Aufrufen der [ `RunActionAsync` ](https://developer.xamarin.com/api/member/Urho.Node.RunActionsAsync) Methode auf eine `Node`.

Die erste Aktion wird das Feld mit einem springenden Effekt skaliert und dem zweiten Ausdruck das Feld immer dreht:

```csharp
await boxNode.RunActionsAsync(
    new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1)));
```

Die oben zeigt, wie die erste Aktion, die wir erstellen eine [ `ScaleTo` ](https://developer.xamarin.com/api/type/Urho.Actions.ScaleTo/) Aktion, dies ist lediglich ein Grund, der angibt, dass Sie für eine zweite für den Wert einer Eigenschaft "Skalierung" eines Knotens skalieren möchten.  Hierdurch wiederum wird umgibt eine Beschleunigungsfunktionen Aktion die [ `EaseBounceOut` ](https://developer.xamarin.com/api/type/Urho.Actions.EaseBounceInOut/) Aktion.  Die Beschleunigungsfunktionen Aktionen im Simulator beeinträchtigt werden der linearen Ausführung einer Aktion und Anwenden von Effekten, in diesem Fall bietet es den Effekt verarbeit Out.
Damit Sie unsere Rezept wie folgt geschrieben werden konnte:

```csharp
var recipe = new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1));
```

Sobald das Rezept erstellt wurde, wird das Rezept ausgeführt:

```csharp
await boxNode.RunActionsAsync (recipe)
```

Der "await" gibt an, dass die sollten mit der Ausführung nach dieser Zeile fortgesetzt, wenn die Aktion abgeschlossen ist.  Nach Abschluss die Aktion auslösen wir die zweite Animation.

Die [UrhoSharp verwenden](~/graphics-games/urhosharp/using.md) Dokument wird ausführlicher erklärt, die Konzepte hinter Urho und wie Sie Code zum Erstellen eines Spiels zu strukturieren.

## <a name="copyrights"></a>Urheberrechte

Diese Dokumentation umfasst den ursprünglichen Inhalt von Xamarin Inc, jedoch umfassend in der open-Source-Dokumentation für das Projekt Urho3D zeichnet und Screenshots aus dem Cocos2D-Projekt enthält.

### <a name="related-links"></a>Verwandte Links

- [Planeten Erde Arbeitsmappe](https://developer.xamarin.com/workbooks/graphics/urhosharp/planetearth/planetearth.workbook)
- [Untersuchen die Arbeitsmappe Koordinaten](https://developer.xamarin.com/workbooks/graphics/urhosharp/coordinates/ExploringUrhoCoordinates.workbook)
