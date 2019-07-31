---
title: Xamarin.Forms StackLayout
description: In diesem Artikel erläutert die Xamarin.Forms-StackLayout-Klasse zu verwenden, um Auflistungen von Ansichten für eine Dimension darzustellen.
ms.prod: xamarin
ms.assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
ms.openlocfilehash: ad20ba50b8ff0f7dcbba3e8d297b2281544a373b
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68657089"
---
# <a name="xamarinforms-stacklayout"></a>Xamarin.Forms StackLayout

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)

[Stacklayout](xref:Xamarin.Forms.StackLayout) organisiert Sichten in einer eindimensionalen Linie ("Stapel"), entweder horizontal oder vertikal. Ansichten in einer `StackLayout` basierend auf den Speicherplatz im Layout mit Layoutoptionen vergrößert werden kann. Positionierung wird durch die Reihenfolge bestimmt, die das Layout und die Layoutoptionen Ansichten Ansichten hinzugefügt wurden.

[![](stack-layout-images/layouts-sml.png "Xamarin.Forms-Layouts")](stack-layout-images/layouts.png#lightbox "Xamarin.Forms-Layouts")

## <a name="purpose"></a>Zweck

`StackLayout` ist weniger komplex als anderen Ansichten. Einfache lineare Schnittstellen können erstellt werden, durch das Hinzufügen nur Ansichten, um eine `StackLayout`, und komplexere Schnittstellen, die durch das Schachteln von ihnen erstellt.

## <a name="usage--behavior"></a>Nutzung und Verhalten

### <a name="spacing"></a>Abstand

In der Standardeinstellung `StackLayout` wird einen 6px Rand zwischen den Ansichten hinzugefügt. Dies kann gesteuert oder festgelegt werden, damit kein Rand durch Festlegen der `Spacing` StackLayout Eigenschaft. Das folgende Beispiel veranschaulicht, wie Abstände und die Auswirkungen der verschiedenen Abstandsoptionen festlegen:

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

Der Abstand = 0:

![](stack-layout-images/spacing-zero.png "StackLayout mit Abstand = 0")

Der Abstand von zehn:

![](stack-layout-images/spacing-ten.png "StackLayout mit Abstand = 10")

### <a name="sizing"></a>Größenanpassung

Die Größe einer Ansicht in einem StackLayout hängt davon ab, sowohl die Höhe und Breite Anforderungen als auch die Layoutoptionen. `StackLayout` Erzwingt die Auffüllung. Die folgenden `LayoutOption`s führt dazu, dass Ansichten nimmt so viel Speicherplatz wie das Layout verfügbar ist:

- **CenterAndExpand** &ndash; konzentriert sich die Sicht innerhalb des Layouts und wird erweitert, um so viel Speicherplatz wie das Layout sie erhalten dauern.
- **EndAndExpand** &ndash; positioniert die Ansicht am Ende des Layouts (untere oder rechte Begrenzung) und wird erweitert, um so viel Speicherplatz wie das Layout sie erhalten dauern.
- **FillAndExpand** &ndash; die Ansicht so anordnet, dass er verfügt über keine Auffüllung, und so viel Speicherplatz wie das Layout es erhalten hat.
- **StartAndExpand** &ndash; platziert die Ansicht zu Beginn des Layouts und nimmt so viel Speicherplatz wie das übergeordnete Element bereitgestellt wird.

Weitere Informationen finden Sie unter [Erweiterung](~/xamarin-forms/user-interface/layouts/layout-options.md#expansion).

### <a name="positioning"></a>Positionierung

Ansichten in einem StackLayout positioniert werden können und die Größe mit `LayoutOptions`. Jede Sicht kann angegeben werden `VerticalOptions` und `HorizontalOptions`, definieren, wie die Ansichten selbst relativ zu das Layout positioniert werden. Die folgenden vordefinierten `LayoutOptions` sind verfügbar:

- **Center** &ndash; die Ansicht innerhalb des Layouts zentriert wird.
- **End** &ndash; positioniert die Ansicht am Ende des Layouts (untere oder rechte Begrenzung).
- **Geben Sie** &ndash; die Ansicht so anordnet, dass sie keine Auffüllung verfügt.
- **Starten Sie** &ndash; platziert die Ansicht zu Beginn des Layouts.

Der folgende Code veranschaulicht Layout Festlegen von Optionen an:

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

## <a name="exploring-a-complex-layout"></a>Erkunden ein komplexes Layout

Jede der Layouts hat Stärken und Schwächen für bestimmte Layouts erstellen. In dieser Artikelreihe Layout eine Beispiel-app mit der gleichen Layout mit drei verschiedenen Layouts implementiert wurde.

Betrachten Sie das folgende XAML:

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

Der obige Code führt das folgende Layout:

![](stack-layout-images/stack.png "Komplexe StackLayout")

Beachten Sie, dass `StackLayouts`s geschachtelt sind, da in einigen Fällen Schachteln von Layouts einfacher als die Darstellung aller Elemente innerhalb des gleichen Layouts sein kann. Beachten Sie, dass da `StackLayout` unterstützt keine überlappenden Elemente, die Seite nicht haben einige der Layout-Sicherungsfunktionalität finden Sie auf den Seiten für die anderen Layouts.



## <a name="related-links"></a>Verwandte Links

- [LayoutOptions](~/xamarin-forms/user-interface/layouts/layout-options.md)
- [Layout (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [BusinessTumble-Beispiel (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-businesstumble)
