---
title: Erstellen der Plattform-video-Player
ms.topic: article
ms.prod: xamarin
ms.assetid: EEE2FB9B-EB73-4A3F-A859-7A1D4808E149
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: d9204593eea97f27b9f461f3f7571c326919f0a9
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="creating-the-platform-video-players"></a>Erstellen der Plattform-video-Player

Die [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) Lösung enthält den gesamten Code zum Implementieren eines Videoplayers für Xamarin.Forms. Darüber hinaus eine Reihe von Seiten, die den Videoplayer in einer Anwendung veranschaulicht. Alle der `VideoPlayer` Code und die Plattform-Renderer befinden sich im Projektordner mit dem Namen `FormsVideoLibrary`, und auch den Namespace verwenden `FormsVideoLibrary`. Dies sollte erleichtern, kopieren Sie die Dateien in Ihrer eigenen Anwendung und die Klassen zu verweisen.

## <a name="the-video-player"></a>Der Videoplayer

Die [ `VideoPlayer` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/CustomRenderers/VideoPlayerDemos/VideoPlayerDemos/VideoPlayerDemos/VideoPlayer.cs) Klasse ist Teil der **VideoPlayerDemos** portable Klassenbibliothek (PCL), die von den Plattformen gemeinsam verwendet wird. Er leitet sich von `View`:

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

Alle drei Plattformen enthält eine Klasse namens `VideoPlayerRenderer` , enthält die plattformspezifischen Code einen Videoplayer implementiert. Die Hauptaufgabe des diesem Renderers ist die Erstellung ein Videoplayers für diese Plattform.

### <a name="the-ios-player-view-controller"></a>IOS Player-View-controller

Mehrere Klassen sind beteiligt, wenn einen video Player in iOS-Implementierung. Die Anwendung zuerst erstellt ein [ `AVPlayerViewController` ](https://developer.xamarin.com/api/type/AVKit.AVPlayerViewController/) und legt dann die [ `Player` ](https://developer.xamarin.com/api/property/AVKit.AVPlayerViewController.Player/) Eigenschaft, um ein Objekt des Typs [ `AVPlayer` ](https://developer.xamarin.com/api/type/AVFoundation.AVPlayer/). Zusätzliche Klassen sind erforderlich, wenn der Spieler eine Videoquelle zugewiesen wird.

Wie allen Renderern, die iOS [ `VideoPlayerRenderer` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/CustomRenderers/VideoPlayerDemos/VideoPlayerDemos/VideoPlayerDemos.iOS/VideoPlayerRenderer.cs) enthält ein `ExportRenderer` -Attribut, identifiziert die `VideoPlayer` Sicht mit den Renderer:

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

In der Regel ein Renderer, der ein Steuerelement Plattform festlegt ableitet, der [ `ViewRenderer<View, NativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs) -Klasse, in dem `View` ist die Xamarin.Forms `View` Ableitung (in diesem Fall `VideoPlayer`) und `NativeView` wird ein iOS `UIView` Ableitung für den Rendererklasse. Für diesen Renderer das generische Argument einfach auf festgelegt ist `UIView`, Gründen wird in Kürze angezeigt.

Wenn ein Renderer auf basiert eine `UIViewController` Ableitung (wie dieser vorhanden ist), und dann die Klasse überschreiben, sollte die `ViewController` Eigenschaft und die Rückgabewerte der modellansichtcontroller in diesem Fall `AVPlayerViewController`. D. h. den Zweck der `_playerViewController` Feld:

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

Die Hauptfunktion des der `OnElementChanged` Außerkraftsetzung wird beim Überprüfen der `Control` Eigenschaft ist `null` und, wenn dies der Fall ist, erstellen Sie ein Steuerelement für die Plattform, und übergeben Sie sie an der `SetNativeControl` Methode. Dieses Objekt ist in diesem Fall nur verfügbar, von der `View` Eigenschaft von der `AVPlayerViewController`. `UIView` Ableitung entspricht eine private Klasse mit dem Namen `AVPlayerView`, aber da es privat ist, es kann nicht explizit angegeben werden als das zweite generischen Argument `ViewRenderer`.

Im Allgemeinen die `Control` Eigenschaft der Rendererklasse danach bezieht sich auf die `UIView` verwendet, um den Renderer, implementieren aber in diesem Fall die `Control` Eigenschaft wird nicht an anderer Stelle verwendet werden.

### <a name="the-android-video-view"></a>Die Android-video-Sicht

Die Android-Renderer für `VideoPlayer` basiert auf dem Android [ `VideoView` ](https://developer.xamarin.com/api/type/Android.Widget.VideoView/) Klasse. Jedoch wenn `VideoView` von sich selbst verwendet, um ein Video in einer Xamarin.Forms-Anwendung, die video füllt den Bereich innerhalb der vorgesehenen für spielen die `VideoPlayer` ohne das richtige Seitenverhältnis beibehalten. Für diesen Grund (wie Sie in Kürze sehen), die `VideoView` erfolgt ein untergeordnetes Element von einer Android `RelativeLayout`. Ein `using` Richtlinie definiert `ARelativeLayout` zur Unterscheidung von den Xamarin.Forms `RelativeLayout`, und das zweite generische Argument in ist die `ViewRenderer`:

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

Xamarin.Forms 2.5 ab, der Android-Renderer sollten umfassen einen Konstruktor mit einem `Context` Argument.

Die `OnElementChanged` Außerkraftsetzung erstellt sowohl die `VideoView` und `RelativeLayout` und legt die layoutparameter für die `VideoView` zur in der Mitte der `RelativeLayout`.


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

Ein Handler für das `Prepared` Ereignis ist in dieser Methode angefügt und getrennt die `Dispose` Methode. Dieses Ereignis wird ausgelöst, wenn die `VideoView` verfügt über ausreichende Informationen zum Wiedergeben einer Videodatei beginnen.

### <a name="the-uwp-media-element"></a>Die uwp-Media-element

In der universelle Windows-Plattform (UWP), die am häufigsten verwendete Videoplayer ist [ `MediaElement` ](/uwp/api/Windows.UI.Xaml.Controls.MediaElement/). Die Dokumentation des `MediaElement` gibt an, dass die [ `MediaPlayerElement` ](/uwp/api/windows.ui.xaml.controls.mediaplayerelement/) sollte stattdessen verwendet werden, wenn es nur zur Unterstützung von Versionen von Windows 10 ab Build 1607 erforderlich ist.

Der `OnElementChanged` Außerkraftsetzung muss zum Erstellen einer `MediaElement`, legen Sie eine Reihe von Ereignishandler aus, und übergeben der `MediaElement` -Objekt `SetNativeControl`:

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

Die beiden Ereignishandler sind getrennt die `Dispose` Ereignis für den Renderer.

## <a name="showing-the-transport-controls"></a>Anzeigen von die Transport-Steuerelementen

Alle drei Plattformen enthalten Videoplayer unterstützt einen Standardsatz von Transport-Steuerelemente, die enthalten Schaltflächen für die Wiedergabe durch das Anhalten und einen Balken an, dass die aktuelle Position innerhalb des Videos und an eine neue Position zu verschieben.

Die `VideoPlayer` Klasse definiert eine Eigenschaft namens `AreTransportControlsEnabled` und legt den Standardwert fest, um `true`:


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

Obwohl diese Eigenschaft sowohl hat `set` und `get` Accessoren Renderer hat, um Fälle zu behandeln, nur, wenn die Eigenschaft festgelegt ist. Die `get` Accessor gibt einfach den aktuellen Wert der Eigenschaft zurück.

Eigenschaften, z. B. `AreTransportControlsEnabled` Plattform Renderern auf zwei Arten erfolgen:

- Das erste Mal ist, wenn Sie Xamarin.Forms erstellt eine `VideoPlayer` Element. Dadurch wird angegeben, der `OnElementChanged` des Renderers überschreiben bei der `NewElement` Eigenschaft ist nicht `null`. Zu diesem Zeitpunkt den Renderer festzulegen kann eigene Plattform Videoplayer ursprünglichen Wert der Eigenschaft ist, gemäß der `VideoPlayer`.

- Wenn die Eigenschaft in `VideoPlayer` später geändert wird, und klicken Sie dann die `OnElementPropertyChanged` im Renderer wird aufgerufen. Dadurch wird den Renderer den Plattform-Videoplayer, basierend auf der neuen Einstellung der Eigenschaft zu aktualisieren.

Hier wird wie die `AreTransportControlsEnabled` Eigenschaft wird in drei Plattformen behandelt:

### <a name="ios-playback-controls"></a>Wiedergabesteuerelemente für iOS

Die Eigenschaft der e/as `AVPlayerViewController` , steuert die Anzeige des Transports ist Steuerelemente [ `ShowsPlaybackControls` ](https://developer.xamarin.com/api/property/AVKit.AVPlayerViewController.ShowsPlaybackControls/). Hier ist die Festlegung dieser Eigenschaft in der iOS- `VideoViewRenderer`:

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

### <a name="the-android-media-controller"></a>Die Android-Media-controller

In der Android-Geräten zum Anzeigen der Steuerelemente benötigt erstellen eine [ `MediaController` ](https://developer.xamarin.com/api/type/Android.Widget.MediaController/) -Objekt und verknüpfen es mit der `VideoView` Objekt. Die Mechanismen demonstriert die `SetAreTransportControlsEnabled` Methode:

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

### <a name="the-uwp-transport-controls-property"></a>Die Eigenschaft für die uwp-Steuerelemente

UWP `MediaElement` definiert eine Eigenschaft mit dem Namen [ `AreTransportControlsEnabled` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_AreTransportControlsEnabled), sodass Eigenschaft, von gesetzt wird der `VideoPlayer` Eigenschaft mit dem gleichen Namen:

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

Eine weitere Eigenschaft ist erforderlich, starten Sie die Wiedergabe eines Videos: Dies ist der entscheidende `Source` Eigenschaft, die eine Videodatei verweist. Implementieren der `Source` Eigenschaft wird in der nächsten Artikel beschrieben [Wiedergabe eines Videos Web](web-videos.md).


## <a name="related-links"></a>Verwandte Links

- [Video Player Demos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
