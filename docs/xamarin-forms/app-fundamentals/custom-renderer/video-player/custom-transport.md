---
title: Benutzerdefinierte video Transport-Steuerelemente
ms.prod: xamarin
ms.assetid: CE9E955D-A9AC-4019-A5D7-6390D80DECA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 5463a91dba5840ebe655aa1509d9f98e73643d26
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="custom-video-transport-controls"></a>Benutzerdefinierte video Transport-Steuerelemente

Die Transport-Steuerelemente von einem Videoplayer umfassen die Schaltflächen, die die Funktionen ausführen **wiedergeben**, **anhalten**, und **beenden**. Diese Schaltflächen sind mit vertrauten Symbole anstelle von Text, in der Regel identifiziert und die **wiedergeben** und **anhalten** Funktionen zu einer Schaltfläche in der Regel kombiniert.

Wird standardmäßig die `VideoPlayer` zeigt Transportsteuerelemente von jeder Plattform unterstützt werden. Beim Festlegen der `AreTransportControlsEnabled` Eigenschaft `false`, diese Steuerelemente werden unterdrückt. Dann können Sie steuern die `VideoPlayer` programmgesteuert oder stellen Sie eine eigene Steuerelemente bereit.

## <a name="the-play-pause-and-stop-methods"></a>Die Wiedergabe, Pause und Stop-Methoden

Die `VideoPlayer` Klasse definiert drei Methoden, die mit dem Namen `Play`, `Pause`, und `Stop` bereit, die durch das Auslösen von Ereignissen implementiert:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        public event EventHandler PlayRequested;

        public void Play()
        {
            PlayRequested?.Invoke(this, EventArgs.Empty);
        }

        public event EventHandler PauseRequested;

        public void Pause()
        {
            PauseRequested?.Invoke(this, EventArgs.Empty);
        }

        public event EventHandler StopRequested;

        public void Stop()
        {
            StopRequested?.Invoke(this, EventArgs.Empty);
        }
    }
}
```

Ereignishandler für diese Ereignisse werden festgelegt, indem die `VideoPlayerRenderer` -Klasse in jede Plattform, wie unten dargestellt:

### <a name="ios-transport-implementations"></a>iOS transport Implementierungen

Der iOS-Version `VideoPlayerRenderer` verwendet die `OnElementChanged` -Methode zum Festlegen der Handler für diese drei Ereignisse bei der `NewElement` Eigenschaft ist nicht `null` und trennt die Ereignishandler bei `OldElement` ist nicht `null`:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        AVPlayer player;
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                args.NewElement.PlayRequested += OnPlayRequested;
                args.NewElement.PauseRequested += OnPauseRequested;
                args.NewElement.StopRequested += OnStopRequested;
            }

            if (args.OldElement != null)
            {
                ···
                args.OldElement.PlayRequested -= OnPlayRequested;
                args.OldElement.PauseRequested -= OnPauseRequested;
                args.OldElement.StopRequested -= OnStopRequested;
            }
        }
        ···
        // Event handlers to implement methods
        void OnPlayRequested(object sender, EventArgs args)
        {
            player.Play();
        }

        void OnPauseRequested(object sender, EventArgs args)
        {
            player.Pause();
        }

        void OnStopRequested(object sender, EventArgs args)
        {
            player.Pause();
            player.Seek(new CMTime(0, 1));
        }
    }
}
```

Die Ereignishandler werden durch Aufrufen von Methoden für implementiert die `AVPlayer` Objekt. Es ist keine `Stop` Methode für die `AVPlayer`, sodass es simuliert wird, indem Sie das Video anhalten und verschieben die Position, an den Anfang.

### <a name="android-transport-implementations"></a>Android-Transport-Implementierungen

Die Android-Implementierung ähnelt der iOS-Implementierung. Die Handler für die drei Funktionen werden festgelegt, wenn `NewElement` nicht `null` und wann getrennt `OldElement` nicht `null`:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                args.NewElement.PlayRequested += OnPlayRequested;
                args.NewElement.PauseRequested += OnPauseRequested;
                args.NewElement.StopRequested += OnStopRequested;
            }

            if (args.OldElement != null)
            {
                ···
                args.OldElement.PlayRequested -= OnPlayRequested;
                args.OldElement.PauseRequested -= OnPauseRequested;
                args.OldElement.StopRequested -= OnStopRequested;
            }
        }
        ···
        void OnPlayRequested(object sender, EventArgs args)
        {
            videoView.Start();
        }

        void OnPauseRequested(object sender, EventArgs args)
        {
            videoView.Pause();
        }

        void OnStopRequested(object sender, EventArgs args)
        {
            videoView.StopPlayback();
        }
    }
}
```

Die drei Funktionen aufrufen durch definierte Methoden `VideoView`.

### <a name="uwp-transport-implementations"></a>Uwp-Transport-Implementierungen

Die uwp-Implementierung der drei Transportfunktionen ist sehr ähnlich, IOS- und Android-Implementierungen:

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
                args.NewElement.PlayRequested += OnPlayRequested;
                args.NewElement.PauseRequested += OnPauseRequested;
                args.NewElement.StopRequested += OnStopRequested;
            }

            if (args.OldElement != null)
            {
                ···
                args.OldElement.PlayRequested -= OnPlayRequested;
                args.OldElement.PauseRequested -= OnPauseRequested;
                args.OldElement.StopRequested -= OnStopRequested;
            }
        }
        ···
        // Event handlers to implement methods
        void OnPlayRequested(object sender, EventArgs args)
        {
            Control.Play();
        }

        void OnPauseRequested(object sender, EventArgs args)
        {
            Control.Pause();
        }

        void OnStopRequested(object sender, EventArgs args)
        {
            Control.Stop();
        }
    }
}
```

## <a name="the-video-player-status"></a>Der Videoplayer-status

Implementieren der **wiedergeben**, **anhalten**, und **beenden** Funktionen ist nicht ausreichend für die Transport-Steuerelemente unterstützen. Häufig die **wiedergeben** und **anhalten** Befehle werden implementiert, mit der Schaltfläche "Angleichen", die ändert seine Darstellung, um anzugeben, ob das Video derzeit mit unterschiedlichen "oder" angehalten wird. Darüber hinaus darf nicht die Schaltfläche auch aktiviert werden, wenn das Video noch nicht geladen wurde.

Diese Anforderungen impliziert, dass der Videoplayer muss eine aktuelle Status angezeigt wird, zur Verfügung, wenn sie Wiedergabe oder angehalten oder noch nicht bereit für die Wiedergabe eines Videos ist. (Drei Plattformen unterstützen auch die Eigenschaften, die an, ob das Video angehalten werden kann, oder auf eine neue Position verschoben werden kann, aber diese Eigenschaften gelten für Streamingvideos anstatt Videodateien wiedergeben, werden, damit sie nicht in unterstützt werden die `VideoPlayer` hier beschrieben.)

Die **VideoPlayerDemos** -Projekt enthält eine `VideoStatus` Enumeration mit drei Membern:

```csharp
namespace FormsVideoLibrary
{
    public enum VideoStatus
    {
        NotReady,
        Playing,
        Paused
    }
}
```

Die `VideoPlayer` Klasse definiert eine reine Real bindbare Eigenschaft mit dem Namen `Status` vom Typ `VideoStatus`. Diese Eigenschaft wird als schreibgeschützt definiert, da es nur von der Plattform-Renderer festgelegt werden soll:

```csharp
using System;
using Xamarin.Forms;

namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Status read-only property
        private static readonly BindablePropertyKey StatusPropertyKey =
            BindableProperty.CreateReadOnly(nameof(Status), typeof(VideoStatus), typeof(VideoPlayer), VideoStatus.NotReady);

        public static readonly BindableProperty StatusProperty = StatusPropertyKey.BindableProperty;

        public VideoStatus Status
        {
            get { return (VideoStatus)GetValue(StatusProperty); }
        }

        VideoStatus IVideoPlayerController.Status
        {
            set { SetValue(StatusPropertyKey, value); }
            get { return Status; }
        }
        ···
    }
}
```

In der Regel ist, müsste eine bindbare Eigenschaft schreibgeschützt sind und ein privates `set` Zugriffsmethode auf die `Status` Eigenschaft, damit Sie von innerhalb der Klasse festgelegt werden kann. Für eine `View` Ableitung vom Renderer, jedoch unterstützt die Eigenschaft muss festgelegt werden von außerhalb der Klasse, aber nur von der Plattform-Renderer.

Aus diesem Grund wird eine andere Eigenschaft mit dem Namen definiert `IVideoPlayerController.Status`. Dies ist eine explizite Implementierung und mit einem möglich erfolgt die `IVideoPlayerController` Schnittstelle, die die `VideoPlayer` -Klasse implementiert:

```csharp
namespace FormsVideoLibrary
{
    public interface IVideoPlayerController
    {
        VideoStatus Status { set; get; }

        TimeSpan Duration { set; get; }
    }
}
```

Dies ist ähnlich wie die [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) steuern verwendet der [ `IWebViewController` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IWebViewController/) Schnittstelle zum Implementieren der `CanGoBack` und `CanGoForward` Eigenschaften. (Finden Sie unter den Quellcode der [ `WebView` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/WebView.cs) und seine-Renderer für Details.)

Dies ermöglicht es einer Klasse außerhalb `VideoPlayer` festzulegende der `Status` Eigenschaft durch Verweisen auf die `IVideoPlayerController` Schnittstelle. (Sie müssen den Code in Kürze Siehe.) Die Eigenschaft kann von anderen Klassen als auch festgelegt werden, aber es ist unwahrscheinlich, dass versehentlich festgelegt werden. Allem voran sind die `Status` Eigenschaft kann nicht über eine Bindung festgelegt werden.

Zur Unterstützung der Renderer beim Schutz von dies `Status` -Eigenschaft wurde aktualisiert, die `VideoPlayer` Klasse definiert ein `UpdateStatus` Ereignis, das ausgelöst jeder Zehntelsekunde:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        public event EventHandler UpdateStatus;

        public VideoPlayer()
        {
            Device.StartTimer(TimeSpan.FromMilliseconds(100), () =>
            {
                UpdateStatus?.Invoke(this, EventArgs.Empty);
                return true;
            });
        }
        ···
    }
}
```

### <a name="the-ios-status-setting"></a>Die Einstellung der iOS-status

IOS `VideoPlayerRenderer` legt einen Handler für die `UpdateStatus` Ereignis (und trennt dieser Handler Wenn des zugrunde liegenden `VideoPlayer` Element nicht vorhanden ist), und den Ereignishandler verwendet, um festzulegen der `Status` Eigenschaft:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                args.NewElement.UpdateStatus += OnUpdateStatus;
                ···
            }

            if (args.OldElement != null)
            {
                args.OldElement.UpdateStatus -= OnUpdateStatus;
                ···
            }
        }
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            VideoStatus videoStatus = VideoStatus.NotReady;

            switch (player.Status)
            {
                case AVPlayerStatus.ReadyToPlay:
                    switch (player.TimeControlStatus)
                    {
                        case AVPlayerTimeControlStatus.Playing:
                            videoStatus = VideoStatus.Playing;
                            break;

                        case AVPlayerTimeControlStatus.Paused:
                            videoStatus = VideoStatus.Paused;
                            break;
                    }
                    break;
                }
            }

            ((IVideoPlayerController)Element).Status = videoStatus;
            ···
        }
        ···
    }
}
```

Zwei Eigenschaften des `AVPlayer` zugegriffen werden muss: die [ `Status` ](https://developer.xamarin.com/api/property/AVFoundation.AVPlayer.Status/) Eigenschaft vom Typ `AVPlayerStatus` und [ `TimeControlStatus` ](https://developer.xamarin.com/api/property/AVFoundation.AVPlayer.TimeControlStatus/) Eigenschaft vom Typ `AVPlayerTimeControlStatus`. Beachten Sie, dass die `Element` Eigenschaft (also die `VideoPlayer`) umgewandelt werden muss, um `IVideoPlayerController` festzulegende der `Status` Eigenschaft.

### <a name="the-android-status-setting"></a>Die Einstellung für Android-status

Die [ `IsPlaying` ](https://developer.xamarin.com/api/property/Android.Widget.VideoView.IsPlaying/) Eigenschaft von der Android `VideoView` ist ein boolescher Wert, der nur gibt an, ob das Video mit unterschiedlichen "oder" angehalten. Zum bestimmen, ob die `VideoView` können weder Play noch das Video anhalten noch, die `Prepared` -Ereignis `VideoView` müssen behandelt werden. Diese zwei Handler werden festgelegt, der `OnElementChanged` -Methode, und getrennt, während die `Dispose` außer Kraft setzen:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        ···
        bool isPrepared;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    ···
                    videoView.Prepared += OnVideoViewPrepared;
                    ···
                }
                ···
                args.NewElement.UpdateStatus += OnUpdateStatus;
                ···
            }

            if (args.OldElement != null)
            {
                args.OldElement.UpdateStatus -= OnUpdateStatus;
                ···
            }

        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null && videoView != null)
            {
                videoView.Prepared -= OnVideoViewPrepared;
            }
            if (Element != null)
            {
                Element.UpdateStatus -= OnUpdateStatus;
            }

            base.Dispose(disposing);
        }
        ···
    }
}
```

Die `UpdateStatus` Handler verwendet die `isPrepared` Feld (festgelegt der `Prepared` Handler) und die `IsPlaying` festzulegende Eigenschaft der `Status` Eigenschaft:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        ···
        bool isPrepared;
        ···
        void OnVideoViewPrepared(object sender, EventArgs args)
        {
            isPrepared = true;
            ···
        }
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            VideoStatus status = VideoStatus.NotReady;

            if (isPrepared)
            {
                status = videoView.IsPlaying ? VideoStatus.Playing : VideoStatus.Paused;
            }
            ···
        }
        ···
    }
}
```

### <a name="the-uwp-status-setting"></a>Die uwp-Status-Einstellung

UWP `VideoPlayerRenderer` nutzt die `UpdateStatus` Ereignis jedoch nicht benötigt für die Einstellung der `Status` Eigenschaft. Die `MediaElement` definiert eine [ `CurrentStateChanged` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_CurrentStateChanged) Ereignis, das den Renderer ermöglicht beim benachrichtigt werden, die [ `CurrentState` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_CurrentState) -Eigenschaft geändert hat. Die Eigenschaft wurde getrennt, der `Dispose` außer Kraft setzen:

```csharp
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
                    ···
                    mediaElement.CurrentStateChanged += OnMediaElementCurrentStateChanged;
                };
                ···
            }
            ···
        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null)
            {
                ···
                Control.CurrentStateChanged -= OnMediaElementCurrentStateChanged;
            }

            base.Dispose(disposing);
        }
        ···
    }
}
```

Die `CurrentState` -Eigenschaft ist vom Typ [ `MediaElementState` ](/uwp/api/windows.ui.xaml.media.mediaelementstate), und ordnet Sie einfach im `VideoStatus`:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        void OnMediaElementCurrentStateChanged(object sender, RoutedEventArgs args)
        {
            VideoStatus videoStatus = VideoStatus.NotReady;

            switch (Control.CurrentState)
            {
                case MediaElementState.Playing:
                    videoStatus = VideoStatus.Playing;
                    break;

                case MediaElementState.Paused:
                case MediaElementState.Stopped:
                    videoStatus = VideoStatus.Paused;
                    break;
            }

            ((IVideoPlayerController)Element).Status = videoStatus;
        }
        ···
    }
}
```

## <a name="play-pause-and-stop-buttons"></a>Wiedergabe, Pause und Beenden von Schaltflächen

Verwenden von Unicode-Zeichen für symbolische **wiedergeben**, **anhalten**, und **beenden** Bilder ist problematisch. Die [sonstige technische](https://unicode-table.com/en/blocks/miscellaneous-technical/) Abschnitt im Unicode-Standard definiert drei Symbolzeichen scheinbar für diesen Zweck geeignet. Diese lauten wie folgt:

- 0x23F5 (schwarze mittlere nach rechts zeigendes Dreieck) oder &#x23F5; für **wiedergeben**
- 0x23F8 (doppelter senkrechter Strich) oder &#x23F8; für **anhalten**
- 0x23F9 (schwarze Quadrat) oder &#x23F9; für **beenden**

Unabhängig davon wie diese Symbole in Ihrem Browser angezeigt werden (und verschiedene Browsern unterschiedlich behandeln), sie werden nicht angezeigt konsistent auf Plattformen von Xamarin.Forms unterstützt. Auf IOS- und uwp-Geräten die **anhalten** und **beenden** Zeichen haben eine grafische Darstellung des mit einem blauen Hintergrund 3D und einem weißen Vordergrund. Dies ist die Groß-/Kleinschreibung auf Android-Geräten, auf dem das Symbol einfach Blau ist nicht. Allerdings die 0x23F5 Codepunkt für **wiedergeben** besitzt, dass die gleiche Darstellung auf UWP, und es auch nicht auf IOS- und Android unterstützt wird.

Aus diesem Grund kann die 0x23F5 Codepoint für verwendet werden **wiedergeben**. Ein guter Ersatz ist:

- 0x25B6 (schwarze nach rechts zeigendes Dreieck) oder &#x25B6; für **wiedergeben**

Dies wird von allen drei Plattformen unterstützt, außer dass es sich um eine einfache schwarzen Dreiecks handelt, die keinen die 3D-Darstellung des ähnelt **anhalten** und **beenden**. Eine Möglichkeit besteht darin, den 0x25B6 Codepoint mit einem variant zu befolgen:

- 0x25B6 gefolgt von 0xFE0F (Variant 16) oder &#x25B6; &#xFE0F; für **wiedergeben**

Dies ist im unten gezeigten Markup verwendet. Bei iOS kann dadurch die **wiedergeben** gleichen 3D-Darstellung als symbol der **anhalten** und **beenden** Schaltflächen, aber die Variante funktioniert nicht unter Android und UWP.

Die **Custom-Transporteigenschaften** Seite legt die **AreTransportControlsEnabled** Eigenschaft **"false"** und enthält eine `ActivityIndicator` angezeigt, wenn das Video geladen wird, und zwei Schaltflächen. `DataTrigger` Objekte werden zum Aktivieren und Deaktivieren der `ActivityIndicator` und die Schaltflächen aus, und die erste Schaltfläche zwischen wechseln **wiedergeben** und **anhalten**:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.CustomTransportPage"
             Title="Custom Transport">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0"
                           AutoPlay="False"
                           AreTransportControlsEnabled="False"
                           Source="{StaticResource BigBuckBunny}" />

        <ActivityIndicator Grid.Row="0"
                           Color="Gray"
                           IsVisible="False">
            <ActivityIndicator.Triggers>
                <DataTrigger TargetType="ActivityIndicator"
                             Binding="{Binding Source={x:Reference videoPlayer},
                                               Path=Status}"
                             Value="{x:Static video:VideoStatus.NotReady}">
                    <Setter Property="IsVisible" Value="True" />
                    <Setter Property="IsRunning" Value="True" />
                </DataTrigger>
            </ActivityIndicator.Triggers>
        </ActivityIndicator>

        <StackLayout Grid.Row="1"
                     Orientation="Horizontal"
                     Margin="0, 10"
                     BindingContext="{x:Reference videoPlayer}">

            <Button Text="&#x25B6;&#xFE0F; Play"
                    HorizontalOptions="CenterAndExpand"
                    Clicked="OnPlayPauseButtonClicked">
                <Button.Triggers>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding Status}"
                                 Value="{x:Static video:VideoStatus.Playing}">
                        <Setter Property="Text" Value="&#x23F8; Pause" />
                    </DataTrigger>

                    <DataTrigger TargetType="Button"
                                 Binding="{Binding Status}"
                                 Value="{x:Static video:VideoStatus.NotReady}">
                        <Setter Property="IsEnabled" Value="False" />
                    </DataTrigger>
                </Button.Triggers>
            </Button>

            <Button Text="&#x23F9; Stop"
                    HorizontalOptions="CenterAndExpand"
                    Clicked="OnStopButtonClicked">
                <Button.Triggers>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding Status}"
                                 Value="{x:Static video:VideoStatus.NotReady}">
                        <Setter Property="IsEnabled" Value="False" />
                    </DataTrigger>
                </Button.Triggers>
            </Button>
        </StackLayout>
    </Grid>
</ContentPage>
```

Datentrigger werden ausführlich im Artikel beschrieben [Trigger Data](~/xamarin-forms/app-fundamentals/triggers.md#data).

Die Code-Behind-Datei hat die Handler für die Schaltfläche `Clicked` Ereignisse:

```csharp
namespace VideoPlayerDemos
{
    public partial class CustomTransportPage : ContentPage
    {
        public CustomTransportPage()
        {
            InitializeComponent();
        }

        void OnPlayPauseButtonClicked(object sender, EventArgs args)
        {
            if (videoPlayer.Status == VideoStatus.Playing)
            {
                videoPlayer.Pause();
            }
            else if (videoPlayer.Status == VideoStatus.Paused)
            {
                videoPlayer.Play();
            }
        }

        void OnStopButtonClicked(object sender, EventArgs args)
        {
            videoPlayer.Stop();
        }
    }
}
```

Da `AutoPlay` auf festgelegt ist `false` in der **CustomTransport.xaml** -Datei müssen Sie die drücken die **wiedergeben** -Schaltfläche, wenn es aktiviert wird, um das Video zu starten. Die Schaltflächen werden definiert, sodass ihre textentsprechungen die Unicode-Zeichen, die weiter oben erläuterten begleitet werden. Bei der Wiedergabe des Videos, haben die Schaltflächen eine konsistente Darstellung auf jeder Plattform:

[![Wiedergabe von benutzerdefinierten Transportbindungselementen](custom-transport-images/customtransportplaying-small.png "benutzerdefinierten Transports wiedergeben")](custom-transport-images/customtransportplaying-large.png#lightbox "benutzerdefinierten Transports wiedergeben")

Jedoch unter Android und universelle Windows-Plattform die **wiedergeben** Schaltfläche sieht sehr unterschiedlich, wenn das Video angehalten wird:

[![Benutzerdefinierter Transport angehalten](custom-transport-images/customtransportpaused-small.png "benutzerdefinierter Transport angehalten")](custom-transport-images/customtransportpaused-large.png#lightbox "benutzerdefinierter Transport angehalten")

In einer produktionsanwendung sollten Sie möglicherweise eigene Bitmap-Images für die Schaltflächen zu verwenden, um visuelle Einheitlichkeit zu erzielen.


## <a name="related-links"></a>Verwandte Links

- [Video Player Demos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
