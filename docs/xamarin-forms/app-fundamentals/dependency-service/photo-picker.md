---
title: Auswählen eines Fotos aus der Bildbibliothek
description: In diesem Artikel wird beschrieben, wie die Klasse „DependencyService“ von Xamarin.Forms verwendet wird, um ein Foto aus der Bildbibliothek des Smartphones auszuwählen.
ms.prod: xamarin
ms.assetid: 4F51B0E7-6A63-403C-B488-500CCBCE75DD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/06/2017
ms.openlocfilehash: 3452c79621013690f967e065c7afaf0768a50c3f
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "53057487"
---
# <a name="picking-a-photo-from-the-picture-library"></a>Auswählen eines Fotos aus der Bildbibliothek

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample)

Dieser Artikel führt durch die Erstellung einer Anwendung, die es dem Benutzer ermöglicht, ein Foto aus der Bildbibliothek des Smartphones auszuwählen. Da Xamarin.Forms diese Funktion nicht enthält, ist es erforderlich, mithilfe von [`DependencyService`](xref:Xamarin.Forms.DependencyService) auf native APIs auf jeder Plattform zuzugreifen.  Dieser Artikel behandelt die folgenden Schritte bei der Verwendung von `DependencyService` für diese Aufgabe:

- **[Erstellen der Schnittstelle:](#Creating_the_Interface)** Erfahren Sie, wie die Schnittstelle im freigegebenen Code erstellt wird.
- **[iOS-Implementierung:](#iOS_Implementation)** Erfahren Sie, wie die Schnittstelle in nativem Code für iOS implementiert wird.
- **[Android-Implementierung:](#Android_Implementation)** Erfahren Sie, wie die Schnittstelle in nativem Code für Android implementiert wird.
- **[UWP-Implementierung:](#UWP_Implementation)** Erfahren Sie, wie die Schnittstelle in nativem Code für die Universelle Windows-Plattform (UWP) implementiert wird.
- **[Implementierung in freigegebenem Code:](#Implementing_in_Shared_Code)** Erfahren Sie, wie Sie mit `DependencyService` die native Implementierung aus freigegebenem Code aufrufen.

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Erstellen der Schnittstelle

Erstellen Sie zunächst eine Schnittstelle in freigegebenem Code, der die gewünschte Funktionalität ausdrückt. Im Falle einer Anwendung zur Auswahl eines Fotos ist nur eine Methode erforderlich. Dies wird in der [`IPicturePicker`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/DependencyServiceSample/IPicturePicker.cs)-Schnittstelle in der .NET Standard-Bibliothek des Beispielcodes definiert:

```csharp
namespace DependencyServiceSample
{
    public interface IPicturePicker
    {
        Task<Stream> GetImageStreamAsync();
    }
}
```

Die `GetImageStreamAsync`-Methode ist als asynchron definiert, da die Methode schnell zurückgeben muss. Ein `Stream`-Objekt für das ausgewählte Foto kann jedoch erst zurückgegeben werden, wenn der Benutzer die Bildbibliothek durchsucht und eins ausgewählt hat.

Diese Schnittstelle ist plattformübergreifend mithilfe von plattformspezifischem Code implementiert.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS-Implementierung

Bei der iOS-Implementierung der `IPicturePicker`-Schnittstelle wird das [`UIImagePickerController`](https://developer.xamarin.com/api/type/UIKit.UIImagePickerController/)-Element wie in der Vorgehensweise unter [**Choose a Photo from the Gallery**](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery) (Auswählen eines Fotos aus dem Katalog) und [ Beispielcode](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery) beschrieben.

Die iOS-Implementierung ist in der [`PicturePickerImplementation`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/iOS/PicturePickerImplementation.cs)-Klasse im iOS-Projekt des Beispielcodes enthalten. Um diese Klasse für den `DependencyService`-Manager sichtbar zu machen, muss die Klasse mit einem [`assembly`]-Attribut vom Typ `Dependency` identifiziert werden, öffentlich sein und explizit die `IPicturePicker`-Schnittstelle implementieren:

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

Die `GetImageStreamAsync`-Methode erstellt ein `UIImagePickerController`-Element und initialisiert es, um Bilder aus der Fotobibliothek auszuwählen. Es werden zwei Ereignishandler benötigt: Ein Ereignishandler für die Auswahl eines Fotos durch den Benutzer und der andere zum Abbrechen der Fotobibliotheksanzeige durch den Benutzer. Dem Benutzer wird dann mithilfe des `PresentModalViewController`-Elements die Fotobibliothek angezeigt.

An dieser Stelle muss die `GetImageStreamAsync`-Methode ein `Task<Stream>`-Objekt an den Code zurückgeben, der sie aufruft. Dieser Task ist erst abgeschlossen, wenn der Benutzer die Interaktion mit der Fotobibliothek beendet hat und einer der Ereignishandler aufgerufen wird. Für solche Situationen ist die [`TaskCompletionSource`](https://msdn.microsoft.com/library/dd449174(v=vs.110).aspx)-Klasse unverzichtbar. Die Klasse stellt ein `Task`-Objekt vom richtigen generischen Typ für die Rückgabe aus der `GetImageStreamAsync`-Methode zur Verfügung. Später kann der Klasse dann signalisiert werden, wenn der Task abgeschlossen ist.

Der Ereignishandler `FinishedPickingMedia` wird aufgerufen, wenn der Benutzer ein Bild ausgewählt hat. Der Handler stellt jedoch ein `UIImage`-Objekt bereit und das `Task`-Element muss ein `Stream`-Objekt von .NET zurückgeben. Dies geschieht in zwei Schritten: Das `UIImage`-Objekt wird zunächst in eine JPEG-Datei in einem Speicher konvertiert, der sich in einem `NSData`-Objekt befindet. Dann wird das `NSData`-Objekt in ein `Stream`-Objekt von .NET umgewandelt. Ein Aufruf der `SetResult`-Methode des `TaskCompletionSource`-Objekts schließt den Task durch die Bereitstellung des `Stream`-Objekts ab:

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

                UnregisterEventHandlers();

                // Set the Stream as the completion of the Task
                taskCompletionSource.SetResult(stream);
            }
            else
            {
                UnregisterEventHandlers();
                taskCompletionSource.SetResult(null);
            }
            imagePicker.DismissModalViewController(true);
        }

        void OnImagePickerCancelled(object sender, EventArgs args)
        {
            UnregisterEventHandlers();
            taskCompletionSource.SetResult(null);
            imagePicker.DismissModalViewController(true);
        }

        void UnregisterEventHandlers()
        {
            imagePicker.FinishedPickingMedia -= OnImagePickerFinishedPickingMedia;
            imagePicker.Canceled -= OnImagePickerCancelled;
        }
    }
}

```

Eine iOS-Anwendung erfordert die Zugriffsberechtigung des Benutzers für die Fotobibliothek des Smartphones. Fügen Sie Folgendes zum `dict`-Abschnitt der Datei „Info.plist“ hinzu:

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>Picture Picker uses photo library</string>
```


<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android-Implementierung

Bei der Android-Implementierung wird die in der Vorgehensweise unter [**Select an Image**](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image) (Auswählen eines Bilds) und dem [Beispielcode](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image) beschriebene Methode verwendet. Die Methode, die jedoch aufgerufen wird, wenn der Benutzer ein Bild aus der Bildbibliothek ausgewählt hat, ist die Überschreibung von `OnActivityResult` in einer Klasse, die aus `Activity` abgeleitet ist. Aus diesem Grund wurde die normale [`MainActivity`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/MainActivity.cs)-Klasse im Android-Projekt um ein Feld, eine Eigenschaft und eine Überschreibung für die `OnActivityResult`-Methode ergänzt:

```csharp
public class MainActivity : FormsAppCompatActivity
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

Die Überschreibung von `OnActivityResult` gibt die ausgewählte Bilddatei mit einem `Uri`-Objekt von Android an. Dieses kann jedoch in ein `Stream`-Objekt von .NET konvertiert werden, indem die `OpenInputStream`-Methode des `ContentResolver`-Objekts aufgerufen wird, die aus der `ContentResolver`-Eigenschaft der Aktivität abgerufen wurde.

Wie bei der iOS-Implementierung wird auch bei der Android-Implementierung ein `TaskCompletionSource`-Objekt verwendet, das signalisiert, sobald der Task abgeschlossen ist. Dieses `TaskCompletionSource`-Objekt ist als öffentliche Eigenschaft in der `MainActivity`-Klasse definiert. So kann auf die Eigenschaft in der [`PicturePickerImplementation`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/PicturePickerImplementation.cs)-Klasse im Android-Projekt verwiesen werden. Dies ist die Klasse mit der `GetImageStreamAsync`-Methode:

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

Diese Methode greift auf die `MainActivity`-Klasse für mehrere Zwecke zu: für die Eigenschaft `Instance`, für das Feld `PickImageId`, für die Eigenschaft `TaskCompletionSource` und für den Aufruf von `StartActivityForResult`. Diese Methode wird durch die `FormsAppCompatActivity`-Klasse definiert, wobei es sich um die Basisklasse von `MainActivity` handelt.

<a name="UWP_Implementation" />

## <a name="uwp-implementation"></a>UWP-Implementierung

Im Gegensatz zu den iOS- und Android-Implementierungen wird die `TaskCompletionSource`-Klasse nicht zur Implementierung der Fotoauswahl für die Universelle Windows-Plattform benötigt. Die [`PicturePickerImplementation`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/UWP/PicturePickerImplementation.cs)-Klasse verwendet die [`FileOpenPicker`](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/)-Klasse für den Zugriff auf die Fotobibliothek. Da die `PickSingleFileAsync`-Methode von `FileOpenPicker` selbst asynchron ist, kann die `GetImageStreamAsync`-Methode einfach `await` mit dieser Methode (und anderen asynchronen Methoden) verwenden und ein `Stream`-Objekt zurückgeben:


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

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Implementierung in freigegebenem Code

Nachdem die Schnittstelle nun für jede Plattform implementiert wurde, kann die Anwendung in der .NET Standard-Bibliothek darauf zugreifen.

Die [`App`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/DependencyServiceSample/DependencyServiceSample.cs)Klasse erstellt ein `Button`-Element, um ein Foto auszuwählen:

```csharp
Button pickPictureButton = new Button
{
    Text = "Pick Photo",
    VerticalOptions = LayoutOptions.CenterAndExpand,
    HorizontalOptions = LayoutOptions.CenterAndExpand
};
stack.Children.Add(pickPictureButton);
```

Der `Clicked`-Handler verwendet die `DependencyService`-Klasse, um `GetImageStreamAsync` aufzurufen. Dies führt zu einem Aufruf im Plattformprojekt. Wenn die Methode ein `Stream`-Objekt zurückgibt, erstellt der Handler ein `Image`-Element für dieses Bild mit einem `TapGestureRecognizer`-Element. Anschließend wird das `StackLayout`-Element auf der Seite durch das `Image`-Element ersetzt:

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

Wenn Sie auf das `Image`-Element tippen, wird wieder die reguläre Seite angezeigt.


## <a name="related-links"></a>Verwandte Links

- [Choose a Photo from the Gallery (Auswählen eines Fotos aus dem Katalog) (iOS)](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery)
- [Select an Image (Auswählen eines Bilds) (Android)](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image)
- [DependencyService (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample)
