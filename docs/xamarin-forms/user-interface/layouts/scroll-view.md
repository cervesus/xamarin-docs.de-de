---
Title: " Xamarin.Forms ScrollView" Description: "in diesem Artikel wird erläutert, wie Sie mit der Xamarin.Forms ScrollView-Klasse Layouts darstellen, die nicht nur auf einen Bildschirm passen und auf denen Inhalte Platz für die Tastatur machen."
ms. Prod: ms. assetid: ms. Technology: Author: ms. Author: ms. Date: NO-LOC:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

---

# <a name="xamarinforms-scrollview"></a>Xamarin.FormsScrollView

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)

[`ScrollView`](xref:Xamarin.Forms.ScrollView)enthält Layouts und ermöglicht den Bildlauf nach unten. `ScrollView`wird auch verwendet, um zuzulassen, dass Ansichten automatisch in den sichtbaren Teil des Bildschirms verschoben werden, wenn die Tastatur angezeigt wird.

[![](scroll-view-images/layouts-sml.png "Xamarin.Forms Layouts")](scroll-view-images/layouts.png#lightbox "Xamarin.Forms Layouts")

## <a name="purpose"></a>Zweck

[`ScrollView`](xref:Xamarin.Forms.ScrollView)kann verwendet werden, um sicherzustellen, dass größere Ansichten auf kleineren Telefonen gut angezeigt werden. Beispielsweise kann ein Layout, das auf einem iPhone 6S funktioniert, auf ein iPhone 4S zugeschnitten werden. Wenn Sie verwenden `ScrollView` , können die ausgeschnittenen Teile des Layouts auf dem kleineren Bildschirm angezeigt werden.

## <a name="usage"></a>Verbrauch

> [!NOTE]
> [`ScrollView`](xref:Xamarin.Forms.ScrollView)Objekte sollten nicht in den Netz Körper eingefügt werden. Außerdem `ScrollView` sollten s nicht mit anderen Steuerelementen, die einen Bildlauf wie und bereitstellen, mit einem Bildlauf versehen werden `ListView` `WebView` .

[`ScrollView`](xref:Xamarin.Forms.ScrollView)macht eine- `Content` Eigenschaft verfügbar, die auf eine einzelne Ansicht oder ein Layout festgelegt werden kann. Betrachten Sie dieses Beispiel für ein Layout mit einer sehr großen boxview, gefolgt von einem `Entry` :

```xaml
<ContentPage.Content>
    <ScrollView>
        <StackLayout>
            <BoxView BackgroundColor="Red" HeightRequest="600" WidthRequest="150" />
            <Entry />
        </StackLayout>
    </ScrollView>
</ContentPage.Content>
```

In C#:

```csharp
public class ScrollingDemoCode : ContentPage
{
    public ScrollingDemoCode()
    {
        StackLayout stackLayout = new StackLayout();
        stackLayout.Children.Add(new BoxView { BackgroundColor = Color.Red, HeightRequest = 600, WidthRequest = 150 });
        stackLayout.Children.Add(new Entry());
        ScrollView scrollView = new ScrollView();
        scrollView.Content = stackLayout;
        Content = scrollView;
    }
}
```

Bevor der Benutzer einen Bildlauf durchführt, wird nur der angezeigt `BoxView` :

![](scroll-view-images/scroll-start.png "BoxView in ScrollView")

Beachten Sie Folgendes: Wenn der Benutzer mit der Eingabe von Text in der beginnt `Entry` , wird die Ansicht mit einem Bildlauf fortgeführt

![](scroll-view-images/scroll-end.png "Entry in ScrollView")

## <a name="properties"></a>Eigenschaften

[`ScrollView`](xref:Xamarin.Forms.ScrollView)definiert die folgenden Eigenschaften:

- [`ContentSize`](xref:Xamarin.Forms.ScrollView.ContentSizeProperty)Ruft einen [`Size`](xref:Xamarin.Forms.Size) Wert ab, der die Größe des Inhalts darstellt.
- [`Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty)Ruft einen- [`ScrollOrientation`](xref:Xamarin.Forms.ScrollOrientation) Enumerationswert ab, der die Scrollrichtung der darstellt, oder legt diesen fest `ScrollView` .
- [`ScrollX`](xref:Xamarin.Forms.ScrollView.ScrollXProperty)Ruft eine ab `double` , die die aktuelle X-Scrollposition darstellt.
- [`ScrollY`](xref:Xamarin.Forms.ScrollView.ScrollYProperty)Ruft eine ab `double` , die die aktuelle Y-Scrollposition darstellt.
- [`HorizontalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.HorizontalScrollBarVisibilityProperty)Ruft einen Wert ab [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility) , der angibt, wann die horizontale Schiebe Leiste sichtbar ist, oder legt diesen fest.
- [`VerticalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.VerticalScrollBarVisibilityProperty)Ruft einen Wert ab [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility) , der angibt, wann die vertikale Schiebe Leiste sichtbar ist, oder legt diesen fest.

> [!NOTE]
> Das Scrollen kann durch Festlegen der- [`Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) Eigenschaft auf deaktiviert werden `Neither` .

## <a name="methods"></a>Methoden

[`ScrollView`](xref:Xamarin.Forms.ScrollView)stellt eine- `ScrollToAsync` Methode bereit, die verwendet werden kann, um entweder mithilfe von Koordinaten oder durch Angeben einer bestimmten Sicht, die sichtbar gemacht werden soll, einen Bildlauf der Sicht durchführen

Wenn Sie Koordinaten verwenden, geben Sie die `x` `y` Koordinaten und sowie einen booleschen Wert an, der angibt, ob der Bildlauf animiert werden soll:

```csharp
scroll.ScrollToAsync(0, 150, true); //scrolls so that the position at 150px from the top is visible

scroll.ScrollToAsync(label, ScrollToPosition.Start, true); //scrolls so that the label is at the start of the list
```

> [!IMPORTANT]
> Die- `ScrollToAsync` Methode führt nicht zu einem Bildlauf, wenn die- [`ScrollView.Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) Eigenschaft auf festgelegt ist `Neither` .

Beim Scrollen zu einem bestimmten Element gibt die- `ScrollToPosition` Enumeration an, wo in der Ansicht das Element angezeigt wird:

- **Zentrieren** &ndash; führt im-Element einen Bildlauf zur Mitte des sichtbaren Teils der Sicht durch.
- **Ende** &ndash; führt im-Element einen Bildlauf bis zum Ende des sichtbaren Teils der Ansicht durch.
- **MakeVisible** &ndash; führt einen Bildlauf durch das-Element durch, sodass es in der Ansicht sichtbar ist.
- **Starten** &ndash; Sie führt im-Element einen Bildlauf zum Anfang des sichtbaren Teils der Sicht durch.

Die-Eigenschaft gibt an, wie ein Rollup `IsAnimated` der Sicht ausgeführt wird. Wenn diese Einstellung auf festgelegt `true` ist, wird eine Smooth Animation verwendet, anstatt den Inhalt sofort in die Ansicht zu verschieben.

## <a name="events"></a>Events

[`ScrollView`](xref:Xamarin.Forms.ScrollView)definiert nur ein Ereignis, `Scrolled` . `Scrolled`wird ausgelöst, wenn die Ansicht den Bildlauf abgeschlossen hat. Der Ereignishandler für `Scrolled` nimmt `ScrolledEventArgs` , der die-Eigenschaft und die-Eigenschaft aufweist `ScrollX` `ScrollY` . Im folgenden wird veranschaulicht, wie eine Bezeichnung mit der aktuellen Scrollposition eines aktualisiert wird `ScrollView` :

```csharp
Label label = new Label { Text = "Position: " };
ScrollView scroll = new ScrollView();
scroll.Scrolled += (object sender, ScrolledEventArgs e) => {
    label.Text = "Position: " + e.ScrollX + " x " + e.ScrollY;
};
```

Beachten Sie, dass scrollpositionen aufgrund des Sprung Effekts beim Scrollen am Ende einer Liste negativ sein können.

## <a name="related-links"></a>Verwandte Links

- [Layout (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [Businesstumm Beispiel (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-businesstumble)
