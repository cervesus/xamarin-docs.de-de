---
title: Xamarin. Forms swipeer View
description: Bei xamarin. Forms swipeer View handelt es sich um ein Container Steuerelement, das ein Inhalts Element umschließt und Kontextmenü Elemente bereitstellt, die durch eine Schwenkbewegung angezeigt werden.
ms.prod: xamarin
ms.assetId: 602456B5-701B-4948-B454-B1F31283F1CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2019
ms.openlocfilehash: 4119a650c431013bb0c8e680de600ed4e73d0c93
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75490505"
---
# <a name="xamarinforms-swipeview"></a>Xamarin. Forms swipeer View

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-swipeviewdemos/)

Der `SwipeView` ist ein Container Steuerelement, das ein Inhalts Element umschließt und Kontextmenü Elemente bereitstellt, die durch eine Schwenk Geste angezeigt werden:

[![Screenshot der swipeer View-Elemente in einer CollectionView unter IOS und Android](swipeview-images/swipeview-collectionview.png "Swipinview-Streifen Elemente")](swipeview-images/swipeview-collectionview-large.png#lightbox "Swipinview-Streifen Elemente")

`SwipeView` ist in xamarin. Forms 4,4 verfügbar. Es ist jedoch zurzeit experimentell und kann nur verwendet werden, indem die folgende Codezeile zu ihrer `AppDelegate`-Klasse unter IOS, zu ihrer `MainActivity`-Klasse unter Android oder zur `App`-Klasse in UWP hinzugefügt wird, bevor `Forms.Init`aufgerufen wird:

```csharp
Forms.SetFlags("SwipeView_Experimental");
```

`SwipeView` definiert die folgenden Eigenschaften an:

- `LeftItems`vom Typ `SwipeItems`, der die schwingenden Elemente darstellt, die aufgerufen werden können, wenn das Steuerelement von der linken Seite entfernt wird.
- `RightItems`vom Typ `SwipeItems`, der die schwingenden Elemente darstellt, die aufgerufen werden können, wenn das Steuerelement von der rechten Seite entfernt wird.
- `TopItems`vom Typ `SwipeItems`, der die schwingenden Elemente darstellt, die aufgerufen werden können, wenn das Steuerelement von oben nach unten entfernt wird.
- `BottomItems`vom Typ `SwipeItems`, der die schwingenden Elemente darstellt, die aufgerufen werden können, wenn das Steuerelement von unten nach oben geleitet wird.

Diese Eigenschaften werden [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekten unterstützt. Dies bedeutet, dass Sie Ziele von Daten Bindungen sein können und formatiert sind.

Außerdem erbt die `SwipeView` die [`Content`](xref:Xamarin.Forms.ContentView.Content) -Eigenschaft von der [`ContentView`](xref:Xamarin.Forms.ContentView) -Klasse. Die `Content`-Eigenschaft ist die Content-Eigenschaft der `SwipeView`-Klasse und muss daher nicht explizit festgelegt werden.

Die `SwipeView`-Klasse definiert außerdem vier Ereignisse:

- `SwipeStarted` wird ausgelöst, wenn eine Schwenkbewegung gestartet wird. Das `SwipeStartedEventArgs` Objekt, das dieses Ereignis begleitet, verfügt über eine `SwipeDirection`-Eigenschaft vom Typ `SwipeDirection`.
- `SwipeChanging` wird ausgelöst, wenn der Schwenk bewegt wird. Das `SwipeChangingEventArgs` Objekt, das dieses Ereignis begleitet, verfügt über eine `SwipeDirection`-Eigenschaft des Typs `SwipeDirection`und eine `Offset`-Eigenschaft vom Typ `double`.
- `SwipeEnded` wird ausgelöst, wenn ein Schwenk endet. Das `SwipeEndedEventArgs` Objekt, das dieses Ereignis begleitet, verfügt über eine `SwipeDirection`-Eigenschaft vom Typ `SwipeDirection`.
- `CloseRequested` wird ausgelöst, wenn die schwenkbaren Elemente geschlossen werden.

Außerdem definiert `SwipeView` eine `Close`-Methode, die die Schwenk Elemente schließt.

> [!NOTE]
> `SwipeView` verfügt über eine plattformspezifische Plattform für IOS und Android, die den Übergang steuert, der beim Öffnen einer `SwipeView`verwendet wird. Weitere Informationen finden Sie unter [swipeer View Swipe Transition Mode on IOS](~/xamarin-forms/platform/ios/swipeview-swipetransitionmode.md) and [swipeer View Swipe Transition Mode on Android](~/xamarin-forms/platform/android/swipeview-swipetransitionmode.md).

## <a name="create-a-swipeview"></a>Erstellen einer swipeer View

Ein `SwipeView` muss den Inhalt definieren, der von der `SwipeView` umschlossen wird, und die von der Schwenkbewegung offenbarten Elemente. Die Schwenk Elemente sind ein oder mehrere `SwipeItem` Objekte, die in einer der vier `SwipeView` direktionalen Auflistungen `LeftItems`, `RightItems`, `TopItems`oder `BottomItems`platziert werden.

Das folgende Beispiel zeigt, wie Sie instanziieren ein `SwipeView` in XAML:

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

In diesem Beispiel ist der `SwipeView` Inhalt ein [`Grid`](xref:Xamarin.Forms.Grid) , der einen [`Label`](xref:Xamarin.Forms.Label)enthält:

[![Screenshot von swidanview-Inhalten unter IOS und Android](swipeview-images/swipeview-content.png "Inhalt von swidansicht")](swipeview-images/swipeview-content-large.png#lightbox "Inhalt von swidansicht")

Die wischen-Elemente werden verwendet, um Aktionen für den `SwipeView` Inhalt auszuführen. Sie werden angezeigt, wenn das Steuerelement von der linken Seite aus angezeigt wird:

[![Screenshot der swidanview-Streifen Elemente unter IOS und Android](swipeview-images/swipeview-swipeitems.png "Swipinview-Streifen Elemente")](swipeview-images/swipeview-swipeitems-large.png#lightbox "Swipinview-Streifen Elemente")

Standardmäßig wird ein Schwenk Element ausgeführt, wenn es vom Benutzer getippt wird. Dieses Verhalten kann jedoch geändert werden. Weitere Informationen finden Sie unter [Swipe-Modus](#swipe-mode).

Nachdem ein Schwenk Element ausgeführt wurde, werden die Elemente ausgeblendet, und der `SwipeView` Inhalt wird erneut angezeigt. Dieses Verhalten kann jedoch geändert werden. Weitere Informationen finden Sie unter [Swipe-Verhalten](#swipe-behavior).

> [!NOTE]
> Streifen Inhalt und Streifen Elemente können Inline eingefügt oder als Ressourcen definiert werden.

## <a name="swipe-items"></a>Elemente schwenken

Die `LeftItems`-, `RightItems`-, `TopItems`-und `BottomItems`-Auflistungen sind vom Typ `SwipeItems`. Die `SwipeItems`-Klasse definiert die folgenden Eigenschaften:

- `Mode`vom Typ `SwipeMode`, der die Auswirkung einer wischen-Interaktion angibt. Weitere Informationen zum wischen-Modus finden Sie unter [wischen-Modus](#swipe-mode).
- `SwipeBehaviorOnInvoked`vom Typ `SwipeBehaviorOnInvoked`, der angibt, wie sich ein `SwipeView` verhält, nachdem ein Schwenk Element aufgerufen wurde. Weitere Informationen zum wischen-Verhalten finden Sie unter [wischen-Verhalten](#swipe-behavior).

Diese Eigenschaften werden [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekten unterstützt. Dies bedeutet, dass Sie Ziele von Daten Bindungen sein können und formatiert sind.

Jedes Schwenk Element wird als `SwipeItem` Objekt definiert, das in einer der vier `SwipeItems` direktionalen Auflistungen platziert wird. Die `SwipeItem`-Klasse wird von der [`MenuItem`](xref:Xamarin.Forms.MenuItem) -Klasse abgeleitet und fügt die folgenden Member hinzu:

- Eine `BackgroundColor` Eigenschaft vom Typ "`Color`", die die Hintergrundfarbe des Schwenk Elements definiert. Diese Eigenschaft wird durch eine bindbare Eigenschaft unterstützt.
- Ein `Invoked` Ereignis, das ausgelöst wird, wenn das wischen-Element ausgeführt wird.

> [!IMPORTANT]
> Die [`MenuItem`](xref:Xamarin.Forms.MenuItem) -Klasse definiert mehrere Eigenschaften, einschließlich `Command`, `CommandParameter`, `IconImageSource`und `Text`. Diese Eigenschaften können für ein `SwipeItem` Objekt festgelegt werden, um die Darstellung zu definieren, und um eine `ICommand` zu definieren, die ausgeführt wird, wenn das wischen-Element aufgerufen wird. Weitere Informationen finden Sie unter [xamarin. Forms MenuItem](~/xamarin-forms/user-interface/menuitem.md).

Das folgende Beispiel zeigt zwei `SwipeItem` Objekte in der `LeftItems` Auflistung eines `SwipeView`:

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

Die Darstellung der einzelnen `SwipeItem` wird durch die Eigenschaften `Text`, `IconImageSource`und `BackgroundColor` definiert:

[![Screenshot der swidanview-Streifen Elemente unter IOS und Android](swipeview-images/swipeview-swipeitems.png "Swipinview-Streifen Elemente")](swipeview-images/swipeview-swipeitems-large.png#lightbox "Swipinview-Streifen Elemente")

Wenn ein `SwipeItem` abgetippt wird, wird das `Invoked` Ereignis ausgelöst und vom registrierten Ereignishandler behandelt. Alternativ kann die `Command`-Eigenschaft auf eine `ICommand` Implementierung festgelegt werden, die ausgeführt wird, wenn die `SwipeItem` aufgerufen wird.

> [!NOTE]
> Neben der Definition von Schwenk Elementen als `SwipeItem` Objekten ist es auch möglich, benutzerdefinierte Streifen Element Sichten zu definieren. Weitere Informationen finden Sie unter [benutzerdefinierte wischen-Elemente](#custom-swipe-items).

## <a name="swipe-direction"></a>Richtung schwenken

`SwipeView` unterstützt vier verschiedene Streifen Richtungen, wobei die Richtung des direktionalen `SwipeItems` der Auflistung, der die `SwipeItem` Objekte hinzugefügt werden, die Richtung definiert wird. Jede Richtung kann eigene Streifen Elemente enthalten. Das folgende Beispiel zeigt z. b. eine `SwipeView`, deren wischen-Elemente von der Schwenk Richtung abhängen:

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
> Auf einem `SwipeView`kann jeweils nur eine Instanz einer direktionalen `SwipeItems` Auflistung festgelegt werden. Aus diesem Grund ist es nicht möglich, zwei `LeftItems` Definitionen für eine `SwipeView`zu haben.

Die `SwipeStarted`-, `SwipeChanging`-und `SwipeEnded`-Ereignisse melden die Richtung des in den Ereignis Argumenten über die `SwipeDirection`-Eigenschaft. Diese Eigenschaft hat den Typ `SwipeDirection`, bei dem es sich um eine Enumeration handelt, die aus vier Membern besteht:

- `Right` gibt an, dass eine Rechte Schwenkbewegung aufgetreten ist.
- `Left` gibt an, dass eine linke Schwenkbewegung aufgetreten ist.
- `Up` gibt an, dass ein aufwärts schwenken aufgetreten ist.
- `Down` gibt an, dass eine Abwärtsbewegung aufgetreten ist.

## <a name="swipe-mode"></a>Schwenk Modus

Die `SwipeItems`-Klasse verfügt über eine `Mode`-Eigenschaft, die den Effekt einer Schwenk Interaktion angibt. Diese Eigenschaft sollte auf einen der `SwipeMode` Enumerationsmember festgelegt werden:

- `Reveal` gibt an, dass eine schwenkbare Elemente angezeigt wird. Dies ist der Standardwert der `SwipeItems.Mode`-Eigenschaft.
- `Execute` gibt an, dass ein wischen die Schwenk Elemente ausführt.

Im Modus "offenlegen" wird der Benutzer durch einen `SwipeView` ein Menü geöffnet, das aus einem oder mehreren wischen Elementen besteht, und es muss explizit auf ein Schwenk Element tippen, um es auszuführen. Nachdem das Schwenk Element ausgeführt wurde, werden die Schwenk Elemente geschlossen, und der `SwipeView` Inhalt wird erneut angezeigt. Im Ausführungs Modus schwentiert der Benutzer eine `SwipeView`, um ein Menü zu öffnen, das aus einem der weiteren Schwenk Elemente besteht, die dann automatisch ausgeführt werden. Nach der Ausführung werden die Elemente geschlossen, und der `SwipeView` Inhalt wird erneut angezeigt.

Das folgende Beispiel zeigt eine `SwipeView`, die für die Verwendung des Ausführungs Modus konfiguriert ist:

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

In diesem Beispiel kann der `SwipeView` Inhalt direkt weitergeleitet werden, um das Schwenk Element anzuzeigen, das sofort ausgeführt wird. Nach der Ausführung wird der `SwipeView` Inhalt erneut angezeigt.

## <a name="swipe-behavior"></a>Swipe-Verhalten

Die `SwipeItems`-Klasse verfügt über eine `SwipeBehaviorOnInvoked`-Eigenschaft, die angibt, wie sich ein `SwipeView` verhält, nachdem ein Schwenk Element aufgerufen wurde. Diese Eigenschaft sollte auf einen der `SwipeBehaviorOnInvoked` Enumerationsmember festgelegt werden:

- `Auto` gibt an, dass der `SwipeView` im Modus "anzeigen" geschlossen wird, nachdem ein Schwenk Element aufgerufen wurde, und im Ausführungs Modus bleibt der `SwipeView` geöffnet, nachdem ein Element aufgerufen wurde. Dies ist der Standardwert der `SwipeItems.SwipeBehaviorOnInvoked`-Eigenschaft.
- `Close` gibt an, dass die `SwipeView` geschlossen wird, nachdem ein Schwenk Element aufgerufen wurde.
- `RemainOpen` gibt an, dass der `SwipeView` geöffnet bleibt, nachdem ein Schwenk Element aufgerufen wurde.

Das folgende Beispiel zeigt eine `SwipeView`, die so konfiguriert sind, dass Sie geöffnet bleibt, nachdem ein Element aufgerufen wurde:

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

Benutzerdefinierte wischen-Elemente können mit dem `SwipeItemView`-Typ definiert werden. Die `SwipeItemView`-Klasse wird von der-Klasse [`ContentView`](xref:Xamarin.Forms.ContentView) abgeleitet und fügt die folgenden Eigenschaften hinzu:

- `Command`vom Typ `ICommand`, der ausgeführt wird, wenn ein Schwenk Element getippt wird.
- `CommandParameter` vom Typ `object`: der Parameter, der an `Command` übergeben wird.

Diese Eigenschaften werden [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekten unterstützt. Dies bedeutet, dass Sie Ziele von Daten Bindungen sein können und formatiert sind.

Die `SwipeItemView`-Klasse definiert auch ein `Invoked` Ereignis, das ausgelöst wird, wenn das Element nach der Ausführung des `Command` abgerufen wird.

Das folgende Beispiel zeigt ein `SwipeItemView` Objekt in der `LeftItems`-Auflistung eines `SwipeView`:

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

In diesem Beispiel umfasst die `SwipeItemView` eine [`StackLayout`](xref:Xamarin.Forms.StackLayout) , die eine [`Entry`](xref:Xamarin.Forms.Entry) und eine [`Label`](xref:Xamarin.Forms.Label)enthält. Nachdem der Benutzereingaben in den `Entry`eingegeben hat, kann der Rest des `SwipeViewItem` abgetippt werden, der die `ICommand` ausführt, die von der `SwipeItemView.Command`-Eigenschaft definiert wird.

## <a name="disable-a-swipeview"></a>Deaktivieren von swipeer View

Eine Anwendung kann einen Status eingeben, bei dem das Schwenken eines Inhalts Elements kein gültiger Vorgang ist. In solchen Fällen kann die `SwipeView` durch Festlegen der `IsEnabled`-Eigenschaft auf `false`deaktiviert werden. Dadurch wird verhindert, dass Benutzerinhalte schwenken, um Streifen Elemente anzuzeigen.

Außerdem kann beim Definieren der `Command`-Eigenschaft eines `SwipeItem` oder `SwipeItemView`der `CanExecute` Delegat des `ICommand` angegeben werden, um das wischen-Element zu aktivieren oder zu deaktivieren.

## <a name="related-links"></a>Verwandte Links

- [Swidanview (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-swipeviewdemos/)
- [Xamarin. Forms (MenuItem)](~/xamarin-forms/user-interface/menuitem.md)
