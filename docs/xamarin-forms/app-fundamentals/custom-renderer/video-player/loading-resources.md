---
title: Laden von anwendungsressourcenvideos
description: In diesem Artikel wird erläutert, wie beim Laden von Videos, die als Ressourcen in einer video-Player-Anwendung mithilfe von Xamarin.Forms Anwendung gespeichert wird.
ms.prod: xamarin
ms.assetid: F75BD540-9354-4C17-A119-57F3DEC66D54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 17e9e7061e4329431a0f34abdbbb616a1aff1b43
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2018
ms.locfileid: "52171325"
---
# <a name="loading-application-resource-videos"></a>Laden von anwendungsressourcenvideos

Die benutzerdefinierte Renderer für die `VideoPlayer` Ansicht sind in der Lage, der Wiedergabe von video-Dateien, die in die einzelnen Plattformprojekte als Anwendungsressourcen eingebettet wurden. Allerdings die aktuelle Version des `VideoPlayer` keinen Zugriff auf Ressourcen in einer .NET Standard-Bibliothek eingebettet.

Um diese Ressourcen zu laden, erstellen Sie eine Instanz des `ResourceVideoSource` durch Festlegen der `Path` Eigenschaft für den Dateinamen (oder den Ordner und Dateiname) der Ressource. Alternativ können Sie die statische Aufrufen `VideoSource.FromResource` Methode, um die Ressource verweisen. Legen Sie dann die `ResourceVideoSource` -Objekt an die `Source` Eigenschaft `VideoPlayer`.

## <a name="storing-the-video-files"></a>Speichern der video-Dateien

Speichern eine Videodatei in das plattformprojekt ist je nach Plattform unterschiedlich.

### <a name="ios-video-resources"></a>iOS-video-Ressourcen

Ein iOS-Projekt können Sie ein Video im Speichern der **Ressourcen** Ordner oder einen Unterordner des der **Ressourcen** Ordner. Die Datei benötigen eine `Build Action` von `BundleResource`. Legen Sie die `Path` Eigenschaft `ResourceVideoSource` an den Dateinamen, z. B. **MyFile.mp4** für eine Datei in die **Ressourcen** Ordner oder **MyFolder/MyFile.mp4**, wo **MyFolder** ist ein Unterordner des **Ressourcen**.

In der **VideoPlayerDemos** Lösung, die **VideoPlayerDemos.iOS** -Projekt enthält einen Unterordner des **Ressourcen** mit dem Namen **Videos** die Datei mit dem Namen **iOSApiVideo.mp4**. Dies ist ein kurzes Video an, das zeigt, wie Sie mit der Xamarin-Website finden Sie Dokumentation für das iOS `AVPlayerViewController` Klasse.

### <a name="android-video-resources"></a>Android-video-Ressourcen

In einem Android-Projekt Videos gespeichert werden müssen, in einem Unterordner des **Ressourcen** mit dem Namen **unformatierten**. Die **unformatierten** Ordner dürfen keine Unterordner enthalten. Geben Sie die Datei eine `Build Action` von `AndroidResource`. Legen Sie die `Path` Eigenschaft `ResourceVideoSource` an den Dateinamen, z. B. **MyFile.mp4**.

Die **VideoPlayerDemos.Android** -Projekt enthält einen Unterordner des **Ressourcen** mit dem Namen **unformatierten**, enthält eine Datei namens **AndroidApiVideo.mp4**.

### <a name="uwp-video-resources"></a>UWP-Videoressourcen

In einem Projekt auf die universelle Windows-Plattform können Sie Videos in einem Ordner in das Projekt speichern. Geben Sie der Datei eine `Build Action` von `Content`. Legen Sie die `Path` Eigenschaft `ResourceVideoSource` für den Ordner und Dateiname, z. B. **MyFolder/MyVideo.mp4**.

Die **VideoPlayerDemos.UWP** Projekt enthält einen Ordner namens **Videos** mit der Datei **UWPApiVideo.mp4**.

## <a name="loading-the-video-files"></a>Die video-Dateien

Jede Plattform Renderingklassen enthält Code in die `SetSource` Methode zum Laden von video-Dateien als Ressourcen gespeichert.

### <a name="ios-resource-loading"></a>Laden von iOS-Ressourcen

Die iOS-Version von `VideoPlayerRenderer` verwendet die `GetUrlForResource` -Methode der `NSBundle` für das Laden der Ressource. Der vollständige Pfad muss ein Dateiname, Erweiterung und Directory geteilt werden. Der Code verwendet die `Path` Klasse in .NET `System.IO` Namespace-URI für den Dateipfad in diese Komponenten unterteilen:

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

Die Android `VideoPlayerRenderer` wird der Dateiname und die Paket-Name verwendet wird, zum Erstellen einer `Uri` Objekt. Der Paketname ist der Name der Anwendung, die in diesem Fall **VideoPlayerDemos.Android**, die abgerufen werden kann, aus der statischen `Context.PackageName` Eigenschaft. Die resultierenden `Uri` Objekt wird dann zum Übergeben der `SetVideoURI` -Methode der `VideoView`:

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

### <a name="uwp-resource-loading"></a>Laden der UWP

Die UWP `VideoPlayerRenderer` erstellt eine `Uri` Objekt für den Pfad und legt es auf die `Source` Eigenschaft `MediaElement`:

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

## <a name="playing-the-resource-file"></a>Die Ressourcendatei spielen

Die **Video Ressource wiedergeben** auf der Seite die **VideoPlayerDemos** Lösung verwendet die `OnPlatform` Klasse, um die Datei für jede Plattform anzugeben:

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

Wenn in die iOS-Ressource gespeichert ist die **Ressourcen** Ordner, und wenn die UWP-Ressource im Stammordner des Projekts gespeichert ist, Sie können die gleiche Dateinamen für jede Plattform. Wenn das der Fall ist, können Sie direkt mit diesem Namen Festlegen der `Source` Eigenschaft `VideoPlayer`.

Hier ist diese Seite, die ausgeführt wird:

[![Wiedergabe von Video Ressource](loading-resources-images/playvideoresource-small.png "Wiedergabe von Video Ressource")](loading-resources-images/playvideoresource-large.png#lightbox "Wiedergabe von Video-Ressource")

Sie haben nun gesehen, wie Sie [Laden von Videos aus einem Web-URI](web-videos.md) und eingebettete Ressourcen zu spielen. Darüber hinaus können Sie [Laden von Videos aus Videobibliothek des Geräts](accessing-library.md).


## <a name="related-links"></a>Verwandte Links

- [Videodemos Player (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
