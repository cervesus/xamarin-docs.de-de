---
title: Auswählen eines xamarin. Forms-Layouts
description: Mit xamarin. Forms-layoutklassen können Sie UI-Steuerelemente in Ihrer Anwendung anordnen und gruppieren.
ms.prod: xamarin
ms.assetid: 05A39752-A174-447E-A30D-3CC9EF98CB96
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/21/2018
ms.openlocfilehash: 161da8948f356fef997a411855598bc99d2f49b7
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69893997"
---
# <a name="choose-a-xamarinforms-layout"></a>Auswählen eines xamarin. Forms-Layouts

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)

Mit xamarin. Forms-layoutklassen können Sie UI-Steuerelemente in Ihrer Anwendung anordnen und gruppieren. Wenn Sie eine Layoutklasse auswählen, müssen Sie wissen, wie das Layout seine untergeordneten Elemente positioniert und wie das Layout seine untergeordneten Elemente ändert. Außerdem kann es erforderlich sein, Layouts zu schachteln, um das gewünschte Layout zu erstellen.

Die folgende Abbildung zeigt typische Layouts, die mit den xamarin. Forms-Haupt layoutklassen erreicht werden können:

[ ![Die hauptlayoutklassen in xamarin. Forms](images/layouts.png "xamarin. Forms-layoutklassen") ] (images/layouts-large.png#lightbox "Xamarin. Forms-layoutklassen")

## <a name="stacklayout"></a>StackLayout

Ein [`StackLayout`](xref:Xamarin.Forms.StackLayout) organisiert Elemente in einem eindimensionalen Stapel, entweder horizontal oder vertikal. Die [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) -Eigenschaft gibt die Richtung der Elemente an, und die Standardausrichtung [`Vertical`](xref:Xamarin.Forms.StackOrientation)ist. `StackLayout`wird normalerweise verwendet, um einen unter Abschnitt der Benutzeroberfläche auf einer Seite anzuordnen.

Der folgende XAML-Code zeigt, wie ein [`StackLayout`](xref:Xamarin.Forms.StackLayout) vertikal mit [`Label`](xref:Xamarin.Forms.Label) drei Objekten erstellt wird:

```xaml
<StackLayout Margin="20,35,20,25">
    <Label Text="The StackLayout has its Margin property set, to control the rendering position of the StackLayout." />
    <Label Text="The Padding property can be set to specify the distance between the StackLayout and its children." />
    <Label Text="The Spacing property can be set to specify the distance between views in the StackLayout." />
</StackLayout>
```

Wenn in [`StackLayout`](xref:Xamarin.Forms.StackLayout)einer die Größe eines Elements nicht explizit festgelegt ist, wird es erweitert, um die verfügbare Breite auszufüllen, oder [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) die Höhe, wenn [`Horizontal`](xref:Xamarin.Forms.StackOrientation)die-Eigenschaft auf festgelegt ist.

Eine [`StackLayout`](xref:Xamarin.Forms.StackLayout) wird häufig als übergeordnetes Layout verwendet, das andere untergeordnete Layouts enthält. Ein `StackLayout` sollte jedoch nicht zum reproduzieren eines [`Grid`](xref:Xamarin.Forms.Grid) Layouts mithilfe einer Kombination von `StackLayout` -Objekten verwendet werden. Der folgende Code zeigt ein Beispiel für diese fehl Vorgehensweise:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Details.HomePage"
             Padding="0,20,0,0">
    <StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Name:" />
            <Entry Placeholder="Enter your name" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Age:" />
            <Entry Placeholder="Enter your age" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Occupation:" />
            <Entry Placeholder="Enter your occupation" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Address:" />
            <Entry Placeholder="Enter your address" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

Dies ist Vergeudung, da unnötige Layoutberechnungen durchgeführt werden. Stattdessen kann das gewünschte Layout besser mithilfe eines [`Grid`](xref:Xamarin.Forms.Grid)erreicht werden.

> [!TIP]
> Wenn Sie einen [`StackLayout`](xref:Xamarin.Forms.StackLayout)verwenden, stellen Sie sicher, dass nur ein unter [`LayoutOptions.Expands`](xref:Xamarin.Forms.LayoutOptions.Expands)geordnetes Element auf festgelegt ist. Mit dieser Eigenschaft wird sichergestellt, dass das angegebene untergeordnete Element den größten Bereich belegt, der im `StackLayout` verfügbar ist. Zudem ist es Vergeudung, diese Berechnungen mehrmals durchzuführen.

Weitere Informationen finden Sie unter [xamarin. Forms Stacklayout](stack-layout.md).

## <a name="grid"></a>Raster

Ein [`Grid`](xref:Xamarin.Forms.Grid) wird zum Anzeigen von Elementen in Zeilen und Spalten verwendet, die eine proportionale oder absolute Größe aufweisen können. Die Zeilen und Spalten eines Rasters werden mit der [`RowDefinitions`](xref:Xamarin.Forms.Grid.RowDefinitions) - [`ColumnDefinitions`](xref:Xamarin.Forms.Grid.ColumnDefinitions) Eigenschaft und der-Eigenschaft angegeben.

Um Elemente in bestimmten [`Grid`](xref:Xamarin.Forms.Grid) Zellen zu positionieren, verwenden Sie die [`Grid.Column`](xref:Xamarin.Forms.Grid.ColumnProperty) angefügten Eigenschaften und [`Grid.Row`](xref:Xamarin.Forms.Grid.RowProperty) . Um Elemente über mehrere Zeilen und Spalten hinweg zu spannen, verwenden [`Grid.RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty) Sie [`Grid.ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) die angefügten Eigenschaften und.

> [!NOTE]
> Ein [`Grid`](xref:Xamarin.Forms.Grid) Layout sollte nicht mit Tabellen verwechselt werden und ist nicht für die Darstellung von Tabellendaten vorgesehen. Im Gegensatz zu HTML- `Grid` Tabellen ist ein zum Anordnen von Inhalt vorgesehen. Zum Anzeigen von Tabellendaten sollten Sie die Verwendung von [ListView](~/xamarin-forms/user-interface/listview/index.md), [CollectionView](~/xamarin-forms/user-interface/collectionview/index.md)oder [TableView](~/xamarin-forms/user-interface/tableview.md)in Erwägung gezogen.

Der folgende XAML-Code zeigt, wie [`Grid`](xref:Xamarin.Forms.Grid) Sie einen mit zwei Zeilen und zwei Spalten erstellen:

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="50" />
        <RowDefinition Height="50" />
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto" />
        <ColumnDefinition />
    </Grid.ColumnDefinitions>    
    <Label Text="Column 0, Row 0"
           Width="200" />
    <Label Grid.Column="1"
           Text="Column 1, Row 0" />
    <Label Grid.Row="1"
           Text="Column 0, Row 1" />
    <Label Grid.Column="1"
           Grid.Row="1"
           Text="Column 1, Row 1" />
</Grid>
```

In diesem Beispiel funktioniert die Größenanpassung wie folgt:

- Jede Zeile hat eine explizite Höhe von 50 geräteunabhängigen Einheiten.
- Die Breite der ersten Spalte wird auf festgelegt [`Auto`](xref:Xamarin.Forms.GridLength.Auto)und ist daher für die untergeordneten Elemente so breit wie erforderlich. In diesem Fall handelt es sich um 200 geräteunabhängige Einheiten, die der Breite des ersten [`Label`](xref:Xamarin.Forms.Label)entsprechen.

Der Speicherplatz kann in einer Spalte oder Zeile mithilfe der automatischen Größenanpassung verteilt werden, sodass die Größe von Spalten und Zeilen an ihren Inhalt angepasst werden kann. Dies wird erreicht, indem die Höhe [`RowDefinition`](xref:Xamarin.Forms.RowDefinition)eines oder die Breite [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition)eines auf [`Auto`](xref:Xamarin.Forms.GridLength.Auto)festgelegt wird. Die proportionale Größenanpassung kann auch verwendet werden, um den verfügbaren Platz zwischen den Zeilen und Spalten des Rasters durch gewichtete Proportionen zu verteilen. Dies wird erreicht, indem die Höhe `RowDefinition`eines oder die Breite `ColumnDefinition`eines auf einen Wert festgelegt wird, der den `*` Operator verwendet.

> [!CAUTION]
> Stellen Sie sicher, dass so wenige Zeilen und Spalten wie möglich auf [`Auto`](xref:Xamarin.Forms.GridLength.Auto) Größe festgelegt sind. Durch jede Zeile oder Spalte, deren Größe automatisch angepasst wird, wird verursacht, dass die Layout-Engine zusätzliche Layoutberechnungen durchführt. Verwenden Sie stattdessen wenn möglich Zeilen und Spalten mit festen Größen. Alternativ können Sie Zeilen und Spalten festlegen, um eine proportionale Menge an Speicher [`GridUnitType.Star`](xref:Xamarin.Forms.GridUnitType.Star) Platz mit dem-Enumerationswert zu belegen.

Weitere Informationen finden Sie unter [xamarin. Forms Grid](grid.md).

## <a name="flexlayout"></a>FlexLayout

Ein [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) ähnelt einem [`StackLayout`](xref:Xamarin.Forms.StackLayout) in, dass untergeordnete Elemente entweder horizontal oder vertikal in einem Stapel angezeigt werden. Allerdings kann `FlexLayout` ein auch seine untergeordneten Elemente einschließen, wenn zu viele für eine einzelne Zeile oder Spalte vorhanden sind, und eine präzisetere Steuerung der Größe, Ausrichtung und Ausrichtung der untergeordneten Elemente ermöglicht.

Der folgende XAML-Code zeigt, wie [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) ein erstellt wird, das seine Sichten in einer einzelnen Spalte anzeigt:

```xaml
<FlexLayout Direction="Column"
            AlignItems="Center"
            JustifyContent="SpaceEvenly">
    <Label Text="FlexLayout in Action" />
    <Button Text="Button" />
    <Label Text="Another Label" />
</FlexLayout>
```

In diesem Beispiel funktioniert das Layout wie folgt:

- Die [`Direction`](xref:Xamarin.Forms.FlexLayout.Direction) -Eigenschaft ist auf `Column`festgelegt, was `FlexLayout` bewirkt, dass die untergeordneten Elemente von in einer einzelnen Spalte von Elementen angeordnet werden.
- Die [`AlignItems`](xref:Xamarin.Forms.FlexLayout.AlignItems) -Eigenschaft ist auf `Center`festgelegt, was bewirkt, dass jedes Element horizontal zentriert wird.
- Die [`JustifyContent`](xref:Xamarin.Forms.FlexLayout.JustifyContent) -Eigenschaft ist auf `SpaceEvenly`festgelegt. Dadurch wird der gesamte restliche vertikale Leerraum gleichmäßig zwischen allen Elementen und oberhalb des ersten Elements und unterhalb des letzten Elements zugeordnet.

Weitere Informationen finden Sie unter [xamarin. Forms flexlayout](flex-layout.md).

## <a name="relativelayout"></a>RelativeLayout

Eine [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) wird verwendet, um Elemente relativ zu den Eigenschaften des Layouts oder der gleich geordneten Elemente zu positionieren und zu verkleinern. Standardmäßig wird ein-Element in der oberen linken Ecke des Layouts positioniert. Ein `RelativeLayout` kann verwendet werden, um Benutzeroberflächen zu erstellen, die proportional zwischen den Gerätegrößen skaliert werden.

[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)Innerhalb von werden Positionen und Größen als Einschränkungen angegeben. Einschränkungen verfügen [`Factor`](xref:Xamarin.Forms.ConstraintExpression.Factor) über [`Constant`](xref:Xamarin.Forms.ConstraintExpression.Constant) -und-Eigenschaften, die verwendet werden können, um Positionen und Größen als Vielfache (oder Bruch) von Eigenschaften anderer Objekte sowie eine Konstante zu definieren. Außerdem können Konstanten eine negative Zahl sein.

> [!NOTE]
> Ein [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) unterstützt die Positionierung von Elementen außerhalb ihrer eigenen Begrenzungen.

Der folgende XAML-Code zeigt, wie Elemente in [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)einem angeordnet werden:

```xaml
<RelativeLayout>
    <BoxView Color="Blue"
             HeightRequest="50"
             WidthRequest="50"
             RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0}"
             RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0}" />
    <BoxView Color="Red"
             HeightRequest="50"
             WidthRequest="50"
             RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=.85}"
             RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0}" />
    <BoxView x:Name="pole"
             Color="Gray"
             WidthRequest="15"
             RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.75}"
             RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=.45}"
             RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.25}" />
    <BoxView Color="Green"
             RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.10, Constant=10}"
             RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=.2, Constant=20}"
             RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToView, ElementName=pole, Property=X, Constant=15}"
             RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToView, ElementName=pole, Property=Y, Constant=0}" />
</RelativeLayout>
```

In diesem Beispiel funktioniert das Layout wie folgt:

- Der blaue [`BoxView`](xref:Xamarin.Forms.BoxView) Wert erhält eine explizite Größe von 50 x 50 geräteunabhängigen Einheiten. Sie befindet sich in der linken oberen Ecke des Layouts. Dies ist die Standardposition.
- Die rote [`BoxView`](xref:Xamarin.Forms.BoxView) Größe erhält eine explizite Größe von 50 x 50 geräteunabhängigen Einheiten. Sie befindet sich in der rechten oberen Ecke des Layouts.
- Das graue [`BoxView`](xref:Xamarin.Forms.BoxView) erhält eine explizite Breite von 15 geräteunabhängigen Einheiten, und die Höhe wird auf 75% der Höhe des übergeordneten Elements festgelegt.
- Das Grün [`BoxView`](xref:Xamarin.Forms.BoxView) erhält keine explizite Größe. Seine Position wird relativ zum `BoxView` benannten `pole`festgelegt.

> [!WARNING]
> Vermeiden Sie möglichst `RelativeLayout` die Verwendung von. Dies führt dazu, dass die CPU erheblich mehr Arbeit übernehmen muss.

Weitere Informationen finden Sie unter [xamarin. Forms relativelayout](relative-layout.md).

## <a name="absolutelayout"></a>AbsoluteLayout

Eine [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) wird verwendet, um Elemente mit expliziten Werten oder mit Werten relativ zur Größe des Layouts zu positionieren und zu verkleinern. Die Position wird von der linken oberen Ecke des untergeordneten Elements relativ zur linken oberen Ecke des `AbsoluteLayout`angegeben.

Ein [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) sollte als ein spezielles Layout angesehen werden, das nur verwendet werden kann, wenn Sie eine Größe für untergeordnete Elemente festlegen können, oder wenn die Größe des Elements die Positionierung anderer untergeordneter Elemente nicht beeinträchtigt. Standardmäßig wird dieses Layout verwendet, um eine Überlagerung zu erstellen, die die Seite mit anderen Steuerelementen abdeckt, um den Benutzer vor der Interaktion mit den normalen Steuerelementen auf der Seite zu schützen.

> [!IMPORTANT]
> Die `HorizontalOptions` und `VerticalOptions` Eigenschaften wirken sich nicht auf die untergeordneten Elemente ein `AbsoluteLayout`.

Innerhalb von wird die [`AbsoluteLayout.LayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty) angefügte-Eigenschaft verwendet, um die horizontale Position, die vertikale Position, die Breite und die Höhe eines Elements anzugeben. [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) Außerdem gibt die [`AbsoluteLayout.LayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty) angefügte-Eigenschaft an, wie die layoutbegrenzungen interpretiert werden.

Der folgende XAML-Code zeigt, wie Elemente in [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)einem angeordnet werden:

```xaml
<AbsoluteLayout Margin="40">
    <BoxView Color="Red"
             AbsoluteLayout.LayoutFlags="PositionProportional"
             AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100"
             Rotation="30" />
    <BoxView Color="Green"
             AbsoluteLayout.LayoutFlags="PositionProportional"
             AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100"
             Rotation="60" />
    <BoxView Color="Blue"
             AbsoluteLayout.LayoutFlags="PositionProportional"
             AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100" />
</AbsoluteLayout>
```

In diesem Beispiel funktioniert das Layout wie folgt:

- Jeder [`BoxView`](xref:Xamarin.Forms.BoxView) erhält eine explizite Größe von 100 x 100 und wird an derselben Position, horizontal zentriert, angezeigt.
- Der rote [`BoxView`](xref:Xamarin.Forms.BoxView) wird um 30 Grad gedreht, und das `BoxView` grüne wird um 60 Grad gedreht.
- Auf jedem [`BoxView`](xref:Xamarin.Forms.BoxView)wird die [`AbsoluteLayout.LayoutFlags`angefügte](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty) -Eigenschaft auf festgelegt. Dies gibt an ,dassdiePositionproportionalzumverbleibendenBereichist,nachdemBreiteundHöheberücksichtigtwurden.`PositionProportional`

> [!CAUTION]
> Vermeiden Sie die [`AbsoluteLayout.AutoSize`](xref:Xamarin.Forms.AbsoluteLayout.AutoSize) Verwendung der-Eigenschaft, wenn dies möglich ist, da die Layout-Engine weitere Layoutberechnungen durchführt.

Weitere Informationen finden Sie unter [xamarin. Forms AbsoluteLayout](absolute-layout.md).

## <a name="input-transparency"></a>Eingabe Transparenz

Jedes visuelle Element verfügt über [`InputTransparent`](xref:Xamarin.Forms.VisualElement.InputTransparent) eine-Eigenschaft, mit der definiert wird, ob das Element Eingaben empfängt. Der Standardwert ist `false`, um sicherzustellen, dass das Element Eingaben empfängt.

Wenn diese Eigenschaft für eine Layoutklasse festgelegt ist, wird Ihr Wert zu untergeordneten Elementen übertragen. Das Festlegen der [`InputTransparent`](xref:Xamarin.Forms.VisualElement.InputTransparent) - `true` Eigenschaft auf für eine Layoutklasse führt daher dazu, dass alle Elemente innerhalb des Layouts keine Eingaben erhalten.

## <a name="layout-performance"></a>Layoutleistung

Um die bestmögliche layoutleistung zu erzielen, befolgen Sie die Richtlinien unter [Optimieren der layoutleistung](~/xamarin-forms/deploy-test/performance.md#optimize-layout-performance).

Außerdem kann die Seiten Rendering-Leistung mithilfe der layoutkomprimierung verbessert werden, die die angegebenen Layouts aus der visuellen Struktur entfernt. Weitere Informationen finden Sie unter [layoutkomprimierung](layout-compression.md).

## <a name="related-links"></a>Verwandte Links

- [Layout (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [Xamarin. Forms-Layouts (Video)](https://youtu.be/4HlLjTZQzjM)
- [Xamarin. Forms Stacklayout](stack-layout.md)
- [Xamarin. Forms-Raster](grid.md)
- [Xamarin. Forms-flexlayout](flex-layout.md)
- [Xamarin. Forms AbsoluteLayout](absolute-layout.md)
- [Xamarin. Forms relativelayout](relative-layout.md)
- [Optimieren der layoutleistung](~/xamarin-forms/deploy-test/performance.md#optimize-layout-performance)
- [Layoutkomprimierung](layout-compression.md)
