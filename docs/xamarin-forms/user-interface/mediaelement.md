---
title: Xamarin. Forms Media Element
description: In diesem Artikel wird erläutert, wie Media Element verwendet wird, um Video und Audiodaten in einer xamarin. Forms-Anwendung wiederzugeben.
ms.prod: xamarin
ms.assetid: e65f1e56-a80d-46c7-9ff4-7ae6650a3165
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/18/2020
ms.openlocfilehash: 945b0ffcff589da28c5e0ef2509a9d2ad7c388b5
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77635975"
---
# <a name="xamarinforms-mediaelement"></a>Xamarin. Forms Media Element

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-samples/tree/pre-release/WorkingWithMediaElement)

[`MediaElement`](xref:Xamarin.Forms.MediaElement) ist eine Ansicht für die Wiedergabe von Video und Audiodaten. Medien, die von der zugrunde liegenden Plattform unterstützt werden, können aus den folgenden Quellen wiedergegeben werden:

- Das Web unter Verwendung eines URIs (http oder HTTPS).
- Eine in die Platt Form Anwendung eingebettete Ressource, die das `ms-appx:///` URI-Schema verwendet.
- Dateien, die aus den lokalen und temporären Daten Ordnern der APP stammen und das `ms-appdata:///` URI-Schema verwenden.
- Die Bibliothek des Geräts.

[`MediaElement`](xref:Xamarin.Forms.MediaElement) können die Steuerelemente für die Platt Form Wiedergabe verwenden, die als Transport Steuerelemente bezeichnet werden. Sie sind jedoch standardmäßig deaktiviert und können durch ihre eigenen Transport Steuerelemente ersetzt werden. Die folgenden Screenshots zeigen die `MediaElement` Abspielen eines Videos mit den Platt Form Transport-Steuerelementen:

[![Screenshot eines Media Element-Videos unter IOS und Android](mediaelement-images/playback-controls.png "MediaElement, das ein Video abspielt")](mediaelement-images/playback-controls-large.png#lightbox "MediaElement, das ein Video abspielt")

[`MediaElement`](xref:Xamarin.Forms.MediaElement) ist in xamarin. Forms 4,5 verfügbar. Es ist jedoch zurzeit experimentell und kann nur verwendet werden, indem der *app.XAML.cs* -Datei die folgende Codezeile hinzugefügt wird:

```csharp
Device.SetFlags(new string[]{ "MediaElement_Experimental" });
```

> [!NOTE]
> [`MediaElement`](xref:Xamarin.Forms.MediaElement) ist unter IOS, Android, der universelle Windows-Plattform (UWP) und weiteren Plattformen verfügbar.

[`MediaElement`](xref:Xamarin.Forms.MediaElement) definiert die folgenden Eigenschaften:

- [`Aspect`](xref:Xamarin.Forms.MediaElement.Aspect)vom Typ [`Aspect`](xref:Xamarin.Forms.Aspect)bestimmt, wie die Medien so skaliert werden, dass Sie dem Anzeigebereich entsprechen. Der Standardwert dieser Eigenschaft ist `AspectFit`.
- [`AutoPlay`](xref:Xamarin.Forms.MediaElement.AutoPlay)vom Typ `bool`gibt an, ob die Medienwiedergabe automatisch startet, wenn die [`Source`](xref:Xamarin.Forms.MediaElement.Source) -Eigenschaft festgelegt wird. Der Standardwert dieser Eigenschaft ist `true`.
- [`BufferingProgress`](xref:Xamarin.Forms.MediaElement.BufferingProgress)vom Typ `double`gibt den aktuellen Puffer Status an. Der Standardwert dieser Eigenschaft ist 0,0.
- [`CanSeek`](xref:Xamarin.Forms.MediaElement.CanSeek)vom Typ `bool`gibt an, ob die Medien neu positioniert werden können, indem der Wert der [`Position`](xref:Xamarin.Forms.MediaElement.Position) -Eigenschaft festgelegt wird. Dies ist eine schreibgeschützte Eigenschaft.
- [`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState)vom Typ [`MediaElementState`](xref:Xamarin.Forms.MediaElementState)gibt den aktuellen Status des Steuer Elements an. Dies ist eine schreibgeschützte Eigenschaft, deren Standardwert `MediaElementState.Closed`ist.
- [`Duration`](xref:Xamarin.Forms.MediaElement.Duration)vom Typ `TimeSpan?`gibt die Dauer des aktuell geöffneten Mediums an. Dies ist eine schreibgeschützte Eigenschaft, deren Standardwert `null`ist.
- [`IsLooping`](xref:Xamarin.Forms.MediaElement.IsLooping)vom Typ `bool`beschreibt, ob die derzeit geladene Medienquelle die Wiedergabe von Anfang an wieder aufnehmen soll, nachdem das Ende erreicht wurde. Der Standardwert dieser Eigenschaft ist `false`.
- [`KeepScreenOn`](xref:Xamarin.Forms.MediaElement.KeepScreenOn)vom Typ `bool`bestimmt, ob der Bildschirm des Geräts während der Medienwiedergabe eingeschaltet bleiben soll. Der Standardwert dieser Eigenschaft ist `false`.
- [`Position`](xref:Xamarin.Forms.MediaElement.Position)vom Typ `TimeSpan`beschreibt den aktuellen Fortschritt durch die Wiedergabezeit des Mediums. Der Standardwert dieser Eigenschaft ist `TimeSpan.Zero`.
- [`ShowsPlaybackControls`](xref:Xamarin.Forms.MediaElement.ShowsPlaybackControls)vom Typ `bool`bestimmt, ob die Plattformen Wiedergabe Steuerelemente angezeigt werden. Der Standardwert dieser Eigenschaft ist `false`.
- [`Source`](xref:Xamarin.Forms.MediaElement.Source)vom Typ [`MediaSource`](xref:Xamarin.Forms.MediaSource)gibt die Quelle des Mediums an, das in das Steuerelement geladen wurde.
- [`VideoHeight`](xref:Xamarin.Forms.MediaElement.VideoHeight)vom Typ `int`gibt die Höhe des Steuer Elements an. Dies ist eine schreibgeschützte Eigenschaft.
- [`VideoWidth`](xref:Xamarin.Forms.MediaElement.VideoWidth)vom Typ `int`gibt die Breite des Steuer Elements an. Dies ist eine schreibgeschützte Eigenschaft.
- [`Volume`](xref:Xamarin.Forms.MediaElement.Volume)vom Typ `double`bestimmt das Volume des Mediums, das auf einer linearen Skala zwischen 0 und 1 dargestellt wird. Diese Eigenschaft verwendet eine `TwoWay` Bindung, und der Standardwert ist 1.

Diese Eigenschaften, mit Ausnahme der `CanSeek`-Eigenschaft, werden von [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Objekten gestützt, was bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Die [`MediaElement`](xref:Xamarin.Forms.MediaElement) -Klasse definiert außerdem vier Ereignisse:

- [`MediaOpened`](xref:Xamarin.Forms.MediaElement.MediaOpened) wird ausgelöst, wenn der Mediendaten Strom überprüft und geöffnet wurde.
- [`MediaEnded`](xref:Xamarin.Forms.MediaElement.MediaEnded) wird ausgelöst, wenn die `MediaElement` die Wiedergabe der Medien abgeschlossen hat.
- [`MediaFailed`](xref:Xamarin.Forms.MediaElement.MediaFailed) wird ausgelöst, wenn der Medienquelle ein Fehler zugeordnet ist.
- [`SeekCompleted`](xref:Xamarin.Forms.MediaElement.SeekCompleted) wird ausgelöst, wenn der Suchpunkt eines angeforderten Such Vorgangs für die Wiedergabe bereit ist.

Außerdem umfasst [`MediaElement`](xref:Xamarin.Forms.MediaElement) [`Play`](xref:Xamarin.Forms.MediaElement.Play)-, [`Pause`](xref:Xamarin.Forms.MediaElement.Pause)-und [`Stop`](xref:Xamarin.Forms.MediaElement.Stop) -Methoden.

Informationen zu den unterstützten Medienformaten unter Android finden Sie [unter Unterstützte Medienformate](https://developer.android.com/guide/topics/media/media-formats) auf Developer.Android.com. Informationen zu den unterstützten Medienformaten im universelle Windows-Plattform (UWP) finden Sie [unter unterstützte Codecs](/windows/uwp/audio-video-camera/supported-codecs).

## <a name="play-remote-media"></a>Wiedergeben von Remote Medien

Eine [`MediaElement`](xref:Xamarin.Forms.MediaElement) kann Remote Mediendateien mit den HTTP-und HTTPS-URI-Schemas wiedergeben. Dies wird erreicht, indem die [`Source`](xref:Xamarin.Forms.MediaElement.Source) -Eigenschaft auf den URI der Mediendatei festgelegt wird:

```xaml
<MediaElement Source="https://sec.ch9.ms/ch9/5d93/a1eab4bf-3288-4faf-81c4-294402a85d93/XamarinShow_mid.mp4"
              ShowsPlaybackControls="True" />
```

Standardmäßig wird das von der [`Source`](xref:Xamarin.Forms.MediaElement.Source) -Eigenschaft definierte Medium sofort nach dem Öffnen des Mediums abgespielt. Legen Sie die [`AutoPlay`](xref:Xamarin.Forms.MediaElement.AutoPlay) -Eigenschaft auf `false`fest, um die automatische Wiedergabe zu unterdrücken.

Steuerelemente für die Medienwiedergabe werden standardmäßig deaktiviert und aktiviert, indem die [`ShowsPlaybackControls`](xref:Xamarin.Forms.MediaElement.ShowsPlaybackControls) -Eigenschaft auf `true`festgelegt wird. [`MediaElement`](xref:Xamarin.Forms.MediaElement) verwendet dann die Steuerelemente für die Platt Form Wiedergabe.

## <a name="play-local-media"></a>Lokale Medien wiedergeben

Lokale Medien können aus den folgenden Quellen wiedergegeben werden:

- Eine in die Platt Form Anwendung eingebettete Ressource, die das `ms-appx:///` URI-Schema verwendet.
- Dateien, die aus den lokalen und temporären Daten Ordnern der APP stammen und das `ms-appdata:///` URI-Schema verwenden.
- Die Bibliothek des Geräts.

Weitere Informationen zu diesen URI-Schemas finden Sie unter [URI-Schemas](/windows/uwp/app-resources/uri-schemes).

### <a name="play-media-embedded-in-the-app-package"></a>Wiedergeben von Medien, die im App-Paket eingebettet sind

Mit einem [`MediaElement`](xref:Xamarin.Forms.MediaElement) können Mediendateien wiedergegeben werden, die mit dem `ms-appx:///` URI-Schema in das App-Paket eingebettet sind. Mediendateien werden in das App-Paket eingebettet, indem Sie im Platt Form Projekt platziert werden.

Das Speichern einer Mediendatei im Platt Form Projekt unterscheidet sich für jede Plattform:

- Unter IOS müssen Mediendateien im Ordner " **Resources** " oder in einem Unterordner des Ordners " **Resources** " gespeichert werden. Die Mediendatei muss über eine `Build Action` `BundleResource`verfügen.
- Unter Android müssen Mediendateien in einem Unterordner von **Ressourcen** mit dem Namen **RAW**gespeichert werden. Der Ordner **raw** darf keine Unterordner enthalten. Die Mediendatei muss über eine `Build Action` `AndroidResource`verfügen.
- Bei UWP können Mediendateien in einem beliebigen Ordner im Projekt gespeichert werden. Die Mediendatei muss über eine `BuildAction` `Content`verfügen.

Mediendateien, die diese Kriterien erfüllen, können dann mit dem `ms-appx:///`-URI-Schema wiedergegeben werden:

```xaml
<MediaElement Source="ms-appx:///XamarinForms101UsingEmbeddedImages.mp4"
              ShowsPlaybackControls="True" />
```

Wenn Sie die Datenbindung verwenden, kann ein Wert Konverter verwendet werden, um dieses URI-Schema anzuwenden:

```csharp
public class VideoSourceConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value == null)
            return null;

        if (string.IsNullOrWhiteSpace(value.ToString()))
            return null;

        if (Device.RuntimePlatform == Device.UWP)
            return new Uri($"ms-appx:///Assets/{value}");
        else
            return new Uri($"ms-appx:///{value}");
    }
    // ...
}
```

Eine Instanz des-`VideoSourceConverter` kann dann verwendet werden, um das `ms-appx:///` URI-Schema auf eine eingebettete Mediendatei anzuwenden:

```xaml
<MediaElement Source="{Binding MediaSource, Converter={StaticResource VideoSourceConverter}}"
              ShowsPlaybackControls="True" />
```

Weitere Informationen zum MS-AppX-URI-Schema finden Sie unter [MS-AppX und MS-AppX-Web](/windows/uwp/app-resources/uri-schemes#ms-appx-and-ms-appx-web).

### <a name="play-media-from-the-apps-local-and-temporary-folders"></a>Wiedergeben von Medien aus den lokalen und temporären Ordnern der APP

Eine [`MediaElement`](xref:Xamarin.Forms.MediaElement) kann Mediendateien wiedergeben, die mithilfe des `ms-appdata:///` URI-Schemas in die lokalen oder temporären Datenordner der APP kopiert werden.

Im folgenden Beispiel wird die [`Source`](xref:Xamarin.Forms.MediaElement.Source) -Eigenschaft auf eine Mediendatei festgelegt, die im lokalen Datenordner der APP gespeichert ist:

```xaml
<MediaElement Source="ms-appdata:///local/XamarinVideo.mp4"
              ShowsPlaybackControls="True" />
```

Im folgenden Beispiel wird die [`Source`](xref:Xamarin.Forms.MediaElement.Source) -Eigenschaft für eine Mediendatei gezeigt, die im temporären Datenordner der APP gespeichert ist:

```xaml
<MediaElement Source="ms-appdata:///temp/XamarinVideo.mp4"
              ShowsPlaybackControls="True" />
```

> [!IMPORTANT]
> Zusätzlich zur Wiedergabe von Mediendateien, die in den lokalen oder temporären Daten Ordnern der APP gespeichert sind, kann UWP auch Mediendateien wiedergeben, die sich im roamingordner der APP befinden. Dies kann erreicht werden, indem die Mediendatei mit `ms-appdata:///roaming/`vorangestellt wird.

Wenn Sie die Datenbindung verwenden, kann ein Wert Konverter verwendet werden, um dieses URI-Schema anzuwenden:

```csharp
public class VideoSourceConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value == null)
            return null;

        if (string.IsNullOrWhiteSpace(value.ToString()))
            return null;

        return new Uri($"ms-appdata:///{value}");
    }
    // ...
}
```

Eine Instanz des `VideoSourceConverter` kann dann verwendet werden, um das `ms-appdata:///` URI-Schema auf eine Mediendatei im lokalen oder temporären Datenordner der APP anzuwenden:

```xaml
<MediaElement Source="{Binding MediaSource, Converter={StaticResource VideoSourceConverter}}"
              ShowsPlaybackControls="True" />
```

Weitere Informationen zum MS-APPDATA-URI-Schema finden Sie unter [MS-APPDATA](/windows/uwp/app-resources/uri-schemes#ms-appdata).

#### <a name="copying-a-media-file-to-the-apps-local-or-temporary-data-folder"></a>Kopieren einer Mediendatei in den lokalen oder temporären Datenordner der APP

Wenn Sie eine Mediendatei abspielen, die im lokalen oder temporären Datenordner der APP gespeichert ist, muss die Mediendatei dort von der APP kopiert werden. Dies kann z. b. durch Kopieren einer Mediendatei aus dem App-Paket erreicht werden:

```csharp
// This method copies the video from the app package to the app data
// directory for your app. To copy the video to the temp directory
// for your app, comment out the first line of code, and uncomment
// the second line of code.
public static async Task CopyVideoIfNotExists(string filename)
{
    string folder = FileSystem.AppDataDirectory;
    //string folder = Path.GetTempPath();
    string videoFile = Path.Combine(folder, "XamarinVideo.mp4");

    if (!File.Exists(videoFile))
    {
        using (Stream inputStream = await FileSystem.OpenAppPackageFileAsync(filename))
        {
            using (FileStream outputStream = File.Create(videoFile))
            {
                await inputStream.CopyToAsync(outputStream);
            }
        }
    }
}
```

> [!NOTE]
> Im obigen Codebeispiel wird die `FileSystem`-Klasse verwendet, die in xamarin. Essentials enthalten ist. Weitere Informationen finden Sie unter [xamarin. Essentials: File System](~/essentials/file-system-helpers.md?context=xamarin%2Fxamarin-forms&tabs=android)-Hilfsprogramme.

### <a name="play-media-from-the-device-library"></a>Wiedergeben von Medien in der Geräte Bibliothek

Die meisten modernen mobilen Geräte und Desktop Computer können Videos und Audiodaten mithilfe der Kamera und des Mikrofons des Geräts aufzeichnen. Die erstellten Medien werden dann als Dateien auf dem Gerät gespeichert. Diese Dateien können aus der Bibliothek abgerufen und von der [`MediaElement`](xref:Xamarin.Forms.MediaElement)abgespielt werden.

Jede der Plattformen enthält eine Funktion, die es dem Benutzer ermöglicht, Medien aus der Bibliothek des Geräts auszuwählen. In xamarin. Forms können Platt Form Projekte diese Funktion aufrufen und können von der [`DependencyService`](xref:Xamarin.Forms.DependencyService) -Klasse aufgerufen werden.

Der in der Beispielanwendung verwendete in der Beispielanwendung verwendete Video Pick-Abhängigkeits Dienst ähnelt dem, das in [Auswählen eines Fotos aus der Bildbibliothek](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)definiert ist, mit dem Unterschied, dass die Auswahl einen Dateinamen anstelle eines `Stream` Objekts zurückgibt. Das freigegebene Code Projekt definiert eine Schnittstelle mit dem Namen `IVideoPicker`, die eine einzelne Methode mit dem Namen `GetVideoFileAsync`definiert. Diese Schnittstelle wird von jeder Plattform in einer `VideoPicker` Klasse implementiert.

Im folgenden Codebeispiel wird gezeigt, wie eine Mediendatei aus der Geräte Bibliothek abgerufen wird:

```csharp
string filename = await DependencyService.Get<IVideoPicker>().GetVideoFileAsync();
if (!string.IsNullOrWhiteSpace(filename))
{
    mediaElement.Source = new FileMediaSource
    {
        File = filename
    };
}
```

Der Abhängigkeits Dienst für die Videoauswahl wird aufgerufen, indem die `DependencyService.Get`-Methode aufgerufen wird, um die Implementierung einer `IVideoPicker` Schnittstelle im Platt Form Projekt abzurufen. Anschließend wird die `GetVideoFileAsync`-Methode für diese Instanz aufgerufen, und der zurückgegebene Dateiname wird verwendet, um ein [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource) Objekt zu erstellen und es auf die [`Source`](xref:Xamarin.Forms.MediaElement.Source) -Eigenschaft der [`MediaElement`](xref:Xamarin.Forms.MediaElement)festzulegen.

## <a name="change-video-aspect-ratio"></a>Videoseiten Verhältnis ändern

Die [`Aspect`](xref:Xamarin.Forms.MediaElement.Aspect) -Eigenschaft bestimmt, wie Video Medien so skaliert werden, dass Sie an den Anzeigebereich angepasst werden. Standardmäßig ist diese Eigenschaft auf den `AspectFit` Enumerationsmember festgelegt, kann jedoch auf einen der [`Aspect`](xref:Xamarin.Forms.Aspect) Enumerationsmember festgelegt werden:

- `AspectFit` gibt an, dass das Video bei Bedarf in den Anzeigebereich passt, wobei das Seitenverhältnis beibehalten wird.
- `AspectFill` gibt an, dass das Video abgeschnitten wird, sodass es den Anzeigebereich füllt, während das Seitenverhältnis beibehalten wird.
- `Fill` gibt an, dass das Video gestreckt wird, um den Anzeigebereich auszufüllen.

## <a name="poll-for-position-data"></a>Abrufen von Positionsdaten

Die Eigenschafts Änderungs Benachrichtigung für die [`Position`](xref:Xamarin.Forms.MediaElement.Position) bindbare Eigenschaft wird nur in den Schlüssel Augenblicken ausgelöst, z. b. beim Starten und Beenden der Wiedergabe und beim Anhalten. Aus diesem Grund ergibt die Datenbindung an die `Position`-Eigenschaft keine exakten Positionsdaten. Stattdessen müssen Sie einen Timer einrichten und die-Eigenschaft Abfragen.

Ein guter Ausgangspunkt hierfür ist die `OnAppearing` Außerkraftsetzung für die Seite, die die Positionsdaten erfordert, wenn Medien wiedergegeben werden:

```csharp
bool polling = true;

protected override void OnAppearing()
{
    base.OnAppearing();

    Device.StartTimer(TimeSpan.FromMilliseconds(1000), () =>
    {
        Device.BeginInvokeOnMainThread(() =>
        {
            positionLabel.Text = mediaElement.Position.ToString("hh\\:mm\\:ss");
        });
        return polling;
    });
}

protected override void OnDisappearing()
{
    base.OnDisappearing();
    polling = false;
}
```

In diesem Beispiel startet die `OnAppearing` Überschreibung einen Timer, der die `positionLabel` mit dem `Position` Wert pro Sekunde aktualisiert. Der Timer-Rückruf wird jede Sekunde aufgerufen, bis der Rückruf `false`zurückgibt. Bei der Seitennavigation wird der `OnDisappearing` Überschreibungs Vorgang ausgeführt, der den Aufruf des timerrückrufs stoppt.

## <a name="understand-mediasource-types"></a>Grundlegendes zu MediaSource-Typen

Ein [`MediaElement`](xref:Xamarin.Forms.MediaElement) kann Medien wiedergeben, indem seine [`Source`](xref:Xamarin.Forms.MediaElement.Source) -Eigenschaft auf eine Remote Datei oder eine lokale Mediendatei festgelegt wird. Die `Source`-Eigenschaft ist vom Typ [`MediaSource`](xref:Xamarin.Forms.MediaSource), und diese Klasse definiert zwei statische Methoden:

- [`FromFile`](xref:Xamarin.Forms.MediaSource.FromFile*)gibt eine [`MediaSource`](xref:Xamarin.Forms.MediaSource) Instanz aus einem `string`-Argument zurück.
- [`FromUri`](xref:Xamarin.Forms.MediaSource.FromUri*)gibt eine [`MediaSource`](xref:Xamarin.Forms.MediaSource) Instanz aus einem `Uri`-Argument zurück.

Außerdem verfügt die [`MediaSource`](xref:Xamarin.Forms.MediaSource) -Klasse auch über implizite Operatoren, die `MediaSource`-Instanzen von `string` und `Uri`-Argumenten zurückgeben.

> [!NOTE]
> Wenn die [`Source`](xref:Xamarin.Forms.MediaElement.Source) -Eigenschaft in XAML festgelegt wird, wird ein Typkonverter aufgerufen, um eine [`MediaSource`](xref:Xamarin.Forms.MediaSource) Instanz aus einem `string` oder `Uri`zurückzugeben.

Die [`MediaSource`](xref:Xamarin.Forms.MediaSource) -Klasse verfügt auch über zwei abgeleitete Klassen:

- [`UriMediaSource`](xref:Xamarin.Forms.UriMediaSource), das zum Angeben einer Remote Mediendatei aus einem URI verwendet wird. Diese Klasse verfügt über eine [`Uri`](xref:Xamarin.Forms.UriMediaSource.Uri) -Eigenschaft, die auf einen `Uri`festgelegt werden kann.
- [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource), das verwendet wird, um eine lokale Mediendatei aus einem `string`anzugeben. Diese Klasse verfügt über eine [`File`](xref:Xamarin.Forms.FileMediaSource.File) -Eigenschaft, die auf einen `string`festgelegt werden kann. Außerdem verfügt diese Klasse über implizite Operatoren zum Konvertieren eines `string` in ein `FileMediaSource`-Objekt und ein `FileMediaSource`-Objekt in eine `string`.

> [!NOTE]
> Wenn ein [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource) Objekt in XAML erstellt wird, wird ein Typkonverter aufgerufen, um eine [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource) Instanz von einem `string`zurückzugeben.

## <a name="determine-mediaelement-status"></a>Ermitteln des Media Element-Status

Die [`MediaElement`](xref:Xamarin.Forms.MediaElement) -Klasse definiert eine schreibgeschützte bindbare Eigenschaft mit dem Namen [`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState)vom Typ [`MediaElementState`](xref:Xamarin.Forms.MediaElementState). Diese Eigenschaft gibt den aktuellen Status des Steuer Elements an, z. b. ob die Medien wiedergegeben oder angehalten werden oder ob die Medien noch nicht wiedergegeben werden können.

Die [`MediaElementState`](xref:Xamarin.Forms.MediaElementState) -Enumeration definiert die folgenden Member:

- `Closed` gibt an, dass die `MediaElement` keine Medien enthält.
- `Opening` gibt an, dass die `MediaElement` die angegebene Quelle überprüft und versucht, die angegebene Quelle zu laden.
- `Buffering` gibt an, dass die `MediaElement` die Medien für die Wiedergabe lädt. Die [`Position`](xref:Xamarin.Forms.MediaElement.Position) -Eigenschaft wird während dieses Zustands nicht Fortschritt. Wenn das `MediaElement` Video abgespielt hat, zeigt es weiterhin den zuletzt angezeigten Frame an.
- `Playing` gibt an, dass die `MediaElement` die Medienquelle wieder gibt.
- `Paused` gibt an, dass der `MediaElement` seine [`Position`](xref:Xamarin.Forms.MediaElement.Position) -Eigenschaft nicht vornimmt. Wenn das `MediaElement` Video abgespielt hat, wird der aktuelle Frame weiterhin angezeigt.
- `Stopped` gibt an, dass die `MediaElement` Medien enthält, aber nicht wiedergegeben oder angehalten wird. Die [`Position`](xref:Xamarin.Forms.MediaElement.Position) -Eigenschaft ist 0 (null) und wird nicht fortfahren. Wenn das geladene Medium ein Video ist, wird im `MediaElement` der erste Frame angezeigt.

Im Allgemeinen ist es nicht notwendig, die [`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState) -Eigenschaft zu überprüfen, wenn Sie die [`MediaElement`](xref:Xamarin.Forms.MediaElement) -Transport Steuerelemente verwenden Diese Eigenschaft wird jedoch beim Implementieren ihrer eigenen Transport Steuerelemente wichtig.

## <a name="implement-custom-transport-controls"></a>Implementieren von benutzerdefinierten Transport Steuerelementen

Die Transport Steuerelemente eines Media Players enthalten die Schalt **Flächen, die**die Funktionen **abspielen**, anhalten und **Anhalten**ausführen. In der Regel werden diese Schaltflächen mit bekannten Symbolen anstelle von Text dargestellt, und die Funktionen **Wiedergabe** und **Pause** werden in eine Schaltfläche kombiniert.

Standardmäßig sind die [`MediaElement`](xref:Xamarin.Forms.MediaElement) Wiedergabe Steuerelemente deaktiviert. Dies ermöglicht es Ihnen, die `MediaElement` Programm gesteuert zu steuern oder eigene Transport Steuerelemente bereitzustellen. Um dies zu unterstützen, umfasst `MediaElement` [`Play`](xref:Xamarin.Forms.MediaElement.Play)-, [`Pause`](xref:Xamarin.Forms.MediaElement.Pause)-und [`Stop`](xref:Xamarin.Forms.MediaElement.Stop) -Methoden.

Das folgende XAML-Beispiel zeigt eine Seite, die eine [`MediaElement`](xref:Xamarin.Forms.MediaElement) und benutzerdefinierte Transport Steuerelemente enthält:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MediaElementDemos.CustomTransportPage"
             Title="Custom transport">
    <Grid>
        ...
        <MediaElement x:Name="mediaElement"
                      AutoPlay="False"
                      ... />
        <StackLayout BindingContext="{x:Reference mediaElement}"
                     ...>
            <Button Text="&#x25B6;&#xFE0F; Play"
                    HorizontalOptions="CenterAndExpand"
                    Clicked="OnPlayPauseButtonClicked">
                <Button.Triggers>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding CurrentState}"
                                 Value="{x:Static MediaElementState.Playing}">
                        <Setter Property="Text"
                                Value="&#x23F8; Pause" />
                    </DataTrigger>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding CurrentState}"
                                 Value="{x:Static MediaElementState.Buffering}">
                        <Setter Property="IsEnabled"
                                Value="False" />
                    </DataTrigger>
                </Button.Triggers>
            </Button>
            <Button Text="&#x23F9; Stop"
                    HorizontalOptions="CenterAndExpand"
                    Clicked="OnStopButtonClicked">
                <Button.Triggers>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding CurrentState}"
                                 Value="{x:Static MediaElementState.Stopped}">
                        <Setter Property="IsEnabled"
                                Value="False" />
                    </DataTrigger>
                </Button.Triggers>
            </Button>
        </StackLayout>
    </Grid>
</ContentPage>
```

In diesem Beispiel werden die benutzerdefinierten Transport Steuerelemente als [`Button`](xref:Xamarin.Forms.Button) Objekte definiert. Es gibt jedoch nur zwei `Button` Objekte, wobei die erste `Button` die wieder **Gabe** und **Pause**darstellt, und die zweite `Button`, die den Vorgang " **Anhalten**" darstellt. [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) Objekte werden verwendet, um die Schaltflächen zu aktivieren und zu deaktivieren und um die erste Schaltfläche zwischen wieder **Gabe** und **Pause**zu wechseln. Weitere Informationen zu Daten Triggern finden Sie unter [xamarin. Forms-Trigger](~/xamarin-forms/app-fundamentals/triggers.md).

Die Code-Behind-Datei enthält die Handler für die [`Clicked`](xref:Xamarin.Forms.Button.Clicked) -Ereignisse:

```csharp
void OnPlayPauseButtonClicked(object sender, EventArgs args)
{
    if (mediaElement.CurrentState == MediaElementState.Stopped ||
        mediaElement.CurrentState == MediaElementState.Paused)
    {
        mediaElement.Play();
    }
    else if (mediaElement.CurrentState == MediaElementState.Playing)
    {
        mediaElement.Pause();
    }
}

void OnStopButtonClicked(object sender, EventArgs args)
{
    mediaElement.Stop();
}
```

Die **Wiedergabe Schaltfläche kann** gedrückt werden, sobald Sie aktiviert wird, um die Wiedergabe zu starten:

[![Screenshot eines Media Element mit benutzerdefinierten Transport Steuerelementen unter IOS und Android](mediaelement-images/custom-transport-playback.png "MediaElement, das ein Video abspielt")](mediaelement-images/custom-transport-playback-large.png#lightbox "MediaElement, das ein Video abspielt")

Wenn Sie **die Schaltfläche Anhalten drücken,** wird die Wiedergabe angehalten:

[![Screenshot eines Media Element-Elements mit angehaltenen Wiedergabe unter IOS und Android](mediaelement-images/custom-transport-paused.png "Media Element mit angehaltenen Videos")](mediaelement-images/custom-transport-paused-large.png#lightbox "Media Element mit angehaltenen Videos")

Durch Drücken der Schaltfläche **Beenden** wird die Wiedergabe beendet und die Position der Mediendatei an den Anfang zurückgegeben.

## <a name="implement-a-custom-position-bar"></a>Implementieren einer benutzerdefinierten Positions Leiste

Die von jeder Plattform implementierten Transportsteuerelemente umfassen auch eine Fortschrittsleiste. Diese Leiste ähnelt einem Schieberegler und zeigt den aktuellen Speicherort der Medien innerhalb der gesamten Dauer an. Außerdem können Sie die Positions Leiste so bearbeiten, dass Sie an eine neue Position im Video vorwärts oder rückwärts verschoben wird.

Zum Implementieren einer benutzerdefinierten Positions Leiste müssen Sie die Dauer der Medien und die aktuelle Wiedergabe Position kennen. Diese Daten sind in den Eigenschaften [`Duration`](xref:Xamarin.Forms.MediaElement.Duration) und [`Position`](xref:Xamarin.Forms.MediaElement.Position) verfügbar.

> [!IMPORTANT]
> Der [`Position`](xref:Xamarin.Forms.MediaElement.Position) muss abgepolgt werden, um genaue Positionsdaten zu erhalten. Weitere Informationen finden Sie unter Abrufen [von Positionsdaten](#poll-for-position-data).

Eine benutzerdefinierte Positions Leiste kann mithilfe eines [`Slider`](xref:Xamarin.Forms.Slider)implementiert werden, wie im folgenden Beispiel gezeigt:

```csharp
public class PositionSlider : Slider
{
    public static readonly BindableProperty DurationProperty =
        BindableProperty.Create(nameof(Duration), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan(1),
                                propertyChanged: (bindable, oldValue, newValue) =>
                                {
                                    ((PositionSlider)bindable).SetTimeToEnd();
                                    double seconds = ((TimeSpan)newValue).TotalSeconds;
                                    ((Slider)bindable).Maximum = seconds <= 0 ? 1 : seconds;
                                });

    public TimeSpan Duration
    {
        get { return (TimeSpan)GetValue(DurationProperty); }
        set { SetValue(DurationProperty, value); }
    }

    public static readonly BindableProperty PositionProperty =
        BindableProperty.Create(nameof(Position), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan(0),
                                propertyChanged: (bindable, oldValue, newValue) => ((PositionSlider)bindable).SetTimeToEnd());

    public TimeSpan Position
    {
        get { return (TimeSpan)GetValue(PositionProperty); }
        set { SetValue(PositionProperty, value); }
    }

    static readonly BindablePropertyKey TimeToEndPropertyKey =
        BindableProperty.CreateReadOnly(nameof(TimeToEnd), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan());

    public static readonly BindableProperty TimeToEndProperty = TimeToEndPropertyKey.BindableProperty;

    public TimeSpan TimeToEnd
    {
        get { return (TimeSpan)GetValue(TimeToEndProperty); }
        private set { SetValue(TimeToEndPropertyKey, value); }
    }

    public PositionSlider()
    {
        PropertyChanged += (sender, args) =>
        {
            if (args.PropertyName == "Value")
            {
                TimeSpan newPosition = TimeSpan.FromSeconds(Value);
                if (Math.Abs(newPosition.TotalSeconds - Position.TotalSeconds) / Duration.TotalSeconds > 0.01)
                {
                    Position = newPosition;
                }
            }
        };
    }

    void SetTimeToEnd()
    {
        TimeToEnd = Duration - Position;
    }
}
```

Die `PositionSlider`-Klasse definiert eigene `Duration` und `Position` bindbare Eigenschaften und eine `TimeToEnd` bindbare Eigenschaft. Alle drei Eigenschaften sind vom Typ `TimeSpan`. Der von der Eigenschaft geänderte Handler für die `Duration`-Eigenschaft legt die `Maximum`-Eigenschaft des [`Slider`](xref:Xamarin.Forms.Slider) auf die `TotalSeconds`-Eigenschaft des `TimeSpan` Werts fest. Die `TimeToEnd`-Eigenschaft wird auf der Grundlage von Änderungen an den Eigenschaften `Duration` und `Position` berechnet und beginnt bei der Dauer des Mediums und sinkt bei der Wiedergabe auf den Wert 0 (null).

Der `PositionSlider` wird vom zugrunde liegenden [`Slider`](xref:Xamarin.Forms.Slider) aktualisiert, wenn die `Slider` verschoben wird, um anzugeben, dass die Medien an eine neue Position erweitert oder umgekehrt werden sollen. Dies wird im `PropertyChanged` Handler im `PositionSlider`-Konstruktor erkannt. Der Handler prüft nach Änderungen an der `Value`-Eigenschaft. Wenn sie sich von der `Position`-Eigenschaft unterscheidet, wird die `Position`-Eigenschaft über die `Value`-Eigenschaft festgelegt. Weitere Informationen zur Verwendung eines [`Slider`](xref:Xamarin.Forms.Slider) finden Sie unter [xamarin. Forms Slider.](~/xamarin-forms/user-interface/slider.md)

> [!NOTE]
> Unter Android verfügt der [`Slider`](xref:Xamarin.Forms.Slider) nur über 1000 diskrete Schritte, unabhängig von den Einstellungen für `Minimum` und `Maximum`. Wenn die Medien Länge mehr als 1000 Sekunden beträgt, entsprechen zwei unterschiedliche `Position` Werte dem gleichen `Value` der `Slider`. Aus diesem Grund prüft der obige Code, ob die neue Position und die vorhandene Position größer als ein Hundertstel der Gesamtdauer sind.

Das folgende Beispiel zeigt die `PositionSlider`, die auf einer Seite verbraucht werden:

```xaml
<controls:PositionSlider x:Name="positionSlider"
                         BindingContext="{x:Reference mediaElement}"
                         Duration="{Binding Duration}"
                         ValueChanged="OnPositionSliderValueChanged">
    <controls:PositionSlider.Triggers>
        <DataTrigger TargetType="controls:PositionSlider"
                     Binding="{Binding CurrentState}"
                     Value="{x:Static MediaElementState.Buffering}">
            <Setter Property="IsEnabled" Value="False" />
        </DataTrigger>
    </controls:PositionSlider.Triggers>
</controls:PositionSlider>
```

In diesem Beispiel ist die `Duration`-Eigenschaft des `PositionSlider` Daten gebunden an die [`Duration`](xref:Xamarin.Forms.MediaElement.Duration) -Eigenschaft der [`MediaElement`](xref:Xamarin.Forms.MediaElement). Wenn sich die [`Value`](xref:Xamarin.Forms.Slider.Value) -Eigenschaft des [`Slider`](xref:Xamarin.Forms.Slider) ändert, wird das `ValueChanged`-Ereignis ausgelöst, und der `OnPositionSliderValueChanged` Handler wird ausgeführt. Mit diesem Handler wird die [`MediaElement.Position`](xref:Xamarin.Forms.MediaElement.Position) -Eigenschaft auf den Wert der `PositionSlider.Position`-Eigenschaft festgelegt. Wenn Sie die `Slider` ziehen, wird daher die Position der Medienwiedergabe geändert:

[![Screenshot eines Media Element mit einer benutzerdefinierten Positions Leiste unter IOS und Android](mediaelement-images/custom-position-bar.png "Media Element mit einer benutzerdefinierten Positions Leiste")](mediaelement-images/custom-position-bar-large.png#lightbox "Media Element mit einer benutzerdefinierten Positions Leiste")

Außerdem wird ein [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) -Objekt verwendet, um die `PositionSlider` zu deaktivieren, wenn die Medien gepuffert werden. Weitere Informationen zu Daten Triggern finden Sie unter [xamarin. Forms-Trigger](~/xamarin-forms/app-fundamentals/triggers.md).

## <a name="implement-a-custom-volume-control"></a>Implementieren eines benutzerdefinierten volumesteuerelements

Die von jeder Plattform implementierten Steuerelemente zur Medienwiedergabe enthalten eine volumeleiste. Diese Leiste ähnelt einem Schieberegler und zeigt das Volume der Medien an. Außerdem können Sie die volumeleiste bearbeiten, um das Volume zu vergrößern oder zu verkleinern.

Eine benutzerdefinierte volumeleiste kann mithilfe eines [`Slider`](xref:Xamarin.Forms.Slider)implementiert werden, wie im folgenden Beispiel gezeigt:

```xaml
<StackLayout>
    <MediaElement AutoPlay="False"
                  Source="{StaticResource AdvancedAsync}" />
    <Slider Maximum="1.0"
            Minimum="0.0"
            Value="{Binding Volume}"
            Rotation="270"
            WidthRequest="100" />
</StackLayout>
```

In diesem Beispiel bindet der [`Slider`](xref:Xamarin.Forms.Slider) Daten seine `Value`-Eigenschaft an die [`Volume`](xref:Xamarin.Forms.MediaElement.Volume) -Eigenschaft der [`MediaElement`](xref:Xamarin.Forms.MediaElement). Dies ist möglich, da die `Volume`-Eigenschaft eine `TwoWay` Bindung verwendet. Daher wird durch Ändern der `Value` Eigenschaft die `Volume` Eigenschaft geändert.

> [!NOTE]
> Die [`Volume`](xref:Xamarin.Forms.MediaElement.Volume) -Eigenschaft verfügt über einen vlidation-Rückruf, mit dem sichergestellt wird, dass der Wert größer oder gleich 0,0 und kleiner oder gleich 1,0 ist.

Weitere Informationen zur Verwendung eines [`Slider`](xref:Xamarin.Forms.Slider) finden Sie unter [xamarin. Forms Slider.](~/xamarin-forms/user-interface/slider.md)

## <a name="related-links"></a>Verwandte Links

- [Mediaelementdemos (Beispiel)](https://github.com/xamarin/xamarin-forms-samples/tree/pre-release/WorkingWithMediaElement)
- [URI-Schemas](/windows/uwp/app-resources/uri-schemes)
- [Xamarin. Forms-Trigger](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin. Forms-Schieberegler](~/xamarin-forms/user-interface/slider.md)
- [Android: Unterstützte Medienformate](https://developer.android.com/guide/topics/media/media-formats)
- [UWP: unterstützte Codecs](/windows/uwp/audio-video-camera/supported-codecs)
