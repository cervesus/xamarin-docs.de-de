---
title: Arkit 2 in xamarin. IOS
description: In diesem Dokument werden die Updates für Arkit in IOS 12 beschrieben. Der Schwerpunkt liegt auf der Verwendung von Referenzobjekten und Images für die Erkennung, der Code für die Umgebungs Texturierung und der Erörterung allgemeiner Probleme bei der Arkit-Programmierung
ms.prod: xamarin
ms.assetid: af758092-1523-4ab7-aa53-c37a81fb156a
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/22/2018
ms.openlocfilehash: 4af5e7ea9c1d744cd3b5ea5444312ba68bfcea11
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70120453"
---
# <a name="arkit-2-in-xamarinios"></a>Arkit 2 in xamarin. IOS

Arkit hat seit seiner Einführung im letzten Jahr in ios 11 erheblich gereift. Vor allem können Sie nun vertikale und horizontale Flächen erkennen, wodurch die Praktikabilität von erweiterter Reality-Umgebungen im Innenbereich erheblich verbessert wird. Außerdem gibt es neue Funktionen:

- Erkennen von Verweis Bildern und Objekten als Verbindung zwischen der realen Welt und digitalen Bildern
- Ein neuer Beleuchtungs Modus, der die reale Beleuchtung simuliert
- Die Möglichkeit zum Freigeben und beibehalten von aren Umgebungen
- Ein neues Dateiformat, das zum Speichern von aren Inhalten bevorzugt wird.

## <a name="recognizing-reference-objects"></a>Erkennen von Verweis Objekten

Ein Showcase-Feature in Arkit 2 ist die Möglichkeit, Verweis Bilder und-Objekte zu erkennen. Verweis Images können aus normalen Bilddateien geladen werden ([Siehe](#more-tracking-configurations)weiter unten). Verweis Objekte müssen jedoch mithilfe des Entwickler Fokus [`ARObjectScanningConfiguration`](xref:ARKit.ARObjectScanningConfiguration)gescannt werden.

### <a name="sample-app-scanning-and-detecting-3d-objects"></a>Beispiel-App: Scannen und erkennen von 3D-Objekten

Das Beispiel für das [Scannen und erkennen von 3D-Objekten](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-scanninganddetecting3dobjects) ist ein Port eines [Apple-Projekts](https://developer.apple.com/documentation/arkit/scanning_and_detecting_3d_objects?language=objc) , das Folgendes veranschaulicht:

- Anwendungs Zustands Verwaltung mithilfe [`NSNotification`](xref:Foundation.NSNotification) von Objekten
- Benutzerdefinierte Visualisierung
- Komplexe Gesten
- Objekt Überprüfung
- Speichern eines[`ARReferenceObject`](xref:ARKit.ARReferenceObject)

Das Scannen eines Referenz Objekts ist Batterie-und Prozessor intensiv, und ältere Geräte haben häufig Probleme beim erreichen stabiler Nachverfolgung.

### <a name="state-management-using-nsnotification-objects"></a>Zustands Verwaltung mit nsnotification-Objekten

Diese Anwendung verwendet einen Zustands Automaten, der zwischen den folgenden Zuständen übergeht:

- `AppState.StartARSession`
- `AppState.NotReady`
- `AppState.Scanning`
- `AppState.Testing`

Und verwendet zusätzlich einen eingebetteten Satz von Zuständen und Übergängen, `AppState.Scanning`wenn in:

- `Scan.ScanState.Ready`
- `Scan.ScanState.DefineBoundingBox`
- `Scan.ScanState.Scanning`
- `Scan.ScanState.AdjustingOrigin`

Die APP verwendet eine reaktive Architektur, die Zustands Übergangs Benachrichtigungen an [`NSNotificationCenter`](xref:Foundation.NSNotificationCenter) sendet und diese Benachrichtigungen abonniert. Das Setup sieht wie der folgende Code Ausschnitt `ViewController.cs`aus:

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

Ein typischer Benachrichtigungs Handler aktualisiert die Benutzeroberfläche und kann ggf. den Anwendungs Zustand ändern, z. b. diesen Handler, der beim Scannen des Objekts aktualisiert wird:

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

Zum Schluss ändern Methoden das Modell und die UX entsprechend dem neuen Zustand: `Enter{State}`

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

### <a name="custom-visualization"></a>Benutzerdefinierte Visualisierung

Die APP zeigt die "Point Cloud" auf niedriger Ebene des Objekts an, das in einem umgebenden Feld enthalten ist, das auf einer erkannten horizontalen Ebene projiziert wurde.

Diese punktcloud steht Entwicklern in der [`ARFrame.RawFeaturePoints`](xref:ARKit.ARFrame.RawFeaturePoints) -Eigenschaft zur Verfügung. Das effiziente visualisieren der Point-Cloud kann ein kniffliges Problem darstellen. Durch Durchlaufen der Punkte und anschließendes Erstellen und Platzieren eines neuen scenekit-Knotens für jeden Punkt würde die Framerate abbrechen. Wenn Sie den asynchronen Vorgang asynchron durchgeführt haben, wird eine Verzögerung angezeigt. Im Beispiel wird die Leistung mit einer dreiteiligen Strategie beibehalten:

- Verwenden von unsicherem Code, um die Daten an einem Ort anzuheften und die Daten als Rohdaten Puffer zu interpretieren.
- Das umrechnen dieses rohpuffers in einen [`SCNGeometrySource`](xref:SceneKit.SCNGeometrySource) und das Erstellen eines "Template" [`SCNGeometryElement`](xref:SceneKit.SCNGeometryElement) -Objekts.
- Schnelles Zusammenfügen der Rohdaten und der Vorlage mithilfe von[`SCNGeometry.Create(SCNGeometrySource[], SCNGeometryElement[])`](xref:SceneKit.SCNGeometry.Create(SceneKit.SCNGeometrySource[],SceneKit.SCNGeometryElement[]))

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

Das Ergebnis sieht wie folgt aus:

![point_cloud](images/arkit_point_cloud.jpeg)

### <a name="complex-gestures"></a>Komplexe Gesten

Der Benutzer kann das umgebende Feld, das das Zielobjekt umgibt, skalieren, drehen und ziehen. In den zugeordneten Gesten Erkennungs Programmen gibt es zwei interessante Dinge.

Zuerst werden alle Gesten Erkennungs Tools erst aktiviert, nachdem ein Schwellenwert überschritten wurde. Beispielsweise hat ein Finger so viele Pixel gezogen, oder die Drehung überschreitet einen Winkel. Das Verfahren besteht darin, den Verschiebe Vorgang zu erhöhen, bis der Schwellenwert überschritten wurde, und ihn dann inkrementell anzuwenden

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

Das zweite interessante passiert in Bezug auf Gesten, wie das umgebende Feld in Bezug auf erkannte reale Flächen verschoben wird. Dieser Aspekt wird in [diesem xamarin-Blogbeitrag](https://blog.xamarin.com/exploring-new-ios-12-arkit-capabilities-with-xamarin/)erläutert.

## <a name="other-new-features-in-arkit-2"></a>Weitere neue Funktionen in Arkit 2

### <a name="more-tracking-configurations"></a>Weitere Überwachungs Konfigurationen

Nun können Sie eine der folgenden Funktionen als Grundlage für eine gemischte Realität verwenden:

- Nur der Geräte Beschleunigungsmesser ([`AROrientationTrackingConfiguration`](xref:ARKit.AROrientationTrackingConfiguration), IOS 11)
- Gesichter ([`ARFaceTrackingConfiguration`](xref:ARKit.ARFaceTrackingConfiguration), IOS 11)
- Referenz Images ([`ARImageTrackingConfiguration`](xref:ARKit.ARImageTrackingConfiguration), IOS 12)
- Scannen von 3D-[`ARObjectScanningConfiguration`](xref:ARKit.ARObjectScanningConfiguration)Objekten (, IOS 12)
- Visual Trägheits odometry ([`ARWorldTrackingConfiguration`](xref:ARKit.ARWorldTrackingConfiguration), verbessert in IOS 12)

`AROrientationTrackingConfiguration`, das in [diesem Blogbeitrag und F# Sample](https://github.com/lobrien/FSharp_Face_AR)erläutert wird, ist die restriktivste Umgebung und bietet eine schlechte gemischte Umgebung, da Sie nur digitale Objekte im Zusammenhang mit der Bewegung des Geräts platziert, ohne zu versuchen, das Gerät und den Bildschirm in der realen Welt zu binden.

`ARImageTrackingConfiguration` Ermöglicht es Ihnen, reale 2D-images (Gemälde, Logos usw.) zu erkennen und diese zum Verankern digitaler Bilder zu verwenden:

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

Für diese Konfiguration gibt es zwei interessante Aspekte:

- Sie ist effizient und kann mit einer potenziell großen Anzahl von Verweis Bildern verwendet werden.
- Die digitalen Bilder sind am Bild verankert, auch wenn sich dieses Bild in der realen Welt bewegt (Wenn z. b. die Abdeckung eines Buchs erkannt wird, wird das Buch nachverfolgt, wenn es aus dem Regal gezogen, festgelegt usw.).

Die `ARObjectScanningConfiguration` wurde [bereits](#recognizing-reference-objects) erläutert und ist eine Entwickler zentrierte Konfiguration zum Scannen von 3D-Objekten. Dies ist hochgradig Prozessor-und Akku intensiv und sollte nicht in Endbenutzer Anwendungen verwendet werden. Das Beispiel zum [Scannen und erkennen von 3D-Objekten](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-scanninganddetecting3dobjects) veranschaulicht die Verwendung dieser Konfiguration.

Die letzte nach Verfolgungs Konfiguration `ARWorldTrackingConfiguration` ,, ist das workpferd der meisten Mixed-Reality-Umgebungen. Diese Konfiguration verwendet "Visual Trägheits odometry", um reale "featurepunkte" mit digitalen Bildern zu verknüpfen. Digitale Geometrie oder Sprites sind relativ zur horizontalen und vertikalen Ebene der realen und in Bezug auf erkannte `ARReferenceObject` Instanzen verankert. In dieser Konfiguration ist der Ursprung der Kamera die ursprüngliche Position der Kamera, bei der die Z-Achse an der Schwerkraft ausgerichtet ist, und digitale Objekte, die sich relativ zu den Objekten in der realen Welt befinden.

### <a name="environmental-texturing"></a>Umgebungs Texturierung

Arkit 2 unterstützt "Umgebungs Texturierung", das erfasste Bilder verwendet, um die Beleuchtung zu schätzen und sogar Glanzlichter auf glänzende Objekte anzuwenden. Die Umgebungs-cubemap wird dynamisch erstellt und kann, sobald die Kamera in alle Richtungen ausgesehen hat, ein beeindruckend realistisches Verhalten erzielen:

![Demo Bild zu Umgebungs Texturierung](images/arkit_env_texturing.png)

Um die Umgebungs Texturierung zu verwenden:

- Die Objekte müssen verwenden [`SCNLightingModel.PhysicallyBased`](xref:SceneKit.SCNLightingModel.PhysicallyBased) und einen Wert im Bereich von 0 bis 1 für [`Metalness.Contents`](xref:SceneKit.SCNMaterial.Metalness) und [`Roughness.Contents`](xref:SceneKit.SCNMaterialProperty.Contents) und zuweisen. [`SCNMaterial`](xref:SceneKit.SCNMaterial)
- Die Überwachungskonfiguration muss Folgendes [`EnvironmentTexturing`](xref:ARKit.ARWorldTrackingConfiguration.EnvironmentTexturing) festlegen  =  [`AREnvironmentTexturing.Automatic`](xref:ARKit.AREnvironmentTexturing.Automatic) :

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

Obwohl die im vorangehenden Code Ausschnitt gezeigte vollständig reflektierende Textur in einem Beispiel Spaß macht, wird die Umgebungs Texturierung wahrscheinlich besser verwendet, und es ist nicht möglich, eine "ungeschützte Valley"-Antwort zu auslösen (die Textur ist nur eine Schätzung basierend auf dem, was die Kamera angeht). aufgezeichnet).


### <a name="shared-and-persistent-ar-experiences"></a>Freigegebene und persistente are-Erfahrungen

Eine weitere wichtige Ergänzung von Arkit 2 ist [`ARWorldMap`](xref:ARKit.ARWorldMap) die-Klasse, die es Ihnen ermöglicht, weltweit nach Verfolgungs Daten zu teilen oder zu speichern. Sie erhalten die aktuelle Weltkarte mit [`ARSession.GetCurrentWorldMapAsync`](xref:ARKit.ARSession.GetCurrentWorldMapAsync) oder: [`GetCurrentWorldMap(Action<ARWorldMap,NSError>)`](xref:ARKit.ARSession.GetCurrentWorldMap(System.Action{ARKit.ARWorldMap,Foundation.NSError}))

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

So können Sie die World Map freigeben oder wiederherstellen:

1. Laden Sie die Daten aus der Datei.
2. Archivieren Sie in ein `ARWorldMap` Objekt,
3. Verwenden Sie dies als Wert für die [`ARWorldTrackingConfiguration.InitialWorldMap`](xref:ARKit.ARWorldTrackingConfiguration.InitialWorldMap) -Eigenschaft:

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

Der `ARWorldMap` enthält nur nicht sichtbare World-Tracking-Daten und die [`ARAnchor`](xref:ARKit.ARAnchor) -Objekte, die _keine_ digitalen Ressourcen enthalten. Wenn Sie Geometrie oder Bilder freigeben möchten, müssen Sie eine eigene Strategie entwickeln, die für Ihren Anwendungsfall geeignet ist (möglicherweise durch Speichern/übertragen nur der Position und Ausrichtung der Geometrie und anwenden `SCNGeometry` der Geometrie auf statische oder möglicherweise durch Speichern/übertragen). serialisierte Objekte). Der Vorteil `ARWorldMap` der besteht darin, dass Ressourcen, die in Relation zu einem `ARAnchor`freigegebenen platziert werden, konsistent zwischen Geräten oder Sitzungen angezeigt werden.

### <a name="universal-scene-description-file-format"></a>Format der universellen Szenen Beschreibungsdatei

Das abschließende Feature von Arkit 2 ist die Übernahme des Datei Formats der [universellen Szenenbeschreibung](https://graphics.pixar.com/usd/docs/Introduction-to-USD.html) von Pixar. Dieses Format ersetzt das DAE-Format von Collada als bevorzugtes Format für die Freigabe und Speicherung von Arkit-Assets. Die Unterstützung für die Visualisierung von Medienobjekten ist in IOS 12 und in die-. Die usdz-Dateierweiterung ist ein unkomprimiertes und unverschlüsseltes ZIP-Archiv, das USD-Dateien enthält. Pixar [bietet Tools zum Arbeiten mit USD-Dateien](https://graphics.pixar.com/usd/docs/USD-Toolset.html#USDToolset-usdedit) , aber es gibt noch keine Unterstützung von Drittanbietern.

## <a name="arkit-programming-tips"></a>Tipps zur Arkit-Programmierung

### <a name="manual-resource-management"></a>Manuelle Ressourcenverwaltung

In Arkit ist es von entscheidender Bedeutung, Ressourcen manuell zu verwalten. Dies ermöglicht nicht nur hohe Frameraten, sondern es ist auch _erforderlich_ , eine verwirrende "Bildschirmsperre" zu vermeiden. Das Arkit-Framework ist für die Bereitstellung eines neuen Kamera[`ARSession.CurrentFrame`](xref:ARKit.ARSession.CurrentFrame)Rahmens (. Bis der aktuelle [`ARFrame`](xref:ARKit.ARFrame) von der `Dispose()` aktuellen aufgerufen wurde, stellt Arkit keinen neuen Frame bereit. Dadurch wird das Video "eingefroren", auch wenn der Rest der APP reaktionsfähig ist. Die Lösung besteht darin, immer `ARSession.CurrentFrame` mit einem `using` -Block zuzugreifen oder `Dispose()` manuell aufzurufen.

Alle von `NSObject` abgeleiteten Objekte `IDisposable` sind `NSObject` und implementieren das Lösch [Muster](https://docs.microsoft.com/dotnet/standard/design-guidelines/dispose-pattern). Daher sollten Sie [dieses Muster für die Implementierung `Dispose` von in einer abgeleiteten Klasse](https://docs.microsoft.com/dotnet/standard/garbage-collection/implementing-dispose)befolgen.

### <a name="manipulating-transform-matrices"></a>Manipulieren von Transformations Matrizen

In jeder 3D-Anwendung befassen Sie sich mit 4X4-Transformations Matrizen, die kompakt beschreiben, wie Sie ein Objekt über 3D-Raum verschieben, drehen und verscheren können. In scenekit sind [`SCNMatrix4`](xref:SceneKit.SCNMatrix4) dies Objekte.  

Die [`SCNNode.Transform`](xref:SceneKit.SCNNode.Transform) -Eigenschaft gibt `SCNMatrix4` die Transformationsmatrix für [`SCNNode`](xref:SceneKit.SCNNode) das-Objekt zurück, das vom `simdfloat4x4` Zeilen Haupttyp unter _stützt wird_ . Zum Beispiel:

```csharp
var node = new SCNNode { Position = new SCNVector3(2, 3, 4) };  
var xform = node.Transform;
Console.WriteLine(xform);
// Output is: "(1, 0, 0, 0)\n(0, 1, 0, 0)\n(0, 0, 1, 0)\n(2, 3, 4, 1)"
```  

Wie Sie sehen können, wird die Position in den ersten drei Elementen der untersten Zeile codiert.

In xamarin ist `NVector4`der gemeinsame Typ zum Bearbeiten von Transformations Matrizen, der in einer Spalten Hauptweise interpretiert wird. Das heißt, dass die Translation/Positions-Komponente in M14, M24, M34, not M41, M42, M43 erwartet wird:

![Row-Major vs Column-Major](images/arkit_row_vs_column.png)

Die Konsistenz mit der Auswahl der Matrix Interpretation ist wichtig für das richtige Verhalten. Da 3D-Transformations Matrizen 4 x 4 sind, führen Konsistenz Fehler nicht zu einer Art der Kompilierzeit oder sogar zur Lauf Zeit Ausnahme – es ist nur, dass Vorgänge unerwartet ausgeführt werden. Wenn Sie feststellen, dass die scenekit/Arkit-Objekte hängen, Weg oder Jitter sind, ist eine falsche Transformationsmatrix eine gute Möglichkeit. Die Lösung ist einfach: [`NMatrix4.Transpose`](xref:OpenTK.NMatrix4.Transpose*) führt eine direkte Umsetzung von Elementen durch.

## <a name="related-links"></a>Verwandte Links

- [Beispiel-App – Scannen und erkennen von 3D-Objekten](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-scanninganddetecting3dobjects)
- [Neues in Arkit 2 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/602/)
- [Grundlegendes zur Arkit-Nachverfolgung und-Erkennung (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/610/)
- [Einführung in Arkit in xamarin. IOS](https://docs.microsoft.com/xamarin/ios/platform/introduction-to-ios11/arkit/)
