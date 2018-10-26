---
title: TextureView
ms.prod: xamarin
ms.assetid: DD1F3D68-5DD8-4644-8A13-08AE7719DE30
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/30/2017
ms.openlocfilehash: f43147d98599d36aa136e4ddc2975205e0e69e26
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122841"
---
# <a name="textureview"></a>TextureView

Die `TextureView` -Klasse ist eine Ansicht, die hardwarebeschleunigtes 2D-Rendering verwendet, um ein Video oder einen OpenGL-Inhaltsdatenstrom anzuzeigende zu ermöglichen. Der folgende Screenshot zeigt beispielsweise die `TextureView` einen livefeed von der Kamera des Geräts in dessen anzeigen:

[![Beispielscreenshot, der ein live-Image aus der Gerätekamera.](texture-view-images/22-textureviewcamera.png)](texture-view-images/22-textureviewcamera.png#lightbox)

Im Gegensatz zu den `SurfaceView` -Klasse, die auch kann zum Anzeigen von OpenGL oder Videoinhalte verwendet werden, die TextureView wird in einem separaten Fenster nicht gerendert.
Aus diesem Grund `TextureView` ist Ansichtstransformationen wie andere Ansichten zu unterstützen. Z. B. drehen eine `TextureView` möglich, indem Sie einfach die `Rotation` -Eigenschaft, die Transparenz durch Festlegen seiner `Alpha` -Eigenschaft, und So weiter.

Daher wird mit der `TextureView` wir können jetzt Dinge wie die Anzeige ein livestreams von der Kamera und transformieren, wie im folgenden Code gezeigt:

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

Der obige Code erstellt eine `TextureView` -Instanz in der Aktivitäts `OnCreate` Methode und legt die Aktivität als die `TextureView`des `SurfaceTextureListener`. Sollen die `SurfaceTextureListener`, implementiert die Aktivität der `TextureView.ISurfaceTextureListener` Schnittstelle. Das System Ruft die `OnSurfaceTextAvailable` Methode bei der `SurfaceTexture` ist zur Verwendung bereit. Nehmen wir in dieser Methode die `SurfaceTexture` , übergeben und legen Sie sie auf der Kamera Vorschau Textur ist. Wir können normale ansichtsbasierte Vorgänge ausführen, wie z. B. Einstellung sind die `Rotation` und `Alpha`, wie im obigen Beispiel. Die Anwendung, auf einem Gerät ausgeführt wird, wird unten gezeigt:

[![Beispiel für die app ausgeführt wird, auf einem Gerät, das Anzeigen eines Bilds](texture-view-images/17-textureviewdemo.png)](texture-view-images/17-textureviewdemo.png#lightbox)

Verwenden der `TextureView`, Hardwarebeschleunigung muss aktiviert sein, wird es standardmäßig ab API-Ebene 14 sein. Auch, da in diesem Beispiel wird die Kamera, verwendet sowohl die `android.permission.CAMERA` Berechtigung und die `android.hardware.camera` Feature muss festgelegt werden, der **"androidmanifest.xml"**.



## <a name="related-links"></a>Verwandte Links

- [TextureViewDemo (Beispiel)](https://developer.xamarin.com/samples/monodroid/TextureViewDemo/)
- [Einführung in die Ice Cream Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0-Plattform](http://developer.android.com/sdk/android-4.0.html)
