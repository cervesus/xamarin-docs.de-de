---
ms.openlocfilehash: d87289e481b69592b68627d053e937856d3d6067
ms.sourcegitcommit: 3f0e4f10e5def19122588bb05f26ab2baa9df6eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2020
ms.locfileid: "61375387"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Ändern Sie in **MainPage.xaml** die [`Grid`](xref:Xamarin.Forms.Grid)-Deklaration, um Spalten und Zeilen zu bestimmen, und Inhalte in die Spalten und Zeilen zu platzieren:

    ```xaml
    <Grid Margin="20,35,20,20">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.5*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="50" />
        </Grid.RowDefinitions>
        <Label Text="Column 0, Row 0" />
        <Label Grid.Column="1"
               Text="Column 1, Row 0" />
        <Label Grid.Row="1"
               Text="Column 0, Row 1" />
        <Label Grid.Column="1"
               Grid.Row="1"
               Text="Column 1, Row 1" />
    </Grid>
    ```

    Dieser Code gibt Spalten und Zeilen für die [`Grid`](xref:Xamarin.Forms.Grid)-Klasse an, und positioniert [`Label`](xref:Xamarin.Forms.Label)-Instanzen in bestimmten Spalten und Zeilen. Die Spalten- und Zeilendaten werden in den [`ColumnDefinitions`](xref:Xamarin.Forms.Grid.ColumnDefinitions)- und [`RowDefinitions`](xref:Xamarin.Forms.Grid.RowDefinitions)-Eigenschaften gespeichert. Diese sind jeweils Sammlungen von [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition)- und [`RowDefinition`](xref:Xamarin.Forms.RowDefinition)-Objekten. Die Breite jeder `ColumnDefinition`-Eigenschaft wird von der [`ColumnDefinition.Width`](xref:Xamarin.Forms.ColumnDefinition.Width)-Eigenschaft festgelegt; die Höhe jeder `RowDefinition`-Klasse wird von der [`RowDefinition.Height`](xref:Xamarin.Forms.RowDefinition.Height)-Eigenschaft festgelegt. Gültige Breiten- und Höhenwerte sind die folgenden:

    - [`Auto`](xref:Xamarin.Forms.GridUnitType.Auto): Dadurch wird die Größe der Spalte oder Zeile auf den Inhalt abgestimmt.
    - Proportionale Werte: Dadurch werden die Spalten- und Zeilengrößen proportional zum verbleibenden Platz angepasst. Proportionale Werte werden dadurch angezeigt, dass sie auf `*` enden.
    - Absolute Werte: Diese bestimmen die Größe der Spalten und Zeilen mit bestimmten, festgelegten Werten.

    Deshalb hat im obigen Code jede Spalte eine Breite, die der Hälfte der [`Grid`](xref:Xamarin.Forms.Grid)-Klasse entspricht, während jede Zeile eine Höhe von 50 geräteunabhängigen Einheiten hat.

    Die Position jeder [`Label`](xref:Xamarin.Forms.Label)-Klasse in der [`Grid`](xref:Xamarin.Forms.Grid)-Klasse wird durch die angefügten Eigenschaften [`Grid.Column`](xref:Xamarin.Forms.Grid.ColumnProperty) und [`Grid.Row`](xref:Xamarin.Forms.Grid.RowProperty) angegeben. Hierbei wird ein nullbasierter Index verwendet. Deshalb ist die erste Spalte die Spalte 0, und die erste Zeile die Zeile 0. Die erste `Label`-Klasse verfügt nicht über diese angefügten Eigenschaften, da sämtliche Ansichten untergeordneter Elemente, die diese nicht festlegen, automatisch in Spalte 0, Zeile 0 gerendert werden.

    > [!NOTE]
    > Der Abstand zwischen Spalten und Zeilen in einer [`Grid`](xref:Xamarin.Forms.Grid)-Klasse wird mithilfe der Eigenschaften [`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing) und [`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing) festgelegt. Weitere Informationen hierzu finden Sie unter [Spacing (Abstände)](~/xamarin-forms/user-interface/layouts/grid.md#spacing) in [Xamarin.Forms Grid (Xamarin.Forms-Raster)](~/xamarin-forms/user-interface/layouts/grid.md).

1. Klicken Sie in der Symbolleiste von Visual Studio auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Remotesimulator oder Android-Emulator zu starten:

    [![Screenshot: Raster mit Inhalt, der sich über mehrere Spalten und Zeilen erstreckt, unter iOS und Android](../images/columns-rows.png "Raster mit Inhalt in Spalten und Zeilen")](../images/columns-rows-large.png#lightbox "Raster mit Inhalt in Spalten und Zeilen")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Ändern Sie in **MainPage.xaml** die [`Grid`](xref:Xamarin.Forms.Grid)-Deklaration, um Spalten und Zeilen zu bestimmen, und Inhalte in die Spalten und Zeilen zu platzieren:

    ```xaml
    <Grid Margin="20,35,20,20">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.5*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="50" />
        </Grid.RowDefinitions>
        <Label Text="Column 0, Row 0" />
        <Label Grid.Column="1"
               Text="Column 1, Row 0" />
        <Label Grid.Row="1"
               Text="Column 0, Row 1" />
        <Label Grid.Column="1"
               Grid.Row="1"
               Text="Column 1, Row 1" />
    </Grid>
    ```

    Dieser Code gibt Spalten und Zeilen für die [`Grid`](xref:Xamarin.Forms.Grid)-Klasse an, und positioniert [`Label`](xref:Xamarin.Forms.Label)-Instanzen in bestimmten Spalten und Zeilen. Die Spalten- und Zeilendaten werden in den [`ColumnDefinitions`](xref:Xamarin.Forms.Grid.ColumnDefinitions)- und [`RowDefinitions`](xref:Xamarin.Forms.Grid.RowDefinitions)-Eigenschaften gespeichert. Diese sind jeweils Sammlungen von [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition)- und [`RowDefinition`](xref:Xamarin.Forms.RowDefinition)-Objekten. Die Breite jeder `ColumnDefinition`-Eigenschaft wird von der [`ColumnDefinition.Width`](xref:Xamarin.Forms.ColumnDefinition.Width)-Eigenschaft festgelegt; die Höhe jeder `RowDefinition`-Klasse wird von der [`RowDefinition.Height`](xref:Xamarin.Forms.RowDefinition.Height)-Eigenschaft festgelegt. Gültige Breiten- und Höhenwerte sind die folgenden:

    - [`Auto`](xref:Xamarin.Forms.GridUnitType.Auto): Dadurch wird die Größe der Spalte oder Zeile auf den Inhalt abgestimmt.
    - Proportionale Werte: Dadurch werden die Spalten- und Zeilengrößen proportional zum verbleibenden Platz angepasst. Proportionale Werte werden dadurch angezeigt, dass sie auf `*` enden.
    - Absolute Werte: Diese bestimmen die Größe der Spalten und Zeilen mit bestimmten, festgelegten Werten.

    Deshalb hat im obigen Code jede Spalte eine Breite, die der Hälfte der [`Grid`](xref:Xamarin.Forms.Grid)-Klasse entspricht, während jede Zeile eine Höhe von 50 geräteunabhängigen Einheiten hat.

    Die Position jeder [`Label`](xref:Xamarin.Forms.Label)-Klasse in der [`Grid`](xref:Xamarin.Forms.Grid)-Klasse wird durch die angefügten Eigenschaften [`Grid.Column`](xref:Xamarin.Forms.Grid.ColumnProperty) und [`Grid.Row`](xref:Xamarin.Forms.Grid.RowProperty) angegeben. Hierbei wird ein nullbasierter Index verwendet. Deshalb ist die erste Spalte die Spalte 0, und die erste Zeile die Zeile 0. Die erste `Label`-Klasse verfügt nicht über diese angefügten Eigenschaften, da sämtliche Ansichten untergeordneter Elemente, die diese nicht festlegen, automatisch in Spalte 0, Zeile 0 gerendert werden.

    > [!NOTE]
    > Der Abstand zwischen Spalten und Zeilen in einer [`Grid`](xref:Xamarin.Forms.Grid)-Klasse wird mithilfe der Eigenschaften [`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing) und [`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing) festgelegt. Weitere Informationen hierzu finden Sie unter [Spacing (Abstände)](~/xamarin-forms/user-interface/layouts/grid.md#spacing) in [Xamarin.Forms Grid (Xamarin.Forms-Raster)](~/xamarin-forms/user-interface/layouts/grid.md).

1. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator oder Android-Emulator zu starten.

    [![Screenshot: Raster mit Inhalt, der sich über mehrere Spalten und Zeilen erstreckt, unter iOS und Android](../images/columns-rows.png "Raster mit Inhalt in Spalten und Zeilen")](../images/columns-rows-large.png#lightbox "Raster mit Inhalt in Spalten und Zeilen")
