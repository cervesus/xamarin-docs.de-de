---
title: Manuelle Kamera Steuerelemente in Xamarin.iOS
description: Dieses Dokument beschreibt, wie das iOS-AVFoundation-Framework mit Xamarin.iOS zum manuellen Kamera Steuerelemente ermöglichen verwendet werden kann. Manuelle Kamera Steuerelemente ermöglichen einem Benutzer den Steuerelementfokus, weiß Saldo und Offenlegung von Einstellungen.
ms.prod: xamarin
ms.assetid: 56340225-5F3C-4BFC-9A79-61496D7FE5B5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: a0f605a38117df87a03801c3b9d86b0b7361c232
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790824"
---
# <a name="manual-camera-controls-in-xamarinios"></a>Manuelle Kamera Steuerelemente in Xamarin.iOS

Die manuelle Kamera Kontrollmechanismen, die von bereitgestellten der `AVFoundation Framework` in iOS 8, können Sie eine mobile Anwendung mit vollständiger Kontrolle über ein iOS-Gerät Kamera. Derartige Differenzierte Steuerung kann verwendet werden, erstellen professionelle Ebene Kamera Anwendungen und Interpreten Kompositionen bereitstellen, indem Sie die Parameter der Kamera beim Offlineschalten von ein noch Bild oder ein Video optimieren.

Diese Steuerelemente können auch nützlich sein, wenn wissenschaftliche oder gewerbliche Anwendungen entwickeln, in dem die Ergebnisse sind weniger darauf ausgerichtet, gegen die Richtigkeit oder Vorteil des Bilds und Hervorhebung einige Feature oder ein Element des Bilds genommen werden mehr befasst.

## <a name="avfoundation-capture-objects"></a>AVFoundation Capture-Objekte

Ob Videos oder Bilder, die Verwendung der Kamera auf iOS-Gerät zu machen, ist der Prozess zur Aufzeichnung dieser Abbilder weitgehend identisch. Dies gilt, verwenden die standardmäßige automatische Kamera Steuerelemente oder solche, die die neue manuelle Kamera Steuerelemente nutzen:

 [![](intro-to-manual-camera-controls-images/image1.png "Übersicht über die AVFoundation Capture-Objekte")](intro-to-manual-camera-controls-images/image1.png#lightbox)

Eingabe stammt aus einer `AVCaptureDeviceInput` in einer `AVCaptureSession` über eine `AVCaptureConnection`. Das Ergebnis ist entweder Ausgabe als noch Image oder als Videostreams. Der gesamte Prozess wird gesteuert, indem ein `AVCaptureDevice`.

## <a name="manual-controls-provided"></a>Manuelle Steuerelemente bereitgestellt

Verwenden die neuen APIs, die von iOS 8 bereitgestellt, kann die Anwendung die folgenden Funktionen der Kamera steuern:

-  **Manueller Fokus** – dadurch, dass der Endbenutzer direkte Kontrolle über den Fokus zu übernehmen, eine Anwendung bieten mehr Kontrolle über das Image erstellt zugreifen kann.
-  **Manuelle Offenlegung** – eine Anwendung kann durch manuelle Steuerung der Offenlegung zur Verfügung, mehr Freiheit Benutzern zur Verfügung gestellt und ermöglicht es ihnen, ein gestalteten Erscheinungsbild zu erzielen.
-  **Manuelle weiß Saldo** – Whitepaper Saldo wird verwendet, um die Farbe eines Bildes anpassen – häufig, um realistische unübersichtlich sind. Verschiedene Lichtquellen haben andere Farbe Temperaturen sowie die verwendeten Kameraeinstellungen zum Erfassen eines Abbilds wird angepasst, um diese Unterschiede zu kompensieren. Erneut, können die Benutzer können Benutzer steuern, ob das weißen Guthaben, Anpassungen vornehmen, die automatisch durchgeführt werden kann.


iOS 8 bietet Erweiterungen und Verbesserungen an vorhandenen iOS-APIs Bereitstellen dieser eine präzisere Kontrolle über das Image erfassen Prozess.

## <a name="bracketed-capture"></a>In Klammern erfassen

Die Klammern erfassen wird auf Grundlage der Einstellungen von den oben aufgeführten manuellen Kamera-Steuerelementen und ermöglicht es der Anwendung zu einen Zeitpunkt, in einer Vielzahl von Möglichkeiten zu erfassen.

Einfach ausgedrückt, ist erfassen Klammern eine große Menge noch Bilder mit einer Vielzahl von Einstellungen aus Bild Bild.

## <a name="requirements"></a>Anforderungen

Im folgenden sind erforderlich, die in diesem Artikel vorgestellten Schritte ausführen:

-  **Xcode 7 und höher und iOS 8 oder höher** – Apple Xcode 7 und iOS 8 oder neueren-APIs installiert und auf dem Computer des Entwicklers konfiguriert werden müssen.
-  **Visual Studio für Mac** – die neueste Version von Visual Studio für Mac installiert und auf dem Benutzergerät konfiguriert werden.
-  **iOS 8 Gerät** – ein iOS-Gerät mit der neuesten Version von iOS 8. Kamera Funktionen können nicht in der iOS-Simulator getestet werden.


## <a name="general-av-capture-setup"></a>Allgemeine AV Capture-Setup

Beim Aufzeichnen von Videos auf einem iOS-Gerät ist es allgemeine setupüberprüfung Code, der immer erforderlich ist. Dieser Abschnitt befasst sich mit der minimale einrichtungsanforderungen, die erforderlich sind, Video, von dem iOS-Gerät Kamera aufzeichnen und Anzeigen von diesem Video in Echtzeit in ein `UIImageView`.

### <a name="output-sample-buffer-delegate"></a>Ausgabe Beispiel Puffer Delegaten

Eine Ihrer ersten Aufgaben benötigt ein Delegat zum Überwachen des Beispiel-Ausgabepuffers, und zeigen Sie ein Bild aus dem Puffer zu erfassten werden wird eine `UIImageView` in die Benutzeroberfläche der Anwendung.

Die folgenden Routine der Beispiel-Puffer überwacht und aktualisiert die Benutzeroberfläche:

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

Mit dieser Routine eingerichtet ist die `AppDelegate` kann geändert werden, um AV erfassen Sitzung zum Aufzeichnen von live-video-Feeds zu öffnen.

### <a name="creating-an-av-capture-session"></a>Erstellen einer Sitzung für AV-Erfassung

Die AV erfassungssitzung verwendet, um die Aufzeichnung von live-Video, von dem iOS-Gerät Kamera steuern und ist erforderlich, um das video in einer iOS-Anwendung zu erhalten. Da im Beispiel `ManualCameraControl` beispielanwendung die erfassungssitzung an verschiedenen Stellen verwendet, wird er in konfiguriert die `AppDelegate` und die gesamte Anwendung zur Verfügung gestellt.

Gehen Sie zum Ändern der Anwendung `AppDelegate` und fügen Sie den erforderlichen Code hinzu:


1. Doppelklicken Sie auf die `AppDelegate.cs` Datei im Projektmappen-Explorer, um ihn zur Bearbeitung zu öffnen.
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

1. Fügen Sie die folgenden privaten Variablen und berechnete Eigenschaften, die `AppDelegate` Klasse:
    
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
  
1. Überschreiben Sie die abgeschlossene Methode, und passen Sie ihn an:
    
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


Mit diesem Code werden kann die manuelle Kamera Steuerelemente problemlos zum experimentieren und Testen von implementiert werden.

## <a name="manual-focus"></a>Manueller Fokus

Dadurch, dass der Endbenutzer Steuerelemente den Fokus direkt benötigen, kann eine Anwendung mehr künstlerische Kontrolle über das Image erstellt bereitstellen.

Beispielsweise kann eine professionelle Fotografen weicher den Fokus von einem Bild zu erzielen eine [Bokeh Effekt](http://en.wikipedia.org/wiki/Bokeh):

[![](intro-to-manual-camera-controls-images/image2.png "Ein Effekt Bokeh")](intro-to-manual-camera-controls-images/image2.png#lightbox)

Oder erstellen Sie eine [Fokus Pull Auswirkungen](http://www.mediacollege.com/video/camera/focus/pull.html), z. B.:

[![](intro-to-manual-camera-controls-images/image3.png "Die Auswirkung der Fokus Pull")](intro-to-manual-camera-controls-images/image3.png#lightbox)

Für Wissenschaftlern oder einen Writer des medizinische Anwendungen sollten die Anwendung programmgesteuert Linse für Experimente bewegen. In beiden Fällen der neuen API ermöglicht es, dass der Benutzer oder die Anwendung die Steuerung über den Fokus zum Zeitpunkt der Image erstellt wird.

### <a name="how-focus-works"></a>Funktionsweise der Fokus

Erläutert die Details der Steuerung des Fokus in einer IOS 8-Anwendung. Werfen wir einen kurzen Blick auf die Funktionsweise der Fokus in einem iOS-Gerät:

[![](intro-to-manual-camera-controls-images/image4.png "Funktionsweise der Fokus in einem iOS-Gerät")](intro-to-manual-camera-controls-images/image4.png#lightbox)

Im hellen wechselt in den objektiv auf dem iOS-Gerät und konzentriert sich auf ein Bild-Temperatursensor. Der Abstand der Linse Sensor steuert, in dem die als zentraler Punkt (dem Bereich, in dem das Bild der besten angezeigt wird), in Beziehung zu der Sensor ist. Desto Linse ist von der Sensor, Abstand Objekte scheint besten und näher, in der Nähe von Objekten besten scheint.

In einem iOS-Gerät wird die Linse Magnete und Springs näher zu oder von der Sensor verschoben. Daher ist die genaue Positionierung der Linse unmöglich, ist abhängig vom Gerät und kann von Parametern wie die Ausrichtung des Geräts oder das Alter des Geräts und Spring beeinflusst werden.

### <a name="important-focus-terms"></a>Fokus wichtige Begriffe

Beim Umgang mit Fokus sind einige Begriffe, die der Entwickler vertraut sein sollten:

-  **Die Tiefe des Felds** – den Abstand zwischen den nächsten und den am weitesten Fokus-Objekten. 
-  **Makro** -Dies ist fast am Ende der Fokus Farbspektrum und den geringsten Abstand, bei dem der Fokus konzentrieren kann.
-  **Unendlich** – Dies ist dem anderen Ende des Spektrums Fokus und der am weitesten entfernten Abstand, an dem der Fokus konzentrieren kann.
-  **Hyperfocal Abstand** – Dies ist der Punkt im Fokus Farbspektrum ist, in dem das am weitesten entfernten Objekt im Frame am äußersten Ende des Fokus. Das heißt, ist dies die Zentrum Ankerposition, die Tiefe des Felds maximiert. 
-  **Position Tiefenschärfe** – wie alle oben genannten gesteuert werden also andere Begriffe. Dies entspricht dem Abstand der Linse Sensor und dadurch den Controller der Fokus.


Diese Begriffe und das wissen, denken Sie daran kann die neue manuelle Fokus Steuerelemente erfolgreich in eine iOS 8-Anwendung implementiert werden.

### <a name="existing-focus-controls"></a>Vorhandene Fokus-Steuerelemente

iOS 7 und früheren Versionen bereitgestellten vorhandenen Fokus-Steuerelementen über `FocusMode`-Eigenschaft:

-   `AVCaptureFocusModeLocked` – Der Fokus wird zu einem einzelnen Fokus Zeitpunkt gesperrt.
-   `AVCaptureFocusModeAutoFocus` – Die Kamera fasst den Fokus über alle als zentraler Punkt, bis er scharf findet, und klicken Sie dann dort bleibt.
-   `AVCaptureFocusModeContinuousAutoFocus` – Die Kamera refocuses immer eine Out-of-Focus Bedingung erkannt wird.


Ebenfalls bereitgestellt, die vorhandene Steuerelemente festlegbaren Interessenbereich über die`FocusPointOfInterest` -Eigenschaft, damit der Benutzer nutzen kann, um den Fokus auf einen bestimmten Bereich. Die Anwendung kann auch die Verschiebung der Linse verfolgen, indem die Überwachung der `IsAdjustingFocus` Eigenschaft.

Darüber hinaus Einschränkung des Zeichenbereichs bereitgestellt wurde, durch die `AutoFocusRangeRestriction` -Eigenschaft:

-   `AVCaptureAutoFocusRangeRestrictionNear` – Schränkt die Autofocus zu der nahe gelegenen Tiefen an. In Situationen nützlich, z. B. Scannen von QR-Code oder Barcode.
-   `AVCaptureAutoFocusRangeRestrictionFar` – Schränkt die Autofocus auf entfernten Tiefen ein. In Situationen, in denen Objekte, die bekanntermaßen nicht relevant sind in das Blickfeld (z. B. einen Fensterrahmen) sind, nützlich.


Schließlich folgt die `SmoothAutoFocus` -Eigenschaft, der automatisch den Fokus Algorithmus verlangsamt und fährt es in kleineren Schritten zu verhindern, dass Elemente beim Aufzeichnen von Videos.

### <a name="new-focus-controls-in-ios-8"></a>Neue Fokus Steuerelemente in iOS 8

Zusätzlich zu den Features, die bereits bereitgestellt von iOS 7 und höher sind die folgenden Features zum Steuern der Fokus im iOS 8 verfügbar:

-  Vollständige manuelle Steuerung der Position des Fokus beim Sperren der Fokus.
-  Schlüssel-Wert-Beobachtung der Linse Position in einem beliebigen fokusmodus.


Implementieren Sie die oben genannten Funktionen der `AVCaptureDevice` Klasse wurde geändert, um ein schreibgeschütztes enthalten `LensPosition` Eigenschaft verwendet, um die aktuelle Position der objektiv abzurufen.

Um die manuelle Steuerung der Linse Position nutzen zu können, muss das Gerät erfassen im Fokusmodus gesperrt sein. Beispiel:

 `CaptureDevice.FocusMode = AVCaptureFocusMode.Locked;`

Die `SetFocusModeLocked` Methode des Geräts erfassen wird verwendet, um die Position der Kameralinse anzupassen. Eine optionale Abbruchroutine können bereitgestellt werden, um Benachrichtigungen zu erhalten, wenn die Änderung wirksam wird. Beispiel:

```csharp
ThisApp.CaptureDevice.LockForConfiguration(out Error);
ThisApp.CaptureDevice.SetFocusModeLocked(Position.Value,null);
ThisApp.CaptureDevice.UnlockForConfiguration();
```

Wie im obigen Code sehen können, muss das Gerät erfassen für die Konfiguration gesperrt werden, bevor eine Änderung im Fokus Position vorgenommen werden kann. Gültige Linse Position-Werte liegen zwischen 0,0 und 1,0.

### <a name="manual-focus-example"></a>Manueller Fokus-Beispiel

Mit den allgemeinen AV erfassen Setupcode eingerichtet ist eine `UIViewController` an die Anwendung Storyboard hinzugefügt und wie folgt konfiguriert werden können:

[![](intro-to-manual-camera-controls-images/image5.png "Eine UIViewController kann Anwendungen Storyboard hinzugefügt und konfiguriert, wie hier gezeigt.")](intro-to-manual-camera-controls-images/image5.png#lightbox)

Die Ansicht enthält die folgenden Hauptelemente:

-  Ein `UIImageView` , wird das video-Feed anzeigen.
-  Ein `UISegmentedControl` , die ändert sich der Fokus-Modus von automatisch auf gesperrt.
-  Ein `UISlider` wird, anzeigen und Aktualisieren der aktuellen Position des Fokus.


Führen Sie Folgendes ein, um die View-Controller für manuelle Fokus Steuerelement anschließen:


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
  
1. Überschreiben Sie die `ViewDidLoad` Methode und fügen Sie den folgenden Code hinzu:

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
  
1. Überschreiben Sie die `ViewDidAppear` Methode und fügen Sie Folgendes zum Starten der Aufzeichnung die Ansicht beim Laden:

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
  
1. Mit der Kamera im Auto-Modus wird automatisch die Schieberegler bewegen, wie die Kamera passt den Fokus an:

    [![](intro-to-manual-camera-controls-images/image6.png "Der Schieberegler werden automatisch verschoben, wie die Kamera des Fokus in diesem Beispiel-app passt")](intro-to-manual-camera-controls-images/image6.png#lightbox)
1. Tippen Sie auf das Segment gesperrt, und ziehen Sie die Schieberegler, um die Position des Fokus manuell anzupassen:

    [![](intro-to-manual-camera-controls-images/image7.png "Die Position des Fokus anpassen manuell")](intro-to-manual-camera-controls-images/image7.png#lightbox)
1. Beenden Sie die Anwendung.


Der obige Code zeigt, wie die Position der Fokus im automatischen Modus wird die Kamera zu überwachen oder einen Schieberegler verwenden, um die Position der Fokus zu steuern, wenn es in den Modus gesperrt ist.

## <a name="manual-exposure"></a>Manuelle Offenlegung

Offenlegung bezieht sich auf die Helligkeit der ein Bild im Verhältnis die Helligkeit der Quelle, und wird bestimmt durch wie viel Licht den Sensor, für wie lange und durch die verstärkungsstufe der Sensor (ISO-Zuordnung) erreicht. Durch die Bereitstellung die manuelle Steuerung der Offenlegung, eine Anwendung ermöglichen mehr Freiheit für den Endbenutzer und ermöglicht es ihnen, ein gestalteten Erscheinungsbild zu erzielen.

Manuelle Offenlegung-Steuerelemente verwenden, kann der Benutzer ein Bild von unrealistisch hell, dunkel und moody annehmen:

[![](intro-to-manual-camera-controls-images/image8.png "Ein Beispiel für ein Bild, die Offenlegung von unrealistisch hell zu dunkel und moody anzeigt")](intro-to-manual-camera-controls-images/image8.png#lightbox)

Erneut, kann dies erfolgen automatisch mit programmgesteuerte Kontrolle für wissenschaftliche Anwendungen oder über manuelle Steuerelemente, die von der Benutzeroberfläche von Anwendungen bereitgestellt. In beiden Fällen geben Sie neue iOS 8 Offenlegung-APIs eine präzisere Kontrolle über die Kamera Offenlegung Einstellungen.

### <a name="how-exposure-works"></a>Funktionsweise der Offenlegung

Erläutert die Details der Steuern der Datenanzeige im ein IOS 8-Anwendung. Werfen wir einen kurzen Blick auf die Funktionsweise der Offenlegung von:

[![](intro-to-manual-camera-controls-images/image9.png "Funktionsweise der Offenlegung")](intro-to-manual-camera-controls-images/image9.png#lightbox)

Die drei grundlegenden Elemente, die zur Steuerung der Offenlegung zusammentreffen sind:

-  **Geschwindigkeit Auslöser** – Dies ist die Zeitdauer, die den Auslöser, lassen Licht auf die Kamera-Temperatursensor geöffnet ist. Je kürzer die Uhrzeit, die der Auslöser geöffnet ist, wird, das weniger Licht in können und die klarere das Bild ist (weniger Motion Blur). Je länger der Auslöser öffnen, das weitere Licht ist in und desto motion Blur, das auftritt, können.
-  **Zuordnen von ISO** – Dies ist ein Begriff aus Filme Fotos geliehener und bezieht sich auf die Vertraulichkeit dieser Chemikalien in den Film ergeben. Niedrig ISO-Werte in Filme haben weniger Körnung und eine genauere Farbe Vervielfältigung; Niedrige Werte für die ISO auf digitale Sensoren haben weniger Sensor Rauschen aber weniger Helligkeit. Je höher der ISO-Wert, desto heller das Bild ohne weitere Sensor Rauschen. "ISO" auf eine digitale Sensor ist ein Maß [elektronische Gewinn](http://en.wikipedia.org/wiki/Gain), nicht auf eine physische Funktion. 
-  **Öffnung Tiefenschärfe** – Dies ist die Größe der Linse öffnen. Auf allen iOS-Geräten ist die Blende fest, damit die nur zwei Werte, die verwendet werden können, zum Anpassen der Offenlegung Auslöser Geschwindigkeit und ISO sind.


### <a name="how-continuous-auto-exposure-works"></a>Wie kontinuierliche automatisch Offenlegung Works

Bevor Sie mehr erfahren wie manuelle Offenlegung funktioniert, es ist eine gute Idee, um zu verstehen, wie kontinuierliche automatisch Offenlegung arbeitet in einem iOS-Gerät.

[![](intro-to-manual-camera-controls-images/image10.png "Funktionsweise der Offenlegung von kontinuierlichen automatisch in einem iOS-Gerät")](intro-to-manual-camera-controls-images/image10.png#lightbox)

Zuerst wird der automatische Offenlegung-Block, weist den Auftrag zur Berechnung der ideale Offenlegung und wird fortlaufend Metering Statistiken eingezogen. Er verwendet diese Informationen zum Berechnen der optimalen Mischung von ISO "und" Auslöser Geschwindigkeit die Szene auch leuchtet abgerufen. Dieser Zyklus wird die Schleife AE genannt.

### <a name="how-locked-exposure-works"></a>Wie gesperrten Offenlegung Works

Als Nächstes sehen wir uns wie gesperrten Offenlegung funktioniert in iOS-Geräte.

[![](intro-to-manual-camera-controls-images/image11.png "Wie gesperrten Offenlegung funktioniert in iOS-Geräte")](intro-to-manual-camera-controls-images/image11.png#lightbox)

Erneut, müssen Sie die automatische Offenlegung-Block, der versucht, eine optimale IOS- und Duration-Werte zu berechnen. In diesem Modus wird jedoch der AE Block vom Modul Metering Statistiken getrennt.

### <a name="existing-exposure-controls"></a>Vorhandene Offenlegung-Steuerelemente

iOS 7 und höher, bieten die folgenden vorhandenen Offenlegung Steuerelemente über die `ExposureMode` Eigenschaft:

-   `AVCaptureExposureModeLocked` – Die Szene einmal prüft und verwendet diese Werte in der gesamten der Szene haben.
-   `AVCaptureExposureModeContinuousAutoExposure` – Prüft die Szene fortlaufend, um sicherzustellen, dass es auch leuchtet.


Die `ExposurePointOfInterest` kann, tippen Sie auf, um der Szene verfügbar zu machen, dazu ein Zielobjekt auf verfügbar gemacht, und die Anwendung überwachen kann die `AdjustingExposure` Eigenschaft, um festzustellen, wann Offenlegung angepasst wird.

### <a name="new-exposure-controls-in-ios-8"></a>Neue Offenlegung Steuerelemente in iOS 8

Zusätzlich zu den Features, die bereits bereitgestellt von iOS 7 und höher sind die folgenden Features zum Steuern der Datenanzeige im iOS 8 verfügbar:

-  Vollständig manuelle benutzerdefinierte offenlegen.
-  Get-, Set- und Schlüssel-Wert beobachten IOS- und Auslöser Geschwindigkeit (Dauer).


Implementieren Sie die oben beschriebenen Features, eine neue `AVCaptureExposureModeCustom` Modus hinzugefügt wurde. Wenn die Kamera in den benutzerdefinierten Modus ist, kann der folgende Code verwendet werden, die Offenlegung von Dauer und ISO anpassen:

```csharp
CaptureDevice.LockForConfiguration(out Error);
CaptureDevice.LockExposure(DurationValue,ISOValue,null);
CaptureDevice.UnlockForConfiguration();
```

In den Modi "Auto" und gesperrt kann anpassen, die Anwendung die Verschiebung der automatische Offenlegung Routine mit dem folgenden Code:

```csharp
CaptureDevice.LockForConfiguration(out Error);
CaptureDevice.SetExposureTargetBias(Value,null);
CaptureDevice.UnlockForConfiguration();
```

Die minimale und maximale Einstellung Bereiche verwendet wird, hängt das Gerät, das die Anwendung ausgeführt wird, sodass sie nie hartcodiert werden sollte. Verwenden Sie stattdessen die folgenden Eigenschaften, um die minimalen und maximalen Wert liegt im Bereich abzurufen:

-   `CaptureDevice.MinExposureTargetBias` 
-   `CaptureDevice.MaxExposureTargetBias` 
-   `CaptureDevice.ActiveFormat.MinISO` 
-   `CaptureDevice.ActiveFormat.MaxISO` 
-   `CaptureDevice.ActiveFormat.MinExposureDuration` 
-   `CaptureDevice.ActiveFormat.MaxExposureDuration` 


Wie im obigen Code sehen können, muss das Gerät erfassen für die Konfiguration gesperrt werden, bevor eine Änderung in Offenlegung vorgenommen werden kann.

### <a name="manual-exposure-example"></a>Manuelle Offenlegung-Beispiel

Mit den allgemeinen AV erfassen Setupcode eingerichtet ist eine `UIViewController` an die Anwendung Storyboard hinzugefügt und wie folgt konfiguriert werden können:

[![](intro-to-manual-camera-controls-images/image12.png "Eine UIViewController kann Anwendungen Storyboard hinzugefügt und konfiguriert, wie hier gezeigt.")](intro-to-manual-camera-controls-images/image12.png#lightbox)

Die Ansicht enthält die folgenden Hauptelemente:

-  Ein `UIImageView` , wird das video-Feed anzeigen.
-  Ein `UISegmentedControl` , die ändert sich der Fokus-Modus von automatisch auf gesperrt.
-  Vier `UISlider` Steuerelemente, die anzeigen und Offset, Dauer, ISO und Bias zu aktualisieren.


Führen Sie Folgendes ein, um die View-Controller für die manuelle Anzeigesteuerung anschließen:


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
  
1. Überschreiben Sie die `ViewDidLoad` Methode und fügen Sie den folgenden Code hinzu:

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
  
1. Überschreiben Sie die `ViewDidAppear` Methode und fügen Sie Folgendes zum Starten der Aufzeichnung die Ansicht beim Laden:

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
  
1. Mit der Kamera im Auto-Modus werden automatisch die Schieberegler verschieben, wie die Kamera Offenlegung passt:

    [![](intro-to-manual-camera-controls-images/image13.png "Die Schieberegler werden automatisch verschoben, wie die Kamera Offenlegung passt")](intro-to-manual-camera-controls-images/image13.png#lightbox)
1. Tippen Sie auf das Segment gesperrt, und ziehen Sie den Schieberegler Zeitunterschied die Verschiebung des automatischen Offenlegung manuell anzupassen:

    [![](intro-to-manual-camera-controls-images/image14.png "Die Verschiebung des automatischen Offenlegung anpassen manuell")](intro-to-manual-camera-controls-images/image14.png#lightbox)
1. Tippen Sie auf das benutzerdefinierte-Segment, und ziehen Sie die Dauer und ISO-Schieberegler, um die Offenlegung manuell zu steuern:

    [![](intro-to-manual-camera-controls-images/image15.png "Ziehen Sie die Dauer und ISO-Schieberegler manuell Steuerelement Gefährdung")](intro-to-manual-camera-controls-images/image15.png#lightbox)
1. Beenden Sie die Anwendung.


Der obige Code zeigt, wie die Einstellungen für die Offenlegung zu überwachen, wann die Kamera im automatischen Modus wird und wie Sie die Schieberegler zu verwenden, um die Offenlegung zu steuern, wenn es in den Modi für gesperrt oder Benutzerdefiniert ist.

## <a name="manual-white-balance"></a>Manuelle weißen Saldo

Whitepaper Saldo Steuerelemente ermöglichen Benutzern das Gleichgewicht der Colosr in einem Bild zu machen realistischere Aussehen anpassen. Verschiedene Lichtquellen haben andere Farbe Temperaturen sowie die verwendeten Kameraeinstellungen zum Erfassen eines Abbilds angepasst werden müssen, um diese Unterschiede zu berücksichtigen. Können Benutzer steuern, ob das weißen Guthaben können sie erneut, professionelle Anpassungen vorzunehmen, die die automatische Routinen nicht unterstützen künstlerische Effekte erzielen.

[![](intro-to-manual-camera-controls-images/image16.png "Ein Beispiel-Image mit manuellen weiß Saldo Korrekturen")](intro-to-manual-camera-controls-images/image16.png#lightbox)

Sommerzeit verfügt beispielsweise über eine bläuliches Umwandlung, während Wolfram elektrisches Licht Farbton für die cachevorbereitung, gelb Orange aufweisen. ("Kalte" Farben müssen verwirrend, höhere Farbe Temperaturen als "warmen" Farben. Farbe Temperaturen sind ein physisches Measure, nicht Perzeptive ein.)

Die menschliche Denken Sie daran ist sehr gut für die Unterschiede in der Farbentemperatur kompensierenden, aber dies ist etwas, das eine Kamera nicht möglich. Die Kamera funktioniert, indem die Verstärkung Farbe auf der entgegengesetzten Spektrum für die Farbe Unterschiede angepasst werden müssen.

Die neue iOS 8 Offenlegung-API ermöglicht die Anwendung die Kontrolle über den Prozess und bieten eine präzisere Kontrolle über die Kamera weißen Netzwerklastenausgleich-Einstellungen.

### <a name="how-white-balance-works"></a>Wie weiß Saldo Works

Erläutert die Details zum Steuern des weißen Saldo ein IOS 8-Anwendung. Werfen wir einen Blick darauf wie weiß Saldo funktioniert:

In der Studie von Gefühl Farbe die [CEI 1931 RGB Farbe, Speicherplatz und CEI 1931 XYZ Farbraum](http://en.wikipedia.org/wiki/CIE_1931_color_space) sind die erste mathematisch Farbräumen definiert. Sie können in 1931 wurden durch die Internationale Kommission auf Beleuchtung (CIE) erstellt.

[![](intro-to-manual-camera-controls-images/image17.png "Die CEI 1931 RGB-Farbspektrum und CEI 1931 XYZ Farbspektrum")](intro-to-manual-camera-controls-images/image17.png#lightbox)

Im obigen Diagramm uns alle Farben Tabellenlayout, aus Blau hellgrün hellen rot angezeigt. Mit einem X und Y-Wert, kann einen beliebigen Punkt im Diagramm gezeichnet werden, wie in der Abbildung weiter oben gezeigt.

Wie im Diagramm sichtbar X- und Y-Werte, die im Diagramm angezeigt werden können, die außerhalb des Bereichs der menschlichen Vision vorhanden sind, und daher diese Farben können nicht von einer Kamera reproduziert werden kann.

Die kleinere Kurve im obigen Diagramm wird aufgerufen, die [Planckian Locus](http://en.wikipedia.org/wiki/Planckian_locus), die die Farbentemperatur (in Grad Kelvin) mit einer höheren Anzahl auf der blauen Seite (eine bessere Datenbasis) drückt und unten auf der Seite Rot (Kühlgerät) nummeriert. Diese sind für typische Beleuchtung Situationen nützlich.

Unter Umständen gemischten Beleuchtung müssen weißen Saldo Korrekturen von der Planckian Locus die erforderlichen Änderungen vor abweichen. In diesen Situationen die Anpassung benötigten werden verschoben wird, wie auf die grüne, oder rot/Magenta neben der CIE skalieren.

iOS-Geräte, die von den entgegengesetzten Farbe Gewinn Verstärkung Farbe Umwandlungen kompensiert werden. Beispielsweise verfügt eine Szene zu viel Blau, wird dann rote Verstärkung erhöht werden um dies auszugleichen. Diese Werte werden für bestimmte Geräte kalibriert, damit sie Gerät abhängig sind, entsteht.

### <a name="existing-white-balance-controls"></a>Vorhandene weiß-Balance-Steuerelemente

iOS 7 und höher bereitgestellt über die folgenden vorhandenen weiß Saldo Steuerelemente `WhiteBalanceMode` Eigenschaft:

-   `AVCapture WhiteBalance ModeLocked` – Prüft der Szene einmal und verwenden diese Werte in der gesamten der Szene haben.
-   `AVCapture WhiteBalance ModeContinuousAutoExposure` – Prüft die Szene fortlaufend, um sicherzustellen, dass sie gut ausgewogen ist.


Und die Anwendung zu überwachen, die `AdjustingWhiteBalance` Eigenschaft, um festzustellen, wann Offenlegung angepasst wird.

### <a name="new-white-balance-controls-in-ios-8"></a>Neuen weiß Balance-Steuerelemente in iOS 8

Zusätzlich zu den Features, die bereits bereitgestellt von iOS 7 und höher sind die folgenden Features jetzt Steuerelement weiß Saldo in iOS 8 zur Verfügung:

-  Vollständig manuelle Kontrolle des Geräts RGB erhält.
-  Get-, Set- und beobachten Sie die Schlüssel-Wert erhält das Gerät RGB.
-  Unterstützung für weiß Lastenausgleich mit einem grau-Karte.
-  Konvertierungsroutinen in und aus dem geräteunabhängigen Farbräumen Gerät.


Implementieren Sie die oben beschriebenen Features, die `AVCaptureWhiteBalanceGain` Struktur mit den folgenden Membern hinzugefügt wurde:

-   `RedGain` 
-   `GreenGain` 
-   `BlueGain` 


Die maximale weißen Saldo Gewinn ist derzeit vier (4) und kann von der `MaxWhiteBalanceGain` Eigenschaft. Damit des zulässigen Bereichs von eins (1) zur wird `MaxWhiteBalanceGain` (4) derzeit.

Die `DeviceWhiteBalanceGains` Eigenschaft kann verwendet werden, um die aktuellen Werte zu beobachten. Verwendung `SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains` , passen Sie das Guthaben erhält, wird die Kamera in der gesperrten weiß-Balance-Modus.

#### <a name="conversion-routines"></a>Konvertierungsroutinen

Konvertierungsroutinen wurden hinzugefügt, um iOS 8 unterstützen Sie beim Konvertieren in und aus unabhängigen Farbräumen Gerät. Zum Implementieren der Konvertierungsroutinen der `AVCaptureWhiteBalanceChromaticityValues` Struktur mit den folgenden Membern hinzugefügt wurde:

-   `X` -ist ein Wert zwischen 0 und 1.
-   `Y` -ist ein Wert zwischen 0 und 1.


Eine `AVCaptureWhiteBalanceTemperatureAndTintValues` Struktur auch mit den folgenden Membern hinzugefügt wurde:

-   `Temperature` -Ein Gleitkommatyp Wert in Grad Kelvin zeigen.
-   `Tint` -ein Offset von Grün oder von 0 auf 150 mit positiven Werten für die grünen Richtung Magenta und gegen die Magenta negativ ist.


Verwenden der `CaptureDevice.GetTemperatureAndTintValues`und `CaptureDevice.GetDeviceWhiteBalanceGains`Methoden zum Konvertieren zwischen Temperatur und Farbton, Farbigkeit und RGB erhalten Farbräumen.

> [!NOTE]
> Die Konvertierungsroutinen sind genauer näher ist der Wert konvertiert werden, die Planckian Locus.




#### <a name="gray-card-support"></a>Graue-Unterstützung

Apple wird mit dem Begriff grau World zum Verweisen auf die Karte grau Unterstützung iOS 8 integriert. Er ermöglicht es dem Benutzer, die auf physischen Karten bei sich grau konzentrieren, die Sie Informationen zu mindestens 50 % der in der Mitte des Rahmens sowie verwendet, um den Saldo weiß anpassen. Der Zweck der Karte Grau ist, weiß zu erreichen, die neutrale angezeigt wird.

Dies kann in einer Anwendung durch der Benutzer aufgefordert, platzieren Sie die physischen Karten bei sich vor der Kamera, grauen implementiert werden Überwachung der `GrayWorldDeviceWhiteBalanceGains` Eigenschaft und warten, bis die Werte unten ausgleichen.

Die Anwendung würde dann die Gewinne weiß Saldo für Sperren die `SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains` Methode anhand der Werte auf die `GrayWorldDeviceWhiteBalanceGains` Eigenschaft, um die Änderungen zu übernehmen.

Das Gerät erfassen muss für die Konfiguration gesperrt werden, bevor eine Änderung in Weiß Lastenausgleich vorgenommen werden kann.

### <a name="manual-white-balance-example"></a>Manuelle weiß-Balance-Beispiel

Mit den allgemeinen AV erfassen Setupcode eingerichtet ist eine `UIViewController` an die Anwendung Storyboard hinzugefügt und wie folgt konfiguriert werden können:

[![](intro-to-manual-camera-controls-images/image18.png "Eine UIViewController kann Anwendungen Storyboard hinzugefügt und konfiguriert, wie hier gezeigt.")](intro-to-manual-camera-controls-images/image18.png#lightbox)

Die Ansicht enthält die folgenden Hauptelemente:

-  Ein `UIImageView` , wird das video-Feed anzeigen.
-  Ein `UISegmentedControl` , die ändert sich der Fokus-Modus von automatisch auf gesperrt.
-  Zwei `UISlider` Steuerelemente, die anzeigen und aktualisieren Sie die Temperatur und Farbton.
-  Ein `UIButton` zum Beispiel ein Leerzeichen grau-Karte (grau World), und legen Sie das White-Guthaben mit diesen Werten verwendet.


Führen Sie Folgendes ein, um die View-Controller für manuelle weiß Saldo Steuerelement anschließen:


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
  
1. Fügen Sie die folgende private Methode zum Festlegen der neuen weiß Temperatur und Farbton ausgleichen:

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
  
1. Überschreiben Sie die `ViewDidLoad` Methode und fügen Sie den folgenden Code hinzu:

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
  
1. Überschreiben Sie die `ViewDidAppear` Methode und fügen Sie Folgendes zum Starten der Aufzeichnung die Ansicht beim Laden:

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
  
1. Speichern Sie die Änderungen der codieren, und führen Sie die Anwendung.
1. Mit der Kamera im Auto-Modus werden automatisch die Schieberegler verschieben, wie die Kamera weißen Saldo passt:

    [![](intro-to-manual-camera-controls-images/image19.png "Die Schieberegler werden automatisch verschoben, wie die Kamera weißen Saldo passt")](intro-to-manual-camera-controls-images/image19.png#lightbox)
1. Tippen Sie auf das Segment gesperrt, und ziehen Sie die Temp "und" Farbton Schieberegler, um den weißen Saldo manuell anzupassen:

    [![](intro-to-manual-camera-controls-images/image20.png "Ziehen Sie die Temp \"und\" Farbton Schieberegler, um den weißen Saldo manuell anzupassen")](intro-to-manual-camera-controls-images/image20.png#lightbox)
1. Platzieren Sie mit dem gesperrt-Segment ausgewählter die physischen Karten bei sich am Anfang der Kamera grauen aus, und tippen Sie auf die grau Karte-Schaltfläche, um auf der Welt grau weißen ausgleichen:

    [![](intro-to-manual-camera-controls-images/image21.png "Tippen Sie auf die grau Karte-Schaltfläche, um auf der Welt grau weißen ausgleichen")](intro-to-manual-camera-controls-images/image21.png#lightbox)
1. Beenden Sie die Anwendung.

Der obige Code zeigt, wie die weißen Netzwerklastenausgleich-Einstellungen im automatischen Modus wird die Kamera zu überwachen oder verwenden Schieberegler, um den weißen Saldo zu steuern, wenn es in den Modus gesperrt ist.

## <a name="bracketed-capture"></a>In Klammern erfassen

Die Klammern erfassen wird auf Grundlage der Einstellungen von den oben aufgeführten manuellen Kamera-Steuerelementen und ermöglicht es der Anwendung zu einen Zeitpunkt, in einer Vielzahl von Möglichkeiten zu erfassen.

Einfach ausgedrückt, ist erfassen Klammern eine große Menge noch Bilder mit einer Vielzahl von Einstellungen aus Bild Bild.

[![](intro-to-manual-camera-controls-images/image22.png "Funktionsweise von Klammern zu erfassen.")](intro-to-manual-camera-controls-images/image22.png#lightbox)

Mithilfe von Klammern erfassen in iOS 8, kann eine Anwendung Voreinstellung verwenden, eine Reihe von manuellen Kamera Steuerelemente, einen einzelnen Befehl und der aktuellen Szene haben eine Reihe von Bildern für jede der manuellen Voreinstellungen zurückgeben.

### <a name="bracketed-capture-basics"></a>In Klammern Capture-Grundlagen

Erfassen von Klammern ist, eine große Menge noch Bilder mit unterschiedlichen Einstellungen von Bild zu Bild. Die Typen von Klammern erfassen verfügbar sind:

-  **Automatische Offenlegung Klammer** –, in dem alle Bilder über einen unterschiedlichen Bias verfügen.
-  **Manuelle Offenlegung Klammer** –, in dem alle Bilder einer vielfältigen Auslöser Geschwindigkeit (Dauer) und ISO haben Betrag.
-  **Einfache Burst Klammer** – eine Reihe von weiterhin in rascher ausgeführten Bildern.


### <a name="new-bracketed-capture-controls-in-ios-8"></a>Neuen Klammern erfassen Steuerelemente in iOS 8

Alle Klammern erfassen Befehle werden implementiert, der `AVCaptureStillImageOutput` Klasse. Verwenden der `CaptureStillImageBracket`Methode, um eine Reihe von Bildern mit dem angegebenen Array von Einstellungen abzurufen.

Zwei neue Klassen es wurden implementiert, um Einstellungen zu behandeln:

-   `AVCaptureAutoExposureBracketedStillImageSettings` – Verfügt über eine Eigenschaft, `ExposureTargetBias`, mit denen die Verschiebung für eine automatische Offenlegung Klammer festgelegt. 
-   `AVCaptureManual`  `ExposureBracketedStillImageSettings` – sie verfügt über zwei Eigenschaften `ExposureDuration` und `ISO`, mit denen der Auslöser Geschwindigkeit und ISO für eine manuelle Offenlegung Klammer festlegen. 


### <a name="bracketed-capture-controls-dos-and-donts"></a>In Klammern Capture steuert, Richtlinien für Kennwörter

#### <a name="dos"></a>Aufgaben

Im folgenden finden eine Liste der Aufgaben, die ausgeführt werden soll, wenn mithilfe von Klammern Capture in iOS 8-Steuerelemente:

-  Vorbereiten der Anwendung für die Erfassung Worst-Case-Situation durch Aufrufen der `PrepareToCaptureStillImageBracket` Methode.
-  Wird davon ausgegangen Sie, dass die Puffer Beispiel stammen aus dem gleichen freigegebenen Pool beabsichtigen.
-  Rufen Sie zum Freigeben des Speichers, der durch einen vorherigen Aufruf von Prepare belegt wurde, `PrepareToCaptureStillImageBracket` erneut aus, und senden Sie es ein Array des einen Objekts.


#### <a name="donts"></a>Für die Navigation

Im folgenden finden eine Liste der Aufgaben, die nicht ausgeführt werden soll, wenn mithilfe von Klammern Capture in iOS 8-Steuerelemente:

-  Verwenden Sie Klammern erfassen Einstellungen Typen in eine einzelne Erfassung ein.
-  Anforderung nicht mehr als `MaxBracketedCaptureStillImageCount` Bilder in eine einzelne Erfassung ein.


### <a name="bracketed-capture-details"></a>In Klammern Capture-Details

Folgendes sollte bei der Verwendung von Klammern erfassen in iOS 8 berücksichtigt werden:

-  Vorübergehendes Überschreiben von Einstellungen in Klammern der `AVCaptureDevice` Einstellungen.
-  Flash und weiterhin die Stabilisierung für Bilder werden ignoriert.
-  Alle Images müssen die gleiche Ausgabeformat (Jpeg, Png, usw.) verwenden.
-  Video-Vorschau kann Frames löschen.
-  Auf allen Geräten mit iOS 8 kompatibel wird in Klammern Capture unterstützt.


Mit diesen Informationen Denken Sie daran werfen wir einen Blick auf ein Beispiel der Verwendung von Klammern erfassen in iOS 8.

### <a name="bracket-capture-example"></a>Beispiel: Erfassen

Mit den allgemeinen AV erfassen Setupcode eingerichtet ist eine `UIViewController` an die Anwendung Storyboard hinzugefügt und wie folgt konfiguriert werden können:

[![](intro-to-manual-camera-controls-images/image23.png "Eine UIViewController kann Anwendungen Storyboard hinzugefügt und konfiguriert, wie hier gezeigt.")](intro-to-manual-camera-controls-images/image23.png#lightbox)

Die Ansicht enthält die folgenden Hauptelemente:

-  Ein `UIImageView` , wird das video-Feed anzeigen.
-  Drei `UIImageViews` , werden die Ergebnisse der Erfassung anzeigen.
-  Ein `UIScrollView` für die Videobeitrag und Ergebnis-Ansichten.
-  Ein `UIButton` verwendet, um die Klammern erfassen bei einige Voreinstellungen verwendet werden.


Führen Sie Folgendes ein, um die View-Controller für Klammern Capture anschließen:


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
  
1. Fügen Sie die folgende private Methode zum Erstellen der erforderlichen Ausgabe Image Ansichten hinzu:

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
  
1. Überschreiben Sie die `ViewDidLoad` Methode und fügen Sie den folgenden Code hinzu:
    
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
    
  
1. Überschreiben Sie die `ViewDidAppear` Methode und fügen Sie den folgenden Code hinzu:

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
    
1. Speichern Sie die Änderungen der codieren, und führen Sie die Anwendung.
1. Eine Szene Frame aus, und tippen Sie auf die Schaltfläche "erfassen Klammer":

    [![](intro-to-manual-camera-controls-images/image24.png "Rahmen Sie eine Szene, und tippen Sie auf die Schaltfläche \"erfassen Klammer\"")](intro-to-manual-camera-controls-images/image24.png#lightbox)
1. Streichen Sie nach rechts nach links, um die drei Grafiken genommene Klammern erfassen finden Sie unter:

    [![](intro-to-manual-camera-controls-images/image25.png "Streichen Sie nach rechts nach links, um die drei Grafiken genommene Klammern erfassen finden Sie unter")](intro-to-manual-camera-controls-images/image25.png#lightbox)
1. Beenden Sie die Anwendung.


Der obige Code zeigt, wie konfigurieren und iOS 8 nehmen eine automatische Offenlegung Klammern zu erfassen.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben wir eine Einführung in die neue manuelle Kamera Steuerelemente bereitgestellt, die von iOS 8 behandelt und behandelt die Grundlagen von was sie tun und deren Funktionsweise. Wir haben Beispiele manuell den Fokus, manuelle Offenlegung und manuelle weiß Saldo gegeben. Ein Beispiel für eine Klammern erfassen mit den bereits vorgestellten manuelle Kamera Steuerelementen dauert schließlich hat

## <a name="related-links"></a>Verwandte Links

- [ManualCameraControls (Beispiel)](https://developer.xamarin.com/samples/monotouch/ManualCameraControls)
- [Einführung in iOS 8](~/ios/platform/introduction-to-ios8.md)
