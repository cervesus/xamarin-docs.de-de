---
title: Erstellen von Plattformen für Videoplayer
description: In diesem Artikel wird beschrieben, wie Sie mithilfe von RendererXamarin.Forms einen benutzerdefinierten Renderer für Videoplayer auf Plattformen implementieren.
ms.prod: xamarin
ms.assetid: EEE2FB9B-EB73-4A3F-A859-7A1D4808E149
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 007c027772701e424aad5995c0ec025c3589171c
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "76725090"
---
# <a name="creating-the-platform-video-players"></a>Erstellen von Plattformen für Videoplayer

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)

Die [**VideoPlayerDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)-Projektmappe enthält den gesamten Code, der in einen Videoplayer für Xamarin.Forms implementiert werden soll. Außerdem beinhaltet sie mehrere Seiten, auf denen gezeigt wird, wie der Videoplayer innerhalb einer Anwendung zu verwenden ist. Der gesamte `VideoPlayer`-Code und die betreffenden Plattformrenderer sind in einem Projektordner mit dem Namen `FormsVideoLibrary` gespeichert und verwenden den Namespace `FormsVideoLibrary`. Dies vereinfacht das Kopieren der Dateien in Ihre Anwendung sowie das Herstellen von Verweisen auf die Klassen.

## <a name="the-video-player"></a>Der Videoplayer

Die [`VideoPlayer`](https://github.com/xamarin/xamarin-forms-samples/blob/master/CustomRenderers/VideoPlayerDemos/VideoPlayerDemos/VideoPlayerDemos/FormsVideoLibrary/VideoPlayer.cs)-Klasse ist Teil der **VideoPlayerDemos**-.NET Standard-Bibliothek, die sich die Plattformen teilen. Sie wird aus der `View` abgeleitet:

```csharp
using System;
using Xamarin.Forms;

namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
    }
}
```

Die Member dieser Klasse (und der Schnittstelle `IVideoPlayerController`) werden in den folgenden Artikeln beschrieben.

Jede Plattform enthält eine Klasse mit dem Namen `VideoPlayerRenderer`, die den plattformspezifischen Code enthält, der in den Videoplayer implementiert werden soll. Die Hauptaufgabe dieses Renderers ist das Erstellen eines Videoplayers für diese Plattform.

### <a name="the-ios-player-view-controller"></a>Der Ansichtscontroller für den iOS-Player

Es sind mehrere Klassen an der Implementierung eines Videoplayers in iOS beteiligt. Die Anwendung erstellt zunächst einen [`AVPlayerViewController`](xref:AVKit.AVPlayerViewController) und legt dann die [`Player`](xref:AVKit.AVPlayerViewController.Player*)-Eigenschaft auf ein Objekt vom Typ [`AVPlayer`](xref:AVFoundation.AVPlayer) fest. Wenn der Player einer Videoquelle zugewiesen wird, sind weitere Klassen erforderlich.

Wie alle anderen Renderer auch enthält der iOS-[`VideoPlayerRenderer`](https://github.com/xamarin/xamarin-forms-samples/blob/master/CustomRenderers/VideoPlayerDemos/VideoPlayerDemos/VideoPlayerDemos.iOS/FormsVideoLibrary/VideoPlayerRenderer.csVideoPlayerRenderer.cs) ein `ExportRenderer`-Attribut, das die `VideoPlayer`-Ansicht mit dem Renderer in Verbindung bringt:

```csharp
using System;
using System.ComponentModel;
using System.IO;

using AVFoundation;
using AVKit;
using CoreMedia;
using Foundation;
using UIKit;

using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer(typeof(FormsVideoLibrary.VideoPlayer),
                          typeof(FormsVideoLibrary.iOS.VideoPlayerRenderer))]

namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
    }
}
```

In der Regel werden Renderer, die ein Plattformsteuerelement festlegen, von der [`ViewRenderer<View, NativeView>`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs)-Klasse abgeleitet, wobei die `View` eine Ableitung der iOS-`View` (hier `VideoPlayer`) und die `NativeView` eine Ableitung der iOS-`UIView` für die Rendererklasse ist. Für diesen Renderer ist dieses generische Argument nur auf die `UIView` festgelegt. Ihnen sollte bald klar werden, warum dies der Fall ist.

Wenn ein Renderer auf einer Ableitung eines `UIViewController` basiert (wie dies hier der Fall ist), sollte die Klasse die `ViewController`-Eigenschaft überschreiben und den Ansichtscontroller zurückgeben, in diesem Fall `AVPlayerViewController`. Das `_playerViewController`-Feld hat den folgenden Zweck:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        AVPlayer player;
        AVPlayerItem playerItem;
        AVPlayerViewController _playerViewController;       // solely for ViewController property

        public override UIViewController ViewController => _playerViewController;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            base.OnElementChanged(args);

            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    // Create AVPlayerViewController
                    _playerViewController = new AVPlayerViewController();

                    // Set Player property to AVPlayer
                    player = new AVPlayer();
                    _playerViewController.Player = player;

                    // Use the View from the controller as the native control
                    SetNativeControl(_playerViewController.View);
                }
                ···
            }
        }
        ···
    }
}
```

Die Hauptaufgabe der Überschreibung `OnElementChanged` ist es, zu überprüfen, ob die `Control`-Eigenschaft den Wert `null` hat, und, falls dies der Fall ist, ein Plattformsteuerelement zu erstellen und an die `SetNativeControl`-Methode zu übergeben. In diesem Beispiel ist das Objekt nur über die `View`-Eigenschaft des `AVPlayerViewController` verfügbar. Bei dieser Ableitung von `UIView` handelt es sich um eine private Klasse mit dem Namen `AVPlayerView`. Da es sich um eine private Klasse handelt, kann diese allerdings nicht explizit als das zweite generische Argument zu `ViewRenderer` angegeben werden.

In der Regel bezieht sich dann die `Control`-Eigenschaft der Rendererklasse auf die `UIView`, die zum Implementieren des Renderers verwendet wird, aber in diesem Fall wird die `Control`-Eigenschaft nicht an anderer Stelle verwendet.

### <a name="the-android-video-view"></a>Die Videoansicht unter Android

Der Android-Renderer für `VideoPlayer` basiert auf der Android-Klasse [`VideoView`](xref:Android.Widget.VideoView). Wenn allerdings `VideoView` alleine verwendet wird, um ein Video in einer Xamarin.Forms-Anwendung abzuspielen, füllt das Video den Bereich aus, der dem `VideoPlayer` zugewiesen ist und behält dabei nicht das richtige Seitenverhältnis bei. Aus diesem Grund (wie gleich deutlich werden sollte) ist die `VideoView` ein untergeordnetes Element des Android-`RelativeLayout`. Eine `using`-Direktive definiert das `ARelativeLayout`, um dieses von der `RelativeLayout` für Xamarin.Forms zu unterscheiden. Dies stellt das zweite generische Argument im `ViewRenderer` dar:

```csharp
using System;
using System.ComponentModel;
using System.IO;

using Android.Content;
using Android.Media;
using Android.Widget;
using ARelativeLayout = Android.Widget.RelativeLayout;

using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

[assembly: ExportRenderer(typeof(FormsVideoLibrary.VideoPlayer),
                          typeof(FormsVideoLibrary.Droid.VideoPlayerRenderer))]

namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        public VideoPlayerRenderer(Context context) : base(context)
        {
        }
        ···
    }
}
```

Ab Xamarin.Forms 2.5 sollten Android-Renderer einen Konstruktor in ein `Context`-Argument einbinden.

Durch die Überschreibung `OnElementChanged` wird sowohl die `VideoView` als auch das `RelativeLayout` überschrieben und die Layoutparameter für die `VideoView` festgelegt, damit diese im `RelativeLayout` zentriert wird.

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        MediaController mediaController;    // Used to display transport controls
        bool isPrepared;
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            base.OnElementChanged(args);

            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    // Save the VideoView for future reference
                    videoView = new VideoView(Context);

                    // Put the VideoView in a RelativeLayout
                    ARelativeLayout relativeLayout = new ARelativeLayout(Context);
                    relativeLayout.AddView(videoView);

                    // Center the VideoView in the RelativeLayout
                    ARelativeLayout.LayoutParams layoutParams =
                        new ARelativeLayout.LayoutParams(LayoutParams.MatchParent, LayoutParams.MatchParent);
                    layoutParams.AddRule(LayoutRules.CenterInParent);
                    videoView.LayoutParameters = layoutParams;

                    // Handle a VideoView event
                    videoView.Prepared += OnVideoViewPrepared;

                    // Use the RelativeLayout as the native control
                    SetNativeControl(relativeLayout);
                }
                ···
            }
            ···
        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null && videoView != null)
            {
                videoView.Prepared -= OnVideoViewPrepared;
            }
            base.Dispose(disposing);
        }

        void OnVideoViewPrepared(object sender, EventArgs args)
        {
            isPrepared = true;
            ···
        }
        ···
    }
}
```

Ein Handler für das `Prepared`-Ereignis wird im Rahmen dieser Methode angefügt und in der `Dispose`-Methode gelöst. Dieses Ereignis wird ausgelöst, wenn die `VideoView` über genügend Informationen verfügt, um eine Videodatei wiederzugeben.

### <a name="the-uwp-media-element"></a>Das UWP-Medienelement

Für die Universelle Windows-Plattform (UWP) wird am häufigsten [`MediaElement`](xref:Windows.UI.Xaml.Controls.MediaElement) als Videoplayer verwendet. In der Dokumentation zum `MediaElement` wird darauf hingewiesen, dass stattdessen besser [`MediaPlayerElement`](xref:Windows.UI.Xaml.Controls.MediaPlayerElement) verwendet werden soll, wenn nur Windows 10-Versionen ab Build 1607 unterstützt werden müssen.

Die Überschreibung `OnElementChanged` muss ein `MediaElement` erstellen, zwei Ereignishandler festlegen und das `MediaElement`-Objekt an das `SetNativeControl` übergeben:

```csharp
using System;
using System.ComponentModel;

using Windows.Storage;
using Windows.Storage.Streams;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

using Xamarin.Forms;
using Xamarin.Forms.Platform.UWP;

[assembly: ExportRenderer(typeof(FormsVideoLibrary.VideoPlayer),
                          typeof(FormsVideoLibrary.UWP.VideoPlayerRenderer))]

namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            base.OnElementChanged(args);

            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    MediaElement mediaElement = new MediaElement();
                    SetNativeControl(mediaElement);

                    mediaElement.MediaOpened += OnMediaElementMediaOpened;
                    mediaElement.CurrentStateChanged += OnMediaElementCurrentStateChanged;
                }
                ···
            }
            ···
        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null)
            {
                Control.MediaOpened -= OnMediaElementMediaOpened;
                Control.CurrentStateChanged -= OnMediaElementCurrentStateChanged;
            }

            base.Dispose(disposing);
        }        
        ···
    }
}
```

Die beiden Ereignishandler werden im `Dispose`-Ereignis für den Renderer getrennt.

## <a name="showing-the-transport-controls"></a>Anzeigen der Transportsteuerlemente

Alle den Plattformen hinzugefügten Videoplayer unterstützen mehrere Standardsteuerelemente für den Transport, die Schaltflächen zum Wiedergeben und Pausieren sowie eine Leiste umfassen, auf der angezeigt wird, an welcher Stelle des Videos man sich derzeit befindet bzw. über die man an eine beliebigen Stelle wechseln kann.

Die `VideoPlayer`-Klasse definiert eine Eigenschaft mit dem Namen `AreTransportControlsEnabled` und legt den Standardwert auf `true` fest:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // AreTransportControlsEnabled property
        public static readonly BindableProperty AreTransportControlsEnabledProperty =
            BindableProperty.Create(nameof(AreTransportControlsEnabled), typeof(bool), typeof(VideoPlayer), true);

        public bool AreTransportControlsEnabled
        {
            set { SetValue(AreTransportControlsEnabledProperty, value); }
            get { return (bool)GetValue(AreTransportControlsEnabledProperty); }
        }
        ···
    }
}
```

Obwohl diese Eigenschaft sowohl über die `set`- als auch über die `get`-Zugriffsmethode verfügt, muss der Renderer nur Fälle behandeln, wenn die Eigenschaft festgelegt ist. Die Zugriffsmethode `get` gibt nur den aktuellen Wert der Eigenschaft zurück.

Es gibt zwei Methoden, wie Plattformrenderer Eigenschaften wie `AreTransportControlsEnabled` handhaben:

- Im ersten Fall erstellt Xamarin.Forms ein `VideoPlayer`-Element. Dies wird in der Überschreibung `OnElementChanged` des Renderers angezeigt, wenn die `NewElement`-Eigenschaft nicht den Wert `null` hat. Zu diesem Zeitpunkt kann der Renderer über den Anfangswert der in `VideoPlayer` angegebenen Eigenschaft seinen eigenen Videoplayer für die Plattform festlegen.

- Der zweite Fall tritt ein, wenn sich die Eigenschaft in `VideoPlayer` später ändert. Dann wird die `OnElementPropertyChanged`-Methode im Renderer aufgerufen. Dadurch kann der Renderer den Videoplayer für die Plattform basierend auf den neuen Eigenschafteneinstellung aktualisieren.

In den folgenden Abschnitten wird erläutert, wie die `AreTransportControlsEnabled`-Eigenschaft auf den einzelnen Plattformen verarbeitet wird.

### <a name="ios-playback-controls"></a>iOS-Wiedergabesteuerelemente

Die Eigenschaft des iOS-`AVPlayerViewController`, die die Anzeige der Transportsteuerlemente steuert, lautet [`ShowsPlaybackControls`](xref:AVKit.AVPlayerViewController.ShowsPlaybackControls*). Die Eigenschaft wird wie folgt im iOS-`VideoViewRenderer` festgelegt:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        AVPlayerViewController _playerViewController;       // solely for ViewController property

        public override UIViewController ViewController => _playerViewController;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetAreTransportControlsEnabled();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(sender, args);

            if (args.PropertyName == VideoPlayer.AreTransportControlsEnabledProperty.PropertyName)
            {
                SetAreTransportControlsEnabled();
            }
            ···
        }

        void SetAreTransportControlsEnabled()
        {
            ((AVPlayerViewController)ViewController).ShowsPlaybackControls = Element.AreTransportControlsEnabled;
        }
        ···
    }
}
```

Die `Element`-Eigenschaft des Renderers bezieht sich auf die `VideoPlayer`-Klasse.

### <a name="the-android-media-controller"></a>Der Android-Media-Controller

Damit die Transportsteuerelemente unter Android angezeigt werden, muss ein [`MediaController`](xref:Android.Widget.MediaController)-Objekt erstellt und dem `VideoView`-Objekt zugeordnet werden. Die Abläufe werden in der `SetAreTransportControlsEnabled`-Methode dargestellt:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        MediaController mediaController;    // Used to display transport controls
        bool isPrepared;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetAreTransportControlsEnabled();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(sender, args);

            if (args.PropertyName == VideoPlayer.AreTransportControlsEnabledProperty.PropertyName)
            {
                SetAreTransportControlsEnabled();
            }
            ···
        }

        void SetAreTransportControlsEnabled()
        {
            if (Element.AreTransportControlsEnabled)
            {
                mediaController = new MediaController(Context);
                mediaController.SetMediaPlayer(videoView);
                videoView.SetMediaController(mediaController);
            }
            else
            {
                videoView.SetMediaController(null);

                if (mediaController != null)
                {
                    mediaController.SetMediaPlayer(null);
                    mediaController = null;
                }
            }
        }
        ···
    }
}
```

### <a name="the-uwp-transport-controls-property"></a>Die Eigenschaft für Transportsteuerlemente für die UWP

Das UWP-`MediaElement` definiert eine Eigenschaft mit dem Namen [`AreTransportControlsEnabled`](xref:Windows.UI.Xaml.Controls.MediaElement.AreTransportControlsEnabled*), die mithilfe der `VideoPlayer`-Eigenschaft mit demselben Namen festgelegt wird:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetAreTransportControlsEnabled();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(sender, args);

            if (args.PropertyName == VideoPlayer.AreTransportControlsEnabledProperty.PropertyName)
            {
                SetAreTransportControlsEnabled();
            }
            ···
        }

        void SetAreTransportControlsEnabled()
        {
            Control.AreTransportControlsEnabled = Element.AreTransportControlsEnabled;
        }
        ···
    }
}
```

Damit das Video abgespielt werden kann, wird eine weitere Eigenschaft benötigt. Dies ist die wichtige `Source`-Eigenschaft, die auf eine Videodatei verweist. Im nächsten Artikel ([Playing a Web Video (Wiedergeben eines Webvideos)](web-videos.md)) wird beschrieben, wie die `Source`-Eigenschaft implementiert wird.

## <a name="related-links"></a>Verwandte Links

- [Video Player Demos (Videoplayerdemos (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)
