---
title: Laden von Anwendungsressourcenvideos
description: In diesem Artikel wird erläutert, wie Videos, die als Anwendungsressourcen gespeichert sind, mithilfe von Xamarin.Forms in einer Videoplayeranwendung geladen werden.
ms.prod: xamarin
ms.assetid: F75BD540-9354-4C17-A119-57F3DEC66D54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 4573d58f80b9c168f5d0a8a3f72beb64c29b1703
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70771836"
---
# <a name="loading-application-resource-videos"></a>Laden von Anwendungsressourcenvideos

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)

Die benutzerdefinierten Renderer der Ansicht `VideoPlayer` können Videodateien wiedergeben, die als Anwendungsressourcen in die einzelnen Plattformprojekte eingebettet wurden. Mit der aktuellen Version von `VideoPlayer` kann jedoch nicht auf Ressourcen zugegriffen werden, die in eine .NET Standard-Bibliothek eingebettet sind.

Erstellen Sie zum Laden dieser Ressourcen eine Instanz von `ResourceVideoSource`, indem Sie die Eigenschaft `Path` auf den Dateinamen (oder den Ordner und Dateinamen) der Ressource festlegen. Alternativ können Sie die statische Methode `VideoSource.FromResource` aufrufen, um auf die Ressource zu verweisen. Legen Sie anschließend für das Objekt `ResourceVideoSource` die Eigenschaft `Source` auf `VideoPlayer` fest.

## <a name="storing-the-video-files"></a>Speichern der Videodateien

Sie müssen beim Speicher einer Videodatei in das Plattformprojekt anders vorgehen als bei den einzelnen Plattformen.

### <a name="ios-video-resources"></a>iOS-Videoressourcen

In einem iOS-Projekt können Sie ein Video im Ordner **Ressourcen** oder in einem Unterordner des Ordners **Ressourcen** speichern. Die Videodatei muss bei `Build Action` den Wert `BundleResource` aufweisen. Legen Sie die `Path`-Eigenschaft `ResourceVideoSource` auf den Dateinamen fest, z. B. **MeineDatei.mp4** für eine Datei im Ordner **Ressourcen** oder **MeinOrdner/MeineDatei.mp4**, wobei **MeinOrdner** ein Unterordner von **Ressourcen** ist.

In der Projektmappe **Video Player Demos** enthält das Projekt **VideoPlayerDemos.iOS** einen Unterordner des Ordners **Ressourcen**, der den Namen **Videos** trägt und eine Datei namens **iOSApiVideo.mp4** enthält. Dies ist ein kurzes Video, in dem Ihnen gezeigt wird, wie Sie über die Xamarin-Website Dokumentationsartikel zur iOS-Klasse `AVPlayerViewController` finden.

### <a name="android-video-resources"></a>Android-Videoressourcen

In einem Android-Projekt müssen Videos in einem Unterordner des Ordners **Ressourcen** mit dem Namen **raw** (unformatiert) gespeichert werden. Der Ordner **raw** darf keine Unterordner enthalten. Ordnen Sie der Videodatei einen `Build Action` vom Typ `AndroidResource` zu. Legen Sie die `Path`-Eigenschaft von `ResourceVideoSource` auf den Dateinamen fest, z. B. **MeineDatei.mp4**.

Das Projekt **VideoPlayerDemos.Android** enthält einen Unterordner des Ordners **Ressourcen** mit dem Namen **unformatiert**, der eine Datei namens **AndroidApiVideo.mp4** enthält.

### <a name="uwp-video-resources"></a>UWP-Videoressourcen

Sie können in einem UWP-Projekt (Universelle Windows-Plattform) Videos in einem beliebigen Ordner im Projekt speichern. Ordnen Sie der Datei einen `Build Action` vom Typ `Content` zu. Legen Sie die `Path`-Eigenschaft von `ResourceVideoSource` auf den Ordner und Dateinamen fest, z. B. **MeinOrdner/MeinVideo.mp4**.

Das Projekt **VideoPlayerDemos.UWP** enthält einen Ordner namens **Videos** mit der Datei **UWPApiVideo.mp4**.

## <a name="loading-the-video-files"></a>Laden der Videodateien

Die einzelnen Rendererklassen der Plattform enthalten in der zugehörigen `SetSource`-Methode Code zum Laden von Videodateien, die als Ressourcen gespeichert sind.

### <a name="ios-resource-loading"></a>Laden der iOS-Ressourcen

Die iOS-Version von `VideoPlayerRenderer` verwendet die `GetUrlForResource`-Methode von `NSBundle` zum Laden der Ressource. Der vollständige Pfad muss in einen Dateinamen, eine Erweiterung und ein Verzeichnis unterteilt werden. Der Code verwendet die `Path`-Klasse im Namespace .NET `System.IO`, um den Dateipfad in folgende Komponenten zu unterteilen:

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

                if (!String.IsNullOrWhiteSpace(path))
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

### <a name="android-resource-loading"></a>Laden der Android-Ressourcen

In der Android-Version von `VideoPlayerRenderer` werden zum Erstellen eines `Uri`-Objekts der Dateiname und der Paketname verwendet. Bei dem Paketnamen handelt es sich um den Namen der Anwendung, in diesem Fall **VideoPlayerDemos.Android**, die über die statische `Context.PackageName`-Eigenschaft abgerufen werden kann. Das resultierende `Uri`-Objekt wird anschließend an die `SetVideoURI`-Methode `VideoView` übergeben:

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

### <a name="uwp-resource-loading"></a>Laden der UPW-Ressourcen

In der UWP-Version von `VideoPlayerRenderer` wird ein `Uri`-Objekt für den Pfad erstellt und auf die `Source`-Eigenschaft `MediaElement` festgelegt:

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

## <a name="playing-the-resource-file"></a>Wiedergeben der Ressourcendatei

Auf der Seite **Play Video Resource** (Videoressource wiedergeben) in der Projektmappe **VideoPlayerDemos** wird die `OnPlatform`-Klasse für die Angabe der Videodatei für die einzelnen Plattformen verwendet:

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

Wenn die iOS-Ressource im Ordner **Ressourcen** und die UWP-Ressource im Stammordner des Projekts gespeichert ist, können Sie für die einzelnen Plattformen den gleichen Dateinamen verwenden. In diesem Fall können Sie diesen Namen direkt auf die `Source`-Eigenschaft von `VideoPlayer` festlegen.

Dies ist die Seite, die ausgeführt wird:

[![Videoressource wiedergeben](loading-resources-images/playvideoresource-small.png "Play Video Resource")](loading-resources-images/playvideoresource-large.png#lightbox "Play Video Resource")

Sie haben nun die Vorgehensweise zum [Laden von Videos aus einem Web-URI](web-videos.md) und zum Wiedergeben eingebetteter Ressourcen kennengelernt. Darüber hinaus können Sie [Videos aus der Videobibliothek des Geräts laden](accessing-library.md).

## <a name="related-links"></a>Verwandte Links

- [Video Player Demos (Videoplayerdemos (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)
