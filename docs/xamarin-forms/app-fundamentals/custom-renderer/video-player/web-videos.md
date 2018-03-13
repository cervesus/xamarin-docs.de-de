---
title: Wiedergeben eines Web-Videos
ms.topic: article
ms.prod: xamarin
ms.assetid: 75781A10-865D-4BA8-8D6B-E3DA012922BC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: a5a98df4346c8720ae25fae4f27b5294993111c4
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="playing-a-web-video"></a>Wiedergeben eines Web-Videos

Die `VideoPlayer` Klasse definiert ein `Source` Eigenschaft verwendet, um die Quelle der Videodatei anzugeben sowie einen `AutoPlay` Eigenschaft. `AutoPlay` hat die Standardeinstellung von `true`, was bedeutet, dass das Video mit unterschiedlichen Rollen automatisch nach beginnen soll `Source` festgelegt wurde:

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

Die `Source` -Eigenschaft ist vom Typ `VideoSource`, die den Xamarin.Forms Standardressource ist [ `ImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/) abstrakte Klasse und ihre drei ableitungen [ `UriImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/), [ `FileImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FileImageSource/), und [ `StreamImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StreamImageSource/). Keine Stream-Option steht für die `VideoPlayer` jedoch, da iOS und Android Wiedergabe eines Videos aus einem Stream nicht unterstützt wird.

## <a name="video-sources"></a>Videoquellen

Die abstrakte `VideoSource` Klasse besteht ausschließlich aus drei statischen Methoden, die die drei Klassen zu, das Instanziieren von abgeleitet werden `VideoSource`:

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

Die `UriVideoSource` Klasse wird verwendet, um eine herunterladbare Videodatei mit einem URI angeben. Er definiert eine einzelne Eigenschaft vom Typ `string`:

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

Behandlung von Objekte vom Typ `UriVideoSource` wird im folgenden beschrieben.

Die `ResourceVideoSource` Klasse dient Videodateien, die als eingebettete Ressourcen in der Plattform-Anwendung, die auch mit angegebenen gespeichert sind, den Zugriff auf eine `string` Eigenschaft:

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

Behandlung von Objekte vom Typ `ResourceVideoSource` ist im Artikel beschriebenen [laden Anwendung Ressource Videos](loading-resources.md). Die `VideoPlayer` -Klasse verfügt über keine Möglichkeit, eine Videodatei gespeichert als Ressource in der portablen Klassenbibliothek zu laden.

Die `FileVideoSource` Klasse wird verwendet, um die Videobibliothek des Geräts Videodateien zuzugreifen. Die einzige Eigenschaft ist auch vom Typ `string`:

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

Behandlung von Objekte vom Typ `FileVideoSource` ist im Artikel beschriebenen [den Zugriff auf des Geräts Videobibliothek](accessing-library.md).

Die `VideoSource` Klasse enthält eine `TypeConverter` Attribut, das auf `VideoSourceConverter`:

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

Dieser Konverter aufgerufen wurde bei der `Source` Eigenschaft in eine Zeichenfolge in XAML festgelegt ist. So sieht die `VideoSourceConverter` Klasse:

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

Die `ConvertFromInvariantString` Methode versucht, die Zeichenfolge zum Konvertieren einer `Uri` Objekt. Wenn dies erfolgreich ist, und das Schema nicht `file:`, und klicken Sie dann die Methode gibt ein `UriVideoSource`. Andernfalls gibt es eine `ResourceVideoSource`.

## <a name="setting-the-video-source"></a>Festlegen der Videoquelle

Alle anderen im Zusammenhang mit Videoquellen Logik wird in den einzelnen Plattform Renderern mit implementiert. Die folgenden Abschnitte zeigen, wie die Renderer Plattform Videos spielen bei der `Source` -Eigenschaftensatz auf eine `UriVideoSource` Objekt.

### <a name="the-ios-video-source"></a>Die iOS-Videoquelle

Zwei Abschnitte mit den `VideoPlayerRenderer` Videoquelle von video Player beteiligt sind. Wenn Xamarin.Forms zunächst ein Objekt des Typs erstellt `VideoPlayer`, `OnElementChanged` -Methode aufgerufen wird und die `NewElement` -Eigenschaft des Arguments-Objekt festgelegt werden, mit denen `VideoPlayer`. Die `OnElementChanged` Methodenaufrufe `SetSource`:

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

Später auf, wenn die `Source` Eigenschaft geändert wird, die `OnElementPropertyChanged` -Methode aufgerufen wird und eine `PropertyName` Eigenschaft "Quelle", und `SetSource` erneut aufgerufen wird.

Wiedergeben eine Videodatei in iOS, ein Objekt des Typs [ `AVAsset` ](https://developer.xamarin.com/api/type/AVFoundation.AVAsset/) wird zuerst erstellt werden, um die Videodatei zu kapseln und, dient zum Erstellen einer [ `AVPlayerItem` ](https://developer.xamarin.com/api/type/AVFoundation.AVPlayerItem/), die dann an die übergebenwird`AVPlayer`Objekt. Hier wird wie die `SetSource` Methode behandelt das `Source` Eigenschaft wird vom Typ `UriVideoSource`:

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

Die `AutoPlay` Eigenschaft hat keine Entsprechung in den iOS-video-Klassen, damit die Eigenschaft am Ende der untersucht wird die `SetSource` aufzurufende der `Play` Methode für die `AVPlayer` Objekt.

In einigen Fällen Videos Fortsetzung Wiedergabe nach der Seite mit den `VideoPlayer` navigiert zur Startseite zurück. So beenden Sie das Video der `ReplaceCurrentItemWithPlayerItem` auch festgelegt ist, der `Dispose` außer Kraft setzen:

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

### <a name="the-android-video-source"></a>Android Videoquelle

Die Android `VideoPlayerRenderer` muss die Videoquelle des Players festgelegt bei der `VideoPlayer` wird zuerst erstellt und später bei der `Source` eigenschaftenänderungen:

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

Die `SetSource` Methode verarbeitet, Objekte des Typs `UriVideoSource` durch Aufrufen von `SetVideoUri` auf die `VideoView` mit einem Android `Uri` Objekt aus der URI-Zeichenfolge erstellt. Die `Uri` Klasse ist vollqualifizierte hier zur Unterscheidung von .NET `Uri` Klasse:

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

Gibt es ein Unterschied zwischen dem Verhalten des iOS und Android-Renderer ist, wenn die `Source` Eigenschaft `VideoPlayer` auf festgelegt ist `null`, oder wenn die `Uri` Eigenschaft `UriVideoSource` auf festgelegt ist `null` oder eine leere Zeichenfolge. Wenn der iOS-Videoplayer derzeit ein Video wiedergibt und `Source` auf festgelegt ist `null` (oder die Zeichenfolge ist `null` oder leer), `ReplaceCurrentItemWithPlayerItem` aufgerufen wird und `null` Wert. Das aktuelle Video ersetzt und die Wiedergabe beendet.

Eine ähnliche Funktion für unterstützt Android nicht. Wenn die `Source` -Eigenschaftensatz auf `null`die `SetSource` Methode einfach ignoriert, und das aktuelle Video wird weiter abgespielt.

### <a name="the-uwp-video-source"></a>Die uwp-Videoquelle

UWP `MediaElement` definiert eine `AutoPlay` -Eigenschaft, die in den Renderer, wie jede andere Eigenschaft behandelt wird:

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

Die `SetSource` -Eigenschaft behandelt eine `UriVideoSource` Objekt durch Festlegen der `Source` Eigenschaft `MediaElement` in ein .NET `Uri` Wert, oder auf `null` Wenn die `Source` Eigenschaft `VideoPlayer` auf festgelegt ist `null`:

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

Durch die Implementierung dieser Eigenschaften in den drei Renderern mit ist es möglich, die Wiedergabe eines Videos aus einer URL-Quelle. Die **Web Video abspielen** auf der Seite der [ **VideoPlayDemos** ]( https://developer.xamarin.com/samples/xamarin-forms/customrenderers/videoplayerdemos/index.md) Programm durch folgenden XAML-Datei definiert ist:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayWebVideoPage"
             Title="Play Web Video">

    <video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />

</ContentPage>
```

Die `VideoSourceConverter` Klasse konvertiert die Zeichenfolge in eine `UriVideoSource`. Wenn Sie zum Navigieren der **Web Video abspielen** Seite des Videos beginnt, laden und beginnt mit der Wiedergabe, wenn eine ausreichende Menge an Daten gepuffert und heruntergeladen wurde. Das Video ist ca. 10 Minuten lang:

[![Web-Video abspielen](web-videos-images/playwebvideo-small.png "Web Video abspielen")](web-videos-images/playwebvideo-large.png#lightbox "Web Video abspielen")

Auf allen drei Plattformen ausblenden die Transport-Steuerelemente, wenn sie nicht verwendet werden, jedoch wiederhergestellt werden können, um anzuzeigen, tippen Sie auf das Video.

Sie können verhindern, dass das Video automatisch gestartet wird, indem Sie die Einstellung der `AutoPlay` Eigenschaft `false`:

```xaml
<video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4"
                   AutoPlay="false" />
```

Sie müssen drücken die **wiedergeben** Schaltfläche, um das Video zu starten.

Auf ähnliche Weise können Sie die Anzeige der Transport Steuerelemente unterdrücken, indem die `AreTransportControlsEnabled` Eigenschaft `false`:

```xaml
<video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4"
                   AreTransportControlsEnabled="False" />
```

Wenn Sie beide Eigenschaften, um festlegen `false`, klicken Sie dann das Video abspielt, wird nicht, und es werden keine Möglichkeit, ihn zu starten! Sie müssten Aufrufen `Play` aus der CodeBehind-Datei oder eine eigene Steuerelemente zu erstellen, wie im Artikel beschrieben [implementieren benutzerdefinierte Video Transport Steuerelemente](custom-transport.md). 

Die **App.xaml** -Datei enthält zwei Videos finden Sie zusätzliche Ressourcen:

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

Auf eine von diesen anderen Filmen verweisen, können Sie die explizite URL im Ersetzen der **PlayWebVideo.xaml** -Datei mit einer `StaticResource` Markuperweiterung, in diesem Fall `VideoSourceConverter` ist nicht erforderlich, erstellen Sie die `UriVideoSource` Objekt:

```xaml
<video:VideoPlayer Source="{StaticResource ElephantsDream}" />
```

Alternativ können Sie festlegen der `Source` Eigenschaft aus einer Videodatei in einem `ListView`gemäß der Beschreibung in das nächste Dokument [Videoquellen binden, zu der Spieler](source-bindings.md).





## <a name="related-links"></a>Verwandte Links

- [Video Player Demos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
