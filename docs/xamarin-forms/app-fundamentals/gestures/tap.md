---
title: Hinzufügen einer Gestenerkennung für Tippbewegungen
description: In diesem Artikel wird beschrieben, wie die Tippbewegung in einer Xamarin.Forms-App für die Tipperkennung verwendet wird. Die Tipperkennung wurde mit der Klasse TapGestureRecognizer implementiert.
ms.prod: xamarin
ms.assetid: 1D150BAF-4157-49BC-90A0-153323B8EBCF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1f5b476ac83f801b4ccded3e18bb601c06e0d0ef
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84570608"
---
# <a name="adding-a-tap-gesture-recognizer"></a>Hinzufügen einer Gestenerkennung für Tippbewegungen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-tapgesture)

_Die Tippbewegung wird zur Tipperkennung verwendet und wurde mit der Klasse TapGestureRecognizer implementiert._

Erstellen Sie eine Instanz [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer), bearbeiten Sie das Ereignis [`Tapped`](xref:Xamarin.Forms.TapGestureRecognizer.Tapped), und fügen Sie die neue Funktion zur Gestenerkennung der Sammlung [`GestureRecognizers`](xref:Xamarin.Forms.View.GestureRecognizers) im Benutzeroberflächenelement hinzu, damit ein Benutzeroberflächenelement mit der Tippbewegung angeklickt werden kann. Das folgende Codebeispiel stellt die Instanz `TapGestureRecognizer` dar, die an das Element [`Image`](xref:Xamarin.Forms.Image) angefügt ist:

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.Tapped += (s, e) => {
    // handle the tap
};
image.GestureRecognizers.Add(tapGestureRecognizer);
```

Standardmäßig reagiert das Bild auf einzelne Tippbewegungen. Legen Sie die Eigenschaft [`NumberOfTapsRequired`](xref:Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired) so fest, dass auf ein Doppeltippen (oder falls erforderlich mehrmaliges Tippen) gewartet wird.

```csharp
tapGestureRecognizer.NumberOfTapsRequired = 2; // double-tap
```

Wenn [`NumberOfTapsRequired`](xref:Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired) auf größer als 1 festgelegt ist, wird der Ereignishandler nur ausgeführt, wenn innerhalb eines festgelegten Zeitraums Tippbewegungen vorgenommen werden. Dieser Zeitraum ist nicht konfigurierbar. Sollte die zweite Tippbewegung (oder weitere) nicht in diesem Zeitraum stattfinden, werden sie effektiv ignoriert, und das „Zählen der Tippbewegungen“ startet neu.

## <a name="using-xaml"></a>Verwenden von XAML

Eine Gestenerkennung kann in XAML zum Steuerelement hinzugefügt werden, indem angefügte Eigenschaften verwendet werden. Die Syntax zum Hinzufügen von [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) zu einem Bild wird nachfolgend dargestellt (in diesem Fall wird ein Ereignis mit *Doppeltippen* definiert):

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
                Tapped="OnTapGestureRecognizerTapped"
                NumberOfTapsRequired="2" />
  </Image.GestureRecognizers>
</Image>
```

Der Code für den Ereignishandler (im Beispiel) erhöht einen Zähler und verändert das Bild von farbig zu schwarz-weiß.

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

Apps, die das MVVM-Muster (Model View ViewModel) verwenden, sollten `ICommand` verwenden, anstatt Ereignishandler direkt zu verknüpfen. [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) kann `ICommand` unterstützen, indem die Bindung im Code festgelegt wird:

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.SetBinding (TapGestureRecognizer.CommandProperty, "TapCommand");
image.GestureRecognizers.Add(tapGestureRecognizer);
```

Alternativ kann sie durch XAML festgelegt werden:

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
            Command="{Binding TapCommand}"
            CommandParameter="Image1" />
    </Image.GestureRecognizers>
</Image>
```

Den vollständigen Code für dieses Ansichtsmodells finden Sie im Beispiel. Die wesentlichen Implementierungsdetails von `Command` werden nachfolgend veranschaulicht:

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

- [TapGesture (sample) (TapGesture (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-tapgesture)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [TapGestureRecognizer Class (TapGestureRecognizer-Klasse)](xref:Xamarin.Forms.TapGestureRecognizer)
