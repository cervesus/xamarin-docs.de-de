---
title: Hinzufügen einer Tap-stiftbewegungs-Erkennung
description: In diesem Artikel wird erläutert, wie Sie mit der tippbewegung zur Erkennung von Tippen Sie in einer Xamarin.Forms-Anwendung. Tippen Sie auf Erkennung wird mit der TapGestureRecognizer-Klasse implementiert.
ms.prod: xamarin
ms.assetid: 1D150BAF-4157-49BC-90A0-153323B8EBCF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: a28afb30770f15861aef06643e7f51070199ea9b
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2018
ms.locfileid: "38994853"
---
# <a name="adding-a-tap-gesture-recognizer"></a>Hinzufügen einer Tap-stiftbewegungs-Erkennung

_Die tippbewegung dient zur Erkennung von tippen, und es wird mit der TapGestureRecognizer-Klasse implementiert._

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

Anwendungen, die in der Regel verwenden Sie das Model-View-ViewModel (MVVM) Muster `ICommand` statt direkt Ereignishandler verknüpfen. Die [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) kann problemlos unterstützen `ICommand` entweder durch die Bindung im Code festlegen:

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


## <a name="related-links"></a>Verwandte Links

- [TapGesture (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/TapGesture/)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [TapGestureRecognizer](xref:Xamarin.Forms.TapGestureRecognizer)
