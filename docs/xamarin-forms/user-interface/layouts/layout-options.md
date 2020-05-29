---
title: Layoutoptionen inXamarin.Forms
description: Jede Xamarin.Forms Ansicht verfügt über die Eigenschaften "horizontaloptions" und "verticaloptions" vom Typ "layoutoptions". In diesem Artikel werden die Auswirkungen der einzelnen layoutoptions-Werte auf die Ausrichtung und die Erweiterung einer Sicht erläutert.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 17f4e76f9bef71352cabddfba9397e95bcdd24d3
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138029"
---
# <a name="layout-options-in-xamarinforms"></a>Layoutoptionen inXamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layoutoptions)

_Jede Xamarin.Forms Ansicht verfügt über die Eigenschaften "horizontaloptions" und "verticaloptions" vom Typ "layoutoptions". In diesem Artikel werden die Auswirkungen der einzelnen layoutoptions-Werte auf die Ausrichtung und die Erweiterung einer Sicht erläutert._

## <a name="overview"></a>Übersicht

Die [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) Struktur kapselt zwei Layouteinstellungen:

- **Ausrichtung** – die bevorzugte Ausrichtung der Ansicht, die ihre Position und Größe innerhalb des übergeordneten Layouts bestimmt.
- **Erweiterung** – wird nur von einer verwendet [`StackLayout`](xref:Xamarin.Forms.StackLayout) und gibt an, ob die Sicht zusätzlichen Speicherplatz verwenden soll, wenn Sie verfügbar ist.

Diese Layouteinstellungen können relativ zu ihrem übergeordneten Element angewendet werden, [`View`](xref:Xamarin.Forms.View) indem die- [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) Eigenschaft oder die- [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) Eigenschaft des `View` auf eines der öffentlichen Felder aus der-Struktur festgelegt wird [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) . Die öffentlichen Felder lauten wie folgt:

- [`Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

Die `Start` `Center` Felder,, `End` und `Fill` dienen zum Definieren der Ausrichtung der Ansicht innerhalb des übergeordneten Layouts:

- Positioniert bei horizontaler Ausrichtung [`Start`](xref:Xamarin.Forms.LayoutOptions.Start) den [`View`](xref:Xamarin.Forms.View) auf der linken Seite des übergeordneten Layouts, und bei vertikaler Ausrichtung wird der am `View` oberen Rand des übergeordneten Layouts positioniert.
- Bei horizontaler und vertikaler Ausrichtung [`Center`](xref:Xamarin.Forms.LayoutOptions.Center) zentriert die horizontal oder vertikal [`View`](xref:Xamarin.Forms.View) .
- Positioniert bei horizontaler Ausrichtung [`End`](xref:Xamarin.Forms.LayoutOptions.End) den [`View`](xref:Xamarin.Forms.View) auf der rechten Seite des übergeordneten Layouts, und bei vertikaler Ausrichtung wird der am `View` unteren Rand des übergeordneten Layouts positioniert.
- Bei horizontaler Ausrichtung wird [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) von sichergestellt, dass die [`View`](xref:Xamarin.Forms.View) Breite des übergeordneten Layouts füllt. bei vertikaler Ausrichtung wird dadurch sichergestellt, dass die `View` Höhe des übergeordneten Layouts füllt.

Die `StartAndExpand` `CenterAndExpand` -, `EndAndExpand` -,-und- `FillAndExpand` Werte werden verwendet, um die Ausrichtungs Einstellung zu definieren und anzugeben, ob die Ansicht mehr Platz einnimmt, wenn Sie im übergeordneten Element verfügbar ist [`StackLayout`](xref:Xamarin.Forms.StackLayout) .

> [!NOTE]
> Der Standardwert der Eigenschaften [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) und [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) einer Ansicht lautet [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill).

<a name="alignment" />

## <a name="alignment"></a>Ausrichtung

Ausrichtung steuert, wie eine Ansicht innerhalb des übergeordneten Layouts positioniert wird, wenn das übergeordnete Layout nicht verwendeten Platz enthält (das heißt, das übergeordnete Layout ist größer als die kombinierte Größe aller seiner untergeordneten Elemente).

Ein berücksichtigt [`StackLayout`](xref:Xamarin.Forms.StackLayout) nur die `Start` Felder,, und in untergeordneten Sichten, die sich `Center` `End` `Fill` [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) in umgekehrter Richtung zur `StackLayout` Ausrichtung befinden. Daher können untergeordnete Sichten innerhalb eines vertikal ausgerichteten `StackLayout` Ihre [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) Eigenschaften auf eines der `Start` `Center` Felder,, oder festlegen `End` `Fill` . Ebenso können untergeordnete Sichten innerhalb einer horizontalen Ausrichtung `StackLayout` Ihre [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) Eigenschaften auf eines der `Start` `Center` Felder,, oder festlegen `End` `Fill` .

[`StackLayout`](xref:Xamarin.Forms.StackLayout)Die `Start` `Center` -,-, `End` -und- `Fill` [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) Felder für untergeordnete Sichten, die die gleiche Richtung wie die Ausrichtung haben, werden von `StackLayout` nicht berücksichtigt. Daher ignoriert ein vertikal orientierter `StackLayout` die `Start` Felder, `Center` , `End` oder, `Fill` Wenn Sie für die Eigenschaften von untergeordneten Ansichten festgelegt sind [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) . Analog dazu ignoriert eine horizontal ausgerichtete `StackLayout` die `Start` `Center` Felder,, oder, `End` `Fill` Wenn Sie für die Eigenschaften von untergeordneten Sichten festgelegt sind [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) .

> [!NOTE]
> [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)in der Regel werden die mit den Eigenschaften und angegebenen Größenanforderungen überschrieben [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) .

Das folgende XAML-Codebeispiel veranschaulicht eine vertikale [`StackLayout`](xref:Xamarin.Forms.StackLayout) Ausrichtung, bei der jedes untergeordnete Element [`Label`](xref:Xamarin.Forms.Label) seine- [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) Eigenschaft auf eines der vier Ausrichtungs Felder aus der- [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) Struktur festlegt:

```xaml
<StackLayout Margin="0,20,0,0">
  ...
  <Label Text="Start" BackgroundColor="Gray" HorizontalOptions="Start" />
  <Label Text="Center" BackgroundColor="Gray" HorizontalOptions="Center" />
  <Label Text="End" BackgroundColor="Gray" HorizontalOptions="End" />
  <Label Text="Fill" BackgroundColor="Gray" HorizontalOptions="Fill" />
</StackLayout>
```

Der entsprechende c#-Code wird unten dargestellt:

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

Der Code führt zu dem Layout, das in den folgenden Screenshots angezeigt wird:

[![](layout-options-images/alignment.png "Alignment Layout Options")](layout-options-images/alignment-large.png#lightbox "Alignment Layout Options")

<a name="expansion" />

## <a name="expansion"></a>Ausdehnung

Erweiterung steuert, ob eine Ansicht in einer mehr Speicherplatz einnimmt (falls verfügbar) [`StackLayout`](xref:Xamarin.Forms.StackLayout) . Wenn der `StackLayout` nicht verwendeten Speicherplatz enthält (d. h., `StackLayout` ist größer als die kombinierte Größe aller seiner untergeordneten Elemente), wird der nicht verwendete Speicherplatz gleichmäßig von allen untergeordneten Sichten gemeinsam genutzt, die eine Erweiterung anfordern, indem die-Eigenschaft [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) oder die-Eigenschaft [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) auf ein [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) Feld mit dem- `AndExpand` Suffix Beachten Sie, dass `StackLayout` die Erweiterungsoptionen keine Auswirkung haben, wenn der gesamte Speicherplatz in der verwendet wird.

Eine [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse kann Ansichten untergeordneter Elemente nur in die Orientierungsrichtung erweitern. Aus diesem Grund kann ein vertikal orientierter `StackLayout` untergeordnete Sichten erweitern, deren [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) Eigenschaften auf eines der `StartAndExpand` Felder,, oder festgelegt `CenterAndExpand` `EndAndExpand` `FillAndExpand` werden, wenn der nicht `StackLayout` verwendeten Speicherplatz enthält. Auf ähnliche Weise kann eine horizontal ausgerichtete `StackLayout` untergeordnete Sichten erweitern, die Ihre [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) Eigenschaften auf eines `StartAndExpand` der `CenterAndExpand` Felder,, `EndAndExpand` oder festlegen `FillAndExpand` , wenn der nicht `StackLayout` verwendeten Speicherplatz enthält.

Ein [`StackLayout`](xref:Xamarin.Forms.StackLayout) kann untergeordnete Sichten nicht in der Richtung erweitern, die der Ausrichtung entspricht. Daher hat das Festlegen der-Eigenschaft in einer untergeordneten Ansicht auf eine vertikale Ausrichtung `StackLayout` [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) [`StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand) denselben Effekt wie das Festlegen der-Eigenschaft auf [`Start`](xref:Xamarin.Forms.LayoutOptions.Start) .

> [!NOTE]
> Beachten Sie, dass durch das Aktivieren der Erweiterung die Größe einer Ansicht nur geändert wird, wenn Sie verwendet wird [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) .

Das folgende XAML-Codebeispiel veranschaulicht eine vertikale Ausrichtung [`StackLayout`](xref:Xamarin.Forms.StackLayout) , bei der jedes untergeordnete Element [`Label`](xref:Xamarin.Forms.Label) seine- [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) Eigenschaft auf eines der vier Erweiterungs Felder aus der- [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) Struktur festlegt:

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

Der entsprechende c#-Code wird unten dargestellt:

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

Der Code führt zu dem Layout, das in den folgenden Screenshots angezeigt wird:

[![](layout-options-images/expansion.png "Expansion Layout Options")](layout-options-images/expansion-large.png#lightbox "Expansion Layout Options")

Jede [`Label`](xref:Xamarin.Forms.Label) beansprucht die gleiche Menge an Speicherplatz in [`StackLayout`](xref:Xamarin.Forms.StackLayout) . Allerdings hat nur die letzte `Label`-Klasse, die ihre [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions)-Eigenschaft auf [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) festlegt, eine andere Größe. Darüber hinaus wird jeder `Label` durch einen kleinen roten getrennt [`BoxView`](xref:Xamarin.Forms.BoxView) , wodurch der Platz, den die `Label` einnimmt, problemlos angezeigt werden kann.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie sich jeder [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) Struktur Wert auf die Ausrichtung und Erweiterung einer Ansicht bezieht, relativ zum übergeordneten Element. Die `Start` `Center` Felder,, `End` und `Fill` werden verwendet, um die Ausrichtung der Ansicht innerhalb des übergeordneten Layouts zu definieren. `StartAndExpand` die `CenterAndExpand` Felder,, `EndAndExpand` und `FillAndExpand` werden verwendet, um die Ausrichtungs Einstellung zu definieren und um zu bestimmen, ob die Ansicht in einer mehr Speicherplatz einnimmt (falls verfügbar) [`StackLayout`](xref:Xamarin.Forms.StackLayout) .

## <a name="related-links"></a>Verwandte Links

- [Layoutoptions (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layoutoptions)
- [LayoutOptions](xref:Xamarin.Forms.LayoutOptions)
