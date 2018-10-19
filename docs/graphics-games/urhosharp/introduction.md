---
title: Eine Einführung in die von UrhoSharp
description: Dieses Dokument beschreibt die grundlegende Struktur einer von UrhoSharp-Anwendung und die Links zu verschiedenen Anleitungen und Beispielanwendungen, die Verwendung von UrhoSharp veranschaulicht.
ms.prod: xamarin
ms.assetid: 18041443-5093-4AF7-8B20-03E00478EF35
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: a3e14ebca961e828fc578035adaca5ba2a809438
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2018
ms.locfileid: "34783554"
---
# <a name="an-introduction-to-urhosharp"></a>Eine Einführung in die von UrhoSharp

![Von UrhoSharp-logo](introduction-images/urhosharp-icon.png)

Von UrhoSharp ist eine leistungsfähige 3D Spiel-Engine für Entwickler, Xamarin und .NET.  Dabei handelt es sich ähnlich wie im Geiste Apple SceneKit und SpriteKit Physik enthalten, Navigation, Netzwerke und vieles mehr zwar trotzdem plattformübergreifend wird.

Es ist eine .NET an der [Urho3D](http://urho3d.github.io/) -engine und ermöglicht es Entwicklern, plattformübergreifenden Code zu schreiben, die auf Android, iOS abzielen können, Windows und Mac mit der gleichen Codebasis und Systeme OpenGL and Direct3D und rendern können.

Von UrhoSharp ist eine Spiele-Engine mit einer Vielzahl von Funktionen zur Verfügung, das Feld an:

- Leistungsstarke 3D Grafikrendering
- [Die Physik Simulation](https://developer.xamarin.com/api/namespace/Urho.Physics/) (mithilfe der Aufzählungszeichen-Bibliothek)
- [Behandlung von Szene](https://developer.xamarin.com/api/type/Urho.Scene/)
- Async/await-Unterstützung
- [Benutzerfreundliche Aktionen API](https://developer.xamarin.com/api/namespace/Urho.Actions/)
- [2D Integration in 3D-Szenen](https://developer.xamarin.com/api/namespace/Urho.Urho2D/)
- [Schriftart-Rendering mit FreeType](https://developer.xamarin.com/api/type/Urho.Gui.FontFaceFreeType/)
- [Client- und Netzwerkfunktionen](https://developer.xamarin.com/api/namespace/Urho.Network/)
- [Importieren Sie eine Vielzahl von Ressourcen](https://developer.xamarin.com/api/namespace/Urho.Resources/) (mit Ressourcen-Bibliothek öffnen)
- [Navigation Mesh und Pathfinding](https://developer.xamarin.com/api/namespace/Urho.Navigation/) (mit neu gefasst/Umleitung)
- [Generierung der konvexen Hülle zur kollisionserkennung](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape/) (mit StanHull)
- [Audiowiedergabe](https://developer.xamarin.com/api/namespace/Urho.Audio/) (mit **Libvorbis**)

## <a name="getting-started"></a>Erste Schritte

Von UrhoSharp wird als bequem verteilt eine [NuGet-Paket](https://www.nuget.org/) und hinzugefügt werden, Ihre C#- oder f#-Projekte die Zielplattform Windows, Mac, Android oder iOS.  NuGet enthält sowohl die Bibliotheken benötigt, um das Programm auszuführen, als auch die grundlegende Ressourcen (CoreData), die von der Engine verwendet.

### <a name="urho-as-a-portable-class-library"></a>Urho als Portable Klassenbibliothek

Das Paket Urho kann entweder aus einem plattformspezifische Projekt oder ein Projekt "Portable Klassenbibliothek", in denen Sie Ihren gesamten Code auf allen Plattformen wiederverwenden können genutzt werden.  Dies bedeutet, dass alles, was Sie auf jeder Plattform durchzuführen hätten schreiben Ihre bestimmten Einstiegspunkt für die Plattform, und klicken Sie dann den freigegebenen spielcode Übertragung der Steuerung.

### <a name="samples"></a>Proben

Sie können einen Eindruck von den Funktionen der Urho abrufen, öffnen Sie in Visual Studio für Mac oder Visual Studio die Projektmappe aus:

[https://github.com/xamarin/urho-samples](https://github.com/xamarin/urho-samples)

Die Standardprojektmappe enthält Projekte für Android, iOS, Windows und Mac.  Wir haben diese Projektmappe so strukturiert, dass wir eine kleine Plattform bestimmte Startprogramm haben, und alle spielcode und Beispielcode befindet sich in einer portablen Klassenbibliothek, zeigen, wie zum Maximieren der Wiederverwendung von Code auf allen Plattformen.

Wenden Sie sich an den [Urho und Ihre Plattform](~/graphics-games/urhosharp/platform/index.md) Weitere Informationen zum Erstellen Ihrer eigenen Lösungen.

Da alle Beispiele einen gemeinsamen Satz von Elementen der Benutzeroberfläche gemeinsam nutzen, haben die Beispiele in dieser Datei die grundlegende Einrichtung abstrahiert:

[https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs](https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs)

Dies stellt eine Beispiel-Basisklasse, die einige grundlegende Tastenanschläge behandelt und Berührungsereignisse, Setups eine Kamera, enthält die grundlegenden Elemente der Benutzeroberfläche, und dies können jedes Beispiel auf die spezielle Funktionalität zu konzentrieren, die präsentiert wird, werden.

Das folgende Beispiel zeigt, wie die Engine folgendermaßen ist:

- [Samply spielen](https://github.com/xamarin/urho-samples/tree/master/SamplyGame) einen einfachen Klon des ShootySkies.

Während es sich bei den anderen Beispielen einzelne Eigenschaften für jedes Sample wird.

## <a name="basic-structure"></a>Grundlegende Struktur

Ihr Spiel sollte die Unterklasse der [`Application`](https://developer.xamarin.com/api/type/Urho.Application/)
Klasse, dies ist, in dem Sie Ihr Spiel einrichten, werden (auf der [ `Setup` ](https://developer.xamarin.com/api/member/Urho.Application.Setup/) Methode), und starten Sie Ihr Spiel (in der [ `Start` ](https://developer.xamarin.com/api/member/Urho.Application.Start) Methode).  Klicken Sie dann erstellen Sie Ihre Haupt-Benutzeroberfläche.  Wir werden durch einen kleinen Teil führen, die die APIs für das Einrichten einer 3D-Szene, einige Elemente der Benutzeroberfläche und ein einfaches Verhalten an diese angefügt wird.

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

Wenn Sie diese Anwendung ausführen, werden Sie rasch feststellen, dass die Laufzeit zu beschweren ist Ihre Ressourcen sind nicht vorhanden.  Was Sie tun müssen ist eine Hierarchie in Ihrem Projekt zu erstellen, die mit den Namen des speziellen Verzeichnisses "Daten" beginnt und in dieser, würde man die Medienobjekte, die Sie in Ihrem Programm verweisen.  Sie müssen dann festlegen, in den Elementeigenschaften für jedes Medienobjekt die "in Ausgabeverzeichnis kopieren", "Kopieren wenn neuer" werden, die Stellen Sie sicher, dass Ihre Daten vorhanden sind.

Lassen Sie uns erklären Sie, was hier passiert.

Zum Starten der Anwendung rufen Sie die Engine Initialisierungsfunktion, gefolgt von erstellen eine neue Instanz der Anwendungsklasse wie folgt:

```csharp
new MySample().Run();
```

Die Laufzeit ruft die `Setup` und `Start` Methoden.  Wenn Sie außer Kraft setzen `Setup` können Sie die Engine-Parameter (nicht mehr anzeigen in diesem Beispiel) konfigurieren.

Sie müssen überschreiben `Start` wie damit Ihr Spiel gestartet wird.  Bei dieser Methode Sie laden Sie Ihre Assets, verbinden Sie Ereignishandler, Ihrer Szene einrichten und starten alle Aktionen, die Sie wünschen.  In diesem Beispiel erstellen wir sowohl ein wenig der Benutzeroberfläche des Benutzers sowie das Einrichten einer 3D-Szene angezeigt.

Der folgende Codeabschnitt verwendet das Framework für UI ein Textelement zu erstellen, und fügen Sie es Ihrer Anwendung hinzu:

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

Das UI-Framework gibt es eine sehr einfache Benutzeroberfläche für die im Spiel bereitstellen, und dies erfolgt durch das Hinzufügen neuer Knoten, die [ `UI.Root` ](https://developer.xamarin.com/api/property/Urho.Gui.UI.Root/) Knoten.

Der zweite Teil unserer Beispiel-Setups die main-Szene.  Dies umfasst eine Reihe von Schritten, die Erstellung einer 3D-Szene, erstellen ein 3D-Objekt Feld auf dem Bildschirm hinzufügen, ein Licht, eine Kamera und einen Viewport aus.  Diese werden in Abschnitt ausführlicher besprochen [Szene, Knoten, Komponenten und Kameras](~/graphics-games/urhosharp/using.md#scenenodescomponentsandcameras).

Der dritte Teil des in diesem Beispiel wird eine Reihe von Aktionen ausgelöst.  Aktionen sind Anleitungen, die ein bestimmtes Effekts zu beschreiben, und sie nach der Erstellung von einem Knoten bei Bedarf ausgeführt werden können, durch den Aufruf der [ `RunActionAsync` ](https://developer.xamarin.com/api/member/Urho.Node.RunActionsAsync) Methode für eine `Node`.

Die erste Aktion wird das Feld mit einen Sprungeffekt skaliert, und das zweite Argument wird das Feld immer:

```csharp
await boxNode.RunActionsAsync(
    new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1)));
```

Die oben zeigt, wie die erste Aktion, die wir erstellen eine [ `ScaleTo` ](https://developer.xamarin.com/api/type/Urho.Actions.ScaleTo/) Aktion, dies ist lediglich ein Rezept, der angibt, dass für eine Sekunde auf den Wert einer Eigenschaft "Skalierung" eines Knotens skaliert werden soll.  Dadurch wird wiederum um eine Aktion Migrationstoolkit umschlossen der [ `EaseBounceOut` ](https://developer.xamarin.com/api/type/Urho.Actions.EaseBounceInOut/) Aktion.  Die Ads Aktionen verzerren die lineare Ausführung einer Aktion, und wenden Sie einen Effekt, in diesem Fall bietet den springenden-Out-Effekt.
Unserer könnte also folgendermaßen geschrieben werden:

```csharp
var recipe = new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1));
```

Nachdem das Rezept erstellt wurde, führen wir das Rezept:

```csharp
await boxNode.RunActionsAsync (recipe)
```

Der "await" gibt an, dass die sollten mit der Ausführung nach dieser Zeile fortgesetzt, wenn die Aktion abgeschlossen ist.  Nach Abschluss der Aktion lösen wir die zweite Animation.

Die [Verwenden von UrhoSharp](~/graphics-games/urhosharp/using.md) Dokument werden erläuterungen zu ausführlicher Urho und wie Sie Ihren Code zum Erstellen eines Spiels zu strukturieren.

## <a name="copyrights"></a>Urheberrechte

Diese Dokumentation enthält die ursprünglichen Inhalte von Xamarin Inc., aber sehr häufig in der open-Source-Dokumentation für das Projekt Urho3D zeichnet und enthält Screenshots aus dem Projekt Cocos2D.

