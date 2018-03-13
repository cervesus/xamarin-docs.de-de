---
title: Verwenden von ARKit mit UrhoSharp
ms.topic: article
ms.prod: xamarin
ms.assetid: 877AF974-CC2E-48A2-8E1A-0EF9ABF2C92D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/01/2016
ms.openlocfilehash: 94963e92f8372316a982bbe38f1fb653d38b2a3b
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="using-arkit-with-urhosharp"></a>Verwenden von ARKit mit UrhoSharp

Durch die Einführung des [ARKit](https://developer.apple.com/arkit/), Apple machte es einfache für Entwickler zum Erstellen von Anwendungen augmented Reality-Modus. ARKit verfolgen Sie die genaue Position des Geräts und verschiedene Flächen auf der ganzen Welt erkennen kann, und nun ist es Aufgabe des Entwicklers, die Daten stammen aus ARKit in Ihren Code in blend.

[UrhoSharp](~/graphics-games/urhosharp/index.md) bietet eine umfassende und einfach zu verwendende 3D-API, die Sie zum Erstellen von 3D Anwendungen verwenden können.   Beide können werden gemischt zusammen, die physische Anmeldeinformationen über die Welt ARKit und Urho, um die Ergebnisse zu rendern.

Auf dieser Seite wird erläutert, wie diese beiden Welten zusammen zum Erstellen von großen augmented Reality-Modus-Anwendungen eine Verbindung herstellen.


## <a name="the-basics"></a>Die Grundlagen

Was wir wollen ist vorhanden-3D-Inhalte auf der ganzen Welt, sieht das iPhone.   Die Idee besteht darin, den Inhalt des Telefons Kamera mit den 3D-Inhalt stammt blend, und wie den Platz um sicherzustellen, dass der Benutzer der Telefonnummer bewegt 3D-Objekt Verhalten, da sie Teil dieser Platz ist – dies wird durch die Objekte in dieser Welt verankern.

![Animierte Abbildung im ARKit](urhosharp-images/image1.gif)


Wir verwenden die Urho-Bibliothek auf unsere 3D Objekte laden und auf der ganzen Welt zu platzieren, und wir verwenden ARKit abzurufenden den Videostream stammt von der Kamera sowie den Speicherort der Telefonnummer in der ganzen Welt.   Wenn der Benutzer mit seinem Telefon bewegt, verwenden wir die Änderungen am Speicherort auf um das Koordinatensystem zu aktualisieren, das das Modul Urho angezeigt wird.

Auf diese Weise spiegelt wider, wenn Sie ein Objekt in der 3D-Bereich platzieren und der Benutzer wechselt, der Speicherort der 3D-Objekt den Ort und Stelle, wo sie abgelegt wurde.

## <a name="setting-up-your-application"></a>Einrichten Ihrer dienstanwendung

### <a name="ios-application-launch"></a>iOS-Anwendung starten

Die iOS-Anwendung zu erstellen und starten Sie Ihren 3D-Inhalt benötigt, hierzu erstellen ein durch eine Unterklasse von der [ `Urho.Application` ](https://developer.xamarin.com/api/type/Urho.Application/) , und geben Sie den Setupcode durch Überschreiben der `Start` Methode.  Dies ist, in dem die Szene mit Daten, Ereignis-Handler werden usw. einrichten Werten aufgefüllt wird.

Wir eingeführt, ein `Urho.ArkitApp` Klasse diese Unterklassen `Urho.Application` und klicken Sie auf die `Start` Methode bewirkt die anstrengende Arbeit.   Sie müssen lediglich auf Ihre vorhandenen Urho Anwendung ist die Basisklasse Datentyp ändern `Urho.ArkitApp` und Sie haben eine Anwendung, die Ihre Urho Szene in der ganzen Welt ausgeführt werden.

### <a name="the-arkitapp-class"></a>Die ArkitApp-Klasse

Diese Klasse stellt einen Satz von geeigneten Standardwerte, die beide eine Szene mit einigen Schlüsselobjekte sowie die Verarbeitung von Ereignissen ARKit bereit, wie sie vom Betriebssystem übermittelt werden.

Das Setup erfolgt der `Start` virtuelle Methode.   Wenn Sie auf die Unterklasse diese Methode überschreiben, müssen Sie die Kette übergeordneter mit sicherstellen, `base.Start()` auf Ihre eigene Implementierung.

Die `Start` Methode richtet der Szene, Viewports, Kamera und eine direktionale helle und sicherheitsänderungen angezeigt, die als öffentliche Eigenschaften:

- eine [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Scene/) um Objekte zu speichern.
- eine direktionale [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light/) mit Schatten und deren Speicherort über verfügbar ist die `LightNode` Eigenschaft
- eine [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera/) , dessen Komponenten werden aktualisiert, wenn ARKit ein Update an die Anwendung übermittelt und
- eine [ `ViewPort` ](https://developer.xamarin.com/api/type/Urho.Viewport/) zum Anzeigen der Ergebnisse.


### <a name="your-code"></a>Der code

Sie müssen dann Unterklasse der `ArkitApp` Klasse, und überschreiben die `Start` Methode.   Die die Methode sollte als Erstes wird Kette bis zu der `ArkitApp.Start` durch Aufrufen von `base.Start()`.  Danach können Sie eine der Eigenschaften Setup durch ArkitApp der Szene Ihrer Objekte hinzufügen, Anpassen der Leuchten, Schatten oder Ereignisse, die Sie behandeln möchten.

Die ARKit/UrhoSharp-Beispiel lädt ein animiertes Zeichen mit Texturen und gibt die Animation, mit der folgenden Implementierung:

    ```csharp
    public class MutantDemo : ArkitApp
    {
        [Preserve]
        public MutantDemo(ApplicationOptions opts) : base(opts) { }

        Node mutantNode;

        protected override void Start()
        {
            base.Start ();

            // Mutant
            mutantNode = Scene.CreateChild();
            mutantNode.Rotation = new Quaternion(x: 0, y:15, z:0);
            mutantNode.Position = new Vector3(0, -1f, 2f); /*two meters away*/
            mutantNode.SetScale(0.5f);

            var mutant = mutantNode.CreateComponent<AnimatedModel>();
            mutant.Model = ResourceCache.GetModel("Models/Mutant.mdl");
            mutant.Material = ResourceCache.GetMaterial("Materials/mutant_M.xml");

            var animation = mutantNode.CreateComponent<AnimationController>();
            animation.Play("Animations/Mutant_HipHop1.ani", 0, true, 0.2f);
        }
    }
    ```

Und das ist wirklich, Sie haben an dieser Stelle tun, damit Ihre 3D-Inhalt im augmented Reality-Modus angezeigt haben.

Urho verwendet benutzerdefinierte Formate für 3D-Modelle und Animationen, daher Sie Ihre Medienobjekte in dieses Format zu exportieren müssen.   Sie können Tools wie der [Urho3D Blender-Add-in](https://github.com/reattiva/Urho3D-Blender) und [UrhoAssetImporter](https://github.com/EgorBo/UrhoAssetImporter) , konvertiert werden kann diese Ressourcen aus häufig verwendeten Formaten wie DBX "," DAE "," OBJ "," Blend 3D-Max in das Format von Urho erforderlich.

Weitere Informationen zum Erstellen von 3D Anwendungen mithilfe von Urho finden Sie auf der [Einführung in UrhoSharp](~/graphics-games/urhosharp/introduction.md) Handbuch.

## <a name="arkitapp-in-depth"></a>ArkitApp im Detail

> [!NOTE]
> In diesem Abschnitt ist für Entwickler vorgesehen, die die standarderfahrung UrhoSharp und ARKit angepasst werden soll, oder einen einen tieferen Einblick in die Funktionsweise der Integration abgerufen werden soll.   Es ist nicht notwendig, diesen Abschnitt lesen.

Die ARKit-API ist ganz einfach, Sie erstellen und Konfigurieren einer [ARSession](https://developer.apple.com/documentation/arkit/arsession) -Objekt, das ein, und starten Sie die Bereitstellung von [ARFrame](https://developer.apple.com/documentation/arkit/arframe) Objekte.   Diese enthalten sowohl die von der Kamera sowie die geschätzte realen Position des Geräts erfasste Abbild.

Wir werden die Images wird von der Kamera übermittelt, uns mit unserem 3D-Inhalt verfassen, und passen Sie die Kamera in UrhoSharp entsprechend der Wahrscheinlichkeit in der Gerätespeicherort und die Position.

Das folgende Diagramm zeigt, was erfolgt der `ArkitApp` Klasse:

[![Diagramm von Klassen und Bildschirme in der ArkitApp](urhosharp-images/image2.png)](urhosharp-images/image2.png#lightbox)

### <a name="rendering-the-frames"></a>Rendering von Frames

Die Idee ist einfach, kombinieren das Video aus der Kamera mit unserem 3D-Grafiken kombinierte Image erstellen.     Wir werden eine Reihe von diesen erfasste Abbilder wird nacheinander abrufen, und wir werden diese Eingabe mit der Szene Urho mischen.

Die einfachste Möglichkeit dafür ist das Einfügen einer [ `RenderPathCommand` ](https://developer.xamarin.com/api/type/Urho.RenderPathCommand/) in die Main [ `RenderPath` ](https://developer.xamarin.com/api/type/Urho.RenderPath/).  Dies ist eine Reihe von Befehlen, die ausgeführt werden, um ein einzelnes Bild gezeichnet werden soll.  Mit diesem Befehl wird der Viewport mit jede Textur ausgefüllt ist. wir übergeben.    Wir diese Einstellung wird auf das erste Frame, der Prozess ist, und der eigentlichen Klassendefinition erfolgt in th **ARRenderPath.xml** Datei, die zu diesem Zeitpunkt geladen wird.

Wir müssen jedoch zwei Probleme auf diese beiden Welten ineinander Datenwachstums:


1. Texturen GPU benötigen bei iOS kann eine Lösung besteht darin, die eine Potenz von zwei ist, aber die Frames, die wir von der Kamera erhält keine Lösung, die eine Potenz von zwei, z. B.: 1280 x 720.
2. Werden die Frames im codiert [YUV](https://en.wikipedia.org/wiki/YUV) Format, dargestellt durch zwei Bildern - Luma und Chroma.

Die YUV-Frames gibt es zwei verschiedene Lösungen.  ein 1280 x 720 Bild, Intensität (im Wesentlichen ein Abbild Graustufen) und viele kleinere 640 x 360 für die Komponente Chrominanz darstellt:

![Bild Demonstrieren der Kombination von Y und UV-Komponenten](urhosharp-images/image3.png)


Um eine vollständige farbige Abbild mithilfe von OpenGL ES zeichnen haben wir einen kleineren Shader zu schreiben, der Luminanz (Y-Komponente)- und (UV Ebenen) von der Textur Slots führt.  In UrhoSharp Namen sind - "sDiffMap" und "sNormalMap" und in RGB-Format zu konvertieren:

```csharp
mat4 ycbcrToRGBTransform = mat4(
    vec4(+1.0000, +1.0000, +1.0000, +0.0000),
    vec4(+0.0000, -0.3441, +1.7720, +0.0000),
    vec4(+1.4020, -0.7141, +0.0000, +0.0000),
    vec4(-0.7010, +0.5291, -0.8860, +1.0000));

vec4 ycbcr = vec4(texture2D(sDiffMap, vTexCoord).r,
                    texture2D(sNormalMap, vTexCoord).ra, 1.0);
gl_FragColor = ycbcrToRGBTransform * ycbcr;
```

Um die Struktur zu rendern, die keine Potenz von zwei Auflösung haben wir Texture2D mit den folgenden Parametern zu definieren:

```chsarp
// texture for UV-plane;
cameraUVtexture = new Texture2D();
cameraUVtexture.SetNumLevels(1);
cameraUVtexture.SetSize(640, 360, Graphics.LuminanceAlphaFormat, TextureUsage.Dynamic);
cameraUVtexture.FilterMode = TextureFilterMode.Bilinear;
cameraUVtexture.SetAddressMode(TextureCoordinate.U, TextureAddressMode.Clamp);
cameraUVtexture.SetAddressMode(TextureCoordinate.V, TextureAddressMode.Clamp);
```

Daher können wir erfasste Abbilder als Hintergrund gerendert und Rendern einer Szene darüber liegenden ähneln furchterregend Mutante.

### <a name="adjusting-the-camera"></a>Anpassen der Kamera

Die `ARFrame` Objekte auch die Position des geschätzten Gerät enthalten.  Wir nun wir verschieben Spiel Kamera ARFrame müssen-vor ARKit kein großer nachverfolgen geräteausrichtung (Rollup, Tonhöhe und gieren) und Rendern einer angeheftete Hologramm zusätzlich das Video - aber wenn Sie Ihr Gerät ein wenig verschieben - Hologramme war wird Abweichung.

Die liegt daran, dass integrierte Sensoren z. B. Gyroskop nicht Bewegungen verfolgen können, sie können nur Acceleration festlegen.  ARKit Analysen jede Frame- und extrahiert-Funktion verweist, um zu verfolgen und kann somit an uns zu einer genauen transformieren Matrix, Verschiebung und Drehung Daten enthält.

Dies ist z. B. wie wir die aktuelle Position abrufen können:

```csharp
var row = arCamera.Transform.Row3;
CameraNode.Position = new Vector3(row.X, row.Y, -row.Z);
```

Wir verwenden `-row.Z` da ARKit ein rechtshändiges Koordinatensystem verwendet.


### <a name="plane-detection"></a>Ebene-Erkennung

ARKit horizontale Ebenen erkennen und auf diese Weise können Sie für die Interaktion mit der realen Welt, wir können z. B. die Mutante auf eine echte Tabelle oder eine Etage platzieren. Die einfachste Möglichkeit hierzu ist die Verwendung von HitTest-Methode (Raycasting). Er konvertiert die Bildschirmkoordinaten (0,5, 0,5 steht im Mittelpunkt) in der realen Welt Koordinaten (0; 0; 0 ist der erste Frame-Speicherort).

```chsarp
protected Vector3? HitTest(float screenX = 0.5f, float screenY = 0.5f)
{
    var result = ARSession.CurrentFrame.HitTest(new CGPoint(screenX, screenY),
        ARHitTestResultType.ExistingPlaneUsingExtent)?.FirstOrDefault();
    if (result != null)
    {
        var row = result.WorldTransform.Row3;
        return new Vector3(row.X, row.Y, -row.Z);
    }
    return null;
}
```

Jetzt können wir die Mutante auf einer horizontalen Oberfläche je nach Speicherort auf dem Gerätebildschirm ablegen, die wir Tippen Sie auf:

```chsarp
void OnTouchEnd(TouchEndEventArgs e)
{
    float x = e.X / (float)Graphics.Width;
    float y = e.Y / (float)Graphics.Height;
    var pos = HitTest(x, y);
    if (pos != null)
    mutantNode.Position = pos.Value;
}
```

![Animierte Abbildung ändern Ebenen als Ansicht wechseln](urhosharp-images/image4.gif)

### <a name="realistic-lighting"></a>Realistische Beleuchtung

Je nachdem, wie die reale Welt Beleuchtung sollte die virtuelle Szene dunkler auf ihre Umgebung besser entsprechen oder heller sein. ARFrame enthält eine LightEstimate-Eigenschaft, die wir verwenden können, passen Sie das Ambiente Urho Licht, dies erfolgt folgendermaßen:


    var ambientIntensity = (float) frame.LightEstimate.AmbientIntensity / 1000f;
    var zone = Scene.GetComponent<Zone>();
    zone.AmbientColor = Color.White * ambientIntensity;


### <a name="beyond-ios---hololens"></a>Nach iOS - HoloLens

UrhoSharp [ausgeführt wird, auf allen wichtigen Betriebssystemen](~/graphics-games/urhosharp/platform/index.md), sodass Sie den vorhandenen Code an anderer Stelle wiederverwenden können.

HoloLens ist eine von der interessantesten Plattformen, mit denen, die Sie ausgeführt wird.   Dies bedeutet, dass Sie einfach zwischen IOS- und HoloLens zum Erstellen von awesome augmentiert, dass Sie tatsächlich Anwendungen mit UrhoSharp wechseln können.

Sie finden die Quelle MutantDemo unter [github.com/EgorBo/ARKitXamarinDemo](https://github.com/EgorBo/ARKitXamarinDemo).


## <a name="related-links"></a>Verwandte Links

- [UrhoSharp](~/graphics-games/urhosharp/index.md)
- [ARKitXamarinDemo (mit UrhoSharp) (Beispiel)](https://github.com/EgorBo/ARKitXamarinDemo)
