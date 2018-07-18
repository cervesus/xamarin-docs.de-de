---
title: Benutzerdefinierte videotransport-Steuerelemente
description: In diesem Artikel wird erläutert, wie benutzerdefinierte Steuerelemente in einer video-Player-Anwendung mithilfe von Xamarin.Forms implementiert wird.
ms.prod: xamarin
ms.assetid: CE9E955D-A9AC-4019-A5D7-6390D80DECA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 84870de28ffd30b2d29fb5d8fbea815e1fd0d9c4
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996437"
---
# <a name="custom-video-transport-controls"></a>Benutzerdefinierte videotransport-Steuerelemente

Die Transport-Steuerelemente, der ein Videoplayer enthalten, die Schaltflächen, die die Funktionen ausführen **spielen**, **anhalten**, und **beenden**. Diese Schaltflächen werden mit vertrauten Symbole anstelle von Text in der Regel identifiziert und die **spielen** und **anhalten** Funktionen werden in der Regel in eine Schaltfläche kombiniert.

In der Standardeinstellung die `VideoPlayer` zeigt Transportsteuerelemente, die von jeder Plattform unterstützt werden. Beim Festlegen der `AreTransportControlsEnabled` Eigenschaft `false`, diese Steuerelemente werden unterdrückt. Dann können Sie steuern die `VideoPlayer` programmgesteuert oder stellen Sie Ihre eigenen Transport-Steuerelemente.

## <a name="the-play-pause-and-stop-methods"></a>Die Wiedergabe, Anhalten und beenden-Methoden

Die `VideoPlayer` Klasse definiert drei Methoden, die mit dem Namen `Play`, `Pause`, und `Stop` , werden durch das Auslösen von Ereignissen implementiert:

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

Ereignishandler für diese Ereignisse werden festgelegt, indem die `VideoPlayerRenderer` Klasse in jede Plattform, wie unten dargestellt:

### <a name="ios-transport-implementations"></a>iOS-Implementierungen transport

Die iOS-Version von `VideoPlayerRenderer` verwendet die `OnElementChanged` Methode zum Festlegen der Handler für diese drei Ereignisse bei der `NewElement` Eigenschaft ist nicht `null` und trennt die Ereignishandler beim `OldElement` ist nicht `null`:

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

Die Ereignishandler werden durch Aufrufen von Methoden für implementiert die `AVPlayer` Objekt. Es gibt keine `Stop` -Methode für `AVPlayer`, sodass es simuliert wird, durch das Video angehalten, und verschieben die Position, an den Anfang.

### <a name="android-transport-implementations"></a>Android transportimplementierungen

Die Android-Implementierung ist ähnlich wie die iOS-Implementierung. Der Handler für die drei Funktionen werden festgelegt, wenn `NewElement` nicht `null` und wenn trennen `OldElement` nicht `null`:

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

Die drei Funktionen aufrufen definierte Methoden `VideoView`.

### <a name="uwp-transport-implementations"></a>UWP-Transport-Implementierungen

Die UWP-Implementierung der drei Transportfunktionen ähnelt IOS- und Android-Implementierungen:

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

## <a name="the-video-player-status"></a>Der video-Player-status

Implementieren der **spielen**, **anhalten**, und **beenden** Funktionen ist nicht ausreichend für die Transport-Steuerelemente unterstützen. Häufig die **spielen** und **anhalten** Befehle werden implementiert, mit der Schaltfläche mit den gleichen seine Darstellung ändert, um anzugeben, ob das Video derzeit während der Wiedergabe oder angehalten ist. Darüber hinaus darf nicht die Schaltfläche auch aktiviert werden, wenn das Video wurde noch nicht geladen.

Diese Anforderungen impliziert, dass der Videoplayer muss einem aktuellen Status, der angibt, zur Verfügung, wenn sie angehalten oder wird von Spielen oder noch nicht bereit für die Wiedergabe eines Videos ist. (Drei Plattformen unterstützen auch die Eigenschaften, die angibt, ob das Video angehalten werden kann, oder an eine neue Position verschoben werden kann, aber diese Eigenschaften gelten für Streamingvideo anstelle von Videodateien wiedergeben, werden, damit sie nicht in unterstützt werden die `VideoPlayer` hier beschrieben.)

Die **VideoPlayerDemos** -Projekt enthält eine `VideoStatus` Enumeration mit drei Elementen:

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

Die `VideoPlayer` -Klasse definiert eine reine Real bindbare Eigenschaft, die mit dem Namen `Status` des Typs `VideoStatus`. Diese Eigenschaft wird als schreibgeschützt definiert, da sie nur von der Plattform-Renderer festgelegt werden soll:

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

In der Regel würde eine bindbare Eigenschaft schreibgeschützt besitzen einen privaten `set` Accessors für den `Status` Eigenschaft, damit Sie von innerhalb der Klasse festgelegt werden kann. Für eine `View` Ableitung, die von Renderern, jedoch unterstützt die Eigenschaft muss festgelegt werden von außerhalb der Klasse, aber nur von der Plattform-Renderer.

Aus diesem Grund wird eine andere Eigenschaft mit dem Namen definiert `IVideoPlayerController.Status`. Dies ist eine explizite schnittstellenimplementierung und erfolgt durch die `IVideoPlayerController` Schnittstelle, die die `VideoPlayer` -Klasse implementiert:

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

Dies ist ähnlich wie die [ `WebView` ](xref:Xamarin.Forms.WebView) -Steuerelement verwendet die [ `IWebViewController` ](xref:Xamarin.Forms.IWebViewController) Schnittstelle zum Implementieren der `CanGoBack` und `CanGoForward` Eigenschaften. (Finden Sie unter den Quellcode der [ `WebView` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/WebView.cs) und der Renderer für Details.)

Dadurch kann für eine Klasse außerhalb `VideoPlayer` Festlegen der `Status` Eigenschaft durch Verweisen auf die `IVideoPlayerController` Schnittstelle. (Sie müssen den Code in Kürze sehen.) Die Eigenschaft kann von anderen Klassen auch festgelegt werden, aber es ist unwahrscheinlich, dass versehentlich festgelegt werden. Vor allem die `Status` Eigenschaft nicht festgelegt werden, bis eine Datenbindung.

Bei der die Renderer beim Schutz von dieser `Status` -Eigenschaft wurde aktualisiert, die `VideoPlayer` -Klasse definiert ein `UpdateStatus` Ereignis, das ausgelöst jeder Zehntel Sekunde:

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

IOS `VideoPlayerRenderer` legt einen Handler für die `UpdateStatus` Ereignis (und trennt dieser Handler bei den zugrunde liegenden `VideoPlayer` Element ist nicht vorhanden), und den Handler verwendet, um festzulegen der `Status` Eigenschaft:

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

Zwei Eigenschaften des `AVPlayer` zugegriffen werden muss: die [ `Status` ](https://developer.xamarin.com/api/property/AVFoundation.AVPlayer.Status/) Eigenschaft vom Typ `AVPlayerStatus` und [ `TimeControlStatus` ](https://developer.xamarin.com/api/property/AVFoundation.AVPlayer.TimeControlStatus/) Eigenschaft vom Typ `AVPlayerTimeControlStatus`. Beachten Sie, dass die `Element` Eigenschaft (d.h. die `VideoPlayer`) umgewandelt werden muss `IVideoPlayerController` Festlegen der `Status` Eigenschaft.

### <a name="the-android-status-setting"></a>Die Einstellung für Android-status

Die [ `IsPlaying` ](https://developer.xamarin.com/api/property/Android.Widget.VideoView.IsPlaying/) Eigenschaft von der Android `VideoView` ist ein boolescher Wert, der nur gibt an, ob das Video wieder oder wurde angehalten. Bestimmt, ob die `VideoView` weder Play können auch das Video aber, die `Prepared` Ereignis `VideoView` müssen verarbeitet werden. In diesen zwei Handler festgelegt sind das `OnElementChanged` -Methode und getrennte während der die `Dispose` außer Kraft setzen:

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

Die `UpdateStatus` Ereignishandler verwendet die `isPrepared` Feld (Legen Sie in der `Prepared` Handler) und die `IsPlaying` festzulegende Eigenschaft der `Status` Eigenschaft:

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

### <a name="the-uwp-status-setting"></a>Die Einstellung der UWP-status

Die UWP `VideoPlayerRenderer` nutzt die `UpdateStatus` Ereignis, aber es nicht benötigt wird für die Einstellung der `Status` Eigenschaft. Die `MediaElement` definiert eine [ `CurrentStateChanged` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_CurrentStateChanged) Ereignis, mit dem den Renderer bei Zustellung der [ `CurrentState` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_CurrentState) -Eigenschaft geändert hat. Die Eigenschaft wird getrennt, der `Dispose` außer Kraft setzen:

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

Die `CurrentState` Eigenschaft ist vom Typ [ `MediaElementState` ](/uwp/api/windows.ui.xaml.media.mediaelementstate), und ordnet Sie ganz einfach in `VideoStatus`:

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

Mithilfe von Unicode-Zeichen für symbolische **spielen**, **anhalten**, und **beenden** Images ist problematisch. Die [verschiedene technische](https://unicode-table.com/en/blocks/miscellaneous-technical/) Abschnitt der Unicode-Standard definiert drei Symbolzeichen scheinbar für diesen Zweck geeignet. Diese lauten wie folgt:

- 0x23F5 (schwarze mittlere nach rechts zeigendes Dreieck) oder &#x23F5; für **wiedergeben**
- 0x23F8 (doppelter senkrechter Strich) oder &#x23F8; für **anhalten**
- 0x23F9 (schwarzes Quadrat) oder &#x23F9; für **beenden**

Unabhängig davon, ob wie diese Symbole, die in Ihrem Browser angezeigt werden (und die verschiedenen Browser verarbeiten sie unterschiedlich), sie werden nicht angezeigt konsistent auf Xamarin.Forms unterstützten Plattformen. Auf IOS- und UWP-Geräte die **anhalten** und **beenden** Zeichen lang sein, eine grafische Darstellung, mit einem blauen Hintergrund für 3D und einen weißen Vordergrund. Ist dies nicht der Fall unter Android, in dem das Symbol einfach Blau ist. Allerdings die 0x23F5 Codepunkt für **spielen** besitzt keine, dass die gleiche Darstellung auf der UWP und nicht einmal unter iOS und Android unterstützt wird.

Aus diesem Grund nicht für den Codepunkt 0x23F5 verwendet werden **spielen**. Ein guter Ersatz ist:

- 0x25B6 (schwarze nach rechts zeigendes Dreieck) oder &#x25B6; für **wiedergeben**

Dies wird von allen drei Plattformen unterstützt, außer dass es sich um eine einfache schwarze Dreieck handelt, die nicht die 3D-Darstellung der ähnelt **anhalten** und **beenden**. Eine Möglichkeit besteht darin, folgen den 0x25B6 Codepoint mit einem variant-Code:

- 0x25B6 gefolgt von 0xFE0F (16-Variante) oder &#x25B6; &#xFE0F; für **wiedergeben**

Dies ist im nachstehenden Markup verwendet werden. Unter iOS, die sie erhalten die **spielen** symbol 3D genauso dargestellt wie die **anhalten** und **beenden** Schaltflächen, aber die Variante, die unter Android und UWP funktioniert nicht.

Die **Custom-Transporteigenschaften** Seite legt die **AreTransportControlsEnabled** Eigenschaft **"false"** sowie eine `ActivityIndicator` angezeigt, wenn das Video geladen wird, und zwei Schaltflächen. `DataTrigger` Objekte werden zum Aktivieren und Deaktivieren der `ActivityIndicator` und die Schaltflächen und zum Wechseln zwischen der ersten Schaltfläche **spielen** und **anhalten**:

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

Datentrigger werden ausführlich im Artikel beschrieben [Datentrigger](~/xamarin-forms/app-fundamentals/triggers.md#data).

Die Code-Behind-Datei hat die Handler für die Schaltfläche mit den `Clicked` Ereignisse:

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

Da `AutoPlay` nastaven NA hodnotu `false` in die **CustomTransport.xaml** -Datei müssen Sie drücken die **spielen** -Schaltfläche, wenn es aktiviert wird, um das Video zu beginnen. Die Schaltflächen werden definiert, sodass die Unicode-Zeichen, die weiter oben erläuterten durch ihre textentsprechungen begleitet werden. Schaltflächen sind eine konsistente Darstellung auf jeder Plattform, wenn die Wiedergabe:

[![Wiedergabe von benutzerdefinierten Transport](custom-transport-images/customtransportplaying-small.png "benutzerdefinierten Transports wiedergeben")](custom-transport-images/customtransportplaying-large.png#lightbox "benutzerdefinierten Transports wiedergeben")

Aber unter Android und UWP die **spielen** Schaltfläche sieht anders, wenn das Video angehalten wird:

[![Benutzerdefinierter Transport angehalten](custom-transport-images/customtransportpaused-small.png "benutzerdefinierter Transport angehalten")](custom-transport-images/customtransportpaused-large.png#lightbox "benutzerdefinierter Transport angehalten")

In einer produktionsanwendung sollten Sie wahrscheinlich Ihre eigenen Images Bitmap für die Schaltflächen zu verwenden, um visuelle Einheitlichkeit zu erzielen.


## <a name="related-links"></a>Verwandte Links

- [Videodemos Player (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
