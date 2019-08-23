---
title: Xamarin. Forms-Frame
description: Die xamarin. Forms-Frame Klasse ist ein Layout, das verwendet wird, um eine Ansicht oder ein Layout mit einem Rahmen zu umschließen, der mit Farbe, Schatten und anderen Optionen konfiguriert werden kann.
ms.prod: xamarin
ms.assetId: 4E074714-0928-41C8-A468-B60E23236A8C
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 08/06/2019
ms.openlocfilehash: 619b29a9d65594b1badd805c3361fe1a174d7174
ms.sourcegitcommit: 1341f2950b775a4daa7d0548a51fdef759afd6e3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2019
ms.locfileid: "69976503"
---
# <a name="xamarinforms-frame"></a>Xamarin. Forms-Frame

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-frame/)

Die xamarin. Forms [`Frame`](xref:Xamarin.Forms.Frame) -Klasse ist ein Layout, das verwendet wird, um eine Ansicht mit einem Rahmen zu umschließen, der mit Farbe, Schatten und anderen Optionen konfiguriert werden kann. Frames werden häufig zum Erstellen von Begrenzungen um Steuerelemente verwendet, Sie können jedoch auch verwendet werden, um komplexere Benutzeroberflächen zu erstellen. Weitere Informationen finden Sie unter [Erweiterte Frame Verwendung](#advanced-frame-usage).

Der folgende Screenshot zeigt `Frame` Steuerelemente unter IOS und Android:

Frame Beispiele für Frame Beispiele [für IOS und Android unter IOS und Android ![](frame-images/frame-cropped.png)](frame-images/frame-full.png#lightbox "")

Die `Frame` -Klasse definiert die folgenden Eigenschaften:

* [`BorderColor`](xref:Xamarin.Forms.Frame.BorderColor)ein `Color` -Wert, der die Farbe `Frame` des Rahmens bestimmt.
* [`CornerRadius`](xref:Xamarin.Forms.Frame.CornerRadius)ein `float` -Wert, der den gerundeten Radius der Ecke bestimmt.
* [`HasShadow`](xref:Xamarin.Forms.Frame.HasShadow)ein `bool` Wert, der bestimmt, ob der Frame einen Schlag Schatten aufweist.

Diese Eigenschaften werden von [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekten unterstützt. dies `Frame` bedeutet, dass das Ziel von Daten Bindungen sein kann.

> [!NOTE]
> Das `HasShadow` Eigenschafts Verhalten ist plattformabhängig. Der Standardwert ist `true` auf allen Plattformen. Bei UWP werden jedoch keine Schlag Schatten gerendert. Schlag Schatten werden sowohl für Android als auch für IOS gerendert, aber Drop Shadows on IOS sind dunkler und belegen mehr Platz.

## <a name="create-a-frame"></a>Frame erstellen

Eine `Frame` kann in XAML instanziiert werden. Das Standard `Frame` --Objekt verfügt über einen weißen Hintergrund, einen Ablage Schatten und keinen Rahmen. Ein `Frame` -Objekt umschließt normalerweise ein anderes Steuerelement. Das folgende Beispiel zeigt ein Standard `Frame` mäßiges umwickeln eines `Label` -Objekts:

```xaml
<Frame>
  <Label Text="Example" />
</Frame>
```

Ein `Frame` kann auch im Code erstellt werden:

```csharp
Frame defaultFrame = new Frame
{
    Content = new Label { Text = "Example" }
};
```

`Frame`Objekte können mit abgerundeten Ecken, farbigen Rahmen und Schlag Schatten durch Festlegen von Eigenschaften im XAML angepasst werden. Das folgende Beispiel zeigt ein angepasstes `Frame` Objekt:

```xaml
<Frame BorderColor="Orange"
       CornerRadius="10"
       HasShadow="True">
  <Label Text="Example" />
</Frame>
```

Diese Instanzeigenschaften können auch im Code festgelegt werden:

```csharp
Frame frame = new Frame
{
    BorderColor = Color.Orange,
    CornerRadius = 10,
    HasShadow = true,
    Content = new Label { Text = "Example" }
};
```

## <a name="advanced-frame-usage"></a>Erweiterte Frame Verwendung

Die `Frame` Klasse erbt von `ContentView`, d. h., Sie `View` kann jeden Objekttyp enthalten, einschließlich `Layout` -Objekten. Diese Fähigkeit ermöglicht die `Frame` Verwendung von, um komplexe UI-Objekte, z. b. Karten, zu erstellen.

### <a name="create-a-card-with-a-frame"></a>Erstellen einer Karte mit einem Frame

Die Kombi `Frame` Nation eines- `Layout` Objekts mit einem- `StackLayout` Objekt, wie z. b. einem-Objekt, ermöglicht die Erstellung einer komplexeren Der folgende Screenshot zeigt eine Beispiel Karte, die mit einem `Frame` -Objekt erstellt wurde:

["Screenshot einer Karte, die mit einem Frame erstellt wurde" Screenshot einer Karte, die mit einem Frame erstellt wurde ![](frame-images/frame-card-cropped.png)](frame-images/frame-full.png#lightbox "")

Der folgende XAML-Code zeigt, wie Sie mit der `Frame` -Klasse eine Karte erstellen:

```xaml
<Frame BorderColor="Gray"
       CornerRadius="5"
       Padding="8">
  <StackLayout>
    <Label Text="Card Example"
           FontSize="Medium"
           FontAttributes="Bold" />
    <BoxView Color="Gray"
             HeightRequest="2"
             HorizontalOptions="Fill" />
    <Label Text="Frames can wrap more complex layouts to create more complex UI components, such as this card!"/>
  </StackLayout>
</Frame>
```

Eine Karte kann auch im Code erstellt werden:

```csharp
Frame cardFrame = new Frame
{
    BorderColor = Color.Gray,
    CornerRadius = 5,
    Padding = 8,
    Content = new StackLayout
    {
        Children =
        {
            new Label
            {
                Text = "Card Example",
                FontSize = Device.GetNamedSize(NamedSize.Medium, typeof(Label)),
                FontAttributes = FontAttributes.Bold
            },
            new BoxView
            {
                Color = Color.Gray,
                HeightRequest = 2,
                HorizontalOptions = LayoutOptions.Fill
            },
            new Label
            {
                Text = "Frames can wrap more complex layouts to create more complex UI components, such as this card!"
            }
        }
    }
};
```

### <a name="round-elements"></a>Round-Elemente

Die `CornerRadius` -Eigenschaft `Frame` des-Steuer Elements kann zum Erstellen eines Kreis Bilds verwendet werden. Der folgende Screenshot zeigt ein Beispiel für ein Rundungs Bild, das mit `Frame` einem-Objekt erstellt wurde:

["Screenshot eines Kreis Bilds, das mit einem Frame erstellt wurde" Screenshot eines Kreis Bilds, das mit einem Frame erstellt wurde ![](frame-images/circle-image-cropped.png)](frame-images/frame-full.png#lightbox "")

Der folgende XAML-Code zeigt, wie ein Kreis Bild in XAML erstellt wird:

```xaml
<Frame Margin="10"
       BorderColor="Black"
       CornerRadius="50"
       HeightRequest="60"
       WidthRequest="60"
       IsClippedToBounds="True"
       HorizontalOptions="Center"
       VerticalOptions="Center">
  <Image Source="outdoors.jpg"
         Aspect="AspectFill"
         Margin="-20"
         HeightRequest="100"
         WidthRequest="100" />
</Frame>
```

Ein Kreis Bild kann auch im Code erstellt werden:

```csharp
Frame circleImageFrame = new Frame
{
    Margin = 10,
    BorderColor = Color.Black,
    CornerRadius = 50,
    HeightRequest = 60,
    WidthRequest = 60,
    IsClippedToBounds = true,
    HorizontalOptions = LayoutOptions.Center,
    VerticalOptions = LayoutOptions.Center,
    Content = new Image
    {
        Source = ImageSource.FromFile("outdoors.jpg"),
        Aspect = Aspect.AspectFill,
        Margin = -20,
        HeightRequest = 100,
        WidthRequest = 100
    }
};
```

Das Image " **Outdoor. jpg** " muss jedem Platt Form Projekt hinzugefügt werden, und wie dies erreicht wird, hängt von der Plattform ab. Weitere Informationen finden Sie unter [Images in xamarin. Forms](~/xamarin-forms/user-interface/images.md).

> [!NOTE]
> Abgerundete Ecken Verhalten sich über Plattformen hinweg etwas anders. Der `Image` Wert des `Margin` -Objekts sollte der Hälfte der Differenz zwischen der Bildbreite und der übergeordneten Rahmenbreite liegen und sollte negativ sein, um das Bild gleich `Frame` mäßig innerhalb des Objekts zentrieren zu können. Die angeforderten Breite und Höhe werden jedoch nicht garantiert, sodass die `Margin`Eigenschaften, `HeightRequest` und `WidthRequest` möglicherweise basierend auf der Bildgröße und anderen Layoutoptionen geändert werden müssen.

## <a name="related-links"></a>Verwandte Links

* [Frame-Demos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-frame/)
* [Bilder in xamarin. Forms](~/xamarin-forms/user-interface/images.md)
