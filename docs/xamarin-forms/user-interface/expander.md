---
title: Xamarin.FormsExpander
description: Das Xamarin.Forms Expander-Steuerelement stellt einen erweiterbaren Container zum Hosten beliebiger Inhalte bereit. Der Inhalt wird durch Tippen auf den Expander-Header angezeigt oder ausgeblendet.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5e9afa0f6d27003891963af5715d5721e3129306
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84129532"
---
# <a name="xamarinforms-expander"></a>Xamarin.FormsExpander

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-expanderdemos/)

Das- Xamarin.Forms `Expander` Steuerelement stellt einen erweiterbaren Container zum Hosten beliebiger Inhalte bereit. Das Steuerelement verfügt über einen Header und Inhalt, und der Inhalt wird durch Tippen auf den Header angezeigt oder ausgeblendet `Expander` . Wenn nur der- `Expander` Header angezeigt wird, `Expander` wird *collapsed*der reduziert. Wenn der `Expander` Inhalt sichtbar ist, `Expander` wird *erweitert*.

Die folgenden Screenshots zeigen eine `Expander` in den reduzierten und erweiterten Zuständen, wobei rote Felder den Header und den Inhalt angeben:

![Screenshot eines Expander in reduzierten und erweiterten Zuständen unter IOS und Android](expander-images/expander.png "Expander unter IOS und Android")

> [!IMPORTANT]
> `Expander`ist zurzeit experimentell und kann nur verwendet werden, indem das-Flag festgelegt wird `Expander_Experimental` . Weitere Informationen finden Sie unter [experimentelle Flags](~/xamarin-forms/internals/experimental-flags.md).
>
> Außerdem `Expander` ist das Steuerelement vollständig im- `Xamarin.Forms` Namespace implementiert. Daher ist es auf allen Plattformen verfügbar, die von unterstützt werden Xamarin.Forms .

Das- `Expander` Steuerelement definiert die folgenden Eigenschaften:

- `CollapseAnimationEasing`vom Typ [`Easing`](xref:Xamarin.Forms.Easing) , der die Beschleunigungs Funktion darstellt, die beim reduzieren auf den Inhalt angewendet werden soll `Expander` .
- `CollapseAnimationLength`vom Typ `uint` , der die Dauer der Animation definiert, wenn der reduziert `Expander` wird. Der Standardwert dieser Eigenschaft ist 250 ms.
- `Command`vom Typ `ICommand` , der ausgeführt wird, wenn der `Expander` Header getippt wird.
- `CommandParameter` vom Typ `object`: der Parameter, der an `Command` übergeben wird.
- `Content`vom Typ [`View`](xref:Xamarin.Forms.View) , der den Inhalt definiert, der angezeigt werden soll, wenn `Expander` erweitert wird.
- `ContentTemplate`vom Typ [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) . Dies ist die Vorlage, die verwendet wird, um den Inhalt von dynamisch aufzufüllen `Expander` .
- `ExpandAnimationEasing`vom Typ [`Easing`](xref:Xamarin.Forms.Easing) , der die Beschleunigungs Funktion darstellt, die während der Erweiterung auf den Inhalt angewendet werden soll `Expander` .
- `ExpandAnimationLength`vom Typ `uint` , der die Dauer der Animation definiert, wenn erweitert wird `Expander` . Der Standardwert dieser Eigenschaft ist 250 ms.
- `ForceUpdateSizeCommand`vom Typ `ICommand` , der den Befehl definiert, der ausgeführt wird, wenn die Größe des-Aufforderungs Updates `Expander` erzwungen wird. Diese Eigenschaft verwendet den `OneWayToSource` Bindungs Modus.
- `Header`vom Typ [`View`](xref:Xamarin.Forms.View) , der den Header Inhalt definiert.
- `IsExpanded`vom Typ `bool` , der bestimmt, ob der `Expander` erweitert ist. Diese Eigenschaft verwendet den `TwoWay` -Bindungs Modus und hat den Standardwert `false` .
- `Spacing`vom Typ `double` , der den Leerraum zwischen dem Header und dessen Inhalt darstellt. Der Standardwert dieser Eigenschaft ist 0.
- `State`vom Typ `ExpanderState` , der den Zustand von darstellt `Expander` . Diese Eigenschaft verwendet den `OneWayToSource` Bindungs Modus.

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

> [!NOTE]
> Die `Content` -Eigenschaft ist die Content-Eigenschaft der `Expander` -Klasse und muss daher nicht explizit aus XAML festgelegt werden.

Die `ExpanderState`-Enumeration definiert die folgenden Member:

- `Expanding`Gibt an, dass der `Expander` erweitert wird.
- `Expanded`Gibt an, dass der `Expander` erweitert ist.
- `Collapsing`Gibt an, dass reduziert `Expander` wird.
- `Collapsed`Gibt an, dass der reduziert `Expander` ist.

Das `Expander` -Steuerelement definiert auch ein `Tapped` -Ereignis, das ausgelöst wird, wenn der `Expander` Header getippt wird. Außerdem `Expander` enthält eine- `ForceUpdateSize` Methode, die aufgerufen werden kann, um die Größe der zur Laufzeit Programm gesteuert zu ändern `Expander` .

## <a name="create-an-expander"></a>Erstellen eines Expander

Im folgenden Beispiel wird gezeigt, wie ein `Expander` in XAML instanziiert wird:

```xaml
<Expander>
    <Expander.Header>
        <Label Text="Baboon"
               FontAttributes="Bold"
               FontSize="Medium" />
    </Expander.Header>
    <Grid Padding="10">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>
        <Image Source="http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
               Aspect="AspectFill"
               HeightRequest="120"
               WidthRequest="120" />
        <Label Grid.Column="1"
               Text="Baboons are African and Arabian Old World monkeys belonging to the genus Papio, part of the subfamily Cercopithecinae."
               FontAttributes="Italic" />
    </Grid>
</Expander>
```

In diesem Beispiel ist der `Expander` standardmäßig reduziert und zeigt [`Label`](xref:Xamarin.Forms.Label) als Header an. Das Tippen auf den Header bewirkt, dass der `Expander` erweitert wird, um seinen Inhalt anzuzeigen, bei dem es sich um ein untergeordnetes Steuerelement handelt [`Grid`](xref:Xamarin.Forms.Grid) Wenn der `Expander` erweitert wird, wird beim Tippen auf den zugehörigen Header das reduziert `Expander` .

> [!IMPORTANT]
> Wenn Sie die- `Expander.Content` Eigenschaft entweder implizit oder explizit festlegen, `Expander` wird der Inhalt erstellt, wenn die Seite, die Sie enthält, zu navigiert wird, auch wenn der reduziert `Expander` ist. Die- `Expander.ContentTemplate` Eigenschaft kann jedoch auf Inhalt festgelegt werden, der nur aufgeblasen wird, wenn das `Expander` zum ersten Mal erweitert wird. Weitere Informationen finden Sie [unter Erstellen von Expander-Inhalten nach Bedarf](#create-expander-content-on-demand).

Alternativ kann eine `Expander` im Code erstellt werden:

```csharp
Expander expander = new Expander
{
    Header = new Label
    {
        Text = "Baboon",
        FontAttributes = FontAttributes.Bold,
        FontSize = Device.GetNamedSize(NamedSize.Medium, typeof(Label))
    }
};

Grid grid = new Grid
{
    Padding = new Thickness(10),
    ColumnDefinitions =
    {
        new ColumnDefinition { Width = GridLength.Auto },
        new ColumnDefinition { Width = GridLength.Auto }
    }
};

grid.Children.Add(new Image
{
    Source = "http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg",
    Aspect = Aspect.AspectFill,
    HeightRequest = 120,
    WidthRequest = 120
});

grid.Children.Add(new Label
{
    Text = "Baboons are African and Arabian Old World monkeys belonging to the genus Papio, part of the subfamily Cercopithecinae.",
    FontAttributes = FontAttributes.Italic
}, 1, 0);

expander.Content = grid;
```

## <a name="create-expander-content-on-demand"></a>Erstellen von Expander-Inhalten nach Bedarf

`Expander`der Inhalt kann bei Bedarf als Reaktion auf den `Expander` erweiterbaren erstellt werden. Dies kann erreicht werden, indem die- `Expander.ContentTemplate` Eigenschaft auf ein festgelegt wird [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , das den Inhalt enthält:

```xaml
<Expander>
    <Expander.Header>
        <Label Text="{Binding Name}"
               FontAttributes="Bold"
               FontSize="Medium" />
    </Expander.Header>
    <Expander.ContentTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                </Grid.ColumnDefinitions>
                <Image Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="120"
                       WidthRequest="120" />
                <Label Grid.Column="1"
                       Text="{Binding Details}"
                       FontAttributes="Italic" />
            </Grid>
        </DataTemplate>
    </Expander.ContentTemplate>
</Expander>
```

In diesem Beispiel wird der `Expander` Inhalt nur aufgeblasen, wenn das `Expander` zum ersten Mal erweitert wird.

Der Vorteil dieses Ansatzes besteht darin, dass `Expander` der Inhalt für eine nur erstellt wird, wenn eine Seite mehrere Objekte enthält, `Expander` Wenn er vom Benutzer zum ersten Mal erweitert wird.

## <a name="add-an-expansion-indicator"></a>Erweiterungs Indikator hinzufügen

Ein [`Image`](xref:Xamarin.Forms.Image) kann einem-Header hinzugefügt werden `Expander` , um eine visuelle Angabe des Erweiterungs Zustands bereitzustellen. Ein [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) kann an den angefügt werden `Image` , der die- `Source` Eigenschaft basierend auf dem Wert der- `Expander.IsExpanded` Eigenschaft ändert:

```xaml
<Expander>
    <Expander.Header>
        <Grid>
            <Label Text="{Binding Name}"
                   FontAttributes="Bold"
                   FontSize="Medium" />
            <Image Source="expand.png"
                   HorizontalOptions="End"
                   VerticalOptions="Start">
                <Image.Triggers>
                    <DataTrigger TargetType="Image"
                                 Binding="{Binding Source={RelativeSource AncestorType={x:Type Expander}}, Path=IsExpanded}"
                                 Value="True">
                        <Setter Property="Source"
                                Value="collapse.png" />
                    </DataTrigger>
                </Image.Triggers>
            </Image>
        </Grid>
    </Expander.Header>
    <Expander.ContentTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                </Grid.ColumnDefinitions>
                <Image Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="120"
                       WidthRequest="120" />
                <Label Grid.Column="1"
                       Text="{Binding Details}"
                       FontAttributes="Italic" />
            </Grid>
        </DataTemplate>
    </Expander.ContentTemplate>
</Expander>
```

In diesem Beispiel zeigt das [`Image`](xref:Xamarin.Forms.Image) `expand` Symbol standardmäßig an:

![Screenshot eines Expander-Symbols im reduzierten Zustand unter IOS und Android](expander-images/icon-expand.png "Expandd-Symbol unter IOS und Android")

Die- `IsExpanded` Eigenschaft wird `true` , wenn der `Expander` Header abgetippt wird, was dazu führt, `collapse` dass das Symbol angezeigt wird:

![Screenshot eines Expander-Symbols im Erweiterungs Zustand unter IOS und Android](expander-images/icon-collapse.png "Expandd-Symbol unter IOS und Android")

Weitere Informationen zu Triggern finden Sie unter [ Xamarin.Forms Trigger](~/xamarin-forms/app-fundamentals/triggers.md).

## <a name="define-the-space-between-header-and-content"></a>Hiermit wird der Leerraum zwischen Header und Inhalt definiert.

Standardmäßig wird der Inhalt in einem `Expander` direkt unterhalb seines Headers angezeigt. Dieses Verhalten kann jedoch geändert werden, indem die `Spacing` -Eigenschaft auf einen Wert festgelegt wird `double` , der den leeren Leerraum zwischen dem Inhalt und dem zugehörigen Header darstellt:

```xaml
<Expander Spacing="50"
          IsExpanded="true">
    <Expander.Header>
        <Label Text="Baboon"
               FontAttributes="Bold"
               FontSize="Medium" />
    </Expander.Header>
    <Grid Padding="10">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>
        <Image Source="http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
               Aspect="AspectFill"
               HeightRequest="120"
               WidthRequest="120" />
        <Label Grid.Column="1"
               Text="Baboons are African and Arabian Old World monkeys belonging to the genus Papio, part of the subfamily Cercopithecinae."
               FontAttributes="Italic" />
    </Grid>
</Expander>
```

In diesem Beispiel wird der `Expander` Inhalt 50 geräteunabhängige Einheiten unterhalb des Headers angezeigt:

![Screenshot eines Expander mit fest gelegenden Abstände unter IOS und Android](expander-images/expander-spacing.png "Expander mit einem in IOS und Android festgelegten Abstand")

## <a name="embed-an-expander-in-an-expander"></a>Einbetten eines Expander in eine Expander

Der Inhalt eines `Expander` kann auf ein anderes Steuerelement festgelegt werden `Expander` , um mehrere Ebenen der Erweiterung zu aktivieren. Der folgende XAML-Code zeigt ein- `Expander` Objekt, dessen Inhalt ein anderes `Expander` Objekt ist:

```xaml
<Expander Spacing="10">
    <Expander.Header>
        <Label Text="{Binding Name}"
               FontAttributes="Bold"
               FontSize="Medium" />
    </Expander.Header>
    <Expander Padding="10"
              Spacing="10">
        <Expander.Header>
            <Label Text="{Binding Location}"
                   FontSize="Medium" />
        </Expander.Header>
            <Expander.ContentTemplate>
                <DataTemplate>
                    <Grid>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="Auto" />
                            <ColumnDefinition Width="Auto" />
                        </Grid.ColumnDefinitions>
                        <Image Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="120"
                               WidthRequest="120" />
                        <Label Grid.Column="1"
                               Text="{Binding Details}"
                               FontAttributes="Italic" />
                    </Grid>
                </DataTemplate>
            </Expander.ContentTemplate>
    </Expander>
</Expander>
```

Wenn Sie in diesem Beispiel auf den root-Header tippen, wird `Expander` der Header für das untergeordnete Element angezeigt `Expander` :

![Screenshot eines eingebetteten Expander unter IOS und Android](expander-images/embedded-expander1.png "Eingebetteter Expander unter IOS und Android")

Das Tippen auf den untergeordneten `Expander` Header führt dazu, dass der Inhalt aufgeblasen und angezeigt wird:

![Screenshot eines eingebetteten Expander unter IOS und Android](expander-images/embedded-expander2.png "Eingebetteter Expander unter IOS und Android")

## <a name="define-the-expand-and-collapse-animation"></a>Definieren der Animation zum Erweitern und reduzieren

Die Animation, die auftritt, wenn eine `Expander` erweitert oder reduziert wird, kann durch Festlegen der `ExpandAnimationEasing` -Eigenschaft und der-Eigenschaft `CollapseAnimationEasing` auf eine der in enthaltenen Beschleunigungsfunktionen Xamarin.Forms oder benutzerdefinierte Beschleunigungsfunktionen definiert werden. Standardmäßig werden die Animationen zum Erweitern und reduzieren über 250 MS ausgeführt. Allerdings können diese Zeiträume geändert werden, indem die `ExpandAnimationLength` -Eigenschaft und die-Eigenschaft auf-Werte festgelegt werden `CollapseAnimationLength` `uint` .

Der folgende XAML-Code zeigt ein Beispiel für die Definition der Animation, die auftritt, wenn der `Expander` vom Benutzer erweitert oder reduziert wird:

```xaml
<Expander ExpandAnimationEasing="{x:Static Easing.CubicIn}"
          ExpandAnimationLength="500"
          CollapseAnimationEasing="{x:Static Easing.CubicOut}"
          CollapseAnimationLength="500">
    <Expander.Header>
        <Label Text="{Binding Name}"
               FontAttributes="Bold"
               FontSize="Medium" />
    </Expander.Header>
    <Expander.ContentTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                </Grid.ColumnDefinitions>
                <Image Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="120"
                       WidthRequest="120" />
                <Label Grid.Column="1"
                       Text="{Binding Details}"
                       FontAttributes="Italic" />
            </Grid>
        </DataTemplate>
    </Expander.ContentTemplate>
</Expander>
```

In diesem Beispiel beschleunigt die Beschleunigungs `CubicIn` Funktion langsam die Erweiterungs Animation über 500 ms, und die Beschleunigungs `CubicOut` Funktion verlangsamt schnell die Reduzier Animation über 500 ms.

Weitere Informationen zu Beschleunigungsfunktionen finden Sie unter Beschleunigungs [ Xamarin.Forms Funktionen](~/xamarin-forms/user-interface/animation/easing.md).

## <a name="resize-an-expander-at-runtime"></a>Ändern der Größe eines Expander zur Laufzeit

`Expander`Mit der-Methode kann eine programmgesteuerte Größe zur Laufzeit geändert werden `ForceUpdateSize` .

Wenn ein `Expander` `expander` mit dem Namen enthält, dessen Inhalt ein enthält, [`Label`](xref:Xamarin.Forms.Label) `TapGestureRecognizer` dem eine angefügt ist, wird im folgenden Codebeispiel das Aufrufen der- `ForceUpdateSize` Methode veranschaulicht:

```csharp
void OnLabelTapped(object sender, EventArgs e)
{
    Label label = sender as Label;
    Expander expander = label.Parent.Parent.Parent as Expander;

    if (label.FontSize == Device.GetNamedSize(NamedSize.Default, label))
    {
        label.FontSize = Device.GetNamedSize(NamedSize.Large, label);
    }
    else
    {
        label.FontSize = Device.GetNamedSize(NamedSize.Default, label);
    }
    expander.ForceUpdateSize();
}
```

In diesem Beispiel ändert sich die `FontSize` einer-Änderung, [`Label`](xref:Xamarin.Forms.Label) Wenn der `Label` getippt wird. Aufgrund der Größe der Schriftart, die geändert wird, müssen Sie die Größe des aktualisieren, indem Sie die zugehörige- `Expander` `ForceUpdateSize` Methode aufrufen.

## <a name="disable-an-expander"></a>Deaktivieren eines Expander

Eine Anwendung kann einen Status eingeben, in dem die Erweiterung eines `Expander` keinen gültigen Vorgang ist. In solchen Fällen kann die `Expander` durch Festlegen der- `IsEnabled` Eigenschaft auf false deaktiviert werden. Dadurch wird verhindert, dass Benutzer das Erweitern oder reduzieren `Expander` .

## <a name="related-links"></a>Verwandte Links

- [Expander-Demos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-expanderdemos/)
- [Xamarin.FormsBeschleunigungsfunktionen](~/xamarin-forms/user-interface/animation/easing.md)
- [Xamarin.FormsAuslöst](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.FormsBindbare Layouts](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
