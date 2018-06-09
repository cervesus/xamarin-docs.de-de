---
title: Benutzerdefinierte video Positionierung
description: In diesem Artikel wird erläutert, wie eine benutzerdefinierte Position Leiste in Videoplayer-Anwendungen, wobei Xamarin.Forms implementiert.
ms.prod: xamarin
ms.assetid: 6D792264-30FF-46F7-8C1B-2FEF9D277DF4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: b5f3c9dcbaa6ba1a9e86568ccabe38416cc653f2
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241909"
---
# <a name="custom-video-positioning"></a>Benutzerdefinierte video Positionierung

Die Transport-Steuerelemente, die von jeder Plattform implementiert umfassen einen Balken Position. Diese Leiste ähnelt einem Schieberegler oder Bildlaufleiste und zeigt die aktuelle Position des Videos innerhalb der gesamten Dauer. Darüber hinaus kann der Benutzer die Position Leiste vorwärts oder rückwärts verschieben, um eine neue Position im Video bearbeiten.

In diesem Artikel wird gezeigt, wie Sie eigene benutzerdefinierte Position Leiste implementieren können.

## <a name="the-duration-property"></a>Die Duration-Eigenschaft

Ein Element der Informationen, die `VideoPlayer` muss eine benutzerdefinierte unterstützen Position Leiste ist die Dauer des Videos. Die `VideoPlayer` definiert ein schreibgeschütztes `Duration` Eigenschaft vom Typ `TimeSpan`:

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

Wie die `Status` Eigenschaft beschrieben werden, der [vorherigen Artikel](custom-transport.md), gibt diese `Duration` Eigenschaft ist schreibgeschützt. Er definiert wird, mit einer privaten `BindablePropertyKey` und kann nur festgelegt werden, durch Verweisen auf die `IVideoPlayerController` -Schnittstelle, die Dies schließt `Duration` Eigenschaft:

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

Beachten Sie außerdem den durch geänderte Eigenschaften ausgelöste Handler, der eine Methode mit dem Namen ruft `SetTimeToEnd` weiter unten in diesem Artikel beschrieben wird.

Die Dauer eines Videos ist *nicht* verfügbar sofort, nachdem die `Source` Eigenschaft `VideoPlayer` festgelegt ist. Die Videodatei muss teilweise heruntergeladen werden, bevor der zugrunde liegenden Videoplayer seine Dauer bestimmen kann.

Hier wird wie jede Plattform-Renderer die Dauer des Videos erhält:

### <a name="video-duration-in-ios"></a>Video Dauer in iOS

In iOS, die Dauer eines Videos abgerufen wird, aus der `Duration` Eigenschaft `AVPlayerItem`, jedoch nicht sofort nach der `AVPlayerItem` wird erstellt. Es ist möglich, legen Sie einen iOS-Beobachter für die `Duration` -Eigenschaft, aber die `VideoPlayerRenderer` Ruft die Dauer in der `UpdateStatus` Methode, die 10 Mal pro Sekunde aufgerufen wird:

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

Die `ConvertTime` Methode konvertiert ein `CMTime` -Objekt an eine `TimeSpan` Wert.

### <a name="video-duration-in-android"></a>Video Dauer in Android

Die `Duration` Eigenschaft von der Android `VideoView` meldet eine gültige Dauer in Millisekunden bei der `Prepared` -Ereignis `VideoView` ausgelöst wird. Die Android `VideoPlayerRenderer` Klasse verwendet dieser Handler zum Abrufen der `Duration` Eigenschaft:

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

### <a name="video-duration-in-uwp"></a>Video Dauer in uwp-App

Die `NaturalDuration` Eigenschaft `MediaElement` ist ein `TimeSpan` Wert und wird ungültig, wenn `MediaElement` ausgelöst wird die `MediaOpened` Ereignis:

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

`VideoPlayer` muss auch ein `Position` -Eigenschaft, die im Bereich von 0 (null), um steigt `Duration` wie die Videowiedergabe. `VideoPlayer` implementiert diese Eigenschaft z. B. die `Position` Eigenschaft in UWP `MediaElement`, dies ist eine normale bindbare Eigenschaft mit öffentlichen `set` und `get` Zugriffsmethoden:

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

Die `get` Accessor gibt die aktuelle Position des Videos zurück, wie es wiedergegeben wird, aber die `set` Zugriffsmethode richtet sich so reagieren Sie auf der Benutzer die Manipulation des Balkens Position umgezogen video Position vorwärts oder rückwärts.

In iOS und Android, hat die Eigenschaft, die die aktuelle Position erhält nur eine `get` -Accessor und einen `Seek` Methode ist für die zweite Aufgabe verfügbar. Wenn Sie eine Separate Bedenken `Seek` Methode scheint ein vernünftiger Ansatz als ein einzelnes `Position` Eigenschaft. Ein einzelnes `Position` Eigenschaft verfügt über eine inhärenten Problems: als die Wiedergabe der `Position` Eigenschaft muss ständig aktualisiert werden, damit die neue Position wiedergegeben. Sie möchten jedoch nicht die meisten Änderungen an der `Position` Eigenschaft, um den Videoplayer auf eine neue Position im Video verschieben verursachen. In diesem Fall der Videoplayer würde reagieren, indem Sie Suchvorgänge auf den letzten Wert der `Position` -Eigenschaft, und das Video wäre nicht verschieben.

Trotz der Probleme bei der Implementierung einer `Position` Eigenschaft mit dem `set` und `get` Accessoren dieser Ansatz wurde gewählt, weil sie mit UWP konsistent sind `MediaElement`, und es wurde einen großen Vorteil mit Datenbindung: die `Position` Eigenschaft der `VideoPlayer` gebunden werden kann, um den Schieberegler, die sowohl auf die Position anzuzeigen und zu suchende zu einer neuen Position verwendet wird. Allerdings sind einige Vorsichtsmaßnahmen erforderlich, wenn Sie dies implementieren `Position` Eigenschaft, um Feedback Schleifen zu vermeiden.

### <a name="setting-and-getting-ios-position"></a>Festlegen und Abrufen von iOS-position

In iOS die `CurrentTime` Eigenschaft von der `AVPlayerItem` Objekt gibt an, die aktuelle Position der Wiedergabe von Videos. IOS `VideoPlayerRenderer` legt die `Position` Eigenschaft in der `UpdateStatus` Handler zur gleichen Zeit, die er festlegt der `Duration` Eigenschaft:

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

Der Renderer erkennt, wenn die `Position` Eigenschaftensatz aus `VideoPlayer` wurde geändert der `OnElementPropertyChanged` außer Kraft setzen und verwendet diesen neuen Wert aufrufen, eine `Seek` Methode auf der `AVPlayer` Objekt:

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

Beachten Sie, dass jedes Mal die `Position` Eigenschaft in `VideoPlayer` festgelegt ist, aus der `OnUpdateStatus` Handler auf, die `Position` Eigenschaft ausgelöst wird eine `PropertyChanged` -Ereignis, das in erkannt wird die `OnElementPropertyChanged` außer Kraft setzen. Für die meisten dieser Änderungen die `OnElementPropertyChanged` Methode nichts tun. Andernfalls würde bei jeder Änderung in der Zeitskala des Videos Position, es an derselben Position verschoben werden, die es nur bei Erreichen!

Um diese Feedbackschleife zu vermeiden der `OnElementPropertyChanged` nur Methodenaufrufe `Seek` bei den Unterschied zwischen der `Position` -Eigenschaft und der aktuellen Position des der `AVPlayer` ist größer als eine Sekunde.

### <a name="setting-and-getting-android-position"></a>Festlegen und Abrufen der Android-position

Wie in der iOS-Renderer, den Android `VideoPlayerRenderer` setzt einen neuen Wert für die `Position` Eigenschaft in der `OnUpdateStatus` Handler. Die `CurrentPosition` Eigenschaft `VideoView` enthält die neue Position in Einheiten von Millisekunden:

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

Wie in der iOS-Renderer, ruft der Android-Renderer außerdem der `SeekTo` Methode `VideoView` beim der `Position` -Eigenschaft geändert wurde, jedoch nur, wenn die Änderung mehr als eine Sekunde aus ist der `CurrentPosition` Wert `VideoView`:

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

### <a name="setting-and-getting-uwp-position"></a>Festlegen und Abrufen von uwp-position

UWP `VideoPlayerRenderer` Handles die `Position` auf die gleiche Weise wie IOS- und Android-Renderer, aber da die `Position` Eigenschaft von UWP `MediaElement` ist auch eine `TimeSpan` Wert ist keine Konvertierung erforderlich:

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

In einigen Fällen Videoplayer anzeigen, die im Video verbleibende Zeit. Dieser Wert beginnt mit der Zeitskala des Videos Dauer, wann das Video beginnt und auf null verringert werden, wenn das Video endet.

`VideoPlayer` enthält einen schreibgeschützten `TimeToEnd` -Eigenschaft, die sich vollständig innerhalb der Klasse behandelt wird basierend auf Änderungen an der `Duration` und `Position` Eigenschaften:

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

Die `SetTimeToEnd` Methode wird aufgerufen, über den Ereignishandler durch geänderte Eigenschaften ausgelöste beider `Duration` und `Position`.

## <a name="a-custom-slider-for-video"></a>Eine benutzerdefinierte Schieberegler Video

Es ist möglich, können Sie ein benutzerdefiniertes Steuerelement für einen Balken Position zu schreiben und verwenden Sie das Xamarin.Forms `Slider` oder eine Klasse, die abgeleitet `Slider`, z. B. die folgenden `PositionSlider` Klasse. Die Klasse definiert zwei neue Eigenschaften, die mit dem Namen `Duration` und `Position` des Typs `TimeSpan` , auf die zwei Eigenschaften mit demselben Namen in datengebundenen werden sollen `VideoPlayer`. Beachten Sie, dass der Bindungsmodus der Standardwert der `Position` Eigenschaft bidirektionale ist:

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

Durch geänderte Eigenschaften ausgelöste Handler für die `Duration` Eigenschaftensätze der `Maximum` Eigenschaft des zugrunde liegenden `Slider` auf die `TotalSeconds` Eigenschaft von der `TimeSpan` Wert. Auf ähnliche Weise, die durch geänderte Eigenschaften ausgelöste Handler für `Position` legt die `Value` Eigenschaft von der `Slider`. Auf diese Weise die zugrunde liegende `Slider` verfolgt die Position der `PositionSlider`.

Die `PositionSlider` wird aktualisiert, aus der zugrunde liegenden `Slider` in nur einer Instanz: Wenn der Benutzer bearbeitet die `Slider` , um anzugeben, dass das Video erweitert werden soll, oder zu einer neuen Position umgekehrt. Dies wird erkannt, der `PropertyChanged` im Konstruktor der Handler die `PositionSlider`. Der Handler überprüft, ob eine Änderung in der `Value` -Eigenschaft, und wenn es sich unterscheidet die `Position` -Eigenschaft, die `Position` Eigenschaftensatz wird von der `Value` Eigenschaft.

In der Theorie der inneren `if` Anweisung wie folgt geschrieben werden konnte:

```csharp
if (newPosition.Seconds != Position.Seconds)
{
    Position = newPosition;
}
```

Allerdings die Android-Implementierung von `Slider` besteht nur 1.000 diskrete Schritten, unabhängig von der `Minimum` und `Maximum` Einstellungen. Die Länge eines Videos ist größer als 1.000 Sekunden, und klicken Sie dann auf zwei verschiedenen `Position` Werte würde die gleiche entsprechen `Value` festlegen, der die `Slider`, und dies `if` Anweisung falsch-positive Werte für einen Benutzer die Bearbeitung von ausgelöst würde die `Slider`. Es ist sicherer, stattdessen überprüfen, dass die neuen und vorhandenen Position größer als die Gesamtdauer Hundertstelsekunde sind.

## <a name="using-the-positionslider"></a>Verwenden die PositionSlider

Dokumentation für die universelle Windows-Plattform [ `MediaElement` ](/uwp/api/Windows.UI.Xaml.Controls.MediaElement/) gibt eine Warnung aus, über das Binden an die `Position` Eigenschaft, da die Eigenschaft häufig aktualisiert. Die Dokumentation empfiehlt, dass ein Timer verwendet werden, Abfrage die `Position` Eigenschaft.

Dies ist eine gute Empfehlung, aber die drei `VideoPlayerRenderer` Klassen sind bereits indirekt mithilfe eines Zeitgebers zum Aktualisieren der `Position` Eigenschaft. Die `Position` -Eigenschaft geändert wird, in einen Handler für das `UpdateStatus` -Ereignis, das nur 10 Mal pro Sekunde ausgelöst wird.

Aus diesem Grund die `Position` Eigenschaft der `VideoPlayer` gebunden werden kann, um die `Position` Eigenschaft der `PositionSlider` ohne Leistungsprobleme, wie in der **benutzerdefinierte Position Leiste** Seite:

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

Die ersten drei Punkten (· ·) verbirgt die `ActivityIndicator`; er ist der gleiche wie im vorherigen **Custom-Transporteigenschaften** Seite. Beachten Sie die beiden `Label` Elemente anzeigen die `Position` und `TimeToEnd` Eigenschaften. Mit den Auslassungspunkten zwischen diesen beiden `Label` Elemente Blendet Sie aus den beiden `Button` Elemente angezeigt, der **Custom-Transporteigenschaften** Seite für die Wiedergabe, Pause und beenden. Die Code-Behind-Logik entspricht auch der **Custom-Transporteigenschaften** Seite.

[![Benutzerdefinierte Positionierung](custom-positioning-images/custompositioning-small.png "benutzerdefinierte Positionierung")](custom-positioning-images/custompositioning-large.png#lightbox "benutzerdefinierte Positionierung")

Dies schließt die Erläuterung der `VideoPlayer`.

## <a name="related-links"></a>Verwandte Links

- [Video Player Demos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
