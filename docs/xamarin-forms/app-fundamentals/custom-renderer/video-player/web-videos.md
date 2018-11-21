---
title: Wiedergeben eines Webvideos
description: In diesem Artikel wird erläutert, wie Wiedergabe von Webvideos in einer video-Player-Anwendung, die mit Xamarin.Forms wird.
ms.prod: xamarin
ms.assetid: 75781A10-865D-4BA8-8D6B-E3DA012922BC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 7f40d0d11fc932121b4ff7789969bbb1e354024c
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2018
ms.locfileid: "52172209"
---
# <a name="playing-a-web-video"></a>Wiedergeben eines Webvideos

Die `VideoPlayer` -Klasse definiert eine `Source` Eigenschaft verwendet, um die Quelle der Videodatei, angegeben als auch eines `AutoPlay` Eigenschaft. `AutoPlay` hat die Standardeinstellung von `true`, was bedeutet, dass das Video automatisch nach Wiedergabe beginnen soll `Source` festgelegt wurde:

```csharp
using System;
using Xamarin.Forms;

namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Source property
        public static readonly BindableProperty SourceProperty =
            BindableProperty.Create(nameof(Source), typeof(VideoSource), typeof(VideoPlayer), null);

        [TypeConverter(typeof(VideoSourceConverter))]
        public VideoSource Source
        {
            set { SetValue(SourceProperty, value); }
            get { return (VideoSource)GetValue(SourceProperty); }
        }

        // AutoPlay property
        public static readonly BindableProperty AutoPlayProperty =
            BindableProperty.Create(nameof(AutoPlay), typeof(bool), typeof(VideoPlayer), true);

        public bool AutoPlay
        {
            set { SetValue(AutoPlayProperty, value); }
            get { return (bool)GetValue(AutoPlayProperty); }
        }
        ···
    }
}
```

Die `Source` Eigenschaft ist vom Typ `VideoSource`, die das Xamarin.Forms Standardressource ist [ `ImageSource` ](xref:Xamarin.Forms.ImageSource) abstrakte Klasse, und seine drei Derivate, [ `UriImageSource` ](xref:Xamarin.Forms.UriImageSource), [ `FileImageSource` ](xref:Xamarin.Forms.FileImageSource), und [ `StreamImageSource` ](xref:Xamarin.Forms.StreamImageSource). Kein Stream-Option steht für die `VideoPlayer` jedoch, weil iOS und Android Wiedergabe eines Videos aus einem Stream nicht unterstützt wird.

## <a name="video-sources"></a>Bildquellen

Die abstrakte `VideoSource` Klasse besteht ausschließlich aus drei statische Methoden, die die drei Klassen zu, die Instanziieren von abgeleitet aus `VideoSource`:

```csharp
namespace FormsVideoLibrary
{
    [TypeConverter(typeof(VideoSourceConverter))]
    public abstract class VideoSource : Element
    {
        public static VideoSource FromUri(string uri)
        {
            return new UriVideoSource() { Uri = uri };
        }

        public static VideoSource FromFile(string file)
        {
            return new FileVideoSource() { File = file };
        }

        public static VideoSource FromResource(string path)
        {
            return new ResourceVideoSource() { Path = path };
        }
    }
}
```

Die `UriVideoSource` Klasse wird verwendet, um eine herunterladbare Datei mit einem URI angeben. Definiert eine einzelne Eigenschaft vom Typ `string`:

```csharp
namespace FormsVideoLibrary
{
    public class UriVideoSource : VideoSource
    {
        public static readonly BindableProperty UriProperty =
            BindableProperty.Create(nameof(Uri), typeof(string), typeof(UriVideoSource));

        public string Uri
        {
            set { SetValue(UriProperty, value); }
            get { return (string)GetValue(UriProperty); }
        }
    }
}
```

Behandeln Objekte des Typs `UriVideoSource` wird im folgenden beschrieben.

Die `ResourceVideoSource` Klasse wird verwendet, um die Videodateien, die als eingebettete Ressourcen in der Platform-Anwendung, die auch mit angegebenen gespeichert sind, Zugriff auf eine `string` Eigenschaft:

```csharp
namespace FormsVideoLibrary
{
    public class ResourceVideoSource : VideoSource
    {
        public static readonly BindableProperty PathProperty =
            BindableProperty.Create(nameof(Path), typeof(string), typeof(ResourceVideoSource));

        public string Path
        {
            set { SetValue(PathProperty, value); }
            get { return (string)GetValue(PathProperty); }
        }
    }
}
```

Behandeln Objekte des Typs `ResourceVideoSource` wird in diesem Artikel beschrieben [Laden von Anwendungsressourcenvideos](loading-resources.md). Die `VideoPlayer` -Klasse verfügt über keine Funktion zum Laden einer Videodatei als Ressource in der .NET Standard-Bibliothek gespeichert.

Die `FileVideoSource` Klasse wird verwendet, um die Videobibliothek des Geräts Videodateien zugreifen. Es ist auch die einzelne Eigenschaft vom Typ `string`:

```csharp
namespace FormsVideoLibrary
{
    public class FileVideoSource : VideoSource
    {
        public static readonly BindableProperty FileProperty =
                  BindableProperty.Create(nameof(File), typeof(string), typeof(FileVideoSource));

        public string File
        {
            set { SetValue(FileProperty, value); }
            get { return (string)GetValue(FileProperty); }
        }
    }
}
```

Behandeln Objekte des Typs `FileVideoSource` wird in diesem Artikel beschrieben [Zugriff auf die Videobibliothek des Geräts](accessing-library.md).

Die `VideoSource` Klasse enthält eine `TypeConverter` Attribut, das verweist `VideoSourceConverter`:

```csharp
namespace FormsVideoLibrary
{
    [TypeConverter(typeof(VideoSourceConverter))]
    public abstract class VideoSource : Element
    {
        ···
    }
}
```

Dieser Konverter wird aufgerufen, wenn die `Source` -Eigenschaftensatz auf eine Zeichenfolge im XAML. Hier ist die `VideoSourceConverter` Klasse:

```csharp
namespace FormsVideoLibrary
{
    public class VideoSourceConverter : TypeConverter
    {
        public override object ConvertFromInvariantString(string value)
        {
            if (!String.IsNullOrWhiteSpace(value))
            {
                Uri uri;
                return Uri.TryCreate(value, UriKind.Absolute, out uri) && uri.Scheme != "file" ?
                                VideoSource.FromUri(value) : VideoSource.FromResource(value);
            }

            throw new InvalidOperationException("Cannot convert null or whitespace to ImageSource");
        }
    }
}
```

Die `ConvertFromInvariantString` Methode versucht, konvertieren Sie die Zeichenfolge, die eine `Uri` Objekt. Wenn dies erfolgreich ist, und das Schema nicht `file:`, und klicken Sie dann die Methode gibt eine `UriVideoSource`. Andernfalls wird eine `ResourceVideoSource`.

## <a name="setting-the-video-source"></a>Videoquelle

Alle anderen im Zusammenhang mit Videoquellen Logik wird in den einzelnen Platform Renderern mit implementiert. Die folgenden Abschnitte zeigen, wie die Plattform-Renderer Videos wiedergegeben werden bei der `Source` -Eigenschaftensatz auf eine `UriVideoSource` Objekt.

### <a name="the-ios-video-source"></a>Die iOS-Videoquelle

Zwei Abschnitte mit den `VideoPlayerRenderer` sind beteiligt, der die verfügbaren Videoplayer Videoquelle festzulegen. Xamarin.Forms erstellt Wenn zunächst ein Objekt des Typs `VideoPlayer`, `OnElementChanged` Methode wird aufgerufen, mit der `NewElement` -Eigenschaft des Objekts Argumente festlegen, `VideoPlayer`. Die `OnElementChanged` Methodenaufrufe `SetSource`:

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
                SetSource();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.SourceProperty.PropertyName)
            {
                SetSource();
            }
            ···
        }
        ···
    }
}
```

Unten auf, wenn die `Source` -Eigenschaft geändert wird, die `OnElementPropertyChanged` Methode wird aufgerufen, mit einer `PropertyName` Eigenschaft "Source" und `SetSource` erneut aufgerufen wird.

Zum Wiedergeben einer Videodatei in iOS, ein Objekt vom Typ [ `AVAsset` ](https://developer.xamarin.com/api/type/AVFoundation.AVAsset/) zuerst erstellt, um die Datei, zu kapseln und dient zum Erstellen einer [ `AVPlayerItem` ](https://developer.xamarin.com/api/type/AVFoundation.AVPlayerItem/), die wird dann an die übergeben`AVPlayer`Objekt. Hier ist die `SetSource` verarbeitet die `Source` Eigenschaft, wenn es vom Typ `UriVideoSource`:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        AVPlayer player;
        AVPlayerItem playerItem;
        ···
        void SetSource()
        {
            AVAsset asset = null;

            if (Element.Source is UriVideoSource)
            {
                string uri = (Element.Source as UriVideoSource).Uri;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    asset = AVAsset.FromUrl(new NSUrl(uri));
                }
            }
            ···
            if (asset != null)
            {
                playerItem = new AVPlayerItem(asset);
            }
            else
            {
                playerItem = null;
            }

            player.ReplaceCurrentItemWithPlayerItem(playerItem);

            if (playerItem != null && Element.AutoPlay)
            {
                player.Play();
            }
        }
        ···
    }
}
```

Die `AutoPlay` Eigenschaft hat keine Analog in den iOS-video-Klassen, sodass die Eigenschaft, am Ende überprüft wird der `SetSource` aufzurufende Methode der `Play` Methode für die `AVPlayer` Objekt.

In einigen Fällen Videos weiter spielen nach der Seite mit den `VideoPlayer` navigiert zurück zur Startseite. Video, das Beenden der `ReplaceCurrentItemWithPlayerItem` ist auch im Festlegen der `Dispose` außer Kraft setzen:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        protected override void Dispose(bool disposing)
        {
            base.Dispose(disposing);

            if (player != null)
            {
                player.ReplaceCurrentItemWithPlayerItem(null);
            }
        }
        ···
    }
}
```

### <a name="the-android-video-source"></a>Die Android Videoquelle

Die Android `VideoPlayerRenderer` muss die Videoquelle des Players festgelegt bei der `VideoPlayer` wird zuerst erstellt und später bei der `Source` eigenschaftsänderungen:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetSource();
                ···
            }
        }
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.SourceProperty.PropertyName)
            {
                SetSource();
            }
            ···
        }
        ···
    }
}
```

Die `SetSource` Methode verarbeitet, Objekte des Typs `UriVideoSource` durch Aufrufen von `SetVideoUri` auf die `VideoView` mit Android `Uri` Objekt aus der URI-Zeichenfolge erstellt. Die `Uri` Klasse ist vollqualifizierte hier zur Unterscheidung von .NET `Uri` Klasse:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        void SetSource()
        {
            isPrepared = false;
            bool hasSetSource = false;

            if (Element.Source is UriVideoSource)
            {
                string uri = (Element.Source as UriVideoSource).Uri;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    videoView.SetVideoURI(Android.Net.Uri.Parse(uri));
                    hasSetSource = true;
                }
            }
            ···

            if (hasSetSource && Element.AutoPlay)
            {
                videoView.Start();
            }
        }
        ···
    }
}
```

Die Android `VideoView` verfügt nicht über ein entsprechendes `AutoPlay` -Eigenschaft, sodass die `Start` Methode wird aufgerufen, wenn ein neues Video festgelegt wurde.

Gibt es ein Unterschied zwischen dem Verhalten des iOS und Android-Renderer ist, wenn die `Source` Eigenschaft `VideoPlayer` nastaven NA hodnotu `null`, oder wenn der `Uri` Eigenschaft `UriVideoSource` nastaven NA hodnotu `null` oder eine leere Zeichenfolge. Wenn des iOS-Videoplayers derzeit ein Video abgespielt wird und `Source` nastaven NA hodnotu `null` (oder die Zeichenfolge ist `null` oder leer), `ReplaceCurrentItemWithPlayerItem` aufgerufen wird und `null` Wert. Das aktuelle Video ersetzt wird und die Wiedergabe beendet.

Eine ähnliche Funktion für unterstützt Android nicht. Wenn die `Source` -Eigenschaftensatz auf `null`, `SetSource` Methode, die es, einfach ignoriert, und das aktuelle Video wird weiter abgespielt.

### <a name="the-uwp-video-source"></a>Die UWP-Videoquelle

Die UWP `MediaElement` definiert eine `AutoPlay` -Eigenschaft, die in den Renderer, wie jede andere Eigenschaft behandelt wird:

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
                SetSource();
                SetAutoPlay();
                ···    
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.SourceProperty.PropertyName)
            {
                SetSource();
            }
            else if (args.PropertyName == VideoPlayer.AutoPlayProperty.PropertyName)
            {
                SetAutoPlay();
            }
            ···
        }
        ···
    }
}
```

Die `SetSource` -Eigenschaft behandelt eine `UriVideoSource` Objekt durch Festlegen der `Source` Eigenschaft `MediaElement` in ein .NET `Uri` Wert oder `null` Wenn die `Source` Eigenschaft `VideoPlayer` nastaven NA hodnotu `null`:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        async void SetSource()
        {
            bool hasSetSource = false;

            if (Element.Source is UriVideoSource)
            {
                string uri = (Element.Source as UriVideoSource).Uri;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    Control.Source = new Uri(uri);
                    hasSetSource = true;
                }
            }
            ···
            if (!hasSetSource)
            {
                Control.Source = null;
            }
        }

        void SetAutoPlay()
        {
            Control.AutoPlay = Element.AutoPlay;
        }
        ···
    }
}
```

## <a name="setting-a-url-source"></a>Festlegen einer URL-Quelle

Bei der Implementierung dieser Eigenschaften in den drei Renderern mit ist es möglich, die Wiedergabe eines Videos aus einer URL-Quelle. Die **Wiedergeben eines Webvideos** auf der Seite die [ **VideoPlayDemos** ]( https://developer.xamarin.com/samples/xamarin-forms/customrenderers/videoplayerdemos/index.md) Programm wird durch den folgenden XAML-Datei definiert:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayWebVideoPage"
             Title="Play Web Video">

    <video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />

</ContentPage>
```

Die `VideoSourceConverter` Klasse konvertiert die Zeichenfolge in eine `UriVideoSource`. Beim Navigieren zur der **Web-Video wiedergeben** Seite, die das Video beginnt, laden und wiedergeben, wenn eine ausreichende Menge von Daten gepuffert und heruntergeladen wurde gestartet. Das Video ist ungefähr 10 Minuten lang:

[![Web-Video wiedergeben](web-videos-images/playwebvideo-small.png "Web-Video wiedergeben")](web-videos-images/playwebvideo-large.png#lightbox "Web-Video wiedergeben")

Auf allen Plattformen Ausblenden der Transport-Steuerelemente, wenn sie nicht verwendet werden, jedoch wiederhergestellt werden können, um durch Tippen auf das Video anzuzeigen.

Sie können verhindern, dass das Video automatisch gestartet, durch Festlegen der `AutoPlay` Eigenschaft `false`:

```xaml
<video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4"
                   AutoPlay="false" />
```

Müssen Sie drücken die **spielen** Schaltfläche, um das Video zu starten.

Auf ähnliche Weise können Sie die Anzeige der Steuerelemente Transport unterdrücken, indem die `AreTransportControlsEnabled` Eigenschaft `false`:

```xaml
<video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4"
                   AreTransportControlsEnabled="False" />
```

Wenn Sie beide Eigenschaften auf `false`, klicken Sie dann das Video abspielt, wird nicht, und es werden keine Möglichkeit, starten Sie ihn! Sie müssten Aufrufen `Play` aus der CodeBehind-Datei oder Ihre eigenen Transport-Steuerelemente zu erstellen, wie in diesem Artikel beschrieben [benutzerdefinierte Video-Transport-Steuerelemente implementieren](custom-transport.md).

Die **"App.xaml"** -Datei enthält Ressourcen für zwei zusätzliche Videos:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.App">
    <Application.Resources>
        <ResourceDictionary>

            <video:UriVideoSource x:Key="ElephantsDream"
                                  Uri="https://archive.org/download/ElephantsDream/ed_hd_512kb.mp4" />

            <video:UriVideoSource x:Key="BigBuckBunny"
                                  Uri="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />

            <video:UriVideoSource x:Key="Sintel"
                                  Uri="https://archive.org/download/Sintel/sintel-2048-stereo_512kb.mp4" />

        </ResourceDictionary>
    </Application.Resources>
</Application>
```

Um auf eine dieser anderen Filme verweisen, können Sie ersetzen die explizite URL in die **PlayWebVideo.xaml** -Datei mit einer `StaticResource` Markuperweiterung, in diesem Fall `VideoSourceConverter` ist nicht erforderlich, zum Erstellen der `UriVideoSource` Objekt:

```xaml
<video:VideoPlayer Source="{StaticResource ElephantsDream}" />
```

Alternativ können Sie festlegen der `Source` Eigenschaft aus einer Videodatei in einem `ListView`, wie im nächsten Artikel beschrieben [Binden von Videoquellen an den Spieler](source-bindings.md).





## <a name="related-links"></a>Verwandte Links

- [Videodemos Player (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
