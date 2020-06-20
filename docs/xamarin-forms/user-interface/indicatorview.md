---
title: Xamarin.FormsIndikator Ansicht
description: Die "sichorview" ist ein Steuerelement, das Indikatoren anzeigt, die die Anzahl der Elemente und die aktuelle Position in einer "carouselview" darstellen.
ms.prod: xamarin
ms.assetId: BBCC223B-4B02-46B7-80BB-EE0E86A67CE2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/27/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0e6d223fd10e7b792f2d145a7cdd417865a095bb
ms.sourcegitcommit: d86b7a18cf8b1ef28cd0fe1d311f1c58a65101a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2020
ms.locfileid: "85101431"
---
# <a name="xamarinforms-indicatorview"></a>Xamarin.FormsIndikator Ansicht

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/)

Das `IndicatorView` -Steuerelement ist ein Steuerelement, das Indikatoren anzeigt, die die Anzahl der Elemente und die aktuelle Position in einem darstellen `CarouselView` :

[![Screenshot von "carouselview" und "indikatorview" unter IOS und Android](indicatorview-images/circles.png "Sichorview-Kreise")](indicatorview-images/circles-large.png#lightbox "Sichorview-Kreise")

`IndicatorView` definiert die folgenden Eigenschaften:

- `Count`, vom Typ `int` , die Anzahl der Indikatoren.
- `HideSingle`Gibt an, `bool` ob der Indikator ausgeblendet werden soll, wenn nur ein solcher vorhanden ist. Der Standardwert ist `true`.
- `IndicatorColor`, vom Typ `Color` , die Farbe der Indikatoren.
- `IndicatorSize`, vom Typ `double` , die Größe der Indikatoren. Der Standardwert ist 6,0.
- `IndicatorLayout``Layout<View>`definiert die zum Rendering von verwendete Layoutklasse des Typs `IndicatorView` . Diese Eigenschaft wird von festgelegt Xamarin.Forms und muss in der Regel nicht von Entwicklern festgelegt werden.
- `IndicatorTemplate`, vom Typ `DataTemplate` , die Vorlage, die die Darstellung der einzelnen Indikatoren definiert.
- `IndicatorsShape`, vom Typ `IndicatorShape` , die Form jedes Indikators.
- `ItemsSource`, vom Typ `IEnumerable` , der Auflistung, für die Indikatoren angezeigt werden. Diese Eigenschaft wird automatisch festgelegt, wenn die- `CarouselView.IndicatorView` Eigenschaft festgelegt wird.
- `MaximumVisible`, vom Typ `int` , die maximale Anzahl sichtbarer Indikatoren. Der Standardwert ist `int.MaxValue`.
- `Position`, vom Typ `int` , der derzeit ausgewählte Indikator Index. Diese Eigenschaft verwendet eine- `TwoWay` Bindung. Diese Eigenschaft wird automatisch festgelegt, wenn die- `CarouselView.IndicatorView` Eigenschaft festgelegt wird.
- `SelectedIndicatorColor`, vom Typ `Color` , die Farbe des Indikators, der das aktuelle Element in der darstellt `CarouselView` .

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

## <a name="create-an-indicatorview"></a>Erstellen einer "indikatorview"

Im folgenden Beispiel wird gezeigt, wie ein `IndicatorView` in XAML instanziiert wird:

```xaml
<StackLayout>
    <CarouselView ItemsSource="{Binding Monkeys}"
                  IndicatorView="indicatorView">
        <CarouselView.ItemTemplate>
            <!-- DataTemplate that defines item appearance -->
        </CarouselView.ItemTemplate>
    </CarouselView>
    <IndicatorView x:Name="indicatorView"
                   IndicatorColor="LightGray"
                   SelectedIndicatorColor="DarkGray"
                   HorizontalOptions="Center" />
</StackLayout>
```

In diesem Beispiel wird der unter `IndicatorView` dem gerendert `CarouselView` , mit einem Indikator für jedes Element in der `CarouselView` . Die `IndicatorView` wird mit Daten aufgefüllt, indem die- `CarouselView.IndicatorView` Eigenschaft auf das-Objekt festgelegt wird `IndicatorView` . Jeder Indikator ist ein heller grauer Kreis, während der Indikator, der das aktuelle Element in darstellt, `CarouselView` dunkelgrau ist.

> [!IMPORTANT]
> `CarouselView.IndicatorView`Wenn Sie die-Eigenschaft festlegen `IndicatorView.Position` , wird die-Eigenschaft an die-Eigenschaft gebunden `CarouselView.Position` , und die-Eigenschaft wird `IndicatorView.ItemsSource` an die- `CarouselView.ItemsSource` Eigenschaft gebunden.

## <a name="change-indicator-shape"></a>Form "Indikator ändern"

Die- `IndicatorView` Klasse verfügt über eine- `IndicatorsShape` Eigenschaft, mit der die Form der Indikatoren bestimmt wird. Diese Eigenschaft kann auf einen der Enumerationsmember festgelegt werden `IndicatorShape` :

- `Circle`Gibt an, dass die Indikator Formen zirkulär sein werden. Dies ist der Standardwert der `IndicatorView.IndicatorsShape`-Eigenschaft.
- `Square`Gibt an, dass die Indikator Formen quadratisch sind.

Das folgende Beispiel zeigt einen `IndicatorView` , der für die Verwendung von quadratischen Indikatoren konfiguriert ist:

```xaml
<IndicatorView x:Name="indicatorView"
               IndicatorsShape="Square"
               IndicatorColor="LightGray"
               SelectedIndicatorColor="DarkGray" />
```

## <a name="change-indicator-size"></a>Indikator Größe ändern

Die- `IndicatorView` Klasse verfügt über eine- `IndicatorSize` Eigenschaft vom Typ `double` , mit der die Größe der Indikatoren in geräteunabhängigen Einheiten bestimmt wird. Der Standardwert dieser Eigenschaft ist 6,0.

Das folgende Beispiel zeigt einen `IndicatorView` , der für die Anzeige größerer Indikatoren konfiguriert ist:

```xaml
<IndicatorView x:Name="indicatorView"
               IndicatorSize="18" />
```

## <a name="limit-the-number-of-indicators-displayed"></a>Anzahl der angezeigten Indikatoren begrenzen

Die- `IndicatorView` Klasse verfügt über eine- `MaximumVisible` Eigenschaft vom Typ `int` , die die maximale Anzahl der sichtbaren Indikatoren bestimmt.

Das folgende Beispiel zeigt einen `IndicatorView` , der so konfiguriert ist, dass maximal sechs Indikatoren angezeigt werden:

```xaml
<IndicatorView x:Name="indicatorView"
               MaximumVisible="6" />
```

## <a name="define-indicator-appearance"></a>Indikator Darstellung definieren

Die Darstellung der einzelnen Indikatoren kann definiert werden, indem die-Eigenschaft auf festgelegt wird `IndicatorView.IndicatorTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) :

```xaml
<StackLayout>
    <CarouselView ItemsSource="{Binding Monkeys}"
                  IndicatorView="indicatorView">
        <CarouselView.ItemTemplate>
            <!-- DataTemplate that defines item appearance -->
        </CarouselView.ItemTemplate>
    </CarouselView>
    <IndicatorView x:Name="indicatorView"
                   IndicatorColor="LightGray"
                   SelectedIndicatorColor="Black"
                   HorizontalOptions="Center">
        <IndicatorView.IndicatorTemplate>
            <DataTemplate>
                <Image Source="{FontImage &#xf30c;, FontFamily={OnPlatform iOS=Ionicons, Android=ionicons.ttf#}, Size=12}" />
            </DataTemplate>
        </IndicatorView.IndicatorTemplate>
    </IndicatorView>
</StackLayout>
```

Die in der angegebenen Elemente [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) definieren die Darstellung der einzelnen Indikatoren. In diesem Beispiel ist jeder Indikator ein [`Image`](xref:Xamarin.Forms.Image) , der ein Schriftart Symbol mithilfe der `FontImage` Markup Erweiterung anzeigt.

Die folgenden Screenshots zeigen Indikatoren, die mithilfe eines Schriftart Symbols gerendert werden:

[![Screenshot einer Vorlagen basierten Anzeige Ansicht unter IOS und Android](indicatorview-images/templated.png "Vorlagenbasierte "indikatorview"")](indicatorview-images/templated-large.png#lightbox "Vorlagenbasierte "indikatorview"")

Weitere Informationen zur `FontImage` Markup Erweiterung finden Sie unter [fontimage Markup Extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#fontimage-markup-extension).

## <a name="related-links"></a>Verwandte Links

- [Indikator orview (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/)
- [FontImage-Markuperweiterung](~/xamarin-forms/xaml/markup-extensions/consuming.md#fontimage-markup-extension)
