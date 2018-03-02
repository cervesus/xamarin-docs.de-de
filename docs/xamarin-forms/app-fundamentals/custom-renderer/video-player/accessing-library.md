---
title: "Zugreifen auf die Videobibliothek des Geräts"
ms.topic: article
ms.prod: xamarin
ms.assetid: 364C1D43-EAAE-45B9-BE24-0DA5AE74C4D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 95f992c2f3ca976c1e02ff3043ceef0a4a81e4f6
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="accessing-the-devices-video-library"></a>Zugreifen auf die Videobibliothek des Geräts

Die meisten modernen mobilen Geräten und desktop-PCs haben die Möglichkeit zum Aufzeichnen von Videos über die Kamera des Geräts. Die Videos, die ein Benutzer erstellt werden dann als Dateien auf dem Gerät gespeichert. Diese Dateien können aus der Image-Bibliothek abgerufen und wiedergegeben werden, indem die `VideoPlayer` Klasse wie jede andere Video.

## <a name="the-photo-picker-dependency-service"></a>Das Foto Datumsauswahl Abhängigkeitsdienst

Alle drei Plattformen umfasst eine Funktion, die dem Benutzer des Geräts Bildbibliothek eines Fotos oder Videos aus ermöglicht. Der erste Schritt bei der Wiedergabe eines Videos aus des Geräts Bildbibliothek wird eine Abhängigkeitsdienst erstellt, die die Auswahl einer Image auf jeder Plattform aufruft. Die unten beschriebene Abhängigkeitsdienst ist sehr ähnlich definiert, der [ **Kommissionieren eines Fotos aus der Bildbibliothek** ](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md) Artikel, außer dass die Auswahl einer video einen Dateinamen statt einer gibt`Stream`Objekt.

PCL-Projekt definiert eine Schnittstelle, die mit dem Namen `IVideoPicker` für den Abhängigkeitsdienst:

```csharp
namespace FormsVideoLibrary
{
    public interface IVideoPicker
    {
        Task<string> GetVideoFileAsync();
    }
}
```

Alle drei Plattformen enthält eine Klasse namens `VideoPicker` , das diese Schnittstelle implementiert.

### <a name="the-ios-video-picker"></a>Die iOS-video-Auswahl

IOS `VideoPicker` verwendet iOS [ `UIImagePickerController` ](https://developer.xamarin.com/api/type/UIKit.UIImagePickerController/) auf die Bildbibliothek gibt an, dass er auf Videos (bezeichnet als "Filme") in der iOS-beschränkt sein soll `MediaType` Eigenschaft. Beachten Sie, dass `VideoPicker` implementiert ausdrücklich die `IVideoPicker` Schnittstelle. Beachten Sie außerdem die `Dependency` Attribut, das diese Klasse als eine Abhängigkeitsdienst identifiziert. Dies sind die beiden Anforderungen, mit die Sie Xamarin.Forms, um den Abhängigkeitsdienst im plattformprojekt finden können:

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

### <a name="the-android-video-picker"></a>Die Auswahl einer Android video

Die Android-Implementierung von `IVideoPicker` erfordert eine Rückrufmethode, die Teil der Aktivität der Anwendung ist. Aus diesem Grund die `MainActivity` Klasse definiert zwei Eigenschaften, die ein Feld und eine Rückrufmethode:

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

Die `OnCreate` Methode im `MainActivity` speichert eine eigene Instanz in der statischen `Current` Eigenschaft. Dadurch wird die Implementierung der `IVideoPicker` zum Abrufen der `MainActivity` Instanz für das Starten der **Video auswählen** Auswahl:

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

Virtual Machine Additions auf die `MainActivity` Objekt sind der einzige Code in der [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) Lösung, in denen normale Anwendungscode geändert werden, damit Sie unterstützen muss, die `FormsVideoLibrary` Klassen.

### <a name="the-uwp-video-picker"></a>Die Auswahl einer uwp-video

Die uwp-Implementierung der `IVideoPicker` -Schnittstelle verwendet, die auf UWP [ `FileOpenPicker` ](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/). Es beginnt die Dateisuche mit der Bibliothek "Bilder", und schränkt die Dateitypen MP4 und WMV (Windows Media Video):

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

## <a name="invoking-the-dependency-service"></a>Aufrufen des Diensts zur Abhängigkeitseigenschaft

Die **Bibliothek Video abspielen** auf der Seite der [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) Programm veranschaulicht, wie die Auswahl einer video Abhängigkeitsdienst. Die Verwendung von XAML-Datei enthält eine `VideoPlayer` Instanz und ein `Button` mit der Bezeichnung **Videobibliothek anzeigen**:

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

Die Code-Behind-Datei enthält die `Clicked` Handler für das `Button`. Aufrufen des Diensts Abhängigkeit erfordert einen Aufruf von `DependencyService.Get` die Implementierung der abzurufenden ein `IVideoPicker` Schnittstelle im plattformprojekt. Die `GetVideoFileAsync` Methode klicken Sie dann diese Instanz aufgerufen wird:

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

Die `Clicked` Handler verwendet diesen Dateinamen zum Erstellen einer `FileVideoSource` Objekt und zum Festlegen der `Source` Eigenschaft der `VideoPlayer`.

Jede der `VideoPlayerRenderer` Klassen enthält Code, in dessen `SetSource` Methode für Objekte vom Typ `FileVideoSource`. Diese sind im folgenden dargestellt:

### <a name="handling-ios-files"></a>Behandlung von iOS-Dateien

Die iOS-Version des `VideoPlayerRenderer` Prozesse `FileVideoSource` Objekte mithilfe der statischen `Asset.FromUrl` Methode mit dem Dateinamen. Dies erstellt ein `AVAsset` Objekt, das die Datei in das Gerät Bildbibliothek darstellt:

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

Beim Verarbeiten von Objekten des Typs `FileVideoSource`, Android-Implementierung des `VideoPlayerRenderer` verwendet die `SetVideoPath` Methode `VideoView` um die Datei in der Bildbibliothek des Geräts anzugeben:

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

### <a name="handling-uwp-files"></a>Behandlung von uwp-Dateien

Beim Verarbeiten der Objekte vom Typ `FileVideoSource`, die uwp-Implementierung von der `SetSource` -Methode muss zum Erstellen einer `StorageFile` Objekt, öffnen Sie die Datei zum Lesen und übergeben Sie das Streamobjekt, der `SetSource` Methode der `MediaElement`:

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

Für alle drei Plattformen des Videos wird fast unmittelbar nach dem Video wiedergegeben Quelle festgelegt ist, da die Datei auf dem Gerät ist und nicht heruntergeladen werden muss.



## <a name="related-links"></a>Verwandte Links

- [Video Player Demos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
- [Kommissionieren eines Fotos aus der Bildbibliothek](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
