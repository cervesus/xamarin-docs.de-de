---
title: Xamarin.FormsCarouselview-Daten
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1081bfafae8e4d7a7a522414e9b45cde48037f1d
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136551"
---
# <a name="xamarinforms-carouselview-data"></a>Xamarin.FormsCarouselview-Daten

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView)enthält die folgenden Eigenschaften, die die anzuzeigenden Daten und ihre Darstellung definieren:

- [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)`IEnumerable`gibt die Auflistung der anzuzeigenden Elemente an und hat den Standardwert `null` .
- [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)gibt die Vorlage an, die auf jedes Element in der Auflistung der anzuzeigenden Elemente angewendet werden soll.

Diese Eigenschaften werden von- [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Objekten unterstützt. Dies bedeutet, dass die Eigenschaften Ziele von Daten Bindungen sein können.

> [!NOTE]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView)definiert eine `ItemsUpdatingScrollMode` Eigenschaft, die das Scrollverhalten von darstellt, `CarouselView` Wenn der neue Elemente hinzugefügt werden. Weitere Informationen zu dieser Eigenschaft finden Sie unter [Steuern der Scrollposition, wenn neue Elemente hinzugefügt werden](scrolling.md#control-scroll-position-when-new-items-are-added).

[`CarouselView`](xref:Xamarin.Forms.CarouselView)unterstützt inkrementelle Datenvirtualisierung beim Scrollen des Benutzers Weitere Informationen finden Sie unter [inkrementelles Laden von Daten](#load-data-incrementally).

## <a name="populate-a-carouselview-with-data"></a>Auffüllen einer "carouselview" mit Daten

Ein [`CarouselView`](xref:Xamarin.Forms.CarouselView) wird mit Daten aufgefüllt, indem seine- [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) Eigenschaft auf eine beliebige Auflistung festgelegt wird, die implementiert `IEnumerable` . Elemente können in XAML hinzugefügt werden, indem die-Eigenschaft aus einem Zeichen folgen Array initialisiert wird `ItemsSource` :

```xaml
<CarouselView>
    <CarouselView.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>Baboon</x:String>
            <x:String>Capuchin Monkey</x:String>
            <x:String>Blue Monkey</x:String>
            <x:String>Squirrel Monkey</x:String>
            <x:String>Golden Lion Tamarin</x:String>
            <x:String>Howler Monkey</x:String>
            <x:String>Japanese Macaque</x:String>
        </x:Array>
    </CarouselView.ItemsSource>
</CarouselView>
```

> [!NOTE]
> Bedenken Sie, dass Sie für das Element `x:Array` ein `Type`-Attribute benötigen, das die Elementtypen im Array angibt.

Der entsprechende C#-Code lautet:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.ItemsSource = new string[]
{
    "Baboon",
    "Capuchin Monkey",
    "Blue Monkey",
    "Squirrel Monkey",
    "Golden Lion Tamarin",
    "Howler Monkey",
    "Japanese Macaque"
};
```

> [!IMPORTANT]
> Wenn die beim [`CarouselView`](xref:Xamarin.Forms.CarouselView) hinzufügen, entfernen oder Ändern von Elementen in der zugrunde liegenden Auflistung aktualisiert werden muss, sollte die zugrunde liegende Auflistung eine-Auflistung sein, die `IEnumerable` Benachrichtigungen über Eigenschafts Änderungen sendet, z `ObservableCollection` . b..

Standardmäßig [`CarouselView`](xref:Xamarin.Forms.CarouselView) zeigt Elemente horizontal an. Die folgenden Screenshots zeigen, wie `CarouselView` unterschiedliche Zeichen folgen Elemente unter IOS und Android angezeigt werden:

[![Screenshot von carouselview mit Textelementen unter IOS und Android](populate-data-images/text.png "Text Elemente in einer carouselview")](populate-data-images/text-large.png#lightbox "Text Elemente in einer carouselview")

Informationen zum Ändern der Ausrichtung finden Sie unter " [`CarouselView`](xref:Xamarin.Forms.CarouselView) [ Xamarin.Forms carouselview Layout](layout.md)". Informationen dazu, wie die Darstellung der einzelnen Elemente in der definiert `CarouselView` wird, finden Sie unter [Definieren der Element](#define-item-appearance)Darstellung.

### <a name="data-binding"></a>Datenbindung

[`CarouselView`](xref:Xamarin.Forms.CarouselView)kann mit Daten aufgefüllt werden, indem die-Eigenschaft mithilfe der Datenbindung an eine-Auflistung gebunden wird [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) `IEnumerable` . In XAML wird dies mit der `Binding` Markup Erweiterung erreicht:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}" />
```

Der entsprechende C#-Code lautet:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

In diesem Beispiel werden die [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) Eigenschaften Daten an die- `Monkeys` Eigenschaft des verbundenen ViewModel gebunden.

> [!NOTE]
> Kompilierte Bindungen können aktiviert werden, um die Daten bindungsleistung in-Anwendungen zu verbessern Xamarin.Forms . Weitere Informationen finden Sie unter [Kompilierte Bindungen](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md).

Weitere Informationen zur Datenbindung finden Sie unter [ Xamarin.Forms Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md).

## <a name="define-item-appearance"></a>Element Darstellung definieren

Die Darstellung der einzelnen Elemente in der [`CarouselView`](xref:Xamarin.Forms.CarouselView) kann definiert werden, indem die-Eigenschaft auf festgelegt wird [`CarouselView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) :

```xaml
<CarouselView ItemsSource="{Binding Monkeys}">
    <CarouselView.ItemTemplate>
        <DataTemplate>
            <StackLayout>
                <Frame HasShadow="True"
                       BorderColor="DarkGray"
                       CornerRadius="5"
                       Margin="20"
                       HeightRequest="300"
                       HorizontalOptions="Center"
                       VerticalOptions="CenterAndExpand">
                    <StackLayout>
                        <Label Text="{Binding Name}"
                               FontAttributes="Bold"
                               FontSize="Large"
                               HorizontalOptions="Center"
                               VerticalOptions="Center" />
                        <Image Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="150"
                               WidthRequest="150"
                               HorizontalOptions="Center" />
                        <Label Text="{Binding Location}"
                               HorizontalOptions="Center" />
                        <Label Text="{Binding Details}"
                               FontAttributes="Italic"
                               HorizontalOptions="Center"
                               MaxLines="5"
                               LineBreakMode="TailTruncation" />
                    </StackLayout>
                </Frame>
            </StackLayout>
        </DataTemplate>
    </CarouselView.ItemTemplate>
</CarouselView>
```

Der entsprechende C#-Code lautet:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");

carouselView.ItemTemplate = new DataTemplate(() =>
{
    Label nameLabel = new Label { ... };
    nameLabel.SetBinding(Label.TextProperty, "Name");

    Image image = new Image { ... };
    image.SetBinding(Image.SourceProperty, "ImageUrl");

    Label locationLabel = new Label { ... };
    locationLabel.SetBinding(Label.TextProperty, "Location");

    Label detailsLabel = new Label { ... };
    detailsLabel.SetBinding(Label.TextProperty, "Details");

    StackLayout stackLayout = new StackLayout
    {
        Children = { nameLabel, image, locationLabel, detailsLabel }
    };

    Frame frame = new Frame { ... };
    StackLayout rootStackLayout = new StackLayout
    {
        Children = { frame }
    };

    return rootStackLayout;
});
```

Die in der angegebenen Elemente [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) definieren die Darstellung der einzelnen Elemente in der `CarouselView` . Im Beispiel wird das Layout in `DataTemplate` von einem verwaltet [`StackLayout`](xref:Xamarin.Forms.StackLayout) , und die Daten werden mit einem [`Image`](xref:Xamarin.Forms.Image) -Objekt und drei-Objekten angezeigt, [`Label`](xref:Xamarin.Forms.Label) die alle an die Eigenschaften der-Klasse gebunden sind `Monkey` :

```csharp
public class Monkey
{
    public string Name { get; set; }
    public string Location { get; set; }
    public string Details { get; set; }
    public string ImageUrl { get; set; }
}
```

Die folgenden Screenshots zeigen das Ergebnis der Vorlagen Darstellung der einzelnen Elemente:

[![Screenshot von carouselview, in dem jedes Element auf IOS und Android Vorlagen basiert](populate-data-images/datatemplate.png "Auf Vorlagen basierende Elemente in einer "carouselview"")](populate-data-images/datatemplate-large.png#lightbox "Auf Vorlagen basierende Elemente in einer "carouselview"")

Weitere Informationen zu Datenvorlagen finden Sie unter [ Xamarin.Forms Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="choose-item-appearance-at-runtime"></a>Auswählen der Element Darstellung zur Laufzeit

Die Darstellung der einzelnen Elemente in der [`CarouselView`](xref:Xamarin.Forms.CarouselView) kann zur Laufzeit basierend auf dem Elementwert ausgewählt werden, indem die- [`CarouselView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) Eigenschaft auf ein-Objekt festgelegt wird [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) :

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CarouselViewDemos.Controls"
             x:Class="CarouselViewDemos.Views.HorizontalLayoutDataTemplateSelectorPage">
    <ContentPage.Resources>
        <DataTemplate x:Key="AmericanMonkeyTemplate">
            ...
        </DataTemplate>

        <DataTemplate x:Key="OtherMonkeyTemplate">
            ...
        </DataTemplate>

        <controls:MonkeyDataTemplateSelector x:Key="MonkeySelector"
                                             AmericanMonkey="{StaticResource AmericanMonkeyTemplate}"
                                             OtherMonkey="{StaticResource OtherMonkeyTemplate}" />
    </ContentPage.Resources>

    <CarouselView ItemsSource="{Binding Monkeys}"
                  ItemTemplate="{StaticResource MonkeySelector}" />
</ContentPage>
```

Der entsprechende C#-Code lautet:

```csharp
CarouselView carouselView = new CarouselView
{
    ItemTemplate = new MonkeyDataTemplateSelector { ... }
};
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

Die- [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) Eigenschaft ist auf ein-Objekt festgelegt `MonkeyDataTemplateSelector` . Das folgende Beispiel zeigt die- `MonkeyDataTemplateSelector` Klasse:

```csharp
public class MonkeyDataTemplateSelector : DataTemplateSelector
{
    public DataTemplate AmericanMonkey { get; set; }
    public DataTemplate OtherMonkey { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        return ((Monkey)item).Location.Contains("America") ? AmericanMonkey : OtherMonkey;
    }
}
```

Die `MonkeyDataTemplateSelector` -Klasse definiert die `AmericanMonkey` `OtherMonkey` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) Eigenschaften und, die auf unterschiedliche Datenvorlagen festgelegt sind. Die `OnSelectTemplate` außer Kraft Setzung gibt die Vorlage zurück, `AmericanMonkey` Wenn der affenname "America" enthält. Wenn der affenname nicht "America" enthält, gibt die Überschreibung `OnSelectTemplate` die `OtherMonkey` Vorlage zurück, die die Daten abgeblendet anzeigt:

[![Screenshot der Auswahl von "carouselview Runtime item Template" unter IOS und Android](populate-data-images/datatemplateselector.png "Auswahl der Lauf Zeitelement Vorlage in einer "carouselview"")](populate-data-images/datatemplateselector-large.png#lightbox "Auswahl der Lauf Zeitelement Vorlage in einer "carouselview"")

Weitere Informationen zu Datenvorlagen Selektoren finden Sie unter [Create a Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

> [!IMPORTANT]
> Wenn Sie verwenden [`CarouselView`](xref:Xamarin.Forms.CarouselView) , legen Sie das Stamm Element der [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) Objekte nie auf einen fest `ViewCell` . Dies führt dazu, dass eine Ausnahme ausgelöst wird, da `CarouselView` kein Konzept von Zellen aufweist.

## <a name="display-indicators"></a>Anzeigeindikatoren

Die Indikatoren, die die Anzahl der Elemente und die aktuelle Position in einem darstellen `CarouselView` , können neben dem angezeigt werden `CarouselView` . Dies kann mit dem-Steuerelement erreicht werden `IndicatorView` :

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

In diesem Beispiel wird der unter `IndicatorView` dem gerendert `CarouselView` , mit einem Indikator für jedes Element in der `CarouselView` . Die `IndicatorView` wird mit Daten aufgefüllt, indem die- `CarouselView.IndicatorView` Eigenschaft auf das-Objekt festgelegt wird `IndicatorView` . Jeder Indikator ist ein heller grauer Kreis, während der Indikator, der das aktuelle Element in der darstellt, `CarouselView` dunkelgrau ist:

[![Screenshot von "carouselview" und "indikatorview" unter IOS und Android](populate-data-images/indicators.png "Sichorview-Kreise")](populate-data-images/indicators-large.png#lightbox "Sichorview-Kreise")

> [!IMPORTANT]
> `CarouselView.IndicatorView`Wenn Sie die-Eigenschaft festlegen `IndicatorView.Position` , wird die-Eigenschaft an die-Eigenschaft gebunden `CarouselView.Position` , und die-Eigenschaft wird `IndicatorView.ItemsSource` an die- `CarouselView.ItemsSource` Eigenschaft gebunden.

Weitere Informationen zu Indikatoren finden Sie unter " [ Xamarin.Forms indikatorview](~/xamarin-forms/user-interface/indicatorview.md)".

## <a name="context-menus"></a>Kontextmenüs

[`CarouselView`](xref:Xamarin.Forms.CarouselView)unterstützt Kontextmenüs für Datenelemente über das-Element `SwipeView` , das das Kontextmenü mit einer Schwenkbewegung zeigt. Der `SwipeView` ist ein Container Steuerelement, das ein Element des Inhalts umschließt und Kontextmenü Elemente für das Inhalts Element bereitstellt. Daher werden Kontextmenüs für ein implementiert, `CarouselView` indem ein erstellt wird `SwipeView` , das den Inhalt definiert `SwipeView` , den der umschließt, sowie die Kontextmenü Elemente, die durch die Schwenkbewegung angezeigt werden. Dies wird erreicht, indem ein-Objekt hinzugefügt wird `SwipeView` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , das die Darstellung der einzelnen Datenelemente in der definiert `CarouselView` :

```xaml
<CarouselView x:Name="carouselView"
              ItemsSource="{Binding Monkeys}">
    <CarouselView.ItemTemplate>
        <DataTemplate>
            <StackLayout>
                    <Frame HasShadow="True"
                           BorderColor="DarkGray"
                           CornerRadius="5"
                           Margin="20"
                           HeightRequest="300"
                           HorizontalOptions="Center"
                           VerticalOptions="CenterAndExpand">
                        <SwipeView>
                            <SwipeView.TopItems>
                                <SwipeItems>
                                    <SwipeItem Text="Favorite"
                                               IconImageSource="favorite.png"
                                               BackgroundColor="LightGreen"
                                               Command="{Binding Source={x:Reference carouselView}, Path=BindingContext.FavoriteCommand}"
                                               CommandParameter="{Binding}" />
                                </SwipeItems>
                            </SwipeView.TopItems>
                            <SwipeView.BottomItems>
                                <SwipeItems>
                                    <SwipeItem Text="Delete"
                                               IconImageSource="delete.png"
                                               BackgroundColor="LightPink"
                                               Command="{Binding Source={x:Reference carouselView}, Path=BindingContext.DeleteCommand}"
                                               CommandParameter="{Binding}" />
                                </SwipeItems>
                            </SwipeView.BottomItems>
                            <StackLayout>
                                <!-- Define item appearance -->
                            </StackLayout>
                        </SwipeView>
                    </Frame>
            </StackLayout>
        </DataTemplate>
    </CarouselView.ItemTemplate>
</CarouselView>
```

Der entsprechende C#-Code lautet:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");

carouselView.ItemTemplate = new DataTemplate(() =>
{
    StackLayout stackLayout = new StackLayout();
    Frame frame = new Frame { ... };

    SwipeView swipeView = new SwipeView();
    SwipeItem favoriteSwipeItem = new SwipeItem
    {
        Text = "Favorite",
        IconImageSource = "favorite.png",
        BackgroundColor = Color.LightGreen
    };
    favoriteSwipeItem.SetBinding(MenuItem.CommandProperty, new Binding("BindingContext.FavoriteCommand", source: carouselView));
    favoriteSwipeItem.SetBinding(MenuItem.CommandParameterProperty, ".");

    SwipeItem deleteSwipeItem = new SwipeItem
    {
        Text = "Delete",
        IconImageSource = "delete.png",
        BackgroundColor = Color.LightPink
    };
    deleteSwipeItem.SetBinding(MenuItem.CommandProperty, new Binding("BindingContext.DeleteCommand", source: carouselView));
    deleteSwipeItem.SetBinding(MenuItem.CommandParameterProperty, ".");

    swipeView.TopItems = new SwipeItems { favoriteSwipeItem };
    swipeView.BottomItems = new SwipeItems { deleteSwipeItem };

    StackLayout swipeViewStackLayout = new StackLayout { ... };
    swipeView.Content = swipeViewStackLayout;
    frame.Content = swipeView;
    stackLayout.Children.Add(frame);

    return stackLayout;
});
```

In diesem Beispiel ist der `SwipeView` Inhalt eine [`StackLayout`](xref:Xamarin.Forms.StackLayout) , die die Darstellung der einzelnen Elemente definiert, die von einem [`Frame`](xref:Xamarin.Forms.Frame) in der umgeben sind [`CarouselView`](xref:Xamarin.Forms.CarouselView) . Die wischen-Elemente werden verwendet, um Aktionen für den Inhalt auszuführen. Sie `SwipeView` werden angezeigt, wenn das Steuerelement von oben nach unten und von unten nach oben gezogen wird:

[![Screenshot des Kontextmenü Elements "carouselview" unter IOS und Android](populate-data-images/swipeview-bottom.png "Carouselview mit dem unteren Kontextmenü Element "swidansicht"")](populate-data-images/swipeview-bottom-large.png#lightbox "Carouselview mit dem unteren Kontextmenü Element "swidansicht"") 
 [ ![Screenshot des oberen Menü Elements "carouselview" unter IOS und Android](populate-data-images/swipeview-top.png "Carouselview mit dem oberen Kontextmenü Element "swidansicht"")](populate-data-images/swipeview-top-large.png#lightbox "Carouselview mit dem oberen Kontextmenü Element "swidansicht"")

`SwipeView`unterstützt vier verschiedene Richtung, bei der die Richtung von der direktionalen Auflistung definiert wird, `SwipeItems` zu der die `SwipeItems` Objekte hinzugefügt werden. Standardmäßig wird ein Schwenk Element ausgeführt, wenn es vom Benutzer getippt wird. Außerdem werden nach dem Ausführen eines Schwenk Elements die Schwenk Elemente ausgeblendet, und der `SwipeView` Inhalt wird erneut angezeigt. Diese Verhaltensweisen können jedoch geändert werden.

Weitere Informationen zum-Steuerelement finden Sie unter `SwipeView` [ Xamarin.Forms swipeer View](~/xamarin-forms/user-interface/swipeview.md).

## <a name="pull-to-refresh"></a>Aktualisierung durch Ziehen

[`CarouselView`](xref:Xamarin.Forms.CarouselView)unterstützt die Funktion zum Aktualisieren von Pull-Funktionen über das-Element `RefreshView` , das die Aktualisierung der angezeigten Daten ermöglicht, indem die Elemente abgerufen werden. Bei handelt es sich um ein Container Steuerelement, das dem untergeordneten Pullvorgang die `RefreshView` Aktualisierungs Funktionalität bereitstellt Der Pull-Vorgang zum Aktualisieren wird daher für ein implementiert, `CarouselView` indem er als untergeordnetes Element eines festgelegt wird `RefreshView` :

```xaml
<RefreshView IsRefreshing="{Binding IsRefreshing}"
             Command="{Binding RefreshCommand}">
    <CarouselView ItemsSource="{Binding Animals}">
        ...
    </CarouselView>
</RefreshView>
```

Der entsprechende C#-Code lautet:

```csharp
RefreshView refreshView = new RefreshView();
ICommand refreshCommand = new Command(() =>
{
    // IsRefreshing is true
    // Refresh data here
    refreshView.IsRefreshing = false;
});
refreshView.Command = refreshCommand;

CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
refreshView.Content = carouselView;
// ...
```

Wenn der Benutzer eine Aktualisierung initiiert, `ICommand` wird der durch die- `Command` Eigenschaft definierte ausgeführt, wodurch die angezeigten Elemente aktualisiert werden. Eine Aktualisierungs Visualisierung wird angezeigt, während die Aktualisierung durchgeführt wird, die aus einem animierten Fortschritts Kreis besteht:

[![Screenshot von "carouselview Pull-to-refresh" unter IOS und Android](populate-data-images/pull-to-refresh.png "Carouselview Pull-to-refresh")](populate-data-images/pull-to-refresh-large.png#lightbox "Carouselview Pull-to-refresh")

Der Wert der- `RefreshView.IsRefreshing` Eigenschaft gibt den aktuellen Zustand des an `RefreshView` . Wenn eine Aktualisierung durch den Benutzer ausgelöst wird, wird diese Eigenschaft automatisch zu migrischt `true` . Nachdem die Aktualisierung abgeschlossen ist, sollten Sie die-Eigenschaft auf Zurücksetzen `false` .

Weitere Informationen zu finden Sie in der `RefreshView` [ Xamarin.Forms aktuerfrischenden Ansicht](~/xamarin-forms/user-interface/refreshview.md).

## <a name="load-data-incrementally"></a>Inkrementelles Laden von Daten

[`CarouselView`](xref:Xamarin.Forms.CarouselView)unterstützt inkrementelle Datenvirtualisierung beim Scrollen des Benutzers Dies ermöglicht Szenarios, wie z. b. das asynchrone Laden einer Datenseite aus einem Webdienst, während der Benutzer einen Bildlauf ausführt. Außerdem ist der Punkt, an dem mehr Daten geladen werden, so konfigurierbar, dass Benutzern kein leerer Bereich angezeigt wird oder der Bildlauf angehalten wird.

[`CarouselView`](xref:Xamarin.Forms.CarouselView)definiert die folgenden Eigenschaften zum Steuern des inkrementellen Ladens von Daten:

- `RemainingItemsThreshold`, vom Typ `int` , der Schwellenwert für Elemente, die noch nicht in der Liste sichtbar sind, in der das `RemainingItemsThresholdReached` Ereignis ausgelöst wird.
- `RemainingItemsThresholdReachedCommand`vom Typ `ICommand` , der ausgeführt wird, wenn `RemainingItemsThreshold` erreicht wird.
- `RemainingItemsThresholdReachedCommandParameter` vom Typ `object`: der Parameter, der an `RemainingItemsThresholdReachedCommand` übergeben wird.

[`CarouselView`](xref:Xamarin.Forms.CarouselView)definiert auch ein- `RemainingItemsThresholdReached` Ereignis, das ausgelöst wird, wenn ein `CarouselView` scrollt, das so weit genug ist, dass `RemainingItemsThreshold` Elemente nicht mehr angezeigt werden. Dieses Ereignis kann behandelt werden, um weitere Elemente zu laden. Wenn das `RemainingItemsThresholdReached` Ereignis ausgelöst wird, `RemainingItemsThresholdReachedCommand` wird außerdem ausgeführt, sodass inkrementelles Laden von Daten in einem ViewModel erfolgen kann.

Der Standardwert der `RemainingItemsThreshold` -Eigenschaft ist-1 und gibt an, dass das `RemainingItemsThresholdReached` Ereignis nie ausgelöst wird. Wenn der-Eigenschafts Wert 0 ist, wird das-Ereignis ausgelöst, `RemainingItemsThresholdReached` Wenn das letzte Element in der [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) angezeigt wird. Bei Werten, die größer als 0 sind, wird das-Ereignis ausgelöst, `RemainingItemsThresholdReached` Wenn das die `ItemsSource` Anzahl der Elemente enthält, für die noch kein Rollup durchgeführt wurde.

> [!NOTE]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView)überprüft die `RemainingItemsThreshold` -Eigenschaft, sodass Ihr Wert immer größer oder gleich-1 ist.

Das folgende XAML-Beispiel zeigt einen [`CarouselView`](xref:Xamarin.Forms.CarouselView) , der Daten inkrementell lädt:

```xaml
<CarouselView ItemsSource="{Binding Animals}"
              RemainingItemsThreshold="2"
              RemainingItemsThresholdReached="OnCarouselViewRemainingItemsThresholdReached"
              RemainingItemsThresholdReachedCommand="{Binding LoadMoreDataCommand}">
    ...
</CarouselView>
```

Der entsprechende C#-Code lautet:

```csharp
CarouselView carouselView = new CarouselView
{
    RemainingItemsThreshold = 2
};
carouselView.RemainingItemsThresholdReached += OnCollectionViewRemainingItemsThresholdReached;
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
```

In diesem Codebeispiel wird das-Ereignis ausgelöst, `RemainingItemsThresholdReached` Wenn für 2 Elemente noch kein Rollup durchgeführt wurde und der Ereignishandler in der Antwort ausgeführt wird `OnCollectionViewRemainingItemsThresholdReached` :

```csharp
void OnCollectionViewRemainingItemsThresholdReached(object sender, EventArgs e)
{
    // Retrieve more data here and add it to the CollectionView's ItemsSource collection.
}
```

> [!NOTE]
> Daten können auch inkrementell geladen werden, indem der `RemainingItemsThresholdReachedCommand` an eine `ICommand` Implementierung in ViewModel gebunden wird.

## <a name="related-links"></a>Verwandte Links

- [Carouselview (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
- [Xamarin.FormsIndikator Ansicht](~/xamarin-forms/user-interface/indicatorview.md)
- [Xamarin.FormsAktuerfrischendes Ansicht](~/xamarin-forms/user-interface/refreshview.md)
- [Xamarin.FormsDatenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Xamarin.FormsDatenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Erstellen eines Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
