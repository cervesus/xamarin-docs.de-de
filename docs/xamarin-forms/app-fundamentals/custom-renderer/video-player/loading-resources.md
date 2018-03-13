---
title: Laden die Anwendung Ressourcen videos
ms.topic: article
ms.prod: xamarin
ms.assetid: F75BD540-9354-4C17-A119-57F3DEC66D54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 08d07a82651887c9d87b908acd82296a3d80e43f
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="loading-application-resource-videos"></a>Laden die Anwendung Ressourcen videos

Die benutzerdefinierten Renderer für die `VideoPlayer` Ansicht Videodateien, die in den einzelnen plattformprojekten als Anwendungsressourcen eingebettet wurden wiedergegeben werden. Allerdings die aktuelle Version des `VideoPlayer` keinen Zugriff auf Ressourcen in einer portablen Klassenbibliothek eingebettet.

Um diese Ressourcen zu laden, erstellen Sie eine Instanz des `ResourceVideoSource` durch Festlegen der `Path` Eigenschaft für den Dateinamen (oder den Ordner und Dateiname) der Ressource. Rufen Sie alternativ die statische `VideoSource.FromResource` Methode, um die Ressource verweisen. Schalten Sie dann die `ResourceVideoSource` -Objekt an die `Source` Eigenschaft `VideoPlayer`. 

## <a name="storing-the-video-files"></a>Speichern der video-Dateien

Speichern eine Videodatei im plattformprojekt unterscheidet sich für die drei Plattformen:

### <a name="ios-video-resources"></a>iOS-Videoressourcen

Ein iOS-Projekt können Sie ein Video in speichern die **Ressourcen** Ordner oder einen Unterordner des der **Ressourcen** Ordner. Die Videodatei benötigen eine `Build Action` von `BundleResource`. Festlegen der `Path` Eigenschaft `ResourceVideoSource` zum Dateinamen, z. B. **MyFile.mp4** für eine Datei in die **Ressourcen** Ordner oder **MyFolder/MyFile.mp4**, wobei **MyFolder** ist ein Unterordner des **Ressourcen**.

In der **VideoPlayerDemos** Lösung, die **VideoPlayerDemos.iOS** Projekt enthält einen Unterordner des **Ressourcen** mit dem Namen **Videos** enthält eine Datei namens **iOSApiVideo.mp4**. Dies ist ein kurzes Video an, die Ihnen zeigt, wie Sie die Xamarin-Website zu verwenden, um die Dokumentation für das iOS finden Sie `AVPlayerViewController` Klasse.

### <a name="android-video-resources"></a>Android Videoressourcen

In einem Android-Projekt müssen in einem Unterordner des Videos gespeichert werden **Ressourcen** mit dem Namen **unformatierten**. Die **unformatierten** Ordner darf keine Unterordner enthalten. Geben Sie die Datei eine `Build Action` von `AndroidResource`. Legen Sie die `Path` Eigenschaft `ResourceVideoSource` zum Dateinamen, z. B. **MyFile.mp4**. 

Die **VideoPlayerDemos.Android** Projekt enthält einen Unterordner des **Ressourcen** mit dem Namen **unformatierten**, enthält eine Datei namens **AndroidApiVideo.mp4**. 

### <a name="uwp-video-resources"></a>Uwp-video-Ressourcen

In einem Uwp-Projekt können Sie Videos in einem beliebigen Ordner im Projekt speichern. Geben Sie der Datei ein `Build Action` von `Content`. Legen Sie die `Path` Eigenschaft `ResourceVideoSource` auf den Ordner und den Dateinamen, z. B. **MyFolder/MyVideo.mp4**. 

Die **VideoPlayerDemos.UWP** Projekt enthält einen Ordner namens **Videos** mit der Datei **UWPApiVideo.mp4**.

## <a name="loading-the-video-files"></a>Laden die Videodateien

Jede Plattform Renderingklassen enthält Code, in dessen `SetSource` Methode zum Laden von Videodateien, die als Ressourcen gespeichert.

### <a name="ios-resource-loading"></a>iOS-Ladeverfahren für Ressourcen

Die iOS-Version des `VideoPlayerRenderer` verwendet die `GetUrlForResource` Methode `NSBundle` für die Ressource zu laden. Der vollständige Pfad muss ein Dateiname, Erweiterung und Directory geteilt werden. Der Code verwendet die `Path` Klasse in .NET `System.IO` Namespace-URI für den Dateipfad in diese Komponenten unterteilen:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        void SetSource()
        {
            AVAsset asset = null;
            ···
            else if (Element.Source is ResourceVideoSource)
            {
                string path = (Element.Source as ResourceVideoSource).Path;

                if (!String.IsNullOrWhitespace(path))
                {
                    string directory = Path.GetDirectoryName(path);
                    string filename = Path.GetFileNameWithoutExtension(path);
                    string extension = Path.GetExtension(path).Substring(1);
                    NSUrl url = NSBundle.MainBundle.GetUrlForResource(filename, extension, directory);
                    asset = AVAsset.FromUrl(url);
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="android-resource-loading"></a>Laden von Android-Ressourcen

Die Android `VideoPlayerRenderer` verwendet den Dateinamen und die Paket-Namen zum Erstellen einer `Uri` Objekt. Der Paketname ist der Name der Anwendung, in diesem Fall **VideoPlayerDemos.Android**, die abgerufen werden kann, aus der statischen `Context.PackageName` Eigenschaft. Die resultierenden `Uri` Objekt wird dann zum Übergeben der `SetVideoURI` Methode `VideoView`:

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
            ···
            else if (Element.Source is ResourceVideoSource)
            {
                string package = Context.PackageName;
                string path = (Element.Source as ResourceVideoSource).Path;

                if (!String.IsNullOrWhiteSpace(path))
                {
                    string filename = Path.GetFileNameWithoutExtension(path).ToLowerInvariant();
                    string uri = "android.resource://" + package + "/raw/" + filename;
                    videoView.SetVideoURI(Android.Net.Uri.Parse(uri));
                    hasSetSource = true;
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="uwp-resource-loading"></a>Uwp-Ladeverfahren für Ressourcen

UWP `VideoPlayerRenderer` erstellt eine `Uri` Objekt für den Pfad und legt es auf die `Source` Eigenschaft `MediaElement`:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        async void SetSource()
        {
            bool hasSetSource = false;
            ···
            else if (Element.Source is ResourceVideoSource)
            {
                string path = "ms-appx:///" + (Element.Source as ResourceVideoSource).Path;

                if (!String.IsNullOrWhiteSpace(path))
                {
                    Control.Source = new Uri(path);
                    hasSetSource = true;
                }
            }
        }
        ···
    }
}
```

## <a name="playing-the-resource-file"></a>Wiedergabe der Ressourcendatei

Die **Videoressourcen wiedergeben** auf der Seite der **VideoPlayerDemos** Lösung verwendet die `OnPlatform` Klasse, um die Datei für jede Plattform anzugeben:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayVideoResourcePage"
             Title="Play Video Resource">
    <video:VideoPlayer>
        <video:VideoPlayer.Source>
            <video:ResourceVideoSource>
                <video:ResourceVideoSource.Path>
                    <OnPlatform x:TypeArguments="x:String">
                        <On Platform="iOS" Value="Videos/iOSApiVideo.mp4" />
                        <On Platform="Android" Value="AndroidApiVideo.mp4" />
                        <On Platform="UWP" Value="Videos/UWPApiVideo.mp4" />
                    </OnPlatform>
                </video:ResourceVideoSource.Path>
            </video:ResourceVideoSource>
        </video:VideoPlayer.Source>
    </video:VideoPlayer>
</ContentPage>
```

Wenn die iOS-Ressource in gespeichert ist die **Ressourcen** Ordner, und wenn die Ressource für die universelle Windows-Plattform im Stammordner des Projekts gespeichert wird, können Sie den gleichen Dateinamen verwenden, für die drei Plattformen. Wenn das der Fall ist, können Sie direkt mit diesem Namen Festlegen der `Source` Eigenschaft `VideoPlayer`. 

Hier wird die Seite, die auf drei Plattformen ausgeführt:

[![Wiedergeben von Videoressourcen](loading-resources-images/playvideoresource-small.png "Videoressourcen wiedergeben")](loading-resources-images/playvideoresource-large.png#lightbox "Videoressourcen wiedergeben")

Sie haben nun gesehen, wie [Laden von Videos aus einem URI Web](web-videos.md) und eingebettete Ressourcen wiedergeben. Darüber hinaus können Sie [Laden von Videos aus Videobibliothek des Geräts](accessing-library.md).


## <a name="related-links"></a>Verwandte Links

- [Video Player Demos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
