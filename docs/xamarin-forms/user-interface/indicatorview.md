---
title: Xamarin. Forms-Anzeige Ansicht
description: Die "sichorview" ist ein Steuerelement, das Indikatoren anzeigt, die die Anzahl der Elemente und die aktuelle Position in einer "carouselview" darstellen.
ms.prod: xamarin
ms.assetId: BBCC223B-4B02-46B7-80BB-EE0E86A67CE2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/17/2019
ms.openlocfilehash: a5a9daa39dcc94bbf77d9c91ea651bda6ec5747b
ms.sourcegitcommit: 524fc148bad17272bda83c50775771daa45bfd7e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2020
ms.locfileid: "77480553"
---
# <a name="xamarinforms-indicatorview"></a>Xamarin. Forms-Anzeige Ansicht

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/)

Der `IndicatorView` ist ein Steuerelement, das Indikatoren anzeigt, die die Anzahl der Elemente und die aktuelle Position in einem `CarouselView`darstellen:

[![Screenshot von "carouselview" und "indikatorview" unter IOS und Android](indicatorview-images/circles.png "Sichorview-Kreise")](indicatorview-images/circles-large.png#lightbox "Sichorview-Kreise")

`IndicatorView` ist in xamarin. Forms 4,4 auf den IOS-und Android-Plattformen verfügbar. Es ist jedoch zurzeit experimentell und kann nur verwendet werden, wenn Sie Ihrer `AppDelegate`-Klasse unter IOS oder ihrer `MainActivity`-Klasse unter Android die folgende Codezeile hinzufügen, bevor Sie `Forms.Init`aufrufen:

```csharp
Forms.SetFlags("IndicatorView_Experimental");
```

`IndicatorView` definiert die folgenden Eigenschaften:

- `Count`vom Typ `int`die Anzahl der Indikatoren.
- `HideSingle`vom Typ `bool`gibt an, ob der Indikator ausgeblendet werden soll, wenn nur ein solcher vorhanden ist. Standardwert: `true`.
- `IndicatorColor`vom Typ `Color`die Farbe der Indikatoren.
- `IndicatorSize`vom Typ `double`die Größe der Indikatoren. Der Standardwert ist 6,0.
- `IndicatorLayout`vom Typ `Layout<View>`definiert die zum Rendering der `IndicatorView`verwendete Layoutklasse. Diese Eigenschaft wird von xamarin. Forms festgelegt und muss in der Regel nicht von Entwicklern festgelegt werden.
- `IndicatorTemplate`vom Typ `DataTemplate`die Vorlage, die die Darstellung der einzelnen Indikatoren definiert.
- `IndicatorsShape`vom Typ `IndicatorShape`die Form jedes Indikators.
- `ItemsSource`vom Typ `IEnumerable`die Auflistung, für die Indikatoren angezeigt werden. Diese Eigenschaft wird automatisch festgelegt, wenn die `ItemsSourceBy` angefügte Eigenschaft festgelegt wird.
- `ItemsSourceBy`vom Typ `VisualElement`, das `CarouselView` Objekt, für das Indikatoren angezeigt werden sollen. Dies ist eine angefügte Eigenschaft.
- `MaximumVisible`vom Typ `int`die maximale Anzahl sichtbarer Indikatoren. Standardwert: `int.MaxValue`.
- `Position`vom Typ `int`den aktuell ausgewählten Indikator Index. Diese Eigenschaft verwendet eine `TwoWay` Bindung. Diese Eigenschaft wird automatisch festgelegt, wenn die `ItemsSourceBy` angefügte Eigenschaft festgelegt wird.
- `SelectedIndicatorColor`vom Typ `Color`die Farbe des Indikators, der das aktuelle Element in der `CarouselView`darstellt.

Diese Eigenschaften werden [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekten unterstützt. Dies bedeutet, dass Sie Ziele von Daten Bindungen sein können und formatiert sind.

## <a name="create-an-indicatorview"></a>Erstellen einer "indikatorview"

Im folgenden Beispiel wird gezeigt, wie ein `IndicatorView` in XAML instanziiert wird:

```xaml
<StackLayout>
    <CarouselView x:Name="carouselView"
                  ItemsSource="{Binding Monkeys}">
        <CarouselView.ItemTemplate>
            <!-- DataTemplate that defines item appearance -->
        </CarouselView.ItemTemplate>
    </CarouselView>
    <IndicatorView IndicatorView.ItemsSourceBy="carouselView"
                   IndicatorColor="LightGray"
                   SelectedIndicatorColor="DarkGray"
                   HorizontalOptions="Center" />
</StackLayout>
```

In diesem Beispiel wird der `IndicatorView` unterhalb des `CarouselView`gerendert, mit einem Indikator für jedes Element im `CarouselView`. Die `IndicatorView` wird mit Daten aufgefüllt, indem die `ItemsSourceBy` angefügte-Eigenschaft auf das `CarouselView`-Objekt festgelegt wird. Jeder Indikator ist ein heller grauer Kreis, während der Indikator, der das aktuelle Element im `CarouselView` darstellt, dunkelgrau ist.

> [!IMPORTANT]
> Wenn Sie die `ItemsSourceBy` angefügte Eigenschaft festlegen, wird die `Position`-Eigenschaft an die `CarouselView.Position`-Eigenschaft gebunden, und die `ItemsSource` Eigenschaften Bindung an die `CarouselView.ItemsSource`-Eigenschaft.

## <a name="change-indicator-shape"></a>Form "Indikator ändern"

Die `IndicatorView`-Klasse verfügt über eine `IndicatorsShape`-Eigenschaft, die die Form der Indikatoren angibt. Diese Eigenschaft kann auf einen der `IndicatorShape` Enumerationsmember festgelegt werden:

- `Circle` gibt an, dass die Indikator Formen zirkulär sein werden. Dies ist der Standardwert der `IndicatorView.IndicatorsShape`-Eigenschaft.
- `Square` gibt an, dass die Indikator Formen quadratisch sind.

Das folgende Beispiel zeigt eine `IndicatorView`, die für die Verwendung von quadratischen Indikatoren konfiguriert ist:

```xaml
<IndicatorView IndicatorsShape="Square"
               IndicatorView.ItemsSourceBy="carouselView"
               IndicatorColor="LightGray"
               SelectedIndicatorColor="DarkGray" />
```

## <a name="define-indicator-appearance"></a>Indikator Darstellung definieren

Die Darstellung der einzelnen Indikatoren kann definiert werden, indem Sie die `IndicatorView.IndicatorTemplate`-Eigenschaft auf einen [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)festlegen:

```xaml
<StackLayout>
    <CarouselView x:Name="carouselView"
                  ItemsSource="{Binding Monkeys}">
        <CarouselView.ItemTemplate>
            <!-- DataTemplate that defines item appearance -->
        </CarouselView.ItemTemplate>
    </CarouselView>
    <IndicatorView IndicatorView.ItemsSourceBy="carouselView"
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

Die im [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) angegebenen Elemente definieren die Darstellung der einzelnen Indikatoren. In diesem Beispiel ist jeder Indikator eine [`Image`](xref:Xamarin.Forms.Image) , die ein Schriftart Symbol mithilfe der `FontImage` Markup Erweiterung anzeigt.

Die folgenden Screenshots zeigen Indikatoren, die mithilfe eines Schriftart Symbols gerendert werden:

[![Screenshot einer Vorlagen basierten Anzeige Ansicht unter IOS und Android](indicatorview-images/templated.png "Vorlagenbasierte indikatorview")](indicatorview-images/templated-large.png#lightbox "Vorlagenbasierte indikatorview")

Weitere Informationen zur `FontImage` Markup Erweiterung finden Sie unter [fontimage Markup Extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#fontimage-markup-extension).

## <a name="related-links"></a>Verwandte Links

- [Indikator orview (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/)
- [FontImage-Markuperweiterung](~/xamarin-forms/xaml/markup-extensions/consuming.md#fontimage-markup-extension)
