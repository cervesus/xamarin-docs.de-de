---
title: Wiedergeben eines Webvideos
description: In diesem Artikel wird erläutert, wie Webvideos in einer Videoplayeranwendung mithilfe von Xamarin.Forms wiedergegeben werden.
ms.prod: xamarin
ms.assetid: 75781A10-865D-4BA8-8D6B-E3DA012922BC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 107b2a970041c70bb021b03dd98f8c91eaea8d34
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70771761"
---
# <a name="playing-a-web-video"></a>Wiedergeben eines Webvideos

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)

Die `VideoPlayer`-Klasse definiert eine `Source`-Eigenschaft, die zum Festlegen der Quelle der Videodatei verwendet wird, sowie eine `AutoPlay`-Eigenschaft. `AutoPlay` verfügt über die Standardeinstellung `true`, d.h., das Video sollte automatisch abgespielt werden, nachdem `Source` festgelegt wurde:

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

Die `Source`-Eigenschaft weist den Typ `VideoSource`, der der abstrakten Xamarin.Forms-Klasse [`ImageSource`](xref:Xamarin.Forms.ImageSource) nachempfunden ist, sowie dessen Derivate [`UriImageSource`](xref:Xamarin.Forms.UriImageSource), [`FileImageSource`](xref:Xamarin.Forms.FileImageSource) und [`StreamImageSource`](xref:Xamarin.Forms.StreamImageSource) auf. Für `VideoPlayer` ist jedoch keine Streamingoption verfügbar, da iOS und Android die Wiedergabe von Videos aus einem Stream nicht unterstützen.

## <a name="video-sources"></a>Videoquellen

Die abstrakte Klasse `VideoSource` besteht ausschließlich aus drei statischen Methoden, die die drei Klassen instanziieren, die von `VideoSource` abgeleitet werden:

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

Die `UriVideoSource`-Klasse wird zum Angeben einer herunterladbaren Videodatei mit einem URI verwendet. Sie definiert eine einzelne Eigenschaft vom Typ `string`:

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

Das Behandeln von Objekten vom Typ `UriVideoSource` wird unten beschrieben.

Die `ResourceVideoSource`-Klasse wird verwendet, um auf Videodateien zuzugreifen, die als eingebettete Ressourcen in der Plattformanwendung gespeichert werden. Sie wird auch mit einer `string`-Eigenschaft angegeben:

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

Das Behandeln von Objekten vom Typ `ResourceVideoSource` wird im Artikel [Loading Application Resource Videos (Laden von Anwendungsressourcenvideos)](loading-resources.md) beschrieben. Die `VideoPlayer`-Klasse verfügt über keine Funktion zum Laden von Videodateien, die als Ressource in der .NET Standard-Bibliothek gespeichert sind.

Die `FileVideoSource`-Klasse wird für den Zugriff auf Videodateien über die Videobibliothek des Geräts verwendet. Die einzelne Eigenschaft weist ebenfalls den Typ `string` auf:

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

Das Behandeln von Objekten vom Typ `FileVideoSource` wird im Artikel [Accessing the Device's Video Library (Zugreifen auf die Videobibliothek des Geräts)](accessing-library.md) beschrieben.

Die `VideoSource`-Klasse enthält ein `TypeConverter`-Attribut, das auf `VideoSourceConverter` verweist:

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

Dieser Typkonverter wird aufgerufen, wenn die `Source`-Eigenschaft im XAML-Code auf eine Zeichenfolge festgelegt wird. Dies ist die `VideoSourceConverter`-Klasse:

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

Die `ConvertFromInvariantString`-Methode versucht, die Zeichenfolge in ein `Uri`-Objekt zu konvertieren. Wenn dies erfolgreich ist, und das Schema nicht `file:` ist, gibt die Methode `UriVideoSource` zurück. Andernfalls wird `ResourceVideoSource` zurückgegeben.

## <a name="setting-the-video-source"></a>Festlegen der Videoquelle

Jegliche weitere Logik bezüglich Videoquellen wird in den individuellen Rendern der jeweiligen Plattform implementiert. In den folgenden Abschnitten erfahren Sie, wie die Plattformrenderer Videos wiedergeben, wenn die `Source`-Eigenschaft für ein `UriVideoSource`-Objekt festgelegt wird.

### <a name="the-ios-video-source"></a>Die iOS-Videoquelle

Zwei Abschnitte von `VideoPlayerRenderer` sind für das Festlegen der Videoquelle des Videoplayers relevant. Wenn Xamarin.Forms zuerst ein Objekt vom Typ `VideoPlayer` erstellt, wird die Methode `OnElementChanged` mit der auf `VideoPlayer` festgelegten Eigenschaft `NewElement` des Objekts des Arguments aufgerufen. Die `OnElementChanged`-Methode ruft `SetSource` auf:

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

Später, wenn die `Source`-Eigenschaft geändert wird, wird die `OnElementPropertyChanged`-Methode mit einer `PropertyName`-Eigenschaft mit dem Wert „Source“ aufgerufen, und `SetSource` wird erneut aufgerufen.

Zuerst wird ein Objekt vom Typ [`AVAsset`](xref:AVFoundation.AVAsset) zum Kapseln der Videodatei erstellt, die dann zum Erstellen einer [`AVPlayerItem`](xref:AVFoundation.AVPlayerItem)-Klasse wird, die wiederum an das `AVPlayer`-Objekt übergeben wird, um eine Videodatei unter iOS wiederzugeben. Im Folgenden wird veranschaulicht, wie die `SetSource`-Methode die `Source`-Eigenschaft verarbeitet, wenn sie den Typ `UriVideoSource` aufweist:

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

Für die `AutoPlay`-Eigenschaft gibt es in den Klassen für Videos unter iOS kein Gegenstück. Deshalb wird die Eigenschaft am Ende der `SetSource`-Methode untersucht, um die `Play`-Methode für das `AVPlayer`-Objekt aufzurufen.

In einigen Fällen wurden Videos weiterhin abgespielt, nachdem von der Seite mit dem `VideoPlayer` zurück zur Homepage navigiert wurde. Der `ReplaceCurrentItemWithPlayerItem`-Vorgang ist auch in der `Dispose`-Überschreibung festgelegt, um das Video zu beenden:

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

### <a name="the-android-video-source"></a>Die Android-Videoquelle

Für die `VideoPlayerRenderer`-Klasse für Android muss die Videoquelle des Players festgelegt werden, wenn die `VideoPlayer`-Klasse erstellt wird oder später, wenn die `Source`-Eigenschaft geändert wird:

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

Die `SetSource`-Methode verarbeitet Objekte vom Typ `UriVideoSource`, indem `SetVideoUri` für `VideoView` mit einem aus dem Zeichenfolgen-URI erstellten `Uri`-Objekt für Android aufgerufen wird. Die `Uri`-Klasse ist hier vollqualifiziert, damit sie leichter von der .NET-Klasse `Uri` unterschieden werden kann:

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

Die `VideoView`-Klasse für Android verfügt über keine entsprechende `AutoPlay`-Eigenschaft. Deshalb wird die `Start`-Methode aufgerufen, wenn ein neues Video festgelegt wurde.

Zwischen dem Verhalten von iOS- und Android-Renderern besteht ein Unterschied, wenn die `Source`-Eigenschaft von `VideoPlayer` auf `null` festgelegt wird oder die `Uri`-Eigenschaft von `UriVideoSource` auf `null` oder eine leere Zeichenfolge festgelegt wird. Wenn der iOS-Videoplayer derzeit ein Video abspielt und `Source` auf `null` festgelegt ist (bzw. die Zeichenfolge ist `null` oder leer), wird `ReplaceCurrentItemWithPlayerItem` mit dem Wert `null` aufgerufen. Das aktuelle Video wird gestoppt und ersetzt.

Android verfügt über keine ähnliche Funktion. Wenn `null` für die `Source`-Eigenschaft festgelegt ist, wird diese von der `SetSource`-Methode einfach ignoriert, und das aktuelle Video wird weiterhin abgespielt.

### <a name="the-uwp-video-source"></a>Die UWP-Videoquelle

Das `MediaElement` der UWP definiert eine `AutoPlay`-Eigenschaft, die im Renderer wie jede andere Eigenschaft verarbeitet wird:

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

Die `SetSource`-Eigenschaft verarbeitet ein `UriVideoSource`-Objekt, indem sie die `Source`-Eigenschaft von `MediaElement` auf einen `Uri`-Wert von .NET oder auf `null`, wenn die `Source`-Eigenschaft von `VideoPlayer` auf `null` festgelegt ist, festlegt:

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

Wenn diese Eigenschaften in die drei Renderer implementiert sind, können Sie ein Video aus einer URL-Quelle wiedergeben. Die Seite **Play Web Video** (Webvideo abspielen) im Programm [**VideoPlayDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos) wird von der folgenden XAML-Datei definiert:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayWebVideoPage"
             Title="Play Web Video">

    <video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />

</ContentPage>
```

Die `VideoSourceConverter`-Klasse konvertiert die Zeichenfolge in `UriVideoSource`. Wenn Sie zur Seite **Play Web Video** (Webvideo abspielen) navigieren, wird das Video geladen und abgespielt, sobald ausreichend Daten heruntergeladen und gepuffert wurden. Das Video ist etwa 10 Minuten lang:

[![Webvideo abspielen](web-videos-images/playwebvideo-small.png "Webvideo abspielen")](web-videos-images/playwebvideo-large.png#lightbox "Webvideo abspielen")

Auf jeder Plattform werden die Transportsteuerelemente ausgeblendet, wenn sie nicht verwendet werden. Jedoch können sie durch Tippen auf das Video wieder eingeblendet werden.

Sie können das automatische Starten des Videos verhindern, indem Sie `false` für die Eigenschaft `AutoPlay` festlegen:

```xaml
<video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4"
                   AutoPlay="false" />
```

Dann müssen Sie auf die Schaltfläche **Wiedergeben** tippen, um das Video zu starten.

Auf ähnliche Weise können Sie die Anzeige der Transportsteuerelemente verhindern, indem Sie `false` für die Eigenschaft `AreTransportControlsEnabled` festlegen:

```xaml
<video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4"
                   AreTransportControlsEnabled="False" />
```

Wenn Sie beide Eigenschaften auf `false` festlegen, wird das Video nicht abgespielt, und es gibt keine Möglichkeit zum Starten des Videos. In diesem Fall müssten Sie `Play` von der CodeBehind-Datei aufrufen oder Ihre eigenen Transportsteuerelemente wie im Artikel [Implementing Custom Video Transport Controls (Implementieren benutzerdefinierter Transport-Steuerelemente für Videos)](custom-transport.md) beschrieben erstellen.

Die Datei **App.xaml** enthält Ressourcen für zwei zusätzliche Videos:

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

Sie können die explizite URL in der Datei **PlayWebVideo.xaml** durch eine `StaticResource`-Markuperweiterung ersetzen, um auf eines dieser anderen Videos zu verweisen. In diesem Fall ist `VideoSourceConverter` nicht erforderlich, um das `UriVideoSource`-Objekt zu erstellen:

```xaml
<video:VideoPlayer Source="{StaticResource ElephantsDream}" />
```

Alternativ können Sie die `Source`-Eigenschaft einer Videodatei wie im nächsten Artikel ([Binding Video Sources to the Player (Binden von Videoquellen an den Player)](source-bindings.md)) beschrieben in einem `ListView`-Element festlegen.

## <a name="related-links"></a>Verwandte Links

- [Video Player Demos (Videoplayerdemos (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)
