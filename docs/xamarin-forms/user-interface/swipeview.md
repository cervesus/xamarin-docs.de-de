---
title: Xamarin. Forms swipeer View
description: Bei xamarin. Forms swipeer View handelt es sich um ein Container Steuerelement, das ein Inhalts Element umschließt und Kontextmenü Elemente bereitstellt, die durch eine Schwenkbewegung angezeigt werden.
ms.prod: xamarin
ms.assetId: 602456B5-701B-4948-B454-B1F31283F1CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/26/2020
ms.openlocfilehash: 992e4dd1a2b2a1d1a4f0b76dadf4704241486415
ms.sourcegitcommit: 520ea9d52266f745d2c09642bac21f64a56f8c31
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2020
ms.locfileid: "82859112"
---
# <a name="xamarinforms-swipeview"></a>Xamarin. Forms swipeer View

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-swipeviewdemos/)

Der `SwipeView` ist ein Container Steuerelement, das ein Element des Inhalts umschließt und Kontextmenü Elemente bereitstellt, die durch eine Schwenkbewegung angezeigt werden:

[![Screenshot der swipeer View-Elemente in einer CollectionView unter IOS und Android](swipeview-images/swipeview-collectionview.png "Swipinview-Streifen Elemente")](swipeview-images/swipeview-collectionview-large.png#lightbox "Swipinview-Streifen Elemente")

`SwipeView`ist in xamarin. Forms 4,4 verfügbar. Es ist `AppDelegate` jedoch zurzeit experimentell und kann nur verwendet werden, indem der Klasse in Ios, ihrer `MainActivity` Klasse unter Android oder Ihrer `App` Klasse auf der UWP die folgende Codezeile hinzugefügt wird, bevor aufgerufen `Forms.Init`wird:

```csharp
Forms.SetFlags("SwipeView_Experimental");
```

`SwipeView` definiert die folgenden Eigenschaften:

- `LeftItems`vom Typ `SwipeItems`, der die schwingenden Elemente darstellt, die aufgerufen werden können, wenn das Steuerelement von der linken Seite entfernt wird.
- `RightItems`vom Typ `SwipeItems`, der die schwingenden Elemente darstellt, die aufgerufen werden können, wenn das Steuerelement von der rechten Seite entfernt wird.
- `TopItems`vom Typ `SwipeItems`, der die schwingenden Elemente darstellt, die aufgerufen werden können, wenn das Steuerelement von oben nach unten entfernt wird.
- `BottomItems`vom Typ `SwipeItems`, der die schwingenden Elemente darstellt, die aufgerufen werden können, wenn das Steuerelement von unten nach oben geleitet wird.

Diese Eigenschaften werden von [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Objekten unterstützt. Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Außerdem erbt die `SwipeView` - [`Content`](xref:Xamarin.Forms.ContentView.Content) Eigenschaft von der- [`ContentView`](xref:Xamarin.Forms.ContentView) Klasse. Die `Content` -Eigenschaft ist die Content-Eigenschaft `SwipeView` der-Klasse und muss daher nicht explizit festgelegt werden.

Die `SwipeView` -Klasse definiert außerdem vier Ereignisse:

- `SwipeStarted`wird ausgelöst, wenn eine Schwenkbewegung gestartet wird. Das `SwipeStartedEventArgs` -Objekt, das dieses Ereignis begleitet `SwipeDirection` , verfügt über eine `SwipeDirection`-Eigenschaft vom Typ.
- `SwipeChanging`wird ausgelöst, wenn der Schwenk bewegt wird. Das `SwipeChangingEventArgs` -Objekt, das dieses Ereignis begleitet `SwipeDirection` , verfügt über eine `SwipeDirection`-Eigenschaft vom `Offset` Typ und eine `double`-Eigenschaft vom Typ.
- `SwipeEnded`wird ausgelöst, wenn eine flüpe endet. Das `SwipeEndedEventArgs` -Objekt, das dieses Ereignis begleitet `SwipeDirection` , verfügt über eine `SwipeDirection`-Eigenschaft vom Typ.
- `CloseRequested`wird ausgelöst, wenn die flüften Elemente geschlossen werden.

Darüber hinaus `SwipeView` schließt `Open` die- `Close` Methode und die-Methode ein, die die Streifen Elemente Programm gesteuert öffnen und schließen.

> [!NOTE]
> `SwipeView`verfügt über eine plattformspezifische plattformspezifische unter IOS und Android, die den Übergang steuert, der `SwipeView`beim Öffnen eines verwendet wird. Weitere Informationen finden Sie unter [swipeer View Swipe Transition Mode on IOS](~/xamarin-forms/platform/ios/swipeview-swipetransitionmode.md) and [swipeer View Swipe Transition Mode on Android](~/xamarin-forms/platform/android/swipeview-swipetransitionmode.md).

## <a name="create-a-swipeview"></a>Erstellen einer swipeer View

Ein `SwipeView` muss den Inhalt `SwipeView` definieren, der von umschlossen wird, und die von der Schwenkbewegung offenbarten Elemente. Die wischen-Elemente sind ein oder `SwipeItem` `SwipeView` mehrere Objekte, die in einer der vier direktionalen Auflistungen `LeftItems`, `RightItems` `TopItems`, `BottomItems`, oder platziert werden.

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

In diesem Beispiel ist der `SwipeView` Inhalt eine [`Grid`](xref:Xamarin.Forms.Grid) , die eine [`Label`](xref:Xamarin.Forms.Label)enthält:

[![Screenshot von swidanview-Inhalten unter IOS und Android](swipeview-images/swipeview-content.png "Inhalt von swidansicht")](swipeview-images/swipeview-content-large.png#lightbox "Inhalt von swidansicht")

Die wischen-Elemente werden verwendet, um Aktionen für `SwipeView` den Inhalt auszuführen. Sie werden angezeigt, wenn das Steuerelement von der linken Seite entfernt wird:

[![Screenshot der swidanview-Streifen Elemente unter IOS und Android](swipeview-images/swipeview-swipeitems.png "Swipinview-Streifen Elemente")](swipeview-images/swipeview-swipeitems-large.png#lightbox "Swipinview-Streifen Elemente")

Standardmäßig wird ein Schwenk Element ausgeführt, wenn es vom Benutzer getippt wird. Dieses Verhalten kann jedoch geändert werden. Weitere Informationen finden Sie unter [Swipe-Modus](#swipe-mode).

Nachdem ein Schwenk Element ausgeführt wurde, werden die Schwenk Elemente ausgeblendet, und der `SwipeView` Inhalt wird erneut angezeigt. Dieses Verhalten kann jedoch geändert werden. Weitere Informationen finden Sie unter [Swipe-Verhalten](#swipe-behavior).

> [!NOTE]
> Streifen Inhalt und Streifen Elemente können Inline eingefügt oder als Ressourcen definiert werden.

## <a name="swipe-items"></a>Elemente schwenken

Die `LeftItems`- `RightItems`, `TopItems`-, `BottomItems` -und-Auflistungen sind vom Typ `SwipeItems`. Die `SwipeItems` -Klasse definiert die folgenden Eigenschaften:

- `Mode`vom Typ `SwipeMode`, der die Auswirkung einer Schwenk Interaktion angibt. Weitere Informationen zum wischen-Modus finden Sie unter [wischen-Modus](#swipe-mode).
- `SwipeBehaviorOnInvoked`vom Typ `SwipeBehaviorOnInvoked`, der angibt, wie sich `SwipeView` ein verhält, nachdem ein wischen-Element aufgerufen wurde. Weitere Informationen zum wischen-Verhalten finden Sie unter [wischen-Verhalten](#swipe-behavior).

Diese Eigenschaften werden von [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Objekten unterstützt. Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Jedes "Wischen"-Element wird `SwipeItem` als-Objekt definiert, das in einer der `SwipeItems` vier direktionalen Auflistungen platziert wird. Die `SwipeItem` -Klasse wird von [`MenuItem`](xref:Xamarin.Forms.MenuItem) der-Klasse abgeleitet und fügt die folgenden Member hinzu:

- Eine `BackgroundColor` Eigenschaft vom Typ `Color`, die die Hintergrundfarbe für das Schwenk Element definiert. Diese Eigenschaft wird durch eine bindbare Eigenschaft unterstützt.
- Ein `Invoked` Ereignis, das ausgelöst wird, wenn das wischen-Element ausgeführt wird.

> [!IMPORTANT]
> Die [`MenuItem`](xref:Xamarin.Forms.MenuItem) -Klasse definiert mehrere Eigenschaften, `Command`einschließlich `CommandParameter`, `IconImageSource`, und `Text`. Diese Eigenschaften können für ein `SwipeItem` -Objekt festgelegt werden, um die Darstellung zu definieren, `ICommand` und um einen zu definieren, der ausgeführt wird, wenn das wischen-Element aufgerufen wird. Weitere Informationen finden Sie unter [xamarin. Forms MenuItem](~/xamarin-forms/user-interface/menuitem.md).

Im folgenden Beispiel werden zwei `SwipeItem` -Objekte in `LeftItems` der-Auflistung `SwipeView`einer angezeigt:

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

Die Darstellung der einzelnen `SwipeItem` wird durch eine Kombination der Eigenschaften `Text`, `IconImageSource`und `BackgroundColor` definiert:

[![Screenshot der swidanview-Streifen Elemente unter IOS und Android](swipeview-images/swipeview-swipeitems.png "Swipinview-Streifen Elemente")](swipeview-images/swipeview-swipeitems-large.png#lightbox "Swipinview-Streifen Elemente")

Wenn ein `SwipeItem` -Ereignis abgetippt `Invoked` wird, wird das zugehörige-Ereignis ausgelöst und vom registrierten Ereignishandler behandelt. Alternativ kann die `Command` -Eigenschaft auf eine `ICommand` -Implementierung festgelegt werden, die ausgeführt wird `SwipeItem` , wenn aufgerufen wird.

> [!NOTE]
> Wenn die Darstellung eines nur `SwipeItem` mit der-Eigenschaft oder `Text` `IconImageSource` der-Eigenschaft definiert wird, wird der Inhalt immer zentriert.

Neben der Definition von Schwenk Elementen als `SwipeItem` Objekte ist es auch möglich, benutzerdefinierte Streifen Element Sichten zu definieren. Weitere Informationen finden Sie unter [benutzerdefinierte wischen-Elemente](#custom-swipe-items).

## <a name="swipe-direction"></a>Richtung schwenken

`SwipeView`unterstützt vier verschiedene Richtung, bei der die Richtung von der direktionalen `SwipeItems` Auflistung definiert wird, `SwipeItem` zu der die Objekte hinzugefügt werden. Jede Richtung kann eigene Streifen Elemente enthalten. Das folgende Beispiel zeigt z. b. `SwipeView` ein-Element, dessen Streifen Elemente von der Schwenk Richtung abhängen:

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
> Auf einem `SwipeView`kann jeweils nur eine `SwipeItems` Instanz einer direktionalen Auflistung festgelegt werden. Daher können Sie nicht über zwei `LeftItems` Definitionen für einen `SwipeView`verfügen.

Das `SwipeStarted`- `SwipeChanging`Ereignis, `SwipeEnded` das-Ereignis und das-Ereignis melden `SwipeDirection` die Schwenk Richtung über die-Eigenschaft in den Ereignis Argumenten. Diese Eigenschaft hat den Typ `SwipeDirection`, bei dem es sich um eine Enumeration handelt, die aus vier Membern besteht:

- `Right`Gibt an, dass eine Rechte Schwenkbewegung aufgetreten ist.
- `Left`Gibt an, dass eine linke Schwenkbewegung aufgetreten ist.
- `Up`Gibt an, dass ein aufwärts schwenken aufgetreten ist.
- `Down`Gibt an, dass eine Abwärtsbewegung aufgetreten ist.

## <a name="swipe-mode"></a>Schwenk Modus

Die `SwipeItems` -Klasse verfügt `Mode` über eine-Eigenschaft, die den Effekt einer wischen-Interaktion angibt. Diese Eigenschaft sollte auf einen der `SwipeMode` Enumerationsmember festgelegt werden:

- `Reveal`Gibt an, dass eine schwenkbare Elemente angezeigt wird. Dies ist der Standardwert der `SwipeItems.Mode`-Eigenschaft.
- `Execute`Gibt an, dass ein wischen die Schwenk Elemente ausführt.

Im Modus "anzeigen" wird ein `SwipeView` vom Benutzer zum Öffnen eines Menüs, das aus einem oder mehreren wischen Elementen besteht, zum Öffnen eines Menüs, das aus einem oder mehreren wischen Elementen hervorgeht, und für die Ausführung explizit tippen. Nachdem das Schwenk Element ausgeführt wurde, werden die Schwenk Elemente geschlossen, und der `SwipeView` Inhalt wird erneut angezeigt. Im Ausführungs Modus wird ein durch den Benutzer ein `SwipeView` , um ein Menü zu öffnen, das aus einem der weiteren Streifen Elemente besteht, die dann automatisch ausgeführt werden. Nach der Ausführung werden die Elemente geschlossen, und der `SwipeView` Inhalt wird erneut angezeigt.

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

In diesem Beispiel kann der `SwipeView` Inhalt direkt weitergeleitet werden, um das Schwenk Element anzuzeigen, das sofort ausgeführt wird. Nach der Ausführung wird `SwipeView` der Inhalt erneut angezeigt.

## <a name="swipe-behavior"></a>Swipe-Verhalten

Die `SwipeItems` -Klasse verfügt `SwipeBehaviorOnInvoked` über eine-Eigenschaft, die `SwipeView` angibt, wie sich ein verhält, nachdem ein wischen-Element aufgerufen wurde. Diese Eigenschaft sollte auf einen der `SwipeBehaviorOnInvoked` Enumerationsmember festgelegt werden:

- `Auto`Gibt an, dass im Ausrichtungsmodus die `SwipeView` geschlossen wird, nachdem ein Schwenk Element aufgerufen wurde, und `SwipeView` im Ausführungs Modus bleibt geöffnet, nachdem ein Schwenk Element aufgerufen wurde. Dies ist der Standardwert der `SwipeItems.SwipeBehaviorOnInvoked`-Eigenschaft.
- `Close`Gibt an, `SwipeView` dass geschlossen wird, nachdem ein Schwenk Element aufgerufen wurde.
- `RemainOpen`Gibt an, `SwipeView` dass geöffnet bleibt, nachdem ein Element aufgerufen wurde.

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

Benutzerdefinierte wischen-Elemente können mit dem `SwipeItemView` -Typ definiert werden. Die `SwipeItemView` -Klasse wird von [`ContentView`](xref:Xamarin.Forms.ContentView) der-Klasse abgeleitet und fügt die folgenden Eigenschaften hinzu:

- `Command`vom Typ `ICommand`, der ausgeführt wird, wenn ein Schwenk Element getippt wird.
- `CommandParameter` vom Typ `object`: der Parameter, der an `Command` übergeben wird.

Diese Eigenschaften werden von [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Objekten unterstützt. Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Die `SwipeItemView` -Klasse definiert auch `Invoked` ein Ereignis, das ausgelöst wird, wenn das Element nach dem `Command` Ausführen von abgetippt wird.

Das folgende Beispiel zeigt ein `SwipeItemView` -Objekt in `LeftItems` der-Auflistung `SwipeView`einer:

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

In diesem `SwipeItemView` Beispiel besteht aus [`StackLayout`](xref:Xamarin.Forms.StackLayout) einem mit einem [`Entry`](xref:Xamarin.Forms.Entry) und einem. [`Label`](xref:Xamarin.Forms.Label) Nachdem der `Entry`Benutzereingaben in eingegeben hat, `SwipeViewItem` kann der Rest des-Objekts abgetippt werden, `ICommand` das die durch `SwipeItemView.Command` die-Eigenschaft definierte ausführt.

## <a name="open-and-close-a-swipeview-programmatically"></a>Programm gesteuertes öffnen und Schließen einer swidansicht

`SwipeView`schließt `Open` die `Close` -Methode und die-Methode ein, die die-Streifen Elemente Programm gesteuert öffnen und schließen.

Die `Open` -Methode erfordert `OpenSwipeItem` ein-Argument, um die Richtung `SwipeView` anzugeben, von der aus geöffnet wird. Die `OpenSwipeItem` -Enumeration hat vier Member:

- `LeftItems`Gibt an, dass das `SwipeView` -Element von Links aus geöffnet wird, um die wischen-Elemente in `LeftItems` der Auflistung anzuzeigen.
- `TopItems`Gibt an, dass das `SwipeView` -Element von oben aus geöffnet wird, um die Schwenk Elemente in der `TopItems` Auflistung anzuzeigen.
- `RightItems`Gibt an, dass das `SwipeView` -Element von der rechten Seite aus geöffnet wird, um die wischen- `RightItems` Elemente in der Auflistung anzuzeigen.
- `BottomItems`Gibt an, dass das `SwipeView` -Element von der unteren Seite aus geöffnet wird, um die Schwenk Elemente in `BottomItems` der Auflistung anzuzeigen.

`SwipeView` Im folgenden Beispiel `swipeView`wird gezeigt, wie ein `SwipeView` geöffnet wird, um die wischen-Elemente in der `LeftItems` Auflistung anzuzeigen:

```csharp
swipeView.Open(OpenSwipeItem.LeftItems);
```

Der `swipeView` kann dann mit der `Close` -Methode geschlossen werden:

```csharp
swipeView.Close();
```

> [!NOTE]
> Wenn die `Close` -Methode aufgerufen wird, `CloseRequested` wird das-Ereignis ausgelöst.

## <a name="disable-a-swipeview"></a>Deaktivieren von swipeer View

Eine Anwendung kann einen Status eingeben, bei dem das Schwenken eines Inhalts Elements kein gültiger Vorgang ist. In solchen Fällen kann der `SwipeView` deaktiviert werden, indem die zugehörige `IsEnabled` - `false`Eigenschaft auf festgelegt wird. Dadurch wird verhindert, dass Benutzerinhalte schwenken, um Streifen Elemente anzuzeigen.

`Command` Außerdem `ICommand` kann beim Definieren der-Eigenschaft eines `SwipeItem` oder `SwipeItemView`des `CanExecute` -Delegaten angegeben werden, um das wischen-Element zu aktivieren oder zu deaktivieren.

## <a name="related-links"></a>Verwandte Links

- [Swidanview (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-swipeviewdemos/)
- [Xamarin.Forms: MenuItem](~/xamarin-forms/user-interface/menuitem.md)
