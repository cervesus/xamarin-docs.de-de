---
title: Xamarin. Forms Media Element
description: In diesem Artikel wird erläutert, wie Media Element verwendet wird, um Video und Audiodaten in einer xamarin. Forms-Anwendung wiederzugeben.
ms.prod: xamarin
ms.assetid: e65f1e56-a80d-46c7-9ff4-7ae6650a3165
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/7/2020
ms.openlocfilehash: 67e4e9eec38f9dd23ebc3efe503d4b2a34ea8b39
ms.sourcegitcommit: 07941cf9704ff88cf4087de5ebdea623ff54edb1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2020
ms.locfileid: "77145718"
---
# <a name="xamarinforms-mediaelement"></a>Xamarin. Forms Media Element

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-samples/tree/pre-release/WorkingWithMediaElement)

`MediaElement` rendert einen Media Player für die Wiedergabe von Video-und Audiodaten. Die Betriebssystem-Wiedergabe Steuerelemente, die als Transport Steuerelemente bezeichnet werden, können verwendet werden, oder Sie können diese ausblenden und ihre eigenen Steuerelemente bereitstellen, um Ihre Entwurfs Anforderungen zu erfüllen.

Bevor Sie dieses neue Vorschau Steuerelement verwenden können, müssen Sie sich für die Verwendung entscheiden, indem Sie das entsprechende Flag in ihrer `App.xaml.xs`festlegen:

```csharp
Device.SetFlags(new string[]{ "MediaElement_Experimental" });
```

Unterstützte Plattformen:

- Android
- iOS
- UWP
- WPF
- macOS
- Tizen

## <a name="set-media-source"></a>Medienquelle festlegen

Die `MediaElement`-Eigenschaft `Source` kann einen URI oder einen lokalen Dateipfad annehmen. Die Wiedergabe beginnt unmittelbar nach dem Öffnen des Mediums.

```xaml
<MediaElement Source="http://sec.ch9.ms/ch9/5d93/a1eab4bf-3288-4faf-81c4-294402a85d93/XamarinShow_mid.mp4" />
```

![](mediaelement-images/mediaelement-android.png "MediaElement on Android")

Verwenden Sie das URI-Schema "MS-AppX", um die Quelle auf ein lokales Asset aus dem Platt Form Projekt festzulegen.

```xaml
<MediaElement Source="ms-appx://XamarinShow_mid.mp4" />
```

Wenn Sie die Datenbindung verwenden, möchten Sie möglicherweise einen Wert Konverter verwenden, um dieses URI-Schema anzuwenden:

```csharp
public class VideoSourceConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value == null)
            return null;

        if (String.IsNullOrWhiteSpace(value.ToString()))
            return null;

        if (Device.RuntimePlatform == Device.UWP)
            return new Uri($"ms-appx:///Assets/{value}");
        else
            return new Uri($"ms-appx:///{value}");
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```

```xaml
<MediaElement
    Source="{Binding VideoSource, Converter={StaticResource VideoSourceConverter}}" />
```

## <a name="control-playback"></a>Steuerelement Wiedergabe

Standardmäßig verwendet die `MediaElement` die Plattformen Native Wiedergabe Steuerelemente für Wiedergabe/Pause, volumetaste und stumm Schaltung, Suche und Zeitanzeige.

Um Ihre eigenen Steuerelemente zu überschreiben und zu implementieren, deaktivieren Sie zunächst die Platt Form Steuerelemente, indem Sie `ShowsPlaybackControls="False"` Sie können jetzt Ihre eigenen Steuerelemente verwenden, um den Wiedergabe Status und die Wiedergabe von Steuerelementen mithilfe der folgenden Eigenschaften, Methoden und Ereignisse widerzuspiegeln:

- `Play()`, `Pause()``Stop()` Methoden zum Steuern der Wiedergabe
- `AutoPlay` zum Festlegen des anfänglichen Verhaltens
- `CanSeek` zum Aktivieren oder Deaktivieren der Suche
- Eigenschaften von `Position` und `Duration` für Zeit
- `Volume` Eigenschaft zum Ändern von Volume und stumm Schaltung
- `IsLooping` Eigenschaft zum wiederholen, wenn die Wiedergabe abgeschlossen ist
- `StateRequested`, `PositionRequested``VolumeRequested` für die Verarbeitung von Aktualisierungen der Benutzeroberfläche

### <a name="handle-additional-media-events"></a>Behandeln zusätzlicher Medienereignisse

Sie können auf gängige Medienereignisse wie das `MediaOpened`-, `MediaEnded`-, `MediaFailed`-und `CurrentStateChanged`-Ereignis reagieren. Es wird empfohlen, immer das MediaFailed-Ereignis zu behandeln.
CurrentStateChanged gibt Informationen über den Wiedergabe Status an. Verwenden eines Werts der `MediaElementState` Enumeration:

- **Closed**: enthält kein Medium und zeigt einen transparenten Rahmen an
- **Öffnen**: überprüft und versucht, die angegebene Quelle zu laden.
- **Pufferung**: lädt die Medien für die Wiedergabe
  - Die `Position` wird während dieses Zustands nicht Fortschritt.
  - Wenn Sie bereits Videos abspielen, wird der zuletzt angezeigte Frame weiterhin angezeigt.
- Wieder **Gabe**: gibt die aktuelle Medienquelle wieder
- **Angeh**alten: der `Position` wird nicht fortgesetzt.
  - Wenn Sie Video abspielen, wird der aktuelle Frame weiterhin angezeigt.
- **Beendet**: enthält Medien, wird aber nicht abgespielt oder angehalten.
  - Der `Position` ist 0 und wird nicht fortfahren.
  - Wenn das geladene Medium ein Video ist, wird im `MediaElement` der erste Frame angezeigt.

## <a name="control-display"></a>Steuerelement Anzeige

Ähnlich wie bei [`Image`](xref:Xamarin.Forms.Image)verfügt das `MediaElement`-Steuerelement über `VideoHeight`, `VideoWidth`und `Aspect`. Der Aspekt erfordert drei verschiedene Optionen:

- **Aspectfill**: das Video gibt die gesamte Breite und Höhe des Steuer Elements aus, wobei häufig außerhalb der Steuerelement Grenzen eine Blutung erfolgt, während der Quell Aspekt beibehalten wird.
- **Aspectfit**: das Video passt in die Breite und Höhe des Steuer Elements, während der Quell Aspekt beibehalten wird.
- **Ausfüllen**: das Video wird die Breite und Höhe des Steuer Elements ausfüllen.

## <a name="additional-properties"></a>Zusätzliche Eigenschaften

- `VideoWidth`: Breite des Steuer Elements
- `VideoHeight`: Höhe des Steuer Elements
- `KeepScreenOn`:, wenn der Bildschirm des Geräts während der Wiedergabe eingeschaltet bleiben soll

## <a name="related-links"></a>Verwandte Links

- [MediaElement (Beispiel)](https://github.com/xamarin/xamarin-forms-samples/tree/pre-release/WorkingWithMediaElement)
