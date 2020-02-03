---
title: Xamarin. Android textureview
ms.prod: xamarin
ms.assetid: DD1F3D68-5DD8-4644-8A13-08AE7719DE30
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/30/2017
ms.openlocfilehash: 2ffa544789e0d605a241c8e038c790650a7fc6a3
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724976"
---
# <a name="xamarinandroid-textureview"></a>Xamarin. Android textureview

Die `TextureView`-Klasse ist eine Ansicht, die ein Hardware beschleunigtes 2D-Rendering verwendet, um das Anzeigen eines Videos oder eines OpenGL-Inhaltsdaten Stroms zu ermöglichen. Der folgende Screenshot zeigt z. b. den `TextureView`, der einen LiveFeed von der Kamera des Geräts anzeigt:

[Screenshot eines Livebilds von der Kamera des Geräts ![Beispiel](texture-view-images/22-textureviewcamera.png)](texture-view-images/22-textureviewcamera.png#lightbox)

Anders als bei der `SurfaceView`-Klasse, die auch zum Anzeigen von OpenGL-oder Videoinhalten verwendet werden kann, wird textureview nicht in einem separaten Fenster gerendert.
Daher ist `TextureView` in der Lage, Ansichts Transformationen wie jede andere Sicht zu unterstützen. So kann z. b. das Drehen einer `TextureView` durch einfaches Festlegen der `Rotation`-Eigenschaft, der Transparenz durch Festlegen der `Alpha`-Eigenschaft usw. erreicht werden.

Aus diesem Grund können wir mit dem `TextureView` jetzt einen Livestream von der Kamera anzeigen und transformieren, wie im folgenden Code gezeigt:

```csharp
public class TextureViewActivity : Activity,
    TextureView.ISurfaceTextureListener
{
    Camera _camera;
    TextureView _textureView;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        _textureView = new TextureView (this);
        _textureView.SurfaceTextureListener = this;

        SetContentView (_textureView);
    }

    public void OnSurfaceTextureAvailable (
        Android.Graphics.SurfaceTexture surface,
        int width, int height)
    {
        _camera = Camera.Open ();
        var previewSize = _camera.GetParameters ().PreviewSize;
        _textureView.LayoutParameters =
            new FrameLayout.LayoutParams (previewSize.Width,
                previewSize.Height, (int)GravityFlags.Center);

        try {
            _camera.SetPreviewTexture (surface);
            _camera.StartPreview ();
        } catch (Java.IO.IOException ex) {
            Console.WriteLine (ex.Message);
        }

        // this is the sort of thing TextureView enables
        _textureView.Rotation = 45.0f;
        _textureView.Alpha = 0.5f;
    }
    …
}
```

Der obige Code erstellt eine `TextureView` Instanz in der `OnCreate`-Methode der Aktivität und legt die-Aktivität als `SurfaceTextureListener`des `TextureView`fest. Um die `SurfaceTextureListener`zu werden, implementiert die-Aktivität die `TextureView.ISurfaceTextureListener`-Schnittstelle. Das System ruft die `OnSurfaceTextAvailable`-Methode auf, wenn die `SurfaceTexture` bereit für die Verwendung ist. Bei dieser Methode übernehmen wir die `SurfaceTexture`, die übermittelt werden, und legen Sie auf die Vorschau Textur der Kamera fest. Anschließend können Sie normale Ansichts basierte Vorgänge ausführen, wie z. b. das Festlegen der `Rotation` und `Alpha`, wie im obigen Beispiel. Die resultierende Anwendung, die auf einem Gerät ausgeführt wird, wird unten dargestellt:

[![Beispiel für die APP, die auf einem Gerät ausgeführt wird und ein Bild anzeigt](texture-view-images/17-textureviewdemo.png)](texture-view-images/17-textureviewdemo.png#lightbox)

Um die `TextureView`zu verwenden, muss die Hardwarebeschleunigung aktiviert werden. diese wird standardmäßig auf API-Ebene 14 fest. Da in diesem Beispiel die Kamera verwendet wird, müssen sowohl die `android.permission.CAMERA`-Berechtigung als auch die `android.hardware.camera`-Funktion in der Datei " **androidmanifest. XML**" festgelegt werden.

## <a name="related-links"></a>Verwandte Links

- [Textureviewdemo (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/textureviewdemo)/)
