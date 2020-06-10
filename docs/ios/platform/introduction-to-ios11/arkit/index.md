---
title: Einführung in Arkit in xamarin. IOS
description: In diesem Dokument wird die erweiterte Realität in ios 11 mit Arkit beschrieben. Darin wird erläutert, wie Sie ein 3D-Modell zu einer APP hinzufügen, die Ansicht konfigurieren, einen Sitzungs Delegaten implementieren, das 3D-Modell auf der Welt positionieren und die Sitzung für die erweiterte Realität anhalten.
ms.prod: xamarin
ms.assetid: 70291430-BCC1-445F-9D41-6FBABE87078E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/30/2017
ms.openlocfilehash: 51b28ec05af91dea21b1291956de30c549b1868e
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571674"
---
# <a name="introduction-to-arkit-in-xamarinios"></a>Einführung in Arkit in xamarin. IOS

_Erweiterte Realität für IOS 11_

Arkit ermöglicht eine Vielzahl von erweiterten Reality-Anwendungen und-spielen. Dieser Abschnitt enthält die folgenden Themen:

- [Einstieg in Arkit](#gettingstarted)
- [Verwenden von Arkit mit urhusharp](urhosharp.md)

<a name="gettingstarted"></a>

## <a name="getting-started-with-arkit"></a>Einstieg in Arkit

Für den Einstieg in die erweiterte Realität durchlaufen die folgenden Anweisungen eine einfache Anwendung: Positionieren eines 3D-Modells und lassen des Modells mit der Überwachungs Funktionalität.

![Jet-3D-Modell in Kamerabild](images/jet-sml.png)

### <a name="1-add-a-3d-model"></a>1. Hinzufügen eines 3D-Modells

Assets sollten mit der **scenekitasset** -Buildaktion dem Projekt hinzugefügt werden.

![Scenekit-Assets in einem Projekt](images/scene-assets.png)

### <a name="2-configure-the-view"></a>2. Konfigurieren der Ansicht

Laden Sie in der-Methode des Ansichts Controllers `ViewDidLoad` das Szene Objekt, und legen Sie die- `Scene` Eigenschaft für die Ansicht fest:

```csharp
ARSCNView SceneView = (View as ARSCNView);

// Create a new scene
var scene = SCNScene.FromFile("art.scnassets/ship");

// Set the scene to the view
SceneView.Scene = scene;
```

### <a name="3-optionally-implement-a-session-delegate"></a>3. implementieren Sie optional einen Sitzungs Delegaten.

Obwohl es für einfache Fälle nicht erforderlich ist, kann die Implementierung eines Sitzungs Delegaten hilfreich sein, um den Status der Arkit-Sitzung (und in realen Anwendungen, das Feedback für den Benutzer bereitzustellen) zu debuggen. Erstellen Sie mithilfe des folgenden Codes einen einfachen Delegaten:

```csharp
public class SessionDelegate : ARSessionDelegate
{
  public SessionDelegate() {}
  public override void CameraDidChangeTrackingState(ARSession session, ARCamera camera)
  {
    Console.WriteLine("{0} {1}", camera.TrackingState, camera.TrackingStateReason);
  }
}
```

Weisen Sie den Delegaten in der- `ViewDidLoad` Methode zu:

```csharp
// Track changes to the session
SceneView.Session.Delegate = new SessionDelegate();
```

### <a name="4-position-the-3d-model-in-the-world"></a>4. Positionieren Sie das 3D-Modell weltweit.

In `ViewWillAppear` wird mit dem folgenden Code eine Arkit-Sitzung eingerichtet und die Position des 3D-Modells in Bezug auf die Kamera des Geräts festgelegt:

```csharp
// Create a session configuration
var configuration = new ARWorldTrackingConfiguration {
  PlaneDetection = ARPlaneDetection.Horizontal,
  LightEstimationEnabled = true
};

// Run the view's session
SceneView.Session.Run(configuration, ARSessionRunOptions.ResetTracking);

// Find the ship and position it just in front of the camera
var ship = SceneView.Scene.RootNode.FindChildNode("ship", true);

ship.Position = new SCNVector3(2f, -2f, -9f);
```

Jedes Mal, wenn die Anwendung ausgeführt wird oder fortgesetzt wird, wird das 3D-Modell vor der Kamera positioniert. Nachdem das Modell positioniert ist, verschieben Sie die Kamera, und beobachten Sie, wie das Modell von Arkit positioniert wird.

### <a name="5-pause-the-augmented-reality-session"></a>5. Anhalten der Sitzung für die erweiterte Realität

Es empfiehlt sich, die Arkit-Sitzung anzuhalten, wenn der Ansichts Controller nicht sichtbar ist (in der- `ViewWillDisappear` Methode:

```csharp
SceneView.Session.Pause();
```

## <a name="summary"></a>Zusammenfassung

Der obige Code ergibt eine einfache Arkit-Anwendung. Komplexere Beispiele erwarten, dass der Ansichts Controller, der die erweiterte Reality-Sitzung gehostet `IARSCNViewDelegate` , implementieren und zusätzliche Methoden implementiert werden.

Arkit bietet viele anspruchsvollere Features, wie z. b. Oberflächen Überwachung und Benutzerinteraktion. Ein Beispiel für die Kombination der Arkit-Überwachung mit urhusharp finden Sie in der [urhusharp-Demo](urhosharp.md) .

## <a name="related-links"></a>Verwandte Links

- [Erweiterte Realität (Apple)](https://developer.apple.com/arkit/)
- [Verwenden von Arkit mit urhusharp](urhosharp.md)
- [Beispiel für einfaches Arkit (Jet)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-arkitsample)
- [Arkit-Objekte, die Objekte platzieren (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-arkitplacingobjects)
- [Einführung in Arkit-Augmented Reality for IOS (WWDC) (Video)](https://developer.apple.com/videos/play/wwdc2017/602/)
