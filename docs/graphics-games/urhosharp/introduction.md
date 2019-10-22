---
title: Einführung in urhusharp
description: Dieses Dokument beschreibt die grundlegende Struktur einer urhusharp-Anwendung und Links zu verschiedenen Handbüchern und Beispielanwendungen, die die Verwendung von urhusharp veranschaulichen.
ms.prod: xamarin
ms.assetid: 18041443-5093-4AF7-8B20-03E00478EF35
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 441a3cc19b4246fb2bdea54508142a894af5c051
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "67832546"
---
# <a name="introduction-to-urhosharp"></a>Einführung in urhusharp

![Urhusharp-Logo](introduction-images/urhosharp-icon.png)

Urhusharp ist eine leistungsstarke 3D-Spiel-Engine für xamarin-und .NET-Entwickler.  Es ähnelt dem von Apple scenekit und spritekit und umfasst auch die Physik, Navigation, Netzwerke und vieles mehr.

Es handelt sich um eine .net-Bindung an das [Urho3D](http://urho3d.github.io/) -Modul und ermöglicht Entwicklern das Schreiben von Platt Form übergreifendem Code, der auf Android, Ios, Windows und Mac mit der gleichen Codebasis abzielen und sowohl für OpenGL-als auch Direct3D-Systeme gerenden werden kann

Urhusharp ist eine Spiel-Engine mit vielen Funktionen, die im folgenden aufgeführt sind:

- Leistungsstarkes 3D-Grafik Rendering
- Physik-Simulation (mithilfe der Bullet-Bibliothek)
- Szenen Behandlung
- Unterstützung für warten/Async
- API für benutzerfreundliche Aktionen
- 2D-Integration in 3D-Szenen
- Schriftart Rendering mit FreeType
- Client-und Servernetzwerk Funktionen
- Importieren einer breiten Palette von Assets (mit Open Assets Library)
- Navigations Gitter und pathsuche (mithilfe von umleiten/umleiten)
- Zusammenstellung der konnevexen Hülle für die Konflikterkennung (mithilfe von stanhull)
- Audiowiedergabe (mit **libvorbis**)

## <a name="get-started"></a>Erste Schritte

Urhusharp ist bequem als [nuget-Paket](https://www.nuget.org/) verteilt und kann zu Ihren C# -oder F# -Projekten hinzugefügt werden, die auf Windows, Mac, Android oder IOS ausgerichtet sind.  Das nuget enthält sowohl die Bibliotheken, die zum Ausführen des Programms erforderlich sind, als auch die grundlegenden Ressourcen (CoreData), die von der Engine verwendet werden.

### <a name="urho-as-a-portable-class-library"></a>Urho als portable Klassenbibliothek

Das Urho-Paket kann entweder von einem plattformspezifischen Projekt oder von einem Projekt für eine portable Klassenbibliothek genutzt werden, sodass Sie den gesamten Code auf allen Plattformen wieder verwenden können.  Dies bedeutet, dass Sie für jede Plattform lediglich den plattformspezifischen Einstiegspunkt schreiben und dann die Steuerung an den freigegebenen Spiel Code übertragen müssen.

### <a name="samples"></a>Proben

Sie erhalten einen Vorgeschmack auf die Funktionen von Urho, indem Sie entweder in Visual Studio für Mac oder Visual Studio die Beispiel Projekt Mappe öffnen:

[https://github.com/xamarin/urho-samples](https://github.com/xamarin/urho-samples)

Die Standardlösung enthält Projekte für Android, Ios, Windows und Mac.  Wir haben diese Lösung strukturiert, damit wir ein kleines plattformspezifisches Start Programm haben. der gesamte Beispielcode und der Spiel Code befinden sich in einer portablen Klassenbibliothek und veranschaulichen, wie Sie die Wiederverwendung von Code auf allen Plattformen maximieren können.

Weitere Informationen zum Erstellen eigener Lösungen finden Sie auf der Seite [Urho und ihrer Plattform](~/graphics-games/urhosharp/platform/index.md) .

Da alle Beispiele einen gemeinsamen Satz von Benutzeroberflächen Elementen aufweisen, haben die Beispiele die grundlegende Einrichtung in dieser Datei abstrahiert:

[https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs](https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs)

Dies stellt eine Beispiel Basisklasse bereit, die einige grundlegende Tastatur Anschläge und Berührungs Ereignisse behandelt, eine Kamera einrichtet, grundlegende Elemente der Benutzeroberfläche bereitstellt. so kann sich jedes Beispiel auf die jeweilige Funktionalität konzentrieren, die angezeigt wird.

Das folgende Beispiel zeigt, was die Engine ausführen kann:

- [Samply Game](https://github.com/xamarin/urho-samples/tree/master/SamplyGame) ein einfacher Klon von shootyskies.

Die anderen Beispiele zeigen die einzelnen Eigenschaften der einzelnen Beispiele.

## <a name="basic-structure"></a>Grundlegende Struktur

Ihr Spiel sollte die `Application` Klasse Unterklassen, wo Sie das Spiel einrichten (bei der `Setup`-Methode) und das Spiel starten (in der `Start`-Methode).  Anschließend erstellen Sie die Hauptbenutzer Oberfläche.  Wir werden ein kleines Beispiel durchgehen, das die APIs zum Einrichten einer 3D-Szene, einige Benutzeroberflächen Elemente und das Anfügen eines einfachen Verhaltens an die APIs zeigt.

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

Wenn Sie diese Anwendung ausführen, werden Sie schnell feststellen, dass die Laufzeit nicht mehr vorhanden ist.  Sie müssen in Ihrem Projekt eine Hierarchie erstellen, die mit dem speziellen Verzeichnisnamen "Data" beginnt. in diesem Fall würden Sie die Ressourcen, auf die Sie verweisen, in Ihrem Programm platzieren.  Anschließend müssen Sie in den Element Eigenschaften für jedes Asset den Wert "in Ausgabeverzeichnis kopieren" in "kopieren, wenn neuer" festlegen, um sicherzustellen, dass Ihre Daten vorhanden sind.

Wir erläutern, was hier passiert.

Zum Starten der Anwendung rufen Sie die Initialisierungsfunktion der Engine auf, gefolgt von der Erstellung einer neuen Instanz Ihrer Anwendungsklasse, wie folgt:

```csharp
new MySample().Run();
```

Die Laufzeit ruft die `Setup`-und `Start` Methoden für Sie auf.  Wenn Sie `Setup` überschreiben, können Sie die Engine-Parameter konfigurieren (in diesem Beispiel nicht angezeigt).

Sie müssen `Start` überschreiben, da dadurch das Spiel gestartet wird.  In dieser Methode laden Sie Ihre Assets, verbinden Ereignishandler, richten ihre Szene ein und starten alle gewünschten Aktionen.  In unserem Beispiel erstellen wir sowohl eine Benutzeroberfläche, die dem Benutzer angezeigt wird, als auch eine 3D-Szene.

Im folgenden Code Abschnitt wird das UI-Framework verwendet, um ein Textelement zu erstellen und es der Anwendung hinzuzufügen:

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

Das UI-Framework ist vorhanden, um eine sehr einfache Benutzeroberfläche im Spiel zu bieten, und es funktioniert, indem dem Knoten "`UI.Root`" neue Knoten hinzugefügt werden.

Der zweite Teil des Beispiels ist die zentrale Szene.  Dies umfasst eine Reihe von Schritten, das Erstellen einer 3D-Szene, das Erstellen eines 3D-Felds im Bildschirm, das Hinzufügen eines Lichts, eine Kamera und einen Viewport.  Diese werden im Abschnitt [Szene, Knoten, Komponenten und Kameras](~/graphics-games/urhosharp/using.md#scenenodescomponentsandcameras)ausführlicher erläutert.

Der dritte Teil des Beispiels löst eine Reihe von Aktionen aus.  Aktionen sind Rezepte, die einen bestimmten Effekt beschreiben. Nachdem Sie erstellt wurden, können Sie von einem Knoten bei Bedarf ausgeführt werden, indem Sie die `RunActionAsync`-Methode für eine `Node` aufrufen.

Mit der ersten Aktion wird das Feld mit einem Sprung Effekt skaliert, und das zweite Feld dreht das Feld immer wieder:

```csharp
await boxNode.RunActionsAsync(
    new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1)));
```

Im obigen Beispiel wird gezeigt, wie die erste Aktion, die wir erstellen, eine `ScaleTo` Aktion ist. dabei handelt es sich lediglich um ein Rezept, das angibt, dass Sie die Skalierung für eine Sekunde auf den Wert einer Skalierungs Eigenschaft eines Knotens durchführen möchten.  Diese Aktion wird wiederum um eine Beschleunigungs Aktion, die `EaseBounceOut` Aktion umschlossen.  Die Beschleunigungs Aktionen verzerren die lineare Ausführung einer Aktion und wenden einen Effekt an. in diesem Fall stellt Sie den Bouncing-out-Effekt bereit.
Unser Rezept könnte also wie folgt geschrieben werden:

```csharp
var recipe = new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1));
```

Nachdem das Rezept erstellt wurde, führen wir das Rezept aus:

```csharp
await boxNode.RunActionsAsync (recipe)
```

Der Erwartungswert gibt an, dass die Ausführung nach dieser Zeile fortsetzen soll, wenn die Aktion abgeschlossen ist.  Nachdem die Aktion abgeschlossen ist, wird die zweite Animation auslöst.

Das Dokument " [using urhusharp](~/graphics-games/urhosharp/using.md) " untersucht ausführlicher die Konzepte hinter Urho und erläutert, wie Sie Ihren Code für die Erstellung eines Spiels strukturieren.

## <a name="copyrights"></a>Urheberrechte

Diese Dokumentation enthält Originalinhalte von xamarin Inc, zeichnet sich jedoch in der Open-Source-Dokumentation für das Urho3D-Projekt aus und enthält Screenshots aus dem Cocos2D-Projekt.
