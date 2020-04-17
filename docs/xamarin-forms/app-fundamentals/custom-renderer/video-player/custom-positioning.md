---
title: Benutzerdefinierte Videopositionierung
description: In diesem Artikel wird erläutert, wie eine benutzerdefinierte Fortschrittsleiste mithilfe von Xamarin.Forms in eine Videoplayeranwendung implementiert wird.
ms.prod: xamarin
ms.assetid: 6D792264-30FF-46F7-8C1B-2FEF9D277DF4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 12633b728240c2f90d0265fe7b9efb65ea49bf1f
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "68650642"
---
# <a name="custom-video-positioning"></a>Benutzerdefinierte Videopositionierung

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)

Die von jeder Plattform implementierten Transportsteuerelemente umfassen auch eine Fortschrittsleiste. Diese Leiste ähnelt einem Schieberegler oder einer Scrollleiste und zeigt die aktuelle Zeitposition in der Gesamtdauer des Videos an. Der Benutzer kann die Fortschrittsleiste außerdem ändern, um das Video vor- oder zurückzuspulen.

In diesem Artikel wird veranschaulicht, wie Sie Ihre eigene, benutzerdefinierte Fortschrittsleiste implementieren können.

## <a name="the-duration-property"></a>Die Duration-Eigenschaft

Ein Informationselement, das für den `VideoPlayer` erforderlich ist, um benutzerdefinierte Fortschrittsleisten zu unterstützen, ist die Dauer des Videos. Der `VideoPlayer` definiert eine schreibgeschützte `Duration`-Eigenschaft vom Typ `TimeSpan`:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Duration read-only property
        private static readonly BindablePropertyKey DurationPropertyKey =
            BindableProperty.CreateReadOnly(nameof(Duration), typeof(TimeSpan), typeof(VideoPlayer), new TimeSpan(),
                propertyChanged: (bindable, oldValue, newValue) => ((VideoPlayer)bindable).SetTimeToEnd());

        public static readonly BindableProperty DurationProperty = DurationPropertyKey.BindableProperty;

        public TimeSpan Duration
        {
            get { return (TimeSpan)GetValue(DurationProperty); }
        }

        TimeSpan IVideoPlayerController.Duration
        {
            set { SetValue(DurationPropertyKey, value); }
            get { return Duration; }
        }
        ···
    }
}
```

Diese `Duration` Eigenschaft ist wie die im [vorherigen Artikel](custom-transport.md) beschriebene `Status`-Eigenschaft schreibgeschützt. Sie wird mit einer privaten `BindablePropertyKey`-Klasse definiert und kann nur festgelegt werden, indem auf die `IVideoPlayerController`-Schnittstelle verwiesen wird, die folgende `Duration`-Eigenschaft enthält:

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

Beachten Sie außerdem den durch die geänderte Eigenschaft ausgelösten Handler, der eine Methode namens `SetTimeToEnd` aufruft und weiter unten in diesem Artikel beschrieben wird.

Die Dauer eines Videos ist *nicht* sofort nach der Festlegung der `Source`-Eigenschaft von `VideoPlayer` verfügbar. Die Videodatei muss teilweise heruntergeladen werden, bevor der zugrunde liegende Videoplayer die Dauert ermitteln kann.

Im Folgenden wird erläutert, wie die plattformspezifischen Renderer die Dauer von Videos ermitteln:

### <a name="video-duration-in-ios"></a>Videodauer unter iOS

Unter iOS wird die Dauer von Videos aus der `Duration`-Eigenschaft von `AVPlayerItem` abgerufen. Dies geschieht jedoch nicht unmittelbar nach der Erstellung der `AVPlayerItem`-Klasse. Es ist möglich einen iOS-Beobachter für die `Duration`-Eigenschaft festzulegen, aber der `VideoPlayerRenderer` ruft die Dauer aus der `UpdateStatus`-Methode ab, die zehn mal pro Sekunde aufgerufen wird:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ···
            if (playerItem != null)
            {
                ((IVideoPlayerController)Element).Duration = ConvertTime(playerItem.Duration);
                ···
            }
        }

        TimeSpan ConvertTime(CMTime cmTime)
        {
            return TimeSpan.FromSeconds(Double.IsNaN(cmTime.Seconds) ? 0 : cmTime.Seconds);

        }
        ···
    }
}
```

Die `ConvertTime`-Methode konvertiert ein `CMTime`-Objekt in einen `TimeSpan`-Wert.

### <a name="video-duration-in-android"></a>Videodauer unter Android

Die `Duration`-Eigenschaft der `VideoView`-Klasse von Android meldet eine gültige Dauer in Millisekunden an, wenn das `Prepared`-Ereignis von `VideoView` ausgelöst wird. Die `VideoPlayerRenderer`-Klasse von Android verwendet diesen Handler, um die `Duration`-Eigenschaft abzurufen:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        void OnVideoViewPrepared(object sender, EventArgs args)
        {
            ···
            ((IVideoPlayerController)Element).Duration = TimeSpan.FromMilliseconds(videoView.Duration);
        }
        ···
    }
}
```

### <a name="video-duration-in-uwp"></a>Videodauer auf der UWP

Die `NaturalDuration`-Eigenschaft von `MediaElement` ist ein `TimeSpan`-Wert, der gültig wird, wenn `MediaElement` das `MediaOpened`-Ereignis auslöst:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        void OnMediaElementMediaOpened(object sender, RoutedEventArgs args)
        {
            ((IVideoPlayerController)Element).Duration = Control.NaturalDuration.TimeSpan;
        }
        ···
    }
}
```

## <a name="the-position-property"></a>Die Position-Eigenschaft

Für den `VideoPlayer` ist auch eine `Position`-Eigenschaft erforderlich, die von 0 (null) bis zum Wert der `Duration`-Eigenschaft steigt, während das Video wiedergegeben wird. `VideoPlayer` implementiert diese Eigenschaft wie die `Position`-Eigenschaft der `MediaElement`-Klasse der UWP, die einer normalen bindbaren Eigenschaft mit den öffentlichen Zugriffsmethoden `set` und `get` entspricht:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Position property
        public static readonly BindableProperty PositionProperty =
            BindableProperty.Create(nameof(Position), typeof(TimeSpan), typeof(VideoPlayer), new TimeSpan(),
                propertyChanged: (bindable, oldValue, newValue) => ((VideoPlayer)bindable).SetTimeToEnd());

        public TimeSpan Position
        {
            set { SetValue(PositionProperty, value); }
            get { return (TimeSpan)GetValue(PositionProperty); }
        }
        ···
    }
}
```

Die `get`-Zugriffsmethode gibt die aktuelle Zeitposition des Videos während der Wiedergabe zurück, die `set`-Zugriffsmethode ist jedoch dafür konzipiert, auf den Eingriff auf die Fortschrittsleiste durch den Benutzer zu reagieren, indem die Zeitposition des Videos vorwärts oder rückwärts bewegt wird.

Unter iOS und Android verfügt die Eigenschaft, die die aktuelle Position abruft, lediglich über eine `get`-Zugriffsmethode. Zum Ausführen der zweiten Aufgabe ist eine `Seek`-Methode verfügbar. Bei näherer Untersuchung erweist sich eine separate `Seek`-Methode im Vergleich zu einer einzelnen `Position`-Eigenschaft als bessere Vorgehensweise. Das Verwenden einer einzelnen `Position`-Eigenschaft bringt von Natur aus ein Problem mit sich: Während der Wiedergabe des Videos muss die `Position`-Eigenschaft kontinuierlich aktualisiert werden, um die neue Zeitposition anzugeben. Sie möchten aber nicht, dass die meisten Änderungen an der `Position`-Eigenschaft die Zeitposition des Videos verschieben. Wenn das passiert, reagiert der Videoplayer darauf, indem er nach dem letzten Wert der `Position`-Eigenschaft sucht, und das Video würde anhalten.

Dieser Ansatz wurde trotz der Schwierigkeiten beim Implementieren einer `Position`-Eigenschaft mit den Zugriffsmethoden `set` und `get` ausgewählt, da er mit der `MediaElement`-Klasse der UWP übereinstimmt und einen großen Vorteil bei der Datenbindung mit sich bringt: Die `Position`-Eigenschaft von `VideoPlayer` kann an den Schieberegler gebunden werden, der sowohl zum Anzeigen der Position als auch zum Suchen nach einer neuen Position verwendet wird. Allerdings sind mehrere Vorsichtsmaßnahmen bei der Implementierung dieser `Position`-Eigenschaft erforderlich, um Rückkopplungsschleifen zu vermeiden.

### <a name="setting-and-getting-ios-position"></a>Festlegen und Abrufen der Position unter iOS

Die `CurrentTime`-Eigenschaft des `AVPlayerItem`-Objekts gibt die aktuelle Position des wiedergegebenen Videos unter iOS an. Der `VideoPlayerRenderer` von iOS legt die `Position`-Eigenschaft für den `UpdateStatus`-Handler zur gleichen Zeit fest, zu der die `Duration`-Eigenschaft festgelegt wird:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ···
            if (playerItem != null)
            {
                ···
                ((IElementController)Element).SetValueFromRenderer(VideoPlayer.PositionProperty, ConvertTime(playerItem.CurrentTime));
            }
        }
        ···
    }
}
```

Der Renderer erkennt, wenn der Wert der von `VideoPlayer` festgelegten `Position`-Eigenschaft in der `OnElementPropertyChanged`-Überschreibung geändert wird, und verwendet diesen neuen Wert, um eine `Seek`-Methode für das `AVPlayer`-Objekt aufzurufen:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.PositionProperty.PropertyName)
            {
                TimeSpan controlPosition = ConvertTime(player.CurrentTime);

                if (Math.Abs((controlPosition - Element.Position).TotalSeconds) > 1)
                {
                    player.Seek(CMTime.FromSeconds(Element.Position.TotalSeconds, 1));
                }
            }
        }
        ···
    }
}
```

Beachten Sie, dass jedes Mal, wenn die `Position`-Eigenschaft in `VideoPlayer` über den `OnUpdateStatus`-Handler festgelegt wird, ein `PropertyChanged`-Ereignis von der `Position`-Eigenschaft ausgelöst wird, was in der `OnElementPropertyChanged`-Überschreibung ermittelt wird. Für die meisten dieser Änderungen sollte die `OnElementPropertyChanged`-Methode nichts unternehmen. Andernfalls würde jede Änderung an der Zeitposition des Videos zur eben erreichten Position verschoben werden.

Die `OnElementPropertyChanged`-Methode ruft `Seek` nur auf, wenn der Unterschied zwischen der `Position`-Eigenschaft und der aktuellen Position von `AVPlayer` größer als eine Sekunde ist, um diese Rückkopplungsschleife zu vermeiden.

### <a name="setting-and-getting-android-position"></a>Festlegen und Abrufen der Position unter Android

Genau wie der Renderer von iOS, legt die `VideoPlayerRenderer`-Klasse von Android einen neuen Wert für die `Position`-Eigenschaft im `OnUpdateStatus`-Handler fest. Die `CurrentPosition`-Eigenschaft von `VideoView` enthält die neue Position in Millisekunden:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ···
            TimeSpan timeSpan = TimeSpan.FromMilliseconds(videoView.CurrentPosition);
            ((IElementController)Element).SetValueFromRenderer(VideoPlayer.PositionProperty, timeSpan);
        }
        ···
    }
}
```

Ebenso wie der iOS-Renderer ruft der Android-Renderer die `SeekTo`-Methode von `VideoView` auf, wenn die `Position`-Eigenschaft geändert wird. Dies geschieht aber nur, wenn die Änderung sich um mindestens eine Sekunde vom `CurrentPosition`-Wert von `VideoView` unterscheidet:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.PositionProperty.PropertyName)
            {
                if (Math.Abs(videoView.CurrentPosition - Element.Position.TotalMilliseconds) > 1000)
                {
                    videoView.SeekTo((int)Element.Position.TotalMilliseconds);
                }
            }
        }
        ···
    }
}
```

### <a name="setting-and-getting-uwp-position"></a>Festlegen und Abrufen der Position auf der UWP

Der `VideoPlayerRenderer` der UWP verarbeitet die `Position`-Eigenschaft auf die gleiche Weise wie die iOS- und Android-Renderer, da die `Position`-Eigenschaft der `MediaElement`-Klasse von UWP jedoch auch ein `TimeSpan`-Wert ist, ist keine Konvertierung erforderlich:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.PositionProperty.PropertyName)
            {
                if (Math.Abs((Control.Position - Element.Position).TotalSeconds) > 1)
                {
                    Control.Position = Element.Position;
                }
            }
        }
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ((IElementController)Element).SetValueFromRenderer(VideoPlayer.PositionProperty, Control.Position);
        }
        ···
    }
}
```

## <a name="calculating-a-timetoend-property"></a>Berechnen einer TimeToEnd-Eigenschaft

In einigen Fällen zeigen Videoplayer die verbleibende Zeitspanne des Videos an. Dieser Wert beginnt am Anfang bei der Dauer des Videos und wird bis zum Ende des Videos auf 0 (null) reduziert.

`VideoPlayer` enthält eine schreibgeschützte `TimeToEnd`-Eigenschaft, die anhand der Änderungen an den Eigenschaften `Duration` und `Position` vollständig innerhalb der Klasse verarbeitet wird:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        private static readonly BindablePropertyKey TimeToEndPropertyKey =
            BindableProperty.CreateReadOnly(nameof(TimeToEnd), typeof(TimeSpan), typeof(VideoPlayer), new TimeSpan());

        public static readonly BindableProperty TimeToEndProperty = TimeToEndPropertyKey.BindableProperty;

        public TimeSpan TimeToEnd
        {
            private set { SetValue(TimeToEndPropertyKey, value); }
            get { return (TimeSpan)GetValue(TimeToEndProperty); }
        }

        void SetTimeToEnd()
        {
            TimeToEnd = Duration - Position;
        }
        ···
    }
}
```

Die `SetTimeToEnd`-Methode wird von den durch die geänderte Eigenschaft ausgelösten Handlern der Eigenschaften `Duration` und `Position` aufgerufen.

## <a name="a-custom-slider-for-video"></a>Benutzerdefinierte Schieberegler für Videos

Sie können ein benutzerdefiniertes Steuerelement für eine Fortschrittsleiste schreiben oder die `Slider`-Klasse von Xamarin.Forms bzw. eine von `Slider` abgeleitete Klasse verwenden, z.B. die folgende `PositionSlider`-Klasse. Die Klasse definiert zwei neue Eigenschaften vom Typ `TimeSpan` namens `Duration` und `Position`, die für die Datenbindung an die zwei Eigenschaften mit dem gleichen Namen im `VideoPlayer`-Steuerelement konzipiert sind. Beachten Sie, dass der Standardbindungsmodus der `Position`-Eigenschaft bidirektional ist:

```csharp
namespace FormsVideoLibrary
{
    public class PositionSlider : Slider
    {
        public static readonly BindableProperty DurationProperty =
            BindableProperty.Create(nameof(Duration), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan(1),
                                    propertyChanged: (bindable, oldValue, newValue) =>
                                    {
                                        double seconds = ((TimeSpan)newValue).TotalSeconds;
                                        ((PositionSlider)bindable).Maximum = seconds <= 0 ? 1 : seconds;
                                    });

        public TimeSpan Duration
        {
            set { SetValue(DurationProperty, value); }
            get { return (TimeSpan)GetValue(DurationProperty); }
        }

        public static readonly BindableProperty PositionProperty =
            BindableProperty.Create(nameof(Position), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan(0),
                                    defaultBindingMode: BindingMode.TwoWay,
                                    propertyChanged: (bindable, oldValue, newValue) =>
                                    {
                                        double seconds = ((TimeSpan)newValue).TotalSeconds;
                                        ((PositionSlider)bindable).Value = seconds;
                                    });

        public TimeSpan Position
        {
            set { SetValue(PositionProperty, value); }
            get { return (TimeSpan)GetValue(PositionProperty); }
        }

        public PositionSlider()
        {
            PropertyChanged += (sender, args) =>
            {
                if (args.PropertyName == "Value")
                {
                    TimeSpan newPosition = TimeSpan.FromSeconds(Value);

                    if (Math.Abs(newPosition.TotalSeconds - Position.TotalSeconds) / Duration.TotalSeconds > 0.01)
                    {
                        Position = newPosition;
                    }
                }
            };
        }
    }
}
```

Der durch die geänderte Eigenschaft ausgelöste Handler für die `Duration`-Eigenschaft legt die `Maximum`-Eigenschaft der zugrunde liegenden `Slider`-Klasse auf die `TotalSeconds`-Eigenschaft des Werts `TimeSpan` fest. Auf ähnliche Weise legt der durch die geänderte Eigenschaft ausgelöste Handler für die `Position`-Eigenschaft die `Value`-Eigenschaft der `Slider`-Klasse fest. So kann die zugrunde liegende `Slider`-Klasse die Position der `PositionSlider`-Klasse verfolgen.

Die `PositionSlider`-Klasse wird nur in einem Szenario von der zugrunde liegenden `Slider`-Klasse aktualisiert: Wenn der Benutzer die `Slider`-Klasse bearbeitet, um anzugeben, dass das Video zu einer neuen Position vor- oder zurückgespult werden soll. Dies wird vom `PropertyChanged`-Handler im Konstruktor der `PositionSlider`-Klasse ermittelt. Der Handler prüft nach Änderungen an der `Value`-Eigenschaft. Wenn sie sich von der `Position`-Eigenschaft unterscheidet, wird die `Position`-Eigenschaft über die `Value`-Eigenschaft festgelegt.

Theoretisch könnte die innere `if`-Anweisung wie folgt geschrieben werden:

```csharp
if (newPosition.Seconds != Position.Seconds)
{
    Position = newPosition;
}
```

Allerdings verfügt die Android-Implementierung der `Slider`-Klasse unabhängig von den `Minimum`- und `Maximum`-Einstellungen über nur 1.000 einzelne Schritte. Wenn die Länge eines Videos 1.000 Sekunden überschreitet, entsprechen zwei verschiedene `Position`-Werte der gleichen `Value`-Einstellung der `Slider`-Klasse, und die `if`-Anweisung löst fälschlicherweise eine Reaktion auf eine Änderung der `Slider`-Klasse durch den Benutzer aus. Stattdessen ist es sicherer, zu überprüfen, ob die neue und die vorhandene Position ein Hundertstel der Gesamtdauer überschreiten.

## <a name="using-the-positionslider"></a>Verwenden der PositionSlider-Klasse

Die Dokumentation für die [`MediaElement`](/uwp/api/Windows.UI.Xaml.Controls.MediaElement/)-Klasse der UWP warnt vor Bindungen an die `Position`-Eigenschaft, da die Eigenschaft häufig aktualisiert wird. In der Dokumentation wird empfohlen, einen Timer zu verwenden, um die `Position`-Eigenschaft abzufragen.

Diese Empfehlung ist gut, allerdings verwenden die drei `VideoPlayerRenderer`-Klassen bereits indirekt einen Timer, um die `Position`-Eigenschaft zu aktualisieren. Die `Position`-Eigenschaft wird in einem Handler für das `UpdateStatus`-Ereignis geändert, das zehn mal pro Sekunde ausgelöst wird.

Aus diesem Grund kann die `Position`-Eigenschaft des `VideoPlayer`-Steuerelements wie in der folgenden Seite **Custom Position Bar** gezeigt ohne Leistungseinbußen an die `Position`-Eigenschaft der `PositionSlider`-Klasse gebunden werden:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.CustomPositionBarPage"
             Title="Custom Position Bar">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0"
                           AreTransportControlsEnabled="False"
                           Source="{StaticResource ElephantsDream}" />

        ···

        <StackLayout Grid.Row="1"
                     Orientation="Horizontal"
                     Margin="10, 0"
                     BindingContext="{x:Reference videoPlayer}">

            <Label Text="{Binding Path=Position,
                                  StringFormat='{0:hh\\:mm\\:ss}'}"
                   VerticalOptions="Center"/>

            ···

            <Label Text="{Binding Path=TimeToEnd,
                                  StringFormat='{0:hh\\:mm\\:ss}'}"
                   VerticalOptions="Center" />
        </StackLayout>

        <video:PositionSlider Grid.Row="2"
                              Margin="10, 0, 10, 10"
                              BindingContext="{x:Reference videoPlayer}"
                              Duration="{Binding Duration}"           
                              Position="{Binding Position}">
            <video:PositionSlider.Triggers>
                <DataTrigger TargetType="video:PositionSlider"
                             Binding="{Binding Status}"
                             Value="{x:Static video:VideoStatus.NotReady}">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </video:PositionSlider.Triggers>
        </video:PositionSlider>
    </Grid>
</ContentPage>
```

Mit den ersten Auslassungspunkten (···) wird das `ActivityIndicator`-Element ausgeblendet, da es der vorherigen Seite **Benutzerdefinierte Transportsteuerelemente** entspricht. Beachten Sie, dass die `Label`-Elemente die Eigenschaften `Position` und `TimeToEnd` anzeigen. Die Auslassungspunkte zwischen den beiden `Label`-Elementen blenden die zwei `Button`-Elemente aus, die bereits auf der Seite **Benutzerdefinierte Transportsteuerelemente** für Wiedergabe, Pause und Stopp gezeigt wurden. Die CodeBehind-Logik entspricht ebenfalls der Seite **Benutzerdefinierte Transportsteuerelemente**.

[![Benutzerdefinierte Positionierung](custom-positioning-images/custompositioning-small.png "Benutzerdefinierte Positionierung")](custom-positioning-images/custompositioning-large.png#lightbox "Benutzerdefinierte Positionierung")

Damit ist die Erläuterung des `VideoPlayer`-Steuerelements abgeschlossen.

## <a name="related-links"></a>Verwandte Links

- [Video Player Demos (Videoplayerdemos (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)
