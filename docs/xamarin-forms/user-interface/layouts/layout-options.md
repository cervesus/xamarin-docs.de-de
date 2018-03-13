---
title: LayoutOptions
description: Jede Ansicht Xamarin.Forms hat HorizontalOptions und VerticalOptions Eigenschaften vom Typ LayoutOptions. Dieser Artikel beschreibt die Auswirkungen jeder LayoutOptions Wert auf die Ausrichtung und die Erweiterung einer Sicht.
ms.topic: article
ms.prod: xamarin
ms.assetid: 7CAB5631-5153-4DEF-8AD7-C6011CE44307
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/10/2017
ms.openlocfilehash: a2aa143d5aeb801cd753dd99718ca9cf6dd72353
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="layoutoptions"></a>LayoutOptions

_Jede Ansicht Xamarin.Forms hat HorizontalOptions und VerticalOptions Eigenschaften vom Typ LayoutOptions. Dieser Artikel beschreibt die Auswirkungen jeder LayoutOptions Wert auf die Ausrichtung und die Erweiterung einer Sicht._

## <a name="overview"></a>Übersicht

Die [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) Struktur kapselt zwei layouteinstellungen:

- **Ausrichtung** – die Sicht die bevorzugte Ausrichtung, die festlegt, seiner Position und Größe innerhalb des übergeordneten Layouts.
- **Erweiterung** – verwendet nur durch eine [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), und gibt an, ob die Sicht zusätzlichen Speicherplatz verwendet werden soll, sofern dieser verfügbar ist.

Diese layouteinstellungen können angewendet werden, um eine [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/), relativ zum übergeordneten Element, indem Sie festlegen der [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) oder [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) Eigenschaft von der `View` auf einen öffentlichen Felder aus der [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) Struktur. Die öffentliche Felder sind wie folgt aus:

- [`Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/)
- [`Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/)
- [`End`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/)
- [`Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/)
- [`StartAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/)
- [`CenterAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.CenterAndExpand/)
- [`EndAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.EndAndExpand/)
- [`FillAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/)

Die `Start`, `Center`, `End`, und `Fill` Felder verwendet, um die Ansicht Ausrichtung innerhalb des übergeordneten Layouts definieren:

- Für die horizontale Ausrichtung [ `Start` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/) Positionen der [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) auf der linken Seite des übergeordneten Layouts und für die vertikale Ausrichtung, die Position der `View` am oberen Rand der übergeordnete Layout.
- Für die horizontale und vertikale Ausrichtung [ `Center` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/) horizontal oder vertikal zentriert die [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/).
- Für die horizontale Ausrichtung [ `End` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/) Positionen der [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) auf der rechten Seite des übergeordneten Layouts und für die vertikale Ausrichtung, die Position der `View` unten von der übergeordneten-Layout.
- Für die horizontale Ausrichtung [ `Fill` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/) wird sichergestellt, dass die [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) füllt die Breite des übergeordneten Layouts und für die vertikale Ausrichtung, stellt er sicher, dass die `View` füllt die die Höhe des übergeordneten Layouts.

Die `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, und `FillAndExpand` Werte werden verwendet, um die Voreinstellung für die Ausrichtung, definieren und gibt an, ob die Sicht mehr Speicherplatz gegebenenfalls innerhalb des übergeordneten Elements eingenommenen [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/).

> [!NOTE]
> Der Standardwert einer Sicht [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) und [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) Eigenschaften [ `LayoutOptions.Fill` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/).

<a name="alignment" />

## <a name="alignment"></a>Ausrichtung

Ausrichtung steuert, wie eine Sicht innerhalb des übergeordneten Layouts positioniert wird, wenn das Layout des übergeordneten ungenutzten Speicherplatz enthält (d. h. das übergeordnete Layout ist größer als die kombinierte Größe aller untergeordneten Elemente).

Ein [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) berücksichtigt nur die `Start`, `Center`, `End`, und `Fill` [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) Felder auf, die in die entgegengesetzte Richtung sind untergeordnete Ansichten um die `StackLayout` Ausrichtung. Deshalb untergeordnete Ansichten in einer vertikal ausgerichtet `StackLayout` können festlegen, ihre [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) Eigenschaften in eines der `Start`, `Center`, `End`, oder `Fill` Felder. Auf ähnliche Weise untergeordnete Ansichten in einem horizontal ausgerichteten `StackLayout` können festlegen, ihre [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) zu einer der Eigenschaften der `Start`, `Center`, `End`, oder `Fill` Felder.

Ein [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) berücksichtigt nicht die `Start`, `Center`, `End`, und `Fill` [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) Felder auf untergeordnete Ansichten, die in der gleichen Richtung wie sind die `StackLayout` Ausrichtung. Aus diesem Grund eine vertikal ausgerichtet `StackLayout` ignoriert die `Start`, `Center`, `End`, oder `Fill` switchrichtlinien auf Felder in der [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) Eigenschaften der untergeordneten Ansichten. Auf ähnliche Weise einem horizontal ausgerichteten `StackLayout` ignoriert die `Start`, `Center`, `End`, oder `Fill` switchrichtlinien auf Felder in der [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) Eigenschaften der untergeordneten Ansichten.

> [!NOTE]
> [`LayoutOptions.Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/) im Allgemeinen überschreibt Größe Anforderungen angegeben, mit der [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) und [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) Eigenschaften.

Im folgenden XAML-Codebeispiel wird veranschaulicht, eine vertikal ausgerichtet [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) , in dem jedes untergeordnete Element [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) legt seine [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) Eigenschaft einer der vier Ausrichtung Felder aus der [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) Struktur:

```xaml
<StackLayout Margin="0,20,0,0">
  ...
  <Label Text="Start" BackgroundColor="Gray" HorizontalOptions="Start" />
  <Label Text="Center" BackgroundColor="Gray" HorizontalOptions="Center" />
  <Label Text="End" BackgroundColor="Gray" HorizontalOptions="End" />
  <Label Text="Fill" BackgroundColor="Gray" HorizontalOptions="Fill" />
</StackLayout>
```

Die entsprechende C#-Code wird unten gezeigt:

```csharp
Content = new StackLayout
{
  Margin = new Thickness(0, 20, 0, 0),
  Children = {
    ...
    new Label { Text = "Start", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Start },
    new Label { Text = "Center", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Center },
    new Label { Text = "End", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.End },
    new Label { Text = "Fill", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Fill }
  }
};
```

Der Code führt das Layout in den folgenden Screenshots dargestellt:

[![](layout-options-images/alignment.png "Ausrichtung Layoutoptionen")](layout-options-images/alignment-large.png#lightbox "Ausrichtung Layoutoptionen")

<a name="expansion" />

## <a name="expansion"></a>Erweiterung

Erweiterung wird gesteuert, ob eine Sicht mehr Speicherplatz belegen wird ggf. in einem [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/). Wenn die `StackLayout` ungenutzten Speicherplatz enthält (d. h. die `StackLayout` ist größer als die kombinierte Größe aller untergeordneten), der nicht verwendete Speicherplatz freigegeben ist gleichermaßen alle untergeordneten Ansichten, die Erweiterung durch Festlegen von anfordern ihre [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/)oder [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) Eigenschaften einer [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) Feld, verwendet die `AndExpand` Suffix. Hinweis: Wenn der Speicherplatz in der `StackLayout` wird verwendet, die Erweiterungsoptionen haben keine Auswirkungen.

Ein [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) können nur untergeordnete Ansichten in seiner ausrichtungsrichtung erweitern. Daher eine vertikal ausgerichtet `StackLayout` können erweitert werden untergeordnete Ansichten, die festgelegt ihre [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) zu einer der Eigenschaften der `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, oder `FillAndExpand` Felder, wenn die `StackLayout` ungenutzten Speicherplatz enthält. Entsprechend einem horizontal ausgerichteten `StackLayout` können untergeordnete Ansichten, die festgelegt erweitern ihre [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) Eigenschaften auf einen der der `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, oder `FillAndExpand` Felder, wenn der `StackLayout` ungenutzten Speicherplatz enthält.

Ein [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) untergeordnete Ansichten in der Richtung statt der Ausrichtung nicht erweitert werden kann. Aus diesem Grund auf eine vertikal ausgerichtet `StackLayout`wird durch das Festlegen der [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) Eigenschaft auf eine untergeordnete Ansicht zu [ `StartAndExpand` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/) hat dieselbe Wirkung wie das Festlegen der Eigenschaft auf [ `Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/).

> [!NOTE]
> Beachten Sie, dass die Erweiterung aktivieren die Größe einer Ansicht ändern nicht, es sei denn, sie verwendet [ `LayoutOptions.FillAndExpand` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/).

Im folgenden XAML-Codebeispiel wird veranschaulicht, eine vertikal ausgerichtet [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) , in dem jedes untergeordnete Element [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) legt seine [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) Eigenschaft einer der vier Erweiterung Felder aus der [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) Struktur:

```xaml
<StackLayout Margin="0,20,0,0">
  ...
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="Start" BackgroundColor="Gray" VerticalOptions="StartAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="Center" BackgroundColor="Gray" VerticalOptions="CenterAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="End" BackgroundColor="Gray" VerticalOptions="EndAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="Fill" BackgroundColor="Gray" VerticalOptions="FillAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
</StackLayout>
```

Die entsprechende C#-Code wird unten gezeigt:

```csharp
Content = new StackLayout
{
  Margin = new Thickness(0, 20, 0, 0),
  Children = {
    ...
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "StartAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.StartAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "CenterAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.CenterAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "EndAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.EndAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "FillAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.FillAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 }
  }
};
```

Der Code führt das Layout in den folgenden Screenshots dargestellt:

[![](layout-options-images/expansion.png "Erweiterung Layoutoptionen")](layout-options-images/expansion-large.png#lightbox "Erweiterung Layoutoptionen")

Jede [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) belegt die gleiche Menge an Speicherplatz innerhalb der [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/). Jedoch nur der endgültige `Label`, welche legt seine [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) Eigenschaft [ `FillAndExpand` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/) eine andere Größe hat. Darüber hinaus jede `Label` wird getrennt durch ein kleines rotes [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/), wodurch den Speicherplatz der `Label` belegt, um ganz einfach angezeigt werden.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel erläutert die Auswirkungen, da jedes [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) Struktur-Wert aufweist, auf die Ausrichtung und die Erweiterung einer Ansicht, relativ zu seinem übergeordneten Element. Die `Start`, `Center`, `End`, und `Fill` Felder verwendet, um die Ansicht Ausrichtung innerhalb des übergeordneten Layouts definieren und die `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, und `FillAndExpand` Felder werden verwendet, um zu definieren Die Voreinstellung für die Ausrichtung, und um zu bestimmen, ob die Sicht mehr Speicherplatz belegen wird gegebenenfalls innerhalb einer [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/).



## <a name="related-links"></a>Verwandte Links

- [LayoutOptions (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/layoutoptions/)
- [LayoutOptions](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/)
