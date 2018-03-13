---
title: TextureView
ms.topic: article
ms.prod: xamarin
ms.assetid: DD1F3D68-5DD8-4644-8A13-08AE7719DE30
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/30/2017
ms.openlocfilehash: d2d9c455f2ddd652a76177527586673901edd012
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="textureview"></a>TextureView

Die `TextureView` Klasse ist eine Sicht, die Hardware accelerated 2D Rendering verwendet, um ein Video oder OpenGL-Datenstrom, der angezeigt wird, werden zu ermöglichen. Das folgende Bildschirmfoto zeigt z. B. die `TextureView` einen live Feed von der Kamera des Geräts anzeigen:

[![Beispiel-Screenshot, der ein live-Abbild auf die Kamera des Geräts](texture-view-images/22-textureviewcamera.png)](texture-view-images/22-textureviewcamera.png#lightbox)

Im Gegensatz zu den `SurfaceView` -Klasse, die auch verwendet werden kann, OpenGL oder Videoinhalt anzuzeigen, die TextureView wird in einem separaten Fenster nicht gerendert.
Aus diesem Grund `TextureView` Ansichtstransformationen wie jede andere Sicht unterstützen kann. Drehen Sie z. B. eine `TextureView` kann erreicht werden, indem Sie einfach festlegen seine `Rotation` -Eigenschaft, der Transparenz durch Festlegen seiner `Alpha` Eigenschaft usw.

Daher wird mit der `TextureView` wir können jetzt Aktionen durchführen können, wie die Anzeige ein livestreams von der Kamera und transformieren, wie im folgenden Code gezeigt:

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

Der obige Code erstellt ein `TextureView` Instanz in der Aktivitätssymbols `OnCreate` Methode und legt die Aktivität als die `TextureView`des `SurfaceTextureListener`. Werden die `SurfaceTextureListener`, die Aktivität implementiert die `TextureView.ISurfaceTextureListener` Schnittstelle. Das System Ruft den `OnSurfaceTextAvailable` Methode bei der `SurfaceTexture` ist verwendungsbereit. Nehmen wir in dieser Methode die `SurfaceTexture` , übergeben und legen Sie sie auf die Kamera Preview Textur ist. Dann kann nach Belieben normalen Ansicht-basierte Vorgänge ausführen, wie z. B. Einstellung werden die `Rotation` und `Alpha`, wie im obigen Beispiel. Die resultierende Anwendung, die auf einem Gerät ausgeführt wird unten gezeigt:

[![Beispiel für die app, die auf einem Gerät, das Anzeigen eines Bilds](texture-view-images/17-textureviewdemo.png)](texture-view-images/17-textureviewdemo.png#lightbox)

Verwenden der `TextureView`, die Hardwarebeschleunigung muss aktiviert sein, wird es standardmäßig ab API-Ebene 14 sein. Auch, da in diesem Beispiel wird die Kamera verwendet sowohl der `android.permission.CAMERA` Berechtigung und die `android.hardware.camera` Funktion muss festgelegt werden, der **AndroidManifest.xml**.



## <a name="related-links"></a>Verwandte Links

- [TextureViewDemo (Beispiel)](https://developer.xamarin.com/samples/monodroid/TextureViewDemo/)
- [Einführung in Eis Rustikal Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 Platform](http://developer.android.com/sdk/android-4.0.html)
