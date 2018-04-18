---
title: Kommissionieren eines Fotos aus der Bildbibliothek
description: Verwenden Sie DependencyService, um ein Foto des Telefons Bildbibliothek auswählen
ms.prod: xamarin
ms.assetid: 4F51B0E7-6A63-403C-B488-500CCBCE75DD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/06/2017
ms.openlocfilehash: 95ac9912f0ff6788a2a633b3f8d3495e286030f1
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
---
# <a name="picking-a-photo-from-the-picture-library"></a>Kommissionieren eines Fotos aus der Bildbibliothek

Dieser Artikel führt durch die Erstellung einer Anwendung, die dem Benutzer ermöglicht, die ein Foto des Telefons Bildbibliothek auswählen. Da Xamarin.Forms nicht diese Funktion enthält, ist es notwendig, verwenden [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) auf systemeigene APIs auf jeder Plattform zugreifen.  Dieser Artikel befasst sich mit der folgenden Schritte aus, für die Verwendung von `DependencyService` für diese Aufgabe:

- **[Zum Erstellen der Schnittstelle](#Creating_the_Interface)**  &ndash; zu verstehen, wie die Schnittstelle im gemeinsamen Code erstellt wird.
- **[iOS Implementierung](#iOS_Implementation)**  &ndash; erfahren Sie, wie die Schnittstelle in systemeigenem Code für iOS zu implementieren.
- **[Android-Implementierung](#Android_Implementation)**  &ndash; erfahren Sie, wie die Schnittstelle für Android in systemeigenem Code zu implementieren.
- **[Universelle Windows-Plattform-Implementierung](#UWP_Implementation)**  &ndash; erfahren Sie, wie die Schnittstelle in systemeigenem Code für die universelle Windows-Plattform (UWP) zu implementieren.
- **[Windows Phone-Implementierung](#Windows_Phone_Implementation)**  &ndash; erfahren Sie, wie die Schnittstelle für Windows Phone 8.1 in systemeigenem Code zu implementieren.
- **[Implementieren im freigegebenen Code](#Implementing_in_Shared_Code)**  &ndash; erfahren, wie `DependencyService` in die systemeigene Implementierung von freigegebenem Code aufrufen.

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Erstellen der Schnittstelle

Erstellen Sie zunächst eine Schnittstelle im freigegebenen Code, der die gewünschte Funktionalität ausdrückt. Im Fall einer Anwendung Foto Kommissionierung ist nur eine Methode erforderlich. Dies wird definiert, der [ `IPicturePicker` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/DependencyServiceSample/IPicturePicker.cs) -Schnittstelle in der portablen Klassenbibliothek des Beispielcodes:

```csharp
namespace DependencyServiceSample
{
    public interface IPicturePicker
    {
        Task<Stream> GetImageStreamAsync();
    }
}
```

Die `GetImageStreamAsync` Methode als asynchroner definiert ist, da die Methode schnell zurückgeben muss, aber es kann nicht zurückgegeben werden, eine `Stream` Objekt für das ausgewählte Foto, bis der Benutzer die Bildbibliothek durchsucht und eines ausgewählt hat.

Diese Schnittstelle wird auf allen Plattformen, die mit plattformspezifischen Code implementiert.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS-Implementierung

Die iOS-Implementierung von der `IPicturePicker` Benutzeroberfläche verwendet die [ `UIImagePickerController` ](https://developer.xamarin.com/api/type/UIKit.UIImagePickerController/) wie beschrieben in der [ **Foto aus dem Katalog auswählen** ](https://developer.xamarin.com/recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery/) Rezept und [Beispielcode](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery).

Die iOS-Implementierung befindet sich der [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/iOS/PicturePickerImplementation.cs) Abfrageklasse der Beispielcode im iOS-Projekt. Auf diese Klasse sichtbar zu machen die `DependencyService` -Manager die Klasse muss angegeben werden mit einer [`assembly`] Attribut des Typs `Dependency`, und die Klasse muss öffentlich sein und Explizites Implementieren von der `IPicturePicker` Schnittstelle:

```csharp
[assembly: Dependency (typeof (PicturePickerImplementation))]

namespace DependencyServiceSample.iOS
{
    public class PicturePickerImplementation : IPicturePicker
    {
        TaskCompletionSource<Stream> taskCompletionSource;
        UIImagePickerController imagePicker;

        public Task<Stream> GetImageStreamAsync()
        {
            // Create and define UIImagePickerController
            imagePicker = new UIImagePickerController
            {
                SourceType = UIImagePickerControllerSourceType.PhotoLibrary,
                MediaTypes = UIImagePickerController.AvailableMediaTypes(UIImagePickerControllerSourceType.PhotoLibrary)
            };

            // Set event handlers
            imagePicker.FinishedPickingMedia += OnImagePickerFinishedPickingMedia;
            imagePicker.Canceled += OnImagePickerCancelled;

            // Present UIImagePickerController;
            UIWindow window = UIApplication.SharedApplication.KeyWindow;
            var viewController = window.RootViewController;
            viewController.PresentModalViewController(imagePicker, true);

            // Return Task object
            taskCompletionSource = new TaskCompletionSource<Stream>();
            return taskCompletionSource.Task;
        }
        ...
    }
}

```

Die `GetImageStreamAsync` Methode erstellt eine `UIImagePickerController` und initialisiert dieses, um Bilder aus der Fotobibliothek auszuwählen. Zwei Ereignishandler sind erforderlich: eine für, wenn der Benutzer auswählt, ein Foto und die andere für den Abbruch des Benutzers auf der Anzeige der Fotobibliothek. Die `PresentModalViewController` zeigt dann die Fotobibliothek für den Benutzer.

An diesem Punkt der `GetImageStreamAsync` Methodenrückgabewert muss ein `Task<Stream>` Objekt, das den Code, der es aufgerufen wird. Dieser Vorgang ist abgeschlossen, nur, wenn der Benutzer die Interaktion mit der Fotobibliothek abgeschlossen hat und eine der Ereignishandler wird aufgerufen. Für Situationen, die [ `TaskCompletionSource` ](https://msdn.microsoft.com/library/dd449174(v=vs.110).aspx) Klasse ist wichtig. Die Klasse bietet einen `Task` Objekt mit dem richtigen generischen Typ zurückzugebenden aus der `GetImageStreamAsync` -Methode und die Klasse kann später signalisiert, wenn der Task abgeschlossen ist.

Die `FinishedPickingMedia` -Ereignishandler wird aufgerufen, wenn der Benutzer ein Bild ausgewählt hat. Der Handler bietet jedoch eine `UIImage` Objekt und die `Task` muss .NET zurückgeben `Stream` Objekt. Dies erfolgt in zwei Schritten: die `UIImage` -Objekt konvertiert wird zuerst in eine JPEG-Datei im Arbeitsspeicher gespeichert, ein `NSData` -Objekt, und klicken Sie dann die `NSData` Objekt wird in einer .NET konvertiert `Stream` Objekt. Ein Aufruf der `SetResult` Methode der `TaskComkpletionSource` Objekt führt die Aufgabe durch die Bereitstellung der `Stream` Objekt:

```csharp
namespace DependencyServiceSample.iOS
{
    public class PicturePickerImplementation : IPicturePicker
    {
        TaskCompletionSource<Stream> taskCompletionSource;
        UIImagePickerController imagePicker;
        ...
        void OnImagePickerFinishedPickingMedia(object sender, UIImagePickerMediaPickedEventArgs args)
        {
            UIImage image = args.EditedImage ?? args.OriginalImage;

            if (image != null)
            {
                // Convert UIImage to .NET Stream object
                NSData data = image.AsJPEG(1);
                Stream stream = data.AsStream();

                // Set the Stream as the completion of the Task
                taskCompletionSource.SetResult(stream);
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

Eine iOS-Anwendung erfordert die Berechtigung vom Benutzer zum Zugriff auf dem Telefon Foto-Bibliothek. Fügen Sie Folgendes an der `dict` Abschnitt der Datei "Info.plist":

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>Picture Picker uses photo library</string>
```


<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android-Implementierung

Die Android-Implementierung verwendet die beschriebene Technik für die [ **wählen Sie ein Image** ](https://developer.xamarin.com/recipes/android/other_ux/pick_image/) Rezept und [Beispielcode](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image). Allerdings ist die Methode, die aufgerufen wird, wenn der Benutzer ein Bild aus der Bildbibliothek ausgewählt hat eine `OnActivityResult` außer Kraft setzen in einer Klasse, die abgeleitet `Activity`. Aus diesem Grund der normalen [ `MainActivity` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/MainActivity.cs) Klasse im Android-Projekt ist mit einem Feld, eine Eigenschaft und eine Überschreibung der ergänzt wurden die `OnActivityResult` Methode:

```csharp
public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
{
    ...
    // Field, property, and method for Picture Picker
    public static readonly int PickImageId = 1000;

    public TaskCompletionSource<Stream> PickImageTaskCompletionSource { set; get; }

    protected override void OnActivityResult(int requestCode, Result resultCode, Intent intent)
    {
        base.OnActivityResult(requestCode, resultCode, intent);

        if (requestCode == PickImageId)
        {
            if ((resultCode == Result.Ok) && (intent != null))
            {
                Android.Net.Uri uri = intent.Data;
                Stream stream = ContentResolver.OpenInputStream(uri);

                // Set the Stream as the completion of the Task
                PickImageTaskCompletionSource.SetResult(stream);
            }
            else
            {
                PickImageTaskCompletionSource.SetResult(null);
            }
        }
    }
}

```

Die `OnActivityResult`Außerkraftsetzung gibt an, das ausgewählte Bild-Datei mit einem Android `Uri` -Objekt, aber dies kann in einer .NET konvertiert werden `Stream` Objekt durch Aufrufen der `OpenInputStream` Methode der `ContentResolver` -Objekt, das erhaltene der Aktivität `ContentResolver` Eigenschaft.

Wie bei der iOS-Implementierung, die Android-Implementierung verwendet ein `TaskCompletionSource` zu signalisieren, wenn die Aufgabe abgeschlossen wurde. Dies `TaskCompletionSource` Objekt ist definiert als eine öffentliche Eigenschaft in der `MainActivity` Klasse. Dadurch wird die Eigenschaft in Verweise auf die [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/PicturePickerImplementation.cs) Klasse im Android-Projekt. Dies ist die Klasse mit dem `GetImageStreamAsync` Methode:

```csharp
[assembly: Dependency(typeof(PicturePickerImplementation))]

namespace DependencyServiceSample.Droid
{
    public class PicturePickerImplementation : IPicturePicker
    {
        public Task<Stream> GetImageStreamAsync()
        {
            // Define the Intent for getting images
            Intent intent = new Intent();
            intent.SetType("image/*");
            intent.SetAction(Intent.ActionGetContent);

            // Start the picture-picker activity (resumes in MainActivity.cs)
            MainActivity.Instance.StartActivityForResult(
                Intent.CreateChooser(intent, "Select Picture"),
                MainActivity.PickImageId);

            // Save the TaskCompletionSource object as a MainActivity property
            MainActivity.Instance.PickImageTaskCompletionSource = new TaskCompletionSource<Stream>();

            // Return Task object
            return MainActivity.Instance.PickImageTaskCompletionSource.Task;
        }
    }
}
```

Diese Methode greift auf die `MainActivity` Klasse für mehrere Zwecke: für die `Instance` -Eigenschaft, für die `PickImageId` Feld für die `TaskCompletionSource` -Eigenschaft, und zum Aufrufen `StartActivityForResult`. Diese Methode wird von definiert die `FormsApplicationActivity` Klasse, die die Basisklasse von `MainActivity`.

<a name="UWP_Implementation" />

## <a name="uwp-implementation"></a>Uwp-Implementierung

Im Gegensatz zu IOS- und Android-Implementierungen, die Implementierung von der Auswahl einer Foto für die universelle Windows-Plattform erfordert keine der `TaskCompletionSource` Klasse. Die [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/UWP/PicturePickerImplementation.cs) -Klasse verwendet die [ `FileOpenPicker` ](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/) Klasse zum Zugriff auf die Fotobibliothek. Da die `PickSingleFileAsync` Methode `FileOpenPicker` ist selbst asynchron, der `GetImageStreamAsync` Methode können einfach `await` mit dieser Methode (und anderen asynchronen Methoden) und Zurückgeben einer `Stream` Objekt:


```csharp
[assembly: Dependency(typeof(PicturePickerImplementation))]

namespace DependencyServiceSample.UWP
{
    public class PicturePickerImplementation : IPicturePicker
    {
        public async Task<Stream> GetImageStreamAsync()
        {
            // Create and initialize the FileOpenPicker
            FileOpenPicker openPicker = new FileOpenPicker
            {
                ViewMode = PickerViewMode.Thumbnail,
                SuggestedStartLocation = PickerLocationId.PicturesLibrary,
            };

            openPicker.FileTypeFilter.Add(".jpg");
            openPicker.FileTypeFilter.Add(".jpeg");
            openPicker.FileTypeFilter.Add(".png");

            // Get a file and return a Stream
            StorageFile storageFile = await openPicker.PickSingleFileAsync();

            if (storageFile == null)
            {
                return null;
            }

            IRandomAccessStreamWithContentType raStream = await storageFile.OpenReadAsync();
            return raStream.AsStreamForRead();
        }
    }
}
```

<a name="Windows_Phone_Implementation" />

## <a name="windows-phone-81-implementation"></a>Windows Phone 8.1-Implementierung

Die Windows Phone 8.1-Implementierung ähnelt die uwp-Implementierung mit Ausnahme von einem Belang, die eine erhebliche Auswirkungen hat: die `PickSingleFileAsync` Methode `FileOpenPicker` ist nicht verfügbar. Stattdessen die `GetImageStreamAsync` -Methodenaufruf müssen `PickSingleFileAndContinue`. Diese Methode zurückgegeben, wenn die Fotobibliothek für den Benutzer angezeigt wird, aber die Auswahl des Benutzers eines Fotos, durch einen Aufruf signalisiert wird der `OnActivated` Methode der `App` Klasse.

Aus diesem Grund die [ `App` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/WinPhone/App.xaml.cs) Klasse erstellt, indem die Xamarin.Forms-Projektvorlage in das Windows Phone 8.1-Projekt wurde mit einer Eigenschaft vom Typ ergänzt wurden `TaskCompletionSource` und eine Überschreibung der `OnActivated` Methode:

```csharp
namespace DependencyServiceSample.WinPhone
{
    public sealed partial class App : Application
    {
        ...
        public TaskCompletionSource<Stream> TaskCompletionSource { set; get; }

        protected async override void OnActivated(IActivatedEventArgs args)
        {
            base.OnActivated(args);

            IContinuationActivatedEventArgs continuationArgs = args as IContinuationActivatedEventArgs;

            if (continuationArgs != null &&
                continuationArgs.Kind == ActivationKind.PickFileContinuation)
            {
                FileOpenPickerContinuationEventArgs pickerArgs = args as FileOpenPickerContinuationEventArgs;

                if (pickerArgs.Files.Count > 0)
                {
                    // Get the file and a Stream
                    StorageFile storageFile = pickerArgs.Files[0];
                    IRandomAccessStreamWithContentType raStream = await storageFile.OpenReadAsync();
                    Stream stream = raStream.AsStreamForRead();

                    // Set the completion of the Task
                    TaskCompletionSource.SetResult(stream);
                }
                else
                {
                    TaskCompletionSource.SetResult(null);
                }
            }
        }
    }
}
```

Die `OnActivated` Methode kann mehrere Ursachen haben, z. B. Start der Anwendung aufgerufen werden. Schränkt der Code selbst auf nur diese Aufrufe, bei der Auswahl einer geöffneten Datei wurde beendet, und klicken Sie dann erhält ein `Stream` -Objekt aus der `StorageFile`.

Die [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/WinPhone/PicturePickerImplementation.cs) Klasse enthält die `GetImageStreamAsync` -Methode, erstellt der `FileOpenPicker` und Aufrufe `PickSingleFileAndContainue`:

```csharp
[assembly: Xamarin.Forms.Dependency(typeof(PicturePickerImplementation))]

namespace DependencyServiceSample.WinPhone
{
    public class PicturePickerImplementation : IPicturePicker
    {
        public Task<Stream> GetImageStreamAsync()
        {
            // Create and initialize the FileOpenPicker
            FileOpenPicker openPicker = new FileOpenPicker
            {
                ViewMode = PickerViewMode.Thumbnail,
                SuggestedStartLocation = PickerLocationId.PicturesLibrary,
            };

            openPicker.FileTypeFilter.Add(".jpg");
            openPicker.FileTypeFilter.Add(".jpeg");
            openPicker.FileTypeFilter.Add(".png");

            // Display the picker for a single file (resumes in OnActivated in App.xaml.cs)
            openPicker.PickSingleFileAndContinue();

            // Create a TaskCompletionSource stored in App.xaml.cs
            TaskCompletionSource<Stream> taskCompletionSource = new TaskCompletionSource<Stream>();
            (Application.Current as App).TaskCompletionSource = taskCompletionSource;

            // Return the Task object
            return taskCompletionSource.Task;
        }
    }
}

```

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Implementieren im freigegebenen Code

Nun, da die Schnittstelle für jede Plattform implementiert wurde, kann die Anwendung in der gemeinsamen portablen Klassenbibliothek nutzen.

Die [ `App` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/DependencyServiceSample/DependencyServiceSample.cs) -Klasse erstellt eine `Button` ein Foto auswählen:

```csharp
Button pickPictureButton = new Button
{
    Text = "Pick Photo",
    VerticalOptions = LayoutOptions.CenterAndExpand,
    HorizontalOptions = LayoutOptions.CenterAndExpand
};
stack.Children.Add(pickPictureButton);
```

Die `Clicked` Handler verwendet die `DependencyService` Klasse aufrufen `GetImageStreamAsync`. Dies führt zu einem Aufruf in das plattformprojekt. Wenn die Methode gibt ein `Stream` Objekt, und klicken Sie dann den Ereignishandler erstellt ein `Image` -Element für das Bild mit eine `TabGestureRecognizer`, und ersetzt die `StackLayout` auf der Seite mit dem `Image`:

```csharp
pickPictureButton.Clicked += async (sender, e) =>
{
    pickPictureButton.IsEnabled = false;
    Stream stream = await DependencyService.Get<IPicturePicker>().GetImageStreamAsync();

    if (stream != null)
    {
        Image image = new Image
        {
            Source = ImageSource.FromStream(() => stream),
            BackgroundColor = Color.Gray
        };

        TapGestureRecognizer recognizer = new TapGestureRecognizer();
        recognizer.Tapped += (sender2, args) =>
        {
            (MainPage as ContentPage).Content = stack;
            pickPictureButton.IsEnabled = true;
        };
        image.GestureRecognizers.Add(recognizer);

        (MainPage as ContentPage).Content = image;
    }
    else
    {
        pickPictureButton.IsEnabled = true;
    }
};
```

Tippen Sie auf die `Image` Element zurückgibt, die Seite normal.


## <a name="related-links"></a>Verwandte Links

- [Wählen Sie ein Foto aus dem Katalog (iOS)](https://developer.xamarin.com/recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery/)
- [Wählen Sie ein Bild (Android)](https://developer.xamarin.com/recipes/android/other_ux/pick_image/)
- [DependencyService (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample)
