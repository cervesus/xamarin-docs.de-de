---
Title: " Xamarin.Forms ScrollView" Description: "ScrollView Xamarin.Forms ist ein Layout, das den Inhalt des Bildlaufs durch Scrollen kann."
ms. Prod: xamarin ms. assetid: 7b542872-b3d1-49b3-b15e-0e98f 53c1f 6E ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 05/27/2020 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-scrollview"></a>Xamarin.FormsScrollView

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-scrollviewdemos)

[![Xamarin.FormsScrollView](scrollview-images/layouts.png "[! Schel. No-Loc (xamarin. Forms)] ScrollView")](scrollview-images/layouts-large.png#lightbox "[! Schel. No-Loc (xamarin. Forms)] ScrollView")

[`ScrollView`](xref:Xamarin.Forms.ScrollView)ist ein Layout, das ihren Inhalt Scrollen kann. Die `ScrollView` -Klasse wird von der [`Layout`](xref:Xamarin.Forms.Layout) -Klasse abgeleitet und führt standardmäßig einen Bildlauf des Inhalts durch. Ein `ScrollView` kann nur ein einzelnes untergeordnetes Element haben, obwohl dies andere Layouts sein kann.

> [!WARNING]
> [`ScrollView`](xref:Xamarin.Forms.ScrollView)Objekte sollten nicht in den Netz Körper eingefügt werden. Außerdem `ScrollView` sollten-Objekte nicht mit anderen Steuerelementen, die einen Bildlauf bereitstellen, wie [`CollectionView`](xref:Xamarin.Forms.CollectionView) , und, eingefügt werden [`ListView`](xref:Xamarin.Forms.ListView) [`WebView`](xref:Xamarin.Forms.WebView) .

[`ScrollView`](xref:Xamarin.Forms.ScrollView)definiert die folgenden Eigenschaften:

- [`Content`](xref:Xamarin.Forms.ScrollView.Content)[`View`](xref:Xamarin.Forms.View)stellt den Inhalt dar, der in der angezeigt werden soll [`ScrollView`](xref:Xamarin.Forms.ScrollView) .
- [`ContentSize`](xref:Xamarin.Forms.ScrollView), vom Typ [`Size`](xref:Xamarin.Forms.Size) , stellt die Größe des Inhalts dar. Dies ist eine schreibgeschützte Eigenschaft.
- [`HorizontalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView), vom Typ [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.HorizontalScrollBarVisibility) , stellt dar, wenn die horizontale Schiebe Leiste sichtbar ist.
- [`Orientation`](xref:Xamarin.Forms.ScrollView.Orientation), vom Typ [`ScrollOrientation`](xref:Xamarin.Forms.ScrollOrientation) , stellt die Scrollrichtung des dar [`ScrollView`](xref:Xamarin.Forms.ScrollView) . Der Standardwert dieser Eigenschaft ist `Vertical`.
- [`ScrollX`](xref:Xamarin.Forms.ScrollView.ScrollX)Gibt die aktuelle X-Bild Lauf Position des Typs an `double` . Der Standardwert dieser schreibgeschützten Eigenschaft ist 0.
- [`ScrollY`](xref:Xamarin.Forms.ScrollView.ScrollY)`double`gibt die aktuelle Y-Scrollposition des Typs an. Der Standardwert dieser schreibgeschützten Eigenschaft ist 0.
- [`VerticalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView), vom Typ [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.HorizontalScrollBarVisibility) , stellt dar, wenn die vertikale Schiebe Leiste sichtbar ist.

Diese Eigenschaften werden durch-Objekte unterstützt, [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) mit Ausnahme der- [`Content`](xref:Xamarin.Forms.ScrollView.Content) Eigenschaft. Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatierten sein können.

Die [`Content`](xref:Xamarin.Forms.ScrollView.Content) -Eigenschaft ist der der [`ContentProperty`](xref:Xamarin.Forms.ContentPropertyAttribute) [`ScrollView`](xref:Xamarin.Forms.ScrollView) -Klasse und muss daher nicht explizit aus XAML festgelegt werden.

> [!TIP]
> Um die bestmögliche layoutleistung zu erzielen, befolgen Sie die Richtlinien unter [Optimieren der layoutleistung](~/xamarin-forms/deploy-test/performance.md#optimize-layout-performance).

## <a name="scrollview-as-a-root-layout"></a>ScrollView als Stamm Layout

Ein [`ScrollView`](xref:Xamarin.Forms.ScrollView) kann nur über ein einzelnes untergeordnetes Element verfügen, das andere Layouts aufweisen kann. Daher ist es üblich, dass ein `ScrollView` das Stamm Layout auf einer Seite ist. Um einen Bildlauf für den untergeordneten Inhalt durchführen zu können, wird [`ScrollView`](xref:Xamarin.Forms.ScrollView) der Unterschied zwischen der Höhe des Inhalts und der eigenen Höhe berechnet. Dieser Unterschied ist der Betrag, den der durch den `ScrollView` Inhalt Scrollen kann.

Eine ist [`StackLayout`](xref:Xamarin.Forms.StackLayout) oft das untergeordnete Element eines `ScrollView` . In diesem Szenario bewirkt, `ScrollView` dass die `StackLayout` so groß wie die Summe der Höhe der untergeordneten Elemente ist. Anschließend `ScrollView` kann der den Umfang der Inhalte ermitteln, die durch den Inhalt des Inhalts berechnet werden können. Weitere Informationen zu finden Sie unter `StackLayout` [ Xamarin.Forms Stacklayout](stacklayout.md).

> [!CAUTION]
> Vermeiden Sie in einem vertikalen, dass [`ScrollView`](xref:Xamarin.Forms.ScrollView) die- `VerticalOptions` Eigenschaft auf, oder festgelegt wird `Start` `Center` `End` . Dies weist das `ScrollView` an, nur so groß wie das zu sein, das NULL sein muss. Während Xamarin.Forms sich gegen diese Eventualität schützt, ist es am besten, Code zu vermeiden, der etwas vorschlägt, das Sie nicht möchten.

Das folgende XAML-Beispiel enthält ein-Element [`ScrollView`](xref:Xamarin.Forms.ScrollView) als Stamm Layout auf einer Seite:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ScrollViewDemos"
             x:Class="ScrollViewDemos.Views.ColorListPage"
             Title="ScrollView demo">
    <ScrollView>
        <StackLayout BindableLayout.ItemsSource="{x:Static local:NamedColor.All}">
            <BindableLayout.ItemTemplate>
                <DataTemplate>
                    <StackLayout Orientation="Horizontal">
                        <BoxView Color="{Binding Color}"
                                 HeightRequest="32"
                                 WidthRequest="32"
                                 VerticalOptions="Center" />
                        <Label Text="{Binding FriendlyName}"
                               FontSize="24"
                               VerticalOptions="Center" />
                    </StackLayout>
                </DataTemplate>
            </BindableLayout.ItemTemplate>
        </StackLayout>
    </ScrollView>
</ContentPage>
```

In diesem Beispiel ist der [`ScrollView`](xref:Xamarin.Forms.ScrollView) Inhalt des auf einen festgelegt, [`StackLayout`](xref:Xamarin.Forms.StackLayout) der mithilfe eines bindbaren Layouts die [`Color`](xref:Xamarin.Forms.Color) von definierten Felder anzeigt Xamarin.Forms . Standardmäßig führt ein vertikal einen Bildlauf durch `ScrollView` , wodurch mehr Inhalt angezeigt wird:

[![Screenshot eines Stamm ScrollView-Layouts](scrollview-images/root-layout.png "Layout der ScrollView-Stammdatei")](scrollview-images/root-layout-large.png#lightbox "Layout der ScrollView-Stammdatei")

Der entsprechende C#-Code lautet:

```csharp
public class ColorListPageCode : ContentPage
{
    public ColorListPageCode()
    {
        DataTemplate dataTemplate = new DataTemplate(() =>
        {
            BoxView boxView = new BoxView
            {
                HeightRequest = 32,
                WidthRequest = 32,
                VerticalOptions = LayoutOptions.Center
            };
            boxView.SetBinding(BoxView.ColorProperty, "Color");

            Label label = new Label
            {
                FontSize = 24,
                VerticalOptions = LayoutOptions.Center
            };
            label.SetBinding(Label.TextProperty, "FriendlyName");

            StackLayout horizontalStackLayout = new StackLayout
            {
                Orientation = StackOrientation.Horizontal,
                Children = { boxView, label }
            };
            return horizontalStackLayout;
        });

        StackLayout stackLayout = new StackLayout();
        BindableLayout.SetItemsSource(stackLayout, NamedColor.All);
        BindableLayout.SetItemTemplate(stackLayout, dataTemplate);

        ScrollView scrollView = new ScrollView { Content = stackLayout };

        Title = "ScrollView demo";
        Content = scrollView;
    }
}
```

Weitere Informationen zu bindbaren Layouts finden Sie unter [bindbare Layouts in Xamarin.Forms ](bindable-layouts.md).

## <a name="scrollview-as-a-child-layout"></a>ScrollView als untergeordnetes Layout

Ein [`ScrollView`](xref:Xamarin.Forms.ScrollView) kann ein untergeordnetes Layout für ein anderes übergeordnetes Layout sein.

Eine ist [`ScrollView`](xref:Xamarin.Forms.ScrollView) oft das untergeordnete Element eines [`StackLayout`](xref:Xamarin.Forms.StackLayout) . Eine `ScrollView` erfordert eine bestimmte Höhe, um den Unterschied zwischen der Höhe des Inhalts und der eigenen Höhe zu berechnen, wobei der Unterschied darin liegt, dass der den Inhalt durchlaufen `ScrollView` kann. Wenn ein untergeordnetes Element eines `ScrollView` ist `StackLayout` , erhält es keine bestimmte Höhe. Der `StackLayout` wünscht `ScrollView` , dass die so kurz wie möglich ist, d. h. die Höhe des `ScrollView` Inhalts oder 0 (null). Um dieses Szenario zu behandeln, `VerticalOptions` sollte die-Eigenschaft von `ScrollView` auf festgelegt werden `FillAndExpand` . Dadurch erhält der den `StackLayout` `ScrollView` gesamten zusätzlichen Platz, der für die anderen untergeordneten Elemente nicht erforderlich ist, und der `ScrollView` erhält dann eine bestimmte Höhe.

Das folgende XAML-Beispiel verfügt über ein [`ScrollView`](xref:Xamarin.Forms.ScrollView) als untergeordnetes Layout für ein-Element [`StackLayout`](xref:Xamarin.Forms.StackLayout) :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ScrollViewDemos.Views.BlackCatPage"
             Title="ScrollView as a child layout demo">
    <StackLayout Margin="20">
        <Label Text="THE BLACK CAT by Edgar Allan Poe"
               FontSize="Medium"
               FontAttributes="Bold"
               HorizontalOptions="Center" />
        <ScrollView VerticalOptions="FillAndExpand">
            <StackLayout>
                <Label Text="FOR the most wild, yet most homely narrative which I am about to pen, I neither expect nor solicit belief. Mad indeed would I be to expect it, in a case where my very senses reject their own evidence. Yet, mad am I not -- and very surely do I not dream. But to-morrow I die, and to-day I would unburthen my soul. My immediate purpose is to place before the world, plainly, succinctly, and without comment, a series of mere household events. In their consequences, these events have terrified -- have tortured -- have destroyed me. Yet I will not attempt to expound them. To me, they have presented little but Horror -- to many they will seem less terrible than barroques. Hereafter, perhaps, some intellect may be found which will reduce my phantasm to the common-place -- some intellect more calm, more logical, and far less excitable than my own, which will perceive, in the circumstances I detail with awe, nothing more than an ordinary succession of very natural causes and effects." />
                <!-- More Label objects go here -->
            </StackLayout>
        </ScrollView>
    </StackLayout>
</ContentPage>
```

In diesem Beispiel gibt es zwei- [`StackLayout`](xref:Xamarin.Forms.StackLayout) Objekte. Der erste `StackLayout` ist das stammlayoutobjekt, das ein [`Label`](xref:Xamarin.Forms.Label) -Objekt und ein-Objekt als untergeordnete Elemente aufweist [`ScrollView`](xref:Xamarin.Forms.ScrollView) . Der `ScrollView` weist einen `StackLayout` als Inhalt auf, wobei `StackLayout` mehrere- `Label` Objekte enthält. Diese Anordnung stellt sicher, dass die erste `Label` immer auf dem Bildschirm ist, während der Text, der von den anderen Objekten angezeigt wird, `Label` gescrollt werden kann:

[![Screenshot eines untergeordneten ScrollView-Layouts](scrollview-images/child-layout.png "Layout der untergeordneten ScrollView")](scrollview-images/child-layout-large.png#lightbox "Layout der untergeordneten ScrollView")

Der entsprechende C#-Code lautet:

```csharp
public class BlackCatPageCS : ContentPage
{
    public BlackCatPageCS()
    {
        Label titleLabel = new Label
        {
            Text = "THE BLACK CAT by Edgar Allan Poe",
            // More properties set here to define the Label appearance
        };

        ScrollView scrollView = new ScrollView
        {
            VerticalOptions = LayoutOptions.FillAndExpand,
            Content = new StackLayout
            {
                Children =
                {
                    new Label
                    {
                        Text = "FOR the most wild, yet most homely narrative which I am about to pen, I neither expect nor solicit belief. Mad indeed would I be to expect it, in a case where my very senses reject their own evidence. Yet, mad am I not -- and very surely do I not dream. But to-morrow I die, and to-day I would unburthen my soul. My immediate purpose is to place before the world, plainly, succinctly, and without comment, a series of mere household events. In their consequences, these events have terrified -- have tortured -- have destroyed me. Yet I will not attempt to expound them. To me, they have presented little but Horror -- to many they will seem less terrible than barroques. Hereafter, perhaps, some intellect may be found which will reduce my phantasm to the common-place -- some intellect more calm, more logical, and far less excitable than my own, which will perceive, in the circumstances I detail with awe, nothing more than an ordinary succession of very natural causes and effects.",
                    },
                    // More Label objects go here
                }
            }
        };

        Title = "ScrollView as a child layout demo";
        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children = { titleLabel, scrollView }
        };
    }
}
```

## <a name="orientation"></a>Ausrichtung

[`ScrollView`](xref:Xamarin.Forms.ScrollView)verfügt über eine- [`Orientation`](xref:Xamarin.Forms.ScrollView.Orientation) Eigenschaft, die die Scrollrichtung des darstellt `ScrollView` . Diese Eigenschaft [`ScrollOrientation`](xref:Xamarin.Forms.ScrollOrientation) hat den Typ, der die folgenden Member definiert:

- `Vertical`Gibt an, dass der einen vertikalen Bildlauf durch `ScrollView` führt. Dieser Member ist der Standardwert der- [`Orientation`](xref:Xamarin.Forms.ScrollView.Orientation) Eigenschaft.
- `Horizontal`Gibt an, dass der einen horizontalen Bildlauf durch `ScrollView` führt.
- `Both`Gibt an, dass der einen `ScrollView` horizontalen und vertikalen Bildlauf durchführt.
- `Neither`Gibt an, dass der `ScrollView` nicht scrollen soll.

> [!TIP]
> Das Scrollen kann durch Festlegen der- [`Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) Eigenschaft auf deaktiviert werden `Neither` .

## <a name="detect-scrolling"></a>Scrollen erkennen

[`ScrollView`](xref:Xamarin.Forms.ScrollView)definiert ein [`Scrolled`](xref:Xamarin.Forms.ScrollView.Scrolled) Ereignis, das ausgelöst wird, um anzugeben, dass der Bildlauf ausgeführt wurde. Das [`ScrolledEventArgs`](xref:Xamarin.Forms.ScrolledEventArgs) Objekt, das das `Scrolled` Ereignis begleitet, verfügt über die `ScrollX` `ScrollY` Eigenschaften und, beide vom Typ `double` .

> [!IMPORTANT]
> Die `ScrolledEventArgs.ScrollX` -Eigenschaft und die-Eigenschaft `ScrolledEventArgs.ScrollY` können aufgrund des Sprung Effekts, der beim Scrollen zurück zum Anfang eines auftritt, negative Werte aufweisen [`ScrollView`](xref:Xamarin.Forms.ScrollView) .

Das folgende XAML-Beispiel zeigt [`ScrollView`](xref:Xamarin.Forms.ScrollView) , wie ein Ereignishandler für das-Ereignis festgelegt wird [`Scrolled`](xref:Xamarin.Forms.ScrollView.Scrolled) :

```xaml
<ScrollView Scrolled="OnScrollViewScrolled">
        ...
</ScrollView>
```

Der entsprechende C#-Code lautet:

```csharp
ScrollView scrollView = new ScrollView();
scrollView.Scrolled += OnScrollViewScrolled;
```

In diesem Beispiel wird der `OnScrollViewScrolled` Ereignishandler ausgeführt, wenn das [`Scrolled`](xref:Xamarin.Forms.ScrollView.Scrolled) Ereignis ausgelöst wird:

```csharp
void OnScrollViewScrolled(object sender, ScrolledEventArgs e)
{
    Console.WriteLine($"ScrollX: {e.ScrollX}, ScrollY: {e.ScrollY}");
}
```

In diesem Beispiel gibt der- `OnScrollViewScrolled` Ereignishandler die Werte des- [`ScrolledEventArgs`](xref:Xamarin.Forms.ScrolledEventArgs) Objekts aus, das das-Ereignis begleitet.

> [!NOTE]
> Das [`Scrolled`](xref:Xamarin.Forms.ScrollView.Scrolled) Ereignis wird für benutzergesteuerte scrollläufe und für programmatische scrollläufe ausgelöst.

## <a name="scroll-programmatically"></a>Programm gesteuertes Scrollen

[`ScrollView`](xref:Xamarin.Forms.ScrollView)definiert zwei [`ScrollToAsync`](xref:Xamarin.Forms.ScrollView.ScrollToAsync*) Methoden, die asynchron einen Bildlauf durchführen `ScrollView` . Eine der-über Ladungen führt einen Bildlauf zu einer angegebenen Position im aus `ScrollView` , während die andere ein angegebenes Element in die Ansicht verschiebt. Beide über Ladungen verfügen über ein zusätzliches Argument, das verwendet werden kann, um anzugeben, ob der Bild Lauf animiert werden soll.

> [!IMPORTANT]
> Die [`ScrollToAsync`](xref:Xamarin.Forms.ScrollView.ScrollToAsync*) Methoden führen nicht zu einem Bildlauf, wenn die- [`ScrollView.Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) Eigenschaft auf festgelegt ist `Neither` .

### <a name="scroll-a-position-into-view"></a>Bildlauf zu einer Position in der Ansicht

Eine Position in einem [`ScrollView`](xref:Xamarin.Forms.ScrollView) kann mit der [`ScrollToAsync`](xref:Xamarin.Forms.ScrollView.ScrollToAsync*) -Methode, die `double` `x` -und- `y` Argumente akzeptiert `ScrollView`Im folgenden Beispiel wird ein vertikales Objekt mit dem Namen veranschaulicht `scrollView` , wie Sie von oben nach zu 150 geräteunabhängigen Einheiten Scrollen `ScrollView` :

```csharp
await scrollView.ScrollToAsync(0, 150, true);
```

Das dritte Argument für [`ScrollToAsync`](xref:Xamarin.Forms.ScrollView.ScrollToAsync*) ist das- `animated` Argument, das bestimmt, ob eine scrollanimation angezeigt wird, wenn ein Programm gesteuerter Bildlauf durchführt [`ScrollView`](xref:Xamarin.Forms.ScrollView) .

### <a name="scroll-an-element-into-view"></a>Scrollen eines Elements in eine Ansicht

Ein Element in einem [`ScrollView`](xref:Xamarin.Forms.ScrollView) kann mit der [`ScrollToAsync`](xref:Xamarin.Forms.ScrollView.ScrollToAsync*) -Methode, die [`Element`](xref:Xamarin.Forms.Element) -und-Argumente annimmt, in die Ansicht gescrollt werden [`ScrollToPosition`](xref:Xamarin.Forms.ScrollToPosition) . `ScrollView` `scrollView` [`Label`](xref:Xamarin.Forms.Label) `label` Im folgenden Beispiel wird gezeigt, wie ein Element mit dem Namen und ein mit dem Namen dargestellt wird:

```csharp
await scrollView.ScrollToAsync(label, ScrollToPosition.End, true);
```

Das dritte Argument für [`ScrollToAsync`](xref:Xamarin.Forms.ScrollView.ScrollToAsync*) ist das- `animated` Argument, das bestimmt, ob eine scrollanimation angezeigt wird, wenn ein Programm gesteuerter Bildlauf durchführt [`ScrollView`](xref:Xamarin.Forms.ScrollView) .

Beim Scrollen eines Elements in die Ansicht kann die genaue Position des Elements nach Abschluss des Bildlaufs mit dem zweiten Argument der Methode festgelegt werden `position` [`ScrollToAsync`](xref:Xamarin.Forms.ScrollView.ScrollToAsync*) . Dieses Argument akzeptiert einen- [`ScrollToPosition`](xref:Xamarin.Forms.ScrollToPosition) Enumerationsmember:

- `MakeVisible`Gibt an, dass ein Rollup des-Elements ausgeführt werden soll, bis es in der sichtbar ist `ScrollView` .
- `Start`Gibt an, dass für das Element ein Rollup zum Anfang der ausgeführt werden soll `ScrollView` .
- `Center`Gibt an, dass das Element in den Mittelpunkt des-Elements gescrollt werden soll `ScrollView` .
- `End`Gibt an, dass das Element an das Ende der gescrollt werden soll `ScrollView` .

## <a name="scroll-bar-visibility"></a>Sichtbarkeit der Scrollleiste

[`ScrollView`](xref:Xamarin.Forms.ScrollView)definiert [`HorizontalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView) [`VerticalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView) die Eigenschaften und, die durch bindbare Eigenschaften unterstützt werden. Diese Eigenschaften erhalten einen- [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.HorizontalScrollBarVisibility) Enumerationswert, der angibt, ob die horizontale oder vertikale Bild Lauf Leiste sichtbar ist, oder legt diesen fest. Die `ScrollBarVisibility`-Enumeration definiert die folgenden Member:

- `Default`Gibt das Standardverhalten der Bild Lauf Leiste für die Plattform an, und ist der Standardwert der `HorizontalScrollBarVisibility` `VerticalScrollBarVisibility` Eigenschaften und.
- `Always`Gibt an, dass Scrollleisten sichtbar sind, auch wenn der Inhalt in die Ansicht passt.
- `Never`Gibt an, dass Scrollleisten nicht sichtbar sind, auch wenn der Inhalt nicht in die Ansicht passt.

## <a name="related-links"></a>Verwandte Links

- [ScrollView-Demos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-scrollviewdemos)
- [Xamarin.FormsStackLayout](stacklayout.md)
- [Bindbare Layouts inXamarin.Forms](bindable-layouts.md)
