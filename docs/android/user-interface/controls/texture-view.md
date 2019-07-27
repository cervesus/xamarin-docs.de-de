---
title: Xamarin. Android textureview
ms.prod: xamarin
ms.assetid: DD1F3D68-5DD8-4644-8A13-08AE7719DE30
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/30/2017
ms.openlocfilehash: 589cf1787f5dc3090cbfb1165e91d8ef58df37a6
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510162"
---
# <a name="xamarinandroid-textureview"></a>Xamarin. Android textureview

Bei `TextureView` der-Klasse handelt es sich um eine Sicht, die ein Hardware beschleunigtes 2D-Rendering verwendet, um das Anzeigen eines Videos oder eines OpenGL-Inhaltsdaten Stroms Der folgende Screenshot zeigt `TextureView` beispielsweise, wie ein Live Feed von der Kamera des Geräts angezeigt wird:

[![Beispiel Bildschirm Abbildung eines Livebilds von der Kamera des Geräts](texture-view-images/22-textureviewcamera.png)](texture-view-images/22-textureviewcamera.png#lightbox)

Anders als `SurfaceView` bei der-Klasse, die auch zum Anzeigen von OpenGL-oder Videoinhalten verwendet werden kann, wird textureview nicht in einem separaten Fenster gerendert.
Daher ist `TextureView` in der Lage, Ansichts Transformationen wie jede andere Sicht zu unterstützen. Beispielsweise kann das Drehen `TextureView` eines erreicht werden, indem einfach seine `Rotation` -Eigenschaft festgelegt wird, die `Alpha` Transparenz durch Festlegen der-Eigenschaft und so weiter.

Daher können Sie mit `TextureView` dem jetzt einen Livestream von der Kamera anzeigen und transformieren, wie im folgenden Code gezeigt:

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

Der obige Code erstellt eine `TextureView` `TextureView`-Instanz in der- `OnCreate` Methode der-Aktivität und legt die- `SurfaceTextureListener`Aktivität als fest. Um das `SurfaceTextureListener`zu sein, implementiert die- `TextureView.ISurfaceTextureListener` Aktivität die-Schnittstelle. Das System ruft die `OnSurfaceTextAvailable` -Methode auf, wenn die `SurfaceTexture` zur Verwendung bereit ist. Bei dieser Methode nehmen wir die an `SurfaceTexture` , die übermittelt wird, und legen Sie auf die Vorschau Textur der Kamera fest. Anschließend können Sie normale Ansichts basierte Vorgänge ausführen, wie z. b. das `Rotation` festlegen `Alpha`von und, wie im obigen Beispiel. Die resultierende Anwendung, die auf einem Gerät ausgeführt wird, wird unten dargestellt:

[![Beispiel für die APP, die auf einem Gerät ausgeführt wird und ein Bild anzeigt](texture-view-images/17-textureviewdemo.png)](texture-view-images/17-textureviewdemo.png#lightbox)

Um das zu `TextureView`verwenden, muss die Hardwarebeschleunigung aktiviert werden. diese wird standardmäßig auf API-Ebene 14 fest. Da in diesem Beispiel die Kamera verwendet wird, müssen die `android.permission.CAMERA` -Berechtigung und `android.hardware.camera` die-Funktion in der Datei " **androidmanifest. XML**" festgelegt werden.



## <a name="related-links"></a>Verwandte Links

- [Textureviewdemo (Beispiel)](https://developer.xamarin.com/samples/monodroid/TextureViewDemo/)
- [Einführung in Ice Cream Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4,0-Plattform](https://developer.android.com/sdk/android-4.0.html)
