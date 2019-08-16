---
title: Verwenden von Arkit mit urhusharp in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie Sie eine Arkit-app in xamarin. IOS einrichten. Anschließend wird erläutert, wie Frames gerendert werden, wie die Kamera angepasst wird, wie Sie mit der Beleuchtung arbeiten können und vieles mehr. Außerdem werden urhusharp und das Schreiben von Code für hololens erläutert.
ms.prod: xamarin
ms.assetid: 877AF974-CC2E-48A2-8E1A-0EF9ABF2C92D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/01/2017
ms.openlocfilehash: 106d6100d373c8d14a35aaee59035cf4a98083a5
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69528253"
---
# <a name="using-arkit-with-urhosharp-in-xamarinios"></a>Verwenden von Arkit mit urhusharp in xamarin. IOS

Mit der Einführung von [Arkit](https://developer.apple.com/arkit/)hat Apple es Entwicklern ermöglicht, erweiterte Reality-Anwendungen zu erstellen. Arkit kann die genaue Position des Geräts verfolgen und verschiedene Oberflächen auf der ganzen Welt erkennen, und der Entwickler kann dann die Daten aus Arkit in Ihren Code einblenden.

[Urhusharp](~/graphics-games/urhosharp/index.md) stellt eine umfassende und leicht zu verwendende 3D-API bereit, die Sie zum Erstellen von 3D-Anwendungen verwenden können.   Beide können kombiniert werden, Arkit, um die physischen Informationen zur Welt bereitzustellen, und Urho zum Rendering der Ergebnisse.

Auf dieser Seite wird erläutert, wie diese beiden Welten miteinander verbunden werden, um hervorragend erweiterte Reality-Anwendungen zu erstellen.

## <a name="the-basics"></a>Die Grundlagen

Wir möchten 3D-Inhalte auf der ganzen Welt darstellen, wie es auf dem iPhone/iPad zu sehen ist.   Die Idee besteht darin, die Inhalte, die von der Kamera des Geräts stammen, mit dem 3D-Inhalt zu kombinieren, und wenn der Benutzer des Geräts den Raum bewegt, um sicherzustellen, dass sich das 3D-Objekt in diesem Raum verhält, wird dies durch das Verankern der Objekte in dieser Welt erreicht.

![Animierte Abbildung in Arkit](urhosharp-images/image1.gif)

Wir verwenden die Urho-Bibliothek, um die 3D-Assets zu laden und auf der ganzen Welt zu platzieren. Wir verwenden Arkit, um den Videostream von der Kamera und den Ort des Telefons in der Welt zu erhalten.   Wenn sich der Benutzer mit seinem Telefon bewegt, werden die Änderungen am Speicherort verwendet, um das Koordinatensystem zu aktualisieren, das von der Urho-Engine angezeigt wird.

Wenn Sie auf diese Weise ein Objekt in den 3D-Bereich platzieren und der Benutzer sich verschiebt, gibt der Speicherort des 3D-Objekts den Ort und die Position an, an der es platziert wurde.

## <a name="setting-up-your-application"></a>Einrichten der Anwendung

### <a name="ios-application-launch"></a>IOS-Anwendungsstart

Ihre IOS-Anwendung muss ihren 3D-Inhalt erstellen und starten. dazu erstellen Sie einen, der eine Unterklasse von `Urho.Application` implementiert, und geben den Setup Code an, indem Sie die `Start` -Methode überschreiben.  An dieser Stelle wird Ihre Szene mit Daten aufgefüllt, Ereignishandler werden eingerichtet usw.

Wir haben eine `Urho.ArkitApp` -Klasse eingeführt, die `Urho.Application` Unterklassen und `Start` für Ihre-Methode die große Arbeit leistet.   Sie müssen lediglich Ihre vorhandene Urho-Anwendung verwenden, um die Basisklasse in den Typ `Urho.ArkitApp` zu ändern, und Sie haben eine Anwendung, die ihre Urho-Szene auf der ganzen Welt ausführen wird.

### <a name="the-arkitapp-class"></a>Die arkitapp-Klasse

Diese Klasse bietet eine Reihe von praktischen Standardwerten, sowohl eine Szene mit einigen schlüsselobjekten als auch die Verarbeitung von Arkit-Ereignissen, die vom Betriebssystem bereitgestellt werden.

Das Setup erfolgt in der `Start` virtuellen Methode.   Wenn Sie diese Methode für die-Unterklasse überschreiben, müssen Sie sicherstellen, dass Sie mit Ihrem über `base.Start()` geordneten Element verkettet werden, indem Sie Ihre eigene Implementierung verwenden.

Die `Start` -Methode richtet die Szene, den Viewport, die Kamera und eine direktionale Beleuchtung ein und zeigt diese als öffentliche Eigenschaften an:

- ein `Scene` , der die Objekte enthalten soll.
- eine direktionale `Light` mit Shadows und deren Position über die `LightNode` -Eigenschaft verfügbar ist.
- ein `Camera` , dessen Komponenten aktualisiert werden, wenn Arkit ein Update für die Anwendung bereitstellt und
- eine `ViewPort` , die die Ergebnisse anzeigt.

### <a name="your-code"></a>Ihr Code

Anschließend müssen Sie die `ArkitApp` -Klasse Unterklassen und die `Start` -Methode überschreiben.   Die erste Aufgabe, die die Methode ausführen sollte, besteht darin, `ArkitApp.Start` durch Aufrufen `base.Start()`von eine Verkettung durchzuführen.  Anschließend können Sie eine beliebige der Eigenschaften einrichten, die von arkitapp eingerichtet werden, um die Objekte der Szene hinzuzufügen, die Lichter, Schatten oder Ereignisse anzupassen, die Sie behandeln möchten.

Das Arkit/urhusharp-Beispiel lädt ein animiertes Zeichen mit Texturen und spielt die Animation mit der folgenden Implementierung:

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

Und das ist schon alles, was Sie an dieser Stelle tun müssen, damit Ihr 3D-Inhalt in erweitertes Reality angezeigt wird.

Urho verwendet benutzerdefinierte Formate für 3D-Modelle und-Animationen, sodass Sie Ihre Assets in dieses Format exportieren müssen.   Sie können Tools wie das [Urho3D Blender-Add-in](https://github.com/reattiva/Urho3D-Blender) und [urhuassetimporter](https://github.com/EgorBo/UrhoAssetImporter) verwenden, die diese Assets aus gängigen Formaten wie dbx, DAE, obj, Blend, 3D-Max in das für Urho erforderliche Format konvertieren können.

Weitere Informationen zum Erstellen von 3D-Anwendungen mit Urho finden Sie im Leitfaden [Einführung in urhusharp](~/graphics-games/urhosharp/introduction.md) .

## <a name="arkitapp-in-depth"></a>Arkitapp ausführlich

> [!NOTE]
> Dieser Abschnitt richtet sich an Entwickler, die die Standarddarstellung von urhusharp und Arkit anpassen möchten oder einen tieferen Einblick in die Funktionsweise der Integration erhalten möchten.   Es ist nicht erforderlich, diesen Abschnitt zu lesen.

Die Arkit-API ist ziemlich einfach, Sie erstellen und konfigurieren ein [arsession](https://developer.apple.com/documentation/arkit/arsession) -Objekt, das dann mit der Übermittlung von [arframe](https://developer.apple.com/documentation/arkit/arframe) -Objekten beginnt.   Diese enthalten sowohl das von der Kamera erfasste Bild als auch die geschätzte reale Position des Geräts.

Wir werden die Bilder, die von der Kamera geliefert werden, mit unserem 3D-Inhalt zusammenstellen und die Kamera in urhusharp so anpassen, dass Sie den Chancen der Geräte Position und-Position entspricht.

Das folgende Diagramm zeigt, was in der `ArkitApp` -Klasse geschieht:

[![Diagramm der Klassen und Bildschirme in arkitapp](urhosharp-images/image2.png)](urhosharp-images/image2.png#lightbox)

### <a name="rendering-the-frames"></a>Rendern der Frames

Die Idee ist einfach, das Video, das aus der Kamera stammt, mit unseren 3D-Grafiken zu kombinieren, um das kombinierte Bild zu schaffen.     Wir erhalten eine Reihe von aufgezeichneten Images nacheinander, und wir werden diese Eingabe mit der Urho-Szene mischen.

Die einfachste Möglichkeit hierfür ist das Einfügen eines `RenderPathCommand` in den Haupt `RenderPath`-.  Dies ist ein Satz von Befehlen, die zum Zeichnen eines einzelnen Frames ausgeführt werden.  Mit diesem Befehl wird der Viewport mit jeder Textur aufgefüllt, die an ihn übergeben wird.    Dies wird für den ersten Frame festgelegt, der verarbeitet wird, und die tatsächliche Definition erfolgt in der Datei " **arrenderpath. XML** ", die zu diesem Zeitpunkt geladen wird.

Wir haben jedoch zwei Probleme, um diese beiden Welten miteinander zu kombinieren:


1. In ios müssen GPU-Texturen eine Auflösung von zwei Möglichkeiten aufweisen, aber die Rahmen, die wir von der Kamera erhalten, haben keine Auflösung, die eine Potenz von zwei ist, z. b.: 1.280 x 720.
2. Die Frames werden im [YUV](https://en.wikipedia.org/wiki/YUV) -Format codiert und durch zwei Bilder dargestellt: "Luma" und "Chroma".

Die YUV-Frames sind in zwei unterschiedlichen Auflösungen enthalten.  ein 1.280 x 720-Bild, das die Beleuchtung darstellt (im Grunde ein graues Skalierungs Bild) und wesentlich kleiner 640 x 360 für die Chrominanz-Komponente:

![Bild zum Kombinieren von Y-und UV-Komponenten](urhosharp-images/image3.png)


Zum Zeichnen eines vollständig farbigen Bilds mithilfe von OpenGL müssen wir einen kleinen Shader schreiben, der die Leuchtkraft (Y-Komponente) und Chrominanz (UV-Flächen) aus den Textur Slots übernimmt.  In urhusharp haben Sie den Namen "sdiffmap" und "schnormalmap" und konvertieren Sie in das RGB-Format:

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

Um die Textur zu erzeugen, die keine Potenz von zwei Auflösungen hat, müssen wir Texture2D mit den folgenden Parametern definieren:

```chsarp
// texture for UV-plane;
cameraUVtexture = new Texture2D();
cameraUVtexture.SetNumLevels(1);
cameraUVtexture.SetSize(640, 360, Graphics.LuminanceAlphaFormat, TextureUsage.Dynamic);
cameraUVtexture.FilterMode = TextureFilterMode.Bilinear;
cameraUVtexture.SetAddressMode(TextureCoordinate.U, TextureAddressMode.Clamp);
cameraUVtexture.SetAddressMode(TextureCoordinate.V, TextureAddressMode.Clamp);
```

Daher können Sie erfasste Bilder als Hintergrund darstellen und jede Szene über der IT wie die erschreckende Mutant-Darstellung Rendering.

### <a name="adjusting-the-camera"></a>Anpassen der Kamera

Die `ARFrame` Objekte enthalten auch die geschätzte Geräte Position.  Wir müssen jetzt den Spiel Kamera-arframe verschieben. vor dem Arkit war es keine große Sache, die Geräte Ausrichtung (Roll, Pitch und Yaw) zu verfolgen und ein angeheftete – Hologramm auf dem Video zu erzeugen. Wenn Sie Ihr Gerät verschieben, wird jedoch ein Bit-holograms-Wert zurückgesetzt.

Dies liegt daran, dass integrierte Sensoren, wie z. b. Gyroskop, nicht in der Lage sind, Bewegungen zu verfolgen, Sie können nur beschleunigt werden.  Arkit analysiert jeden Frame und extrahiert Merkmals Punkte, die nachverfolgt werden können, und kann somit eine exakte Transformationsmatrix mit Verschiebungs-und Drehungs Daten bereitstellen.

So können wir beispielsweise die aktuelle Position abrufen:

```csharp
var row = arCamera.Transform.Row3;
CameraNode.Position = new Vector3(row.X, row.Y, -row.Z);
```

Wir verwenden `-row.Z` , weil Arkit ein rechts händiges Koordinatensystem verwendet.


### <a name="plane-detection"></a>Ebenenerkennung

Arkit ist in der Lage, horizontale Ebenen zu erkennen, sodass Sie mit der realen Umgebung interagieren können, z. b. können wir die muant in einer realen Tabelle oder in einem Fußboden platzieren. Die einfachste Möglichkeit hierzu ist die Verwendung der HitTest-Methode (Raycasting). Es konvertiert Bildschirm Koordinaten (0,5; 0,5 ist das Zentrum) in die realen Koordinaten (0; 0; 0 ist der Speicherort des ersten Frames).

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

Nun können wir die Mutant auf einer horizontalen Oberfläche platzieren, je nachdem, wo auf dem Geräte Bildschirm wir tippen:

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

![Animierte Abbildung zum Ändern von Ebenen als Ansichts Verschiebungen](urhosharp-images/image4.gif)

### <a name="realistic-lighting"></a>Realistische Beleuchtung

Abhängig von den realen Beleuchtungsbedingungen sollte die virtuelle Szene heller oder dunkler sein, um der Umgebung besser gerecht zu werden. Arframe enthält eine lightestimate-Eigenschaft, mit der wir das Urho Ambient Light anpassen können. Dies geschieht wie folgt:

```csharp
var ambientIntensity = (float) frame.LightEstimate.AmbientIntensity / 1000f;
var zone = Scene.GetComponent<Zone>();
zone.AmbientColor = Color.White * ambientIntensity;
```

### <a name="beyond-ios---hololens"></a>Über IOS-hololens hinaus

Urhusharp wird [unter allen wichtigen Betriebssystemen](~/graphics-games/urhosharp/platform/index.md)ausgeführt, sodass Sie den vorhandenen Code an anderer Stelle wieder verwenden können.

Hololens ist eine der aufregendsten Plattformen, auf denen Sie ausgeführt wird.   Dies bedeutet, dass Sie problemlos zwischen IOS und hololens wechseln können, um mit urhusharp beeindruckende Anwendungen mit erweiterbarer Realität zu erstellen.

Die muantdemo-Quelle finden Sie unter [GitHub.com/EgorBo/ARKitXamarinDemo](https://github.com/EgorBo/ARKitXamarinDemo).


## <a name="related-links"></a>Verwandte Links

- [UrhoSharp](~/graphics-games/urhosharp/index.md)
- [Arkitxamindemo (mit urhusharp) (Beispiel)](https://github.com/EgorBo/ARKitXamarinDemo)
