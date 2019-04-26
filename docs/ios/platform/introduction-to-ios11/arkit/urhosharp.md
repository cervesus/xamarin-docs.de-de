---
title: Verwenden ARKit mit von UrhoSharp in Xamarin.iOS
description: 'In diesem Dokument wird beschrieben, wie Sie eine app ARKit in Xamarin.iOS einrichten, und klicken Sie dann untersucht, wie Bilder, Gewusst wie: Anpassen die Kamera, gerendert werden beim Erkennen Farbebenen, arbeiten mit Beleuchtung und vieles mehr. Außerdem wird erläutert, von UrhoSharp und Schreiben von Code für HoloLens.'
ms.prod: xamarin
ms.assetid: 877AF974-CC2E-48A2-8E1A-0EF9ABF2C92D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/01/2017
ms.openlocfilehash: b02ecc8a40f6ff8a1862d50202439d369003a53d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61262436"
---
# <a name="using-arkit-with-urhosharp-in-xamarinios"></a>Verwenden ARKit mit von UrhoSharp in Xamarin.iOS

Mit der Einführung von [ARKit](https://developer.apple.com/arkit/), Apple hat es Entwicklern, augmented Reality-Anwendungen erstellen. ARKit verfolgen Sie die genaue Position des Geräts und verschiedene Flächen auf der ganzen Welt erkennen kann, und es ist Aufgabe des Entwicklers, der die Daten stammen aus ARKit in Ihren Code in blend.

[Von UrhoSharp](~/graphics-games/urhosharp/index.md) bietet eine umfassende und benutzerfreundliche 3D-API, die Sie zum Erstellen von 3D-Anwendungen verwenden können.   Beide können werden gemischt zusammen ARKit, die physische Informationen über die Welt bereitzustellen und Urho, um die Ergebnisse zu rendern.

Auf dieser Seite wird erläutert, wie die Verbindung dieser beiden Welten zusammengeführt, um hervorragende augmented Reality-Anwendungen zu erstellen.


## <a name="the-basics"></a>Die Grundlagen

Was wir tun möchten ist vorhanden-3D-Inhalte auf der ganzen Welt, wie das iPhone/iPad zu sehen.   Die Idee besteht darin, den Inhalt von Kamera des Geräts, mit der 3D-Inhalt blend, und wenn der Benutzer des Geräts im Raum bewegt wird, um sicherzustellen, dass das 3D-Objekt Verhalten ist, dass sie Teil dieser Platz: Dies erfolgt durch die Objekte in dieser Welt verankern.

![Animierte Figur in ARKit](urhosharp-images/image1.gif)


Wir verwenden die Bibliothek Urho unsere 3D-Objekte zu laden, und platzieren Sie sie auf der ganzen Welt, und verwenden wir ARKit um den Videostream, die von der Kamera als auch den Speicherort des Telefons in der ganzen Welt erhalten.   Wenn der Benutzer mit seinem Telefon bewegt wird, verwenden wir die Änderungen am Speicherort, um das Koordinatensystem zu aktualisieren, das die Engine Urho angezeigt wird.

Auf diese Weise gibt wieder fügen Sie ein Objekt, in der 3D-Raum und der Benutzer bewegt, der Speicherort des 3D-Objekts den Ort und den Speicherort, in dem sie abgelegt wurde.

## <a name="setting-up-your-application"></a>Einrichten der Anwendung

### <a name="ios-application-launch"></a>iOS-Anwendung zu starten

Muss Ihre iOS-Anwendung erstellen und starten den 3D-Inhalt, dazu erstellen eine eine Unterklasse von implementieren die [ `Urho.Application` ](https://developer.xamarin.com/api/type/Urho.Application/) , und geben Sie Ihren Setupcode durch Überschreiben der `Start` Methode.  Dies ist, in dem Ihre Szene Ruft mit Daten gefüllt, Ereignis-Handler werden usw. einrichten.

Wir haben eingeführt ein `Urho.ArkitApp` Klasse diese Unterklassen `Urho.Application` und klicken Sie auf die `Start` Methode leistet die Schwerarbeit.   Sie müssen lediglich auf Ihre vorhandenen Urho Anwendung ist die Basisklasse ändern Typ `Urho.ArkitApp` und Sie haben eine Anwendung, die Ihre Urho Szene in der ganzen Welt ausgeführt werden.

### <a name="the-arkitapp-class"></a>Die ArkitApp-Klasse

Diese Klasse stellt einen Satz von praktische Standardwerte, die sowohl eine Szene mit einige wichtige Objekte sowie die Verarbeitung von Ereignissen von ARKit, wie sie vom Betriebssystem empfangen werden.

Die Einrichtung erfolgt der `Start` virtuelle Methode.   Wenn Sie diese Methode auf Ihrer Unterklasse überschreiben, müssen Sie sicherstellen, eine Verkettung mit Ihrer übergeordneten mit `base.Start()` auf Ihre eigene Implementierung.

Die `Start` Methode richtet der Szene, Viewport, Kamera und eine gerichtetes Licht, und gibt diese als öffentliche Eigenschaften:

- eine [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Scene/) um Objekte zu speichern.
- ein direktionaler [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light/) mit Schatten und deren Speicherort über verfügbar ist. die `LightNode` Eigenschaft
- eine [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera/) , deren Komponenten werden aktualisiert, wenn ARKit ein Update auf die Anwendung bietet und
- eine [ `ViewPort` ](https://developer.xamarin.com/api/type/Urho.Viewport/) zum Anzeigen der Ergebnisse.


### <a name="your-code"></a>Ihr code

Sie müssen dann um eine Unterklasse der `ArkitApp` Klasse, und überschreiben die `Start` Methode.   Die die Methode sollte als Erstes ist die Kette bis zu der `ArkitApp.Start` durch Aufrufen von `base.Start()`.  Danach können Sie keine der erforderlichen Konfigurationsmaßnahmen Eigenschaften von ArkitApp fügen Sie Ihre Objekte in der Szene, Anpassen der Beleuchtung, Schatten oder Ereignisse, die Sie behandeln möchten.

Das Beispiel ARKit/von UrhoSharp lädt ein animiertes Zeichen mit Texturen und gibt die Animation, durch die folgende Implementierung:

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

Und das ist wirklich alles müssen Sie an dieser Stelle, um dem 3D-Inhalt im augmented Reality-Modus angezeigt.

Urho verwendet benutzerdefinierte Formate für 3D-Modelle und Animationen, daher Sie Ihre Assets in dieses Format zu exportieren müssen.   Sie können Tools wie die [Urho3D Blender-Add-in](https://github.com/reattiva/Urho3D-Blender) und [UrhoAssetImporter](https://github.com/EgorBo/UrhoAssetImporter) , konvertiert werden kann diese Objekte aus gängigen Formaten wie DBX "," DAE "," OBJ "," Blend, maximale-3D in das Format von Urho erforderlich.

Weitere Informationen zum Erstellen von 3D-Anwendungen mit Urho finden Sie auf die [Einführung von UrhoSharp](~/graphics-games/urhosharp/introduction.md) Guide.

## <a name="arkitapp-in-depth"></a>ArkitApp im Detail

> [!NOTE]
> In diesem Abschnitt ist für Entwickler vorgesehen, die die standarderfahrung von UrhoSharp und ARKit anpassen möchten, oder einen tieferen Einblick in die Funktionsweise der Integration abgerufen werden soll.   Es ist nicht notwendig, diesen Abschnitt zu lesen.

Die ARKit-API ist ziemlich einfach, Sie erstellen und Konfigurieren einer [ARSession](https://developer.apple.com/documentation/arkit/arsession) -Objekt, das dann mit der Bereitstellung [ARFrame](https://developer.apple.com/documentation/arkit/arframe) Objekte.   Diese enthalten beide das Bild, die von der Kamera sowie die geschätzte Real-World-Position des Geräts erfasst.

Wir werden die Bilder, die an die Kamera mit uns mit unserer-3D-Inhalte verfassen, und passen Sie die Kamera in von UrhoSharp entsprechend die Wahrscheinlichkeit, dass in der Gerätespeicherort und die Position.

Das folgende Diagramm zeigt, was durchgeführt wird der `ArkitApp` Klasse:

[![Diagramm von Klassen und Bildschirme in der ArkitApp](urhosharp-images/image2.png)](urhosharp-images/image2.png#lightbox)

### <a name="rendering-the-frames"></a>Rendering von Frames

Die Idee ist einfach, kombinieren das Video aus der Kamera mit unserer 3D-Grafiken, um die kombinierten Bild zu erzeugen.     Wir erhalten eine Reihe von diesen erfasste Abbilder nacheinander, und wir werden diese Eingabe mit der Szene Urho mischen.

Die einfachste Möglichkeit dafür ist das Einfügen einer [ `RenderPathCommand` ](https://developer.xamarin.com/api/type/Urho.RenderPathCommand/) im Hauptordner [ `RenderPath` ](https://developer.xamarin.com/api/type/Urho.RenderPath/).  Dies ist eine Reihe von Befehlen, die ausgeführt werden, um ein einzelnes Frame zu zeichnen.  Mit diesem Befehl wird den Viewport mit jede Textur ausgefüllt, die wir an sie übergeben wird.    Wir richten dies auf den ersten Frame, der Prozess ist und der eigentlichen Klassendefinition erfolgt in te **ARRenderPath.xml** -Datei, die zu diesem Zeitpunkt geladen wird.

Wir sind jedoch mit zwei Probleme auf diese beiden Welten ineinander konfrontiert:


1. Unter iOS Texturen von GPU benötigen eine Lösung, die eine Potenz von zwei ist, aber die Frames, die wir von der Kamera erhält keine Lösung, die eine Potenz von zwei, z. B. sind: 1280 x 720.
2. Werden die Frames im codiert [YUV](https://en.wikipedia.org/wiki/YUV) -Format, durch zwei Bilder – Luma und Chroma dargestellt wird.

Die YUV-Frames gibt es in zwei verschiedenen Auflösungen.  ein 1280 x 720-Image, Leuchtdichte (im Grunde ein Abbild Graustufe) und viele kleinere 640 x 360 für die Farbanteiltabellen-Komponente darstellt:

![Images, die veranschaulichen, Kombinieren von Y "und" UV-Komponenten](urhosharp-images/image3.png)


Um ein vollständiges farbigen Image mithilfe von OpenGL ES zeichnen müssen wir einen kleinen Shader schreiben, der aus die Steckplätze Textur Luminanz (Komponente Y)- und (UV-Ebenen) akzeptiert.  In von UrhoSharp Namen sind: "sDiffMap" und "sNormalMap" und in RGB-Format konvertieren:

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

Um die Textur zu rendern, die keine Potenz von zwei Lösung haben wir Texture2D mit den folgenden Parametern zu definieren:

```chsarp
// texture for UV-plane;
cameraUVtexture = new Texture2D();
cameraUVtexture.SetNumLevels(1);
cameraUVtexture.SetSize(640, 360, Graphics.LuminanceAlphaFormat, TextureUsage.Dynamic);
cameraUVtexture.FilterMode = TextureFilterMode.Bilinear;
cameraUVtexture.SetAddressMode(TextureCoordinate.U, TextureAddressMode.Clamp);
cameraUVtexture.SetAddressMode(TextureCoordinate.V, TextureAddressMode.Clamp);
```

Daher können wir zum Rendern der erfassten Bilder als Hintergrund und Rendern einer Szene darüber genauso wie der problematische Mutante.

### <a name="adjusting-the-camera"></a>Anpassen der Kamera

Die `ARFrame` Objekte enthalten auch die geschätzten Gerät Position.  Jetzt müssen wir so verschieben Sie Spiele Kamera ARFrame – bevor ARKit war es keine große Sache nachverfolgen geräteausrichtung (Roll und Pitch Yaw) und Rendern einer angehefteten Hologramm des Videos – aber wenn Sie Ihr Gerät ein wenig verschieben - Hologramme wird abweichen.

In diesem Fall, da integrierte Sensoren wie z. B. Gyroskop nicht Bewegungen nachverfolgen können, kann er nur Beschleunigung.  ARKit Analysen jede Frame und extrahiert die Funktion verweist, die zum Nachverfolgen und kann somit an uns zu einer genauen transformieren Matrix, die Verschiebung und Drehung Daten enthält.

Dies ist z. B. wie wir die aktuelle Position abrufen können:

```csharp
var row = arCamera.Transform.Row3;
CameraNode.Position = new Vector3(row.X, row.Y, -row.Z);
```

Wir verwenden `-row.Z` da ARKit ein rechtshändiges Koordinatensystem verwendet.


### <a name="plane-detection"></a>Datenebene-Erkennung

ARKit horizontale Ebenen zu erkennen und mit dieser Funktion können Sie mit der realen Welt interagieren, z. B. können wir die Mutante in einer tatsächlichen Tabelle oder einem Stockwerk platzieren. Die einfachste Möglichkeit hierzu ist die Verwendung von HitTest-Methode (feinheiten beim Raycasting). Er konvertiert die Bildschirmkoordinaten (0.5, 0.5 liegt im Zentrum) in der realen Welt Koordinaten (0; 0; 0 ist der erste Frame-Speicherort).

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

Jetzt können wir die Mutante auf einer horizontalen Fläche abhängig davon, wo auf dem Bildschirm platzieren, die wir Tippen Sie auf:

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

![Animierte Änderung Abbildung als Ansicht wechselt Farbebenen](urhosharp-images/image4.gif)

### <a name="realistic-lighting"></a>Realistische Beleuchtung

Abhängig von der realen Welt Lichtverhältnissen sollte die virtuelle Szene heller oder dunkler, dessen Umgebung besser zu entsprechen. ARFrame enthält eine LightEstimate-Eigenschaft, die wir verwenden können, passen Sie die Urho Umgebungslicht, dies geschieht wie folgt aus:

```csharp
var ambientIntensity = (float) frame.LightEstimate.AmbientIntensity / 1000f;
var zone = Scene.GetComponent<Zone>();
zone.AmbientColor = Color.White * ambientIntensity;
```

### <a name="beyond-ios---hololens"></a>Nach iOS - HoloLens

Von UrhoSharp [ausgeführt wird, auf allen wichtigen Betriebssystemen](~/graphics-games/urhosharp/platform/index.md), sodass Sie Ihren vorhandenen Code an anderer Stelle wiederverwenden können.

HoloLens ist eine der interessantesten Plattformen, die sie ausgeführt wird.   Dies bedeutet, dass Sie problemlos zwischen iOS und HoloLens awesome Verwenden von UrhoSharp Augmented Reality-Anwendungen erstellen können.

Sie finden die Quelle MutantDemo an [github.com/EgorBo/ARKitXamarinDemo](https://github.com/EgorBo/ARKitXamarinDemo).


## <a name="related-links"></a>Verwandte Links

- [UrhoSharp](~/graphics-games/urhosharp/index.md)
- [ARKitXamarinDemo (mit von UrhoSharp) (Beispiel)](https://github.com/EgorBo/ARKitXamarinDemo)
