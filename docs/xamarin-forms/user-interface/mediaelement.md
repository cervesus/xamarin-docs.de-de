---
title: Xamarin.FormsMedia Element
description: In diesem Artikel wird erläutert, wie Media Element verwendet wird, um Videos und Audiodaten in einer-Anwendung wiederzugeben Xamarin.Forms .
ms.prod: xamarin
ms.assetid: e65f1e56-a80d-46c7-9ff4-7ae6650a3165
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/18/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1dfa51177bba3ebf1e3e29208cc926c77567a048
ms.sourcegitcommit: c000c0ed15b7b2ef2a8f46a39171e11b6d9f8a5d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84980102"
---
# <a name="xamarinforms-mediaelement"></a>Xamarin.FormsMedia Element

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-mediaelementdemos/)

[`MediaElement`](xref:Xamarin.Forms.MediaElement)ist eine Ansicht für die Wiedergabe von Video-und Audiodaten. Medien, die von der zugrunde liegenden Plattform unterstützt werden, können aus den folgenden Quellen wiedergegeben werden:

- Das Web unter Verwendung eines URIs (http oder HTTPS).
- Eine in die Platt Form Anwendung eingebettete Ressource, die das `ms-appx:///` URI-Schema verwendet.
- Dateien, die aus den lokalen und temporären Daten Ordnern der APP stammen, mithilfe des `ms-appdata:///` URI-Schemas.
- Die Bibliothek des Geräts.

[`MediaElement`](xref:Xamarin.Forms.MediaElement)kann die Steuerelemente für die Platt Form Wiedergabe verwenden, die als Transport Steuerelemente bezeichnet werden. Sie sind jedoch standardmäßig deaktiviert und können durch ihre eigenen Transport Steuerelemente ersetzt werden. Die folgenden Screenshots zeigen, wie Sie `MediaElement` ein Video mit den Steuerelementen der Plattform-Transport abspielen:

[![Screenshot eines Media Element-Videos unter IOS und Android](mediaelement-images/playback-controls.png "MediaElement, das ein Video abspielt")](mediaelement-images/playback-controls-large.png#lightbox "MediaElement, das ein Video abspielt")

[`MediaElement`](xref:Xamarin.Forms.MediaElement)ist in Xamarin.Forms 4,5 verfügbar. Es ist jedoch zurzeit experimentell und kann nur verwendet werden, indem der *app.XAML.cs* -Datei die folgende Codezeile hinzugefügt wird:

```csharp
Device.SetFlags(new string[]{ "MediaElement_Experimental" });
```

> [!NOTE]
> [`MediaElement`](xref:Xamarin.Forms.MediaElement)ist unter IOS, Android, den universelle Windows-Plattform (UWP), macOS, Windows Presentation Foundation und tizen verfügbar.

[`MediaElement`](xref:Xamarin.Forms.MediaElement)definiert die folgenden Eigenschaften:

- [`Aspect`](xref:Xamarin.Forms.MediaElement.Aspect)[`Aspect`](xref:Xamarin.Forms.Aspect)bestimmt, wie die Medien skaliert werden, um dem Anzeigebereich anzupassen. Der Standardwert dieser Eigenschaft ist `AspectFit`.
- [`AutoPlay`](xref:Xamarin.Forms.MediaElement.AutoPlay)gibt mit dem Typ an `bool` , ob die Medienwiedergabe automatisch beginnt, wenn die- [`Source`](xref:Xamarin.Forms.MediaElement.Source) Eigenschaft festgelegt wird. Der Standardwert dieser Eigenschaft ist `true`.
- [`BufferingProgress`](xref:Xamarin.Forms.MediaElement.BufferingProgress)`double`gibt den aktuellen Puffer Status des Typs an. Der Standardwert dieser Eigenschaft ist 0,0.
- [`CanSeek`](xref:Xamarin.Forms.MediaElement.CanSeek)Gibt an, `bool` ob Medien durch Festlegen des Werts der-Eigenschaft neu positioniert werden können [`Position`](xref:Xamarin.Forms.MediaElement.Position) . Dies ist eine schreibgeschützte Eigenschaft.
- [`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState)[`MediaElementState`](xref:Xamarin.Forms.MediaElementState)gibt den aktuellen Status des Steuer Elements vom Typ an. Dies ist eine schreibgeschützte Eigenschaft, deren Standardwert ist `MediaElementState.Closed` .
- [`Duration`](xref:Xamarin.Forms.MediaElement.Duration)`TimeSpan?`gibt die Dauer des aktuell geöffneten Mediums an. Dies ist eine schreibgeschützte Eigenschaft, deren Standardwert ist `null` .
- [`IsLooping`](xref:Xamarin.Forms.MediaElement.IsLooping)Gibt an, `bool` ob die derzeit geladene Medienquelle die Wiedergabe von Anfang an wieder aufnehmen soll, nachdem das Ende erreicht wurde. Der Standardwert dieser Eigenschaft ist `false`.
- [`KeepScreenOn`](xref:Xamarin.Forms.MediaElement.KeepScreenOn)`bool`bestimmt, ob der Bildschirm des Geräts während der Medienwiedergabe eingeschaltet bleiben soll. Der Standardwert dieser Eigenschaft ist `false`.
- [`Position`](xref:Xamarin.Forms.MediaElement.Position), vom Typ `TimeSpan` , beschreibt den aktuellen Fortschritt durch die Wiedergabezeit des Mediums. Der Standardwert dieser Eigenschaft ist `TimeSpan.Zero`.
- [`ShowsPlaybackControls`](xref:Xamarin.Forms.MediaElement.ShowsPlaybackControls)legt vom Typ fest `bool` , ob die Plattformen Wiedergabe Steuerelemente angezeigt werden. Der Standardwert dieser Eigenschaft ist `false`.
- [`Source`](xref:Xamarin.Forms.MediaElement.Source)[`MediaSource`](xref:Xamarin.Forms.MediaSource)gibt die Quelle des in das-Steuerelement geladenen Mediums an.
- [`VideoHeight`](xref:Xamarin.Forms.MediaElement.VideoHeight)`int`gibt die Höhe des Steuer Elements vom Typ an. Dies ist eine schreibgeschützte Eigenschaft.
- [`VideoWidth`](xref:Xamarin.Forms.MediaElement.VideoWidth)`int`gibt die Breite des-Steuer Elements vom Typ an. Dies ist eine schreibgeschützte Eigenschaft.
- [`Volume`](xref:Xamarin.Forms.MediaElement.Volume), vom Typ `double` , bestimmt das Volume des Mediums, das auf einer linearen Skala zwischen 0 und 1 dargestellt wird. Diese Eigenschaft verwendet eine `TwoWay` Bindung, und ihr Standardwert ist 1.

Diese Eigenschaften, mit Ausnahme der- `CanSeek` Eigenschaft, werden von-Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Die- [`MediaElement`](xref:Xamarin.Forms.MediaElement) Klasse definiert außerdem vier Ereignisse:

- [`MediaOpened`](xref:Xamarin.Forms.MediaElement.MediaOpened)wird ausgelöst, wenn der Mediendaten Strom überprüft und geöffnet wurde.
- [`MediaEnded`](xref:Xamarin.Forms.MediaElement.MediaEnded)wird ausgelöst, wenn die wieder `MediaElement` Gabe der Medien beendet.
- [`MediaFailed`](xref:Xamarin.Forms.MediaElement.MediaFailed)wird ausgelöst, wenn der Medienquelle ein Fehler zugeordnet ist.
- [`SeekCompleted`](xref:Xamarin.Forms.MediaElement.SeekCompleted)wird ausgelöst, wenn der Suchpunkt eines angeforderten Such Vorgangs für die Wiedergabe bereit ist.

Außerdem enthält die [`MediaElement`](xref:Xamarin.Forms.MediaElement) [`Play`](xref:Xamarin.Forms.MediaElement.Play) Methoden, [`Pause`](xref:Xamarin.Forms.MediaElement.Pause) und [`Stop`](xref:Xamarin.Forms.MediaElement.Stop) .

Informationen zu den unterstützten Medienformaten unter Android finden Sie [unter Unterstützte Medienformate](https://developer.android.com/guide/topics/media/media-formats) auf Developer.Android.com. Informationen zu den unterstützten Medienformaten im universelle Windows-Plattform (UWP) finden Sie [unter unterstützte Codecs](/windows/uwp/audio-video-camera/supported-codecs).

## <a name="play-remote-media"></a>Wiedergeben von Remote Medien

Eine [`MediaElement`](xref:Xamarin.Forms.MediaElement) kann Remote Mediendateien mit den HTTP-und HTTPS-URI-Schemas wiedergeben. Dies wird erreicht, indem die- [`Source`](xref:Xamarin.Forms.MediaElement.Source) Eigenschaft auf den URI der Mediendatei festgelegt wird:

```xaml
<MediaElement Source="https://sec.ch9.ms/ch9/5d93/a1eab4bf-3288-4faf-81c4-294402a85d93/XamarinShow_mid.mp4"
              ShowsPlaybackControls="True" />
```

Standardmäßig wird das von der-Eigenschaft definierte Medium [`Source`](xref:Xamarin.Forms.MediaElement.Source) sofort nach dem Öffnen des Mediums abgespielt. Legen Sie die-Eigenschaft auf fest, um die automatische Medienwiedergabe zu unterdrücken [`AutoPlay`](xref:Xamarin.Forms.MediaElement.AutoPlay) `false`

Steuerelemente für die Medienwiedergabe werden standardmäßig deaktiviert und aktiviert, indem die-Eigenschaft auf festgelegt wird [`ShowsPlaybackControls`](xref:Xamarin.Forms.MediaElement.ShowsPlaybackControls) `true` . [`MediaElement`](xref:Xamarin.Forms.MediaElement)verwendet dann die Steuerelemente für die Platt Form Wiedergabe.

## <a name="play-local-media"></a>Lokale Medien wiedergeben

Lokale Medien können aus den folgenden Quellen wiedergegeben werden:

- Eine in die Platt Form Anwendung eingebettete Ressource, die das `ms-appx:///` URI-Schema verwendet.
- Dateien, die aus den lokalen und temporären Daten Ordnern der APP stammen, mithilfe des `ms-appdata:///` URI-Schemas.
- Die Bibliothek des Geräts.

Weitere Informationen zu diesen URI-Schemas finden Sie unter [URI-Schemas](/windows/uwp/app-resources/uri-schemes).

### <a name="play-media-embedded-in-the-app-package"></a>Wiedergeben von Medien, die im App-Paket eingebettet sind

Eine [`MediaElement`](xref:Xamarin.Forms.MediaElement) kann Mediendateien wiedergeben, die mit dem URI-Schema in das App-Paket eingebettet sind `ms-appx:///` . Mediendateien werden in das App-Paket eingebettet, indem Sie im Platt Form Projekt platziert werden.

Das Speichern einer Mediendatei im Platt Form Projekt unterscheidet sich für jede Plattform:

- Unter IOS müssen Mediendateien im Ordner " **Resources** " oder in einem Unterordner des Ordners " **Resources** " gespeichert werden. Die Mediendatei muss den-Typ aufweisen `Build Action` `BundleResource` .
- Unter Android müssen Mediendateien in einem Unterordner von **Ressourcen** mit dem Namen **RAW**gespeichert werden. Der Ordner **raw** darf keine Unterordner enthalten. Die Mediendatei muss den-Typ aufweisen `Build Action` `AndroidResource` .
- Bei UWP können Mediendateien in einem beliebigen Ordner im Projekt gespeichert werden. Die Mediendatei muss den-Typ aufweisen `BuildAction` `Content` .

Mediendateien, die diese Kriterien erfüllen, können dann mit dem URI-Schema wiedergegeben werden `ms-appx:///` :

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

Eine Instanz von `VideoSourceConverter` kann dann verwendet werden, um das `ms-appx:///` URI-Schema auf eine eingebettete Mediendatei anzuwenden:

```xaml
<MediaElement Source="{Binding MediaSource, Converter={StaticResource VideoSourceConverter}}"
              ShowsPlaybackControls="True" />
```

Weitere Informationen zum MS-AppX-URI-Schema finden Sie unter [MS-AppX und MS-AppX-Web](/windows/uwp/app-resources/uri-schemes#ms-appx-and-ms-appx-web).

### <a name="play-media-from-the-apps-local-and-temporary-folders"></a>Wiedergeben von Medien aus den lokalen und temporären Ordnern der APP

Eine [`MediaElement`](xref:Xamarin.Forms.MediaElement) kann Mediendateien wiedergeben, die mithilfe des URI-Schemas in die lokalen oder temporären Datenordner der APP kopiert werden `ms-appdata:///` .

Das folgende Beispiel zeigt, wie die [`Source`](xref:Xamarin.Forms.MediaElement.Source) -Eigenschaft auf eine Mediendatei festgelegt ist, die im lokalen Datenordner der APP gespeichert ist:

```xaml
<MediaElement Source="ms-appdata:///local/XamarinVideo.mp4"
              ShowsPlaybackControls="True" />
```

Im folgenden Beispiel wird die- [`Source`](xref:Xamarin.Forms.MediaElement.Source) Eigenschaft für eine Mediendatei gezeigt, die im temporären Datenordner der APP gespeichert ist:

```xaml
<MediaElement Source="ms-appdata:///temp/XamarinVideo.mp4"
              ShowsPlaybackControls="True" />
```

> [!IMPORTANT]
> Zusätzlich zur Wiedergabe von Mediendateien, die in den lokalen oder temporären Daten Ordnern der APP gespeichert sind, kann UWP auch Mediendateien wiedergeben, die sich im roamingordner der APP befinden. Dies kann erreicht werden, indem die Mediendatei mit vorangestellt wird `ms-appdata:///roaming/` .

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

Eine Instanz von `VideoSourceConverter` kann dann verwendet werden, um das `ms-appdata:///` URI-Schema auf eine Mediendatei im lokalen oder temporären Datenordner der APP anzuwenden:

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
> Im obigen Codebeispiel wird die-Klasse verwendet, die `FileSystem` in enthalten ist Xamarin.Essentials . Weitere Informationen finden Sie unter [ Xamarin.Essentials Datei System](~/essentials/file-system-helpers.md?context=xamarin%2Fxamarin-forms&tabs=android)-Hilfsprogramme.

### <a name="play-media-from-the-device-library"></a>Wiedergeben von Medien in der Geräte Bibliothek

Die meisten modernen mobilen Geräte und Desktop Computer können Videos und Audiodaten mithilfe der Kamera und des Mikrofons des Geräts aufzeichnen. Die erstellten Medien werden dann als Dateien auf dem Gerät gespeichert. Diese Dateien können aus der Bibliothek abgerufen und von abgespielt werden [`MediaElement`](xref:Xamarin.Forms.MediaElement) .

Jede der Plattformen enthält eine Funktion, die es dem Benutzer ermöglicht, Medien aus der Bibliothek des Geräts auszuwählen. In Xamarin.Forms können Platt Form Projekte diese Funktion aufrufen und können von der-Klasse aufgerufen werden [`DependencyService`](xref:Xamarin.Forms.DependencyService) .

Der in der Beispielanwendung verwendete Abhängigkeits Dienst für die Video Entnahme ähnelt sehr dem, das in [Auswählen eines Fotos aus der Bildbibliothek](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)definiert ist, mit dem Unterschied, dass die Auswahl einen Dateinamen anstelle eines-Objekts zurückgibt `Stream` . Das freigegebene Code Projekt definiert eine Schnittstelle mit dem Namen `IVideoPicker` , die eine einzelne Methode mit dem Namen definiert `GetVideoFileAsync` . Diese Schnittstelle wird von jeder Plattform in einer `VideoPicker` Klasse implementiert.

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

Der Abhängigkeits Dienst für die Videoauswahl wird aufgerufen, indem die-Methode aufgerufen wird `DependencyService.Get` , um die Implementierung einer `IVideoPicker` Schnittstelle im Platt Form Projekt abzurufen. `GetVideoFileAsync`Anschließend wird die-Methode für diese Instanz aufgerufen, und der zurückgegebene Dateiname wird verwendet, um ein [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource) -Objekt zu erstellen und es auf die- [`Source`](xref:Xamarin.Forms.MediaElement.Source) Eigenschaft von festzulegen [`MediaElement`](xref:Xamarin.Forms.MediaElement) .

## <a name="change-video-aspect-ratio"></a>Videoseiten Verhältnis ändern

Die- [`Aspect`](xref:Xamarin.Forms.MediaElement.Aspect) Eigenschaft bestimmt, wie Video Medien so skaliert werden, dass Sie dem Anzeigebereich entsprechen. Standardmäßig ist diese Eigenschaft auf den `AspectFit` Enumerationsmember festgelegt, kann jedoch auf einen beliebigen Enumerationsmember festgelegt werden [`Aspect`](xref:Xamarin.Forms.Aspect) :

- `AspectFit`Gibt an, dass das Video bei Bedarf in den Anzeigebereich passt, wobei das Seitenverhältnis beibehalten wird.
- `AspectFill`Gibt an, dass das Video abgeschnitten wird, sodass der Anzeigebereich gefüllt wird, während das Seitenverhältnis beibehalten wird.
- `Fill`Gibt an, dass das Video gestreckt wird, um den Anzeigebereich auszufüllen.

## <a name="poll-for-position-data"></a>Abrufen von Positionsdaten

Die Benachrichtigung über die Eigenschafts Änderung für die [`Position`](xref:Xamarin.Forms.MediaElement.Position) bindbare Eigenschaft wird nur in wichtigen Zeitpunkten ausgelöst, z. b. beim Starten und Beenden der Wiedergabe und beim Anhalten Aus diesem Grund ergibt die Datenbindung an die- `Position` Eigenschaft keine exakten Positionsdaten. Stattdessen müssen Sie einen Timer einrichten und die-Eigenschaft Abfragen.

Ein guter Ausgangspunkt hierfür ist die `OnAppearing` außer Kraft setzung der Seite, die die Positionsdaten erfordert, wenn Medien wiedergegeben werden:

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

In diesem Beispiel startet die `OnAppearing` außer Kraft Setzung einen Timer, der `positionLabel` jede Sekunde mit dem Wert aktualisiert wird `Position` . Der Timer-Rückruf wird jede Sekunde aufgerufen, bis der Rückruf zurückgegeben wird `false` . Bei der Seitennavigation `OnDisappearing` wird die außer Kraft Setzung ausgeführt, die den Aufruf des timerrückrufs stoppt.

## <a name="understand-mediasource-types"></a>Grundlegendes zu MediaSource-Typen

Eine [`MediaElement`](xref:Xamarin.Forms.MediaElement) kann Medien wiedergeben, indem die [`Source`](xref:Xamarin.Forms.MediaElement.Source) zugehörige-Eigenschaft auf eine Remote-oder lokale Mediendatei festgelegt wird Die `Source` -Eigenschaft ist vom Typ [`MediaSource`](xref:Xamarin.Forms.MediaSource) , und diese Klasse definiert zwei statische Methoden:

- [`FromFile`](xref:Xamarin.Forms.MediaSource.FromFile*)gibt eine- [`MediaSource`](xref:Xamarin.Forms.MediaSource) Instanz von einem- `string` Argument zurück.
- [`FromUri`](xref:Xamarin.Forms.MediaSource.FromUri*)gibt eine- [`MediaSource`](xref:Xamarin.Forms.MediaSource) Instanz von einem- `Uri` Argument zurück.

Außerdem [`MediaSource`](xref:Xamarin.Forms.MediaSource) verfügt die-Klasse auch über implizite Operatoren, die `MediaSource` Instanzen von `string` -und-Argumenten zurückgeben `Uri` .

> [!NOTE]
> Wenn die- [`Source`](xref:Xamarin.Forms.MediaElement.Source) Eigenschaft in XAML festgelegt wird, wird ein Typkonverter aufgerufen, um eine- [`MediaSource`](xref:Xamarin.Forms.MediaSource) Instanz aus einem oder zurückzugeben `string` `Uri` .

Die- [`MediaSource`](xref:Xamarin.Forms.MediaSource) Klasse verfügt auch über zwei abgeleitete Klassen:

- [`UriMediaSource`](xref:Xamarin.Forms.UriMediaSource), die verwendet wird, um eine Remote Mediendatei aus einem URI anzugeben. Diese Klasse verfügt über eine- [`Uri`](xref:Xamarin.Forms.UriMediaSource.Uri) Eigenschaft, die auf festgelegt werden kann `Uri` .
- [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource), die verwendet wird, um eine lokale Mediendatei aus einer anzugeben `string` . Diese Klasse verfügt über eine- [`File`](xref:Xamarin.Forms.FileMediaSource.File) Eigenschaft, die auf festgelegt werden kann `string` . Außerdem verfügt diese Klasse über implizite Operatoren zum Konvertieren eines `string` `FileMediaSource` -Objekts in ein-Objekt und ein- `FileMediaSource` Objekt in ein-Objekt `string` .

> [!NOTE]
> Wenn ein- [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource) Objekt in XAML erstellt wird, wird ein Typkonverter aufgerufen, um eine- [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource) Instanz aus einem zurückzugeben `string` .

## <a name="determine-mediaelement-status"></a>Ermitteln des Media Element-Status

Die- [`MediaElement`](xref:Xamarin.Forms.MediaElement) Klasse definiert eine schreibgeschützte bindbare Eigenschaft mit dem Namen [`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState) vom Typ [`MediaElementState`](xref:Xamarin.Forms.MediaElementState) . Diese Eigenschaft gibt den aktuellen Status des Steuer Elements an, z. b. ob die Medien wiedergegeben oder angehalten werden oder ob die Medien noch nicht wiedergegeben werden können.

Die- [`MediaElementState`](xref:Xamarin.Forms.MediaElementState) Enumeration definiert die folgenden Member:

- `Closed`Gibt an, dass `MediaElement` keine Medien enthält.
- `Opening`Gibt an, dass der die `MediaElement` angegebene Quelle überprüft und versucht, die angegebene Quelle zu laden.
- `Buffering`Gibt an, dass das `MediaElement` die Medien für die Wiedergabe lädt. Die- [`Position`](xref:Xamarin.Forms.MediaElement.Position) Eigenschaft wird während dieses Zustands nicht Fortschritt. Wenn das `MediaElement` Video abgespielt hat, zeigt es weiterhin den zuletzt angezeigten Frame an.
- `Playing`Gibt an, dass der `MediaElement` die Medienquelle wieder gibt.
- `Paused`Gibt an, dass die- `MediaElement` Eigenschaft nicht voranstellt [`Position`](xref:Xamarin.Forms.MediaElement.Position) . Wenn das `MediaElement` Video abgespielt hat, wird der aktuelle Frame weiterhin angezeigt.
- `Stopped`Gibt an, dass das `MediaElement` Medium enthält, aber nicht wiedergegeben oder angehalten wird. Die [`Position`](xref:Xamarin.Forms.MediaElement.Position) -Eigenschaft ist 0 (null) und wird nicht fortfahren. Wenn das geladene Medium ein Video ist, wird der `MediaElement` erste Frame angezeigt.

Es ist im Allgemeinen nicht notwendig, die-Eigenschaft zu überprüfen, [`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState) Wenn die Transport Steuerelemente verwendet werden [`MediaElement`](xref:Xamarin.Forms.MediaElement) . Diese Eigenschaft wird jedoch beim Implementieren ihrer eigenen Transport Steuerelemente wichtig.

## <a name="implement-custom-transport-controls"></a>Implementieren von benutzerdefinierten Transport Steuerelementen

Die Transport Steuerelemente eines Media Players enthalten die Schalt **Flächen, die**die Funktionen **abspielen**, anhalten und **Anhalten**ausführen. In der Regel werden diese Schaltflächen mit bekannten Symbolen anstelle von Text dargestellt, und die Funktionen **Wiedergabe** und **Pause** werden in eine Schaltfläche kombiniert.

Standardmäßig sind die [`MediaElement`](xref:Xamarin.Forms.MediaElement) Wiedergabe Steuerelemente deaktiviert. Dies ermöglicht es Ihnen, die Programm gesteuert zu steuern `MediaElement` oder eigene Transport Steuerelemente bereitzustellen. Die Unterstützung hierfür `MediaElement` umfasst die [`Play`](xref:Xamarin.Forms.MediaElement.Play) [`Pause`](xref:Xamarin.Forms.MediaElement.Pause) Methoden, und [`Stop`](xref:Xamarin.Forms.MediaElement.Stop) .

Das folgende XAML-Beispiel zeigt eine Seite, die ein [`MediaElement`](xref:Xamarin.Forms.MediaElement) und benutzerdefinierte Transport Steuerelemente enthält:

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

In diesem Beispiel werden die benutzerdefinierten Transport Steuerelemente als- [`Button`](xref:Xamarin.Forms.Button) Objekte definiert. Es gibt jedoch nur zwei- `Button` Objekte, wobei die erste `Button` wieder **Gabe** und **Pause**darstellt und die zweite den Vorgang zum `Button` **Anhalten**darstellt. [`DataTrigger`](xref:Xamarin.Forms.DataTrigger)-Objekte werden verwendet, um die Schaltflächen zu aktivieren und zu deaktivieren und um die erste Schaltfläche zwischen wieder **Gabe** und **Pause**zu wechseln. Weitere Informationen zu Daten Triggern finden Sie unter [ Xamarin.Forms Trigger](~/xamarin-forms/app-fundamentals/triggers.md).

Die Code-Behind-Datei enthält die Handler für die- [`Clicked`](xref:Xamarin.Forms.Button.Clicked) Ereignisse:

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

Zum Implementieren einer benutzerdefinierten Positions Leiste müssen Sie die Dauer der Medien und die aktuelle Wiedergabe Position kennen. Diese Daten sind in der-Eigenschaft und der-Eigenschaft verfügbar [`Duration`](xref:Xamarin.Forms.MediaElement.Duration) [`Position`](xref:Xamarin.Forms.MediaElement.Position) .

> [!IMPORTANT]
> [`Position`](xref:Xamarin.Forms.MediaElement.Position)Muss abgepolgt werden, um genaue Positionsdaten zu erhalten. Weitere Informationen finden Sie unter Abrufen [von Positionsdaten](#poll-for-position-data).

Eine benutzerdefinierte Positions Leiste kann mit einem implementiert werden [`Slider`](xref:Xamarin.Forms.Slider) , wie im folgenden Beispiel gezeigt:

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

Die `PositionSlider` Klasse definiert ihre eigenen `Duration` und `Position` bindbaren Eigenschaften und eine `TimeToEnd` bindbare Eigenschaft. Alle drei Eigenschaften sind vom Typ `TimeSpan` . Der von der Eigenschaft geänderte Handler für die- `Duration` Eigenschaft legt die- `Maximum` Eigenschaft des-Objekts [`Slider`](xref:Xamarin.Forms.Slider) auf die- `TotalSeconds` Eigenschaft des- `TimeSpan` Werts fest. Die `TimeToEnd` -Eigenschaft wird auf der Grundlage von Änderungen an den `Duration` Eigenschaften und berechnet `Position` und beginnt bei der Dauer des Mediums und sinkt bei der Wiedergabe auf den Wert 0 (null).

`PositionSlider`Wird von der zugrunde liegenden aktualisiert [`Slider`](xref:Xamarin.Forms.Slider) , wenn `Slider` verschoben wird, um anzugeben, dass die Medien an einer neuen Position erweitert oder umgekehrt werden sollen. Dies wird im- `PropertyChanged` Handler im- `PositionSlider` Konstruktor erkannt. Der Handler prüft nach Änderungen an der `Value`-Eigenschaft. Wenn sie sich von der `Position`-Eigenschaft unterscheidet, wird die `Position`-Eigenschaft über die `Value`-Eigenschaft festgelegt. Weitere Informationen zur Verwendung von " [`Slider`](xref:Xamarin.Forms.Slider) Siehe", [ Xamarin.Forms Schieberegler](~/xamarin-forms/user-interface/slider.md)

> [!NOTE]
> Unter Android [`Slider`](xref:Xamarin.Forms.Slider) verfügt nur über 1000 diskrete Schritte, unabhängig von der `Minimum` -Einstellung und der- `Maximum` Einstellung. Wenn die Medien Länge größer als 1000 Sekunden ist, entsprechen zwei unterschiedliche `Position` Werte dem gleichen `Value` des `Slider` . Aus diesem Grund prüft der obige Code, ob die neue Position und die vorhandene Position größer als ein Hundertstel der Gesamtdauer sind.

Das folgende Beispiel zeigt `PositionSlider` , wie auf einer Seite verwendet wird:

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

In diesem Beispiel ist die `Duration` -Eigenschaft des `PositionSlider` Daten gebundenen an die- [`Duration`](xref:Xamarin.Forms.MediaElement.Duration) Eigenschaft des-Objekts [`MediaElement`](xref:Xamarin.Forms.MediaElement) . Wenn [`Value`](xref:Xamarin.Forms.Slider.Value) sich die-Eigenschaft der [`Slider`](xref:Xamarin.Forms.Slider) ändert, `ValueChanged` wird das-Ereignis ausgelöst, und der- `OnPositionSliderValueChanged` Handler wird ausgeführt. Mit diesem Handler wird die- [`MediaElement.Position`](xref:Xamarin.Forms.MediaElement.Position) Eigenschaft auf den Wert der-Eigenschaft festgelegt `PositionSlider.Position` . Daher wird das Ziehen der `Slider` Ergebnisse in die Position der Medienwiedergabe geändert:

[![Screenshot eines Media Element mit einer benutzerdefinierten Positions Leiste unter IOS und Android](mediaelement-images/custom-position-bar.png "Media Element mit einer benutzerdefinierten Positions Leiste")](mediaelement-images/custom-position-bar-large.png#lightbox "Media Element mit einer benutzerdefinierten Positions Leiste")

Außerdem wird ein- [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) Objekt verwendet, um den zu deaktivieren, `PositionSlider` Wenn die Medien gepuffert werden. Weitere Informationen zu Daten Triggern finden Sie unter [ Xamarin.Forms Trigger](~/xamarin-forms/app-fundamentals/triggers.md).

## <a name="implement-a-custom-volume-control"></a>Implementieren eines benutzerdefinierten volumesteuerelements

Die von jeder Plattform implementierten Steuerelemente zur Medienwiedergabe enthalten eine volumeleiste. Diese Leiste ähnelt einem Schieberegler und zeigt das Volume der Medien an. Außerdem können Sie die volumeleiste bearbeiten, um das Volume zu vergrößern oder zu verkleinern.

Eine benutzerdefinierte volumeleiste kann mit einem implementiert werden [`Slider`](xref:Xamarin.Forms.Slider) , wie im folgenden Beispiel gezeigt:

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

In diesem Beispiel bindet die [`Slider`](xref:Xamarin.Forms.Slider) Daten die- `Value` Eigenschaft an die- [`Volume`](xref:Xamarin.Forms.MediaElement.Volume) Eigenschaft von [`MediaElement`](xref:Xamarin.Forms.MediaElement) . Dies ist möglich, da die- `Volume` Eigenschaft eine- `TwoWay` Bindung verwendet. Daher wird durch Ändern der- `Value` Eigenschaft die- `Volume` Eigenschaft geändert.

> [!NOTE]
> Die- [`Volume`](xref:Xamarin.Forms.MediaElement.Volume) Eigenschaft verfügt über einen vlidation-Rückruf, mit dem sichergestellt wird, dass der Wert größer oder gleich 0,0 und kleiner oder gleich 1,0 ist.

Weitere Informationen zur Verwendung von " [`Slider`](xref:Xamarin.Forms.Slider) Siehe", [ Xamarin.Forms Schieberegler](~/xamarin-forms/user-interface/slider.md)

## <a name="related-links"></a>Verwandte Links

- [Mediaelementdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-mediaelementdemos/)
- [URI-Schemas](/windows/uwp/app-resources/uri-schemes)
- [Xamarin.Forms-Trigger](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.FormsKeits](~/xamarin-forms/user-interface/slider.md)
- [Android: Unterstützte Medienformate](https://developer.android.com/guide/topics/media/media-formats)
- [UWP: unterstützte Codecs](/windows/uwp/audio-video-camera/supported-codecs)
