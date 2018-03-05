---
title: "Hinzufügen einer Tap Geste Gestenhandler-Erkennung"
description: "Die Tap-Geste wird für die Erkennung von Tap verwendet und wird mit der TapGestureRecognizer-Klasse implementiert."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1D150BAF-4157-49BC-90A0-153323B8EBCF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: d767b50d98b88e6b97a07caffcc103c70cfda428
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="adding-a-tap-gesture-gesture-recognizer"></a>Hinzufügen einer Tap Geste Gestenhandler-Erkennung

_Die Tap-Geste wird für die Erkennung von Tap verwendet und wird mit der TapGestureRecognizer-Klasse implementiert._

## <a name="overview"></a>Übersicht

Um ein Element der Benutzeroberfläche mit der Tap-Geste klickbar gemacht werden, erstellen Sie eine [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) -Instanz verwenden, behandeln die [ `Tapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.TapGestureRecognizer.Tapped/) Ereignis und fügen Sie die neue Geste Erkennung der [ `GestureRecognizers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.GestureRecognizers/) Auflistung auf das Element der Debuggerbenutzeroberfläche. Das folgende Codebeispiel zeigt eine `TapGestureRecognizer` angefügt, um eine [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Element:

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.Tapped += (s, e) => {
    // handle the tap
};
image.GestureRecognizers.Add(tapGestureRecognizer);
```

Standardmäßig wird das Abbild auf einzelnen Taps reagieren. Legen Sie die [ `NumberOfTapsRequired` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired/) Eigenschaft Doppeltippen (oder mehr Taps, falls erforderlich) warten.

```csharp
tapGestureRecognizer.NumberOfTapsRequired = 2; // double-tap
```

Wenn [ `NumberOfTapsRequired` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired/) -Wert über 1, der Ereignishandler nur ausgeführt, wenn die datenabzweigungen in einem festgelegten Zeitraum auftreten (dies Zeitraum ist nicht konfigurierbar). Wenn die zweiten (oder nachfolgenden) Taps nicht innerhalb dieses Zeitraums auftreten, werden sie effektiv ignoriert, und "Tap Count" neu gestartet wird.

<a name="Using_Xaml" />

## <a name="using-xaml"></a>Verwendung von Xaml

Eine Geste Erkennung kann in Xaml angefügten Eigenschaften mit einem Steuerelement hinzugefügt werden. Die Syntax zum Hinzufügen einer [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) wird zu einem Bild unten gezeigt (in diesem Fall definieren eine *Doppeltippen* Ereignis):

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
                Tapped="OnTapGestureRecognizerTapped"
                NumberOfTapsRequired="2" />
  </Image.GestureRecognizers>
</Image>
```

Der Code für den Ereignishandler (im Beispiel) erhöht einen Zähler und dabei ändert sich das Bild Farbe auf Schwarz &amp; weiß.

```csharp
void OnTapGestureRecognizerTapped(object sender, EventArgs args)
{
    tapCount++;
    var imageSender = (Image)sender;
    // watch the monkey go from color to black&white!
    if (tapCount % 2 == 0) {
        imageSender.Source = "tapped.jpg";
    } else {
        imageSender.Source = "tapped_bw.jpg";
    }
}
```

## <a name="using-icommand"></a>Mithilfe von ICommand

Anwendungen, die in der Regel verwenden Sie das Mvvm-Muster verwenden `ICommand` statt direkt Verkabelung Ereignishandler. Die [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) kann problemlos unterstützen `ICommand` entweder durch die Bindung im Code festlegen:

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.SetBinding (TapGestureRecognizer.CommandProperty, "TapCommand");
image.GestureRecognizers.Add(tapGestureRecognizer);
```

oder bei Verwendung von Xaml:

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
            Command="{Binding TapCommand}"
            CommandParameter="Image1" />
    </Image.GestureRecognizers>
</Image>
```

Der vollständige Code für dieses Modell anzeigen, kann im Beispiel gefunden werden. Die relevanten `Command` Implementierungsdetails sind unten dargestellt:

```csharp
public class TapViewModel : INotifyPropertyChanged
{
    int taps = 0;
    ICommand tapCommand;
    public TapViewModel () {
        // configure the TapCommand with a method
        tapCommand = new Command (OnTapped);
    }
    public ICommand TapCommand {
        get { return tapCommand; }
    }
    void OnTapped (object s)  {
        taps++;
        Debug.WriteLine ("parameter: " + s);
    }
    //region INotifyPropertyChanged code omitted
}
```

## <a name="summary"></a>Zusammenfassung

Die Tap-Geste für Tap-Erkennung verwendet wird, und wird implementiert, mit der [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) Klasse. Die Anzahl der abtaststellen Doppeltippen erkennen angegeben werden kann (oder Triple-Tap oder mehrere tippt) Verhalten.


## <a name="related-links"></a>Verwandte Links

- [TapGesture (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/TapGesture/)
- [GestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/)
- [TapGestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/)
