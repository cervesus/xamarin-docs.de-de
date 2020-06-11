---
Title: " Xamarin.Forms swidanview" Description: " Xamarin.Forms swipeer View ist ein Container Steuerelement, das ein Inhalts Element umschließt und Kontextmenü Elemente bereitstellt, die durch eine Schwenkbewegung angezeigt werden."
ms. Prod: xamarin ms. assetid: 602456b5-701b-4948-B454-B1F31283F1CF ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 03/26/2020 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-swipeview"></a>Xamarin.FormsSwidansicht

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-swipeviewdemos/)

Der `SwipeView` ist ein Container Steuerelement, das ein Element des Inhalts umschließt und Kontextmenü Elemente bereitstellt, die durch eine Schwenkbewegung angezeigt werden:

[![Screenshot der swipeer View-Elemente in einer CollectionView unter IOS und Android](swipeview-images/swipeview-collectionview.png "Swipinview-Streifen Elemente")](swipeview-images/swipeview-collectionview-large.png#lightbox "Swipinview-Streifen Elemente")

`SwipeView`ist in Xamarin.Forms 4,4 verfügbar. Es ist jedoch zurzeit experimentell und kann nur verwendet werden, indem der `AppDelegate` Klasse in Ios, ihrer `MainActivity` Klasse unter Android oder ihrer Klasse auf der UWP die folgende Codezeile hinzugefügt wird, `App` bevor aufgerufen wird `Forms.Init` :

```csharp
Forms.SetFlags("SwipeView_Experimental");
```

`SwipeView` definiert die folgenden Eigenschaften:

- `LeftItems`vom Typ `SwipeItems` , der die schwingenden Elemente darstellt, die aufgerufen werden können, wenn das Steuerelement von der linken Seite entfernt wird.
- `RightItems`vom Typ `SwipeItems` , der die schwingenden Elemente darstellt, die aufgerufen werden können, wenn das Steuerelement von der rechten Seite entfernt wird.
- `TopItems`vom Typ `SwipeItems` , der die schwingenden Elemente darstellt, die aufgerufen werden können, wenn das Steuerelement von oben nach unten entfernt wird.
- `BottomItems`vom Typ `SwipeItems` , der die schwingenden Elemente darstellt, die aufgerufen werden können, wenn das Steuerelement von unten nach oben geleitet wird.

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Außerdem `SwipeView` erbt die- [`Content`](xref:Xamarin.Forms.ContentView.Content) Eigenschaft von der- [`ContentView`](xref:Xamarin.Forms.ContentView) Klasse. Die `Content` -Eigenschaft ist die Content-Eigenschaft der `SwipeView` -Klasse und muss daher nicht explizit festgelegt werden.

Die- `SwipeView` Klasse definiert außerdem vier Ereignisse:

- `SwipeStarted`wird ausgelöst, wenn eine Schwenkbewegung gestartet wird. Das- `SwipeStartedEventArgs` Objekt, das dieses Ereignis begleitet, verfügt über eine- `SwipeDirection` Eigenschaft vom Typ `SwipeDirection` .
- `SwipeChanging`wird ausgelöst, wenn der Schwenk bewegt wird. Das `SwipeChangingEventArgs` -Objekt, das dieses Ereignis begleitet, verfügt über eine `SwipeDirection` -Eigenschaft vom Typ `SwipeDirection` und eine- `Offset` Eigenschaft vom Typ `double` .
- `SwipeEnded`wird ausgelöst, wenn eine flüpe endet. Das- `SwipeEndedEventArgs` Objekt, das dieses Ereignis begleitet, verfügt über eine- `SwipeDirection` Eigenschaft vom Typ `SwipeDirection` .
- `CloseRequested`wird ausgelöst, wenn die flüften Elemente geschlossen werden.

Darüber hinaus `SwipeView` schließt `Open` die-Methode und die- `Close` Methode ein, die die Streifen Elemente Programm gesteuert öffnen und schließen.

> [!NOTE]
> `SwipeView`verfügt über eine plattformspezifische plattformspezifische unter IOS und Android, die den Übergang steuert, der beim Öffnen eines verwendet wird `SwipeView` . Weitere Informationen finden Sie unter [swipeer View Swipe Transition Mode on IOS](~/xamarin-forms/platform/ios/swipeview-swipetransitionmode.md) and [swipeer View Swipe Transition Mode on Android](~/xamarin-forms/platform/android/swipeview-swipetransitionmode.md).

## <a name="create-a-swipeview"></a>Erstellen einer swipeer View

Ein `SwipeView` muss den Inhalt definieren, der `SwipeView` von umschlossen wird, und die von der Schwenkbewegung offenbarten Elemente. Die wischen-Elemente sind ein oder mehrere `SwipeItem` Objekte, die in einer der vier `SwipeView` direktionalen Auflistungen,,, oder platziert werden `LeftItems` `RightItems` `TopItems` `BottomItems` .

Im folgenden Beispiel wird gezeigt, wie ein `SwipeView` in XAML instanziiert wird:

```xaml
<SwipeView>
    <SwipeView.LeftItems>
        <SwipeItems>
            <SwipeItem Text="Favorite"
                       IconImageSource="favorite.png"
                       BackgroundColor="LightGreen"
                       Invoked="OnFavoriteSwipeItemInvoked" />
            <SwipeItem Text="Delete"
                       IconImageSource="delete.png"
                       BackgroundColor="LightPink"
                       Invoked="OnDeleteSwipeItemInvoked" />
        </SwipeItems>
    </SwipeView.LeftItems>
    <!-- Content -->
    <Grid HeightRequest="60"
          WidthRequest="300"
          BackgroundColor="LightGray">
        <Label Text="Swipe right"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
    </Grid>
</SwipeView>
```

Der entsprechende C#-Code lautet:

```csharp
// SwipeItems
SwipeItem favoriteSwipeItem = new SwipeItem
{
    Text = "Favorite",
    IconImageSource = "favorite.png",
    BackgroundColor = Color.LightGreen
};
favoriteSwipeItem.Invoked += OnFavoriteSwipeItemInvoked;

SwipeItem deleteSwipeItem = new SwipeItem
{
    Text = "Delete",
    IconImageSource = "delete.png",
    BackgroundColor = Color.LightPink
};
deleteSwipeItem.Invoked += OnDeleteSwipeItemInvoked;

List<SwipeItem> swipeItems = new List<SwipeItem>() { favoriteSwipeItem, deleteSwipeItem };

// SwipeView content
Grid grid = new Grid
{
    HeightRequest = 60,
    WidthRequest = 300,
    BackgroundColor = Color.LightGray
};
grid.Children.Add(new Label
{
    Text = "Swipe right",
    HorizontalOptions = LayoutOptions.Center,
    VerticalOptions = LayoutOptions.Center
});

SwipeView swipeView = new SwipeView
{
    LeftItems = new SwipeItems(swipeItems),
    Content = grid
};
```

In diesem Beispiel ist der `SwipeView` Inhalt eine [`Grid`](xref:Xamarin.Forms.Grid) , die eine enthält [`Label`](xref:Xamarin.Forms.Label) :

[![Screenshot von swidanview-Inhalten unter IOS und Android](swipeview-images/swipeview-content.png "Inhalt von swidansicht")](swipeview-images/swipeview-content-large.png#lightbox "Inhalt von swidansicht")

Die wischen-Elemente werden verwendet, um Aktionen für den Inhalt auszuführen. Sie `SwipeView` werden angezeigt, wenn das Steuerelement von der linken Seite entfernt wird:

[![Screenshot der swidanview-Streifen Elemente unter IOS und Android](swipeview-images/swipeview-swipeitems.png "Swipinview-Streifen Elemente")](swipeview-images/swipeview-swipeitems-large.png#lightbox "Swipinview-Streifen Elemente")

Standardmäßig wird ein Schwenk Element ausgeführt, wenn es vom Benutzer getippt wird. Dieses Verhalten kann jedoch geändert werden. Weitere Informationen finden Sie unter [Swipe-Modus](#swipe-mode).

Nachdem ein Schwenk Element ausgeführt wurde, werden die Schwenk Elemente ausgeblendet, und der `SwipeView` Inhalt wird erneut angezeigt. Dieses Verhalten kann jedoch geändert werden. Weitere Informationen finden Sie unter [Swipe-Verhalten](#swipe-behavior).

> [!NOTE]
> Streifen Inhalt und Streifen Elemente können Inline eingefügt oder als Ressourcen definiert werden.

## <a name="swipe-items"></a>Elemente schwenken

Die `LeftItems` `RightItems` -, `TopItems` -,-und-Auflistungen `BottomItems` sind vom Typ `SwipeItems` . Die- `SwipeItems` Klasse definiert die folgenden Eigenschaften:

- `Mode`vom Typ `SwipeMode` , der die Auswirkung einer Schwenk Interaktion angibt. Weitere Informationen zum wischen-Modus finden Sie unter [wischen-Modus](#swipe-mode).
- `SwipeBehaviorOnInvoked`vom Typ `SwipeBehaviorOnInvoked` , der angibt, wie sich ein `SwipeView` verhält, nachdem ein wischen-Element aufgerufen wurde. Weitere Informationen zum wischen-Verhalten finden Sie unter [wischen-Verhalten](#swipe-behavior).

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Jedes "Wischen"-Element wird als `SwipeItem` -Objekt definiert, das in einer der vier direktionalen Auflistungen platziert wird `SwipeItems` . Die `SwipeItem` -Klasse wird von der [`MenuItem`](xref:Xamarin.Forms.MenuItem) -Klasse abgeleitet und fügt die folgenden Member hinzu:

- Eine `BackgroundColor` Eigenschaft vom Typ `Color` , die die Hintergrundfarbe für das Schwenk Element definiert. Diese Eigenschaft wird durch eine bindbare Eigenschaft unterstützt.
- Ein `Invoked` Ereignis, das ausgelöst wird, wenn das wischen-Element ausgeführt wird.

> [!IMPORTANT]
> Die- [`MenuItem`](xref:Xamarin.Forms.MenuItem) Klasse definiert mehrere Eigenschaften, einschließlich `Command` , `CommandParameter` , `IconImageSource` und `Text` . Diese Eigenschaften können für ein-Objekt festgelegt werden `SwipeItem` , um die Darstellung zu definieren, und um einen zu definieren, `ICommand` der ausgeführt wird, wenn das wischen-Element aufgerufen wird. Weitere Informationen finden Sie unter [ Xamarin.Forms MenuItem](~/xamarin-forms/user-interface/menuitem.md).

Im folgenden Beispiel werden zwei- `SwipeItem` Objekte in der-Auflistung `LeftItems` einer angezeigt `SwipeView` :

```xaml
<SwipeView>
    <SwipeView.LeftItems>
        <SwipeItems>
            <SwipeItem Text="Favorite"
                       IconImageSource="favorite.png"
                       BackgroundColor="LightGreen"
                       Invoked="OnFavoriteSwipeItemInvoked" />
            <SwipeItem Text="Delete"
                       IconImageSource="delete.png"
                       BackgroundColor="LightPink"
                       Invoked="OnDeleteSwipeItemInvoked" />
        </SwipeItems>
    </SwipeView.LeftItems>
    <!-- Content -->
</SwipeView>
```

Die Darstellung der einzelnen `SwipeItem` wird durch eine Kombination der `Text` Eigenschaften, `IconImageSource` und definiert `BackgroundColor` :

[![Screenshot der swidanview-Streifen Elemente unter IOS und Android](swipeview-images/swipeview-swipeitems.png "Swipinview-Streifen Elemente")](swipeview-images/swipeview-swipeitems-large.png#lightbox "Swipinview-Streifen Elemente")

Wenn ein `SwipeItem` `Invoked` -Ereignis abgetippt wird, wird das zugehörige-Ereignis ausgelöst und vom registrierten Ereignishandler behandelt. Alternativ kann die- `Command` Eigenschaft auf eine-Implementierung festgelegt werden `ICommand` , die ausgeführt wird, wenn `SwipeItem` aufgerufen wird.

> [!NOTE]
> Wenn die Darstellung eines `SwipeItem` nur mit der-Eigenschaft oder der-Eigenschaft definiert wird `Text` `IconImageSource` , wird der Inhalt immer zentriert.

Neben der Definition von Schwenk Elementen als `SwipeItem` Objekte ist es auch möglich, benutzerdefinierte Streifen Element Sichten zu definieren. Weitere Informationen finden Sie unter [benutzerdefinierte wischen-Elemente](#custom-swipe-items).

## <a name="swipe-direction"></a>Richtung schwenken

`SwipeView`unterstützt vier verschiedene Richtung, bei der die Richtung von der direktionalen Auflistung definiert wird, `SwipeItems` zu der die `SwipeItem` Objekte hinzugefügt werden. Jede Richtung kann eigene Streifen Elemente enthalten. Das folgende Beispiel zeigt z. b. ein-Element, `SwipeView` dessen Streifen Elemente von der Schwenk Richtung abhängen:

```xaml
<SwipeView>
    <SwipeView.LeftItems>
        <SwipeItems>
            <SwipeItem Text="Delete"
                       IconImageSource="delete.png"
                       BackgroundColor="LightPink"
                       Command="{Binding DeleteCommand}" />
        </SwipeItems>
    </SwipeView.LeftItems>
    <SwipeView.RightItems>
        <SwipeItems>
            <SwipeItem Text="Favorite"
                       IconImageSource="favorite.png"
                       BackgroundColor="LightGreen"
                       Command="{Binding FavoriteCommand}" />
            <SwipeItem Text="Share"
                       IconImageSource="share.png"
                       BackgroundColor="LightYellow"
                       Command="{Binding ShareCommand}" />
        </SwipeItems>
    </SwipeView.RightItems>
    <!-- Content -->
</SwipeView>
```

In diesem Beispiel kann der `SwipeView` Inhalt nach rechts oder Links geleitet werden. Wenn Sie nach rechts wischen, wird das **Lösch** Element angezeigt, während beim Schwenken auf der linken Seite die **Favoriten** -und **Freigabe** Elemente angezeigt werden.

> [!WARNING]
> Auf einem kann jeweils nur eine Instanz einer direktionalen Auflistung `SwipeItems` festgelegt werden `SwipeView` . Daher können Sie nicht über zwei `LeftItems` Definitionen für einen verfügen `SwipeView` .

Das `SwipeStarted` `SwipeChanging` -Ereignis, das-Ereignis und das- `SwipeEnded` Ereignis melden die Schwenk Richtung über die- `SwipeDirection` Eigenschaft in den Ereignis Argumenten. Diese Eigenschaft `SwipeDirection` hat den Typ, bei dem es sich um eine Enumeration handelt, die aus vier Membern besteht:

- `Right`Gibt an, dass eine Rechte Schwenkbewegung aufgetreten ist.
- `Left`Gibt an, dass eine linke Schwenkbewegung aufgetreten ist.
- `Up`Gibt an, dass ein aufwärts schwenken aufgetreten ist.
- `Down`Gibt an, dass eine Abwärtsbewegung aufgetreten ist.

## <a name="swipe-mode"></a>Schwenk Modus

Die- `SwipeItems` Klasse verfügt über eine- `Mode` Eigenschaft, die den Effekt einer wischen-Interaktion angibt. Diese Eigenschaft sollte auf einen der Enumerationsmember festgelegt werden `SwipeMode` :

- `Reveal`Gibt an, dass eine schwenkbare Elemente angezeigt wird. Dies ist der Standardwert der `SwipeItems.Mode`-Eigenschaft.
- `Execute`Gibt an, dass ein wischen die Schwenk Elemente ausführt.

Im Modus "anzeigen" wird ein vom Benutzer `SwipeView` zum Öffnen eines Menüs, das aus einem oder mehreren wischen Elementen besteht, zum Öffnen eines Menüs, das aus einem oder mehreren wischen Elementen hervorgeht, und für die Ausführung explizit tippen. Nachdem das Schwenk Element ausgeführt wurde, werden die Schwenk Elemente geschlossen, und der `SwipeView` Inhalt wird erneut angezeigt. Im Ausführungs Modus wird ein durch den Benutzer ein `SwipeView` , um ein Menü zu öffnen, das aus einem der weiteren Streifen Elemente besteht, die dann automatisch ausgeführt werden. Nach der Ausführung werden die Elemente geschlossen, und der `SwipeView` Inhalt wird erneut angezeigt.

Das folgende Beispiel zeigt einen `SwipeView` , der für die Verwendung des Ausführungs Modus konfiguriert ist:

```xaml
<SwipeView>
    <SwipeView.LeftItems>
        <SwipeItems Mode="Execute">
            <SwipeItem Text="Delete"
                       IconImageSource="delete.png"
                       BackgroundColor="LightPink"
                       Command="{Binding DeleteCommand}" />
        </SwipeItems>
    </SwipeView.LeftItems>
    <!-- Content -->
</SwipeView>
```

In diesem Beispiel kann der `SwipeView` Inhalt direkt weitergeleitet werden, um das Schwenk Element anzuzeigen, das sofort ausgeführt wird. Nach der Ausführung `SwipeView` wird der Inhalt erneut angezeigt.

## <a name="swipe-behavior"></a>Swipe-Verhalten

Die- `SwipeItems` Klasse verfügt über eine- `SwipeBehaviorOnInvoked` Eigenschaft, die angibt, wie sich ein `SwipeView` verhält, nachdem ein wischen-Element aufgerufen wurde. Diese Eigenschaft sollte auf einen der Enumerationsmember festgelegt werden `SwipeBehaviorOnInvoked` :

- `Auto`Gibt an, dass im `SwipeView` Ausrichtungsmodus die geschlossen wird, nachdem ein Schwenk Element aufgerufen wurde, und im Ausführungs Modus `SwipeView` bleibt geöffnet, nachdem ein Schwenk Element aufgerufen wurde. Dies ist der Standardwert der `SwipeItems.SwipeBehaviorOnInvoked`-Eigenschaft.
- `Close`Gibt an, dass `SwipeView` geschlossen wird, nachdem ein Schwenk Element aufgerufen wurde.
- `RemainOpen`Gibt an, dass `SwipeView` geöffnet bleibt, nachdem ein Element aufgerufen wurde.

Das folgende Beispiel zeigt einen `SwipeView` , der so konfiguriert ist, dass er geöffnet bleibt, nachdem ein Schwenk Element aufgerufen wurde:

```xaml
<SwipeView>
    <SwipeView.LeftItems>
        <SwipeItems SwipeBehaviorOnInvoked="RemainOpen">
            <SwipeItem Text="Favorite"
                       IconImageSource="favorite.png"
                       BackgroundColor="LightGreen"
                       Invoked="OnFavoriteSwipeItemInvoked" />
            <SwipeItem Text="Delete"
                       IconImageSource="delete.png"
                       BackgroundColor="LightPink"
                       Invoked="OnDeleteSwipeItemInvoked" />
        </SwipeItems>
    </SwipeView.LeftItems>
    <!-- Content -->
</SwipeView>
```

## <a name="custom-swipe-items"></a>Benutzerdefinierte Streifen Elemente

Benutzerdefinierte wischen-Elemente können mit dem-Typ definiert werden `SwipeItemView` . Die `SwipeItemView` -Klasse wird von der [`ContentView`](xref:Xamarin.Forms.ContentView) -Klasse abgeleitet und fügt die folgenden Eigenschaften hinzu:

- `Command`vom Typ `ICommand` , der ausgeführt wird, wenn ein Schwenk Element getippt wird.
- `CommandParameter` vom Typ `object`: der Parameter, der an `Command` übergeben wird.

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Die- `SwipeItemView` Klasse definiert auch ein `Invoked` Ereignis, das ausgelöst wird, wenn das Element nach dem Ausführen von abgetippt wird `Command` .

Das folgende Beispiel zeigt ein- `SwipeItemView` Objekt in der-Auflistung `LeftItems` einer `SwipeView` :

```xaml
<SwipeView>
    <SwipeView.LeftItems>
        <SwipeItems>
            <SwipeItemView Command="{Binding CheckAnswerCommand}"
                           CommandParameter="{Binding Source={x:Reference resultEntry}, Path=Text}">
                <StackLayout Margin="10"
                             WidthRequest="300">
                    <Entry x:Name="resultEntry"
                           Placeholder="Enter answer"
                           HorizontalOptions="CenterAndExpand" />
                    <Label Text="Check"
                           FontAttributes="Bold"
                           HorizontalOptions="Center" />
                </StackLayout>
            </SwipeItemView>
        </SwipeItems>
    </SwipeView.LeftItems>
    <!-- Content -->
</SwipeView>
```

In diesem Beispiel `SwipeItemView` besteht aus einem [`StackLayout`](xref:Xamarin.Forms.StackLayout) mit einem [`Entry`](xref:Xamarin.Forms.Entry) und einem [`Label`](xref:Xamarin.Forms.Label) . Nachdem der Benutzereingaben in eingegeben `Entry` hat, kann der Rest des- `SwipeViewItem` Objekts abgetippt werden, das die `ICommand` durch die-Eigenschaft definierte ausführt `SwipeItemView.Command` .

## <a name="open-and-close-a-swipeview-programmatically"></a>Programmgesteuertes Öffnen und Schließen von SwipeView

`SwipeView`schließt `Open` die-Methode und die- `Close` Methode ein, die die-Streifen Elemente Programm gesteuert öffnen und schließen.

Die- `Open` Methode erfordert ein- `OpenSwipeItem` Argument, um die Richtung anzugeben, `SwipeView` von der aus geöffnet wird. Die- `OpenSwipeItem` Enumeration hat vier Member:

- `LeftItems`Gibt an, dass das-Element `SwipeView` von Links aus geöffnet wird, um die wischen-Elemente in der Auflistung anzuzeigen `LeftItems` .
- `TopItems`Gibt an, dass das-Element `SwipeView` von oben aus geöffnet wird, um die Schwenk Elemente in der Auflistung anzuzeigen `TopItems` .
- `RightItems`Gibt an, dass das-Element `SwipeView` von der rechten Seite aus geöffnet wird, um die wischen-Elemente in der Auflistung anzuzeigen `RightItems` .
- `BottomItems`Gibt an, dass das-Element `SwipeView` von der unteren Seite aus geöffnet wird, um die Schwenk Elemente in der Auflistung anzuzeigen `BottomItems` .

`SwipeView` `swipeView` Im folgenden Beispiel wird gezeigt, wie ein geöffnet wird, `SwipeView` um die wischen-Elemente in der Auflistung anzuzeigen `LeftItems` :

```csharp
swipeView.Open(OpenSwipeItem.LeftItems);
```

Der `swipeView` kann dann mit der-Methode geschlossen werden `Close` :

```csharp
swipeView.Close();
```

> [!NOTE]
> Wenn die- `Close` Methode aufgerufen wird, `CloseRequested` wird das-Ereignis ausgelöst.

## <a name="disable-a-swipeview"></a>Deaktivieren von swipeer View

Eine Anwendung kann einen Status eingeben, bei dem das Schwenken eines Inhalts Elements kein gültiger Vorgang ist. In solchen Fällen kann der deaktiviert werden, indem die zugehörige- `SwipeView` Eigenschaft auf festgelegt wird `IsEnabled` `false` . Dadurch wird verhindert, dass Benutzerinhalte schwenken, um Streifen Elemente anzuzeigen.

Außerdem kann beim Definieren der- `Command` Eigenschaft eines oder des-Delegaten `SwipeItem` `SwipeItemView` `CanExecute` `ICommand` angegeben werden, um das wischen-Element zu aktivieren oder zu deaktivieren.

## <a name="related-links"></a>Verwandte Links

- [Swidanview (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-swipeviewdemos/)
- [Xamarin.FormsMENUITEM](~/xamarin-forms/user-interface/menuitem.md)
