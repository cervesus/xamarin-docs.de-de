---
title: Zugreifen auf die Videobibliothek des Geräts
description: In diesem Artikel wird beschrieben, wie mithilfe von Xamarin.Forms in einer Videoplayeranwendung auf die Videobibliothek des Geräts zugegriffen werden kann.
ms.prod: xamarin
ms.assetid: 364C1D43-EAAE-45B9-BE24-0DA5AE74C4D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 00f1434a55fb815710bff26ac090a90bce5f41ee
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84197536"
---
# <a name="accessing-the-devices-video-library"></a>Zugreifen auf die Videobibliothek des Geräts

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)

Bei den meisten modernen mobilen Geräte und Desktop-Computern ist es möglich, mit der Kamera des Geräts Videos aufzunehmen. Die Videos, die ein Benutzer erstellt, werden dann als Dateien auf dem Gerät gespeichert. Diese Dateien können aus der Bildbibliothek abgerufen und durch die `VideoPlayer`-Klasse wie jedes andere Video abgespielt werden.

## <a name="the-photo-picker-dependency-service"></a>Der Fotoauswahl-Abhängigkeitsdienst

Alle Plattformen umfassen eine Funktion, die dem Benutzer die Auswahl eines Fotos oder Videos aus der Bildbibliothek des Geräts ermöglicht. Der erste Schritt zum Abspielen eines Videos aus der Bildbibliothek des Geräts ist die Erstellung eines Abhängigkeitsdiensts, der die Bildauswahl auf jeder Plattform aufruft. Der unten beschriebene Abhängigkeitsdienst ist dem im Artikel [**Picking a Photo from the Picture Library (Auswählen von Fotos aus der Bildbibliothek)** ](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md) definierten sehr ähnlich. Die Videoauswahl gibt statt eines `Stream`-Objekts jedoch einen Dateinamen zurück.

Das .NET Standard-Bibliotheksprojekt definiert eine Schnittstelle mit dem Namen `IVideoPicker` für den Abhängigkeitsdienst:

```csharp
namespace FormsVideoLibrary
{
    public interface IVideoPicker
    {
        Task<string> GetVideoFileAsync();
    }
}
```

Alle Plattformen enthalten eine Klasse mit dem Namen `VideoPicker`, die diese Schnittstelle implementiert.

### <a name="the-ios-video-picker"></a>Die iOS-Videoauswahl

Die iOS-`VideoPicker`-Klasse verwendet die iOS-[`UIImagePickerController`](xref:UIKit.UIImagePickerController)-Klasse, um auf die Bildbibliothek zuzugreifen, wobei angegeben wird, dass der Zugriff auf Videos (bezeichnet als „Filme“) in der iOS-`MediaType`-Eigenschaft begrenzt sein sollte. Beachten Sie, dass die `VideoPicker`-Klasse explizit die `IVideoPicker`-Schnittstelle implementiert. Beachten Sie außerdem das `Dependency`-Attribut, das diese Klasse als einen Abhängigkeitsdienst identifiziert. Dies sind die beiden Anforderungen, die erfüllt sein müssen, damit Xamarin.Forms den Abhängigkeitsdienst im Plattformprojekt ermitteln kann:

```csharp
using System;
using System.Threading.Tasks;
using UIKit;
using Xamarin.Forms;

[assembly: Dependency(typeof(FormsVideoLibrary.iOS.VideoPicker))]

namespace FormsVideoLibrary.iOS
{
    public class VideoPicker : IVideoPicker
    {
        TaskCompletionSource<string> taskCompletionSource;
        UIImagePickerController imagePicker;

        public Task<string> GetVideoFileAsync()
        {
            // Create and define UIImagePickerController
            imagePicker = new UIImagePickerController
            {
                SourceType = UIImagePickerControllerSourceType.SavedPhotosAlbum,
                MediaTypes = new string[] { "public.movie" }
            };

            // Set event handlers
            imagePicker.FinishedPickingMedia += OnImagePickerFinishedPickingMedia;
            imagePicker.Canceled += OnImagePickerCancelled;

            // Present UIImagePickerController;
            UIWindow window = UIApplication.SharedApplication.KeyWindow;
            var viewController = window.RootViewController;
            viewController.PresentViewController(imagePicker, true, null);

            // Return Task object
            taskCompletionSource = new TaskCompletionSource<string>();
            return taskCompletionSource.Task;
        }

        void OnImagePickerFinishedPickingMedia(object sender, UIImagePickerMediaPickedEventArgs args)
        {
            if (args.MediaType == "public.movie")
            {
                taskCompletionSource.SetResult(args.MediaUrl.AbsoluteString);
            }
            else
            {
                taskCompletionSource.SetResult(null);
            }
            imagePicker.DismissModalViewController(true);
        }

        void OnImagePickerCancelled(object sender, EventArgs args)
        {
            taskCompletionSource.SetResult(null);
            imagePicker.DismissModalViewController(true);
        }
    }
}
```

### <a name="the-android-video-picker"></a>Die Android-Videoauswahl

Die Android-Implementierung von `IVideoPicker` erfordert eine Rückrufmethode, die Teil der Anwendungsaktivität ist. Aus diesem Grund definiert die `MainActivity`-Klasse zwei Eigenschaften, eine Feld- und eine Rückrufmethode:

```csharp
namespace VideoPlayerDemos.Droid
{
    ···
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            Current = this;
            ···
        }

        // Field, properties, and method for Video Picker
        public static MainActivity Current { private set; get; }

        public static readonly int PickImageId = 1000;

        public TaskCompletionSource<string> PickImageTaskCompletionSource { set; get; }

        protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
        {
            base.OnActivityResult(requestCode, resultCode, data);

            if (requestCode == PickImageId)
            {
                if ((resultCode == Result.Ok) && (data != null))
                {
                    // Set the filename as the completion of the Task
                    PickImageTaskCompletionSource.SetResult(data.DataString);
                }
                else
                {
                    PickImageTaskCompletionSource.SetResult(null);
                }
            }
        }
    }
}
```

Die `OnCreate`-Methode in der `MainActivity`-Klasse speichert eine eigene Instanz in der statischen `Current`-Eigenschaft. Dies ermöglicht die Implementierung der `IVideoPicker`-Schnittstelle, damit die `MainActivity`-Instanz die **Select Video (Video auswählen)** -Auswahl startet:

```csharp
using System;
using System.Threading.Tasks;
using Android.Content;
using Xamarin.Forms;

// Need application's MainActivity
using VideoPlayerDemos.Droid;

[assembly: Dependency(typeof(FormsVideoLibrary.Droid.VideoPicker))]

namespace FormsVideoLibrary.Droid
{
    public class VideoPicker : IVideoPicker
    {
        public Task<string> GetVideoFileAsync()
        {
            // Define the Intent for getting images
            Intent intent = new Intent();
            intent.SetType("video/*");
            intent.SetAction(Intent.ActionGetContent);

            // Get the MainActivity instance
            MainActivity activity = MainActivity.Current;

            // Start the picture-picker activity (resumes in MainActivity.cs)
            activity.StartActivityForResult(
                Intent.CreateChooser(intent, "Select Video"),
                MainActivity.PickImageId);

            // Save the TaskCompletionSource object as a MainActivity property
            activity.PickImageTaskCompletionSource = new TaskCompletionSource<string>();

            // Return Task object
            return activity.PickImageTaskCompletionSource.Task;
        }
    }
}
```

Die Ergänzungen zum `MainActivity`-Objekt sind der einzige Code in der [**Video Player Demos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)-Projektmappe, während normaler Anwendungscode geändert werden muss, um die `FormsVideoLibrary`-Klassen zu unterstützen.

### <a name="the-uwp-video-picker"></a>Die UWP-Videoauswahl

Die UWP-Implementierung der `IVideoPicker`-Schnittstellen verwendet die UWP-[`FileOpenPicker`](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/)-Klasse. Sie beginnt die Dateisuche in der Bildbibliothek und begrenzt die Dateitypen auf MP4 und WMV (Windows Media Video):

```csharp
using System;
using System.Threading.Tasks;
using Windows.Storage;
using Windows.Storage.Pickers;
using Xamarin.Forms;

[assembly: Dependency(typeof(FormsVideoLibrary.UWP.VideoPicker))]

namespace FormsVideoLibrary.UWP
{
    public class VideoPicker : IVideoPicker
    {
        public async Task<string> GetVideoFileAsync()
        {
            // Create and initialize the FileOpenPicker
            FileOpenPicker openPicker = new FileOpenPicker
            {
                ViewMode = PickerViewMode.Thumbnail,
                SuggestedStartLocation = PickerLocationId.PicturesLibrary
            };

            openPicker.FileTypeFilter.Add(".wmv");
            openPicker.FileTypeFilter.Add(".mp4");

            // Get a file and return the path
            StorageFile storageFile = await openPicker.PickSingleFileAsync();
            return storageFile?.Path;
        }
    }
}
```

## <a name="invoking-the-dependency-service"></a>Aufrufen des Abhängigkeitsdiensts

Die Seite **Play Library Video** des [**VideoPlayerDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)-Programms zeigt, wie der Videoauswahl-Abhängigkeitsdienst verwendet wird. Die XAML-Datei enthält eine `VideoPlayer`-Instanz und einen `Button` mit der Bezeichnung **Videobibliothek anzeigen**:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayLibraryVideoPage"
             Title="Play Library Video">
    <StackLayout>
        <video:VideoPlayer x:Name="videoPlayer"
                           VerticalOptions="FillAndExpand" />

        <Button Text="Show Video Library"
                Margin="10"
                HorizontalOptions="Center"
                Clicked="OnShowVideoLibraryClicked" />
    </StackLayout>
</ContentPage>
```

Die CodeBehind-Datei enthält den `Clicked`-Handler für den `Button`. Das Aufrufen des Abhängigkeitsdiensts erfordert einen Aufruf an `DependencyService.Get`, um eine `IVideoPicker`-Schnittstelle im Plattformprojekt zu implementieren. Die `GetVideoFileAsync`-Methode wird dann für diese Instanz aufgerufen:

```csharp
namespace VideoPlayerDemos
{
    public partial class PlayLibraryVideoPage : ContentPage
    {
        public PlayLibraryVideoPage()
        {
            InitializeComponent();
        }

       async void OnShowVideoLibraryClicked(object sender, EventArgs args)
        {
            Button btn = (Button)sender;
            btn.IsEnabled = false;

            string filename = await DependencyService.Get<IVideoPicker>().GetVideoFileAsync();

            if (!String.IsNullOrWhiteSpace(filename))
            {
                videoPlayer.Source = new FileVideoSource
                {
                    File = filename
                };
            }

            btn.IsEnabled = true;
        }
    }
}
```

Der `Clicked`-Handler verwendet anschließend diesen Dateinamen, um ein `FileVideoSource`-Objekt zu erstellen und es auf die `Source`-Eigenschaft der `VideoPlayer`-Klasse festzulegen.

Jede der `VideoPlayerRenderer`-Klassen enthält Code in ihrer `SetSource`-Methode für Objekte des Typs `FileVideoSource`. Diese sind unten dargestellt:

### <a name="handling-ios-files"></a>Behandeln von iOS-Dateien

Die iOS-Version der `VideoPlayerRenderer`-Klasse verarbeitet `FileVideoSource`-Objekte mithilfe der statischen `Asset.FromUrl`-Methode mit dem Dateinamen. Diese erstellen ein `AVAsset`-Objekt, das die Datei in der Bildbibliothek des Geräts abbildet:

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
            else if (Element.Source is FileVideoSource)
            {
                string uri = (Element.Source as FileVideoSource).File;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    asset = AVAsset.FromUrl(new NSUrl(uri));
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="handling-android-files"></a>Behandeln von Android-Dateien

Wenn Objekte vom Typ `FileVideoSource` verarbeitet werden, wird für die Android-Implementierung der `VideoPlayerRenderer`-Klasse die `SetVideoPath`-Methode der `VideoView`-Klasse verwendet, um die Datei in der Bildbibliothek des Geräts anzugeben:

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
            else if (Element.Source is FileVideoSource)
            {
                string filename = (Element.Source as FileVideoSource).File;

                if (!String.IsNullOrWhiteSpace(filename))
                {
                    videoView.SetVideoPath(filename);
                    hasSetSource = true;
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="handling-uwp-files"></a>Behandeln von UWP-Dateien

Wenn Objekte vom Typ `FileVideoSource` behandelt werden, muss die UWP-Implementierung der `SetSource`-Methode ein `StorageFile`-Objekt erstellen, diese Datei zum Lesen öffnen, und das Datenstromobjekt an die `SetSource`-Methode des `MediaElement` übergeben:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
        async void SetSource()
        {
            bool hasSetSource = false;
            ···
            else if (Element.Source is FileVideoSource)
            {
                // Code requires Pictures Library in Package.appxmanifest Capabilities to be enabled
                string filename = (Element.Source as FileVideoSource).File;

                if (!String.IsNullOrWhiteSpace(filename))
                {
                    StorageFile storageFile = await StorageFile.GetFileFromPathAsync(filename);
                    IRandomAccessStreamWithContentType stream = await storageFile.OpenReadAsync();
                    Control.SetSource(stream, storageFile.ContentType);
                    hasSetSource = true;
                }
            }
            ···
        }
        ···
    }
}
```

Auf allen Plattformen wird das Videos fast unmittelbar nach dem Festlegen der Videoquelle abgespielt, da die Datei bereits auf dem Gerät vorhanden ist und nicht erst heruntergeladen werden muss.

## <a name="related-links"></a>Verwandte Links

- [Video Player Demos (Videoplayerdemos (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)
- [Auswählen eines Fotos aus der Bildbibliothek](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
