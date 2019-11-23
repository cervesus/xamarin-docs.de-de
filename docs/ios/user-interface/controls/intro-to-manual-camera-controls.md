---
title: Manuelle Kamera Steuerelemente in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie das IOS AVFoundation-Framework mit xamarin. IOS verwendet werden kann, um manuelle Kamera Steuerelemente zu aktivieren. Manuelle Kamera Steuerelemente ermöglichen es einem Benutzer, die Einstellungen für Fokus, Leerraum und verfügbar zu steuern.
ms.prod: xamarin
ms.assetid: 56340225-5F3C-4BFC-9A79-61496D7FE5B5
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 1ae7e4c39a07ccfcb472f7add580bf5109166c3d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022058"
---
# <a name="manual-camera-controls-in-xamarinios"></a>Manuelle Kamera Steuerelemente in xamarin. IOS

Die manuellen Kamera Steuerelemente, die vom `AVFoundation Framework` in ios 8 bereitgestellt werden, ermöglichen einer mobilen Anwendung die vollständige Kontrolle über die Kamera eines IOS-Geräts. Diese differenzierte Steuerungsebene kann verwendet werden, um professionelle Kamera Anwendungen zu erstellen und Künstler Kompositionen bereitzustellen, indem Sie die Parameter der Kamera optimieren, während gleichzeitig ein Bild oder Video erstellt wird.

Diese Steuerelemente können auch nützlich sein, wenn Sie wissenschaftliche oder industrielle Anwendungen entwickeln, bei denen die Ergebnisse weniger auf die Richtigkeit oder Schönheit des Bilds ausgerichtet sind und Sie eher darauf ausgerichtet sind, ein Feature oder Element des Abbilds hervorzuheben.

## <a name="avfoundation-capture-objects"></a>AVFoundation-Erfassungs Objekte

Unabhängig davon, ob Sie Videos oder Bilder mithilfe der Kamera auf einem IOS-Gerät aufnehmen, ist der Prozess, der zum Erfassen dieser Images verwendet wird, größtenteils identisch. Dies gilt für Anwendungen, die die standardmäßigen automatisierten Kamera Steuerelemente verwenden, oder für Anwendungen, die die neuen manuellen Kamera Steuerelemente nutzen:

 [![](intro-to-manual-camera-controls-images/image1.png "AVFoundation Capture Objects overview")](intro-to-manual-camera-controls-images/image1.png#lightbox)

Eingaben werden mithilfe eines `AVCaptureConnection`aus einer `AVCaptureDeviceInput` in eine `AVCaptureSession` entnommen. Das Ergebnis wird entweder als Bild oder als Videostream ausgegeben. Der gesamte Prozess wird von einem `AVCaptureDevice`gesteuert.

## <a name="manual-controls-provided"></a>Manuell bereitgestellte Steuerelemente

Mithilfe der neuen APIs, die von IOS 8 bereitgestellt werden, kann die Anwendung die folgenden Kamera Features übernehmen:

- **Manueller Fokus** – durch den Endbenutzer, der den Fokus direkt steuern soll, kann eine Anwendung mehr Kontrolle über das Abbild bereitstellen.
- **Manuelle** Verfügbarkeit – durch die Bereitstellung einer manuellen Kontrolle über die Verfügbarkeit kann eine Anwendung den Benutzern mehr Freiheit bieten und Ihnen ermöglichen, ein stilisiertes aussehen zu erzielen.
- **Manueller** Leerraum – ein weißer Saldo wird verwendet, um die Farbe in einem Bild zu ändern – oft so, dass es realistisch aussieht. Unterschiedliche Lichtquellen haben unterschiedliche Farbtemperaturen, und die Kameraeinstellungen zum Erfassen eines Bilds werden so angepasst, dass diese Unterschiede kompensiert werden. Durch die Möglichkeit, die Benutzersteuerung über den weißen Saldo zuzulassen, können Benutzeranpassungen vornehmen, die nicht automatisch durchgeführt werden können.

IOS 8 bietet Erweiterungen und Verbesserungen für vorhandene IOS-APIs, um die fein abgestimmte Kontrolle über den Abbild Erfassungsprozess bereitzustellen.

## <a name="bracketed-capture"></a>Erfassung in Klammern

Die Erfassung in Klammern basiert auf den Einstellungen der oben dargestellten manuellen Kamera Steuerelemente und ermöglicht es der Anwendung, auf verschiedene Weise einen Zeitpunkt zu erfassen.

Die in Klammern gefasste Erfassung ist ein Burst von Bildern, die mit einer Vielzahl von Einstellungen von Bild zu Bild erstellt wurden.

## <a name="requirements"></a>Voraussetzungen

Folgende Schritte sind erforderlich, um die in diesem Artikel beschriebenen Schritte auszuführen:

- **Xcode 7 + und IOS 8 oder** höher – die APIs Xcode 7 und IOS 8 oder höher von Apple müssen auf dem Computer des Entwicklers installiert und konfiguriert werden.
- **Visual Studio für Mac** – die neueste Version von Visual Studio für Mac muss auf dem Benutzergerät installiert und konfiguriert werden.
- **IOS 8-Gerät** – ein IOS-Gerät, auf dem die neueste Version von IOS 8 ausgeführt wird. Die Kamerafunktionen können nicht im IOS-Simulator getestet werden.

## <a name="general-av-capture-setup"></a>Allgemeines Einrichten der AV-Erfassung

Beim Aufzeichnen von Videos auf einem IOS-Gerät gibt es einen allgemeinen Installationscode, der immer erforderlich ist. In diesem Abschnitt wird das minimale Setup behandelt, das zum Aufzeichnen von Videos von der Kamera des IOS-Geräts und zum Anzeigen des Videos in Echtzeit in einem `UIImageView`erforderlich ist.

### <a name="output-sample-buffer-delegate"></a>Ausgabe Beispiel-Puffer Delegat

Eines der ersten erforderlichen Elemente ist ein Delegat zum Überwachen des Beispielausgabe Puffers und zum Anzeigen eines Bildes, das aus dem Puffer zu einem `UIImageView` in der Benutzeroberfläche der Anwendung gepackt wird.

Mit der folgenden Routine wird der Beispiel Puffer überwacht und die Benutzeroberfläche aktualisiert:

```csharp
using System;
using Foundation;
using UIKit;
using System.CodeDom.Compiler;
using System.Collections.Generic;
using System.Linq;
using AVFoundation;
using CoreVideo;
using CoreMedia;
using CoreGraphics;

namespace ManualCameraControls
{
    public class OutputRecorder : AVCaptureVideoDataOutputSampleBufferDelegate
    {
        #region Computed Properties
        public UIImageView DisplayView { get; set; }
        #endregion

        #region Constructors
        public OutputRecorder ()
        {

        }
        #endregion

        #region Private Methods
        private UIImage GetImageFromSampleBuffer(CMSampleBuffer sampleBuffer) {

            // Get a pixel buffer from the sample buffer
            using (var pixelBuffer = sampleBuffer.GetImageBuffer () as CVPixelBuffer) {
                // Lock the base address
                pixelBuffer.Lock (0);

                // Prepare to decode buffer
                var flags = CGBitmapFlags.PremultipliedFirst | CGBitmapFlags.ByteOrder32Little;

                // Decode buffer - Create a new colorspace
                using (var cs = CGColorSpace.CreateDeviceRGB ()) {

                    // Create new context from buffer
                    using (var context = new CGBitmapContext (pixelBuffer.BaseAddress,
                        pixelBuffer.Width,
                        pixelBuffer.Height,
                        8,
                        pixelBuffer.BytesPerRow,
                        cs,
                        (CGImageAlphaInfo)flags)) {

                        // Get the image from the context
                        using (var cgImage = context.ToImage ()) {

                            // Unlock and return image
                            pixelBuffer.Unlock (0);
                            return UIImage.FromImage (cgImage);
                        }
                    }
                }
            }
        }
        #endregion

        #region Override Methods
        public override void DidOutputSampleBuffer (AVCaptureOutput captureOutput, CMSampleBuffer sampleBuffer, AVCaptureConnection connection)
        {
            // Trap all errors
            try {
                // Grab an image from the buffer
                var image = GetImageFromSampleBuffer(sampleBuffer);

                // Display the image
                if (DisplayView !=null) {
                    DisplayView.BeginInvokeOnMainThread(() => {
                        // Set the image
                        if (DisplayView.Image != null) DisplayView.Image.Dispose();
                        DisplayView.Image = image;

                        // Rotate image to the correct display orientation
                        DisplayView.Transform = CGAffineTransform.MakeRotation((float)Math.PI/2);
                    });
                }

                // IMPORTANT: You must release the buffer because AVFoundation has a fixed number
                // of buffers and will stop delivering frames if it runs out.
                sampleBuffer.Dispose();
            }
            catch(Exception e) {
                // Report error
                Console.WriteLine ("Error sampling buffer: {0}", e.Message);
            }
        }
        #endregion
    }
}
```

Wenn diese Routine eingerichtet ist, kann die `AppDelegate` so geändert werden, dass eine AV-Erfassungs Sitzung geöffnet wird, um einen livevideofeed aufzuzeichnen.

### <a name="creating-an-av-capture-session"></a>Erstellen einer AV-Erfassungs Sitzung

Die AV-Erfassungs Sitzung dient zum Steuern der Aufzeichnung von Livevideos von der Kamera des IOS-Geräts und ist erforderlich, um Videos in einer IOS-Anwendung zu erhalten. Da im Beispiel `ManualCameraControl` Beispielanwendung die Erfassungs Sitzung an verschiedenen Stellen verwendet wird, wird Sie im `AppDelegate` konfiguriert und für die gesamte Anwendung zur Verfügung gestellt.

Gehen Sie folgendermaßen vor, um die `AppDelegate` der Anwendung zu ändern und den erforderlichen Code hinzuzufügen:

1. Doppelklicken Sie auf die `AppDelegate.cs` Datei im Projektmappen-Explorer, um Sie für die Bearbeitung zu öffnen.
1. Fügen Sie die folgenden "using"-Anweisungen am Anfang der Datei ein:

    ```csharp
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    ```

1. Fügen Sie der `AppDelegate`-Klasse die folgenden privaten Variablen und berechneten Eigenschaften hinzu:

    ```csharp
    #region Private Variables
    private NSError Error;
    #endregion

    #region Computed Properties
    public override UIWindow Window {get;set;}
    public bool CameraAvailable { get; set; }
    public AVCaptureSession Session { get; set; }
    public AVCaptureDevice CaptureDevice { get; set; }
    public OutputRecorder Recorder { get; set; }
    public DispatchQueue Queue { get; set; }
    public AVCaptureDeviceInput Input { get; set; }
    #endregion
    ```

1. Überschreiben Sie die fertige-Methode, und ändern Sie Sie in:

    ```csharp
    public override void FinishedLaunching (UIApplication application)
    {
        // Create a new capture session
        Session = new AVCaptureSession ();
        Session.SessionPreset = AVCaptureSession.PresetMedium;

        // Create a device input
        CaptureDevice = AVCaptureDevice.DefaultDeviceWithMediaType (AVMediaType.Video);
        if (CaptureDevice == null) {
            // Video capture not supported, abort
            Console.WriteLine ("Video recording not supported on this device");
            CameraAvailable = false;
            return;
        }

        // Prepare device for configuration
        CaptureDevice.LockForConfiguration (out Error);
        if (Error != null) {
            // There has been an issue, abort
            Console.WriteLine ("Error: {0}", Error.LocalizedDescription);
            CaptureDevice.UnlockForConfiguration ();
            return;
        }

        // Configure stream for 15 frames per second (fps)
        CaptureDevice.ActiveVideoMinFrameDuration = new CMTime (1, 15);

        // Unlock configuration
        CaptureDevice.UnlockForConfiguration ();

        // Get input from capture device
        Input = AVCaptureDeviceInput.FromDevice (CaptureDevice);
        if (Input == null) {
            // Error, report and abort
            Console.WriteLine ("Unable to gain input from capture device.");
            CameraAvailable = false;
            return;
        }

        // Attach input to session
        Session.AddInput (Input);

        // Create a new output
        var output = new AVCaptureVideoDataOutput ();
        var settings = new AVVideoSettingsUncompressed ();
        settings.PixelFormatType = CVPixelFormatType.CV32BGRA;
        output.WeakVideoSettings = settings.Dictionary;

        // Configure and attach to the output to the session
        Queue = new DispatchQueue ("ManCamQueue");
        Recorder = new OutputRecorder ();
        output.SetSampleBufferDelegate (Recorder, Queue);
        Session.AddOutput (output);

        // Let tabs know that a camera is available
        CameraAvailable = true;
    }
    ```

1. Speichern Sie die Änderungen in der Datei.

Wenn dieser Code vorhanden ist, können die manuellen Kamera Steuerelemente problemlos für Experimente und Tests implementiert werden.

## <a name="manual-focus"></a>Manueller Fokus

Wenn der Endbenutzer die Steuerung des Fokus direkt übernehmen kann, kann eine Anwendung eine bessere Kontrolle über das Abbild bereitstellen.

Beispielsweise kann ein professioneller Fotograf den Fokus eines Bilds abschwächen, um einen [Bokeh-Effekt](https://en.wikipedia.org/wiki/Bokeh)zu erzielen:

[![](intro-to-manual-camera-controls-images/image2.png "A Bokeh Effect")](intro-to-manual-camera-controls-images/image2.png#lightbox)

Oder erstellen Sie einen [Fokus-Pull-Effekt](http://www.mediacollege.com/video/camera/focus/pull.html), wie z. b.:

[![](intro-to-manual-camera-controls-images/image3.png "The Focus Pull Effect")](intro-to-manual-camera-controls-images/image3.png#lightbox)

Für Analysten oder einen Writer von medizinischen Anwendungen kann die Anwendung den Bereich für Experimente Programm gesteuert verschieben. Die neue API ermöglicht es entweder dem Endbenutzer oder der Anwendung, die Kontrolle über den Fokus zu nehmen, wenn das Image erstellt wird.

### <a name="how-focus-works"></a>Funktionsweise des Fokus

Vor der Erörterung der Details zum Steuern des Fokus in einer IOS 8-Anwendung. Werfen wir einen kurzen Blick auf die Funktionsweise des Fokus auf einem IOS-Gerät:

[![](intro-to-manual-camera-controls-images/image4.png "How focus works in an iOS device")](intro-to-manual-camera-controls-images/image4.png#lightbox)

Light wechselt in den Kamera-und auf dem IOS-Gerät und konzentriert sich auf einen Bildsensor. Der Abstand des Bilds von der Sensor steuert, wo der Fokuspunkt (der Bereich, in dem das Bild als Sharpest angezeigt wird) in Beziehung zum Sensor ist. Je weiter der Fokus vom Sensor entfernt wird, Entfernungs Objekte scheinen klügsten, und die Näheres naheliegenden Objekte scheinen klügsten zu sein.

Auf einem IOS-Gerät wird der Sensor durch Magnete und Sprünge näher oder von diesem Sensor entfernt. Folglich ist die exakte Positionierung des Lens nicht möglich, da er von Gerät zu Gerät variiert und von Parametern, wie z. b. der Ausrichtung des Geräts oder dem Alter des Geräts und Spring, beeinflusst werden kann.

### <a name="important-focus-terms"></a>Wichtige Schwerpunkt Begriffe

Beim Umgang mit dem Fokus gibt es einige Begriffe, mit denen der Entwickler vertraut sein sollte:

- **Tiefe des Felds** – der Abstand zwischen den nächstgelegenen und weit entfernten Objekten im Fokus.
- **Makro** : Dies ist das Near-Ende des Fokus Spektrums und ist die nächstgelegene Distanz, mit der sich der Fokus erreichen kann.
- **Unendlich** – Dies ist das weitreichende Ende des Fokus Spektrums und ist die weit entfernte Entfernung, in der sich der Fokus befinden kann.
- **Hyperfokus-Distanz** – Dies ist der Punkt im Fokusbereich, bei dem sich das am weitesten entfernte Objekt im Frame nur am äußersten Ende des Fokus befindet. Mit anderen Worten: Dies ist die Schwerpunkt Position, die die Tiefe des Felds maximiert.
- **Position des Lens** – das Steuerelement steuert alle oben genannten anderen Begriffe. Dies ist der Abstand des Sensors vom Sensor und somit der Fokus des Controllers.

Mit diesen Begriffen und Kenntnissen können die neuen manuellen Fokus Steuerelemente in einer IOS 8-Anwendung erfolgreich implementiert werden.

### <a name="existing-focus-controls"></a>Vorhandene Fokus Steuerelemente

IOS 7 und frühere Versionen haben vorhandene Fokus Steuerungen über `FocusMode`Eigenschaft wie folgt bereitgestellt:

- `AVCaptureFocusModeLocked` – der Fokus wird an einem einzelnen Fokuspunkt gesperrt.
- `AVCaptureFocusModeAutoFocus` – die Kamera durchläuft alle Mittelpunkte, bis Sie den Spitzen Fokus findet und dort bleibt.
- `AVCaptureFocusModeContinuousAutoFocus` – die Kamera konzentriert sich immer wieder, wenn eine Bedingung außerhalb des Fokus entdeckt wird.

Die vorhandenen Steuerelemente haben auch über die`FocusPointOfInterest`-Eigenschaft eine festleg Bare Point of Interest bereitgestellt, sodass der Benutzer auf einen bestimmten Bereich konzentrieren kann. Die Anwendung kann auch die Bewegung des Durchsatzes nachverfolgen, indem Sie die `IsAdjustingFocus`-Eigenschaft überwacht.

Außerdem wurde die Bereichs Einschränkung durch die `AutoFocusRangeRestriction`-Eigenschaft wie folgt bereitgestellt:

- `AVCaptureAutoFocusRangeRestrictionNear` – schränkt den Autofocus auf die nahe gelegene Tiefe ein. Nützlich in Situationen wie dem Scannen eines QR-Codes oder Barcode.
- `AVCaptureAutoFocusRangeRestrictionFar` – schränkt den Autofocus auf eine entfernte Tiefe ein. Nützlich in Situationen, in denen Objekte, die bekanntermaßen irrelevant sind, im Sichtfeld (z. a. einem Fensterrahmen) angezeigt werden.

Schließlich gibt es die `SmoothAutoFocus`-Eigenschaft, die den Algorithmus für den automatischen Fokus verlangsamt und in kleineren Schritten durchläuft, um das Verschieben von Artefakten beim Aufzeichnen von Videos zu vermeiden.

### <a name="new-focus-controls-in-ios-8"></a>Neue Fokus Steuerelemente in ios 8

Zusätzlich zu den Features, die bereits von IOS 7 und höher bereitgestellt werden, stehen nun die folgenden Features zum Steuern des Fokus in ios 8 zur Verfügung:

- Vollständige manuelle Steuerung der Linsen Position beim Sperren des Fokus.
- Die Schlüssel-Wert-Beobachtung der Linsen Position in einem beliebigen Fokus Modus.

Um die oben genannten Funktionen zu implementieren, wurde die `AVCaptureDevice` Klasse so geändert, dass Sie eine schreibgeschützte `LensPosition` Eigenschaft enthält, die verwendet wird, um die aktuelle Position des Kamera-Lens zu erhalten.

Das Erfassungsgerät muss sich im gesperrten Fokus Modus befinden, damit die Position des Ablaufens manuell gesteuert werden kann. Beispiel:

 `CaptureDevice.FocusMode = AVCaptureFocusMode.Locked;`

Mit der `SetFocusModeLocked`-Methode des Aufzeichnungs Geräts wird die Position der Kameralinse angepasst. Eine optionale Rückruf Routine kann bereitgestellt werden, um eine Benachrichtigung zu erhalten, wenn die Änderung wirksam wird. Beispiel:

```csharp
ThisApp.CaptureDevice.LockForConfiguration(out Error);
ThisApp.CaptureDevice.SetFocusModeLocked(Position.Value,null);
ThisApp.CaptureDevice.UnlockForConfiguration();
```

Wie im obigen Code gezeigt, muss das Erfassungsgerät für die Konfiguration gesperrt sein, bevor eine Änderung an der Position des Endpunkts vorgenommen werden kann. Gültige Werte für die Linsen Position liegen zwischen 0,0 und 1,0.

### <a name="manual-focus-example"></a>Beispiel für einen manuellen Fokus

Mit dem allgemeinen Installationscode für die AV-Erfassung kann eine `UIViewController` dem Storyboard der Anwendung hinzugefügt und wie folgt konfiguriert werden:

[![](intro-to-manual-camera-controls-images/image5.png "A UIViewController can be added to the applications Storyboard and configured as shown here")](intro-to-manual-camera-controls-images/image5.png#lightbox)

Die-Sicht enthält die folgenden Hauptelemente:

- Eine `UIImageView`, die den Videofeed anzeigt.
- Eine `UISegmentedControl`, mit der der Fokus Modus von automatisch in gesperrt geändert wird.
- Eine `UISlider`, mit der die aktuelle Position des-anlens angezeigt und aktualisiert wird.

Gehen Sie folgendermaßen vor, um den Ansichts Controller für die manuelle Steuerung des Fokus zu verknüpfen:

1. Fügen Sie die folgenden using-Anweisungen hinzu:

    ```csharp
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using System.Timers;
    ```

1. Fügen Sie die folgenden privaten Variablen hinzu:

    ```csharp
    #region Private Variables
    private NSError Error;
    private bool Automatic = true;
    #endregion
    ```

1. Fügen Sie die folgenden berechneten Eigenschaften hinzu:

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```

1. Überschreiben Sie die `ViewDidLoad`-Methode, und fügen Sie folgenden Code hinzu:

    ```csharp
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();

        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;

        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;

        // Create a timer to monitor and update the UI
        SampleTimer = new Timer (5000);
        SampleTimer.Elapsed += (sender, e) => {
            // Update position slider
            Position.BeginInvokeOnMainThread(() =>{
                Position.Value = ThisApp.Input.Device.LensPosition;
            });
        };

        // Watch for value changes
        Segments.ValueChanged += (object sender, EventArgs e) => {

            // Lock device for change
            ThisApp.CaptureDevice.LockForConfiguration(out Error);

            // Take action based on the segment selected
            switch(Segments.SelectedSegment) {
            case 0:
                // Activate auto focus and start monitoring position
                Position.Enabled = false;
                ThisApp.CaptureDevice.FocusMode = AVCaptureFocusMode.ContinuousAutoFocus;
                SampleTimer.Start();
                Automatic = true;
                break;
            case 1:
                // Stop auto focus and allow the user to control the camera
                SampleTimer.Stop();
                ThisApp.CaptureDevice.FocusMode = AVCaptureFocusMode.Locked;
                Automatic = false;
                Position.Enabled = true;
                break;
            }

            // Unlock device
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };

        // Monitor position changes
        Position.ValueChanged += (object sender, EventArgs e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic) return;

            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.SetFocusModeLocked(Position.Value,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    }
    ```

1. Überschreiben Sie die `ViewDidAppear`-Methode, und fügen Sie Folgendes hinzu, um aufzuzeichnen, wenn die Ansicht geladen wird:

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);

        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;

            ThisApp.Session.StartRunning ();
            SampleTimer.Start ();
        }
    }
    ```

1. Wenn sich die Kamera im Auto-Modus befindet, wird der Schieberegler automatisch verschoben, wenn die Kamera den Fokus anpasst:

    [![](intro-to-manual-camera-controls-images/image6.png "The slider will move automatically as the camera adjusts focus in this sample app")](intro-to-manual-camera-controls-images/image6.png#lightbox)
1. Tippen Sie auf das gesperrte Segment, und ziehen Sie den Schieberegler, um die Position des Zieh Punkts manuell anzupassen

    [![](intro-to-manual-camera-controls-images/image7.png "Manually adjusting the lens position")](intro-to-manual-camera-controls-images/image7.png#lightbox)
1. Beendet die Anwendung.

Der obige Code hat gezeigt, wie die Position des Bild Laufes überwacht wird, wenn sich die Kamera im automatischen Modus befindet, oder einen Schieberegler zum Steuern der Position im gesperrten Modus verwenden.

## <a name="manual-exposure"></a>Manuelles verfügbar machen

"Verfügbar" bezieht sich auf die Helligkeit eines Bilds in Relation zur Quell Helligkeit und wird dadurch bestimmt, wie viel Licht auf den Sensor trifft, wie lange der Sensor erreicht wird (ISO-Zuordnung). Durch die manuelle Kontrolle über die Verfügbarkeit kann eine Anwendung den Endbenutzern mehr Freiheit bieten und Ihnen ermöglichen, ein stilisiertes aussehen zu erzielen.

Mithilfe der manuellen verfügbar machung von Steuerelementen kann der Benutzer ein Bild von unrealistischer zu dunkel und von Moody machen:

[![](intro-to-manual-camera-controls-images/image8.png "A sample an image showing exposure from unrealistically bright to dark and moody")](intro-to-manual-camera-controls-images/image8.png#lightbox)

Dies kann auch automatisch mithilfe der programmgesteuerten Steuerung für wissenschaftliche Anwendungen oder über manuelle Steuerelemente erfolgen, die von der Benutzeroberfläche der Anwendungen bereitgestellt werden. In beiden Fällen bieten die neuen API-APIs für die Verfügbarkeit von IOS 8 eine präzisere Kontrolle über die Einstellungen der Kamera.

### <a name="how-exposure-works"></a>Funktionsweise von Verfügbarkeit

Vor der Erörterung der Details zum Steuern der Verfügbarkeit in einer IOS 8-Anwendung. Werfen wir einen kurzen Blick darauf, wie die Verfügbarkeit funktioniert:

[![](intro-to-manual-camera-controls-images/image9.png "How exposure works")](intro-to-manual-camera-controls-images/image9.png#lightbox)

Die drei grundlegenden Elemente, die zur Steuerung des verfügbar sind, sind:

- **Auslösegeschwindigkeit** – Dies ist die Zeitspanne, in der der Verschluss geöffnet ist, um das Licht auf den Kamerasensor zu lassen. Der kürzerer Zeit, in der der Verschluss geöffnet ist, ist weniger hell in und der Crisper des Bilds (weniger Bewegungs weich). Wenn der Schalker länger geöffnet ist, desto mehr Licht ist ein Let in und desto mehr Bewegungsunschärfe.
- **ISO-Zuordnung** – Dies ist ein Begriff, der aus der Film Fotografie stammt und sich auf die Vertraulichkeit der im Film zu Licht enden Chemikalien bezieht. Niedrige ISO-Werte in Filmen haben weniger Granularität und eine präzisere Farb Reproduktion. niedrige ISO-Werte auf digitalen Sensoren weisen weniger Sensor Rauschen auf, aber weniger Helligkeit. Je höher der ISO-Wert ist, desto heller das Bild, jedoch mit mehr Sensor Rauschen. "ISO" auf einem digitalen Sensor ist ein Maß für den [elektronischen Gewinn](https://en.wikipedia.org/wiki/Gain), nicht für ein physisches Feature.
- **Lens Aperture** – Dies ist die Größe des öffnenden Lens. Auf allen IOS-Geräten wird die Bild-auf-Taste korrigiert, sodass nur zwei Werte, mit denen die Verfügbarkeit angepasst werden kann, die Auslösegeschwindigkeit und ISO-Werte sind.

### <a name="how-continuous-auto-exposure-works"></a>Funktionsweise der kontinuierlichen automatischen verfügbar machung

Bevor Sie sich mit der manuellen Funktionsweise vertraut machen, empfiehlt es sich, zu verstehen, wie die kontinuierliche automatische Verfügbarkeit auf einem IOS-Gerät funktioniert.

[![](intro-to-manual-camera-controls-images/image10.png "How continuous auto exposure works in an iOS device")](intro-to-manual-camera-controls-images/image10.png#lightbox)

Der erste ist der automatische zulegungs Block, er hat die Aufgabe, eine optimale Verfügbarkeit zu berechnen, und wird kontinuierlich über eine Messungs Statistik versorgt. Diese Informationen werden verwendet, um die optimale Mischung aus ISO und der Auslösegeschwindigkeit zu berechnen, damit die Szene gut beleuchtet wird. Dieser Cycle wird als AE-Schleife bezeichnet.

### <a name="how-locked-exposure-works"></a>Funktionsweise der gesperrten Gefährdung

Als nächstes untersuchen wir, wie das Sperr Risiko auf IOS-Geräten funktioniert.

[![](intro-to-manual-camera-controls-images/image11.png "How locked exposure works in iOS devices")](intro-to-manual-camera-controls-images/image11.png#lightbox)

Auch hier haben Sie den automatischen zulegungs Block, der versucht, die optimalen Werte für IOS und Duration zu berechnen. In diesem Modus wird der AE-Block jedoch von der Messungs Statistik-Engine getrennt.

### <a name="existing-exposure-controls"></a>Vorhandene verfügbar machen

IOS 7 und höher: Stellen Sie die folgenden vorhandenen verfügbar machung-Steuerelemente über die `ExposureMode`-Eigenschaft bereit:

- `AVCaptureExposureModeLocked` – die Szene einmal als Stichprobe und verwendet diese Werte in der Szene.
- `AVCaptureExposureModeContinuousAutoExposure` – durchläuft die Szene fortlaufend, um sicherzustellen, dass Sie gut beleuchtet ist.

Der `ExposurePointOfInterest` kann verwendet werden, um die Szene verfügbar zu machen, indem Sie ein Zielobjekt auswählen, das verfügbar gemacht werden soll, und die Anwendung kann die `AdjustingExposure` Eigenschaft überwachen, um festzustellen, wann die Offenlegung angepasst wird.

### <a name="new-exposure-controls-in-ios-8"></a>Neue verfügbar machen von Steuerelementen in ios 8

Zusätzlich zu den Features, die bereits von IOS 7 und höher bereitgestellt werden, sind jetzt die folgenden Features verfügbar, um die Verfügbarkeit in ios 8 zu steuern:

- Vollständig manuelle benutzerdefinierte verfügbar machung.
- "Get", "Set" und "Key-Value" beobachten IOS und die Zeitspanne (Dauer).

Um die oben genannten Features zu implementieren, wurde ein neuer `AVCaptureExposureModeCustom` Modus hinzugefügt. Wenn die Kamera in der benutzerdefinierte Modus ist, kann der folgende Code verwendet werden, um die Gültigkeitsdauer und ISO anzupassen:

```csharp
CaptureDevice.LockForConfiguration(out Error);
CaptureDevice.LockExposure(DurationValue,ISOValue,null);
CaptureDevice.UnlockForConfiguration();
```

Im Modus "automatisch" und "gesperrt" kann die Anwendung die Verschiebung der automatischen Funktions Routine mithilfe des folgenden Codes anpassen:

```csharp
CaptureDevice.LockForConfiguration(out Error);
CaptureDevice.SetExposureTargetBias(Value,null);
CaptureDevice.UnlockForConfiguration();
```

Die minimalen und maximalen Einstellungs Bereiche hängen vom Gerät ab, auf dem die Anwendung ausgeführt wird, sodass Sie niemals hart codiert werden sollten. Verwenden Sie stattdessen die folgenden Eigenschaften, um die minimalen und maximalen Wertebereiche zu erhalten:

- `CaptureDevice.MinExposureTargetBias`
- `CaptureDevice.MaxExposureTargetBias`
- `CaptureDevice.ActiveFormat.MinISO`
- `CaptureDevice.ActiveFormat.MaxISO`
- `CaptureDevice.ActiveFormat.MinExposureDuration`
- `CaptureDevice.ActiveFormat.MaxExposureDuration`

Wie im obigen Code gezeigt, muss das Erfassungsgerät für die Konfiguration gesperrt sein, bevor eine Änderung der Verfügbarkeit vorgenommen werden kann.

### <a name="manual-exposure-example"></a>Beispiel für manuelle Übersetzung

Mit dem allgemeinen Installationscode für die AV-Erfassung kann eine `UIViewController` dem Storyboard der Anwendung hinzugefügt und wie folgt konfiguriert werden:

[![](intro-to-manual-camera-controls-images/image12.png "A UIViewController can be added to the applications Storyboard and configured as shown here")](intro-to-manual-camera-controls-images/image12.png#lightbox)

Die-Sicht enthält die folgenden Hauptelemente:

- Eine `UIImageView`, die den Videofeed anzeigt.
- Eine `UISegmentedControl`, mit der der Fokus Modus von automatisch in gesperrt geändert wird.
- Vier `UISlider` Steuerelemente, die Offset, Dauer, ISO und Bias anzeigen und aktualisieren.

Gehen Sie folgendermaßen vor, um den Ansichts Controller für manuelles verfügbar machen von Steuerelementen zu verknüpfen:

1. Fügen Sie die folgenden using-Anweisungen hinzu:

    ```csharp
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using System.Timers;
    ```

1. Fügen Sie die folgenden privaten Variablen hinzu:

    ```csharp
    #region Private Variables
    private NSError Error;
    private bool Automatic = true;
    private nfloat ExposureDurationPower = 5;
    private nfloat ExposureMinimumDuration = 1.0f/1000.0f;
    #endregion
    ```

1. Fügen Sie die folgenden berechneten Eigenschaften hinzu:

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```

1. Überschreiben Sie die `ViewDidLoad`-Methode, und fügen Sie folgenden Code hinzu:

    ```csharp
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();

        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;

        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;

        // Set min and max values
        Offset.MinValue = ThisApp.CaptureDevice.MinExposureTargetBias;
        Offset.MaxValue = ThisApp.CaptureDevice.MaxExposureTargetBias;

        Duration.MinValue = 0.0f;
        Duration.MaxValue = 1.0f;

        ISO.MinValue = ThisApp.CaptureDevice.ActiveFormat.MinISO;
        ISO.MaxValue = ThisApp.CaptureDevice.ActiveFormat.MaxISO;

        Bias.MinValue = ThisApp.CaptureDevice.MinExposureTargetBias;
        Bias.MaxValue = ThisApp.CaptureDevice.MaxExposureTargetBias;

        // Create a timer to monitor and update the UI
        SampleTimer = new Timer (5000);
        SampleTimer.Elapsed += (sender, e) => {
            // Update position slider
            Offset.BeginInvokeOnMainThread(() =>{
                Offset.Value = ThisApp.Input.Device.ExposureTargetOffset;
            });

            Duration.BeginInvokeOnMainThread(() =>{
                var newDurationSeconds = CMTimeGetSeconds(ThisApp.Input.Device.ExposureDuration);
                var minDurationSeconds = Math.Max(CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MinExposureDuration), ExposureMinimumDuration);
                var maxDurationSeconds = CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MaxExposureDuration);
                var p = (newDurationSeconds - minDurationSeconds) / (maxDurationSeconds - minDurationSeconds);
                Duration.Value = (float)Math.Pow(p, 1.0f/ExposureDurationPower);
            });

            ISO.BeginInvokeOnMainThread(() => {
                ISO.Value = ThisApp.Input.Device.ISO;
            });

            Bias.BeginInvokeOnMainThread(() => {
                Bias.Value = ThisApp.Input.Device.ExposureTargetBias;
            });
        };

        // Watch for value changes
        Segments.ValueChanged += (object sender, EventArgs e) => {

            // Lock device for change
            ThisApp.CaptureDevice.LockForConfiguration(out Error);

            // Take action based on the segment selected
            switch(Segments.SelectedSegment) {
            case 0:
                // Activate auto exposure and start monitoring position
                Duration.Enabled = false;
                ISO.Enabled = false;
                ThisApp.CaptureDevice.ExposureMode = AVCaptureExposureMode.ContinuousAutoExposure;
                SampleTimer.Start();
                Automatic = true;
                break;
            case 1:
                // Lock exposure and allow the user to control the camera
                SampleTimer.Stop();
                ThisApp.CaptureDevice.ExposureMode = AVCaptureExposureMode.Locked;
                Automatic = false;
                Duration.Enabled = false;
                ISO.Enabled = false;
                break;
            case 2:
                // Custom exposure and allow the user to control the camera
                SampleTimer.Stop();
                ThisApp.CaptureDevice.ExposureMode = AVCaptureExposureMode.Custom;
                Automatic = false;
                Duration.Enabled = true;
                ISO.Enabled = true;
                break;
            }

            // Unlock device
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };

        // Monitor position changes
        Duration.ValueChanged += (object sender, EventArgs e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic) return;

            // Calculate value
            var p = Math.Pow(Duration.Value,ExposureDurationPower);
            var minDurationSeconds = Math.Max(CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MinExposureDuration),ExposureMinimumDuration);
            var maxDurationSeconds = CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MaxExposureDuration);
            var newDurationSeconds = p * (maxDurationSeconds - minDurationSeconds) +minDurationSeconds;

            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.LockExposure(CMTime.FromSeconds(p,1000*1000*1000),ThisApp.CaptureDevice.ISO,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };

        ISO.ValueChanged += (object sender, EventArgs e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic) return;

            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.LockExposure(ThisApp.CaptureDevice.ExposureDuration,ISO.Value,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };

        Bias.ValueChanged += (object sender, EventArgs e) => {

            // If we are in the automatic mode, ignore changes
            // if (Automatic) return;

            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.SetExposureTargetBias(Bias.Value,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    }
    ```

1. Überschreiben Sie die `ViewDidAppear`-Methode, und fügen Sie Folgendes hinzu, um aufzuzeichnen, wenn die Ansicht geladen wird:

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);

        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;

            ThisApp.Session.StartRunning ();
            SampleTimer.Start ();
        }
    }
    ```

1. Wenn sich die Kamera im Auto-Modus befindet, werden die Schieberegler automatisch verschoben, wenn die Kamera die Verfügbarkeit anpasst:

    [![](intro-to-manual-camera-controls-images/image13.png "The sliders will move automatically as the camera adjusts exposure")](intro-to-manual-camera-controls-images/image13.png#lightbox)
1. Tippen Sie auf das gesperrte Segment, und ziehen Sie den Schieberegler "Bias", um den Schieberegler manuell anzupassen:

    [![](intro-to-manual-camera-controls-images/image14.png "Adjusting the bias of the automatic exposure manually")](intro-to-manual-camera-controls-images/image14.png#lightbox)
1. Tippen Sie auf das benutzerdefinierte Segment, und ziehen Sie die Dauer-und ISO-Schieberegler, um die Anzeige

    [![](intro-to-manual-camera-controls-images/image15.png "Drag the Duration and ISO sliders to manually control exposure")](intro-to-manual-camera-controls-images/image15.png#lightbox)
1. Beendet die Anwendung.

Der obige Code hat gezeigt, wie die Einstellungen für die Verfügbarkeit überwacht werden, wenn sich die Kamera im automatischen Modus befindet, und wie Schieberegler verwendet werden, um die Verfügbarkeit zu steuern, wenn Sie sich im gesperrten oder benutzerdefinierten Modus befindet.

## <a name="manual-white-balance"></a>Manueller weiß Ausgleich

Mit den Steuerelementen "White Balance" können Benutzer das Gleichgewicht von "kolsr" in einem Bild anpassen, damit Sie realistischer werden. Unterschiedliche Lichtquellen haben unterschiedliche Farbtemperaturen, und die Kameraeinstellungen zum Erfassen eines Bilds müssen so angepasst werden, dass diese Unterschiede kompensiert werden. Durch die Möglichkeit, das Benutzer Steuerelement über den weißen Saldo zuzulassen, können Sie auch professionelle Anpassungen vornehmen, mit denen die automatischen Routinen keine künstlerischen Auswirkungen erreichen können.

[![](intro-to-manual-camera-controls-images/image16.png "A sample image showing Manual White Balance adjustments")](intro-to-manual-camera-controls-images/image16.png#lightbox)

Beispielsweise hat die Sommerzeit eine blueish-Umwandlung, wohingegen Wolfram-Incandescent-Glühbirnen ein gewärtes, gelb orangefarbenes tint haben. (Verwirrend ist, dass "kalte" Farben höhere Farbtemperaturen als "warme" Farben aufweisen. Farbtemperaturen sind ein physisches Measure, kein perperer Wert.)

Das menschliche Denken ist sehr gut, wenn die Unterschiede in der Farbtemperatur kompensiert werden, dies ist jedoch nicht möglich. Die Kamera arbeitet durch Verstärkung der Farbe auf dem entgegengesetzten Spektrum, um die Farbunterschiede anzupassen.

Die neue API für die Bereitstellung von IOS 8 ermöglicht es der Anwendung, die Kontrolle über den Prozess zu übernehmen und präzise Kontrolle über die Einstellungen des weißen Ausgleichs der Kamera zu bieten.

### <a name="how-white-balance-works"></a>Funktionsweise des weißen Ausgleichs

Vor der Erörterung der Details zum Steuern des weißen Ausgleichs in einer IOS 8-Anwendung. Werfen wir einen kurzen Blick auf die Funktionsweise des weißen Ausgleichs:

In der Studie der Farbwahrnehmung sind der [CIE 1931 RGB-Farbraum und der CIE 1931 XYZ-Farbraum](https://en.wikipedia.org/wiki/CIE_1931_color_space) die ersten mathematisch definierten Farbbereiche. Sie wurden von der International Commission on Illumination (CIE) in 1931 erstellt.

[![](intro-to-manual-camera-controls-images/image17.png "The CIE 1931 RGB color space and CIE 1931 XYZ color space")](intro-to-manual-camera-controls-images/image17.png#lightbox)

Das obige Diagramm zeigt alle Farben, die für das menschliche Auge sichtbar sind, von Deep Blue bis hellgrün bis hellrot. Jeder Punkt im Diagramm kann mit einem X-und Y-Wert gezeichnet werden, wie im obigen Diagramm gezeigt.

Wie im Diagramm dargestellt, gibt es X-und Y-Werte, die im Diagramm dargestellt werden können, das sich außerhalb des Bereichs der menschlichen Vision befinden würde. Folglich können diese Farben nicht von einer Kamera reproduziert werden.

Die kleinere Kurve im obigen Diagramm heißt [planckian Locus](https://en.wikipedia.org/wiki/Planckian_locus), der die Farbtemperatur (in Grad Kelvin) mit höheren Zahlen auf der blauen Seite (heißer) und niedrigeren Ziffern auf der roten Seite (Kühler) ausdrückt. Diese sind für typische Beleuchtungssituationen nützlich.

Bei gemischten Beleuchtungsbedingungen müssen die Anpassungen des weißen durch Laufwerks von planckian Locus abweichen, um die erforderlichen Änderungen vorzunehmen. In diesen Fällen muss die Anpassung entweder auf die grüne oder rote/Magenta-Seite der Cie-Skala verlagert werden.

IOS-Geräte kompensieren Farb Umwandlungen, indem Sie den entgegengesetzten farbgewinn erhöhen. Wenn eine Szene beispielsweise zu viel blau ist, wird der rote Gewinn erhöht, um dies auszugleichen. Diese Werte für den Gewinn werden für bestimmte Geräte, sodass Sie Geräte abhängig sind, kalibriert.

### <a name="existing-white-balance-controls"></a>Vorhandene Steuerelemente für weißen Ausgleich

IOS 7 und höher stellte die folgenden vorhandenen weißen Steuerelemente über `WhiteBalanceMode` Eigenschaft bereit:

- `AVCapture WhiteBalance ModeLocked` – eine Stichprobe der Szene und die Verwendung dieser Werte in der gesamten Szene.
- `AVCapture WhiteBalance ModeContinuousAutoExposure` – die Szene ständig abgleichen, um sicherzustellen, dass Sie gut ausgeglichen ist.

Und die Anwendung kann die `AdjustingWhiteBalance`-Eigenschaft überwachen, um festzustellen, wann die Gefährdung angepasst wird.

### <a name="new-white-balance-controls-in-ios-8"></a>Neue Steuerelemente für das weiße Gleichgewicht in ios 8

Zusätzlich zu den Features, die bereits von IOS 7 und höher bereitgestellt werden, stehen nun die folgenden Features zum Steuern des weißen Ausgleichs in ios 8 zur Verfügung:

- Vollständige manuelle Kontrolle über den Geräte-RGB-Gewinn.
- Get, Set und Key-Value beobachten den RGB-Gewinn des Geräts.
- Unterstützung für einen weißen Ausgleich mithilfe einer grauen Karte.
- Konvertierungs Routinen in und aus geräteunabhängigen Farbräumen.

Um die oben genannten Funktionen zu implementieren, wurde die `AVCaptureWhiteBalanceGain` Struktur mit den folgenden Membern hinzugefügt:

- `RedGain`
- `GreenGain`
- `BlueGain`

Der Höchstwert für den weißen Saldo beträgt derzeit vier (4) und kann über die `MaxWhiteBalanceGain`-Eigenschaft vorbereitet werden. Der zulässige Bereich liegt also zwischen 1 und `MaxWhiteBalanceGain` (4).

Die `DeviceWhiteBalanceGains`-Eigenschaft kann verwendet werden, um die aktuellen Werte zu beobachten. Verwenden Sie `SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains`, um die Balance zu ändern, wenn sich die Kamera im gesperrten weißen Ausgleichs Modus befindet.

#### <a name="conversion-routines"></a>Konvertierungs Routinen

Konvertierungs Routinen wurden IOS 8 hinzugefügt, um die Konvertierung in und aus geräteunabhängigen Farbraum zu unterstützen. Um die Konvertierungs Routinen zu implementieren, wurde die `AVCaptureWhiteBalanceChromaticityValues` Struktur mit den folgenden Membern hinzugefügt:

- `X`: ein Wert zwischen 0 und 1.
- `Y`: ein Wert zwischen 0 und 1.

Eine `AVCaptureWhiteBalanceTemperatureAndTintValues` Struktur wurde auch mit den folgenden Membern hinzugefügt:

- `Temperature`: ein Gleit Komma Wert in Grad Kelvin.
- `Tint`-ist ein Offset von grün oder Magenta zwischen 0 und 150 mit positiven Werten in Richtung der grünen Richtung und negative Richtung in der Magenta.

Verwenden Sie die Methoden `CaptureDevice.GetTemperatureAndTintValues`und `CaptureDevice.GetDeviceWhiteBalanceGains`, um zwischen Temperatur-und tint-, Chromaticity-und RGB-Farbräumen zu konvertieren.

> [!NOTE]
> Die Konvertierungs Routinen sind genauer, je näher der zu konvertierende Wert dem planckian-Locus entspricht.

#### <a name="gray-card-support"></a>Unterstützung für graue Karten

Apple verwendet den Begriff "graue Welt", um auf die in ios 8 integrierte Unterstützung für graue Karten zu verweisen. Der Benutzer kann sich auf eine physische graue Karte konzentrieren, die mindestens 50% der Mitte des Frames abdeckt und diese zum Anpassen des weißen Ausgleichs verwendet. Der Zweck der grauen Karte besteht darin, weiß, dass es sich um eine neutrale Karte handelt.

Dies kann in einer Anwendung implementiert werden, indem der Benutzer aufgefordert wird, eine physische graue Karte vor der Kamera zu platzieren, die `GrayWorldDeviceWhiteBalanceGains`-Eigenschaft zu überwachen und zu warten, bis die Werte abgefragt werden.

Die Anwendung würde dann den weißen Saldo für die `SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains` Methode mithilfe der Werte aus der `GrayWorldDeviceWhiteBalanceGains`-Eigenschaft sperren, um die Änderungen zu übernehmen.

Das Erfassungsgerät muss für die Konfiguration gesperrt sein, bevor eine Änderung des weißen Ausgleichs vorgenommen werden kann.

### <a name="manual-white-balance-example"></a>Beispiel für manuelles weißen Gleichgewicht

Mit dem allgemeinen Installationscode für die AV-Erfassung kann eine `UIViewController` dem Storyboard der Anwendung hinzugefügt und wie folgt konfiguriert werden:

[![](intro-to-manual-camera-controls-images/image18.png "A UIViewController can be added to the applications Storyboard and configured as shown here")](intro-to-manual-camera-controls-images/image18.png#lightbox)

Die-Sicht enthält die folgenden Hauptelemente:

- Eine `UIImageView`, die den Videofeed anzeigt.
- Eine `UISegmentedControl`, mit der der Fokus Modus von automatisch in gesperrt geändert wird.
- Zwei `UISlider` Steuerelemente, die die Temperatur und das tint anzeigen und aktualisieren.
- Eine `UIButton`, die verwendet wird, um ein Beispiel für eine graue Karte (grauer Welt) zu verwenden und den weißen Saldo mithilfe dieser Werte festzulegen.

Gehen Sie folgendermaßen vor, um den Ansichts Controller für manuelles Steuerelement für den weißen Ausgleich zu verknüpfen:

1. Fügen Sie die folgenden using-Anweisungen hinzu:

    ```csharp
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using System.Timers;
    ```

1. Fügen Sie die folgenden privaten Variablen hinzu:

    ```csharp
    #region Private Variables
    private NSError Error;
    private bool Automatic = true;
    #endregion
    ```

1. Fügen Sie die folgenden berechneten Eigenschaften hinzu:

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```

1. Fügen Sie die folgende private Methode hinzu, um die neue Farbe für den weißen Saldo und die Farbe festzulegen:

    ```csharp
    #region Private Methods
    void SetTemperatureAndTint() {
        // Grab current temp and tint
        var TempAndTint = new AVCaptureWhiteBalanceTemperatureAndTintValues (Temperature.Value, Tint.Value);

        // Convert Color space
        var gains = ThisApp.CaptureDevice.GetDeviceWhiteBalanceGains (TempAndTint);

        // Set the new values
        if (ThisApp.CaptureDevice.LockForConfiguration (out Error)) {
            gains = NomralizeGains (gains);
            ThisApp.CaptureDevice.SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains (gains, null);
            ThisApp.CaptureDevice.UnlockForConfiguration ();
        }
    }

    AVCaptureWhiteBalanceGains NomralizeGains (AVCaptureWhiteBalanceGains gains)
    {
        gains.RedGain = Math.Max (1, gains.RedGain);
        gains.BlueGain = Math.Max (1, gains.BlueGain);
        gains.GreenGain = Math.Max (1, gains.GreenGain);

        float maxGain = ThisApp.CaptureDevice.MaxWhiteBalanceGain;
        gains.RedGain = Math.Min (maxGain, gains.RedGain);
        gains.BlueGain = Math.Min (maxGain, gains.BlueGain);
        gains.GreenGain = Math.Min (maxGain, gains.GreenGain);

        return gains;
    }
    #endregion
    ```

1. Überschreiben Sie die `ViewDidLoad`-Methode, und fügen Sie folgenden Code hinzu:

    ```csharp
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();

        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;

        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;

        // Set min and max values
        Temperature.MinValue = 1000f;
        Temperature.MaxValue = 10000f;

        Tint.MinValue = -150f;
        Tint.MaxValue = 150f;

        // Create a timer to monitor and update the UI
        SampleTimer = new Timer (5000);
        SampleTimer.Elapsed += (sender, e) => {
            // Convert color space
            var TempAndTint = ThisApp.CaptureDevice.GetTemperatureAndTintValues (ThisApp.CaptureDevice.DeviceWhiteBalanceGains);

            // Update slider positions
            Temperature.BeginInvokeOnMainThread (() => {
                Temperature.Value = TempAndTint.Temperature;
            });

            Tint.BeginInvokeOnMainThread (() => {
                Tint.Value = TempAndTint.Tint;
            });
        };

        // Watch for value changes
        Segments.ValueChanged += (sender, e) => {
            // Lock device for change
            if (ThisApp.CaptureDevice.LockForConfiguration (out Error)) {

                // Take action based on the segment selected
                switch (Segments.SelectedSegment) {
                case 0:
                // Activate auto focus and start monitoring position
                    Temperature.Enabled = false;
                    Tint.Enabled = false;
                    ThisApp.CaptureDevice.WhiteBalanceMode = AVCaptureWhiteBalanceMode.ContinuousAutoWhiteBalance;
                    SampleTimer.Start ();
                    Automatic = true;
                    break;
                case 1:
                // Stop auto focus and allow the user to control the camera
                    SampleTimer.Stop ();
                    ThisApp.CaptureDevice.WhiteBalanceMode = AVCaptureWhiteBalanceMode.Locked;
                    Automatic = false;
                    Temperature.Enabled = true;
                    Tint.Enabled = true;
                    break;
                }

                // Unlock device
                ThisApp.CaptureDevice.UnlockForConfiguration ();
            }
        };

        // Monitor position changes
        Temperature.TouchUpInside += (sender, e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic)
                return;

            // Update white balance
            SetTemperatureAndTint ();
        };

        Tint.TouchUpInside += (sender, e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic)
                return;

            // Update white balance
            SetTemperatureAndTint ();
        };

        GrayCardButton.TouchUpInside += (sender, e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic)
                return;

            // Get gray card values
            var gains = ThisApp.CaptureDevice.GrayWorldDeviceWhiteBalanceGains;

            // Set the new values
            if (ThisApp.CaptureDevice.LockForConfiguration (out Error)) {
                ThisApp.CaptureDevice.SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains (gains, null);
                ThisApp.CaptureDevice.UnlockForConfiguration ();
            }
        };
    }
    ```

1. Überschreiben Sie die `ViewDidAppear`-Methode, und fügen Sie Folgendes hinzu, um aufzuzeichnen, wenn die Ansicht geladen wird:

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);

        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;

            ThisApp.Session.StartRunning ();
            SampleTimer.Start ();
        }
    }
    ```

1. Speichern Sie den in Code, und führen Sie die Anwendung aus.
1. Wenn sich die Kamera im Auto-Modus befindet, werden die Schieberegler automatisch verschoben, wenn die Kamera den weißen Saldo anpasst:

    [![](intro-to-manual-camera-controls-images/image19.png "The sliders will move automatically as the camera adjusts white balance")](intro-to-manual-camera-controls-images/image19.png#lightbox)
1. Tippen Sie auf das gesperrte Segment, und ziehen Sie die Schieberegler Temp und tint, um den weißen Saldo manuell anzupassen:

    [![](intro-to-manual-camera-controls-images/image20.png "Drag the Temp and Tint sliders to adjust the white balance manually")](intro-to-manual-camera-controls-images/image20.png#lightbox)
1. Wenn das gesperrte Segment noch ausgewählt ist, platzieren Sie eine physische graue Karte vor der Kamera, und tippen Sie auf die graue Karten Schaltfläche, um den weißen Saldo an die graue Welt anzupassen:

    [![](intro-to-manual-camera-controls-images/image21.png "Tap the Gray Card button to adjust white balance to the Gray World")](intro-to-manual-camera-controls-images/image21.png#lightbox)
1. Beendet die Anwendung.

Der obige Code hat gezeigt, wie Sie die Einstellungen für den weißen Saldo überwachen, wenn sich die Kamera im automatischen Modus befindet, oder Schieberegler verwenden, um den weißen Saldo im gesperrten Modus zu steuern.

## <a name="bracketed-capture"></a>Erfassung in Klammern

Die Erfassung in Klammern basiert auf den Einstellungen der oben dargestellten manuellen Kamera Steuerelemente und ermöglicht es der Anwendung, auf verschiedene Weise einen Zeitpunkt zu erfassen.

Die in Klammern gefasste Erfassung ist ein Burst von Bildern, die mit einer Vielzahl von Einstellungen von Bild zu Bild erstellt wurden.

[![](intro-to-manual-camera-controls-images/image22.png "How Bracketed Capture works")](intro-to-manual-camera-controls-images/image22.png#lightbox)

Mithilfe der in Klammern gesetzten Erfassung in ios 8 kann eine Anwendung eine Reihe manueller Kamera Steuerelemente vorsetzen, einen einzelnen Befehl ausgeben und die aktuelle Szene für jede manuelle Voreinstellungen eine Reihe von Bildern zurückgeben lassen.

### <a name="bracketed-capture-basics"></a>Grundlagen zur Erfassung in Klammern

Auch hier ist die Erfassung in Klammern ein Burst von Bildern, die mit unterschiedlichen Einstellungen von Bild zu Bild erstellt wurden. Die Typen der verfügbaren Erfassung in Klammern sind:

- **Klammer** für die automatische Verfügbarkeit – wobei alle Bilder einen unterschiedlichen Wert für die Größe aufweisen.
- **Hand eckige Klammer** für die Verfügbarkeit – wobei alle Bilder eine unterschiedliche Dauer (Dauer) und einen ISO-Wert aufweisen.
- **Einfache Burst Klammer** – eine Reihe von Bildern, die in schneller Folge erstellt wurden.

### <a name="new-bracketed-capture-controls-in-ios-8"></a>Neue Erfassungs Steuerelemente in Klammern in ios 8

Alle Aufzeichnungs Befehle in Klammern werden in der `AVCaptureStillImageOutput`-Klasse implementiert. Verwenden Sie die `CaptureStillImageBracket`-Methode, um eine Reihe von Bildern mit dem angegebenen Array von Einstellungen zu erhalten.

Es wurden zwei neue Klassen implementiert, um die Einstellungen zu behandeln:

- `AVCaptureAutoExposureBracketedStillImageSettings` – Sie verfügt über eine Eigenschaft, `ExposureTargetBias`, die verwendet wird, um die Verschiebung für eine Klammer für die automatische Verfügbarkeit festzulegen.
- `AVCaptureManual``ExposureBracketedStillImageSettings` – es verfügt über zwei Eigenschaften: `ExposureDuration` und `ISO`, mit denen die Auslösegeschwindigkeit und ISO für eine manuelle Klammer für die festgelegte Klammer festgelegt wird.

### <a name="bracketed-capture-controls-dos-and-donts"></a>Erfassungs Steuerelemente in Klammern

#### <a name="dos"></a>So

Im folgenden finden Sie eine Liste der Dinge, die durchgeführt werden sollten, wenn in ios 8 die in Klammern stehenden Erfassungs Steuerelemente verwendet werden:

- Bereiten Sie die APP auf die schlechteste Erfassungs Situation vor, indem Sie die `PrepareToCaptureStillImageBracket`-Methode aufrufen.
- Angenommen, die Beispiel Puffer stammen aus demselben freigegebenen Pool.
- Um den Arbeitsspeicher freizugeben, der von einem vorherigen Vorbereitungs aufrufzeichen zugewiesen wurde, können Sie `PrepareToCaptureStillImageBracket` erneut aufzurufen und ein Array mit einem Objekt senden.

#### <a name="donts"></a>Vergabe

Im folgenden finden Sie eine Liste der Dinge, die nicht durchgeführt werden sollten, wenn in ios 8 die in Klammern stehenden Erfassungs Steuerelemente verwendet werden:

- Kombinieren Sie die Erfassungs Einstellungs Typen in Klammern nicht in einer einzigen Erfassung.
- Fordern Sie nicht mehr als `MaxBracketedCaptureStillImageCount` Images in einer einzigen Erfassung an.

### <a name="bracketed-capture-details"></a>Details zur Erfassung in Klammern

Die folgenden Details sollten bei der Arbeit mit der Erfassung in Klammern in ios 8 berücksichtigt werden:

- Einstellungen in Klammern überschreiben vorübergehend die `AVCaptureDevice` Einstellungen.
- Die Einstellungen für die Flash-und Bildstabilisierung werden ignoriert.
- Alle Bilder müssen das gleiche Ausgabeformat (JPEG, PNG usw.) verwenden.
- Die Video Vorschau kann Frames löschen.
- Die Erfassung in Klammern wird auf allen mit IOS 8 kompatiblen Geräten unterstützt.

Sehen wir uns anhand dieser Informationen ein Beispiel für die Verwendung der Erfassung von Klammern in ios 8 an.

### <a name="bracket-capture-example"></a>Beispiel für Klammer Erfassung

Mit dem allgemeinen Installationscode für die AV-Erfassung kann eine `UIViewController` dem Storyboard der Anwendung hinzugefügt und wie folgt konfiguriert werden:

[![](intro-to-manual-camera-controls-images/image23.png "A UIViewController can be added to the applications Storyboard and configured as shown here")](intro-to-manual-camera-controls-images/image23.png#lightbox)

Die-Sicht enthält die folgenden Hauptelemente:

- Eine `UIImageView`, die den Videofeed anzeigt.
- Drei `UIImageViews`, die die Ergebnisse der Erfassung anzeigen.
- Eine `UIScrollView`, um den Videofeed und die Ergebnis Ansicht zu beherbergen.
- Eine `UIButton`, die verwendet wird, um eine in Klammern gefasste Erfassung mit einigen vordefinierten Einstellungen zu erstellen.

Gehen Sie folgendermaßen vor, um den Ansichts Controller für die Erfassung in Klammern zu verknüpfen:

1. Fügen Sie die folgenden using-Anweisungen hinzu:

    ```csharp
    using System;
    using System.Drawing;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using CoreImage;
    ```

1. Fügen Sie die folgenden privaten Variablen hinzu:

    ```csharp
    #region Private Variables
    private NSError Error;
    private List<UIImageView> Output = new List<UIImageView>();
    private nint OutputIndex = 0;
    #endregion
    ```

1. Fügen Sie die folgenden berechneten Eigenschaften hinzu:

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    #endregion
    ```

1. Fügen Sie die folgende private Methode hinzu, um die erforderlichen Ausgabe Bild Sichten zu erstellen:

    ```csharp
    #region Private Methods
    private UIImageView BuildOutputView(nint n) {

        // Create a new image view controller
        var imageView = new UIImageView (new CGRect (CameraView.Frame.Width * n, 0, CameraView.Frame.Width, CameraView.Frame.Height));

        // Load a temp image
        imageView.Image = UIImage.FromFile ("Default-568h@2x.png");

        // Add a label
        UILabel label = new UILabel (new CGRect (0, 20, CameraView.Frame.Width, 24));
        label.TextColor = UIColor.White;
        label.Text = string.Format ("Bracketed Image {0}", n);
        imageView.AddSubview (label);

        // Add to scrolling view
        ScrollView.AddSubview (imageView);

        // Return new image view
        return imageView;
    }
    #endregion
    ```

1. Überschreiben Sie die `ViewDidLoad`-Methode, und fügen Sie folgenden Code hinzu:

    ```csharp
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();

        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;

        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;

        // Setup scrolling area
        ScrollView.ContentSize = new SizeF (CameraView.Frame.Width * 4, CameraView.Frame.Height);

        // Add output views
        Output.Add (BuildOutputView (1));
        Output.Add (BuildOutputView (2));
        Output.Add (BuildOutputView (3));

        // Create preset settings
        var Settings = new AVCaptureBracketedStillImageSettings[] {
            AVCaptureAutoExposureBracketedStillImageSettings.Create(-2.0f),
            AVCaptureAutoExposureBracketedStillImageSettings.Create(0.0f),
            AVCaptureAutoExposureBracketedStillImageSettings.Create(2.0f)
        };

        // Wireup capture button
        CaptureButton.TouchUpInside += (sender, e) => {
            // Reset output index
            OutputIndex = 0;

            // Tell the camera that we are getting ready to do a bracketed capture
            ThisApp.StillImageOutput.PrepareToCaptureStillImageBracket(ThisApp.StillImageOutput.Connections[0],Settings,async (bool ready, NSError err) => {
                // Was there an error, if so report it
                if (err!=null) {
                    Console.WriteLine("Error: {0}",err.LocalizedDescription);
                }
            });

            // Ask the camera to snap a bracketed capture
            ThisApp.StillImageOutput.CaptureStillImageBracket(ThisApp.StillImageOutput.Connections[0],Settings, (sampleBuffer, settings, err) =>{
                // Convert raw image stream into a Core Image Image
                var imageData = AVCaptureStillImageOutput.JpegStillToNSData(sampleBuffer);
                var image = CIImage.FromData(imageData);

                // Display the resulting image
                Output[OutputIndex++].Image = UIImage.FromImage(image);

                // IMPORTANT: You must release the buffer because AVFoundation has a fixed number
                // of buffers and will stop delivering frames if it runs out.
                sampleBuffer.Dispose();
            });
        };
    }
    ```

1. Überschreiben Sie die `ViewDidAppear`-Methode, und fügen Sie folgenden Code hinzu:

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);

        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;

            ThisApp.Session.StartRunning ();
        }
    }

    ```

1. Speichern Sie den in Code, und führen Sie die Anwendung aus.
1. Erstellen Sie eine Szene, und tippen Sie auf die Schaltfläche Aufnahme Klammer

    [![](intro-to-manual-camera-controls-images/image24.png "Frame a scene and tap the Capture Bracket button")](intro-to-manual-camera-controls-images/image24.png#lightbox)
1. Wischen Sie von rechts nach links, um die drei Bilder anzuzeigen, die von der Erfassung in Klammern übernommen werden:

    [![](intro-to-manual-camera-controls-images/image25.png "Swipe right to left to see the three images taken by the Bracketed Capture")](intro-to-manual-camera-controls-images/image25.png#lightbox)
1. Beendet die Anwendung.

Der obige Code hat gezeigt, wie Sie in ios 8 eine automatische Erfassung in Klammern konfigurieren und eine automatische verfügbar machen.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben wir eine Einführung in die neuen manuellen Kamera Steuerelemente von IOS 8 behandelt und die Grundlagen der Aufgaben und deren Funktionsweise erläutert. Wir haben Beispiele für den manuellen Fokus, manuelles verfügbar machen und manuellen Leerstand angegeben. Zum Schluss haben wir ein Beispiel mit einer Erfassung in Klammern mit den zuvor beschriebenen manuellen Kamera Steuerelementen erstellt.

## <a name="related-links"></a>Verwandte Themen

- [Manualcameracontrols (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/manualcameracontrols)
- [Einführung in iOS 8](~/ios/platform/introduction-to-ios8.md)
