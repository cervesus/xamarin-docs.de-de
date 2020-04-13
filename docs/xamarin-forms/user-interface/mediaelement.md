---
title: Xamarin.Forms MediaElement
description: In diesem Artikel wird erläutert, wie Sie MediaElement zum Abspielen von Video und Audio in einer Xamarin.Forms-Anwendung verwenden.
ms.prod: xamarin
ms.assetid: e65f1e56-a80d-46c7-9ff4-7ae6650a3165
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/18/2020
ms.openlocfilehash: 6f6c51c428de569ceb09ed6a26cfc36881c86dc5
ms.sourcegitcommit: b93754b220fca3d6e3d131341e3cfbe233d10f84
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2020
ms.locfileid: "80628338"
---
# <a name="xamarinforms-mediaelement"></a>Xamarin.Forms MediaElement

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel](~/media/shared/download.png) herunterladen Download des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-mediaelementdemos/)

[`MediaElement`](xref:Xamarin.Forms.MediaElement)ist eine Ansicht zum Abspielen von Video und Audio. Medien, die von der zugrunde liegenden Plattform unterstützt werden, können aus den folgenden Quellen abgespielt werden:

- Das Web mit einem URI (HTTP oder HTTPS).
- Eine Ressource, die mithilfe des `ms-appx:///` URI-Schemas in die Plattformanwendung eingebettet ist.
- Dateien, die aus den lokalen und temporären `ms-appdata:///` Datenordnern der App stammen, verwenden das URI-Schema.
- Die Bibliothek des Geräts.

[`MediaElement`](xref:Xamarin.Forms.MediaElement)kann die Plattformwiedergabesteuerelemente verwenden, die als Transportsteuerelemente bezeichnet werden. Sie sind jedoch standardmäßig deaktiviert und können durch eigene Transportsteuerelemente ersetzt werden. Die folgenden Screenshots zeigen `MediaElement` die Wiedergabe eines Videos mit den Plattform-Transportsteuerungen:

[![Screenshot eines MediaElements, das ein Video abspielt, auf iOS und Android](mediaelement-images/playback-controls.png "MediaElement, das ein Video wiedergibt")](mediaelement-images/playback-controls-large.png#lightbox "MediaElement, das ein Video wiedergibt")

[`MediaElement`](xref:Xamarin.Forms.MediaElement)ist in Xamarin.Forms 4.5 verfügbar. Es ist jedoch derzeit experimentell und kann nur verwendet werden, indem Sie die folgende Codezeile zu Ihrer *App.xaml.cs-Datei* hinzufügen:

```csharp
Device.SetFlags(new string[]{ "MediaElement_Experimental" });
```

> [!NOTE]
> [`MediaElement`](xref:Xamarin.Forms.MediaElement)ist für iOS, Android, die Universelle Windows-Plattform (UWP), macOS, Windows Presentation Foundation und Tizen verfügbar.

[`MediaElement`](xref:Xamarin.Forms.MediaElement)definiert die folgenden Eigenschaften:

- [`Aspect`](xref:Xamarin.Forms.MediaElement.Aspect), vom [`Aspect`](xref:Xamarin.Forms.Aspect)Typ , bestimmt, wie das Medium an den Anzeigebereich skaliert wird. Der Standardwert dieser Eigenschaft ist `AspectFit`.
- [`AutoPlay`](xref:Xamarin.Forms.MediaElement.AutoPlay), vom `bool`Typ , gibt an, ob [`Source`](xref:Xamarin.Forms.MediaElement.Source) die Medienwiedergabe automatisch beginnt, wenn die Eigenschaft festgelegt wird. Der Standardwert dieser Eigenschaft ist `true`.
- [`BufferingProgress`](xref:Xamarin.Forms.MediaElement.BufferingProgress), vom `double`Typ , gibt den aktuellen Pufferfortschritt an. Der Standardwert dieser Eigenschaft ist 0.0.
- [`CanSeek`](xref:Xamarin.Forms.MediaElement.CanSeek), vom `bool`Typ , gibt an, ob Medien [`Position`](xref:Xamarin.Forms.MediaElement.Position) neu positioniert werden können, indem der Wert der Eigenschaft gesetzt wird. Dies ist eine schreibgeschützte Eigenschaft.
- [`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState), vom [`MediaElementState`](xref:Xamarin.Forms.MediaElementState)Typ , gibt den aktuellen Status des Steuerelements an. Dies ist eine schreibgeschützte Eigenschaft, `MediaElementState.Closed`deren Standardwert ist.
- [`Duration`](xref:Xamarin.Forms.MediaElement.Duration), vom `TimeSpan?`Typ , gibt die Dauer des aktuell geöffneten Mediums an. Dies ist eine schreibgeschützte Eigenschaft, deren Standardwert `null`ist.
- [`IsLooping`](xref:Xamarin.Forms.MediaElement.IsLooping), vom `bool`Typ , beschreibt, ob die aktuell geladene Medienquelle die Wiedergabe von Anfang an nach Erreichen des Endes fortsetzen soll. Der Standardwert dieser Eigenschaft ist `false`.
- [`KeepScreenOn`](xref:Xamarin.Forms.MediaElement.KeepScreenOn), vom `bool`Typ , bestimmt, ob der Gerätebildschirm während der Medienwiedergabe eingeschaltet bleiben soll. Der Standardwert dieser Eigenschaft ist `false`.
- [`Position`](xref:Xamarin.Forms.MediaElement.Position), vom `TimeSpan`Typ , beschreibt den aktuellen Fortschritt während der Wiedergabezeit des Mediums. Der Standardwert dieser Eigenschaft ist `TimeSpan.Zero`.
- [`ShowsPlaybackControls`](xref:Xamarin.Forms.MediaElement.ShowsPlaybackControls), vom `bool`Typ , bestimmt, ob die Plattformen-Wiedergabesteuerelemente angezeigt werden. Der Standardwert dieser Eigenschaft ist `false`.
- [`Source`](xref:Xamarin.Forms.MediaElement.Source), vom [`MediaSource`](xref:Xamarin.Forms.MediaSource)Typ , gibt die Quelle des in das Steuerelement geladenen Mediums an.
- [`VideoHeight`](xref:Xamarin.Forms.MediaElement.VideoHeight), vom `int`Typ , gibt die Höhe des Steuerelements an. Dies ist eine schreibgeschützte Eigenschaft.
- [`VideoWidth`](xref:Xamarin.Forms.MediaElement.VideoWidth), vom `int`Typ , gibt die Breite des Steuerelements an. Dies ist eine schreibgeschützte Eigenschaft.
- [`Volume`](xref:Xamarin.Forms.MediaElement.Volume), vom `double`Typ , bestimmt das Medienvolumen, das auf einer linearen Skala zwischen 0 und 1 dargestellt wird. Diese Eigenschaft `TwoWay` verwendet eine Bindung, und ihr Standardwert ist 1.

Diese Eigenschaften, mit Ausnahme `CanSeek` der Eigenschaft, [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) werden von Objekten unterstützt, was bedeutet, dass sie Ziele von Datenbindungen sein können, und formatiert werden können.

Die [`MediaElement`](xref:Xamarin.Forms.MediaElement) Klasse definiert außerdem vier Ereignisse:

- [`MediaOpened`](xref:Xamarin.Forms.MediaElement.MediaOpened)wird ausgelöst, wenn der Medienstream validiert und geöffnet wurde.
- [`MediaEnded`](xref:Xamarin.Forms.MediaElement.MediaEnded)wird ausgelöst, `MediaElement` wenn die Wiedergabe der Medien beendet wird.
- [`MediaFailed`](xref:Xamarin.Forms.MediaElement.MediaFailed)wird ausgelöst, wenn der Medienquelle ein Fehler zugeordnet ist.
- [`SeekCompleted`](xref:Xamarin.Forms.MediaElement.SeekCompleted)wird ausgelöst, wenn der Suchpunkt eines angeforderten Suchvorgangs für die Wiedergabe bereit ist.

Umfasst darüber [`MediaElement`](xref:Xamarin.Forms.MediaElement) [`Play`](xref:Xamarin.Forms.MediaElement.Play)hinaus [`Pause`](xref:Xamarin.Forms.MediaElement.Pause), [`Stop`](xref:Xamarin.Forms.MediaElement.Stop) und Methoden.

Informationen zu unterstützten Medienformaten auf Android finden Sie unter [Unterstützte Medienformate](https://developer.android.com/guide/topics/media/media-formats) auf developer.android.com. Informationen zu unterstützten Medienformaten auf der universellen Windows-Plattform (UWP) finden Sie unter [Unterstützte Codecs](/windows/uwp/audio-video-camera/supported-codecs).

## <a name="play-remote-media"></a>Wiedergabe von Remotemedien

A [`MediaElement`](xref:Xamarin.Forms.MediaElement) kann Remotemediendateien mithilfe der HTTP- und HTTPS-URI-Schemata wiedergeben. Dies wird erreicht, indem die [`Source`](xref:Xamarin.Forms.MediaElement.Source) Eigenschaft auf den URI der Mediendatei gesetzt wird:

```xaml
<MediaElement Source="https://sec.ch9.ms/ch9/5d93/a1eab4bf-3288-4faf-81c4-294402a85d93/XamarinShow_mid.mp4"
              ShowsPlaybackControls="True" />
```

Standardmäßig werden die Medien, die [`Source`](xref:Xamarin.Forms.MediaElement.Source) von der Eigenschaft definiert werden, unmittelbar nach dem Öffnen des Mediums wiedergegeben. Um die automatische Medienwiedergabe [`AutoPlay`](xref:Xamarin.Forms.MediaElement.AutoPlay) zu `false`unterdrücken, legen Sie die Eigenschaft auf .

Medienwiedergabesteuerelemente sind standardmäßig deaktiviert und werden [`ShowsPlaybackControls`](xref:Xamarin.Forms.MediaElement.ShowsPlaybackControls) aktiviert, `true`indem die Eigenschaft auf . [`MediaElement`](xref:Xamarin.Forms.MediaElement)verwendet dann die Plattformwiedergabesteuerung.

## <a name="play-local-media"></a>Lokale Medien abspielen

Lokale Medien können aus den folgenden Quellen abgespielt werden:

- Eine Ressource, die mithilfe des `ms-appx:///` URI-Schemas in die Plattformanwendung eingebettet ist.
- Dateien, die aus den lokalen und temporären `ms-appdata:///` Datenordnern der App stammen, verwenden das URI-Schema.
- Die Bibliothek des Geräts.

Weitere Informationen zu diesen URI-Schemas finden Sie unter [URI-Schemas](/windows/uwp/app-resources/uri-schemes).

### <a name="play-media-embedded-in-the-app-package"></a>Wiedergabe von Medien, die in das App-Paket eingebettet sind

A [`MediaElement`](xref:Xamarin.Forms.MediaElement) kann Mediendateien wiedergeben, die im `ms-appx:///` App-Paket eingebettet sind, und das URI-Schema verwenden. Mediendateien werden in das App-Paket eingebettet, indem sie im Plattformprojekt platziert werden.

Das Speichern einer Mediendatei im Plattformprojekt unterscheidet sich für jede Plattform:

- Unter iOS müssen Mediendateien im Ordner **Ressourcen** oder in einem Unterordner des **Ordners Ressourcen** gespeichert werden. Die Mediendatei muss `Build Action` `BundleResource`über eine von verfügen.
- Unter Android müssen Mediendateien in einem Unterordner von **Resources** mit dem Namen **raw**gespeichert werden. Der Ordner **raw** darf keine Unterordner enthalten. Die Mediendatei muss `Build Action` `AndroidResource`über eine von verfügen.
- Auf UWP können Mediendateien in jedem Ordner im Projekt gespeichert werden. Die Mediendatei muss `BuildAction` `Content`über eine von verfügen.

Mediendateien, die diese Kriterien erfüllen, `ms-appx:///` können dann mit dem URI-Schema wiedergegeben werden:

```xaml
<MediaElement Source="ms-appx:///XamarinForms101UsingEmbeddedImages.mp4"
              ShowsPlaybackControls="True" />
```

Bei Verwendung der Datenbindung kann ein Wertkonverter verwendet werden, um dieses URI-Schema anzuwenden:

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

Eine Instanz `VideoSourceConverter` des kann dann verwendet `ms-appx:///` werden, um das URI-Schema auf eine eingebettete Mediendatei anzuwenden:

```xaml
<MediaElement Source="{Binding MediaSource, Converter={StaticResource VideoSourceConverter}}"
              ShowsPlaybackControls="True" />
```

Weitere Informationen zum MS-appx-URI-Schema finden Sie unter [ms-appx und ms-appx-web](/windows/uwp/app-resources/uri-schemes#ms-appx-and-ms-appx-web).

### <a name="play-media-from-the-apps-local-and-temporary-folders"></a>Wiedergabe von Medien aus den lokalen und temporären Ordnern der App

A [`MediaElement`](xref:Xamarin.Forms.MediaElement) kann Mediendateien wiedergeben, die mithilfe des `ms-appdata:///` URI-Schemas in die lokalen oder temporären Datenordner der App kopiert werden.

Das folgende Beispiel [`Source`](xref:Xamarin.Forms.MediaElement.Source) zeigt den Eigenschaftssatz für eine Mediendatei, die im lokalen Datenordner der App gespeichert ist:

```xaml
<MediaElement Source="ms-appdata:///local/XamarinVideo.mp4"
              ShowsPlaybackControls="True" />
```

Das folgende Beispiel [`Source`](xref:Xamarin.Forms.MediaElement.Source) zeigt die Eigenschaft für eine Mediendatei, die im temporären Datenordner der App gespeichert ist:

```xaml
<MediaElement Source="ms-appdata:///temp/XamarinVideo.mp4"
              ShowsPlaybackControls="True" />
```

> [!IMPORTANT]
> Zusätzlich zur Wiedergabe von Mediendateien, die in den lokalen oder temporären Datenordnern der App gespeichert sind, kann UWP auch Mediendateien wiedergeben, die sich im Roamingordner der App befinden. Dies kann erreicht werden, indem `ms-appdata:///roaming/`der Mediendatei mit vorangegeben wird.

Bei Verwendung der Datenbindung kann ein Wertkonverter verwendet werden, um dieses URI-Schema anzuwenden:

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

Eine Instanz `VideoSourceConverter` von der kann dann `ms-appdata:///` verwendet werden, um das URI-Schema auf eine Mediendatei im lokalen oder temporären Datenordner der App anzuwenden:

```xaml
<MediaElement Source="{Binding MediaSource, Converter={StaticResource VideoSourceConverter}}"
              ShowsPlaybackControls="True" />
```

Weitere Informationen zum MS-appdata-URI-Schema finden Sie unter [ms-appdata](/windows/uwp/app-resources/uri-schemes#ms-appdata).

#### <a name="copying-a-media-file-to-the-apps-local-or-temporary-data-folder"></a>Kopieren einer Mediendatei in den lokalen oder temporären Datenordner der App

Wenn Sie eine Mediendatei wiedergeben, die im lokalen oder temporären Datenordner der App gespeichert ist, muss die Mediendatei dort von der App kopiert werden. Dies kann z. B. durch Kopieren einer Mediendatei aus dem App-Paket erreicht werden:

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
> Im obigen Codebeispiel wird die `FileSystem` in Xamarin.Essentials enthaltene Klasse verwendet. Weitere Informationen finden Sie unter [Xamarin.Essentials: File System Helpers](~/essentials/file-system-helpers.md?context=xamarin%2Fxamarin-forms&tabs=android).

### <a name="play-media-from-the-device-library"></a>Wiedergabe von Medien aus der Gerätebibliothek

Die meisten modernen Mobilgeräte und Desktop-Computer können Videos und Audio mit der Kamera und dem Mikrofon des Geräts aufnehmen. Die erstellten Medien werden dann als Dateien auf dem Gerät gespeichert. Diese Dateien können aus der Bibliothek [`MediaElement`](xref:Xamarin.Forms.MediaElement)abgerufen und von der abgespielt werden.

Jede der Plattformen enthält eine Funktion, mit der der Benutzer Medien aus der Gerätebibliothek auswählen kann. In Xamarin.Forms können Plattformprojekte diese Funktionalität aufrufen und [`DependencyService`](xref:Xamarin.Forms.DependencyService) von der Klasse aufrufen.

Der in der Beispielanwendung verwendete Abhängigkeitsdienst für die Videoauswahl ist dem in [Der Auswahl eines Fotos aus der Bildbibliothek](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)definierten sehr ähnlich, mit der Ausnahme, dass die Auswahl einen Dateinamen anstelle eines `Stream` Objekts zurückgibt. Das Projekt für freigegebenen `IVideoPicker`Code definiert eine Schnittstelle `GetVideoFileAsync`mit dem Namen , die eine einzelne Methode mit dem Namen definiert. Jede Plattform implementiert diese Schnittstelle `VideoPicker` dann in einer Klasse.

Das folgende Codebeispiel zeigt, wie eine Mediendatei aus der Gerätebibliothek abgerufen wird:

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

Der Videoauswahlabhängigkeitsdienst wird aufgerufen, indem die `DependencyService.Get` Methode `IVideoPicker` aufgerufen wird, um die Implementierung einer Schnittstelle im Plattformprojekt abzurufen. Die `GetVideoFileAsync` Methode wird dann für diese Instanz aufgerufen, und [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource) der zurückgegebene Dateiname [`Source`](xref:Xamarin.Forms.MediaElement.Source) wird [`MediaElement`](xref:Xamarin.Forms.MediaElement)verwendet, um ein Objekt zu erstellen und es auf die Eigenschaft der festzulegen.

## <a name="change-video-aspect-ratio"></a>Ändern des Video-Seitenverhältnisses

Die [`Aspect`](xref:Xamarin.Forms.MediaElement.Aspect) Eigenschaft bestimmt, wie Videomedien an den Anzeigebereich skaliert werden. Standardmäßig ist diese Eigenschaft auf `AspectFit` den Enumerationsmember festgelegt, kann jedoch [`Aspect`](xref:Xamarin.Forms.Aspect) auf alle Enumerationsmember festgelegt werden:

- `AspectFit`gibt an, dass das Video bei Bedarf in den Anzeigebereich passt, wobei das Seitenverhältnis beibehalten wird.
- `AspectFill`gibt an, dass das Video abgeschnitten wird, sodass es den Anzeigebereich ausfüllt, während das Seitenverhältnis beibehalten wird.
- `Fill`zeigt an, dass das Video gestreckt wird, um den Anzeigebereich zu füllen.

## <a name="poll-for-position-data"></a>Umfrage für Positionsdaten

Die Eigenschaftsänderungsbenachrichtigung für die [`Position`](xref:Xamarin.Forms.MediaElement.Position) bindbare Eigenschaft wird nur in Schlüsselmomenten wie Demastbeginn und -ende ausgelöst, und es tritt eine Pause ein. Daher liefert die `Position` Datenbindung an die Eigenschaft keine genauen Positionsdaten. Stattdessen müssen Sie einen Timer einrichten und die Eigenschaft abwählen.

Ein guter Ort, um `OnAppearing` dies zu tun, ist in der Außerkraftsetzung für die Seite, die die Positionsdaten erfordert, wie Medien wiedergegeben werden:

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

In diesem Beispiel `OnAppearing` startet die Außerkraftsetzung `Position` einen Timer, der jede Sekunde mit dem Wert aktualisiert `positionLabel` wird. Der Timerrückruf wird jede Sekunde aufgerufen, bis `false`der Rückruf zurückkehrt. Wenn die Seitennavigation stattfindet, wird die `OnDisappearing` Außerkraftsetzung ausgeführt, wodurch verhindert wird, dass der Timerrückruf aufgerufen wird.

## <a name="understand-mediasource-types"></a>Verstehen von MediaSource-Typen

A [`MediaElement`](xref:Xamarin.Forms.MediaElement) kann Medien wiedergeben, indem seine [`Source`](xref:Xamarin.Forms.MediaElement.Source) Eigenschaft auf eine Remote- oder lokale Mediendatei gesetzt wird. Die `Source` Eigenschaft ist [`MediaSource`](xref:Xamarin.Forms.MediaSource)vom Typ , und diese Klasse definiert zwei statische Methoden:

- [`FromFile`](xref:Xamarin.Forms.MediaSource.FromFile*), gibt [`MediaSource`](xref:Xamarin.Forms.MediaSource) eine `string` Instanz aus einem Argument zurück.
- [`FromUri`](xref:Xamarin.Forms.MediaSource.FromUri*), gibt [`MediaSource`](xref:Xamarin.Forms.MediaSource) eine `Uri` Instanz aus einem Argument zurück.

Darüber hinaus [`MediaSource`](xref:Xamarin.Forms.MediaSource) verfügt die Klasse auch `MediaSource` über `string` `Uri` implizite Operatoren, von denen Instanzen und Argumente zurückgegeben werden.

> [!NOTE]
> Wenn [`Source`](xref:Xamarin.Forms.MediaElement.Source) die Eigenschaft in XAML festgelegt ist, wird [`MediaSource`](xref:Xamarin.Forms.MediaSource) ein Typkonverter aufgerufen, um eine Instanz von einem `string` oder `Uri`zurückzugeben.

Die [`MediaSource`](xref:Xamarin.Forms.MediaSource) Klasse hat außerdem zwei abgeleitete Klassen:

- [`UriMediaSource`](xref:Xamarin.Forms.UriMediaSource), die verwendet wird, um eine Remotemediendatei aus einem URI anzugeben. Diese Klasse [`Uri`](xref:Xamarin.Forms.UriMediaSource.Uri) verfügt über eine Eigenschaft, die auf eine `Uri`festgelegt werden kann.
- [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource), die verwendet wird, um eine `string`lokale Mediendatei aus einer anzugeben. Diese Klasse [`File`](xref:Xamarin.Forms.FileMediaSource.File) verfügt über eine Eigenschaft, die auf eine `string`festgelegt werden kann. Darüber hinaus verfügt diese Klasse über `string` implizite Operatoren zum Konvertieren eines in ein `FileMediaSource` Objekt und ein `FileMediaSource` Objekt in eine `string`.

> [!NOTE]
> Wenn [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource) ein Objekt in XAML erstellt wird, wird [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource) ein Typkonverter aufgerufen, um eine Instanz von einer `string`zurückzugeben.

## <a name="determine-mediaelement-status"></a>Bestimmen des MediaElement-Status

Die [`MediaElement`](xref:Xamarin.Forms.MediaElement) Klasse definiert eine schreibgeschützte bindbare Eigenschaft mit dem Namen [`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState), vom Typ [`MediaElementState`](xref:Xamarin.Forms.MediaElementState). Diese Eigenschaft gibt den aktuellen Status des Steuerelements an, z. B. ob das Medium wiedergegeben oder angehalten wird oder ob es noch nicht bereit ist, die Medien wiederzugeben.

Die [`MediaElementState`](xref:Xamarin.Forms.MediaElementState) Enumeration definiert die folgenden Elemente:

- `Closed`gibt an, dass der `MediaElement` keine Medien enthält.
- `Opening`gibt an, dass der die `MediaElement` angegebene Quelle validiert und versucht, sie zu laden.
- `Buffering`gibt an, dass das `MediaElement` Medium für die Wiedergabe geladen wird. Seine [`Position`](xref:Xamarin.Forms.MediaElement.Position) Eigenschaft schreitet während dieses Zustands nicht voran. Wenn `MediaElement` das Video abgespielt wurde, wird weiterhin der zuletzt angezeigte Frame angezeigt.
- `Playing`gibt an, dass die `MediaElement` Medienquelle abgespielt wird.
- `Paused`zeigt an, dass `MediaElement` [`Position`](xref:Xamarin.Forms.MediaElement.Position) der seine Eigenschaft nicht voranbringt. Wenn `MediaElement` das Video abgespielt wurde, wird der aktuelle Frame weiterhin angezeigt.
- `Stopped`gibt an, dass das `MediaElement` Medienmedium enthält, aber nicht abgespielt oder angehalten wird. Seine [`Position`](xref:Xamarin.Forms.MediaElement.Position) Eigenschaft ist 0 und nicht voran. Wenn es sich bei `MediaElement` dem geladenen Medium um Einvideo handelt, wird der erste Frame angezeigt.

Es ist in der Regel [`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState) nicht erforderlich, die Eigenschaft zu untersuchen, wenn die [`MediaElement`](xref:Xamarin.Forms.MediaElement) Transportsteuerungen verwendet werden. Diese Eigenschaft wird jedoch beim Implementieren eigener Transportkontrollen wichtig.

## <a name="implement-custom-transport-controls"></a>Implementieren benutzerdefinierter Transportsteuerungen

Die Transportsteuerungen eines Media Players umfassen die Schaltflächen, die die Funktionen **Wiedergabe**, **Pause**und **Stopp**ausführen. In der Regel werden diese Schaltflächen mit bekannten Symbolen anstelle von Text dargestellt, und die Funktionen **Wiedergabe** und **Pause** werden in eine Schaltfläche kombiniert.

Standardmäßig sind [`MediaElement`](xref:Xamarin.Forms.MediaElement) die Wiedergabesteuerelemente deaktiviert. Auf diese Weise `MediaElement` können Sie die programmgesteuerte Steuerung oder durch die Bereitstellung eigener Transportsteuerungen steuern. Zur Unterstützung `MediaElement` dieses, [`Play`](xref:Xamarin.Forms.MediaElement.Play) [`Pause`](xref:Xamarin.Forms.MediaElement.Pause)enthält [`Stop`](xref:Xamarin.Forms.MediaElement.Stop) , und Methoden.

Das folgende XAML-Beispiel zeigt [`MediaElement`](xref:Xamarin.Forms.MediaElement) eine Seite, die ein und benutzerdefinierte Transportsteuerelemente enthält:

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

In diesem Beispiel werden die benutzerdefinierten Transportsteuerelemente als [`Button`](xref:Xamarin.Forms.Button) Objekte definiert. Es gibt jedoch `Button` nur zwei Objekte, wobei das `Button` erste `Button` **Play** und **Pause**und das zweite **Stop**darstellt. [`DataTrigger`](xref:Xamarin.Forms.DataTrigger)Objekte werden verwendet, um die Schaltflächen zu aktivieren und zu deaktivieren und um die erste Schaltfläche zwischen **Wiedergabe** und **Pause**zu wechseln. Weitere Informationen zu Datentriggern finden Sie unter [Xamarin.Forms Triggers](~/xamarin-forms/app-fundamentals/triggers.md).

Die CodeBehind-Datei enthält die [`Clicked`](xref:Xamarin.Forms.Button.Clicked) Handler für die Ereignisse:

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

Die **Play-Taste** kann gedrückt werden, sobald sie aktiviert wird, um die Wiedergabe zu beginnen:

[![Screenshot eines MediaElements mit benutzerdefinierten Transportsteuerungen auf iOS und Android](mediaelement-images/custom-transport-playback.png "MediaElement, das ein Video wiedergibt")](mediaelement-images/custom-transport-playback-large.png#lightbox "MediaElement, das ein Video wiedergibt")

Wenn Sie die **Pause-Taste** drücken, wird die Wiedergabe angehalten:

[![Screenshot eines MediaElements mit angehaltener Wiedergabe, auf iOS und Android](mediaelement-images/custom-transport-paused.png "MediaElement mit angehaltenem Video")](mediaelement-images/custom-transport-paused-large.png#lightbox "MediaElement mit angehaltenem Video")

Durch Drücken der **Stopp-Taste** wird die Wiedergabe beendet und die Position der Mediendatei an den Anfang zurück.

## <a name="implement-a-custom-position-bar"></a>Implementieren einer benutzerdefinierten Positionsleiste

Die von jeder Plattform implementierten Transportsteuerelemente umfassen auch eine Fortschrittsleiste. Dieser Balken ähnelt einem Schieberegler und zeigt die aktuelle Position des Mediums innerhalb seiner Gesamtdauer an. Darüber hinaus können Sie die Positionsleiste bearbeiten, um vorwärts oder rückwärts zu einer neuen Position im Video zu wechseln.

Das Implementieren einer benutzerdefinierten Positionsleiste erfordert die Kenntnis der Dauer des Mediums und der aktuellen Wiedergabeposition. Diese Daten sind [`Duration`](xref:Xamarin.Forms.MediaElement.Duration) in [`Position`](xref:Xamarin.Forms.MediaElement.Position) der und in den Eigenschaften verfügbar.

> [!IMPORTANT]
> Die [`Position`](xref:Xamarin.Forms.MediaElement.Position) müssen abgefragt werden, um genaue Positionsdaten zu erhalten. Weitere Informationen finden Sie unter [Umfrage für Positionsdaten](#poll-for-position-data).

Eine benutzerdefinierte Positionsleiste kann [`Slider`](xref:Xamarin.Forms.Slider)mithilfe einer implementiert werden, wie im folgenden Beispiel gezeigt:

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

Die `PositionSlider` Klasse definiert `Duration` ihre `Position` eigenen und bindbaren Eigenschaften sowie eine `TimeToEnd` bindbare Eigenschaft. Alle drei Eigenschaften `TimeSpan`sind vom Typ . Der Property-Changed-Handler `Duration` für `Maximum` die Eigenschaft [`Slider`](xref:Xamarin.Forms.Slider) legt `TotalSeconds` die `TimeSpan` Eigenschaft des wert es für die Eigenschaft fest. Die `TimeToEnd` Eigenschaft wird basierend auf `Duration` `Position` Änderungen an der und Eigenschaften berechnet und beginnt bei der Dauer des Mediums und verringert sich im Verlauf der Wiedergabe auf Null.

Der `PositionSlider` wird vom [`Slider`](xref:Xamarin.Forms.Slider) Basiswert `Slider` aktualisiert, wenn der verschoben wird, um anzuzeigen, dass das Medium erweitert oder an eine neue Position umgekehrt werden soll. Dies wird `PropertyChanged` im Handler `PositionSlider` im Konstruktor erkannt. Der Handler prüft nach Änderungen an der `Value`-Eigenschaft. Wenn sie sich von der `Position`-Eigenschaft unterscheidet, wird die `Position`-Eigenschaft über die `Value`-Eigenschaft festgelegt. Weitere Informationen zur [`Slider`](xref:Xamarin.Forms.Slider) Verwendung eines weiteren Informationen finden Sie unter [Xamarin.Forms Slider](~/xamarin-forms/user-interface/slider.md)

> [!NOTE]
> Auf Android [`Slider`](xref:Xamarin.Forms.Slider) hat die einzige 1000 diskrete `Minimum` `Maximum` Schritte, unabhängig von der und Einstellungen. Wenn die Medienlänge größer als 1000 `Position` Sekunden ist, entsprechen `Value` zwei `Slider`unterschiedliche Werte dem gleichen Wert der . Aus diesem Grund überprüft der obige Code, ob die neue Position und die vorhandene Position größer als ein Hundertstel der Gesamtdauer sind.

Das folgende Beispiel `PositionSlider` zeigt das Verbrauchte auf einer Seite:

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

In diesem Beispiel `Duration` `PositionSlider` ist die Eigenschaft des [`Duration`](xref:Xamarin.Forms.MediaElement.Duration) an die [`MediaElement`](xref:Xamarin.Forms.MediaElement)Eigenschaft der gebunden. Wenn [`Value`](xref:Xamarin.Forms.Slider.Value) die Eigenschaft [`Slider`](xref:Xamarin.Forms.Slider) der `ValueChanged` Änderungen, das `OnPositionSliderValueChanged` Ereignis ausgelöst wird und der Handler ausgeführt wird. Dieser Handler [`MediaElement.Position`](xref:Xamarin.Forms.MediaElement.Position) legt die Eigenschaft `PositionSlider.Position` auf den Wert der Eigenschaft fest. Ziehen Sie `Slider` daher die Ergebnisse in der Medienwiedergabeposition, wenn Sie sich ändern:

[![Screenshot eines MediaElements mit benutzerdefinierter Positionsleiste unter iOS und Android](mediaelement-images/custom-position-bar.png "MediaElement mit benutzerdefinierter Positionsleiste")](mediaelement-images/custom-position-bar-large.png#lightbox "MediaElement mit benutzerdefinierter Positionsleiste")

Darüber hinaus [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) wird ein Objekt `PositionSlider` verwendet, um die zu deaktivieren, wenn das Medium gepuffert wird. Weitere Informationen zu Datentriggern finden Sie unter [Xamarin.Forms Triggers](~/xamarin-forms/app-fundamentals/triggers.md).

## <a name="implement-a-custom-volume-control"></a>Implementieren einer benutzerdefinierten Lautstärkeregelung

Medienwiedergabesteuerelemente, die von jeder Plattform implementiert werden, enthalten eine Lautstärkeleiste. Dieser Balken ähnelt einem Schieberegler und zeigt die Lautstärke des Mediums an. Darüber hinaus können Sie die Lautstärkeleiste bearbeiten, um die Lautstärke zu erhöhen oder zu verringern.

Eine benutzerdefinierte Volumeleiste kann [`Slider`](xref:Xamarin.Forms.Slider)mithilfe einer implementiert werden, wie im folgenden Beispiel gezeigt:

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

In diesem Beispiel [`Slider`](xref:Xamarin.Forms.Slider) bindet die `Value` Daten [`Volume`](xref:Xamarin.Forms.MediaElement.Volume) ihre Eigenschaft [`MediaElement`](xref:Xamarin.Forms.MediaElement)an die Eigenschaft der . Dies ist `Volume` möglich, `TwoWay` da die Eigenschaft eine Bindung verwendet. Daher führt `Value` das Ändern der `Volume` Eigenschaft zu einer Änderung der Eigenschaft.

> [!NOTE]
> Die [`Volume`](xref:Xamarin.Forms.MediaElement.Volume) Eigenschaft verfügt über einen vlidation-Rückruf, der sicherstellt, dass ihr Wert größer oder gleich 0,0 und kleiner oder gleich 1,0 ist.

Weitere Informationen zur [`Slider`](xref:Xamarin.Forms.Slider) Verwendung eines weiteren Informationen finden Sie unter [Xamarin.Forms Slider](~/xamarin-forms/user-interface/slider.md)

## <a name="related-links"></a>Verwandte Links

- [MediaElementDemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-mediaelementdemos/)
- [URI-Schemas](/windows/uwp/app-resources/uri-schemes)
- [Xamarin.Forms-Trigger](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.Forms Schieberegler](~/xamarin-forms/user-interface/slider.md)
- [Android: Unterstützte Medienformate](https://developer.android.com/guide/topics/media/media-formats)
- [UWP: Unterstützte Codecs](/windows/uwp/audio-video-camera/supported-codecs)
