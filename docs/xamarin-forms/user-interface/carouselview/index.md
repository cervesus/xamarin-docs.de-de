---
Title: " Xamarin.Forms carouselview" Description: "carouselview ist eine Ansicht für die Darstellung von Daten in einem Bild lauffähigen Layout, in dem Benutzer eine Auflistung von Elementen bewegen können."
ms. Prod: xamarin ms. assetid: 5b673347-CDBA-4532-820b-beb5f 070c86bc ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 10/08/2019 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-carouselview"></a>Xamarin.FormsCarouselview

![](~/media/shared/preview.png "This API is currently pre-release")

## <a name="introduction"></a>[Introduction (Einführung)](introduction.md)

[`CarouselView`](xref:Xamarin.Forms.CarouselView)Ist eine Ansicht für die Darstellung von Daten in einem Bild lauffähigen Layout, in dem Benutzer zum Durchlaufen einer Auflistung von Elementen schwenken können.

## <a name="data"></a>[Daten](populate-data.md)

Ein [`CarouselView`](xref:Xamarin.Forms.CarouselView) wird mit Daten aufgefüllt, indem seine- [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) Eigenschaft auf eine beliebige Auflistung festgelegt wird, die implementiert `IEnumerable` . Die Darstellung der einzelnen Elemente kann definiert werden, indem die-Eigenschaft auf festgelegt wird [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) .

## <a name="layout"></a>[Layout](layout.md)

Standardmäßig [`CarouselView`](xref:Xamarin.Forms.CarouselView) werden die Elemente in einer horizontalen Liste angezeigt. Sie hat jedoch auch Zugriff auf dieselben Layouts wie CollectionView, einschließlich vertikaler Ausrichtung.

## <a name="interaction"></a>[Interaktion](interaction.md)

Auf das aktuell angezeigte Element in einem [`CarouselView`](xref:Xamarin.Forms.CarouselView) kann über die-Eigenschaft und die-Eigenschaft zugegriffen werden `CurrentItem` `Position` .

## <a name="empty-views"></a>[Leere Ansichten](emptyview.md)

In [`CarouselView`](xref:Xamarin.Forms.CarouselView) kann eine leere Ansicht angegeben werden, die dem Benutzer Feedback bietet, wenn keine Daten zur Anzeige verfügbar sind. Die leere Ansicht kann eine Zeichenfolge, eine Ansicht oder mehrere Ansichten sein.

## <a name="scrolling"></a>[Scrollen](scrolling.md)

Wenn ein Benutzer einen Bildlauf initiiert, kann die Endposition des Bildlaufs gesteuert werden, sodass Elemente vollständig angezeigt werden. Außerdem [`CarouselView`](xref:Xamarin.Forms.CarouselView) definiert zwei [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) Methoden, die Elemente Programm gesteuert in die Ansicht scrollen. Eine der-über Ladungen führt einen Bildlauf für das Element am angegebenen Index durch, während der andere das angegebene Element in die Ansicht verschiebt.
