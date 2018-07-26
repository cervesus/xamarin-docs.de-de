---
title: Wählen ein Foto aus der Bildbibliothek
description: In diesem Artikel wird erläutert, wie die Xamarin.Forms DependencyService-Klasse verwendet wird, ein Foto-Bildbibliothek des Telefons ausgewählt wird.
ms.prod: xamarin
ms.assetid: 4F51B0E7-6A63-403C-B488-500CCBCE75DD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/06/2017
ms.openlocfilehash: dafa60ff57f34bd4169af48e380079d9637d8d26
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241106"
---
# <a name="picking-a-photo-from-the-picture-library"></a>Wählen ein Foto aus der Bildbibliothek

Dieser Artikel führt durch die Erstellung einer Anwendung, die dem Benutzer ermöglicht, ein Foto-Bildbibliothek des Telefons auswählen. Da Xamarin.Forms nicht diese Funktion enthalten ist, ist es erforderlich, verwenden [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) den Zugriff auf native APIs auf jeder Plattform.  Dieser Artikel behandelt die folgenden Schritte aus, für die Verwendung von `DependencyService` für diese Aufgabe:

- **[Zum Erstellen der Schnittstelle](#Creating_the_Interface)**  &ndash; zu verstehen, wie die Schnittstelle in freigegebenem Code erstellt wird.
- **[iOS-Implementierung](#iOS_Implementation)**  &ndash; erfahren Sie, wie die Schnittstelle in nativem Code für iOS zu implementieren.
- **[Android-Implementierung](#Android_Implementation)**  &ndash; erfahren Sie, wie die Schnittstelle für Android in nativem Code zu implementieren.
- **[Implementierung der universellen Windows-Plattform](#UWP_Implementation)**  &ndash; erfahren Sie, wie die Schnittstelle in nativem Code für die universelle Windows-Plattform (UWP) zu implementieren.
- **[In freigegebenem Code implementieren](#Implementing_in_Shared_Code)**  &ndash; erfahren Sie, wie Sie mit `DependencyService` , die native Implementierung von freigegebenem Code aufzurufen.

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Erstellen der Schnittstelle

Erstellen Sie zunächst eine Schnittstelle in freigegebenem Code, der die gewünschte Funktionalität ausdrückt. Bei einer Foto-Picking-Anwendung ist nur eine Methode erforderlich. Dies wird definiert, der [ `IPicturePicker` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/DependencyServiceSample/IPicturePicker.cs) -Schnittstelle in der .NET Standard-Bibliothek mit dem Beispielcode:

```csharp
namespace DependencyServiceSample
{
    public interface IPicturePicker
    {
        Task<Stream> GetImageStreamAsync();
    }
}
```

Die `GetImageStreamAsync` Methode wird als asynchron definiert, da die Methode schnell zurückgeben muss, aber es kann nicht zurückgegeben werden, eine `Stream` -Objekt für die ausgewählten Fotos, bis der Benutzer die Bildbibliothek durchsucht und eines ausgewählt hat.

Diese Schnittstelle ist in allen Plattformen, die mithilfe von plattformspezifischen Code implementiert.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS-Implementierung

Die iOS-Implementierung von der `IPicturePicker` Schnittstelle verwendet die [ `UIImagePickerController` ](https://developer.xamarin.com/api/type/UIKit.UIImagePickerController/) wie beschrieben in der [ **wählen Sie ein Foto aus dem Katalog** ](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery) Rezept und [Beispielcode](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery).

Die iOS-Implementierung befindet sich der [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/iOS/PicturePickerImplementation.cs) Klasse im iOS-Projekt mit dem Beispielcode. Um diese Klasse sichtbar zu machen die `DependencyService` -Manager, die Klasse muss identifiziert werden, mit einer [`assembly`] Attribut vom Typ `Dependency`, und die Klasse muss öffentlich sein und explizit implementieren die `IPicturePicker` Schnittstelle:

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

Die `GetImageStreamAsync` -Methode erstellt eine `UIImagePickerController` und initialisiert dieses, um Images aus der Fotobibliothek auswählen. Zwei Ereignishandler sind erforderlich: eine für, wenn der Benutzer auswählt, ein Foto, und die andere für den Abbruch des Benutzers auf der Anzeige der Fotobibliothek. Die `PresentModalViewController` zeigt dann an den Benutzer der Fotobibliothek.

An diesem Punkt die `GetImageStreamAsync` Methodenrückgabewert muss eine `Task<Stream>` Objekt, das den Code, der es aufgerufen wird. Diese Aufgabe wird nur, wenn der Benutzer die Interaktion mit der Fotobibliothek abgeschlossen hat und eine der Ereignishandler aufgerufen wird. Für diesen Fällen die [ `TaskCompletionSource` ](https://msdn.microsoft.com/library/dd449174(v=vs.110).aspx) Klasse ist wichtig. Die Klasse bietet einen `Task` Objekt den richtigen generischen Typ für die zurückzugebenden aus der `GetImageStreamAsync` -Methode, und die Klasse kann später signalisiert, wenn die Aufgabe abgeschlossen ist.

Die `FinishedPickingMedia` Ereignishandler wird aufgerufen, wenn der Benutzer ein Bild ausgewählt hat. Der Handler bietet jedoch eine `UIImage` Objekt und die `Task` muss ein .NET zurückgeben `Stream` Objekt. Dies erfolgt in zwei Schritten: die `UIImage` -Objekt konvertiert wird zuerst eine JPEG-Datei im Arbeitsspeicher gespeichert, die einer `NSData` -Objekt, und klicken Sie dann die `NSData` Objekt wird in einer .NET konvertiert `Stream` Objekt. Ein Aufruf der `SetResult` -Methode der der `TaskCompletionSource` Objekt führt die Aufgabe durch die Bereitstellung der `Stream` Objekt:

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

Eine iOS-Anwendung erfordert die Berechtigung des Benutzers auf der Fotobibliothek. Fügen Sie den folgenden der `dict` -Abschnitt der Datei "Info.plist":

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>Picture Picker uses photo library</string>
```


<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android-Implementierung

Die Android-Implementierung verwendet das Verfahren beschrieben, die der [ **wählen Sie ein Image** ](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image) Rezept und [Beispielcode](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image). Ist Sie jedoch die Methode, die aufgerufen wird, wenn der Benutzer ein Bild aus der Bildbibliothek ausgewählt verfügt über eine `OnActivityResult` in eine abgeleitete Klasse überschreiben, `Activity`. Aus diesem Grund die normale [ `MainActivity` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/MainActivity.cs) Klasse im Android-Projekt verfügt über ein Feld, eine Eigenschaft und eine Überschreibung der außerdem wurde die `OnActivityResult` Methode:

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

Die `OnActivityResult`Außerkraftsetzung gibt an, die Datei mit dem ausgewählten Bild, mit einer Android `Uri` -Objekt, aber dies kann in einer .NET konvertiert werden `Stream` -Objekt durch Aufrufen der `OpenInputStream` -Methode der der `ContentResolver` -Objekt, das aus abgerufen wurden die Aktivität `ContentResolver` Eigenschaft.

Wie bei der iOS-Implementierung, die Android-Implementierung verwendet eine `TaskCompletionSource` zu signalisieren, wenn die Aufgabe abgeschlossen wurde. Dies `TaskCompletionSource` Objekt ist definiert als eine öffentliche Eigenschaft in der `MainActivity` Klasse. Dadurch wird die Eigenschaft, die in verwiesen werden, die [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/PicturePickerImplementation.cs) Klasse im Android-Projekt. Dies ist die Klasse mit dem `GetImageStreamAsync` Methode:

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

Diese Methode greift auf die `MainActivity` -Klasse für mehrere Zwecke: für die `Instance` -Eigenschaft, für die `PickImageId` Feld für die `TaskCompletionSource` -Eigenschaft und Aufrufen `StartActivityForResult`. Diese Methode wird von definiert die `FormsAppCompatActivity` -Klasse, die die Basisklasse der `MainActivity`.

<a name="UWP_Implementation" />

## <a name="uwp-implementation"></a>UWP-Implementierung

Im Gegensatz zu den IOS- und Android-Implementierungen, die Implementierung von der Foto-Auswahl für die universelle Windows-Plattform erfordert keine der `TaskCompletionSource` Klasse. Die [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/UWP/PicturePickerImplementation.cs) -Klasse verwendet die [ `FileOpenPicker` ](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/) Klasse für den Zugriff auf die Fotobibliothek. Da die `PickSingleFileAsync` -Methode der `FileOpenPicker` ist selbst asynchron, die `GetImageStreamAsync` Methode können einfach `await` mit dieser Methode (und anderen asynchronen Methoden) und Zurückgeben einer `Stream` Objekt:


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

## <a name="implementing-in-shared-code"></a>In freigegebenem Code implementieren

Nun, da die Schnittstelle für jede Plattform implementiert wurde, kann die Anwendung in der .NET Standard-Bibliothek nutzen.

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

Die `Clicked` Ereignishandler verwendet die `DependencyService` Klasse aufrufen `GetImageStreamAsync`. Dies führt zu einem Aufruf im plattformprojekt. Wenn die Methode zurückgibt eine `Stream` Objekt, und klicken Sie dann den Ereignishandler erstellt ein `Image` -Element für dieses Bild mit einem `TabGestureRecognizer`, und ersetzt die `StackLayout` auf der Seite mit dem `Image`:

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

Durch Tippen auf die `Image` Element gibt die Seite zur Normalität zurück.


## <a name="related-links"></a>Verwandte Links

- [Wählen Sie ein Foto aus dem Katalog (iOS)](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery)
- [Wählen Sie ein Bild (Android)](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image)
- [DependencyService (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample)
