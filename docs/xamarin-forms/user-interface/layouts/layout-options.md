---
title: Layoutoptionen in Xamarin.Forms
description: Alle Xamarin.Forms-Sicht hat es sich um HorizontalOptions "und" Eigenschaften "verticaloptions" Eigenschaften des Typs LayoutOptions. Dieser Artikel beschreibt die Auswirkungen jeder LayoutOptions-Wert für die Ausrichtung und die Erweiterung einer Ansicht.
ms.prod: xamarin
ms.assetid: 7CAB5631-5153-4DEF-8AD7-C6011CE44307
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/10/2017
ms.openlocfilehash: 63c8cb285c51d7c10e2109c9d0b7cffbd0fb0898
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70770333"
---
# <a name="layout-options-in-xamarinforms"></a>Layoutoptionen in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layoutoptions)

_Alle Xamarin.Forms-Sicht hat es sich um HorizontalOptions "und" Eigenschaften "verticaloptions" Eigenschaften des Typs LayoutOptions. Dieser Artikel beschreibt die Auswirkungen jeder LayoutOptions-Wert für die Ausrichtung und die Erweiterung einer Ansicht._

## <a name="overview"></a>Übersicht

Die [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) Struktur kapselt zwei layouteinstellungen:

- **Ausrichtung** – die Ansicht die bevorzugte Ausrichtung, die die Position und Größe innerhalb des Layouts des übergeordneten bestimmt.
- **Erweiterung** : werden verwendet, nur von einem [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), und gibt an, ob die Ansicht diese zusätzlichen Leerräume verwenden sollten, sofern diese verfügbar ist.

Diese layouteinstellungen anwenden, um eine [ `View` ](xref:Xamarin.Forms.View)im Verhältnis zu seinem übergeordneten Element, durch Festlegen der [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) oder [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) Eigenschaft der `View` eines öffentlichen Felder aus der [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) Struktur. Die öffentlichen Felder sind wie folgt aus:

- [`Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

Die `Start`, `Center`, `End`, und `Fill` Felder verwendet, um die Ansicht die Ausrichtung innerhalb des Layouts des übergeordneten definieren:

- Für die horizontale Ausrichtung [ `Start` ](xref:Xamarin.Forms.LayoutOptions.Start) Positionen der [ `View` ](xref:Xamarin.Forms.View) auf der linken Seite des übergeordneten Layouts und für die vertikale Ausrichtung, die Position der `View` am oberen Rand der übergeordnete Layout.
- Für die horizontale und vertikale Ausrichtung [ `Center` ](xref:Xamarin.Forms.LayoutOptions.Center) horizontal oder vertikal zentriert die [ `View` ](xref:Xamarin.Forms.View).
- Für die horizontale Ausrichtung [ `End` ](xref:Xamarin.Forms.LayoutOptions.End) Positionen der [ `View` ](xref:Xamarin.Forms.View) auf der rechten Seite des übergeordneten Layouts und für die vertikale Ausrichtung, die Position der `View` am unteren Rand das übergeordnete-Layout.
- Für die horizontale Ausrichtung [ `Fill` ](xref:Xamarin.Forms.LayoutOptions.Fill) wird sichergestellt, dass die [ `View` ](xref:Xamarin.Forms.View) füllt die gesamte Breite des übergeordneten Layouts sowie für die vertikale Ausrichtung wird gewährleistet, dass die `View` füllt die die Höhe des übergeordneten Layouts.

Die `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, und `FillAndExpand` Werte werden verwendet, um die bevorzugte Ausrichtung definieren und gibt an, ob die Sicht mehr Speicherplatz gegebenenfalls innerhalb des übergeordneten Elements belegt wird [ `StackLayout` ](xref:Xamarin.Forms.StackLayout).

> [!NOTE]
> Der Standardwert einer Ansicht des [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) und [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) Eigenschaften [ `LayoutOptions.Fill` ](xref:Xamarin.Forms.LayoutOptions.Fill).

<a name="alignment" />

## <a name="alignment"></a>Ausrichtung

Ausrichtung steuert, wie eine Ansicht innerhalb des Layouts des übergeordneten positioniert wird, wenn das übergeordnete Layout nicht verwendeten Speicherplatz enthält (d. h. das Layout des übergeordneten ist größer als die kombinierte Größe aller untergeordneten Elemente).

Ein [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) berücksichtigt nur die `Start`, `Center`, `End`, und `Fill` [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) Felder auf untergeordneten Ansichten, die in die entgegengesetzte Richtung um die `StackLayout` Ausrichtung. Aus diesem Grund untergeordneten Ansichten in einem vertikal ausgerichteten `StackLayout` können festlegen, deren [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) Eigenschaften eines der `Start`, `Center`, `End`, oder `Fill` Felder. Auf ähnliche Weise untergeordneten Ansichten in einem horizontal ausgerichteten `StackLayout` können festlegen, deren [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) Eigenschaften eines der `Start`, `Center`, `End`, oder `Fill` Felder.

Ein [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) berücksichtigt nicht die `Start`, `Center`, `End`, und `Fill` [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) Felder auf untergeordneten Ansichten, die in die gleiche Richtung wie sind die `StackLayout` Ausrichtung. Aus diesem Grund einem vertikal ausgerichteten `StackLayout` ignoriert die `Start`, `Center`, `End`, oder `Fill` Felder, wenn sie festgelegt sind das [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) Eigenschaften von untergeordneten Ansichten. Auf ähnliche Weise einem horizontal ausgerichteten `StackLayout` ignoriert die `Start`, `Center`, `End`, oder `Fill` Felder, wenn sie festgelegt sind das [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) Eigenschaften von untergeordneten Ansichten.

> [!NOTE]
> [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) Außerkraftsetzungen Größe in der Regel Anforderungen angegeben, mit der [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) und [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) Eigenschaften.

Im folgenden XAML-Codebeispiel wird veranschaulicht, einem vertikal ausgerichteten [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) , in dem jedes untergeordnete Element [ `Label` ](xref:Xamarin.Forms.Label) legt die [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) Eigenschaft einer der vier Ausrichtung Felder aus der [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) Struktur:

```xaml
<StackLayout Margin="0,20,0,0">
  ...
  <Label Text="Start" BackgroundColor="Gray" HorizontalOptions="Start" />
  <Label Text="Center" BackgroundColor="Gray" HorizontalOptions="Center" />
  <Label Text="End" BackgroundColor="Gray" HorizontalOptions="End" />
  <Label Text="Fill" BackgroundColor="Gray" HorizontalOptions="Fill" />
</StackLayout>
```

Der entsprechende C#-Code wird unten gezeigt:

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

Der Code führt das Layout, die in den folgenden Screenshots gezeigt:

[![](layout-options-images/alignment.png "Optionen für textausrichtung Layout")](layout-options-images/alignment-large.png#lightbox "Ausrichtungsoptionen für Layout")

<a name="expansion" />

## <a name="expansion"></a>Erweiterung

Erweiterung wird gesteuert, ob eine Ansicht mehr Speicherplatz belegt wird gegebenenfalls in einem [ `StackLayout` ](xref:Xamarin.Forms.StackLayout). Wenn die `StackLayout` Speicherplatz enthält, (d. h. die `StackLayout` ist größer als die kombinierte Größe aller untergeordneten), der nicht verwendete Speicherplatz freigegeben ist gleichermaßen alle untergeordneten Ansichten, die Erweiterung durch Festlegen von anfordern ihrer [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions)oder [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) Eigenschaften, die eine [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) Feld, verwendet der `AndExpand` Suffix. Hinweis: Wenn der Speicherplatz für die in der `StackLayout` wird verwendet, die Erweiterungsoptionen haben keine Auswirkungen.

Ein [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) können nur untergeordnete Ansichten in Richtung der die Ausrichtung erweitern. Aus diesem Grund einem vertikal ausgerichteten `StackLayout` untergeordneten Ansichten, die festlegen können erweitern, ihre [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) Eigenschaften eines der `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, oder `FillAndExpand` Felder, wenn die `StackLayout` Speicherplatz enthält. Auf ähnliche Weise einem horizontal ausgerichteten `StackLayout` untergeordneten Ansichten, die festlegen können erweitern, ihre [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) Eigenschaften eines der `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, oder `FillAndExpand` Felder, wenn die `StackLayout` Speicherplatz enthält.

Ein [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) untergeordnete Ansichten in der Richtung entgegengesetzt der Ausrichtung nicht erweitert werden kann. Aus diesem Grund auf einem vertikal ausgerichteten `StackLayout`wird durch das Festlegen der [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) Eigenschaft für eine untergeordnete Ansicht zu [ `StartAndExpand` ](xref:Xamarin.Forms.LayoutOptions.StartAndExpand) hat dieselbe Wirkung wie das Festlegen der Eigenschaft auf [ `Start`](xref:Xamarin.Forms.LayoutOptions.Start).

> [!NOTE]
> Beachten Sie, dass das Aktivieren der Erweiterung der Größe einer Ansicht ändern nicht, es sei denn, er verwendet [ `LayoutOptions.FillAndExpand` ](xref:Xamarin.Forms.LayoutOptions.FillAndExpand).

Im folgenden XAML-Codebeispiel wird veranschaulicht, einem vertikal ausgerichteten [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) , in dem jedes untergeordnete Element [ `Label` ](xref:Xamarin.Forms.Label) legt die [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) Eigenschaft einer der vier Erweiterung Felder aus der [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) Struktur:

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

Der entsprechende C#-Code wird unten gezeigt:

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

Der Code führt das Layout, die in den folgenden Screenshots gezeigt:

[![](layout-options-images/expansion.png "Erweiterung Layoutoptionen")](layout-options-images/expansion-large.png#lightbox "Layoutoptionen Erweiterung")

Jede [ `Label` ](xref:Xamarin.Forms.Label) belegt die gleiche Menge an Speicherplatz innerhalb der [ `StackLayout` ](xref:Xamarin.Forms.StackLayout). Allerdings nur der letzte `Label`, welche Gruppen die [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) Eigenschaft [ `FillAndExpand` ](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) eine andere Größe hat. Darüber hinaus jede `Label` wird getrennt durch ein kleines rotes [ `BoxView` ](xref:Xamarin.Forms.BoxView), das es ermöglicht, des Speicherplatzes der `Label` belegt wird, um ganz einfach angezeigt werden.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel erläutert die Auswirkungen, die jeweils [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) Struktur-Wert aufweist, auf die Ausrichtung und die Erweiterung einer Ansicht, relativ zum übergeordneten Element. Die `Start`, `Center`, `End`, und `Fill` Felder verwendet, um die Ansicht die Ausrichtung innerhalb des Layouts des übergeordneten, definieren und die `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, und `FillAndExpand` Felder werden verwendet, um zu definieren Die Voreinstellung für die Ausrichtung, und bestimmen, ob die Sicht mehr Speicherplatz belegt wird falls verfügbar, in einem [ `StackLayout` ](xref:Xamarin.Forms.StackLayout).

## <a name="related-links"></a>Verwandte Links

- [LayoutOptions (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layoutoptions)
- [LayoutOptions](xref:Xamarin.Forms.LayoutOptions)
