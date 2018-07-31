---
title: Einführung in die ARKit in Xamarin.iOS
description: Dieses Dokument beschreibt die in iOS 11 mit ARKit augmented Reality-Modus. Es wird erläutert, wie ein 3D-Modell zu einer app hinzufügen, konfigurieren Sie die Ansicht, implementieren Sie einen Delegaten für die Sitzung, positionieren in der Welt das 3D-Modell und Anhalten der Sitzungs augmented Reality-Modus.
ms.prod: xamarin
ms.assetid: 70291430-BCC1-445F-9D41-6FBABE87078E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2017
ms.openlocfilehash: 14bbb35477c098738e9cd7e2cb92154422d394ee
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350614"
---
# <a name="introduction-to-arkit-in-xamarinios"></a>Einführung in die ARKit in Xamarin.iOS

_Augmented Reality-Modus für iOS 11_

ARKit ermöglicht eine Vielzahl von augmented Reality-Anwendungen und spielen. In diesem Abschnitt werden die folgenden Themen behandelt:

- [Erste Schritte mit ARKit](#gettingstarted)
- [Verwenden ARKit mit von UrhoSharp](urhosharp.md)

<a name="gettingstarted" />

## <a name="getting-started-with-arkit"></a>Erste Schritte mit ARKit

Um den ersten Schritten mit augmented Reality-Modus in den folgenden Anweisungen, die über eine einfache Anwendung geführt: Positionieren eines 3D-Modells und lassen Sie ARKit, die das Modell mit den Funktionen für die nachverfolgung beibehalten.

![Jet-3D-Modell Gleitkommazahl in der Kamera image](images/jet-sml.png)

### <a name="1-add-a-3d-model"></a>1. Fügen Sie ein 3D-Modell hinzu

Ressourcen des Projekts mit hinzugefügt werden sollen die **SceneKitAsset** Buildvorgang.

![SceneKit-Ressourcen in einem Projekt](images/scene-assets.png)


### <a name="2-configure-the-view"></a>2. Konfigurieren Sie die Ansicht

In des ansichtscontrollers `ViewDidLoad` -Methode, laden Sie das Medienobjekt Szene, und legen Sie die `Scene` Eigenschaft in der Ansicht:

```csharp
ARSCNView SceneView = (View as ARSCNView);

// Create a new scene
var scene = SCNScene.FromFile("art.scnassets/ship");

// Set the scene to the view
SceneView.Scene = scene;
```

### <a name="3-optionally-implement-a-session-delegate"></a>3. Optional können Sie einen Delegaten für die Sitzung Implementieren

Auch in einfachen Fällen nicht erforderlich ist, kann die Implementierung eines Delegaten für die Sitzung für das Debuggen des Status der Sitzung ARKit (und in realen Anwendungen, die Senden von Feedback an den Benutzer) hilfreich sein. Erstellen Sie einen einfachen Stellvertreter an, mit dem folgenden Code:

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

Weisen Sie den Delegaten in der in der `ViewDidLoad` Methode:

```csharp
// Track changes to the session
SceneView.Session.Delegate = new SessionDelegate();
```

### <a name="4-position-the-3d-model-in-the-world"></a>4. Positionieren Sie das 3D-Modell in der ganzen Welt

In `ViewWillAppear`, der folgende Code richtet eine Sitzung ARKit und legt die Position der 3D-Modell im Raum relativ zu das Gerät die Kamera fest:

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

Jedes Mal die Anwendung ausgeführt oder fortgesetzt wird, wird vor der Kamera 3D-Modell positioniert werden. Sobald das Modell befindet, Verschieben der Kamera, und beobachten, wie ARKit Modell positioniert wird.

### <a name="5-pause-the-augmented-reality-session"></a>5. Anhalten der Sitzung augmented Reality-Modus

Es wird empfohlen, die Sitzung ARKit anhalten, wenn nicht der ansichtscontroller sichtbar ist (in der `ViewWillDisappear` Methode:

```csharp
SceneView.Session.Pause();
```

## <a name="summary"></a>Zusammenfassung

Der obige Code führt zu einer einfachen ARKit-Anwendung. Komplexere Beispiele würde erwarten, dass den ansichtscontroller, die die augmented Reality-Sitzung implementiert `IARSCNViewDelegate`, und zusätzliche Methoden implementiert werden.

ARKit bietet viele komplexerer Features, z. B. Surface nachverfolgen und Eingreifen des Benutzers. Finden Sie unter den [von UrhoSharp Demo](urhosharp.md) verdeutlicht, die nachverfolgung mit von UrhoSharp ARKit kombinieren.


## <a name="related-links"></a>Verwandte Links

- [Augmented Reality-Modus (Apple)](https://developer.apple.com/arkit/)
- [Verwenden ARKit mit von UrhoSharp](urhosharp.md)
- [Beispiel für einfache ARKit (Jet)](https://developer.xamarin.com/samples/monotouch/ios11/ARKitSample/)
- [ARKit Platzieren von Objekten (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios11/ARKitPlacingObjects/)
- [Einführung in ARKit - Augmented Reality-Modus für iOS (WWDC) (Video)](https://developer.apple.com/videos/play/wwdc2017/602/)
