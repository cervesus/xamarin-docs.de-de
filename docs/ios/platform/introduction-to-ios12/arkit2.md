---
title: ARKit 2 in Xamarin.iOS
description: Dieses Dokument beschreibt die Updates für ARKit in iOS 12. Er konzentriert sich auf Verweisobjekte und Bilder für die Erkennung enthält Code für die Umwelt Texturen und werden häufige Probleme bei der ARKit Programmierung erläutert.
ms.prod: xamarin
ms.assetid: af758092-1523-4ab7-aa53-c37a81fb156a
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/22/2018
ms.openlocfilehash: 7f3c196eafd71e8571ea49a17784e5290e7ef44e
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233601"
---
# <a name="arkit-2-in-xamarinios"></a>ARKit 2 in Xamarin.iOS

ARKit so weit gereift erheblich seit der Einführung des Vorjahres in iOS 11. Zunächst können Sie jetzt vertikale und horizontale Ebenen erkennen die verbessert erheblich die Durchführbarkeit ruhiges augmented Reality-Umgebungen. Darüber hinaus stehen die neue Funktionen:

* Erkennen von referenzimages und Objekte als die Verknüpfung zwischen der realen Welt und digitaler Bilddaten
* Einen neuen Beleuchtung-Modus, der simuliert reale Beleuchtung
* Die Möglichkeit, freigeben und Beibehalten von AR-Umgebungen
* Ein neues Dateiformat, das zum Speichern von AR-Inhalten bevorzugt

## <a name="recognizing-reference-objects"></a>Reference-Objekte erkennen

Ein Showcase Feature ARKit 2 ist die Möglichkeit zur Erkennung von referenzimages und Objekte. Referenzimages von normalen Bilddateien geladen werden können ([weiter unten erläutert](#more-tracking-configurations)), aber Objekte mit überprüft werden müssen, die auf Entwickler ausgerichteten Verweis [ `ARObjectScanningConfiguration` ](xref:ARKit.ARObjectScanningConfiguration).

### <a name="sample-app-scanning-and-detecting-3d-objects"></a>Beispiel-app: Überprüfen und Erkennen von 3D-Objekten

Die [Scannen und Erkennen von 3D-Objekten](https://developer.xamarin.com/samples/monotouch/ios12/ScanningAndDetecting3DObjects/) Beispiel ist Bestandteil einer [Apple-Projekt](https://developer.apple.com/documentation/arkit/scanning_and_detecting_3d_objects?language=objc) veranschaulicht, dass:

* Mithilfe von Application State Management [ `NSNotification` ](xref:Foundation.NSNotification) Objekte
* Benutzerdefinierter Visualisierungen
* Komplexe Gesten
* Objekt-Überprüfung
* Speichern einer [`ARReferenceObject`](xref:ARKit.ARReferenceObject)

Scannen ein Verweisobjekt Akku und prozessorintensiv ist, und ältere Geräte haben oft Probleme beim stabile Überwachung.

### <a name="state-management-using-nsnotification-objects"></a>Zustandsverwaltung verwenden NSNotification-Objekten

Diese Anwendung verwendet einen Statuscomputer, der wechselt zwischen den folgenden Status:

* `AppState.StartARSession`
* `AppState.NotReady`
* `AppState.Scanning`
* `AppState.Testing`

Und außerdem wird ein embedded Reihe von Zuständen und geht in `AppState.Scanning`:

* `Scan.ScanState.Ready`
* `Scan.ScanState.DefineBoundingBox`
* `Scan.ScanState.Scanning`
* `Scan.ScanState.AdjustingOrigin`

Die app verwendet eine reaktive Architektur, der Statusübergang Benachrichtigungen sendet [ `NSNotificationCenter` ](xref:Foundation.NSNotificationCenter) und diese Benachrichtigungen abonniert. Das Setup sieht dieser Ausschnitt `ViewController.cs`:

```csharp
// Configure notifications for application state changes
var notificationCenter = NSNotificationCenter.DefaultCenter;

notificationCenter.AddObserver(Scan.ScanningStateChangedNotificationName, State.ScanningStateChanged);
notificationCenter.AddObserver(ScannedObject.GhostBoundingBoxCreatedNotificationName, State.GhostBoundingBoxWasCreated);
notificationCenter.AddObserver(ScannedObject.GhostBoundingBoxRemovedNotificationName, State.GhostBoundingBoxWasRemoved);
notificationCenter.AddObserver(ScannedObject.BoundingBoxCreatedNotificationName, State.BoundingBoxWasCreated);
notificationCenter.AddObserver(BoundingBox.ScanPercentageChangedNotificationName, ScanPercentageChanged);
notificationCenter.AddObserver(BoundingBox.ExtentChangedNotificationName, BoundingBoxExtentChanged);
notificationCenter.AddObserver(BoundingBox.PositionChangedNotificationName, BoundingBoxPositionChanged);
notificationCenter.AddObserver(ObjectOrigin.PositionChangedNotificationName, ObjectOriginPositionChanged);
notificationCenter.AddObserver(NSProcessInfo.PowerStateDidChangeNotification, DisplayWarningIfInLowPowerMode);

```

Ein typische Benachrichtigung-Handler die Benutzeroberfläche aktualisiert und ändert Sie gegebenenfalls den Zustand der Anwendung an, wie z. B. dieser Handler, die aktualisiert, wenn das Objekt überprüft wird:

```csharp
private void ScanPercentageChanged(NSNotification notification)
{
    var pctNum = TryGet<NSNumber>(notification.UserInfo, BoundingBox.ScanPercentageUserKey);
    if (pctNum == null)
    {
        return;
    }
    double percentage = pctNum.DoubleValue;
    // Switch to the next state if scan is complete
    if (percentage >= 100.0)
    {
        State.SwitchToNextState();
    }
    else
    {
        DispatchQueue.MainQueue.DispatchAsync(() => navigationBarController.SetNavigationBarTitle($"Scan ({percentage})"));
    }
}

```

Zum Schluss `Enter{State}` Methoden das Modell und die UX in den neuen Zustand entsprechend zu ändern:

```csharp
internal void EnterStateTesting()
{
    navigationBarController.SetNavigationBarTitle("Testing");
    navigationBarController.ShowBackButton(false);
    loadModelButton.Hidden = true;
    flashlightButton.Hidden = false;
    nextButton.Enabled = true;
    nextButton.SetTitle("Share", UIControlState.Normal);

    testRun = new TestRun(sessionInfo, sceneView);
    TestObjectDetection();
    CancelMaxScanTimeTimer();
}
```

### <a name="custom-visualization"></a>Benutzerdefinierter Visualisierungen

Die app-zeigt die Low-Level "Punkt-Cloud" des Objekts in einem umgebenden Feld enthalten, die auf eine erkannte horizontale Ebene projiziert wird.

Diese Cloud Punkt steht Entwicklern in der [ `ARFrame.RawFeaturePoints` ](xref:ARKit.ARFrame.RawFeaturePoints) Eigenschaft. Die Punkt Cloud effizient zu visualisieren, kann ein gravierendes Problem sein. Durchlaufen die Punkte, würde dann erstellen und platzieren einen neuen SceneKit-Knoten für jeden Punkt die Framerate beenden. Auch wenn asynchron erfolgt, wäre es eine Verzögerung. Das Beispiel verwaltet der Leistung mit einer Strategie für die drei Teilen:

* Verwenden von unsicherem Code für das anheften die Daten in platzieren und die Interpretation der Daten Form eines Rohdatenpuffers von Bytes.
* Konvertieren diesen unformatierten Puffer in eine [ `SCNGeometrySource` ](xref:SceneKit.SCNGeometrySource) und erstellen ein "Template" [ `SCNGeometryElement` ](xref:SceneKit.SCNGeometryElement) Objekt.
* Schnell zusammenfügen"" die unformatierten Daten und die Vorlage über [`SCNGeometry.Create(SCNGeometrySource[], SCNGeometryElement[])`](xref:SceneKit.SCNGeometry.Create(SceneKit.SCNGeometrySource[],SceneKit.SCNGeometryElement[]))

```csharp
internal static SCNGeometry CreateVisualization(NVector3[] points, UIColor color, float size)
{
  if (points.Length == 0)
  {
    return null;
  }

  unsafe
  {
    var stride = sizeof(float) * 3;

    // Pin the data down so that it doesn't move
    fixed (NVector3* pPoints = &amp;points[0])
    {
      // Important: Don't unpin until after `SCNGeometry.Create`, because geometry creation is lazy

      // Grab a pointer to the data and treat it as a byte buffer of the appropriate length
      var intPtr = new IntPtr(pPoints);
      var pointData = NSData.FromBytes(intPtr, (System.nuint) (stride * points.Length));

      // Create a geometry source (factory) configured properly for the data (3 vertices)
      var source = SCNGeometrySource.FromData(
        pointData,
        SCNGeometrySourceSemantics.Vertex,
        points.Length,
        true,
        3,
        sizeof(float),
        0,
        stride
      );

      // Create geometry element
      // The null and bytesPerElement = 0 look odd, but this is just a template object
      var template = SCNGeometryElement.FromData(null, SCNGeometryPrimitiveType.Point, points.Length, 0);
      template.PointSize = 0.001F;
      template.MinimumPointScreenSpaceRadius = size;
      template.MaximumPointScreenSpaceRadius = size;

      // Stitch the data (source) together with the template to create the new object
      var pointsGeometry = SCNGeometry.Create(new[] { source }, new[] { template });
      pointsGeometry.Materials = new[] { Utilities.Material(color) };
      return pointsGeometry;
    }
  }
}
```

Das Ergebnis sieht folgendermaßen aus:

![point_cloud](images/arkit_point_cloud.jpeg)

### <a name="complex-gestures"></a>Komplexe Gesten

Der Benutzer kann zu skalieren, drehen, und ziehen Sie das umgebende Feld, das das Zielobjekt umgibt. Es gibt zwei interessante Elemente in die zugeordnete stiftbewegung Erkennungen.

Zunächst aktiviert alle die Bewegung freihanderkennung nur, wenn ein Schwellenwert übergeben wurde; Klicken Sie z. B. ein Finger hat so viele Pixel gezogen, oder die Drehung überschreitet einige Winkel. Die Technik besteht darin, die Verschiebung werden gesammelt, bis der Schwellenwert wurde überschritten, und klicken Sie dann inkrementell anwenden:

```csharp
// A custom rotation gesture recognizer that fires only when a threshold is passed
internal partial class ThresholdRotationGestureRecognizer : UIRotationGestureRecognizer
{
    // The threshold after which this gesture is detected.
    const double threshold = Math.PI / 15; // (12°)

    // Indicates whether the currently active gesture has exceeded the threshold
    private bool thresholdExceeded = false;

    private double previousRotation = 0;
    internal double RotationDelta { get; private set; }

    internal ThresholdRotationGestureRecognizer(IntPtr handle) : base(handle)
    {
    }

    // Observe when the gesture's state changes to reset the threshold
    public override UIGestureRecognizerState State
    {
        get => base.State;
        set
        {
            base.State = value;

            switch(value)
            {
                case UIGestureRecognizerState.Began :
                case UIGestureRecognizerState.Changed :
                    break;
                default :
                    // Reset threshold check
                    thresholdExceeded = false;
                    previousRotation = 0;
                    RotationDelta = 0;
                    break;
            }
        }
    }

    public override void TouchesMoved(NSSet touches, UIEvent evt)
    {
        base.TouchesMoved(touches, evt);

        if (thresholdExceeded)
        {
            RotationDelta = Rotation - previousRotation;
            previousRotation = Rotation;
        }

        if (! thresholdExceeded && Math.Abs(Rotation) > threshold)
        {
            thresholdExceeded = true;
            previousRotation = Rotation;
        }
    }
}
```

Der zweite interessante Aspekt in Bezug auf Aktionen zu erledigende ist die Möglichkeit, der das umgebende Feld in Bezug auf verschoben wird, reale Ebenen erkannt. Dieser Aspekt wird weiter in [in diesem Blogbeitrag von Xamarin](https://blog.xamarin.com/exploring-new-ios-12-arkit-capabilities-with-xamarin/).

## <a name="other-new-features-in-arkit-2"></a>Weitere neue Features in ARKit 2

### <a name="more-tracking-configurations"></a>Weitere Überwachung-Konfigurationen

Jetzt können Sie eine der folgenden als Grundlage für ein mixed Reality-Erfahrung verwenden:

* Nur die Geräte Beschleunigungsmesser ([`AROrientationTrackingConfiguration`](xref:ARKit.AROrientationTrackingConfiguration), iOS 11)
* Gesichter ([`ARFaceTrackingConfiguration`](xref:ARKit.ARFaceTrackingConfiguration), iOS 11)
* Verweisen auf Bilder ([`ARImageTrackingConfiguration`](xref:ARKit.ARImageTrackingConfiguration), iOS, 12)
* Scannen 3D-Objekte ([`ARObjectScanningConfiguration`](xref:ARKit.ARObjectScanningConfiguration), iOS, 12)
* Visual eingestellten Odometry ([`ARWorldTrackingConfiguration`](xref:ARKit.ARWorldTrackingConfiguration), verbesserte in iOS-12)

`AROrientationTrackingConfiguration`, ausführlicher [in diesem Blogbeitrag und F# Beispiel](https://github.com/lobrien/FSharp_Face_AR), die am stärksten begrenzt ist, und bietet eine schlechte mixed Reality-Erfahrung, es wird nur digitale Objekte in Bezug auf das Gerät während der Übertragung, ohne zu versuchen, verknüpfen das Gerät als auch in der realen Welt.

Die `ARImageTrackingConfiguration` ermöglicht es Ihnen, erkennen reale 2D-Bilder (Originalwerke, Logos usw.) und verwenden diese digitaler Bilddaten Anker:

```csharp
var imagesAndWidths = new[] {
    ("cover1.jpg", 0.185F),
    ("cover2.jpg", 0.185F),
     //...etc...
    ("cover100.jpg", 0.185F),
};

var referenceImages = new NSSet<ARReferenceImage>(
    imagesAndWidths.Select( imageAndWidth =>
    {
      // Tuples cannot be destructured in lambda arguments
        var (image, width) = imageAndWidth;
        // Read the image
        var img = UIImage.FromFile(image).CGImage;
        return new ARReferenceImage(img, ImageIO.CGImagePropertyOrientation.Up, width);
    }).ToArray());

configuration.TrackingImages = referenceImages;
```

Es gibt zwei interessante Aspekte dieser Konfiguration:

* Er ist effizient und kann verwendet werden, mit einer potenziell großen Anzahl an Referenzabbildern
* Die digitale Bilddaten ist auf das Bild, verankert, selbst wenn das Image in der realen Welt bewegt (z. B. wenn die Abdeckung eines Buchs erkannt wird, es verfolgt das Buch, wie es ohne weitere Anpassung, angeordnet nach unten usw. abgerufen werden.).

Die `ARObjectScanningConfiguration` wurde diskutiert [zuvor](#recognizing-reference-objects) und ist eine Entwickler-Konfiguration für die Überprüfung von 3D-Objekten. Es ist ausgesprochen Prozessor- und Akku rechenintensiver und sollte nicht in endbenutzeranwendungen verwendet werden. Das Beispiel [Scannen und Erkennen von 3D-Objekten](https://developer.xamarin.com/samples/monotouch/ios12/ScanningAndDetecting3DObjects/) veranschaulicht die Verwendung dieser Konfiguration.

Die endgültige Überwachungskonfiguration `ARWorldTrackingConfiguration` , ist das Arbeitspferd in den meisten mixed Reality-Funktionen. Diese Konfiguration verwendet "visual eingestellten Odometry" echten "Feature Punkte" mit digitaler Bilddaten zu verbinden. Digitale Geometrie Sprites werden relativ zur horizontalen und vertikalen Ebenen von realen verankert oder relativ zum erkannt `ARReferenceObject` Instanzen. In dieser Konfiguration Welt stammt aus der Kamera ursprüngliche Position im Raum der z-Achse auf Schwerkraft ausgerichtet und digitale Objekte "Position bleibt" relativ zu Objekten in der realen Welt.

### <a name="environmental-texturing"></a>Environmental Texturen

ARKit-2 unterstützt "environmental Texturen", die erfassten Bilder zum Schätzen der Beleuchtung und gelten auch Glanzlichter für beleuchtete Objekte verwendet. Environmental cubemaps wird dynamisch erstellt, nach oben und nach die Kamera in alle Richtungen ausgestrahlt, geprüft hat, kann eine unglaublich realistische Benutzeroberfläche erstellen:

![Umgebung mit Demo-Bild](images/arkit_env_texturing.png)

Um die Umwelt Texturen verwenden:

* Ihre [ `SCNMaterial` ](xref:SceneKit.SCNMaterial) Objekte müssen verwenden [ `SCNLightingModel.PhysicallyBased` ](xref:SceneKit.SCNLightingModel.PhysicallyBased) und weisen Sie einen Wert im Bereich von 0 bis 1 für [ `Metalness.Contents` ](xref:SceneKit.SCNMaterial.Metalness) und [ `Roughness.Contents` ](xref:SceneKit.SCNMaterialProperty.Contents) und
* Legen Sie die Überwachungskonfiguration muss [ `EnvironmentTexturing` ](xref:ARKit.ARWorldTrackingConfiguration.EnvironmentTexturing)  =  [AREnvironmentTexturing.Automatic "](xref:ARKit.AREnvironmentTexturing.Automatic) :

```csharp
var sphere = SCNSphere.Create(0.33F);
sphere.FirstMaterial.LightingModelName = SCNLightingModel.PhysicallyBased;
// Shiny metallic sphere
sphere.FirstMaterial.Metalness.Contents = new NSNumber(1.0F);
sphere.FirstMaterial.Roughness.Contents = new NSNumber(0.0F);

// Session configuration:
var configuration = new ARWorldTrackingConfiguration
{
    PlaneDetection = ARPlaneDetection.Horizontal | ARPlaneDetection.Vertical,
    LightEstimationEnabled = true,
    EnvironmentTexturing = AREnvironmentTexturing.Automatic
};
```

Die perfekt reflektierende Textur, die im vorherigen Codeausschnitt gezeigt Spaß in einem Beispiel ist environmental Texturen zwar wahrscheinlich besser mit zeigen verwendet werden, damit sie eine "Meine Valley"-Antwort (die Textur ist lediglich eine Schätzung, die basierend auf welche die Kamera auslösen aufgezeichnet).


### <a name="shared-and-persistent-ar-experiences"></a>Freigegebene und beständige AR-Umgebungen

Eine weitere wichtige Neuerung ARKit 2 ist die [ `ARWorldMap` ](xref:ARKit.ARWorldMap) -Klasse, die ermöglicht Ihnen das Freigeben oder speichern die World-Überwachungsdaten. Sie erhalten die aktuelle Weltkarte mit [ `ARSession.GetCurrentWorldMapAsync` ](xref:ARKit.ARSession.GetCurrentWorldMapAsync) oder [ `GetCurrentWorldMap(Action<ARWorldMap,NSError>` ](xref:ARKit.ARSession.GetCurrentWorldMap(System.Action{ARKit.ARWorldMap,Foundation.NSError})) :

```csharp
// Local storage
var PersistentWorldPath => Environment.GetFolderPath(Environment.SpecialFolder.Personal) + "/arworldmap";

// Later, after scanning the environment thoroughly...
var worldMap = await Session.GetCurrentWorldMapAsync();
if (worldMap != null)
{
    var data = NSKeyedArchiver.ArchivedDataWithRootObject(worldMap, true, out var err);
    if (err != null)
    {
        Console.WriteLine(err);
    }
    File.WriteAllBytes(PersistentWorldPath, data.ToArray());
}
```

Zum Freigeben oder Wiederherstellen der Weltkarte:

1. Laden Sie die Daten aus der Datei,
2. Hochgeladenen ihn in ein `ARWorldMap` Objekt
3. Verwenden Sie, die als Wert für die [ `ARWorldTrackingConfiguration.InitialWorldMap` ](xref:ARKit.ARWorldTrackingConfiguration.InitialWorldMap) Eigenschaft:

```csharp
var data = NSData.FromArray(File.ReadAllBytes(PersistentWorldController.PersistenWorldPath));
var worldMap = (ARWorldMap)NSKeyedUnarchiver.GetUnarchivedObject(typeof(ARWorldMap), data, out var err);

var configuration = new ARWorldTrackingConfiguration
{
    PlaneDetection = ARPlaneDetection.Horizontal | ARPlaneDetection.Vertical,
    LightEstimationEnabled = true,
    EnvironmentTexturing = AREnvironmentTexturing.Automatic,
    InitialWorldMap = worldMap
};
```

Die `ARWorldMap` enthält nur die nicht sichtbaren World-Überwachungsdaten und die [ `ARAnchor` ](xref:ARKit.ARAnchor) Objekten, die zwar _nicht_ digitale Ressourcen enthalten. Geometrie oder Bilder freigeben möchten, müssen Sie Ihre eigene Strategie für Ihren Anwendungsfall geeignet zu entwickeln (z. B. durch das Speichern/übertragen nur den Speicherort und die Ausrichtung der Geometrie und deren Anwendung auf statische `SCNGeometry` oder vielleicht durch das Speichern/übertragen serialisierte Objekte). Der Vorteil der `ARWorldMap` besteht darin, dass Ressourcen, nachdem angeordnet, relativ zu einer freigegebenen `ARAnchor`, wird zwischen einzelnen Geräten oder Sitzungen konsistent angezeigt.

### <a name="universal-scene-description-file-format"></a>Universelle Beschreibung der Szene-Dateiformat

Die letzte Überschrift-Funktion von ARKit 2 ist Apple Einführung des Pixar [Universal Description der Szene](https://graphics.pixar.com/usd/docs/Introduction-to-USD.html) Dateiformat. Dieses Format ersetzt Colladas DAE-Format als das bevorzugte Format für die Freigabe und ARKit Ressourcen speichern. Unterstützung für die visuelle Darstellung der Ressourcen ist in iOS 12 und Mojave integriert. Die Dateierweiterung USDZ ist eine nicht komprimierte und nicht verschlüsselte Zip-Archiv, das US-Dollar-Dateien enthält. Pixar [bietet Tools zum Arbeiten mit Dateien mit US-Dollar](https://graphics.pixar.com/usd/docs/USD-Toolset.html#USDToolset-usdedit) , aber es ist noch nicht viel Drittanbieter-Unterstützung.

## <a name="arkit-programming-tips"></a>ARKit Programmiertipps

### <a name="manual-resource-management"></a>Manuelle ressourcenverwaltung

In ARKit ist es entscheidend, um Ressourcen manuell verwalten. Nicht nur lässt dies zu hohe Frameraten, sondern tatsächlich _erforderlichen_ vermeiden eine verwirrende "Einfrieren des Bildschirms." ARKit Framework wird lazy über einen neuen Frame mit Kamera angeben ([`ARSession.CurrentFrame`](xref:ARKit.ARSession.CurrentFrame). Bis das aktuelle [ `ARFrame` ](xref:ARKit.ARFrame) hatte `Dispose()` aufgerufen, ARKit wird nicht Geben Sie einen neuen Frame! Dies bewirkt, dass das Video "eingefroren", obwohl der Rest der app reaktionsfähig ist. Die Lösung besteht darin, immer Zugriff auf `ARSession.CurrentFrame` mit einem `using` blockieren oder manuell aufrufen `Dispose()` darauf.

Alle Objekte, die von abgeleiteten `NSObject` sind `IDisposable` und `NSObject` implementiert die [Dispose-Muster](https://docs.microsoft.com/dotnet/standard/design-guidelines/dispose-pattern), sodass Sie, in der Regel befolgen sollten [dieses Muster für die Implementierung `Dispose` auf eine abgeleitete Klasse](https://docs.microsoft.com/dotnet/standard/garbage-collection/implementing-dispose).

### <a name="manipulating-transform-matrices"></a>Bearbeiten von Transformation Matrizen

In allen 3D-Anwendungen machen Sie mit 4 x 4-Transformationsmatrizen, die relativ kompakt beschrieben, wie Sie verschieben, drehen und ein Objekt über 3D-Raum Scheren zu tun haben. In SceneKit, sind dies [ `SCNMatrix4` ](xref:SceneKit.SCNMatrix4) Objekte.  

Die [ `SCNNode.Transform` ](xref:SceneKit.SCNNode.Transform) -Eigenschaft gibt die `SCNMatrix4` Transformationsmatrix für die [ `SCNNode` ](xref:SceneKit.SCNNode) _wie durch gesichert_ der zeilengerichteter `simdfloat4x4` Typ. Also beispielsweise:

```csharp
var node = new SCNNode { Position = new SCNVector3(2, 3, 4) };  
var xform = node.Transform;
Console.WriteLine(xform);
// Output is: "(1, 0, 0, 0)\n(0, 1, 0, 0)\n(0, 0, 1, 0)\n(2, 3, 4, 1)"
```  

Wie Sie sehen können, ist die Position in der untersten Zeile der ersten drei Elemente codiert.

In Xamarin, ist der allgemeine Typ zum Bearbeiten von Transformationsmatrizen `NVector4`, der gemäß der Konvention die Spaltengröße so interpretiert wird. Das heißt, muss die translationsposition/Komponente M14, M24, M34, nicht M41, M42, M43:

![zeilengerichteter Vs Spaltengröße](images/arkit_row_vs_column.png)

Mit der Auswahl der Interpretation der Matrix konsistent ist entscheidend für das ordnungsgemäße Verhalten. Da 3D-Transformation Matrizen 4 x 4 sind, erzeugen Konsistenz Fehler keine Art von während der Kompilierung oder sogar ein Laufzeit-Ausnahme: Es ist einfach, dass Vorgänge unerwartet reagieren werden. Wenn Ihre SceneKit / ARKit Objekte hängen bleiben, fliegen entfernt oder jitter scheinen, wird eine falsche Transformationsmatrix ist eine gute Möglichkeit. Die Lösung ist einfach: [ `NMatrix4.Transpose` ](https://developer.xamarin.com/api/member/OpenTK.NMatrix4.Transpose) führt eine direkte Umsetzung von Elementen.

## <a name="related-links"></a>Verwandte Links

- [Beispiel-app – Scannen und Erkennen von 3D-Objekten](https://developer.xamarin.com/samples/monotouch/iOS12/ScanningAndDetecting3DObjects/)
- [Neuerungen in ARKit 2 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/602/)
- [Grundlegendes zu ARKit nachverfolgen und Erkennung (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/610/)
- [Einführung in die ARKit in Xamarin.iOS](https://docs.microsoft.com/xamarin/ios/platform/introduction-to-ios11/arkit/)
