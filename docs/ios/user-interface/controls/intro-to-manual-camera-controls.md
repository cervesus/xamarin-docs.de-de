---
title: Steuerelemente der manuellen Kamera in Xamarin.iOS
description: Dieses Dokument beschreibt, wie das Framework der iOS-AVFoundation mit Xamarin.iOS verwendet werden kann, um Steuerelemente der manuellen Kamera zu aktivieren. Steuerelemente der manuellen Kamera zulassen ein Benutzers den Steuerelementfokus, weiß-Balance und Offenlegung von Einstellungen.
ms.prod: xamarin
ms.assetid: 56340225-5F3C-4BFC-9A79-61496D7FE5B5
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: c61b3fee9009afb86ccd3fd0e16d7812a8e90feb
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57672793"
---
# <a name="manual-camera-controls-in-xamarinios"></a>Steuerelemente der manuellen Kamera in Xamarin.iOS

Den Kamerasteuerelemente manuell, gebotenen die `AVFoundation Framework` in iOS 8 können Sie eine mobile Anwendung vollständiger Kontrolle über ein iOS-Gerät-Kamera. Erstellen professionelle Ebene Kamera-Anwendungen und Bereitstellen von Interpreten Kompositionen Anpassungen von den Parametern der Kamera und gleichzeitig eine noch Bild- oder videobearbeitung, kann dieses detaillierte Maß an Kontrolle verwendet werden.

Diese Steuerelemente können auch nützlich sein, bei der Entwicklung von Anwendungen für wissenschaftlicher oder Industrie,, in dem die Ergebnisse sind weniger darauf ausgerichtet, für die Richtigkeit oder zeichnet das Bild, und sind besser auf Hervorhebung einige Feature oder ein Element des Abbilds ausgeführt werden zugeschnitten.

## <a name="avfoundation-capture-objects"></a>AVFoundation Capture-Objekte

Ob Videos oder Verwenden der Kamera auf iOS-Geräten zu erkennen, ist der Prozess verwendet, um diese Images erfassen weitgehend identisch. Dies gilt für Anwendungen, die mit den kamerasteuerelemente automatisierte Standardverfahren oder solche, die die neuen Steuerelemente der manuellen Kamera nutzen zur Verfügung:

 [![](intro-to-manual-camera-controls-images/image1.png "Übersicht über die AVFoundation Capture-Objekte")](intro-to-manual-camera-controls-images/image1.png#lightbox)

Eingabe stammt aus einer `AVCaptureDeviceInput` in einer `AVCaptureSession` über eine `AVCaptureConnection`. Das Ergebnis ist entweder Ausgabe als ein Standbild oder als einen Videostream. Der gesamte Prozess wird gesteuert, indem ein `AVCaptureDevice`.

## <a name="manual-controls-provided"></a>Manuelle Kontrollelementen

Verwenden die neuen APIs, die von iOS 8 bereitgestellt, kann die Anwendung die folgenden Funktionen für die Kamera steuern:

-  **Manueller Fokus** – dadurch, dass dem Benutzer den Fokus direkt steuern, kann eine Anwendung bereitstellen, mehr Kontrolle über die Abbildung stammt.
-  **Manuelle Belichtung** – eine Anwendung kann durch die Bereitstellung manuell Kontrolle über die Offenlegung, Benutzern mehr Freiheit bereitstellen und ermöglicht es ihnen, einem stilisierten Erscheinungsbild zu erzielen.
-  **Manuelle weiß Saldo** – Whitepaper Saldo wird verwendet, um die Farbe in einem Bild anpassen – häufig, um realistisch aussieht. Verschiedene Lichtquellen haben andere Farbe Temperaturen, und die verwendeten Kameraeinstellungen zum Erfassen eines Abbilds wird angepasst, um diese Unterschiede zu kompensieren. In diesem Fall können Benutzer dadurch, dass das Steuerelement über die weißen Balance, Anpassungen vornehmen, die automatisch durchgeführt werden kann.


iOS 8 verfügt über Erweiterungen und Verbesserungen an vorhandenen iOS-APIs bieten diese eine präzisere Kontrolle über das Image erfassen, verarbeiten.

## <a name="bracketed-capture"></a>In Klammern gesetzten erfassen

Die Klammern erfassen wird basierend auf den Einstellungen der manuellen Kamera-Steuerelemente, die oben aufgeführten und kann die Anwendung auf einem bestimmten Zeitpunkt, in einer Vielzahl von Möglichkeiten zu erfassen.

Einfach ausgedrückt ist in Klammern Erfassen eines plötzlichen Ansturms von Standbilder, die mit dem eine Reihe von Einstellungen aus Bild zu Bild.

## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, um die in diesem Artikel vorgestellten Schritte auszuführen:

-  **Xcode 7 und iOS 8 oder höher** – Apple Xcode 7 und iOS 8 oder neueren APIs müssen installiert und konfiguriert werden, auf dem Computer des Entwicklers.
-  **Visual Studio für Mac** – die neueste Version von Visual Studio für Mac installiert und auf dem Benutzergerät konfiguriert werden soll.
-  **iOS 8 Gerät** – ein iOS-Gerät mit der neuesten Version von iOS 8. Kamera-Funktionen können nicht in der iOS-Simulator getestet werden.


## <a name="general-av-capture-setup"></a>Allgemeine AV-Capture-Setup

Bei der Aufzeichnung von Videos auf iOS-Geräten besteht einige allgemeine Setupcode, der immer erforderlich ist. Dieser Abschnitt erläutert die mindesteinstellungen, die erforderlich sind zum Aufzeichnen von Video von der Kamera für das iOS-Gerät, und zeigen Sie dieses Video in Echtzeit eine `UIImageView`.

### <a name="output-sample-buffer-delegate"></a>Ausgabe Beispiel Puffer Delegaten

Für eine der ersten Maßnahmen erforderlich gibt es einen Delegaten für den Puffer für die Ausgabe des Beispiels zu überwachen, und zeigen Sie ein Bild vom der Puffer, in einem `UIImageView` in der Benutzeroberfläche der Anwendung.

Die folgende Routine wird der Beispiel-Puffer zu überwachen und die Benutzeroberfläche aktualisiert:

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

Mit dieser Routine an Stelle der `AppDelegate` kann geändert werden, um AV erfassen Sitzung zum Aufzeichnen einer livevideofeed zu öffnen.

### <a name="creating-an-av-capture-session"></a>Erstellen einer Sitzung für AV-Erfassung

Die AV-erfassungssitzung wird verwendet, um die Aufzeichnung von live-Video von der Kamera für das iOS-Gerät zu steuern und ist erforderlich, um das video in einer iOS-Anwendung zu erhalten. Da im Beispiel `ManualCameraControl` beispielanwendung die erfassungssitzung an verschiedenen Stellen verwendet, wird es in konfiguriert die `AppDelegate` und für die gesamte Anwendung verfügbar gemacht.

Gehen Sie zum Ändern der Anwendung `AppDelegate` und fügen Sie den erforderlichen Code hinzu:


1. Doppelklicken Sie auf die `AppDelegate.cs` -Datei im Projektmappen-Explorer auf die sie für die Bearbeitung zu öffnen.
1. Fügen Sie die folgenden using-Anweisungen am Anfang der Datei:
    
    ```
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

1. Fügen Sie die folgenden privaten Variablen und die berechneten Eigenschaften, die `AppDelegate` Klasse:
    
    ```
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
  
1. Überschreiben Sie die abgeschlossene Methode aus, und ändern Sie ihn in:
    
    ```
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


Mit diesem Code werden kann den Kamerasteuerelemente manuelle problemlos zum experimentieren und Testen von implementiert werden.

## <a name="manual-focus"></a>Manueller-Fokus

Dadurch, dass der Endbenutzer direkt zu Steuerelemente den Fokus, bieten eine Anwendung veranlagten Kontrolle über die Abbildung stammt.

Z. B. Weichzeichnen berufsfotografin kann sich ein Bild, das zu erreichen einer [Bokeh Auswirkungen](https://en.wikipedia.org/wiki/Bokeh):

[![](intro-to-manual-camera-controls-images/image2.png "Ein Bokeh-Effekt")](intro-to-manual-camera-controls-images/image2.png#lightbox)

Erstellen einer [Fokus Pull Auswirkungen](http://www.mediacollege.com/video/camera/focus/pull.html), z. B.:

[![](intro-to-manual-camera-controls-images/image3.png "Der Fokus-Pull-Effekt")](intro-to-manual-camera-controls-images/image3.png#lightbox)

Für Datenanalysten oder einen Writer des medizinische Anwendungen sollten die Anwendung programmgesteuert den Fokus für Experimente verschieben. In beiden Fällen die neue API ermöglicht, dass entweder Benutzer oder die Anwendung über den Fokus zum Zeitpunkt der Abbildung Kontrolle ausgeführt wird.

### <a name="how-focus-works"></a>Funktionsweise der Fokus

Ehe wir näher auf die Details der Steuerung des Fokus in einer IOS 8-Anwendung. Werfen wir einen kurzen Blick auf die Funktionsweise der Fokus in einem iOS-Gerät:

[![](intro-to-manual-camera-controls-images/image4.png "Funktionsweise der Fokus in einem iOS-Gerät")](intro-to-manual-camera-controls-images/image4.png#lightbox)

Light gibt Kameralinse auf iOS-Geräten und konzentriert sich auf ein Image-Sensor. Der Abstand der Linse vom Sensor steuert, in dem als zentraler Punkt (der Bereich, in dem das Bild der klügsten angezeigt wird), in Beziehung zu den Sensor ist. Weiter wird auf den Fokus vom Sensor, Abstand Objekte scheinen klügsten und näher, in der Nähe von Objekten anscheinend NULL.

In einem iOS-Gerät wird die Linse Magnetquellen und Springs näher auf oder Weiter, von dem Sensor verschoben. Daher ist die exakte Positionierung der Linse unmöglich, variiert je nach Gerät und kann von Parametern wie die Ausrichtung des Geräts bzw. die dem Gerät und dem Spring beeinflusst werden.

### <a name="important-focus-terms"></a>Besonderes Augenmerk Begriffe

Beim Umgang mit Fokus sind einige Begriffe, die der Entwickler mit vertraut sein sollten:

-  **Die Tiefe des Felds** : der Abstand zwischen den nächsten und am weitesten bildschärfenmodus-Objekten. 
-  **Makro** – Dies ist der nahezu Ende des Spektrums konzentrieren und den geringsten Abstand, an dem die Linse konzentrieren kann.
-  **Unendlich** – Dies ist dem anderen Ende des Spektrums Fokus und die weiteste Entfernung, die an dem die Linse konzentrieren kann.
-  **Hyperfocal Abstand** – Dies ist der Punkt im Fokus Spektrum, bei der am weitesten entfernten Objekt, in dem Bild am äußersten Ende des Fokus. Das heißt, ist dies der als zentrales Position, die die Tiefe des Felds maximiert. 
-  **Fokus Position** – das ist was alle oben genannten Steuerelemente sonstigen Bedingungen. Dies ist der Abstand der Linse vom Sensor und somit auch der Controller den Fokus.


Mit diesen Begriffen und wissen, denken Sie daran kann die neuen Steuerelemente der manuellen Fokus erfolgreich in eine iOS 8-Anwendung implementiert werden.

### <a name="existing-focus-controls"></a>Vorhandene Fokus-Steuerelemente

iOS 7 und früheren Versionen bereitgestellte vorhandene Fokus Steuerelemente über `FocusMode`-Eigenschaft:

-   `AVCaptureFocusModeLocked` – Der Fokus wird auf einem einzelnen Fokuspunkt gesperrt.
-   `AVCaptureFocusModeAutoFocus` – Die Kamera führt ein Sweep die Linse über alle als zentraler Punkt, bis er scharf findet, und klicken Sie dann dort bleibt.
-   `AVCaptureFocusModeContinuousAutoFocus` – Die Kamera refocuses, wenn eine Out-of-Focus-Bedingung erkannt wird.


Die bestehenden Steuerelemente ebenfalls bereitgestellt, einen festlegbaren Point of Interest, über die`FocusPointOfInterest` -Eigenschaft, damit der Benutzer tippen kann, um auf einen bestimmten Bereich zu konzentrieren. Die Anwendung kann auch das Verschieben des Fokus nachverfolgen, durch die Überwachung der `IsAdjustingFocus` Eigenschaft.

Darüber hinaus Einschränkung des Zeichenbereichs bereitgestellt wurde, durch die `AutoFocusRangeRestriction` -Eigenschaft:

-   `AVCaptureAutoFocusRangeRestrictionNear` – Beschränkt den Autofocus, in der Nähe Tiefen an. In Situationen nützlich, z. B. einen QR-Code oder der Barcode scannen.
-   `AVCaptureAutoFocusRangeRestrictionFar` – Beschränkt den Autofocus zu entfernten Tiefen an. In Situationen nützlich, in denen Objekte, die bekannt ist, dass Sie nicht relevant werden in das Sichtfeld (z. B. einen Fensterrahmen) sind.


Schließlich besteht die `SmoothAutoFocus` Eigenschaft, die der Algorithmus automatisch den Fokus verlangsamt und fährt es in kleineren Schritten auszuführen, um zu vermeiden, verschieben Elemente aus, bei der Aufzeichnung des Videos.

### <a name="new-focus-controls-in-ios-8"></a>Neue Steuerelemente von den Fokus in iOS 8

Zusätzlich zu den Features, die bereits bereitgestellten von iOS 7 und höher sind die folgenden Funktionen zum Steuern des Fokus in iOS 8 verfügbar:

-  Vollständige manuelle Steuerung für die codelens-Position, an den Fokus zu sperren.
-  Schlüssel-Wert-Beobachtung der Linse Position in einem beliebigen fokusmodus.


Implementieren Sie die oben beschriebenen Funktionen, die `AVCaptureDevice` Klasse wurde geändert, um eine schreibgeschützte enthalten `LensPosition` Eigenschaft verwendet, um die aktuelle Position der Kameralinse abzurufen.

Um die manuelle Steuerung für die Position der Linse nutzen zu können, muss das Gerät erfassen im Fokusmodus gesperrt sein. Beispiel:

 `CaptureDevice.FocusMode = AVCaptureFocusMode.Locked;`

Die `SetFocusModeLocked` Methode des Geräts erfassen wird verwendet, um die Position der Kameralinse anzupassen. Eine optionale Rückrufroutine können bereitgestellt werden, um Benachrichtigungen zu erhalten, wenn die Änderung wirksam wird. Beispiel:

```csharp
ThisApp.CaptureDevice.LockForConfiguration(out Error);
ThisApp.CaptureDevice.SetFocusModeLocked(Position.Value,null);
ThisApp.CaptureDevice.UnlockForConfiguration();
```

Wie im obigen Code sehen können, muss das Gerät erfassen für die Konfiguration gesperrt werden, bevor eine Änderung an der Codelens-Position gemacht werden kann. Gültige Linse Position-Werte liegen zwischen 0,0 und 1,0.

### <a name="manual-focus-example"></a>Beispiel für die manuelle Fokus

Mit dem Code allgemeine erfassen Zugriffsverletzung-Einrichtung an Stelle einer `UIViewController` kann wie folgt konfiguriert und der Anwendung Storyboard hinzugefügt werden:

[![](intro-to-manual-camera-controls-images/image5.png "Einem uiviewcontroller-Element werden mit den Anwendungen Storyboard hinzugefügt und konfiguriert werden, wie hier gezeigt")](intro-to-manual-camera-controls-images/image5.png#lightbox)

Die Ansicht enthält die folgenden Hauptelemente:

-  Ein `UIImageView` , wird der videofeed anzeigen.
-  Ein `UISegmentedControl` , das wird nun den Fokusmodus aus automatisch gesperrt.
-  Ein `UISlider` , anzeigen und aktualisieren Sie die aktuelle Position der Linse.


Führen Sie Folgendes ein, um den ansichtscontroller für manuelle Fokus Steuerelement anschließen:


1. Fügen Sie die folgenden using-Anweisungen:

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
  
1. Überschreiben der `ViewDidLoad` Methode, und fügen Sie den folgenden Code hinzu:

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
  
1. Überschreiben der `ViewDidAppear` Methode und fügen Sie Folgendes zum Starten der Aufzeichnung der geladenen-Ansicht:

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
  
1. Mit der Kamera in der Auto-Modus werden automatisch den Schieberegler verschieben, wie die Kamera Fokus passt:

    [![](intro-to-manual-camera-controls-images/image6.png "Der Schieberegler werden automatisch verschoben werden, wie die Kamera des Fokus in dieser Beispiel-app angepasst wird.")](intro-to-manual-camera-controls-images/image6.png#lightbox)
1. Tippen Sie auf das Segment gesperrt, und ziehen Sie den Schieberegler Position, um die Position der Linse manuell anzupassen:

    [![](intro-to-manual-camera-controls-images/image7.png "Die Position der Linse anpassen manuell")](intro-to-manual-camera-controls-images/image7.png#lightbox)
1. Beenden Sie die Anwendung.


Der obige Code zeigt, wie die Position der Linse überwachen, wenn die Kamera im automatischen Modus ist, oder einen Schieberegler verwenden, um die Position der Fokus zu steuern, wenn es in den gesperrten Modus ist.

## <a name="manual-exposure"></a>Manuelle Belichtung

Offenlegung bezieht sich auf die Helligkeit des Bilds in Bezug auf die Quelle Helligkeit, und es wird bestimmt durch wie viel Licht den Sensor, wie lange und die verstärkungsstufe des Sensors (ISO-Zuordnung) erreicht. Durch die Bereitstellung manuell Kontrolle über die Offenlegung, kann eine Anwendung mehr Spielraum für den Endbenutzer bereitzustellen und ermöglicht es ihnen, einem stilisierten Erscheinungsbild zu erzielen.

Die manuelle Offenlegung-Steuerelemente verwenden, kann der Benutzer ein Bild von unrealistisch hell, dunkel und moody annehmen:

[![](intro-to-manual-camera-controls-images/image8.png "Ein Beispiel für ein Bild mit Offenlegung von unrealistisch hell, dunkel und moody")](intro-to-manual-camera-controls-images/image8.png#lightbox)

In diesem Fall kann dies erfolgen automatisch der programmgesteuerte Kontrolle für wissenschaftliche Anwendungen oder über die manuellen Steuerelemente zur Verfügung gestellt, von der Benutzeroberfläche von Anwendungen mit. In beiden Fällen stellen des neuen iOS 8 Offenlegung-APIs eine präzisere Kontrolle über die Einstellungen für die Offenlegung der Kamera.

### <a name="how-exposure-works"></a>Funktionsweise der Offenlegung

Ehe wir näher auf die Details der Steuern der Bereitstellung in einer IOS 8-Anwendung. Werfen wir einen kurzen Blick auf die Funktionsweise der Offenlegung von:

[![](intro-to-manual-camera-controls-images/image9.png "Funktionsweise der Offenlegung")](intro-to-manual-camera-controls-images/image9.png#lightbox)

Die drei grundlegenden Elemente, die zusammenkommen, um die Datenfreigabe zu kontrollieren, sind:

-  **Geschwindigkeit Auslöser** – Dies ist die Zeitspanne, die der Auslöser Licht auf den Kamerasensor können geöffnet ist. Je kürzer die Uhrzeit, die der Verschluss geöffnet ist, wird, das Licht weniger lassen und die schärfere das Image ist (weniger Motion-Weichzeichner). Je länger der Verschluss geöffnet ist, mehr Licht ist in und desto motion Unschärfe, das auftritt, können.
-  **Zuordnen von ISO** – Dies ist ein Begriff aus dem Filmfotografie übernommen und bezieht sich auf die Vertraulichkeit der Chemikalien im Film als Licht. Niedrig-ISO-Werte im Film haben weniger Granularität und eine Farbe Vervielfältigung ist. Niedrige Werte von ISO auf digitalen Sensoren haben weniger Störungen Sensor, aber weniger Helligkeit an. Je höher der ISO-Wert, der hellere das Bild jedoch weitere Sensor Störungen. "ISO" auf einem digitalen Sensor ist ein Maß [elektronische Gewinn](https://en.wikipedia.org/wiki/Gain), kein physischer Feature. 
-  **Fokus Aperture** – Dies ist die Größe der Fokus beim Öffnen. Auf allen iOS-Geräten ist die Blende fest, daher sind die nur zwei Werte, die verwendet werden können, um Offenlegung anzupassen Verschluss Geschwindigkeit und ISO.


### <a name="how-continuous-auto-exposure-works"></a>Wie kontinuierliche automatische Offenlegung funktioniert

Bevor Sie mehr darüber, wie manuelle Belichtung funktioniert, es empfiehlt sich zu ermitteln, wie kontinuierliche automatische Offenlegung funktioniert auf einem iOS-Gerät.

[![](intro-to-manual-camera-controls-images/image10.png "So funktioniert der fortlaufende automatische Offenlegung in einem iOS-Gerät")](intro-to-manual-camera-controls-images/image10.png#lightbox)

Zunächst ist die automatische Offenlegung, er ist für den Auftrag, der Berechnung der ideale Offenlegung und kontinuierlich Datenspeicherungssysteme Softwaremessung Statistiken. Sie verwendet diese Informationen, um die optimale Mischung aus ISO und Verschluss Geschwindigkeit die Szene auch leuchtet zu berechnen. Dieser Zyklus wird als AE-Schleife bezeichnet.

### <a name="how-locked-exposure-works"></a>Wie gesperrte Offenlegung funktioniert

Als Nächstes sehen wir uns wie gesperrte Offenlegung funktioniert in iOS-Geräte.

[![](intro-to-manual-camera-controls-images/image11.png "Wie gesperrte Offenlegung funktioniert in iOS-Geräte")](intro-to-manual-camera-controls-images/image11.png#lightbox)

In diesem Fall müssen Sie die automatische Offenlegung-Block, der versucht, eine optimale IOS- und Duration-Werte zu berechnen. In diesem Modus wird jedoch der AE-Block aus der Engine für die Softwaremessung-Statistiken getrennt.

### <a name="existing-exposure-controls"></a>Vorhandene Offenlegung-Steuerelemente

iOS 7 und höher geben Sie die folgenden vorhandenen Offenlegung Steuerelemente über die `ExposureMode` Eigenschaft:

-   `AVCaptureExposureModeLocked` – Beispiele für die Szene einmal, und verwendet diese Werte in der gesamten Szene.
-   `AVCaptureExposureModeContinuousAutoExposure` – Beispiele für die Szene fortlaufend, um sicherzustellen, dass er auch markiert ist.


Die `ExposurePointOfInterest` können verwendet werden, um tippen, um der Szene verfügbar zu machen, durch Auswahl eines Zielobjekts auf verfügbar machen, und die Anwendung überwachen, kann die `AdjustingExposure` Eigenschaft, um anzuzeigen, wenn Offenlegung angepasst wird.

### <a name="new-exposure-controls-in-ios-8"></a>Neue Offenlegung-Steuerelementen in iOS 8

Zusätzlich zu den Features, die bereits bereitgestellten von iOS 7 und höher sind die folgenden Funktionen zum Steuern der Offenlegung von IOS 8 verfügbar:

-  Vollständig manuelle benutzerdefinierte Offenlegung.
-  Get-, Set- und Schlüssel-Wert beobachten IOS- und Verschluss Geschwindigkeit (Dauer).


Implementieren Sie die oben beschriebenen Funktionen, eine neue `AVCaptureExposureModeCustom` Modus wurde hinzugefügt. Wenn die Kamera in der benutzerdefinierten Modus ist, kann der folgende Code verwendet werden, die Dauer der Offenlegung und ISO anpassen:

```csharp
CaptureDevice.LockForConfiguration(out Error);
CaptureDevice.LockExposure(DurationValue,ISOValue,null);
CaptureDevice.UnlockForConfiguration();
```

In den Modi "Auto" und "gesperrt" kann die Anwendung die Verschiebung der automatische Offenlegung-Routine, die mithilfe des folgenden Codes anpassen:

```csharp
CaptureDevice.LockForConfiguration(out Error);
CaptureDevice.SetExposureTargetBias(Value,null);
CaptureDevice.UnlockForConfiguration();
```

Die minimalen und maximalen Einstellung Bereiche verlassen sich auf dem Gerät, die, das die Anwendung ausgeführt wird, dass sie nie hartcodiert sein sollte. Verwenden Sie stattdessen die folgenden Eigenschaften zum Abrufen der minimalen und maximalen Wert liegt:

-   `CaptureDevice.MinExposureTargetBias` 
-   `CaptureDevice.MaxExposureTargetBias` 
-   `CaptureDevice.ActiveFormat.MinISO` 
-   `CaptureDevice.ActiveFormat.MaxISO` 
-   `CaptureDevice.ActiveFormat.MinExposureDuration` 
-   `CaptureDevice.ActiveFormat.MaxExposureDuration` 


Wie im obigen Code sehen können, muss das Gerät erfassen für die Konfiguration gesperrt werden, bevor eine Änderung der Verfügbarkeit gemacht werden kann.

### <a name="manual-exposure-example"></a>Manuelle Belichtung-Beispiel

Mit dem Code allgemeine erfassen Zugriffsverletzung-Einrichtung an Stelle einer `UIViewController` kann wie folgt konfiguriert und der Anwendung Storyboard hinzugefügt werden:

[![](intro-to-manual-camera-controls-images/image12.png "Einem uiviewcontroller-Element werden mit den Anwendungen Storyboard hinzugefügt und konfiguriert werden, wie hier gezeigt")](intro-to-manual-camera-controls-images/image12.png#lightbox)

Die Ansicht enthält die folgenden Hauptelemente:

-  Ein `UIImageView` , wird der videofeed anzeigen.
-  Ein `UISegmentedControl` , das wird nun den Fokusmodus aus automatisch gesperrt.
-  Vier `UISlider` Steuerelemente, die anzeigen und Aktualisieren der Offset, die Dauer, die ISO und die Bias werden.


Führen Sie Folgendes ein, um den ansichtscontroller für manuelle Anzeigesteuerung anschließen:


1. Fügen Sie die folgenden using-Anweisungen:
    
    ```
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
  
1. Überschreiben der `ViewDidLoad` Methode, und fügen Sie den folgenden Code hinzu:

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
  
1. Überschreiben der `ViewDidAppear` Methode und fügen Sie Folgendes zum Starten der Aufzeichnung der geladenen-Ansicht:

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
  
1. Mit der Kamera in der Auto-Modus werden der Schieberegler automatisch verschoben, wie die Kamera Offenlegung passt:

    [![](intro-to-manual-camera-controls-images/image13.png "Die Schieberegler werden automatisch verschoben werden, wie die Kamera Offenlegung angepasst wird.")](intro-to-manual-camera-controls-images/image13.png#lightbox)
1. Tippen Sie auf das Segment gesperrt, und ziehen Sie den Schieberegler Bias, um die Verschiebung des automatische Offenlegung manuell anzupassen:

    [![](intro-to-manual-camera-controls-images/image14.png "Die Verschiebung des automatische Offenlegung anpassen manuell")](intro-to-manual-camera-controls-images/image14.png#lightbox)
1. Tippen Sie auf das benutzerdefinierte-Segment aus, und ziehen Sie die Dauer und ISO-Schieberegler, um die Offenlegung manuell zu steuern:

    [![](intro-to-manual-camera-controls-images/image15.png "Ziehen Sie die Dauer und ISO-Schieberegler manuell Steuerelement Gefährdung")](intro-to-manual-camera-controls-images/image15.png#lightbox)
1. Beenden Sie die Anwendung.


Der obige Code zeigt, wie die Einstellungen für die Offenlegung zu überwachen, wenn die Kamera im automatischen Modus ist und wie Sie die Schieberegler zu verwenden, um die Datenfreigabe zu kontrollieren, wenn es in den Modi gesperrt oder Benutzerdefiniert ist.

## <a name="manual-white-balance"></a>Manuelle-weiß-Balance

Whitepaper für Steuerelemente können Benutzer anpassen, die Balance zwischen Colosr in ein Bild, das sie realistischere aussehen zu lassen. Verschiedene Lichtquellen haben andere Farbe Temperaturen, und die verwendeten Kameraeinstellungen zum Erfassen eines Abbilds angepasst werden müssen, um diese Unterschiede zu berücksichtigen. Durch zuzulassen, dass die Benutzer über die Balance weiß können sie in diesem Fall professional Anpassungen vornehmen, die die automatische Routinen Programms künstlerische Effekte zu erzielen.

[![](intro-to-manual-camera-controls-images/image16.png "Ein Beispielbild mit manuellen Whitepaper für Anpassungen")](intro-to-manual-camera-controls-images/image16.png#lightbox)

Beispielsweise verfügt die Sommerzeit keine Umwandlung bläuliches während Tungsten elektrisches Licht einen wärmere "," gelb-Orange Farbton aufweisen. ("Kalte" Farben haben verwirrend, höhere Farbe Temperaturen als "warmen" Farben. Farbe Temperaturen sind ein physisches Measure nicht Perzeptive ein.)

Der menschlichen Denken Sie daran ist sehr gut für die Unterschiede bei der Farbtemperatur kompensieren, aber dies ist etwas, das eine Kamera nicht möglich. Die Kamera funktioniert, indem Sie die Farbe für das entgegengesetzte Spektrum anpassen, die die Farbe Unterschiede boosting.

Der neuen iOS 8 ermöglicht Offenlegung-API der Anwendung, übernehmen Sie die Kontrolle des Prozesses, und geben Sie eine präzise Steuerung der Kamera weißen Netzwerklastenausgleich-Einstellungen.

### <a name="how-white-balance-works"></a>Wie weiß Saldo funktioniert

Ehe wir näher auf die Details der weißen Lastenausgleich in einer IOS 8-Anwendung steuern. Werfen wir einen kurzen Blick auf wie weiß Saldo funktioniert:

Bei der Wahrnehmung der Farbe die [CIE 1931 RGB Farbe, Speicherplatz und CIE 1931 XYZ-Farbraum](https://en.wikipedia.org/wiki/CIE_1931_color_space) sind die erste mathematisch Farbräume definiert. Sie wurden von der International-Kommission auf Beleuchtung (CIE) 1931 erstellt.

[![](intro-to-manual-camera-controls-images/image17.png "Die CIE 1931 RGB-Farbspektrum und CIE 1931 XYZ-Farbraum")](intro-to-manual-camera-controls-images/image17.png#lightbox)

Das obige Diagramm zeigt uns alle Farben sichtbar für das menschliche Auge, aus Blau Grelles Grün, Hellrot angezeigt. Mit einem X und Y-Wert, kann einem beliebigen Punkt im Diagramm gezeichnet werden, wie im obigen Diagramm gezeigt.

Als im Diagramm sichtbar ist stehen X- und Y-Werte, die im Diagramm dargestellt werden können, die außerhalb des Bereichs der menschlichen maschinelles sehen und daher diese Farben können nicht durch eine Kamera reproduziert werden kann.

Die kleineren Kurve im obigen Diagramm wird aufgerufen, die [Planckian Ursprung](https://en.wikipedia.org/wiki/Planckian_locus), die drückt der Farbtemperatur (in Grad Kelvin), auf der blauen Seite (warm) bei einer höheren und niedrigeren Zahlen seitens des roten (kälter). Dies sind besonders für typische Beleuchtung.

In gemischten Lichtverhältnissen müssen die weißen Saldo Anpassungen von den Ursprung Planckian, um die erforderlichen Änderungen vorzunehmen abweichen. In diesen Fällen müssen die Anpassung werden verschoben, entweder auf die grüne aus, oder rot/Magenta neben der CIE skalieren.

iOS-Geräte, die durch die Verstärkung des entgegengesetzten Farbe Gewinn für Umwandlungen mit Farbe kompensiert werden. Beispielsweise verfügt eine Szene zu viel Blau, werden dann der rote Gewinn verstärkt um dies auszugleichen. Diese nutzen, die Werte für bestimmte Geräte kalibriert werden, damit sie Gerät abhängig sind.

### <a name="existing-white-balance-controls"></a>Vorhandene weiß-Balance-Steuerelemente

iOS 7 und höher über die folgenden vorhandenen Whitepaper für Steuerelemente `WhiteBalanceMode` Eigenschaft:

-   `AVCapture WhiteBalance ModeLocked` – Beispiele für die Szene einmal und verwenden diese Werte in der gesamten Szene.
-   `AVCapture WhiteBalance ModeContinuousAutoExposure` – Beispiele für die Szene fortlaufend, um sicherzustellen, dass es gut ausgewogen ist.


Und die Anwendung zu überwachen, die `AdjustingWhiteBalance` Eigenschaft, um anzuzeigen, wenn Offenlegung angepasst wird.

### <a name="new-white-balance-controls-in-ios-8"></a>Neue Whitepaper für Steuerelemente in iOS 8

Zusätzlich zu den Features, die bereits bereitgestellten von iOS 7 und höher sind die folgenden Features jetzt Steuerelement Whitepaper für IOS 8 zur Verfügung:

-  Vollständig manuelle Steuerung des Geräts RGB erhält.
-  Get-, Set- und beobachten Sie Schlüssel-Wert erhält das Gerät RGB.
-  Unterstützung für das Whitepaper für eine grau-Karte.
-  Konvertierungsroutinen in und aus unabhängigen Farbräume Gerät.


Implementieren Sie die oben beschriebenen Funktionen, die `AVCaptureWhiteBalanceGain` Struktur mit den folgenden Elementen hinzugefügt wurde:

-   `RedGain` 
-   `GreenGain` 
-   `BlueGain` 


Die maximale weißen Saldo Verstärkung befindet sich derzeit vier (4) und kann von der `MaxWhiteBalanceGain` Eigenschaft. Also des zulässigen Bereichs von eins (1) um `MaxWhiteBalanceGain` (4) ist aktuell.

Die `DeviceWhiteBalanceGains` Eigenschaft kann verwendet werden, um die aktuellen Werte zu beobachten. Verwendung `SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains` anpassen, die Balance erzielt wird, wenn die Kamera im gesperrten-weiß-Balance-Modus ist.

#### <a name="conversion-routines"></a>Konvertierungsroutinen

Konvertierungsroutinen wurden hinzugefügt, um iOS 8 beim Konvertieren in und aus unabhängigen Farbräume Gerät. Konvertierungsroutinen, implementieren die `AVCaptureWhiteBalanceChromaticityValues` Struktur mit den folgenden Elementen hinzugefügt wurde:

-   `X` -ist ein Wert zwischen 0 und 1.
-   `Y` -ist ein Wert zwischen 0 und 1.


Ein `AVCaptureWhiteBalanceTemperatureAndTintValues` Struktur auch mit den folgenden Elementen hinzugefügt wurde:

-   `Temperature` – wird eine floating point-Wert in Grad Kelvin.
-   `Tint` -ist ein Offset von Grün oder von 0 auf 150 mit positiven Werten für die Richtung Grün Magenta und negativ auf die Magenta.


Verwenden der `CaptureDevice.GetTemperatureAndTintValues`und `CaptureDevice.GetDeviceWhiteBalanceGains`Methoden zum Konvertieren zwischen Temperatur und Farbton, Farbigkeit und RGB-erhalten Farbräume.

> [!NOTE]
> Konvertierungsroutinen sind genauer, je näher ist der Wert konvertiert werden, um den Ursprung Planckian.




#### <a name="gray-card-support"></a>Unterstützung für grauen Karten

Apple verwendet den Begriff grau Welt zum Verweisen auf die grau Smartcard-Unterstützung in iOS 8 integriert. Es ermöglicht den Benutzer, die auf physischen Karten bei sich grau konzentrieren, die befasst sich mit mindestens 50 % für den Mittelpunkt des Frames und verwendet, um das Gleichgewicht weiß anpassen. Der Zweck der Karte Grau ist, weiß zu erreichen, die neutrale angezeigt wird.

Dies kann in einer Anwendung durch den Benutzer auffordern, platzieren Sie die physischen Karten bei sich vor der Kamera, grauen implementiert werden Überwachung der `GrayWorldDeviceWhiteBalanceGains` -Eigenschaft, und warten, bis die Werte nach unten zu begleichen.

Die Anwendung würde dann die Gewinne weiß Saldo für Sperren die `SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains` Methode, die mit den Werten aus der `GrayWorldDeviceWhiteBalanceGains` Eigenschaft, um die Änderungen zu übernehmen.

Das Gerät erfassen müssen für die Konfiguration gesperrt werden, bevor eine Änderung in Weiß Lastenausgleich ausgeführt werden kann.

### <a name="manual-white-balance-example"></a>Manuelle-weiß-Balance-Beispiel

Mit dem Code allgemeine erfassen Zugriffsverletzung-Einrichtung an Stelle einer `UIViewController` kann wie folgt konfiguriert und der Anwendung Storyboard hinzugefügt werden:

[![](intro-to-manual-camera-controls-images/image18.png "Einem uiviewcontroller-Element werden mit den Anwendungen Storyboard hinzugefügt und konfiguriert werden, wie hier gezeigt")](intro-to-manual-camera-controls-images/image18.png#lightbox)

Die Ansicht enthält die folgenden Hauptelemente:

-  Ein `UIImageView` , wird der videofeed anzeigen.
-  Ein `UISegmentedControl` , das wird nun den Fokusmodus aus automatisch gesperrt.
-  Zwei `UISlider` Steuerelemente, die anzeigen und aktualisieren Sie die Temperatur und den Farbton.
-  Ein `UIButton` zum Beispiel ein Leerzeichen grau-Karte (grau World), und legen Sie das Whitepaper-Guthaben, die mit diesen Werten verwendet.


Führen Sie Folgendes ein, um den ansichtscontroller für manuelle weiß Saldo Steuerelement anschließen:


1. Fügen Sie die folgenden using-Anweisungen:

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
  
1. Fügen Sie die folgende private Methode zum Festlegen der neuen weiß Ausgleichen von Temperatur und den Farbton:

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
  
1. Überschreiben der `ViewDidLoad` Methode, und fügen Sie den folgenden Code hinzu:

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
  
1. Überschreiben der `ViewDidAppear` Methode und fügen Sie Folgendes zum Starten der Aufzeichnung der geladenen-Ansicht:

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
  
1. Speichern Sie die Änderungen der code, und führen Sie die Anwendung.
1. Mit der Kamera in der Auto-Modus werden der Schieberegler automatisch verschoben, wie die Kamera weißen Saldo passt:

    [![](intro-to-manual-camera-controls-images/image19.png "Die Schieberegler werden automatisch verschoben werden, wie die Kamera weißen Saldo angepasst wird.")](intro-to-manual-camera-controls-images/image19.png#lightbox)
1. Tippen Sie auf das Segment gesperrt, und ziehen Sie die Schieberegler Temp und den Farbton, um das Gleichgewicht weißen manuell anzupassen:

    [![](intro-to-manual-camera-controls-images/image20.png "Ziehen Sie die Schieberegler Temp und den Farbton den weißen Saldo manuell anpassen")](intro-to-manual-camera-controls-images/image20.png#lightbox)
1. Platzieren Sie mit dem gesperrten-Segment, das noch ausgewählt ist die physischen Karten bei sich am Anfang der Kamera grauen aus, und tippen Sie auf die Schaltfläche mit den grau-Karte, um die weißen Lastenausgleich auf der Welt grau anpassen:

    [![](intro-to-manual-camera-controls-images/image21.png "Tippen Sie auf die Schaltfläche mit den grau-Karte, um die weißen Lastenausgleich auf der Welt grau anpassen")](intro-to-manual-camera-controls-images/image21.png#lightbox)
1. Beenden Sie die Anwendung.

Der obige Code zeigt, wie die weißen Netzwerklastenausgleich-Einstellungen zu überwachen, wenn die Kamera im automatischen Modus ist, oder verwenden Schieberegler, um die weißen Balance zu steuern, wenn es in den gesperrten Modus ist.

## <a name="bracketed-capture"></a>In Klammern gesetzten erfassen

Die Klammern erfassen wird basierend auf den Einstellungen der manuellen Kamera-Steuerelemente, die oben aufgeführten und kann die Anwendung auf einem bestimmten Zeitpunkt, in einer Vielzahl von Möglichkeiten zu erfassen.

Einfach ausgedrückt ist in Klammern Erfassen eines plötzlichen Ansturms von Standbilder, die mit dem eine Reihe von Einstellungen aus Bild zu Bild.

[![](intro-to-manual-camera-controls-images/image22.png "Funktionsweise von Klammern zu erfassen.")](intro-to-manual-camera-controls-images/image22.png#lightbox)

Mit der in Klammern Capture IOS 8 kann Anwendungen eine Reihe von manuellen Kamerasteuerelemente Voreinstellung, einen einzelnen Befehl und haben die aktuelle Szene, die eine Reihe von Bildern für jede der manuellen Vorgaben zurückgeben.

### <a name="bracketed-capture-basics"></a>In Klammern gesetzten Capture-Grundlagen

In diesem Fall ist die in Klammern Erfassen eines plötzlichen Ansturms von Standbilder, die mit unterschiedlichen Einstellungen von Bild zu Bild. Die Typen von in Klammern erfassen verfügbar sind:

-  **Automatische Offenlegung Klammer** –, in dem alle Bilder über einen unterschiedlichen Bias verfügen.
-  **Manuelle Offenlegung Klammer** –, in dem alle Bilder ein verschiedene Auslöser Geschwindigkeit (Dauer) und ISO sind Betrag.
-  **Einfache Burst Klammer** – eine Reihe von Standbilder in rascher Folge erstellt.


### <a name="new-bracketed-capture-controls-in-ios-8"></a>Neues in Klammern erfassen Steuerelementen in iOS 8

Alle Befehle mit erfassen in Klammern werden implementiert, der `AVCaptureStillImageOutput` Klasse. Verwenden der `CaptureStillImageBracket`Methode, um eine Reihe von Bildern mit dem angegebenen Array der Einstellungen abzurufen.

Zwei neue Klassen wurden implementiert, um Einstellungen zu behandeln:

-   `AVCaptureAutoExposureBracketedStillImageSettings` – Verfügt über eine Eigenschaft, `ExposureTargetBias`verwendet, um die Verschiebung für eine automatische Offenlegung Klammer festgelegt. 
-   `AVCaptureManual`  `ExposureBracketedStillImageSettings` – Es hat zwei Eigenschaften `ExposureDuration` und `ISO`verwendet, um die Auslöser-Geschwindigkeit und die ISO für eine manuelle Belichtung Klammer festgelegt. 


### <a name="bracketed-capture-controls-dos-and-donts"></a>In Klammern gesetzten Capture steuert, Empfehlungen und Warnungen

#### <a name="dos"></a>Empfehlungen

Im folgenden finden eine Liste der Aufgaben, die ausgeführt werden soll, wenn mit der in Klammern Capture IOS 8-Steuerelemente:

-  Bereiten Sie die app für die Erfassung von Worst-Case-Situation durch Aufrufen der `PrepareToCaptureStillImageBracket` Methode.
-  Wird davon ausgegangen Sie, dass die Beispiel-Puffer aus dem gleichen freigegebenen Pool geschaltet werden.
-  Um den Speicher freizugeben, die von einem vorherigen Aufruf für die Vorbereitung zugeordnet wurde, rufen Sie `PrepareToCaptureStillImageBracket` erneut und ein Array von einem Objekt zu senden.


#### <a name="donts"></a>Empfehlungen

Im folgenden finden eine Liste der Aufgaben, die nicht ausgeführt werden soll, wenn mit der in Klammern Capture IOS 8-Steuerelemente:

-  Mischen Sie nicht in Klammern erfassen Einstellungstypen in eine einzelne Erfassung ein.
-  Anforderung nicht mehr als `MaxBracketedCaptureStillImageCount` Bilder in eine einzelne Erfassung ein.


### <a name="bracketed-capture-details"></a>In Klammern gesetzten Capture-Details

Folgendes sollte bei der Verwendung von Klammern erfassen IOS 8 berücksichtigt werden:

-  In Klammern stehenden Einstellungen vorübergehend außer Kraft setzen der `AVCaptureDevice` Einstellungen.
-  Flash und weiterhin die Stabilisierung-Image-Einstellungen werden ignoriert.
-  Alle Images müssen das gleiche Ausgabeformat (Jpeg, Png, usw.) verwenden.
-  Video-Vorschau kann Frames verworfen.
-  In Klammern gesetzten Capture wird auf allen Geräten, die kompatibel mit iOS 8 unterstützt.


Mit diesen Informationen Denken Sie daran werfen wir einen Blick auf ein Beispiel für IOS 8 mithilfe von Klammern zu erfassen.

### <a name="bracket-capture-example"></a>Beispiel: Erfassen

Mit dem Code allgemeine erfassen Zugriffsverletzung-Einrichtung an Stelle einer `UIViewController` kann wie folgt konfiguriert und der Anwendung Storyboard hinzugefügt werden:

[![](intro-to-manual-camera-controls-images/image23.png "Einem uiviewcontroller-Element werden mit den Anwendungen Storyboard hinzugefügt und konfiguriert werden, wie hier gezeigt")](intro-to-manual-camera-controls-images/image23.png#lightbox)

Die Ansicht enthält die folgenden Hauptelemente:

-  Ein `UIImageView` , wird der videofeed anzeigen.
-  Drei `UIImageViews` , werden die Ergebnisse der Erfassung anzeigen.
-  Ein `UIScrollView` auf den videofeed und das Ergebnis-Ansichten enthalten.
-  Ein `UIButton` verwendet, um eine in Klammern zu erfassen, die mit einigen vordefinierten Einstellungen nutzen.


Führen Sie Folgendes ein, um den ansichtscontroller für Capture Klammern anschließen:


1. Fügen Sie die folgenden using-Anweisungen:

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
  
1. Fügen Sie die folgende private Methode, um die erforderliche Ausgabe-Image-Ansichten zu erstellen:

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
  
1. Überschreiben der `ViewDidLoad` Methode, und fügen Sie den folgenden Code hinzu:
    
    ```
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
    
  
1. Überschreiben der `ViewDidAppear` Methode, und fügen Sie den folgenden Code hinzu:

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
    
1. Speichern Sie die Änderungen der code, und führen Sie die Anwendung.
1. Frame einer Szene, und tippen Sie auf die Schaltfläche mit den Klammer erfassen:

    [![](intro-to-manual-camera-controls-images/image24.png "Frame einer Szene, und tippen Sie auf die Schaltfläche \"Klammer erfassen\"")](intro-to-manual-camera-controls-images/image24.png#lightbox)
1. Streichen Sie nach von rechts nach links, um die drei Images, die von der in Klammern erfassen anzuzeigen:

    [![](intro-to-manual-camera-controls-images/image25.png "Streichen Sie nach von rechts nach links, um die drei Images, die von der in Klammern erfassen anzuzeigen")](intro-to-manual-camera-controls-images/image25.png#lightbox)
1. Beenden Sie die Anwendung.


Der obige Code zeigt, wie konfigurieren und IOS 8 nutzen eine automatische Offenlegung in Klammern zu erfassen.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben wir eine Einführung in die neuen manuellen Kamerasteuerelemente, die von iOS 8 bereitgestellten behandelt und behandelt die Grundlagen der Aktivitäten und deren Funktionsweise. Wir haben es sich um Beispiele für die manuelle Fokus, manuelle Offenlegung und manuelle weiß gegeben. Schließlich hat ein Beispiel, das dauert ein in Klammern erfasst mithilfe der zuvor beschriebenen manuellen Kamera-Steuerelemente

## <a name="related-links"></a>Verwandte Links

- [ManualCameraControls (Beispiel)](https://developer.xamarin.com/samples/monotouch/ManualCameraControls)
- [Einführung in iOS 8](~/ios/platform/introduction-to-ios8.md)
