---
title: Einführung in ARKit
description: Augmented-Reality-Modus für iOS 11
ms.prod: xamarin
ms.assetid: 70291430-BCC1-445F-9D41-6FBABE87078E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2016
ms.openlocfilehash: f48cdd48e63131fe234fef1bb60b555724dd8a92
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-arkit"></a>Einführung in ARKit

_Augmented-Reality-Modus für iOS 11_

ARKit ermöglicht eine Vielzahl von Anwendungen augmented Reality-Modus und Spiele. In diesem Abschnitt werden die folgenden Themen behandelt:

- [Erste Schritte mit ARKit](#gettingstarted)
- [Verwenden von ARKit mit UrhoSharp](urhosharp.md)

<a name="gettingstarted" />

## <a name="getting-started-with-arkit"></a>Erste Schritte mit ARKit

Zum Einstieg in augmented Reality-Modus führt die folgenden Anweisungen über eine einfache Anwendung: Positionieren eines 3D-Modells und lassen ARKit Remotezugriffslösung mit seinen Überwachungsfunktionen des Modells beibehalten.

![Gleitkommatyp in der Kamera Image Jet-3D-Modell](images/jet-sml.png)

### <a name="1-add-a-3d-model"></a>1. Hinzufügen eines 3D-Modells

Ressourcen des Projekts mit hinzugefügt werden sollen die **SceneKitAsset** Buildvorgang.

![SceneKit Bestand in einem Projekt](images/scene-assets.png)


### <a name="2-configure-the-view"></a>2. Konfigurieren Sie die Ansicht

In des Ansicht Controllers `ViewDidLoad` -Methode, laden Sie das Medienobjekt Szene und Festlegen der `Scene` Eigenschaft für die Sicht:

```csharp
ARSCNView SceneView = (View as ARSCNView);

// Create a new scene
var scene = SCNScene.FromFile("art.scnassets/ship");

// Set the scene to the view
SceneView.Scene = scene;
```

### <a name="3-optionally-implement-a-session-delegate"></a>3. Optional können Sie einen Delegaten für die Sitzung Implementieren

Obwohl in einfachen Fällen nicht erforderlich ist, kann die Implementieren eines Delegaten für die Sitzung für das Debuggen des Status der Sitzung ARKit (und in echten Anwendungen, Übermitteln von Feedback für den Benutzer) hilfreich sein. Erstellen Sie einen einfachen Delegaten, die mit den folgenden Code:

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

Weisen Sie die Delegaten in der in der `ViewDidLoad` Methode:

```csharp
// Track changes to the session
SceneView.Session.Delegate = new SessionDelegate();
```

### <a name="4-position-the-3d-model-in-the-world"></a>4. Positionieren Sie in der ganzen Welt 3D-Modell

In `ViewWillAppear`, der folgende Code eine ARKit-Sitzung eingerichtet und legt die Position des 3D-Modells im Raum relativ zu Kamera des Geräts:

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

Jedes Mal die Anwendung ausgeführt oder fortgesetzt wird, wird vor der Kamera 3D-Modell positioniert werden. Sobald das Modell befindet, verschieben Sie die Kamera, und beobachten Sie, wie ARKit des Modells positioniert bleiben.

### <a name="5-pause-the-augmented-reality-session"></a>5. Anhalten der Sitzung augmented Reality-Modus

Es wird empfohlen, die Sitzung ARKit anhalten, wenn die View-Controller nicht sichtbar ist (in der `ViewWillDisappear` Methode:

```csharp
SceneView.Session.Pause();
```

## <a name="summary"></a>Zusammenfassung

Der obige Code führt eine einfache ARKit-Anwendung. Komplexere Beispiele erwarten die modellansichtcontroller hostet die Sitzung augmented Reality-Modus implementieren `IARSCNViewDelegate`, und weitere Methoden implementiert werden.

ARKit bietet viele komplexere Funktionen, z. B.-Oberfläche Überwachung und Eingreifen des Benutzers. Finden Sie unter der [UrhoSharp Demo](urhosharp.md) für ein Beispiel für nachverfolgung mit UrhoSharp ARKit kombinieren.


## <a name="related-links"></a>Verwandte Links

- [Augmented-Reality-Modus (Apple)](https://developer.apple.com/arkit/)
- [Verwenden von ARKit mit UrhoSharp](urhosharp.md)
- [Beispiel für einfache ARKit (Jet)](https://developer.xamarin.com/samples/monotouch/ios11/ARKitSample/)
- [ARKit platzieren Objekte (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios11/ARKitPlacingObjects/)
- [Einführung in ARKit - Augmented-Reality-Modus für iOS (WWDC) (Video)](https://developer.apple.com/videos/play/wwdc2017/602/)
