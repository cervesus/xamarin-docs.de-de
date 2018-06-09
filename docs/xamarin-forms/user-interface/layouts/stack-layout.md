---
title: Xamarin.Forms StackLayout
description: In diesem Artikel erläutert, wie die Xamarin.Forms StackLayout-Klasse verwenden, um Auflistungen von Sichten über eine Dimension darzustellen.
ms.prod: xamarin
ms.assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
ms.openlocfilehash: 6e278c466c352ad19575cd3a84d6e38e14ec2587
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244596"
---
# <a name="xamarinforms-stacklayout"></a>Xamarin.Forms StackLayout

`StackLayout` Ansichten in einer eindimensionalen Linie ("Stapel"), organisiert, horizontal oder vertikal. Ansichten in einer `StackLayout` basierend auf den Speicherplatz im Layout mit Layoutoptionen vergrößert werden kann. Positionieren wird durch die Reihenfolge bestimmt, die das Layout und die gruppenbezogenen Layoutoptionen Ansichten Sichten hinzugefügt wurden.

[![](stack-layout-images/layouts-sml.png "Xamarin.Forms Layouts")](stack-layout-images/layouts.png#lightbox "Xamarin.Forms Layouts")

## <a name="purpose"></a>Zweck

`StackLayout` ist kleiner als andere Sichten komplex. Einfache lineare Schnittstellen können durch Hinzufügen zu Sichten erstellt werden ein `StackLayout`, und eine komplexere Schnittstellen erstellt, indem sie schachteln.

## <a name="usage--behavior"></a>Verwendung und Verhalten

### <a name="spacing"></a>Abstand

Standardmäßig `StackLayout` einen 6px Rand zwischen den Ansichten werden hinzugefügt. Dies kann gesteuert oder festgelegt werden, haben keine Rand durch Festlegen der `Spacing` Eigenschaft StackLayout. Das folgende Beispiel zeigt, wie Abstand und die Wirkung der unterschiedliche Abstände Optionen festgelegt:

In XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.StackLayoutDemo"
Title="StackLayout Demo">
  <ContentPage.Content>
    <StackLayout Spacing="10" x:Name="layout">
      <Button Text="StackLayout" VerticalOptions="Start"
        HorizontalOptions="FillAndExpand" />
      <BoxView Color="Yellow" VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView Color="Green" VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView HeightRequest="75" Color="Blue" VerticalOptions="End"
        HorizontalOptions="FillAndExpand" />
    </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

In C#:

```csharp
public class StackLayoutCode : ContentPage
{
  public StackLayoutCode ()
  {
    var layout = new StackLayout ();
    var button = new Button { Text = "StackLayout", VerticalOptions = LayoutOptions.Start,
     HorizontalOptions = LayoutOptions.FillAndExpand };
    var yellowBox = new BoxView { Color = Color.Yellow, VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var greenBox = new BoxView { Color = Color.Green, VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var blueBox = new BoxView { Color = Color.Blue, VerticalOptions = LayoutOptions.FillAndExpand,
      HorizontalOptions = LayoutOptions.FillAndExpand, HeightRequest = 75 };

    layout.Children.Add(button);
    layout.Children.Add(yellowBox);
    layout.Children.Add(greenBox);
    layout.Children.Add(blueBox);
    layout.Spacing = 10;
    Content = layout;
  }
}
```

Abstand = 0:

![](stack-layout-images/spacing-zero.png "StackLayout mit Abstand = 0")

Abstand von zehn:

![](stack-layout-images/spacing-ten.png "StackLayout mit Abstand = 10")

### <a name="sizing"></a>Anpassen der Größe

Die Größe einer Sicht in einer StackLayout hängt davon ab, die Höhe und Breite Anforderungen und die gruppenbezogenen Layoutoptionen. `StackLayout` Erzwingt die Auffüllung. Die folgenden `LayoutOption`s führt dazu, dass Ansichten nimmt so viel Speicherplatz wie aus dem Layout verfügbar ist:

- **CenterAndExpand** &ndash; zentriert die Ansicht innerhalb des Layouts und wird erweitert, um dauern, bis so viel Speicherplatz wie das Layout es bereitgestellt wird.
- **EndAndExpand** &ndash; platziert die Ansicht am Ende des Layouts (oder Begrenzung ganz rechts unten) und wird erweitert, um dauern, bis so viel Speicherplatz wie das Layout es bereitgestellt wird.
- **FillAndExpand** &ndash; platziert die Sicht, damit sie keine Auffüllung hat und wie viel Platz einnehmen, wie das Layout es bereitgestellt wird.
- **StartAndExpand** &ndash; platziert die Ansicht zu Beginn des Layouts und wie viel Platz einnehmen, wie das übergeordnete Element bereitgestellt wird.

Weitere Informationen finden Sie unter [Erweiterung](~/xamarin-forms/user-interface/layouts/layout-options.md#expansion).

### <a name="positioning"></a>Positionierung

Ansichten in einer StackLayout positioniert werden kann und Größe mit `LayoutOptions`. Jede Sicht kann angegeben werden `VerticalOptions` und `HorizontalOptions`, definieren, wie die Ansichten selbst relativ zu das Layout positioniert werden. Die folgenden vordefinierten `LayoutOptions` sind verfügbar:

- **Center** &ndash; zentriert die Ansicht innerhalb des Layouts.
- **End** &ndash; platziert die Ansicht am Ende des Layouts (oder Begrenzung ganz rechts unten).
- **Füllen Sie** &ndash; platziert die Sicht so, dass sie über keine Auffüllung verfügt.
- **Starten Sie** &ndash; platziert die Ansicht zu Beginn des Layouts.

Der folgende Code zeigt Layout Festlegen von Optionen an:

In XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.StackLayoutDemo"
Title="StackLayout Demo">
  <ContentPage.Content>
    <StackLayout x:Name="layout">
      <Button VerticalOptions="Start"
        HorizontalOptions="FillAndExpand" />
      <BoxView VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView HeightRequest="75" VerticalOptions="End"
        HorizontalOptions="FillAndExpand" />
    </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

In C#:

```csharp
public class StackLayoutCode : ContentPage
{
  public StackLayoutCode ()
  {
    var layout = new StackLayout ();
    var button = new Button { VerticalOptions = LayoutOptions.Start,
     HorizontalOptions = LayoutOptions.FillAndExpand };
    var oneBox = new BoxView { VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var twoBox = new BoxView {  VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var threeBox = new BoxView {  VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };

    layout.Children.Add(button);
    layout.Children.Add(oneBox);
    layout.Children.Add(twoBox);
    layout.Children.Add(threeBox);
    Content = layout;
  }
}
```

Weitere Informationen finden Sie unter [Ausrichtung](~/xamarin-forms/user-interface/layouts/layout-options.md#alignment).

## <a name="exploring-a-complex-layout"></a>Untersuchen ein komplexes Layout

Jedes der Layouts aufweisen Stärken und Schwächen für bestimmte Layouts erstellen. In der gesamten diese Artikelreihe Layout wurde eine Beispiel-app mit dem gleichen Seitenlayout mithilfe von drei verschiedenen Layouts implementiert erstellt.

Betrachten Sie das folgende XAML-Code ein:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TheBusinessTumble.StackLayoutPage"
BackgroundColor="Maroon"
Title="StackLayouts">
    <ContentPage.Content>
    <ScrollView>
      <StackLayout Spacing="0" Padding="0" BackgroundColor="Maroon">
        <BoxView HorizontalOptions="FillAndExpand" HeightRequest="100"
          VerticalOptions="Start" Color="Gray" />
        <Button BorderRadius="30" HeightRequest="60" WidthRequest="60"
          BackgroundColor="Red" HorizontalOptions="Center" VerticalOptions="Start" />
        <StackLayout HeightRequest="100" VerticalOptions="Start" HorizontalOptions="FillAndExpand" Spacing="20" BackgroundColor="Maroon">
          <Label Text="User Name" FontSize="28" HorizontalOptions="Center"
            VerticalOptions="Center" FontAttributes="Bold" />
          <Entry Text="Bio + Hashtags" TextColor="White"
            BackgroundColor="Maroon" HorizontalOptions="FillAndExpand" VerticalOptions="CenterAndExpand" />
        </StackLayout>
        <StackLayout Orientation="Horizontal" HeightRequest="50" BackgroundColor="White" Padding="5">
          <StackLayout Spacing="0" BackgroundColor="White" Orientation="Horizontal" HorizontalOptions="Start">
            <BoxView BackgroundColor="Black" WidthRequest="40" HeightRequest="40"  HorizontalOptions="StartAndExpand" VerticalOptions="Center" />
            <Label FontSize="14" TextColor="Black" Text="Accent Color" HorizontalOptions="StartAndExpand" VerticalOptions="Center" />
          </StackLayout>
          <StackLayout Spacing="0" BackgroundColor="White" Orientation="Horizontal" HorizontalOptions="EndAndExpand">
            <BoxView BackgroundColor="Maroon" WidthRequest="40" HeightRequest="40" HorizontalOptions="Start" VerticalOptions="Center" />
            <Label FontSize="14" TextColor="Black" Text="Primary Color" HorizontalOptions="StartAndExpand" VerticalOptions="Center" />
          </StackLayout>
        </StackLayout>
        <StackLayout Orientation="Horizontal">
          <Label FontSize="14" Text="Age:" TextColor="White" HorizontalOptions="Start" VerticalOptions="Center" WidthRequest="100" />
          <Entry HorizontalOptions="FillAndExpand" Text="35" TextColor="White" BackgroundColor="Maroon" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
          <Label FontSize="14" Text="Interests:" TextColor="White"
          HorizontalOptions="Start" VerticalOptions="Center" WidthRequest="100" />
          <Entry HorizontalOptions="FillAndExpand" Text="Xamarin.Forms" TextColor="White" BackgroundColor="Maroon" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
          <Label FontSize="14" Text="Ask me about:" TextColor="White"
          HorizontalOptions="Start" VerticalOptions="Center" WidthRequest="100"/>
          <Entry HorizontalOptions="FillAndExpand" Text="Xamarin, C#, .NET, Mono..." TextColor="White" BackgroundColor="Maroon" />
        </StackLayout>
      </StackLayout>
    </ScrollView>
    </ContentPage.Content>
</ContentPage>

```

Der obige Code erhalten Sie im folgenden dargestellt:

![](stack-layout-images/stack.png "Komplexe StackLayout")

Beachten Sie, dass `StackLayouts`s geschachtelt werden, da in einigen Fällen Schachteln von Layouts einfacher, als vertrauenswürdig sind alle Elemente innerhalb des gleichen Layouts sein kann. Beachten Sie, dass auch, da `StackLayout` unterstützt keine überlappenden Elemente der Seite "ist nicht Teil der Layout-Sicherungsfunktionalität gefunden auf den Seiten für die andere Tastaturlayouts stimmen.



## <a name="related-links"></a>Verwandte Links

- [LayoutOptions](~/xamarin-forms/user-interface/layouts/layout-options.md)
- [Layout (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble-Beispiel (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
