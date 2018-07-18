---
title: Hinzufügen einer Tap Geste Stiftbewegungs-Erkennung
description: In diesem Artikel wird erläutert, wie Sie mit der tippbewegung zur Erkennung von Tippen Sie in einer Xamarin.Forms-Anwendung. Tippen Sie auf Erkennung wird mit der TapGestureRecognizer-Klasse implementiert.
ms.prod: xamarin
ms.assetid: 1D150BAF-4157-49BC-90A0-153323B8EBCF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: e602ae1f140640d9a895b65d78feab3d0a3b7861
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994853"
---
# <a name="adding-a-tap-gesture-gesture-recognizer"></a>Hinzufügen einer Tap Geste Stiftbewegungs-Erkennung

_Die tippbewegung dient zur Erkennung von tippen, und es wird mit der TapGestureRecognizer-Klasse implementiert._

## <a name="overview"></a>Übersicht

Um ein Element der Benutzeroberfläche geklickt werden kann, mit der tippbewegung machen, erstellen eine [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) -Instanz, behandeln Sie die [ `Tapped` ](xref:Xamarin.Forms.TapGestureRecognizer.Tapped) Ereignis und fügen Sie der neuen stiftbewegungs-Erkennung, um die [ `GestureRecognizers` ](xref:Xamarin.Forms.View.GestureRecognizers) Auflistung in das Benutzeroberflächenelement. Das folgende Codebeispiel zeigt eine `TapGestureRecognizer` angefügt, um eine [ `Image` ](xref:Xamarin.Forms.Image) Element:

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.Tapped += (s, e) => {
    // handle the tap
};
image.GestureRecognizers.Add(tapGestureRecognizer);
```

Standardmäßig wird das Abbild auf einzelnen Tippen Antworten. Legen Sie die [ `NumberOfTapsRequired` ](xref:Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired) Eigenschaft warten, Doppeltippen (oder weitere Taps, falls erforderlich).

```csharp
tapGestureRecognizer.NumberOfTapsRequired = 2; // double-tap
```

Wenn [ `NumberOfTapsRequired` ](xref:Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired) festgelegt ist über 1, der Ereignishandler nur ausgeführt wird, wenn die Klicks innerhalb einer festgelegten Zeitspanne auftreten (dieser Zeitraum ist nicht konfigurierbar). Wenn es sich bei der zweiten (oder nachfolgender) Taps nicht innerhalb dieses Zeitraums auftreten, werden sie effektiv ignoriert und "Anzahl der Tippen Sie auf" neu gestartet wird.

<a name="Using_Xaml" />

## <a name="using-xaml"></a>Mithilfe von Xaml

Eine stiftbewegungs-Erkennung kann an ein Steuerelement in Xaml mit angefügten Eigenschaften hinzugefügt werden. Die Syntax zum Hinzufügen einer [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) wird zu einem Bild unten gezeigt (in diesem Fall definieren eine *Doppeltippen* Ereignis):

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
                Tapped="OnTapGestureRecognizerTapped"
                NumberOfTapsRequired="2" />
  </Image.GestureRecognizers>
</Image>
```

Der Code für den Ereignishandler (im Beispiel) erhöht einen Zähler, und ändert sich das Image von Farbe auf Schwarz &amp; weiß.

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

## <a name="using-icommand"></a>Verwenden von ICommand

Anwendungen, die in der Regel verwenden Sie das Mvvm-Muster verwenden `ICommand` statt direkt Ereignishandler verknüpfen. Die [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) kann problemlos unterstützen `ICommand` entweder durch die Bindung im Code festlegen:

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.SetBinding (TapGestureRecognizer.CommandProperty, "TapCommand");
image.GestureRecognizers.Add(tapGestureRecognizer);
```

oder mit Xaml:

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
            Command="{Binding TapCommand}"
            CommandParameter="Image1" />
    </Image.GestureRecognizers>
</Image>
```

Der vollständige Code für dieses Ansichtsmodells finden Sie im Beispiel. Die relevanten `Command` Implementierungsdetails werden unten angezeigt:

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

Die tippbewegung für Tap-Erkennung verwendet wird, und wird implementiert, mit der [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) Klasse. Die Anzahl der abtaststellen kann angegeben werden, um die Erkennung von Doppeltippen Sie auf (oder Triple-tippen, oder mehrere tippt) Verhalten.


## <a name="related-links"></a>Verwandte Links

- [TapGesture (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/TapGesture/)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [TapGestureRecognizer](xref:Xamarin.Forms.TapGestureRecognizer)
