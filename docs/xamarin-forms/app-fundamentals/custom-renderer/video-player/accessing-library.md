---
title: Zugreifen auf die Videobibliothek des Geräts
description: In diesem Artikel wird erläutert, wie auf die Videobibliothek des Geräts, in einer video-Player-Anwendung, die mit Xamarin.Forms wird.
ms.prod: xamarin
ms.assetid: 364C1D43-EAAE-45B9-BE24-0DA5AE74C4D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 619469e4c4fd3901491c20d6215ec0a25c49f69d
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2018
ms.locfileid: "52171182"
---
# <a name="accessing-the-devices-video-library"></a>Zugreifen auf die Videobibliothek des Geräts

Die meisten modernen mobilen Geräten und desktop-PCs haben die Möglichkeit zum Aufzeichnen von Videos mit der Gerätekamera. Die Videos, die ein Benutzer erstellt werden dann als Dateien auf dem Gerät gespeichert. Diese Dateien können aus der Abbildbibliothek abgerufen und dazu die `VideoPlayer` Klasse genau wie alle anderen Videos.

## <a name="the-photo-picker-dependency-service"></a>Die Foto-Auswahl Abhängigkeitsdienst

Alle Plattformen umfasst eine Funktion, die Benutzer die Auswahl eines Fotos oder Videos aus der Abbildbibliothek des Geräts ermöglicht. Der erste Schritt bei der Wiedergabe eines Videos aus der Abbildbibliothek des Geräts ist ein Abhängigkeitsdienst erstellen, die die Auswahl Bild auf jeder Plattform aufgerufen. Die unten beschriebene Abhängigkeitsdienst ist sehr ähnlich definiert, der [ **Auswählen eines Fotos aus der Bildbibliothek** ](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md) Artikel mit dem Unterschied, dass die video-Auswahl einen Dateinamen statt einer zurückgibt`Stream`Objekt.

.NET Standard Library-Projekt definiert eine Schnittstelle, die mit dem Namen `IVideoPicker` für den Dependency-Dienst:

```csharp
namespace FormsVideoLibrary
{
    public interface IVideoPicker
    {
        Task<string> GetVideoFileAsync();
    }
}
```

Alle Plattformen enthält eine Klasse namens `VideoPicker` , das diese Schnittstelle implementiert.

### <a name="the-ios-video-picker"></a>Die iOS-video-Auswahl

IOS `VideoPicker` verwendet das iOS [ `UIImagePickerController` ](https://developer.xamarin.com/api/type/UIKit.UIImagePickerController/) auf der Abbildbibliothek, die angeben, beschränkt auf Videos (als "Movies" bezeichnet) in der iOS- `MediaType` Eigenschaft. Beachten Sie, dass `VideoPicker` explizit implementiert die `IVideoPicker` Schnittstelle. Beachten Sie auch die `Dependency` Attribut, das diese Klasse als Abhängigkeitsdienst identifiziert. Dies sind die beiden Anforderungen, mit denen Sie Xamarin.Forms, um den Abhängigkeitsdienst in die plattformprojekt zu ermitteln:

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
            viewController.PresentModalViewController(imagePicker, true);

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

### <a name="the-android-video-picker"></a>Die Android-video-Auswahl

Die Android-Implementierung von `IVideoPicker` erfordert eine Rückrufmethode, die Teil der Aktivität der Anwendung ist. Aus diesem Grund die `MainActivity` -Klasse definiert zwei Eigenschaften, ein Feld und eine Callback-Methode:

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

Die `OnCreate` -Methode in der `MainActivity` speichert eine eigene Instanz in der statischen `Current` Eigenschaft. Dies ermöglicht die Implementierung der `IVideoPicker` zum Abrufen der `MainActivity` Instanz zum Starten der **Video auswählen** Auswahl:

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

Die Ergänzungen der `MainActivity` Objekt sind der einzige Code im der [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) Lösung, in denen normale Anwendungscode werden, um die Unterstützung geändert muss, der `FormsVideoLibrary` Klassen.

### <a name="the-uwp-video-picker"></a>Die UWP-video-Auswahl

Die UWP-Implementierung, der die `IVideoPicker` -Schnittstelle verwendet, die UWP [ `FileOpenPicker` ](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/). Es beginnt die Dateisuche mit der Bibliothek "Bilder", und schränkt die Dateitypen, MP4 und WMV (Windows Media Video):

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

## <a name="invoking-the-dependency-service"></a>Aufrufen des Diensts Abhängigkeit

Die **-Bibliothek-Video wiedergeben** auf der Seite die [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) Programm veranschaulicht, wie die video-Auswahl Abhängigkeitsdienst. Die XAML-Datei enthält eine `VideoPlayer` Instanz und ein `Button` mit der Bezeichnung **Videobibliothek anzeigen**:

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

Die CodeBehind-Datei enthält die `Clicked` Handler für die `Button`. Aufrufen des Diensts Abhängigkeit erfordert einen Aufruf `DependencyService.Get` die Implementierung der abzurufenden ein `IVideoPicker` -Schnittstelle im plattformprojekt. Die `GetVideoFileAsync` Methode klicken Sie dann für diese Instanz aufgerufen wird:

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

Die `Clicked` Handler verwendet diesen Dateinamen zum Erstellen einer `FileVideoSource` Objekt und festgelegt ist, dass die `Source` Eigenschaft der `VideoPlayer`.

Jede der `VideoPlayerRenderer` Klassen enthält Code in die `SetSource` -Methode für Objekte des Typs `FileVideoSource`. Diese sind unten dargestellt:

### <a name="handling-ios-files"></a>Behandlung von iOS-Dateien

Die iOS-Version von `VideoPlayerRenderer` Prozesse `FileVideoSource` Objekte mithilfe der statischen `Asset.FromUrl` Methode mit dem Dateinamen. Dies erstellt eine `AVAsset` Objekt, das die Datei in der Bildbibliothek des Geräts:

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

### <a name="handling-android-files"></a>Behandlung von Android-Dateien

Beim Verarbeiten von Objekten des Typs `FileVideoSource`, die Android-Implementierung von `VideoPlayerRenderer` verwendet die `SetVideoPath` -Methode der `VideoView` um die Datei in der Bildbibliothek des Geräts anzugeben:

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

### <a name="handling-uwp-files"></a>Behandlung von UWP-Dateien

Beim Verarbeiten der Objekte des Typs `FileVideoSource`, die UWP-Implementierung von der `SetSource` Methode muss zum Erstellen einer `StorageFile` -Objekt, öffnen Sie diese Datei zum Lesen und übergeben Sie das Streamobjekt, das die `SetSource` -Methode der der `MediaElement`:

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

Für jede Plattform, das Video wird wiedergegeben, die fast unmittelbar auf das Video Quelle festgelegt ist, da die Datei auf dem Gerät ist und nicht heruntergeladen werden muss.



## <a name="related-links"></a>Verwandte Links

- [Videodemos Player (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
- [Wählen ein Foto aus der Bildbibliothek](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
