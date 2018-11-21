---
title: Erstellen die Plattform Videoplayer
description: In diesem Artikel wird erläutert, wie einen benutzerdefinierter Renderer von video-Player auf jeder Plattform mit Xamarin.Forms implementiert werden.
ms.prod: xamarin
ms.assetid: EEE2FB9B-EB73-4A3F-A859-7A1D4808E149
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 0090ec798e8d7b1dfb9bd8e25f09d71ec0353b45
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2018
ms.locfileid: "52171910"
---
# <a name="creating-the-platform-video-players"></a>Erstellen die Plattform Videoplayer

Die [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) Lösung enthält den gesamten Code um ein video Player für Xamarin.Forms zu implementieren. Darüber hinaus eine Reihe von Seiten, die zeigt, wie Sie mit der Videoplayer in einer Anwendung. Alle der `VideoPlayer` Code und die Plattform-Renderer befinden sich im Projektordner mit dem Namen `FormsVideoLibrary`, und auch den Namespace verwenden `FormsVideoLibrary`. Dies sollte es leicht machen, kopieren Sie die Dateien in Ihrer eigenen Anwendung, und verweisen auf die Klassen.

## <a name="the-video-player"></a>Video-player

Die [ `VideoPlayer` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/CustomRenderers/VideoPlayerDemos/VideoPlayerDemos/VideoPlayerDemos/VideoPlayer.cs) Klasse ist Teil der **VideoPlayerDemos** .NET Standard-Bibliothek, die von den Plattformen gemeinsam genutzt wird. Es leitet sich von `View`:

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

Die Member dieser Klasse (und die `IVideoPlayerController` Schnittstelle) werden in den Artikeln, die Folgen beschrieben.

Alle Plattformen enthält eine Klasse namens `VideoPlayerRenderer` , enthält die plattformspezifischen Code einen Videoplayer implementiert. Die primäre Aufgabe der Renderer ist einen Videoplayer für diese Plattform zu erstellen.

### <a name="the-ios-player-view-controller"></a>Der iOS-Player-View-controller

Mehrere Klassen sind beteiligt, bei der einen Videoplayer in iOS-Implementierung. Erstellt die Anwendung zunächst eine [ `AVPlayerViewController` ](https://developer.xamarin.com/api/type/AVKit.AVPlayerViewController/) und legt dann die [ `Player` ](https://developer.xamarin.com/api/property/AVKit.AVPlayerViewController.Player/) Eigenschaft, um ein Objekt des Typs [ `AVPlayer` ](https://developer.xamarin.com/api/type/AVFoundation.AVPlayer/). Zusätzliche Klassen sind erforderlich, wenn der Spieler eine Videoquelle zugewiesen wird.

Wie allen Renderern, die für die iOS [ `VideoPlayerRenderer` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/CustomRenderers/VideoPlayerDemos/VideoPlayerDemos/VideoPlayerDemos.iOS/VideoPlayerRenderer.cs) enthält ein `ExportRenderer` -Attribut, identifiziert die `VideoPlayer` Ansicht mit den Renderer:

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

In der Regel ein Renderer, der ein Steuerelement für die Plattform festlegt abgeleitet der [ `ViewRenderer<View, NativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs) -Klasse, in denen `View` ist die Xamarin.Forms `View` Ableitung (in diesem Fall `VideoPlayer`) und `NativeView` ist ein iOS `UIView` Ableitung für die Rendererklasse. Für diesen Renderer, generische Argument einfach nastaven NA hodnotu `UIView`, Gründen, die Sie werden sehen gleich.

Wenn ein Renderer auf basiert eine `UIViewController` Ableitung (wie dieser ist), und klicken Sie dann die Klasse überschreiben, sollte die `ViewController` Eigenschaft und die Rückgabewerte der View-Controller, in diesem Fall `AVPlayerViewController`. D. h. der Zweck der `_playerViewController` Feld:

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

Die Hauptaufgabe einer der `OnElementChanged` Außerkraftsetzung wird überprüft, ob die `Control` -Eigenschaft ist `null` und, wenn dies der Fall ist, erstellen Sie ein Steuerelement für die Plattform, und übergeben Sie sie an der `SetNativeControl` Methode. In diesem Fall ist das Objekt nur verfügbar, aus der `View` Eigenschaft der `AVPlayerViewController`. Dass `UIView` Ableitung ist eine private Klasse mit dem Namen `AVPlayerView`, aber da es privat ist, es kann nicht explizit angegeben werden als das zweite generische Argument für `ViewRenderer`.

In der Regel die `Control` -Eigenschaft der Rendererklasse danach bezieht sich auf die `UIView` verwendet, um den Renderer, implementieren aber in diesem Fall die `Control` Eigenschaft wird nicht an anderer Stelle verwendet werden.

### <a name="the-android-video-view"></a>Die Android-video-Ansicht

Die Android-Renderer für `VideoPlayer` basiert auf dem Android [ `VideoView` ](https://developer.xamarin.com/api/type/Android.Widget.VideoView/) Klasse. Aber wenn `VideoView` von sich selbst verwendet, um ein Video in einer Xamarin.Forms-Anwendung, das video füllt der zugeteilten Bereich für die Wiedergabe der `VideoPlayer` ohne das richtige Seitenverhältnis beizubehalten. Für diese Grund (wie Sie gleich sehen werden), die `VideoView` erfolgt ein untergeordnetes Element des Android `RelativeLayout`. Ein `using` Richtlinie definiert `ARelativeLayout` zur Unterscheidung von den Xamarin.Forms `RelativeLayout`, und das ist das zweite generische Argument in der `ViewRenderer`:

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

Xamarin.Forms 2.5 ab, der Android Renderer sollte enthalten eines Konstruktors mit einem `Context` Argument.

Die `OnElementChanged` Außerkraftsetzung erstellt sowohl die `VideoView` und `RelativeLayout` und legt die layoutparameter für die `VideoView` zentrieren sie in der `RelativeLayout`.


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

Ein Handler für die `Prepared` Ereignis ist in dieser Methode angefügt und getrennt die `Dispose` Methode. Dieses Ereignis wird ausgelöst, wenn die `VideoView` verfügt über ausreichende Informationen, um eine Videodatei Wiedergabe zu starten.

### <a name="the-uwp-media-element"></a>Das UWP-Media-element

In der universellen Windows-Plattform (UWP), der am häufigsten verwendete Videoplayer ist [ `MediaElement` ](/uwp/api/Windows.UI.Xaml.Controls.MediaElement/). Diese Dokumentation von `MediaElement` gibt an, dass die [ `MediaPlayerElement` ](/uwp/api/windows.ui.xaml.controls.mediaplayerelement/) sollte stattdessen verwendet werden, wenn sie nur zur Unterstützung von Versionen ab Windows 10 Build 1607 erforderlich ist.

Die `OnElementChanged` außer Kraft setzen muss zum Erstellen einer `MediaElement`, legen Sie eine Reihe von Ereignishandlern und übergeben die `MediaElement` -Objekt `SetNativeControl`:

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

Die beiden Ereignishandler getrennt werden, der `Dispose` Ereignis für den Renderer.

## <a name="showing-the-transport-controls"></a>Stellt den Transport-Steuerelemente

Alle Videoplayer, die in der Plattformen enthalten unterstützt einen Standardsatz von Transport-Steuerelemente, die enthalten Schaltflächen für die Wiedergabe anhalten und einen Balken an, dass die aktuelle Position im Video und an eine neue Position zu verschieben.

Die `VideoPlayer` -Klasse definiert eine Eigenschaft namens `AreTransportControlsEnabled` und legt den Standardwert fest, um `true`:


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

Obwohl beide für diese Eigenschaft verfügt über `set` und `get` -Accessor angeben, der Renderer hat, um Fälle zu behandeln, nur, wenn die Eigenschaft festgelegt ist. Die `get` Accessor gibt einfach den aktuellen Wert der Eigenschaft zurück.

Eigenschaften, z. B. `AreTransportControlsEnabled` Plattform Renderern auf zwei Arten erfolgen:

- Das erste Mal ist, wenn Sie Xamarin.Forms erstellt eine `VideoPlayer` Element. Dies wird angezeigt, der `OnElementChanged` des Renderers überschreiben bei der `NewElement` Eigenschaft ist nicht `null`. Zu diesem Zeitpunkt den Renderer festzulegen kann eigenen Plattform-video-Player aus dem ersten Wert der Eigenschaft ist, wie in der `VideoPlayer`.

- Wenn die Eigenschaft im `VideoPlayer` später geändert wird, und klicken Sie dann die `OnElementPropertyChanged` -Methode in der Renderer wird aufgerufen. Dadurch wird den Renderer den Plattform-Videoplayer, basierend auf der neuen Einstellung der Eigenschaft zu aktualisieren.

Den folgenden Abschnitten wird erläutert, wie die `AreTransportControlsEnabled` Eigenschaft wird auf jeder Plattform verarbeitet.

### <a name="ios-playback-controls"></a>iOS-Playback-Steuerelementen

Die Eigenschaft des iOS `AVPlayerViewController` , steuert die Anzeige des Transports Steuerelemente ist [ `ShowsPlaybackControls` ](https://developer.xamarin.com/api/property/AVKit.AVPlayerViewController.ShowsPlaybackControls/). Hier ist, wie diese Eigenschaft festgelegt ist, in der iOS- `VideoViewRenderer`:

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

Die `Element` Eigenschaft des Renderers bezieht sich auf die `VideoPlayer` Klasse.

### <a name="the-android-media-controller"></a>Der Android-Media-controller

In Android zum Anzeigen der Transport-Steuerelemente benötigt erstellen eine [ `MediaController` ](https://developer.xamarin.com/api/type/Android.Widget.MediaController/) Objekt und ordnet ihm dabei die `VideoView` Objekt. Die Mechanismen werden veranschaulicht die `SetAreTransportControlsEnabled` Methode:

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

### <a name="the-uwp-transport-controls-property"></a>Die Eigenschaft für die UWP-Transport-Steuerelemente

Die UWP `MediaElement` definiert eine Eigenschaft namens [ `AreTransportControlsEnabled` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_AreTransportControlsEnabled), sodass die Eigenschaft festgelegt ist, aus der `VideoPlayer` Eigenschaft mit dem gleichen Namen:

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

Eine weitere Eigenschaft ist erforderlich, starten Sie die Wiedergabe eines Videos: Dies ist der entscheidende `Source` Eigenschaft, die eine Videodatei auf. Implementieren der `Source` Eigenschaft wird im nächsten Artikel beschrieben [Wiedergeben eines Webvideos](web-videos.md).


## <a name="related-links"></a>Verwandte Links

- [Videodemos Player (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
