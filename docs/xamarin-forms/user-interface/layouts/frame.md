---
title: Xamarin.FormsDr
description: Die Xamarin.Forms Frame-Klasse ist ein Layout, das verwendet wird, um eine Ansicht oder ein Layout mit einem Rahmen zu umschließen, der mit Farbe, Schatten und anderen Optionen konfiguriert werden kann.
ms.prod: ''
ms.assetId: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 42192111befbefda7e0f62b7691a8392c2828818
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137188"
---
# <a name="xamarinforms-frame"></a>Xamarin.FormsDr

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-frame/)

Die- Xamarin.Forms [`Frame`](xref:Xamarin.Forms.Frame) Klasse ist ein Layout, das verwendet wird, um eine Ansicht mit einem Rahmen zu umschließen, der mit Farbe, Schatten und anderen Optionen konfiguriert werden kann. Frames werden häufig zum Erstellen von Begrenzungen um Steuerelemente verwendet, Sie können jedoch auch verwendet werden, um komplexere Benutzeroberflächen zu erstellen. Weitere Informationen finden Sie unter [Erweiterte Frame Verwendung](#advanced-frame-usage).

Der folgende Screenshot zeigt Steuer `Frame` Elemente unter IOS und Android:

[!["Frame Beispiele für IOS und Android"](frame-images/frame-cropped.png)](frame-images/frame-full.png#lightbox "Frame Beispiele unter IOS und Android")

Die- `Frame` Klasse definiert die folgenden Eigenschaften:

* [`BorderColor`](xref:Xamarin.Forms.Frame.BorderColor)ein- `Color` Wert, der die Farbe des Rahmens bestimmt `Frame` .
* [`CornerRadius`](xref:Xamarin.Forms.Frame.CornerRadius)ein- `float` Wert, der den gerundeten Radius der Ecke bestimmt.
* [`HasShadow`](xref:Xamarin.Forms.Frame.HasShadow)ein `bool` Wert, der bestimmt, ob der Frame einen Schlag Schatten aufweist.

Diese Eigenschaften werden von- [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Objekten unterstützt. Dies bedeutet, `Frame` dass das Ziel von Daten Bindungen sein kann.

> [!NOTE]
> Das `HasShadow` Eigenschafts Verhalten ist plattformabhängig. Der Standardwert ist `true` auf allen Plattformen. Bei UWP werden jedoch keine Schlag Schatten gerendert. Schlag Schatten werden sowohl für Android als auch für IOS gerendert, aber Drop Shadows on IOS sind dunkler und belegen mehr Platz.

## <a name="create-a-frame"></a>Frame erstellen

Eine `Frame` kann in XAML instanziiert werden. Das Standard `Frame` --Objekt verfügt über einen weißen Hintergrund, einen Ablage Schatten und keinen Rahmen. Ein-Objekt umschließt `Frame` normalerweise ein anderes Steuerelement. Das folgende Beispiel zeigt ein Standardmäßiges `Frame` umwickeln eines- `Label` Objekts:

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

Die `Frame` Klasse erbt von `ContentView` , d. h., Sie kann jeden Objekttyp enthalten, `View` einschließlich- `Layout` Objekten. Diese Fähigkeit ermöglicht die `Frame` Verwendung von, um komplexe UI-Objekte, z. b. Karten, zu erstellen.

### <a name="create-a-card-with-a-frame"></a>Erstellen einer Karte mit einem Frame

Die Kombination eines- `Frame` Objekts mit einem-Objekt, `Layout` wie z. b. einem- `StackLayout` Objekt, ermöglicht die Erstellung einer komplexeren Der folgende Screenshot zeigt eine Beispiel Karte, die mit einem-Objekt erstellt wurde `Frame` :

[!["Screenshot einer Karte, die mit einem Frame erstellt wurde"](frame-images/frame-card-cropped.png)](frame-images/frame-full.png#lightbox "Screenshot einer mit einem Frame erstellten Karte")

Der folgende XAML-Code zeigt, wie Sie mit der-Klasse eine Karte erstellen `Frame` :

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

Die- `CornerRadius` Eigenschaft des- `Frame` Steuer Elements kann zum Erstellen eines Kreis Bilds verwendet werden. Der folgende Screenshot zeigt ein Beispiel für ein Rundungs Bild, das mit einem-Objekt erstellt wurde `Frame` :

[!["Screenshot eines mit einem Frame erstellten Kreis Bilds"](frame-images/circle-image-cropped.png)](frame-images/frame-full.png#lightbox "Screenshot eines mit einem Frame erstellten Kreis Bilds")

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

Das Image " **Outdoor. jpg** " muss jedem Platt Form Projekt hinzugefügt werden, und wie dies erreicht wird, hängt von der Plattform ab. Weitere Informationen finden Sie unter [Images in Xamarin.Forms ](~/xamarin-forms/user-interface/images.md).

> [!NOTE]
> Abgerundete Ecken Verhalten sich über Plattformen hinweg etwas anders. Der Wert des `Image` -Objekts `Margin` sollte der Hälfte der Differenz zwischen der Bildbreite und der übergeordneten Rahmenbreite liegen und sollte negativ sein, um das Bild gleichmäßig innerhalb des Objekts zentrieren zu können `Frame` . Die angeforderten Breite und Höhe werden jedoch nicht garantiert, sodass die `Margin` Eigenschaften, `HeightRequest` und `WidthRequest` möglicherweise basierend auf der Bildgröße und anderen Layoutoptionen geändert werden müssen.

## <a name="related-links"></a>Verwandte Links

* [Frame-Demos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-frame/)
* [Bilder inXamarin.Forms](~/xamarin-forms/user-interface/images.md)
