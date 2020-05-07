---
title: Xamarin. Forms-Expander
description: Das xamarin. Forms Expander-Steuerelement stellt einen erweiterbaren Container zum Hosten beliebiger Inhalte bereit. Der Inhalt wird durch Tippen auf den Expander-Header angezeigt oder ausgeblendet.
ms.prod: xamarin
ms.assetid: 381DCB55-522D-4414-B45B-E8DD70AA9985
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/15/2020
ms.openlocfilehash: b1e573a6070a637ef2fdfa65bb0fc1375522fc3c
ms.sourcegitcommit: 443ecd9146fe2a7bbb9b5ab6d33c835876efcf1f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2020
ms.locfileid: "82852496"
---
# <a name="xamarinforms-expander"></a>Xamarin. Forms-Expander

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-expanderdemos/)

Das xamarin. Forms `Expander` -Steuerelement stellt einen erweiterbaren Container zum Hosten beliebiger Inhalte bereit. Das Steuerelement verfügt über einen Header und Inhalt, und der Inhalt wird durch Tippen auf den `Expander` Header angezeigt oder ausgeblendet. Wenn nur der `Expander` -Header angezeigt wird, `Expander` wird der *reduziert.* Wenn der `Expander` Inhalt sichtbar ist, `Expander` wird *erweitert*.

Die folgenden Screenshots zeigen eine `Expander` in den reduzierten und erweiterten Zuständen, wobei rote Felder den Header und den Inhalt angeben:

![Screenshot eines Expander in reduzierten und erweiterten Zuständen unter IOS und Android](expander-images/expander.png "Expander unter IOS und Android")

> [!IMPORTANT]
> `Expander`ist zurzeit experimentell und kann nur verwendet werden, indem `Expander_Experimental` das-Flag festgelegt wird. Weitere Informationen finden Sie unter [experimentelle Flags](~/xamarin-forms/internals/experimental-flags.md).
>
> Außerdem ist das `Expander` Steuerelement vollständig im- `Xamarin.Forms` Namespace implementiert. Daher ist Sie auf allen Plattformen verfügbar, die von xamarin. Forms unterstützt werden.

Das `Expander` -Steuerelement definiert die folgenden Eigenschaften:

- `CollapseAnimationEasing`vom Typ [`Easing`](xref:Xamarin.Forms.Easing), der die Beschleunigungs Funktion darstellt, die beim reduzieren auf den `Expander` Inhalt angewendet werden soll.
- `CollapseAnimationLength`vom Typ `uint`, der die Dauer der Animation definiert, wenn der `Expander` reduziert wird. Der Standardwert dieser Eigenschaft ist 250 ms.
- `Command`vom Typ `ICommand`, der ausgeführt wird, wenn der `Expander` Header getippt wird.
- `CommandParameter` vom Typ `object`: der Parameter, der an `Command` übergeben wird.
- `Content`vom Typ [`View`](xref:Xamarin.Forms.View), der den Inhalt definiert, der angezeigt werden soll, `Expander` wenn erweitert wird.
- `ContentTemplate`vom Typ [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) `Expander`. Dies ist die Vorlage, die verwendet wird, um den Inhalt von dynamisch aufzufüllen.
- `ExpandAnimationEasing`vom Typ [`Easing`](xref:Xamarin.Forms.Easing), der die Beschleunigungs Funktion darstellt, die während der Erweiterung auf `Expander` den Inhalt angewendet werden soll.
- `ExpandAnimationLength`vom Typ `uint`, der die Dauer der Animation definiert, wenn `Expander` erweitert wird. Der Standardwert dieser Eigenschaft ist 250 ms.
- `ForceUpdateSizeCommand`vom Typ `ICommand`, der den Befehl definiert, der ausgeführt `Expander` wird, wenn die Größe des-Aufforderungs Updates erzwungen wird. Diese Eigenschaft verwendet den `OneWayToSource` Bindungs Modus.
- `Header`vom Typ [`View`](xref:Xamarin.Forms.View), der den Header Inhalt definiert.
- `IsExpanded`vom Typ `bool`, der bestimmt, ob der `Expander` erweitert ist. Diese Eigenschaft verwendet den `TwoWay` -Bindungs Modus und hat den Standardwert `false`.
- `Spacing`vom Typ `double`, der den Leerraum zwischen dem Header und dessen Inhalt darstellt. Der Standardwert dieser Eigenschaft ist 0.
- `State`vom Typ `ExpanderState`, der den Zustand von darstellt `Expander`. Diese Eigenschaft verwendet den `OneWayToSource` Bindungs Modus.

Diese Eigenschaften werden von [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Objekten unterstützt. Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

> [!NOTE]
> Die `Content` -Eigenschaft ist die Content-Eigenschaft `Expander` der-Klasse und muss daher nicht explizit aus XAML festgelegt werden.

Die `ExpanderState`-Enumeration definiert die folgenden Member:

- `Expanding`Gibt an, `Expander` dass der erweitert wird.
- `Expanded`Gibt an, `Expander` dass der erweitert ist.
- `Collapsing`Gibt an, `Expander` dass reduziert wird.
- `Collapsed`Gibt an, `Expander` dass der reduziert ist.

Das `Expander` -Steuerelement definiert `Tapped` auch ein-Ereignis, das `Expander` ausgelöst wird, wenn der Header getippt wird. Außerdem `Expander` enthält eine `ForceUpdateSize` -Methode, die aufgerufen werden kann, um die Größe der `Expander` zur Laufzeit Programm gesteuert zu ändern.

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

In diesem Beispiel ist der `Expander` standardmäßig reduziert und zeigt [`Label`](xref:Xamarin.Forms.Label) als Header an. Das Tippen auf den Header bewirkt, `Expander` dass der erweitert wird, um seinen Inhalt anzuzeigen [`Grid`](xref:Xamarin.Forms.Grid) , bei dem es sich um ein untergeordnetes Steuerelement handelt Wenn der `Expander` erweitert wird, wird beim Tippen auf den `Expander`zugehörigen Header das reduziert.

> [!IMPORTANT]
> Wenn Sie die `Expander.Content` -Eigenschaft entweder implizit oder explizit festlegen, `Expander` wird der Inhalt erstellt, wenn die Seite, die Sie enthält, zu navigiert `Expander` wird, auch wenn der reduziert ist. Die `Expander.ContentTemplate` -Eigenschaft kann jedoch auf Inhalt festgelegt werden, der nur aufgeblasen `Expander` wird, wenn das zum ersten Mal erweitert wird. Weitere Informationen finden Sie [unter Erstellen von Expander-Inhalten nach Bedarf](#create-expander-content-on-demand).

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

`Expander`der Inhalt kann bei Bedarf als Reaktion auf den `Expander` erweiterbaren erstellt werden. Dies kann erreicht werden, indem die `Expander.ContentTemplate` -Eigenschaft auf [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) ein festgelegt wird, das den Inhalt enthält:

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

In diesem Beispiel wird der `Expander` Inhalt nur aufgeblasen, wenn `Expander` das zum ersten Mal erweitert wird.

Der Vorteil dieses Ansatzes besteht darin, dass der Inhalt für eine `Expander` `Expander` nur erstellt wird, wenn eine Seite mehrere Objekte enthält, wenn er vom Benutzer zum ersten Mal erweitert wird.

## <a name="add-an-expansion-indicator"></a>Erweiterungs Indikator hinzufügen

Ein [`Image`](xref:Xamarin.Forms.Image) kann einem- `Expander` Header hinzugefügt werden, um eine visuelle Angabe des Erweiterungs Zustands bereitzustellen. Ein [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) kann an den `Image`angefügt werden, der die `Source` -Eigenschaft basierend auf dem Wert der `Expander.IsExpanded` -Eigenschaft ändert:

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

Die `IsExpanded` -Eigenschaft `true` wird, `Expander` wenn der Header abgetippt wird, was `collapse` dazu führt, dass das Symbol angezeigt wird:

![Screenshot eines Expander-Symbols im Erweiterungs Zustand unter IOS und Android](expander-images/icon-collapse.png "Expandd-Symbol unter IOS und Android")

Weitere Informationen zu Triggern finden Sie unter [xamarin. Forms-Trigger](~/xamarin-forms/app-fundamentals/triggers.md).

## <a name="define-the-space-between-header-and-content"></a>Hiermit wird der Leerraum zwischen Header und Inhalt definiert.

Standardmäßig wird der Inhalt in einem `Expander` direkt unterhalb seines Headers angezeigt. Dieses Verhalten kann jedoch geändert werden, indem die `Spacing` -Eigenschaft auf einen `double` Wert festgelegt wird, der den leeren Leerraum zwischen dem Inhalt und dem zugehörigen Header darstellt:

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

Der Inhalt eines `Expander` kann auf ein anderes `Expander` Steuerelement festgelegt werden, um mehrere Ebenen der Erweiterung zu aktivieren. Der folgende XAML-Code `Expander` zeigt ein-Objekt `Expander` , dessen Inhalt ein anderes Objekt ist:

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

Wenn Sie in diesem Beispiel auf den `Expander` root-Header tippen, wird der `Expander`Header für das untergeordnete Element angezeigt:

![Screenshot eines eingebetteten Expander unter IOS und Android](expander-images/embedded-expander1.png "Eingebetteter Expander unter IOS und Android")

Das Tippen auf `Expander` den untergeordneten Header führt dazu, dass der Inhalt aufgeblasen und angezeigt wird:

![Screenshot eines eingebetteten Expander unter IOS und Android](expander-images/embedded-expander2.png "Eingebetteter Expander unter IOS und Android")

## <a name="define-the-expand-and-collapse-animation"></a>Definieren der Animation zum Erweitern und reduzieren

Die Animation, die auftritt, `Expander` wenn eine erweitert oder reduziert wird, kann durch `ExpandAnimationEasing` festlegen `CollapseAnimationEasing` der-Eigenschaft und der-Eigenschaft auf eine der in xamarin. Forms enthaltenen Beschleunigungsfunktionen oder benutzerdefinierte Beschleunigungsfunktionen definiert werden. Standardmäßig werden die Animationen zum Erweitern und reduzieren über 250 MS ausgeführt. Allerdings können diese Zeiträume geändert werden, indem die- `ExpandAnimationLength` Eigenschaft `CollapseAnimationLength` und die `uint` -Eigenschaft auf-Werte festgelegt werden.

Der folgende XAML-Code zeigt ein Beispiel für die Definition der Animation, `Expander` die auftritt, wenn der vom Benutzer erweitert oder reduziert wird:

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

In diesem Beispiel beschleunigt die `CubicIn` Beschleunigungs Funktion langsam die Erweiterungs Animation über 500 ms, und die `CubicOut` Beschleunigungs Funktion verlangsamt schnell die Reduzier Animation über 500 ms.

Weitere Informationen zu Beschleunigungsfunktionen finden Sie unter [xamarin. Forms](~/xamarin-forms/user-interface/animation/easing.md)-Beschleunigungsfunktionen.

## <a name="resize-an-expander-at-runtime"></a>Ändern der Größe eines Expander zur Laufzeit

Mit `Expander` der `ForceUpdateSize` -Methode kann eine programmgesteuerte Größe zur Laufzeit geändert werden.

Wenn ein `Expander` mit `expander`dem Namen enthält, dessen [`Label`](xref:Xamarin.Forms.Label) Inhalt ein enthält `TapGestureRecognizer` , dem eine angefügt ist, wird im folgenden Code `ForceUpdateSize` Beispiel das Aufrufen der-Methode veranschaulicht:

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

In diesem Beispiel ändert sich `FontSize` die einer [`Label`](xref:Xamarin.Forms.Label) -Änderung, `Label` wenn der getippt wird. Aufgrund der Größe der Schriftart, die geändert wird, müssen Sie die Größe des aktualisieren, `Expander` indem Sie die zugehörige- `ForceUpdateSize` Methode aufrufen.

## <a name="disable-an-expander"></a>Deaktivieren eines Expander

Eine Anwendung kann einen Status eingeben, in dem `Expander` die Erweiterung eines keinen gültigen Vorgang ist. In solchen Fällen kann die `Expander` durch Festlegen der- `IsEnabled` Eigenschaft auf false deaktiviert werden. Dadurch wird verhindert, dass Benutzer das `Expander`erweitern oder reduzieren.

## <a name="related-links"></a>Verwandte Links

- [Expander-Demos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-expanderdemos/)
- [Xamarin. Forms-Beschleunigungsfunktionen](~/xamarin-forms/user-interface/animation/easing.md)
- [Xamarin.Forms-Trigger](~/xamarin-forms/app-fundamentals/triggers.md)
- [Bindbare xamarin. Forms-Layouts](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
