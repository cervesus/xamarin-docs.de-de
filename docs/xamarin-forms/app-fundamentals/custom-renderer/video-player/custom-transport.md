---
title: Benutzerdefinierte Transportsteuerelemente für Videos
description: In diesem Artikel wird erläutert, wie benutzerdefinierte Transportsteuerelemente mithilfe von Xamarin.Forms in eine Videoplayeranwendung implementiert werden.
ms.prod: xamarin
ms.assetid: CE9E955D-A9AC-4019-A5D7-6390D80DECA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: ab3ed8895a4f7c6b44c978e52e0b00fc32850f75
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68650639"
---
# <a name="custom-video-transport-controls"></a>Benutzerdefinierte Transportsteuerelemente für Videos

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)

Die Transportsteuerelemente eines Videoplayers umfassen die Schaltflächen für Funktionen wie **Wiedergabe**, **Pause** und **Stopp**. In der Regel werden diese Schaltflächen mit bekannten Symbolen anstelle von Text dargestellt, und die Funktionen **Wiedergabe** und **Pause** werden in eine Schaltfläche kombiniert.

`VideoPlayer` zeigt standardmäßig Transportsteuerelemente an, die von jeder Plattform unterstützt werden. Wenn Sie die `AreTransportControlsEnabled`-Eigenschaft auf `false` festlegen, werden diese Steuerelemente unterdrückt. Sie können den `VideoPlayer` dann programmgesteuert steuern oder Ihre eigenen Transportsteuerelemente bereitstellen.

## <a name="the-play-pause-and-stop-methods"></a>Die Methoden „Wiedergabe“, „Pause“ und „Stopp“

Die `VideoPlayer`-Klasse definiert drei Methoden namens `Play`, `Pause` und `Stop`, die durch Auslösen von Ereignissen implementiert werden:

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

Ereignishandler für diese Ereignisse werden wie im Folgenden gezeigt für jede Plattform von der `VideoPlayerRenderer`-Klasse festgelegt:

### <a name="ios-transport-implementations"></a>Transportimplementierungen für iOS

Die iOS-Version von `VideoPlayerRenderer` nutzt die Methode `OnElementChanged`, um Handler für diese drei Ereignisse festzulegen, wenn die `NewElement`-Eigenschaft nicht `null` ist und die Ereignishandler löst, wenn `OldElement` nicht den Wert `null` aufweist:

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

Die Ereignishandler werden implementiert, indem die Methoden für das `AVPlayer`-Objekt aufgerufen werden. Für `AVPlayer` gibt es keine `Stop`-Methode. Deshalb wird sie simuliert, indem das Video pausiert und zum Anfang bewegt wird.

### <a name="android-transport-implementations"></a>Transportimplementierungen für Android

Die Implementierung unter Android ähnelt der iOS-Implementierung. Die Handler für die drei Funktionen werden festgelegt, wenn `NewElement` nicht den Wert `null` aufweist, und gelöst, wenn `OldElement` nicht den Wert `null` aufweist:

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

Die drei Funktionen rufen von `VideoView` definierte Methoden auf.

### <a name="uwp-transport-implementations"></a>Transportimplementierungen für die UWP

Die UWP-Implementierung der drei Transportfunktionen ähnelt den iOS- und Android-Implementierungen stark:

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

## <a name="the-video-player-status"></a>Der Status des Videoplayers

Das Implementieren der Funktionen **Wiedergabe**, **Pause** und **Stopp** genügt nicht, um Transportsteuerelemente zu unterstützen. Die Befehle **Wiedergabe** und **Pause** werden oft mit der gleichen Schaltfläche implementiert, deren Darstellung geändert wird, um anzugeben, dass das Video derzeit abgespielt wird oder pausiert ist. Darüber hinaus sollte die Schaltfläche nicht aktiviert sein, wenn das Video noch nicht geladen wurde.

Diese Anforderungen implizieren, dass der Videoplayer einen aktuellen Status angeben muss, der angibt, ob ein Video wiedergegeben wird, pausiert ist oder noch nicht für die Wiedergabe bereit ist. (Darüber hinaus unterstützt jede Plattform Eigenschaften, die angeben, ob das Video pausiert oder an eine andere Zeitposition bewegt werden kann. Diese Eigenschaften gelten jedoch eher für Videostreams als für Videodateien, weshalb sie für den hier beschriebenen `VideoPlayer` nicht unterstützt werden.)

Das Projekt **VideoPlayerDemos** enthält eine `VideoStatus`-Enumeration mit drei Elementen:

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

Die `VideoPlayer`-Klasse definiert eine schreibgeschützte, bindbare Eigenschaft vom Typ `VideoStatus` namens `Status`. Diese Eigenschaft wird schreibgeschützt definiert, da sie nur über den Plattformrenderer festgelegt werden sollte:

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

Normalerweise würde eine schreibgeschützte, bindbare Eigenschaft über eine private `set`-Zugriffsmethode für die Eigenschaft `Status` verfügen, um das Festlegen innerhalb der Klasse zu ermöglichen. Für eine von Renderern unterstützte `View`-Ableitung muss die Eigenschaft jedoch außerhalb der Klasse festgelegt werden, aber nur vom Plattformrenderer.

Aus diesem Grund wird eine andere Eigenschaft mit den Namen `IVideoPlayerController.Status` definiert. Dies ist eine explizite Schnittstellenimplementierung, die von der `IVideoPlayerController`-Schnittstelle ermöglicht wird, die von der `VideoPlayer`-Klasse implementiert wird:

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

Dies ähnelt der Verwendung der [`IWebViewController`](xref:Xamarin.Forms.IWebViewController)-Schnittstelle des [`WebView`](xref:Xamarin.Forms.WebView)-Steuerelements zum Implementieren der Eigenschaften `CanGoBack` und `CanGoForward`. (Ausführliche Informationen finden Sie im Quellcode von [`WebView`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/WebView.cs) und dessen Renderern.)

Dies ermöglicht Klassen außerhalb von `VideoPlayer`, um die `Status`-Eigenschaft mithilfe von Verweisen auf die `IVideoPlayerController`-Schnittstelle festzulegen. (Der Code wird in Kürze veranschaulicht.) Die Eigenschaft kann auch über andere Klassen festgelegt werden, aber es ist unwahrscheinlich, dass sie versehentlich festgelegt wird. Insbesondere die `Status`-Eigenschaft kann über eine Datenbindung nicht festgelegt werden.

Die `VideoPlayer`-Klasse definiert ein `UpdateStatus`-Ereignis, das nach jeder Zehntelsekunde ausgelöst wird, um die Renderer dabei zu unterstützen, die `Status`-Eigenschaft auf aktuellem Stand zu halten:

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

### <a name="the-ios-status-setting"></a>Die Statuseinstellung unter iOS

Der `VideoPlayerRenderer` von iOS legt einen Handler für das `UpdateStatus`-Ereignis fest (und löst diesen Handler, wenn das zugrundeliegende `VideoPlayer`-Element nicht vorhanden ist) und nutzt den Handler, um die `Status`-Eigenschaft festzulegen:

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

Auf zwei Eigenschaften von `AVPlayer` muss zugegriffen werden: Die [`Status`](xref:AVFoundation.AVPlayer.Status*)-Eigenschaft vom Typ `AVPlayerStatus` und die [`TimeControlStatus`](xref:AVFoundation.AVPlayer.TimeControlStatus*)-Eigenschaft vom Typ `AVPlayerTimeControlStatus`. Beachten Sie, dass die `Element`-Eigenschaft (d.h. der `VideoPlayer`) in `IVideoPlayerController` umgewandelt werden muss, um die `Status`-Eigenschaft festzulegen.

### <a name="the-android-status-setting"></a>Die Statuseinstellung unter Android

Die [`IsPlaying`](xref:Android.Widget.VideoView.IsPlaying)-Eigenschaft des `VideoView` von Android entspricht einem booleschen Wert, der nur angibt, ob das Video abgespielt wird oder pausiert ist. Das `Prepared`-Ereignis von `VideoView` muss verarbeitet werden, um zu ermitteln, ob `VideoView` das Video bereits wiedergeben oder pausieren kann. Diese zwei Handler werden in der `OnElementChanged`-Methode festgelegt und während der `Dispose`-Überschreibung getrennt:

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

Der `UpdateStatus`-Handler verwendet das `isPrepared`-Feld (im `Prepared`-Handler festgelegt) und die `IsPlaying`-Eigenschaft, um die `Status`-Eigenschaft festzulegen:

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

### <a name="the-uwp-status-setting"></a>Die Statuseinstellung für die UWP

Der `VideoPlayerRenderer` der UWP nutzt das `UpdateStatus`-Ereignis, jedoch ist es zum Festlegen der `Status`-Eigenschaft nicht erforderlich. Das `MediaElement` definiert ein [`CurrentStateChanged`](xref:Windows.UI.Xaml.Controls.MediaElement.CurrentStateChanged)-Ereignis, mit dem der Renderer benachrichtigt wird, wenn die [`CurrentState`](xref:Windows.UI.Xaml.Controls.MediaElement.CurrentState*)-Eigenschaft geändert wurde. Die Eigenschaft wird in der `Dispose`-Überschreibung getrennt:

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

Die `CurrentState`-Eigenschaft ist vom Typ [`MediaElementState`](/uwp/api/windows.ui.xaml.media.mediaelementstate) und wird einfach zu `VideoStatus` zugeordnet:

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

## <a name="play-pause-and-stop-buttons"></a>Die Schaltflächen „Wiedergabe“, „Pause“ und „Stopp“

Das Verwenden von Unicode-Zeichen für symbolische Bilder für **Wiedergabe**, **Pause** und **Stopp** ist problematisch. Im Abschnitt [Verschiedene technische Zeichen](https://unicode-table.com/en/blocks/miscellaneous-technical/) des Unicode-Standards werden drei symbolische Zeichen definiert, die für diesen Zweck geeignet sind. Diese lauten wie folgt:

- 0x23F5 (schwarzes mittelgroßes Dreieck, das nach rechts zeigt) oder &#x23F5; für **Wiedergabe**
- 0x23F8 (doppelter senkrechter Strich) oder &#x23F8; für **Pause**
- 0x23F9 (schwarzes Quadrat) oder &#x23F9; für **Stopp**

Unabhängig davon, wie diese Symbole in Ihrem Browser angezeigt werden (verschiedene Browser verarbeiten sie auf unterschiedliche Weise), werden sie auf den von Xamarin.Forms unterstützten Plattformen nicht einheitlich dargestellt. Auf iOS- und UWP-Geräten werden die Zeichen für **Pause** und **Stopp** mit einem dreidimensionalen blauen Hintergrund und einem weißen Vordergrund grafisch dargestellt. Bei Android-Geräten ist dies nicht der Fall, dort ist es einfach blau. Allerdings verfügt der Codepoint „0x23F5“ für **Wiedergabe** nicht über die gleiche Darstellung auf der UWP und wird unter iOS und Android nicht einmal unterstützt.

Aus diesem Grund ist der Codepunkt „0x23F5“ für die **Wiedergabe** ungeeignet. Ein guter Ersatz ist:

- 0x25B6 (schwarzes nach rechts zeigendes Dreieck) oder &#x25B6; für **Wiedergabe**

Dieses wird für jede Plattform mit dem Nachteil unterstützt, dass es sich um ein einfaches schwarzes Dreieck handelt, das nicht der dreidimensionalen Darstellung von **Pause** und **Stopp** entspricht. Es besteht die Möglichkeit, dem Codepunkt „0x25B6“ einen Variantencode anzufügen:

- 0x25B6 gefolgt von 0xFE0F (Variante 16) oder &#x25B6;&#xFE0F; für **Wiedergabe**

Dies wird im unten gezeigten Markup verwendet. Unter iOS erhält das Symbol für **Wiedergabe** die gleiche dreidimensionale Darstellung wie die Schaltflächen **Pause** und **Stopp**, diese Variante funktioniert jedoch nicht unter Android und auf der UWP.

Die Seite **Custom Transport** (Benutzerdefinierte Transportsteuerelemente) legt die Eigenschaft **AreTransportControlsEnabled** auf **FALSE** fest, fügt ein `ActivityIndicator`-Element, was angezeigt wird, wenn das Video lädt, sowie zwei Schaltflächen ein. `DataTrigger`-Objekte werden dazu verwendet, das `ActivityIndicator`-Element und die zwei Schaltflächen zu aktivieren und zu deaktivieren und die erste Schaltfläche zwischen **Wiedergabe** und **Pause** zu wechseln:

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

Ausführliche Informationen über Datentrigger finden Sie im Artikel [Data Triggers (Datentrigger)](~/xamarin-forms/app-fundamentals/triggers.md#data).

Die CodeBehind-Datei verfügt über Handler für die `Clicked`-Schaltflächenereignisse:

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

Da `AutoPlay` in der Datei **CustomTransport.xaml** auf `false` festgelegt ist, müssen Sie auf die Schaltfläche **Wiedergabe** tippen, wenn sie aktiviert ist, um das Video zu starten. Die Schaltflächen sind so definiert, dass die entsprechenden Texte der oben genannten Unicode-Zeichen ebenfalls angezeigt werden. Die Schaltflächen weisen auf allen Plattformen eine einheitliche Darstellung auf, wenn das Video wiedergegeben wird:

[![Benutzerdefinierte Transportsteuerelemente während der Wiedergabe](custom-transport-images/customtransportplaying-small.png "Benutzerdefinierte Transportsteuerelemente während der Wiedergabe")](custom-transport-images/customtransportplaying-large.png#lightbox "Benutzerdefinierte Transportsteuerelemente während der Wiedergabe")

Unter Android und auf der UWP sieht die Schaltfläche **Wiedergabe** jedoch anders aus, wenn das Video pausiert ist:

[![Benutzerdefinierte Transportsteuerelemente während der Pause](custom-transport-images/customtransportpaused-small.png "Benutzerdefinierte Transportsteuerelemente während der Pause")](custom-transport-images/customtransportpaused-large.png#lightbox "Benutzerdefinierte Transportsteuerelemente während der Pause")

In einer Produktionsanwendung sollten Sie Ihre eigenen Bitmap-Bilder für die Schaltflächen benutzen, damit diese visuell einheitlich sind.


## <a name="related-links"></a>Verwandte Links

- [Video Player Demos (Videoplayerdemos (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)
